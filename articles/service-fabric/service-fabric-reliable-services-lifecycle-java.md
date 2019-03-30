---
title: Azure Service Fabric Reliable Services yaşam döngüsü | Microsoft Docs
description: Service Fabric Reliable Services yaşam döngüsü olayları hakkında bilgi edinin.
services: service-fabric
documentationcenter: java
author: PavanKunapareddyMSFT
manager: chackdan
ms.assetid: ''
ms.service: service-fabric
ms.devlang: java
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/30/2017
ms.author: pakunapa
ms.openlocfilehash: 36c1ff2ace944d84120bf456060c7504170a814c
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58667421"
---
# <a name="reliable-services-lifecycle"></a>Reliable Services yaşam döngüsü
> [!div class="op_single_selector"]
> * [Windows üzerinde C#](service-fabric-reliable-services-lifecycle.md)
> * [Linux üzerinde Java](service-fabric-reliable-services-lifecycle-java.md)
>
>

Reliable Services, Azure Service Fabric programlama modelleri biridir. Reliable Services yaşam döngüsü hakkında öğrenme, temel yaşam döngüsü olaylarını anlamak en önemlidir. Olayları tam sıralama yapılandırma ayrıntıya bağlıdır. 

Genel olarak, aşağıdaki olaylar Reliable Services yaşam döngüsü içerir:

* Başlatma sırasında:
  * Hizmetleri oluşturulur.
  * Hizmetleri oluşturmak ve sıfır veya daha fazla dinleyicileri döndürmek için fırsatınız olur.
  * Hizmet ile iletişim için döndürülen tüm dinleyiciler açılmadı.
  * Hizmetin `runAsync` yöntemi çağrıldığında, hizmet, uzun süre çalışan yapın veya arka plan iş.
* Kapatma sırasında:
  * Geçildi iptal belirteci `runAsync` iptal edilir ve dinleyicileri kapatılır.
  * Hizmet nesnesi imha.

Reliable Services özelliğinde olayların sırası, güvenilir hizmet durum bilgisi olan veya bağlı olarak biraz değişebilir. 

Ayrıca, durum bilgisi olan hizmetler için birincil takas senaryo incelemeniz gerekir. Bu dizisi sırasında birincil rolü başka bir çoğaltmaya aktarılır (veya geri gelir) olmadan hizmet kapatılıyor. 

Son olarak, hata veya hata koşulları hakkında düşünmek zorunda.

## <a name="stateless-service-startup"></a>Durum bilgisi olmayan hizmet başlatma
Durum bilgisi olmayan hizmet yaşam döngüsü oldukça basittir. Olayların sırası şöyledir:

1. Hizmeti oluşturulur.
2. Bu olaylar, paralel olarak gerçekleşir:
    - `StatelessService.createServiceInstanceListeners()` çağrılır, ve tüm dinleyicileri açıldığında döndürdü. `CommunicationListener.openAsync()` Her dinleyici adı verilir.
    - Hizmetin `runAsync` yöntemi (`StatelessService.runAsync()`) olarak adlandırılır.
3. Varsa, hizmetin kendi `onOpenAsync` yöntemi çağrılır. Özellikle, `StatelessService.onOpenAsync()` çağrılır. Genel olmayan bir geçersiz kılma budur ancak kullanılabilir.

Dinleyici oluşturup çağrı ve çağrısı arasında hiçbir sıralama olduğuna dikkat edin önemlidir `runAsync`. Dinleyiciler önce açabilir `runAsync` başlatılır. Benzer şekilde, `runAsync` önce iletişim dinleyicileri açık veya hatta yapılandırıldıktan önce çağrılır. Herhangi bir eşitleme gerekiyorsa, uygulayan tarafından yapılması gerekir. Bazı yaygın çözümleri şunlardır:

* Bazen dinleyicileri diğer bilgileri oluşturulur veya başka bir çalışma yapılmadan kadar çalışamaz. İş genellikle hizmetin oluşturucuda yapılabilir, durum bilgisi olmayan hizmetler için. Sırasında yapılabilir `createServiceInstanceListeners()` çağrısı, ya da dinleyici oluşumu bir parçası olarak.
* Bazen kodu `runAsync` kadar dinleyicileri başlatılamıyor. Bu durumda, ek koordinasyon gereklidir. Dinleyiciler bayrak ekleme genel bir çözümdür. Dinleyiciler bittiğinde bayrak belirtir. `runAsync` Yöntemi denetler bu asıl işi devam etmeden önce.

## <a name="stateless-service-shutdown"></a>Durum bilgisi olmayan hizmet kapanması
Bir durum bilgisi olmayan hizmeti kapatılıyor, aynı deseni takip edilen, ancak geriye doğru:

1. Bu olaylar, paralel olarak gerçekleşir:
    - Tüm açık dinleyiciler kapatılır. `CommunicationListener.closeAsync()` Her dinleyici adı verilir.
    - Geçildi iptal belirteci `runAsync()` iptal edilir. İptal belirtecinin denetimi `isCancelled` özelliği döndürür `true`ve çağrılırsa, belirtecin `throwIfCancellationRequested` yöntem bir `CancellationException`.
2. Zaman `closeAsync()` her dinleyici tamamlandıktan ve `runAsync()` de tamamlanır, hizmetin `StatelessService.onCloseAsync()` yöntemi çağrıldığında, varsa. Yeniden, bu yaygın bir geçersiz kılma değil, ancak güvenli bir şekilde kaynakları kapatın, arka plan işlemesi durdurmak, dış durumu kaydediyor son veya varolan bağlantıları kapatmak için kullanılabilir.
3. Sonra `StatelessService.onCloseAsync()` tamamlandığında, hizmet nesnesi imha.

## <a name="stateful-service-startup"></a>Durum bilgisi olan hizmet başlatma
Durum bilgisi olan hizmetler birkaç değişiklik ile durum bilgisi olmayan hizmetler için benzer bir düzeni vardır.  Durum bilgisi olan hizmet başlatma olayların sırası şöyledir:

1. Hizmeti oluşturulur.
2. `StatefulServiceBase.onOpenAsync()` çağrılır. Bu çağrı hizmette yaygın olarak geçersiz kılınmış değil.
3. Bu olaylar, paralel olarak gerçekleşir:
    - `StatefulServiceBase.createServiceReplicaListeners()` çağrılır. 
      - Hizmet, birincil hizmet ise, döndürülen tüm dinleyicileri açılır. `CommunicationListener.openAsync()` Her dinleyici adı verilir.
      - İkincil bir hizmet hizmeti ise yalnızca dinleyicileri olarak işaretlenmiş `listenOnSecondary = true` açılmadı. İkincil veritabanı üzerinde açık olan dinleyicileri olması daha az yaygındır.
    - Hizmet şu anda birincil, hizmete ait olup olmadığını `StatefulServiceBase.runAsync()` yöntemi çağrılır.
4. Tüm çoğaltma dinleyicinin sonra `openAsync()` bitiş çağırır ve `runAsync()` çağrıldığında `StatefulServiceBase.onChangeRoleAsync()` çağrılır. Bu çağrı hizmette yaygın olarak geçersiz kılınmış değil.

Durum bilgisi olan hizmet, durum bilgisi olmayan hizmetler var. benzer hiçbir sırası dinleyiciler, oluşturulan ve açılan arasında koordinasyon gereksinimini olabildiğince ve ne zaman `runAsync` çağrılır. Koordinasyon gerekiyorsa, çözümleri çok aynıdır. Ancak, durum bilgisi olan hizmet için bir ek durum vardır. Söyleyin iletişim dinleyicileri gelen çağrıları bazı içinde tutulan bilgileri gerektirir [güvenilir koleksiyonlar](service-fabric-reliable-services-reliable-collections.md). Reliable Collections okunabilir veya yazılabilir önce iletişim dinleyicileri açabilir çünkü ve önce `runAsync` başlatır, bazı ek koordinasyon gereklidir. Basit ve en yaygın hata kodunu döndürmek iletişim dinleyicileri için çözümüdür. İstemci isteği yeniden denemek için bilmeniz gereken hata kodu kullanır.

## <a name="stateful-service-shutdown"></a>Durum bilgisi olan hizmet kapanması
Durum bilgisi olmayan hizmetler gibi yaşam döngüsü olayları sırasında kapatma başlatma sırasında aynıdır, ancak tersine. Durum bilgisi olan hizmet kapatılıyor, aşağıdaki olaylar gerçekleşir:

1. Bu olaylar, paralel olarak gerçekleşir:
    - Tüm açık dinleyiciler kapatılır. `CommunicationListener.closeAsync()` Her dinleyici adı verilir.
    - Geçildi iptal belirteci `runAsync()` iptal edilir. Bir iptal belirtecinin çağrı `isCancelled()` yöntemi döndürür `true`ve çağrılırsa, belirtecin `throwIfCancellationRequested()` yöntem bir `OperationCanceledException`.
2. Sonra `closeAsync()` her dinleyici tamamlandıktan ve `runAsync()` de tamamlanır, hizmetin `StatefulServiceBase.onChangeRoleAsync()` çağrılır. Bu çağrı hizmette yaygın olarak geçersiz kılınmış değil.

   > [!NOTE]  
   > Bekleyen `runAsync` tamamlanması yalnızca bu çoğaltma birincil çoğaltmaya ise gereklidir.

3. Sonra `StatefulServiceBase.onChangeRoleAsync()` yöntemi tamamlandığında, `StatefulServiceBase.onCloseAsync()` yöntemi çağrılır. Bu çağrı, seyrek bir geçersiz kılma olmakla birlikte kullanılabilir.
3. Sonra `StatefulServiceBase.onCloseAsync()` tamamlandığında, hizmet nesnesi imha.

## <a name="stateful-service-primary-swaps"></a>Durum bilgisi olan hizmet birincil değiştirir
Durum bilgisi olan hizmet çalışırken, iletişim dinleyicileri açıldığından ve `runAsync` için durum bilgisi olan hizmet, yalnızca birincil çoğaltmalara yöntemi çağrılır. İkincil çoğaltma oluşturulur, ancak daha fazla çağrı görürsünüz. Durum bilgisi olan hizmet çalışırken, şu anda çoğaltma birincil değiştirebilirsiniz. Durum bilgisi olan bir çoğaltma görebilirsiniz yaşam döngüsü olaylarını çoğaltma indirgenir veya değiştirme sırasında yükseltilen olmasına göre değişir.

### <a name="for-the-demoted-primary"></a>İndirgenen birincil için
Service Fabric iletileri işlemeyi durdur ve herhangi bir arka plan iş durdurmak için yetkisi birincil çoğaltma gerekir. Bu adım hizmet zaman kapatıldığında benzer. İkincil ettiğinden bir hizmet imha değil veya kapalı, farktır. Aşağıdaki olaylar gerçekleşir:

1. Bu olaylar, paralel olarak gerçekleşir:
    - Tüm açık dinleyiciler kapatılır. `CommunicationListener.closeAsync()` Her dinleyici adı verilir.
    - Geçildi iptal belirteci `runAsync()` iptal edilir. Bir iptal belirtecinin denetimini `isCancelled()` yöntemi döndürür `true`. Çağrılırsa, belirtecin `throwIfCancellationRequested()` yöntem bir `OperationCanceledException`.
2. Sonra `closeAsync()` her dinleyici tamamlandıktan ve `runAsync()` de tamamlanır, hizmetin `StatefulServiceBase.onChangeRoleAsync()` çağrılır. Bu çağrı hizmette yaygın olarak geçersiz kılınmış değil.

### <a name="for-the-promoted-secondary"></a>Yükseltilen ikincil için
Benzer şekilde, Service Fabric kablo iletileri dinlemeye başla ve tamamlanması gereken herhangi bir arka plan görevi başlatmak için yükseltilen ikincil bir çoğaltmaya gerekir. Bu işlem, hizmet oluşturulduğunda benzerdir. Çoğaltma zaten farktır. Aşağıdaki olaylar gerçekleşir:

1. Bu olaylar, paralel olarak gerçekleşir:
    - `StatefulServiceBase.createServiceReplicaListeners()` çağrılan ve tüm dinleyicileri açıldığında döndürdü. `CommunicationListener.openAsync()` Her dinleyici adı verilir.
    - Hizmetin `StatefulServiceBase.runAsync()` yöntemi çağrılır.
2. Tüm çoğaltma dinleyicinin sonra `openAsync()` bitiş çağırır ve `runAsync()` çağrıldığında `StatefulServiceBase.onChangeRoleAsync()` çağrılır. Bu çağrı hizmette yaygın olarak geçersiz kılınmış değil.

### <a name="common-issues-during-stateful-service-shutdown-and-primary-demotion"></a>Durum bilgisi olan hizmet kapanması ve birincil indirgeme sırasında sık karşılaşılan sorunlar
Service Fabric durum bilgisi olan bir hizmeti için birden çok nedenden dolayı birincil değiştirir. En yaygın nedenleri şunlardır [küme yeniden Dengeleme](service-fabric-cluster-resource-manager-balancing.md) ve [uygulama yükseltmesi](service-fabric-application-upgrade.md). Bu işlemler sırasında hizmet uyar, önemli `cancellationToken`. Bu normal hizmeti kapatma sırasında da geçerlidir IF gibi hizmeti silindi.

İptal düzgün bir şekilde işlemez Hizmetleri, çeşitli sorunlar oluşabilir. Bu işlem, Service Fabric Hizmetleri düzgün bitirmesini beklediğinden yavaş yükleniyor. Bu sonuç olarak başarısız yükseltme, zaman aşımına uğruyor ve geri alma neden olabilir. İptal belirtecini kabul etmenin hatası imbalanced kümeleri da neden olabilir. Düğümleri sık aldığından kümeleri dengesiz hale gelir. Ancak, başka bir yere taşınması çok uzun sürdüğü için Hizmetleri dengelenir olamaz. 

Durum bilgisi olan hizmetler olduğundan, ayrıca hizmetleri kullanmak büyük olasılıkla [güvenilir koleksiyonlar](service-fabric-reliable-services-reliable-collections.md). Birincil indirgenir, Service Fabric'te gerçekleşen ilk şey temel alınan durum yazma erişimi iptal edilmediğini biridir. Bu hizmet yaşam döngüsü etkileyebilecek sorunları ikinci bir set yol açar. Dönüş özel durumları, zamanlama ve yineleme olup taşınırken göre koleksiyonlar veya kapat. Bu özel durumların doğru bir şekilde işlemek önemlidir. 

Service Fabric tarafından oluşturulan özel durumları olan ya da kalıcı [(`FabricException`)](https://docs.microsoft.com/java/api/system.fabric.exception) veya geçici [(`FabricTransientException`)](https://docs.microsoft.com/java/api/system.fabric.exception.fabrictransientexception). Kalıcı özel durumlar günlüğe kaydedilir ve durum. Geçici özel durumlar, yeniden deneme mantığı temelinde denenebilir.

Sınama ve doğrulama Reliable Services önemli bir bölümü, gelen kullanarak özel durumları işleme `ReliableCollections` hizmet yaşam döngüsü olayları ile birlikte okunmalıdır. Hizmetinizi yük altında her zaman çalıştırmanızı öneririz. Yükseltmeler de yapmalısınız ve [chaos test](service-fabric-controlled-chaos.md) üretim ortamına dağıtmadan önce. Hizmetinizi düzgün şekilde uygulandığını ve bunu doğru şekilde yaşam döngüsü olaylarını işler temel adımları sağlamak.

## <a name="notes-on-service-lifecycle"></a>Hizmet yaşam döngüsü ile ilgili notlar
* Her iki `runAsync()` yöntemi ve `createServiceInstanceListeners/createServiceReplicaListeners` çağrılarıdır isteğe bağlı. Bir hizmet bir, iki veya hiçbiri olabilir. Örneğin, hizmet tüm yanıt kullanıcı çağrı yapar, gerek yoktur, uygulamak için `runAsync()`. İletişim dinleyicileri ve ilişkili kodunu gereklidir.  Benzer şekilde, oluşturma ve iletişim dinleyicileri döndürerek isteğe bağlıdır. Hizmet, yalnızca uygulamak gereken şekilde yapmak için yalnızca arka plan iş olabilir `runAsync()`.
* Bir hizmet için geçerli olduğundan `runAsync()` başarıyla ve ondan geri dönüş. Bu hata durumu olarak kabul değil. Bu hizmet sonlandırma arka plan işi temsil eder. Durum bilgisi olan Reliable Services için `runAsync()` hizmet, birincil sunucudan yetkisi varsa yeniden adlandırılan ve ardından birincil siteye yükseltildi.
* Hizmet gelen çıkılıyorsa `runAsync()` bazı beklenmeyen özel durum olarak, bu hata. Hizmet nesnesi kapatılır ve sistem durumu hata bildirilir.
* Yazma olanağı hemen olmasa da zaman sınırı bu yöntemleri döndüren üzerinde kaybedersiniz. Bu nedenle, herhangi bir gerçek çalışma tamamlanamıyor. İptal isteği alır almaz mümkün olduğunca çabuk dönüş öneririz. Hizmetinizi şu API çağrıları için makul bir süre içinde yanıt vermezse, Service Fabric hizmetinizi zorla sonlandırabilir. Bu genellikle, yalnızca uygulama yükseltmeleri veya hizmet ne zaman siliniyor sırasında gerçekleşir. Bu zaman aşımı, varsayılan değer 15 dakikadır.
* Hataları `onCloseAsync()` yolu sonucunda `onAbort()` çağrılan. Bu çağrı, hizmeti temizlemek ve talep tüm kaynakları serbest bırakmak son şans, mümkün olan en iyi bir fırsattır. Bu genellikle düğümde kalıcı bir hata algılandığında veya Service Fabric güvenilir hizmeti örneğinin yaşam döngüsü iç hatalar nedeniyle yönetememesi durumunda olarak adlandırılır.
* `OnChangeRoleAsync()` durum bilgisi olan hizmet çoğaltma rolü, örneğin birincil veya ikincil değiştirirken çağırılır. Birincil çoğaltmalara yazma durumu verilmiştir (oluşturun ve güvenilir koleksiyonlar için yazma izni verilir). İkincil çoğaltmalar, Okuma durumu (yalnızca var olan güvenilir koleksiyonlardan okuyabilir) atanır. Çoğu iş durum bilgisi olan hizmet, birincil çoğaltma üzerinde gerçekleştirilir. İkincil çoğaltmaların salt okunur doğrulama, rapor oluşturma, veri araştırma veya diğer salt okunur işleri gerçekleştirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
* [Reliable Services giriş](service-fabric-reliable-services-introduction.md)
* [Reliable Services hızlı başlangıç](service-fabric-reliable-services-quick-start-java.md)

