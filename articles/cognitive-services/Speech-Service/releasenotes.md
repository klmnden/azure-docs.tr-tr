---
title: Sürüm Notları - konuşma Hizmetleri
titlesuffix: Azure Cognitive Services
description: Azure konuşma Hizmetleri için özellik sürümleri, geliştirmeleri, hata düzeltmeleri ve bilinen sorunlar çalışan günlüğe bakın.
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 06/26/2019
ms.author: wolfma
ms.custom: seodec18
ms.openlocfilehash: 609443a4926fabd991846faee4a0a7dffe3a696b
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67490188"
---
# <a name="release-notes"></a>Sürüm notları

## <a name="speech-sdk-160-2019-june-release"></a>SDK'sı 1.6.0 konuşma: 2019 Haziran yayını

**Örnekler**
*   Metin okuma UWP ve Unity için hızlı başlangıç örnekleri
*   İOS Swift için hızlı başlangıç örnek
*   Konuşma & amacı tanımayı ve çeviri için Unity örnekleri
*   DialogServiceConnector için güncelleştirilmiş hızlı başlangıç örnekleri

**Geliştirmeleri / değiştirir**
* İletişim kutusu ad alanı:
    * SpeechBotConnector DialogServiceConnector için yeniden adlandırıldı
    * BotConfig DialogServiceConfig için yeniden adlandırıldı
    * BotConfig::FromChannelSecret() DialogServiceConfig::FromBotSecret() için eşleştirilmiş
    * Tüm mevcut doğrudan satır konuşma istemcileri yeniden adlandırma sonrasında desteklenmeye devam edilir.
* Proxy, kalıcı bağlantı desteklemek için TTS REST bağdaştırıcısı güncelleştirme
* Hata iletisi geçersiz olan bir bölge geçirildiğinde geliştirin
* Swift/Objective-C:
    * Gelişmiş hata raporlama: Bir hatayla sonuçlanabileceği yöntemleri artık iki sürümlerinde mevcuttur: Kullanıma sunan bir bir `NSError` hata işleme ve özel durum harekete bir nesne. Eski Swift için sunulur. Bu değişiklik, mevcut SWIFT kodu uyarlamaları gerektirir.
    * Gelişmiş olay işleme

**Hata düzeltmeleri**
*   TTS için düzeltme: Burada SpeakTextAsync gelecekteki döndürülen ses işleme tamamlanmasını beklemenize gerek kalmadan
*   Dizeleri taşıma için düzeltme C# tam dil desteğini etkinleştirmek için
*   Çekirdek kitaplığı örneklerindeki net461 hedef çerçeve ile yüklemek .NET core uygulama sorunu için düzeltme
*   Örnek çıktı klasöründe yerel kitaplıkları dağıtmak zaman zaman sorunlar için düzeltme
*   Web yuvası güvenilir bir şekilde kapatma için düzeltme
*   Linux üzerinde çok ağır yük altında bir bağlantı açılırken olası kilitlenme yönelik düzeltme
*   MacOS için framework paketteki meta veriler eksik düzeltme
*   İle ilgili sorunlar için düzeltme `pip install --user` Windows üzerinde


## <a name="speech-sdk-151"></a>SDK'sı 1.5.1 konuşma

Bir hata düzeltmesi sürüm ve yalnızca yerel ve yönetilen SDK'yı etkileyen budur. JavaScript SDK'sı sürümünü etkilemiyor.

**Hata düzeltmeleri**

* Konuşma Transkripsiyonu ile kullanıldığında FromSubscription düzeltin.
* Anahtar sözcüğü için ses öncelikli sanal asistan kapsamlı hata düzeltildi.


## <a name="speech-sdk-150-2019-may-release"></a>SDK'sı 1.5.0 konuşma: Mayıs 2019 sürüm

**Yeni Özellikler**

* Uyandırma sözcüğünü (anahtar sözcüğü kapsamlı/KWS) işlevselliği, artık Windows ve Linux için kullanılabilir. KWS işlevselliği KWS destekler, ancak resmi bir mikrofon türüyle çalışabilir şu anda sınırlı mikrofon dizilerine Azure Kinect DK donanım veya konuşma cihaz SDK'sı bulunamadı.
* Tümcecik ipucu işlevselliği SDK aracılığıyla kullanılabilir. Daha fazla bilgi için [burada](how-to-phrase-lists.md).
* Konuşma transkripsiyonu işlevselliği SDK aracılığıyla kullanılabilir. Bkz: [burada](conversation-transcription-service.md).
* Ses öncelikli sanal Yardımcıları doğrudan satır konuşma kanalını kullanma için destek eklendi.

**Örnekler**

* Yeni özelliklerin veya SDK'sı tarafından desteklenen yeni hizmetler eklenen örnekleri.

**Geliştirmeleri / değiştirir**

* Hizmet davranışı veya hizmet sonuçları (küfür ve diğer maskeleme gibi) ayarlamak için çeşitli tanıyıcı özellikleri eklendi.
* Tanıyıcı oluşturulsa bile tanıyıcı aracılığıyla standart yapılandırma özellikleri, artık yapılandırabilirsiniz `FromEndpoint`.
* Objective-C: `OutputFormat` SPXSpeechConfiguration için özelliği eklendi.
* SDK, artık bir Linux dağıtımı Debian 9 destekler.

**Hata düzeltmeleri**

* Konuşmacı kaynak metin okuma çok erken burada imha bir sorun düzeltildi.
## <a name="speech-sdk-142"></a>Konuşma SDK 1.4.2

Bir hata düzeltmesi sürüm ve yalnızca yerel ve yönetilen SDK'yı etkileyen budur. JavaScript SDK'sı sürümünü etkilemiyor.

## <a name="speech-sdk-141"></a>Konuşma SDK 1.4.1

Bu yalnızca JavaScript bir sürümdür. Herhangi bir özellik ekledik. Aşağıdaki düzeltmeleri yapıldı:

* Web paketi https proxy aracısı yüklemesini engeller.

## <a name="speech-sdk-140-2019-april-release"></a>SDK'sı 1.4.0 konuşma: 2019 Nisan sürüm

**Yeni Özellikler** 

* SDK'sı, artık bir beta sürümü metin okuma hizmet destekler. Windows ve Linux Masaüstü'nden desteklenen C++ ve C#. Daha fazla bilgi için kontrol [metin okuma genel bakış](text-to-speech.md#get-started-with-text-to-speech).
* SDK, artık akışı giriş dosyalarını MP3 ve geçerli/OGG ses dosyaları destekler. Bu özellik yalnızca C++ Linux'ta kullanılabilir ve C# ve şu anda beta sürümünde olan (daha fazla ayrıntı [burada](how-to-use-codec-compressed-audio-input-streams.md)).
* Java, .NET core Speech SDK'sı C++ ve Objective-C macOS destek elde. MacOS için Objective-C desteği şu anda beta sürümünde olan.
* iOS: İOS (Objective-C) Speech SDK'sı artık ayrıca bir CocoaPod yayımlanır.
* JavaScript: Varsayılan olmayan mikrofon olarak bir giriş cihazının desteği.
* JavaScript: Node.js için proxy desteği.

**Örnekler**

* Konuşma SDK'yı macOS üzerinde Objective-C ve C++ ile kullanma örnekleri eklenmiştir.
* Metin okuma hizmeti kullanımını gösteren örnekleri eklenmiştir.

**Geliştirmeleri / değiştirir**

* Python: Ek özellikler tanıma sonuçlarının aracılığıyla artık sunulur `properties` özelliği.
* Ek geliştirme ve hata ayıklama desteği için bir günlük dosyasına SDK günlüğe kaydetme ve tanılama bilgileri yönlendirebilirsiniz (daha fazla ayrıntı [burada](how-to-use-logging.md)).
* JavaScript: Ses işleme performansını iyileştirin.

**Hata düzeltmeleri**

* Mac/iOS: Konuşma hizmeti için bir bağlantı kurulamadı için uzun bekleme neden olan bir hata düzeltildi.
* Python: hata işleme Python geri çağırmaları bağımsız değişkenleri için geliştirin.
* JavaScript: Konuşma raporlama sabit durumu yanlış RequestSession üzerinde sona erdi.

## <a name="speech-sdk-131-2019-february-refresh"></a>SDK'sı 1.3.1 konuşma: Şubat 2019 yenileme

Bir hata düzeltmesi sürüm ve yalnızca yerel ve yönetilen SDK'yı etkileyen budur. JavaScript SDK'sı sürümünü etkilemiyor.

