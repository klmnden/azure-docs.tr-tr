---
title: Birden çok düğüm havuzları Azure Kubernetes Service (AKS) kullanma
description: Azure Kubernetes Service (AKS) kümesi için birden çok düğüm havuzları oluşturma ve yönetme hakkında bilgi edinin
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 05/17/2019
ms.author: iainfou
ms.openlocfilehash: 4af2e97e8ace432c37a770f1930514dd19e30944
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66235767"
---
# <a name="preview---create-and-manage-multiple-node-pools-for-a-cluster-in-azure-kubernetes-service-aks"></a>Önizleme - oluşturma ve Azure Kubernetes Service (AKS) kümesi için birden çok düğüm havuzları yönetme

Azure Kubernetes Service (AKS), düğümleri aynı yapılandırmaya sahip birlikte halinde gruplandırılır *düğüm havuzları*. Bu düğüm havuzları, uygulamalarınızı çalıştırmak temel VM'ler içeriyor. Oluşturur bir AKS kümesi oluşturduğunuzda, ilk düğüm sayısını ve bunların boyutu (SKU) tanımlanan bir *varsayılan düğüm havuzu*. Farklı işlem veya depolama talepleri olan uygulamaları desteklemek için ek düğümü havuzları oluşturabilirsiniz. Örneğin, yoğun işlem gücü kullanımlı uygulamaları için GPU'ları girin veya yüksek performanslı SSD depolama alanına erişmek için bu ek düğümü havuzlarını kullanabilirsiniz.

Bu makalede, bir AKS kümesindeki birden çok düğüm havuzları oluşturma ve yönetme işlemini göstermektedir. Bu özellik şu anda önizleme sürümündedir.

> [!IMPORTANT]
> AKS Önizleme özellikleri, Self Servis, kabul etme. Görüş ve hata topluluğumuza toplamak için sağlanır. Önizleme'de, bu özelliklerin üretim kullanılmak üzere geliştirilmiş değildir. Genel Önizleme Özellikleri 'en yüksek çaba' destek kapsamında ayrılır. İş saatleri Pasifik Saat dilimi sırasında (Pasifik Saati) yalnızca AKS teknik destek ekipleri Yardım kullanılabilir. Ek bilgi için lütfen aşağıdaki destek makaleleri bakın:
>
> * [AKS destek ilkeleri][aks-support-policies]
> * [Azure desteği SSS][aks-faq]

## <a name="before-you-begin"></a>Başlamadan önce

Azure CLI Sürüm 2.0.61 gerekir veya daha sonra yüklü ve yapılandırılmış. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][install-azure-cli].

### <a name="install-aks-preview-cli-extension"></a>Aks önizlemesini CLI uzantısını yükleme

Birden çok düğüm havuzları oluşturma ve yönetme için CLI komutları kullanılabilir *aks önizlemesini* CLI uzantısı. Yükleme *aks önizlemesini* uzantısını Azure CLI kullanarak [az uzantısı ekleme] [ az-extension-add] aşağıdaki örnekte gösterildiği gibi komut:

```azurecli-interactive
az extension add --name aks-preview
```

> [!NOTE]
> Daha önce yüklediyseniz *aks önizlemesini* uzantısını kullanarak güncelleştirmeleri yükle kullanılabilen `az extension update --name aks-preview` komutu.

### <a name="register-multiple-node-pool-feature-provider"></a>Birden çok düğüm havuzu özelliği sağlayıcısını Kaydet

Birden çok düğüm havuzları kullanan bir AKS kümesi oluşturmak için ilk iki özellik bayraklarını aboneliğinizi etkinleştirin. Çok düğümlü havuzu kümeleri (VMSS) bir sanal makine ölçek kümesi dağıtımı ve Kubernetes düğümlerini yapılandırmasını yönetmek için kullanın. Kayıt *MultiAgentpoolPreview* ve *VMSSPreview* özellik bayraklarını kullanarak [az özelliği kayıt] [ az-feature-register] gösterildiği komutu Aşağıdaki örnekte:

