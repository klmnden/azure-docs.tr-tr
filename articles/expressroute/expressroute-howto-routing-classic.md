---
title: 'Yönlendirme (bir ExpressRoute için eşliği) hattı yapılandırma: Azure: Klasik | Microsoft Docs'
description: Bu makalede, bir ExpressRoute bağlantı hattı için özel, ortak ve Microsoft eşlemesinin nasıl oluşturulduğu ve sağlandığı adım adım anlatılmaktadır. Bu makalede ayrıca bağlantı hattınızın durumunu denetleme, bağlantı hattını güncelleştirme veya silme işlemlerinin nasıl yapıldığı da anlatılmaktadır.
documentationcenter: na
services: expressroute
author: ganesr
manager: timlt
editor: ''
tags: azure-service-management
ms.assetid: a4bd39d2-373a-467a-8b06-36cfcc1027d2
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/21/2017
ms.author: ganesr;cherylmc
ms.openlocfilehash: 9cebb196bd91da704798fb001763a76e6d090472
ms.sourcegitcommit: 3017211a7d51efd6cd87e8210ee13d57585c7e3b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "31594146"
---
# <a name="create-and-modify-peering-for-an-expressroute-circuit-classic"></a>Oluşturma ve bir expressroute bağlantı hattı (Klasik) için eşleme değiştirme
> [!div class="op_single_selector"]
> * [Azure Portal](expressroute-howto-routing-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-routing-arm.md)
> * [Azure CLI](howto-routing-cli.md)
> * [Video - özel eşliği](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-private-peering-for-your-expressroute-circuit)
> * [Video - ortak eşleme](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-public-peering-for-your-expressroute-circuit)
> * [Video - Microsoft eşlemesi](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-microsoft-peering-for-your-expressroute-circuit)
> * [PowerShell (klasik)](expressroute-howto-routing-classic.md)
> 

Bu makalede, PowerShell ve klasik dağıtım modeli kullanarak bir expressroute için yönlendirme yapılandırması oluşturma ve yönetme için adım adım anlatılmaktadır. Aşağıdaki adımlarda ayrıca bir ExpressRoute bağlantı hattının durumunu denetleme, güncelleştirme veya bağlantı hattını silme ve eşlemelerin sağlamasını kaldırma işlemleri de anlatılmaktadır.

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

**Azure dağıtım modelleri hakkında**

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]


## <a name="configuration-prerequisites"></a>Yapılandırma önkoşulları
* Azure Hizmet Yönetimi (SM) PowerShell cmdlet'lerinin en yeni sürümünü gerekir. Daha fazla bilgi için bkz: [Azure PowerShell cmdlet'leri ile çalışmaya başlama](/powershell/azure/overview).  
* Yapılandırmaya başlamadan önce [önkoşullar](expressroute-prerequisites.md) sayfasını, [yönlendirme gereksinimleri](expressroute-routing.md) sayfasını ve [iş akışları](expressroute-workflows.md) sayfasını gözden geçirdiğinizden emin olun.
* Etkin bir ExpressRoute bağlantı hattınızın olması gerekir. Yönergeleri izleyerek [bir expressroute bağlantı hattı oluşturma](expressroute-howto-circuit-classic.md) ve devam etmeden önce bağlantı sağlayıcınız tarafından etkinleştirilen hattı sahip. Aşağıda açıklanan cmdlet’leri çalıştırmanız için ExpressRoute bağlantı hattının sağlanmış ve etkin durumda olması gerekir.

> [!IMPORTANT]
> Bu yönergeler yalnızca Katman 2 bağlantı hizmetleri sunan hizmet sağlayıcıları ile oluşturulan bağlantı hatları için geçerlidir. Yönetilen Katman 3 hizmetleri (genellikle MPLS gibi bir IPVPN) sunan bir hizmet sağlayıcısı kullanıyorsanız, bağlantı sağlayıcınız yönlendirmeyi sizin için yapılandırır ve yönetir.
> 
> 

