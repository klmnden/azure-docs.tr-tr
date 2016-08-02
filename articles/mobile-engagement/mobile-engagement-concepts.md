<properties
    pageTitle="Mobile Engagement kavramları | Microsoft Azure"
    description="Azure Mobile Engagement kavramları"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="02/26/2016"
    ms.author="piyushjo" />

# Azure Mobile Engagement kavramları

Mobile Engagement, desteklenen tüm platformlar için ortak olan birkaç kavram tanımlar. Bu makalede, söz konusu kavramlar kısaca açıklanmaktadır.

Mobile Engagement’la ilgilenmeye yeni başlayanlar için bu makale iyi bir başlangıç noktasıdır. Ayrıca kullandığınız platforma özgü belgeleri de okumaya özen gösterin. Bu belgeler, ek ayrıntılar ve örneklerin yanı sıra olası sınırlamaları sunarak bu makalede açıklanan kavramları netleştirecektir.

## Cihazlar ve kullanıcılar
Mobile Engagement, her cihaz için benzersiz bir tanımlayıcı oluşturarak kullanıcıları tanımlar. Bu tanımlayıcıya, cihaz tanımlayıcısı (veya `deviceid`) adı verilir. Bu tanımlayıcı, aynı cihazda çalışan tüm uygulamaların aynı cihaz tanımlayıcısına sahip olmalarını sağlayacak şekilde oluşturulur.

Buradan açığa çıkan, Mobile Engagement’ın bir cihazı tek bir kullanıcıya ait olarak düşündüğü, kullanıcı ve cihazı eşdeğer kavramlar şeklinde gördüğüdür.

## Oturumlar ve etkinlikler
Oturum, uygulamayı bir kullanıcının kullanmaya başlamasından durmasına kadar süren, uygulamanın kullanıcı tarafından gerçekleştirilen bir kullanımıdır.

Etkinlik, uygulamanın belirli bir alt parçasının bir kullanıcı tarafından gerçekleştirilen bir kullanımıdır (genellikle bir ekrandır, ancak uygulamaya uygun her şey olabilir).

Bir kullanıcı, bir seferde yalnızca bir etkinlik gerçekleştirebilir.

Etkinlik, 64 karakterle sınırlanan bir ad tarafından tanımlanır ve isteğe bağlı olarak bazı ek veriler (1024 bayt ile sınırlı) eklenebilir.

Oturumlar, kullanıcılar tarafından gerçekleştirilen etkinlikleri serisi kullanılarak otomatik olarak hesaplanır. Oturum, kullanıcı ilk etkinliğini başlattığında başlar ve son etkinliğini bitirdiğinde durur. Bu, oturumun açıkça başlatılması veya durdurulmasına gerek olmadığı anlamına gelir. Bunun yerine, etkinlikler açıkça başlatılır veya durdurulur. Hiçbir etkinlik bildirilmezse, hiçbir oturum da bildirilmez.

## Olaylar
Olaylar, anlık eylemleri (basılan düğme veya kullanıcılar tarafından okunan makaleler gibi) bildirmek için kullanılır.

Olay, geçerli oturum veya çalıştırılan bir iş ile ilgili olabilir ya da tek başına bir olay olabilir.

Olay, 64 karakterle sınırlanan bir ad tarafından tanımlanır ve isteğe bağlı olarak bazı ek veriler (1024 bayt ile sınırlı) eklenebilir.

## Hata
Hatalar, uygulama tarafından doğru bir şekilde algılanan sorunları (yanlış kullanıcı eylemleri veya API çağrısı hataları gibi) bildirmek için kullanılır.

Hata, geçerli oturum veya çalıştırılan bir iş ile ilgili olabilir ya da tek başına bir hata olabilir.

Hata, 64 karakterle sınırlanan bir ad tarafından tanımlanır ve isteğe bağlı olarak bazı ek veriler (1024 bayt ile sınırlı) eklenebilir.

## İş
İşler, bir süresi olan eylemleri (API çağrılarının süresi, reklamların gösterilme süresi, arka plan görevlerinin süresi veya kullanıcı eylemlerinin süresi gibi) bildirmek için kullanılır.

Görevler herhangi bir kullanıcı etkileşimi olmadan arka planda gerçekleştirilebildiğinden bir iş, bir oturum ile ilgili değildir.

