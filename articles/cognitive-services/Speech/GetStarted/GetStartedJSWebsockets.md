---
title: Javascript'teki Bing konuşma tanıma API'si ile çalışmaya başlama | Microsoft Docs
titlesuffix: Azure Cognitive Services
description: Bing konuşma tanıma API'si Bilişsel hizmetler, sürekli olarak Konuşmayı metne dönüştürme uygulamalar geliştirmek için kullanın.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.subservice: bing-speech
ms.topic: article
ms.date: 09/18/2018
ms.author: zhouwang
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 17901ad40a48e9ee8d1a8b872b04ad52b75b3a52
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60515206"
---
# <a name="get-started-with-the-speech-recognition-api-in-javascript"></a>JavaScript içinde konuşma tanıma API'si ile çalışmaya başlama

[!INCLUDE [Deprecation note](../../../../includes/cognitive-services-bing-speech-api-deprecation-note.md)]

Konuşma tanıma API'sini kullanarak Konuşmayı metne dönüştürme uygulamaları geliştirebilirsiniz. JavaScript istemci Kitaplığı'nı kullanan [konuşma hizmeti WebSocket Protokolü](../API-Reference-REST/websocketprotocol.md), metin ve konuşma almanızı sağlayan aynı anda transcribed. Bu makalede javascript'teki konuşma tanıma API'si ile çalışmaya başlamanıza yardımcı olur.

## <a name="prerequisites"></a>Önkoşullar

### <a name="subscribe-to-the-speech-recognition-api-and-get-a-free-trial-subscription-key"></a>Konuşma tanıma API'si için abone olur ve ücretsiz deneme aboneliği anahtarını alma

Konuşma tanıma API'si, Bilişsel hizmetler bir parçasıdır. Ücretsiz deneme aboneliği anahtarları alabilirsiniz [Bilişsel hizmetler abonelik](https://azure.microsoft.com/try/cognitive-services/) sayfası. Konuşma tanıma API'si belirledikten sonra seçin **API anahtarı alma** anahtarını almak için. Birincil ve ikincil anahtar döndürür. İki anahtarı kullanabilmeniz için her iki anahtarı aynı kotası bağlıdır.

> [!IMPORTANT]
> Bir abonelik anahtarı edinirler. Konuşma istemci kitaplıkları kullanabilmeniz için önce olmalıdır bir [abonelik anahtarı](https://azure.microsoft.com/try/cognitive-services/).

## <a name="get-started"></a>başlarken

Bu bölümde biz, örnek bir HTML sayfası yüklemek için gereken adımlarda size yol gösterir. Örnek bulunan bizim [GitHub deposu](https://github.com/Azure-Samples/SpeechToText-WebSockets-Javascript). Yapabilecekleriniz **örnek doğrudan açmak** deposundan veya **örneği yerel bir kopyasından açın** depo.

> [!NOTE]
> Bazı tarayıcılar dönüştürmesinin güvenliği kaldırılamıyor kaynak mikrofon erişimi engelleyin. Bu nedenle, 'örneği' barındırmak için önerilir / 'uygulamanızı' bunu desteklenen tüm tarayıcılarda çalıştırmak için https.

### <a name="open-the-sample-directly"></a>Örnek doğrudan açın

Yukarıda açıklanan şekilde bir abonelik anahtarı edinin. Açılacağını [örnek bağlantı](https://htmlpreview.github.io/? https://github.com/Azure-Samples/SpeechToText-WebSockets-Javascript/blob/preview/samples/browser/Sample.html). Bu sayfayı varsayılan tarayıcınızı yükler (kullanılarak oluşturulması [htmlPreview](https://github.com/htmlpreview/htmlpreview.github.com)).

### <a name="open-the-sample-from-a-local-copy"></a>Örneği yerel bir kopyasından açın

Örnekleri yerel makineye denemek için bu depoyu Kopyala:

```
git clone https://github.com/Azure-Samples/SpeechToText-WebSockets-Javascript
```

TypeScript kaynakları Derle ve bunları tek bir JavaScript dosyasına paketleyin ([npm](https://www.npmjs.com/) makinenizde yüklü olması gerekir). Kopyalanan deponun kök ile değiştirebilir ve komutları çalıştırın:

```
cd SpeechToText-WebSockets-Javascript && npm run bundle
```

Açık `samples\browser\Sample.html` sık kullandığınız tarayıcıda.

## <a name="next-steps"></a>Sonraki adımlar

SDK'sı kendi Web eklemek hakkında daha fazla bilgi edinilebilir [burada](https://github.com/Azure-Samples/SpeechToText-WebSockets-Javascript).

## <a name="remarks"></a>Açıklamalar

- Konuşma tanıma API'si üç destekler [tanıma modları](../concepts.md#recognition-modes). Güncelleştirerek moduna geçebilirsiniz **Setup()** işlevi örnek.HTML dosyasında bulunamadı. Örnek modu ayarlar `Interactive` varsayılan olarak. Parametre modunu değiştirmek için güncelleştirme `SR.RecognitionMode.Interactive` başka bir mod. Örneğin, parametre değiştirme `SR.RecognitionMode.Conversation`.
- Desteklenen dillerin tam bir listesi için bkz. [desteklenen diller](../API-Reference-REST/supportedlanguages.md).

## <a name="related-topics"></a>İlgili konular

- [Örnek depoyu JavaScript konuşma tanıma API'si](https://github.com/Azure-Samples/SpeechToText-WebSockets-Javascript)
- [REST API'si ile çalışmaya başlama](GetStartedREST.md)
