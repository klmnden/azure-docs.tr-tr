---
title: Azure API Management'te rol tabanlı erişim denetimini kullanma | Microsoft Docs
description: Yerleşik roller kullanın ve Azure API Management'te özel roller oluşturma hakkında bilgi edinin
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
ms.date: 11/27/2017
ms.author: apimpm
ms.openlocfilehash: c775780a39c4d423c62bf88f55d35675c70442c7
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="how-to-use-role-based-access-control-in-azure-api-management"></a>Azure API Management'te rol tabanlı erişim denetimi kullanma
Azure API Management üzerinde Azure rol tabanlı erişim denetimi (API Management hizmet ve varlıkları (örneğin, API ve ilkeleri) için ayrıntılı erişim yönetimini etkinleştirmek için RBAC) kullanır. Bu makalede, API Management'te yerleşik ve özel roller genel bir bakış sağlar. Azure portalında erişim yönetimi hakkında daha fazla bilgi için bkz: [Azure portalında erişim yönetimini kullanmaya başlama](https://azure.microsoft.com/documentation/articles/role-based-access-control-what-is/).

## <a name="built-in-roles"></a>Yerleşik roller
API Management anda üç yerleşik roller sağlar ve iki daha fazla rol yakın gelecekte ekler. Bu rolleri abonelik, kaynak grubu ve tek tek API Management örneği dahil farklı kapsamların atanabilir. Örneğin, kaynak grubu düzeyinde bir kullanıcı "Azure API Management hizmeti okuyucu" rolüne atarsanız, kullanıcı kaynak grubu içindeki tüm API Management örnekleri için okuma erişimi vardır. 

