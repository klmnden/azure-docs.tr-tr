---
title: Azure Event Hubs'a güvenlik duvarı kuralları | Microsoft Docs
description: Azure Event Hubs belirli IP adreslerinden bağlantılara izin vermek için güvenlik duvarı kurallarını kullanın.
services: event-hubs
documentationcenter: ''
author: spelluru
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.custom: seodec18
ms.topic: article
ms.date: 12/06/2018
ms.author: spelluru
ms.openlocfilehash: ccb2fa7b0805b332957513c52c0c1051d068d2cc
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60821672"
---
# <a name="use-firewall-rules"></a>Güvenlik duvarı kurallarını kullanın

Hangi Azure Event hubs'ı yalnızca bilinen belirli sitelerden erişilebilir olması gereken senaryoları için güvenlik duvarı kuralları belirli IPv4 adreslerinden gelen trafiği kabul etmek için kurallar yapılandırın olanak sağlar. Örneğin, bu adresler kurumsal bir NAT ağ geçidinin bu olabilir.

## <a name="when-to-use"></a>Kullanılması gereken durumlar

Event Hubs ad alanınız Kurulum istiyorsanız alması gereken gibi trafiği yalnızca belirtilen IP adreslerinden ve diğer her şeyi Reddet sonra yararlanabileceğiniz bir *güvenlik duvarı kuralı* olay hub'ı uç noktalarından engellemek için diğer IP adresleri. Örneğin, Event Hubs ile kullanırsanız [Azure Express Route][express-route], oluşturabileceğiniz bir *güvenlik duvarı kuralı* , şirket içi altyapı IP trafiği kısıtlamak için adresleri.

## <a name="how-filter-rules-are-applied"></a>Filtre kurallarının uygulanma yöntemi

IP Filtresi kurallarının Event Hubs ad alanı düzeyinde uygulanır. Bu nedenle, kurallar, istemcilerden herhangi bir desteklenen protokolünü kullanarak tüm bağlantıları için geçerlidir.

Event Hubs ad alanı olarak reddedildi üzerinde bir izin verilen IP kuralıyla eşleşmeyen bir IP adresinden bağlantı girişimleri yetkisiz. Yanıt IP kuralı başvurmayacak.

## <a name="default-setting"></a>Varsayılan ayar

Varsayılan olarak, **IP Filtresi** portalında Event Hubs için kılavuz boştur. Bu varsayılan ayar, olay hub'ınıza herhangi bir IP adresinden gelen bağlantıları kabul etmesini anlamına gelir. Bu varsayılan ayarı 0.0.0.0/0 IP adresi aralığı kabul eden bir kural eşdeğerdir.

## <a name="ip-filter-rule-evaluation"></a>IP filtresi kuralı değerlendirme

IP Filtresi kurallarının sırayla uygulanır ve IP adresi ile eşleşen ilk kural kabul etme veya reddetme eylemi belirler.

>[!WARNING]
> Uygulama güvenlik duvarları, diğer Azure Hizmetleri Event Hubs ile etkileşim engelleyebilirsiniz.
>
> Güvenilen Microsoft Hizmetleri desteklenmez zaman IP Filtreleme (güvenlik duvarları) uygulanır ve yakında kullanıma sunulacaktır.
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
> - Azure Web Apps
> - Azure İşlevleri

### <a name="creating-a-firewall-rule-with-azure-resource-manager-templates"></a>Azure Resource Manager şablonları ile bir güvenlik duvarı kuralı oluşturma

> [!IMPORTANT]
> Güvenlik duvarı kuralları desteklenmektedir **standart** ve **adanmış** Event Hubs'ın katmanları. Temel katmanda desteklenmiyor.

Aşağıdaki Resource Manager şablonu var olan bir Event Hubs ad alanı için bir IP filtresi kuralı ekleme sağlar.

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
      "eventhubNamespaceName": {
        "type": "string",
        "metadata": {
          "description": "Name of the Event Hubs namespace"
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
      "namespaceNetworkRuleSetName": "[concat(parameters('eventhubNamespaceName'), concat('/', 'default'))]",
    },
    "resources": [
      {
        "apiVersion": "2018-01-01-preview",
        "name": "[parameters('eventhubNamespaceName')]",
        "type": "Microsoft.EventHub/namespaces",
        "location": "[parameters('location')]",
        "sku": {
          "name": "Standard",
          "tier": "Standard"
        },
        "properties": { }
      },
      {
        "apiVersion": "2018-01-01-preview",
        "name": "[variables('namespaceNetworkRuleSetName')]",
        "type": "Microsoft.EventHub/namespaces/networkruleset",
        "dependsOn": [
          "[concat('Microsoft.EventHub/namespaces/', parameters('eventhubNamespaceName'))]"
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

Azure sanal ağları, Event hubs'a kısıtlayan erişmek için şu bağlantıya bakınız:

- [Sanal ağ hizmet uç noktaları için olay hub'ları][lnk-vnet]

<!-- Links -->

[express-route]:  /azure/expressroute/expressroute-faqs#supported-services
[lnk-deploy]: ../azure-resource-manager/resource-group-template-deploy.md
[lnk-vnet]: event-hubs-service-endpoints.md
