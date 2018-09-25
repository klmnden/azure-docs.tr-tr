---
title: 'Azure Active Directory Domain Services: eşitleme kapsamı | Microsoft Docs'
description: Yönetilen etki alanlarınızı Azure AD'den kapsamlı eşitlemeyi yapılandırma
services: active-directory-ds
documentationcenter: ''
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: 9389cf0f-0036-4b17-95da-80838edd2225
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/19/2018
ms.author: maheshu
ms.openlocfilehash: ada6e7536ec91e5b1f35f1740177ae264fe68cf7
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47063500"
---
# <a name="configure-scoped-synchronization-from-azure-ad-to-your-managed-domain"></a>Yönetilen etki alanınızı Azure AD'den kapsamlı eşitlemeyi yapılandırma
Bu makalede, yalnızca belirli kullanıcı hesaplarını Azure AD dizininizi Azure AD Domain Services yönetilen Etki Alanınızla eşitlenmek üzere yapılandırma işlemini göstermektedir.


## <a name="group-based-scoped-synchronization"></a>Grup tabanlı kapsamlı eşitleme
Varsayılan olarak, yönetilen Etki Alanınızla tüm kullanıcıları ve grupları Azure AD dizininizdeki eşitlenir. Yönetilen etki alanında yalnızca bazı kullanıcılar tarafından kullanılıyorsa, yönetilen etki alanında yalnızca bu kullanıcı hesaplarını eşitlemek tercih edebilirsiniz. Grup tabanlı kapsamlı eşitleme bunu yapmanızı sağlar. Yapılandırıldığında, yalnızca kullanıcı hesapları, belirttiğiniz bir gruba ait olan yönetilen etki alanına eşitlenir.


## <a name="get-started-install-the-required-powershell-modules"></a>Kullanmaya başlayın: gerekli PowerShell modüllerini yükleyin

