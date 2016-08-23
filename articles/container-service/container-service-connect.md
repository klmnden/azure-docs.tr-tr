<properties
   pageTitle="Azure Kapsayıcı Hizmeti kümesine bağlanma | Microsoft Azure"
   description="SSH tüneli kullanarak Azure Kapsayıcı Hizmeti kümesine bağlanın."
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, Kapsayıcılar, Mikro hizmetler, DC/OS, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/12/2016"
   ms.author="rogardle"/>


# Azure Kapsayıcı Hizmeti kümesine bağlanma

Azure Container Service tarafından dağıtılan DC/OS ve Docker Swarm kümeleri REST uç noktalarını kullanıma sunar. Ancak, bu uç noktalar dış dünyaya açık değildir. Bu uç noktaları yönetmek için bir Secure Shell (SSH) tüneli oluşturmanız gerekir. SSH tüneli oluşturulduktan sonra küme uç noktalarına karşı komutları çalıştırabilir ve kendi sisteminizdeki bir tarayıcı aracılığıyla küme kullanıcı arabirimini görüntüleyebilirsiniz. Bu belgede size Linux, OS X ve Windows’da bir SSH tüneli oluşturma konusunu adım adım anlatılmaktadır.

>[AZURE.NOTE] Bir küme yönetimi sistemi ile SSH oturumu oluşturabilirsiniz. Ancak bu önerilmemektedir. Doğrudan bir yönetim sistemi üzerinde çalışmak yanlışlıkla yapılandırma değişiklikleri yapma riski doğurur.   

## Linux veya OS X’de SSH tüneli oluşturma

Linux veya OS X’de bir SSH tüneli oluşturduğunuzda yapacağınız ilk şey yük dengeli ana sunucuların genel DNS adını bulmaktır. Bunu yapmak için, her kaynak görüntülenecek şekilde kaynak grubunu genişletin. Ana sunucunun ortak IP adresini bulun ve seçin. Bu, DNS adını içeren ortak IP adresi hakkında bilgiler içeren dikey pencereyi açar. Bu adı daha sonra kullanmak için kaydedin. <br />


![Genel DNS adı](media/pubdns.png)

Şimdi bir kabuğu açın ve aşağıdaki komutu çalıştırın, buradaki ifadelerin anlamları şu şekildedir:

**PORT** kullanıma sunmak istediğiniz bağlantı noktasıdır. Swarm için, bu 2375’dir. DC/OS için, 80 numaralı bağlantı noktasını kullanın.  
**USERNAME** Kümeyi dağıttığınızda sağlanan kullanıcı adıdır.  
**DNSPREFIX** Kümeyi dağıttığınızda sağladığınız DNS önekidir.  
**REGION** kaynak grubunuzun bulunduğu bölgedir.  
**PATH_TO_PRIVATE_KEY** [OPTIONAL] Kapsayıcı Hizmeti kümesini oluştururken sağladığınız ortak anahtara karşılık gelen özel anahtar yoludur. Bu seçeneği i flag ile birlikte kullanın.

```bash
# ssh sample

ssh -L PORT:localhost:PORT -f -N [USERNAME]@[DNSPREFIX]mgmt.[REGION].cloudapp.azure.com -p 2200
```
> SSH bağlantı noktası 2200’dür; standart bağlantı noktası 22 değildir.

## DC/OS tüneli

DC/OS ile ilgili uç noktalara bir tünel açmak için, aşağıdakine benzeyen bir komut yürütün:

```bash
# ssh sample

sudo ssh -L 80:localhost:80 -f -N azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com -p 2200
```

Şimdi aşağıdakiler üzerinde DC/OS ile ilgili uç noktalara erişebilirsiniz:

- DC/OS: `http://localhost/`
- Marathon: `http://localhost/marathon`
- Mesos: `http://localhost/mesos`

Benzer şekilde, bu tünel üzerinden her uygulama için rest API'lerine ulaşabilirsiniz.

## Swarm tüneli

Swarm uç noktalarına bir tünel açmak için, aşağıdakine benzeyen bir komut yürütün:

```bash
# ssh sample

ssh -L 2375:localhost:2375 -f -N azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com -p 2200
```

Şimdi DOCKER_HOST ortam değişkeninizi aşağıdaki gibi ayarlayabilirsiniz. Docker komut satırı arabiriminizi (CLI) normal şekilde kullanmaya devam edebilirsiniz.

```bash
export DOCKER_HOST=:2375
```

## Windows’da SSH tüneli oluşturma

Windows’da SSH tünelleri oluşturmak için birden çok seçenek vardır. Bu belgede bunu yapmak için PuTTY’nin nasıl kullanılacağı açıklanmıştır.

PuTTY’yi Windows sisteminize indirin ve uygulamayı çalıştırın.

Kümedeki ilk ana sunucunun küme yöneticisi kullanıcı adı ve genel DNS adından oluşan bir ana bilgisayar adı girin. **Ana bilgisayar adı** şuna benzeyecektir: `adminuser@PublicDNS`. **Bağlantı Noktası** için 2200 girin.

![PuTTY yapılandırması 1](media/putty1.png)

**SSH** ve **Kimlik Doğrulaması**’nı seçin. Kimlik doğrulaması için özel anahtar dosyanızı ekleyin.

![PuTTY yapılandırması 2](media/putty2.png)

**Tüneller** öğesini seçin ve aşağıdaki iletilen bağlantı noktalarını yapılandırın:
- **Kaynak Bağlantı Noktası:** Tercihiniz--DC/OS için 80 veya Swarm için 2375 kullanın.
- **Hedef:** DC/OS için localhost:80 veya Swarm için 2375 kullanın.

Aşağıdaki örnek DC/OS için yapılandırılmıştır, ancak Docker Swarm için olan benzer olacaktır.

>[AZURE.NOTE] Bu tüneli oluştururken bağlantı noktası 80 kullanımda olmamalıdır.

![PuTTY yapılandırması 3](media/putty3.png)

İşiniz bittiğinde, bağlantı yapılandırmasını kaydedin ve PuTTY oturumuna bağlanın. Bağlandığınızda, bağlantı noktası yapılandırmasını PuTTY olay günlüğünde görebilirsiniz.

![PuTTY olay günlüğü](media/putty4.png)

DC/OS için tüneli yapılandırdığınızda aşağıdakiler üzerinde ilgili uç noktaya erişebilirsiniz:

- DC/OS: `http://localhost/`
- Marathon: `http://localhost/marathon`
- Mesos: `http://localhost/mesos`

Docker Swarm için tüneli yapılandırdığınızda Docker CLI aracılığıyla Swarm kümesine erişebilirsiniz. Önce ` :2375` değeriyle `DOCKER_HOST` adlı bir Windows ortamı değişkeni yapılandırmanız gerekir.

## Sonraki adımlar

DC/OS ya da Swarm ile kapsayıcıları dağıtın ve yönetin:

- [Azure Container Service ve DC/OS ile çalışma](container-service-mesos-marathon-rest.md)
- [Azure Container Service ve Docker Swarm ile çalışma](container-service-docker-swarm.md)



<!--HONumber=Aug16_HO1-->


