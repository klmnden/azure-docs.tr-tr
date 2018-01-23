---
title: "Portalı kullanarak Azure SSIS tümleştirme çalışma zamanı sağlama | Microsoft Docs"
description: "Bu makalede Azure portalını kullanarak Azure-SSIS tümleştirme çalışma zamanı oluşturma açıklanır."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: 
ms.devlang: 
ms.topic: hero-article
ms.date: 01/09/2018
ms.author: spelluru
ms.openlocfilehash: b6a795f8a26340f24f9e09aea371ba90afe50101
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2018
---
# <a name="provision-an-azure-ssis-integration-runtime-by-using-the-data-factory-ui"></a>Data Factory kullanıcı arabirimini kullanarak Azure SSIS tümleştirme çalışma zamanı sağlama
Bu öğretici, Azure portalını kullanarak Azure Data Factory’de bir Azure-SSIS tümleştirme çalışma zamanı (IR) sağlama adımlarını sunar. Daha sonra, SQL Server Veri Araçları (SSDT) veya SQL Server Management Studio’yu (SSMS) kullanarak Azure’da bu çalışma zamanına SQL Server Integration Services (SSIS) paketleri dağıtabilirsiniz. Azure-SSIS IR hakkında kavramsal bilgiler için bkz. [Azure SSIS tümleştirme çalışma zamanına genel bakış](concepts-integration-runtime.md#azure-ssis-integration-runtime).

Bu öğreticide, aşağıdaki adımları gerçekleştireceksiniz:

> [!div class="checklist"]
> * Veri fabrikası oluşturma.
> * Azure-SSIS tümleştirme çalışma zamanı oluşturma ve başlatma

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Data Factory hizmetinin genel kullanıma açık (GA) 1. sürümünü kullanıyorsanız [Data Factory sürüm 1 belgeleri](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) konusunu inceleyin.


## <a name="prerequisites"></a>Ön koşullar
- **Azure aboneliği**. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun. 
- **Azure SQL Veritabanı sunucusu**. Henüz bir veritabanı sunucunuz yoksa, başlamadan önce Azure portalında bir tane oluşturun. Azure Data Factory, bu veritabanı sunucusunda SSIS Kataloğu veritabanını (SSISDB) oluşturur. Veritabanı sunucusunu tümleştirme çalışma zamanı ile aynı Azure bölgesinde oluşturmanız önerilir. Bu yapılandırma, tümleştirme çalışma zamanının Azure bölgelerinden geçmeden SSISDB’ye yürütme günlüklerini yazmasına olanak tanır. 
    - Veritabanı sunucusunda "**Azure hizmetlerine erişime izin ver**" ayarının etkin olduğundan emin olun. Daha fazla bilgi için bkz. [Azure SQL veritabanınızın güvenliğini sağlama](../sql-database/sql-database-security-tutorial.md#create-a-server-level-firewall-rule-in-the-azure-portal). Bu ayarı PowerShell kullanarak etkinleştirmek için bkz. [New-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule?view=azurermps-4.4.1).
    - Veritabanı sunucusunun güvenlik duvarı ayarlarındaki istemci IP adresi listesine istemci makinenin IP adresini veya istemci makinenin IP adresini içeren IP adresi aralığını ekleyin. Daha fazla bilgi için bkz. [Azure SQL Veritabanı'nda sunucu düzeyinde ve veritabanı düzeyinde güvenlik duvarı kuralları yapılandırma](../sql-database/sql-database-firewall-configure.md). 
 
## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

1. [Azure Portal](https://portal.azure.com/)’da oturum açın.    
2. Soldaki menüde **Yeni**, **Veri + Analiz** ve **Data Factory** öğesine tıklayın. 
   
   ![Yeni->DataFactory](./media/tutorial-create-azure-ssis-runtime-portal/new-data-factory-menu.png)
3. **Yeni veri fabrikası** sayfasında **ad** için **MyAzureSsisDataFactory** adını girin. 
      
     ![Yeni veri fabrikası sayfası](./media/tutorial-create-azure-ssis-runtime-portal/new-azure-data-factory.png)
 
   Azure data factory adı **küresel olarak benzersiz** olmalıdır. Aşağıdaki hatayı alırsanız veri fabrikasının adını değiştirin (örneğin adınızMyAzureSsisDataFactory) ve oluşturmayı yeniden deneyin. Data Factory yapıtlarını adlandırma kuralları için [Data Factory - Adlandırma Kuralları](naming-rules.md) makalesine bakın.
  
       `Data factory name “MyAzureSsisDataFactory” is not available`
3. Veri fabrikasını oluşturmak istediğiniz Azure **aboneliğini** seçin. 
4. **Kaynak Grubu** için aşağıdaki adımlardan birini uygulayın:
     
      - **Var olanı kullan**’ı seçin ve ardından açılır listeden var olan bir kaynak grubu belirleyin. 
      - **Yeni oluştur**’u seçin ve bir kaynak grubunun adını girin.   
         
      Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/resource-group-overview.md).  
4. **Sürüm** için **V2 (Önizleme)** öğesini seçin.
5. Data factory için **konum** seçin. Listede yalnızca veri fabrikası oluşturma için desteklenen konumlar gösterilir.
6. **Panoya sabitle**’yi seçin.     
7. **Oluştur**’a tıklayın.
8. Panoda şu kutucuğu ve üzerinde şu durumu görürsünüz: **Veri fabrikası dağıtılıyor**. 

    ![veri fabrikası dağıtılıyor kutucuğu](media/tutorial-create-azure-ssis-runtime-portal/deploying-data-factory.png)
9. Oluşturma işlemi tamamlandıktan sonra, resimde gösterildiği gibi **Data Factory** sayfasını görürsünüz.
   
   ![Data factory giriş sayfası](./media/tutorial-create-azure-ssis-runtime-portal/data-factory-home-page.png)
10. Azure Data Factory Kullanıcı Arabirimini (UI) ayrı bir sekmede açmak için **Geliştir ve İzle**’ye tıklayın. 

## <a name="provision-an-azure-ssis-integration-runtime"></a>Azure SSIS tümleştirme çalışma zamanı sağlama

1. Başlarken sayfasında **SSIS Tümleştirme Çalışma Zamanı Yapılandır** kutucuğuna tıklayın. 

   ![SSIS Tümleştirme Çalışma Zamanı Yapılandır kutucuğu](./media/tutorial-create-azure-ssis-runtime-portal/configure-ssis-integration-runtime-tile.png)
2. **Tümleştirme Çalışma Zamanı Kurulumu**’nun **Genel Ayarlar** sayfasında aşağıdaki adımları uygulayın: 

   ![Genel ayarlar](./media/tutorial-create-azure-ssis-runtime-portal/general-settings.png)

    1. Tümleştirme çalışma zamanı için bir **ad** belirtin.
    2. Tümleştirme çalışma zamanının **konumunu** belirtin. Yalnızca desteklenen konumlar görüntülenir.
    3. SSIS çalışma zamanı ile birlikte yapılandırılacak **düğümün boyutunu** seçin.
    4. Kümedeki **düğüm sayısını** belirtin.
    5. **İleri**’ye tıklayın. 
1. **SQL Ayarları** bölümünde aşağıdaki adımları uygulayın: 

    ![SQL ayarları](./media/tutorial-create-azure-ssis-runtime-portal/sql-settings.png)

    1. Azure SQL Server’ı içeren Azure **aboneliğini** belirtin. 
    2. **Katalog Veritabanı Sunucu Uç Noktası** için Azure SQL Server’ınızı seçin.
    3. **Yöneticinin** kullanıcı adını belirtin.
    4. Yöneticinin **parolasını** girin.  
    5. SSISDB veritabanı için **hizmet katmanını** seçin. Varsayılan değer Temel’dir.
    6. **İleri**’ye tıklayın. 
1.  **Gelişmiş Ayarlar** sayfasında **Düğüm Başına En Fazla Paralel Yürütme** için bir değer seçin.   

    ![Gelişmiş ayarlar](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings.png)    
5. Bu adım **isteğe bağlıdır**. Tümleştirme çalışma zamanının katılmasını istediğiniz klasik bir sanal ağınız (VNet) varsa, **Azure-SSIS tümleştirme çalışma zamanınızın katılması için bir VNet seçin ve Azure hizmetlerinin VNet izinlerini/ayarlarını yapılandırmasına izin verin** seçeneğini belirleyip aşağıdaki adımları uygulayın: 

    ![VNet ile gelişmiş ayarlar](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings-vnet.png)    

    1. Klasik VNet’i içeren **aboneliği** belirtin. 
    2. **VNet**'i seçin. <br/>
    4. **Alt Ağ**'ı seçin.<br/> 
1. Azure-SSIS tümleştirme çalışma zamanı oluşturma işlemini başlatmak için **Son**’a tıklayın. 

    > [!IMPORTANT]
    > - Bu işlemin tamamlanması yaklaşık 20 dakika sürer
    > - Data Factory hizmeti, SSIS Kataloğu veritabanını (SSISDB) hazırlamak için Azure SQL Veritabanınıza bağlanır. Betik ayrıca belirtilmişse sanal ağınıza ilişkin izin ve ayarları yapılandırır ve yeni Azure SSIS tümleştirme çalışma zamanı örneğini sanal ağa ekler.
7. **Bağlantılar** penceresinde, gerekirse **Tümleştirme Çalışma Zamanları**’na geçin. Durumu yenilemek için **Yenile**’ye tıklayın. 

    ![Oluşturma durumu](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-creation-status.png)
8. Tümleştirme çalışma zamanını izlemek, durdurmak/başlatmak, düzenlemek veya silmek için **Eylemler** sütununun altındaki bağlantıları kullanın. Tümleştirme çalışma zamanının JSON kodunu görüntülemek için son bağlantıyı kullanın. Düzenle ve sil düğmeleri yalnızca IR durdurulmuş durumdayken etkin olur. 

    ![Azure SSIS IR - eylemler](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-actions.png)        
9. **Eylemler** altında **İzleyici** bağlantısına tıklayın.  

    ![Azure SSIS IR - ayrıntılar](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-details.png)
10. Azure SSIS IR ile ilişkili bir **hata** oluşmuşsa bu sayfada hata sayısını ve hatayla ilgili ayrıntıları görüntüleme bağlantısını görürsünüz. Örneğin, veritabanı sunucusunda SSIS Kataloğu zaten mevcutsa SSISDB veritabanının mevcut olduğunu gösteren bir hata görürsünüz.  
11. Veri fabrikasıyla ilişkili tüm tümleştirme çalışma zamanlarını görmek üzere önceki sayfaya dönmek için üstteki **Tümleştirme Çalışma Zamanları**’na tıklayın.  

## <a name="azure-ssis-integration-runtimes-in-the-portal"></a>Portalda Azure SSIS tümleştirmesi çalışma zamanları

1. Veri fabrikanızdaki mevcut tümleştirme çalışma zamanlarını görüntülemek için Azure Data Factory kullanıcı arabiriminde **Düzenle** sekmesine geçin, **Bağlantılar**’a tıklayın ve sonra **Tümleştirme Çalışma Zamanları** sekmesine geçin. 
    ![Mevcut IR’leri görüntüle](./media/tutorial-create-azure-ssis-runtime-portal/view-azure-ssis-integration-runtimes.png)
1. Yeni bir Azure-SSIS IR oluşturmak için **Yeni**'ye tıklayın. 

    ![Menü aracılığıyla tümleştirme çalışma zamanı](./media/tutorial-create-azure-ssis-runtime-portal/edit-connections-new-integration-runtime-button.png)
2. Azure-SSIS tümleştirme çalışma zamanı oluşturmak için resimde gösterildiği gibi **Yeni**’ye tıklayın. 
3. Tümleştirme Çalışma Zamanı Kurulumu penceresinde **Mevcut SSIS paketlerini Azure’da yürütmek üzere lift-and-shift ile taşıma**’yı seçip **İleri**’ye tıklayın.

    ![Tümleştirme çalışma zamanının türünü belirtin](./media/tutorial-create-azure-ssis-runtime-portal/integration-runtime-setup-options.png)
4. Geriye kalan Azure SSIS IR kurulumu adımları için bkz. [Azure SSIS tümleştirme çalışma zamanı sağlama](#provision-an-azure-ssis-integration-runtime). 

    
## <a name="deploy-ssis-packages"></a>SSIS paketlerini dağıtma
Şimdi SQL Server Veri Araçları (SSDT) veya SQL Server Management Studio’yu (SSMS) kullanarak SSIS paketlerinizi Azure’a dağıtın. SSIS kataloğunu (SSISDB) barındıran Azure SQL sunucunuza bağlanın. Azure SQL sunucusunun adı şu biçimdedir: `<servername>.database.windows.net` (Azure SQL Veritabanı için). 

SSIS belgelerinde aşağıdaki makalelere bakın: 

- [Azure’da bir SSIS paketi dağıtma, çalıştırma ve izleme](/sql/integration-services/lift-shift/ssis-azure-deploy-run-monitor-tutorial)   
- [Azure’da SSIS kataloğuna bağlanma](/sql/integration-services/lift-shift/ssis-azure-connect-to-catalog-database)
- [Azure’da paket yürütmeyi zamanlama](/sql/integration-services/lift-shift/ssis-azure-schedule-packages)
- [Windows kimlik doğrulaması ile şirket içi veri kaynaklarına bağlanma](/sql/integration-services/lift-shift/ssis-azure-connect-with-windows-auth) 

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, şunların nasıl yapıldığını öğrendiniz: 

> [!div class="checklist"]
> * Veri fabrikası oluşturma.
> * Azure-SSIS tümleştirme çalışma zamanı oluşturma ve başlatma

Şirket içinden buluta veri kopyalama hakkında bilgi edinmek için aşağıdaki öğreticiye geçin: 

> [!div class="nextstepaction"]
>[Buluttaki verileri kopyalama](tutorial-copy-data-portal.md)
