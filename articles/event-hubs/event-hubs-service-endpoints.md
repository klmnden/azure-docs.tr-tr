---
title: Sanal ağ hizmet uç noktaları ve Azure Event Hubs için kuralları | Microsoft Docs
description: Microsoft.EventHub hizmet uç noktası, bir sanal ağa ekleyin.
services: event-hubs
documentationcenter: ''
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.date: 08/16/2018
ms.author: shvija
ms.openlocfilehash: 29b5f877065029dc271e49c1afd6d547def58a6e
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2018
ms.locfileid: "49408141"
---
# <a name="use-virtual-network-service-endpoints-with-azure-event-hubs"></a>Azure Event Hubs ile sanal ağ hizmet uç noktaları kullanma

Event Hubs ile tümleştirilmesi [(VNet) sanal ağ hizmet uç noktaları] [ vnet-sep] sanal olana bağlanmış sanal makineleri gibi iş yükleri için Mesajlaşma işlevlerini güvenli erişim sağlar Her iki End'i korunan ağ trafiği yoluyla ağ. 

> [!IMPORTANT]
> Sanal ağlar desteklenir **standart** ve **adanmış** Event Hubs'ın katmanları. Temel katmanda desteklenmiyor. 

En az bir sanal ağ alt ağı için hizmet uç noktasını bağlanacak yapılandırıldıktan sonra ilgili Event Hubs ad alanı artık her yerde trafiği kabul eder, ancak sanal ağlardaki alt ağlara yetkili. Sanal ağ açısından bakıldığında, sanal ağ alt ağından bir yalıtılmış ağ tüneli Mesajlaşma hizmeti için hizmet uç noktası bir Event Hubs ad alanı bağlama yapılandırır.

Sonuç, alt ağ ve ilgili Event Hubs ad alanı, Mesajlaşma Hizmeti uç noktası bir genel IP aralığında olma gözlemlenebilir ağ adresi artma bağlı iş yükleri arasındaki özel ve yalıtılmış bir ilişkidir.

## <a name="advanced-security-scenarios-enabled-by-vnet-integration"></a>VNet tümleştirmesi etkin Gelişmiş Güvenlik senaryoları 

Sıkı ve compartmentalized güvenlik gerektiren ve sanal ağ alt ağları compartmentalized hizmetler arasında ayrılmasını sağlarsınız çözümleri genellikle yine de bu bölmeleri içinde bulunan hizmetler arasındaki iletişim yolları gerekir.

TCP/IP üzerinden HTTPS taşıyan dahil olmak üzere bölmeler arasında anında herhangi IP yönlendirme, güvenlik açıklarına karşı ağ katmanı kötüye kullanılma riskini taşır üzerinde yukarı. Burada iletileri bile, taraflar arasında geçiş olarak diske yazılır, tamamen yalıtılmış iletişim yolları Mesajlaşma hizmetleri sağlar. İlgili ağ yalıtım sınırı bütünlüğü korunur ancak her ikisi de aynı Event Hubs örneğine bağlı olan iki farklı sanal ağlarda bulunan iş yüklerini verimli bir şekilde ve güvenilir bir şekilde aracılığıyla iletileri, iletişim kurabilir.
 
Bu, bulut çözümleri yalnızca Azure sektör lideri güvenilir ve ölçeklenebilir zaman uyumsuz Mesajlaşma işlevlerini erişmesine, ancak bunlar artık Mesajlaşma iletişim yolları arasında güvenli bir çözüm oluşturmak için kullanabileceğiniz önemli güvenlik compartments anlamına gelir. HTTPS ve diğer TLS Güvenli Yuva protokolleri dahil olmak üzere, tüm eşler arası iletişimi modu ile ulaşılabilir nedir daha doğal olarak daha güvenlidir.

## <a name="bind-event-hubs-to-virtual-networks"></a>Olay hub'ları sanal ağlara bağlama

*Sanal ağ kuralları* Azure Event Hubs ad alanınız belirli bir sanal ağ alt ağından gelen bağlantıları kabul edip etmeyeceğini denetleyen güvenlik duvarı güvenliği özelliğidir.

Bir Event Hubs ad alanı, bir sanal ağa bağlama iki adımlı bir işlemdir. İlk oluşturmak gereken bir **sanal ağ hizmet uç noktası** bir sanal ağ alt ağı ve "Microsoft.EventHub" için açıklanan etkinleştir [hizmet uç noktası genel bakış] [ vnet-sep]. Hizmet uç noktası ekledikten sonra Event Hubs ad alanı ile bağlama bir *sanal ağ kuralı*.

Sanal ağ kuralı bir adlandırılmış Event Hubs ad alanı ile bir sanal ağ alt işbirliğidir. Kural bulunduğu sürece bir alt ağa bağlı tüm iş yükleri Event Hubs ad alanına erişimi verilir. Event hubs'ı kendisi asla giden bağlantı kurar, erişim gerekmez ve bu nedenle asla erişimi alt ağınız bu kuralı etkinleştirmek tarafından verilir.

### <a name="create-a-virtual-network-rule-with-azure-resource-manager-templates"></a>Azure Resource Manager şablonları ile bir sanal ağ kuralı oluşturma

Aşağıdaki Resource Manager şablonu var olan bir Event Hubs ad alanı için bir sanal ağ kuralı ekleyerek sağlar.

Şablon parametreleri:

* **namespaceName**: Event Hubs ad alanı.
* **vnetRuleName**: Oluşturulacak sanal ağ kuralı adı.
* **virtualNetworkingSubnetId**: tam Resource Manager yolu için sanal ağ alt ağı; Örneğin, `subscriptions/{id}/resourceGroups/{rg}/providers/Microsoft.Network/virtualNetworks/{vnet}/subnets/default` sanal ağ varsayılan alt ağ.

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
            "type":"Microsoft.EventHub/namespaces/VirtualNetworkRules",         
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
- [Azure Event Hubs IP filtreleme][ip-filtering]

[vnet-sep]: ../virtual-network/virtual-network-service-endpoints-overview.md
[lnk-deploy]: ../azure-resource-manager/resource-group-template-deploy.md
[ip-filtering]: event-hubs-ip-filtering.md