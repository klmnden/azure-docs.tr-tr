---
title: Azure kapsayıcı örnekleri sorunlarını giderme
description: Azure kapsayıcı örnekleri ile ilgili sorunları giderme hakkında bilgi edinin
services: container-instances
author: seanmck
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 03/14/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: a4067db9955b804f126e889fa73641f69fef56ab
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="troubleshoot-container-and-deployment-issues-in-azure-container-instances"></a>Azure kapsayıcı örnekleri de kapsayıcı ve dağıtım sorunlarını giderme

Bu makalede kapsayıcıları Azure kapsayıcı örnekleri dağıtırken ilgili sorunları gidermek nasıl gösterilmektedir. Ayrıca bazı içine çalışabilir yaygın sorunları açıklar.

## <a name="view-logs-and-stream-output"></a>Günlükleri görüntüle ve akış çıkışı

Hatalı davranan bir kapsayıcıya sahip, kendi günlükleri ile görüntüleyerek başlattığınızda [az kapsayıcı günlükleri][az-container-logs]ve kendi standart çıkış ve standart hata akışı [az kapsayıcı ekleme] [az-container-attach].

### <a name="view-logs"></a>Günlükleri görüntüleme

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

### <a name="attach-output-streams"></a>Çıkış akışları ekleme

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

Başarıyla dağıtmak, kapsayıcı başarısız olursa, Azure kapsayıcı örnekleri kaynak sağlayıcısı tarafından sağlanan tanı bilgilerini gözden geçirmeniz gerekir. Kapsayıcı için olayları görüntülemek için çalıştırın [az kapsayıcı Göster] [ az-container-show] komutu:

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

## <a name="common-deployment-issues"></a>Genel dağıtım sorunları

Aşağıdaki bölümlerde, kapsayıcı dağıtımda hataların çoğu bu hesaba sık karşılaşılan sorunları açıklanmaktadır:

