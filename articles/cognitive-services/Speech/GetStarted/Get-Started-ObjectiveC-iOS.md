---
title: Microsoft konuşma tanıma API'si Objective C'de iOS kullanmaya başlama | Microsoft Docs
description: Microsoft konuşma tanıma API'si konuşulan sesi metne dönüştürme iOS uygulamaları geliştirmek için kullanın.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 09/29/2017
ms.author: zhouwang
ms.openlocfilehash: bbb8d3975cdab537135b97ca9bbf6e845aa3fa0e
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352240"
---
# <a name="get-started-with-the-speech-recognition-api-in-objective-c-on-ios"></a>Konuşma tanıma API'si Objective C'de iOS kullanmaya başlama

Konuşma tanıma API'si ile konuşulan sesi metne dönüştürmek için bulut tabanlı bir konuşma hizmeti kullanan iOS uygulamaları geliştirebilirsiniz. Uygulamanız aynı anda olabilir ve hizmete ses gönderiyor aynı anda kısmi tanıma sonuçlarını zaman uyumsuz olarak almak için gerçek zamanlı akış, API destekler.

Bu makalede bir örnek uygulamanın bir iOS uygulaması geliştirmek için konuşma tanıma API'si ile çalışmaya nasıl başlayacağınız temelleri göstermek için kullanır. Tam bir API başvuru için bkz: [konuşma SDK istemci kitaplığı başvurusu](https://cdn.rawgit.com/Microsoft/Cognitive-Speech-STT-iOS/master/com.Microsoft.SpeechSDK-1_0-for-iOS.docset/Contents/Resources/Documents/index.html).

## <a name="prerequisites"></a>Önkoşullar

### <a name="platform-requirements"></a>Platform gereksinimleri

Mac XCode IDE yüklü olduğundan emin olun.

### <a name="get-the-client-library-and-examples"></a>İstemci Kitaplığı ve örnekleri alın

