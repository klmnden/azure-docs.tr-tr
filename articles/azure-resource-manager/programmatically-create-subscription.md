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
ms.date: 06/05/2018
ms.author: jlian
ms.openlocfilehash: df66ba1ec2c855a24731387210b0127892f5796f
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35234793"
---
# <a name="programmatically-create-azure-enterprise-subscriptions-preview"></a>Program aracılığıyla Azure Kurumsal abonelikler (Önizleme) oluşturma

Bir Azure müşteri olarak [Kurumsal Anlaşma (Kurumsal Sözleşme)](https://azure.microsoft.com/pricing/enterprise-agreement/), program aracılığıyla EA (MS-AZR - 0017 P) ve EA geliştirme ve Test (MS-AZR - 0148 P) abonelikleri oluşturabilirsiniz. Bu makalede, Azure Resource Manager kullanarak programlı olarak abonelikleri oluşturma öğrenin.

Bu API'Azure aboneliği oluşturduğunuzda, bu abonelik altında Microsoft Azure hizmetlerini Microsoft ya da bir yetkili satıcılarından aldığınız sözleşmesi tabidir. Daha fazla bilgi için bkz: [Microsoft Azure yasal bilgiler](https://azure.microsoft.com/support/legal/).

## <a name="prerequisites"></a>Önkoşullar

* Hesabınıza bir Azure EA kayıt içindeki bir hesap sahip olması gerekir. Değilse, kayıt için yöneticinizden [EA Portalı'nı kullanarak bir hesap sahibi olarak eklediğiniz](https://ea.azure.com/helpdocs/addNewAccount) (günlük-gerekli '). El ile ilk bir aboneliği oluşturmak için aldığınız davet e-posta'ndaki yönergeleri izleyin. Hesap sahipliğini doğrulamanız ve bir başlangıç EA abonelik sonraki adıma geçmeden önce el ile oluşturun. Eklemeniz yeterlidir kayıt hesabına yeterli değildir.

* Bir hizmet sorumlusu EA abonelik oluşturmak için kullanmak istiyorsanız, şunları yapmalısınız [bu hizmet sorumlusunun abonelikleri oluşturma olanağı vermek](grant-access-to-create-subscription.md).

## <a name="find-accounts-you-have-access-to"></a>Erişiminiz hesapları bulun

Bir hesap sahibi olarak Azure EA kayıt eklemiş sonra Azure hesap kaydını ilişki Abonelik ücretleri faturalandırmak nereye belirlemek için kullanır. Hesabı altında oluşturulan tüm abonelikleri hesabın içinde olduğu EA kayıt doğrultusunda faturalandırılır. Abonelik oluşturmak için değerleri kayıt hesap ve abonelik sahibi kullanıcı ilkeleri hakkında geçmesi gerekir. 

Aşağıdaki komutları çalıştırmak için hesap sahibinin için oturum açmanız gerekir *giriş dizini*, varsayılan olarak abonelikleri oluşturulan dizini olduğu.

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

Aşağıdaki örnek adlı abonelik oluşturmak için bir istek oluşturur *geliştirme ekibi abonelik* ve abonelik teklif *MS-AZR - 0017P* (normal EA). Kayıt hesabıdır `747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx` (yer tutucu değerini, bu değeri olan bir GUID), kayıt hesabı olduğu için SignUpEngineering@contoso.com. Ayrıca isteğe bağlı olarak iki kullanıcı abonelik için RBAC sahipleri olarak ekler.

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

Bu önizleme modülü kullanmak için çalıştırarak yüklemek `Install-Module AzureRM.Subscription -AllowPrerelease` ilk. Emin olmak için `-AllowPrerelease` çalıştığı PowerShellGet en son sürümünü yükleyin [alma PowerShellGet Modülü](/powershell/gallery/installing-psget).

Kullanım [New-AzureRmSubscription](/powershell/module/azurerm.subscription.preview) ile birlikte `enrollmentAccount` nesne kimliği olarak `EnrollmentAccountObjectId` parametresini kullanarak yeni bir abonelik oluşturun. 

```azurepowershell-interactive
New-AzureRmSubscription -OfferType MS-AZR-0017P -Name "Dev Team Subscription" -EnrollmentAccountObjectId 747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx -OwnerObjectId <userObjectId>,<servicePrincipalObjectId>
```

| Öğe adı  | Gerekli | Tür   | Açıklama                                                                                               |
|---------------|----------|--------|-----------------------------------------------------------------------------------------------------------|
| `Name` | Hayır      | Dize | Abonelik görünen adı. Belirtilmezse, "Microsoft Azure Kurumsal" gibi teklif adına ayarlanır                                 |
| `OfferType`   | Evet      | Dize | Abonelik teklif. EA için iki seçenek olan [MS-AZR - 0017P](https://azure.microsoft.com/pricing/enterprise-agreement/) (üretim kullanın) ve [MS-AZR - 0148P](https://azure.microsoft.com/offers/ms-azr-0148p/) (geliştirme ve test olması gerekir [EA Portalı'nı kullanarak açık](https://ea.azure.com/helpdocs/DevOrTestOffer)).                |
| `EnrollmentAccountObjectId`      | Evet       | Dize | Abonelik altında oluşturulur ve için fatura kayıt hesabı nesne kimliği. Bu değer aldığınız GUID'dir `Get-AzureRmEnrollmentAccount`. |
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
| `enrollment-account-object-id`      | Evet       | Dize | Abonelik altında oluşturulur ve için fatura kayıt hesabı nesne kimliği. Bu değer aldığınız GUID'dir `az billing enrollment-account list`. |
| `owner-object-id`      | Hayır       | Dize | Aboneliğe ilişkin RBAC sahibi olarak oluşturulduğunda eklemek istediğiniz herhangi bir kullanıcı nesnesi kimliği.  |
| `owner-upn`    | Hayır       | Dize | Aboneliğe ilişkin RBAC sahibi olarak oluşturulduğunda eklemek istediğiniz herhangi bir kullanıcı e-posta adresi. Bu parametre yerine kullanabileceğiniz `owner-object-id`.|
| `owner-spn` | Hayır       | Dize | Aboneliğe ilişkin RBAC sahibi olarak oluşturulduğunda eklemek istediğiniz herhangi bir hizmet asıl uygulama kimliği. Bu parametre yerine kullanabileceğiniz `owner-object-id`.| 

Tüm parametreler tam listesini görmek için bkz: [az hesabı oluşturma](/cli/azure/ext/subscription/account?view=azure-cli-latest#-ext-subscription-az-account-create).

----

## <a name="limitations-of-azure-enterprise-subscription-creation-api"></a>Azure Enterprise abonelik oluşturma API sınırlamaları

- Yalnızca Azure Enterprise abonelikleri bu API'si kullanılarak oluşturulabilir.
- Hesap başına 50 abonelik sınırı yoktur. Bundan sonra abonelikleri yalnızca hesap Merkezi'ni kullanarak oluşturulabilir.
- Var. en az bir EA veya EA geliştirme ve Test abonelik hesap sahibi el ile kaydolma en az bir kez geçti anlamına gelir hesabı altında olması gerekir.
- Hesap sahipleri değildir, ancak bir kayıt hesabına RBAC, aracılığıyla eklenen kullanıcılar hesap Merkezi'ni kullanarak abonelikleri oluşturulamıyor.
- Oluşturulması için aboneliği için Kiracı seçemezsiniz. Abonelik hesap sahibi ev kiracısında her zaman oluşturulur. Farklı bir kiracı aboneliği taşımak için bkz: [abonelik kiracısını değiştirme](..\active-directory\active-directory-how-subscriptions-associated-directory.md).

## <a name="next-steps"></a>Sonraki adımlar

* .NET kullanarak abonelikleri oluşturma konusunda bir örnek için bkz: [örnek kodu github'da](https://github.com/Azure-Samples/create-azure-subscription-dotnet-core).
* Bir abonelik oluşturduğunuza göre bu özelliği diğer kullanıcılar ve hizmet asıl adı verebilirsiniz. Daha fazla bilgi için bkz: [Azure Kurumsal abonelikler (Önizleme) oluşturmak için erişim izni verme](grant-access-to-create-subscription.md).
* Çok sayıda yönetim gruplarını kullanarak abonelikleri yönetme hakkında daha fazla bilgi edinmek için [kaynaklarınızı Azure Yönetim grupları ile düzenleme](management-groups-overview.md)
