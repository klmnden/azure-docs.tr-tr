---
title: Azure Kubernetes Service (AKS) kümesi ölçeklendiriciyi kullanmak
description: Kümenizi Azure Kubernetes Service (AKS) kümesi içinde uygulama taleplerini karşılamak üzere otomatik olarak ölçeklendirmek için küme ölçeklendiriciyi kullanmayı öğrenin.
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 07/08/2019
ms.author: mlearned
ms.openlocfilehash: 3ce080871ff2a38efcc75f6ff6b584af14014879
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67666014"
---
# <a name="preview---automatically-scale-a-cluster-to-meet-application-demands-on-azure-kubernetes-service-aks"></a>Önizleme - Azure Kubernetes Service'teki (AKS) uygulama taleplerini karşılamak üzere küme otomatik olarak ölçeklendirme

Azure Kubernetes Service (AKS) uygulama taleplerini tutmak için iş yüklerinizi çalıştırmak düğüm sayısını ayarlamanız gerekebilir. Küme otomatik ölçeklendiricinin bileşeni için kaynak kısıtlamaları nedeniyle zamanlanamaz kümenizi pod'ların izleyebilirsiniz. Sorunlar tespit edildiğinde, uygulama talebi karşılamak için düğüm sayısı artar. Düğümler, daha sonra gerektiğinde azalan düğüm sayısını ile pod'ları, çalışan bir olmaması için de düzenli olarak denetlenir. Bu özelliği otomatik olarak ölçeği artırın veya azaltın, AKS kümenizdeki düğüm sayısını, verimli ve ekonomik bir küme çalıştırmanıza olanak tanır.

Bu makalede etkinleştirin ve bir AKS kümesindeki Küme ölçeklendiriciyi yönetme gösterilmektedir. Küme ölçeklendiriciyi tek düğüm havuzu AKS kümeleriyle'te önizlemesi yalnızca test edilmelidir.

> [!IMPORTANT]
> AKS Önizleme özellikleri, Self Servis, kabul etme. Görüş ve hata topluluğumuza toplamak için sağlanır. Önizleme'de, bu özelliklerin üretim kullanılmak üzere geliştirilmiş değildir. Genel Önizleme Özellikleri 'en yüksek çaba' destek kapsamında ayrılır. İş saatleri Pasifik Saat dilimi sırasında (Pasifik Saati) yalnızca AKS teknik destek ekipleri Yardım kullanılabilir. Ek bilgi için lütfen aşağıdaki destek makaleleri bakın:
>
> * [AKS destek ilkeleri][aks-support-policies]
> * [Azure desteği SSS][aks-faq]

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, Azure CLI Sürüm 2.0.65 gerekir veya üzeri. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme][azure-cli-install].

### <a name="install-aks-preview-cli-extension"></a>Aks önizlemesini CLI uzantısını yükleme

Küme ölçeklendiriciyi kullanmak için gerekir *aks önizlemesini* CLI 0.4.1 uzantı sürümü veya üzeri. Yükleme *aks önizlemesini* uzantısını Azure CLI kullanarak [az uzantısı ekleme][az-extension-add] command, then check for any available updates using the [az extension update][az-extension-update] komut::

```azurecli-interactive
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```

### <a name="register-scale-set-feature-provider"></a>Ölçek kümesi özelliği sağlayıcısını Kaydet

Ölçek kullanan bir AKS oluşturmak için ayarlar, ayrıca, aboneliğinizde özellik bayrağı etkinleştirmeniz gerekir. Kaydedilecek *VMSSPreview* özellik bayrağı, kullanın [az özelliği kayıt][az-feature-register] komutu aşağıdaki örnekte gösterildiği gibi:

> [!CAUTION]
> Bir Abonelikteki bir özellik kaydettiğinizde, bu özellik şu anda kaydını yapamazsınız. Bazı Önizleme özellikleri etkinleştirdikten sonra varsayılan ardından aboneliği için oluşturulan tüm AKS kümeleri için kullanılabilir. Önizleme özellikleri üretim Aboneliklerde etkinleştirmeyin. Önizleme özellikleri test ve geri bildirim toplamak için ayrı bir abonelik kullanın.

