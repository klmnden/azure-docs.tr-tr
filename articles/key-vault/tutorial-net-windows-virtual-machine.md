---
title: Öğretici - bir Windows sanal makinede .NET ile Azure anahtar kasası | Microsoft Docs
description: Bu öğreticide, anahtar kasasından gizli dizi okumak için bir ASP.NET core uygulaması yapılandırın.
services: key-vault
author: msmbaldwin
manager: rajvijan
ms.service: key-vault
ms.topic: tutorial
ms.date: 01/02/2019
ms.author: pryerram
ms.custom: mvc
ms.openlocfilehash: 4ae02a494949e92ad8e59cd35e46b6ce246ae7cc
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67115016"
---
# <a name="tutorial-use-azure-key-vault-with-a-windows-virtual-machine-in-net"></a>Öğretici: Bir Windows sanal makinede .NET ile Azure anahtar kasası

Azure Key Vault API anahtarları, uygulamalar, hizmetlerinizi ve BT kaynaklarına erişmek için gereken veritabanı bağlantı dizeleri gibi gizli dizileri korumanıza yardımcı olur.

Bu öğreticide, Azure Key Vault'tan bilgileri okumak için bir konsol uygulaması alma konusunda bilgi edinin. Bunu yapmak için Azure kaynakları için yönetilen bir kimlik kullanın. 

Öğretici şunların nasıl yapıldığını göstermektedir:

> [!div class="checklist"]
> * Bir kaynak grubu oluşturun.
> * Bir anahtar kasası oluşturma.
> * Gizli anahtar Kasası'na ekleyin.
> * Anahtar kasasından bir gizli dizi alma.
> * Bir Azure sanal makinesi oluşturun.
> * Etkinleştirme bir [yönetilen kimliği](../active-directory/managed-identities-azure-resources/overview.md) sanal makine için.
> * VM kimliği için izinleri atayın.

