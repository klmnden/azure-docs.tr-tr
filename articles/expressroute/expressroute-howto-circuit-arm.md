---
title: 'Oluşturma ve bir ExpressRoute bağlantı hattı - PowerShell değiştirin: Azure | Microsoft Docs'
description: Oluşturma, sağlama, doğrulayın, güncelleştirme, silme ve bir ExpressRoute bağlantı hattının sağlamasını Kaldır.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: article
ms.date: 02/20/2019
ms.author: ganesr;cherylmc
ms.custom: seodec18
ms.openlocfilehash: 06f49dc00b83ee2190f6361ebb8e6f052384402a
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67657313"
---
# <a name="create-and-modify-an-expressroute-circuit-using-powershell"></a>Oluşturma ve PowerShell kullanarak ExpressRoute devresi değiştirme
> [!div class="op_single_selector"]
> * [Azure portal](expressroute-howto-circuit-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-circuit-arm.md)
> * [Azure CLI](howto-circuit-cli.md)
> * [Azure Resource Manager şablonu](expressroute-howto-circuit-resource-manager-template.md)
> * [Video - Azure portalı](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [PowerShell (klasik)](expressroute-howto-circuit-classic.md)
>

Bu makalede, PowerShell cmdlet'leri ve Azure Resource Manager dağıtım modeli kullanarak ExpressRoute devresi oluşturmanıza yardımcı olur. Ayrıca durumu denetleme, güncelleştirme silin veya bir bağlantı hattının sağlamasını Kaldır.

## <a name="before-you-begin"></a>Başlamadan önce

Başlamadan önce gözden [önkoşulları](expressroute-prerequisites.md) ve [iş akışları](expressroute-workflows.md) yapılandırmaya başlamadan önce.

### <a name="working-with-azure-powershell"></a>Azure PowerShell ile çalışma

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [expressroute-cloudshell](../../includes/expressroute-cloudshell-powershell-about.md)]

## <a name="create"></a>Oluşturma ve bir ExpressRoute bağlantı hattı sağlama
### <a name="1-sign-in-to-your-azure-account-and-select-your-subscription"></a>1. Azure hesabınızda oturum açın ve aboneliğinizi seçin

[!INCLUDE [sign in](../../includes/expressroute-cloud-shell-connect.md)]

### <a name="2-get-the-list-of-supported-providers-locations-and-bandwidths"></a>2. Desteklenen sağlayıcılar, konumları ve bant genişlikleri listesini alın
Bir ExpressRoute bağlantı hattı oluşturmadan önce desteklenen bağlantı sağlayıcıları ve konumları bant genişliği seçenekleri listesi gerekir.

PowerShell cmdlet **Get-AzExpressRouteServiceProvider** , sonraki adımlarda kullanacağınız bu bilgileri döndürür:

```azurepowershell-interactive
Get-AzExpressRouteServiceProvider
```

Bağlantı sağlayıcınız listelenip listelenmediğini denetleyin. Bir devreyi oluşturduğunuzda, daha sonra ihtiyacınız aşağıdaki bilgileri not edin:

* Ad
* PeeringLocations
* BandwidthsOffered

Bir ExpressRoute bağlantı hattı oluşturmak artık hazırsınız.

### <a name="3-create-an-expressroute-circuit"></a>3. ExpressRoute bağlantı hattı oluşturma
Bir kaynak grubu zaten sahip değilseniz, ExpressRoute devreniz oluşturmadan önce bir oluşturmanız gerekir. Aşağıdaki komutu çalıştırarak bunu yapabilirsiniz:

```azurepowershell-interactive
New-AzResourceGroup -Name "ExpressRouteResourceGroup" -Location "West US"
```

Aşağıdaki örnek, 200 MB/sn, Silikon vadisi ExpressRoute bağlantı hattı üzerinden Equinix oluşturma işlemi gösterilmektedir. Farklı bir sağlayıcı ve farklı ayarlar kullanıyorsanız, bu bilgileri, isteğinde bulunduğunda değiştirin. Yeni bir hizmet anahtarı istemek için aşağıdaki örneği kullanın:

```azurepowershell-interactive
New-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup" -Location "West US" -SkuTier Standard -SkuFamily MeteredData -ServiceProviderName "Equinix" -PeeringLocation "Silicon Valley" -BandwidthInMbps 200
```

SKU ailesi ve SKU katmanı doğru belirttiğinizden emin olun:

* SKU katmanı, bir ExpressRoute standart ya da ExpressRoute premium eklenti etkin olup olmadığını belirler. Belirtebileceğiniz *standart* standart SKU'nun almak veya *Premium* premium eklenti için.
* SKU ailesi, fatura türü belirler. Belirtebileceğiniz *Metereddata* ölçülen veri planı için ve *Unlimiteddata* sınırsız veri planı için. Fatura türünden değiştirebilirsiniz *Metereddata* için *Unlimiteddata*, ancak türünden değiştiremezsiniz *Unlimiteddata* için *Metereddata*.

> [!IMPORTANT]
> ExpressRoute bağlantı hattı, bir hizmet anahtarı verildiğinde andan itibaren faturalandırılır. Bağlantı sağlayıcısı devreyi sağlamak hazır olduğunda bu işlem bir şekilde gerçekleştirdiğinizden emin olun.
>
>

Yanıt hizmet anahtarı içerir. Aşağıdaki komutu çalıştırarak tüm parametrelerin ayrıntılı açıklamaları alabilirsiniz:

```azurepowershell-interactive
get-help New-AzExpressRouteCircuit -detailed
```


### <a name="4-list-all-expressroute-circuits"></a>4. Tüm ExpressRoute devreleri listesi
Oluşturduğunuz tüm ExpressRoute devreleri listesini almak için çalıştırın **Get-AzExpressRouteCircuit** komutu:

```azurepowershell-interactive
Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
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

Dilediğiniz zaman bu bilgileri kullanarak alabilirsiniz `Get-AzExpressRouteCircuit` cmdlet'i. Parametresiz çağrıyı yapan tüm devreler listeler. Hizmet anahtarınız listelenen *Servicekey'ini* alan:

```azurepowershell-interactive
Get-AzExpressRouteCircuit
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


