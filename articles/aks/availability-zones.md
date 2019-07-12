---
title: Azure Kubernetes Service'te (AKS) kullanılabilirlik alanlarını kullanma
description: Azure Kubernetes Service (AKS) kullanılabilirlik bölgelerindeki düğümleri dağıtan bir küme oluşturmayı öğrenin
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 06/24/2019
ms.author: iainfou
ms.openlocfilehash: 0f99386aa9eeb75a990507e383c32412fb39eceb
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67840686"
---
# <a name="preview---create-an-azure-kubernetes-service-aks-cluster-that-uses-availability-zones"></a>Önizleme - kullanılabilirlik alanı kullanan bir Azure Kubernetes Service (AKS) kümesi oluşturma

Azure Kubernetes Service (AKS) kümesini, gibi altyapı işlem düğümleri ve temel alınan Azure mantıksal bölümler arasında depolama kaynaklarını dağıtır. Bu dağıtım modeli ve düğümleri ayrı güncelleştirme ve hata etki alanlarında tek bir Azure veri merkezi arasında çalışmak emin olur. Bu varsayılan davranışı ile dağıtılan AKS kümeleri planlanan bakım olayı ya da bir yüksek düzeyde bir donanım hatasına karşı korumak için kullanılabilirlik sağlar.

Yüksek seviyede kullanılabilirlik uygulamalarınıza sağlamak için AKS kümeleri kullanılabilirlik alanları genelinde dağıtılabilir. Bu bölgeler, belirli bir bölge içinde fiziksel olarak ayrı veri merkezleri olan. Küme bileşenleri birden çok alanda dağıtıldığında, AKS kümenizin bu bölgelerinden birindeki bir hatasını tolere kuramıyor. Uygulamalarınızı ve yönetim işlemlerini bir veri merkezinin tamamı bir sorun olsa bile kullanılabilir olmaya devam.

Bu makalede bir AKS kümesi oluşturma ve kullanılabilirlik alanları genelinde düğümü bileşenleri dağıtma gösterilmektedir. Bu özellik şu anda önizleme sürümündedir.

> [!IMPORTANT]
> AKS Önizleme özellikleri, Self Servis, kabul etme. Görüş ve hata topluluğumuza toplamak için sağlanır. Önizleme'de, bu özelliklerin üretim kullanılmak üzere geliştirilmiş değildir. Genel Önizleme Özellikleri 'en yüksek çaba' destek kapsamında ayrılır. İş saatleri Pasifik Saat dilimi sırasında (Pasifik Saati) yalnızca AKS teknik destek ekipleri Yardım kullanılabilir. Ek bilgi için lütfen aşağıdaki destek makaleleri bakın:
>
> * [AKS destek ilkeleri][aks-support-policies]
> * [Azure desteği SSS][aks-faq]

## <a name="before-you-begin"></a>Başlamadan önce

Azure CLI Sürüm 2.0.66 gerekir veya daha sonra yüklü ve yapılandırılmış. Çalıştırma `az --version` sürümü bulmak için. Gerekirse yüklemek veya yükseltmek bkz [Azure CLI yükleme][install-azure-cli].

### <a name="install-aks-preview-cli-extension"></a>Aks önizlemesini CLI uzantısını yükleme

Kullanılabilirlik ana bölgelerini kullanmak AKS kümeleri oluşturmak için ihtiyacınız *aks önizlemesini* CLI 0.4.1 uzantı sürümü veya üzeri. Yükleme *aks önizlemesini* uzantısını Azure CLI kullanarak [az uzantısı ekleme][az-extension-add] command, then check for any available updates using the [az extension update][az-extension-update] komut::

```azurecli-interactive
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```

### <a name="register-feature-flags-for-your-subscription"></a>Özellik bayrakları, abonelik için kaydolun

