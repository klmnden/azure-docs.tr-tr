---
title: 'Öğretici: Web Uygulamasından Azure Key Vault’u kullanma | Microsoft Docs'
description: Bu öğretici, web uygulamasından Azure Key Vault’u kullanmayı öğrenmenize yardımcı olacaktır.
services: key-vault
author: barclayn
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 9b7d065e-1979-4397-8298-eeba3aec4792
ms.service: key-vault
ms.workload: identity
ms.topic: tutorial
ms.date: 10/09/2018
ms.author: barclayn
ms.openlocfilehash: b66c9912ba0b6508c2beb786d2327efa779c6645
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "49079472"
---
# <a name="tutorial-use-azure-key-vault-from-a-web-application"></a>Öğretici: Web uygulamasından Azure Key Vault’u kullanma

Bu öğretici, Azure'daki bir web uygulamasından Azure Key Vault’u kullanmayı öğrenmenize yardımcı olacaktır. Web uygulamasında kullanmak üzere Azure Key Vault'taki bir gizli diziye erişme sürecini göstermektedir. Öğretici ayrıca bu süreci bir adım ileri götürerek gizli anahtar yerine sertifika kullanımını da gösterecektir. Bu öğretici, Azure'da web uygulaması oluşturma konusundaki temel kavramlara hakim olan web geliştiricileri için tasarlanmıştır.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz: 

> [!div class="checklist"]
> * Uygulama ayarlarını web.config dosyasına ekleme
> * Erişim belirteci almak için bir metot ekleme
> * Uygulama çalıştırıldığında belirteci alma
> * Sertifika kimlik doğrulama

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdaki öğelere sahip olmanız gerekir:

* Azure Key Vault içindeki bir gizli dizinin URI'si
* Azure Active Directory'ye kaydedilmiş ve anahtar kasanıza erişimi olan bir web uygulamasının İstemci Kimliği ve Gizli Anahtarı
* Bir web uygulaması. Bu öğreticide Azure'da bir Web Uygulaması olarak dağıtılmış olan ASP.NET MVC uygulamasıyla ilgili adımlar gösterilir.

Gizli dizi URI'sini, İstemci Kimliğini, Gizli Anahtarı almak ve uygulamayı kaydetmek için [Azure Key Vault'u kullanmaya başlama](key-vault-get-started.md) sayfasındaki adımları uygulayın. Web uygulaması kasaya erişecektir ve Azure Active Directory'de kayıtlı olması gerekir. Ayrıca anahtar kasasına erişim haklarına da sahip olmalıdır. Uygulama bu şartları karşılamıyorsa Kullanmaya Başlama öğreticisindeki Uygulama Kaydetme bölümüne dönün ve oradaki adımları uygulayın. Azure Web Apps uygulaması oluşturma hakkında daha fazla bilgi için bkz. [Web Apps'e genel bakış](../app-service/app-service-web-overview.md).

