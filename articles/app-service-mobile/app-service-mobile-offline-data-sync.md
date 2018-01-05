---
title: "Çevrimdışı veri eşitlemeye Azure mobil uygulamalarda | Microsoft Docs"
description: "Kavramsal başvurusu ve Azure Mobile Apps için çevrimdışı veri eşitlemeyi özelliğine genel bakış"
documentationcenter: windows
author: conceptdev
manager: crdun
editor: 
services: app-service\mobile
ms.assetid: 982fb683-8884-40da-96e6-77eeca2500e3
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/30/2016
ms.author: crdun
ms.openlocfilehash: 5ea1d655f50da49be88f7b6ae91231c4d2258fa7
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2018
---
# <a name="offline-data-sync-in-azure-mobile-apps"></a>Azure Mobile Apps’te Çevrimdışı Veri Eşitleme
## <a name="what-is-offline-data-sync"></a>Çevrimdışı veri eşitlemeye nedir?
Çevrimdışı veri eşitlemeye istemci ve sunucu bir ağ bağlantısı işlev uygulamaları oluşturmak geliştiriciler için kolaylaştıran Azure Mobile Apps SDK özelliği ' dir.

Uygulamanızı çevrimdışı modda olduğunda, hala oluşturun ve bir yerel deposuna kaydedilmeden verileri değiştirebilir. Uygulama yeniden çevrimiçi olduğunda, yerel değişiklikler, Azure mobil uygulama arka ucu ile eşitleyebilirsiniz. Özellik de aynı kayıt hem istemci hem de arka uç değiştirildiğinde çakışmaları saptamak için destek içerir. Çakışmalar, daha sonra sunucu veya istemci işlenebilir.

Çevrimdışı eşitleme birkaç avantaj vardır:

* Sunucu verilerini cihazda yerel olarak önbelleğe alarak uygulama yanıt hızını artırmak
* Ağ sorunları olduğunda yararlı kalır sağlam uygulamalar oluşturma
* Son kullanıcılar oluşturmak ve çok az kayıpla veya hiç bağlanabilirlik senaryolarını destekleyen hiçbir ağ erişimi olduğunda bile veri değiştirmek izin ver
* Birçok cihaz arasında veri eşitlemek ve aynı kayıt iki cihazlar tarafından değiştirildiğinde çakışmalarını Algıla
* Yüksek gecikmeli veya ölçülen ağlarda ağ kullanımını sınırla

Aşağıdaki öğreticiler nasıl Azure Mobile Apps kullanarak mobil istemcileriniz çevrimdışı eşitleme ekleneceğini gösterir:

* [Android: Çevrimdışı eşitleme etkinleştir]
* [Apache Cordova: Çevrimdışı eşitleme etkinleştir](app-service-mobile-cordova-get-started-offline-data.md)
* [iOS: Çevrimdışı eşitleme etkinleştir]
* [Xamarin iOS: Çevrimdışı eşitleme etkinleştir]
* [Xamarin Android: Çevrimdışı eşitleme etkinleştir]
* [Xamarin.Forms: Etkinleştir çevrimdışı eşitleme](app-service-mobile-xamarin-forms-get-started-offline-data.md)
* [Evrensel Windows Platformu: etkinleştirmek çevrimdışı eşitleme]

## <a name="what-is-a-sync-table"></a>Bir eşitleme tablo nedir?
"/ Tabloları" uç noktasına erişmek için Azure Mobile istemci SDK'ları sağlaması arabirimleri gibi `IMobileServiceTable` (.NET İstemci SDK) veya `MSTable` (iOS istemci). Bu API'ları doğrudan Azure mobil uygulama arka ucuna bağlama ve istemci aygıt bir ağ bağlantısı yoksa başarısız.

Çevrimdışı kullanım desteklemek için uygulamanızı kullanmalısınız *eşitleme tablo* API'leri gibi `IMobileServiceSyncTable` (.NET İstemci SDK) veya `MSSyncTable` (iOS istemci). Tümü aynı CRUD işlemleri (oluşturma, okuma, güncelleştirme, silme) karşı eşitleme iş API'leri tablo, bunlar okuma artık dışındaki veya yazma bir *yerel deposu*. Yerel depodan eşitleme tablo işlemleri gerçekleştirilmeden önce başlatılması gerekir.

## <a name="what-is-a-local-store"></a>Yerel bir depo nedir?
Veri saklama katmanını istemci cihazda bir yerel deposudur. Azure Mobile Apps istemci SDK'ları varsayılan yerel mağaza uygulamasını belirtin. Windows, Xamarin ve Android, SQLite üzerinde temel alır. İos'ta, temel verilere dayanır.

Windows Phone veya Windows mağazası 8.1 SQLite tabanlı bir uygulama kullanmak için bir SQLite uzantısını yüklemeniz gerekir. Daha fazla bilgi için bkz: [Evrensel Windows Platformu: etkinleştirmek çevrimdışı eşitleme]. SQLite kendi sürümü başvurmak gerekli değildir SQLite sürümü aygıt işletim sisteminde kendisi, android ve iOS birlikte.

Geliştiriciler Ayrıca kendi yerel deposu uygulayabilirsiniz. Örneğin, şifrelenmiş biçimde mobil istemci üzerindeki veri depolamak istiyorsanız, şifreleme için SQLCipher kullanan yerel bir depo tanımlayabilirsiniz.

## <a name="what-is-a-sync-context"></a>Eşitleme bağlamı nedir?
A *eşitleme bağlamı* bir mobil istemci nesneyle ilişkilendirilen (gibi `IMobileServiceClient` veya `MSClient`) ve eşitleme tablolarla yapılan değişiklikleri izler. Eşitleme bağlamı tutar bir *işlem sırası*, hangi sonraki CUD işlemleri (oluşturma, güncelleştirme, silme) sıralı listesini tutar sunucusuna gönderilemez.

Yerel bir depo Initialize yöntemini kullanarak eşitleme bağlamla ilişkilendirilen `IMobileServicesSyncContext.InitializeAsync(localstore)` içinde [.NET İstemci SDK].

## <a name="how-sync-works"></a>Eşitleme çevrimdışı nasıl çalışır
Eşitleme tabloları kullanırken, istemci kodunuzun bir Azure mobil uygulama arka ucu ile yerel değişiklikler olduğunda eşitlenir denetler. Çağrı kadar hiçbir şey arka ucuna gönderilen *itme* yerel değişiklikler. Benzer şekilde, yalnızca bir çağrı olduğunda yerel deposu yeni verilerle doldurulur *çekme* veri.