Kullanılabilirlik alanları, bir AKS kümesi oluşturmak için önce bazı özellik bayraklarını aboneliğinizi etkinleştirin. Kümeleri, sanal makine ölçek kümesi dağıtımı ve Kubernetes düğümlerini yapılandırmasını yönetmek için kullanın. *Standart* Azure yük dengeleyicinin SKU, kümenizle ağ bileşenleri trafiği yönlendirmek için dayanıklılık sağlamak için de gereklidir. Kayıt *AvailabilityZonePreview*, *AKSAzureStandardLoadBalancer*, ve *VMSSPreview* özellik bayraklarını kullanarak [az özelliği kayıt][az-feature-register] komutu aşağıdaki örnekte gösterildiği gibi:

> [!CAUTION]
> Bir Abonelikteki bir özellik kaydettiğinizde, bu özellik şu anda kaydını yapamazsınız. Bazı Önizleme özellikleri etkinleştirdikten sonra varsayılan ardından aboneliği için oluşturulan tüm AKS kümeleri için kullanılabilir. Önizleme özellikleri üretim Aboneliklerde etkinleştirmeyin. Önizleme özellikleri test ve geri bildirim toplamak için ayrı bir abonelik kullanın.

```azurecli-interactive
az feature register --name AvailabilityZonePreview --namespace Microsoft.ContainerService
az feature register --name AKSAzureStandardLoadBalancer --namespace Microsoft.ContainerService
az feature register --name VMSSPreview --namespace Microsoft.ContainerService
```

