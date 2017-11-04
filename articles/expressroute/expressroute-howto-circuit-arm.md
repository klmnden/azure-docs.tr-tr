---
title: "Oluşturma ve bir expressroute bağlantı hattı değiştirme: PowerShell: Azure Resource Manager | Microsoft Docs"
description: "Bu makalede, oluşturmak, sağlamak, doğrulayın, güncelleştirme, silme ve bir expressroute bağlantı hattı yetkisini kaldırma açıklar."
documentationcenter: na
services: expressroute
author: ganesr
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f997182e-9b25-4a7a-b079-b004221dadcc
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/18/2017
ms.author: ganesr;cherylmc
ms.openlocfilehash: ef092a48994b68268109cb98bd6cd4526e259d5b
ms.sourcegitcommit: cf4c0ad6a628dfcbf5b841896ab3c78b97d4eafd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/21/2017
---
# <a name="create-and-modify-an-expressroute-circuit-using-powershell"></a>Oluşturma ve PowerShell kullanarak bir expressroute bağlantı hattı değiştirme
> [!div class="op_single_selector"]
> * [Azure portal](expressroute-howto-circuit-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-circuit-arm.md)
> * [Azure CLI](howto-circuit-cli.md)
> * [Video - Azure portalı](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [PowerShell (klasik)](expressroute-howto-circuit-classic.md)
>

Bu makalede PowerShell cmdlet'leri ve Azure Resource Manager dağıtım modeli kullanarak bir Azure expressroute oluşturmayı açıklar. Bu makalede ayrıca devrenin durumunu denetleyin, güncelleştirme veya silme ve onu yetkisini kaldırma kullanmayı gösterir.

## <a name="before-you-begin"></a>Başlamadan önce
* Azure Resource Manager PowerShell cmdlet'lerinin en son sürümünü yükleyin. Daha fazla bilgi için bkz: [Azure PowerShell genel bakış](/powershell/azure/overview).
* Gözden geçirme [Önkoşullar](expressroute-prerequisites.md) ve [iş akışları](expressroute-workflows.md) yapılandırmaya başlamadan önce.


## <a name="create"></a>Oluşturma ve bir expressroute bağlantı hattı sağlama
### <a name="1-sign-in-to-your-azure-account-and-select-your-subscription"></a>1. Azure hesabınızda oturum açın ve aboneliğinizi seçin
Yapılandırmanızı başlamak için Azure hesabınızda oturum açın. Aşağıdaki örnekler bağlanmanıza yardımcı olması için kullanın:

```powershell
Login-AzureRmAccount
```

Hesap için abonelikleri kontrol edin:

```powershell
Get-AzureRmSubscription
```

Bir expressroute bağlantı hattı için oluşturmak istediğiniz aboneliği seçin:

```powershell
Select-AzureRmSubscription -SubscriptionId "<subscription ID>"
```

### <a name="2-get-the-list-of-supported-providers-locations-and-bandwidths"></a>2. Desteklenen sağlayıcılar, konumları ve bant genişlikleri listesini alma
Bir expressroute bağlantı hattı oluşturmadan önce desteklenen bağlantı sağlayıcıları, konumları ve bant seçeneklerini listesi gerekir.

PowerShell cmdlet **Get-AzureRmExpressRouteServiceProvider** , sonraki adımlarda kullanacağınız bu bilgiler verir:

```powershell
Get-AzureRmExpressRouteServiceProvider
```

Bağlantı sağlayıcınız listelenip listelenmediğini denetleyin. Bir bağlantı hattı oluşturduğunuzda, daha sonra ihtiyacınız aşağıdaki bilgileri not edin:

* Ad
* PeeringLocations
* BandwidthsOffered

Artık bir expressroute bağlantı hattı oluşturmak hazırsınız.   

### <a name="3-create-an-expressroute-circuit"></a>3. ExpressRoute bağlantı hattı oluşturma
Bir kaynak grubu zaten yoksa, expressroute bağlantı hattı oluşturmadan önce bir oluşturmanız gerekir. Aşağıdaki komutu çalıştırarak bunu yapabilirsiniz:

```powershell
New-AzureRmResourceGroup -Name "ExpressRouteResourceGroup" -Location "West US"
```


Aşağıdaki örnek 200 MB/sn expressroute bağlantı hattı üzerinden Equinix Silikon Vadisi'nde oluşturulacağını gösterir. Farklı bir sağlayıcı ve farklı ayarlar kullanıyorsanız, bu bilgileri isteğiniz yaptığınızda değiştirin. Yeni bir hizmet anahtarı istemek için aşağıdaki örneği kullanın:

```powershell
New-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup" -Location "West US" -SkuTier Standard -SkuFamily MeteredData -ServiceProviderName "Equinix" -PeeringLocation "Silicon Valley" -BandwidthInMbps 200
```

SKU ailesi ve doğru SKU katmanı belirttiğinizden emin olun:

* SKU katmanı, bir ExpressRoute standart ya da bir ExpressRoute premium Eklentisi etkin olup olmadığını belirler. Belirleyebileceğiniz *standart* standart SKU almak için veya *Premium* premium eklenti için.
* SKU ailesi faturalama türü belirler. Belirleyebileceğiniz *Metereddata* ölçülen veri planı için ve *Unlimiteddata* sınırsız veri planı için. Fatura türünden değiştirebileceğiniz *Metereddata* için *Unlimiteddata*, ancak türünden değiştiremezsiniz *Unlimiteddata* için *Metereddata*.

> [!IMPORTANT]
> ExpressRoute bağlantı hattınız bir hizmet anahtarı verilen andan itibaren faturalandırılır. Bağlantı sağlayıcı bağlantı hattı sağlamak hazır olduğunda bu işlemi gerçekleştirmek emin olun.
> 
> 

Yanıt hizmet anahtarını içerir. Aşağıdaki komutu çalıştırarak tüm parametrelerin ayrıntılı açıklamaları alabilirsiniz:

```powershell
get-help New-AzureRmExpressRouteCircuit -detailed
```


### <a name="4-list-all-expressroute-circuits"></a>4. Tüm ExpressRoute bağlantı hatları listesi
Oluşturduğunuz tüm expressroute bağlantı hatları listesini almak için çalıştırın **Get-AzureRmExpressRouteCircuit** komutu:

```powershell
Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
```

Yanıt aşağıdaki örneğe benzer:

    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                         "Tier": "Standard",
                                         "Family": "MeteredData"
                                          }
    CircuitProvisioningState          : Enabled
    ServiceProviderProvisioningState  : NotProvisioned
    ServiceProviderNotes              :
    ServiceProviderProperties         : {
                                          "ServiceProviderName": "Equinix",
                                          "PeeringLocation": "Silicon Valley",
                                          "BandwidthInMbps": 200
                                        }
    ServiceKey                        : **************************************
    Peerings                          : []

