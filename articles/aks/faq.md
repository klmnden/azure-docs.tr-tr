---
title: Sık sorulan sorular için Azure Kubernetes Service (AKS)
description: Azure Kubernetes Service (AKS) hakkında genel soruların yanıtlarını sağlar.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/06/2019
ms.author: iainfou
ms.openlocfilehash: f365fcd61944fbae131ab79a1c3660aaf02fa8d7
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65073942"
---
# <a name="frequently-asked-questions-about-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) hakkında sık sorulan sorular

Bu makalede adresleri sorular Azure Kubernetes Service (AKS) hakkında sık kullanılan.

## <a name="which-azure-regions-provide-the-azure-kubernetes-service-aks-today"></a>Hangi Azure bölgeleri, Azure Kubernetes Service (AKS) Bugün sağlar?

Kullanılabilir bölgelerin tam listesi için bkz. [AKS bölgeler ve kullanılabilirlik][aks-regions].

## <a name="does-aks-support-node-autoscaling"></a>AKS düğümü otomatik ölçeklendirme destekliyor mu?

Evet, otomatik ölçeklendirme aracılığıyla kullanılabilir [Kubernetes otomatik ölçeklendiricinin] [ auto-scaler] Kubernetes 1.10 itibaren. El ile yapılandırmak ve küme ölçeklendiriciyi kullanmak hakkında daha fazla bilgi için bkz. [AKS kümesi otomatik][aks-cluster-autoscale].

Ayrıca yerleşik küme ölçeklendiriciyi (şu anda önizlemede aks'deki) düğümlerini ölçeklendirme yönetmek için kullanabilirsiniz. Daha fazla bilgi için [aks'deki uygulama taleplerini karşılamak üzere küme otomatik olarak ölçeklendirme][aks-cluster-autoscaler].

## <a name="does-aks-support-kubernetes-role-based-access-control-rbac"></a>AKS, Kubernetes rol tabanlı erişim denetimi (RBAC) destekliyor mu?

Evet, Kubernetes RBAC, Azure CLI ile kümeleri oluşturduğunuzda varsayılan olarak etkindir. RBAC, şablonları ve Azure portalı kullanılarak oluşturulan kümeler için etkinleştirilebilir.

## <a name="can-i-deploy-aks-into-my-existing-virtual-network"></a>AKS my mevcut bir sanal ağa dağıtabilir miyim?

Evet, var olan bir sanal ağ kullanarak bir AKS kümesi dağıtabilirsiniz [Gelişmiş ağ özelliği][aks-advanced-networking].

## <a name="can-i-restrict-the-kubernetes-api-server-to-only-be-accessible-within-my-virtual-network"></a>Yalnızca benim sanal ağ içinde erişilebilir olmasını Kubernetes API sunucusu kısıtlarım?

Şu anda değil. Genel bir tam etki alanı adı (FQDN) olarak Kubernetes API sunucusu kullanıma sunulur. Kullanarak, kümeye erişimi denetleyebilirsiniz [Kubernetes rol tabanlı erişim denetimi (RBAC) ve Azure Active Directory (AAD)][aks-rbac-aad]

## <a name="are-security-updates-applied-to-aks-agent-nodes"></a>Güvenlik güncelleştirmeleri için AKS aracı düğümleri uygulandı?

Azure güvenlik yamaları Linux düğümleri gecelik bir zamanlamaya göre otomatik olarak uygulanır. Ancak, bu Linux düğümleri olarak yeniden gerekli sağlamak sizin sorumluluğunuzdadır. Düğümü yeniden başlatma işlemlerini gerçekleştirmek için birkaç seçeneğiniz vardır:

- El ile Azure portal veya Azure CLI ile.
- AKS kümenizi yükseltme tarafından. Yükseltme otomatik olarak küme [kordon altına alma ve düğüm boşaltma][cordon-drain], ardından her düğümü en son Ubuntu görüntüsünü ve yeni bir düzeltme eki sürümü veya ikincil bir Kubernetes sürümü ile çevrimiçine yedekleyin. Daha fazla bilgi için [AKS kümesini yükseltme][aks-upgrade].
- Kullanarak [Kured](https://github.com/weaveworks/kured), Kubernetes için bir açık kaynak önyükleme arka plan programı. Kured çalışırken bir [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) ve her düğüm için bir yeniden başlatma gerekli olduğunu belirten bir dosyanın varlığını izler. İşletim sistemi yeniden başlatma, aynı kümede yönetilir [kordon altına alma ve boşaltma işlemi] [ cordon-drain] olarak bir küme yükseltmesi.

Kured kullanma hakkında daha fazla bilgi için bkz. [AKS düğümleri için güvenlik ve çekirdek güncelleştirmeleri uygulamak][node-updates-kured].

### <a name="windows-server-nodes"></a>Windows sunucu düğümleri

(Şu anda önizlemede aks'deki) Windows Server düğümleri için Windows Update otomatik olarak çalıştırın ve en son güncelleştirmeleri uygulayın. Düzenli bir zamanlamaya göre Windows Güncelleştirme sürüm döngüsü ve kendi doğrulama işlemi, AKS kümenizi yükseltme üzerinde Windows Server düğüm havuzları gerçekleştirmeniz gerekir. Bu yükseltme işlemi, düzeltme ekleri ve en son Windows Server görüntüsü çalıştıran düğümlere oluşturur, ardından eski düğümleri kaldırır. Bu işlem hakkında daha fazla bilgi için bkz. [aks'deki bir düğüm havuzunu yükseltme][nodepool-upgrade].

## <a name="why-are-two-resource-groups-created-with-aks"></a>İki kaynak grubu, AKS ile neden oluşturulur?

Her bir AKS dağıtımı iki kaynak grubu içinde barındırıyor:

- İlk kaynak grubu tarafından oluşturulur ve yalnızca Kubernetes Hizmet kaynağı içeriyor. Gibi otomatik olarak oluşturur, dağıtım sırasında ikinci bir AKS kaynak sağlayıcısı *MC_myResourceGroup_myAKSCluster_eastus*. Bu ikinci bir kaynak grubu adını nasıl belirtebilirsiniz hakkında daha fazla bilgi için sonraki bölüme bakın.
- Bu ikinci bir kaynak grubu, gibi *MC_myResourceGroup_myAKSCluster_eastus*, kümeyle ilişkili altyapı kaynaklarını içerir. Bu kaynaklar, Kubernetes düğüm Vm'leri, sanal ağ ve depolama alanı içerir. Bu ayrı kaynak grubu, kaynak temizleme işlemi basitleştirmek için oluşturulur.

Depolama hesapları veya ayrılmış genel IP adresleri gibi bir AKS kümesi ile kullanılacak kaynaklar oluşturma, bunları otomatik olarak oluşturulan kaynak grubunda koyun.

## <a name="can-i-provide-my-own-name-for-the-aks-infrastructure-resource-group"></a>Ben AKS altyapı kaynak grubu için kendi ad verebilir misiniz?

Evet. Varsayılan olarak, AKS kaynak sağlayıcısı otomatik olarak ikincil bir kaynak grubu dağıtımı sırasında gibi oluşturur *MC_myResourceGroup_myAKSCluster_eastus*. Şirket ilkesi ile uyum sağlamak için bu yönetilen kümesi için kendi ad sağlayabilirsiniz (*MC_*) kaynak grubu.

Kendi kaynak grubu adı belirtmek için yükleme [aks önizlemesini] [ aks-preview-cli] Azure CLI uzantısı sürüm *0.3.2* veya üzeri. Kullanarak bir AKS kümesi oluştururken [az aks oluşturma] [ az-aks-create] komutu, kullanın *--düğümü-resource-group* parametre ve kaynak grubu için bir ad belirtin. Varsa, [bir Azure Resource Manager şablonu kullanma] [ aks-rm-template] bir AKS kümesi dağıtmak için kaynak grubu adını kullanarak tanımlayabilirsiniz *nodeResourceGroup* özelliği.

* Bu kaynak grubu, kendi aboneliğinizdeki Azure kaynak sağlayıcısı tarafından otomatik olarak oluşturulur.
* Küme oluşturulduğunda, yalnızca bir özel kaynak grubu adı belirtebilirsiniz.

Aşağıdaki senaryolar desteklenmez:

* Mevcut bir kaynak grubu için belirtemezsiniz *MC_* grubu.
* Farklı bir abonelik için belirtemezsiniz *MC_* kaynak grubu.
* Değiştiremezsiniz *MC_* Küme oluşturulduktan sonra kaynak grubu adı.
* Yönetilen kaynaklar için adları belirtemezsiniz *MC_* kaynak grubu.
* Değiştiremez veya içinde yönetilen kaynak etiketleri silme *MC_* kaynak grubu (sonraki bölümde ek bilgi bakın).

## <a name="can-i-modify-tags-and-other-properties-of-the-aks-resources-in-the-mc-resource-group"></a>Etiketleri ve diğer özellikleri AKS kaynakları MC_ * kaynak grubunda değişiklik yapabilirsiniz?

Değiştirme ve silme Azure tarafından oluşturulmuş etiketleri ve diğer özellikleri kaynakların *MC_** kaynak grubu ölçekleme ve yükseltme hataları gibi beklenmeyen sonuçlara yol açabilir. Oluşturma ve bir iş birimi veya maliyet merkezi atamak gibi ek özel etiketler değiştirme desteklenir. Kaynakları altındaki değiştirme *MC_** AKS kümesi hizmet düzeyi hedefi (SLO) keser. Daha fazla bilgi için [sunan bir hizmet düzeyi sözleşmesi AKS mu?](#does-aks-offer-a-service-level-agreement)

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

AKS giriş denetleyicileri listesini değiştirmek şu anda mümkün değildir.

## <a name="is-azure-key-vault-integrated-with-aks"></a>Azure Key Vault, AKS ile tümleşiktir?

AKS şu anda yerel olarak Azure Key Vault ile tümleşik değil. Ancak, [Kubernetes projesi için Azure anahtar kasası FlexVolume] [ keyvault-flexvolume] doğrudan KeyVault gizli dizileri Kubernetes pod'ların entegrasyonunu sağlar.

## <a name="can-i-run-windows-server-containers-on-aks"></a>Windows Server kapsayıcıları AKS üzerinde çalıştırabilir mi?

Evet, Windows Server kapsayıcıları Önizleme sürümünde kullanılabilir. Windows Server kapsayıcıları, AKS içinde çalıştırmak için konuk işletim sistemi Windows Server çalıştıran bir düğüm havuzu oluşturun. Windows Server kapsayıcıları, yalnızca Windows Server 2019 kullanabilirsiniz. Başlamak için [Windows Server düğüm havuzu ile bir AKS kümesi oluşturma][aks-windows-cli].

Yukarı Akış Kubernetes projesi Windows Server'da parçası olan bazı sınırlamalar penceresi sunucu düğüm havuzu desteği içerir. Bu sınırlamalar hakkında daha fazla bilgi için bkz. [AKS sınırlamaları Windows Server kapsayıcıları][aks-windows-limitations].

## <a name="does-aks-offer-a-service-level-agreement"></a>AKS bir hizmet düzeyi sözleşmesi sunar?

Bir hizmet düzeyi sözleşmesi (SLA), sağlayıcı, yayımlanan bir hizmet düzeyi karşılanmazsa hizmet maliyetini müşteri karşılamayı kabul eder. AKS kendi ücretsiz olduğundan, hiçbir ücret karşılamayı kullanılabilir ve bu nedenle biçimsel SLA yoktur. Ancak, Kubernetes API sunucusu için en az % 99,5 kullanılabilirliği sürdürmek AKS arar.

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
[aks-rm-template]: /rest/api/aks/managedclusters/createorupdate#managedcluster
[aks-cluster-autoscaler]: cluster-autoscaler.md
[nodepool-upgrade]: use-multiple-node-pools.md#upgrade-a-node-pool
[aks-windows-cli]: windows-container-cli.md
[aks-windows-limitations]: windows-node-limitations.md

<!-- LINKS - external -->

[auto-scaler]: https://github.com/kubernetes/autoscaler
[cordon-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/
[hexadite]: https://github.com/Hexadite/acs-keyvault-agent
[admission-controllers]: https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/
[keyvault-flexvolume]: https://github.com/Azure/kubernetes-keyvault-flexvol
