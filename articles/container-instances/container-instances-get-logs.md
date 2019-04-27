---
title: Kapsayıcı günlüklerini ve olayları Azure Container Instances ile alın
description: Kapsayıcı günlüklerini ve olayları Azure Container Instances ile hata ayıklama hakkında bilgi edinin
services: container-instances
author: dlepow
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 03/21/2019
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: f286e2136b12a88e65e40f8fb956542233f71715
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60579791"
---
# <a name="retrieve-container-logs-and-events-in-azure-container-instances"></a>Kapsayıcı günlüklerini ve olayları Azure Container ınstances'da alma

İle onun günlüklerini görüntüleyerek davranan bir kapsayıcıya sahip olduğunuzda, Başlat [az kapsayıcı günlüklerini][az-container-logs]ve kendi standart çıkış ve standart hata akışı [az kapsayıcı ekleme] [az-container-attach].

## <a name="view-logs"></a>Günlükleri görüntüleme

Bir kapsayıcıdaki uygulama kodunuzdan günlükleri görüntülemek için kullanabileceğiniz [az kapsayıcı günlüklerini] [ az-container-logs] komutu.

Görev tabanlı örnek kapsayıcısında günlük çıktısını verilmiştir [ACI kapsayıcı görevi çalıştırma](container-instances-restart-policy.md)işlemek için geçersiz bir URL beslenir sonra:

```console
$ az container logs --resource-group myResourceGroup --name mycontainer
Traceback (most recent call last):
  File "wordcount.py", line 11, in <module>
    urllib.request.urlretrieve (sys.argv[1], "foo.txt")
  File "/usr/local/lib/python3.6/urllib/request.py", line 248, in urlretrieve
    with contextlib.closing(urlopen(url, data)) as fp:
  File "/usr/local/lib/python3.6/urllib/request.py", line 223, in urlopen
    return opener.open(url, data, timeout)
  File "/usr/local/lib/python3.6/urllib/request.py", line 532, in open
    response = meth(req, response)
  File "/usr/local/lib/python3.6/urllib/request.py", line 642, in http_response
    'http', request, response, code, msg, hdrs)
  File "/usr/local/lib/python3.6/urllib/request.py", line 570, in error
    return self._call_chain(*args)
  File "/usr/local/lib/python3.6/urllib/request.py", line 504, in _call_chain
    result = func(*args)
  File "/usr/local/lib/python3.6/urllib/request.py", line 650, in http_error_default
    raise HTTPError(req.full_url, code, msg, hdrs, fp)
urllib.error.HTTPError: HTTP Error 404: Not Found
```

## <a name="attach-output-streams"></a>Çıkış akışları ekleme

[Az kapsayıcı ekleme] [ az-container-attach] komutu, kapsayıcı başlatma sırasında tanılama bilgileri sağlar. Kapsayıcı başladıktan sonra yerel konsolunuza STDOUT ve STDERR akışı.

Örneğin, görev tabanlı kapsayıcısında çıktısı şöyledir [ACI kapsayıcı görevi çalıştırma](container-instances-restart-policy.md)işlemek için geçerli bir URL büyük metin dosyasının sağlanan sonra:

```console
$ az container attach --resource-group myResourceGroup --name mycontainer
Container 'mycontainer' is in state 'Unknown'...
Container 'mycontainer' is in state 'Waiting'...
Container 'mycontainer' is in state 'Running'...
(count: 1) (last timestamp: 2019-03-21 19:42:39+00:00) pulling image "mcr.microsoft.com/azuredocs/aci-wordcount:latest"
Container 'mycontainer1' is in state 'Running'...
(count: 1) (last timestamp: 2019-03-21 19:42:39+00:00) pulling image "mcr.microsoft.com/azuredocs/aci-wordcount:latest"
(count: 1) (last timestamp: 2019-03-21 19:42:52+00:00) Successfully pulled image "mcr.microsoft.com/azuredocs/aci-wordcount:latest"
(count: 1) (last timestamp: 2019-03-21 19:42:55+00:00) Created container
(count: 1) (last timestamp: 2019-03-21 19:42:55+00:00) Started container

Start streaming logs:
[('the', 22979),
 ('I', 20003),
 ('and', 18373),
 ('to', 15651),
 ('of', 15558),
 ('a', 12500),
 ('you', 11818),
 ('my', 10651),
 ('in', 9707),
 ('is', 8195)]
```

## <a name="get-diagnostic-events"></a>Tanılama Olayları Alma

Kapsayıcınızı başarıyla dağıtmak başarısız olursa Azure Container Instances kaynak sağlayıcısı tarafından sağlanan tanılama bilgileri gözden geçirmeniz gerekir. Kapsayıcı olayları görüntülemek için [az container show] [az container show] komutunu çalıştırın:

```azurecli-interactive
az container show --resource-group myResourceGroup --name mycontainer
```

Çıktı (burada kesilmiş olarak gösterilen) dağıtım olaylarla birlikte kapsayıcınızı çekirdek özellikleri içerir:

```JSON
{
  "containers": [
    {
      "command": null,
      "environmentVariables": [],
      "image": "mcr.microsoft.com/azuredocs/aci-helloworld",
      ...
        "events": [
          {
            "count": 1,
            "firstTimestamp": "2019-03-21T19:46:22+00:00",
            "lastTimestamp": "2019-03-21T19:46:22+00:00",
            "message": "pulling image \"mcr.microsoft.com/azuredocs/aci-helloworld\"",
            "name": "Pulling",
            "type": "Normal"
          },
          {
            "count": 1,
            "firstTimestamp": "2019-03-21T19:46:28+00:00",
            "lastTimestamp": "2019-03-21T19:46:28+00:00",
            "message": "Successfully pulled image \"mcr.microsoft.com/azuredocs/aci-helloworld\"",
            "name": "Pulled",
            "type": "Normal"
          },
          {
            "count": 1,
            "firstTimestamp": "2019-03-21T19:46:31+00:00",
            "lastTimestamp": "2019-03-21T19:46:31+00:00",
            "message": "Created container",
            "name": "Created",
            "type": "Normal"
          },
          {
            "count": 1,
            "firstTimestamp": "2019-03-21T19:46:31+00:00",
            "lastTimestamp": "2019-03-21T19:46:31+00:00",
            "message": "Started container",
            "name": "Started",
            "type": "Normal"
          }
        ],
        "previousState": null,
        "restartCount": 0
      },
      "name": "mycontainer",
      "ports": [
        {
          "port": 80,
          "protocol": null
        }
      ],
      ...
    }
  ],
  ...
}
```
## <a name="next-steps"></a>Sonraki adımlar
Bilgi edinmek için nasıl [ortak kapsayıcı ve dağıtım sorunlarını giderme](container-instances-troubleshooting.md) Azure Container Instances için.

<!-- LINKS - Internal -->
[az-container-attach]: /cli/azure/container#az-container-attach
[az-container-logs]: /cli/azure/container#az-container-logs
