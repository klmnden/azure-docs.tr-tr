---
title: "Hızlı başlangıç: Node Web Uygulaması kullanarak Azure Key Vault'tan gizli dizi ayarlama ve alma | Microsoft Docs"
description: "Hızlı başlangıç: Node Web Uygulaması kullanarak Azure Key Vault'tan gizli dizi ayarlama ve alma"
services: key-vault
author: prashanthyv
manager: sumedhb
ms.service: key-vault
ms.topic: quickstart
ms.date: 07/24/2018
ms.author: barclayn
ms.custom: mvc
ms.openlocfilehash: 0188d06e5c58287e1040f6a15456d3ffe291b04a
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "42023548"
---
# <a name="quickstart-set-and-retrieve-a-secret-from-azure-key-vault-using-a-net-web-app"></a>Hızlı başlangıç: .NET Web Uygulaması kullanarak Azure Key Vault'tan gizli dizi ayarlama ve alma

Bu hızlı başlangıçta, Azure web uygulamasının yönetilen hizmet kimliklerini kullanarak Anahtar Kasasından bilgi okumasını sağlamak için gerekli adımların üzerinden geçeceksiniz. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Anahtar Kasası oluşturun.
> * Anahtar Kasasında bir gizli dizi depolayın.
> * Anahtar Kasasındaki bir gizli anahtarı alın.
> * Azure Web Uygulaması oluşturun.
> * [Yönetilen hizmet kimliklerini etkinleştirin](../active-directory/managed-service-identity/overview.md).
> * Web uygulamasının Anahtar Kasasından verileri okuması için gereken izinleri verin.

