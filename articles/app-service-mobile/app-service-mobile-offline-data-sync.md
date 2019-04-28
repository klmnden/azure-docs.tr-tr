---
title: Azure Mobile apps'te çevrimdışı veri eşitleme | Microsoft Docs
description: Kavramsal başvurusu ve Azure Mobile Apps için çevrimdışı veri eşitleme özelliği genel bakış
documentationcenter: windows
author: conceptdev
manager: crdun
editor: ''
services: app-service\mobile
ms.assetid: 982fb683-8884-40da-96e6-77eeca2500e3
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/30/2016
ms.author: crdun
ms.openlocfilehash: ab8fb4a567e4c4a7bf1e884999a4e403a98547a0
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62128024"
---
# <a name="offline-data-sync-in-azure-mobile-apps"></a>Azure Mobile Apps’te Çevrimdışı Veri Eşitleme
## <a name="what-is-offline-data-sync"></a>Çevrimdışı veri eşitleme nedir?
Çevrimdışı veri eşitleme, bir istemci ve sunucu bir ağ bağlantısı olmadan çalışabilen uygulamalar oluşturmak geliştiriciler için kolaylaştıran Azure Mobile Apps SDK'sı özelliği ' dir.

Uygulamanızı çevrimdışı modda olduğunda, yine de oluşturabilir ve yerel bir depoya kaydedilen veri değiştirebilirsiniz. Uygulama tekrar çevrimiçi olduğunda yerel değişiklikleri, Azure mobil uygulama arka ucu ile eşitleyebilirsiniz. Özellik ayrıca aynı kaydın hem istemcide hem de arka uç değiştirildiğinde çıkabilecek çakışmaların saptanmasını desteği içerir. Çakışmalar, daha sonra sunucu veya istemci işlenebilir.

Çevrimdışı eşitleme, birçok faydası vardır:

* Sunucu verilerini yerel olarak cihazda önbelleğe alarak uygulama yanıt verme hızını artırın
* Ağ sorunları güçlü uygulamalar oluşturun
* Son kullanıcıların çok az kayıpla veya hiç bağlantısı ile senaryolarını destekleyen hiçbir ağ erişimi olduğunda bile veri oluşturup izin ver
* Cihazlar arasında veri eşitleyin ve çakışmaları aynı kaydın iki cihazlar tarafından değiştirildiğinde Algıla
* Yüksek gecikmeli veya tarifeli ağlar üzerindeki ağ kullanımını sınırla

Aşağıdaki öğretiler Azure Mobile Apps'i kullanarak mobil istemcilerinize çevrimdışı eşitleme ekleme göster:

* [Android: Çevrimdışı eşitlemeyi etkinleştirme]
* [Apache Cordova: Çevrimdışı eşitlemeyi etkinleştirme](app-service-mobile-cordova-get-started-offline-data.md)
* [iOS: Çevrimdışı eşitlemeyi etkinleştirme]
* [Xamarin iOS: Çevrimdışı eşitlemeyi etkinleştirme]
* [Xamarin Android: Çevrimdışı eşitlemeyi etkinleştirme]
* [Xamarin.Forms: Çevrimdışı eşitlemeyi etkinleştirme](app-service-mobile-xamarin-forms-get-started-offline-data.md)
* [Evrensel Windows Platformu: Çevrimdışı eşitlemeyi etkinleştirme]

## <a name="what-is-a-sync-table"></a>Tablosunu eşitleme nedir?
"/ Tablolar" uç noktasına erişmek için Azure mobil istemci SDK'ları sağlayın arabirimleri gibi `IMobileServiceTable` (.NET İstemci SDK'sı) veya `MSTable` (iOS istemci). Bu API'ler, doğrudan Azure mobil uygulama arka ucuna bağlama ve istemci cihaza bir ağ bağlantısı olmaması durumunda başarısız.

