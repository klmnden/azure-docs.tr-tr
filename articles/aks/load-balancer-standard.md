---
title: Önizleme - standart SKU yük dengeleyicide Azure Kubernetes Service (AKS) kullanma
description: Hizmetlerinizi Azure Kubernetes Service (AKS) ile kullanıma sunmak için standart bir SKU ile bir yük dengeleyici kullanmayı öğrenin.
services: container-service
author: zr-msft
ms.service: container-service
ms.topic: article
ms.date: 06/25/2019
ms.author: zarhoads
ms.openlocfilehash: a9cf3db3a15fab5a2f067a146950e02923a20379
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67476803"
---
# <a name="preview---use-a-standard-sku-load-balancer-in-azure-kubernetes-service-aks"></a>Önizleme - standart SKU yük dengeleyicide Azure Kubernetes Service (AKS) kullanma

Azure Kubernetes Service (AKS) uygulamalarınıza erişim sağlamak için oluşturabilir ve Azure Load Balancer'ı kullanın. AKS'de çalışan bir yük dengeleyici, iç veya dış yük dengeleyici olarak kullanılabilir. İç yük dengeleyici, bir Kubernetes hizmeti yalnızca AKS kümesi aynı sanal ağda çalışan uygulamalar için erişilebilir hale getirir. Bir dış yük dengeleyici, giriş için bir veya daha fazla genel IP'ler alır ve bir Kubernetes hizmeti dışarıdan genel IP'ler kullanarak erişilebilir hale getirir.

Azure Load Balancer iki SKU ile - kullanılabilir *temel* ve *standart*. Varsayılan olarak, *temel* SKU AKS üzerinde bir yük dengeleyici oluşturmak için bir hizmet bildirimi kullanıldığında kullanılır. Kullanarak bir *standart* SKU yük dengeleyicide ek özellikleri ve daha büyük arka uç havuzu boyutunda ve kullanılabilirlik bölgeleri gibi işlevleri sağlar. Arasındaki farkları anlamak önemlidir *standart* ve *temel* yük Dengeleyiciler kullanılacağı seçmeden önce. Bir AKS kümesi oluşturduktan sonra bu küme için yük dengeleyici SKU değiştirilemiyor. Daha fazla bilgi için *temel* ve *standart* SKU'ları, bkz: [Azure yük dengeleyici SKU karşılaştırma][azure-lb-comparison].

Bu makalede bir Azure Load Balancer ile oluşturup kullanacağınızı gösterilmektedir *standart* SKU ile Azure Kubernetes Service (AKS).

Bu makalede, bir temel kavramlarını Kubernetes ve Azure Load Balancer varsayılır. Daha fazla bilgi için [Kubernetes kavramlarını Azure Kubernetes Service (AKS) için çekirdek][kubernetes-concepts] and [What is Azure Load Balancer?][azure-lb].

Bu özellik şu anda önizleme sürümündedir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale, Azure CLI Sürüm 2.0.59 çalıştırdığınız gerekir veya üzeri. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme][install-azure-cli].

## <a name="before-you-begin"></a>Başlamadan önce

AKS kümesi hizmet sorumlusu bir mevcut alt ağı veya kaynak grubu kullanıyorsanız, ağ kaynaklarını yönetmek için izniniz gerekiyor. Genel olarak, Ata *ağ Katılımcısı* rolüne temsilci kaynaklar, hizmet sorumlusu. İzinler hakkında daha fazla bilgi için bkz. [temsilci AKS için diğer Azure kaynaklarına erişim][aks-sp].

SKU yük dengeleyici için ayarlar bir AKS kümesi oluşturmalısınız *standart* varsayılan yerine *temel*. Bir AKS kümesi oluşturma, bir sonraki adımda ele alınmıştır, ancak ilk birkaç Önizleme özelliklerini etkinleştirmek gerekir.

> [!IMPORTANT]
> AKS Önizleme özellikleri, Self Servis, kabul etme. Görüş ve hata topluluğumuza toplamak için sağlanır. Önizleme'de, bu özelliklerin üretim kullanılmak üzere geliştirilmiş değildir. Genel Önizleme Özellikleri 'en yüksek çaba' destek kapsamında ayrılır. İş saatleri Pasifik Saat dilimi sırasında (Pasifik Saati) yalnızca AKS teknik destek ekipleri Yardım kullanılabilir. Ek bilgi için lütfen aşağıdaki destek makaleleri bakın:
>
> * [AKS destek ilkeleri][aks-support-policies]
> * [Azure desteği SSS][aks-faq]

