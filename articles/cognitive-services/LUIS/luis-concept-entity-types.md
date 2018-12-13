---
title: Varlık türleri
titleSuffix: Language Understanding - Azure Cognitive Services
description: Language Understanding Intelligent Service (LUIS) uygulamalarında varlıklar (anahtar, uygulamanızın etki alanı veri) ekleyin.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: diberry
ms.openlocfilehash: f0e543263c7a9890abc485d0f0cd6bec88f16dd4
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2018
ms.locfileid: "53135211"
---
# <a name="entity-types-and-their-purposes-in-luis"></a>Varlık türleri ve bunların amacıyla LUIS

Sözcükleri veya tümcecikleri anahtar verileri, uygulamanızın etki alanındaki konuşma varlıklardır.

## <a name="entity-compared-to-intent"></a>Intent'e karşılaştırıldığında varlık
Varlık, bir sözcük veya tümcecik ayıklanan istediğiniz utterance içinde temsil eder. Bir utterance birçok varlığın veya hiçbiri hiç içerebilir. Bir varlık (yerlerde, öğeleri, kişiler, olayları veya kavramları) benzer nesnelerinin bir koleksiyonunu içeren bir sınıfı temsil eder. Bazen uygulamanızın görevini gerçekleştirmek gerekli olan ve bilgi ıntent'e ilgili varlıkları anlatmaktadır. Örneğin, bir haber arama uygulaması "konu", "kaynak", "anahtar" ve "yayımlama tarihi" Haberler için arama anahtar veri olan gibi varlıklar içerebilir. Seyahat kayıt uygulaması, "Konum", "tarih", "Havayolu" içinde "seyahat class" ve "biletleri" anahtar için uçuş kayıt ("Bookflight" hedefi için ilgili) bilgilerdir.

Buna karşılık olarak amaç tüm utterance tahminini temsil eder. 

## <a name="entities-represent-data"></a>Varlık verilerini temsil eder.
Utterance çekmek için istediğiniz verilerin varlıklardır. Bu, bir ad, tarih, ürün adı veya herhangi bir kelimelerin grubu olabilir. 

|İfade|Varlık|Veriler|
|--|--|--|
|New York 3 bilet satın alma|Önceden oluşturulmuş numarası<br>Location.Destination|3<br>New York|
|5 Mart Londra New York'tan bilet satın alma|Location.Origin<br>Location.Destination<br>Önceden oluşturulmuş datetimeV2|New York<br>Londra<br>5 Mart 2018|

## <a name="entities-are-optional-but-highly-recommended"></a>Varlıkları isteğe bağlıdır ancak uygulanması önemle önerilir.
Varlıklar, amacı gerekli olsa da, isteğe bağlıdır. Uygulamanızdaki her bir kavram, ancak yalnızca uygulamanın eylem için gerekli varlıkları oluşturmak gerekmez. 

Botunuzun devam etmesi için gerekli ayrıntıları, konuşma yoksa, bunları eklemeniz gerekmez. Uygulamanız geliştikçe daha sonra ekleyebilirsiniz. 

Bilgileri nasıl kullanacağınız emin değilseniz, birkaç ortak önceden oluşturulmuş varlıklar datetimeV2 sıralı gibi e-posta ve telefon numarası ekleyin.

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

Utterance içinde "bana Paris bilet kitap", "İstanbul" türü konumun bir varlıktır. Uygulamasında kullanıcı girdisi belirtilen varlık tanıma tarafından LUIS bir hedefini karşılamak için belirli eylemler seçmenize yardımcı olur.

## <a name="assign-entities-in-none-intent"></a>Varlıkların hiçbiri hedefi atama
Dahil olmak üzere tüm hedefleri **hiçbiri** amacı, varlıkları etiketlenmiş olması gerekir. Bu varlıkların içinde konuşma nerede ve ne varlıkları sözcüklerdir hakkında daha fazla LUIS yardımcı olur. 

## <a name="entity-status-for-predictions"></a>Tahminler elde etmek için varlık durumu

Bkz: [varlık durumu Öngörüler](luis-how-to-add-example-utterances.md#entity-status-predictions) daha fazla bilgi için. 

## <a name="types-of-entities"></a>Varlık türleri
LUIS, çok sayıda varlık türlerini sunar. önceden oluşturulmuş varlıklar, varlıkları ve varlıklar listesi özel makine öğrendiniz.

| Ad | Etiketleyebilirsiniz | Açıklama |
| -- |--|--|
| **Önceden oluşturulmuş** <br/>[Özel](#prebuilt)| |  **Tanım**<br>Genel kavramlar temsil eden yerleşik türler. <br><br>**Liste**<br/>anahtar ifade sayı, sıra, sıcaklık, boyut, para, yaş, yüzde, e-posta, URL, telefon numarası ve anahtar ifade. <br><br>Önceden oluşturulmuş varlık adları ayrılmıştır. <br><br>Uygulamaya eklenen tüm önceden oluşturulmuş varlıklar döndürülür [uç nokta](luis-glossary.md#endpoint) sorgu. Daha fazla bilgi için [önceden oluşturulmuş varlıklarla](./luis-prebuilt-entities.md). <br/><br/>[Varlık için örnek yanıt](luis-concept-data-extraction.md#prebuilt-entity-data)|
|<!-- added week of 3/21/08 --> **Normal ifade**<br/>[Normal ifade](#regex)||**Tanım**<br>Biçimlendirilmiş ham utterance metin için özel normal ifade. Küçük büyük harf duyarlı ve kültürel bir değişken yok sayar.  <br><br>Bu varlık sözcük ve tümcecikleri tutarlı olan herhangi bir değişim ile tutarlı bir şekilde biçimlendirilmiş için uygundur.<br><br>Normal ifadenin eşleştirilmesi, yazım denetimi değişiklikleri karakter düzeyinde belirteci düzeyinde değil sonra uygulanır. Bölümü ancak bazıları [.Net Regex](https://docs.microsoft.com/dotnet/standard/base-types/regular-expressions) kitaplığı.<br><br>Normal ifade birçok oluşur. parantez kullanılarak gibi çok karmaşık ise, ifade modele eklemek mümkün değildir. <br><br>**Örnek**<br>`kb[0-9]{6,}` KB123456 eşleşir.<br/><br/>[Hızlı Başlangıç](luis-quickstart-intents-regex-entity.md)<br>[Varlık için örnek yanıt](luis-concept-data-extraction.md)|
| **Basit** <br/>[Makine öğrendiniz](#machine-learned) | ✔ | **Tanım**<br>Bir varlığın tek bir kavram açıklayan ve makine öğrenilen bağlamından öğrenilen genel bir varlıktır. Bağlam, sözcük seçimi, word yerleştirme ve utterance uzunluğu içerir.<br/><br/>Bu, sözcük ve tümcecikleri tutarlı bir şekilde biçimlendirilmemiş, ancak aynı şeyi göstermek için iyi bir varlıktır. <br/><br/>[Hızlı Başlangıç](luis-quickstart-primary-and-secondary-data.md)<br/>[Varlık için örnek yanıt](luis-concept-data-extraction.md#simple-entity-data)|  
| **Liste** <br/>[Tam eşleşme](#exact-match)|| **Tanım**<br>Liste varlıkları kendi synoymns yanı sıra ilgili sözcükleri sabit, kapalı bir dizi sisteminizde temsil eder. <br><br>Her liste varlığı, bir veya daha fazla form olabilir. Aynı kavram temsil etmek için yol çeşitli bilinen bir dizi için en iyi şekilde kullanılır.<br/><br/>LUIS, liste varlıkları için ek değerler bulmaz. Kullanım **önerilir** yeni sözcükleri sunabileceği önerileri görmek için özellik geçerli listede bağlı.<br/><br>Birden fazla liste varlığı ile aynı değeri varsa, her varlık uç nokta sorguda döndürülür. <br/><br/>[Hızlı Başlangıç](luis-quickstart-intent-and-list-entity.md)<br>[Varlık için örnek yanıt](luis-concept-data-extraction.md#list-entity-data)| 
| **Pattern.Any** <br/>[Karma](#mixed) | ✔|**Tanım**<br>Patterns.Any yalnızca bir desenin şablon utterance varlık burada başlar ve biter işaretlemek için kullanılan bir değişken uzunluklu yer tutucudur.  <br><br>**Örnek**<br>Başlığa göre kitap için bir utterance arama göz önünde bulundurulduğunda, tam başlık pattern.any ayıklar. Bir şablon utterance pattern.Any kullanarak olan `Who wrote {BookTitle}[?]`.<br/><br/>[Öğretici](luis-tutorial-pattern.md)<br>[Varlık için örnek yanıt](luis-concept-data-extraction.md#composite-entity-data)|  
| **Bileşik** <br/>[Makine öğrendiniz](#machine-learned) | ✔|**Tanım**<br>Bileşik bir varlık oluşur diğer önceden oluşturulmuş varlıklar, basit gibi varlıkları, regex, hiyerarşik liste. Ayrı varlıklar, tam bir varlık oluşturur. <br><br>**Örnek**<br>Önceden oluşturulmuş alt varlıklar PlaneTicketOrder adlı bileşik bir varlık olabilir `number` ve `ToLocation`. <br/><br/>[Öğretici](luis-tutorial-composite-entity.md)<br>[Varlık için örnek yanıt](luis-concept-data-extraction.md#composite-entity-data)|  
| **Hiyerarşik** <br/>[Makine öğrendiniz](#machine-learned) |✔ | **Tanım**<br>Hiyerarşik bir varlık, basit bağlamsal öğrenilen varlıklar kategorisidir.<br><br>**Örnek**<br>Hiyerarşik bir varlığın verilen `Location` çocuklarla `ToLocation` ve `FromLocation`, her alt göre belirlenebilir **bağlam** utterance içinde. Utterance içinde `Book 2 tickets from Seattle to New York`, `ToLocation` ve `FromLocation` bağlamsal farklı sözcüklerin etrafına bağlı. <br/><br/>**Kullanmayın**<br>Bağlamı bağımsız olarak alt öğeleri için tam metin eşleşme olan bir varlık için kullanmak istiyorsanız, bir liste varlığı kullanmanız gerekir. Bir üst-alt ilişkisi için diğer varlık türleriyle arıyorsanız, bileşik varlığı kullanmanız gerekir.<br/><br/>[Hızlı Başlangıç](luis-quickstart-intent-and-hier-entity.md)<br>[Varlık için örnek yanıt](luis-concept-data-extraction.md#hierarchical-entity-data)|

<a name="prebuilt"></a>
**Önceden oluşturulmuş** LUIS tarafından sağlanan özel varlıklar varlıklardır. Bu varlıklardan bazıları açık kaynaklı tanımlanan [tanıyıcıları metin](https://github.com/Microsoft/Recognizers-Text) proje. Kullanabileceğiniz birçok [örnekler](https://github.com/Microsoft/Recognizers-Text/tree/master/Specs) desteklenen kültürler /Specs dizininde. Belirli bir kültürün veya varlık şu anda desteklenmemektedir, projeye katkıda bulunur. 

<a name="machine-learned"></a>
**Makine öğrenilen** varlıkları en iyi şekilde çalıştığı aracılığıyla test edildiğinde [uç nokta sorguları](luis-concept-test.md#endpoint-testing) ve [konuşma uç noktası gözden geçirme](luis-how-to-review-endoint-utt.md). 

<a name="regex"></a>
**Normal ifade varlıkları** kullanıcı varlığı tanımının bir parçası sağlar, normal bir ifade tarafından tanımlanır. 

<a name="exact-match"></a>
**Tam eşleşme** varlıkları varlık içinde sağlanan metin eşleşen bir tam metin yapmak için kullanın.

<a name="mixed"></a>
**Karma** varlıkları bir varlık algılama yöntemleri birleşimini kullanın.

## <a name="entity-limits"></a>Varlık sınırları
Gözden geçirme [sınırları](luis-boundaries.md#model-boundaries) anlamak için bir model ekleyebilirsiniz kaç her varlık türü.

## <a name="roles-versus-hierarchical-entities"></a>Hiyerarşik varlıkları ve rolleri

Daha fazla bilgi için bkz. [Rollerle hiyerarşik varlıkların karşılaştırması](luis-concept-roles.md#roles-versus-hierarchical-entities).

## <a name="composite-vs-hierarchical-entities"></a>Bileşik vs hiyerarşik varlıklar
Bileşik varlıkları ve hiyerarşik varlıkları hem üst-alt ilişkileri ve makine öğrendiniz. Makine öğrenimi, farklı bağlamlardaki (bir kelimelerin düzenleme) tabanlı varlıkları anlamak LUIS sağlar. Bileşik varlıklar, bunların alt öğeleri olarak farklı varlık türleri izin verdiğinden daha esnektir. Hiyerarşik bir varlığın alt yalnızca basit varlıklardır. 

|Tür|Amaç|Örnek|
|--|--|--|
|Hiyerarşik|Üst-alt Basit varlık|Location.Origin=New York<br>Location.Destination=London|
|Bileşik|Üst-alt varlıklar: önceden oluşturulmuş, liste, basit, hiyerarşik| sayı = 3<br>Liste birinci sınıf =<br>prebuilt.datetimeV2=March 5|

## <a name="data-matching-multiple-entities"></a>Birden çok varlık eşleşen veri
Bir sözcük veya tümcecik birden fazla varlık eşleşirse, uç nokta sorgu her bir varlık döndürür. Önceden oluşturulmuş sayı varlık hem prebuild datetimeV2 ekleyin ve bir utterance sahip `create meeting on 2018/03/12 for lunch with wayne`, LUIS tüm varlıkları tanır ve JSON bitiş noktası yanıtın bir parçası varlıkları bir dizi döndürür: 

```JSON
{
  "query": "create meeting on 2018/03/12 for lunch with wayne",
  "topScoringIntent": {
    "intent": "Calendar.Add",
    "score": 0.9333419
  },
  "entities": [
    {
      "entity": "2018/03/12",
      "type": "builtin.datetimeV2.date",
      "startIndex": 18,
      "endIndex": 27,
      "resolution": {
        "values": [
          {
            "timex": "2018-03-12",
            "type": "date",
            "value": "2018-03-12"
          }
        ]
      }
    },
    {
      "entity": "2018",
      "type": "builtin.number",
      "startIndex": 18,
      "endIndex": 21,
      "resolution": {
        "value": "2018"
      }
    },
    {
      "entity": "03/12",
      "type": "builtin.number",
      "startIndex": 23,
      "endIndex": 27,
      "resolution": {
        "value": "0.25"
      }
    }
  ]
}
```

## <a name="data-matching-multiple-list-entities"></a>Birden çok listesi varlık eşleşen veri
Bir sözcük veya tümcecik birden fazla liste varlığı eşleşirse, uç nokta sorgu her liste varlığı döndürür.

Sorgu için `when is the best time to go to red rock?`, ve uygulama word `red` LUIS birden fazla listesinde, tüm varlıkları tanıyan ve JSON bitiş noktası yanıtın bir parçası varlıkları bir dizi döndürür: 

```JSON
{
  "query": "when is the best time to go to red rock?",
  "topScoringIntent": {
    "intent": "Calendar.Find",
    "score": 0.06701678
  },
  "entities": [
    {
      "entity": "red",
      "type": "Colors",
      "startIndex": 31,
      "endIndex": 33,
      "resolution": {
        "values": [
          "Red"
        ]
      }
    },
    {
      "entity": "red rock",
      "type": "Cities",
      "startIndex": 31,
      "endIndex": 38,
      "resolution": {
        "values": [
          "Destinations"
        ]
      }
    }
  ]
}
```

## <a name="if-you-need-more-than-the-maximum-number-of-entities"></a>Varlıklar, en fazla sayısından daha ihtiyacınız varsa 

Hiyerarşik ve bileşik varlıkları kullanmanız gerekebilir. Hiyerarşik varlıkları özellikleri paylaşan ya da bir kategori üyesi olan varlıklar arasında ilişki yansıtır. Alt varlıklar, kendi üst kategori, tüm üyeleridir. Örneğin, PlaneTicketClass adlı hiyerarşik bir varlık alt varlıklar EconomyClass ve FirstClass olabilir. Hiyerarşi tek düzey derinliğini aşıyor.  

Bileşik varlık, bir bütün parçalarını temsil eder. Örneğin, PlaneTicketOrder adlı bileşik bir varlık alt varlıklar Havayolu, hedef DepartureCity DepartureDate ve PlaneTicketClass olabilir. Daha önceden mevcut olan basit varlıklar, hiyerarşik varlıklar veya önceden oluşturulmuş varlıklarla alt bileşik bir varlıktan oluşturacaksınız.  

LUIS LUIS uygulamanızı sabit değerler listesini belirtmenizi sağlar ancak değil makine-öğrenilen listesi varlık türünü de sağlar. Bkz: [LUIS sınırları](luis-boundaries.md) sınırları varlık türleri gözden geçirmek için başvuru. 

Kabul hiyerarşik, bileşik ve listelemeye ve hala en fazla sınıra ihtiyacınız varsa, desteğe başvurun. Bunu yapmak için sisteminizin hakkında ayrıntılı bilgi toplamak, Git [LUIS](luis-reference-regions.md#luis-website) Web sitesine gidin ve ardından **Destek**. Destek Hizmetleri Azure aboneliğinize dahildir, başvurun [Azure teknik desteğine](https://azure.microsoft.com/support/options/). 

## <a name="best-practices"></a>En iyi uygulamalar

Oluşturma bir [varlık](luis-concept-entity-types.md) çağıran uygulama veya bot gerektiğinde bazı parametreler veya bir eylemi çalıştırmak için gerekli utterance verileri. Bir varlık bir sözcük ya da ihtiyacınız utterance ifadeye--belki de bir işlev için parametre olarak ayıklanır. 

Uygulamanıza eklemek için varlık doğru türünü seçmek için kullanıcılar tarafından verilerin nasıl girildiğini bilmek gerekir. Her varlık türü kullanarak makine öğrenimi, kapalı listesi veya normal ifade gibi farklı bir mekanizma bulunur. Emin değilseniz, basit bir varlık ile başlamalı ve bir sözcük veya tümcecik tüm konuşma verilerin hiçbiri dahil olmak üzere tüm hedefleri arasında temsil eden hedefi etiket.  

Burada bir varlık normal bir ifade olarak tanımlanan veya bir tam metin eşleşme ile genel kullanım bulmak için düzenli olarak konuşma uç noktası gözden geçirin.  

Gözden geçirme işleminin bir parçası olarak, bir sinyal için LUIS sözcükleri veya tümcecikleri, etki alanınıza önemlidir, ancak tam eşleşme değildir ve LUIS yüksek güvenilirliğe sahip değil bir ifade listesine eklemeyi düşünün.  

Bkz: [en iyi uygulamalar](luis-concept-best-practices.md) daha fazla bilgi için.

## <a name="next-steps"></a>Sonraki adımlar

Kavramları iyi hakkında bilgi edinin [konuşma](luis-concept-utterance.md). 

Bkz: [varlık Ekle](luis-how-to-add-entities.md) LUIS uygulamanızı varlıklar ekleme hakkında daha fazla bilgi için.