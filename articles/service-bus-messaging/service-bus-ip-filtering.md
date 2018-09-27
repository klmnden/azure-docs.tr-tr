---
title: Azure Service Bus IP bağlantı filtreleri | Microsoft Docs
description: IP bloğu bağlantıları, Azure Service Bus belirli IP adreslerinden filtrelemesini kullanma
services: service-bus
documentationcenter: ''
author: clemensv
manager: timlt
ms.service: service-bus
ms.devlang: na
ms.topic: article
ms.date: 09/26/2018
ms.author: clemensv
ms.openlocfilehash: ccb759a9151d734aa99a6f6b9c68e6072874dd84
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2018
ms.locfileid: "47394837"
---
# <a name="use-ip-filters"></a>IP filtreleri kullanma

Azure Service Bus olduğu yalnızca bilinen belirli sitelerden erişilebilir senaryoları için *IP Filtresi* özelliği özel IPv4 adreslerinden gelen trafiği kabul etmesini ya da reddetme kurallarını yapılandırmanıza olanak sağlar. Örneğin, bu adresler kurumsal bir NAT ağ geçidinin bu olabilir.

## <a name="when-to-use"></a>Kullanılması gereken durumlar

Service Bus uç noktaları belirli IP adresleri için engellemek yararlı olan iki belirli kullanım örnekleri vardır:

- Service Bus, yalnızca belirtilen IP adreslerinden trafiğini almasına ve diğer her şeyi reddet. Örneğin, Service Bus ile kullandığınız [Azure Express Route] [ express-route] şirket içi altyapınız ile özel bağlantılar oluşturmak için.
- Service Bus yönetici tarafından kuşkulu olarak belirlenmiştir IP adreslerinden gelen trafiği reddetmek gerekir.

## <a name="how-filter-rules-are-applied"></a>Filtre kurallarının uygulanma yöntemi

IP Filtresi kurallarının Service Bus ad alanı düzeyinde uygulanır. Bu nedenle, kurallar, istemcilerden herhangi bir desteklenen protokolünü kullanarak tüm bağlantıları için geçerlidir.

Herhangi bir bağlantı denemesi bir IP adresinden yetkisiz eşleşmeler bir Service Bus ad alanı rejecting IP kuralı olarak reddedilir. Yanıt IP kuralı başvurmayacak.

## <a name="default-setting"></a>Varsayılan ayar

Varsayılan olarak, **IP Filtresi** portalında hizmet veri yolu için kılavuz boştur. Bu varsayılan ayar, ad alanınız herhangi bir IP adresi bağlantılarını kabul anlamına gelir. Bu varsayılan ayarı 0.0.0.0/0 IP adresi aralığı kabul eden bir kural eşdeğerdir.

## <a name="ip-filter-rule-evaluation"></a>IP filtresi kuralı değerlendirme

IP Filtresi kurallarının sırayla uygulanır ve IP adresi ile eşleşen ilk kural kabul etme veya reddetme eylemi belirler.

Örneğin, aralığı 70.37.104.0/24 adresleri kabul edin ve diğer her şeyi Reddet istiyorsanız, kılavuzdaki ilk kural adres aralığı 70.37.104.0/24 kabul etmelidir. Sonraki kural aralığı 0.0.0.0/0 kullanarak tüm adresleri reddedecek.

> [!NOTE]
> Reddetme IP adresleri, diğer Azure Hizmetleri (örneğin, Azure Stream Analytics, Azure sanal makineler veya Portalı'nda Device Explorer) Service Bus ile etkileşim engelleyebilirsiniz.

### <a name="creating-a-virtual-network-rule-with-azure-resource-manager-templates"></a>Azure Resource Manager şablonları ile bir sanal ağ kuralı oluşturuluyor

Aşağıdaki Resource Manager şablonu var olan bir Service Bus ad alanı için bir sanal ağ kuralı ekleyerek sağlar.

Şablon parametreleri:

- **ipFilterRuleName** en fazla 128 karakter benzersiz, büyük/küçük harf, alfasayısal bir dize olmalıdır.
- **ipFilterAction** ya da **Reddet** veya **kabul** için IP filtresi kuralı uygulamak için eylem olarak.
- **ipMask** tek bir IPv4 adresi veya IP adresleri CIDR gösteriminde bir bloğu. Örneğin, CIDR gösterimi 70.37.104.0/24 256 IPv4 adresi 70.37.104.0 70.37.104.255, aralığı için önemli bir önek bit sayısını gösteren 24 ile temsil eder.

```json
{  
   "$schema":"http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
   "contentVersion":"1.0.0.0",
   "parameters":{     
          "namespaceName":{  
             "type":"string",
             "metadata":{  
                "description":"Name of the namespace"
             }
          },
          "ipFilterRuleName":{  
             "type":"string",
             "metadata":{  
                "description":"Name of the Authorization rule"
             }
          },
          "ipFilterAction":{  
             "type":"string",
             "allowedValues": ["Reject", "Accept"],
             "metadata":{  
                "description":"IP Filter Action"
             }
          },
          "IpMask":{  
             "type":"string",
             "metadata":{  
                "description":"IP Mask"
             }
          }
      },
    "resources": [
        {
            "apiVersion": "2018-01-01-preview",
            "name": "[concat(parameters('namespaceName'), '/', parameters('ipFilterRuleName'))]",
            "type": "Microsoft.ServiceBus/Namespaces/IPFilterRules",
            "properties": {
                "FilterName":"[parameters('ipFilterRuleName')]",
                "Action":"[parameters('ipFilterAction')]",              
                "IpMask": "[parameters('IpMask')]"
            }
        } 
    ]
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