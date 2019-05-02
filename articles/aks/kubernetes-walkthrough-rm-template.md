---
title: Hızlı Başlangıç - Azure Kubernetes Service (AKS) kümesi oluşturma
description: Hızla bir Azure Resource Manager şablonu kullanarak bir Kubernetes kümesi oluşturmayı öğrenin ve Azure Kubernetes Service (AKS) uygulama dağıtma
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: quickstart
ms.date: 04/19/2019
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 524eb97a2c865a14800cf503edd7f506151521bb
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64920201"
---
# <a name="quickstart-deploy-an-azure-kubernetes-service-aks-cluster-using-an-azure-resource-manager-template"></a>Hızlı Başlangıç: Bir Azure Resource Manager şablonu kullanarak bir Azure Kubernetes Service (AKS) kümesini dağıtma

Azure Kubernetes Service (AKS) hızla dağıtın ve kümelerini yönetme sağlayan yönetilen bir Kubernetes hizmetidir. Bu hızlı başlangıçta, Azure Resource Manager şablonu kullanarak bir AKS kümesi dağıtın. Bir web ön ucu ve bir Redis örneğinden oluşan çok kapsayıcılı bir uygulama, kümede çalıştırılır.

![Azure Vote’a göz atma görüntüsü](media/container-service-kubernetes-walkthrough/azure-vote.png)

Bu hızlı başlangıç, Kubernetes kavramlarının temel olarak bilindiğini varsayar. Daha fazla bilgi için [Kubernetes kavramlarını Azure Kubernetes Service (AKS) için çekirdek][kubernetes-concepts].

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı seçerseniz bu hızlı başlangıçta Azure CLI Sürüm 2.0.61 çalıştırdığınız gerektirir veya üzeri. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli-install].

## <a name="prerequisites"></a>Önkoşullar

