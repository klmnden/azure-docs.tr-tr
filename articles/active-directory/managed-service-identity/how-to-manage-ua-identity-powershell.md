---
title: Oluşturma, liste ve Azure PowerShell kullanarak bir kullanıcıya atanan (MSI) Sil
description: Adım adım yönergeler oluşturmak, liste ve kullanıcıyı silmek yönetilen hizmet Azure PowerShell kullanarak kimliği atanır.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/16/2018
ms.author: daveba
ms.openlocfilehash: 0f4041bd34a0b4978d820b64b45afd1f155cd6ab
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="create-list-or-delete-a-user-assigned-identity-using-azure-powershell"></a>Oluşturma, liste veya Azure PowerShell kullanarak bir kullanıcı tarafından atanan kimlik silme

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Yönetilen hizmet kimliği Azure Active Directory'deki yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimlik, Azure AD kimlik doğrulaması, kimlik bilgileri, kodunuzda gerek kalmadan destekleyen hizmetlerin kimlik doğrulaması için kullanabilirsiniz. 

Bu makalede, oluşturma, liste ve Azure PowerShell kullanarak bir kullanıcı tarafından atanan kimlik silme öğrenin.

## <a name="prerequisites"></a>Önkoşullar

- Yönetilen hizmet kimliği ile emin değilseniz, kullanıma [genel bakış bölümünde](overview.md). **Gözden geçirmeyi unutmayın [atanan sistemi ve kullanıcı kimliği atanır arasındaki farkı](overview.md#how-does-it-work)**.
- Bir Azure hesabınız yoksa [ücretsiz bir hesap için kaydolun](https://azure.microsoft.com/free/) devam etmeden önce.
- Yükleme [Azure PowerShell'in en son sürümünü](https://www.powershellgallery.com/packages/AzureRM) yapmadıysanız.
- Yüklemek ve PowerShell yerel olarak kullanmak seçerseniz, bu öğreticide Azure PowerShell modülü sürümü 5.7.0 gerektirir veya sonraki bir sürümü. Sürümü bulmak için ` Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Login-AzureRmAccount` komutunu da çalıştırmanız gerekir.

## <a name="create-a-user-assigned-identity"></a>Bir kullanıcı kimliği atanır oluşturun

Bir kullanıcı tarafından atanan kimlik oluşturmak üzere kullanmak [yeni AzureRmUserAssignedIdentity](/powershell/module/azurerm.managedserviceidentity/new-azurermuserassignedidentity) komutu. `ResourceGroupName` Parametresi, kaynak grubuna atanan kullanıcı kimliğini oluşturulacağı yeri belirtir ve `-Name` parametresi adını belirtir. Değiştir `<RESOURCE GROUP>` ve `<USER ASSIGNED IDENTITY NAME>` parametre değerlerini kendi değerlerinizi ile:

> [!IMPORTANT]
> Kullanıcı adında özel karakterler (örneğin, alt çizgi) kimliklerle atanan oluşturma şu anda desteklenmiyor. Lütfen alfasayısal karakterler kullanın. Geri güncelleştirmeleri denetleyin.  Daha fazla bilgi için bkz: [SSS ve bilinen sorunlar](known-issues.md).

 ```azurepowershell-interactive
New-AzureRmUserAssignedIdentity -ResourceGroupName <RESOURCEGROUP> -Name <USER ASSIGNED IDENTITY NAME>
```
## <a name="list-user-assigned-identities"></a>Liste kullanıcı kimlikleri atandı

Atanan kullanıcı kimlikleri listesinden, kullanmak için [Get-AzureRmUserAssigned](/powershell/module/azurerm.managedserviceidentity/get-azurermuserassignedidentity) komutu.  `-ResourceGroupName` Parametresi, kullanıcı kimliği atanır oluşturulduğu kaynak grubu belirtir.  Değiştir `<RESOURCE GROUP>` kendi değerine sahip:

```azurepowershell-interactive
Get-AzureRmUserAssignedIdentity -ResourceGroupName <RESOURCE GROUP>
```
Yanıtta, kullanıcı kimliklerini sahip `"Microsoft.ManagedIdentity/userAssignedIdentities"` anahtar için döndürülen değer `Type`.

`Type :Microsoft.ManagedIdentity/userAssignedIdentities`

## <a name="delete-a-user-assigned-identity"></a>Bir kullanıcı kimliği atanır Sil

Bir kullanıcı kimliği silmek için kullanın [Kaldır AzureRmUserAssignedIdentity](/powershell/module/azurerm.managedserviceidentity/remove-azurermuserassignedidentity) komutu.  `-ResourceGroupName` Parametresi, kullanıcı kimliği atanır oluşturulduğu kaynak grubunun belirtir ve `-Name` parametresi adını belirtir.  Değiştir `<RESOURCE GROUP>` ve `<USER ASSIGNED IDENTITY NAME>` kendi değerlerinizi parametreleri değerlerle:

 ```azurecli-interactive
Remove-AzurRmUserAssignedIdentity -ResourceGroupName <RESOURCE GROUP> -Name <USER ASSIGNED IDENTITY NAME>
```
> [!NOTE]
> Bir kullanıcı kimliği atanır silme başvuru atanıp atanmadığını kaynaktan kaldırmaz. Kimlik atamaları ayrı olarak kaldırılması gerekir.

## <a name="related-content"></a>İlgili içerik

Tam bir listesi ve Azure PowerShell MSI komutları ilgili daha fazla ayrıntı için bkz: [AzureRM.ManagedServiceIdentity](/powershell/module/azurerm.managedserviceidentity#managed_service_identity).


 
