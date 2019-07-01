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
ms.date: 06/26/2019
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: bd2c055d2f7f1e90918cb160cfdebfe88f7f8ea4
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67484815"
---
# <a name="provision-the-azure-ssis-integration-runtime-in-azure-data-factory"></a>Azure Data Factory'de Azure-SSIS Tümleştirme Çalışma Zamanı Sağlama

Bu öğretici, Azure portalını kullanarak bir Azure SQL Server Integration Services (SSIS) Integration Runtime (IR) Azure Data Factory (ADF) sağlamak için adımlar sağlar. Azure SQL veritabanı sunucusu/tarafından yönetilen örneği (Proje dağıtım modeli) barındırılan SSIS kataloğunu (SSISDB) ve bu dosya sistemlerine/dosyasına dağıtılan dağıtılan paketleri çalıştıran bir azure-SSIS IR destekler paylaşımları/Azure dosyaları (paket dağıtım modeli). Azure-SSIS IR sağlandıktan sonra daha sonra SQL Server veri Araçları (SSDT) gibi alışık olduğunuz araçları kullanabilirsiniz / SQL Server Management Studio (SSMS) ve komut satırı yardımcı programları gibi `dtinstall` / `dtutil` / `dtexec`, dağıtın ve paketlerinizi Azure'da çalıştırın. Azure-SSIS IR’ler hakkında kavramsal bilgiler için bkz. [Azure SSIS tümleştirme çalışma zamanına genel bakış](concepts-integration-runtime.md#azure-ssis-integration-runtime).

Bu öğreticide, aşağıdaki adımları tamamlayacaksınız:

> [!div class="checklist"]
> * Veri fabrikası oluşturma.
> * Azure-SSIS tümleştirme çalışma zamanı sağlama.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

