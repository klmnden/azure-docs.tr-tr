---
title: Hızlı Başlangıç - kümesi ve bir düğüm web uygulaması kullanarak Azure Key vault'tan gizli dizi alma | Microsoft Docs
description: Bu hızlı başlangıçta, ayarlayın ve bir düğüm web uygulaması kullanarak Azure Key Vault'tan bir gizli dizi alma
services: key-vault
author: mbaldwin
manager: sumedhb
ms.service: key-vault
ms.topic: quickstart
ms.date: 09/05/2018
ms.author: barclayn
ms.custom: mvc
ms.openlocfilehash: 9aa7c4a5464230abe9ac7e75854a7422534f40f6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60461189"
---
# <a name="quickstart-set-and-retrieve-a-secret-from-azure-key-vault-by-using-a-node-web-app"></a>Hızlı Başlangıç: Bir düğüm web uygulaması kullanarak Azure Key Vault'tan bir gizli dizi alma ve ayarlama 

Bu hızlı başlangıçta, Azure anahtar Kasası'nda bir gizli dizi depolamak nasıl ve nasıl bir web uygulamasını kullanarak alınacağını gösterir. Anahtar kasasını kullanarak, bilgilerin güvende tutulmasına yardımcı olur. Gizli değer görmek için bu hızlı başlangıçta, Azure üzerinde çalıştırmak gerekir. Bu hızlı başlangıçta, Azure kaynakları için yönetilen kimlikler ve Node.js kullanılmaktadır. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

