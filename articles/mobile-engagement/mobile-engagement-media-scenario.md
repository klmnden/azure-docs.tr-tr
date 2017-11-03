---
title: "Medya uygulaması için Azure Mobile Engagement uygulaması"
description: "Azure Mobile Engagement uygulamak için medya uygulaması senaryosu"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 48201cc8-4e04-485c-a8dc-d6406d23f3ed
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: c1591c3e436981e621830916cf0cdc4b7f395d7b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="implement-mobile-engagement-with-media-app"></a>Medya uygulaması ile Mobile Engagement uygulama
## <a name="overview"></a>Genel Bakış
John, büyük media şirket için bir mobil proje yöneticisidir. Daha kısa bir süre önce bir çok yüksek indirme sayısı sahip yeni bir uygulama başlattı. Kendisine indirme için kendi hedefleri erişti ancak hala kendi dönüş üzerinde Investment(ROI) kullanıcı başına kendi gereksinimlerini karşılamıyor. 

John, kendi ROI çok düşük olmasının zaten belirledi. Yalnızca 2 hafta sonra uygulamayı kullanarak kullanıcıların sık durdurun ve bunların çoğu asla geri dönün. Kendi uygulama bekletme artırmak istediği.

Bazı test ilk sonra kendisinin kullanıcıları anında iletme bildirimleri ile girdiğinde kendisinin öğrenilen oldukları kendi uygulamayı kullanmaya devam etmek için eğilimi gösterir. Ayrıca etkin olmayan kullanıcılar genellikle uygulamanın kendisinin gönderir bildirimleri bağlı olarak döndürür. John, katılım programının Gelişmiş hedefleme ile anında iletme bildirimleri kullanan kendi uygulama için bir tür yatırım karar verir.

John son okuyun [Azure Mobile Engagement - en iyi yöntemlerle Başlangıç Kılavuzu](mobile-engagement-getting-started-best-practices.md) ve Kılavuzu önerileri uygulamak karar vermiştir.

