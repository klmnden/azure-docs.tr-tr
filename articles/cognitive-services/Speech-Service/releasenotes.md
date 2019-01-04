---
title: Sürüm Notları - konuşma Hizmetleri
titlesuffix: Azure Cognitive Services
description: Azure konuşma Hizmetleri için özellik sürümleri, geliştirmeleri, hata düzeltmeleri ve bilinen sorunlar çalışan günlüğe bakın.
services: cognitive-services
author: wolfma61
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 12/18/2018
ms.author: wolfma
ms.custom: seodec18
ms.openlocfilehash: 7485ca1e4b1143ed46c9b3bef9ca66af0638b4f8
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/19/2018
ms.locfileid: "53599425"
---
# <a name="release-notes"></a>Sürüm notları

## <a name="speech-sdk-120-2018-december-release"></a>SDK'sı 1.2.0 konuşma: Aralık 2018 sürüm

**Yeni Özellikler**

* Python
  * Python desteği (3.5 ve üzeri) Beta sürümü bu sürümle birlikte kullanılabilir. Daha fazla ayrıntı için [Burada gördüğünüz](quickstart-python.md).
* JavaScript
  * JavaScript Speech SDK'sı, açık kaynaklı olmuştur. Kaynak kodu [GitHub](https://github.com/Microsoft/cognitive-services-speech-sdk-js).
  * Şimdi Node.js destekliyoruz, daha fazla bilgi bulunabilir [burada](quickstart-js-node.md).
  * Yeniden bağlanma kapak altında otomatik olarak gerçekleştirilecek ses oturumlar için uzunluk kısıtlamasına kaldırıldı.
* Bağlantı nesnesi
  * Tanıyıcı bir bağlantı nesnesi erişebilirsiniz. Bu nesne, açıkça hizmet bağlantı başlatma ve bağlanma ve bağlantıyı kesme olayları abone olmak sağlar.
    (Bu henüz JavaScript ve Python kullanılabilir değildir.)
* Ubuntu 18.04 desteği.
* Android
  * Etkinleştirildiğinde proguard'ı APK oluşturma sırasında destekler.

**Geliştirmeleri**

* İş parçacıkları, kilitleri, mutex'leri sayısını azaltmayı iç iş parçacığı kullanımı geliştirmeleri.
* İyileştirilmiş hata raporlama / bilgileri. Bazı durumlarda hata iletileri ölçeği yayıldıktan değil.
* Geliştirme bağımlılıkları güncel modülleri kullanma JavaScript güncelleştirildi.

**Hata düzeltmeleri**

* RecognizeAsync içinde tür uyuşmazlığı nedeniyle sabit bir bellek sızıntısı.
* Bazı durumlarda özel durumları sızmasını.
* Bellek sızıntısı çeviri olay bağımsız değişkenlerini düzeltiliyor.
* Sabit bir kilitleme sorunu yeniden içinde uzun süre çalışan oturumları.
* Hangi başarısız çevirileri için nihai sonucu eksik yol açabilecek bir sorun düzeltildi.
* C# İÇİN: Ana iş parçacığında zaman uyumsuz bir işlemi bekleniyor değildi, tanıyıcı zaman uyumsuz görev tamamlanmadan önce bırakılan mümkündü.
* Java: Java sanal makinenin içindeki bir kilitlenme kaynaklanan bir sorun düzeltildi.
* Objective-C: Sabit liste eşlemesi RecognizedIntent RecognizingIntent yerine döndürüldü.
* JavaScript: Varsayılan çıkış biçimini SpeechConfig 'basit' kümesi.
* JavaScript: JavaScript içinde yapılandırma nesnesindeki özellikleri ve diğer diller arasında tutarsızlık kaldırılıyor.

**Örnekler**

