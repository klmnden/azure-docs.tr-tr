---
title: Azure AD RBAC - Azure depolama ile blob ve kuyruk verilere erişim haklarını yönetmek için Azure PowerShell'i kullanma
description: Kapsayıcılar ve rol tabanlı erişim denetimi (RBAC) kuyruklarla erişim atamak için Azure PowerShell kullanırsınız. Azure depolama, Azure AD ile kimlik doğrulaması için yerleşik ve özel RBAC rollerini destekler.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 06/26/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: bf888b72cca806822ca7a37542e71a5be0c8d5c3
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67443734"
---
# <a name="grant-access-to-azure-blob-and-queue-data-with-rbac-using-powershell"></a>PowerShell ile RBAC ile Azure blob ve kuyruk verilere erişim izni ver

Azure Active Directory (Azure AD) ile güvenli kaynaklara erişim hakları yetkilendirir [rol tabanlı erişim denetimi (RBAC)](../../role-based-access-control/overview.md). Azure depolama genel kapsayıcılar veya sıralara erişmek için kullanılan izin kümelerini kapsayacak yerleşik RBAC rolleri kümesi tanımlar. 

Bir RBAC rolü için bir Azure AD güvenlik sorumlusu atandığında, Azure verir, bir güvenlik sorumlusu için bu kaynaklara erişin. Abonelik, kaynak grubu, depolama hesabı veya bir kapsayıcının veya kuyruk düzeyi için erişimi sınırlayabilirsiniz. Bir Azure AD güvenlik sorumlusu olabilir bir kullanıcı, Grup, bir uygulama hizmet sorumlusu veya [yönetilen Azure kaynakları için kimliği](../../active-directory/managed-identities-azure-resources/overview.md).

