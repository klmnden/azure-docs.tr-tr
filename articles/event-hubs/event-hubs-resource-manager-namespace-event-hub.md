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
ms.date: 06/14/2019
ms.author: shvija
ms.openlocfilehash: 007e016672f8548956b37b961805183a504d6bf0
ms.sourcegitcommit: 1289f956f897786090166982a8b66f708c9deea1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67154078"
---
# <a name="quickstart-create-an-event-hub-by-using-an-azure-resource-manager-template"></a>Hızlı Başlangıç: Bir Azure Resource Manager şablonu kullanarak bir olay hub'ı oluşturma

Azure Event Hubs saniyede milyonlarca olay alıp işleme kapasitesine sahip olan bir Büyük Veri akış platformu ve olay alma hizmetidir. Event Hubs dağıtılan yazılımlar ve cihazlar tarafından oluşturulan olayları, verileri ve telemetrileri işleyebilir ve depolayabilir. Bir olay hub’ına gönderilen veriler, herhangi bir gerçek zamanlı analiz sağlayıcısı ve işlem grubu oluşturma/depolama bağdaştırıcıları kullanılarak dönüştürülüp depolanabilir. Olay Hub’larının ayrıntılı genel bakışı için bkz. [Olay Hub’larına genel bakış](event-hubs-about.md) ve [Olay Hub’ları özellikleri](event-hubs-features.md).

Bu hızlı başlangıçta, bir olay hub'ı kullanarak oluşturduğunuz bir [Azure Resource Manager şablonu](../azure-resource-manager/resource-group-overview.md). Türünde bir ad alanı oluşturmak için bir Azure Resource Manager şablonu dağıtma [Event Hubs](event-hubs-what-is-event-hubs.md), bir olay hub'ı ile. Makalede nasıl tanımlamak için hangi kaynaklara dağıtılır ve parametrelerin nasıl dağıtıldığının ve dağıtım yürütülürken belirtilen gösterilmektedir. Bu şablonu kendi dağıtımlarınız için kullanabilir veya kendi gereksinimlerinize göre özelleştirebilirsiniz. Şablonları oluşturma hakkında daha fazla bilgi için bkz: [Azure Resource Manager şablonları yazma][Authoring Azure Resource Manager templates]. JSON söz dizimi ve bir şablonunda kullanmak için özellikler için bkz: [Microsoft.EventHub kaynak türleri](/azure/templates/microsoft.eventhub/allversions).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="create-an-event-hub"></a>Olay hub’ı oluşturma

Bu hızlı başlangıçta, kullandığınız bir [mevcut Resource Manager şablonu](https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/101-eventhubs-create-namespace-and-eventhub/azuredeploy.json). Daha fazla şablon örnekleri bulmak için bkz: [Azure hızlı başlangıç şablonları](https://azure.microsoft.com/resources/templates/?term=eventhub&pageNumber=1&sort=Popular).

Şablonu dağıtmak için:

1. Seçin **deneyin** gelen aşağıdaki kod bloğunu ve ardından Azure bulut kabuğunda oturum açmak için yönergeleri izleyin.

   ```azurepowershell-interactive
   $projectName = Read-Host -Prompt "Enter a project name that is used for generating resource names"
   $location = Read-Host -Prompt "Enter the location (i.e. centralus)"
   $resourceGroupName = "${projectName}rg"
   $templateUri = "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/101-eventhubs-create-namespace-and-eventhub/azuredeploy.json"

   New-AzResourceGroup -Name $resourceGroupName -Location $location
   New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri $templateUri -projectName $projectName

   Write-Host "Press [ENTER] to continue ..."
   ```

   Bir olay hub'ı oluşturmak için birkaç dakika sürer.

1. Seçin **kopyalama** PowerShell betiğini kopyalanacak.
1. Kabuk konsolun sağ tıklayın ve ardından **Yapıştır**.

## <a name="verify-the-deployment"></a>Dağıtımı doğrulama

Dağıtımı doğrulamak ya da kaynak grubundan açabilirsiniz [Azure portalında](https://portal.azure.com), veya aşağıdaki Azure PowerShell betiğini kullanın.  Cloud shell hala açık değilse, ilk satırın (Read-Host) kopyalama/çalıştırma gerekmez.

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter the same project name that you used in the last procedure"
$resourceGroupName = "${projectName}rg"
$namespaceName = "${projectName}ns"

Get-AzEventHub -ResourceGroupName $resourceGroupName -Namespace $namespaceName

Write-Host "Press [ENTER] to continue ..."
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık Azure kaynakları gerekli değilse, kaynak grubunu silerek dağıttığınız kaynakları temizleyin. Cloud shell hala açık değilse, ilk satırın (Read-Host) kopyalama/çalıştırma gerekmez.

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter the same project name that you used in the last procedure"
$resourceGroupName = "${projectName}rg"

Remove-AzResourceGroup -ResourceGroupName $resourceGroupName

Write-Host "Press [ENTER] to continue ..."
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir Event Hubs ad alanı ve bir olay hub'ında ad alanı oluşturdunuz. Olayları gönderme (veya) bir olay hub'ından olay alma hakkında adım adım yönergeler için bkz **olayları alıp göndermek** öğreticiler:

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
