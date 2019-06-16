---
title: Sanal ağ hizmet uç noktaları - Azure Event Hubs | Microsoft Docs
description: Bu makalede, bir sanal ağa Microsoft.EventHub hizmet uç noktası ekleme hakkında bilgi sağlar.
services: event-hubs
documentationcenter: ''
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.date: 03/12/2019
ms.author: shvija
ms.openlocfilehash: 15912ce2e100a4317e775d72972ca6eacfac0d42
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67080531"
---
# <a name="use-virtual-network-service-endpoints-with-azure-event-hubs"></a>Azure Event Hubs ile sanal ağ hizmet uç noktaları kullanma

Event Hubs ile tümleştirilmesi [(VNet) sanal ağ hizmet uç noktaları] [ vnet-sep] sanal olana bağlanmış sanal makineleri gibi iş yükleri için Mesajlaşma işlevlerini güvenli erişim sağlar Her iki End'i korunan ağ trafiği yoluyla ağ.

En az bir sanal ağ alt ağı için hizmet uç noktasını bağlı yapılandırıldıktan sonra ilgili Event Hubs ad alanı artık her yerde trafiği kabul eder, ancak sanal ağlardaki alt ağlara yetkili. Sanal ağ açısından bakıldığında, sanal ağ alt ağından bir yalıtılmış ağ tüneli Mesajlaşma hizmeti için hizmet uç noktası bir Event Hubs ad alanı bağlama yapılandırır. 

Sonuç, alt ağ ve ilgili Event Hubs ad alanı, Mesajlaşma Hizmeti uç noktası bir genel IP aralığında olma gözlemlenebilir ağ adresi artma bağlı iş yükleri arasındaki özel ve yalıtılmış bir ilişkidir. Bu davranış bir istisna vardır. Varsayılan olarak, bir hizmet uç noktası etkinleştirildiğinde, sanal ağ ile ilişkili IP Güvenlik Duvarı'nda denyall kuralı etkinleştirilir. Olay hub'ı genel uç noktaya erişimi etkinleştirmek üzere IP Güvenlik Duvarı'nda, belirli IP adresleri ekleyebilirsiniz. 


>[!WARNING]
> Sanal ağlar tümleştirme uygulama diğer Azure Hizmetleri, Event Hubs ile etkileşim engelleyebilirsiniz.
>
> Sanal ağlar uygulandığında Hizmetleri desteklenmez Microsoft güvenilir.
>
> Sanal ağlar ile çalışmayan genel Azure senaryoları (liste Not **değil** kapsamlı)-
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

> [!IMPORTANT]
> Sanal ağlar desteklenir **standart** ve **adanmış** Event Hubs'ın katmanları. Temel katmanda desteklenmiyor.

## <a name="advanced-security-scenarios-enabled-by-vnet-integration"></a>VNet tümleştirmesi etkin Gelişmiş Güvenlik senaryoları 

Sıkı ve compartmentalized güvenlik gerektiren ve sanal ağ alt ağları compartmentalized hizmetler arasında ayrılmasını sağlarsınız çözümleri, söz konusu bölmelerden içinde bulunan hizmetler arasındaki iletişim yolları yine de gerekir.

TCP/IP üzerinden HTTPS taşıyan dahil olmak üzere bölmeler arasında anında herhangi IP yönlendirme, güvenlik açıklarına karşı ağ katmanı kötüye kullanılma riskini taşır üzerinde yukarı. Burada iletileri bile, taraflar arasında geçiş olarak diske yazılır, tamamen yalıtılmış iletişim yolları Mesajlaşma hizmetleri sağlar. İlgili ağ yalıtım sınırı bütünlüğü korunur ancak her ikisi de aynı Event Hubs örneğine bağlı olan iki farklı sanal ağlarda bulunan iş yüklerini verimli bir şekilde ve güvenilir bir şekilde aracılığıyla iletileri, iletişim kurabilir.
 
