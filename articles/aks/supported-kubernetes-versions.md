---
title: Azure Kubernetes Service'te desteklenen bir Kubernetes sürümleri
description: Azure Kubernetes Service (AKS) kümesinin yaşam döngüsü ve Kubernetes sürümü destek ilkesi anlama
services: container-service
author: sauryadas
ms.service: container-service
ms.topic: article
ms.date: 05/20/2019
ms.author: saudas
ms.openlocfilehash: a7b3176e39ccaa0f9ddb1ef45c33ec6902e62f1c
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65956317"
---
# <a name="supported-kubernetes-versions-in-azure-kubernetes-service-aks"></a>Desteklenen Kubernetes sürümlerini Azure Kubernetes Service (AKS)

Kubernetes topluluğu, küçük sürümleri yaklaşık üç ayda bir yayınlamaktadır. Bu yayınlar yeni özellikler ve geliştirmeler içerir. Düzeltme eki yayınları daha sıktır (bazen haftalık) ve yalnızca bir küçük sürümdeki kritik hata düzeltmelerine yöneliktir. Bu düzeltme eki sürümler, güvenlik açıklarını veya çok sayıda müşteriler ve üretimde Kubernetes üzerinde çalışan ürünleri etkileyen önemli hatalar için düzeltmeler içerir.

Alt sürümü içinde kullanılabilir yapılan yeni bir Kubernetes [aks altyapısı] [ aks-engine] günlük bir. AKS Hizmet Düzeyi Hedefi (SLO), yayının kararlılığına bağlı olarak 30 gün içinde AKS kümelerinin küçük sürümü yayınlamayı hedeflemektedir.

## <a name="kubernetes-version-support-policy"></a>Kubernetes sürüm destek ilkesi

AKS, Kubernetes’in dört küçük sürümünü destekler:

- Yukarı Akış (n) olan geçerli alt sürümü yayımlanan
- Önceki üç küçük sürüm. Desteklenen her küçük sürüm, ayrıca iki kararlı düzeltme eki de destekler.

Örneğin, AKS sunarsa *1.13.x* bugün desteği de için sağlanan *1.12.a* + *1.12.b*, *1.11.c*  +  *1.11d*, *1.10.e* + *1.10F* (burada yitirmiş düzeltme eki sürümleri iki en son kararlı yapı değildir).

Yeni bir küçük sürüm sunulduğunda, desteklenen en eski küçük sürüm ve düzletme eki yayınları kullanım dışı olur. Yeni alt sürümü ve gelecek sürümünü emeklilik yayınlanmadan önce 30 gün üzerinden bir duyuru yapılır [Azure güncelleştirme kanalları][azure-update-channel]. Where yukarıdaki örnekte *1.13.x* olan yayımlanan devre dışı bırakılan sürümleridir *1.9.g* + *1.9.h*.

Bir AKS kümesini portalda veya Azure CLI ile dağıttığınızda, küme her zaman n-1 küçük sürümüne ve en son düzeltme ekine ayarlanır. Örneğin, AKS destekliyorsa *1.13.x*, *1.12.a* + *1.12.b*, *1.11.c*  +   *1.11d*, *1.10.e* + *1.10F*, yeni küme için varsayılan sürüm *1.11.b*.

## <a name="list-currently-supported-versions"></a>Şu anda desteklenen sürümler listesi

Hangi sürümlerine bölge ve abonelik için şu anda kullanılabilir olduğunu bulmak için kullanın [az aks get-versions] [ az-aks-get-versions] komutu. Aşağıdaki örnek için kullanılabilen Kubernetes sürümlerini listeler *EastUS* bölgesi:

```azurecli-interactive
az aks get-versions --location eastus --output table
```

Kubernetes sürümü gösteren aşağıdaki örneğe benzer bir çıkış *1.13.5* en son sürümü kullanılabilir:

```
KubernetesVersion    Upgrades
-------------------  ------------------------
1.13.5               None available
1.12.7               1.13.5
1.12.6               1.12.7, 1.13.5
1.11.9               1.12.6, 1.12.7
1.11.8               1.11.9, 1.12.6, 1.12.7
1.10.13              1.11.8, 1.11.9
1.10.12              1.10.13, 1.11.8, 1.11.9
```

## <a name="faq"></a>SSS

**Bir müşteri desteklenmeyen bir ikincil sürüm ile bir Kubernetes kümesi yükseltildiğinde ne olur?**

Kullanıyorsanız *n-4* sürümü, SLO dışında olan. Sonra yükseltme sürümü n-4 n-3 başarılı olursa, SLO geri sunulur. Örneğin:

- Desteklenen AKS sürümleri varsa *1.13.x*, *1.12.a* + *1.12.b*, *1.11.c*  +  *1.11d*, ve *1.10.e* + *1.10F* ve bulunduğunuz *1.9.g* veya *1.9.h*, SLO dışında olan.
- Yükseltme işlemi *1.9.g* veya *1.9.h* için *1.10.e* veya *1.10.f* başarılı, SLO geri olursunuz.

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
[aks-engine]: https://github.com/Azure/aks-engine
[azure-update-channel]: https://azure.microsoft.com/updates/?product=kubernetes-service

<!-- LINKS - Internal -->
[aks-upgrade]: upgrade-cluster.md
[az-aks-get-versions]: /cli/azure/aks#az-aks-get-versions
