---
title: Azure Data Factory'de Azure-SSIS tümleştirme çalışma zamanı oluştur | Microsoft Docs
description: Dağıtımı ve SSIS paketlerini Azure'da çalıştırmak için Azure Data Factory'de bir Azure-SSIS tümleştirme çalışma zamanı oluşturmayı öğrenin.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/23/2018
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: 90f6841ddc2fe1c017dbfc7e9046fae4a0ceb097
ms.sourcegitcommit: 6361a3d20ac1b902d22119b640909c3a002185b3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2018
ms.locfileid: "49363818"
---
# <a name="create-the-azure-ssis-integration-runtime-in-azure-data-factory"></a>Azure Data Factory'de Azure-SSIS tümleştirme çalışma zamanı oluşturma
Bu makalede, Azure Data factory'de bir Azure-SSIS tümleştirme çalışma zamanı sağlama adımlarını sunar. Daha sonra, SQL Server Veri Araçları (SSDT) veya SQL Server Management Studio'yu (SSMS) kullanarak Azure'da bu çalışma zamanında SQL Server Integration Services (SSIS) paketleri dağıtabilir ve çalıştırabilirsiniz. 

Öğreticiyi [Öğreticisi: (SSIS) SQL Server Integration Services paketlerini Azure'a dağıtma](tutorial-create-azure-ssis-runtime-portal.md) SSIS kataloğunu barındırmak için Azure SQL veritabanını kullanarak bir Azure-SSIS tümleştirme çalışma zamanı (IR) oluşturma işlemi gösterilmektedir. Bu makale öğreticiyi genişletip ve aşağıdaki işlemleri gösterilmektedir: 

- İsteğe bağlı olarak Azure SQL veritabanı, SSIS kataloğunu (SSISDB veritabanı) barındırmak için veritabanı sunucusu olarak sanal ağ hizmet uç noktaları/yönetilen ile örneği kullanın. SSISDB'yi barındırmak için veritabanı sunucu türünü seçmek kılavuzluk etmesi için bkz [karşılaştırma SQL veritabanı mantıksal sunucusu ve SQL veritabanı yönetilen örneği](create-azure-ssis-integration-runtime.md#compare-sql-database-logical-server-and-sql-database-managed-instance). Bir önkoşul olarak, Azure-SSIS IR'yi bir sanal ağa katılın ve sanal ağ izinleri ve ayarları gerektiği şekilde yapılandırmanız gerekir. Bkz: [katılın Azure-SSIS IR'yi bir sanal ağa](https://docs.microsoft.com/en-us/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network). 

- İsteğe bağlı olarak Azure Active Directory (AAD) kimlik doğrulaması, Azure Data Factory yönetilen kimliklerle Azure-SSIS IR için Azure kaynakları için bir veritabanı sunucusuna bağlanmak için kullanın. Bir önkoşul olarak ihtiyacınız olacak, Data Factory MSI veritabanı sunucusuna erişim izni olan bir AAD gruba eklemek için bkz: [etkinleştirme AAD kimlik doğrulaması için Azure-SSIS IR](https://docs.microsoft.com/en-us/azure/data-factory/enable-aad-authentication-azure-ssis-ir). 

## <a name="overview"></a>Genel Bakış
Bu makalede bir Azure-SSIS IR sağladıktan farklı yöntemler gösterilmektedir: 

- [Azure portal](#azure-portal) 
- [Azure PowerShell](#azure-powershell) 
- [Azure Resource Manager şablonu](#azure-resource-manager-template) 

Bir Azure-SSIS IR oluşturmak, Data Factory SSIS Kataloğu veritabanını (SSISDB) hazırlamak için Azure SQL veritabanına bağlanır. Betik ayrıca belirtilmişse sanal ağınıza ilişkin izin ve ayarları yapılandırır ve yeni bir Azure-SSIS tümleştirme çalışma zamanı örneğini sanal ağa katılır. 

Bir Azure-SSIS IR örneğini sağladığınızda, SSIS için Azure Feature Pack ve Access Redistributable da yüklenir. Bu bileşenler, Excel ile Access dosyalarıyla ve yerleşik bileşenler tarafından desteklenen veri kaynaklarına ek olarak çeşitli Azure veri kaynaklarıyla bağlantı kurma olanağı sunar. Ek bileşenleri de yükleyebilirsiniz. Daha fazla bilgi için bkz. [Azure-SSIS tümleştirmesi çalışma zamanı için özel kurulum](how-to-configure-azure-ssis-ir-custom-setup.md). 

## <a name="prerequisites"></a>Önkoşullar 
- **Azure aboneliği**. Bir aboneliğiniz yoksa, bir [ücretsiz deneme](http://azure.microsoft.com/pricing/free-trial/) hesabı oluşturabilirsiniz. 

- **Azure SQL veritabanı mantıksal sunucusu veya yönetilen örneği**. Henüz bir veritabanı sunucunuz yoksa, başlamadan önce Azure portalında bir tane oluşturun. Bu sunucu, SSIS Katalog veritabanını (SSISDB) barındırır. Veritabanı sunucusunu tümleştirme çalışma zamanı ile aynı Azure bölgesinde oluşturmanız önerilir. Bu yapılandırma, tümleştirme çalışma zamanının Azure bölgelerinden geçmeden SSISDB’ye yürütme günlüklerini yazmasına olanak tanır. Seçilen veritabanı sunucusuna göre SSISDB sizin adınıza tek veritabanı, elastik havuzun bir parçası veya Yönetilen Örnek biçiminde oluşturulabilir ve genel ağ üzerinden veya sanal ağa eklenerek erişilebilir. Azure SQL veritabanı için desteklenen fiyatlandırma katmanlarının bir listesi için bkz. [SQL veritabanı kaynak limitleri](../sql-database/sql-database-resource-limits.md). 

    Azure SQL veritabanı mantıksal sunucusu veya yönetilen örneği zaten SSIS kataloğuna (SSIDB veritabanı) sahip değil, emin olun. Azure-SSIS IR’nin sağlanması, mevcut bir SSIS Kataloğunun kullanılmasını desteklemez. 

- **Klasik veya Azure Resource Manager sanal ağı (isteğe bağlı)**. Aşağıdaki koşullardan biri doğru ise bir Azure sanal ağ olması gerekir: 
    - Sanal ağ hizmet uç noktaları/yönetilen bir sanal ağ içinde olan örneği ile Azure SQL veritabanı'nda SSIS Kataloğu veritabanını barındırır. 
    - Şirket içi veri depolarına bir Azure-SSIS tümleştirme çalışma zamanı üzerinde çalışan SSIS paketlerinden bağlanmak istiyorsanız. 

- **Azure PowerShell**. Bölümündeki yönergeleri [Azure PowerShell'i yükleme ve yapılandırma konusunda](/powershell/azure/install-azurerm-ps), bulutta SSIS paketleri çalıştıran sağlama Azure-SSIS tümleştirme çalışma zamanı için bir betik çalıştırmak için PowerShell kullanıyorsanız. 

### <a name="region-support"></a>Bölge desteği
Data Factory'nin kullanılabileceği Azure bölgelerinin bir listesi için bir sonraki sayfada ilgilendiğiniz bölgeleri seçin ve **Analytics**'i genişleterek **Data Factory**: [Products available by region](https://azure.microsoft.com/global-infrastructure/services/) (Bölgeye göre kullanılabilir durumdaki ürünler) bölümünü bulun.

Hangi Azure-SSIS tümleştirme çalışma zamanı şu anda Azure bölgelerinin listesi için aşağıdaki sayfada faiz ve ardından genişletin bölgeleri seçin **Analytics** bulunacak **SSIS tümleştirme çalışma zamanı** : [Bölgelere göre kullanılabilir ürünler](https://azure.microsoft.com/global-infrastructure/services/). ### SQL veritabanı karşılaştırın ve yönetilen örneği

### <a name="compare-sql-database-logical-server-and-sql-database-managed-instance"></a>SQL veritabanı mantıksal sunucusu ve SQL veritabanı yönetilen örneği karşılaştırın

SSIR Azure IR teklifiyle gibi belirli özelliklerini SQL veritabanı mantıksal sunucusu ve SQL veritabanı yönetilen örneği aşağıdaki tabloda karşılaştırılmıştır:

| Özellik | SQL veritabanı mantıksal sunucusu| SQL veritabanı - yönetilen örnek |
|---------|--------------|------------------|
| **Zamanlama** | SQL Server Aracısı kullanılamıyor.<br/><br/>Bkz: [bir Azure Data Factory işlem hattı bir parçası olarak bir paket zamanlama](/sql/integration-services/lift-shift/ssis-azure-schedule-packages#activity).| SQL Server Agent kullanılabilir. |
| **Kimlik doğrulaması** | Herhangi bir Azure Active Directory kullanıcı temsil eden bir kapsanan veritabanı kullanıcı hesabı ile bir veritabanı oluşturabilirsiniz **dbmanager** rol.<br/><br/>Bkz: [etkinleştirin, Azure SQL veritabanı'nda Azure AD](enable-aad-authentication-azure-ssis-ir.md#enable-azure-ad-on-azure-sql-database). | Azure AD yönetici dışında herhangi bir Azure Active Directory kullanıcı temsil eden bir kapsanan veritabanı kullanıcı hesabı ile bir veritabanı oluşturulamıyor <br/><br/>Bkz: [etkinleştirin, Azure SQL veritabanı yönetilen örneği Azure AD](enable-aad-authentication-azure-ssis-ir.md#enable-azure-ad-on-azure-sql-database-managed-instance). |
| **Hizmet katmanı** | SQL veritabanı'nda Azure-SSIS IR oluşturduğunuzda, hizmet katmanı için SSISDB seçebilirsiniz. Birden çok hizmet katmanlarını vardır. | Azure-SSIS IR yönetilen örneği'nde oluşturduğunuzda, hizmet katmanı için SSISDB seçemezsiniz. Tüm veritabanları aynı yönetilen örneği, örneğe ayrılan aynı kaynak paylaşır. |
| **Sanal ağ** | Hem Azure Resource Manager ve klasik sanal ağlar'ı destekler. | Yalnızca Azure Resource Manager sanal ağı destekler. Sanal ağ gereklidir.<br/><br/>Yönetilen örneği aynı sanal ağ için Azure-SSIS IR katılırsanız, Azure-SSIS IR yönetilen örneği'den farklı bir alt ağda olduğundan emin olun. Azure-SSIS IR, yönetilen örneğe farklı bir sanal ağa katılırsanız, (Bu aynı bölgeye sınırlıdır) sanal ağ eşlemesi veya sanal ağ için sanal ağ bağlantısı öneririz. Bkz: [uygulamanızı Azure SQL veritabanı yönetilen örneği bağlamak](../sql-database/sql-database-managed-instance-connect-app.md). |
| **Dağıtılmış işlemler** | Microsoft Dağıtılmış İşlem Düzenleyicisi (MSDTC) işlemler desteklenmez. Paketlerinizi dağıtılmış işlemleri düzenlemesine olanak MSDTC kullanıyorsanız, SQL veritabanı için esnek işlemleri'ni kullanarak geçici bir çözüm uygulamak mümkün olabilir. Şu anda SSIS elastik işlemler için yerleşik destek yok. Elastik işlemler SSIS paketinizdeki kullanmak için bir betik görevde özel ADO.NET kod yazmak zorunda. Bu betik görevi başlangıcını ve bitişini işlemi ve işlemin gerçekleşmesi gereken tüm eylemleri içermelidir.<br/><br/>Elastik işlemler kodlama hakkında daha fazla bilgi için bkz. [ile Azure SQL veritabanı elastik işlemler](https://azure.microsoft.com/blog/elastic-database-transactions-with-azure-sql-database/). Genel olarak, elastik işlemler hakkında daha fazla bilgi için bkz [bulut veritabanlarında dağıtılmış işlemler](../sql-database/sql-database-elastic-transactions-overview.md). | Desteklenmiyor. |
| | | |

## <a name="azure-portal"></a>Azure portal
Bu bölümde, bir Azure-SSIS IR oluşturmak için Azure Portal'daki Data Factory UI özellikle kullanın 

### <a name="create-a-data-factory"></a>Veri fabrikası oluşturma 
1. **Microsoft Edge** veya **Google Chrome** web tarayıcısını açın. Şu anda Data Factory kullanıcı arabirimi yalnızca Microsoft Edge ve Google Chrome web tarayıcılarında desteklenmektedir. 
1. [Azure Portal](https://portal.azure.com/)’da oturum açın. 
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
1. Panoda şu kutucuğu ve üzerinde şu durumu görürsünüz: **Veri fabrikası dağıtılıyor**. 

    ![veri fabrikası dağıtılıyor kutucuğu](media/tutorial-create-azure-ssis-runtime-portal/deploying-data-factory.png)

1. Oluşturma işlemi tamamlandıktan sonra, resimde gösterildiği gibi **Data Factory** sayfasını görürsünüz. 

    ![Data factory giriş sayfası](./media/tutorial-create-azure-ssis-runtime-portal/data-factory-home-page.png)

1. Azure Data Factory Kullanıcı Arabirimini (UI) ayrı bir sekmede açmak için **Geliştir ve İzle**’ye tıklayın. 

### <a name="provision-an-azure-ssis-integration-runtime"></a>Azure SSIS tümleştirme çalışma zamanı sağlama 
1. Başlarken sayfasında **SSIS Tümleştirme Çalışma Zamanı Yapılandır** kutucuğuna tıklayın. 

   ![SSIS Tümleştirme Çalışma Zamanı Yapılandır kutucuğu](./media/tutorial-create-azure-ssis-runtime-portal/configure-ssis-integration-runtime-tile.png)

1. **Tümleştirme Çalışma Zamanı Kurulumu**’nun **Genel Ayarlar** sayfasında aşağıdaki adımları tamamlayın: 

   ![Genel ayarlar](./media/tutorial-create-azure-ssis-runtime-portal/general-settings.png)

    a. **Name** (Ad) alanına tümleştirme çalışma zamanınızın adını girin. 

    b. **Description** (Açıklama) alanına tümleştirme çalışma zamanınız için bir açıklama girin. 

    c. **Location** (Konum)alanında tümleştirme çalışma zamanınızın konumunu seçin. Yalnızca desteklenen konumlar görüntülenir. SSISDB'yi barındırmak için veritabanı sunucunuz ile aynı konumu seçmenizi öneririz. 

    d. **Node Size** (Düğüm Boyutu) alanında tümleştirme çalışma zamanınızdaki düğümün boyutunu seçin. Yalnızca desteklenen düğüm boyutları görüntülenir. Yoğun işlemci/bellek kullanan çok sayıda paket çalıştırmak istiyorsanız büyük bir düğüm boyutu seçin. 

    e. **Node Number** (Düğüm Sayısı) alanında tümleştirme çalışma zamanınızdaki düğüm sayısını seçin. Yalnızca desteklenen düğüm sayıları görüntülenir. Aynı anda birden fazla paket çalıştırmak istiyorsanız çok sayıda düğüme sahip bir küme seçin (ölçeklendirme için). 

    f. **Edition/License** (Sürüm/Lisans) alanında tümleştirme çalışma zamanınız için SQL Server sürümünü/lisansını seçin: Standard veya Enterprise. Tümleştirme çalışma zamanınızda gelişmiş/premium özellikleri kullanmak istiyorsanız Enterprise sürümünü seçin. 

    g. **Save Money** (Tasarruf Edin) alanında tümleştirme çalışma zamanınız için Azure Hibrit Avantajı (AHB) seçeneğini ayarlayın: Evet veya Hayır. Karma kullanımla maliyet tasarrufu yapmak üzere Yazılım Güvencesi ile kendi SQL Server lisansınızı getirmek istiyorsanız Evet'i seçin. 

    h. **İleri**’ye tıklayın. 

1. **SQL Ayarları** sayfasında aşağıdaki adımları tamamlayın: 

   ![SQL ayarları](./media/tutorial-create-azure-ssis-runtime-portal/sql-settings.png)

    a. **Subscription** (Abonelik) alanında SSISDB'yi barındıran Azure aboneliğini seçin. 

    b. **Location** (Konum) alanında SSISDB'yi barındıran veritabanı sunucunuzun konumunu seçin. Tümleştirme çalışma zamanınızla aynı konumu seçmenizi öneririz. 

    c. **Catalog Database Server Endpoint** (Katalog Veritabanı Sunucusu Uç Noktası) alanında SSISDB'yi barındıracak veritabanı sunucunuzun uç noktasını seçin. Seçilen veritabanı sunucusuna göre SSISDB sizin adınıza tek veritabanı, elastik havuzun bir parçası veya Yönetilen Örnek biçiminde oluşturulabilir ve genel ağ üzerinden veya sanal ağa eklenerek erişilebilir. 

    d. Üzerinde **kullanım AAD kimlik doğrulaması...**  onay kutusunu SSISDB'yi barındırmak için veritabanı sunucusu için kimlik doğrulama yöntemini seçin: SQL veya Azure Active Directory (AAD), Azure Data Factory ile yönetilen Azure kaynakları için kimliği. Kaydetmeden gerekirse, Data Factory MSI veritabanı sunucusuna erişim izni olan bir AAD gruba eklemek için bkz: [etkinleştirme AAD kimlik doğrulaması için Azure-SSIS IR](https://docs.microsoft.com/en-us/azure/data-factory/enable-aad-authentication-azure-ssis-ir). 

    e. **Admin Username** (Yönetici Kullanıcı Adı) alanına SSISDB'yi barındıracak veritabanı sunucunuzun SQL kimlik doğrulaması kullanıcı adını girin. 

    f. **Admin Password** (Yönetici Parolası) alanına SSISDB'yi barındıracak veritabanı sunucunuzun SQL kimlik doğrulaması parolasını girin. 

    g. **Catalog Database Service Tier** (Katalog Veritabanı Hizmet Katmanı) alanında SSISDB'yi barındıracak veritabanı sunucunuzun hizmet katmanını seçin: Temel/Standart/Premium katman veya elastik havuz adı. 

    h. **Test Connection** (Bağlantıyı Sına) öğesine tıklayın ve başarılı olursa **Next** (İleri) öğesine tıklayın. 

1.  **Advanced Settings** (Gelişmiş Ayarlar) sayfasında aşağıdaki adımları tamamlayın: 

    ![Gelişmiş ayarlar](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings.png)

    a. **Maximum Parallel Executions Per Node** (Düğüm Başına Maksimum Paralel Yürütme) alanında tümleştirme çalışma zamanınızda her düğümde eş zamanlı olarak yürütülecek maksimum paket sayısını seçin. Yalnızca desteklenen paket sayıları görüntülenir. İşlem/bellek tek bir büyük/ağır paketini çalıştırmak için birden fazla çekirdeği kullanmak istiyorsanız, düşük bir sayı seçin-yoğun. Tek bir çekirdekte bir veya daha fazla küçük/hafif paket çalıştırmak istiyorsanız daha yüksek bir sayı seçin. 

    b. **Custom Setup Container SAS URI** (Özel Kurulum Kapsayıcısı SAS URI'si) alanında isteğe bağlı olarak kurulum betiğinizin ve ilgili dosyalarının depolandığı Azure Depolama Blobu kapsayıcısının Paylaşılan Erişim İmzası (SAS) Tekdüzen Kaynak Tanımlayıcısı (URI) değerini girebilirsiniz, bkz. [Azure-SSIS IR için özel kurulum](https://docs.microsoft.com/en-us/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup). 

1. Üzerinde **bir sanal ağ seçin...**  onay kutusunu, tümleştirme çalışma zamanı bir sanal ağa katılmak isteyip istemediğinizi belirtin. Sanal ağ hizmet uç noktaları/yönetilen örnek SSISDB'yi barındırmak için ile Azure SQL veritabanı'nı kullanma veya şirket içi verilere erişim gerektiren kontrol edin; diğer bir deyişle, şirket içi veri kaynaklarınız/hedeflerini SSIS paketlerinizi bkz [katılın Azure-SSIS IR'yi bir sanal ağa](https://docs.microsoft.com/en-us/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network). Bunu işaretlerseniz, aşağıdaki adımları tamamlayın: 

   ![Sanal ağ ile Gelişmiş ayarlar](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings-vnet.png)

    a. İçin **abonelik**, sanal ağınıza sahip Azure aboneliğini seçin. 

    b. İçin **konumu**, Integration runtime'nın aynı konum seçilir. 

    c. İçin **türü**, sanal ağınızı türünü seçin: Klasik veya Azure Resource Manager. Öneririz Azure Resource Manager sanal ağı seçin, sonra Klasik sanal ağ yakında kullanımdan kaldırılacak. 

    d. İçin **VNet adı**, sanal ağınızın adını seçin. Bu sanal ağ, şirket içi ağınıza bağlı ve SSISDB barındırmak için sanal ağ hizmet uç noktaları/yönetilen ile örneği Azure SQL veritabanı için kullanılan aynı sanal ağda olmalıdır. 

    e. İçin **alt ağ adı**, sanal ağınızın alt ağ adını seçin. Bu yönetilen örneği için SSISDB'yi barındırmak için kullanılan farklı bir alt olmalıdır. 

1. Tıklayın **VNet doğrulama** ve başarılı olursa, tıklayın **son** , Azure-SSIS tümleştirme çalışma zamanının oluşturulmasını başlatmak için. 

    > [!IMPORTANT]
    > - Bu işlem yaklaşık 20-30 dakikada tamamlanan
    > - Data Factory hizmeti, SSIS Kataloğu veritabanını (SSISDB) hazırlamak için Azure SQL Veritabanınıza bağlanır. Betik ayrıca belirtilmişse sanal ağınıza ilişkin izin ve ayarları yapılandırır ve yeni bir Azure-SSIS tümleştirme çalışma zamanı örneğini sanal ağa katılır. 

1. **Bağlantılar** penceresinde, gerekirse **Tümleştirme Çalışma Zamanları**’na geçin. Durumu yenilemek için **Yenile**’ye tıklayın. 

   ![Oluşturma durumu](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-creation-status.png)

1. Altındaki bağlantıları kullanın **eylemleri** durdurmak/başlatmak, düzenlemek veya Integration runtime'ı silmek için sütun. Tümleştirme çalışma zamanının JSON kodunu görüntülemek için son bağlantıyı kullanın. Düzenle ve sil düğmeleri yalnızca IR durdurulmuş durumdayken etkin olur. 

   ![Azure SSIS IR - eylemler](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-actions.png)

### <a name="azure-ssis-integration-runtimes-in-the-portal"></a>Portalda Azure SSIS tümleştirmesi çalışma zamanları
1. Veri fabrikanızdaki mevcut tümleştirme çalışma zamanlarını görüntülemek için Azure Data Factory kullanıcı arabiriminde **Düzenle** sekmesine geçin, **Bağlantılar**’a tıklayın ve sonra **Tümleştirme Çalışma Zamanları** sekmesine geçin. 

   ![Mevcut Ir'leri görüntüle](./media/tutorial-create-azure-ssis-runtime-portal/view-azure-ssis-integration-runtimes.png)

1. Yeni bir Azure-SSIS IR oluşturmak için **Yeni**'ye tıklayın. 

   ![Menü aracılığıyla tümleştirme çalışma zamanı](./media/tutorial-create-azure-ssis-runtime-portal/edit-connections-new-integration-runtime-button.png)

1. Azure-SSIS tümleştirme çalışma zamanı oluşturmak için resimde gösterildiği gibi **Yeni**’ye tıklayın. 
1. Tümleştirme Çalışma Zamanı Kurulumu penceresinde **Mevcut SSIS paketlerini Azure’da yürütmek üzere lift-and-shift ile taşıma**’yı seçip **İleri**’ye tıklayın. 

   ![Tümleştirme çalışma zamanının türünü belirtin](./media/tutorial-create-azure-ssis-runtime-portal/integration-runtime-setup-options.png)

1. Geriye kalan Azure SSIS IR kurulumu adımları için bkz. [Azure SSIS tümleştirme çalışma zamanı sağlama](#provision-an-azure-ssis-integration-runtime). 

## <a name="azure-powershell"></a>Azure PowerShell
Bu bölümde, bir Azure-SSIS IR oluşturmak için Azure PowerShell kullanma

### <a name="create-variables"></a>Değişken oluşturma
Bu öğreticideki betiklerde kullanılacak değişkenleri tanımlayın:

```powershell
### Azure Data Factory information 
# If your input contains a PSH special character, e.g. "$", precede it with the escape character "`" like "`$".
$SubscriptionName = "[your Azure subscription name]"
$ResourceGroupName = "[your Azure resource group name]"
$DataFactoryName = "[your data factory name]"
$DataFactoryLocation = "EastUS" 

### Azure-SSIS integration runtime information - This is the Data Factory compute resource for running SSIS packages
$AzureSSISName = "[specify a name for your Azure-SSIS IR]"
$AzureSSISDescription = "[specify a description for your Azure-SSIS IR]"
$AzureSSISLocation = "EastUS" 
# Only Standard_A4_v2|Standard_A8_v2|Standard_D1_v2|Standard_D2_v2|Standard_D3_v2|Standard_D4_v2 are supported.
$AzureSSISNodeSize = "Standard_D4_v2"
# Only 1-10 nodes are supported.
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
$VnetId = "[your virtual network resource ID or leave it empty]" # REQUIRED if you use Azure SQL Database with virtual network service endpoints/Managed Instance/on-premises data, Azure Resource Manager virtual network is recommended, Classic virtual network will be deprecated soon    
$SubnetName = "[your subnet name or leave it empty]" # WARNING: Please use a different subnet than the one used for your Managed Instance

### SSISDB info
$SSISDBServerEndpoint = "[your Azure SQL Database server name or Managed Instance name.DNS prefix].database.windows.net" # WARNING: Please ensure that there is no existing SSISDB, so we can prepare and manage one on your behalf
# Authentication info: SQL or Azure Active Directory (AAD)
$SSISDBServerAdminUserName = "[your server admin username for SQL authentication or leave it empty for AAD authentication]"
$SSISDBServerAdminPassword = "[your server admin password for SQL authentication or leave it empty for AAD authentication]"
$SSISDBPricingTier = "[Basic|S0|S1|S2|S3|S4|S6|S7|S9|S12|P1|P2|P4|P6|P11|P15|…|ELASTIC_POOL(name = <elastic_pool_name>) for Azure SQL Database or leave it empty for Managed Instance]"
```

### <a name="log-in-and-select-subscription"></a>Oturum açma ve abonelik seçme
Komut dosyasının oturum açıp Azure aboneliğinizi seçmek için aşağıdaki kodu ekleyin: 

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
Azure'da SSIS paketlerini çalıştıran bir Azure-SSIS tümleştirme çalışma zamanı oluşturmak için aşağıdaki komutları çalıştırın. 

Siz değil ile sanal ağ hizmet uç noktaları/yönetilen örnek SSISDB'yi barındırmak için Azure SQL veritabanı'nı kullanın ya da şirket içi verilere erişim gerektiren Vnetıd ve alt ağ parametrelerini atlayın veya boş değerler geçirileceğini. Aksi takdirde, olamaz bunları çıkarın ve gerekir geçerli değerler, sanal ağ yapılandırmasından geçmek için bkz: [katılın Azure-SSIS IR'yi bir sanal ağa](https://docs.microsoft.com/en-us/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network). 

SSISDB'yi barındırmak için yönetilen örneği kullanıyorsanız, CatalogPricingTier parametreyi atlarsanız veya bunun için boş bir değer geçirin. Aksi takdirde, olamaz atlayın ve gerekir geçerli bir değer listesinden fiyatlandırma katmanları Azure SQL veritabanı için desteklenen geçmek için bkz: [SQL veritabanı kaynak limitleri](../sql-database/sql-database-resource-limits.md). 

Kullanıyorsanız, Azure Data Factory ile Azure Active Directory (AAD) kimlik doğrulama kimlik veritabanı sunucusuna bağlanmak Azure kaynakları için yönetilen, CatalogAdminCredential parametreyi atlayabilirsiniz, ancak, Data Factory MSI ile AAD grubu içine eklemeniz gerekir erişim izinlerini veritabanı sunucusu için bkz: [etkinleştirme AAD kimlik doğrulaması için Azure-SSIS IR](https://docs.microsoft.com/en-us/azure/data-factory/enable-aad-authentication-azure-ssis-ir). Aksi takdirde, atlayamazsınız ve Sunucu Yöneticisi kullanıcı adı ve parola SQL kimlik doğrulaması için oluşturulmuş geçerli bir nesne geçmesi gerekir.

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
Bir Azure-SSIS tümleştirme çalışma zamanı oluşturur tam komut dosyası aşağıda verilmiştir. 

```powershell
### Azure Data Factory information 
# If your input contains a PSH special character, e.g. "$", precede it with the escape character "`" like "`$".
$SubscriptionName = "[your Azure subscription name]"
$ResourceGroupName = "[your Azure resource group name]"
$DataFactoryName = "[your data factory name]"
$DataFactoryLocation = "EastUS" 

### Azure-SSIS integration runtime information - This is the Data Factory compute resource for running SSIS packages
$AzureSSISName = "[specify a name for your Azure-SSIS IR]"
$AzureSSISDescription = "[specify a description for your Azure-SSIS IR]"
$AzureSSISLocation = "EastUS" 
# Only Standard_A4_v2|Standard_A8_v2|Standard_D1_v2|Standard_D2_v2|Standard_D3_v2|Standard_D4_v2 are supported.
$AzureSSISNodeSize = "Standard_D4_v2"
# Only 1-10 nodes are supported.
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
$VnetId = "[your virtual network resource ID or leave it empty]" # REQUIRED if you use Azure SQL Database with virtual network service endpoints/Managed Instance/on-premises data, Azure Resource Manager virtual network is recommended, Classic virtual network will be deprecated soon    
$SubnetName = "[your subnet name or leave it empty]" # WARNING: Please use a different subnet than the one used for your Managed Instance

### SSISDB info
$SSISDBServerEndpoint = "[your Azure SQL Database server name or Managed Instance name.DNS prefix].database.windows.net" # WARNING: Please ensure that there is no existing SSISDB, so we can prepare and manage one on your behalf
# Authentication info: SQL or Azure Active Directory (AAD)
$SSISDBServerAdminUserName = "[your server admin username for SQL authentication or leave it empty for AAD authentication]"
$SSISDBServerAdminPassword = "[your server admin password for SQL authentication or leave it empty for AAD authentication]"
$SSISDBPricingTier = "[Basic|S0|S1|S2|S3|S4|S6|S7|S9|S12|P1|P2|P4|P6|P11|P15|…|ELASTIC_POOL(name = <elastic_pool_name>) for Azure SQL Database or leave it empty for Managed Instance]"

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
Bu bölümde, Azure-SSIS tümleştirme çalışma zamanı oluşturmak için Azure Resource Manager şablonu kullanın. İzlenecek örnek yol şu şekildedir: 

1. Aşağıdaki Azure Resource Manager şablonu ile bir JSON dosyası oluşturun. Açılı ayraçlar (yer tutucu) değerleri kendi değerlerinizle değiştirin. 

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
    
1. Azure Resource Manager şablonu dağıtmak için burada ADFTutorialResourceGroup kaynak grubunuzun adını ve C:\adftutorial içeren JSON dosyasını aşağıdaki örnekte gösterildiği gibi New-AzureRmResourceGroupDeployment komutu çalıştırın. data factory ve Azure-SSIS IR'yi tanımı 

    ```powershell
    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json
    ```

    Bu komut, data factory ve Azure-SSIS IR içinde oluşturur, ancak IR başlamıyor 

1. Azure-SSIS IR başlatmak için başlangıç AzureRmDataFactoryV2IntegrationRuntime komutu çalıştırın: 

    ```powershell
    Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName "<Resource Group Name>" `
                                                 -DataFactoryName "<Data Factory Name>" `
                                                 -Name "<Azure SSIS IR Name>" `
                                                 -Force
    ``` 

## <a name="deploy-ssis-packages"></a>SSIS paketlerini dağıtma
Şimdi SQL Server Veri Araçları (SSDT) veya SQL Server Management Studio’yu (SSMS) kullanarak SSIS paketlerinizi Azure’a dağıtın. SSIS kataloğunu (SSISDB) barındıran veritabanı sunucunuza bağlanın. Veritabanı sunucusunun adı şu biçimdedir: &lt;Azure SQL veritabanı sunucu adı&gt;. database.windows.net veya &lt;yönetilen örnek adı. DNS ön eki&gt;. database.windows.net. Yönergeler için [Paket dağıtma](/sql/integration-services/packages/deploy-integration-services-ssis-projects-and-packages#deploy-packages-to-integration-services-server) makalesine bakın. 

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede diğer Azure-SSIS IR konulara bakın: 

- [Azure-SSIS tümleştirme çalışma zamanı](concepts-integration-runtime.md#azure-ssis-integration-runtime). Bu makalede Azure-SSIS IR'yi genel dahil tümleştirme çalışma zamanları hakkında kavramsal bilgiler sağlar 
- [Öğretici: SSIS paketlerini Azure’a dağıtma](tutorial-create-azure-ssis-runtime-portal.md). Bu makale bir Azure-SSIS IR oluşturmaya ilişkin adım adım yönergeler sağlar ve SSIS kataloğunu barındırmak için bir Azure SQL veritabanı kullanır. 
- [Azure-SSIS IR’yi izleme](monitor-integration-runtime.md#azure-ssis-integration-runtime). Bu makalede bir Azure-SSIS IR ile ilgili bilgileri ve döndürülen bilgilerdeki durumların açıklamalarını alma işlemi gösterilmektedir. 
- [Azure-SSIS IR’yi yönetme](manage-azure-ssis-integration-runtime.md). Bu makale bir Azure-SSIS IR’yi durdurma, başlatma veya kaldırma işlemini gösterir. Ayrıca, IR’ye daha fazla düğüm ekleyerek Azure-SSIS IR’nizi ölçeklendirmeyi gösterir. 
- [Azure-SSIS IR’yi bir sanal ağa ekleyin](join-azure-ssis-integration-runtime-virtual-network.md). Bu makalede, Azure-SSIS IR, Azure sanal ağına birleştirme hakkında kavramsal bilgiler verilmektedir. Ayrıca, Azure portalını kullanarak Azure-SSIS IR’nin sanal ağa katılmasını sağlayacak şekilde sanal ağı yapılandırma adımlarını da sunar. 
