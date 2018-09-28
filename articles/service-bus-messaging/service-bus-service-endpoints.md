---
title: Sanal ağ hizmet uç noktaları ve Azure Service Bus kuralları | Microsoft Docs
description: Microsoft.ServiceBus hizmet uç noktası, bir sanal ağa ekleyin.
services: event-hubs
documentationcenter: ''
author: clemensv
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.date: 09/05/2018
ms.author: clemensv
ms.openlocfilehash: 05930dfce64378d792213ccaefa3d15057bd5dfd
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2018
ms.locfileid: "47405018"
---
# <a name="use-virtual-network-service-endpoints-with-azure-service-bus"></a>Azure Service Bus ile sanal ağ hizmet uç noktaları kullanma

Service Bus ile tümleştirilmesi [(VNet) sanal ağ hizmet uç noktaları] [ vnet-sep] sanal ağlara bağlı sanal makineler gibi iş yükleri için Mesajlaşma işlevlerini güvenli erişim sağlar , her iki End'i korunan ağ trafiği yoluyla. 

En az bir sanal ağ alt ağı için hizmet uç noktasını bağlanacak yapılandırıldıktan sonra ilgili Service Bus ad alanı artık herhangi bir gelen trafiği kabul eder, ancak sanal ağlar yetkili. Sanal ağ açısından bakıldığında, sanal ağ alt ağından bir yalıtılmış ağ tüneli Mesajlaşma hizmeti için hizmet uç noktası bir Service Bus ad alanı bağlama yapılandırır.

Sonuç, alt ağ ve ilgili Service Bus ad alanı, Mesajlaşma Hizmeti uç noktası bir genel IP aralığında olma gözlemlenebilir ağ adresi artma bağlı iş yükleri arasındaki özel ve yalıtılmış bir ilişkidir.

## <a name="enable-service-endpoints-with-service-bus"></a>Hizmet uç noktaları ile Service Bus'ı etkinleştir

Sanal ağlar yalnızca desteklenen [Premium katmanı](service-bus-premium-messaging.md) Service Bus ad alanları. 

Sanal ağ hizmet uç noktaları ile Service Bus kullanırken önemli bir konu, bu uç noktaları, standart ve Premium katman hizmet veri yolu ad alanı karıştırma uygulamalarda etkinleştirmemelisiniz ' dir. Standart katman sanal ağlar desteklemediğinden, uç nokta yalnızca Premium katmanı ad sınırlıdır. VNet standart ad alanı için trafiği engeller. 

## <a name="advanced-security-scenarios-enabled-by-vnet-integration"></a>VNet tümleştirmesi etkin Gelişmiş Güvenlik senaryoları 

Sıkı ve compartmentalized güvenlik gerektiren ve sanal ağ alt ağları compartmentalized hizmetler arasında ayrılmasını sağlarsınız çözümleri genellikle yine de bu bölmeleri içinde bulunan hizmetler arasındaki iletişim yolları gerekir.

TCP/IP üzerinden HTTPS taşıyan dahil olmak üzere bölmeler arasında anında herhangi IP yönlendirme, güvenlik açıklarına karşı ağ katmanı kötüye kullanılma riskini taşır üzerinde yukarı. Burada iletileri bile, taraflar arasında geçiş olarak diske yazılır, tamamen yalıtılmış iletişim yolları Mesajlaşma hizmetleri sağlar. İlgili ağ yalıtım sınırı bütünlüğü korunur ancak her ikisi de aynı Service Bus örneğine bağlı olan iki farklı sanal ağlarda bulunan iş yüklerini verimli bir şekilde ve güvenilir bir şekilde aracılığıyla iletileri, iletişim kurabilir.
 
Bu, bulut çözümleri yalnızca Azure sektör lideri güvenilir ve ölçeklenebilir zaman uyumsuz Mesajlaşma işlevlerini erişmesine, ancak bunlar artık Mesajlaşma iletişim yolları arasında güvenli bir çözüm oluşturmak için kullanabileceğiniz önemli güvenlik compartments anlamına gelir. HTTPS ve diğer TLS Güvenli Yuva protokolleri dahil olmak üzere, tüm eşler arası iletişimi modu ile ulaşılabilir nedir daha doğal olarak daha güvenlidir.

## <a name="binding-service-bus-to-virtual-networks"></a>Service Bus'a sanal ağları bağlama

*Sanal ağ kuralları* Azure Service Bus sunucunuzun belirli bir sanal ağ alt ağından gelen bağlantıları kabul edip etmeyeceğini denetleyen güvenlik duvarı güvenliği özelliğidir.

Service Bus ad alanı, bir sanal ağa bağlama iki adımlı bir işlemdir. İlk oluşturmak gereken bir **sanal ağ hizmet uç noktası** bir sanal ağ alt ağı ve "Microsoft.ServiceBus" için açıklanan etkinleştir [hizmet uç noktası genel bakış] [ vnet-sep]. Hizmet uç noktası ekledikten sonra Service Bus ad alanı ile bağlama bir *sanal ağ kuralı*.

Sanal ağ kuralı bir adlandırılmış Service Bus ad alanı ile bir sanal ağ alt işbirliğidir. Kural bulunduğu sürece bir alt ağa bağlı tüm iş yükleri Service Bus ad alanı için erişim verilir. Service Bus kendisi asla giden bağlantı kurar, erişim gerekmez ve bu nedenle asla erişimi alt ağınız bu kuralı etkinleştirmek tarafından verilir.

### <a name="creating-a-virtual-network-rule-with-azure-resource-manager-templates"></a>Azure Resource Manager şablonları ile bir sanal ağ kuralı oluşturuluyor

Aşağıdaki Resource Manager şablonu var olan bir Service Bus ad alanı için bir sanal ağ kuralı ekleyerek sağlar.

Şablon parametreleri:

* **namespaceName**: Service Bus ad alanı.
* **vnetRuleName**: Oluşturulacak sanal ağ kuralı adı.
* **virtualNetworkingSubnetId**: tam Resource Manager yolu için sanal ağ alt ağı; Örneğin, `/subscriptions/{id}/resourceGroups/{rg}/providers/Microsoft.Network/virtualNetworks/{vnet}/subnets/default` sanal ağ varsayılan alt ağ.

Şablon:

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
          "vnetRuleName":{  
             "type":"string",
             "metadata":{  
                "description":"Name of the Authorization rule"
             }
          },
          "virtualNetworkSubnetId":{  
             "type":"string",
             "metadata":{  
                "description":"subnet Azure Resource Manager ID"
             }
          }
      },
    "resources": [
        {
            "apiVersion": "2018-01-01-preview",
            "name": "[concat(parameters('namespaceName'), '/', parameters('vnetRuleName'))]",
            "type":"Microsoft.ServiceBus/namespaces/VirtualNetworkRules",           
            "properties": {             
                "virtualNetworkSubnetId": "[parameters('virtualNetworkSubnetId')]"  
            }
        } 
    ]
}
```

Şablonu dağıtmak için yönergeleri izleyin. [Azure Resource Manager][lnk-deploy].

## <a name="next-steps"></a>Sonraki adımlar

Sanal ağlar hakkında daha fazla bilgi için aşağıdaki bağlantılara bakın:

- [Azure sanal ağ hizmet uç noktaları][vnet-sep]
- [Azure Service Bus IP filtreleme][ip-filtering]

[vnet-sep]: ../virtual-network/virtual-network-service-endpoints-overview.md
[lnk-deploy]: ../azure-resource-manager/resource-group-template-deploy.md
[ip-filtering]: service-bus-ip-filtering.md