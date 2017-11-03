---
title: "Azure kapsayıcı örnekleri sorunlarını giderme"
description: "Azure kapsayıcı örnekleri ile ilgili sorunları giderme hakkında bilgi edinin"
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/31/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: ff6da0ce95d0405714602c3872da34a2bff344d3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshoot-deployment-issues-with-azure-container-instances"></a>Azure kapsayıcı örnekleri dağıtım sorunlarını giderme

Bu makalede kapsayıcıları Azure kapsayıcı örnekleri dağıtırken ilgili sorunları gidermek nasıl gösterilmektedir. Ayrıca bazı içine çalışabilir yaygın sorunları açıklar.

## <a name="getting-diagnostic-events"></a>Tanılama Olayları Alma

Bir kapsayıcı içindeki uygulama kodunuzdan günlükleri görüntülemek için kullanabileceğiniz [az kapsayıcı günlükleri](/cli/azure/container#logs) komutu. Ancak, kapsayıcı başarıyla dağıtma, Azure kapsayıcı örnekleri kaynak sağlayıcısı tarafından sağlanan tanı bilgilerini gözden geçirmek gerekebilir. Kapsayıcı olayları görüntülemek için aşağıdaki komutu çalıştırın:

```azurecli-interactive
az container show -n mycontainername -g myresourcegroup
```

Çıktı dağıtım olayları yanı sıra, kapsayıcı çekirdek özelliklerini içerir:

```bash
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
        "firstTimestamp": "2017-08-03T22:12:52+00:00",
        "lastTimestamp": "2017-08-03T22:12:52+00:00",
        "message": "Pulling: pulling image \"microsoft/aci-helloworld\"",
        "type": "Normal"
      },
      {
        "count": 1,
        "firstTimestamp": "2017-08-03T22:12:55+00:00",
        "lastTimestamp": "2017-08-03T22:12:55+00:00",
        "message": "Pulled: Successfully pulled image \"microsoft/aci-helloworld\"",
        "type": "Normal"
      },
      {
        "count": 1,
        "firstTimestamp": "2017-08-03T22:12:55+00:00",
        "lastTimestamp": "2017-08-03T22:12:55+00:00",
        "message": "Created: Created container with id 61602059d6c31529c27609ef4ec0c858b0a96150177fa045cf944d7cf8fbab69",
        "type": "Normal"
      },
      {
        "count": 1,
        "firstTimestamp": "2017-08-03T22:12:55+00:00",
        "lastTimestamp": "2017-08-03T22:12:55+00:00",
        "message": "Started: Started container with id 61602059d6c31529c27609ef4ec0c858b0a96150177fa045cf944d7cf8fbab69",
        "type": "Normal"
      }
    ],
    "name": "helloworld",
      "ports": [
        {
          "port": 80
        }
      ],
    ...
  ]
}
```

## <a name="common-deployment-issues"></a>Genel dağıtım sorunları

Bu hesaba hataların çoğu dağıtımda bazı yaygın sorunlar vardır.

### <a name="unable-to-pull-image"></a>Çekme görüntü oluşturulamıyor

Azure kapsayıcı örnek görüntünüzü başlangıçta çekmesini kaydedemediği sonunda başarısız önce belirli bir süre için yeniden dener. Görüntü çekilen, olayları aşağıdaki gibi gösterilir:

```bash
"events": [
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:19:31+00:00",
    "lastTimestamp": "2017-08-03T22:19:31+00:00",
    "message": "Pulling: pulling image \"microsoft/aci-hellowrld\"",
    "type": "Normal"
  },
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:19:32+00:00",
    "lastTimestamp": "2017-08-03T22:19:32+00:00",
    "message": "Failed: Failed to pull image \"microsoft/aci-hellowrld\": rpc error: code 2 desc Error: image microsoft/aci-hellowrld:latest not found",
    "type": "Warning"
  },
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:19:33+00:00",
    "lastTimestamp": "2017-08-03T22:19:33+00:00",
    "message": "BackOff: Back-off pulling image \"microsoft/aci-hellowrld\"",
    "type": "Normal"
  }
]
```

Çözümlemek için kapsayıcıyı silin ve görüntü adı doğru yazdığınızı ödeyen Kapat dikkat dağıtımınızı yeniden deneyin.

### <a name="container-continually-exits-and-restarts"></a>Kapsayıcı sürekli olarak çıkar ve yeniden başlatır

Şu anda, Azure kapsayıcı örnekleri yalnızca uzun süre çalışan hizmetleri destekler. Kapsayıcı tamamlama ve çıkar çalıştırıyorsa, otomatik olarak yeniden başlatılır ve yeniden çalıştırır. Bu durumda, olayları olanlar aşağıdaki gibi gösterilir. Kapsayıcı sorunsuz başlatıldıktan sonra hızlı bir şekilde yeniden unutmayın. Kapsayıcı örnekleri API içeren bir `retryCount` belirli bir kapsayıcıda kaç kez gösteren özelliği başlatıldıktan.

```bash
"events": [
  {
    "count": 5,
    "firstTimestamp": "2017-08-03T22:21:55+00:00",
    "lastTimestamp": "2017-08-03T22:23:22+00:00",
    "message": "Pulling: pulling image \"alpine\"",
    "type": "Normal"
  },
  {
    "count": 5,
    "firstTimestamp": "2017-08-03T22:21:57+00:00",
    "lastTimestamp": "2017-08-03T22:23:23+00:00",
    "message": "Pulled: Successfully pulled image \"alpine\"",
    "type": "Normal"
  },
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:21:57+00:00",
    "lastTimestamp": "2017-08-03T22:21:57+00:00",
    "message": "Created: Created container with id ad2bf9bc51761c5f935260b4bab53b164d52d9cbc045b16afcb26fb4d14d0a70",
    "type": "Normal"
  },
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:21:57+00:00",
    "lastTimestamp": "2017-08-03T22:21:57+00:00",
    "message": "Started: Started container with id ad2bf9bc51761c5f935260b4bab53b164d52d9cbc045b16afcb26fb4d14d0a70",
    "type": "Normal"
  },
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:21:58+00:00",
    "lastTimestamp": "2017-08-03T22:21:58+00:00",
    "message": "Created: Created container with id 7687b9bd15dc01731fa66fc45f6f0241495600602dd03841e559453245e7f70b",
    "type": "Normal"
  },
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:21:58+00:00",
    "lastTimestamp": "2017-08-03T22:21:58+00:00",
    "message": "Started: Started container with id 7687b9bd15dc01731fa66fc45f6f0241495600602dd03841e559453245e7f70b",
    "type": "Normal"
  },
  {
    "count": 13,
    "firstTimestamp": "2017-08-03T22:21:59+00:00",
    "lastTimestamp": "2017-08-03T22:24:36+00:00",
    "message": "BackOff: Back-off restarting failed container",
    "type": "Warning"
  },
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:22:13+00:00",
    "lastTimestamp": "2017-08-03T22:22:13+00:00",
    "message": "Created: Created container with id 72e347e891290e238135e4a6b3078748ca25a1275dbbff30d8d214f026d89220",
    "type": "Normal"
  },
  ...
```

> [!NOTE]
> Linux dağıtımları için çoğu kapsayıcı görüntüleri bash gibi bir kabuk varsayılan komut olarak ayarlayın. Kendi başına bir kabuk uzun süre çalışan hizmet olmadığından, bu kapsayıcılar hemen çıkmak ve yeniden başlatma döngüye ayrılır.

### <a name="container-takes-a-long-time-to-start"></a>Kapsayıcı başlatmak için çok uzun sürüyor

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

Görüntü boyutları küçük tutmak için anahtarı son görüntünüzü çalışma zamanında gerekli olmayan bir şey içermediğinden emin olmaktır. Yapmanın bir yolu bu olan [çok aşama derlemeleri](https://docs.docker.com/engine/userguide/eng-image/multistage-build/). Çok aşama yapma, yalnızca uygulamanız için gereksinim duyduğunuz yapıları son görüntüsünü içeren ve herhangi bir ek içerik sağlamak kolay derleme zamanında gerekli oluşturur.

Kapsayıcının başlangıç zamanında görüntü çekme etkisini azaltmak için diğer Azure kapsayıcı örneği kullanmak istiyorsanız, aynı bölgede Azure kapsayıcı kayıt defterini kullanarak kapsayıcı görüntüsü barındırmak için bir yoludur. Bu, kapsayıcı görüntü seyahat gereken ağ yolu önemli ölçüde karşıdan yükleme süresini kısaltmak kısaltır.

### <a name="resource-not-available-error"></a>Kaynak kullanılamaz hatası

Azure'da yük bölgesel kaynak değişen nedeniyle, bir kapsayıcı örnek dağıtmak çalışılırken aşağıdaki hata alabilirsiniz:

`The requested resource with 'x' CPU and 'y.z' GB memory is not available in the location 'example region' at this moment. Please retry with a different resource request or in another location.`

Bu hata dağıtmak çalıştığınız bölgede ağır yükü nedeniyle, o anda kapsayıcısı için belirtilen kaynaklar ayrılamıyor gösterir. Bir veya daha fazlasını kullanın, sorunu gidermek için aşağıdaki azaltma adımları.

* Kapsayıcı dağıtım ayarlarınızı kalan içinde tanımlanan parametrelerin içinde doğrulayın [Azure kapsayıcı örnekleri için bölge kullanılabilirliği](container-instances-region-availability.md)
* Kapsayıcı daha düşük CPU ve bellek ayarlarını belirtin
* Farklı bir Azure bölgesine dağıtma
* Daha sonraki bir zamanda dağıtma
