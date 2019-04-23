---
title: Kuruluş kavramları
titleSuffix: Language Understanding - Azure Cognitive Services
description: Büyük LUIS uygulama ve LUIS ve soru-cevap Oluşturucu birlikte dahil olmak üzere birden fazla uygulama için tasarım kavramları anlayın.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 01/09/2019
ms.author: diberry
ms.openlocfilehash: e5d7e2bfe1ee4e3ca248f40701aa65e757fc4d74
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59795511"
---
# <a name="enterprise-strategies-for-a-luis-app"></a>Bir LUIS uygulaması için Kurumsal stratejileri
Bu tasarım stratejiler Kurumsal uygulamanız için gözden geçirin.

## <a name="when-you-expect-luis-requests-beyond-the-quota"></a>Kotayı aşan LUIS istekler zaman beklediğiniz
LUIS uygulama istek oranınız izin verilen aşarsa [kota oranı](https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/), yük ile daha fazla LUIS uygulamalarına yayılan [aynı uygulama tanımı](#use-multiple-apps-with-same-app-definition) veya oluşturun ve [birden çok anahtar atama](#assign-multiple-luis-keys-to-same-app) için uygulama. 

### <a name="use-multiple-apps-with-same-app-definition"></a>Birden fazla uygulama ile aynı uygulama tanımı kullanın
Özgün LUIS uygulaması dışarı aktardıktan sonra ayrı uygulamalarda uygulama içeri aktarın. Her uygulamanın kendi uygulama kimliği vardır. Tüm uygulamalar arasında aynı anahtarı kullanarak yerine yayımladığınızda, her uygulama için ayrı bir anahtar oluşturun. Böylece hiç tek uygulama doludur tüm uygulamalar arasında yük dengeleyin. Ekleme [Application Insights](luis-tutorial-bot-csharp-appinsights.md) kullanımını izlemek için. 

Tüm uygulamalar arasında aynı üst amaç alabilmek için birinci ve ikinci amacı arasında hedefi tahmin LUIS konuşma küçük çeşitleri için uygulamalar arasında farklı sonuçlar vermek kafanız, olmadığını yetecek kadar geniş olduğundan emin olun. 

Tek bir uygulama yöneticisi olarak belirleyin. Gözden geçirme için önerilen herhangi bir konuşma ana uygulamaya eklenen ardından için tüm diğer uygulamaları geri taşınır. Uygulamanın tam bir dışarı aktarma veya etiketli konuşma asıl alt öğelerine yüklenirken budur. Yükleme araçtan yapılabilir [LUIS](luis-reference-regions.md) Web sitesi veya geliştirme API için bir [tek utterance](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c08) veya bir [batch](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c09). 

Düzenli bir zamanlama [konuşma uç noktası incelenmesi](luis-how-to-review-endpoint-utterances.md) iki haftada gibi etkin olarak öğrenmeye için daha sonra yeniden eğitme ve yeniden yayımlayın. 

### <a name="assign-multiple-luis-keys-to-same-app"></a>Birden çok LUIS anahtarları aynı uygulamaya atama
LUIS uygulamanızı daha fazla uç noktası İsabeti tek anahtarının kota izin verdiğinden, oluşturma ve daha fazla anahtarları LUIS uygulaması için atama alırsa. Bir traffic manager oluşturma veya yük dengeleyici uç nokta sorguları uç nokta anahtarlarını yönetme. 

## <a name="when-your-monolithic-app-returns-wrong-intent"></a>Tek parça uygulamanızı yanlış hedefi döndürdüğünde
Uygulamanızı çok çeşitli kullanıcı konuşma tahmin etmek için geliyorsa, uygulamayı düşünün [gönderme modeli](#dispatch-tool-and-model). Tek parça bir uygulamayı kesme LUIS üst uygulama arasında hedefleri ve alt uygulamalar arasında yanıltıcı yerine başarıyla ıntents arasında odağı algılama sağlar. 

Düzenli bir zamanlama [konuşma uç noktası incelenmesi](luis-how-to-review-endpoint-utterances.md) iki haftada gibi etkin olarak öğrenmeye için daha sonra yeniden eğitme ve yeniden yayımlayın. 

## <a name="when-you-need-to-have-more-than-500-intents"></a>500'den fazla hedefleri gerektiğinde
Örneğin, 500'den fazla amacı olan bir office Yardımcısı, geliştirmekte olduğunuz varsayalım. 200 amacı, Toplantı zamanlama için ilişkiliyse, ilgili anımsatıcılar 200 olan, iş arkadaşlarınızın hakkında bilgi alma 200 olduğundan ve e-posta göndermek için 200 olan, grubunun hedefleri böylece her grubu tek bir uygulama olarak, ardından her hedefi içeren üst düzey bir uygulama oluşturabilirsiniz. Kullanım [dağıtım aracı ve mimari](#dispatch-tool-and-model) en üst düzey uygulama oluşturmak için. Ardından botunuzun Göster olarak basamaklı çağrısı kullanmak üzere değiştirmek [gönderme Öğreticisi][dispatcher-application-tutorial]. 

## <a name="when-you-need-to-combine-several-luis-and-qna-maker-apps"></a>Çeşitli LUIS ve soru-cevap Oluşturucu uygulamaları birleştirmek gerektiğinde
Bot, kullanım için yanıtlamaları gereken birkaç LUIS ve soru-cevap Oluşturucu uygulamalar varsa [gönderme aracı](#dispatch-tool-and-model) en üst düzey uygulama oluşturmak için. Ardından botunuzun Göster olarak basamaklı çağrısı kullanmak üzere değiştirmek [gönderme Öğreticisi][dispatcher-application-tutorial]. 

## <a name="dispatch-tool-and-model"></a>Dağıtım aracı ve modeli
Kullanım [gönderme] [ dispatch-tool] bulunan komut satırı aracı [Botbuilder'da Araçları](https://github.com/Microsoft/botbuilder-tools) üst LUIS uygulaması birden çok LUIS ve/veya soru-cevap Oluşturucu uygulamaları birleştirilecek. Bu yaklaşım, tüm konular ve farklı alt konu etki alanlarını ayrı uygulamalar da dahil olmak üzere bir üst etki alanınız olanak tanır. 

![Kavramsal gönderme mimarisi görüntüsü](./media/luis-concept-enterprise/dispatch-architecture.png)

Üst etki alanını adlı bir sürümle LUIS içinde belirtilir `Dispatch` uygulamalar listesinde. 

Sohbet botu utterance alır, ardından üst LUIS uygulaması tahmin için gönderir. Üst uygulama üst tahmin edilen amacından LUIS uygulaması sonraki denir hangi alt belirler. Sohbet botu utterance daha belirli bir tahmin için alt uygulamaya gönderir.

Bu çağrı hiyerarşisini Bot Builder v4 duruma nasıl getirileceğini anlamanız [dağıtıcı uygulaması Öğreticisi][dispatcher-application-tutorial].  

### <a name="intent-limits-in-dispatch-model"></a>Gönderme modeli hedefi sınırları
Gönderme uygulama en fazla 500 gönderme kaynağı, 500 hedefleri için eşdeğer yok. 

## <a name="next-steps"></a>Sonraki adımlar

* Bilgi edinmek için nasıl [toplu test](luis-how-to-batch-test.md)

[dispatcher-application-tutorial]: https://aka.ms/bot-dispatch
[dispatch-tool]: https://aka.ms/dispatch-tool
