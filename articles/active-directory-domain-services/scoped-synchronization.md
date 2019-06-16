---
title: 'Azure Active Directory etki alanı Hizmetleri: Eşitleme kapsamı | Microsoft Docs'
description: Yönetilen etki alanlarınızı Azure AD'den kapsamlı eşitlemeyi yapılandırma
services: active-directory-ds
documentationcenter: ''
author: MikeStephens-MS
manager: daveba
editor: curtand
ms.assetid: 9389cf0f-0036-4b17-95da-80838edd2225
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/20/2019
ms.author: mstephen
ms.openlocfilehash: 332c0bc43a269734e0dc4db37228006a78e460e2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66246817"
---
# <a name="configure-scoped-synchronization-from-azure-ad-to-your-managed-domain"></a>Yönetilen etki alanınızı Azure AD'den kapsamlı eşitlemeyi yapılandırma
Bu makalede, yalnızca belirli kullanıcı hesaplarını Azure AD dizininizi Azure AD Domain Services yönetilen Etki Alanınızla eşitlenmek üzere yapılandırma işlemini göstermektedir.


## <a name="group-based-scoped-synchronization"></a>Grup tabanlı kapsamlı eşitleme
Varsayılan olarak, yönetilen Etki Alanınızla tüm kullanıcıları ve grupları Azure AD dizininizdeki eşitlenir. Yalnızca bazı kullanıcılar yönetilen etki alanını kullan, yalnızca bu kullanıcı hesaplarını eşitlemek. Grup tabanlı kapsamlı eşitleme bunu yapmanızı sağlar. Yapılandırıldığında, yalnızca kullanıcı hesapları, belirttiğiniz bir gruba ait olan yönetilen etki alanına eşitlenir.

Aşağıdaki tabloda nasıl kapsamlı eşitleme kullanılacağını belirlemenize yardımcı olur:

| **Geçerli durumu** | **İstenen durum** | **Gerekli yapılandırma** |
| --- | --- | --- |
| Mevcut yönetilen etki alanınıza, tüm kullanıcı hesapları ve grupları eşitlemek için yapılandırılır. | Yönetilen etki alanınıza belirli gruplara ait kullanıcı hesaplarını eşitlemek istediğiniz. | [Mevcut yönetilen etki alanını Sil](delete-aadds.md). Ardından, yeniden yapılandırılmış kapsamlı eşitleme ile oluşturmak için bu makaledeki yönergeleri izleyin. |
| Mevcut bir yönetilen etki alanına sahip değilsiniz. | Yeni bir yönetilen etki alanı oluşturmak ve yalnızca belirli gruplara ait kullanıcı hesaplarını eşitlemek istediğiniz. | Kapsamlı eşitleme ile yapılandırılmış yeni bir yönetilen etki alanı oluşturmak için bu makaledeki yönergeleri izleyin. |
| Mevcut yönetilen etki alanınıza yalnızca belirli gruplara ait hesaplarını eşitlemek üzere yapılandırılır. | Kullanıcıları Yönet etki eşitlenmesi gerektiğini grupları listesini değiştirmek istediğiniz. | Kapsamlı eşitleme değiştirmek için bu makaledeki yönergeleri izleyin. |

> [!WARNING]
> **Eşitleme kapsamı değiştirmek, yeniden eşitleme gitmek yönetilen etki alanınızı neden olur.**
> 
>  * Yönetilen bir etki alanı için eşitleme kapsamı değiştirdiğinizde, bir tam eşitleme gerçekleşir.
>  * Yönetilen etki alanında artık gerekli olan nesneler silinir. Yeni nesneler yönetilen etki alanında oluşturulur.
>  * Yeniden eşitleme, yönetilen etki alanınızı ve Azure AD dizininizi nesne (kullanıcılar, gruplar ve grup üyelikleri) sayısına bağlı olarak tamamlanması uzun sürebilir. Yüz binlerce nesne içeren büyük dizinler için yeniden eşitleme birkaç gün sürebilir.


## <a name="create-a-new-managed-domain-and-enable-group-based-scoped-synchronization-using-azure-portal"></a>Yeni bir yönetilen etki alanı oluşturma ve Azure portalını kullanarak grup tabanlı kapsamlı eşitlemeyi etkinleştirme

1. İzleyin [Başlarken kılavuzunda](create-instance.md) yönetilen bir etki alanı oluşturma.
2. Seçin **kapsamlı** sırasında Azure AD Domain Services Oluşturma Sihirbazı'nı eşitleme stil seçimi.

