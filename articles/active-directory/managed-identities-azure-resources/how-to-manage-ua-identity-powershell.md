---
title: Oluşturma, liste ve Azure PowerShell kullanarak bir kullanıcı tarafından atanan yönetilen kimlik Sil
description: Adım adım yönergeler oluşturmak, liste ve kullanıcı tarafından atanan silmek yönetilen Azure PowerShell kullanarak kimliği.
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
ms.openlocfilehash: bbac82b272b7f7beea6e43f000d265ab6f87c796
ms.sourcegitcommit: d372d75558fc7be78b1a4b42b4245f40f213018c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/09/2018
ms.locfileid: "51300532"
---
# <a name="create-list-or-delete-a-user-assigned-managed-identity-using-azure-powershell"></a>Oluşturma, liste veya Azure PowerShell kullanarak bir kullanıcı tarafından atanan yönetilen kimlik silme

[!INCLUDE [preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Azure kaynakları için yönetilen kimlikleri Azure Active Directory'deki yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimlik, kimlik bilgileri, kodunuzda gerek kalmadan Azure AD kimlik doğrulaması, Destek Hizmetleri kimlik doğrulaması için kullanabilirsiniz. 

Bu makalede, oluşturmak, liste ve Azure PowerShell kullanarak bir kullanıcı tarafından atanan yönetilen kimlik Sil öğrenin.

## <a name="prerequisites"></a>Önkoşullar

- Azure kaynakları için yönetilen kimliklerle bilmiyorsanız kullanıma [genel bakış bölümünde](overview.md). **Gözden geçirmeyi unutmayın [sistem tarafından atanan ve kullanıcı tarafından atanan bir yönetilen kimlik arasındaki farkı](overview.md#how-does-it-work)**.
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).
- Yükleme [Azure PowerShell'in en son sürümünü](https://www.powershellgallery.com/packages/AzureRM) henüz yapmadıysanız.
- PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici, Azure PowerShell modülü 5.7.0 veya sonraki bir sürümünü gerektirir. Sürümü bulmak için ` Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). 
- PowerShell'i yerel ortamda çalıştırıyorsanız şunları da yapmanız gerekir: 
    - Azure ile bağlantı oluşturmak için `Login-AzureRmAccount` komutunu çalıştırın.
    - [PowerShellGet'in en son sürümünü](/powershell/gallery/installing-psget#for-systems-with-powershell-50-or-newer-you-can-install-the-latest-powershellget) yükleyin.
    - `Install-Module -Name PowerShellGet -AllowPrerelease` komutunu çalıştırarak `PowerShellGet` modülünün yayın öncesi sürümünü alın (`AzureRM.ManagedServiceIdentity` modülünü yüklemek için bu komutu çalıştırdıktan sonra geçerli PowerShell oturumundan `Exit` ile çıkmanız gerekebilir).
    - Çalıştırma `Install-Module -Name AzureRM.ManagedServiceIdentity -AllowPrerelease` bir ön sürümünü yüklemek için `AzureRM.ManagedServiceIdentity` modülü kullanıcı tarafından atanan gerçekleştirmek için bu makaledeki kimlik işlemleri yönetilen.

## <a name="create-a-user-assigned-managed-identity"></a>Kullanıcı tarafından atanan yönetilen kimlik oluşturma

Kullanıcı tarafından atanan bir yönetilen kimlik oluşturmak için hesabınızın gerekli [yönetilen kimlik Katılımcısı](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rol ataması.

Kullanıcı tarafından atanan bir yönetilen kimlik oluşturmak üzere kullanmak [yeni AzureRmUserAssignedIdentity](/powershell/module/azurerm.managedserviceidentity/new-azurermuserassignedidentity) komutu. `ResourceGroupName` Parametresi, kullanıcı tarafından atanan yönetilen kimlik, oluşturulacağı kaynak grubunu belirtir ve `-Name` parametre adını belirtir. Değiştirin `<RESOURCE GROUP>` ve `<USER ASSIGNED IDENTITY NAME>` parametre değerlerini kendi değerlerinizle:

[!INCLUDE [ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

 ```azurepowershell-interactive
New-AzureRmUserAssignedIdentity -ResourceGroupName <RESOURCEGROUP> -Name <USER ASSIGNED IDENTITY NAME>
```
## <a name="list-user-assigned-managed-identities"></a>Kullanıcı tarafından atanan yönetilen kimlikleri listesi

Kullanıcı tarafından atanan bir yönetilen kimlik listesi/okuma için hesabınızın gerekir [yönetilen kimlik işleci](/azure/role-based-access-control/built-in-roles#managed-identity-operator) veya [yönetilen kimlik Katılımcısı](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rol ataması.

Kullanıcı tarafından atanan yönetilen kimlikleri listesinde, kullanmak için [Get-AzureRmUserAssigned](/powershell/module/azurerm.managedserviceidentity/get-azurermuserassignedidentity) komutu.  `-ResourceGroupName` Parametresi, kullanıcı tarafından atanan bir yönetilen kimlik oluşturulduğu kaynak grubunu belirtir. Değiştirin `<RESOURCE GROUP>` kendi değerine sahip:

```azurepowershell-interactive
Get-AzureRmUserAssignedIdentity -ResourceGroupName <RESOURCE GROUP>
```
Yanıt olarak, kullanıcı tarafından atanan yönetilen kimliklerine sahip `"Microsoft.ManagedIdentity/userAssignedIdentities"` anahtarı için döndürülen değer `Type`.

`Type :Microsoft.ManagedIdentity/userAssignedIdentities`

## <a name="delete-a-user-assigned-managed-identity"></a>Kullanıcı tarafından atanan bir yönetilen kimlik Sil

Kullanıcı tarafından atanan bir yönetilen kimlik silmek için hesabınızın gerekli [yönetilen kimlik Katılımcısı](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rol ataması.

Kullanıcı tarafından atanan bir yönetilen kimlik silmek için kullanın [Remove-AzureRmUserAssignedIdentity](/powershell/module/azurerm.managedserviceidentity/remove-azurermuserassignedidentity) komutu.  `-ResourceGroupName` Parametresi, kullanıcı tarafından atanan kimlik oluşturulduğu kaynak grubu belirtir ve `-Name` parametre adını belirtir. Değiştirin `<RESOURCE GROUP>` ve `<USER ASSIGNED IDENTITY NAME>` parametrelerin değerleri kendi değerlerinizle:

 ```azurepowershell-interactive
Remove-AzurRmUserAssignedIdentity -ResourceGroupName <RESOURCE GROUP> -Name <USER ASSIGNED IDENTITY NAME>
```
> [!NOTE]
> Kullanıcı tarafından atanan bir yönetilen kimlik silme başvuru atanmış herhangi bir kaynaktan kaldırmaz. Kimlik atamaları ayrı olarak kaldırılması gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Azure kaynaklarını komutları için yönetilen kimlikleri tam bir listesi ve Azure PowerShell ile ilgili daha fazla ayrıntı için bkz: [AzureRM.ManagedServiceIdentity](/powershell/module/azurerm.managedserviceidentity#managed_service_identity).
