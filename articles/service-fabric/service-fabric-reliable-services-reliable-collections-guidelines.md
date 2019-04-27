---
title: Kılavuzlar ve öneriler azure'da güvenilir koleksiyonlar için Service Fabric | Microsoft Docs
description: Yönergeler ve öneriler, Service Fabric Reliable Collections kullanma
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chackdan
editor: masnider,rajak,zhol
ms.assetid: 62857523-604b-434e-bd1c-2141ea4b00d1
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 12/10/2017
ms.author: aljo
ms.openlocfilehash: 810427c394c3912142e0a21cf1b5c29b81620afb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60774106"
---
# <a name="guidelines-and-recommendations-for-reliable-collections-in-azure-service-fabric"></a>Azure Service fabric'te güvenilir koleksiyonlar için yönergeler ve öneriler
Bu bölümde, güvenilir durum Yöneticisi ve güvenilir koleksiyonlar kullanma yönergeleri sağlar. Kullanıcıların yaygın görülen tehlikeleri önlemek yardımcı olmaktır.

Kılavuzları basit önerileri şartlarını önek olarak düzenlenir *yapmak*, *düşünün*, *kaçının* ve *olmayan*.

* Okuma işlemleri tarafından döndürülen özel türde bir nesne değiştirmeyin (örneğin, `TryPeekAsync` veya `TryGetValueAsync`). Güvenilir koleksiyonlar, eş zamanlı koleksiyonlar gibi nesneleri ve bir kopya bir başvuru döndürür.
* Derin kopya döndürülen nesne özel türü değiştirmeden önce yapın. Yapılar ve Yerleşik türlerin değere göre geçişi olduğundan, başvuru türü belirlenmiş alanları veya değiştirmek için istediğinize özellikleri içermiyorsa bunlar üzerinde derin kopya yapmak gerekmez.
* Kullanmayın `TimeSpan.MaxValue` zaman aşımları için. Zaman aşımları, kilitlenmeleri algılamak için kullanılmalıdır.
* Bunu kaydedilmiş, durduruldu, bırakılan veya oluşturulduktan sonra bir işlem kullanmayın.
* Bir numaralandırma, oluşturulduğu işlem kapsamı dışında kullanmayın.
* Bir işlem başka bir işlem içinde oluşturmayın `using` deyimi kilitlenmeleri neden olabileceği için.
* Güvenilir durumuyla oluşturmayın `IReliableStateManager.GetOrAddAsync` ve aynı işlemde güvenilir durumunu kullanın. Bu bir InvalidOperationException sonuçlanır.
* Emin olun, `IComparable<TKey>` uygulamasının doğru olduğundan. Bağımlılık sistem vereceğine `IComparable<TKey>` kontrol noktalarına ve satır birleştirme.
* Güncelleştirme kilidi belirli sınıfının kilitlenmeleri önlemek için güncelleştirmek için bir amaç sahip bir öğe okurken kullanın.
* Reliable Collections tutma sayısı 1000'den az olacak şekilde bölüm başına göz önünde bulundurun. Güvenilir koleksiyonlar ile daha fazla öğe daha az öğe ile güvenilir koleksiyonlar üzerinden tercih eder.
* 80 Kbayt öğelerinizi (örneğin, TKey + güvenilir sözlüğün TValue) ayrı tutmaya dikkat edin: küçük iyi. Bu, büyük nesne yığını kullanım yanı sıra disk ve ağ g/ç gereksinimleri miktarını azaltır. Genellikle, değerin yalnızca bir küçük bölümü güncelleştirildiğinde yinelenen verilerin çoğaltılması azaltır. Bu güvenilir bir sözlükte yapmanın yaygın bir yolu, birden fazla satır, satır sonu etmektir.
* Yedekleme kullanmayı deneyin ve geri yükleme işlevselliğinin, olağanüstü durum kurtarma sağlamak için.
* Tek varlık işlemleri ve çok varlık işlemleri karıştırmaktan kaçının (ör. `GetCountAsync`, `CreateEnumerableAsync`) aynı işlemde farklı yalıtım düzeyleri nedeniyle.
* InvalidOperationException işleyin. Çeşitli nedenlerle için sistemi tarafından kullanıcı işlemleri iptal. Örneğin, güvenilir durum yöneticisi rolünü birincil dışında değiştirirken veya uzun süre çalışan bir işlemin, işlem günlüğünün kesilmesi engelliyor. Böyle durumlarda kullanıcı zaten kendi işlem sonlandırıldı belirten InvalidOperationException alabilirsiniz. Varsayıldığında, sonlandırma işlemi kullanıcı tarafından bu özel durumu işlemek için en iyi yoludur işlem atmayı istenmemiştir iptal belirtecini sinyal (veya çoğaltma rolü değiştirildi) denetleyin ve yeni bir oluşturma işlem ve yeniden deneyin.  

Akılda tutulması gereken bazı noktalar şunlardır:

* Varsayılan zaman aşımı, güvenilir koleksiyon API'leri için dört saniyedir. Çoğu kullanıcı, varsayılan zaman aşımı kullanmanız gerekir.
* Varsayılan iptal belirteci `CancellationToken.None` tüm güvenilir koleksiyonlar API'lerindeki.
* Anahtar türü parametre (*TKey*) güvenilir bir sözlük doğru uygulamalıdır `GetHashCode()` ve `Equals()`. Anahtarları sabit olmalıdır.
* Güvenilir koleksiyonlar için yüksek kullanılabilirlik elde etmek için her hizmetin en az bir hedef ve en küçük çoğaltma kümesi boyutu 3 olması gerekir.
* İkincil okuma işlemleri, kaydedilen çekirdek olmayan sürümleri okuyabilirsiniz.
  Başka bir deyişle, tek bir ikincil bölgeden okuma veri sürümü yanlış progressed.
  Birincil okumalardan kararlı her zaman: hiçbir zaman yanlış progressed.
* Güvenlik/gizlilik verilerin kalıcı olarak kararı ve konu, Depolama Yönetimi tarafından sağlanan korumaları için güvenilir bir koleksiyondaki sayesinde uygulamanız olur YANİ İşletim sistemi disk şifrelemesi, bekleyen verilerinizi korumak için kullanılabilir.  

### <a name="next-steps"></a>Sonraki adımlar
* [Güvenilir Koleksiyonlar ile çalışma](service-fabric-work-with-reliable-collections.md)
* [İşlemler ve kilitler](service-fabric-reliable-services-reliable-collections-transactions-locks.md)
* Verileri yönetme
  * [Yedekleme ve Geri Yükleme](service-fabric-reliable-services-backup-restore.md)
  * [Bildirimler](service-fabric-reliable-services-notifications.md)
  * [Seri hale getirme ve yükseltme](service-fabric-application-upgrade-data-serialization.md)
  * [Güvenilir durum Yöneticisi'ni yapılandırma](service-fabric-reliable-services-configuration.md)
* Diğer
  * [Reliable Services hızlı başlangıç](service-fabric-reliable-services-quick-start.md)
  * [Güvenilir koleksiyonlar için Geliştirici Başvurusu](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
