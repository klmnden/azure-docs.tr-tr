---
title: Azure Data Factory'de Azure SSIS tümleştirmesi çalışma zamanı oluşturma | Microsoft Docs
description: Dağıtma ve SSIS paketleri Azure'da çalışması için bir Azure SSIS tümleştirmesi çalışma zamanı Azure Data Factory oluşturmayı öğrenin.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/24/2018
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: f33b24544373bc778f27ef3da18da8d62407a639
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36751646"
---
# <a name="create-the-azure-ssis-integration-runtime-in-azure-data-factory"></a>Azure Data Factory'de Azure SSIS tümleştirmesi çalışma zamanı oluşturma
Bu makalede Azure Data Factory bir Azure SSIS tümleştirmesi çalışma zamanı sağlamak için adımları sağlar. Daha sonra, SQL Server Veri Araçları (SSDT) veya SQL Server Management Studio'yu (SSMS) kullanarak Azure'da bu çalışma zamanında SQL Server Integration Services (SSIS) paketleri dağıtabilir ve çalıştırabilirsiniz. 

Öğretici [Öğreticisi: SQL Server Integration Services (SSIS) Azure'a dağıtabilmeniz](tutorial-create-azure-ssis-runtime-portal.md) SSIS katalog barındırmak için Azure SQL veritabanı kullanarak Azure SSIS tümleştirmesi çalışma zamanı (IR) oluşturulacağını gösterir. Bu makalede öğreticiyi genişletir ve aşağıdaki işlemleri gösterilmektedir: 

- İsteğe bağlı olarak Azure SQL veritabanı SSIS kataloğunuzu (SSISDB veritabanı) barındırmak için veritabanı sunucusu olarak sanal ağ hizmet uç noktaları/yönetilen ile örneği (Önizleme) kullanın. Bir sanal ağa Azure SSIS IR katılmasını ve gerekirse, bkz: sanal ağ izinlerini ve ayarlarını yapılandırmak gerekir bir önkoşul olarak [birleştirme Azure SSIS IR sanal bir ağa](https://docs.microsoft.com/en-us/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network). 

- İsteğe bağlı olarak Azure Active Directory (AAD) kimlik doğrulaması ile Azure veri fabrikası yönetilen hizmet kimliği (MSI) Azure SSIS IR için veritabanı sunucusuna bağlanmak için kullanın. Bir önkoşul olarak ihtiyacınız olacak, veri fabrikası MSI veritabanı sunucusuna erişim izinlerine sahip bir AAD gruba eklemek için bkz: [etkinleştirmek AAD kimlik doğrulaması Azure SSIS IR için](https://docs.microsoft.com/en-us/azure/data-factory/enable-aad-authentication-azure-ssis-ir). 

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Data Factory hizmetinin genel kullanıma açık (GA) 1. sürümünü kullanıyorsanız [Data Factory sürüm 1 belgeleri](v1/data-factory-introduction.md) konusunu inceleyin. 

## <a name="overview"></a>Genel Bakış
Bu makalede Azure SSIS IR sağlama farklı yolu gösterilmektedir: 

- [Azure portal](#azure-portal) 
- [Azure PowerShell](#azure-powershell) 
- [Azure Resource Manager şablonu](#azure-resource-manager-template) 

Bir Azure SSIS IR oluşturduğunuzda, veri fabrikası, Azure SQL SSIS Katalog veritabanı (SSISDB) hazırlamak için veritabanına bağlar. Komut ayrıca belirtilen izinleri ve sanal ağınız için ayarları yapılandırır ve sanal ağa Azure SSIS tümleştirmesi çalışma zamanı yeni bir örneğini birleştirir. 

Bir Azure-SSIS IR örneğini sağladığınızda, SSIS için Azure Feature Pack ve Access Redistributable da yüklenir. Bu bileşenler, Excel ile Access dosyalarıyla ve yerleşik bileşenler tarafından desteklenen veri kaynaklarına ek olarak çeşitli Azure veri kaynaklarıyla bağlantı kurma olanağı sunar. Ek bileşenleri de yükleyebilirsiniz. Daha fazla bilgi için bkz. [Azure-SSIS tümleştirmesi çalışma zamanı için özel kurulum](how-to-configure-azure-ssis-ir-custom-setup.md). 

## <a name="prerequisites"></a>Önkoşullar 
- **Azure aboneliği**. Bir aboneliğiniz yoksa, bir [ücretsiz deneme](http://azure.microsoft.com/pricing/free-trial/) hesabı oluşturabilirsiniz. 

- **Azure SQL veritabanı sunucusu/yönetilen örneği (Önizleme)**. Henüz bir veritabanı sunucunuz yoksa, başlamadan önce Azure portalında bir tane oluşturun. Bu sunucu, SSIS Katalog veritabanını (SSISDB) barındırır. Veritabanı sunucusunu tümleştirme çalışma zamanı ile aynı Azure bölgesinde oluşturmanız önerilir. Bu yapılandırma, tümleştirme çalışma zamanının Azure bölgelerinden geçmeden SSISDB’ye yürütme günlüklerini yazmasına olanak tanır. Seçili veritabanı sunucusunda bağlı olarak, SSISDB sizin adınıza tek bir veritabanını bir esnek havuz, veya yönetilen bir örneğinde (Önizleme) bir parçası olarak oluşturulabilir ve ortak ağ veya sanal ağ katılarak erişilebilir. Azure SQL veritabanı için desteklenen fiyatlandırma katmanlarına listesi için bkz: [SQL veritabanı kaynak sınırları](../sql-database/sql-database-resource-limits.md). 

    Azure SQL veritabanı sunucusu ya da yönetilen örneği (Önizleme) zaten bir SSIS katalog (SSIDB veritabanı) emin olun. Azure-SSIS IR’nin sağlanması, mevcut bir SSIS Kataloğunun kullanılmasını desteklemez. 

- **Klasik veya Azure Resource Manager sanal ağ (isteğe bağlı)**. Aşağıdaki koşulların en az biri doğruysa bir Azure sanal ağı olması gerekir: 
    - Sanal ağ hizmet uç noktaları/yönetilen bir sanal ağ içinde örneği (Önizleme) ile Azure SQL veritabanında SSIS Katalog veritabanı barındırır. 
    - Şirket içi veri depolarına bir Azure-SSIS tümleştirme çalışma zamanı üzerinde çalışan SSIS paketlerinden bağlanmak istiyorsanız. 

- **Azure PowerShell**. ' Ndaki yönergeleri izleyin [Azure PowerShell'i yükleme ve yapılandırma nasıl](/powershell/azure/install-azurerm-ps), SSIS paketleri bulutta çalışan sağlama Azure SSIS tümleştirmesi çalışma zamanı için bir komut dosyasını çalıştırmak için PowerShell kullanın. 

### <a name="region-support"></a>Bölge desteği
Veri fabrikası aşağıdaki bölgelerde oluşturabilirsiniz: Doğu ABD, Doğu ABD 2, Güneydoğu Asya ve Batı Avrupa. 

Azure SSIS IR’yi şu bölgelerde oluşturabilirsiniz: Doğu ABD, Doğu ABD 2, Orta ABD, Batı ABD 2, Kuzey Avrupa, Batı Avrupa, UK Güney ve Avustralya Doğu. 

### <a name="compare-sql-database-and-managed-instance-preview"></a>SQL veritabanı ve yönetilen örneği (Önizleme) ile karşılaştırın

Azure SSIR IR ilgili olarak aşağıdaki tabloda, SQL veritabanını ve örneği (Önizleme) yönetilen belirli özellikleri karşılaştırılır:

| Özellik | SQL Veritabanı | Yönetilen Örnek |
|---------|--------------|------------------|
| **Zamanlama** | SQL Server Agent kullanılabilir değil.<br/><br/>Bkz: [bir Azure Data Factory işlem hattı bir parçası olarak bir paket zamanlama](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-schedule-packages.md#activity).| SQL Server Agent kullanılabilir. |
| **Kimlik doğrulaması** | Herhangi bir Azure Active Directory kullanıcı temsil eden bir kapsanan veritabanı kullanıcı hesabı olan bir veritabanı oluşturabilirsiniz **dbmanager** rol.<br/><br/>Bkz: [etkinleştirmek Azure SQL veritabanı üzerinde Azure AD](enable-aad-authentication-azure-ssis-ir.md#enable-azure-ad-on-azure-sql-database). | Bir Azure AD Yöneticisi dışındaki herhangi bir Azure Active Directory kullanıcı temsil eden bir kapsanan veritabanı kullanıcı hesabı ile bir veritabanı oluşturulamıyor <br/><br/>Bkz: [etkinleştirmek Azure SQL veritabanı yönetilen örneği üzerinde Azure AD](enable-aad-authentication-azure-ssis-ir.md#enable-azure-ad-on-azure-sql-database-managed-instance). |
| **Hizmet katmanı** | SQL veritabanı üzerinde Azure SSIS IR oluşturduğunuzda, hizmet katmanı için SSISDB seçebilirsiniz. Birden çok hizmet katmanlarını vardır. | Yönetilen bir örneğinde Azure SSIS IR oluşturduğunuzda, hizmet katmanı için SSISDB seçemezsiniz. Aynı yönetilen örneğindeki tüm veritabanları için bu örneği ayrılan aynı kaynağı paylaşan. |
| **Sanal ağ** | Azure Resource Manager ve klasik sanal ağlar destekler. | Yalnızca Azure Resource Manager sanal ağ destekler. Sanal ağ gereklidir.<br/><br/>Aynı sanal ağa yönetilen örnek olarak, Azure SSIS IR katılırsanız, Azure SSIS IR yönetilen örneği farklı bir alt ağ olduğundan emin olun. Yönetilen örneği farklı bir sanal ağa Azure SSIS IR katılırsanız (hangi aynı bölgeye sınırlıdır) sanal ağ eşlemesi veya bir sanal ağ sanal ağ bağlantısı öneririz. Bkz: [yönetilen Azure SQL veritabanı örneğine bağlamak](../sql-database/sql-database-managed-instance-connect-app.md). |
| **Dağıtılmış işlemler** | Esnek hareketler desteklenir. Microsoft Dağıtılmış İşlem Düzenleyicisi (MSDTC) işlemleri desteklenmez. Paketlerinizi dağıtılmış işlemleri koordine etmek için MSDTC kullanırsanız, SQL veritabanı için esnek harekete geçirmeyi düşünün. Daha fazla bilgi için bkz: [dağıtılmış işlemler bulut veritabanları arasında](../sql-database/sql-database-elastic-transactions-overview.md). | Desteklenmiyor. |
| | | |

## <a name="azure-portal"></a>Azure portalına
Bu bölümde, bir Azure SSIS IR oluşturmak için Azure portal, veri fabrikası UI özellikle kullanın 

### <a name="create-a-data-factory"></a>Veri fabrikası oluşturma 
1. **Microsoft Edge** veya **Google Chrome** web tarayıcısını açın. Şu anda Data Factory kullanıcı arabirimi yalnızca Microsoft Edge ve Google Chrome web tarayıcılarında desteklenmektedir. 
2. [Azure Portal](https://portal.azure.com/)’da oturum açın. 
3. Soldaki menüde **Yeni**, **Veri + Analiz** ve **Data Factory** öğesine tıklayın. 

   ![Yeni->DataFactory](./media/tutorial-create-azure-ssis-runtime-portal/new-data-factory-menu.png)

4. **Yeni veri fabrikası** sayfasında **ad** için **MyAzureSsisDataFactory** adını girin. 

   ![Yeni veri fabrikası sayfası](./media/tutorial-create-azure-ssis-runtime-portal/new-azure-data-factory.png)

   Azure data factory adı **küresel olarak benzersiz** olmalıdır. Aşağıdaki hatayı alırsanız veri fabrikasının adını değiştirin (örneğin adınızMyAzureSsisDataFactory) ve yeniden oluşturmayı deneyin. Data Factory yapıtlarının adlandırma kuralları için [Data Factory - Adlandırma Kuralları](naming-rules.md) makalesine bakın. 

   `Data factory name “MyAzureSsisDataFactory” is not available`

5. Veri fabrikasını oluşturmak istediğiniz Azure **aboneliğini** seçin. 
6. **Kaynak Grubu** için aşağıdaki adımlardan birini uygulayın: 

   - **Var olanı kullan**’ı seçin ve ardından açılır listeden var olan bir kaynak grubu belirleyin. 
   - **Yeni oluştur**’u seçin ve bir kaynak grubunun adını girin. 

   Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/resource-group-overview.md). 

7. **Sürüm** için **V2 (Önizleme)** öğesini seçin. 
8. Data factory için **konum** seçin. Listede yalnızca veri fabrikası oluşturma için desteklenen konumlar gösterilir. 
9. **Panoya sabitle**’yi seçin. 
10. **Oluştur**’a tıklayın. 
11. Panoda şu kutucuğu ve üzerinde şu durumu görürsünüz: **Veri fabrikası dağıtılıyor**. 

    ![veri fabrikası dağıtılıyor kutucuğu](media/tutorial-create-azure-ssis-runtime-portal/deploying-data-factory.png)

12. Oluşturma işlemi tamamlandıktan sonra, resimde gösterildiği gibi **Data Factory** sayfasını görürsünüz. 

    ![Data factory giriş sayfası](./media/tutorial-create-azure-ssis-runtime-portal/data-factory-home-page.png)

13. Azure Data Factory Kullanıcı Arabirimini (UI) ayrı bir sekmede açmak için **Geliştir ve İzle**’ye tıklayın. 

### <a name="provision-an-azure-ssis-integration-runtime"></a>Azure SSIS tümleştirme çalışma zamanı sağlama 
1. Başlarken sayfasında **SSIS Tümleştirme Çalışma Zamanı Yapılandır** kutucuğuna tıklayın. 

   ![SSIS Tümleştirme Çalışma Zamanı Yapılandır kutucuğu](./media/tutorial-create-azure-ssis-runtime-portal/configure-ssis-integration-runtime-tile.png)

2. **Tümleştirme Çalışma Zamanı Kurulumu**’nun **Genel Ayarlar** sayfasında aşağıdaki adımları tamamlayın: 

   ![Genel ayarlar](./media/tutorial-create-azure-ssis-runtime-portal/general-settings.png)

    a. **Name** (Ad) alanına tümleştirme çalışma zamanınızın adını girin. 

    b. **Description** (Açıklama) alanına tümleştirme çalışma zamanınız için bir açıklama girin. 

    c. **Location** (Konum)alanında tümleştirme çalışma zamanınızın konumunu seçin. Yalnızca desteklenen konumlar görüntülenir. SSISDB'yi barındırmak için veritabanı sunucunuz ile aynı konumu seçmenizi öneririz. 

    d. **Node Size** (Düğüm Boyutu) alanında tümleştirme çalışma zamanınızdaki düğümün boyutunu seçin. Yalnızca desteklenen düğüm boyutları görüntülenir. Yoğun işlemci/bellek kullanan çok sayıda paket çalıştırmak istiyorsanız büyük bir düğüm boyutu seçin. 

    e. **Node Number** (Düğüm Sayısı) alanında tümleştirme çalışma zamanınızdaki düğüm sayısını seçin. Yalnızca desteklenen düğüm sayıları görüntülenir. Aynı anda birden fazla paket çalıştırmak istiyorsanız çok sayıda düğüme sahip bir küme seçin (ölçeklendirme için). 

    f. **Edition/License** (Sürüm/Lisans) alanında tümleştirme çalışma zamanınız için SQL Server sürümünü/lisansını seçin: Standard veya Enterprise. Tümleştirme çalışma zamanınızda gelişmiş/premium özellikleri kullanmak istiyorsanız Enterprise sürümünü seçin. 

    g. **Save Money** (Tasarruf Edin) alanında tümleştirme çalışma zamanınız için Azure Hibrit Avantajı (AHB) seçeneğini ayarlayın: Evet veya Hayır. Karma kullanımla maliyet tasarrufu yapmak üzere Yazılım Güvencesi ile kendi SQL Server lisansınızı getirmek istiyorsanız Evet'i seçin. 

    h. **İleri**’ye tıklayın. 

3. **SQL Ayarları** sayfasında aşağıdaki adımları tamamlayın: 

   ![SQL ayarları](./media/tutorial-create-azure-ssis-runtime-portal/sql-settings.png)

    a. **Subscription** (Abonelik) alanında SSISDB'yi barındıran Azure aboneliğini seçin. 

    b. **Location** (Konum) alanında SSISDB'yi barındıran veritabanı sunucunuzun konumunu seçin. Tümleştirme çalışma zamanınızla aynı konumu seçmenizi öneririz. 

    c. **Catalog Database Server Endpoint** (Katalog Veritabanı Sunucusu Uç Noktası) alanında SSISDB'yi barındıracak veritabanı sunucunuzun uç noktasını seçin. Seçili veritabanı sunucusunda bağlı olarak, SSISDB sizin adınıza tek bir veritabanını bir esnek havuz, veya yönetilen bir örneğinde (Önizleme) bir parçası olarak oluşturulabilir ve ortak ağ veya sanal ağ katılarak erişilebilir. 

    d. Üzerinde **kullanım AAD kimlik doğrulaması...**  onay kutusunu konak SSISDB için veritabanı sunucunuz için kimlik doğrulama yöntemini seçin: SQL veya Azure Active Directory (AAD) ile Azure veri fabrikası yönetilen hizmet kimliği (MSI). Bunu işaretlerseniz, ihtiyacınız, veri fabrikası MSI veritabanı sunucusuna erişim izinlerine sahip bir AAD gruba eklemek için bkz: [etkinleştirmek AAD kimlik doğrulaması Azure SSIS IR için](https://docs.microsoft.com/en-us/azure/data-factory/enable-aad-authentication-azure-ssis-ir). 

    e. **Admin Username** (Yönetici Kullanıcı Adı) alanına SSISDB'yi barındıracak veritabanı sunucunuzun SQL kimlik doğrulaması kullanıcı adını girin. 

    f. **Admin Password** (Yönetici Parolası) alanına SSISDB'yi barındıracak veritabanı sunucunuzun SQL kimlik doğrulaması parolasını girin. 

    g. **Catalog Database Service Tier** (Katalog Veritabanı Hizmet Katmanı) alanında SSISDB'yi barındıracak veritabanı sunucunuzun hizmet katmanını seçin: Temel/Standart/Premium katman veya Elastik Havuz adı. 

    h. **Test Connection** (Bağlantıyı Sına) öğesine tıklayın ve başarılı olursa **Next** (İleri) öğesine tıklayın. 

4.  **Advanced Settings** (Gelişmiş Ayarlar) sayfasında aşağıdaki adımları tamamlayın: 

    ![Gelişmiş ayarlar](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings.png)

    a. **Maximum Parallel Executions Per Node** (Düğüm Başına Maksimum Paralel Yürütme) alanında tümleştirme çalışma zamanınızda her düğümde eş zamanlı olarak yürütülecek maksimum paket sayısını seçin. Yalnızca desteklenen paket sayıları görüntülenir. Birden çok çekirdek işlem/bellek tek bir büyük/ağır paketini çalıştırmak için kullanmak istiyorsanız, düşük bir sayı seçin-yoğun. Tek bir çekirdekte bir veya daha fazla küçük/hafif paket çalıştırmak istiyorsanız daha yüksek bir sayı seçin. 

    b. **Custom Setup Container SAS URI** (Özel Kurulum Kapsayıcısı SAS URI'si) alanında isteğe bağlı olarak kurulum betiğinizin ve ilgili dosyalarının depolandığı Azure Depolama Blobu kapsayıcısının Paylaşılan Erişim İmzası (SAS) Tekdüzen Kaynak Tanımlayıcısı (URI) değerini girebilirsiniz, bkz. [Azure-SSIS IR için özel kurulum](https://docs.microsoft.com/en-us/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup). 

5. Üzerinde **bir sanal ağı seçin...**  onay kutusu, bir sanal ağa tümleştirmesi çalışma zamanı katılmak istediğinizi seçin. SSISDB ana bilgisayar veya şirket içi verilere erişmesi için sanal ağ hizmet uç noktaları/yönetilen ile örneği (Önizleme) Azure SQL veritabanı kullanıyorsanız denetleyin; diğer bir deyişle, şirket içi veri kaynakları/hedefleri SSIS paketlerinizi bkz [katılma Azure SSIS IR sanal bir ağa](https://docs.microsoft.com/en-us/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network). Bunu işaretlerseniz, aşağıdaki adımları tamamlayın: 

   ![Sanal ağ ile gelişmiş ayarları](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings-vnet.png)

    a. İçin **abonelik**, sanal ağınızda varsa Azure aboneliğini seçin. 

    b. İçin **konumu**, aynı konuma Integration zamanının seçilir. 

    c. İçin **türü**, sanal ağınızı türünü seçin: Klasik veya Azure Resource Manager. Öneririz Azure Resource Manager sanal ağı seçin, sonra Klasik sanal ağ en kısa sürede kullanım dışı kalacaktır. 

    d. İçin **VNet adı**, sanal ağınızı adını seçin. Bu sanal ağ SSISDB ve/veya şirket içi ağınıza bağlı bir konak için sanal ağ hizmet uç noktaları/yönetilen ile örneği (Önizleme) Azure SQL veritabanı için kullanılan aynı sanal ağda olmalıdır. 

    e. İçin **alt ağ adı**, alt ağ adı sanal ağınız için seçin. Bu örnek için (Önizleme), yönetilen kullanılan olandan farklı bir alt konak SSISDB olmalıdır. 

6. Tıklatın **VNet doğrulama** tıklatıp başarılı olursa, **son** , Azure SSIS Integration zamanının oluşturmayı başlatmak için. 

    > [!IMPORTANT]
    > - Bu işlem yaklaşık 20-30 dakika sürer
    > - Data Factory hizmeti, SSIS Kataloğu veritabanını (SSISDB) hazırlamak için Azure SQL Veritabanınıza bağlanır. Komut ayrıca belirtilen izinleri ve sanal ağınız için ayarları yapılandırır ve sanal ağa Azure SSIS tümleştirmesi çalışma zamanı yeni bir örneğini birleştirir. 

7. **Bağlantılar** penceresinde, gerekirse **Tümleştirme Çalışma Zamanları**’na geçin. Durumu yenilemek için **Yenile**’ye tıklayın. 

   ![Oluşturma durumu](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-creation-status.png)

8. Bağlantılar altından kullanmak **Eylemler** Durdur/Başlat, düzenlemek veya tümleştirme çalışma zamanı silmek için sütun. Tümleştirme çalışma zamanının JSON kodunu görüntülemek için son bağlantıyı kullanın. Düzenle ve sil düğmeleri yalnızca IR durdurulmuş durumdayken etkin olur. 

   ![Azure SSIS IR - eylemler](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-actions.png)

### <a name="azure-ssis-integration-runtimes-in-the-portal"></a>Portalda Azure SSIS tümleştirmesi çalışma zamanları
1. Veri fabrikanızdaki mevcut tümleştirme çalışma zamanlarını görüntülemek için Azure Data Factory kullanıcı arabiriminde **Düzenle** sekmesine geçin, **Bağlantılar**’a tıklayın ve sonra **Tümleştirme Çalışma Zamanları** sekmesine geçin. 

   ![Varolan IRS görüntüleyin](./media/tutorial-create-azure-ssis-runtime-portal/view-azure-ssis-integration-runtimes.png)

2. Yeni bir Azure-SSIS IR oluşturmak için **Yeni**'ye tıklayın. 

   ![Menü aracılığıyla tümleştirme çalışma zamanı](./media/tutorial-create-azure-ssis-runtime-portal/edit-connections-new-integration-runtime-button.png)

3. Azure-SSIS tümleştirme çalışma zamanı oluşturmak için resimde gösterildiği gibi **Yeni**’ye tıklayın. 
4. Tümleştirme Çalışma Zamanı Kurulumu penceresinde **Mevcut SSIS paketlerini Azure’da yürütmek üzere lift-and-shift ile taşıma**’yı seçip **İleri**’ye tıklayın. 

   ![Tümleştirme çalışma zamanının türünü belirtin](./media/tutorial-create-azure-ssis-runtime-portal/integration-runtime-setup-options.png)

5. Geriye kalan Azure SSIS IR kurulumu adımları için bkz. [Azure SSIS tümleştirme çalışma zamanı sağlama](#provision-an-azure-ssis-integration-runtime). 

## <a name="azure-powershell"></a>Azure PowerShell
Bu bölümde, bir Azure SSIS IR oluşturmak için Azure PowerShell kullanma

### <a name="create-variables"></a>Değişken oluşturma
Bu öğreticideki betiklerde kullanılacak değişkenleri tanımlayın:

```powershell
### Azure Data Factory version 2 information 
# If your input contains a PSH special character, e.g. "$", precede it with the escape character "`" like "`$".
$SubscriptionName = "[your Azure subscription name]"
$ResourceGroupName = "[your Azure resource group name]"
$DataFactoryName = "[your data factory name]"
# You can create a data factory of version 2 in the following regions: East US, East US 2, Southeast Asia, and West Europe. 
$DataFactoryLocation = "EastUS" 

### Azure-SSIS integration runtime information - This is the Data Factory compute resource for running SSIS packages
$AzureSSISName = "[specify a name for your Azure-SSIS IR]"
$AzureSSISDescription = "[specify a description for your Azure-SSIS IR]"
# You can create an Azure-SSIS IR in the following regions: East US, East US 2, Central US, West US 2, North Europe, West Europe, UK South, and Australia East.
$AzureSSISLocation = "EastUS" 
# In public preview, only Standard_A4_v2|Standard_A8_v2|Standard_D1_v2|Standard_D2_v2|Standard_D3_v2|Standard_D4_v2 are supported.
$AzureSSISNodeSize = "Standard_D4_v2"
# In public preview, only 1-10 nodes are supported.
$AzureSSISNodeNumber = 2 
# Azure-SSIS IR edition/license info: Standard or Enterprise 
$AzureSSISEdition = "" # Standard by default, while Enterprise lets you use advanced/premium features on your Azure-SSIS IR
# Azure-SSIS IR hybrid usage info: LicenseIncluded or BasePrice
$AzureSSISLicenseType = "" # LicenseIncluded by default, while BasePrice lets you bring your own on-premises SQL Server license with Software Assurance to earn cost savings from Azure Hybrid Benefit (AHB) option
# For a Standard_D1_v2 node, 1-4 parallel executions per node are supported. For other nodes, it's 1-8.
$AzureSSISMaxParallelExecutionsPerNode = 8 
# Custom setup info
$SetupScriptContainerSasUri = "" # OPTIONAL to provide SAS URI of blob container where your custom setup script and its associated files are stored
# Virtual network info: Classic or Azure Resource Manager
$VnetId = "[your virtual network resource ID or leave it empty]" # REQUIRED if you use Azure SQL Database with virtual network service endpoints/Managed Instance (Preview)/on-premises data, Azure Resource Manager virtual network is recommended, Classic virtual network will be deprecated soon    
$SubnetName = "[your subnet name or leave it empty]" # WARNING: Please use a different subnet than the one used for your Managed Instance (Preview)

### SSISDB info
$SSISDBServerEndpoint = "[your Azure SQL Database server name or Managed Instance (Preview) name.DNS prefix].database.windows.net" # WARNING: Please ensure that there is no existing SSISDB, so we can prepare and manage one on your behalf
# Authentication info: SQL or Azure Active Directory (AAD)
$SSISDBServerAdminUserName = "[your server admin username for SQL authentication or leave it empty for AAD authentication]"
$SSISDBServerAdminPassword = "[your server admin password for SQL authentication or leave it empty for AAD authentication]"
$SSISDBPricingTier = "[Basic|S0|S1|S2|S3|S4|S6|S7|S9|S12|P1|P2|P4|P6|P11|P15|…|ELASTIC_POOL(name = <elastic_pool_name>) for Azure SQL Database or leave it empty for Managed Instance (Preview)]"
```

### <a name="log-in-and-select-subscription"></a>Oturum açma ve abonelik seçme
Oturum açma ve Azure aboneliğinizi seçin komut dosyasını aşağıdaki kodu ekleyin: 

```powershell
Connect-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $SubscriptionName
```

### <a name="validate-the-connection-to-database"></a>Veritabanı bağlantısını doğrulama
Azure SQL veritabanı sunucusu uç noktanızı doğrulamak için aşağıdaki betiği ekleyin. 

```powershell
# Validate only when you do not use VNet nor AAD authentication
if([string]::IsNullOrEmpty($VnetId) -and [string]::IsNullOrEmpty($SubnetName))
{
    if(![string]::IsNullOrEmpty($SSISDBServerAdminUserName) -and ![string]::IsNullOrEmpty($SSISDBServerAdminPassword))
    {
        $SSISDBConnectionString = "Data Source=" + $SSISDBServerEndpoint + ";User ID=" + $SSISDBServerAdminUserName + ";Password=" + $SSISDBServerAdminPassword    
        $sqlConnection = New-Object System.Data.SqlClient.SqlConnection $SSISDBConnectionString;
        Try
        {
            $sqlConnection.Open();
        }
        Catch [System.Data.SqlClient.SqlException]
        {
            Write-Warning "Cannot connect to your Azure SQL Database server, exception: $_";
            Write-Warning "Please make sure the server you specified has already been created. Do you want to proceed? [Y/N]"
            $yn = Read-Host
            if(!($yn -ieq "Y"))
            {
                Return;
            } 
        }
    }
}
```

### <a name="configure-virtual-network"></a>Sanal ağ yapılandırma
Azure-SSIS tümleştirme çalışma zamanının katılacağı sanal ağın izinlerini/ayarlarını otomatik olarak yapılandırmak için aşağıdaki betiği ekleyin.

```powershell
# Make sure to run this script against the subscription to which the virtual network belongs.
if(![string]::IsNullOrEmpty($VnetId) -and ![string]::IsNullOrEmpty($SubnetName))
{
    # Register to the Azure Batch resource provider
    $BatchApplicationId = "ddbf3205-c6bd-46ae-8127-60eb93363864"
    $BatchObjectId = (Get-AzureRmADServicePrincipal -ServicePrincipalName $BatchApplicationId).Id
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
    while(!(Get-AzureRmResourceProvider -ProviderNamespace "Microsoft.Batch").RegistrationState.Contains("Registered"))
    {
    Start-Sleep -s 10
    }
    if($VnetId -match "/providers/Microsoft.ClassicNetwork/")
    {
        # Assign the VM contributor role to Microsoft.Batch
        New-AzureRmRoleAssignment -ObjectId $BatchObjectId -RoleDefinitionName "Classic Virtual Machine Contributor" -Scope $VnetId
    }
}
```

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
[New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) komutunu kullanarak yeni bir [Azure kaynak grubu](../azure-resource-manager/resource-group-overview.md) oluşturun. Kaynak grubu, Azure kaynaklarının grup olarak dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. 

```powershell
New-AzureRmResourceGroup -Location $DataFactoryLocation -Name $ResourceGroupName
```

### <a name="create-a-data-factory"></a>Veri fabrikası oluşturma
Veri fabrikası oluşturmak için aşağıdaki komutu çalıştırın.

```powershell
Set-AzureRmDataFactoryV2 -ResourceGroupName $ResourceGroupName `
                         -Location $DataFactoryLocation `
                         -Name $DataFactoryName
```

### <a name="create-an-integration-runtime"></a>Tümleştirme çalışma zamanı oluşturma
SSIS paketleri Azure'da çalışan bir Azure SSIS tümleştirmesi çalışma zamanı oluşturmak için aşağıdaki komutları çalıştırın. 

Azure SQL veritabanı sanal ağ hizmet uç noktaları/yönetilen ile örneği (Önizleme) ya da erişim gerektiren SSISDB barındırmak için şirket içi veri değil kullanırsanız, VNetId ve alt ağ parametrelerini atlayın veya boş değerleri kendileri için geçirin. Aksi takdirde olamaz bunları atlayın ve gerekir, sanal ağ yapılandırmasından geçerli değerleri geçirmek, bkz [katılma Azure SSIS IR sanal bir ağa](https://docs.microsoft.com/en-us/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network). 

SSISDB konağa yönetilen örneği (Önizleme) kullanırsanız, CatalogPricingTier parametreyi veya onun için boş bir değer geçirin. Aksi takdirde olamaz, atlayın ve gerekir fiyatlandırma katmanlarına Azure SQL veritabanı için desteklenen geçerli bir değer listesinden geçirmek, bkz [SQL veritabanı kaynak sınırları](../sql-database/sql-database-resource-limits.md). 

Veritabanı sunucusuna bağlanmak için Azure veri fabrikası yönetilen hizmet kimliği (MSI ile) Azure Active Directory (AAD) kimlik doğrulaması kullanıyorsanız, CatalogAdminCredential parametreyi atlayabilirsiniz, ancak AAD grubuna erişim, veri fabrikası MSI eklemeniz gerekir Veritabanı sunucusu izinleri bkz [etkinleştirmek AAD kimlik doğrulaması Azure SSIS IR için](https://docs.microsoft.com/en-us/azure/data-factory/enable-aad-authentication-azure-ssis-ir). Aksi takdirde, atlayamazsınız ve server yönetici kullanıcı adı ve parola SQL kimlik doğrulaması için oluşturulmuş geçerli bir nesne geçmesi gerekir.

```powershell               
Set-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                           -DataFactoryName $DataFactoryName `
                                           -Type Managed `
                                           -Name $AzureSSISName `
                                           -Description $AzureSSISDescription `
                                           -Location $AzureSSISLocation `
                                           -NodeSize $AzureSSISNodeSize `
                                           -NodeCount $AzureSSISNodeNumber `
                                           -Edition $AzureSSISEdition `
                                           -LicenseType $AzureSSISLicenseType `
                                           -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                           -SetupScriptContainerSasUri $SetupScriptContainerSasUri `
                                           -VnetId $VnetId `
                                           -Subnet $SubnetName `
                                           -CatalogServerEndpoint $SSISDBServerEndpoint `
                                           -CatalogPricingTier $SSISDBPricingTier

if(![string]::IsNullOrEmpty($SSISDBServerAdminUserName) –and ![string]::IsNullOrEmpty($SSISDBServerAdminPassword))
{
    $secpasswd = ConvertTo-SecureString $SSISDBServerAdminPassword -AsPlainText -Force
    $serverCreds = New-Object System.Management.Automation.PSCredential($SSISDBServerAdminUserName, $secpasswd)

    Set-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -CatalogAdminCredential $serverCreds
}
```

### <a name="start-integration-runtime"></a>Tümleştirme çalışma zamanını başlatma
Azure-SSIS tümleştirme çalışma zamanını başlatmak için aşağıdaki komutu çalıştırın: 

```powershell
write-host("##### Starting #####")
Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

write-host("##### Completed #####")
write-host("If any cmdlet is unsuccessful, please consider using -Debug option for diagnostics.")                                  
```

Bu komutun tamamlanması **20-30 dakika** sürer. 

### <a name="full-script"></a>Tam betik
Burada, Azure SSIS tümleştirmesi çalışma zamanı oluşturur tam komut verilmiştir. 

```powershell
### Azure Data Factory version 2 information 
# If your input contains a PSH special character, e.g. "$", precede it with the escape character "`" like "`$".
$SubscriptionName = "[your Azure subscription name]"
$ResourceGroupName = "[your Azure resource group name]"
$DataFactoryName = "[your data factory name]"
# You can create a data factory of version 2 in the following regions: East US, East US 2, Southeast Asia, and West Europe. 
$DataFactoryLocation = "EastUS" 

### Azure-SSIS integration runtime information - This is the Data Factory compute resource for running SSIS packages
$AzureSSISName = "[specify a name for your Azure-SSIS IR]"
$AzureSSISDescription = "[specify a description for your Azure-SSIS IR]"
# You can create an Azure-SSIS IR in the following regions: East US, East US 2, Central US, West US 2, North Europe, West Europe, UK South, and Australia East.
$AzureSSISLocation = "EastUS" 
# In public preview, only Standard_A4_v2|Standard_A8_v2|Standard_D1_v2|Standard_D2_v2|Standard_D3_v2|Standard_D4_v2 are supported.
$AzureSSISNodeSize = "Standard_D4_v2"
# In public preview, only 1-10 nodes are supported.
$AzureSSISNodeNumber = 2 
# Azure-SSIS IR edition/license info: Standard or Enterprise 
$AzureSSISEdition = "" # Standard by default, while Enterprise lets you use advanced/premium features on your Azure-SSIS IR
# Azure-SSIS IR hybrid usage info: LicenseIncluded or BasePrice
$AzureSSISLicenseType = "" # LicenseIncluded by default, while BasePrice lets you bring your own on-premises SQL Server license with Software Assurance to earn cost savings from Azure Hybrid Benefit (AHB) option
# For a Standard_D1_v2 node, 1-4 parallel executions per node are supported. For other nodes, it's 1-8.
$AzureSSISMaxParallelExecutionsPerNode = 8 
# Custom setup info
$SetupScriptContainerSasUri = "" # OPTIONAL to provide SAS URI of blob container where your custom setup script and its associated files are stored
# Virtual network info: Classic or Azure Resource Manager
$VnetId = "[your virtual network resource ID or leave it empty]" # REQUIRED if you use Azure SQL Database with virtual network service endpoints/Managed Instance (Preview)/on-premises data, Azure Resource Manager virtual network is recommended, Classic virtual network will be deprecated soon    
$SubnetName = "[your subnet name or leave it empty]" # WARNING: Please use a different subnet than the one used for your Managed Instance (Preview)

### SSISDB info
$SSISDBServerEndpoint = "[your Azure SQL Database server name or Managed Instance (Preview) name.DNS prefix].database.windows.net" # WARNING: Please ensure that there is no existing SSISDB, so we can prepare and manage one on your behalf
# Authentication info: SQL or Azure Active Directory (AAD)
$SSISDBServerAdminUserName = "[your server admin username for SQL authentication or leave it empty for AAD authentication]"
$SSISDBServerAdminPassword = "[your server admin password for SQL authentication or leave it empty for AAD authentication]"
$SSISDBPricingTier = "[Basic|S0|S1|S2|S3|S4|S6|S7|S9|S12|P1|P2|P4|P6|P11|P15|…|ELASTIC_POOL(name = <elastic_pool_name>) for Azure SQL Database or leave it empty for Managed Instance (Preview)]"

### Log in and select subscription
Connect-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

### Validate the connection to database
# Validate only when you do not use VNet nor AAD authentication
if([string]::IsNullOrEmpty($VnetId) -and [string]::IsNullOrEmpty($SubnetName))
{
    if(![string]::IsNullOrEmpty($SSISDBServerAdminUserName) -and ![string]::IsNullOrEmpty($SSISDBServerAdminPassword))
    {
        $SSISDBConnectionString = "Data Source=" + $SSISDBServerEndpoint + ";User ID=" + $SSISDBServerAdminUserName + ";Password=" + $SSISDBServerAdminPassword    
        $sqlConnection = New-Object System.Data.SqlClient.SqlConnection $SSISDBConnectionString;
        Try
        {
            $sqlConnection.Open();
        }
        Catch [System.Data.SqlClient.SqlException]
        {
            Write-Warning "Cannot connect to your Azure SQL Database server, exception: $_";
            Write-Warning "Please make sure the server you specified has already been created. Do you want to proceed? [Y/N]"
            $yn = Read-Host
            if(!($yn -ieq "Y"))
            {
                Return;
            } 
        }
    }
}

### Configure virtual network
# Make sure to run this script against the subscription to which the virtual network belongs.
if(![string]::IsNullOrEmpty($VnetId) -and ![string]::IsNullOrEmpty($SubnetName))
{
    # Register to the Azure Batch resource provider
    $BatchApplicationId = "ddbf3205-c6bd-46ae-8127-60eb93363864"
    $BatchObjectId = (Get-AzureRmADServicePrincipal -ServicePrincipalName $BatchApplicationId).Id
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
    while(!(Get-AzureRmResourceProvider -ProviderNamespace "Microsoft.Batch").RegistrationState.Contains("Registered"))
    {
    Start-Sleep -s 10
    }
    if($VnetId -match "/providers/Microsoft.ClassicNetwork/")
    {
        # Assign the VM contributor role to Microsoft.Batch
        New-AzureRmRoleAssignment -ObjectId $BatchObjectId -RoleDefinitionName "Classic Virtual Machine Contributor" -Scope $VnetId
    }
}

### Create a data factory
Set-AzureRmDataFactoryV2 -ResourceGroupName $ResourceGroupName `
                         -Location $DataFactoryLocation `
                         -Name $DataFactoryName

### Create an integration runtime
Set-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                           -DataFactoryName $DataFactoryName `
                                           -Type Managed `
                                           -Name $AzureSSISName `
                                           -Description $AzureSSISDescription `
                                           -Location $AzureSSISLocation `
                                           -NodeSize $AzureSSISNodeSize `
                                           -NodeCount $AzureSSISNodeNumber `
                                           -Edition $AzureSSISEdition `
                                           -LicenseType $AzureSSISLicenseType `
                                           -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                           -SetupScriptContainerSasUri $SetupScriptContainerSasUri `
                                           -VnetId $VnetId `
                                           -Subnet $SubnetName `
                                           -CatalogServerEndpoint $SSISDBServerEndpoint `
                                           -CatalogPricingTier $SSISDBPricingTier

if(![string]::IsNullOrEmpty($SSISDBServerAdminUserName) –and ![string]::IsNullOrEmpty($SSISDBServerAdminPassword))
{
    $secpasswd = ConvertTo-SecureString $SSISDBServerAdminPassword -AsPlainText -Force
    $serverCreds = New-Object System.Management.Automation.PSCredential($SSISDBServerAdminUserName, $secpasswd)

    Set-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -CatalogAdminCredential $serverCreds
}

### Start integration runtime   
write-host("##### Starting your Azure-SSIS integration runtime. This command takes 20 to 30 minutes to complete. #####")
Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

write-host("##### Completed #####")
write-host("If any cmdlet is unsuccessful, please consider using -Debug option for diagnostics.")
```

## <a name="azure-resource-manager-template"></a>Azure Resource Manager şablonu
Bu bölümde, Azure SSIS tümleştirmesi çalışma zamanı oluşturmak için Azure Resource Manager şablonunu kullanın. Bir örnek kılavuz şöyledir: 

1. Aşağıdaki Azure Resource Manager şablonu ile bir JSON dosyası oluşturun. Açılı ayraç (yer tutucu) değerleri kendi değerlerinizle değiştirin. 

    ```json
    {
        "contentVersion": "1.0.0.0",
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "parameters": {},
        "variables": {},
        "resources": [{
            "name": "<Specify a name for your data factory>",
            "apiVersion": "2017-09-01-preview",
            "type": "Microsoft.DataFactory/factories",
            "location": "East US",
            "properties": {},
            "resources": [{
                "type": "integrationruntimes",
                "name": "<Specify a name for your Azure-SSIS IR>",
                "dependsOn": [ "<The name of the data factory you specified at the beginning>" ],
                "apiVersion": "2017-09-01-preview",
                "properties": {
                    "type": "Managed",
                    "typeProperties": {
                        "computeProperties": {
                            "location": "East US",
                            "nodeSize": "Standard_D4_v2",
                            "numberOfNodes": 1,
                            "maxParallelExecutionsPerNode": 8
                        },
                        "ssisProperties": {
                            "catalogInfo": {
                                "catalogServerEndpoint": "<Azure SQL Database server name>.database.windows.net",
                                "catalogAdminUserName": "<Azure SQL Database server admin username>",
                                "catalogAdminPassword": {
                                    "type": "SecureString",
                                    "value": "<Azure SQL Database server admin password>"
                                },
                                "catalogPricingTier": "Basic"
                            }
                        }
                    }
                }
            }]
        }]
    }
    ```
    
2. Azure Resource Manager şablonu dağıtmak için burada ADFTutorialResourceGroup kaynak grubunuzun adını ve C:\adfgetstarted JSON içeren dosyasını aşağıdaki örnekte gösterildiği gibi New-AzureRmResourceGroupDeployment komutunu çalıştırın. Veri Fabrikası ve Azure SSIS IR tanımı 

    ```powershell
    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json
    ```

    Bu komut, veri fabrikası ve Azure SSIS IR içinde oluşturur, ancak IR başlamıyor 

3. Azure SSIS IR başlatmak için Başlat AzureRmDataFactoryV2IntegrationRuntime komutu çalıştırın: 

    ```powershell
    Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName "<Resource Group Name>" `
                                                 -DataFactoryName "<Data Factory Name>" `
                                                 -Name "<Azure SSIS IR Name>" `
                                                 -Force
    ``` 

## <a name="deploy-ssis-packages"></a>SSIS paketlerini dağıtma
Şimdi SQL Server Veri Araçları (SSDT) veya SQL Server Management Studio’yu (SSMS) kullanarak SSIS paketlerinizi Azure’a dağıtın. SSIS katalog (SSISDB) barındıran, veritabanı sunucusuna bağlanın. Veritabanı sunucusu adı şu biçimdedir: &lt;Azure SQL veritabanı sunucusu adı&gt;. database.windows.net veya &lt;yönetilen örneği (Önizleme) adı. DNS öneki&gt;. database.windows.net. Yönergeler için [Paket dağıtma](/sql/integration-services/packages/deploy-integration-services-ssis-projects-and-packages#deploy-packages-to-integration-services-server) makalesine bakın. 

## <a name="next-steps"></a>Sonraki adımlar
Bu belge diğer Azure SSIS IR konulara bakın: 

- [Azure SSIS tümleştirmesi çalışma zamanı](concepts-integration-runtime.md#azure-ssis-integration-runtime). Bu makalede Azure SSIS IR genel dahil tümleştirme çalışma zamanları hakkında kavramsal bilgiler sağlar 
- [Öğretici: SSIS paketlerini Azure’a dağıtma](tutorial-create-azure-ssis-runtime-portal.md). Bu makale bir Azure-SSIS IR oluşturmaya ilişkin adım adım yönergeler sağlar ve SSIS kataloğunu barındırmak için bir Azure SQL veritabanı kullanır. 
- [Azure-SSIS IR’yi izleme](monitor-integration-runtime.md#azure-ssis-integration-runtime). Bu makalede bir Azure-SSIS IR ile ilgili bilgileri ve döndürülen bilgilerdeki durumların açıklamalarını alma işlemi gösterilmektedir. 
- [Azure-SSIS IR’yi yönetme](manage-azure-ssis-integration-runtime.md). Bu makale bir Azure-SSIS IR’yi durdurma, başlatma veya kaldırma işlemini gösterir. Ayrıca, IR’ye daha fazla düğüm ekleyerek Azure-SSIS IR’nizi ölçeklendirmeyi gösterir. 
- [Azure-SSIS IR’yi bir sanal ağa ekleyin](join-azure-ssis-integration-runtime-virtual-network.md). Bu makalede, Azure SSIS IR Azure sanal ağını birleştirmek hakkında kavramsal bilgiler verilmektedir. Ayrıca, Azure portalını kullanarak Azure-SSIS IR’nin sanal ağa katılmasını sağlayacak şekilde sanal ağı yapılandırma adımlarını da sunar. 
