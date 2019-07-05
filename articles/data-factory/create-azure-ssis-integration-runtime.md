---
title: Azure-SSIS Integration Runtime, Azure veri fabrikasında | Microsoft Docs
description: Dağıtabilir ve SSIS paketlerini Azure'da çalıştırmak için Azure Data Factory'de bir Azure-SSIS tümleştirme çalışma zamanı oluşturmayı öğrenin.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 06/26/2019
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: fb5335c8dfd94006ba3f0d8d6b890869dd9f3717
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67484818"
---
# <a name="create-azure-ssis-integration-runtime-in-azure-data-factory"></a>Azure Data Factory Azure SSIS tümleştirme çalışma zamanı oluşturma

Bu öğreticide, bir Azure SQL Server Integration Services (SSIS) Integration Runtime (IR) Azure Data Factory (ADF) sağlama adımlarını sunar. Azure SQL veritabanı sunucusu/tarafından yönetilen örneği (Proje dağıtım modeli) barındırılan SSIS kataloğunu (SSISDB) ve bu dosya sistemlerine/dosyasına dağıtılan dağıtılan paketleri çalıştıran bir azure-SSIS IR destekler paylaşımları/Azure dosyaları (paket dağıtım modeli). Azure-SSIS IR sağlandıktan sonra daha sonra SQL Server veri Araçları (SSDT) gibi alışık olduğunuz araçları kullanabilirsiniz / SQL Server Management Studio (SSMS) ve komut satırı yardımcı programları gibi `dtinstall` / `dtutil` / `dtexec`, dağıtın ve paketlerinizi Azure'da çalıştırın.

[Öğreticisi: Azure-SSIS IR sağlama](tutorial-create-azure-ssis-runtime-portal.md) Azure portal/ADF uygulaması aracılığıyla bir Azure-SSIS IR oluşturma ve isteğe bağlı olarak Azure SQL veritabanı sunucusu/yönetilen örnek SSISDB'yi barındırmak için kullanma işlemi gösterilmektedir. Bu makale öğreticiyi genişletip ve aşağıdaki işlemleri gösterilmektedir:

- İsteğe bağlı olarak sanal ağ hizmet uç noktaları/yönetilen örnek SSISDB'yi barındırmak için bir sanal ağ ile Azure SQL veritabanı sunucusu kullanın. Bir önkoşul olarak, Azure-SSIS IR'yi bir sanal ağa katılmak sanal ağın izinlerini/ayarlarını yapılandırmanız gerekir.

- İsteğe bağlı olarak Azure Active Directory (AAD) kimlik doğrulaması, ADF için yönetilen kimlik ile Azure SQL veritabanı sunucusu/yönetilen örnek bağlanmak için kullanın. Bir önkoşul olarak, yönetilen kimlik, ADF için bir veritabanı kullanıcısı SSISDB istemcilerinizle eklemeniz gerekir.

## <a name="overview"></a>Genel Bakış

Bu makalede, Azure-SSIS IR sağlama farklı yöntemler gösterilmektedir:

