---
title: Microsoft Bing konuşma hizmeti | Microsoft Docs
titlesuffix: Azure Cognitive Services
description: Microsoft konuşma tanıma API'si, kullanıcıların gerçek zamanlı etkileşime dahil olmak üzere uygulamalarınıza sesle yönetilen işlemler eklemek için kullanın.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.subservice: bing-speech
ms.topic: article
ms.date: 09/18/2018
ms.author: zhouwang
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: d2c7211831658a18e65e04aa753607f4eb22dac8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60515081"
---
# <a name="what-is-bing-speech"></a>Bing konuşma tanıma nedir?

[!INCLUDE [Deprecation note](../../../includes/cognitive-services-bing-speech-api-deprecation-note.md)]

Bulut tabanlı Microsoft Bing konuşma tanıma API'si, geliştiricilerin uygulamalarında, sesli komut denetimi, doğal konuşma konuşma ve konuşma transkripsiyonu ve dikte kullanarak kullanıcı iletişim gibi konuşma tanıma özellikli güçlü özellikler oluşturmak için kolay bir yol sağlar. Microsoft konuşma tanıma API'si her ikisini de destekler *Konuşmayı metne dönüştürme* ve *metin okuma* dönüştürme.

- **Konuşmayı metne dönüştürme** API İnsan konuşma uygulamanızı denetlemek için komutları veya girdi kullanılabilir metne dönüştürür.
- **Metin okuma** API uygulamanızın kullanıcıya çalınabilecek ses akışları metin dönüştürür.

## <a name="speech-to-text-speech-recognition"></a>Konuşmadan metne (konuşma tanıma)

Microsoft konuşma tanıma API'si *dönüştürür* uygulamanızı görüntülemek için kullanıcı veya olarak alacak bir metne ses akışları giriş komutu. Bu, geliştiricilerin kendi uygulamalarına Konuşma ekleme iki yol sunar: REST API'leri **veya** Websocket tabanlı istemci kitaplıkları.

- [REST API'leri](GetStarted/GetStartedREST.md): Geliştiriciler, konuşma tanıma hizmeti uygulamalarını HTTP çağrıları kullanabilir.
- [İstemci kitaplıkları](GetStarted/GetStartedClientLibraries.md): Gelişmiş özellikler için geliştiriciler Microsoft Speech istemci kitaplıklarını indirin ve kendi uygulamalarınızda bağlantı.  İstemci kitaplıkları (C#, Java, JavaScript, ObjectiveC) farklı dilleri kullanan çeşitli platformlarda (Windows, Android, iOS) kullanılabilir. REST API'ler farklı olarak, istemci kitaplıkları Websocket tabanlı protokolü kullanır.

| Uygulama alanları | [REST API'ler](GetStarted/GetStartedREST.md) | [İstemci kitaplıkları](GetStarted/GetStartedClientLibraries.md) |
|-----|-----|-----|
| Dönüştürme kısa konuşmada geçen bir ses (ses uzunluğu < 15 s) geçiş sonuçları gibi komutları. | Evet | Evet |
| Uzun bir ses (> 15 s) dönüştürme | Hayır | Evet |
| İstenen Ara sonuçlarla Stream ses | Hayır | Evet |
| LUIS kullanarak seslerden Dönüştürülen metin anlama | Hayır | Evet |

Hangi yaklaşımın geliştiriciler (REST API veya istemci kitaplıkları) seçin, Microsoft konuşma hizmeti aşağıdakileri destekler:

- Konuşma tanıma teknolojileri Cortana, dikte Office, Office Translator ve diğer Microsoft ürünleri tarafından kullanılan Microsoft tarafından sunulan Gelişmiş.
- Gerçek zamanlı sürekli tanıma. Konuşma tanıma API'si, kullanıcıların gerçek zamanlı ve destekler şimdiye tanınan bir kelimelerin Ara sonuçlar almak için metne ses özelliği sağlar. Konuşma hizmeti, konuşma uç algılama da destekler. Ayrıca, kullanıcılar, büyük/küçük harf ve noktalama işaretleri ve maskeleme küfür metin normalleştirme gibi ek biçimlendirme özellikleri seçebilirsiniz.
- Destekler, konuşma tanıma sonuçları için iyileştirilmiş *etkileşimli*, *konuşma*, ve *dikte* senaryoları. Özel dil modelleri ve akustik modeller gerektiren kullanıcı senaryoları için [özel konuşma hizmeti](../custom-speech-service/cognitive-services-custom-speech-home.md) uygulamanız ve kullanıcılarınız için uyarlanmış konuşma modelleri oluşturmanıza olanak sağlar.
- Birçok konuşulan dili içinde birden çok diyalektleri destekler. Her tanıma modunda desteklenen dillerin tam listesi için bkz. [tanıma diller](api-reference-rest/supportedlanguages.md).
- Language understanding ile tümleştirme. Giriş Sesi metne dönüştürme yanı sıra *Konuşmayı metne dönüştürme* uygulamaları metin ne anlama geldiğini anlamak için ek bir özellik sağlar. Kullandığı [Language Understanding Intelligent Service(LUIS)](../LUIS/what-is-luis.md) amaç ve varlıkları tanınan ından cümleler ayıklamak için.

### <a name="next-steps"></a>Sonraki adımlar

- Microsoft konuşma tanıma hizmeti ile kullanmaya başlama [REST API'leri](GetStarted/GetStartedREST.md) veya [istemci kitaplıkları](GetStarted/GetStartedClientLibraries.md).
- Kullanıma [örnek uygulamalar](samples.md) tercih ettiğiniz programlama dili.
- Bulunacak başvuru bölümüne Git [Microsoft konuşma Protokolü](API-Reference-REST/websocketprotocol.md) ayrıntıları ve API başvuruları.

## <a name="text-to-speech-speech-synthesis"></a>Metin okuma (konuşma sentezi)

*Metin okuma* API'lerini yapılandırılmış metin bir ses akışına dönüştürmek için REST kullanma. API'leri, çeşitli seslerle ve dilleri hızlı metinleri konuşmaya dönüştürme sağlar. Ayrıca kullanıcılar da telaffuz, birim, aralık vb. gibi ses özelliklerini değiştirmek için sahipsiniz. SSML'yi kullanarak etiketler.

### <a name="next-steps"></a>Sonraki adımlar

- Microsoft metin okuma hizmetini kullanmaya başlayın: [Metin okuma API Başvurusu](api-reference-rest/bingvoiceoutput.md). Diller ve seslerle metin okuma tarafından desteklenen tam listesi için bkz: [desteklenen yerel ayarlar ve ses tiplerini](api-reference-rest/bingvoiceoutput.md#SupLocales).