Resource Manager şablonu kullanarak bir AKS kümesi oluşturmak için bir SSH ortak anahtarını ve Azure Active Directory Hizmet sorumlusu sağlar. Bu kaynakların gerekiyorsa aşağıdaki bölüme bakın; Aksi takdirde atlamak [AKS kümesi oluşturma](#create-an-aks-cluster) bölümü.

### <a name="create-an-ssh-key-pair"></a>SSH anahtar çifti oluşturma

AKS düğümlerini erişmek için bir SSH anahtar çiftini kullanarak bağlanın. Kullanım `ssh-keygen` SSH ortak ve özel anahtar dosyaları oluşturmak için komutu. Varsayılan olarak, bu dosyaları oluşturulan *~/.ssh* dizin. Belirtilen konumda aynı ada sahip bir SSH anahtar çiftiniz varsa bu dosyaların üzerine yazılır.

Aşağıdaki komut, RSA şifreleme ve 2048 bit uzunluğu kullanarak SSH anahtar çifti oluşturur:

```azurecli-interactive
ssh-keygen -t rsa -b 2048
```

SSH anahtarları oluşturma hakkında daha fazla bilgi için bkz. [oluşturun ve Azure kimlik doğrulaması için SSH anahtarları yönetme][ssh-keys].

### <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma

Bir AKS kümesinin diğer Azure kaynaklarıyla etkileşime geçmesini sağlamak için bir Azure Active Directory hizmet sorumlusu kullanılır. Kullanarak bir hizmet sorumlusu oluşturma [az ad sp create-for-rbac] [ az-ad-sp-create-for-rbac] komutu. `--skip-assignment` parametresi, ek izinlerin atanmasını engeller. Varsayılan olarak, bu hizmet sorumlusunun bir yıl süreyle geçerlidir.

```azurecli-interactive
az ad sp create-for-rbac --skip-assignment
```

Çıktı aşağıdaki örneğe benzer:

```json
{
  "appId": "8b1ede42-d407-46c2-a1bc-6b213b04295f",
  "displayName": "azure-cli-2019-04-19-21-42-11",
  "name": "http://azure-cli-2019-04-19-21-42-11",
  "password": "27e5ac58-81b0-46c1-bd87-85b4ef622682",
  "tenant": "73f978cf-87f2-41bf-92ab-2e7ce012db57"
}
```

*appId* ve *password* değerlerini not edin. Bu değerler aşağıdaki adımlarda kullanılacaktır.

## <a name="create-an-aks-cluster"></a>AKS kümesi oluşturma

Bu hızlı başlangıçta kullanılan şablonu [Azure Kubernetes Service kümesini dağıtma](https://azure.microsoft.com/resources/templates/101-aks/). Daha fazla AKS örnekleri için bkz [AKS hızlı başlangıç şablonları] [ aks-quickstart-templates] site.

1. Aşağıdaki görüntüyü seçerek Azure'da oturum açıp bir şablon açın.

    [![Azure’a dağıtma](./media/kubernetes-walkthrough-rm-template/deploy-to-azure.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-aks%2Fazuredeploy.json)

2. Aşağıdaki değerleri seçin veya girin.  

    Bu hızlı başlangıçta, varsayılan değerleri bırakın *işletim sistemi Disk boyutu GB*, *aracı sayısı*, *aracı VM boyutu*, *işletim sistemi türü*ve *Kubernetes sürümü*. Aşağıdaki şablon parametreleri için kendi değerlerinizi sağlayın:

    * **Abonelik**: Bir Azure aboneliği seçin.
    * **Kaynak grubu**: **Yeni oluştur**’u seçin. Kaynak grubu için benzersiz bir ad girmeniz *myResourceGroup*, ardından **Tamam**.
    * **Konum**: Gibi bir konum seçin **Doğu ABD**.
    * **Küme adı**: AKS kümesi için benzersiz bir ad girmeniz *myAKSCluster*.
    * **DNS ön eki**: Kümeniz için benzersiz bir DNS ön eki girin *myakscluster*.
    * **Linux Yöneticisi kullanıcı adı**: SSH kullanarak bağlamak için bir kullanıcı adı girin *azureuser*.
    * **SSH RSA ortak anahtarı**: Kopyalama ve yapıştırma *genel* SSH anahtar çiftinizi parçası (varsayılan olarak, içeriğini *~/.ssh/id_rsa.pub*).
    * **Hizmet sorumlusu istemci kimliği**: Kopyalama ve yapıştırma *AppID* hizmetinizin gelen asıl `az ad sp create-for-rbac` komutu.
    * **Hizmet sorumlusu istemci parolası**: Kopyalama ve yapıştırma *parola* hizmetinizin gelen asıl `az ad sp create-for-rbac` komutu.
    * **Yukarıdaki hüküm ve koşulları durumu için kabul ediyorum**: Kabul etmek için bu kutuyu işaretleyin.

    ![Portalda Azure Kubernetes Service kümesi oluşturmak için resource Manager şablonu](./media/kubernetes-walkthrough-rm-template/create-aks-cluster-using-template-portal.png)

3. **Satın al**'ı seçin.

AKS kümesi oluşturmak için birkaç dakika sürer. Kümenin sonraki adıma geçmeden önce sorunsuz dağıtılması bekleyin.

## <a name="connect-to-the-cluster"></a>Kümeye bağlanma

Bir Kubernetes kümesini yönetmek için kullandığınız [kubectl][kubectl], Kubernetes komut satırı istemcisi. Azure Cloud Shell kullanıyorsanız `kubectl` zaten yüklü. Yüklenecek `kubectl` yerel olarak, [az aks yükleme-cli] [ az-aks-install-cli] komutu:

```azurecli
az aks install-cli
```

`kubectl` istemcisini Kubernetes kümenize bağlanacak şekilde yapılandırmak için [az aks get-credentials][az-aks-get-credentials] komutunu kullanın. Bu komut, kimlik bilgilerini indirir ve Kubernetes CLI'yi bunları kullanacak şekilde yapılandırır.

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

Kümenize bağlantıyı doğrulamak için [kubectl get][kubectl-get] komutunu kullanarak küme düğümleri listesini alın.

```azurecli-interactive
kubectl get nodes
```

Aşağıdaki örnek çıktıda önceki adımlarda oluşturulan düğümleri gösterir. Tüm düğümlerinin durumunu olduğundan emin olun *hazır*:

```
NAME                       STATUS   ROLES   AGE     VERSION
aks-agentpool-41324942-0   Ready    agent   6m44s   v1.12.6
aks-agentpool-41324942-1   Ready    agent   6m46s   v1.12.6
aks-agentpool-41324942-2   Ready    agent   6m45s   v1.12.6
```

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Kubernetes bildirim dosyası çalıştırmak için hangi kapsayıcı görüntüleri gibi küme için istenen durumu tanımlar. Bu hızlı başlangıçta, Azure Vote uygulamasını çalıştırmak için gerekli tüm nesneleri oluşturmak için bir bildirim kullanılır. Bu bildirimi iki içerir [Kubernetes dağıtımları] [ kubernetes-deployment] -örnek Azure Vote Python uygulamaları ve diğer Redis örneği için. İki [Kubernetes Hizmetleri] [ kubernetes-service] ayrıca oluşturulur - Redis örneği için bir iç hizmet ve internet'ten Azure Vote uygulaması erişmek için bir dış hizmet.

> [!TIP]
> Bu hızlı başlangıçta, uygulama bildirimlerini el ile oluşturup AKS kümesine dağıtacaksınız. Daha fazla gerçek dünya senaryolarında kodunuzu doğrudan AKS kümesinde hızlıca yineleyip hatalarını ayıklamak için [Azure Dev Spaces][azure-dev-spaces]’ı kullanabilirsiniz. Dev Spaces’ı işletim sistemi platformları ile geliştirme ortamlarında kullanabilir ve ekibinizdeki diğer kişilerle birlikte çalışabilirsiniz.

Adlı bir dosya oluşturun `azure-vote.yaml` aşağıdaki YAML tanımı kopyalayın. Azure Cloud Shell'i kullanırsanız, bu dosya kullanılarak oluşturulabilir `vi` veya `nano` bir sanal veya fiziksel sistemde olarak çalışıyorsanız:

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
      nodeSelector:
        "beta.kubernetes.io/os": linux
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
      nodeSelector:
        "beta.kubernetes.io/os": linux
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

Kullanarak uygulamayı dağıtma [kubectl uygulamak] [ kubectl-apply] komut ve YAML bildiriminizi adını belirtin:

```azurecli-interactive
kubectl apply -f azure-vote.yaml
```

Aşağıdaki örnek çıktıda, Hizmetleri başarıyla oluşturuldu ve dağıtımları gösterir:

```
deployment "azure-vote-back" created
service "azure-vote-back" created
deployment "azure-vote-front" created
service "azure-vote-front" created
```

## <a name="test-the-application"></a>Uygulamayı test etme

Uygulama çalıştırıldığında, uygulama ön ucu İnternet'e bir Kubernetes hizmeti sunar. Bu işlemin tamamlanması birkaç dakika sürebilir.

İlerleme durumunu izlemek için [kubectl get service][kubectl-get] komutunu `--watch` bağımsız değişkeniyle birlikte kullanın.

```azurecli-interactive
kubectl get service azure-vote-front --watch
```

Başlangıçta *EXTERNAL-IP* için *azure-vote-front* hizmet olarak gösterildiği *bekleyen*.

```
NAME               TYPE           CLUSTER-IP   EXTERNAL-IP   PORT(S)        AGE
azure-vote-front   LoadBalancer   10.0.37.27   <pending>     80:30572/TCP   6s
```

Zaman *EXTERNAL-IP* adresi değişirse *bekleyen* gerçek genel IP adresi için `CTRL-C` durdurmak için `kubectl` işlemi izleyin. Hizmete atanan geçerli genel IP adresi aşağıdaki örnek çıktı gösterilmektedir:

```
azure-vote-front   LoadBalancer   10.0.37.27   52.179.23.131   80:30572/TCP   2m
```

Uygulamada Azure Vote uygulamasını görmek için hizmetin dış IP adresini bir web tarayıcısı açın.

![Azure Vote’a göz atma görüntüsü](media/container-service-kubernetes-walkthrough/azure-vote.png)

## <a name="delete-cluster"></a>Kümeyi silme

Kümeye artık ihtiyacınız yoksa [az group delete][az-group-delete] komutunu kullanarak kaynak grubunu, kapsayıcı hizmetini ve ilgili tüm kaynakları kaldırın.

```azurecli-interactive
az group delete --name myResourceGroup --yes --no-wait
```

> [!NOTE]
> Kümeyi sildiğinizde, AKS kümesi tarafından kullanılan Azure Active Directory hizmet sorumlusu kaldırılmaz. Hizmet sorumlusunu kaldırma adımları için bkz. [AKS hizmet sorumlusuyla ilgili önemli noktalar ve silme][sp-delete].

## <a name="get-the-code"></a>Kodu alma

Bu hızlı başlangıçta, Kubernetes dağıtımı oluşturmak için önceden oluşturulmuş kapsayıcı görüntüleri kullanılan. İlgili uygulama kodu, Dockerfile ve Kubernetes bildirim dosyası GitHub'da bulunur.

[https://github.com/Azure-Samples/azure-voting-app-redis][azure-vote-app]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir Kubernetes kümesi dağıtıp ve bu kümeye çok kapsayıcılı bir uygulama dağıttınız. [Kubernetes web panosuna erişme] [ kubernetes-dashboard] , oluşturduğunuz küme için.

AKS hakkında daha fazla bilgi ve dağıtım örneği için tam kod açıklaması için Kubernetes küme öğreticisine geçin.

> [!div class="nextstepaction"]
> [AKS öğreticisi][aks-tutorial]

<!-- LINKS - external -->
[azure-vote-app]: https://github.com/Azure-Samples/azure-voting-app-redis.git
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[azure-dev-spaces]: https://docs.microsoft.com/azure/dev-spaces/
[aks-quickstart-templates]: https://azure.microsoft.com/resources/templates/?term=Azure+Kubernetes+Service

<!-- LINKS - internal -->
[kubernetes-concepts]: concepts-clusters-workloads.md
[aks-monitor]: https://aka.ms/coingfonboarding
[aks-tutorial]: ./tutorial-kubernetes-prepare-app.md
[az-aks-browse]: /cli/azure/aks?view=azure-cli-latest#az-aks-browse
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[az-aks-install-cli]: /cli/azure/aks?view=azure-cli-latest#az-aks-install-cli
[az-group-create]: /cli/azure/group#az-group-create
[az-group-delete]: /cli/azure/group#az-group-delete
[azure-cli-install]: /cli/azure/install-azure-cli
[sp-delete]: kubernetes-service-principal.md#additional-considerations
[azure-portal]: https://portal.azure.com
[kubernetes-deployment]: concepts-clusters-workloads.md#deployments-and-yaml-manifests
[kubernetes-service]: concepts-network.md#services
[kubernetes-dashboard]: kubernetes-dashboard.md
[ssh-keys]: ../virtual-machines/linux/create-ssh-keys-detailed.md
[az-ad-sp-create-for-rbac]: /cli/azure/ad/sp#az-ad-sp-create-for-rbac
