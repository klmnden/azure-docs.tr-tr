---
title: "Hızlı başlangıç: Node Web Uygulaması kullanarak Azure Key Vault'tan gizli dizi ayarlama ve alma | Microsoft Docs"
description: "Hızlı başlangıç: Node Web Uygulaması kullanarak Azure Key Vault'tan gizli dizi ayarlama ve alma"
services: key-vault
documentationcenter: ''
author: prashanthyv
manager: sumedhb
ms.service: key-vault
ms.workload: identity
ms.topic: quickstart
ms.date: 09/05/2018
ms.author: barclayn
ms.custom: mvc
ms.openlocfilehash: 860294ebc7fbadd3eeefc4298ec740ca7f704587
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "44714403"
---
# <a name="quickstart-set-and-retrieve-a-secret-from-azure-key-vault-using-a-node-web-app"></a>Hızlı başlangıç: Node Web Uygulaması kullanarak Azure Key Vault'tan gizli dizi ayarlama ve alma 

Bu hızlı başlangıçta Anahtar Kasasında gizli dizi depolama ve bunu Web uygulaması kullanarak alma adımları gösterilmektedir. Gizli dizi değerini görmek için bunu Azure'da çalıştırmanız gerekir. Bu hızlı başlangıçta, Azure kaynakları için yönetilen kimlikler ve Node.js kullanılmaktadır.