- **Azure aboneliği**. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.
- **Azure SQL veritabanı sunucusu (isteğe bağlı)** . Bir veritabanı sunucusu zaten yoksa, başlamadan önce Azure portalında bir tane oluşturun. ADF sırayla SSISDB bu veritabanı sunucusunda oluşturur. Veritabanı sunucusunu tümleştirme çalışma zamanı ile aynı Azure bölgesinde oluşturmanız önerilir. Bu yapılandırma, tümleştirme çalışma zamanının Azure bölgelerinden geçmeden ssısdb'ye yürütme günlüklerini SSISDB yazma sağlar. 
    - Seçilen veritabanı sunucusuna göre SSISDB sizin adınıza tek veritabanı, elastik havuzun bir parçası veya Yönetilen Örnek biçiminde oluşturulabilir ve genel ağ üzerinden veya sanal ağa eklenerek erişilebilir. SSISDB'yi barındırmak için veritabanı sunucu türünü seçmek kılavuzluk etmesi için bkz [karşılaştırma Azure SQL veritabanı tek veritabanı/esnek havuz/yönetilen örnek](../data-factory/create-azure-ssis-integration-runtime.md#compare-sql-database-single-databaseelastic-pool-and-sql-database-managed-instance). Azure SQL veritabanı sunucusu ile sanal ağ hizmet uç noktaları/yönetilen örnek bir sanal ağdaki SSISDB barındırmak veya şirket içi verilere erişmesi için kullandığınız gerekirse, Azure-SSIS IR'yi bir sanal ağa katılın, bkz: [Azure-SSIS IR oluşturma bir sanal ağdaki](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime).
    - Veritabanı sunucusunda **Azure hizmetlerine erişime izin ver** ayarının etkin olduğundan emin olun. Sanal ağ hizmet uç noktaları/yönetilen örnek SSISDB'yi barındırmak için bir sanal ağ ile Azure SQL veritabanı sunucusu kullandığınızda, bu geçerli değildir. Daha fazla bilgi için bkz. [Azure SQL veritabanınızın güvenliğini sağlama](../sql-database/sql-database-security-tutorial.md#create-firewall-rules). PowerShell kullanarak bu ayarı etkinleştirmek için bkz: [yeni AzSqlServerFirewallRule](/powershell/module/az.sql/new-azsqlserverfirewallrule).
    - İstemci makinenin IP adresini veya veritabanı sunucusu için Güvenlik Duvarı ayarlarındaki istemci IP adresi listesine istemci makinenin IP adresini içeren bir IP adresi aralığı ekleyin. Daha fazla bilgi için bkz. [Azure SQL Veritabanı'nda sunucu düzeyinde ve veritabanı düzeyinde güvenlik duvarı kuralları yapılandırma](../sql-database/sql-database-firewall-configure.md).
    - Sunucu yönetici kimlik bilgilerinizle SQL kimlik doğrulaması veya Azure Active Directory (AAD) kimlik doğrulaması için ADF ile yönetilen kimlik kullanarak veritabanı sunucusunda bağlanabilirsiniz. İkinci seçenekte ADF için yönetilen kimliğinizi veritabanı sunucusuna erişim izni olan bir AAD grubuna eklemeniz gerekir, bkz. [AAD kimlik doğrulaması ile Azure-SSIS IR oluşturma](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime).
    - Veritabanı sunucunuza bir SSISDB zaten sahip olmadığını onaylayın. Bir Azure-SSIS IR'nin sağlanması var olan bir SSISDB kullanma desteği olmamasıdır.

> [!NOTE]
> - ADF ve Azure-SSIS IR olduğu anda Azure bölgelerinin listesi için bkz. [ADF + SSIS IR bölgelere göre kullanılabilirliği](https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all). 

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

   a. Üzerinde **oluşturma SSIS Kataloğu...**  dağıtım modeli Azure-SSIS IR üzerinde çalıştırmak paketler için onay kutusunu seçin: Veritabanı sunucunuz tarafından barındırılan SSISDB ile paketlerinin dağıtıldığı proje dağıtım modeli veya paket dağıtımda, dosya sistemleri/dosya paylaşımları/Azure'a paketlerinin dağıtıldığı dosya. Bunu işaretlerseniz, kendi veritabanı sunucusu oluşturmak ve yönetmek size SSISDB'yi barındırmak için sizin adınıza getirmek gerekir.
   
   b. **Subscription** (Abonelik) alanında SSISDB'yi barındıran Azure aboneliğini seçin. 

   c. **Location** (Konum) alanında SSISDB'yi barındıran veritabanı sunucunuzun konumunu seçin. Tümleştirme çalışma zamanınızla aynı konumu seçmenizi öneririz. 

   d. **Catalog Database Server Endpoint** (Katalog Veritabanı Sunucusu Uç Noktası) alanında SSISDB'yi barındıracak veritabanı sunucunuzun uç noktasını seçin. Seçilen veritabanı sunucusuna göre SSISDB sizin adınıza tek veritabanı, elastik havuzun bir parçası veya Yönetilen Örnek biçiminde oluşturulabilir ve genel ağ üzerinden veya sanal ağa eklenerek erişilebilir. SSISDB'yi barındırmak için veritabanı sunucu türünü seçmek kılavuzluk etmesi için bkz [karşılaştırma Azure SQL veritabanı tek veritabanı/esnek havuz/yönetilen örnek](../data-factory/create-azure-ssis-integration-runtime.md#compare-sql-database-single-databaseelastic-pool-and-sql-database-managed-instance). SSISDB barındırmak veya şirket içi verilere erişmesi için bir sanal ağdaki sanal ağ hizmet uç noktaları/yönetilen örnek ile Azure SQL veritabanı sunucusu seçin gerekirse, Azure-SSIS IR'yi bir sanal ağa katılın, bkz: [Azure-SSIS oluşturma Bir sanal ağ içinde IR](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime). 

   e. Üzerinde **kullanım AAD kimlik doğrulaması...**  onay kutusunu SSISDB'yi barındırmak için veritabanı sunucusu için kimlik doğrulama yöntemini seçin: SQL kimlik doğrulaması veya, ADF için AAD kimlik doğrulamasını yönetilen kimliğe sahip. Kaydetmeden gerekirse, veritabanı sunucunuza erişim izni olan bir AAD gruba, ADF için yönetilen kimlik eklemek için bkz: [AAD kimlik doğrulaması ile Azure-SSIS IR oluşturma](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime). 

   f. **Admin Username** (Yönetici Kullanıcı Adı) alanına SSISDB'yi barındıracak veritabanı sunucunuzun SQL kimlik doğrulaması kullanıcı adını girin. 

   g. **Admin Password** (Yönetici Parolası) alanına SSISDB'yi barındıracak veritabanı sunucunuzun SQL kimlik doğrulaması parolasını girin. 

   h. İçin **Katalog veritabanı hizmet katmanı**, veritabanı sunucunuza SSISDB'yi barındırmak için Hizmet katmanını seçin: Temel/standart/Premium katmanı veya elastik havuz adı. 

   i. **Test Connection** (Bağlantıyı Sına) öğesine tıklayın ve başarılı olursa **Next** (İleri) öğesine tıklayın. 

1. **Advanced Settings** (Gelişmiş Ayarlar) sayfasında aşağıdaki adımları tamamlayın: 

   ![Gelişmiş ayarlar](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings.png)

   a. **Maximum Parallel Executions Per Node** (Düğüm Başına Maksimum Paralel Yürütme) alanında tümleştirme çalışma zamanınızda her düğümde eş zamanlı olarak yürütülecek maksimum paket sayısını seçin. Yalnızca desteklenen paket sayıları görüntülenir. Yoğun işlemci/bellek kullanan tek bir büyük/ağır paket çalıştırmak için birden fazla çekirdek kullanmak istiyorsanız düşük bir sayı seçin. Tek bir çekirdekte bir veya daha fazla küçük/hafif paket çalıştırmak istiyorsanız daha yüksek bir sayı seçin. 

   b. **Custom Setup Container SAS URI** (Özel Kurulum Kapsayıcısı SAS URI'si) alanında isteğe bağlı olarak kurulum betiğinizin ve ilgili dosyalarının depolandığı Azure Depolama Blobu kapsayıcısının Paylaşılan Erişim İmzası (SAS) Tekdüzen Kaynak Tanımlayıcısı (URI) değerini girebilirsiniz, bkz. [Azure-SSIS IR için özel kurulum](https://docs.microsoft.com/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup). 

   c. **Select a VNet...** (VNet seçin...) onay kutusunda tümleştirme çalışma zamanınızı sanal ağa eklemek isteyip istemediğinizi seçin. Sanal ağ hizmet uç noktaları/yönetilen örnek SSISDB'yi barındırmak için bir sanal ağ ile Azure SQL veritabanı sunucusu kullanın ya da şirket içi verilere erişim gerektiren bkz denetlemelisiniz [oluşturma Azure-SSIS IR'yi bir sanal ağdaki](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime). 

1. Tümleştirme çalışma zamanı oluşturma işlemini başlatmak için **Finish** (Son) öğesine tıklayın. 

   > [!NOTE]
   > Herhangi bir özel kurulum süresi, bu işlem, 5 dakika içinde tamamlanmalıdır.
   >
   > SSISDB kullanırsanız, ADF hizmet SSISDB hazırlamak için veritabanı sunucusuna bağlanır. 
   > 
   > Bir Azure-SSIS IR sağladığınızda, Access Redistributable ve SSIS için Azure Feature Pack da yüklenir. Bu bileşenler, Excel/erişim dosyaları ve zaten yerleşik bileşenler tarafından desteklenen veri kaynaklarının yanı sıra çeşitli Azure veri kaynaklarıyla bağlantı sağlar. Ek bileşenleri de yükleyebilirsiniz bkz [Azure-SSIS IR için özel kurulum](how-to-configure-azure-ssis-ir-custom-setup.md).

1. **Bağlantılar** sekmesinde, gerekirse **Tümleştirme Çalışma Zamanları**’na geçin. Durumu yenilemek için **Yenile**’yi seçin. 

   !["Yenile" düğmesi ile oluşturma durumu](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-creation-status.png)

1. Tümleştirme çalışma zamanını durdurmak/başlatmak, düzenlemek veya silmek için **Eylemler** sütunundaki bağlantıları kullanın. Tümleştirme çalışma zamanının JSON kodunu görüntülemek için son bağlantıyı kullanın. Düzenle ve sil düğmeleri yalnızca IR durdurulmuş durumdayken etkin olur. 

   !["Eylemler" sütunundaki bağlantılar](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-actions.png) 

## <a name="deploy-ssis-packages"></a>SSIS paketlerini dağıtma

SSISDB kullanırsanız, paketlerinizi dağıtmayı ve Azure-SSIS IR, sunucu uç noktası aracılığıyla veritabanı sunucunuza bağlanmasına SSDT/SSMS araçlarını kullanarak çalıştırın. Genel bir uç nokta ile Azure SQL veritabanı sunucusu/yönetilen örnek için sunucu uç noktası biçimidir `<server name>.database.windows.net` / `<server name>.public.<dns prefix>.database.windows.net,3342`sırasıyla. SSISDB kullanmazsanız, paketlerinizi dosya sistemleri/dosya paylaşımları/Azure'a dağıtabilirsiniz dosyaları ve bunları Azure-SSIS IR kullanarak çalıştırın `dtinstall` / `dtutil` / `dtexec` komut satırı yardımcı programları. Daha fazla bilgi için [dağıtma SSIS paketlerini](/sql/integration-services/packages/deploy-integration-services-ssis-projects-and-packages#deploy-packages-to-integration-services-server). Her iki durumda da Azure-SSIS IR üzerinde dağıtılan paketlerinizi çalıştırabilirsiniz ADF hatlarında kullanarak SSIS paketi yürütme etkinliği bkz [çağırma SSIS paketi yürütme birinci sınıf bir ADF etkinlik olarak](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity). 

Ayrıca, SSIS belgelerinde aşağıdaki makalelere bakın: 

- [Dağıtma, çalıştırma ve SSIS paketlerini azure'da izleme](/sql/integration-services/lift-shift/ssis-azure-deploy-run-monitor-tutorial) 
- [Azure'da SSISDB bağlanma](/sql/integration-services/lift-shift/ssis-azure-connect-to-catalog-database) 
- [Azure'da paket yürütmeler zamanlama](/sql/integration-services/lift-shift/ssis-azure-schedule-packages) 
- [Windows kimlik doğrulaması ile şirket içi veri kaynaklarına bağlanma](/sql/integration-services/lift-shift/ssis-azure-connect-with-windows-auth) 

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz: 

> [!div class="checklist"]
> * Veri fabrikası oluşturma.
> * Azure-SSIS tümleştirme çalışma zamanı sağlama.
> * SSIS paketlerini dağıtma

Azure-SSIS tümleştirme çalışma zamanınızı özelleştirme hakkında bilgi edinmek için şu makaleye bakın: 

> [!div class="nextstepaction"]
> [Azure-SSIS IR hizmetini özelleştirme](https://docs.microsoft.com/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup)