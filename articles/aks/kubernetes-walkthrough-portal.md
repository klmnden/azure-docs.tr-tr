---
title: Hızlı başlangıç - Portalda Azure Kubernetes Service kümesi oluşturma
description: Azure portalı kullanarak hızlıca bir Azure Kubernetes Service (AKS) kümesi oluşturup ardından bir uygulama dağıtmayı ve izlemeyi öğrenin.
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: quickstart
ms.date: 09/24/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: fb1ef9c2bb448d74d447e647f6dc8122cda6e1f7
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52445771"
---
# <a name="quickstart-deploy-an-azure-kubernetes-service-aks-cluster"></a>Hızlı Başlangıç: Azure Kubernetes Hizmeti (AKS) kümesini dağıtma

Bu hızlı başlangıçta, Azure portalını kullanarak bir AKS kümesi dağıtırsınız. Ardından web ön ucu ve bir Redis örneğinden oluşan çok kapsayıcılı bir uygulama küme üzerinde çalıştırılır. Tamamlandığında, uygulamaya İnternet üzerinden erişilebilir.

![Azure Vote örnek uygulamasına göz atma görüntüsü](media/container-service-kubernetes-walkthrough/azure-vote.png)

Bu hızlı başlangıç, Kubernetes kavramlarının temel olarak bilindiğini varsayar. Kubernetes hakkında ayrıntılı bilgi için bkz. [Kubernetes belgeleri][kubernetes-documentation].

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

http://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="create-an-aks-cluster"></a>AKS kümesi oluşturma

Azure portalının sol üst köşesinde bulunan **Create a resource** (Kaynak oluştur) > **Kubernetes Service** (Kubernetes Hizmeti) öğesini seçin.

AKS kümesi oluşturmak için aşağıdaki adımları tamamlayın:

1. **Temel**: Aşağıdaki seçenekleri yapılandırın:
    - *PROJE AYRINTILARI*: Bir Azure aboneliği seçtikten sonra bir Azure kaynak grubu seçin veya *myResourceGroup* adıyla yeni bir tane oluşturun. **Kubernetes kümesi adı** alanına *myAKSCluster* gibi bir ad girin.
    - *KÜME AYRINTILARI*: AKS kümesi için bölge, Kubernetes sürümü ve DNS adı ön eki seçin.
    - *ÖLÇEK*: AKS düğümleri için bir sanal makine boyutu seçin. AKS kümesi dağıtıldıktan sonra, sanal makine boyutu **değiştirilemez**.
        - Kümeye dağıtılacak düğüm sayısını seçin. Bu hızlı başlangıç **Düğüm sayısı** değerini *1* olarak belirleyin. Küme dağıtıldıktan sonra düğüm sayısı **ayarlanabilir**.
    
    ![AKS kümesi oluşturma - temel bilgileri sağlama](media/kubernetes-walkthrough-portal/create-cluster-basics.png)

    Tamamladığınızda **İleri: Kimlik doğrulaması**'nı seçin.

1. **Kimlik Doğrulaması**: Aşağıdaki seçenekleri yapılandırın:
    - Yeni bir hizmet sorumlusu oluşturun veya *Yapılandır* seçeneğiyle mevcut bir hizmet sorumlusunu kullanın. Mevcut bir SPN kullanırken, SPN istemci kimliğini ve gizli dizisini sağlamanız gerekir.
    - Kubernetes rol tabanlı erişim denetimleri (RBAC) seçeneğini etkinleştirin. Bu denetimler AKS kümenize dağıtılmış olan Kubernetes kaynakları üzerinde daha ayrıntılı denetim sunar.

    Tamamlandığında, **Sonraki: Ağ** seçeneğini belirleyin.

