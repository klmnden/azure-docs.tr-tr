---
title: "Azure Event Hubs kaynakları yönetmek için PowerShell kullanma | Microsoft Docs"
description: "PowerShell modülü oluşturmak ve olay hub'ları yönetmek için kullanın"
services: event-hubs
documentationcenter: .NET
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/15/2017
ms.author: sethm
ms.openlocfilehash: 2b49c01153b1104612e6ebf9c88566fc40d1f635
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="use-powershell-to-manage-event-hubs-resources"></a>Olay hub'ları kaynakları yönetmek için PowerShell kullanma

Microsoft Azure PowerShell denetlemek ve dağıtımını ve Azure Hizmetleri yönetimini otomatikleştirmek için kullanabileceğiniz bir komut dosyası ortamıdır. Bu makalede nasıl kullanılacağını açıklar [olay hub'ları Resource Manager PowerShell Modülü](/powershell/module/azurerm.eventhub) sağlamak ve Event Hubs varlıkları (ad, tek tek olay hub'ları ve tüketici grupları) yönetmek için yerel bir Azure PowerShell konsolunu kullanarak veya komut dosyası.

Olay hub'ları kaynakları Azure Resource Manager şablonları kullanarak da yönetebilirsiniz. Daha fazla bilgi için bkz: [bir Azure Resource Manager şablonu kullanarak olay hub'ı ve tüketici grubu ile bir olay hub'ları ad alanı oluşturma](event-hubs-resource-manager-namespace-event-hub.md).

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce aşağıdakiler gerekir:

* Azure aboneliği. Bir aboneliği edinme hakkında daha fazla bilgi için bkz: [satın alma seçeneği][purchase options], [üye teklifleri][member offers], veya [boş Hesap][free account].
* Azure PowerShell ile bir bilgisayar. Yönergeler için bkz: [Azure PowerShell cmdlet'leri kullanmaya başlama](/powershell/azure/get-started-azureps).
* PowerShell komut dosyaları, NuGet paketlerini ve .NET Framework genel anlama.

## <a name="get-started"></a>başlarken

İlk adım, Azure hesabınızı ve Azure abonelik oturum açmak için PowerShell kullanmaktır. ' Ndaki yönergeleri izleyin [Azure PowerShell cmdlet'leri kullanmaya başlama](/powershell/azure/get-started-azureps) Azure hesabınızda oturum açmak için daha sonra almak ve Azure aboneliğinizde kaynaklara erişim.

## <a name="provision-an-event-hubs-namespace"></a>Bir olay hub'ları ad alanı sağlama

Olay hub'ları ad alanları ile çalışırken, kullanabileceğiniz [Get-AzureRmEventHubNamespace](/powershell/module/azurerm.eventhub/get-azurermeventhubnamespace), [yeni AzureRmEventHubNamespace](/powershell/module/azurerm.eventhub/new-azurermeventhubnamespace), [Kaldır AzureRmEventHubNamespace](/powershell/module/azurerm.eventhub/remove-azurermeventhubnamespace), ve [kümesi AzureRmEventHubNamespace](/powershell/module/azurerm.eventhub/set-azurermeventhubnamespace) cmdlet'leri.

Bu örnek komut dosyasında birkaç yerel değişkenler oluşturur; `$Namespace` ve `$Location`.

* `$Namespace`Biz birlikte çalışmak istediğiniz olay hub'ları ad alanının adıdır.
* `$Location`hangi veri merkezinde tanımlayan ad sağlamak.
* `$CurrentNamespace`Biz alınamıyor (veya oluşturma) başvuru ad alanını depolar.

Gerçek bir betik içinde `$Namespace` ve `$Location` parametre olarak geçirilebilir.

Bu komut dosyasının parçası şunları yapar:

1. Belirtilen ada sahip bir olay hub'ları ad almaya çalışır.
2. Ad alanı bulunursa, bulunanları bildirir.
3. Ad alanı bulunmazsa, ad alanı oluşturur ve yeni oluşturulan ad alanı alır.

    ```powershell
    # Query to see if the namespace currently exists
    $CurrentNamespace = Get-AzureRMEventHubNamespace -ResourceGroupName $ResGrpName -NamespaceName $Namespace
   
    # Check if the namespace already exists or needs to be created
    if ($CurrentNamespace)
    {
        Write-Host "The namespace $Namespace already exists in the $Location region:"
        # Report what was found
        Get-AzureRMEventHubNamespace -ResourceGroupName $ResGrpName -NamespaceName $Namespace
    }
    else
    {
        Write-Host "The $Namespace namespace does not exist."
        Write-Host "Creating the $Namespace namespace in the $Location region..."
        New-AzureRmEventHubNamespace -ResourceGroupName $ResGrpName -NamespaceName $Namespace -Location $Location
        $CurrentNamespace = Get-AzureRMEventHubNamespace -ResourceGroupName $ResGrpName -NamespaceName $Namespace
        Write-Host "The $Namespace namespace in Resource Group $ResGrpName in the $Location region has been successfully created."
    }
    ```