Herhangi bir zamanda bu bilgileri kullanarak alabilirsiniz `Get-AzureRmExpressRouteCircuit` cmdlet'i. Hiçbir parametre çağrıyı yapan tüm devreler listeler. Hizmet anahtarınız listelenen *ServiceKey* alan:

```powershell
Get-AzureRmExpressRouteCircuit
```


Yanıt aşağıdaki örneğe benzer:

    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                         "Tier": "Standard",
                                         "Family": "MeteredData"
                                          }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : NotProvisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                         "ServiceProviderName": "Equinix",
                                         "PeeringLocation": "Silicon Valley",
                                         "BandwidthInMbps": 200
                                          }
    ServiceKey                       : **************************************
    Peerings                         : []


Aşağıdaki komutu çalıştırarak tüm parametrelerin ayrıntılı açıklamaları alabilirsiniz:

```powershell
get-help Get-AzureRmExpressRouteCircuit -detailed
```

### <a name="5-send-the-service-key-to-your-connectivity-provider-for-provisioning"></a>5. Hizmet anahtarı sağlamak için bağlantı sağlayıcınızı Gönder
*ServiceProviderProvisioningState* hizmet sağlayıcı tarafında sağlama geçerli durumu hakkında bilgi sağlar. Durum Microsoft tarafında durumunu sağlar. Bağlantı hattı durumları sağlama hakkında daha fazla bilgi için bkz: [iş akışları](expressroute-workflows.md#expressroute-circuit-provisioning-states).

Yeni bir expressroute bağlantı hattı oluşturduğunuzda, bağlantı hattı şu durumda olur:

    ServiceProviderProvisioningState : NotProvisioned
    CircuitProvisioningState         : Enabled



Bağlantı sağlayıcı onu sizin için etkinleştirme sürecinde olduğunda bağlantı hattı için şu durum değiştirir:

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled

Bir expressroute bağlantı hattı kullanabilmek için şu durumda olmalıdır:

    ServiceProviderProvisioningState : Provisioned
    CircuitProvisioningState         : Enabled

### <a name="6-periodically-check-the-status-and-the-state-of-the-circuit-key"></a>6. Durum ve hattı anahtar durumunu düzenli aralıklarla denetleyin
Durum ve hattı anahtar durumunu denetleme, sağlayıcınız hattınız etkin olduğunda bilmenizi sağlar. Bağlantı hattı yapılandırıldıktan sonra *ServiceProviderProvisioningState* olarak görünür *hazırlandı*, aşağıdaki örnekte gösterildiği gibi:

```powershell
Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
```


Yanıt aşağıdaki örneğe benzer:

    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                         "Tier": "Standard",
                                         "Family": "MeteredData"
                                       }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                         "ServiceProviderName": "Equinix",
                                         "PeeringLocation": "Silicon Valley",
                                         "BandwidthInMbps": 200
                                       }
    ServiceKey                       : **************************************
    Peerings                         : []