### <a name="install-aks-preview-cli-extension"></a>Aks önizlemesini CLI uzantısını yükleme

Azure load balancer'ı kullanmak için standart SKU ihtiyacınız *aks önizlemesini* CLI 0.4.1 uzantı sürümü veya üzeri. Yükleme *aks önizlemesini* uzantısını Azure CLI kullanarak [az uzantısı ekleme][az-extension-add] command, then check for any available updates using the [az extension update][az-extension-update] komut::

```azurecli-interactive
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```

### <a name="register-aksazurestandardloadbalancer-preview-feature"></a>AKSAzureStandardLoadBalancer önizleme özelliği Kaydet

Bir yük dengeleyici ile kullanabileceğiniz bir AKS kümesi oluşturma *standart* SKU, etkinleştirmelidir *AKSAzureStandardLoadBalancer* özelliği aboneliğinizde bayrağı. *AKSAzureStandardLoadBalancer* özelliği de kullandığı *VMSSPreview* sanal makine ölçek kümeleri'ni kullanarak bir kümeyi oluştururken. Bu özellik, bir küme yapılandırırken en son hizmet geliştirmeleri kümesini sağlar. Zorunlu olsa da etkinleştirmeniz önerilir *VMSSPreview* özelliği de bayrağı.

> [!CAUTION]
> Bir Abonelikteki bir özellik kaydettiğinizde, bu özellik şu anda kaydını yapamazsınız. Bazı Önizleme özellikleri etkinleştirdikten sonra varsayılan ardından aboneliği için oluşturulan tüm AKS kümeleri için kullanılabilir. Önizleme özellikleri üretim Aboneliklerde etkinleştirmeyin. Önizleme özellikleri test ve geri bildirim toplamak için ayrı bir abonelik kullanın.

Kayıt *VMSSPreview* ve *AKSAzureStandardLoadBalancer* özellik bayraklarını kullanarak [az özelliği kayıt][az-feature-register] komutu aşağıdaki örnekte gösterildiği gibi:

```azurecli-interactive
az feature register --namespace "Microsoft.ContainerService" --name "VMSSPreview"
az feature register --namespace "Microsoft.ContainerService" --name "AKSAzureStandardLoadBalancer"
```

> [!NOTE]
> Başarıyla kaydettikten sonra oluşturduğunuz herhangi bir AKS kümesinde *VMSSPreview* veya *AKSAzureStandardLoadBalancer* özellik bayrakları Bu önizleme küme deneyimi kullanın. Normal, tam olarak desteklenen kümeleri oluşturmak devam etmek için üretim Aboneliklerde Önizleme özelliklerini etkinleştirme. Önizleme özellikleri test için ayrı bir test veya geliştirme Azure aboneliği kullanın.