Bu, bulut çözümleri yalnızca Azure sektör lideri güvenilir ve ölçeklenebilir zaman uyumsuz Mesajlaşma işlevlerini erişmesine, ancak bunlar artık Mesajlaşma iletişim yolları arasında güvenli bir çözüm oluşturmak için kullanabileceğiniz önemli güvenlik compartments anlamına gelir. HTTPS ve diğer TLS Güvenli Yuva protokolleri dahil olmak üzere, tüm eşler arası iletişimi modu ile ulaşılabilir nedir daha doğal olarak daha güvenlidir.

## <a name="bind-event-hubs-to-virtual-networks"></a>Olay hub'ları sanal ağlara bağlama

*Sanal ağ kuralları* Azure Event Hubs ad alanınız belirli bir sanal ağ alt ağından gelen bağlantıları kabul edip etmeyeceğini denetleyen güvenlik duvarı güvenliği özelliğidir.

Bir Event Hubs ad alanı, bir sanal ağa bağlama iki adımlı bir işlemdir. İlk oluşturmak gereken bir **sanal ağ hizmet uç noktası** bir sanal ağ alt ağı ve "Microsoft.EventHub" için açıklanan etkinleştir [hizmet uç noktası genel bakış] [ vnet-sep]. Hizmet uç noktası ekledikten sonra Event Hubs ad alanı ile bağlama bir *sanal ağ kuralı*.

Event Hubs ad alanı bir sanal ağ alt ağ ile sanal ağ kuralı işbirliğidir. Kural bulunduğu sürece bir alt ağa bağlı tüm iş yükleri Event Hubs ad alanına erişimi verilir. Event hubs'ı kendisi asla giden bağlantı kurar, erişim gerekmez ve bu nedenle asla erişimi alt ağınız bu kuralı etkinleştirmek tarafından verilir.

### <a name="create-a-virtual-network-rule-with-azure-resource-manager-templates"></a>Azure Resource Manager şablonları ile bir sanal ağ kuralı oluşturma

Aşağıdaki Resource Manager şablonu var olan bir Event Hubs ad alanı için bir sanal ağ kuralı ekleyerek sağlar.

Şablon parametreleri:

* **namespaceName**: Event Hubs ad alanı.
* **vnetRuleName**: Oluşturulacak sanal ağ kuralı adı.
* **virtualNetworkingSubnetId**: Sanal ağ alt ağı için Resource Manager tam yolunu; Örneğin, `/subscriptions/{id}/resourceGroups/{rg}/providers/Microsoft.Network/virtualNetworks/{vnet}/subnets/default` sanal ağ varsayılan alt ağ.

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
      "virtualNetworkName": {
        "type": "string",
        "metadata": {
          "description": "Name of the Virtual Network Rule"
        }
      },
      "subnetName": {
        "type": "string",
        "metadata": {
          "description": "Name of the Virtual Network Sub Net"
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
      "subNetId": "[resourceId('Microsoft.Network/virtualNetworks/subnets/', parameters('virtualNetworkName'), parameters('subnetName'))]"
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
        "apiVersion": "2017-09-01",
        "name": "[parameters('virtualNetworkName')]",
        "location": "[parameters('location')]",
        "type": "Microsoft.Network/virtualNetworks",
        "properties": {
          "addressSpace": {
            "addressPrefixes": [
              "10.0.0.0/23"
            ]
          },
          "subnets": [
            {
              "name": "[parameters('subnetName')]",
              "properties": {
                "addressPrefix": "10.0.0.0/23",
                "serviceEndpoints": [
                  {
                    "service": "Microsoft.EventHub"
                  }
                ]
              }
            }
          ]
        }
      },
      {
        "apiVersion": "2018-01-01-preview",
        "name": "[variables('namespaceNetworkRuleSetName')]",
        "type": "Microsoft.EventHub/namespaces/networkruleset",
        "dependsOn": [
          "[concat('Microsoft.EventHub/namespaces/', parameters('eventhubNamespaceName'))]"
        ],
        "properties": {
          "virtualNetworkRules": 
          [
            {
              "subnet": {
                "id": "[variables('subNetId')]"
              },
              "ignoreMissingVnetServiceEndpoint": false
            }
          ],
          "ipRules":[<YOUR EXISTING IP RULES>],
          "defaultAction": "Deny"
        }
      }
    ],
    "outputs": { }
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
