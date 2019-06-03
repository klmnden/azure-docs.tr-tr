---
title: Öğretici - kullanma Linux sanal makinesi ve bir ASP.NET uygulaması gizli dizileri Azure Key Vault'ta depolamak için konsol | Microsoft Docs
description: Bu öğreticide, Azure Key vault'tan bir gizli dizi okumak için bir ASP.NET Core uygulaması yapılandırma konusunda bilgi edinin.
services: key-vault
author: msmbaldwin
manager: rajvijan
ms.service: key-vault
ms.topic: tutorial
ms.date: 12/21/2018
ms.author: pryerram
ms.custom: mvc
ms.openlocfilehash: 576ce0abc15a646738fea57dfabf43c889635a4b
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66416941"
---
# <a name="tutorial-use-a-linux-vm-and-a-net-app-to-store-secrets-in-azure-key-vault"></a>Öğretici: Gizli dizileri Azure Key Vault'ta depolamak için bir Linux VM ile bir .NET uygulaması'nı kullanın

Azure Key Vault API anahtarları ve uygulamaları, hizmetlerinizi ve BT kaynaklarına erişmek için gereken veritabanı bağlantı dizeleri gibi gizli dizileri korumanıza yardımcı olur.

Bu öğreticide, Azure kaynakları için yönetilen kimliklerle bilgilerini Azure Key Vault'tan okumak için bir .NET konsol uygulaması ayarlayın. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Bir anahtar kasası oluşturma
> * Anahtar Kasası'nda bir gizli dizi Store
> * Bir Azure Linux sanal makinesi oluşturma
> * Etkinleştirme bir [yönetilen kimliği](../active-directory/managed-identities-azure-resources/overview.md) sanal makine için
> * Konsol uygulamasını Key Vault'tan verileri okumak için gerekli izinleri verin
> * Key Vault'tan bir gizli dizi alma

