---
title: Azure Service Bus ad alanı oluşturma ve Azure Resource Manager şablonu kullanarak sıraya | Microsoft Docs
description: Service Bus ad alanı ve Azure Resource Manager şablonu kullanarak kuyruk oluşturma
services: service-bus-messaging
documentationcenter: .net
author: spelluru
manager: timlt
editor: ''
ms.assetid: a6bfb5fd-7b98-4588-8aa1-9d5f91b599b6
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 01/23/2019
ms.author: spelluru
ms.openlocfilehash: 2952c446e2d66505ebaec42eb82e37be02035483
ms.sourcegitcommit: 8313d5bf28fb32e8531cdd4a3054065fa7315bfd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2019
ms.locfileid: "59047798"
---
# <a name="create-a-service-bus-namespace-and-a-queue-using-an-azure-resource-manager-template"></a>Service Bus ad alanı ve bir Azure Resource Manager şablonu kullanarak kuyruk oluşturma

Bu makalede, Service Bus ad alanı ve bu ad alanı içinde bir kuyruk oluşturan bir Azure Resource Manager şablonunun nasıl kullanılacağı gösterilmektedir. Makalede nasıl belirtmek için hangi kaynaklara dağıtılır ve parametrelerin nasıl dağıtıldığının ve dağıtım yürütülürken belirtilen açıklanmaktadır. Bu şablonu kendi dağıtımlarınız için kullanabilir veya kendi gereksinimlerinize göre özelleştirebilirsiniz.

Şablonları oluşturma hakkında daha fazla bilgi için lütfen bkz [Azure Resource Manager şablonları yazma][Authoring Azure Resource Manager templates].

Tam şablon için bkz: [Service Bus ad alanı ve kuyruk şablon] [ Service Bus namespace and queue template] GitHub üzerinde.

> [!NOTE]
> Aşağıdaki Azure Resource Manager şablonları, yükleme ve dağıtım için kullanılabilir.
> 
> * [Kuyruk ve yetkilendirme kuralı ile bir Service Bus ad alanı oluşturma](service-bus-resource-manager-namespace-auth-rule.md)
> * [Konu ve abonelik ile Service Bus ad alanı oluşturma](service-bus-resource-manager-namespace-topic.md)
> * [Service Bus ad alanı oluşturma](service-bus-resource-manager-namespace.md)
> * [Konusu, aboneliği ve kuralı ile bir Service Bus ad alanı oluşturma](service-bus-resource-manager-namespace-topic-with-rule.md)
> 
> En yeni şablonları denetlemek için ziyaret [Azure hızlı başlangıç şablonları] [ Azure Quickstart Templates] galeri ve arama **Service Bus**.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="what-will-you-deploy"></a>Ne dağıtacaksınız?

Bu şablonu kullanarak bir Service Bus ad alanı bir kuyruk aracılığıyla dağıtın.

[Service Bus kuyruklarını](service-bus-queues-topics-subscriptions.md#queues) ilk giren ilk çıkar (FIFO) bir veya birden çok rakip tüketiciye ileti teslimi sunar.

Dağıtımı otomatik olarak çalıştırmak için aşağıdaki düğmeye tıklayın:

[![Dazure'a aşamasıyla](./media/service-bus-resource-manager-namespace-queue/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-queue%2Fazuredeploy.json)

## <a name="parameters"></a>Parametreler

Azure Resource Manager sayesinde, şablon dağıtıldığında belirtmek istediğiniz değerlerin parametrelerini siz tanımlarsınız. Adlı bir bölüm şablonu içerir `Parameters` , tüm parametre değerlerini içerir. Dağıtmakta olduğunuz projeye veya dağıtım yaptığınız ortama bağlı olarak değişir bu değerleri için bir parametre tanımlamanız gerekir. Her zaman aynı kalır değerler için parametre tanımlamayın. Her parametre değeri, dağıtılan kaynakları tanımlamak için şablonda kullanılır.

Şablon aşağıdaki parametreleri tanımlar.

### <a name="servicebusnamespacename"></a>serviceBusNamespaceName
Oluşturmak için Service Bus ad alanı adı.

```json
"serviceBusNamespaceName": {
"type": "string",
"metadata": { 
    "description": "Name of the Service Bus namespace" 
    }
}
```

### <a name="servicebusqueuename"></a>serviceBusQueueName
Service Bus ad alanında oluşturulan Kuyruğun adı.

```json
"serviceBusQueueName": {
"type": "string"
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
Standart bir Service Bus ad alanı türü oluşturur **Mesajlaşma**, bir kuyruk aracılığıyla.

```json
"resources ": [{
        "apiVersion": "[variables('sbVersion')]",
        "name": "[parameters('serviceBusNamespaceName')]",
        "type": "Microsoft.ServiceBus/Namespaces",
        "location": "[variables('location')]",
        "kind": "Messaging",
        "sku": {
            "name": "Standard",
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

JSON söz dizimi ve özellikler için bkz: [ad alanları](/azure/templates/microsoft.servicebus/namespaces) ve [kuyrukları](/azure/templates/microsoft.servicebus/namespaces/queues).

## <a name="commands-to-run-deployment"></a>Dağıtımı çalıştırma komutları
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>PowerShell

```powershell
New-AzResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-queue/azuredeploy.json>
```

## <a name="azure-cli"></a>Azure CLI

```azurecli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-queue/azuredeploy.json>
```

## <a name="next-steps"></a>Sonraki adımlar
Oluşturulan ve dağıtılan kaynakları Azure Resource Manager kullanarak göre bu kaynakları Bu makaleler görüntüleyerek yönetmeyi öğrenin:

* [Service Bus’ı PowerShell ile yönetme](service-bus-manage-with-ps.md)
* [Hizmet Veri Yolu Gezgini ile Hizmet Veri Yolu kaynaklarını yönetme](https://github.com/paolosalvatori/ServiceBusExplorer/releases)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Service Bus namespace and queue template]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/
[Azure Quickstart Templates]: https://azure.microsoft.com/documentation/templates/?term=service+bus
[Learn more about Service Bus queues]: service-bus-queues-topics-subscriptions.md
[Using Azure PowerShell with Azure Resource Manager]: ../azure-resource-manager/powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md
