---
title: "Azure Resource Manager şablonu kullanarak Service Bus yetkilendirme kuralını | Microsoft Docs"
description: "Ad alanı ve Azure Resource Manager şablonu kullanarak sıra için bir hizmet veri yolu yetkilendirme kuralı oluştur"
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 7f1443a0-5fa8-4d90-8637-1a977ef0b1f0
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 08/07/2017
ms.author: sethm;shvija
ms.openlocfilehash: fbd2372829a1aefa2c080c0a8a72b9ff4375b16f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-service-bus-authorization-rule-for-namespace-and-queue-using-an-azure-resource-manager-template"></a>Ad alanı ve bir Azure Resource Manager şablonu kullanarak sıra için bir hizmet veri yolu yetkilendirme kuralı oluştur

Bu makalede oluşturan bir Azure Resource Manager şablonu kullanmayı gösterir bir [yetkilendirme kuralı](service-bus-authentication-and-authorization.md#shared-access-signature-authentication) için bir hizmet veri yolu ad alanı ve sıra. Nasıl tanımlamak için hangi kaynağın dağıtılan ve ne zaman dağıtım yürütülen parametreler tanımlamak nasıl belirtilen öğreneceksiniz. Bu şablonu kendi dağıtımlarınız için kullanabilir veya kendi gereksinimlerinize göre özelleştirebilirsiniz.

Şablonları oluşturma hakkında daha fazla bilgi için lütfen bkz [Azure Resource Manager şablonları yazma][Authoring Azure Resource Manager templates].

Tam şablon için bkz: [Service Bus yetkilendirme kuralı şablonunu] [ Service Bus auth rule template] github'da.

> [!NOTE]
> Aşağıdaki Azure Resource Manager şablonları, yükleme ve dağıtım için kullanılabilir.
> 
> * [Hizmet veri yolu ad alanı oluşturma](service-bus-resource-manager-namespace.md)
> * [Sıra ile Service Bus ad alanı oluşturma](service-bus-resource-manager-namespace-queue.md)
> * [Hizmet veri yolu ad alanı konu ve abonelik oluşturma](service-bus-resource-manager-namespace-topic.md)
> * [Konu, abonelik ve kuralı ile Service Bus ad alanı oluşturma](service-bus-resource-manager-namespace-topic-with-rule.md)
> 
> Son şablonları denetlemek için ziyaret edin [Azure hızlı başlangıç şablonlarını] [ Azure Quickstart Templates] Galerisi ve "Service Bus" için arama
> 
> 

## <a name="what-will-you-deploy"></a>Ne dağıtacaksınız?
Bu şablon kullanılarak bir hizmet veri yolu yetkilendirme kuralı için bir ad alanı ve mesajlaşma varlığıyla (Bu durumda, bir kuyruktaki) dağıtır.

Bu şablonu kullanan [paylaşılan erişim imzası (SAS)](service-bus-sas.md) kimlik doğrulaması için. SAS hizmet veri yolu ad alanı veya belirli haklar ilişkili Mesajlaşma varlığıyla (kuyruk veya konu) yapılandırılmış bir erişim anahtarı kullanarak kimlik doğrulaması uygulamaları etkinleştirir. Bu anahtar ardından istemcilerin sırayla Service Bus kimlik doğrulaması yapmak için kullanabileceği bir SAS belirteci oluşturmak için de kullanabilirsiniz.

Dağıtımı otomatik olarak çalıştırmak için aşağıdaki düğmeye tıklayın:

[![Azure’a dağıtma](./media/service-bus-resource-manager-namespace-auth-rule/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F301-servicebus-create-authrule-namespace-and-queue%2Fazuredeploy.json)

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

### <a name="namespaceauthorizationrulename"></a>namespaceAuthorizationRuleName
Yetkilendirme kuralı adı ad.

```json
"namespaceAuthorizationRuleName ": {
"type": "string"
}
```

### <a name="servicebusqueuename"></a>serviceBusQueueName
Hizmet veri yolu ad alanında kuyruk adı.

```json
"serviceBusQueueName": {
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
Standart bir hizmet veri yolu ad alanı türü oluşturur **ileti**ve ad alanı ve varlık için bir hizmet veri yolu yetkilendirme kuralı.

```json
"resources": [
        {
            "apiVersion": "[variables('sbVersion')]",
            "name": "[parameters('serviceBusNamespaceName')]",
            "type": "Microsoft.ServiceBus/namespaces",
            "location": "[variables('location')]",
            "kind": "Messaging",
            "sku": {
                "name": "StandardSku",
                "tier": "Standard"
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

## <a name="commands-to-run-deployment"></a>Dağıtımı çalıştırma komutları
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell
```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/301-servicebus-create-authrule-namespace-and-queue/azuredeploy.json>
```

## <a name="azure-cli"></a>Azure CLI
```azurecli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/301-servicebus-create-authrule-namespace-and-queue/azuredeploy.json>
```

## <a name="next-steps"></a>Sonraki adımlar
Oluşturulan ve Azure Resource Manager kullanarak kaynakları dağıtılan göre bu kaynakları Bu makaleler görüntüleyerek yönetmeyi öğrenin:

* [Service Bus PowerShell ile yönetme](service-bus-powershell-how-to-provision.md)
* [Hizmet veri yolu Gezgini ile Service Bus kaynaklarını yönetme](https://github.com/paolosalvatori/ServiceBusExplorer/releases)
* [Hizmet veri yolu kimlik doğrulama ve yetkilendirme](service-bus-authentication-and-authorization.md)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Azure Quickstart Templates]: https://azure.microsoft.com/documentation/templates/?term=service+bus
[Using Azure PowerShell with Azure Resource Manager]: ../azure-resource-manager/powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md
[Service Bus auth rule template]: https://github.com/Azure/azure-quickstart-templates/blob/master/301-servicebus-create-authrule-namespace-and-queue/
