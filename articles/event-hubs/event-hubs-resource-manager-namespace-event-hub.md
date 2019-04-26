---
title: Tüketici grubu - Azure Event Hubs bir olay hub'ı oluşturun | Microsoft Docs
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
ms.openlocfilehash: d5dc65dc225d11a996d9b9d3c329151a17321fb6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60343503"
---
# <a name="quickstart-create-an-event-hub-using-azure-resource-manager-template"></a>Hızlı Başlangıç: Azure Resource Manager şablonu kullanarak bir olay hub'ı oluşturma
Azure Event Hubs saniyede milyonlarca olay alıp işleme kapasitesine sahip olan bir Büyük Veri akış platformu ve olay alma hizmetidir. Event Hubs dağıtılan yazılımlar ve cihazlar tarafından oluşturulan olayları, verileri ve telemetrileri işleyebilir ve depolayabilir. Bir olay hub’ına gönderilen veriler, herhangi bir gerçek zamanlı analiz sağlayıcısı ve işlem grubu oluşturma/depolama bağdaştırıcıları kullanılarak dönüştürülüp depolanabilir. Olay Hub’larının ayrıntılı genel bakışı için bkz. [Olay Hub’larına genel bakış](event-hubs-about.md) ve [Olay Hub’ları özellikleri](event-hubs-features.md).

Bu hızlı başlangıçta, bir Azure Resource Manager şablonu kullanarak bir olay hub'ı oluşturun. Bir Azure Resource Manager şablonu türünde bir ad alanı oluşturmak için kullandığınız [Event Hubs](event-hubs-what-is-event-hubs.md)bir olay hub'ı ve tek bir tüketici grubu. Makalede nasıl tanımlamak için hangi kaynaklara dağıtılır ve parametrelerin nasıl dağıtıldığının ve dağıtım yürütülürken belirtilen gösterilmektedir. Bu şablonu kendi dağıtımlarınız için kullanabilir veya kendi gereksinimlerinize göre özelleştirebilirsiniz. Şablonları oluşturma hakkında daha fazla bilgi için bkz: [Azure Resource Manager şablonları yazma][Authoring Azure Resource Manager templates]. JSON söz dizimi ve bir şablonunda kullanmak için özellikler için bkz: [Microsoft.EventHub kaynak türleri](/azure/templates/microsoft.eventhub/allversions).

> [!NOTE]
> Tam şablon için bkz: [olay hub'ı ve tüketici grubu şablonunu] [ Event Hub and consumer group template] GitHub üzerinde. Bu şablon, bir olay hub'ı ad alanı ve bir olay hub'ı ek bir tüketici grubu oluşturuldu. En yeni şablonları denetlemek için [Azure Hızlı Başlangıç Şablonları][Azure Quickstart Templates] galerisini ziyaret edin ve Event Hubs araması yapın.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu hızlı başlangıcı tamamlamak bir Azure aboneliğinizin olması gerekir. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

Kullanmak istiyorsanız **Azure PowerShell** Resource Manager şablonu dağıtmak için [Azure PowerShell yükleme](https://docs.microsoft.com/powershell/azure/install-az-ps).

Kullanmak istiyorsanız **Azure CLI** Resource Manager şablonu dağıtmak için [Azure CLI yükleme]( /cli/azure/install-azure-cli).

## <a name="create-the-resource-manager-template-json"></a>Resource Manager şablonu JSON'ı oluşturma
Aşağıdaki içerikle MyEventHub.json adlı bir JSON dosyası oluşturun ve bir klasöre kaydedin (örneğin: C:\EventHubsQuickstarts\ResourceManagerTemplate).

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "eventhub-namespace-name": {
            "type": "String"
        },
        "eventhub_name": {
            "type": "String"
        }
    },
    "resources": [
        {
            "type": "Microsoft.EventHub/namespaces",
            "sku": {
                "name": "Standard",
                "tier": "Standard",
                "capacity": 1
            },
            "name": "[parameters('eventhub-namespace-name')]",
            "apiVersion": "2017-04-01",
            "location": "East US",
            "tags": {},
            "scale": null,
            "properties": {
                "isAutoInflateEnabled": false,
                "maximumThroughputUnits": 0
            },
            "dependsOn": []
        },
        {
            "type": "Microsoft.EventHub/namespaces/eventhubs",
            "name": "[concat(parameters('eventhub-namespace-name'), '/', parameters('eventhub_name'))]",
            "apiVersion": "2017-04-01",
            "location": "East US",
            "scale": null,
            "properties": {
                "messageRetentionInDays": 7,
                "partitionCount": 1,
                "status": "Active"
            },
            "dependsOn": [
                "[resourceId('Microsoft.EventHub/namespaces', parameters('eventhub-namespace-name'))]"
            ]
        }
    ]
}
```

## <a name="create-the-parameters-json"></a>Parametreler JSON oluşturma
Azure Resource Manager şablonuna yönelik parametreleri içeren MyEventHub-Parameters.json adlı bir JSON dosyası oluşturun. 

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
        "eventhub-namespace-name": {
            "value": "<specify a name for the event hub namespace>"
        },
        "eventhub_name": {
            "value": "<Specify a name for the event hub in the namespace>"
        }
  }
}
```



