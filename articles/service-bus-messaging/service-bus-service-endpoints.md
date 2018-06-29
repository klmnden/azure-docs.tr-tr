---
title: Sanal ağ hizmet uç noktaları ve Azure hizmet veri yolu için kuralları | Microsoft Docs
description: Bir sanal ağa Microsoft.ServiceBus hizmet uç noktası ekleyin.
services: event-hubs
documentationcenter: ''
author: clemensv
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.date: 06/26/2018
ms.author: clemensv
ms.openlocfilehash: 7716ff503bd492cc4b5d510758cb20d74eb82a4f
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37036828"
---
# <a name="use-virtual-network-service-endpoints-with-azure-service-bus"></a>Azure Service Bus ile sanal ağ hizmet uç noktaları kullanma

Service Bus ile tümleştirilmesi [sanal ağ (VNet) hizmet uç noktaları] [ vnet-sep] sanal ağlara bağlı olan sanal makineler gibi iş yükleri gelen Mesajlaşma işlevlerini güvenli erişim sağlar , her iki ucunun güvenli ağ trafiği yoluna sahip. 

En az bir sanal ağ alt ağ hizmeti uç noktasına bağlı olmasını yapılandırdıktan sonra ilgili hizmet veri yolu ad alanı artık yerden trafiğini kabul eder ancak sanal ağlar yetkili. Sanal ağ açısından bakıldığında, sanal ağ alt ağdan bir yalıtılmış ağ tüneli Mesajlaşma hizmeti için hizmet uç noktası için bir hizmet veri yolu ad alanı bağlama yapılandırır.

Alt ağ ve ilgili hizmet veri yolu ad alanı, Mesajlaşma Hizmeti uç noktası bir ortak IP aralığında olma observable ağ adresi tüm bağlı iş yükleri arasında özel ve yalıtılmış bir ilişki sonucudur.

## <a name="advanced-security-scenarios-enabled-by-vnet-integration"></a>VNet tümleştirmesi etkin Gelişmiş Güvenlik senaryoları 

Sıkı ve compartmentalized güvenliği gerektiren ve burada kesimleme compartmentalized hizmetleri arasında sanal ağ alt ağları sağlama çözümleri hala genellikle bu bölmeler bulunan hizmetler arasındaki iletişim yolları gerekir.

TCP/IP üzerinden HTTPS taşıyan dahil olmak üzere bölmeler arasında hemen tüm IP yolu ağ katmanı güvenlik açıklarından yararlanılması riski taşır üzerinde yukarı. Mesajlaşma Hizmetleri iletileri bile taraf arasında geçiş olarak diske yazılacağı tamamen yalıtılmış iletişim yolları sağlar. İlgili ağ yalıtımı sınır bütünlüğü korunur ancak her ikisi de aynı hizmet veri yolu örneğine bağlı olan iki farklı sanal ağlar iş yüklerini verimli bir şekilde ve güvenilir bir şekilde aracılığıyla iletileri, iletişim kurabilir.
 
Bulut çözümleri yalnızca Azure endüstri lideri güvenilir ve ölçeklenebilir zaman uyumsuz Mesajlaşma özelliklere erişmek, ancak bunlar artık Mesajlaşma iletişim yolları arasında güvenli bir çözümdür oluşturmak için kullanabilirsiniz hassas güvenlik compartments anlamına gelir eşler arası iletişim modu HTTPS ve diğer TLS Güvenli Yuva protokolleri de dahil olmak üzere tüm ulaşılabilir nedir daha kendiliğinden daha güvenlidir.

## <a name="binding-service-bus-to-virtual-networks"></a>Sanal ağlara bağlama hizmet veri yolu

*Sanal ağ kuralları* Azure Service Bus sunucunuzu belirli bir sanal ağ alt ağından gelen bağlantıları kabul edip etmeyeceğini denetleyen güvenlik duvarı güvenlik özelliğidir.

Bir hizmet veri yolu ad alanı bir sanal ağa bağlama iki adımlı bir işlemdir. İlk oluşturmak gereken bir **sanal ağ hizmeti uç noktası** , bir sanal ağ alt ağı ve "Microsoft.ServiceBus" için açıklanan etkinleştir [hizmet uç noktası genel bakış] [ vnet-sep]. Hizmet uç noktası ekledikten sonra hizmet veri yolu ad alanı ile bağlamak bir *sanal ağ kuralı*.

Sanal ağ kuralı bir adlandırılmış hizmet veri yolu ad alanı ile bir sanal ağ alt ilişkidir. Kural bulunmakla birlikte alt ağına bağlı tüm iş yükleri hizmet veri yolu ad alanına erişimi verilir. Hizmet veri yolu kendisini hiçbir zaman giden bağlantı kurar, erişim gerekmez ve bu nedenle hiçbir zaman erişimi alt ağınızı bu kural etkinleştirerek verilir.

### <a name="creating-a-virtual-network-rule-with-azure-resource-manager-templates"></a>Azure Resource Manager şablonları ile bir sanal ağ kuralı oluşturma

Varolan hizmet veri yolu ad alanı için bir sanal ağ kuralı ekleme aşağıdaki Resource Manager şablonu sağlar.

Şablon parametreleri:

* **namespaceName**: Service Bus ad alanı.
* **vnetRuleName**: sanal ağ kuralı oluşturulması için ad.
* **virtualNetworkingSubnetId**: tam Kaynak Yöneticisi'ni yolu için sanal ağ alt; Örneğin, `/subscriptions/{id}/resourceGroups/{rg}/providers/Microsoft.Network/virtualNetworks/{vnet}/subnets/default` sanal ağ varsayılan alt ağ için.

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

Şablonu dağıtmak için yönergeleri izleyin [Azure Resource Manager][lnk-deploy].

## <a name="next-steps"></a>Sonraki adımlar

Sanal ağlar hakkında daha fazla bilgi için aşağıdaki bağlantılara bakın:

- [Azure sanal ağı hizmet uç noktaları][vnet-sep]
- [Azure hizmet veri yolu IP filtreleme][ip-filtering]

[vnet-sep]: ../virtual-network/virtual-network-service-endpoints-overview.md
[lnk-deploy]: ../azure-resource-manager/resource-group-template-deploy.md
[ip-filtering]: service-bus-ip-filtering.md