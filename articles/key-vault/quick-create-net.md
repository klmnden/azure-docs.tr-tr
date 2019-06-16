---
title: "Hızlı Başlangıç: .NET web uygulaması - Azure Key Vault kullanarak Azure Key Vault'tan bir gizli dizi alma ve ayarlama | Microsoft Docs"
description: Bu hızlı başlangıçta, ayarlayın ve .NET web uygulaması kullanarak Azure Key Vault'tan bir gizli dizi alma
services: key-vault
author: msmbaldwin
manager: sumedhb
ms.service: key-vault
ms.topic: quickstart
ms.date: 01/02/2019
ms.author: barclayn
ms.custom: mvc
ms.openlocfilehash: 4f9fff41e4b9043c271d656583fb8b9a11ff3a7a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67052793"
---
# <a name="quickstart-set-and-retrieve-a-secret-from-azure-key-vault-by-using-a-net-web-app"></a>Hızlı Başlangıç: .NET web uygulaması kullanarak Azure Key Vault'tan bir gizli dizi alma ve ayarlama

Bu hızlı başlangıçta, Azure kaynakları için yönetilen kimliklerle bilgilerini Azure Key Vault'tan okumak için bir Azure web uygulaması alma adımlarını izleyin. Anahtar kasasını kullanarak, bilgilerin güvende tutulmasına yardımcı olur. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

* Bir anahtar kasası oluşturma.
* Anahtar kasasına bir gizli dizi depolama.
* Anahtar kasasından bir gizli dizi alma.
* Azure web uygulaması oluşturma.
* Web uygulaması için [yönetilen hizmet kimliğini](../active-directory/managed-identities-azure-resources/overview.md) etkinleştirin.
* Web uygulamasının anahtar kasasından verileri okuması için gereken izinleri verme.

