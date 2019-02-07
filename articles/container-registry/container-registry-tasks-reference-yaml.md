---
title: Azure kapsayıcı kayıt defteri görevleri başvurusu - YAML
description: ACR görev özellikleri, adım türleri, adım özellikleri ve yerleşik değişkenler dahil olmak üzere görev için YAML içinde görevleri tanımlama için başvuru.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 11/13/2018
ms.author: danlep
ms.openlocfilehash: c9b4a27ff1b5467eb752e8cfc09f697ca1a966ba
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55820394"
---
# <a name="acr-tasks-reference-yaml"></a>ACR görevleri başvurusu: YAML

Çok adımlı görev tanımı ACR görevleri oluşturmaya, sınamaya ve kapsayıcıları düzeltme eki uygulama üzerinde odaklanmış bir kapsayıcı merkezli işlem temel sağlar. Bu makale, komutlar, Parametreler, özellikleri ve, çok adımlı görevler tanımlayın YAML dosyalar için söz dizimi kapsar.

Bu makale, çok adımlı görev ACR görev için YAML dosyaları oluşturmak için başvuru içerir. ACR görevleri giriş isterseniz bkz [ACR görevlere genel bakış](container-registry-tasks-overview.md).

> [!IMPORTANT]
> ACR görevleri çok adımlı görevler özelliği şu anda Önizleme aşamasındadır. Önizlemeler, [ek kullanım koşullarını][terms-of-use] kabul etmeniz şartıyla kullanımınıza sunulur. Bu özelliğin bazı yönleri genel kullanıma açılmadan önce değişebilir.

## <a name="acr-taskyaml-file-format"></a>ACR task.yaml dosya biçimi

