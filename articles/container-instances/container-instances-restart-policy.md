---
title: Azure Container Instances'da kullanım yeniden ilkeleri ile kapsayıcılı görevleri
description: Derleme, test veya görüntü işleme işlerini gibi tamamlanmak üzere çalıştırılmasını görevleri yürütmek için Azure Container Instances'ı kullanmayı öğrenin.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: article
ms.date: 04/15/2019
ms.author: danlep
ms.openlocfilehash: 06872eefd0d500a22214109ad5055dd236b5a6ac
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60608122"
---
# <a name="run-containerized-tasks-with-restart-policies"></a>Yeniden başlatma ilkeleri ile kapsayıcılı görevleri çalıştırma

Azure Container ınstances'da kapsayıcı dağıtma hızlı ve kolay bir kapsayıcı örneği derleme, test ve görüntü işleme gibi kere çalıştırılacak görevleri yürütmek için ilgi çekici bir platform sağlar.

Yapılandırılabilir yeniden başlatma ilkesi ile işlemlerini tamamladıktan sonra kapsayıcılarınızı durdurulur belirtebilirsiniz. Kapsayıcı örnekleri saniye bazında faturalandırılır olduğundan, yalnızca Görev yürütülürken kapsayıcı çalışırken kullanılan işlem kaynakları için ücretlendirilirsiniz.

Örnek Azure CLI bu makalede kullanımda sunulur. Azure CLI Sürüm 2.0.21 olmalıdır veya büyük [yerel olarak yüklü][azure-cli-install], veya CLI'daki [Azure Cloud Shell](../cloud-shell/overview.md).

## <a name="container-restart-policy"></a>Kapsayıcı yeniden başlatma ilkesi

Oluştururken bir [kapsayıcı grubu](container-instances-container-groups.md) Azure Container Instances'da üç yeniden başlatma ilkesi ayarlarından birini belirtebilirsiniz.

| Yeniden başlatma ilkesi   | Açıklama |
| ---------------- | :---------- |
| `Always` | Kapsayıcı grubundaki kapsayıcı her zaman yeniden başlatılır. Bu **varsayılan** container oluşturulması sırasında hiçbir yeniden başlatma ilkesi belirtildiğinde uygulanan ayarı. |
| `Never` | Kapsayıcı grubundaki kapsayıcı hiçbir zaman yeniden başlatılır. Kapsayıcıları en fazla bir kez çalıştırın. |
| `OnFailure` | Kapsayıcı grubundaki kapsayıcı (sıfır olmayan çıkış kodu ile sonlandırıldığında) yalnızca kapsayıcıda yürütülen işlemin başarısız olduğunda başlatılır. Kapsayıcıları en az bir kez çalıştırılır. |

## <a name="specify-a-restart-policy"></a>Yeniden başlatma ilkesi belirtin

Nasıl bir yeniden başlatma İlkesi'ni belirtin, kapsayıcı örneklerinizin gibi Azure PowerShell cmdlet'lerini, Azure CLI veya Azure portalında oluşturma üzerinde bağlıdır. Azure CLI'de belirtin `--restart-policy` çağırdığınızda parametresi [az kapsayıcı oluşturma][az-container-create].

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer \
    --image mycontainerimage \
    --restart-policy OnFailure
```

## <a name="run-to-completion-example"></a>Tamamlama örneği çalıştırma

Yeniden başlatma ilkesi iş başında görmek için Microsoft'tan bir kapsayıcı örneği oluşturma [Acı wordcount] [ aci-wordcount-image] görüntü ve belirtin `OnFailure` yeniden başlatma ilkesi. Bu örnek kapsayıcı varsayılan olarak, Shakespeare'nın metni inceler, bir Python betiği çalıştıran [Hamlet](http://shakespeare.mit.edu/hamlet/full.html)en yaygın 10 sözcükleri STDOUT yazar ve kapanır.

Örnek kapsayıcı ile aşağıdaki komutu çalıştırın [az kapsayıcı oluşturma] [ az-container-create] komutu:

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer \
    --image mcr.microsoft.com/azuredocs/aci-wordcount:latest \
    --restart-policy OnFailure
```

Azure Container Instances'a kapsayıcı başladıktan ve kendi uygulama (veya bu durumda betik) çıktığında durdurur. Azure Container Instances, yeniden başlatma ilkesi olan bir kapsayıcı durduğunda `Never` veya `OnFailure`, kapsayıcının durumu kümesine **kesildi**. Bir kapsayıcının durumuyla denetleyebilirsiniz [az container show] [ az-container-show] komutu:

```azurecli-interactive
az container show --resource-group myResourceGroup --name mycontainer --query containers[0].instanceView.currentState.state
```

Örnek çıktı:

```bash
"Terminated"
```

Örnek kapsayıcının durumunun sonra *kesildi*, kapsayıcı günlüklerini görüntüleyerek görev çıktısını görebilirsiniz. Çalıştırma [az kapsayıcı günlüklerini] [ az-container-logs] betiğini görüntülemek için komut çıktısı:

```azurecli-interactive
az container logs --resource-group myResourceGroup --name mycontainer
```

Çıkış:

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

Bu örnek betik STDOUT için gönderilen bir çıktı gösterir. Kapsayıcılı görevlerinizi, sonraki alma için kalıcı depolama için ancak çıktılarını yerine yazabilirsiniz. Örneğin, bir [Azure dosya paylaşımının](container-instances-mounting-azure-files-volume.md).

## <a name="next-steps"></a>Sonraki adımlar

Toplu işleme çeşitli kapsayıcıları ile büyük bir veri kümesini gibi görev tabanlı senaryoları yararlanabilir özel [ortam değişkenlerini](container-instances-environment-variables.md) veya [komut satırları](container-instances-start-command.md) zamanında.

Tamamlanmak üzere çalıştırılmasını kapsayıcılarınızı çıktısını kalıcı hale getirmek hakkında ayrıntılı bilgi için bkz. [Azure Container Instances ile Azure dosya paylaşımını bağlama](container-instances-mounting-azure-files-volume.md).

<!-- LINKS - External -->
[aci-wordcount-image]: https://hub.docker.com/_/microsoft-azuredocs-aci-wordcount

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container?view=azure-cli-latest#az-container-create
[az-container-logs]: /cli/azure/container?view=azure-cli-latest#az-container-logs
[az-container-show]: /cli/azure/container?view=azure-cli-latest#az-container-show
[azure-cli-install]: /cli/azure/install-azure-cli
