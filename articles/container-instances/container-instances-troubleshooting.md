---
title: "Azure kapsayıcı örnekleri sorunlarını giderme"
description: "Azure kapsayıcı örnekleri ile ilgili sorunları giderme hakkında bilgi edinin"
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: article
ms.date: 01/02/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 561729e5e495500222ccec5b4b536a3152cb25e3
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="troubleshoot-deployment-issues-with-azure-container-instances"></a>Azure kapsayıcı örnekleri dağıtım sorunlarını giderme

Bu makalede kapsayıcıları Azure kapsayıcı örnekleri dağıtırken ilgili sorunları gidermek nasıl gösterilmektedir. Ayrıca bazı içine çalışabilir yaygın sorunları açıklar.

## <a name="get-diagnostic-events"></a>Tanılama Olayları Al

Bir kapsayıcı içindeki uygulama kodunuzdan günlükleri görüntülemek için kullanabileceğiniz [az kapsayıcı günlükleri] [ az-container-logs] komutu. Ancak, kapsayıcı başarıyla dağıtma, Azure kapsayıcı örnekleri kaynak sağlayıcısı tarafından sağlanan tanı bilgilerini gözden geçirmek gerekebilir. Kapsayıcı için olayları görüntülemek için çalıştırın [az kapsayıcı Göster] [ az-container-show] komutu:

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

Bu hesaba hataların çoğu dağıtımda bazı yaygın sorunlar vardır.

## <a name="image-version-not-supported"></a>Görüntü sürümü desteklenmiyor

Görüntüyü Azure kapsayıcı örnekler destekleyemiyor belirtilirse, formun bir hata döndürülür `ImageVersionNotSupported`. Hata değeri gösterecektir `The version of image '{0}' is not supported.`. Bu hata Windows 1709 görüntüleri kullanmak azaltmak için şu anda bir LTS Windows görüntüsüne uygular. Windows 1709 görüntüler için destek işleniyor.

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

Kapsayıcı başlatma, ancak sonuç uzun süren, başarılı, kapsayıcı görüntünüzü boyutta bakarak başlatın. Azure kapsayıcı örnekleri kapsayıcı görüntünüzü isteğe bağlı olarak çeker çünkü karşılaştığınız başlangıç zamanını boyutuna doğrudan ilişkilidir.

Docker CLI kullanarak kapsayıcı görüntünüzün boyutunu görüntüleyebilirsiniz:

```bash
docker images
```

Çıktı:

```bash
REPOSITORY                             TAG                 IMAGE ID            CREATED             SIZE
microsoft/aci-helloworld               latest              7f78509b568e        13 days ago         68.1MB
```

Görüntü boyutları küçük tutmak için anahtarı son görüntünüzü çalışma zamanında gerekli olmayan bir şey içermediğinden emin olmaktır. Yapmanın bir yolu bu olan [çok aşama derlemeleri][docker-multi-stage-builds]. Çok aşama yapma, yalnızca uygulamanız için gereksinim duyduğunuz yapıları son görüntüsünü içeren ve herhangi bir ek içerik sağlamak kolay derleme zamanında gerekli oluşturur.

Kapsayıcının başlangıç zamanında görüntü çekme etkisini azaltmak için diğer Azure kapsayıcı örneği kullanmak istiyorsanız, aynı bölgede Azure kapsayıcı kayıt defterini kullanarak kapsayıcı görüntüsü barındırmak için bir yoludur. Bu, kapsayıcı görüntü seyahat gereken ağ yolu önemli ölçüde karşıdan yükleme süresini kısaltmak kısaltır.

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

<!-- LINKS - Internal -->
[az-container-logs]: /cli/azure/container#az_container_logs
[az-container-show]: /cli/azure/container#az_container_show