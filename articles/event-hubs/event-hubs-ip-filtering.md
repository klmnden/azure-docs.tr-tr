---
title: Azure olay hub'ları IP bağlantı filtrelerini | Microsoft Docs
description: Belirli IP adreslerinden Azure Event Hubs bloğu bağlantıları filtreleme IP kullanın.
services: event-hubs
documentationcenter: ''
author: clemensv
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.date: 06/26/2018
ms.author: clemensv
ms.openlocfilehash: 425a5b641fbfd2e52e1294c6317b51ff2a584aa3
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37036397"
---
# <a name="use-ip-filters"></a>IP filtreleri kullanın

Azure Event Hubs olduğu yalnızca iyi bilinen belirli sitelerinden erişilebilir senaryoları için *IP Filtresi* özelliği reddetme veya belirli IPv4 adresleri olan trafiği kabul edilmesi için kuralları yapılandırmanıza olanak sağlar. Örneğin, bu adresleri bir kurumsal ağ geçidi NAT içeriğiyle olabilir.

## <a name="when-to-use"></a>Kullanılması gereken durumlar

İki önemli belirli IP adreslerini gibidir olay hub'ları uç noktaları engellemek faydalı olduğu durumlarda kullanın:

- Olay hub'larınız, yalnızca belirtilen IP adreslerinden trafiğini almasına ve şey Reddet gerekir. Örneğin, olay hub'larıyla kullanarak [Azure Express rota] [ express-route] şirket içi altyapınıza özel bağlantılar oluşturmak için. 
- Kuşkulu olarak Event Hubs yönetici tarafından belirlenen IP adreslerinden gelen trafiği reddetmeye gerekir.

## <a name="how-filter-rules-are-applied"></a>Filtre kuralları nasıl uygulanır

IP filtre kuralları olay hub'ları ad alanı düzeyinde uygulanır. Bu nedenle, kurallar herhangi bir desteklenen protokolünü kullanarak istemcilerinden gelen tüm bağlantılara uygulanır.

Olay hub'ları ad alanında rejecting IP kural olarak reddedilir eşleşmeleri yetkisiz bir IP adresi herhangi bağlantı girişimi. Yanıt IP kural Bahsediyor değil.

## <a name="default-setting"></a>Varsayılan ayar

Varsayılan olarak, **IP Filtresi** olay hub'ları için portal kılavuzunda boştur. Bu varsayılan ayarı, olay hub'ınızı tüm IP adreslerinden gelen bağlantıları kabul anlamına gelir. Bu varsayılan ayarı 0.0.0.0/0 IP adresi aralığı kabul eden bir kural eşdeğerdir.

## <a name="ip-filter-rule-evaluation"></a>IP filtre kural değerlendirmesi

IP filtre kuralları sırayla uygulanır ve IP adresi ile eşleşen ilk kural kabul etme veya reddetme eylemi belirler.

Örneğin, aralığı 70.37.104.0/24 adresleri kabul edin ve başka her şeyi Reddet istiyorsanız, ilk kural kılavuzunda adres aralığı 70.37.104.0/24 kabul etmelidir. Sonraki kural aralığı 0.0.0.0/0 kullanarak tüm adresleri reddedecek.

> [!NOTE]
> IP adreslerini reddetme, diğer Azure hizmetleriyle (örneğin, Azure akış analizi, Azure sanal makineleri veya portal aygıt Explorer'da) Event Hubs ile etkileşim engelleyebilir.

### <a name="creating-a-virtual-network-rule-with-azure-resource-manager-templates"></a>Azure Resource Manager şablonları ile bir sanal ağ kuralı oluşturma

Var olan bir olay hub'ları ad alanı için bir sanal ağ kuralı ekleme aşağıdaki Resource Manager şablonu sağlar.

Şablon parametreleri:

- **ipFilterRuleName** en çok 128 karakter uzunluğunda benzersiz, büyük küçük harf duyarsız, alfasayısal bir dize olmalıdır.
- **ipFilterAction** ya **Reddet** veya **kabul** IP filtre kuralı için uygulanacak eylem olarak.
- **ipMask** tek bir IPv4 adresi veya IP adreslerinin CIDR gösteriminde. Örneğin, içinde CIDR gösterimi 70.37.104.0/24 256 IPv4 adresleri 70.37.104.0 aralığı için önemli önek bit sayısını belirten 24 ile 70.37.104.255 temsil eder.

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
            "type": "Microsoft.EventHub/Namespaces/IPFilterRules",
            "properties": {
                "FilterName":"[parameters('ipFilterRuleName')]",
                "Action":"[parameters('ipFilterAction')]",              
                "IpMask": "[parameters('IpMask')]"
            }
        } 
    ]
}
```

Şablonu dağıtmak için yönergeleri izleyin [Azure Resource Manager][lnk-deploy].

## <a name="next-steps"></a>Sonraki adımlar

Azure sanal ağlar için Event hubs'a kısıtlayan erişim için şu bağlantıya bakın:

- [Olay hub'ları için sanal ağ hizmet uç noktaları][lnk-vnet]

<!-- Links -->

[express-route]:  /azure/expressroute/expressroute-faqs#supported-services
[lnk-deploy]: ../azure-resource-manager/resource-group-template-deploy.md
[lnk-vnet]: event-hubs-service-endpoints.md