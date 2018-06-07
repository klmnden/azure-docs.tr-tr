---
title: Yönetilen hizmet kimliği (MSI) Azure CLI kullanarak kullanıcı yönetme atanan
description: Adım adım yönergeler oluşturma, liste ve atanmış bir kullanıcı silme hizmeti Azure CLI kullanarak kimlik yönetilen.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/16/2018
ms.author: daveba
ms.openlocfilehash: 24172ebac8c7f124d0873b9d93d260fa2e1a8a44
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34594620"
---
# <a name="create-list-or-delete-a-user-assigned-identity-using-the-azure-cli"></a>Oluşturma, liste veya Azure CLI kullanarak kimlik atanmış bir kullanıcı silme

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Yönetilen hizmet kimliği Azure Active Directory'deki yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimlik, Azure AD kimlik doğrulaması, kimlik bilgileri, kodunuzda gerek kalmadan destekleyen hizmetlerin kimlik doğrulaması için kullanabilirsiniz. 

Bu makalede, oluşturma, liste ve Azure CLI kullanarak bir kullanıcı tarafından atanan kimlik silme öğrenin.

## <a name="prerequisites"></a>Önkoşullar

- Yönetilen hizmet kimliği ile emin değilseniz, kullanıma [genel bakış bölümünde](overview.md). **Gözden geçirmeyi unutmayın [atanan sistemi ve kullanıcı kimliği atanır arasındaki farkı](overview.md#how-does-it-work)**.
- Bir Azure hesabınız yoksa [ücretsiz bir hesap için kaydolun](https://azure.microsoft.com/free/) devam etmeden önce.

- CLI komut dosyası örnekleri çalıştırmak için üç seçeneğiniz vardır:

    - Kullanım [Azure bulut Kabuk](../../cloud-shell/overview.md) Azure portalından (sonraki bölüme bakın).
    - Her kod bloğunun sağ üst köşesinde yer alan "deneyin" düğmesini, aracılığıyla katıştırılmış Azure bulut kabuğunu kullanın.
    - [CLI 2.0'ın en son sürümünü yüklemek](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.13 veya sonrası) yerel CLI konsol kullanmayı tercih ederseniz. Oturum Azure kullanmaya `az login`, altında atanan kullanıcı kimliğini dağıtmak istiyor Azure aboneliğiyle ilişkili olan bir hesabı kullanarak.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-a-user-assigned-managed-identity"></a>Yönetilen kimlik atanmış bir kullanıcı oluşturun 

Bir kullanıcı tarafından atanan kimlik oluşturmak üzere kullanmak [az kimliği oluşturma](/cli/azure/identity#az-identity-create) komutu. `-g` Parametresi, kaynak grubuna atanan kullanıcı kimliğini oluşturulacağı yeri belirtir ve `-n` parametresi adını belirtir. Değiştir `<RESOURCE GROUP>` ve `<USER ASSIGNED IDENTITY NAME>` parametre değerlerini kendi değerlerinizi ile:

[!INCLUDE[ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

 ```azurecli-interactive
az identity create -g <RESOURCE GROUP> -n <USER ASSIGNED IDENTITY NAME>
```
## <a name="list-user-assigned-identities"></a>Liste kullanıcı kimlikleri atandı

Atanan kullanıcı kimlikleri listesinden, kullanmak için [az kimlik listesi](/cli/azure/identity#az-identity-list) komutu.  `-g` Parametresi, kullanıcı kimliği atanır oluşturulduğu kaynak grubu belirtir.  Değiştir `<RESOURCE GROUP>` kendi değerine sahip:

```azurecli-interactive
az identity list -g <RESOURCE GROUP>
```
Json yanıt olarak, kullanıcı kimliklerini sahip `"Microsoft.ManagedIdentity/userAssignedIdentities"` anahtar için döndürülen değer `type`.

`"type": "Microsoft.ManagedIdentity/userAssignedIdentities"`

## <a name="delete-a-user-assigned-identity"></a>Bir kullanıcı kimliği atanır Sil

Atanan kullanıcı kimliğini silmek için kullanın [az kimlik silmek](/cli/azure/identity#az-identity-delete) komutu.  -N parametre adını ve kullanıcı kimliği atanır oluşturulduğu kaynak grubunun -g parametresi belirtir.  Değiştir `<USER ASSIGNED IDENTITY NAME>` ve `<RESOURCE GROUP>` kendi değerlerinizi parametreleri değerlerle:

 ```azurecli-interactive
az identity delete -n <USER ASSIGNED IDENTITY NAME> -g <RESOURCE GROUP>
```
> [!NOTE]
> Bir kullanıcı kimliği atanır silme başvuru atanıp atanmadığını kaynaktan kaldırmaz. Lütfen bu VM/VMSS kullanarak kaldırın `az vm/vmss identity remove` komutu

## <a name="related-content"></a>İlgili içerik

Azure CLI kimlik komutları tam bir listesi için bkz: [az kimlik](/cli/azure/identity).

Bir Azure VM görmek atanan kullanıcı kimliğini atama hakkında bilgi için [Yapılandırma Yönetilen hizmet kimliği (Azure CLI kullanarak MSI)](qs-configure-cli-windows-vm.md#user-assigned-identity)


 
