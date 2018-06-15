---
title: Bir Web uygulamasından Azure anahtar kasası kullanan | Microsoft Docs
description: Bir web uygulamasından Azure anahtar kasası kullanmayı öğrenmenize yardımcı olmak için bu öğreticiyi kullanın.
services: key-vault
author: adhurwit
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 9b7d065e-1979-4397-8298-eeba3aec4792
ms.service: key-vault
ms.workload: identity
ms.topic: article
ms.date: 05/10/2018
ms.author: adhurwit
ms.openlocfilehash: 3a191c3ee7eea641aab81008a6da801b609fb4c5
ms.sourcegitcommit: b7290b2cede85db346bb88fe3a5b3b316620808d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34802111"
---
# <a name="use-azure-key-vault-from-a-web-application"></a>Bir Web uygulamasından Azure anahtar kasası kullanın

## <a name="introduction"></a>Giriş

Azure web uygulamasından Azure anahtar kasası kullanmayı öğrenmenize yardımcı olmak için bu öğreticiyi kullanın. Bu, böylece web uygulamanızda kullanılabilir gizli bir Azure anahtar Kasası'erişme işlemi açıklanmaktadır.

**Tahmini tamamlanma süresi:** 15 dakika

Azure Anahtar Kasası genel bakış bilgileri için bkz. [Azure Anahtar Kasası nedir?](key-vault-whatis.md)

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdakilere sahip olmanız gerekir:

* Azure anahtar Kasası'nda bir gizlilik için bir URI
* Bir istemci kimliği ve bir web uygulaması için bir gizli anahtar kasanızı erişimi Azure Active Directory'ye kayıtlı
* Bir web uygulamasıdır. Biz gösteren Azure Web uygulaması olarak dağıtılmış bir ASP.NET MVC uygulaması için adımları.

