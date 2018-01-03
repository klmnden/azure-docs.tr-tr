---
title: ".NET kullanarak Azure anahtar kasası için hizmetten hizmete kimlik doğrulaması"
description: ".NET kullanarak Azure anahtar kasası için kimlik doğrulaması için Microsoft.Azure.Services.AppAuthentication kitaplığını kullanın."
keywords: "Azure anahtar kasası kimlik doğrulaması yerel kimlik bilgileri"
author: lleonard-msft
manager: mbaldwin
services: key-vault
ms.author: alleonar
ms.date: 11/15/2017
ms.topic: article
ms.prod: 
ms.service: microsoft-keyvault
ms.technology: 
ms.assetid: 4be434c4-0c99-4800-b775-c9713c973ee9
ms.openlocfilehash: f67f81aeee0775ea8d90e4459f2c46266a774786
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/16/2017
---
# <a name="service-to-service-authentication-to-azure-key-vault-using-net"></a>.NET kullanarak Azure anahtar kasası için hizmetten hizmete kimlik doğrulaması

Azure anahtar Kasası'na kimliğini doğrulamak için bir Azure Active Directory (AD) kimlik bilgileri, bir paylaşılan gizlilik veya bir sertifika gerekir. Bu tür kimlik bilgilerini yönetmek zor olabilir ve kaynak veya yapılandırma dosyalarında dahil ederek kimlik bilgileri uygulamaya paketini tempting.

`Microsoft.Azure.Services.AppAuthentication` İçin bu sorun .NET kitaplığı basitleştirir. Yerel geliştirme sırasında kimlik doğrulaması yapmak için Geliştirici kimlik bilgilerini kullanır. Çözümü daha sonra Azure'a dağıtıldığında, kitaplık uygulaması kimlik bilgileri otomatik olarak geçer.  

Azure AD kimlik oluşturmak veya kimlik bilgilerini geliştiriciler arasında paylaşmak gerekmediği için yerel geliştirme sırasında Geliştirici kimlik bilgilerini kullanarak daha güvenlidir.

`Microsoft.Azure.Services.AppAuthentication` Kitaplığı yönetir kimlik doğrulaması otomatik olarak, hangi sırayla kimlik bilgilerinizi yerine, çözümünüz odaklanmanıza olanak tanır.

`Microsoft.Azure.Services.AppAuthentication` Kitaplığı Microsoft Visual Studio, Azure CLI veya Azure AD ile tümleşik kimlik doğrulaması ile yerel geliştirme destekler. Azure App Services veya bir Azure sanal makine (VM) dağıtıldığında, kitaplık otomatik olarak kullanan [yönetilen hizmet kimliği](/azure/active-directory/msi-overview) (MSI). Kod veya yapılandırma değişiklik gerekmez. Kitaplık ayrıca Azure AD doğrudan kullanımını destekleyen [istemci kimlik bilgileri](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-authenticate-service-principal) MSI kullanılabilir olmadığında veya ne zaman yerel geliştirme sırasında Geliştirici güvenlik bağlamı belirlenemiyor.

<a name="asal"></a>
## <a name="using-the-library"></a>Kitaplığı kullanma

.NET uygulamaları için bir yönetilen hizmet kimliği (MSI ile) çalışmaya en basit yolu aracılığıyladır `Microsoft.Azure.Services.AppAuthentication` paket. Nasıl başlayacağınızı şöyledir:

1. Bir başvuru ekleyin [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) uygulamanıza NuGet paketi.

2. Aşağıdaki kodu ekleyin:

    ``` csharp
    using Microsoft.Azure.Services.AppAuthentication;
    using Microsoft.Azure.KeyVault;

    // ...

    var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(
    azureServiceTokenProvider.KeyVaultTokenCallback));

    // or

    var azureServiceTokenProvider = new AzureServiceTokenProvider();
    string accessToken = await azureServiceTokenProvider.GetAccessTokenAsync(
       "https://management.azure.com/").ConfigureAwait(false);
    ```

`AzureServiceTokenProvider` Sınıfı belirtecin bellekte önbelleğe alır ve hemen önce sona erme Azure AD'den alır. Sonuç olarak, artık çağırmadan önce sona erme denetlemek yok `GetAccessTokenAsync` yöntemi. Yalnızca belirteç kullanmak istediğinizde bu yöntemi çağırın. 

