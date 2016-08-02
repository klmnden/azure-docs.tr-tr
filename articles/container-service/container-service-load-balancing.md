<properties
   pageTitle="Azure Kapsayıcı Hizmeti kümesi yük dengelemesi | Microsoft Azure"
   description="Azure Kapsayıcı Hizmeti kümesi yük dengelemesi."
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Containers, Micro-services, DC/OS, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/18/2016"
   ms.author="rogardle"/>

# Azure Kapsayıcı Hizmeti kümesi yük dengelemesi

Bu makalede, Azure LB’nin arkasında hizmetleri sunmak için ölçeklendirilebilecek bir web ön ucu ayarlayacağız.


## Ön koşullar

[DCOS orchestrator türüyle Azure Kapsayıcı Hizmeti örneğini dağıtın](container-service-deployment.md), [istemcinizin kümenize](container-service-connect.md) bağlanabildiğinden ve [AZURE.INCLUDE [install the DC/OS CLI](../../includes/container-service-install-dcos-cli-include.md)] emin olun.


## Yük dengeleme

Bir Kapsayıcı Hizmeti kümesinde iki yük dengeleyici katmanı vardır: Ortak giriş noktaları için Azure LB (son kullanıcıların temas edeceği) ve gelen istekleri kapsayıcı örnekleri hizmet isteklerine yönlendiren temel marathon-lb. Hizmet sağlayan kapsayıcıları ölçeklendirirken, marathon-lb dinamik olarak uyarlanır.

## Marathon LB 

Marathon LB çözümü kendisini dağıttığınız kapsayıcılara göre dinamik olarak yeniden yapılandırır. Bu ayrıca kapsayıcı ya da aracı kaybına karşı esnektir; bu meydana gelirse, Mesos başka yerde kapsayıcıyı yeniden başlatır ve Marathon LB’yi yeniden yapılandırır. 

Marathon LB’yi yüklemek için, istemci makinenizde aşağıdaki komutu çalıştırın:

```bash
dcos package install marathon-lb 
``` 

Artık marathon-lb paketine sahip olduğumuza göre, aşağıdaki yapılandırmayı kullanarak basit bir web sunucusu dağıtabiliriz.


```json
{ 
  "id": "web", 
  "container": { 
    "type": "DOCKER", 
    "docker": { 
      "image": "tutum/hello-world", 
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

Bunun anahtar bölümleri şunlardır: 
  * HAProxy_0_VHOST değerini aracılarınız için yük dengeleyici FQDN'sine ayarlayın. Bu `<acsName>agents.<region>.cloudapp.azure.com` biçimindedir. Örneğin, `West US` bölgesinde `myacs` adıyla bir Kapsayıcı Hizmeti kümesi oluşturduysam, FQDN şu şekilde olur: `myacsagents.westus.cloudapp.azure.com`. Bunu, [Azure Portal](https://portal.azure.com)’da kapsayıcı hizmetiniz için oluşturduğunuz kaynak gurubunda kaynaklara bakarken adında “agent” olan yük dengeleyiciyi arayarak bulabilirsiniz.
  * servicePort değerini bağlantı noktası >= 10.000 olacak şekilde ayarlayın. Bunu yapmanız bu kapsayıcıda çalıştırılan hizmeti tanımlar; marathon-lb bunu dengelemesi gereken hizmetleri tanımlamak için kullanır.
  * HAPROXY_GROUP etiketini "external" olarak ayarlayın.
  * hostPort değerini 0 olarak ayarlayın. Bunu yaptığınızda marathon, kullanılabilir bir bağlantı noktasını rastgele ayıracaktır.

Bu JSON’u `hello-web.json` adlı dosyaya kopyalayın ve bir kapsayıcıyı dağıtmak için kullanın: 

```bash
dcos marathon app add hello-web.json 
``` 

## Azure LB 

Varsayılan olarak, Azure LB 80, 8080 ve 443 numaralı bağlantı noktalarını kullanıma sunar. Bu üç bağlantı noktasından birini kullanıyorsanız (yukarıdaki örnekte yaptığımız gibi), bir şey yapmanız gerekmez: aracı LB’nizin FQDN’sine temas edebilmeli ve her yenilemenizde üç web sunucusundan birine hepsini bir kez dener biçimde temas eder. Ancak, farklı bir bağlantı noktası kullanıyorsanız, kullandığınız bağlantı noktası için Azure LB’ye hepsini bir kez deneme kuralı ve bir araştırma eklemeniz gerekir. Bu, `azure lb rule create` ve `azure lb probe create` komutlarıyla [Azure XPLAT CLI](../xplat-cli-azure-resource-manager.md)’den yapılabilir.


## İlave senaryolar

Farklı hizmetleri kullanıma sunmak için farklı etki alanlarını kullandığınız bir senaryonuz olabilir. Örneğin: 

mydomain1.com -> Azure LB:80 -> marathon-lb:10001 -> mycontainer1:33292  
mydomain2.com -> Azure LB:80 -> marathon-lb:10002 -> mycontainer2:22321 

Bunu başarmak için, etki alanlarını belirli marathon lb yollarıyla ilişkilendirmek için bir yol sunan [Sanal Konaklar](https://mesosphere.com/blog/2015/12/04/dcos-marathon-lb/)’a bakın.

Alternatif olarak, farklı bağlantı noktalarını kullanıma sunabilir ve bunları marathon lb arkasında doğru hizmete yeniden eşleyebilirsiniz. Örneğin:

Azure lb:80 -> marathon-lb:10001 -> mycontainer:233423  
Azure lb:8080 -> marathon-lb:1002 -> mycontainer2:33432 
 

## Sonraki adımlar

Marathon LB hakkında daha fazla bilgi için [bu web günlüğü gönderisine](https://mesosphere.com/blog/2015/12/04/dcos-marathon-lb/) bakın.



<!--HONumber=Jun16_HO2-->


