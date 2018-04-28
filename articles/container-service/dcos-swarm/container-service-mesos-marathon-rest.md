---
title: Azure DC/OS kümesi Marathon REST API ile yönetme
description: Marathon REST API kullanarak Azure kapsayıcı hizmeti DC/OS kümesine kapsayıcıları dağıtın.
services: container-service
author: dlepow
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 04/04/2017
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: edd66be25bf2571a7315372898300476fec101ca
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="dcos-container-management-through-the-marathon-rest-api"></a>DC/OS Marathon REST API'si aracılığıyla kapsayıcı Yönetimi

DC/OS, temel donanımı özetlerken, kümelenmiş iş yüklerini dağıtmak ve ölçeklendirmek için ortam sağlar. DC/OS’nin en üstünde, hesaplama iş yüklerini zamanlamayı ve yürütmeyi yöneten bir çerçeve vardır. Çerçeveler çok sayıda yaygın iş yükü için kullanılabilir, ancak bu belgenin oluşturma ve Marathon REST API'sini kullanarak kapsayıcı dağıtımlarını ölçeklendirme başlamanızı sağlar. 

## <a name="prerequisites"></a>Önkoşullar

Bu örneklerin üzerinden geçmeden önce, Azure Kapsayıcı Hizmeti’nde yapılandırılan bir DC/OS kümeniz olması gerekir. Bu kümeye uzaktan bağlantınız olması da gerekir. Bu öğeler hakkında daha fazla bilgi için, aşağıdaki makalelere bakın:

* [Azure Container Service kümesini dağıtma](container-service-deployment.md)
* [Azure Container Service kümesine bağlanma](../container-service-connect.md)

## <a name="access-the-dcos-apis"></a>DC/OS API'lere erişim
Azure kapsayıcı hizmeti kümesine bağlandıktan sonra DC/OS ve ilgili REST API'leri aracılığıyla erişebilirsiniz http://localhost:local-port. Bu belgedeki örneklerde, bağlantı noktası 80 üzerinde tünel oluşturulmaktadır. Örneğin, Marathon uç noktaları URI'ler erişilebildiğini itibaren `http://localhost/marathon/v2/`. 

Çeşitli API'ler hakkında daha fazla bilgi için, [Marathon API’si](https://mesosphere.github.io/marathon/docs/rest-api.html) ve [Chronos API’si](https://mesos.github.io/chronos/docs/api.html) için Mesosphere belgelerine ve [Mesos Scheduler API’si](http://mesos.apache.org/documentation/latest/scheduler-http-api/) için Apache belgelerine bakın.

## <a name="gather-information-from-dcos-and-marathon"></a>DC/OS’den ve Marathon’dan bilgi toplama
DC/OS kümesine kapsayıcıları dağıtmadan önce adları ve DC/OS aracıların durumu gibi DC/OS kümesi hakkında bazı bilgileri toplayın. Bunu yapmak için, DC/OS REST API’sinin `master/slaves` uç noktasını sorgulayın.  Her şey yolunda giderse, sorgu DC/OS aracıları ve her biri için çeşitli özellikler listesini döndürür.

```bash
curl http://localhost/mesos/master/slaves
```

Şimdi, DC/OS kümesine geçerli uygulama dağıtımlarını denetlemek için Marathon `/apps` uç noktasını kullanın. Bu yeni bir küme ise, uygulamalar için boş bir dizi görürsünüz.

```bash
curl localhost/marathon/v2/apps

{"apps":[]}
```

## <a name="deploy-a-docker-formatted-container"></a>Docker biçimli kapsayıcı dağıtma
Docker biçimli kapsayıcıları Marathon REST API'si aracılığıyla hedeflenen dağıtımı açıklayan bir JSON dosyası kullanarak dağıtın. Aşağıdaki örnek bir Nginx kapsayıcısı kümedeki özel bir aracıya dağıtır. 

```json
{
  "id": "nginx",
  "cpus": 0.1,
  "mem": 32.0,
  "instances": 1,
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        { "containerPort": 80, "servicePort": 9000, "protocol": "tcp" }
      ]
    }
  }
}
```

Docker biçimli bir kapsayıcıyı dağıtmak için JSON dosyasının erişilebilir bir yerde saklayın. Ardından, kapsayıcıyı dağıtmak için aşağıdaki komutu çalıştırın. JSON dosyasının adını belirtin (`marathon.json` Bu örnekte).

```bash
curl -X POST http://localhost/marathon/v2/apps -d @marathon.json -H "Content-type: application/json"
```

Çıkış aşağıdakine benzer:

```json
{"version":"2015-11-20T18:59:00.494Z","deploymentId":"b12f8a73-f56a-4eb1-9375-4ac026d6cdec"}
```

Şimdi, uygulamalar için Marathon’u sorguladığınızda, bu yeni uygulama çıktıda görünür.

```bash
curl localhost/marathon/v2/apps
```

## <a name="reach-the-container"></a>Kapsayıcı ulaşmak

