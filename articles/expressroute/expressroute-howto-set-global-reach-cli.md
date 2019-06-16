---
title: 'ExpressRoute küresel erişim yapılandırın: Azure CLI | Microsoft Docs'
description: Bu makalede, yardımcı birlikte özel ağ arasında şirket içi ağlarınız ve Global erişim etkinleştirme yapmak için ExpressRoute bağlantı hattına bağlayın.
services: expressroute
author: jaredr80
ms.service: expressroute
ms.topic: conceptual
ms.date: 12/12/2018
ms.author: jaredro
ms.custom: seodec18
ms.openlocfilehash: 89ada41c5f3c9cf1ca7a2ac707363f57080c361d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64869974"
---
# <a name="configure-expressroute-global-reach-by-using-the-azure-cli"></a>Azure CLI kullanarak ExpressRoute Global erişim yapılandırma

Bu makalede Azure CLI kullanarak Azure ExpressRoute Global erişim yapılandırmanıza yardımcı olur. Daha fazla bilgi için bkz. [ExpressRoute Global Reach](expressroute-global-reach.md).
 
Yapılandırmaya başlamadan önce aşağıdaki gereksinimleri tamamlayın:

* Azure CLI'ın en son sürümünü yükleyin. Bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli) ve [Azure CLI kullanmaya başlama](/cli/azure/get-started-with-azure-cli).
* ExpressRoute bağlantı hattı sağlama anlamak [iş akışları](expressroute-workflows.md).
* ExpressRoute bağlantı hatları sağlanan durumunda olduğundan emin olun.
* Azure özel eşdüzey hizmet sağlama, ExpressRoute bağlantı hatları üzerinde yapılandırıldığından emin olun.  

### <a name="sign-in-to-your-azure-account"></a>Azure hesabınızda oturum açma

Yapılandırmayı başlatmak için Azure hesabınızda oturum açın. Aşağıdaki komut, varsayılan tarayıcınızı açar ve Azure hesabınız için oturum açma kimlik bilgileri ister:  

```azurecli
az login
```

Birden çok Azure aboneliğiniz varsa, hesabın aboneliklerini denetleyin:

```azurecli
az account list
```

Kullanmak istediğiniz aboneliği belirtin:

```azurecli
az account set --subscription <your subscription ID>
```

### <a name="identify-your-expressroute-circuits-for-configuration"></a>Yapılandırma için ExpressRoute devreleri tanımlayın

Desteklenen ülke/bölgelerde bulunan ve eşleme farklı konumlarda oluşturulan sürece her iki ExpressRoute devreniz arasında ExpressRoute Global erişim etkinleştirebilirsiniz. Her iki bağlantı hatları, aboneliğin sahibi, yapılandırması, bu makalenin sonraki bölümlerinde açıklandığı gibi çalıştırmak için her iki bağlantı hattı seçebilirsiniz. İki bağlantı hatlarının farklı Azure aboneliklerinde ise yetkilendirme bir Azure aboneliğine sahip olmanız gerekir ve diğer Azure aboneliğinde yapılandırma komutu çalıştırdığınızda yetkilendirme anahtarıyla geçmesi gerekir.

## <a name="enable-connectivity-between-your-on-premises-networks"></a>Şirket içi ağlarınızı arasındaki bağlantıyı etkinleştir

Bağlantısını etkinleştirmek için komutu çalıştırırken, parametre değerleri için aşağıdaki gereksinimleri dikkate alın:

* *Eş devre* tam kaynak kimliği olmalıdır Örneğin:

  > / subscriptions/{your_subscription_id}/resourceGroups/{your_resource_group}/providers/Microsoft.Network/expressRouteCircuits/{your_circuit_name}

* *Adres ön eki* bir "/ 29" IPv4 alt ağı (örneğin, "10.0.0.0/29") olmalıdır. IP adreslerini bu alt ağda iki ExpressRoute bağlantı hatları arasında bağlantı kurmak için kullanırız. Adresleri bu alt ağda Azure sanal ağlarınıza veya şirket içi ağlarınızı kullanmamanız gerekir.