### <a name="7-create-your-routing-configuration"></a>7. Yönlendirme yapılandırması oluşturma
Adım adım yönergeler için bkz: [expressroute bağlantı hattı yönlendirme yapılandırması](expressroute-howto-routing-arm.md) oluşturup hattı eşlemeler değiştirmek için makale.

> [!IMPORTANT]
> Bu yönergeler yalnızca Katman 2 bağlantı hizmetleri sunan hizmet sağlayıcıları ile oluşturulan bağlantı hatları için geçerlidir. Yönetilen sunan bir hizmet sağlayıcısı kullanıyorsanız, Katman 3 Hizmetleri (genellikle bir IP VPN, MPLS gibi), bağlantı sağlayıcınız yönlendirmeyi sizin için yönetir ve yapılandırır.
> 
> 

### <a name="8-link-a-virtual-network-to-an-expressroute-circuit"></a>8. ExpressRoute bağlantı hattına bir sanal ağı bağlama
Ardından, bir sanal ağ, expressroute bağlantı hattına bağlayın. Kullanım [ExpressRoute bağlantı hatları için sanal ağları bağlama](expressroute-howto-linkvnet-arm.md) makale Resource Manager dağıtım modeliyle çalışırken.

## <a name="getting-the-status-of-an-expressroute-circuit"></a>Bir expressroute bağlantı hattı durumunu alma
Herhangi bir zamanda bu bilgileri kullanarak alabilirsiniz **Get-AzureRmExpressRouteCircuit** cmdlet'i. Hiçbir parametre çağrıyı yapan tüm devreler listeler.

```powershell
Get-AzureRmExpressRouteCircuit
```


Yanıt aşağıdaki örneğe benzer:

    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                         "Tier": "Standard",
                                         "Family": "MeteredData"
                                       }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                            "ServiceProviderName": "Equinix",
                                            "PeeringLocation": "Silicon Valley",
                                            "BandwidthInMbps": 200
                                          }
    ServiceKey                       : **************************************
    Peerings                         : []


