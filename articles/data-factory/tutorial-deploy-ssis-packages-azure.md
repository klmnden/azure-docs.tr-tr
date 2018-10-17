---
title: Azure-SSIS Tümleştirme Çalışma Zamanı Sağlama | Microsoft Docs
description: Azure'da SSIS paketleri dağıtıp çalıştırabilmek için Azure Data Factory'de Azure-SSIS tümleştirme çalışma zamanı sağlamayı öğrenin.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: tutorial
ms.date: 09/23/2018
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: 20c12891d629cb3e3f7f3c0e0013dffc28b14c63
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2018
ms.locfileid: "48248680"
---
# <a name="provision-the-azure-ssis-integration-runtime-in-azure-data-factory"></a>Azure Data Factory'de Azure-SSIS Tümleştirme Çalışma Zamanı Sağlama
Bu öğretici, Azure portalını kullanarak Azure Data Factory’de bir Azure-SSIS tümleştirme çalışma zamanı (IR) sağlama adımlarını sunar. Daha sonra, SQL Server Veri Araçları veya SQL Server Management Studio'yu kullanarak Azure'da bu çalışma zamanında SQL Server Integration Services (SSIS) paketleri dağıtabilir ve çalıştırabilirsiniz. Azure-SSIS IR’ler hakkında kavramsal bilgiler için bkz. [Azure SSIS tümleştirme çalışma zamanına genel bakış](concepts-integration-runtime.md#azure-ssis-integration-runtime).

Bu öğreticide, aşağıdaki adımları tamamlayacaksınız:

> [!div class="checklist"]
> * Veri fabrikası oluşturma.
> * Azure-SSIS tümleştirme çalışma zamanı sağlama.

## <a name="prerequisites"></a>Ön koşullar
- **Azure aboneliği**. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun. 
- **Azure SQL Veritabanı sunucusu**. Henüz bir veritabanı sunucunuz yoksa, başlamadan önce Azure portalında bir tane oluşturun. Azure Data Factory, bu veritabanı sunucusunda SSIS Kataloğu (SSISDB veritabanı) oluşturur. Veritabanı sunucusunu tümleştirme çalışma zamanı ile aynı Azure bölgesinde oluşturmanız önerilir. Bu yapılandırma, tümleştirme çalışma zamanının Azure bölgelerinden geçmeden SSISDB veritabanına yürütme günlüklerini yazmasına olanak tanır. 
- Seçilen veritabanı sunucusuna göre SSISDB sizin adınıza tek veritabanı, elastik havuzun bir parçası veya Yönetilen Örnek biçiminde oluşturulabilir ve genel ağ üzerinden veya sanal ağa eklenerek erişilebilir. SSISDB barındırmak için Azure SQL Veritabanı'nı sanal ağ hizmet uç noktaları/Yönetilen Örnek ile kullanırsanız veya şirket içi verilere erişmeniz gerekiyorsa Azure-SSIS IR örneğinizi bir sanal ağa eklemeniz gerekir, bkz. [Sanal ağda Azure-SSIS IR oluşturma](https://docs.microsoft.com/en-us/azure/data-factory/create-azure-ssis-integration-runtime). 
- Veritabanı sunucusunda **Azure hizmetlerine erişime izin ver** ayarının etkin olduğundan emin olun. Bu durum SSISDB barındırmak için Azure SQL Veritabanı'nı sanal ağ hizmet uç noktaları/Yönetilen Örnek ile birlikte kullandığınızda geçerli değildir. Daha fazla bilgi için bkz. [Azure SQL veritabanınızın güvenliğini sağlama](../sql-database/sql-database-security-tutorial.md#create-a-server-level-firewall-rule-in-the-azure-portal). Bu ayarı PowerShell kullanarak etkinleştirmek için bkz. [New-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule?view=azurermps-4.4.1). 
- Veritabanı sunucusunun güvenlik duvarı ayarlarındaki istemci IP adresi listesine istemci makinenin IP adresini veya istemci makinenin IP adresini içeren IP adresi aralığını ekleyin. Daha fazla bilgi için bkz. [Azure SQL Veritabanı'nda sunucu düzeyinde ve veritabanı düzeyinde güvenlik duvarı kuralları yapılandırma](../sql-database/sql-database-firewall-configure.md). 
- Veritabanı sunucusuna bağlanmak için sunucu yöneticisi kimlik bilgilerinizle SQL kimlik doğrulamasını veya Azure Data Factory (ADF) Azure kaynakları için yönetilen kimlik bilgilerinizle Azure Active Directory (AAD) kimlik doğrulamasını kullanabilirsiniz.  İkinci seçenek için ADF MSI bilgilerinizi veritabanı sunucusuna erişim izni olan bir AAD grubuna eklemeniz gerekir, bkz. [AAD kimlik doğrulaması ile Azure-SSIS IR oluşturma](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime). 
- Azure SQL Veritabanı sunucunuzun SSIS Kataloğuna (SSISDB veritabanı) sahip olmadığını doğrulayın. Azure-SSIS IR’nin sağlanması, mevcut bir SSIS Kataloğunun kullanılmasını desteklemez. 

> [!NOTE]
> - Data Factory'nin kullanılabileceği Azure bölgelerinin bir listesi için bir sonraki sayfada ilgilendiğiniz bölgeleri seçin ve **Analytics**'i genişleterek **Data Factory**: [Products available by region](https://azure.microsoft.com/global-infrastructure/services/) (Bölgeye göre kullanılabilir durumdaki ürünler) bölümünü bulun. 
> - Azure SSIS Tümleştirme Çalışma Zamanı'nın kullanılabileceği Azure bölgelerinin bir listesi için bir sonraki sayfada ilgilendiğiniz bölgeleri seçin ve **Analytics**'i genişleterek **SSIS Tümleştirme Çalışma Zamanı**: [Products available by region](https://azure.microsoft.com/global-infrastructure/services/) (Bölgeye göre kullanılabilir durumdaki ürünler) bölümünü bulun. 

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

1. **Microsoft Edge** veya **Google Chrome** web tarayıcısını açın. Şu anda Data Factory kullanıcı arabirimi yalnızca Microsoft Edge ve Google Chrome web tarayıcılarında desteklenmektedir. 
1. [Azure Portal](https://portal.azure.com/) oturum açın. 
1. Soldaki menüden **Yeni**’yi, sonra **Veri ve Analiz**’i ve ardından **Data Factory**’i seçin. 

   ![“Yeni” bölmesinde Data Factory seçimi](./media/tutorial-create-azure-ssis-runtime-portal/new-data-factory-menu.png)

1. **Yeni veri fabrikası** sayfasında **ad** altında **MyAzureSsisDataFactory** adını girin. 

   ![“Yeni veri fabrikası” sayfası](./media/tutorial-create-azure-ssis-runtime-portal/new-azure-data-factory.png)

   Azure data factory adı *küresel olarak benzersiz* olmalıdır. Aşağıdaki hatayı alırsanız veri fabrikasının adını değiştirin (örneğin **&lt;adınız&gt;MyAzureSsisDataFactory**) ve yeniden oluşturmayı deneyin. Data Factory yapıtlarının adlandırma kuralları için [Data Factory - adlandırma kuralları](naming-rules.md) makalesini inceleyin. 

   `Data factory name “MyAzureSsisDataFactory” is not available`

1. **Abonelik** için, veri fabrikasını oluşturmak istediğiniz Azure aboneliğini seçin. 
1. **Kaynak Grubu** için aşağıdaki adımlardan birini uygulayın: 

   - **Var olanı kullan**’ı ve ardından listeden var olan bir kaynak grubunu seçin. 
   - **Yeni oluştur**’u seçin ve bir kaynak grubunun adını girin. 

   Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/resource-group-overview.md). 
1. **Sürüm** için **V2 (Önizleme)** öğesini seçin. 
1. **Konum** için, veri fabrikasının konumunu seçin. Bu listede yalnızca veri fabrikası oluşturma için desteklenen konumlar gösterilir. 
1. **Panoya sabitle**’yi seçin. 
1. **Oluştur**’u seçin. 
1. Panoda, **Veri fabrikası dağıtılıyor** durumuna sahip aşağıdaki kutucuğu görürsünüz: 

   ![“Data Factory Dağıtılıyor” kutucuğu](media/tutorial-create-azure-ssis-runtime-portal/deploying-data-factory.png)

1. Oluşturma işlemi tamamlandıktan sonra, **Veri fabrikası** sayfasını görürsünüz. 

   ![Veri fabrikasının giriş sayfası](./media/tutorial-create-azure-ssis-runtime-portal/data-factory-home-page.png)

1. Data Factory kullanıcı arabirimini (UI) ayrı bir sekmede açmak için **Yazar ve İzleyici**’yi seçin. 

## <a name="provision-an-azure-ssis-integration-runtime"></a>Azure-SSIS tümleştirme çalışma zamanı sağlama

1. **Başlarken** sayfasında **SSIS Tümleştirme Çalışma Zamanı Yapılandır** kutucuğunu seçin. 

   !["SSIS Tümleştirme Çalışma Zamanı Yapılandır" kutucuğu](./media/tutorial-create-azure-ssis-runtime-portal/configure-ssis-integration-runtime-tile.png)

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

   c. **Catalog Database Server Endpoint** (Katalog Veritabanı Sunucusu Uç Noktası) alanında SSISDB'yi barındıracak veritabanı sunucunuzun uç noktasını seçin. Seçilen veritabanı sunucusuna göre SSISDB sizin adınıza tek başına veritabanı, elastik havuzun bir parçası veya Yönetilen Örnek biçiminde oluşturulabilir ve genel ağ üzerinden veya sanal ağa eklenerek erişilebilir. SSISDB hizmetini barındıracak veritabanı sunucusu türünü seçme konusunda yardım almak için bkz. [SQL Veritabanı mantıksal sunucusu ile Yönetilen Örneği Karşılaştırma](../data-factory/create-azure-ssis-integration-runtime.md#compare-sql-database-logical-server-and-sql-database-managed-instance). SSISDB barındırmak için Azure SQL Veritabanı'nı sanal ağ hizmet uç noktaları/Yönetilen Örnek ile birlikte seçerseniz veya şirket içi verilere erişmeniz gerekiyorsa Azure-SSIS IR örneğinizi bir sanal ağa eklemeniz gerekir. Bkz. [Sanal ağda Azure-SSIS IR oluşturma](https://docs.microsoft.com/en-us/azure/data-factory/create-azure-ssis-integration-runtime). 

   d. **Use AAD authentication...** (AAD kimlik doğrulaması kullan...) onay kutusunda SSISDB'yi barındıracak veritabanı sunucunuz için kimlik doğrulama yöntemini belirleyin: SQL veya Azure Active Directory (AAD) ve Azure Data Factory (ADF) Azure kaynakları için yönetilen kimlik bilgileriniz. Bu seçeneği işaretlerseniz ADF MSI bilgilerinizi veritabanı sunucusuna erişim izni olan bir AAD grubuna eklemeniz gerekir, bkz. [AAD kimlik doğrulaması ile Azure-SSIS IR oluşturma](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime). 

   e. **Admin Username** (Yönetici Kullanıcı Adı) alanına SSISDB'yi barındıracak veritabanı sunucunuzun SQL kimlik doğrulaması kullanıcı adını girin. 

   f. **Admin Password** (Yönetici Parolası) alanına SSISDB'yi barındıracak veritabanı sunucunuzun SQL kimlik doğrulaması parolasını girin. 

   g. **Catalog Database Service Tier** (Katalog Veritabanı Hizmet Katmanı) alanında SSISDB'yi barındıracak veritabanı sunucunuzun hizmet katmanını seçin: Temel/Standart/Premium katman veya elastik havuz adı. 

   h. **Test Connection** (Bağlantıyı Sına) öğesine tıklayın ve başarılı olursa **Next** (İleri) öğesine tıklayın. 

1. **Advanced Settings** (Gelişmiş Ayarlar) sayfasında aşağıdaki adımları tamamlayın: 

   ![Gelişmiş ayarlar](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings.png)

   a. **Maximum Parallel Executions Per Node** (Düğüm Başına Maksimum Paralel Yürütme) alanında tümleştirme çalışma zamanınızda her düğümde eş zamanlı olarak yürütülecek maksimum paket sayısını seçin. Yalnızca desteklenen paket sayıları görüntülenir. Yoğun işlemci/bellek kullanan tek bir büyük/ağır paket çalıştırmak için birden fazla çekirdek kullanmak istiyorsanız düşük bir sayı seçin. Tek bir çekirdekte bir veya daha fazla küçük/hafif paket çalıştırmak istiyorsanız daha yüksek bir sayı seçin. 

   b. **Custom Setup Container SAS URI** (Özel Kurulum Kapsayıcısı SAS URI'si) alanında isteğe bağlı olarak kurulum betiğinizin ve ilgili dosyalarının depolandığı Azure Depolama Blobu kapsayıcısının Paylaşılan Erişim İmzası (SAS) Tekdüzen Kaynak Tanımlayıcısı (URI) değerini girebilirsiniz, bkz. [Azure-SSIS IR için özel kurulum](https://docs.microsoft.com/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup). 

   c. **Select a VNet...** (VNet seçin...) onay kutusunda tümleştirme çalışma zamanınızı sanal ağa eklemek isteyip istemediğinizi seçin. SSISDB barındırmak için Azure SQL Veritabanı'nı sanal ağ hizmet uç noktaları/Yönetilen Örnek ile kullanmanız veya şirket içi verilere erişmeniz gerekip gerekmediğini kontrol etmeniz gerekir, bkz. [Sanal ağda Azure-SSIS IR oluşturma](https://docs.microsoft.com/en-us/azure/data-factory/create-azure-ssis-integration-runtime). 

1. Tümleştirme çalışma zamanı oluşturma işlemini başlatmak için **Finish** (Son) öğesine tıklayın. 

   > [!IMPORTANT]
   > Bu işlemin tamamlanması yaklaşık 20-30 dakika sürer.
   >
   > Data Factory hizmeti, SSIS Kataloğunu (SSISDB veritabanı) hazırlamak için Azure SQL Veritabanı sunucunuza bağlanır. 
   > 
   > Bir Azure-SSIS IR örneğini sağladığınızda, SSIS için Azure Feature Pack ve Access Redistributable da yüklenir. Bu bileşenler, Excel ile Access dosyalarıyla ve yerleşik bileşenler tarafından desteklenen veri kaynaklarına ek olarak çeşitli Azure veri kaynaklarıyla bağlantı kurma olanağı sunar. Ek bileşenleri de yükleyebilirsiniz. Daha fazla bilgi için bkz. [Azure-SSIS tümleştirmesi çalışma zamanı için özel kurulum](how-to-configure-azure-ssis-ir-custom-setup.md). 

1. **Bağlantılar** sekmesinde, gerekirse **Tümleştirme Çalışma Zamanları**’na geçin. Durumu yenilemek için **Yenile**’yi seçin. 

   !["Yenile" düğmesi ile oluşturma durumu](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-creation-status.png)

1. Tümleştirme çalışma zamanını durdurmak/başlatmak, düzenlemek veya silmek için **Eylemler** sütunundaki bağlantıları kullanın. Tümleştirme çalışma zamanının JSON kodunu görüntülemek için son bağlantıyı kullanın. Düzenle ve sil düğmeleri yalnızca IR durdurulmuş durumdayken etkin olur. 

   !["Eylemler" sütunundaki bağlantılar](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-actions.png) 

## <a name="create-an-azure-ssis-integration-runtime"></a>Azure SSIS tümleştirme çalışma zamanı oluşturma

1. Veri fabrikanızdaki mevcut tümleştirme çalışma zamanlarını görüntülemek için Azure Data Factory kullanıcı arabiriminde **Düzenle** sekmesine geçin, **Bağlantılar**’ı seçin ve sonra **Tümleştirme Çalışma Zamanları** sekmesine geçin. 

   ![Var olan IR’leri görüntüleme seçimleri](./media/tutorial-create-azure-ssis-runtime-portal/view-azure-ssis-integration-runtimes.png)

1. Bir Azure-SSIS IR oluşturmak için **Yeni**'yi seçin. 

   ![Menü aracılığıyla tümleştirme çalışma zamanı](./media/tutorial-create-azure-ssis-runtime-portal/edit-connections-new-integration-runtime-button.png)

1. **Tümleştirme Çalışma Zamanı Kurulumu** penceresinde **Mevcut SSIS paketlerini Azure’da yürütmek üzere lift-and-shift ile taşıma**’yı seçip **İleri**’yi seçin. 

   ![Tümleştirme çalışma zamanının türünü belirtin](./media/tutorial-create-azure-ssis-runtime-portal/integration-runtime-setup-options.png)

1. Azure SSIS IR kurulumunun diğer adımları için [Azure SSIS tümleştirme çalışma zamanı sağlama](#provision-an-azure-ssis-integration-runtime) bölümüne bakın. 

## <a name="deploy-ssis-packages"></a>SSIS paketlerini dağıtma
Şimdi SQL Server Veri Araçları (SSDT) veya SQL Server Management Studio’yu (SSMS) kullanarak SSIS paketlerinizi Azure’a dağıtın. SSIS Kataloğunu (SSISDB veritabanı) barındıran Azure SQL Veritabanı sunucunuza bağlanın. Azure SQL Veritabanı sunucusunun adı şu biçimdedir: `<servername>.database.windows.net`. 

SSIS belgelerinde aşağıdaki makalelere bakın: 

- [Azure’da bir SSIS paketi dağıtma, çalıştırma ve izleme](/sql/integration-services/lift-shift/ssis-azure-deploy-run-monitor-tutorial) 
- [Azure’da SSIS Kataloğuna bağlanma](/sql/integration-services/lift-shift/ssis-azure-connect-to-catalog-database) 
- [Azure’da paket yürütmeyi zamanlama](/sql/integration-services/lift-shift/ssis-azure-schedule-packages) 
- [Windows kimlik doğrulaması ile şirket içi veri kaynaklarına bağlanma](/sql/integration-services/lift-shift/ssis-azure-connect-with-windows-auth) 

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, şunların nasıl yapıldığını öğrendiniz: 

> [!div class="checklist"]
> * Veri fabrikası oluşturma.
> * Azure-SSIS tümleştirme çalışma zamanı sağlama.

Şirket içinden buluta veri kopyalama hakkında bilgi edinmek için sonraki öğreticiye geçin: 

> [!div class="nextstepaction"]
> [Verileri buluta kopyalama](tutorial-copy-data-portal.md)
