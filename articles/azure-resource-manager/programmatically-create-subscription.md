---
title: Program aracılığıyla Azure Enterprise abonelikleri oluşturma | Microsoft Docs
description: Ek Azure Enterprise veya Kurumsal geliştirme ve Test abonelikleri program aracılığıyla oluşturmayı öğrenin.
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
ms.openlocfilehash: 36e69696b292454598faed2a95a844dc7a7442c5
ms.sourcegitcommit: a5eb246d79a462519775a9705ebf562f0444e4ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2018
ms.locfileid: "39266107"
---
# <a name="programmatically-create-azure-enterprise-subscriptions-preview"></a>Program aracılığıyla Azure Kurumsal abonelikler (Önizleme) oluşturma

Üzerinde bir Azure müşterisi olarak [Kurumsal Anlaşma (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/), program aracılığıyla EA (MS-AZR - 0017 P) ve EA geliştirme/Test (MS-AZR - 0148 P) abonelikleri oluşturabilirsiniz. Bu makalede, Azure Resource Manager kullanarak program aracılığıyla abonelikleri oluşturma öğrenin.

Bu API, bir Azure aboneliği oluşturduğunuzda, söz konusu abonelik altında Microsoft Azure hizmetlerini Microsoft veya yetkili bir satıcıdan edindiğiniz olan sözleşmeye göre yönetilir. Daha fazla bilgi için bkz. [Microsoft Azure yasal bilgileri](https://azure.microsoft.com/support/legal/).

## <a name="prerequisites"></a>Önkoşullar

* Hesabınız bir Azure EA kayıt bir hesap sahibi olması gerekir. Aksi takdirde, kayıt için yöneticinizden [EA portalı kullanarak hesap sahibi olarak eklediğiniz](https://ea.azure.com/helpdocs/addNewAccount) (gerekli oturum). El ile bir ilk aboneliklerini oluşturması için aldığınız davet e-postadaki yönergeleri izleyin. Hesap sahipliğini doğrulamak ve el ile bir sonraki adıma devam etmeden önce bir ilk EA aboneliği oluşturun. Yalnızca ekleme hesap kaydı için yeterli değildir.

* EA aboneliği oluşturmak için bir hizmet sorumlusu adını kullanmak istiyorsanız gerekir [bu hizmet sorumlusunun abonelikleri oluşturma yetkisi](grant-access-to-create-subscription.md).

## <a name="find-accounts-you-have-access-to"></a>Erişiminiz Hesapla

Azure hesap kaydını ilişki için bir hesap sahibi olarak bir Azure EA kayıt eklenen sonra abonelik ücretler fatura yerini belirlemek için kullanır. Hesabı altında oluşturulan tüm abonelikleri hesabının bulunduğu EA kayıt doğrultusunda faturalandırılırsınız. Abonelikler oluşturmak için değerleri kayıt hesabı ve abonelik sahibi kullanıcı ilkeleri hakkında geçmesi gerekir. 

Aşağıdaki komutları çalıştırmak için hesap sahibinin için oturum açmanız gerekir *giriş dizini*, varsayılan olarak abonelik oluşturduğunuz dizine olduğu.

# <a name="resttabrest"></a>[REST](#tab/rest)

Tüm kayıt hesaplarını listelemek için istek:

```json
GET https://management.azure.com/providers/Microsoft.Billing/enrollmentAccounts?api-version=2018-03-01-preview
```

Azure, erişiminiz olan tüm kayıt hesaplarının listesi ile yanıt verir:

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

Kullanım [Get-AzureRmEnrollmentAccount komut](/powershell/module/azurerm.billing/get-azurermenrollmentaccount) tüm kayıt hesaplarını listelemek için erişiminiz olması.

```azurepowershell-interactive
Get-AzureRmEnrollmentAccount
```

Azure hesabı nesne kimlikleri ve e-posta adresleri listesi ile yanıt verir.

```azurepowershell
ObjectId                               | PrincipalName
747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx   | SignUpEngineering@contoso.com
4cd2fcf6-xxxx-xxxx-xxxx-xxxxxxxxxxxx   | BillingPlatformTeam@contoso.com
```

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

Kullanım [az fatura kayıt hesabı listesi](https://aka.ms/EASubCreationPublicPreviewCLI) erişiminiz olan tüm kayıt hesaplarını listelemek için komutu.

```azurecli-interactive 
az billing enrollment-account list
```

Azure hesabı nesne kimlikleri ve e-posta adresleri listesi ile yanıt verir.

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

Kullanım `principalName` özelliği için faturalandırılmaya abonelikleri istediğiniz hesabı belirleyin. Kullanma `id` olarak `enrollmentAccount` sonraki adımda abonelik oluşturmak için kullandığınız değer.

## <a name="create-subscriptions-under-a-specific-enrollment-account"></a>Belirli kayıt hesabı altında abonelikleri oluşturma 

Aşağıdaki örnekte adlı abonelik oluşturma isteği oluşturur *geliştirme ekibi abonelik* ve abonelik teklifi *MS-AZR - 0017P* (normal EA). Kayıt hesabı `747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx` (yer tutucu değerini, bu değeri olduğu bir GUID), kayıt hesabı olduğu için SignUpEngineering@contoso.com. Ayrıca isteğe bağlı olarak iki kullanıcı abonelik için RBAC sahip olarak ekler.

# <a name="resttabrest"></a>[REST](#tab/rest)

Kullanım `id` , `enrollmentAccount` yolunda abonelik oluşturma isteği.

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
| `displayName` | Hayır      | Dize | Aboneliğin görünen adı. Belirtilmezse, adı "Microsoft Azure Kurumsal" gibi bir teklif olarak ayarlanır                                 |
| `offerType`   | Evet      | Dize | Abonelik teklifi. İki seçenekleri için EA [MS-AZR - 0017P](https://azure.microsoft.com/pricing/enterprise-agreement/) (üretim kullanımı) ve [MS-AZR - 0148P](https://azure.microsoft.com/offers/ms-azr-0148p/) (geliştirme ve test, olması gerekir [EA portal'ı kullanarak açık](https://ea.azure.com/helpdocs/DevOrTestOffer)).                |
| `owners`      | Hayır       | Dize | Nesne oluşturulduğunda bir abonelik RBAC sahip olarak eklemek istediğiniz herhangi bir kullanıcı kimliği.  |

Yanıtta ulaşırsınız bir `subscriptionOperation` izleme nesnesi. Abonelik oluşturma işlemi bittikten sonra `subscriptionOperation` nesne döndürmesine bir `subscriptionLink` abonelik kimliğine sahip nesne

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

Bu önizleme modülü kullanmak için çalıştırarak yükleyin `Install-Module AzureRM.Subscription -AllowPrerelease` ilk. Emin olmak için `-AllowPrerelease` çalıştığı PowerShellGet yeni bir sürümünü yükleme [PowerShellGet modülü Al](/powershell/gallery/installing-psget).

Kullanım [New-AzureRmSubscription](/powershell/module/azurerm.subscription) ile birlikte `enrollmentAccount` nesne kimliği olarak `EnrollmentAccountObjectId` parametresini kullanarak yeni bir abonelik oluşturun. 

```azurepowershell-interactive
New-AzureRmSubscription -OfferType MS-AZR-0017P -Name "Dev Team Subscription" -EnrollmentAccountObjectId 747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx -OwnerObjectId <userObjectId>,<servicePrincipalObjectId>
```

| Öğe adı  | Gerekli | Tür   | Açıklama                                                                                               |
|---------------|----------|--------|-----------------------------------------------------------------------------------------------------------|
| `Name` | Hayır      | Dize | Aboneliğin görünen adı. Belirtilmezse, adı "Microsoft Azure Kurumsal" gibi bir teklif olarak ayarlanır                                 |
| `OfferType`   | Evet      | Dize | Abonelik teklifi. İki seçenekleri için EA [MS-AZR - 0017P](https://azure.microsoft.com/pricing/enterprise-agreement/) (üretim kullanımı) ve [MS-AZR - 0148P](https://azure.microsoft.com/offers/ms-azr-0148p/) (geliştirme ve test, olması gerekir [EA portal'ı kullanarak açık](https://ea.azure.com/helpdocs/DevOrTestOffer)).                |
| `EnrollmentAccountObjectId`      | Evet       | Dize | Kayıt hesabı abonelik altında oluşturulur ve için faturalandırılır, nesne kimliği. Bu değer aldığınız bir GUID'dir `Get-AzureRmEnrollmentAccount`. |
| `OwnerObjectId`      | Hayır       | Dize | Nesne oluşturulduğunda bir abonelik RBAC sahip olarak eklemek istediğiniz herhangi bir kullanıcı kimliği.  |
| `OwnerSignInName`    | Hayır       | Dize | Abonelik bir RBAC sahip olarak oluşturulduğunda eklemek istediğiniz herhangi bir kullanıcı e-posta adresi. Bu parametre yerine kullanabileceğiniz `OwnerObjectId`.|
| `OwnerApplicationId` | Hayır       | Dize | Abonelik bir RBAC sahip olarak oluşturulduğunda eklemek istediğiniz herhangi bir hizmet sorumlusu uygulama kimliği. Bu parametre yerine kullanabileceğiniz `OwnerObjectId`.| 

Tüm parametreler tam listesini görmek için bkz: [New-AzureRmSubscription](/powershell/module/azurerm.subscription.preview).

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

Bu önizleme uzantısını kullanacak şekilde çalıştırarak yükleyin `az extension add --name subscription` ilk.

Kullanım [az hesabı oluşturma](/cli/azure/ext/subscription/account?view=azure-cli-latest#-ext-subscription-az-account-create) ile birlikte `enrollmentAccount` nesne kimliği olarak `enrollment-account-object-id` parametresini kullanarak yeni bir abonelik oluşturun.

```azurecli-interactive 
az account create --offer-type "MS-AZR-0017P" --display-name "Dev Team Subscription" --enrollment-account-object-id "747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx" --owner-object-id "<userObjectId>","<servicePrincipalObjectId>"
```

| Öğe adı  | Gerekli | Tür   | Açıklama                                                                                               |
|---------------|----------|--------|-----------------------------------------------------------------------------------------------------------|
| `display-name` | Hayır      | Dize | Aboneliğin görünen adı. Belirtilmezse, adı "Microsoft Azure Kurumsal" gibi bir teklif olarak ayarlanır                                 |
| `offer-type`   | Evet      | Dize | Abonelik teklifi. İki seçenekleri için EA [MS-AZR - 0017P](https://azure.microsoft.com/pricing/enterprise-agreement/) (üretim kullanımı) ve [MS-AZR - 0148P](https://azure.microsoft.com/offers/ms-azr-0148p/) (geliştirme ve test, olması gerekir [EA portal'ı kullanarak açık](https://ea.azure.com/helpdocs/DevOrTestOffer)).                |
| `enrollment-account-object-id`      | Evet       | Dize | Kayıt hesabı abonelik altında oluşturulur ve için faturalandırılır, nesne kimliği. Bu değer aldığınız bir GUID'dir `az billing enrollment-account list`. |
| `owner-object-id`      | Hayır       | Dize | Nesne oluşturulduğunda bir abonelik RBAC sahip olarak eklemek istediğiniz herhangi bir kullanıcı kimliği.  |
| `owner-upn`    | Hayır       | Dize | Abonelik bir RBAC sahip olarak oluşturulduğunda eklemek istediğiniz herhangi bir kullanıcı e-posta adresi. Bu parametre yerine kullanabileceğiniz `owner-object-id`.|
| `owner-spn` | Hayır       | Dize | Abonelik bir RBAC sahip olarak oluşturulduğunda eklemek istediğiniz herhangi bir hizmet sorumlusu uygulama kimliği. Bu parametre yerine kullanabileceğiniz `owner-object-id`.| 

Tüm parametreler tam listesini görmek için bkz: [az hesabı oluşturma](/cli/azure/ext/subscription/account?view=azure-cli-latest#-ext-subscription-az-account-create).

----

## <a name="limitations-of-azure-enterprise-subscription-creation-api"></a>Azure Kurumsal abonelik oluşturma API sınırlamaları

- Yalnızca Azure Enterprise abonelikleri bu API'si kullanılarak oluşturulabilir.
- Hesap başına 50 abonelik limiti yoktur. Bundan sonra abonelik yalnızca hesap merkezi aracılığıyla oluşturulabilir.
- Var. en az bir kurumsal Anlaşma veya EA geliştirme/Test abonelik hesap sahibi el ile kayıt en az bir kez geçti anlamına gelir hesabı altında olması gerekir.
- Hesap sahipleri değildir, ancak bir kayıt hesabı, RBAC aracılığıyla eklenen kullanıcılar kullanan hesap Merkezi'nde abonelikleri oluşturulamıyor.
- Oluşturulması için Kiracı aboneliği için seçemezsiniz. Abonelik hesap sahibi giriş kiracısında her zaman oluşturulur. Abonelik farklı bir kiracıya taşımak için bkz: [abonelik kiracısını değiştirme](../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md).

## <a name="next-steps"></a>Sonraki adımlar

* .NET kullanarak abonelikleri oluşturma örneği için bkz. [örnek kodu github'da](https://github.com/Azure-Samples/create-azure-subscription-dotnet-core).
* Bir abonelik oluşturduğunuza göre diğer kullanıcılar ve hizmet sorumluları için bu özelliği verebilirsiniz. Daha fazla bilgi için [(Önizleme) Azure Kurumsal abonelikler oluşturmak için erişim verin](grant-access-to-create-subscription.md).
* Çok sayıda Yönetim grupları kullanarak aboneliklerini yönetme hakkında daha fazla bilgi edinmek için [kaynaklarınızı Azure Yönetim grupları ile düzenleme](management-groups-overview.md)