```azurecli-interactive
az feature register --name VMSSPreview --namespace Microsoft.ContainerService
```

Gösterilecek durum için birkaç dakika sürer *kayıtlı*. Kayıt kullanarak durumu denetleyebilirsiniz [az özellik listesi][az-feature-list] komutu:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/VMSSPreview')].{Name:name,State:properties.state}"
```

Hazır olduğunuzda, kayıt yenileme *Microsoft.ContainerService* kullanarak kaynak sağlayıcısını [az provider register][az-provider-register] komutu:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

## <a name="limitations"></a>Sınırlamalar

Oluştururken ve küme ölçeklendiriciyi kullanmak AKS kümelerini yönetme aşağıdaki sınırlamalar geçerlidir:

* HTTP uygulama yönlendirme eklenti kullanılamaz.

## <a name="about-the-cluster-autoscaler"></a>Küme otomatik ölçeklendiricinin hakkında

Uygulama talepleri değişen gibi kümeleri workday arasındaki Akşam veya hafta, genellikle otomatik olarak ölçeklendirmek için bir yol gerekir. ayarlamak için. AKS kümeleri iki yoldan biriyle ölçeklendirebilirsiniz:

* **Ölçeklendiriciyi küme** gözcüler düğümlerinde kaynak kısıtlamaları nedeniyle zamanlanamaz pod'ları için. Küme otomatik olarak sonra arttıkça düğüm sayısı.
* **Yatay pod otomatik ölçeklendiricinin** ölçümleri sunucunun, kaynak talebi, pod'ları izlemek için içindeki bir Kubernetes kümesi kullanır. Bir hizmet daha fazla kaynak gerekiyorsa, talebi karşılamak için pod'ların sayısını otomatik olarak artırılır.

![Küme otomatik ölçeklendiricinin ve yatay pod otomatik ölçeklendiricinin genellikle gerekli uygulama taleplerini desteklemek için birlikte çalışır](media/autoscaler/cluster-autoscaler.png)

Yatay pod otomatik ölçeklendiricinin hem de küme ölçeklendirici, pod'ların ve gerektiğinde düğüm sayısını sonra düşürebilir. Bir süre için kullanılmayan kapasite olduğunda küme ölçeklendiriciyi düğüm sayısını azaltır. Pod'ları tarafından küme ölçeklendiriciyi kaldırılacak bir düğümde, başka bir yerde kümeye güvenli bir şekilde zamanlanır. Küme ölçeklendirici, pod'ların taşınamıyor durumunda ölçeği küçültebilirsiniz edilemiyor, aşağıdaki durumlarda olduğu gibi olabilir:

* Bir pod doğrudan oluşturulur ve denetleyici nesne, böyle bir dağıtım veya çoğaltma kümesi tarafından yedeklenmiyor.
* Pod kesintisi bütçe (PDB) fazla kısıtlayıcı ve belirli bir eşiğin altına düşen olması için pod'ların sayısını izin vermez.
* Bir pod düğüm Seçici veya başka bir düğümde zamanladıysanız gerçekleştirilemez benzeşim karşıtlığı kullanır.

Nasıl küme ölçeklendiriciyi ölçeğini olabilir hakkında daha fazla bilgi için bkz. [pod'ların can ne tür bir düğüm kaldırmasını küme ölçeklendiriciyi önlemek?][autoscaler-scaledown]

Küme otomatik ölçeklendiricinin ölçek olayları ve kaynak eşikleri arasında zaman aralıkları gibi şeyler için başlangıç parametreleri kullanır. Bu parametreler Azure platformu tarafından tanımlanır ve ayarlamak için şu anda kullanıma sunulmaz. Hangi parametreler hakkında daha fazla bilgi için küme ölçeklendiriciyi kullanır, bkz: [küme ölçeklendiriciyi parametreleri nelerdir?][autoscaler-parameters].

İki autoscalers birlikte çalışabilir ve genellikle bir kümede hem de dağıtılır. Birleştirildiğinde yatay pod otomatik ölçeklendiricinin üzerinde çalışan uygulama talebi karşılamak için gerekli pod'ların sayısını odaklanmıştır. Küme otomatik ölçeklendiricinin zamanlanmış pod'ların desteklemek için gereken düğüm üzerinde çalışan odaklanmıştır.

> [!NOTE]
> Küme otomatik ölçeklendiricinin kullandığınızda el ile ölçeklendirme devre dışı bırakılır. Gerekli düğüm sayısını belirlemek küme ölçeklendiriciyi olanak tanır. Kümenizi, el ile ölçeklendirme istiyorsanız [küme ölçeklendiriciyi devre dışı](#disable-the-cluster-autoscaler).

## <a name="create-an-aks-cluster-and-enable-the-cluster-autoscaler"></a>AKS kümesi oluşturma ve küme ölçeklendiriciyi etkinleştir

Bir AKS kümesi oluşturmak ihtiyacınız varsa [az aks oluşturma][az-aks-create] komutu. Belirtin bir *--kubernetes sürümü* , karşıladığını veya önceki özetlendiği gibi gerekli en düşük sürüm numarası [başlamadan önce](#before-you-begin) bölümü. Etkinleştirmek ve küme ölçeklendiriciyi yapılandırmak için kullanın *--enable-kümesi-otomatik ölçeklendiricinin* parametresi ve bir düğüm belirtin *--min-count* ve *--sayısı üst sınırı*.

> [!IMPORTANT]
> Küme otomatik ölçeklendiricinin Kubernetes bileşendir. AKS kümesi bir sanal makine ölçek kümesi düğümleri kullansa da, yoksa el ile etkinleştirmeniz veya Azure portalında veya Azure CLI kullanarak ölçek kümesi ölçeklendirme ayarlarını düzenleyin. Kubernetes küme ölçeklendiriciyi gerekli ölçek ayarları yönetmenize olanak tanır. Daha fazla bilgi için [AKS kaynakları düğümü kaynak grubunda değişiklik yapabilirsiniz?](faq.md#can-i-modify-tags-and-other-properties-of-the-aks-resources-in-the-node-resource-group)

Aşağıdaki örnek sanal makine ölçek kümesi ve etkin küme ölçeklendiriciyi ile bir AKS kümesi oluşturur ve en az *1* ve en fazla *3* düğümleri:

```azurecli-interactive
# First create a resource group
az group create --name myResourceGroup --location canadaeast

