---
title: Öğretici:. NET'te bir Azure web uygulaması ile Azure anahtar kasası | Microsoft Docs
description: Öğretici - Key vault'tan bir gizli dizi okumak için bir ASP.NET core uygulaması yapılandırma
services: key-vault
documentationcenter: ''
author: prashanthyv
manager: rajvijan
ms.assetid: 0e57f5c7-6f5a-46b7-a18a-043da8ca0d83
ms.service: key-vault
ms.workload: identity
ms.topic: tutorial
ms.date: 09/05/2018
ms.author: pryerram
ms.custom: mvc
ms.openlocfilehash: 50a7f3166d677fe1af961866ccae4445a3d810b8
ms.sourcegitcommit: e37fa6e4eb6dbf8d60178c877d135a63ac449076
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2018
ms.locfileid: "53322150"
---
# <a name="tutorial-use-azure-key-vault-with-an-azure-web-app-in-net"></a>Öğretici: . NET'te bir Azure web uygulaması ile Azure anahtar kasası

Azure Key Vault, API anahtarları gibi gizli dizileri koruyun ve veritabanı bağlantı dizelerini yardımcı olur. Uygulamaları, hizmetleri ve BT kaynaklarına erişim sağlar.

Bu öğreticide, bir Azure anahtar kasası bilgileri okuyabilir bir Azure web uygulaması oluşturmayı öğrenin. İşlem, Azure kaynakları için yönetilen kimlikleri kullanır. Azure web uygulamaları hakkında daha fazla bilgi için bkz. [Azure Web Apps](../app-service/app-service-web-overview.md).

Makaleyi gösterilmektedir için:

> [!div class="checklist"]
> * Bir anahtar kasası oluşturma.
> * Anahtar kasasına bir gizli dizi depolama.
> * Anahtar kasasından bir gizli dizi alma.
> * Azure web uygulaması oluşturma.
> * Web uygulaması için [yönetilen kimliği](../active-directory/managed-identities-azure-resources/overview.md) etkinleştirin.
> * Web uygulamasının anahtar kasasından verileri okuması için gereken izinleri verme.
> * Web uygulamasını Azure'da çalıştırmak.

