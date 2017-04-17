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
ms.date: 04/05/2017
ms.author: anhowe
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 538f282b28e5f43f43bf6ef28af20a4d8daea369
ms.openlocfilehash: 5c529ae41b42d276d37e6103305e33ed04694e18
ms.lasthandoff: 04/07/2017

---

# <a name="get-started-with-a-kubernetes-cluster-in-container-service"></a>Kapsayıcı Hizmetinde Kubernetes kümesine başlangıç


Bu kılavuzda, Azure CLI 2.0 komutlarını kullanarak nasıl Azure Container Service’te bir Kubernetes kümesi oluşturabileceğiniz açıklanır. Daha sonra, `kubectl` komut satırı aracını kullanarak kümedeki kapsayıcılarla çalışmaya başlayabilirsiniz.

Aşağıdaki resimde, bir ana düğüm ile iki aracı düğüme sahip bir Container Service kümesinin mimarisi gösterilmektedir. Ana düğüm Kubernetes REST API işlevi görür. Aracı düğümleri Azure kullanılabilirlik kümesinde gruplandırılır ve kapsayıcılarınızı çalıştırır. Tüm sanal makineler aynı gizli sanal ağ üzerindedir ve birbirlerine tam olarak erişilebilir.

![Azure’da Kubernetes kümesinin görüntüsü](media/container-service-kubernetes-walkthrough/kubernetes.png)

## <a name="prerequisites"></a>Ön koşullar
Bu kılavuzda [Azure CLI 2.0](/cli/azure/install-az-cli2) aracını yükleyip ayarlamış olduğunuz varsayılır. 

Komut örnekleri için Azure CLI’yı Linux ve macOS’ta yaygın olan bir bash kabuğunda çalıştırdığınız varsayılır. Azure CLI’yı bir Windows istemcisinde çalıştırırsanız, komut kabuğunuza bağlı olarak bazı betik oluşturma ve dosya söz dizimi farklılıkları olabilir. 

## <a name="create-your-kubernetes-cluster"></a>Kubernetes kümenizi oluşturma

Aşağıda, kümenizi oluşturmak için Azure CLI 2.0 kullanan kabuk komutlarının kısa bir listesi sağlanmıştır. 

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
Kümenizi oluşturmak için ilk olarak belirli bir konumda bir kaynak grubu oluşturmanız gerekir. Aşağıdaki gibi komutlar çalıştırın:

```azurecli
RESOURCE_GROUP=my-resource-group
LOCATION=westus
az group create --name=$RESOURCE_GROUP --location=$LOCATION
```

### <a name="create-a-cluster"></a>Küme oluşturma
Bir kaynak grubu oluşturduktan sonra bu grupta bir küme oluşturabilirsiniz. Aşağıdaki örnekte kullanılan `--generate-ssh-keys` seçeneği, varsayılan `~/.ssh/` dizininde zaten mevcut olmaması durumunda dağıtım için gerekli SSH genel ve özel anahtar dosyalarını oluşturur. 

Bu komut, Azure’daki bir Kubernetes kümesinin kullandığı [Azure Active Directory hizmet sorumlusunu](container-service-kubernetes-service-principal.md) da otomatik olarak oluşturur.

```azurecli
DNS_PREFIX=some-unique-value
CLUSTER_NAME=any-acs-cluster-name
az acs create --orchestrator-type=kubernetes --resource-group $RESOURCE_GROUP --name=$CLUSTER_NAME --dns-prefix=$DNS_PREFIX --generate-ssh-keys
```


Birkaç dakika geçtikten sonra komut tamamlanır ve çalışan bir Kubernetes kümesi görmeniz gerekir.

### <a name="connect-to-the-cluster"></a>Kümeye bağlanma

