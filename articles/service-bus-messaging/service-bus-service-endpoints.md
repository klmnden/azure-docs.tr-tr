---
title: Sanal ağ hizmet uç noktaları ve Azure Service Bus kuralları | Microsoft Docs
description: Microsoft.ServiceBus hizmet uç noktası, bir sanal ağa ekleyin.
services: service-bus
documentationcenter: ''
author: axisc
manager: timlt
editor: spelluru
ms.service: service-bus
ms.devlang: na
ms.topic: article
ms.date: 09/05/2018
ms.author: aschhab
ms.openlocfilehash: 0801469d586e6f2d6514927cdc7b894900a3aa35
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61471970"
---
# <a name="use-virtual-network-service-endpoints-with-azure-service-bus"></a>Azure Service Bus ile sanal ağ hizmet uç noktaları kullanma

Service Bus ile tümleştirilmesi [(VNet) sanal ağ hizmet uç noktaları] [ vnet-sep] sanal ağlara bağlı sanal makineler gibi iş yükleri için Mesajlaşma işlevlerini güvenli erişim sağlar , her iki End'i korunan ağ trafiği yoluyla.

En az bir sanal ağ alt ağı için hizmet uç noktasını bağlanacak yapılandırıldıktan sonra ilgili Service Bus ad alanı artık herhangi bir gelen trafiği kabul eder, ancak sanal ağlar yetkili. Sanal ağ açısından bakıldığında, sanal ağ alt ağından bir yalıtılmış ağ tüneli Mesajlaşma hizmeti için hizmet uç noktası bir Service Bus ad alanı bağlama yapılandırır.

Sonuç, alt ağ ve ilgili Service Bus ad alanı, Mesajlaşma Hizmeti uç noktası bir genel IP aralığında olma gözlemlenebilir ağ adresi artma bağlı iş yükleri arasındaki özel ve yalıtılmış bir ilişkidir.

>[!WARNING]
> Sanal ağlar tümleştirme uygulama diğer Azure Hizmetleri, Service Bus ile etkileşim engelleyebilirsiniz.
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
> - Azure App Service
> - Azure İşlevleri

> [!IMPORTANT]
> Sanal ağlar yalnızca desteklenen [Premium katmanı](service-bus-premium-messaging.md) Service Bus ad alanları.

## <a name="enable-service-endpoints-with-service-bus"></a>Hizmet uç noktaları ile Service Bus'ı etkinleştir

Sanal ağ hizmet uç noktaları ile Service Bus kullanırken önemli bir konu, bu uç noktaları, standart ve Premium katman hizmet veri yolu ad alanı karıştırma uygulamalarda etkinleştirmemelisiniz ' dir. Standart katman sanal ağlar desteklemediğinden, uç nokta yalnızca Premium katmanı ad sınırlıdır.

## <a name="advanced-security-scenarios-enabled-by-vnet-integration"></a>VNet tümleştirmesi etkin Gelişmiş Güvenlik senaryoları 

Sıkı ve compartmentalized güvenlik gerektiren ve sanal ağ alt ağları compartmentalized hizmetler arasında ayrılmasını sağlarsınız çözümleri genellikle yine de bu bölmeleri içinde bulunan hizmetler arasındaki iletişim yolları gerekir.

TCP/IP üzerinden HTTPS taşıyan dahil olmak üzere bölmeler arasında anında herhangi IP yönlendirme, güvenlik açıklarına karşı ağ katmanı kötüye kullanılma riskini taşır üzerinde yukarı. Burada iletileri bile, taraflar arasında geçiş olarak diske yazılır, tamamen yalıtılmış iletişim yolları Mesajlaşma hizmetleri sağlar. İlgili ağ yalıtım sınırı bütünlüğü korunur ancak her ikisi de aynı Service Bus örneğine bağlı olan iki farklı sanal ağlarda bulunan iş yüklerini verimli bir şekilde ve güvenilir bir şekilde aracılığıyla iletileri, iletişim kurabilir.
 
