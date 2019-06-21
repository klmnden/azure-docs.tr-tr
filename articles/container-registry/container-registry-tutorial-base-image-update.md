---
title: Öğretici - kapsayıcı görüntüsü derlemelerinde temel görüntü güncelleştirme - Azure Container kayıt defteri görevleri otomatikleştirin
description: Bu öğreticide, temel görüntü güncelleştirildiğinde kapsayıcı görüntüsü yapılarınızı bulutta otomatik olarak tetiklemek için bir Azure kapsayıcı kayıt defteri görevi yapılandırma konusunda bilgi edinin.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: tutorial
ms.date: 06/12/2019
ms.author: danlep
ms.custom: seodec18, mvc
ms.openlocfilehash: 7d7cba63060756bff786b9475275e5262627cae9
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67295708"
---
# <a name="tutorial-automate-container-image-builds-when-a-base-image-is-updated-in-an-azure-container-registry"></a>Öğretici: Temel görüntü Azure container registry güncelleştirildiğinde kapsayıcı görüntü oluşturmayı otomatikleştirme 

ACR Görevleri, kapsayıcının temel görüntüsü güncelleştirildiğinde örneğin temel görüntülerinizden birinde işletim sistemine ve uygulama çerçevesine yama uyguladığınızda otomatik derleme yürütmeyi destekler. Bu öğreticide, ACR Görevlerinde bir kapsayıcı temel görüntüsü kayıt defterinize gönderildiğinde bulutta bir görev tetikleyen derleme görevini oluşturmayı öğreneceksiniz.

Bu öğreticide, serinin son kısmı:

> [!div class="checklist"]
> * Temel görüntü oluşturma
> * Uygulama görüntüsü derleme görevi oluşturma
> * Uygulama görüntüsü görevini tetiklemek için temel görüntüyü güncelleştirme
> * Tetiklenen görevi görüntüleme
> * Güncelleştirilmiş uygulama görüntüsünü doğrulama

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure CLI’yı yerel olarak kullanmak istiyorsanız Azure CLI **2.0.46** veya sonraki bir sürüm yüklü olmalıdır. Sürümü bulmak için `az --version` komutunu çalıştırın. Gerekirse yükleyin veya CLI'yı yükseltmek için bkz: [Azure CLI yükleme][azure-cli].

## <a name="prerequisites"></a>Önkoşullar

### <a name="complete-the-previous-tutorials"></a>Önceki öğreticileri tamamlama

Bu öğreticide, serinin ilk iki öğreticisindeki adımları zaten tamamladığınız ve şu işlemleri yaptığınız varsayılır:

* Azure kapsayıcı kayıt defteri oluşturma
* Örnek deponun çatalını oluşturma
* Örnek depoyu kopyalama
* GitHub kişisel erişim belirteci oluşturma

Henüz yapmadıysanız, devam etmeden önce ilk iki öğreticiyi tamamlayın:

[Azure Container Registry Görevleri ile bulutta kapsayıcı görüntüleri derleme](container-registry-tutorial-quick-task.md)

[Azure Container Registry Görevleri ile kapsayıcı görüntüsü derlemelerini otomatik hale getirme](container-registry-tutorial-build-task.md)

### <a name="configure-the-environment"></a>Ortamı yapılandırma

Bu kabuk ortam değişkenlerini ortamınıza uygun değerlerle doldurun. Bu adımın yapılması kesinlikle zorunlu değildir ancak bu öğreticideki çok satırlı Azure CLI komutlarını yürütmeyi biraz daha kolaylaştırır. Bu ortam değişkenlerini doldurmazsanız her değeri, örnek komutlarda her göründükleri durumda el ile değiştirmeniz gerekir.

```azurecli-interactive
ACR_NAME=<registry-name>        # The name of your Azure container registry
GIT_USER=<github-username>      # Your GitHub user account name
GIT_PAT=<personal-access-token> # The PAT you generated in the second tutorial
```

## <a name="base-images"></a>Temel görüntüler

Çoğu kapsayıcı görüntüsünü tanımlayan Dockerfile'lar, temel alınan ve çoğunlukla *temel görüntü* olarak adlandırılan bir üst görüntü belirtir. Temel görüntüler genellikle içerir işletim sistemi, örneğin [Alpine Linux][base-alpine] or [Windows Nano Server][base-windows], hangi kapsayıcının katmanları geri kalanını uygulanır. Bunlar ayrıca uygulama çerçeveleri gibi içerebilir [Node.js][temel düğümlü] veya [.NET Core][base-dotnet].

### <a name="base-image-updates"></a>Temel görüntü güncelleştirmeleri