```azurecli-interactive
az feature register --name MultiAgentpoolPreview --namespace Microsoft.ContainerService
az feature register --name VMSSPreview --namespace Microsoft.ContainerService
```

> [!NOTE]
> Başarıyla kaydettikten sonra oluşturduğunuz herhangi bir AKS kümesinde *MultiAgentpoolPreview* Bu önizleme küme deneyimi kullanın. Normal, tam olarak desteklenen kümeleri oluşturmak devam etmek için üretim Aboneliklerde Önizleme özelliklerini etkinleştirme. Önizleme özellikleri test için ayrı bir test veya geliştirme Azure aboneliği kullanın.

Gösterilecek durum için birkaç dakika sürer *kayıtlı*. Kayıt kullanarak durumu denetleyebilirsiniz [az özellik listesi] [ az-feature-list] komutu:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/MultiAgentpoolPreview')].{Name:name,State:properties.state}"
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/VMSSPreview')].{Name:name,State:properties.state}"
```

Hazır olduğunuzda, kayıt yenileme *Microsoft.ContainerService* kullanarak kaynak sağlayıcısını [az provider register] [ az-provider-register] komutu:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

## <a name="limitations"></a>Sınırlamalar

Oluşturma ve birden çok düğüm havuzları destekleyen AKS kümeleri yönetme aşağıdaki sınırlamalar geçerlidir:

* Birden çok düğüm havuzları yalnızca başarıyla kaydettikten sonra oluşturulan küme için kullanılabilir *MultiAgentpoolPreview* ve *VMSSPreview* aboneliğiniz için özellikleri. Ekleyemez veya bu özellikleri başarıyla kaydettirildi önce oluşturulmuş mevcut bir AKS kümesiyle düğümü havuzlarını yönetme.
* İlk düğüm havuzunu silemezsiniz.
* HTTP uygulama yönlendirme eklenti kullanılamaz.
* Ekleme/güncelleştirme/silme düğüm havuzları gibi çoğu işlemi var olan bir Resource Manager şablonu kullanarak yapamazsınız. Bunun yerine, [ayrı bir Resource Manager şablonu kullanma](#manage-node-pools-using-a-resource-manager-template) bir AKS kümesindeki düğüm havuzları değişiklik yapmak için.

Bu özellik Önizleme aşamasında olduğu sürece, aşağıdaki ek kısıtlamalar uygulanır:

* AKS kümesi en fazla sekiz düğüm havuzları sahip olabilir.
* AKS kümesi en fazla 400 düğümleri bu sekiz düğüm havuzları arasında olabilir.

## <a name="create-an-aks-cluster"></a>AKS kümesi oluşturma

Başlamak için tek bir düğüm havuzu ile bir AKS kümesi oluşturun. Aşağıdaki örnekte [az grubu oluşturma] [ az-group-create] adlı bir kaynak grubu oluşturmak için komut *myResourceGroup* içinde *eastus* bölge. Adlı bir AKS kümesi *myAKSCluster* ardından kullanılarak oluşturulan [az aks oluşturma] [ az-aks-create] komutu. A *--kubernetes sürümü* , *1.12.6* aşağıdaki bir adımda bir düğüm havuzunu güncelleştirme işlemini göstermek için kullanılır. Tüm belirtebilirsiniz [Kubernetes sürümü desteklenen][supported-versions].

```azurecli-interactive
# Create a resource group in East US
az group create --name myResourceGroup --location eastus

# Create a basic single-node AKS cluster
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --enable-vmss \
    --node-count 1 \
    --generate-ssh-keys \
    --kubernetes-version 1.12.6