>[!IMPORTANT]
>* Bu örnek AAD kimlikleri el ile sağlama daha eski bir şekilde bağlıdır. Şu anda var. yeni bir özellik olarak adlandırılan önizlemede [yönetilen hizmet kimliği (MSI)](https://docs.microsoft.com/azure/active-directory/msi-overview), hangi otomatik olarak sağlayabilirler AAD kimlikleri. Lütfen üzerinde aşağıdaki örneğe bakın [GitHub](https://github.com/Azure-Samples/app-service-msi-keyvault-dotnet/) daha ayrıntılı bilgi için.

> [!NOTE]
>* Listelenen adımları tamamladınız gereklidir [Azure anahtar kasası ile çalışmaya başlama](key-vault-get-started.md) Bu öğretici için böylece bir web uygulaması için bir gizlilik ve istemci kimliği ve istemci parolası URI sahip.


Anahtar kasası erişecek web uygulamasını Azure Active Directory'de kayıtlı ve anahtar Kasası'na erişim verildi adrestir. Bu durumda değilse, bir kasaya Başlarken Öğreticisi uygulamada geri dönün ve listelenen adımları yineleyin.

Bu öğretici, Azure üzerinde web uygulamaları oluşturma temelleri anlamak web geliştiricileri için tasarlanmıştır. Azure Web Apps hakkında daha fazla bilgi için bkz. [Web Apps'e genel bakış](../app-service/app-service-web-overview.md).

## <a id="packages"></a>NuGet paketleri ekleme

Yüklü web uygulamanız gereken iki paket vardır.

* Active Directory Authentication Library - Azure Active Directory ile etkileşim ve kullanıcı kimliği yönetmek için yöntemler içerir
* Azure anahtar kasası Library - Azure anahtar kasası ile etkileşim için yöntemler içerir

Bu paketleri her ikisi de Install-Package komutunu kullanarak Paket Yöneticisi konsolu kullanılarak yüklenebilir.

```
// this is currently the latest stable version of ADAL
Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.16.204221202
Install-Package Microsoft.Azure.KeyVault
```

## <a id="webconfig"></a>Web.Config değiştirme

Web.config dosyasında aşağıdaki gibi eklenmesi gereken üç uygulama ayarlarını vardır.

```
    <!-- ClientId and ClientSecret refer to the web application registration with Azure Active Directory -->
    <add key="ClientId" value="clientid" />
    <add key="ClientSecret" value="clientsecret" />

    <!-- SecretUri is the URI for the secret in Azure Key Vault -->
    <add key="SecretUri" value="secreturi" />
```

Uygulamanızı Azure Web uygulaması olarak barındırmak için yapmayacağınız, web.config dosyasına gerçek ClientID, istemci gizli anahtarı ve gizli anahtar URI'sini değerleri eklemeniz gerekir. Aksi takdirde ek bir güvenlik düzeyi için Azure Portalı'nda gerçek değerler biz alacağınız çünkü bu kukla değerleri bırakın.

## <a id="gettoken"></a>Bir erişim belirteci almak için bir yöntem ekleyin

Anahtar kasası API kullanmak için bir erişim belirteci gerekir. Anahtar kasası istemci anahtar kasası API çağrılarını işler, ancak erişim belirtecini alır işleviyle sağlamanız gerekir.  

Azure Active Directory'den bir erişim belirteci alma kodu verilmiştir. Bu kodu, uygulamanızda herhangi bir yere gidebilirsiniz. Bir yardımcı programları veya EncryptionHelper sınıfı eklemek ister.  

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
```

> [!NOTE]
>* Şu anda yeni bir özellik olarak Yönetilen Hizmet Kimliği (MSI), kimlik doğrulaması için en kolay yöntemdir. Daha ayrıntılı bilgi için, [Bir .NET uygulamasında MSI ile Key Vault](https://github.com/Azure-Samples/app-service-msi-keyvault-dotnet/) ve ilgili [App Service ve İşlevler ile MSI öğreticisini](https://docs.microsoft.com/azure/app-service/app-service-managed-service-identity) kullanarak örneğin aşağıdaki bağlantısına bakın. 
>* İstemci kimliği ve istemci gizli anahtarını kullanarak Azure AD uygulaması kimliğini doğrulamak için başka bir yoludur. Ve web uygulamanızı kullanarak ayrılması görevlerini ve anahtar yönetimi üzerinde daha fazla denetim sağlar. Ancak, yapılandırma ayarlarınızda korumak istediğiniz gizli koyma olarak olarak riskli olabilecek bazı yapılandırma ayarlarınızın istemci gizli anahtarı koyma kullanır. Aşağıdaki bir tartışma için bir istemci kimliği ve sertifika istemci kimliği ve istemci parolası yerine Azure AD uygulaması kimliğini doğrulamak için nasıl kullanılacağı hakkında bakın.

## <a id="appstart"></a>Uygulama başlangıç gizli anahtarı alma

Şimdi anahtar kasası API çağrısı ve gizli anahtarı almak için kod gerekir. Aşağıdaki kod herhangi bir yere kullanmaya ihtiyacım önce çağırılır sürece koyabilirsiniz. Böylece gizli uygulama için kullanılabilir hale getirir ve Başlat'bir kez çalıştırılır t bu kodu Global.asax uygulama başlatma olayı koymak.

```cs
//add these using statements
using Microsoft.Azure.KeyVault;
using System.Web.Configuration;

// I put my GetToken method in a Utils class. Change for wherever you placed your method.
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetToken));
var sec = await kv.GetSecretAsync(WebConfigurationManager.AppSettings["SecretUri"]);

