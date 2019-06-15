---
title: Azure PowerShell kullanarak Azure kaynakları için atamaları izin verilmeyenler listesi | Microsoft Docs
description: Kullanıcılar, gruplar, hizmet sorumluları ve Azure PowerShell kullanarak belirli kapsamları, belirli bir Azure kaynak eylemlerine erişim reddedildi yönetilen kimlikleri listesi öğrenin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/12/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: c1ea26fdb4d60262f89ea6ab0f87220a08c01e68
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67110491"
---
# <a name="list-deny-assignments-for-azure-resources-using-azure-powershell"></a>Azure PowerShell kullanarak Azure kaynakları için atamaları izin verilmeyenler listesi

[Atamalar Reddet](deny-assignments.md) bir rol ataması bunları erişim verse bile kullanıcıların belirli bir Azure kaynak eylemler gerçekleştirme. Bu makalede nasıl listelemek Azure PowerShell kullanarak atamaları reddet.

> [!NOTE]
> Doğrudan kendi oluşturamazsınız atamaları reddet. Hakkında bilgi reddetmek için atamaları oluşturulur bkz [atamaları Reddet](deny-assignments.md).

## <a name="prerequisites"></a>Önkoşullar

Bir reddetme atama hakkında bilgi edinmek için şunlara sahip olmalısınız:

- `Microsoft.Authorization/denyAssignments/read` çoğu dahil izni [Azure kaynakları için yerleşik roller](built-in-roles.md)
- [Azure Cloud shell'de PowerShell](/azure/cloud-shell/overview) veya [Azure PowerShell](/powershell/azure/install-az-ps)

## <a name="list-deny-assignments"></a>Atamalar izin verilmeyenler listesi

### <a name="list-all-deny-assignments"></a>İzin verilmeyenler listesi tüm atamaları

Tümünü Reddet kullanın geçerli abonelik atamalarını listelemek için [Get-AzDenyAssignment](/powershell/module/az.resources/get-azdenyassignment).

```azurepowershell
Get-AzDenyAssignment
```

```Example
PS C:\> Get-AzDenyAssignment

Id                      : 22222222-2222-2222-2222-222222222222
DenyAssignmentName      : Deny assignment '22222222-2222-2222-2222-222222222222' created by Blueprint Assignment
                          '/subscriptions/11111111-1111-1111-1111-111111111111/providers/Microsoft.Blueprint/blueprintAssignments/assignment-locked-storageaccount-TestingBPLocks'.
Description             : Created by Blueprint Assignment '/subscriptions/11111111-1111-1111-1111-111111111111/providers/Microsoft.Blueprint/blueprintAssignments/assignment-locked-storageaccount-TestingBPLocks'.
Actions                 : {*}
NotActions              : {*/read}
DataActions             : {}
NotDataActions          : {}
Scope                   : /subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/TestingBPLocks
DoNotApplyToChildScopes : True
Principals              : {
                          DisplayName:  All Principals
                          ObjectType:   SystemDefined
                          ObjectId:     00000000-0000-0000-0000-000000000000
                          }
ExcludePrincipals       : {
                          ObjectType:   ServicePrincipal
                          }
IsSystemProtected       : True

Id                      : 33333333-3333-3333-3333-333333333333
DenyAssignmentName      : Deny assignment '33333333-3333-3333-3333-333333333333' created by Blueprint Assignment
                          '/subscriptions/11111111-1111-1111-1111-111111111111/providers/Microsoft.Blueprint/blueprintAssignments/assignment-locked-storageaccount-TestingBPLocks'.
Description             : Created by Blueprint Assignment '/subscriptions/11111111-1111-1111-1111-111111111111/providers/Microsoft.Blueprint/blueprintAssignments/assignment-locked-storageaccount-TestingBPLocks'.
Actions                 : {*}
NotActions              : {*/read}
DataActions             : {}
NotDataActions          : {}
Scope                   : /subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/TestingBPLocks/providers/Microsoft.Storage/storageAccounts/storep6vkuxmu4m4pq
DoNotApplyToChildScopes : True
Principals              : {
                          DisplayName:  All Principals
                          ObjectType:   SystemDefined
                          ObjectId:     00000000-0000-0000-0000-000000000000
                          }
ExcludePrincipals       : {
                          DisplayName:  assignment-locked-storageaccount-TestingBPLocks
                          ObjectType:   ServicePrincipal
                          ObjectId:     2311a0b7-657a-4ca2-af6f-d1c33f6d2fff
                          }
IsSystemProtected       : True
```

### <a name="list-deny-assignments-at-a-resource-group-scope"></a>Bir kaynak grubu kapsamda atamaları izin verilmeyenler listesi

Tümünü Reddet atamalarını kapsamda bir kaynak grubu, kullanım liste [Get-AzDenyAssignment](/powershell/module/az.resources/get-azdenyassignment).

```azurepowershell
Get-AzDenyAssignment -ResourceGroupName <resource_group_name>
```

```Example
PS C:\> Get-AzDenyAssignment -ResourceGroupName TestingBPLocks | FL DenyAssignmentName, Scope

DenyAssignmentName : Deny assignment '22222222-2222-2222-2222-222222222222' created by Blueprint Assignment
                     '/subscriptions/11111111-1111-1111-1111-111111111111/providers/Microsoft.Blueprint/blueprintAssignments/assignment-locked-storageaccount-TestingBPLocks'.
Scope              : /subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/TestingBPLocks
Principals         : {
                     DisplayName:       All Principals
                     ObjectType:        SystemDefined
                     ObjectId:  00000000-0000-0000-0000-000000000000
                     }
```

### <a name="list-deny-assignments-at-a-subscription-scope"></a>Bir abonelik kapsamda atamaları izin verilmeyenler listesi

Listelemek için bir abonelik kapsamda kullanım atamaları tümünü Reddet [Get-AzDenyAssignment](/powershell/module/az.resources/get-azdenyassignment). Abonelik Kimliğini almak için bunu bulabileceğiniz **abonelikleri** dikey penceresinde Azure portalını veya kullanabilir [Get-AzSubscription](/powershell/module/Az.Accounts/Get-AzSubscription).

```azurepowershell
Get-AzDenyAssignment -Scope /subscriptions/<subscription_id>
```

```Example
PS C:\> Get-AzDenyAssignment -Scope /subscriptions/11111111-1111-1111-1111-111111111111
```

## <a name="next-steps"></a>Sonraki adımlar

- [Anlamak Azure kaynakları için atamaları Reddet](deny-assignments.md)
- [Azure portalını kullanarak Azure kaynakları için atamaları izin verilmeyenler listesi](deny-assignments-portal.md)
- [REST API kullanarak Azure kaynakları için atamaları izin verilmeyenler listesi](deny-assignments-rest.md)