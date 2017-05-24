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
ms.date: 05/08/2017
ms.author: anhowe
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 2ec155129374c03ba7e0ecaa5d2bf29a1d3111aa
ms.contentlocale: tr-tr
ms.lasthandoff: 05/10/2017

---

# <a name="get-started-with-a-kubernetes-cluster-in-container-service"></a>Kapsayıcı Hizmetinde Kubernetes kümesine başlangıç


Bu kılavuzda, Azure CLI 2.0 komutlarını kullanarak nasıl Azure Container Service’te bir Kubernetes kümesi oluşturabileceğiniz açıklanır. Daha sonra, `kubectl` komut satırı aracını kullanarak kümedeki kapsayıcılarla çalışmaya başlayabilirsiniz.

Aşağıdaki resimde, bir Linux ana düğümü ile iki Linux aracı düğümüne sahip bir Container Service kümesinin mimarisi gösterilmektedir. Ana düğüm Kubernetes REST API işlevi görür. Aracı düğümleri Azure kullanılabilirlik kümesinde gruplandırılır ve kapsayıcılarınızı çalıştırır. Tüm sanal makineler aynı gizli sanal ağ üzerindedir ve birbirlerine tam olarak erişilebilir.

![Azure’da Kubernetes kümesinin görüntüsü](media/container-service-kubernetes-walkthrough/kubernetes.png)

Daha fazla arka plan bilgisi için bkz. [Azure Container Service’e giriş](container-service-intro.md) ve [Kubernetes belgeleri](https://kubernetes.io/docs/home/).

## <a name="prerequisites"></a>Ön koşullar
Azure CLI 2.0 ile bir Azure Container Service kümesi oluşturmak için şunlar gerekir:
* bir Azure hesabı ([ücretsiz deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/))
* [Azure CLI 2.0](/cli/azure/install-az-cli2) aracının yüklü ve ayarlanmış olması

Ayrıca, şunlar gereklidir (veya küme dağıtımı sırasında otomatik olarak oluşturmak için Azure CLI’yi kullanabilirsiniz):

* **SSH RSA ortak anahtarı**: Güvenli Kabuk (SSH) RSA anahtarlarını önceden oluşturmak istiyorsanız, [macOS ve Linux](../virtual-machines/linux/mac-create-ssh-keys.md) ya da [Windows](../virtual-machines/linux/ssh-from-windows.md) yönergelerine bakın. 

* **Hizmet sorumlusu istemci kimliği ve gizli dizi** : Azure Active Directory hizmet sorumlusu oluşturma adımları ve ek bilgiler için bkz. [Kubernetes kümesinde hizmet sorumlusu hakkında](container-service-kubernetes-service-principal.md).

 Bu makaledeki komut örneği, SSH anahtarlarını ve hizmet sorumlusunu otomatik olarak oluşturur.

## <a name="create-your-kubernetes-cluster"></a>Kubernetes kümenizi oluşturma

Aşağıda, kümenizi oluşturmak için Azure CLI 2.0 kullanan bash kabuk komutlarının kısa bir listesi sağlanmıştır. 

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
Kümenizi oluşturmak için, ilk olarak Azure Container Service’in [kullanılabilir](https://azure.microsoft.com/regions/services/) olduğu bir konumda kaynak grubu oluşturmanız gerekir. Aşağıdaki gibi komutlar çalıştırın:

```azurecli
RESOURCE_GROUP=my-resource-group
LOCATION=westus
az group create --name=$RESOURCE_GROUP --location=$LOCATION
```

### <a name="create-a-cluster"></a>Küme oluşturma
Kaynak grubunuzda `--orchestrator-type=kubernetes` ile `az acs create` komutunu kullanarak bir Kubernetes kümesi oluşturun. Komut söz dizimi için bkz. `az acs create` [yardım](/cli/azure/acs#create).

Komutun bu sürümü, Kubernetes kümesinin SSH RSA anahtarları ve hizmet sorumlusunu otomatik olarak oluşturur.



```azurecli
DNS_PREFIX=some-unique-value
CLUSTER_NAME=any-acs-cluster-name
az acs create --orchestrator-type=kubernetes --resource-group $RESOURCE_GROUP --name=$CLUSTER_NAME --dns-prefix=$DNS_PREFIX --generate-ssh-keys
```

Birkaç dakika geçtikten sonra komut tamamlanır ve çalışan bir Kubernetes kümesi görmeniz gerekir.

> [!IMPORTANT]
> Hesabınız Azure AD hizmet sorumlusu oluşturma izinlerine sahip değilse, komut `Insufficient privileges to complete the operation.` benzeri bir hata oluşturur. Daha fazla bilgi için bkz. [Kubernetes kümesinde hizmet sorumlusu hakkında](container-service-kubernetes-service-principal.md).
> 



### <a name="connect-to-the-cluster"></a>Kümeye bağlanma

İstemci bilgisayarınızdan Kubernetes kümesine bağlanmak için Kubernetes’in komut satırı istemcisini ([`kubectl`](https://kubernetes.io/docs/user-guide/kubectl/)) kullanırsınız. 

`kubectl` henüz yüklenmediyse `az acs kubernetes install-cli` ile yükleyebilirsiniz. (Ayrıca [Kubernetes sitesinden](https://kubernetes.io/docs/tasks/kubectl/install/) indirebilirsiniz.)

```azurecli
sudo az acs kubernetes install-cli
```

> [!TIP]
> Varsayılan olarak, bu komut `kubectl` ikili dosyasını Linux veya macOS sistemlerde `/usr/local/bin/kubectl` konumuna, Windows’da ise `C:\Program Files (x86)\kubectl.exe` konumuna yükler. Farklı bir yükleme yolu belirtmek için `--install-location` parametresini kullanın.
>
> `kubectl` yüklendikten sonra, dizininin sistem yolunuzda olduğundan emin olun veya dizini yola ekleyin. 
>


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

### <a name="start-a-container"></a>Bir kapsayıcı başlatma
Aşağıdaki komutu çalıştırarak bir kapsayıcı (bu örnekte Nginx web sunucusu) çalıştırabilirsiniz:

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
Bu komut, [http://localhost:8001/ui](http://localhost:8001/ui) üzerinde çalışan Kubernetes web kullanıcı arabirimini görüntülemek için kullanabileceğiniz, kimliği doğrulanmış bir proxy çalıştırır. Daha fazla bilgi edinmek için bkz: [Kubernetes web kullanıcı arabirimini Azure Container Service ile kullanma](container-service-kubernetes-ui.md).

![Kubernetes panosunun görüntüsü](media/container-service-kubernetes-walkthrough/kubernetes-dashboard.png)

### <a name="remote-sessions-inside-your-containers"></a>Kapsayıcılarınızın içindeki uzak oturumlar
Kubernetes, komutları kümenizde çalışan uzak bir Docker kapsayıcısında çalıştırmanıza olanak tanır.

```bash
# Get the name of your nginx pods
kubectl get pods
```

Pod adınızı kullanarak, pod üzerinde bir uzak komutu çalıştırabilirsiniz. Örneğin:

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