- [Azure portal](#azure-portal)
- [Azure PowerShell](#azure-powershell)
- [Azure Resource Manager şablonu](#azure-resource-manager-template)

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

- **Azure aboneliği**. Zaten bir aboneliğiniz yoksa, oluşturabileceğiniz bir [ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial/) hesabı.
- **Azure SQL veritabanı sunucusu/yönetilen örnek (isteğe bağlı)** . Bir veritabanı sunucusu zaten yoksa, başlamadan önce Azure portalında bir tane oluşturun. ADF sırayla SSISDB bu veritabanı sunucusunda oluşturur. Veritabanı sunucusunu tümleştirme çalışma zamanı ile aynı Azure bölgesinde oluşturmanız önerilir. Bu yapılandırma, tümleştirme çalışma zamanının Azure bölgelerinden geçmeden ssısdb'ye yürütme günlüklerini SSISDB yazma sağlar. 
    - Seçilen veritabanı sunucusuna göre SSISDB sizin adınıza tek veritabanı, elastik havuzun bir parçası veya Yönetilen Örnek biçiminde oluşturulabilir ve genel ağ üzerinden veya sanal ağa eklenerek erişilebilir. SSISDB'yi barındırmak için veritabanı sunucu türünü seçmek kılavuzluk etmesi için bkz [karşılaştırma Azure SQL veritabanı tek veritabanı/esnek havuz/yönetilen örnek](../data-factory/create-azure-ssis-integration-runtime.md#compare-sql-database-single-databaseelastic-pool-and-sql-database-managed-instance). Azure SQL veritabanı sunucusu ile sanal ağ hizmet uç noktaları/yönetilen örnek bir sanal ağdaki SSISDB barındırmak veya şirket içi verilere erişmesi için kullandığınız gerekirse, Azure-SSIS IR'yi bir sanal ağa katılın, bkz: [birleştirme, Azure-SSIS IR bir sanal ağ](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network).
    - Veritabanı sunucusunda **Azure hizmetlerine erişime izin ver** ayarının etkin olduğundan emin olun. Sanal ağ hizmet uç noktaları/yönetilen örnek SSISDB'yi barındırmak için bir sanal ağ ile Azure SQL veritabanı sunucusu kullandığınızda, bu geçerli değildir. Daha fazla bilgi için bkz. [Azure SQL veritabanınızın güvenliğini sağlama](../sql-database/sql-database-security-tutorial.md#create-firewall-rules). PowerShell kullanarak bu ayarı etkinleştirmek için bkz: [yeni AzSqlServerFirewallRule](/powershell/module/az.sql/new-azsqlserverfirewallrule).
    - İstemci makinenin IP adresini veya veritabanı sunucusu için Güvenlik Duvarı ayarlarındaki istemci IP adresi listesine istemci makinenin IP adresini içeren bir IP adresi aralığı ekleyin. Daha fazla bilgi için bkz. [Azure SQL Veritabanı'nda sunucu düzeyinde ve veritabanı düzeyinde güvenlik duvarı kuralları yapılandırma](../sql-database/sql-database-firewall-configure.md).
    - Sunucu yönetici kimlik bilgilerinizle SQL kimlik doğrulaması veya Azure Active Directory (AAD) kimlik doğrulaması için ADF ile yönetilen kimlik kullanarak veritabanı sunucusunda bağlanabilirsiniz.  İkincisi için gereken veritabanı sunucusuna erişim izni olan bir AAD gruba, ADF için yönetilen kimlik eklemek için bkz: [etkinleştirme AAD kimlik doğrulaması için Azure-SSIS IR](https://docs.microsoft.com/azure/data-factory/enable-aad-authentication-azure-ssis-ir).
    - Veritabanı sunucunuza bir SSISDB zaten sahip olmadığını onaylayın. Bir Azure-SSIS IR'nin sağlanması var olan bir SSISDB kullanma desteği olmamasıdır.
- **(İsteğe bağlı) Azure Resource Manager sanal ağı**. Aşağıdaki koşullardan biri doğru ise bir Azure Resource Manager sanal ağı olması gerekir:
    - Sanal ağ hizmet uç noktaları/yönetilen örnek bir sanal ağ ile Azure SQL veritabanı sunucusunda SSISDB barındırıyorsanız.
    - Şirket içi veri depoları, Azure-SSIS IR'yi üzerinde çalışan SSIS paketlerinden bağlanmak istediğiniz
- **Azure PowerShell (isteğe bağlı)** . Yönergeleri takip edin [Azure PowerShell'i yükleme ve yapılandırma konusunda](/powershell/azure/install-az-ps), isterseniz, Azure-SSIS IR sağlamak için bir PowerShell betiğini çalıştırmak

### <a name="region-support"></a>Bölge desteği

Azure bölgesi ile ADF ve Azure-SSIS IR'yi şu anda kullanılabilir bir listesi için bkz. [ADF + SSIS IR bölgelere göre kullanılabilirliği](https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all).

### <a name="compare-sql-database-single-databaseelastic-pool-and-sql-database-managed-instance"></a>SQL veritabanı tek veritabanı esnek havuzunu ve SQL veritabanı yönetilen örneği karşılaştırın

Bunlar SSIR Azure IR için ilgili olarak aşağıdaki tabloda Azure SQL veritabanı yönetilen örneği ve belirli özellikleri karşılaştırılır:

| Özellik | tek veritabanı elastik havuz| Yönetilen Örnek |
|---------|--------------|------------------|
| **Zamanlama** | SQL Server Aracısı kullanılamıyor.<br/><br/>Bkz: [ADF işlem hattındaki bir paket yürütmeyi zamanlama](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-schedule-packages?view=sql-server-2017#activity).| Yönetilen örnek aracı kullanılabilir. |
| **Kimlik Doğrulaması** | SSISDB ile ADF yönetilen kimliğe sahip tüm AAD grubu üyesi olarak temsil eden bir bağımsız veritabanı kullanıcısı oluşturabilirsiniz **db_owner** rol.<br/><br/>Bkz: [SSISDB Azure SQL veritabanı sunucusu oluşturmak için kimlik doğrulamasını etkinleştirme Azure AD'ye](enable-aad-authentication-azure-ssis-ir.md#enable-azure-ad-on-azure-sql-database). | SSISDB ile ADF yönetilen kimliğini temsil eden bağımsız veritabanı kullanıcısı oluşturabilirsiniz. <br/><br/>Bkz: [Azure SQL veritabanı yönetilen örneği'nde SSISDB oluşturmak için kimlik doğrulamasını etkinleştirme Azure AD'ye](enable-aad-authentication-azure-ssis-ir.md#enable-azure-ad-on-azure-sql-database-managed-instance). |
| **Hizmet katmanı** | Azure-SSIS IR, Azure SQL veritabanı sunucunuzu oluşturduğunuzda, hizmet katmanı için SSISDB seçebilirsiniz. Birden çok hizmet katmanı vardır. | Azure-SSIS IR ile yönetilen Örneğinize oluşturduğunuzda, hizmet katmanı için SSISDB seçemezsiniz. Yönetilen Örneğinize tüm veritabanlarını bu örneğe ayrılan aynı kaynak paylaşın. |
| **Sanal ağ** | Yalnızca Azure Resource Manager sanal ağları, sanal ağ hizmet uç noktaları ile Azure SQL veritabanı sunucusunu kullanabilir veya şirket içi veri depolarına erişmesi katılmak Azure-SSIS IR için destekler. | Yalnızca Azure Resource Manager sanal ağları Azure-SSIS IR katılmak için destekler. Sanal ağ, her zaman gerekli değildir.<br/><br/>Yönetilen Örneğinize aynı sanal ağ için Azure-SSIS IR katılırsanız, Azure-SSIS IR yönetilen Örneğinize değerinden farklı bir alt ağda olduğundan emin olun. Azure-SSIS IR yönetilen Örneğinize farklı bir sanal ağa katılın, bir sanal ağ eşlemesi veya sanal ağ için sanal ağ bağlantısı öneririz. Bkz: [uygulamanızı Azure SQL veritabanı yönetilen örneği bağlamak](../sql-database/sql-database-managed-instance-connect-app.md). |
| **Dağıtılmış işlemler** | Elastik işlemler desteklenir. Microsoft Dağıtılmış İşlem Düzenleyicisi (MSDTC) işlemler desteklenmez. SSIS paketlerinizi dağıtılmış işlemleri düzenlemesine olanak MSDTC kullanıyorsanız, Azure SQL veritabanı için elastik işlemler için geçiş göz önünde bulundurun. Daha fazla bilgi için bkz. [bulut veritabanlarında dağıtılmış işlemler](../sql-database/sql-database-elastic-transactions-overview.md). | Desteklenmiyor. |
| | | |

## <a name="azure-portal"></a>Azure portal

Bu bölümde, Azure portalı, özellikle ADF kullanıcı arabirimi (UI) kullanmanızı / Azure-SSIS IR oluşturmak için uygulama

### <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

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

    a. Üzerinde **oluşturma SSIS Kataloğu...**  dağıtım modeli Azure-SSIS IR üzerinde çalıştırmak paketler için onay kutusunu seçin: Veritabanı sunucunuz tarafından barındırılan SSISDB ile paketlerinin dağıtıldığı proje dağıtım modeli veya paket dağıtımda, dosya sistemleri/dosya paylaşımları/Azure'a paketlerinin dağıtıldığı dosya. Bunu işaretlerseniz, kendi veritabanı sunucusu oluşturmak ve yönetmek size SSISDB'yi barındırmak için sizin adınıza getirmek gerekir.
   
    b. **Subscription** (Abonelik) alanında SSISDB'yi barındıran Azure aboneliğini seçin. 

    c. **Location** (Konum) alanında SSISDB'yi barındıran veritabanı sunucunuzun konumunu seçin. Tümleştirme çalışma zamanınızla aynı konumu seçmenizi öneririz. 

    d. **Catalog Database Server Endpoint** (Katalog Veritabanı Sunucusu Uç Noktası) alanında SSISDB'yi barındıracak veritabanı sunucunuzun uç noktasını seçin. Seçilen veritabanı sunucusuna göre SSISDB sizin adınıza tek veritabanı, elastik havuzun bir parçası veya Yönetilen Örnek biçiminde oluşturulabilir ve genel ağ üzerinden veya sanal ağa eklenerek erişilebilir. SSISDB'yi barındırmak için veritabanı sunucu türünü seçmek kılavuzluk etmesi için bkz [karşılaştırma Azure SQL veritabanı tek veritabanı/esnek havuz/yönetilen örnek](../data-factory/create-azure-ssis-integration-runtime.md#compare-sql-database-single-databaseelastic-pool-and-sql-database-managed-instance). SSISDB barındırmak veya şirket içi verilere erişmesi için bir sanal ağdaki sanal ağ hizmet uç noktaları/yönetilen örnek ile Azure SQL veritabanı sunucusu seçin gerekirse, Azure-SSIS IR'yi bir sanal ağa katılın, bkz: [birleştirme Azure-SSIS IR bir sanal ağa](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network). 

    e. Üzerinde **kullanım AAD kimlik doğrulaması...**  onay kutusunu SSISDB'yi barındırmak için veritabanı sunucusu için kimlik doğrulama yöntemini seçin: SQL kimlik doğrulaması veya, ADF için AAD kimlik doğrulamasını yönetilen kimliğe sahip. Kaydetmeden gerekirse, veritabanı sunucunuza erişim izni olan bir AAD gruba, ADF için yönetilen kimlik eklemek için bkz: [etkinleştirme AAD kimlik doğrulaması için Azure-SSIS IR](https://docs.microsoft.com/azure/data-factory/enable-aad-authentication-azure-ssis-ir). 

    f. **Admin Username** (Yönetici Kullanıcı Adı) alanına SSISDB'yi barındıracak veritabanı sunucunuzun SQL kimlik doğrulaması kullanıcı adını girin. 

    g. **Admin Password** (Yönetici Parolası) alanına SSISDB'yi barındıracak veritabanı sunucunuzun SQL kimlik doğrulaması parolasını girin. 

    h. İçin **Katalog veritabanı hizmet katmanı**, veritabanı sunucunuza SSISDB'yi barındırmak için Hizmet katmanını seçin: Temel/standart/Premium katmanı veya elastik havuz adı. 

    i. **Test Connection** (Bağlantıyı Sına) öğesine tıklayın ve başarılı olursa **Next** (İleri) öğesine tıklayın. 

4. **Advanced Settings** (Gelişmiş Ayarlar) sayfasında aşağıdaki adımları tamamlayın:

    ![Gelişmiş ayarlar](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings.png)

    a. **Maximum Parallel Executions Per Node** (Düğüm Başına Maksimum Paralel Yürütme) alanında tümleştirme çalışma zamanınızda her düğümde eş zamanlı olarak yürütülecek maksimum paket sayısını seçin. Yalnızca desteklenen paket sayıları görüntülenir. İşlem/bellek tek bir büyük/ağır paketini çalıştırmak için birden fazla çekirdeği kullanmak istiyorsanız, düşük bir sayı seçin-yoğun. Tek bir çekirdekte bir veya daha fazla küçük/hafif paket çalıştırmak istiyorsanız daha yüksek bir sayı seçin.

    b. **Custom Setup Container SAS URI** (Özel Kurulum Kapsayıcısı SAS URI'si) alanında isteğe bağlı olarak kurulum betiğinizin ve ilgili dosyalarının depolandığı Azure Depolama Blobu kapsayıcısının Paylaşılan Erişim İmzası (SAS) Tekdüzen Kaynak Tanımlayıcısı (URI) değerini girebilirsiniz, bkz. [Azure-SSIS IR için özel kurulum](https://docs.microsoft.com/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup).

5. Üzerinde **bir sanal ağ seçin...**  onay kutusunu, tümleştirme çalışma zamanı bir sanal ağa katılmak isteyip istemediğinizi belirtin. Sanal ağ hizmet uç noktaları/yönetilen örnek SSISDB'yi barındırmak için bir sanal ağ ile Azure SQL veritabanı sunucusu kullanma veya şirket içi verilere erişim gerektiren kontrol edin; diğer bir deyişle, şirket içi veri kaynaklarınız/hedeflerini SSIS paketlerinizi bkz [katılın Azure-SSIS IR'yi bir sanal ağa](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network). Bunu işaretlerseniz, aşağıdaki adımları tamamlayın:

   ![Sanal ağ ile Gelişmiş ayarlar](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings-vnet.png)

    a. İçin **abonelik**, sanal ağınıza sahip Azure aboneliğini seçin.

    b. İçin **konumu**, Integration runtime'nın aynı konum seçilir.

    c. İçin **türü**, sanal ağınızı türünü seçin: Klasik veya Azure Resource Manager. Öneririz Azure Resource Manager sanal ağı seçin, sonra Klasik sanal ağ yakında kullanımdan kaldırılacak.

    d. İçin **VNet adı**, sanal ağınızın adını seçin. Bu sanal ağın sanal ağ hizmet uç noktaları/yönetilen örnek bir sanal ağ ile Azure SQL veritabanı sunucusu için SSISDB ya da şirket içi ağınıza bağlı bir barındırmak için kullanılan hizmet örneğiyle aynı olması gerekir.

    e. İçin **alt ağ adı**, sanal ağınızın alt ağ adını seçin. Bu yönetilen örneği için bir sanal ağda SSISDB'yi barındırmak için kullanılan farklı bir alt olmalıdır.

6. Tıklayın **VNet doğrulama** ve başarılı olursa, tıklayın **son** , Azure-SSIS tümleştirme çalışma zamanının oluşturulmasını başlatmak için.

    > [!NOTE]
    > Özel Kurulum saatlerine hariç bu işlemi 5 dakika içinde tamamlandı, ancak Azure-SSIS IR'yi bir sanal ağa bağlanmaya yaklaşık 20-30 dakika sürebilir.
    >
    > SSISDB kullanırsanız, ADF hizmet SSISDB hazırlamak için veritabanı sunucusuna bağlanır. Ayrıca belirtilmişse sanal ağınıza ilişkin izin ve ayarları yapılandırır ve Azure-SSIS IR sanal ağa katılır.
    > 
    > Bir Azure-SSIS IR sağladığınızda, Access Redistributable ve SSIS için Azure Feature Pack da yüklenir. Bu bileşenler, Excel/erişim dosyaları ve zaten yerleşik bileşenler tarafından desteklenen veri kaynaklarının yanı sıra çeşitli Azure veri kaynaklarıyla bağlantı sağlar. Ek bileşenleri de yükleyebilirsiniz bkz [Azure-SSIS IR için özel kurulum](how-to-configure-azure-ssis-ir-custom-setup.md).

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

Kopyalayın ve aşağıdaki betiği yapıştırın: değişkenlerin değerlerini belirtin. 

```powershell
### Azure Data Factory information
# If your input contains a PSH special character, e.g. "$", precede it with the escape character "`" like "`$"
$SubscriptionName = "[your Azure subscription name]"
$ResourceGroupName = "[your Azure resource group name]"
# Data factory name - Must be globally unique
$DataFactoryName = "[your data factory name]"
# For supported regions, see https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all
$DataFactoryLocation = "EastUS"

### Azure-SSIS integration runtime information - This is a Data Factory compute resource for running SSIS packages
$AzureSSISName = "[your Azure-SSIS IR name]"
$AzureSSISDescription = "[your Azure-SSIS IR description]"
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
$VnetId = "[your virtual network resource ID or leave it empty]" # REQUIRED if you use Azure SQL Database server with virtual network service endpoints/Managed Instance in a virtual network/on-premises data, Azure Resource Manager virtual network is recommended, Classic virtual network will be deprecated soon
$SubnetName = "[your subnet name or leave it empty]" # WARNING: Please use the same subnet as the one used with your Azure SQL Database server with virtual network service endpoints or a different subnet than the one used for your Managed Instance in a virtual network

### SSISDB info
$SSISDBServerEndpoint = "[your Azure SQL Database server name.database.windows.net or Managed Instance name.DNS prefix.database.windows.net or Managed Instance name.public.DNS prefix.database.windows.net,3342 or leave it empty if you do not use SSISDB]" # WARNING: If you use SSISDB, please ensure that there is no existing SSISDB on your database server, so we can prepare and manage one on your behalf
# Authentication info: SQL or Azure Active Directory (AAD)
$SSISDBServerAdminUserName = "[your server admin username for SQL authentication or leave it empty for AAD authentication]"
$SSISDBServerAdminPassword = "[your server admin password for SQL authentication or leave it empty for AAD authentication]"
# For the basic pricing tier, specify "Basic", not "B" - For standard/premium/elastic pool tiers, specify "S0", "S1", "S2", "S3", etc., see https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-database-server
$SSISDBPricingTier = "[Basic|S0|S1|S2|S3|S4|S6|S7|S9|S12|P1|P2|P4|P6|P11|P15|…|ELASTIC_POOL(name = <elastic_pool_name>) for Azure SQL Database server or leave it empty for Managed Instance]"
```

### <a name="sign-in-and-select-subscription"></a>Oturum açın ve aboneliği seçin

Komut dosyasını açıp Azure aboneliğinizi seçmek için aşağıdaki kodu ekleyin:

```powershell
Connect-AzAccount
Select-AzSubscription -SubscriptionName $SubscriptionName
```

### <a name="validate-the-connection-to-database-server"></a>Veritabanı sunucusu için bağlantıyı doğrulama

Azure SQL veritabanı sunucusu/yönetilen örnek doğrulamak için aşağıdaki betiği ekleyin.

```powershell
# Validate only if you use SSISDB and do not use VNet or AAD authentication
if(![string]::IsNullOrEmpty($SSISDBServerEndpoint))
{
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

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Oluşturma bir [Azure kaynak grubu](../azure-resource-manager/resource-group-overview.md) kullanarak [yeni AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) komutu. Kaynak grubu, Azure kaynaklarının grup olarak dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

Kaynak grubunuz zaten varsa, bu kodu betiğinize kopyalamayın. 

```powershell
New-AzResourceGroup -Location $DataFactoryLocation -Name $ResourceGroupName
```

### <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

Veri fabrikası oluşturmak için aşağıdaki komutu çalıştırın.

```powershell
Set-AzDataFactoryV2 -ResourceGroupName $ResourceGroupName `
                         -Location $DataFactoryLocation `
                         -Name $DataFactoryName
```

### <a name="create-an-integration-runtime"></a>Tümleştirme çalışma zamanı oluşturma

Azure'da SSIS paketlerini çalıştıran bir Azure-SSIS tümleştirme çalışma zamanı oluşturmak için aşağıdaki komutları çalıştırın.

SSISDB kullanmazsanız CatalogServerEndpoint CatalogPricingTier ve CatalogAdminCredential parametreleri atlayabilirsiniz.

Sanal ağ hizmet uç noktaları/yönetilen örnek SSISDB'yi barındırmak için bir sanal ağ ile Azure SQL veritabanı sunucusu kullanın ya da şirket içi verilere erişim gerektiren Vnetıd ve alt ağ parametrelerini atlayın veya boş değerler geçirileceğini. Aksi takdirde, olamaz bunları çıkarın ve gerekir geçerli değerler, sanal ağ yapılandırmasından geçmek için bkz: [katılın Azure-SSIS IR'yi bir sanal ağa](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network).

SSISDB'yi barındırmak için yönetilen örneği kullanıyorsanız, CatalogPricingTier parametreyi atlarsanız veya bunun için boş bir değer geçirin. Aksi takdirde, olamaz atlayın ve gerekir geçerli bir değer listesinden fiyatlandırma katmanları Azure SQL veritabanı için desteklenen geçmek için bkz: [SQL veritabanı kaynak limitleri](../sql-database/sql-database-resource-limits.md).

Azure Active Directory (AAD) kimlik doğrulaması ile ADF için yönetilen kimlik veritabanı sunucusuna bağlanmak için kullandığınız, CatalogAdminCredential parametreyi atlayabilirsiniz, ancak bir AAD grubuna erişim ile ADF için yönetilen kimlik eklemeniz gerekir Veritabanı sunucusu için izinleri görüntüle [etkinleştirme AAD kimlik doğrulaması için Azure-SSIS IR](https://docs.microsoft.com/azure/data-factory/enable-aad-authentication-azure-ssis-ir). Aksi takdirde, atlayamazsınız ve Sunucu Yöneticisi kullanıcı adı ve parola SQL kimlik doğrulaması için oluşturulmuş geçerli bir nesne geçmesi gerekir.

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
                                           -Subnet $SubnetName
       
# Add CatalogServerEndpoint, CatalogPricingTier, and CatalogAdminCredential parameters if you use SSISDB
if(![string]::IsNullOrEmpty($SSISDBServerEndpoint))
{
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -CatalogServerEndpoint $SSISDBServerEndpoint `
                                               -CatalogPricingTier $SSISDBPricingTier

    if(![string]::IsNullOrEmpty($SSISDBServerAdminUserName) –and ![string]::IsNullOrEmpty($SSISDBServerAdminPassword)) # Add CatalogAdminCredential parameter if you do not use AAD authentication
    {
        $secpasswd = ConvertTo-SecureString $SSISDBServerAdminPassword -AsPlainText -Force
        $serverCreds = New-Object System.Management.Automation.PSCredential($SSISDBServerAdminUserName, $secpasswd)

        Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                                   -DataFactoryName $DataFactoryName `
                                                   -Name $AzureSSISName `
                                                   -CatalogAdminCredential $serverCreds
    }
}

# Add SetupScriptContainerSasUri parameter if you use custom setup
if(![string]::IsNullOrEmpty($SetupScriptContainerSasUri))
{
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -SetupScriptContainerSasUri $SetupScriptContainerSasUri
}
```

### <a name="start-integration-runtime"></a>Tümleştirme çalışma zamanını başlatma

Azure-SSIS tümleştirme çalışma zamanını başlatmak için aşağıdaki komutları çalıştırın.

```powershell
write-host("##### Starting #####")
Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

write-host("##### Completed #####")
write-host("If any cmdlet is unsuccessful, please consider using -Debug option for diagnostics.")
```

> [!NOTE]
> Özel Kurulum saatlerine hariç bu işlemi 5 dakika içinde tamamlandı, ancak Azure-SSIS IR'yi bir sanal ağa bağlanmaya yaklaşık 20-30 dakika sürebilir.
>
> SSISDB kullanırsanız, ADF hizmet SSISDB hazırlamak için veritabanı sunucusuna bağlanır. Ayrıca belirtilmişse sanal ağınıza ilişkin izin ve ayarları yapılandırır ve Azure-SSIS IR sanal ağa katılır.
> 
> Bir Azure-SSIS IR sağladığınızda, Access Redistributable ve SSIS için Azure Feature Pack da yüklenir. Bu bileşenler, Excel/erişim dosyaları ve zaten yerleşik bileşenler tarafından desteklenen veri kaynaklarının yanı sıra çeşitli Azure veri kaynaklarıyla bağlantı sağlar. Ek bileşenleri de yükleyebilirsiniz bkz [Azure-SSIS IR için özel kurulum](how-to-configure-azure-ssis-ir-custom-setup.md).

### <a name="full-script"></a>Tam betik

Bir Azure-SSIS tümleştirme çalışma zamanı oluşturur tam komut dosyası aşağıda verilmiştir.

```powershell
### Azure Data Factory information
# If your input contains a PSH special character, e.g. "$", precede it with the escape character "`" like "`$"
$SubscriptionName = "[your Azure subscription name]"
$ResourceGroupName = "[your Azure resource group name]"
# Data factory name - Must be globally unique
$DataFactoryName = "[your data factory name]"
# For supported regions, see https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all
$DataFactoryLocation = "EastUS"

### Azure-SSIS integration runtime information - This is a Data Factory compute resource for running SSIS packages
$AzureSSISName = "[your Azure-SSIS IR name]"
$AzureSSISDescription = "[your Azure-SSIS IR description]"
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
$VnetId = "[your virtual network resource ID or leave it empty]" # REQUIRED if you use Azure SQL Database server with virtual network service endpoints/Managed Instance in a virtual network/on-premises data, Azure Resource Manager virtual network is recommended, Classic virtual network will be deprecated soon
$SubnetName = "[your subnet name or leave it empty]" # WARNING: Please use the same subnet as the one used with your Azure SQL Database server with virtual network service endpoints or a different subnet than the one used for your Managed Instance in a virtual network

### SSISDB info
$SSISDBServerEndpoint = "[your Azure SQL Database server name.database.windows.net or Managed Instance name.DNS prefix.database.windows.net or Managed Instance name.public.DNS prefix.database.windows.net,3342 or leave it empty if you do not use SSISDB]" # WARNING: If you use SSISDB, please ensure that there is no existing SSISDB on your database server, so we can prepare and manage one on your behalf
# Authentication info: SQL or Azure Active Directory (AAD)
$SSISDBServerAdminUserName = "[your server admin username for SQL authentication or leave it empty for AAD authentication]"
$SSISDBServerAdminPassword = "[your server admin password for SQL authentication or leave it empty for AAD authentication]"
# For the basic pricing tier, specify "Basic", not "B" - For standard/premium/elastic pool tiers, specify "S0", "S1", "S2", "S3", etc., see https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-database-server
$SSISDBPricingTier = "[Basic|S0|S1|S2|S3|S4|S6|S7|S9|S12|P1|P2|P4|P6|P11|P15|…|ELASTIC_POOL(name = <elastic_pool_name>) for Azure SQL Database server or leave it empty for Managed Instance]"

### Sign in and select subscription
Connect-AzAccount
Select-AzSubscription -SubscriptionName $SubscriptionName

### Validate the connection to database server
# Validate only if you use SSISDB and do not use VNet or AAD authentication
if(![string]::IsNullOrEmpty($SSISDBServerEndpoint))
{
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
                                           -Subnet $SubnetName
       
# Add CatalogServerEndpoint, CatalogPricingTier, and CatalogAdminCredential parameters if you use SSISDB
if(![string]::IsNullOrEmpty($SSISDBServerEndpoint))
{
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -CatalogServerEndpoint $SSISDBServerEndpoint `
                                               -CatalogPricingTier $SSISDBPricingTier

    if(![string]::IsNullOrEmpty($SSISDBServerAdminUserName) –and ![string]::IsNullOrEmpty($SSISDBServerAdminPassword)) # Add CatalogAdminCredential parameter if you do not use AAD authentication
    {
        $secpasswd = ConvertTo-SecureString $SSISDBServerAdminPassword -AsPlainText -Force
        $serverCreds = New-Object System.Management.Automation.PSCredential($SSISDBServerAdminUserName, $secpasswd)

        Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                                   -DataFactoryName $DataFactoryName `
                                                   -Name $AzureSSISName `
                                                   -CatalogAdminCredential $serverCreds
    }
}

# Add SetupScriptContainerSasUri parameter when you use custom setup
if(![string]::IsNullOrEmpty($SetupScriptContainerSasUri))
{
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -SetupScriptContainerSasUri $SetupScriptContainerSasUri
}

### Start integration runtime
write-host("##### Starting #####")
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

> [!NOTE]
> Özel Kurulum saatlerine hariç bu işlemi 5 dakika içinde tamamlandı, ancak Azure-SSIS IR'yi bir sanal ağa bağlanmaya yaklaşık 20-30 dakika sürebilir.
>
> SSISDB kullanırsanız, ADF hizmet SSISDB hazırlamak için veritabanı sunucusuna bağlanır. Ayrıca belirtilmişse sanal ağınıza ilişkin izin ve ayarları yapılandırır ve Azure-SSIS IR sanal ağa katılır.
> 
> Bir Azure-SSIS IR sağladığınızda, Access Redistributable ve SSIS için Azure Feature Pack da yüklenir. Bu bileşenler, Excel/erişim dosyaları ve zaten yerleşik bileşenler tarafından desteklenen veri kaynaklarının yanı sıra çeşitli Azure veri kaynaklarıyla bağlantı sağlar. Ek bileşenleri de yükleyebilirsiniz bkz [Azure-SSIS IR için özel kurulum](how-to-configure-azure-ssis-ir-custom-setup.md).

## <a name="deploy-ssis-packages"></a>SSIS paketlerini dağıtma

SSISDB kullanırsanız, paketlerinizi dağıtmayı ve Azure-SSIS IR, sunucu uç noktası aracılığıyla veritabanı sunucunuza bağlanmasına SSDT/SSMS araçlarını kullanarak çalıştırın.  Azure SQL veritabanı sunucusu/yönetilen için örnek bir genel uç noktası ile bir sanal ağ/yönetilen örnek olarak, sunucu uç noktası biçimdir `<server name>.database.windows.net` / `<server name>.<dns prefix>.database.windows.net` / `<server name>.public.<dns prefix>.database.windows.net,3342`sırasıyla. SSISDB kullanmazsanız, paketlerinizi dosya sistemleri/dosya paylaşımları/Azure'a dağıtabilirsiniz dosyaları ve bunları Azure-SSIS IR kullanarak çalıştırın `dtinstall` / `dtutil` / `dtexec` komut satırı yardımcı programları. Daha fazla bilgi için [dağıtma SSIS paketlerini](/sql/integration-services/packages/deploy-integration-services-ssis-projects-and-packages#deploy-packages-to-integration-services-server). Her iki durumda da Azure-SSIS IR üzerinde dağıtılan paketlerinizi çalıştırabilirsiniz ADF hatlarında kullanarak SSIS paketi yürütme etkinliği bkz [çağırma SSIS paketi yürütme birinci sınıf bir ADF etkinlik olarak](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity).

## <a name="next-steps"></a>Sonraki adımlar

Ayrıca bu belgeleri diğer Azure-SSIS IR konulara bakın:

- [Azure-SSIS tümleştirme çalışma zamanı](concepts-integration-runtime.md#azure-ssis-integration-runtime). Bu makalede genel Azure-SSIS IR'yi dahil olmak üzere bir tümleştirme çalışma zamanları hakkında bilgi sağlar.
- [Azure-SSIS IR'yi izleme](monitor-integration-runtime.md#azure-ssis-integration-runtime). Bu makalede, Azure-SSIS IR'yi hakkında bilgi almak ve nasıl gösterir
- [Azure-SSIS IR'yi yönetme](manage-azure-ssis-integration-runtime.md). Bu makalede, durdurma, başlatma veya Azure-SSIS IR silme işlemini göstermektedir: Ayrıca, daha fazla düğüm ekleyerek Azure-SSIS IR ölçeklendirme işlemini göstermektedir.
- [Azure-SSIS IR'yi bir sanal ağa ekleme](join-azure-ssis-integration-runtime-virtual-network.md). Bu makalede, Azure-SSIS IR'yi bir sanal ağa birleştirme hakkında bilgi sağlar.