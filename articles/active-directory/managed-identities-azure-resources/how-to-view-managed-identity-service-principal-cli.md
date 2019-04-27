---
title: Hizmet sorumlusunu Azure CLI kullanarak bir yönetilen kimlik görüntüleme
description: Hizmet sorumlusunu Azure CLI kullanarak bir yönetilen kimlik görüntülemeye ilişkin adım adım yönergeler için.
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
ms.date: 11/29/2018
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: f379c78113a4edc1efc288617a8a1c205d03552a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60442293"
---
# <a name="view-the-service-principal-of-a-managed-identity-using-azure-cli"></a>Hizmet sorumlusunu Azure CLI kullanarak bir yönetilen kimlik görüntüleyin

Azure kaynakları için yönetilen kimlikleri Azure Active Directory'de otomatik olarak yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimlik, kimlik bilgilerini kodunuzda zorunda kalmadan Azure AD kimlik doğrulamasını destekleyen herhangi bir hizmeti kimlik doğrulaması için kullanabilirsiniz. 

Bu makalede, hizmet sorumlusunu Azure CLI kullanarak bir yönetilen kimlik görüntüleme hakkında bilgi edinin.

## <a name="prerequisites"></a>Önkoşullar

- Azure kaynakları için yönetilen kimliklerle bilmiyorsanız kullanıma [genel bakış bölümünde](overview.md).
- Azure hesabınız yoksa, [ücretsiz bir hesap için kaydolun](https://azure.microsoft.com/free/).
- Etkinleştirme [bir sanal makinede sistem tarafından atanan kimlik](/azure/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm#system-assigned-managed-identity) veya [uygulama](/azure/app-service/overview-managed-identity#adding-a-system-assigned-identity).
- CLI betiği örnekleri çalıştırmak için üç seçeneğiniz vardır:
    - Kullanım [Azure Cloud Shell](../../cloud-shell/overview.md) Azure portalından (sonraki bölüme bakın).
    - Katıştırılmış Azure Cloud Shell aracılığıyla her kod bloğunun sağ üst köşesinde bulunan "Try It" düğmesini kullanın.
    - [Azure CLI'ın en son sürümü yükleyin](https://docs.microsoft.com/cli/azure/install-azure-cli) yerel bir CLI konsolu ve Azure kullanarak oturum açma tercih ettiğiniz `az login`
 
[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="view-the-service-principal"></a>Hizmet sorumlusu görüntüleyin

Bu aşağıdaki komut, etkin yönetilen kimlikle bir VM veya uygulama hizmet sorumlusu görüntülemek nasıl gösterir. Değiştirin `<VM or application name>` kendi değerlerinizle. 

```azurecli-interactive
az ad sp list --display-name <VM or application name>
```

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI kullanarak Azure AD hizmet sorumlusu yönetme ile ilgili daha fazla bilgi için bkz: [az ad sp](/cli/azure/ad/sp).


