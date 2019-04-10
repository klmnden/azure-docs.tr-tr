---
title: Azure-SSIS Integration Runtime, Azure veri fabrikasında | Microsoft Docs
description: Dağıtabilir ve SSIS paketlerini Azure'da çalıştırmak için Azure Data Factory'de bir Azure-SSIS tümleştirme çalışma zamanı oluşturmayı öğrenin.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 02/20/2019
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: d30ec0765627ec173f0027e49f44cb77f6b26ac6
ms.sourcegitcommit: 43b85f28abcacf30c59ae64725eecaa3b7eb561a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59361477"
---
# <a name="create-azure-ssis-integration-runtime-in-azure-data-factory"></a>Azure Data Factory Azure SSIS tümleştirme çalışma zamanı oluşturma

Bu makale, sağlama Azure-SSIS Integration Runtime (IR) Azure Data Factory (ADF) için adımları sağlar. Ardından, dağıtmak ve bu tümleştirme çalışma zamanı azure'da üzerinde SQL Server Integration Services (SSIS) paketlerini çalıştırmak için SQL Server veri Araçları (SSDT) veya SQL Server Management Studio (SSMS) kullanabilirsiniz.

[Öğreticisi: SSIS paketlerini Azure'a dağıtma](tutorial-create-azure-ssis-runtime-portal.md) konak SSIS Kataloğu veritabanını (SSISDB) için Azure SQL veritabanı sunucusu kullanarak Azure-SSIS IR oluşturma işlemi gösterilmektedir. Bu makale öğreticiyi genişletip ve aşağıdaki işlemleri gösterilmektedir:

- İsteğe bağlı olarak sanal ağ hizmet uç noktaları/yönetilen örnek SSISDB'yi barındırmak için Azure SQL veritabanı sunucusu kullanın. SSISDB'yi barındırmak için veritabanı sunucu türünü seçmek kılavuzluk etmesi için bkz [karşılaştırma Azure SQL veritabanı tek veritabanlarını elastik havuzları ve yönetilen örneği](create-azure-ssis-integration-runtime.md#compare-sql-database-single-databaseelastic-pool-and-sql-database-managed-instance). Bir önkoşul olarak, Azure-SSIS IR'yi bir sanal ağa katılın ve sanal ağın izinlerini/ayarlarını gereken şekilde yapılandırmanız gerekir. Bkz: [katılın Azure-SSIS IR'yi bir sanal ağa](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network).

- İsteğe bağlı olarak Azure Active Directory (AAD) kimlik doğrulaması ile ADF için yönetilen kimlik veritabanı sunucusuna bağlanmak için kullanın. Bir önkoşul olarak ihtiyacınız olacak, Azure SQL veritabanı sunucusu/yönetilen içinde örneği SSISDB istemcilerinizle bağımsız veritabanı kullanıcısı olarak, ADF için yönetilen kimlik eklemek için bkz: [etkinleştirme AAD kimlik doğrulaması için Azure-SSIS IR](https://docs.microsoft.com/azure/data-factory/enable-aad-authentication-azure-ssis-ir).

## <a name="overview"></a>Genel Bakış

Bu makalede, Azure-SSIS IR sağlama farklı yöntemler gösterilmektedir:

- [Azure portalı](#azure-portal)
- [Azure PowerShell](#azure-powershell)
- [Azure Resource Manager şablonu](#azure-resource-manager-template)

Azure-SSIS IR oluşturduğunuzda, Azure SQL veritabanı sunucusu/yönetilen örnek SSISDB hazırlamak için ADF hizmeti bağlanır. Ayrıca belirtilmişse sanal ağınıza izinlerini/ayarlarını yapılandırır ve Azure-SSIS IR sanal ağa katılır.

Azure-SSIS IR sağladığınızda, Azure Feature Pack SSIS ve Access Redistributable da yüklenir. Bu bileşenler, Excel/erişim dosyaları bağlantısı ve yerleşik bileşenler tarafından desteklenen veri kaynaklarının yanı sıra çeşitli Azure veri kaynakları sağlar. Ek bileşenleri de yükleyebilirsiniz. Daha fazla bilgi için bkz. [Azure-SSIS tümleştirmesi çalışma zamanı için özel kurulum](how-to-configure-azure-ssis-ir-custom-setup.md).

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

- **Azure aboneliği**. Zaten bir aboneliğiniz yoksa, oluşturabileceğiniz bir [ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial/) hesabı.

- **Azure SQL veritabanı sunucusu veya yönetilen örneği**. Bir veritabanı sunucusu zaten yoksa, başlamadan önce Azure Portalı'nda oluşturabilirsiniz. Bu sunucu SSISDB barındıracak. Veritabanı sunucusu, tümleştirme çalışma zamanı ile aynı Azure bölgesinde oluşturmanız önerilir. Bu yapılandırma, tümleştirme çalışma zamanının Azure bölgelerinden geçmeden ssısdb'ye yürütme günlüklerini SSISDB için yazma sağlar. Seçili veritabanı sunucusunda bağlı olarak, SSISDB sizin adınıza bir tek veritabanı olarak, bir elastik havuzun veya yönetilen Örneğinize ve erişilebilir bir parçası, genel ağında veya bir sanal ağa bağlanmaya tarafından oluşturulabilir. Azure SQL veritabanı için desteklenen fiyatlandırma katmanlarının bir listesi için bkz. [SQL veritabanı kaynak limitleri](../sql-database/sql-database-resource-limits.md).

    Azure SQL veritabanı sunucusu/yönetilen örnek zaten bir SSISDB olmadığından emin olun. Azure-SSIS IR'nin sağlanması var olan bir SSISDB kullanma desteği olmamasıdır.

- **(İsteğe bağlı) Azure Resource Manager sanal ağı**. Aşağıdaki koşullardan biri doğru ise bir Azure Resource Manager sanal ağı olması gerekir:

  - Sanal ağ hizmet uç noktaları ile Azure SQL veritabanı sunucusu veya yönetilen bir sanal ağ içinde olan örneği SSISDB barındırıyorsanız.
  - Şirket içi veri depoları, Azure-SSIS IR'yi üzerinde çalışan SSIS paketlerinden bağlanmak istediğiniz

- **Azure PowerShell**. Yönergeleri takip edin [Azure PowerShell'i yükleme ve yapılandırma konusunda](/powershell/azure/install-az-ps), Azure-SSIS IR sağlamak için bir PowerShell Betiği çalıştırmak istiyorsanız

### <a name="region-support"></a>Bölge desteği

Azure bölgesi ile ADF ve Azure-SSIS IR'yi şu anda kullanılabilir bir listesi için bkz. [ADF + SSIS IR bölgelere göre kullanılabilirliği](https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all).

### <a name="compare-sql-database-single-databaseelastic-pool-and-sql-database-managed-instance"></a>SQL veritabanı tek veritabanı esnek havuzunu ve SQL veritabanı yönetilen örneği karşılaştırın

Bunlar SSIR Azure IR için ilgili olarak aşağıdaki tabloda Azure SQL veritabanı yönetilen örneği ve belirli özellikleri karşılaştırılır:

| Özellik | tek veritabanı elastik havuz| Yönetilen Örnek |
|---------|--------------|------------------|
| **Zamanlanıyor** | SQL Server Aracısı kullanılamıyor.<br/><br/>Bkz: [ADF işlem hattındaki bir paket yürütmeyi zamanlama](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-schedule-packages?view=sql-server-2017#activity).| Yönetilen örnek aracı kullanılabilir. |
| **Authentication** | SSISDB ile ADF yönetilen kimliğe sahip tüm AAD grubu üyesi olarak temsil eden bir bağımsız veritabanı kullanıcısı oluşturabilirsiniz **db_owner** rol.<br/><br/>Bkz: [SSISDB Azure SQL veritabanı sunucusu oluşturmak için kimlik doğrulamasını etkinleştirme Azure AD'ye](enable-aad-authentication-azure-ssis-ir.md#enable-azure-ad-on-azure-sql-database). | SSISDB ile ADF yönetilen kimliğini temsil eden bağımsız veritabanı kullanıcısı oluşturabilirsiniz. <br/><br/>Bkz: [Azure SQL veritabanı yönetilen örneği'nde SSISDB oluşturmak için kimlik doğrulamasını etkinleştirme Azure AD'ye](enable-aad-authentication-azure-ssis-ir.md#enable-azure-ad-on-azure-sql-database-managed-instance). |
| **Hizmet katmanı** | Azure-SSIS IR, Azure SQL veritabanı sunucunuzu oluşturduğunuzda, hizmet katmanı için SSISDB seçebilirsiniz. Birden çok hizmet katmanı vardır. | Azure-SSIS IR ile yönetilen Örneğinize oluşturduğunuzda, hizmet katmanı için SSISDB seçemezsiniz. Yönetilen Örneğinize tüm veritabanlarını bu örneğe ayrılan aynı kaynak paylaşın. |
| **Sanal ağ** | Yalnızca Azure Resource Manager sanal ağları, sanal ağ hizmet uç noktaları ile Azure SQL veritabanı sunucusunu kullanabilir veya şirket içi veri depolarına erişmesi katılmak Azure-SSIS IR için destekler. | Yalnızca Azure Resource Manager sanal ağları Azure-SSIS IR katılmak için destekler. Sanal ağ, her zaman gerekli değildir.<br/><br/>Yönetilen Örneğinize aynı sanal ağ için Azure-SSIS IR katılırsanız, Azure-SSIS IR yönetilen Örneğinize değerinden farklı bir alt ağda olduğundan emin olun. Azure-SSIS IR yönetilen Örneğinize farklı bir sanal ağa katılın, bir sanal ağ eşlemesi veya sanal ağ için sanal ağ bağlantısı öneririz. Bkz: [uygulamanızı Azure SQL veritabanı yönetilen örneği bağlamak](../sql-database/sql-database-managed-instance-connect-app.md). |
| **Dağıtılmış işlemler** | Elastik işlemler desteklenir. Microsoft Dağıtılmış İşlem Düzenleyicisi (MSDTC) işlemler desteklenmez. SSIS paketlerinizi dağıtılmış işlemleri düzenlemesine olanak MSDTC kullanıyorsanız, Azure SQL veritabanı için elastik işlemler için geçiş göz önünde bulundurun. Daha fazla bilgi için bkz. [bulut veritabanlarında dağıtılmış işlemler](../sql-database/sql-database-elastic-transactions-overview.md). | Desteklenmiyor. |
| | | |

## <a name="azure-portal"></a>Azure portalı

Bu bölümde, Azure portalı, özellikle ADF kullanıcı arabirimi (UI) kullanmanızı / Azure-SSIS IR oluşturmak için uygulama

### <a name="create-a-data-factory"></a>Data factory oluştur

1. **Microsoft Edge** veya **Google Chrome** web tarayıcısını açın. Şu anda Data Factory kullanıcı arabirimi yalnızca Microsoft Edge ve Google Chrome web tarayıcılarında desteklenmektedir.
1. [Azure Portal](https://portal.azure.com/) oturum açın.
1. Soldaki menüde **Yeni**, **Veri + Analiz** ve **Data Factory** öğesine tıklayın.

   ![Yeni->DataFactory](./media/tutorial-create-azure-ssis-runtime-portal/new-data-factory-menu.png)

1. **Yeni veri fabrikası** sayfasında **ad** için **MyAzureSsisDataFactory** adını girin.

   ![Yeni veri fabrikası sayfası](./media/tutorial-create-azure-ssis-runtime-portal/new-azure-data-factory.png)

   Azure data factory adı **küresel olarak benzersiz** olmalıdır. Aşağıdaki hatayı alırsanız veri fabrikasının adını değiştirin (örneğin adınızMyAzureSsisDataFactory) ve yeniden oluşturmayı deneyin. Data Factory yapıtlarının adlandırma kuralları için [Data Factory - Adlandırma Kuralları](naming-rules.md) makalesine bakın.

   `Data factory name “MyAzureSsisDataFactory” is not available`

1. Veri fabrikasını oluşturmak istediğiniz Azure **aboneliğini** seçin.
1. **Kaynak Grubu** için aşağıdaki adımlardan birini uygulayın:

   - **Var olanı kullan**’ı seçin ve ardından açılır listeden var olan bir kaynak grubu belirleyin.
   - **Yeni oluştur**’u seçin ve bir kaynak grubunun adını girin.

   Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/resource-group-overview.md).

1. **Sürüm** için **V2**'yi seçin.
1. Data factory için **konum** seçin. Listede yalnızca veri fabrikası oluşturma için desteklenen konumlar gösterilir.
1. **Panoya sabitle**’yi seçin.
1. **Oluştur**’a tıklayın.
1. Panoda durumuna sahip aşağıdaki kutucuğu görürsünüz: **Veri Fabrikası dağıtılıyor**.

    ![veri fabrikası dağıtılıyor kutucuğu](media/tutorial-create-azure-ssis-runtime-portal/deploying-data-factory.png)

1. Oluşturma işlemi tamamlandıktan sonra, resimde gösterildiği gibi **Data Factory** sayfasını görürsünüz.

    ![Data factory giriş sayfası](./media/tutorial-create-azure-ssis-runtime-portal/data-factory-home-page.png)

1. Azure Data Factory Kullanıcı Arabirimini (UI) ayrı bir sekmede açmak için **Geliştir ve İzle**’ye tıklayın.

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

    f. İçin **sürümü/lisansı**, SQL Server sürümü/lisansı için Integration runtime'ı seçin: Standart veya Kurumsal. Tümleştirme çalışma zamanınızda gelişmiş/premium özellikleri kullanmak istiyorsanız Enterprise sürümünü seçin.

    g. İçin **paradan tasarruf**, Integration runtime için Azure hibrit Avantajı'nı (AHB) seçeneğini belirleyin: Evet veya Hayır. Karma kullanımla maliyet tasarrufu yapmak üzere Yazılım Güvencesi ile kendi SQL Server lisansınızı getirmek istiyorsanız Evet'i seçin.

    h. **İleri**’ye tıklayın.

3. **SQL Ayarları** sayfasında aşağıdaki adımları tamamlayın:

   ![SQL ayarları](./media/tutorial-create-azure-ssis-runtime-portal/sql-settings.png)

    a. **Subscription** (Abonelik) alanında SSISDB'yi barındıran Azure aboneliğini seçin.

    b. **Location** (Konum) alanında SSISDB'yi barındıran veritabanı sunucunuzun konumunu seçin. Tümleştirme çalışma zamanınızla aynı konumu seçmenizi öneririz.

    c. **Catalog Database Server Endpoint** (Katalog Veritabanı Sunucusu Uç Noktası) alanında SSISDB'yi barındıracak veritabanı sunucunuzun uç noktasını seçin. Seçilen veritabanı sunucusuna göre SSISDB sizin adınıza tek veritabanı, elastik havuzun bir parçası veya Yönetilen Örnek biçiminde oluşturulabilir ve genel ağ üzerinden veya sanal ağa eklenerek erişilebilir.

    d. Üzerinde **kullanım AAD kimlik doğrulaması...**  onay kutusunu SSISDB'yi barındırmak için veritabanı sunucusu için kimlik doğrulama yöntemini seçin: SQL veya Azure Active Directory (AAD), Azure Data Factory için yönetilen kimliğe sahip. Kaydetmeden gerekirse veritabanı sunucusuna erişim izni olan bir AAD gruba, ADF için yönetilen kimlik eklemek için bkz: [etkinleştirme AAD kimlik doğrulaması için Azure-SSIS IR](https://docs.microsoft.com/azure/data-factory/enable-aad-authentication-azure-ssis-ir).

    e. **Admin Username** (Yönetici Kullanıcı Adı) alanına SSISDB'yi barındıracak veritabanı sunucunuzun SQL kimlik doğrulaması kullanıcı adını girin.

    f. **Admin Password** (Yönetici Parolası) alanına SSISDB'yi barındıracak veritabanı sunucunuzun SQL kimlik doğrulaması parolasını girin.

    g. İçin **Katalog veritabanı hizmet katmanı**, veritabanı sunucunuza SSISDB'yi barındırmak için Hizmet katmanını seçin: Temel/standart/Premium katmanı veya elastik havuz adı.

    h. **Test Connection** (Bağlantıyı Sına) öğesine tıklayın ve başarılı olursa **Next** (İleri) öğesine tıklayın.

4. **Advanced Settings** (Gelişmiş Ayarlar) sayfasında aşağıdaki adımları tamamlayın:

    ![Gelişmiş ayarlar](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings.png)

    a. **Maximum Parallel Executions Per Node** (Düğüm Başına Maksimum Paralel Yürütme) alanında tümleştirme çalışma zamanınızda her düğümde eş zamanlı olarak yürütülecek maksimum paket sayısını seçin. Yalnızca desteklenen paket sayıları görüntülenir. İşlem/bellek tek bir büyük/ağır paketini çalıştırmak için birden fazla çekirdeği kullanmak istiyorsanız, düşük bir sayı seçin-yoğun. Tek bir çekirdekte bir veya daha fazla küçük/hafif paket çalıştırmak istiyorsanız daha yüksek bir sayı seçin.

    b. **Custom Setup Container SAS URI** (Özel Kurulum Kapsayıcısı SAS URI'si) alanında isteğe bağlı olarak kurulum betiğinizin ve ilgili dosyalarının depolandığı Azure Depolama Blobu kapsayıcısının Paylaşılan Erişim İmzası (SAS) Tekdüzen Kaynak Tanımlayıcısı (URI) değerini girebilirsiniz, bkz. [Azure-SSIS IR için özel kurulum](https://docs.microsoft.com/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup).

5. Üzerinde **bir sanal ağ seçin...**  onay kutusunu, tümleştirme çalışma zamanı bir sanal ağa katılmak isteyip istemediğinizi belirtin. Sanal ağ hizmet uç noktaları/yönetilen örnek SSISDB'yi barındırmak için ile Azure SQL veritabanı'nı kullanma veya şirket içi verilere erişim gerektiren kontrol edin; diğer bir deyişle, şirket içi veri kaynaklarınız/hedeflerini SSIS paketlerinizi bkz [katılın Azure-SSIS IR'yi bir sanal ağa](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network). Bunu işaretlerseniz, aşağıdaki adımları tamamlayın:

   ![Sanal ağ ile Gelişmiş ayarlar](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings-vnet.png)

    a. İçin **abonelik**, sanal ağınıza sahip Azure aboneliğini seçin.

    b. İçin **konumu**, Integration runtime'nın aynı konum seçilir.

    c. İçin **türü**, sanal ağınızı türünü seçin: Klasik veya Azure Resource Manager. Öneririz Azure Resource Manager sanal ağı seçin, sonra Klasik sanal ağ yakında kullanımdan kaldırılacak.

    d. İçin **VNet adı**, sanal ağınızın adını seçin. Bu sanal ağ, şirket içi ağınıza bağlı ve SSISDB barındırmak için sanal ağ hizmet uç noktaları/yönetilen ile örneği Azure SQL veritabanı için kullanılan aynı sanal ağda olmalıdır.

    e. İçin **alt ağ adı**, sanal ağınızın alt ağ adını seçin. Bu yönetilen örneği için SSISDB'yi barındırmak için kullanılan farklı bir alt olmalıdır.

6. Tıklayın **VNet doğrulama** ve başarılı olursa, tıklayın **son** , Azure-SSIS tümleştirme çalışma zamanının oluşturulmasını başlatmak için.

    > [!IMPORTANT]
    > - Bu işlem yaklaşık 20-30 dakikada tamamlanan
    > - Data Factory hizmeti, SSIS Kataloğu veritabanını (SSISDB) hazırlamak için Azure SQL Veritabanınıza bağlanır. Ayrıca belirtilmişse sanal ağınıza ilişkin izin ve ayarları yapılandırır ve yeni bir Azure-SSIS tümleştirme çalışma zamanı örneğini sanal ağa katılır.

7. **Bağlantılar** penceresinde, gerekirse **Tümleştirme Çalışma Zamanları**’na geçin. Durumu yenilemek için **Yenile**’ye tıklayın.

   ![Oluşturma durumu](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-creation-status.png)

8. Altındaki bağlantıları kullanın **eylemleri** durdurmak/başlatmak, düzenlemek veya Integration runtime'ı silmek için sütun. Tümleştirme çalışma zamanının JSON kodunu görüntülemek için son bağlantıyı kullanın. Düzenle ve sil düğmeleri yalnızca IR durdurulmuş durumdayken etkin olur.

   ![Azure SSIS IR - eylemler](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-actions.png)

### <a name="azure-ssis-integration-runtimes-in-the-portal"></a>Portalda Azure SSIS tümleştirmesi çalışma zamanları

1. Veri fabrikanızdaki mevcut tümleştirme çalışma zamanlarını görüntülemek için Azure Data Factory kullanıcı arabiriminde **Düzenle** sekmesine geçin, **Bağlantılar**’a tıklayın ve sonra **Tümleştirme Çalışma Zamanları** sekmesine geçin.

   ![Mevcut Ir'leri görüntüle](./media/tutorial-create-azure-ssis-runtime-portal/view-azure-ssis-integration-runtimes.png)

2. Yeni bir Azure-SSIS IR oluşturmak için **Yeni**'ye tıklayın.

   ![Menü aracılığıyla tümleştirme çalışma zamanı](./media/tutorial-create-azure-ssis-runtime-portal/edit-connections-new-integration-runtime-button.png)

3. Azure-SSIS tümleştirme çalışma zamanı oluşturmak için resimde gösterildiği gibi **Yeni**’ye tıklayın.
4. Tümleştirme Çalışma Zamanı Kurulumu penceresinde **Mevcut SSIS paketlerini Azure’da yürütmek üzere lift-and-shift ile taşıma**’yı seçip **İleri**’ye tıklayın.

   ![Tümleştirme çalışma zamanının türünü belirtin](./media/tutorial-create-azure-ssis-runtime-portal/integration-runtime-setup-options.png)

5. Geriye kalan Azure SSIS IR kurulumu adımları için bkz. [Azure SSIS tümleştirme çalışma zamanı sağlama](#provision-an-azure-ssis-integration-runtime).

## <a name="azure-powershell"></a>Azure PowerShell

Bu bölümde, bir Azure-SSIS IR oluşturmak için Azure PowerShell kullanma

### <a name="create-variables"></a>Değişken oluşturma

Bu öğreticideki betiklerde kullanılacak değişkenleri tanımlayın:

```powershell
### Azure Data Factory information
# If your input contains a PSH special character, e.g. "$", precede it with the escape character "`" like "`$"
$SubscriptionName = "[your Azure subscription name]"
$ResourceGroupName = "[your Azure resource group name]"
$DataFactoryName = "[your data factory name]"
# For supported regions, see https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all
$DataFactoryLocation = "EastUS"

### Azure-SSIS integration runtime information - This is a Data Factory compute resource for running SSIS packages
$AzureSSISName = "[specify a name for your Azure-SSIS IR]"
$AzureSSISDescription = "[specify a description for your Azure-SSIS IR]"
# For supported regions, see https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all
$AzureSSISLocation = "EastUS"
# For supported node sizes, see https://azure.microsoft.com/pricing/details/data-factory/ssis/
$AzureSSISNodeSize = "Standard_D8_v3"
# 1-10 nodes are currently supported
$AzureSSISNodeNumber = 2
# Azure-SSIS IR edition/license info: Standard or Enterprise
$AzureSSISEdition = "Standard" # Standard by default, while Enterprise lets you use advanced/premium features on your Azure-SSIS IR
# Azure-SSIS IR hybrid usage info: LicenseIncluded or BasePrice
$AzureSSISLicenseType = "LicenseIncluded" # LicenseIncluded by default, while BasePrice lets you bring your own on-premises SQL Server license with Software Assurance to earn cost savings from Azure Hybrid Benefit (AHB) option
# For a Standard_D1_v2 node, up to 4 parallel executions per node are supported, but for other nodes, up to (2 x number of cores) are currently supported
$AzureSSISMaxParallelExecutionsPerNode = 8
# Custom setup info
$SetupScriptContainerSasUri = "" # OPTIONAL to provide SAS URI of blob container where your custom setup script and its associated files are stored
# Virtual network info: Classic or Azure Resource Manager
$VnetId = "[your virtual network resource ID or leave it empty]" # REQUIRED if you use Azure SQL Database with virtual network service endpoints/Managed Instance/on-premises data, Azure Resource Manager virtual network is recommended, Classic virtual network will be deprecated soon
$SubnetName = "[your subnet name or leave it empty]" # WARNING: Please use the same subnet as the one used with your Azure SQL Database with virtual network service endpoints or a different subnet than the one used for your Managed Instance

### SSISDB info
$SSISDBServerEndpoint = "[your Azure SQL Database server name or Managed Instance name.DNS prefix].database.windows.net" # WARNING: Please ensure that there is no existing SSISDB, so we can prepare and manage one on your behalf
# Authentication info: SQL or Azure Active Directory (AAD)
$SSISDBServerAdminUserName = "[your server admin username for SQL authentication or leave it empty for AAD authentication]"
$SSISDBServerAdminPassword = "[your server admin password for SQL authentication or leave it empty for AAD authentication]"
$SSISDBPricingTier = "[Basic|S0|S1|S2|S3|S4|S6|S7|S9|S12|P1|P2|P4|P6|P11|P15|…|ELASTIC_POOL(name = <elastic_pool_name>) for Azure SQL Database or leave it empty for Managed Instance]"
```

### <a name="sign-in-and-select-subscription"></a>Oturum açın ve aboneliği seçin

Komut dosyasını açıp Azure aboneliğinizi seçmek için aşağıdaki kodu ekleyin:

```powershell
Connect-AzAccount
Select-AzSubscription -SubscriptionName $SubscriptionName
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
# Make sure to run this script against the subscription to which the virtual network belongs
if(![string]::IsNullOrEmpty($VnetId) -and ![string]::IsNullOrEmpty($SubnetName))
{
    # Register to the Azure Batch resource provider
    $BatchApplicationId = "ddbf3205-c6bd-46ae-8127-60eb93363864"
    $BatchObjectId = (Get-AzADServicePrincipal -ServicePrincipalName $BatchApplicationId).Id
    Register-AzResourceProvider -ProviderNamespace Microsoft.Batch
    while(!(Get-AzResourceProvider -ProviderNamespace "Microsoft.Batch").RegistrationState.Contains("Registered"))
    {
    Start-Sleep -s 10
    }
    if($VnetId -match "/providers/Microsoft.ClassicNetwork/")
    {
        # Assign the VM contributor role to Microsoft.Batch
        New-AzRoleAssignment -ObjectId $BatchObjectId -RoleDefinitionName "Classic Virtual Machine Contributor" -Scope $VnetId
    }
}
```

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturun

Oluşturma bir [Azure kaynak grubu](../azure-resource-manager/resource-group-overview.md) kullanarak [yeni AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) komutu. Kaynak grubu, Azure kaynaklarının grup olarak dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

```powershell
New-AzResourceGroup -Location $DataFactoryLocation -Name $ResourceGroupName
```

### <a name="create-a-data-factory"></a>Data factory oluştur

Veri fabrikası oluşturmak için aşağıdaki komutu çalıştırın.

```powershell
Set-AzDataFactoryV2 -ResourceGroupName $ResourceGroupName `
                         -Location $DataFactoryLocation `
                         -Name $DataFactoryName
```

### <a name="create-an-integration-runtime"></a>Tümleştirme çalışma zamanı oluşturma

Azure'da SSIS paketlerini çalıştıran bir Azure-SSIS tümleştirme çalışma zamanı oluşturmak için aşağıdaki komutları çalıştırın.

Siz değil ile sanal ağ hizmet uç noktaları/yönetilen örnek SSISDB'yi barındırmak için Azure SQL veritabanı'nı kullanın ya da şirket içi verilere erişim gerektiren Vnetıd ve alt ağ parametrelerini atlayın veya boş değerler geçirileceğini. Aksi takdirde, olamaz bunları çıkarın ve gerekir geçerli değerler, sanal ağ yapılandırmasından geçmek için bkz: [katılın Azure-SSIS IR'yi bir sanal ağa](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network).

SSISDB'yi barındırmak için yönetilen örneği kullanıyorsanız, CatalogPricingTier parametreyi atlarsanız veya bunun için boş bir değer geçirin. Aksi takdirde, olamaz atlayın ve gerekir geçerli bir değer listesinden fiyatlandırma katmanları Azure SQL veritabanı için desteklenen geçmek için bkz: [SQL veritabanı kaynak limitleri](../sql-database/sql-database-resource-limits.md).

Azure Active Directory (AAD) kimlik doğrulaması, Azure Data Factory için yönetilen kimliğe sahip bir veritabanı sunucusuna bağlanmak için kullandığınız, CatalogAdminCredential parametreyi atlayabilirsiniz, ancak içine bir AAD grubu ile ADF için yönetilen kimlik eklemeniz gerekir erişim izinlerini veritabanı sunucusu için bkz: [etkinleştirme AAD kimlik doğrulaması için Azure-SSIS IR](https://docs.microsoft.com/azure/data-factory/enable-aad-authentication-azure-ssis-ir). Aksi takdirde, atlayamazsınız ve Sunucu Yöneticisi kullanıcı adı ve parola SQL kimlik doğrulaması için oluşturulmuş geçerli bir nesne geçmesi gerekir.

```powershell
Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                           -DataFactoryName $DataFactoryName `
                                           -Name $AzureSSISName `
                                           -Description $AzureSSISDescription `
                                           -Type Managed `
                                           -Location $AzureSSISLocation `
                                           -NodeSize $AzureSSISNodeSize `
                                           -NodeCount $AzureSSISNodeNumber `
                                           -Edition $AzureSSISEdition `
                                           -LicenseType $AzureSSISLicenseType `
                                           -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                           -VnetId $VnetId `
                                           -Subnet $SubnetName `
                                           -CatalogServerEndpoint $SSISDBServerEndpoint `
                                           -CatalogPricingTier $SSISDBPricingTier

# Add SetupScriptContainerSasUri parameter when you use custom setup
if(![string]::IsNullOrEmpty($SetupScriptContainerSasUri))
{
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -SetupScriptContainerSasUri $SetupScriptContainerSasUri
}

# Add CatalogAdminCredential parameter when you do not use AAD authentication
if(![string]::IsNullOrEmpty($SSISDBServerAdminUserName) –and ![string]::IsNullOrEmpty($SSISDBServerAdminPassword))
{
    $secpasswd = ConvertTo-SecureString $SSISDBServerAdminPassword -AsPlainText -Force
    $serverCreds = New-Object System.Management.Automation.PSCredential($SSISDBServerAdminUserName, $secpasswd)

    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -CatalogAdminCredential $serverCreds
}
```

### <a name="start-integration-runtime"></a>Tümleştirme çalışma zamanını başlatma

Azure-SSIS tümleştirme çalışma zamanını başlatmak için aşağıdaki komutu çalıştırın:

```powershell
write-host("##### Starting #####")
Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

write-host("##### Completed #####")
write-host("If any cmdlet is unsuccessful, please consider using -Debug option for diagnostics.")
```

Bu komutun tamamlanması **20-30 dakika** sürer.

### <a name="full-script"></a>Tam betik

Bir Azure-SSIS tümleştirme çalışma zamanı oluşturur tam komut dosyası aşağıda verilmiştir.

```powershell
### Azure Data Factory information
# If your input contains a PSH special character, e.g. "$", precede it with the escape character "`" like "`$"
$SubscriptionName = "[your Azure subscription name]"
$ResourceGroupName = "[your Azure resource group name]"
$DataFactoryName = "[your data factory name]"
# For supported regions, see https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all
$DataFactoryLocation = "EastUS"

### Azure-SSIS integration runtime information - This is a Data Factory compute resource for running SSIS packages
$AzureSSISName = "[specify a name for your Azure-SSIS IR]"
$AzureSSISDescription = "[specify a description for your Azure-SSIS IR]"
# For supported regions, see https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all
$AzureSSISLocation = "EastUS"
# For supported node sizes, see https://azure.microsoft.com/pricing/details/data-factory/ssis/
$AzureSSISNodeSize = "Standard_D8_v3"
# 1-10 nodes are currently supported
$AzureSSISNodeNumber = 2
# Azure-SSIS IR edition/license info: Standard or Enterprise
$AzureSSISEdition = "Standard" # Standard by default, while Enterprise lets you use advanced/premium features on your Azure-SSIS IR
# Azure-SSIS IR hybrid usage info: LicenseIncluded or BasePrice
$AzureSSISLicenseType = "LicenseIncluded" # LicenseIncluded by default, while BasePrice lets you bring your own on-premises SQL Server license with Software Assurance to earn cost savings from Azure Hybrid Benefit (AHB) option
# For a Standard_D1_v2 node, up to 4 parallel executions per node are supported, but for other nodes, up to (2 x number of cores) are currently supported
$AzureSSISMaxParallelExecutionsPerNode = 8
# Custom setup info
$SetupScriptContainerSasUri = "" # OPTIONAL to provide SAS URI of blob container where your custom setup script and its associated files are stored
# Virtual network info: Classic or Azure Resource Manager
$VnetId = "[your virtual network resource ID or leave it empty]" # REQUIRED if you use Azure SQL Database with virtual network service endpoints/Managed Instance/on-premises data, Azure Resource Manager virtual network is recommended, Classic virtual network will be deprecated soon
$SubnetName = "[your subnet name or leave it empty]" # WARNING: Please use the same subnet as the one used with your Azure SQL Database with virtual network service endpoints or a different subnet than the one used for your Managed Instance

### SSISDB info
$SSISDBServerEndpoint = "[your Azure SQL Database server name or Managed Instance name.DNS prefix].database.windows.net" # WARNING: Please ensure that there is no existing SSISDB, so we can prepare and manage one on your behalf
# Authentication info: SQL or Azure Active Directory (AAD)
$SSISDBServerAdminUserName = "[your server admin username for SQL authentication or leave it empty for AAD authentication]"
$SSISDBServerAdminPassword = "[your server admin password for SQL authentication or leave it empty for AAD authentication]"
$SSISDBPricingTier = "[Basic|S0|S1|S2|S3|S4|S6|S7|S9|S12|P1|P2|P4|P6|P11|P15|…|ELASTIC_POOL(name = <elastic_pool_name>) for Azure SQL Database or leave it empty for Managed Instance]"

### Log in and select subscription
Connect-AzAccount
Select-AzSubscription -SubscriptionName $SubscriptionName

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
# Make sure to run this script against the subscription to which the virtual network belongs
if(![string]::IsNullOrEmpty($VnetId) -and ![string]::IsNullOrEmpty($SubnetName))
{
    # Register to the Azure Batch resource provider
    $BatchApplicationId = "ddbf3205-c6bd-46ae-8127-60eb93363864"
    $BatchObjectId = (Get-AzADServicePrincipal -ServicePrincipalName $BatchApplicationId).Id
    Register-AzResourceProvider -ProviderNamespace Microsoft.Batch
    while(!(Get-AzResourceProvider -ProviderNamespace "Microsoft.Batch").RegistrationState.Contains("Registered"))
    {
    Start-Sleep -s 10
    }
    if($VnetId -match "/providers/Microsoft.ClassicNetwork/")
    {
        # Assign the VM contributor role to Microsoft.Batch
        New-AzRoleAssignment -ObjectId $BatchObjectId -RoleDefinitionName "Classic Virtual Machine Contributor" -Scope $VnetId
    }
}

### Create a data factory
Set-AzDataFactoryV2 -ResourceGroupName $ResourceGroupName `
                         -Location $DataFactoryLocation `
                         -Name $DataFactoryName

### Create an integration runtime
Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                           -DataFactoryName $DataFactoryName `
                                           -Name $AzureSSISName `
                                           -Description $AzureSSISDescription `
                                           -Type Managed `
                                           -Location $AzureSSISLocation `
                                           -NodeSize $AzureSSISNodeSize `
                                           -NodeCount $AzureSSISNodeNumber `
                                           -Edition $AzureSSISEdition `
                                           -LicenseType $AzureSSISLicenseType `
                                           -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                           -VnetId $VnetId `
                                           -Subnet $SubnetName `
                                           -CatalogServerEndpoint $SSISDBServerEndpoint `
                                           -CatalogPricingTier $SSISDBPricingTier

# Add SetupScriptContainerSasUri parameter when you use custom setup
if(![string]::IsNullOrEmpty($SetupScriptContainerSasUri))
{
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -SetupScriptContainerSasUri $SetupScriptContainerSasUri
}

# Add CatalogAdminCredential parameter when you do not use AAD authentication
if(![string]::IsNullOrEmpty($SSISDBServerAdminUserName) –and ![string]::IsNullOrEmpty($SSISDBServerAdminPassword))
{
    $secpasswd = ConvertTo-SecureString $SSISDBServerAdminPassword -AsPlainText -Force
    $serverCreds = New-Object System.Management.Automation.PSCredential($SSISDBServerAdminUserName, $secpasswd)

    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -CatalogAdminCredential $serverCreds
}

### Start integration runtime
write-host("##### Starting your Azure-SSIS integration runtime. This command takes 20 to 30 minutes to complete. #####")
Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

write-host("##### Completed #####")
write-host("If any cmdlet is unsuccessful, please consider using -Debug option for diagnostics.")
```

## <a name="azure-resource-manager-template"></a>Azure Resource Manager şablonu

Bu bölümde, Azure-SSIS tümleştirme çalışma zamanı oluşturmak için Azure Resource Manager şablonu kullanın. İzlenecek örnek yol şu şekildedir:

1. Aşağıdaki Azure Resource Manager şablonu ile bir JSON dosyası oluşturun. Açılı ayraçlar (yer tutucu) değerleri kendi değerlerinizle değiştirin.

    ```json
    {
      "contentVersion": "1.0.0.0",
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "parameters": {},
        "variables": {},
        "resources": [{
            "name": "<Specify a name for your data factory>",
            "apiVersion": "2018-06-01",
            "type": "Microsoft.DataFactory/factories",
            "location": "East US",
            "properties": {},
            "resources": [{
                "type": "integrationruntimes",
                "name": "<Specify a name for your Azure-SSIS IR>",
                "dependsOn": [ "<The name of the data factory you specified at the beginning>" ],
                "apiVersion": "2018-06-01",
                "properties": {
                    "type": "Managed",
                    "typeProperties": {
                        "computeProperties": {
                            "location": "East US",
                            "nodeSize": "Standard_D8_v3",
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

2. Azure Resource Manager şablonu dağıtmak için burada ADFTutorialResourceGroup kaynak grubunuzun adını, JSON tanımı içeren dosyanın C:\adftutorial ise aşağıdaki örnekte gösterildiği gibi yeni AzResourceGroupDeployment komutu çalıştırın. data factory ve Azure-SSIS IR

    ```powershell
    New-AzResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json
    ```

    Bu komut, data factory ve Azure-SSIS IR içinde oluşturur, ancak IR başlamıyor

3. Azure-SSIS IR başlatmak için başlangıç AzDataFactoryV2IntegrationRuntime komutu çalıştırın:

    ```powershell
    Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName "<Resource Group Name>" `
                                                 -DataFactoryName "<Data Factory Name>" `
                                                 -Name "<Azure SSIS IR Name>" `
                                                 -Force
    ```

## <a name="deploy-ssis-packages"></a>SSIS paketlerini dağıtma

Şimdi SQL Server Veri Araçları (SSDT) veya SQL Server Management Studio’yu (SSMS) kullanarak SSIS paketlerinizi Azure’a dağıtın. SSIS kataloğunu (SSISDB) barındıran veritabanı sunucunuza bağlanın. Veritabanı sunucusunun adını biçimdedir: &lt;Azure SQL veritabanı sunucu adı&gt;. database.windows.net veya &lt;yönetilen örnek adı. DNS ön eki&gt;. database.windows.net. Yönergeler için [Paket dağıtma](/sql/integration-services/packages/deploy-integration-services-ssis-projects-and-packages#deploy-packages-to-integration-services-server) makalesine bakın.

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede diğer Azure-SSIS IR konulara bakın:

- [Azure-SSIS tümleştirme çalışma zamanı](concepts-integration-runtime.md#azure-ssis-integration-runtime). Bu makalede Azure-SSIS IR'yi genel dahil tümleştirme çalışma zamanları hakkında kavramsal bilgiler sağlar
- [Öğretici: SSIS paketlerini Azure’a dağıtma](tutorial-create-azure-ssis-runtime-portal.md). Bu makale bir Azure-SSIS IR oluşturmaya ilişkin adım adım yönergeler sağlar ve SSIS kataloğunu barındırmak için bir Azure SQL veritabanı kullanır.
- [Azure-SSIS IR’yi izleme](monitor-integration-runtime.md#azure-ssis-integration-runtime). Bu makalede bir Azure-SSIS IR ile ilgili bilgileri ve döndürülen bilgilerdeki durumların açıklamalarını alma işlemi gösterilmektedir.
- [Azure-SSIS IR’yi yönetme](manage-azure-ssis-integration-runtime.md). Bu makale bir Azure-SSIS IR’yi durdurma, başlatma veya kaldırma işlemini gösterir. Ayrıca, IR’ye daha fazla düğüm ekleyerek Azure-SSIS IR’nizi ölçeklendirmeyi gösterir.
- [Azure-SSIS IR’yi bir sanal ağa ekleyin](join-azure-ssis-integration-runtime-virtual-network.md). Bu makalede, Azure-SSIS IR, Azure sanal ağına birleştirme hakkında kavramsal bilgiler verilmektedir. Ayrıca, Azure portalını kullanarak Azure-SSIS IR’nin sanal ağa katılmasını sağlayacak şekilde sanal ağı yapılandırma adımlarını da sunar.
