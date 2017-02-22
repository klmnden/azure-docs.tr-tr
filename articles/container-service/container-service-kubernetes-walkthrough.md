---
title: "Azure’da Kubernetes kümesi hızlı başlangıç | Microsoft Docs"
description: "Azure Container Service’te Kubernetes kümesi dağıtma ve kullanmaya başlama"
services: container-service
documentationcenter: 
author: anhowe
manager: timlt
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.assetid: 8da267e8-2aeb-4c24-9a7a-65bdca3a82d6
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2016
ms.author: anhowe
translationtype: Human Translation
ms.sourcegitcommit: 7ed8fb75f057d5a7cfde5436e72e8fec52d07156
ms.openlocfilehash: f3b2fc301bf7083f192c0ec872c4e032472eef97


---

# <a name="microsoft-azure-container-service-engine---kubernetes-walkthrough"></a>Microsoft Azure Container Service Altyapısı - Kubernetes Kılavuzu

## <a name="prerequisites"></a>Ön koşullar
Bu kılavuz bilgisayarınızda ['azure-cli' komut satırı aracının](https://github.com/azure/azure-cli#installation) yüklü olduğunu ve `~/.ssh/id_rsa.pub` konumunda [SSH ortak anahtarını](../virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md) oluşturduğunuzu varsayar.

## <a name="overview"></a>Genel Bakış

Aşağıdaki yönergeler bir ana ve iki çalışan düğümü ile Kubernetes kümesi oluşturur.
Ana düğüm Kubernetes REST API işlevi görür.  Çalışan düğümü ise Azure kullanılabilirlik kümesinde gruplandırılıp kapsayıcılarınızı çalıştırır. Tüm sanal makineler aynı sanal ağ içindedir ve birbirine tam olarak erişilebilir.

> [!NOTE]
> Azure Container Service'teki Kubernetes desteği şu anda önizleme aşamasındadır.
>

Aşağıdaki görüntüde bir ana ve iki aracı düğüme sahip kapsayıcı hizmeti kümesinin mimarisi gösterilmektedir:

![Azure’da Kubernetes kümesinin görüntüsü](media/container-service-kubernetes-walkthrough/kubernetes.png)

## <a name="creating-your-kubernetes-cluster"></a>Kubernetes kümesi oluşturma

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
Kümenizi oluşturmak için ilk olarak belirli bir konumda bir kaynak grubu oluşturmanız gerekir. Bunu şununla yapabilirsiniz:
```console
RESOURCE_GROUP=my-resource-group
LOCATION=westus
az group create --name=$RESOURCE_GROUP --location=$LOCATION
```

### <a name="create-a-cluster"></a>Küme oluşturma
Bir kaynak grubu oluşturduktan sonra bu grupta aşağıdaki bir küme oluşturabilirsiniz:
```console
DNS_PREFIX=some-unique-value
SERVICE_NAME=any-acs-service-name
az acs create --orchestrator-type=kubernetes --resource-group $RESOURCE_GROUP --name=$SERVICE_NAME --dns-prefix=$DNS_PREFIX
```

> [!NOTE]
> azure-cli, `~/.ssh/id_rsa.pub` öğesini Linux VM’lerine yükler.
>

Bu komut tamamlandıktan sonra çalışan bir Kubernetes kümesine sahip olursunuz.

### <a name="configure-kubectl"></a>Kubectl yapılandırma
`kubectl`, Kubernetes komut satırı istemcisidir.  Henüz yüklemediyseniz şununla yükleyebilirsiniz:

```console
az acs kubernetes install-cli
```

`kubectl` yüklendikten sonra aşağıdaki komutun çalıştırılması ana kubernetes küme yapılandırmasını ~/.kube/config dosyasına indirir
```console
az acs kubernetes get-credentials --resource-group=$RESOURCE_GROUP --name=$SERVICE_NAME
```

Bu noktada makinenizden kümenize erişmeye hazır olmanız gerekir. Şunu çalıştırmayı deneyin:
```console
kubectl get nodes
```

Bu durumda kümenizdeki makineleri görebildiğinizi doğrulayın.

## <a name="create-your-first-kubernetes-service"></a>İlk Kubernetes Hizmetinizi oluşturma

Bu kılavuzu tamamladıktan sonra şunları öğrenmiş olacaksınız:
 * Docker uygulaması dağıtma ve genel kullanıma sunma,
 * `kubectl exec` kullanarak bir kapsayıcıdaki komutları çalıştırma, 
 * Kubernetes panosuna erişme.

### <a name="start-a-simple-container"></a>Basit bir kapsayıcı başlatma
Aşağıdaki komutu çalıştırarak basit bir kapsayıcı (bu örnekte `nginx` web sunucusu) çalıştırabilirsiniz:

```console
kubectl run nginx --image nginx
```

Bu komut nginx Docker kapsayıcısını düğümlerin birindeki bir pod içinde başlatır.

Çalışan kapsayıcıyı görmek için
```console
kubectl get pods
```

komutunu çalıştırabilirsiniz.

### <a name="expose-the-service-to-the-world"></a>Hizmeti genel kullanıma sunma
Hizmeti genel kullanıma sunmak için.  `LoadBalancer` türünde bir Kubernetes `Service` oluşturun:

```console
kubectl expose deployments nginx --port=80 --type=LoadBalancer
```

Bunun yapılması Kubernetes’in ortak bir IP ile Azure Load Balancer oluşturmasına neden olur. Değişikliğin yük dengeleyiciye yayılması yaklaşık 2-3 dakika sürer.

Hizmetin "bekliyor" durumundan bir dış ip türüne geçtiğini izlemek için:
```console
watch 'kubectl get svc'
```

  ![Bekliyor durumundan dış ip’ye geçişi izleme görüntüsü](media/container-service-kubernetes-walkthrough/kubernetes-nginx3.png)

Dış IP’yi görmenizden sonra tarayıcınızda bu IP’ye göz atabilirsiniz:

  ![nginx’e göz atma görüntüsü](media/container-service-kubernetes-walkthrough/kubernetes-nginx4.png)  


### <a name="browse-the-kubernetes-ui"></a>Kubernetes Kullanıcı Arabirimini Göz Atma
Kubernetes web arabirimini görmek için şunu kullanabilirsiniz:

```console
kubectl proxy
```
Bu komut localhost üzerinde [kubernetes kullanıcı arabirimini](http://localhost:8001/ui) görüntülemek için kullanabileceğiniz basit bir kimliği doğrulanmış ara sunucu çalıştırır

![Kubernetes panosunun görüntüsü](media/container-service-kubernetes-walkthrough/kubernetes-dashboard.png)

### <a name="remote-sessions-inside-your-containers"></a>Kapsayıcılarınızın içindeki uzak oturumlar
Kubernetes, komutları kümenizde çalışan uzak bir Docker kapsayıcısında çalıştırmanıza olanak tanır.

```console
# Get the name of your nginx pod
kubectl get pods
```

Pod adınızı kullanarak, pod üzerinde bir uzak komutu çalıştırabilirsiniz.  Örneğin:
```console
kubectl exec nginx-701339712-retbj date
```

Ayrıca `-it` bayraklarını kullanarak tam etkileşimli bir oturum elde edebilirsiniz:

```console
kubectl exec nginx-701339712-retbj -it bash
```

![podIP eğrisinin görüntüsü](media/container-service-kubernetes-walkthrough/kubernetes-remote.png)


## <a name="details"></a>Ayrıntılar
### <a name="installing-the-kubectl-configuration-file"></a>Kubectl yapılandırma dosyasını yükleme
`az acs kubernetes get-credentials` komutunu çalıştırdığınızda, uzaktan erişim için kube yapılandırma dosyası ~/.kube/config giriş dizininin altına depolanmıştı.

Doğrudan indirmeniz gerekirse Linux veya OS X’te `ssh`, windows’ta `Putty` kullanabilirsiniz:

#### <a name="windows"></a>Windows
Pscp’yi [putty](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)’den kullanmak için.  Sertifikanızın [pageant](https://github.com/Azure/acs-engine/blob/master/docs/ssh.md#key-management-and-agent-forwarding-with-windows-pageant) aracılığıyla kullanıma sunulduğundan emin olun:
  ```
  # MASTERFQDN is obtained in step1
  pscp azureuser@MASTERFQDN:.kube/config .
  SET KUBECONFIG=%CD%\config
  kubectl get nodes
  ```

#### <a name="os-x-or-linux"></a>OS X veya Linux:
  ```
  # MASTERFQDN is obtained in step1
  scp azureuser@MASTERFQDN:.kube/config .
  export KUBECONFIG=`pwd`/config
  kubectl get nodes
  ```
## <a name="learning-more"></a>Daha Fazla Bilgi

### <a name="azure-container-service"></a>Azure Container Service

1. [Azure Container Service belgeleri](https://azure.microsoft.com/en-us/documentation/services/container-service/)
2. [Azure Container Service Açık Kaynak Altyapısı](https://github.com/azure/acs-engine)

### <a name="kubernetes-community-documentation"></a>Kubernetes Topluluk Belgeleri

1. [Kubernetes Bootcamp](https://katacoda.com/embed/kubernetes-bootcamp/1/) - kapsayıcılı uygulamalar için dağıtma, ölçeklendirme, güncelleştirme ve hata ayıklama işlemlerini gösterir.
2. [Kubernetes Kullanıcı Kılavuzu](http://kubernetes.io/docs/user-guide/) - var olan bir Kubernetes kümesindeki çalışan programlar hakkında bilgi sağlar.
3. [Kubernetes Örnekleri](https://github.com/kubernetes/kubernetes/tree/master/examples) - Kubernetes ile gerçek uygulamaları çalıştırmaya ilişkin birkaç örnek verir.



<!--HONumber=Jan17_HO4-->


