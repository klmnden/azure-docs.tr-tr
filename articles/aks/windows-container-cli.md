---
title: Önizleme - bir Azure Kubernetes Service (AKS) kümesine bir Windows Server kapsayıcı oluşturma
description: Hızla bir Kubernetes kümesi oluşturma, bir Windows Server kapsayıcıdaki Azure Kubernetes hizmeti (Azure CLI kullanılarak AKS) uygulama dağıtma hakkında bilgi edinin.
services: container-service
author: tylermsft
ms.service: container-service
ms.topic: article
ms.date: 06/17/2019
ms.author: twhitney
ms.openlocfilehash: a9887e923358b5658a365b5cfc88759eca2501e0
ms.sourcegitcommit: 82efacfaffbb051ab6dc73d9fe78c74f96f549c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67303565"
---
# <a name="preview---create-a-windows-server-container-on-an-azure-kubernetes-service-aks-cluster-using-the-azure-cli"></a>Önizleme - Azure CLI kullanarak bir Azure Kubernetes Service (AKS) kümesine bir Windows Server kapsayıcı oluşturma

Azure Kubernetes Service (AKS) hızla dağıtın ve kümelerini yönetme sağlayan yönetilen bir Kubernetes hizmetidir. Bu makalede, Azure CLI kullanarak bir AKS kümesi dağıtın. Ayrıca kümeye bir Windows Server kapsayıcı örnek ASP.NET uygulamasını dağıtın.

Bu özellik şu anda önizleme sürümündedir.

![ASP.NET örnek uygulamaya göz atma görüntüsü](media/windows-container/asp-net-sample-app.png)

Bu makalede, bir temel Kubernetes kavramlarını varsayılır. Daha fazla bilgi için [Kubernetes kavramlarını Azure Kubernetes Service (AKS) için çekirdek][kubernetes-concepts].

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale, Azure CLI Sürüm 2.0.61 çalıştırdığınız gerekir veya üzeri. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme][azure-cli-install].

## <a name="before-you-begin"></a>Başlamadan önce

Windows Server kapsayıcıları çalıştırmak üzere, küme oluşturduktan sonra bir ek düğüm havuzu eklemeniz gerekir. Bir ek düğüm havuzu ekleme, bir sonraki adımda ele alınmıştır, ancak ilk birkaç Önizleme özelliklerini etkinleştirmek gerekir.

> [!IMPORTANT]
> AKS Önizleme özellikleri, Self Servis, kabul etme. Görüş ve hata topluluğumuza toplamak için sağlanır. Önizleme'de, bu özelliklerin üretim kullanılmak üzere geliştirilmiş değildir. Genel Önizleme Özellikleri 'en yüksek çaba' destek kapsamında ayrılır. İş saatleri Pasifik Saat dilimi sırasında (Pasifik Saati) yalnızca AKS teknik destek ekipleri Yardım kullanılabilir. Ek bilgi için lütfen aşağıdaki destek makaleleri bakın:
>
> * [AKS destek ilkeleri][aks-support-policies]
> * [Azure desteği SSS][aks-faq]

### <a name="install-aks-preview-cli-extension"></a>Aks önizlemesini CLI uzantısını yükleme
    
Birden çok düğüm havuzları oluşturma ve yönetme için CLI komutları kullanılabilir *aks önizlemesini* CLI uzantısı. Yükleme *aks önizlemesini* uzantısını Azure CLI kullanarak [az uzantısı ekleme][az-extension-add] aşağıdaki örnekte gösterildiği gibi komut:

```azurecli-interactive
az extension add --name aks-preview
```

> [!NOTE]
> Daha önce yüklediyseniz *aks önizlemesini* uzantısını kullanarak güncelleştirmeleri yükle kullanılabilen `az extension update --name aks-preview` komutu.

### <a name="register-windows-preview-feature"></a>Windows önizleme özelliği Kaydet

Birden çok düğüm havuzları kullanabilir ve Windows Server kapsayıcıları çalıştırmak bir AKS kümesi oluşturmak için önce etkinleştirmeniz *WindowsPreview* özellik bayrakları aboneliğinizde. *WindowsPreview* özelliği de çok düğümlü havuzu kümeleri ve sanal makine ölçek kümesi dağıtımı ve Kubernetes düğümlerini yapılandırmasını yönetmek için kullanır. Kayıt *WindowsPreview* özellik bayrağını kullanarak [az özelliği kayıt][az-feature-register] komutu aşağıdaki örnekte gösterildiği gibi:

```azurecli-interactive
az feature register --name WindowsPreview --namespace Microsoft.ContainerService
```

> [!NOTE]
> Başarıyla kaydettikten sonra oluşturduğunuz herhangi bir AKS kümesinde *WindowsPreview* Bu önizleme küme deneyimi özellik bayrağını kullanın. Normal, tam olarak desteklenen kümeleri oluşturmak devam etmek için üretim Aboneliklerde Önizleme özelliklerini etkinleştirme. Önizleme özellikleri test için ayrı bir test veya geliştirme Azure aboneliği kullanın.

Kaydı tamamlamak birkaç dakika sürer. Kayıt kullanarak durumu denetleyin [az özellik listesi][az-feature-list] komutu:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/WindowsPreview')].{Name:name,State:properties.state}"
```

Kayıt durumu olduğunda `Registered`, durum izlemeyi durdurmak için Ctrl-C tuşlarına basın.  Ardından kayıt yenileyin *Microsoft.ContainerService* kullanarak kaynak sağlayıcısını [az provider register][az-provider-register] komutu:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

### <a name="limitations"></a>Sınırlamalar

Oluşturma ve birden çok düğüm havuzları destekleyen AKS kümeleri yönetme aşağıdaki sınırlamalar geçerlidir:

* Birden fazla düğüm havuzu başarıyla kaydettikten sonra oluşturulan küme için kullanılabilir *WindowsPreview*. Birden çok düğüm havuzları da kaydederseniz, kullanılabilir *MultiAgentpoolPreview* ve *VMSSPreview* aboneliğiniz için özellikleri. Ekleyemez veya bu özellikleri başarıyla kaydettirildi önce oluşturulmuş mevcut bir AKS kümesiyle düğümü havuzlarını yönetme.
* İlk düğüm havuzunu silemezsiniz.

Bu özellik Önizleme aşamasında olduğu sürece, aşağıdaki ek kısıtlamalar uygulanır:

* AKS kümesi en fazla sekiz düğüm havuzları sahip olabilir.
* AKS kümesi en fazla 400 düğümleri bu sekiz düğüm havuzları arasında olabilir.
* Windows Server düğüm havuzu adı, 6 karakterlik bir sınırı vardır.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği mantıksal bir gruptur. Bir kaynak grubu oluştururken konum belirtmeniz istenir. Bu kaynak grubu meta verilerini depolandığı bir konumdur başka bir bölgede kaynak oluşturma sırasında belirtmezseniz kaynaklarınızı Azure üzerinde çalıştırdığı de olabilir. Kullanarak bir kaynak grubu oluşturmanız [az grubu oluşturma][az-group-create] komutu.

Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

> [!NOTE]
> Bu makalede, bu öğreticide komutlarında Bash sözdizimini kullanır.
> Azure Cloud Shell kullanıyorsanız, açılır menüden Cloud Shell pencerenin sol üst ayarlandığından emin olun **Bash**.

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

## <a name="create-an-aks-cluster"></a>AKS kümesi oluşturma

Windows Server kapsayıcıları için düğüm havuzları destekleyen bir AKS kümesi çalıştırmak için kümenizi kullanan bir ağ ilkesi kullanması gerekir [Azure CNI][azure-cni-about] (advanced) network plugin. For more detailed information to help plan out the required subnet ranges and network considerations, see [configure Azure CNI networking][use-advanced-networking]. Kullanım [az aks oluşturma][az-aks-oluşturma] adlı bir AKS kümesi oluşturmak için komut *myAKSCluster*. Bu komut, gerekli ağ kaynaklarına yoksa oluşturur.
  * Kümenin bir düğümü ile yapılandırılmış
  * *Windows yönetici parolasını* ve *windows yönetici kullanıcı* parametrelerini, küme üzerinde oluşturulan herhangi bir Windows Server kapsayıcıları için yönetici kimlik bilgilerini ayarlayın.

Kendi güvenli sağlamak *PASSWORD_WIN* (Bu makalede komutları bir BASH kabuğundan girildiğini unutmayın):

```azurecli-interactive
PASSWORD_WIN="P@ssw0rd1234"