Aşağıdaki tabloda yerleşik roller kısa açıklamaları sağlar. Azure portalında veya Azure dahil olmak üzere diğer araçları kullanarak bu roller atayabilirsiniz [PowerShell](https://docs.microsoft.com/azure/active-directory/role-based-access-control-manage-access-powershell), [Azure CLI](https://docs.microsoft.com/azure/active-directory/role-based-access-control-manage-access-azure-cli), ve [REST API](https://docs.microsoft.com/azure/active-directory/role-based-access-control-manage-access-rest). Yerleşik roller atama hakkında daha fazla ayrıntı için bkz: [Azure aboneliği kaynaklarınıza erişimi yönetmek için rol atamalarını kullanın](https://azure.microsoft.com/documentation/articles/role-based-access-control-what-is/).

| Rol          | Okuma erişimi<sup>[1]</sup> | Yazma erişimi<sup>[2]</sup> | Hizmet oluşturma, silme, ölçeklendirme, VPN ve özel etki alanı yapılandırması | Eski yayımcı portalına erişim | Açıklama
| ------------- | ---- | ---- | ---- | ---- | ---- | ---- |
| Azure API Management hizmeti katkıda bulunan | ✓ | ✓ | ✓ | ✓ | Süper kullanıcı. API Management hizmet ve varlıkları (örneğin, API ve ilkeleri) tam CRUD erişimi vardır. Eski yayımcı portalına erişebilir. |
| Azure API Management Service Reader | ✓ | | || API Management hizmet ve varlıkları salt okunur erişimi vardır. |
| Azure API Management hizmeti işleci | ✓ | | ✓ | | API Management services, ancak olmayan varlıklar yönetebilirsiniz.|
| Azure API Management hizmeti Düzenleyicisi<sup>*</sup> | ✓ | ✓ | |  | API Management varlıklar, ancak değil hizmetleri yönetebilirsiniz.|
| Azure API Management İçerik Yöneticisi<sup>*</sup> | ✓ | | | ✓ | Geliştirici Portalı yönetebilirsiniz. Hizmetler ve varlıklar için salt okunur erişim.|

<sup>[1] API Management Hizmetleri ve varlıkları (örneğin, API ve ilkeleri) okuma erişimi.</sup>

<sup>[2] yazma erişimi API Management hizmet ve aşağıdaki işlemleri dışında varlıklar: örnek oluşturma, silme ve ölçeklendirme; VPN yapılandırması; ve özel etki alanı kurulumu.</sup>

<sup>\* Biz tüm yönetim kullanıcı Arabirimi var olan yayımcı Portalı'ndan Azure portalına geçirdikten sonra hizmet Düzenleyicisi rolü kullanılabilir. Yayımcı portalı yalnızca Geliştirici Portalı yönetmeyle ilgili işlevleri içerecek şekilde yeniden sonra içerik yöneticisi rolü kullanılabilir.</sup>  

## <a name="custom-roles"></a>Özel roller
Yerleşik roller hiçbiri belirli ihtiyaçlarınızı karşılamıyorsa, API Management varlıklar için daha ayrıntılı erişim yönetimini sağlamak için özel rolleri oluşturulabilir. Örneğin, bir API Management hizmeti salt okunur erişimi olan, ancak yalnızca belirli bir API yazma erişimi olan bir özel rolü oluşturabilirsiniz. Özel rolleri hakkında daha fazla bilgi için bkz: [Azure rbac'de özel roller](https://docs.microsoft.com/azure/active-directory/role-based-access-control-custom-roles). 

Özel bir rol oluşturduğunuzda, yerleşik roller biri ile başlatmak daha kolaydır. Eklemek için öznitelikleri düzenlemenize **Eylemler**, **NotActions**, veya **AssignableScopes**ve ardından yeni bir rol olarak değişiklikleri kaydedin. Aşağıdaki örnekte "Azure API Management hizmeti okuyucu" rolü başlar ve "hesaplayıcı API'si Düzenleyici." adlı özel bir rol oluşturur Belirli bir API için özel rol atayabilirsiniz. Sonuç olarak, bu rolün yalnızca o API erişebilir. 

```
$role = Get-AzureRmRoleDefinition "API Management Service Reader Role"
$role.Id = $null
$role.Name = 'Calculator API Contributor'
$role.Description = 'Has read access to Contoso APIM instance and write access to the Calculator API.'
$role.Actions.Add('Microsoft.ApiManagement/service/apis/write')
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add('/subscriptions/<subscription ID>/resourceGroups/<resource group name>/providers/Microsoft.ApiManagement/service/<service name>/apis/<api ID>')
New-AzureRmRoleDefinition -Role $role
New-AzureRmRoleAssignment -ObjectId <object ID of the user account> -RoleDefinitionName 'Calculator API Contributor' -Scope '/subscriptions/<subscription ID>/resourceGroups/<resource group name>/providers/Microsoft.ApiManagement/service/<service name>/apis/<api ID>'
```

[Azure Resource Manager kaynak sağlayıcısı işlemleri](../active-directory/role-based-access-control-resource-provider-operations.md#microsoftapimanagement) makale API Management düzeyinde verilebilir izinleri listesini içerir.

## <a name="video"></a>Video


> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Role-Based-Access-Control-in-API-Management/player]
>
>

## <a name="next-steps"></a>Sonraki adımlar

Azure rol tabanlı erişim denetimi hakkında daha fazla bilgi için aşağıdaki makalelere bakın:
  * [Azure portalında erişim yönetimi ile çalışmaya başlama](https://azure.microsoft.com/documentation/articles/role-based-access-control-what-is/)
  * [Azure abonelik kaynaklarınıza erişimi yönetmek için rol atamalarını kullanın](https://azure.microsoft.com/documentation/articles/role-based-access-control-what-is/)
  * [Azure rbac'de özel roller](https://docs.microsoft.com/azure/active-directory/role-based-access-control-custom-roles)
  * [Azure Resource Manager kaynak sağlayıcısı işlemleri](../active-directory/role-based-access-control-resource-provider-operations.md#microsoftapimanagement)