## <a name="create-an-event-hub"></a>Olay hub’ı oluşturma

Bir olay hub'ı oluşturmak için önceki bölümde komut dosyası kullanarak bir ad alanı denetimi gerçekleştirin. Ardından, [yeni AzureRmEventHub](/powershell/module/azurerm.eventhub/new-azurermeventhub) olay hub'ı oluşturmak için cmdlet'i:

```powershell
# Check if event hub already exists
$CurrentEH = Get-AzureRMEventHub -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName

if($CurrentEH)
{
    Write-Host "The event hub $EventHubName already exists in the $Location region:"
    # Report what was found
    Get-AzureRmEventHub -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName
}
else
{
    Write-Host "The $EventHubName event hub does not exist."
    Write-Host "Creating the $EventHubName event hub in the $Location region..."
    New-AzureRmEventHub -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -Location $Location -MessageRetentionInDays 3
    $CurrentEH = Get-AzureRmEventHub -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName
    Write-Host "The $EventHubName event hub in Resource Group $ResGrpName in the $Location region has been successfully created."
}
```

### <a name="create-a-consumer-group"></a>Bir tüketici grubu oluştur

Bir event hub içinde bir tüketici grubu oluşturma, ad alanı ve olay hub'ı gerçekleştirmek için önceki bölümde komut dosyalarını kullanarak denetler. Ardından, [yeni AzureRmEventHubConsumerGroup](/powershell/module/azurerm.eventhub/new-azurermeventhubconsumergroup) event hub tüketici grubu oluşturmak için cmdlet'i. Örneğin:

```powershell
# Check if consumer group already exists
$CurrentCG = Get-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -ConsumerGroupName $ConsumerGroupName -ErrorAction Ignore

if($CurrentCG)
{
    Write-Host "The consumer group $ConsumerGroupName in event hub $EventHubName already exists in the $Location region:"
    # Report what was found
    Get-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName
}
else
{
    Write-Host "The $ConsumerGroupName consumer group does not exist."
    Write-Host "Creating the $ConsumerGroupName consumer group in the $Location region..."
    New-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -ConsumerGroupName $ConsumerGroupName
    $CurrentCG = Get-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName
    Write-Host "The $ConsumerGroupName consumer group in event hub $EventHubName in Resource Group $ResGrpName in the $Location region has been successfully created."
}
```

#### <a name="set-user-metadata"></a>Set kullanıcı meta verileri

Kullanabileceğiniz betikler önceki bölümlerde yürüttükten sonra [kümesi AzureRmEventHubConsumerGroup](/powershell/module/azurerm.eventhub/set-azurermeventhubconsumergroup) cmdlet'i aşağıdaki örnekteki gibi bir tüketici grubu özelliklerini güncelleştirmek için:

```powershell
# Set some user metadata on the CG
Write-Host "Setting the UserMetadata field to 'Testing'"
Set-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -ConsumerGroupName $ConsumerGroupName -UserMetadata "Testing"
# Show result
Get-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -ConsumerGroupName $ConsumerGroupName
```

## <a name="remove-event-hub"></a>Olay hub'ı kaldırma

Oluşturduğunuz olay hub'ları kaldırmak için kullanabileceğiniz `Remove-*` aşağıdaki örnekte olduğu gibi cmdlet:

```powershell
# Clean up
Remove-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -ConsumerGroupName $ConsumerGroupName
Remove-AzureRmEventHub -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName
Remove-AzureRmEventHubNamespace -ResourceGroupName $ResGrpName -NamespaceName $Namespace
```

## <a name="next-steps"></a>Sonraki adımlar

- Tam olay hub'ları Resource Manager PowerShell modülü belgelerine bakın [burada](/powershell/module/azurerm.eventhub). Bu sayfa, tüm kullanılabilir cmdlet'leri listeler.
- Azure Resource Manager şablonları kullanma hakkında daha fazla bilgi için bkz: [bir Azure Resource Manager şablonu kullanarak olay hub'ı ve tüketici grubu ile bir olay hub'ları ad alanı oluşturma](event-hubs-resource-manager-namespace-event-hub.md).
- Hakkında bilgi [olay hub'ları .NET Yönetim kitaplıklarını](event-hubs-management-libraries.md).

[purchase options]: http://azure.microsoft.com/pricing/purchase-options/
[member offers]: http://azure.microsoft.com/pricing/member-offers/
[free account]: http://azure.microsoft.com/pricing/free-trial/
