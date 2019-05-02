---
title: 'Bir bağlantı hattı için - ExpressRoute eşlemesi yapılandırın: Azure: Klasik | Microsoft Docs'
description: Bu makalede, bir ExpressRoute bağlantı hattı için özel, ortak ve Microsoft eşlemesinin nasıl oluşturulduğu ve sağlandığı adım adım anlatılmaktadır. Bu makalede ayrıca bağlantı hattınızın durumunu denetleme, bağlantı hattını güncelleştirme veya silme işlemlerinin nasıl yapıldığı da anlatılmaktadır.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 04/24/2019
ms.author: cherylmc
ms.custom: seodec18
ms.openlocfilehash: d1662d17f37e668e989103989df9de49036bab6a
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64726205"
---
# <a name="create-and-modify-peering-for-an-expressroute-circuit-classic"></a>Bir ExpressRoute bağlantı hattı için (Klasik) eşlemesi oluşturma ve değiştirme
> [!div class="op_single_selector"]
> * [Azure portal](expressroute-howto-routing-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-routing-arm.md)
> * [Azure CLI](howto-routing-cli.md)
> * [Video - özel eşdüzey hizmet sağlama](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-private-peering-for-your-expressroute-circuit)
> * [Video - genel eşdüzey hizmet sağlama](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-public-peering-for-your-expressroute-circuit)
> * [Video - Microsoft eşdüzey hizmet sağlama](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-microsoft-peering-for-your-expressroute-circuit)
> * [PowerShell (klasik)](expressroute-howto-routing-classic.md)
> 

Bu makalede PowerShell ve klasik dağıtım modeli kullanarak ExpressRoute bağlantı hattı için eşleme/yönlendirme yapılandırması oluşturma ve yönetme için adımlarında size kılavuzluk eder. Aşağıdaki adımlarda ayrıca bir ExpressRoute bağlantı hattının durumunu denetleme, güncelleştirme veya bağlantı hattını silme ve eşlemelerin sağlamasını kaldırma işlemleri de anlatılmaktadır. Bir, iki veya üç eşlemenin tamamını (Azure özel, Azure genel ve Microsoft) bir ExpressRoute bağlantı hattı için yapılandırabilirsiniz. Eşlemeleri seçtiğiniz herhangi bir sırayla yapılandırabilirsiniz. Ancak, her eşlemenin yapılandırmasını birer birer tamamladığınızdan emin olmanız gerekir. 

Bu yönergeler yalnızca Katman 2 bağlantı hizmetleri sunan hizmet sağlayıcıları ile oluşturulan bağlantı hatları için geçerlidir. Yönetilen sunan bir hizmet sağlayıcısı kullanıyorsanız, Katman 3 Hizmetleri (genellikle gibi bir IPVPN MPLS), bağlantı sağlayıcınız yapılandıracak ve yönlendirmeyi sizin için yönetme.

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

**Azure dağıtım modelleri hakkında**

[!INCLUDE [vpn-gateway-classic-rm](../../includes/vpn-gateway-classic-rm-include.md)]


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="configuration-prerequisites"></a>Yapılandırma önkoşulları

* Yapılandırmaya başlamadan önce [önkoşullar](expressroute-prerequisites.md) sayfasını, [yönlendirme gereksinimleri](expressroute-routing.md) sayfasını ve [iş akışları](expressroute-workflows.md) sayfasını gözden geçirdiğinizden emin olun.
* Etkin bir ExpressRoute bağlantı hattınızın olması gerekir. Yönergelerini izleyin [ExpressRoute devresi oluşturma](expressroute-howto-circuit-classic.md) ve devam etmeden önce bağlantı sağlayıcınız tarafından etkinleştirilen devreniz olduğunu. Aşağıda açıklanan cmdlet’leri çalıştırmanız için ExpressRoute bağlantı hattının sağlanmış ve etkin durumda olması gerekir.

### <a name="download-the-latest-powershell-cmdlets"></a>En son PowerShell cmdlet'lerini indirin

Azure Hizmet Yönetimi (SM) PowerShell modüllerine ve ExpressRoute modülünün en son sürümlerini yükleyin. Aşağıdaki örnek kullanırken, cmdlet'leri daha yeni sürümleri çıktıkça sürüm numarasını (Bu örnekte, 5.1.1) değişeceğini unutmayın.

```powershell
Import-Module 'C:\Program Files\WindowsPowerShell\Modules\Azure\5.1.1\Azure\Azure.psd1'
Import-Module 'C:\Program Files\WindowsPowerShell\Modules\Azure\5.1.1\ExpressRoute\ExpressRoute.psd1'
```