az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --node-count 1 \
    --enable-addons monitoring \
    --kubernetes-version 1.14.0 \
    --generate-ssh-keys \
    --windows-admin-password $PASSWORD_WIN \
    --windows-admin-username azureuser \
    --enable-vmss \
    --network-plugin azure
```

> [!Note]
> Parola doğrulama hatası alırsanız, başka bir bölgede kaynak grubunuzu oluşturmayı deneyin.
> Ardından yeni bir kaynak grubuyla kümesi oluşturmayı deneyin.

Birkaç dakika sonra komut tamamlanır ve küme hakkında JSON ile biçimlendirilmiş bilgiler döndürür.

## <a name="add-a-windows-server-node-pool"></a>Bir Windows Server düğüm havuzu ekleme

Varsayılan olarak, Linux kapsayıcıları çalıştırmak üzere bir düğüm havuzu ile bir AKS kümesi oluşturulur. Kullanım `az aks nodepool add` Windows Server kapsayıcıları çalıştırmak üzere bir ek düğüm havuzu eklemek için komutu.

```azurecli
az aks nodepool add \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --os-type Windows \
    --name npwin \
    --node-count 1 \
    --kubernetes-version 1.14.0
```

Yukarıdaki komut adlı yeni bir düğüm havuzu oluşturur *npwin* ve ekler *myAKSCluster*. Windows Server kapsayıcıları çalıştırmak için bir düğümü havuzu oluştururken, varsayılan değer *düğüm vm boyutu* olduğu *Standard_D2s_v3*. Ayarlanacak seçerseniz *düğüm vm boyutu* parametre listesini gözden geçirin [kısıtlı VM boyutları][restricted-vm-sizes]. Önerilen boyut düşük düzeyde grup üyeliğidir *Standard_D2s_v3*. Yukarıdaki komutu çalıştırırken oluşturulan varsayılan sanal ağ varsayılan alt ağ de kullanır. `az aks create`.

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
NAME                                STATUS   ROLES   AGE    VERSION
aks-nodepool1-12345678-vmssfedcba   Ready    agent   13m    v1.14.0
aksnpwin987654                      Ready    agent   108s   v1.14.0
```

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Kubernetes bildirim dosyası çalıştırmak için hangi kapsayıcı görüntüleri gibi küme için istenen durumu tanımlar. Bu makalede, bir Windows Server kapsayıcısında ASP.NET örnek uygulamayı çalıştırmak için gerekli tüm nesneleri oluşturmak için bir bildirim kullanılır. Bu bildirimi içeren bir [Kubernetes dağıtımı][kubernetes-deployment] for the ASP.NET sample application and an external [Kubernetes service][kubernetes-service] uygulamaya internet'ten erişmek için.

ASP.NET örnek uygulamanın bir parçası olarak sağlanan [.NET Framework örnekleri][dotnet-samples] ve bir Windows Server kapsayıcıda çalışır. AKS, Windows Server kapsayıcıları görüntüleri temel alan gerektirir *Windows Server 2019* veya büyük. Kubernetes bildirim dosyası da tanımlamalıdır bir [düğüm Seçicisi][node-selector] Windows Server kapsayıcıları çalıştırmak üzere bir düğümde, ASP.NET örnek uygulamanızın pod çalıştırmak için AKS kümenizi söylemek için.

Adlı bir dosya oluşturun `sample.yaml` aşağıdaki YAML tanımı kopyalayın. Azure Cloud Shell'i kullanırsanız, bu dosya kullanılarak oluşturulabilir `vi` veya `nano` bir sanal veya fiziksel sistemde olarak çalışıyorsanız:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: sample
  labels:
    app: sample
spec:
  replicas: 1
  template:
    metadata:
      name: sample
      labels:
        app: sample
    spec:
      nodeSelector:
        "beta.kubernetes.io/os": windows
      containers:
      - name: sample
        image: mcr.microsoft.com/dotnet/framework/samples:aspnetapp
        resources:
          limits:
            cpu: 1
            memory: 800m
          requests:
            cpu: .1
            memory: 300m
        ports:
          - containerPort: 80
  selector:
    matchLabels:
      app: sample
---
apiVersion: v1
kind: Service
metadata:
  name: sample
spec:
  type: LoadBalancer
  ports:
  - protocol: TCP
    port: 80
  selector:
    app: sample
