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
ms.date: 10/28/2018
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: 26aa8b17917e92a0bcb2393ac3f5d69a70dceefe
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66145201"
---
# <a name="provision-the-azure-ssis-integration-runtime-in-azure-data-factory"></a>Azure Data Factory'de Azure-SSIS Tümleştirme Çalışma Zamanı Sağlama
Bu öğretici, Azure portalını kullanarak Azure Data Factory’de bir Azure-SSIS tümleştirme çalışma zamanı (IR) sağlama adımlarını sunar. Daha sonra, SQL Server Veri Araçları (SSDT) veya SQL Server Management Studio'yu (SSMS) kullanarak Azure'da bu çalışma zamanında SQL Server Integration Services (SSIS) paketleri dağıtabilir ve çalıştırabilirsiniz. Azure-SSIS IR’ler hakkında kavramsal bilgiler için bkz. [Azure SSIS tümleştirme çalışma zamanına genel bakış](concepts-integration-runtime.md#azure-ssis-integration-runtime).

Bu öğreticide, aşağıdaki adımları tamamlayacaksınız:

> [!div class="checklist"]
> * Veri fabrikası oluşturma.
> * Azure-SSIS tümleştirme çalışma zamanı sağlama.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

- **Azure aboneliği**. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun. 
- **Azure SQL Veritabanı sunucusu**. Henüz bir veritabanı sunucunuz yoksa, başlamadan önce Azure portalında bir tane oluşturun. Azure Data Factory, bu veritabanı sunucusunda SSIS Kataloğu (SSISDB veritabanı) oluşturur. Veritabanı sunucusunu tümleştirme çalışma zamanı ile aynı Azure bölgesinde oluşturmanız önerilir. Bu yapılandırma, tümleştirme çalışma zamanının Azure bölgelerinden geçmeden SSISDB veritabanına yürütme günlüklerini yazmasına olanak tanır. 
- Seçilen veritabanı sunucusuna göre SSISDB sizin adınıza tek veritabanı, elastik havuzun bir parçası veya Yönetilen Örnek biçiminde oluşturulabilir ve genel ağ üzerinden veya sanal ağa eklenerek erişilebilir. SSISDB barındırmak için Azure SQL Veritabanı'nı sanal ağ hizmet uç noktaları/Yönetilen Örnek ile kullanırsanız veya şirket içi verilere erişmeniz gerekiyorsa Azure-SSIS IR örneğinizi bir sanal ağa eklemeniz gerekir, bkz. [Sanal ağda Azure-SSIS IR oluşturma](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime). 
- Veritabanı sunucusunda **Azure hizmetlerine erişime izin ver** ayarının etkin olduğundan emin olun. Bu durum SSISDB barındırmak için Azure SQL Veritabanı'nı sanal ağ hizmet uç noktaları/Yönetilen Örnek ile birlikte kullandığınızda geçerli değildir. Daha fazla bilgi için bkz. [Azure SQL veritabanınızın güvenliğini sağlama](../sql-database/sql-database-security-tutorial.md#create-firewall-rules). PowerShell kullanarak bu ayarı etkinleştirmek için bkz: [yeni AzSqlServerFirewallRule](/powershell/module/az.sql/new-azsqlserverfirewallrule). 
- Veritabanı sunucusunun güvenlik duvarı ayarlarındaki istemci IP adresi listesine istemci makinenin IP adresini veya istemci makinenin IP adresini içeren IP adresi aralığını ekleyin. Daha fazla bilgi için bkz. [Azure SQL Veritabanı'nda sunucu düzeyinde ve veritabanı düzeyinde güvenlik duvarı kuralları yapılandırma](../sql-database/sql-database-firewall-configure.md). 
- Veritabanı sunucusuna bağlanmak için sunucu yöneticisi kimlik bilgilerinizle SQL kimlik doğrulamasını veya Azure Data Factory (ADF) için yönetilen kimliğinizle Azure Active Directory (AAD) kimlik doğrulamasını kullanabilirsiniz.  İkinci seçenekte ADF için yönetilen kimliğinizi veritabanı sunucusuna erişim izni olan bir AAD grubuna eklemeniz gerekir, bkz. [AAD kimlik doğrulaması ile Azure-SSIS IR oluşturma](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime). 
- Azure SQL Veritabanı sunucunuzun SSIS Kataloğuna (SSISDB veritabanı) sahip olmadığını doğrulayın. Azure-SSIS IR’nin sağlanması, mevcut bir SSIS Kataloğunun kullanılmasını desteklemez. 

> [!NOTE]
> - Data Factory ve Azure-SSIS Integration Runtime hizmetlerinin kullanılabildiği Azure bölgelerinin listesi için bkz. [Bölgelere göre ADF + SSIS IR kullanılabilirliği](https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all). 

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

## <a name="create-an-azure-ssis-integration-runtime"></a>Azure SSIS tümleştirme çalışma zamanı oluşturma

### <a name="from-the-data-factory-overview"></a>Data Factory genel bakış'tan

1. **Başlarken** sayfasında **SSIS Tümleştirme Çalışma Zamanı Yapılandır** kutucuğunu seçin. 

   !["SSIS Tümleştirme Çalışma Zamanı Yapılandır" kutucuğu](./media/tutorial-create-azure-ssis-runtime-portal/configure-ssis-integration-runtime-tile.png)

1. Azure SSIS IR kurulumunun diğer adımları için [Azure SSIS tümleştirme çalışma zamanı sağlama](#provision-an-azure-ssis-integration-runtime) bölümüne bakın. 

### <a name="from-the-authoring-ui"></a>Yazma Arabiriminden

1. Veri fabrikanızdaki mevcut tümleştirme çalışma zamanlarını görüntülemek için Azure Data Factory kullanıcı arabiriminde **Düzenle** sekmesine geçin, **Bağlantılar**’ı seçin ve sonra **Tümleştirme Çalışma Zamanları** sekmesine geçin. 

   ![Var olan IR’leri görüntüleme seçimleri](./media/tutorial-create-azure-ssis-runtime-portal/view-azure-ssis-integration-runtimes.png)

1. Bir Azure-SSIS IR oluşturmak için **Yeni**'yi seçin. 

   ![Menü aracılığıyla tümleştirme çalışma zamanı](./media/tutorial-create-azure-ssis-runtime-portal/edit-connections-new-integration-runtime-button.png)

1. **Tümleştirme Çalışma Zamanı Kurulumu** penceresinde **Mevcut SSIS paketlerini Azure’da yürütmek üzere lift-and-shift ile taşıma**’yı seçip **İleri**’yi seçin. 

   ![Tümleştirme çalışma zamanının türünü belirtin](./media/tutorial-create-azure-ssis-runtime-portal/integration-runtime-setup-options.png)

1. Azure SSIS IR kurulumunun diğer adımları için [Azure SSIS tümleştirme çalışma zamanı sağlama](#provision-an-azure-ssis-integration-runtime) bölümüne bakın. 

## <a name="provision-an-azure-ssis-integration-runtime"></a>Azure-SSIS tümleştirme çalışma zamanı sağlama

1. **Tümleştirme Çalışma Zamanı Kurulumu**’nun **Genel Ayarlar** sayfasında aşağıdaki adımları tamamlayın: 

   ![Genel ayarlar](./media/tutorial-create-azure-ssis-runtime-portal/general-settings.png)

   a. **Name** (Ad) alanına tümleştirme çalışma zamanınızın adını girin. 

   b. **Description** (Açıklama) alanına tümleştirme çalışma zamanınız için bir açıklama girin. 

   c. **Location** (Konum)alanında tümleştirme çalışma zamanınızın konumunu seçin. Yalnızca desteklenen konumlar görüntülenir. SSISDB'yi barındırmak için veritabanı sunucunuz ile aynı konumu seçmenizi öneririz. 

   d. **Node Size** (Düğüm Boyutu) alanında tümleştirme çalışma zamanınızdaki düğümün boyutunu seçin. Yalnızca desteklenen düğüm boyutları görüntülenir. Yoğun işlemci/bellek kullanan çok sayıda paket çalıştırmak istiyorsanız büyük bir düğüm boyutu seçin. 

   e. **Node Number** (Düğüm Sayısı) alanında tümleştirme çalışma zamanınızdaki düğüm sayısını seçin. Yalnızca desteklenen düğüm sayıları görüntülenir. Aynı anda birden fazla paket çalıştırmak istiyorsanız çok sayıda düğüme sahip bir küme seçin (ölçeklendirme için). 

   f. İçin **sürümü/lisansı**, SQL Server sürümü/lisansı için Integration runtime'ı seçin: Standart veya Kurumsal. Tümleştirme çalışma zamanınızda gelişmiş/premium özellikleri kullanmak istiyorsanız Enterprise sürümünü seçin. 

   g. İçin **paradan tasarruf**, Integration runtime için Azure hibrit Avantajı'nı (AHB) seçeneğini belirleyin: Evet veya Hayır. Karma kullanımla maliyet tasarrufu yapmak üzere Yazılım Güvencesi ile kendi SQL Server lisansınızı getirmek istiyorsanız Evet'i seçin. 

   h. **İleri**’ye tıklayın. 

1. **SQL Ayarları** sayfasında aşağıdaki adımları tamamlayın: 

   ![SQL ayarları](./media/tutorial-create-azure-ssis-runtime-portal/sql-settings.png)

   a. **Subscription** (Abonelik) alanında SSISDB'yi barındıran Azure aboneliğini seçin. 

   b. **Location** (Konum) alanında SSISDB'yi barındıran veritabanı sunucunuzun konumunu seçin. Tümleştirme çalışma zamanınızla aynı konumu seçmenizi öneririz. 

   c. **Catalog Database Server Endpoint** (Katalog Veritabanı Sunucusu Uç Noktası) alanında SSISDB'yi barındıracak veritabanı sunucunuzun uç noktasını seçin. Seçilen veritabanı sunucusuna göre SSISDB sizin adınıza tek veritabanı, elastik havuzun bir parçası veya Yönetilen Örnek biçiminde oluşturulabilir ve genel ağ üzerinden veya sanal ağa eklenerek erişilebilir. SSISDB'yi barındırmak için veritabanı sunucu türünü seçmek kılavuzluk etmesi için bkz [karşılaştırma Azure SQL veritabanı tek veritabanlarını elastik havuzları ve yönetilen örneği](../data-factory/create-azure-ssis-integration-runtime.md#compare-sql-database-single-databaseelastic-pool-and-sql-database-managed-instance). SSISDB barındırmak için Azure SQL Veritabanı'nı sanal ağ hizmet uç noktaları/Yönetilen Örnek ile birlikte seçerseniz veya şirket içi verilere erişmeniz gerekiyorsa Azure-SSIS IR örneğinizi bir sanal ağa eklemeniz gerekir. Bkz. [Sanal ağda Azure-SSIS IR oluşturma](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime). 

   d. Üzerinde **kullanım AAD kimlik doğrulaması...**  onay kutusunu SSISDB'yi barındırmak için veritabanı sunucusu için kimlik doğrulama yöntemini seçin: SQL veya Azure Active Directory (AAD), Azure Data Factory (ADF) için yönetilen kimliğe sahip. Bunu işaretlerseniz ADF için yönetilen kimliğinizi veritabanı sunucusuna erişim izni olan bir AAD grubuna eklemeniz gerekir, bkz. [AAD kimlik doğrulaması ile Azure-SSIS IR oluşturma](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime). 

   e. **Admin Username** (Yönetici Kullanıcı Adı) alanına SSISDB'yi barındıracak veritabanı sunucunuzun SQL kimlik doğrulaması kullanıcı adını girin. 

   f. **Admin Password** (Yönetici Parolası) alanına SSISDB'yi barındıracak veritabanı sunucunuzun SQL kimlik doğrulaması parolasını girin. 

   g. İçin **Katalog veritabanı hizmet katmanı**, veritabanı sunucunuza SSISDB'yi barındırmak için Hizmet katmanını seçin: Temel/standart/Premium katmanı veya elastik havuz adı. 

   h. **Test Connection** (Bağlantıyı Sına) öğesine tıklayın ve başarılı olursa **Next** (İleri) öğesine tıklayın. 

1. **Advanced Settings** (Gelişmiş Ayarlar) sayfasında aşağıdaki adımları tamamlayın: 

   ![Gelişmiş ayarlar](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings.png)

   a. **Maximum Parallel Executions Per Node** (Düğüm Başına Maksimum Paralel Yürütme) alanında tümleştirme çalışma zamanınızda her düğümde eş zamanlı olarak yürütülecek maksimum paket sayısını seçin. Yalnızca desteklenen paket sayıları görüntülenir. Yoğun işlemci/bellek kullanan tek bir büyük/ağır paket çalıştırmak için birden fazla çekirdek kullanmak istiyorsanız düşük bir sayı seçin. Tek bir çekirdekte bir veya daha fazla küçük/hafif paket çalıştırmak istiyorsanız daha yüksek bir sayı seçin. 

   b. **Custom Setup Container SAS URI** (Özel Kurulum Kapsayıcısı SAS URI'si) alanında isteğe bağlı olarak kurulum betiğinizin ve ilgili dosyalarının depolandığı Azure Depolama Blobu kapsayıcısının Paylaşılan Erişim İmzası (SAS) Tekdüzen Kaynak Tanımlayıcısı (URI) değerini girebilirsiniz, bkz. [Azure-SSIS IR için özel kurulum](https://docs.microsoft.com/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup). 

   c. **Select a VNet...** (VNet seçin...) onay kutusunda tümleştirme çalışma zamanınızı sanal ağa eklemek isteyip istemediğinizi seçin. SSISDB barındırmak için Azure SQL Veritabanı'nı sanal ağ hizmet uç noktaları/Yönetilen Örnek ile kullanmanız veya şirket içi verilere erişmeniz gerekip gerekmediğini kontrol etmeniz gerekir, bkz. [Sanal ağda Azure-SSIS IR oluşturma](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime). 

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

Azure-SSIS tümleştirme çalışma zamanınızı özelleştirme hakkında bilgi edinmek için şu makaleye bakın: 

> [!div class="nextstepaction"]
> [Azure-SSIS IR hizmetini özelleştirme](https://docs.microsoft.com/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup)
