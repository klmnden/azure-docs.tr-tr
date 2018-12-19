---
title: Öğretici - Azure Key Vault ile Azure Windows sanal makinesine .NET kullanma | Microsoft Docs
description: Öğretici Anahtar Kasasından gizli dizi okumak için bir ASP.NET Core uygulaması yapılandırma
services: key-vault
documentationcenter: ''
author: prashanthyv
manager: rajvijan
ms.assetid: 0e57f5c7-6f5a-46b7-a18a-043da8ca0d83
ms.service: key-vault
ms.workload: key-vault
ms.topic: tutorial
ms.date: 09/05/2018
ms.author: pryerram
ms.custom: mvc
ms.openlocfilehash: 8ebc07f68e2d26126d0bd387f32204255404d6e1
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/19/2018
ms.locfileid: "53605322"
---
# <a name="tutorial-how-to-use-azure-key-vault-with-azure-windows-virtual-machine-in-net"></a>Öğretici: Azure Key Vault ile Azure Windows sanal makinesine .NET kullanma

Azure Key Vault uygulamalarınıza, hizmetlerinize ve BT kaynaklarınıza erişmek için gereken API Anahtarları, Veritabanı Bağlantısı dizeleri gibi gizli dizileri korumanıza yardımcı olur.

Bu öğreticide, Azure kaynakları için yönetilen kimliklerle bilgilerini Azure Key Vault'tan okumak için bir konsol uygulaması almak için gerekli adımları izleyin. Şunları yapmayı öğreneceksiniz:

> [!div class="checklist"]
> * Bir anahtar kasası oluşturma.
> * Anahtar kasasına bir gizli dizi depolama.
> * Anahtar kasasından bir gizli dizi alma.
> * Bir Azure sanal makinesi oluşturun.
> * Etkinleştirme bir [yönetilen kimliği](../active-directory/managed-identities-azure-resources/overview.md) sanal makine için.
> * Anahtar kasasından verileri okumak konsol uygulaması için gerekli izinleri verin.
> * Key Vault'tan gizli dizilerini alma