## <a name="use-azure-powershell-to-deploy-the-template"></a>Şablonu dağıtmak için Azure PowerShell'i kullanma

### <a name="sign-in-to-azure"></a>Azure'da oturum açma
1. Azure PowerShell'i başlatın

2. Azure'da oturum açmak için aşağıdaki komutu çalıştırın:

   ```azurepowershell
   Login-AzAccount
   ```
3. Varsa, geçerli abonelik bağlamını ayarlamak için aşağıdaki komutları yürütün:

   ```azurepowershell
   Select-AzSubscription -SubscriptionName "<YourSubscriptionName>" 
   ```

### <a name="provision-resources"></a>Kaynak sağlama
Azure PowerShell kullanarak kaynakları dağıtma/sağlamak için aşağıdaki komutları çalıştırın C:\EventHubsQuickStart\ARM\ klasöre geçin:

> [!IMPORTANT]
> Azure kaynak grubu için bir ad, komutları çalıştırmadan önce $resourceGroupName için bir değer belirtin. 

```azurepowershell
$resourceGroupName = "<Specify a name for the Azure resource group>"

# Create an Azure resource group
New-AzResourceGroup $resourceGroupName -location 'East US'

# Deploy the Resource Manager template. Specify the names of deployment itself, resource group, JSON file for the template, JSON file for parameters
New-AzResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName $resourceGroupName -TemplateFile MyEventHub.json -TemplateParameterFile MyEventHub-Parameters.json
```

## <a name="use-azure-cli-to-deploy-the-template"></a>Şablonu dağıtmak için Azure CLI kullanma

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Komutları Cloud Shell'de çalıştırıyorsanız aşağıdaki adımları atlayabilirsiniz. CLI'yı yerel ortamda çalıştırıyorsanız Azure'da oturum açmak ve geçerli aboneliğinizi ayarlamak için aşağıdaki adımları uygulayın:

Azure'da oturum açmak için aşağıdaki komutu çalıştırın:

```azurecli
az login
```

Geçerli abonelik bağlamını ayarlayın. `MyAzureSub` yerine kullanmak istediğiniz Azure aboneliğinin adını yazın:

```azurecli
az account set --subscription <Name of your Azure subscription>
``` 

### <a name="provision-resources"></a>Kaynak sağlama
Azure CLI kullanarak kaynakları dağıtmak için C:\EventHubsQuickStart\ARM\ klasöre geçin ve aşağıdaki komutları çalıştırın:

> [!IMPORTANT]
> Komut az grubundaki Azure kaynak grubu oluşturmak için bir ad belirtin. .

```azurecli
# Create an Azure resource group
az group create --name <YourResourceGroupName> --location eastus

# # Deploy the Resource Manager template. Specify the names of resource group, deployment, JSON file for the template, JSON file for parameters
az group deployment create --name <Specify a name for the deployment> --resource-group <YourResourceGroupName> --template-file MyEventHub.json --parameters @MyEventHub-Parameters.json
```

Tebrikler! Bir Event Hubs ad alanı ve bu ad alanı içinde bir olay hub'ı oluşturmak için Azure Resource Manager Şablonu'ı kullandınız.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Event Hubs ad alanını oluşturdunuz ve olay hub'ınızdan olay gönderip almak için örnek uygulamaları kullandınız. Olayları gönderme (veya) bir olay hub'ından olay alma hakkında adım adım yönergeler için bkz **olayları alıp göndermek** öğreticiler: 

- [.NET Core](event-hubs-dotnet-standard-getstarted-send.md)
- [.NET Framework](event-hubs-dotnet-framework-getstarted-send.md)
- [Java](event-hubs-java-get-started-send.md)
- [Python](event-hubs-python-get-started-send.md)
- [Node.js](event-hubs-node-get-started-send.md)
- [Go](event-hubs-go-get-started-send.md)
- [C (yalnızca gönderme)](event-hubs-c-getstarted-send.md)
- [Apache Storm (yalnızca reecive)](event-hubs-storm-getstarted-receive.md)


[3]: ./media/event-hubs-quickstart-powershell/sender1.png
[4]: ./media/event-hubs-quickstart-powershell/receiver1.png
[5]: ./media/event-hubs-quickstart-powershell/metrics.png

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Azure Quickstart Templates]:  https://azure.microsoft.com/documentation/templates/?term=event+hubs
[Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
[Event hub and consumer group template]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-event-hubs-create-event-hub-and-consumer-group/