1. **Ağ**: Aşağıdaki ağ seçeneklerini yapılandırın, bu değerlerin varsayılan olarak aşağıdaki şekilde ayarlanmış olması gerekir:
    
    - **Http uygulama yönlendirme**: Otomatik genel DNS adı oluşturma işlemiyle tümleşik bir giriş denetleyicisini yapılandırmak için **Evet**'i seçin. Http yönlendirme hakkında daha fazla bilgi için bkz. [AKS HTTP yönlendirme ve DNS][http-routing].
    - **Ağ yapılandırması**: [Azure CNI][azure-cni] ile gelişmiş ağ yapılandırması yerine [kubenet][kubenet] Kubernetes eklentisini kullanan **Temel** ağ iletişimini seçin. Ağ seçenekleri hakkında daha fazla bilgi için bkz. [AKS ağına genel bakış][aks-network].
    
    Tamamlandığında, **Sonraki: Ağ** seçeneğini belirleyin.

1. AKS kümesi dağıtılırken kapsayıcılar için Azure İzleyici, AKS kümesinin ve kümede çalıştırılan pod’ların durumunu izlemek için yapılandırılabilir. Küme durumu izleme hakkında daha fazla bilgi için bkz. [Azure Kubernetes Hizmeti durumunu izleme][aks-monitor].

    **Evet**’i seçerek kapsayıcı izlemeyi etkinleştirin ve mevcut bir Log Analytics çalışma alanı seçin veya yenisini oluşturun.
    
    **Gözden geçir + oluştur**’u seçin ve hazır olduğunuzda **Oluştur**’a tıklayın.

AKS kümesinin oluşturulması ve kullanıma hazır olması birkaç dakika sürer. *myResourceGroup* gibi bir AKS küme kaynağı grubuna göz atın ve *myAKSCluster* gibi bir AKS kaynağını seçin. Aşağıdaki örnek ekran görüntüsündeki gibi AKS küme panosu gösterilir:

![Azure portalda örnek AKS panosu](media/kubernetes-walkthrough-portal/aks-portal-dashboard.png)

## <a name="connect-to-the-cluster"></a>Kümeye bağlanma

Kubernetes kümesini yönetmek için Kubernetes komut satırı istemcisi [kubectl][kubectl]’i kullanın. `kubectl` istemcisi Azure Cloud Shell’de önceden yüklüdür.

Azure portalının sağ üst köşesindeki düğmeyi kullanarak Cloud Shell’i açın.

