---
title: "Azure Resource Manager şablonu kullanarak Azure Service Bus ad alanı konu aboneliği oluştur | Microsoft Docs"
description: "Konu ve abonelik Azure Resource Manager şablonu kullanarak bir hizmet veri yolu ad alanı oluşturma"
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: d3d55200-5c60-4b5f-822d-59974cafff0e
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 08/07/2017
ms.author: sethm;shvija
ms.openlocfilehash: 8dd48787e7b788d249085b3110484de1a2c1d265
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-service-bus-namespace-with-topic-and-subscription-using-an-azure-resource-manager-template"></a>Konu ve abonelik bir Azure Resource Manager şablonu kullanarak bir hizmet veri yolu ad alanı oluşturma

Bu makale bir hizmet veri yolu ad alanı ve bir konu, ad alanı içinde abonelik oluşturur ve bir Azure Resource Manager şablonu kullanmayı gösterir. Nasıl tanımlamak için hangi kaynağın dağıtılan ve ne zaman dağıtım yürütülen parametreler tanımlamak nasıl belirtilen öğreneceksiniz. Bu şablonu kendi dağıtımlarınız için kullanabilir veya kendi gereksinimlerinize göre özelleştirebilirsiniz

Şablonları oluşturma hakkında daha fazla bilgi için lütfen bkz [Azure Resource Manager şablonları yazma][Authoring Azure Resource Manager templates].

Tam şablon için bkz: [konu ve abonelik ile Service Bus ad alanı] [ Service Bus namespace with topic and subscription] şablonu.

> [!NOTE]
> Aşağıdaki Azure Resource Manager şablonları, yükleme ve dağıtım için kullanılabilir.
> 
> * [Hizmet veri yolu ad alanı oluşturma](service-bus-resource-manager-namespace.md)
> * [Sıra ile Service Bus ad alanı oluşturma](service-bus-resource-manager-namespace-queue.md)
> * [Kuyruk ve yetkilendirme kuralı ile Service Bus ad alanı oluşturma](service-bus-resource-manager-namespace-auth-rule.md)
> * [Konu, abonelik ve kuralı ile Service Bus ad alanı oluşturma](service-bus-resource-manager-namespace-topic-with-rule.md)
> 
> Son şablonları denetlemek için ziyaret edin [Azure hızlı başlangıç şablonlarını] [ Azure Quickstart Templates] Galerisi ve "Service Bus" için arama
> 
> 

## <a name="what-will-you-deploy"></a>Ne dağıtacaksınız?

Bu şablonla, konu ve abonelik içeren bir hizmet veri yolu ad dağıtır.

[Service Bus konuları ve abonelikleri](service-bus-queues-topics-subscriptions.md#topics-and-subscriptions) iletişim, bir çok form sağladığınız bir *Yayınla/Abone ol* düzeni.

Dağıtımı otomatik olarak çalıştırmak için aşağıdaki düğmeye tıklayın:

[![Azure’a dağıtma](./media/service-bus-resource-manager-namespace-topic/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-topic-and-subscription%2Fazuredeploy.json)

## <a name="parameters"></a>Parametreler

Azure Resource Manager sayesinde, şablon dağıtıldığında belirtmek istediğiniz değerlerin parametrelerini siz tanımlarsınız. Şablon adlı bir bölüm içerir `Parameters` , tüm parametre değerlerini içerir. Dağıttığınız projesini temel alan veya dağıttığınız ortamı dayanarak değişir bu değerleri için bir parametre tanımlamanız gerekir. Her zaman aynı kalır değerleri parametrelerini tanımlamayın. Her parametre değeri, dağıtılan kaynakları tanımlamak için şablonda kullanılır.

Şablon aşağıdaki parametreleri tanımlar.

### <a name="servicebusnamespacename"></a>serviceBusNamespaceName
Oluşturmak için hizmet veri yolu ad alanı adı.

```json
"serviceBusNamespaceName": {
"type": "string"
}
```

### <a name="servicebustopicname"></a>serviceBusTopicName
Hizmet veri yolu ad alanında oluşturulan konu adı.

```json
"serviceBusTopicName": {
"type": "string"
}
```

### <a name="servicebussubscriptionname"></a>serviceBusSubscriptionName
Hizmet veri yolu ad alanında oluşturulan abonelik adı.

```json
"serviceBusSubscriptionName": {
"type": "string"
}
```

### <a name="servicebusapiversion"></a>serviceBusApiVersion
Şablon hizmet veri yolu API'sini sürümü.

```json
"serviceBusApiVersion": {
"type": "string"
}
```
## <a name="resources-to-deploy"></a>Dağıtılacak kaynaklar
Standart bir hizmet veri yolu ad alanı türü oluşturur **ileti**, konu ve abonelik ile.

```json
"resources ": [{
        "apiVersion": "[variables('sbVersion')]",
        "name": "[parameters('serviceBusNamespaceName')]",
        "type": "Microsoft.ServiceBus/Namespaces",
        "location": "[variables('location')]",
        "kind": "Messaging",
        "sku": {
            "name": "StandardSku",
            "tier": "Standard"
        },
        "resources": [{
            "apiVersion": "[variables('sbVersion')]",
            "name": "[parameters('serviceBusTopicName')]",
            "type": "Topics",
            "dependsOn": [
                "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
            ],
            "properties": {
                "path": "[parameters('serviceBusTopicName')]",
            },
            "resources": [{
                "apiVersion": "[variables('sbVersion')]",
                "name": "[parameters('serviceBusSubscriptionName')]",
                "type": "Subscriptions",
                "dependsOn": [
                    "[parameters('serviceBusTopicName')]"
                ],
                "properties": {}
            }]
        }]
    }]
```

## <a name="commands-to-run-deployment"></a>Dağıtımı çalıştırma komutları
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>PowerShell
```powershell
New-AzureResourceGroupDeployment -Name \<deployment-name\> -ResourceGroupName \<resource-group-name\> -TemplateUri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-and-subscription/azuredeploy.json>
```

## <a name="azure-cli"></a>Azure CLI
```azurecli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-and-subscription/azuredeploy.json>
```

## <a name="next-steps"></a>Sonraki adımlar
Oluşturulan ve Azure Resource Manager kullanarak kaynakları dağıtılan göre bu kaynakları Bu makaleler görüntüleyerek yönetmeyi öğrenin:

* [Service Bus PowerShell ile yönetme](service-bus-manage-with-ps.md)
* [Hizmet veri yolu Gezgini ile Service Bus kaynaklarını yönetme](https://github.com/paolosalvatori/ServiceBusExplorer/releases)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Azure Quickstart Templates]: https://azure.microsoft.com/documentation/templates/?term=service+bus
[Learn more about Service Bus topics and subscriptions]: service-bus-queues-topics-subscriptions.md
[Using Azure PowerShell with Azure Resource Manager]: ../azure-resource-manager/powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md
[Service Bus namespace with topic and subscription]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-topic-and-subscription/
