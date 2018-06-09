---
title: Azure Kurumsal abonelikler oluşturmak için erişim izni verme | Microsoft Docs
description: Bir kullanıcı veya hizmet sorumlusu program aracılığıyla Azure Enterprise abonelikleri oluşturma vermek öğrenin.
services: azure-resource-manager
author: jlian
manager: jlian
editor: ''
ms.assetid: ''
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/05/2018
ms.author: jlian
ms.openlocfilehash: 4c5d505f431ef684b73adc04629464883d336a5b
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35238290"
---
# <a name="grant-access-to-create-azure-enterprise-subscriptions-preview"></a>Azure Kurumsal abonelikler (Önizleme) oluşturmak için erişim izni ver

Bir Azure müşteri olarak [Kurumsal Anlaşma (Kurumsal Sözleşme)](https://azure.microsoft.com/pricing/enterprise-agreement/), hesabınıza faturalandırılır abonelikleri oluşturmak için başka bir kullanıcı veya hizmet asıl izin verebilirsiniz. Bu makalede, nasıl kullanılacağını öğrenirsiniz [rol tabanlı erişim denetimi (RBAC)](../active-directory/role-based-access-control-configure.md) abonelikleri ve abonelik oluşturmaları denetleme oluşturma olanağı paylaşmak için.

Bir aboneliği oluşturmak için bkz: [program aracılığıyla Azure Kurumsal abonelikler (Önizleme) oluşturma](programmatically-create-subscription.md).

## <a name="delegate-access-to-an-enrollment-account-using-rbac"></a>RBAC kullanarak bir kayıt hesaba temsilci seçme

Başka bir kullanıcı veya hizmet sorumlusu belirli bir hesaba karşı abonelikleri oluşturma olanağı vermek için [kayıt hesabı kapsamındaki bir RBAC sahip rolünü vermediğiniz](../active-directory/role-based-access-control-manage-access-rest.md). Aşağıdaki örnek, Kiracı ile içinde kullanıcıya verir `principalId` , `<userObjectId>` (için SignUpEngineering@contoso.com) sahip rolü kayıt hesabındaki. Asıl Kimliğinde ve kayıt hesabını bulmak için bkz: [program aracılığıyla Azure Kurumsal abonelikler (Önizleme) oluşturma](programmatically-create-subscription.md).

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
Sahip rolü kayıt hesap kapsamda başarıyla atandığında, Azure rol ataması bilgilerle yanıt verir:

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

Kullanım [New-AzureRmRoleAssignment](../active-directory/role-based-access-control-manage-access-powershell.md) kayıt hesabınıza başka bir kullanıcıya sahip erişim vermek için.

```azurepowershell-interactive
New-AzureRmRoleAssignment -RoleDefinitionName Owner -ObjectId <userObjectId> -Scope /providers/Microsoft.Billing/enrollmentAccounts/747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

Kullanım [az rol ataması oluşturma](../active-directory/role-based-access-control-manage-access-azure-cli.md) kayıt hesabınıza başka bir kullanıcıya sahip erişim vermek için.

```azurecli-interactive 
az role assignment create --role Owner --assignee-object-id <userObjectId> --scope /providers/Microsoft.Billing/enrollmentAccounts/747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

----

Bir kullanıcı kaydı hesabınız için RBAC sahibi olduktan sonra program aracılığıyla altındaki abonelikleri oluşturabilirsiniz. Özgün hesap sahibi Hizmet Yöneticisi olarak atanmış bir kullanıcı tarafından oluşturulmuş bir abonelik hala içeriyor, ancak aynı zamanda temsilci atanan kullanıcı sahibi olarak varsayılan olarak bulunur. 

## <a name="audit-who-created-subscriptions-using-activity-logs"></a>Etkinlik günlükleri kullanarak abonelikleri oluşturan denetleme

Bu API oluşturulan abonelikleri izlemek için [Kiracı etkinlik günlüğü API](/rest/api/monitor/tenantactivitylogs). Şu anda abonelik oluşturma izlemek için PowerShell'i, CLI veya Azure portalını kullanmak mümkün değil.

1. Azure AD kiracısı bir kiracı Yöneticisi olarak [erişimini yükseltme](../active-directory/role-based-access-control-tenant-admin-access.md) kapsamı'da Denetim kullanıcıya bir okuyucu rolüne atayın `/providers/microsoft.insights/eventtypes/management`.
1. Denetim kullanıcı olarak çağrı [Kiracı etkinlik günlüğü API](/rest/api/monitor/tenantactivitylogs) abonelik oluşturma etkinlikleri görmek için. Örnek:

```
GET "/providers/Microsoft.Insights/eventtypes/management/values?api-version=2015-04-01&$filter=eventTimestamp ge '{greaterThanTimeStamp}' and eventTimestamp le '{lessThanTimestamp}' and eventChannels eq 'Operation' and resourceProvider eq 'Microsoft.Subscription'" 
```

> [!NOTE]
> Rahat komut satırından bu API çağrısı için deneyin [ARMClient](https://github.com/projectkudu/ARMClient).

## <a name="next-steps"></a>Sonraki adımlar

* Kullanıcı veya hizmet sorumlusu bir aboneliği oluşturmak için izne sahip, kimliğe kullanabilirsiniz [program aracılığıyla Azure Enterprise abonelikleri oluşturma](programmatically-create-subscription.md).
* .NET kullanarak abonelikleri oluşturma konusunda bir örnek için bkz: [örnek kodu github'da](https://github.com/Azure-Samples/create-azure-subscription-dotnet-core).
* Azure Resource Manager ve API'lerini hakkında daha fazla bilgi için bkz: [Azure Resource Manager'a genel bakış](resource-group-overview.md).
* Çok sayıda yönetim gruplarını kullanarak abonelikleri yönetme hakkında daha fazla bilgi edinmek için [kaynaklarınızı Azure Yönetim grupları ile düzenleme](management-groups-overview.md)
* Abonelik idare üzerine büyük kurumlar için kapsamlı bir en iyi uygulama kılavuzunu görmek için [Azure enterprise iskele - Düzenleyici abonelik yönetimi](/azure/architecture/cloud-adoption-guide/subscription-governance)