Bu örnek, Azure Active Directory kimliklerinin el ile sağlanmasına dayanmaktadır. Bunun yerine Azure AD kimliklerini otomatik olarak sağlayan [Azure kaynakları için yönetilen kimlikleri](../active-directory/managed-identities-azure-resources/overview.md) kullanmanız gerekir. Daha fazla bilgi için [GitHub](https://github.com/Azure-Samples/app-service-msi-keyvault-dotnet/) üzerindeki örneğe ve ilgili [App Service ve İşlevler öğreticisine](https://docs.microsoft.com/azure/app-service/app-service-managed-service-identity) bakın. Anahtar kasasına özgü [Anahtar Kasasından gizli dizi okumak için bir Azure web uygulaması yapılandırma öğreticisine](tutorial-web-application-keyvault.md) de bakabilirsiniz.

## <a id="packages"></a>NuGet paketlerini ekleme

Web uygulamanız için yüklemiş olmanız gereken iki paket vardır.

* Active Directory Authentication Library içinde Azure Active Directory ile etkileşim kurmak ve kullanıcı kimliğini yönetmek için kullanabileceğiniz metotlar vardır
* Azure Key Vault Library içinde Azure Key Vault ile etkileşim kurmak için kullanabileceğiniz metotlar vardır

Bu paketlerin ikisi de Paket Yöneticisi Konsolu'ndan Install-Package komutuyla yüklenebilir.

```powershell
Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory 
Install-Package Microsoft.Azure.KeyVault
```

## <a id="webconfig"></a>web.config dosyasını değiştirme

web.config dosyasına aşağıdaki şekilde eklenmesi gereken üç uygulama ayarı vardır. Ek bir güvenlik düzeyi sağlamak için gerçek değerleri Azure portalda ekleyeceğiz.

```xml
    <!-- ClientId and ClientSecret refer to the web application registration with Azure Active Directory -->
    <add key="ClientId" value="clientid" />
    <add key="ClientSecret" value="clientsecret" />

    <!-- SecretUri is the URI for the secret in Azure Key Vault -->
    <add key="SecretUri" value="secreturi" />
    <!-- If you aren't hosting your app as an Azure Web App, then you should use the actual ClientId, Client Secret, and Secret URI values -->
```



## <a id="gettoken"></a>Erişim belirteci almak için bir metot ekleme

Key Vault API'sini kullanmak için bir erişim belirtecine ihtiyacınız vardır. Key Vault İstemcisi, Key Vault API'sine gönderilen çağrıları işler ancak erişim belirtecini alan bir işlev kullanmanız gerekir. Aşağıdaki örnekte Azure Active Directory'den erişim belirteci almak için kullanılabilecek kod gösterilmiştir. Bu kod uygulamanızın herhangi bir bölümünde kullanılabilir. Ben Utils veya EncryptionHelper sınıfı eklemeyi tercih ediyorum.  

```cs
//add these using statements
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using System.Threading.Tasks;
using System.Web.Configuration;

//this is an optional property to hold the secret after it is retrieved
public static string EncryptSecret { get; set; }

//the method that will be provided to the KeyVaultClient
public static async Task<string> GetToken(string authority, string resource, string scope)
{
    var authContext = new AuthenticationContext(authority);
    ClientCredential clientCred = new ClientCredential(WebConfigurationManager.AppSettings["ClientId"],
                WebConfigurationManager.AppSettings["ClientSecret"]);
    AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

    if (result == null)
        throw new InvalidOperationException("Failed to obtain the JWT token");

    return result.AccessToken;
}
// Using Client ID and Client Secret is a way to authenticate an Azure AD application.
// Using it in your web application allows for a separation of duties and more control over your key management. 
// However, it does rely on putting the Client Secret in your configuration settings.
// For some people, this can be as risky as putting the secret in your configuration settings.
```

## <a id="appstart"></a>Uygulama çalıştırıldığında gizli diziyi alma

Şimdi Key Vault API'sini çağırmak ve gizli diziyi almak için kullanacağımız koda ihtiyacımız var. Aşağıdaki kodu kullanmadan önce çağırdığınız sürece herhangi bir yere yerleştirebilirsiniz. Bu kodu Global.asax içindeki Application Start olayının içine yerleştirdim, bu sayede başlangıçta bir kez çalışarak gizli diziyi uygulama için kullanılabilir hale getiriyor.

```cs
//add these using statements
using Microsoft.Azure.KeyVault;
using System.Web.Configuration;

// I put my GetToken method in a Utils class. Change for wherever you placed your method.
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetToken));
var sec = await kv.GetSecretAsync(WebConfigurationManager.AppSettings["SecretUri"]);

//I put a variable in a Utils class to hold the secret for general application use.
Utils.EncryptSecret = sec.Value;
```

## <a id="portalsettings"></a>Azure portalda uygulama ayarlarını ekleme

Artık Azure Web App'te Azure portal'dan gerçek AppSettings değerlerini ekleyebilirsiniz. Bu adımı gerçekleştirdiğinizde gerçek değerler web.config dosyasında olmaz ancak ayrı erişim denetimi özelliklerine sahip olduğunuz Portal aracılığıyla koruma altına alınır. Bu değerler web.config dosyasına girdiğiniz değerlerle değiştirilir. Adların aynı olduğundan emin olun.

![Azure portalda görüntülenen Uygulama Ayarları][1]

## <a name="authenticate-with-a-certificate-instead-of-a-client-secret"></a>Gizli anahtar yerine bir sertifikayla kimlik doğrulaması gerçekleştirme

Bir Azure AD uygulamasının kimliğini İstemci Kimliği ve Gizli Anahtar kullanarak doğrulamayı kavradınız, şimdi İstemci Kimliği ve bir sertifika kullanalım. Azure Web App'te sertifika kullanmak için aşağıdaki adımları uygulayın:

1. Sertifika alma veya oluşturma
2. Sertifikayı bir Azure AD uygulaması ile ilişkilendirme
3. Sertifikayı kullanmak için web uygulamanıza kod ekleme
4. Web uygulamanıza sertifika ekleme

### <a name="get-or-create-a-certificate"></a>Sertifika alma veya oluşturma

 Bu öğretici için bir test sertifikası oluşturacağız. Otomatik olarak imzalanan bir sertifika oluşturmak için bu betiği kullanabilirsiniz. Dizini sertifika dosyalarınızın oluşturulmasını istediğiniz yerle değiştirin.  Sertifikanın başlangıç ve bitiş tarihi olarak geçerli tarihe bir yıl ekleyebilirsiniz.

```powershell
#Create self-signed certificate and export pfx and cer files 
$PfxFilePath = 'KVWebApp.pfx'
$CerFilePath = 'KVWebApp.cer'
$DNSName = 'MyComputer.Contoso.com'
$Password = 'MyPassword"'

$StoreLocation = 'CurrentUser' #be aware that LocalMachine requires elevated privileges
$CertBeginDate = Get-Date
$CertExpiryDate = $CertBeginDate.AddYears(1)

$SecStringPw = ConvertTo-SecureString -String $Password -Force -AsPlainText 
$Cert = New-SelfSignedCertificate -DnsName $DNSName -CertStoreLocation "cert:\$StoreLocation\My" -NotBefore $CertBeginDate -NotAfter $CertExpiryDate -KeySpec Signature
Export-PfxCertificate -cert $Cert -FilePath $PFXFilePath -Password $SecStringPw 
Export-Certificate -cert $Cert -FilePath $CerFilePath 
```

Bitiş tarihini ve .pfx dosyasının parolasını not edin (bu örnekte: 15 Mayıs 2019 ve MyPassword). Aşağıdaki betik için bu değerlere ihtiyacınız olacak. 
### <a name="associate-the-certificate-with-an-azure-ad-application"></a>Sertifikayı bir Azure AD uygulaması ile ilişkilendirme

Artık bir sertifikanız olduğuna göre bunu bir Azure AD uygulamasıyla ilişkilendirebilirsiniz. İlişkilendirme işlemi PowerShell üzerinden tamamlanabilir. Sertifikayı Azure AD uygulamasıyla ilişkilendirmek için aşağıdaki komutları çalıştırın:

```powershell
$x509 = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2
$x509.Import("C:\data\KVWebApp.cer")
$credValue = [System.Convert]::ToBase64String($x509.GetRawCertData())


$adapp = New-AzureRmADApplication -DisplayName "KVWebApp" -HomePage "http://kvwebapp" -IdentifierUris "http://kvwebapp" -CertValue $credValue -StartDate $x509.NotBefore -EndDate $x509.NotAfter


$sp = New-AzureRmADServicePrincipal -ApplicationId $adapp.ApplicationId


Set-AzureRmKeyVaultAccessPolicy -VaultName 'contosokv' -ServicePrincipalName "http://kvwebapp" -PermissionsToSecrets get,list,set,delete,backup,restore,recover,purge -ResourceGroupName 'contosorg'

# get the thumbprint to use in your app settings
$x509.Thumbprint
```

Bu komutları çalıştırdıktan sonra uygulamayı Azure AD'de görebilirsiniz. Uygulama kayıtlarında arama yaparken, arama iletişim kutusunda "All apps" (Tüm uygulamalar) yerine **My apps** (Uygulamalarım) öğesini seçtiğinizden emin olun. 

### <a name="add-code-to-your-web-app-to-use-the-certificate"></a>Sertifikayı kullanmak için web uygulamanıza kod ekleme

Şimdi Web Uygulamanıza sertifikaya erişmesi ve onu kullanarak kimlik doğrulaması yapması için gerekli kodları ekleyeceğiz. 

İlk olarak, sertifika erişmek için gerekli koda bakalım. StoreLocation değerinin LocalMachine değil CurrentUser olduğuna dikkat edin. Test sertifikası kullandığımızdan Find metoduna 'false' değeri veriyoruz.

```cs
//Add this using statement
using System.Security.Cryptography.X509Certificates;  

public static class CertificateHelper
{
    public static X509Certificate2 FindCertificateByThumbprint(string findValue)
    {
        X509Store store = new X509Store(StoreName.My, StoreLocation.CurrentUser);
        try
        {
            store.Open(OpenFlags.ReadOnly);
            X509Certificate2Collection col = store.Certificates.Find(X509FindType.FindByThumbprint,
                findValue, false); // Don't validate certs, since the test root isn't installed.
            if (col == null || col.Count == 0)
                return null;
            return col[0];
        }
        finally
        {
            store.Close();
        }
    }
}
```



Bir sonraki kod CertificateHelper metodunu kullanarak kimlik doğrulaması için gerekli ClientAssertionCertificate metodunu oluşturur.

```cs
public static ClientAssertionCertificate AssertionCert { get; set; }

public static void GetCert()
{
    var clientAssertionCertPfx = CertificateHelper.FindCertificateByThumbprint(WebConfigurationManager.AppSettings["thumbprint"]);
    AssertionCert = new ClientAssertionCertificate(WebConfigurationManager.AppSettings["clientid"], clientAssertionCertPfx);
}
```

Erişim belirtecini almak için kullanmanız gereken yeni kod budur. Bu kod, bir önceki örnekteki GetToken metodunun yerini alır. Kolaylık sağlamak için farklı bir ad verdim. Kullanım kolaylığı için bu kodun tamamını Web Uygulaması projemin Utils sınıfına ekledim.

```cs
public static async Task<string> GetAccessToken(string authority, string resource, string scope)
{
    var context = new AuthenticationContext(authority, TokenCache.DefaultShared);
    var result = await context.AcquireTokenAsync(resource, AssertionCert);
    return result.AccessToken;
}
```



Son kod değişikliği, Application_Start metodundadır. ClientAssertionCertificate metodunu çağırmak için öncelikle GetCert() metodunu çağırmamız gerekir. Ardından yeni bir KeyVaultClient oluştururken sağladığımız geri çağırma metodunu değiştireceğiz. Bu kod, bir önceki örnekteki kodun yerini alır.

```cs
Utils.GetCert();
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetAccessToken));
```

### <a name="add-a-certificate-to-your-web-app-through-the-azure-portal"></a>Azure portal aracılığıyla web uygulamanıza sertifika ekleme

Web Uygulamanıza Sertifika ekleme, iki adımlı basit bir süreçtir. İlk olarak Azure portala gidip Web Uygulamanızı bulun. Web Uygulamanızın Ayarlar sayfasında **SSL ayarları** girişine tıklayın. Sayfa açıldığında önceki örnekte oluşturduğunuz KVWebApp.pfx Sertifikasını yükleyin. pfx dosyasının parolasını hatırladığınızdan emin olun.

![Azure portalda bir Web Uygulamasına Sertifika ekleme][2]

Yapmanız gereken son şey, Web Uygulamanıza WEBSITE\_LOAD\_CERTIFICATES adına ve * değerine sahip bir Uygulama Ayarı eklemektir. Bu adım, tüm Sertifikaların yüklenmesini sağlayacaktır. Yalnızca yüklediğiniz Sertifikaların yüklenmesini isterseniz parmak izlerini virgülle ayırarak girebilirsiniz.


## <a name="clean-up-resources"></a>Kaynakları temizleme
İhtiyacınız kalmadığında öğretici için kullandığınız uygulama hizmetini, anahtar kasasını ve Azure AD uygulamasını silin.  


## <a id="next"></a>Sonraki adımlar
> [!div class="nextstepaction"]
>[Azure Key Vault Yönetim API'si Başvurusu](/dotnet/api/overview/azure/keyvault/management).


<!--Image references-->
[1]: ./media/key-vault-use-from-web-application/PortalAppSettings.png
[2]: ./media/key-vault-use-from-web-application/PortalAddCertificate.png
 