## <a name="objectives-and-kpis"></a>Hedefleri ve KPI'leri
Önemli katılımcılara Can'ın uygulama için karşılar. Kullanıcılar kendi medya kullanma gibi iş reklamları'ndan oluşturulur. Kullanıcı başına tüketilen içerik artırarak John kendi gelirleri artırır. Tüm bir ana hedefte kabul etmiş olursunuz: % 25 ads satış artırmak için. Bunlar iş anahtar performans göstergelerini (KPI'lar) ölçü ve sürücü bu amaç oluşturun

* Kullanıcı başına tıklanan reklam sayısı
* Kaç tane makale ziyaret edilen sayfalar (kullanıcı başına / oturumu başına / haftada / aylık...)
* Sık kullanılan kategorileri nelerdir

Önemli katılımcılara Can'ın Toplantı göre kendisine kendi iş KPI'leri tanımlamıştır. Kendisine Kısım 1 / izleyen [Azure Mobile Engagement - en iyi yöntemlerle Başlangıç Kılavuzu](mobile-engagement-getting-started-best-practices.md). 

Ardından, hedefleri ulaşıldığında emin olmak için aşağıdaki katılım KPI'leri oluşturur:

* Şu aralıklar bekletme izlemek: günlük, haftalık, iki haftalık ve aylık.
* Etkin kullanıcı sayısı
* Uygulama derecelendirme uygulamasında depolar

BT Ekibi önerilerine dayalı olarak, aşağıdaki teknik KPI'ları, aşağıdaki soruları yanıtlamak için eklendi:

* My kullanıcı yolu nedir (hangi sayfasını ziyaret, kaç kullanıcının harcamanız üzerinde)
* Kilitlenmelerine veya oturum başına karşılaşılan hataların sayısı?
* İşletim sistemi sürümleri çalıştıran Kullanıcılarım nelerdir?
* Kullanıcılarım için ekran ortalama boyutu nedir?
* Ne tür bir internet bağlantıları Kullanıcılarım var mı?

Her KPI için kendisine gerekli verileri sınıflandırır ve kendisine onun playbook doğru konumda kaydeder.

## <a name="engagement-program-and-integration"></a>Katılım programı ve tümleştirme
Bu John kendi KPI'leri tanımlamaya bitirdi artık, kendisinin kendi katılım stratejisi Aşama 4 katılım programları ve bunların hedefler tanımlayarak başlar:![][1]

Sonra John anında iletme bildirimleri her program için ayrıntılı tarafından daha derin gider. Anında iletme bildirimi beş öğeleri tarafından tanımlanır:

1. Hedef: bildirim amacı nedir
2. Amaç ulaşıldı nasıl
3. Hedef: kimin bildirim alırsınız?
4. İçeriği: İfade ve (içinde App/Out, uygulama) bildirim biçimi nedir
5. Ne zaman: Bu anında iletme bildirimi göndermek için en iyi dakikanızı nedir
   
    ![][2]

Daha fazla bilgi için bkz [Playbooks](https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks).

Bölümü göre incelemeyi John 2 hedef bölüm toplamak için sahip hangi verileri tanımlamak için kullanır ve kendi etiket çözümü uygulamak için planlama BT Ekibi ile ortaklaşa yazar. Uygulama ve kullanıcı kabul testi 1 hafta sonra John son olarak, programlar başlatabilirsiniz.

## <a name="program-results"></a>Program sonuçları
4 ay sonra John programlar performansını inceler. Karşılama programı ve haftalık programı kendi hedefleri ulaşmanızı. Yalnızca bir oturum ile kullanıcı sayısını azaltır, haftada bağlantı sayısı iki katına ve uygulamanın daha fazla özellik kullanılır.

**Etkin olmayan Program** John kullanıcı tendencies anlamanıza yardımcı olur. % 15'etkin olmayan kullanıcıların bu uygulamaya geri dönün görünür. Ancak bunların çoğu birden fazla 1 ay etkin kalmaz. John bu sırası ek bildirimler ve kendi içerik seçimleri genişletme ile olası iyileştirmesi foresees.

**Bul Program** düzgün çalışmıyor. Bu, yeterli ancak kendi hedeflere ulaşmayı çapraz satış artırır. John tanımlayan kendisinin ilgili yapmak için yeterli veri yok hedefleme ve uygun içeriğin önerin. Kendisi bu programı durdurur ve Azure Mobile Engagement ile "düzenleme anında iletme bildirimleri" gönderme odaklanır. Kendi gazeteciler anında iletme bildirimleri göndermek için bir CMS Çözüm zaten var ve değiştirmek istemezsiniz.

John AZME Web arabirimi kullanmak zorunda kalmadan Reach kampanyaları yönetme izin veren bir HTTP REST API'si olan Reach API'sini kullanmaya karar verir. Bu yaklaşımda John kendisinin gerekir ve CMS çözüm kullanmaya devam etmek kendi yazıcılarının izin verileri toplayabilir.

Bu özellik doğru çalıştığından emin olmak için aşağıdaki noktalarında temkinli olması için BT Ekibi John ister:

1. **İşletim sistemi** : hepsi John tüm çalışmalarını Listele karar verir ve API'leri, işleme, denetler anında iletme bildirimleri yönetmek için kendi kurallarına sahiptir.
   Örneğin: İOS durumuyla olmayan büyük resmi Android anında iletme sistem izin verir.
2. **Zaman dilimi**: John istediği zaman dilimi ve bir uç kampanyalar için ayarlanmış bir API. Herhangi bir kesintiye uğratan bildirim bombing kullanıcıların korumak istediği.
3. **Kategoriler**: Pazarlama Takım şablon her uyarı türü için hazırlar. John API içinde kategorilerini ayarlamak için BT Ekibi sorar.

Bazı testleri sonra John memnun kalır. Bu API sayesinde gazeteciler kendi CMS ve Azure Mobile Engagement ile anında iletme bildirimleri için tüm davranış verileri toplar göndermeye devam edebilir

Bu 4 sonra ilk ay, sonuçları iyi bir genel performans ve verir güvenirlik John ve kendi Panosu, kullanıcı artar % 15 başına Başına ROI yansıtacak ve Mobil Satış %7.5 artışı yalnızca dört ay içinde % 17,5 toplam satış temsil eder.

<!--Image references-->
[1]: ./media/mobile-engagement-media-scenario/engagement-strategy.png
[2]: ./media/mobile-engagement-media-scenario/push-scenarios.png

<!--Link references-->
[Media Playbook link]: https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks
