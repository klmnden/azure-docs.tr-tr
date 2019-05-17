---
title: Program aracılığıyla Azure Enterprise abonelikleri oluşturma | Microsoft Docs
description: Ek Azure Enterprise veya Kurumsal geliştirme ve Test abonelikleri program aracılığıyla oluşturmayı öğrenin.
services: azure-resource-manager
author: jureid
manager: jureid
editor: ''
ms.assetid: ''
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/10/2019
ms.author: jureid
ms.openlocfilehash: 7985451eb2bb5e9fd4fbcfb3d2fcf35149122c15
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65796071"
---
# <a name="programmatically-create-azure-enterprise-subscriptions-preview"></a>Program aracılığıyla Azure Kurumsal abonelikler (Önizleme) oluşturma

Üzerinde bir Azure müşterisi olarak [Kurumsal Anlaşma (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/), program aracılığıyla EA (MS-AZR - 0017 P) ve EA geliştirme/Test (MS-AZR - 0148 P) abonelikleri oluşturabilirsiniz. Bu makalede, Azure Resource Manager kullanarak program aracılığıyla abonelikleri oluşturma öğrenin.

Bu API, bir Azure aboneliği oluşturduğunuzda, söz konusu abonelik altında Microsoft Azure hizmetlerini Microsoft veya yetkili bir satıcıdan edindiğiniz olan sözleşmeye göre yönetilir. Daha fazla bilgi için bkz. [Microsoft Azure yasal bilgileri](https://azure.microsoft.com/support/legal/).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Önkoşullar

Bir sahip rolü altında abonelikler oluşturmak istediğiniz kayıt hesabı olması gerekir. Bu roller almanın iki yolu vardır:

* Kayıt Yöneticisi [bir hesap sahibi olun](https://ea.azure.com/helpdocs/addNewAccount) (oturum açma gereklidir) getiren, kayıt hesabı sahibi. El ile bir ilk aboneliklerini oluşturması için aldığınız davet e-postadaki yönergeleri izleyin. Hesap sahipliğini doğrulamak ve el ile bir sonraki adıma devam etmeden önce bir ilk EA aboneliği oluşturun. Yalnızca ekleme hesap kaydı için yeterli değildir.

* Mevcut bir kayıt hesabı sahibi olabilir [size erişim vermesini](grant-access-to-create-subscription.md). Benzer şekilde, EA aboneliği oluşturmak için bir hizmet sorumlusu adını kullanmak istiyorsanız gerekir [bu hizmet sorumlusunun abonelikleri oluşturma yetkisi](grant-access-to-create-subscription.md).

## <a name="find-accounts-you-have-access-to"></a>Erişiminiz Hesapla

Azure hesap kaydını ilişki için bir hesap sahibi olarak bir Azure EA kayıt eklenen sonra abonelik ücretler fatura yerini belirlemek için kullanır. Hesabı altında oluşturulan tüm abonelikleri hesabının bulunduğu EA kayıt doğrultusunda faturalandırılırsınız. Abonelikler oluşturmak için değerleri kayıt hesabı ve abonelik sahibi kullanıcı ilkeleri hakkında geçmesi gerekir. 

Aşağıdaki komutları çalıştırmak için hesap sahibinin için oturum açmanız gerekir *giriş dizini*, varsayılan olarak abonelik oluşturduğunuz dizine olduğu.

## <a name="resttabrest"></a>[REST](#tab/rest)

Erişiminiz olan tüm kayıt hesaplarını listelemek için istek:

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

Kullanım `principalName` özelliği için faturalandırılmaya abonelikleri istediğiniz hesabı belirleyin. Kopyalama `name` o hesabın. Örneğin, abonelikler oluşturmak istiyorsanız SignUpEngineering@contoso.com kayıt hesabı kopyalayın ```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx```. Kayıt hesabı nesne kimliğidir. Bu değer bir sonraki adımda kullanmak yere yapıştırın `enrollmentAccountObjectId`.

## <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

Açık [Azure Cloud Shell](https://shell.azure.com/) ve PowerShell seçin.

Kullanım [Get-AzEnrollmentAccount](/powershell/module/az.billing/get-azenrollmentaccount) erişiminiz olan tüm kayıt hesaplarını listelemek için kullanın.

```azurepowershell-interactive
Get-AzEnrollmentAccount
```

Azure erişiminiz kayıt hesaplarının listesi ile yanıt verir:

```azurepowershell
ObjectId                               | PrincipalName
747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx   | SignUpEngineering@contoso.com
4cd2fcf6-xxxx-xxxx-xxxx-xxxxxxxxxxxx   | BillingPlatformTeam@contoso.com
```
Kullanım `principalName` özelliği için faturalandırılmaya abonelikleri istediğiniz hesabı belirleyin. Kopyalama `ObjectId` o hesabın. Örneğin, abonelikler oluşturmak istiyorsanız SignUpEngineering@contoso.com kayıt hesabı kopyalayın ```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx```. Bu nesne kimliği bir sonraki adımda kullanmak bir yere yapıştırmak `enrollmentAccountObjectId`.

## <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

Kullanım [az fatura kayıt hesabı listesi](https://aka.ms/EASubCreationPublicPreviewCLI) erişiminiz olan tüm kayıt hesaplarını listelemek için komutu.

```azurecli-interactive 
az billing enrollment-account list
```

Azure erişiminiz kayıt hesaplarının listesi ile yanıt verir:

```json
[
  {
    "id": "/providers/Microsoft.Billing/enrollmentAccounts/747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "name": "747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "principalName": "SignUpEngineering@contoso.com",
    "type": "Microsoft.Billing/enrollmentAccounts",
  },
  {
    "id": "/providers/Microsoft.Billing/enrollmentAccounts/747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "name": "4cd2fcf6-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "principalName": "BillingPlatformTeam@contoso.com",
    "type": "Microsoft.Billing/enrollmentAccounts",
  }
]
```

Kullanım `principalName` özelliği için faturalandırılmaya abonelikleri istediğiniz hesabı belirleyin. Kopyalama `name` o hesabın. Örneğin, abonelikler oluşturmak istiyorsanız SignUpEngineering@contoso.com kayıt hesabı kopyalayın ```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx```. Kayıt hesabı nesne kimliğidir. Bu değer bir sonraki adımda kullanmak yere yapıştırın `enrollmentAccountObjectId`.

---

## <a name="create-subscriptions-under-a-specific-enrollment-account"></a>Belirli kayıt hesabı altında abonelikleri oluşturma

Aşağıdaki örnekte adlı bir abonelik oluşturur *geliştirme ekibi abonelik* önceki adımda seçtiğiniz kayıt hesabı. Abonelik teklif *MS-AZR - 0017P* (Normal Microsoft Kurumsal anlaşması). Ayrıca isteğe bağlı olarak iki kullanıcı abonelik için RBAC sahip olarak ekler.

# <a name="resttabrest"></a>[REST](#tab/rest)

Aşağıdaki isteği yapmak, değiştirme `<enrollmentAccountObjectId>` ile `name` ilk adımda kopyaladığınız (```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx```). Sahipleri belirtmek istiyorsanız, bilgi [kullanıcı nesne kimliklerini almak nasıl](grant-access-to-create-subscription.md#userObjectId).

```json
POST https://management.azure.com/providers/Microsoft.Billing/enrollmentAccounts/<enrollmentAccountObjectId>/providers/Microsoft.Subscription/createSubscription?api-version=2018-03-01-preview

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
| `displayName` | Hayır      | String | Aboneliğin görünen adı. Belirtilmezse, adı "Microsoft Azure Kurumsal" gibi bir teklif olarak ayarlanır                                 |
| `offerType`   | Evet      | String | Abonelik teklifi. İki seçenekleri için EA [MS-AZR - 0017P](https://azure.microsoft.com/pricing/enterprise-agreement/) (üretim kullanımı) ve [MS-AZR - 0148P](https://azure.microsoft.com/offers/ms-azr-0148p/) (geliştirme ve test, olması gerekir [EA portal'ı kullanarak açık](https://ea.azure.com/helpdocs/DevOrTestOffer)).                |
| `owners`      | Hayır       | String | Nesne oluşturulduğunda bir abonelik RBAC sahip olarak eklemek istediğiniz herhangi bir kullanıcı kimliği.  |

Yanıtta ulaşırsınız bir `subscriptionOperation` izleme nesnesi. Abonelik oluşturma işlemi bittikten sonra `subscriptionOperation` nesne döndürmesine bir `subscriptionLink` abonelik kimliğine sahip nesne

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

İlk olarak, bu Önizleme modülü çalıştırarak yükleyin `Install-Module Az.Subscription -AllowPrerelease`. Emin olmak için `-AllowPrerelease` çalıştığı PowerShellGet yeni bir sürümünü yükleme [PowerShellGet modülü Al](/powershell/gallery/installing-psget).

Çalıştırma [yeni AzSubscription](/powershell/module/az.subscription) altındaki değiştirme komutu `<enrollmentAccountObjectId>` ile `ObjectId` ilk adımda toplanan (```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx```). Sahipleri belirtmek istiyorsanız, bilgi [kullanıcı nesne kimliklerini almak nasıl](grant-access-to-create-subscription.md#userObjectId).

```azurepowershell-interactive
New-AzSubscription -OfferType MS-AZR-0017P -Name "Dev Team Subscription" -EnrollmentAccountObjectId <enrollmentAccountObjectId> -OwnerObjectId <userObjectId1>,<servicePrincipalObjectId>
```

| Öğe adı  | Gerekli | Tür   | Açıklama                                                                                               |
|---------------|----------|--------|-----------------------------------------------------------------------------------------------------------|
| `Name` | Hayır      | String | Aboneliğin görünen adı. Belirtilmezse, adı "Microsoft Azure Kurumsal" gibi bir teklif olarak ayarlanır                                 |
| `OfferType`   | Evet      | String | Abonelik teklifi. İki seçenekleri için EA [MS-AZR - 0017P](https://azure.microsoft.com/pricing/enterprise-agreement/) (üretim kullanımı) ve [MS-AZR - 0148P](https://azure.microsoft.com/offers/ms-azr-0148p/) (geliştirme ve test, olması gerekir [EA portal'ı kullanarak açık](https://ea.azure.com/helpdocs/DevOrTestOffer)).                |
| `EnrollmentAccountObjectId`      | Evet       | String | Kayıt hesabı abonelik altında oluşturulur ve için faturalandırılır, nesne kimliği. Bu değer aldığınız bir GUID'dir `Get-AzEnrollmentAccount`. |
| `OwnerObjectId`      | Hayır       | String | Nesne oluşturulduğunda bir abonelik RBAC sahip olarak eklemek istediğiniz herhangi bir kullanıcı kimliği.  |
| `OwnerSignInName`    | Hayır       | String | Abonelik bir RBAC sahip olarak oluşturulduğunda eklemek istediğiniz herhangi bir kullanıcı e-posta adresi. Bu parametre yerine kullanabileceğiniz `OwnerObjectId`.|
| `OwnerApplicationId` | Hayır       | String | Abonelik bir RBAC sahip olarak oluşturulduğunda eklemek istediğiniz herhangi bir hizmet sorumlusu uygulama kimliği. Bu parametre yerine kullanabileceğiniz `OwnerObjectId`. Bu parametreyi kullanırken, hizmet sorumlusu olmalıdır [dizine okuma erişimi](/powershell/azure/active-directory/signing-in-service-principal?view=azureadps-2.0#give-the-service-principal-reader-access-to-the-current-tenant-get-azureaddirectoryrole).| 

Tüm parametreler tam listesini görmek için bkz: [yeni AzSubscription](/powershell/module/az.subscription.preview).

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

İlk olarak, bu Önizleme uzantısı çalıştırarak yükleyin `az extension add --name subscription`.

Çalıştırma [az hesabı oluşturma](/cli/azure/ext/subscription/account?view=azure-cli-latest#-ext-subscription-az-account-create) altındaki değiştirme komutu `<enrollmentAccountObjectId>` ile `name` ilk adımda kopyaladığınız (```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx```). Sahipleri belirtmek istiyorsanız, bilgi [kullanıcı nesne kimliklerini almak nasıl](grant-access-to-create-subscription.md#userObjectId).

```azurecli-interactive 
az account create --offer-type "MS-AZR-0017P" --display-name "Dev Team Subscription" --enrollment-account-object-id "<enrollmentAccountObjectId>" --owner-object-id "<userObjectId>","<servicePrincipalObjectId>"
```

| Öğe adı  | Gerekli | Tür   | Açıklama                                                                                               |
|---------------|----------|--------|-----------------------------------------------------------------------------------------------------------|
| `display-name` | Hayır      | String | Aboneliğin görünen adı. Belirtilmezse, adı "Microsoft Azure Kurumsal" gibi bir teklif olarak ayarlanır                                 |
| `offer-type`   | Evet      | String | Abonelik teklifi. İki seçenekleri için EA [MS-AZR - 0017P](https://azure.microsoft.com/pricing/enterprise-agreement/) (üretim kullanımı) ve [MS-AZR - 0148P](https://azure.microsoft.com/offers/ms-azr-0148p/) (geliştirme ve test, olması gerekir [EA portal'ı kullanarak açık](https://ea.azure.com/helpdocs/DevOrTestOffer)).                |
| `enrollment-account-object-id`      | Evet       | String | Kayıt hesabı abonelik altında oluşturulur ve için faturalandırılır, nesne kimliği. Bu değer aldığınız bir GUID'dir `az billing enrollment-account list`. |
| `owner-object-id`      | Hayır       | String | Nesne oluşturulduğunda bir abonelik RBAC sahip olarak eklemek istediğiniz herhangi bir kullanıcı kimliği.  |
| `owner-upn`    | Hayır       | String | Abonelik bir RBAC sahip olarak oluşturulduğunda eklemek istediğiniz herhangi bir kullanıcı e-posta adresi. Bu parametre yerine kullanabileceğiniz `owner-object-id`.|
| `owner-spn` | Hayır       | String | Abonelik bir RBAC sahip olarak oluşturulduğunda eklemek istediğiniz herhangi bir hizmet sorumlusu uygulama kimliği. Bu parametre yerine kullanabileceğiniz `owner-object-id`. Bu parametreyi kullanırken, hizmet sorumlusu olmalıdır [dizine okuma erişimi](/powershell/azure/active-directory/signing-in-service-principal?view=azureadps-2.0#give-the-service-principal-reader-access-to-the-current-tenant-get-azureaddirectoryrole).| 

Tüm parametreler tam listesini görmek için bkz: [az hesabı oluşturma](/cli/azure/ext/subscription/account?view=azure-cli-latest#-ext-subscription-az-account-create).

----

## <a name="limitations-of-azure-enterprise-subscription-creation-api"></a>Azure Kurumsal abonelik oluşturma API sınırlamaları

- Yalnızca Azure Enterprise abonelikleri bu API'si kullanılarak oluşturulabilir.
- Kayıt hesabı başına 50 aboneliklerinin ilk bir sınır yoktur ancak yapabilecekleriniz [bir destek isteği oluşturma](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) 200 sayısı sınırını artırmak için. Bundan sonra abonelik yalnızca hesap merkezi oluşturulabilir.
- Var. en az bir kurumsal Anlaşma veya EA geliştirme/Test abonelik hesap sahibi el ile kayıt en az bir kez geçti anlamına gelir hesabı altında olması gerekir.
- Hesap sahipleri değildir, ancak bir kayıt hesabı, RBAC aracılığıyla eklenen kullanıcılar kullanan hesap Merkezi'nde abonelikleri oluşturulamıyor.
- Oluşturulması için Kiracı aboneliği için seçemezsiniz. Abonelik hesap sahibi giriş kiracısında her zaman oluşturulur. Abonelik farklı bir kiracıya taşımak için bkz: [abonelik kiracısını değiştirme](../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md).

## <a name="next-steps"></a>Sonraki adımlar

* .NET kullanarak abonelikleri oluşturma örneği için bkz. [örnek kodu github'da](https://github.com/Azure-Samples/create-azure-subscription-dotnet-core).
* Bir abonelik oluşturduğunuza göre diğer kullanıcılar ve hizmet sorumluları için bu özelliği verebilirsiniz. Daha fazla bilgi için [(Önizleme) Azure Kurumsal abonelikler oluşturmak için erişim verin](grant-access-to-create-subscription.md).
* Çok sayıda Yönetim grupları kullanarak aboneliklerini yönetme hakkında daha fazla bilgi edinmek için [kaynaklarınızı Azure Yönetim grupları ile düzenleme](management-groups-overview.md)
