---
title: Sık sorulan sorular (SSS)
titleSuffix: Azure Cognitive Services
description: Bu makale, Language Understanding (LUIS) hakkında sık sorulan soruların yanıtlarını içerir.
author: diberry
manager: nitinme
ms.custom: seodec18
services: cognitive-services
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 03/21/2019
ms.author: diberry
ms.openlocfilehash: 672c9d43007f954d870f8195bcad63d9cee69523
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58894466"
---
# <a name="language-understanding-frequently-asked-questions-faq"></a>Language Understanding'i sık sorulan sorular (SSS)

Bu makale, Language Understanding (LUIS) hakkında sık sorulan soruların yanıtlarını içerir.

<a name="luis-authoring"></a>

## <a name="authoring"></a>Yazma

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

### <a name="how-do-i-transfer-ownership-of-a-luis-app"></a>Bir LUIS uygulaması sahipliğini nasıl aktarabilir?
Bir LUIS uygulaması için farklı bir Azure aboneliği aktarmayı LUIS uygulaması dışarı aktarma ve yeni bir hesap kullanarak içe aktarın. Çağıran istemci uygulamasındaki LUIS uygulama kodunu güncelleştirin. Yeni uygulamayı biraz daha farklı LUIS özgün uygulamadan puanları döndürebilir.

### <a name="a-prebuilt-entity-is-tagged-in-an-example-utterance-instead-of-my-custom-entity-how-do-i-fix-this"></a>Önceden oluşturulmuş bir varlık, bir örnek utterance my özel bir varlık yerine etiketlenir. Bunu nasıl düzeltirim? 

