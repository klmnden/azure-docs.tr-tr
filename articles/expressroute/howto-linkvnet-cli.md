---
title: "Bir sanal ağ için bir expressroute bağlantı: CLI: Azure | Microsoft Docs"
description: "Bu belge, CLI ve Resource Manager dağıtım modeli kullanarak, ExpressRoute bağlantı hatları için sanal ağlar (Vnet'ler) bağlamak nasıl bir genel bakış sağlar."
services: expressroute
documentationcenter: na
author: cherylmc
manager: timlit
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/25/2017
ms.author: anzaman,cherylmc
<<<<<<< HEAD
ms.openlocfilehash: 0ea696e796ec3a943bc028f56da417978b728b82
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
=======
ms.openlocfilehash: fa55cbad9fca799faff4e4cef87f9eedb8d2023f
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2017
---
# <a name="connect-a-virtual-network-to-an-expressroute-circuit-using-cli"></a>CLI kullanarak bir expressroute bağlantı hattı için bir sanal ağa bağlanma

Bu makale, CLI kullanarak Azure ExpressRoute bağlantı hatları için sanal ağlar (Vnet'ler) bağlantı yardımcı olur. Azure CLI kullanarak bağlamak için sanal ağlar Resource Manager dağıtım modeli kullanılarak oluşturulmuş olması gerekir. Bunlar aynı abonelik veya başka bir abonelik parçası olabilir. Bir expressroute bağlantı hattı ağınıza bağlanmak için farklı bir yöntem kullanmak istiyorsanız, aşağıdaki listeden bir makale seçebilirsiniz:

> [!div class="op_single_selector"]
> * [Azure Portal](expressroute-howto-linkvnet-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-linkvnet-arm.md)
> * [Azure CLI](howto-linkvnet-cli.md)
> * [Video - Azure portalı](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit)
> * [PowerShell (klasik)](expressroute-howto-linkvnet-classic.md)
> 

## <a name="configuration-prerequisites"></a>Yapılandırma önkoşulları

* Komut satırı arabirimi (CLI) en son sürümünü gerekir. Daha fazla bilgi için bkz: [Azure CLI 2.0 yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli).
* Gözden geçirmeniz gereken [Önkoşullar](expressroute-prerequisites.md), [yönlendirme gereksinimleri](expressroute-routing.md), ve [iş akışları](expressroute-workflows.md) yapılandırmaya başlamadan önce.
* Etkin bir ExpressRoute bağlantı hattınızın olması gerekir. 
  * Yönergeleri izleyerek [bir expressroute bağlantı hattı oluşturma](howto-circuit-cli.md) ve bağlantı sağlayıcınız tarafından etkinleştirilmiş hattı sahip. 
  * Bağlantı hattınız için yapılandırılmış Azure özel eşleme olduğundan emin olun. Bkz: [yönlendirmeyi yapılandırma](howto-routing-cli.md) yönlendirme yönergeleri için makalenin. 
  * Azure özel eşleme yapılandırıldığından emin olun. Uçtan uca bağlantı etkinleştirebilmeniz için ağınız ve Microsoft arasında eşleme BGP yukarı olması gerekir.
  * Bir sanal ağ ve oluşturulan ve tam olarak sağlanan bir sanal ağ geçidi olduğundan emin olun. Yönergeleri izleyerek [ExpressRoute için bir sanal ağ geçidi yapılandırma](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli). Kullandığınızdan emin olun `--gateway-type ExpressRoute`.

* Standart bir expressroute bağlantı hattı için en fazla 10 sanal ağlara bağlantı oluşturabilirsiniz. Tüm sanal ağları, standart bir expressroute bağlantı hattını kullanırken aynı coğrafi bölgede olması gerekir. 

* ExpressRoute premium eklentisi etkinleştirirseniz, bir sanal ağ expressroute bağlantı hattı coğrafi bölge dışında bağlantı ya da çok sayıda sanal ağlar, ExpressRoute devreme bağlayabilir. Premium eklentisi hakkında daha fazla bilgi için bkz: [SSS](expressroute-faqs.md).

## <a name="connect-a-virtual-network-in-the-same-subscription-to-a-circuit"></a>Bir sanal ağ ile aynı abonelikte bir devreye bağlanmak

Bir expressroute bağlantı hattı için bir sanal ağ geçidi örneğini kullanarak bağlanabilir. Sanal ağ geçidi oluşturulur ve komutu çalıştırmadan önce bağlama için hazır olduğundan emin olun.

```azurecli
az network vpn-connection create --name ERConnection --resource-group ExpressRouteResourceGroup --vnet-gateway1 VNet1GW --express-route-circuit2 MyCircuit
```

## <a name="connect-a-virtual-network-in-a-different-subscription-to-a-circuit"></a>Farklı abonelikteki bir sanal ağı devreye bağlama

Bir expressroute bağlantı hattı birden çok abonelik paylaşabilirsiniz. Aşağıdaki şekilde arasında birden çok abonelik basit bir ExpressRoute bağlantı hatları için nasıl paylaşım works'ün şematik gösterilmektedir.

Her büyük bulut içinde daha küçük bulut, kuruluş içindeki farklı departmanlara ait abonelikleri temsil etmek için kullanılır. Her kuruluş içinde bölümlerin dağıtmak için kendi hizmetlerini--ancak kendi aboneliği kullanabilirsiniz, şirket içi ağınıza bağlanmak için tek bir expressroute bağlantı hattı paylaşabilirsiniz. Tek bir bölüm (Bu örnekte: BT) expressroute bağlantı hattına sahip olabilir. Kuruluştaki diğer abonelikler expressroute bağlantı hattı kullanabilirsiniz.

> [!NOTE]
> Bağlantı ve bant genişliği ücretleri ayrılmış bağlantı hattı için ExpressRoute bağlantı hattı sahibine uygulanır. Tüm sanal ağları aynı bant genişliğini paylaşır.
> 
> 

![Çapraz abonelik bağlantısı](./media/expressroute-howto-linkvnet-classic/cross-subscription.png)

### <a name="administration---circuit-owners-and-circuit-users"></a>Yönetim - hattı sahipleri ve hattı kullanıcılar

'Devre sahibinden' ExpressRoute bağlantı hattı kaynak yetkili bir güç kullanıcısıdır. Devre sahibinden 'hattı kullanıcılar tarafından kullanılan yetkilerini oluşturabilirsiniz. Bağlantı hattı, expressroute bağlantı hattı aynı abonelik içinde olmayan sanal ağ geçitlerini sahipleri kullanıcılardır. Bağlantı hattı kullanıcıların yetkilerini (sanal ağ başına bir yetkilendirme) kullanmak.

Devre sahibinden yetkilerini herhangi bir zamanda iptal etme ve değiştirmek için power sahiptir. Bir yetkilendirme iptal edildiğinde, tüm bağlantı erişimini iptal edildi abonelikten silinir.

### <a name="circuit-owner-operations"></a>Sahip işlem hattı

**Bir yetkilendirme oluşturmak için**

Devre sahibinden devre kullanıcı tarafından kendi sanal ağ geçitlerini expressroute bağlantı hattına bağlanmak için kullanılan bir yetkilendirme anahtarı oluşturur bir yetkilendirme oluşturur. Bir yetkilendirme yalnızca bir bağlantı için geçerli değil.

Aşağıdaki örnekte bir yetkilendirme oluşturulacağını gösterir:

```azurecli
az network express-route auth create --circuit-name MyCircuit -g ExpressRouteResourceGroup -n MyAuthorization
```

Yanıt, durum ve yetkilendirme anahtar içerir:

```azurecli
"authorizationKey": "0a7f3020-541f-4b4b-844a-5fb43472e3d7",
"authorizationUseStatus": "Available",
"etag": "W/\"010353d4-8955-4984-807a-585c21a22ae0\"",
"id": "/subscriptions/81ab786c-56eb-4a4d-bb5f-f60329772466/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/MyCircuit/authorizations/MyAuthorization1",
"name": "MyAuthorization1",
"provisioningState": "Succeeded",
"resourceGroup": "ExpressRouteResourceGroup"
```

**Yetkilerini gözden geçirmek için**

Devre sahibinden aşağıdaki örnekte çalıştırarak belirli bir bağlantı hattı üzerinde verilen tüm yetkilerini gözden geçirebilirsiniz:

```azurecli
az network express-route auth list --circuit-name MyCircuit -g ExpressRouteResourceGroup
```

**Yetkilerini eklemek için**

Devre sahibinden yetkilerini aşağıdaki örneği kullanarak ekleyebilirsiniz:

```azurecli
az network express-route auth create --circuit-name MyCircuit -g ExpressRouteResourceGroup -n MyAuthorization1
```

**Yetkilerini silmek için**

Devre sahibinden revoke/yetkilerini kullanıcıya aşağıdaki örnekte çalıştırarak silme:

```azurecli
az network express-route auth delete --circuit-name MyCircuit -g ExpressRouteResourceGroup -n MyAuthorization1
```

### <a name="circuit-user-operations"></a>Bağlantı hattı kullanıcı işlemleri

Bağlantı hattı kullanıcı eş kimliği ve devre sahibinden yetkilendirme anahtarından gerekir. Yetkilendirme anahtarının bir GUID değeridir.

```azurecli
Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
```

**Bir bağlantı yetkilendirme kullanmak için**

Bağlantı hattı kullanıcı bağlantısı yetkilendirme kullanmak için aşağıdaki örnekte çalıştırabilirsiniz:

```azurecli
az network vpn-connection create --name ERConnection --resource-group ExpressRouteResourceGroup --vnet-gateway1 VNet1GW --express-route-circuit2 MyCircuit --authorization-key "^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^"
```

**Bir bağlantı yetkilendirme serbest bırakmak için**

Bir yetkilendirme bağlanan sanal ağ expressroute bağlantı hattı bağlantı silerek serbest bırakabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

ExpressRoute hakkında daha fazla bilgi için, bkz. [ExpressRoute SSS](expressroute-faqs.md).