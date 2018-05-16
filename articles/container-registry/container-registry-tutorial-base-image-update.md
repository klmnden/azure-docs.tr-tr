---
title: Öğretici - Azure Container Registry Derlemesi ile temel görüntü güncelleştirmesinde kapsayıcı görüntüsü derlemelerini otomatik hale getirme
description: Bu öğreticide, temel görüntü güncelleştirildiğinde bulutta kapsayıcı görüntü derlemelerini otomatik olarak tetiklemek üzere bir derleme görevi yapılandırmayı öğreneceksiniz.
services: container-registry
author: mmacy
manager: jeconnoc
ms.service: container-registry
ms.topic: tutorial
ms.date: 05/07/2018
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: 976f61d99b88d241b39bfec9d95e16de272d9c14
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="tutorial-automate-image-builds-on-base-image-update-with-azure-container-registry-build"></a>Öğretici: Azure Container Registry Derlemesi ile temel görüntü güncelleştirmesinde görüntü derlemelerini otomatik hale getirme

ACR Build, kapsayıcının temel görüntüsü güncelleştirildiğinde örneğin temel görüntülerinizden birinde işletim sistemine ve uygulama çerçevesine yama uyguladığınızda otomatik derleme yürütmeyi destekler. Bu öğreticide, ACR Build'de bir kapsayıcı temel görüntüsü kayıt defterinize gönderildiğinde bulutta bir derleme tetikleyen derleme görevini oluşturmayı öğreneceksiniz.

Bu öğreticide, serinin son kısmı:

> [!div class="checklist"]
> * Temel görüntü oluşturma
> * Uygulama görüntüsü derleme görevi oluşturma
> * Uygulama görüntüsü derlemeyi tetiklemek için temel görüntüyü güncelleştirme
> * Tetiklenen derlemeyi görüntüleme
> * Güncelleştirilmiş uygulama görüntüsünü doğrulama

> [!IMPORTANT]
> ACR Build şu anda önizleme aşamasındadır ve yalnızca **Doğu ABD** (eastus) ve **Batı Avrupa** (westeurope) bölgelerindeki Azure kapsayıcı kayıt defterleri tarafından desteklenir. Önizlemeler, [ek kullanım koşullarını][terms-of-use] kabul etmeniz şartıyla kullanımınıza sunulur. Bu özelliğin bazı yönleri genel kullanıma açılmadan önce değişebilir.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure CLI’yı yerel olarak kullanmak istiyorsanız Azure CLI **2.0.32** veya sonraki bir sürüm yüklü olmalıdır. Sürümü bulmak için `az --version` komutunu çalıştırın. CLI’yı yüklemeniz veya yükseltmeniz gerekiyorsa bkz. [Azure CLI 2.0’ı yükleme][azure-cli].

## <a name="prerequisites"></a>Ön koşullar

### <a name="complete-previous-tutorials"></a>Önceki öğreticileri tamamlama

Bu öğreticide, serinin ilk iki öğreticisindeki adımları zaten tamamladığınız ve şu işlemleri yaptığınız varsayılır:

* Azure kapsayıcı kayıt defteri oluşturma
* Örnek deponun çatalını oluşturma
* Örnek depoyu kopyalama
* GitHub kişisel erişim belirteci oluşturma

Henüz yapmadıysanız, devam etmeden önce ilk iki öğreticiyi tamamlayın:

[Azure Container Registry Derlemesi ile bulutta kapsayıcı görüntüleri derleme](container-registry-tutorial-quick-build.md)

[Azure Container Registry Derlemesi ile kapsayıcı görüntüsü derlemelerini otomatik hale getirme](container-registry-tutorial-build-task.md)

### <a name="configure-environment"></a>Ortamı yapılandırma

Bu kabuk ortam değişkenlerini ortamınıza uygun değerlerle doldurun. Bunun yapılması kesinlikle zorunlu değildir ancak bu öğreticideki çok satırlı Azure CLI komutlarını yürütmeyi biraz daha kolaylaştırır. Bu alanları doldurmazsanız her değeri, örnek komutlarda her göründükleri durumda el ile değiştirmeniz gerekir.