Bu makalede, yerleşik RBAC rolleri listelemek ve onları kullanıcılara atamak için Azure PowerShell kullanmayı açıklar. Azure PowerShell kullanma hakkında daha fazla bilgi için bkz. [genel bakış, Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="rbac-roles-for-blobs-and-queues"></a>RBAC rolleri için BLOB'lar ve Kuyruklar

[!INCLUDE [storage-auth-rbac-roles-include](../../../includes/storage-auth-rbac-roles-include.md)]

## <a name="determine-resource-scope"></a>Kaynak kapsamını belirle

[!INCLUDE [storage-auth-resource-scope-include](../../../includes/storage-auth-resource-scope-include.md)]

## <a name="list-available-rbac-roles"></a>Liste kullanılabilir RBAC rolleri

Azure PowerShell ile kullanılabilen yerleşik RBAC rolleri listelemek için kullanın [Get-AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition) komutu:

```powershell
Get-AzRoleDefinition | FT Name, Description
```

Listelenen diğer yerleşik rolleri için Azure ile birlikte yerleşik Azure depolama veri rolleri görürsünüz:

```Example
Storage Blob Data Contributor             Allows for read, write and delete access to Azure Storage blob containers and data
Storage Blob Data Owner                   Allows for full access to Azure Storage blob containers and data, including assigning POSIX access control.
Storage Blob Data Reader                  Allows for read access to Azure Storage blob containers and data
Storage Queue Data Contributor            Allows for read, write, and delete access to Azure Storage queues and queue messages
Storage Queue Data Message Processor      Allows for peek, receive, and delete access to Azure Storage queue messages
Storage Queue Data Message Sender         Allows for sending of Azure Storage queue messages
Storage Queue Data Reader                 Allows for read access to Azure Storage queues and queue messages
```

## <a name="assign-an-rbac-role-to-a-security-principal"></a>Bir güvenlik sorumlusu için bir RBAC rolü atayın

Bir RBAC rolü için bir güvenlik sorumlusu atama [yeni AzRoleAssignment](/powershell/module/az.resources/new-azroleassignment) komutu. Komut biçimi atama kapsamına göre değişebilir. Aşağıdaki örnekler, çeşitli kapsamları sırasında bir kullanıcıya rol atamak gösterilmektedir, ancak aynı komutu tüm güvenlik sorumlusu rol atamak için kullanabilirsiniz.

### <a name="container-scope"></a>Kapsayıcı kapsamı

Bir kapsayıcı için kapsamlı bir rol atamak için kapsayıcı kapsamını içeren bir dize belirtin. `--scope` parametresi. Bir kapsayıcı için kapsamı şu şekildedir:

```
/subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/blobServices/default/containers/<container-name>
```

Aşağıdaki örnek atar **depolama Blob verileri katkıda bulunan** adlı bir kapsayıcı için kapsamlı, bir kullanıcı rolüne *örnek kapsayıcı*. Örnek değerler ve köşeli ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirdiğinizden emin olun: 

```powershell
New-AzRoleAssignment -SignInName <email> `
    -RoleDefinitionName "Storage Blob Data Contributor" `
    -Scope  "/subscriptions/<subscription>/resourceGroups/sample-resource-group/providers/Microsoft.Storage/storageAccounts/<storage-account>/blobServices/default/containers/sample-container"
```

### <a name="queue-scope"></a>Sıra kapsamı

Bir kuyruk için kapsamlı bir rol atamak için sıra kapsamı içeren bir dize belirtin. `--scope` parametresi. Bir kuyruk için kapsamı şu şekildedir:

```
/subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/queueServices/default/queues/<queue-name>
```

Aşağıdaki örnek atar **depolama kuyruk verileri katkıda bulunan** adlı bir sıra kapsamı, bir kullanıcı rolüne *örnek kuyruk*. Örnek değerler ve köşeli ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirdiğinizden emin olun: 

```powershell
New-AzRoleAssignment -SignInName <email> `
    -RoleDefinitionName "Storage Queue Data Contributor" `
    -Scope  "/subscriptions/<subscription>/resourceGroups/sample-resource-group/providers/Microsoft.Storage/storageAccounts/<storage-account>/queueServices/default/queues/sample-queue"
```

### <a name="storage-account-scope"></a>Depolama hesabı kapsamı

Depolama hesabı için kapsamlı bir rol atamak için depolama hesabı kaynağı için kapsamı belirtin `--scope` parametresi. Bir depolama hesabı için kapsamı şu şekildedir:

```
/subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>
```

Aşağıdaki örnekte gösterildiği nasıl kapsama **depolama Blob verileri okuyucu** depolama hesabı düzeyinde bir kullanıcı rolü. Örnek değerleri kendi değerlerinizle değiştirdiğinizden emin olun: 

```powershell
New-AzRoleAssignment -SignInName <email> `
    -RoleDefinitionName "Storage Blob Data Reader" `
    -Scope  "/subscriptions/<subscription>/resourceGroups/sample-resource-group/providers/Microsoft.Storage/storageAccounts/<storage-account>"
```

### <a name="resource-group-scope"></a>Kaynak Grup kapsamı

Kaynak grubu kapsamında bir rol atamak için kaynak grubu adı veya kimliği belirtin `--resource-group` parametresi. Aşağıdaki örnek atar **depolama kuyruk verileri okuyucu** kaynak grubu düzeyinde bir kullanıcı rolü. Köşeli ayraçlar içindeki yer tutucu değerlerini ve örnek değerleri kendi değerlerinizle değiştirdiğinizden emin olun: 

```powershell
New-AzRoleAssignment -SignInName <email> `
    -RoleDefinitionName "Storage Queue Data Reader" `
    -ResourceGroupName "sample-resource-group"
```

### <a name="subscription-scope"></a>Abonelik kapsamı

Abonelik kapsamında bir rol atamak için abonelik için kapsamı belirtin `--scope` parametresi. Kapsam bir abonelik için şu şekildedir:

```
/subscriptions/<subscription>
```

Aşağıdaki örnek nasıl atanacağını gösterir **depolama Blob verileri okuyucu** depolama hesabı düzeyinde bir kullanıcı rolü. Örnek değerleri kendi değerlerinizle değiştirdiğinizden emin olun: 

```powershell
New-AzRoleAssignment -SignInName <email> `
    -RoleDefinitionName "Storage Blob Data Reader" `
    -Scope  "/subscriptions/<subscription>"
```

## <a name="next-steps"></a>Sonraki adımlar

- [RBAC ve Azure PowerShell kullanarak Azure kaynaklarına erişimi yönetme](../../role-based-access-control/role-assignments-powershell.md)
- [Azure CLI kullanarak RBAC ile Azure blob ve kuyruk verilere erişim izni ver](storage-auth-aad-rbac-cli.md)
- [Azure blob ve kuyruk verilere RBAC ile Azure portalında erişim izni ver](storage-auth-aad-rbac-portal.md)
