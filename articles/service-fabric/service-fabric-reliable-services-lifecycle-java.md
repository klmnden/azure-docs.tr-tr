---
title: Azure Service Fabric Reliable Services yaşam döngüsü | Microsoft Docs
description: Service Fabric Reliable Services yaşam döngüsü olayları hakkında bilgi edinin.
services: service-fabric
documentationcenter: java
author: PavanKunapareddyMSFT
manager: timlt
ms.assetid: ''
ms.service: service-fabric
ms.devlang: java
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/30/2017
ms.author: pakunapa
ms.openlocfilehash: cc89d174a201b38d79c7993d548c8eac4a47fbcb
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="reliable-services-lifecycle"></a>Reliable Services yaşam döngüsü
> [!div class="op_single_selector"]
> * [Windows üzerinde C#](service-fabric-reliable-services-lifecycle.md)
> * [Linux üzerinde Java](service-fabric-reliable-services-lifecycle-java.md)
>
>

Güvenilir hizmetler biridir programlama modelleri Azure Service Fabric içinde kullanılabilir. Reliable Services yaşam döngüsü hakkında öğrenme, en temel yaşam döngüsü olayları anlamak önemlidir. Olayların tam sıralama yapılandırma ayrıntıya bağlıdır. 

Genel olarak, aşağıdaki olaylar Reliable Services yaşam döngüsü içerir:

* Başlatma sırasında:
  * Hizmetleri oluşturulur.
  * Hizmetleri oluşturmak ve sıfır veya daha fazla dinleyicileri dönmek için fırsatınız olur.
  * Tüm döndürülen dinleyiciler, hizmet ile iletişim için açılır.
  * Hizmetin `runAsync` yöntemi çağrılır, böylece hizmeti uzun süre çalışan yapın veya arka plan iş.
* Kapatma sırasında:
  * Geçirilmedi iptal belirteci `runAsync` iptal edilir ve dinleyicileri kapalı.
  * Hizmet nesnesi destructed.

Güvenilir hizmetler olay sırasını güvenilir hizmet durum bilgisiz veya durum bilgisi olan bağlı olarak biraz değişebilir. 

Ayrıca, durum bilgisi olan hizmetler için birincil takas senaryo incelemeniz gerekir. Bu dizisi sırasında birincil rolü başka bir çoğaltmaya aktarılır (veya geri gelir) olmadan hizmet kapatılıyor. 

Son olarak, hata ve hata koşulları hakkında düşünmeniz gerekir.

## <a name="stateless-service-startup"></a>Durum bilgisiz hizmetini başlatma
Durum bilgisiz hizmet yaşam döngüsü oldukça basittir. Olayların sırası şöyledir:

1. Hizmet oluşturulur.
2. Bu olaylar, paralel olarak oluşur:
    - `StatelessService.createServiceInstanceListeners()` çalıştırılan ve herhangi bir dinleyicileri açıldığından döndürdü. `CommunicationListener.openAsync()` Her dinleyici adı verilir.
    - Hizmetin `runAsync` yöntemi (`StatelessService.runAsync()`) olarak adlandırılır.
3. Varsa, hizmetin kendi `onOpenAsync` yöntemi çağrılır. Özellikle, `StatelessService.onOpenAsync()` olarak adlandırılır. Seyrek bir geçersiz kılma budur ancak kullanılabilir değildir.

Hiçbir oluşturmak ve dinleyicileri açmak için arama ve çağrısı arasında sıralama olduğunu dikkate almak önemlidir `runAsync`. Dinleyicileri önce açabilir `runAsync` başlatılır. Benzer şekilde, `runAsync` iletişim dinleyicileri açık önce ya da hatta yapılandırıldıktan önce çağrılan. Herhangi bir eşitleme gerekiyorsa, uygulayan tarafından yapılması gerekir. Bazı yaygın çözümleri şunlardır:

* Bazen dinleyicileri diğer bilgileri oluşturulur veya diğer iş yapılır kadar çalışamaz. İş genellikle hizmetin oluşturucuda yapılabilecek durum bilgisi olmayan hizmetler için. Sırasında yapılabilir `createServiceInstanceListeners()` çağrısı, veya dinleyici yapımı parçası olarak.
* Bazen kodda `runAsync` dinleyicileri açık kadar başlatılmaz. Bu durumda, ek koordinasyon gereklidir. Bayrak dinleyicileri ekleme ortak bir çözümdür. Bayrağı dinleyicileri bittiği gösterir. `runAsync` Yöntemi denetler bu asıl işi devam etmeden önce.

## <a name="stateless-service-shutdown"></a>Durum bilgisiz hizmet kapanması
Durum bilgisi olmayan bir hizmeti kapatılıyor aynı düzeni izlenen, ancak geriye doğru olur:

1. Bu olaylar, paralel olarak oluşur:
    - Tüm açık dinleyiciler kapatılır. `CommunicationListener.closeAsync()` Her dinleyici adı verilir.
    - Geçirilmedi iptal belirteci `runAsync()` iptal edilir. İptal belirtecin denetimi `isCancelled` özelliği döndürür `true`ve çağırdıysanız, belirtecin `throwIfCancellationRequested` yöntemi atar bir `CancellationException`.
2. Zaman `closeAsync()` her Dinleyicide tamamlandıktan ve `runAsync()` bittikten ayrıca hizmetin `StatelessService.onCloseAsync()` yöntemi çağrıldığında, varsa. Yeniden, bu ortak bir geçersiz kılma değil, ancak güvenle kaynakları kapatmak, arka plan işleme durdurmak, dış durumu kaydetmeyi bitirmek veya var olan bağlantıların kapatmak için kullanılabilir.
3. Sonra `StatelessService.onCloseAsync()` bitirdiğinde, hizmet nesnesi destructed.

## <a name="stateful-service-startup"></a>Durum bilgisi olan hizmet başlatma
Durum bilgisi olan hizmetler birkaç değişikliklerle durum bilgisi olmayan hizmetler için benzer bir desen sahiptir.  Durum bilgisi olan bir hizmeti başlatmak için olayların sırası şöyledir:

1. Hizmet oluşturulur.
2. `StatefulServiceBase.onOpenAsync()` adı verilir. Bu çağrı hizmetinde yaygın olarak geçersiz kılındı değil.
3. Bu olaylar, paralel olarak oluşur:
    - `StatefulServiceBase.createServiceReplicaListeners()` çağrılır. 
      - Hizmet birincil service ise, tüm döndürülen dinleyicileri açılır. `CommunicationListener.openAsync()` Her dinleyici adı verilir.
      - Hizmet bir ikincil service ise, yalnızca dinleyicileri olarak işaretlenmiş `listenOnSecondary = true` açılır. İkincil üzerinde açık dinleyicileri sahip daha az yaygın bir durumdur.
    - Hizmet şu anda bir birincil, hizmet ait olup olmadığını `StatefulServiceBase.runAsync()` yöntemi çağrılır.
4. Tüm çoğaltma dinleyicisi ait sonra `openAsync()` bitiş çağırır ve `runAsync()` olarak adlandırılır, `StatefulServiceBase.onChangeRoleAsync()` olarak adlandırılır. Bu çağrı hizmetinde yaygın olarak geçersiz kılındı değil.

Durum bilgisi olan hizmetindeki durum bilgisi olmayan hizmetler var. benzer dinleyicileri, oluşturulan ve açılan sırasını arasında hiçbir düzenlemesi ve ne zaman `runAsync` olarak adlandırılır. Düzenleme gerekiyorsa, çözümler çok aynıdır. Ancak, durum bilgisi olan hizmet için bir ek söz konusu değildir. İletişim dinleyicileri gelen çağrıları bazı içinde tutulur bilgi gerektiren söyleyin [güvenilir koleksiyonları](service-fabric-reliable-services-reliable-collections.md). Güvenilir koleksiyonları okunabilir veya yazılabilir önce iletişim dinleyicileri açabilir olduğundan ve önce `runAsync` başlatır, bazı ek koordinasyon gereklidir. En basit ve en yaygın bir hata kodu döndürmek iletişim dinleyicileri için çözümüdür. İstemci, isteği yeniden denemek için bilmeniz hata kodunu kullanır.

## <a name="stateful-service-shutdown"></a>Durum bilgisi olan hizmet kapatma
Durum bilgisi olmayan hizmetler gibi kapatma sırasında yaşam döngüsü olayları başlatma sırasında aynıdır, ancak tersine. Durum bilgisi olan hizmet kapatılıyor, aşağıdaki olaylar gerçekleşir:

1. Bu olaylar, paralel olarak oluşur:
    - Tüm açık dinleyiciler kapatılır. `CommunicationListener.closeAsync()` Her dinleyici adı verilir.
    - Geçirilmedi iptal belirteci `runAsync()` iptal edildi. İptal belirtecin çağrısı `isCancelled()` yöntemi döndürür `true`ve çağırdıysanız, belirtecin `throwIfCancellationRequested()` yöntemi atar bir `OperationCanceledException`.
2. Sonra `closeAsync()` her Dinleyicide tamamlandıktan ve `runAsync()` bittikten ayrıca hizmetin `StatefulServiceBase.onChangeRoleAsync()` olarak adlandırılır. Bu çağrı hizmetinde yaygın olarak geçersiz kılındı değil.

   > [!NOTE]  
   > Bekleyen `runAsync` tamamlamak için yalnızca bu çoğaltma birincil kopya ise gereklidir.

3. Sonra `StatefulServiceBase.onChangeRoleAsync()` yöntemi sonlandığında `StatefulServiceBase.onCloseAsync()` yöntemi çağrılır. Bu çağrı, seyrek bir geçersiz kılma olmakla birlikte kullanılabilir.
3. Sonra `StatefulServiceBase.onCloseAsync()` bitirdiğinde, hizmet nesnesi destructed.

## <a name="stateful-service-primary-swaps"></a>Durum bilgisi olan hizmet birincil değiştirir
Durum bilgisi olan hizmet çalışırken, iletişim dinleyicileri açıldığından ve `runAsync` yöntemi yalnızca durum bilgisi olan hizmet birincil çoğaltmaları çağrılır. İkincil çoğaltmaları oluşturulur, ancak hiçbir sonraki çağrılar görürsünüz. Durum bilgisi olan hizmet çalışırken, şu anda çoğaltma birincil değiştirebilirsiniz. Durum bilgisi olan bir çoğaltma, çoğaltma veya indirgenir sırasında takas yükseltilmiş olmasına bağlıdır görebilirsiniz yaşam döngüsü olayları.

### <a name="for-the-demoted-primary"></a>İçin indirgenen birincil
Service Fabric iletilerini işleme durdurmak ve herhangi bir arka plan iş durdurmak için indirgenir birincil çoğaltmanın ihtiyacı vardır. Bu adım hizmetin ne zaman kapatılır benzer. Bir ikincil olarak kaldığından bir hizmet destructed değil veya kapalı, farktır. Aşağıdaki olaylar gerçekleşir:

1. Bu olaylar, paralel olarak oluşur:
    - Tüm açık dinleyiciler kapatılır. `CommunicationListener.closeAsync()` Her dinleyici adı verilir.
    - Geçirilmedi iptal belirteci `runAsync()` iptal edilir. İptal belirtecin kontrol `isCancelled()` yöntemi döndürür `true`. Çağırdıysanız, belirtecin `throwIfCancellationRequested()` yöntemi atar bir `OperationCanceledException`.
2. Sonra `closeAsync()` her Dinleyicide tamamlandıktan ve `runAsync()` bittikten ayrıca hizmetin `StatefulServiceBase.onChangeRoleAsync()` olarak adlandırılır. Bu çağrı hizmetinde yaygın olarak geçersiz kılındı değil.

### <a name="for-the-promoted-secondary"></a>Yükseltilen ikincil için
Benzer şekilde, Service Fabric hattaki iletiler için dinleme başlatmak ve tamamlamak için gereken herhangi bir arka plan görevi başlatmak için yükseltilen ikincil çoğaltma gerekir. Bu işlem, hizmet oluşturulduğunda benzer. Çoğaltma zaten farktır. Aşağıdaki olaylar gerçekleşir:

1. Bu olaylar, paralel olarak oluşur:
    - `StatefulServiceBase.createServiceReplicaListeners()` çağrılır ve herhangi bir dinleyicileri açıldığından döndürdü. `CommunicationListener.openAsync()` Her dinleyici adı verilir.
    - Hizmetin `StatefulServiceBase.runAsync()` yöntemi çağrılır.
2. Tüm çoğaltma dinleyicisi ait sonra `openAsync()` bitiş çağırır ve `runAsync()` olarak adlandırılır, `StatefulServiceBase.onChangeRoleAsync()` olarak adlandırılır. Bu çağrı hizmetinde yaygın olarak geçersiz kılındı değil.

### <a name="common-issues-during-stateful-service-shutdown-and-primary-demotion"></a>Durum bilgisi olan hizmet kapanması ve birincil indirgeme sırasında ortak sorunlar
Service Fabric durum bilgisi olan hizmeti için birden çok nedenden dolayı birincil değiştirir. En yaygın nedenleri [küme dengelenmesi](service-fabric-cluster-resource-manager-balancing.md) ve [uygulama yükseltme](service-fabric-application-upgrade.md). Bu işlemler sırasında hizmet uyar önemli `cancellationToken`. Bu aynı zamanda normal hizmet kapatılırken geçerlidir gelmesi gibi hizmet silindi.

İptal düzgün bir şekilde işlemek olmayan hizmetleri, çeşitli sorunlar yaşayabilirsiniz. Bu işlemler yavaş olduğu hizmetlerin düzgün biçimde durması için Service Fabric bekler. Bu sonuçta başarısız yükseltme o zaman aşımı ve geri alma neden olabilir. İptal belirteci vermenizin hatası imbalanced kümeleri de neden olabilir. Düğümleri etkin aldığından kümeleri dengesiz haline gelir. Başka bir yerde taşınması çok uzun sürdüğü için ancak, hizmetler yeniden dengelenir olamaz. 

Hizmetleri durum bilgisi olduğundan, ayrıca hizmetlerinin kullandığını büyük olasılıkla [güvenilir koleksiyonları](service-fabric-reliable-services-reliable-collections.md). Birincil indirgenir, Service Fabric içinde gerçekleşen ilk şey alttaki durumu yazma erişimi iptal edilmediğini biridir. Bu hizmet yaşam döngüsü etkileyebilecek sorunları ikinci bir dizi sağlar. Zamanlama ve yineleme olup taşındığı dönüş özel durumlar göre koleksiyonları veya kapat. Bu özel durumlar doğru şekilde işlemek önemlidir. 

Service Fabric tarafından oluşturulan özel durumları olan ya da kalıcı [(`FabricException`)](https://docs.microsoft.com/java/api/system.fabric.exception) veya geçici [(`FabricTransientException`)](https://docs.microsoft.com/java/api/system.fabric.exception._fabric_transient_exception). Kalıcı özel durumları günlüğe ve durum. Geçici özel durumlar yeniden deneme mantığına göre yeniden denenebilir.

Test etme ve güvenilir hizmetler doğrulama önemli bir bölümü kullanarak gelen özel durum işleme `ReliableCollections` hizmet yaşam döngüsü olayları ile birlikte. Hizmetinizi yük altında her zaman çalıştırmanızı öneririz. Yükseltmeler gerçekleştirmesi gereken ve [chaos sınama](service-fabric-controlled-chaos.md) üretime dağıtmadan önce. Bu temel adımları hizmetinizin doğru uygulanır ve bunu doğru şekilde yaşam döngüsü olayları işleyen sağlamaya yardımcı olur.

## <a name="notes-on-service-lifecycle"></a>Hizmet yaşam döngüsü ile ilgili notlar
* Her iki `runAsync()` yöntemi ve `createServiceInstanceListeners/createServiceReplicaListeners` çağrıları isteğe bağlıdır. Bir hizmet, hem veya hiçbiri olabilir. Örneğin, hizmet yanıt kullanıcı çağrıları olarak tüm çalışmasını varsa, gerek yoktur, uygulamak için `runAsync()`. İletişim dinleyicileri ve bunların ilişkili kodu gereklidir.  Benzer şekilde, oluşturma ve iletişim dinleyicileri döndürme isteğe bağlıdır. Yalnızca uygulamak gereken şekilde hizmet yapmak için yalnızca arka plan çalışması olabilir `runAsync()`.
* Tamamlamak bir hizmet için geçerli `runAsync()` başarıyla ve dönüş almaktır. Bu bir hata durumu olarak kabul değil. Hizmet sonlandırma arka plan işi temsil eder. Durum bilgisi olan güvenilir hizmetler için `runAsync()` hizmet birincil sunucudan yetkisi varsa yeniden çağrılır ve geri birincil yükseltilmiş.
* Gelen bir hizmet bulunup bulunmadığını `runAsync()` bazı beklenmeyen bir özel durum atma tarafından bu bir hatadır. Hizmet nesnesi kapatılır ve bir sistem durumu hatası bildirilir.
* Hiçbir zaman bir sınır olmasa bu yöntemlerden döndürme, hemen yazabilen kaybedersiniz. Bu nedenle, herhangi bir gerçek iş tamamlayamıyor. İptal isteği aldığında gerçekleştireceği mümkün olduğunca hızlı bir şekilde olarak dönüş öneririz. Hizmetiniz bu API çağrıları için makul bir sürede yanıt vermezse, Service Fabric zorla hizmetinizi sonlandırabilir. Genellikle, yalnızca uygulama yükseltme veya ne zaman bir hizmet silindiğinden sırasında meydana gelir. Bu zaman aşımı, varsayılan değer 15 dakikadır.
* Başarısızlık `onCloseAsync()` yolu sonucunda `onAbort()` çağrılıyor. Bu çağrı, temizleme ve talep tüm kaynakları serbest bırakmak hizmet için son fırsat, en yüksek çaba bir fırsattır. Bu genellikle düğümde kalıcı bir hata algılandığında veya Service Fabric güvenilir bir şekilde iç hataları nedeniyle hizmet örneğinin yaşam döngüsü yönetemediğinde olarak adlandırılır.
* `OnChangeRoleAsync()` durum bilgisi olan hizmet çoğaltma rolü, örneğin birincil veya ikincil değiştirilirken adı verilir. Birincil çoğaltmalara yazma durumu verilmiştir (oluşturun ve güvenilir koleksiyonlarına yazma izni verilir). İkincil çoğaltmaları (yalnızca var olan güvenilir koleksiyonlarından okuyabilir) okuma durumu atanır. Durum bilgisi olan hizmet çoğu çalışmasında birincil kopyada gerçekleştirilir. İkincil çoğaltmaların salt okunur doğrulama, rapor oluşturma, veri araştırma veya diğer salt okunur işleri gerçekleştirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
* [Güvenilir hizmetlerine giriş](service-fabric-reliable-services-introduction.md)
* [Güvenilir hizmetler hızlı başlangıç](service-fabric-reliable-services-quick-start-java.md)