# Now create the AKS cluster and enable the cluster autoscaler
az aks create \
  --resource-group myResourceGroup \
  --name myAKSCluster \
  --node-count 1 \
  --enable-vmss \
  --enable-cluster-autoscaler \
  --min-count 1 \
  --max-count 3
```

Kümeyi oluşturmak ve küme ölçeklendiriciyi ayarlarını yapılandırmak için birkaç dakika sürer.

### <a name="enable-the-cluster-autoscaler-on-an-existing-aks-cluster"></a>Mevcut bir AKS kümesinde Küme ölçeklendiriciyi etkinleştir

Önceki özetlenen gereksinimleri karşılayan mevcut bir AKS kümesinde Küme ölçeklendiriciyi etkinleştirebilirsiniz [başlamadan önce](#before-you-begin) bölümü. Kullanım [az aks güncelleştirme][az-aks-update] komut ve tercih *--etkin küme ölçeklendiriciyi*, ardından bir düğüm belirtin *--min-count* ve *--sayısı üst sınırı* . Aşağıdaki örnek, en az kullanan mevcut bir kümede küme ölçeklendiriciyi etkinleştirir *1* ve en fazla *3* düğümleri:

```azurecli-interactive
az aks update \
  --resource-group myResourceGroup \
  --name myAKSCluster \
  --enable-cluster-autoscaler \
  --min-count 1 \
  --max-count 3
