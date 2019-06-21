---
title: Azure Kurumsal abonelikler oluşturmak için erişim verme | Microsoft Docs
description: Bir kullanıcı veya hizmet sorumlusu program aracılığıyla Azure Enterprise abonelikleri oluşturma olanağı sağlayacak öğrenin.
services: azure-resource-manager
author: jureid
manager: jureid
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: jureid
ms.openlocfilehash: a7ed7dffd27b51c1314c4293820dc33be4d7e8e0
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67206634"
---
# <a name="grant-access-to-create-azure-enterprise-subscriptions-preview"></a>Azure Kurumsal abonelikler (Önizleme) oluşturma erişimi verme

Üzerinde bir Azure müşterisi olarak [Kurumsal Anlaşma (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/), faturalandırılan abonelikler oluşturmak için başka bir kullanıcı veya hizmet sorumlusu izin verebilirsiniz. Bu makalede, kullanmayı öğrenirsiniz [rol tabanlı erişim denetimi (RBAC)](../active-directory/role-based-access-control-configure.md) abonelikleri ve abonelik oluşturma denetleme oluşturma olanağı paylaşmak için. Sahip rolü, paylaşmak istediğiniz hesabı olması gerekir.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="grant-access"></a>Erişim verme

İçin [kayıt hesabı kapsamında abonelikleri oluşturabilir](programmatically-create-subscription.md), kullanıcılar [RBAC sahip rolü](../role-based-access-control/built-in-roles.md#owner) o hesapta. Aşağıdaki adımları izleyerek bir kullanıcı veya kullanıcılar grubunun kayıt hesaplarında RBAC sahip rolü verebilirsiniz:

1. Erişim vermek istediğiniz kayıt hesabı nesne Kimliğini alın

    Diğer kayıt hesaplarında RBAC sahip rolü vermek için ya da hesap sahibi veya hesabın bir RBAC sahip olması gerekir.

    # <a name="resttabrest"></a>[REST](#tab/rest)

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

    Kullanım `principalName` RBAC sahip erişimi vermek istediğiniz hesabı tanımlamak için kullanılan özellik. Kopyalama `name` o hesabın. Örneğin, RBAC sahip erişimi vermek istediyseniz SignUpEngineering@contoso.com kayıt hesabı kopyalama ```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx```. Kayıt hesabı nesne kimliğidir. Bu değer bir sonraki adımda kullanmak yere yapıştırın `enrollmentAccountObjectId`.

    # <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

    Kullanım [Get-AzEnrollmentAccount](/powershell/module/az.billing/get-azenrollmentaccount) erişiminiz olan tüm kayıt hesaplarını listelemek için kullanın. Seçin **deneyin** açmak için [Azure Cloud Shell](https://shell.azure.com/). Kod yapıştırmak için Kabuğu windows Seç sağ tıklayın ve **yapıştırın**.

    ```azurepowershell-interactive
    Get-AzEnrollmentAccount
    ```

    Azure erişiminiz kayıt hesaplarının listesi ile yanıt verir:

    ```azurepowershell
    ObjectId                               | PrincipalName
    747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx   | SignUpEngineering@contoso.com
    4cd2fcf6-xxxx-xxxx-xxxx-xxxxxxxxxxxx   | BillingPlatformTeam@contoso.com
    ```

    Kullanım `principalName` RBAC sahip erişimi vermek istediğiniz hesabı tanımlamak için kullanılan özellik. Kopyalama `ObjectId` o hesabın. Örneğin, RBAC sahip erişimi vermek istediyseniz SignUpEngineering@contoso.com kayıt hesabı kopyalama ```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx```. Bu nesne kimliği bir sonraki adımda kullanmak bir yere yapıştırmak `enrollmentAccountObjectId`.

    # <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

    Kullanım [az fatura kayıt hesabı listesi](https://aka.ms/EASubCreationPublicPreviewCLI) erişiminiz olan tüm kayıt hesaplarını listelemek için komutu. Seçin **deneyin** açmak için [Azure Cloud Shell](https://shell.azure.com/). Kod yapıştırmak için Kabuğu windows Seç sağ tıklayın ve **yapıştırın**.

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

    ---

    Kullanım `principalName` RBAC sahip erişimi vermek istediğiniz hesabı tanımlamak için kullanılan özellik. Kopyalama `name` o hesabın. Örneğin, RBAC sahip erişimi vermek istediyseniz SignUpEngineering@contoso.com kayıt hesabı kopyalama ```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx```. Kayıt hesabı nesne kimliğidir. Bu değer bir sonraki adımda kullanmak yere yapıştırın `enrollmentAccountObjectId`.

1. <a id="userObjectId"></a>Kullanıcı veya grup için RBAC sahip rolünü vermek istediğiniz nesne Kimliğini alın

    1. Azure portalında arama **Azure Active Directory**.
    1. Bir kullanıcıya erişim vermek istiyorsanız, tıklayarak **kullanıcılar** sol taraftaki menüden. Bir gruba erişim vermek istiyorsanız, tıklayın **grupları**.
    1. Kullanıcı veya grup için RBAC sahip rolünü vermek istediğiniz seçin.
    1. Bir kullanıcı seçtiğinizde, nesne kimliği Profil sayfasında bulabilirsiniz. Bir grubu seçtiğinizde, genel bakış sayfasında bulunan nesne kimliği olur. Kopyalama **objectID** metin kutusunun sağındaki simgesine tıklayarak. Sonraki adımda kullanabilmesi için bu bir yere yapıştırmak `userObjectId`.

1. Kullanıcının izni veya RBAC sahip rolü kayıt hesabındaki grubu

    İlk iki adımını toplanan değerler kullanarak, kullanıcı vermek veya RBAC sahip rolü kayıt hesabındaki grup.

    # <a name="resttabrest-2"></a>[REST](#tab/rest-2)

    Aşağıdaki komutu çalıştırın değiştirerek ```<enrollmentAccountObjectId>``` ile `name` ilk adımda kopyaladığınız (```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx```). Değiştirin ```<userObjectId>``` ikinci adımda kopyaladığınız nesne Kimliğine sahip.

    ```json
    PUT  https://management.azure.com/providers/Microsoft.Billing/enrollmentAccounts/<enrollmentAccountObjectId>/providers/Microsoft.Authorization/roleAssignments/<roleAssignmentGuid>?api-version=2015-07-01

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

    # <a name="powershelltabazure-powershell-2"></a>[PowerShell](#tab/azure-powershell-2)

    Aşağıdaki komutu çalıştırın [yeni AzRoleAssignment](../active-directory/role-based-access-control-manage-access-powershell.md) komutuyla değiştirerek ```<enrollmentAccountObjectId>``` ile `ObjectId` ilk adımda toplanan (```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx```). Değiştirin ```<userObjectId>``` nesneyle ikinci adım kimliği toplanmadı.

    ```azurepowershell-interactive
    New-AzRoleAssignment -RoleDefinitionName Owner -ObjectId <userObjectId> -Scope /providers/Microsoft.Billing/enrollmentAccounts/<enrollmentAccountObjectId>
    ```

    # <a name="azure-clitabazure-cli-2"></a>[Azure CLI](#tab/azure-cli-2)

    Aşağıdaki komutu çalıştırın [az rol ataması oluşturma](../active-directory/role-based-access-control-manage-access-azure-cli.md) komutuyla değiştirerek ```<enrollmentAccountObjectId>``` ile `name` ilk adımda kopyaladığınız (```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx```). Değiştirin ```<userObjectId>``` nesneyle ikinci adım kimliği toplanmadı.

    ```azurecli-interactive
    az role assignment create --role Owner --assignee-object-id <userObjectId> --scope /providers/Microsoft.Billing/enrollmentAccounts/<enrollmentAccountObjectId>
    ```

    Bir kullanıcı bir kayıt hesabınız için RBAC sahip olduktan sonra yapabilirler [programlı abonelikler oluşturmak](programmatically-create-subscription.md) altındaki. Yetkilendirilmiş bir kullanıcı tarafından oluşturulmuş bir abonelik hala özgün hesap sahibi olarak Hizmet Yöneticisi vardır, ancak ayrıca yetkilendirilmiş kullanıcının RBAC sahip olarak varsayılan olarak sahiptir.

    ---

## <a name="audit-who-created-subscriptions-using-activity-logs"></a>Etkinlik günlüklerini kullanarak abonelikleri oluşturan denetim

Bu API aracılığıyla oluşturulan abonelikleri izlemek için [Kiracı etkinlik günlüğü API](/rest/api/monitor/tenantactivitylogs). Şu anda abonelik oluşturma izlemek için PowerShell, CLI veya Azure portalını kullanmak mümkün değildir.

1. Azure AD kiracısının kiracı yöneticisi olarak, [erişimi yükseltin](../active-directory/role-based-access-control-tenant-admin-access.md) ve sonra da `/providers/microsoft.insights/eventtypes/management` kapsamı üzerinden denetleyen kullanıcıya Okuyucu rolü atayın.
1. Denetim kullanıcı olarak çağrı [Kiracı etkinlik günlüğü API](/rest/api/monitor/tenantactivitylogs) abonelik oluşturma etkinlikleri görmek için. Örnek:

    ```
    GET "/providers/Microsoft.Insights/eventtypes/management/values?api-version=2015-04-01&$filter=eventTimestamp ge '{greaterThanTimeStamp}' and eventTimestamp le '{lessThanTimestamp}' and eventChannels eq 'Operation' and resourceProvider eq 'Microsoft.Subscription'"
    ```

Bu API'yi komut satırından rahatça çağırmak için [ARMClient](https://github.com/projectkudu/ARMClient)'ı deneyin.

## <a name="next-steps"></a>Sonraki adımlar

* Artık kullanıcı veya hizmet sorumlusu abonelik oluşturma izni verildiğine göre bu kimlik için kullanabileceğiniz [program aracılığıyla Azure Enterprise abonelikleri oluşturma](programmatically-create-subscription.md).
* .NET kullanarak abonelikleri oluşturma örneği için bkz. [örnek kodu github'da](https://github.com/Azure-Samples/create-azure-subscription-dotnet-core).
* Azure Resource Manager ve kendi API hakkında daha fazla bilgi için bkz: [Azure Resource Manager'a genel bakış](resource-group-overview.md).
* Çok sayıda Yönetim grupları kullanarak aboneliklerini yönetme hakkında daha fazla bilgi edinmek için [kaynaklarınızı Azure Yönetim grupları ile düzenleme](management-groups-overview.md)
* Büyük kuruluşlar için kapsamlı bir en iyi uygulama kılavuzunu abonelik İdaresi üzerinde görmek için bkz [Azure Kurumsal iskelesi: öngörücü abonelik İdaresi](/azure/architecture/cloud-adoption-guide/subscription-governance)
