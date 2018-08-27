---
title: REST kullanarak yönetilen kimlikleri nasıl yöneteceğinizi Azure kullanıcı atanmış
description: Adım adım oluşturmak, liste ve kullanıcı silme hakkında yönergeler REST API çağrıları gerçekleştirmek için yönetilen bir kimlik atanır.
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
ms.date: 06/26/2018
ms.author: daveba
ms.openlocfilehash: a6241c105019f04df09080a89e8fe3b77b5f9385
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/24/2018
ms.locfileid: "42888772"
---
# <a name="create-list-or-delete-a-user-assigned-identity-using-rest-api-calls"></a>Oluşturma, liste veya REST API çağrıları kullanarak bir kullanıcı tarafından atanan kimliği silme

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Yönetilen kimlik Hizmetleri, kimlik bilgileri, kodunuzda gerek kalmadan söz konusu destek Azure AD kimlik doğrulamasını kimlik doğrulamasını becerisini Azure hizmetleri sağlar. 

Bu makalede, oluşturma, listesi ve bir kullanıcı tarafından atanan REST API çağrıları gerçekleştirmek için CURL kullanarak yönetilen kimliği silmeyi öğrenin.

## <a name="prerequisites"></a>Önkoşullar

- Yönetilen hizmet kimliği ile bilmiyorsanız, kullanıma [genel bakış bölümünde](overview.md). **Gözden geçirmeyi unutmayın [sistem tarafından atanan ve kullanıcı tarafından atanan kimliği arasındaki fark](overview.md#how-does-it-work)**.
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).
- Windows kullanıyorsanız, yükleme [Linux için Windows alt sistemi](https://msdn.microsoft.com/commandline/wsl/about) veya [Azure Cloud Shell](../../cloud-shell/overview.md) Azure portalında.
- Kullanırsanız [Linux için Windows alt sistemi](https://msdn.microsoft.com/commandline/wsl/about) veya [Linux dağıtım işletim sistemi](/cli/azure/install-azure-cli-apt?view=azure-cli-latest), [Azure CLI'yı yerel Konsolu yükleme](/cli/azure/install-azure-cli).
- Azure CLI'yı yerel Konsolu kullanıyorsanız, Azure kullanarak oturum açın `az login` dağıtmayı veya almak istediğiniz Azure aboneliğiyle ilişkili olan bir hesapla kullanıcı yönetilen kimlik bilgileri atanır.
- Bu makalede yönetim işlemlerini gerçekleştirmek için aşağıdaki rol atamaları hesabınızın gerekir:
    - [Yönetilen kimlik Katılımcısı](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rolü oluşturmak için (liste) okuma, güncelleştirme ve bir kullanıcı tarafından atanan kimliği silinemiyor.
    - [Yönetilen kimlik işleci](/azure/role-based-access-control/built-in-roles#managed-identity-operator) (liste), bir kullanıcı tarafından atanan kimlik özelliklerini okumak için rol.
- Bir taşıyıcı belirteç kullanarak erişimini almak `az account get-access-token` aşağıdaki kullanıcı gerçekleştirmek için yönetilen kimlik işlemleri atanmış.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-a-user-assigned-managed-identity"></a>Yönetilen kimlik atanmış bir kullanıcı oluşturun 

Yönetilen kimlik atanmış bir kullanıcı oluşturmak için aşağıdaki CURL isteği Azure Resource Manager API'si kullanın. Değiştirin `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>`, `<USER ASSIGNED IDENTITY NAME>`,`<LOCATION>`, ve `<ACCESS TOKEN>` değerleri kendi değerlerinizle:

[!INCLUDE[ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroup
s/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>?api-version=2015-08-31-preview' -X PUT -d '{"loc
ation": "<LOCATION>"}' -H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS TOKEN>"
```

## <a name="list-user-assigned-managed-identities"></a>Liste kullanıcı tarafından yönetilen kimlikleri atanan

Kullanıcı tarafından yönetilen kimlikleri atanan listesinde, aşağıdaki CURL isteği Azure Resource Manager API'si kullanın. Değiştirin `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>`, ve `<ACCESS TOKEN>` değerleri kendi değerlerinizle:

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities?api-version=2015-08-31-preview' -H "Authorization: Bearer <ACCESS TOKEN>"
```
## <a name="delete-a-user-assigned-managed-identity"></a>Yönetilen kimlik atanan kullanıcı silme

Bir kullanıcı tarafından atanan kimliği yönetilen silmek için aşağıdaki CURL isteği Azure Resource Manager API'si kullanın. Değiştirin `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>`, ve `<ACCESS TOKEN>` parametrelerin değerleri kendi değerlerinizle:

> [!NOTE]
> Bir kullanıcı tarafından atanan kimliği siliniyor başvuru atanmış herhangi bir kaynaktan kaldırmaz. Atanmış bir kullanıcıyı kaldırmak için CURL bakın kullanarak bir VM'den yönetilen [bir kullanıcı tarafından atanan kimliği bir Azure VM'den kaldırın](qs-configure-rest-vm.md#remove-a-user-assigned identity-from-an-azure-vm).

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroup
s/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>?api-version=2015-08-31-preview' -X DELETE -H "Authorization: Bearer <ACCESS TOKEN>"
```

## <a name="next-steps"></a>Sonraki adımlar

Bir kullanıcı tarafından atanan kimliği bir listelenen için Azure VM/VMSS atama hakkında bilgi için bkz: CURL kullanarak [CURL kullanarak bir Azure sanal makinesinde yönetilen kimlik yapılandırma](qs-configure-rest-vm.md#user-assigned-identity) ve [CURL kullanarak bir sanal makine ölçek kümesi üzerinde yönetilen kimliği yapılandırma ](qs-configure-rest-vmss.md#user-assigned-identity).