Bu, bulut çözümleri yalnızca Azure sektör lideri güvenilir ve ölçeklenebilir zaman uyumsuz Mesajlaşma işlevlerini erişmesine, ancak bunlar artık Mesajlaşma iletişim yolları arasında güvenli bir çözüm oluşturmak için kullanabileceğiniz önemli güvenlik compartments anlamına gelir. HTTPS ve diğer TLS Güvenli Yuva protokolleri dahil olmak üzere, tüm eşler arası iletişimi modu ile ulaşılabilir nedir daha doğal olarak daha güvenlidir.

## <a name="binding-service-bus-to-virtual-networks"></a>Service Bus'a sanal ağları bağlama

*Sanal ağ kuralları* Azure Service Bus sunucunuzun belirli bir sanal ağ alt ağından gelen bağlantıları kabul edip etmeyeceğini denetleyen güvenlik duvarı güvenliği özelliğidir.

Service Bus ad alanı, bir sanal ağa bağlama iki adımlı bir işlemdir. İlk oluşturmak gereken bir **sanal ağ hizmet uç noktası** bir sanal ağ alt ağı ve "Microsoft.ServiceBus" için açıklanan etkinleştir [hizmet uç noktası genel bakış] [ vnet-sep]. Hizmet uç noktası ekledikten sonra Service Bus ad alanı ile bağlama bir *sanal ağ kuralı*.

Service Bus ad alanı bir sanal ağ alt ağ ile sanal ağ kuralı işbirliğidir. Kural bulunduğu sürece bir alt ağa bağlı tüm iş yükleri Service Bus ad alanı için erişim verilir. Service Bus kendisi asla giden bağlantı kurar, erişim gerekmez ve bu nedenle asla erişimi alt ağınız bu kuralı etkinleştirmek tarafından verilir.

### <a name="creating-a-virtual-network-rule-with-azure-resource-manager-templates"></a>Azure Resource Manager şablonları ile bir sanal ağ kuralı oluşturuluyor

Aşağıdaki Resource Manager şablonu var olan bir Service Bus ad alanı için bir sanal ağ kuralı ekleyerek sağlar.

Şablon parametreleri:

* **namespaceName**: Service Bus ad alanı.
* **virtualNetworkingSubnetId**: Sanal ağ alt ağı için Resource Manager tam yolunu; Örneğin, `/subscriptions/{id}/resourceGroups/{rg}/providers/Microsoft.Network/virtualNetworks/{vnet}/subnets/default` sanal ağ varsayılan alt ağ.

> [!NOTE]
> Olası hiçbir Reddet kural varken, Azure Resource Manager şablonu ayarlanmış varsayılan eylem sahip **"İzin ver"** hangi bağlantıları kısıtlama yoktur.
> Sanal ağ veya güvenlik duvarı kuralları yaparken, ki değiştirmeli ***"Defaultactıon"***
> 
> başlangıç
> ```json
> "defaultAction": "Allow"
> ```
> -
> ```json
> "defaultAction": "Deny"
> ```
>

Şablon:

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
      "namespaceNetworkRuleSetName": "[concat(parameters('servicebusNamespaceName'), concat('/', 'default'))]",
      "subNetId": "[resourceId('Microsoft.Network/virtualNetworks/subnets/', parameters('virtualNetworkName'), parameters('subnetName'))]"
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
                    "service": "Microsoft.ServiceBus"
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
        "type": "Microsoft.ServiceBus/namespaces/networkruleset",
        "dependsOn": [
          "[concat('Microsoft.ServiceBus/namespaces/', parameters('servicebusNamespaceName'))]"
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
- [Azure Service Bus IP filtreleme][ip-filtering]

[vnet-sep]: ../virtual-network/virtual-network-service-endpoints-overview.md
[lnk-deploy]: ../azure-resource-manager/resource-group-template-deploy.md
[ip-filtering]: service-bus-ip-filtering.md
