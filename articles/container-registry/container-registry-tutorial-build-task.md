---
title: Öğretici - Azure Container Registry Derlemesi ile kapsayıcı görüntüsü derlemelerini otomatik hale getirme
description: Bu öğreticide, bir Git deposuna kaynak kodu işlediğinizde bulutta kapsayıcı görüntü derlemelerini otomatik olarak tetiklemek üzere bir derleme görevi yapılandırmayı öğreneceksiniz.
services: container-registry
author: mmacy
manager: jeconnoc
ms.service: container-registry
ms.topic: tutorial
ms.date: 05/11/2018
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: 71ea0f489df6969f0916ac14d187e10a90a520cd
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38722720"
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

[!INCLUDE [container-registry-build-preview-note](../../includes/container-registry-build-preview-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure CLI’yı yerel olarak kullanmak istiyorsanız Azure CLI **2.0.32** veya sonraki bir sürüm yüklü olmalıdır. Sürümü bulmak için `az --version` komutunu çalıştırın. CLI’yı yüklemeniz veya yükseltmeniz gerekiyorsa bkz. [Azure CLI’yı yükleme][azure-cli].

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
  "creationDate": "2018-05-10T19:34:48.086776+00:00",
  "id": "/subscriptions/<Subscription ID>/resourceGroups/mycontainerregistry/providers/Microsoft.ContainerRegistry/registries/mycontainerregistry/buildTasks/buildhelloworld",
  "location": "eastus",
  "name": "buildhelloworld",
  "platform": {
    "additionalProperties": {},
    "cpu": 1,
    "osType": "Linux"
  },
  "properties": {
    "additionalProperties": {
      "imageName": null
    },
    "baseImageDependencies": null,
    "baseImageTrigger": "Runtime",
    "branch": "master",
    "buildArguments": [],
    "contextPath": null,
    "dockerFilePath": "Dockerfile",
    "imageNames": [
      "helloworld:{{.Build.ID}}"
    ],
    "isPushEnabled": true,
    "noCache": false,
    "provisioningState": "Succeeded",
    "type": "Docker"
  },
  "provisioningState": "Succeeded",
  "resourceGroup": "mycontainerregistry",
  "sourceRepository": {
    "additionalProperties": {},
    "isCommitTriggerEnabled": true,
    "repositoryUrl": "https://github.com/gituser/acr-build-helloworld-node",
    "sourceControlAuthProperties": null,
    "sourceControlType": "GitHub"
  },
  "status": "Enabled",
  "tags": null,
  "timeout": 3600,
  "type": "Microsoft.ContainerRegistry/registries/buildTasks"
}
```

## <a name="test-the-build-task"></a>Derleme görevini test etme

Artık derlemenizi tanımlayan bir derleme göreviniz var. Derleme tanımını test etmek için, [az acr build-task run][az-acr-build-task-run] komutunu yürüterek el ile bir derleme tetikleyin:

```azurecli-interactive
az acr build-task run --registry $ACR_NAME --name buildhelloworld
```

Varsayılan olarak, `az acr build-task run` komutunu yürüttüğünüzde komut, günlük çıktısını konsolunuza akışla aktarır. Burada çıktı, **aa2** derlemesinin kuyruğa alınıp derlendiğini gösterir.

```console
$ az acr build-task run --registry $ACR_NAME --name buildhelloworld
Queued a build with build ID: aa2
Waiting for a build agent...
time="2018-05-10T19:37:17Z" level=info msg="Running command git clone https://x-access-token:*************@github.com/gituser/acr-build-helloworld-node /root/acr-builder/src"
Cloning into '/root/acr-builder/src'...
time="2018-05-10T19:37:17Z" level=info msg="Running command git checkout master"
Already on 'master'
Your branch is up to date with 'origin/master'.
920f16cfafa36d0bc3f397c3dd48185a03499404
time="2018-05-10T19:37:17Z" level=info msg="Running command git rev-parse --verify HEAD"
time="2018-05-10T19:37:17Z" level=info msg="Running command docker build --pull -f Dockerfile -t mycontainerregistry.azurecr.io/helloworld:aa2 ."
Sending build context to Docker daemon  209.9kB
Step 1/5 : FROM node:9-alpine
9-alpine: Pulling from library/node
Digest: sha256:5149aec8f508d48998e6230cdc8e6832cba192088b442c8ef7e23df3c6892cd3
Status: Image is up to date for node:9-alpine
 ---> 7af437a39ec2
Step 2/5 : COPY . /src
 ---> 48a7735fa94e

[...]

26b0c207c4a9: Pushed
917e7cdebc8b: Pushed
aa2: digest: sha256:6975f01e2e202c084581e676acbe6047788fbe616836328b0b31ce8c58e9fc89 size: 1367
time="2018-05-10T19:37:57Z" level=info msg="Running command docker inspect --format \"{{json .RepoDigests}}\" mycontainerregistrtyy.azurecr.io/helloworld:aa2"
"["mycontainerregistrtyy.azurecr.io/helloworld@sha256:6975f01e2e202c084581e676acbe6047788fbe616836328b0b31ce8c58e9fc89"]"
time="2018-05-10T19:37:57Z" level=info msg="Running command docker inspect --format \"{{json .RepoDigests}}\" node:9-alpine"
"["node@sha256:5149aec8f508d48998e6230cdc8e6832cba192088b442c8ef7e23df3c6892cd3"]"
ACR Builder discovered the following dependencies:
- image:
    registry: mycontainerregistrtyy.azurecr.io
    repository: helloworld
    tag: aa2
    digest: sha256:6975f01e2e202c084581e676acbe6047788fbe616836328b0b31ce8c58e9fc89
  runtime-dependency:
    registry: registry.hub.docker.com
    repository: library/node
    tag: 9-alpine
    digest: sha256:5149aec8f508d48998e6230cdc8e6832cba192088b442c8ef7e23df3c6892cd3
  git:
    git-head-revision: 920f16cfafa36d0bc3f397c3dd48185a03499404

Build complete
Build ID: aa2 was successful after 46.491407373s
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
Showing logs for the last updated build
Build ID: aa3

[...]

Build complete
Build ID: aa3 was successful after 1m14.26397548s
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
Showing logs for the last updated build
Build ID: aa4

[...]

Build complete
Build ID: aa4 was successful after 39.164385024s
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
aa4         buildhelloworld  Linux       Succeeded  Git Commit  2018-05-10T19:49:40Z  00:00:45
aa3         buildhelloworld  Linux       Succeeded  Manual      2018-05-10T19:41:50Z  00:01:20
aa2         buildhelloworld  Linux       Succeeded  Manual      2018-05-10T19:37:11Z  00:00:50
aa1                          Linux       Succeeded  Manual      2018-05-10T19:10:14Z  00:00:55
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir Git deposuna kaynak kodu işlediğinizde Azure’da kapsayıcı görüntü derlemelerini otomatik olarak tetiklemek üzere bir derleme görevi kullanmayı öğrendiniz. Bir kapsayıcı görüntüsünün temel görüntüsü güncelleştirildiğinde derlemeleri tetikleyen derleme görevleri oluşturmak için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Temel görüntü güncelleştirmesi ile derlemeleri otomatikleştirme](container-registry-tutorial-base-image-update.md)

<!-- LINKS - External -->
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
