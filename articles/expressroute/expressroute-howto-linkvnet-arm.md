---
title: 'Bir sanal ağ için bir expressroute bağlantı: PowerShell: Azure | Microsoft Docs'
description: Bu belge, Resource Manager dağıtım modeli ve PowerShell kullanarak, ExpressRoute bağlantı hatları için sanal ağlar (Vnet'ler) bağlamak nasıl bir genel bakış sağlar.
services: expressroute
documentationcenter: na
author: ganesr
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: daacb6e5-705a-456f-9a03-c4fc3f8c1f7e
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/08/2018
ms.author: ganesr
ms.openlocfilehash: 354f7c455e1a2846bbdd63fa12b1cc01e2a1b9c5
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
ms.locfileid: "29877560"
---
# <a name="connect-a-virtual-network-to-an-expressroute-circuit"></a>Bir expressroute bağlantı hattı için bir sanal ağa bağlanma
> [!div class="op_single_selector"]
> * [Azure Portal](expressroute-howto-linkvnet-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-linkvnet-arm.md)
> * [Azure CLI](howto-linkvnet-cli.md)
> * [Video - Azure portalı](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit)
> * [PowerShell (klasik)](expressroute-howto-linkvnet-classic.md)
>

Bu makalede sanal ağlar (Vnet'ler) bağlamak için Azure ExpressRoute bağlantı hatlarının Resource Manager dağıtım modeli ve PowerShell kullanarak yardımcı olur. Sanal ağlar ya da aynı abonelik ya da başka bir abonelik parçası olabilir. Bu makalede ayrıca, sanal ağ bağlantısını güncelleştirme gösterilmektedir. 

## <a name="before-you-begin"></a>Başlamadan önce
* Azure PowerShell modüllerinin en son sürümünü yükleyin. Daha fazla bilgi için bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/overview).

* Gözden geçirme [Önkoşullar](expressroute-prerequisites.md), [yönlendirme gereksinimleri](expressroute-routing.md), ve [iş akışları](expressroute-workflows.md) yapılandırmaya başlamadan önce.

* Etkin bir ExpressRoute bağlantı hattınızın olması gerekir. 
  * Yönergeleri izleyerek [bir expressroute bağlantı hattı oluşturma](expressroute-howto-circuit-arm.md) ve bağlantı sağlayıcınız tarafından etkinleştirilmiş hattı sahip. 
  * Bağlantı hattınız için yapılandırılmış Azure özel eşleme olduğundan emin olun. Bkz: [yönlendirmeyi yapılandırma](expressroute-howto-routing-arm.md) yönlendirme yönergeleri için makalenin. 
  * Azure özel eşleme yapılandırılır ve uçtan uca bağlantı etkinleştirebilmeniz için ağınız ve Microsoft arasında BGP eşliği yukarı olduğundan emin olun.
  * Bir sanal ağ ve oluşturulan ve tam olarak sağlanan bir sanal ağ geçidi olduğundan emin olun. Yönergeleri izleyerek [ExpressRoute için bir sanal ağ geçidi oluşturmak](expressroute-howto-add-gateway-resource-manager.md). ExpressRoute için bir sanal ağ geçidi GatewayType 'ExpressRoute' değil VPN kullanır.

* Standart bir expressroute bağlantı hattı için en fazla 10 sanal ağlara bağlantı oluşturabilirsiniz. Tüm sanal ağları, standart bir expressroute bağlantı hattını kullanırken aynı coğrafi bölgede olması gerekir. 

* En fazla dört ExpressRoute bağlantı hatları için tek bir sanal ağa bağlanabilir. Bağlanmakta olduğunuz her expressroute bağlantı hattı için yeni bir bağlantı nesnesi oluşturmak için aşağıdaki işlemi kullanın. Expressroute bağlantı hatları aynı abonelik, farklı Aboneliklerde veya her ikisinin bir karışımı olabilir.

* Expressroute bağlantı hattı coğrafi bölge dışında sanal ağlara bağlantı veya ExpressRoute premium eklentisi etkinse, expressroute bağlantı hattı için çok sayıda sanal ağlar bağlanabilirsiniz. Denetleme [SSS](expressroute-faqs.md) premium eklentisi hakkında daha fazla ayrıntı için.


## <a name="connect-a-virtual-network-in-the-same-subscription-to-a-circuit"></a>Bir sanal ağ ile aynı abonelikte bir devreye bağlanmak
Bir expressroute bağlantı hattı için bir sanal ağ geçidi aşağıdaki cmdlet'i kullanarak bağlanabilir. Sanal ağ geçidi oluşturulur ve cmdlet'ini çalıştırmadan önce bağlama için hazır olduğundan emin olun:

```powershell
$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
$gw = Get-AzureRmVirtualNetworkGateway -Name "ExpressRouteGw" -ResourceGroupName "MyRG"
$connection = New-AzureRmVirtualNetworkGatewayConnection -Name "ERConnection" -ResourceGroupName "MyRG" -Location "East US" -VirtualNetworkGateway1 $gw -PeerId $circuit.Id -ConnectionType ExpressRoute
```

## <a name="connect-a-virtual-network-in-a-different-subscription-to-a-circuit"></a>Farklı abonelikteki bir sanal ağı devreye bağlama
Bir expressroute bağlantı hattı birden çok abonelik paylaşabilirsiniz. Aşağıdaki şekilde arasında birden fazla abonelik basit bir ExpressRoute bağlantı hatları için nasıl paylaşım works'ün şematik gösterilmektedir.

Her büyük bulut içinde daha küçük bulut, kuruluş içindeki farklı departmanlara ait abonelikleri temsil etmek için kullanılır. Her kuruluş içinde bölümlerin dağıtmak için kendi hizmetlerini--ancak kendi aboneliği kullanabilirsiniz, şirket içi ağınıza bağlanmak için tek bir expressroute bağlantı hattı paylaşabilirsiniz. Tek bir bölüm (Bu örnekte: BT) expressroute bağlantı hattına sahip olabilir. Kuruluştaki diğer abonelikler expressroute bağlantı hattı kullanabilirsiniz.

> [!NOTE]
> Expressroute bağlantı hattı için bağlantı ve bant genişliği ücretleri abonelik sahibi uygulanır. Tüm sanal ağları aynı bant genişliğini paylaşır.
> 
> 

![Çapraz abonelik bağlantısı](./media/expressroute-howto-linkvnet-classic/cross-subscription.png)


### <a name="administration---circuit-owners-and-circuit-users"></a>Yönetim - hattı sahipleri ve hattı kullanıcılar

'Devre sahibinden' ExpressRoute bağlantı hattı kaynak yetkili bir güç kullanıcısıdır. Devre sahibinden 'hattı kullanıcılar tarafından kullanılan yetkilerini oluşturabilirsiniz. Bağlantı hattı, expressroute bağlantı hattı aynı abonelik içinde olmayan sanal ağ geçitlerini sahipleri kullanıcılardır. Bağlantı hattı kullanıcıların yetkilerini (sanal ağ başına bir yetkilendirme) kullanmak.

Devre sahibinden yetkilerini herhangi bir zamanda iptal etme ve değiştirmek için power sahiptir. Bir yetkilendirme sonuçlarına tüm bağlantı erişimini iptal edildi abonelikten silinmesini iptal ediliyor.

### <a name="circuit-owner-operations"></a>Bağlantı hattı sahibi işlemleri

**Bir yetkilendirme oluşturmak için**

Devre sahibinden bir yetkilendirme oluşturur. Bu, kendi sanal ağ geçitlerini expressroute bağlantı hattına bağlanmak için bir bağlantı hattı kullanıcı tarafından kullanılan bir yetkilendirme anahtarı oluşturulmasını sonuçlanır. Bir yetkilendirme yalnızca bir bağlantı için geçerli değil.

Aşağıdaki cmdlet'i parçacığını bir yetkilendirme oluşturulacağını gösterir:

```powershell
$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
Add-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "MyAuthorization1"
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $circuit

$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
$auth1 = Get-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "MyAuthorization1"
```


Bu yanıt durumu ve yetkilendirme anahtar içerir:

    Name                   : MyAuthorization1
    Id                     : /subscriptions/&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&/resourceGroups/ERCrossSubTestRG/providers/Microsoft.Network/expressRouteCircuits/CrossSubTest/authorizations/MyAuthorization1
    Etag                   : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&& 
    AuthorizationKey       : ####################################
    AuthorizationUseStatus : Available
    ProvisioningState      : Succeeded



**Yetkilerini gözden geçirmek için**

Devre sahibinden aşağıdaki cmdlet'i çalıştırarak belirli bir bağlantı hattı üzerinde verilen tüm yetkilerini gözden geçirebilirsiniz:

```powershell
$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
$authorizations = Get-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit
```

**Yetkilerini eklemek için**

Devre sahibinden, aşağıdaki cmdlet'i kullanarak yetkilerini ekleyebilirsiniz:

```powershell
$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
Add-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "MyAuthorization2"
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $circuit

$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
$authorizations = Get-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit
```

**Yetkilerini silmek için**

Devre sahibinden revoke/yetkilerini kullanıcı için aşağıdaki cmdlet'i çalıştırarak silme:

```powershell
Remove-AzureRmExpressRouteCircuitAuthorization -Name "MyAuthorization2" -ExpressRouteCircuit $circuit
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $circuit
```    

### <a name="circuit-user-operations"></a>Bağlantı hattı kullanıcı işlemleri

Bağlantı hattı kullanıcı eş kimliği ve devre sahibinden yetkilendirme anahtarından gerekir. Yetkilendirme anahtarının bir GUID değeridir.

Aşağıdaki komutu eş kimliği denetlenebilir:

```powershell
Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
```

**Bir bağlantı yetkilendirme kullanmak için**

Bağlantı hattı kullanıcı bağlantısı yetkilendirme kullanmak için aşağıdaki cmdlet'i çalıştırabilirsiniz:

```powershell
$id = "/subscriptions/********************************/resourceGroups/ERCrossSubTestRG/providers/Microsoft.Network/expressRouteCircuits/MyCircuit"    
$gw = Get-AzureRmVirtualNetworkGateway -Name "ExpressRouteGw" -ResourceGroupName "MyRG"
$connection = New-AzureRmVirtualNetworkGatewayConnection -Name "ERConnection" -ResourceGroupName "RemoteResourceGroup" -Location "East US" -VirtualNetworkGateway1 $gw -PeerId $id -ConnectionType ExpressRoute -AuthorizationKey "^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^"
```

**Bir bağlantı yetkilendirme serbest bırakmak için**

Bir yetkilendirme bağlanan sanal ağ expressroute bağlantı hattı bağlantı silerek serbest bırakabilirsiniz.

## <a name="modify-a-virtual-network-connection"></a>Sanal ağ bağlantısı değiştirme
Belirli bir sanal ağ bağlantısı özellikleri güncelleştirebilirsiniz. 

**Bağlantı kalınlığı güncelleştirmek için**

Sanal ağınız için birden çok ExpressRoute bağlantı hatları bağlanabilir. Aynı öneke birden fazla expressroute bağlantı hattı alabilirsiniz. Bu öneki için giden trafiği göndermek için hangi bağlantı seçmek için değiştirebileceğiniz *RoutingWeight* bağlantısı. Trafik, en yüksek bağlantı gönderilecek *RoutingWeight*.

```powershell
$connection = Get-AzureRmVirtualNetworkGatewayConnection -Name "MyVirtualNetworkConnection" -ResourceGroupName "MyRG"
$connection.RoutingWeight = 100
Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection
```

Aralığı *RoutingWeight* 0 ile 32000 arasıdır. Varsayılan değer 0'dır.

## <a name="next-steps"></a>Sonraki adımlar
ExpressRoute hakkında daha fazla bilgi için, bkz. [ExpressRoute SSS](expressroute-faqs.md).
