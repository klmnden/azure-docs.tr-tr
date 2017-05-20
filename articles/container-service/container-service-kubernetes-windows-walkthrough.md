---
title: "Windows için Azure Kubernetes kümesi | Microsoft Docs"
description: "Azure Container Service’te Windows kapsayıcıları için Kubernetes kümesi dağıtma ve kullanmaya başlama"
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/04/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 18d4994f303a11e9ce2d07bc1124aaedf570fc82
ms.openlocfilehash: 4e730b65a98af05ea00c5f8ebd9914e3367b66a7
ms.contentlocale: tr-tr
ms.lasthandoff: 05/09/2017


---

# <a name="get-started-with-kubernetes-and-windows-containers-in-container-service"></a>Kapsayıcı Hizmetinde Kubernetes ve Windows kapsayıcılarına başlangıç


Bu makalede, Azure Container Service’te Windows kapsayıcılarını çalıştırmaya yönelik Windows düğümleri içeren bir Kubernetes kümesinin nasıl oluşturulacağı açıklanmaktadır. Azure Container Service’te bir Kubernetes kümesi oluşturmak için `az acs` Azure CLI 2.0 komutlarını kullanmaya başlayın. Ardından, Kubernetes `kubectl` komut satırı aracını kullanarak Docker görüntülerinden oluşturulmuş Windows kapsayıcıları ile çalışmaya başlayın. 

> [!NOTE]
> Azure Container Service’te Kubernetes ile Windows kapsayıcıları desteği önizleme aşamasındadır. 
>



Aşağıdaki görüntüde, Azure Container Service’te bir Linux ana düğümü ve iki Windows aracı düğümü içeren bir Kubernetes kümesinin mimarisi gösterilmektedir. 

![Azure’da Kubernetes kümesinin görüntüsü](media/container-service-kubernetes-windows-walkthrough/kubernetes-windows.png)

* Linux ana düğümü, Kubernetes REST API’ye hizmet verir ve SSH tarafından bağlantı noktası 22’de veya `kubectl` tarafından bağlantı noktası 443'te erişilebilir durumdadır. 
* Windows aracı düğümleri, bir Azure kullanılabilirlik kümesinde gruplandırılıp kapsayıcılarınızı çalıştırır. Windows düğümlerine, bir RDP SSH tüneli üzerinden ana düğüm aracılığıyla erişilebilir. Azure Load Balancer kuralları, kullanıma sunulan hizmetlere göre kümeye dinamik olarak eklenir.



Tüm sanal makineler aynı gizli sanal ağ üzerindedir ve birbirlerine tam olarak erişilebilir. Tüm VM’ler, bir kubelet, Docker ve bir ara sunucu çalıştırır.

