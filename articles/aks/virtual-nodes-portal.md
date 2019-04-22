---
title: Azure Kubernetes Hizmetleri (AKS) portalı kullanarak sanal düğümler oluştur
description: Pod'ları çalıştırmak için sanal düğümü kullanan Azure Kubernetes Hizmetleri (AKS) kümesini oluşturmak için Azure portalını kullanmayı öğrenin.
services: container-service
author: iainfoulds
ms.topic: conceptual
ms.service: container-service
ms.date: 12/03/2018
ms.author: iainfou
ms.openlocfilehash: 4b9e9aeab6ed24dd2179f853def02ad194fe1b67
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59681038"
---
# <a name="preview---create-and-configure-an-azure-kubernetes-services-aks-cluster-to-use-virtual-nodes-in-the-azure-portal"></a>Önizleme - oluşturma ve Azure portalında sanal düğümü kullanmak için Azure Kubernetes Hizmetleri (AKS) kümesi yapılandırma

Azure Kubernetes Service (AKS) kümesini iş yüklerini hızla dağıtmak için sanal düğümü kullanabilirsiniz. Sanal düğüm ile pod'ların hızlı sağlama sahip ve yalnızca yürütme zamanları için saniye başına ödeme yaparsınız. Ölçekleme bir senaryoda, ek pod'lar çalıştırmak için VM hesaplama düğümlerini dağıtmak Kubernetes küme ölçeklendiriciyi için beklemeniz gerekmez. Bu makalede oluşturma ve etkin sanal düğümü ile sanal ağ kaynakları ve AKS kümesi yapılandırma gösterilmektedir.

> [!IMPORTANT]
> AKS Önizleme özellikleri, Self Servis ve kabul etme. Görüş ve hata topluluğumuza toplamak üzere önizlemeleri sağlanır. Ancak, Azure teknik destek birimi tarafından desteklenmez. Bir küme oluşturun veya var olan kümeleri için bu özellikleri ekleyin, bu özellik artık Önizleme aşamasındadır ve genel kullanılabilirlik (GA) mezunu kadar bu küme desteklenmiyor.
>
> Önizleme özellikleri sorunlarla karşılaşırsanız [AKS GitHub deposunda bir sorun açın] [ aks-github] hata başlığı önizleme özelliğini adı.

## <a name="regional-availability"></a>Bölgesel kullanılabilirlik

Aşağıdaki bölgelerde sanal düğüm dağıtımları için desteklenir:

* Avustralya Doğu (australiaeast)
* Doğu ABD (myresourcegroup)
* Batı Orta ABD (westcentralus)
* Batı Avrupa (westeurope)
* Batı ABD (westus)

## <a name="known-limitations"></a>Bilinen sınırlamalar
Sanal düğümler işlevleri ACI'ın özellik kümesi üzerinde bağımlılığa sahiptir. Aşağıdaki senaryolar ile sanal düğümü henüz desteklenmiyor