İlerlemeden önce lütfen [temel kavramları](key-vault-whatis.md#basic-concepts) okuyun.

>[!NOTE]
Aşağıdaki öğreticinin neden en iyi yöntem olduğunu anlamak için bilmeniz gereken birkaç kavram vardır. Key Vault, gizli dizilerin program aracılığıyla depolandığı merkezi bir depodur. Bunu yapmak için uygulamaların/kullanıcıların gizli dizi sunarak Key Vault kimlik doğrulamasından geçmesi gerekir. Güvenlikle ilgili en iyi yöntemlerin uygulanması için bu ilk gizli dizinin de düzenli olarak döndürülmesi gerekir. Ancak [Yönetilen Hizmet Kimliği](../active-directory/managed-service-identity/overview.md) sayesinde Azure'da çalışan uygulamalara Azure tarafından otomatik olarak yönetilen bir kimlik verilir. Bu da **Gizli Dizi Belirleme Sorununu** çözerek kullanıcıların/uygulamaların en iyi yöntemleri uygulamasına ve ilk gizli diziyi döndürme konusunda endişelenmemesine yardımcı olur.

## <a name="prerequisites"></a>Ön koşullar

* Windows'da:
  * [Visual Studio 2017 sürüm 15.7.3 veya üzeri](https://www.microsoft.com/net/download/windows), aşağıdaki iş yükleriyle birlikte:
    * ASP.NET ve web geliştirme
    * .NET Core çoklu platform geliştirme
  * [.NET Core 2.1 SDK veya üzeri](https://www.microsoft.com/net/download/windows)

* Mac'te:
  * https://visualstudio.microsoft.com/vs/mac/

* Tüm platformlar:
  * GIT'i [buradan](https://git-scm.com/downloads) indirin.
  * Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.
  * [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) Azure CLI sürüm 2.0.4 veya üzeri gerekir. Bu uygulama Windows, Mac ve Linux’ta kullanılabilir

## <a name="login-to-azure"></a>Azure'da oturum açma

   CLI kullanarak Azure'da oturum açmak için şunu yazabilirsiniz:

```azurecli
az login
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create](/cli/azure/group#az-group-create) komutuyla bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

Lütfen bir Kaynak Grubu adı seçin ve yer tutucunun yerine yazın.
Aşağıdaki örnekte, *eastus* konumunda *<YourResourceGroupName>* adlı bir kaynak grubu oluşturulur.

```azurecli
# To list locations: az account list-locations --output table
az group create --name "<YourResourceGroupName>" --location "East US"
```

Az önce oluşturduğunuz kaynak grubu bu makale boyunca kullanılır.

## <a name="create-an-azure-key-vault"></a>Azure Key Vault oluşturma

Daha sonra, önceki adımda oluşturulan kaynak grubunda bir Anahtar Kasası oluşturursunuz. Şu bilgileri belirtin:

* Kasa adı: **Lütfen buradan bir Anahtar Kasası adı seçin**. Anahtar Kasası adı, 3-24 karakter uzunluğundaki bir dize olmalı ve yalnızca 0-9, a-z, A-Z ve - karakterlerini içermelidir.
* Kaynak grubu adı: **Lütfen buradan bir Kaynak Grubu Adı seçin**.
* Konum: **Doğu ABD**.

```azurecli
az keyvault create --name "<YourKeyVaultName>" --resource-group "<YourResourceGroupName>" --location "East US"
```

Bu noktada Azure hesabınız, bu yeni anahtar kasasında herhangi bir işlemi gerçekleştirmeye yetkili olan tek hesaptır.

## <a name="add-a-secret-to-key-vault"></a>Key Vault’a gizli dizi ekleme

Bunun nasıl çalıştığını göstermemize yardımcı olması için bir gizli dizi ekliyoruz. Güvenle korumanız ama uygulamanıza da sağlamanız gereken bir SQL bağlantı dizesini veya başka bir bilgiyi depoluyor olabilirsiniz. Bu öğreticide, parola **AppSecret** olarak adlandırılır ve içinde **MySecret** değeri depolanır.

Anahtar Kasasında **MySecret** değerini depolayacak **AppSecret** adlı gizli dizi oluşturmak için aşağıdaki komutları yazın:

```azurecli
az keyvault secret set --vault-name "<YourKeyVaultName>" --name "AppSecret" --value "MySecret"
```

Gizli dizi içindeki değeri düz metin olarak görüntülemek için:

```azurecli
az keyvault secret show --name "AppSecret" --vault-name "<YourKeyVaultName>"
```

Bu komut, URI de dahil olmak üzere gizli bilgiyi gösterir. Bu adımları tamamladıktan sonra, Azure Key Vault'ta bu gizli dizinin URI'sine sahip olmalısınız. Bu bilgileri not alın. Sonraki adımlardan birinde gerekecektir.

## <a name="clone-the-repo"></a>Depoyu kopyalama

Aşağıdaki komutu kullanarak kaynağı düzenleyebilmeniz için yerel kopyasını oluşturmak üzere depoyu kopyalayın:

```
git clone https://github.com/Azure-Samples/key-vault-dotnet-core-quickstart.git
```

## <a name="open-and-edit-the-solution"></a>Çözümü açma ve düzenleme

Örneği belirlediğiniz anahtar kasası adıyla çalıştırmak için program.cs dosyasını düzenleyin.

1. key-vault-dotnet-core-quickstart klasörüne gidin
2. key-vault-dotnet-core-quickstart.sln dosyasını Visual Studio 2017'de açın
3. Program.cs dosyasını açın ve <YourKeyVaultName> yer tutucusunun yerine önceden oluşturduğunuz Anahtar Kasası adını yazın.

Bu çözümde [AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) ve [KeyVault](https://www.nuget.org/packages/Microsoft.Azure.KeyVault) NuGet kitaplıkları kullanılmaktadır

## <a name="run-the-app"></a>Uygulamayı çalıştırma

Visual Studio 2017 ana menüsünden Hata Ayıklama > Hata ayıklama olmadan başlat'ı seçin. Tarayıcı görüntülendiğinde Hakkında sayfasına gidin. AppSecret'in değeri görüntülenir.

## <a name="publish-the-web-application-to-azure"></a>Web uygulamasını Azure’a yayımlama

Canlı bir web uygulaması olarak incelemek ve gizli dizi değerini aldığımızı görmek için bu uygulamayı Azure'da yayımlıyoruz

1. Visual Studio'da **key-vault-dotnet-core-quickstart** Projesini seçin.
2. **Yayımla**'yı ve ardından **Başlat**'ı seçin.
3. Yeni bir **App Service** oluşturun ve **Yayımla**'yı seçin.
4. Uygulama Adını "keyvaultdotnetcorequickstart" olarak değiştirin
5. **Oluştur**’u seçin.

>[!VIDEO https://sec.ch9.ms/ch9/e93d/a6ac417f-2e63-4125-a37a-8f34bf0fe93d/KeyVault_high.mp4]

## <a name="enable-managed-service-identities-msi"></a>Yönetilen Hizmet Kimliklerini (MSI) etkinleştirme

Azure Key Vault kimlik bilgilerini ve diğer anahtarlarla gizli dizileri güvenle depolamak için bir yol sağlar, ama bunları alabilmek için kodunuzun Azure Key Vault'ta kimlik doğrulaması yapması gerekir. Yönetilen Hizmet Kimliği (MSI), Azure hizmetlerine Azure Active Directory (Azure AD) üzerinde otomatik olarak yönetilen bir kimlik vererek bu durumu kolaylaştırır. Bu kimliği kullanarak, Key Vault da dahil olmak üzere Azure AD kimlik doğrulamasını destekleyen tüm hizmetlerde kodunuzda kimlik bilgileri bulunmasına gerek kalmadan kimlik doğrulaması yapabilirsiniz.

1. Azure CLI'ye dönme
2. Bu uygulamanın kimliğini oluşturmak için assign-identity komutunu çalıştırın:

```azurecli
az webapp identity assign --name "keyvaultdotnetcorequickstart" --resource-group "<YourResourceGroupName>"
```

>[!NOTE]
>Bu komut, portala gidip web uygulaması özelliklerinde **Yönetilen hizmet kimliği** ayarını **Açık** duruma getirmekle eşdeğerdir.

## <a name="assign-permissions-to-your-application-to-read-secrets-from-key-vault"></a>Key Vault gizli dizilerini okumak için uygulamanıza izin atama

[Uygulamayı Azure'da yayımladığınızda][] çıkışı not edin. Şu biçimde olmalıdır:
        
        {
          "principalId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
          "tenantId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
          "type": "SystemAssigned"
        }
        
Ardından Key Vault adını ve yukarıdan tamamladığınız PrincipalId değerini kopyalayarak şu komutu çalıştırın:

```azurecli

az keyvault set-policy --name '<YourKeyVaultName>' --object-id <PrincipalId> --secret-permissions get

```

**Uygulamayı çalıştırdığınızda gizli dizi değerinin alındığını görmeniz gerekir**

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Key Vault Ana Sayfası](https://azure.microsoft.com/services/key-vault/)
* [Azure Key Vault Belgeleri](https://docs.microsoft.com/azure/key-vault/)
* [.NET için Azure SDK](https://github.com/Azure/azure-sdk-for-net)
* [Azure REST API Başvurusu](https://docs.microsoft.com/rest/api/keyvault/)