```

Kullanarak uygulamayı dağıtma [kubectl uygulamak][kubectl-apply] komut ve YAML bildiriminizi adını belirtin:

```azurecli-interactive
kubectl apply -f sample.yaml
```

Hizmet başarıyla oluşturuldu ve dağıtım aşağıdaki örnek çıktı gösterilmektedir:

```
deployment.apps/sample created
service/sample created
```

## <a name="test-the-application"></a>Uygulamayı test etme

Uygulama çalıştırıldığında, uygulama ön ucu İnternet'e bir Kubernetes hizmeti sunar. Bu işlemin tamamlanması birkaç dakika sürebilir.

İlerleme durumunu izlemek için [kubectl get service][kubectl-get] komutunu `--watch` bağımsız değişkeniyle birlikte kullanın.

```azurecli-interactive
kubectl get service sample --watch
```

Başlangıçta *EXTERNAL-IP* için *örnek* hizmet olarak gösterildiği *bekleyen*.

```
NAME               TYPE           CLUSTER-IP   EXTERNAL-IP   PORT(S)        AGE
sample             LoadBalancer   10.0.37.27   <pending>     80:30572/TCP   6s
```

Zaman *EXTERNAL-IP* adresi değişirse *bekleyen* gerçek genel IP adresi için `CTRL-C` durdurmak için `kubectl` işlemi izleyin. Hizmete atanan geçerli genel IP adresi aşağıdaki örnek çıktı gösterilmektedir:

```
sample  LoadBalancer   10.0.37.27   52.179.23.131   80:30572/TCP   2m
```

Örnek uygulamayı çalışırken görmek için hizmetin dış IP adresini bir web tarayıcısı açın.

![ASP.NET örnek uygulamaya göz atma görüntüsü](media/windows-container/asp-net-sample-app.png)

## <a name="delete-cluster"></a>Kümeyi silme

Küme artık gerekli değilse, [az grubu Sil][az-group-delete] komutunu kullanarak kaynak grubunu, kapsayıcı hizmetini kaldırmak için ve tüm ilgili kaynakları.

```azurecli-interactive
az group delete --name myResourceGroup --yes --no-wait
```

> [!NOTE]
> Kümeyi sildiğinizde, AKS kümesi tarafından kullanılan Azure Active Directory hizmet sorumlusu kaldırılmaz. Hizmet sorumlusunu kaldırma adımları için bkz: [AKS hizmet sorumlusu hakkında önemli noktalar ve silme][sp-delete].

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir Kubernetes kümesi dağıtıldı ve Windows Server kapsayıcı örnek ASP.NET uygulamasını dağıtılmış. [Kubernetes web panosuna erişme][kubernetes-dashboard] oluşturduğunuz küme için.

AKS hakkında daha fazla bilgi ve dağıtım örneği için tam kod açıklaması için Kubernetes küme öğreticisine geçin.

> [!div class="nextstepaction"]
> [AKS Öğreticisi][aks-tutorial]

<!-- LINKS - external -->
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[node-selector]: https://kubernetes.io/docs/concepts/configuration/assign-pod-node/
[dotnet-samples]: https://hub.docker.com/_/microsoft-dotnet-framework-samples/
[azure-cni]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md

<!-- LINKS - internal -->
[kubernetes-concepts]: concepts-clusters-workloads.md
[aks-monitor]: https://aka.ms/coingfonboarding
[aks-tutorial]: ./tutorial-kubernetes-prepare-app.md
[az-aks-browse]: /cli/azure/aks?view=azure-cli-latest#az-aks-browse
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[az-aks-install-cli]: /cli/azure/aks?view=azure-cli-latest#az-aks-install-cli
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-group-create]: /cli/azure/group#az-group-create
[az-group-delete]: /cli/azure/group#az-group-delete
[az-provider-register]: /cli/azure/provider#az-provider-register
[azure-cli-install]: /cli/azure/install-azure-cli
[azure-cni-about]: concepts-network.md#azure-cni-advanced-networking
[sp-delete]: kubernetes-service-principal.md#additional-considerations
[azure-portal]: https://portal.azure.com
[kubernetes-deployment]: concepts-clusters-workloads.md#deployments-and-yaml-manifests
[kubernetes-service]: concepts-network.md#services
[kubernetes-dashboard]: kubernetes-dashboard.md
[restricted-vm-sizes]: quotas-skus-regions.md#restricted-vm-sizes
[use-advanced-networking]: configure-advanced-networking.md
[aks-support-policies]: support-policies.md
[aks-faq]: faq.md
