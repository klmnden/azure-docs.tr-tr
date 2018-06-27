---
title: Azure Service Bus kaynaklarını yönetmek için PowerShell kullanma | Microsoft Docs
description: PowerShell modülü oluşturun ve Service Bus kaynaklarını yönetmek için kullanın
services: service-bus-messaging
documentationcenter: .NET
author: sethmanheim
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/21/2017
ms.author: sethm
ms.openlocfilehash: 962134c0c71ac0a251f8adf1f0f067d6067cb808
ms.sourcegitcommit: 0fa8b4622322b3d3003e760f364992f7f7e5d6a9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37018630"
---
# <a name="use-powershell-to-manage-service-bus-resources"></a>Service Bus kaynaklarını yönetmek için PowerShell kullanma

Microsoft Azure PowerShell denetlemek ve dağıtımını ve Azure Hizmetleri yönetimini otomatikleştirmek için kullanabileceğiniz bir komut dosyası ortamıdır. Bu makalede nasıl kullanılacağını açıklar [Service Bus Resource Manager PowerShell Modülü](/powershell/module/azurerm.servicebus) sağlamak ve hizmet veri yolu varlıklarını (ad alanları, kuyruklar, konular ve abonelikler) yönetmek için yerel Azure PowerShell konsolunda veya komut dosyası kullanarak.

Hizmet veri yolu varlıklarını Azure Resource Manager şablonları kullanarak da yönetebilirsiniz. Daha fazla bilgi için bkz: [oluşturma Service Bus kaynaklarını Azure Resource Manager şablonları kullanarak](service-bus-resource-manager-overview.md).

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce aşağıdaki önkoşullar gerekir:

* Azure aboneliği. Bir aboneliği edinme hakkında daha fazla bilgi için bkz: [satın alma seçeneği][purchase options], [üye teklifleri][member offers], veya [boş Hesap][free account].
* Azure PowerShell ile bir bilgisayar. Yönergeler için bkz: [Azure PowerShell cmdlet'leri kullanmaya başlama](/powershell/azure/get-started-azureps).
* PowerShell komut dosyaları, NuGet paketlerini ve .NET Framework genel anlama.

## <a name="get-started"></a>başlarken

İlk adım, Azure hesabınızı ve Azure abonelik oturum açmak için PowerShell kullanmaktır. ' Ndaki yönergeleri izleyin [Azure PowerShell cmdlet'leri kullanmaya başlama](/powershell/azure/get-started-azureps) Azure hesabınızda oturum açın ve almak ve Azure aboneliğinizde kaynaklara erişmek için.

## <a name="provision-a-service-bus-namespace"></a>Bir hizmet veri yolu ad alanı sağlama

Hizmet veri yolu ad alanları ile çalışırken, kullanabileceğiniz [Get-AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/get-azurermservicebusnamespace), [yeni AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/new-azurermservicebusnamespace), [Remove-AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/remove-azurermservicebusnamespace), ve [kümesi AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/set-azurermservicebusnamespace) cmdlet'leri.

Bu örnek komut dosyasında birkaç yerel değişkenler oluşturur; `$Namespace` ve `$Location`.

* `$Namespace` Biz birlikte çalışmak istediğiniz hizmet veri yolu ad alanı adıdır.
* `$Location` Veri Merkezi biz ad sağlamak tanımlar.
* `$CurrentNamespace` Biz alınamıyor (veya oluşturma) başvuru ad alanını depolar.

Gerçek bir betik içinde `$Namespace` ve `$Location` parametre olarak geçirilebilir.

Bu komut dosyasının parçası şunları yapar:

1. Belirtilen ada sahip bir hizmet veri yolu ad alanı almaya çalışır.
2. Ad alanı bulunursa, bulunanları bildirir.
3. Ad alanı bulunmazsa, ad alanı oluşturur ve yeni oluşturulan ad alanı alır.
   
    ``` powershell
    # Query to see if the namespace currently exists
    $CurrentNamespace = Get-AzureRMServiceBusNamespace -ResourceGroup $ResGrpName -NamespaceName $Namespace
   
    # Check if the namespace already exists or needs to be created
    if ($CurrentNamespace)
    {
        Write-Host "The namespace $Namespace already exists in the $Location region:"
        # Report what was found
        Get-AzureRMServiceBusNamespace -ResourceGroup $ResGrpName -NamespaceName $Namespace
    }
    else
    {
        Write-Host "The $Namespace namespace does not exist."
        Write-Host "Creating the $Namespace namespace in the $Location region..."
        New-AzureRmServiceBusNamespace -ResourceGroup $ResGrpName -NamespaceName $Namespace -Location $Location
        $CurrentNamespace = Get-AzureRMServiceBusNamespace -ResourceGroup $ResGrpName -NamespaceName $Namespace
        Write-Host "The $Namespace namespace in Resource Group $ResGrpName in the $Location region has been successfully created."
                
    }
    ```

### <a name="create-a-namespace-authorization-rule"></a>Ad alanı yetkilendirme kuralı oluştur

