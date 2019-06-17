---
title: Android'de Java Microsoft konuşma tanıma API'si ile çalışmaya başlama | Microsoft Docs
titlesuffix: Azure Cognitive Services
description: Konuşmayı metne dönüştürme Android uygulamaları geliştirmek için Microsoft konuşma tanıma API'si kullanın.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.subservice: bing-speech
ms.topic: article
ms.date: 09/18/2018
ms.author: zhouwang
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 147042e300e629dd7e354d4e9079cc4855a8146c
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60515192"
---
# <a name="quickstart-use-the-bing-speech-recognition-api-in-java-on-android"></a>Hızlı Başlangıç: Bing konuşma tanıma API'si Java'da Android'de kullanın

[!INCLUDE [Deprecation note](../../../../includes/cognitive-services-bing-speech-api-deprecation-note.md)]

Bing konuşma tanıma API'SİYLE konuşma kayıtlarını metne dönüştürmek için Bing konuşma bulut tabanlı hizmet kullanan Android uygulamaları geliştirebilirsiniz. Uygulamanız aynı anda olabilir ve zaman uyumsuz olarak hizmete ses gönderiyor aynı anda kısmi tanıma sonuçları almak için gerçek zamanlı akış, API'yi destekler.

Bu makalede, Android için konuşma istemcisi kitaplığını Android cihazları için Java konuşma metin uygulama geliştirmek için nasıl kullanılacağını göstermek için örnek bir uygulama kullanılmıştır.

## <a name="prerequisites"></a>Önkoşullar

### <a name="platform-requirements"></a>Platform gereksinimleri

