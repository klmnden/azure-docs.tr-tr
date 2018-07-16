---
title: Öğretici - Azure Container Registry Derlemesi ile bulutta kapsayıcı görüntüleri derleme
description: Bu öğreticide, Azure’da Azure Container Registry Derlemesi (ACR Derlemesi) ile bir Docker kapsayıcı görüntüsü derleme, ardından bu görüntüyü Azure Container Instances’a dağıtma hakkında bilgi edineceksiniz.
services: container-registry
author: mmacy
manager: jeconnoc
ms.service: container-registry
ms.topic: tutorial
ms.date: 05/11/2018
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: d86c47c890fd15515c590e06b395ca82f9747ffe
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38461478"
---
# <a name="tutorial-build-container-images-in-the-cloud-with-azure-container-registry-build"></a>Öğretici: Azure Container Registry Derlemesi ile bulutta kapsayıcı görüntüleri derleme

**ACR Derlemesi**, Azure’da kolaylaştırılmış ve verimli Docker kapsayıcı görüntüsü derlemeleri sağlayan bir Azure Container Registry özellik paketidir. Bu makalede, ACR Derlemesinin *Hızlı Derleme* özelliğini kullanmayı öğreneceksiniz. Hızlı Derleme, geliştirme "iç döngünüzü" buluta genişleterek, derleme başarısını doğrulama ve başarıyla derlenen görüntüleri kapsayıcı kayıt defterinize otomatik olarak gönderme olanağı sağlar. Görüntüleriniz bulutta, kayıt defterinize yakın bir konumda yerel olarak derlenerek daha hızlı dağıtımı mümkün kılar.

Tüm Dockerfile uzmanlığınız ACR Derlemesi’ne doğrudan aktarılabilir. ACR Derlemesi ile bulutta derlemek için Dockerfile’ı değiştirmeniz gerekmez, yalnızca çalıştırdığınız komutu değiştirmeniz yeterlidir.

Dizinin birinci bölümü olan bu öğreticide şunları öğreneceksiniz:

> [!div class="checklist"]
> * Örnek uygulama kaynak kodunu alma
> * Azure’da kapsayıcı görüntüsü derleme
> * Azure Container Instances‘a kapsayıcı dağıtma

Sonraki öğreticilerde, ACR Derlemesi’nin kod işleme ve temel görüntü güncelleştirmesi üzerindeki otomatik kapsayıcı görüntüsü derlemelerine yönelik derleme görevlerini kullanmayı öğreneceksiniz.