```azurecli-interactive
ACR_NAME=mycontainerregistry # The name of your Azure container registry
GIT_USER=gituser             # Your GitHub user account name
GIT_PAT=personalaccesstoken  # The PAT you generated in the second tutorial
```

## <a name="base-images"></a>Temel görüntüler

Çoğu kapsayıcı görüntüsünü tanımlayan Dockerfile'lar, temel alınan ve çoğunlukla *temel görüntü* olarak adlandırılan bir üst görüntü belirtir. Normalde temel görüntüler, [Alpine Linux][base-alpine] veya [Windows Nano Server][base-windows] gibi bir işletim sistemi içerir ve kapsayıcının kalan katmanları bu işletim sistemi üzerine uygulanır. Bunlar ayrıca [Node.js][base-node] veya [.NET Core][base-dotnet] gibi uygulama çerçeveleri de içerir.

### <a name="base-image-updates"></a>Temel görüntü güncelleştirmeleri

Temel görüntü çoğunlukla, görüntüdeki işletim sistemi veya çerçevenin yeni özelliklerini veya geliştirmelerini içermesi için görüntü bakımcısı tarafından güncelleştirilir. Temel görüntüyü güncelleştirmenin bir diğer yaygın nedeni de güvenlik yamalarıdır.

Temel görüntü güncelleştirildiğinde, yeni özellik ve düzeltmelerin eklenmesi için kayıt defterinizde bulunan ve bu temel görüntüye dayanan tüm kapsayıcı görüntülerini yeniden oluşturmanız gerektiği belirtilir. ACR Build, kapsayıcının temel görüntüsü güncelleştirildiğinde sizin için görüntüleri otomatik olarak oluşturma özelliğine sahiptir.

### <a name="base-image-update-scenario"></a>Temel görüntü güncelleştirme senaryosu

Bu öğreticide, bir temel görüntü güncelleştirme senaryosunda size yol gösterilir. [Kod örneği][code-sample] iki Dockerfile: bir uygulama görüntüsü ve bunun temel olarak belirttiği bir görüntü. Aşağıdaki bölümlerde, kapsayıcı kayıt defterinize yeni bir temel görüntü sürümü gönderildiğinde uygulama görüntüsü oluşturulmasını otomatik olarak tetikleyen bir ACR Build görevi oluşturursunuz.

[Dockerfile-app][dockerfile-app]: Temel alınan Node.js sürümünün görüntülendiği bir statik web sayfası işleyen küçük bir Node.js web uygulaması. Sürüm dizesinin simülasyonu yapılır ve bu, temel görüntüde tanımlanan `NODE_VERSION` ortam değişkeninin içeriğini görüntüler.

[Dockerfile-base][dockerfile-base]: `Dockerfile-app` tarafından kendi temeli olarak belirtilen görüntü. Bunun kendisi de [Node][base-node] görüntüsünü temel alır ve `NODE_VERSION` ortam değişkenini içerir.

Aşağıdaki bölümlerde bir derleme görevi oluşturacak, temel görüntü Dockerfile içinde `NODE_VERSION` değerini güncelleştirecek ve sonra da ACR Build'i kullanarak temel görüntü oluşturacaksınız. ACR Build yeni temel görüntüyü kayıt defterinize gönderdikten sonra, uygulama görüntüsünün derlemesini otomatik olarak tetikler. İsteğe bağlı olarak, derleme görüntülerinde farklı sürüm dizeleri görmek için uygulama kapsayıcısı görüntüsünü yerel olarak çalıştırırsınız.

## <a name="build-base-image"></a>Temel görüntü oluşturma

Başlangıç olarak ACR Build *Hızlı Derlemesi* ile temel görüntüyü oluşturun. Serinin [ilk öğreticisinde](container-registry-tutorial-quick-build.md) açıklandığı gibi, bu yalnızca görüntüyü oluşturmakla kalmaz, oluşturma başarılı olduysa bunu kapsayıcınızın kayıt defterine de gönderir.

```azurecli-interactive
az acr build --registry $ACR_NAME --image baseimages/node:9-alpine --file Dockerfile-base .
```

## <a name="create-build-task"></a>Derleme görevi oluşturma

Ardından, [az acr build-task create][az-acr-build-task-create] ile bir derleme görevi oluşturun:

```azurecli-interactive
az acr build-task create \
    --registry $ACR_NAME \
    --name buildhelloworld \
    --image helloworld:{{.Build.ID}} \
    --build-arg REGISTRY_NAME=$ACR_NAME.azurecr.io \
    --context https://github.com/$GIT_USER/acr-build-helloworld-node \
    --file Dockerfile-app \
    --branch master \
    --git-access-token $GIT_PAT
```

Bu derleme görevi, [önceki öğreticide](container-registry-tutorial-build-task.md) oluşturulan göreve benzer. ACR Build'e, işlemeler `--context` tarafından belirtilen depoya gönderildiğinde bir görüntü derlemesi tetiklemesini bildirir.

Farklılık davranışındadır çünkü *temel görüntüsü* güncelleştirildiğinde de bir görüntü derlemesi tetikler. `--file` bağımsız değişkeni tarafından belirtilen Dockerfile ([Dockerfile-app][dockerfile-app]), aynı kayıt defteri içinden kendi temeli olarak bir görüntü belirtmeyi destekler:

```Dockerfile
FROM ${REGISTRY_NAME}/baseimages/node:9-alpine
```

Derleme görevini çalıştırdığınızda, ACR Build görüntünün bağımlılıklarını algılar. `FROM` deyiminde belirtilen temel görüntü aynı kayıt defteri içinde yer alıyorsa, temeli her güncelleştirildiğinde bu görüntünün yeniden oluşturulmasını sağlamak üzere bir kanca ekler.

> [!NOTE]
> Önizleme sırasında ACR Build türetilmiş görüntü derlemesini yalnızca hem temel görüntünün hem de buna başvuran görüntünün aynı Azure kapsayıcı kayıt defterinde yer alması durumunda destekler.

## <a name="build-application-container"></a>Uygulama kapsayıcısı oluşturma

ACR Build'in kapsayıcı görüntüsünün bağımlılıklarını belirlemesine ve izlemesine olanak tanımak için (kendi temel görüntüsünü de içerir), önce derleme görevini **en az bir kere** tetiklemeniz **gerekir**.

Derleme görevini el ile tetiklemek ve uygulama görüntüsünü oluşturmak için [az acr build-task run][az-acr-build-task-run] kullanın:

```azurecli-interactive
az acr build-task run --registry $ACR_NAME --name buildhelloworld
```

Derleme tamamlandıktan sonra, aşağıdaki isteğe bağlı adımı tamamlamak istiyorsanız **Build ID** (örneğin, "eastus6") değerini not alın.

### <a name="optional-run-application-container-locally"></a>İsteğe bağlı: Uygulama kapsayıcısını yerel olarak çalıştırma

Cloud Shell'de değil de yerel olarak çalışıyorsanız ve Docker'ı yüklediyseniz, temel görüntüsünü oluşturmadan önce web tarayıcısında işlenen uygulamayı görmek için kapsayıcıyı çalıştırın. Cloud Shell kullanıyorsanız bu bölümü atlayın (Cloud Shell `az acr login` veya `docker run` komutunu desteklemez).

İlk olarak, [az acr login][az-acr-login] ile kapsayıcınızın kayıt defterinde oturum açın:

```azurecli
az acr login --name $ACR_NAME
```

Şimdi `docker run` ile kapsayıcıyı yerel olarak çalıştırın. **\<build-id\>** değerini önceki adımdaki çıkışta bulunan Build ID değeriyle (örneğin, "eastus5") değiştirin.

```azurecli
docker run -d -p 8080:80 $ACR_NAME.azurecr.io/helloworld:<build-id>
```

Tarayıcınızda http://localhost:8080 adresine gidin; aşağıdakine benzer biçimde web sayfasında işlenmiş Node.js sürüm numarasını görüyor olmalısınız. Sonraki adımlardan birinde, sürüm dizesine bir "a" ekleyerek sürümü yükseltirsiniz.

![Tarayıcıda işlenen örnek uygulamanın ekran görüntüsü][base-update-01]

## <a name="list-builds"></a>Derlemeleri listeleme

Ardından, ACR Build'in kayıt defteriniz için tamamladığı derlemeleri listeleyin:

```azurecli-interactive
az acr build-task list-builds --registry $ACR_NAME --output table
```

Önceki öğreticiyi tamamladıysanız (ve kayıt defterini silmediyseniz), aşağıdakine benzer bir çıkış görüyor olmalısınız. Sonraki bölümde temel görüntüyü güncelleştirdikten sonra çıkışı karşılaştırabilmek için, derleme sayısını ve en son BUILD ID değerini not alın.

```console
$ az acr build-task list-builds --registry $ACR_NAME --output table
BUILD ID    TASK             PLATFORM    STATUS     TRIGGER       STARTED               DURATION
----------  ---------------  ----------  ---------  ------------  --------------------  ----------
eastus6     buildhelloworld  Linux       Succeeded  Manual        2018-04-22T00:03:46Z  00:00:40
eastus5                                  Succeeded  Manual        2018-04-22T00:01:45Z  00:00:25
eastus4     buildhelloworld  Linux       Succeeded  Git Commit    2018-04-21T23:52:33Z  00:00:30
eastus3     buildhelloworld  Linux       Succeeded  Manual        2018-04-21T23:50:10Z  00:00:35
eastus2     buildhelloworld  Linux       Succeeded  Manual        2018-04-21T23:46:15Z  00:00:55
eastus1                                  Succeeded  Manual        2018-04-21T23:24:05Z  00:00:35
```

## <a name="update-base-image"></a>Temel görüntüyü güncelleştirme

Burada, temel görüntüde bir çerçeve yamasının simülasyonunu yaparsınız. **Dockerfile-base** öğesini düzenleyin ve `NODE_VERSION` içinde tanımlanan sürüm numarasından sonra bir "a" ekleyin:

```Dockerfile
ENV NODE_VERSION 9.11.1a
```

Değiştirilmiş temel görüntüyü derlemek için ACR Build'de bir Hızlı Derleme çalıştırın. Çıkıştaki **Derleme Kimliği** değerini not alın.

```azurecli-interactive
az acr build --registry $ACR_NAME --image baseimages/node:9-alpine --file Dockerfile-base .
```

Derleme tamamlandıktan ve ACR Build yeni temel görüntüyü kayıt defterinize gönderdikten sonra, uygulama görüntüsünün derlemesini tetikler. Daha önce oluşturduğunuz ACR Build görevinin uygulama görüntüsü derlemeyi tetiklemesi birkaç dakika sürebilir çünkü yeni tamamlanan ve gönderilen temel görüntüyü algılaması gerekir.

## <a name="list-builds"></a>Derlemeleri listeleme

Artık temel görüntüyü güncelleştirdiğinize göre, derlemelerinizi bir kez daha listeleyip önceki listeyle bunu karşılaştırın. İlk seferinde çıkış farklı görünmüyorsa, yeni derlemenin listede yer aldığını görmek için komutu düzenli aralıklarla çalıştırın.

```azurecli-interactive
az acr build-task list-builds --registry $ACR_NAME --output table
```

Çıktı aşağıdakine benzer olacaktır. Son yürütülen derlemenin TRIGGER değeri "Image Update" olmalıdır; bu değer, derleme görevinin temel görüntünüzün Hızlı Derlemesi ile başlatıldığını gösterir.

```console
$ az acr build-task list-builds --registry $ACR_NAME --output table
BUILD ID    TASK             PLATFORM    STATUS     TRIGGER       STARTED               DURATION
----------  ---------------  ----------  ---------  ----------    --------------------  ----------
eastus8     buildhelloworld  Linux       Succeeded  Image Update  2018-04-22T00:09:24Z  00:00:50
eastus7                                  Succeeded  Manual        2018-04-22T00:08:49Z  00:00:40
eastus6     buildhelloworld  Linux       Succeeded  Image Update  2018-04-20T00:15:30Z  00:00:43
eastus5     buildhelloworld  Linux       Succeeded  Manual        2018-04-20T00:10:05Z  00:00:45
eastus4     buildhelloworld  Linux       Succeeded  Git Commit    2018-04-19T23:40:38Z  00:00:40
eastus3     buildhelloworld  Linux       Succeeded  Manual        2018-04-19T23:36:37Z  00:00:40
eastus2     buildhelloworld  Linux       Succeeded  Manual        2018-04-19T23:35:27Z  00:00:40
eastus1                                  Succeeded  Manual        2018-04-19T22:51:13Z  00:00:30
```

Yeni oluşturulan kapsayıcıyı çalıştırıp güncelleştirilmiş sürüm numarasını görmek için aşağıdaki isteğe bağlı adımı uygulamak isterseniz, Görüntü Güncelleştirmesi ile tetiklenen derlemenin **BUILD ID** değerini (önceki çıkışta "eastus6") not alın.

### <a name="optional-run-newly-built-image"></a>İsteğe bağlı: Yeni oluşturulan görüntüyü çalıştırma

Cloud Shell'de değil de yerel olarak çalışıyorsanız ve Docker'ı yüklediyseniz, derlemesi tamamlandıktan sonra yeni uygulama görüntüsünü çalıştırın. `<build-id>` değerinin yerine önceki adımda aldığınız BUILD ID değerini koyun. Cloud Shell kullanıyorsanız bu bölümü atlayın (Cloud Shell `docker run` komutunu desteklemez).

```bash
docker run -d -p 8081:80 $ACR_NAME.azurecr.io/helloworld:<build-id>
```

Tarayıcınızda http://localhost:8081 adresine gidin; web sayfasında güncelleştirilmiş Node.js sürüm numarasını ("a" ile) görüyor olmalısınız:

![Tarayıcıda işlenen örnek uygulamanın ekran görüntüsü][base-update-02]

**Temel** görüntünüzü yeni sürüm numarasıyla güncelleştirdiğinize ama son oluşturulan **uygulama** görüntüsünde yeni sürümün görüntülendiğine dikkat etmelisiniz. ACR Build temel görüntüde yaptığınız değişikliği almış ve uygulama görüntünüzü otomatik olarak yeniden oluşturmuştur.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kapsayıcı kayıt defteri, kapsayıcı örneğin, anahtar kasası ve hizmet sorumlusu dahil olmak üzere bu öğretici serisinde oluşturduğunuz tüm kaynakları kaldırmak için aşağıdaki komutları gönderin:

```azurecli-interactive
az group delete --resource-group $RES_GROUP
az ad sp delete --id http://$ACR_NAME-pull
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, temel görüntü güncelleştirildiğinde kapsayıcı görüntü derlemelerini otomatik olarak tetiklemek üzere bir derleme görevi kullanmayı öğrendiniz. Artık kapsayıcı kayıt defteriniz için kimlik doğrulaması konusunu öğrenmeye geçebilirsiniz.

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
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - Internal -->
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-build]: /cli/azure/acr#az-acr-build-run
[az-acr-build-task-create]: /cli/azure/acr#az-acr-build-task-create
[az-acr-build-task-run]: /cli/azure/acr#az-acr-build-task-run
[az-acr-login]: /cli/azure/acr#az-acr-login

<!-- IMAGES -->
[base-update-01]: ./media/container-registry-tutorial-base-image-update/base-update-01.png
[base-update-02]: ./media/container-registry-tutorial-base-image-update/base-update-02.png