* Bir anahtar kasası oluşturma.
* Anahtar kasasına bir gizli dizi depolama.
* Anahtar kasasından bir gizli dizi alma.
* Azure web uygulaması oluşturma.
* Web uygulaması için [yönetilen kimliği](https://docs.microsoft.com/azure/active-directory/managed-service-identity/overview) etkinleştirin.
* Web uygulamasının anahtar kasasından verileri okuması için gereken izinleri verme.

Devam etmeden önce alışık olduğunuz emin [Key Vault için temel kavramlar](key-vault-whatis.md#basic-concepts).

> [!NOTE]
> Key Vault, gizli dizilerin program aracılığıyla depolandığı merkezi bir depodur. Ancak bunun için uygulamaların ve kullanıcıların bir Key Vault'a kimlik doğrulaması yapması, yani gizli dizi sunması gerekir. Mantığıyla en iyi güvenlik uygulamaları, ilk bu gizli dizinin düzenli aralıklarla döndürülmesi gerekir. 
>
> İle [yönetilen hizmet kimliklerini Azure kaynakları için](../active-directory/managed-identities-azure-resources/overview.md), Azure'da çalışan uygulamalar otomatik olarak Azure tarafından yönetilen bir kimlik alın. Bu, *gizli dizi belirleme sorununu* çözerek kullanıcıların ve uygulamaların en iyi yöntemleri uygulayabilmesini ve ilk gizli diziyi döndürme konusunda endişelenmemesini sağlar.

## <a name="prerequisites"></a>Önkoşullar

* [Node.js](https://nodejs.org/en/)
* [Git](https://www.git-scm.com/)
* [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) 2.0.4 veya üzeri. Bu hızlı başlangıçta, Azure CLI'yi yerel olarak çalıştırmanızı gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. CLI’yı yüklemeniz veya yükseltmeniz gerekiyorsa bkz. [Azure CLI 2.0’ı yükleme](https://review.docs.microsoft.com/en-us/cli/azure/install-azure-cli?branch=master&view=azure-cli-latest).
* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="log-in-to-azure"></a>Azure'da oturum açma

Azure CLI'yi kullanarak Azure'da oturum açmak için, şunları girin:

```azurecli
az login
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create](/cli/azure/group#az-group-create) komutunu kullanarak bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

Kaynak grubu adı seçin ve yer tutucuyu doldurun.
Aşağıdaki örnek, Doğu ABD konumunda bir kaynak grubu oluşturur.

```azurecli
# To list locations: az account list-locations --output table
az group create --name "<YourResourceGroupName>" --location "East US"
```

Az önce oluşturduğunuz kaynak grubu bu makale boyunca kullanılır.

## <a name="create-a-key-vault"></a>Bir anahtar kasası oluşturma

Ardından, önceki adımda oluşturduğunuz kaynak grubunu kullanarak bir anahtar kasası oluşturun. Bu makalede adı olarak "ContosoKeyVault" kullansa da, benzersiz bir ad kullanmanız gerekir. Şu bilgileri belirtin:

* Anahtar kasası adı.
* Kaynak grubu adı. Ad 3-24 karakter dizesi olmalı ve yalnızca 0-9, içermelidir a-z, A-Z ve tire (-).
* Konum: **Doğu ABD**.

```azurecli
az keyvault create --name "<YourKeyVaultName>" --resource-group "<YourResourceGroupName>" --location "East US"
```

Bu noktada Azure hesabınız, bu yeni anahtar kasasında herhangi bir işlemi gerçekleştirmeye yetkili olan tek hesaptır.

## <a name="add-a-secret-to-the-key-vault"></a>Anahtar kasasına gizli dizi ekleme

Bunun nasıl çalıştığını göstermemize yardımcı olması için bir gizli dizi ekliyoruz. Güvenle korumanız ancak uygulamanıza da sağlamanız gereken bir SQL bağlantı dizesini veya başka bir bilgiyi depoluyor olabilirsiniz. Bu öğreticide, parola **AppSecret** olarak adlandırılır ve içinde **MySecret** değeri depolanır.

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
git clone https://github.com/Azure-Samples/key-vault-node-quickstart.git
```

## <a name="install-dependencies"></a>Bağımlılıkları yükleme

Bağımlılıkları yüklemek için aşağıdaki komutları çalıştırın:

```
cd key-vault-node-quickstart
npm install
```

Bu projede iki düğüm modüllerini kullanır: [ms rest azure](https://www.npmjs.com/package/ms-rest-azure) ve [azure keyvault](https://www.npmjs.com/package/azure-keyvault).

## <a name="publish-the-web-app-to-azure"></a>Web uygulamasını Azure’da yayımlama

Oluşturma bir [Azure App Service](https://azure.microsoft.com/services/app-service/) planı. Bu planda birden fazla web uygulaması depolayabilirsiniz.

    ```
    az appservice plan create --name myAppServicePlan --resource-group myResourceGroup
    ```
Ardından, bir web uygulaması oluşturun. Aşağıdaki örnekte, değiştirin `<app_name>` bir genel benzersiz uygulama adıyla (geçerli karakterler: a-z, 0-9 ve -). Çalışma zamanı NODE|6.9 olarak ayarlanmıştır. Desteklenen tüm çalışma zamanları görmek için şunu çalıştırın `az webapp list-runtimes`.

    ```
    # Bash
    az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app_name> --runtime "NODE|6.9" --deployment-local-git
    ```
Web uygulaması oluşturulduğunda Azure CLI aşağıda yer alan çıktıdaki gibi bilgiler gösterir:

    ```
    {
      "availabilityState": "Normal",
      "clientAffinityEnabled": true,
      "clientCertEnabled": false,
      "cloningInfo": null,
      "containerSize": 0,
      "dailyMemoryTimeQuota": 0,
      "defaultHostName": "<app_name>.azurewebsites.net",
      "enabled": true,
      "deploymentLocalGitUrl": "https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git"
      < JSON data removed for brevity. >
    }
    ```
Yeni oluşturulan web uygulamanıza gidin ve düzgün çalıştığını görmelisiniz. Değiştirin `<app_name>` benzersiz bir uygulama adına sahip.

    ```
    http://<app name>.azurewebsites.net
    ```
Yukarıdaki komut, ayrıca, yerel Git deponuzdan Azure'a dağıtmanızı sağlayan Git etkin bir uygulama oluşturur. Yerel Git deposu bu URL ile yapılandırılmış: `https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git`.

Önceki komutta tamamladıktan sonra yerel Git deponuza bir Azure uzak ekleyebilirsiniz. Değiştirin `<url>` Git deposunun URL'si ile.

    ```
    git remote add azure <url>
    ```

## <a name="enable-a-managed-identity-for-the-web-app"></a>Web uygulaması için yönetilen kimliği etkinleştirme

Azure Key Vault kimlik bilgilerini ve diğer anahtarlarla gizli dizileri güvenle depolamak için bir yol sağlar, ama bunları alabilmek için kodunuzun Key Vault'ta kimlik doğrulaması yapması gerekir. [Azure kaynakları için yönetilen kimliklere genel bakış](../active-directory/managed-identities-azure-resources/overview.md), Azure hizmetlerine Azure Active Directory (Azure AD) üzerinde otomatik olarak yönetilen bir kimlik vererek bu soruna daha basit bir çözüm getirir. Bu kimliği kullanarak, Key Vault da dahil olmak üzere Azure AD kimlik doğrulamasını destekleyen tüm hizmetlerde kodunuzda kimlik bilgileri bulunmasına gerek kalmadan kimlik doğrulaması yapabilirsiniz.

Bu uygulamanın kimliğini oluşturmak için assign-identity komutunu çalıştırın:

```azurecli
az webapp identity assign --name <app_name> --resource-group "<YourResourceGroupName>"
```

Bu komut, portala gidip web uygulaması özelliklerinde **Kimlik/Sistem tarafından atanan** ayarını **Açık** duruma getirmekle eşdeğerdir.

### <a name="assign-permissions-to-your-application-to-read-secrets-from-key-vault"></a>Key Vault gizli dizilerini okumak için uygulamanıza izin atama

Önceki komutun çıktısındaki not edin. Şu biçimde olmalıdır:
        
        {
          "principalId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
          "tenantId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
          "type": "SystemAssigned"
        }
        
Daha sonra anahtar kasasının adını ve değerini kullanarak aşağıdaki komutu çalıştırın **Principalıd**:

```azurecli
az keyvault set-policy --name '<YourKeyVaultName>' --object-id <PrincipalId> --secret-permissions get set
```

## <a name="deploy-the-node-app-to-azure-and-retrieve-the-secret-value"></a>Düğüm uygulamasını Azure'a dağıtma ve gizli değer alma

Uygulamayı Azure'a dağıtmak için aşağıdaki komutu çalıştırın:

```
git push azure master
```

Bundan sonra göz atarken `https://<app_name>.azurewebsites.net`, gizli değer görebilirsiniz. Adı yerine emin `<YourKeyVaultName>` vault adınızla.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Node için Azure SDK](https://docs.microsoft.com/javascript/api/overview/azure/key-vault)
