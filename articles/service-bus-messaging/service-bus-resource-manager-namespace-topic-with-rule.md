---
title: Azure Service Bus konu aboneliği oluşturun ve Azure Resource Manager şablonu kullanarak kural | Microsoft Docs
description: Konu aboneliği ve Azure Resource Manager şablonu kullanarak bir kural ile bir Service Bus ad alanı oluşturma
services: service-bus-messaging
documentationcenter: .net
author: spelluru
manager: timlt
editor: ''
ms.assetid: 9e0aaf58-0214-4bca-bd00-d29c08f9b1bc
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 01/23/2019
ms.author: spelluru
ms.openlocfilehash: 5c6ad222110081cd8f8838208da407e0e1d50f75
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "54851275"
---
# <a name="create-a-service-bus-namespace-with-topic-subscription-and-rule-using-an-azure-resource-manager-template"></a>Konusu, aboneliği ve kuralı bir Azure Resource Manager şablonu kullanarak bir Service Bus ad alanı oluşturma

Bu makalede, bir konu, aboneliği ve kuralı (filtre) ile bir Service Bus ad alanı oluşturan bir Azure Resource Manager şablonunun nasıl kullanılacağı gösterilmektedir. Makalede nasıl belirtmek için hangi kaynaklara dağıtılır ve parametrelerin nasıl dağıtıldığının ve dağıtım yürütülürken belirtilen açıklanmaktadır. Bu şablonu kendi dağıtımlarınız için kullanabilir veya kendi gereksinimlerinize göre özelleştirebilirsiniz

Şablon oluşturma hakkında daha fazla bilgi için bkz. [Azure Resource Manager şablonları yazma][Authoring Azure Resource Manager templates].

Azure kaynakları adlandırma kuralları hakkında uygulama ve desenler hakkında daha fazla bilgi için bkz. [Azure kaynakları için önerilen adlandırma kuralları][Recommended naming conventions for Azure resources].

Tam şablon için bkz: [konusu, aboneliği ve kuralı ile Service Bus ad alanı] [ Service Bus namespace with topic, subscription, and rule] şablonu.

> [!NOTE]
> Aşağıdaki Azure Resource Manager şablonları, yükleme ve dağıtım için kullanılabilir.
> 
> * [Kuyruk ve yetkilendirme kuralı ile bir Service Bus ad alanı oluşturma](service-bus-resource-manager-namespace-auth-rule.md)
> * [Kuyruk ile bir Service Bus ad alanı oluşturma](service-bus-resource-manager-namespace-queue.md)
> * [Service Bus ad alanı oluşturma](service-bus-resource-manager-namespace.md)
> * [Konu ve abonelik ile Service Bus ad alanı oluşturma](service-bus-resource-manager-namespace-topic.md)
> 
> En yeni şablonları denetlemek için ziyaret [Azure hızlı başlangıç şablonları] [ Azure Quickstart Templates] galeri ve Service Bus arayın.
> 
> 

## <a name="what-do-you-deploy"></a>Ne dağıtacağınız?

Bu şablonu kullanarak bir Service Bus ad alanı konusu, aboneliği ve kuralı (filtre) ile dağıtın.