Bkz: [önceden oluşturulmuş varlıklarla ilgili sorunları giderme](luis-concept-entity-types.md#troubleshooting-prebuilt-entities).

### <a name="i-tried-to-import-an-app-or-version-file-but-i-got-an-error-what-happened"></a>Bir uygulama veya sürüm dosyasını içeri aktarma çalıştı, ancak ne olduğunu, bir hata aldım? 

Daha fazla bilgi edinin [sürümü alma hataları](luis-how-to-manage-versions.md#import-errors) ve [uygulama alma hataları](luis-how-to-start-new-app.md#import-errors).

<a name="luis-collaborating"></a>

## <a name="collaborating"></a>İşbirliği yapma

### <a name="how-do-i-give-collaborators-access-to-luis-with-azure-active-directory-azure-ad-or-role-based-access-control-rbac"></a>Nasıl miyim ortak çalışanlar LUIS ile Azure Active Directory (Azure AD) veya rol tabanlı erişim denetimi (RBAC) erişmesini?

Bkz: [Azure Active Directory kaynaklarını](luis-how-to-collaborate.md#azure-active-directory-resources) ve [Azure Active Directory Kiracı Kullanıcı](luis-how-to-collaborate.md#azure-active-directory-tenant-user) ortak çalışanlar erişmesini hakkında bilgi edinmek için. 

<a name="luis-endpoint"></a>

## <a name="endpoint"></a>Uç Nokta

### <a name="my-endpoint-query-returned-unexpected-results-what-should-i-do"></a>Uç nokta Sorgum beklenmeyen bir sonuç döndürdü. Ne yapmalıyım?

Beklenmeyen sorgu tahmin sonuçlarını yayımlanan model durumuna dayanır. Model düzeltmek için modeli değiştirmek için eğitme ve yeniden yayımlayın. 

Model düzeltme ile başlayan [etkin olarak öğrenmeye](luis-how-to-review-endpoint-utterances.md).

Güncelleştirerek belirleyici eğitim kaldırabilirsiniz [uygulama sürümü ayarları API](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/versions-update-application-version-settings) tüm eğitim verilerini kullanmak için.

Gözden geçirme [en iyi uygulamalar](luis-concept-best-practices.md) diğer ipuçları için. 

### <a name="why-does-luis-add-spaces-to-the-query-around-or-in-the-middle-of-words"></a>Neden LUIS geçici bir çözüm veya sözcük ortasında sorguya alanları ekliyor mu?
LUIS [tokenizes](luis-glossary.md#token) utterance temel alarak [kültür](luis-language-support.md#tokenization). Parçalanmış değeri ve özgün değeri kullanılabilir [veri ayıklama](luis-concept-data-extraction.md#tokenized-entity-returned).

### <a name="how-do-i-create-and-assign-a-luis-endpoint-key"></a>Nasıl oluştururum ve uç noktası anahtarı bir LUIS atama?
[Uç nokta oluşturma](luis-how-to-azure-subscription.md) için azure'da, [hizmet](https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/) düzeyi. [Anahtar atama](luis-how-to-azure-subscription.md) üzerinde **[anahtarları ve uç noktaları](luis-how-to-azure-subscription.md)** sayfası. Bu eyleme karşılık gelen hiçbir API yoktur. HTTP isteği için uç nokta için değiştirmeniz gerekir sonra [yeni uç nokta anahtarını kullanmak](luis-concept-keys.md#use-endpoint-key-in-query).

### <a name="how-do-i-interpret-luis-scores"></a>LUIS puanları nasıl yorumlanacağı?
Sisteminizi, en yüksek Puanlama amaç değeri ne olursa olsun kullanmanız gerekir. Örneğin, 0,5 (daha az % 50'den) altında bir puan mutlaka LUIS düşük güven olduğunu gelmez. Daha fazla eğitim verileri yardımcı sağlama artırmak [puanı](luis-concept-prediction-score.md) olasılıkla hedefinin.

### <a name="why-dont-i-see-my-endpoint-hits-in-my-apps-dashboard"></a>Benim uygulamamın Pano uç noktası isabet neden göremiyorum?
Uygulamanızın panosunda toplam uç noktası İsabeti düzenli olarak güncelleştirilir ancak Azure portalında LUIS uç nokta anahtarıyla ilişkili ölçümleri daha sık güncelleştirilir.

Güncelleştirilmiş uç noktası İsabeti panosunda görmüyorsanız, Azure portalında oturum açın ve LUIS uç nokta anahtarınız ile ilişkili kaynak bulun ve açın **ölçümleri** seçilecek **toplam çağrı** ölçümü. Uç nokta için birden fazla LUIS uygulaması kullandıysanız, Azure portalında ölçüm kullanan tüm LUIS uygulamalardan gelen çağrıları toplam sayısını gösterir.

### <a name="is-there-a-powershell-command-get-to-the-endpoint-quota"></a>Bir PowerShell komut almak için uç nokta kotası var mı?

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Uç nokta kota görmek için bir PowerShell komutu kullanabilirsiniz:

```powershell
Get-AzCognitiveServicesAccountUsage -ResourceGroupName <your-resource-group> -Name <your-resource-name>
``` 

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

Bkz: [aynı uygulamanın bir kopyasını tahmin farklılıklardan](luis-concept-prediction-score.md#review-intents-with-similar-scores).

### <a name="some-utterances-go-to-the-wrong-intent-after-i-made-changes-to-my-app-the-issue-seems-to-disappear-at-random-how-do-i-fix-it"></a>Uygulamama yaptığım değişiklikleri sonra bazı konuşma yanlış ıntent'e gidin. Rastgele olarak kaybolması sorunu görünüyor. Bunu nasıl düzeltirim? 

Bkz: [tüm verilerle Train](luis-how-to-train.md#train-with-all-data).

## <a name="app-publishing"></a>Uygulama yayımlama

### <a name="what-is-the-tenant-id-in-the-add-a-key-to-your-app-window"></a>Kiracı kimliği "Anahtarı uygulamanıza ekleme" penceresinde nedir?
Azure'da, bir kiracı istemcisi veya bir hizmet ile ilişkili kuruluş temsil eder. Azure portalında Kiracı Kimliğinizi bulmak **dizin kimliği** kutusunu seçerek **Azure Active Directory** > **Yönet**  >  **Özellikleri**.

![Azure portalında Kiracı kimliği](./media/luis-manage-keys/luis-assign-key-tenant-id.png)

<a name="why-are-there-more-subscription-keys-on-my-apps-publish-page-than-i-assigned-to-the-app"></a>
<a name="why-are-there-more-endpoint-keys-on-my-apps-publish-page-than-i-assigned-to-the-app"></a>


### <a name="why-are-there-more-endpoint-keys-assigned-to-my-app-than-i-assigned"></a>Neden atadığım daha uygulamama atanan daha fazla uç nokta anahtarları vardır?
Her LUIS uygulaması yazma başlangıç anahtarı kolaylık uç nokta listesinde yok. LUIS deneyebilirsiniz. Bu nedenle bu anahtar yalnızca birkaç uç noktası İsabeti sağlar.  

LUIS genel kullanıma (GA) şeklindeydi uygulamanız varsa, aboneliğinizdeki LUIS uç nokta anahtarları otomatik olarak atanır. Bu, GA geçiş kolaylaştırmak için yapıldı. Azure portalında yeni bir LUIS uç nokta anahtarlar _değil_ LUIS otomatik atanmış.

## <a name="key-management"></a>Anahtar yönetimi

### <a name="how-do-i-know-what-key-i-need-where-i-get-it-and-what-i-do-with-it"></a>Hangi anahtar gerekiyor, bunu nereden bulabilirim nasıl bilebilirim ve onunla neler yapabilirim? 

Bkz: [LUIS yazma ve sorgu tahmin uç nokta anahtarlarını](luis-concept-keys.md) arasındaki farklar hakkında bilgi edinmek için [anahtar yazma](luis-how-to-account-settings.md) ve [uç noktası tahmin anahtarı](luis-how-to-azure-subscription.md). 

### <a name="i-got-an-error-about-being-out-of-quota-how-do-i-fix-it"></a>Yetersiz kota ilgili bir hata aldım. Bunu nasıl düzeltirim? 

Bkz, [anahtar fiyatlandırma katmanı kullanımı aştığında Kota aşımı hataları düzeltmek nasıl](luis-how-to-azure-subscription.md##how-to-fix-out-of-quota-errors-when-the-key-exceeds-pricing-tier-usage) daha fazla bilgi için.

### <a name="i-need-to-handle-more-endpoint-queries-how-do-i-do-that"></a>Daha fazla uç nokta sorguları işlemek gerekir. Bu ne yapmalıyım? 

Bkz, [anahtar fiyatlandırma katmanı kullanımı aştığında Kota aşımı hataları düzeltmek nasıl](luis-how-to-azure-subscription.md##how-to-fix-out-of-quota-errors-when-the-key-exceeds-pricing-tier-usage) daha fazla bilgi için.



## <a name="app-management"></a>Uygulama yönetimi

### <a name="how-do-i-download-a-log-of-user-utterances"></a>Kullanıcı konuşma günlüğünü nasıl indiririm?
Varsayılan olarak, kullanıcıların konuşma LUIS uygulamanızı günlüğe kaydeder. LUIS uygulamanızı kullanıcılara gönderme konuşma günlüğünü indirmek için Git **uygulamalarım**ve uygulamayı seçin. Bağlamsal araç çubuğunda, seçin **uç nokta günlükleri dışarı aktar**. Günlük bir virgülle ayrılmış değer (CSV) dosyası olarak biçimlendirilir.

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

### <a name="how-does-microsoft-manage-data-i-send-to-luis"></a>Microsoft, LUIS için gönderebilirim verileri nasıl yönetir?

[Güven Merkezi](https://www.microsoft.com/trustcenter) sunduğumuz ve veri yönetimi ve erişimi Azure Hizmetleri için seçenekleri açıklar.

## <a name="language-and-translation-support"></a>Dil ve çeviri desteği

### <a name="i-have-an-app-in-one-language-and-want-to-create-a-parallel-app-in-another-language-what-is-the-easiest-way-to-do-so"></a>Tek bir dilde bir uygulamaya sahip ve başka bir dilde paralel bir uygulama oluşturmak istiyorsanız bildirimi. Bunu yapmanın en kolay yolu nedir?
1. Uygulamanızı dışarı aktarın.
2. Hedef dil için dışarı aktarılan uygulama JSON dosyasındaki etiketli sesleri çevirin.
3. Amaç ve varlıkları adlarını değiştirme veya oldukları gibi bunları bırakın gerekebilir.
4. Son olarak, hedef dilde bir LUIS uygulaması için uygulama içeri aktarın.

## <a name="app-notification"></a>Uygulama bildirimi

### <a name="why-did-i-get-an-email-saying-im-almost-out-of-quota"></a>Neden neredeyse kotası aşıldı ben bildiren bir e-posta almak?
Yazma başlangıç anahtarınızı yalnızca 1000 kullanılabilir uç nokta, bir ay sorgular. Bir LUIS uç noktası anahtarı (ücretsiz veya Ücretli) oluşturun ve uç nokta sorgu oluştururken bu anahtarı kullanın. Uç nokta sorguları bir bot veya başka bir istemci uygulaması oluşturuyorsanız, LUIS uç noktası anahtarı var. değiştirmeniz gerekir.

## <a name="bots"></a>Botlar

### <a name="my-luis-bot-isnt-working-what-do-i-do"></a>My LUIS bot çalışmıyor. Ne yapmalıyım?

İlk sorun LUIS için ilgili veya LUIS ara yazılım dışında'olmuyor yalıtmak için sorunudur. 

#### <a name="resolve-issue-in-luis"></a>LUIS sorunu çözün
LUIS aynı utterance geçirmek [LUIS uç nokta](luis-get-started-create-app.md#query-the-endpoint-with-a-different-utterance). Bir hata alırsanız, hata artık döndürülünceye kadar LUIS sorunu çözün. Sık karşılaşılan hatalar şunlardır:

* `Out of call volume quota. Quota will be replenished in <time>.` -Bu sorun için yazma anahtarından değiştirmenize gerek ya da belirten bir [uç noktası anahtarı](luis-how-to-azure-subscription.md) veya değiştirmeniz gerekirse [hizmet katmanları](luis-how-to-azure-subscription.md#change-pricing-tier). 

#### <a name="resolve-issue-in-azure-bot-service"></a>Azure Bot hizmeti, sorunu

Azure Bot hizmeti kullanıyorsanız ve sorunu olan **Test Web sohbeti** döndürür `Sorry, my bot code is having an issue`, günlüklerinizi denetleyin:

1. Azure portalında, botunuza ilişkin gelen **Bot Yönetim** bölümünden **yapı**.
1. Çevrimiçi Kod Düzenleyicisi'ni açın. 
1. Üst, mavi gezinti çubuğunda, robot adı (ikinci öğe sağındaki) seçin.
1. Sonuçta elde edilen aşağı açılan listesinde seçin **Kudu konsolu aç**.
1. Seçin **LogFiles**, ardından **uygulama**. Tüm günlük dosyalarını gözden geçirin. Hata uygulama klasöründe görmüyorsanız, altındaki tüm günlük dosyalarını gözden geçirin. **LogFiles**. 
1. Gibi derlenmiş bir dil kullanıyorsanız, projeyi yeniden derleyin unutmayın C#.

> [!Tip] 
> Konsolu, paketleri de yükleyebilirsiniz. 

#### <a name="resolve-issue-while-debugging-on-local-machine-with-bot-framework"></a>Bot Framework ile yerel makinede hata ayıklama sırasında sorununu çözümleyin. 

Yerel bir bot hata ayıklama hakkında daha fazla bilgi için bkz: [bir bot hata ayıklama](https://docs.microsoft.com/azure/bot-service/bot-service-debug-bot?view=azure-bot-service-4.0).

## <a name="integrating-luis"></a>LUIS tümleştirme

### <a name="where-is-my-luis-app-created-during-the-azure-web-app-bot-subscription-process"></a>LUIS uygulamamı Azure web app botu abonelik işlemi sırasında oluşturulduğu?
Bir LUIS şablonu seçin ve seçin, **seçin** düğmesi Şablon bölmesinde, sol taraftaki bölmede şablon türü içerecek şekilde değiştirir ve LUIS şablonu oluşturmak için hangi bölgede sorar. Web app botu işlemi yine de bir LUIS abonelik oluşturmaz.

![LUIS şablonu web app botu bölgesi](./media/luis-faq/web-app-bot-location.png)

### <a name="what-luis-regions-support-bot-framework-speech-priming"></a>Hangi LUIS bölgeleri Bot Framework konuşma Hazırlama işlemi destekler?
[Konuşma Hazırlama işlemi](https://docs.microsoft.com/bot-framework/bot-service-manage-speech-priming) yalnızca merkezi (ABD) örneğinde LUIS uygulamalar için desteklenir.

## <a name="api-programming-strategies"></a>API programlama stratejileri

### <a name="how-do-i-programmatically-get-the-luis-region-of-a-resource"></a>LUIS bölgesi bir kaynak program aracılığıyla nasıl alabilirim? 

LUIS sample'a [bölge bulma](https://github.com/Azure-Samples/cognitive-services-language-understanding/tree/master/documentation-samples/find-region) kullanılarak programlama yoluyla C# veya Node.Js. 

## <a name="luis-service"></a>LUIS hizmeti

### <a name="is-language-understanding-luis-available-on-premises-or-in-private-cloud"></a>Language Understanding (LUIS) şirket içi kullanılabilir mi ya da özel bulutta?

Evet, LUIS kullanabileceğiniz [kapsayıcı](luis-container-howto.md) kullanımını ölçmek için gerekli bağlantı varsa, bu senaryolar için. 

### <a name="at-the-build-2018-conference-i-heard-about-a-language-understanding-feature-or-demo-but-i-dont-remember-what-it-was-called"></a>Derleme 2018 konferansta ben bir dil anlama özelliği veya tanıtım heard ancak ne çağrıldı hatırlamıyorum?

Aşağıdaki özellikler Build 2018 konferansında yayımlanan:

|Ad|İçerik|
|--|--|
|Geliştirmeler|[Normal ifade](luis-concept-data-extraction.md##regular-expression-entity-data) varlık ve [anahtar tümcecik](luis-concept-data-extraction.md#key-phrase-extraction-entity-data) varlık
|Desenler|Desenler [kavramı](luis-concept-patterns.md), [öğretici](luis-tutorial-pattern.md), [nasıl yapılır](luis-how-to-model-intent-pattern.md)<br>[Patterns.Any](luis-concept-entity-types.md) varlık kavramı da dahil olmak üzere [açık listesi](luis-concept-patterns.md#explicit-lists) özel durumları<br>[Rolleri](luis-concept-roles.md) kavramı|
|Tümleştirmeler|[Metin analizi](https://docs.microsoft.com/azure/cognitive-services/text-analytics/) tümleştirilmesi [yaklaşım analizi](luis-how-to-publish-app.md#enable-sentiment-analysis)<br>[Konuşma](https://docs.microsoft.com/azure/cognitive-services/speech) konuşma Hazırlama işlemi ile birlikte tümleştirilmesi [Speech SDK'sı](https://aka.ms/SpeechSDK)|
|Dağıtım Aracı|Parçası [Botbuilder'da Araçları](https://github.com/Microsoft/botbuilder-tools), gönderme komut satırı [aracı](luis-concept-enterprise.md#when-you-need-to-combine-several-luis-and-qna-maker-apps) tek LUIS uygulamasına bir Bot daha iyi amaç tanıma için birden çok LUIS ve soru-cevap Oluşturucu uygulamaları birleştirmek için

Ek yazma [API yolları](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/authoring-routes.md) dahil edildi.

Videolar:
* [Azure Friday Build 2018'e en: Bilişsel hizmetler - dil (LUIS)](https://channel9.msdn.com/Shows/Azure-Friday/At-Build-2018-Cognitive-Services-Language-LUIS/player)
* [Derleme 2018 AI Show - Language Understanding hizmeti ile yenilikler nelerdir?](https://channel9.msdn.com/Shows/AI-Show/Whats-New-with-Language-Understanding-Service-LUIS/player)
* [Build 2018 Session - Bot zekası, Konuşma Özellikleri ve NLU en iyi uygulamaları](https://channel9.msdn.com/events/Build/2018/BRK3208)
* [Build 2018'e - LUIS güncelleştirmeleri](https://channel9.msdn.com/events/Build/2018/THR3118/player)

Projeler:
* [Contoso Cafe bot](https://github.com/botbuilderbuild2018/build2018demo) tanıtım - GitHub üzerinde kaynak kodu

## <a name="next-steps"></a>Sonraki adımlar

LUIS hakkında daha fazla bilgi edinmek için aşağıdaki kaynaklara bakın:
* [LUIS ile etiketlenmiş bir yığın taşması soru](https://stackoverflow.com/questions/tagged/luis)
* [Hizmetleri MSDN dil akıllı anlama (LUIS) Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=LUIS)
