---
title: Sık sorulan sorular için Azure Kubernetes Service (AKS)
description: Azure Kubernetes Service (AKS) hakkında genel soruların yanıtlarını sağlar.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 04/25/2019
ms.author: iainfou
ms.openlocfilehash: 17bc1d2b7a08314f19f1bf8f87d0c774afc37500
ms.sourcegitcommit: 8fc5f676285020379304e3869f01de0653e39466
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65508171"
---
# <a name="frequently-asked-questions-about-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) hakkında sık sorulan sorular

Bu makalede adresleri sorular Azure Kubernetes Service (AKS) hakkında sık kullanılan.

## <a name="which-azure-regions-provide-the-azure-kubernetes-service-aks-today"></a>Hangi Azure bölgeleri, Azure Kubernetes Service (AKS) Bugün sağlar?

Kullanılabilir bölgelerin tam listesi için bkz. [AKS bölgeler ve kullanılabilirlik][aks-regions].

## <a name="does-aks-support-node-autoscaling"></a>AKS düğümü otomatik ölçeklendirme destekliyor mu?

Evet, otomatik ölçeklendirme aracılığıyla kullanılabilir [Kubernetes otomatik ölçeklendiricinin] [ auto-scaler] Kubernetes 1.10 itibaren. Yapılandırma ve küme ölçeklendiriciyi kullanmak hakkında daha fazla bilgi için bkz. [AKS kümesi otomatik][aks-cluster-autoscale].

## <a name="does-aks-support-kubernetes-role-based-access-control-rbac"></a>AKS, Kubernetes rol tabanlı erişim denetimi (RBAC) destekliyor mu?

Evet, Kubernetes RBAC, Azure CLI ile kümeleri oluşturduğunuzda varsayılan olarak etkindir. RBAC, şablonları ve Azure portalı kullanılarak oluşturulan kümeler için etkinleştirilebilir.

## <a name="can-i-deploy-aks-into-my-existing-virtual-network"></a>AKS my mevcut bir sanal ağa dağıtabilir miyim?

Evet, var olan bir sanal ağ kullanarak bir AKS kümesi dağıtabilirsiniz [Gelişmiş ağ özelliği][aks-advanced-networking].

## <a name="can-i-restrict-the-kubernetes-api-server-to-only-be-accessible-within-my-virtual-network"></a>Yalnızca benim sanal ağ içinde erişilebilir olmasını Kubernetes API sunucusu kısıtlarım?

Şu anda değil. Genel bir tam etki alanı adı (FQDN) olarak Kubernetes API sunucusu kullanıma sunulur. Kullanarak, kümeye erişimi denetleyebilirsiniz [Kubernetes rol tabanlı erişim denetimi (RBAC) ve Azure Active Directory (AAD)][aks-rbac-aad]

## <a name="are-security-updates-applied-to-aks-agent-nodes"></a>Güvenlik güncelleştirmeleri için AKS aracı düğümleri uygulandı?

Evet, Azure otomatik olarak güvenlik yamaları düğümleri gecelik bir zamanlamaya göre uygulanır. Ancak, düğümleri yeniden başlatılır sağlamaktan sorumlu olduğunuz gerektiğinde. Düğümü yeniden başlatma işlemlerini gerçekleştirmek için birkaç seçeneğiniz vardır:

- El ile Azure portal veya Azure CLI ile.
- AKS kümenizi yükseltme tarafından. Yükseltme otomatik olarak küme [kordon altına alma ve düğüm boşaltma][cordon-drain], ardından her düğümü en son Ubuntu görüntüsünü ve yeni bir düzeltme eki sürümü veya ikincil bir Kubernetes sürümü ile çevrimiçine yedekleyin. Daha fazla bilgi için [AKS kümesini yükseltme][aks-upgrade].
- Kullanarak [Kured](https://github.com/weaveworks/kured), Kubernetes için bir açık kaynak önyükleme arka plan programı. Kured çalışırken bir [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) ve her düğüm için bir yeniden başlatma gerekli olduğunu belirten bir dosyanın varlığını izler. İşletim sistemi yeniden başlatma, aynı kümede yönetilir [kordon altına alma ve boşaltma işlemi] [ cordon-drain] olarak bir küme yükseltmesi.

Kured kullanma hakkında daha fazla bilgi için bkz. [AKS düğümleri için güvenlik ve çekirdek güncelleştirmeleri uygulamak][node-updates-kured].

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

Windows Server kapsayıcıları çalıştırmak için Windows Server tabanlı düğümleri çalıştırmanız gerekir. Windows Server tabanlı düğümler şu anda AKS içinde kullanılamaz. Ancak, Azure Container Instances Windows kapsayıcıları zamanlayabilir ve AKS kümenizin bir parçası yönetmek için Virtual Kubelet kullanabilirsiniz. Daha fazla bilgi için [kullanım Virtual Kubelet ile AKS][virtual-kubelet].

## <a name="does-aks-offer-a-service-level-agreement"></a>AKS bir hizmet düzeyi sözleşmesi sunar?

Bir hizmet düzeyi sözleşmesi (SLA), sağlayıcı, yayımlanan bir hizmet düzeyi karşılanmazsa hizmet maliyetini müşteri karşılamayı kabul eder. AKS kendi ücretsiz olduğundan, hiçbir ücret karşılamayı kullanılabilir ve bu nedenle biçimsel SLA yoktur. Ancak, Kubernetes API sunucusu için en az % 99,5 kullanılabilirliği sürdürmek AKS arar.

## <a name="why-can-i-not-set-maxpods-below-30"></a>Neden değil ayarlarım `maxPods` 30 aşağıda?

AKS ayarı destekler `maxPods` Azure CLI ve Azure Resource Manager şablonları ile küme oluşturma zamanında değeri. Ancak, bir *en düşük değer* (oluşturma zamanında doğrulanmış) hem Kubernetes hem de Azure CNI için aşağıda gösterilmektedir:

| Ağ | Minimum | Maksimum |
| -- | :--: | :--: |
| Azure CNI | 30 | 250 |
| Kubernetes | 30 | 110 |

AKS, yönetilen bir hizmet olduğundan, eklentiler ve pod'ları dağıtma ve yönetme kümenin bir parçası sağlar. Geçmişte, kullanıcıların tanımlayabilirsiniz bir `maxPods` yönetilen pod çalıştırmak gerekli değerden daha düşük değer (örnek: 30), AKS artık en düşük ile pod'ların sayısını hesaplar: ((maxPods veya (maxPods * vm_count)) > Yönetilen eklenti pod'ların en az.

Kullanıcılara en düşük geçersiz kılamaz `maxPods` doğrulama.

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

<!-- LINKS - external -->

[auto-scaler]: https://github.com/kubernetes/autoscaler
[cordon-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/
[hexadite]: https://github.com/Hexadite/acs-keyvault-agent
[admission-controllers]: https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/
[keyvault-flexvolume]: https://github.com/Azure/kubernetes-keyvault-flexvol
