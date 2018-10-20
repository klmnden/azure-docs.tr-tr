---
title: Şablon kullanarak bir Azure Event Hubs ad alanı ve tüketici grubu oluştur | Microsoft Docs
description: Bir olay hub'ı ve Azure Resource Manager şablonlarını kullanarak bir tüketici grubu ile bir Event Hubs ad alanı oluşturma
services: event-hubs
documentationcenter: .net
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: 28bb4591-1fd7-444f-a327-4e67e8878798
ms.service: event-hubs
ms.devlang: tbd
ms.topic: article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 10/16/2018
ms.author: shvija
ms.openlocfilehash: 48d41ef4df986f959dfabe04e07552e287fdfcd0
ms.sourcegitcommit: 668b486f3d07562b614de91451e50296be3c2e1f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2018
ms.locfileid: "49457111"
---
# <a name="create-an-event-hubs-namespace-with-event-hub-and-consumer-group-using-an-azure-resource-manager-template"></a>Bir Event Hubs ad alanı bir Azure Resource Manager şablonu kullanarak olay hub'ı ve tüketici grubu oluşturun
Azure Event Hubs saniyede milyonlarca olay alıp işleme kapasitesine sahip olan bir Büyük Veri akış platformu ve olay alma hizmetidir. Event Hubs dağıtılan yazılımlar ve cihazlar tarafından oluşturulan olayları, verileri ve telemetrileri işleyebilir ve depolayabilir. Bir olay hub’ına gönderilen veriler, herhangi bir gerçek zamanlı analiz sağlayıcısı ve işlem grubu oluşturma/depolama bağdaştırıcıları kullanılarak dönüştürülüp depolanabilir. Event Hubs ayrıntılı bakış için bkz: [Event Hubs'a genel bakış](event-hubs-about.md) ve [Event Hubs özellikleri](event-hubs-features.md).

Bu hızlı başlangıçta, bir Azure Resource Manager şablonu kullanarak bir olay hub'ı oluşturun. Bir Azure Resource Manager şablonu türünde bir ad alanı oluşturmak için kullandığınız [Event Hubs](event-hubs-what-is-event-hubs.md)bir olay hub'ı ve tek bir tüketici grubu. Makalede nasıl tanımlamak için hangi kaynaklara dağıtılır ve parametrelerin nasıl dağıtıldığının ve dağıtım yürütülürken belirtilen gösterilmektedir. Bu şablonu kendi dağıtımlarınız için kullanabilir veya kendi gereksinimlerinize göre özelleştirebilirsiniz. Şablonları oluşturma hakkında daha fazla bilgi için bkz: [Azure Resource Manager şablonları yazma][Authoring Azure Resource Manager templates].

Tam şablon için bkz: [olay hub'ı ve tüketici grubu şablonunu] [ Event Hub and consumer group template] GitHub üzerinde.

> [!NOTE]
> En yeni şablonları denetlemek için [Azure Hızlı Başlangıç Şablonları][Azure Quickstart Templates] galerisini ziyaret edin ve Event Hubs araması yapın.

## <a name="prerequisites"></a>Önkoşullar
Bu hızlı başlangıcı tamamlamak bir Azure aboneliğinizin olması gerekir. Biri yoksa [ücretsiz hesap oluşturun] başlamadan önce [].

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Kullanıyorsanız **Azure PowerShell** Resource Manager şablonu dağıtmak için yerel olarak, bu hızlı başlangıcı tamamlamak için PowerShell'in en son sürümünü çalıştırmanız gerekir. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell'i Yükleme ve Yapılandırma](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-5.7.0).

Yükleyip kullanmayı tercih ederseniz **Azure CLI** Resource Manager şablonu dağıtmak için yerel olarak Bu öğretici, Azure CLI 2.0.4 sürüm gerektirir veya üzeri. Sürümünüzü kontrol etmek için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme]( /cli/azure/install-azure-cli).

## <a name="what-will-you-deploy"></a>Ne dağıtacaksınız?

Bu şablonu kullanarak bir Event Hubs ad alanı bir olay hub'ı ve bir tüketici grubu ile dağıtın.

Dağıtımı otomatik olarak çalıştırmak için aşağıdaki düğmeyi seçin:

[![Azure’a dağıtma](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-event-hubs-create-event-hub-and-consumer-group%2Fazuredeploy.json)

## <a name="define-parameters"></a>Parametreleri tanımlayın
Azure Resource Manager sayesinde, şablon dağıtıldığında belirtmek istediğiniz değerlerin parametrelerini siz tanımlarsınız. Şablon, tüm parametre değerlerini içeren `Parameters` adlı bir bölüm içerir. Dağıtmakta olduğunuz projeye veya dağıtım yaptığınız ortama bağlı değişir değerleri için bir parametre tanımlamanız gerekir. Her zaman aynı kalan değerler için parametre tanımlamayın. Her bir şablon parametre değeri, dağıtılan kaynakları tanımlar.

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

Oluşturulan olay hub'ı için tüketici grubunun adıdır.

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

## <a name="define-resources-to-deploy"></a>Dağıtılacak kaynaklar tanımlayın

Türünde bir ad alanı oluşturur **EventHubs**bir olay hub'ı ve bir tüketici grubu:

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

## <a name="azure-powershell"></a>Azure PowerShell
Azure PowerShell kullanarak kaynakları dağıtmak için aşağıdaki komutu çalıştırın:

```azurepowershell
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-event-hubs-create-event-hub-and-consumer-group/azuredeploy.json
```

## <a name="azure-cli"></a>Azure CLI
Azure PowerShell kullanarak kaynakları dağıtmak için aşağıdaki komutu çalıştırın:

```azurecli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri [https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-event-hubs-create-event-hub-and-consumer-group/azuredeploy.json][]
```

Tebrikler! Bir Event Hubs ad alanı ve bu ad alanı içinde bir olay hub'ı oluşturmak için Azure Resource Manager Şablonu'ı kullandınız.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Event Hubs ad alanı oluşturan ve olay hub'ından olayları alıp göndermek için örnek uygulamaları kullanılır. Olayları (gönderip) olayları bir event hub'ından adım adım yönergeler için aşağıdaki öğreticilere bakın: 

- **Olay hub'ına olayları gönderme**: [.NET Standard](event-hubs-dotnet-standard-getstarted-send.md), [.NET Framework](event-hubs-dotnet-framework-getstarted-send.md), [Java](event-hubs-java-get-started-send.md), [Python](event-hubs-python-get-started-send.md), [Node.js ](event-hubs-node-get-started-send.md), [Git](event-hubs-go-get-started-send.md), [C](event-hubs-c-getstarted-send.md)
- **Bir olay hub'ından olay alma**: [.NET Standard](event-hubs-dotnet-standard-getstarted-receive-eph.md), [.NET Framework](event-hubs-dotnet-framework-getstarted-receive-eph.md), [Java](event-hubs-java-get-started-receive-eph.md), [Python](event-hubs-python-get-started-receive.md), [ Node.js](event-hubs-node-get-started-receive.md), [Git](event-hubs-go-get-started-receive-eph.md), [Apache Storm](event-hubs-storm-getstarted-receive.md)

[3]: ./media/event-hubs-quickstart-powershell/sender1.png
[4]: ./media/event-hubs-quickstart-powershell/receiver1.png
[5]: ./media/event-hubs-quickstart-powershell/metrics.png

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Azure Quickstart Templates]:  https://azure.microsoft.com/documentation/templates/?term=event+hubs
[Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
[Event hub and consumer group template]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-event-hubs-create-event-hub-and-consumer-group/