```

Kümenin oluşturulması birkaç dakika sürer.

Küme hazır olduğunda, kullanmak [az aks get-credentials] [ az-aks-get-credentials] ile kullanmak için küme kimlik bilgilerini almak için komut `kubectl`:

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

## <a name="add-a-node-pool"></a>Bir düğüm havuzu ekleme

Önceki adımda oluşturulan küme, tek düğüm havuzu vardır. İkinci bir düğüm havuzunu kullanan ekleyelim [az aks düğümü havuzu ekleme] [ az-aks-nodepool-add] komutu. Aşağıdaki örnekte adlı bir düğüm havuzu oluşturur *mynodepool* çalıştırılan *3* düğümleri:

```azurecli-interactive
az aks nodepool add \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name mynodepool \
    --node-count 3
```

Düğüm havuzlarınızı durumunu görmek için [az aks düğümü havuzu listesi] [ az-aks-nodepool-list] komut ve kaynak grubu ve küme adını belirtin:

```azurecli-interactive
az aks nodepool list --resource-group myResourceGroup --cluster-name myAKSCluster -o table
```

Aşağıdaki örnek çıktı gösterilmektedir *mynodepool* düğüm havuzdaki üç düğüm ile başarıyla oluşturuldu. AKS kümesi önceki adımda, bir varsayılan oluşturuldu zaman *nodepool1* bir düğüm sayısı ile oluşturulmuş *1*.

```console
$ az aks nodepool list -g myResourceGroup --cluster-name myAKSCluster -o table

AgentPoolType            Count    MaxPods    Name        OrchestratorVersion    OsDiskSizeGb    OsType    ProvisioningState    ResourceGroup         VmSize
-----------------------  -------  ---------  ----------  ---------------------  --------------  --------  -------------------  --------------------  ---------------
VirtualMachineScaleSets  3        110        mynodepool  1.12.6                 100             Linux     Succeeded            myResourceGroupPools  Standard_DS2_v2
VirtualMachineScaleSets  1        110        nodepool1   1.12.6                 100             Linux     Succeeded            myResourceGroupPools  Standard_DS2_v2
```

> [!TIP]
> Hayır ise *OrchestratorVersion* veya *VmSize* bir düğüm havuzu eklediğinizde, belirtilen düğümlerin AKS kümesi için varsayılan değerleri temel alınarak oluşturulur. Bu örnekte, Kubernetes sürümü olan *1.12.6* ve düğüm boyutunu *Standard_DS2_v2*.

## <a name="upgrade-a-node-pool"></a>Bir düğüm havuzunu yükseltme

AKS kümenizi ilk adımda oluşturulduğunda bir `--kubernetes-version` , *1.12.6* belirtildi. Şimdi yükseltin *mynodepool* kubernetes'e *1.12.7*. Kullanım [az aks düğümü havuzu yükseltme] [ az-aks-nodepool-upgrade] komutu aşağıdaki örnekte gösterildiği gibi düğüm havuzu yükseltmek için:

```azurecli-interactive
az aks nodepool upgrade \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name mynodepool \
    --kubernetes-version 1.12.7 \
    --no-wait
```

Düğüm havuzlarınızı kullanarak yeniden durumunu listeleyin [az aks düğümü havuzu listesi] [ az-aks-nodepool-list] komutu. Aşağıdaki örnek, gösterir *mynodepool* bulunduğu *yükseltme* durumunu *1.12.7*:

```console
$ az aks nodepool list -g myResourceGroup --cluster-name myAKSCluster -o table

