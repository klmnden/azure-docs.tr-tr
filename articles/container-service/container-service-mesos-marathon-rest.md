<properties
   pageTitle="REST API’si aracılığıyla Azure Kapsayıcı Hizmeti kapsayıcı yönetimi | Microsoft Azure"
   description="Marathon REST API’sini kullanarak Azure Kapsayıcı Hizmeti Mesos kümesine kapsayıcıları dağıtın."
   services="container-service"
   documentationCenter=""
   authors="neilpeterson"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, Containers, Micro-services, Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="02/16/2016"
   ms.author="nepeters"/>

# REST API’si aracılığıyla kapsayıcı yönetimi

DC/OS, temel donanımı özetlerken, kümelenmiş iş yüklerini dağıtmak ve ölçeklendirmek için ortam sağlar. DC/OS’nin en üstünde, hesaplama iş yüklerini zamanlamayı ve yürütmeyi yöneten bir çerçeve vardır.

Çerçeveler çok sayıda yaygın iş yükü için kullanılabilir, ancak bu belgede Marathon ile kapsayıcı dağıtımlarını nasıl oluşturacağınız ve ölçeklendireceğiniz açıklanmıştır. Bu örneklerin üzerinden geçmeden önce, Azure Kapsayıcı Hizmeti’nde yapılandırılan bir DC/OS kümeniz olması gerekir. Bu kümeye uzaktan bağlantınız olması da gerekir. Bu öğeler hakkında daha fazla bilgi için, aşağıdaki makalelere bakın:

- [Azure Kapsayıcı Hizmeti kümesini dağıtma](container-service-deployment.md)
- [Azure Kapsayıcı Hizmeti kümesine bağlanma](container-service-connect.md)

Azure Kapsayıcı Hizmeti kümesine bağlandıktan sonra, DC/OS’ye ve ilgili REST API’lerine http://localhost:local-port aracılığıyla erişebilirsiniz. Bu belgedeki örneklerde, bağlantı noktası 80 üzerinde tünel oluşturulmaktadır. Örneğin, Marathon uç noktasına `http://localhost/marathon/v2/` üzerinde erişilebilir. Çeşitli API'ler hakkında daha fazla bilgi için, [Marathon API’si](https://mesosphere.github.io/marathon/docs/rest-api.html) ve [Chronos API’si](https://mesos.github.io/chronos/docs/api.html) için Mesosphere belgelerine ve [Mesos Scheduler API’si](http://mesos.apache.org/documentation/latest/scheduler-http-api/) için Apache belgelerine bakın.

## DC/OS’den ve Marathon’dan bilgi toplama

DC/OS kümesine kapsayıcıları dağıtmadan önce, adları ve DC/OS aracılarının adları ve geçerli durumu gibi, DC/OS kümesi hakkında bazı bilgileri toplayın. Bunu yapmak için, DC/OS REST API’sinin `master/slaves` uç noktasını sorgulayın.  Her şey yolunda giderse, DC/OS aracıları ve her biri için çeşitli özellikler listesini görürsünüz.

```bash
curl http://localhost/mesos/master/slaves
```

Şimdi, DC/OS kümesine geçerli uygulama dağıtımlarını denetlemek için Marathon `/apps` uç noktasını kullanın. Bu yeni bir küme ise, uygulamalar için boş bir dizi görürsünüz.

```
curl localhost/marathon/v2/apps

{"apps":[]}
```

## Docker biçimli kapsayıcı dağıtma

Docker biçimli kapsayıcıları Marathon aracılığıyla, hedeflenen dağıtımı açıklayan bir JSON dosyası kullanarak dağıtın. Aşağıdaki örnek, Nginx kapsayıcısı DC/OS aracısı bağlama bağlantı noktası 80’i kapsayıcının 80 numaralı bağlantı noktasına dağıtır. Ayrıca, “acceptedResourceRoles” özelliğinin “slave_public” olarak ayarlandığına dikkat edin. Bu, kapsayıcıyı genel kullanıma yönelik aracı ölçek grubundaki aracıya dağıtır.

```json
{
  "id": "nginx",
  "cpus": 0.1,
  "mem": 16.0,
  "instances": 1,
    "acceptedResourceRoles": [
    "slave_public"
  ],
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        { "containerPort": 80, "hostPort": 80, "servicePort": 9000, "protocol": "tcp" }
      ]
    }
  }
}
```

