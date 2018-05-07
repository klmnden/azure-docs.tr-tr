---
title: Program aracılığıyla Azure Enterprise abonelikleri oluşturma | Microsoft Docs
description: Ek Azure Enterprise veya Enterprise geliştirme ve Test abonelik program aracılığıyla oluşturmayı öğrenin.
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
ms.date: 4/30/2018
ms.author: jlian
ms.openlocfilehash: 8d1eb3229f22b2da3a356562250fedb3c35c4816
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="programmatically-create-azure-enterprise-subscriptions-preview"></a>Program aracılığıyla Azure Kurumsal abonelikler (Önizleme) oluşturma

Bir Azure müşteri olarak [Kurumsal Anlaşma (Kurumsal Sözleşme)](https://azure.microsoft.com/pricing/enterprise-agreement/), program aracılığıyla EA (MS-AZR - 0017 P) ve EA geliştirme ve Test (MS-AZR - 0148 P) abonelikleri oluşturabilirsiniz. Başka bir kullanıcı veya hizmet sorumlusu hesabınıza faturalandırılır abonelikleri oluşturma izni vermek için onlara [rol tabanlı erişim denetimi (RBAC)](../active-directory/role-based-access-control-configure.md) kayıt hesabınıza erişimi. 

> [!IMPORTANT]
> Bu API oluşturulan Azure aboneliği altında Microsoft Azure hizmetlerini Microsoft ya da bir yetkili satıcılarından almış anlaşma tarafından yönetilir. Daha fazla bilgi için bkz: [Microsoft Azure yasal bilgiler](https://azure.microsoft.com/support/legal/).

Bu makalede şunları yapacaksınız:

> [!div class="checklist"]
> * Program aracılığıyla Azure Kaynak Yöneticisi'ni kullanarak abonelikleri oluşturma hakkında bilgi edinin
> * RBAC EA hesabınıza faturalandırılır abonelikleri oluşturma olanağı paylaşmak için nasıl kullanılacağını anlayın

Ayrıca bkz [.NET örnek kodu github'da](https://github.com/Azure-Samples/create-azure-subscription-dotnet-core).

## <a name="ask-your-ea-enrollment-admin-to-add-you-as-account-owner"></a>Hesap sahibi olarak eklemek için EA kayıt yöneticinizden isteyin

Başlamak için kayıt için yöneticinizden [EA portalı kullanarak hesap sahibi olarak eklediğiniz](https://ea.azure.com/helpdocs/addNewAccount) (günlük-gerekli '). El ile ilk bir aboneliği oluşturmak için aldığınız davet e-posta'ndaki yönergeleri izleyin.

> [!IMPORTANT]
> Hesap sahipliğini doğrulamanız ve bir başlangıç EA abonelik sonraki adıma geçmeden önce el ile oluşturmanız gerekir. Yalnızca hesap için kayıt eklemek yeterli değildir.

## <a name="find-accounts-you-have-access-to"></a>Erişiminiz hesapları bulun

Bir hesap sahibi olarak Azure EA kayıt eklemiş sonra Azure hesap kaydını ilişki Abonelik ücretleri faturalandırmak nereye belirlemek için kullanır. Abonelik oluşturmak için ilk önce hangi kayıt hesapları erişiminiz çıkışı bulun. Şu anda bir EA hesabına sahip olduğunuz ve bu API'yi kullanmaya çalıştığınızda, Azure için aşağıdaki koşulları denetler:

- Hesabınız için bir EA kayıt eklendi
- EA veya EA geliştirme ve Test abonelikler, bir veya daha fazla el ile kaydolma en az bir kez çıkmadığınızı anlamları
- Hesap sahibinin oturum açtınız *giriş dizini*, varsayılan olarak abonelikleri oluşturulan dizini olduğu

Yukarıdaki iki koşul karşılanıyorsa bir `enrollmentAccount` kaynak döndürülür ve bu hesapta abonelik oluşturma başlayabilirsiniz. Hesabı altında oluşturulan tüm abonelikleri hesabın içinde olduğu EA kayıt doğrultusunda faturalandırılır.

# <a name="resttabrest"></a>[REST](#tab/rest)

Tüm kayıt hesapları listelemek için istek:

```json
GET https://management.azure.com/providers/Microsoft.Billing/enrollmentAccounts?api-version=2018-03-01-preview
```

Azure erişiminiz tüm kayıt hesaplarının bir listesiyle yanıt verir:

```json
{
  "value": [
    {
      "id": "/providers/Microsoft.Billing/enrollmentAccounts/747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "name": "747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "type": "Microsoft.Billing/enrollmentAccounts",
      "properties": {
        "principalName": "SignUpEngineering@contoso.com"
      }
    },
    {
      "id": "/providers/Microsoft.Billing/enrollmentAccounts/4cd2fcf6-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "name": "4cd2fcf6-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "type": "Microsoft.Billing/enrollmentAccounts",
      "properties": {
        "principalName": "BillingPlatformTeam@contoso.com"
      }
    }
  ]
}
```

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Kullanım [Get-AzureRmEnrollmentAccount komutu](/powershell/module/azurerm.billing/get-azurermenrollmentaccount) tüm kayıt hesapları listelemek için erişebilirsiniz.

```azurepowershell-interactive
Get-AzureRmEnrollmentAccount
```

Azure hesaplarının nesne kimlikleri ve e-posta adresleri listesi ile yanıt verir.

```azurepowershell
ObjectId                               | PrincipalName
747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx   | SignUpEngineering@contoso.com
4cd2fcf6-xxxx-xxxx-xxxx-xxxxxxxxxxxx   | BillingPlatformTeam@contoso.com
```

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Kullanım [az fatura kayıt hesabı listesi](https://aka.ms/EASubCreationPublicPreviewCLI) erişiminiz tüm kayıt hesapları listelemek için komutu.

```azurecli-interactive 
az billing enrollment-account list
```
Azure hesaplarının nesne kimlikleri ve e-posta adresleri listesi ile yanıt verir.

```json
{
  "value": [
    {
      "id": "/providers/Microsoft.Billing/enrollmentAccounts/747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "name": "747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "type": "Microsoft.Billing/enrollmentAccounts",
      "properties": {
        "principalName": "SignUpEngineering@contoso.com"
      }
    },
    {
      "id": "/providers/Microsoft.Billing/enrollmentAccounts/747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "name": "4cd2fcf6-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "type": "Microsoft.Billing/enrollmentAccounts",
      "properties": {
        "principalName": "BillingPlatformTeam@contoso.com"
      }
    }
  ]
}
```

---

Kullanım `principalName` için faturalandırılmaya abonelikleri istediğiniz hesabı tanımlamak üzere özelliği. Kullanmak `id` olarak `enrollmentAccount` sonraki adımda abonelik oluşturmak için kullandığınız değer.

## <a name="create-subscriptions-under-a-specific-enrollment-account"></a>Belirli kayıt hesabı altında abonelikleri oluşturma 

Aşağıdaki örnek adlı abonelik oluşturmak için bir istek oluşturur *geliştirme ekibi abonelik* ve abonelik teklif *MS-AZR - 0017P* (normal EA). Kayıt hesabıdır `747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx` (yer tutucu değerini, bu GUID'dir), kayıt hesabı olduğu SignUpEngineering@contoso.com. Ayrıca isteğe bağlı olarak iki kullanıcı abonelik için RBAC sahipleri olarak ekler.

# <a name="resttabrest"></a>[REST](#tab/rest)

Kullanım `id` , `enrollmentAccount` abonelik oluşturmak için istek yolundaki.

```json
POST https://management.azure.com/providers/Microsoft.Billing/enrollmentAccounts/747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx/providers/Microsoft.Subscription/createSubscription?api-version=2018-03-01-preview

{
  "displayName": "Dev Team Subscription",
  "offerType": "MS-AZR-0017P",
  "owners": [
    {
      "objectId": "<userObjectId>"
    },
    {
      "objectId": "<servicePrincipalObjectId>"
    }
  ]
}
```

| Öğe adı  | Gerekli | Tür   | Açıklama                                                                                               |
|---------------|----------|--------|-----------------------------------------------------------------------------------------------------------|
| `displayName` | Hayır      | Dize | Abonelik görünen adı. Belirtilmezse, "Microsoft Azure Kurumsal" gibi teklif adına ayarlanır                                 |
| `offerType`   | Evet      | Dize | Abonelik teklif. EA için iki seçenek olan [MS-AZR - 0017P](https://azure.microsoft.com/pricing/enterprise-agreement/) (üretim kullanın) ve [MS-AZR - 0148P](https://azure.microsoft.com/offers/ms-azr-0148p/) (geliştirme ve test olması gerekir [EA Portalı'nı kullanarak açık](https://ea.azure.com/helpdocs/DevOrTestOffer)).                |
| `owners`      | Hayır       | Dize | Aboneliğe ilişkin RBAC sahibi olarak oluşturulduğunda eklemek istediğiniz herhangi bir kullanıcı nesnesi kimliği.  |

Yanıtta ulaşırsınız bir `subscriptionOperation` izleme nesnesi. Abonelik oluşturma tamamlandığında, `subscriptionOperation` nesne döndürebildiği bir `subscriptionLink` abonelik kimliğine sahip nesne

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

Bu önizleme modülü kullanmak için çalıştırarak yüklemek `Install-Module AzureRM.Subscription -AllowPrerelease` ilk. Emin olmak için `-AllowPrerelease` çalıştığı PowerShellGet en son sürümünü yükleyin [alma PowerShellGet Modülü](/powershell/gallery/psget/get_psget_module).

Kullanım [New-AzureRmSubscription](/powershell/module/azurerm.subscription.preview) ile birlikte `enrollmentAccount` nesne kimliği olarak `EnrollmentAccountObjectId` parametresini kullanarak yeni bir abonelik oluşturun. 

```azurepowershell-interactive
New-AzureRmSubscription -OfferType MS-AZR-0017P -Name "Dev Team Subscription" -EnrollmentAccountObjectId 747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx -OwnerObjectId <userObjectId>,<servicePrincipalObjectId>
```

| Öğe adı  | Gerekli | Tür   | Açıklama                                                                                               |
|---------------|----------|--------|-----------------------------------------------------------------------------------------------------------|
| `Name` | Hayır      | Dize | Abonelik görünen adı. Belirtilmezse, "Microsoft Azure Kurumsal" gibi teklif adına ayarlanır                                 |
| `OfferType`   | Evet      | Dize | Abonelik teklif. EA için iki seçenek olan [MS-AZR - 0017P](https://azure.microsoft.com/pricing/enterprise-agreement/) (üretim kullanın) ve [MS-AZR - 0148P](https://azure.microsoft.com/offers/ms-azr-0148p/) (geliştirme ve test olması gerekir [EA Portalı'nı kullanarak açık](https://ea.azure.com/helpdocs/DevOrTestOffer)).                |
| `EnrollmentAccountObjectId`      | Evet       | Dize | Abonelik altında oluşturulur ve için fatura kayıt hesabı nesne kimliği. Bu aldığınız bir GUID değeridir `Get-AzureRmEnrollmentAccount`. |
| `OwnerObjectId`      | Hayır       | Dize | Aboneliğe ilişkin RBAC sahibi olarak oluşturulduğunda eklemek istediğiniz herhangi bir kullanıcı nesnesi kimliği.  |
| `OwnerSignInName`    | Hayır       | Dize | Aboneliğe ilişkin RBAC sahibi olarak oluşturulduğunda eklemek istediğiniz herhangi bir kullanıcı e-posta adresi. Bu parametre yerine kullanabileceğiniz `OwnerObjectId`.|
| `OwnerApplicationId` | Hayır       | Dize | Aboneliğe ilişkin RBAC sahibi olarak oluşturulduğunda eklemek istediğiniz herhangi bir hizmet asıl uygulama kimliği. Bu parametre yerine kullanabileceğiniz `OwnerObjectId`.| 

Tüm parametreler tam listesini görmek için bkz: [New-AzureRmSubscription](/powershell/module/azurerm.subscription.preview).

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

Bu önizleme uzantısını kullanacak şekilde çalıştırarak yüklemek `az extension add --name subscription` ilk.

Kullanım [az hesabı oluşturma](/cli/azure/ext/subscription/account?view=azure-cli-latest#-ext-subscription-az-account-create) ile birlikte `enrollmentAccount` nesne kimliği olarak `enrollment-account-object-id` parametresini kullanarak yeni bir abonelik oluşturun.

```azurecli-interactive 
az account create --offer-type "MS-AZR-0017P" --display-name "Dev Team Subscription" --enrollment-account-object-id "747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx" --owner-object-id "<userObjectId>","<servicePrincipalObjectId>"
```

| Öğe adı  | Gerekli | Tür   | Açıklama                                                                                               |
|---------------|----------|--------|-----------------------------------------------------------------------------------------------------------|
| `display-name` | Hayır      | Dize | Abonelik görünen adı. Belirtilmezse, "Microsoft Azure Kurumsal" gibi teklif adına ayarlanır                                 |
| `offer-type`   | Evet      | Dize | Abonelik teklif. EA için iki seçenek olan [MS-AZR - 0017P](https://azure.microsoft.com/pricing/enterprise-agreement/) (üretim kullanın) ve [MS-AZR - 0148P](https://azure.microsoft.com/offers/ms-azr-0148p/) (geliştirme ve test olması gerekir [EA Portalı'nı kullanarak açık](https://ea.azure.com/helpdocs/DevOrTestOffer)).                |
| `enrollment-account-object-id`      | Evet       | Dize | Abonelik altında oluşturulur ve için fatura kayıt hesabı nesne kimliği. Bu aldığınız bir GUID değeridir `az billing enrollment-account list`. |
| `owner-object-id`      | Hayır       | Dize | Aboneliğe ilişkin RBAC sahibi olarak oluşturulduğunda eklemek istediğiniz herhangi bir kullanıcı nesnesi kimliği.  |
| `owner-upn`    | Hayır       | Dize | Aboneliğe ilişkin RBAC sahibi olarak oluşturulduğunda eklemek istediğiniz herhangi bir kullanıcı e-posta adresi. Bu parametre yerine kullanabileceğiniz `owner-object-id`.|
| `owner-spn` | Hayır       | Dize | Aboneliğe ilişkin RBAC sahibi olarak oluşturulduğunda eklemek istediğiniz herhangi bir hizmet asıl uygulama kimliği. Bu parametre yerine kullanabileceğiniz `owner-object-id`.| 

Tüm parametreler tam listesini görmek için bkz: [az hesabı oluşturma](/cli/azure/ext/subscription/account?view=azure-cli-latest#-ext-subscription-az-account-create).

----

## <a name="delegate-access-to-an-enrollment-account-using-rbac"></a>RBAC kullanarak bir kayıt hesaba temsilci seçme

Başka bir kullanıcı veya hizmet sorumlusu belirli bir hesaba karşı abonelikleri oluşturma olanağı vermek için [kayıt hesabı kapsamındaki bir RBAC sahip rolünü vermediğiniz](../active-directory/role-based-access-control-manage-access-rest.md). Aşağıdaki örnek, Kiracı ile içinde kullanıcıya verir `principalId` , `<userObjectId>` (için SignUpEngineering@contoso.com) sahip rolü kayıt hesabındaki. 

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

## <a name="limitations-of-azure-enterprise-subscription-creation-api"></a>Azure Enterprise abonelik oluşturma API sınırlamaları

- Yalnızca Azure Enterprise abonelikleri bu API'si kullanılarak oluşturulabilir.
- Hesap başına 50 abonelik sınırı yoktur. Bundan sonra abonelikleri yalnızca hesap Merkezi'ni kullanarak oluşturulabilir.
- Var. en az bir EA veya EA geliştirme ve Test abonelik hesap sahibi el ile kaydolma en az bir kez geçti anlamına gelir hesabı altında olması gerekir.
- Hesap sahipleri değildir, ancak bir kayıt hesabına RBAC, aracılığıyla eklenen kullanıcılar hesap Merkezi'ni kullanarak abonelikleri oluşturulamıyor.
- Oluşturulması için aboneliği için Kiracı seçemezsiniz. Abonelik hesap sahibi ev kiracısında her zaman oluşturulur. Farklı bir kiracı aboneliği taşımak için bkz: [abonelik kiracısını değiştirme](..\active-directory\active-directory-how-subscriptions-associated-directory.md).

## <a name="next-steps"></a>Sonraki adımlar

* .NET kullanarak abonelikleri oluşturma konusunda bir örnek için bkz: [örnek kodu github'da](https://github.com/Azure-Samples/create-azure-subscription-dotnet-core).
* Azure Resource Manager ve API'lerini hakkında daha fazla bilgi için bkz: [Azure Resource Manager'a genel bakış](resource-group-overview.md).
* Çok sayıda yönetim gruplarını kullanarak abonelikleri yönetme hakkında daha fazla bilgi edinmek için [kaynaklarınızı Azure Yönetim grupları ile düzenleme](management-groups-overview.md)
* Abonelik idare üzerine büyük kurumlar için kapsamlı bir en iyi uygulama kılavuzunu görmek için [Azure enterprise iskele - Düzenleyici abonelik yönetimi](resource-manager-subscription-governance.md)
