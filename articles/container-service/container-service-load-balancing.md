<properties
   pageTitle="Azure Kapsayıcı Hizmeti kümesi yük dengelemesi | Microsoft Azure"
   description="Azure Kapsayıcı Hizmeti kümesi yük dengelemesi."
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Kapsayıcılar, Mikro hizmetler, DC/OS, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/11/2016"
   ms.author="rogardle"/>

# Azure Kapsayıcı Hizmeti kümesi yük dengelemesi

Bu makalede bir DC/OS yönetilen Azure Container Service üzerinde web ön ucu oluşturulacaktır. Ayrıca uygulama ölçeğini artırmanız için bir Marathon-LB yapılandırılacaktır.

## Ön koşullar

[DC/OS orchestrator türüyle Azure Container Service örneğini dağıtın](container-service-deployment.md) ve [istemcinizin kümenize bağlanabildiğinden emin olun](container-service-connect.md). 

## Yük dengeleme

Oluşturulacak Kapsayıcı Hizmeti kümesinde iki yük dengeleme katmanı mevcuttur: 

  1. Azure Load Balancer iki ortak giriş noktası sağlar (kullanıcıların bulacağı giriş noktaları). Bu özellik Azure Container Service tarafından otomatik olarak sağlanır ve varsayılan olarak 80, 443 ve 8080 bağlantı noktalarını ortaya çıkaracak şekilde yapılandırılır.
  2. Marathon Yük Dengeleyici (marathon-lb) gelen istekleri bu isteklere hizmet eden kapsayıcı örneklerine yönlendirir. Web hizmetini sağlayan kapsayıcılar ölçeklendirilirken marathon-lb dinamik olarak uyarlanır. Bu yük dengeleyici, Kapsayıcı Hizmetinizde varsayılan olarak sağlanmaz, ancak yüklenmesi çok kolaydır.

## Marathon Yük Dengeleyici

Marathon Yük Dengeleyici dağıttığınız kapsayıcılara göre kendini dinamik olarak yeniden yapılandırır. Bu ayrıca kapsayıcı ya da aracı kaybına karşı esnektir; bu meydana gelirse, Apache Mesos başka yerde kapsayıcıyı yeniden başlatır ve marathon-lB buna uyarlanır.

Marathon Yük Dengeleyiciyi yüklemek için DC/OS web kullanıcı arabirimini veya komut satırını kullanabilirsiniz.

### DC/OS web kullanıcı arabirimi kullanarak Marathon-LB yükleme

  1. 'Universe' öğesine tıklayın
  2. 'Marathon LB' araması yapın
  3. Yükle'ye tıklayın

![DC/OS Web Arabirimi üzerinden marathon-lb yükleme](./media/dcos/marathon-lb-install.png)

### DC/OS CLI’si kullanarak Marathon-LB yükleme

DC/OS CLI’sini yükleyip kümenize bağlanabildiğinizden emin olduktan sonra istemci makineden aşağıdaki komutu çalıştırın:

```bash
dcos package install marathon-lb
```

## Yük Dengeli Web Uygulaması Dağıtma

Artık marathon-lb paketine sahip olduğumuza göre, aşağıdaki yapılandırmayı kullanarak basit bir web sunucusu dağıtabiliriz:

```json
{
  "id": "web",
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "yeasy/simple-web",
      "network": "BRIDGE",
      "portMappings": [
        { "hostPort": 0, "containerPort": 80, "servicePort": 10000 }
      ],
      "forcePullImage":true
    }
  },
  "instances": 3,
  "cpus": 0.1,
  "mem": 65,
  "healthChecks": [{
      "protocol": "HTTP",
      "path": "/",
      "portIndex": 0,
      "timeoutSeconds": 10,
      "gracePeriodSeconds": 10,
      "intervalSeconds": 2,
      "maxConsecutiveFailures": 10
  }],
  "labels":{
    "HAPROXY_GROUP":"external",
    "HAPROXY_0_VHOST":"YOUR FQDN",
    "HAPROXY_0_MODE":"http"
  }
}

```

  * `HAProxy_0_VHOST` değerini aracılarınız için yük dengeleyici FQDN'sine ayarlayın. Bu değer `<acsName>agents.<region>.cloudapp.azure.com` biçimindedir. Örneğin, `West US` bölgesinde `myacs` adıyla bir Kapsayıcı Hizmeti kümesi oluşturursanız FQDN şu şekilde olur: `myacsagents.westus.cloudapp.azure.com`. Bunu, [Azure portal](https://portal.azure.com)’da kapsayıcı hizmetiniz için oluşturduğunuz kaynak gurubunda kaynaklara bakarken adında “agent” olan yük dengeleyiciyi arayarak bulabilirsiniz.
  * servicePort değerini bağlantı noktası >= 10.000 olacak şekilde ayarlayın. Bunu yapmanız bu kapsayıcıda çalıştırılan hizmeti tanımlar; marathon-lb bunu dengelemesi gereken hizmetleri tanımlamak için kullanır.
  * `HAPROXY_GROUP` etiketini "dış" olarak ayarlayın.
  * `hostPort` değerini 0 olarak ayarlayın. Bunun yapılması Marathon’un kullanılabilir bir bağlantı noktasını rastgele ayıracağı anlamına gelir.
  * `instances` değerini oluşturmak istediğiniz örnek sayısına ayarlayın. Bunların ölçeğini daha sonra dilediğiniz zaman artırıp azaltabilirsiniz.

### DC/OS web kullanıcı arabirimi kullanarak dağıtma

  1. http://localhost/marathon adresindeki Marathon sayfasını ziyaret edin ([SSH tünelinizi](container-service-connect.md) ayarladıktan sonra `Create Appliction`
  2. `New Application` iletişim kutusunda sağ üst köşedeki `JSON Mode` öğesine tıklayın
  3. Yukarıdaki JSON’u düzenleyiciye yapıştırın
  4. Şuna tıklayın: `Create Appliction`

### DC/OS CLI’si kullanarak dağıtma

Bu uygulamayı DC/OS CLI ile dağıtmak için yukarıdaki JSON’u `hello-web.json` adlı bir dosyaya kopyalayın ve şu komutu çalıştırın:

```bash
dcos marathon app add hello-web.json
```

## Azure Load Balancer

Varsayılan olarak, Azure Load Balancer 80, 8080 ve 443 bağlantı noktalarını ortaya çıkarır. Bu bağlantı noktalarından birini kullanıyorsanız (yukarıdaki örnekte olduğu gibi) bir şey yapmanıza gerek yoktur. Aracı yük dengeleyicinizin FQDN’sini bulabilmeniz gerekir ve her yenilediğinizde üç web sunucunuzdan her birini bir kere isabet ettirirsiniz. Ancak, farklı bir bağlantı noktası kullanıyorsanız kullandığınız bağlantı noktası için yük dengeleyiciye hepsini bir kez deneme kuralı ve bir araştırma eklemeniz gerekir. Bunu [Azure CLI](../xplat-cli-azure-resource-manager.md)’dan `azure lb rule create` ve `azure lb probe create` komutlarıyla yapabilirsiniz. Bunu ayrıca Azure Portal'ı kullanarak yapabilirsiniz.


## İlave senaryolar

Farklı hizmetleri kullanıma sunmak için farklı etki alanlarını kullandığınız bir senaryonuz olabilir. Örneğin:

mydomain1.com -> Azure LB:80 -> marathon-lb:10001 -> mycontainer1:33292  
mydomain2.com -> Azure LB:80 -> marathon-lb:10002 -> mycontainer2:22321

Bunu başarmak için, etki alanlarını belirli marathon lb yollarıyla ilişkilendirmek için bir yol sunan [sanal konaklar](https://mesosphere.com/blog/2015/12/04/dcos-marathon-lb/)’a bakın.

Alternatif olarak, farklı bağlantı noktalarını kullanıma sunabilir ve bunları marathon-lb arkasında doğru hizmete yeniden eşleyebilirsiniz. Örneğin:

Azure lb:80 -> marathon-lb:10001 -> mycontainer:233423  
Azure lb:8080 -> marathon-lb:1002 -> mycontainer2:33432


## Sonraki adımlar

[marathon-lb](https://dcos.io/docs/1.7/usage/service-discovery/marathon-lb/) hakkında daha fazla bilgi için DC/OS belgelerine bakın.



<!--HONumber=Aug16_HO1-->


