---
title: Azure Kubernetes Service için sık sorulan sorular
description: Azure Kubernetes hizmeti hakkında genel soruların yanıtlarını sağlar.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 07/11/2018
ms.author: iainfou
ms.openlocfilehash: 915f74df69596b1677a0e03770e076ae50efc609
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39001254"
---
# <a name="frequently-asked-questions-about-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) hakkında sık sorulan sorular

Bu makalede adresleri sorular Azure Kubernetes Service (AKS) hakkında sık kullanılan.

## <a name="which-azure-regions-provide-the-azure-kubernetes-service-aks-today"></a>Hangi Azure bölgeleri, Azure Kubernetes Service (AKS) Bugün sağlar?

Azure Kubernetes hizmeti bkz [bölgeler ve kullanılabilirlik] [ aks-regions] tam listesi için belgelere.

## <a name="are-security-updates-applied-to-aks-agent-nodes"></a>Güvenlik güncelleştirmeleri için AKS aracı düğümleri uygulandı?

Azure güvenlik yamaları düğümleri gecelik bir zamanlamaya göre otomatik olarak uygulanır. Ancak, düğümleri yeniden başlatılır sağlamaktan sorumlu olduğunuz gerektiğinde. Düğümü yeniden başlatma işlemlerini gerçekleştirmek için birkaç seçeneğiniz vardır:

- El ile Azure portal veya Azure CLI ile.
- AKS kümenizi yükseltme tarafından. Yükseltme otomatik olarak küme [kordon altına alma ve düğüm boşaltma](https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/), bunları en son bir Ubuntu görüntüsünü ile ardından çevrimiçine yedekleyin. Kubernetes sürümleri geçerli küme sürümünde belirterek değiştirmeden, düğümlerde işletim sistemi görüntüsü güncelleştirme `az aks upgrade`.
- Kullanarak [Kured](https://github.com/weaveworks/kured), Kubernetes için bir açık kaynak önyükleme arka plan programı. Kured çalışırken bir [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) ve her düğüm için bir yeniden başlatma gerekli olduğunu belirten bir dosyanın varlığını izler. Ardından aynı cordon ve daha önce açıklanan boşaltma işlemi aşağıdaki küme genelinde yeniden düzenler.

## <a name="does-aks-support-node-autoscaling"></a>AKS düğümü otomatik ölçeklendirme destekliyor mu?

Evet, otomatik ölçeklendirme aracılığıyla kullanılabilir [Kubernetes otomatik ölçeklendiricinin] [ auto-scaler] Kubernetes 1.10 itibaren.

## <a name="does-aks-support-kubernetes-role-based-access-control-rbac"></a>AKS, Kubernetes rol tabanlı erişim denetimi (RBAC) destekliyor mu?

Evet, RBAC, Azure CLI veya Azure Resource Manager şablonundan bir AKS kümesi dağıtırken etkinleştirilebilir. Bu işlev yakında Azure portalında gelir.

## <a name="what-kubernetes-admission-controllers-does-aks-support-can-this-be-configured"></a>Hangi Kubernetes giriş denetleyicileri AKS destekliyor mu? Bu yapılandırılabilir mi?

AKS aşağıdakileri destekler [giriş denetleyicileri][admission-controllers]:

* NamespaceLifecycle
* LimitRanger
* ServiceAccount
* DefaultStorageClass
* DefaultTolerationSeconds
* MutatingAdmissionWebhook
* ValidatingAdmissionWebhook
* ResourceQuota
* DenyEscalatingExec
* AlwaysPullImages

AKS giriş denetleyicileri listesini değiştirmek şu anda mümkün değildir.

## <a name="can-i-deploy-aks-into-my-existing-virtual-network"></a>AKS my mevcut bir sanal ağa dağıtabilir miyim?

Evet, var olan bir sanal ağ kullanarak bir AKS kümesi dağıtabilirsiniz [Gelişmiş ağ özelliği](https://github.com/MicrosoftDocs/azure-docs/blob/master/articles/aks/networking-overview.md).

## <a name="is-azure-key-vault-integrated-with-aks"></a>Azure Key Vault, AKS ile tümleşiktir?

AKS şu anda Azure anahtar kasası ile yerel olarak tümleşikleştirilmemiştir. Ancak, gibi topluluk çözümleri vardır [acs-keyvault-Aracıdan Hexadite][hexadite].

## <a name="can-i-run-windows-server-containers-on-aks"></a>Windows Server kapsayıcıları AKS üzerinde çalıştırabilir mi?

Windows Server kapsayıcıları çalıştırmak için Windows Server tabanlı düğümleri çalıştırmanız gerekir. Windows Server tabanlı düğümler şu anda AKS içinde kullanılamaz. Ancak, Azure Container Instances Windows kapsayıcıları zamanlayabilir ve AKS kümenizin bir parçası yönetmek için Virtual Kubelet kullanabilirsiniz. Daha fazla bilgi için [kullanım Virtual Kubelet ile AKS][virtual-kubelet].

## <a name="why-are-two-resource-groups-created-with-aks"></a>İki kaynak grubu, AKS ile neden oluşturulur?

Her bir AKS dağıtımı iki kaynak grubu kapsar. İlk oluşturulur ve yalnızca Kubernetes Hizmet kaynağı içeriyor. AKS kaynak sağlayıcısı gibi bir ada sahip ikinci bir dağıtımı sırasında otomatik olarak oluşturur. *MC_myResourceGroup_myAKSCluster_eastus*. İkinci kaynak grubunun tüm VM gibi kümesi ile ilişkili altyapı kaynaklarını içeren ağ ve depolama. Kaynak temizleme işlemi basitleştirmek için oluşturulur.

Depolama hesapları veya ayrılmış genel IP adresi gibi bir AKS kümesi ile kullanılacak kaynaklar oluşturuyorsanız otomatik olarak oluşturulan kaynak grubunda yerleştirmeniz gerekir.

## <a name="does-aks-offer-a-service-level-agreement"></a>AKS bir hizmet düzeyi sözleşmesi sunar?

Bir hizmet düzeyi sözleşmesi (SLA), sağlayıcı, yayımlanan bir hizmet düzeyi değil karşılanması gereken müşteri için hizmeti maliyetini karşılamayı kabul eder. AKS kendi ücretsiz olduğundan, hiçbir ücret karşılamayı kullanılabilir ve bu nedenle biçimsel SLA yoktur. Ancak biz Kubernetes API sunucusu için en az % 99,5 kullanılabilirliği sürdürmek arama.

<!-- LINKS - internal -->

[aks-regions]: ./container-service-quotas.md
[virtual-kubelet]: virtual-kubelet.md

<!-- LINKS - external -->
[auto-scaler]: https://github.com/kubernetes/autoscaler
[hexadite]: https://github.com/Hexadite/acs-keyvault-agent
[admission-controllers]: https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/
