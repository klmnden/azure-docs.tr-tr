---
title: Azure Container Registry görevleri zamanlama
description: Bir Azure Container Registry görev tanımlanmış bir zamanlamaya göre çalıştırılacak zamanlayıcılar ayarlayın.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 06/27/2019
ms.author: danlep
ms.openlocfilehash: a1123a30025f9be6e994e69703f5ee1aa05d1b49
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67509709"
---
# <a name="run-an-acr-task-on-a-defined-schedule"></a>ACR görev tanımlanmış bir zamanlamaya göre çalıştırın

Bu makalede nasıl çalıştırılacağını gösteren bir [ACR görev](container-registry-tasks-overview.md) bir zamanlamaya göre. Bir veya daha fazla ayarlayarak bir görev zamanlama *Zamanlayıcı Tetikleyicileri*. 

Bir görev zamanlama, aşağıdaki gibi senaryoları için kullanışlıdır:

* Zamanlanan bakım işlemleri için bir kapsayıcı iş yükü çalıştırın. Örneğin, kayıt defterinizden gereksiz görüntülerini kaldırmak için bir kapsayıcı uygulaması çalıştırın.
* Test kümesi, workday parçası olarak, Canlı site izleme sırasında bir üretim görüntüsü üzerinde çalıştırın.

Bu makaledeki örnekleri çalıştırmak için Azure Cloud Shell veya Azure CLI'ın yerel bir yüklemesi'ni kullanabilirsiniz. Yerel olarak 2.0.68 sürümü kullanmak istiyorsanız veya daha sonra gerekli olur. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme][azure-cli-install].


## <a name="about-scheduling-a-task"></a>Bir görev zamanlama hakkında