### <a name="5-send-the-service-key-to-your-connectivity-provider-for-provisioning"></a>5. Hizmet anahtarı sağlamak için bağlantı sağlayıcınıza gönderin.
*ServiceProviderProvisioningState* hizmet sağlayıcı tarafında sağlama geçerli durumu hakkında bilgi sağlar. Durum Microsoft tarafında durumu sağlar. Bağlantı hattı sağlama durumları hakkında daha fazla bilgi için bkz. [iş akışları](expressroute-workflows.md#expressroute-circuit-provisioning-states).

Yeni bir ExpressRoute bağlantı hattı'ı oluşturduğunuzda, bağlantı hattı şu durumda olur:

    ServiceProviderProvisioningState : NotProvisioned
    CircuitProvisioningState         : Enabled



Bağlantı sağlayıcısı, etkinleştirmeden sürecinde olduğunda bağlantı hattının aşağıdaki duruma değiştirir:

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled

Bir ExpressRoute bağlantı hattı kullanabilmek için şu durumda olmalıdır:

    ServiceProviderProvisioningState : Provisioned
    CircuitProvisioningState         : Enabled

### <a name="6-periodically-check-the-status-and-the-state-of-the-circuit-key"></a>6. Durum ve bağlantı hattı tuşunun durumunu düzenli aralıklarla denetleyin
Durum ve bağlantı hattı tuşunun durumunu denetleme, sağlayıcınız bağlantı hattınızın etkin olduğunda bilmenizi sağlar. Bağlantı hattı yapılandırıldıktan sonra *ServiceProviderProvisioningState* olarak görünür *sağlanan*, aşağıdaki örnekte gösterildiği gibi:

```azurepowershell-interactive
Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
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

### <a name="7-create-your-routing-configuration"></a>7. Kullanarak yönlendirme yapılandırması oluşturma
Adım adım yönergeler için bkz: [ExpressRoute bağlantı hattı yönlendirme yapılandırması](expressroute-howto-routing-arm.md) makale oluşturma ve değiştirme devre eşlemeleri.

> [!IMPORTANT]
> Bu yönergeler yalnızca Katman 2 bağlantı hizmetleri sunan hizmet sağlayıcıları ile oluşturulan bağlantı hatları için geçerlidir. Yönetilen sunan bir hizmet sağlayıcısı kullanıyorsanız, Katman 3 Hizmetleri (genellikle bir IP VPN, MPLS gibi), bağlantı sağlayıcınız yapılandırır ve yönlendirmeyi sizin için yönetir.
>
>

### <a name="8-link-a-virtual-network-to-an-expressroute-circuit"></a>8. ExpressRoute bağlantı hattına bir sanal ağı bağlama
Ardından, bir sanal ağ, ExpressRoute bağlantı hattına bağlayın. Kullanım [sanal ağları ExpressRoute devresine bağlama](expressroute-howto-linkvnet-arm.md) makale Resource Manager dağıtım modeliyle çalışırken.

## <a name="getting-the-status-of-an-expressroute-circuit"></a>ExpressRoute bağlantı hattının durumunu alma
Dilediğiniz zaman bu bilgileri kullanarak alabilirsiniz **Get-AzExpressRouteCircuit** cmdlet'i. Parametresiz çağrıyı yapan tüm devreler listeler.

```azurepowershell-interactive
Get-AzExpressRouteCircuit
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


Kaynak grubu adı ve bağlantı hattı adı çağrısına parametre olarak geçirerek belirli bir ExpressRoute bağlantı hattı hakkında bilgi alabilirsiniz:

```azurepowershell-interactive
Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
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

```azurepowershell-interactive
get-help get-azurededicatedcircuit -detailed
```

## <a name="modify"></a>Bir ExpressRoute bağlantı hattını değiştirme
Belirli bir ExpressRoute bağlantı hattı özelliklerini bağlantıyı etkilemeden değiştirebilirsiniz.

Kapalı kalma süresi olmadan aşağıdaki görevleri gerçekleştirebilirsiniz:

* Etkinleştirmek veya ExpressRoute bağlantı hattı için ExpressRoute premium eklenti devre dışı bırakın.
* ExpressRoute bağlantı hattı bant genişliği var. sağlanan kapasite kullanılabilir bağlantı noktası üzerinde artırın. Bağlantı hattı bant önceki sürüme indirme desteklenmiyor.
* Ölçüm planını, ölçülen verilerden sınırsız veri değiştirin. Ölçüm plan sınırsız verilerden ölçülen veri değiştirme desteklenmiyor.
* Etkinleştirebilir ve devre dışı *Klasik işlemlere izin Ver'i*.

Sınırlar ve sınırlamalar hakkında daha fazla bilgi için bkz. [ExpressRoute SSS](expressroute-faqs.md).

### <a name="to-enable-the-expressroute-premium-add-on"></a>ExpressRoute premium eklentisi etkinleştirmek için
ExpressRoute premium eklentisi aşağıdaki PowerShell kod parçacığını kullanarak, varolan bağlantı hattınız için etkinleştirebilirsiniz:

```azurepowershell-interactive
$ckt = Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

$ckt.Sku.Tier = "Premium"
$ckt.sku.Name = "Premium_MeteredData"

Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt
```

Bağlantı hattı artık etkin ExpressRoute premium eklenti özellikleri vardır. Komut başarıyla çalıştırıldı hemen sonra için premium eklenti özelliğini fatura başlamadan.

### <a name="to-disable-the-expressroute-premium-add-on"></a>ExpressRoute premium eklentisi devre dışı bırakmak için
> [!IMPORTANT]
> Bu işlem için standart devreyi izin daha büyük olan kaynaklar kullanıyorsanız, başarısız olabilir.
>
>

Aşağıdaki bilgileri not edin:

* Premium katmanından standart sürümüne düşürme önce işlem hattına bağlı sanal ağlar sayısı 10'dan küçük olduğundan emin olmanız gerekir. Aksi takdirde, güncelleştirme isteği başarısız olur ve premium fiyatları üzerinden faturalandırılırsınız.
* Diğer jeopolitik bölgede tüm sanal ağları bağlantısını kaldırmanız gerekir. Bunu yapmazsanız, güncelleştirme isteği başarısız olur ve premium fiyatları üzerinden faturalandırılırsınız.
* Özel eşdüzey hizmet sağlama için 4000'den az yollar yol tablonuz olması gerekir. Rota tablosu boyutunuz 4000 yollara kıyasla daha büyükse, BGP oturumu bırakır ve 4.000 tanıtılan ön ek sayısı ölçeklendirilinceye kadar yeniden iler hale gerekmez.

Aşağıdaki PowerShell cmdlet'ini kullanarak mevcut bir devreyi için ExpressRoute premium eklentisi devre dışı bırakabilirsiniz:

```azurepowershell-interactive
$ckt = Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

$ckt.Sku.Tier = "Standard"
$ckt.sku.Name = "Standard_MeteredData"

Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="to-update-the-expressroute-circuit-bandwidth"></a>ExpressRoute bağlantı hattı bant genişliğini güncelleştirmek için
Denetimi sağlayıcınız için desteklenen bir bant genişliği seçenekleri için [ExpressRoute SSS](expressroute-faqs.md). Mevcut bağlantı hattınızın boyutundan daha büyük herhangi bir boyutta seçebilirsiniz.

> [!IMPORTANT]
> ExpressRoute bağlantı hattı mevcut bağlantı noktası üzerinde yetersiz kapasite ise yeniden oluşturmanız gerekebilir. Yoksa hiçbir ek kapasite kullanılabilir o konumda devre yükseltemezsiniz.
>
> Kesintisiz bir ExpressRoute bağlantı hattı bant indiremezsiniz. Bant genişliği eski sürüme düşürme, ExpressRoute bağlantı hattının sağlamasını kaldırma ve ardından yeni ExpressRoute bağlantı hattı yeniden sağlamak istiyor.
>

Gereksinim boyutu karar verdikten sonra bağlantı hattınızı yeniden boyutlandırmak için aşağıdaki komutu kullanın:

```azurepowershell-interactive
$ckt = Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

$ckt.ServiceProviderProperties.BandwidthInMbps = 1000

Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt
```


Bağlantı hattınız Microsoft tarafında boyutlandırılıp. Ardından bu değişikliği eşleşecek şekilde kendi tarafında yapılandırmaları güncelleştirmek için bağlantı sağlayıcınıza başvurun gerekir. Bu bildirim yaptıktan sonra güncelleştirilmiş bant seçeneği için faturalama başlayacağız.

### <a name="to-move-the-sku-from-metered-to-unlimited"></a>SKU taşımak için sınırsız olarak ölçülür
Aşağıdaki PowerShell kod parçacığını kullanarak bir ExpressRoute bağlantı hattı SKU'su değiştirebilirsiniz:

```azurepowershell-interactive
$ckt = Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

$ckt.Sku.Family = "UnlimitedData"
$ckt.sku.Name = "Premium_UnlimitedData"

Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="to-control-access-to-the-classic-and-resource-manager-environments"></a>Klasik ve Resource Manager ortamları erişimi denetlemek için
Gözden geçirme yönergeleri [Resource Manager dağıtım modeline taşıma ExpressRoute devreleri Klasikten](expressroute-howto-move-arm.md).

## <a name="delete"></a>Sağlama kaldırmayı ve bir ExpressRoute bağlantı hattı siliniyor
Aşağıdaki bilgileri not edin:

* ExpressRoute bağlantı hattınızdaki tüm sanal ağların bağlantısını kaldırmanız gerekir. Bu işlem başarısız olursa, tüm sanal ağları işlem hattına bağlı olmadığını denetleyin.
* ExpressRoute bağlantı hattı Hizmet Sağlayıcısı sağlama durumu ise **sağlama** veya **sağlanan** kendi tarafında bağlantı hattını sağlamasını kaldırmak için hizmet sağlayıcınızla birlikte çalışmanız gerekir. Kaynak ayırmanıza ve hizmeti sağlayıcısı devreyi sağlamayı kaldırma tamamlandıktan ve bize bildiren kadar faturalandırılırsınız devam ediyoruz.
* Hizmet sağlayıcısı devreyi sağlamayı durdurduğunda varsa (Hizmet Sağlayıcısı sağlama durumu ayarlamak **sağlanmadı**), bağlantı hattının silebilirsiniz. Bu durumda bağlantı hattının faturalandırılması durdurulur.

Aşağıdaki komutu çalıştırarak, ExpressRoute devreniz silebilirsiniz:

```azurepowershell-interactive
Remove-AzExpressRouteCircuit -ResourceGroupName "ExpressRouteResourceGroup" -Name "ExpressRouteARMCircuit"
```

## <a name="next-steps"></a>Sonraki adımlar

Bağlantı hattınızı oluşturduktan sonra sonraki aşağıdakileri yaptığınızdan emin olun:

* [ExpressRoute bağlantı hattı için yönlendirme oluşturma ve değiştirme](expressroute-howto-routing-arm.md)
* [Sanal ağınız, ExpressRoute devresine bağlama](expressroute-howto-linkvnet-arm.md)
