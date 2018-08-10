---
title: Oluşturma, liste ve Azure PowerShell kullanarak bir kullanıcıya atanan (MSI) Sil
description: Adım adım yönergeler oluşturmak, liste ve kullanıcıyı silmek yönetilen hizmet kimliği Azure PowerShell kullanarak atanmış.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/16/2018
ms.author: daveba
ms.openlocfilehash: 31a632138a4946accfcab858b7b61782fb4e7d72
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2018
ms.locfileid: "40005380"
---
# <a name="create-list-or-delete-a-user-assigned-identity-using-azure-powershell"></a>Oluşturma, liste veya Azure PowerShell kullanarak bir kullanıcı tarafından atanan kimliği silme

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Yönetilen hizmet kimliği Azure Active Directory'deki yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimlik, kimlik bilgileri, kodunuzda gerek kalmadan Azure AD kimlik doğrulaması, Destek Hizmetleri kimlik doğrulaması için kullanabilirsiniz. 

Bu makalede, oluşturma, listeleme ve Azure PowerShell kullanarak bir kullanıcı tarafından atanan kimliği silme öğrenin.

## <a name="prerequisites"></a>Önkoşullar

- Yönetilen hizmet kimliği ile bilmiyorsanız, kullanıma [genel bakış bölümünde](overview.md). **Gözden geçirmeyi unutmayın [sistem tarafından atanan ve kullanıcı tarafından atanan kimliği arasındaki fark](overview.md#how-does-it-work)**.
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).
- Yükleme [Azure PowerShell'in en son sürümünü](https://www.powershellgallery.com/packages/AzureRM) henüz yapmadıysanız.
- PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz Bu öğretici Azure PowerShell modülü sürüm 5.7.0 gerektirir veya üzeri. Sürümü bulmak için ` Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Login-AzureRmAccount` komutunu da çalıştırmanız gerekir.
- Bu makalede yönetim işlemlerini gerçekleştirmek için aşağıdaki rol atamaları hesabınızın gerekir:
    - [Yönetilen kimlik Katılımcısı](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rolü oluşturmak için (liste) okuma, güncelleştirme ve bir kullanıcı tarafından atanan kimliği silinemiyor.
    - [Yönetilen kimlik işleci](/azure/role-based-access-control/built-in-roles#managed-identity-operator) (liste), bir kullanıcı tarafından atanan kimlik özelliklerini okumak için rol.

> [!NOTE]
> Kullanıcı atanmış durumdayken Kimlikleridir hala Önizleme AzureRM.ManagedServiceIdentity Modülü aşağıdaki komutu kullanarak el ile yüklemeniz gerekir. 
```azurepowershell-interactive
Install-Module -Name AzureRM.ManagedServiceIdentity -AllowPrerelease
```

## <a name="create-a-user-assigned-identity"></a>Kullanıcı tarafından atanan kimliği oluşturma

Bir kullanıcı tarafından atanan kimliği oluşturmak için kullanın [yeni AzureRmUserAssignedIdentity](/powershell/module/azurerm.managedserviceidentity/new-azurermuserassignedidentity) komutu. `ResourceGroupName` Parametresi, kullanıcı tarafından atanan kimlik oluşturulacağı kaynak grubunu belirtir ve `-Name` parametre adını belirtir. Değiştirin `<RESOURCE GROUP>` ve `<USER ASSIGNED IDENTITY NAME>` parametre değerlerini kendi değerlerinizle:

[!INCLUDE[ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

 ```azurepowershell-interactive
New-AzureRmUserAssignedIdentity -ResourceGroupName <RESOURCEGROUP> -Name <USER ASSIGNED IDENTITY NAME>
```
## <a name="list-user-assigned-identities"></a>Liste kullanıcı kimlikleri atandı

Kullanıcı tarafından atanan kimlikleri listesinde, kullanmak için [Get-AzureRmUserAssigned](/powershell/module/azurerm.managedserviceidentity/get-azurermuserassignedidentity) komutu.  `-ResourceGroupName` Parametresi, kullanıcı tarafından atanan kimliği oluşturulduğu kaynak grubunu belirtir. Değiştirin `<RESOURCE GROUP>` kendi değerine sahip:

```azurepowershell-interactive
Get-AzureRmUserAssignedIdentity -ResourceGroupName <RESOURCE GROUP>
```
Yanıt olarak, kullanıcı kimliklerine sahip `"Microsoft.ManagedIdentity/userAssignedIdentities"` anahtarı için döndürülen değer `Type`.

`Type :Microsoft.ManagedIdentity/userAssignedIdentities`

## <a name="delete-a-user-assigned-identity"></a>Bir kullanıcı tarafından atanan Kimliği Sil

Bir kullanıcı kimliği silmek için kullanın [Remove-AzureRmUserAssignedIdentity](/powershell/module/azurerm.managedserviceidentity/remove-azurermuserassignedidentity) komutu.  `-ResourceGroupName` Parametresi, kullanıcı tarafından atanan kimliği oluşturulduğu kaynak grubu belirtir ve `-Name` parametre adını belirtir. Değiştirin `<RESOURCE GROUP>` ve `<USER ASSIGNED IDENTITY NAME>` parametrelerin değerleri kendi değerlerinizle:

 ```azurepowershell-interactive
Remove-AzurRmUserAssignedIdentity -ResourceGroupName <RESOURCE GROUP> -Name <USER ASSIGNED IDENTITY NAME>
```
> [!NOTE]
> Bir kullanıcı tarafından atanan kimliği siliniyor başvuru atanmış herhangi bir kaynaktan kaldırmaz. Kimlik atamaları ayrı olarak kaldırılması gerekir.

## <a name="related-content"></a>İlgili içerik

Tam bir listesi ve Azure PowerShell'i MSI komutlar hakkında daha fazla ayrıntı için bkz: [AzureRM.ManagedServiceIdentity](/powershell/module/azurerm.managedserviceidentity#managed_service_identity).
