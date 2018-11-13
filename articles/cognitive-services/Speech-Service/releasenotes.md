---
title: Konuşma Tanıma Hizmeti SDK'sı Belgeleri
titlesuffix: Azure Cognitive Services
description: Sürüm Notları - en son sürümlerde nelerin değiştiğini
services: cognitive-services
author: wolfma61
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 11/12/2018
ms.author: wolfma
ms.openlocfilehash: 706f51ae1e2d81e2003f2fcd637def95c7a42f8e
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2018
ms.locfileid: "51567545"
---
# <a name="release-notes"></a>Sürüm notları

## <a name="speech-service-sdk-110"></a>Konuşma hizmeti SDK'sını 1.1.0

**Yeni Özellikler**

* Android x86 x64 desteği.
* Proxy desteği: SpeechConfig nesnesinde artık proxy bilgilerini (ana bilgisayar adı, bağlantı noktası, kullanıcı adı ve parola) ayarlamak için bir işlevi çağırabilirsiniz. Bu özellik henüz İos'ta kullanılabilir değil.
* Geliştirilmiş hata kodu ve iletileri. Bir tanıma bir hata döndürülürse, bu zaten ayarlanmış `Reason` (içinde iptal edilen olay için) veya `CancellationDetails` (tanıma işleminin sonucu içinde) için `Error`. İptal edilen olay şimdi iki ek üyeleri içeren `ErrorCode` ve `ErrorDetails`. Sunucu ek hata bilgileri ile bildirilen hata döndürdüyse, artık yeni üyeleri kullanılabilir olacaktır.

**Geliştirmeleri**

* Eklenen ek doğrulama tanıyıcı yapılandırma ve eklenen ek hata iletisi.
* Uzun süreli sessizlik ortasında bir ses dosyasının işlenmesinde'yi tıklatın.
* NuGet paketi: .NET Framework projeleri AnyCPU yapılandırmayla yapı engellemek.

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

## <a name="speech-service-sdk-101"></a>Konuşma hizmeti SDK'sını 1.0.1

Güvenilirlik geliştirmeleri ve hata düzeltmeleri:

* Disposing tanıyıcı yarış durumu nedeniyle olası sabit önemli hata
* Ayarlama özellikleri durumunda sabit olası önemli hata.
* Eklenen ek hata ve parametre denetimi.
* İçinde NSString adı geçersiz kılma nedeni objective-C: sabit olası önemli hata oluştu.
* API görünürlüğünü objective-C: ayarlandı
* JavaScript: olaylar ve yükleri düzeltildi.
* Belgeleri geliştirmeleri.

İçinde bizim [örnek depoyu](https://aka.ms/csspeech/samples), JavaScript için yeni bir örnek eklendi.

## <a name="cognitive-services-speech-sdk-100-2018-september-release"></a>Bilişsel hizmetler konuşma SDK 1.0.0: Eylül 2018 sürüm

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
* Deneysel: Java 8 (64-bit) Windows ve Linux (Ubuntu 16.04 x 64) desteği.
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

## <a name="cognitive-services-speech-sdk-050-2018-july-release"></a>Bilişsel hizmetler konuşma SDK buradan 0.5.0 sürümünü: Temmuz 2018 sürüm

**Yeni Özellikler**

* Android platform desteği (API 23: Android 6.0 Marshmallow veya üzeri). Kullanıma [Android Hızlı Başlangıç](quickstart-java-android.md).
* Windows üzerinde .NET Standard 2.0 desteği. Kullanıma [.NET Core hızlı](quickstart-csharp-dotnetcore-windows.md).
* Deneysel: Destek UWP üzerinde Windows (sürüm 1709 veya üzeri).
  * Kullanıma [UWP hızlı](quickstart-csharp-uwp.md).
  * Not: Windows uygulama sertifikası Kiti (WACK) Speech SDK'sı ile oluşturulan UWP uygulamaları henüz geçmeyin.
* Otomatik yeniden bağlanma ile uzun süreli tanıma destekler.

**İşlevsel değişiklikler**

* `StartContinuousRecognitionAsync()` uzun süreli tanıma destekler.
* Tanıma işleminin sonucu daha fazla alan içeriyor. Ses başına ve süresi (hem de saat döngüsü) ve tanınan metin tanıma durumu, örneğin, temsil eden ek değerler uzaklığı `InitialSilenceTimeout` ve `InitialBabbleTimeout`.
* AuthorizationToken factory örnekleri oluşturmak için destek.

**Bozucu değişiklikler**

* Tanıma olayları: NoMatch olay türü, hata olayı birleştirilmiş.
* SpeechOutputFormat C#, C++ ile uyumlu kalmak için OutputFormat olarak değiştirildi.
* Bazı yöntemleri dönüş türünü `AudioInputStream` arabirim biraz değişti:
   * Java'da,. `read` yöntemi şimdi döndürür `long` yerine `int`.
   * C# ' ta, `Read` yöntemi şimdi döndürür `uint` yerine `int`.
   * C++ ' ta `Read` ve `GetFormat` yöntemleri artık dönüş `size_t` yerine `int`.
* C++: Yalnızca ses giriş akışları artık örneklerini geçirilebilir bir `shared_ptr`.

**Hata düzeltmeleri**

* Sonuçta hatalı dönüş değerleri sabit olduğunda `RecognizeAsync()` zaman aşımına uğrar.
* Windows media foundation kitaplıkları bağımlılığı kaldırıldı. SDK'sı artık çekirdek ses API'leri kullanır.
* Belge düzeltmesi: eklenen bir [bölgeleri](regions.md) desteklenen bölgelerin açıklamak için sayfa.

**Bilinen sorun**

* Android Speech SDK'sı için çeviri konuşma sentezi sonuçları bildirmez. Bu sorun, sonraki sürümde düzeltilecektir.

## <a name="cognitive-services-speech-sdk-040-2018-june-release"></a>Bilişsel hizmetler konuşma SDK 0.4.0: Haziran 2018'den sürüm

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
