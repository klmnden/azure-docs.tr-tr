---
title: Kapsayıcı günlüklerini ve olayları ile Azure kapsayıcı örnekleri alın
description: Kapsayıcı günlüklerini ve olayları Azure kapsayıcı örneğiyle ile hata ayıklama öğrenin
services: container-instances
author: jluk
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 05/30/18
ms.author: juluk
ms.custom: mvc
ms.openlocfilehash: e10abc0e1b8c163af5d0d42cccfe62b05557b0a4
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34701410"
---
# <a name="retrieve-container-logs-and-events-in-azure-container-instances"></a>Kapsayıcı günlüklerini ve olayları Azure kapsayıcı durumlarda alma

Hatalı davranan bir kapsayıcıya sahip, kendi günlükleri ile görüntüleyerek başlattığınızda [az kapsayıcı günlükleri][az-container-logs]ve kendi standart çıkış ve standart hata akışı [az kapsayıcı ekleme] [az-container-attach].

## <a name="view-logs"></a>Günlükleri görüntüleme

Bir kapsayıcı içindeki uygulama kodunuzdan günlükleri görüntülemek için kullanabileceğiniz [az kapsayıcı günlükleri] [ az-container-logs] komutu.

Görev tabanlı örnek kapsayıcısında günlük çıktısı aşağıdadır [ACI içinde kapsayıcılı bir görevi çalıştırmayı](container-instances-restart-policy.md)işlemek için geçersiz bir URL sat sonra:

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

[Az kapsayıcı ekleme] [ az-container-attach] komutu kapsayıcı başlatma sırasında tanılama bilgileri sağlar. Kapsayıcı başladıktan sonra yerel konsolunuza STDOUT ve STDERR akışları.

Örneğin, görev tabanlı kapsayıcısında çıktısını işte [ACI içinde kapsayıcılı bir görevi çalıştırmayı](container-instances-restart-policy.md)işlemek için geçerli bir URL büyük metin dosyasının sağlanan sonra:

```console
$ az container attach --resource-group myResourceGroup --name mycontainer
Container 'mycontainer' is in state 'Unknown'...
Container 'mycontainer' is in state 'Waiting'...
Container 'mycontainer' is in state 'Running'...
(count: 1) (last timestamp: 2018-03-09 23:21:33+00:00) pulling image "microsoft/aci-wordcount:latest"
(count: 1) (last timestamp: 2018-03-09 23:21:49+00:00) Successfully pulled image "microsoft/aci-wordcount:latest"
(count: 1) (last timestamp: 2018-03-09 23:21:49+00:00) Created container with id e495ad3e411f0570e1fd37c1e73b0e0962f185aa8a7c982ebd410ad63d238618
(count: 1) (last timestamp: 2018-03-09 23:21:49+00:00) Started container with id e495ad3e411f0570e1fd37c1e73b0e0962f185aa8a7c982ebd410ad63d238618

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

## <a name="get-diagnostic-events"></a>Tanılama Olayları Al

Başarıyla dağıtmak, kapsayıcı başarısız olursa, Azure kapsayıcı örnekleri kaynak sağlayıcısı tarafından sağlanan tanı bilgilerini gözden geçirmeniz gerekir. Kapsayıcı olayları görüntülemek için [az kapsayıcı Göster] [az kapsayıcı Göster] komutunu çalıştırın:

```azurecli-interactive
az container show --resource-group myResourceGroup --name mycontainer
```

Çıktı (burada kesilmiş gösterilen) dağıtım olayları yanı sıra, kapsayıcı çekirdek özelliklerini içerir:

```JSON
{
  "containers": [
    {
      "command": null,
      "environmentVariables": [],
      "image": "microsoft/aci-helloworld",
      ...
        "events": [
          {
            "count": 1,
            "firstTimestamp": "2017-12-21T22:50:49+00:00",
            "lastTimestamp": "2017-12-21T22:50:49+00:00",
            "message": "pulling image \"microsoft/aci-helloworld\"",
            "name": "Pulling",
            "type": "Normal"
          },
          {
            "count": 1,
            "firstTimestamp": "2017-12-21T22:50:59+00:00",
            "lastTimestamp": "2017-12-21T22:50:59+00:00",
            "message": "Successfully pulled image \"microsoft/aci-helloworld\"",
            "name": "Pulled",
            "type": "Normal"
          },
          {
            "count": 1,
            "firstTimestamp": "2017-12-21T22:50:59+00:00",
            "lastTimestamp": "2017-12-21T22:50:59+00:00",
            "message": "Created container with id 2677c7fd54478e5adf6f07e48fb71357d9d18bccebd4a91486113da7b863f91f",
            "name": "Created",
            "type": "Normal"
          },
          {
            "count": 1,
            "firstTimestamp": "2017-12-21T22:50:59+00:00",
            "lastTimestamp": "2017-12-21T22:50:59+00:00",
            "message": "Started container with id 2677c7fd54478e5adf6f07e48fb71357d9d18bccebd4a91486113da7b863f91f",
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
Bilgi edinmek için nasıl [ortak kapsayıcı ve dağıtım sorunlarını giderme](container-instances-troubleshooting.md) Azure kapsayıcı örnekleri için.

<!-- LINKS - Internal -->
[az-container-attach]: /cli/azure/container#az_container_attach
[az-container-logs]: /cli/azure/container#az_container_logs