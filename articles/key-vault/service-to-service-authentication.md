---
title: .NET kullanarak Azure Key Vault hizmetten hizmete kimlik doğrulaması
description: .NET kullanarak Azure Key Vault için kimlik doğrulaması için Microsoft.Azure.Services.AppAuthentication kitaplığını kullanın.
keywords: yerel kimlik bilgilerini Azure key vault kimlik doğrulaması
author: bryanla
manager: mbaldwin
services: key-vault
ms.author: bryanla
ms.date: 11/27/2018
ms.topic: conceptual
ms.prod: ''
ms.service: key-vault
ms.technology: ''
ms.assetid: 4be434c4-0c99-4800-b775-c9713c973ee9
ms.openlocfilehash: 1eadea53dda60ef5ac8bbbc3d9e9cfe4b5b373dc
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/27/2018
ms.locfileid: "52423601"
---
# <a name="service-to-service-authentication-to-azure-key-vault-using-net"></a>.NET kullanarak Azure Key Vault hizmetten hizmete kimlik doğrulaması

Azure anahtar Kasası'na kimlik doğrulaması için bir Azure Active Directory (AD) kimlik bilgisi, paylaşılan bir gizli dizi veya bir sertifika gerekir. Bu kimlik bilgilerinin yönetimiyle zor olabilir ve kaynak veya yapılandırma dosyalarında dahil ederek bir uygulamaya kimlik bilgilerini paket daha cazip.

`Microsoft.Azure.Services.AppAuthentication` .NET kitaplığı bu sorunu kolaylaştırır. Yerel geliştirme sırasında kimlik doğrulaması yapmak için Geliştirici kimlik bilgilerini kullanır. Çözüm daha sonra Azure'a dağıtıldığında, kitaplık için uygulama kimlik bilgileri otomatik olarak geçer.  

Azure AD kimlik bilgileri oluşturun veya paylaşım geliştiricileri arasında kimlik bilgileri gerekmez çünkü, yerel geliştirme sırasında Geliştirici kimlik bilgilerinizi kullanarak daha güvenlidir.

`Microsoft.Azure.Services.AppAuthentication` Kitaplığı yönetir kimlik doğrulama otomatik olarak hangi sırayla kimlik bilgilerinizi yerine çözümünüzün odaklanmanızı sağlar.

`Microsoft.Azure.Services.AppAuthentication` Kitaplığı, Microsoft Visual Studio, Azure CLI veya Azure AD tümleşik kimlik doğrulaması ile yerel geliştirmeyi destekler. Azure App Services veya Azure sanal makinesi (VM) dağıtıldığında otomatik olarak kitaplığı kullanan [yönetilen Azure Hizmetleri için kimlikleri](/azure/active-directory/msi-overview). Kod veya yapılandırma değişiklik gerekmez. Kitaplık ayrıca doğrudan Azure AD'ye destekler [istemci kimlik bilgileri](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authenticate-service-principal) bir yönetilen kimlik mevcut olmadığında ya da yerel geliştirme sırasında Geliştirici güvenlik bağlamı belirlenemiyor.

<a name="asal"></a>
## <a name="using-the-library"></a>Kitaplığı kullanma

.NET uygulamaları için yönetilen bir kimlik ile çalışmak için en basit yolu aracılığıyladır `Microsoft.Azure.Services.AppAuthentication` paket. Nasıl başlayacağınızı şöyledir:

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

`AzureServiceTokenProvider` Sınıfı bellekteki belirteci önbelleğe alır ve süre sonu hemen önce Azure AD'den alır. Sonuç olarak, artık çağırmadan önce sona erme denetlemek sahip olduğunuz `GetAccessTokenAsync` yöntemi. Yalnızca belirteç kullanmak istediğinizde bu yöntemi çağırın. 

