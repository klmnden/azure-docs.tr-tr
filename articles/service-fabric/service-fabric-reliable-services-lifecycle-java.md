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
ms.openlocfilehash: 80eb68346dd05c256c60725eb082aa0651fe7cbd
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2017
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

## <a name="notes-on-service-lifecycle"></a>Hizmet yaşam döngüsü ile ilgili notlar
* Her iki `runAsync()` yöntemi ve `createServiceInstanceListeners` çağrıları isteğe bağlıdır. Bir hizmeti bunları, her ikisini birden veya hiçbiri olabilir. Örneğin, hizmet yanıt kullanıcı çağrıları olarak tüm çalışmasını varsa, gerek yoktur, uygulamak için `runAsync()`. İletişim dinleyicileri ve bunların ilişkili kodu gereklidir. Hizmet yalnızca yapmak için arka plan çalışması ve uygulamak için bu nedenle yalnızca gereksinimlerine sahip olabilir benzer şekilde, oluşturma ve iletişim dinleyicileri döndürme isteğe bağlı, aynıdır`runAsync()`
* Tamamlamak bir hizmet için geçerli `runAsync()` başarıyla ve dönüş almaktır. Bu bir hata durumu olarak kabul edilmez ve hizmet tamamlama arka plan iş temsil eder. Durum bilgisi olan güvenilir hizmetler için `runAsync()` hizmet birincil sunucudan indirgenir ve geri birincil yükseltilmiş yeniden çağrılması.
* Gelen bir hizmet bulunup bulunmadığını `runAsync()` bazı beklenmeyen bir özel durum atma tarafından bu bir hatadır ve hizmet nesnesi kapatılır ve bir sistem durumu hatası bildirdi.
* Varken hiçbir zaman sınırı bu yöntemlerden döndürme, hemen yazmak için yönetemez ve bu nedenle herhangi bir gerçek iş tamamlayamıyor. İptal isteği aldığında gerçekleştireceği mümkün olduğunca hızlı bir şekilde olarak dönüş önerilir. Hizmetinizi makul bir sürede API çağrıları, yanıt vermiyorsa Service Fabric hizmeti zorla sonlandırabilirsiniz. Genellikle yalnızca uygulama yükseltmelerini ya da hizmet zaman siliniyor sırasında meydana gelir. Bu zaman aşımı, varsayılan değer 15 dakikadır.
* Başarısızlık `onCloseAsync()` yolu sonucunda `onAbort()` hizmetinin temizlemek bir son fırsat en yüksek çaba fırsat olduğu çağrılan ayarlama ve talep tüm kaynakları serbest bırakmak.

> [!NOTE]
> Durum bilgisi olan güvenilir hizmetler Java'da henüz desteklenmiyor.
>
>

## <a name="next-steps"></a>Sonraki adımlar
* [Güvenilir hizmetlerine giriş](service-fabric-reliable-services-introduction.md)
* [Güvenilir hizmetler hızlı başlangıç](service-fabric-reliable-services-quick-start.md)
* [Kullanım Gelişmiş güvenilir hizmetler](service-fabric-reliable-services-advanced-usage.md)