Devam etmeden önce okuma [Key Vault temel kavramlarını](key-vault-whatis.md#basic-concepts).

## <a name="prerequisites"></a>Önkoşullar

* Windows'da:
  * [.NET Core 2.1 SDK veya üzeri](https://www.microsoft.com/net/download/windows)

* Mac'te:
  * [Mac için Visual Studio](https://visualstudio.microsoft.com/vs/mac/)

* Tüm platformlar:
  * [Git](https://git-scm.com/downloads)
  * Bir Azure aboneliği <br />(Bir Azure aboneliğiniz yoksa, oluşturun bir [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) başlamadan önce.)
  * [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) sürüm 2.0.4 veya üzeri, Windows, Mac ve Linux için kullanılabilir
  * [.NET Core](https://www.microsoft.com/net/download/dotnet-core/2.1)

## <a name="managed-service-identity-and-how-it-works"></a>Yönetilen hizmet kimliği ve nasıl çalışır?

Bunlar, kodunuzda kalmayacak azure Key Vault kimlik bilgilerini güvenli bir şekilde depolar. Ancak, Azure Key Vault'a anahtarlarınızı almak için kimlik doğrulaması gerekir. Anahtar Kasası'na kimlik doğrulaması için bir kimlik bilgisi gerekir. Bu, klasik bir önyükleme ikilemle olur. Yönetilen hizmet kimliği (MSI) sağlayarak bu sorunu çözer bir _bootstrap kimlik_ , bu süreci kolaylaştırır.

Bir Azure hizmeti için MSI etkinleştirdiğinizde (örneğin: Sanal makineler, App Service ve İşlevler), Azure oluşturur bir [hizmet sorumlusu](key-vault-whatis.md#basic-concepts). MSI kimlik bilgileri, söz konusu örneğine hizmet sorumlusu ekler ve bunun için Azure Active Directory (Azure AD) hizmeti örneğini yapar.

![MSI diyagramı](media/MSI.png)

Ardından, kodunuzu bir erişim belirteci almak için Azure kaynak üzerinde kullanılabilir olan bir yerel meta veri hizmeti çağırır. Kodunuz yerel MSI_ENDPOINT'ten aldığı erişim belirtecini kullanarak Azure Key Vault hizmetinde kimlik doğrulaması yapar.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure CLI'yi kullanarak Azure'da oturum açmanız için şunu girin:

```azurecli
az login
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

1. [az group create](/cli/azure/group#az-group-create) komutunu kullanarak bir kaynak grubu oluşturun.
1. Kaynak grubu adı seçin ve yer tutucuyu doldurun. Aşağıdaki örnekte Batı ABD konumunda bir kaynak grubu oluşturulur:

   ```azurecli
   # To list locations: az account list-locations --output table
   az group create --name "<YourResourceGroupName>" --location "West US"
   ```

Bu öğretici boyunca bu kaynak grubu kullanırsınız.

## <a name="create-a-key-vault"></a>Bir anahtar kasası oluşturma

Kaynak grubunuzda bir anahtar kasası oluşturmak için aşağıdaki bilgileri sağlayın:

* Anahtar kasası adı: yalnızca sayı, harf ve kısa çizgi içerebilir ve 3-24 karakter dizisi (örneğin: 0-9, a-z, A-Z ve -)
* Kaynak grubu adı
* Konum: **Batı ABD**

Azure CLI içinde aşağıdaki komutu girin:

```azurecli
az keyvault create --name "<YourKeyVaultName>" --resource-group "<YourResourceGroupName>" --location "West US"
```

Bu noktada Azure hesabınız, bu yeni anahtar kasasında herhangi bir işlemi gerçekleştirmeye yetkili olan tek hesaptır.

## <a name="add-a-secret-to-the-key-vault"></a>Anahtar kasasına gizli dizi ekleme

Artık bir gizli dizi ekleyebilirsiniz. Bir SQL bağlantı dizesi veya güvenli ve uygulamanız için kullanılabilir durumda tutmak için gereken herhangi bir bilgi olabilir.

Anahtar Kasası'nda bir gizli dizi oluşturmak için aşağıdaki komutu adlı türü **AppSecret**. Bu gizli dizinin değerini depolar **ettiyseniz**.

```azurecli
az keyvault secret set --vault-name "<YourKeyVaultName>" --name "AppSecret" --value "MySecret"
```

Gizli diziyi düz metin olarak yer alan değeri görüntülemek için aşağıdaki komutu girin:

```azurecli
az keyvault secret show --name "AppSecret" --vault-name "<YourKeyVaultName>"
```

Bu komut, URI dahil olmak üzere gizli dizi bilgilerini gösterir. Bu adımları tamamladıktan sonra, bir anahtar kasasında gizli dizi URI'sine sahip olmanız gerekir. Bu bilgileri not alın. Daha sonraki bir adımda gerekecektir.

## <a name="create-a-net-core-web-app"></a>.NET Core web uygulaması oluşturma

İzleyin [öğretici](../app-service/app-service-web-get-started-dotnet.md) bir .NET Core web uygulaması oluşturma ve **yayımlama** Azure için. Şu videoyu da izleyebilirsiniz:

>[!VIDEO https://www.youtube.com/embed/EdiiEH7P-bU]

## <a name="open-and-edit-the-solution"></a>Çözümü açma ve düzenleme

1. Gözat **sayfaları** > **About.cshtml.cs** dosya.
2. Bu NuGet paketlerini yükleyin:
   - [AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication)
   - [KeyVault](https://www.nuget.org/packages/Microsoft.Azure.KeyVault)
3. Aşağıdaki kodu About.cshtml.cs dosyasında içeri aktarın:

   ```csharp
    using Microsoft.Azure.KeyVault;
    using Microsoft.Azure.KeyVault.Models;
    using Microsoft.Azure.Services.AppAuthentication;
   ```

4. Kodunuzu AboutModel sınıfı aşağıdaki gibi:

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
                /* The below 4 lines of code shows you how to use AppAuthentication library to fetch secrets from your Key Vault*/
                AzureServiceTokenProvider azureServiceTokenProvider = new AzureServiceTokenProvider();
                KeyVaultClient keyVaultClient = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(azureServiceTokenProvider.KeyVaultTokenCallback));
                var secret = await keyVaultClient.GetSecretAsync("https://<YourKeyVaultName>.vault.azure.net/secrets/AppSecret")
                        .ConfigureAwait(false);
                Message = secret.Value;

                /* The below do while logic is to handle throttling errors thrown by Azure Key Vault. It shows how to do exponential backoff which is the recommended client side throttling*/
                do
                {
                    long waitTime = Math.Min(getWaitTime(retries), 2000000);
                    secret = await keyVaultClient.GetSecretAsync("https://<YourKeyVaultName>.vault.azure.net/secrets/AppSecret")
                        .ConfigureAwait(false);
                    retry = false;
                } 
                while(retry && (retries++ < 10));
            }
            /// <exception cref="KeyVaultErrorException">
            /// Thrown when the operation returned an invalid status code
            /// </exception>
            catch (KeyVaultErrorException keyVaultException)
            {
                Message = keyVaultException.Message;
                if((int)keyVaultException.Response.StatusCode == 429)
                    retry = true;
            }
        }

        // This method implements exponential backoff incase of 429 errors from Azure Key Vault
        private static long getWaitTime(int retryCount)
        {
            long waitTime = ((long)Math.Pow(2, retryCount) * 100L);
            return waitTime;
        }

        // This method fetches a token from Azure Active Directory which can then be provided to Azure Key Vault to authenticate
        public async Task<string> GetAccessTokenAsync()
        {
            var azureServiceTokenProvider = new AzureServiceTokenProvider();
            string accessToken = await azureServiceTokenProvider.GetAccessTokenAsync("https://vault.azure.net");
            return accessToken;
        }
    }
    ```

## <a name="run-the-app"></a>Uygulamayı çalıştırma

1. Visual Studio 2017'in ana menüden seçin **hata ayıklama** > **Başlat** ile veya hata ayıklama olmadan. 
1. Tarayıcı görüntülendiğinde **Hakkında** sayfasına gidin.
1. **AppSecret** değeri görüntülenir.

## <a name="enable-a-managed-identity-for-the-web-app"></a>Web uygulaması için yönetilen kimliği etkinleştirme

Azure Key Vault kimlik bilgileri ve diğer gizli dizileri güvenli bir şekilde depolamak için bir yol sağlar, ancak kodunuzu bunları almak için anahtar Kasası'na kimlik doğrulaması yapması. [Yönetilen Azure kaynaklarına genel bakış için kimlikleri](../active-directory/managed-identities-azure-resources/overview.md) Azure vererek bu sorunu çözmeye yardımcı olur, Azure AD'de otomatik olarak yönetilen bir kimlik Hizmetleri. Bu kimliği kullanarak, Key Vault da dahil olmak üzere Azure AD kimlik doğrulamasını destekleyen tüm hizmetlerde kodunuzda kimlik bilgileri bulunmasına gerek kalmadan kimlik doğrulaması yapabilirsiniz.

1. Azure CLI, bu uygulama için kimlik oluşturmak için Ata kimlik komutu çalıştırın:

   ```azurecli

   az webapp identity assign --name "<YourAppName>" --resource-group "<YourResourceGroupName>"

   ```

   >[!NOTE]
   >Değiştirmek zorunda \<Uygulamanızınadı\> azure'da yayımlanan uygulamanın adı. Örneğin, yayımlanan uygulama adı, **MyAwesomeapp.azurewebsites.net**, değiştirin \<Uygulamanızınadı\> ile **MyAwesomeapp**.

1. Not `PrincipalId` uygulamayı azure'a yayımlarken. 1. adımda komutunun çıkışı şu biçimde olmalıdır:

   ```json
   {
     "principalId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
     "tenantId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
     "type": "SystemAssigned"
   }
   ```

>[!NOTE]
>Bu yordamdaki komut, [portala](https://portal.azure.com) gidip web uygulaması özelliklerinde **Kimlik/Sistem tarafından atanan** ayarını **Açık** duruma getirmekle eşdeğerdir.

## <a name="assign-permissions-to-your-application-to-read-secrets-from-key-vault"></a>Key Vault gizli dizilerini okumak için uygulamanıza izin atama

Değiştirin \<YourKeyVaultName\> anahtar kasanıza adıyla ve \<Principalıd\> değeriyle **Principalıd** aşağıdaki komutta:

```azurecli
az keyvault set-policy --name '<YourKeyVaultName>' --object-id <PrincipalId> --secret-permissions get list
```

Bu komut, uygulama kimliği (MSI) yapmak için hizmet izinleri verir. **alma** ve **listesi** anahtar kasanıza operations.

## <a name="publish-the-web-application-to-azure"></a>Web uygulamasını Azure’a yayımlama

Web uygulamanızı canlı web uygulamanızın gizli değer getirebilirsiniz yeniden görmek için Azure'a yayımlayın.

1. Visual Studio'da **key-vault-dotnet-core-quickstart** projesini seçin.
2. **Yayımla** > **Başlat** seçeneğini belirleyin.
3. **Oluştur**’u seçin.

Uygulamayı çalıştırdığınızda, gizli değer alabilir görmeniz gerekir.

Şimdi, başarılı bir şekilde bir web uygulaması, depolayan ve Key Vault'tan rodcpurgeaccount getirir. NET'te oluşturdunuz.

## <a name="next-steps"></a>Sonraki adımlar

>[!div class="nextstepaction"]
>[Azure Key Vault Geliştirici Kılavuzu](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-developers-guide)
