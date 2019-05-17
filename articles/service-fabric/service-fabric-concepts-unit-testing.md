---
title: Azure Service Fabric'te durum bilgisi olan hizmetler test birimi | Microsoft Docs
description: Uygulamalarının birim testi Service Fabric durum bilgisi olan hizmetler ve kavramlar hakkında bilgi edinin.
services: service-fabric
documentationcenter: .net
author: athinanthny
manager: chackdan
editor: vturecek
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/04/2018
ms.author: atsenthi
ms.openlocfilehash: ad7cf3a1dfcef8795ceb378a59a1cf0b2010293e
ms.sourcegitcommit: 1fbc75b822d7fe8d766329f443506b830e101a5e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65595506"
---
# <a name="unit-testing-stateful-services-in-service-fabric"></a>Birim Service Fabric durum bilgisi olan hizmetler testi

Bu makale, kavramlar ve birim testi Service Fabric durum bilgisi olan hizmetler uygulamalarını kapsar. Birim testi içinde Service Fabric uygulama kodu birden çok farklı bağlamı altında etkin bir şekilde çalışır. Bunun nedeni, kendi konuları hak ediyor. Bu makalede, uygulama kodu her çalışabilmesi için farklı Bağlamlar altında kapsandığından emin olmak için kullanılan yöntemler açıklanır.

## <a name="unit-testing-and-mocking"></a>Test etme ve sahte işlem birimi
Birim testi bu makalede bağlamında otomatik MSTest veya NUnit gibi bir test Çalıştırıcısı bağlam içinde yürütülen test etme. Bu makalede içinde birim testleri bir veritabanı veya RESTFul API gibi uzak bir kaynağa karşı işlemleri değil. Bu uzak kaynaklar örnek. Bu makalede bağlamında sahte işlem sahte, kayıt ve dönüş değerlerini uzak kaynaklar için denetim.

### <a name="service-fabric-considerations"></a>Service Fabric hakkında önemli noktalar
Birim testi bir Service Fabric durum bilgisi olan hizmeti bazı önemli noktalar vardır. İlk olarak, birden çok düğümde ancak farklı rolleri altında hizmet kodu yürütür. Birim testlerini kod kapsamlı bilgi elde etmek için her rolün değerlendirmelidir. Farklı rolleri birincil ve ikincil etkin, boşta ikincil ve bilinmiyor olacaktır. Hiçbir rol genellikle ihtiyacı yoktur hiçbir özel kapsam olarak Service Fabric hizmeti void veya null olarak bu rolü göz önünde bulundurur. İkincisi, her düğüm rolünü verilen herhangi bir noktada değiştirir. Tam kapsamı elde etmek için kod yürütme yolun gerçekleşen rol değişikliklerle test edilmelidir.

## <a name="why-unit-test-stateful-services"></a>Neden birim testi durum bilgisi olan hizmetler? 
Birim testi durum bilgisi olan hizmetler, mutlaka geleneksel uygulama veya etki alanına özgü birim testi tarafından yakalanabilen değil yapılan bazı yaygın hatalar ortaya çıkarmaya yardımcı olabilir. Örneğin, durum bilgisi olan hizmet herhangi bir bellek içi durum varsa, bu tür bir testi bu bellek içi durumu her çoğaltma arasında eşitleme tutulur doğrulayabilirsiniz. Bu tür bir durum bilgisi olan hizmeti tarafından Service Fabric düzenleme uygun şekilde geçirilen iptal belirteçleri için yanıt verdiğini da doğrulayabilirsiniz. İptalleri tetiklendiğinde, hizmetin uzun çalıştığından ve/veya zaman uyumsuz işlemleri durdurma.  

## <a name="common-practices"></a>Genel yöntemler

