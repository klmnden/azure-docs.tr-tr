---
title: Öğretici - Azure Container Registry Derlemesi ile kapsayıcı görüntüsü derlemelerini otomatik hale getirme
description: Bu öğreticide, bir Git deposuna kaynak kodu işlediğinizde bulutta kapsayıcı görüntü derlemelerini otomatik olarak tetiklemek üzere bir derleme görevi yapılandırmayı öğreneceksiniz.
services: container-registry
author: mmacy
manager: jeconnoc
ms.service: container-registry
ms.topic: tutorial
ms.date: 05/07/2018
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: fba499441d092f4dce09d13d607dfc5de65d98b2
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="tutorial-automate-container-image-builds-with-azure-container-registry-build"></a>Öğretici: Azure Container Registry Derlemesi ile kapsayıcı görüntüsü derlemelerini otomatik hale getirme

ACR Derlemesi, [Hızlı Derleme](container-registry-tutorial-quick-build.md)’ye ek olarak *derleme görevi* ile otomatik Docker kapsayıcı görüntüsü derlemesini destekler. Bu öğreticide, bir Git deposuna kaynak kodu işlediğinizde bulutta görüntü derlemelerini otomatik olarak tetikleyen bir derleme görevi oluşturmak için Azure CLI kullanacaksınız.

Bu öğreticide, serinin ikinci kısmı:

> [!div class="checklist"]
> * Derleme görevi oluşturma
> * Derleme görevini test etme
> * Derleme durumunu görüntüleme
> * Kod işlemesi ile derleme görevini tetikleme