Size daha fazla ayrıntıya önce okuyun [anahtar kasası temel kavramları](key-vault-whatis.md#basic-concepts).

## <a name="prerequisites"></a>Önkoşullar

* [Git](https://git-scm.com/downloads).
* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.
* [Azure CLI 2.0 veya sonraki](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) veya Azure Cloud Shell.

[!INCLUDE [Azure Cloud Shell](../../includes/cloud-shell-try-it.md)]

## <a name="understand-managed-service-identity"></a>Yönetilen hizmet kimliği anlama

Azure Key Vault kimlik bilgilerini güvenle depolayabilir ve bunlar kodunuzda yer almaz, ama bunları almak için Azure Key Vault'ta kimliğinizi doğrulamanız gerekir. Ancak, anahtar Kasası'na kimlik doğrulaması için bir kimlik bilgisi gerekir. Klasik bir önyükleme sorun var. Azure ve Azure Active Directory (Azure AD) ile yönetilen hizmet kimliği (MSI) işlemleri başlatmak çok daha kolay getirir önyükleme bir kimlik sağlayabilirsiniz.

Sanal makineler, App Service ve işlevler gibi bir Azure hizmeti için MSI etkinleştirdiğinizde, Azure hizmet sorumlusu hizmet örneği için Azure Active Directory'de oluşturur. Hizmet sorumlusu kimlik bilgileri hizmet örneğine ekler.

![MSI](media/MSI.png)

Ardından, kodunuzu bir erişim belirteci almak için Azure kaynak üzerinde kullanılabilir olan bir yerel meta veri hizmeti çağırır.
Kodunuz yerel MSI_ENDPOINT'ten aldığı erişim belirtecini kullanarak Azure Key Vault hizmetinde kimlik doğrulaması yapar.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure CLI'yi kullanarak Azure'da oturum açmanız için şunu girin:

```azurecli-interactive
az login
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Kullanarak bir kaynak grubu oluşturma `az group create` komutu. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

Batı ABD konumunda bir kaynak grubu oluşturun. Kaynak grubunuz için bir ad seçin ve Değiştir `YourResourceGroupName` aşağıdaki örnekte:

```azurecli-interactive
# To list locations: az account list-locations --output table
az group create --name "<YourResourceGroupName>" --location "West US"
```

Bu kaynak grubu öğretici boyunca kullanırsınız.

## <a name="create-a-key-vault"></a>Bir anahtar kasası oluşturma

Ardından, kaynak grubunuzda bir anahtar kasası oluşturun. Şu bilgileri belirtin:

* Anahtar kasası adı: bir dizenin yalnızca sayı, harf ve kısa çizgi içerebilir ve 3-24 karakter (0-9, a-z, A-Z ve \- ).
* Kaynak grubu adı
* Konum: **Batı ABD**

```azurecli-interactive
az keyvault create --name "<YourKeyVaultName>" --resource-group "<YourResourceGroupName>" --location "West US"
```

Bu noktada, Azure hesabınız yalnızca bu yeni kasa üzerinde herhangi bir işlemi gerçekleştirme yetkisi.

## <a name="add-a-secret-to-the-key-vault"></a>Anahtar kasasına gizli dizi ekleme

Şimdi bir gizli dizi ekleyin. Bir gerçek dünya senaryosunda, bir SQL bağlantı dizesi veya güvenli bir şekilde devam edin, ama uygulamanızı kullanılabilir hale getirmek için ihtiyacınız olan tüm diğer bilgileri depoluyor olabilir.

Bu öğreticide, anahtar kasasında gizli dizi oluşturmak için aşağıdaki komutları yazın. Gizli dizi olarak adlandırılan **AppSecret** ve değeri **ettiyseniz**.

```azurecli
az keyvault secret set --vault-name "<YourKeyVaultName>" --name "AppSecret" --value "MySecret"
```

## <a name="create-a-linux-virtual-machine"></a>Linux sanal makinesi oluşturma

İle bir VM `az vm create` komutu.

Aşağıdaki örnek, **myVM** adlı bir VM oluşturur ve **azureuser** adlı bir kullanıcı hesabı ekler. `--generate-ssh-keys` Parametresi bize SSH otomatik olarak oluşturmak için kullanılan anahtar ve varsayılan anahtar konumunda yerleştirin ( **~/.ssh**). Bunun yerine belirli bir anahtar kümesini kullanmak için `--ssh-key-value` seçeneğini kullanın.

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --image UbuntuLTS \
  --admin-username azureuser \
  --generate-ssh-keys
```

VM’yi ve destekleyici kaynakları oluşturmak birkaç dakika sürer. Aşağıdaki örnek çıktıda gösterildiği VM oluşturma işlemi başarılı oldu.

```azurecli
{
  "fqdns": "",
  "id": "/subscriptions/<guid>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "westus",
  "macAddress": "00-00-00-00-00-00",
  "powerState": "VM running",
  "privateIpAddress": "XX.XX.XX.XX",
  "publicIpAddress": "XX.XX.XXX.XXX",
  "resourceGroup": "myResourceGroup"
}
```

Not, `publicIpAddress` VM'nizi çıktıda. Bu adres, sonraki adımlarda VM'ye erişmek için kullanacaksınız.

## <a name="assign-an-identity-to-the-vm"></a>Bir kimlik VM'ye atayın

Aşağıdaki komutu çalıştırarak sanal makine için sistem tarafından atanan bir kimlik oluşturun:

```azurecli
az vm identity assign --name <NameOfYourVirtualMachine> --resource-group <YourResourceGroupName>
```

Komut çıktısı olmalıdır:

```azurecli
{
  "systemAssignedIdentity": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "userAssignedIdentities": {}
}
```

Not `systemAssignedIdentity`. Sonraki adımda bunu kullanın.

## <a name="give-the-vm-identity-permission-to-key-vault"></a>Anahtar Kasası'na VM kimlik izin ver

Artık oluşturduğunuz kimliğine anahtar kasası izin verebilirsiniz. Şu komutu çalıştırın:

```azurecli
az keyvault set-policy --name '<YourKeyVaultName>' --object-id <VMSystemAssignedIdentity> --secret-permissions get list
```

## <a name="log-in-to-the-vm"></a>VM'de oturum açın

Bir terminal kullanarak sanal makineye oturum açın.

```terminal
ssh azureuser@<PublicIpAddress>
```

## <a name="install-net-core-on-linux"></a>Linux üzerinde .NET Core yükleyin

Linux sanal makinenizde:

Microsoft ürün anahtarı güvenilir aşağıdaki komutları çalıştırarak kaydedin:

   ```console
   curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
   sudo mv microsoft.gpg /etc/apt/trusted.gpg.d/microsoft.gpg
   ```

Bağlı işletim sistemi akışı istediğiniz sürümü konak paketini ayarlayın:

```console
   # Ubuntu 17.10
   sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-ubuntu-artful-prod artful main" > /etc/apt/sources.list.d/dotnetdev.list'
   sudo apt-get update
   
   # Ubuntu 17.04
   sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-ubuntu-zesty-prod zesty main" > /etc/apt/sources.list.d/dotnetdev.list'
   sudo apt-get update
   
   # Ubuntu 16.04 / Linux Mint 18
   sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-ubuntu-xenial-prod xenial main" > /etc/apt/sources.list.d/dotnetdev.list'
   sudo apt-get update
   
   # Ubuntu 14.04 / Linux Mint 17
   sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-ubuntu-trusty-prod trusty main" > /etc/apt/sources.list.d/dotnetdev.list'
   sudo apt-get update
```

.NET yükleme ve sürümünü denetleyin:

   ```console
   sudo apt-get install dotnet-sdk-2.1.4
   dotnet --version
   ```

## <a name="create-and-run-a-sample-net-app"></a>Oluşturma ve örnek bir .NET uygulaması çalıştırma

Aşağıdaki komutları çalıştırın. "Hello World" konsola yazdırılmasını görmeniz gerekir.

```console
dotnet new console -o helloworldapp
cd helloworldapp
dotnet run
```

## <a name="edit-the-console-app-to-fetch-your-secret"></a>Gizli getirilecek konsol uygulamasını Düzenle

Program.cs dosyasını açın ve bu paketleri ekleyin:

   ```csharp
   using System;
   using System.IO;
   using System.Net;
   using System.Text;
   using Newtonsoft.Json;
   using Newtonsoft.Json.Linq;
   ```

Bu anahtar kasasında gizli dizi erişim sağlamak için sınıf dosyası değiştirmek için iki adımlı bir işlemdir.

1. Bir belirteç sırayla Azure Active Directory'den bir belirteç getiren VM'deki yerel MSI uç noktasından getirin.
1. Anahtar Kasası'na belirtecin geçip ve gizli anahtarı getirilemedi.

   Aşağıdaki kod içeren sınıf dosyasını düzenleyin:

   ```csharp
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

Artık bir Azure Linux sanal makinesinde çalışan bir .NET uygulamasında Azure Key Vault ile işlemleri gerçekleştirmeyi öğrendiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık ihtiyacınız kalmadığında kaynak grubunu, sanal makine ve tüm ilgili kaynakları silin. Bunu yapmak için kaynak grubunu seçin ve sanal makine için seçin **Sil**.

Anahtar kasasını kullanarak silme `az keyvault delete` komutu:

```azurecli-interactive
az keyvault delete --name
                   [--resource group]
                   [--subscription]
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure anahtar kasası REST API'si](https://docs.microsoft.com/rest/api/keyvault/)
