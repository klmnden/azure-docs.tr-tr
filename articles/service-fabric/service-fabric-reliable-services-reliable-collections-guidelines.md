---
title: "Yönergeleri & Azure güvenilir koleksiyonlar için öneriler hizmeti doku | Microsoft Docs"
description: "Kılavuzları ve önerileri Service Fabric güvenilir koleksiyonları kullanma"
services: service-fabric
documentationcenter: .net
author: mcoskun
manager: timlt
editor: masnider,rajak
ms.assetid: 62857523-604b-434e-bd1c-2141ea4b00d1
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 5/3/2017
ms.author: mcoskun
ms.openlocfilehash: 053a7bca76362035e428fc11806b3e4f83d00946
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="guidelines-and-recommendations-for-reliable-collections-in-azure-service-fabric"></a>Kılavuzları ve önerileri Azure Service Fabric güvenilir koleksiyonlar için
Bu bölümde, durum Yöneticisi'ni güvenilir ve güvenilir koleksiyonları kullanma yönergeleri sağlar. Amacı kullanıcıların yaygın tehlikesinden yardımcı olmaktır.

Yönergeler koşulları önekli basit öneriler olarak düzenlenir *yapmak*, *düşünün*, *kaçının* ve *sağlamadığı*.

* Okuma işlemleri tarafından döndürülen özel türde bir nesne değiştirmeyin (örneğin, `TryPeekAsync` veya `TryGetValueAsync`). Eşzamanlı koleksiyonları gibi güvenilir koleksiyonları nesneleri ve bir kopyasını bir başvuru döndürür.
* Derin kopyalama değiştirmeden önce bir özel tür döndürülen nesne yapın. Yapılar ve yerleşik türler geçişi değerli olduğundan, onlar üzerinde derin bir kopya yapmak gerekmez.
* Kullanmayın `TimeSpan.MaxValue` zaman aşımları için. Zaman aşımları kilitlenmeleri algılamak için kullanılmalıdır.
* Bunu kabul edilen, iptal, atıldı veya oluşturulduktan sonra bir işlem kullanmayın.
* Numaralandırma oluşturulduğu işlem kapsamı dışında kullanmayın.
* Bir işlem içinde başka bir işlemdeki oluşturmayın `using` deyimi kilitlenmeleri neden.
* Emin olun, `IComparable<TKey>` uygulamasıdır doğru. Sistem bağımlılık alır `IComparable<TKey>` kontrol noktalarına ve satır birleştirmek için.
* Güncelleştirme kilidi belirli bir sınıf kilitlenmelerin önlemek amacıyla güncelleştirmek için bir amaç bir öğesiyle okunurken kullanın.
* 80 Kbayt öğelerinizi (örneğin, TKey + güvenilir sözlüğü için TValue) halde tutmayı düşünün: kadar küçük olursa o kadar iyi olur. Bu, büyük nesne yığın kullanımı yanı sıra disk ve ağ g/ç gereksinimleri miktarını azaltır. Genellikle, yalnızca bir küçük değerinin bir parçası güncelleştirildiğinde yinelenen veri çoğaltmak azaltır. Bu güvenilir sözlükte elde etmek için genel yoludur, satır birden çok satıra geçirmesini.
* Yedekleme kullanmayı düşünün ve olağanüstü durum kurtarma için işlevselliği geri yükleyin.
* Tek bir varlık işlemleri ve birden çok varlık işlemleri karıştırma önlemek (örneğin `GetCountAsync`, `CreateEnumerableAsync`) farklı yalıtım düzeyi nedeniyle aynı işlem.
* InvalidOperationException işleyin. Kullanıcı işlemleri birçok nedenden dolayı sistem tarafından iptal. Örneğin, güvenilir durum Yöneticisi rolüne birincil dışında değiştirirken veya uzun süre çalışan işlem, işlem günlüğü kesilmesi engelliyor. Böyle durumlarda, kullanıcı kendi işlem zaten sonlandırıldı belirten InvalidOperationException alabilirsiniz. Varsayıldığında, işlemin sonlandırılması kullanıcı tarafından bu özel durumu işlemek için en iyi yoldur işlem silmek için istenmemiştir iptal belirteci işaret (veya çoğaltma rolü değiştirildi varsa) onay ve yeni bir oluşturma işlem ve yeniden deneyin.  

Göz önünde bulundurmanız gereken bazı şeyler şunlardır:

* Varsayılan zaman aşımı, tüm güvenilir koleksiyonu API'leri için dört saniyedir. Kullanıcıların çoğunun varsayılan zaman aşımı kullanmanız gerekir.
* Varsayılan iptal belirteci `CancellationToken.None` tüm güvenilir koleksiyonları API'lerde.
* Anahtar türü parametre (*TKey*) güvenilir bir sözlük doğru uygulamalıdır için `GetHashCode()` ve `Equals()`. Anahtarları sabit olmalıdır.
* Güvenilir koleksiyonlar için yüksek kullanılabilirlik elde etmek için her hizmetin en az bir hedef ve en küçük çoğaltma kümesi boyutu 3 olması gerekir.
* Okuma işlemleri ikincil kaydedilen çekirdek değildir sürümleri okuyabilirsiniz.
  Başka bir deyişle, tek bir ikincil okuma verilerinin sürümü yanlış progressed.
  Birincil okuma kararlı her zaman: hiçbir zaman false progressed.

### <a name="next-steps"></a>Sonraki adımlar
* [Güvenilir Koleksiyonlar ile çalışma](service-fabric-work-with-reliable-collections.md)
* [İşlemler ve kilitleri](service-fabric-reliable-services-reliable-collections-transactions-locks.md)
* [Güvenilir durum Yöneticisi'ni ve koleksiyon dahili bileşenleri](service-fabric-reliable-services-reliable-collections-internals.md)
* Veri Yönetimi
  * [Yedekleme ve Geri Yükleme](service-fabric-reliable-services-backup-restore.md)
  * [Bildirimler](service-fabric-reliable-services-notifications.md)
  * [Seri hale getirme ve yükseltme](service-fabric-application-upgrade-data-serialization.md)
  * [Güvenilir durum Yöneticisi yapılandırması](service-fabric-reliable-services-configuration.md)
* Diğer
  * [Güvenilir hizmetler hızlı başlangıç](service-fabric-reliable-services-quick-start.md)
  * [Güvenilir koleksiyonlar için Geliştirici Başvurusu](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