Gösterilecek durum için birkaç dakika sürer *kayıtlı*. Kayıt kullanarak durumu denetleyebilirsiniz [az özellik listesi][az-feature-list] komutu:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/AvailabilityZonePreview')].{Name:name,State:properties.state}"
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/AKSAzureStandardLoadBalancer')].{Name:name,State:properties.state}"
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/VMSSPreview')].{Name:name,State:properties.state}"
```

Hazır olduğunuzda, kayıt yenileme *Microsoft.ContainerService* kullanarak kaynak sağlayıcısını [az provider register][az-provider-register] komutu:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

## <a name="limitations-and-region-availability"></a>Sınırlamalar ve bölge kullanılabilirliği

AKS kümeleri şu anda şu bölgelerde kullanılabilirlik alanları kullanılarak oluşturulabilir:

* Doğu ABD 2
* Kuzey Avrupa
* Güneydoğu Asya
* Batı Avrupa
* Batı ABD 2

Kullanılabilirlik alanları ile bir AKS kümesi oluşturduğunuzda, aşağıdaki sınırlamalar geçerlidir:

* Kullanılabilirlik alanları, küme oluşturulduğunda yalnızca etkinleştirebilirsiniz.
* Küme oluşturulduktan sonra kullanılabilirlik bölgesi ayarları güncelleştirilemiyor. Ayrıca, kullanılabilirlik ana bölgelerini kullanmak için var olan ve olmayan kullanılabilirlik bölgesi kümesi güncelleştirilemiyor.
* Oluşturulduktan sonra bir AKS kümesi için kullanılabilirlik alanları devre dışı bırakamazsınız.
* Seçili düğüm boyutunu (VM SKU), tüm kullanılabilirlik alanları genelinde kullanılabilir olması gerekir.
* Kullanılabilirlik bölgeleri etkin gerektiren kümeleriyle Azure standart Load Balancer'ın alanları genelinde dağıtım için kullanın.
* Kubernetes sürümü 1.13.5 kullanmanız gerekir ya da standart Load balancer'ları dağıtmak için daha büyük.

Kullanılabilirlik ana bölgelerini kullanmak AKS kümeleri, Azure load balancer kullanmalıdır *standart* SKU. Varsayılan *temel* Azure yük dengeleyicinin SKU, kullanılabilirlik alanları genelinde dağıtım desteklemez. Daha fazla bilgi ve sınırlamalar standart yük dengeleyici için bkz: [Azure load balancer standart SKU Önizleme sınırlamaları][standard-lb-limitations].

### <a name="azure-disks-limitations"></a>Azure diskleri sınırlamaları

Azure kullanan birimleri yönetilen diskleri olmayan şu an bölgesel kaynaklar. Kendi özgün bölgeden farklı bir bölgede yeniden pod'ları, önceki disklerin iliştirilemiyor. Bölgesel gelebilir arasında kalıcı depolama sorunları gerektirmeyen durum bilgisiz iş yüklerini çalıştırmak için önerilir.

Durum bilgisi olan iş yükleri çalıştırmanız gerekiyorsa, disklerle aynı bölgedeki pod'ları oluşturmak için Kubernetes Zamanlayıcı bildirmek için pod özellikleri içinde taints ve tolerations kullanın. Alternatif olarak, Azure bölgeleri arasında zamanlanmış gibi pod'ların ekleyebilirsiniz dosyaları gibi ağ tabanlı depolama kullanın.

## <a name="overview-of-availability-zones-for-aks-clusters"></a>AKS için kullanılabilirlik alanları genel bakış kümeleri

Kullanılabilirlik alanları, veri merkezi arızasına karşı uygulamalarınızı ve verilerinizi koruyan sunan bir yüksek kullanılabilirlik olur. Bir Azure bölgesi içinde benzersiz fiziksel konumlara bölgeleridir. Her bölge, soğutma ve ağ bağımsız güç ile donatılmış bir veya daha fazla veri merkezlerinden oluşur. Dayanıklılık sağlamak için üç ayrı bölge etkinleştirilmiş tüm bölgelerde en az yoktur. Bir bölge içinde kullanılabilirlik alanlarının fiziksel olarak ayrılması, uygulamaları ve verileri veri merkezi arızasına karşı korur. Bölgesel olarak yedekli Hizmetleri, uygulamaları ve verileri tek-noktaları-ın-arızasına karşı korumak için kullanılabilirlik alanları genelinde çoğaltın.

Daha fazla bilgi için [Azure kullanılabilirlik alanları nedir?][az-overview].

Kullanılabilirlik alanları kullanılarak dağıtılan AKS küme düğümlerini tek bir bölgede birden çok bölge arasında dağıtabilirsiniz. Örneğin, bir kümede *Doğu ABD 2* bölge üç kullanılabilirlik alanlarında tüm düğümleri oluşturabilir *Doğu ABD 2*. Belirli bir bölgenin başarısız olmasına karşı dayanıklı olarak bu dağıtım AKS küme kaynaklarının küme kullanılabilirliğini artırır.

![Kullanılabilirlik bölgelerindeki AKS düğümü dağıtım](media/availability-zones/aks-availability-zones.png)

Bir bölgenin kesinti düğümleri el ile yeniden Dengelenecek veya küme ölçeklendiriciyi kullanmak. Tek bir bölge kullanılamaz duruma gelirse uygulamalarınızın çalışmaya devam eder.

## <a name="create-an-aks-cluster-across-availability-zones"></a>Kullanılabilirlik alanları genelinde bir AKS kümesi oluşturma

Kullanarak bir küme oluştururken [az aks oluşturma][az-aks-create] komutu `--node-zones` parametresi tanımlar hangi bölgeleri aracı düğümleri içine dağıtılır. Belirten bir küme oluşturduğunuzda AKS denetim düzlemi bileşenleri kümeniz için en yüksek kullanılabilir bir yapılandırmaya bölgelerindeki ayrıca yayılır `--node-zones` parametresi.

Bir AKS kümesi oluşturduğunuzda, varsayılan bir aracı havuzu için herhangi bir bölgeyi tanımlamazsanız, kullanılabilirlik kümenizin AKS denetim düzlemi bileşenleri kullanmaz. (Şu anda önizlemede aks'deki) ek düğüm havuzları ekleyebilirsiniz kullanarak [az aks nodepool ekleme][az-aks-nodepool-add] belirtin ve komutu `--node-zones` yeni aracı düğümleri için ancak denetim düzlemi bileşenler kalır kullanılabilirlik alanı tanıma. Dilimini tanıma değiştiremezsiniz bunlar dağıttıktan sonra bir düğüm havuzunu veya AKS düzlemi bileşenleri denetlemek için.

Aşağıdaki örnekte adlı bir AKS kümesi oluşturur *myAKSCluster* adlı kaynak grubunda *myResourceGroup*. Toplam *3* düğümler oluşturulur - bölgedeki bir aracı *1*, diğeri de *2*ve ardından bir *3*. Kümenin oluşturma gibi işlem tanımlanan bu yana yüksek kullanılabilir bir yapılandırmaya bölgelerindeki AKS denetim düzlemi bileşenleri de dağıtılır.

```azurecli-interactive
az group create --name myResourceGroup --location eastus2