`GetAccessTokenAsync` Yöntemi, bir kaynak tanımlayıcısı gerektirir. Daha fazla bilgi için bkz: [, Azure hizmetleri destekleyen yönetilen hizmet kimliği](https://docs.microsoft.com/en-us/azure/active-directory/msi-overview#which-azure-services-support-managed-service-identity).


<a name="samples"></a>
## <a name="samples"></a>Örnekler

Aşağıdaki örnekleri Göster `Microsoft.Azure.Services.AppAuthentication` Eylem Kitaplığı:

1. [Bir gizli anahtar Azure anahtar Kasası'çalışma zamanında almak için bir yönetilen hizmet Kimliği'ni (MSI) kullanın](https://github.com/Azure-Samples/app-service-msi-keyvault-dotnet)

2. [Program aracılığıyla bir Azure VM bir MSI ile bir Azure Resource Manager şablonu dağıtmayı](https://github.com/Azure-Samples/windowsvm-msi-arm-dotnet).

3. [Bir Azure Linux VM'den Azure Hizmetleri çağırmak için .NET Core örnek ve MSI kullanın](https://github.com/Azure-Samples/linuxvm-msi-keyvault-arm-dotnet/).


<a name="local"></a>
## <a name="local-development-authentication"></a>Yerel geliştirme kimlik doğrulaması

Yerel geliştirme için iki birincil kimlik doğrulaması senaryo vardır:

- [Azure hizmetlerine kimlik doğrulaması](#authenticating-to-azure-services)
- [Özel hizmetler için kimlik doğrulaması](#authenticating-to-custom-services)

Burada, her senaryo ve desteklenen araçları gereksinimleri öğrenin.


### <a name="authenticating-to-azure-services"></a>Azure hizmetlerine kimlik doğrulaması

Yerel makine yönetilen hizmet kimliği (MSI) desteklemez.  Sonuç olarak, `Microsoft.Azure.Services.AppAuthentication` Kitaplığı yerel geliştirme ortamınızda çalıştırmak için Geliştirici kimlik bilgilerini kullanır. Çözüm Azure'a dağıtıldığında, kitaplık MSI bir OAuth 2.0 istemci kimlik bilgileri verin akışı geçiş yapmak için kullanır.  Başka bir deyişle, aynı kod yerel olarak ve Uzaktan endişelenmeyin test edebilirsiniz.

Yerel geliştirme için `AzureServiceTokenProvider` kullanarak belirteçleri getirir **Visual Studio**, **Azure komut satırı arabirimi** (CLI) veya **Azure AD ile tümleşik kimlik doğrulaması**. Her seçenek sırayla denenir ve başarılı ilk seçenek kitaplığını kullanır. Hiçbir seçenek çalışırsa, bir `AzureServiceTokenProviderException` özel durumu ayrıntılı bilgilerle oluşur.

### <a name="authenticating-with-visual-studio"></a>Visual Studio ile kimlik doğrulaması

Visual Studio kullanmak için doğrulayın:

1. Yüklediğiniz [Visual Studio 2017 v15.5](https://blogs.msdn.microsoft.com/visualstudio/2017/10/11/visual-studio-2017-version-15-5-preview/) veya sonraki bir sürümü.

2. [Visual Studio için uygulama kimlik doğrulaması uzantısı](https://go.microsoft.com/fwlink/?linkid=862354) yüklenir.
 
3. Visual Studio açtığınız ve yerel geliştirme için kullanılacak bir hesap seçtiniz. Kullanım **Araçları**&nbsp;>&nbsp;**seçenekleri**&nbsp;>&nbsp;**Azure hizmeti kimlik doğrulama**bir yerel geliştirme hesabı seçin. 

Belirteç sağlayıcısı dosya ile ilgili hataları gibi Visual Studio kullanarak sorunları alıyorsanız bu adımları dikkatle gözden geçirin. 

Geliştirici belirtecinizi sağlamalarını gerekebilir.  Bunu yapmak için şu adrese gidin **Araçları**&nbsp;>&nbsp;**seçenekleri**>**Azure&nbsp;hizmet&nbsp;kimlik doğrulaması**  ve aramak bir **yeniden kimlik doğrulaması** bağlantı Seçili hesabı altında.  Kimlik doğrulaması için seçin. 

### <a name="authenticating-with-azure-cli"></a>Azure CLI ile kimlik doğrulaması

Yerel geliştirme için Azure CLI kullanmak için:

1. Yükleme [Azure CLI v2.0.12](/cli/azure/install-azure-cli) veya sonraki bir sürümü. Önceki sürümlerde yükseltin. 

2. Kullanım **az oturum açma** Azure'a oturum açmak için.

Kullanım `az account get-access-token` erişimi doğrulamak için.  Bir hata alırsanız, adım 1 başarıyla tamamlandığını doğrulayın. 

Azure CLI varsayılan dizinine yüklü değilse, bildirdiği bir hata iletisi alabilirsiniz `AzureServiceTokenProvider` yolu için Azure CLI bulamıyor.  Kullanım **AzureCLIPath**Azure CLI yükleme klasörü tanımlamak için ortam değişkeni. `AzureServiceTokenProvider`Belirtilen dizin ekler **AzureCLIPath** ortam değişkenine **yolu** ortam değişkeni gerekli olduğunda.

Azure CLI için birden çok hesabı kullanarak oturum açtığınızdan ya da birden çok abonelik hesabınıza erişimi varsa, kullanılacak belirli aboneliği belirtmeniz gerekir.  Bunu yapmak için kullanın:

```
az account set --subscription <subscription-id>
```

Bu komut yalnızca hatasında bir çıktı üretir.  Geçerli hesap ayarlarını doğrulamak için kullanın:

```
az account list
```

### <a name="authenticating-with-azure-ad-integrate-authentication"></a>Azure AD tümleştirme kimlik doğrulaması ile kimlik doğrulama

Azure AD kimlik doğrulaması kullanmak için aşağıdakileri doğrulayın:

- Şirket içi active Directory'nizi [eşitlemeler Azure ad](/azure/active-directory/connect/active-directory-aadconnect).

- Kodunuzu etki alanına katılmış bir makinede çalışıyor.


### <a name="authenticating-to-custom-services"></a>Özel hizmetler için kimlik doğrulaması

Azure Hizmetleri kullanıcılar ve uygulamalar için erişim izni olduğundan hizmeti çağrıları Azure Hizmetleri, önceki adımları çalışır.  

Özel bir hizmeti çağıran bir hizmet oluştururken, Azure AD istemci kimlik bilgileri yerel geliştirme kimlik doğrulaması için kullanın.  İki seçenek vardır: 

1.  Azure'da oturum için bir hizmet sorumlusu kullanın:

    1.  [Bir hizmet sorumlusu oluşturma](/cli/azure/create-an-azure-service-principal-azure-cli).

    2.  Oturum açmak için Azure CLI kullanın:

        ```
        az login --service-principal -u <principal-id> --password <password>
           --tenant <tenant-id> --allow-no-subscriptions
        ```

        Hizmet sorumlusu aboneliğine erişiminiz olmayabilir çünkü `--allow-no-subscriptions` bağımsız değişkeni.

2.  Ortam değişkenleri hizmet asıl ayrıntılarını belirtmek için kullanın.  Ayrıntılar için bkz [hizmet sorumlusunu kullanarak uygulamayı çalıştıran](#running-the-application-using-a-service-principal).

Azure için oturum açtığınız sonra `AzureServiceTokenProvider` yerel geliştirme için bir belirteç almak için hizmet sorumlusu kullanır.

Bu, yalnızca yerel geliştirme için geçerlidir. Çözümünüzü Azure'da dağıtıldığında, kitaplık MSI kimlik doğrulaması geçer.

<a name="msi"></a>
## <a name="running-the-application-using-a-managed-service-identity"></a>Yönetilen hizmet kimliği kullanarak uygulamayı çalıştırma 

Bir Azure uygulama hizmeti veya bir Azure VM etkin MSI ile kodunuzu çalıştırmak, kitaplık otomatik olarak yönetilen hizmet kimliği kullanır. Kod değişiklik gerekmez. 


<a name="sp"></a>
## <a name="running-the-application-using-a-service-principal"></a>Bir hizmet sorumlusu kullanarak uygulamayı çalıştırma 

Kimlik doğrulaması için bir Azure AD istemcisi kimlik bilgisi oluşturmak gerekli olabilir. Ortak örnekler:

1. Kodunuz, bir yerel geliştirme ortamı ancak geliştiricinin kimlik altında çalışır.  Örneğin, Service Fabric kullanan [NetworkService hesabını](/azure/service-fabric/service-fabric-application-secret-management) yerel geliştirme.
 
2. Bir yerel geliştirme ortamında kodunuzun çalıştığı ve dolayısıyla Geliştirici kimliğinizi kullanamaz özel bir hizmetin kimliğini. 
 
3. Kodunuzu, henüz hizmet kimliği yönetilen, Azure Batch gibi desteklemeyen bir Azure işlem kaynak üzerinde çalışıyor.

Azure AD ile imzalamak için bir sertifika kullanmak için:

1. Oluşturma bir [hizmet asıl sertifikasını](/azure/azure-resource-manager/resource-group-authenticate-service-principal). 

2. Sertifika ya da dağıtmak _LocalMachine_ veya _Currentuser'a_ depolar. 

3. Adlı bir ortam değişkeni ayarlamak **AzureServicesAuthConnectionString** için:

    ```
    RunAs=App;AppId={AppId};TenantId={TenantId};CertificateThumbprint={Thumbprint};
          CertificateStoreLocation={LocalMachine or CurrentUser}.
    ```
 
    Değiştir _{AppID}_, _{Tenantıd}_, ve _{Thumbprint}_ 1. adımda oluşturulan değerlere sahip.

    **CertificateStoreLocation** ya da olmalıdır _Currentuser'a_ veya _LocalMachine_, dağıtım planınıza göre.

4. Uygulamayı çalıştırın. 

Bir Azure kullanarak oturum AD gizli kimlik bilgisi paylaşılan:

1. Oluşturma bir [bir parola ile hizmet sorumlusu](/azure/azure-resource-manager/resource-group-authenticate-service-principal) ve anahtar kasası için erişim verin. 

2. Adlı bir ortam değişkeni ayarlamak **AzureServicesAuthConnectionString** için:

    ```
    RunAs=App;AppId={AppId};TenantId={TenantId};AppKey={ClientSecret}. 
    ```

    Değiştir _{AppID}_, _{Tenantıd}_, ve _{ClientSecret}_ 1. adımda oluşturulan değerlere sahip.

3. Uygulamayı çalıştırın. 

Her şeyin doğru başka hiçbir kod değişikliklerini ayarlandıktan sonra gereklidir.  `AzureServiceTokenProvider`Azure AD ile kimlik doğrulaması için ortam değişkeni ve sertifikayı kullanır. 

<a name="connectionstrings"></a>
## <a name="connection-string-support"></a>Bağlantı dizesi desteği

Varsayılan olarak, `AzureServiceTokenProvider` bir belirteç almak için birden çok yöntem kullanır. 

İşlemini kontrol eden geçirilen bir bağlantı dizesi kullanmak `AzureServiceTokenProvider` Oluşturucusu veya belirtilen *AzureServicesAuthConnectionString* ortam değişkeni. 

Aşağıdaki seçenekleri desteklenir:

| Bağlantı&nbsp;dize&nbsp;seçeneği | Senaryo | Yorumlar|
|:--------------------------------|:------------------------|:----------------------------|
| `RunAs=Developer; DeveloperTool=AzureCli` | Yerel geliştirme | AzureServiceTokenProvider AzureCli belirtecini almak için kullanır. |
| `RunAs=Developer; DeveloperTool=VisualStudio` | Yerel geliştirme | AzureServiceTokenProvider Visual Studio belirtecini almak için kullanır. |
| `RunAs=CurrentUser;` | Yerel geliştirme | AzureServiceTokenProvider belirtecini almak için Azure AD ile tümleşik kimlik doğrulaması kullanır. |
| `RunAs=App;` | Yönetilen Hizmet Kimliği | AzureServiceTokenProvider belirtecini almak için Yönetilen hizmet kimliği kullanır. |
| `RunAs=App;AppId={AppId};TenantId={TenantId};CertificateThumbprint`<br>`   ={Thumbprint};CertificateStoreLocation={LocalMachine or CurrentUser}`  | Hizmet sorumlusu | `AzureServiceTokenProvider`Azure AD'den belirtecini almak için sertifika kullanır. |
| `RunAs=App;AppId={AppId};TenantId={TenantId};`<br>`   CertificateSubjectName={Subject};CertificateStoreLocation=`<br>`   {LocalMachine or CurrentUser}` | Hizmet sorumlusu | `AzureServiceTokenProvider`Azure AD'den belirtecini almak için sertifika kullanıyor|
| `RunAs=App;AppId={AppId};TenantId={TenantId};AppKey={ClientSecret}` | Hizmet sorumlusu |`AzureServiceTokenProvider`Azure AD'den belirtecini almak için gizli anahtarı kullanır. |


## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinmek [yönetilen hizmet kimliği](/azure/app-service/app-service-managed-service-identity).

- Bilgi edinmek için farklı yollar [kimlik doğrulama ve yetkilendirme uygulamaları](/azure/app-service/app-service-authentication-overview).

- Azure AD hakkında daha fazla bilgi [kimlik doğrulama senaryoları](/azure/active-directory/develop/active-directory-authentication-scenarios#web-browser-to-web-application).