![Azure Cloud Shell'i portalda açma](media/kubernetes-walkthrough-portal/aks-cloud-shell.png)

[az aks get-credentials][az-aks-get-credentials] komutunu kullanarak, `kubectl` istemcisini Kubernetes kümenize bağlanacak şekilde yapılandırın. Aşağıdaki örnek *myResourceGroup* adlı kaynak grubu içindeki *myAKSCluster* adlı kümenin kimlik bilgilerini alır:

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

Kümenize bağlantıyı doğrulamak için [kubectl get][kubectl-get] komutunu kullanarak küme düğümleri listesini alın.

```azurecli-interactive
kubectl get nodes
```

Aşağıdaki örnekte önceki adımlarda oluşturulan tek düğüm gösterilmiştir.

```
NAME                       STATUS    ROLES     AGE       VERSION
aks-agentpool-14693408-0   Ready     agent     10m       v1.11.2
```

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Kubernetes bildirim dosyaları, hangi kapsayıcı görüntülerinin çalıştırılması gerektiği de dahil olmak üzere, küme için istenen durumu tanımlar. Bu hızlı başlangıçta Azure Vote örnek uygulamasını çalıştırmak için gerekli tüm nesneleri oluşturmak için bir bildirim kullanılır. Bu nesneler, biri Azure Vote ön ucu için, diğeri de Redis örneği için olmak üzere iki [Kubernetes dağıtımı][kubernetes-deployment] içerir. Ayrıca, Redis örneği için bir iç hizmet ve İnternet’ten Azure Vote uygulamasına erişmek için bir dış hizmet olmak üzere iki [Kubernetes Hizmeti][kubernetes-service] oluşturulur.

> [!TIP]
> Bu hızlı başlangıçta, uygulama bildirimlerini el ile oluşturup AKS kümesine dağıtacaksınız. Daha fazla gerçek dünya senaryolarında kodunuzu doğrudan AKS kümesinde hızlıca yineleyip hatalarını ayıklamak için [Azure Dev Spaces][azure-dev-spaces]’ı kullanabilirsiniz. Dev Spaces’ı işletim sistemi platformları ile geliştirme ortamlarında kullanabilir ve ekibinizdeki diğer kişilerle birlikte çalışabilirsiniz.

`azure-vote.yaml` adlı bir dosya oluşturun ve dosyayı aşağıdaki YAML koduna kopyalayın. Azure Cloud Shell'de çalışıyorsanız, bu dosyayı bir sanal veya fiziksel sistemde olduğu gibi `vi` veya `Nano` kullanarak oluşturabilirsiniz.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: azure-vote-back
spec:
  replicas: 1
  selector:
    matchLabels:
      app: azure-vote-back
  template:
    metadata:
      labels:
        app: azure-vote-back
    spec:
      containers:
      - name: azure-vote-back
        image: redis
        resources:
          requests:
            cpu: 100m
            memory: 128Mi
          limits:
            cpu: 250m
            memory: 256Mi
        ports:
        - containerPort: 6379
          name: redis
---
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-back
spec:
  ports:
  - port: 6379
  selector:
    app: azure-vote-back
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: azure-vote-front
spec:
  replicas: 1
  selector:
    matchLabels:
      app: azure-vote-front
  template:
    metadata:
      labels:
        app: azure-vote-front
    spec:
      containers:
      - name: azure-vote-front
        image: microsoft/azure-vote-front:v1
        resources:
          requests:
            cpu: 100m
            memory: 128Mi
          limits:
            cpu: 250m
            memory: 256Mi
        ports:
        - containerPort: 80
        env:
        - name: REDIS
          value: "azure-vote-back"
---
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-front
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: azure-vote-front
```

Uygulamayı çalıştırmak için [kubectl apply][kubectl-apply] komutunu kullanın.

```azurecli-interactive
kubectl apply -f azure-vote.yaml
```

Aşağıdaki örnek çıktı AKS kümenizde oluşturulan Kubernetes kaynağını göstermektedir:

```
deployment "azure-vote-back" created
service "azure-vote-back" created
deployment "azure-vote-front" created
service "azure-vote-front" created
```

## <a name="test-the-application"></a>Uygulamayı test etme

Uygulama çalıştırıldığında, uygulamayı İnternet üzerinden kullanıma sunmak için bir [Kubernetes hizmeti][kubernetes-service] oluşturulur. Bu işlemin tamamlanması birkaç dakika sürebilir.

İlerleme durumunu izlemek için [kubectl get service][kubectl-get] komutunu `--watch` bağımsız değişkeniyle birlikte kullanın.

```azurecli-interactive
kubectl get service azure-vote-front --watch
```

Başlangıçta *azure-vote-front* için *EXTERNAL-IP* durumu *pending* olarak görünür.

```
NAME               TYPE           CLUSTER-IP   EXTERNAL-IP   PORT(S)        AGE
azure-vote-front   LoadBalancer   10.0.37.27   <pending>     80:30572/TCP   6s
```

*EXTERNAL-IP* adresi *pending* durumundan *IP address* değerine değiştiğinde kubectl izleme işlemini durdurmak için `CTRL-C` komutunu kullanın.

```
azure-vote-front   LoadBalancer   10.0.37.27   52.179.23.131   80:30572/TCP   2m
```

Azure Vote uygulamanızı görmek için aşağıdaki örnekte gösterildiği gibi hizmetinizin dış IP adresini bir web tarayıcısından açın:

![Azure Vote örnek uygulamasına göz atma görüntüsü](media/container-service-kubernetes-walkthrough/azure-vote.png)

## <a name="monitor-health-and-logs"></a>Sistem durumunu ve günlükleri izleme

Kapsayıcı içgörüleri izleme özelliği kümeyi oluşturduğunuzda etkinleştirilmiştir. Bu izleme özelliği hem AKS kümesi hem de kümede çalışan pod'lar için sistem durumu ölçümleri sağlar. Küme durumu izleme hakkında daha fazla bilgi için bkz. [Azure Kubernetes Hizmeti durumunu izleme][aks-monitor].

Azure portalında bu verilerin doldurulması birkaç dakika sürebilir. Azure Vote pod'larının geçerli durumunu, çalışma süresini ve kaynak kullanımını görmek için Azure portalda *myAKSCluster* gibi bir AKS kaynağına geri gidin. Sistem durumuna aşağıdaki şekilde erişebilirsiniz:

1. Sol taraftaki **İzleme** bölümünde **İçgörüler (önizleme)** öğesini seçin
1. Üst taraftan **+ Filtre Ekle**'yi seçin
1. *Ad alanı* özelliğini ve ardından *\<kube-system hariç tümünü\> seçin*
1. **Kapsayıcılar** bölümünü görüntüleyin.

Aşağıdaki örnekte olduğu gibi *azure-vote-back* ve *azure-vote-front* kapsayıcıları görüntülenir:

![AKS'de çalışan kapsayıcıların durumunu görüntüleme](media/kubernetes-walkthrough-portal/monitor-containers.png)

`azure-vote-front` pod'unun günlüklerini görmek için kapsayıcı listesinin sağ tarafındaki **Kapsayıcı günlüklerini görüntüle** bağlantısını seçin. Bu günlükler, kapsayıcıdaki *stdout* ve *stderr* akışlarını içerir.

![AKS'deki kapsayıcı günlüklerini görüntüleme](media/kubernetes-walkthrough-portal/monitor-container-logs.png)

## <a name="delete-cluster"></a>Kümeyi silme

Küme artık gerekli olmadığında, tüm ilişkili kaynaklarla birlikte küme kaynağını silin. AKS kümesi panosunda **Sil** düğmesi seçilerek Azure portalında bu işlem tamamlanabilir. Alternatif olarak, Cloud Shell’de [az aks delete][az-aks-delete] komutu kullanılabilir:

```azurecli-interactive
az aks delete --resource-group myResourceGroup --name myAKSCluster --no-wait
```

> [!NOTE]
> Kümeyi sildiğinizde, AKS kümesi tarafından kullanılan Azure Active Directory hizmet sorumlusu kaldırılmaz. Hizmet sorumlusunu kaldırma adımları için bkz. [AKS hizmet sorumlusuyla ilgili önemli noktalar ve silme][sp-delete].

## <a name="get-the-code"></a>Kodu alma

Bu hızlı başlangıçta, Kubernetes dağıtımı oluşturmak için önceden oluşturulmuş kapsayıcı görüntüleri kullanılır. İlgili uygulama kodu, Dockerfile ve Kubernetes bildirim dosyası GitHub'da bulunur.

[https://github.com/Azure-Samples/azure-voting-app-redis][azure-vote-app]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir Kubernetes kümesi dağıtıp ve bu kümeye çok kapsayıcılı bir uygulama dağıttınız.

AKS hakkında daha fazla bilgi ve dağıtım örneği için tam kod açıklaması için Kubernetes küme öğreticisine geçin.

> [!div class="nextstepaction"]
> [AKS öğreticisi][aks-tutorial]

<!-- LINKS - external -->
[azure-vote-app]: https://github.com/Azure-Samples/azure-voting-app-redis.git
[azure-cni]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubenet]: https://kubernetes.io/docs/concepts/cluster-administration/network-plugins/#kubenet
[kubernetes-deployment]: https://kubernetes.io/docs/concepts/workloads/controllers/deployment/
[kubernetes-documentation]: https://kubernetes.io/docs/home/
[kubernetes-service]: https://kubernetes.io/docs/concepts/services-networking/service/

<!-- LINKS - internal -->
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[az-aks-delete]: /cli/azure/aks#az-aks-delete
[aks-monitor]: ../monitoring/monitoring-container-health.md
[aks-network]: ./concepts-network.md
[aks-tutorial]: ./tutorial-kubernetes-prepare-app.md
[http-routing]: ./http-application-routing.md
[sp-delete]: kubernetes-service-principal.md#additional-considerations
[azure-dev-spaces]: https://docs.microsoft.com/azure/dev-spaces/