* **Cron ifadesi tetikleyicisiyle** -bir Görev Zamanlayıcı tetikleyicisi kullanan bir *cron ifadesi*. İfade, dakika, saat, gün, ay ve tetikleyici haftanın günü belirtme beş alanları içeren bir dizedir. Dakika başına bir kez kadar sıklıkları desteklenir. 

  Örneğin, ifade `"0 12 * * Mon-Fri"` öğle UTC Haftanın günlerinin üzerinde saatinde bir görevini tetikler. Bkz: [ayrıntıları](#cron-expressions) bu makalenin ilerleyen bölümlerinde.
* **Birden çok Zamanlayıcı tetikleyici** -birden çok kez bir göreve eklemeye izin verilir, zamanlamaları farklı olduğu sürece. 
    * Görev oluşturma veya bunları daha sonra ekledikten sonra birden çok Zamanlayıcı tetikleyici belirtin.
    * İsteğe bağlı olarak daha kolay yönetim için Tetikleyiciler adı veya varsayılan tetikleyici adlarının ACR görevleri sağlar.
    * Zamanlayıcı zamanlamaları teker teker çakışırsa, ACR görevleri için her bir Zamanlayıcı görevi zamanlanan saatte tetikler. 
* **Diğer görev Tetikleyicileri** -Zamanlayıcı ile tetiklenen bir görevde temel alan Tetikleyicileri da etkinleştirebilirsiniz [kaynak kod tamamlama](container-registry-tutorial-build-task.md) veya [temel görüntü güncelleştirme](container-registry-tutorial-base-image-update.md). Diğer ACR görevler gibi ayrıca [el ile tetikleme][az-acr-task-run] zamanlanmış bir görev.

## <a name="create-a-task-with-a-timer-trigger"></a>Bir zamanlayıcı tetikleyicisi bir görev oluşturun

Bir görev oluşturduğunuzda, [az acr görev oluşturma][az-acr-task-create] komutu, bir zamanlayıcı tetikleyicisi isteğe bağlı olarak ekleyebilirsiniz. Ekleme `--schedule` parametresi ve pass Zamanlayıcı bir cron ifadesi. 

Basit bir örnek olarak, aşağıdaki komut çalıştıran bir tetikleyici `hello-world` Docker Hub görüntüsünden her gün saat 21:00 UTC. Görev, bir kaynak kod bağlamı çalışır.

```azurecli
az acr task create \
  --name mytask \
  --registry myregistry \
  --context /dev/null \
  --cmd hello-world \
  --schedule "0 21 * * *"
```

Çalıştırma [az acr görev show][az-acr-task-show] Zamanlayıcı tetikleyicisi yapılandırıldığını görmek için komutu. Varsayılan olarak, temel görüntü güncelleştirme tetikleyici de etkinleştirilir.

```console
$ az acr task show --name mytask --registry registry --output table
NAME      PLATFORM    STATUS    SOURCE REPOSITORY       TRIGGERS
--------  ----------  --------  -------------------     -----------------
mytask    linux       Enabled                           BASE_IMAGE, TIMER
```

El ile tetikleyici [az acr görevi Çalıştır][az-acr-task-run] düzgün şekilde ayarlandığını emin olmak için:

```azurecli
az acr task run --name mytask --registry myregistry
```

Kapsayıcı başarıyla çalışırsa, çıktı aşağıdakine benzer olacaktır:

```console
Queued a run with ID: cf2a
Waiting for an agent...
2019/06/28 21:03:36 Using acb_vol_2ca23c46-a9ac-4224-b0c6-9fde44eb42d2 as the home volume
2019/06/28 21:03:36 Creating Docker network: acb_default_network, driver: 'bridge'
[...]
2019/06/28 21:03:38 Launching container with name: acb_step_0

Hello from Docker!
This message shows that your installation appears to be working correctly.
[...]
```

Zamanlanmış saat sonrasında çalıştırma [az acr görev listesi-çalıştığında][az-acr-task-list-runs] Zamanlayıcı beklendiği gibi görev tetiklenen doğrulamak için komut:

```azurecli
az acr task list runs --name mytask --registry myregistry --output table
``` 

Zamanlayıcı başarılı olduğunda, çıktı şuna benzer:

```console
RUN ID    TASK     PLATFORM    STATUS     TRIGGER    STARTED               DURATION
--------  -------- ----------  ---------  ---------  --------------------  ----------
[...]
cf2b      mytask   linux       Succeeded  Timer      2019-06-28T21:00:23Z  00:00:06
cf2a      mytask   linux       Succeeded  Manual     2019-06-28T20:53:23Z  00:00:06
```
            
## <a name="manage-timer-triggers"></a>Zamanlayıcı Tetikleyicileri yönetme

Kullanım [az acr Görev Zamanlayıcı][az-acr-task-timer] ACR Görev Zamanlayıcı tetikleyici yönetecek komutlar.

### <a name="add-or-update-a-timer-trigger"></a>Ekleme veya güncelleştirme bir zamanlayıcı tetikleyicisi

Bir görev oluşturduktan sonra isteğe bağlı olarak bir zamanlayıcı tetikleyicisi kullanarak eklemek [az acr Görev Zamanlayıcı ekleme][az-acr-task-timer-add] komutu. Aşağıdaki örnek, bir zamanlayıcı tetikleyicisi adı ekler *Süreölçer2* için *mytask* önceden oluşturulmuş. Bu Zamanlayıcı görevi her gün saat 10:30 tetikler. UTC.

```azurecli
az acr task timer add \
  --name mytask \
  --registry myregistry \
  --timer-name timer2 \
  --schedule "30 10 * * *"
```

Varolan bir tetikleyiciyi zamanlamasını güncelleştirme veya kullanarak, durumunu değiştirme [az acr Görev Zamanlayıcı güncelleştirme][az-acr-task-timer-update] komutu. Örneğin, adlı tetikleyici güncelleştirme *Süreölçer2* görev 11: 30'da tetiklemek için UTC:

```azurecli
az acr task timer update \
  --name mytask \
  --registry myregistry \
  --timer-name timer2 \
  --schedule "30 11 * * *"
```

### <a name="list-timer-triggers"></a>Liste Zamanlayıcı Tetikleyicileri

[Az acr Görev Zamanlayıcı listesi][az-acr-task-timer-list] komut, bir görev için ayarlanan Zamanlayıcı Tetikleyicileri gösterir:

```azurecli
az acr task timer list --name mytask --registry myregistry
```

Örnek çıktı:

```JSON
[
  {
    "name": "timer2",
    "schedule": "30 11 * * *",
    "status": "Enabled"
  },
  {
    "name": "t1",
    "schedule": "0 21 * * *",
    "status": "Enabled"
  }
]
```

### <a name="remove-a-timer-trigger"></a>Bir zamanlayıcı tetikleyicisi Kaldır 

Kullanım [az acr Görev Zamanlayıcı Kaldır][az-acr-task-timer-remove] bir zamanlayıcı tetikleyicisi bir görevden kaldırmak için komutu. Aşağıdaki örnek kaldırır *Süreölçer2* gelen tetikleme *mytask*:

```azurecli
az acr task timer remove \
  --name mytask \
  --registry myregistry \
  --timer-name timer2
```

## <a name="cron-expressions"></a>Sıralanmış iş ifadeleri

ACR görevleri kullanan [NCronTab](https://github.com/atifaziz/NCrontab) sıralanmış iş ifadeleri olarak yorumlamak için kitaplığı. ACR görevleri desteklenen ifadelerinde boşluk tarafından ayrılmış beş gerekli alanlar vardır:

`{minute} {hour} {day} {month} {day-of-week}`

Sıralanmış iş ifadeleri ile kullanılan saat dilimini Eşgüdümlü Evrensel Saat (UTC) ' dir. Saat 24 saat biçimindedir.

> [!NOTE]
> ACR görevleri desteklemiyor `{second}` veya `{year}` sıralanmış iş ifadeleri alanındaki. Başka bir sistemde kullanılan bir cron ifadesi kopyalarsanız, bunların kullanılması durumunda bu alanları kaldırdığınızdan emin olun.

Her bir alan, şu tür değerlerden biri olabilir:

|Tür  |Örnek  |Tetiklendiğinde  |
|---------|---------|---------|
|Belirli bir değer |<nobr>"5 * * * *"</nobr>|her saat başını saati aşan 5 dakika|
|Tüm değerleri (`*`)|<nobr>"* 5 * * *"</nobr>|her dakika, saat başına 5:00 UTC (60 günde kez)|
|Aralık (`-` işleci)|<nobr>"0 1-3 * * *"</nobr>|1:00, günde 3 kez başına 2:00-3:00 UTC|  
|Bir değerler kümesi (`,` işleci)|<nobr>"20,30,40 * * * *"</nobr>|3 kez saatte, 20 dakika, 30 dakika ve 40 geçe|
|Aralık değeri (`/` işleci)|<nobr>"*/10 * * * *"</nobr>|10 dakika, 20 dakika ve saati aşan belirli bir benzeri saat başına 6 zaman

[!INCLUDE [functions-cron-expressions-months-days](../../includes/functions-cron-expressions-months-days.md)]

### <a name="cron-examples"></a>Cron örnekleri

|Örnek|Tetiklendiğinde  |
|---------|---------|
|`"*/5 * * * *"`|beş dakikada bir kez|
|`"0 * * * *"`|sonra her saat başında|
|`"0 */2 * * *"`|Her iki saatte bir kez|
|`"0 9-17 * * *"`|saatte 9:00 17:00 UTC|
|`"30 9 * * *"`|9:30, UTC her gün|
|`"30 9 * * 1-5"`|9:30, hafta içi her gün UTC|
|`"30 9 * Jan Mon"`|9:30, UTC Ocak Ayındaki her Pazartesi|


## <a name="next-steps"></a>Sonraki adımlar

Görev örnekleri güncelleştirmeleri kaynak kod tamamlama veya temel görüntü tarafından tetiklenen göz atın [ACR görevleri öğretici serisinin](container-registry-tutorial-quick-task.md).



<!-- LINKS - External -->
[task-examples]: https://github.com/Azure-Samples/acr-tasks


<!-- LINKS - Internal -->
[az-acr-task-create]: /cli/azure/acr/task#az-acr-task-create
[az-acr-task-show]: /cli/azure/acr/task#az-acr-task-show
[az-acr-task-list-runs]: /cli/azure/acr/task#az-acr-task-list-runs
[az-acr-task-timer]: /cli/azure/acr/task/timer
[az-acr-task-timer-add]: /cli/azure/acr/task/timer#az-acr-task-timer-add
[az-acr-task-timer-remove]: /cli/azure/acr/task/timer#az-acr-task-timer-remove
[az-acr-task-timer-list]: /cli/azure/acr/task/timer#az-acr-task-timer-list
[az-acr-task-timer-update]: /cli/azure/acr/task/timer#az-acr-task-timer-update
[az-acr-task-run]: /cli/azure/acr/task#az-acr-task-run
[az-acr-task]: /cli/azure/acr/task
[azure-cli-install]: /cli/azure/install-azure-cli