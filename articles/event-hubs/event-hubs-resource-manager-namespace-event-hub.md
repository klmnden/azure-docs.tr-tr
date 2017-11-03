---
title: "Bir şablonu kullanarak bir Azure Event Hubs ad alanı ve tüketici grubu oluşturun | Microsoft Docs"
description: "Azure Resource Manager şablonları kullanarak bir tüketici grubu ve bir event hub ile Event Hubs ad alanı oluşturma"
services: event-hubs
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 28bb4591-1fd7-444f-a327-4e67e8878798
ms.service: event-hubs
ms.devlang: tbd
ms.topic: article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 10/09/2017
ms.author: sethm;shvija
ms.openlocfilehash: 4cc9a0b9eaabb15a5a316e094deb178ef2219692
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-an-event-hubs-namespace-with-event-hub-and-consumer-group-using-an-azure-resource-manager-template"></a>Bir Azure Resource Manager şablonu kullanarak olay hub'ı ve tüketici grubu ile bir olay hub'ları ad alanı oluşturma

Bu makalede bir olay hub ile Event Hubs türünde bir ad ve bir tüketici grubu oluşturur bir Azure Resource Manager şablonu kullanmayı gösterir. Makaleyi nasıl tanımlamak için hangi kaynağın dağıtılan ve ne zaman dağıtım yürütülen parametreler tanımlamak nasıl belirtilen gösterir. Bu şablonu kendi dağıtımlarınız için kullanabilir veya kendi gereksinimlerinize göre özelleştirebilirsiniz

Şablon oluşturma hakkında daha fazla bilgi için bkz. [Azure Resource Manager şablonları yazma][Authoring Azure Resource Manager templates].

Tam şablon için bkz: [olay hub'ı ve tüketici grubu şablonunu] [ Event Hub and consumer group template] github'da.

> [!NOTE]
> En yeni şablonları denetlemek için [Azure Hızlı Başlangıç Şablonları][Azure Quickstart Templates] galerisini ziyaret edin ve Event Hubs araması yapın.
> 
> 

## <a name="what-will-you-deploy"></a>Ne dağıtacaksınız?
Bu şablon kullanılarak bir olay hub'ı ve bir tüketici grubu içeren bir olay hub'ları ad dağıtır.

[Event Hubs](event-hubs-what-is-event-hubs.md), düşük gecikme süresi ve yüksek güvenilirlikle Azure’a büyük ölçekte olay ve telemetri girişi sağlayan bir olay işleme hizmetidir.

Dağıtımı otomatik olarak çalıştırmak için aşağıdaki düğmeye tıklayın:

[![Azure’a dağıtma](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-event-hubs-create-event-hub-and-consumer-group%2Fazuredeploy.json)

## <a name="parameters"></a>Parametreler
Azure Resource Manager sayesinde, şablon dağıtıldığında belirtmek istediğiniz değerlerin parametrelerini siz tanımlarsınız. Şablon, tüm parametre değerlerini içeren `Parameters` adlı bir bölüm içerir. Dağıttığınız projesini temel alan veya dağıtmakta olduğunuz ortamına bağlı olarak değişir bu değerleri için bir parametre tanımlamanız gerekir. Her zaman aynı kalan değerler için parametre tanımlamayın. Her parametre değeri şablondaki dağıtılan kaynakları tanımlar.

Şablon aşağıdaki parametreleri tanımlar:

### <a name="eventhubnamespacename"></a>eventHubNamespaceName
Oluşturulacak Event Hubs ad alanının adı.

```json
"eventHubNamespaceName": {
"type": "string"
}
```

### <a name="eventhubname"></a>eventHubName
Event Hubs ad alanında oluşturulan olay hub’ının adı.

```json
"eventHubName": {
"type": "string"
}
```

### <a name="eventhubconsumergroupname"></a>eventHubConsumerGroupName
Olay hub'ı için oluşturulan tüketici grubu adı.

```json
"eventHubConsumerGroupName": {
"type": "string"
}
```

### <a name="apiversion"></a>apiVersion
Şablonun API sürümü.

```json
"apiVersion": {
"type": "string"
}
```

## <a name="resources-to-deploy"></a>Dağıtılacak kaynaklar
Tür bir ad oluşturur **EventHubs**bir olay hub'ı ve bir tüketici grubu.

```json
"resources":[  
      {  
         "apiVersion":"[variables('ehVersion')]",
         "name":"[parameters('namespaceName')]",
         "type":"Microsoft.EventHub/namespaces",
         "location":"[variables('location')]",
         "sku":{  
            "name":"Standard",
            "tier":"Standard"
         },
         "resources":[  
            {  
               "apiVersion":"[variables('ehVersion')]",
               "name":"[parameters('eventHubName')]",
               "type":"EventHubs",
               "dependsOn":[  
                  "[concat('Microsoft.EventHub/namespaces/', parameters('namespaceName'))]"
               ],
               "properties":{  
                  "path":"[parameters('eventHubName')]"
               },
               "resources":[  
                  {  
                     "apiVersion":"[variables('ehVersion')]",
                     "name":"[parameters('consumerGroupName')]",
                     "type":"ConsumerGroups",
                     "dependsOn":[  
                        "[parameters('eventHubName')]"
                     ],
                     "properties":{  

                     }
                  }
               ]
            }
         ]
      }
   ],
```

## <a name="commands-to-run-deployment"></a>Dağıtımı çalıştırma komutları
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>PowerShell
```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-event-hubs-create-event-hub-and-consumer-group/azuredeploy.json
```

## <a name="azure-cli"></a>Azure CLI
```azurecli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri [https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-event-hubs-create-event-hub-and-consumer-group/azuredeploy.json][]
```

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki bağlantıları inceleyerek Event Hubs hakkında daha fazla bilgi edinebilirsiniz:

* [Event Hubs’a genel bakış](event-hubs-what-is-event-hubs.md)
* [Olay Hub’ı oluşturma](event-hubs-create.md)
* [Event Hubs ile ilgili SSS](event-hubs-faq.md)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Azure Quickstart Templates]:  https://azure.microsoft.com/documentation/templates/?term=event+hubs
[Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
[Event hub and consumer group template]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-event-hubs-create-event-hub-and-consumer-group/
