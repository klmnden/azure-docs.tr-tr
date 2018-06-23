---
title: Microsoft konuşma hizmet | Microsoft Docs
description: Microsoft konuşma kullanıcılarla gerçek zamanlı etkileşim dahil olmak üzere, uygulamalarınızı konuşma temelli eylemler eklemek için API kullanın.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 09/15/2017
ms.author: zhouwang
ms.openlocfilehash: c041132e992f07e94e4b6669ec7ce174f7c2d0dd
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352205"
---
# <a name="microsoft-speech-api-overview"></a>Microsoft konuşma API genel bakış

Bulut tabanlı Microsoft konuşma API geliştiricilerin güçlü konuşma etkin özellikler gibi ses komut denetimi, kullanıcı iletişim doğal konuşma konuşma ve konuşma transcription ve dikte kullanarak kendi uygulamalarında oluşturmak için kolay bir yol sağlar. Microsoft konuşma API destekler *metin konuşma* ve *metin okuma* dönüştürme.

- **Metin konuşma** API İnsan Konuşma giriş veya komutları uygulamanızı denetlemek için kullanılabilir metne dönüştürür.
- **Metin okuma** API uygulamanızı kullanıcıya çalınma ses akışları metin dönüştürür.

## <a name="speech-to-text-speech-recognition"></a>Konuşma metin (konuşma tanıma)

Microsoft konuşma tanıma API'si *transcribes* , uygulamanızın görüntülenen kullanıcıya veya olarak görev metne ses akışları giriş komutu. Konuşma için uygulamalarını eklemek geliştiriciler için iki yöntem sağlar: REST API'ler **veya** Websocket tabanlı istemci kitaplıkları.

- [REST API](GetStarted/GetStartedREST.md): geliştiriciler uygulamalarını gelen HTTP çağrıları için konuşma tanıma hizmetini kullanabilirsiniz.
- [İstemci kitaplıkları](GetStarted/GetStartedClientLibraries.md): Gelişmiş özellikler için geliştiriciler Microsoft Speech istemci kitaplıkları indirebilir ve bunların uygulamalarla bağlantı.  İstemci kitaplıkları, farklı diller (C#, Java, JavaScript, ObjectiveC) kullanarak çeşitli platformlarda (Windows, Android, iOS) kullanılabilir. REST API'leri alınırken Websocket tabanlı istemci kitaplıkları kullanın.

| Uygulama alanları | [REST API'ler](GetStarted/GetStartedREST.md) | [İstemci kitaplıkları](GetStarted/GetStartedClientLibraries.md) |
|-----|-----|-----|
| Dönüştürme kısa konuşma ses (ses uzunluğu < 15 s) Ara sonuç olmadan örneğin, komutları. | Evet | Evet |
| Uzun bir ses (> 15 s) Dönüştür | Hayır | Evet |
| İstenen Ara sonuçlarla Akış ses | Hayır | Evet |
| HALUK kullanarak ses Dönüştürülen metin anlama | Hayır | Evet |

Hangi yaklaşımın geliştiriciler (REST API veya istemci kitaplıkları) seçin, Microsoft konuşma hizmeti aşağıdakileri destekler:

- Konuşma tanıma teknolojilerini Cortana, Office dikte, Office çeviricisi ve diğer Microsoft ürünleri tarafından kullanılan Microsoft Gelişmiş.
- Gerçek zamanlı sürekli tanıma. Konuşma tanıma API'si kullanıcıların gerçek zamanlı ve o ana kadarki tanınan sözcükleri Ara sonuçlarını almak için destekler metne ses transcribe olanak tanır. Konuşma hizmeti de Konuşma sonu algılama destekler. Ayrıca, kullanıcıların büyük/küçük harf ve noktalama işaretleri, maskeleme uygunsuz metin ve metin normalleştirme gibi ek biçimlendirme özellikleri seçebilirsiniz.
- Destekler konuşma tanıma sonuçları için iyileştirilmiş *etkileşimli*, *konuşma*, ve *dikte* senaryoları. Özelleştirilmiş dil modelleri ve akustik modelleri gerektiren kullanıcı senaryoları için [özel konuşma hizmet](../custom-speech-service/cognitive-services-custom-speech-home.md) uygulamanız ve kullanıcılarınız için özel olarak hazırlanmış konuşma modelleri oluşturmanıza olanak sağlar.
- İçinde birden çok Portekizce birçok konuşulan dilleri destekler. Desteklenen dillerin her tanıma modunda tam listesi için bkz: [tanıma dillerini](api-reference-rest/supportedlanguages.md).
- Dil anlama ile tümleştirme. Giriş Ses, metne dönüştürme yanı sıra *metin konuşma* uygulamaları metni ne anlama geldiğini anlamak için ek bir yetenek sağlar. Kullandığı [dil anlama akıllı Service(LUIS)](../LUIS/Home.md) hedefleri ve varlıkları tanınan Metinden ayıklamak için.

### <a name="next-steps"></a>Sonraki adımlar

- Microsoft konuşma tanıma hizmeti ile kullanmaya başlamak [REST API'leri](GetStarted/GetStartedREST.md) veya [istemci kitaplıkları](GetStarted/GetStartedClientLibraries.md).
- Kullanıma [örnek uygulamaları](samples.md) tercih ettiğiniz programlama dili.
- Bulmak için herhangi bir bölüme gidin [Microsoft konuşma Protokolü](API-Reference-REST/websocketprotocol.md) ayrıntılarını ve API başvuru.

## <a name="text-to-speech-speech-synthesis"></a>Metin okuma (konuşma Birleştirici)

*Metin okuma* API REST bir ses akışını yapılandırılmış metin dönüştürmek için kullanın. Hızlı metin okuma dönüştürme çeşitli sesler ve dillerde API'ler sağlar. Ayrıca kullanıcılar ses özelliklerini Söyleniş, birim, aralık vb. gibi değiştirme olanağı vardır. SSML kullanarak etiketler.

### <a name="next-steps"></a>Sonraki adımlar

- Microsoft metin okuma hizmetini kullanmaya başlama: [konuşma API Başvurusu metne](api-reference-rest/bingvoiceoutput.md). Diller ve metin okuma tarafından desteklenen sesleri tam listesi için bkz: [desteklenen yerel ayarlar ve ses yazı tiplerini](api-reference-rest/bingvoiceoutput.md#SupLocales).
