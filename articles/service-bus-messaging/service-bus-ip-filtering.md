---
title: Azure hizmet veri yolu güvenlik duvarı kuralları | Microsoft Docs
description: Azure Service Bus belirli IP adreslerinden bağlantılara izin vermek için güvenlik duvarı kuralları kullanma
services: service-bus
documentationcenter: ''
author: axisc
manager: timlt
editor: spelluru
ms.service: service-bus
ms.devlang: na
ms.topic: article
ms.date: 04/23/2019
ms.author: aschhab
ms.openlocfilehash: 540435e3e018ae77477030ae8b9f727d71782121
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64704580"
---
# <a name="use-firewall-rules"></a>Güvenlik duvarı kurallarını kullanın

Azure Service Bus yalnızca bilinen belirli sitelere erişilebilir olduğu senaryolar için güvenlik duvarı kuralları belirli bir IPv4 adreslerinden gelen trafiği kabul etmek için kurallar yapılandırmanıza olanak sağlar. Örneğin, bu adresler kurumsal bir NAT ağ geçidinin bu olabilir.

## <a name="when-to-use"></a>Kullanılması gereken durumlar

Service Bus kurulumu için yalnızca belirtilen IP adreslerinden trafiğini almasına ve diğer her şeyi Reddet gerekir ve ardından yararlanabileceğiniz gibi istiyorsanız bir *Güvenlik Duvarı* Service Bus uç noktaları diğer IP adreslerinden engellemek için. Örneğin, Service Bus ile kullandığınız [Azure Express Route] [ express-route] şirket içi altyapınız ile özel bağlantılar oluşturmak için. 

## <a name="how-filter-rules-are-applied"></a>Filtre kurallarının uygulanma yöntemi

IP Filtresi kurallarının Service Bus ad alanı düzeyinde uygulanır. Bu nedenle, kurallar, istemcilerden herhangi bir desteklenen protokolünü kullanarak tüm bağlantıları için geçerlidir.

Herhangi bir Service Bus ad alanı olarak reddedildi izin verilen IP kuralı eşleşmeyen bir IP adresinden bağlantı denemesi yetkisiz. Yanıt IP kuralı başvurmayacak.

## <a name="default-setting"></a>Varsayılan ayar

Varsayılan olarak, **IP Filtresi** portalında hizmet veri yolu için kılavuz boştur. Bu varsayılan ayar, ad alanınız herhangi bir IP adresi bağlantılarını kabul anlamına gelir. Bu varsayılan ayarı 0.0.0.0/0 IP adresi aralığı kabul eden bir kural eşdeğerdir.

## <a name="ip-filter-rule-evaluation"></a>IP filtresi kuralı değerlendirme

IP Filtresi kurallarının sırayla uygulanır ve IP adresi ile eşleşen ilk kural kabul etme veya reddetme eylemi belirler.

>[!WARNING]
> Güvenlik duvarı kurallarını uygulama diğer Azure Hizmetleri, Service Bus ile etkileşim engelleyebilirsiniz.
>
> Güvenilen Microsoft Hizmetleri desteklenmez zaman IP Filtreleme (güvenlik duvarı kuralları) uygulanır ve yakında kullanıma sunulacaktır.
>
> IP Filtresi ile çalışmayan genel Azure senaryoları (liste Not **değil** kapsamlı)-
> - Azure İzleyici
> - Azure Stream Analytics
> - Azure Event Grid ile tümleştirme
> - Azure IOT hub'ı yönlendirir
> - Azure IOT Device Explorer
> - Azure Veri Gezgini
>
> Microsoft Hizmetleri bir sanal ağda olması gerekir
> - Azure uygulama hizmeti
> - Azure İşlevleri

### <a name="creating-a-virtual-network-and-firewall-rule-with-azure-resource-manager-templates"></a>Azure Resource Manager şablonları ile bir sanal ağ ve güvenlik duvarı kuralı oluşturma

> [!IMPORTANT]
> Güvenlik duvarları ve sanal ağlar desteklenir yalnızca **premium** katman hizmet veri yolu.

Aşağıdaki Resource Manager şablonu var olan bir Service Bus ad alanı için bir sanal ağ kuralı ekleyerek sağlar.

Şablon parametreleri:

- **ipMask** tek bir IPv4 adresi veya IP adresleri CIDR gösteriminde bir bloğu. Örneğin, CIDR gösterimi 70.37.104.0/24 256 IPv4 adresi 70.37.104.0 70.37.104.255, aralığı için önemli bir önek bit sayısını gösteren 24 ile temsil eder.

> [!NOTE]
> Olası hiçbir Reddet kural varken, Azure Resource Manager şablonu ayarlanmış varsayılan eylem sahip **"İzin ver"** hangi bağlantıları kısıtlama yoktur.
> Sanal ağ veya güvenlik duvarı kuralları yaparken, ki değiştirmeli ***"Defaultactıon"***
> 
> from
> ```json
> "defaultAction": "Allow"
> ```
> -
> ```json
> "defaultAction": "Deny"
> ```
>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "servicebusNamespaceName": {
        "type": "string",
        "metadata": {
          "description": "Name of the Service Bus namespace"
        }
      },
      "location": {
        "type": "string",
        "metadata": {
          "description": "Location for Namespace"
        }
      }
    },
    "variables": {
      "namespaceNetworkRuleSetName": "[concat(parameters('servicebusNamespaceName'), concat('/', 'default'))]",
    },
    "resources": [
      {
        "apiVersion": "2018-01-01-preview",
        "name": "[parameters('servicebusNamespaceName')]",
        "type": "Microsoft.ServiceBus/namespaces",
        "location": "[parameters('location')]",
        "sku": {
          "name": "Premium",
          "tier": "Premium"
        },
        "properties": { }
      },
      {
        "apiVersion": "2018-01-01-preview",
        "name": "[variables('namespaceNetworkRuleSetName')]",
        "type": "Microsoft.ServiceBus/namespaces/networkruleset",
        "dependsOn": [
          "[concat('Microsoft.ServiceBus/namespaces/', parameters('servicebusNamespaceName'))]"
        ],
        "properties": {
          "virtualNetworkRules": [<YOUR EXISTING VIRTUAL NETWORK RULES>],
          "ipRules": 
          [
            {
                "ipMask":"10.1.1.1",
                "action":"Allow"
            },
            {
                "ipMask":"11.0.0.0/24",
                "action":"Allow"
            }
          ],
          "defaultAction": "Deny"
        }
      }
    ],
    "outputs": { }
  }
```

Şablonu dağıtmak için yönergeleri izleyin. [Azure Resource Manager][lnk-deploy].

## <a name="next-steps"></a>Sonraki adımlar

Azure sanal ağları, Service Bus kısıtlayan erişmek için şu bağlantıya bakınız:

- [Sanal ağ hizmet uç noktaları için hizmet veri yolu][lnk-vnet]

<!-- Links -->

[lnk-deploy]: ../azure-resource-manager/resource-group-template-deploy.md
[lnk-vnet]: service-bus-service-endpoints.md
[express-route]:  /azure/expressroute/expressroute-faqs#supported-services
