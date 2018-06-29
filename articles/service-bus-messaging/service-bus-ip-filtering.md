---
title: Azure hizmet veri yolu IP bağlantı filtrelerini | Microsoft Docs
description: Azure hizmet veri yolu için belirli IP adreslerinden bloğu bağlantıları filtreleme IP kullanmayı.
services: service-bus
documentationcenter: ''
author: clemensv
manager: timlt
ms.service: service-bus
ms.devlang: na
ms.topic: article
ms.date: 06/26/2018
ms.author: clemensv
ms.openlocfilehash: e009bb9120fafc6edf60b68fab3336b9d1add507
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37036355"
---
# <a name="use-ip-filters"></a>IP filtreleri kullanın

Azure Service Bus olduğu yalnızca iyi bilinen belirli sitelerinden erişilebilir senaryoları için *IP Filtresi* özelliği reddetme veya belirli IPv4 adresleri olan trafiği kabul edilmesi için kuralları yapılandırmanıza olanak sağlar. Örneğin, bu adresleri bir kurumsal ağ geçidi NAT içeriğiyle olabilir.

## <a name="when-to-use"></a>Kullanılması gereken durumlar

Belirli IP adresleri için Service Bus uç noktaları engellemek yararlı olan iki belirli kullanım örnekleri şunlardır:

- Hizmet veri yolu, yalnızca belirtilen IP adreslerinden trafiğini almasına ve şey Reddet gerekir. Örneğin, Service Bus ile kullanarak [Azure Express rota] [ express-route] şirket içi altyapınıza özel bağlantılar oluşturmak için.
- Kuşkulu olarak Service Bus yönetici tarafından belirlenen IP adreslerinden gelen trafiği reddetmeye gerekir.

## <a name="how-filter-rules-are-applied"></a>Filtre kuralları nasıl uygulanır

IP filtre kuralları hizmet veri yolu ad alanı düzeyinde uygulanır. Bu nedenle, kurallar herhangi bir desteklenen protokolünü kullanarak istemcilerinden gelen tüm bağlantılara uygulanır.

Service Bus ad alanı bir rejecting IP kuralı olarak reddedilir eşleşmeleri yetkisiz bir IP adresi herhangi bağlantı girişimi. Yanıt IP kural Bahsediyor değil.

## <a name="default-setting"></a>Varsayılan ayar

Varsayılan olarak, **IP Filtresi** hizmet veri yolu için portal kılavuzunda boştur. Bu varsayılan ayarı ad alanınızı herhangi bir IP adresi bağlantılarını kabul anlamına gelir. Bu varsayılan ayarı 0.0.0.0/0 IP adresi aralığı kabul eden bir kural eşdeğerdir.

## <a name="ip-filter-rule-evaluation"></a>IP filtre kural değerlendirmesi

IP filtre kuralları sırayla uygulanır ve IP adresi ile eşleşen ilk kural kabul etme veya reddetme eylemi belirler.

Örneğin, aralığı 70.37.104.0/24 adresleri kabul edin ve başka her şeyi Reddet istiyorsanız, ilk kural kılavuzunda adres aralığı 70.37.104.0/24 kabul etmelidir. Sonraki kural aralığı 0.0.0.0/0 kullanarak tüm adresleri reddedecek.

> [!NOTE]
> IP adreslerini reddetme (Azure akış analizi, Azure sanal makineleri veya portal aygıt Explorer'da gibi) diğer Azure hizmetleriyle Service Bus ile etkileşim engelleyebilir.

### <a name="creating-a-virtual-network-rule-with-azure-resource-manager-templates"></a>Azure Resource Manager şablonları ile bir sanal ağ kuralı oluşturma

Varolan hizmet veri yolu ad alanı için bir sanal ağ kuralı ekleme aşağıdaki Resource Manager şablonu sağlar.

Şablon parametreleri:

- **ipFilterRuleName** benzersiz, büyük küçük harf duyarsız, alfasayısal bir dize en çok 128 karakter uzunluğunda olmalıdır.
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

Şablonu dağıtmak için yönergeleri izleyin [Azure Resource Manager][lnk-deploy].

## <a name="next-steps"></a>Sonraki adımlar

Azure sanal ağlar için hizmet veri yolu için kısıtlayan erişim için şu bağlantıya bakın:

- [Hizmet veri yolu için sanal ağ hizmet uç noktaları][lnk-vnet]

<!-- Links -->

[lnk-deploy]: ../azure-resource-manager/resource-group-template-deploy.md
[lnk-vnet]: service-bus-service-endpoints.md
[express-route]:  /azure/expressroute/expressroute-faqs#supported-services