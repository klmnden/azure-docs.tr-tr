---
title: Bing konuşma tanıma API'si Objective C'de iOS kullanmaya başlayın | Microsoft Docs
titlesuffix: Azure Cognitive Services
description: Bing konuşma tanıma API'si, Konuşmayı metne dönüştürme iOS uygulamaları geliştirmek için kullanın.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.subservice: bing-speech
ms.topic: article
ms.date: 09/18/2018
ms.author: zhouwang
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 09b7e8961e59bd6fad49408c28e9ee9a4a209cae
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60515286"
---
# <a name="quickstart-use-the-bing-speech-recognition-api-in-objective-c-on-ios"></a>Hızlı Başlangıç: Bing konuşma tanıma API'si, iOS üzerinde Objective-C içinde kullanın

[!INCLUDE [Deprecation note](../../../../includes/cognitive-services-bing-speech-api-deprecation-note.md)]

Konuşma tanıma API'SİYLE konuşma kayıtlarını metne dönüştürmek için bulut tabanlı konuşma hizmeti kullanan iOS uygulamaları geliştirebilirsiniz. Uygulamanız aynı anda olabilir ve zaman uyumsuz olarak hizmete ses gönderiyor aynı anda kısmi tanıma sonuçları almak için gerçek zamanlı akış, API'yi destekler.