**Hata düzeltmesi**

* Mikrofon girişini kullanarak bir bellek sızıntısı düzeltildi. Stream tabanlı veya giriş dosyası etkilenmez.

## <a name="speech-sdk-130-2019-february-release"></a>SDK'sı 1.3.0 konuşma: Şubat 2019 sürümü

**Yeni Özellikler**

* Speech SDK'sı, seçimi giriş mikrofonu AudioConfig sınıfı aracılığıyla destekler. Bu, veri akışı için ses konuşma Hizmetleri için varsayılan olmayan mikrofondan gelen sağlar. Belgeleri açıklayan daha fazla bilgi için bkz. [ses giriş cihaz seçimi](how-to-select-audio-input-devices.md). Bu özellik henüz JavaScript'ten kullanılamaz.
* Speech SDK'sı, artık bir beta sürümünde Unity destekler. Sorun bölümünde aracılığıyla geri bildirim sağlamak [GitHub örnek deposundan](https://aka.ms/csspeech/samples). Bu sürüm Windows x86 ve x64 (Masaüstü veya evrensel Windows platformu uygulamaları), Unity destekler ve Android (ARM32/64, x86). Daha fazla bilgi kullanılabilir bizim [Unity hızlı](quickstart-csharp-unity.md).
* Dosya `Microsoft.CognitiveServices.Speech.csharp.bindings.dll` (önceki sürümlerde sevk) artık gerekli değildir. İşlevselliği artık çekirdek SDK'sı tümleşiktir.


**Örnekler**