Konuşma istemci kitaplığı ve iOS için örnekleri kullanılabilir [konuşma istemci SDK'sı iOS için](https://github.com/microsoft/cognitive-speech-stt-ios).

### <a name="subscribe-to-the-speech-recognition-api-and-get-a-free-trial-subscription-key"></a>Konuşma tanıma API'si için abone olmak ve ücretsiz deneme aboneliği anahtarı alma

Konuşma API Bilişsel hizmetler (daha önce proje Oxford) bir parçasıdır. Ücretsiz deneme aboneliği anahtarları alma [Bilişsel hizmetler abonelik](https://azure.microsoft.com/try/cognitive-services/) sayfası. Konuşma API seçtikten sonra Seç **alma API anahtarı** anahtarı alınamadı. Birincil ve ikincil anahtar döndürür. Her iki anahtar kullanabilmek için her iki anahtarı aynı kotasını bağlıdır.

Kullanmak istiyorsanız, *amacıyla tanıma*, kaydolmak etmeniz [dil anlama akıllı hizmet (HALUK)](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/).

> [!IMPORTANT]
> * Abonelik anahtarı edinin. Konuşma istemci kitaplıkları kullanmadan önce bilmeniz gereken bir [abonelik anahtarı](https://azure.microsoft.com/try/cognitive-services/).
>
> * Abonelik anahtarınızı kullanın. Sağlanan iOS örnek uygulama ile dosya Samples/SpeechRecognitionServerExample/settings.plist abonelik anahtarınızı ile güncelleştirmeniz gerekir. Daha fazla bilgi için bkz: [derleme ve çalıştırma örnekleri](#build-and-run-samples).

## <a name="use-the-speech-client-library"></a>Konuşma istemci kitaplığını kullanma

İstemci Kitaplığı bir XCode projeye eklemek için aşağıdaki adımları [yönergeleri](https://github.com/Azure-Samples/Cognitive-Speech-STT-iOS#the-client-library).

İstemci Kitaplığı Başvurusu için iOS bulmak için bu bkz [Web sayfası](https://cdn.rawgit.com/Microsoft/Cognitive-Speech-STT-iOS/master/com.Microsoft.SpeechSDK-1_0-for-iOS.docset/Contents/Resources/Documents/index.html).

## <a name="build-and-run-samples"></a>Derleme ve çalıştırma örnekleri

Bu derleme ve çalıştırma örnekleri hakkında daha fazla bilgi için bkz [README page](https://github.com/Azure-Samples/Cognitive-Speech-STT-iOS#the-sample).

## <a name="samples-explained"></a>Açıklanan örnekleri

### <a name="create-recognition-clients"></a>Tanıma istemcileri oluşturma

Aşağıdaki örnek kodda kullanıcı senaryolarını temel alarak tanıma istemci sınıfları oluşturulacağını gösterir:

```
{
    NSString* language = @"en-us";

    NSString* path = [[NSBundle mainBundle] pathForResource:@"settings" ofType:@"plist"];
    NSDictionary* settings = [[NSDictionary alloc] initWithContentsOfFile:path];

    NSString* primaryOrSecondaryKey = [settings objectForKey:(@"primaryKey")];
    NSString* luisAppID = [settings objectForKey:(@"luisAppID")];
    NSString* luisSubscriptionID = [settings objectForKey:(@"luisSubscriptionID")];

    if (isMicrophoneReco) {
        if (!isIntent) {
            micClient = [SpeechRecognitionServiceFactory createMicrophoneClient:(recoMode)
                                                                   withLanguage:(language)
                                                                        withKey:(primaryOrSecondaryKey)
                                                                   withProtocol:(self)];
        }
        else {
            MicrophoneRecognitionClientWithIntent* micIntentClient;
            micIntentClient = [SpeechRecognitionServiceFactory createMicrophoneClientWithIntent:(language)
                                                                                        withKey:(primaryOrSecondaryKey)
                                                                                  withLUISAppID:(luisAppID)
                                                                                 withLUISSecret:(luisSubscriptionID)
                                                                                   withProtocol:(self)];
            micClient = micIntentClient;
        }
    }
    else {
        if (!isIntent) {
            dataClient = [SpeechRecognitionServiceFactory createDataClient:(recoMode)
                                                              withLanguage:(language)
                                                                   withKey:(primaryOrSecondaryKey)
                                                              withProtocol:(self)];
        }
        else {
            DataRecognitionClientWithIntent* dataIntentClient;
            dataIntentClient = [SpeechRecognitionServiceFactory createDataClientWithIntent:(language)
                                                                                   withKey:(primaryOrSecondaryKey)
                                                                             withLUISAppID:(luisAppID)
                                                                            withLUISSecret:(luisSubscriptionID)
                                                                              withProtocol:(self)];
            dataClient = dataIntentClient;
        }
    }
}

```

İstemci Kitaplığı, önceden uygulanan tanıma konuşma tanıma tipik senaryolarda istemci sınıfları sağlar:

* `DataRecognitionClient`: Konuşma tanıma verilerle PCM (örneğin, bir dosya veya ses kaynağı). Veri arabelleği ayrılır ve her arabellek konuşma hizmetine gönderilir. Kullanıcılar kendi sessizlik algılama isterseniz uygulayabilmek için hiçbir değişiklik arabellekleri için yapılır. Veri WAV dosyalarını sağlanmazsa, sunucunun dosya sağdan veriler gönderebilir. Ham verileri varsa, örneğin, ses Bluetooth üzerinden, gelen, ilk biçimi üstbilgisi izleyen verilerden sunucusuna gönderir.
* `MicrophoneRecognitionClient`: Konuşma tanıma mikrofon gelen ses ile. Mikrofon açık olduğundan ve mikrofon verilerden konuşma tanıma hizmetine gönderilir emin olun. Tanıma hizmetine gönderilmeden önce bir yerleşik "sessizlik algılayıcısı" Mikrofon verilere uygulanır.
* `DataRecognitionClientWithIntent` ve `MicrophoneRecognitionClientWithIntent`: tanıma metin ek olarak, bu istemciler uygulamalarınızı eylemler için daha fazla kullanabilir Konuşmacı amacı hakkında yapılandırılmış bilgi döndürür. "Amacı" kullanmak için öncelikle kullanarak bir modeli eğitmek gerekir [HALUK](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/).

### <a name="recognition-language"></a>Tanıma dili

Kullandığınızda `SpeechRecognitionServiceFactory` istemci oluşturmak için bir dil seçin. Konuşma hizmeti tarafından desteklenen diller tam listesi için bkz: [desteklenen diller](../API-Reference-REST/supportedlanguages.md).

### <a name="speechrecognitionmode"></a>SpeechRecognitionMode

Ayrıca belirtmek zorunda `SpeechRecognitionMode` istemciyle oluşturduğunuzda `SpeechRecognitionServiceFactory`:

* `SpeechRecognitionMode_ShortPhrase`: Bir utterance 15 kadar uzun saniye. Hizmete gönderilen veri gibi istemci birden çok kısmi sonuçlar ve birden çok n en iyi seçenek bir nihai sonucu alır.
* `SpeechRecognitionMode_LongDictation`: Bir utterance iki kadar uzun dakika. Hizmete gönderilen veri gibi istemci birden çok kısmi sonuçlar ve sunucu cümle duraklatır burada tanımlar göre birden çok son sonuçlarını alır.

### <a name="attach-event-handlers"></a>Olay işleyicileri ekleme

İstemciye çeşitli olay işleyicileri iliştirebilirsiniz oluşturduğunuz:

* **Kısmi sonuçlar olayları**: bile konuşarak bitirmeden ne size, belirten konuşma hizmet tahmin her zaman bu olayı adlı (kullanırsanız `MicrophoneRecognitionClient`) veya veri gönderme işlemini tamamladıktan (kullanıyorsanız `DataRecognitionClient`).
* **Hata olayları**: hizmet bir hata algıladığında çağrılır.
* **Hedefi olayları**: adlı istemcilerde "WithIntent" (yalnızca modunda ShortPhrase) son tanıma sonra sonuç yapılandırılmış bir JSON hedefi ayrıştırılır.
* **Neden olayları**:
  * İçinde `SpeechRecognitionMode_ShortPhrase` modu, bu olay adı verilir ve Konuşmayı bitirdikten sonra n en iyi sonuçlar verir.
  * İçinde `SpeechRecognitionMode_LongDictation` modu, olay işleyicisi çağrılır birden çok kez hizmeti cümle duraklatır burada tanımlar tabanlı.
  * **Her n en iyi seçenek**, güvenirlik değeri ve birkaç farklı biçimlerde tanınan metni döndürülür. Daha fazla bilgi için bkz: [çıktı biçimi](../Concepts.md#output-format).

## <a name="related-topics"></a>İlgili konular

* [İOS için İstemci Kitaplığı Başvurusu](https://cdn.rawgit.com/Microsoft/Cognitive-Speech-STT-iOS/master/com.Microsoft.SpeechSDK-1_0-for-iOS.docset/Contents/Resources/Documents/index.html)
* [Microsoft konuşma tanıma ve/veya hedefi Android üzerinde Java kullanmaya başlama](GetStartedJavaAndroid.md)
* [JavaScript Microsoft konuşma API'si ile çalışmaya başlama](GetStartedJSWebsockets.md)
* [Microsoft konuşma API REST üzerinden kullanmaya başlama](GetStartedREST.md)