Aşağıdaki bölümde, birim testi durum bilgisi olan hizmet için yaygın uygulamalarını önerir. Sahte işlem katmanı yakından durum yönetimi ve Service Fabric düzenleme hizalamak olmalıdır önerir. [ServiceFabric.Mocks](https://www.nuget.org/packages/ServiceFabric.Mocks/) 3.3.0 itibarıyla ya da sonraki önerilen sahte işlevini sağlayan bir tür kitaplığı ve aşağıda açıklanan yöntemler izler.

### <a name="arrangement"></a>Düzenleme

#### <a name="use-multiple-service-instances"></a>Birden çok hizmet örnekleri kullan
Birim testleri, durum bilgisi olan bir hizmet birden çok örneğini getirmesi gerekir. Bu, aslında burada Service Fabric hizmetinizi farklı düğümlere çalıştıran birden çok çoğaltma sağlar küme üzerinde ne benzetimini yapar. Bu örneklerin birinde farklı bir bağlam altında ancak yürütülmesi. Test çalışırken, her örneği kümede beklendiği rolü yapılandırması ile primed. Örneğin, hizmet, hedef çoğaltma boyutu 3 olması bekleniyorsa, Service Fabric farklı düğümlere üç kopyaya sağlayın. Bunlardan biri, birincil ve etkin ikincil ait olan ve diğer iki oluşturuluyor.

Çoğu durumda, hizmet yürütme yolun biraz bu rollerin her birini için farklılık gösterir. Örneğin, hizmetin bir etkin ikincil gelen istekleri kabul, hizmetin bir onay isteği gösteren bilgilendirici bir özel durum bir ikincil girişiminde bulunuldu geri atmak bu durum için olabilir. Birden çok örnek içeren test edilecek bu duruma izin verir.

Ayrıca, birden fazla örnek içeren yanıtlar rağmen rol değişiklikleri tutarlı olduğunu doğrulamak için bu örneklerin her rollerini değiştirmek için testleri sağlar.

#### <a name="mock-the-state-manager"></a>Durum Yöneticisi sahte
Durum Yöneticisi uzak bir kaynak olarak kabul edilir ve bu nedenle örnek. Durum Yöneticisi sahte işlem, yok, okuma doğrulanır ve böylece durum Yöneticisi'ne kaydedilir izleme için bazı temel alınan bellek içi depolama olması gerekir. Bunu başarmak için basit bir yol, güvenilir koleksiyonlar türlerinin her biri sahte örneklerini oluşturmaktır. Bu mocks içinde söz konusu koleksiyonunda gerçekleştirilen işlemleri ile yakından eşleşen bir veri türünü kullanın. Güvenilir her koleksiyon için bazı önerilen veri türleri aşağıda verilmiştir

- < TKey, TValue > IReliableDictionary System.Collections.Concurrent.ConcurrentDictionary < TKey, TValue > ->
- IReliableQueue<T> sıra System.Collections.Generic.Queue ' -><T>
- IReliableConcurrentQueue<T> System.Collections.Concurrent.ConcurrentQueue -><T>

#### <a name="many-state-manager-instances-single-storage"></a>Birçok durum Yöneticisi örneği, tek bir depolama
Daha önce belirtildiği gibi durum Yöneticisi ve güvenilir koleksiyonlar, uzak bir kaynak olarak düşünülmelidir. Bu nedenle, bu kaynaklar gerekir ve birim testleri örnek. Ancak, birden çok durum bilgisi olan hizmet örneklerini çalıştırırken bu sahte bir durum Yöneticisi her farklı durum bilgisi olan hizmet örnekleri arasında eşitlenmiş şekilde tutmanızı sağlayacak bir sınama olacaktır. Durum bilgisi olan hizmet küme üzerinde çalışırken, Service Fabric durum Yöneticisi her ikincil yinelemenin birincil çoğaltma ile tutarlı kalmasını üstlenir. Bu nedenle, böylece rol değişiklikleri benzetimini yapabilirsiniz testler aynı çalışacaktır.

Bu eşitleme gerçekleştirilebilir, basit bir yol olan güvenilir her koleksiyon için yazılan veri depolayan temel alınan nesne için bir singleton deseni kullanılacak. Örneğin, bir durum bilgisi olan hizmeti kullanarak bir `IReliableDictionary<string, string>`. Sahte bir durum Yöneticisi, sahte döndürmelidir `IReliableDictionary<string, string>`. Bu sahte kullanabilecek bir `ConcurrentDictionary<string, string>` yazılan anahtar/değer çiftleri izlemek için. `ConcurrentDictionary<string, string>` Durumu yöneticilerinin tüm örnekleri tarafından kullanılan bir singleton hizmete geçirilmelidir.

#### <a name="keep-track-of-cancellation-tokens"></a>İptal belirteçleri izler
İptal belirteçleri, önemli bir henüz yaygın durum bilgisi olan hizmetler yönüyle göz ardı edilir ' dir. Service Fabric durum bilgisi olan hizmet için birincil kopya başlatıldığında, bir iptal belirteci sağlanır. Bu iptal belirteci kaldırıldı veya farklı bir role indirgenir hizmete göstermek için tasarlanmıştır. Durum bilgisi olan hizmet, Service Fabric rol değişikliği iş akışı işlemini tamamlayabiliyorum, uzun çalışan veya zaman uyumsuz işlemleri durdurmanız gerekir.

Birim, RunAsync için sağlanan herhangi bir iptal belirteçlerini testleri, test yürütme sırasında ChangeRoleAsync OpenAsync ve CloseAsync tutulmalıdır. Bu belirteçler üzerine tutan bir hizmet kapanması veya indirgeme benzetimini gerçekleştirmek ve hizmet yanıt uygun şekilde doğrulamak, test izin verir.

#### <a name="test-end-to-end-with-mocked-remote-resources"></a>Uçtan uca sahte uzak kaynaklar ile test
Birim testleri, mümkün olduğunca durum bilgisi olan hizmet durumunu değiştirebilir uygulamasını kodun çoğu olarak çalıştırılmalıdır. Testler daha-doğası gereği uca önerilir. Kayıt, benzetimini yapmak ve/veya uzak kaynak etkileşimleri doğrulamak için mevcut yalnızca mocks var. Bu durum Yöneticisi ve güvenilir koleksiyonlar ile etkileşim içerir. Aşağıdaki kod parçacığı için baştan sona test gösteren bir test gherkin örneğidir:

```
    Given stateful service named "fabric:/MyApp/MyService" is created
    And a new replica is created as "Primary" with id "111"
    And a new replica is created as "IdleSecondary" with id "222"
    And a new replica is created as "IdleSecondary" with id "333"
    And all idle secondary replicas are promoted to active secondary
    When a request is made to add the an employee "John Smith"
    And the active secondary replica "222" is promoted to primary
    And a request is made to get all employees
    Then the request should should return the "John Smith" employee
```

Bu test, bir çoğaltma üzerinde Yakalanan veriler birincil siteye yükseltildiğinde ikincil bir çoğaltmaya kullanılabilir olduğunu onaylar. Güvenilir koleksiyon çalışan verilerini yedekleme deposu olduğu varsayılarak, bu test ile yakalanan Aa olası uygulama kodu çalışmadı varsa hatasıdır `CommitAsync` hareketi yeni çalışan kaydetmek için. Bu durumda, ikinci istek çalışanlar çalışan ilk istek tarafından eklenen döndürecekti değil.

### <a name="acting"></a>Hareket
#### <a name="mimic-service-fabric-replica-orchestration"></a>Service Fabric çoğaltma düzenleme ekranı
Birden çok hizmeti örneği yönetirken, testleri başlatmak ve bu hizmetleri, Service Fabric düzenleme aynı şekilde ayırma. Örneğin, yeni birincil Çoğaltmada bir hizmeti oluşturulduğunda Service Fabric CreateServiceReplicaListener, OpenAsync, ChangeRoleAsync ve RunAsync çağırır. Yaşam döngüsü olayları Aşağıdaki makaleler de belgelenmiştir:

- [Durum bilgisi olan hizmet başlatma](service-fabric-reliable-services-lifecycle.md#stateful-service-startup)
- [Durum bilgisi olan hizmet kapanması](service-fabric-reliable-services-lifecycle.md#stateful-service-shutdown)
- [Durum bilgisi olan hizmet birincil Takasları](service-fabric-reliable-services-lifecycle.md#stateful-service-primary-swaps)

#### <a name="run-replica-role-changes"></a>Çoğaltma rolü değişiklikleri çalıştırın
Birim testlerini rolleri hizmet örneklerinin Service Fabric orchestration ile aynı şekilde değiştirmeniz gerekir. Rol Durum makinesi şu makalede belgelenmiştir:

[Çoğaltma rolü Durum makinesi](service-fabric-concepts-replica-lifecycle.md#replica-role)

Rol değişiklikleri benzetimi test daha önemli özelliklerinden biridir ve yineleme durumu olmadığı birbiriyle tutarlı sorunları ortaya çıkarabilirsiniz. Statik bellek içi durumda veya sınıf düzeyi örneği değişkenleri depolama nedeniyle tutarsız çoğaltma durumu ortaya çıkabilir. Bu örnekler, iptal belirteçlerini, numaralandırmalar ve yapılandırma nesneleri/değerleri olabilir. Bu da hizmeti rolü değişikliğin gerçekleşmesi RunAsync sırasında sağlanan iptal belirteçlerini saygı göstermek garanti eder. Rol değişiklikleri benzetimi RunAsync çağrısından birden çok kez izin vermek için kod yazılmadığında oluşabilecek sorunları ortaya çıkarabilirsiniz.

#### <a name="cancel-cancellation-tokens"></a>İptal belirteçleri iptal et
Burada sağlanan RunAsync için iptal belirteci iptal birim testleri bulunmalıdır. Bu hizmet düzgün bir şekilde kapanıyorsa öyle kapanır olduğunu doğrulamak test olanak tanır. Uzun çalışan veya zaman uyumsuz işlemleri Bu kapatma sırasında durdurulması gerekir. Bir hizmette oluşabilecek bir uzun süre çalışan işlem örneği güvenilir kuyruktaki iletileri dinler biridir. Bu, doğrudan RunAsync veya arka plan iş parçacığı içinde bulunabilir. Uygulama, bu iptal belirteci iptal edilirse işlemi çıkmadan mantığı eklemeniz gerekir.

Yalnızca birincil bulunmalı veya bellek içi önbelleğe durumu Hizmetleri durum bilgisi olan kullanırsanız, şu anda atılmalıdır. Bu durum, daha sonra tekrar birincil düğüm hale gelirse, tutarlı olmasını sağlamak için budur. Testi iptal bu durumu düzgün bir şekilde elden doğrulamak sınama izin verir.

#### <a name="execute-requests-against-multiple-replicas"></a>Birden çok çoğaltma istekler yürütün
Assert aynı istekte farklı yinelemenin karşı testler çalıştırılacak. Rol değişikliklerle birlikte kullanıldığında, tutarlılık sorunları yazdığı ortaya çıkarıldı olabilir. Örnek test, aşağıdaki adımları gerçekleştirebilir:
1. Geçerli birincil karşı bir yazma isteği yürütün
2. 1. adım geçerli birincil karşı yazılmış verileri döndüren Okuma isteği yürütün
3. Birincil ikincil tanıtın. Bu da geçerli birincil ikincil siteye indirgemeniz gerekir
4. 2. adımda yeni ikincil karşı aynı Okuma isteği yürütün.

Son adımda, döndürülen verilerin tutarlı test onaylama işlemi. Bu ortaya çıkarmaya olası bir sorunu hizmet tarafından döndürülen verileri güvenilir bir koleksiyona göre sonuçta yedeklenmiş ancak bellekte olmasıdır. Bellek içi verilerin olmayabilir ne güvenilir koleksiyonda düzgün mevcut ile eşitlenmiş durumda tutulmasını.

Bellek içi verileri, genellikle ikincil dizinler veya var olan veri toplama güvenilir bir koleksiyon oluşturmak için kullanılır.

### <a name="asserting"></a>Sunduğundan
#### <a name="ensure-responses-match-across-replicas"></a>Yanıtları genelinde çoğaltma eşleştiğinden emin olun
Birim testleri, bunlar birincil siteden geçiş sonra belirli bir istek için yanıt genelinde birden çok çoğaltma tutarlı olduğunu onay. Bu, burada yanıtta sağlanan verileri değil güvenilir bir koleksiyon tarafından desteklenen, veya bellek içi tutulan olası sorunları ortaya çıkabilir genelinde çoğaltma verileri eşitlemek için bir mekanizma olmadan. Bu, Service Fabric yeniden dengeler veya yeni bir birincil çoğaltmaya yük devreder sonra hizmeti yeniden tutarlı yanıtları gönderir garanti eder.

#### <a name="verify-service-respects-cancellation"></a>Hizmet gizliliğinize iptal doğrulayın
Bir iptal belirteci iptal edildiğinde sonlandırılmalıdır uzun süreli veya zaman uyumsuz işlemler, bunlar gerçekten iptal işleminden sonra sonlandırıldı doğrulanmalıdır. Bu, geçiş tamamlanmadan önce rollerini değiştirme çoğaltma rağmen birincil olmayan çoğaltma üzerinde çalışmaya devam etmesini amaçlanmayan işlemleri durdurun garanti eder. Bu, ayrıca burada bu tür bir işlem tamamlamanızı rol kapatma ya da değişiklik isteği Service fabric'teki engeller sorunları ortaya çıkarabilirsiniz.

#### <a name="verify-which-replicas-should-serve-requests"></a>Hangi çoğaltmalar isteklere hizmet doğrulayın.
Testleri, bir istek olmayan birincil çoğaltmaya yönlendirilir, Beklenen davranış onay. Service Fabric isteklere hizmet ikincil çoğaltma olanağı sunar. Bununla birlikte, güvenilir koleksiyonlar için yazma işlemleri yalnızca birincil çoğaltmadan ortaya çıkabilir. Uygulamanız yalnızca birincil çoğaltmalara isteklerine hizmet vermeye düşünüyor veya bir ikincil istekleri yalnızca bir alt işlenebilir erişemiyorsanız, testleri pozitif ve negatif çalışmalarını yönelik beklenen davranışın onay. Bir istek negatif durum istek ve pozitif işlememesi bir çoğaltmaya yönlendirilir tersi oluşturuluyor.

## <a name="next-steps"></a>Sonraki adımlar
Bilgi edinmek için nasıl [birim test durum bilgisi olan hizmetler](service-fabric-how-to-unit-test-stateful-services.md).
