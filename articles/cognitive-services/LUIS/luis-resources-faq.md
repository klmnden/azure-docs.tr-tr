---
title: Language Understanding (LUIS) Azure sık sorulan sorular | Microsoft Docs
description: Language Understanding (LUIS) hakkında sık sorulan soruların yanıtlarını alın
author: diberry
manager: cjgronlund
services: cognitive-services
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 07/26/2018
ms.author: diberry
ms.openlocfilehash: 75159319e25ac63907e0d8a2cbf044cf9a854785
ms.sourcegitcommit: cfff72e240193b5a802532de12651162c31778b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39308754"
---
# <a name="language-understanding-faq"></a>Language Understanding hakkında SSS

Bu makale, Language Understanding (LUIS) hakkında sık sorulan soruların yanıtlarını içerir.

## <a name="luis-authoring"></a>LUIS yazma

### <a name="what-are-the-luis-best-practices"></a>LUIS en iyi uygulamalar nelerdir? 
İle başlayan [geliştirme döngüsü](luis-concept-app-iteration.md), ardından okuma [en iyi uygulamalar](luis-concept-best-practices.md). 

### <a name="what-is-the-best-way-to-start-building-my-app-in-luis"></a>LUIS uygulama oluşturmaya başlamak için en iyi yolu nedir?

Uygulamanızı oluşturmak için en iyi yollarından biri sayesinde bir [artımlı işlem](luis-concept-app-iteration.md). 

### <a name="what-is-a-good-practice-to-model-the-intents-of-my-app-should-i-create-more-specific-or-more-generic-intents"></a>Uygulamamın ıntents modellemek için iyi bir uygulama nedir? Daha özel ya da daha genel bir ıntents oluşturmalıyım?

Çakışan, ancak bu nedenle özel olmayan, benzer amaçları arasında ayrım yapmak LUIS zorlaştırır emin olacak şekilde genel olmayan hedefleri seçin. Discriminative belirli hedefleri oluşturma LUIS modelleme için en iyi biridir.

### <a name="is-it-important-to-train-the-none-intent"></a>Hiçbiri hedefi eğitmek önemlidir?

Evet, eğitmek iyi olduğu, **hiçbiri** diğer amaçlar için daha fazla etiket ekledikçe daha fazla Konuşma ile hedefi. 1 veya 2 etiketleri eklenen iyi oranıdır **hiçbiri** bir amaç için eklenen her 10 etiketler. Bu oran LUIS discriminative gücünü artırıyor.

### <a name="how-can-i-correct-spelling-mistakes-in-utterances"></a>Konuşma, yazım hatalarını düzeltmek nasıl?