Bir ExpressRoute bağlantı hattı için bir, iki veya üç eşlemenin tamamını (Azure özel, Azure ortak ve Microsoft) yapılandırabilirsiniz. Eşlemeleri seçtiğiniz herhangi bir sırayla yapılandırabilirsiniz. Ancak, her eşlemenin yapılandırmasını birer birer tamamladığınızdan emin olmanız gerekir.


### <a name="log-in-to-your-azure-account-and-select-a-subscription"></a>Azure hesabınızda oturum açın ve bir abonelik seçin
1. PowerShell konsolunuzu yükseltilmiş haklarla açın ve hesabınıza bağlanın. Bağlanmanıza yardımcı olması için aşağıdaki örneği kullanın:

        Connect-AzureRmAccount

2. Hesapla ilişkili abonelikleri kontrol edin.

        Get-AzureRmSubscription

3. Birden fazla aboneliğiniz varsa, kullanmak istediğiniz aboneliği seçin.

        Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

4. Ardından, Klasik dağıtım modeli için PowerShell için Azure aboneliğinize eklemek için aşağıdaki cmdlet'i kullanın.

        Add-AzureAccount


## <a name="azure-private-peering"></a>Azure özel eşlemesi
Bu bölümde bir ExpressRoute bağlantı hattı için Azure özel eşleme yapılandırmasını oluşturma, alma, güncelleştirme ve silme hakkında yönergeler açıklanmaktadır. 

### <a name="to-create-azure-private-peering"></a>Azure özel eşlemesi oluşturmak için
1. **ExpressRoute için PowerShell modülünü içeri aktarın.**
   
    ExpressRoute cmdlet'lerini kullanmaya başlamak için PowerShell oturumuna Azure ve ExpressRoute modülleri içeri aktarmalısınız. Azure ve ExpressRoute modüllerini PowerShell oturumuna içeri aktarmak için aşağıdaki komutları çalıştırın. Sürüm farklılık gösterebilir.    
   
        Import-Module 'C:\Program Files\WindowsPowerShell\Modules\Azure\5.1.1\Azure\Azure.psd1'
        Import-Module 'C:\Program Files\WindowsPowerShell\Modules\Azure\5.1.1\ExpressRoute\ExpressRoute.psd1'
2. **Bir expressroute bağlantı hattı oluşturun.**
   
    Bir [ExpressRoute bağlantı hattı](expressroute-howto-circuit-classic.md) oluşturmak için yönergeleri izleyin ve bağlantı sağlayıcısından bağlantı hattını sağlamasını isteyin. Bağlantı sağlayıcınız yönetilen Katman 3 hizmetleri sunuyorsa, bağlantı sağlayıcınızdan sizin için Azure özel eşlemeyi etkinleştirmesini isteyebilirsiniz. Bu durumda, sonraki bölümlerde listelenen yönergeleri izlemeniz gerekmez. Ancak, bağlantı sağlayıcınız yönlendirmeyi sizin için yönetmiyorsa, bağlantı hattınızı oluşturduktan sonra aşağıdaki yönergeleri izleyin. 
3. **Expressroute bağlantı hattının sağlandığından emin olmak için kontrol edin.**
   
    Önce ExpressRoute ağ geçidinin Sağlandığından ve Etkin durumda olduğundan emin olmanız gerekir. Aşağıdaki örneğe bakın.
   
        PS C:\> Get-AzureDedicatedCircuit -ServiceKey "*********************************"
   
        Bandwidth                        : 200
        CircuitName                      : MyTestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Standard
        Status                           : Enabled
   
    Bağlantı hattı hazırlandı ve etkin gösterdiğinden emin olun. Seçili değilse, bağlantı hattınız gerekli durumu ve durumunu almak için bağlantı sağlayıcınız ile çalışır.
   
        ServiceProviderProvisioningState : Provisioned
        Status                           : Enabled
