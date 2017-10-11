## <a name="obtain-an-azure-resource-manager-token"></a>Bir Azure Resource Manager belirteç edinme
Azure Active Directory kaynakları Azure Kaynak Yöneticisi'ni kullanarak gerçekleştirdiğiniz tüm görevler kimliğini doğrulaması gerekir. Burada gösterilen örnekte, parola kimlik doğrulaması kullanır, diğer yaklaşım için bkz [Azure Resource Manager kimlik doğrulama istekleri][lnk-authenticate-arm].

1. Aşağıdaki kodu ekleyin **ana** uygulama kimliği ve parola kullanarak Azure AD'den bir belirteç almak için Program.cs yöntemi.
   
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
2. Oluşturma bir **ResourceManagementClient** sonuna aşağıdaki kodu ekleyerek belirteç kullanan nesne **ana** yöntemi:
   
    ```
    var creds = new TokenCredentials(token.AccessToken);
    var client = new ResourceManagementClient(creds);
    client.SubscriptionId = subscriptionId;
    ```
3. Oluşturma veya bir başvuru için kullanmakta olduğunuz kaynak grubu elde:
   
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