> [!div class="checklist"]
> * Anahtar Kasası oluşturun.
> * Anahtar Kasasında bir gizli dizi depolayın.
> * Anahtar Kasasındaki bir gizli anahtarı alın.
> * Azure Web Uygulaması oluşturun.
> * Web uygulaması için [yönetilen kimliği](https://docs.microsoft.com/azure/active-directory/managed-service-identity/overview) etkinleştirin.
> * Web uygulamasının Anahtar Kasasından verileri okuması için gereken izinleri verin.

Devam etmeden önce [temel kavramlar](key-vault-whatis.md#basic-concepts) hakkında bilgi sahibi olduğunuzdan emin olun.

>[!NOTE]
Aşağıdaki öğreticinin neden en iyi yöntem olduğunu anlamak için bilmeniz gereken birkaç kavram vardır. Key Vault, gizli dizilerin program aracılığıyla depolandığı merkezi bir depodur. Bunu yapmak için uygulamaların/kullanıcıların gizli dizi sunarak Key Vault kimlik doğrulamasından geçmesi gerekir. Güvenlikle ilgili en iyi yöntemlerin uygulanması için bu ilk gizli dizinin de düzenli olarak döndürülmesi gerekir. Ancak [Azure kaynakları için yönetilen kimlikler](../active-directory/managed-identities-azure-resources/overview.md) sayesinde Azure’da çalıştırılan uygulamalara otomatik olarak Azure tarafından yönetilen bir kimlik verilir. Bu da **Gizli Dizi Belirleme Sorununu** çözerek kullanıcıların/uygulamaların en iyi yöntemleri uygulamasına ve ilk gizli diziyi döndürme konusunda endişelenmemesine yardımcı olur.

## <a name="prerequisites"></a>Ön koşullar

* [Node JS](https://nodejs.org/en/)
* [Git](https://www.git-scm.com/)
* [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) 2.0.4 veya üzeri
* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="login-to-azure"></a>Azure'da oturum açma

CLI kullanarak Azure'da oturum açmak için şunu yazabilirsiniz:

```azurecli
az login
```

## <a name="create-resource-group"></a>Kaynak grubu oluşturma

[az group create](/cli/azure/group#az-group-create) komutuyla bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

Lütfen bir Kaynak Grubu adı seçin ve yer tutucunun yerine yazın.
Aşağıdaki örnekte, *eastus* konumunda *<YourResourceGroupName>* adlı bir kaynak grubu oluşturulur.

```azurecli
# To list locations: az account list-locations --output table
az group create --name "<YourResourceGroupName>" --location "East US"
```

Az önce oluşturduğunuz kaynak grubu bu öğretici boyunca kullanılır.

## <a name="create-an-azure-key-vault"></a>Azure Key Vault oluşturma

Daha sonra, önceki adımda oluşturulan kaynak grubunda bir Anahtar Kasası oluşturursunuz. Bu makale boyunca Anahtar Kasası için “ContosoKeyVault” adı kullanılıyor olsa da, sizin benzersiz bir ad kullanmanız gerekir. Şu bilgileri belirtin:

* Kasa adı: **Buradan bir Anahtar Kasası adı seçin**.
* Kaynak grubu adı: **Buradan bir Kaynak Grubu Adı seçin**.
* Konum: **Doğu ABD**.

```azurecli
az keyvault create --name "<YourKeyVaultName>" --resource-group "<YourResourceGroupName>" --location "East US"
```

Bu noktada Azure hesabınız, bu yeni anahtar kasasında herhangi bir işlemi gerçekleştirmeye yetkili olan tek hesaptır.

## <a name="add-a-secret-to-key-vault"></a>Anahtar kasasına gizli dizi ekleme

Bunun nasıl çalıştığını göstermemize yardımcı olması için bir gizli dizi ekliyoruz. Güvenle korumanız ama uygulamanıza da sağlamanız gereken bir SQL bağlantı dizesini veya başka bir bilgiyi depoluyor olabilirsiniz. Bu öğreticide, parola **AppSecret** olarak adlandırılır ve içinde **MySecret** değeri depolanır.

Anahtar Kasasında **MySecret** değerini depolayacak **AppSecret** adlı gizli dizi oluşturmak için aşağıdaki komutları yazın:

```azurecli
az keyvault secret set --vault-name "<YourKeyVaultName>" --name "AppSecret" --value "MySecret"
```

Gizli dizi içindeki değeri düz metin olarak görüntülemek için:

```azurecli
az keyvault secret show --name "AppSecret" --vault-name "<YourKeyVaultName>"
```

Bu komut, URI de dahil olmak üzere gizli bilgiyi gösterir. Bu adımları tamamladıktan sonra, Azure Key Vault'ta bu gizli dizinin URI'sine sahip olmalısınız. Bu bilgileri bir yere yazın. Sonraki adımlardan birinde gerekecektir.

## <a name="clone-the-repo"></a>Depoyu kopyalama

Aşağıdaki komutu kullanarak kaynağı düzenleyebilmeniz için yerel kopyasını oluşturmak üzere depoyu kopyalayın:

```
git clone https://github.com/Azure-Samples/key-vault-node-quickstart.git
```

## <a name="install-dependencies"></a>Bağımlılıkları yükleme

Bu adımda bağımlılıkları yükleyeceksiniz. Şu komutları çalıştırın: cd key-vault-node-quickstart npm install

Bu projede 2 düğüm modülü kullanılmaktadır:

* [ms-rest-azure](https://www.npmjs.com/package/ms-rest-azure) 
* [azure-keyvault](https://www.npmjs.com/package/azure-keyvault)

## <a name="publish-the-web-application-to-azure"></a>Web uygulamasını Azure’a yayımlama

Yapmanız gereken birkaç adım aşağıda gösterilmiştir

- 1. adım, [Azure App Service](https://azure.microsoft.com/services/app-service/) Planı oluşturmaktır. Bu planda birden fazla web uygulaması depolayabilirsiniz.

    ```
    az appservice plan create --name myAppServicePlan --resource-group myResourceGroup
    ```
- Sonraki adımda bir web uygulaması oluşturmanız gerekir. Aşağıdaki örnekte <app_name> kısmını genel olarak benzersiz bir uygulama adıyla değiştirin (geçerli karakterler a-z, 0-9 ve - şeklindedir). Çalışma zamanı NODE|6.9 olarak ayarlanmıştır. Desteklenen tüm çalışma zamanlarını görmek için, az webapp list-runtimes komutunu çalıştırın.
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
    Yeni oluşturduğunuz web uygulamasına göz attığınızda çalışan bir web uygulaması görmeniz gerekir. <app_name> değerini benzersiz bir uygulama adıyla değiştirin.

    ```
    http://<app name>.azurewebsites.net
    ```
    Yukarıdaki komut ayrıca yerel git ortamınızdan Azure'a dağıtım yapmanızı sağlayan Git özellikli bir uygulama oluşturur. 
    Yerel git, şu URL'ye sahiptir: 'https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git'

- Dağıtım kullanıcısı oluşturma Bir önceki komut tamamlandıktan sonra yerel Git deponuza bir Azure uzak bağlantı ekleyebilirsiniz. <url> yerine Uygulamanız için Git'i etkinleştirme adımında aldığınız Git uzak ortamının URL'sini yazın.

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

Yukarıdaki komutun çıktısını not edin veya kopyalayın. Şu biçimde olmalıdır:
        
        {
          "principalId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
          "tenantId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
          "type": "SystemAssigned"
        }
        
Ardından Key Vault adını ve yukarıdan tamamladığınız PrincipalId değerini kopyalayarak şu komutu çalıştırın:

```azurecli
az keyvault set-policy --name '<YourKeyVaultName>' --object-id <PrincipalId> --secret-permissions get
```

## <a name="deploy-the-node-app-to-azure-and-retrieve-the-secret-value"></a>Node Uygulamasını Azure'a dağıtma ve gizli diziyi alma

Tüm ayarları yaptınız. Uygulamayı Azure'a dağıtmak için aşağıdaki komutu çalıştırın

```
git push azure master
```

Ardından https://<app_name>.azurewebsites.net adresine gittiğinizde gizli diziyi görebilirsiniz.
<YourKeyVaultName> yerine kasanızın adını yazdığınızdan emin olun

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Key Vault Ana Sayfası](https://azure.microsoft.com/services/key-vault/)
* [Azure Key Vault Belgeleri](https://docs.microsoft.com/azure/key-vault/)
* [Node için Azure SDK](https://docs.microsoft.com/javascript/api/overview/azure/key-vault)
* [Azure REST API Başvurusu](https://docs.microsoft.com/rest/api/keyvault/)
