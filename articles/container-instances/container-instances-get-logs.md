---
title: Kapsayıcı günlüklerini ve olayları Azure Container Instances ile alın
description: Kapsayıcı günlüklerini ve olayları Azure Container Instances ile hata ayıklama hakkında bilgi edinin
services: container-instances
author: jluk
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 05/30/2018
ms.author: juluk
ms.custom: mvc
ms.openlocfilehash: 21e75beffbf592f25257b0dca7ba48b0ffe560a8
ms.sourcegitcommit: 8ca6cbe08fa1ea3e5cdcd46c217cfdf17f7ca5a7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/22/2019
ms.locfileid: "56669780"
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
Bilgi edinmek için nasıl [ortak kapsayıcı ve dağıtım sorunlarını giderme](container-instances-troubleshooting.md) Azure Container Instances için.

<!-- LINKS - Internal -->
[az-container-attach]: /cli/azure/container#az-container-attach
[az-container-logs]: /cli/azure/container#az-container-logs
