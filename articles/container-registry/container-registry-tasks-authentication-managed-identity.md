---
title: Azure kapsayıcı kayıt defteri görevler ile yönetilen bir kimlik kullanın
description: Azure kaynakları için yönetilen bir kimlik atayarak diğer özel kapsayıcı kayıt defterleri dahil olmak üzere Azure kaynakları için bir Azure Container Registry görev erişim sağlar.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 06/12/2019
ms.author: danlep
ms.openlocfilehash: 5b60727472a06aaac8ccd3dce8609461e8972311
ms.sourcegitcommit: 72f1d1210980d2f75e490f879521bc73d76a17e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/14/2019
ms.locfileid: "67148041"
---
# <a name="use-an-azure-managed-identity-in-acr-tasks"></a>Azure kullanan yönetilen kimliği ACR görevler 

Kullanım bir [yönetilen Azure kaynakları için kimliği](../active-directory/managed-identities-azure-resources/overview.md) sağlayın veya kod kimlik bilgilerini yönetmek Azure container registry veya diğer Azure kaynakları için ACR görevlerden olmadan kimliğini gerek kalmadan. Örneğin, bir görevin adımı olarak başka bir kayıt defterine kapsayıcı görüntüleri İtme veya çekme için yönetilen bir kimlik kullanın.

Bu makalede, yönetilen kimlikleri hakkında daha fazla bilgi edinmek ve nasıl yapılır:

> [!div class="checklist"]
> * Sistem tarafından atanan bir kimlik veya bir ACR görevde bir kullanıcı tarafından atanan kimliği etkinleştirme
> * Kimlik gibi diğer Azure kapsayıcısı kayıt defterleri Azure kaynaklarına erişim
> * Bir görevden kaynaklara erişmek için yönetilen kimliği kullanma 

Azure kaynaklarını oluşturmak için bu makalede, Azure CLI Sürüm 2.0.66 çalıştırmanızı gerektirir veya üzeri. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli].

## <a name="why-use-a-managed-identity"></a>Yönetilen bir kimlik neden kullanmalısınız?

Azure kaynakları için yönetilen bir kimlik Azure Active Directory'de (Azure AD) otomatik olarak yönetilen bir kimlikle Azure hizmetleri sağlar. Yapılandırabileceğiniz [belirli Azure kaynaklarına](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md), yönetilen bir kimlikle ACR görevler de dahil olmak üzere. Ardından, kod veya betiklerde kimlik bilgilerini geçmeden diğer Azure kaynaklarına erişmek için kimliğini kullanın.

Yönetilen kimlik iki türleri şunlardır:

* *Kullanıcı tarafından atanan kimlikleri*, birden çok kaynağa atayın ve istediğiniz sürece için kalıcı. Kullanıcı tarafından atanan kimlikleri, şu anda Önizleme aşamasındadır.

* A *sistem yönetilen kimliği*, ACR görev gibi belirli bir kaynağa benzersizdir ve bu kaynağın ömrü boyunca sürer.

Yönetilen bir kimlik ile bir Azure kaynağı ayarladıktan sonra herhangi bir güvenlik sorumlusu gibi başka bir kaynak kimliği erişimi verin. Örneğin, yönetilen bir kimlik, bir özel kapsayıcı kayıt defterine Azure çekme, gönderme ve çekme veya diğer izinler sahip bir rol atayın. (Kayıt defteri rolleri tam bir listesi için bkz. [Azure Container Registry rolleri ve izinleri](container-registry-roles.md).) Bir veya daha fazla kaynak için bir kimlik erişimi verebilirsiniz.

## <a name="create-container-registries"></a>Kapsayıcı kayıt defterleri oluşturma

Bu öğretici için üç kapsayıcı kayıt defterleri gerekir:

* İlk kayıt defteri oluşturmak ve ACR görevleri yürütmek için kullanın. Bu makalede, bu kaynak kayıt defteri adlı *myregistry*. 
* İkinci ve üçüncü kayıt defterleri yapıların görüntü gönderebilmeniz ilk örnek görev için hedef kayıt defterleri ' dir. Bu makalede, hedef kayıt defterleri adlandırılır *customregistry1* ve *customregistry2*.

Sonraki adımlarda kendi kayıt defteri adıyla değiştirin.

Gerekli Azure kapsayıcısı kayıt defterleri yoksa bkz [hızlı başlangıç: Azure CLI kullanarak bir özel kapsayıcı kayıt defteri oluşturma](container-registry-get-started-azure-cli.md). Registry'ye görüntüleri gönderme, henüz gerek yoktur.

## <a name="example-task-with-a-system-assigned-identity"></a>Örnek: Sistem tarafından atanan bir kimlikle görevi

Bu örnek nasıl oluşturulacağını gösterir bir [çok adımlı görev](container-registry-tasks-multi-step.md) sistem tarafından atanan bir kimlikle. Görev, bir görüntü oluşturur ve görüntü gönderebilmeniz için iki hedef kayıt defterleri ile kimlik doğrulaması için kimlik'i kullanır.

Bu örnek görev için adımları tanımlanan bir [YAML dosyası](container-registry-tasks-reference-yaml.md) adlı `testtask.yaml`. Dosya multipleRegistries dizininde bulunan [acr görevleri](https://github.com/Azure-Samples/acr-tasks) depo örnekleri. Dosya burada tekrar üretilmektedir:

```yml
version: v1.0.0
steps:
  - build: -t {{.Values.REGISTRY1}}/hello-world:{{.Run.ID}} . -f hello-world.dockerfile
  - push: ["{{.Values.REGISTRY1}}/hello-world:{{.Run.ID}}"]
  - build: -t {{.Values.REGISTRY2}}/hello-world:{{.Run.ID}} . -f hello-world.dockerfile
  - push: ["{{.Values.REGISTRY2}}/hello-world:{{.Run.ID}}"]
```

### <a name="create-task-with-system-assigned-identity"></a>Sistem tarafından atanan kimlikle görev oluşturma

Görev oluşturma *birden çok kayıt defteri* aşağıdaki yürüterek [az acr görev oluşturma] [ az-acr-task-create] komutu. Görev bağlamı multipleRegistries örnekleri deponun bir klasördür ve komut dosyası başvuruları `testtask.yaml` depodaki. `--assign-identity` Parametresi ek değer içermeyen bir görev için sistem tarafından atanan kimliği oluşturur. Bu görev, el ile tetiklemek sahip olduğunuz, ancak bunu depoya yürütmelere veya bir çekme isteğinde yapılan çalıştırılacak ayarlama böylece ayarlanır. 

```azurecli
az acr task create \
  --registry myregistry \
  --name multiple-reg \
  --context https://github.com/Azure-Samples/acr-tasks.git#:multipleRegistries \
  --file testtask.yaml \
  --commit-trigger-enabled false \
  --pull-request-trigger-enabled false \
  --assign-identity
```

Komut çıktısında `identity` bölüm gösterir bir kimlik türü `SystemAssigned` görevde ayarlanır. `principalId` Kimliğini hizmet asıl kimliği:

```console
[...]
  "identity": {
    "principalId": "xxxxxxxx-2703-42f9-97d0-xxxxxxxxxxxx",
    "tenantId": "xxxxxxxx-86f1-41af-91ab-xxxxxxxxxxxx",
    "type": "SystemAssigned",
    "userAssignedIdentities": null
  },
  "location": "eastus",
[...]
``` 

Kullanma [az acr görev Göster] [ az-acr-task-show] depolamak için komut `principalId` değişkeninde daha sonraki komutlarda kullanmak için:

```azurecli
principalID=$(az acr task show --name multiple-reg --registry myregistry --query identity.principalId --output tsv)
```

### <a name="give-identity-push-permissions-to-two-target-container-registries"></a>İki hedef kapsayıcı kayıt defterleri için kimlik gönderme izni verin

Bu bölümde, sistem tarafından atanan kimlik adlı iki hedef kayıt defterlerinde gönderme izni vermek *customregistry1* ve *customregistry2*.

İlk olarak [az acr show] [ az-acr-show] her kayıt defteri kaynak Kimliğini alın ve değişkenleri kimliklerini depolamak için komut:

```azurecli
reg1_id=$(az acr show --name customregistry1 --query id --output tsv)
reg2_id=$(az acr show --name customregistry2 --query id --output tsv)
```

Kullanım [az rol ataması oluşturma] [ az-role-assignment-create] kimliği atamak için komutu `acrpush` her kayıt için rol. Bu rol, bir kapsayıcı kayıt defterine görüntü çekme ve itme izinlere sahip.

```azurecli
az role assignment create --assignee $principalID --scope $reg1_id --role acrpush
az role assignment create --assignee $principalID --scope $reg2_id --role acrpush
```

### <a name="add-target-registry-credentials-to-task"></a>Görev hedef kayıt defteri kimlik bilgilerini ekleyin

Artık [az acr görev kimlik bilgileri ekleme] [ az-acr-task-credential-add] ile hem de hedef kayıt defterleri gerçekleştirebilmesi göreve kimliğin kimlik bilgilerini eklemek için komutu.

```azurecli
az acr task credential add \
  --name multiple-reg \
  --registry myregistry \
  --login-server customregistry1.azurecr.io \
  --use-identity [system]

az acr task credential add \
  --name multiple-reg \
  --registry myregistry \
  --login-server customregistry2.azurecr.io \
  --use-identity [system]
```

### <a name="manually-run-the-task"></a>El ile görevi çalıştır

Kullanım [az acr görevi Çalıştır] [ az-acr-task-run] el ile tetikleyici için komutu. `--set` Parametre, iki hedef kayıt defterleri oturum açma sunucusu adını olarak görev değişkenlerin değerlerini geçirmek için kullanılır `REGISTRY1` ve `REGISTRY2`.

```azurecli
az acr task run \
  --name multiple-reg \
  --registry myregistry \
  --set REGISTRY1=customregistry1.azurecr.io \
  --set REGISTRY2=customregistry2.azurecr.io
```

Çıkış şuna benzer olacaktır:

```console
Sending build context to Docker daemon  4.096kB
Step 1/1 : FROM hello-world
 ---> fce289e99eb9
Successfully built fce289e99eb9
Successfully tagged customregistry2.azurecr.io/hello-world:cf31
2019/05/31 22:16:02 Successfully executed container: acb_step_0
2019/05/31 22:16:02 Executing step ID: acb_step_1. Timeout(sec): 600, Working directory: 'multipleRegistries', Network: 'acb_default_network'
2019/05/31 22:16:02 Pushing image: customregistry2.azurecr.io/hello-world:cf31, attempt 1
The push refers to repository [customregistry2.azurecr.io/hello-world]
af0b15c8625b: Preparing
af0b15c8625b: Pushed
cf31: digest: sha256:92c7f9c92844bbbb5d0a101b22f7c2a7949e40f8ea90c8b3bc396879d95e899a size: 524
2019/05/31 22:16:08 Successfully pushed image: customregistry2.azurecr.io/hello-world:cf31
2019/05/31 22:16:08 Executing step ID: acb_step_2. Timeout(sec): 600, Working directory: 'multipleRegistries', Network: 'acb_default_network'
2019/05/31 22:16:08 Scanning for dependencies...
2019/05/31 22:16:08 Successfully scanned dependencies
2019/05/31 22:16:08 Launching container with name: acb_step_2
Sending build context to Docker daemon  4.096kB
Step 1/1 : FROM hello-world
 ---> fce289e99eb9
Successfully built fce289e99eb9
Successfully tagged customregistry1.azurecr.io/hello-world:cf31
2019/05/31 22:16:09 Successfully executed container: acb_step_2
2019/05/31 22:16:09 Executing step ID: acb_step_3. Timeout(sec): 600, Working directory: 'multipleRegistries', Network: 'acb_default_network'
2019/05/31 22:16:09 Pushing image: customregistry1.azurecr.io/hello-world:cf31, attempt 1
The push refers to repository [customregistry1.azurecr.io/hello-world]
af0b15c8625b: Preparing
af0b15c8625b: Pushed
cf31: digest: sha256:92c7f9c92844bbbb5d0a101b22f7c2a7949e40f8ea90c8b3bc396879d95e899a size: 524
2019/05/31 22:16:21 Successfully pushed image: customregistry1.azurecr.io/hello-world:cf31
2019/05/31 22:16:21 Step ID: acb_step_0 marked as successful (elapsed time in seconds: 1.985291)
2019/05/31 22:16:21 Populating digests for step ID: acb_step_0...
2019/05/31 22:16:22 Successfully populated digests for step ID: acb_step_0
2019/05/31 22:16:22 Step ID: acb_step_1 marked as successful (elapsed time in seconds: 5.743225)
2019/05/31 22:16:22 Step ID: acb_step_2 marked as successful (elapsed time in seconds: 1.925959)
2019/05/31 22:16:22 Populating digests for step ID: acb_step_2...
2019/05/31 22:16:24 Successfully populated digests for step ID: acb_step_2
2019/05/31 22:16:24 Step ID: acb_step_3 marked as successful (elapsed time in seconds: 11.061057)
2019/05/31 22:16:24 The following dependencies were found:
2019/05/31 22:16:24
- image:
    registry: customregistry2.azurecr.io
    repository: hello-world
    tag: cf31
    digest: sha256:92c7f9c92844bbbb5d0a101b22f7c2a7949e40f8ea90c8b3bc396879d95e899a
  runtime-dependency:
    registry: registry.hub.docker.com
    repository: library/hello-world
    tag: latest
    digest: sha256:6f744a2005b12a704d2608d8070a494ad1145636eeb74a570c56b94d94ccdbfc
  git:
    git-head-revision: 05275dca2bc61f584085ca913c39d509236f576b
- image:
    registry: customregistry1.azurecr.io
    repository: hello-world
    tag: cf31
    digest: sha256:92c7f9c92844bbbb5d0a101b22f7c2a7949e40f8ea90c8b3bc396879d95e899a
  runtime-dependency:
    registry: registry.hub.docker.com
    repository: library/hello-world
    tag: latest
    digest: sha256:6f744a2005b12a704d2608d8070a494ad1145636eeb74a570c56b94d94ccdbfc
  git:
    git-head-revision: 05275dca2bc61f584085ca913c39d509236f576b

Run ID: cf31 was successful after 35s
```

## <a name="example-task-with-a-user-assigned-identity"></a>Örnek: Bir kullanıcı tarafından atanan kimlikle görevi

Bu örnekte bir Azure anahtar kasasından gizli dizileri Okuma izinlerine sahip bir kullanıcı tarafından atanan kimliği oluşturma. Gizli dizi okur, görüntüyü oluşturur ve resim etiketi okumak için Azure CLI ile oturum açtığında çok adımlı bir görev için bu kimlik atadığınız.

### <a name="create-a-key-vault-and-store-a-secret"></a>Key vault oluşturma ve bir gizli

Gerekiyorsa, ilk olarak, adlı bir kaynak grubu oluşturma *myResourceGroup* içinde *eastus* aşağıdaki konum [az grubu oluşturma] [ az-group-create]komutu:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Kullanım [az keyvault oluşturma] [ az-keyvault-create] anahtar kasası oluşturma komutu. Benzersiz anahtar kasası adı belirttiğinizden emin olun. 

```azurecli-interactive
az keyvault create --name mykeyvault --resource-group myResourceGroup --location eastus
```

Bir örnek gizli dizi kullanarak anahtar kasası Store [az keyvault secret set] [ az-keyvault-secret-set] komutu:

```azurecli
az keyvault secret set \
  --name SampleSecret \
  --value "Hello ACR Tasks!" \
  --description ACRTasksecret  \
  --vault-name mykeyvault
```

Örneğin, özel bir görüntü çekebilirsiniz için özel bir Docker kayıt defteri ile kimlik doğrulaması için kimlik bilgilerini saklamak isteyebilirsiniz.

### <a name="create-an-identity"></a>Bir kimlik oluşturun

Adlı bir kimliği oluşturma *myACRTasksId* aboneliği kullanarak [az kimliği oluşturma] [ az-identity-create] komutu. Daha önce bir kapsayıcı kayıt defteri veya anahtar kasası veya farklı bir tane oluşturmak için kullanılan aynı kaynak grubunu kullanabilirsiniz.

```azurecli-interactive
az identity create --resource-group myResourceGroup --name myACRTasksId
```

Aşağıdaki adımlarda kimliğini yapılandırmak için kullanın [az kimlik show] [ az-identity-show] kimliğin kaynağı kimliği ve hizmet sorumlusu kimliği değişkenlerinde depolanacak komutu.

```azurecli
# Get resource ID of the user-assigned identity
resourceID=$(az identity show --resource-group myResourceGroup --name myACRTasksId --query id --output tsv)

# Get service principal ID of the user-assigned identity
principalID=$(az identity show --resource-group myResourceGroup --name myACRTasksId --query principalId --output tsv)
```

### <a name="grant-identity-access-to-keyvault-to-read-secret"></a>Gizli dizi okumak için keyvault kimlik erişimi verme

Aşağıdaki komutu çalıştırın [az keyvault set-policy] [ az-keyvault-set-policy] anahtar kasasında bir erişim ilkesi ayarlamak için komutu. Aşağıdaki örnekte, gizli dizileri anahtar kasasından almak kullanıcı tarafından atanan kimlik sağlar. Bu erişim, daha sonra bir çok adımlı görevi başarıyla çalışması için gereklidir.

```azurecli-interactive
 az keyvault set-policy --name mykeyvault --resource-group myResourceGroup --object-id $principalID --secret-permissions get
```

### <a name="grant-identity-reader-access-to-the-resource-group-for-registry"></a>Kayıt defteri için kaynak grubu kimlik okuyucu erişimi verme

Aşağıdaki komutu çalıştırın [az rol ataması oluşturma] [ az-role-assignment-create] kimlik bir okuyucu rolüne bu durumda kaynak kayıt içeren kaynak grubunu atamak için komutu. Bu rol, daha sonra bir çok adımlı görevi başarıyla çalışması için gereklidir.

```azurecli
az role assignment create --role reader --resource-group myResourceGroup --assignee $principalID
```

### <a name="create-task-with-user-assigned-identity"></a>Kullanıcı tarafından atanan kimlikle görev oluşturma

Şimdi oluşturmak bir [çok adımlı görev](container-registry-tasks-multi-step.md) ve kullanıcı tarafından atanan kimliği atayın. Bu örnek görev oluşturmak bir [YAML dosyası](container-registry-tasks-reference-yaml.md) adlı `managed-identities.yaml` bir yerel çalışma dizinindeki ve aşağıdaki içeriği yapıştırın. Anahtar kasası adı dosyasındaki anahtar kasanıza adı ile değiştirdiğinizden emin olun

```yml
version: v1.0.0
# Replace mykeyvault with the name of your key vault
secrets:
  - id: name
    keyvault: https://mykeyvault.vault.azure.net/secrets/SampleSecret
steps:
  # Verify that the task can access the secret in the key vault
  - cmd: bash -c 'if [ -z "$MY_SECRET" ]; then echo "Secret not resolved"; else echo "Secret resolved"; fi'
    env: 
      - MY_SECRET='{{.Secrets.name}}' 

  # Build/push the website image to source registry, using dockerfile in the Azure-Samples repo
  - cmd: docker build -t {{.Run.Registry}}/my-website:{{.Run.ID}} https://github.com/Azure-Samples/aci-helloworld.git
  - push: 
    - "{{.Run.Registry}}/my-website:{{.Run.ID}}"
  
  # Login to Azure CLI with identity and list the tags to verify that the image is in the registry
  - cmd: microsoft/azure-cli az login --identity
  - cmd: microsoft/azure-cli az acr repository show-tags -n {{.Values.registryName}} --repository my-website
```

Bu görevi, şunları yapar:

* Bu anahtar kasanızdaki gizli erişebildiğini doğrular. Bu adım gösterim amaçları içindir. Gerçek hayattaki bir senaryoda, özel bir Docker Hub deposunu erişmek için kimlik bilgilerini almak için bir görev adımı gerekebilir.
* Yapıları ve gönderim `mywebsite` kaynak kayıt defterine görüntü.
* Azure CLI'yı listesine oturum açtığı `my-website` etiketleri kaynak kayıt defterindeki görüntü.

Çağrılan bir görev oluşturma *msitask* ve daha önce oluşturduğunuz kullanıcı tarafından atanan kimliği kaynak kimliği geçirin. Bu örnek görevi oluşturulur `managed-identities.yaml` el ile tetiklemek sahip olduğunuz için yerel çalışma dizininizde kaydettiğiniz dosya.

```azurecli
az acr task create \
  --name msitask \
  --registry myregistry \
  --context /dev/null \
  --file managed-identities.yaml \
  --pull-request-trigger-enabled false \
  --commit-trigger-enabled false \
  --assign-identity $resourceID
```

Komut çıktısında `identity` bölüm gösterir bir kimlik türü `UserAssigned` görevde ayarlanır. `principalId` Kimliğini hizmet asıl kimliği:

```console
[...]
"identity": {
    "principalId": null,
    "tenantId": null,
    "type": "UserAssigned",
    "userAssignedIdentities": {
      "/subscriptions/xxxxxxxx-d12e-4760-9ab6-xxxxxxxxxxxx/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/myACRTasksId": {
        "clientId": "xxxxxxxx-f17e-4768-bb4e-xxxxxxxxxxxx",
        "principalId": "xxxxxxxx-1335-433d-bb6c-xxxxxxxxxxxx"
      }
[...]
```

### <a name="manually-run-the-task"></a>El ile görevi çalıştır

Kullanım [az acr görevi Çalıştır] [ az-acr-task-run] el ile tetikleyici için komutu. `--set` Parametresi, kaynak kayıt defteri adlarında göreve geçirmek için kullanılır:

```azurecli
az acr task run --name msitask --registry myregistry --set registryName=myregistry  
```

Gizli dizi Çözüldü, görüntünün başarıyla oluşturulmuş ve gönderildi ve resim etiketi kaynak kayıt defterinden okumak için Azure CLI kimlik ile oturum görev açtığı çıktı gösterilmektedir:

```console
Queued a run with ID: cf32
Waiting for an agent...
2019/06/10 23:25:37 Downloading source code...
2019/06/10 23:25:38 Finished downloading source code
2019/06/10 23:25:39 Using acb_vol_4e4a0a7c-b6ef-4ec5-b40f-3436fc5eb0f5 as the home volume
2019/06/10 23:25:41 Creating Docker network: acb_default_network, driver: 'bridge'
2019/06/10 23:25:41 Successfully set up Docker network: acb_default_network
2019/06/10 23:25:41 Setting up Docker configuration...
2019/06/10 23:25:42 Successfully set up Docker configuration
2019/06/10 23:25:42 Logging in to registry: myregistry.azurecr.io
2019/06/10 23:25:43 Successfully logged into myregistry.azurecr.io
2019/06/10 23:25:43 Executing step ID: acb_step_0. Timeout(sec): 600, Working directory: '', Network: 'acb_default_network'
2019/06/10 23:25:43 Launching container with name: acb_step_0
Secret resolved
2019/06/10 23:25:44 Successfully executed container: acb_step_0
2019/06/10 23:25:44 Executing step ID: acb_step_1. Timeout(sec): 600, Working directory: '', Network: 'acb_default_network'
2019/06/10 23:25:44 Launching container with name: acb_step_1
Sending build context to Docker daemon  74.75kB

[...]

cf32: digest: sha256:cbb4aa83b33f6959d83e84bfd43ca901084966a9f91c42f111766473dc977f36 size: 1577
2019/06/10 23:26:05 Successfully pushed image: myregistry.azurecr.io/my-website:cf32
2019/06/10 23:26:05 Executing step ID: acb_step_3. Timeout(sec): 600, Working directory: '', Network: 'acb_default_network'
2019/06/10 23:26:05 Launching container with name: acb_step_3
[
  {
    "environmentName": "AzureCloud",
    "id": "xxxxxxxx-d12e-4760-9ab6-xxxxxxxxxxxx",
    "isDefault": true,
    "name": "DanLep Internal Consumption",
    "state": "Enabled",
    "tenantId": "xxxxxxxx-86f1-41af-91ab-xxxxxxxxxxxx",
      "user": {
      "assignedIdentityInfo": "MSI",
      "name": "systemAssignedIdentity",
      "type": "servicePrincipal"
    }
  }
]
2019/06/10 23:26:09 Successfully executed container: acb_step_3
2019/06/10 23:26:09 Executing step ID: acb_step_4. Timeout(sec): 600, Working directory: '', Network: 'acb_default_network'
2019/06/10 23:26:09 Launching container with name: acb_step_4
[
  "cf32"
]
2019/06/10 23:26:14 Successfully executed container: acb_step_4
2019/06/10 23:26:14 Step ID: acb_step_0 marked as successful (elapsed time in seconds: 1.025312)
2019/06/10 23:26:14 Step ID: acb_step_1 marked as successful (elapsed time in seconds: 13.703823)
2019/06/10 23:26:14 Step ID: acb_step_2 marked as successful (elapsed time in seconds: 6.791506)
2019/06/10 23:26:14 Step ID: acb_step_3 marked as successful (elapsed time in seconds: 3.852972)
2019/06/10 23:26:14 Step ID: acb_step_4 marked as successful (elapsed time in seconds: 5.079079)
```


## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, yönetilen Azure kapsayıcı kayıt defteri görevlerle kimliklerle ilgili öğrendiniz ve nasıl yapılır:

> [!div class="checklist"]
> * Sistem tarafından atanan bir kimliğini etkinleştirme veya kullanıcı tarafından atanan bir ACR görevi
> * Kimlik gibi diğer Azure kapsayıcısı kayıt defterleri Azure kaynaklarına erişim
> * Bir görevden kaynaklara erişmek için yönetilen kimliği kullanma  

* Daha fazla bilgi edinin [kimliklerini Azure kaynakları için yönetilen](/azure/active-directory/managed-identities-azure-resources/).

<!-- LINKS - Internal -->
[az-login]: /cli/azure/reference-index#az-login
[az-acr-login]: /cli/azure/acr#az-acr-login
[az-acr-show]: /cli/azure/acr#az-acr-show
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
[az-acr-login]: /cli/azure/acr#az-acr-login
[az-identity-create]: /cli/azure/identity?view=azure-cli-latest#az-identity-create
[az-identity-show]: /cli/azure/identity#az-identity-show
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-task-create]: /cli/azure/acr/task#az-acr-task-create
[az-acr-task-show]: /cli/azure/acr/task#az-acr-task-show
[az-acr-task-run]: /cli/azure/acr/task#az-acr-task-run
[az-acr-task-list-runs]: /cli/azure/acr/task#az-acr-task-list-runs
[az-acr-task-credential-add]: /cli/azure/acr/task/credential#az-acr-task-credential-add
[az-group-create]: /cli/azure/group?#az-group-create
[az-keyvault-create]: /cli/azure/keyvault?#az-keyvault-create
[az-keyvault-secret-set]: /cli/azure/keyvault/secret#az-keyvault-secret-set
[az-keyvault-set-policy]: /cli/azure/keyvault#az-keyvault-set-policy
