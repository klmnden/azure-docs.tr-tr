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
ms.date: 06/21/2019
ms.author: spelluru
ms.openlocfilehash: 4162775153a48dc8ea28e06f7c99f9927b9c602a
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67444767"
---
# <a name="create-a-service-bus-namespace-by-using-an-azure-resource-manager-template"></a>Bir Azure Resource Manager şablonu kullanarak bir Service Bus ad alanı oluşturma

Service Bus ad alanı oluşturmak için bir Azure Resource Manager şablonu dağıtmayı öğrenin. Bu şablonu kendi dağıtımlarınız için kullanabilir veya kendi gereksinimlerinize göre özelleştirebilirsiniz. Şablonları oluşturma hakkında daha fazla bilgi için bkz. [Azure Resource Manager belgelerini](/azure/azure-resource-manager/).

Aşağıdaki şablonlar, Service Bus ad alanı oluşturmak için kullanılabilir:

* [Kuyruk ile bir Service Bus ad alanı oluşturma](./service-bus-resource-manager-namespace-queue.md)
* [Konu ve abonelik ile Service Bus ad alanı oluşturma](./service-bus-resource-manager-namespace-topic.md)
* [Kuyruk ve yetkilendirme kuralı ile bir Service Bus ad alanı oluşturma](./service-bus-resource-manager-namespace-auth-rule.md)
* [Konusu, aboneliği ve kuralı ile bir Service Bus ad alanı oluşturma](./service-bus-resource-manager-namespace-topic-with-rule.md)

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="create-a-service-bus-namespace"></a>Bir service bus ad alanı oluşturma

Bu hızlı başlangıçta, kullandığınız bir [mevcut Resource Manager şablonu](https://github.com/Azure/azure-quickstart-templates/blob/master/101-servicebus-create-namespace/azuredeploy.json) gelen [Azure hızlı başlangıç şablonları](https://azure.microsoft.com/resources/templates/):

[!code-json[create-azure-service-bus-namespace](~/quickstart-templates/101-servicebus-create-namespace/azuredeploy.json)]

Daha fazla şablon örnekleri bulmak için bkz: [Azure hızlı başlangıç şablonları](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Servicebus&pageNumber=1&sort=Popular).

Bir şablonu dağıtarak bir service bus ad alanı oluşturmak için:

1. Seçin **deneyin** gelen aşağıdaki kod bloğunu ve ardından Azure bulut kabuğunda oturum açmak için yönergeleri izleyin.

    ```azurepowershell-interactive
    $serviceBusNamespaceName = Read-Host -Prompt "Enter a name for the service bus namespace to be created"
    $location = Read-Host -Prompt "Enter the location (i.e. centralus)"
    $resourceGroupName = "${serviceBusNamespaceName}rg"
    $templateUri = "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-servicebus-create-namespace/azuredeploy.json"

    New-AzResourceGroup -Name $resourceGroupName -Location $location
    New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri $templateUri -serviceBusNamespaceName $serviceBusNamespaceName

    Write-Host "Press [ENTER] to continue ..."
    ```

    Service bus ad alanı adı ile kaynak grubu adı olan **rg** eklenir.

2. Seçin **kopyalama** PowerShell betiğini kopyalanacak.
3. Kabuk konsolun sağ tıklayın ve ardından **Yapıştır**.

Bir olay hub'ı oluşturmak için birkaç dakika sürer.

## <a name="verify-the-deployment"></a>Dağıtımı doğrulama

Dağıtılan hizmet veri yolu ad alanı görmek için kaynak grubunu Azure portalından açın veya aşağıdaki Azure PowerShell Betiği kullanabilirsiniz. Cloud shell hala açık değilse, aşağıdaki komut dosyası birinci ve ikinci satırlarını kopyala/çalıştırma gerekmez.

```azurepowershell-interactive
$serviceBusNamespaceName = Read-Host -Prompt "Enter the same service bus namespace name used earlier"
$resourceGroupName = "${serviceBusNamespaceName}rg"

Get-AzServiceBusNamespace -ResourceGroupName $resourceGroupName -Name $serviceBusNamespaceName

Write-Host "Press [ENTER] to continue ..."
```

Azure PowerShell, bu öğreticide şablonu dağıtmak için kullanılır. Diğer şablon dağıtım yöntemleri için bkz:

* [Azure portalını kullanarak](../azure-resource-manager/resource-group-template-deploy-portal.md).
* [Azure CLI kullanarak](../azure-resource-manager/resource-group-template-deploy-cli.md).
* [REST API'yi kullanarak](../azure-resource-manager/resource-group-template-deploy-rest.md).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık Azure kaynakları gerekli değilse, kaynak grubunu silerek dağıttığınız kaynakları temizleyin. Cloud shell hala açık değilse, aşağıdaki komut dosyası birinci ve ikinci satırlarını kopyala/çalıştırma gerekmez.

```azurepowershell-interactive
$serviceBusNamespaceName = Read-Host -Prompt "Enter the same service bus namespace name used earlier"
$resourceGroupName = "${serviceBusNamespaceName}rg"

Remove-AzResourceGroup -ResourceGroupName $resourceGroupName

Write-Host "Press [ENTER] to continue ..."
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Service Bus ad alanı oluşturdunuz. Bkz. kuyruklar, konular/abonelikler, oluşturma konusunda bilgi almak için diğer hızlı başlangıçlar ve bunları kullanabilirsiniz:

* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Hizmet veri yolu konuları ile çalışmaya başlama](service-bus-dotnet-how-to-use-topics-subscriptions.md)