4. **Bağlantı hattı için Azure özel eşlemesini yapılandırın.**
   
    Sonraki adımlara devam etmeden önce aşağıdaki öğelerin bulunduğundan emin olun:
   
   * Birincil bağlantı için bir /30 alt ağı. Bu, sanal ağlar için ayrılmış herhangi bir adres alanının parçası olmamalıdır.
   * İkincil bağlantı için bir /30 alt ağı. Bu, sanal ağlar için ayrılmış herhangi bir adres alanının parçası olmamalıdır.
   * Bu eşlemenin kurulacağı geçerli bir VLAN kimliği. Bağlantı hattındaki başka bir eşlemenin aynı VLAN kimliğini kullanmadığından emin olun.
   * Eşleme için AS numarası. 2 bayt ve 4 bayt AS numaralarını kullanabilirsiniz. Bu eşleme için özel bir AS numarası kullanabilirsiniz. 65515’i kullanmadığınızdan emin olun.
   * Kullanmayı seçerseniz bir MD5 karma değeri. **Bu isteğe bağlıdır**.
     
    Bağlantı hattınız için Azure özel eşlemesini yapılandırmak üzere aşağıdaki cmdlet’i çalıştırabilirsiniz.
     
        Yeni AzureBGPPeering - AccessType özel - ServiceKey "***" - PrimaryPeerSubnet "10.0.0.0/30" - SecondaryPeerSubnet "10.0.0.4/30" - PeerAsn 1234 - Vlanıd 100
     
    Bir MD5 karma değeri kullanmayı seçerseniz, aşağıdaki cmdlet'i kullanabilirsiniz.
     
        Yeni AzureBGPPeering - AccessType özel - ServiceKey "***" - PrimaryPeerSubnet "10.0.0.0/30" - SecondaryPeerSubnet "10.0.0.4/30" - PeerAsn 1234 - Vlanıd 100 - SharedKey "A1B2C3D4"
     
     > [!IMPORTANT]
     > AS numaranızı müşteri ASN’si değil eşleme ASN’si olarak belirttiğinizden emin olun.
     > 
     > 

### <a name="to-view-azure-private-peering-details"></a>Azure özel eşleme ayrıntılarını görüntülemek için
Aşağıdaki cmdlet'i kullanarak yapılandırma ayrıntılarını alabilirsiniz

    Get-AzureBGPPeering -AccessType Private -ServiceKey "*********************************"

    AdvertisedPublicPrefixes       : 
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 
    PeerAsn                        : 1234
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 10.0.0.0/30
    RoutingRegistryName            : 
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 10.0.0.4/30
    State                          : Enabled
    VlanId                         : 100


### <a name="to-update-azure-private-peering-configuration"></a>Azure özel eşleme yapılandırmasını güncelleştirmek için
Aşağıdaki cmdlet'i kullanarak yapılandırmanın herhangi bir bölümünü güncelleştirebilirsiniz. Aşağıdaki örnekte, bağlantı hattının VLAN kimliği 100'den 500’e güncelleştiriliyor.

    Set-AzureBGPPeering -AccessType Private -ServiceKey "*********************************" -PrimaryPeerSubnet "10.0.0.0/30" -SecondaryPeerSubnet "10.0.0.4/30" -PeerAsn 1234 -VlanId 500 -SharedKey "A1B2C3D4"

### <a name="to-delete-azure-private-peering"></a>Azure özel eşlemesini silmek için
Aşağıdaki cmdlet'i çalıştırarak eşleme yapılandırmanızı kaldırabilirsiniz.

> [!WARNING]
> Bu cmdlet'i çalıştırmadan önce ExpressRoute bağlantı hattından tüm sanal ağların bağlantısının kaldırıldığından emin olmalısınız. 
> 
> 

    Remove-AzureBGPPeering -AccessType Private -ServiceKey "*********************************"


## <a name="azure-public-peering"></a>Azure ortak eşleme
Bu bölümde bir ExpressRoute bağlantı hattı için Azure ortak eşleme yapılandırmasını oluşturma, alma, güncelleştirme ve silme hakkında yönergeler açıklanmaktadır.

