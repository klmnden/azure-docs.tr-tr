---
title: Bir sanal ağa Azure SSIS tümleştirmesi çalışma zamanı katılma | Microsoft Docs
description: Bir Azure sanal ağı Azure SSIS tümleştirmesi çalışma zamanı katılma öğrenin.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/13/2018
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: fa496271d949f131da53a4cdab0f3b9a15e82007
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36268146"
---
# <a name="join-an-azure-ssis-integration-runtime-to-a-virtual-network"></a>Bir Azure SSIS tümleştirmesi çalışma zamanı sanal bir ağa katılmasını sağlayın
Azure sanal ağını aşağıdaki senaryolarda, Azure SSIS tümleştirmesi çalışma zamanı (IR) Katıl: 

- Şirket içi veri depolarına bir Azure-SSIS tümleştirme çalışma zamanı üzerinde çalışan SSIS paketlerinden bağlanmak istiyorsanız.

- Sanal ağ hizmet uç noktaları/yönetilen örneği (Önizleme) ile Azure SQL veritabanında SQL Server Integration Services (SSIS) Katalog veritabanı barındırır.

 Azure Data Factory sürüm 2 (Önizleme), Klasik dağıtım modeli veya Azure Resource Manager dağıtım modeli oluşturulan sanal bir ağa, Azure SSIS tümleştirmesi çalışma zamanı katılma olanak tanır. 

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel kullanılabilirlik (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [Data Factory sürüm 1 belgelerine](v1/data-factory-introduction.md).

## <a name="access-to-on-premises-data-stores"></a>Şirket içi veri depolarına erişim
SSIS paketleri yalnızca genel bulut veri depolarına erişirseniz, bir sanal ağa Azure SSIS IR katılma gerek yoktur. SSIS paketleri şirket içi veri depolarına erişirse, şirket içi ağı'na bağlı bir sanal ağ Azure SSIS IR katılması gerekir. 

Dikkat edilecek bazı önemli noktalar şunlardır: 

- Varsa var olan bir sanal ağı şirket içi ağınıza bağlı, ilk Oluştur bir [Azure Resource Manager sanal ağ](../virtual-network/quick-create-portal.md#create-a-virtual-network) veya [Klasik sanal ağ](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) Azure SSIS tümleştirme katılmak için çalışma zamanı. Ardından, bir site siteye yapılandırın [VPN ağ geçidi bağlantısı](../vpn-gateway/vpn-gateway-howto-site-to-site-classic-portal.md) veya [ExpressRoute](../expressroute/expressroute-howto-linkvnet-classic.md) bu sanal ağ bağlantısından şirket içi ağınıza.
- Mevcut Azure Resource Manager veya Klasik sanal ağa, Azure SSIS IR ile aynı konumda şirket içi ağınıza bağlı ise, bu sanal ağa IR birleştirebilirsiniz.
- Mevcut Klasik sanal ağda, Azure SSIS IR farklı bir konumda şirket içi ağınıza bağlı ise, ilk oluşturabileceğiniz bir [Klasik sanal ağı](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) katılmak, Azure SSIS IR için. Ardından, yapılandırma bir [Klasik Klasik sanal ağda](../vpn-gateway/vpn-gateway-howto-vnet-vnet-portal-classic.md) bağlantı. Veya oluşturabileceğiniz bir [Azure Resource Manager sanal ağ](../virtual-network/quick-create-portal.md#create-a-virtual-network) katılmak, Azure SSIS tümleştirmesi çalışma zamanı için. Daha sonra yapılandırma bir [Klasik Azure Resource Manager sanal ağ](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md) bağlantı.
- Mevcut Azure Resource Manager sanal ağ, Azure SSIS IR farklı bir konumda şirket içi ağınıza bağlı ise, ilk oluşturabileceğiniz bir [Azure Resource Manager sanal ağ](../virtual-network/quick-create-portal.md##create-a-virtual-network) Azure-SSIS için Katılmak için IR. Ardından, bir Azure Kaynak Yöneticisi Azure Resource Manager sanal ağ bağlantısını yapılandırın. Veya oluşturabileceğiniz bir [Klasik sanal ağı](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) katılmak, Azure SSIS IR için. Ardından, yapılandırma bir [Klasik Azure Resource Manager sanal ağ](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md) bağlantı.

## <a name="host-the-ssis-catalog-database-in-azure-sql-database-with-virtual-network-service-endpointsmanaged-instance-preview"></a>Konak sanal ağ hizmet uç noktaları/yönetilen örneği (Önizleme) ile Azure SQL veritabanında SSIS Katalog veritabanı
SSIS katalog sanal ağ hizmet uç noktaları/yönetilen ile örneği (Önizleme) Azure SQL veritabanı'nda barındırılıyorsa, Azure SSIS IR birleştirebilirsiniz:

- Aynı sanal ağ
- Sanal ağ hizmet uç noktaları/yönetilen ile örneği (Önizleme) Azure SQL veritabanı için kullanılan tek bir ağ ağ bağlantısına sahip farklı bir sanal ağ

Sanal ağı Klasik dağıtım modeli veya Azure Resource Manager dağıtım modeli dağıtılabilir. Azure SSIS IR katılma planlıyorsanız *aynı sanal ağ* , zaten katılırsa (Önizleme) örneği tarafından yönetilen, Azure SSIS IR olduğundan emin olun bir *farklı alt* kullanılan bir (Önizleme) yönetilen örneği için.   

Aşağıdaki bölümlerde daha ayrıntılı bilgi sağlar.

## <a name="requirements-for-virtual-network-configuration"></a>Sanal ağ yapılandırması için gereksinimleri
-   Olduğundan emin olun `Microsoft.Batch` Azure SSIS IR barındıran sanal ağ alt ağınızı abonelik altında kayıtlı bir sağlayıcı Klasik sanal ağı kullanıyorsanız, aynı zamanda katılma `MicrosoftAzureBatch` bu sanal ağ için Klasik sanal makine Katılımcısı rolüne.

-   Azure SSIS IR barındırmak için uygun alt ağ seçin Bkz: [alt ağ seçin](#subnet).

-   Sanal ağ kendi etki alanı adı Hizmetleri (DNS) sunucusu kullanıyorsanız, bkz: [etki alanı adı Hizmetleri sunucusu](#dns_server).

-   Alt ağda bir ağ güvenlik grubu (NSG) kullanıyorsanız, bkz: [ağ güvenlik grubu](#nsg)

-   Azure hızlı rota kullanarak veya kullanıcı tanımlı yönlendirme (UDR) yapılandırma, bkz: [kullanım Azure ExpressRoute ya da kullanıcı tanımlanan yol](#route).

-   Sanal ağ kaynak grubu oluşturabilir ve belirli Azure ağ kaynaklarını silebilirsiniz emin olun. Bkz: [kaynak grubu için gereksinimleri](#resource-group).

### <a name="subnet"></a> Alt ağ seçin
-   Sanal ağ geçitleri için ayrılmış olduğundan, bir Azure SSIS tümleştirmesi çalışma zamanı dağıtmak için GatewaySubnet seçmeyin.

-   Seçtiğiniz alt kullanmak Azure SSIS IR için yeterli kullanılabilir adresi alanı olduğundan emin olun. En az 2 bırakın * kullanılabilir IP adresleri IR düğüm sayısı. Her alt ağ içindeki bazı IP adreslerini Azure ayırır ve bu adresleri kullanılamaz. Alt ağlar ilk ve son IP adreslerini Azure Hizmetleri için kullanılan üç daha fazla adres birlikte Protokolü uyum için ayrılmıştır. Daha fazla bilgi için bkz: [bu alt ağ içindeki IP adresleri kullanma kısıtlamaları vardır?](../virtual-network/virtual-networks-faq.md#are-there-any-restrictions-on-using-ip-addresses-within-these-subnets).

-   Diğer Azure Hizmetleri tarafından (örneğin, SQL veritabanı yönetilen örneği (Önizleme), uygulama hizmeti, vb.) özel olarak dolu bir alt ağın kullanmayın.

### <a name="dns_server"></a> Etki alanı adı Hizmetleri sunucusu 
Azure SSIS tümleştirmesi çalışma zamanı tarafından birleştirilmiş bir sanal ağ içinde kendi etki alanı adı Hizmetleri (DNS) sunucusu kullanmanız gerekiyorsa, ortak Azure ana bilgisayar adları çözümlemek emin olun (örneğin, bir Azure depolama blob adı, `<your storage account>.blob.core.windows.net`).

Aşağıdaki adımları önerilir:

-   Azure DNS'ye isteklerini iletmek için özel DNS yapılandırın. Azure'nın özyinelemeli Çözümleyicileri (168.63.129.16) IP adresine çözümlenmemiş DNS kayıtlarını kendi DNS sunucusuna iletebilir.

-   Özel DNS birincil olarak ve Azure DNS sanal ağ için ikincil olarak ayarlayın. Kendi DNS sunucusu kullanılamaz durumda Azure'un özyinelemeli Çözümleyicileri (168.63.129.16) IP adresini ikincil DNS sunucusu olarak kaydedin.

Daha fazla bilgi için bkz: [ad kendi DNS sunucusunu kullanır çözümlemesi](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server).

### <a name="nsg"></a> Ağ güvenlik grubu
Gelen/giden trafik bir ağ güvenlik grubu (NSG) uygulamak, Azure SSIS tümleştirmesi çalışma zamanı tarafından birleştirilmiş bir sanal ağdaki gerekiyorsa, aşağıdaki bağlantı noktaları izin ver:

| Yön | Aktarım Protokolü | Kaynak | Kaynak bağlantı noktası aralığı | Hedef | Hedef bağlantı noktası aralığı | Yorumlar |
|---|---|---|---|---|---|---|
| Gelen | TCP | Internet | * | VirtualNetwork | 29876, (bir Azure Resource Manager sanal ağı'de IR katılırsanız) 29877 <br/><br/>10100, 20100, (bir Klasik sanal ağı'de IR katılırsanız) 30100| Data Factory hizmetinin bu bağlantı noktaları sanal ağda Azure SSIS Integration zamanının düğümleriyle iletişim kurmak için kullanır. <br/><br/> Bir NSG'yi veya belirttiğiniz olsun, veri fabrikası her zaman bir NSG Azure SSIS IR barındıran sanal makinelere bağlı ağ arabirimi kartlarıyla (NIC) düzeyinde yapılandırır Yalnızca veri fabrikası IP adreslerinden gelen trafiğe izin verilir. Internet trafiği için bu bağlantı noktalarını açmak olsa bile, veri fabrikası IP adresleri değil IP adreslerinden gelen trafiğin NIC düzeyinde engellendi. |
| Giden | TCP | VirtualNetwork | * | Internet | 443 | Sanal ağda Azure SSIS Integration zamanının düğümleri, Azure Storage ve Azure Event Hubs gibi Azure hizmetlerine erişmek için bu bağlantı noktasını kullanır. |
| Giden | TCP | VirtualNetwork | * | Internet veya Sql | 11000 11999, 14000 14999 1433 | Azure SQL veritabanı sunucunuz tarafından - SSISDB erişmek için bu bağlantı noktaları barındırılan sanal ağ kullanımda Azure SSIS Integration zamanının düğümleri bu amaçla yönetilen örneği (Önizleme) tarafından barındırılan SSISDB için geçerli değildir. |
||||||||

### <a name="route"></a> Azure ExpressRoute veya kullanıcı tanımlı yol kullanın
Bağlanabileceğiniz bir [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/) şirket içi ağınızı Azure'a genişletmek için sanal ağ altyapınızı devresine. 

Zorlamalı tünel kullanmak için genel bir yapılandırma olduğundan (0.0.0.0/0 sanal ağ için bir BGP rota tanıtma) hangi zorlar giden Internet trafiği sanal ağ akışından denetleme ve günlüğe kaydetme için şirket içi ağ Gereci için. Bu trafik akışı sanal ağda bağımlı Azure Data Factory Hizmetleri ile Azure SSIS IR arasındaki bağlantıyı keser. Çözümü bir (veya daha fazla) tanımlamaktır [kullanıcı tanımlı yollar (Udr'ler)](../virtual-network/virtual-networks-udr-overview.md) Azure SSIS IR içeren alt ağda Bir UDR BGP yolu yerine dikkate alınır alt özel yollar tanımlar.

Veya denetleme ve günlüğe kaydetme için kullanıcı tanımlı yollar giden Internet trafiği bir güvenlik duvarı veya bir DMZ konak sanal bir ağ uygulaması barındıran başka bir alt ağ için Azure SSIS IR barındıran alt ağdan zorlamak için (Udr'ler) tanımlayabilirsiniz.

Her iki durumda da, sonraki atlama türü ile 0.0.0.0/0 rota uygulamak **Internet** Azure SSIS IR barındıran ve böylece Data Factory hizmeti olan Azure SSIS IR arasındaki iletişimi başarılı olabilmesi için alt ağdaki. 

![Bir rota ekleyin](media/join-azure-ssis-integration-runtime-virtual-network/add-route-for-vnet.png)

Alt giden Internet trafiği incelemek için özelliğini kaybetme endişelisiniz, ayrıca bir NSG kuralı Giden hedeflere kısıtlamak için alt ağdaki ekleyebileceğiniz [Azure veri merkezi IP adreslerini](https://www.microsoft.com/download/details.aspx?id=41653).

Bkz: [bu PowerShell Betiği](https://gallery.technet.microsoft.com/scriptcenter/Adds-Azure-Datacenter-IP-dbeebe0c) bir örnek. Azure veri merkezi IP adres listesinde güncel tutmak üzere haftalık komut dosyasını çalıştırmanız gerekir.

### <a name="resource-group"></a> Kaynak grubu için gereksinimler
Azure SSIS IR Azure yük dengeleyici, Azure ortak IP adresi ve ağ iş güvenlik grubu dahil olmak üzere sanal ağ ile aynı kaynak grubunda belirli ağ kaynaklarınıza oluşturması gerekir.

-   Kaynak grubuna veya aboneliğe ait olduğu sanal ağ üzerindeki tüm kaynak kilit yok emin olun. Salt okunur bir kilidi ya da delete kilit yapılandırırsanız, başlatma ve durdurma IR askıda veya başarısız.

-   Aşağıdaki kaynaklar kaynak grubunu veya sanal ağının ait olduğu abonelik altında oluşturulan önleyen bir Azure ilke yok emin olun:
    -   Microsoft.Network/LoadBalancers
    -   Microsoft.Network/NetworkSecurityGroups
    -   Microsoft.Network/PublicIPAddresses

## <a name="azure-portal-data-factory-ui"></a>Azure portal (Data Factory UI)
Bu bölümde Azure portalı ve veri fabrikası UI kullanarak bir sanal ağ (Klasik veya Azure Resource Manager) var olan bir Azure SSIS çalışma zamanı katılma gösterilmiştir. İlk olarak, Azure SSIS IR katılmadan önce sanal ağ uygun şekilde yapılandırmanız gerekir. Sanal ağ (Klasik veya Azure Resource Manager) türüne göre sonraki iki bölümde biri aracılığıyla gidin. Ardından, sanal ağa, Azure SSIS IR katılmaya üçüncü bölümüyle devam edin. 

### <a name="use-the-portal-to-configure-an-azure-resource-manager-virtual-network"></a>Bir Azure Resource Manager sanal ağı yapılandırmak üzere portalı kullanın
Bir sanal ağ için bir Azure SSIS IR katılabilmesi için önce yapılandırmanız gerekir.

1. Microsoft Edge veya Google Chrome başlatın. Şu anda, veri fabrikası UI yalnızca bu web tarayıcılarda desteklenir.
2. [Azure Portal](https://portal.azure.com) oturum açın.
3. Seçin **daha fazla hizmet**. Filtreleyip seçmek **sanal ağlar**.
4. Filtre ve sanal ağınız listeden seçin. 
5. Üzerinde **sanal ağ** sayfasında, **özellikleri**. 
6. Kopyala düğmesini seçin **kaynak kimliği** sanal ağ için kaynak kimliği panoya kopyalamak için. OneNote veya dosyasında panodan kimliği kaydedin.
7. Seçin **alt ağlar** sol menüde. Sayısı emin **kullanılabilir adresler** Azure SSIS tümleştirmesi Çalışma Zamanı Modülü düğümlerin değerinden daha büyük.
8. Azure Batch sağlayıcısı sanal ağ olan Azure aboneliğinde kayıtlı olduğunu doğrulayın. Veya, Azure Batch sağlayıcıyı kaydettirin. Bir Azure Batch hesabı aboneliğinizde zaten varsa, aboneliğiniz için Azure Batch kayıtlı değil. (Data Factory Portalı'nda Azure SSIS IR oluşturursanız, Azure Batch sağlayıcısı otomatik olarak sizin için kaydedilir.)

   a. Azure portalında seçin **abonelikleri** sol menüde.

   b. Aboneliğinizi seçin. 
   
   c. Seçin **kaynak sağlayıcıları** sol taraftaki onaylayın **Microsoft.Batch** kayıtlı bir sağlayıcı.
      !["Kaydedildi" durumunu onaylama](media/join-azure-ssis-integration-runtime-virtual-network/batch-registered-confirmation.png)
      
   Görmüyorsanız, **Microsoft.Batch** , kaydedilecek listesinde [boş bir Azure Batch hesabı oluşturma](../batch/batch-account-create-portal.md) aboneliğinizde. Daha sonra silebilirsiniz.

### <a name="use-the-portal-to-configure-a-classic-virtual-network"></a>Klasik sanal ağı yapılandırmak üzere portalı kullanın
Bir sanal ağ için bir Azure SSIS IR katılabilmesi için önce yapılandırmanız gerekir.

1. Microsoft Edge veya Google Chrome başlatın. Şu anda, veri fabrikası UI yalnızca bu web tarayıcılarda desteklenir.
2. [Azure Portal](https://portal.azure.com) oturum açın.
3. Seçin **daha fazla hizmet**. Filtreleyip seçmek **sanal ağları (Klasik)**.
4. Filtre ve sanal ağınız listeden seçin. 
5. Üzerinde **sanal ağ (Klasik)** sayfasında, **özellikleri**.
    ![Klasik sanal ağ kaynak kimliği](media/join-azure-ssis-integration-runtime-virtual-network/classic-vnet-resource-id.png)
    
6. Kopyala düğmesini seçin **kaynak kimliği** Klasik ağ için kaynak kimliği panoya kopyalamak için. OneNote veya dosyasında panodan kimliği kaydedin.
7. Seçin **alt ağlar** sol menüde. Sayısı emin **kullanılabilir adresler** Azure SSIS tümleştirmesi Çalışma Zamanı Modülü düğümlerin değerinden daha büyük.
    ![Sanal ağda kullanılabilir adreslerin sayısı](media/join-azure-ssis-integration-runtime-virtual-network/number-of-available-addresses.png)
    
8. Katılma **MicrosoftAzureBatch** için **Klasik sanal makine Katılımcısı** rolü sanal ağ için.

    a. Seçin **erişim denetimi (IAM)** soldaki menüden ve seçim **Ekle** araç çubuğunda.
      !["Erişim denetimi" ve "düğme ekleme"](media/join-azure-ssis-integration-runtime-virtual-network/access-control-add.png)
      
    b. Üzerinde **izinleri eklemek** sayfasında, **Klasik sanal makine Katılımcısı** için **rol**. Yapıştır **ddbf3205-c6bd-46ae-8127-60eb93363864** içinde **seçin** kutusuna ve ardından **Microsoft Azure toplu işlem** arama sonuçları listesinden.
      !["İzinleri Ekle" sayfasında arama sonuçları](media/join-azure-ssis-integration-runtime-virtual-network/azure-batch-to-vm-contributor.png)
      
    c. Seçin **kaydetmek** ayarlarını kaydetmek için ve sayfasını kapatın.
      ![Erişim ayarlarını Kaydet](media/join-azure-ssis-integration-runtime-virtual-network/save-access-settings.png)
      
    d. Gördüğünüzü onaylayın **Microsoft Azure toplu işlem** katkıda bulunanlar listesinde.
      ![Azure Batch erişimi onaylayın](media/join-azure-ssis-integration-runtime-virtual-network/azure-batch-in-list.png)
      
9. Azure Batch sağlayıcısı sanal ağ olan Azure aboneliğinde kayıtlı olduğunu doğrulayın. Veya, Azure Batch sağlayıcıyı kaydettirin. Bir Azure Batch hesabı aboneliğinizde zaten varsa, aboneliğiniz için Azure Batch kayıtlı değil. (Data Factory Portalı'nda Azure SSIS IR oluşturursanız, Azure Batch sağlayıcısı otomatik olarak sizin için kaydedilir.)

   a. Azure portalında seçin **abonelikleri** sol menüde.

   b. Aboneliğinizi seçin.

   c. Seçin **kaynak sağlayıcıları** sol taraftaki onaylayın **Microsoft.Batch** kayıtlı bir sağlayıcı.
      !["Kaydedildi" durumunu onaylama](media/join-azure-ssis-integration-runtime-virtual-network/batch-registered-confirmation.png)
      
   Görmüyorsanız, **Microsoft.Batch** , kaydedilecek listesinde [boş bir Azure Batch hesabı oluşturma](../batch/batch-account-create-portal.md) aboneliğinizde. Daha sonra silebilirsiniz. 

### <a name="join-the-azure-ssis-ir-to-a-virtual-network"></a>Bir sanal ağa Azure SSIS IR katılma
1. Microsoft Edge veya Google Chrome başlatın. Şu anda, veri fabrikası UI yalnızca bu web tarayıcılarda desteklenir.
2. İçinde [Azure portal](https://portal.azure.com)seçin **veri fabrikaları** sol menüde. Görmüyorsanız, **veri fabrikaları** menüsünde seçin **daha fazla hizmet**seçip **veri fabrikaları** içinde **INTELLİGENCE + ANALİZ**bölümü. 
    
   ![Veri fabrikaları listesi](media/join-azure-ssis-integration-runtime-virtual-network/data-factories-list.png)
2. Data factory'nizi Azure SSIS tümleştirmesi çalışma zamanı listeden seçin. Veri fabrikanızın giriş sayfasına bakın. Seçin **Yazar & Dağıt** döşeme. Veri Fabrikası UI ayrı bir sekmede bakın. 

   ![Data factory giriş sayfası](media/join-azure-ssis-integration-runtime-virtual-network/data-factory-home-page.png)
3. Veri Fabrikası UI geçmek **Düzenle** sekmesine **bağlantıları**ve geçiş **tümleştirme çalışma zamanları** sekmesi. 

   !["Tümleştirme çalışma zamanları" sekmesi](media/join-azure-ssis-integration-runtime-virtual-network/integration-runtimes-tab.png)
4. Azure SSIS IR çalışıyorsa tümleştirmesi çalışma zamanı listesinde seçin **durdurmak** düğmesini **Eylemler** Azure SSIS IR sütun Siz durduruncaya kadar bir IR düzenleyemezsiniz. 

   ![IR Durdur](media/join-azure-ssis-integration-runtime-virtual-network/stop-ir-button.png)
1. Tümleştirme çalışma zamanı listeden seçin **Düzenle** düğmesini **Eylemler** Azure SSIS IR sütun

   ![Tümleştirme çalışma zamanı Düzenle](media/join-azure-ssis-integration-runtime-virtual-network/integration-runtime-edit.png)
5. Üzerinde **genel ayarları** sayfasında **tümleştirmesi çalışma zamanı Kurulum** penceresinde, seçin **sonraki**. 

   ![IR kurulumu için genel ayarlar](media/join-azure-ssis-integration-runtime-virtual-network/ir-setup-general-settings.png)
6. Üzerinde **SQL ayarlarını** sayfasında, yönetici parolasını girin ve seçin **sonraki**.

   ![IR kurulumu için SQL Server ayarları](media/join-azure-ssis-integration-runtime-virtual-network/ir-setup-sql-settings.png)
7. Üzerinde **Gelişmiş ayarları** sayfasında, aşağıdaki eylemleri gerçekleştirebilirsiniz: 

   a. Onay kutusunu seçin **VNet izinleri/ayarlarını yapılandırmak için bir VNet için Azure SSIS tümleştirme katılmak ve Azure Hizmetleri izin vermek çalışma zamanı modülü seçin**.

   b. İçin **türü**sanal ağı Klasik sanal ağ veya bir Azure Resource Manager sanal ağ olup olmadığını seçin. 

   c. İçin **VNet adı**, sanal ağınızı seçin.

   d. İçin **alt ağ adı**, sanal ağ, alt ağ seçin.

   e. Tıklatın **VNet doğrulama** tıklatıp başarılı olursa, **güncelleştirme**. 

   ![IR kurulumu için Gelişmiş ayarlar](media/join-azure-ssis-integration-runtime-virtual-network/ir-setup-advanced-settings.png)
8. Kullanarak IR şimdi başlatabilirsiniz **Başlat** düğmesini **Eylemler** Azure SSIS IR sütun Yaklaşık 20-30 Azure SSIS IR başlatmak için dakika sürer 

## <a name="azure-powershell"></a>Azure PowerShell

### <a name="configure-a-virtual-network"></a>Sanal ağ yapılandırma
İçin Azure SSIS IR katılabilmesi için önce bir sanal ağ yapılandırmanız gerekir. Otomatik olarak sanal ağına bağlanmak, Azure SSIS tümleştirmesi çalışma zamanı için sanal ağ izinlerini/ayarlarını yapılandırmak için aşağıdaki betiği ekleyin:

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

### <a name="create-an-azure-ssis-ir-and-join-it-to-a-virtual-network"></a>Bir Azure SSIS IR oluşturmak ve bir sanal ağa katılma
Bir Azure SSIS IR oluşturabilir ve aynı anda bir sanal ağa katılın. Tam komut dosyası ve yönergeler için bkz: [Azure SSIS tümleştirmesi çalışma zamanı oluşturma](create-azure-ssis-integration-runtime.md#azure-powershell).

### <a name="join-an-existing-azure-ssis-ir-to-a-virtual-network"></a>Var olan bir Azure SSIS IR sanal bir ağa katılmasını sağlayın
Komut dosyasında [Azure SSIS tümleştirmesi çalışma zamanı oluşturma](create-azure-ssis-integration-runtime.md) makale bir Azure SSIS IR oluşturmak ve bir sanal ağ aynı komut eklemek nasıl gösterir. Var olan bir Azure SSIS IR varsa, sanal ağa eklemek için aşağıdaki adımları gerçekleştirin: 

1. Azure SSIS IR Durdur
2. Azure SSIS IR sanal ağına bağlanmak için yapılandırın. 
3. Azure SSIS IR Başlat 

### <a name="define-the-variables"></a>Değişkenleri tanımlayın
```powershell
$ResourceGroupName = "<your Azure resource group name>"
$DataFactoryName = "<your Data Factory name>" 
$AzureSSISName = "<your Azure-SSIS IR name>"
# Specify the information about your classic or Azure Resource Manager virtual network.
$VnetId = "<your Azure virtual network resource ID>"
$SubnetName = "<the name of subnet in your virtual network>"
```

### <a name="stop-the-azure-ssis-ir"></a>Azure SSIS IR Durdur
Bir sanal ağa katılabilmesi için önce Azure SSIS tümleştirmesi çalışma zamanı durdurun. Bu komut tüm düğümlerinin serbest bırakır ve faturalama durdurur:

```powershell
Stop-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                            -DataFactoryName $DataFactoryName `
                                            -Name $AzureSSISName `
                                            -Force 
```

### <a name="configure-virtual-network-settings-for-the-azure-ssis-ir-to-join"></a>Katılmak Azure SSIS IR için sanal ağ ayarlarını yapılandırma
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
        # Assign VM contributor role to Microsoft.Batch
        New-AzureRmRoleAssignment -ObjectId $BatchObjectId -RoleDefinitionName "Classic Virtual Machine Contributor" -Scope $VnetId
    }
}
```

### <a name="configure-the-azure-ssis-ir"></a>Azure SSIS IR yapılandırın
Sanal ağına bağlanmak için Azure SSIS tümleştirmesi çalışma zamanı yapılandırmak için çalıştırın `Set-AzureRmDataFactoryV2IntegrationRuntime` komutu: 

```powershell
Set-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                           -DataFactoryName $DataFactoryName `
                                           -Name $AzureSSISName `
                                           -Type Managed `
                                           -VnetId $VnetId `
                                           -Subnet $SubnetName
```

### <a name="start-the-azure-ssis-ir"></a>Azure SSIS IR Başlat
Azure SSIS tümleştirmesi çalışma zamanı başlatmak için aşağıdaki komutu çalıştırın: 

```powershell
Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

```

Bu komutun tamamlanması için 20-30 dakika sürer.

## <a name="next-steps"></a>Sonraki adımlar
Azure SSIS çalışma zamanı hakkında daha fazla bilgi için aşağıdaki konulara bakın: 

- [Azure SSIS tümleştirmesi çalışma zamanı](concepts-integration-runtime.md#azure-ssis-integration-runtime). Bu makalede Azure SSIS IR dahil olmak üzere tümleştirme çalışma zamanları hakkında kavramsal bilgiler genel olarak, sağlar. 
- [Öğretici: SSIS paketlerini Azure’a dağıtma](tutorial-create-azure-ssis-runtime-portal.md). Bu makale bir Azure SSIS IR oluşturmak için adım adım yönergeler sağlar SSIS katalog barındırmak için Azure SQL veritabanı kullanır. 
- [Azure-SSIS tümleştirme çalışma zamanı oluşturma](create-azure-ssis-integration-runtime.md). Bu makalede öğreticiyi genişletir ve Azure SQL veritabanı SSIS katalog barındırmak için sanal ağ hizmet uç noktaları/yönetilen ile örneği (Önizleme) kullanarak ve bir sanal ağa IR katılması yönergeler sağlar. 
- [Azure-SSIS IR’yi izleme](monitor-integration-runtime.md#azure-ssis-integration-runtime). Bu makalede bir Azure-SSIS IR ile ilgili bilgileri ve döndürülen bilgilerdeki durumların açıklamalarını alma işlemi gösterilmektedir. 
- [Azure-SSIS IR’yi yönetme](manage-azure-ssis-integration-runtime.md). Bu makale bir Azure-SSIS IR’yi durdurma, başlatma veya kaldırma işlemini gösterir. Ayrıca, düğüm ekleyerek, Azure SSIS IR ölçeklendirmek nasıl gösterir. 
