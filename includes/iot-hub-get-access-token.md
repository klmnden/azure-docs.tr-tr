---
author: robinsh
ms.author: robinsh
ms.service: iot-hub
ms.topic: include
ms.date: 10/26/2018
ms.openlocfilehash: b6ea8c7b3a6374572c8bd31e3c62b788efbafcbc
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67189025"
---
## <a name="obtain-an-azure-resource-manager-token"></a>Bir Azure Resource Manager belirteç edinme
Azure Active Directory, Azure Resource Manager kullanarak kaynakları üzerinde gerçekleştirdiğiniz tüm görevleri doğrulaması gerekir. Diğer yaklaşımlar görmek için parola kimlik doğrulaması, burada gösterilen örnek kullanır [Azure Resource Manager kimlik doğrulama istekleri][lnk-authenticate-arm].

1. Aşağıdaki kodu ekleyin **ana** program.CS'de Webhostbuilder'a uygulama kimliği ve parola kullanarak Azure AD'den bir belirteç almak için yöntemi.
   
    ```
    var authContext = new AuthenticationContext(string.Format  
      ("https://login.microsoftonline.com/{0}", tenantId));
    var credential = new ClientCredential(applicationId, password);
    AuthenticationResult token = authContext.AcquireTokenAsync
      ("https://management.core.windows.net/", credential).Result;
   
    if (token == null)
    {
      Console.WriteLine("Failed to obtain the token");
      return;
    }
    ```
2. Oluşturma bir **ResourceManagementClient** sonuna aşağıdaki kodu ekleyerek belirteci kullanan nesne **ana** yöntemi:
   
    ```
    var creds = new TokenCredentials(token.AccessToken);
    var client = new ResourceManagementClient(creds);
    client.SubscriptionId = subscriptionId;
    ```
3. Oluşturma veya kullanmakta olduğunuz kaynak grubu için bir başvuru alın:
   
    ```
    var rgResponse = client.ResourceGroups.CreateOrUpdate(rgName,
        new ResourceGroup("East US"));
    if (rgResponse.Properties.ProvisioningState != "Succeeded")
    {
      Console.WriteLine("Problem creating resource group");
      return;
    }
    ```

[lnk-authenticate-arm]: https://msdn.microsoft.com/library/azure/dn790557.aspx