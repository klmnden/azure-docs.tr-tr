---
title: Azure Service Fabric Reliable Services yaşam döngüsü'ne genel bakış | Microsoft Docs
description: Service Fabric güvenilir hizmetler farklı bir yaşam döngüsü olayları hakkında bilgi edinin
services: Service-Fabric
documentationcenter: .net
author: masnider
manager: chackdan
editor: vturecek;
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: ebc7aec63b34630b606178aa17e2ae7fdd0fc87f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60723586"
---
# <a name="reliable-services-lifecycle-overview"></a>Reliable Services yaşam döngüsüne genel bakış
> [!div class="op_single_selector"]
> * [Windows üzerinde C#](service-fabric-reliable-services-lifecycle.md)
> * [Linux üzerinde Java](service-fabric-reliable-services-lifecycle-java.md)
>
>

Azure Service Fabric Reliable Services yaşam döngüleri hakkında düşünüyorsanız, yaşam döngüsü temelleri en önemli olan. Genel olarak, yaşam döngüsü aşağıdakileri içerir:

- Başlatma sırasında:
  - Hizmetleri oluşturulur.
  - Hizmetleri oluşturmak ve sıfır veya daha fazla dinleyicileri döndürmek için fırsatınız olur.
  - Hizmeti ile iletişimi sağlayan tüm döndürülen dinleyiciler açılmadı.
  - Hizmetin **RunAsync** yöntemi çağrıldığında, uzun soluklu görevlerin yapın veya arka plan iş hizmete izin verme.
- Kapatma sırasında:
  - İptal belirteci geçirilen **RunAsync** iptal edilir ve dinleyicileri kapatılır.
  - Hizmet nesnesi dinleyicileri kapattıktan sonra imha.

Bu olayları tam sıralama etrafında ayrıntıları vardır. Olayların sırası, güvenilir hizmet durum bilgisi olan veya bağlı olarak biraz değiştirebilirsiniz. Ayrıca, durum bilgisi olan hizmetler için biz birincil takas senaryo ile işlem yapması gerekir. Bu dizisi sırasında birincil rolü başka bir çoğaltmaya aktarılır (veya geri gelir) olmadan hizmet kapatılıyor. Son olarak, biz hata veya hata koşulları hakkında düşünmeniz gerekir.

## <a name="stateless-service-startup"></a>Durum bilgisi olmayan hizmet başlatma
Durum bilgisi olmayan hizmet yaşam döngüsü oldukça basittir. Olayların sırası şöyledir:

1. Hizmeti oluşturulur.
2. Ardından, paralel olarak iki şey olur:
    - `StatelessService.CreateServiceInstanceListeners()` çağrılan ve tüm dinleyicileri açıldığında döndürdü. `ICommunicationListener.OpenAsync()` Her dinleyici adı verilir.
    - Hizmetin `StatelessService.RunAsync()` yöntemi çağrılır.
3. Varsa, hizmetin `StatelessService.OnOpenAsync()` yöntemi çağrılır. Bu çağrı, seyrek bir geçersiz kılma olmakla birlikte kullanılabilir. Şu anda genişletilmiş hizmeti başlatma görevleri başlatılabilir.

Sıralama yok oluşturmak ve dinleyicileri açmak için çağrıları arasında olduğunu aklınızda bulundurun ve **RunAsync**. Dinleyiciler önce açabilirsiniz **RunAsync** başlatılır. Benzer şekilde, çağırabilirsiniz **RunAsync** önce iletişim dinleyicileri açık veya hatta oluşturulmuş. Herhangi bir eşitleme gerekiyorsa, için uygulayan bir alıştırma olarak kalır. Bazı yaygın çözümleri şunlardır:

  - Bazen dinleyicileri bazı diğer bilgilere oluşturulduğunda veya bir çalışma yapılmadan sürece çalışamaz. İş genellikle aşağıdaki gibi başka konumlarda yapılabilir, durum bilgisi olmayan hizmetler için: 
    - Hizmetin Oluşturucu.
    - Sırasında `CreateServiceInstanceListeners()` çağırın.
    - Dinleyici oluşumu bir parçası olarak.
  - Bazen kod **RunAsync** dinleyicileri kadar başlamaz. Bu durumda, ek koordinasyon gereklidir. Tek bir ortak çözüm zaman tamamladınız gösteren bir bayrak dinleyicileri içinde olmasıdır. Bu bayrak sonra iade **RunAsync** gerçek çalışmaya devam etmeden önce.

## <a name="stateless-service-shutdown"></a>Durum bilgisi olmayan hizmet kapanması
Bir durum bilgisi olmayan hizmeti kapatılıyor için aynı desene, yalnızca tersten izlenir:

1. Paralel olarak:
    - Tüm açık dinleyiciler kapatılır. `ICommunicationListener.CloseAsync()` Her dinleyici adı verilir.
    - İptal belirteci geçirilen `RunAsync()` iptal edilir. Bir iptal belirtecinin denetimini `IsCancellationRequested` özelliği true döndürür ve çağrılırsa, belirtecin `ThrowIfCancellationRequested` yöntem bir `OperationCanceledException`.
2. Sonra `CloseAsync()` her dinleyici tamamlandıktan ve `RunAsync()` de tamamlanır, hizmetin `StatelessService.OnCloseAsync()` yöntemi çağrıldığında, varsa.  Durum bilgisi olmayan hizmet örneğini düzgün biçimde kapatılması kolaylaştıracağını OnCloseAsync çağrılır. Bu hizmet kodunun yükseltiliyor, hizmet örneği, Yük Dengelemesi nedeniyle taşınırken ya da geçici bir hata algılandığında ortaya çıkabilir. Geçersiz kılmak için sıra dışı `StatelessService.OnCloseAsync()`, ancak güvenli bir şekilde kaynakları kapatın, arka plan işlemesi durdurmak, dış durumu kaydediyor son veya varolan bağlantıları kapatmak için kullanılabilir.
3. Sonra `StatelessService.OnCloseAsync()` tamamlandığında, hizmet nesnesi imha.

## <a name="stateful-service-startup"></a>Durum bilgisi olan hizmet başlatma
Durum bilgisi olan hizmetler birkaç değişiklik ile durum bilgisi olmayan hizmetler için benzer bir desen vardır. Durum bilgisi olan hizmet başlatmak için olayların sırası aşağıdaki gibidir:

1. Hizmeti oluşturulur.
2. `StatefulServiceBase.OnOpenAsync()` çağrılır. Bu çağrı hizmette yaygın olarak geçersiz kılınmış değil.
3. Paralel olarak aşağıdakiler gerçekleşir:
    - `StatefulServiceBase.CreateServiceReplicaListeners()` çağrılır. 
      - Hizmet, birincil hizmet ise, döndürülen tüm dinleyicileri açılır. `ICommunicationListener.OpenAsync()` Her dinleyici adı verilir.
      - İkincil bir hizmet hizmeti ise yalnızca bu dinleyicileri olarak işaretlenmiş `ListenOnSecondary = true` açılmadı. İkincil veritabanı üzerinde açık olan dinleyicileri olması daha az yaygındır.
    - Hizmet şu anda birincil, hizmete ait olup olmadığını `StatefulServiceBase.RunAsync()` yöntemi çağrılır.
4. Tüm çoğaltma dinleyicinin sonra `OpenAsync()` bitiş çağırır ve `RunAsync()` çağrıldığında `StatefulServiceBase.OnChangeRoleAsync()` çağrılır. Bu çağrı hizmette yaygın olarak geçersiz kılınmış değil.

Durum bilgisi olmayan hizmetler, orada benzer hiçbir sırası dinleyiciler, oluşturulan ve açılan arasında koordinasyon gereksinimini olabildiğince ve ne zaman **RunAsync** çağrılır. Koordinasyon gerekiyorsa, çözümleri çok aynıdır. Durum bilgisi olan hizmet için ek bir durum yoktur. Söyleyin iletişim dinleyicileri gelen çağrıları bazı içinde tutulan bilgileri gerektirir [güvenilir koleksiyonlar](service-fabric-reliable-services-reliable-collections.md).

   > [!NOTE]  
   > Güvenilir koleksiyonlar okunabilir veya yazılabilir önce iletişim dinleyicileri açılamıyor çünkü ve önce **RunAsync** başlatamadığından, bazı ek koordinasyon gereklidir. Basit ve en sık karşılaşılan istemci isteği yeniden denemek için bilmeniz gereken kullanan bir hata kodu döndürülecek iletişim dinleyicileri için çözümüdür.

## <a name="stateful-service-shutdown"></a>Durum bilgisi olan hizmet kapanması
Durum bilgisi olmayan hizmetler gibi yaşam döngüsü olayları sırasında kapatma başlatma sırasında aynıdır, ancak tersine. Durum bilgisi olan hizmet kapatılıyor, aşağıdaki olaylar gerçekleşir:

1. Paralel olarak:
    - Tüm açık dinleyiciler kapatılır. `ICommunicationListener.CloseAsync()` Her dinleyici adı verilir.
    - İptal belirteci geçirilen `RunAsync()` iptal edilir. Bir iptal belirtecinin denetimini `IsCancellationRequested` özelliği true döndürür ve çağrılırsa, belirtecin `ThrowIfCancellationRequested` yöntem bir `OperationCanceledException`.
2. Sonra `CloseAsync()` her dinleyici tamamlandıktan ve `RunAsync()` de tamamlanır, hizmetin `StatefulServiceBase.OnChangeRoleAsync()` çağrılır. Bu çağrı hizmette yaygın olarak geçersiz kılınmış değil.

   > [!NOTE]  
   > İçin beklemenize gerek **RunAsync** tamamlamak için bu çoğaltma birincil çoğaltmaya ise yalnızca gerekli olur.

3. Sonra `StatefulServiceBase.OnChangeRoleAsync()` yöntemi tamamlandığında, `StatefulServiceBase.OnCloseAsync()` yöntemi çağrılır. Bu çağrı, seyrek bir geçersiz kılma olmakla birlikte kullanılabilir.
3. Sonra `StatefulServiceBase.OnCloseAsync()` tamamlandığında, hizmet nesnesi imha.

## <a name="stateful-service-primary-swaps"></a>Durum bilgisi olan hizmet birincil takasları
Durum bilgisi olan hizmet çalışırken, açılan kendi iletişim dinleyicileri, durum bilgisi olan hizmet için birincil çoğaltmalara sahip ve bunların **RunAsync** yöntemi çağrılır. İkincil çoğaltma oluşturulur, ancak daha fazla çağrı görürsünüz. Durum bilgisi olan hizmet çalışırken, o anda birincil çoğaltma hatası veya iyileştirme Dengeleme kümesi sonucu olarak değiştirebilirsiniz. Bu bir çoğaltma görebilirsiniz yaşam döngüsü olaylarını bağlamında ne demektir? Durum bilgisi olan çoğaltma görür davranışı, çoğaltma indirgenir veya değiştirme sırasında yükseltilen olmasına göre bağlıdır.

### <a name="for-the-primary-thats-demoted"></a>İndirgenir birincil için
İndirgenir birincil çoğaltma için bu çoğaltma iletileri işlemeyi durdur ve yapmakta olduğu herhangi bir arka plan iş çıkmak için Service Fabric gerekir. Bu adım sonuç olarak, hizmet kapatıldığında olduğu görülüyor. Tek fark, hizmet imha değil veya bir ikincil olarak ettiğinden Kapalı ' dir. Aşağıdaki API adı verilir:

1. Paralel olarak:
    - Tüm açık dinleyiciler kapatılır. `ICommunicationListener.CloseAsync()` Her dinleyici adı verilir.
    - İptal belirteci geçirilen `RunAsync()` iptal edilir. Bir iptal belirtecinin denetimini `IsCancellationRequested` özelliği true döndürür ve çağrılırsa, belirtecin `ThrowIfCancellationRequested` yöntem bir `OperationCanceledException`.
2. Sonra `CloseAsync()` her dinleyici tamamlandıktan ve `RunAsync()` de tamamlanır, hizmetin `StatefulServiceBase.OnChangeRoleAsync()` çağrılır. Bu çağrı hizmette yaygın olarak geçersiz kılınmış değil.

### <a name="for-the-secondary-thats-promoted"></a>Tanıtılması için ikincil
Benzer şekilde, Service Fabric kablo iletileri dinleyen ve tamamlanması gereken herhangi bir arka plan görevini başlatmak için yükseltilen ikincil bir çoğaltmaya gerekir. Bu işlem sonucunda, hizmet oluşturulduğunda dışında çoğaltma zaten var olduğu görülüyor. Aşağıdaki API adı verilir:

1. Paralel olarak:
    - `StatefulServiceBase.CreateServiceReplicaListeners()` çağrılan ve tüm dinleyicileri açıldığında döndürdü. `ICommunicationListener.OpenAsync()` Her dinleyici adı verilir.
    - Hizmetin `StatefulServiceBase.RunAsync()` yöntemi çağrılır.
2. Tüm çoğaltma dinleyicinin sonra `OpenAsync()` bitiş çağırır ve `RunAsync()` çağrıldığında `StatefulServiceBase.OnChangeRoleAsync()` çağrılır. Bu çağrı hizmette yaygın olarak geçersiz kılınmış değil.

### <a name="common-issues-during-stateful-service-shutdown-and-primary-demotion"></a>Durum bilgisi olan hizmet kapanması ve birincil indirgeme sırasında sık karşılaşılan sorunlar
Service Fabric durum bilgisi olan bir hizmet çeşitli nedenlerle için birincil değiştirir. En yaygın olan [küme yeniden Dengeleme](service-fabric-cluster-resource-manager-balancing.md) ve [uygulama yükseltmesi](service-fabric-application-upgrade.md). Bu işlemler (betiklerinizi hizmet silindikten sonra göreceğiniz gibi normal hizmeti kapatma sırasında), hizmet uyduğunuzdan önemli olduğu `CancellationToken`. 

İptal düzgün bir şekilde işlememesi Hizmetleri, çeşitli sorunlar oluşabilir. Bu işlem, Service Fabric Hizmetleri düzgün bitirmesini beklediğinden yavaş yükleniyor. Sonuç olarak, zaman aşımı başarısız yükseltme sağlama ve geri alma olabilir. İptal belirtecini kabul etmenin hatası imbalanced kümeleri da neden olabilir. Küme düğümleri sık erişimli alabilirsiniz, ama başka bir yere taşınması çok uzun sürdüğü için hizmetleri yeniden Dengelenecek olamaz çünkü dengesiz hale. 

Durum bilgisi olan hizmetler olduğundan, ayrıca, kullandıkları olma olasılığı yüksektir [güvenilir koleksiyonlar](service-fabric-reliable-services-reliable-collections.md). Birincil indirgenir, Service Fabric'te gerçekleşen ilk şey temel alınan durum yazma erişimi iptal edilmediğini biridir. Bu hizmet yaşam döngüsü etkileyebilecek sorunları ikinci bir set yol açar. Dönüş özel durumları, zamanlama ve yineleme olup taşınırken göre koleksiyonlar veya kapat. Bu özel durumların doğru şekilde yapılması gerekir. Service Fabric tarafından oluşturulan özel durumları kalan kalıcı [(`FabricException`)](https://docs.microsoft.com/dotnet/api/system.fabric.fabricexception?view=azure-dotnet) ve geçici [(`FabricTransientException`)](https://docs.microsoft.com/dotnet/api/system.fabric.fabrictransientexception?view=azure-dotnet) kategorileri. Kalıcı özel durumlar günlüğe kaydedilir ve geçici özel durumlar, bazı yeniden deneme mantığı temelinde denenebilir sırasında oluşturulur.

Kullanımından gelen özel durumları işleme `ReliableCollections` hizmet yaşam döngüsü olayları ile birlikte sınarken ve doğrularken bir güvenilir hizmet, önemli bir parçasıdır. Yükseltme gerçekleştirirken hizmetinizin yük altında her zaman çalıştırmanızı öneririz ve [chaos test](service-fabric-controlled-chaos.md) üretim ortamına dağıtmadan önce. Hizmetinizin doğru bir şekilde uygulandığından ve yaşam döngüsü olayları doğru olarak işler temel adımları sağlamak.


## <a name="notes-on-the-service-lifecycle"></a>Hizmet yaşam döngüsü ile ilgili notlar
  - Her iki `RunAsync()` yöntemi ve `CreateServiceReplicaListeners/CreateServiceInstanceListeners` çağrılarıdır isteğe bağlı. Bir hizmetin bunları, her ikisi de veya hiçbiri olabilir. Örneğin, hizmet tüm yanıt kullanıcı çağrı yapar, gerek yoktur, uygulamak için `RunAsync()`. İletişim dinleyicileri ve ilişkili kodunu gereklidir. Hizmet, yalnızca arka plan iş yapmak ve uygulamak için bu nedenle yalnızca gereksinimlerini olabildiği gibi benzer şekilde, oluşturma ve iletişim dinleyicileri döndürerek isteğe bağlı, `RunAsync()`.
  - Bir hizmet için geçerli olduğundan `RunAsync()` başarıyla ve ondan geri dönüş. Tamamlanıyor hata durumu değil. Tamamlama `RunAsync()` hizmetin arka plan iş tamamladığını gösterir. Durum bilgisi olan reliable services için `RunAsync()` çoğaltmayı birincil sunucudan ikincil indirgenir ve sonra birincil siteye yükseltilen yeniden adlandırılır.
  - Hizmet gelen çıkılıyorsa `RunAsync()` bazı beklenmeyen özel durum olarak, bu bir hata oluşturur. Hizmet nesnesi kapatılır ve sistem durumu hata bildirilir.
  - Olmasa da zaman sınırı bu yöntemleri döndüren üzerinde güvenilir koleksiyonlar için yazma olanağı derhal kaybeder ve bu nedenle, herhangi bir gerçek çalışma tamamlanamıyor. İptal isteği alır almaz mümkün olduğunca çabuk dönüş önerilir. Hizmetinizi şu API çağrıları için makul bir süre içinde yanıt vermezse, Service Fabric hizmetinizi zorla sonlandırabilirsiniz. Bu genellikle yalnızca uygulama yükseltmeleri veya hizmet ne zaman siliniyor sırasında gerçekleşir. Bu zaman aşımı, varsayılan değer 15 dakikadır.
  - Hataları `OnCloseAsync()` yolu sonucunda `OnAbort()` çağrılan, olan son şans en yüksek çaba fırsat hizmeti temizlemek ve talep tüm kaynakları serbest bırakmak. Bu genellikle düğümde kalıcı bir hata algılandığında veya Service Fabric güvenilir hizmeti örneğinin yaşam döngüsü iç hatalar nedeniyle yönetememesi durumunda olarak adlandırılır.
  - `OnChangeRoleAsync()` durum bilgisi olan hizmet çoğaltma rolü, örneğin birincil veya ikincil değiştirirken çağırılır. Birincil çoğaltmalara yazma durumu verilmiştir (oluşturun ve güvenilir koleksiyonlar için yazma izni verilir). İkincil çoğaltmalar, Okuma durumu (yalnızca var olan güvenilir koleksiyonlardan okuyabilir) atanır. Çoğu iş durum bilgisi olan hizmet, birincil çoğaltma üzerinde gerçekleştirilir. İkincil çoğaltmaların salt okunur doğrulama, rapor oluşturma, veri araştırma veya diğer salt okunur işleri gerçekleştirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
- [Reliable Services giriş](service-fabric-reliable-services-introduction.md)
- [Reliable Services hızlı başlangıç](service-fabric-reliable-services-quick-start.md)
- [Çoğaltmalar ve örnekler](service-fabric-concepts-replica-lifecycle.md)
