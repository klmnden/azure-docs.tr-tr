---
title: "Azure CLI kullanarak bir Azure VM için bir kullanıcı tarafından atanan MSI yapılandırma"
description: "Azure Azure CLI kullanarak VM için bir kullanıcı tarafından atanan yönetilen hizmet kimliği (MSI) yapılandırma için adım yönergeler tarafından adım."
services: active-directory
documentationcenter: 
author: BryanLa
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
ms.openlocfilehash: 4b6f4e2b0e42724276448fd4726c8326de8ea6ee
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2018
---
# <a name="configure-a-user-assigned-managed-service-identity-msi-for-a-vm-using-azure-cli"></a>Azure CLI kullanarak bir VM için bir kullanıcı tarafından atanan yönetilen hizmet kimliği (MSI) yapılandırma

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Yönetilen hizmet kimliği Azure Active Directory'deki yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimlik, Azure AD kimlik doğrulaması, kimlik bilgileri, kodunuzda gerek kalmadan destekleyen hizmetlerin kimlik doğrulaması için kullanabilirsiniz. 

Bu makalede, Azure Azure CLI kullanarak VM için kullanıcı tarafından atanan bir MSI kaldırma etkinleştirip öğreneceksiniz.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

Bu öğreticide CLI komut dosyası örnekleri çalıştırmak için iki seçeneğiniz vardır:

- Kullanım [Azure bulut Kabuk](~/articles/cloud-shell/overview.md) Azure portalından veya "deneyin" düğmesini, aracılığıyla her kod bloğunun sağ üst köşesinde bulunan.
- [CLI 2.0'ın en son sürümünü yüklemek](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.23 veya sonrası) yerel CLI konsol kullanmayı tercih ederseniz. Ardından Azure kullanarak oturum açın [az oturum açma](/cli/azure/#login). Altında kullanıcı tarafından atanan MSI ve VM dağıtmak istediğiniz Azure aboneliğiyle ilişkili olan bir hesabı kullanın:

   ```azurecli
   az login
   ```

## <a name="enable-msi-during-creation-of-an-azure-vm"></a>Bir Azure VM oluşturma sırasında MSI etkinleştir

Bu bölümde VM VM oluşturulmasını ve kullanıcı tarafından atanan MSI atanması anlatılmaktadır. Zaten kullanmak istediğiniz bir VM'niz varsa, bu bölüm atlayın ve sonraki devam edin.

1. Kullanmak istediğiniz bir kaynak grubu zaten varsa bu adımı atlayabilirsiniz. Oluşturma bir [kaynak grubu](~/articles/azure-resource-manager/resource-group-overview.md#terminology) kapsama ve, MSI dağıtımı için kullanarak [az grubu oluşturma](/cli/azure/group/#create). Değiştirdiğinizden emin olun `<RESOURCE GROUP>` ve `<LOCATION>` parametre değerlerini kendi değerlere sahip. :

   ```azurecli-interactive 
   az group create --name <RESOURCE GROUP> --location <LOCATION>
   ```

2. Bir kullanıcı tarafından atanan MSI kullanarak oluşturduğunuz [az kimliği oluşturma](/cli/azure/identity#az_identity_create).  `-g` Parametresi, burada MSI oluşturulur, kaynak grubu belirtir ve `-n` parametresi adını belirtir. Değiştirdiğinizden emin olun `<RESOURCE GROUP>` ve `<MSI NAME>` parametre değerlerini kendi değerlerinizi ile:

    ```azurecli-interactive
    az identity create -g <RESOURCE GROUP> -n <MSI NAME>
    ```
Yanıt, oluşturulan, aşağıdakine benzer kullanıcı tarafından atanan MSI ayrıntıları içerir. Kaynak `id` MSI atanan değer aşağıdaki adımda kullanılır.

   ```json
   {
        "clientId": "73444643-8088-4d70-9532-c3a0fdc190fz",
        "clientSecretUrl": "https://control-westcentralus.identity.azure.net/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>/credentials?tid=5678&oid=9012&aid=73444643-8088-4d70-9532-c3a0fdc190fz",
        "id": "/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>",
        "location": "westcentralus",
        "name": "<MSI NAME>",
        "principalId": "e5fdfdc1-ed84-4d48-8551-fe9fb9dedfll",
        "resourceGroup": "<RESOURCE GROUP>",
        "tags": {},
        "tenantId": "733a8f0e-ec41-4e69-8ad8-971fc4b533bl",
        "type": "Microsoft.ManagedIdentity/userAssignedIdentities"    
   }
   ```

3. Kullanarak bir VM oluşturun [az vm oluşturma](/cli/azure/vm/#create). Aşağıdaki örnek, yeni kullanıcı tarafından atanan MSI, belirtildiği gibi ilişkili bir VM oluşturur `--assign-identity` parametresi. Değiştirdiğinizden emin olun `<RESOURCE GROUP>`, `<VM NAME>`, `<USER NAME>`, `<PASSWORD>`, ve `<`MSI kimliği >` parameter values with your own values. For `<MSI ID>`, use the user-assigned MSI's resource `Kimliği ' özelliği önceki adımda oluşturduğunuz: 

   ```azurecli-interactive 
   az vm create --resource-group <RESOURCE GROUP> --name <VM NAME> --image UbuntuLTS --admin-username <USER NAME> --admin-password <PASSWORD> --assign-identity <MSI ID>
   ```

## <a name="enable-msi-on-an-existing-azure-vm"></a>Var olan Azure VM'de MSI etkinleştir

1. Bir kullanıcı tarafından atanan MSI kullanarak oluşturduğunuz [az kimliği oluşturma](/cli/azure/identity#az_identity_create).  `-g` Parametresi, burada MSI oluşturulur, kaynak grubu belirtir ve `-n` parametresi adını belirtir. Değiştirdiğinizden emin olun `<RESOURCE GROUP>` ve `<MSI NAME>` parametre değerlerini kendi değerlerinizi ile:

    ```azurecli-interactive
    az identity create -g <RESOURCE GROUP> -n <MSI NAME>
    ```
Yanıt, oluşturulan, aşağıdakine benzer kullanıcı tarafından atanan MSI ayrıntıları içerir. Kaynak `id` MSI atanan değer aşağıdaki adımda kullanılır.

   ```json
   {
        "clientId": "73444643-8088-4d70-9532-c3a0fdc190fz",
        "clientSecretUrl": "https://control-westcentralus.identity.azure.net/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>/credentials?tid=5678&oid=9012&aid=73444643-8088-4d70-9532-c3a0fdc190fz",
        "id": "/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>",
        "location": "westcentralus",
        "name": "<MSI NAME>",
        "principalId": "e5fdfdc1-ed84-4d48-8551-fe9fb9dedfll",
        "resourceGroup": "<RESOURCE GROUP>",
        "tags": {},
        "tenantId": "733a8f0e-ec41-4e69-8ad8-971fc4b533bl",
        "type": "Microsoft.ManagedIdentity/userAssignedIdentities"    
   }
   ```

2. Kullanıcı tarafından atanan MSI kullanarak VM atamak [az vm Ata-identity](/cli/azure/vm#az_vm_assign_identity). Değiştirdiğinizden emin olun `<RESOURCE GROUP>` ve `<VM NAME>` parametre değerlerini kendi değerlere sahip. `<MSI ID>` Kullanıcı tarafından atanan MSI kaynak olacak `id` özelliği, önceki adımda oluşturduğunuz olarak:

    ```azurecli-interactive
    az vm assign-identity -g <RESOURCE GROUP> -n <VM NAME> --identities <MSI ID>
    ```

## <a name="remove-msi-from-an-azure-vm"></a>MSI bir Azure sanal makineden kaldırın

1. Kullanıcı tarafından atanan MSI, VM kullanımından kaldırmak [az vm Kaldır-identity](/cli/azure/vm#az_vm_remove_identity). Değiştirdiğinizden emin olun `<RESOURCE GROUP>` ve `<VM NAME>` parametre değerlerini kendi değerlere sahip. `<MSI NAME>` Kullanıcı tarafından atanan MSI olacaktır `name` sırasında belirtilen özellik `az identity create` komutu (önceki bölümlerde örneklere bakın):

   ```azurecli-interactive
   az vm remove-identity -g <RESOURCE GROUP> -n <VM NAME> --identities <MSI NAME>
   ```

## <a name="next-steps"></a>Sonraki adımlar

- [Yönetilen hizmet Kimliği'ne genel bakış](msi-overview.md)
- Tam Azure VM oluşturmak için hızlı başlangıç ipuçları, bakın: 

  - [CLI ile Windows sanal makine oluşturma](~/articles/virtual-machines/windows/quick-create-cli.md)  
  - [CLI ile Linux sanal makine oluşturma](~/articles/virtual-machines/linux/quick-create-cli.md) 

Geri bildirim sağlamak ve iyileştirmek ve içeriği şekil yardımcı olmak için aşağıdaki açıklamaları bölümü kullanın.
















