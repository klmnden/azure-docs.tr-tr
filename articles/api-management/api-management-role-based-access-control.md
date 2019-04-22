---
title: Azure API Yönetimi'nde rol tabanlı erişim denetimini kullanma | Microsoft Docs
description: Yerleşik rolleri kullanabileceğiniz ve Azure API Management'te özel roller oluşturma hakkında bilgi edinin
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: 364cd53e-88fb-4301-a093-f132fa1f88f5
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2018
ms.author: apimpm
ms.openlocfilehash: 2e53b0d582a69e10de22e85720833800d44058e3
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58793875"
---
# <a name="how-to-use-role-based-access-control-in-azure-api-management"></a>Azure API Yönetimi'nde rol tabanlı erişim denetimi kullanma

Azure API Management hakkında Azure rol tabanlı erişim denetimi (API Management Hizmetleri ve varlıkları (örneğin, API'leri ve ilkeleri) ayrıntılı erişim yönetimini etkinleştirmek için RBAC) kullanır. Bu makalede, API Yönetimi'nde yerleşik ve özel roller genel bir bakış sağlar. Azure portalında erişim yönetimi hakkında daha fazla bilgi için bkz. [Azure portalında erişim yönetimini kullanmaya başlama](https://azure.microsoft.com/documentation/articles/role-based-access-control-what-is/).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="built-in-roles"></a>Yerleşik roller

API Yönetimi şu anda üç yerleşik rol sağlar ve iki daha fazla rol yakın gelecekte ekleyeceksiniz. Bu roller, abonelik, kaynak grubu ve tek bir API Management örneği gibi farklı kapsamların atanabilir. Örneğin, kaynak grubu düzeyinde bir kullanıcı için "Azure API Management hizmet okuyucusu" rolünü atamanız durumunda kullanıcı kaynak grubunun içindeki tüm API Management örneği için okuma erişimi olduğundan. 

Aşağıdaki tabloda, yerleşik rollerin kısa açıklamaları verilmiştir. Azure portal veya Azure gibi diğer araçları kullanarak bu roller atayabilirsiniz [PowerShell](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-powershell), [Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli), ve [REST API](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-rest). Yerleşik rollerin nasıl atanacağı hakkında daha fazla ayrıntı için bkz: [Azure abonelik kaynaklarınıza erişimi yönetmek için rol atamalarını kullanma](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal).

| Rol          | Okuma erişimi<sup>[1]</sup> | Yazma erişimi<sup>[2]</sup> | Hizmet oluşturma, silme, ölçeklendirme, VPN ve özel etki alanı yapılandırması | Eski yayımcı portalına erişim | Açıklama
| ------------- | ---- | ---- | ---- | ---- | ---- 
| Azure API Yönetimi Hizmeti Katılımcısı | ✓ | ✓ | ✓ | ✓ | Süper kullanıcı. API Management Hizmetleri ve varlıkları (örneğin, API'leri ve ilkeleri) tam CRUD erişimi vardır. Eski yayımcı portalına erişebilir. |
| Azure API Management hizmet okuyucusu | ✓ | | || API Management Hizmetleri ve varlıkların salt okunur erişimi vardır. |
| Azure API Management hizmet operatörü | ✓ | | ✓ | | API Management Hizmetleri, ancak değil varlıkları yönetebilir.|
| Azure API Yönetimi Hizmeti Düzenleyicisi<sup>*</sup> | ✓ | ✓ | |  | API Yönetimi varlıkları, ancak değil hizmetlerini yönetebilir.|
| Azure API Management Content Manager<sup>*</sup> | ✓ | | | ✓ | Geliştirici portalını yönetebilirsiniz. Hizmetler ve varlıklar için yalnızca okuma erişimi.|

<sup>[1] API Management Hizmetleri ve varlıkları (örneğin, API'leri ve ilkeleri) okuma erişimi.</sup>

<sup>[2] API Management Hizmetleri ve aşağıdaki işlemleri varlıklar yazma erişimi: örnek oluşturma, silme ve ölçeklendirme; VPN yapılandırması; ve özel etki alanı kurulumu.</sup>

<sup>\* Biz tüm yönetici kullanıcı Arabirimi mevcut yayımcı portalından Azure portalına geçiş yaptıktan sonra Service Düzenleyicisi rolü kullanılabilir. İçerik Yöneticisi rolü, yayımcı portalındaki Geliştirici Portalı yönetmeyle ilgili işlevleri yalnızca içerecek şekilde yeniden düzenlenen sonra kullanılabilir olacaktır.</sup>  

## <a name="custom-roles"></a>Özel roller

Yerleşik rollerden hiçbiri gereksinimlerinize uygun değilse, API Management varlıklar için daha ayrıntılı erişim yönetimi sağlamak için özel roller oluşturabilirsiniz. Örneğin, bir API Management hizmeti salt okunur erişimi vardır, ancak yalnızca belirli bir API'yi yazma erişimi olan özel bir rol oluşturabilirsiniz. Özel roller hakkında daha fazla bilgi için bkz: [Azure rbac'de özel roller](https://docs.microsoft.com/azure/role-based-access-control/custom-roles). 

> [!NOTE]
> Azure portalında bir API Management örneği görebilmeniz için özel bir rol içermesi gerekir ```Microsoft.ApiManagement/service/read``` eylem.

Özel bir rol oluşturduğunuzda, yerleşik rollerden biri ile başlamak kolaydır. Öznitelikleri eklemek için Düzenle **eylemleri**, **NotActions**, veya **AssignableScopes**ve sonra değişiklikleri yeni bir rolü olarak kaydedebilirsiniz. Aşağıdaki örnek "Azure API Management hizmet okuyucusu" rolüne sahip başlar ve "hesaplayıcı API'si Düzenleyici." adlı özel bir rol oluşturur Belirli bir API'ye özel rol atayabilirsiniz. Sonuç olarak, bu rol, bu API'ye erişim yalnızca sahiptir. 

```powershell
$role = Get-AzRoleDefinition "API Management Service Reader Role"
$role.Id = $null
$role.Name = 'Calculator API Contributor'
$role.Description = 'Has read access to Contoso APIM instance and write access to the Calculator API.'
$role.Actions.Add('Microsoft.ApiManagement/service/apis/write')
$role.Actions.Add('Microsoft.ApiManagement/service/apis/*/write')
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add('/subscriptions/<subscription ID>/resourceGroups/<resource group name>/providers/Microsoft.ApiManagement/service/<service name>/apis/<api ID>')
New-AzRoleDefinition -Role $role
New-AzRoleAssignment -ObjectId <object ID of the user account> -RoleDefinitionName 'Calculator API Contributor' -Scope '/subscriptions/<subscription ID>/resourceGroups/<resource group name>/providers/Microsoft.ApiManagement/service/<service name>/apis/<api ID>'
```

[Azure Resource Manager kaynak sağlayıcısı işlemleri](../role-based-access-control/resource-provider-operations.md#microsoftapimanagement) makale, API Management düzeyine verilen izinleri listesini içerir.

## <a name="video"></a>Video


> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Role-Based-Access-Control-in-API-Management/player]
>
>

## <a name="next-steps"></a>Sonraki adımlar

Azure rol tabanlı erişim denetimi hakkında daha fazla bilgi için aşağıdaki makalelere bakın:
  * [Azure portalında erişim yönetimi ile çalışmaya başlama](../role-based-access-control/overview.md)
  * [Azure abonelik kaynaklarınıza erişimi yönetmek için rol atamalarını kullanın](../role-based-access-control/role-assignments-portal.md)
  * [Azure rbac'de özel roller](../role-based-access-control/custom-roles.md)
  * [Azure Resource Manager kaynak sağlayıcısı işlemleri](../role-based-access-control/resource-provider-operations.md#microsoftapimanagement)