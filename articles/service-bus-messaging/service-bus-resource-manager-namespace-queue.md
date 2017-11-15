---
title: "Azure Service Bus ad alanı oluşturma ve Azure Resource Manager şablonu kullanarak sıraya | Microsoft Docs"
description: "Bir hizmet veri yolu ad alanı ve Azure Resource Manager şablonu kullanarak bir sıra oluşturun"
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: a6bfb5fd-7b98-4588-8aa1-9d5f91b599b6
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 11/10/2017
ms.author: sethm;shvija
ms.openlocfilehash: 2ac9bb07a1b690681e0b263eea233cd5373d990b
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/11/2017
---
# <a name="create-a-service-bus-namespace-and-a-queue-using-an-azure-resource-manager-template"></a>Bir hizmet veri yolu ad alanı ve bir Azure Resource Manager şablonu kullanarak bir sıra oluşturun

Bu makale bir hizmet veri yolu ad alanı ve bu ad alanı içindeki bir kuyruk oluşturur bir Azure Resource Manager şablonu kullanmayı gösterir. Makalede nasıl belirtmek için hangi kaynağın dağıtılan ve ne zaman dağıtım yürütülen parametreler tanımlamak nasıl belirtilen açıklanmaktadır. Bu şablonu kendi dağıtımlarınız için kullanabilir veya kendi gereksinimlerinize göre özelleştirebilirsiniz.

Şablonları oluşturma hakkında daha fazla bilgi için lütfen bkz [Azure Resource Manager şablonları yazma][Authoring Azure Resource Manager templates].

Tam şablon için bkz: [hizmet veri yolu ad alanı ve sıra şablonu] [ Service Bus namespace and queue template] github'da.

> [!NOTE]
> Aşağıdaki Azure Resource Manager şablonları, yükleme ve dağıtım için kullanılabilir.
> 
> * [Kuyruk ve yetkilendirme kuralı ile Service Bus ad alanı oluşturma](service-bus-resource-manager-namespace-auth-rule.md)
> * [Hizmet veri yolu ad alanı konu ve abonelik oluşturma](service-bus-resource-manager-namespace-topic.md)
> * [Hizmet veri yolu ad alanı oluşturma](service-bus-resource-manager-namespace.md)
> * [Konu, abonelik ve kuralı ile Service Bus ad alanı oluşturma](service-bus-resource-manager-namespace-topic-with-rule.md)
> 
> Son şablonları denetlemek için ziyaret edin [Azure hızlı başlangıç şablonlarını] [ Azure Quickstart Templates] Galerisi ve arama **Service Bus**.
> 
> 

## <a name="what-will-you-deploy"></a>Ne dağıtacaksınız?

Bu şablon kullanılarak bir hizmet veri yolu ad alanı bir kuyruk dağıtın.

[Service Bus kuyruklarını](service-bus-queues-topics-subscriptions.md#queues) ilk giren ilk çıkar (FIFO) bir veya birden çok rakip tüketiciye ileti teslimi sunar.

Dağıtımı otomatik olarak çalıştırmak için aşağıdaki düğmeye tıklayın:

[![Azure’a dağıtma](./media/service-bus-resource-manager-namespace-queue/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-queue%2Fazuredeploy.json)

## <a name="parameters"></a>Parametreler

Azure Resource Manager sayesinde, şablon dağıtıldığında belirtmek istediğiniz değerlerin parametrelerini siz tanımlarsınız. Şablon adlı bir bölüm içerir `Parameters` , tüm parametre değerlerini içerir. Dağıttığınız projesini temel alan veya dağıttığınız ortamı dayanarak değişir bu değerleri için bir parametre tanımlamanız gerekir. Her zaman aynı kalır değerleri parametrelerini tanımlamayın. Her parametre değeri, dağıtılan kaynakları tanımlamak için şablonda kullanılır.

Şablon aşağıdaki parametreleri tanımlar.

### <a name="servicebusnamespacename"></a>serviceBusNamespaceName
Oluşturmak için hizmet veri yolu ad alanı adı.

```json
"serviceBusNamespaceName": {
"type": "string",
"metadata": { 
    "description": "Name of the Service Bus namespace" 
    }
}
```

### <a name="servicebusqueuename"></a>serviceBusQueueName
Hizmet veri yolu ad alanında oluşturduğunuz kuyruğa adı.

```json
"serviceBusQueueName": {
"type": "string"
}
```

### <a name="servicebusapiversion"></a>serviceBusApiVersion
Şablon hizmet veri yolu API'sini sürümü.

```json
"serviceBusApiVersion": { 
       "type": "string", 
       "defaultValue": "2017-04-01", 
       "metadata": { 
           "description": "Service Bus ApiVersion used by the template" 
       }
```

## <a name="resources-to-deploy"></a>Dağıtılacak kaynaklar
Standart bir hizmet veri yolu ad alanı türü oluşturur **ileti**, bir kuyruk.

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
            "name": "[parameters('serviceBusQueueName')]",
            "type": "Queues",
            "dependsOn": [
                "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
            ],
            "properties": {
                "path": "[parameters('serviceBusQueueName')]",
            }
        }]
    }]
```

## <a name="commands-to-run-deployment"></a>Dağıtımı çalıştırma komutları
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>PowerShell

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-queue/azuredeploy.json>
```

## <a name="azure-cli"></a>Azure CLI

```azurecli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-queue/azuredeploy.json>
```

## <a name="next-steps"></a>Sonraki adımlar
Oluşturulan ve Azure Resource Manager kullanarak kaynakları dağıtılan göre bu kaynakları Bu makaleler görüntüleyerek yönetmeyi öğrenin:

* [Service Bus PowerShell ile yönetme](service-bus-manage-with-ps.md)
* [Hizmet veri yolu Gezgini ile Service Bus kaynaklarını yönetme](https://github.com/paolosalvatori/ServiceBusExplorer/releases)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Service Bus namespace and queue template]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/
[Azure Quickstart Templates]: https://azure.microsoft.com/documentation/templates/?term=service+bus
[Learn more about Service Bus queues]: service-bus-queues-topics-subscriptions.md
[Using Azure PowerShell with Azure Resource Manager]: ../azure-resource-manager/powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md
