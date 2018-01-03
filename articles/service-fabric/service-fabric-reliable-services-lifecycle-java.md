---
title: "Azure Service Fabric güvenilir hizmetler yaşam döngüsüne genel bakış | Microsoft Docs"
description: "Service Fabric güvenilir hizmetler farklı yaşam döngüsü olayları hakkında bilgi edinin"
services: Service-Fabric
documentationcenter: java
author: PavanKunapareddyMSFT
manager: timlt
ms.assetid: 
ms.service: Service-Fabric
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/30/2017
ms.author: pakunapa;
ms.openlocfilehash: 93fd003ff5ba7673e5a719fb1ced0cbb97034610
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2017
---
# <a name="reliable-services-lifecycle-overview"></a>Güvenilir hizmetler yaşam döngüsüne genel bakış
> [!div class="op_single_selector"]
> * [Windows üzerinde C#](service-fabric-reliable-services-lifecycle.md)
> * [Linux üzerinde Java](service-fabric-reliable-services-lifecycle-java.md)
>
>

Güvenilir hizmetler yaşam döngüleri hakkında düşünürken, yaşam döngüsü temelleri en önemli olan. Genel olarak:

* Başlatma sırasında
  * Hizmetleri oluşturulur
  * Oluşturun ve sıfır veya daha fazla dinleyicileri dönüş fırsatına sahip
  * Hizmeti ile iletişimi sağlayan tüm döndürülen dinleyiciler açıldı
  * Hizmetin runAsync yöntemi çağrılır, uzun çalışan yapın veya iş arka plan hizmetinin izin verme
* Kapatma sırasında
  * RunAsync için geçirilen iptal belirteci iptal edilir ve dinleyicileri kapalı
  * Bu işlem tamamlandıktan sonra destructed hizmeti nesnesi

Bu olaylar tam sıralama geçici ayrıntıları vardır. Özellikle, olayların sırası biraz güvenilir hizmet Stateless veya durum bilgisi olan olmasına bağlı olarak değişebilir. Ayrıca, durum bilgisi olan hizmetler için birincil takas senaryoyla dağıtılacak sahibiz. Bu dizisi sırasında birincil rolü başka bir çoğaltmaya aktarılır (veya geri gelir) olmadan hizmet kapatılıyor. Son olarak, biz hata veya hata koşulları hakkında düşünmeniz gerekir.

## <a name="stateless-service-startup"></a>Durum bilgisiz hizmetini başlatma
Durum bilgisiz hizmet yaşam döngüsü oldukça basittir. Olayların sırası şöyledir:

1. Hizmet oluşturulur
2. Ardından paralel iki şey olur:
    - `StatelessService.createServiceInstanceListeners()`çağrılan tüm döndürülen dinleyiciler açıldığı (`CommunicationListener.openAsync()` her dinleyici adı verilir)
    - Hizmetin runAsync yöntemi (`StatelessService.runAsync()`) olarak adlandırılır
3. Varsa, hizmetin kendi onOpenAsync yöntemi çağrılır (özellikle, `StatelessService.onOpenAsync()` olarak adlandırılır. Seyrek bir geçersiz kılma budur ancak kullanılabilir).

Hiçbir oluşturmak ve dinleyicileri ve runAsync açmak için çağrıları arasında sıralama olduğunu dikkate almak önemlidir. RunAsync başlatılmadan önce dinleyicileri açılabilir. Benzer şekilde, runAsync iletişim dinleyicileri açık veya hatta oluşturulan önce çağrılan bitirebilirsiniz. Herhangi bir eşitleme gerekiyorsa, uygulayan bir alıştırma olarak kalır. Yaygın çözümleri:

* Bazen diğer bazı bilgiler oluşturulur veya çalışma kadar dinleyicileri çalışamaz. İş genellikle hizmetin oluşturucuda sırasında yapılabilir durum bilgisi olmayan hizmetler için `createServiceInstanceListeners()` çağrısı, veya dinleyici yapımı bir parçası olarak.
* Bazen runAsync kodda başlatana kadar dinleyicileri açık istemiyor. Bu durumda ek koordinasyon gereklidir. Bir ortak Yöneticiler, gerçek çalışmaya devam etmeden önce runAsync denetleneceği tamamladığınızda belirten dinleyicileri içinde bazı bayrağı çözümüdür.

## <a name="stateless-service-shutdown"></a>Durum bilgisiz hizmet kapanması
Durum bilgisi olmayan bir hizmeti kapatılıyor, yalnızca geriye doğru aynı düzeni izlenir:

1. Paralel olarak
    - Tüm açık dinleyiciler kapalı (`CommunicationListener.closeAsync()` her dinleyici adı verilir)
    - İptal belirteci geçirilen `runAsync()` iptal edildi (iptal belirtecin denetimi `isCancelled` özelliği true döndürür ve belirtecin çağrıldıklarında `throwIfCancellationRequested` yöntemi atar bir `CancellationException`)
2. Bir kez `closeAsync()` her Dinleyicide tamamlandıktan ve `runAsync()` de tamamlar, hizmetin `StatelessService.onCloseAsync()` yöntemi çağrıldığında, varsa (yeniden seyrek bir geçersiz kılma budur).
3. Sonra `StatelessService.onCloseAsync()` tamamlar, hizmet nesnesi destructed

## <a name="stateful-service-startup"></a>Durum bilgisi olan hizmet başlatma
Durum bilgisi olan hizmetler birkaç değişikliklerle durum bilgisi olmayan hizmetler için benzer bir desen sahiptir. Bir durum bilgisi olan hizmet başlatmak için olayların sırası aşağıdaki gibidir:

1. Hizmet oluşturulur.
2. `StatefulServiceBase.onOpenAsync()`adı verilir. Bu çağrı hizmetinde yaygın olarak geçersiz kılındı değil.
3. Paralel olarak aşağıdakiler gerçekleşir:
    - `StatefulServiceBase.createServiceReplicaListeners()`çağrılır. 
      - Hizmet birincil service ise, tüm döndürülen dinleyicileri açılır. `CommunicationListener.openAsync()`Her dinleyici adı verilir.
      - Hizmet bir ikincil service ise, yalnızca bu dinleyicileri olarak işaretlenmiş `listenOnSecondary = true` açılır. İkincil üzerinde açık dinleyicileri sahip daha az yaygın bir durumdur.
    - Hizmet şu anda bir birincil, hizmet ait olup olmadığını `StatefulServiceBase.runAsync()` yöntemi çağrılır.
4. Tüm çoğaltma dinleyicisi ait sonra `openAsync()` bitiş çağırır ve `runAsync()` olarak adlandırılır, `StatefulServiceBase.onChangeRoleAsync()` olarak adlandırılır. Bu çağrı hizmetinde yaygın olarak geçersiz kılındı değil.

Durum bilgisi olmayan hizmetler, var. benzer dinleyicileri, oluşturulan ve açılan sırasını arasında hiçbir düzenlemesi ve ne zaman **runAsync** olarak adlandırılır. Düzenleme gerekiyorsa, çözümler çok aynıdır. Durum bilgisi olan hizmet için bir ek söz konusu değildir. İletişim dinleyicileri gelen çağrıları bazı içinde tutulur bilgi gerektiren söyleyin [güvenilir koleksiyonları](service-fabric-reliable-services-reliable-collections.md). Güvenilir koleksiyonları okunabilir veya yazılabilir önce açamadı. iletişim dinleyicileri olduğundan ve önce **runAsync** başlatamadığından, bazı ek koordinasyon gereklidir. En basit ve en yaygın iletişim dinleyicileri isteği yeniden denemek için bilmeniz istemcinin kullandığı bir hata kodu döndürülecek çözümüdür.

## <a name="stateful-service-shutdown"></a>Durum bilgisi olan hizmet kapatma
Durum bilgisi olmayan hizmetler gibi kapatma sırasında yaşam döngüsü olayları başlatma sırasında aynıdır, ancak tersine. Durum bilgisi olan hizmet kapatılıyor, aşağıdaki olaylar gerçekleşir:

1. Paralel olarak:
    - Tüm açık dinleyiciler kapatılır. `CommunicationListener.closeAsync()`Her dinleyici adı verilir.
    - İptal belirteci geçirilen `runAsync()` iptal edildi. İptal belirtecin çağrısı `isCancelled()` yöntemi true döndürür ve çağırdıysanız, belirtecin `throwIfCancellationRequested()` yöntemi atar bir `OperationCanceledException`.
2. Sonra `closeAsync()` her Dinleyicide tamamlandıktan ve `runAsync()` bittikten ayrıca hizmetin `StatefulServiceBase.onChangeRoleAsync()` olarak adlandırılır. Bu çağrı hizmetinde yaygın olarak geçersiz kılındı değil.

   > [!NOTE]  
   > Beklemek zorunda **runAsync** tamamlamak için bu çoğaltma birincil kopya ise yalnızca gerekli olur.

3. Sonra `StatefulServiceBase.onChangeRoleAsync()` yöntemi sonlandığında `StatefulServiceBase.onCloseAsync()` yöntemi çağrılır. Bu çağrı, seyrek bir geçersiz kılma olmakla birlikte kullanılabilir.
3. Sonra `StatefulServiceBase.onCloseAsync()` bitirdiğinde, hizmet nesnesi destructed.

## <a name="stateful-service-primary-swaps"></a>Durum bilgisi olan hizmet birincil takasları
Durum bilgisi olan hizmet çalışırken, durum bilgisi olan hizmet, yalnızca birincil çoğaltmaları açılan kendi iletişim dinleyicileri sahip ve bunların **runAsync** yöntemi çağrılır. İkincil çoğaltmaları oluşturulur, ancak hiçbir sonraki çağrılar görürsünüz. Durum bilgisi olan hizmet çalışırken, şu anda çoğaltma birincil değiştirebilirsiniz. Bu ne bir çoğaltma görebilirsiniz yaşam döngüsü olayları şartlarında anlama geliyor? Durum bilgisi olan çoğaltma görür davranışı çoğaltma veya indirgenir sırasında takas yükseltilmiş mı olduğuna bağlıdır.

### <a name="for-the-primary-thats-demoted"></a>İçin indirgenir birincil
İndirgenir birincil çoğaltma için Service Fabric iletilerini işleme durdurmak ve yapmakta olduğu herhangi bir arka plan iş çıkmak için bu çoğaltma gerekir. Bu adım sonuç olarak, hizmet kapatıldığında olduğu görülüyor. Tek fark, hizmet destructed değil veya bir ikincil olarak kaldığından Kapalı ' dir. Aşağıdaki API adı verilir:

1. Paralel olarak:
    - Tüm açık dinleyiciler kapatılır. `CommunicationListener.closeAsync()`Her dinleyici adı verilir.
    - İptal belirteci geçirilen `runAsync()` iptal edilir. İptal belirtecin kontrol `isCancelled()` yöntemi true döndürür ve çağırdıysanız, belirtecin `throwIfCancellationRequested()` yöntemi atar bir `OperationCanceledException`.
2. Sonra `closeAsync()` her Dinleyicide tamamlandıktan ve `runAsync()` bittikten ayrıca hizmetin `StatefulServiceBase.onChangeRoleAsync()` olarak adlandırılır. Bu çağrı hizmetinde yaygın olarak geçersiz kılındı değil.

### <a name="for-the-secondary-thats-promoted"></a>Tanıtılması için ikincil
Benzer şekilde, Service Fabric hattaki iletiler için dinleme ve tamamlaması gereken herhangi bir arka plan görevi başlatmak için yükseltilen ikincil çoğaltma gerekir. Bu işlem sonucu olarak hizmet oluşturulduğunda, çoğaltma zaten var ancak bu olduğu görülüyor. Aşağıdaki API adı verilir:

1. Paralel olarak:
    - `StatefulServiceBase.createServiceReplicaListeners()`çağrılır ve herhangi bir dinleyicileri açıldığından döndürdü. `CommunicationListener.openAsync()`Her dinleyici adı verilir.
    - Hizmetin `StatefulServiceBase.runAsync()` yöntemi çağrılır.
2. Tüm çoğaltma dinleyicisi ait sonra `openAsync()` bitiş çağırır ve `runAsync()` olarak adlandırılır, `StatefulServiceBase.onChangeRoleAsync()` olarak adlandırılır. Bu çağrı hizmetinde yaygın olarak geçersiz kılındı değil.


### <a name="common-issues-during-stateful-service-shutdown-and-primary-demotion"></a>Durum bilgisi olan hizmet kapanması ve birincil indirgeme sırasında ortak sorunlar
Service Fabric durum bilgisi olan bir hizmet çeşitli nedenlerle için birincil değiştirir. En yaygın olan [küme dengelenmesi](service-fabric-cluster-resource-manager-balancing.md) ve [uygulama yükseltme](service-fabric-application-upgrade.md). Bu işlemleri sırasında (yanı sıra hizmet silinmiş olup olmadığını göreceğiniz gibi normal hizmet kapatma sırasında), hizmet saygı önemli `cancellationToken`. 

İptal düzgün bir şekilde işleyemez Hizmetleri, çeşitli sorunlar yaşayabilirsiniz. Bu işlemler yavaş olduğu hizmetlerin düzgün biçimde durması için Service Fabric bekler. Sonuç olarak bu zaman aşımı başarısız yükseltme sağlama ve geri alma olabilir. İptal belirteci vermenizin hatası imbalanced kümeleri de neden olabilir. Küme düğümleri etkin alırsınız, ancak başka bir yerde taşınması çok uzun sürdüğü için hizmetleri yeniden dengelenir yapılamıyor çünkü dengesiz olur. 

Hizmetleri durum bilgisi olduğundan, ayrıca bunların kullanmak büyük olasılıkla [güvenilir koleksiyonları](service-fabric-reliable-services-reliable-collections.md). Birincil indirgenir, Service Fabric içinde gerçekleşen ilk şey alttaki durumu yazma erişimi iptal edilmediğini biridir. Bu hizmet yaşam döngüsü etkileyebilecek sorunları ikinci bir dizi sağlar. Zamanlama ve yineleme olup taşındığı dönüş özel durumlar göre koleksiyonları veya kapat. Bu özel durumlar doğru yapılmalıdır. Service Fabric tarafından oluşturulan özel durumları kalan kalıcı [(`FabricException`)](https://docs.microsoft.com/en-us/java/api/system.fabric.exception) ve geçici [(`FabricTransientException`)](https://docs.microsoft.com/en-us/java/api/system.fabric.exception._fabric_transient_exception) kategoriler. Kalıcı özel durumları günlüğe ve bazı yeniden deneme mantığına göre geçici özel durumlar denenebilir sırada oluşturulur.

Kullanımından gelen özel durumları işleme `ReliableCollections` hizmet yaşam döngüsü olayları ile birlikte test etme ve güvenilir bir hizmeti doğrulanıyor önemli bir parçasıdır. Yükseltme gerçekleştirirken hizmetinizi yük altında her zaman çalıştırmanızı öneririz ve [chaos sınama](service-fabric-controlled-chaos.md) üretime dağıtmadan önce. Bu temel adımları hizmetinizin doğru uygulanır ve yaşam döngüsü olayları doğru olarak işler sağlamaya yardımcı olur.

## <a name="notes-on-service-lifecycle"></a>Hizmet yaşam döngüsü ile ilgili notlar
* Her iki `runAsync()` yöntemi ve `createServiceInstanceListeners/createServiceReplicaListeners` çağrıları isteğe bağlıdır. Bir hizmeti bunları, her ikisini birden veya hiçbiri olabilir. Örneğin, hizmet yanıt kullanıcı çağrıları olarak tüm çalışmasını varsa, gerek yoktur, uygulamak için `runAsync()`. İletişim dinleyicileri ve bunların ilişkili kodu gereklidir. Hizmet yalnızca yapmak için arka plan çalışması ve uygulamak için bu nedenle yalnızca gereksinimlerine sahip olabilir benzer şekilde, oluşturma ve iletişim dinleyicileri döndürme isteğe bağlı, aynıdır`runAsync()`
* Tamamlamak bir hizmet için geçerli `runAsync()` başarıyla ve dönüş almaktır. Bu bir hata durumu olarak kabul edilmez ve hizmet tamamlama arka plan iş temsil eder. Durum bilgisi olan güvenilir hizmetler için `runAsync()` hizmet birincil sunucudan indirgenir ve geri birincil yükseltilmiş yeniden çağrılması.
* Gelen bir hizmet bulunup bulunmadığını `runAsync()` bazı beklenmeyen bir özel durum atma tarafından bu bir hatadır ve hizmet nesnesi kapatılır ve bir sistem durumu hatası bildirdi.
* Varken hiçbir zaman sınırı bu yöntemlerden döndürme, hemen yazmak için yönetemez ve bu nedenle herhangi bir gerçek iş tamamlayamıyor. İptal isteği aldığında gerçekleştireceği mümkün olduğunca hızlı bir şekilde olarak dönüş önerilir. Hizmetinizi makul bir sürede API çağrıları, yanıt vermiyorsa Service Fabric hizmeti zorla sonlandırabilirsiniz. Genellikle yalnızca uygulama yükseltmelerini ya da hizmet zaman siliniyor sırasında meydana gelir. Bu zaman aşımı, varsayılan değer 15 dakikadır.
* Başarısızlık `onCloseAsync()` yolu sonucunda `onAbort()` hizmetinin temizlemek bir son fırsat en yüksek çaba fırsat olduğu çağrılan ayarlama ve talep tüm kaynakları serbest bırakmak.

## <a name="next-steps"></a>Sonraki adımlar
* [Güvenilir hizmetlerine giriş](service-fabric-reliable-services-introduction.md)
* [Güvenilir hizmetler hızlı başlangıç](service-fabric-reliable-services-quick-start-java.md)
* [Kullanım Gelişmiş güvenilir hizmetler](service-fabric-reliable-services-advanced-usage.md)
