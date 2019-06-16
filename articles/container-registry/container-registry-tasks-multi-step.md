---
title: Görüntü derleme, test ve düzeltme eki Azure Container Registry çok adımlı görevler ile otomatik hale getirin
description: Giriş çok adımlı görevler, görev tabanlı iş akışları oluşturma, test etme ve bulutta kapsayıcı görüntülerini düzeltme sağlayan bir Azure Container Registry ACR görevlerinde bir özelliğidir.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 03/28/2019
ms.author: danlep
ms.openlocfilehash: ac0e4e9019a35d3fdb35c0b7af9cb1289f4bceeb
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60829591"
---
# <a name="run-multi-step-build-test-and-patch-tasks-in-acr-tasks"></a>ACR görevleri çok adımlı derleme, test ve düzeltme eki görevleri Çalıştır

Çok adımlı görevler tek görüntü derleme ve anında iletme özelliği ACR görevleri çok adımlı, çok container tabanlı iş akışları ile genişletin. Çok adımlı Görevler oluşturun ve birkaç görüntüyü serisindeki veya paralel göndermek için kullanır. Ardından bu görüntüleri olarak tek bir görevi çalıştırma komutlarını çalıştırın. Her adım, bir kapsayıcı görüntüsü tanımlar oluşturun veya gönderme işlemi ve kapsayıcı yürütülmesini de tanımlayabilirsiniz. Çok adımlı görev her adımda bir kapsayıcı, yürütme ortamı olarak kullanır.

> [!IMPORTANT]
> Önizlemede daha önce `az acr build-task` komutuyla görev oluşturduysanız [az acr task][az-acr-task] komutuyla bu görevleri yeniden oluşturmanız gerekebilir.

Örneğin, aşağıdaki mantık otomatikleştirmek adımlarla bir görev çalıştırabilirsiniz:

1. Bir web uygulama görüntüsü oluşturun
1. Web uygulaması kapsayıcısını çalıştırın
1. Bir web uygulamasını test görüntüsü oluşturun
1. Testleri çalıştırma karşı gerçekleştiren web uygulaması test kapsayıcısının çalıştırmak uygulama kapsayıcısı
1. Testler başarıyla geçirilirse, bir Helm grafiği arşiv paketi oluşturun
1. Gerçekleştirmek bir `helm upgrade` yeni Helm grafiği arşiv paketi kullanma

Tüm adımlar, Azure, Azure'un işlem kaynakları için iş boşaltma ve altyapı yönetiminden boşaltma içinde gerçekleştirilir. Azure kapsayıcı kayıt defterinizde yanı sıra, yalnızca kullandığınız kaynaklar için ödeme yaparsınız. Fiyatlandırma hakkında daha fazla bilgi için bkz. **kapsayıcı derleme** konusundaki [Azure Container Registry fiyatlandırma][pricing].


## <a name="common-task-scenarios"></a>Görev senaryoları

Çok adımlı görevler aşağıdaki mantık gibi senaryolara olanak tanır:

* Derleme, etiketi ve bir veya daha fazla kapsayıcı görüntüleri, paralel veya seri gönderin.
* Çalıştırın ve birim test ve kod kapsamı sonuçlarını yakalayın.
* Çalıştırın ve işlevsel testleri yakalayın. ACR görevleri birden fazla kapsayıcı, çalışan bir dizi isteğin aralarında yürütülmesini destekler.
* Kapsayıcı görüntü derlemesi ön/son adımları dahil olmak üzere, görev tabanlı yürütme gerçekleştirin.
* Sık kullandığınız dağıtım altyapınız hedef ortamınız için bir veya daha fazla kapsayıcıları dağıtın.

## <a name="multi-step-task-definition"></a>Çok adımlı görev tanımı

ACR görevleri çok adımlı bir görevde bir YAML dosyası içinde bir dizi olarak tanımlanır. Her adım, bir veya daha fazla önceki adımları başarıyla tamamlandığında bağımlılıkları belirtebilirsiniz. Aşağıdaki görev adım türleri kullanılabilir:

* [`build`](container-registry-tasks-reference-yaml.md#build): Tanıdık kullanarak bir veya daha fazla kapsayıcı görüntüleri oluşturma `docker build` söz dizimi, paralel veya seri.
* [`push`](container-registry-tasks-reference-yaml.md#push): Yerleşik görüntüleri bir kapsayıcı kayıt defterine gönderin. Azure Container Registry gibi özel kayıt defterleri, genel Docker hub'ı olarak desteklenir.
* [`cmd`](container-registry-tasks-reference-yaml.md#cmd): Çalışan görev bağlamında bir işlevi olarak çalışabilir, bir kapsayıcı çalıştırın. Kapsayıcının parametreler geçirebilir `[ENTRYPOINT]`ve env gibi özellikleri belirtin, ayırma ve diğer tanıdık `docker run` parametreleri. `cmd` Adım türü, birim ve işlevsel test, eş zamanlı kapsayıcı yürütme ile etkinleştirir.

Aşağıdaki kod parçacıkları, bu görev adımı türlerini birleştirme işlemini göstermektedir. Çok adımlı görevler olarak bir Dockerfile tek bir görüntü oluşturma ve benzer bir YAML dosyası ile kayıt defterine gönderme gibi basit olabilir:

```yml
version: v1.0.0
steps:
  - build: -t {{.Run.Registry}}/hello-world:{{.Run.ID}} .
  - push: ["{{.Run.Registry}}/hello-world:{{.Run.ID}}"]
```

Veya daha karmaşık, derleme için adımları içeren kurgusal bu çok adımlı tanımı gibi test, helm paket ve helm (kapsayıcı kayıt defteri ve Helm deposu yapılandırması gösterilmez) dağıtın:

```yml
version: v1.0.0
steps:
  - id: build-web
    build: -t {{.Run.Registry}}/hello-world:{{.Run.ID}} .
    when: ["-"]
  - id: build-tests
    build -t {{.Run.Registry}}/hello-world-tests ./funcTests
    when: ["-"]
  - id: push
    push: ["{{.Run.Registry}}/helloworld:{{.Run.ID}}"]
    when: ["build-web", "build-tests"]
  - id: hello-world-web
    cmd: {{.Run.Registry}}/helloworld:{{.Run.ID}}
  - id: funcTests
    cmd: {{.Run.Registry}}/helloworld:{{.Run.ID}}
    env: ["host=helloworld:80"]
  - cmd: {{.Run.Registry}}/functions/helm package --app-version {{.Run.ID}} -d ./helm ./helm/helloworld/
  - cmd: {{.Run.Registry}}/functions/helm upgrade helloworld ./helm/helloworld/ --reuse-values --set helloworld.image={{.Run.Registry}}/helloworld:{{.Run.ID}}
```

Bkz: [görev örnekleri] [ task-examples] için birkaç senaryo için çok adımlı görev YAML dosyalarını ve dockerfile'ları tamamlayın.

## <a name="run-a-sample-task"></a>Bir örnek görevini Çalıştır

"Hızlı Çalıştır" adlı iki el ile yürütme görevlerini destekler ve Git işleme veya temel görüntüye otomatik yürütme güncelleştirin.

Görevi çalıştırmak için önce görevin adımları bir YAML dosyası içinde tanımlayın, ardından Azure CLI komutu yürüterek [çalıştırma az acr][az-acr-run].

Örnek görev YAML dosyası kullanarak bir görev çalıştırdığında Azure CLI komutunu bir örnek aşağıda verilmiştir. Adımları, oluşturun ve ardından görüntüyü gönderin. Güncelleştirme `\<acrName\>` komutu çalıştırmadan önce kendi Azure kapsayıcı kayıt defteri adı.

```azurecli
az acr run --registry <acrName> -f build-push-hello-world.yaml https://github.com/Azure-Samples/acr-tasks.git
```

Görevini çalıştırdığınızda, çıkış YAML dosyasında tanımlanan her bir adımın durumunu göstermesi gerekir. Adımları aşağıdaki çıktıda görünür `acb_step_0` ve `acb_step_1`.

```console
$ az acr run --registry myregistry -f build-push-hello-world.yaml https://github.com/Azure-Samples/acr-tasks.git
Sending context to registry: myregistry...
Queued a run with ID: yd14
Waiting for an agent...
2018/09/12 20:08:44 Using acb_vol_0467fe58-f6ab-4dbd-a022-1bb487366941 as the home volume
2018/09/12 20:08:44 Creating Docker network: acb_default_network
2018/09/12 20:08:44 Successfully set up Docker network: acb_default_network
2018/09/12 20:08:44 Setting up Docker configuration...
2018/09/12 20:08:45 Successfully set up Docker configuration
2018/09/12 20:08:45 Logging in to registry: myregistry.azurecr-test.io
2018/09/12 20:08:46 Successfully logged in
2018/09/12 20:08:46 Executing step: acb_step_0
2018/09/12 20:08:46 Obtaining source code and scanning for dependencies...
2018/09/12 20:08:47 Successfully obtained source code and scanned for dependencies
Sending build context to Docker daemon  109.6kB
Step 1/1 : FROM hello-world
 ---> 4ab4c602aa5e
Successfully built 4ab4c602aa5e
Successfully tagged myregistry.azurecr-test.io/hello-world:yd14
2018/09/12 20:08:48 Executing step: acb_step_1
2018/09/12 20:08:48 Pushing image: myregistry.azurecr-test.io/hello-world:yd14, attempt 1
The push refers to repository [myregistry.azurecr-test.io/hello-world]
428c97da766c: Preparing
428c97da766c: Layer already exists
yd14: digest: sha256:1a6fd470b9ce10849be79e99529a88371dff60c60aab424c077007f6979b4812 size: 524
2018/09/12 20:08:55 Successfully pushed image: myregistry.azurecr-test.io/hello-world:yd14
2018/09/12 20:08:55 Step id: acb_step_0 marked as successful (elapsed time in seconds: 2.035049)
2018/09/12 20:08:55 Populating digests for step id: acb_step_0...
2018/09/12 20:08:57 Successfully populated digests for step id: acb_step_0
2018/09/12 20:08:57 Step id: acb_step_1 marked as successful (elapsed time in seconds: 6.832391)
The following dependencies were found:
- image:
    registry: myregistry.azurecr-test.io
    repository: hello-world
    tag: yd14
    digest: sha256:1a6fd470b9ce10849be79e99529a88371dff60c60aab424c077007f6979b4812
  runtime-dependency:
    registry: registry.hub.docker.com
    repository: library/hello-world
    tag: latest
    digest: sha256:0add3ace90ecb4adbf7777e9aacf18357296e799f81cabc9fde470971e499788
  git: {}


Run ID: yd14 was successful after 19s
```

Git işleme veya temel görüntü güncelleştirme otomatik derlemeler hakkında daha fazla bilgi için bkz. [görüntü derlemeleri otomatikleştirme](container-registry-tutorial-build-task.md) ve [temel görüntü güncelleştirme derlemeleri](container-registry-tutorial-base-image-update.md) öğretici makaleleriyle.

## <a name="next-steps"></a>Sonraki adımlar

Çok adımlı görev başvurusu ve örnekleri aşağıda bulabilirsiniz:

* [Görev başvurusu](container-registry-tasks-reference-yaml.md) -görev adım türler, özellikler ve kullanım.
* [Görev örnekleri] [ task-examples] -örnek `task.yaml` çeşitli senaryoları, karmaşık için basit için dosyaları.
* [Cmd depo](https://github.com/AzureCR/cmd) -ACR görevleri için komutları olarak kapsayıcılar koleksiyonu.

<!-- IMAGES -->

<!-- LINKS - External -->
[pricing]: https://azure.microsoft.com/pricing/details/container-registry/
[task-examples]: https://github.com/Azure-Samples/acr-tasks
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - Internal -->
[az-acr-task-create]: /cli/azure/acr/task#az-acr-task-create
[az-acr-run]: /cli/azure/acr#az-acr-run
[az-acr-task]: /cli/azure/acr/task