---
title: Öğretici - .NET'te Azure Web Uygulaması ile Azure Key Vault'u kullanma | Microsoft Docs
description: Öğretici Anahtar Kasasından gizli dizi okumak için bir ASP.NET Core uygulaması yapılandırma
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
ms.openlocfilehash: 9cc22e158a9473b7b60f7e8bcb57174abc1fb8cc
ms.sourcegitcommit: 1b186301dacfe6ad4aa028cfcd2975f35566d756
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2018
ms.locfileid: "51218561"
---
# <a name="tutorial-how-to-use-azure-key-vault-with-azure-web-app-in-net"></a>Öğretici: .NET'te Azure Web Uygulaması ile Azure Key Vault'u kullanma

Azure Key Vault uygulamalarınıza, hizmetlerinize ve BT kaynaklarınıza erişmek için gereken API Anahtarları, Veritabanı Bağlantısı dizeleri gibi gizli dizileri korumanıza yardımcı olur.

Bu öğreticide, bir Azure web uygulamasının Azure kaynakları için yönetilen kimlikleri kullanarak Azure Key Vault’tan bilgi okumasını sağlamak için gerekli adımları uygulayacaksınız. Bu öğreticide [Azure Web Apps](../app-service/app-service-web-overview.md) temel alınmıştır. Şunları yapmayı öğreneceksiniz:

> [!div class="checklist"]
> * Bir anahtar kasası oluşturma.
> * Anahtar kasasına bir gizli dizi depolama.
> * Anahtar kasasından bir gizli dizi alma.
> * Azure web uygulaması oluşturma.
> * Web uygulaması için [yönetilen kimliği](../active-directory/managed-identities-azure-resources/overview.md) etkinleştirin.
> * Web uygulamasının anahtar kasasından verileri okuması için gereken izinleri verme.
> * Azure'da Web Uygulamasını çalıştırma

