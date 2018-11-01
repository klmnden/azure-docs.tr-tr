---
title: Azure Kubernetes Service'te desteklenen bir Kubernetes sürümleri
description: Azure Kubernetes Service (AKS) kümesinin yaşam döngüsü ve Kubernetes sürümü destek ilkesi anlama
services: container-service
author: sauryadas
ms.service: container-service
ms.topic: article
ms.date: 09/21/2018
ms.author: saudas
ms.openlocfilehash: d8da717b83b43395309c695a4f9edaeda8144a8b
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2018
ms.locfileid: "49379204"
---
# <a name="supported-kubernetes-versions-in-azure-kubernetes-service-aks"></a>Desteklenen Kubernetes sürümlerini Azure Kubernetes Service (AKS)

Kubernetes topluluğuyla ikincil sürümleri kabaca her üç ayda bir serbest bırakır. Bu sürümler, yeni özellikler ve geliştirmeler içerir. Düzeltme eki sürümleri (bazen haftalık) daha sık ve yalnızca küçük bir sürümdeki Kritik hata düzeltmeleri yöneliktir. Bu düzeltme eki sürümler, güvenlik açıklarını veya çok sayıda müşteriler ve üretimde Kubernetes üzerinde çalışan ürünleri etkileyen önemli hatalar için düzeltmeler içerir.

Alt sürümü içinde kullanılabilir yapılan yeni bir Kubernetes [acs-engine] [ acs-engine] günlük bir. AKS kümeleri için alt sürüm yayın kararlılığını tabi 30 gün içinde serbest AKS hizmet düzeyi hedefi (SLO) hedefler.

## <a name="kubernetes-version-support-policy"></a>Kubernetes sürümü destek ilkesi

AKS, dört alt Kubernetes sürümlerini destekler:

- Yukarı Akış (n) olan geçerli alt sürümü yayımlanan
- İkincil bir üç önceki sürümleri. Her desteklenen podverze iki kararlı düzeltme ekleri de destekler.

Örneğin, AKS sunarsa *1.11.x* bugün desteği de için sağlanan *1.10.a* + *1.10.b*, *1.9.c*  +  *1.9d*, *1.8.e* + *1.8F* (burada yitirmiş düzeltme eki sürümleri iki en son kararlı yapı değildir).

Yeni bir ikincil sürüm eklendiğinde, desteklenen en eski ikincil sürüm ve yama sürümleri kullanımdan kaldırıldı. Yeni alt sürümü ve gelecek sürümünü emeklilik yayınlanmadan önce 15 gün duyuru Azure güncelleştirme kanalları yapılır. Where yukarıdaki örnekte *1.11.x* olan yayımlanan devre dışı bırakılan sürümleridir *1.7.g* + *1.7.h*.

Portalında veya Azure CLI ile bir AKS kümesi dağıtırken, küme her zaman en son düzeltme eki ve n-1 podverze ayarlayın. Örneğin, AKS destekliyorsa *1.11.x*, *1.10.a* + *1.10.b*, *1.9.c* + *1.9 d*, *1.8.e* + *1.8F*, yeni küme için varsayılan sürüm *1.10.b*.

## <a name="list-currently-supported-versions"></a>Şu anda desteklenen sürümler listesi

Hangi sürümlerine bölge ve abonelik için şu anda kullanılabilir olduğunu bulmak için kullanın [az aks get-versions] [ az-aks-get-versions] komutu. Aşağıdaki örnek için kullanılabilen Kubernetes sürümlerini listeler *EastUS* bölgesi:

```azurecli-interactive
az aks get-versions --location eastus --output table
```

Kubernetes sürümü gösteren aşağıdaki örneğe benzer bir çıkış *1.11.3* en son sürümü kullanılabilir:

```
KubernetesVersion    Upgrades
-------------------  ----------------------
1.11.3               None available
1.11.2               1.11.3
1.10.8               1.11.2, 1.11.3
1.10.7               1.10.8, 1.11.2, 1.11.3
1.9.10               1.10.7, 1.10.8
1.9.9                1.9.10, 1.10.7, 1.10.8
1.8.15               1.9.9, 1.9.10
1.8.14               1.8.15, 1.9.9, 1.9.10
```

## <a name="faq"></a>SSS

**Bir müşteri desteklenmeyen bir ikincil sürüm ile bir Kubernetes kümesi yükseltildiğinde ne olur?**

Kullanıyorsanız *n-4* sürümü, SLO dışında olan. Sonra yükseltme sürümü n-4 n-3 başarılı olursa, SLO geri sunulur. Örneğin:

- Desteklenen AKS sürümleri varsa *1.10.a* + *1.10.b*, *1.9.c* + *1.9d*,  *1.8.e* + *1.8F* ve bulunduğunuz *1.7.g* veya *1.7.h*, SLO dışında olan.
- Yükseltme işlemi *1.7.g* veya *1.7.h* için *1.8.e* veya *1.8.f* başarılı, SLO geri olursunuz.

Şundan eski sürümlere yükseltme *n-4* desteklenmez. Bu gibi durumlarda, müşterilerin yeni AKS küme oluşturma ve iş yüklerini yeniden öneririz.

**Bir müşteri desteklenmeyen bir ikincil sürüm ile bir Kubernetes kümesi ölçeklendirildiğinde ne olur?**

AKS tarafından desteklenmeyen ikincil sürümleri için içe veya dışa ölçeklendirme çalışmaya devam eder.

**Bir müşteri Kubernetes sürümü sonsuza kadar kalabileceği?**

Evet. Ancak, küme AKS tarafından desteklenen sürümlerinden birinde değilse kümenin dışında AKS SLO ' dir. Azure otomatik olarak kümenizi yükseltmek veya silin.

**Aracıyı kümenin desteklenen AKS sürümlerden biri değilse, hangi sürümünün ana destek mu?**

Ana, desteklenen en son sürüme otomatik olarak güncelleştirilir.

## <a name="next-steps"></a>Sonraki adımlar

Kümenizi yükseltme hakkında daha fazla bilgi için bkz: [Azure Kubernetes Service (AKS) kümesini yükseltme][aks-upgrade].

<!-- LINKS - External -->
[acs-engine]: https://github.com/Azure/acs-engine

<!-- LINKS - Internal -->
[aks-upgrade]: upgrade-cluster.md
[az-aks-get-versions]: /cli/azure/aks#az-aks-get-versions