## <a name="create-a-new-managed-domain-and-enable-group-based-scoped-synchronization-using-powershell"></a>Yeni bir yönetilen etki alanı oluşturma ve PowerShell kullanarak grup tabanlı kapsamlı eşitlemeyi etkinleştirme
Bu adım kümesini tamamlamak için PowerShell kullanın. Yönergelere bakın [Azure Active Directory etki alanı PowerShell kullanarak Services'i etkinleştirme](powershell-create-instance.md). Bu makaledeki adımlarda birkaç değiştirildiğinde biraz daha kapsamlı eşitleme yapılandırılamadı.

Grup tabanlı kapsamlı eşitleme yönetilen etki alanınıza yapılandırmak için aşağıdaki adımları tamamlayın:

1. Aşağıdaki görevleri tamamlayın:
   * [1. Görev: Gerekli PowerShell modüllerini yükleyin](powershell-create-instance.md#task-1-install-the-required-powershell-modules).
   * [2. Görev: Azure AD dizininizde gerekli hizmet sorumlusu oluşturma](powershell-create-instance.md#task-2-create-the-required-service-principal-in-your-azure-ad-directory).
   * [Görev 3: Oluşturma ve yapılandırma 'AAD DC Administrators' group]powershell-create-instance.md#task-3-create-and-configure-the-aad-dc-administrators-group).
   * [Görev 4: Azure AD Domain Services kaynak sağlayıcısını kaydetme](powershell-create-instance.md#task-4-register-the-azure-ad-domain-services-resource-provider).
   * [Görev 5: Bir kaynak grubu oluşturma](powershell-create-instance.md#task-5-create-a-resource-group).
   * [6. Görev: Oluşturma ve sanal ağ yapılandırma](powershell-create-instance.md#task-6-create-and-configure-the-virtual-network).

2. Yönetilen etki alanınızla eşitlenmesini istediğiniz grupların görünen adını belirtin ve istediğiniz grupları seçin.

3. Kaydet [betik aşağıdaki bölümdeki](scoped-synchronization.md#script-to-select-groups-to-synchronize-to-the-managed-domain-select-groupstosyncps1) adlı bir dosyaya ```Select-GroupsToSync.ps1```. Betik yürütme aşağıdaki gibi:

   ```powershell
   .\Select-GroupsToSync.ps1 -groupsToAdd @("AAD DC Administrators", "GroupName1", "GroupName2")
   ```

   > [!WARNING]
   > **'AAD DC Administrators' grubuna eklemeyi unutmayın.**
   >
   > Kapsamlı eşitleme için yapılandırılan Grup listesinde 'AAD DC Administrators' grubuna eklemeniz gerekir. Bu grup eklemezseniz, yönetilen etki alanında kullanılamaz.
   >

4. Şimdi, yönetilen etki alanı oluşturun ve grup tabanlı kapsamlı eşitleme yönetilen etki alanı için etkinleştirin. Özelliği ```"filteredSync" = "Enabled"``` içinde ```Properties``` parametresi. Kopyalama kaynağı aşağıdaki kod parçası, örneği için bkz: [görev 7: Azure AD Domain Services yönetilen etki alanı sağlama](powershell-create-instance.md#task-7-provision-the-azure-ad-domain-services-managed-domain).

   ```powershell
   $AzureSubscriptionId = "YOUR_AZURE_SUBSCRIPTION_ID"
   $ManagedDomainName = "contoso100.com"
   $ResourceGroupName = "ContosoAaddsRg"
   $VnetName = "DomainServicesVNet_WUS"
   $AzureLocation = "westus"

   # Enable Azure AD Domain Services for the directory.
   New-AzResource -ResourceId "/subscriptions/$AzureSubscriptionId/resourceGroups/$ResourceGroupName/providers/Microsoft.AAD/DomainServices/$ManagedDomainName" `
   -Location $AzureLocation `
   -Properties @{"DomainName"=$ManagedDomainName; "filteredSync" = "Enabled"; `
    "SubnetId"="/subscriptions/$AzureSubscriptionId/resourceGroups/$ResourceGroupName/providers/Microsoft.Network/virtualNetworks/$VnetName/subnets/DomainServices"} `
   -ApiVersion 2017-06-01 -Force -Verbose
   ```

   > [!TIP]
   > Eklenecek unutmadığınızdan ```"filteredSync" = "Enabled"``` içinde ```-Properties``` parametresi, yönetilen etki alanı için kapsamlı eşitleme etkin.


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
        Write-Error "Exception occurred assigning Object-ID: $id. Exception: $($_.Exception)."
    }
}

Write-Output "****************************************************************************`n"
```


## <a name="modify-group-based-scoped-synchronization"></a>Grup tabanlı kapsamlı eşitleme değiştirme
Olan kullanıcılar yönetilen Etki Alanınızla eşitlenmesi gerektiğini grupları listesini değiştirmek için yeniden çalıştırma [PowerShell Betiği](scoped-synchronization.md#script-to-select-groups-to-synchronize-to-the-managed-domain-select-groupstosyncps1) ve yeni grup listesini belirtin. 'AAD DC Administrators' grubunun her zaman bu listede belirtmeyi unutmayın.

> [!WARNING]
> **'AAD DC Administrators' grubuna eklemeyi unutmayın.**
>
> Kapsamlı eşitleme için yapılandırılan Grup listesinde 'AAD DC Administrators' grubuna eklemeniz gerekir. Bu grup eklemezseniz, yönetilen etki alanında kullanılamaz.
>


## <a name="disable-group-based-scoped-synchronization"></a>Grup tabanlı kapsamlı eşitleme devre dışı bırak
Grup tabanlı devre dışı bırakmak için aşağıdaki PowerShell betiğini kullanın, yönetilen etki alanınız için eşitleme kapsamı:

```powershell
// Login to your Azure AD tenant
Login-AzAccount

// Retrieve the Azure AD Domain Services resource.
$DomainServicesResource = Get-AzResource -ResourceType "Microsoft.AAD/DomainServices"

// Disable group-based scoped synchronization.
$disableScopedSync = @{"filteredSync" = "Disabled"}

Set-AzResource -Id $DomainServicesResource.ResourceId -Properties $disableScopedSync
```

## <a name="next-steps"></a>Sonraki adımlar
* [Azure AD Etki Alanı Hizmetleri'nde eşitleme anlama](synchronization.md)
* [Azure Active Directory etki alanı PowerShell kullanarak Services'i etkinleştirme](powershell-create-instance.md)
