---
author: robinsh
ms.author: robinsh
ms.service: iot-hub
ms.topic: include
ms.date: 10/26/2018
ms.openlocfilehash: 74f15452c7def068403b97b11e784170835edc18
ms.sourcegitcommit: 6e32f493eb32f93f71d425497752e84763070fad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2019
ms.locfileid: "59472301"
---
## <a name="prepare-to-authenticate-azure-resource-manager-requests"></a>Azure Resource Manager isteklerinin kimliğini doğrulamak hazırlama
Kaynakları kullanarak gerçekleştirdiğiniz tüm işlemleri kimlik doğrulaması yapması gereken [Azure Resource Manager] [ lnk-authenticate-arm] ile Azure Active Directory (AD). Bunu yapılandırmak için en kolay yolu, PowerShell veya Azure CLI kullanmaktır.

Yükleme [Azure PowerShell cmdlet'lerini] [ lnk-powershell-install] devam etmeden önce.

Aşağıdaki adımlar PowerShell kullanarak bir AD uygulaması için parola kimlik doğrulaması ayarlama işlemini göstermektedir. Standart bir PowerShell oturumunda aşağıdaki komutları çalıştırabilirsiniz.

1. Aşağıdaki komutu kullanarak Azure aboneliğinizde oturum açın:

    ```powershell
    Connect-AzAccount
    ```

1. Birden çok Azure aboneliğiniz varsa Azure'da oturum açma, kimlik bilgilerinizle ilişkili tüm Azure abonelikleri erişim verir. Azure aboneliklerini kullanmak için size sunulan listelemek için aşağıdaki komutu kullanın:

    ```powershell
    Get-AzSubscription
    ```

    IOT hub'ınıza yönetmek için komutları çalıştırmak için kullanmak istediğiniz aboneliği seçmek için aşağıdaki komutu kullanın. Önceki komutun çıkışında yer alan abonelik adını veya kimliği kullanabilirsiniz:

    ```powershell
    Select-AzSubscription `
        -SubscriptionName "{your subscription name}"
    ```

2. Not, **Tenantıd** ve **Subscriptionıd**. Bunlara daha sonra ihtiyacınız olacak.
3. Yer tutucu değiştirerek aşağıdaki komutu kullanarak yeni bir Azure Active Directory uygulaması oluşturun:
   
   * **{Görünen adı}:** gibi uygulamanız için bir görünen ad **MySampleApp**
   * **{Giriş sayfası URL'si}:** gibi uygulamanızın giriş sayfasının URL'sini **http:\/mysampleapp/giriş**. Bu URL için gerçek bir uygulamada noktası gerekmez.
   * **{Uygulama} tanımlayıcısı:** Benzersiz bir tanımlayıcı gibi **http:\//mysampleapp**. Bu URL için gerçek bir uygulamada noktası gerekmez.
   * **{Password}:** Uygulamanız ile kimlik doğrulaması için kullandığınız parola.
     
     ```powershell
     $SecurePassword=ConvertTo-SecureString {password} –asplaintext –force
     New-AzADApplication -DisplayName {Display name} -HomePage {Home page URL} -IdentifierUris {Application identifier} -Password $SecurePassword
     ```
4. Not **ApplicationId** oluşturduğunuz uygulamanın. Buna daha sonra ihtiyacınız olur.
5. Aşağıdaki komutu kullanarak, değiştirerek yeni bir hizmet sorumlusu oluşturma **{MyApplicationId}** ile **ApplicationId** önceki adımdan:
   
    ```powershell
    New-AzADServicePrincipal -ApplicationId {MyApplicationId}
    ```
6. Aşağıdaki komutu kullanarak, değiştirerek bir rol ataması kümesi **{MyApplicationId}** ile **ApplicationId**.
   
    ```powershell
    New-AzRoleAssignment -RoleDefinitionName Owner -ServicePrincipalName {MyApplicationId}
    ```

Artık, özel kimlik doğrulaması olanak tanıyan Azure AD uygulaması oluşturmayı tamamladıktan C# uygulama. Aşağıdaki değerlerden Bu öğreticide daha sonra ihtiyacınız vardır:

* TenantId
* SubscriptionId
* ApplicationId
* Parola

[lnk-authenticate-arm]: https://msdn.microsoft.com/library/azure/dn790557.aspx
[lnk-powershell-install]: /powershell/azure/install-Az-ps