AgentPoolType            Count    MaxPods    Name        OrchestratorVersion    OsDiskSizeGb    OsType    ProvisioningState    ResourceGroup    VmSize
-----------------------  -------  ---------  ----------  ---------------------  --------------  --------  -------------------  ---------------  ---------------
VirtualMachineScaleSets  3        110        mynodepool  1.12.7                 100             Linux     Upgrading            myResourceGroup  Standard_DS2_v2
VirtualMachineScaleSets  1        110        nodepool1   1.12.6                 100             Linux     Succeeded            myResourceGroup  Standard_DS2_v2
```

Düğümleri belirtilen sürüme yükseltmek için birkaç dakika sürer.

En iyi uygulama, bir AKS kümesindeki tüm düğüm havuzları aynı Kubernetes sürümüne yükseltmeniz gerekir. Tek tek düğüm havuzları yükseltme olanağı, sıralı yükseltme gerçekleştirme ve uygulama çalışma süresini sağlamak için düğüm havuzları arasında pod'ları zamanlama sağlar.

## <a name="scale-a-node-pool"></a>Bir düğüm havuzunu ölçeklendirmek

Uygulamanızla iş yükü, değişiklik talep, bir düğüm havuzdaki düğüm sayısını ölçeklendirmeniz gerekebilir. Düğüm sayısı yukarı veya aşağı ölçeklendirilebilir.

<!--If you scale down, nodes are carefully [cordoned and drained][kubernetes-drain] to minimize disruption to running applications.-->

Bir düğüm havuzdaki düğüm sayısını ölçeklendirmek için kullanmak [az aks düğümü havuzu ölçek] [ az-aks-nodepool-scale] komutu. Aşağıdaki örnek, düğüm sayısını ölçeklendirir *mynodepool* için *5*:

```azurecli-interactive
az aks nodepool scale \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name mynodepool \
    --node-count 5 \
    --no-wait
```

Düğüm havuzlarınızı kullanarak yeniden durumunu listeleyin [az aks düğümü havuzu listesi] [ az-aks-nodepool-list] komutu. Aşağıdaki örnek, gösterir *mynodepool* bulunduğu *ölçeklendirme* yeni sayısı ile durum *5* düğümleri:

```console
$ az aks nodepool list -g myResourceGroupPools --cluster-name myAKSCluster -o table

AgentPoolType            Count    MaxPods    Name        OrchestratorVersion    OsDiskSizeGb    OsType    ProvisioningState    ResourceGroup         VmSize
-----------------------  -------  ---------  ----------  ---------------------  --------------  --------  -------------------  --------------------  ---------------
VirtualMachineScaleSets  5        110        mynodepool  1.12.7                 100             Linux     Scaling              myResourceGroupPools  Standard_DS2_v2
VirtualMachineScaleSets  1        110        nodepool1   1.12.6                 100             Linux     Succeeded            myResourceGroupPools  Standard_DS2_v2
```

Ölçeklendirme işleminin tamamlanması birkaç dakika sürer.

## <a name="delete-a-node-pool"></a>Bir düğüm havuzunu Sil

Bir havuz artık ihtiyacınız kalmadığında, silin ve temel alınan VM düğümleri kaldırma. Bir düğüm havuzunu silmek için kullanın [az aks düğümü havuzunu silme] [ az-aks-nodepool-delete] komut ve düğüm havuzu adı belirtin. Aşağıdaki örnek siler *mynoodepool* önceki adımlarda oluşturulan:

> [!CAUTION]
> Bir düğüm havuzu sildiğinizde oluşabilecek veri kaybı için hiçbir kurtarma seçeneği vardır. Pod'ların düğümü havuzlarını zamanlanamaz, söz konusu uygulamaların kullanılamaz. Kullanımdaki uygulamaları veri yedekleri veya küme düğümü havuzlarını üzerinde çalıştırma olanağı olmadığında bir düğüm havuzunu silme emin olun.

```azurecli-interactive
az aks nodepool delete -g myResourceGroup --cluster-name myAKSCluster --name mynodepool --no-wait
```

Aşağıdaki örnek çıktısı [az aks düğümü havuzu listesi] [ az-aks-nodepool-list] komut gösterir *mynodepool* bulunduğu *silme* durumu:

```console
$ az aks nodepool list -g myResourceGroup --cluster-name myAKSCluster -o table

