---
title: Yönetilen hizmet kimliği (MSI) Azure CLI kullanarak bir kullanıcıyı yönetmek nasıl atanır
description: Adım adım oluşturmak, liste ve atanan kullanıcı silme hakkında yönergeler yönetilen Azure CLI kullanarak kimlik hizmeti.
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
ms.openlocfilehash: 5deaace49bfff994defc06a5f60597add6affc0b
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2018
ms.locfileid: "39188157"
---
# <a name="create-list-or-delete-a-user-assigned-identity-using-the-azure-cli"></a>Oluşturma, liste veya bir kullanıcı tarafından atanan kimliği Azure CLI kullanarak silme

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Yönetilen hizmet kimliği Azure Active Directory'deki yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimlik, kimlik bilgileri, kodunuzda gerek kalmadan Azure AD kimlik doğrulaması, Destek Hizmetleri kimlik doğrulaması için kullanabilirsiniz. 

Bu makalede, oluşturma, listeleme ve Azure CLI kullanarak bir kullanıcı tarafından atanan kimliği silme öğrenin.

## <a name="prerequisites"></a>Önkoşullar

- Yönetilen hizmet kimliği ile bilmiyorsanız, kullanıma [genel bakış bölümünde](overview.md). **Gözden geçirmeyi unutmayın [sistem tarafından atanan ve kullanıcı tarafından atanan kimliği arasındaki fark](overview.md#how-does-it-work)**.
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).
- Bu makalede yönetim işlemlerini gerçekleştirmek için aşağıdaki rol atamaları hesabınızın gerekir:
    - [Yönetilen kimlik Katılımcısı](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rolü oluşturmak için (liste) okuma, güncelleştirme ve bir kullanıcı tarafından atanan kimliği silinemiyor.
    - [Yönetilen kimlik işleci](/azure/role-based-access-control/built-in-roles#managed-identity-operator) (liste), bir kullanıcı tarafından atanan kimlik özelliklerini okumak için rol.
- CLI betiği örnekleri çalıştırmak için üç seçeneğiniz vardır:
    - Kullanım [Azure Cloud Shell](../../cloud-shell/overview.md) Azure portalından (sonraki bölüme bakın).
    - Katıştırılmış Azure Cloud Shell aracılığıyla her kod bloğunun sağ üst köşesinde bulunan "Try It" düğmesini kullanın.
    - [CLI 2. 0'ın en son sürümünü yüklemek](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.13 veya üzeri) yerel bir CLI konsol kullanmak istiyorsanız. Oturum açmak için Azure kullanarak `az login`, altında istediğiniz kullanıcı tarafından atanan kimlik dağıtmak Azure aboneliği ile ilişkili olan bir hesap kullanarak.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-a-user-assigned-managed-identity"></a>Yönetilen kimlik atanmış bir kullanıcı oluşturun 

Bir kullanıcı tarafından atanan kimliği oluşturmak için kullanın [az kimliği oluşturma](/cli/azure/identity#az-identity-create) komutu. `-g` Parametresi, kullanıcı tarafından atanan kimlik oluşturulacağı kaynak grubunu belirtir ve `-n` parametre adını belirtir. Değiştirin `<RESOURCE GROUP>` ve `<USER ASSIGNED IDENTITY NAME>` parametre değerlerini kendi değerlerinizle:

[!INCLUDE[ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

 ```azurecli-interactive
az identity create -g <RESOURCE GROUP> -n <USER ASSIGNED IDENTITY NAME>
```
## <a name="list-user-assigned-identities"></a>Liste kullanıcı kimlikleri atandı

Kullanıcı tarafından atanan kimlikleri listesinde, kullanmak için [az kimlik listesi](/cli/azure/identity#az-identity-list) komutu. Değiştirin `<RESOURCE GROUP>` kendi değerine sahip:

```azurecli-interactive
az identity list -g <RESOURCE GROUP>
```
Json yanıt olarak, kullanıcı kimliklerine sahip `"Microsoft.ManagedIdentity/userAssignedIdentities"` anahtarı için döndürülen değer `type`.

`"type": "Microsoft.ManagedIdentity/userAssignedIdentities"`

## <a name="delete-a-user-assigned-identity"></a>Bir kullanıcı tarafından atanan Kimliği Sil

Bir kullanıcı tarafından atanan kimliği silmek için kullanın [az kimlik Sil](/cli/azure/identity#az-identity-delete) komutu.  -N parametre adı, kullanıcı tarafından atanan kimliği oluşturulduğu kaynak grubu -g parametresi belirtir. Değiştirin `<USER ASSIGNED IDENTITY NAME>` ve `<RESOURCE GROUP>` parametrelerin değerleri kendi değerlerinizle:

 ```azurecli-interactive
az identity delete -n <USER ASSIGNED IDENTITY NAME> -g <RESOURCE GROUP>
```
> [!NOTE]
> Bir kullanıcı tarafından atanan kimliği siliniyor başvuru atanmış herhangi bir kaynaktan kaldırmaz. Lütfen VM/VMSS kullanarak bunları kaldırabiliriz `az vm/vmss identity remove` komutu

## <a name="related-content"></a>İlgili içerik

Azure CLI kimlik komutların tam listesi için bkz. [az kimlik](/cli/azure/identity).

Bir Azure VM görmek, bir kullanıcı tarafından atanan kimliği atama hakkında bilgi için [Yapılandırma Yönetilen hizmet kimliği (Azure CLI kullanarak MSI)](qs-configure-cli-windows-vm.md#user-assigned-identity)


 
