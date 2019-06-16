---
title: Azure Container Instances'da bir başlangıç komut satırını kullanın
description: İçindeki bir kapsayıcı görüntüsü Azure container örneği dağıttığınızda yapılandırılmış giriş noktasını geçersiz kıl
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: article
ms.date: 04/15/2019
ms.author: danlep
ms.openlocfilehash: da94a4c79694f511d41e5c8dda8c786fc7049726
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64569640"
---
# <a name="set-the-command-line-in-a-container-instance-to-override-the-default-command-line-operation"></a>Komut satırı varsayılan komut satırı işlemi geçersiz kılmak için bir kapsayıcı örneği ayarlayın

İsteğe bağlı olarak bir kapsayıcı örneği oluşturduğunuzda, kapsayıcı görüntüsüne desteklenmiş varsayılan komut satırı yönergesi geçersiz kılmak için bir komutu belirtin. Bu davranış benzer `--entrypoint` komut satırı bağımsız değişkeni `docker run`.

İstediğiniz ayarı [ortam değişkenlerini](container-instances-environment-variables.md) container Instances için komut satırı başlatılıyor belirtme yararlıdır toplu işlemler için burada görev özgü yapılandırma ile dinamik olarak her kapsayıcı hazırlamanız gerekir.

## <a name="command-line-guidelines"></a>Komut satırı yönergeleri

* Varsayılan olarak, komut satırını belirtir. bir *tek bir kabuk başlatır işlem* kapsayıcısında. Örneğin, komut satırında Python betiğini veya yürütülebilir dosya çalıştırabilirsiniz. 

* Birden çok komut yürütmek için komut satırınızda kapsayıcı işletim sisteminin desteklenen bir kabuk ortamını ayarlayarak başlar. Örnekler:

  |İşletim sistemi  |Varsayılan kabuğunu  |
  |---------|---------|
  |Ubuntu     |   `/bin/bash`      |
  |Alpine     |   `/bin/sh`      |
  |Windows     |    `cmd`     |

  Birden çok komutları sırayla çalıştırmak için birleştirmek için kabuk kuralları takip edin.

* Kapsayıcı yapılandırmasına bağlı olarak, komut satırı yürütülebilirini tam yolunu ya da bağımsız değişkenler ayarlamanız gerekebilir.

* Uygun bir kümesi [yeniden başlatma ilkesi](container-instances-restart-policy.md) olup komut satırı uzun süre çalışan bir görev veya bir kere çalıştırılacak görev belirtir bağlı olarak, kapsayıcı örneği için. Örneğin, yeniden başlatma ilkesi `Never` veya `OnFailure` bir kere çalıştırılacak görev için önerilir. 

* Varsayılan giriş noktası bir kapsayıcı görüntüsüne ayarlama hakkında bilgi almanız gerekiyorsa, kullanın [docker görüntüsü İnceleme](https://docs.docker.com/engine/reference/commandline/image_inspect/) komutu.

## <a name="command-line-syntax"></a>Komut satırı sözdizimi

Komut satırı sözdizimi örnekleri oluşturmak için kullanılan araç ve Azure API bağlı olarak değişir. Ayrıca bir kabuk ortamını belirtirseniz, kabuğun komut sözdizimi kurallarının gözlemleyin.

* [az container oluşturma] [ az-container-create] komutu: İle bir dizeyi geçirmek `--command-line` parametresi. Örnek: `--command-line "python myscript.py arg1 arg2"`).

* [Yeni-AzureRmContainerGroup] [ new-azurermcontainergroup] Azure PowerShell cmdlet: İle bir dizeyi geçirmek `-Command` parametresi. Örnek: `-Command "echo hello"`.

* Azure portalı: İçinde **komutu geçersiz kılma** kapsayıcı yapılandırmasının özelliği dizeleri, tırnak işaretleri olmadan, virgülle ayrılmış listesini sağlayın. Örnek: `python, myscript.py, arg1, arg2`). 