* [Görüntü sürümü desteklenmiyor](#image-version-not-supported)
* [Çekme görüntü oluşturulamıyor](#unable-to-pull-image)
* [Kapsayıcı sürekli olarak çıkar ve yeniden başlatır](#container-continually-exits-and-restarts)
* [Kapsayıcı başlatmak için çok uzun sürüyor](#container-takes-a-long-time-to-start)
* ["Kaynak mevcut değil" hatası](#resource-not-available-error)

## <a name="image-version-not-supported"></a>Görüntü sürümü desteklenmiyor

Azure kapsayıcı örnekler destekleyemiyor, bir görüntü belirtirseniz bir `ImageVersionNotSupported` hata döndürülür. Hata değeri `The version of image '{0}' is not supported.`ve şu anda Windows 1709 yansımaları için geçerlidir. Bu sorunu azaltmak için LTS Windows görüntüyü kullanın. Windows 1709 görüntüler için destek işleniyor.

## <a name="unable-to-pull-image"></a>Çekme görüntü oluşturulamıyor

Azure kapsayıcı örnek görüntünüzü başlangıçta çekmesini kaydedemediği sonunda başarısız önce belirli bir süre için yeniden dener. Görüntü çekilen, olayları aşağıdaki gibi çıktısında gösterilen [az kapsayıcı Göster][az-container-show]:

```bash
"events": [
  {
    "count": 3,
    "firstTimestamp": "2017-12-21T22:56:19+00:00",
    "lastTimestamp": "2017-12-21T22:57:00+00:00",
    "message": "pulling image \"microsoft/aci-hellowrld\"",
    "name": "Pulling",
    "type": "Normal"
  },
  {
    "count": 3,
    "firstTimestamp": "2017-12-21T22:56:19+00:00",
    "lastTimestamp": "2017-12-21T22:57:00+00:00",
    "message": "Failed to pull image \"microsoft/aci-hellowrld\": rpc error: code 2 desc Error: image t/aci-hellowrld:latest not found",
    "name": "Failed",
    "type": "Warning"
  },
  {
    "count": 3,
    "firstTimestamp": "2017-12-21T22:56:20+00:00",
    "lastTimestamp": "2017-12-21T22:57:16+00:00",
    "message": "Back-off pulling image \"microsoft/aci-hellowrld\"",
    "name": "BackOff",
    "type": "Normal"
  }
],
```

Çözümlemek için kapsayıcıyı silin ve görüntü adı doğru yazdığınızı ödeyen Kapat dikkat dağıtımınızı yeniden deneyin.

## <a name="container-continually-exits-and-restarts"></a>Kapsayıcı sürekli olarak çıkar ve yeniden başlatır

Kapsayıcı tamamlanıncaya kadar çalışır ve otomatik olarak yeniden başlatır, ayarlamak gerekebilecek bir [İlkesi yeniden](container-instances-restart-policy.md) , **OnFailure** veya **hiçbir zaman**. Belirtirseniz **OnFailure** ve hala bakın sürekli yeniden başlatıldıktan sonra uygulama veya kapsayıcı içinde yürütülen betik ile ilgili bir sorun olabilir.

Kapsayıcı örnekleri API içeren bir `restartCount` özelliği. Kapsayıcı için yeniden başlatma sayısını denetlemek için kullanabileceğiniz [az kapsayıcı Göster] [ az-container-show] Azure CLI 2.0 komutu. Gördüğünüz (hangi okumanızdır kesildi) örnek çıktı aşağıdaki işlemde `restartCount` çıktısının sonunda özelliği.

```json
...
 "events": [
   {
     "count": 1,
     "firstTimestamp": "2017-11-13T21:20:06+00:00",
     "lastTimestamp": "2017-11-13T21:20:06+00:00",
     "message": "Pulling: pulling image \"myregistry.azurecr.io/aci-tutorial-app:v1\"",
     "type": "Normal"
   },
   {
     "count": 1,
     "firstTimestamp": "2017-11-13T21:20:14+00:00",
     "lastTimestamp": "2017-11-13T21:20:14+00:00",
     "message": "Pulled: Successfully pulled image \"myregistry.azurecr.io/aci-tutorial-app:v1\"",
     "type": "Normal"
   },
   {
     "count": 1,
     "firstTimestamp": "2017-11-13T21:20:14+00:00",
     "lastTimestamp": "2017-11-13T21:20:14+00:00",
     "message": "Created: Created container with id bf25a6ac73a925687cafcec792c9e3723b0776f683d8d1402b20cc9fb5f66a10",
     "type": "Normal"
   },
   {
     "count": 1,
     "firstTimestamp": "2017-11-13T21:20:14+00:00",
     "lastTimestamp": "2017-11-13T21:20:14+00:00",
     "message": "Started: Started container with id bf25a6ac73a925687cafcec792c9e3723b0776f683d8d1402b20cc9fb5f66a10",
     "type": "Normal"
   }
 ],
 "previousState": null,
 "restartCount": 0
...
}
```

> [!NOTE]
> Linux dağıtımları için çoğu kapsayıcı görüntüleri bash gibi bir kabuk varsayılan komut olarak ayarlayın. Kendi başına bir kabuk uzun süre çalışan hizmet olmadığından, bu kapsayıcılar hemen çıkın ve varsayılan yapılandırıldığında bir yeniden başlatma döngü içinde kalan **her zaman** İlkesi yeniden başlatın.

## <a name="container-takes-a-long-time-to-start"></a>Kapsayıcı başlatmak için çok uzun sürüyor

Kapsayıcı başlangıç saati, Azure kapsayıcı örnekleri katkıda iki birincil faktörleri şunlardır:

* [Görüntü boyutu](#image-size)
* [Görüntü konumu](#image-location)

Windows görüntülerini sahip [ek hususlar](#use-recent-windows-images).

### <a name="image-size"></a>Görüntü boyutu

Kapsayıcı başlatma, ancak sonuç uzun süren, başarılı, kapsayıcı görüntünüzü boyutta bakarak başlatın. Azure kapsayıcı örnekleri kapsayıcı görüntünüzü isteğe bağlı olarak çeker çünkü karşılaştığınız başlangıç zamanını boyutuna doğrudan ilişkilidir.

Kapsayıcı görüntünüzün boyutunu kullanarak görüntüleyebileceğiniz `docker images` Docker CLI komutunu:

```console
$ docker images
REPOSITORY                  TAG       IMAGE ID        CREATED        SIZE
microsoft/aci-helloworld    latest    7f78509b568e    13 days ago    68.1MB
```

Görüntü boyutları küçük tutmak için anahtarı son görüntünüzü çalışma zamanında gerekli olmayan bir şey içermediğinden emin olmaktır. Yapmanın bir yolu bu olan [çok aşama derlemeleri][docker-multi-stage-builds]. Çok aşama yapma, yalnızca uygulamanız için gereksinim duyduğunuz yapıları son görüntüsünü içeren ve herhangi bir ek içerik sağlamak kolay derleme zamanında gerekli oluşturur.

### <a name="image-location"></a>Görüntü konumu

Kapsayıcı görüntüde barındırmak için kapsayıcının başlangıç zamanında görüntü çekme etkisini azaltmak için başka bir yol olduğu [Azure kapsayıcı kayıt defteri](/azure/container-registry/) kapsayıcı örnekleri dağıtmayı düşündüğünüz burada aynı bölgede. Bu, kapsayıcı görüntü seyahat gereken ağ yolu önemli ölçüde karşıdan yükleme süresini kısaltmak kısaltır.

### <a name="use-recent-windows-images"></a>En son Windows görüntüleri kullanın

Azure kapsayıcı örnekleri hızı kapsayıcı başlangıç zamanını belirli Windows görüntülerini temel görüntüleri için yardımcı olmak için bir önbelleğe alma mekanizması kullanır.

Windows kapsayıcı başlangıç süreye emin olmak için aşağıdakilerden birini kullanmak **en son üç** aşağıdaki sürümlerini **iki görüntü** temel görüntü olarak:

* [Windows Server 2016] [ docker-hub-windows-core] (yalnızca LTS)
* [Windows Server 2016 Nano Server][docker-hub-windows-nano]

## <a name="resource-not-available-error"></a>Kaynak kullanılamaz hatası

Azure'da yük bölgesel kaynak değişen nedeniyle, bir kapsayıcı örnek dağıtmak çalışılırken aşağıdaki hata alabilirsiniz:

`The requested resource with 'x' CPU and 'y.z' GB memory is not available in the location 'example region' at this moment. Please retry with a different resource request or in another location.`

Bu hata dağıtmak çalıştığınız bölgede ağır yükü nedeniyle, o anda kapsayıcısı için belirtilen kaynaklar ayrılamıyor gösterir. Sorunu gidermek için bir veya daha fazla aşağıdaki azaltma adımlarını kullanın.

* Kapsayıcı dağıtım ayarlarınızı kalan içinde tanımlanan parametrelerin içinde doğrulayın [kotalar ve Azure kapsayıcı örnekleri için bölge kullanılabilirliği](container-instances-quotas.md#region-availability)
* Kapsayıcı daha düşük CPU ve bellek ayarlarını belirtin
* Farklı bir Azure bölgesine dağıtma
* Daha sonraki bir zamanda dağıtma

<!-- LINKS - External -->
[docker-multi-stage-builds]: https://docs.docker.com/engine/userguide/eng-image/multistage-build/
[docker-hub-windows-core]: https://hub.docker.com/r/microsoft/windowsservercore/
[docker-hub-windows-nano]: https://hub.docker.com/r/microsoft/nanoserver/

<!-- LINKS - Internal -->
[az-container-attach]: /cli/azure/container#az_container_attach
[az-container-logs]: /cli/azure/container#az_container_logs
[az-container-show]: /cli/azure/container#az_container_show