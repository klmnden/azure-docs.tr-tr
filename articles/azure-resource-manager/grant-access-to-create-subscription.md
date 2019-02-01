---
title: Azure Kurumsal abonelikler oluşturmak için erişim verme | Microsoft Docs
description: Bir kullanıcı veya hizmet sorumlusu program aracılığıyla Azure Enterprise abonelikleri oluşturma olanağı sağlayacak öğrenin.
services: azure-resource-manager
author: adpick
manager: adpick
editor: ''
ms.assetid: ''
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/05/2018
ms.author: adpick
ms.openlocfilehash: 3577edff19788ed9f0925876e3de737eb749b90e
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55490932"
---
# <a name="grant-access-to-create-azure-enterprise-subscriptions-preview"></a>Azure Kurumsal abonelikler (Önizleme) oluşturma erişimi verme

Üzerinde bir Azure müşterisi olarak [Kurumsal Anlaşma (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/), faturalandırılan abonelikler oluşturmak için başka bir kullanıcı veya hizmet sorumlusu izin verebilirsiniz. Bu makalede, kullanmayı öğrenirsiniz [rol tabanlı erişim denetimi (RBAC)](../active-directory/role-based-access-control-configure.md) abonelikleri ve abonelik oluşturma denetleme oluşturma olanağı paylaşmak için. Sahip rolü, paylaşmak istediğiniz hesabı olması gerekir.

Bir abonelik oluşturmak için bkz: [program aracılığıyla Azure Kurumsal abonelikler (Önizleme) oluşturma](programmatically-create-subscription.md).

## <a name="delegate-access-to-an-enrollment-account-using-rbac"></a>RBAC kullanarak bir kayıt hesabı temsilci erişimi

Başka bir kullanıcı veya hizmet sorumlusu aboneliklere yönelik özel bir hesap oluşturma olanağı vermek [kayıt hesabı kapsamındaki bir RBAC sahip rolü vermediğiniz](../active-directory/role-based-access-control-manage-access-rest.md). Aşağıdaki örnek bir kullanıcıya sahip kiracısındaki sağlar. `principalId` , `<userObjectId>` (için SignUpEngineering@contoso.com) kayıt hesabında bir sahip rolü. Kayıt hesabı Kimliğinizi ve asıl Kimliğinizi bulmak için bkz: [program aracılığıyla Azure Kurumsal abonelikler (Önizleme) oluşturma](programmatically-create-subscription.md).

# <a name="resttabrest"></a>[REST](#tab/rest)

```json
PUT  https://management.azure.com/providers/Microsoft.Billing/enrollmentAccounts/747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx/providers/Microsoft.Authorization/roleAssignments/<roleAssignmentGuid>?api-version=2015-07-01

{
  "properties": {
    "roleDefinitionId": "/providers/Microsoft.Billing/enrollmentAccounts/providers/Microsoft.Authorization/roleDefinitions/<ownerRoleDefinitionId>",
    "principalId": "<userObjectId>"
  }
}
```

Sahip rolü kayıt hesabı kapsamda başarıyla atandığında, Azure rol ataması bilgilerle yanıt verir:

```json
{
  "properties": {
    "roleDefinitionId": "/providers/Microsoft.Billing/enrollmentAccounts/providers/Microsoft.Authorization/roleDefinitions/<ownerRoleDefinitionId>",
    "principalId": "<userObjectId>",
    "scope": "/providers/Microsoft.Billing/enrollmentAccounts/747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "createdOn": "2018-03-05T08:36:26.4014813Z",
    "updatedOn": "2018-03-05T08:36:26.4014813Z",
    "createdBy": "<assignerObjectId>",
    "updatedBy": "<assignerObjectId>"
  },
  "id": "/providers/Microsoft.Billing/enrollmentAccounts/providers/Microsoft.Authorization/roleDefinitions/<ownerRoleDefinitionId>",
  "type": "Microsoft.Authorization/roleAssignments",
  "name": "<roleAssignmentGuid>"
}
```

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

Kullanım [yeni AzRoleAssignment](../active-directory/role-based-access-control-manage-access-powershell.md) kayıt hesabınıza başka bir kullanıcıya sahip erişimi vermek için.

```azurepowershell-interactive
New-AzRoleAssignment -RoleDefinitionName Owner -ObjectId <userObjectId> -Scope /providers/Microsoft.Billing/enrollmentAccounts/747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

Kullanım [az rol ataması oluşturma](../active-directory/role-based-access-control-manage-access-azure-cli.md) kayıt hesabınıza başka bir kullanıcıya sahip erişimi vermek için.

```azurecli-interactive 
az role assignment create --role Owner --assignee-object-id <userObjectId> --scope /providers/Microsoft.Billing/enrollmentAccounts/747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

----

Bir kullanıcı bir kayıt hesabınız için RBAC sahip olduktan sonra programlı olarak abonelik altındaki oluşturabilirler. Yetkilendirilmiş bir kullanıcı tarafından oluşturulmuş bir abonelik hala özgün hesap sahibi olarak Hizmet Yöneticisi vardır, ancak ayrıca yetkilendirilmiş kullanıcının sahibi olarak varsayılan olarak sahiptir. 

## <a name="audit-who-created-subscriptions-using-activity-logs"></a>Etkinlik günlüklerini kullanarak abonelikleri oluşturan denetim

Bu API aracılığıyla oluşturulan abonelikleri izlemek için [Kiracı etkinlik günlüğü API](/rest/api/monitor/tenantactivitylogs). Şu anda abonelik oluşturma izlemek için PowerShell, CLI veya Azure portalını kullanmak mümkün değildir.

1. Azure AD kiracısının kiracı yöneticisi olarak, [erişimi yükseltin](../active-directory/role-based-access-control-tenant-admin-access.md) ve sonra da `/providers/microsoft.insights/eventtypes/management` kapsamı üzerinden denetleyen kullanıcıya Okuyucu rolü atayın.
1. Denetim kullanıcı olarak çağrı [Kiracı etkinlik günlüğü API](/rest/api/monitor/tenantactivitylogs) abonelik oluşturma etkinlikleri görmek için. Örnek:

```
GET "/providers/Microsoft.Insights/eventtypes/management/values?api-version=2015-04-01&$filter=eventTimestamp ge '{greaterThanTimeStamp}' and eventTimestamp le '{lessThanTimestamp}' and eventChannels eq 'Operation' and resourceProvider eq 'Microsoft.Subscription'" 
```

> [!NOTE]
> Bu API'yi komut satırından rahatça çağırmak için [ARMClient](https://github.com/projectkudu/ARMClient)'ı deneyin.

## <a name="next-steps"></a>Sonraki adımlar

* Artık kullanıcı veya hizmet sorumlusu abonelik oluşturma izni verildiğine göre bu kimlik için kullanabileceğiniz [program aracılığıyla Azure Enterprise abonelikleri oluşturma](programmatically-create-subscription.md).
* .NET kullanarak abonelikleri oluşturma örneği için bkz. [örnek kodu github'da](https://github.com/Azure-Samples/create-azure-subscription-dotnet-core).
* Azure Resource Manager ve kendi API hakkında daha fazla bilgi için bkz: [Azure Resource Manager'a genel bakış](resource-group-overview.md).
* Çok sayıda Yönetim grupları kullanarak aboneliklerini yönetme hakkında daha fazla bilgi edinmek için [kaynaklarınızı Azure Yönetim grupları ile düzenleme](management-groups-overview.md)
* Büyük kuruluşlar için kapsamlı bir en iyi uygulama kılavuzunu abonelik İdaresi üzerinde görmek için bkz [Azure Kurumsal iskelesi: öngörücü abonelik İdaresi](/azure/architecture/cloud-adoption-guide/subscription-governance)