AgentPoolType            Count    MaxPods    Name        OrchestratorVersion    OsDiskSizeGb    OsType    ProvisioningState    ResourceGroup         VmSize
-----------------------  -------  ---------  ----------  ---------------------  --------------  --------  -------------------  --------------------  ---------------
VirtualMachineScaleSets  5        110        mynodepool  1.12.7                 100             Linux     Deleting             myResourceGroupPools  Standard_DS2_v2
VirtualMachineScaleSets  1        110        nodepool1   1.12.6                 100             Linux     Succeeded            myResourceGroupPools  Standard_DS2_v2
```

Düğüm ve düğüm havuzu silmek için birkaç dakika sürer.

## <a name="specify-a-vm-size-for-a-node-pool"></a>Bir düğüm havuzu için bir VM boyutu belirtin

Bir düğüm havuzu oluşturmak için önceki örneklerde oluşturulan kümedeki düğümler için bir varsayılan sanal makine boyutu kullanıldı. Daha yaygın bir senaryoda, farklı VM boyutları ve özellikleri sayesinde düğüm havuzları oluşturmanıza olanak içindir. Örneğin, büyük miktarda CPU veya bellek düğümleri içeren bir düğüm havuzunu veya GPU desteği sağlayan bir düğüm havuzunu oluşturabilirsiniz. Sonraki adımda, [kullanım taints ve tolerations][#schedule-pods-using-taints-and-tolerations] Kubernetes Zamanlayıcı pod'ların erişimi sınırlamak nasıl bildirmek için bu düğümler üzerinde çalıştırabilirsiniz.

Aşağıdaki örnekte, kullanan bir GPU tabanlı düğüm havuzu oluşturmak *işler için standart_nc6* VM boyutu. Bu sanal makineler, NVIDIA Tesla K80 kartını desteklenir. Kullanılabilir VM boyutları hakkında daha fazla bilgi için bkz: [azure'da Linux sanal makine boyutları][vm-sizes].

Kullanarak bir düğüm havuzu oluşturma [az aks düğümü havuzu ekleme] [ az-aks-nodepool-add] yeniden komutu. Bu kez, adı belirtin *gpunodepool*ve `--node-vm-size` belirtmek için parametre *işler için standart_nc6* boyutu:

```azurecli-interactive
az aks nodepool add \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name gpunodepool \
    --node-count 1 \
    --node-vm-size Standard_NC6 \
    --no-wait
```

Aşağıdaki örnek çıktısı [az aks düğümü havuzu listesi] [ az-aks-nodepool-list] komut gösterir *gpunodepool* olduğu *oluşturma* düğümleri Belirtilen *VmSize*:

```console
$ az aks nodepool list -g myResourceGroup --cluster-name myAKSCluster -o table

AgentPoolType            Count    MaxPods    Name         OrchestratorVersion    OsDiskSizeGb    OsType    ProvisioningState    ResourceGroup         VmSize
-----------------------  -------  ---------  -----------  ---------------------  --------------  --------  -------------------  --------------------  ---------------
VirtualMachineScaleSets  1        110        gpunodepool  1.12.6                 100             Linux     Creating             myResourceGroupPools  Standard_NC6
VirtualMachineScaleSets  1        110        nodepool1    1.12.6                 100             Linux     Succeeded            myResourceGroupPools  Standard_DS2_v2
```

Birkaç dakika sürer *gpunodepool* başarıyla oluşturulacak.

## <a name="schedule-pods-using-taints-and-tolerations"></a>Pod'ların taints ve tolerations kullanarak zamanlama

Artık iki düğüm havuzları kümenizde - başlangıçta oluşturulan varsayılan düğüm havuzu ve GPU tabanlı düğüm havuzu sahipsiniz. Kullanım [kubectl alma düğümleri] [ kubectl-get] kümenizdeki düğümleri görmek için komutu. Aşağıdaki örnek çıktıda her düğüm havuzunda bir düğüm gösterir:

```console
$ kubectl get nodes