Bu makalede, bir iOS uygulaması geliştirmeye yönelik Konuşma tanıma API'si ile çalışmaya başlama ilişkin temel bilgileri göstermek için örnek uygulamayı kullanır. Bir tam API Başvurusu için bkz. [Speech SDK'sı istemci kitaplığı başvurusu](https://cdn.rawgit.com/Microsoft/Cognitive-Speech-STT-iOS/master/com.Microsoft.SpeechSDK-1_0-for-iOS.docset/Contents/Resources/Documents/index.html).

## <a name="prerequisites"></a>Önkoşullar

### <a name="platform-requirements"></a>Platform gereksinimleri

Mac XCode IDE'ın yüklü olduğundan emin olun.

### <a name="get-the-client-library-and-examples"></a>İstemci Kitaplığı ve örnek alma

İOS için örnekler ve konuşma istemcisi kitaplığını kullanılabilir [konuşma istemci SDK'sı, iOS için](https://github.com/microsoft/cognitive-speech-stt-ios).

### <a name="subscribe-to-the-speech-recognition-api-and-get-a-free-trial-subscription-key"></a>Konuşma tanıma API'si için abone olur ve ücretsiz deneme aboneliği anahtarını alma

Konuşma tanıma API'si, Bilişsel hizmetler (daha önce Project Oxford) bir parçasıdır. Ücretsiz deneme aboneliği anahtarları alabilirsiniz [Bilişsel hizmetler abonelik](https://azure.microsoft.com/try/cognitive-services/) sayfası. Konuşma tanıma API'si belirledikten sonra seçin **API anahtarı alma** anahtarını almak için. Birincil ve ikincil anahtar döndürür. İki anahtarı kullanabilmeniz için her iki anahtarı aynı kotası bağlıdır.

Kullanmak istiyorsanız *tanıma amacıyla*, ayrıca kaydolmanız gerekir [Language Understanding Intelligent Service (LUIS)](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/).

> [!IMPORTANT]
> * Bir abonelik anahtarı edinirler. Konuşma istemci kitaplıkları kullanabilmeniz için önce olmalıdır bir [abonelik anahtarı](https://azure.microsoft.com/try/cognitive-services/).
>
> * Abonelik anahtarınızı kullanın. Sağlanan iOS örnek uygulama ile birlikte ' % s'dosyası Samples/SpeechRecognitionServerExample/settings.plist abonelik anahtarınız ile güncelleştirmeniz gerekir. Daha fazla bilgi için [derleme ve çalıştırma örnekleri](#build-and-run-samples).

## <a name="use-the-speech-client-library"></a>Konuşma istemci kitaplığını kullanma

İstemci Kitaplığı bir XCode projesine eklemek için aşağıdaki adımları [yönergeleri](https://github.com/Azure-Samples/Cognitive-Speech-STT-iOS#the-client-library).

İstemci Kitaplığı Başvurusu için iOS bulmak için bkz [Web sayfası](https://cdn.rawgit.com/Microsoft/Cognitive-Speech-STT-iOS/master/com.Microsoft.SpeechSDK-1_0-for-iOS.docset/Contents/Resources/Documents/index.html).

## <a name="build-and-run-samples"></a>Derleme ve örneklerini çalıştırma

Derleme ve çalıştırma örnekleri hakkında daha fazla bilgi için bkz. Bu [Benioku sayfa](https://github.com/Azure-Samples/Cognitive-Speech-STT-iOS#the-sample).

## <a name="samples-explained"></a>Açıklanan örnekleri

### <a name="create-recognition-clients"></a>Tanıma istemcileri oluşturma

Aşağıdaki kod örneğinde, kullanıcı senaryoları tabanlı tanıma istemci sınıfları oluşturma işlemi gösterilmektedir:

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

İstemci Kitaplığı önceden uygulanan tanıma, konuşma tanıma tipik senaryolar için istemci sınıfları sağlar:

* `DataRecognitionClient`: Konuşma tanıma PCM verileri (örneğin, bir dosya ya da ses kaynağından gelen) ile. Veri arabellekleri ayrılmıştır ve her arabellek konuşma hizmeti için gönderilir. Kullanıcılar kendi sessizlik algılama isterseniz uygulayabilmek için hiçbir değişiklik arabellekleri için gerçekleştirilir. Veri WAV dosyalarını sağlanmazsa, sunucunun dosya sağdan veriler gönderebilir. Ham verileri varsa, örneğin, Bluetooth üzerinden gelen ses, ilk biçimi üstbilgi verileri sunucusuna gönderir.
* `MicrophoneRecognitionClient`: Konuşma tanıma mikrofondan gelen sesi ile. Mikrofon açık ve mikrofon verileri için konuşma tanıma hizmeti gönderilir emin olun. Tanıma hizmetine gönderilmeden önce bir yerleşik "sessizlik algılayıcısı" Mikrofon verilere uygulanır.
* `DataRecognitionClientWithIntent` ve `MicrophoneRecognitionClientWithIntent`: Tanıma metne ek olarak, bu istemciler, uygulamaları daha da seslerini için kullanabilir konuşmacının amacı hakkında yapılandırılmış bilgiler döndürür. "Hedefi" kullanmak için önce kullanarak bir model eğitip gerekir [LUIS](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/).

### <a name="recognition-language"></a>Tanıma dili

Kullanırken `SpeechRecognitionServiceFactory` istemcisi oluşturmak için bir dil seçmeniz gerekir. Konuşma hizmeti tarafından desteklenen dillerin tam listesi için bkz. [desteklenen diller](../API-Reference-REST/supportedlanguages.md).

### <a name="speechrecognitionmode"></a>SpeechRecognitionMode

Ayrıca belirtmenize gerek `SpeechRecognitionMode` istemciyle oluşturduğunuzda `SpeechRecognitionServiceFactory`:

* `SpeechRecognitionMode_ShortPhrase`: Uzun bir utterance fazla 15 saniye. Hizmetine gönderilen veri gibi istemci, birden çok kısmı sonuç ve birden çok en iyi n seçim ile bir nihai sonucu alır.
* `SpeechRecognitionMode_LongDictation`: Uzun bir utterance en fazla iki dakika. Hizmetine gönderilen veri gibi istemci, birden çok kısmı sonuç ve birden çok Nihai sonuç, sunucunun cümle duraklamaları burada tanımlar temel alır.

### <a name="attach-event-handlers"></a>Olay işleyicileri ekleme

İstemciye çeşitli olay işleyicileri ekleyebilirsiniz, oluşturan:

* **Kısmi sonuçlar olayları**: Bile Konuşmayı bitirmeden kişilerin, yorumlarını, konuşma tanıma hizmeti tahmin her başlatıldığında bu olay adlı (kullanırsanız `MicrophoneRecognitionClient`) veya veri gönderen son (kullanırsanız `DataRecognitionClient`).
* **Hata olayları**: Hizmet bir hata algıladığında çağrılır.
* **Hedefi olayları**: Adlı istemcilerde "WithIntent" (yalnızca modunda ShortPhrase) son tanıma sonra sonuç yapılandırılmış bir JSON hedefi ayrıştırılır.
* **Neden olayların**:
  * İçinde `SpeechRecognitionMode_ShortPhrase` modu, bu olay adı verilir ve konuşma tamamladıktan sonra en iyi n sonuçlarını döndürür.
  * İçinde `SpeechRecognitionMode_LongDictation` modu, olay işleyicisi adlı birden çok kez bağlı hizmeti cümle duraklamaları burada tanımlar.
  * **Her en iyi n seçim**, güvenirlik değeri ve tanınan metin birkaç farklı biçimleri döndürülür. Daha fazla bilgi için [çıkış biçimi](../Concepts.md#output-format).

## <a name="related-topics"></a>İlgili konular

* [İOS istemci Kitaplığı Başvurusu](https://cdn.rawgit.com/Microsoft/Cognitive-Speech-STT-iOS/master/com.Microsoft.SpeechSDK-1_0-for-iOS.docset/Contents/Resources/Documents/index.html)
* [Microsoft konuşma tanıma ve/veya Android üzerinde Java amaca kullanmaya başlama](GetStartedJavaAndroid.md)
* [JavaScript içinde Microsoft konuşma tanıma API'si ile çalışmaya başlama](GetStartedJSWebsockets.md)
* [REST aracılığıyla Microsoft konuşma tanıma API'si ile çalışmaya başlama](GetStartedREST.md)
