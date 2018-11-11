---
title: REST kullanarak Azure kullanıcı tarafından atanan yönetilen kimliklerini nasıl yöneteceğiniz
description: Adım adım yönergeler oluşturmak, liste ve bir kullanıcı tarafından atanan silmek yönetilen REST API çağrıları gerçekleştirmek için kimliği.
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
ms.openlocfilehash: 4bf77cd34ba985dfcfa568db0543150c0510c406
ms.sourcegitcommit: d372d75558fc7be78b1a4b42b4245f40f213018c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/09/2018
ms.locfileid: "51300107"
---
# <a name="create-list-or-delete-a-user-assigned-managed-identity-using-rest-api-calls"></a>Oluşturma, liste veya REST API çağrıları kullanarak bir kullanıcı tarafından atanan yönetilen kimlik silme

[!INCLUDE [preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Kimlikleri Hizmetleri kimlik bilgileri, kodunuzda gerek kalmadan, söz konusu destek Azure AD kimlik doğrulamasını kimlik doğrulama becerisi sağlamak Azure hizmetleri sağlayan Azure kaynakları için yönetilen. 

Bu makalede, oluşturma, liste ve REST API çağrıları gerçekleştirmek için CURL kullanarak bir kullanıcı tarafından atanan yönetilen kimlik silme öğrenin.

## <a name="prerequisites"></a>Önkoşullar

- Azure kaynakları için yönetilen kimliklerle bilmiyorsanız kullanıma [genel bakış bölümünde](overview.md). **Gözden geçirmeyi unutmayın [sistem tarafından atanan ve kullanıcı tarafından atanan bir yönetilen kimlik arasındaki farkı](overview.md#how-does-it-work)**.
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).
- Windows kullanıyorsanız, yükleme [Linux için Windows alt sistemi](https://msdn.microsoft.com/commandline/wsl/about) veya [Azure Cloud Shell](../../cloud-shell/overview.md) Azure portalında.
- Kullanırsanız [Linux için Windows alt sistemi](https://msdn.microsoft.com/commandline/wsl/about) veya [Linux dağıtım işletim sistemi](/cli/azure/install-azure-cli-apt?view=azure-cli-latest), [Azure CLI'yı yerel Konsolu yükleme](/cli/azure/install-azure-cli).
- Azure CLI'yı yerel Konsolu kullanıyorsanız, Azure kullanarak oturum açın `az login` dağıtmayı veya kullanıcı tarafından atanan yönetilen kimlik bilgileri almak istediğiniz Azure aboneliğiyle ilişkili olan bir hesapla.
- Bir taşıyıcı belirteç kullanarak erişimini almak `az account get-access-token` aşağıdaki yönetilen kimlik kullanıcı tarafından atanan işlemleri gerçekleştirmek için.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-a-user-assigned-managed-identity"></a>Kullanıcı tarafından atanan yönetilen kimlik oluşturma 

Kullanıcı tarafından atanan bir yönetilen kimlik oluşturmak için hesabınızın gerekli [yönetilen kimlik Katılımcısı](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rol ataması.

Kullanıcı tarafından atanan bir yönetilen kimlik oluşturmak için aşağıdaki CURL isteği Azure Resource Manager API'si kullanın. Değiştirin `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>`, `<USER ASSIGNED IDENTITY NAME>`,`<LOCATION>`, ve `<ACCESS TOKEN>` değerleri kendi değerlerinizle:

[!INCLUDE [ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroup
s/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>?api-version=2015-08-31-preview' -X PUT -d '{"loc
ation": "<LOCATION>"}' -H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS TOKEN>"
```

## <a name="list-user-assigned-managed-identities"></a>Kullanıcı tarafından atanan yönetilen kimlikleri listesi

Kullanıcı tarafından atanan bir yönetilen kimlik listesi/okuma için hesabınızın gerekir [yönetilen kimlik işleci](/azure/role-based-access-control/built-in-roles#managed-identity-operator) veya [yönetilen kimlik Katılımcısı](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rol ataması.

Kullanıcı tarafından atanan yönetilen kimlikleri listelemek için aşağıdaki CURL isteği Azure Resource Manager API'si kullanın. Değiştirin `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>`, ve `<ACCESS TOKEN>` değerleri kendi değerlerinizle:

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities?api-version=2015-08-31-preview' -H "Authorization: Bearer <ACCESS TOKEN>"
```
## <a name="delete-a-user-assigned-managed-identity"></a>Kullanıcı tarafından atanan bir yönetilen kimlik Sil

Kullanıcı tarafından atanan bir yönetilen kimlik silmek için hesabınızın gerekli [yönetilen kimlik Katılımcısı](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rol ataması.

Kullanıcı tarafından atanan bir yönetilen kimlik silmek için aşağıdaki CURL isteği Azure Resource Manager API'si kullanın. Değiştirin `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>`, ve `<ACCESS TOKEN>` parametrelerin değerleri kendi değerlerinizle:

> [!NOTE]
> Kullanıcı tarafından atanan bir yönetilen kimlik silme başvuru atanmış herhangi bir kaynaktan kaldırmaz. Bir kullanıcı tarafından atanan kaldırmak için CURL bakın kullanarak bir VM'den yönetilen [bir kullanıcı tarafından atanan kimliği bir Azure VM'den kaldırın](qs-configure-rest-vm.md#remove-a-user-assigned identity-from-an-azure-vm).

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroup
s/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>?api-version=2015-08-31-preview' -X DELETE -H "Authorization: Bearer <ACCESS TOKEN>"
```

## <a name="next-steps"></a>Sonraki adımlar

Kullanıcı tarafından atanan bir yönetilen kimlik bir listelenen için Azure VM/VMSS atama hakkında bilgi için bkz: CURL kullanarak [yapılandırma kimlikleri REST API çağrıları kullanarak bir Azure sanal makinesinde Azure kaynakları için yönetilen](qs-configure-rest-vm.md#user-assigned-managed-identity) ve [yönetilen yapılandırma REST API çağrıları kullanan bir sanal makine ölçek kümesi Azure kaynakları için kimlikleri](qs-configure-rest-vmss.md#user-assigned-managed-identity).