NAME                                 STATUS   ROLES   AGE     VERSION
aks-gpunodepool-28993262-vmss000000  Ready    agent   4m22s   v1.12.6
aks-nodepool1-28993262-vmss000000    Ready    agent   115m    v1.12.6
```

Kubernetes Zamanlayıcı taints ve tolerations düğümler üzerinde hangi iş yüklerini çalıştırabilirsiniz kısıtlamak için kullanabilirsiniz.

* A **taint** belirli pod'ların yalnızca bunlara zamanlanabilir belirten bir düğüm uygulanır.
* A **toleration** izin veren bir pod uygulanır *tolere* düğümün taint.

Gelişmiş zamanlanmış Kubernetes özellikleri nasıl kullanılacağı hakkında daha fazla bilgi için bkz: [aks'deki Gelişmiş Zamanlayıcı özellikleri için en iyi yöntemler][taints-tolerations]

Bu örnekte, bir taint kullanarak düğüm GPU tabanlı uygulama [kubectl taint düğüm] [ kubectl-taint] komutu. GPU tabanlı düğümünüzü önceki çıktısından adını `kubectl get nodes` komutu. Taint olarak uygulanan bir *anahtar: değer* ve ardından bir zamanlama seçeneği. Aşağıdaki örnekte *sku gpu =* eşleştirebilir ve pod'ların tanımlar Aksi takdirde *NoSchedule* özelliği:

```console
kubectl taint node aks-gpunodepool-28993262-vmss000000 sku=gpu:NoSchedule
```

Aşağıdaki temel örnek YAML bildirimi bir toleration NGINX pod GPU tabanlı düğümde çalıştırılacak Kubernetes Zamanlayıcı izin vermek için kullanır. MNIST veri kümesinde bir Tensorflow işini çalıştırmak bir daha uygun ancak zaman yoğun örneği için bkz [AKS üzerinde yoğun işlem gücü kullanımlı iş yükleri için kullanım Gpu'lar][gpu-cluster].

Adlı bir dosya oluşturun `gpu-toleration.yaml` ve aşağıdaki örnekte YAML kopyalayın:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
  - image: nginx:1.15.9
    name: mypod
    resources:
      requests:
        cpu: 100m
        memory: 128Mi
      limits:
        cpu: 1
        memory: 2G
  tolerations:
  - key: "sku"
    operator: "Equal"
    value: "gpu"
    effect: "NoSchedule"
```

Pod kullanarak zamanlamayı `kubectl apply -f gpu-toleration.yaml` komutu:

```console
kubectl apply -f gpu-toleration.yaml
```

Pod zamanlamak ve NGINX görüntü çekmek için birkaç saniye sürer. Kullanım [kubectl açıklayan pod] [ kubectl-describe] pod durumunu görüntülemek için komutu. Aşağıdaki sıkıştırılmış örneğe çıktısı bunu gösterir *sku gpu:NoSchedule =* toleration uygulanır. Olayları bölümünde Zamanlayıcı pod'u atadığı *aks gpunodepool 28993262 vmss000000* GPU tabanlı düğüm:

```console
$ kubectl describe pod mypod

[...]
Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s
                 node.kubernetes.io/unreachable:NoExecute for 300s
                 sku=gpu:NoSchedule
Events:
  Type    Reason     Age    From                                          Message
  ----    ------     ----   ----                                          -------
  Normal  Scheduled  4m48s  default-scheduler                             Successfully assigned default/mypod to aks-gpunodepool-28993262-vmss000000
  Normal  Pulling    4m47s  kubelet, aks-gpunodepool-28993262-vmss000000  pulling image "nginx:1.15.9"
  Normal  Pulled     4m43s  kubelet, aks-gpunodepool-28993262-vmss000000  Successfully pulled image "nginx:1.15.9"
  Normal  Created    4m40s  kubelet, aks-gpunodepool-28993262-vmss000000  Created container
  Normal  Started    4m40s  kubelet, aks-gpunodepool-28993262-vmss000000  Started container
```

