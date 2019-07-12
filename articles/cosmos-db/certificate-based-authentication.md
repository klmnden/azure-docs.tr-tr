---
title: Azure Active Directory Sertifika tabanlı kimlik doğrulaması ile Azure Cosmos DB
description: Sertifika tabanlı kimlik doğrulaması için bir Azure AD kimlik anahtarlara erişmek için Azure Cosmos DB'den yapılandırmayı öğrenin.
author: voellm
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/11/2019
ms.author: tvoellm
ms.reviewer: sngun
ms.openlocfilehash: cc39cc09259c1ae681e1fee070777575e2788323
ms.sourcegitcommit: 441e59b8657a1eb1538c848b9b78c2e9e1b6cfd5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67827840"
---
# <a name="certificate-based-authentication-for-an-azure-ad-identity-to-access-keys-from-an-azure-cosmos-db-account"></a>Azure Cosmos DB hesabı için erişim anahtarlarını bir Azure AD kimlik için sertifika tabanlı kimlik doğrulaması

Sertifika tabanlı kimlik doğrulaması, bir istemci sertifikası ile istemci uygulamanızı Azure Active Directory (Azure AD) kullanarak kimlik doğrulaması sağlar. Şirket içi makine veya sanal makine azure'da gibi bir kimlik gerek duyduğunuz bir makinedeki sertifika tabanlı kimlik doğrulaması gerçekleştirebilirsiniz. Uygulamanızı Azure Cosmos DB anahtarları okuyabilir ve anahtarları doğrudan uygulamasında zorunda kalmadan. Bu makalede nasıl bir örnek Azure AD uygulaması oluşturun, sertifika tabanlı kimlik doğrulaması için yapılandırmak, yeni uygulama kimliğini kullanarak Azure'da oturum açın ve ardından Azure Cosmos hesabınızdan anahtarları alır. Bu makalede kimliklerini için Azure PowerShell kullanır ve bir C# kimliğini doğrular ve Azure Cosmos hesabınızdan anahtarları erişen örnek uygulaması.  

## <a name="prerequisites"></a>Önkoşullar

* Yükleme [en son sürümü](/powershell/azure/install-az-ps) Azure PowerShell'in.