Docker biçimli bir kapsayıcıyı dağıtmak için, kendi JSON dosyanızı oluşturun veya [Azure Kapsayıcı Hizmeti gösteriminde](https://raw.githubusercontent.com/rgardler/AzureDevTestDeploy/master/marathon/marathon.json) verilen örneği kullanın. Bunu erişilebilir bir yerde saklayın. Ardından, kapsayıcıyı dağıtmak için aşağıdaki komutu çalıştırın. JSON dosyasının adını belirtin.

```
# deploy container

curl -X POST http://localhost/marathon/v2/apps -d @marathon.json -H "Content-type: application/json"
```

Çıktı aşağıdakine benzerdir.

```json
{"version":"2015-11-20T18:59:00.494Z","deploymentId":"b12f8a73-f56a-4eb1-9375-4ac026d6cdec"}
```

Şimdi, uygulamalar için Marathon’u sorguladığınızda, bu yeni uygulama çıktıda gösterir.

```
curl localhost/marathon/v2/apps
```

## Kapsayıcılarınızı ölçeklendirme

Marathon API’sini uygulama dağıtımlarının ölçeğini genişletmek ve ölçeğini daraltmak için de kullanabilirsiniz. Önceki örnekte, uygulamanın bir örneğini dağıttınız. Şimdi bunun ölçeğini uygulamanın üç örneğine genişletelim. Bunu yapmak için, aşağıdaki JSON metnini kullanarak bir JSON dosyası oluşturun ve erişilebilir bir konumda depolayın.

```json
{ "instances": 3 }
```

Uygulamanın ölçeğini genişletmek için aşağıdaki komutu çalıştırın.

>[AZURE.NOTE] URI, http://localhost/marathon/v2/apps/ şeklinde olur ve ardından ölçeklendirilecek uygulamanın kimliği gelir. Burada sağlanan Nginx örneğini kullanıyorsanız, URI http://localhost/marathon/v2/apps/nginx şeklinde olacaktır.

```json
# scale container

curl http://localhost/marathon/v2/apps/nginx -H "Content-type: application/json" -X PUT -d @scale.json
```

Son olarak, uygulamalar için Marathon uç noktasını sorgulayın. Artık üç adet Nginx kapsayıcısı olduğunu görürsünüz.

```
curl localhost/marathon/v2/apps
```

## Bu uygulama için PowerShell kullanın: Marathon REST API’sinin PowerShell ile etkileşimi

Bir Windows sisteminde PowerShell komutlarını kullanarak aynı eylemleri gerçekleştirebilirsiniz.

Aracı adları ve aracı durumu gibi, DC/OS kümesi hakkında bilgi toplamak için aşağıdaki komutu çalıştırın:

```powershell
Invoke-WebRequest -Uri http://localhost/mesos/master/slaves
```

Docker biçimli kapsayıcıları Marathon aracılığıyla, hedeflenen dağıtımı açıklayan bir JSON dosyası kullanarak dağıtın. Aşağıdaki örnek, Nginx kapsayıcısı DC/OS aracısı bağlama bağlantı noktası 80’i kapsayıcının 80 numaralı bağlantı noktasına dağıtır.

```json
{
  "id": "nginx",
  "cpus": 0.1,
  "mem": 16.0,
  "instances": 1,
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        { "containerPort": 80, "hostPort": 80, "servicePort": 9000, "protocol": "tcp" }
      ]
    }
  }
}
```

Kendi JSON dosyanızı oluşturun veya [Azure Kapsayıcı Hizmeti gösteriminde](https://raw.githubusercontent.com/rgardler/AzureDevTestDeploy/master/marathon/marathon.json) verilen örneği kullanın. Bunu erişilebilir bir yerde saklayın. Ardından, kapsayıcıyı dağıtmak için aşağıdaki komutu çalıştırın. JSON dosyasının adını belirtin.

```powershell
# deploy container

Invoke-WebRequest -Method Post -Uri http://localhost/marathon/v2/apps -ContentType application/json -InFile 'c:\marathon.json'
```

Marathon API’sini uygulama dağıtımlarının ölçeğini genişletmek ve ölçeğini daraltmak için de kullanabilirsiniz. Önceki örnekte, uygulamanın bir örneğini dağıttınız. Şimdi bunun ölçeğini uygulamanın üç örneğine genişletelim. Bunu yapmak için, aşağıdaki JSON metnini kullanarak bir JSON dosyası oluşturun ve erişilebilir bir konumda depolayın.

```json
{ "instances": 3 }
```

Uygulamanın ölçeğini genişletmek için aşağıdaki komutu çalıştırın.

> [AZURE.NOTE] URI, http://localhost/marathon/v2/apps/ şeklinde olur ve ardından ölçeklendirilecek uygulamanın kimliği gelir. Burada sağlanan Nginx örneğini kullanıyorsanız, URI http://localhost/marathon/v2/apps/nginx şeklinde olacaktır.

```powershell
# scale container

Invoke-WebRequest -Method Put -Uri http://localhost/marathon/v2/apps/nginx -ContentType application/json -InFile 'c:\scale.json'
```

## Sonraki adımlar

[Mesos HTTP uç noktaları hakkında daha fazla bilgi edinin]( http://mesos.apache.org/documentation/latest/endpoints/).
[Marathon REST API’si hakkında daha fazla bilgi edinin]( https://mesosphere.github.io/marathon/docs/rest-api.html).






<!----HONumber=Jun16_HO2-->