İlerlemeden önce lütfen [temel kavramları](key-vault-whatis.md#basic-concepts) okuyun.

## <a name="prerequisites"></a>Ön koşullar

* Windows'da:
  * [.NET Core 2.1 SDK veya üzeri](https://www.microsoft.com/net/download/windows)

* Mac'te:
  * Bkz. [Mac için Visual Studio'daki Yenilikler](https://visualstudio.microsoft.com/vs/mac/).

* Tüm platformlar:
  * Git ([indir](https://git-scm.com/downloads)).
  * Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.
  * [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) 2.0.4 veya sonraki sürümü. Bu uygulama Windows, Mac ve Linux’ta kullanılabilir.
  * [.NET Core](https://www.microsoft.com/net/download/dotnet-core/2.1)

## <a name="what-is-managed-service-identity-and-how-does-it-work"></a>Yönetilen Hizmet Kimliği nedir ve nasıl çalışır?
 Daha fazla ilerlemeden önce, MSI'yi anlayalım. Azure Key Vault kimlik bilgilerini güvenle depolayabilir ve bunlar kodunuzda yer almaz, ama bunları almak için Azure Key Vault'ta kimliğinizi doğrulamanız gerekir. Key Vault'ta kimlik doğrulaması yapmak için kimlik bilgilerine ihtiyacınız vardır! Klasik bir önyükleme sorunu. Azure'un ve Azure AD'nin büyüsü sayesinde MSI işleri başlatmayı çok basitleştiren bir “önyükleme kimliği” sağlar.

Şöyle çalışır! Sanal Makineler, App Service veya İşlevler gibi bir Azure hizmeti için MSI'yi etkinleştirdiğinizde, Azure hizmetin örneği için Azure Active Directory'de bir [Hizmet Sorumlusu](key-vault-whatis.md#basic-concepts) oluşturur ve Hizmet Sorumlusunun kimlik bilgilerini hizmetin örneğine ekler. 

![MSI](media/MSI.png)

Ardından, kodunuz erişim belirtecini almak için Azure kaynağında sağlanan bir yerel meta veri hizmetini çağırır.
Kodunuz yerel MSI_ENDPOINT'ten aldığı erişim belirtecini kullanarak Azure Key Vault hizmetinde kimlik doğrulaması yapar. 

Şimdi öğreticiye başlayalım.

## <a name="log-in-to-azure"></a>Azure'da oturum açma

Azure CLI'yi kullanarak Azure'da oturum açmak için, şunları girin:

```azurecli
az login
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create](/cli/azure/group#az-group-create) komutunu kullanarak bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

Kaynak grubu adı seçin ve yer tutucuyu doldurun.
Aşağıdaki örnekte Batı ABD konumunda bir kaynak grubu oluşturulur:

```azurecli
# To list locations: az account list-locations --output table
az group create --name "<YourResourceGroupName>" --location "West US"
```

Az önce oluşturduğunuz kaynak grubu bu makale boyunca kullanılır.

## <a name="create-a-key-vault"></a>Bir anahtar kasası oluşturma

Ardından, önceki adımda oluşturduğunuz kaynak grubunda bir anahtar kasası oluşturursunuz. Şu bilgileri belirtin:

* Anahtar kasası adı: Adı 3-24 karakterden oluşan bir dize olmalı ve yalnızca (0-9, a-z, A-Z ve -) karakterlerini içermelidir.
* Kaynak grubu adı.
* Konum: **West US**.

```azurecli
az keyvault create --name "<YourKeyVaultName>" --resource-group "<YourResourceGroupName>" --location "West US"
```

Bu noktada Azure hesabınız, bu yeni anahtar kasasında herhangi bir işlemi gerçekleştirmeye yetkili olan tek hesaptır.

## <a name="add-a-secret-to-the-key-vault"></a>Anahtar kasasına gizli dizi ekleme

Bunun nasıl çalıştığını göstermemize yardımcı olması için bir gizli dizi ekliyoruz. Güvenle korumanız ancak uygulamanıza da sağlamanız gereken bir SQL bağlantı dizesini veya başka bir bilgiyi depoluyor olabilirsiniz.

Anahtar kasasında **AppSecret** adlı bir gizli dizi oluşturmak için aşağıdaki komutları yazın. Bu gizli dizi **MySecret** değerini depolar.

```azurecli
az keyvault secret set --vault-name "<YourKeyVaultName>" --name "AppSecret" --value "MySecret"
```

Gizli dizi içindeki değeri düz metin olarak görüntülemek için:

```azurecli
az keyvault secret show --name "AppSecret" --vault-name "<YourKeyVaultName>"
```

Bu komut, URI dahil olmak üzere gizli dizi bilgilerini gösterir. Bu adımları tamamladıktan sonra, bir anahtar kasasında gizli dizi URI'sine sahip olmanız gerekir. Bu bilgileri not alın. Daha sonraki bir adımda gerekecektir.

## <a name="create-a-net-core-web-app"></a>.NET Core Web Uygulaması oluşturma

Bu [öğreticiyi](../app-service/app-service-web-get-started-dotnet.md) izleyerek bir .NET Core Web Uygulaması oluşturun ve bunu Azure'da **yayımlayın** **VEYA** aşağıdaki videoyu izleyin
> [!VIDEO https://www.youtube.com/embed/EdiiEH7P-bU]

## <a name="open-and-edit-the-solution"></a>Çözümü açma ve düzenleme

1. Sayfalar > About.cshtml.cs dosyasına gidin.
2. Şu 2 Nuget paketlerini yükleyin
    - [AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication)
    - [KeyVault](https://www.nuget.org/packages/Microsoft.Azure.KeyVault)
3. Aşağıdakini About.cshtml.cs dosyasına içeri aktarın

    ```
    using Microsoft.Azure.KeyVault;
    using Microsoft.Azure.KeyVault.Models;
    using Microsoft.Azure.Services.AppAuthentication;
    ```
4. AboutModel sınıfında kodunuz aşağıdaki gibi görünmelidir
    ```
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

Visual Studio 2017 ana menüsünde **Hata Ayıkla** > **Hata ayıklamayla/ayıklamadan başlat** seçeneğini belirleyin. Tarayıcı görüntülendiğinde **Hakkında** sayfasına gidin. **AppSecret** değeri görüntülenir.

## <a name="enable-a-managed-identity-for-the-web-app"></a>Web uygulaması için yönetilen kimliği etkinleştirme

Azure Key Vault kimlik bilgilerini ve diğer anahtarlarla gizli dizileri güvenle depolamak için bir yol sağlar, ama bunları alabilmek için kodunuzun Key Vault'ta kimlik doğrulaması yapması gerekir. [Azure kaynakları için yönetilen kimliklere genel bakış](../active-directory/managed-identities-azure-resources/overview.md), Azure hizmetlerine Azure Active Directory (Azure AD) üzerinde otomatik olarak yönetilen bir kimlik vererek bu soruna daha basit bir çözüm getirir. Bu kimliği kullanarak, Key Vault da dahil olmak üzere Azure AD kimlik doğrulamasını destekleyen tüm hizmetlerde kodunuzda kimlik bilgileri bulunmasına gerek kalmadan kimlik doğrulaması yapabilirsiniz.

1. Azure CLI'ye dönün.
2. Bu uygulamanın kimliğini oluşturmak için assign-identity komutunu çalıştırın: 

   ```azurecli
   az webapp identity assign --name "<YourAppName>" --resource-group "<YourResourceGroupName>"
   ```
   <YourAppName> değerini Azure'da yayımlanan uygulamanın adıyla değiştirmeyi lütfen unutmayın; örneğin, yayımlanan uygulamanızın adı MyAwesomeapp.azurewebsites.net ise <YourAppName> değerini MyAwesomeapp ile değiştirin
 
 Yukarıdaki komutun çıkışı şuna benzer. Uygulamayı Azure'a yayımladığınızda PrincipalId değerini not alın. Şu biçimde olmalıdır:
   ```
   {
     "principalId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
     "tenantId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
     "type": "SystemAssigned"
   }
  ```
>[!NOTE]
>Bu yordamdaki komut, [portala](https://portal.azure.com) gidip web uygulaması özelliklerinde **Kimlik/Sistem tarafından atanan** ayarını **Açık** duruma getirmekle eşdeğerdir.

## <a name="assign-permissions-to-your-application-to-read-secrets-from-key-vault"></a>Key Vault gizli dizilerini okumak için uygulamanıza izin atama
        
Ardından anahtar kasanızın adını ve **PrincipalId** değerini kullanarak bu komutu çalıştırın:

```azurecli

az keyvault set-policy --name '<YourKeyVaultName>' --object-id <PrincipalId> --secret-permissions get list

```

## <a name="publish-the-web-application-to-azure"></a>Web uygulamasını Azure’a yayımlama

Canlı web uygulaması olarak görüntülemek ve gizli dizi değerini getirebildiğinizi görmek için bu uygulamayı Azure'da bir kez daha yayımlayın.

1. Visual Studio'da **key-vault-dotnet-core-quickstart** projesini seçin.
2. **Yayımla** > **Başlat** seçeneğini belirleyin.
3. **Oluştur**’u seçin.

Yukarıdaki komutta, App Service Kimliğine (MSI), Key Vault’unuzda **alma** ve **liste** işlemlerini yapma izni veriyorsunuz. <br />
Uygulamayı çalıştırdığınızda gizli dizi değerinizin alındığını görürsünüz. 

Hepsi bu kadar. Artık .NET'te başarılı bir şekilde gizli dizilerini Key Vault'ta depolayan ve buradan getiren bir Web Uygulaması oluşturdunuz.