Başlamadan önce okumanız [Key Vault temel kavramlarını](key-vault-whatis.md#basic-concepts). 

Azure aboneliğiniz yoksa, oluşturun bir [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="prerequisites"></a>Önkoşullar

Windows, Mac ve Linux için:
  * [Git](https://git-scm.com/downloads)
  * Bu öğretici, Azure CLI'yi yerel olarak çalıştırmanızı gerektirir. Sonraki bir sürümünün yüklü veya Azure CLI 2.0.4 sürüm olmalıdır. Sürümü bulmak için `az --version` komutunu çalıştırın. CLI’yı yüklemeniz veya yükseltmeniz gerekiyorsa bkz. [Azure CLI 2.0’ı yükleme](https://review.docs.microsoft.com/cli/azure/install-azure-cli).

## <a name="about-managed-service-identity"></a>Yönetilen Hizmet Kimliği hakkında

Kodunuzda görüntülenmez için azure Key Vault kimlik bilgilerini güvenli bir şekilde, depolar. Ancak, Azure Key Vault'a anahtarlarınızı almak için kimlik doğrulaması gerekir. Anahtar Kasası'na kimlik doğrulaması için bir kimlik bilgisi gerekir. Bu, klasik bir önyükleme ikilemle olur. Yönetilen hizmet kimliği (MSI) sağlayarak bu sorunu çözer bir _bootstrap kimlik_ , bu süreci kolaylaştırır.

Azure sanal makineler, Azure App Service veya Azure işlevleri gibi bir Azure hizmeti için MSI'yı etkinleştirdiğinizde, Azure oluşturur bir [hizmet sorumlusu](key-vault-whatis.md#basic-concepts). MSI, Azure Active Directory'de (Azure AD) hizmet örneği için bunu yapar ve bu örneğe hizmet sorumlusu kimlik bilgileri ekler. 

![MSI](media/MSI.png)

Ardından, bir erişim belirteci almak için kodunuzu Azure kaynak üzerinde kullanılabilir olan bir yerel meta veri hizmeti çağırır. Bir Azure anahtar kasası hizmetinde kimlik doğrulaması için kodunuzu yerel MSI uç noktasından alır ve bir erişim belirteci kullanır. 

## <a name="create-resources-and-assign-permissions"></a>Kaynakları oluşturma ve izinleri atama

Bazı kaynaklar oluşturmak için ihtiyacınız kodlama başlamadan önce bir gizli dizi, anahtar kasasını yerleştirmek ve izinleri atayın.

### <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure CLI'yi kullanarak Azure'da oturum açmanız için şunu girin:

```azurecli
az login
```

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. [az group create](/cli/azure/group#az-group-create) komutunu kullanarak bir kaynak grubu oluşturun. 

Bu örnek, Batı ABD konumunda bir kaynak grubu oluşturur:

```azurecli
# To list locations: az account list-locations --output table
az group create --name "<YourResourceGroupName>" --location "West US"
```

Yeni oluşturulan kaynak grubunuz, Bu öğretici boyunca kullanılır.

### <a name="create-a-key-vault-and-populate-it-with-a-secret"></a>Key vault oluşturma ve gizli dizi ile doldurma

Anahtar kasası kaynak grubunuzda sağlayarak oluşturma [az keyvault oluşturun](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-create) komutunu aşağıdaki bilgilerle:

* Anahtar kasası adı: bir dizenin yalnızca rakam (0-9) içerebilir ve 3-24 karakter harf (a-z, A-Z) ve tire (-)
* Kaynak grubu adı
* Konum: **Batı ABD**

```azurecli
az keyvault create --name "<YourKeyVaultName>" --resource-group "<YourResourceGroupName>" --location "West US"
```
Bu noktada, Azure hesabınızı kullanarak bu yeni bir anahtar kasası işlemleri gerçekleştirmek için yetkili tek hesaptır.

Şimdi bir gizli dizi kullanarak anahtar kasası ekleyin [az keyvault secret set](/cli/azure/keyvault/secret?view=azure-cli-latest#az-keyvault-secret-set) komutu


Anahtar Kasası'nda bir gizli dizi oluşturmak için çağrılan **AppSecret**, aşağıdaki komutu girin:

```azurecli
az keyvault secret set --vault-name "<YourKeyVaultName>" --name "AppSecret" --value "MySecret"
```

Bu gizli dizinin değerini depolar **ettiyseniz**.

### <a name="create-a-virtual-machine"></a>Sanal makine oluşturma
Aşağıdaki yöntemlerden birini kullanarak bir sanal makine oluşturun:

* [Azure CLI](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-cli)
* [PowerShell](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-powershell)
* [Azure portalı](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal)

### <a name="assign-an-identity-to-the-vm"></a>Bir kimlik VM'ye atayın
Sanal makine ile bir sistem tarafından atanan kimliği oluşturma [az vm kimliği atamak](/cli/azure/vm/identity?view=azure-cli-latest#az-vm-identity-assign) komutu:

```azurecli
az vm identity assign --name <NameOfYourVirtualMachine> --resource-group <YourResourceGroupName>
```

Aşağıdaki kodda gösterilen sistem tarafından atanan kimliğini not edin. Yukarıdaki komut çıktısı şu şekilde olur: 

```azurecli
{
  "systemAssignedIdentity": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "userAssignedIdentities": {}
}
```

### <a name="assign-permissions-to-the-vm-identity"></a>VM kimliği için izinler atama
Anahtarınızı daha önce oluşturulan kimlik izni kasa ile Ata [az keyvault set-policy](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-set-policy) komutu:

```azurecli
az keyvault set-policy --name '<YourKeyVaultName>' --object-id <VMSystemAssignedIdentity> --secret-permissions get list
```

### <a name="sign-in-to-the-virtual-machine"></a>Sanal makinede oturum açın

Sanal makineye oturum açmak için yönergeleri izleyin. [bağlanma ve oturum açma Windows çalıştıran bir Azure sanal makinesine](https://docs.microsoft.com/azure/virtual-machines/windows/connect-logon).

## <a name="set-up-the-console-app"></a>Konsol uygulamasını ayarlama

Bir konsol uygulaması oluşturma ve kullanarak gerekli paketleri yükleyin `dotnet` komutu.

### <a name="install-net-core"></a>.NET Core'u yükleyin

.NET Core yüklemek için Git [.NET indirir](https://www.microsoft.com/net/download) sayfası.

### <a name="create-and-run-a-sample-net-app"></a>Oluşturma ve örnek bir .NET uygulaması çalıştırma

Bir komut istemi açın.

Aşağıdaki komutları çalıştırarak konsolda "Hello World" yazdırabilirsiniz:

```console
dotnet new console -o helloworldapp
cd helloworldapp
dotnet run
```

### <a name="install-the-packages"></a>Paketleri yükleme

 Konsol penceresinden, bu hızlı başlangıç için gereken .NET paketleri yükleyin:

 ```console
dotnet add package System.IO;
dotnet add package System.Net;
dotnet add package System.Text;
dotnet add package Newtonsoft.Json;
dotnet add package Newtonsoft.Json.Linq;
```

## <a name="edit-the-console-app"></a>Konsol uygulamasını Düzenle

Açık *Program.cs* dosya ve bu paketleri ekleyin:

```csharp
using System;
using System.IO;
using System.Net;
using System.Text;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
```

Aşağıdaki iki adımlı işlem kodu içermesi için sınıf dosyasını düzenleyin:

1. Bir belirteç VM'de yerel MSI uç noktasından getirin. Bunun yapılması ayrıca Azure AD'den bir belirtecini getirir.
1. Anahtar kasanızı belirtecin geçip ve sonra gizli anahtarı getirilemedi. 

```csharp
 class Program
    {
        static void Main(string[] args)
        {
            // Step 1: Get a token from the local (URI) Managed Service Identity endpoint, which in turn fetches it from Azure AD
            var token = GetToken();

            // Step 2: Fetch the secret value from your key vault
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
            WebRequest kvRequest = WebRequest.Create("https://<YourVaultName>.vault.azure.net/secrets/<YourSecretName>?api-version=2016-10-01");
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

Yukarıdaki kod, bir Windows sanal makinesinde Azure Key Vault ile işlemleri yapıp gösterilir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında sanal makine ve anahtar kasanıza silin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure anahtar kasası REST API'si](https://docs.microsoft.com/rest/api/keyvault/)