Aşağıdaki yeni içerikler ise bizim [örnek depoyu](https://aka.ms/csspeech/samples):

* AudioConfig.FromMicrophoneInput için ek örnekleri.
* Amaç tanıma ve çeviri için ek Python örnekleri.
* Bağlantı nesnesi kullanarak iOS için ek örnekler.
* Ses çıkış çevirisi için ek Java örnekleri.
* Kullanımı için yeni örnek [Batch Transkripsiyonu REST API'si](batch-transcription.md).

**Geliştirmeleri / değiştirir**

* Python
  * Gelişmiş parametre doğrulama ve hata iletilerinde SpeechConfig.
  * Bağlantı nesnesi için destek eklendi.
  * (X86) üzerinde Windows 32 bit Python desteği.
  * Python Speech SDK'sı beta ' dir.
* iOS
  * SDK'sı artık iOS SDK'sı sürüm 12,1 karşı yerleşik olarak bulunur.
  * SDK'sı artık iOS 9.2 ve sonraki sürümleri destekler.
  * Başvuru belgelerini geliştirmeye ve birkaç özellik adları düzeltin.
* JavaScript
  * Bağlantı nesnesi için destek eklendi.
  * İle birlikte gelen JavaScript için tür tanım dosyalarını ekleyin
  * İlk destek ve uygulama tümcecik ipuçları için.
  * Tanıma için JSON hizmetiyle özellikler koleksiyonunu döndürür
* Windows DLL'leri, artık bir sürüm kaynağı içerir.
* Bir tanıyıcı oluşturursanız `FromEndpoint` doğrudan uç nokta URL'sine parametreleri ekleyebilirsiniz. Kullanarak `FromEndpoint` tanıyıcı aracılığıyla standart yapılandırma özelliklerini yapılandıramazsınız.

**Hata düzeltmeleri**

* Boş proxy kullanıcı adı ve Ara sunucu parolasını doğru olarak işlenen değil. Proxy kullanıcı adı ve Ara sunucu parolasını boş bir dize olarak ayarlarsanız, bu sürümle birlikte, bunlar proxy sunucusuna bağlanırken gönderilmez.
* Oturum kimliği oluşturuldu SDK tarafından her zaman tamamen bazı diller için rastgele değildi&nbsp;/ ortamları. Bu sorunu gidermek için rasgele oluşturucunun başlatma eklendi.
* Yetkilendirme belirteci işlenmesini geliştirin. Bir yetkilendirme belirteci kullanmak istiyorsanız, SpeechConfig içinde belirtin ve abonelik anahtarını boş bırakın. Tanıyıcı aşağıda zamanki gibi oluşturabilir.
* Bazı durumlarda bağlantı nesnesi doğru şekilde serbest bırakılmasını değildi. Bu sorun düzeltilmiştir.
* JavaScript örnek ses çıkış çeviri sentezi Safari üzerinde de desteklemek için düzeltildi.

## <a name="speech-sdk-121"></a>SDK'sı 1.2.1 konuşma

Bu yalnızca JavaScript bir sürümdür. Herhangi bir özellik ekledik. Aşağıdaki düzeltmeleri yapıldı:

* Speech.end değil, turn.end, akışın sonuna kov.
* Geçerli gönderme başarısız olursa zamanlamaz sonraki gönderdiniz ses pompa hatayı düzeltin.
* Sürekli tanıma ile kimlik doğrulama belirteci düzeltin.
* Hata düzeltmesi için farklı tanıyıcı / uç noktaları.
* Belgeleri geliştirmeleri.

## <a name="speech-sdk-120-2018-december-release"></a>SDK'sı 1.2.0 konuşma: Aralık 2018 sürüm

**Yeni Özellikler**

* Python
  * Python desteği (3.5 ve üzeri) Beta sürümü bu sürümle birlikte kullanılabilir. Daha fazla bilgi için here](quickstart-python.md) bakın.
* JavaScript
  * JavaScript Speech SDK'sı, açık kaynaklı olmuştur. Kaynak kodu [GitHub](https://github.com/Microsoft/cognitive-services-speech-sdk-js).
  * Şimdi Node.js destekliyoruz, daha fazla bilgi bulunabilir [burada](quickstart-js-node.md).
  * Yeniden bağlanma kapak altında otomatik olarak gerçekleştirilecek ses oturumlar için uzunluk kısıtlamasına kaldırıldı.
* Bağlantı nesnesi
  * Tanıyıcı bir bağlantı nesnesi erişebilirsiniz. Bu nesne, açıkça hizmet bağlantı başlatma ve bağlanma ve bağlantıyı kesme olayları abone olmak sağlar.
    (Bu özellik henüz JavaScript ve Python kullanılabilir değildir.)
* Ubuntu 18.04 desteği.
* Android
  * Etkinleştirildiğinde proguard'ı APK oluşturma sırasında destekler.

**Geliştirmeleri**

* İş parçacıkları, kilitleri, mutex'leri sayısını azaltmayı iç iş parçacığı kullanımı geliştirmeleri.
* İyileştirilmiş hata raporlama / bilgileri. Bazı durumlarda, hata iletileri çıkış ölçeği yayıldıktan değil.
* Geliştirme bağımlılıkları güncel modülleri kullanma JavaScript güncelleştirildi.

**Hata düzeltmeleri**

* RecognizeAsync içinde tür uyuşmazlığı nedeniyle sabit bir bellek sızıntısı.
* Bazı durumlarda özel durumları sızmasını.
* Bellek sızıntısı çeviri olay bağımsız değişkenlerini düzeltiliyor.
* Sabit bir kilitleme sorunu yeniden içinde uzun süre çalışan oturumları.
* Başarısız çevirileri için nihai sonucu eksik yol açabilecek bir sorun düzeltildi.
* C# İÇİN: Ana iş parçacığında zaman uyumsuz bir işlemi bekleniyor değildi, tanıyıcı zaman uyumsuz görev tamamlanmadan önce bırakılan mümkündü.
* Java: Java sanal makinenin içindeki bir kilitlenme kaynaklanan bir sorun düzeltildi.
* Objective-C: Sabit liste eşlemesi RecognizedIntent RecognizingIntent yerine döndürüldü.
* JavaScript: Varsayılan çıkış biçimini SpeechConfig 'basit' kümesi.
* JavaScript: JavaScript içinde yapılandırma nesnesindeki özellikleri ve diğer diller arasında tutarsızlık kaldırılıyor.

**Örnekler**

* Güncelleştirilmiş ve çeşitli örnekleri (örneğin çıkış sesleri çeviri, vb..) sabit.
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

* Tanıyıcılar işlem bulunan birkaç özel durum düzeltildi. Ayrıca, özel durum yakalandı ve iptal edildi olaya dönüştürülür.
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

* Bu sürümle birlikte, birkaç önemli değişiklikler yapılmıştır.
  Denetleme [bu sayfayı](https://aka.ms/csspeech/breakingchanges_1_0_0) Ayrıntılar için.

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