### <a name="to-create-azure-public-peering"></a>Azure ortak eşlemesi oluşturmak için
1. **ExpressRoute için PowerShell modülünü içeri aktarın.**
   
    ExpressRoute cmdlet'lerini kullanmaya başlamak için PowerShell oturumuna Azure ve ExpressRoute modülleri içeri aktarmalısınız. Azure ve ExpressRoute modüllerini PowerShell oturumuna içeri aktarmak için aşağıdaki komutları çalıştırın. Sürüm farklılık gösterebilir.   
   
        Import-Module 'C:\Program Files\WindowsPowerShell\Modules\Azure\5.1.1\Azure\Azure.psd1'
        Import-Module 'C:\Program Files\WindowsPowerShell\Modules\Azure\5.1.1\ExpressRoute\ExpressRoute.psd1'
2. **ExpressRoute bağlantı hattı oluşturma**
   
    Bir [ExpressRoute bağlantı hattı](expressroute-howto-circuit-classic.md) oluşturmak için yönergeleri izleyin ve bağlantı sağlayıcısından bağlantı hattını sağlamasını isteyin. Bağlantı sağlayıcınız yönetilen Katman 3 hizmetleri sunuyorsa, bağlantı sağlayıcınızdan sizin için Azure ortak eşlemeyi etkinleştirmesini isteyebilirsiniz. Bu durumda, sonraki bölümlerde listelenen yönergeleri izlemeniz gerekmez. Ancak, bağlantı sağlayıcınız yönlendirmeyi sizin için yönetmiyorsa, bağlantı hattınızı oluşturduktan sonra aşağıdaki yönergeleri izleyin.
3. **Expressroute bağlantı hattının sağlandığından emin olmak için kontrol edin**
   
    Önce ExpressRoute ağ geçidinin Sağlandığından ve Etkin durumda olduğundan emin olmanız gerekir. Aşağıdaki örneğe bakın.
   
        PS C:\> Get-AzureDedicatedCircuit -ServiceKey "*********************************"
   
        Bandwidth                        : 200
        CircuitName                      : MyTestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Standard
        Status                           : Enabled
   
    Bağlantı hattı hazırlandı ve etkin gösterdiğinden emin olun. Seçili değilse, bağlantı hattınız gerekli durumu ve durumunu almak için bağlantı sağlayıcınız ile çalışır.
   
        ServiceProviderProvisioningState : Provisioned
        Status                           : Enabled
4. **Bağlantı hattı için Azure ortak eşlemesini yapılandırın**
   
    Devam etmeden önce aşağıdaki bilgilere sahip olduğunuzdan emin olun.
   
   * Birincil bağlantı için bir /30 alt ağı. Bu geçerli bir ortak IPv4 öneki olmalıdır.
   * İkincil bağlantı için bir /30 alt ağı. Bu geçerli bir ortak IPv4 öneki olmalıdır.
   * Bu eşlemenin kurulacak geçerli bir VLAN kimliği. Bağlantı hattındaki başka bir eşlemenin aynı VLAN kimliğini kullanmadığından emin olun.
   * Eşleme için AS numarası. 2 bayt ve 4 bayt AS numaralarını kullanabilirsiniz.
   * Kullanmayı seçerseniz bir MD5 karma değeri. **Bu isteğe bağlıdır**.
     
    Bağlantı hattınız için Azure özel eşlemesini yapılandırmak üzere aşağıdaki cmdlet’i çalıştırabilirsiniz.
     
        Yeni AzureBGPPeering - AccessType ortak - ServiceKey "***" - PrimaryPeerSubnet "131.107.0.0/30" - SecondaryPeerSubnet "131.107.0.4/30" - PeerAsn 1234 - Vlanıd 200
     
    Bir MD5 karma değeri kullanmayı seçerseniz, aşağıdaki cmdlet'i kullanabilirsiniz
     
        Yeni AzureBGPPeering - AccessType ortak - ServiceKey "***" - PrimaryPeerSubnet "131.107.0.0/30" - SecondaryPeerSubnet "131.107.0.4/30" - PeerAsn 1234 - Vlanıd 200 - SharedKey "A1B2C3D4"
     
     > [!IMPORTANT]
     > AS numaranızı müşteri ASN’si değil eşleme ASN’si olarak belirttiğinizden emin olun.
     > 
     > 

