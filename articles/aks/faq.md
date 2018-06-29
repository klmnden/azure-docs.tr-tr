---
title: Azure Kubernetes hizmeti için sık sorulan sorular
description: Bazı Azure Kubernetes hizmet hakkında genel soruların yanıtlarını içerir.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 6/25/2018
ms.author: iainfou
ms.openlocfilehash: ffd81835de82cc5a00b3f6705a7607a51bb3bfa0
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37096460"
---
# <a name="frequently-asked-questions-about-azure-kubernetes-service-aks"></a>Azure Kubernetes hizmet (AKS) hakkında sık sorulan sorular

Bu makale adresleri soruları Azure Kubernetes hizmet (AKS) hakkında sık.

## <a name="which-azure-regions-provide-the-azure-kubernetes-service-aks-today"></a>Hangi Azure bölgeleri Azure Kubernetes hizmet (AKS) Bugün sağlıyor?

- Avustralya Doğu
- Orta Kanada
- Doğu Kanada
- Orta ABD
- Doğu ABD
- Doğu US2
- Kuzey Avrupa
- Birleşik Krallık Güney
- Batı Avrupa
- Batı ABD
- Batı ABD 2

## <a name="are-security-updates-applied-to-aks-agent-nodes"></a>Güvenlik güncelleştirmeleri AKS Aracısı düğümlerine uygulanır?

Azure güvenlik yamaları gecelik bir zamanlamaya göre kümenizdeki düğümlerin otomatik olarak uygular. Ancak, düğümleri yeniden başlatılır sağlamaktan sorumludur gerektiği gibi. Düğümü yeniden başlatmalar gerçekleştirmek için birkaç seçeneğiniz vardır:

- El ile Azure portalında veya Azure CLI aracılığıyla.
- AKS kümenizi yükselterek. Yükseltmeler otomatik olarak küme [cordon ve düğüm boşaltma](https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/), bunları son Ubuntu görüntüsüyle ardından çevrimiçine yedekleyin. İşletim sistemi görüntüsü, düğümlerde geçerli küme sürümde belirterek Kubernetes sürümleri değiştirmeden güncelleştirme `az aks upgrade`.
- Kullanarak [Kured](https://github.com/weaveworks/kured), Kubernetes için bir açık kaynak önyükleme arka plan programı. Kured çalışırken bir [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) ve her düğüm için yeniden başlatma gerekli olduğunu belirten bir dosyasının varlığı, izler. Ardından aynı cordon ve daha önce açıklanan boşaltma işlemi aşağıdaki kümede yeniden başlatmalar düzenler.

## <a name="does-aks-support-node-autoscaling"></a>AKS düğümü otomatik ölçeklendirmeyi destekliyor mu?

Evet, otomatik ölçeklendirmeyi aracılığıyla kullanılabilir [Kubernetes autoscaler] [ auto-scaler] Kubernetes 1.10 itibariyle.

## <a name="does-aks-support-kubernetes-role-based-access-control-rbac"></a>AKS Kubernetes rol tabanlı erişim denetimi (RBAC) destekliyor mu?

Evet, RBAC, Azure CLI veya Azure Resource Manager şablonu AKS kümeden dağıtırken etkinleştirilebilir. Bu işlev, Azure portalında yakında gelecektir.

## <a name="what-kubernetes-admission-controllers-does-aks-support-can-this-be-configured"></a>Hangi Kubernetes giriş denetleyicileri AKS destekliyor mu? Bu yapılandırılabilir mi?

AKS destekleyen aşağıdaki [giriş denetleyicileri][admission-controllers]:

* NamespaceLifecycle
* LimitRanger
* HizmetHesabı
* DefaultStorageClass
* DefaultTolerationSeconds
* MutatingAdmissionWebhook
* ValidatingAdmissionWebhook
* ResourceQuota
* DenyEscalatingExec
* AlwaysPullImages

AKS giriş denetleyicileri listesini değiştirmek şu anda mümkün değildir.

## <a name="can-i-deploy-aks-into-my-existing-virtual-network"></a>Varolan sanal ağımla AKS dağıtabilir miyim?

Evet, kullanarak varolan bir sanal ağ içinde bir AKS kümesi dağıtabilirsiniz [Gelişmiş Ağ özellik](https://github.com/MicrosoftDocs/azure-docs/blob/master/articles/aks/networking-overview.md).

## <a name="is-azure-key-vault-integrated-with-aks"></a>Azure anahtar kasası AKS ile tümleştirilir?

AKS yerel olarak Azure anahtar kasası ile şu anda tümleştirilmiş değil. Ancak, topluluk çözümleri gibi vardır [acs-keyvault-Aracıdan Hexadite][hexadite].

## <a name="can-i-run-windows-server-containers-on-aks"></a>Windows Server kapsayıcıları AKS üzerinde çalıştırabilir miyim?

Windows Server kapsayıcıları çalıştırmak için Windows Server tabanlı düğümleri çalıştırmanız gerekir. Windows Server tabanlı düğümleri şu anda AKS içinde kullanılamaz. Windows Server kapsayıcıları Azure Kubernetes çalıştırmak ihtiyacınız varsa, lütfen bkz [acs altyapısı belgelerine](https://github.com/Azure/acs-engine/blob/master/docs/kubernetes/windows.md).

## <a name="why-are-two-resource-groups-created-with-aks"></a>Neden iki kaynak grubu ile AKS oluşturulur?

Her AKS dağıtım iki kaynak grubu yayar. İlk oluşturulur ve yalnızca Kubernetes Hizmet kaynağı içerir. AKS kaynak sağlayıcısı gibi bir ada sahip ikinci bir dağıtım sırasında otomatik olarak oluşturur. *MC_myResourceGroup_myAKSCluster_eastus*. İkinci kaynak grubu VM gibi kümesi ile ilişkili tüm altyapı kaynakları içeren ağ ve depolama. Kaynak temizleme basitleştirmek için oluşturulur.

Depolama hesapları veya ayrılmış genel IP adresi gibi AKS kümenizi kullanılacak kaynakları oluşturuyorsanız, otomatik olarak oluşturulan kaynak grubunda yerleştirmelisiniz.

## <a name="does-aks-offer-a-service-level-agreement"></a>Bir hizmet düzeyi sözleşmesi AKS sunar?

Bir hizmet düzeyi sözleşmesi (SLA), sağlayıcı yayınlanan hizmet düzeyi karşılanmadı hizmet maliyetini müşteri geri ödemenin kabul eder. AKS kendisi boş olduğundan, hiçbir ücret geri ödemenin kullanılabilir ve bu nedenle hiçbir resmi SLA yoktur. Ancak, biz Kubernetes API sunucunuz için en az % 99,5 kullanılabilirliğini bulundurmak arama.

<!-- LINKS - external -->
[auto-scaler]: https://github.com/kubernetes/autoscaler
[hexadite]: https://github.com/Hexadite/acs-keyvault-agent
[admission-controllers]: https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/