Uygulanan bu taint sahip pod düğümlerinde zamanlanabilir *gpunodepool*. Diğer bir pod içinde zamanlanacak *nodepool1* düğüm havuzu. Ek düğüm havuzları oluşturursanız, ek taints kullanabilirsiniz ve bu düğüm kaynaklar üzerinde hangi pod'ların sınırlamak için tolerations zamanlanabilir.

## <a name="manage-node-pools-using-a-resource-manager-template"></a>Resource Manager şablonu kullanarak düğüm havuzları yönetme

Yönetilen kaynaklar oluşturmak için bir Azure Resource Manager şablonu kullandığınızda ve kaynak güncelleştirmek için şablon ve yeniden dağıtma ayarları genellikle güncelleştirebilirsiniz. AKS kümesi oluşturulduktan sonra aks'deki nodepools ile ilk nodepool profili güncelleştirilemiyor. Bu davranış, olamaz var olan bir Resource Manager şablonu güncelleştirme, değişiklik yapmak için düğüm havuzları, yeniden anlamına gelir. Bunun yerine, aracı havuzları yalnızca var olan bir AKS kümesi için güncelleştirmeleri ayrı bir Resource Manager şablonu oluşturmanız gerekir.

Gibi bir şablon oluşturma `aks-agentpools.json` ve aşağıdaki örnek bildirimde yapıştırın. Bu örnek şablon, aşağıdaki ayarları yapılandırır:

* Güncelleştirmeleri *Linux* adlı aracı havuzu *myagentpool* üç düğüm çalıştırılacak.
* Kubernetes sürümü çalıştırmak için düğüm havuzdaki düğümler ayarlar *1.12.8*.
* Düğüm boyutu olarak tanımlayan *Standard_DS2_v2*.

Bu değerler, güncelleştirmek için ekleme veya gerektiğinde düğüm havuzları silme gibi düzenleyin:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "clusterName": {
      "type": "string",
      "metadata": {
        "description": "The name of your existing AKS cluster."
      }
    },
    "location": {
      "type": "string",
      "metadata": {
        "description": "The location of your existing AKS cluster."
      }
    },
    "agentPoolName": {
      "type": "string",
      "defaultValue": "myagentpool",
      "metadata": {
        "description": "The name of the agent pool to create or update."
      }
    },
    "vnetSubnetId": {
      "type": "string",
      "defaultValue": "",
      "metadata": {
        "description": "The Vnet subnet resource ID for your existing AKS cluster."
      }
    }
  },
  "variables": {
    "apiVersion": {
      "aks": "2019-04-01"
    },
    "agentPoolProfiles": {
      "maxPods": 30,
      "osDiskSizeGB": 0,
      "agentCount": 3,
      "agentVmSize": "Standard_DS2_v2",
      "osType": "Linux",
      "vnetSubnetId": "[parameters('vnetSubnetId')]"
    }
  },
  "resources": [
    {
      "apiVersion": "2019-04-01",
      "type": "Microsoft.ContainerService/managedClusters/agentPools",
      "name": "[concat(parameters('clusterName'),'/', parameters('agentPoolName'))]",
      "location": "[parameters('location')]",
      "properties": {
            "maxPods": "[variables('agentPoolProfiles').maxPods]",
            "osDiskSizeGB": "[variables('agentPoolProfiles').osDiskSizeGB]",
            "count": "[variables('agentPoolProfiles').agentCount]",
            "vmSize": "[variables('agentPoolProfiles').agentVmSize]",
            "osType": "[variables('agentPoolProfiles').osType]",
            "storageProfile": "ManagedDisks",
      "type": "VirtualMachineScaleSets",
            "vnetSubnetID": "[variables('agentPoolProfiles').vnetSubnetId]",
            "orchestratorVersion": "1.12.8"
      }
    }
  ]
}
```

Bu şablonu kullanarak dağıtma [az grubu dağıtımı oluşturmak] [ az-group-deployment-create] , aşağıdaki örnekte gösterildiği gibi komutu. Mevcut AKS küme adı ve konumu istenir:

```azurecli-interactive
az group deployment create \
    --resource-group myResourceGroup \
    --template-file aks-agentpools.json
