---
title: Azure-SSIS tümleştirme çalışma zamanını bir sanal ağa katılın | Microsoft Docs
description: Azure-SSIS tümleştirme çalışma zamanının Azure sanal ağına katılın öğrenin.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/08/2019
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: 6978b83e66f58e468d9f98394904861c8a4d8bd0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66152693"
---
# <a name="join-an-azure-ssis-integration-runtime-to-a-virtual-network"></a>Bir Azure-SSIS tümleştirme çalışma zamanını bir sanal ağa katılın
Bir Azure sanal ağına aşağıdaki senaryolarda, Azure-SSIS Integration runtime (IR) katılın: 

- Şirket içi veri depolarına bir Azure-SSIS tümleştirme çalışma zamanı üzerinde çalışan SSIS paketlerinden bağlanmak istiyorsanız. 

- Sanal ağ hizmet uç noktaları/yönetilen örnek ile Azure SQL veritabanı'nda SQL Server Integration Services (SSIS) Katalog veritabanı barındırır. 

  Azure Data Factory, Azure-SSIS tümleştirme çalışma zamanınızın Klasik dağıtım modeli veya Azure Resource Manager dağıtım modeli oluşturulan bir sanal ağa eklemenize olanak tanır. 

> [!IMPORTANT]
> Klasik sanal ağ şu anda kullanımdan kaldırılıyor, bu nedenle Lütfen bunun yerine Azure Resource Manager sanal ağ.  Klasik sanal ağ'ı zaten kullanıyorsanız, Azure Resource Manager sanal ağı olabildiğince çabuk kullanmak için lütfen geçiş yapın.

## <a name="access-to-on-premises-data-stores"></a>Şirket içi veri depolarına erişim
SSIS paketlerini tek genel bulut veri depoları erişirseniz, Azure-SSIS IR'yi bir sanal ağa eklemeniz gerekmez. SSIS paketlerini şirket içi veri depolarına erişirseniz, Azure-SSIS IR'nin şirket içi ağa bağlı bir sanal ağa eklemeniz gerekir. 

Dikkat edilecek bazı önemli noktalar şunlardır: 