### <a name="to-view-azure-public-peering-details"></a>Azure ortak eşleme ayrıntılarını görüntülemek için
Aşağıdaki cmdlet'i kullanarak yapılandırma ayrıntılarını alabilirsiniz

    Get-AzureBGPPeering -AccessType Public -ServiceKey "*********************************"

    AdvertisedPublicPrefixes       : 
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 
    PeerAsn                        : 1234
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 131.107.0.0/30
    RoutingRegistryName            : 
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 131.107.0.4/30
    State                          : Enabled
    VlanId                         : 200


### <a name="to-update-azure-public-peering-configuration"></a>Azure ortak eşleme yapılandırmasını güncelleştirmek için
Aşağıdaki cmdlet'i kullanarak yapılandırmanın herhangi bir bölümünü güncelleştirebilirsiniz

    Set-AzureBGPPeering -AccessType Public -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -PeerAsn 1234 -VlanId 600 -SharedKey "A1B2C3D4"

Yukarıdaki örnekte, bağlantı hattının VLAN kimliği 200'den 600’e güncelleştiriliyor.

### <a name="to-delete-azure-public-peering"></a>Azure ortak eşlemesini silmek için
Aşağıdaki cmdlet'i çalıştırarak eşleme yapılandırmanızı kaldırabilirsiniz

    Remove-AzureBGPPeering -AccessType Public -ServiceKey "*********************************"

## <a name="microsoft-peering"></a>Microsoft eşlemesi
Bu bölümde bir ExpressRoute bağlantı hattı için Microsoft eşleme yapılandırmasını oluşturma, alma, güncelleştirme ve silme hakkında yönergeler açıklanmaktadır. 

### <a name="to-create-microsoft-peering"></a>Microsoft eşlemesi oluşturmak için
1. **ExpressRoute için PowerShell modülünü içeri aktarın.**
   
    ExpressRoute cmdlet'lerini kullanmaya başlamak için PowerShell oturumuna Azure ve ExpressRoute modülleri içeri aktarmalısınız. Azure ve ExpressRoute modüllerini PowerShell oturumuna içeri aktarmak için aşağıdaki komutları çalıştırın. Sürüm farklılık gösterebilir.   
   
        Import-Module 'C:\Program Files\WindowsPowerShell\Modules\Azure\5.1.1\Azure\Azure.psd1'
        Import-Module 'C:\Program Files\WindowsPowerShell\Modules\Azure\5.1.1\ExpressRoute\ExpressRoute.psd1'
2. **ExpressRoute bağlantı hattı oluşturma**
   
    Bir [ExpressRoute bağlantı hattı](expressroute-howto-circuit-classic.md) oluşturmak için yönergeleri izleyin ve bağlantı sağlayıcısından bağlantı hattını sağlamasını isteyin. Bağlantı sağlayıcınız yönetilen Katman 3 hizmetleri sunuyorsa, bağlantı sağlayıcınızdan sizin için Azure özel eşlemeyi etkinleştirmesini isteyebilirsiniz. Bu durumda, sonraki bölümlerde listelenen yönergeleri izlemeniz gerekmez. Ancak, bağlantı sağlayıcınız yönlendirmeyi sizin için yönetmiyorsa, bağlantı hattınızı oluşturduktan sonra aşağıdaki yönergeleri izleyin.
3. **Expressroute bağlantı hattının sağlandığından emin olmak için kontrol edin**
   
    Expressroute bağlantı hattı hazırlandı ve etkin durumda olup olmadığını görmek için ilk olarak işaretlemeniz gerekir.
   
        PS C:\> Get-AzureDedicatedCircuit -ServiceKey "*********************************"
   
        Bandwidth                        : 200
        CircuitName                      : MyTestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Standard
        Status                           : Enabled
   
    Bağlantı hattı hazırlandı ve etkin gösterdiğinden emin olun. Seçili değilse, bağlantı hattınız gerekli durumu ve durumunu almak için bağlantı sağlayıcınız ile çalışır.
   
        ServiceProviderProvisioningState : Provisioned
        Status                           : Enabled
