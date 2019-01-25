---
title: Hizmet sorumlusunu Azure CLI kullanarak bir yönetilen kimlik görüntüleme
description: Hizmet sorumlusunu Azure CLI kullanarak bir yönetilen kimlik görüntülemeye ilişkin adım adım yönergeler için.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/29/2018
ms.author: priyamo
ms.openlocfilehash: 8b30ad2acbad05a4f3095ab948dd987a295d5166
ms.sourcegitcommit: b4755b3262c5b7d546e598c0a034a7c0d1e261ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54887400"
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