Bu öğreticide, [önceki öğreticide](container-registry-tutorial-quick-build.md) yer alan adımları zaten tamamladığınız varsayılır. Henüz yapmadıysanız, devam etmeden önce önceki öğreticinin [Önkoşullar](container-registry-tutorial-quick-build.md#prerequisites) bölümündeki adımları tamamlayın.

> [!IMPORTANT]
> ACR Derlemesi şu anda önizleme aşamasındadır ve yalnızca **Doğu ABD** (eastus) ve **Batı Avrupa** (westeurope) bölgelerindeki Azure kapsayıcı kayıt defterleri tarafından desteklenir. Önizlemeler, [ek kullanım koşullarını][terms-of-use] kabul etmeniz şartıyla kullanımınıza sunulur. Bu özelliğin bazı yönleri genel kullanıma açılmadan önce değişebilir.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure CLI’yı yerel olarak kullanmak istiyorsanız Azure CLI **2.0.32** veya sonraki bir sürüm yüklü olmalıdır. Sürümü bulmak için `az --version` komutunu çalıştırın. CLI’yı yüklemeniz veya yükseltmeniz gerekiyorsa bkz. [Azure CLI 2.0’ı yükleme][azure-cli].

## <a name="prerequisites"></a>Ön koşullar

### <a name="get-sample-code"></a>Örnek kodu alma

Bu öğreticide, [önceki öğreticide](container-registry-tutorial-quick-build.md) yer alan adımları zaten tamamladığınız ve örnek deponun çatalını ve kopyasını oluşturduğunuz varsayılır. Henüz yapmadıysanız, devam etmeden önce önceki öğreticinin [Önkoşullar](container-registry-tutorial-quick-build.md#prerequisites) bölümündeki adımları tamamlayın.

### <a name="container-registry"></a>Kapsayıcı kayıt defteri

Bu öğreticiyi tamamlamak için Azure aboneliğinizde bir Azure kapsayıcı kayıt defteri olması gerekir. Bir kayıt defterine ihtiyacınız varsa, [önceki öğreticiye](container-registry-tutorial-quick-build.md) veya [Hızlı Başlangıç: Azure CLI kullanarak kapsayıcı kayıt defteri oluşturma](container-registry-get-started-azure-cli.md) bölümüne bakın.

## <a name="build-task"></a>Derleme görevi

Derleme görevi, kapsayıcı görüntüsü kaynak kodunun konumu ve derlemeyi tetikleyen olay gibi otomatik bir derlemenin özelliklerini tanımlar. Git deposuna işleme gibi derleme görevinde tanımlanan bir olay gerçekleştiğinde, ACR Derlemesi bulutta bir kapsayıcı görüntüsü derlemesi başlatır. Varsayılan olarak, başarıyla derlenmiş bir görüntüyü daha sonra görevde belirtilen Azure kapsayıcı kayıt defterine gönderir.

ACR Derlemesi şu anda aşağıdaki derleme görevi tetikleyicilerini destekler:

* Git deposuna işleme
* Temel görüntü güncelleştirme

## <a name="create-a-build-task"></a>Derleme görevi oluşturma

Bu bölümde ilk olarak ACR Derlemesi ile birlikte kullanılacak bir GitHub kişisel erişim belirteci (PAT) oluşturacaksınız. Daha sonra, deponuzun çatalına kod işlendiğinde derlemeyi tetikleyen bir derleme görevi oluşturacaksınız.

### <a name="create-a-github-personal-access-token"></a>GitHub kişisel erişim belirteci oluşturma

Git deposuna işleme sonrasında bir derleme tetiklemek için, ACR Derlemesinin depoya erişmek üzere bir kişisel erişim belirtecine (PAT) sahip olması gerekir. Github'da bir PAT oluşturmak için aşağıdaki adımları izleyin:

1. GitHub üzerinde https://github.com/settings/tokens/new adresindeki PAT oluşturma sayfasında gidin
1. Belirteç için kısa bir **açıklama** girin; örneğin, "ACR Derleme Görevi Tanıtımı"
1. **Depo** altında **depo:durum** seçeneğini ve **genel_depo** seçeneklerini etkinleştirin

   ![GitHub'da Kişisel Erişim Belirteci oluşturma sayfasının ekran görüntüsü][build-task-01-new-token]

1. **Belirteç Oluştur** düğmesini seçin (parolanızı onaylamanız istenebilir)
1. Oluşturulan belirteci kopyalayın ve **güvenli bir konuma** kaydedin (bu belirteci sonraki bölümde bir derleme görevi tanımlarken kullanacaksınız)

   ![GitHub'da oluşturulan Kişisel Erişim Belirtecinin ekran görüntüsü][build-task-02-generated-token]

### <a name="create-the-build-task"></a>Derleme görevi oluşturma

ACR Derlemesinin işleme durumunu okumasını etkinleştirmek ve bir depoda web kancaları oluşturmak için gereken adımları tamamladıktan sonra, depoya işleme yapılması üzerine kapsayıcı görüntüsü derlemesini tetikleyen bir derleme görevi oluşturabilirsiniz.

İlk olarak, bu kabuk ortam değişkenlerini ortamınıza uygun değerlerle doldurun. Bunun yapılması kesinlikle zorunlu değildir ancak bu öğreticideki çok satırlı Azure CLI komutlarını yürütmeyi biraz daha kolaylaştırır. Bu alanları doldurmazsanız her değeri, örnek komutlarda her göründükleri durumda el ile değiştirmeniz gerekir.

```azurecli-interactive
ACR_NAME=mycontainerregistry # The name of your Azure container registry
GIT_USER=gituser             # Your GitHub user account name
GIT_PAT=personalaccesstoken  # The PAT you generated in the previous section
```

Şimdi aşağıdaki [az acr build-task create][az-acr-build-task-create] komutunu yürüterek derleme görevini oluşturun:

```azurecli-interactive
az acr build-task create \
    --registry $ACR_NAME \
    --name buildhelloworld \
    --image helloworld:{{.Build.ID}} \
    --context https://github.com/$GIT_USER/acr-build-helloworld-node \
    --branch master \
    --git-access-token $GIT_PAT
```

Bu derleme görevi, `--context` ile belirtilen depodaki *ana* dala kod işlenen her durumda ACR Derlemesinin söz konusu daldaki koddan kapsayıcı görüntüsü derleyeceğini belirtir. `--image` bağımsız değişkeni, görüntü etiketinin sürüm kısmı için parametreli `{{.Build.ID}}` değeri belirtir ve derlenen görüntünün belirli bir derleme ile ilişkili olmasını ve benzersiz şekilde etiketlenmesini sağlar.

Başarılı bir [az acr build-task create][az-acr-build-task-create] komutundaki çıktı aşağıdakilere benzer:

```console
$ az acr build-task create \
>     --registry $ACR_NAME \
>     --name buildhelloworld \
>     --image helloworld:{{.Build.ID}} \
>     --context https://github.com/$GIT_USER/acr-build-helloworld-node \
>     --branch master \
>     --git-access-token $GIT_PAT
{
  "additionalProperties": {},
  "alias": "buildhelloworld",
  "creationDate": "2018-04-18T23:14:45.905395+00:00",
  "id": "/subscriptions/<subscriptionID>/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/mycontainerregistry/buildTasks/buildhelloworld",
  "location": "eastus",
  "name": "buildhelloworld",
  "platform": {
    "additionalProperties": {},
    "cpu": 1,
    "osType": "Linux"
  },
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "sourceRepository": {
    "additionalProperties": {},
    "isCommitTriggerEnabled": true,
    "repositoryUrl": "https://github.com/gituser/acr-build-helloworld-node",
    "sourceControlAuthProperties": null,
    "sourceControlType": "Github"
  },
  "status": "enabled",
  "tags": null,
  "timeout": null,
  "type": "Microsoft.ContainerRegistry/registries/buildTasks"
}

```

## <a name="test-the-build-task"></a>Derleme görevini test etme

Artık derlemenizi tanımlayan bir derleme göreviniz var. Derleme tanımını test etmek için, [az acr build-task run][az-acr-build-task-run] komutunu yürüterek el ile bir derleme tetikleyin:

```azurecli-interactive
az acr build-task run --registry $ACR_NAME --name buildhelloworld
```

Varsayılan olarak, `az acr build-task run` komutunu yürüttüğünüzde komut, günlük çıktısını konsolunuza akışla aktarır. Burada çıktı, **eastus2** derlemesinin kuyruğa alınıp derlendiğini gösterir.

```console
$ az acr build-task run --registry mycontainerregistry --name buildhelloworld
Queued a build with build-id: eastus2.
Starting to stream the logs...
Cloning into '/root/acr-builder/src'...
time="2018-04-19T00:06:20Z" level=info msg="Running command git checkout master"
Already on 'master'
Your branch is up to date with 'origin/master'.
ffef1347389a008c9a8bfdf8c6a0ed78b0479894
time="2018-04-19T00:06:20Z" level=info msg="Running command git rev-parse --verify HEAD"
time="2018-04-19T00:06:20Z" level=info msg="Running command docker build --pull -f Dockerfile -t mycontainerregistry.azurecr.io/helloworld:eastus2 ."
Sending build context to Docker daemon  182.8kB
Step 1/5 : FROM node:9-alpine
9: Pulling from library/node
Digest: sha256:bd7b9aaf77ab2ce1e83e7e79fc0969229214f9126ced222c64eab49dc0bdae90
Status: Image is up to date for node:9-alpine
 ---> aa3e171e4e95
Step 2/5 : COPY . /src
 ---> e1c04dc2993b

[...]

6e5e20cbf4a7: Layer already exists
b69680cb4898: Pushed
b54af9b858b7: Pushed
eastus2: digest: sha256:9a7b73d06077ced2a02f7462f53e31a3e51e95ea5544fbcdb01e2fef094da1b6 size: 2423
time="2018-04-19T00:06:51Z" level=info msg="Running command docker inspect --format \"{{json .RepoDigests}}\" mycontainerregistry.azurecr.io/helloworld:eastus2"
"["mycontainerregistry.azurecr.io/helloworld@sha256:9a7b73d06077ced2a02f7462f53e31a3e51e95ea5544fbcdb01e2fef094da1b6"]"
time="2018-04-19T00:06:51Z" level=info msg="Running command docker inspect --format \"{{json .RepoDigests}}\" node:9-alpine"
"["node@sha256:bd7b9aaf77ab2ce1e83e7e79fc0969229214f9126ced222c64eab49dc0bdae90"]"
ACR Builder discovered the following dependencies:
- image:
    registry: mycontainerregistry.azurecr.io
    repository: helloworld
    tag: eastus2
    digest: sha256:9a7b73d06077ced2a02f7462f53e31a3e51e95ea5544fbcdb01e2fef094da1b6
  runtime-dependency:
    registry: registry.hub.docker.com
    repository: library/node
    tag: 9-alpine
    digest: sha256:5149aec8f508d48998e6230cdc8e6832cba192088b442c8ef7e23df3c6892cd3
  git:
    git-head-revision: 6944c6bd0602f96e5fecf56ff8d66e2d268223e3

Build complete
Build ID: eastus2 was successful after 39.789138274s
```

## <a name="view-build-status"></a>Derleme durumunu görüntüleme

El ile tetiklemediğiniz devam eden bir derlemenin durumunu görüntülemeyi bazı durumlarda faydalı bulabilirsiniz. Örneğin, kaynak kodu işlemeleri ile tetiklenen derlemelerin sorunlarını giderirken. Bu bölümde, el ile bir derlemeyi tetikleyecek, diğer yandan konsolunuza derleme günlüğünü akışla aktarmaya yönelik varsayılan davranışı engelleyeceksiniz. Ardından, `az acr build-task logs` komutunu kullanarak devam eden derlemeyi izleyeceksiniz.

İlk olarak, daha önce yaptığınız gibi bir derlemeyi el ile tetikleyin ancak konsolunuzda günlüğe kaydedilmesini engellemek için `--no-logs` bağımsız değişkenini belirtin:

```azurecli-interactive
az acr build-task run --registry $ACR_NAME --name buildhelloworld --no-logs
```

Ardından, `az build-task logs` komutunu kullanarak o anda devam eden derlemenin günlüğünü görüntüleyin:

```azurecli-interactive
az acr build-task logs --registry $ACR_NAME
```

O anda devam eden derlemenin günlüğü konsolunuza akışla aktarılır ve aşağıdaki çıktıya benzer şekilde görünmelidir (burada kesilmiş olarak gösterilir):

```console
$ az acr build-task logs --registry $ACR_NAME
Showing logs for the last updated build...
Build-id: eastus3

[...]

Build complete
Build ID: eastus3 was successful after 30.076988169s
```

## <a name="trigger-a-build-with-a-commit"></a>İşleme ile derleme tetikleme

Derleme görevini el ile çalıştırarak test ettikten sonra, bir kaynak kodu değişikliği ile otomatik olarak tetikleyin.

İlk olarak, [depo][sample-repo] yerel kopyanızı içeren dizinde olduğunuzdan emin olun:

```azurecli-interactive
cd acr-build-helloworld-node
```

Ardından, yeni bir dosya oluşturmak, işlemek ve GitHub üzerindeki depo çatalınıza göndermek için aşağıdaki komutları yürütün:

```azurecli-interactive
echo "Hello World!" > hello.txt
git add hello.txt
git commit -m "Testing ACR Build"
git push origin master
```

`git push` komutunu yürüttüğünüzde GitHub kimlik bilgilerinizi sağlamanız istenebilir. GitHub kullanıcı adınızı sağlayın ve parola için daha önce oluşturduğunuz kişisel erişim belirtecini (PAT) girin.

```console
$ git push origin master
Username for 'https://github.com': <github-username>
Password for 'https://githubuser@github.com': <personal-access-token>
```

Bir işlemeyi deponuza gönderdikten sonra, ACR Derlemesi tarafından oluşturulan web kancası başlatılır ve Azure Container Registry’de bir derleme başlatır. Derlemenin ilerleme durumunu doğrulamak ve izlemek için o anda devam eden derlemenin derleme günlüklerini görüntüleyin:

```azurecli-interactive
az acr build-task logs --registry $ACR_NAME
```

Çıktı aşağıdakine benzer ve o anda yürütülen (veya son yürütülen) derlemeyi gösterir:

```console
$ az acr build-task logs --registry $ACR_NAME
Showing logs for the last updated build...
Build-id: eastus4

[...]

Build complete
Build ID: eastus4 was successful after 28.9587031s
```

## <a name="list-builds"></a>Derlemeleri listeleme

ACR Derlemesinin kayıt defteriniz için tamamladığı derlemelerin bir listesini görmek üzere [az acr build-task list-builds][az-acr-build-task-list-builds] komutunu çalıştırın:

```azurecli-interactive
az acr build-task list-builds --registry $ACR_NAME --output table
```

Komut çıktısı aşağıdakine benzer şekilde görünmelidir. ACR Derlemesinin yürüttüğü derlemeler gösterilir ve en son derleme için TRIGGER sütununda "Git İşleme" ifadesi görünür:

```console
$ az acr build-task list-builds --registry $ACR_NAME --output table
BUILD ID    TASK             PLATFORM    STATUS     TRIGGER     STARTED               DURATION
----------  ---------------  ----------  ---------  ----------  --------------------  ----------
eastus4     buildhelloworld  Linux       Succeeded  Git Commit  2018-04-20T22:50:27Z  00:00:35
eastus3     buildhelloworld  Linux       Succeeded  Manual      2018-04-20T22:47:19Z  00:00:30
eastus2     buildhelloworld  Linux       Succeeded  Manual      2018-04-20T22:46:14Z  00:00:55
eastus1                                  Succeeded  Manual      2018-04-20T22:38:22Z  00:00:55
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir Git deposuna kaynak kodu işlediğinizde Azure’da kapsayıcı görüntü derlemelerini otomatik olarak tetiklemek üzere bir derleme görevi kullanmayı öğrendiniz. Bir kapsayıcı görüntüsünün temel görüntüsü güncelleştirildiğinde derlemeleri tetikleyen derleme görevleri oluşturmak için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Temel görüntü güncelleştirmesi ile derlemeleri otomatikleştirme](container-registry-tutorial-base-image-update.md)

<!-- LINKS - External -->
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/
[sample-repo]: https://github.com/Azure-Samples/acr-build-helloworld-node

<!-- LINKS - Internal -->
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-build-task]: /cli/azure/acr#az-acr-build-task
[az-acr-build-task-create]: /cli/azure/acr#az-acr-build-task-create
[az-acr-build-task-run]: /cli/azure/acr#az-acr-build-task-run
[az-acr-build-task-list-builds]: /cli/azure/acr#az-acr-build-task-list-build

<!-- IMAGES -->
[build-task-01-new-token]: ./media/container-registry-tutorial-build-tasks/build-task-01-new-token.png
[build-task-02-generated-token]: ./media/container-registry-tutorial-build-tasks/build-task-02-generated-token.png