* **Anında iletme**: itme eşitleme bağlamı üzerinde bir işlemi ve bu yana son zorlama tüm CUD değişiklikleri gönderir. Aksi takdirde işlemleri bozuk gönderilebilir yalnızca tek bir tablonun değişiklikleri göndermek mümkün değildir, çünkü olduğunu unutmayın. Anında iletme bir dizi sırayla server veritabanınıza değiştirir, Azure mobil uygulama arka ucu için REST çağrısı yürütür.
* **Çekme**: çekme bir tablo başına temelinde gerçekleştirilir ve yalnızca bir kısmı sunucu verilerini almak için bir sorgu ile özelleştirilebilir. Azure Mobile istemci SDK'ları sonuç verileri yerel deposuna sonra ekleyin.
* **Örtük iter**: çekme bekleyen yerel güncelleştirmeleri içeren bir tablo karşı yürütülürse, çekme ilk yürüten bir `push()` eşitleme bağlamı üzerinde. Bu itme zaten kuyruğa alınmış değişiklikleri ve yeni verileri sunucudan arasındaki çakışmaları en aza indirmenize yardımcı olur.
* **Artımlı eşitleme**: ilk parametre istek işlemi için bir *sorgu adı* yalnızca istemcide kullanılır. Null olmayan sorgu adı kullanırsanız, Azure Mobile SDK'sı gerçekleştiren bir *Artımlı eşitleme*. Her zaman bir çekme işlemi döndürür sonuçlarından, en son `updatedAt` zaman damgası, sonuç kümesinden SDK yerel sistem tablolarında depolanır. Sonraki çekme işlemleri yalnızca kayıtları sonra zaman damgası alın.

  Artımlı eşitleme kullanmak için sunucunuzu anlamlı döndürmelidir `updatedAt` değerleri ve bu alana göre sıralama desteklemesi gerekir. Ancak, SDK'sı kendi sıralama updatedAt alanı ekler olduğundan, kendi sahip bir çekme sorgu kullanamazsınız `orderBy` yan tümcesi.

  Sorgu adı, seçtiğiniz herhangi bir dize olabilir, ancak uygulamanızda mantıksal her sorgu için benzersiz olmalıdır.
  Aksi takdirde aynı Artımlı eşitleme zaman damgası farklı çekme işlemleri üzerine yazmak ve sorgularınızı hatalı sonuçlar döndürebilir.

  Sorgu parametresi varsa, benzersiz sorgu adı oluşturmak için bir parametre değeri eklemenizi yoludur.
  UserId üzerinde filtreleme, örneğin, sorgu adı gibi (C# ') aşağıdakilerden biri olabilir:

        await todoTable.PullAsync("todoItems" + userid,
            syncTable.Where(u => u.UserId == userid));

  Artımlı eşitleme dışında tercih istiyorsanız, geçirmek `null` sorgu kimliği olarak Bu durumda, tüm kayıtlar için her çağrıda alınır `PullAsync`, büyük olasılıkla yetersiz olduğu.
* **Temizleme**: yerel deposunu kullanan İçindekiler temizleyebilirsiniz `IMobileServiceSyncTable.PurgeAsync`.
  Temizleme, eski veri istemci veritabanında varsa veya bekleyen tüm değişiklikleri atmak istiyor gerekli olabilir.

  Bir temizleme yerel deposu tablosundan temizler. Sunucu veritabanı ile eşitlemeyi bekleyen işlemler varsa, temizleme sürece aykırı *temizleme zorla* parametrenin ayarlanmış.

  Eski istemci üzerindeki veri, bir örnek, Device1 yalnızca tamamlanmamış öğeleri çeker "Yapılacaklar listesi" örnekte varsayalım. Sunucu üzerinde başka bir aygıt tarafından tamamlandı "sütlü satın al" olarak işaretlenmiş bir todoıtem. Ancak, yalnızca tam olarak işaretlenmez öğeleri çekme çünkü Device1 hala "Satın Al sütlü" todoıtem yerel deposunda vardır. Bir temizleme eski bu öğeyi temizler.

## <a name="next-steps"></a>Sonraki adımlar
* [iOS: Çevrimdışı eşitleme etkinleştir]
* [Xamarin iOS: Çevrimdışı eşitleme etkinleştir]
* [Xamarin Android: Çevrimdışı eşitleme etkinleştir]
* [Evrensel Windows Platformu: etkinleştirmek çevrimdışı eşitleme]

<!-- Links -->
[.NET İstemci SDK]: app-service-mobile-dotnet-how-to-use-client-library.md
[Android: Çevrimdışı eşitleme etkinleştir]: app-service-mobile-android-get-started-offline-data.md
[iOS: Çevrimdışı eşitleme etkinleştir]: app-service-mobile-ios-get-started-offline-data.md
[Xamarin iOS: Çevrimdışı eşitleme etkinleştir]: app-service-mobile-xamarin-ios-get-started-offline-data.md
[Xamarin Android: Çevrimdışı eşitleme etkinleştir]: app-service-mobile-xamarin-android-get-started-offline-data.md
[Evrensel Windows Platformu: etkinleştirmek çevrimdışı eşitleme]: app-service-mobile-windows-store-dotnet-get-started-offline-data.md
