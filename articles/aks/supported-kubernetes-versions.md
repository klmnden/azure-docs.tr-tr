---
title: Azure Kubernetes Service'te desteklenen bir Kubernetes sürümleri
description: Azure Kubernetes Service (AKS) kümesinin yaşam döngüsü ve Kubernetes sürümü destek ilkesi anlama
services: container-service
author: sauryadas
ms.service: container-service
ms.topic: article
ms.date: 05/20/2019
ms.author: saudas
ms.openlocfilehash: 2d555908007f4e43a38b6d0eff909ef5050878ea
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67069671"
---
# <a name="supported-kubernetes-versions-in-azure-kubernetes-service-aks"></a>Desteklenen Kubernetes sürümlerini Azure Kubernetes Service (AKS)

Kubernetes topluluğu, küçük sürümleri yaklaşık üç ayda bir yayınlamaktadır. Bu yayınlar yeni özellikler ve geliştirmeler içerir. Düzeltme eki yayınları daha sıktır (bazen haftalık) ve yalnızca bir küçük sürümdeki kritik hata düzeltmelerine yöneliktir. Bu düzeltme eki sürümler, güvenlik açıklarını veya çok sayıda müşteriler ve üretimde Kubernetes üzerinde çalışan ürünleri etkileyen önemli hatalar için düzeltmeler içerir.

AKS doğrulamak ve Yukarı Akış yayın, yayın kararlılığını tabi 30 gün içinde yeni Kubernetes sürümlerini yayınlamak amaçlar.

## <a name="kubernetes-versions"></a>Kubernetes sürümleri

