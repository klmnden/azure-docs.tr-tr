---
title: Öğretici:. NET'te bir Azure web uygulaması ile Azure anahtar kasası | Microsoft Docs
description: Bu öğreticide, anahtar kasasından gizli dizi okumak için bir ASP.NET core uygulaması yapılandırın.
services: key-vault
author: msmbaldwin
manager: rajvijan
ms.service: key-vault
ms.topic: tutorial
ms.date: 12/21/2018
ms.author: pryerram
ms.custom: mvc
ms.openlocfilehash: 9a59255097c6cb2a6728a14c3dbe19dbcbb0932a
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2019
ms.locfileid: "65236785"
---
# <a name="tutorial-use-azure-key-vault-with-an-azure-web-app-in-net"></a>Öğretici: . NET'te bir Azure web uygulaması ile Azure anahtar kasası

Azure Key Vault, API anahtarları gibi gizli dizileri koruyun ve veritabanı bağlantı dizelerini yardımcı olur. Uygulamaları, hizmetleri ve BT kaynaklarına erişim sağlar.

Bu öğreticide, bir Azure anahtar kasası bilgileri okuyabilir bir Azure web uygulaması oluşturmayı öğrenin. İşlem, Azure kaynakları için yönetilen kimlikleri kullanır. Azure web uygulamaları hakkında daha fazla bilgi için bkz. [Azure App Service](../app-service/overview.md).

Öğretici şunların nasıl yapıldığını göstermektedir:

> [!div class="checklist"]
> * Bir anahtar kasası oluşturma.
> * Gizli anahtar Kasası'na ekleyin.
> * Anahtar kasasından bir gizli dizi alma.
> * Bir Azure web uygulaması oluşturun.
> * Web uygulaması için bir yönetilen kimlik etkinleştirin.
> * Web uygulamasının iznini atayın.
> * Web uygulamasını Azure'da çalıştırın.