İki ExpressRoute bağlantı hatları bağlanmak için aşağıdaki CLI komutunu çalıştırın:

```azurecli
az network express-route peering connection create -g <ResourceGroupName> --circuit-name <Circuit1Name> --peering-name AzurePrivatePeering -n <ConnectionName> --peer-circuit <Circuit2ResourceID> --address-prefix <__.__.__.__/29>
```

CLI çıktıyı şöyle görünür:

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

Bu işlem tamamlandığında, şirket içi ağlarınız ile iki ExpressRoute bağlantı hatları her iki tarafında arasında bağlantı gerekir.

## <a name="enable-connectivity-between-expressroute-circuits-in-different-azure-subscriptions"></a>ExpressRoute bağlantı hatlarını farklı Azure abonelikleri arasında bağlantıyı etkinleştirmek

İki bağlantı hatlarının aynı Azure aboneliğinde mevcut değilse yetki vermeniz gerekir. Aşağıdaki yapılandırmasında, yetkilendirme devre 2 aboneliği oluşturmak ve 1 bağlantı hattı için yetkilendirme anahtarı geçirirsiniz.

1. Yetkilendirme anahtarı oluşturun:

   ```azurecli
   az network express-route auth create --circuit-name <Circuit2Name> -g <Circuit2ResourceGroupName> -n <AuthorizationName>
   ```

   CLI çıktıyı şöyle görünür:

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

1. Hem kaynak kimliği hem de 2 bağlantı hattı için yetkilendirme anahtarını not edin.

1. Bağlantı hattı 1, 2 devre kaynak kimliği ve yetkilendirme anahtarını geçirerek aşağıdaki komutu çalıştırın:

   ```azurecli
   az network express-route peering connection create -g <ResourceGroupName> --circuit-name <Circuit1Name> --peering-name AzurePrivatePeering -n <ConnectionName> --peer-circuit <Circuit2ResourceID> --address-prefix <__.__.__.__/29> --authorization-key <authorizationKey>
   ```

Bu işlem tamamlandığında, şirket içi ağlarınız ile iki ExpressRoute bağlantı hatları her iki tarafında arasında bağlantı gerekir.

## <a name="get-and-verify-the-configuration"></a>Alma ve yapılandırmayı doğrulama

Yapılandırma (yukarıdaki örnekte 1 bağlantı hattı) yapıldığı devredeki yapılandırmasını doğrulamak için aşağıdaki komutu kullanın:

```azurecli
az network express-route show -n <CircuitName> -g <ResourceGroupName>
```

CLI Çıkışta gördüğünüz *CircuitConnectionStatus*. Bu, iki bağlantı hatlarının arasındaki bağlantıyı mi olduğunu bildirir ("bağlandı") kurulamadı veya oluşturulan değil ("bağlantı kesildi"). 

## <a name="disable-connectivity-between-your-on-premises-networks"></a>Şirket içi ağlarınız arasında bağlantı devre dışı bırak

Bağlantı devre dışı bırakmak için yapılandırma (önceki örnekte bulunan 1 bağlantı hattı) yapıldığı karşı bağlantı hattının aşağıdaki komutu çalıştırın.

```azurecli
az network express-route peering connection delete -g <ResourceGroupName> --circuit-name <Circuit1Name> --peering-name AzurePrivatePeering -n <ConnectionName>
```

Kullanım ```show``` durumunu doğrulamak için komutu.

Bu işlem tamamlandıktan sonra ExpressRoute bağlantı hatları aracılığıyla, şirket içi ağlar arasında bağlantı artık gerekir.

## <a name="next-steps"></a>Sonraki adımlar

* [ExpressRoute Global erişim hakkında daha fazla bilgi edinin](expressroute-global-reach.md)
* [ExpressRoute bağlantısını doğrulama](expressroute-troubleshooting-expressroute-overview.md)
* [ExpressRoute bağlantı hattına bir sanal ağa bağlantı](expressroute-howto-linkvnet-arm.md)