4. **Microsoft bağlantı hattı için eşlemesini yapılandırın**
   
    Devam etmeden önce aşağıdaki bilgilere sahip olduğunuzdan emin olun.
   
   * Birincil bağlantı için bir /30 alt ağı. Bu size ait ve bir RIR / IRR içinde kayıtlı bir geçerli ortak IPv4 ön eki olmalıdır.
   * İkincil bağlantı için bir /30 alt ağı. Bu size ait ve bir RIR / IRR içinde kayıtlı bir geçerli ortak IPv4 ön eki olmalıdır.
   * Bu eşlemenin kurulacağı geçerli bir VLAN kimliği. Bağlantı hattındaki başka bir eşlemenin aynı VLAN kimliğini kullanmadığından emin olun.
   * Eşleme için AS numarası. 2 bayt ve 4 bayt AS numaralarını kullanabilirsiniz.
   * Tanıtılan önekler: BGP oturumunda tanıtmayı planladığınız tüm öneklerin bir listesini sağlamanız gerekir. Yalnızca genel IP adresi önekleri kabul edilir. Bir ön ek kümesi göndermeyi planlıyorsanız, virgülle ayrılmış bir liste gönderebilirsiniz. Bu önekler size bir RIR / IRR içinde kaydedilmiş olmalıdır.
   * Müşteri ASN’si: Eşleme AS numarasına kayıtlı olmayan önekler tanıtıyorsanız, kayıtlı oldukları AS numarasını belirtebilirsiniz. **Bu isteğe bağlıdır**.
   * Yönlendirme Kayıt Defteri Adı: AS numarası ve öneklerinin kaydedildiği RIR / IRR’yi belirtebilirsiniz.
   * Kullanmayı seçerseniz bir MD5 karma değeri. **Bu isteğe bağlıdır.**
     
    Bağlantı hattınız için Microsoft pering yapılandırmak için aşağıdaki cmdlet'i çalıştırabilirsiniz.
     
        Yeni AzureBGPPeering - AccessType Microsoft - ServiceKey "***" - PrimaryPeerSubnet "131.107.0.0/30" - SecondaryPeerSubnet "131.107.0.4/30" - Vlanıd 300 - PeerAsn 1234 - CustomerAsn 2245 - AdvertisedPublicPrefixes "123.0.0.0/30" - RoutingRegistryName "ARIN" - SharedKey "A1B2C3D4"

### <a name="to-view-microsoft-peering-details"></a>Microsoft eşleme ayrıntılarını görüntülemek için
Aşağıdaki cmdlet'i kullanarak yapılandırma ayrıntılarını alabilirsiniz.

    Get-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************"

    AdvertisedPublicPrefixes       : 123.0.0.0/30
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 2245
    PeerAsn                        : 1234
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 10.0.0.0/30
    RoutingRegistryName            : ARIN
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 10.0.0.4/30
    State                          : Enabled
    VlanId                         : 300


### <a name="to-update-microsoft-peering-configuration"></a>Microsoft eşlemesi yapılandırmasını güncelleştirmek için
Aşağıdaki cmdlet'i kullanarak yapılandırmanın herhangi bir bölümünü güncelleştirebilirsiniz.

    Set-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -VlanId 300 -PeerAsn 1234 -CustomerAsn 2245 -AdvertisedPublicPrefixes "123.0.0.0/30" -RoutingRegistryName "ARIN" -SharedKey "A1B2C3D4"

### <a name="to-delete-microsoft-peering"></a>Microsoft eşlemesini silmek için
Aşağıdaki cmdlet'i çalıştırarak eşleme yapılandırmanızı kaldırabilirsiniz.

    Remove-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************"

## <a name="next-steps"></a>Sonraki adımlar
Ardından, [expressroute bağlantı hattına bir VNet bağlama](expressroute-howto-linkvnet-classic.md).

* İş akışları hakkında daha fazla bilgi için bkz: [ExpressRoute iş akışları](expressroute-workflows.md).
* Bağlantı hattı eşlemesi hakkında daha fazla bilgi için bkz. [ExpressRoute bağlantı hattı ve yönlendirme etki alanları](expressroute-circuit-peerings.md).