Aşağıdaki örnek ad alanı yetkilendirme kurallarını kullanarak yönetmek nasıl gösterir [yeni AzureRmServiceBusNamespaceAuthorizationRule](/powershell/module/azurerm.servicebus/new-azurermservicebusnamespaceauthorizationrule), [Get-AzureRmServiceBusNamespaceAuthorizationRule](/powershell/module/azurerm.servicebus/get-azurermservicebusnamespaceauthorizationrule), [Kümesi AzureRmServiceBusNamespaceAuthorizationRule](/powershell/module/azurerm.servicebus/set-azurermservicebusnamespaceauthorizationrule), ve [Kaldır AzureRmServiceBusNamespaceAuthorizationRule cmdlet'leri](/powershell/module/azurerm.servicebus/remove-azurermservicebusnamespaceauthorizationrule).

```powershell
# Query to see if rule exists
$CurrentRule = Get-AzureRmServiceBusNamespaceAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthorizationRuleName $AuthRule

# Check if the rule already exists or needs to be created
if ($CurrentRule)
{
    Write-Host "The $AuthRule rule already exists for the namespace $Namespace."
}
else
{
    Write-Host "The $AuthRule rule does not exist."
    Write-Host "Creating the $AuthRule rule for the $Namespace namespace..."
    New-AzureRmServiceBusAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthorizationRuleName $AuthRule -Rights @("Listen","Send")
    $CurrentRule = Get-AzureRmServiceBusAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthorizationRuleName $AuthRule
    Write-Host "The $AuthRule rule for the $Namespace namespace has been successfully created."

    Write-Host "Setting rights on the namespace"
    $authRuleObj = Get-AzureRmServiceBusAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthorizationRuleName $AuthRule

    Write-Host "Remove Send rights"
    $authRuleObj.Rights.Remove("Send")
    Set-AzureRmServiceBusAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthRuleObj $authRuleObj

    Write-Host "Add Send and Manage rights to the namespace"
    $authRuleObj.Rights.Add("Send")
    Set-AzureRmServiceBusAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthRuleObj $authRuleObj
    $authRuleObj.Rights.Add("Manage")
    Set-AzureRmServiceBusAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthRuleObj $authRuleObj

    Write-Host "Show value of primary key"
    $CurrentKey = Get-AzureRmServiceBusKey -ResourceGroup $ResGrpName -NamespaceName $Namespace -Name $AuthRule
        
    Write-Host "Remove this authorization rule"
    Remove-AzureRmServiceBusAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -Name $AuthRule
}
```

## <a name="create-a-queue"></a>Bir kuyruk oluşturma

Bir kuyruk veya konu oluşturmak için önceki bölümde komut dosyası kullanarak bir ad alanı denetimi gerçekleştirin. Ardından, sıranın oluşturun:

```powershell
# Check if queue already exists
$CurrentQ = Get-AzureRmServiceBusQueue -ResourceGroup $ResGrpName -NamespaceName $Namespace -QueueName $QueueName

if($CurrentQ)
{
    Write-Host "The queue $QueueName already exists in the $Location region:"
}
else
{
    Write-Host "The $QueueName queue does not exist."
    Write-Host "Creating the $QueueName queue in the $Location region..."
    New-AzureRmServiceBusQueue -ResourceGroup $ResGrpName -NamespaceName $Namespace -QueueName $QueueName -EnablePartitioning $True
    $CurrentQ = Get-AzureRmServiceBusQueue -ResourceGroup $ResGrpName -NamespaceName $Namespace -QueueName $QueueName
    Write-Host "The $QueueName queue in Resource Group $ResGrpName in the $Location region has been successfully created."
}
```

### <a name="modify-queue-properties"></a>Sıra özelliklerini değiştir

Kullanabileceğiniz önceki bölümde komut dosyası yürütme sonrasında [kümesi AzureRmServiceBusQueue](/powershell/module/azurerm.servicebus/set-azurermservicebusqueue) cmdlet'i aşağıdaki örnekteki gibi bir sıranın özelliklerini güncelleştirmek için:

```powershell
$CurrentQ.DeadLetteringOnMessageExpiration = $True
$CurrentQ.MaxDeliveryCount = 7
$CurrentQ.MaxSizeInMegabytes = 2048
$CurrentQ.EnableExpress = $True

Set-AzureRmServiceBusQueue -ResourceGroup $ResGrpName -NamespaceName $Namespace -QueueName $QueueName -QueueObj $CurrentQ
```

## <a name="provisioning-other-service-bus-entities"></a>Diğer hizmet veri yolu varlıklarını sağlama

Kullanabileceğiniz [Service Bus PowerShell modülünü](/powershell/module/azurerm.servicebus) konu başlıkları ve abonelikler gibi diğer varlıklar sağlayacak. Bu cmdlet, önceki bölümde gösterilen kuyruk oluşturma cmdlet'leri sözdizimsel olarak benzerdir.

## <a name="next-steps"></a>Sonraki adımlar

- Tam Service Bus Resource Manager PowerShell modülü belgelerine bakın [burada](/powershell/module/azurerm.servicebus). Bu sayfa, tüm kullanılabilir cmdlet'leri listeler.
- Azure Resource Manager şablonları kullanma hakkında daha fazla bilgi için bkz: [oluşturma Service Bus kaynaklarını Azure Resource Manager şablonları kullanarak](service-bus-resource-manager-overview.md).
- Hakkında bilgi [Service Bus .NET Yönetim kitaplıklarını](service-bus-management-libraries.md).

Bu Web günlüğü postaları açıklandığı gibi hizmet veri yolu varlıklarını yönetmek için bazı alternatif yolu vardır:

* [Service Bus kuyrukları, konuları ve abonelikleri bir PowerShell Betiği kullanılarak oluşturma](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
* [Hizmet veri yolu Namespace ve bir PowerShell komut dosyası kullanarak bir Event Hub oluşturma](http://blogs.msdn.com/b/paolos/archive/2014/12/01/how-to-create-a-service-bus-namespace-and-an-event-hub-using-a-powershell-script.aspx)
* [Hizmet veri yolu PowerShell betikleri](https://code.msdn.microsoft.com/Service-Bus-PowerShell-a46b7059)

<!--Anchors-->

[purchase options]: http://azure.microsoft.com/pricing/purchase-options/
[member offers]: http://azure.microsoft.com/pricing/member-offers/
[free account]: http://azure.microsoft.com/pricing/free-trial/