Örnek tarafından geliştirilen [Android Studio](https://developer.android.com/sdk/index.html) Java'da Windows için.

### <a name="get-the-client-library-and-sample-application"></a>İstemci Kitaplığı ve örnek uygulaması alma

Konuşma istemcisi kitaplığını ve Android için örnekleri kullanılabilir [konuşma tanıma istemcisi Android için SDK](https://github.com/microsoft/cognitive-speech-stt-android). Derlenebilir örnek samples/SpeechRecoExample dizininin altında bulabilirsiniz. İhtiyacınız kullanmak için kendi uygulamalarında SpeechSDK/kitaplıkları armeabi altında iki kitaplıkları ve x86 bulabilirsiniz klasör. Libandroid_platform.so dosyasının boyutu, 22 MB olmakla birlikte dağıtım sırasında 4 MB olarak azaltılır.

#### <a name="subscribe-to-the-speech-api-and-get-a-free-trial-subscription-key"></a>Konuşma tanıma API'si için abone olur ve ücretsiz deneme aboneliği anahtarını alma

Konuşma tanıma API'si, Bilişsel hizmetler (daha önce Project Oxford) bir parçasıdır. Ücretsiz deneme aboneliği anahtarları alabilirsiniz [Bilişsel hizmetler abonelik](https://azure.microsoft.com/try/cognitive-services/) sayfası. Konuşma tanıma API'si belirledikten sonra seçin **API anahtarı alma** anahtarını almak için. Birincil ve ikincil anahtar döndürür. İki anahtarı kullanabilmeniz için her iki anahtarı aynı kotası bağlıdır.

Kullanmak istiyorsanız *tanıma amacıyla*, ayrıca kaydolmanız gerekir [Language Understanding Intelligent Service (LUIS)](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/).

> [!IMPORTANT]
>* Bir abonelik anahtarı edinirler. Konuşma istemci kitaplıkları kullanabilmeniz için önce olmalıdır bir [abonelik anahtarı](https://azure.microsoft.com/try/cognitive-services/).
>
>* Abonelik anahtarınızı kullanın. Sağlanan Android örnek uygulama ile birlikte dosya samples/SpeechRecoExample/res/values/strings.xml abonelik anahtarlarınızı ile güncelleştirin. Daha fazla bilgi için [derleme ve çalıştırma örnekleri](#build-and-run-samples).

## <a name="use-the-speech-client-library"></a>Konuşma istemci kitaplığını kullanma

Uygulamanızda istemci kitaplığını kullanmak için izleyin [yönergeleri](https://github.com/microsoft/cognitive-speech-stt-android#the-client-library).

Kitaplık Başvurusu docs klasöründe bulunan Android için istemci bulabilirsiniz [konuşma tanıma istemcisi Android için SDK](https://github.com/microsoft/cognitive-speech-stt-android).

## <a name="build-and-run-samples"></a>Derleme ve örneklerini çalıştırma

Derleme ve örneklerini çalıştırma hakkında bilgi edinmek için bu bkz [Benioku sayfa](https://github.com/microsoft/cognitive-speech-stt-android#the-sample).

## <a name="samples-explained"></a>Açıklanan örnekleri

### <a name="create-recognition-clients"></a>Tanıma istemcileri oluşturma

Aşağıdaki örnek kodda, kullanıcı senaryoları tabanlı tanıma istemci sınıfları oluşturma işlemi gösterilmektedir:

```java
void initializeRecoClient()
    {
        String language = "en-us";

        String subscriptionKey = this.getString(R.string.subscription_key);
        String luisAppID = this.getString(R.string.luisAppID);
        String luisSubscriptionID = this.getString(R.string.luisSubscriptionID);

        if (m_isMicrophoneReco && null == m_micClient) {
            if (!m_isIntent) {
                m_micClient = SpeechRecognitionServiceFactory.createMicrophoneClient(this,
                                                                                     m_recoMode,
                                                                                     language,
                                                                                     this,
                                                                                     subscriptionKey);
            }
            else {
                MicrophoneRecognitionClientWithIntent intentMicClient;
                intentMicClient = SpeechRecognitionServiceFactory.createMicrophoneClientWithIntent(this,
                                                                                                   language,
                                                                                                   this,
                                                                                                   subscriptionKey,
                                                                                                   luisAppID,
                                                                                                   luisSubscriptionID);
                m_micClient = intentMicClient;

            }
        }
        else if (!m_isMicrophoneReco && null == m_dataClient) {
            if (!m_isIntent) {
                m_dataClient = SpeechRecognitionServiceFactory.createDataClient(this,
                                                                                m_recoMode,
                                                                                language,
                                                                                this,
                                                                                subscriptionKey);
            }
            else {
                DataRecognitionClientWithIntent intentDataClient;
                intentDataClient = SpeechRecognitionServiceFactory.createDataClientWithIntent(this,
                                                                                              language,
                                                                                              this,
                                                                                              subscriptionKey,
                                                                                              luisAppID,
                                                                                              luisSubscriptionID);
                m_dataClient = intentDataClient;
            }
        }
    }

```

İstemci Kitaplığı önceden uygulanan tanıma, konuşma tanıma tipik senaryolar için istemci sınıfları sağlar:

* `DataRecognitionClient`: Konuşma tanıma PCM verileri (örneğin, bir dosya ya da ses kaynağından gelen) ile. Veri arabellekleri ayrılmıştır ve her arabellek konuşma hizmeti için gönderilir. Kullanıcının kendi sessizlik algılama isterseniz uygulayabilmek için hiçbir değişiklik arabellekleri için gerçekleştirilir. Veri WAV dosyalarını sağlanırsa, konuşma tanıma hizmeti dosya sağdan veriler gönderebilir. Ham verileri varsa, örneğin, Bluetooth üzerinden gelen ses, ilk biçimi üstbilgisi konuşma hizmeti verileri gönderin.
* `MicrophoneRecognitionClient`: Konuşma tanıma mikrofondan gelen sesi ile. Mikrofondan gelen veriler için konuşma tanıma hizmeti gönderilir ve mikrofon açık olduğundan emin olun. Tanıma hizmetine gönderilmeden önce bir yerleşik "sessizlik algılayıcısı" Mikrofon verilere uygulanır.
* `DataRecognitionClientWithIntent` ve `MicrophoneRecognitionClientWithIntent`: Bu istemciler, metin tanıma, yapılandırılmış bilgiler amacı hakkında daha fazla Eylemler sürücüsüne uygulamalarınız tarafından kullanılabilecek konuşmacının ek olarak döndürür. "Hedefi" kullanmak için önce kullanarak bir model eğitip gerekir [LUIS](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/).

### <a name="recognition-language"></a>Tanıma dili

Kullanırken `SpeechRecognitionServiceFactory` istemcisi oluşturmak için bir dil seçmeniz gerekir. Konuşma hizmeti tarafından desteklenen dillerin tam listesi için bkz. [desteklenen diller](../API-Reference-REST/supportedlanguages.md).

### `SpeechRecognitionMode`

Ayrıca belirtmenize gerek `SpeechRecognitionMode` istemciyle oluşturduğunuzda `SpeechRecognitionServiceFactory`:

* `ShortPhrase`: Uzun bir utterance fazla 15 saniye. Hizmetine gönderilen veri gibi istemci, birden çok kısmı sonuç ve birden çok en iyi n seçim ile bir nihai sonucu alır.
* `LongDictation`: Uzun bir utterance en fazla iki dakika. Hizmete gönderilen veri gibi istemci birden çok kısmı sonuç ve hizmeti cümle duraklamaları burada tanımlar göre birden çok Nihai sonuç alır.

### <a name="attach-event-handlers"></a>Olay işleyicileri ekleme

İstemciye çeşitli olay işleyicileri ekleyebilirsiniz, oluşturan:

* **Kısmi sonuçlar olayları**: Konuşma hizmeti bile Konuşmayı bitirmeden kişilerin, yorumlarını tahmin her başlatıldığında bu olay adlı (kullanırsanız `MicrophoneRecognitionClient`) veya veri gönderen son (kullanırsanız `DataRecognitionClient`).
* **Hata olayları**: Hizmet bir hata algıladığında çağrılır.
* **Hedefi olayları**: "WithIntent" istemciler üzerinde çağrılır (yalnızca `ShortPhrase` modu) sonra son tanıma işleminin sonucu yapılandırılmış bir JSON hedefi ayrıştırılır.
* **Neden olayların**:
  * İçinde `ShortPhrase` modu, bu olay adı verilir ve konuşma tamamladıktan sonra en iyi n sonuçlarını döndürür.
  * İçinde `LongDictation` modu, olay işleyicisi adlı birden çok kez bağlı hizmeti cümle duraklamaları burada tanımlar.
  * **Her en iyi n seçim**, güvenirlik değeri ve tanınan metin birkaç farklı biçimleri döndürülür. Daha fazla bilgi için [çıkış biçimi](../Concepts.md#output-format).

## <a name="related-topics"></a>İlgili konular

* [Android istemci Kitaplığı Başvurusu](https://github.com/Azure-Samples/Cognitive-Speech-STT-Android/tree/master/docs)
* [C# Microsoft konuşma tanıma API'si için .NET içinde Windows kullanmaya başlayın](GetStartedCSharpDesktop.md)
* [İOS üzerinde Objective-C, Microsoft konuşma tanıma API'si ile başlama](Get-Started-ObjectiveC-iOS.md)
* [JavaScript içinde Microsoft konuşma tanıma API'si ile çalışmaya başlama](GetStartedJSWebsockets.md)
* [REST aracılığıyla Microsoft konuşma tanıma API'si ile çalışmaya başlama](GetStartedREST.md)