* Resource Manager şablonu veya YAML dosyası ya da Azure SDK'ları biri: Komut satırı özelliği dize dizisi belirtin. Örnek: JSON dizisi `["python", "myscript.py", "arg1", "arg2"]` Resource Manager şablonunda. 

  Aşina değilseniz [Dockerfile](https://docs.docker.com/engine/reference/builder/) sözdizimi, bu biçim benzer *exec* CMD yönerge biçimi.

### <a name="examples"></a>Örnekler

|    |  Azure CLI   | Portal | Şablon | 
| ---- | ---- | --- | --- |
| Tek komutu | `--command-line "python myscript.py arg1 arg2"` | **Komut geçersiz kılma**: `python, myscript.py, arg1, arg2` | `"command": ["python", "myscript.py", "arg1", "arg2"]` |
| Birden çok komut | `--command-line "/bin/bash -c 'mkdir test; touch test/myfile; tail -f /dev/null'"` |**Komut geçersiz kılma**: `/bin/bash, -c, mkdir test; touch test/myfile; tail -f /dev/null` | `"command": ["/bin/bash", "-c", "mkdir test; touch test/myfile; tail -f /dev/null"]` |

## <a name="azure-cli-example"></a>Azure CLI örneği

Örneğin, davranışını değiştirmek [Acı/microsoft-wordcount] [ aci-wordcount] Shakespeare'nın metni inceler, kapsayıcı görüntüsü *Hamlet* en sık bulmak için sözcükleri gerçekleşiyor. Çözümleme yerine *Hamlet*, farklı metin kaynağına işaret eden bir komut satırı ayarlayabilirsiniz.

Çıktısını görmek için [Acı/microsoft-wordcount] [ aci-wordcount] şununla Çalıştır varsayılan metin analiz edilirken kapsayıcı [az kapsayıcı oluşturma] [ az-container-create] komutu. Varsayılan kapsayıcı komutu çalıştırır için hiçbir başlangıç komut satırı belirtildi. Gösterim amacıyla, bu örnekte [ortam değişkenlerini](container-instances-environment-variables.md) belirten harften en az beş ilk 3 sözcükler bulmak için:

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer1 \
    --image mcr.microsoft.com/azuredocs/aci-wordcount:latest \
    --environment-variables NumWords=3 MinLength=5 \
    --restart-policy OnFailure
```

Olarak kapsayıcının durumunu gösterir. bir kez *kesildi* (kullanın [az container show] [ az-container-show] durumunu denetlemek için), ile günlük görüntüleme [az kapsayıcı günlüklerini] [ az-container-logs] çıktıyı görmek için.

```azurecli-interactive
az container logs --resource-group myResourceGroup --name mycontainer1
```

Çıktı:

```console
[('HAMLET', 386), ('HORATIO', 127), ('CLAUDIUS', 120)]
```

Şimdi farklı metni farklı komut satırı belirterek analiz etmek için ikinci bir örnek kapsayıcısı ayarlamak ayarlayın. Kapsayıcı tarafından yürütülen Python betiğini *wordcount.py*, bir URL bağımsız değişken olarak kabul eder ve bu sayfanın içeriğinin varsayılan yerine işler.

Örneğin, üst belirlemek için en az beş olan 3 sözcükleri harfleri uzunluk *Romeo ve Juliet*:

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer2 \
    --image mcr.microsoft.com/azuredocs/aci-wordcount:latest \
    --restart-policy OnFailure \
    --environment-variables NumWords=3 MinLength=5 \
    --command-line "python wordcount.py http://shakespeare.mit.edu/romeo_juliet/full.html"
```

Yeniden kapsayıcı başladıktan sonra *kesildi*, kapsayıcının günlüklerini göstererek çıktısını görüntüleyin:

```azurecli-interactive
az container logs --resource-group myResourceGroup --name mycontainer2
```

Çıktı:

```console
[('ROMEO', 177), ('JULIET', 134), ('CAPULET', 119)]
```

## <a name="next-steps"></a>Sonraki adımlar

Toplu işleme çeşitli kapsayıcıları ile büyük bir veri kümesini gibi görev tabanlı senaryoları, çalışma zamanında özel komut satırlarından yararlı olabilir. Görev tabanlı kapsayıcı çalıştırma hakkında daha fazla bilgi için bkz. [yeniden başlatma ilkeleri ile kapsayıcılı görevleri çalıştırma](container-instances-restart-policy.md).

<!-- LINKS - External -->
[aci-wordcount]: https://hub.docker.com/_/microsoft-azuredocs-aci-wordcount

<!-- LINKS Internal -->
[az-container-create]: /cli/azure/container#az-container-create
[az-container-logs]: /cli/azure/container#az-container-logs
[az-container-show]: /cli/azure/container#az-container-show
[new-azurermcontainergroup]: /powershell/module/azurerm.containerinstance/new-azurermcontainergroup
[portal]: https://portal.azure.com
