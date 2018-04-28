---
title: Azure kapsayıcı durumlarda kapsayıcılı görevleri Çalıştır
description: Azure kapsayıcı örnekleri derleme, test veya görüntü işleme işleri gibi tamamlanıncaya kadar çalışabilmesi görevleri çalıştırmak için nasıl kullanılacağını öğrenin.
services: container-instances
author: mmacy
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 11/16/2017
ms.author: marsma
ms.openlocfilehash: 3bbe3e891423b6ad62a1d1093daef304206f3d76
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="run-a-containerized-task-in-azure-container-instances"></a>Azure kapsayıcı durumlarda kapsayıcılı görevi çalıştırma

Azure kapsayıcı örnekleri kapsayıcılarında dağıtma hızı ve kolaylığı derleme, test ve görüntü oluşturma gibi bir kez çalıştır görevleri bir kapsayıcı örneğinde yürütmek için ilgi çekici bir platform sağlar.

Bir yapılandırılabilir yeniden başlatma ilkesi, süreçlerinin tamamlandığında kapsayıcılarınızı durdurulur belirtebilirsiniz. Kapsayıcı örnekleri ikinciye faturalandırılır olduğundan, yalnızca görev yürütme kapsayıcı çalışırken kullanılan işlem kaynakları için ücret ödersiniz.

Örnekler, bu makalede kullanımda Azure CLI sunulmuştur. Azure CLI Sürüm 2.0.21 olmalıdır veya daha büyük [yerel olarak yüklenmiş][azure-cli-install], veya CLI kullanın [Azure bulut Kabuk](../cloud-shell/overview.md).

## <a name="container-restart-policy"></a>Kapsayıcı yeniden başlatma ilkesi

Azure kapsayıcı durumlarda bir kapsayıcı oluşturduğunuzda, üç yeniden başlatma ilkesi ayarlarından birini belirtebilirsiniz.

| İlke yeniden başlatın   | Açıklama |
| ---------------- | :---------- |
| `Always` | Kapsayıcı grubu kapsayıcılarında her zaman yeniden başlatılır. Bu **varsayılan** hiçbir yeniden başlatma ilkesi kapsayıcı oluşturma sırasında belirtildiğinde uygulanan ayarı. |
| `Never` | Kapsayıcı grubu kapsayıcılarında hiçbir zaman yeniden başlatılır. Kapsayıcılar en fazla bir kez çalıştırın. |
| `OnFailure` | Kapsayıcı grubu kapsayıcılarında (sıfır olmayan çıkış kodu ile sonlandırıldığında) yalnızca kapsayıcısında yürütülen işlemin başarısız olursa yeniden başlatılır. Kapsayıcılar en az bir kez çalıştırılır. |

## <a name="specify-a-restart-policy"></a>Bir yeniden başlatma ilkesi belirtin

Bir yeniden başlatma ilkesi nasıl belirttiğiniz kapsayıcı örneklerinizi gibi Azure portalında veya Azure CLI, Azure PowerShell cmdlet'leri ile oluşturma üzerinde bağlıdır. Azure CLI belirtin `--restart-policy` çağırdığınızda parametresi [az kapsayıcı oluşturmak][az-container-create].

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer \
    --image mycontainerimage \
    --restart-policy OnFailure
```

## <a name="run-to-completion-example"></a>Tamamlama örneği çalıştırma

Eylem yeniden başlatma ilkesi görmek için bir kapsayıcı örneğinden oluşturma [aci/microsoft-wordcount] [ aci-wordcount-image] görüntü ve belirtin `OnFailure` İlkesi yeniden başlatın. Bu örnek kapsayıcı, varsayılan olarak, Shakespeare'nın metin çözümleyen bir Python betiği çalıştıran [Hamlet](http://shakespeare.mit.edu/hamlet/full.html)10 en sık kullanılan sözcük STDOUT yazar ve ardından çıkar.

Örnek kapsayıcı ile aşağıdaki komutu çalıştırarak [az kapsayıcı oluşturmak] [ az-container-create] komutu:

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer \
    --image microsoft/aci-wordcount:latest \
    --restart-policy OnFailure
```

Azure kapsayıcı örnekleri kapsayıcı başlatır ve kendi uygulama (veya bu durumda betik) çıktığında durdurur. Azure kapsayıcı örneklerinin bir kapsayıcısı, yeniden başlatma ilkesi durduğunda `Never` veya `OnFailure`, kapsayıcının durum kümesine **kesildi**. Bir kapsayıcının durumuyla denetleyebilirsiniz [az kapsayıcı Göster] [ az-container-show] komutu:

```azurecli-interactive
az container show --resource-group myResourceGroup --name mycontainer --query containers[0].instanceView.currentState.state
```

Örnek çıktı:

```bash
"Terminated"
```

Örnek kapsayıcının durumunu gösterir sonra *kesildi*, kapsayıcı günlükleri görüntüleyerek görev çıktısını görebilirsiniz. Çalıştırma [az kapsayıcı günlükleri] [ az-container-logs] komut dosyasını görüntülemek için komut çıktısı:

```azurecli-interactive
az container logs --resource-group myResourceGroup --name mycontainer
```

Çıktı:

```bash
[('the', 990),
 ('and', 702),
 ('of', 628),
 ('to', 610),
 ('I', 544),
 ('you', 495),
 ('a', 453),
 ('my', 441),
 ('in', 399),
 ('HAMLET', 386)]
```

Bu örnek betik STDOUT gönderilen çıkış gösterir. Kapsayıcılı görevlerinizi, sonraki alma için kalıcı depolama birimine ancak, kendi çıktı yerine yazabilirsiniz. Örneğin, bir [Azure dosya paylaşımı](container-instances-mounting-azure-files-volume.md).

## <a name="configure-containers-at-runtime"></a>Çalışma zamanında kapsayıcıları yapılandırın

Bir kapsayıcı örneği oluşturduğunuzda, ayarlayabileceğiniz kendi **ortam değişkenleri**, yanı sıra özel bir belirtin **komut satırı** kapsayıcı başlatıldığında çalıştırılacak. Her kapsayıcı görev özgü yapılandırma ile hazırlamak için batch işleriniz bu ayarları kullanın.

## <a name="environment-variables"></a>Ortam değişkenleri

Ortam değişkenleri uygulama veya komut dosyası kapsayıcı tarafından çalıştırmak, dinamik olarak yapılandırılmasını sağlamak için kapsayıcı olarak ayarlayın. Bu benzer `--env` komut satırı bağımsız değişkeni `docker run`.

Örneğin, kapsayıcı örneği oluşturduğunuzda aşağıdaki ortam değişkenleri belirterek örnek kapsayıcı betik davranışını değiştirebilirsiniz:

*NumWords*: STDOUT gönderilen sözcük sayısı.

*MinLength*: en az bir sözcük, sayılması için karakter sayısı. Sık kullanılan sözcük gibi "," ve "." daha yüksek bir sayı yoksayar

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer2 \
    --image microsoft/aci-wordcount:latest \
    --restart-policy OnFailure \
    --environment-variables NumWords=5 MinLength=8
```

Belirterek `NumWords=5` ve `MinLength=8` kapsayıcının ortam değişkenleri için farklı bir çıkış kapsayıcı günlükleri görüntülenmelidir. Kapsayıcı durumu olarak gösterilir sonra *kesildi* (kullanmak `az container show` durumunu denetlemek için), yeni çıkış görmek için günlükleri görüntüleme:

```azurecli-interactive
az container logs --resource-group myResourceGroup --name mycontainer2
```

Çıktı:

```bash
[('CLAUDIUS', 120),
 ('POLONIUS', 113),
 ('GERTRUDE', 82),
 ('ROSENCRANTZ', 69),
 ('GUILDENSTERN', 54)]
```

## <a name="command-line-override"></a>Komut satırı geçersiz kılma

Kapsayıcı görüntüsüne baked komut satırı geçersiz kılmak için bir kapsayıcı örneği oluşturduğunuzda, bir komut satırı belirtin. Bu benzer `--entrypoint` komut satırı bağımsız değişkeni `docker run`.

Örneğin, metin dışındaki analiz örnek kapsayıcı olabilir *Hamlet* farklı komut satırı belirterek. Kapsayıcı tarafından yürütülen Python betiği *wordcount.py*, bir URL bağımsız değişken olarak kabul eder ve varsayılan yerine bu sayfanın içeriğinin işleyecek.

Örneğin, en üst 3 beş harfli sözcükleri belirlemek için *Romeo ve Juliet*:

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer3 \
    --image microsoft/aci-wordcount:latest \
    --restart-policy OnFailure \
    --environment-variables NumWords=3 MinLength=5 \
    --command-line "python wordcount.py http://shakespeare.mit.edu/romeo_juliet/full.html"
```

Yeniden kapsayıcı olduğunda *kesildi*, kapsayıcının günlükleri göstererek çıktısını görüntüleyin:

```azurecli-interactive
az container logs --resource-group myResourceGroup --name mycontainer3
```

Çıktı:

```bash
[('ROMEO', 177), ('JULIET', 134), ('CAPULET', 119)]
```

## <a name="next-steps"></a>Sonraki adımlar

### <a name="persist-task-output"></a>Görev çıktısı Sürdür

Tamamlanıncaya kadar çalışabilmesi kapsayıcılarınızı çıktısını kalıcı hakkında ayrıntılar için bkz: [Azure kapsayıcı örnekleri ile Azure dosya paylaşımının takma](container-instances-mounting-azure-files-volume.md).

<!-- LINKS - External -->
[aci-wordcount-image]: https://hub.docker.com/r/microsoft/aci-wordcount/

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container?view=azure-cli-latest#az_container_create
[az-container-logs]: /cli/azure/container?view=azure-cli-latest#az_container_logs
[az-container-show]: /cli/azure/container?view=azure-cli-latest#az_container_show
[azure-cli-install]: /cli/azure/install-azure-cli
