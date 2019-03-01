---
author: robinsh
ms.author: robin.shahan
ms.service: iot-hub
ms.topic: include
ms.date: 10/26/2018
ms.openlocfilehash: e2d6b6413d1e397eaa3c53f28394dcf321dac729
ms.sourcegitcommit: 15e9613e9e32288e174241efdb365fa0b12ec2ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2019
ms.locfileid: "57011785"
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