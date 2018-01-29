---
title: "Azure CLI kullanarak bir Azure kaynağı için bir kullanıcı tarafından atanan MSI erişim atama"
description: "Adım adım yönergeler bir kaynakta kullanıcı tarafından atanan bir MSI atamak için Azure CLI kullanarak başka bir kaynağa erişin."
services: active-directory
documentationcenter: 
author: bryanLa
manager: mtillman
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/22/2017
ms.author: bryanla
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 6e3bab5356812c256cfd147e42f065f381e0f63d
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2018
---
# <a name="assign-a-user-assigned-managed-service-identity-msi-access-to-a-resource-using-azure-cli"></a>Azure CLI kullanarak bir kaynak için bir kullanıcı tarafından atanan yönetilen hizmet kimliği (MSI) erişimi atayın

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Kullanıcı tarafından atanan bir MSI oluşturduktan sonra herhangi bir güvenlik asıl gibi başka bir kaynak MSI erişim izni verebilirsiniz. Bu örnek bir Azure depolama hesabı için Azure CLI kullanarak bir kullanıcı tarafından atanan MSI erişmesini sağlamak nasıl gösterir.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

Bu öğreticide CLI komut dosyası örnekleri çalıştırmak için iki seçeneğiniz vardır:

- Kullanım [Azure bulut Kabuk](~/articles/cloud-shell/overview.md) Azure portalından veya "deneyin" düğmesini, aracılığıyla her kod bloğunun sağ üst köşesinde bulunan.
- [CLI 2.0'ın en son sürümünü yüklemek](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.23 veya sonrası) yerel CLI konsol kullanmayı tercih ederseniz. Ardından Azure kullanarak oturum açın [az oturum açma](/cli/azure/#az_login). Altında kullanıcı tarafından atanan MSI ve VM dağıtmak istediğiniz Azure aboneliğiyle ilişkili olan bir hesabı kullanın:

   ```azurecli
   az login
   ```

## <a name="use-rbac-to-assign-the-msi-access-to-another-resource"></a>Başka bir kaynağa MSI erişim atamak için RBAC kullanın

Oturum açma veya kaynağa erişim için bir MSI (örneğin, bir VM), ana bilgisayar kaynağı ile kullanmak için MSI ilk rol ataması aracılığıyla kaynak erişim verilmelidir. Ana bilgisayarla ya da sonra MSI ilişkilendirmeden önce bunu yapabilirsiniz. Oluşturma ve bir VM ile ilişkilendirme tam adımlar için bkz: [Azure CLI kullanarak bir VM için bir kullanıcı tarafından atanan MSI yapılandırma](msi-qs-configure-cli-windows-vm.md).

Aşağıdaki örnekte, kullanıcı tarafından atanan bir MSI depolama hesabı için erişim verilir.  

1. MSI erişmesini sağlamak için MSI hizmet sorumlusu istemci kimliği (uygulama kimliği olarak da bilinir) gerekir. Kimliği tarafından sağlanan istemci [az kimliği oluşturma](/cli/azure/identity#az_identity_create), MSI ve (başvurusunu aşağıda gösterilen), hizmet asıl sağlama bağlıdır:

   ```azurecli-interactive
   az identity create -g rgPrivate -n msiServiceApp
   ```

   Başarılı yanıt kullanıcı tarafından atanan MSI hizmet sorumlusu içinde istemci Kimliğini içeren `clientId` özelliği:

   ```json
   {
        "clientId": "9391e5b1-dada-4a8z-834a-43ad44f296bl",
        "clientSecretUrl": "https://control-westcentralus.identity.azure.net/subscriptions/90z696ff-5efa-4909-a64d-f1b616f423ll/resourcegroups/rgPrivate/providers/Microsoft.ManagedIdentity/userAssignedIdentities/msiServiceApp/credentials?tid=733a8f0p-ec41-4e69-8adz-971fc4b533bl&oid=z4baa4fc-fce4-4202-8250-5fb359abblll&aid=9391e5b1-dada-4a8z-834a-43ad44f296bl",
        "id": "/subscriptions/90z696ff-5efa-4909-a64d-f1b616f423ll/resourcegroups/rgPrivate/providers/Microsoft.ManagedIdentity/userAssignedIdentities/msiServiceApp",
        "location": "westcentralus",
        "name": "msiServiceApp",
        "principalId": "z4baa4fc-fce4-4202-8250-5fb359abblll",
        "resourceGroup": "rgPrivate",
        "tags": {},
        "tenantId": "733a8f0p-ec41-4e69-8adz-971fc4b533bl",
        "type": "Microsoft.ManagedIdentity/userAssignedIdentities"
   }
   ```

2. İstemci kimliği bilinen sonra kullanarak [az rol ataması oluşturma](/cli/azure/role/assignment#az_role_assignment_create) MSI erişim için başka bir kaynağa atama. İstemci Kimliğiniz için kullandığınızdan emin olun `--assignee` parametre ve eşleşen abonelik kimliği ve kaynak grubu adı, çalıştırdığınızda döndürülen `az identity create`. Burada MSI "myStorageAcct" adlı bir depolama hesabı "Okuyucu" erişim atanır:

   ```azurecli-interactive
   az role assignment create --assignee 9391e5b1-dada-4a8z-834a-43ad44f296bl --role Reader --scope /subscriptions/90z696ff-5efa-4909-a64d-f1b616f423ll/resourcegroups/rgPrivate/providers/Microsoft.Storage/storageAccounts/myStorageAcct
   ```

   Başarılı yanıt aşağıdaki çıkış benzer:

   ```json
   {
        "id": "/subscriptions/90z696ff-5efa-4909-a64d-f1b616f423ll/resourcegroups/rgPrivate/providers/Microsoft.Storage/storageAccounts/myStorageAcct/providers/Microsoft.Authorization/roleAssignments/f4766864-9493-43c6-91d7-abd131c3c017",
        "name": "f4766864-9493-43c6-91d7-abd131c3c017",
        "properties": {
            "additionalProperties": {
            "createdBy": null,
            "createdOn": "2017-12-21T20:49:37.5590544Z",
            "updatedBy": "pd78b21f-17a4-41az-b7db-9aadec3376bl",
            "updatedOn": "2017-12-21T20:49:37.5590544Z"
            },
        "principalId": "z4baa4fc-fce4-4202-8250-5fb359abblll",
        "roleDefinitionId": "/subscriptions/90z696ff-5efa-4909-a64d-f1b616f423ll/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7",
        "scope": "/subscriptions/90z696ff-5efa-4909-a64d-f1b616f423ll/resourcegroups/rgPrivate/providers/Microsoft.Storage/storageAccounts/myStorageAcct"
        },
        "resourceGroup": "rgPrivate",
        "type": "Microsoft.Authorization/roleAssignments"
   }
   ```

## <a name="next-steps"></a>Sonraki adımlar

- MSI genel bakış için bkz: [yönetilen hizmet Kimliği'ne genel bakış](msi-overview.md).
- Kullanıcı tarafından atanan bir MSI Azure VM'de etkinleştirmek için bkz: [Azure CLI kullanarak bir VM için bir kullanıcı tarafından atanan yönetilen hizmet kimliği (MSI) yapılandırma](msi-qs-configure-cli-windows-vm.md).

Geri bildirim sağlamak ve iyileştirmek ve içeriği şekil yardımcı olmak için aşağıdaki açıklamaları bölümü kullanın.