Çevrimdışı kullanım desteklemek için uygulamanızı kullanmalısınız *tablosunu eşitleme* API'leri gibi `IMobileServiceSyncTable` (.NET İstemci SDK'sı) veya `MSSyncTable` (iOS istemci). Tümü aynı CRUD işlemleri (oluşturma, okuma, güncelleştirme, silme) karşı eşitleme iş tablo API'leri, okuma, artık dışındaki veya yazma bir *yerel depo*. Yerel depo, herhangi bir eşitleme tablo işlemi gerçekleştirilmeden önce başlatılmalıdır.

## <a name="what-is-a-local-store"></a>Yerel bir depo nedir?
Veri saklama katmanını istemci cihazdaki bir yerel deposudur. Azure Mobile Apps istemci SDK'ları varsayılan yerel mağaza uygulamasını belirtin. Windows, Xamarin ve Android üzerinde SQLite üzerinde temel alır. İOS, temel veri temel alır.

Windows Phone veya Microsoft Store SQLite tabanlı bir uygulama kullanmak için bir SQLite uzantısını yüklemeniz gerekir. Daha fazla bilgi için [Evrensel Windows Platformu: Çevrimdışı eşitlemeyi etkinleştirme]. Kendi sürümünüzü SQLite başvurmak gerekli değildir sürümünde cihazın işletim sistemi kendisini SQLite ile android ve iOS gönderin.

Geliştiriciler Ayrıca kendi yerel depo uygulayabilirsiniz. Örneğin, verileri mobil istemci üzerinde şifrelenmiş biçimde depolamak istiyorsanız, şifreleme için SQLCipher kullanan yerel bir depo tanımlayabilirsiniz.

## <a name="what-is-a-sync-context"></a>Bir eşitleme bağlamı nedir?
A *eşitleme bağlamı* bir mobil istemci nesnesi ile ilişkili (gibi `IMobileServiceClient` veya `MSClient`) ve eşitleme tablolarla yapılan değişiklikleri izler. Eşitleme bağlamı tutan bir *işlem sırası*, CUD işlemleri (oluşturma, güncelleştirme, silme) sıralı bir listesi, hangi tutar daha sonra sunucuya gönderilir.