```

En düşük düğüm sayısı mevcut kümedeki düğüm sayısını daha büyükse ek düğümler oluşturulması birkaç dakika sürer.

## <a name="change-the-cluster-autoscaler-settings"></a>Küme otomatik ölçeklendiricinin ayarlarını değiştirme

Oluşturulacak veya güncelleştirilecek var olan bir AKS kümesi önceki adımda küme ölçeklendiriciyi en düşük düğüm sayısı ayarlandı *1*, ve en yüksek düğüm sayısı ayarlandı *3*. Uygulamanızı taleplerini değiştirin, küme ölçeklendiriciyi düğüm sayısı ayarlamanız gerekebilir.

Düğüm sayısını değiştirmek için kullanın [az aks güncelleştirme][az-aks-update] komut ve minimum ve maksimum değer belirtin. Aşağıdaki örnek kümeleri *--min-count* için *1* ve *--sayısı üst sınırı* için *5*:

```azurecli-interactive
az aks update \
  --resource-group myResourceGroup \
  --name myAKSCluster \
  --update-cluster-autoscaler \
  --min-count 1 \
  --max-count 5
```

> [!NOTE]
> Önizleme sırasında küme için ayarlanmış daha yüksek bir en düşük düğüm sayısı ayarlanamıyor. Örneğin, en küçük sayısı ayarlamak şu anda varsa *1*, en küçük sayısı güncelleştirilemiyor *3*.

Uygulama ve hizmetlerinizin performansını izlemek ve küme ölçeklendiriciyi düğüm sayıları gereken performansı eşleşecek şekilde ayarlayın.

## <a name="disable-the-cluster-autoscaler"></a>Küme otomatik ölçeklendiricinin devre dışı bırak

Artık küme ölçeklendiriciyi kullanmak istemiyorsanız, bunu kullanarak devre dışı bırakabilirsiniz [az aks güncelleştirme][az-aks-update] komutu. Küme otomatik ölçeklendiricinin devre dışı bırakıldığında düğümleri kaldırılmaz.

Küme otomatik ölçeklendiricinin kaldırmak için belirtin *--devre dışı bırakma küme ölçeklendiriciyi* parametresi, aşağıdaki örnekte gösterildiği gibi:

```azurecli-interactive
az aks update \
  --resource-group myResourceGroup \
  --name myAKSCluster \
  --disable-cluster-autoscaler
```

Küme kullanarak el ile ölçekleyebilir [az aks ölçek][az-aks-scale] komutu. Yatay pod otomatik ölçeklendiricinin kullanırsanız, bu özelliği devre dışı küme ölçeklendiriciyi ile çalışmaya devam eden, ancak pod'ların düğümünde kaynaklarını tüm kullanımda olduğunda zamanlanmış kurulamıyor bitirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede otomatik olarak AKS düğümleri sayısının ölçeğini nasıl oluşturulacağını gösterir. Yatay pod otomatik ölçeklendiricinin, uygulamanızı çalıştıran pod'ların sayısını otomatik olarak ayarlamak için de kullanabilirsiniz. Yatay pod otomatik ölçeklendiricinin kullanma adımları için bkz [ölçeklendirme uygulamaları AKS][aks-scale-apps].

<!-- LINKS - internal -->
[aks-upgrade]: upgrade-cluster.md
[azure-cli-install]: /cli/azure/install-azure-cli
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-extension-add]: /cli/azure/extension#az-extension-add
[aks-scale-apps]: tutorial-kubernetes-scale.md
[az-aks-create]: /cli/azure/aks#az-aks-create
[az-aks-scale]: /cli/azure/aks#az-aks-scale
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-provider-register]: /cli/azure/provider#az-provider-register
[aks-support-policies]: support-policies.md
[aks-faq]: faq.md
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update

<!-- LINKS - external -->
[az-aks-update]: https://github.com/Azure/azure-cli-extensions/tree/master/src/aks-preview
[autoscaler-scaledown]: https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#what-types-of-pods-can-prevent-ca-from-removing-a-node
[autoscaler-parameters]: https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#what-are-the-parameters-to-ca