Gösterilecek durum için birkaç dakika sürer *kayıtlı*. Kayıt kullanarak durumu denetleyebilirsiniz [az özellik listesi][az-feature-list] komutu:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/VMSSPreview')].{Name:name,State:properties.state}"
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/AKSAzureStandardLoadBalancer')].{Name:name,State:properties.state}"
```

Hazır olduğunuzda, kayıt yenileme *Microsoft.ContainerService* kullanarak kaynak sağlayıcısını [az provider register][az-provider-register] komutu:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

### <a name="limitations"></a>Sınırlamalar

Oluştururken ve bir yük dengeleyici ile destekleyen AKS kümeleri yönetme aşağıdaki sınırlamalar geçerlidir *standart* SKU:

* Kullanırken *standart* SKU yük dengeleyici, genel adresleri izin ve gerekir IP oluşturma engeller herhangi bir Azure İlkesi oluşturmaktan kaçının. AKS kümesi otomatik olarak oluşturur bir *standart* genellikle adlı AKS kümesi için SKU ortak IP'sine aynı kaynak grubunda oluşturulan *MC_* başında. AKS atar genel IP'ye *standart* SKU yük dengeleyici. Genel IP, AKS kümesi çıkış trafiği izin vermek için gereklidir. Bu genel IP, AKS önceki sürümleriyle uyumluluğu korumak için farklı denetim düzlemi ve aracı düğümleri de arasındaki bağlantıyı sürdürmek için de gereklidir.
* Kullanırken *standart* SKU yük dengeleyici için Kubernetes sürümü 1.13.5 kullanmanız gerekir ya da daha büyük.

Bu özellik Önizleme aşamasında olduğu sürece, aşağıdaki ek kısıtlamalar uygulanır:

* Kullanırken *standart* SKU yük dengeleyici aks'deki çıkış için kendi genel IP adresi yük dengeleyici için ayarlanamaz. AKS için load balancer'ınız atar IP adresini kullanmanız gerekir.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği mantıksal bir gruptur. Bir kaynak grubu oluştururken konum belirtmeniz istenir. Bu kaynak grubu meta verilerini depolandığı bir konumdur başka bir bölgede kaynak oluşturma sırasında belirtmezseniz kaynaklarınızı Azure üzerinde çalıştırdığı de olabilir. Kullanarak bir kaynak grubu oluşturmanız [az grubu oluşturma][az-group-create] komutu.

Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Aşağıdaki örnek çıkış, başarılı bir şekilde oluşturduğunuz kaynak grubunda gösterir:

```json
{
  "id": "/subscriptions/<guid>/resourceGroups/myResourceGroup",
  "location": "eastus",
  "managedBy": null,
  "name": "myResourceGroup",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null,
  "type": null
}
```

## <a name="create-aks-cluster"></a>AKS kümesi oluşturma
Bir yük dengeleyici ile destekleyen bir AKS kümesi çalıştırmak için *standart* SKU, kümeniz için ayarlamak gerekli *yük dengeleyici sku* parametresi *standart*. Bu parametre ile bir yük dengeleyici oluşturur *standart* , küme oluşturulduğunda SKU. Çalıştırdığınızda bir *LoadBalancer* hizmet yapılandırmasını kümenizdeki *standart* SK yük dengeleyici, hizmetin yapılandırma ile güncelleştirilir. Kullanım [az aks oluşturma][az-aks-create] adlı bir AKS kümesi oluşturmak için komut *myAKSCluster*.

> [!NOTE]
> *Yük dengeleyici sku* özelliği, yalnızca, küme oluşturulduğunda kullanılabilir. Bir AKS kümesi oluşturulduktan sonra Yük Dengeleyici SKU değiştirilemiyor. Ayrıca, yalnızca tek bir kümede yük dengeleyicinin SKU bir türü kullanabilirsiniz.

```azurecli-interactive
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --enable-vmss \
    --node-count 1 \
    --kubernetes-version 1.14.0 \
    --load-balancer-sku standard \
    --generate-ssh-keys
```

Birkaç dakika sonra komut tamamlanır ve küme hakkında JSON ile biçimlendirilmiş bilgiler döndürür.

## <a name="connect-to-the-cluster"></a>Kümeye bağlanma

Bir Kubernetes kümesini yönetmek için kullandığınız [kubectl][kubectl], Kubernetes komut satırı istemcisi. Azure Cloud Shell kullanıyorsanız `kubectl` zaten yüklü. Yüklenecek `kubectl` yerel olarak, [az aks yükleme-cli][az-aks-install-cli] komutu:

```azurecli
az aks install-cli
```

Yapılandırmak için `kubectl` Kubernetes kümenize bağlanmak için [az aks get-credentials][az-aks-get-credentials] komutu. Bu komut, kimlik bilgilerini indirir ve Kubernetes CLI'yi bunları kullanacak şekilde yapılandırır.

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

Kümenize bağlantıyı doğrulamak için [kubectl get][kubectl-get] komutunu kullanarak küme düğümleri listesini alın.

```azurecli-interactive
kubectl get nodes
```

Aşağıdaki örnekte önceki adımlarda oluşturulan tek düğüm gösterilmiştir. Düğüm durumu olduğundan emin olun *hazır*:

```
NAME                       STATUS   ROLES   AGE     VERSION
aks-nodepool1-31718369-0   Ready    agent   6m44s   v1.14.0
```

## <a name="verify-your-cluster-uses-the-standard-sku"></a>Kümenizi kullanan doğrulayın *standart* SKU

Kullanım [az aks show][az-aks-show] kümenizin yapılandırmasını görüntülemek için.

```console
$ az aks show --resource-group myResourceGroup --name myAKSCluster