Bkz: [Bing yazım denetimi API'si V7](luis-tutorial-bing-spellcheck.md) öğretici. LUIS, Bing yazım denetimi API'si V7 tarafından uygulanan sınırları zorlar. 

### <a name="how-do-i-edit-my-luis-app-programmatically"></a>LUIS uygulamamı program aracılığıyla nasıl düzenleyebilirim?
Program aracılığıyla LUIS uygulamanızı düzenlemek için kullanın [yazma API](https://aka.ms/luis-authoring-apis). Bkz: [API geliştirme LUIS çağrı](./luis-quickstart-node-add-utterance.md) ve [Node.js kullanarak program aracılığıyla LUIS uygulaması oluşturma](./luis-tutorial-node-import-utterances-csv.md) yazma API'nin nasıl çağrılacağını örnekleri için. Yazma API kullanmanızı gerektirir. bir [anahtar yazma](luis-concept-keys.md#authoring-key) yerine bir uç noktası anahtarı. Programlı yazma, ayda en fazla 1.000.000 çağrısı ve beş saniyede sağlar. LUIS ile kullandığınız anahtarları hakkında daha fazla bilgi için bkz. [anahtarları Yönet](./luis-concept-keys.md).

### <a name="where-is-the-pattern-feature-that-provided-regular-expression-matching"></a>Burada sağlanan normal ifade deseni özelliği eşleşiyor mu?
Önceki **deseni özelliği** şu anda kullanım dışı, değiştirilen  **[desenleri](luis-concept-patterns.md)**. 

### <a name="how-do-i-use-an-entity-to-pull-out-the-correct-data"></a>Bir varlık doğru veri çekmek için nasıl kullanırım? 
Bkz: [varlıkları](luis-concept-entity-types.md) ve [veri ayıklama](luis-concept-data-extraction.md).

### <a name="should-variations-of-an-example-utterance-include-punctuation"></a>Bir örnek utterance çeşitleri noktalama dahil edilsin mi? 
Amaç için örnek konuşma olarak farklı çeşitlemeleri ekleyebilir veya ekleme ile örnek utterance desenini [yok saymak için söz dizimi](luis-concept-patterns.md#pattern-syntax) noktalama işareti. 

### <a name="does-luis-currently-support-cortana"></a>LUIS, şu anda cortana'yı destekliyor mu?

Cortana önceden oluşturulmuş uygulamalar, 2017'de kullanım dışı bırakıldı. Bunlar artık desteklenir. 

## <a name="luis-endpoint"></a>LUIS uç noktası

### <a name="why-does-luis-add-spaces-to-the-query-around-or-in-the-middle-of-words"></a>Neden LUIS geçici bir çözüm veya sözcük ortasında sorguya alanları ekliyor mu?
LUIS [tokenizes](luis-glossary.md#token) utterance temel alarak [kültür](luis-supported-languages.md#tokenization). Parçalanmış değeri ve özgün değeri kullanılabilir [veri ayıklama](luis-concept-data-extraction.md#tokenized-entity-returned).

### <a name="how-do-i-create-and-assign-a-luis-endpoint-key"></a>Nasıl oluştururum ve uç noktası anahtarı bir LUIS atama?
[Uç nokta oluşturma](luis-how-to-azure-subscription.md#create-luis-endpoint-key) için azure'da, [hizmet](https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/) düzeyi. [Anahtar atama](luis-how-to-manage-keys.md#assign-endpoint-key) üzerinde **[Yayımla](luis-how-to-publish-app.md)** sayfası. Bu eyleme karşılık gelen hiçbir API yoktur. HTTP isteği için uç nokta için değiştirmeniz gerekir sonra [yeni uç nokta anahtarını kullanmak](luis-concept-keys.md#use-endpoint-key-in-query).

### <a name="how-do-i-interpret-luis-scores"></a>LUIS puanları nasıl yorumlanacağı? 
Sisteminizi, en yüksek Puanlama amaç değeri ne olursa olsun kullanmanız gerekir. Örneğin, 0,5 (daha az % 50'den) altında bir puan mutlaka LUIS düşük güven olduğunu gelmez. Daha fazla eğitim verilerini sağlayarak, olasılıkla amaç puanı artırmaya yardımcı olabilir.

### <a name="why-dont-i-see-my-endpoint-hits-in-my-apps-dashboard"></a>Benim uygulamamın Pano uç noktası isabet neden göremiyorum?
Uygulamanızın panosunda toplam uç noktası İsabeti düzenli olarak güncelleştirilir ancak Azure portalında LUIS uç nokta anahtarıyla ilişkili ölçümleri daha sık güncelleştirilir. 

Güncelleştirilmiş uç noktası İsabeti panosunda görmüyorsanız, Azure Portal'da oturum açın ve LUIS uç nokta anahtarınız ile ilişkili kaynak bulun ve açın **ölçümleri** seçilecek **toplam çağrı** ölçümü. Uç nokta için birden fazla LUIS uygulaması kullandıysanız, Azure portalında ölçüm kullanan tüm LUIS uygulamalardan gelen çağrıları toplam sayısını gösterir.

### <a name="my-luis-app-was-working-yesterday-but-today-im-getting-403-errors-i-didnt-change-the-app-how-do-i-fix-it"></a>LUIS uygulamamı dün çalıştığı ancak bugün 403 hataları alıyorum. Ben uygulama değişmedi. Bunu nasıl düzeltirim? 
Aşağıdaki [yönergeleri](#how-do-i-create-and-assign-a-luis-endpoint-key) LUIS uç noktası anahtarı oluşturun ve uygulamaya atamak için bir sonraki SSS. HTTP isteği için uç nokta için değiştirmeniz gerekir sonra [yeni uç nokta anahtarını kullanmak](luis-concept-keys.md#use-endpoint-key-in-query).

### <a name="how-do-i-secure-my-luis-endpoint"></a>LUIS Noktam güvenliğini nasıl sağlayabilirim? 
Bkz: [uç nokta güvenliği](luis-concept-security.md#securing-the-endpoint).

## <a name="working-within-luis-limits"></a>LUIS sınırlar içinde çalışma

### <a name="what-is-the-maximum-number-of-intents-and-entities-that-a-luis-app-can-support"></a>Hedefleri ve LUIS uygulaması destekleyebileceği varlıkların sayısı nedir?
Bkz: [sınırları](luis-boundaries.md) başvuru.

### <a name="i-want-to-build-a-luis-app-with-more-than-the-maximum-number-of-intents-what-should-i-do"></a>Bir LUIS uygulaması amacı, en fazla sayısından daha oluşturmak istiyorsunuz. Ne yapmalıyım?

Bkz: [hedefleri için en iyi yöntemler](luis-concept-intent.md#if-you-need-more-than-the-maximum-number-of-intents).

### <a name="i-want-to-build-an-app-in-luis-with-more-than-the-maximum-number-of-entities-what-should-i-do"></a>LUIS birden çok varlık sayısı ile bir uygulama oluşturmak istiyorsunuz. Ne yapmalıyım?

Bkz: [varlıklar için en iyi yöntemler](luis-concept-entity-types.md#if-you-need-more-than-the-maximum-number-of-entities)

### <a name="what-are-the-limits-on-the-number-and-size-of-phrase-lists"></a>Listeler sayısına ve tümcecik boyutunu sınırları nelerdir?
Uzunluğunun üst sınırı için bir [tümcecik listesi](./luis-concept-feature.md), bkz: [sınırları](luis-boundaries.md) başvuru.

### <a name="what-are-the-limits-on-example-utterances"></a>Örnek konuşma sınırları nelerdir?
Bkz: [sınırları](luis-boundaries.md) başvuru.

## <a name="testing-and-training"></a>Test ve eğitim

### <a name="i-see-some-errors-in-the-batch-testing-pane-for-some-of-the-models-in-my-app-how-can-i-address-this-problem"></a>Uygulamamı modellerinde bazıları için bölmesinde test toplu işi bazı hataları görüyorum. Bu sorunu gidermek ne?

Hataları, etiketlerinizi ve Modellerinizi ait tahminlerin arasında bazı bir tutarsızlık olduğunu gösterir. Sorunu gidermek için biri veya her ikisi şu görevleri yapın:
* LUIS amaçları arasında Ayrımcılığı geliştirmek amacıyla daha fazla etiket ekleyin.
* Hızlı bilgi LUIS yardımcı olmak için etki alanına özel sözlük tanıtan ifade listesi özellikleri ekleyin.

Bkz: [toplu test](luis-tutorial-batch-testing.md) öğretici.

### <a name="when-an-app-is-exported-then-reimported-into-a-new-app-with-a-new-app-id-the-luis-prediction-scores-are-different-why-does-this-happen"></a>Bir uygulamayı dışarı sonra (yeni bir uygulama kimliği ile) yeni bir uygulamaya yeniden içe tıkladığınızda LUIS tahmin puanları farklıdır. Bu neden gerçekleşir? 

Bkz: [aynı uygulamanın bir kopyasını tahmin farklılıklardan](luis-concept-prediction-score.md#differences-with-predictions).

## <a name="app-publishing"></a>Uygulama yayımlama

### <a name="what-is-the-tenant-id-in-the-add-a-key-to-your-app-window"></a>Kiracı kimliği "Anahtarı uygulamanıza ekleme" penceresinde nedir?
Azure'da, bir kiracı istemcisi veya hizmeti ile ilişkili kuruluş temsil eder. Azure portalında Kiracı Kimliğinizi bulmak **dizin kimliği** kutusunu seçerek **Azure Active Directory** > **Yönet**  >  **Özellikleri**.

![Azure portalında Kiracı kimliği](./media/luis-manage-keys/luis-assign-key-tenant-id.png)

<a name="why-are-there-more-subscription-keys-on-my-apps-publish-page-than-i-assigned-to-the-app"></a>
### <a name="why-are-there-more-endpoint-keys-on-my-apps-publish-page-than-i-assigned-to-the-app"></a>Neden olan var. uygulamamın hakkında daha fazla uç nokta anahtarları yayımlama sayfasını uygulamaya atadığım daha? 
Her LUIS uygulaması yazma başlangıç anahtarına sahiptir. LUIS uç nokta anahtarları GA zaman çerçevesinde oluşturulan bakılmaksızın uygulamaya eklediyseniz Yayımla sayfanızda görünür değildir. Bu, GA geçiş kolaylaştırmak için yapıldı. Herhangi bir yeni LUIS uç noktası anahtarı Yayımla sayfasında görünmez. 

## <a name="app-management"></a>Uygulama Yönetimi

### <a name="how-do-i-transfer-ownership-of-a-luis-app"></a>Bir LUIS uygulaması sahipliğini nasıl aktarabilir?
Bir LUIS uygulaması için farklı bir Azure aboneliği aktarmayı LUIS uygulaması dışarı aktarma ve yeni bir hesap kullanarak içe aktarın. Çağıran istemci uygulamasındaki LUIS uygulama kodunu güncelleştirin. Yeni uygulamayı biraz daha farklı LUIS özgün uygulamadan puanları döndürebilir. 

### <a name="how-do-i-download-a-log-of-user-utterances"></a>Kullanıcı konuşma günlüğünü nasıl indiririm?
Varsayılan olarak, kullanıcıların konuşma LUIS uygulamanızı günlüğe kaydeder. LUIS uygulamanızı kullanıcılara gönderme konuşma günlüğünü indirmek için Git **uygulamalarım**, üç noktaya tıklayın (***...*** ) uygulamanızı listesinde. Ardından **uç nokta günlükleri dışarı aktar**. Günlük bir virgülle ayrılmış değer (CSV) dosyası olarak biçimlendirilir.

### <a name="how-can-i-disable-the-logging-of-utterances"></a>Konuşma günlüğe kaydetmeyi nasıl devre dışı bırakabilirim?
Kullanıcı konuşma oturumdan ayarlayarak etkinleştirebilirsiniz `log=false` içinde sorgu LUIS istemci uygulamanızın kullandığı uç nokta URL'si. Ancak, konuşma önerebilir veya temel alan performansı LUIS uygulamanızın olanağı oturumdan kapatma devre dışı bırakır [etkin olarak öğrenmeye](luis-concept-review-endpoint-utterances.md#what-is-active-learning). Ayarlarsanız `log=false` veri gizliliği kaygıları nedeniyle, bu kullanıcı konuşma kaydını LUIS ' indirin veya uygulamanızı geliştirmek için bu konuşma kullanın.

Günlüğe kaydetme, konuşma, yalnızca depolama alanıdır. 

### <a name="why-dont-i-want-all-my-endpoint-utterances-logged"></a>Neden oturum my uç nokta konuşma istemez miyiz?
Tahmin analizi için günlüğünüzü kullanıyorsanız, test konuşma oturum yakalamayın.

## <a name="data-management"></a>Veri yönetimi

### <a name="can-i-delete-data-from-luis"></a>LUIS verileri silebilir miyim? 

* LUIS eğitim için kullanılan örnek konuşma her zaman silebilirsiniz. LUIS uygulamanızdan bir örnek utterance silerseniz, LUIS web hizmetinden kaldırılır ve dışarı aktarma için kullanılamaz.
* Konuşma içinde LUIS önerir kullanıcı konuşma listesinden silebilirsiniz **gözden geçirin, konuşma uç noktası** sayfası. Konuşma bu listeden silme önerilmesini engelliyor, ancak bunları günlüklerinden silmez.
* Bir hesabı silerseniz, tüm uygulamalar, kendi örnek konuşma ve günlükleri birlikte silinir. Veriler kalıcı olarak silinmeden önce 60 gün boyunca sunucularda tutulur.

### <a name="does-microsoft-access-my-luis-app-data-for-its-own-purposes-for-example-to-enhance-luis-or-microsoft-in-general"></a>Microsoft LUIS uygulaması verilerimi kendi amaçları için örneğin, genel LUIS veya Microsoft geliştirmek için erişir? 

Hayır. LUIS uygulamanın veri modelini bir platform olarak LUIS geliştirmek için LUIS tarafından kullanılmayan veya Microsoft tarafından herhangi bir yolla kullanılmıştır. Her uygulamanın ayrı ve yalnızca ortak çalışanlarla ve kullanıcı tarafından sahip olunan verilerdir. 

Daha fazla bilgi edinin [kullanıcı gizliliğini](luis-reference-gdpr.md), [ek güvenlik Uyumluluk](luis-concept-security.md#security-compliance), ve [veri depolama](luis-concept-data-storage.md).

## <a name="language-and-translation-support"></a>Dil ve çeviri desteği 

### <a name="i-have-an-app-in-one-language-and-want-to-create-a-parallel-app-in-another-language-what-is-the-easiest-way-to-do-so"></a>Tek bir dilde bir uygulamaya sahip ve başka bir dilde paralel bir uygulama oluşturmak istiyorsanız bildirimi. Bunu yapmanın en kolay yolu nedir?
1. Uygulamanızı dışarı aktarın.
2. Hedef dil için dışarı aktarılan uygulama JSON dosyasındaki etiketli sesleri çevirin.
3. Amaç ve varlıkları adlarını değiştirme veya oldukları gibi bunları bırakın gerekebilir.
4. Son olarak, hedef dilde bir LUIS uygulaması için uygulama içeri aktarın.

## <a name="app-notification"></a>Uygulama bildirimi

### <a name="why-did-i-get-an-email-saying-im-almost-out-of-quota"></a>Neden neredeyse kotası aşıldı ben bildiren bir e-posta almak?
Yazma başlangıç anahtarınızı yalnızca 1000 kullanılabilir uç nokta, bir ay sorgular. Bir LUIS uç noktası anahtarı (ücretsiz veya Ücretli) oluşturun ve uç nokta sorgu oluştururken bu anahtarı kullanın. Uç nokta sorguları bir bot veya başka bir istemci uygulaması oluşturuyorsanız, LUIS uç noktası anahtarı var. değiştirmeniz gerekir. 

## <a name="integrating-luis"></a>LUIS tümleştirme

### <a name="where-is-my-luis-app-created-during-the-azure-web-app-bot-subscription-process"></a>LUIS uygulamamı Azure web app botu abonelik işlemi sırasında oluşturulduğu?
Bir LUIS şablonu seçin ve seçin, **seçin** düğmesi Şablon bölmesinde, sol taraftaki bölmede şablon türü içerecek şekilde değiştirir ve LUIS şablonu oluşturmak için hangi bölgede sorar. Web app botu işlemi yine de bir LUIS abonelik oluşturmaz.

![LUIS şablonu web app botu bölgesi](./media/luis-faq/web-app-bot-location.png)

### <a name="what-luis-regions-support-bot-framework-speech-priming"></a>Hangi LUIS bölgeleri Bot Framework konuşma Hazırlama işlemi destekler?
[Konuşma Hazırlama işlemi](https://docs.microsoft.com/bot-framework/bot-service-manage-speech-priming) yalnızca merkezi (ABD) örneğinde LUIS uygulamalar için desteklenir. 

## <a name="luis-service"></a>LUIS hizmeti 

### <a name="is-luis-available-on-premises-or-in-private-cloud"></a>LUIS şirket içi kullanılabilir mi ya da özel bulutta?
Hayır. 


### <a name="at-the-build-2018-conference-i-heard-about-a-language-understanding-feature-or-demo-but-i-dont-remember-what-it-was-called"></a>Derleme 2018 konferansta ben bir dil anlama özelliği veya tanıtım heard ancak ne çağrıldı hatırlamıyorum? 

Aşağıdaki özellikler Build 2018 konferansında yayımlanan:

|Ad|İçerik|
|--|--|
|Geliştirmeler|[Normal ifade](luis-concept-data-extraction.md##regular-expression-entity-data) varlık ve [anahtar tümcecik](luis-concept-data-extraction.md#key-phrase-extraction-entity-data) varlık
|Desenler|Desenler [kavramı](luis-concept-patterns.md), [öğretici](luis-tutorial-pattern.md), [nasıl yapılır](luis-how-to-model-intent-pattern.md)<br>[Patterns.Any](luis-concept-entity-types.md) varlık kavramı da dahil olmak üzere [açık listesi](luis-concept-patterns.md#explicit-lists) özel durumları<br>[Rolleri](luis-concept-roles.md) kavramı|
|Tümleştirmeler|[Metin analizi](https://docs.microsoft.com/azure/cognitive-services/text-analytics/) tümleştirilmesi [yaklaşım analizi](luis-how-to-publish-app.md#enable-sentiment-analysis)<br>[Konuşma](https://docs.microsoft.com/azure/cognitive-services/speech) tümleştirilmesi [konuşma Hazırlama işlemi](luis-how-to-publish-app.md#enable-speech-priming) birlikte [Speech SDK'sı](https://aka.ms/SpeechSDK)|
|Dağıtım Aracı|Parçası [Botbuilder'da Araçları](https://github.com/Microsoft/botbuilder-tools), gönderme komut satırı [aracı](luis-concept-enterprise.md#when-you-need-to-combine-several-luis-and-qna-maker-apps) tek LUIS uygulamasına bir Bot daha iyi amaç tanıma için birden çok LUIS ve soru-cevap Oluşturucu uygulamaları birleştirmek için

Ek yazma [API yolları](https://github.com/Microsoft/LUIS-Samples/blob/master/authoring-routes.md) dahil edildi. 

Videolar: 
* [Azure Friday Build 2018'e en: Bilişsel hizmetler - dil (LUIS)](https://channel9.msdn.com/Shows/Azure-Friday/At-Build-2018-Cognitive-Services-Language-LUIS/player)
* [Derleme 2018 AI Show - Language Understanding hizmeti ile yenilikler nelerdir?](https://channel9.msdn.com/Shows/AI-Show/Whats-New-with-Language-Understanding-Service-LUIS/player)
* [Build 2018 Session - Bot zekası, Konuşma Özellikleri ve NLU en iyi uygulamaları](https://channel9.msdn.com/events/Build/2018/BRK3208)
* [Build 2018'e - LUIS güncelleştirmeleri](https://channel9.msdn.com/events/Build/2018/THR3118/player)

Projeler: 
* [Contoso Cafe bot](https://github.com/botbuilderbuild2018/build2018demo) tanıtım - Github üzerinde kaynak kodu

## <a name="next-steps"></a>Sonraki adımlar

LUIS hakkında daha fazla bilgi edinmek için aşağıdaki kaynaklara bakın:
* [LUIS ile etiketlenmiş bir yığın taşması soru](https://stackoverflow.com/questions/tagged/luis)
* [Hizmetleri MSDN dil akıllı anlama (LUIS) Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=LUIS) 