Temel görüntü çoğunlukla, görüntüdeki işletim sistemi veya çerçevenin yeni özelliklerini veya geliştirmelerini içermesi için görüntü bakımcısı tarafından güncelleştirilir. Temel görüntüyü güncelleştirmenin bir diğer yaygın nedeni de güvenlik yamalarıdır.

Temel görüntü güncelleştirildiğinde, yeni özellik ve düzeltmelerin eklenmesi için kayıt defterinizde bulunan ve bu temel görüntüye dayanan tüm kapsayıcı görüntülerini yeniden oluşturmanız gerektiği belirtilir. ACR Görevleri, kapsayıcının temel görüntüsü güncelleştirildiğinde sizin için görüntüleri otomatik olarak oluşturma özelliğine sahiptir.

### <a name="tasks-triggered-by-a-base-image-update"></a>Bir temel görüntü güncelleştirme tarafından tetiklenen görevleri

* Şu anda görüntü yapılar için bir Dockerfile, aynı Azure kapsayıcı kayıt defteri, genel Docker Hub depo veya Microsoft kapsayıcı kayıt defteri genel bir depoda bağımlılıkları temel görüntüleri ACR görev algılar. Temel görüntü olarak belirtilmişse `FROM` deyimi bu konumlardan birinde bulunur, görüntünün oluşturucularını güncelleştirilir dilediğiniz zaman yeniden sağlamak için bir kancasını ACR görev ekler.

* Bir ACR görevle oluşturduğunuzda [az acr görev oluşturma][az-acr-task-create] komutu, görev varsayılan olarak *etkin* tetikleyicisi tarafından bir temel görüntü güncelleştirme için. Diğer bir deyişle, `base-image-trigger-enabled` özelliği True olarak ayarlayın. Bir görev bu davranışı devre dışı bırakmak isterseniz, özelliği False olarak güncelleştirin. Örneğin, aşağıdaki komutu çalıştırın [az acr görev güncelleştirme][az-acr-task-update] komutu:

  ```azurecli
  az acr task update --myregistry --name mytask --base-image-trigger-enabled False
  ```

* Belirlemek ve--temel görüntüsünü içeren--kapsayıcı görüntüsü ait bağımlılıkları izlemek bir ACR görevini etkinleştirmek için ilk görev tetiklemelidir **en az bir kez**. Örneğin, kullanarak el ile tetikleyici [az acr görevi Çalıştır][az-acr-task-run] komutu.

* Temel görüntü güncelleştirme görevde tetiklemek için temel görüntü olmalıdır bir *kararlı* gibi etiket `node:9-alpine`. Bu etiketleme, işletim sistemi ve framework güncelleştirilir temel bir görüntü için tipik bir en son kararlı sürüme düzeltme. Temel görüntü ile yeni bir sürüme etiket güncelleştirdiyseniz, bir görev tetiklemez. Görüntü etiketleme hakkında daha fazla bilgi için bkz. [Kılavuzu en iyi yöntemler](https://stevelasker.blog/2018/03/01/docker-tagging-best-practices-for-tagging-and-versioning-docker-images/). 

### <a name="base-image-update-scenario"></a>Temel görüntü güncelleştirme senaryosu

Bu öğreticide, bir temel görüntü güncelleştirme senaryosunda size yol gösterilir. [Kod örneği][code-sample] iki dockerfile'ları içerir: bir uygulama görüntüsü ve bir görüntü, temel olarak belirtir. Aşağıdaki bölümlerde, aynı kapsayıcı kayıt defterine yeni bir sürümünü temel görüntü gönderildiğinde otomatik olarak bir uygulama görüntüsü derlemesini tetikleyen bir ACR görevi oluşturun.

[Uygulama içi Dockerfile][dockerfile-app]: Statik bir web sayfası, temel Node.js sürümünü görüntüleme işleyen bir küçük Node.js web uygulaması. Sürüm dizesinin simülasyonu yapılır ve bu, temel görüntüde tanımlanan `NODE_VERSION` ortam değişkeninin içeriğini görüntüler.

[Dockerfile temel][dockerfile-base]: Görüntü, `Dockerfile-app` base belirtir. Kendisine bağlı olduğu bir [düğüm][base-node] görüntü ve içerir `NODE_VERSION` ortam değişkeni.

Aşağıdaki bölümlerde bir görev oluşturacak, temel görüntü Dockerfile içinde `NODE_VERSION` değerini güncelleştirecek ve sonra da ACR Görevlerini kullanarak temel görüntü oluşturacaksınız. ACR görevi yeni temel görüntüyü kayıt defterinize gönderdikten sonra, uygulama görüntüsünün derlemesini otomatik olarak tetikler. İsteğe bağlı olarak, derleme görüntülerinde farklı sürüm dizeleri görmek için uygulama kapsayıcısı görüntüsünü yerel olarak çalıştırırsınız.

Bu öğreticide, ACR görev oluşturur ve bir Dockerfile içinde belirtilen bir uygulama kapsayıcı görüntüyü gönderir. ACR görevleri de çalıştırabilir [çok adımlı görevler](container-registry-tasks-multi-step.md), oluşturmak için adımları tanımlamak için bir YAML dosyası kullanarak anında iletme ve isteğe bağlı olarak birden çok kapsayıcı test edin.

## <a name="build-the-base-image"></a>Temel görüntü oluşturma

Başlangıç olarak ACR Görevleri *hızlı görevi* ile temel görüntüyü oluşturun. Serinin [ilk öğreticisinde](container-registry-tutorial-quick-task.md) açıklandığı gibi, bu işlem yalnızca görüntüyü oluşturmakla kalmaz, oluşturma başarılı olduysa bunu kapsayıcınızın kayıt defterine de gönderir.

```azurecli-interactive
az acr build --registry $ACR_NAME --image baseimages/node:9-alpine --file Dockerfile-base .
```

## <a name="create-a-task"></a>Bir görev oluşturun

Ardından, bir görev oluşturun [az acr görev oluşturma][az-acr-task-create]:

```azurecli-interactive
az acr task create \
    --registry $ACR_NAME \
    --name taskhelloworld \
    --image helloworld:{{.Run.ID}} \
    --arg REGISTRY_NAME=$ACR_NAME.azurecr.io \
    --context https://github.com/$GIT_USER/acr-build-helloworld-node.git \
    --file Dockerfile-app \
    --branch master \
    --git-access-token $GIT_PAT
```

> [!IMPORTANT]
> Önizleme sırasında görevlerin daha önce oluşturduysanız `az acr build-task` komutu kullanılarak yeniden oluşturulması gereken görevleri [az acr görev][az-acr-task] komutu.

Bu görev, [önceki öğreticide](container-registry-tutorial-build-task.md) oluşturulan hızlı göreve benzer. ACR Görevlerine, işlemeler `--context` tarafından belirtilen depoya gönderildiğinde bir görüntü derlemesi tetiklemesini bildirir. Genel bir temel görüntü önceki öğreticide görüntüyü oluşturmak için kullanılan dockerfile dosyasının belirtir (`FROM node:9-alpine`), bu görevi, Dockerfile'da [Dockerfile uygulama][dockerfile-app], aynı kayıt defterinde bir temel görüntü belirtir:

```Dockerfile
FROM ${REGISTRY_NAME}/baseimages/node:9-alpine
```

Bu yapılandırma, daha sonra Bu öğreticide bir framework düzeltme eki temel görüntüde benzetimini yapmak kolaylaştırır.

## <a name="build-the-application-container"></a>Uygulama kapsayıcısını oluşturma

Kullanım [az acr görevi Çalıştır][az-acr-task-run] el ile tetikleyici ve uygulama görüntüsü oluşturun. Bu adım, görev bir temel görüntüye uygulama görüntünün bağımlılık izler sağlar.

```azurecli-interactive
az acr task run --registry $ACR_NAME --name taskhelloworld
```

Görev tamamlandıktan sonra, aşağıdaki isteğe bağlı adımı tamamlamak istiyorsanız **Run ID** (örneğin, "da6") değerini not alın.

### <a name="optional-run-application-container-locally"></a>İsteğe bağlı: Uygulama kapsayıcısı yerel olarak çalıştırma

Cloud Shell'de değil de yerel olarak çalışıyorsanız ve Docker'ı yüklediyseniz, temel görüntüsünü oluşturmadan önce web tarayıcısında işlenen uygulamayı görmek için kapsayıcıyı çalıştırın. Cloud Shell kullanıyorsanız bu bölümü atlayın (Cloud Shell `az acr login` veya `docker run` komutunu desteklemez).

İlk olarak, kapsayıcı kayıt defteri ile kimlik doğrulaması [az acr oturum açma][az-acr-login]:

```azurecli
az acr login --name $ACR_NAME
```

Şimdi `docker run` ile kapsayıcıyı yerel olarak çalıştırın. **\<run-id\>** değerini önceki adımdaki çıktıda bulunan Run ID değeriyle (örneğin, "da6") değiştirin. Bu örnek kapsayıcı adları `myapp` ve içerir `--rm` durdurduğunuzda, kapsayıcı kaldırmak için parametre.

```bash
docker run -d -p 8080:80 --name myapp --rm $ACR_NAME.azurecr.io/helloworld:<run-id>
```

Tarayıcınızda `http://localhost:8080` adresine gidin; aşağıdakine benzer biçimde web sayfasında işlenmiş Node.js sürüm numarasını görüyor olmalısınız. Sonraki adımlardan birinde, sürüm dizesine bir "a" ekleyerek sürümü yükseltirsiniz.

![Tarayıcıda işlenen örnek uygulamanın ekran görüntüsü][base-update-01]

Durdur ve kapsayıcı kaldırmak için aşağıdaki komutu çalıştırın:

```bash
docker stop myapp
```

## <a name="list-the-builds"></a>Derlemeleri listeleme

ACR görevleri kullanarak kayıt tamamlandıktan sonra çalıştırmaları listesi görev [az acr görev listesi-çalıştığında][az-acr-task-list-runs] komutu:

```azurecli-interactive
az acr task list-runs --registry $ACR_NAME --output table
```

Önceki öğreticiyi tamamladıysanız (ve kayıt defterini silmediyseniz), aşağıdakine benzer bir çıkış görüyor olmalısınız. Sonraki bölümde temel görüntüyü güncelleştirdikten sonra çıkışı karşılaştırabilmek için görev sayısını ve en son RUN ID değerini not alın.

```console
$ az acr task list-runs --registry $ACR_NAME --output table

RUN ID    TASK            PLATFORM    STATUS     TRIGGER     STARTED               DURATION
--------  --------------  ----------  ---------  ----------  --------------------  ----------
da6       taskhelloworld  Linux       Succeeded  Manual      2018-09-17T23:07:22Z  00:00:38
da5                       Linux       Succeeded  Manual      2018-09-17T23:06:33Z  00:00:31
da4       taskhelloworld  Linux       Succeeded  Git Commit  2018-09-17T23:03:45Z  00:00:44
da3       taskhelloworld  Linux       Succeeded  Manual      2018-09-17T22:55:35Z  00:00:35
da2       taskhelloworld  Linux       Succeeded  Manual      2018-09-17T22:50:59Z  00:00:32
da1                       Linux       Succeeded  Manual      2018-09-17T22:29:59Z  00:00:57
```

## <a name="update-the-base-image"></a>Temel görüntüyü güncelleştirme

Burada, temel görüntüde bir çerçeve yamasının simülasyonunu yaparsınız. **Dockerfile-base** öğesini düzenleyin ve `NODE_VERSION` içinde tanımlanan sürüm numarasından sonra bir "a" ekleyin:

```Dockerfile
ENV NODE_VERSION 9.11.2a
```

Değiştirilmiş temel görüntüyü derlemek için bir hızlı görev çalıştırın. Çıkıştaki **Run ID** değerini not alın.

```azurecli-interactive
az acr build --registry $ACR_NAME --image baseimages/node:9-alpine --file Dockerfile-base .
```

Derleme tamamlandıktan ve ACR görevi yeni temel görüntüyü kayıt defterinize gönderdikten sonra, uygulama görüntüsünün derlemesini tetikler. Daha önce oluşturduğunuz görevin uygulama görüntüsü derlemeyi tetiklemesi birkaç dakika sürebilir çünkü yeni derlenen ve gönderilen temel görüntüyü algılaması gerekir.

## <a name="list-updated-build"></a>Güncelleştirilmiş derlemeyi listeleme

Artık temel görüntüyü güncelleştirdiğinize göre, görev çalıştırmalarınızı bir kez daha listeleyip önceki listeyle bunu karşılaştırın. İlk seferinde çıkış farklı görünmüyorsa, yeni görev çalıştırmasının listede yer aldığını görmek için komutu düzenli aralıklarla çalıştırın.

```azurecli-interactive
az acr task list-runs --registry $ACR_NAME --output table
```

Çıktı aşağıdakine benzer olacaktır. Son yürütülen derlemenin TRIGGER değeri "Image Update" olmalıdır; bu değer, görevin temel görüntünüzün hızlı görevi ile başlatıldığını gösterir.

```console
$ az acr task list-builds --registry $ACR_NAME --output table

Run ID    TASK            PLATFORM    STATUS     TRIGGER       STARTED               DURATION
--------  --------------  ----------  ---------  ------------  --------------------  ----------
da8       taskhelloworld  Linux       Succeeded  Image Update  2018-09-17T23:11:50Z  00:00:33
da7                       Linux       Succeeded  Manual        2018-09-17T23:11:27Z  00:00:35
da6       taskhelloworld  Linux       Succeeded  Manual        2018-09-17T23:07:22Z  00:00:38
da5                       Linux       Succeeded  Manual        2018-09-17T23:06:33Z  00:00:31
da4       taskhelloworld  Linux       Succeeded  Git Commit    2018-09-17T23:03:45Z  00:00:44
da3       taskhelloworld  Linux       Succeeded  Manual        2018-09-17T22:55:35Z  00:00:35
da2       taskhelloworld  Linux       Succeeded  Manual        2018-09-17T22:50:59Z  00:00:32
da1                       Linux       Succeeded  Manual        2018-09-17T22:29:59Z  00:00:57
```

Yeni oluşturulan kapsayıcıyı çalıştırıp güncelleştirilmiş sürüm numarasını görmek için aşağıdaki isteğe bağlı adımı uygulamak isterseniz, Görüntü Güncelleştirmesi ile tetiklenen derlemenin **RUN ID** değerini (önceki çıktıda "da8") not alın.

### <a name="optional-run-newly-built-image"></a>İsteğe bağlı: Yeni derlenen görüntü çalıştırma

Cloud Shell'de değil de yerel olarak çalışıyorsanız ve Docker'ı yüklediyseniz, derlemesi tamamlandıktan sonra yeni uygulama görüntüsünü çalıştırın. `<run-id>` değerinin yerine önceki adımda aldığınız RUN ID değerini koyun. Cloud Shell kullanıyorsanız bu bölümü atlayın (Cloud Shell `docker run` komutunu desteklemez).

```bash
docker run -d -p 8081:80 --name updatedapp --rm $ACR_NAME.azurecr.io/helloworld:<run-id>
```

Tarayıcınızda http://localhost:8081 adresine gidin; web sayfasında güncelleştirilmiş Node.js sürüm numarasını ("a" ile) görüyor olmalısınız:

![Tarayıcıda işlenen örnek uygulamanın ekran görüntüsü][base-update-02]

**Temel** görüntünüzü yeni sürüm numarasıyla güncelleştirdiğinize ama son oluşturulan **uygulama** görüntüsünde yeni sürümün görüntülendiğine dikkat etmelisiniz. ACR Görevler temel görüntüde yaptığınız değişikliği almış ve uygulama görüntünüzü otomatik olarak yeniden oluşturmuştur.

Durdur ve kapsayıcı kaldırmak için aşağıdaki komutu çalıştırın:

```bash
docker stop updatedapp
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kapsayıcı kayıt defteri, kapsayıcı örneğin, anahtar kasası ve hizmet sorumlusu dahil olmak üzere bu öğretici serisinde oluşturduğunuz tüm kaynakları kaldırmak için aşağıdaki komutları gönderin:

```azurecli-interactive
az group delete --resource-group $RES_GROUP
az ad sp delete --id http://$ACR_NAME-pull
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, temel görüntü güncelleştirildiğinde kapsayıcı görüntü derlemelerini otomatik olarak tetiklemek üzere bir görevi kullanmayı öğrendiniz. Artık kapsayıcı kayıt defteriniz için kimlik doğrulaması konusunu öğrenmeye geçebilirsiniz.

> [!div class="nextstepaction"]
> [Azure Container Registry’de kimlik doğrulaması](container-registry-authentication.md)

<!-- LINKS - External -->
[base-alpine]: https://hub.docker.com/_/alpine/
[base-dotnet]: https://hub.docker.com/r/microsoft/dotnet/
[base-node]: https://hub.docker.com/_/node/
[base-windows]: https://hub.docker.com/r/microsoft/nanoserver/
[code-sample]: https://github.com/Azure-Samples/acr-build-helloworld-node
[dockerfile-app]: https://github.com/Azure-Samples/acr-build-helloworld-node/blob/master/Dockerfile-app
[dockerfile-base]: https://github.com/Azure-Samples/acr-build-helloworld-node/blob/master/Dockerfile-base

<!-- LINKS - Internal -->
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-build]: /cli/azure/acr#az-acr-build-run
[az-acr-task-create]: /cli/azure/acr/task#az-acr-task-create
[az-acr-task-update]: /cli/azure/acr/task#az-acr-task-update
[az-acr-task-run]: /cli/azure/acr/task#az-acr-task-run
[az-acr-login]: /cli/azure/acr#az-acr-login
[az-acr-task-list-runs]: /cli/azure/acr
[az-acr-task]: /cli/azure/acr

<!-- IMAGES -->
[base-update-01]: ./media/container-registry-tutorial-base-image-update/base-update-01.png
[base-update-02]: ./media/container-registry-tutorial-base-image-update/base-update-02.png