ACR görevleri çok adımlı görev bildirimi standart YAML sözdizimini destekler. Daha sonra el ile çalıştırabilirsiniz ya da Git commit veya temel görüntü güncelleştirme otomatik olarak tetiklenen bir YAML dosyası içinde bir görevin adımları tanımlayın. Bu makalede başvuruyor ancak `acr-task.yaml` adımları içeren dosya geçerli bir dosya ile ACR görevleri destekleyen bir [uzantı desteklenen](#supported-task-filename-extensions).

Üst düzey `acr-task.yaml` ilkelleri olan **görev özellikleri**, **adım türleri**, ve **adım özellikleri**:

* [Görev Özellikleri](#task-properties) tüm adımları görev yürütme boyunca geçerlidir. Üç genel görev özellikleri vardır:
  * version
  * stepTimeout
  * totalTimeout
* [Görev Adım türleri](#task-step-types) görevde gerçekleştirilebilecek eylemler türlerini temsil eder. Üç adım vardır:
  * Derleme
  * anında iletme
  * cmd
* [Adım özellikleri görev](#task-step-properties) tek bir adımı için geçerli parametreler. Birkaç adım özellikleri vardır:
  * startDelay
  * timeout
  * zaman
  * .. .ve çok daha fazlası.

Temel biçimi bir `acr-task.yaml` bazı ortak adım özellikleri dahil olmak üzere, dosya izler. Bir ayrıntılı gösterimini değil tüm kullanılabilir adım özellikleri veya adım türü kullanımı sırasında temel dosya biçimi hızlı bir genel bakış sağlar.

```yaml
version: # acr-task.yaml format version.
stepTimeout: # Seconds each step may take.
totalTimeout: # Total seconds allowed for all steps to complete.
steps: # A collection of image or container actions.
    build: # Equivalent to "docker build," but in a multi-tenant environment
    push: # Push a newly built or retagged image to a registry.
      when: # Step property that defines either parallel or dependent step execution.
    cmd: # Executes a container, supports specifying an [ENTRYPOINT] and parameters.
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

Görev özellikleri genellikle üst kısmında görünür bir `acr-task.yaml` dosyasını bulun ve tam görevin yürütülmesini geçerli olan genel özellikler. Bu genel özelliklerin bazıları içinde tek bir adımı kılınabilir.

| Özellik | Type | İsteğe bağlı | Açıklama | Desteklenen bir geçersiz kılma | Varsayılan değer |
| -------- | ---- | -------- | ----------- | ------------------ | ------------- |
| `version` | dize | Hayır | Sürümü `acr-task.yaml` ACR görevleri hizmeti tarafından ayrıştırılan gibi dosya. Geriye dönük uyumluluğu korumak ACR görevleri içindedir, ancak bu değer, ACR içinde tanımlı bir sürüm uyumluluğu korumak için görevleri sağlar. | Hayır | None |
| `stepTimeout` | int (saniye) | Evet | Bir adım çalıştırabilirsiniz saniye sayısı. Bu özellik bir adımda adımının zaman aşımı özelliği ayarlanarak geçersiz kılınabilir. | Evet | 600 (10 dakika) |
| `totalTimeout` | int (saniye) | Evet | Bir görev çalışabilir saniye sayısı. "run" yürütme ve tüm adımları başarılı veya başarısız olmadığını görevde içerir. Ayrıca dahil yazdırma görev algılanan görüntü bağımlılıkları ve görev yürütme durumu gibi çıkış. | Hayır | 3600 (1 saat) |

## <a name="task-step-types"></a>Görev Adım türü

ACR görevleri üç adım türlerini destekler. Her adım türü bölümünde her bir adım türü için ayrıntılı çeşitli özelliklerini destekler.

| Adım türü | Açıklama |
| --------- | ----------- |
| [`build`](#build) | Tanıdık kullanarak bir kapsayıcı görüntüsü derler `docker build` söz dizimi. |
| [`push`](#push) | Yürüten bir `docker push` bir kapsayıcı kayıt defterine yeni oluşturulmuş veya retagged görüntüleri. Azure Container Registry, diğer özel kayıt defterlerini ve genel Docker Hub desteklenir.
| [`cmd`](#cmd) | Kapsayıcı parametrelere sahip bir komut olarak geçirilen kapsayıcının çalışır `[ENTRYPOINT]`. `cmd` Ayırma, adım türünü env gibi parametreleri destekler ve diğer tanıdık `docker run` komut, birim ve işlevsel test kapsayıcı eş zamanlı yürütme ile etkinleştirme seçenekleri. |

## <a name="build"></a>Derleme

Bir kapsayıcı görüntüsü oluşturun. `build` Adım türünü temsil eder, çalışan çok kiracılı, güvenli bir çeşit `docker build` birinci sınıf bir temel olarak bulutta.

### <a name="syntax-build"></a>Sözdizimi: derleme

```yaml
version: 1.0-preview-1
steps:
    - [build]: -t [imageName]:[tag] -f [Dockerfile] [context]
      [property]: [value]
```

`build` Adım türü parametreleri aşağıdaki tabloda destekler. `build` Adım türü de destekler, tüm derleme seçenekleri [docker derleme](https://docs.docker.com/engine/reference/commandline/build/) komutu gibi `--build-arg` derleme zamanı değişkenlerini ayarlamak için.

| Parametre | Açıklama | İsteğe bağlı |
| --------- | ----------- | :-------: |
| `-t` &#124; `--image` | Tam tanımlar `image:tag` yerleşik resmin.<br /><br />Tüm görüntüleri görüntüleri gibi işlevsel testler iç görev doğrulama için kullanılabilir olarak gerektiren `push` bir kayıt defterine. Ancak, bir görüntü içindeki görev yürütme örneği, görüntünün başvurmak için bir ad gerek yoktur.<br /><br />Aksine `az acr build`, ACR görevleri çalıştıran gönderme davranışı varsayılan sağlamaz. ACR görevlerle varsayılan senaryoyu oluşturmak, doğrulamak ve görüntü gönderebilmeniz olanağı varsayar. Bkz: [anında iletme](#push) için isteğe bağlı olarak anında iletme yerleşik görüntüleri hakkında. | Evet |
| `-f` &#124; `--file` | Geçirilen Dockerfile belirtir `docker build`. Belirtilmezse, varsayılan bağlamı kökünde Dockerfile varsayılır. Alternatif Dockerfile belirtmek için bağlam köküne dosya adı geçirin. | Evet |
| `context` | Kök dizin geçirilen `docker build`. Her görev kök dizini ayarlamak için bir paylaşılan [workingDirectory](#task-step-properties)ve ilişkili Git kopyalanan dizininin kökü içerir. | Hayır |

### <a name="properties-build"></a>Özellikler: derleme

`build` Adım türü aşağıdaki özellikleri destekler. Bu özellikleri ayrıntılarını bulabilirsiniz [görev adım özellikleri](#task-step-properties) bu makalenin.

| | | |
| -------- | ---- | -------- |
| `detach` | bool | İsteğe bağlı |
| `entryPoint` | dize | İsteğe bağlı |
| `env` | [dize, dize,...] | İsteğe bağlı |
| `id` | dize | İsteğe bağlı |
| `ignoreErrors` | bool | İsteğe bağlı |
| `keep` | bool | İsteğe bağlı |
| `startDelay` | int (saniye) | İsteğe bağlı |
| `timeout` | int (saniye) | İsteğe bağlı |
| `when` | [dize, dize,...] | İsteğe bağlı |
| `workingDirectory` | dize | İsteğe bağlı |

### <a name="examples-build"></a>Örnekler: oluşturma

#### <a name="build-image---context-in-root"></a>Görüntü - kök bağlamda oluşturun

```azurecli
az acr run -f build-hello-world.yaml https://github.com/AzureCR/acr-tasks-sample.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/build-hello-world.yaml --> [!code-yml[task](~/acr-tasks/build-hello-world.yaml)]

#### <a name="build-image---context-in-subdirectory"></a>Görüntü - alt bağlamda oluşturun

```yaml
version: 1.0-preview-1
steps:
- build: -t {{.Run.Registry}}/hello-world -f hello-world.dockerfile ./subDirectory
```

## <a name="push"></a>anında iletme

Bir anında iletme veya daha fazla yerleşik veya bir kapsayıcı kayıt defterine görüntü retagged. Azure Container Registry gibi özel kayıt defterleri için veya genel Docker hub'ına gönderme destekler.

### <a name="syntax-push"></a>Sözdizimi: anında iletme

`push` Adım türü görüntü koleksiyonunu destekler. Satır içi hem de iç içe geçmiş biçimleri YAML koleksiyon sözdizimini destekler. Tek bir görüntü gönderme genelde satır içi söz dizimi kullanılarak temsil edilir:

```yaml
version: 1.0-preview-1
steps:
  # Inline YAML collection syntax
  - push: ["{{.Run.Registry}}/hello-world:{{.Run.ID}}"]
```

Birden fazla görüntü gönderirken, okunurluğu artırmak için iç içe geçmiş sözdizimini kullanın:

```yaml
version: 1.0-preview-1
steps:
  # Nested YAML collection syntax
  - push:
    - {{.Run.Registry}}/hello-world:{{.Run.ID}}
    - {{.Run.Registry}}/hello-world:latest
```

### <a name="properties-push"></a>Özellikler: gönderme

`push` Adım türü aşağıdaki özellikleri destekler. Bu özellikleri ayrıntılarını bulabilirsiniz [görev adım özellikleri](#task-step-properties) bu makalenin.

| | | |
| -------- | ---- | -------- |
| `env` | [dize, dize,...] | İsteğe bağlı |
| `id` | dize | İsteğe bağlı |
| `ignoreErrors` | bool | İsteğe bağlı |
| `startDelay` | int (saniye) | İsteğe bağlı |
| `timeout` | int (saniye) | İsteğe bağlı |
| `when` | [dize, dize,...] | İsteğe bağlı |

### <a name="examples-push"></a>Örnekler: anında iletme

#### <a name="push-multiple-images"></a>Birden çok görüntüleri gönderme

```azurecli
az acr run -f build-push-hello-world.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/build-push-hello-world.yaml --> [!code-yml[task](~/acr-tasks/build-push-hello-world.yaml)]

#### <a name="build-push-and-run"></a>Oluşturma, gönderme ve çalıştırma

```azurecli
az acr run -f build-run-hello-world.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/build-run-hello-world.yaml --> [!code-yml[task](~/acr-tasks/build-run-hello-world.yaml)]

## <a name="cmd"></a>cmd

`cmd` Adım türü bir kapsayıcı olarak çalıştırılır.

### <a name="syntax-cmd"></a>Sözdizimi: cmd

```yaml
version: 1.0-preview-1
steps:
    - [cmd]: [containerImage]:[tag (optional)] [cmdParameters to the image]
```

### <a name="properties-cmd"></a>Özellikler: cmd

`cmd` Adım türü aşağıdaki özellikleri destekler:

| | | |
| -------- | ---- | -------- |
| `detach` | bool | İsteğe bağlı |
| `entryPoint` | dize | İsteğe bağlı |
| `env` | [dize, dize,...] | İsteğe bağlı |
| `id` | dize | İsteğe bağlı |
| `ignoreErrors` | bool | İsteğe bağlı |
| `keep` | bool | İsteğe bağlı |
| `startDelay` | int (saniye) | İsteğe bağlı |
| `timeout` | int (saniye) | İsteğe bağlı |
| `when` | [dize, dize,...] | İsteğe bağlı |
| `workingDirectory` | dize | İsteğe bağlı |

Bu özellikleri ayrıntılarını bulabilirsiniz [görev adım özellikleri](#task-step-properties) bu makalenin.

### <a name="examples-cmd"></a>Örnekler: cmd

#### <a name="run-hello-world-image"></a>Merhaba-Dünya görüntüsünü çalıştırma

Bu komutu yürütür `hello-world.yaml` başvuran görev dosyası [Merhaba-Dünya](https://hub.docker.com/_/hello-world/) Docker hub'daki resmi.

```azurecli
az acr run -f hello-world.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/hello-world.yaml --> [!code-yml[task](~/acr-tasks/hello-world.yaml)]

#### <a name="run-bash-image-and-echo-hello-world"></a>Bash görüntüsü ve yankı "hello world" çalıştırın

Bu komutu yürütür `bash-echo.yaml` başvuran görev dosyası [bash](https://hub.docker.com/_/bash/) Docker hub'daki resmi.

```azurecli
az acr run -f bash-echo.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/bash-echo.yaml --> [!code-yml[task](~/acr-tasks/bash-echo.yaml)]

#### <a name="run-specific-bash-image-tag"></a>Belirli bir bash resim etiketi çalıştırın

Bir özel görüntü sürümünü çalıştırmak üzere etiketinde belirtin `cmd`.

Bu komutu yürütür `bash-echo-3.yaml` başvuran görev dosyası [bash: 3.0](https://hub.docker.com/_/bash/) Docker hub'daki resmi.

```azurecli
az acr run -f bash-echo-3.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/bash-echo-3.yaml --> [!code-yml[task](~/acr-tasks/bash-echo-3.yaml)]

#### <a name="run-custom-images"></a>Özel görüntüleri çalıştırın

`cmd` Adım türe başvuran standart kullanarak görüntüleri `docker run` biçimi. Kayıt defteri ile başında değil görüntüleri docker.io kaynaklanan varsayılır. Önceki örnekte, eşit olarak gösterilebilir:

```yaml
version: 1.0-preview-1
steps:
    - cmd: docker.io/bash:3.0 echo hello world
```

Standart kullanarak `docker run` görüntü başvurusu kuralı `cmd` herhangi bir özel kayıt veya genel Docker Hub içinde bulunan görüntüleri çalıştırabilirsiniz. ACR görevin yürütüldüğü aynı kayıt defterindeki görüntüleri başvuran herhangi bir kayıt defteri kimlik bilgilerini belirtmeniz gerekmez.

* Bir Azure container Registry'de bulunan bir görüntü çalıştırma

    Değiştirin `[myregistry]` kayıt defterinizin adıyla:

    ```yaml
    version: 1.0-preview-1
    steps:
        - cmd: [myregistry].azurecr.io/bash:3.0 echo hello world
    ```

* Bir çalışma değişkeniyle kayıt defteri başvuru generalize

    Kayıt defteri adınız sabit kodlama yerine bir `acr-task.yaml` dosyası yapabilirsiniz, daha taşınabilir kullanarak bir [değişkeni çalıştırma](#run-variables). `Run.Registry` Görev yürütüyordur kayıt defteri adı için çalışma zamanında değişken genişletir.

    Herhangi bir Azure container Registry'de çalışır böylece önceki görev genelleştirmek için başvuru [Run.Registry](#runregistry) değişken görüntü adı:

    ```yaml
    version: 1.0-preview-1
    steps:
        - cmd: {{.Run.Registry}}/bash:3.0 echo hello world
    ```

## <a name="task-step-properties"></a>Görev Adım özellikleri

Her adım türü kendi türü için uygun çeşitli özelliklerini destekler. Aşağıdaki tabloda tüm kullanılabilir adım özelliklerini tanımlar. Adım türü tüm özellikleri desteklemez. Her bir adım türü için bu özelliklerin kullanılabildiğini görmek için bkz: [cmd](#cmd), [derleme](#build), ve [anında iletme](#push) adım türü başvurusu bölümler.

| Özellik | Type | İsteğe bağlı | Açıklama |
| -------- | ---- | -------- | ----------- |
| `detach` | bool | Evet | Olup kapsayıcı çalıştırırken ayrılmış. |
| `entryPoint` | dize | Evet | Geçersiz kılmalar `[ENTRYPOINT]` bir adım kapsayıcısı. |
| `env` | [dize, dize,...] | Evet | Dizeler dizisi `key=value` adımı için ortam değişkenlerini biçimi. |
| [`id`](#example-id) | dize | Evet | Adım görev içinde benzersiz olarak tanımlar. Diğer adımları görev bir adım başvurabilirsiniz `id`, bağımlılık ile denetimi gibi `when`.<br /><br />`id` Çalıştırılan kapsayıcının adıdır. Görev diğer kapsayıcılardaki çalışan işlemler başvurabilir `id` DNS ana bilgisayar adını veya docker günlükleri [ID] ile örneğin erişmek için. |
| `ignoreErrors` | bool | Evet | Ayarlandığında `true`, adımı tamamlandı olarak mı yürütme sırasında bir hata oluştu bağımsız olarak işaretlenmiş. Varsayılan: `false`. |
| `keep` | bool | Evet | Adım kapsayıcı yürütme sonrasında korunması gerekir. |
| `startDelay` | int (saniye) | Evet | Bir adım yürütme geciktirme saniye sayısı. |
| `timeout` | int (saniye) | Evet | En fazla saniye sayısı, sonlandırılmadan önce bir adımı yürütür. |
| [`when`](#example-when) | [dize, dize,...] | Evet | Görev içindeki bir veya daha fazla diğer adımlar bir adım bağımlılık yapılandırır. |
| `workingDirectory` | dize | Evet | Bir adım için çalışma dizinini ayarlar. Varsayılan olarak, ACR görevleri bir kök dizin çalışma dizininde olarak oluşturur. Derleme birkaç adım vardır, ancak, önceki adımlarda yapıtları sonraki adımlara aynı çalışma dizini belirterek paylaşabilir. |

### <a name="examples-task-step-properties"></a>Örnekler: Görev Adım özellikleri

#### <a name="example-id"></a>Örnek: kimliği

İşlevsel test görüntüsü depolamasına iki görüntü oluşturun. Her adımın bir benzersiz tarafından tanımlanır `id` diğer adımları görev içinde başvuran kendi `when` özelliği.

```azurecli
az acr run -f when-parallel-dependent.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/when-parallel-dependent.yaml --> [!code-yml[task](~/acr-tasks/when-parallel-dependent.yaml)]

#### <a name="example-when"></a>Örnek: zaman

`when` Özelliği, görev içinde diğer adımlar bir adım bağımlılık belirtir. Bu, iki parametre değerlerini destekler:

* `when: ["-"]` -Diğer adımlar üzerinde hiçbir bağımlılık gösterir. Belirten bir adım `when: ["-"]` yürütme hemen başlar ve eş zamanlı adım yürütme sağlar.
* `when: ["id1", "id2"]` -Adım adımlara bağlı bağımlı olduğunu gösterir `id` "ıd1" ve `id` "ıd2". Hem "ıd1" ve "ıd2" adımlar tamamlanana kadar bu adımı yürütülen olmaz.

Varsa `when` belirtilmediyse bir adım, bu adım önceki adımda tamamlanmasına bağımlı olduğu `acr-task.yaml` dosya.

Sıralı adım yürütülmeden `when`:

```azurecli
az acr run -f when-sequential-default.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/when-sequential-default.yaml --> [!code-yml[task](~/acr-tasks/when-sequential-default.yaml)]

Adım sıralı yürütme ile `when`:

```azurecli
az acr run -f when-sequential-id.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/when-sequential-id.yaml --> [!code-yml[task](~/acr-tasks/when-sequential-id.yaml)]

Paralel görüntüsü derleme:

```azurecli
az acr run -f when-parallel.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/when-parallel.yaml --> [!code-yml[task](~/acr-tasks/when-parallel.yaml)]

Paralel görüntüsü derleme ve test bağımlı:

```azurecli
az acr run -f when-parallel-dependent.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/when-parallel-dependent.yaml --> [!code-yml[task](~/acr-tasks/when-parallel-dependent.yaml)]

## <a name="run-variables"></a>Değişkenleri çalıştırın

ACR görevleri bunlar yürüttüğünüzde, görev adımları için kullanılabilen değişkenleri varsayılan kümesini içerir. Bu değişkenler biçimini kullanarak erişilebilir `{{.Run.VariableName}}`burada `VariableName` aşağıdakilerden biridir:

* `Run.ID`
* `Run.Registry`
* `Run.Date`

### <a name="run46id"></a>Çalıştırma&#46;kimliği

Her aracılığıyla Çalıştır, `az acr run`, veya aracılığıyla oluşturulan görevler tabanlı olarak yürütülmesini tetiklemek `az acr task create` benzersiz bir kimliğe sahip Kimlik, şu anda yürütülmekte olan çalıştırma temsil eder.

Genellikle bir benzersiz olarak görüntüyü etiketlemek için kullanılır:

```yaml
version: 1.0-preview-1
steps:
    - build: -t {{.Run.Registry}}/hello-world:{{.Run.ID}} .
```

### <a name="runregistry"></a>Run.Registry

Kayıt defteri tam sunucu adı. Genellikle, görevin çalıştırıldığı kayıt defteri genel olarak başvurmak için kullanılır.

```yaml
version: 1.0-preview-1
steps:
    - build: -t {{.Run.Registry}}/hello-world:{{.Run.ID}} .
```

### <a name="rundate"></a>Run.Date

Geçerli UTC saatine çalıştırma başladı.

## <a name="next-steps"></a>Sonraki adımlar

Çok adımlı görevler genel bakış için bkz. [ACR görevleri çok adımlı derleme, test ve düzeltme eki görevleri çalıştırmak](container-registry-tasks-multi-step.md).

Tek adımlı yapıları için bkz. [ACR görevlere genel bakış](container-registry-tasks-overview.md).

<!-- IMAGES -->

<!-- LINKS - External -->
[acr-tasks]: https://github.com/Azure-Samples/acr-tasks
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - Internal -->
[az-acr-run]: /cli/azure/acr#az-acr-run
[az-configure]: /cli/azure/reference-index#az-configure
