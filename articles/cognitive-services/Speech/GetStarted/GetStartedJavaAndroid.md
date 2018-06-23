---
title: Android üzerinde Java Microsoft konuşma tanıma API'si ile çalışmaya başlama | Microsoft Docs
description: Konuşma ses metne dönüştürme Android uygulamaları geliştirmek için Microsoft konuşma API kullanın.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 09/29/2017
ms.author: zhouwang
ms.openlocfilehash: a10f7be1c36fb431016a9867f606e26be858069e
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352247"
---
# <a name="get-started-with-speech-recognition-in-java-on-android"></a>Konuşma tanıma Android üzerinde Java kullanmaya başlama

Konuşma tanıma API'si ile konuşulan sesi metne dönüştürmek için bulut tabanlı bir konuşma hizmeti kullanan Android uygulamaları geliştirebilirsiniz. Uygulamanız aynı anda olabilir ve hizmete ses gönderiyor aynı anda kısmi tanıma sonuçlarını zaman uyumsuz olarak almak için gerçek zamanlı akış, API destekler.

Bu makalede örnek uygulama Android için konuşma istemci kitaplığı Android cihazları için Java konuşma metin uygulamaları geliştirmek için nasıl kullanılacağını göstermek için kullanır.

## <a name="prerequisites"></a>Önkoşullar

### <a name="platform-requirements"></a>Platform gereksinimleri

