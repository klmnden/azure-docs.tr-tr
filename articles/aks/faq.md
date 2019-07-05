---
title: Sık sorulan sorular için Azure Kubernetes Service (AKS)
description: Azure Kubernetes Service (AKS) hakkında sık sorulan sorulara yanıtlar bulun.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 07/03/2019
ms.author: iainfou
ms.openlocfilehash: d4fa365e1ed055fa8ddeb8fd475e152af84a3b71
ms.sourcegitcommit: d3b1f89edceb9bff1870f562bc2c2fd52636fc21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/04/2019
ms.locfileid: "67560445"
---
# <a name="frequently-asked-questions-about-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) hakkında sık sorulan sorular

Bu makalede adresleri sorular Azure Kubernetes Service (AKS) hakkında sık kullanılan.

## <a name="which-azure-regions-currently-provide-aks"></a>Hangi Azure bölgeleri, şu anda AKS sağlar?

Kullanılabilir bölgelerin tam listesi için bkz. [AKS bölgeler ve kullanılabilirlik][aks-regions].

## <a name="does-aks-support-node-autoscaling"></a>AKS düğümü otomatik ölçeklendirme destekliyor mu?

Evet, aracı düğümlerini AKS yatay otomatik ölçeklendirme özelliği şu anda önizlemede kullanılabilir. Bkz: [aks'deki uygulama taleplerini karşılamak üzere küme otomatik olarak ölçeklendirme][aks-cluster-autoscaler] for instructions. AKS autoscaling is based on the [Kubernetes autoscaler][auto-scaler].

## <a name="can-i-deploy-aks-into-my-existing-virtual-network"></a>AKS my mevcut bir sanal ağa dağıtabilir miyim?

Evet, mevcut bir sanal ağına bir AKS kümesi kullanarak dağıtabileceğiniz [Gelişmiş ağ özelliği][aks-advanced-networking].

## <a name="can-i-limit-who-has-access-to-the-kubernetes-api-server"></a>Kubernetes API sunucusu için kimlerin erişebileceğini sınırlamak?

Evet, kullanarak Kubernetes API sunucusu için erişimi sınırlayabilirsiniz [API sunucusu yetkili IP aralıkları][api-server-authorized-ip-ranges], hangi şu anda Önizleme aşamasındadır.

## <a name="can-i-make-the-kubernetes-api-server-accessible-only-within-my-virtual-network"></a>Kubernetes API sunucusu erişilebilir yalnızca benim sanal ağ içinde oluşturabilir miyim?

Şu anda değil, ancak bu sunulması planlanmaktadır. İlerleme durumunu izleyebilirsiniz [AKS GitHub deposunu][private-clusters-github-issue].

## <a name="can-i-have-different-vm-sizes-in-a-single-cluster"></a>Tek bir kümede farklı VM boyutları olabilir mi?

Evet, farklı sanal makine boyutları AKS kümenizin oluşturarak kullanabileceğiniz [birden çok düğüm havuzları][multi-node-pools], hangi şu anda Önizleme aşamasındadır.

## <a name="are-security-updates-applied-to-aks-agent-nodes"></a>Güvenlik güncelleştirmeleri için AKS aracı düğümleri uygulandı?

Azure güvenlik yamaları Linux düğümleri gecelik bir zamanlamaya göre otomatik olarak uygulanır. Ancak, bu Linux düğümleri olarak yeniden gerekli sağlamak sizin sorumluluğunuzdadır. Düğümlerin yeniden başlatılması için birkaç seçeneğiniz vardır:

- El ile Azure portal veya Azure CLI ile.
- AKS kümenizi yükseltme tarafından. Küme yükseltme [kordon altına alma ve düğüm boşaltma][cordon-drain] automatically and then bring a new node online with the latest Ubuntu image and a new patch version or a minor Kubernetes version. For more information, see [Upgrade an AKS cluster][aks-upgrade].
- Kullanarak [Kured](https://github.com/weaveworks/kured), Kubernetes için bir açık kaynak önyükleme arka plan programı. Kured çalışırken bir [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) ve her düğüm için bir yeniden başlatma gerekli olduğunu belirten bir dosyanın varlığını izler. Küme genelinde tarafından aynı işletim sistemi yeniden başlatma yönetilen [kordon altına alma ve boşaltma işlemi][cordon-drain] olarak bir küme yükseltmesi.

Kured kullanma hakkında daha fazla bilgi için bkz. [AKS düğümleri için güvenlik ve çekirdek güncelleştirmeleri uygulamak][node-updates-kured].

### <a name="windows-server-nodes"></a>Windows sunucu düğümleri

(Şu anda önizlemede aks'deki) Windows Server düğümleri için Windows Update otomatik olarak çalıştırın ve en son güncelleştirmeleri uygulayın. Düzenli bir zamanlamaya göre Windows Güncelleştirme sürüm döngüsü ve kendi doğrulama işlemi, AKS kümenizi yükseltme üzerinde Windows Server düğüm havuzları gerçekleştirmeniz gerekir. Bu yükseltme işlemi, düzeltme ekleri ve en son Windows Server görüntüsü çalıştıran düğümlere oluşturur, ardından eski düğümleri kaldırır. Bu işlem hakkında daha fazla bilgi için bkz. [aks'deki bir düğüm havuzunu yükseltme][nodepool-upgrade].

## <a name="why-are-two-resource-groups-created-with-aks"></a>İki kaynak grubu, AKS ile neden oluşturulur?

Her bir AKS dağıtımı iki kaynak grubu içinde barındırıyor:

1. İlk kaynak grubu oluşturun. Bu Grup yalnızca Kubernetes Hizmet kaynağı içeriyor. AKS kaynak sağlayıcısı, dağıtım sırasında otomatik olarak ikinci bir kaynak grubu oluşturur. İkinci kaynak grubu örneğidir *MC_myResourceGroup_myAKSCluster_eastus*. Bu ikinci bir kaynak grubu adını belirtme hakkında daha fazla bilgi için sonraki bölüme bakın.
1. İkinci kaynak grubu, gibi *MC_myResourceGroup_myAKSCluster_eastus*, kümeyle ilişkili altyapı kaynaklarını içerir. Bu kaynaklar, Kubernetes düğüm Vm'leri, sanal ağ ve depolama alanı içerir. Bu kaynak grubu amacı, kaynak temizleme kolaylaştırmaktır.

Depolama hesapları veya ayrılmış genel IP adresleri gibi bir AKS kümesi ile kullanılacak kaynaklar oluşturma, bunları otomatik olarak oluşturulan kaynak grubunda koyun.

## <a name="can-i-provide-my-own-name-for-the-aks-infrastructure-resource-group"></a>Ben AKS altyapı kaynak grubu için kendi ad verebilir misiniz?

Evet. Varsayılan olarak, AKS kaynak sağlayıcısı otomatik olarak ikincil bir kaynak grubu oluşturur. (gibi *MC_myResourceGroup_myAKSCluster_eastus*) dağıtımı sırasında. Şirket ilkesi ile uyum sağlamak için bu yönetilen kümesi için kendi ad sağlayabilirsiniz (*MC_* ) kaynak grubu.

Kendi kaynak grubu adı belirtmek için yükleme [aks önizlemesini][aks-preview-cli] Azure CLI uzantısı sürüm *0.3.2* veya üzeri. Kullanarak bir AKS kümesi oluşturduğunuzda [az aks oluşturma][az-aks-create] komutu, kullanın *--düğümü-resource-group* parametre ve kaynak grubu için bir ad belirtin. Varsa, [bir Azure Resource Manager şablonu kullanma][aks-rm-template] bir AKS kümesi dağıtmak için kaynak grubu adını kullanarak tanımlayabilirsiniz *nodeResourceGroup* özelliği.

* İkincil kaynak grubu, kendi aboneliğinizdeki Azure kaynak sağlayıcısı tarafından otomatik olarak oluşturulur.
* Kümeyi oluştururken, bir özel kaynak grubu adı belirtebilirsiniz.

İle çalışırken *MC_* kaynak grubu, bunu yapamazsınız unutmayın:

* Mevcut bir kaynak grubu belirtin *MC_* grubu.
* İçin farklı bir abonelik belirtin *MC_* kaynak grubu.
* Değişiklik *MC_* Küme oluşturulduktan sonra kaynak grubu adı.
* İçinde yönetilen kaynakların adlarını belirtin *MC_* kaynak grubu.
* Değiştirme veya silme etiketleri içindeki yönetilen kaynaklar *MC_* kaynak grubu. (Sonraki bölümde ek bilgi bakın.)

## <a name="can-i-modify-tags-and-other-properties-of-the-aks-resources-in-the-mc-resource-group"></a>Etiketleri ve AKS kaynakların MC_ kaynak grubundaki diğer özelliklerini değiştirebilir?

Değiştirmek veya Azure tarafından oluşturulmuş etiketleri ve diğer kaynak özellikleri'nde Sil *MC_* kaynak grubu, ölçekleme ve yükseltme hataları gibi beklenmeyen sonuçlar alma. AKS oluşturmak ve özel etiketler değiştirmenize olanak sağlar. Oluşturma veya özel etiketler, örneğin değiştirmek için bir iş birimi veya maliyet merkezi atamak isteyebilirsiniz. Geçirilmekte olan kaynaklar değiştirerek *MC_* AKS kümesinde hizmet düzeyi hedefi (SLO) bölün. Daha fazla bilgi için [mu AKS bir hizmet düzeyi sözleşmesi sunar?](#does-aks-offer-a-service-level-agreement)

## <a name="what-kubernetes-admission-controllers-does-aks-support-can-admission-controllers-be-added-or-removed"></a>Hangi Kubernetes giriş denetleyicileri AKS destekliyor mu? Giriş denetleyicileri eklendiğinde veya kaldırıldığında?

AKS aşağıdakileri destekler [giriş denetleyicileri][admission-controllers]:

- *NamespaceLifecycle*
- *LimitRanger*
- *ServiceAccount*
- *DefaultStorageClass*
- *DefaultTolerationSeconds*
- *MutatingAdmissionWebhook*
- *ValidatingAdmissionWebhook*
- *ResourceQuota*
- *DenyEscalatingExec*
- *AlwaysPullImages*

Şu anda, AKS içinde giriş denetleyicilerinin listesini değiştiremezsiniz.

## <a name="is-azure-key-vault-integrated-with-aks"></a>Azure Key Vault, AKS ile tümleşiktir?

AKS, şu anda yerel olarak Azure Key Vault ile tümleşik değil. Ancak, [Kubernetes projesi için Azure anahtar kasası FlexVolume][keyvault-flexvolume] doğrudan Key Vault gizli dizileri Kubernetes pod'ların entegrasyonunu sağlar.

## <a name="can-i-run-windows-server-containers-on-aks"></a>Windows Server kapsayıcıları AKS üzerinde çalıştırabilir mi?

Evet, Windows Server kapsayıcıları Önizleme sürümünde kullanılabilir. Windows Server kapsayıcıları, AKS içinde çalıştırmak için konuk işletim sistemi Windows Server çalıştıran bir düğüm havuzu oluşturun. Windows Server kapsayıcıları, yalnızca Windows Server 2019 kullanabilirsiniz. Başlamak için bkz: [Windows Server düğüm havuzu ile bir AKS kümesi oluşturma][aks-windows-cli].

Pencere sunucu desteği düğüm havuzu için Yukarı Akış Kubernetes projesi Windows Server'da parçası olan bazı sınırlamaları içerir. Bu sınırlamalar hakkında daha fazla bilgi için bkz. [AKS sınırlamaları Windows Server kapsayıcıları][aks-windows-limitations].

## <a name="does-aks-offer-a-service-level-agreement"></a>AKS bir hizmet düzeyi sözleşmesi sunar?

Bir hizmet düzeyi sözleşmesi (SLA), sağlayıcı yayınlanan hizmet düzeyi karşılanmazsa hizmet maliyetini müşteri karşılamayı kabul eder. AKS ücretsiz olduğundan, AKS biçimsel SLA yoktur. Bu nedenle, hiçbir ücret karşılamayı kullanılabilir. Ancak, Kubernetes API sunucusu için en az % 99,5 kullanılabilirliğini sürdürmek AKS arar.

## <a name="why-cant-i-set-maxpods-below-30"></a>30 aşağıda maxPods neden olarak ayarlanamıyor?

AKS, ayarladığınız `maxPods` değer Azure CLI ve Azure Resource Manager şablonları kullanarak kümeyi oluşturduğunuzda. Ancak, hem Kubernetes hem de Azure CNI gerektiren bir *en düşük değer* (oluşturma zamanında doğrulanmış):

| Ağ | Minimum | Maksimum |
| -- | :--: | :--: |
| Azure CNI | 30 | 250 |
| Kubernetes | 30 | 110 |

AKS, yönetilen bir hizmet olduğundan, biz dağıtıp eklentileri ve pod'ların kümesinin bir parçası yönetebilirsiniz. Geçmişte, kullanıcıların tanımlayabilirsiniz bir `maxPods` yönetilen pod'ların (örneğin, 30) çalıştırmak için gerekli değerden daha düşük değer. AKS artık bu formülü kullanarak en az bir pod'ların sayısını hesaplar: ((maxPods veya (maxPods * vm_count)) > Yönetilen eklenti pod'ların en az.

Kullanıcılar, minimum kılamaz `maxPods` doğrulama.

## <a name="can-i-apply-azure-reservation-discounts-to-my-aks-agent-nodes"></a>Benim için AKS aracı düğümleri Azure ayırma indirimleri uygulayabilir miyim?

AKS aracı düğümleri, standart Azure sanal makineleri olarak faturalandırılır, satın aldıysanız bunu [Azure ayırmaları][reservation-discounts] AKS kullandığınız için VM boyutu, bu indirimlerin oluştuğu otomatik olarak uygulanır.

<!-- LINKS - internal -->

[aks-regions]: ./quotas-skus-regions.md#region-availability
[aks-upgrade]: ./upgrade-cluster.md
[aks-cluster-autoscale]: ./autoscaler.md
[virtual-kubelet]: virtual-kubelet.md
[aks-advanced-networking]: ./configure-azure-cni.md
[aks-rbac-aad]: ./azure-ad-integration.md
[node-updates-kured]: node-updates-kured.md
[aks-preview-cli]: /cli/azure/ext/aks-preview/aks
[az-aks-create]: /cli/azure/aks#az-aks-create
[aks-rm-template]: /azure/templates/microsoft.containerservice/2019-06-01/managedclusters
[aks-cluster-autoscaler]: cluster-autoscaler.md
[nodepool-upgrade]: use-multiple-node-pools.md#upgrade-a-node-pool
[aks-windows-cli]: windows-container-cli.md
[aks-windows-limitations]: windows-node-limitations.md
[reservation-discounts]: ../billing/billing-save-compute-costs-reservations.md
[api-server-authorized-ip-ranges]: ./api-server-authorized-ip-ranges.md
[multi-node-pools]: ./use-multiple-node-pools.md

<!-- LINKS - external -->

[auto-scaler]: https://github.com/kubernetes/autoscaler
[cordon-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/
[hexadite]: https://github.com/Hexadite/acs-keyvault-agent
[admission-controllers]: https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/
[keyvault-flexvolume]: https://github.com/Azure/kubernetes-keyvault-flexvol
[private-clusters-github-issue]: https://github.com/Azure/AKS/issues/948