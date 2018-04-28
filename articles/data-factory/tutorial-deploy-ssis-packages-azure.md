---
title: SSIS paketlerini Azure’a dağıtma | Microsoft Docs
description: Bu makalede, Azure Data Factory kullanarak SSIS paketlerini Azure’a dağıtma ve Azure-SSIS tümleştirme çalışma zamanı oluşturma işlemleri açıklanmaktadır.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: hero-article
ms.date: 04/13/2018
ms.author: douglasl
ms.openlocfilehash: cc0c26d83794cfb0b398e668ae89e268901df345
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="deploy-sql-server-integration-services-packages-to-azure"></a>SQL Server Integration Services paketlerini Azure'a dağıtma
Bu öğretici, Azure portalını kullanarak Azure Data Factory’de bir Azure-SSIS tümleştirme çalışma zamanı (IR) sağlama adımlarını sunar. Daha sonra, SQL Server Veri Araçları veya SQL Server Management Studio’yu kullanarak Azure’da bu çalışma zamanına SQL Server Integration Services (SSIS) paketleri dağıtabilirsiniz. Azure-SSIS IR’ler hakkında kavramsal bilgiler için bkz. [Azure SSIS tümleştirme çalışma zamanına genel bakış](concepts-integration-runtime.md#azure-ssis-integration-runtime).

Bu öğreticide, aşağıdaki adımları tamamlayacaksınız:

> [!div class="checklist"]
> * Veri fabrikası oluşturma.
> * Azure-SSIS tümleştirme çalışma zamanı sağlama.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Data Factory hizmetinin genel kullanıma açık (GA) 1. sürümünü kullanıyorsanız [Data Factory sürüm 1 belgeleri](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) konusunu inceleyin.


## <a name="prerequisites"></a>Ön koşullar
- **Azure aboneliği**. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun. 
- **Azure SQL Veritabanı sunucusu**. Henüz bir veritabanı sunucunuz yoksa, başlamadan önce Azure portalında bir tane oluşturun. Azure Data Factory, bu veritabanı sunucusunda SSIS Kataloğu (SSISDB veritabanı) oluşturur. Veritabanı sunucusunu tümleştirme çalışma zamanı ile aynı Azure bölgesinde oluşturmanız önerilir. Bu yapılandırma, tümleştirme çalışma zamanının Azure bölgelerinden geçmeden SSISDB veritabanına yürütme günlüklerini yazmasına olanak tanır. 
- Veritabanı sunucusunda **Azure hizmetlerine erişime izin ver** ayarının etkin olduğundan emin olun. Daha fazla bilgi için bkz. [Azure SQL veritabanınızın güvenliğini sağlama](../sql-database/sql-database-security-tutorial.md#create-a-server-level-firewall-rule-in-the-azure-portal). Bu ayarı PowerShell kullanarak etkinleştirmek için bkz. [New-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule?view=azurermps-4.4.1).
- Veritabanı sunucusunun güvenlik duvarı ayarlarındaki istemci IP adresi listesine istemci makinenin IP adresini veya istemci makinenin IP adresini içeren IP adresi aralığını ekleyin. Daha fazla bilgi için bkz. [Azure SQL Veritabanı'nda sunucu düzeyinde ve veritabanı düzeyinde güvenlik duvarı kuralları yapılandırma](../sql-database/sql-database-firewall-configure.md).
- Azure SQL Veritabanı sunucunuzun SSIS Kataloğuna (SSISDB veritabanı) sahip olmadığını doğrulayın. Azure-SSIS IR’nin sağlanması, mevcut bir SSIS Kataloğunun kullanılmasını desteklemez.

> [!NOTE]
> - Sürüm 2’nin veri fabrikasını şu bölgelerde oluşturabilirsiniz: Doğu ABD, Doğu ABD 2, Güneydoğu Asya ve Batı Avrupa. 
> - Azure SSIS IR’yi şu bölgelerde oluşturabilirsiniz: Doğu ABD, Doğu ABD 2, Orta ABD, Batı ABD 2, Kuzey Avrupa, Batı Avrupa, UK Güney ve Avustralya Doğu. 

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

1. **Microsoft Edge** veya **Google Chrome** web tarayıcısını açın. Şu anda Data Factory kullanıcı arabirimi yalnızca Microsoft Edge ve Google Chrome web tarayıcılarında desteklenmektedir.
2. [Azure Portal](https://portal.azure.com/) oturum açın.    
3. Soldaki menüden **Yeni**’yi, sonra **Veri ve Analiz**’i ve ardından **Data Factory**’i seçin. 
   
   ![“Yeni” bölmesinde Data Factory seçimi](./media/tutorial-create-azure-ssis-runtime-portal/new-data-factory-menu.png)
3. **Yeni veri fabrikası** sayfasında **ad** altında **MyAzureSsisDataFactory** adını girin. 
      
   ![“Yeni veri fabrikası” sayfası](./media/tutorial-create-azure-ssis-runtime-portal/new-azure-data-factory.png)
 
   Azure data factory adı *küresel olarak benzersiz* olmalıdır. Aşağıdaki hatayı alırsanız veri fabrikasının adını değiştirin (örneğin **&lt;adınız&gt;MyAzureSsisDataFactory**) ve yeniden oluşturmayı deneyin. Data Factory yapıtlarının adlandırma kuralları için [Data Factory - adlandırma kuralları](naming-rules.md) makalesini inceleyin.
  
   `Data factory name “MyAzureSsisDataFactory” is not available`
4. **Abonelik** için, veri fabrikasını oluşturmak istediğiniz Azure aboneliğini seçin. 
5. **Kaynak Grubu** için aşağıdaki adımlardan birini uygulayın:
     
   - **Var olanı kullan**’ı ve ardından listeden var olan bir kaynak grubunu seçin. 
   - **Yeni oluştur**’u seçin ve bir kaynak grubunun adını girin.   
         
   Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/resource-group-overview.md).  
6. **Sürüm** için **V2 (Önizleme)** öğesini seçin.
7. **Konum** için, veri fabrikasının konumunu seçin. Bu listede yalnızca veri fabrikası oluşturma için desteklenen konumlar gösterilir.
8. **Panoya sabitle**’yi seçin.     
9. **Oluştur**’u seçin.
10. Panoda, **Veri fabrikası dağıtılıyor** durumuna sahip aşağıdaki kutucuğu görürsünüz: 

   ![“Data Factory Dağıtılıyor” kutucuğu](media/tutorial-create-azure-ssis-runtime-portal/deploying-data-factory.png)
11. Oluşturma işlemi tamamlandıktan sonra, **Veri fabrikası** sayfasını görürsünüz.
   
   ![Veri fabrikasının giriş sayfası](./media/tutorial-create-azure-ssis-runtime-portal/data-factory-home-page.png)
12. Data Factory kullanıcı arabirimini (UI) ayrı bir sekmede açmak için **Yazar ve İzleyici**’yi seçin. 

## <a name="provision-an-azure-ssis-integration-runtime"></a>Azure-SSIS tümleştirme çalışma zamanı sağlama

1. **Başlarken** sayfasında **SSIS Tümleştirme Çalışma Zamanı Yapılandır** kutucuğunu seçin. 

   !["SSIS Tümleştirme Çalışma Zamanı Yapılandır" kutucuğu](./media/tutorial-create-azure-ssis-runtime-portal/configure-ssis-integration-runtime-tile.png)
2. **Tümleştirme Çalışma Zamanı Kurulumu**’nun **Genel Ayarlar** sayfasında aşağıdaki adımları tamamlayın: 

   ![Genel ayarlar](./media/tutorial-create-azure-ssis-runtime-portal/general-settings.png)

   a. **Ad** için tümleştirme çalışma zamanı adı belirtin.

   b. **Konum** için tümleştirme çalışma zamanı konumunu seçin. Yalnızca desteklenen konumlar görüntülenir.

   c. **Düğüm Boyutu** için, SSIS çalışma zamanı ile birlikte yapılandırılacak düğümün boyutunu seçin.

   d. **Düğüm Sayısı** için kümedeki düğüm sayısını belirtin.
   
   e. **İleri**’yi seçin. 
3. **SQL Ayarları** sayfasında aşağıdaki adımları tamamlayın: 

   ![SQL ayarları](./media/tutorial-create-azure-ssis-runtime-portal/sql-settings.png)

   a. **Abonelik** için Azure veritabanı sunucusuna sahip Azure aboneliğini belirtin.

   b. **Katalog Veritabanı Sunucusu Uç Noktası** için Azure veritabanı sunucunuzu seçin.

   c. **Yönetici Kullanıcı Adı** için yönetici kullanıcı adını girin.

   d. **Yönetici Parolası** için yöneticiye ait parolayı girin.

   e. **Katalog Veritabanı Sunucusu Katmanı** için SSISDB veritabanının hizmet katmanını seçin. Varsayılan değer **Temel**’dir.

   f. **İleri**’yi seçin. 
4. **Gelişmiş Ayarlar** sayfasında **Düğüm Başına En Fazla Paralel Yürütme** için bir değer seçin.   

   ![Gelişmiş ayarlar](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings.png)    
5. Bu adım *isteğe bağlıdır*. Tümleştirme çalışma zamanının katılmasını istediğiniz bir sanal ağınız (klasik dağıtım modeli veya Azure Resource Manager ile oluşturulmuş) varsa, **Azure-SSIS tümleştirme çalışma zamanınızın katılması için bir VNet seçin ve Azure hizmetlerinin VNet izinlerini/ayarlarını yapılandırmasına izin verin** onay kutusunu seçin. Sonra aşağıdaki adımları tamamlayın: 

   ![Bir sanal ağda gelişmiş ayarlar](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings-vnet.png)    

   a. **Abonelik** için, sanal ağı içeren aboneliği belirtin.

   b. **VNet Adı** için sanal ağın adını seçin.

   c. **Alt Ağ Adı** için sanal ağdaki alt ağın adını seçin. 
6. Azure-SSIS tümleştirme çalışma zamanının oluşturulmasını başlatmak için **Son**’u seçin. 

   > [!IMPORTANT]
   > Bu işlemin tamamlanması yaklaşık 20 dakika sürer.
   >
   > Data Factory hizmeti, SSIS Kataloğunu (SSISDB veritabanı) hazırlamak için Azure SQL veritabanınıza bağlanır. Betik ayrıca, belirtilmişse, sanal ağınızın izin ve ayarlarını da yapılandırır. Bununla birlikte, Azure-SSIS tümleştirme çalışma zamanının yeni örneğini sanal ağa ekler.
   > 
   > Bir Azure-SSIS IR örneğini sağladığınızda, SSIS için Azure Feature Pack ve Access Redistributable da yüklenir. Bu bileşenler, Excel ile Access dosyalarıyla ve yerleşik bileşenler tarafından desteklenen veri kaynaklarına ek olarak çeşitli Azure veri kaynaklarıyla bağlantı kurma olanağı sunar. Ek bileşenleri de yükleyebilirsiniz. Daha fazla bilgi için bkz. [Azure-SSIS tümleştirmesi çalışma zamanı için özel kurulum](how-to-configure-azure-ssis-ir-custom-setup.md).

7. **Bağlantılar** sekmesinde, gerekirse **Tümleştirme Çalışma Zamanları**’na geçin. Durumu yenilemek için **Yenile**’yi seçin. 

   !["Yenile" düğmesi ile oluşturma durumu](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-creation-status.png)
8. Tümleştirme çalışma zamanını durdurmak/başlatmak, düzenlemek veya silmek için **Eylemler** sütunundaki bağlantıları kullanın. Tümleştirme çalışma zamanının JSON kodunu görüntülemek için son bağlantıyı kullanın. Düzenle ve sil düğmeleri yalnızca IR durdurulmuş durumdayken etkin olur. 

   !["Eylemler" sütunundaki bağlantılar](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-actions.png)        

## <a name="create-an-azure-ssis-integration-runtime"></a>Azure SSIS tümleştirme çalışma zamanı oluşturma

1. Veri fabrikanızdaki mevcut tümleştirme çalışma zamanlarını görüntülemek için Azure Data Factory kullanıcı arabiriminde **Düzenle** sekmesine geçin, **Bağlantılar**’ı seçin ve sonra **Tümleştirme Çalışma Zamanları** sekmesine geçin. 
   ![Var olan IR’leri görüntüleme seçimleri](./media/tutorial-create-azure-ssis-runtime-portal/view-azure-ssis-integration-runtimes.png)
2. Bir Azure-SSIS IR oluşturmak için **Yeni**'yi seçin. 

   ![Menü aracılığıyla tümleştirme çalışma zamanı](./media/tutorial-create-azure-ssis-runtime-portal/edit-connections-new-integration-runtime-button.png)
3. **Tümleştirme Çalışma Zamanı Kurulumu** penceresinde **Mevcut SSIS paketlerini Azure’da yürütmek üzere lift-and-shift ile taşıma**’yı seçip **İleri**’yi seçin.

   ![Tümleştirme çalışma zamanının türünü belirtin](./media/tutorial-create-azure-ssis-runtime-portal/integration-runtime-setup-options.png)
4. Azure SSIS IR kurulumunun diğer adımları için [Azure SSIS tümleştirme çalışma zamanı sağlama](#provision-an-azure-ssis-integration-runtime) bölümüne bakın. 

 
## <a name="deploy-ssis-packages"></a>SSIS paketlerini dağıtma
Şimdi SQL Server Veri Araçları veya SQL Server Management Studio’yu kullanarak SSIS paketlerinizi Azure’a dağıtın. SSIS Kataloğunu (SSISDB veritabanı) barındıran Azure veritabanı sunucunuza bağlanın. Azure veritabanı sunucusunun adı şu biçimdedir: `<servername>.database.windows.net` (Azure SQL Veritabanı için). 

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
