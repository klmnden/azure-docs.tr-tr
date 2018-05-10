---
title: Azure Kubernetes hizmeti için sık sorulan sorular
description: Bazı Azure Kubernetes hizmet hakkında genel soruların yanıtlarını içerir.
services: container-service
author: neilpeterson
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/09/2018
ms.author: nepeters
ms.openlocfilehash: d03f906f0cf4d22772388a589424877d8bb2f8ce
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="frequently-asked-questions-about-azure-kubernetes-service-aks"></a>Azure Kubernetes hizmet (AKS) hakkında sık sorulan sorular

Bu makale adresleri soruları Azure Kubernetes hizmet (AKS) hakkında sık.

> [!IMPORTANT]
> Azure Kubernetes Hizmeti (AKS), şu anda **önizleme** aşamasındadır. Önizlemeler, [ek kullanım koşullarını](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) kabul etmeniz şartıyla kullanımınıza sunulur. Bu özelliğin bazı yönleri genel kullanıma açılmadan önce değişebilir.
>

## <a name="which-azure-regions-provide-the-azure-kubernetes-service-aks-today"></a>Hangi Azure bölgeleri Azure Kubernetes hizmet (AKS) Bugün sağlıyor?

- Orta Kanada
- Doğu Kanada
- Orta ABD
- Doğu ABD
- Batı Avrupa

## <a name="when-will-additional-regions-be-added"></a>Ek bölgeler zaman eklenir?

Ek bölgeler talep arttıkça eklenir.

## <a name="are-security-updates-applied-to-aks-agent-nodes"></a>Güvenlik güncelleştirmeleri AKS Aracısı düğümlerine uygulanır?

Azure güvenlik yamaları gecelik bir zamanlamaya göre kümenizdeki düğümlerin otomatik olarak uygular. Ancak, düğümleri yeniden başlatılır sağlamaktan sorumludur gerektiği gibi. Düğümü yeniden başlatmalar gerçekleştirmek için birkaç seçeneğiniz vardır:

- El ile Azure portalında veya Azure CLI aracılığıyla.
- AKS kümenizi yükselterek. Yükseltmeler otomatik olarak küme [cordon ve düğüm boşaltma](https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/), bunları son Ubuntu görüntüsüyle ardından çevrimiçine yedekleyin. İşletim sistemi görüntüsü, düğümlerde geçerli küme sürümde belirterek Kubernetes sürümleri değiştirmeden güncelleştirme `az aks upgrade`.
- Kullanarak [Kured](https://github.com/weaveworks/kured), Kubernetes için bir açık kaynak önyükleme arka plan programı. Kured çalışırken bir [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) ve her düğüm için yeniden başlatma gerekli olduğunu belirten bir dosyasının varlığı, izler. Ardından bu yeniden başlatma aynı cordon ve daha önce açıklanan boşaltma işlemi aşağıdaki kümede düzenler.

## <a name="do-you-recommend-customers-use-acs-or-aks"></a>Kullanım ACS veya AKS müşteriler önerilir?

AKS önizlemede kalırken, ACS Kubernetes kullanarak üretim kümeleri oluşturma öneririz veya [acs altyapısı](https://github.com/azure/acs-engine). AKS kavram kanıtı dağıtımları ve geliştirme ve test ortamları için kullanın.

## <a name="when-will-acs-be-deprecated"></a>Ne zaman ACS kullanım dışı kalacaktır?

ACS AKS İST olduğu zaman kullanım dışı kalacaktır Kümeler için AKS geçirmek için bu tarihten sonraki 12 ay sahip olur. 12 aylık dönem boyunca tüm ACS işlemleri çalıştırabilirsiniz.

## <a name="does-aks-support-node-autoscaling"></a>AKS düğümü otomatik ölçeklendirmeyi destekliyor mu?

Düğüm otomatik ölçeklendirmeyi desteklenmez, ancak yol haritası üzerinde değil. Bu açık kaynaklıdır bir göz atalım isteyebilirsiniz [otomatik ölçeklendirmeyi uygulama][auto-scaler].

## <a name="does-aks-support-kubernetes-role-based-access-control-rbac"></a>AKS Kubernetes rol tabanlı erişim denetimi (RBAC) destekliyor mu?

Hayır, RBAC AKS içinde şu anda desteklenmiyor ancak yakında kullanıma sunulacaktır.

## <a name="can-i-deploy-aks-into-my-existing-virtual-network"></a>Varolan sanal ağımla AKS dağıtabilir miyim?

Hayır, henüz kullanılabilir değil ancak yakında kullanıma sunulacaktır.

## <a name="is-azure-key-vault-integrated-with-aks"></a>Azure anahtar kasası AKS ile tümleştirilir?

Hayır, bu değildir ancak bu tümleştirme planlanmış. Bu arada, aşağıdaki çözümden denemek [Hexadite][hexadite].

## <a name="can-i-run-windows-server-containers-on-aks"></a>Windows Server kapsayıcıları AKS üzerinde çalıştırabilir miyim?

Hayır, Windows Server kapsayıcıları çalıştırılamıyor AKS Windows Server tabanlı aracısı düğümleri, şu anda sağlamaz. Windows Server kapsayıcıları Azure Kubernetes çalıştırmak ihtiyacınız varsa, lütfen bkz [acs altyapısı belgelerine](https://github.com/Azure/acs-engine/blob/master/docs/kubernetes/windows.md).

## <a name="why-are-two-resource-groups-created-with-aks"></a>Neden iki kaynak grubu ile AKS oluşturulur?

Her AKS dağıtım iki kaynak grubu yayar. İlk oluşturulur ve yalnızca AKS kaynak içerir. AKS kaynak sağlayıcısı gibi bir ada sahip ikinci bir dağıtım sırasında otomatik olarak oluşturur. *MC_myResourceGRoup_myAKSCluster_eastus*. İkinci kaynak grubu VM gibi kümesi ile ilişkili tüm altyapı kaynakları içeren ağ ve depolama. Kaynak temizleme basitleştirmek için oluşturulur.

Depolama hesapları veya ayrılmış genel IP adresi gibi AKS kümenizi kullanılacak kaynakları oluşturuyorsanız, otomatik olarak oluşturulan kaynak grubunda yerleştirmelisiniz.

<!-- LINKS - external -->
[auto-scaler]: https://github.com/kubernetes/autoscaler
[hexadite]: https://github.com/Hexadite/acs-keyvault-agent