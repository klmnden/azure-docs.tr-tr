---
title: Azure'da dil anlama (HALUK) sık sorulan sorular | Microsoft Docs
description: Dil anlama (HALUK) hakkında sık sorulan soruların yanıtlarını alın
author: v-geberr
manager: kaiqb
services: cognitive-services
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: v-geberr
ms.openlocfilehash: b6b333937e7c88f566fc401967b26cbd31ca158b
ms.sourcegitcommit: ea5193f0729e85e2ddb11bb6d4516958510fd14c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2018
ms.locfileid: "36301511"
---
# <a name="language-understanding-faq"></a>Dil anlama SSS

Bu makalede dil anlama (HALUK) hakkında sık sorulan soruların yanıtlarını içerir.

## <a name="luis-authoring"></a>HALUK yazma

### <a name="what-are-the-luis-best-practices"></a>HALUK en iyi uygulamalar nelerdir? 
İle başlayan [geliştirme döngüsü](luis-concept-app-iteration.md), ardından okuma [en iyi uygulamalar](luis-concept-best-practices.md). 

### <a name="what-is-the-best-way-to-start-building-my-app-in-luis"></a>Uygulamam HALUK içinde oluşturmaya başlamak için en iyi yolu nedir?

Uygulamanızı oluşturmak için en iyi yolu aracılığıyladır bir [artımlı işlem](luis-concept-app-iteration.md). 

### <a name="what-is-a-good-practice-to-model-the-intents-of-my-app-should-i-create-more-specific-or-more-generic-intents"></a>Uygulamam hedefleri modellemek için iyi bir uygulama nedir? Daha belirgin veya daha genel hedefleri oluşturmanız gerekir?

Çakışan, ancak bu nedenle özel olmayan, benzer hedefleri arasında ayrım yapmak HALUK zorlaştırır emin olacak şekilde genel olmayan hedefleri seçin. Discriminative belirli hedefleri oluşturma modelleme HALUK için en iyi uygulamaları biridir.

### <a name="is-it-important-to-train-the-none-intent"></a>Hiçbiri hedefi eğitmek önemli mi?

Evet, eğitmek yararlı olan, **hiçbiri** diğer amaçlar için daha fazla etiket ekleme gibi daha fazla utterances ile hedefi. Eklenen 1 veya 2 etiketleri iyi orandır **hiçbiri** yönelik bir amacı eklenen her 10 etiketleri için. Bu oran HALUK discriminative gücünü artırır.

### <a name="how-can-i-correct-spelling-mistakes-in-utterances"></a>Yazım utterances içinde nasıl düzeltmek için?

Bkz: [Bing yazım denetleme API V7](luis-tutorial-bing-spellcheck.md) Öğreticisi. Bing yazım denetleme API V7 tarafından uygulanan sınırları HALUK zorlar. 