Size daha fazla ayrıntıya önce okuyun [Key Vault için temel kavramlar](key-vault-whatis.md#basic-concepts).

>[!NOTE]
>Key Vault, gizli dizilerin program aracılığıyla depolandığı merkezi bir depodur. Ancak bunun için uygulamaların ve kullanıcıların bir Key Vault'a kimlik doğrulaması yapması, yani gizli dizi sunması gerekir. Mantığıyla en iyi güvenlik uygulamaları, ilk bu gizli dizinin düzenli aralıklarla döndürülmesi gerekir. 
>
>İle [yönetilen hizmet kimliklerini Azure kaynakları için](../active-directory/managed-identities-azure-resources/overview.md), Azure'da çalışan uygulamalar otomatik olarak Azure tarafından yönetilen bir kimlik alın. Bu, *gizli dizi belirleme sorununu* çözerek kullanıcıların ve uygulamaların en iyi yöntemleri uygulayabilmesini ve ilk gizli diziyi döndürme konusunda endişelenmemesini sağlar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prerequisites"></a>Önkoşullar

* Windows'da:
  * [Visual Studio 2019](https://www.microsoft.com/net/download/windows) aşağıdaki iş yükleri ile:
    * ASP.NET ve web geliştirme
    * .NET core platformlar arası geliştirme
  * [.NET Core 2.1 SDK veya üzeri](https://www.microsoft.com/net/download/windows)

* Mac'te:
  * Bkz. [Mac için Visual Studio'daki Yenilikler](https://visualstudio.microsoft.com/vs/mac/).

* Tüm platformlar:
  * Git ([indir](https://git-scm.com/downloads)).
  * Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.
  * [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) 2.0.4 veya sonraki sürümü. Bu uygulama Windows, Mac ve Linux’ta kullanılabilir.

## <a name="log-in-to-azure"></a>Azure'da oturum açma

Azure CLI'yi kullanarak Azure'da oturum açmak için, şunları girin:

```azurecli
az login
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create](/cli/azure/group#az-group-create) komutunu kullanarak bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

Kaynak grubu adı seçin ve yer tutucuyu doldurun.
Aşağıdaki örnekte Doğu ABD konumunda bir kaynak grubu oluşturulmaktadır:

```azurecli
# To list locations: az account list-locations --output table
az group create --name "<YourResourceGroupName>" --location "East US"
```

Az önce oluşturduğunuz kaynak grubu bu makale boyunca kullanılır.

## <a name="create-a-key-vault"></a>Bir anahtar kasası oluşturma

Ardından, önceki adımda oluşturduğunuz kaynak grubunda bir anahtar kasası oluşturursunuz. Şu bilgileri belirtin:

* Anahtar kasası adı: Ad 3-24 karakter dizesi olmalı ve yalnızca 0-9, içermelidir a-z, A-Z ve tire (-).
* Kaynak grubu adı.
* Konum: **Doğu ABD**.

```azurecli
az keyvault create --name "<YourKeyVaultName>" --resource-group "<YourResourceGroupName>" --location "East US"
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

## <a name="clone-the-repo"></a>Depoyu kopyalama

Kaynağı düzenleyebileceğiniz yerel bir kopya oluşturmak için depoyu kopyalayın. Şu komutu çalıştırın:

```
git clone https://github.com/Azure-Samples/key-vault-dotnet-core-quickstart.git
```

## <a name="open-and-edit-the-solution"></a>Çözümü açma ve düzenleme

Belirlediğiniz anahtar kasanızın adıyla örneği çalıştırmak için program.cs dosyasını düzenleyin:

1. key-vault-dotnet-core-quickstart klasörüne gidin.
2. Visual Studio 2019 içinde key-vault-dotnet-core-quickstart.sln dosyasını açın.
3. Program.cs dosyasını açın ve yer tutucu güncelleştirme *YourKeyVaultName* daha önce oluşturduğunuz anahtar kasasının adı.

Bu çözümde [AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) ve [KeyVault](https://www.nuget.org/packages/Microsoft.Azure.KeyVault) NuGet kitaplıkları kullanılır.

## <a name="run-the-app"></a>Uygulamayı çalıştırma

Visual Studio 2019 ana menüden seçin **hata ayıklama** > **ayıklamadan Başlat**. Tarayıcı görüntülendiğinde **Hakkında** sayfasına gidin. **AppSecret** değeri görüntülenir.

## <a name="publish-the-web-application-to-azure"></a>Web uygulamasını Azure’a yayımlama

Canlı web uygulaması olarak görüntülemek için bu uygulamayı Azure'da yayımlayın ve gizli dizi değerini getirebildiğinizi görmek için:

1. Visual Studio'da **key-vault-dotnet-core-quickstart** projesini seçin.
2. **Yayımla** > **Başlat** seçeneğini belirleyin.
3. Yeni bir **App Service** oluşturun ve ardından **Yayımla** seçeneğini belirleyin.
4. Uygulama adını **keyvaultdotnetcorequickstart** olarak değiştirin.
5. **Oluştur**’u seçin.

>[!VIDEO https://sec.ch9.ms/ch9/e93d/a6ac417f-2e63-4125-a37a-8f34bf0fe93d/KeyVault_high.mp4]

## <a name="enable-a-managed-identity-for-the-web-app"></a>Web uygulaması için yönetilen kimliği etkinleştirme

Azure Key Vault kimlik bilgilerini ve diğer anahtarlarla gizli dizileri güvenle depolamak için bir yol sağlar, ama bunları alabilmek için kodunuzun Key Vault'ta kimlik doğrulaması yapması gerekir. [Azure kaynakları için yönetilen kimliklere genel bakış](../active-directory/managed-identities-azure-resources/overview.md), Azure hizmetlerine Azure Active Directory (Azure AD) üzerinde otomatik olarak yönetilen bir kimlik vererek bu soruna daha basit bir çözüm getirir. Bu kimliği kullanarak, Key Vault da dahil olmak üzere Azure AD kimlik doğrulamasını destekleyen tüm hizmetlerde kodunuzda kimlik bilgileri bulunmasına gerek kalmadan kimlik doğrulaması yapabilirsiniz.

Azure CLI, bu uygulama için kimlik oluşturmak için Ata kimlik komutu çalıştırın:

   ```azurecli
   az webapp identity assign --name "keyvaultdotnetcorequickstart" --resource-group "<YourResourceGroupName>"
   ```

>[!NOTE]
>Bu yordamdaki komut, portala gidip web uygulaması özelliklerinde **Kimlik/Sistem tarafından atanan** ayarını **Açık** duruma getirmekle eşdeğerdir.

## <a name="assign-permissions-to-your-application-to-read-secrets-from-key-vault"></a>Key Vault gizli dizilerini okumak için uygulamanıza izin atama

Uygulamayı Azure'da yayımladığınızda çıktıyı not edin. Şu biçimde olmalıdır:
        
        {
          "principalId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
          "tenantId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
          "type": "SystemAssigned"
        }
        
Ardından anahtar kasanızın adını ve **PrincipalId** değerini kullanarak bu komutu çalıştırın:

```azurecli

az keyvault set-policy --name '<YourKeyVaultName>' --object-id <PrincipalId> --secret-permissions get list

```

Uygulamayı çalıştırdığınızda gizli dizi değerinizin alındığını görürsünüz. Önceki komutta hizmet izinleri yapmak için uygulama kimliğini yaparken **alma** ve **listesi** anahtar kasanıza operations.

## <a name="clean-up-resources"></a>Kaynakları temizleme
Artık ihtiyacınız kalmadığında kaynak grubunu, sanal makine ve tüm ilgili kaynakları silin. Bunu yapmak için anahtar kasası için kaynak grubunu seçin ve seçin **Sil**.

Anahtar kasasını kullanarak silme [az keyvault delete](https://docs.microsoft.com/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-delete) komutu:

```azurecli
az keyvault delete --name
                   [--resource-group]
                   [--subscription]
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Key Vault hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/key-vault/key-vault-whatis)