//I put a variable in a Utils class to hold the secret for general  application use.
Utils.EncryptSecret = sec.Value;
```

## <a id="portalsettings"></a>Uygulama ayarları (isteğe bağlı) Azure portalında ekleme

Bir Azure Web uygulaması varsa, Azure portalında AppSettings için gerçek değerler artık ekleyebilirsiniz. Bunu yaparak, gerçek değerler Web.Config'de olmaz ancak ayrı bir erişim denetimi özelliklerinden sahip olduğu Portal korumalı. Bu değerler, web.config dosyanızda girdiğiniz değerleri için değiştirilecektir. Adları aynı olduğundan emin olun.

![Azure Portalı'nda görüntülenen uygulama ayarları][1]

## <a name="authenticate-with-a-certificate-instead-of-a-client-secret"></a>Bir istemci parolası yerine bir sertifika ile kimlik doğrulaması

Azure AD uygulaması kimliğini doğrulamak için başka bir istemci kimliği ve istemci parolası yerine bir istemci kimliği ve bir sertifika kullanarak yoludur. Bir Azure Web uygulamasında bir sertifika kullanmak için adımları şunlardır:

1. Alma veya bir sertifika oluşturun
2. Sertifika bir Azure AD uygulama ile ilişkilendirme
3. Sertifikayı kullanmak üzere Web uygulamanıza kod ekleme
4. Web uygulamanız için bir sertifika Ekle

### <a name="get-or-create-a-certificate"></a>Alma veya bir sertifika oluşturun

Bizim amaçlar için bir test sertifikası yapacağız. Burada, birkaç Geliştirici Komut İstemi'nde bir sertifika oluşturmak için kullanabileceğiniz komutlar bulunmaktadır. Oluşturulan sertifika dosyaları istediğiniz dizini değiştirin.  Ayrıca, başlangıç ve bitiş tarihi sertifikanın için geçerli tarih artı 1 yıl kullanın.

```
makecert -sv mykey.pvk -n "cn=KVWebApp" KVWebApp.cer -b 07/31/2017 -e 07/31/2018 -r
pvk2pfx -pvk mykey.pvk -spc KVWebApp.cer -pfx KVWebApp.pfx -po test123
```

Bitiş tarihi ve .pfx için parolayı not edin (Bu örnekte: 07/31/2018 ve test123). Bunları aşağıdaki gerekir.

Bir test sertifikası oluşturma hakkında daha fazla bilgi için bkz: [nasıl yapılır: bilgisayarınızı kendi Test sertifikası oluşturma](https://msdn.microsoft.com/library/ff699202.aspx)

### <a name="associate-the-certificate-with-an-azure-ad-application"></a>Sertifika bir Azure AD uygulama ile ilişkilendirme

Bir sertifika sahip olduğunuza göre Azure AD uygulaması ile ilişkilendirmeniz gerekir. Şu anda Azure portalı, bu iş akışı desteklemiyor; Bu PowerShell aracılığıyla tamamlanabilir. Sertifika Azure AD uygulama ile ilişkilendirmek için aşağıdaki komutları çalıştırın:

```ps
$x509 = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2
$x509.Import("C:\data\KVWebApp.cer")
$credValue = [System.Convert]::ToBase64String($x509.GetRawCertData())


$adapp = New-AzureRmADApplication -DisplayName "KVWebApp" -HomePage "http://kvwebapp" -IdentifierUris "http://kvwebapp" -CertValue $credValue -StartDate $x509.NotBefore -EndDate $x509.NotAfter


$sp = New-AzureRmADServicePrincipal -ApplicationId $adapp.ApplicationId


Set-AzureRmKeyVaultAccessPolicy -VaultName 'contosokv' -ServicePrincipalName "http://kvwebapp" -PermissionsToSecrets all -ResourceGroupName 'contosorg'

# get the thumbprint to use in your app settings
$x509.Thumbprint
```

Bu komutları çalıştırdıktan sonra Azure AD'de uygulama görebilirsiniz. Arama yaparken, "Şirketimin sahip olduğu uygulamalar" "Uygulamaları Şirketim kullanıyor yerine" arama iletişim kutusunda seçtiğiniz emin olun.

Azure AD uygulama nesneleri ve ServicePrincipal nesneleri hakkında daha fazla bilgi için bkz: [uygulama ve hizmet sorumlusu nesneleri](../active-directory/active-directory-application-objects.md).

### <a name="add-code-to-your-web-app-to-use-the-certificate"></a>Sertifikayı kullanmak üzere Web uygulamanıza kod ekleme

Şimdi biz cert erişmek ve kimlik doğrulaması için kullanmak üzere Web uygulamanız için kod ekleyeceksiniz.

İlk cert erişmek için kod yoktur.

```cs
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

StoreLocation Currentuser'a LocalMachine yerine olduğuna dikkat edin. Ve bir test sertifikası kullanıyoruz çünkü biz bulma yöntemine ' false' sağlamış olursunuz olduğunu.

