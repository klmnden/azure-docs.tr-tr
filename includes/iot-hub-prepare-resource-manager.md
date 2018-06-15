## <a name="prepare-to-authenticate-azure-resource-manager-requests"></a>Azure Resource Manager isteklerinin kimlik doğrulaması hazırlama
Kaynakları kullanarak gerçekleştirdiğiniz tüm işlemleri kimlik doğrulaması yapmalıdır [Azure Resource Manager] [ lnk-authenticate-arm] ile Azure Active Directory (AD). Bunu yapılandırmak için en kolay yolu, PowerShell veya Azure CLI kullanmaktır.

Yükleme [Azure PowerShell cmdlet'lerini] [ lnk-powershell-install] devam etmeden önce.

Aşağıdaki adımlar PowerShell kullanarak bir AD uygulaması için parola kimlik doğrulaması kurma gösterir. Standart bir PowerShell oturumunda aşağıdaki komutları çalıştırabilirsiniz.

1. Aşağıdaki komutu kullanarak Azure aboneliğinizde oturum açın:

    ```powershell
    Connect-AzureRmAccount
    ```

1. Birden çok Azure aboneliğiniz varsa, Azure'da oturum açma kimlik bilgileriyle ilişkili tüm Azure abonelikleri için size erişim verir. Azure aboneliklerini kullanabilmeniz için kullanılabilir listelemek için aşağıdaki komutu kullanın:

    ```powershell
    Get-AzureRMSubscription
    ```

    IOT hub'ınızı yönetmek için komutları çalıştırmak için kullanmak istediğiniz aboneliği seçmek için aşağıdaki komutu kullanın. Önceki komutun çıkışında yer alan abonelik adını veya kimliği kullanabilirsiniz:

    ```powershell
    Select-AzureRMSubscription `
        -SubscriptionName "{your subscription name}"
    ```

2. Not, **Tenantıd** ve **Subscriptionıd**. Daha sonra gereksinim duyarsınız.
3. Yer tutucu değiştirerek aşağıdaki komutu kullanarak yeni bir Azure Active Directory uygulaması oluşturun:
   
   * **{Görünen adı}:** gibi uygulamanız için bir görünen ad **MySampleApp**
   * **{Giriş sayfası URL'si}:** gibi uygulamanızın giriş sayfasının URL'sini **http://mysampleapp/home**. Bu URL için gerçek bir uygulamada noktası gerekmez.
   * **{Uygulama tanımlayıcısı}:** gibi benzersiz bir tanımlayıcı **http://mysampleapp**. Bu URL için gerçek bir uygulamada noktası gerekmez.
   * **{Parola}:** uygulamanız ile kimlik doğrulaması için kullandığınız parola.
     
     ```powershell
     $SecurePassword=ConvertTo-SecureString {password} –asplaintext –force
     New-AzureRmADApplication -DisplayName {Display name} -HomePage {Home page URL} -IdentifierUris {Application identifier} -Password $SecurePassword
     ```
4. Not **ApplicationId** , oluşturduğunuz uygulamanın. Bu daha sonra ihtiyacınız var.
5. Aşağıdaki komutu kullanarak, değiştirerek yeni bir hizmet sorumlusu oluşturmak **{MyApplicationId}** ile **ApplicationId** önceki adımdan:
   
    ```powershell
    New-AzureRmADServicePrincipal -ApplicationId {MyApplicationId}
    ```
6. Aşağıdaki komutu kullanarak, değiştirerek bir rol ataması ayarlamak **{MyApplicationId}** ile **ApplicationId**.
   
    ```powershell
    New-AzureRmRoleAssignment -RoleDefinitionName Owner -ServicePrincipalName {MyApplicationId}
    ```

Şimdi, özel C# uygulamanızın kimlik doğrulaması sağlayan Azure AD uygulaması oluşturma tamamladınız. Bu öğreticinin ilerleyen bölümlerinde aşağıdaki değerleri gerekir:

* Tenantıd
* SubscriptionId
* ApplicationId
* Parola

[lnk-authenticate-arm]: https://msdn.microsoft.com/library/azure/dn790557.aspx
[lnk-powershell-install]: https://docs.microsoft.com/powershell/azure/install-azurerm-ps
