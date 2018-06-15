---
title: HALUK app - Azure kuruluş kavramları | Microsoft Docs
description: Büyük HALUK uygulamaları için tasarım kavramları anlayın.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/05/2018
ms.author: v-geberr
ms.openlocfilehash: dde7012dee0eb5ea3ac2e1257cb8d2fca5843d4b
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35356490"
---
# <a name="enterprise-strategies-for-a-luis-app"></a>HALUK uygulama için Kurumsal stratejileri
Bu tasarım stratejileri kuruluş uygulamanız için gözden geçirin.

## <a name="when-you-expect-luis-requests-beyond-the-quota"></a>Kota ötesinde HALUK isteklerini zaman beklediğiniz
HALUK uygulama isteği hızı izin verilen aşarsa [kota oranı](https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/), daha fazla HALUK uygulamalarla yükü yayılan [aynı uygulama tanımı](#use-multiple-apps-with-same-app-definition) veya oluşturun ve [birden çok anahtar atama](#assign-multiple-luis-keys-to-same-app) için uygulama. 

### <a name="use-multiple-apps-with-same-app-definition"></a>Birden çok uygulama aynı uygulama tanımıyla kullanın
Özgün HALUK uygulama verin, sonra geri ayrı uygulamalarda uygulama içeri aktarın. Her uygulamanın kendi uygulama kimliği vardır. Tüm uygulamalar arasında aynı anahtarı kullanarak yerine yayımladığınızda, her uygulama için ayrı bir anahtar oluşturun. Böylece hiç tek uygulama doludur tüm uygulamalar arasında Yük Dengelemesi. Ekleme [Application Insights](luis-tutorial-bot-csharp-appinsights.md) kullanımını izlemek için. 

Tüm uygulamalar arasında aynı üst hedefi alabilmek için birinci ve ikinci hedefi arasındaki hedefi tahmin HALUK utterances küçük Çeşitlemeler uygulamalar arasında farklı sonuçlar vermiş kafası, olmadığını yetecek kadar geniş olduğundan emin olun. 

Tek bir uygulama yöneticisi olarak belirleyin. Gözden geçirme için önerilen utterances ana uygulamaya eklenen sonra için tüm diğer uygulamaları geri taşınır. Uygulamanın tam verme ya da etiketli utterances yöneticisinden alt öğelerine yüklenirken budur. Yükleme herhangi birinden yapılabilir [HALUK] [ LUIS] Web sitesi ya da geliştirme API'si bir [tek utterance](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c08) veya bir [toplu](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c09). 

Düzenli bir zamanlama [endpoint utterances incelenmesi](label-suggested-utterances.md) iki haftada gibi etkin öğrenme için daha sonra yeniden eğitme ve yeniden yayımlayın. 

### <a name="assign-multiple-luis-keys-to-same-app"></a>Birden çok HALUK aynı uygulamasına tuşları atama
HALUK uygulamanızı isabetler tek anahtarın kota izin verdiğinden, oluşturma ve HALUK uygulamaya daha fazla tuşları atama daha fazla uç noktası alırsa. Trafik Yöneticisi oluşturma veya yük dengeleyici uç noktası sorguları abonelik anahtarlarını yönetme. 

## <a name="when-your-monolithic-app-returns-wrong-intent"></a>Tek yapılı uygulamanızı yanlış hedefi döndüğünde
Çok çeşitli kullanıcı utterances tahmin etmek için uygulamanızın istediyseniz uygulayın [gönderme modeli](#dispatch-tool-and-model). Tek yapılı bir uygulamasını kurup yeni odak algılama üst uygulama arasında amaçları ve alt uygulamalar arasında kafası yerine başarıyla hedefleri arasında HALUK sağlar. 

Düzenli bir zamanlama [endpoint utterances incelenmesi](label-suggested-utterances.md) iki haftada gibi etkin öğrenme için daha sonra yeniden eğitme ve yeniden yayımlayın. 

## <a name="when-you-need-to-have-more-than-500-intents"></a>500'den fazla hedefleri olması gerektiğinde
Örneğin, 500'den fazla hedefleri sahip bir office Yardımcısı geliştirirken varsayalım. 200 hedefleri toplantılar zamanlamaya ilişkiliyse, 200 anımsatıcıları hakkında iş arkadaşlarınızı hakkında bilgi alma 200 olan ve e-posta göndermek için 200 olan, her grubun tek bir uygulamada, Grup hedefleri sonra her hedefi içeren üst düzey bir uygulama oluşturmak. Kullanım [gönderme aracı ve mimari](#dispatch-tool-and-model) en üst düzey uygulamanızı oluşturmak için. Geçişli çağrısı Göster olarak kullanmak için bot değiştirme [gönderme öğretici][dispatcher-application-tutorial]. 

## <a name="when-you-need-to-combine-several-luis-and-qna-maker-apps"></a>Birkaç HALUK ve QnA maker uygulamalar birleştirmek gerektiğinde
Bot, kullanım için yanıtlamaları gereken birkaç HALUK ve QnA maker uygulamalar varsa [gönderme aracı](#dispatch-tool-and-model) en üst düzey uygulamanızı oluşturmak için. Geçişli çağrısı Göster olarak kullanmak için bot değiştirme [gönderme öğretici][dispatcher-application-tutorial]. 

## <a name="dispatch-tool-and-model"></a>Dağıtım aracı ve modeli
Kullanım [gönderme] [ dispatch-tool] bulunan komut satırı aracı [BotBuilder Araçları](https://github.com/Microsoft/botbuilder-tools) üst HALUK uygulama birden çok HALUK ve/veya QnA Maker uygulamaları birleştirmek için. Bu yaklaşım, tüm konular ve farklı alt konu etki alanlarını ayrı uygulamalar dahil olmak üzere bir üst etki alanına sahip olanak sağlar. 

![Kavramsal görüntü gönderme mimarisi](./media/luis-concept-enterprise/dispatch-architecture.png)

Üst etki alanı HALUK belirtilmiş bir **V gönderme** uygulama. 

![Gönderme aracı tarafından oluşturulan HALUK uygulamayla ekran görüntüsü, HALUK uygulamalar listesi](./media/luis-concept-enterprise/dispatch.png)

Chatbot utterance alır, ardından HALUK app tahmini için üst gönderir. Üst uygulama üst tahmin edilen amaçtan HALUK uygulama sonraki adlı hangi alt belirler. Chatbot daha belirli bir tahmini için alt uygulaması utterance gönderir.

Bu hiyerarşi çağrılarının Bot Oluşturucu v4 duruma nasıl getirileceğini anlamanız [dağıtıcısı uygulaması Öğreticisi][dispatcher-application-tutorial].  

## <a name="next-steps"></a>Sonraki adımlar

* Bilgi edinmek için nasıl [toplu test](luis-how-to-batch-test.md)

[LUIS]:luis-reference-regions.md
[dispatcher-application-tutorial]:https://aka.ms/bot-dispatch
[dispatch-tool]:https://github.com/Microsoft/botbuilder-tools/tree/master/Dispatch