[!INCLUDE [container-registry-build-preview-note](../../includes/container-registry-build-preview-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure CLI’yı yerel olarak kullanmak istiyorsanız Azure CLI **2.0.32** veya sonraki bir sürüm yüklü olmalıdır. Sürümü bulmak için `az --version` komutunu çalıştırın. CLI’yı yüklemeniz veya yükseltmeniz gerekiyorsa bkz. [Azure CLI’yı yükleme][azure-cli].

## <a name="prerequisites"></a>Ön koşullar

### <a name="github-account"></a>GitHub hesabı

Henüz yoksa https://github.com sayfasında bir hesap oluşturun. Bu öğretici serisinde, ACR Derlemesindeki otomatik görüntü derlemelerini göstermek için bir GitHub deposu kullanılmıştır.

### <a name="fork-sample-repository"></a>Örnek deponun çatalını oluşturma

Sonra, GitHub kullanıcı arabirimini kullanarak GitHub hesabınızda örnek deponun çatalını oluşturun. Bu öğreticide, depodaki kaynaktan bir kapsayıcı görüntüsü derleyecek, sonraki öğreticide ise otomatik bir derleme başlatmak üzere deponuzun çatalına bir işleme göndereceksiniz.

Şu deponun çatalını oluşturun: https://github.com/Azure-Samples/acr-build-helloworld-node

![GitHub’daki Çatal düğmesinin (vurgulanmış) ekran görüntüsü][quick-build-01-fork]

### <a name="clone-your-fork"></a>Çatalınızı kopyalama

Deponun çatalını oluşturduktan sonra çatalınızı kopyalayın ve yerel kopyanızı içeren dizine girin.

`git` ile depoyu kopyalayın, **\<your-github-username\>** değerini GitHub kullanıcı adınızla değiştirin:

```azurecli-interactive
git clone https://github.com/<your-github-username>/acr-build-helloworld-node
```

Kaynak kodunu içeren dizine girin:

```azurecli-interactive
cd acr-build-helloworld-node
```

### <a name="bash-shell"></a>Bash kabuğu

Bu öğretici serisindeki komutlar Bash kabuğu için biçimlendirilmiştir. PowerShell, Komut İstemi veya başka bir kabuk kullanmayı tercih ederseniz, satır devamlılığını ve ortam değişkeni biçimini uygun şekilde ayarlamanız gerekebilir.

## <a name="build-in-azure-with-acr-build"></a>ACR Derlemesi ile Azure'da Derleme

Kaynak kodunu makinenize çektikten sonra aşağıdaki adımları izleyerek bir kapsayıcı kayıt defteri oluşturun ve ACR Derlemesi ile kapsayıcı görüntüsünü derleyin.

Örnek komutları yürütmeyi kolaylaştırmak için, bu serideki öğreticilerde kabuk ortam değişkenleri kullanılmıştır. `ACR_NAME` değişkenini ayarlamak için aşağıdaki komutu yürütün. **\<registry-name\>** değerini yeni kapsayıcı kayıt defterinizin benzersiz adıyla değiştirin. Kaynak defteri adı Azure’da benzersiz olmalı ve 5-50 arası alfasayısal karakter içermelidir. Bu öğreticide oluşturduğunuz diğer kaynaklar bu adı temel alır; bu nedenle yalnızca bu ilk değişkeni değiştirmeniz gerekir.

```azurecli-interactive
ACR_NAME=<registry-name>
```

Kapsayıcı kayıt defteri ortam değişkeni doldurulduğunda, herhangi bir değeri düzenlemeden öğreticideki komutların geri kalanını kopyalayıp yapıştırabilirsiniz. Bir kaynak grubu ve kapsayıcı kayıt defteri oluşturmak için aşağıdaki komutları yürütün:

```azurecli-interactive
RES_GROUP=$ACR_NAME # Resource Group name

az group create --resource-group $RES_GROUP --location eastus
az acr create --resource-group $RES_GROUP --name $ACR_NAME --sku Standard --location eastus
```

Bir kayıt defteri oluşturduktan sonra ACR Derlemesi’ni kullanarak örnek koddan bir kapsayıcı görüntüsü derleyebilirsiniz. Bir *Hızlı Derleme* gerçekleştirmek için [az acr build][az-acr-build] komutunu yürütün:

```azurecli-interactive
az acr build --registry $ACR_NAME --image helloacrbuild:v1 .
```

[az acr build][az-acr-build] komutunun çıktısı aşağıdakine benzer. Kaynak kodun Azure’a yüklenme durumunu ("bağlam") ve ACR Derlemesi’nin bulutta çalıştırdığı `docker build` işlemini görebilirsiniz. ACR Derlemesi görüntülerinizi derlemek için `docker build` kullandığından, ACR Derlemesi’ni kullanmaya hemen başlamak üzere Dockerfile üzerinde bir değişiklik yapılması gerekmez.

```console
$ az acr build --registry $ACR_NAME --image helloacrbuild:v1 .
Sending build context (4.909 KiB) to ACR.
Queued a build with build ID: aa1
Waiting for a build agent...
Sending build context to Docker daemon  22.53kB
Step 1/5 : FROM node:9-alpine
9-alpine: Pulling from library/node
Digest: sha256:5149aec8f508d48998e6230cdc8e6832cba192088b442c8ef7e23df3c6892cd3
Status: Image is up to date for node:9-alpine
 ---> 7af437a39ec2
Step 2/5 : COPY . /src
 ---> 0c4814714938
Step 3/5 : RUN cd /src && npm install
 ---> Running in 4f77ce7b330d
npm notice created a lockfile as package-lock.json. You should commit this file.
npm WARN helloworld@1.0.0 No repository field.

up to date in 0.096s
Removing intermediate container 4f77ce7b330d
 ---> e0eb24339e07
Step 4/5 : EXPOSE 80
 ---> Running in 872bd29edbc7
Removing intermediate container 872bd29edbc7
 ---> 22ba8d8ffb4e
Step 5/5 : CMD ["node", "/src/server.js"]
 ---> Running in 1a40c05c4122
Removing intermediate container 1a40c05c4122
 ---> 0a9a4b74fb53
Successfully built 0a9a4b74fb53
Successfully tagged mycontainerregistry.azurecr.io/helloacrbuild:v1
time="2018-05-10T19:10:20Z" level=info msg="Running command docker push mycontainerregistry.azurecr.io/helloacrbuild:v1"
The push refers to repository [mycontainerregistry.azurecr.io/helloacrbuild]
d2b301f7ef94: Preparing
d0e0f2bb8747: Preparing
26b0c207c4a9: Preparing
917e7cdebc8b: Preparing
9dfa40a0da3b: Preparing
26b0c207c4a9: Pushed
d2b301f7ef94: Pushed
d0e0f2bb8747: Pushed
9dfa40a0da3b: Pushed
917e7cdebc8b: Pushed
v1: digest: sha256:78d7980b4c80a078192bd4749c27eeae56421079606ed7b7d8ae84dbb04193fd size: 1366
time="2018-05-10T19:11:07Z" level=info msg="Running command docker inspect --format \"{{json .RepoDigests}}\" mycontainerregistry.azurecr.io/helloacrbuild:v1"
"["mycontainerregistry.azurecr.io/helloacrbuild@sha256:78d7980b4c80a078192bd4749c27eeae56421079606ed7b7d8ae84dbb04193fd"]"
time="2018-05-10T19:11:07Z" level=info msg="Running command docker inspect --format \"{{json .RepoDigests}}\" node:9-alpine"
"["node@sha256:5149aec8f508d48998e6230cdc8e6832cba192088b442c8ef7e23df3c6892cd3"]"
ACR Builder discovered the following dependencies:
- image:
    registry: mycontainerregistry.azurecr.io
    repository: helloacrbuild
    tag: v1
    digest: sha256:78d7980b4c80a078192bd4749c27eeae56421079606ed7b7d8ae84dbb04193fd
  runtime-dependency:
    registry: registry.hub.docker.com
    repository: library/node
    tag: 9-alpine
    digest: sha256:5149aec8f508d48998e6230cdc8e6832cba192088b442c8ef7e23df3c6892cd3

Build complete
Build ID: aa1 was successful after 52.522222729s
```

Çıktının sonunda ACR Derlemesi, görüntünüz için bulunan bağımlılıkları gösterir. Bunun yapılması, bir temel görüntünün işletim sistemi veya çerçeve yaması ile güncelleştirilmesi gibi durumlarda temel görüntü güncelleştirmelerinde görüntü derlemelerini otomatik hale getirmeyi sağlar. Bu öğretici serisinin sonraki bölümlerinde ACR Derlemesi’nin temel görüntü desteği hakkında bilgi edineceksiniz.

## <a name="deploy-to-azure-container-instances"></a>Azure Container Instances’a dağıtma

Varsayılan olarak ACR Derlemesi, başarıyla derlenen görüntüleri otomatik olarak kayıt defterinize gönderir ve kayıt defterinden hemen dağıtmanıza olanak tanır.

Bu bölümde, bir Azure Anahtar Kasası ve hizmet sorumlusu oluşturacak, ardından hizmet sorumlusunun kimlik bilgilerini kullanarak kapsayıcıyı Azure Container Instances (ACI) hizmetine dağıtacaksınız.

### <a name="configure-registry-authentication"></a>Kayıt defteri kimlik doğrulamasını yapılandırma

Tüm üretim senaryoları bir Azure kapsayıcı kayıt defterine erişmek için [hizmet sorumluları][service-principal-auth] kullanmalıdır. Hizmet sorumluları, kapsayıcı görüntüleriniz için rol tabanlı erişim denetimi sağlamanıza olanak tanır. Örneğin, bir hizmet sorumlusunu bir kayıt defterine yalnızca çekme erişimiyle yapılandırabilirsiniz.

#### <a name="create-key-vault"></a>Anahtar kasası oluşturma

[Azure Key Vault](/azure/key-vault/) içinde henüz bir kasanız yoksa, aşağıdaki komutları kullanarak Azure CLI ile bir kasa oluşturun.

```azurecli-interactive
AKV_NAME=$ACR_NAME-vault

az keyvault create --resource-group $RES_GROUP --name $AKV_NAME
```

#### <a name="create-service-principal-and-store-credentials"></a>Hizmet sorumlusu oluşturma ve kimlik bilgilerini depolama

Şimdi bir hizmet sorumlusu oluşturup kimlik bilgilerini anahtar kasanızda depolamanız gerekiyor.

[az ad sp create-for-rbac][az-ad-sp-create-for-rbac] komutunu kullanarak hizmet sorumlusunu oluşturun ve [az keyvault secret set][az-keyvault-secret-set] komutunu kullanarak hizmet sorumlusunun **parolasını** kasanızda depolayın:

```azurecli-interactive
# Create service principal, store its password in AKV (the registry *password*)
az keyvault secret set \
  --vault-name $AKV_NAME \
  --name $ACR_NAME-pull-pwd \
  --value $(az ad sp create-for-rbac \
                --name $ACR_NAME-pull \
                --scopes $(az acr show --name $ACR_NAME --query id --output tsv) \
                --role reader \
                --query password \
                --output tsv)
```

Önceki komutta yer alan `--role` bağımsız değişkeni, kayıt defterine yalnızca çekme erişimi veren *reader* rolü ile hizmet sorumlusunu yapılandırır. Hem gönderme hem de çekme erişimi vermek için `--role` bağımsız değişkenini *katkıda bulunan* olarak değiştirin.

Ardından, hizmet sorumlusunun *uygulama kimliğini* kasada depolayın. Bu değer, kimlik doğrulaması için Azure Container Registry’ye geçirdiğiniz **kullanıcı adıdır**:

```azurecli-interactive
# Store service principal ID in AKV (the registry *username*)
az keyvault secret set \
    --vault-name $AKV_NAME \
    --name $ACR_NAME-pull-usr \
    --value $(az ad sp show --id http://$ACR_NAME-pull --query appId --output tsv)
```

Bir Azure Anahtar Kasası oluşturdunuz ve içinde iki gizli dizi depoladınız:

* `$ACR_NAME-pull-usr`: Kapsayıcı kayıt defterinin **kullanıcı adı** olarak kullanılacak hizmet sorumlusu kimliği.
* `$ACR_NAME-pull-pwd`: Kapsayıcı kayıt defterinin **parolası** olarak kullanılacak hizmet sorumlusu parolası.

Artık siz veya uygulamalarınız ve hizmetleriniz kayıt defterinden görüntüleri çektiğinde bu gizli dizilere ada göre başvurabilirsiniz.

### <a name="deploy-container-with-azure-cli"></a>Azure CLI ile kapsayıcı dağıtma

Hizmet sorumlusu kimlik bilgileri Azure Key Vault gizli dizileri olarak depolandıktan sonra, uygulamalarınız ve hizmetleriniz bu kimlik bilgilerini kullanarak özel kayıt defterinize erişebilir.

Bir kapsayıcı örneği dağıtmak için aşağıdaki [az container create][az-container-create] komutunu yürütün. Komut, kapsayıcı kayıt defterinizde kimlik doğrulaması yapmak için hizmet sorumlusunun Azure Key Vault’ta depolanmış kimlik bilgilerini kullanır.

```azurecli-interactive
az container create \
    --resource-group $RES_GROUP \
    --name acr-build \
    --image $ACR_NAME.azurecr.io/helloacrbuild:v1 \
    --registry-login-server $ACR_NAME.azurecr.io \
    --registry-username $(az keyvault secret show --vault-name $AKV_NAME --name $ACR_NAME-pull-usr --query value -o tsv) \
    --registry-password $(az keyvault secret show --vault-name $AKV_NAME --name $ACR_NAME-pull-pwd --query value -o tsv) \
    --dns-name-label acr-build-$ACR_NAME \
    --query "{FQDN:ipAddress.fqdn}" \
    --output table
```

`--dns-name-label` değeri Azure içinde benzersiz olmalıdır; bu nedenle, yukarıdaki komut kapsayıcı kayıt defterinizin adını kapsayıcının DNS ad etiketine ekler. Komutun çıktısı, kapsayıcının tam etki alanı adını (FQDN) gösterir, örneğin:

```console
$ az container create \
>     --resource-group $RES_GROUP \
>     --name acr-build \
>     --image $ACR_NAME.azurecr.io/helloacrbuild:v1 \
>     --registry-login-server $ACR_NAME.azurecr.io \
>     --registry-username $(az keyvault secret show --vault-name $AKV_NAME --name $ACR_NAME-pull-usr --query value -o tsv) \
>     --registry-password $(az keyvault secret show --vault-name $AKV_NAME --name $ACR_NAME-pull-pwd --query value -o tsv) \
>     --dns-name-label acr-build-$ACR_NAME \
>     --query "{FQDN:ipAddress.fqdn}" \
>     --output table
FQDN
-------------------------------------------
acr-build-1175.eastus.azurecontainer.io
```

Kapsayıcının FQDN'sini not alın, sonraki bölümde kullanacaksınız.

### <a name="verify-deployment"></a>Dağıtımı doğrulama

Kapsayıcının başlangıç işlemini izlemek için [az container attach][az-container-attach] komutunu kullanın:

```azurecli-interactive
az container attach --resource-group $RES_GROUP --name acr-build
```

`az container attach` çıktısı ilk olarak görüntüyü çekip başlatılırken kapsayıcının durumunu gösterir, ardından yerel konsolunuzun STDOUT ve STDERR değerlerini kapsayıcının değerlerine bağlar.

```console
$ az container attach --resource-group $RES_GROUP --name acr-build
Container 'acr-build' is in state 'Waiting'...
Container 'acr-build' is in state 'Running'...
(count: 1) (last timestamp: 2018-04-03 19:45:37+00:00) pulling image "mycontainerregistry.azurecr.io/helloacrbuild:v1"
(count: 1) (last timestamp: 2018-04-03 19:45:44+00:00) Successfully pulled image "mycontainerregistry.azurecr.io/helloacrbuild:v1"
(count: 1) (last timestamp: 2018-04-03 19:45:44+00:00) Created container with id 094ab4da40138b36ca15fc2ad5cac351c358a7540a32e22b52f78e96a4cb3413
(count: 1) (last timestamp: 2018-04-03 19:45:44+00:00) Started container with id 094ab4da40138b36ca15fc2ad5cac351c358a7540a32e22b52f78e96a4cb3413

Start streaming logs:
Server running at http://localhost:80
```

`Server running at http://localhost:80` göründüğünde, çalışan uygulamayı görmek için tarayıcınızda kapsayıcının FQDN’sine gidin:

![Tarayıcıda oluşturulan örnek uygulamanın ekran görüntüsü][quick-build-02-browser]

Konsolunuzu kapsayıcıdan ayırmak için `Control+C` öğesine dokunun.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kapsayıcı örneğini [az container delete][az-container-delete] komutu ile durdurun:

```azurecli-interactive
az container delete --resource-group $RES_GROUP --name acr-build
```

Kapsayıcı kayıt defteri, anahtar kasası ve hizmet sorumlusu dahil olmak üzere bu öğreticide oluşturduğunuz *tüm* kaynakları kaldırmak için aşağıdaki komutları verin. Bu kaynaklar, serinin [sonraki öğreticisinde](container-registry-tutorial-build-task.md) kullanılacaktır ancak doğrudan sonraki öğreticiye geçecekseniz bunları tutmak isteyebilirsiniz.

```azurecli-interactive
az group delete --resource-group $RES_GROUP
az ad sp delete --id http://$ACR_NAME-pull
```

## <a name="next-steps"></a>Sonraki adımlar

Hızlı bir derleme ile iç döngünüzü test ettikten sonra, kaynak kodunu bir Git deposuna işlediğinizde kapsayıcı görüntüsü derlemelerini tetikleyecek bir **derleme görevi** yapılandırın:

> [!div class="nextstepaction"]
> [Derleme görevleri ile otomatik derlemeler tetikleme](container-registry-tutorial-build-task.md)

<!-- LINKS - External -->
[sample-archive]: https://github.com/Azure-Samples/acr-build-helloworld-node/archive/master.zip

<!-- LINKS - Internal -->
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-build]: /cli/azure/acr#az-acr-build
[az-ad-sp-create-for-rbac]: /cli/azure/ad/sp#az-ad-sp-create-for-rbac
[az-container-attach]: /cli/azure/container#az-container-attach
[az-container-create]: /cli/azure/container#az-container-create
[az-container-delete]: /cli/azure/container#az-container-delete
[az-keyvault-create]: /cli/azure/keyvault/secret#az-keyvault-create
[az-keyvault-secret-set]: /cli/azure/keyvault/secret#az-keyvault-secret-set
[service-principal-auth]: container-registry-auth-service-principal.md

<!-- IMAGES -->
[quick-build-01-fork]: ./media/container-registry-tutorial-quick-build/quick-build-01-fork.png
[quick-build-02-browser]: ./media/container-registry-tutorial-quick-build/quick-build-02-browser.png
