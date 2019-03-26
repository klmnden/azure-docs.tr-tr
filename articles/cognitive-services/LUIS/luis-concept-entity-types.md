---
title: Varlık türleri
titleSuffix: Language Understanding - Azure Cognitive Services
description: 'Varlıkları utterance verileri ayıklayın. Varlık türleri veri öngörülebilir ayıklama verin. Varlık iki tür vardır: makine hakkında bilgi edindiniz ve makine öğrendiniz. Konuşma üzerinde çalıştığınız varlık türünü bilmeniz önemlidir.'
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 03/22/2019
ms.author: diberry
ms.openlocfilehash: efe50533a03551a673583265e107263d79cff90a
ms.sourcegitcommit: 72cc94d92928c0354d9671172979759922865615
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2019
ms.locfileid: "58418695"
---
# <a name="entity-types-and-their-purposes-in-luis"></a>Varlık türleri ve bunların amacıyla LUIS

Varlıkları utterance verileri ayıklayın. Varlık türleri veri öngörülebilir ayıklama verin. Varlık iki tür vardır: makine hakkında bilgi edindiniz ve makine öğrendiniz. Konuşma üzerinde çalıştığınız varlık türünü bilmeniz önemlidir. 

## <a name="entity-compared-to-intent"></a>Intent'e karşılaştırıldığında varlık

Varlık, bir sözcük veya tümcecik ayıklanan istediğiniz utterance içinde temsil eder. Bir utterance birçok varlığın veya hiçbiri hiç içerebilir. Bir varlık (yerlerde, öğeleri, kişiler, olayları veya kavramları) benzer nesnelerinin bir koleksiyonunu içeren bir sınıfı temsil eder. Bazen uygulamanızın görevini gerçekleştirmek gerekli olan ve bilgi ıntent'e ilgili varlıkları anlatmaktadır. Örneğin, bir haber arama uygulaması "konu", "kaynak", "anahtar" ve "yayımlama tarihi" Haberler için arama anahtar veri olan gibi varlıklar içerebilir. Seyahat kayıt uygulaması, "Konum", "tarih", "Havayolu" içinde "seyahat class" ve "biletleri" anahtar için uçuş kayıt ("Book uçuş" hedefi için ilgili) bilgilerdir.

Buna karşılık olarak amaç tüm utterance tahminini temsil eder. 

## <a name="entities-help-with-data-extraction-only"></a>Yalnızca veri ayıklama ile varlıkları Yardım

Etiket veya varlık ayıklama, yalnızca BT amacıyla işareti varlıklar, hedefi tahmin ile Yardım değil.

## <a name="entities-represent-data"></a>Varlık verilerini temsil eder.

Utterance çekmek için istediğiniz verilerin varlıklardır. Bu, bir ad, tarih, ürün adı veya herhangi bir kelimelerin grubu olabilir. 

|İfade|Varlık|Veriler|
|--|--|--|
|New York 3 bilet satın alma|Önceden oluşturulmuş numarası<br>Location.Destination|3<br>New York|
|5 Mart Londra New York'tan bilet satın alma|Location.Origin<br>Location.Destination<br>Önceden oluşturulmuş datetimeV2|New York<br>Londra<br>5 Mart 2018|

## <a name="entities-are-optional-but-highly-recommended"></a>Varlıkları isteğe bağlıdır ancak uygulanması önemle önerilir.

Varlıklar, amacı gerekli olsa da, isteğe bağlıdır. Uygulamanızdaki her bir kavram, ancak yalnızca istemci uygulaması eylemi gerçekleştirmek gerekli olanlar için varlıklarınızı gerekmez. 

Botunuzun devam etmesi için gerekli ayrıntıları, konuşma yoksa, bunları eklemeniz gerekmez. Uygulamanız geliştikçe daha sonra ekleyebilirsiniz. 

Bilgileri nasıl kullanacağınız emin değilseniz, birkaç ortak önceden oluşturulmuş varlıklar gibi ekleme [datetimeV2](luis-reference-prebuilt-datetimev2.md), [sıralı](luis-reference-prebuilt-ordinal.md), [e-posta](luis-reference-prebuilt-email.md), ve [telefon numarası ](luis-reference-prebuilt-phonenumber.md).

## <a name="label-for-word-meaning"></a>Word anlamı etiketi

Word choice veya word düzenleme aynıdır, ancak aynı şeyi anlamına gelmez, bu varlıkla etiket değil. 

Aşağıdaki konuşma, word `fair` olan bir eş sesli sözcük. Aynı yazıldığından, ancak farklı bir anlama sahiptir:

|İfade|
|--|
|Ne tür bir ilçe fairs yapıldığını Seattle alanında bu yaz?|
|Geçerli derecelendirme Seattle gözden geçirilmek üzere adil mi?|

Tüm olay verileri bulmak için bir olay varlık istediyseniz, word etiket `fair` ilk utterance, ancak ikinci içinde değil.

## <a name="entities-are-shared-across-intents"></a>Varlıkları amaçları arasında paylaşılır.

Varlıkları amaçları arasında paylaşılır. Bunlar herhangi bir tek ıntent'e ait değil. Amaç ve varlıkları anlamsal olarak ilişkili olabilir ancak özel bir ilişki değil.

Utterance içinde "bana Paris bilet kitap", "İstanbul" konuma başvuran bir varlıktır. Kullanıcının utterance içinde belirtilen varlık tanıma tarafından LUIS istemci uygulamanızın, kullanıcının isteği gerçekleştirmek için belirli eylemleri seçin yardımcı olur.

## <a name="mark-entities-in-none-intent"></a>Varlıkların hiçbiri hedefi işaretle

Dahil olmak üzere tüm hedefleri **hiçbiri** hedefini, varlıklar, mümkün olduğunda işaretlenmiş. Bu varlıkların içinde konuşma nerede ve ne varlıkları sözcüklerdir hakkında daha fazla LUIS yardımcı olur. 

## <a name="entity-status-for-predictions"></a>Tahminler elde etmek için varlık durumu

LUIS portal, ne zaman ya da bir örnek utterance varlıkta olduğunu bildirir. işaretli varlıktan farklı veya başka bir varlıkla ve bu nedenle belirsiz çok yakın. Bu örnek utterance kırmızı alt çizgi ile gösterilir. 