Sonraki CertificateHelper kullanır ve kimlik doğrulaması için gerekli olan bir ClientAssertionCertificate oluşturan kodudur.

```cs
public static ClientAssertionCertificate AssertionCert { get; set; }

public static void GetCert()
{
    var clientAssertionCertPfx = CertificateHelper.FindCertificateByThumbprint(WebConfigurationManager.AppSettings["thumbprint"]);
    AssertionCert = new ClientAssertionCertificate(WebConfigurationManager.AppSettings["clientid"], clientAssertionCertPfx);
}
```

Erişim belirteci almak için yeni kod aşağıdaki gibidir. GetToken yöntemi önceki örnekte değiştirir. I kolaylık sağlamak için farklı bir ad verilen.

```cs
public static async Task<string> GetAccessToken(string authority, string resource, string scope)
{
    var context = new AuthenticationContext(authority, TokenCache.DefaultShared);
    var result = await context.AcquireTokenAsync(resource, AssertionCert);
    return result.AccessToken;
}
```

I tüm bu kod, kullanım kolaylığı için my Web uygulaması projenin yardımcı programları sınıfı yerleştirin.

En son kod değişikliği Application_Start yöntemidir. İlk olarak kimliğinizi ClientAssertionCertificate yüklemek için GetCert() yöntemini çağırmak gerekiyor. Ve biz Biz yeni KeyVaultClient oluştururken sağladığınız geri çağırma yöntemi değiştirebilirsiniz. Bu sizi bir önceki örnekte vardı kod değiştirir unutmayın.

```cs
Utils.GetCert();
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetAccessToken));
```

### <a name="add-a-certificate-to-your-web-app-through-the-azure-portal"></a>Azure Portalı aracılığıyla Web uygulamanız için bir sertifika Ekle

Web uygulamanız için bir sertifika ekleme basit iki adımlı bir işlemdir. İlk olarak, Azure Portalı'na gidin ve Web uygulamanıza gidin. "Özel etki alanları ve SSL" girişini ayarları dikey penceresinde, Web uygulamanız için tıklatın. Önceki örnekte, KVWebApp.pfx, oluşturduğunuz sertifika karşıya kuramaz açılır olduğundan emin olun, dikey penceresinde pfx parolasını unutmayın.

![Azure portalında bir Web uygulaması için bir sertifika ekleme][2]

Bir uygulama ayarı adı Web sitesi sahip, Web uygulamanızın eklemek için yapmanız gereken son şey.\_yük\_SERTİFİKALARI ve değerini *. Bu, tüm sertifikaların yüklü olduğundan güvence altına alır. Karşıya yüklediğiniz sertifikalar yüklemek istiyorsanız, virgülle ayrılmış bir liste kendi parmak izleri girebilirsiniz.

Bir Web uygulaması için bir sertifika ekleme hakkında daha fazla bilgi edinmek için [kullanarak sertifikaları Azure Web siteleri uygulamalarında](https://azure.microsoft.com/blog/2014/10/27/using-certificates-in-azure-websites-applications/)

### <a name="add-a-certificate-to-key-vault-as-a-secret"></a>Bir sertifika anahtar kasası için gizlilik olarak ekleyin.

Sertifikanızı Web App Service'e doğrudan yüklemek yerine, anahtar kasası gizli olarak depolayın ve buradan dağıtın. Bu aşağıdaki blog postasına özetlenen iki adımlı bir işlemdir [anahtar kasası aracılığıyla Azure Web uygulaması sertifikası dağıtma](https://blogs.msdn.microsoft.com/appserviceteam/2016/05/24/deploying-azure-web-app-certificate-through-key-vault/)

## <a id="next"></a>Sonraki adımlar

Programlama başvuruları için bkz: [Azure anahtar kasası C# istemci API Başvurusu](https://msdn.microsoft.com/en-us/library/azure/mt430941.aspx).

<!--Image references-->
[1]: ./media/key-vault-use-from-web-application/PortalAppSettings.png
[2]: ./media/key-vault-use-from-web-application/PortalAddCertificate.png
