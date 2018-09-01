---
title: Azure CLI kullanarak bir kullanıcı tarafından atanan yönetilen kimlik yönetme
description: Adım adım yönergeler oluşturmak, liste ve bir kullanıcı tarafından atanan silmek yönetilen Azure CLI kullanarak kimliği.
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
ms.openlocfilehash: 0f0f5446dc2f3fd698bc825785f4aa44383ef2ba
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/31/2018
ms.locfileid: "43338182"
---
# <a name="create-list-or-delete-a-user-assigned-managed-identity-using-the-azure-cli"></a>Oluşturma, liste veya Azure CLI kullanarak bir kullanıcı tarafından atanan yönetilen kimlik silme

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Azure kaynakları için yönetilen kimlikleri Azure Active Directory'deki yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimlik, kimlik bilgileri, kodunuzda gerek kalmadan Azure AD kimlik doğrulaması, Destek Hizmetleri kimlik doğrulaması için kullanabilirsiniz. 

Bu makalede, oluşturmak, liste ve Azure CLI kullanarak bir kullanıcı tarafından atanan yönetilen kimlik Sil öğrenin.

## <a name="prerequisites"></a>Önkoşullar

- Azure kaynakları için yönetilen kimliklerle bilmiyorsanız kullanıma [genel bakış bölümünde](overview.md). **Gözden geçirmeyi unutmayın [sistem tarafından atanan ve kullanıcı tarafından atanan bir yönetilen kimlik arasındaki farkı](overview.md#how-does-it-work)**.
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).
- Bu makalede yönetim işlemlerini gerçekleştirmek için aşağıdaki rol atamaları hesabınızın gerekir:
    - [Yönetilen kimlik Katılımcısı](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rolü oluşturmak için (liste) okuma, güncelleştirme ve kullanıcı tarafından atanan bir yönetilen kimlik silin.
    - [Yönetilen kimlik işleci](/azure/role-based-access-control/built-in-roles#managed-identity-operator) rol (liste) kullanıcı tarafından atanan bir yönetilen kimlik özelliklerini okuyun.
- CLI betiği örnekleri çalıştırmak için üç seçeneğiniz vardır:
    - Kullanım [Azure Cloud Shell](../../cloud-shell/overview.md) Azure portalından (sonraki bölüme bakın).
    - Katıştırılmış Azure Cloud Shell aracılığıyla her kod bloğunun sağ üst köşesinde bulunan "Try It" düğmesini kullanın.
    - [CLI 2. 0'ın en son sürümünü yüklemek](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.13 veya üzeri) yerel bir CLI konsol kullanmak istiyorsanız. Oturum açmak için Azure kullanarak `az login`, altında istediğiniz kullanıcı tarafından atanan dağıtmak Azure aboneliği ile ilişkili olan bir hesap kullanarak yönetilen kimliği.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-a-user-assigned-managed-identity"></a>Kullanıcı tarafından atanan bir yönetilen kimlik oluşturma 

Kullanıcı tarafından atanan bir yönetilen kimlik oluşturmak üzere kullanmak [az kimliği oluşturma](/cli/azure/identity#az-identity-create) komutu. `-g` Parametresi, kullanıcı tarafından atanan yönetilen kimlik, oluşturulacağı kaynak grubunu belirtir ve `-n` parametre adını belirtir. Değiştirin `<RESOURCE GROUP>` ve `<USER ASSIGNED IDENTITY NAME>` parametre değerlerini kendi değerlerinizle:

[!INCLUDE[ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

 ```azurecli-interactive
az identity create -g <RESOURCE GROUP> -n <USER ASSIGNED IDENTITY NAME>
```
## <a name="list-user-assigned-managed-identities"></a>Kullanıcı tarafından atanan yönetilen kimlikleri listesi

Kullanıcı tarafından atanan yönetilen kimlikleri listesinde, kullanmak için [az kimlik listesi](/cli/azure/identity#az-identity-list) komutu. Değiştirin `<RESOURCE GROUP>` kendi değerine sahip:

```azurecli-interactive
az identity list -g <RESOURCE GROUP>
```
Json yanıt olarak, kullanıcı tarafından atanan yönetilen kimliklerine sahip `"Microsoft.ManagedIdentity/userAssignedIdentities"` anahtarı için döndürülen değer `type`.

`"type": "Microsoft.ManagedIdentity/userAssignedIdentities"`

## <a name="delete-a-user-assigned-managed-identity"></a>Kullanıcı tarafından atanan bir yönetilen kimlik Sil

Kullanıcı tarafından atanan bir yönetilen kimlik silmek için kullanın [az kimlik Sil](/cli/azure/identity#az-identity-delete) komutu.  -N parametre adı, kullanıcı tarafından atanan bir yönetilen kimlik oluşturulduğu kaynak grubu -g parametresi belirtir. Değiştirin `<USER ASSIGNED IDENTITY NAME>` ve `<RESOURCE GROUP>` parametrelerin değerleri kendi değerlerinizle:

 ```azurecli-interactive
az identity delete -n <USER ASSIGNED IDENTITY NAME> -g <RESOURCE GROUP>
```
> [!NOTE]
> Kullanıcı tarafından atanan bir yönetilen kimlik silme başvuru atanmış herhangi bir kaynaktan kaldırmaz. Lütfen VM/VMSS kullanarak bunları kaldırabiliriz `az vm/vmss identity remove` komutu

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI kimlik komutların tam listesi için bkz. [az kimlik](/cli/azure/identity).

Bir kullanıcı tarafından atanan atama hakkında bilgi için bir Azure VM bakın kimliğine yönetilen [yapılandırma kimliklerini Azure CLI kullanarak bir Azure sanal makinesinde Azure kaynakları için yönetilen](qs-configure-cli-windows-vm.md#user-assigned-managed-identity)


 