İş, 64 karakterle sınırlanan bir ad tarafından tanımlanır ve isteğe bağlı olarak bazı ek veriler (1024 bayt ile sınırlı) eklenebilir.

## Kilitlenme
Kilitlenmeler, uygulama tarafından algılanmayan sorunların uygulamanın kilitlenmesine neden olduğu durumlarda uygulama hatalarını bildirmek için Mobile Engagement SDK’sı tarafından otomatik olarak düzenlenir.

## Uygulama bilgileri
Uygulama bilgileri, kullanıcıları etiketlemek, yani bazı verileri bir uygulamanın kullanıcılarıyla ilişkilendirmek için kullanılır (web tanımlama bilgilerine benzer, ancak uygulama bilgileri Azure Mobile Engagement platformunda sunucu tarafında depolanır).

Uygulama bilgileri, Mobile Engagement SDK API'si veya Mobile Engagement platformu Cihaz API'si kullanılarak kaydedilebilir.

Uygulama bilgileri, bir cihazla ilişkili olan bir anahtar/değer çiftidir. Anahtar, uygulama bilgilerinin adıdır (64 ASCII harfi [a-zA-Z], [0-9] rakamları ve alt çizgi [_] ile sınırlıdır). Değer (1024 karakterle sınırlı) herhangi bir dize, tamsayı, tarih (yyyy-AA-gg) ya da Boole değeri (true veya false) olabilir.

Mobile Engagement fiyatlandırma koşulları tarafından tanımlanan sınırların içerisinde olduğu sürece, herhangi bir sayıda uygulama bilgisi bir cihazla ilişkilendirilebilir. Belirli bir anahtar için, Mobile Engagement yalnızca en son değer kümesini izler (tarihçe yoktur). Bir uygulama bilgisi değerinin ayarlanması veya değiştirilmesi Mobile Engagement’ı bu uygulama bilgisinde ayarlanmış hedef kitle ölçütlerini (varsa) yeniden değerlendirmeye zorlar, yani uygulama bilgileri gerçek zamanlı gönderimleri tetiklemek için kullanılabilir.

## Ek veriler
Ek veriler (veya ekler) olaylar, hatalar, etkinlikler ve işlere eklenebilecek bazı rastgele verilerdir.

Ekler, JSON nesnelerine benzer şekilde yapılandırılır: bir anahtar/değer çiftleri ağacından meydana gelirler. Anahtarlar 64 ASCII harf [a-zA-Z], rakamlar [0-9] ve alt çizgi [_] ile sınırlıdır ve eklerin toplam boyutu 1024 karakterle (Mobile Engagement SDK’sı tarafından JSON’da kodlandıktan sonra) sınırlıdır.

Anahtar/değer çiftlerinden oluşan ağacın tamamı bir JSON nesnesi olarak depolanır. Bununla birlikte, Segmentler gibi bazı gelişmiş işlevler tarafından doğrudan erişilebilmesi için anahtarların/değerlerin yalnızca ilk düzeyi parçalanır (örneğin, “content_type” ek anahtarı “bilim kurgu” değerine ayarlanmış olan “content_viewed” olayını geçtiğimiz ay içerisinde en az 10 kez gönderen tüm kullanıcılardan oluşan "Bilim kurgu severler" adlı bir segmenti kolayca tanımlayabilirsiniz). Bu nedenle, yalnızca skaler değerler kullanan anahtar/değer çiftlerinin (dizeler, tarihler, tamsayı veya Boole değerleri gibi) basit listelerinden yapılmış ekler göndermeniz şiddetle önerilir.

## Sonraki adımlar

- [Azure Mobile Engagement için Windows Evrensel SDK’ya genel bakış](mobile-engagement-windows-store-sdk-overview.md)
- [Azure Mobile Engagement için Windows Phone Silverlight SDK’sına genel bakış](mobile-engagement-windows-phone-sdk-overview.md)
- [Azure Mobile Engagement için iOS SDK’sı](mobile-engagement-ios-sdk-overview.md)
- [Azure Mobile Engagement için Android SDK’sı](mobile-engagement-android-sdk-overview.md)



<!--HONumber=Jun16_HO2-->