Örnek tarafından geliştirilen [Android Studio](http://developer.android.com/sdk/index.html) Windows Java için.

### <a name="get-the-client-library-and-sample-application"></a>İstemci Kitaplığı ve örnek uygulama alma

Konuşma istemci kitaplığı ve örnek Android için kullanılabilir olan [konuşma istemci SDK'sı Android için](https://github.com/microsoft/cognitive-speech-stt-android). Oluþturulabilir örnek samples/SpeechRecoExample dizini altında bulabilirsiniz. İhtiyacınız kullanmak üzere kendi uygulamalarda SpeechSDK/kitaplıklar pushservice altında iki kitaplıklarını ve x86 bulabilirsiniz klasör. Libandroid_platform.so dosyasının boyutu, 22 MB olmakla birlikte dağıtım sırasında 4 MB ile azalır.

#### <a name="subscribe-to-the-speech-api-and-get-a-free-trial-subscription-key"></a>Konuşma API'sine abone olmak ve ücretsiz deneme aboneliği anahtarı alma

Konuşma API Bilişsel hizmetler (daha önce proje Oxford) bir parçasıdır. Ücretsiz deneme aboneliği anahtarları alma [Bilişsel hizmetler abonelik](https://azure.microsoft.com/try/cognitive-services/) sayfası. Konuşma API seçtikten sonra Seç **alma API anahtarı** anahtarı alınamadı. Birincil ve ikincil anahtar döndürür. Her iki anahtar kullanabilmek için her iki anahtarı aynı kotasını bağlıdır.

Kullanmak istiyorsanız, *amacıyla tanıma*, kaydolmak etmeniz [dil anlama akıllı hizmet (HALUK)](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/).

> [!IMPORTANT]
>* Abonelik anahtarı edinin. Konuşma istemci kitaplıkları kullanmadan önce bilmeniz gereken bir [abonelik anahtarı](https://azure.microsoft.com/try/cognitive-services/).
>
>* Abonelik anahtarınızı kullanın. Sağlanan Android örnek uygulama ile dosya samples/SpeechRecoExample/res/values/strings.xml abonelik anahtarlarınızı ile güncelleştirin. Daha fazla bilgi için bkz: [derleme ve çalıştırma örnekleri](#build-and-run-samples).

## <a name="use-the-speech-client-library"></a>Konuşma istemci kitaplığını kullanma

İstemci Kitaplığı, uygulamanızda kullanmak için izleyin [yönergeleri](https://github.com/microsoft/cognitive-speech-stt-android#the-client-library).

İstemci Kitaplığı Başvurusu Android için Belgeler klasöründe bulabileceğiniz [konuşma istemci SDK'sı Android için](https://github.com/microsoft/cognitive-speech-stt-android).

## <a name="build-and-run-samples"></a>Derleme ve çalıştırma örnekleri

Derleme ve çalıştırma örnekleri öğrenmek için bu bkz [README page](https://github.com/microsoft/cognitive-speech-stt-android#the-sample).

## <a name="samples-explained"></a>Açıklanan örnekleri

### <a name="create-recognition-clients"></a>Tanıma istemcileri oluşturma

Aşağıdaki örnek kodda kullanıcı senaryolarını temel alarak tanıma istemci sınıfları oluşturulacağını gösterir:

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

İstemci Kitaplığı, önceden uygulanan tanıma konuşma tanıma tipik senaryolarda istemci sınıfları sağlar:

* `DataRecognitionClient`: Konuşma tanıma verilerle PCM (örneğin, bir dosya veya ses kaynağı). Veri arabelleği ayrılır ve her arabellek konuşma hizmetine gönderilir. Kullanıcı kendi sessizlik algılama isterseniz uygulayabilmek için hiçbir değişiklik arabellekleri için yapılır. Veri WAV dosyalarını sağlanırsa, konuşma hizmetine dosya sağdan veriler gönderebilir. Ham verileri varsa, örneğin, ses Bluetooth üzerinden, gelen, ilk biçimi üstbilgisi konuşma izleyen verilerden hizmetine gönderir.
* `MicrophoneRecognitionClient`: Konuşma tanıma mikrofon gelen ses ile. Mikrofon açık olduğundan ve mikrofon verilerden konuşma tanıma hizmete gönderilen emin olun. Tanıma hizmetine gönderilmeden önce bir yerleşik "sessizlik algılayıcısı" Mikrofon verilere uygulanır.
* `DataRecognitionClientWithIntent` ve `MicrophoneRecognitionClientWithIntent`: Bu istemciler dönün, yapılandırılmış tanıma metin yanı sıra, başka eylemler sürücüsüne uygulamalarınız tarafından kullanılabilecek Konuşmacı amacı hakkında bilgi. "Amacı" kullanmak için öncelikle kullanarak bir modeli eğitmek gerekir [HALUK](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/).

### <a name="recognition-language"></a>Tanıma dili

Kullandığınızda `SpeechRecognitionServiceFactory` istemci oluşturmak için bir dil seçin. Konuşma hizmeti tarafından desteklenen diller tam listesi için bkz: [desteklenen diller](../API-Reference-REST/supportedlanguages.md).

### `SpeechRecognitionMode`

Ayrıca belirtmek zorunda `SpeechRecognitionMode` istemciyle oluşturduğunuzda `SpeechRecognitionServiceFactory`:

* `ShortPhrase`: Bir utterance 15 kadar uzun saniye. Hizmete gönderilen veri gibi istemci birden çok kısmi sonuçlar ve birden çok n en iyi seçenek bir nihai sonucu alır.
* `LongDictation`: Bir utterance iki kadar uzun dakika. Hizmete gönderilen veri gibi istemci birden çok kısmi sonuçlar ve hizmet cümle duraklatır burada tanımlar göre birden çok son sonuçlarını alır.

### <a name="attach-event-handlers"></a>Olay işleyicileri ekleme

İstemciye çeşitli olay işleyicileri iliştirebilirsiniz oluşturduğunuz:

* **Kısmi sonuçlar olayları**: bile konuşarak bitirmeden ne size, belirten konuşma hizmet tahmin her zaman bu olayı adlı (kullanırsanız `MicrophoneRecognitionClient`) veya veri gönderme işlemini tamamladıktan (kullanıyorsanız `DataRecognitionClient`).
* **Hata olayları**: hizmet bir hata algıladığında çağrılır.
* **Hedefi olayları**: "WithIntent" istemcilerde adlı (yalnızca `ShortPhrase` modu) içinde yapılandırılmış bir JSON hedefi son tanıma sonuç ayrıştırılır sonra.
* **Neden olayları**:
  * İçinde `ShortPhrase` modu, bu olay adı verilir ve Konuşmayı bitirdikten sonra n en iyi sonuçlar verir.
  * İçinde `LongDictation` modu, olay işleyicisi çağrılır birden çok kez hizmeti cümle duraklatır burada tanımlar tabanlı.
  * **Her n en iyi seçenek**, güvenirlik değeri ve birkaç farklı biçimlerde tanınan metni döndürülür. Daha fazla bilgi için bkz: [çıktı biçimi](../Concepts.md#output-format).

## <a name="related-topics"></a>İlgili konular

* [Android için İstemci Kitaplığı Başvurusu](https://github.com/Azure-Samples/Cognitive-Speech-STT-Android/tree/master/docs)
* [Microsoft konuşma API C# .NET içinde Windows kullanmaya başlama](GetStartedCSharpDesktop.md)
* [Microsoft konuşma API Objective C'de iOS kullanmaya başlama](Get-Started-ObjectiveC-iOS.md)
* [JavaScript Microsoft konuşma API'si ile çalışmaya başlama](GetStartedJSWebsockets.md)
* [Microsoft konuşma API REST üzerinden kullanmaya başlama](GetStartedREST.md)