* Güncelleştirilmiş ve çeşitli örnekleri (örneğin çeviri, vb. çıkış sesleri) sabit.
* Node.js Örnekleri eklenen [örnek depoyu](https://aka.ms/csspeech/samples).

## <a name="speech-sdk-110"></a>Konuşma SDK 1.1.0

**Yeni Özellikler**

* Android x86 x64 desteği.
* Proxy desteği: SpeechConfig nesnesinde proxy bilgilerini (ana bilgisayar adı, bağlantı noktası, kullanıcı adı ve parola) ayarlamak için bir işlevi çağırabilir. Bu özellik henüz İos'ta kullanılabilir değil.
* Geliştirilmiş hata kodu ve iletileri. Bir tanıma bir hata döndürülürse, bu zaten ayarlanmış `Reason` (içinde iptal edilen olay için) veya `CancellationDetails` (tanıma işleminin sonucu içinde) için `Error`. İptal edilen olay şimdi iki ek üyeleri içeren `ErrorCode` ve `ErrorDetails`. Sunucu ek hata bilgileri ile bildirilen hata döndürdüyse, artık yeni üyeleri kullanılabilir olacaktır.

**Geliştirmeleri**

* Eklenen ek doğrulama tanıyıcı yapılandırma ve eklenen ek hata iletisi.
* Uzun süreli sessizlik ortasında bir ses dosyasının işlenmesinde'yi tıklatın.
* NuGet paketi: isteğe bağlı olarak .NET Framework projeleri için yapı AnyCPU yapılandırmayla engeller.

**Hata düzeltmeleri**

* Tanıyıcılar işlem bulunan birkaç özel durum düzeltildi. Ayrıca özel durum yakalandı ve iptal edildi olaya dönüştürülür.
* Özellik Yönetimi'nde bir bellek sızıntısı düzeltin.
* Ses giriş dosyası tanıyıcı kilitlenebiliyordu hata düzeltildi.
* Olayları sonra oturumu durdurulduğunda burada alınan bir hata düzeltildi.
* İş parçacığı içinde bazı yarış durumları düzeltildi.
* Bir iOS bir çökmesine neden neden olabilecek bir uyumluluk sorunu düzeltildi.
* Android mikrofon desteği kararlılık geliştirmeleri.
* Javascript'teki bir tanıyıcı tanıma dil yok burada bir hata düzeltildi.
* (Bazı durumlarda) EndpointId ayarlama engelleyen bir hatayı JavaScript'te düzeltildi.
* JavaScript ve eklenen eksik AddIntent JavaScript imza AddIntent sırayla değiştirilen bir parametre.

**Örnekler**

* C++ eklendi ve C# çekme ve itme stream kullanımına yönelik samplea [örnek depoyu](https://aka.ms/csspeech/samples).

## <a name="speech-sdk-101"></a>Konuşma SDK 1.0.1

Güvenilirlik geliştirmeleri ve hata düzeltmeleri:

* Disposing tanıyıcı yarış durumu nedeniyle olası sabit önemli hata
* Ayarlama özellikleri durumunda sabit olası önemli hata.
* Eklenen ek hata ve parametre denetimi.
* Objective-C: İçinde NSString adı geçersiz kılma nedeni sabit olası önemli hata oluştu.
* Objective-C: API ayarlanmış görünürlüğünü
* JavaScript: Sabit ilgili olaylar ve bunların yükler.
* Belgeleri geliştirmeleri.

İçinde bizim [örnek depoyu](https://aka.ms/csspeech/samples), JavaScript için yeni bir örnek eklendi.

## <a name="cognitive-services-speech-sdk-100-2018-september-release"></a>Bilişsel hizmetler konuşma 1.0.0 SDK: Eylül 2018 sürüm

**Yeni Özellikler**

* İOS üzerinde Objective-C için destek. Kullanıma sunduğumuz [iOS Objective-C hızlı](quickstart-objectivec-ios.md).
* Tarayıcıda JavaScript desteği. Kullanıma sunduğumuz [JavaScript hızlı](quickstart-js-browser.md).

**Bozucu değişiklikler**

* Bu sürümle birlikte birkaç önemli değişiklikler yapılmıştır.
  Lütfen denetleyin [bu sayfayı](https://aka.ms/csspeech/breakingchanges_1_0_0) Ayrıntılar için.

## <a name="cognitive-services-speech-sdk-060-2018-august-release"></a>Bilişsel hizmetler konuşma SDK 0.6.0: Ağustos 2018 sürüm

**Yeni Özellikler**

* UWP uygulamaları artık yerleşik olarak konuşma SDK'sı ile Windows uygulama sertifikası Kiti (WACK) geçirebilirsiniz.
  Kullanıma [UWP hızlı](quickstart-csharp-uwp.md).
* .NET Standard 2.0 Linux'ta (Ubuntu 16.04 x 64) için destek.
* Deneysel: Java 8 (64-bit) Windows ve Linux'ta (Ubuntu 16.04 x 64) destekler.
  Kullanıma [Java Çalışma zamanı ortamı hızlı](quickstart-java-jre.md).

**İşlev değişikliği**

* Ek hata bağlantı hatalarıyla ilgili ayrıntılı bilgi kullanıma sunar.

**Bozucu değişiklikler**

* Java (Android) üzerinde `SpeechFactory.configureNativePlatformBindingWithDefaultCertificate` işlevi artık bir yol parametresi gerektirir. Artık bir yolu, tüm desteklenen platformlarda otomatik olarak algılanır.
* Özelliğin get erişimcisine `EndpointUrl` Java ve C# ' kaldırıldı.

**Hata düzeltmeleri**

* Java dilinde çeviri tanıyıcı ses sentezi sonucuna artık uygulanır.
* Etkin olmayan iş parçacıkları ve açık ve kullanılmayan yuva sayısının artması neden olabilecek bir hata düzeltildi.
* Uzun süreli tanıma ortasında iletim burada sonlandırmak bir sorun düzeltildi.
* Tanıyıcı kapatma bir yarış durumu düzeltildi.

## <a name="cognitive-services-speech-sdk-050-2018-july-release"></a>Bilişsel hizmetler konuşma SDK'sını buradan 0.5.0 sürümünü: Temmuz 2018 sürüm

**Yeni Özellikler**

* Android platform desteği (API 23: Android 6.0 Marshmallow veya üzeri). Kullanıma [Android Hızlı Başlangıç](quickstart-java-android.md).
* Windows üzerinde .NET Standard 2.0 desteği. Kullanıma [.NET Core hızlı](quickstart-csharp-dotnetcore-windows.md).
* Deneysel: UWP, Windows üzerinde (sürüm 1709 veya üzeri) destekler.
  * Kullanıma [UWP hızlı](quickstart-csharp-uwp.md).
  * Not: Konuşma SDK'sı ile oluşturulan UWP uygulamaları, Windows uygulama sertifikası Kiti (WACK) henüz geçmeyin.
* Otomatik yeniden bağlanma ile uzun süreli tanıma destekler.

**İşlevsel değişiklikler**

* `StartContinuousRecognitionAsync()` uzun süreli tanıma destekler.
* Tanıma işleminin sonucu daha fazla alan içeriyor. Ses başına ve süresi (hem de saat döngüsü) ve tanınan metin tanıma durumu, örneğin, temsil eden ek değerler uzaklığı `InitialSilenceTimeout` ve `InitialBabbleTimeout`.
* AuthorizationToken factory örnekleri oluşturmak için destek.

**Bozucu değişiklikler**

* Tanıma olayları: Hata olayı birleştirilmiş NoMatch olay türü.
* SpeechOutputFormat C#, C++ ile uyumlu kalmak için OutputFormat olarak değiştirildi.
* Bazı yöntemleri dönüş türünü `AudioInputStream` arabirim biraz değişti:
   * Java'da,. `read` yöntemi şimdi döndürür `long` yerine `int`.
   * C# ' ta, `Read` yöntemi şimdi döndürür `uint` yerine `int`.
   * C++ ' ta `Read` ve `GetFormat` yöntemleri artık dönüş `size_t` yerine `int`.
* C++: Yalnızca ses giriş akışları artık örneklerini geçirilebilir bir `shared_ptr`.

**Hata düzeltmeleri**

* Sonuçta hatalı dönüş değerleri sabit olduğunda `RecognizeAsync()` zaman aşımına uğrar.
* Windows media foundation kitaplıkları bağımlılığı kaldırıldı. SDK'sı artık çekirdek ses API'leri kullanır.
* Belge düzeltmesi: Eklenen bir [bölgeleri](regions.md) desteklenen bölgelerin açıklamak için sayfa.

**Bilinen sorun**

* Android Speech SDK'sı için çeviri konuşma sentezi sonuçları bildirmez. Bu sorun, sonraki sürümde düzeltilecektir.

## <a name="cognitive-services-speech-sdk-040-2018-june-release"></a>Bilişsel hizmetler konuşma SDK 0.4.0: Haziran 2018 yayını

**İşlevsel değişiklikler**

- AudioInputStream

  Bir tanıyıcı, artık bir akışı ses kaynağı olarak tüketebilir. Daha fazla bilgi için bkz. ilgili [yapılır kılavuzunda](how-to-use-audio-input-streams.md).

- Ayrıntılı çıkış biçimi

  Oluştururken bir `SpeechRecognizer`, talep edebilir `Detailed` veya `Simple` çıkış biçimi. `DetailedSpeechRecognitionResult` Maskelenmiş küfür ile bir güven puanı, tanınan metin, ham bir sözcük formu, normalleştirilmiş form ve normalleştirilmiş form içerir.

**Yeni değişiklik**

- Değiştirilecek `SpeechRecognitionResult.Text` gelen `SpeechRecognitionResult.RecognizedText` C#.

**Hata düzeltmeleri**

- Olası bir geri çağırma sorunu USP katmanda kapatılırken düzeltildi.

- Ses giriş dosyası bir tanıyıcı kullanılan, onu için gerekenden daha uzun dosya tanıtıcısı bulunduran.

- İleti pompası tanıyıcı arasındaki birkaç kilitlenmeleri kaldırıldı.

- Ateş bir `NoMatch` hizmetinden gelen yanıt zaman aşımına neden.

- Windows media foundation kitaplıklarındaki Gecikmeli yüklendi ' dir. Bu kitaplık, yalnızca giriş mikrofon için gereklidir.

- Karşıya yükleme hızı için ses verilerini iki kez özgün ses hızı hakkında sınırlıdır.

- Windows üzerinde C# .NET derlemeleri artık strong adlandırılır.

- Belge düzeltmesi: `Region` bir tanıyıcı oluşturmak için gerekli bir bilgidir.

Daha fazla örnek eklenmiştir ve sürekli olarak güncelleştiriliyor. En son örnekleri için bkz [Speech SDK'sı örnekleri GitHub deposunda](https://aka.ms/csspeech/samples).

## <a name="cognitive-services-speech-sdk-0212733-2018-may-release"></a>Bilişsel hizmetler konuşma SDK 0.2.12733: Mayıs 2018 sürüm

Bu sürüm ilk Bilişsel hizmetler konuşma SDK'sı genel Önizleme sürümüdür.