* Çekme ACR görüntülerine hizmet sorumlusu kullanma. [Geçici çözüm](https://github.com/virtual-kubelet/virtual-kubelet/blob/master/providers/azure/README.md#Private-registry) kullanmaktır [Kubernetes gizli dizileri](https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/#create-a-secret-by-providing-credentials-on-the-command-line)
* [Sanal ağ kısıtlamaları](../container-instances/container-instances-vnet.md) VNet eşlemesi, Kubernetes ağ ilkeleri ve ağ güvenlik grupları ile İnternet'e giden trafiği de dahil olmak üzere.
* Init kapsayıcıları
* [Konak diğer adları](https://kubernetes.io/docs/concepts/services-networking/add-entries-to-pod-etc-hosts-with-host-aliases/)
* [Bağımsız değişkenler](../container-instances/container-instances-exec.md#restrictions) ACI içindeki exec için
* [Daemonsets](concepts-clusters-workloads.md#statefulsets-and-daemonsets) pod'ların sanal düğüme dağıtma

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="create-an-aks-cluster"></a>AKS kümesi oluşturma

Azure portalının sol üst köşesinde bulunan **Create a resource** (Kaynak oluştur) > **Kubernetes Service** (Kubernetes Hizmeti) öğesini seçin.

Üzerinde **Temelleri** sayfasında, aşağıdaki seçenekleri yapılandırın:

- *PROJE AYRINTILARINI*: Bir Azure aboneliği seçin sonra seçin veya bir Azure kaynak grubu gibi oluşturma *myResourceGroup*. **Kubernetes kümesi adı** alanına *myAKSCluster* gibi bir ad girin.
- *KÜME AYRINTILARI*: Bölge, Kubernetes sürümü ve DNS adı ön eki için AKS kümesi seçin.
- *ÖLÇEK*: AKS düğümleri için VM boyutunu seçin. AKS kümesi dağıtıldıktan sonra, sanal makine boyutu **değiştirilemez**.
    - Kümeye dağıtılacak düğüm sayısını seçin. Bu makale için ayarlanmış **düğüm sayısı** için *1*. Küme dağıtıldıktan sonra düğüm sayısı **ayarlanabilir**.
    - Altında **sanal düğümler**seçin *etkin*.

![AKS kümesi oluşturma ve sanal düğümü etkinleştir](media/virtual-nodes-portal/enable-virtual-nodes.png)

Varsayılan olarak, bir Azure Active Directory Hizmet sorumlusu oluşturulur. Bu hizmet sorumlusu, küme iletişimi ve diğer Azure hizmetleriyle tümleştirme için kullanılır.

Küme için Gelişmiş Ağ de yapılandırılır. Sanal düğümler kendi Azure sanal ağ alt ağı kullanmak üzere yapılandırılmış. Bu alt ağ, Azure kaynakları arasında AKS kümeye bağlanmak için gerekli izinlere temsilci. Temsilci alt ağ yoksa, Azure portalında oluşturur ve sanal düğümü ile Azure sanal ağ ve alt kullanım için yapılandırır.

**İncele ve oluştur**’u seçin. Doğrulama tamamlandıktan sonra seçip **Oluştur**.

AKS kümesinin oluşturulması ve kullanıma hazır olması birkaç dakika sürer.

## <a name="connect-to-the-cluster"></a>Kümeye bağlanma

Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. Kubernetes kümesini yönetmek için Kubernetes komut satırı istemcisi [kubectl][kubectl]’i kullanın. `kubectl` istemcisi Azure Cloud Shell’de önceden yüklüdür.

Cloud Shell'i açmak için seçmeniz **deneyin** bir kod bloğunun sağ üst köşesinde öğesinden. İsterseniz [https://shell.azure.com/bash](https://shell.azure.com/bash) adresine giderek Cloud Shell'i ayrı bir tarayıcı sekmesinde de başlatabilirsiniz. **Kopyala**’yı seçerek kod bloğunu kopyalayın, Cloud Shell’e yapıştırın ve Enter tuşuna basarak çalıştırın.

[az aks get-credentials][az-aks-get-credentials] komutunu kullanarak, `kubectl` istemcisini Kubernetes kümenize bağlanacak şekilde yapılandırın. Aşağıdaki örnek *myResourceGroup* adlı kaynak grubu içindeki *myAKSCluster* adlı kümenin kimlik bilgilerini alır:

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

Kümenize bağlantıyı doğrulamak için [kubectl get][kubectl-get] komutunu kullanarak küme düğümleri listesini alın.

```azurecli-interactive
kubectl get nodes
```

Linux için oluşturulan tek VM düğümünden'i ve ardından sanal düğümü aşağıdaki örnek çıktı gösterilmektedir *sanal düğümü-aci-linux*:

```
$ kubectl get nodes

NAME                           STATUS    ROLES     AGE       VERSION
virtual-node-aci-linux         Ready     agent     28m       v1.11.2
aks-agentpool-14693408-0       Ready     agent     32m       v1.11.2
```

## <a name="deploy-a-sample-app"></a>Örnek bir uygulama dağıtma

Azure Cloud Shell'de adlı bir dosya oluşturun `virtual-node.yaml` aşağıdaki YAML'ye kopyalayın. Düğümde, kapsayıcı zamanlamak için bir [nodeSelector] [ node-selector] ve [toleration] [ toleration] tanımlanır. Bu ayarlar sanal düğümde zamanlanması ve özellik başarıyla etkin olduğunu onaylamak pod sağlar.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: aci-helloworld
spec:
  replicas: 1
  selector:
    matchLabels:
      app: aci-helloworld
  template:
    metadata:
      labels:
        app: aci-helloworld
    spec:
      containers:
      - name: aci-helloworld
        image: microsoft/aci-helloworld
        ports:
        - containerPort: 80
      nodeSelector:
        kubernetes.io/role: agent
        beta.kubernetes.io/os: linux
        type: virtual-kubelet
      tolerations:
      - key: virtual-kubelet.io/provider
        operator: Exists
      - key: azure.com/aci
        effect: NoSchedule
```

Uygulamayı çalıştırın [kubectl uygulamak] [ kubectl-apply] komutu.

```azurecli-interactive
kubectl apply -f virtual-node.yaml
```

Kullanım [kubectl pod'ları alma] [ kubectl-get] komutunu `-o wide` pod'ların ve zamanlanmış düğüm listesini çıkarmak için bağımsız değişken. Dikkat `virtual-node-helloworld` pod zamanlandı `virtual-node-linux` düğümü.

```
$ kubectl get pods -o wide

NAME                                     READY     STATUS    RESTARTS   AGE       IP           NODE
virtual-node-helloworld-9b55975f-bnmfl   1/1       Running   0          4m        10.241.0.4   virtual-node-aci-linux
```

Pod sanal düğümü ile kullanmak için temsilci Azure sanal ağ alt ağından iç IP adresi atanır.

> [!NOTE]
> Azure Container Registry'de depolanan görüntülerden kullanırsanız [yapılandırılır ve Kubernetes gizli][acr-aks-secrets]. Azure AD hizmet sorumlusu kimlik doğrulamasını kullanamazsınız, Önizleme aşamasında sanal düğümü geçerli bir kısıtlaması tümleşik. Gizli dizi kullanmazsanız, sanal düğümlerinde zamanlanmış pod'ları başlatmak ve hatayı bildirin başarısız `HTTP response status code 400 error code "InaccessibleImage"`.

## <a name="test-the-virtual-node-pod"></a>Sanal düğüm pod test

Sanal düğüm üzerinde çalışan bir pod test etmek için demo uygulamasını bir web istemcisi ile göz atın. Pod iç IP adresi atandığı gibi bu bağlantıyı başka bir AKS kümesi pod'u hızlıca test edebilirsiniz. Bir test pod oluşturun ve terminal oturumu ekler:

```azurecli-interactive
kubectl run -it --rm virtual-node-test --image=debian
```

Yükleme `curl` pod kullanarak `apt-get`:

```azurecli-interactive
apt-get update && apt-get install -y curl
```

Artık erişim pod kullanmanın adresi `curl`, gibi *http://10.241.0.4*. Önceki gösterilen kendi iç IP adresi verin `kubectl get pods` komutu:

```azurecli-interactive
curl -L http://10.241.0.4
```

Demo uygulamayı aşağıdaki sıkıştırılmış örneğe çıktıda gösterildiği gibi görüntülenir:

```
$ curl -L 10.241.0.4

<html>
<head>
  <title>Welcome to Azure Container Instances!</title>
</head>
[...]
```

Test pod ile terminal oturumunu kapatın `exit`. Oturumunuz sona pod silinmiş olur.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir pod sanal düğümde zamanlanmış ve bir özel, iç IP adresi atanır. Bunun yerine, bir yük dengeleyici veya giriş denetleyicisine üzerinden pod için bir hizmet dağıtımı ve trafiği yönlendirme oluşturabilirsiniz. Daha fazla bilgi için [AKS içinde bir temel giriş denetleyicisine oluşturma][aks-basic-ingress].

Sanal düğümler AKS ölçeklendirme bir çözümde bir bileşenidir. Ölçeklendirme çözümleri hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Kubernetes yatay pod otomatik ölçeklendiricinin kullanın][aks-hpa]
- [Kubernetes küme ölçeklendiriciyi kullanmak][aks-cluster-autoscaler]
- [Sanal düğümler için otomatik ölçeklendirme örnek denetleyin][virtual-node-autoscale]
- [Virtual Kubelet açık kaynak Kitaplığı hakkında daha fazla bilgi][virtual-kubelet-repo]

<!-- LINKS - external -->
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[node-selector]:https://kubernetes.io/docs/concepts/configuration/assign-pod-node/
[toleration]: https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/
[azure-cni]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md
[aks-github]: https://github.com/azure/aks/issues]
[virtual-node-autoscale]: https://github.com/Azure-Samples/virtual-node-autoscale
[virtual-kubelet-repo]: https://github.com/virtual-kubelet/virtual-kubelet

<!-- LINKS - internal -->
[aks-network]: ./networking-overview.md
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[aks-hpa]: tutorial-kubernetes-scale.md
[aks-cluster-autoscaler]: cluster-autoscaler.md
[aks-basic-ingress]: ingress-basic.md
[acr-aks-secrets]: ../container-registry/container-registry-auth-aks.md#access-with-kubernetes-secret