- Varsa mevcut hiçbir sanal ağ, şirket içi ağınıza bağlı, ilk oluşturmak bir [Azure Resource Manager sanal ağı](../virtual-network/quick-create-portal.md#create-a-virtual-network) veya [Klasik sanal ağ](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) , Azure-SSIS tümleştirme katılmak için çalışma zamanı. Ardından, site-to-site yapılandırma [VPN ağ geçidi bağlantısı](../vpn-gateway/vpn-gateway-howto-site-to-site-classic-portal.md) veya [ExpressRoute](../expressroute/expressroute-howto-linkvnet-classic.md) bağlantısı, sanal ağdan şirket içi ağınıza. 

- Bir mevcut Azure Resource Manager veya Klasik sanal ağ, Azure-SSIS IR ile aynı konumda şirket içi ağınıza bağlı ise, bu sanal ağa IR katılabilirsiniz. 

- Azure-SSIS IR farklı bir konumda şirket içi ağınıza bağlı bir Klasik sanal ağınız varsa, ilk oluşturabileceğiniz bir [Klasik sanal ağ](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) katılmak Azure-SSIS IR için. Ardından, yapılandırma bir [Klasik-Klasik sanal ağ](../vpn-gateway/vpn-gateway-howto-vnet-vnet-portal-classic.md) bağlantı. Veya oluşturabileceğiniz bir [Azure Resource Manager sanal ağı](../virtual-network/quick-create-portal.md#create-a-virtual-network) katılmak Azure-SSIS tümleştirme çalışma zamanınızın. Ardından yapılandırma bir [Klasik Azure Resource Manager sanal ağı](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md) bağlantı. 
 
- Mevcut Azure Resource Manager sanal ağ ' Azure-SSIS IR farklı bir konumda şirket içi ağınıza bağlı ise, ilk oluşturabileceğiniz bir [Azure Resource Manager sanal ağı](../virtual-network/quick-create-portal.md##create-a-virtual-network) Azure-SSIS için IR katılın. Ardından, bir Azure Resource Manager Azure Resource Manager sanal ağ bağlantısı yapılandırın. Veya, oluşturabileceğiniz bir [Klasik sanal ağ](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) katılmak Azure-SSIS IR için. Ardından, yapılandırma bir [Klasik Azure Resource Manager sanal ağı](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md) bağlantı. 

## <a name="host-the-ssis-catalog-database-in-azure-sql-database-with-virtual-network-service-endpointsmanaged-instance"></a>Sanal ağ hizmet uç noktaları/yönetilen örnek ile Azure SQL veritabanı'nda SSIS Kataloğu veritabanını barındırmak
SSIS kataloğunu Azure SQL veritabanı'nda sanal ağ hizmet uç noktaları veya yönetilen örneği ile barındırılıyorsa için Azure-SSIS IR birleştirebilirsiniz: 

- Aynı sanal ağ 
- Yönetilen örnek için kullanılan bir ağ ve ağ bağlantısı olan farklı bir sanal ağ 

Sanal ağ hizmet uç noktaları ile Azure SQL veritabanı'nda, SSIS kataloğunu barındırmak, aynı sanal ağ ve alt ağ için Azure-SSIS IR katılın emin olun.

Yönetilen örneği aynı sanal ağ için Azure-SSIS IR katılırsanız, Azure-SSIS IR yönetilen örneği'den farklı bir alt ağda olduğundan emin olun. Azure-SSIS IR, yönetilen örneğe farklı bir sanal ağa katılırsanız, (Bu aynı bölgeye sınırlıdır) sanal ağ eşlemesi veya sanal ağ için sanal ağ bağlantısı öneririz. Bkz: [uygulamanızı Azure SQL veritabanı yönetilen örneği bağlamak](../sql-database/sql-database-managed-instance-connect-app.md).

Her durumda, sanal ağın yalnızca Azure Resource Manager dağıtım modeliyle dağıtılabilir.

Aşağıdaki bölümlerde daha ayrıntılı bilgi verilmektedir. 

## <a name="requirements-for-virtual-network-configuration"></a>Sanal ağ yapılandırması gereksinimleri
-   Emin olun `Microsoft.Batch` Azure-SSIS IR'yi barındıran sanal ağ alt ağınızın abonelik altında kayıtlı bir sağlayıcı Klasik sanal ağ'ı kullanıyorsanız, aynı zamanda katılın `MicrosoftAzureBatch` Klasik sanal makine Katılımcısı rolüne bu sanal ağ. 

-   Gerekli izinlere sahip olduğunuzdan emin olun. Bkz: [gerekli izinler](#perms).

-   Azure-SSIS IR'yi barındırmak için uygun alt ağ seçin Bkz: [alt ağı seçin](#subnet). 

-   Sanal ağ kendi etki alanı Hizmetleri (DNS) sunucusu kullanıyorsanız, bkz [etki alanı adı Hizmetleri sunucusu](#dns_server). 

-   Alt ağda bir ağ güvenlik grubu (NSG) kullanıyorsanız, bkz. [ağ güvenlik grubu](#nsg) 

-   Azure Express Route kullanarak veya kullanıcı tanımlı yol (UDR) yapılandırma Bkz [kullanımı Azure ExpressRoute veya kullanıcı tanımlı yol](#route). 

-   Sanal ağın kaynak grubu oluşturabilir ve belirli Azure ağ kaynakları silmek emin olun. Bkz: [kaynak grubu için gereksinimleri](#resource-group). 

Azure-SSIS IR için gerekli bağlantıları gösteren bir diyagram şu şekildedir:

![Azure-SSIS IR](media/join-azure-ssis-integration-runtime-virtual-network/azure-ssis-ir.png)

### <a name="perms"></a> Gerekli izinler

Azure-SSIS tümleştirme çalışma zamanını oluşturan kullanıcının aşağıdaki izinlere sahip olmalıdır:

- Bir Azure Resource Manager sanal ağı, SSIS IR birleştirdiğimiz, iki seçeneğiniz vardır:

  - Yerleşik kullanın *ağ Katılımcısı* rol. Bu rol ile birlikte gelen _Microsoft.Network/\*_  daha bir çok geniş kapsamı olan izni.

  - Yalnızca gerekli içeren özel bir rol oluşturun _Microsoft.Network/virtualNetworks/\*/JOIN/eylem_ izni. 

- Klasik bir sanal ağa, SSIS IR birleştirdiğimiz, yerleşik kullanmanızı öneririz *Klasik sanal makine Katılımcısı* rol. Aksi takdirde sanal ağ alanına katılma izni içeren özel bir rol tanımlamak gerekir.

### <a name="subnet"></a> Alt ağı seçin
-   Sanal ağ geçitleri için ayrılmış olduğundan, bir Azure-SSIS Integration Runtime'ı dağıtmak için GatewaySubnet seçmeyin. 

-   Seçtiğiniz alt ağı kullanmak Azure-SSIS IR için yeterli kullanılabilir adres alanı olduğundan emin olun. En az 2 bırakın * IR düğüm numarasını kullanılabilir IP adresleri. Azure her alt ağ içinde bazı IP adreslerini ayırır ve bu adresi kullanılamaz. Alt ağların ilk ve son IP adresleri, Azure Hizmetleri için kullanılan üç daha fazla adres birlikte protokol uyumluluğu için ayrılmıştır. Daha fazla bilgi için [bu alt ağları içindeki IP adresleri kullanarak herhangi bir kısıtlama var mıdır?](../virtual-network/virtual-networks-faq.md#are-there-any-restrictions-on-using-ip-addresses-within-these-subnets). 

-   Diğer Azure Hizmetleri ile (örneğin, SQL veritabanı yönetilen örneği, App Service, vb.) özel olarak dolu bir alt ağ kullanmayın. 

### <a name="dns_server"></a> Etki alanı adı Hizmetleri sunucusu 
Azure-SSIS tümleştirme çalışma zamanı tarafından birleştirilmiş bir sanal ağ kendi etki alanı Hizmetleri (DNS) sunucusu kullanmanız gerekiyorsa, Azure genel ana bilgisayar adları çözümlemek emin olun (örneğin, bir Azure depolama blob adı `<your storage account>.blob.core.windows.net`). 

Aşağıdaki adımları önerilir: 

-   Özel DNS, Azure DNS istekleri iletmek üzere yapılandırın. Çözümlenmemiş DNS kayıtlarını IP adresi, Azure'nın yinelemeli Çözümleyicileri (168.63.129.16) için kendi DNS sunucunuzda iletebilir. 

-   Birincil olarak özel DNS ve Azure DNS sanal ağ için ikincil olarak ayarlayın. Azure'nın yinelemeli Çözümleyicileri (168.63.129.16) IP adresini, kendi DNS sunucusu kullanılamıyor durumda bir ikincil DNS sunucusu olarak kaydedin. 

Daha fazla bilgi için bkz. [ad çözümlemesi kendi DNS sunucunuzu kullanan](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server). 

### <a name="nsg"></a> Ağ güvenlik grubu
Gelen/giden trafik bir ağ güvenlik grubu (NSG), Azure-SSIS tümleştirme çalışma zamanı tarafından kullanılan alt ağ uygulamanız gerekiyorsa, aşağıdaki bağlantı noktaları izin ver: 

| Direction | Aktarım Protokolü | Kaynak | Kaynak bağlantı noktası aralığı | Hedef | Hedef bağlantı noktası aralığı | Açıklamalar |
|---|---|---|---|---|---|---|
| Gelen | TCP | AzureCloud<br/>(veya geniş kapsamı Internet gibi) | * | VirtualNetwork | 29876, 29877'numaralı (bir Azure Resource Manager sanal ağı'de IR katılırsanız) <br/><br/>10100, 20100, 30100'numaralı (klasik bir sanal ağ'de IR katılırsanız)| Data Factory hizmetinin bu bağlantı noktaları sanal ağ, Azure-SSIS tümleştirme çalışma zamanı düğümleri ile iletişim kurmak için kullanır. <br/><br/> Veya bir alt ağ düzeyinde NSG oluşturmak ister, Data Factory her zaman bir NSG Azure-SSIS IR'yi barındıran sanal makinelere bağlı ağ arabirim kartı (NIC) düzeyinde yapılandırır Bu NIC düzeyinde NSG tarafından yalnızca belirtilen bağlantı noktalarında veri fabrikası IP adreslerinden gelen trafiğe izin verilir. Alt ağ düzeyinde Internet trafiği için bu bağlantı noktalarını açmanız bile NIC düzeyinde Data Factory IP adresleri olan IP adreslerinden gelen trafik engellenir. |
| Giden | TCP | VirtualNetwork | * | AzureCloud<br/>(veya geniş kapsamı Internet gibi) | 443 | Sanal ağ, Azure-SSIS tümleştirme çalışma zamanı düğümleri, Azure depolama ve Azure Event Hubs gibi Azure hizmetlerine erişmek için bu bağlantı noktasını kullanın. |
| Giden | TCP | VirtualNetwork | * | İnternet | 80 | Sanal ağ, Azure-SSIS tümleştirme çalışma zamanı düğümleri, Internet'ten sertifika iptal listesini indirmek için bu bağlantı noktasını kullanın. |
| Giden | TCP | VirtualNetwork | * | Sql<br/>(veya geniş kapsamı Internet gibi) | 1433, 11000-11999, 14000-14999 | SSISDB erişmek için bu bağlantı noktaları - Azure SQL veritabanı sunucunuz tarafından barındırılan sanal ağ kullanma, Azure-SSIS tümleştirme çalışma zamanı düğümleri bu amaç yönetilen örneği tarafından barındırılan SSISDB için geçerli değildir. |
||||||||

### <a name="route"></a> Azure ExpressRoute veya kullanıcı tanımlı yol kullanın
Bağlanabileceğiniz bir [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/) bağlantı hattına sanal ağ altyapınızı şirket içi ağınızı Azure'a genişletmek için. 

Zorlamalı tünel kullanmak için ortak bir yapılandırma olduğundan (0.0.0.0/0 sanal ağa bir BGP rota tanıtma) hangi zorlar giden Internet trafiğini sanal ağ akıştan şirket içi ağ gerecine inceleme ve günlüğe kaydetme. Bu trafik akışını Azure-SSIS IR ile Azure Data Factory bağlı Hizmetleri sanal ağ arasındaki bağlantıyı keser. Çözüm bir (veya daha fazla) tanımlamaktır [kullanıcı tanımlı yollar (Udr)](../virtual-network/virtual-networks-udr-overview.md) Azure-SSIS IR'yi içeren alt ağda UDR yerine BGP yolu kabul özel alt ağ yollarını tanımlar. 

Veya kullanıcı tanımlı yollar (Udr) bir güvenlik duvarı veya bir çevre konağı bir sanal ağ Gereci barındıran başka bir alt ağ için Azure-SSIS IR barındıran alt ağdan giden Internet trafiğini zorlamak için inceleme ve günlüğe kaydetme için tanımlayabilirsiniz. 

Her iki durumda da, sonraki atlama türü ile 0.0.0.0/0 rota uygulamak **Internet** Azure-SSIS IR barındıran ve böylece Data Factory hizmeti olan Azure-SSIS IR arasındaki iletişimi başarılı olabilmesi için alt ağda. 

![Yol ekleme](media/join-azure-ssis-integration-runtime-virtual-network/add-route-for-vnet.png)

Bu alt ağdan giden Internet trafiğini denetleme olanağı kaybetmekten endişeleriniz varsa da bir NSG kuralı Giden hedeflere kısıtlamak için alt ağda ekleyebilirsiniz [Azure veri merkezi IP adresleri](https://www.microsoft.com/download/details.aspx?id=41653). 

Bkz: [bu PowerShell Betiği](https://gallery.technet.microsoft.com/scriptcenter/Adds-Azure-Datacenter-IP-dbeebe0c) örneği. Azure veri merkezi IP adresi listesi güncel tutmak için haftalık betiği çalıştırmanız gerekir. 

### <a name="resource-group"></a> Kaynak grubu için gereksinimler
-   Azure-SSIS IR sanal ağ aynı kaynak grubu altında belirli ağ kaynakları oluşturma gerekir. Bu kaynakları şunları içerir:
    -   Bu ada sahip bir Azure load balancer  *\<GUID > - azurebatch - cloudserviceloadbalancer*.
    -   Bir Azure genel IP adresi adıyla  *\<GUID > - azurebatch - cloudservicepublicip*.
    -   Bir ağ iş güvenlik grubu adı ile  *\<GUID > - azurebatch - cloudservicenetworksecuritygroup*. 

-   Kaynak grubuna veya aboneliğe ait olduğu sanal ağ üzerindeki herhangi bir kaynak kilidi yoksa emin olun. Bir salt okunur kilidi ya da silme kilidi yapılandırırsanız, başlatma ve IR'yi durdurma başarısız olabilir veya yanıt vermiyor. 

-   Aşağıdaki kaynaklar kaynak grubuna veya aboneliğe ait olduğu sanal ağı altında oluşturulan önleyen bir Azure ilkesine sahip olmadığınızdan emin emin olun: 
    -   Microsoft.Network/LoadBalancers 
    -   Microsoft.Network/NetworkSecurityGroups 
    -   Microsoft.Network/PublicIPAddresses 

## <a name="azure-portal-data-factory-ui"></a>Azure portalı (Data Factory kullanıcı Arabirimi)
Bu bölümde Azure portalı ve Data Factory kullanıcı arabirimini kullanarak bir sanal ağ (Klasik veya Azure Resource Manager) var olan bir Azure-SSIS çalışma zamanı görünümlerle nasıl gösterir. İlk olarak, Azure-SSIS IR katılmadan önce sanal ağ'ı uygun şekilde yapılandırmanız gerekir. Sanal ağınız (Klasik veya Azure Resource Manager) türüne göre sonraki iki bölümden birini inceleyin. Ardından, Azure-SSIS IR sanal ağa katılmak için üçüncü bölümüne geçin. 

### <a name="use-the-portal-to-configure-an-azure-resource-manager-virtual-network"></a>Bir Azure Resource Manager sanal ağı yapılandırmak için portalı kullanma
Sanal ağ için bir Azure-SSIS IR katılabilmesi için önce yapılandırma gerekir. 

1. Microsoft Edge veya Google Chrome başlatın. Şu anda Data Factory kullanıcı Arabirimi, yalnızca bu web tarayıcılarında desteklenmektedir. 

1. [Azure Portal](https://portal.azure.com) oturum açın. 

1. Seçin **diğer hizmetler**. Filtreleyip seçmek **sanal ağları**. 

1. Filtre ve sanal ağınızı listeden seçin. 

1. Üzerinde **sanal ağ** sayfasında **özellikleri**. 

1. Kopyala düğmesi seçin **kaynak kimliği** sanal ağın kaynak Kimliğini panoya kopyalamak için. Panodan OneNote ya da bir dosya kimliği kaydedin. 

1. Seçin **alt ağlar** sol menüsünde. Sayısını emin **kullanılabilir adresleri** , Azure-SSIS tümleştirme çalışma zamanı düğümleri büyüktür. 

1. Sanal ağ olan Azure aboneliğinde Azure Batch sağlayıcısının kayıtlı olduğunu doğrulayın. Veya Azure Batch sağlayıcısını kaydedin. Aboneliğinizde bir Azure Batch hesabı zaten varsa, aboneliğinizi Azure Batch için kayıtlı. (Azure-SSIS IR Data Factory Portalı'nda oluşturursanız, Azure Batch sağlayıcısı otomatik olarak sizin için kaydedilir.) 

   a. Azure portalında **abonelikleri** sol menüsünde. 

   b. Aboneliğinizi seçin. 

   c. Seçin **kaynak sağlayıcıları** soldaki doğrulayın **Microsoft.Batch** kayıtlı bir sağlayıcıdır. 

   !["Kaydedildi" durumunu doğrulama](media/join-azure-ssis-integration-runtime-virtual-network/batch-registered-confirmation.png)

   Görmüyorsanız **Microsoft.Batch** , kaydetmek için bu listedeki [boş bir Azure Batch hesabı oluşturma](../batch/batch-account-create-portal.md) aboneliğinizdeki. Daha sonra silebilirsiniz. 

### <a name="use-the-portal-to-configure-a-classic-virtual-network"></a>Klasik bir sanal ağı yapılandırmak için portalı kullanma
Sanal ağ için bir Azure-SSIS IR katılabilmesi için önce yapılandırma gerekir. 

1. Microsoft Edge veya Google Chrome başlatın. Şu anda Data Factory kullanıcı Arabirimi, yalnızca bu web tarayıcılarında desteklenmektedir. 

1. [Azure Portal](https://portal.azure.com) oturum açın. 

1. Seçin **diğer hizmetler**. Filtreleyip seçmek **sanal ağlar (Klasik)** . 

1. Filtre ve sanal ağınızı listeden seçin. 

1. Üzerinde **sanal ağ (Klasik)** sayfasında **özellikleri**. 

   ![Klasik sanal ağ kaynak kimliği](media/join-azure-ssis-integration-runtime-virtual-network/classic-vnet-resource-id.png)

1. Kopyala düğmesi seçin **kaynak kimliği** Klasik ağ kaynak kimliği panoya kopyalamak için. Panodan OneNote ya da bir dosya kimliği kaydedin. 

1. Seçin **alt ağlar** sol menüsünde. Sayısını emin **kullanılabilir adresleri** , Azure-SSIS tümleştirme çalışma zamanı düğümleri büyüktür. 

   ![Sanal ağda kullanılabilir adreslerin sayısı](media/join-azure-ssis-integration-runtime-virtual-network/number-of-available-addresses.png)

1. Birleştirme **MicrosoftAzureBatch** için **Klasik sanal makine Katılımcısı** rolü için sanal ağ. 

    a. Seçin **erişim denetimi (IAM)** seçin ve soldaki menüden **rol atamaları** sekmesi. 

    !["Erişim denetimi" ve "düğmeleri ekleme"](media/join-azure-ssis-integration-runtime-virtual-network/access-control-add.png)

    b. Seçin **rol ataması Ekle**.

    c. Üzerinde **rol ataması Ekle** sayfasında **Klasik sanal makine Katılımcısı** için **rol**. Yapıştırma **ddbf3205-c6bd-46ae-8127-60eb93363864** içinde **seçin** kutusuna ve ardından **Microsoft Azure Batch** arama sonuçları listesinden. 

    ![Arama sonuçları sayfasında "rol ataması ekleme"](media/join-azure-ssis-integration-runtime-virtual-network/azure-batch-to-vm-contributor.png)

    d. Seçin **Kaydet** ayarları kaydetmek ve sayfasını kapatın. 

    ![Erişim ayarlarını Kaydet](media/join-azure-ssis-integration-runtime-virtual-network/save-access-settings.png)

    e. Gördüğünüzü onaylayın **Microsoft Azure Batch** katkıda bulunanlar listesinde. 

    ![Azure Batch erişimi onaylama](media/join-azure-ssis-integration-runtime-virtual-network/azure-batch-in-list.png)

1. Sanal ağ olan Azure aboneliğinde Azure Batch sağlayıcısının kayıtlı olduğunu doğrulayın. Veya Azure Batch sağlayıcısını kaydedin. Aboneliğinizde bir Azure Batch hesabı zaten varsa, aboneliğinizi Azure Batch için kayıtlı. (Azure-SSIS IR Data Factory Portalı'nda oluşturursanız, Azure Batch sağlayıcısı otomatik olarak sizin için kaydedilir.) 

   a. Azure portalında **abonelikleri** sol menüsünde. 

   b. Aboneliğinizi seçin. 

   c. Seçin **kaynak sağlayıcıları** soldaki doğrulayın **Microsoft.Batch** kayıtlı bir sağlayıcıdır. 

   !["Kaydedildi" durumunu doğrulama](media/join-azure-ssis-integration-runtime-virtual-network/batch-registered-confirmation.png)

   Görmüyorsanız **Microsoft.Batch** , kaydetmek için bu listedeki [boş bir Azure Batch hesabı oluşturma](../batch/batch-account-create-portal.md) aboneliğinizdeki. Daha sonra silebilirsiniz. 

### <a name="join-the-azure-ssis-ir-to-a-virtual-network"></a>Azure-SSIS IR'yi bir sanal ağa ekleme
1. Microsoft Edge veya Google Chrome başlatın. Şu anda Data Factory kullanıcı Arabirimi, yalnızca bu web tarayıcılarında desteklenmektedir. 

1. İçinde [Azure portalında](https://portal.azure.com)seçin **veri fabrikaları** sol menüsünde. Görmüyorsanız **veri fabrikaları** menüsünde **diğer hizmetler**, ardından **veri fabrikaları** içinde **ZEKA + ANALİZ**bölümü. 

   ![Veri fabrikaları listesi](media/join-azure-ssis-integration-runtime-virtual-network/data-factories-list.png)

1. Azure-SSIS tümleştirme çalışma zamanı ile veri fabrikanızı listeden seçin. Veri fabrikanızın giriş sayfasını görürsünüz. Seçin **geliştir ve Dağıt** Döşe. Data Factory kullanıcı arabirimini ayrı bir sekmede görürsünüz. 

   ![Data factory giriş sayfası](media/join-azure-ssis-integration-runtime-virtual-network/data-factory-home-page.png)

1. Data Factory kullanıcı Arabiriminde geçin **Düzenle** sekmesinde **bağlantıları**geçin **tümleştirme çalışma zamanları** sekmesi. 

   !["Tümleştirme çalışma zamanları" sekmesi](media/join-azure-ssis-integration-runtime-virtual-network/integration-runtimes-tab.png)

1. Azure-SSIS IR çalışıyorsa tümleştirme çalışma zamanı listesinde seçin **Durdur** düğmesine **eylemleri** sütunu için Azure-SSIS IR Siz durduruncaya kadar bir IR düzenleyemezsiniz. 

   ![IR'yi durdurma](media/join-azure-ssis-integration-runtime-virtual-network/stop-ir-button.png)

1. Tümleştirme çalışma zamanı listesinde **Düzenle** düğmesine **eylemleri** sütunu için Azure-SSIS IR 

   ![Tümleştirme çalışma zamanının Düzenle](media/join-azure-ssis-integration-runtime-virtual-network/integration-runtime-edit.png)

1. Üzerinde **genel ayarlar** sayfasının **tümleştirme çalışma zamanı Kurulumu** penceresinde **sonraki**. 

   ![IR kurulumu için genel ayarlar](media/join-azure-ssis-integration-runtime-virtual-network/ir-setup-general-settings.png)

1. Üzerinde **SQL ayarları** sayfasında, yönetici parolasını girin ve seçin **sonraki**. 

   ![IR kurulumu için SQL Server ayarları](media/join-azure-ssis-integration-runtime-virtual-network/ir-setup-sql-settings.png)

1. Üzerinde **Gelişmiş ayarlar** sayfasında, aşağıdaki eylemleri gerçekleştirin: 

   a. Onay kutusunu seçin **katılın ve Azure hizmetleri sağlamak Azure-SSIS Integration Runtime VNet izinlerini/ayarlarını yapılandırmak için bir VNet seçin**. 

   b. İçin **türü**, sanal ağı klasik bir sanal ağ veya bir Azure Resource Manager sanal ağı olduğunu seçin. 

   c. İçin **VNet adı**, sanal ağ seçin. 

   d. İçin **alt ağ adı**, sanal ağ, alt ağ seçin. 

   e. Tıklayın **VNet doğrulama** ve başarılı olursa, tıklayın **güncelleştirme**. 

   ![IR kurulumu için Gelişmiş ayarlar](media/join-azure-ssis-integration-runtime-virtual-network/ir-setup-advanced-settings.png)

1. Kullanarak IR şimdi başlayabilirsiniz **Başlat** düğmesine **eylemleri** sütunu için Azure-SSIS IR Yaklaşık 20-30 bir Azure-SSIS IR'yi başlatmak için dakika sürer 

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

### <a name="configure-a-virtual-network"></a>Sanal ağ yapılandırma
Azure-SSIS IR katılabilir önce bir sanal ağ yapılandırmanız gerekir. Sanal ağa katılmak Azure-SSIS tümleştirme çalışma zamanınızın sanal ağın izinlerini/ayarlarını otomatik olarak yapılandırmak için aşağıdaki betiği ekleyin:

```powershell
# Make sure to run this script against the subscription to which the virtual network belongs.
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

### <a name="create-an-azure-ssis-ir-and-join-it-to-a-virtual-network"></a>Bir Azure-SSIS IR oluşturma ve bir sanal ağa katılın
Bir Azure-SSIS IR oluşturma ve aynı anda bir sanal ağa katılın. Tam komut dosyası ve yönergeler için bkz. [bir Azure-SSIS tümleştirme çalışma zamanı oluşturma](create-azure-ssis-integration-runtime.md#azure-powershell).

### <a name="join-an-existing-azure-ssis-ir-to-a-virtual-network"></a>Mevcut bir Azure-SSIS IR'yi bir sanal ağa katılın
Betikte [bir Azure-SSIS tümleştirme çalışma zamanı oluşturma](create-azure-ssis-integration-runtime.md) makale, bir Azure-SSIS IR oluşturma ve aynı betiği sanal ağa katılın. Mevcut bir Azure-SSIS IR varsa, sanal ağa katılmak için aşağıdaki adımları gerçekleştirin: 
1. Azure-SSIS IR'yi durdurma 
1. Azure-SSIS IR'nin sanal ağa katılmak için yapılandırın. 
1. Azure-SSIS IR'yi Başlat 

### <a name="define-the-variables"></a>Değişkenleri tanımlama
```powershell
$ResourceGroupName = "<your Azure resource group name>"
$DataFactoryName = "<your Data Factory name>" 
$AzureSSISName = "<your Azure-SSIS IR name>"
# Specify the information about your classic or Azure Resource Manager virtual network.
$VnetId = "<your Azure virtual network resource ID>"
$SubnetName = "<the name of subnet in your virtual network>"
```

### <a name="stop-the-azure-ssis-ir"></a>Azure-SSIS IR'yi durdurma
Azure-SSIS tümleştirme çalışma zamanını bir sanal ağa katılmadan önce durdurun. Bu komut, tüm alt düğümleri serbest bırakır ve faturalama durdurur:

```powershell
Stop-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                            -DataFactoryName $DataFactoryName `
                                            -Name $AzureSSISName `
                                            -Force 
```

### <a name="configure-virtual-network-settings-for-the-azure-ssis-ir-to-join"></a>Azure-SSIS IR katılmak için sanal ağ ayarlarını yapılandırma
```powershell
# Make sure to run this script against the subscription to which the virtual network belongs.
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
        # Assign VM contributor role to Microsoft.Batch
        New-AzRoleAssignment -ObjectId $BatchObjectId -RoleDefinitionName "Classic Virtual Machine Contributor" -Scope $VnetId
    }
}
```

### <a name="configure-the-azure-ssis-ir"></a>Azure-SSIS IR'yi yapılandırın
Sanal ağa katılmak için Azure-SSIS Integration runtime'ı yapılandırmak için çalıştırın `Set-AzDataFactoryV2IntegrationRuntime` komutu: 

```powershell
Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                           -DataFactoryName $DataFactoryName `
                                           -Name $AzureSSISName `
                                           -Type Managed `
                                           -VnetId $VnetId `
                                           -Subnet $SubnetName
```

### <a name="start-the-azure-ssis-ir"></a>Azure-SSIS IR Başlat
Azure-SSIS tümleştirme çalışma zamanını başlatmak için aşağıdaki komutu çalıştırın: 

```powershell
Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

```

Bu komutun tamamlanması 20-30 dakika sürer.

## <a name="next-steps"></a>Sonraki adımlar
Azure-SSIS çalışma zamanı hakkında daha fazla bilgi için aşağıdaki konulara bakın: 
- [Azure-SSIS tümleştirme çalışma zamanı](concepts-integration-runtime.md#azure-ssis-integration-runtime). Azure-SSIS IR'yi dahil olmak üzere genel olarak, bu makalede tümleştirme çalışma zamanları hakkında kavramsal bilgiler sağlar 
- [Öğretici: SSIS paketlerini Azure’a dağıtma](tutorial-create-azure-ssis-runtime-portal.md). Bu makale bir Azure-SSIS IR oluşturmak için adım adım yönergeler sağlar SSIS kataloğunu barındırmak için Azure SQL veritabanı kullanır. 
- [Azure-SSIS tümleştirme çalışma zamanı oluşturma](create-azure-ssis-integration-runtime.md). Bu makale öğreticiyi genişletip ve Azure SQL veritabanı, SSIS kataloğunu barındırmak için sanal ağ hizmet uç noktaları/yönetilen ile örneği kullanma ve IR'yi bir sanal ağa ekleme hakkında yönergeler sağlar. 
- [Azure-SSIS IR’yi izleme](monitor-integration-runtime.md#azure-ssis-integration-runtime). Bu makalede bir Azure-SSIS IR ile ilgili bilgileri ve döndürülen bilgilerdeki durumların açıklamalarını alma işlemi gösterilmektedir. 
- [Azure-SSIS IR’yi yönetme](manage-azure-ssis-integration-runtime.md). Bu makale bir Azure-SSIS IR’yi durdurma, başlatma veya kaldırma işlemini gösterir. Ayrıca, düğüm ekleyerek Azure-SSIS IR ölçeklendirmek nasıl gösterir. 
