---
title: Azure CLI kullanarak bir kullanıcı tarafından atanan yönetilen kimlik yönetme
description: Adım adım yönergeler oluşturmak, liste ve bir kullanıcı tarafından atanan silmek yönetilen Azure CLI kullanarak kimliği.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/16/2018
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 28520b3ba5d4e62fd4e1c9b78e68cc7dc2b48c61
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58447416"
---
# <a name="create-list-or-delete-a-user-assigned-managed-identity-using-the-azure-cli"></a>Oluşturma, liste veya Azure CLI kullanarak bir kullanıcı tarafından atanan yönetilen kimlik silme

[!INCLUDE [preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Azure kaynakları için yönetilen kimlikleri Azure Active Directory'deki yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimlik, kimlik bilgileri, kodunuzda gerek kalmadan Azure AD kimlik doğrulaması, Destek Hizmetleri kimlik doğrulaması için kullanabilirsiniz. 

Bu makalede, oluşturmak, liste ve Azure CLI kullanarak bir kullanıcı tarafından atanan yönetilen kimlik Sil öğrenin.

## <a name="prerequisites"></a>Önkoşullar

- Azure kaynakları için yönetilen kimliklerle bilmiyorsanız kullanıma [genel bakış bölümünde](overview.md). **Gözden geçirmeyi unutmayın [sistem tarafından atanan ve kullanıcı tarafından atanan bir yönetilen kimlik arasındaki farkı](overview.md#how-does-it-work)**.
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).
- CLI betiği örnekleri çalıştırmak için üç seçeneğiniz vardır:
    - Kullanım [Azure Cloud Shell](../../cloud-shell/overview.md) Azure portalından (sonraki bölüme bakın).
    - Katıştırılmış Azure Cloud Shell aracılığıyla her kod bloğunun sağ üst köşesinde bulunan "Try It" düğmesini kullanın.
    - [Azure CLI'ın en son sürümü yükleyin](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.13 veya üzeri) yerel bir CLI konsol kullanmak istiyorsanız. Oturum açmak için Azure kullanarak `az login`, altında istediğiniz kullanıcı tarafından atanan dağıtmak Azure aboneliği ile ilişkili olan bir hesap kullanarak yönetilen kimliği.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-a-user-assigned-managed-identity"></a>Kullanıcı tarafından atanan yönetilen kimlik oluşturma 

Kullanıcı tarafından atanan bir yönetilen kimlik oluşturmak için hesabınızın gerekli [yönetilen kimlik Katılımcısı](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rol ataması.

Kullanım [az kimliği oluşturma](/cli/azure/identity#az-identity-create) yönetilen kimliği bir kullanıcı tarafından atanan oluşturmak için komutu. `-g` Parametresi, kullanıcı tarafından atanan yönetilen kimlik, oluşturulacağı kaynak grubunu belirtir ve `-n` parametre adını belirtir. Değiştirin `<RESOURCE GROUP>` ve `<USER ASSIGNED IDENTITY NAME>` parametre değerlerini kendi değerlerinizle:

[!INCLUDE [ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

 ```azurecli-interactive
az identity create -g <RESOURCE GROUP> -n <USER ASSIGNED IDENTITY NAME>
```
## <a name="list-user-assigned-managed-identities"></a>Kullanıcı tarafından atanan yönetilen kimlikleri listesi

Kullanıcı tarafından atanan bir yönetilen kimlik listesi/okuma için hesabınızın gerekir [yönetilen kimlik işleci](/azure/role-based-access-control/built-in-roles#managed-identity-operator) veya [yönetilen kimlik Katılımcısı](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rol ataması.

Kullanıcı tarafından atanan yönetilen kimlikleri listesinde, kullanmak için [az kimlik listesi](/cli/azure/identity#az-identity-list) komutu. Değiştirin `<RESOURCE GROUP>` kendi değerine sahip:

```azurecli-interactive
az identity list -g <RESOURCE GROUP>
```
Json yanıt olarak, kullanıcı tarafından atanan yönetilen kimliklerine sahip `"Microsoft.ManagedIdentity/userAssignedIdentities"` anahtarı için döndürülen değer `type`.

`"type": "Microsoft.ManagedIdentity/userAssignedIdentities"`

## <a name="delete-a-user-assigned-managed-identity"></a>Kullanıcı tarafından atanan bir yönetilen kimlik Sil

Kullanıcı tarafından atanan bir yönetilen kimlik silmek için hesabınızın gerekli [yönetilen kimlik Katılımcısı](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rol ataması.

Kullanıcı tarafından atanan bir yönetilen kimlik silmek için kullanın [az kimlik Sil](/cli/azure/identity#az-identity-delete) komutu.  -N parametre adı, kullanıcı tarafından atanan bir yönetilen kimlik oluşturulduğu kaynak grubu -g parametresi belirtir. Değiştirin `<USER ASSIGNED IDENTITY NAME>` ve `<RESOURCE GROUP>` parametrelerin değerleri kendi değerlerinizle:

 ```azurecli-interactive
az identity delete -n <USER ASSIGNED IDENTITY NAME> -g <RESOURCE GROUP>
```
> [!NOTE]
> Kullanıcı tarafından atanan bir yönetilen kimlik silme başvuru atanmış herhangi bir kaynaktan kaldırmaz. Lütfen VM/VMSS kullanarak bunları kaldırabiliriz `az vm/vmss identity remove` komutu

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI kimlik komutların tam listesi için bkz. [az kimlik](/cli/azure/identity).

Bir kullanıcı tarafından atanan atama hakkında bilgi için bir Azure VM bakın kimliğine yönetilen [yapılandırma kimliklerini Azure CLI kullanarak bir Azure sanal makinesinde Azure kaynakları için yönetilen](qs-configure-cli-windows-vm.md#user-assigned-managed-identity)


 
