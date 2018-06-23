---
title: JavaScript Microsoft konuşma tanıma API'si ile çalışmaya başlama | Microsoft Docs
description: Microsoft konuşma tanıma API'si Bilişsel Hizmetleri'nde sürekli konuşulan sesi metne dönüştürme uygulamaları geliştirmek için kullanın.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 12/21/2017
ms.author: zhouwang
ms.openlocfilehash: 56c41fd7f6a00d80bc6bccd61894654e057e926e
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352187"
---
# <a name="get-started-with-the-speech-recognition-api-in-javascript"></a>JavaScript konuşma tanıma API'si ile çalışmaya başlama

Konuşma tanıma API'si kullanılarak konuşulan sesi metne dönüştürme uygulamaları geliştirebilirsiniz. JavaScript istemci kitaplığını kullanan [konuşma hizmet WebSocket Protokolü](../API-Reference-REST/websocketprotocol.md), metin aynı anda transcribed konuşun ve almasına olanak sağlar. Bu makalede, JavaScript konuşma tanıma API'si ile çalışmaya başlamak için yardımcı olur.

## <a name="prerequisites"></a>Önkoşullar

### <a name="subscribe-to-the-speech-recognition-api-and-get-a-free-trial-subscription-key"></a>Konuşma tanıma API'si için abone olmak ve ücretsiz deneme aboneliği anahtarı alma

Konuşma API Bilişsel hizmetler bir parçasıdır. Ücretsiz deneme aboneliği anahtarları alma [Bilişsel hizmetler abonelik](https://azure.microsoft.com/try/cognitive-services/) sayfası. Konuşma API seçtikten sonra Seç **alma API anahtarı** anahtarı alınamadı. Birincil ve ikincil anahtar döndürür. Her iki anahtar kullanabilmek için her iki anahtarı aynı kotasını bağlıdır.

> [!IMPORTANT]
> Abonelik anahtarı edinin. Konuşma istemci kitaplıkları kullanmadan önce bilmeniz gereken bir [abonelik anahtarı](https://azure.microsoft.com/try/cognitive-services/).

## <a name="get-started"></a>başlarken

Bu bölümde biz, örnek bir HTML sayfası yüklemek için gereken adımlarda size yol gösterir. Örnek bulunan bizim [github deposunu](https://github.com/Azure-Samples/SpeechToText-WebSockets-Javascript). Yapabilecekleriniz **örnek doğrudan açmak** depodan veya **yerel bir kopyasından örneği açın** depo. 

> [!NOTE]
> Bazı tarayıcılar dönüştürmesinin güvenliği kaynağındaki mikrofon erişimini engelleyin. Bu nedenle, 'örneği' barındırmak için önerilir / 'uygulamanıza' bunu tüm desteklenen tarayıcılarda çalıştırmak için https. 

### <a name="open-the-sample-directly"></a>Örnek doğrudan açın

Yukarıda açıklandığı gibi bir abonelik anahtarı edinin. Ardından açın [örnek bağlantı](https://htmlpreview.github.io/?https://github.com/Azure-Samples/SpeechToText-WebSockets-Javascript/blob/preview/samples/browser/Sample.html). Bu sayfa varsayılan tarayıcınıza yükler (kullanılarak oluşturulması [htmlPreview](https://github.com/htmlpreview/htmlpreview.github.com)).

### <a name="open-the-sample-from-a-local-copy"></a>Yerel bir kopyasından örneği açın

Örnek yerel olarak denemek için bu depoyu kopyalayın:

```
git clone https://github.com/Azure-Samples/SpeechToText-WebSockets-Javascript
```

TypeScript kaynakları ve paket/browserfy derleme bunları tek bir JavaScript dosyası içine ([npm](https://www.npmjs.com/) makinenize yüklü olması gerekir). Kopyalanan depo kök değiştirin ve komutları çalıştırın:

```
cd SpeechToText-WebSockets-Javascript && npm run bundle
```

Açık `samples\browser\Sample.html` en sevdiğiniz tarayıcınızda.

## <a name="next-steps"></a>Sonraki adımlar

Kendi Web sayfası SDK'sı eklenip hakkında daha fazla bilgi kullanılabilir [burada](https://github.com/Azure-Samples/SpeechToText-WebSockets-Javascript).

## <a name="remarks"></a>Açıklamalar

- Konuşma tanıma API'si üç destekleyen [tanıma modları](../concepts.md#recognition-modes). Güncelleştirerek modu geçebilirsiniz **Setup()** işlevi örnek.HTML dosyasında bulunamadı. Örnek modunu ayarlar `Interactive` varsayılan olarak. Modunu değiştirmek için parametre güncelleştirme `SR.RecognitionMode.Interactive` başka bir mod. Örneğin, parametre değiştirme `SR.RecognitionMode.Conversation`.
- Desteklenen diller tam bir listesi için bkz: [desteklenen diller](../API-Reference-REST/supportedlanguages.md).

## <a name="related-topics"></a>İlgili konular

- [JavaScript konuşma tanıma API'si örnek depo](https://github.com/Azure-Samples/SpeechToText-WebSockets-Javascript)
- [REST API'si ile çalışmaya başlama](GetStartedREST.md)