```

Bu düğüm havuzu ayarları ve Resource Manager şablonunuzda tanımladığınız operations bağlı olarak, AKS kümesi güncelleştirilmesi birkaç dakika sürebilir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu makalede, GPU tabanlı düğümleri içeren bir AKS kümesi oluşturuldu. Gereksiz maliyetini azaltmak için silmek isteyebilirsiniz *gpunodepool*, ya da tüm AKS kümesi.

GPU tabanlı düğüm havuzu silmek için kullanın [az aks nodepool Sil] [ az-aks-nodepool-delete] komutu aşağıdaki örnekte gösterildiği gibi:

```azurecli-interactive
az aks nodepool delete -g myResourceGroup --cluster-name myAKSCluster --name gpunodepool
```

Kümeyi silmek için kullanın [az grubu Sil] [ az-group-delete] AKS kaynak grubunu silmek için:

```azurecli-interactive
az group delete --name myResourceGroup --yes --no-wait
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede bir AKS kümesindeki birden çok düğüm havuzları oluşturma ve yönetme öğrendiniz. Pod'ları arasında düğüm havuzları denetleme hakkında daha fazla bilgi için bkz. [aks'deki Gelişmiş Zamanlayıcı özellikleri için en iyi yöntemler][operator-best-practices-advanced-scheduler].

Oluşturma ve Windows Server kapsayıcı düğüm havuzları kullanma hakkında bilgi için bkz: [AKS içinde bir Windows Server kapsayıcı oluşturma][aks-windows].

<!-- EXTERNAL LINKS -->
[kubernetes-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-taint]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#taint
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe

<!-- INTERNAL LINKS -->
[azure-cli-install]: /cli/azure/install-azure-cli
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-provider-register]: /cli/azure/provider#az-provider-register
[az-aks-get-credentials]: /cli/azure/aks#az-aks-get-credentials
[az-group-create]: /cli/azure/group#az-group-create
[az-aks-create]: /cli/azure/aks#az-aks-create
[az-aks-nodepool-add]: /cli/azure/ext/aks-preview/aks#ext-aks-preview-az-aks-nodepool-add
[az-aks-nodepool-list]: /cli/azure/ext/aks-preview/aks#ext-aks-preview-az-aks-nodepool-list
[az-aks-nodepool-upgrade]: /cli/azure/ext/aks-preview/aks#ext-aks-preview-az-aks-nodepool-upgrade
[az-aks-nodepool-scale]: /cli/azure/ext/aks-preview/aks#ext-aks-preview-az-aks-nodepool-scale
[az-aks-nodepool-delete]: /cli/azure/ext/aks-preview/aks#ext-aks-preview-az-aks-nodepool-delete
[vm-sizes]: ../virtual-machines/linux/sizes.md
[taints-tolerations]: operator-best-practices-advanced-scheduler.md#provide-dedicated-nodes-using-taints-and-tolerations
[gpu-cluster]: gpu-cluster.md
[az-group-delete]: /cli/azure/group#az-group-delete
[install-azure-cli]: /cli/azure/install-azure-cli
[supported-versions]: supported-kubernetes-versions.md
[operator-best-practices-advanced-scheduler]: operator-best-practices-advanced-scheduler.md
[aks-windows]: windows-container-cli.md
[az-group-deployment-create]: /cli/azure/group/deployment#az-group-deployment-create
[aks-support-policies]: support-policies.md
[aks-faq]: faq.md