`GetAccessTokenAsync` Yöntemi bir kaynak tanımlayıcısı gerektirir. Daha fazla bilgi için bkz. [hangi Azure hizmetlerinin Azure kaynakları için yönetilen kimlikleri destekleyen](https://docs.microsoft.com/azure/active-directory/msi-overview#which-azure-services-support-managed-service-identity).


<a name="samples"></a>
## <a name="samples"></a>Örnekler

Aşağıdaki örnekler show `Microsoft.Azure.Services.AppAuthentication` uygulamada kitaplığı:

1. [Çalışma zamanında Azure Key Vault'tan bir gizli dizi alınacak yönetilen bir kimlik kullanın.](https://github.com/Azure-Samples/app-service-msi-keyvault-dotnet)

2. [Program aracılığıyla yönetilen bir kimlik ile bir Azure VM'den bir Azure Resource Manager şablonu dağıtma](https://github.com/Azure-Samples/windowsvm-msi-arm-dotnet).

3. [Bir Azure Linux sanal makinesinden Azure hizmetlerini çağırmak için .NET Core örnek ve yönetilen bir kimlik kullanın](https://github.com/Azure-Samples/linuxvm-msi-keyvault-arm-dotnet/).


<a name="local"></a>
## <a name="local-development-authentication"></a>Yerel geliştirme kimlik doğrulaması

Yerel geliştirme için iki birincil kimlik doğrulama senaryosu vardır:

- [Azure Hizmetleri için kimlik doğrulaması](#authenticating-to-azure-services)
- [Özel hizmetler için kimlik doğrulaması](#authenticating-to-custom-services)

Burada, her senaryo ve desteklenen araçları gereksinimleri öğrenin.


### <a name="authenticating-to-azure-services"></a>Azure Hizmetleri için kimlik doğrulaması

Yerel makineler yönetilen kimlikleri, Azure kaynakları için desteklemez.  Sonuç olarak, `Microsoft.Azure.Services.AppAuthentication` kitaplığı, yerel geliştirme ortamınızda çalıştırmak için Geliştirici kimlik bilgilerinizi kullanır. Kitaplığı yönetilen bir kimlik çözümü Azure'a dağıtıldığında, bir OAuth 2.0 istemci kimlik bilgileri verme akışı geçiş yapmak için kullanır.  Başka bir deyişle, aynı kodu yerel olarak ve Uzaktan endişelenmeyin test edebilirsiniz.

Yerel geliştirme için `AzureServiceTokenProvider` belirteçleri kullanarak getirir **Visual Studio**, **Azure komut satırı arabirimi** (CLI) veya **Azure AD tümleşik kimlik doğrulaması**. Her seçeneği sırayla denenir ve başarılı bir ilk seçenek kitaplık kullanır. Hiçbir seçenek çalışırsa, bir `AzureServiceTokenProviderException` ayrıntılı bilgilerle özel durumu harekete geçirilir.

### <a name="authenticating-with-visual-studio"></a>Visual Studio ile kimlik doğrulaması

Visual Studio kullanmak için aşağıdakileri doğrulayın:

1. Yüklediğiniz [Visual Studio 2017 v15.5](https://blogs.msdn.microsoft.com/visualstudio/2017/10/11/visual-studio-2017-version-15-5-preview/) veya üzeri.

2. [Visual Studio için uygulama kimlik doğrulaması uzantısı](https://go.microsoft.com/fwlink/?linkid=862354) yüklenir.
 
3. Visual Studio'ya açmıştır ve yerel geliştirme için kullanılacak bir hesabı seçtiniz. Kullanım **Araçları**&nbsp;>&nbsp;**seçenekleri**&nbsp;>&nbsp;**Azure hizmeti kimlik doğrulaması**bir yerel geliştirme hesabı seçmek için. 

Belirteç sağlayıcı dosya ilgili hatalar gibi Visual Studio kullanarak sorun yaşarsanız bu adımları dikkatle gözden geçirin. 

Geliştirici belirtecinizi yeniden kimlik doğrulamaya zorlayabilir gerekebilir.  Bunu yapmak için Git **Araçları**&nbsp;>&nbsp;**seçenekleri**>**Azure&nbsp;hizmet&nbsp;kimlik doğrulaması**  ve Ara bir **yeniden kimlik doğrulaması** seçili hesap altında bağlantı.  Kimlik doğrulaması için seçin. 

### <a name="authenticating-with-azure-cli"></a>Azure CLI ile kimlik doğrulaması

Yerel geliştirme için Azure CLI kullanmak için:

1. Yükleme [Azure CLI v2.0.12](/cli/azure/install-azure-cli) veya üzeri. Önceki sürümlerini yükseltin. 

2. Kullanım **az login** için Azure'da oturum açın.

Kullanım `az account get-access-token` erişimi doğrulamak için.  Bir hata alırsanız, 1. adım başarıyla tamamlandığını doğrulayın. 

Azure CLI için varsayılan dizin yüklü değilse, raporlama bir hata alabilirsiniz `AzureServiceTokenProvider` yolu, Azure CLI için bulunamıyor.  Kullanım **AzureCLIPath**ortam değişkenini Azure CLI yükleme klasörünü tanımlayın. `AzureServiceTokenProvider` Belirtilen dizini ekler **AzureCLIPath** ortam değişkenine **yolu** ortam değişkeni gerektiğinde.

Azure CLI kullanarak birden çok hesap için oturum veya hesabınızı birden çok aboneliğe erişiminiz varsa, kullanılacak belirli abonelik belirtmeniz gerekir.  Bunu yapmak için kullanın:

```
az account set --subscription <subscription-id>
```

Bu komut yalnızca hatasında bir çıktı üretir.  Geçerli hesap ayarlarını doğrulamak için kullanın:

```
az account list
```

### <a name="authenticating-with-azure-ad-integrate-authentication"></a>Azure AD tümleştirme kimlik doğrulaması ile kimlik doğrulama

Azure AD kimlik doğrulamasını kullanmak için aşağıdakileri doğrulayın:

- Şirket içi active directory'nizde [Azure AD'ye eşitler](/azure/active-directory/connect/active-directory-aadconnect).

- Kodunuzu etki alanına katılmış bir makinede çalışıyor.


### <a name="authenticating-to-custom-services"></a>Özel hizmetler için kimlik doğrulaması

Azure hizmetleri hem kullanıcılar hem de uygulamaları erişim izni olduğundan hizmet çağrıları Azure Hizmetleri, önceki adımlar işe.  

Özel bir hizmeti çağıran bir hizmet oluştururken, yerel geliştirme kimlik doğrulaması için Azure AD istemci kimlik bilgileri kullanın.  İki seçenek vardır: 

1.  Azure'da oturum açmak için bir hizmet sorumlusu kullanın:

    1.  [Hizmet sorumlusu oluşturma](/cli/azure/create-an-azure-service-principal-azure-cli).

    2.  Oturum açmak için Azure CLI kullanın:

        ```
        az login --service-principal -u <principal-id> --password <password>
           --tenant <tenant-id> --allow-no-subscriptions
        ```

        Hizmet sorumlusu bir aboneliğe erişiminiz olmayabilir çünkü `--allow-no-subscriptions` bağımsız değişken.

2.  Hizmet sorumlusu ayrıntıları belirtmek için ortam değişkenlerini kullanın.  Ayrıntılar için bkz [bir hizmet sorumlusunu kullanarak uygulamayı çalıştıran](#running-the-application-using-a-service-principal).

Azure için açıldıktan sonra `AzureServiceTokenProvider` yerel geliştirme için bir belirteç almak için hizmet sorumlusu kullanır.

Bu, yalnızca yerel geliştirme için geçerlidir. Çözümünüzü Azure'da dağıtıldığında, kitaplığı yönetilen bir kimlik doğrulamasını geçer.

<a name="msi"></a>
## <a name="running-the-application-using-managed-identity"></a>Yönetilen kimlik kullanarak uygulamayı çalıştırma 

Kodunuzu bir Azure App Service veya Azure VM yönetilen bir kimlikle etkin çalıştırdığınızda, kitaplık yönetilen kimlik otomatik olarak kullanır. Hiçbir kod değişikliği gerekli değildir. 


<a name="sp"></a>
## <a name="running-the-application-using-a-service-principal"></a>Bir hizmet sorumlusunu kullanarak uygulamayı çalıştırma 

Kimlik doğrulaması için bir Azure AD istemci kimlik bilgisi oluşturmak için gerekli olabilir. Ortak örnekleri şunlardır:

1. Kodunuz, bir yerel geliştirme ortamı ancak Geliştirici kimlik altında çalışır.  Örneğin, Service Fabric kullanan [NetworkService hesabı](/azure/service-fabric/service-fabric-application-secret-management) yerel geliştirme için.
 
2. Kodunuzu yerel geliştirme ortamınızda çalışır ve Geliştirici kimlik bilgilerinizi kullanamazlar özel bir hizmet için kimlik doğrulaması. 
 
3. Kodunuzu henüz yönetilen kimlikleri, Azure Batch gibi Azure kaynakları için desteği olmayan bir Azure bilgi işlem kaynağına çalışıyor.

Azure AD ile imzalamak için bir sertifika kullanmak için:

1. Oluşturma bir [hizmet sorumlusu sertifikasını](/azure/azure-resource-manager/resource-group-authenticate-service-principal). 

2. Sertifika ya da dağıtma *LocalMachine* veya *CurrentUser* depolayın. 

3. Adlı bir ortam değişkenini ayarlamak **AzureServicesAuthConnectionString** için:

    ```
    RunAs=App;AppId={AppId};TenantId={TenantId};CertificateThumbprint={Thumbprint};
          CertificateStoreLocation={CertificateStore}
    ```
 
    Değiştirin *{AppID}*, *{Tenantıd}*, ve *{Thumbprint}* 1. adımda oluşturulan değerlere sahip. Değiştirin *{CertificateStore}* ya da ile `LocalMachine` veya `CurrentUser`dağıtım planınıza göre.

4. Uygulamayı çalıştırın. 

Azure ile oturum açması AD gizli dizi kimlik bilgisi paylaşılan:

1. Oluşturma bir [bir parola ile hizmet sorumlusu](/azure/azure-resource-manager/resource-group-authenticate-service-principal) ve Key vault'a erişim verin. 

2. Adlı bir ortam değişkenini ayarlamak **AzureServicesAuthConnectionString** için:

    ```
    RunAs=App;AppId={AppId};TenantId={TenantId};AppKey={ClientSecret} 
    ```

    Değiştirin _{AppID}_, _{Tenantıd}_, ve _{ClientSecret}_ 1. adımda oluşturulan değerlere sahip.

3. Uygulamayı çalıştırın. 

Her şeyin doğru bir şekilde başka hiçbir kod değişikliklerini ayarlandıktan sonra gereklidir.  `AzureServiceTokenProvider` Azure AD'ye kimlik doğrulaması için ortam değişkenini ve sertifikayı kullanır. 

<a name="connectionstrings"></a>
## <a name="connection-string-support"></a>Bağlantı dizesi desteği

Varsayılan olarak, `AzureServiceTokenProvider` bir belirteç almak için birden çok yöntem kullanır. 

İşlemini denetlemek için geçirilen bir bağlantı dizesi kullanın `AzureServiceTokenProvider` Oluşturucusu veya belirtilen *AzureServicesAuthConnectionString* ortam değişkeni. 

Aşağıdaki seçenekler desteklenir:

| Bağlantı&nbsp;dize&nbsp;seçeneği | Senaryo | Yorumlar|
|:--------------------------------|:------------------------|:----------------------------|
| `RunAs=Developer; DeveloperTool=AzureCli` | Yerel geliştirme | AzureServiceTokenProvider Azureclı belirteci almak için kullanır. |
| `RunAs=Developer; DeveloperTool=VisualStudio` | Yerel geliştirme | AzureServiceTokenProvider Visual Studio, belirteci almak için kullanır. |
| `RunAs=CurrentUser;` | Yerel geliştirme | AzureServiceTokenProvider, belirteci almak için Azure AD tümleşik kimlik doğrulaması kullanır. |
| `RunAs=App;` | Azure kaynakları için yönetilen kimlikleri | AzureServiceTokenProvider yönetilen bir kimlik belirteci almak için kullanır. |
| `RunAs=App;AppId={AppId};TenantId={TenantId};CertificateThumbprint`<br>`   ={Thumbprint};CertificateStoreLocation={LocalMachine or CurrentUser}`  | Hizmet sorumlusu | `AzureServiceTokenProvider` Azure AD'den belirteci almak için sertifikayı kullanır. |
| `RunAs=App;AppId={AppId};TenantId={TenantId};`<br>`   CertificateSubjectName={Subject};CertificateStoreLocation=`<br>`   {LocalMachine or CurrentUser}` | Hizmet sorumlusu | `AzureServiceTokenProvider` Azure AD'den belirteci almak için sertifikayı kullanır.|
| `RunAs=App;AppId={AppId};TenantId={TenantId};AppKey={ClientSecret}` | Hizmet sorumlusu |`AzureServiceTokenProvider` Azure AD'den belirteci almak için gizli anahtarını kullanır. |


## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [kimliklerini Azure kaynakları için yönetilen](/azure/app-service/app-service-managed-service-identity).

- Bilgi edinmek için farklı yollar [kimliğini doğrulama ve yetkilendirme uygulamaları](/azure/app-service/app-service-authentication-overview).

- Azure AD hakkında daha fazla bilgi [kimlik doğrulama senaryoları](/azure/active-directory/develop/active-directory-authentication-scenarios#web-browser-to-web-application).