Başlamadan önce okumanız [Key Vault temel kavramlarını](key-vault-whatis.md#basic-concepts). 

Azure aboneliğiniz yoksa, oluşturun bir [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="prerequisites"></a>Önkoşullar

* İçin Windows: [.NET Core 2.1 SDK veya üzeri](https://www.microsoft.com/net/download/windows)
* Mac için: [Mac için Visual Studio](https://visualstudio.microsoft.com/vs/mac/)
* Windows, Mac ve Linux için:
  * [Git](https://git-scm.com/downloads)
  * Bu öğretici, Azure CLI'yi yerel olarak çalıştırmanızı gerektirir. Sonraki bir sürümünün yüklü veya Azure CLI 2.0.4 sürüm olmalıdır. Sürümü bulmak için `az --version` komutunu çalıştırın. CLI’yı yüklemeniz veya yükseltmeniz gerekiyorsa bkz. [Azure CLI 2.0’ı yükleme](https://review.docs.microsoft.com/cli/azure/install-azure-cli).
  * [.NET Core](https://www.microsoft.com/net/download/dotnet-core/2.1)

## <a name="about-managed-service-identity"></a>Yönetilen Hizmet Kimliği hakkında

Kodunuzda görüntülenmez için azure Key Vault kimlik bilgilerini güvenli bir şekilde, depolar. Ancak, Azure Key Vault'a anahtarlarınızı almak için kimlik doğrulaması gerekir. Anahtar Kasası'na kimlik doğrulaması için bir kimlik bilgisi gerekir. Bu, klasik bir önyükleme ikilemle olur. Yönetilen hizmet kimliği (MSI) sağlayarak bu sorunu çözer bir _bootstrap kimlik_ , bu süreci kolaylaştırır.

Azure sanal makineler, Azure App Service veya Azure işlevleri gibi bir Azure hizmeti için MSI'yı etkinleştirdiğinizde, Azure oluşturur bir [hizmet sorumlusu](key-vault-whatis.md#basic-concepts). MSI, Azure Active Directory'de (Azure AD) hizmet örneği için bunu yapar ve bu örneğe hizmet sorumlusu kimlik bilgileri ekler.

![MSI diyagramı](media/MSI.png)

Ardından, bir erişim belirteci almak için kodunuzu Azure kaynak üzerinde kullanılabilir olan bir yerel meta veri hizmeti çağırır. Kodunuzu yerel MSI uç noktasından bir Azure anahtar kasası hizmetinde kimlik doğrularken alır erişim belirtecini kullanır.

## <a name="log-in-to-azure"></a>Azure'da oturum açma

Azure CLI'yi kullanarak Azure'da oturum açmak için, şunları girin:

```azurecli
az login
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturun

Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

[az group create](/cli/azure/group#az-group-create) komutunu kullanarak bir kaynak grubu oluşturun.

Ardından, bir kaynak grubu adı seçin ve yer tutucuyu doldurun. Aşağıdaki örnekte Batı ABD konumunda bir kaynak grubu oluşturulur:

   ```azurecli
   # To list locations: az account list-locations --output table
   az group create --name "<YourResourceGroupName>" --location "West US"
   ```

Bu öğretici boyunca bu kaynak grubu kullanırsınız.

## <a name="create-a-key-vault"></a>Bir anahtar kasası oluşturma

Kaynak grubunuzda bir anahtar kasası oluşturmak için aşağıdaki bilgileri sağlayın:

* Anahtar kasası adı: bir dizenin yalnızca rakam (0-9) içerebilir ve 3-24 karakter harf (a-z, A-Z) ve tire (-)
* Kaynak grubu adı
* Konum: **Batı ABD**

Azure CLI, aşağıdaki komutu girin:

```azurecli
az keyvault create --name "<YourKeyVaultName>" --resource-group "<YourResourceGroupName>" --location "West US"
```

Bu noktada Azure hesabınız bu yeni kasa üzerinde işlem gerçekleştirmek için yetkili tek hesaptır.

## <a name="add-a-secret-to-the-key-vault"></a>Anahtar kasasına gizli dizi ekleme

Artık bir gizli dizi ekleyebilirsiniz. Bir SQL bağlantı dizesi veya güvenli ve uygulamanız için kullanılabilir durumda tutmak için gereken herhangi bir bilgi olabilir.

Anahtar Kasası'nda bir gizli dizi oluşturmak için çağrılan **AppSecret**, aşağıdaki komutu girin: 

```azurecli
az keyvault secret set --vault-name "<YourKeyVaultName>" --name "AppSecret" --value "MySecret"
```

Bu gizli dizinin değerini depolar **ettiyseniz**.

Bulunan değerin gizli diziyi düz metin olarak görüntülemek için aşağıdaki komutu girin:

```azurecli
az keyvault secret show --name "AppSecret" --vault-name "<YourKeyVaultName>"
```

Bu komut, URI gizli bilgilerini görüntüler. 

Bu adımları tamamladıktan sonra, bir anahtar kasasında gizli dizi URI'sine sahip olmanız gerekir. Bu öğreticide daha sonra kullanmak için bu bilgileri not edin. 

## <a name="create-a-net-core-web-app"></a>.NET Core web uygulaması oluşturma

.NET Core web uygulaması oluşturma ve bunu Azure'a yayımlamak için yönergeleri izleyin. [Azure'da ASP.NET Core web uygulaması oluşturma](../app-service/app-service-web-get-started-dotnet.md). 

Ayrıca şu videoyu izleyebilirsiniz:

>[!VIDEO https://www.youtube.com/embed/EdiiEH7P-bU]

## <a name="open-and-edit-the-solution"></a>Çözümü açma ve düzenleme

1. Git **sayfaları** > **About.cshtml.cs** dosya.

1. Bu NuGet paketlerini yükleyin:
   - [AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication)
   - [KeyVault](https://www.nuget.org/packages/Microsoft.Azure.KeyVault)

1. Aşağıdaki kodu alma *About.cshtml.cs* dosyası:

   ```csharp
    using Microsoft.Azure.KeyVault;
    using Microsoft.Azure.KeyVault.Models;
    using Microsoft.Azure.Services.AppAuthentication;
   ```

   Kodunuzu AboutModel sınıfı şu şekilde görünmelidir:

   ```csharp
    public class AboutModel : PageModel
    {
        public string Message { get; set; }

        public async Task OnGetAsync()
        {
            Message = "Your application description page.";
            int retries = 0;
            bool retry = false;
            try
            {
                /* The next four lines of code show you how to use AppAuthentication library to fetch secrets from your key vault */
                AzureServiceTokenProvider azureServiceTokenProvider = new AzureServiceTokenProvider();
                KeyVaultClient keyVaultClient = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(azureServiceTokenProvider.KeyVaultTokenCallback));
                var secret = await keyVaultClient.GetSecretAsync("https://<YourKeyVaultName>.vault.azure.net/secrets/AppSecret")
                        .ConfigureAwait(false);
                Message = secret.Value;
            }
            /* If you have throttling errors see this tutorial https://docs.microsoft.com/azure/key-vault/tutorial-net-create-vault-azure-web-app */
            /// <exception cref="KeyVaultErrorException">
            /// Thrown when the operation returned an invalid status code
            /// </exception>
            catch (KeyVaultErrorException keyVaultException)
            {
                Message = keyVaultException.Message;
            }
        }

        // This method implements exponential backoff if there are 429 errors from Azure Key Vault
        private static long getWaitTime(int retryCount)
        {
            long waitTime = ((long)Math.Pow(2, retryCount) * 100L);
            return waitTime;
        }

        // This method fetches a token from Azure Active Directory, which can then be provided to Azure Key Vault to authenticate
        public async Task<string> GetAccessTokenAsync()
        {
            var azureServiceTokenProvider = new AzureServiceTokenProvider();
            string accessToken = await azureServiceTokenProvider.GetAccessTokenAsync("https://vault.azure.net");
            return accessToken;
        }
    }
    ```

## <a name="run-the-web-app"></a>Web uygulamasını çalıştırma

1. Visual Studio 2017'in ana menüsünde **hata ayıklama** > **Başlat**, ile veya hata ayıklama olmadan. 
1. Tarayıcıda, Git **hakkında** sayfası.  
    **AppSecret** değeri görüntülenir.

## <a name="enable-a-managed-identity"></a>Yönetilen bir kimlik etkinleştir

Azure Key Vault kimlik bilgileri ve diğer gizli dizileri güvenli bir şekilde depolamak için bir yol sağlar, ancak kodunuzu bunları almak için anahtar Kasası'na kimlik doğrulaması yapması. [Yönetilen Azure kaynaklarına genel bakış için kimlikleri](../active-directory/managed-identities-azure-resources/overview.md) Azure vererek bu sorunu çözmeye yardımcı olur, Azure AD'de otomatik olarak yönetilen bir kimlik Hizmetleri. Bu kimlik, anahtar kasası, kodunuzda kimlik bilgilerini görüntülemek zorunda kalmadan dahil olmak üzere Azure AD kimlik doğrulamasını destekleyen herhangi bir hizmeti kimlik doğrulaması için kullanabilirsiniz.

Azure CLI, bu uygulama için kimlik oluşturmak için Ata kimlik komutu çalıştırın:

```azurecli
az webapp identity assign --name "<YourAppName>" --resource-group "<YourResourceGroupName>"
```

Değiştirin \<Uygulamanızınadı > azure'da yayımlanan uygulamanın adı.  
    Örneğin, yayımlanan uygulama adı, **MyAwesomeapp.azurewebsites.net**, değiştirin \<Uygulamanızınadı > ile **MyAwesomeapp**.

Not `PrincipalId` uygulamayı azure'a yayımlarken. 1. adımda komutunun çıkışı şu biçimde olmalıdır:

```json
{
  "principalId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "tenantId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "type": "SystemAssigned"
}
```

>[!NOTE]
>Bu yordamdaki komutu gidip eşdeğerdir [Azure portalında](https://portal.azure.com) ve geçiş **kimlik / sistem atanan** ayarını **üzerinde** web uygulamasındaki özellikleri.

## <a name="assign-permissions-to-your-app"></a>Uygulamanıza izinler atama

Değiştirin \<YourKeyVaultName > key vault ile değiştir adıyla \<Principalıd > değeriyle **Principalıd** aşağıdaki komutta:

```azurecli
az keyvault set-policy --name '<YourKeyVaultName>' --object-id <PrincipalId> --secret-permissions get list
```

Bu komut kimliği (MSI) yapmak için app service izni verir. **alma** ve **listesi** anahtar kasanıza operations.

## <a name="publish-the-web-app-to-azure"></a>Web uygulamasını Azure’da yayımlama

Web uygulamanızı yeniden canlı web uygulamanızın gizli değer getirebilirsiniz doğrulamak için Azure'da yayımlayın.

1. Visual Studio'da **key-vault-dotnet-core-quickstart** projesini seçin.
2. **Yayımla** > **Başlat** seçeneğini belirleyin.
3. **Oluştur**’u seçin.

Uygulamayı çalıştırdığınızda, gizli değer alabilir görmeniz gerekir.

Şimdi, depolayan ve key vault'ta rodcpurgeaccount getirir. NET'te başarıyla bir web uygulaması oluşturdunuz.

## <a name="clean-up-resources"></a>Kaynakları temizleme
Artık gerekli değilse, sanal makine ve anahtar kasanıza silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

>[!div class="nextstepaction"]
>[Azure Key Vault Geliştirici Kılavuzu](https://docs.microsoft.com/azure/key-vault/key-vault-developers-guide)
