---
title: "Öğretici: Azure PowerShell'i kullanarak özel bir rol oluşturma | Microsoft Docs"
description: Azure PowerShell'i kullanarak özel rol oluşturmaya başlama.
services: active-directory
documentationCenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: role-based-access-control
ms.devlang: ''
ms.topic: tutorial
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 06/12/2018
ms.author: rolyon
ms.openlocfilehash: f49f6f03b6d9f1c51cada58ae782bbc364fc9d66
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2019
ms.locfileid: "54427296"
---
# <a name="tutorial-create-a-custom-role-using-azure-powershell"></a>Öğretici: Azure PowerShell kullanarak özel bir rol oluşturun

[Yerleşik roller](built-in-roles.md) kuruluşunuzun ihtiyaçlarını karşılamıyorsa kendi özel rollerinizi oluşturabilirsiniz. Bu öğretici için Azure PowerShell'i kullanarak Reader Support Tickets adlı özel bir rol oluşturacaksınız. Bu özel rol, kullanıcının abonelikteki her şeyi görüntülemesini ve destek bileti açmasını sağlar.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Özel rol oluşturma
> * Özel rolleri listeleme
> * Özel rolü güncelleştirme
> * Özel rolü silme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

- [Sahip](built-in-roles.md#owner) veya [Kullanıcı Erişimi Yöneticisi](built-in-roles.md#user-access-administrator) gibi özel rol oluşturma izni
- Yerel ortamda [Azure PowerShell](/powershell/azure/azurerm/install-azurerm-ps)

## <a name="sign-in-to-azure-powershell"></a>Azure PowerShell oturumu açma

[Azure PowerShell](/powershell/azure/authenticate-azureps) oturumu açın.

## <a name="create-a-custom-role"></a>Özel rol oluşturma

Özel rol oluşturmanın en kolay yolu yerleşik rolle başlayıp düzenledikten sonra yeni bir rol oluşturmaktır.

1. PowerShell'de [Get-AzureRmProviderOperation](/powershell/module/azurerm.resources/get-azurermprovideroperation) komutunu kullanarak Microsoft.Support kaynak sağlayıcısında desteklenen işlemlerin listesini alabilirsiniz. İzinlerinizi oluşturmak için kullanabileceğiniz işlemleri bilmeniz yararlıdır. İşlemlerin tam listesini [Azure Resource Manager kaynak sağlayıcısı işlemleri](resource-provider-operations.md#microsoftsupport) sayfasında da görebilirsiniz.

    ```azurepowershell
    Get-AzureRMProviderOperation "Microsoft.Support/*" | FT Operation, Description -AutoSize
    ```
    
    ```Output
    Operation                              Description
    ---------                              -----------
    Microsoft.Support/register/action      Registers to Support Resource Provider
    Microsoft.Support/supportTickets/read  Gets Support Ticket details (including status, severity, contact ...
    Microsoft.Support/supportTickets/write Creates or Updates a Support Ticket. You can create a Support Tic...
    ```

1. [Okuyucu](built-in-roles.md#reader) rolünü JSON biçiminde çıkarmak için [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition) komutunu kullanın.

    ```azurepowershell
    Get-AzureRmRoleDefinition -Name "Reader" | ConvertTo-Json | Out-File C:\CustomRoles\ReaderSupportRole.json
    ```

1. **ReaderSupportRole.json** dosyasını bir düzenleyicide açın.

    Aşağıdaki JSON çıktısı gösterilir. Farklı özellikler hakkında bilgi edinmek için bkz. [Özel roller](custom-roles.md).

    ```json
    {
        "Name":  "Reader",
        "Id":  "acdd72a7-3385-48ef-bd42-f606fba81ae7",
        "IsCustom":  false,
        "Description":  "Lets you view everything, but not make any changes.",
        "Actions":  [
                        "*/read"
                    ],
        "NotActions":  [
    
                       ],
        "DataActions":  [
    
                        ],
        "NotDataActions":  [
    
                           ],
        "AssignableScopes":  [
                                 "/"
                             ]
    }
    ```
    
1. JSON dosyasını düzenleyerek `"Microsoft.Support/*"` işlemini `Actions` özelliğine ekleyin. Okuma işleminden sonra virgül eklemeyi unutmayın. Bu eylem, kullanıcıya destek bileti oluşturma izni verecektir.

1. [Get-AzureRmSubscription](/powershell/module/azurerm.profile/get-azurermsubscription) komutunu kullanarak aboneliğinizin kimliğini alın.

    ```azurepowershell
    Get-AzureRmSubscription
    ```

1. `AssignableScopes` içine abonelik kimliğinizi şu biçimde ekleyin: `"/subscriptions/00000000-0000-0000-0000-000000000000"`

    Açık abonelik kimliklerini girmeniz gerekir, aksi halde rolü aboneliğinize aktaramazsınız.

1. `Id` özellik satırını silin ve `IsCustom` özelliğini `true` olarak değiştirin.

1. `Name` ve `Description` özelliklerini "Okuyucu Destek Biletleri" ve "Abonelikteki her şeyi görüntüleme ve destek bileti açma." olarak değiştirin.

    JSON dosyanız aşağıdaki gibi görünmelidir:

    ```json
    {
        "Name":  "Reader Support Tickets",
        "IsCustom":  true,
        "Description":  "View everything in the subscription and also open support tickets.",
        "Actions":  [
                        "*/read",
                        "Microsoft.Support/*"
                    ],
        "NotActions":  [
    
                       ],
        "DataActions":  [
    
                        ],
        "NotDataActions":  [
    
                           ],
        "AssignableScopes":  [
                                 "/subscriptions/00000000-0000-0000-0000-000000000000"
                             ]
    }
    ```
    
1. Yeni özel rolü oluşturmak için [New-AzureRmRoleDefinition](/powershell/module/azurerm.resources/new-azurermroledefinition) komutunu kullanın ve JSON rol tanımı dosyasını belirtin.

    ```azurepowershell
    New-AzureRmRoleDefinition -InputFile "C:\CustomRoles\ReaderSupportRole.json"
    ```

    ```Output
    Name             : Reader Support Tickets
    Id               : 22222222-2222-2222-2222-222222222222
    IsCustom         : True
    Description      : View everything in the subscription and also open support tickets.
    Actions          : {*/read, Microsoft.Support/*}
    NotActions       : {}
    DataActions      : {}
    NotDataActions   : {}
    AssignableScopes : {/subscriptions/00000000-0000-0000-0000-000000000000}
    ```
        
    Yeni özel rol artık Azure portalında kullanılabilir ve yerleşik roller gibi kullanıcılara, gruplara veya hizmet sorumlularına atanabilir.

## <a name="list-custom-roles"></a>Özel rolleri listeleme

- Özel rollerinizin tümünü listelemek için [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition) komutunu kullanın.

    ```azurepowershell
    Get-AzureRmRoleDefinition | ? {$_.IsCustom -eq $true} | FT Name, IsCustom
    ```

    ```Output
    Name                   IsCustom
    ----                   --------
    Reader Support Tickets     True
    ```
    
    Özel rolü Azure portalında da görebilirsiniz.

    ![Azure portalına aktarılmış olan özel rolün ekran görüntüsü](./media/tutorial-custom-role-powershell/custom-role-reader-support-tickets.png)

## <a name="update-a-custom-role"></a>Özel rolü güncelleştirme

Özel rolü güncelleştirmek için JSON dosyasını güncelleştirebilir veya `PSRoleDefinition` nesnesini kullanabilirsiniz.

1. JSON dosyasını güncelleştirmek için [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition) komutunu kullanarak JSON biçiminde özel rolün çıktısını alın.

    ```azurepowershell
    Get-AzureRmRoleDefinition -Name "Reader Support Tickets" | ConvertTo-Json | Out-File C:\CustomRoles\ReaderSupportRole2.json
    ```

1. Dosyayı bir düzenleyicide açın.

1. `Actions` içine kaynak grubu dağıtımlarını oluşturma ve yönetme işlemini ekleyin: `"Microsoft.Resources/deployments/*"`.

    Güncelleştirilmiş JSON dosyanız aşağıdaki gibi görünmelidir:

    ```json
    {
        "Name":  "Reader Support Tickets",
        "Id":  "22222222-2222-2222-2222-222222222222",
        "IsCustom":  true,
        "Description":  "View everything in the subscription and also open support tickets.",
        "Actions":  [
                        "*/read",
                        "Microsoft.Support/*",
                        "Microsoft.Resources/deployments/*"
                    ],
        "NotActions":  [
    
                       ],
        "DataActions":  [
    
                        ],
        "NotDataActions":  [
    
                           ],
        "AssignableScopes":  [
                                 "/subscriptions/00000000-0000-0000-0000-000000000000"
                             ]
    }
    ```
        
1. Özel rolü güncelleştirmek için [Set-AzureRmRoleDefinition](/powershell/module/azurerm.resources/set-azurermroledefinition) komutunu kullanarak güncelleştirilmiş JSON dosyasını belirtin.

    ```azurepowershell
    Set-AzureRmRoleDefinition -InputFile "C:\CustomRoles\ReaderSupportRole2.json"
    ```

    ```Output
    Name             : Reader Support Tickets
    Id               : 22222222-2222-2222-2222-222222222222
    IsCustom         : True
    Description      : View everything in the subscription and also open support tickets.
    Actions          : {*/read, Microsoft.Support/*, Microsoft.Resources/deployments/*}
    NotActions       : {}
    DataActions      : {}
    NotDataActions   : {}
    AssignableScopes : {/subscriptions/00000000-0000-0000-0000-000000000000}
    ```

1. `PSRoleDefintion` nesnesini kullanarak özel rolünüzü güncelleştirmek için öncelikle [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition) komutunu kullanarak ilgili rolü alın.

    ```azurepowershell
    $role = Get-AzureRmRoleDefinition "Reader Support Tickets"
    ```
    
1. Tanılama ayarlarını okuma işlemini eklemek için `Add` metodunu çağırın.

    ```azurepowershell
    $role.Actions.Add("Microsoft.Insights/diagnosticSettings/*/read")
    ```

1. Rolü güncelleştirmek için [Set-AzureRmRoleDefinition](/powershell/module/azurerm.resources/set-azurermroledefinition) komutunu kullanın.

    ```azurepowershell
    Set-AzureRmRoleDefinition -Role $role
    ```
    
    ```Output
    Name             : Reader Support Tickets
    Id               : 22222222-2222-2222-2222-222222222222
    IsCustom         : True
    Description      : View everything in the subscription and also open support tickets.
    Actions          : {*/read, Microsoft.Support/*, Microsoft.Resources/deployments/*,
                       Microsoft.Insights/diagnosticSettings/*/read}
    NotActions       : {}
    DataActions      : {}
    NotDataActions   : {}
    AssignableScopes : {/subscriptions/00000000-0000-0000-0000-000000000000}
    ```
    
## <a name="delete-a-custom-role"></a>Özel rolü silme

1. Özel rolün kimliğini almak için [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition) komutunu kullanın.

    ```azurepowershell
    Get-AzureRmRoleDefinition "Reader Support Tickets"
    ```

1. Özel rolü silmek için [Remove-AzureRmRoleDefinition](/powershell/module/azurerm.resources/remove-azurermroledefinition) komutunu kullanın ve rol kimliğini belirtin.

    ```azurepowershell
    Remove-AzureRmRoleDefinition -Id "22222222-2222-2222-2222-222222222222"
    ```

    ```Output
    Confirm
    Are you sure you want to remove role definition with id '22222222-2222-2222-2222-222222222222'.
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
    ```

1. Onaylamanız istendiğinde **Y** yazın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [PowerShell'i kullanarak özel roller oluşturma](custom-roles-powershell.md)
