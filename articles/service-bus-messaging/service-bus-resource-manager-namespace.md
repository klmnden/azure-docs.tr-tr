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
ms.date: 01/23/2019
ms.author: spelluru
ms.openlocfilehash: 4471c9d5b6c09bcf4d9100cccfa725f36cf9a3f8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66111228"
---
# <a name="create-a-service-bus-namespace-using-an-azure-resource-manager-template"></a>Bir Azure Resource Manager şablonu kullanarak bir Service Bus ad alanı oluşturma
Bu hızlı başlangıçta, bir Service Bus ad alanı türü oluşturan bir Azure Resource Manager şablonu oluşturma **Mesajlaşma** ile bir **standart** SKU. Makale ayrıca dağıtım yürütülmesi için belirtilen parametreleri tanımlar. Bu şablonu kendi dağıtımlarınız için kullanabilir veya kendi gereksinimlerinize göre özelleştirebilirsiniz. Şablon oluşturma hakkında daha fazla bilgi için bkz. [Azure Resource Manager şablonları yazma][Authoring Azure Resource Manager templates]. Tam şablon için bkz: [Service Bus ad alanı şablon] [ Service Bus namespace template] GitHub üzerinde.

> [!NOTE]
> Aşağıdaki Azure Resource Manager şablonları, yükleme ve dağıtım için kullanılabilir. 
> 
> * [Kuyruk ile bir Service Bus ad alanı oluşturma](service-bus-resource-manager-namespace-queue.md)
> * [Konu ve abonelik ile Service Bus ad alanı oluşturma](service-bus-resource-manager-namespace-topic.md)
> * [Kuyruk ve yetkilendirme kuralı ile bir Service Bus ad alanı oluşturma](service-bus-resource-manager-namespace-auth-rule.md)
> * [Konusu, aboneliği ve kuralı ile bir Service Bus ad alanı oluşturma](service-bus-resource-manager-namespace-topic-with-rule.md)
> 
> En yeni şablonları denetlemek için ziyaret [Azure hızlı başlangıç şablonları] [ Azure Quickstart Templates] galeri ve Service Bus arayın.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="quick-deployment"></a>Hızlı Dağıtım
Herhangi bir JSON yazma ve PowerShell/CLI komutu çalıştırarak örneği olmadan çalıştırmak için aşağıdaki düğmeyi seçin:

[![Azure’a dağıtma](./media/service-bus-resource-manager-namespace/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-servicebus-create-namespace%2Fazuredeploy.json)

Oluşturma ve şablon el ile dağıtmak için aşağıdaki bölümlerde bu makaledeki inceleyin.

## <a name="prerequisites"></a>Önkoşullar
Bu hızlı başlangıcı tamamlamak bir Azure aboneliğinizin olması gerekir. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

Kullanmak istiyorsanız **Azure PowerShell** Resource Manager şablonu dağıtmak için [Azure PowerShell yükleme](https://docs.microsoft.com/powershell/azure/install-Az-ps).

Kullanmak istiyorsanız **Azure CLI** Resource Manager şablonu dağıtmak için [Azure CLI yükleme]( /cli/azure/install-azure-cli).

## <a name="create-the-resource-manager-template-json"></a>Resource Manager şablonu JSON'ı oluşturma 
Adlı bir JSON dosyası oluşturun **MyServiceBusNamespace.json** aşağıdaki içeriğe sahip: 

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "serviceBusNamespaceName": {
            "type": "string",
            "metadata": {
                "description": "Name of the Service Bus namespace"
            }
        },
        "serviceBusSku": {
            "type": "string",
            "allowedValues": [
                "Basic",
                "Standard",
                "Premium"
            ],
            "defaultValue": "Standard",
            "metadata": {
                "description": "The messaging tier for service Bus namespace"
            }
        },
        "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]",
            "metadata": {
                "description": "Location for all resources."
            }
        }
    },
    "resources": [
        {
            "apiVersion": "2017-04-01",
            "name": "[parameters('serviceBusNamespaceName')]",
            "type": "Microsoft.ServiceBus/namespaces",
            "location": "[parameters('location')]",
            "sku": {
                "name": "[parameters('serviceBusSku')]"
            }
        }
    ]
}
```

Bu şablon, standart bir Service Bus ad alanı oluşturur. JSON söz dizimi ve özellikler için bkz: [ad alanları](/azure/templates/microsoft.servicebus/namespaces) şablon başvurusu.

## <a name="create-the-parameters-json"></a>Parametreler JSON oluşturma
Önceki adımda oluşturduğunuz şablonu adlı bir bölüm vardır `Parameters`. Projenin hedef ortama bağlı veya dağıtıyorsanız göre farklılık bu değerler için parametre tanımlarsınız. Bu şablon aşağıdaki parametreleri tanımlar: **serviceBusNamespaceName**, **serviceBusSku**, ve **konumu**. Service Bus SKU'ları hakkında daha fazla bilgi için bkz: [Service Bus SKU'ları](https://azure.microsoft.com/pricing/details/service-bus/) oluşturmak için.

Adlı bir JSON dosyası oluşturun **MyServiceBusNamespace-Parameters.json** aşağıdaki içeriğe sahip: 

> [!NOTE] 
> Service Bus ad alanınız için bir ad belirtin. 


```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "serviceBusNamespaceName": {
      "value": "<Specify a name for the Service Bus namespace>"
    },
    "serviceBusSku": {
      "value": "Standard"
    },
    "location": {
        "value": "East US"
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

### <a name="deploy-resources"></a>Kaynakları dağıtma
Azure PowerShell kullanarak kaynakları dağıtmak için JSON dosyalarını kaydettiğiniz klasöre geçin ve aşağıdaki komutları çalıştırın:

> [!IMPORTANT]
> Azure kaynak grubu için bir ad, komutları çalıştırmadan önce $resourceGroupName için bir değer belirtin. 

1. Kaynak grubu adı için bir değişken tanımlayın ve bunun için bir değer belirtin. 

    ```azurepowershell
    $resourceGroupName = "<Specify a name for the Azure resource group>"
    ```
2. Bir Azure kaynak grubu oluşturun.

    ```azurepowershell
    New-AzResourceGroup $resourceGroupName -location 'East US'
    ```
3. Resource Manager şablonu dağıtın. Dağıtımın kendisi adları, kaynak grubu, şablon parametreleri için JSON dosyası için JSON dosyasını belirtin

    ```azurepowershell
    New-AzResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName $resourceGroupName -TemplateFile MyServiceBusNamespace.json -TemplateParameterFile MyServiceBusNamespace-Parameters.json
    ```

## <a name="use-azure-cli-to-deploy-the-template"></a>Şablonu dağıtmak için Azure CLI kullanma

### <a name="sign-in-to-azure"></a>Oturum açın: Azure

1. Azure'da oturum açmak için aşağıdaki komutu çalıştırın:

    ```azurecli
    az login
    ```
2. Geçerli abonelik bağlamını ayarlayın. `MyAzureSub` yerine kullanmak istediğiniz Azure aboneliğinin adını yazın:

    ```azurecli
    az account set --subscription <Name of your Azure subscription>
    ``` 

### <a name="deploy-resources"></a>Kaynakları dağıtma
Azure CLI kullanarak kaynakları dağıtmak için JSON dosyaları klasöre geçin ve aşağıdaki komutları çalıştırın:

> [!IMPORTANT]
> Komut az grubundaki Azure kaynak grubu oluşturmak için bir ad belirtin. :

1. Bir Azure kaynak grubu oluşturun. 
    ```azurecli
    az group create --name <YourResourceGroupName> --location eastus
    ```

2. Resource Manager şablonu dağıtın. Kaynak grubu, dağıtım, şablon için JSON dosyasını, parametreler için JSON dosyasının adını belirtin.

    ```azurecli
    az group deployment create --name <Specify a name for the deployment> --resource-group <YourResourceGroupName> --template-file MyServiceBusNamespace.json --parameters @MyServiceBusNamespace-Parameters.json
    ```

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Service Bus ad alanı oluşturdunuz. Bkz. kuyruklar, konular/abonelikler, oluşturma konusunda bilgi almak için diğer hızlı başlangıçlar ve bunları kullanabilirsiniz: 

- [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
- [Hizmet veri yolu konuları ile çalışmaya başlama](service-bus-dotnet-how-to-use-topics-subscriptions.md)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Service Bus namespace template]: https://github.com/Azure/azure-quickstart-templates/blob/master/101-servicebus-create-namespace/
[Azure Quickstart Templates]: https://azure.microsoft.com/documentation/templates/?term=service+bus
[Service Bus pricing and billing]: https://azure.microsoft.com/pricing/details/service-bus/
[Using Azure PowerShell with Azure Resource Manager]: ../azure-resource-manager/powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md
