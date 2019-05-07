---
title: Azure kapsayıcı kayıt defteri görevleri başvurusu - YAML
description: ACR görev özellikleri, adım türleri, adım özellikleri ve yerleşik değişkenler dahil olmak üzere görev için YAML içinde görevleri tanımlama için başvuru.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 03/28/2019
ms.author: danlep
ms.openlocfilehash: d50d5bc91fbb86e5c0c3d2acc3b55c7d02c71723
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65192260"
---
# <a name="acr-tasks-reference-yaml"></a>ACR görevleri başvurusu: YAML

Çok adımlı görev tanımı ACR görevleri oluşturmaya, sınamaya ve kapsayıcıları düzeltme eki uygulama üzerinde odaklanmış bir kapsayıcı merkezli işlem temel sağlar. Bu makale, komutlar, Parametreler, özellikleri ve, çok adımlı görevler tanımlayın YAML dosyalar için söz dizimi kapsar.

Bu makale, çok adımlı görev ACR görev için YAML dosyaları oluşturmak için başvuru içerir. ACR görevleri giriş isterseniz bkz [ACR görevlere genel bakış](container-registry-tasks-overview.md).

## <a name="acr-taskyaml-file-format"></a>ACR task.yaml dosya biçimi

ACR görevleri çok adımlı görev bildirimi standart YAML sözdizimini destekler. Bir YAML dosyası içinde bir görevin adımları tanımlarsınız. Ardından görev el ile dosyaya geçirerek çalıştırabilirsiniz [çalıştırma az acr] [ az-acr-run] komutu. Veya, bir görev oluşturmak için bu dosyayı kullanın [az acr görev oluşturma] [ az-acr-task-create] , tetiklenir otomatik olarak bir Git commit veya temel görüntü güncelleştirme. Bu makalede başvuruyor ancak `acr-task.yaml` adımları içeren dosya geçerli bir dosya ile ACR görevleri destekleyen bir [uzantı desteklenen](#supported-task-filename-extensions).

Üst düzey `acr-task.yaml` ilkelleri olan **görev özellikleri**, **adım türleri**, ve **adım özellikleri**:

* [Görev Özellikleri](#task-properties) tüm adımları görev yürütme boyunca geçerlidir. Dahil olmak üzere çeşitli genel görev özellikleri vardır:
  * `version`
  * `stepTimeout`
  * `workingDirectory`
* [Görev Adım türleri](#task-step-types) görevde gerçekleştirilebilecek eylemler türlerini temsil eder. Üç adım vardır:
  * `build`
  * `push`
  * `cmd`
* [Adım özellikleri görev](#task-step-properties) tek bir adımı için geçerli parametreler. Birkaç adım özellikleri vardır:
  * `startDelay`
  * `timeout`
  * `when`
  * .. .ve çok daha fazlası.

Temel biçimi bir `acr-task.yaml` bazı ortak adım özellikleri dahil olmak üzere, dosya izler. Bir ayrıntılı gösterimini değil tüm kullanılabilir adım özellikleri veya adım türü kullanımı sırasında temel dosya biçimi hızlı bir genel bakış sağlar.

```yml
version: # acr-task.yaml format version.
stepTimeout: # Seconds each step may take.
steps: # A collection of image or container actions.
  - build: # Equivalent to "docker build," but in a multi-tenant environment
  - push: # Push a newly built or retagged image to a registry.
    when: # Step property that defines either parallel or dependent step execution.
  - cmd: # Executes a container, supports specifying an [ENTRYPOINT] and parameters.
    startDelay: # Step property that specifies the number of seconds to wait before starting execution.
```

### <a name="supported-task-filename-extensions"></a>Desteklenen görev dosya adı uzantıları

ACR görevler gibi çeşitli dosya adı uzantıları ayrılmış `.yaml`, bir görev dosyası olarak işleyecek. Uzantıyı *değil* aşağıdaki listede, bir Dockerfile olarak ACR görevler tarafından kabul edilir: .yaml .yml, .toml, .json, .sh, .bash, .zsh, .ps1, .ps, .cmd, .bat, .ts, .js, .php, .py, .rb, .lua

YAML, ACR görevler tarafından şu anda desteklenen tek dosya biçimidir. Bir dosya adı uzantıları, olası gelecek destek için ayrılmıştır.

## <a name="run-the-sample-tasks"></a>Örnek görevleri çalıştırma

Aşağıdaki bölümlerde, bu makalenin başvurulan birkaç örnek görev dosyası vardır. Genel bir GitHub deposunda örnek görevleridir [Azure-Samples/acr-görevleri][acr-tasks]. Azure CLI komutuyla çalıştırabilirsiniz [çalıştırma az acr][az-acr-run]. Örnek komutlar için benzerdir:

```azurecli
az acr run -f build-push-hello-world.yaml https://github.com/Azure-Samples/acr-tasks.git
```

Örnek komutlar biçimlendirme, yapılandırdığınız bir varsayılan kayıt defterine Azure CLI, bunlar atlamak için varsayar `--registry` parametresi. Varsayılan kayıt defteri yapılandırmak için kullanın [az yapılandırma] [ az-configure] komutunu `--defaults` parametresini kabul eden bir `acr=REGISTRY_NAME` değeri.

Örneğin, "myregistry" adlı varsayılan kayıt defteri ile Azure CLI'yı yapılandırmak için:

```azurecli
az configure --defaults acr=myregistry
```

## <a name="task-properties"></a>Görev Özellikleri

Görev özellikleri genellikle üst kısmında görünür bir `acr-task.yaml` dosyasını bulun ve tam yürütülmesini görev adımları uygulanan genel özellikler. Bu genel özelliklerin bazıları içinde tek bir adımı kılınabilir.

| Özellik | Tür | İsteğe bağlı | Açıklama | Desteklenen bir geçersiz kılma | Varsayılan değer |
| -------- | ---- | -------- | ----------- | ------------------ | ------------- |
| `version` | string | Evet | Sürümü `acr-task.yaml` ACR görevleri hizmeti tarafından ayrıştırılan gibi dosya. Geriye dönük uyumluluğu korumak ACR görevleri içindedir, ancak bu değer, ACR içinde tanımlı bir sürüm uyumluluğu korumak için görevleri sağlar. En son sürüme belirtilmemişse, varsayılan olarak. | Hayır | None |
| `stepTimeout` | int (saniye) | Evet | Bir adım çalıştırabilirsiniz saniye sayısı. Özelliği bir görevde belirtilmezse, varsayılan ayarlar `timeout` tüm adımları özelliğidir. Varsa `timeout` özelliği belirtilen bir adımında, görev tarafından sağlanan özelliğini geçersiz kılar. | Evet | 600 (10 dakika) |
| `workingDirectory` | string | Evet | Kapsayıcı çalışma zamanı sırasında çalışma dizini. Özelliği bir görevde belirtilmezse, varsayılan ayarlar `workingDirectory` tüm adımları özelliğidir. Bir adımı belirtilmişse, görev tarafından sağlanan özelliğini geçersiz kılar. | Evet | `$HOME` |
| `env` | [dize, dize,...] | Evet |  Dizeler dizisi `key=value` görev için ortam değişkenlerini biçimi. Özelliği bir görevde belirtilmezse, varsayılan ayarlar `env` tüm adımları özelliğidir. Bir adımı belirtilmişse görevden devralınır tüm ortam değişkenlerini geçersiz kılar. | None |
| `secrets` | [gizli anahtar, gizli,...] | Evet | Dizi [gizli](#secret) nesneleri. | None |
| `networks` | [ağ, ağ,...] | Evet | Dizi [ağ](#network) nesneleri. | None |

### <a name="secret"></a>gizli dizi

Gizli dizi nesnesi, aşağıdaki özelliklere sahiptir.

| Özellik | Tür | İsteğe bağlı | Açıklama | Varsayılan değer |
| -------- | ---- | -------- | ----------- | ------- |
| `id` | string | Hayır | Gizli dizi tanımlayıcısı. | None |
| `akv` | string | Evet | Azure Key Vault (AKV) gizli URL'si. | None |
| `clientID` | string | Evet | Kullanıcı tarafından atanan istemci kimliği yönetilen Azure kaynakları için kimliği. | None |

### <a name="network"></a>Ağ

Ağ nesnesini aşağıdaki özelliklere sahiptir.

| Özellik | Tür | İsteğe bağlı | Açıklama | Varsayılan değer |
| -------- | ---- | -------- | ----------- | ------- | 
| `name` | string | Hayır | Ağ adı. | None |
| `driver` | string | Evet | Ağı yönetmek için sürücü. | None |
| `ipv6` | bool | Evet | IPv6 ağ etkinleştirilip etkinleştirilmediği. | `false` |
| `skipCreation` | bool | Evet | Ağ oluşturma atlanmayacağını belirtir. | `false` |
| `isDefault` | bool | Evet | Azure Container Registry ile sağlanan varsayılan bir ağ ağ olup | `false` |

## <a name="task-step-types"></a>Görev Adım türü

ACR görevleri üç adım türlerini destekler. Her adım türü bölümünde her bir adım türü için ayrıntılı çeşitli özelliklerini destekler.

| Adım türü | Açıklama |
| --------- | ----------- |
| [`build`](#build) | Tanıdık kullanarak bir kapsayıcı görüntüsü derler `docker build` söz dizimi. |
| [`push`](#push) | Yürüten bir `docker push` bir kapsayıcı kayıt defterine yeni oluşturulmuş veya retagged görüntüleri. Azure Container Registry, diğer özel kayıt defterlerini ve genel Docker Hub desteklenir. |
| [`cmd`](#cmd) | Kapsayıcı parametrelere sahip bir komut olarak geçirilen kapsayıcının çalışır `[ENTRYPOINT]`. `cmd` Adım türü gibi parametrelerin destekler `env`, `detach`ve diğer tanıdık `docker run` komut, birim ve işlevsel test kapsayıcı eş zamanlı yürütme ile etkinleştirme seçenekleri. |

## <a name="build"></a>Derleme

Bir kapsayıcı görüntüsü oluşturun. `build` Adım türünü temsil eder, çalışan çok kiracılı, güvenli bir çeşit `docker build` birinci sınıf bir temel olarak bulutta.

### <a name="syntax-build"></a>Sözdizimi: derleme

```yml
version: v1.0.0
steps:
  - [build]: -t [imageName]:[tag] -f [Dockerfile] [context]
    [property]: [value]
```

`build` Adım türü parametreleri aşağıdaki tabloda destekler. `build` Adım türü de destekler, tüm derleme seçenekleri [docker derleme](https://docs.docker.com/engine/reference/commandline/build/) komutu gibi `--build-arg` derleme zamanı değişkenlerini ayarlamak için.

| Parametre | Açıklama | İsteğe bağlı |
| --------- | ----------- | :-------: |
| `-t` &#124; `--image` | Tam tanımlar `image:tag` yerleşik resmin.<br /><br />Tüm görüntüleri görüntüleri gibi işlevsel testler iç görev doğrulama için kullanılabilir olarak gerektiren `push` bir kayıt defterine. Ancak, bir görüntü içindeki görev yürütme örneği, görüntünün başvurmak için bir ad gerek yoktur.<br /><br />Aksine `az acr build`, ACR görevleri çalıştıran gönderme davranışı varsayılan sunmaz. ACR görevlerle varsayılan senaryoyu oluşturmak, doğrulamak ve görüntü gönderebilmeniz olanağı varsayar. Bkz: [anında iletme](#push) için isteğe bağlı olarak anında iletme yerleşik görüntüleri hakkında. | Evet |
| `-f` &#124; `--file` | Geçirilen Dockerfile belirtir `docker build`. Belirtilmezse, varsayılan bağlamı kökünde Dockerfile varsayılır. Bir Dockerfile belirtmek için bağlam köküne dosya adı geçirin. | Evet |
| `context` | Kök dizin geçirilen `docker build`. Her görev kök dizini ayarlamak için bir paylaşılan [workingDirectory](#task-step-properties)ve ilişkili Git kopyalanan dizininin kökü içerir. | Hayır |

### <a name="properties-build"></a>Özellikler: derleme

`build` Adım türü aşağıdaki özellikleri destekler. Bu özellikleri ayrıntılarını bulun [görev adım özellikleri](#task-step-properties) bu makalenin.

| | | |
| -------- | ---- | -------- |
| `detach` | bool | İsteğe bağlı |
| `disableWorkingDirectoryOverride` | bool | İsteğe bağlı |
| `entryPoint` | string | İsteğe bağlı |
| `env` | [dize, dize,...] | İsteğe bağlı |
| `expose` | [dize, dize,...] | İsteğe bağlı |
| `id` | string | İsteğe bağlı |
| `ignoreErrors` | bool | İsteğe bağlı |
| `isolation` | string | İsteğe bağlı |
| `keep` | bool | İsteğe bağlı |
| `network` | object | İsteğe bağlı |
| `ports` | [dize, dize,...] | İsteğe bağlı |
| `pull` | bool | İsteğe bağlı |
| `repeat` | int | İsteğe bağlı |
| `retries` | int | İsteğe bağlı |
| `retryDelay` | int (saniye) | İsteğe bağlı |
| `secret` | object | İsteğe bağlı |
| `startDelay` | int (saniye) | İsteğe bağlı |
| `timeout` | int (saniye) | İsteğe bağlı |
| `when` | [dize, dize,...] | İsteğe bağlı |
| `workingDirectory` | string | İsteğe bağlı |

### <a name="examples-build"></a>Örnekler: oluşturma

#### <a name="build-image---context-in-root"></a>Görüntü - kök bağlamda oluşturun

```azurecli
az acr run -f build-hello-world.yaml https://github.com/AzureCR/acr-tasks-sample.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/build-hello-world.yaml -->
[!code-yml[task](~/acr-tasks/build-hello-world.yaml)]

#### <a name="build-image---context-in-subdirectory"></a>Görüntü - alt bağlamda oluşturun

```yml
version: v1.0.0
steps:
  - build: -t {{.Run.Registry}}/hello-world -f hello-world.dockerfile ./subDirectory
```

## <a name="push"></a>anında iletme

Bir anında iletme veya daha fazla yerleşik veya bir kapsayıcı kayıt defterine görüntü retagged. Azure Container Registry gibi özel kayıt defterleri için veya genel Docker hub'ına gönderme destekler.

### <a name="syntax-push"></a>Sözdizimi: anında iletme

`push` Adım türü görüntü koleksiyonunu destekler. Satır içi hem de iç içe geçmiş biçimleri YAML koleksiyon sözdizimini destekler. Tek bir görüntü gönderme genelde satır içi söz dizimi kullanılarak temsil edilir:

```yml
version: v1.0.0
steps:
  # Inline YAML collection syntax
  - push: ["{{.Run.Registry}}/hello-world:{{.Run.ID}}"]
```

Birden fazla görüntü gönderirken, okunurluğu artırmak için iç içe geçmiş sözdizimini kullanın:

```yml
version: v1.0.0
steps:
  # Nested YAML collection syntax
  - push:
    - {{.Run.Registry}}/hello-world:{{.Run.ID}}
    - {{.Run.Registry}}/hello-world:latest
```

### <a name="properties-push"></a>Özellikler: gönderme

`push` Adım türü aşağıdaki özellikleri destekler. Bu özellikleri ayrıntılarını bulun [görev adım özellikleri](#task-step-properties) bu makalenin.

| | | |
| -------- | ---- | -------- |
| `env` | [dize, dize,...] | İsteğe bağlı |
| `id` | string | İsteğe bağlı |
| `ignoreErrors` | bool | İsteğe bağlı |
| `startDelay` | int (saniye) | İsteğe bağlı |
| `timeout` | int (saniye) | İsteğe bağlı |
| `when` | [dize, dize,...] | İsteğe bağlı |

### <a name="examples-push"></a>Örnekler: anında iletme

#### <a name="push-multiple-images"></a>Birden çok görüntüleri gönderme

```azurecli
az acr run -f build-push-hello-world.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/build-push-hello-world.yaml -->
[!code-yml[task](~/acr-tasks/build-push-hello-world.yaml)]

#### <a name="build-push-and-run"></a>Oluşturma, gönderme ve çalıştırma

```azurecli
az acr run -f build-run-hello-world.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/build-run-hello-world.yaml -->
[!code-yml[task](~/acr-tasks/build-run-hello-world.yaml)]

## <a name="cmd"></a>cmd

`cmd` Adım türü bir kapsayıcı olarak çalıştırılır.

### <a name="syntax-cmd"></a>Sözdizimi: cmd

```yml
version: v1.0.0
steps:
  - [cmd]: [containerImage]:[tag (optional)] [cmdParameters to the image]
```

### <a name="properties-cmd"></a>Özellikler: cmd

`cmd` Adım türü aşağıdaki özellikleri destekler:

| | | |
| -------- | ---- | -------- |
| `detach` | bool | İsteğe bağlı |
| `disableWorkingDirectoryOverride` | bool | İsteğe bağlı |
| `entryPoint` | string | İsteğe bağlı |
| `env` | [dize, dize,...] | İsteğe bağlı |
| `expose` | [dize, dize,...] | İsteğe bağlı |
| `id` | string | İsteğe bağlı |
| `ignoreErrors` | bool | İsteğe bağlı |
| `isolation` | string | İsteğe bağlı |
| `keep` | bool | İsteğe bağlı |
| `network` | object | İsteğe bağlı |
| `ports` | [dize, dize,...] | İsteğe bağlı |
| `pull` | bool | İsteğe bağlı |
| `repeat` | int | İsteğe bağlı |
| `retries` | int | İsteğe bağlı |
| `retryDelay` | int (saniye) | İsteğe bağlı |
| `secret` | object | İsteğe bağlı |
| `startDelay` | int (saniye) | İsteğe bağlı |
| `timeout` | int (saniye) | İsteğe bağlı |
| `when` | [dize, dize,...] | İsteğe bağlı |
| `workingDirectory` | string | İsteğe bağlı |

Bu özellikleri ayrıntılarını bulabilirsiniz [görev adım özellikleri](#task-step-properties) bu makalenin.

### <a name="examples-cmd"></a>Örnekler: cmd

#### <a name="run-hello-world-image"></a>Merhaba-Dünya görüntüsünü çalıştırma

Bu komutu yürütür `hello-world.yaml` başvuran görev dosyası [Merhaba-Dünya](https://hub.docker.com/_/hello-world/) Docker hub'daki resmi.

```azurecli
az acr run -f hello-world.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/hello-world.yaml -->
[!code-yml[task](~/acr-tasks/hello-world.yaml)]

#### <a name="run-bash-image-and-echo-hello-world"></a>Bash görüntüsü ve yankı "hello world" çalıştırın

Bu komutu yürütür `bash-echo.yaml` başvuran görev dosyası [bash](https://hub.docker.com/_/bash/) Docker hub'daki resmi.

```azurecli
az acr run -f bash-echo.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/bash-echo.yaml -->
[!code-yml[task](~/acr-tasks/bash-echo.yaml)]

#### <a name="run-specific-bash-image-tag"></a>Belirli bir bash resim etiketi çalıştırın

Bir özel görüntü sürümünü çalıştırmak üzere etiketinde belirtin `cmd`.

Bu komutu yürütür `bash-echo-3.yaml` başvuran görev dosyası [bash: 3.0](https://hub.docker.com/_/bash/) Docker hub'daki resmi.

```azurecli
az acr run -f bash-echo-3.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/bash-echo-3.yaml -->
[!code-yml[task](~/acr-tasks/bash-echo-3.yaml)]

#### <a name="run-custom-images"></a>Özel görüntüleri çalıştırın

`cmd` Adım türe başvuran standart kullanarak görüntüleri `docker run` biçimi. Kayıt defteri ile başında değil görüntüleri docker.io kaynaklanan varsayılır. Önceki örnekte, eşit olarak gösterilebilir:

```yml
version: v1.0.0
steps:
  - cmd: docker.io/bash:3.0 echo hello world
```

Standart kullanarak `docker run` görüntü başvurusu kuralı `cmd` görüntüleri herhangi bir özel kayıt veya genel Docker hub'ı çalıştırabilirsiniz. ACR görevin yürütüldüğü aynı kayıt defterindeki görüntüleri başvuran herhangi bir kayıt defteri kimlik bilgilerini belirtmeniz gerekmez.

* Bir Azure container registry'den bir görüntü çalıştırma

    Değiştirin `[myregistry]` kayıt defterinizin adıyla:

    ```yml
    version: v1.0.0
    steps:
        - cmd: [myregistry].azurecr.io/bash:3.0 echo hello world
    ```

* Bir çalışma değişkeniyle kayıt defteri başvuru generalize

    Kayıt defteri adınız sabit kodlama yerine bir `acr-task.yaml` dosyası yapabilirsiniz, daha taşınabilir kullanarak bir [değişkeni çalıştırma](#run-variables). `Run.Registry` Görev yürütüyordur kayıt defteri adı için çalışma zamanında değişken genişletir.

    Herhangi bir Azure container Registry'de çalışır böylece önceki görev genelleştirmek için başvuru [Run.Registry](#runregistry) değişken görüntü adı:

    ```yml
    version: v1.0.0
    steps:
      - cmd: {{.Run.Registry}}/bash:3.0 echo hello world
    ```

## <a name="task-step-properties"></a>Görev Adım özellikleri

Her adım türü kendi türü için uygun çeşitli özelliklerini destekler. Aşağıdaki tabloda tüm kullanılabilir adım özelliklerini tanımlar. Adım türü tüm özellikleri desteklemez. Her bir adım türü için bu özelliklerin kullanılabildiğini görmek için bkz: [cmd](#cmd), [derleme](#build), ve [anında iletme](#push) adım türü başvurusu bölümler.

| Özellik | Tür | İsteğe bağlı | Açıklama | Varsayılan değer |
| -------- | ---- | -------- | ----------- | ------- |
| `detach` | bool | Evet | Olup kapsayıcı çalıştırırken ayrılmış. | `false` |
| `disableWorkingDirectoryOverride` | bool | Evet | Devre dışı bırakılıp bırakılmayacağını `workingDirectory` işlevleri geçersiz kılar. Bu ile birlikte kullanma `workingDirectory` kapsayıcının çalışma dizini üzerinde tam denetime sahip. | `false` |
| `entryPoint` | string | Evet | Geçersiz kılmalar `[ENTRYPOINT]` bir adım kapsayıcısı. | None |
| `env` | [dize, dize,...] | Evet | Dizeler dizisi `key=value` adımı için ortam değişkenlerini biçimi. | None |
| `expose` | [dize, dize,...] | Evet | Kapsayıcıyı kullanıma sunulan bağlantı noktaları dizisi. |  None |
| [`id`](#example-id) | string | Evet | Adım görev içinde benzersiz olarak tanımlar. Diğer adımları görev bir adım başvurabilirsiniz `id`, bağımlılık ile denetimi gibi `when`.<br /><br />`id` Çalıştırılan kapsayıcının adıdır. Görev diğer kapsayıcılardaki çalışan işlemler başvurabilir `id` DNS ana bilgisayar adını veya docker günlükleri [ID] ile örneğin erişmek için. | `acb_step_%d`, burada `%d` adımın yukarıdan aşağıya YAML dosyası 0 tabanlı dizin. |
| `ignoreErrors` | bool | Evet | Bir adım olarak başarılı olup olmadığını kapsayıcı yürütülürken bir hata oluştu bağımsız olarak işaretlemek belirtir. | `false` |
| `isolation` | string | Evet | Kapsayıcı yalıtım düzeyi. | `default` |
| `keep` | bool | Evet | Adım kapsayıcı yürütme sonrasında korunması gerekir. | `false` |
| `network` | object | Evet | Kapsayıcı çalıştığı bir ağı tanımlar. | None |
| `ports` | [dize, dize,...] | Evet | Kapsayıcıdan konağa yayımlanan bağlantı noktaları dizisi. |  None |
| `pull` | bool | Evet | Tüm önbelleğe alma davranışı önlemek için çalıştırmadan önce bir çekme kapsayıcının zorlamak verilmeyeceğini belirtir. | `false` |
| `privileged` | bool | Evet | Kapsayıcı'ı ayrıcalıklı modda çalıştırılıp çalıştırılmayacağını belirtir. | `false` |
| `repeat` | int | Evet | Bir kapsayıcının yürütme yinelemek için yeniden deneme sayısı. | 0 |
| `retries` | int | Evet | Bir kapsayıcı yürütme başarısız olursa denemek için yeniden deneme sayısı. Bir kapsayıcının çıkış kodu sıfır değilse bir yeniden deneme yalnızca denenir. | 0 |
| `retryDelay` | int (saniye) | Evet | Bir kapsayıcının yürütme yeniden denemeler arasındaki saniye cinsinden gecikme. | 0 |
| `secret` | object | Evet | Bir Azure Key Vault gizli veya Azure kaynakları için yönetilen kimlik tanımlar. | None |
| `startDelay` | int (saniye) | Evet | Bir kapsayıcının yürütme geciktirme saniye sayısı. | 0 |
| `timeout` | int (saniye) | Evet | En fazla saniye sayısı, sonlandırılmadan önce bir adımı yürütür. | 600 |
| [`when`](#example-when) | [dize, dize,...] | Evet | Görev içindeki bir veya daha fazla diğer adımlar bir adım bağımlılık yapılandırır. | None |
| `user` | string | Evet | Kullanıcı adı veya bir kapsayıcı UID'si | None |
| `workingDirectory` | string | Evet | Bir adım için çalışma dizinini ayarlar. Varsayılan olarak, ACR görevleri bir kök dizin çalışma dizininde olarak oluşturur. Derleme birkaç adım vardır, ancak, önceki adımlarda yapıtları sonraki adımlara aynı çalışma dizini belirterek paylaşabilir. | `$HOME` |

### <a name="examples-task-step-properties"></a>Örnekler: Görev Adım özellikleri

#### <a name="example-id"></a>Örnek: kimliği

İşlevsel test görüntüsü depolamasına iki görüntü oluşturun. Her adımın bir benzersiz tarafından tanımlanır `id` diğer adımları görev içinde başvuran kendi `when` özelliği.

```azurecli
az acr run -f when-parallel-dependent.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/when-parallel-dependent.yaml -->
[!code-yml[task](~/acr-tasks/when-parallel-dependent.yaml)]

#### <a name="example-when"></a>Örnek: zaman

`when` Özelliği, görev içinde diğer adımlar bir adım bağımlılık belirtir. Bu, iki parametre değerlerini destekler:

* `when: ["-"]` -Diğer adımlar üzerinde hiçbir bağımlılık gösterir. Belirten bir adım `when: ["-"]` yürütme hemen başlar ve eş zamanlı adım yürütme sağlar.
* `when: ["id1", "id2"]` -Adım adımlara bağlı bağımlı olduğunu gösterir `id` "ıd1" ve `id` "ıd2". Hem "ıd1" ve "ıd2" adımlar tamamlanana kadar bu adımı yürütülen olmaz.

Varsa `when` belirtilmediyse bir adım, bu adım önceki adımda tamamlanmasına bağımlı olduğu `acr-task.yaml` dosya.

Sıralı adım yürütülmeden `when`:

```azurecli
az acr run -f when-sequential-default.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/when-sequential-default.yaml -->
[!code-yml[task](~/acr-tasks/when-sequential-default.yaml)]

Adım sıralı yürütme ile `when`:

```azurecli
az acr run -f when-sequential-id.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/when-sequential-id.yaml -->
[!code-yml[task](~/acr-tasks/when-sequential-id.yaml)]

Paralel görüntüleri oluşturun:

```azurecli
az acr run -f when-parallel.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/when-parallel.yaml -->
[!code-yml[task](~/acr-tasks/when-parallel.yaml)]

Paralel görüntüsü derleme ve test bağımlı:

```azurecli
az acr run -f when-parallel-dependent.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/when-parallel-dependent.yaml -->
[!code-yml[task](~/acr-tasks/when-parallel-dependent.yaml)]

## <a name="run-variables"></a>Değişkenleri çalıştırın

ACR görevleri bunlar yürüttüğünüzde, görev adımları için kullanılabilen değişkenleri varsayılan kümesini içerir. Bu değişkenler biçimini kullanarak erişilebilir `{{.Run.VariableName}}`burada `VariableName` aşağıdakilerden biridir:

* `Run.ID`
* `Run.Registry`
* `Run.Date`
* `Run.Commit`
* `Run.Branch`

### <a name="runid"></a>Run.ID

Her aracılığıyla Çalıştır, `az acr run`, veya aracılığıyla oluşturulan görevler tabanlı olarak yürütülmesini tetiklemek `az acr task create` benzersiz bir kimliğe sahip Kimlik, şu anda yürütülmekte olan çalıştırma temsil eder.

Genellikle bir benzersiz olarak görüntüyü etiketlemek için kullanılır:

```yml
version: v1.0.0
steps:
    - build: -t {{.Run.Registry}}/hello-world:{{.Run.ID}} .
```

### <a name="runregistry"></a>Run.Registry

Kayıt defteri tam sunucu adı. Genellikle, görevin çalıştırıldığı kayıt defteri genel olarak başvurmak için kullanılır.

```yml
version: v1.0.0
steps:
  - build: -t {{.Run.Registry}}/hello-world:{{.Run.ID}} .
```

### <a name="rundate"></a>Run.Date

Geçerli UTC saatine çalıştırma başladı.

### <a name="runcommit"></a>Run.Commit

Bir GitHub deposuna işleme tanımlayıcısı için bir işleme tarafından tetiklenen bir görev.

### <a name="runbranch"></a>Run.Branch

Bir GitHub deposu, dal adı için bir işleme tarafından tetiklenen bir görev.

## <a name="next-steps"></a>Sonraki adımlar

Çok adımlı görevler genel bakış için bkz. [ACR görevleri çok adımlı derleme, test ve düzeltme eki görevleri çalıştırmak](container-registry-tasks-multi-step.md).

Tek adımlı yapıları için bkz. [ACR görevlere genel bakış](container-registry-tasks-overview.md).

<!-- IMAGES -->

<!-- LINKS - External -->
[acr-tasks]: https://github.com/Azure-Samples/acr-tasks

<!-- LINKS - Internal -->
[az-acr-run]: /cli/azure/acr#az-acr-run
[az-acr-task-create]: /cli/azure/acr/task#az-acr-task-create
[az-configure]: /cli/azure/reference-index#az-configure