### <a name="how-do-i-edit-my-luis-app-programmatically"></a>Nasıl HALUK Uygulamam program aracılığıyla Düzenle?
Program aracılığıyla HALUK uygulamanızı düzenlemek için kullanın [yazma API](https://aka.ms/luis-authoring-apis). Bkz: [API yazma HALUK çağrısı](./luis-quickstart-node-add-utterance.md) ve [program aracılığıyla Node.js kullanarak HALUK uygulaması oluşturma](./luis-tutorial-node-import-utterances-csv.md) yazma API'sini çağırmak nasıl örnekleri için. Yazma API kullanmanızı gerektirir bir [anahtar yazma](luis-concept-keys.md#authoring-key) yerine bir uç noktası anahtarı. Programsal yazma ay başına en fazla 1.000.000 çağrı ve saniye başına beş işlemler sağlar. HALUK ile kullandığınız anahtarları hakkında daha fazla bilgi için bkz: [anahtarları Yönet](./luis-concept-keys.md).

### <a name="where-is-the-pattern-feature-that-provided-regular-expression-matching"></a>Burada sağlanan normal ifade deseni özelliği eşleşiyor mu?
Önceki **deseni özelliği** şu anda kullanım dışı olan, değiştirilmiştir  **[desenleri](luis-concept-patterns.md)**. 

### <a name="how-do-i-use-an-entity-to-pull-out-the-correct-data"></a>Doğru veri çıkarmak için bir varlık nasıl kullanırım? 
Bkz: [varlıklar](luis-concept-entity-types.md) ve [veri ayıklama](luis-concept-data-extraction.md).

### <a name="should-variations-of-an-example-utterance-include-punctuation"></a>Bir örnek utterance çeşitlemelerini noktalama işaretlerini dahil edilsin mi? 
Amaç için örnek utterances olarak farklı Çeşitlemeler ekleyebilir veya ile örnek utterance deseni ekleme [yoksaymak için sözdizimi](luis-concept-patterns.md#pattern-syntax) noktalama işareti. 

## <a name="luis-endpoint"></a>HALUK uç noktası

### <a name="why-does-luis-add-spaces-to-the-query-around-or-in-the-middle-of-words"></a>Neden HALUK alanları geçici veya sözcükler ortasında sorguya ekler?
HALUK [tokenizes](luis-glossary.md#token) utterance temel alarak [kültür](luis-supported-languages.md#tokenization). Özgün değeri ve parçalanmış değeri kullanılabilir [veri ayıklama](luis-concept-data-extraction.md#tokenized-entity-returned).

### <a name="how-do-i-create-and-assign-a-luis-endpoint-key"></a>Nasıl oluşturmak ve bir HALUK uç noktası anahtarı atama?
[Uç noktası anahtarı oluşturma](luis-how-to-azure-subscription.md#create-luis-endpoint-key) için azure'da, [hizmet](https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/) düzeyi. [Anahtar atama](Manage-keys.md#assign-endpoint-key) üzerinde **[Yayımla](publishapp.md)** sayfası. Bu eylem için karşılık gelen hiçbir API yoktur. Uç noktası için HTTP isteği değiştirmeli sonra [yeni uç nokta anahtarı kullanmak](luis-concept-keys.md#use-endpoint-key-in-query).

### <a name="how-do-i-interpret-luis-scores"></a>HALUK puanları nasıl yorumlanacağı? 
Sisteminizin değerini bakılmaksızın yüksek Puanlama amacıyla kullanmanız gerekir. Örneğin, bir puan 0,5 (% 50'den) aşağıda mutlaka HALUK düşük güvenilirlik olduğunu gelmez. Daha fazla eğitim verileri sağlayan, olasılıkla hedefinin puan artırmaya yardımcı olabilir.

### <a name="why-dont-i-see-my-endpoint-hits-in-my-apps-dashboard"></a>My uç noktası isabet my uygulamanın panosunda neden göremiyorum?
Uygulamanızın panosunda toplam uç noktası isabet düzenli olarak güncelleştirilir, ancak HALUK abonelik anahtarınızı Azure portalında ilişkili ölçümleri daha sık güncelleştirilir. 

Güncelleştirilmiş uç noktası isabet panosunda görmüyorsanız, Azure portalında oturum açın ve HALUK abonelik anahtarınızı ile ilişkili kaynak bulmak ve açmak **ölçümleri** seçmek için **toplam çağrı sayısı** ölçüm. Abonelik anahtarı için birden fazla HALUK uygulaması kullandıysanız, Azure portalında ölçüm, bunu kullanan tüm HALUK uygulamalar gelen çağrıları toplam sayısını gösterir.

### <a name="my-luis-app-was-working-yesterday-but-today-im-getting-403-errors-i-didnt-change-the-app-how-do-i-fix-it"></a>Dün HALUK Uygulamam çalıştığı ancak bugün 403 hataları alıyorum. Uygulama değişiklik olmadı. Bunu nasıl düzeltirim? 
Aşağıdaki [yönergeleri](#how-do-i-create-and-assign-a-luis-endpoint-key) HALUK uç noktası anahtarı oluşturmak ve uygulamaya atamak için sonraki SSS. Uç noktası için HTTP isteği değiştirmeli sonra [yeni uç nokta anahtarı kullanmak](luis-concept-keys.md#use-endpoint-key-in-query).

### <a name="how-do-i-secure-my-luis-endpoint"></a>HALUK Noktam güvenliğini nasıl sağlayabilirim? 
Bkz: [uç nokta güvenliğini sağlama](luis-concept-security.md#securing-the-endpoint).

## <a name="working-within-luis-limits"></a>HALUK sınırları içinde çalışma

### <a name="what-is-the-maximum-number-of-intents-and-entities-that-a-luis-app-can-support"></a>Hedefleri ve HALUK uygulama destekleyebilir varlıkları maksimum sayısı nedir?
Bkz: [sınırları](luis-boundaries.md) başvuru.

### <a name="i-want-to-build-a-luis-app-with-more-than-the-maximum-number-of-intents-what-should-i-do"></a>Hedefleri üst sınırını aşan bir HALUK uygulaması oluşturmak istiyor. Ne yapmalıyım?

Bkz: [amaçlar için en iyi uygulamaları](luis-concept-intent.md#if-you-need-more-than-the-maximum-number-of-intents).

### <a name="i-want-to-build-an-app-in-luis-with-more-than-the-maximum-number-of-entities-what-should-i-do"></a>HALUK bir uygulamada birden çok varlık sayısı ile oluşturmak istiyorsunuz. Ne yapmalıyım?

Bkz: [varlıklar için en iyi uygulamaları](luis-concept-entity-types.md#if-you-need-more-than-the-maximum-number-of-entities)

### <a name="what-are-the-limits-on-the-number-and-size-of-phrase-lists"></a>Listeler sayısına ve tümcecik boyutunu sınırları nelerdir?
En büyük uzunluğu için bir [tümcecik listesi](./luis-concept-feature.md), bkz: [sınırları](luis-boundaries.md) başvuru.

### <a name="what-are-the-limits-on-example-utterances"></a>Örnek utterances sınırları nelerdir?
Bkz: [sınırları](luis-boundaries.md) başvuru.

## <a name="testing-and-training"></a>Test ve eğitim

### <a name="i-see-some-errors-in-the-batch-testing-pane-for-some-of-the-models-in-my-app-how-can-i-address-this-problem"></a>Bazı hatalar Uygulamam modellerinde bazıları bölmesinde test toplu görüyorum Bu sorunu nasıl ele alabilir?

Hataları, etiketler ve tahminleri sizin modellerinden arasında bazı tutarsızlık olduğunu gösterir. Sorunu gidermek için biri veya her ikisi aşağıdaki görevlerden birini yapın:
* Hedefleri arasında Ayrımcılığı artırmak HALUK yardımcı olmak için daha fazla etiket ekleyin.
* Daha hızlı bilgi HALUK yardımcı olmak için etki alanına özgü sözlük tanıtmak tümcecik listesi özellikleri ekleyin.

Bkz: [toplu sınama](luis-tutorial-batch-testing.md) Öğreticisi.

### <a name="when-an-app-is-exported-then-reimported-into-a-new-app-with-a-new-app-id-the-luis-prediction-scores-are-different-why-does-this-happen"></a>Bir uygulamayı dışarı sonra yeni bir uygulamayla (yeni bir uygulama kimliği) içine yeniden içe zaman HALUK tahmin puanları farklıdır. Bunun nedeni? 

Bkz: [aynı uygulama kopyalarını tahmin farklarını](luis-concept-prediction-score.md#differences-with-predictions).

## <a name="app-publishing"></a>Uygulama yayımlama

### <a name="what-is-the-tenant-id-in-the-add-a-key-to-your-app-window"></a>Kiracı kimliği "Uygulamanıza anahtarı ekleme" penceresinde nedir?
Azure'da, bir kiracı bir hizmetle ilişkili kuruluş ya da istemci temsil eder. Azure Portalı'nda Kiracı Kimliğinizi bulmak **dizin kimliği** seçerek kutusunu **Azure Active Directory** > **Yönet**  >  **Özellikleri**.

![Azure portalında Kiracı kimliği](./media/luis-manage-keys/luis-assign-key-tenant-id.png)

### <a name="why-are-there-more-subscription-keys-on-my-apps-publish-page-than-i-assigned-to-the-app"></a>Neden olan var. my uygulamanın üzerinde daha fazla abonelik anahtarları yayımlama sayfasını uygulamaya atanan daha? 
Her HALUK uygulama geliştirme başlangıç anahtarı yok. GA zaman çerçevesinde oluşturulan HALUK abonelik anahtarlarını ne olursa olsun uygulamaya eklediyseniz, Yayımla sayfasında görünür. Bu, GA geçiş kolaylaştırmak amacıyla yapılmıştır. Yeni HALUK abonelik anahtarlar Yayımla sayfasında görünmez. 

## <a name="app-management"></a>Uygulama Yönetimi

### <a name="how-do-i-transfer-ownership-of-a-luis-app"></a>Nasıl HALUK uygulama sahipliğini aktarma?
Başka bir Azure aboneliğine HALUK uygulama aktarmak için HALUK uygulama dışarı aktarın ve yeni bir hesap kullanarak içe aktarın. Çağıran istemci uygulamasında HALUK uygulama kodunu güncelleştirin. Yeni uygulama biraz farklı HALUK özgün uygulamadan puanları döndürebilir. 

### <a name="how-do-i-download-a-log-of-user-utterances"></a>Kullanıcı utterances günlüğünü nasıl karşıdan?
Varsayılan olarak, kullanıcıların utterances HALUK uygulamanızı günlüğe kaydeder. Kullanıcıların HALUK uygulamanıza gönderme utterances günlüğünü indirmek için Git **My uygulamaları**ve üzerinde üç nokta düğmesine (***...*** ) uygulamanız için listesinde. Ardından **Endpoint günlükleri dışarı**. Günlük bir virgülle ayrılmış değer (CSV) dosyası olarak biçimlendirilir.

### <a name="how-can-i-disable-the-logging-of-utterances"></a>Utterances günlüğe kaydetmeyi nasıl devre dışı bırakabilirim?
Ayarlayarak kullanıcı utterances oturumunu kapatma etkinleştirebilirsiniz `log=false` uç nokta URL'deki sorgu HALUK istemci uygulamanızı kullanır. Ancak, utterances önermek veya kullanıcı sorgularına dayalı performansı HALUK uygulamanızın yeteneği oturumunu kapatma kapatma devre dışı bırakır. Ayarlarsanız `log=false` veri Gizlilik sorunları nedeniyle, bu kullanıcı utterances kaydını HALUK indirin veya değiştiremezsiniz uygulamanızı artırmak için bu utterances kullanın.

### <a name="why-dont-i-want-all-my-endpoint-utterances-logged"></a>Oturum my uç nokta utterances neden istiyor mu?
Tahmin analize günlüğünüzün kullanıyorsanız, günlüğüne test utterances yakalamayın.

## <a name="data-management"></a>Veri yönetimi

### <a name="can-i-delete-data-from-luis"></a>Veri HALUK silebilmek için? 

* Her zaman HALUK eğitmek için kullanılan örnek utterances silebilirsiniz. HALUK uygulamanızdan bir örnek utterance silerseniz, HALUK web hizmetinden kaldırılır ve dışarı aktarma için kullanılamıyor.
* İçinde HALUK önerir kullanıcı utterances listesinden utterances silebilirsiniz **gözden geçirin, uç nokta utterances** sayfası. Bu listeden utterances silme önerilmesini engeller, ancak bunları günlüklerinden silmez.
* Bir hesap silerseniz, tüm uygulamaları, kendi örnek utterances ve günlükleri birlikte silinir. Veri sunucularda kalıcı olarak silinmeden önce 60 gün boyunca tutulur.

## <a name="language-and-translation-support"></a>Dil ve çevirisi desteği 

### <a name="i-have-an-app-in-one-language-and-want-to-create-a-parallel-app-in-another-language-what-is-the-easiest-way-to-do-so"></a>Tek bir dilde sahip bir uygulaması ve başka bir dilde paralel bir uygulama oluşturmak istediğiniz bildirimi. Bunu yapmanın en kolay yolu nedir?
1. Uygulamanızı verin.
2. Hedef Dil dışarı aktarılan uygulamanızın JSON dosyasında etiketli utterances çevir.
3. Hedefleri ve varlıkları adlarını değiştirmek veya bunları olduğu gibi bırakın gerekebilir.
4. Son olarak, hedef dilde bir HALUK uygulamanız için uygulama içeri aktarın.

## <a name="app-notification"></a>Uygulama bildirimi

### <a name="why-did-i-get-an-email-saying-im-almost-out-of-quota"></a>Kota dışında neredeyse ben bildiren bir e-posta neden mı aldınız?
Yazma başlangıç anahtarınızı yalnızca 1000 izin uç noktası bir ay sorgular. (Ücretsiz veya Ücretli) HALUK abonelik anahtar oluşturun ve uç nokta sorguları yaparken bu anahtarı kullanın. Uç nokta sorguları bir bot veya başka bir istemci uygulamadan yapıyorsanız HALUK uç noktası anahtarı var. değiştirmeniz gerekir. 

## <a name="integrating-luis"></a>HALUK tümleştirme

### <a name="where-is-my-luis-app-created-during-the-azure-web-app-bot-subscription-process"></a>Burada HALUK Uygulamam Azure web uygulaması bot abonelik işlemi sırasında oluşturulur?
HALUK şablon seçer ve seçeneğini belirlerseniz **seçin** Şablon bölmesinde, sol taraftaki bölmede düğmesi şablon türü içerecek şekilde değiştirir ve HALUK şablonu oluşturmak için hangi bölgede sorar. Web uygulama bot işlemini HALUK abonelik ancak oluşturmaz.

![HALUK şablonu web uygulaması bot bölgesi](./media/luis-faq/web-app-bot-location.png)

### <a name="what-luis-regions-support-bot-framework-speech-priming"></a>Hangi HALUK bölgeler Bot Framework konuşma Hazırlama işlemi destekliyor?
[Konuşma Hazırlama işlemi](https://docs.microsoft.com/bot-framework/bot-service-manage-speech-priming) yalnızca merkezi (ABD) örneği HALUK uygulamalar için desteklenir. 

## <a name="luis-service"></a>HALUK hizmeti 

### <a name="is-luis-available-on-premise-or-in-private-cloud"></a>HALUK kullanılabilir şirket içindeyse veya özel bulutta?
Hayır. 

## <a name="changes-to-the-docs"></a>Belgeleri yapılan değişiklikler

### <a name="where-did-the-tutorials-go"></a>Öğreticiler nerede? 
Eğitmen bölümünde daha önce olan makaleler artık belgelerinin nasıl yapılır bölümünde bulunur. 

|Öğretici|
|--|
|İle bir bot HALUK tümleştirileceğini [C#](luis-csharp-tutorial-build-bot-framework-sample.md) ve [Node.js](luis-nodejs-tutorial-build-bot-framework-sample.md)|
|Application Insights ile bir Bot ekleyin [C#](luis-tutorial-bot-csharp-appinsights.md) ve [Node.js](luis-tutorial-function-appinsights.md)|
|Program aracılığıyla kullanarak bir HALUK uygulaması derleme [Node.js](luis-tutorial-node-import-utterances-csv.md)|
|Kullanım [Bileşik varlık](luis-tutorial-composite-entity.md) gruplandırılmış veri ayıklamak için|
|Ekleme [listesi varlık](luis-tutorial-list-entity.md) Node.js kullanarak artan varlık algılama|
|İle tahmin doğruluğunu artırmak bir [tümcecik listesi](luis-tutorial-interchangeable-phrase-list.md), [desenleri](luis-tutorial-pattern.md), ve [toplu test etme](luis-tutorial-batch-testing.md)|
|[Yazımı düzeltin](luis-tutorial-batch-testing.md) Bing yazım denetleme API v7 ile

### <a name="at-the-build-2018-conference-i-heard-about-a-language-understanding-feature-or-demo-but-i-dont-remember-what-it-was-called"></a>Yapı 2018 konferansında dil anlama özellik veya tanıtım hakkında heard ancak ne çağrıldı hatırlama? 

Aşağıdaki özellikler yapı 2018 konferansında yayımlanan:

|Ad|İçerik|
|--|--|
|Geliştirmeler|[Normal ifade](luis-concept-data-extraction.md##regular-expression-entity-data) varlık ve [anahtar tümcecik](luis-concept-data-extraction.md#key-phrase-extraction-entity-data) varlık
|Desenler|Desenler [kavram](luis-concept-patterns.md), [öğretici](luis-tutorial-pattern.md), [nasıl yapılır](luis-how-to-model-intent-pattern.md)<br>[Patterns.Any](luis-concept-entity-types.md) varlık kavramı da dahil olmak üzere [açık listesi](luis-concept-patterns.md#explicit-lists) özel durumları<br>[Rolleri](luis-concept-roles.md) kavramı|
|Tümleştirmeler|[Metin analizi](https://docs.microsoft.com/azure/cognitive-services/text-analytics/) tümleştirilmesi [düşünceleri çözümleme](publishapp.md#enable-sentiment-analysis)<br>[Konuşma](https://docs.microsoft.com/azure/cognitive-services/speech) tümleştirilmesi [konuşma Hazırlama işlemi](publishapp.md#enable-speech-priming) birlikte [konuşma SDK'sı](https://aka.ms/SpeechSDK)|
|Dağıtım Aracı|Parçası [BotBuilder Araçları](https://github.com/Microsoft/botbuilder-tools), gönderme komut satırı [aracı](luis-concept-enterprise.md#when-you-need-to-combine-several-luis-and-qna-maker-apps) tek HALUK uygulamada bir Bot daha iyi hedefi tanıma için birden çok HALUK ve QnA Maker uygulamalar birleştirmek için

Ek yazma [API yollar](https://github.com/Microsoft/LUIS-Samples/blob/master/authoring-routes.md) içerdiği. 

Videolar: 
* [Azure Cuma yapı 2018 saat: Bilişsel hizmetler - dili (HALUK)](https://channel9.msdn.com/Shows/Azure-Friday/At-Build-2018-Cognitive-Services-Language-LUIS/player)
* [Yapı 2018 AI Göster - dil anlama hizmetiyle yenilikler nelerdir?](https://channel9.msdn.com/Shows/AI-Show/Whats-New-with-Language-Understanding-Service-LUIS/player)
* [Yapı 2018 oturum - Bot Intelligence, konuşma özellikleri ve NLU en iyi uygulamalar](https://channel9.msdn.com/events/Build/2018/BRK3208)
* [Yapı 2018 - HALUK güncelleştirmeleri](https://channel9.msdn.com/events/Build/2018/THR3118/player)

Projeler: 
* [Contoso Cafe bot](https://github.com/botbuilderbuild2018/build2018demo) demo - github'da kaynak kodu

## <a name="next-steps"></a>Sonraki adımlar

HALUK hakkında daha fazla bilgi edinmek için aşağıdaki kaynaklara bakın:
* [Yığın taşması soruları HALUK ile etiketlenmiş](https://stackoverflow.com/questions/tagged/luis)
* [Akıllı anlama MSDN dil Hizmetleri (HALUK) Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=LUIS) 

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website