Kubernetes kullanan standart [Semantic Versioning](https://semver.org/) sürüm oluşturma düzeni. Başka bir deyişle, her bir Kubernetes sürümü bu numaralandırma düzeni izler:

```
[major].[minor].[patch]

Example:
  1.12.14
  1.12.15
  1.13.7
```

Her bir sayının sürümü önceki sürümle genel uyumluluk gösterir:

* API ana sürüm değişikliği uyumsuz olduğunda değişiklikler veya geriye dönük uyumluluk bozuk olabilir.
* Diğer ikincil sürümleri için geriye dönük uyumlu olan işlevselliği değişiklik yapıldığında ikincil sürümleri değiştirin.
* Geriye dönük uyumlu hata düzeltmeleri, düzeltme eki sürümleri değişiklik yapılır.

Genel olarak, kullanıcıların runtime'daki, örneğin, bir üretim kümesi üzerinde ise ikincil sürüm en son düzeltme eki sürümünü çalıştırılacak endeavor *1.13.6* ve *1.13.7* en son kullanılabilir düzeltme eki için kullanılabilir sürüm yok *1.13* serisi, yükseltme *1.13.7* kümenizi tam olarak düzeltme eki ve desteklenen emin olarak.

## <a name="kubernetes-version-support-policy"></a>Kubernetes sürüm destek ilkesi

AKS, Kubernetes’in dört küçük sürümünü destekler:

* AKS (N) yayınlanan geçerli alt sürümü
* Önceki üç küçük sürüm. Desteklenen her küçük sürüm, ayrıca iki kararlı düzeltme eki de destekler.

Bu, "N-3"-(N (son sürüm) - 3 (ikincil sürümleri)) adı verilir.

Örneğin, AKS sunarsa *1.13.x* Bugün, aşağıdaki sürümleri için destek sağlanır:

Yeni alt sürümü desteklenen sürüm listesi
-----------------        ----------------------
1.13.x                   1.12.a, 1.12.b, 1.11.a, 1.11.b, 1.10.a, 1.10.b

Burada "x" ve "bir" ve ".b" temsili düzeltme eki sürümleri.

Sürüm değişikliklerini ve beklentileri ile ilgili iletişim hakkında daha fazla bilgi için "İletişimleri" aşağıdaki bakın.

Yeni bir ikincil sürüm eklendiğinde, desteklenen en eski ikincil sürüm ve yama sürümler kullanım dışı ve kaldırıldı. Geçerli sürüm listesi destekleniyorsa örneği için:

<a name="supported-version-list"></a>Desteklenen sürüm listesi
----------------------
1.12.a, 1.12.b, 1.11.a, 1.11.b, 1.10.a, 1.10.b, 1.9.a, 1.9.b

Ve AKS 1.13.x sürümleri, bu 1.9.x sürümleri (tüm 1.9 sürümleri) kaldırılacak anlamına gelir ve desteği.

> [!NOTE]
> Lütfen unutmayın, müşterilerin desteklenmeyen bir Kubernetes sürümünü çalıştırıyorsanız, bunlar kümesi için destek isteğinde bulunurken yükseltme istenir. Tarafından desteklenmeyen Kubernetes sürümleri çalıştıran kümeleri alınmamıştır [AKS destek ilkeleri](https://docs.microsoft.com/azure/aks/support-policies).


Yukarıdakilerin yanı sıra küçük sürümlerinde, AKS iki son destekler *düzeltme eki** belirli bir alt sürüm sürümleri. Örneğin, aşağıdaki desteklenen sürümleri verilen:

<a name="supported-version-list"></a>Desteklenen sürüm listesi
----------------------
1.12.1, 1.12.2, 1.11.4, 1.11.5

Yukarı Akış Kubernetes 1.12.3 ve 1.11.6 yayımlanan ve AKS sürümleri bu sürümleri düzeltme eki, en eski düzeltme eki sürümler kullanım dışı ve kaldırıldı ve desteklenen sürüm listesi olur:

<a name="supported-version-list"></a>Desteklenen sürüm listesi
----------------------
1.12.*2*, 1.12.*3*, 1.11.*5*, 1.11.*6*

### <a name="communications"></a>Haberleşme

* Yeni **küçük** Kubernetes sürümleri
  * Tüm kullanıcılar, yeni sürüm ve hangi sürümü kaldırılacak bildirilir.
  * Sürümünü çalıştıran müşteriler **kaldırılacak** sahip oldukları bildirilecek **60 gün** (örn. alt sürümü) desteklenen bir sürüme yükseltmelisiniz.
* Yeni **düzeltme eki** Kubernetes sürümleri
  * Tüm kullanıcılar yayımlanan yeni düzeltme eki sürümü ve en son düzeltme eki sürümüne yükseltmek için bildirilir.
  * Kullanıcıların **30 gün** yeni, desteklenen bir düzeltme eki sürüme yükseltmelisiniz. Kullanıcıların **30 gün** eski kaldırılmadan önce bir desteklenen bir düzeltme sürümüne yükseltmek için.

AKS tanımlar "Genel, tüm SLO etkin kullanıma" / kalite hizmet ölçümleri ve tüm bölgelerde kullanılabilir.

> [!NOTE]
> Müşteriler Kubernetes sürümünü yayımlar ve desteklenen bir sürüme yükseltmek için verilen bir ikincil sürüm kullanım dışı ve kaldırılan kullanıcılar olduğunda bırakılanların, 60 gün bildirilir. Düzeltme eki yayınlar söz konusu olduğunda, müşterilerin desteklenen bir sürüme yükseltmek için 30 gün verilir.

Bildirimleri üzerinden gönderilir:

* [AKS sürüm notları](https://aka.ms/aks/releasenotes)
* Azure portalı bildirimleri
* [Azure güncelleştirme kanalı][azure-update-channel]

### <a name="policy-exceptions"></a>İlkesi özel durumları

AKS, ekleme veya hatalar veya güvenlik sorunları bulunuruz olmadan etkileyen bir veya daha fazla kritik üretim için tanımlanan yeni/mevcut sürümleri kaldırma hakkını saklı tutar.

Belirli bir düzeltme eki yayınlar atlanabiliyor veya hata veya güvenlik sorununu önem derecesine bağlı olarak dağıtım hızlandırılmış.

### <a name="azure-portal-and-cli-default-versions"></a>Azure portalı ve CLI varsayılan sürümleri

Portalında veya Azure CLI ile bir AKS kümesi dağıtırken, kümenin her zaman en son düzeltme eki ve alt sürüm N-1 ayarlayın. Örneğin, AKS destekliyorsa *1.13.x*, *1.12.a* + *1.12.b*, *1.11.a*  +   *1.11.b*, *1.10.a* + *1.10b*, yeni küme için varsayılan sürüm *1.12.b*.

AKS, varsayılan olarak bilinen, kararlı müşterilere sağlamak için N-1 için (minor.latestPatch, örn 1.12.b) ve sürümü, varsayılan olarak düzeltme eki.

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

Kullanıyorsanız *n-4* sürümü, destek dışında olan ve yükseltmeniz istenir. Sürüm n-4 n-3, yükseltme işlemi başarılı olursa, destek ilkelerimiz içinde sunulmuştur. Örneğin:

- Desteklenen AKS sürümleri varsa *1.13.x*, *1.12.a* + *1.12.b*, *1.11.c*  +  *1.11d*, ve *1.10.e* + *1.10F* ve bulunduğunuz *1.9.g* veya *1.9.h*, dışında bir destek sağlar.
- Yükseltme işlemi *1.9.g* veya *1.9.h* için *1.10.e* veya *1.10.f* başarılı, geri olan destek ilkelerimiz içinde.

Şundan eski sürümlere yükseltme *n-4* desteklenmez. Bu gibi durumlarda, müşterilerin yeni AKS küme oluşturma ve iş yüklerini yeniden öneririz.

**'/ Desteği' ne anlama geliyor**

'Dışında Destek', çalıştırmakta olduğunuz sürüm desteklenen sürümleri listesinin dışında olan ve Destek isteğinde bulunurken kümenin desteklenen bir sürüme yükseltmeniz istenir anlamına gelir. Buna ek olarak, AKS herhangi bir çalışma zamanı veya diğer garanti kümeleri dışında Desteklenen sürümlerin listesi için yapmaz.

**Bir müşteri desteklenmeyen bir ikincil sürüm ile bir Kubernetes kümesi ölçeklendirildiğinde ne olur?**

AKS tarafından desteklenmeyen ikincil sürümleri için içe veya dışa ölçeklendirme çalışmaya devam eder.

**Bir müşteri Kubernetes sürümü sonsuza kadar kalabileceği?**

Evet. Ancak, küme AKS tarafından desteklenen sürümlerinden birinde değilse, kümenin dışında AKS destek ilkeleri ' dir. Azure otomatik olarak kümenizi yükseltmek veya silin.

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