Kaynak grubu adı ve bağlantı hattı adı çağrısına parametre olarak geçirerek belirli bir expressroute bağlantı hattı hakkında bilgi alabilirsiniz:

```powershell
Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
```


Yanıt aşağıdaki örneğe benzer:

    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                            "Tier": "Standard",
                                            "Family": "MeteredData"
                                          }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                         "ServiceProviderName": "Equinix",
                                         "PeeringLocation": "Silicon Valley",
                                         "BandwidthInMbps": 200
                                          }
    ServiceKey                       : **************************************
    Peerings                         : []


Aşağıdaki komutu çalıştırarak tüm parametrelerin ayrıntılı açıklamaları alabilirsiniz:

```powershell
get-help get-azurededicatedcircuit -detailed
```

## <a name="modify"></a>Bir expressroute bağlantı hattı değiştirme
Bağlantı etkilemeden belirli bir expressroute bağlantı hattı özelliklerini değiştirebilirsiniz.

Bunu kapalı kalma süresi olmadan aşağıdaki görevleri gerçekleştirebilirsiniz:

* Etkinleştirmek veya devre dışı bir expressroute bağlantı hattı için ExpressRoute premium eklentisi.
* Sağlanmış kapasite kullanılabilir bağlantı noktası, expressroute bağlantı hattı bant genişliğini artırır. Bir bağlantı hattının bant genişliğini önceki sürüme indirme desteklenmiyor. 
* Ölçüm plan sınırsız veri ölçülen verilerden değiştirin. Ölçüm plan sınırsız verilerden ölçülen verileri değiştirme desteklenmez.
* Etkinleştirme ve devre dışı *izin Klasik işlemleri*.

Sınırlar ve sınırlamalar hakkında daha fazla bilgi için bkz: [ExpressRoute SSS](expressroute-faqs.md).

### <a name="to-enable-the-expressroute-premium-add-on"></a>ExpressRoute premium eklentisi etkinleştirmek için
Aşağıdaki PowerShell parçacığını kullanarak, varolan bağlantı hattınız için ExpressRoute premium eklentisi etkinleştirebilirsiniz:

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

$ckt.Sku.Tier = "Premium"
$ckt.sku.Name = "Premium_MeteredData"

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

Bağlantı hattı şimdi etkin ExpressRoute premium eklentisi özellikleri vardır. Komut başarıyla çalıştırıldı hemen premium eklenti özellik faturalama başlayın.

### <a name="to-disable-the-expressroute-premium-add-on"></a>ExpressRoute premium eklentisi devre dışı bırakmak için
> [!IMPORTANT]
> Standart bağlantı hattı için izin daha büyük olan kaynaklar kullanıyorsanız, bu işlemi başarısız olabilir.
> 
> 

Aşağıdaki bilgileri unutmayın:

* Standart Premium'dan düşürmek önce devresine bağlı sanal ağlar sayısı 10'dan az olduğundan emin olmalısınız. Bunu yapmazsanız, güncelleştirme isteği başarısız olur ve biz premium oranlarda fatura.
* Tüm sanal ağları diğer coğrafi bölgelerde bağlantısını gerekir. Bunu yapmazsanız, güncelleştirme isteği başarısız olur ve biz premium oranlarda fatura.
* Yol tablosu özel eşleme için 4. 000'den az yolları olmalıdır. Rota tablosu boyutunuz 4.000 yolları büyükse, BGP oturumu bırakır ve 4.000 tanıtılan ön ek sayısı gider kadar yeniden iler hale olmaz.

Aşağıdaki PowerShell cmdlet'ini kullanarak var olan bağlantı hattı için ExpressRoute premium eklentisi devre dışı bırakabilirsiniz:

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

