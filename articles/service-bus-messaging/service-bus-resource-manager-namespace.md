---
title: Azure Resource Manager şablonu kullanarak Service Bus Mesajlaşması ad alanı oluşturma | Microsoft Docs
description: Bir Service Bus Mesajlaşması ad alanı oluşturmak için Azure Resource Manager şablonu kullanma
services: service-bus-messaging
documentationcenter: .net
author: spelluru
manager: timlt
editor: ''
ms.assetid: dc0d6482-6344-4cef-8644-d4573639f5e4
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 09/11/2018
ms.author: spelluru
ms.openlocfilehash: 6f3f44394ab11c1b66be3af976dbd1f7d23de96e
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2018
ms.locfileid: "47405793"
---
# <a name="create-a-service-bus-namespace-using-an-azure-resource-manager-template"></a>Bir Azure Resource Manager şablonu kullanarak bir Service Bus ad alanı oluşturma

Bu makalede türünde bir Service Bus ad alanı oluşturan bir Azure Resource Manager şablonunun nasıl kullanılacağı **Mesajlaşma** standart SKU ile. Makale ayrıca dağıtım yürütülmesi için belirtilen parametreleri tanımlar. Bu şablonu kendi dağıtımlarınız için kullanabilir veya kendi gereksinimlerinize göre özelleştirebilirsiniz.

Şablon oluşturma hakkında daha fazla bilgi için bkz. [Azure Resource Manager şablonları yazma][Authoring Azure Resource Manager templates].

Tam şablon için bkz: [Service Bus ad alanı şablon] [ Service Bus namespace template] GitHub üzerinde.

> [!NOTE]
> Aşağıdaki Azure Resource Manager şablonları, yükleme ve dağıtım için kullanılabilir. 
> 
> * [Kuyruk ile bir Service Bus ad alanı oluşturma](service-bus-resource-manager-namespace-queue.md)
> * [Konu ve abonelik ile Service Bus ad alanı oluşturma](service-bus-resource-manager-namespace-topic.md)
> * [Kuyruk ve yetkilendirme kuralı ile bir Service Bus ad alanı oluşturma](service-bus-resource-manager-namespace-auth-rule.md)
> * [Konusu, aboneliği ve kuralı ile bir Service Bus ad alanı oluşturma](service-bus-resource-manager-namespace-topic-with-rule.md)
> 
> En yeni şablonları denetlemek için ziyaret [Azure hızlı başlangıç şablonları] [ Azure Quickstart Templates] galeri ve Service Bus arayın.
> 
> 

## <a name="what-will-you-deploy"></a>Ne dağıtacaksınız?

Bu şablonu kullanarak bir Service Bus ad alanı ile dağıttığınız bir [standart veya Premium](https://azure.microsoft.com/pricing/details/service-bus/) SKU.

Dağıtımı otomatik olarak çalıştırmak için aşağıdaki düğmeye tıklayın:

[![Azure’a dağıtma](./media/service-bus-resource-manager-namespace/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-servicebus-create-namespace%2Fazuredeploy.json)

## <a name="parameters"></a>Parametreler

Azure Resource Manager sayesinde, şablon dağıtıldığında belirtmek istediğiniz değerlerin parametrelerini siz tanımlarsınız. Adlı bir bölüm şablonu içerir `Parameters` , tüm parametre değerlerini içerir. Dağıtmakta olduğunuz projeye veya dağıtım yaptığınız ortama göre değişen değerler için bir parametre tanımlamanız gerekir. Her zaman aynı kalan değerler için parametre tanımlamayın. Her parametre değeri, dağıtılan kaynakları tanımlamak için şablonda kullanılır.

Bu şablon aşağıdaki parametreleri tanımlar:

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

### <a name="servicebussku"></a>serviceBusSKU

Service Bus adını [SKU](https://azure.microsoft.com/pricing/details/service-bus/) oluşturmak için.

```json
"serviceBusSku": { 
    "type": "string", 
    "allowedValues": [ 
        "Standard",
        "Premium" 
    ], 
    "defaultValue": "Standard", 
    "metadata": { 
        "description": "The messaging tier for service Bus namespace" 
    } 

```

Şablon (standart veya Premium) Bu parametre için izin verilen değerleri tanımlar. Hiçbir değer belirtilmemişse, varsayılan değer (standart) Kaynak Yöneticisi'ni atar.

Service Bus fiyatlandırması hakkında daha fazla bilgi için bkz. [fiyatlandırma ve faturalama Service Bus][Service Bus pricing and billing].

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

### <a name="service-bus-namespace"></a>Service Bus ad alanı

Standart bir Service Bus ad alanı türü oluşturur **Mesajlaşma**.

```json
"resources": [
    {
        "apiVersion": "[parameters('serviceBusApiVersion')]",
        "name": "[parameters('serviceBusNamespaceName')]",
        "type": "Microsoft.ServiceBus/Namespaces",
        "location": "[variables('location')]",
        "kind": "Messaging",
        "sku": {
            "name": "Standard",
        },
        "properties": {
        }
    }
]
```

## <a name="commands-to-run-deployment"></a>Dağıtımı çalıştırma komutları

[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName <resource-group-name> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-servicebus-create-namespace/azuredeploy.json
```

### <a name="azure-cli"></a>Azure CLI

```azurecli-interactive
azure config mode arm

azure group deployment create <my-resource-group> <my-deployment-name> --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-servicebus-create-namespace/azuredeploy.json
```

## <a name="next-steps"></a>Sonraki adımlar
Oluşturulan ve dağıtılan kaynakları Azure Resource Manager kullanarak göre bu kaynakları bu makaleleri okuyarak yönetmeyi öğrenin:

* [Service Bus’ı PowerShell ile yönetme](service-bus-manage-with-ps.md)
* [Service Bus Explorer ile Service Bus kaynaklarını yönetme](https://github.com/paolosalvatori/ServiceBusExplorer/releases)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Service Bus namespace template]: https://github.com/Azure/azure-quickstart-templates/blob/master/101-servicebus-create-namespace/
[Azure Quickstart Templates]: https://azure.microsoft.com/documentation/templates/?term=service+bus
[Service Bus pricing and billing]: service-bus-pricing-billing.md
[Using Azure PowerShell with Azure Resource Manager]: ../azure-resource-manager/powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md