* Yoksa bir [Azure aboneliği](https://docs.microsoft.com/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing), oluşturun bir [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) başlamadan önce.

## <a name="register-an-app-in-azure-ad"></a>Bir uygulamayı Azure AD'ye kaydetme

Bu adımda, örnek web uygulamasını Azure AD'ye hesabınızdaki kaydeder. Bu uygulama, daha sonra Azure Cosmos DB hesabınızdan anahtarları okumak için kullanılır. Bir uygulamayı kaydetmek için aşağıdaki adımları kullanın: 

1. [Azure portal](https://portal.azure.com/) oturum açın.

1. Azure'ı açın **Active Directory** bölmesinde, uygulama kayıtları bölmesinde ve seçin için go **yeni kayıt**. 

   ![Active Directory'de yeni uygulama kaydı](./media/certificate-based-authentication/new-app-registration.png)

1. Dolgu **bir uygulamayı kaydetme** form aşağıdaki Ayrıntılar ile:  

   * **Adı** – uygulamanız için bir ad belirtin, "örnek uygulaması" gibi herhangi bir ad olabilir.
   * **Hesap türleri için desteklenen** – Seç **Bu dizindeki kuruluş yalnızca (varsayılan dizin) hesapları** kaynakları bu uygulamaya erişmek için geçerli dizinde izin vermek için. 
   * **Yeniden yönlendirme URL'si** – uygulama türü seçin **Web** ve uygulamanızın barındırıldığı bir URL sağlamanız, herhangi bir URL olabilir. Bu örnekte, bir test URL gibi sağlayabilirsiniz `https://sampleApp.com` uygulama mevcut olmasa bile uygundur.

   ![Örnek web uygulaması kaydetme](./media/certificate-based-authentication/register-sample-web-app.png)

1. Seçin **kaydetme** formu doldurduktan sonra.

1. Uygulama kaydedildikten sonra Not **Application(client) kimliği** ve **nesne kimliği**, sonraki adımlarda bu ayrıntıları kullanın. 

   ![Uygulama ve nesne kimlikleri alma](./media/certificate-based-authentication/get-app-object-ids.png)

## <a name="install-the-azuread-module"></a>AzureAD modülüne yükleyin

Bu adımda, Azure AD PowerShell modülünü yükler. Bu modül, önceki adımda kaydettiğiniz uygulama Kimliğini alın ve bu uygulama için otomatik olarak imzalanan bir sertifika ilişkilendirmek için gereklidir. 

1. Yönetici haklarıyla Windows PowerShell ISE'yi açın. Bunu zaten yapmadıysanız, AZ PowerShell modülünü yüklemek ve aboneliğinize bağlanın. Birden fazla aboneliğiniz varsa, geçerli abonelik bağlamında aşağıdaki komutlarda gösterildiği gibi ayarlayabilirsiniz:

   ```powershell

   Install-Module -Name Az -AllowClobber
   Connect-AzAccount

   Get-AzSubscription
   $context = Get-AzSubscription -SubscriptionId <Your_Subscription_ID>
   Set-AzContext $context 
   ```

1. Yükleme ve içeri aktarma [AzureAD](/powershell/module/azuread/?view=azureadps-2.0) Modülü

   ```powershell
   Install-Module AzureAD
   Import-Module AzureAD 
   ```

## <a name="sign-into-your-azure-ad"></a>Azure AD oturum

Uygulamayı nerede kaydolduğundan, Azure AD ile oturum açın. Connect-AzureAD komutunu hesabınızda oturum açmak için açılır pencerede Azure hesabı kimlik bilgilerinizi girin. 

```powershell
Connect-AzureAD 
```

## <a name="create-a-self-signed-certificate"></a>Otomatik olarak imzalanan sertifika oluşturma

Başka bir Windows PowerShell ISE örneği açın ve otomatik olarak imzalanan bir sertifika oluşturun ve sertifikayla ilişkili anahtar okumak için aşağıdaki komutları çalıştırın:

```powershell
$cert = New-SelfSignedCertificate -CertStoreLocation "Cert:\CurrentUser\My" -Subject "CN=sampleAppCert" -KeySpec KeyExchange
$keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData()) 
```

## <a name="create-the-certificate-based-credential"></a>Sertifika tabanlı kimlik bilgileri oluşturma 

Ardından, uygulamanın nesne Kimliğini alın ve sertifika tabanlı kimlik bilgisi oluşturmak için aşağıdaki komutları çalıştırın. Bu örnekte, sertifika bir yıl sonra süresi dolacak şekilde ayarladık, tüm gerekli bitiş tarihini ayarlayın.

```powershell
$application = Get-AzureADApplication -ObjectId <Object_ID_of_Your_Application>

New-AzureADApplicationKeyCredential -ObjectId $application.ObjectId -CustomKeyIdentifier "Key1" -Type AsymmetricX509Cert -Usage Verify -Value $keyValue -EndDate "2020-01-01"
```

Yukarıdaki komutu aşağıdaki ekran görüntüsüne benzer bir çıktı sonuçlanır:

![Sertifika tabanlı kimlik bilgisi oluşturma çıkış](./media/certificate-based-authentication/certificate-based-credential-output.png)

## <a name="configure-your-azure-cosmos-account-to-use-the-new-identity"></a>Azure Cosmos hesabınıza yeni bir kimlik kullanacak şekilde yapılandırma

1. [Azure portal](https://portal.azure.com/) oturum açın.

1. Azure Cosmos hesabınıza, açık gidin **erişim denetimi (IAM)** dikey penceresi.

1. Seçin **Ekle** ve **rol ataması Ekle**. Örnek uygulaması önceki adımda oluşturduğunuz ekleme **katkıda bulunan** aşağıdaki ekran görüntüsünde gösterildiği gibi rolü:

   ![Azure Cosmos hesabını yeni bir kimlik kullanacak şekilde yapılandırma](./media/certificate-based-authentication/configure-cosmos-account-with-identify.png)

1. Seçin **Kaydet** formu doldurduktan sonra


## <a name="access-the-keys-from-powershell"></a>Powershell'den erişim anahtarları

Bu adımda, uygulama ve oluşturduğunuz ve Azure Cosmos hesabınızın anahtarlarını erişim sertifikası kullanarak Azure'a imzalar. 

1. Başlangıçta hesabınızda oturum açmak için kullanılan Azure hesabının kimlik bilgilerini temizleyin. Aşağıdaki komutu kullanarak kimlik bilgilerini silebilirsiniz:

   ```powershell
   Disconnect-AzAccount -Username <Your_Azure_account_email_id> 
   ```

1. Ardından uygulamanın kimlik bilgilerini kullanarak Azure portalında oturum açın ve Azure Cosmos DB anahtarlara erişmek doğrulama:

   ```powershell
   Login-AzAccount -ApplicationId <Your_Application_ID> -CertificateThumbprint $cert.Thumbprint -ServicePrincipal -Tenant <Tenant_ID_of_your_application>

   Invoke-AzResourceAction -Action listKeys -ResourceType "Microsoft.DocumentDB/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName <Resource_Group_Name_of_your_Azure_Cosmos_account> -ResourceName <Your_Azure_Cosmos_Account_Name> 
   ```

Önceki komut, Azure Cosmos hesabınızın birincil ve ikincil ana anahtarları görüntülenir. Get anahtar istek başarılı oldu ve "örnek uygulaması" uygulama tarafından başlatılan olay doğrulamak için Azure Cosmos hesabınızın etkinlik günlüğü görüntüleyebilirsiniz. 
 
![Azure AD'de alma anahtarları çağrısı doğrula](./media/certificate-based-authentication/activity-log-validate-results.png)


## <a name="access-the-keys-from-a-c-application"></a>Erişim anahtarlarını bir C# uygulama 

Anahtarları erişerek bu senaryo doğrulayabilirsiniz bir C# uygulama. Aşağıdaki C# konsol Active Directory'de kayıtlı uygulamasını kullanarak Azure Cosmos DB anahtarları erişebileceği uygulaması. Tenantıd, ClientID, certName, kaynak grubu adı, abonelik kimliği, güncelleştirilecek emin olun kodu çalıştırmadan önce Azure Cosmos hesap adı ayrıntıları. 

```csharp
using System;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using System.Linq;
using System.Net.Http;
using System.Security.Cryptography.X509Certificates;
using System.Threading;
using System.Threading.Tasks;
 
namespace TodoListDaemonWithCert
{
    class Program
    {
        private static string aadInstance = "https://login.windows.net/";
        private static string tenantId = "<Your_Tenant_ID>";
        private static string clientId = "<Your_Client_ID>";
        private static string certName = "<Your_Certificate_Name>";
 
        private static int errorCode = 0;
        static int Main(string[] args)
        {
            MainAync().Wait();
            Console.ReadKey();
 
            return 0;
        } 
 
        static async Task MainAync()
        {
            string authContextURL = aadInstance + tenantId;
            AuthenticationContext authContext = new AuthenticationContext(authContextURL);
            X509Certificate2 cert = ReadCertificateFromStore(certName);
 
            ClientAssertionCertificate credential = new ClientAssertionCertificate(clientId, cert);
            AuthenticationResult result = await authContext.AcquireTokenAsync("https://management.azure.com/", credential);
            if (result == null)
            {
                throw new InvalidOperationException("Failed to obtain the JWT token");
            }
 
            string token = result.AccessToken;
            string subscriptionId = "<Your_Subscription_ID>";
            string rgName = "<ResourceGroup_of_your_Cosmos_account>";
            string accountName = "<Your_Cosmos_account_name>";
            string cosmosDBRestCall = $"https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{rgName}/providers/Microsoft.DocumentDB/databaseAccounts/{accountName}/listKeys?api-version=2015-04-08";
 
            Uri restCall = new Uri(cosmosDBRestCall);
            HttpClient httpClient = new HttpClient();
            httpClient.DefaultRequestHeaders.Remove("Authorization");
            httpClient.DefaultRequestHeaders.Add("Authorization", "Bearer " + token);
            HttpResponseMessage response = await httpClient.PostAsync(restCall, null);
 
            Console.WriteLine("Got result {0} and keys {1}", response.StatusCode.ToString(), response.Content.ReadAsStringAsync().Result);
        }
 
 
        /// <summary>
        /// Reads the certificate
        /// </summary>
        private static X509Certificate2 ReadCertificateFromStore(string certName)
        {
            X509Certificate2 cert = null;
            X509Store store = new X509Store(StoreName.My, StoreLocation.CurrentUser);
            store.Open(OpenFlags.ReadOnly);
            X509Certificate2Collection certCollection = store.Certificates;
 
            // Find unexpired certificates.
            X509Certificate2Collection currentCerts = certCollection.Find(X509FindType.FindByTimeValid, DateTime.Now, false);
 
            // From the collection of unexpired certificates, find the ones with the correct name.
            X509Certificate2Collection signingCert = currentCerts.Find(X509FindType.FindBySubjectName, certName, false);
 
            // Return the first certificate in the collection, has the right name and is current.
            cert = signingCert.OfType<X509Certificate2>().OrderByDescending(c => c.NotBefore).FirstOrDefault();
            store.Close();
            return cert;
        }
 
 
        /// <summary>
        /// Get an access token from Azure AD using client credentials.
        /// If the attempt to get a token fails because the server is unavailable, retry twice after 3 seconds each
        /// </summary>
        private static async Task<AuthenticationResult> GetAccessToken(AuthenticationContext authContext, string resourceUri, ClientAssertionCertificate cert)
        {
            //
            // Get an access token from Azure AD using client credentials.
            // If the attempt to get a token fails because the server is unavailable, retry twice after 3 seconds each.
            //
            AuthenticationResult result = null;
            int retryCount = 0;
            bool retry = false;
 
            do
            {
                retry = false;
                errorCode = 0;
 
                try
                {
                    result = await authContext.AcquireTokenAsync(resourceUri, cert);
                }
                catch (AdalException ex)
                {
                    if (ex.ErrorCode == "temporarily_unavailable")
                    {
                        retry = true;
                        retryCount++;
                        Thread.Sleep(3000);
                    }
 
                    Console.WriteLine(
                        String.Format("An error occurred while acquiring a token\nTime: {0}\nError: {1}\nRetry: {2}\n",
                        DateTime.Now.ToString(),
                        ex.ToString(),
                        retry.ToString()));
 
                    errorCode = -1;
                }
 
            } while ((retry == true) && (retryCount < 3));
            return result;
        }
    }
}
```

Bu betik, aşağıdaki ekran görüntüsünde gösterildiği gibi birincil ve ikincil ana anahtarları çıkarır:

![CSharp uygulama çıktısı](./media/certificate-based-authentication/csharp-application-output.png)

Benzer şekilde, önceki bölümde, Azure Cosmos hesabınızın get anahtarları isteği olayı "örnek uygulaması" uygulama tarafından başlatıldığını doğrulamak için etkinlik günlüğü görüntüleyebilirsiniz. 


## <a name="next-steps"></a>Sonraki adımlar

* [Azure Cosmos anahtarları Azure Key Vault'u kullanarak güvenli hale getirme](access-secrets-from-keyvault.md)

* [Azure Cosmos DB için güvenlik öznitelikleri](cosmos-db-security-attributes.md)