[Service Bus konuları ve abonelikleri](service-bus-queues-topics-subscriptions.md#topics-and-subscriptions) bir-çok form iletişim, sağladığınız bir *yayımlama/abone olma* deseni. Bunun yerine konuları ve Abonelikleri, dağıtılmış uygulamanın bileşenleri birbirleriyle doğrudan, iletişim kurmazlar kullanırken, aracı olarak davranan bir konu aracılığıyla iletileri değiş. Bir konu başlığına bir abonelik konu başlığına gönderilen iletilerin kopyaların alan sanal kuyruğa benzer. Filtre abonelik etkinleştirir, konu başlığına gönderilen iletilerden hangilerinin belirtmek için bir özel konu başlığı aboneliğinde görüntülenmesi gerekir.

## <a name="what-are-rules-filters"></a>(Filtreler) kuralları nelerdir?

Birçok senaryoda belirli özelliklere sahip iletileri farklı yollarla işlenmesi gerekir. Bu özel işlemeyi etkinleştirmek için belirli özelliklere sahip ve ardından bu değişiklikleri uygulamak iletilerini bulmak için abonelik yapılandırabilirsiniz. Service Bus abonelikleri konu başlığına gönderilen tüm iletilerin görmenize karşın, yalnızca sanal abonelik kuyruğuna iletiler kümesini kopyalayabilirsiniz. Abonelik filtreleri kullanarak gerçekleştirilir. (Filtreler) kuralları hakkında daha fazla bilgi için bkz: [kurallar ve Eylemler](service-bus-queues-topics-subscriptions.md#rules-and-actions).

Dağıtımı otomatik olarak çalıştırmak için aşağıdaki düğmeye tıklayın:

[![Azure’a dağıtma](./media/service-bus-resource-manager-namespace-topic/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-topic-subscription-rule%2Fazuredeploy.json)

## <a name="parameters"></a>Parametreler

Azure Resource Manager ile şablon dağıtıldığında belirtmek istediğiniz değerlerin parametrelerini tanımlayın. Şablon, tüm parametre değerlerini içeren `Parameters` adlı bir bölüm içerir. Dağıtmakta olduğunuz projeye veya dağıtım yaptığınız ortama temel farklılık, bu değerler için parametre tanımlayın. Her zaman aynı kalan değerler için parametre tanımlamayın. Her parametre değeri, dağıtılan kaynakları tanımlamak için şablonda kullanılır.

Şablon aşağıdaki parametreleri tanımlar:

### <a name="servicebusnamespacename"></a>serviceBusNamespaceName
Oluşturmak için Service Bus ad alanı adı.

```json
"serviceBusNamespaceName": {
"type": "string"
}
```

### <a name="servicebustopicname"></a>serviceBusTopicName
Service Bus ad alanında oluşturulan konu adı.

```json
"serviceBusTopicName": {
"type": "string"
}
```

### <a name="servicebussubscriptionname"></a>serviceBusSubscriptionName
Service Bus ad alanında oluşturulan abonelik adı.

```json
"serviceBusSubscriptionName": {
"type": "string"
}
```
### <a name="servicebusrulename"></a>serviceBusRuleName
Service Bus ad alanında oluşturulan rule(filter) adı.

```json
   "serviceBusRuleName": {
   "type": "string",
  }
```
### <a name="servicebusapiversion"></a>serviceBusApiVersion
Hizmet veri yolu API'sini şablon sürümü.

```json
"serviceBusApiVersion": { 
       "type": "string", 
       "defaultValue": "2017-04-01", 
       "metadata": { 
           "description": "Service Bus ApiVersion used by the template" 
       }
```
## <a name="resources-to-deploy"></a>Dağıtılacak kaynaklar
Standart bir Service Bus ad alanı türü oluşturur **Mesajlaşma**, konu ve abonelik kuralları.

```json
 "resources": [{
        "apiVersion": "[variables('sbVersion')]",
        "name": "[parameters('serviceBusNamespaceName')]",
        "type": "Microsoft.ServiceBus/Namespaces",
        "location": "[variables('location')]",
        "sku": {
            "name": "Standard",
        },
        "resources": [{
            "apiVersion": "[variables('sbVersion')]",
            "name": "[parameters('serviceBusTopicName')]",
            "type": "Topics",
            "dependsOn": [
                "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
            ],
            "properties": {
                "path": "[parameters('serviceBusTopicName')]"
            },
            "resources": [{
                "apiVersion": "[variables('sbVersion')]",
                "name": "[parameters('serviceBusSubscriptionName')]",
                "type": "Subscriptions",
                "dependsOn": [
                    "[parameters('serviceBusTopicName')]"
                ],
                "properties": {},
                "resources": [{
                    "apiVersion": "[variables('sbVersion')]",
                    "name": "[parameters('serviceBusRuleName')]",
                    "type": "Rules",
                    "dependsOn": [
                        "[parameters('serviceBusSubscriptionName')]"
                    ],
                    "properties": {
                        "filterType": "SqlFilter",
                        "sqlFilter": {
                            "sqlExpression": "StoreName = 'Store1'",
                            "requiresPreprocessing": "false"
                        },
                        "action": {
                            "sqlExpression": "set FilterTag = 'true'"
                        }
                    }
                }]
            }]
        }]
    }]
```

JSON söz dizimi ve özellikler için bkz: [ad alanları](/azure/templates/microsoft.servicebus/namespaces), [konuları](/azure/templates/microsoft.servicebus/namespaces/topics), [abonelikleri](/azure/templates/microsoft.servicebus/namespaces/topics/subscriptions), ve [kuralları](/azure/templates/microsoft.servicebus/namespaces/topics/subscriptions/rules).

## <a name="commands-to-run-deployment"></a>Dağıtımı çalıştırma komutları
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>PowerShell
```powershell
New-AzureResourceGroupDeployment -Name \<deployment-name\> -ResourceGroupName \<resource-group-name\> -TemplateUri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-subscription-rule/azuredeploy.json>
```

## <a name="azure-cli"></a>Azure CLI
```azurecli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-subscription-rule/azuredeploy.json>
```

## <a name="next-steps"></a>Sonraki adımlar
Oluşturulan ve dağıtılan kaynakları Azure Resource Manager kullanarak göre bu kaynakları Bu makaleler görüntüleyerek yönetmeyi öğrenin:

* [Azure Service Bus'ı yönetme](service-bus-management-libraries.md)
* [Service Bus’ı PowerShell ile yönetme](service-bus-manage-with-ps.md)
* [Service Bus Explorer ile Service Bus kaynaklarını yönetme](https://github.com/paolosalvatori/ServiceBusExplorer/releases)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Azure Quickstart Templates]: https://azure.microsoft.com/documentation/templates/?term=service+bus
[Learn more about Service Bus topics and subscriptions]: service-bus-queues-topics-subscriptions.md
[Using Azure PowerShell with Azure Resource Manager]: ../azure-resource-manager/powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md
[Recommended naming conventions for Azure resources]: ../guidance/guidance-naming-conventions.md
[Service Bus namespace with topic, subscription, and rule]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-topic-subscription-rule/
[Service Bus queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md

