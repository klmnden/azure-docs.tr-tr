---
title: 'ExpressRoute küresel erişim yapılandırın: Azure CLI | Microsoft Docs'
description: Bu makalede, yardımcı birlikte özel ağ arasında şirket içi ağlarınız ve Global erişim etkinleştirme yapmak için ExpressRoute bağlantı hattına bağlayın.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 12/12/2018
ms.author: cherylmc
ms.custom: seodec18
ms.openlocfilehash: 9a8e0a5df9383d8e3d7159aa916b0e4fbfeea948
ms.sourcegitcommit: 85d94b423518ee7ec7f071f4f256f84c64039a9d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2018
ms.locfileid: "53384079"
---
# <a name="configure-expressroute-global-reach-using-azure-cli-preview"></a>Azure CLI (Önizleme) kullanarak ExpressRoute küresel erişim yapılandırma
Bu makalede Azure CLI kullanarak ExpressRoute Global erişim yapılandırmanıza yardımcı olur. Daha fazla bilgi için [ExpressRouteRoute Global erişim](expressroute-global-reach.md).
 
## <a name="before-you-begin"></a>Başlamadan önce
> [!IMPORTANT]
> Bu genel önizleme bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılmamalıdır. Belirli özellikler desteklenmiyor olabilir, kısıtlı yeteneklere sahip olabilir veya tüm Azure konumlarında mevcut olmayabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
> 


Yapılandırmaya başlamadan önce aşağıdaki gereksinimleri denetlemek gerekir.

* Azure CLI'nin en son sürümünü yükleyin. Bkz: [Azure CLI yükleme](/cli/azure/install-azure-cli) ve [Azure CLI ile çalışmaya başlama](/cli/azure/get-started-with-azure-cli).
* ExpressRoute bağlantı hattı sağlama anlamak [iş akışları](expressroute-workflows.md).
* ExpressRoute devreleri sağlanan durumda olduğundan emin olun.
* Azure özel eşdüzey hizmet sağlama, ExpressRoute bağlantı hatları üzerinde yapılandırıldığından emin olun.  

### <a name="log-into-your-azure-account"></a>Azure hesabınızda oturum açın
Yapılandırmayı başlatmak için Azure hesabınızda oturum açmalısınız. Komut, varsayılan tarayıcınızı açın ve Azure hesabınızda oturum açma kimlik bilgileri iste.  

```azurecli
az login
```

Birden çok Azure aboneliğiniz varsa, hesabın aboneliklerini denetleyin.

```azurecli
az account list
```

Kullanmak istediğiniz aboneliği belirtin.

```azurecli
az account set --subscription <your subscription ID>
```

### <a name="identify-your-expressroute-circuits-for-configuration"></a>Yapılandırma için ExpressRoute devreleri tanımlayın
ExpressRoute Global erişim herhangi iki ExpressRoute bağlantı hatları arasında eşleme farklı konumlarda oluşturuldukları ve desteklenen ülkede bulunan sürece etkinleştirebilirsiniz. Her iki bağlantı hatları, aboneliğin sahibi, aşağıdaki bölümlerde yapılandırması çalıştırmak için her iki bağlantı hattı seçebilirsiniz. İki bağlantı hatlarının farklı Azure aboneliklerinde ise bir Azure aboneliğinden yetki vermeniz gerekir ve diğer Azure aboneliğinde yapılandırma komutu çalıştırdığınızda, yetkilendirme anahtar geçirin.

## <a name="enable-connectivity-between-your-on-premises-networks"></a>Şirket içi ağlarınızı arasındaki bağlantıyı etkinleştir

Bağlantıyı etkinleştirmek için komutu çalıştırırken, aşağıdaki değerleri göz önünde bulundurun:

* *Eş devre* tam kaynak kimliği olmalıdır Örneğin: 

  ```
  /subscriptions/{your_subscription_id}/resourceGroups/{your_resource_group}/providers/Microsoft.Network/expressRouteCircuits/{your_circuit_name}
  ```
* *-AddressPrefix* bir/29 IPv4 olmalıdır alt ağ, örneğin "10.0.0.0/29". IP adreslerini bu alt ağda iki ExpressRoute bağlantı hatları arasında bağlantı kurmak için kullanacağız. Adresleri bu alt ağda Azure Vnet'ler veya şirket içi ağlarınızı kullanmamanız gerekir.

İki ExpressRoute bağlantı hatları bağlanmak için aşağıdaki CLI çalıştırın. Aşağıdaki örnek komut kullanın:

```azurecli
az network express-route peering connection create -g <ResourceGroupName> --circuit-name <Circuit1Name> --peering-name AzurePrivatePeering -n <ConnectionName> --peer-circuit <Circuit2ResourceID> --address-prefix <__.__.__.__/29>
```

CLI çıktıyı aşağıdaki örnekteki gibi görünür:

```azurecli
{
  "addressPrefix": "<__.__.__.__/29>",
  "authorizationKey": null,
  "circuitConnectionStatus": "Connected",
  "etag": "W/\"48d682f9-c232-4151-a09f-fab7cb56369a\"",
  "expressRouteCircuitPeering": {
    "id": "/subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/expressRouteCircuits/<Circuit1Name>/peerings/AzurePrivatePeering",
    "resourceGroup": "<ResourceGroupName>"
  },
  "id": "/subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/expressRouteCircuits/<Circuit1Name>/peerings/AzurePrivatePeering/connections/<ConnectionName>",
  "name": "<ConnectionName>",
  "peerExpressRouteCircuitPeering": {
    "id": "/subscriptions/<SubscriptionID>/resourceGroups/<Circuit2ResourceGroupName>/providers/Microsoft.Network/expressRouteCircuits/<Circuit2Name>/peerings/AzurePrivatePeering",
    "resourceGroup": "<Circuit2ResourceGroupName>"
  },
  "provisioningState": "Succeeded",
  "resourceGroup": "<ResourceGroupName>",
  "type": "Microsoft.Network/expressRouteCircuits/peerings/connections"
}
```

Yukarıdaki işlem tamamlandığında, şirket içi ağlarınız ile iki ExpressRoute bağlantı hatları her iki tarafında arasında bağlantı olmalıdır.

### <a name="expressroute-circuits-in-different-azure-subscriptions"></a>Farklı Azure aboneliklerinde ExpressRoute işlem hatları

İki bağlantı hatlarının aynı Azure aboneliğinde emin değilseniz, yetkilendirme gerekir. Aşağıdaki yapılandırmasında, yetkilendirme, bağlantı hattı 2 abonelikte oluşturulur ve yetkilendirme anahtarını 1 işlem hattına geçirilen.

Bir yetkilendirme anahtar oluşturun. 
```azurecli
az network express-route auth create --circuit-name <Circuit2Name> -g <Circuit2ResourceGroupName> -n <AuthorizationName>
```

CLI çıktı aşağıdaki gibi görünür.

```azurecli
{
  "authorizationKey": "<authorizationKey>",
  "authorizationUseStatus": "Available",
  "etag": "W/\"cfd15a2f-43a1-4361-9403-6a0be00746ed\"",
  "id": "/subscriptions/<SubscriptionID>/resourceGroups/<Circuit2ResourceGroupName>/providers/Microsoft.Network/expressRouteCircuits/<Circuit2Name>/authorizations/<AuthorizationName>",
  "name": "<AuthorizationName>",
  "provisioningState": "Succeeded",
  "resourceGroup": "<Circuit2ResourceGroupName>",
  "type": "Microsoft.Network/expressRouteCircuits/authorizations"
}
```

Bağlantı hattı 2 kaynak kimliği hem de yetkilendirme anahtarını not edin.

1 bağlantı hattı karşı aşağıdaki komutu çalıştırın. Bağlantı hattı 2 kaynak kimliği ve yetkilendirme anahtarını geçirin 
```azurecli
az network express-route peering connection create -g <ResourceGroupName> --circuit-name <Circuit1Name> --peering-name AzurePrivatePeering -n <ConnectionName> --peer-circuit <Circuit2ResourceID> --address-prefix <__.__.__.__/29> --authorization-key <authorizationKey>
```

Yukarıdaki işlem tamamlandığında, şirket içi ağlarınız ile iki ExpressRoute bağlantı hatları her iki tarafında arasında bağlantı olmalıdır.

## <a name="get-and-verify-the-configuration"></a>Alma ve yapılandırmayı doğrulama

Burada yapılandırma yapıldı, yani devre 1 yukarıdaki örnekte devredeki yapılandırmasını doğrulamak için aşağıdaki komutu kullanın.

```azurecli
az network express-route show -n <CircuitName> -g <ResourceGroupName>
```

CLI'daki çıkış göreceksiniz *CircuitConnectionStatus*. Bu size olup iki bağlantı hatlarının arasındaki bağlantıyı, veya değil, "bağlı" kurulur bildirir "Bağlantısı kesildi". 

## <a name="disable-connectivity-between-your-on-premises-networks"></a>Şirket içi ağlarınız arasında bağlantı devre dışı bırak

Devre dışı bırakmak için burada yapılandırma yapıldı, yani devre 1 yukarıdaki örnekte karşı bağlantı hattını komutları çalıştırın.

```azurecli
az network express-route peering connection delete -g <ResourceGroupName> --circuit-name <Circuit1Name> --peering-name AzurePrivatePeering -n <ConnectionName>
```

Durumu doğrulamak için Göster CLI çalıştırabilirsiniz. 

Yukarıdaki işlem tamamlandıktan sonra ExpressRoute bağlantı hatları aracılığıyla şirket içi ağınız arasında bağlantı artık yoktur. 


## <a name="next-steps"></a>Sonraki adımlar
* [ExpressRoute Global erişim hakkında daha fazla bilgi edinin](expressroute-global-reach.md)
* [ExpressRoute bağlantısını doğrulama](expressroute-troubleshooting-expressroute-overview.md)
* [ExpressRoute bağlantı hattı için Azure sanal ağı bağlama](expressroute-howto-linkvnet-arm.md)