Daha fazla arka plan bilgisi için bkz. [Azure Container Service’e giriş](container-service-intro.md) ve [Kubernetes belgeleri](https://kubernetes.io/docs/home/).

## <a name="prerequisites"></a>Ön koşullar
Azure CLI 2.0 ile bir Azure Container Service kümesi oluşturmak için şunlar gerekir:
* bir Azure hesabı ([ücretsiz deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/))
* [Azure CLI 2.0](/cli/azure/install-az-cli2) aracının yüklü ve oturumunun açık olması

Ayrıca Kubernetes kümeniz için aşağıdakiler gerekir. Bunları önceden hazırlayabilir veya `az acs create` komut seçeneklerini kullanarak küme dağıtımı sırasında otomatik olarak oluşturabilirsiniz. 

* **SSH RSA ortak anahtarı**: Güvenli Kabuk (SSH) RSA anahtarları oluşturmak istiyorsanız, [macOS ve Linux](../virtual-machines/linux/mac-create-ssh-keys.md) ya da [Windows](../virtual-machines/linux/ssh-from-windows.md) yönergelerine bakın. 

* **Hizmet sorumlusu istemci kimliği ve gizli dizi** : Azure Active Directory hizmet sorumlusu oluşturma adımları ve ek bilgiler için bkz. [Kubernetes kümesinde hizmet sorumlusu hakkında](container-service-kubernetes-service-principal.md).

Bu makaledeki komut örneği, SSH anahtarlarını ve hizmet sorumlusunu otomatik olarak oluşturur.
  
## <a name="create-your-kubernetes-cluster"></a>Kubernetes kümenizi oluşturma

Kümenizi oluşturmaya yönelik Azure CLI 2.0 komutları aşağıda verilmiştir. 

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
Azure Container Service’in [kullanılabilir](https://azure.microsoft.com/regions/services/) olduğu bir konumda kaynak grubu oluşturun. Aşağıdaki komut *westus* konumunda *myKubernetesResourceGroup* adlı bir kaynak grubu oluşturur:

```azurecli
az group create --name=myKubernetesResourceGroup --location=westus
```

### <a name="create-a-kubernetes-cluster-with-windows-agent-nodes"></a>Windows aracı düğümleri içeren bir Kubernetes kümesi oluşturma

Kaynak grubunuzda `--orchestrator-type=kubernetes` ve `--windows` aracı seçeneği ile `az acs create` komutunu kullanarak bir Kubernetes kümesi oluşturun. Komut söz dizimi için bkz. `az acs create` [yardım](/cli/azure/acs#create).

Aşağıdaki komut, yönetim düğümü için *myPrefix* DNS ön eki ve Windows düğümlerine ulaşmak için belirtilen kimlik bilgileri ile *myKubernetesClusterName* adlı bir Container Service kümesi oluşturur. Komutun bu sürümü, Kubernetes kümesinin SSH RSA anahtarları ve hizmet sorumlusunu otomatik olarak oluşturur.


```azurecli
az acs create --orchestrator-type=kubernetes \
    --resource-group myKubernetesResourceGroup \
    --name=myKubernetesClusterName \
    --dns-prefix=myPrefix \
    --agent-count=2 \
    --generate-ssh-keys \
    --windows --admin-username myWindowsAdminName \
    --admin-password myWindowsAdminPassword
```

Birkaç dakika geçtikten sonra komut tamamlanır ve çalışan bir Kubernetes kümesi görmeniz gerekir.

> [!IMPORTANT]
> Hesabınız Azure AD hizmet sorumlusu oluşturma izinlerine sahip değilse, komut `Insufficient privileges to complete the operation.` benzeri bir hata oluşturur. Daha fazla bilgi için bkz. [Kubernetes kümesinde hizmet sorumlusu hakkında](container-service-kubernetes-service-principal.md). 
> 

## <a name="connect-to-the-cluster-with-kubectl"></a>kubectl ile kümeye bağlanma

İstemci bilgisayarınızdan Kubernetes kümesine bağlanmak için Kubernetes’in komut satırı istemcisini ([`kubectl`](https://kubernetes.io/docs/user-guide/kubectl/)) kullanın. 

`kubectl` yerel olarak yüklü değilse `az acs kubernetes install-cli` ile yükleyebilirsiniz. (Ayrıca [Kubernetes sitesinden](https://kubernetes.io/docs/tasks/kubectl/install/) indirebilirsiniz.)

**Linux veya macOS**

```azurecli
sudo az acs kubernetes install-cli
```

**Windows**
```azurecli
az acs kubernetes install-cli
```

> [!TIP]
> Varsayılan olarak, bu komut `kubectl` ikili dosyasını Linux veya macOS sistemlerde `/usr/local/bin/kubectl` konumuna ya da Windows’da `C:\Program Files (x86)\kubectl.exe` konumuna yükler. Farklı bir yükleme yolu belirtmek için `--install-location` parametresini kullanın.
>
> `kubectl` yüklendikten sonra, dizininin sistem yolunuzda olduğundan emin olun veya dizini yola ekleyin. 


Sonra, aşağıdaki komutu çalıştırarak ana Kubernetes kümesinin yapılandırmasını yerel `~/.kube/config` dosyasına indirin:

```azurecli
az acs kubernetes get-credentials --resource-group=myKubernetesResourceGroup --name=myKubernetesClusterName
```

Bu noktada makinenizden kümenize erişmeye hazır olursunuz. Şunu çalıştırmayı deneyin:

```bash
kubectl get nodes
```

Kümenizdeki bir makine listesi görebildiğinizi doğrulayın.

![Kubernetes kümesinde çalışan düğümler](media/container-service-kubernetes-windows-walkthrough/kubectl-get-nodes.png)

## <a name="create-your-first-kubernetes-service"></a>İlk Kubernetes hizmetinizi oluşturma

Kümeyi oluşturup `kubectl` ile bağlandıktan sonra bir Docker kapsayıcısından Windows uygulaması başlatmayı ve İnternet'te kullanıma sunmayı deneyin. Bu temel örnekte Microsoft Internet Information Server (IIS) kapsayıcısı belirtmek için bir JSON dosyası kullanılır ve sonra `kubctl apply` kullanılarak kapsayıcı oluşturulur. 

1. `iis.json` adlı bir yerel dosya oluşturun ve aşağıdakini kopyalayın. Bu dosya Kubernetes’e, Windows Server 2016 Server Core üzerinde [Docker Hub](https://hub.docker.com/r/microsoft/iis/)’daki bir genel görüntüyü kullanarak IIS çalıştırmasını söyler. Kapsayıcı 80 numaralı bağlantı noktasını kullanır, ancak başlangıçta yalnızca küme ağından erişim sağlanabilir.

  ```JSON
  {
    "apiVersion": "v1",
    "kind": "Pod",
    "metadata": {
      "name": "iis",
      "labels": {
        "name": "iis"
      }
    },
    "spec": {
      "containers": [
        {
          "name": "iis",
          "image": "microsoft/iis",
          "ports": [
            {
            "containerPort": 80
            }
          ]
        }
      ],
      "nodeSelector": {
        "beta.kubernetes.io/os": "windows"
      }
    }
  }
  ```
2. Uygulamayı başlatmak için aşağıdakini yazın:  
  
  ```bash
  kubectl apply -f iis.json
  ```  
3. Kapsayıcının dağıtımını izlemek için şunu yazın:  
  ```bash
  kubectl get pods
  ```
  Kapsayıcı dağıtılırken durum `ContainerCreating` şeklindedir. 

  ![ContainerCreating durumundaki IIS kapsayıcısı](media/container-service-kubernetes-windows-walkthrough/iis-pod-creating.png)   

  IIS görüntüsünün boyutu nedeniyle, kapsayıcının `Running` durumuna girmesi birkaç dakika sürebilir.

  ![Çalışıyor durumundaki IIS kapsayıcısı](media/container-service-kubernetes-windows-walkthrough/iis-pod-running.png)

4. Kapsayıcıyı kullanıma sunmak için aşağıdaki komutu yazın:

  ```bash
  kubectl expose pods iis --port=80 --type=LoadBalancer
  ```

  Bu komut ile Kubernetes genel bir IP adresi ile Azure Load Balancer kuralı oluşturur. Değişikliğin yük dengeleyiciye yayılması yaklaşık birkaç dakika sürer. Ayrıntılar için bkz. [Azure Container Service’teki bir Kubernetes kümesinde yük dengeleme kapsayıcıları](container-service-kubernetes-load-balancing.md).

5. Hizmetin durumunu görmek için aşağıdaki komutu çalıştırın.

  ```bash
  kubectl get svc
  ```

  IP adresi başlangıçta `pending` olarak görünür:

  ![Bekleyen dış IP adresi](media/container-service-kubernetes-windows-walkthrough/iis-svc-expose.png)

  Birkaç dakika sonra IP adresi ayarlanır:
  
  ![IIS için dış IP adresi](media/container-service-kubernetes-windows-walkthrough/iis-svc-expose-public.png)


6. Dış IP adresi kullanılabilir olduktan sonra tarayıcınızdan bu adrese gidebilirsiniz:

  ![IIS’e göz atma görüntüsü](media/container-service-kubernetes-windows-walkthrough/kubernetes-iis.png)  

7. IIS pod’unu silmek için şunu yazın:

  ```bash
  kubectl delete pods iis
  ```

## <a name="next-steps"></a>Sonraki adımlar

* Kubernetes kullanıcı arabirimini kullanmak için `kubectl proxy` komutunu çalıştırın. Ardından, http://localhost:8001/ui sayfasına göz atın.

* Özel bir IIS web sitesi oluşturma ve bir Windows kapsayıcısında çalıştırma adımları için [Docker Hub'ı](https://hub.docker.com/r/microsoft/iis/) yönergelerine bakın.

* Windows düğümlerine PuTTy ile ana öğenin RDP SSH tüneli üzerinden erişmek için bkz. [ACS Altyapısı belgeleri](https://github.com/Azure/acs-engine/blob/master/docs/ssh.md#create-port-80-tunnel-to-the-master). 