$ckt.Sku.Tier = "Standard"
$ckt.sku.Name = "Standard_MeteredData"

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="to-update-the-expressroute-circuit-bandwidth"></a>ExpressRoute bağlantı hattı bant genişliğini güncelleştirmek için
Denetimi sağlayıcınız için desteklenen bant genişliği seçenekleri için [ExpressRoute SSS](expressroute-faqs.md). Var olan bağlantı hattınız boyutundan büyük herhangi bir boyutta seçebilirsiniz.

> [!IMPORTANT]
> Varolan bir bağlantı üzerinde yetersiz kapasite ise expressroute bağlantı hattı yeniden başlatmanız gerekebilir. Varsa hiçbir ek kapasite kullanılabilir o konumda bağlantı hattı yükseltemezsiniz.
>
> Bir expressroute bağlantı hattı kesintiye uğratmadan bant indiremezsiniz. Bant genişliği eski sürüme düşürmeyi expressroute bağlantı hattı yetkisini kaldırma ve yeni bir expressroute bağlantı hattı yeniden hazırlayana gerektirir.
> 

Gereksinim boyutu karar verdikten sonra bağlantı hattınız yeniden boyutlandırmak için aşağıdaki komutu kullanın:

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

$ckt.ServiceProviderProperties.BandwidthInMbps = 1000

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```


Bağlantı hattınız Microsoft tarafında boyutlandırılıp. Ardından bu değişikliği eşleşecek şekilde kendi tarafında yapılandırmalar güncelleştirileceğini bağlantı sağlayıcınız başvurmanız gerekir. Bu bildirim yaptıktan sonra biz güncelleştirilmiş bant seçeneği için faturalama başlar.

### <a name="to-move-the-sku-from-metered-to-unlimited"></a>SKU taşımak için sınırsız olarak ölçülen
Aşağıdaki PowerShell parçacığını kullanarak ExpressRoute devresi SKU'su değiştirebilirsiniz:

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

$ckt.Sku.Family = "UnlimitedData"
$ckt.sku.Name = "Premium_UnlimitedData"

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="to-control-access-to-the-classic-and-resource-manager-environments"></a>Klasik ve Resource Manager ortamları erişimi denetlemek için
' Ndaki yönergeleri gözden [Resource Manager dağıtım modeline taşıma ExpressRoute bağlantı hatları Klasik](expressroute-howto-move-arm.md).  

## <a name="delete"></a>Sağlamayı kaldırmayı ve bir expressroute bağlantı hattı silme
Aşağıdaki bilgileri unutmayın:

* Expressroute bağlantı hattı tüm sanal ağlardan bağlantısını gerekir. Bu işlem başarısız olursa, bağlantı hattı için herhangi bir sanal ağa bağlı olan denetleyin.
* Sağlama durumu ExpressRoute bağlantı hattı hizmet sağlayıcı ise **sağlama** veya **hazırlandı** kendi tarafında hattı yetkisini kaldırma için hizmet sağlayıcınıza birlikte çalışmalısınız. Kaynakları ayırabilir ve hizmet sağlayıcısı devre sağlama kaldırma işlemi tamamlandıktan ve bize bildiren kadar sizi faturalandırmak devam ediyoruz.
* Hizmet sağlayıcısı hattı sağlaması kaldırılıyor. sağlaması değilse (sağlama durumu hizmet sağlayıcısı kümesine **sağlanmadı**), bağlantı hattı silebilirsiniz. Bağlantı hattı için fatura durdurur.

Aşağıdaki komutu çalıştırarak, expressroute bağlantı hattı silebilirsiniz:

```powershell
Remove-AzureRmExpressRouteCircuit -ResourceGroupName "ExpressRouteResourceGroup" -Name "ExpressRouteARMCircuit"
```

## <a name="next-steps"></a>Sonraki adımlar

Bağlantı hattınız oluşturduktan sonra sonraki aşağıdakileri yaptığınızdan emin olun:

* [Oluşturma ve expressroute bağlantı hattı için yönlendirmeyi değiştirme](expressroute-howto-routing-arm.md)
* [Sanal ağ, ExpressRoute devresine bağlama](expressroute-howto-linkvnet-arm.md)