Daha fazla bilgi için [Azure PowerShell cmdlet'lerini kullanmaya Başlarken](/powershell/azure/overview) bilgisayarınızın Azure PowerShell modüllerinin kullanacak şekilde yapılandırma hakkında adım adım yönergeler için.

### <a name="sign-in"></a>Oturum aç

Azure hesabınızda oturum açmak için aşağıdaki örnekleri kullanın:

1. PowerShell konsolunuzu yükseltilmiş haklarla açın ve hesabınıza bağlanın.

   ```powershell
   Connect-AzAccount
   ```
2. Hesapla ilişkili abonelikleri kontrol edin.

   ```powershell
   Get-AzSubscription
   ```
3. Birden fazla aboneliğiniz varsa, kullanmak istediğiniz aboneliği seçin.

   ```powershell
   Select-AzSubscription -SubscriptionName "Replace_with_your_subscription_name"
   ```

4. Ardından, Azure aboneliğiniz için PowerShell Klasik dağıtım modeli için eklemek için aşağıdaki cmdlet'i kullanın.

   ```powershell
   Add-AzureAccount
   ```

## <a name="azure-private-peering"></a>Azure özel eşlemesi

Bu bölümde bir ExpressRoute bağlantı hattı için Azure özel eşleme yapılandırmasını oluşturma, alma, güncelleştirme ve silme hakkında yönergeler açıklanmaktadır. 

### <a name="to-create-azure-private-peering"></a>Azure özel eşlemesi oluşturmak için

1. **Bir ExpressRoute bağlantı hattı oluşturun.**

   Bir [ExpressRoute bağlantı hattı](expressroute-howto-circuit-classic.md) oluşturmak için yönergeleri izleyin ve bağlantı sağlayıcısından bağlantı hattını sağlamasını isteyin. Bağlantı sağlayıcınız yönetilen Katman 3 hizmetleri sunuyorsa, bağlantı sağlayıcınızdan sizin için Azure özel eşlemeyi etkinleştirmesini isteyebilirsiniz. Bu durumda, sonraki bölümlerde listelenen yönergeleri izlemeniz gerekmez. Ancak, bağlantı sağlayıcınız yönlendirmeyi sizin için yönetmiyorsa, bağlantı hattınızı oluşturduktan sonra aşağıdaki yönergeleri izleyin.
2. **ExpressRoute bağlantı hattının sağlandığından emin olmak için kontrol edin.**
   
   ExpressRoute bağlantı hattı sağlanan ve ayrıca etkin olmadığını denetleyin.

   ```powershell
   Get-AzureDedicatedCircuit -ServiceKey "*********************************"
   ```

   Döndür:

   ```powershell
   Bandwidth                        : 200
   CircuitName                      : MyTestCircuit
   Location                         : Silicon Valley
   ServiceKey                       : *********************************
   ServiceProviderName              : equinix
   ServiceProviderProvisioningState : Provisioned
   Sku                              : Standard
   Status                           : Enabled
   ```
   
   Bağlantı hattı sağlanıyor ve etkin gösterildiğinden emin olun. Aksi takdirde devreniz gerekli durumu ve durumunu almak için bağlantı sağlayıcınız ile çalışır.

   ```powershell
   ServiceProviderProvisioningState : Provisioned
   Status                           : Enabled
   ```
3. **Bağlantı hattı için Azure özel eşlemesini yapılandırın.**

   Sonraki adımlara devam etmeden önce aşağıdaki öğelerin bulunduğundan emin olun:
   
   * Birincil bağlantı için bir /30 alt ağı. Bu, sanal ağlar için ayrılmış herhangi bir adres alanının parçası olmamalıdır.
   * İkincil bağlantı için bir /30 alt ağı. Bu, sanal ağlar için ayrılmış herhangi bir adres alanının parçası olmamalıdır.
   * Bu eşlemenin kurulacağı geçerli bir VLAN kimliği. Hiçbir bağlantı diğer eşlemesi aynı VLAN kimliğini kullanmadığından emin olun
   * Eşleme için AS numarası. 2 bayt ve 4 bayt AS numaralarını kullanabilirsiniz. Bu eşleme için özel bir AS numarası kullanabilirsiniz. 65515 kullanmıyorsanız doğrulayın.
   * Kullanmayı seçerseniz bir MD5 karma değeri. **İsteğe bağlı**.
     
   Bağlantı hattınız için Azure özel eşlemesini yapılandırmak üzere aşağıdaki örneği kullanabilirsiniz:

   ```powershell
   New-AzureBGPPeering -AccessType Private -ServiceKey "*********************************" -PrimaryPeerSubnet "10.0.0.0/30" -SecondaryPeerSubnet "10.0.0.4/30" -PeerAsn 1234 -VlanId 100
   ```    

   Bir MD5 karma değeri kullanmak istiyorsanız, bağlantı hattınız için özel eşlemesini yapılandırmak üzere aşağıdaki örneği kullanın:

   ```powershell
   New-AzureBGPPeering -AccessType Private -ServiceKey "*********************************" -PrimaryPeerSubnet "10.0.0.0/30" -SecondaryPeerSubnet "10.0.0.4/30" -PeerAsn 1234 -VlanId 100 -SharedKey "A1B2C3D4"
   ```
     
   > [!IMPORTANT]
   > AS numaranızı eşleme ASN'si olarak müşteri ASN'si olarak belirttiğinizden emin olun.
   > 

### <a name="to-view-azure-private-peering-details"></a>Azure özel eşleme ayrıntılarını görüntülemek için

Aşağıdaki cmdlet'i kullanarak yapılandırma ayrıntılarını görüntüleyebilirsiniz:

```powershell
Get-AzureBGPPeering -AccessType Private -ServiceKey "*********************************"
```

Döndür:

```
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
```

### <a name="to-update-azure-private-peering-configuration"></a>Azure özel eşleme yapılandırmasını güncelleştirmek için

Aşağıdaki cmdlet'i kullanarak yapılandırmanın herhangi bir bölümünü güncelleştirebilirsiniz. Aşağıdaki örnekte, bağlantı hattının VLAN kimliği 100'den 500'e güncelleştiriliyor.

```powershell
Set-AzureBGPPeering -AccessType Private -ServiceKey "*********************************" -PrimaryPeerSubnet "10.0.0.0/30" -SecondaryPeerSubnet "10.0.0.4/30" -PeerAsn 1234 -VlanId 500 -SharedKey "A1B2C3D4"
```

### <a name="to-delete-azure-private-peering"></a>Azure özel eşlemeyi silmek için

Aşağıdaki cmdlet'i çalıştırarak eşleme yapılandırmanızı kaldırabilirsiniz. Bu cmdlet'i çalıştırmadan önce tüm sanal ağları ExpressRoute bağlantı bağlantısı olduğundan emin olmanız gerekir.

```powershell
Remove-AzureBGPPeering -AccessType Private -ServiceKey "*********************************"
```

## <a name="azure-public-peering"></a>Azure ortak eşleme

Bu bölümde bir ExpressRoute bağlantı hattı için Azure ortak eşleme yapılandırmasını oluşturma, alma, güncelleştirme ve silme hakkında yönergeler açıklanmaktadır.

> [!NOTE]
> Azure genel eşdüzey hizmet sağlama, yeni bağlantı hatları için kullanım dışı bırakılmıştır.
>

### <a name="to-create-azure-public-peering"></a>Azure ortak eşlemesi oluşturmak için

1. **ExpressRoute bağlantı hattı oluşturma**

   Bir [ExpressRoute bağlantı hattı](expressroute-howto-circuit-classic.md) oluşturmak için yönergeleri izleyin ve bağlantı sağlayıcısından bağlantı hattını sağlamasını isteyin. Bağlantı sağlayıcınız yönetilen Katman 3 hizmetleri sunuyorsa, bağlantı sağlayıcınızdan sizin için Azure ortak eşlemeyi etkinleştirmesini isteyebilirsiniz. Bu durumda, sonraki bölümlerde listelenen yönergeleri izlemeniz gerekmez. Ancak, bağlantı sağlayıcınız yönlendirmeyi sizin için yönetmiyorsa, bağlantı hattınızı oluşturduktan sonra aşağıdaki yönergeleri izleyin.
2. **Onay ExpressRoute bağlantı hattının sağlandığından emin olun**

   Önce ExpressRoute ağ geçidinin Sağlandığından ve Etkin durumda olduğundan emin olmanız gerekir.

   ```powershell
   Get-AzureDedicatedCircuit -ServiceKey "*********************************"
   ```

   Döndür:

   ```powershell
   Bandwidth                        : 200
   CircuitName                      : MyTestCircuit
   Location                         : Silicon Valley
   ServiceKey                       : *********************************
   ServiceProviderName              : equinix
   ServiceProviderProvisioningState : Provisioned
   Sku                              : Standard
   Status                           : Enabled
   ```
   
   Bağlantı hattı sağlanıyor ve etkin gösterdiğini doğrulayın. Aksi takdirde devreniz gerekli durumu ve durumunu almak için bağlantı sağlayıcınız ile çalışır.

   ```powershell
   ServiceProviderProvisioningState : Provisioned
   Status                           : Enabled
   ```
4. **Bağlantı hattı için Azure ortak eşlemesini yapılandırın**
   
   Devam etmeden önce aşağıdaki bilgilere sahip olduğundan emin olun:
   
   * Birincil bağlantı için bir /30 alt ağı. Bu geçerli bir ortak IPv4 öneki olmalıdır.
   * İkincil bağlantı için bir /30 alt ağı. Bu geçerli bir ortak IPv4 öneki olmalıdır.
   * Bu eşlemenin kurulacağı geçerli bir VLAN kimliği. Hiçbir bağlantı diğer eşlemesi aynı VLAN kimliğini kullanmadığından emin olun
   * Eşleme için AS numarası. 2 bayt ve 4 bayt AS numaralarını kullanabilirsiniz.
   * Kullanmayı seçerseniz bir MD5 karma değeri. **İsteğe bağlı**.

   > [!IMPORTANT]
   > AS numaranızı müşteri ASN'si değil eşleme ASN'si de belirttiğinizden emin olun.
   >  
     
   Bağlantı hattınız için Azure ortak eşlemesini yapılandırmak üzere aşağıdaki örneği kullanabilirsiniz:

   ```powershell
   New-AzureBGPPeering -AccessType Public -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -PeerAsn 1234 -VlanId 200
   ```
     
   Bir MD5 karma değeri kullanmak istiyorsanız, bağlantı hattınızı yapılandırmak için aşağıdaki örneği kullanın:
     
   ```powershell
   New-AzureBGPPeering -AccessType Public -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -PeerAsn 1234 -VlanId 200 -SharedKey "A1B2C3D4"
   ```
     
### <a name="to-view-azure-public-peering-details"></a>Azure ortak eşleme ayrıntılarını görüntülemek için

Yapılandırma ayrıntılarını görüntülemek için aşağıdaki cmdlet'i kullanın:

```powershell
Get-AzureBGPPeering -AccessType Public -ServiceKey "*********************************"
```

Döndür:

```powershell
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
```

### <a name="to-update-azure-public-peering-configuration"></a>Azure ortak eşleme yapılandırmasını güncelleştirmek için

Aşağıdaki cmdlet'i kullanarak yapılandırmanın herhangi bir bölümünü güncelleştirebilirsiniz. Bu örnekte, bağlantı hattının VLAN kimliği 200'den 600 olarak güncelleştiriliyor.

```powershell
Set-AzureBGPPeering -AccessType Public -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -PeerAsn 1234 -VlanId 600 -SharedKey "A1B2C3D4"
```

Bağlantı hattı sağlanıyor ve etkin gösterdiğini doğrulayın. 
### <a name="to-delete-azure-public-peering"></a>Azure ortak eşlemesini silmek için

Aşağıdaki cmdlet'i çalıştırarak eşleme yapılandırmanızı kaldırabilirsiniz:

```powershell
Remove-AzureBGPPeering -AccessType Public -ServiceKey "*********************************"
```

## <a name="microsoft-peering"></a>Microsoft eşlemesi

Bu bölümde bir ExpressRoute bağlantı hattı için Microsoft eşleme yapılandırmasını oluşturma, alma, güncelleştirme ve silme hakkında yönergeler açıklanmaktadır. 

### <a name="to-create-microsoft-peering"></a>Microsoft eşlemesi oluşturmak için

1. **ExpressRoute bağlantı hattı oluşturma**
  
   Bir [ExpressRoute bağlantı hattı](expressroute-howto-circuit-classic.md) oluşturmak için yönergeleri izleyin ve bağlantı sağlayıcısından bağlantı hattını sağlamasını isteyin. Bağlantı sağlayıcınız yönetilen Katman 3 hizmetleri sunuyorsa, bağlantı sağlayıcınızdan sizin için Azure özel eşlemeyi etkinleştirmesini isteyebilirsiniz. Bu durumda, sonraki bölümlerde listelenen yönergeleri izlemeniz gerekmez. Ancak, bağlantı sağlayıcınız yönlendirmeyi sizin için yönetmiyorsa, bağlantı hattınızı oluşturduktan sonra aşağıdaki yönergeleri izleyin.
2. **Onay ExpressRoute bağlantı hattının sağlandığından emin olun**

   Bağlantı hattı sağlanıyor ve etkin gösterdiğini doğrulayın. 
   
   ```powershell
   Get-AzureDedicatedCircuit -ServiceKey "*********************************"
   ```

   Döndür:
   
   ```powershell
   Bandwidth                        : 200
   CircuitName                      : MyTestCircuit
   Location                         : Silicon Valley
   ServiceKey                       : *********************************
   ServiceProviderName              : equinix
   ServiceProviderProvisioningState : Provisioned
   Sku                              : Standard
   Status                           : Enabled
   ```
   
   Bağlantı hattı sağlanıyor ve etkin gösterdiğini doğrulayın. Aksi takdirde devreniz gerekli durumu ve durumunu almak için bağlantı sağlayıcınız ile çalışır.

   ```powershell
   ServiceProviderProvisioningState : Provisioned
   Status                           : Enabled
   ```
3. **Microsoft bağlantı hattı için eşleme yapılandırma**
   
    Devam etmeden önce aşağıdaki bilgilere sahip olduğunuzdan emin olun.
   
   * Birincil bağlantı için bir /30 alt ağı. Bu size ait ve bir RIR / IRR içinde kayıtlı bir geçerli ortak IPv4 ön eki olmalıdır.
   * İkincil bağlantı için bir /30 alt ağı. Bu size ait ve bir RIR / IRR içinde kayıtlı bir geçerli ortak IPv4 ön eki olmalıdır.
   * Bu eşlemenin kurulacağı geçerli bir VLAN kimliği. Hiçbir bağlantı diğer eşlemesi aynı VLAN kimliğini kullanmadığından emin olun
   * Eşleme için AS numarası. 2 bayt ve 4 bayt AS numaralarını kullanabilirsiniz.
   * Tanıtılan önekler: BGP oturumunda tanıtmayı planladığınız tüm ön eklerin listesini sağlamanız gerekir. Yalnızca ortak IP adresi ön ekleri kabul edilir. Ön ek kümesi göndermeyi planlıyorsanız, virgülle ayrılmış bir liste gönderebilirsiniz. Bu ön ekler size bir RIR / IRR içinde kaydedilmiş olmalıdır.
   * Müşteri ASN'si: Eşleme AS numarasına kayıtlı olmayan önekler tanıtıyorsanız, kayıtlı oldukları AS numarasını belirtebilirsiniz. **İsteğe bağlı**.
   * Yönlendirme kayıt defteri adı: Belirtebileceğiniz RIR / IRR'yi AS numarası ve öneklerinin kaydedildiği rır.
   * Kullanmayı seçerseniz bir MD5 karma değeri. **İsteğe bağlı.**
     
   Microsoft, bağlantı hattı için eşleme yapılandırmak üzere aşağıdaki cmdlet'i çalıştırın:
 
   ```powershell
   New-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -VlanId 300 -PeerAsn 1234 -CustomerAsn 2245 -AdvertisedPublicPrefixes "123.0.0.0/30" -RoutingRegistryName "ARIN" -SharedKey "A1B2C3D4"
   ```

### <a name="to-view-microsoft-peering-details"></a>Microsoft eşleme ayrıntılarını görüntülemek için

Aşağıdaki cmdlet'i kullanarak yapılandırma ayrıntılarını görüntüleyebilirsiniz:

```powershell
Get-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************"
```
Döndür:

```powershell
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
```

### <a name="to-update-microsoft-peering-configuration"></a>Microsoft eşlemesi yapılandırmasını güncelleştirmek için

Aşağıdaki cmdlet'i kullanarak yapılandırmanın herhangi bir bölümünü güncelleştirebilirsiniz:

```powershell
Set-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -VlanId 300 -PeerAsn 1234 -CustomerAsn 2245 -AdvertisedPublicPrefixes "123.0.0.0/30" -RoutingRegistryName "ARIN" -SharedKey "A1B2C3D4"
```

### <a name="to-delete-microsoft-peering"></a>Microsoft eşlemesini silmek için

Aşağıdaki cmdlet'i çalıştırarak eşleme yapılandırmanızı kaldırabilirsiniz:

```powershell
Remove-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************"
```

## <a name="next-steps"></a>Sonraki adımlar

Ardından, [bir ExpressRoute bağlantı hattına bir VNet bağlama](expressroute-howto-linkvnet-classic.md).

* İş akışları hakkında daha fazla bilgi için bkz. [ExpressRoute iş akışları](expressroute-workflows.md).
* Bağlantı hattı eşlemesi hakkında daha fazla bilgi için bkz. [ExpressRoute bağlantı hattı ve yönlendirme etki alanları](expressroute-circuit-peerings.md).