Özel aracıları kümedeki birinde Nginx bir kapsayıcıda çalışıp çalışmadığını doğrulayabilirsiniz. Kapsayıcı çalıştığı bağlantı noktası ve ana bilgisayar bulmak için çalışan görevler için Marathon sorgu: 

```bash
curl localhost/marathon/v2/tasks
```

Değerini bulur `host` çıktıda (benzer şekilde bir IP adresi `10.32.0.x`) ve değeri `ports`.


Şimdi küme yönetimi FQDN için bir SSH terminal bağlantısı (tünel bağlantısı değil) oluşturun. Bağlantı kurulduktan sonra doğru değerini değiştirerek aşağıdaki, istekte `host` ve `ports`:

```bash
curl http://host:ports
```

Nginx server çıktısı aşağıdakine benzer:

![Kapsayıcısından Nginx](./media/container-service-mesos-marathon-rest/nginx.png)




## <a name="scale-your-containers"></a>Kapsayıcılarınızı ölçeklendirme
Ölçeği genişletme veya ölçeklendirmek için Marathon API'si kullanabilirsiniz uygulama dağıtımlarda. Önceki örnekte, uygulamanın bir örneğini dağıttınız. Şimdi bunun ölçeğini uygulamanın üç örneğine genişletelim. Bunu yapmak için, aşağıdaki JSON metnini kullanarak bir JSON dosyası oluşturun ve erişilebilir bir konumda depolayın.

```json
{ "instances": 3 }
```

Tünel bağlantısından uygulamanın ölçeğini genişletmek için aşağıdaki komutu çalıştırın.

> [!NOTE]
> URI http://localhost/marathon/v2/apps/ ölçeklendirilecek uygulamanın kimliği tarafından izlenen. Burada, URI olacaktır sunulan Nginx örneğini kullanıyorsanız, http://localhost/marathon/v2/apps/nginx.
> 
> 

```bash
curl http://localhost/marathon/v2/apps/nginx -H "Content-type: application/json" -X PUT -d @scale.json
```

Son olarak, uygulamalar için Marathon uç noktasını sorgulayın. Artık üç adet Nginx kapsayıcısı olduğunu görürsünüz.

```bash
curl localhost/marathon/v2/apps
```

## <a name="equivalent-powershell-commands"></a>Eşdeğer PowerShell komutları
Bir Windows sisteminde PowerShell komutlarını kullanarak aynı eylemleri gerçekleştirebilirsiniz.

Aracı adları ve aracı durumu gibi DC/OS kümesi hakkında bilgi toplamak için aşağıdaki komutu çalıştırın:

```powershell
Invoke-WebRequest -Uri http://localhost/mesos/master/slaves
```

Docker biçimli kapsayıcıları Marathon aracılığıyla, hedeflenen dağıtımı açıklayan bir JSON dosyası kullanarak dağıtın. Aşağıdaki örnek, Nginx kapsayıcısı DC/OS aracısı bağlama bağlantı noktası 80'i kapsayıcının 80 numaralı bağlantı noktasına dağıtır.

```json
{
  "id": "nginx",
  "cpus": 0.1,
  "mem": 32.0,
  "instances": 1,
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        { "containerPort": 80, "servicePort": 9000, "protocol": "tcp" }
      ]
    }
  }
}
```

Docker biçimli bir kapsayıcıyı dağıtmak için JSON dosyasının erişilebilir bir yerde saklayın. Ardından, kapsayıcıyı dağıtmak için aşağıdaki komutu çalıştırın. JSON dosyasının yolunu belirtin (`marathon.json` Bu örnekte).

```powershell
Invoke-WebRequest -Method Post -Uri http://localhost/marathon/v2/apps -ContentType application/json -InFile 'c:\marathon.json'
```

Marathon API’sini uygulama dağıtımlarının ölçeğini genişletmek ve ölçeğini daraltmak için de kullanabilirsiniz. Önceki örnekte, uygulamanın bir örneğini dağıttınız. Şimdi bunun ölçeğini uygulamanın üç örneğine genişletelim. Bunu yapmak için, aşağıdaki JSON metnini kullanarak bir JSON dosyası oluşturun ve erişilebilir bir konumda depolayın.

```json
{ "instances": 3 }
```

Uygulamanın ölçeğini genişletmek için aşağıdaki komutu çalıştırın:

> [!NOTE]
> URI http://localhost/marathon/v2/apps/ ölçeklendirilecek uygulamanın kimliği tarafından izlenen. Burada sağlanan Nginx örneğini kullanıyorsanız, URI olacaktır http://localhost/marathon/v2/apps/nginx.
> 
> 

```powershell
Invoke-WebRequest -Method Put -Uri http://localhost/marathon/v2/apps/nginx -ContentType application/json -InFile 'c:\scale.json'
```

## <a name="next-steps"></a>Sonraki adımlar
* [Mesos HTTP uç noktaları hakkında daha fazla bilgi](http://mesos.apache.org/documentation/latest/endpoints/)
* [Marathon REST API'si hakkında daha fazla bilgi](https://mesosphere.github.io/marathon/docs/rest-api.html)

