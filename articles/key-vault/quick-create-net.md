---
title: "Hızlı Başlangıç: Node web uygulaması kullanarak Azure Key Vault'tan gizli dizi ayarlama ve alma| Microsoft Docs"
description: "Hızlı Başlangıç: Node web uygulaması kullanarak Azure Key Vault'tan gizli dizi ayarlama ve alma"
services: key-vault
author: prashanthyv
manager: sumedhb
ms.service: key-vault
ms.topic: quickstart
ms.date: 07/24/2018
ms.author: barclayn
ms.custom: mvc
ms.openlocfilehash: a9ae1fb3243c31eb92231320c5ced93d80301a0d
ms.sourcegitcommit: ebb460ed4f1331feb56052ea84509c2d5e9bd65c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/24/2018
ms.locfileid: "42917443"
---
# <a name="quickstart-set-and-retrieve-a-secret-from-azure-key-vault-by-using-a-net-web-app"></a>Hızlı Başlangıç: .NET web uygulaması kullanarak Azure Key Vault'tan gizli dizi ayarlama ve alma

Bu hızlı başlangıçta, bir Azure web uygulamasının yönetilen hizmet kimliklerini kullanarak Azure Key Vault'tan bilgi okumasını sağlamak için gerekli adımları uygulayacaksınız. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Bir anahtar kasası oluşturma.
> * Anahtar kasasına bir gizli dizi depolama.
> * Anahtar kasasından bir gizli dizi alma.
> * Azure web uygulaması oluşturma.
> * [Yönetilen hizmet kimliklerini etkinleştirme](../active-directory/managed-service-identity/overview.md).
> * Web uygulamasının anahtar kasasından verileri okuması için gereken izinleri verme.

İlerlemeden önce lütfen [temel kavramları](key-vault-whatis.md#basic-concepts) okuyun.

>[!NOTE]
>Key Vault, gizli dizilerin program aracılığıyla depolandığı merkezi bir depodur. Ancak bunun için uygulamaların ve kullanıcıların bir Key Vault'a kimlik doğrulaması yapması, yani gizli dizi sunması gerekir. Güvenlikle ilgili en iyi yöntemlerin uygulanması için bu ilk gizli dizinin düzenli olarak döndürülmesi gerekir. 
>
>[Yönetilen Hizmet Kimliği](../active-directory/managed-service-identity/overview.md) ile, Azure'da çalıştırılan uygulamalara Azure'un otomatik olarak yönettiği bir kimlik verilir. Bu, *gizli dizi belirleme sorununu* çözerek kullanıcıların ve uygulamaların en iyi yöntemleri uygulayabilmesini ve ilk gizli diziyi döndürme konusunda endişelenmemesini sağlar.

## <a name="prerequisites"></a>Ön koşullar

* Windows'da:
  * [Visual Studio 2017 sürüm 15.7.3 veya üzeri](https://www.microsoft.com/net/download/windows), aşağıdaki iş yükleriyle birlikte:
    * ASP.NET ve web geliştirme
    * .NET Core çoklu platform geliştirme
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

* Anahtar kasası adı: Adı 3-24 karakterden oluşan bir dize olmalı ve yalnızca (0-9, a-z, A-Z ve -) karakterlerini içermelidir.
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
2. Visual Studio 2017'de key-vault-dotnet-core-quickstart.sln dosyasını açın.
3. Program.cs dosyasını açın ve *YourKeyVaultName* yer tutucusunu daha önce oluşturduğunuz anahtar kasanızın adıyla güncelleştirin.

Bu çözümde [AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) ve [KeyVault](https://www.nuget.org/packages/Microsoft.Azure.KeyVault) NuGet kitaplıkları kullanılır.

## <a name="run-the-app"></a>Uygulamayı çalıştırma

Visual Studio 2017 ana menüsünde **Hata Ayıkla** > **Hata ayıklamadan başlat** seçeneğini belirleyin. Tarayıcı görüntülendiğinde **Hakkında** sayfasına gidin. **AppSecret** değeri görüntülenir.

## <a name="publish-the-web-application-to-azure"></a>Web uygulamasını Azure’a yayımlama

Canlı web uygulaması olarak görüntülemek için bu uygulamayı Azure'da yayımlayın ve gizli dizi değerini getirebildiğinizi görmek için:

1. Visual Studio'da **key-vault-dotnet-core-quickstart** projesini seçin.
2. **Yayımla** > **Başlat** seçeneğini belirleyin.
3. Yeni bir **App Service** oluşturun ve ardından **Yayımla** seçeneğini belirleyin.
4. Uygulama adını **keyvaultdotnetcorequickstart** olarak değiştirin.
5. **Oluştur**’u seçin.

>[!VIDEO https://sec.ch9.ms/ch9/e93d/a6ac417f-2e63-4125-a37a-8f34bf0fe93d/KeyVault_high.mp4]

## <a name="enable-managed-service-identities"></a>Yönetilen hizmet kimliklerini etkinleştirme

Azure Key Vault kimlik bilgilerini ve diğer anahtarlarla gizli dizileri güvenle depolamak için bir yol sağlar, ama bunları alabilmek için kodunuzun Azure Key Vault'ta kimlik doğrulaması yapması gerekir. Yönetilen Hizmet Kimliği, Azure Active Directory'de (Azure AD) Azure hizmetlerine otomatik olarak yönetilen bir kimlik vererek bu işlemi kolaylaştırır. Bu kimliği kullanarak, Key Vault da dahil olmak üzere Azure AD kimlik doğrulamasını destekleyen tüm hizmetlerde kodunuzda kimlik bilgileri bulunmasına gerek kalmadan kimlik doğrulaması yapabilirsiniz.

1. Azure CLI'ye dönün.
2. Bu uygulamanın kimliğini oluşturmak için assign-identity komutunu çalıştırın:

   ```azurecli
   az webapp identity assign --name "keyvaultdotnetcorequickstart" --resource-group "<YourResourceGroupName>"
   ```

>[!NOTE]
>Bu yordamdaki komut, portala gidip web uygulaması özelliklerinde **Yönetilen hizmet kimliğini** **Açık** duruma getirmekle eşdeğerdir.

## <a name="assign-permissions-to-your-application-to-read-secrets-from-key-vault"></a>Key Vault gizli dizilerini okumak için uygulamanıza izin atama

Uygulamayı Azure'da yayımladığınızda çıktıyı not edin. Şu biçimde olmalıdır:
        
        {
          "principalId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
          "tenantId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
          "type": "SystemAssigned"
        }
        
Ardından anahtar kasanızın adını ve **PrincipalId** değerini kullanarak bu komutu çalıştırın:

```azurecli

az keyvault set-policy --name '<YourKeyVaultName>' --object-id <PrincipalId> --secret-permissions get

```

Uygulamayı çalıştırdığınızda gizli dizi değerinizin alındığını görürsünüz.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Key Vault ana sayfası](https://azure.microsoft.com/services/key-vault/)
* [Azure Key Vault belgeleri](https://docs.microsoft.com/azure/key-vault/)
* [.NET için Azure SDK](https://github.com/Azure/azure-sdk-for-net)
* [Azure REST API başvurusu](https://docs.microsoft.com/rest/api/keyvault/)
