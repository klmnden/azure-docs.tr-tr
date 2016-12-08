---
title: "Azure Container Service kümesine bağlanma | Microsoft Belgeleri"
description: "SSH tüneli kullanarak Azure Kapsayıcı Hizmeti kümesine bağlanın."
services: container-service
documentationcenter: 
author: rgardler
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker, Kapsayıcılar, Mikro hizmetler, DC/OS, Azure"
ms.assetid: ff8d9e32-20d2-4658-829f-590dec89603d
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/13/2016
ms.author: rogardle
translationtype: Human Translation
ms.sourcegitcommit: a4882b6fcd75ecaa826cdda3e25ee690b85a0670
ms.openlocfilehash: 34450e25941e0be97b72c1ba30ee348d73f4bc67


---
# <a name="connect-to-an-azure-container-service-cluster"></a>Azure Kapsayıcı Hizmeti kümesine bağlanma
Azure Container Service tarafından dağıtılan DC/OS, Kubernetes ve Docker Swarm kümeleri, REST uç noktalarını kullanıma sunar.  Kubernetes için, bu uç nokta İnternet’te güvenli bir şekilde kullanıma sunulmuştur ve bu uç noktaya İnternet bağlantısı olan herhangi bir makineden doğrudan erişilebilir. DC/OS ve Docker Swarm için, REST uç noktasını güvenli bir şekilde bağlamak için bir SSH tüneli oluşturmanız gerekir. Bu bağlantıların her biri aşağıda açıklanmıştır.

## <a name="connecting-to-a-kubernetes-cluster"></a>Kubernetes kümesine bağlanma.
Kubernetes kümesine bağlanmak için, `kubectl` komut satırı aracının yüklü olması gerekir.  Bu aracı yüklemenin en kolay yolu, Azure 2.0 `az` komut satırı aracını kullanmaktır.

```console
az acs kubernetes install cli [--install-location=/some/directory]
```

Alternatif olarak, istemciyi doğrudan [sürümler sayfasından](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG.md#downloads-for-v146) indirebilirsiniz

`kubectl` komut satırı aracını yükledikten sonra, küme kimlik bilgilerini makinenize kopyalamanız gerekir.  Bunu yapmanın en kolay yolu, yine `az` komut satırı aracını kullanmaktır:

```console
az acs kubernetes get-credentials --dns-prefix=<some-prefix> --location=<some-location>
```

Bu işlem, küme bilgilerini, `kubectl` komut satırı aracının bulunmasını beklediği `$HOME/.kube/config` dizinine indirir.

Alternatif olarak, `scp` komutunu kullanarak, ana VM’deki dosyayı `$HOME/.kube/config` dizininden yerel makinenize güvenli bir şekilde kopyalayabilirsiniz.

```console
mkdir $HOME/.kube/config
scp azureuser@<master-dns-name>:.kube/config $HOME/.kube/config
```

Windows kullanıyorsanız, Windows üzerinde çalışan Ubuntu’da Bash kabuğunu veya Putty 'pscp' aracını kullanmanız gerekir.

`kubectl` komut satırı aracını yapılandırdıktan sonra, bunu,

```console
kubectl get nodes
```

kümenizdeki düğümleri gösterecek şekilde yapılandırabilirsiniz.

Ayrıntılı yönergeler için bkz. [Kubernetes için hızlı başlangıç kılavuzu](http://kubernetes.io/docs/user-guide/quick-start/)

## <a name="connecting-to-a-dcos-or-swarm-cluster"></a>DC/OS veya Swarm kümesine bağlanma

Azure Container Service tarafından dağıtılan DC/OS ve Docker Swarm kümeleri REST uç noktalarını kullanıma sunar. Ancak, bu uç noktalar dış dünyaya açık değildir. Bu uç noktaları yönetmek için bir Secure Shell (SSH) tüneli oluşturmanız gerekir. SSH tüneli oluşturulduktan sonra küme uç noktalarına karşı komutları çalıştırabilir ve kendi sisteminizdeki bir tarayıcı aracılığıyla küme kullanıcı arabirimini görüntüleyebilirsiniz. Bu belgede size Linux, OS X ve Windows’da bir SSH tüneli oluşturma konusunu adım adım anlatılmaktadır.

> [!NOTE]
> Bir küme yönetimi sistemi ile SSH oturumu oluşturabilirsiniz. Ancak bu önerilmemektedir. Doğrudan bir yönetim sistemi üzerinde çalışmak yanlışlıkla yapılandırma değişiklikleri yapma riski doğurur.   
> 
> 

## <a name="create-an-ssh-tunnel-on-linux-or-os-x"></a>Linux veya OS X’de SSH tüneli oluşturma
Linux veya OS X’de bir SSH tüneli oluşturduğunuzda yapacağınız ilk şey yük dengeli ana sunucuların genel DNS adını bulmaktır. Bunu yapmak için, her kaynak görüntülenecek şekilde kaynak grubunu genişletin. Ana sunucunun ortak IP adresini bulun ve seçin. Bu, DNS adını içeren ortak IP adresi hakkında bilgiler içeren dikey pencereyi açar. Bu adı daha sonra kullanmak için kaydedin. <br />

![Genel DNS adı](media/pubdns.png)

Şimdi bir kabuğu açın ve aşağıdaki komutu çalıştırın, buradaki ifadelerin anlamları şu şekildedir:

**PORT** kullanıma sunmak istediğiniz bağlantı noktasıdır. Swarm için, bu 2375’dir. DC/OS için, 80 numaralı bağlantı noktasını kullanın.  
**USERNAME** Kümeyi dağıttığınızda sağlanan kullanıcı adıdır.  
**DNSPREFIX** Kümeyi dağıttığınızda sağladığınız DNS önekidir.  
**REGION** kaynak grubunuzun bulunduğu bölgedir.  
**PATH_TO_PRIVATE_KEY** [OPTIONAL] Kapsayıcı Hizmeti kümesini oluştururken sağladığınız ortak anahtara karşılık gelen özel anahtar yoludur. Bu seçeneği i flag ile birlikte kullanın.

```bash
ssh -L PORT:localhost:PORT -f -N [USERNAME]@[DNSPREFIX]mgmt.[REGION].cloudapp.azure.com -p 2200
```
> SSH bağlantı noktası 2200’dür; standart bağlantı noktası 22 değildir.
> 
> 

## <a name="dcos-tunnel"></a>DC/OS tüneli
DC/OS ile ilgili uç noktalara bir tünel açmak için, aşağıdakine benzeyen bir komut yürütün:

```bash
sudo ssh -L 80:localhost:80 -f -N azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com -p 2200
```

Şimdi aşağıdakiler üzerinde DC/OS ile ilgili uç noktalara erişebilirsiniz:

* DC/OS: `http://localhost/`
* Marathon: `http://localhost/marathon`
* Mesos: `http://localhost/mesos`

Benzer şekilde, bu tünel üzerinden her uygulama için rest API'lerine ulaşabilirsiniz.

## <a name="swarm-tunnel"></a>Swarm tüneli
Swarm uç noktalarına bir tünel açmak için, aşağıdakine benzeyen bir komut yürütün:

```bash
ssh -L 2375:localhost:2375 -f -N azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com -p 2200
```

Şimdi DOCKER_HOST ortam değişkeninizi aşağıdaki gibi ayarlayabilirsiniz. Docker komut satırı arabiriminizi (CLI) normal şekilde kullanmaya devam edebilirsiniz.

```bash
export DOCKER_HOST=:2375
```

## <a name="create-an-ssh-tunnel-on-windows"></a>Windows’da SSH tüneli oluşturma
Windows’da SSH tünelleri oluşturmak için birden çok seçenek vardır. Bu belgede bunu yapmak için PuTTY’nin nasıl kullanılacağı açıklanmıştır.

PuTTY’yi Windows sisteminize indirin ve uygulamayı çalıştırın.

Kümedeki ilk ana sunucunun küme yöneticisi kullanıcı adı ve genel DNS adından oluşan bir ana bilgisayar adı girin. **Ana bilgisayar adı** şuna benzeyecektir: `adminuser@PublicDNS`. **Bağlantı Noktası** için 2200 girin.

![PuTTY yapılandırması 1](media/putty1.png)

**SSH** ve **Kimlik Doğrulaması**’nı seçin. Kimlik doğrulaması için özel anahtar dosyanızı ekleyin.

![PuTTY yapılandırması 2](media/putty2.png)

**Tüneller** öğesini seçin ve aşağıdaki iletilen bağlantı noktalarını yapılandırın:

* **Kaynak Bağlantı Noktası:** Tercihiniz--DC/OS için 80 veya Swarm için 2375 kullanın.
* **Hedef:** DC/OS için localhost:80 veya Swarm için 2375 kullanın.

Aşağıdaki örnek DC/OS için yapılandırılmıştır, ancak Docker Swarm için olan benzer olacaktır.

> [!NOTE]
> Bu tüneli oluştururken bağlantı noktası 80 kullanımda olmamalıdır.
> 
> 

![PuTTY yapılandırması 3](media/putty3.png)

İşiniz bittiğinde, bağlantı yapılandırmasını kaydedin ve PuTTY oturumuna bağlanın. Bağlandığınızda, bağlantı noktası yapılandırmasını PuTTY olay günlüğünde görebilirsiniz.

![PuTTY olay günlüğü](media/putty4.png)

DC/OS için tüneli yapılandırdığınızda aşağıdakiler üzerinde ilgili uç noktaya erişebilirsiniz:

* DC/OS: `http://localhost/`
* Marathon: `http://localhost/marathon`
* Mesos: `http://localhost/mesos`

Docker Swarm için tüneli yapılandırdığınızda Docker CLI aracılığıyla Swarm kümesine erişebilirsiniz. Önce ` :2375` değeriyle `DOCKER_HOST` adlı bir Windows ortamı değişkeni yapılandırmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar
DC/OS ya da Swarm ile kapsayıcıları dağıtın ve yönetin:

* [Azure Container Service ve DC/OS ile çalışma](container-service-mesos-marathon-rest.md)
* [Azure Container Service ve Docker Swarm ile çalışma](container-service-docker-swarm.md)




<!--HONumber=Nov16_HO4-->