İstemci bilgisayarınızdan Kubernetes kümesine bağlanmak için Kubernetes’in komut satırı istemcisini ([`kubectl`](https://kubernetes.io/docs/user-guide/kubectl/)) kullanırsınız. 

`kubectl` henüz yüklenmediyse şununla yükleyebilirsiniz:

```azurecli
sudo az acs kubernetes install-cli
```
> [!TIP]
> Varsayılan olarak, bu komut `kubectl` ikili dosyasını Linux veya macOS sistemlerde `/usr/local/bin/kubectl` konumuna, Windows’da ise `C:\Program Files (x86)\kubectl.exe` konumuna yükler. Farklı bir yükleme yolu belirtmek için `--install-location` parametresini kullanın.
>

`kubectl` yüklendikten sonra, dizininin sistem yolunuzda olduğundan emin olun veya dizini yola ekleyin. 


Sonra, aşağıdaki komutu çalıştırarak ana Kubernetes kümesinin yapılandırmasını `~/.kube/config` dosyasına indirin:

```azurecli
az acs kubernetes get-credentials --resource-group=$RESOURCE_GROUP --name=$CLUSTER_NAME
```

`kubectl` yüklemesi ve yapılandırmasına yönelik daha fazla seçenek için bkz. [Azure Container Service kümesine bağlanma](container-service-connect.md).

Bu noktada makinenizden kümenize erişmeye hazır olmanız gerekir. Şunu çalıştırmayı deneyin:

```bash
kubectl get nodes
```

Kümenizdeki bir makine listesi görebildiğinizi doğrulayın.

## <a name="create-your-first-kubernetes-service"></a>İlk Kubernetes hizmetinizi oluşturma

Bu kılavuzu tamamladıktan sonra şunları öğrenmiş olacaksınız:
* Docker uygulaması dağıtma ve genel kullanıma sunma
* `kubectl exec` kullanarak bir kapsayıcıdaki komutları çalıştırma 
* Kubernetes panosuna erişme

### <a name="start-a-simple-container"></a>Basit bir kapsayıcı başlatma
Aşağıdaki komutu çalıştırarak basit bir kapsayıcı (bu örnekte Nginx web sunucusu) çalıştırabilirsiniz:

```bash
kubectl run nginx --image nginx
```

Bu komut, düğümlerin birindeki bir pod içinde Nginx Docker kapsayıcısını başlatır.

Çalıştırma kapsayıcısını görmek için şu komutu çalıştırın:

```bash
kubectl get pods
```

### <a name="expose-the-service-to-the-world"></a>Hizmeti genel kullanıma sunma
Hizmeti genel kullanıma sunmak için `LoadBalancer` türünde bir Kubernetes `Service` oluşturun:

```bash
kubectl expose deployments nginx --port=80 --type=LoadBalancer
```

Bu komut, Kubernetes’in genel bir IP adresi ile Azure Load Balancer kuralı oluşturmasına neden olur. Değişikliğin yük dengeleyiciye yayılması yaklaşık birkaç dakika sürer. Daha fazla bilgi edinmek için bkz. [Azure Container Service’teki bir Kubernetes kümesinde yük dengeleme kapsayıcıları](container-service-kubernetes-load-balancing.md).

Hizmetin `pending` durumundan çıkarak harici bir IP adresi görüntülemesini sağlamak için aşağıdaki komutu çalıştırın:

```bash
watch 'kubectl get svc'
```

  ![Bekliyor durumundan harici IP adresine geçişi izleme görüntüsü](media/container-service-kubernetes-walkthrough/kubernetes-nginx3.png)

Harici IP adresini gördükten sonra tarayıcınızdan bu adrese gidebilirsiniz:

  ![Nginx’e göz atma görüntüsü](media/container-service-kubernetes-walkthrough/kubernetes-nginx4.png)  


### <a name="browse-the-kubernetes-ui"></a>Kubernetes Kullanıcı Arabirimini Göz Atma
Kubernetes web arabirimini görmek için şunu kullanabilirsiniz:

```bash
kubectl proxy
```
Bu komut, [http://localhost:8001/ui](http://localhost:8001/ui) üzerinde çalışan Kubernetes web kullanıcı arabirimini görüntülemek için kullanabileceğiniz, basit bir kimliği doğrulanmış proxy çalıştırır. Daha fazla bilgi edinmek için bkz: [Kubernetes web kullanıcı arabirimini Azure Container Service ile kullanma](container-service-kubernetes-ui.md).

![Kubernetes panosunun görüntüsü](media/container-service-kubernetes-walkthrough/kubernetes-dashboard.png)

### <a name="remote-sessions-inside-your-containers"></a>Kapsayıcılarınızın içindeki uzak oturumlar
Kubernetes, komutları kümenizde çalışan uzak bir Docker kapsayıcısında çalıştırmanıza olanak tanır.

```bash
# Get the name of your nginx pods
kubectl get pods
```

Pod adınızı kullanarak, pod üzerinde bir uzak komutu çalıştırabilirsiniz.  Örneğin:

```bash
kubectl exec <pod name> date
```

Ayrıca `-it` bayraklarını kullanarak tam etkileşimli bir oturum elde edebilirsiniz:

```bash
kubectl exec <pod name> -it bash
```

![Bir kapsayıcı içinde uzak oturum](media/container-service-kubernetes-walkthrough/kubernetes-remote.png)



## <a name="next-steps"></a>Sonraki adımlar

Kubernetes kümenizle daha fazlasını yapmak için aşağıdaki kaynaklara bakın:

* [Kubernetes Bootcamp](https://katacoda.com/embed/kubernetes-bootcamp/1/) - kapsayıcılı uygulamalar için dağıtma, ölçeklendirme, güncelleştirme ve hata ayıklama işlemlerini gösterir.
* [Kubernetes Kullanıcı Kılavuzu](http://kubernetes.io/docs/user-guide/) - var olan bir Kubernetes kümesindeki çalışan programlar hakkında bilgi sağlar.
* [Kubernetes Örnekleri](https://github.com/kubernetes/kubernetes/tree/master/examples) - Kubernetes ile gerçek uygulama çalıştırmaya ilişkin örnekler sunar.