az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --kubernetes-version 1.13.5 \
    --generate-ssh-keys \
    --enable-vmss \
    --load-balancer-sku standard \
    --node-count 3 \
    --node-zones 1 2 3
```

AKS kümesi oluşturmak için birkaç dakika sürer.

## <a name="verify-node-distribution-across-zones"></a>Düğüm dağıtım dilimlerinde doğrulayın

Küme hazır olduğunda, Ölçek kümesindeki içinde dağıtıldıkları hangi kullanılabilirlik alanı görmek için aracı düğümleri listeler.

İlk olarak kullanarak AKS küme kimlik bilgilerini alma [az aks get-credentials][az-aks-get-credentials] komutu:

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

Ardından, [kubectl açıklayan][kubectl-describe] kümedeki düğümlere listelemek için komutu. Filtre *failure-domain.beta.kubernetes.io/zone* aşağıdaki örnekte gösterildiği gibi değeri:

```console
kubectl describe nodes | grep -e "Name:" -e "failure-domain.beta.kubernetes.io/zone"
```

Aşağıdaki örnek çıktı gibi belirtilen bölge ve kullanılabilirlik alanları dağıtılan üç düğüm gösterilmektedir *eastus2 1* ilk kullanılabilirlik bölgesi için ve *eastus2 2* ikinci Kullanılabilirlik alanı:

```console
Name:       aks-nodepool1-28993262-vmss000000
            failure-domain.beta.kubernetes.io/zone=eastus2-1
Name:       aks-nodepool1-28993262-vmss000001
            failure-domain.beta.kubernetes.io/zone=eastus2-2
Name:       aks-nodepool1-28993262-vmss000002
            failure-domain.beta.kubernetes.io/zone=eastus2-3
```

Ek düğümler için bir aracı havuzu eklediğinizde, Azure platformunun temel sanal makinelerin belirtilen kullanılabilirlik alanları genelinde otomatik olarak dağıtır.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, kullanılabilirlik alanları kullanan bir AKS kümesi oluşturma konusunda ayrıntılı bilgi. Yüksek oranda kullanılabilir kümeleri hakkında daha fazla konular için bkz [aks'deki iş sürekliliği ve olağanüstü durum kurtarma için en iyi yöntemler][best-practices-bc-dr].

<!-- LINKS - internal -->
[install-azure-cli]: /cli/azure/install-azure-cli
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-provider-register]: /cli/azure/provider#az-provider-register
[az-aks-create]: /cli/azure/aks#az-aks-create
[az-overview]: ../availability-zones/az-overview.md
[best-practices-bc-dr]: operator-best-practices-multi-region.md
[aks-support-policies]: support-policies.md
[aks-faq]: faq.md
[standard-lb-limitations]: load-balancer-standard.md#limitations
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
[az-aks-nodepool-add]: /cli/azure/ext/aks-preview/aks/nodepool#ext-aks-preview-az-aks-nodepool-add
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials

<!-- LINKS - external -->
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe
