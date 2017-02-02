---
title: "Azure Container Service kümesine bağlanma | Microsoft Belgeleri"
description: "Uzak bilgisayardan Azure Container Service’teki Kubernetes, DC/OS veya Docker Swarm kümesine bağlanma"
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker, Container’lar, Mikro hizmetler, Kumernetes, DC/OS, Azure"
ms.assetid: ff8d9e32-20d2-4658-829f-590dec89603d
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/12/2017
ms.author: rogardle
translationtype: Human Translation
ms.sourcegitcommit: 2549ca9cd05f44f644687bbdf588f7af01bae3f4
ms.openlocfilehash: 79162e5d31346370e596f39fa4827d49625897b3


---
# <a name="connect-to-an-azure-container-service-cluster"></a>Azure Kapsayıcı Hizmeti kümesine bağlanma
Azure Container Service kümesi oluşturduktan sonra, iş yüklerini dağıtmak ve yönetmek için kümeye bağlanmanız gerekir. Bu makalede uzak bir bilgisayardan kümenin ana VM’ine nasıl bağlanacağınız açıklanır. Kumernetes, DC/OS ve Docker Swarm kümelerinin tamamı REST uç noktalarını kullanıma sunar. Kubernetes için, bu uç nokta İnternet’te güvenli bir şekilde kullanıma sunulmuştur ve bu uç noktaya İnternet bağlantısı olan herhangi bir makineden `kubectl` komutunu çalıştırarak erişebilirsiniz. DC/OS ve Docker Swarm için, REST uç noktasını güvenli bir şekilde bağlamak için bir güvenli teslim (SSH) tüneli oluşturmanız gerekir. 

> [!NOTE]
> Azure Container Service'teki Kubernetes desteği şu anda önizleme aşamasındadır.
>

## <a name="prerequisites"></a>Ön koşullar

* Bir Kubernetes, DC/OS veya Swarm kümesi [Azure Container Service’te dağıtılır](container-service-deployment.md).
* Dağıtım sırasında kümeye eklenen ortak anahtara denk gelen SSH özel anahtar dosyası. Bu komutlar, özel SSH anahtarının bilgisayarınızda `$HOME/.ssh/id_rsa` içerisinde olduğunu varsayar. Daha fazla bilgi için [OS X ve Linux](../virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md) veya [Windows](../virtual-machines/virtual-machines-linux-ssh-from-windows.md) ile ilgili şu yönergelere bakın. SSH bağlantısı çalışmıyorsa,[SSH anahtarlarınızı sıfırlamanız](../virtual-machines/virtual-machines-linux-troubleshoot-ssh-connection.md) gerekebilir.

## <a name="connect-to-a-kubernetes-cluster"></a>Kubernetes kümesine bağlanma

Bilgisayarınızda `kubectl` yükleyip yapılandırmak için şu adımları takip edin.

> [!NOTE] 
> Linux veya OS X’te `sudo` kullanarak bu bölümdeki komutları çalıştırmanız gerekebilir..
> 

### <a name="install-kubectl"></a>Kubectl yükleyin
Bu aracı yüklemenin en kolay yolu, Azure 2.0 `az acs kubernetes install-cli` (Önizleme) komut satırı aracını kullanmaktır. Bu komutu çalıştırmak için Azure CLI 2.0’ın (Önizleme) en son sürümünü [yüklediğinizden](/cli/azure/install-az-cli2) ve bir Azure hesabında (`az login`) oturum açtığınızdan emin olun.

```azurecli
# Linux or OS X
az acs kubernetes install-cli [--install-location=/some/directory/kubectl]

# Windows
az acs kubernetes install-cli [--install-location=C:\some\directory\kubectl.exe]
```

Alternatif olarak, istemciyi doğrudan [sürümler sayfasından](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG.md#downloads-for-v146) indirebilirsiniz.

### <a name="download-cluster-credentials"></a>Küme kimlik bilgilerini indirme
`kubectl` komut satırı aracını yükledikten sonra, küme kimlik bilgilerini makinenize kopyalamanız gerekir. Kimlik bilgilerini almak için başka bir yöntem de `az acs kubernetes get-credentials` komutunu çalıştırmaktır. Kaynak grubunun ve kapsayıcı hizmet kaynağının adını geçirin:


```azurecli
az acs kubernetes get-credentials --resource-group=<cluster-resource-group> --name=<cluster-name>
```

Bu komut, küme bilgilerini, `kubectl` komut satırı aracının bulunmasını beklediği `$HOME/.kube/config` dizinine indirir.

Alternatif olarak, `scp` komutunu kullanarak, ana VM’deki dosyayı `$HOME/.kube/config` dizininden yerel makinenize güvenli bir şekilde kopyalayabilirsiniz. Örneğin:

```console
mkdir $HOME/.kube/config
scp azureuser@<master-dns-name>:.kube/config $HOME/.kube/config
```

Windows kullanıyorsanız, Windows’ta Bash on Ubuntu veya PuTTy güvenli dosya kopyalama istemcisi veya benzer bir araç kullanmanız gerekir.



### <a name="use-kubectl"></a>Kubectl kullanma

`kubectl` yapılandırmasını tamamladıktan sonra bağlantıyı test etmek için kümenizdeki düğümleri listeleyebilirsiniz:

```console
kubectl get nodes
```

Diğer `kubectl` komutlarını deneyebilirsiniz. Örneğin, Kubernetes Panosunu görüntüleyebilirsiniz. İlk olarak Kubernetes API sunucusuna bir ara sunucu çalıştırın:

```console
kubectl proxy
```

Kubernetes UI sayfasına şu adresten ulaşabilirsiniz: `http://localhost:8001/ui`.

Daha fazla bilgi için bkz: [Kubernetes hızlı başlangıç](http://kubernetes.io/docs/user-guide/quick-start/).

## <a name="connect-to-a-dcos-or-swarm-cluster"></a>DC/OS veya Swarm kümesine bağlanma

Azure Container Service tarafından dağıtılan DC/OS ve Docker Swarm kümeleri REST uç noktalarını kullanıma sunar. Ancak, bu uç noktalar dış dünyaya açık değildir. Bu uç noktaları yönetmek için bir Secure Shell (SSH) tüneli oluşturmanız gerekir. SSH tüneli oluşturulduktan sonra küme uç noktalarına karşı komutları çalıştırabilir ve kendi sisteminizdeki bir tarayıcı aracılığıyla küme kullanıcı arabirimini görüntüleyebilirsiniz. Aşağıdaki bölümler Linux, OS X ve Windows işletim sistemli bilgisayarlardan bir SSH tüneli oluşturma konusunda size yol gösterecektir.

> [!NOTE]
> Bir küme yönetimi sistemi ile SSH oturumu oluşturabilirsiniz. Ancak bu önerilmemektedir. Doğrudan bir yönetim sistemi üzerinde çalışmak yanlışlıkla yapılandırma değişiklikleri yapma riski doğurur.
> 

### <a name="create-an-ssh-tunnel-on-linux-or-os-x"></a>Linux veya OS X’de SSH tüneli oluşturma
Linux veya OS X’de bir SSH tüneli oluşturduğunuzda yapacağınız ilk şey yük dengeli ana sunucuların genel DNS adını bulmaktır. Şu adımları uygulayın:


1. [Azure portal](https://portal.azure.com)’da kapsayıcı hizmet kümenizi içeren kaynak grubuna gidin. Her kaynağın görüntülenmesi için kaynak grubu genişletin. 

2. Ana makinenin sanal makinesini bulup seçin. Bir DC/OS kümesinde bu kaynağın adı **dcos-master -** ile başlar. 

    **Sanal makine** dikey penceresi, DNS adını içeren açık IP adresi ile ilgili bilgileri içerir. Bu adı daha sonra kullanmak için kaydedin. 

    ![Genel DNS adı](media/pubdns.png)

3. Şimdi bir kabuk açın ve aşağıdaki değerleri belirleyerek `ssh` komutunu çalıştırın: 

    **PORT** kullanıma sunmak istediğiniz bağlantı noktasıdır. Swarm için 2375 bağlantı noktasını kullanın. DC/OS için, 80 numaralı bağlantı noktasını kullanın.  
    **USERNAME** Kümeyi dağıttığınızda sağlanan kullanıcı adıdır.  
    **DNSPREFIX** Kümeyi dağıttığınızda sağladığınız DNS önekidir.  
    **REGION** kaynak grubunuzun bulunduğu bölgedir.  
    **PATH_TO_PRIVATE_KEY** [OPTIONAL] kümeyi oluştururken sağladığınız ortak anahtara karşılık gelen özel anahtar yoludur. Bu seçeneği `-i` flag ile birlikte kullanın.

    ```bash
    ssh -L PORT:localhost:PORT -f -N [USERNAME]@[DNSPREFIX]mgmt.[REGION].cloudapp.azure.com -p 2200
    ```
    > [!NOTE]
    > SSH bağlantı noktası 2200’dür; standart bağlantı noktası 22 değildir. Birden fazla ana VM bulunan kümede bu, ilk ana VM’e bağlantı noktasıdır.
    > 

Aşağıdaki bölümlerde DC/OS ve Swarm ile ilgili örneklere bakın.    

### <a name="dcos-tunnel"></a>DC/OS tüneli
DC/OS ile ilgili uç noktalara bir tünel açmak için, aşağıdakine benzeyen bir komut çalıştırın:

```bash
sudo ssh -L 80:localhost:80 -f -N azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com -p 2200
```

Şimdi aşağıdakiler üzerinde DC/OS ile ilgili uç noktalara erişebilirsiniz:

* DC/OS: `http://localhost/`
* Marathon: `http://localhost/marathon`
* Mesos: `http://localhost/mesos`

Benzer şekilde, bu tünel üzerinden her uygulama için rest API'lerine ulaşabilirsiniz.

### <a name="swarm-tunnel"></a>Swarm tüneli
Swarm uç noktalarına bir tünel açmak için, aşağıdakine benzeyen bir komut çalıştırın:

```bash
ssh -L 2375:localhost:2375 -f -N azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com -p 2200
```

Şimdi DOCKER_HOST ortam değişkeninizi aşağıdaki gibi ayarlayabilirsiniz. Docker komut satırı arabiriminizi (CLI) normal şekilde kullanmaya devam edebilirsiniz.

```bash
export DOCKER_HOST=:2375
```

### <a name="create-an-ssh-tunnel-on-windows"></a>Windows’da SSH tüneli oluşturma
Windows’da SSH tünelleri oluşturmak için birden çok seçenek vardır. Bu bölüm, tüneli oluşturmak için PuTTy’nin nasıl kullanılacağını açıklar.

1. [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)’yi Windows sisteminize indirin.

2. Uygulamayı çalıştırın.

3. Kümedeki ilk ana sunucunun küme yöneticisi kullanıcı adı ve genel DNS adından oluşan bir ana bilgisayar adı girin. **Ana Bilgisayar Adı** `adminuser@PublicDNSName`’e benzer. **Bağlantı Noktası** için 2200 girin.

    ![PuTTY yapılandırması 1](media/putty1.png)

4. **SSH > Yetkilendirme** öğesini seçin. Özel anahtar dosyanıza (.ppk biçimi) kimlik doğrulaması için bir yol ekleyin. [PuTTYgen](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) gibi bir araç kullanarak bu dosyayı kümenin oluşturulması için kullanılan SSH anahtarından oluşturabilirsiniz.

    ![PuTTY yapılandırması 2](media/putty2.png)

5. **SSH > Tüneller** öğesini seçin ve aşağıdaki iletilen bağlantı noktalarını yapılandırın:

    * **Kaynak Bağlantı Noktası:** DC/OS için 80 veya Swarm için 2375 kullanın.
    * **Hedef:** DC/OS için localhost:80 veya Swarm için&2375; kullanın.

    Aşağıdaki örnek DC/OS için yapılandırılmıştır, ancak Docker Swarm için olan benzer olacaktır.

    > [!NOTE]
    > Bu tüneli oluştururken bağlantı noktası 80 kullanımda olmamalıdır.
    > 

    ![PuTTY yapılandırması 3](media/putty3.png)

6. İşiniz bittiğinde, bağlantı yapılandırmasını kaydetmek için **Oturum > Kaydet**’e tıklayın.

7. PuTTY oturumuna bağlanmak için **Aç**’a tıklayın. Bağlandığınızda, bağlantı noktası yapılandırmasını PuTTY olay günlüğünde görebilirsiniz.

    ![PuTTY olay günlüğü](media/putty4.png)

DC/OS için tünelini yapılandırdıktan sonra aşağıdakiler üzerinde ilgili uç noktaya erişebilirsiniz:

* DC/OS: `http://localhost/`
* Marathon: `http://localhost/marathon`
* Mesos: `http://localhost/mesos`

Docker Swarm için bir tünel yapılandırdıktan sonra `:2375` değeriyle `DOCKER_HOST` adlı bir sistem ortam değişkeni yapılandırmak için Windows ayarlarınızı açın. Ardından Docker CLI üzerinden Swarm kümesine erişebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Kümenizde kapsayıcıları dağıtma ve yönetme:

* [Azure Container Service ve Kubernetes ile çalışma](container-service-kubernetes-ui.md)
* [Azure Container Service ve DC/OS ile çalışma](container-service-mesos-marathon-rest.md)
* [Azure Container Service ve Docker Swarm ile çalışma](container-service-docker-swarm.md)




<!--HONumber=Jan17_HO4-->


