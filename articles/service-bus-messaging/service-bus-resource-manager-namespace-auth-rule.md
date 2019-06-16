---
title: Azure Resource Manager şablonu kullanarak bir Service Bus yetkilendirme kuralı oluştur | Microsoft Docs
description: Bir Service Bus yetkilendirme kuralı için ad alanı ve Azure Resource Manager şablonu kullanarak kuyruk oluşturma
services: service-bus-messaging
documentationcenter: .net
author: axisc
manager: timlt
editor: spelluru
ms.assetid: 7f1443a0-5fa8-4d90-8637-1a977ef0b1f0
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 01/23/2019
ms.author: aschhab
ms.openlocfilehash: f2c82c8ff353889f06dfc1c2ff5c3f316013c54b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66171226"
---
# <a name="create-a-service-bus-authorization-rule-for-namespace-and-queue-using-an-azure-resource-manager-template"></a>Bir Service Bus yetkilendirme kuralı için ad alanı ve bir Azure Resource Manager şablonu kullanarak kuyruk oluşturma

Bu makalede oluşturan bir Azure Resource Manager şablonunun nasıl kullanılacağı gösterilmektedir. bir [yetkilendirme kuralı](service-bus-authentication-and-authorization.md#shared-access-signature-authentication) bir Service Bus ad alanı ve kuyruk. Makalede nasıl belirtmek için hangi kaynaklara dağıtılır ve parametrelerin nasıl dağıtıldığının ve dağıtım yürütülürken belirtilen açıklanmaktadır. Bu şablonu kendi dağıtımlarınız için kullanabilir veya kendi gereksinimlerinize göre özelleştirebilirsiniz.

Şablonları oluşturma hakkında daha fazla bilgi için lütfen bkz [Azure Resource Manager şablonları yazma][Authoring Azure Resource Manager templates].

Tam şablon için bkz: [Service Bus yetkilendirme kuralı şablonunu] [ Service Bus auth rule template] GitHub üzerinde.

> [!NOTE]
> Aşağıdaki Azure Resource Manager şablonları, yükleme ve dağıtım için kullanılabilir.
> 
> * [Service Bus ad alanı oluşturma](service-bus-resource-manager-namespace.md)
> * [Kuyruk ile bir Service Bus ad alanı oluşturma](service-bus-resource-manager-namespace-queue.md)
> * [Konu ve abonelik ile Service Bus ad alanı oluşturma](service-bus-resource-manager-namespace-topic.md)
> * [Konusu, aboneliği ve kuralı ile bir Service Bus ad alanı oluşturma](service-bus-resource-manager-namespace-topic-with-rule.md)
> 
> En yeni şablonları denetlemek için ziyaret [Azure hızlı başlangıç şablonları] [ Azure Quickstart Templates] galeri ve arama **Service Bus**.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="what-will-you-deploy"></a>Ne dağıtacaksınız?

Bu şablonu kullanarak bir Service Bus yetkilendirme kuralı için bir ad alanı ve mesajlaşma varlığı (Bu durumda, bir kuyruktaki) dağıtın.

Bu şablonu kullanan [paylaşılan erişim imzası (SAS)](service-bus-sas.md) kimlik doğrulaması için. SAS, Service Bus ad alanı ya da belirli haklarla ilişkili Mesajlaşma varlığı (kuyruk veya konu) yapılandırılmış bir erişim anahtarı kullanılarak kimlik doğrulaması uygulamaları etkinleştirir. Ardından, istemcilerin sırayla Service Bus kimlik doğrulaması yapmak için kullanabileceği SAS belirteci oluşturmak için bu anahtarı kullanabilirsiniz.

Dağıtımı otomatik olarak çalıştırmak için aşağıdaki düğmeye tıklayın:

[![Azure’a dağıtma](./media/service-bus-resource-manager-namespace-auth-rule/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F301-servicebus-create-authrule-namespace-and-queue%2Fazuredeploy.json)

## <a name="parameters"></a>Parametreler

Azure Resource Manager sayesinde, şablon dağıtıldığında belirtmek istediğiniz değerlerin parametrelerini siz tanımlarsınız. Adlı bir bölüm şablonu içerir `Parameters` , tüm parametre değerlerini içerir. Dağıtmakta olduğunuz projeye veya dağıtım yaptığınız ortama bağlı olarak değişir bu değerleri için bir parametre tanımlamanız gerekir. Her zaman aynı kalır değerler için parametre tanımlamayın. Her parametre değeri, dağıtılan kaynakları tanımlamak için şablonda kullanılır.

Şablon aşağıdaki parametreleri tanımlar.

### <a name="servicebusnamespacename"></a>serviceBusNamespaceName
Oluşturmak için Service Bus ad alanı adı.

```json
"serviceBusNamespaceName": {
"type": "string"
}
```

### <a name="namespaceauthorizationrulename"></a>namespaceAuthorizationRuleName
Ad alanı yetkilendirme kuralı adı.

```json
"namespaceAuthorizationRuleName ": {
"type": "string"
}
```

### <a name="servicebusqueuename"></a>serviceBusQueueName
Service Bus ad alanındaki kuyruk adı.

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
Standart bir Service Bus ad alanı türü oluşturur **Mesajlaşma**ve Service Bus yetkilendirme kuralı için ad alanı ve varlık.

```json
"resources": [
        {
            "apiVersion": "[variables('sbVersion')]",
            "name": "[parameters('serviceBusNamespaceName')]",
            "type": "Microsoft.ServiceBus/namespaces",
            "location": "[variables('location')]",
            "kind": "Messaging",
            "sku": {
                "name": "Standard",
            },
            "resources": [
                {
                    "apiVersion": "[variables('sbVersion')]",
                    "name": "[parameters('serviceBusQueueName')]",
                    "type": "Queues",
                    "dependsOn": [
                        "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
                    ],
                    "properties": {
                        "path": "[parameters('serviceBusQueueName')]"
                    },
                    "resources": [
                        {
                            "apiVersion": "[variables('sbVersion')]",
                            "name": "[parameters('queueAuthorizationRuleName')]",
                            "type": "authorizationRules",
                            "dependsOn": [
                                "[parameters('serviceBusQueueName')]"
                            ],
                            "properties": {
                                "Rights": ["Listen"]
                            }
                        }
                    ]
                }
            ]
        }, {
            "apiVersion": "[variables('sbVersion')]",
            "name": "[variables('namespaceAuthRuleName')]",
            "type": "Microsoft.ServiceBus/namespaces/authorizationRules",
            "dependsOn": ["[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"],
            "location": "[resourceGroup().location]",
            "properties": {
                "Rights": ["Send"]
            }
        }
    ]
```

JSON söz dizimi ve özellikler için bkz: [ad alanları](/azure/templates/microsoft.servicebus/namespaces), [kuyrukları](/azure/templates/microsoft.servicebus/namespaces/queues), ve [Authorizationrules'a](/azure/templates/microsoft.servicebus/namespaces/authorizationrules).

## <a name="commands-to-run-deployment"></a>Dağıtımı çalıştırma komutları
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell
```powershell
New-AzResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/301-servicebus-create-authrule-namespace-and-queue/azuredeploy.json>
```

## <a name="azure-cli"></a>Azure CLI
```azurecli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/301-servicebus-create-authrule-namespace-and-queue/azuredeploy.json>
```

## <a name="next-steps"></a>Sonraki adımlar
Oluşturulan ve dağıtılan kaynakları Azure Resource Manager kullanarak göre bu kaynakları Bu makaleler görüntüleyerek yönetmeyi öğrenin:

* [Service Bus’ı PowerShell ile yönetme](service-bus-powershell-how-to-provision.md)
* [Service Bus Explorer ile Service Bus kaynaklarını yönetme](https://github.com/paolosalvatori/ServiceBusExplorer/releases)
* [Service Bus kimlik doğrulama ve yetkilendirme](service-bus-authentication-and-authorization.md)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Azure Quickstart Templates]: https://azure.microsoft.com/documentation/templates/?term=service+bus
[Using Azure PowerShell with Azure Resource Manager]: ../azure-resource-manager/powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md
[Service Bus auth rule template]: https://github.com/Azure/azure-quickstart-templates/blob/master/301-servicebus-create-authrule-namespace-and-queue/