### <a name="install-and-configure-azure-ad-powershell"></a>Azure AD PowerShell'i yükleme ve yapılandırma
İçin makaledeki yönergeleri [Azure AD PowerShell modülünü yüklemek ve Azure AD'ye bağlanma](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?toc=%2fazure%2factive-directory-domain-services%2ftoc.json).

### <a name="install-and-configure-azure-powershell"></a>Azure PowerShell'i yükleyip yapılandırma
İçin makaledeki yönergeleri [Azure PowerShell modülünü yüklemek ve Azure aboneliğinize bağlayın](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?toc=%2fazure%2factive-directory-domain-services%2ftoc.json).



## <a name="enable-group-based-scoped-synchronization"></a>Grup tabanlı kapsamlı eşitlemeyi etkinleştirme
Grup tabanlı kapsamlı eşitleme yönetilen etki alanınıza yapılandırmak için aşağıdaki adımları tamamlayın:

1. Yönetilen etki alanınızla eşitlenmesini istediğiniz grupların görünen adını belirtin ve istediğiniz grupları seçin.

2. Betik aşağıdaki bölümde adlı bir dosyaya Kaydet ```Select-GroupsToSync.ps1```. Betik yürütme aşağıdaki gibi:

  ```powershell
  .\Select-GroupsToSync.ps1 -groupsToAdd @(“GroupName1”, “GroupName2”)
  ```

3. Şimdi, yönetilen etki alanı için Grup tabanlı ile kapsamlı eşitlenmesini etkinleştirir.

  ```powershell
  // Login to your Azure AD tenant
  Login-AzureRmAccount

  // Retrieve the Azure AD Domain Services resource.
  $DomainServicesResource = Get-AzureRmResource -ResourceType "Microsoft.AAD/DomainServices"

  // Enable group-based scoped synchronization.
  $enableScopedSync = @{"filteredSync" = "Enabled"}

  Set-AzureRmResource -Id $DomainServicesResource.ResourceId -Properties $enableScopedSync
  ```

## <a name="disable-group-based-scoped-synchronization"></a>Grup tabanlı kapsamlı eşitleme devre dışı bırak
Grup tabanlı devre dışı bırakmak için aşağıdaki PowerShell betiğini kullanın, yönetilen etki alanınız için eşitleme kapsamı:

```powershell
// Login to your Azure AD tenant
Login-AzureRmAccount

// Retrieve the Azure AD Domain Services resource.
$DomainServicesResource = Get-AzureRmResource -ResourceType "Microsoft.AAD/DomainServices"

// Disable group-based scoped synchronization.
$disableScopedSync = @{"filteredSync" = "Disabled"}

Set-AzureRmResource -Id $DomainServicesResource.ResourceId -Properties $disableScopedSync
```

## <a name="script-to-select-groups-to-synchronize-to-the-managed-domain-select-groupstosyncps1"></a>Komut (Select-GroupsToSync.ps1) yönetilen etki alanı Eşitleme gruplarını seçmek için
Aşağıdaki betiği bir dosyaya kaydet (```Select-GroupsToSync.ps1```). Bu betik, Azure AD Domain Services yönetilen etki alanında seçilen grupları eşitlemek için yapılandırır. Belirtilen gruba ait olan tüm kullanıcı hesapları için yönetilen etki alanı eşitlenecektir.

```powershell
param (
    [Parameter(Position = 0)]
    [String[]]$groupsToAdd
)

Connect-AzureAD
$sp = Get-AzureADServicePrincipal -Filter "AppId eq '2565bd9d-da50-47d4-8b85-4c97f669dc36'"
$role = $sp.AppRoles | where-object -FilterScript {$_.DisplayName -eq "User"}

Write-Output "`n****************************************************************************"

Write-Output "Total group-assignments need to be added: $($groupsToAdd.Count)"
$newGroupIds = New-Object 'System.Collections.Generic.HashSet[string]'
foreach ($groupName in $groupsToAdd)
{
    try
    {
        $group = Get-AzureADGroup -Filter "DisplayName eq '$groupName'"
        $newGroupIds.Add($group.ObjectId)

        Write-Output "Group-Name: $groupName, Id: $($group.ObjectId)"
    }
    catch
    {
        Write-Error "Failed to find group: $groupName. Exception: $($_.Exception)."
    }
}

Write-Output "****************************************************************************`n"
Write-Output "`n****************************************************************************"

$currentAssignments = Get-AzureADServiceAppRoleAssignment -ObjectId $sp.ObjectId
Write-Output "Total current group-assignments: $($currentAssignments.Count), SP-ObjectId: $($sp.ObjectId)"

$currAssignedObjectIds = New-Object 'System.Collections.Generic.HashSet[string]'
foreach ($assignment in $currentAssignments)
{
    Write-Output "Assignment-ObjectId: $($assignment.PrincipalId)"

    if ($newGroupIds.Contains($assignment.PrincipalId) -eq $false)
    {
        Write-Output "This assignment is not needed anymore. Removing it! Assignment-ObjectId: $($assignment.PrincipalId)"
        Remove-AzureADServiceAppRoleAssignment -ObjectId $sp.ObjectId -AppRoleAssignmentId $assignment.ObjectId
    }
    else
    {
        $currAssignedObjectIds.Add($assignment.PrincipalId)
    }
}

Write-Output "****************************************************************************`n"
Write-Output "`n****************************************************************************"

foreach ($id in $newGroupIds)
{
    try
    {
        if ($currAssignedObjectIds.Contains($id) -eq $false)
        {
            Write-Output "Adding new group-assignment. Role-Id: $($role.Id), Group-Object-Id: $id, ResourceId: $($sp.ObjectId)"
            New-AzureADGroupAppRoleAssignment -Id $role.Id -ObjectId $id -PrincipalId $id -ResourceId $sp.ObjectId
        }
        else
        {
            Write-Output "Group-ObjectId: $id is already assigned."
        }
    }
    catch
    {
        Write-Error "Exception occured assigning Object-ID: $id. Exception: $($_.Exception)."
    }
}

Write-Output "****************************************************************************`n"
```

## <a name="next-steps"></a>Sonraki adımlar
* [Azure AD Etki Alanı Hizmetleri'nde eşitleme anlama](active-directory-ds-synchronization.md)