İlerlemeden önce lütfen [temel kavramları](key-vault-whatis.md#basic-concepts) okuyun.

## <a name="prerequisites"></a>Önkoşullar
* Tüm platformlar:
  * Git ([indir](https://git-scm.com/downloads)).
  * Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.
  * [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) 2.0.4 veya sonraki sürümü. Bu uygulama Windows, Mac ve Linux’ta kullanılabilir.

Bu öğreticide, yönetilen hizmet kimliğini kullanma hale getirir.

## <a name="what-is-managed-service-identity-and-how-does-it-work"></a>Yönetilen Hizmet Kimliği nedir ve nasıl çalışır?
Daha fazla ilerlemeden önce, MSI'yi anlayalım. Azure Key Vault kimlik bilgilerini güvenle depolayabilir ve bunlar kodunuzda yer almaz, ama bunları almak için Azure Key Vault'ta kimliğinizi doğrulamanız gerekir. Key Vault'ta kimlik doğrulaması yapmak için kimlik bilgilerine ihtiyacınız vardır! Klasik bir önyükleme sorunu. Azure'un ve Azure AD'nin büyüsü sayesinde MSI işleri başlatmayı çok basitleştiren bir “önyükleme kimliği” sağlar.

Şöyle çalışır! Sanal Makineler, App Service veya İşlevler gibi bir Azure hizmeti için MSI'yi etkinleştirdiğinizde, Azure hizmetin örneği için Azure Active Directory'de bir [Hizmet Sorumlusu](key-vault-whatis.md#basic-concepts) oluşturur ve Hizmet Sorumlusunun kimlik bilgilerini hizmetin örneğine ekler. 

![MSI](media/MSI.png)

Ardından, kodunuzu bir erişim belirteci almak için Azure kaynak üzerinde kullanılabilir olan bir yerel meta veri hizmeti çağırır.
Kodunuz yerel MSI_ENDPOINT'ten aldığı erişim belirtecini kullanarak Azure Key Vault hizmetinde kimlik doğrulaması yapar. 

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

* Anahtar kasası adı: Ad 3-24 karakter dizesi olmalı ve yalnızca içermelidir (0-9, a-z, A-Z ve -).
* Kaynak grubu adı.
* Konum: **Batı ABD**.

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

## <a name="create-a-virtual-machine"></a>Bir Sanal Makine Oluşturun
İzleyin aşağıdaki bağlantıları bir Windows sanal makine oluşturmak için

[Azure CLI](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-cli) 

[PowerShell](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-powershell)

[Portal](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal)

## <a name="assign-identity-to-virtual-machine"></a>Sanal makineye kimlik atama
Bu adımda Azure CLI aşağıdaki komutu çalıştırarak sanal makineye atanan kimliği bir sistem oluşturuyoruz

```
az vm identity assign --name <NameOfYourVirtualMachine> --resource-group <YourResourceGroupName>
```

Lütfen aşağıda gösterilen systemAssignedIdentity unutmayın. Yukarıdaki komut çıktısı olacaktır 

```
{
  "systemAssignedIdentity": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "userAssignedIdentities": {}
}
```

## <a name="give-vm-identity-permission-to-key-vault"></a>Anahtar Kasası'na VM kimliği izin ver
Şimdi biz yukarıdaki Key Vault kimlik izni aşağıdaki komutu çalıştırarak oluşturduğunuz verebilirsiniz

```
az keyvault set-policy --name '<YourKeyVaultName>' --object-id <VMSystemAssignedIdentity> --secret-permissions get list
```

## <a name="login-to-the-virtual-machine"></a>Sanal makineye oturum açma

Bu takip edebilirsiniz [Öğreticisi](https://docs.microsoft.com/azure/virtual-machines/windows/connect-logon)

## <a name="install-net-core"></a>.NET Core'u yükleyin

Bu adımları uygulayarak .NET Core yükleyin [makale](https://www.microsoft.com/net/download)

## <a name="create-and-run-sample-dot-net-app"></a>Oluşturma ve örnek Dot Net uygulaması çalıştırma

Bir komut istemi açın

Çalıştırarak aşağıdaki "Hello World" konsola yazdırılmasını gördüğünüz komutları

```
dotnet new console -o helloworldapp
cd helloworldapp
dotnet run
```

## <a name="edit-console-app"></a>Konsol uygulamasını Düzenle
Program.cs dosyasını açın ve bu paketleri ekleyin
```
using System;
using System.IO;
using System.Net;
using System.Text;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
```
Ardından sınıfı içeren dosyaya değiştirin kod aşağıda. Bu bir 2 adımlı bir işlemdir. 
1. Hangi inturn Azure Active Directory'den bir belirteç getirir VM'deki yerel MSI uç noktasından bir belirteç getirilemedi
2. Anahtar Kasası'na belirtecin geçip ve gizli anahtarı getirilemedi 

```
 class Program
    {
        static void Main(string[] args)
        {
            // Step 1: Get a token from local (URI) Managed Service Identity endpoint which in turn fetches it from Azure Active Directory
            var token = GetToken();

            // Step 2: Fetch the secret value from Key Vault
            System.Console.WriteLine(FetchSecretValueFromKeyVault(token));
        }

        static string GetToken()
        {
            WebRequest request = WebRequest.Create("http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fvault.azure.net");
            request.Headers.Add("Metadata", "true");
            WebResponse response = request.GetResponse();
            return ParseWebResponse(response, "access_token");
        }

        static string FetchSecretValueFromKeyVault(string token)
        {
            WebRequest kvRequest = WebRequest.Create("https://prashanthwinvmvault.vault.azure.net/secrets/RandomSecret?api-version=2016-10-01");
            kvRequest.Headers.Add("Authorization", "Bearer "+  token);
            WebResponse kvResponse = kvRequest.GetResponse();
            return ParseWebResponse(kvResponse, "value");
        }

        private static string ParseWebResponse(WebResponse response, string tokenName)
        {
            string token = String.Empty;
            using (Stream stream = response.GetResponseStream())
            {
                StreamReader reader = new StreamReader(stream, Encoding.UTF8);
                String responseString = reader.ReadToEnd();

                JObject joResponse = JObject.Parse(responseString);    
                JValue ojObject = (JValue)joResponse[tokenName];             
                token = ojObject.Value.ToString();
            }
            return token;
        }
    }
```


Yukarıdaki kod, Azure Key Vault içinde bir Azure Linux sanal makine ile işlem yapma gösterir. 



## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure anahtar kasası REST API'si](https://docs.microsoft.com/rest/api/keyvault/)