{
  "aadProfile": null,
  "addonProfiles": null,
   ...
   "networkProfile": {
    "dnsServiceIp": "10.0.0.10",
    "dockerBridgeCidr": "172.17.0.1/16",
    "loadBalancerSku": "standard",
    ...
```

Doğrulama *loadBalancerSku* özellik gösterilmiştir olarak *standart*.

## <a name="use-the-load-balancer"></a>Yük Dengeleyici kullanın

Yük Dengeleyici kümenizde kullanmak için hizmet türü ile bir hizmet bildirimi oluşturma *LoadBalancer*. Çalışan yük dengeleyici göstermek için kümenizde çalıştırmak için örnek bir uygulama ile başka bir bildirim oluşturun. Bu örnek uygulama, yük dengeleyici üzerinden kullanıma sunulmuştur ve bir tarayıcı üzerinden görüntülenebilir.

Adlı bir bildirim oluşturmak `sample.yaml` aşağıdaki örnekte gösterildiği gibi:

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
```

Yukarıdaki bildirim iki dağıtım yapılandırır: *azure-vote-front* ve *azure vote arka*. Yapılandırmak için *azure-vote-front* yük dengeleyici kullanılarak açığa dağıtımı adlı bir bildirim oluşturmak `standard-lb.yaml` aşağıdaki örnekte gösterildiği gibi:

```yaml
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

Hizmet *azure-vote-front* kullanan *LoadBalancer* AKS kümenizde bağlanmak için yük dengeleyici yapılandırmak için tür *azure-vote-front* dağıtım.

Örnek uygulama dağıtırsınız ve yük dengeleyici kullanarak [kubectl uygulamak][kubectl-apply] YAML bildirimlerinizi adını belirtin:

```console
kubectl apply -f sample.yaml
kubectl apply -f standard-lb.yaml
```

*Standart* SKU yük dengeleyicide örnek uygulamayı kullanıma sunmak için artık yapılandırılır. Hizmet ayrıntılarını görüntülemek *azure-vote-front* kullanarak [kubectl alma][kubectl-get] yük dengeleyicinin genel IP görmek için. Yük dengeleyicinin genel IP adresi gösterilir *EXTERNAL-IP* sütun. Bir veya değiştirmek IP adresi için iki dakika sürebilir *\<bekleyen\>* gerçek dış IP adresine, aşağıdaki örnekte gösterildiği gibi:

```
$ kubectl get service azure-vote-front

NAME                TYPE           CLUSTER-IP     EXTERNAL-IP     PORT(S)        AGE
azure-vote-front    LoadBalancer   10.0.227.198   52.179.23.131   80:31201/TCP   16s
```

Bir tarayıcıda genel IP gidin ve örnek uygulamayı gördüğünüz doğrulayın. Yukarıdaki örnekte, genel IP olan `52.179.23.131`.

![Azure Vote’a göz atma görüntüsü](media/container-service-kubernetes-walkthrough/azure-vote.png)

> [!NOTE]
> Ayrıca dahili olarak yük dengeleyici yapılandırmanız ve genel IP açığa çıkarmamak. Yük Dengeleyici dahili olarak yapılandırmak için Ekle `service.beta.kubernetes.io/azure-load-balancer-internal: "true"` için ek açıklama olarak *LoadBalancer* hizmeti. İç yük dengeleyici hakkında da gibi daha fazla ayrıntı bildirimi bir örnek yaml gördüğünüz [burada][internal-lb-yaml].

## <a name="clean-up-the-standard-sku-load-balancer-configuration"></a>Standart SKU yük dengeleyici yapılandırmayı Temizle

Örnek uygulama ve yük dengeleyici yapılandırmasını kaldırmak için kullanmak [kubectl Sil][kubectl-delete]:

```console
kubectl delete -f sample.yaml
kubectl delete -f standard-lb.yaml
```

## <a name="next-steps"></a>Sonraki adımlar

Kubernetes hizmetleri hakkında daha fazla bilgi [Kubernetes Hizmetleri belgeleri][kubernetes-services].

<!-- LINKS - External -->
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-delete]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#delete
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubernetes-services]: https://kubernetes.io/docs/concepts/services-networking/service/
[aks-engine]: https://github.com/Azure/aks-engine

<!-- LINKS - Internal -->
[advanced-networking]: configure-azure-cni.md
[aks-support-policies]: support-policies.md
[aks-faq]: faq.md
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[aks-sp]: kubernetes-service-principal.md#delegate-access-to-other-azure-resources
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[az-aks-install-cli]: /cli/azure/aks?view=azure-cli-latest#az-aks-install-cli
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-group-create]: /cli/azure/group#az-group-create
[az-provider-register]: /cli/azure/provider#az-provider-register
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
[azure-lb]: ../load-balancer/load-balancer-overview.md
[azure-lb-comparison]: ../load-balancer/load-balancer-overview.md#skus
[install-azure-cli]: /cli/azure/install-azure-cli
[internal-lb-yaml]: internal-lb.md#create-an-internal-load-balancer
[kubernetes-concepts]: concepts-clusters-workloads.md
[use-kubenet]: configure-kubenet.md
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