Daha fazla bilgi için [varlık durumu Öngörüler](luis-how-to-add-example-utterances.md#entity-status-predictions). 

## <a name="types-of-entities"></a>Varlık türleri

LUIS, çok sayıda varlık türlerini sunar. Verilerin nasıl ayıklanması gereken ve bu ayıklandıktan sonra nasıl da temsil edilebilir göre varlık seçin.

Varlıklar, varlık utterance içinde nasıl görüneceğini hakkında almaya devam etmek LUIS sağlayan machine-learning ile ayıklanabileceği. Makine tam metin ya da bir normal ifade eşleştirme öğrenme olmadan, varlıkları ayıklanabilir. Varlıkları desenleri, karma bir uygulama ile ayıklanabileceği. 

Varlık ayıklandıktan sonra varlık verilerini bilgilerin tek bir birim olarak temsil edilen veya istemci uygulaması kullanabilirsiniz bilgilerinin bir birim oluşturmak için diğer varlıklarla birleştirilmiş.

|Makine öğrendiniz|Can Mark|Öğretici|Örnek<br>Yanıt|Varlık türü|Amaç|
|--|--|--|--|--|--|
|✔|✔|[✔](luis-tutorial-composite-entity.md)|[✔](luis-concept-data-extraction.md#composite-entity-data)|[**Bileşik**](#composite-entity)|Varlık Türü bağımsız olarak varlıklar gruplandırmasıdır.|
|✔|✔|[✔](luis-quickstart-intent-and-hier-entity.md)|[✔](luis-concept-data-extraction.md#hierarchical-entity-data)|[**Hiyerarşik**](#hierarchical-entity)|Basit varlıkları gruplandırmasıdır.|
|||[✔](luis-quickstart-intent-and-list-entity.md)|[✔](luis-concept-data-extraction.md#list-entity-data)|[**Liste**](#list-entity)|Öğelerin listesini ve bunların eş anlamlılar ile tam metin ayıklandı eşleşir.|
|Karma||[✔](luis-tutorial-pattern.md)|[✔](luis-concept-data-extraction.md#patternany-entity-data)|[**Pattern.Any**](#patternany-entity)|Varlığın son saptamak zor olduğu varlık.|
|||[✔](luis-tutorial-prebuilt-intents-entities.md)|[✔](luis-concept-data-extraction.md#prebuilt-entity-data)|[**Önceden oluşturulmuş**](#prebuilt-entity)|Çeşitli türlerde verileri ayıklamak zaten eğitildi.|
|||[✔](luis-quickstart-intents-regex-entity.md)|[✔](luis-concept-data-extraction.md#regular-expression-entity-data)|[**Normal ifade**](#regular-expression-entity)|Metin eşleştirilecek normal ifade kullanır.|
|✔|✔|[✔](luis-quickstart-primary-and-secondary-data.md)|[✔](luis-concept-data-extraction.md#simple-entity-data)|[**Basit**](#simple-entity)|Tek bir kavram sözcük veya tümcecik olarak içerir.|

Yalnızca makine öğrenilen varlıklar, her amaç için örnek konuşma işaretlenmesi gerekir. Varlıkları en iyi şekilde çalıştığı aracılığıyla test edildiğinde makine öğrenilen [uç nokta sorguları](luis-concept-test.md#endpoint-testing) ve [konuşma uç noktası gözden geçirme](luis-how-to-review-endoint-utt.md). 

Pattern.Any varlıklar olarak işaretlenmesi gerekir [deseni](luis-how-to-model-intent-pattern.md) şablon örnekleri, amaç kullanıcı örnekleri. 

Karma varlıkları varlık algılama yöntemleri bir birleşimini kullanın.

## <a name="composite-entity"></a>Bileşik varlık

Bileşik bir varlık önceden oluşturulmuş varlıklar gibi diğer varlıklar, basit, normal ifade, liste ve hiyerarşik varlıkları oluşur. Ayrı varlıklar, tam bir varlık oluşturur. 

Bu varlık yarar ne zaman uygun veri:

* Birbiriyle ilgilidir. 
* Konuşma bağlamında birbiriyle ilişkilidir.
* Varlık türleri çeşitli kullanın.
* Gruplandırılmış ve bilgi bir birim olarak istemci uygulaması tarafından işlenen gerekir.
* Makine öğrenimi gerektiren kullanıcı konuşma çeşitli vardır.

![Bileşik varlık](./media/luis-concept-entities/composite-entity.png)

[Öğretici](luis-tutorial-composite-entity.md)<br>
[Varlık için örnek JSON yanıtı](luis-concept-data-extraction.md#composite-entity-data)<br>

## <a name="hierarchical-entity"></a>Hiyerarşik varlık

Hiyerarşik bir varlık, alt öğeleri olarak adlandırılan bağlamsal öğrenilen basit varlıkları kategorisidir.

Bu varlık yarar ne zaman uygun veri:

* Basit varlıklardır.
* Konuşma bağlamında birbiriyle ilişkilidir.
* Belirli bir sözcük seçimi, her alt varlık belirtmek için kullanın. Bu sözcüklere örnekler şunlardır: from/to, leaving/headed to, away from/toward (çıkış/varış, ayrılıyor/gidiyor, kaynaktan/hedefe doğru)
* Alt sık aynı utterance öğeleridir. 
* İstemci uygulama tarafından bir bilgi birimi olarak gruplanmaları ve işlenmeleri gerekir.

Kullanmayın:

* Bağlamı bağımsız olarak alt öğeleri için tam metin eşleşme olan bir varlık ihtiyacınız vardır. Kullanım bir [varlık listesinde](#list-entity) bunun yerine. 
* Bir varlık için bir üst-alt ilişkisi diğer varlık türleriyle gerekir. Kullanım [Bileşik varlık](#composite-entity).

![hiyerarşik varlık](./media/luis-concept-entities/hierarchical-entity.png)

[Öğretici](luis-quickstart-intent-and-hier-entity.md)<br>
[Varlık için örnek JSON yanıtı](luis-concept-data-extraction.md#hierarchical-entity-data)<br>

### <a name="roles-versus-hierarchical-entities"></a>Hiyerarşik varlıkları ve rolleri

[Rolleri](luis-concept-roles.md#roles-versus-hierarchical-entities) hiyerarşik varlıklar, ancak tüm varlık türlerine uygulanır, deseni aynı sorunu çözdü. Rolleri şu anda yalnızca desenleri kullanılabilir. Rolleri ıntents örnek konuşma içinde kullanılabilir değil.  

## <a name="list-entity"></a>Liste varlığı

Liste varlık ilgili sözcükleri kendi eş anlamlılar yanı sıra sabit, kapalı bir kümesini temsil eder. LUIS, liste varlıkları için ek değerler bulmaz. Kullanım **önerilir** yeni sözcükleri sunabileceği önerileri görmek için özellik geçerli listede bağlı. Birden fazla liste varlığı ile aynı değeri varsa, her varlık uç nokta sorguda döndürülür. 

İyi bir varlıktır ne zaman uygun metin verileri:

* Bilinen bir kümesidir.
* Küme, bu varlık türü için maksimum LUIS [sınırlarını](luis-boundaries.md) aşmaz.
* Konuşmadaki metin bir eşanlamlı sözcük veya kurallı ad ile tam olarak eşleşiyor. LUIS, tam metin eşleşme ötesinde listesi kullanmaz. Dallanma, gerçekleştirebilse ve diğer Çeşitlemeler bir liste varlığı ile çözümlenmiyor. Değişimleri yönetmek için kullanmayı bir [deseni](luis-concept-patterns.md#syntax-to-mark-optional-text-in-a-template-utterance) isteğe bağlı metin sözdizimine sahip.

![Liste varlığı](./media/luis-concept-entities/list-entity.png)

[Öğretici](luis-quickstart-intent-and-list-entity.md)<br>
[Varlık için örnek JSON yanıtı](luis-concept-data-extraction.md#list-entity-data)

## <a name="patternany-entity"></a>Pattern.Any varlık

Pattern.Any yalnızca bir desenin şablon utterance varlık burada başlar ve biter işaretlemek için kullanılan bir değişken uzunluklu yer tutucudur.  

İyi bir varlıktır ne zaman uygun:

* Varlığın son utterance kalan metinle çakışabilir. 
[Öğretici](luis-tutorial-pattern.md)<br>
[Varlık için örnek JSON yanıtı](luis-concept-data-extraction.md#patternany-entity-data)

**Örnek**  
Belirtilen başlığa göre books ilişkin arayan bir istemci uygulaması, tam başlık pattern.any ayıklar. Bu kitap arama için pattern.any kullanarak bir şablonu utterance `Was {BookTitle} written by an American this year[?]`. 

Aşağıdaki tabloda, her satır utterance iki sürümü vardır. Üst utterance nasıl LUIS utterance başlangıçta görürsünüz olduğundan, kitap başlığıyla belirsiz olduğu başlar ve de sona erer. Ayıklama için yerinde bir desen olduğunda LUIS kitap başlığı nasıl bilir alt utterance olur. 

|İfade|
|--|
|ADAM kimin Zannettiği HIS eşim Hat ve diğer Klinik örnekleri için bu yılın bir Amerikan tarafından yazılmış?<br>Olan **Man kimin Zannettiği HIS eşim Hat ve diğer Klinik örnekleri için** bir Amerikan göre bu yılın yazılmış?|
|Yarı uyku Frog Pajamas bir Amerikan tarafından yazılan bu yıl içinde neydi?<br>Olan **Frog Pajamas içinde yarım uyku** bir Amerikan göre bu yılın yazılmış?|
|Limonlu pasta, belirli üzüntü şöyleydi: Bu yıl bir Amerikan tarafından yazılmış bir Romanım?<br>Olan **Limonlu pasta, belirli üzüntü: Bir Romanım** bir Amerikan göre bu yılın yazılmış?|
|My Pocket içinde bir Wocket yok edildi! Bu yıl bir Amerikan tarafından yazılmış?<br>Olan **My Pocket içinde bir Wocket yoktur!** Bu yıl bir Amerikan tarafından yazılmış?|

## <a name="prebuilt-entity"></a>Önceden oluşturulmuş varlık

Önceden oluşturulmuş varlıklarla e-posta, URL ve telefon numarası gibi ortak kavramlar temsil eden yerleşik türleridir. Önceden oluşturulmuş varlık adları ayrılmıştır. [Tüm önceden oluşturulmuş varlıklar](luis-prebuilt-entities.md) uygulamaya eklenen uç nokta tahmin sorguyu utterance içinde bulunursa döndürülür. 

İyi bir varlıktır ne zaman uygun:

* Verileri önceden oluşturulmuş varlıklar için dil, kültür tarafından desteklenen yaygın bir kullanım örneği ile eşleşir. 

Önceden oluşturulmuş varlıklarla eklenir ve herhangi bir zamanda kaldırıldı.

![Sayı önceden oluşturulmuş varlık](./media/luis-concept-entities/number-entity.png)

[Öğretici](luis-tutorial-prebuilt-intents-entities.md)<br>
[Varlık için örnek JSON yanıtı](luis-concept-data-extraction.md#prebuilt-entity-data)

Bazı önceden oluşturulmuş bu varlıkların açık kaynaklı tanımlanan [tanıyıcıları metin](https://github.com/Microsoft/Recognizers-Text) proje. Belirli bir kültürün veya varlık şu anda desteklenmemektedir, projeye katkıda bulunur. 

### <a name="troubleshooting-prebuilt-entities"></a>Önceden oluşturulmuş varlıklarla ilgili sorunları giderme

Önceden oluşturulmuş bir varlık özel varlığınıza yerine etiketlenmiş olması durumunda LUIS Portalı'nda, bu sorunu gidermek nasıl birkaç seçeneğiniz vardır.

Uygulamaya eklenen önceden oluşturulmuş varlıklarla olacak _her zaman_ döndürülmesi, bile utterance özel varlıklar için aynı metni ayıkla. 

#### <a name="change-tagged-entity-in-example-utterance"></a>Örnek utterance etiketli varlığı değiştirme

Önceden oluşturulmuş varlık özel bir varlık olarak aynı metni veya belirteçleri ise, metni seçin örneğin utterance ve etiketli utterance değiştirin. 

Önceden oluşturulmuş varlık daha fazla metin veya özel varlığınıza daha belirteçleri ile etiketlenmiş olması durumunda, birkaç bu sorunu gidermek nasıl seçeneğiniz vardır:

* [Örnek utterance Kaldır](#remove-example-utterance-to-fix-tagging) yöntemi
* [Önceden oluşturulmuş varlık kaldırmak](#remove-prebuilt-entity-to-fix-tagging) yöntemi

#### <a name="remove-example-utterance-to-fix-tagging"></a>Etiketleme düzeltmek için örnek utterance Kaldır 

İlk seçtiğiniz örnek utterance kaldırmaktır. 

1. Örnek utterance silin.
1. Uygulamayı yeniden eğitme. 
1. Word yeniden ekleyin veya tam bir örnek utterance olarak önceden oluşturulmuş bir varlık olarak işaretlenmiş varlık tümceciği. Bir sözcük veya tümcecik hala işaretlenmiş önceden oluşturulmuş varlık olacaktır. 
1. Örnek utterance varlıkta seçin **hedefi** sayfasında, özel varlığınıza değiştirin ve yeniden eğitin. Bu tam metin işaretlenmesi metnin kullanan herhangi bir örnek konuşma önceden oluşturulmuş varlığı olarak bu LUIS engel olur. 
1. Tüm özgün örnek utterance Intent'e ekleyin. Özel varlık, önceden oluşturulmuş bir varlık yerine işaretlenmesini devam etmelidir. Özel varlık işaretlenmemiş olması, konuşma metnin daha fazla örnek eklemeniz gerekir.

#### <a name="remove-prebuilt-entity-to-fix-tagging"></a>Etiketleme düzeltmek için önceden oluşturulmuş varlık Kaldır

1. Önceden oluşturulmuş varlık uygulamadan kaldırabilir. 
1. Üzerinde **hedefi** sayfasında, özel varlıkta örnek utterance işaretleyin.
1. Uygulamayı eğitin.
1. Önceden oluşturulmuş varlık uygulamaya geri ekleyin ve uygulamayı eğitin. Bu düzeltme, önceden oluşturulmuş varlık bileşik bir varlık parçası olmayan varsayar.

## <a name="regular-expression-entity"></a>Normal ifade varlığı 

Normal bir ifade ham utterance metin için en iyisidir. Küçük büyük harf duyarlı ve kültürel bir değişken yok sayar.  Normal ifadenin eşleştirilmesi, yazım denetimi değişiklikleri karakter düzeyinde belirteci düzeyinde değil sonra uygulanır. Normal ifade birçok oluşur. parantez kullanılarak gibi çok karmaşık ise, ifade modele eklemek erişememenizin. Bölümü ancak bazıları [.NET Regex](https://docs.microsoft.com/dotnet/standard/base-types/regular-expressions) kitaplığı. 

İyi bir varlıktır ne zaman uygun:

* Verileri tutarlı olan herhangi bir değişim ile tutarlı bir şekilde biçimlendirilir.
* Normal ifade 2'den fazla iç içe geçme düzeyi gerek yoktur. 

![Normal ifade varlığı](./media/luis-concept-entities/regex-entity.png)

[Öğretici](luis-quickstart-intents-regex-entity.md)<br>
[Varlık için örnek JSON yanıtı](luis-concept-data-extraction.md#regular-expression-entity-data)<br>

## <a name="simple-entity"></a>Basit varlık 

Bir varlığın tek bir kavram açıklayan ve makine öğrenilen bağlamdan öğrenilen genel bir varlıktır. Basit varlıklar genellikle şirket adı, ürün adlarını veya diğer kategori adları gibi adlar olduğu için ekleme bir [tümcecik listesi](luis-concept-feature.md) kullanılan adları sinyal artırmak üzere basit bir varlık kullanırken. 

İyi bir varlıktır ne zaman uygun:

* Verilerin tutarlı bir şekilde biçimlendirilmeyen ancak aynı şeyi göstermek. 

![varlığın](./media/luis-concept-entities/simple-entity.png)

[Öğretici](luis-quickstart-primary-and-secondary-data.md)<br/>
[Varlık için örnek yanıt](luis-concept-data-extraction.md#simple-entity-data)<br/>

## <a name="entity-limits"></a>Varlık sınırları

Gözden geçirme [sınırları](luis-boundaries.md#model-boundaries) anlamak için bir model ekleyebilirsiniz kaç her varlık türü.

## <a name="composite-vs-hierarchical-entities"></a>Bileşik vs hiyerarşik varlıklar

Bileşik varlıkları ve hiyerarşik varlıkları hem üst-alt ilişkileri ve makine öğrendiniz. Makine öğrenimi, farklı bağlamlardaki (bir kelimelerin düzenleme) tabanlı varlıkları anlamak LUIS sağlar. Bileşik varlıklar, bunların alt öğeleri olarak farklı varlık türleri izin verdiğinden daha esnektir. Hiyerarşik bir varlığın alt yalnızca basit varlıklardır. 

|Tür|Amaç|Örnek|
|--|--|--|
|Hiyerarşik|Üst-alt Basit varlık|Location.Origin=New York<br>Location.Destination=London|
|Bileşik|Üst-alt varlıklar: önceden oluşturulmuş, liste, basit, hiyerarşik| sayı = 3<br>Liste birinci sınıf =<br>prebuilt.datetimeV2=March 5|

## <a name="if-you-need-more-than-the-maximum-number-of-entities"></a>Varlıklar, en fazla sayısından daha ihtiyacınız varsa 

Hiyerarşik ve bileşik varlıkları kullanmanız gerekebilir. Hiyerarşik varlıkları özellikleri paylaşan ya da bir kategori üyesi olan varlıklar arasında ilişki yansıtır. Alt varlıklar, kendi üst kategori, tüm üyeleridir. Örneğin, PlaneTicketClass adlı hiyerarşik bir varlık alt varlıklar EconomyClass ve FirstClass olabilir. Hiyerarşi tek düzey derinliğini aşıyor.  

Bileşik varlık, bir bütün parçalarını temsil eder. Örneğin, PlaneTicketOrder adlı bileşik bir varlık alt varlıklar Havayolu, hedef DepartureCity DepartureDate ve PlaneTicketClass olabilir. Daha önceden mevcut olan basit varlıklar, hiyerarşik varlıklar veya önceden oluşturulmuş varlıklarla alt bileşik bir varlıktan oluşturacaksınız.  

LUIS, makine öğrenilen değildir, ancak bir sabit listesi değerleri belirtmek LUIS uygulamanızı sağlayan listesi varlık türünü de sağlar. Bkz: [LUIS sınırları](luis-boundaries.md) sınırları varlık türleri gözden geçirmek için başvuru. 

Kabul hiyerarşik, bileşik ve listelemeye ve hala en fazla sınıra ihtiyacınız varsa, desteğe başvurun. Bunu yapmak için sisteminizin hakkında ayrıntılı bilgi toplamak, Git [LUIS](luis-reference-regions.md#luis-website) Web sitesine gidin ve ardından **Destek**. Destek Hizmetleri Azure aboneliğinize dahildir, başvurun [Azure teknik desteğine](https://azure.microsoft.com/support/options/). 

## <a name="next-steps"></a>Sonraki adımlar

Kavramları iyi hakkında bilgi edinin [konuşma](luis-concept-utterance.md). 

Bkz: [varlık Ekle](luis-how-to-add-entities.md) LUIS uygulamanızı varlıklar ekleme hakkında daha fazla bilgi için.