Yerel bir depo gibi Initialize yöntemi kullanarak eşitleme bağlamı ile ilişkili `IMobileServicesSyncContext.InitializeAsync(localstore)` içinde [.NET İstemci SDK'sı].

## <a name="how-sync-works"></a>Eşitleme çevrimdışı nasıl çalışır
Eşitleme tablolar kullanırken, istemci kodunuz ile bir Azure mobil uygulaması arka ucunu yerel değişiklikler olduğunda eşitlenir denetler. Çağrısı oluncaya kadar hiçbir şey arka ucuna gönderilen *anında iletme* yerel değişiklikleri. Benzer şekilde, yalnızca bir çağrı olduğunda yerel depo yeni verilerle doldurulur *çekme* veri.

* **Anında iletme**: Anında iletme eşitleme bağlamı bir işlem ve son itme itibaren tüm CUD değişiklikleri gönderir. Aksi halde işlem sıralamaya gönderilebilir yalnızca tek bir tablonun değişiklikleri göndermek mümkün değildir, çünkü olduğunu unutmayın. Anında iletme, bir dizi sırayla server veritabanınızı değiştirir, Azure mobil uygulama arka ucu için REST çağrısı yürütür.
* **Çekme**: Çekme tablo başına temelinde gerçekleştirilir ve yalnızca bir alt kümesini sunucu verilerini almak için bir sorgu ile özelleştirilebilir. Azure mobil istemci SDK'ları, elde edilen veriler daha sonra yerel depoya ekleyin.
* **Örtük bildirim**: Bir çekme bekleyen yerel güncelleştirmeleri içeren bir tabloda karşı yürütülürse, çekme yürütür bir `push()` üzerinde eşitleme bağlamı. Bu gönderme, sıraya alınan değişiklikleri ve yeni verileri sunucudan arasındaki çakışmaları en aza yardımcı olur.
* **Artımlı eşitleme**: çekme işlemi için ilk parametre bir *sorgu adı* yalnızca istemci üzerinde kullanılır. Bir null olmayan sorgu adı kullanırsanız, Azure Mobile SDK'sı gerçekleştiren bir *Artımlı eşitleme*. Her çekme işlemi, bir en son sonuç kümesini döndürür `updatedAt` zaman damgası, sonuç kümesinden SDK yerel sistem tablolarında depolanır. Sonraki çekme işlemleri, bu zaman damgasından sonraki yalnızca kayıtları alın.

  Artımlı eşitleme kullanmak üzere sunucunuzu anlamlı döndürmelidir `updatedAt` değerleri ve bu alana göre sıralamayı desteklemesi gerekir. Ancak, SDK'sı kendi sıralama updatedAt alanı ekler. bu yana kendisine ait olan bir çekme sorgu kullanamazsınız `orderBy` yan tümcesi.

  Sorgu adı, seçtiğiniz herhangi bir dize olabilir, ancak uygulamanıza mantıksal her sorgu için benzersiz olmalıdır.
  Aksi takdirde, farklı çekme işlemleri aynı Artımlı eşitleme zaman damgası üzerine yazabilir ve sorgularınızı hatalı sonuçlar döndürebilir.

  Sorgu parametresi varsa, bir benzersiz sorgu adı oluşturmak için bir parametre değeri birleştirmek için yoludur.
  Kullanıcı kimliğine göre uyguladıysanız, örneğin, sorgu adınızı gibi (C# ') olabilir:

        await todoTable.PullAsync("todoItems" + userid,
            syncTable.Where(u => u.UserId == userid));

  Artımlı eşitleme dışında iyileştirilmiş istiyorsanız `null` olarak sorgu kimliği Bu durumda, her çağrıda tüm kayıtlar alınır `PullAsync`, büyük olasılıkla yetersiz olduğu.
* **Temizleme**: Yerel depoyu kullanarak içerikleri temizleyebilirsiniz `IMobileServiceSyncTable.PurgeAsync`.
  Temizleme, istemci veritabanında eski veri varsa veya bekleyen tüm değişiklikleri atmak isterseniz gerekli olabilir.

  Yerel depodan bir tablo bir temizleme temizler. Sunucu veritabanı ile eşitlemeyi bekleyen işlemler varsa, temizleme sürece aykırı *temizleme zorla* parametrenin ayarlanmış.

  Bir istemci eski veri örneği olarak, "Yapılacaklar listesi" örnekte cihaz1 tamamlanmamış öğeleri yalnızca çeker varsayalım. Sunucu üzerinde başka bir cihaz tarafından tamamlandı "sütlü satın al" olarak işaretlenmiş bir todoıtem. Ancak, yalnızca tam olarak işaretlenmez öğeleri çekme çünkü cihaz1 hala "Satın Al sütlü" todoıtem yerel depoya sahiptir. Bu eski öğesi bir temizleme temizler.

## <a name="next-steps"></a>Sonraki adımlar
* [iOS: Çevrimdışı eşitlemeyi etkinleştirme]
* [Xamarin iOS: Çevrimdışı eşitlemeyi etkinleştirme]
* [Xamarin Android: Çevrimdışı eşitlemeyi etkinleştirme]
* [Evrensel Windows Platformu: Çevrimdışı eşitlemeyi etkinleştirme]

<!-- Links -->
[.NET İstemci SDK'sı]: app-service-mobile-dotnet-how-to-use-client-library.md
[Android: Çevrimdışı eşitlemeyi etkinleştirme]: app-service-mobile-android-get-started-offline-data.md
[iOS: Çevrimdışı eşitlemeyi etkinleştirme]: app-service-mobile-ios-get-started-offline-data.md
[Xamarin iOS: Çevrimdışı eşitlemeyi etkinleştirme]: app-service-mobile-xamarin-ios-get-started-offline-data.md
[Xamarin Android: Çevrimdışı eşitlemeyi etkinleştirme]: app-service-mobile-xamarin-android-get-started-offline-data.md
[Evrensel Windows Platformu: Çevrimdışı eşitlemeyi etkinleştirme]: app-service-mobile-windows-store-dotnet-get-started-offline-data.md
