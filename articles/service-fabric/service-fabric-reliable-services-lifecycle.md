---
title: Azure Service Fabric güvenilir hizmetler yaşam döngüsüne genel bakış | Microsoft Docs
description: Service Fabric Reliable Services farklı yaşam döngüsü olayları hakkında bilgi edinin
services: Service-Fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: vturecek;
ms.assetid: ''
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 42833323cbebf25ce2ca14e6ab7ec4fa5adbfd15
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="reliable-services-lifecycle-overview"></a>Güvenilir hizmetler yaşam döngüsüne genel bakış
> [!div class="op_single_selector"]
> * [Windows üzerinde C#](service-fabric-reliable-services-lifecycle.md)
> * [Linux üzerinde Java](service-fabric-reliable-services-lifecycle-java.md)
>
>

Azure Service Fabric güvenilir hizmetler yaşam döngüleri hakkında düşünürken yaşam döngüsü temelleri en önemli olan. Genel olarak, yaşam döngüsü aşağıdakileri içerir:

- Başlatma sırasında:
  - Hizmetleri oluşturulur.
  - Hizmetleri oluşturmak ve sıfır veya daha fazla dinleyicileri dönmek için fırsatınız olur.
  - Hizmeti ile iletişimi sağlayan tüm döndürülen dinleyiciler açıldığından.
  - Hizmetin **RunAsync** yöntemi çağrıldığında, hizmet uzun süre çalışan görevler veya arka plan çalışması için izin verme.
- Kapatma sırasında:
  - İptal belirteci geçirilen **RunAsync** iptal edilir ve dinleyicileri kapalı.
  - Dinleyicileri kapattıktan sonra hizmet nesnesi destructed.

Bu olaylar tam sıralama geçici ayrıntıları vardır. Olayların sırası güvenilir hizmet durum bilgisiz veya durum bilgisi olan bağlı olarak biraz değiştirebilirsiniz. Ayrıca, durum bilgisi olan hizmetler için biz birincil takas senaryoyla ilgilenmeniz gerekir. Bu dizisi sırasında birincil rolü başka bir çoğaltmaya aktarılır (veya geri gelir) olmadan hizmet kapatılıyor. Son olarak, biz hata veya hata koşulları hakkında düşünmeniz gerekir.

## <a name="stateless-service-startup"></a>Durum bilgisiz hizmetini başlatma
Durum bilgisiz hizmet yaşam döngüsü basittir. Olayların sırası şöyledir:

1. Hizmet oluşturulur.
2. Ardından, paralel olarak iki şey olur:
    - `StatelessService.CreateServiceInstanceListeners()` çağrılır ve herhangi bir dinleyicileri açıldığından döndürdü. `ICommunicationListener.OpenAsync()` Her dinleyici adı verilir.
    - Hizmetin `StatelessService.RunAsync()` yöntemi çağrılır.
3. Varsa, hizmetin `StatelessService.OnOpenAsync()` yöntemi çağrılır. Bu çağrı, seyrek bir geçersiz kılma olmakla birlikte kullanılabilir. Genişletilmiş hizmeti başlatma görevleri şu anda başlatılabilir.

Sıralamaya oluşturmak ve dinleyicileri açmak için çağrıları arasında olduğunu aklınızda bulundurun ve **RunAsync**. Dinleyicileri önce açabilirsiniz **RunAsync** başlatılır. Benzer şekilde, çağırabileceği **RunAsync** iletişim dinleyicileri açık veya hatta oluşturulan önce. Herhangi bir eşitleme gerekiyorsa, uygulayan bir alıştırma olarak kalır. Bazı yaygın çözümleri şunlardır:

  - Bazen dinleyicileri diğer bazı bilgiler oluşturulur veya iş yapılır kadar çalışamaz. İş genellikle aşağıdaki gibi diğer konumlarda yapılabilecek durum bilgisi olmayan hizmetler için: 
    - Hizmetin Oluşturucu.
    - Sırasında `CreateServiceInstanceListeners()` çağırın.
    - Dinleyici yapımı bir parçası olarak.
  - Bazen kodda **RunAsync** dinleyicileri açık kadar başlamıyor. Bu durumda, ek koordinasyon gereklidir. Bir ortak çözüm zaman bitirdikten belirten bir bayrak dinleyicileri içinde olmasıdır. Bu bayrak sonra iade **RunAsync** gerçek çalışmaya devam etmeden önce.

## <a name="stateless-service-shutdown"></a>Durum bilgisiz hizmet kapanması
Durum bilgisi olmayan bir hizmeti kapatılıyor için aynı düzeni, yalnızca geriye doğru izlenir:

1. Paralel olarak:
    - Tüm açık dinleyiciler kapatılır. `ICommunicationListener.CloseAsync()` Her dinleyici adı verilir.
    - İptal belirteci geçirilen `RunAsync()` iptal edilir. İptal belirtecin kontrol `IsCancellationRequested` özelliği true döndürür ve çağırdıysanız, belirtecin `ThrowIfCancellationRequested` yöntemi atar bir `OperationCanceledException`.
2. Sonra `CloseAsync()` her Dinleyicide tamamlandıktan ve `RunAsync()` bittikten ayrıca hizmetin `StatelessService.OnCloseAsync()` yöntemi çağrıldığında, varsa.  Durum bilgisiz hizmet örneği düzgün biçimde kapatılması geçerken OnCloseAsync adı verilir. Bu hizmetin kod yükseltilmesi, hizmet örneği, Yük Dengeleme nedeniyle taşındığı ya da geçici bir hata algılandığında ortaya çıkabilir. Geçersiz kılmak için seyrek `StatelessService.OnCloseAsync()`, ancak güvenle kaynakları kapatmak, arka plan işleme durdurmak, dış durumu kaydetmeyi bitirmek veya var olan bağlantıların kapatmak için kullanılabilir.
3. Sonra `StatelessService.OnCloseAsync()` bitirdiğinde, hizmet nesnesi destructed.

## <a name="stateful-service-startup"></a>Durum bilgisi olan hizmet başlatma
Durum bilgisi olan hizmetler birkaç değişikliklerle durum bilgisi olmayan hizmetler için benzer bir desen sahiptir. Bir durum bilgisi olan hizmet başlatmak için olayların sırası aşağıdaki gibidir:

1. Hizmet oluşturulur.
2. `StatefulServiceBase.OnOpenAsync()` adı verilir. Bu çağrı hizmetinde yaygın olarak geçersiz kılındı değil.
3. Paralel olarak aşağıdakiler gerçekleşir:
    - `StatefulServiceBase.CreateServiceReplicaListeners()` çağrılır. 
      - Hizmet birincil service ise, tüm döndürülen dinleyicileri açılır. `ICommunicationListener.OpenAsync()` Her dinleyici adı verilir.
      - Hizmet bir ikincil service ise, yalnızca bu dinleyicileri olarak işaretlenmiş `ListenOnSecondary = true` açılır. İkincil üzerinde açık dinleyicileri sahip daha az yaygın bir durumdur.
    - Hizmet şu anda bir birincil, hizmet ait olup olmadığını `StatefulServiceBase.RunAsync()` yöntemi çağrılır.
4. Tüm çoğaltma dinleyicisi ait sonra `OpenAsync()` bitiş çağırır ve `RunAsync()` olarak adlandırılır, `StatefulServiceBase.OnChangeRoleAsync()` olarak adlandırılır. Bu çağrı hizmetinde yaygın olarak geçersiz kılındı değil.

Durum bilgisi olmayan hizmetler, var. benzer dinleyicileri, oluşturulan ve açılan sırasını arasında hiçbir düzenlemesi ve ne zaman **RunAsync** olarak adlandırılır. Düzenleme gerekiyorsa, çözümler çok aynıdır. Durum bilgisi olan hizmet için bir ek söz konusu değildir. İletişim dinleyicileri gelen çağrıları bazı içinde tutulur bilgi gerektiren söyleyin [güvenilir koleksiyonları](service-fabric-reliable-services-reliable-collections.md). Güvenilir koleksiyonları okunabilir veya yazılabilir önce açamadı. iletişim dinleyicileri olduğundan ve önce **RunAsync** başlatamadığından, bazı ek koordinasyon gereklidir. En basit ve en yaygın iletişim dinleyicileri isteği yeniden denemek için bilmeniz istemcinin kullandığı bir hata kodu döndürülecek çözümüdür.

## <a name="stateful-service-shutdown"></a>Durum bilgisi olan hizmet kapatma
Durum bilgisi olmayan hizmetler gibi kapatma sırasında yaşam döngüsü olayları başlatma sırasında aynıdır, ancak tersine. Durum bilgisi olan hizmet kapatılıyor, aşağıdaki olaylar gerçekleşir:

1. Paralel olarak:
    - Tüm açık dinleyiciler kapatılır. `ICommunicationListener.CloseAsync()` Her dinleyici adı verilir.
    - İptal belirteci geçirilen `RunAsync()` iptal edilir. İptal belirtecin kontrol `IsCancellationRequested` özelliği true döndürür ve çağırdıysanız, belirtecin `ThrowIfCancellationRequested` yöntemi atar bir `OperationCanceledException`.
2. Sonra `CloseAsync()` her Dinleyicide tamamlandıktan ve `RunAsync()` bittikten ayrıca hizmetin `StatefulServiceBase.OnChangeRoleAsync()` olarak adlandırılır. Bu çağrı hizmetinde yaygın olarak geçersiz kılındı değil.

   > [!NOTE]  
   > Beklemek zorunda **RunAsync** tamamlamak için bu çoğaltma birincil kopya ise yalnızca gerekli olur.

3. Sonra `StatefulServiceBase.OnChangeRoleAsync()` yöntemi sonlandığında `StatefulServiceBase.OnCloseAsync()` yöntemi çağrılır. Bu çağrı, seyrek bir geçersiz kılma olmakla birlikte kullanılabilir.
3. Sonra `StatefulServiceBase.OnCloseAsync()` bitirdiğinde, hizmet nesnesi destructed.

## <a name="stateful-service-primary-swaps"></a>Durum bilgisi olan hizmet birincil takasları
Durum bilgisi olan hizmet çalışırken, durum bilgisi olan hizmet, yalnızca birincil çoğaltmaları açılan kendi iletişim dinleyicileri sahip ve bunların **RunAsync** yöntemi çağrılır. İkincil çoğaltmaları oluşturulur, ancak hiçbir sonraki çağrılar görürsünüz. Durum bilgisi olan hizmet çalışırken, şu anda çoğaltma birincil değiştirebilirsiniz. Bu ne bir çoğaltma görebilirsiniz yaşam döngüsü olayları şartlarında anlama geliyor? Durum bilgisi olan çoğaltma görür davranışı çoğaltma veya indirgenir sırasında takas yükseltilmiş mı olduğuna bağlıdır.

### <a name="for-the-primary-thats-demoted"></a>İçin indirgenir birincil
İndirgenir birincil çoğaltma için Service Fabric iletilerini işleme durdurmak ve yapmakta olduğu herhangi bir arka plan iş çıkmak için bu çoğaltma gerekir. Bu adım sonuç olarak, hizmet kapatıldığında olduğu görülüyor. Tek fark, hizmet destructed değil veya bir ikincil olarak kaldığından Kapalı ' dir. Aşağıdaki API adı verilir:

1. Paralel olarak:
    - Tüm açık dinleyiciler kapatılır. `ICommunicationListener.CloseAsync()` Her dinleyici adı verilir.
    - İptal belirteci geçirilen `RunAsync()` iptal edilir. İptal belirtecin kontrol `IsCancellationRequested` özelliği true döndürür ve çağırdıysanız, belirtecin `ThrowIfCancellationRequested` yöntemi atar bir `OperationCanceledException`.
2. Sonra `CloseAsync()` her Dinleyicide tamamlandıktan ve `RunAsync()` bittikten ayrıca hizmetin `StatefulServiceBase.OnChangeRoleAsync()` olarak adlandırılır. Bu çağrı hizmetinde yaygın olarak geçersiz kılındı değil.

### <a name="for-the-secondary-thats-promoted"></a>Tanıtılması için ikincil
Benzer şekilde, Service Fabric hattaki iletiler için dinleme ve tamamlaması gereken herhangi bir arka plan görevi başlatmak için yükseltilen ikincil çoğaltma gerekir. Bu işlem sonucu olarak hizmet oluşturulduğunda, çoğaltma zaten var ancak bu olduğu görülüyor. Aşağıdaki API adı verilir:

1. Paralel olarak:
    - `StatefulServiceBase.CreateServiceReplicaListeners()` çağrılır ve herhangi bir dinleyicileri açıldığından döndürdü. `ICommunicationListener.OpenAsync()` Her dinleyici adı verilir.
    - Hizmetin `StatefulServiceBase.RunAsync()` yöntemi çağrılır.
2. Tüm çoğaltma dinleyicisi ait sonra `OpenAsync()` bitiş çağırır ve `RunAsync()` olarak adlandırılır, `StatefulServiceBase.OnChangeRoleAsync()` olarak adlandırılır. Bu çağrı hizmetinde yaygın olarak geçersiz kılındı değil.

### <a name="common-issues-during-stateful-service-shutdown-and-primary-demotion"></a>Durum bilgisi olan hizmet kapanması ve birincil indirgeme sırasında ortak sorunlar
Service Fabric durum bilgisi olan bir hizmet çeşitli nedenlerle için birincil değiştirir. En yaygın olan [küme dengelenmesi](service-fabric-cluster-resource-manager-balancing.md) ve [uygulama yükseltme](service-fabric-application-upgrade.md). Bu işlemleri sırasında (yanı sıra hizmet silinmiş olup olmadığını göreceğiniz gibi normal hizmet kapatma sırasında), hizmet saygı önemli `CancellationToken`. 

İptal düzgün bir şekilde işleyemez Hizmetleri, çeşitli sorunlar yaşayabilirsiniz. Bu işlemler yavaş olduğu hizmetlerin düzgün biçimde durması için Service Fabric bekler. Sonuç olarak bu zaman aşımı başarısız yükseltme sağlama ve geri alma olabilir. İptal belirteci vermenizin hatası imbalanced kümeleri de neden olabilir. Küme düğümleri etkin alırsınız, ancak başka bir yerde taşınması çok uzun sürdüğü için hizmetleri yeniden dengelenir yapılamıyor çünkü dengesiz olur. 

Hizmetleri durum bilgisi olduğundan, ayrıca bunların kullanmak büyük olasılıkla [güvenilir koleksiyonları](service-fabric-reliable-services-reliable-collections.md). Birincil indirgenir, Service Fabric içinde gerçekleşen ilk şey alttaki durumu yazma erişimi iptal edilmediğini biridir. Bu hizmet yaşam döngüsü etkileyebilecek sorunları ikinci bir dizi sağlar. Zamanlama ve yineleme olup taşındığı dönüş özel durumlar göre koleksiyonları veya kapat. Bu özel durumlar doğru yapılmalıdır. Service Fabric tarafından oluşturulan özel durumları kalan kalıcı [(`FabricException`)](https://docs.microsoft.com/dotnet/api/system.fabric.fabricexception?view=azure-dotnet) ve geçici [(`FabricTransientException`)](https://docs.microsoft.com/dotnet/api/system.fabric.fabrictransientexception?view=azure-dotnet) kategoriler. Kalıcı özel durumları günlüğe ve bazı yeniden deneme mantığına göre geçici özel durumlar denenebilir sırada oluşturulur.

Kullanımından gelen özel durumları işleme `ReliableCollections` hizmet yaşam döngüsü olayları ile birlikte test etme ve güvenilir bir hizmeti doğrulanıyor önemli bir parçasıdır. Yükseltme gerçekleştirirken hizmetinizi yük altında her zaman çalıştırmanızı öneririz ve [chaos sınama](service-fabric-controlled-chaos.md) üretime dağıtmadan önce. Bu temel adımları hizmetinizin doğru uygulanır ve yaşam döngüsü olayları doğru olarak işler sağlamaya yardımcı olur.


## <a name="notes-on-the-service-lifecycle"></a>Hizmet yaşam döngüsü ile ilgili notlar
  - Her iki `RunAsync()` yöntemi ve `CreateServiceReplicaListeners/CreateServiceInstanceListeners` çağrıları isteğe bağlıdır. Bir hizmetin bunları, her ikisini birden veya hiçbiri olabilir. Örneğin, hizmet yanıt kullanıcı çağrıları olarak tüm çalışmasını varsa, gerek yoktur, uygulamak için `RunAsync()`. İletişim dinleyicileri ve bunların ilişkili kodu gereklidir. Hizmet yalnızca yapmak için arka plan çalışması ve uygulamak için bu nedenle yalnızca gereksinimlerine sahip olabilir benzer şekilde, oluşturma ve iletişim dinleyicileri döndürme isteğe bağlı, aynıdır `RunAsync()`.
  - Tamamlamak bir hizmet için geçerli `RunAsync()` başarıyla ve dönüş almaktır. Tamamlanıyor bir hata durumu değil. Tamamlanıyor `RunAsync()` hizmetinin arka plan çalışması tamamlandı gösterir. Durum bilgisi olan güvenilir hizmetler için `RunAsync()` çoğaltmayı birincil sunucudan ikincil indirgenir ve geri birincil yükseltilmiş yeniden adlandırılır.
  - Gelen bir hizmet bulunup bulunmadığını `RunAsync()` bazı beklenmeyen bir özel durum atma tarafından bu bir hata oluşturduğunu. Hizmet nesnesi kapatılır ve bir sistem durumu hatası bildirilir.
  - Hiçbir zaman bir sınır olmasa bu yöntemlerden döndürme, hemen yönetemez güvenilir koleksiyonlarına yazmak için ve bu nedenle, herhangi bir gerçek iş tamamlayamıyor. İptal isteği aldığında gerçekleştireceği mümkün olduğunca hızlı bir şekilde olarak dönüş önerilir. Hizmetinizi makul bir sürede API çağrıları, yanıt vermezse, Service Fabric zorla hizmetinizi sonlandırabilir. Genellikle yalnızca uygulama yükseltmelerini ya da hizmet zaman siliniyor sırasında meydana gelir. Bu zaman aşımı, varsayılan değer 15 dakikadır.
  - Başarısızlık `OnCloseAsync()` yolu sonucunda `OnAbort()` çağrılan, olduğu son olasılığı en yüksek çaba fırsat hizmetinin temizlemek ve talep tüm kaynakları serbest bırakmak. Bu genellikle düğümde kalıcı bir hata algılandığında veya Service Fabric güvenilir bir şekilde iç hataları nedeniyle hizmet örneğinin yaşam döngüsü yönetemediğinde olarak adlandırılır.
  - `OnChangeRoleAsync()` durum bilgisi olan hizmet çoğaltma rolü, örneğin birincil veya ikincil değiştirilirken adı verilir. Birincil çoğaltmalara yazma durumu verilmiştir (oluşturun ve güvenilir koleksiyonlarına yazma izni verilir). İkincil çoğaltmaları (yalnızca var olan güvenilir koleksiyonlarından okuyabilir) okuma durumu atanır. Durum bilgisi olan hizmet çoğu çalışmasında birincil kopyada gerçekleştirilir. İkincil çoğaltmaların salt okunur doğrulama, rapor oluşturma, veri araştırma veya diğer salt okunur işleri gerçekleştirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
- [Güvenilir hizmetlerine giriş](service-fabric-reliable-services-introduction.md)
- [Güvenilir hizmetler hızlı başlangıç](service-fabric-reliable-services-quick-start.md)
- [Çoğaltmalar ve örnekler](service-fabric-concepts-replica-lifecycle.md)
