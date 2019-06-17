---
title: Atama ve Azure PowerShell - Azure Active Directory ile yönetici rol atamasını Kaldır | Microsoft Docs
description: Kişiler için sık rol atamalarını yönetmek, artık Azure PowerShell ile bir Azure AD yöneticisi rolünün üyeleri yönetebilirsiniz.
services: active-directory
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 04/15/2019
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: f6877c3e547d625cf58129a546dae798b37a24ae
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60469103"
---
# <a name="assign-azure-active-directory-admin-roles-using-powershell"></a>PowerShell kullanarak Azure Active Directory'de yönetici rolleri atama

Azure PowerShell kullanarak kullanıcı hesaplarına Rolleri Ata nasıl otomatik hale getirebilirsiniz. Bu makalede [Azure Active Directory PowerShell sürüm 2](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0#directory_roles) modülü.

## <a name="prepare-powershell"></a>PowerShell hazırlama

İlk olarak, şunları yapmalısınız [Azure AD PowerShell modülünü indirme](https://www.powershellgallery.com/packages/AzureAD/).

## <a name="install-the-azure-ad-powershell-module"></a>Azure AD PowerShell modülünü yükleme

Azure AD PowerShell modülünü yüklemek için aşağıdaki komutları kullanın:

```powershell
install-module azuread
import-module azuread
```

Modül kullanıma hazır olduğunu doğrulamak için aşağıdaki komutu kullanın:

```powershell
get-module azuread
  ModuleType Version      Name                         ExportedCommands
  ---------- ---------    ----                         ----------------
  Binary     2.0.0.115    azuread                      {Add-AzureADAdministrati...}
```

Artık modülünde cmdlet'leri kullanmaya başlayabilirsiniz. Azure AD modülündeki cmdlet'ler tam bir açıklaması için lütfen çevrimiçi başvuru belgelerine bakın [Azure Active Directory PowerShell sürüm 2](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0#directory_roles).

## <a name="permissions-required"></a>Gerekli izinler

Azure AD kiracınıza atamayı veya kaldırmayı roller genel yönetici hesabını kullanarak bağlanın.

## <a name="assign-a-single-role"></a>Tek bir rol atayın

Rol atamak için önce görünen adını ve atama rolün adını almanız gerekir. Hesabın görünen adını ve rolün adı varsa, kullanıcıya rol atamak için aşağıdaki cmdlet'leri kullanın.

``` PowerShell
# Fetch user to assign to role
$roleMember = Get-AzureADUser -ObjectId "username@contoso.com"

# Fetch User Account Administrator role instance
$role = Get-AzureADDirectoryRole | Where-Object {$_.displayName -eq 'User Account Administrator'}

# If role instance does not exist, instantiate it based on the role template
if ($role -eq $null) {
    # Instantiate an instance of the role template
    $roleTemplate = Get-AzureADDirectoryRoleTemplate | Where-Object {$_.displayName -eq 'User Account Administrator'}
    Enable-AzureADDirectoryRole -RoleTemplateId $roleTemplate.ObjectId

    # Fetch User Account Administrator role instance again
    $role = Get-AzureADDirectoryRole | Where-Object {$_.displayName -eq 'User Account Administrator'}
}

# Add user to role
Add-AzureADDirectoryRoleMember -ObjectId $role.ObjectId -RefObjectId $roleMember.ObjectId

# Fetch role membership for role to confirm
Get-AzureADDirectoryRoleMember -ObjectId $role.ObjectId | Get-AzureADUser
```

## <a name="assign-a-role-to-a-service-principal"></a>Bir hizmet sorumlusuna bir rol atayın

Bir hizmet sorumlusu, rol atama örneği.

```powershell
# Fetch a service principal to assign to role
$roleMember = Get-AzureADServicePrincipal -ObjectId "00221b6f-4387-4f3f-aa85-34316ad7f956"
 
#Fetch list of all directory roles with object ID
Get-AzureADDirectoryRole
 
# Fetch a directory role by ID
$role = Get-AzureADDirectoryRole -ObjectId "5b3fe201-fa8b-4144-b6f1-875829ff7543"
 
# Add user to role
Add-AzureADDirectoryRoleMember -ObjectId $role.ObjectId -RefObjectId $roleMember.ObjectId
 
# Fetch the assignment for the role to confirm
Get-AzureADDirectoryRoleMember -ObjectId $role.ObjectId | Get-AzureADServicePrincipal
```

## <a name="multiple-role-assignments"></a>Birden çok rol atamaları

Atama ve aynı anda birden çok rol kaldırma örnekleri.

```powershell
#File name
$fileName="<file path>" 
 
$input_Excel = New-Object -ComObject Excel.Application
$input_Workbook = $input_Excel.Workbooks.Open($fileName)
$input_Worksheet = $input_Workbook.Sheets.Item(1)
 
        #Count number of users who have to be assigned to role
$count = $input_Worksheet.UsedRange.Rows.Count
 
#Loop through each line of the csv file starting from line 2 (assuming first line is title)
for ($i=2; $i -le $count; $i++)
{
       #Fetch user display name
       $displayName = $input_Worksheet.Cells.Item($i,1).Text
       
       #Fetch role name
       $roleName = $input_Worksheet.Cells.Item($i,2).Text
 
       #Assign role
       Add-AzureADDirectoryRoleMember -ObjectId (Get-AzureADDirectoryRole | Where-Object DisplayName -eq $roleName).ObjectId -RefObjectId (Get-AzureADUser | Where-Object DisplayName -eq $displayName).ObjectId
}
 
#Remove multiple role assignments
for ($i=2; $i -le $count; $i++)
{
       $displayName = $input_Worksheet.Cells.Item($i,1).Text
       $roleName = $input_Worksheet.Cells.Item($i,2).Text
 
       Remove-AzureADDirectoryRoleMember -ObjectId (Get-AzureADDirectoryRole | Where-Object DisplayName -eq $roleName).ObjectId -MemberId (Get-AzureADUser | Where-Object DisplayName -eq $displayName).ObjectId
}
```

## <a name="remove-a-role-assignment"></a>Rol atamasını kaldırma

Bu örnek, belirtilen kullanıcı için rol ataması kaldırır.

```powershell
# Fetch user to assign to role
$roleMember = Get-AzureADUser -ObjectId "username@contoso.com"

#Fetch list of all directory roles with object id
Get-AzureADDirectoryRole
 
# Fetch a directory role by id
$role = Get-AzureADDirectoryRole -ObjectId "5b3fe201-fa8b-4144-b6f1-875829ff7543"
 
# Remove user from role
Remove-AzureADDirectoryRoleMember -ObjectId $role.ObjectId -MemberId $roleMember.ObjectId 

# Fetch role membership for role to confirm
Get-AzureADDirectoryRoleMember -ObjectId $role.ObjectId | Get-AzureADUser
 
```

## <a name="next-steps"></a>Sonraki adımlar

* Bizimle paylaşmayı rahatça [Azure AD yönetim rolleri Forumu](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=166032).
* Rolleri ve Yönetici rolü atama hakkında daha fazla bilgi için bkz. [yönetici rolleri atama](directory-assign-admin-roles.md).
* Varsayılan kullanıcı izinleri için bkz. bir [varsayılan Konuk ve üye kullanıcı izinlerini karşılaştırma](../fundamentals/users-default-permissions.md).
