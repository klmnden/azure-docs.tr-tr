---
title: İşleç en iyi uygulamalar - Azure Kubernetes Hizmetleri (AKS) kümesini ayırma
description: Azure Kubernetes Service (AKS) yalıtım küme işleci en iyi uygulamaları öğrenin
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: conceptual
ms.date: 11/26/2018
ms.author: iainfou
ms.openlocfilehash: 94aaa72497a8a5f171d6b42f59a3c5b507c71492
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60465315"
---
# <a name="best-practices-for-cluster-isolation-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) kümesi yalıtımı için en iyi uygulamalar

Azure Kubernetes Service (AKS) kümelerini yönetirken, genellikle takımlar ve iş yüklerini yalıtmak gerekir. AKS, çok kiracılı kümeleri çalıştırın ve kaynakları yalıtma nasıl esneklik sağlar. Kubernetes'te yatırımınızı en üst düzeye çıkarmak için bu çok kiracılılık ve yalıtım özellikleri anladım uygulanan ve.

Bu en iyi yöntemler makalesi küme operatörleri için yalıtım odaklanır. Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Çok kiracılı kümeleri ve kaynak ayırma için planlama
> * Mantıksal veya fiziksel yalıtım AKS kümelerinizi kullanın

## <a name="design-clusters-for-multi-tenancy"></a>Çoklu kiracı tasarımı kümeleri

Kubernetes gruplamanıza olanak tanıyan özellikler, takımlar ve aynı küme iş yüklerini yalıtmak sağlar. En az sağlamak için hedef olmalıdır ayrıcalıklar, her takım gereken kaynakları için kapsamlı sayısı. A [Namespace] [ k8s-namespaces] Kubernetes'te mantıksal yalıtım sınırı oluşturur. Ek kubernetes özelliklerini ve konularını yalıtım ve çok kiracılılık için aşağıdaki alanları içerir:

* **Zamanlama** kaynak kotaları ve pod kesintisi bütçelerini gibi temel özellikleri içerir. Bu özellikler hakkında daha fazla bilgi için bkz. [aks'deki temel Zamanlayıcı özellikleri için en iyi yöntemler][aks-best-practices-scheduler].
  * Daha gelişmiş Zamanlayıcı özellikler taints ve tolerations, düğüm seçicileri ve düğüm ve pod benzeşim veya benzeşim karşıtlığı içerir. Bu özellikler hakkında daha fazla bilgi için bkz. [aks'deki Gelişmiş Zamanlayıcı özellikleri için en iyi yöntemler][aks-best-practices-advanced-scheduler].
* **Ağ** pod'ları içeri ve dışarı, trafik akışını denetlemek için ağ ilkeleri kullanımını içerir.
* **Kimlik doğrulama ve yetkilendirme** kullanıcı rol tabanlı erişim denetimi (RBAC) ve Azure Active Directory (AD) tümleştirmesi, pod kimlikler ve gizli anahtarları Azure Key Vault'ta içerir. Bu özellikler hakkında daha fazla bilgi için bkz. [en iyi uygulamalar için kimlik doğrulama ve yetkilendirme aks'deki][aks-best-practices-identity].
* **Kapsayıcıları** pod güvenlik ilkeleri, pod güvenlik kapsamları, tarama görüntüleri ve güvenlik açıkları için çalışma zamanları içerir. Temel alınan düğüme kapsayıcı erişimi kısıtlamak için uygulama Armor veya Seccomp (güvenli bilgi işlem) kullanarak da içerir.

## <a name="logically-isolate-clusters"></a>Mantıksal olarak küme ayırma

**En iyi uygulama kılavuzunu** -takımlara ve projelere ayırmak için mantıksal yalıtım kullanın. Dağıtım için fiziksel AKS küme sayısını en aza indirmeye çalışmanız takımlar veya uygulamaları ayırmak.

Birden çok iş yükleri, takımlar veya ortamlar için mantıksal yalıtım ile tek bir AKS kümesi kullanılabilir. Kubernetes [ad alanları] [ k8s-namespaces] iş yüklerini ve kaynakları için mantıksal yalıtım sınırı oluşturur.

![Aks'deki bir Kubernetes kümesinin mantıksal yalıtım](media/operator-best-practices-cluster-isolation/logical-isolation.png)

Mantıksal ayrılığı kümelerin genellikle fiziksel olarak izole edilmiş kümeleri daha yüksek bir pod yoğunluk sağlar. Kümede boş yer alan daha fazla işlem kapasitesi yoktur. Kubernetes küme ölçeklendiriciyi ile birleştirildiğinde, yukarı veya aşağı taleplerini karşılayan düğüm sayısını ölçekleyebilirsiniz. Bu en iyi uygulama yaklaşımını için otomatik ölçeklendirme, yalnızca gerekli düğüm sayısını çalıştırmanıza olanak tanır ve maliyetleri en aza indirir.

AKS veya başka bir yerde, Kubernetes ortamlarını tehlikeli çok kiracılı kullanım için tamamen güvenli değildir. Ek güvenlik özellikleri gibi *Pod Güvenlik İlkesi* ve düğümleri için daha fazla ayrıntılı rol tabanlı erişim denetimleri (RBAC) daha zor açıkları yapın. Ancak, tehlikeli çok kiracılı iş yüklerini çalıştırırken doğru güvenlik için bir hiper yönetici yalnızca güvenip güvenmeyeceğini güvenlik düzeyidir. Kubernetes için güvenlik etki alanı, tüm küme, tek bir düğüm olur. Bu tür tehlikeli çok kiracılı iş yükleri için fiziksel olarak izole edilmiş kümeleri kullanmanız gerekir.

## <a name="physically-isolate-clusters"></a>Fiziksel kümeler Ayır

**En iyi uygulama kılavuzunu** -her ayrı bir takım veya uygulama dağıtımı için fiziksel yalıtım kullanımını en aza indirin. Bunun yerine, *mantıksal* yalıtım, önceki bölümde anlatıldığı gibidir.

Küme ayırma yaygın bir yaklaşım, fiziksel olarak ayrı AKS kümeleri kullanmaktır. Bu yalıtım modelde ekipleri ve iş yüklerini kendi AKS kümesi atanır. Bu yaklaşım genellikle iş yükleri veya ekipler yalıtmak için en kolay yolu gibi görünüyor, ancak ek yönetim ve Finans ek yükü ekler. Artık bu birden fazla küme korumak sahip ve tek tek erişim sağlamak ve izinleri atamanız gerekir. Tek tek tüm düğümler için ayrıca faturalandırılırsınız.

![AKS fiziksel yalıtım bireysel kubernetes kümeleri](media/operator-best-practices-cluster-isolation/physical-isolation.png)

Fiziksel olarak ayrı kümeler, genellikle düşük pod yoğunluklu sahiptir. Küme genellikle her bir takım veya iş yükü kendi AKS kümesi olduğu gibi işlem kaynaklarıyla aşırı sağlanmış olur. Genellikle, küçük bir pod'ların sayısını, bu düğümlerde zamanlanır. Kullanılmayan kapasite düğümleri üzerinde uygulama veya hizmete geliştirme için diğer ekipler tarafından kullanılamaz. Bu aşırı kaynakları fiziksel olarak ayrı kümelerde ek maliyetler katkıda bulunun.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede küme yalıtım üzerinde odaklanır. AKS kümesi işlemleri hakkında daha fazla bilgi için aşağıdaki en iyi bakın:

* [Temel Kubernetes Zamanlayıcı Özellikleri][aks-best-practices-scheduler]
* [Gelişmiş Kubernetes Zamanlayıcı Özellikleri][aks-best-practices-advanced-scheduler]
* [Kimlik doğrulama ve yetkilendirme][aks-best-practices-identity]

<!-- EXTERNAL LINKS -->

<!-- INTERNAL LINKS -->
[k8s-namespaces]: concepts-clusters-workloads.md#namespaces
[aks-best-practices-scheduler]: operator-best-practices-scheduler.md
[aks-best-practices-advanced-scheduler]: operator-best-practices-advanced-scheduler.md
[aks-best-practices-identity]: operator-best-practices-identity.md
