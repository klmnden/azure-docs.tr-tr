---
title: Azure HALUK uygulamalardaki varlık türlerini anlama | Microsoft Docs
description: Varlıkları (uygulamanızın etki alanındaki anahtar verilerini) dil anlama akıllı hizmet (HALUK) uygulamalarında ekleyin.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/22/2018
ms.author: v-geberr
ms.openlocfilehash: ccb7269109309355e2af95f6fb2aa060c1998b22
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36286027"
---
# <a name="entities-in-luis"></a>HALUK varlıklar

Sözcük veya tümcecik anahtar verileri, uygulamanızın etki alanındaki utterances varlıklardır.

## <a name="entity-compared-to-intent"></a>Varlık amacı için karşılaştırma
Varlık bir sözcük veya tümcecik ayıklanan istediğiniz utterance içinde temsil eder. Bir utterance birçok varlık veya hiçbiri hiç içerebilir. Bir varlık benzer nesneler (yerler, noktalar, kişiler, olayları veya kavramlar) koleksiyonunu içeren bir sınıfı temsil eder. Hedefi için ilgili bilgi varlıkları açıklamakta ve bazı durumlarda, görevi gerçekleştirmek uygulamanız için gerekli olan. Örneğin, bir haber arama uygulaması "konu", "kaynak", "anahtar" ve "yayımlama tarihi" Haberler için aranacak anahtar verileri olan gibi varlıkları içerebilir. Seyahat kayıt uygulama, "Konum", "tarih", "hava yolu" "seyahat sınıf" ve "biletleri" anahtar uçuş kayıt ("Bookflight" hedefi için ilgili) için bilgilerdir.

Karşılaştırma amacını tüm utterance tahmin temsil eder. 

## <a name="entities-represent-data"></a>Varlık verilerini temsil eder
Utterance çekme istediğiniz verileri varlıklardır. Bu, bir ad, tarih, ürün adı veya sözcüklerin herhangi bir grubu olabilir. 

|Utterance|Varlık|Veriler|
|--|--|--|
|New York için 3 bilet Al|Önceden oluşturulmuş numarası<br>Location.Destination|3<br>New York|
|New York biletindeki Londra'ya 5 Mart satın alın|Location.Origin<br>Location.Destination<br>Önceden oluşturulmuş datetimeV2|New York<br>Londra<br>5 Mart 2018|

## <a name="entities-are-optional-but-highly-recommended"></a>Varlıkları isteğe bağlıdır, ancak önemle önerilir.
Hedefleri gerekli olsa da, varlıkları isteğe bağlıdır. Uygulamanızdaki her kavram, ancak yalnızca uygulamanın eylemi için gereken varlıklar oluşturmak gerekmez. 

Utterances, bot devam etmesi için gerekli ayrıntıları yoksa bunları eklemeniz gerekmez. Uygulamanız geliştikçe daha sonra ekleyebilirsiniz. 

Bilgileri nasıl kullanacağınız emin değilseniz, birkaç datetimeV2, sıralı gibi ortak önceden oluşturulmuş varlıklar, e-posta ve telefon numarası ekleyin.

## <a name="label-for-word-meaning"></a>Word anlamı etiketi
Word seçim veya word düzenleme aynıdır, ancak aynı şey ifade etmez, varlıkla etiketlemeyi değil. 

Aşağıdaki utterances, word `fair` bir eş sesli sözcük değil. Aynı yazıldığından ancak farklı bir anlama sahip:

```
What kind of county fairs are happening in the Seattle area this summer?
Is the current rating for the Seattle review fair?
```

Tüm olay verileri bulmak için bir olay varlık istediyseniz, word etiket `fair` ilk utterance, ancak ikinci içinde değil.

## <a name="entities-are-shared-across-intents"></a>Varlıklar arasında hedefleri paylaşılır
Varlıkları hedefleri arasında paylaşılır. Bunlar, tek bir hedefi ait değil. Hedefleri ve varlıkları anlamsal olarak ilişkili olabilir, ancak özel bir relationship değil.

Utterance "Benim Paris bilet kitap", "Paris" türü konumunun bir varlıktır. Kullanıcının giriş açıklanan varlıkları algılamayı tarafından HALUK amacına karşılamak için yapılması gereken belirli işlemler seçmenize yardımcı olur.

## <a name="assign-entities-in-none-intent"></a>Hedefi hiçbiri varlıklarda atayın
Dahil olmak üzere tüm hedefleri **hiçbiri** amacı, etiketli varlıklar sahip olmalıdır. Bu varlıklar utterances nerede ve ne sözcükleri varlıklardır hakkında daha fazla bilgi edinin HALUK yardımcı olur. 

## <a name="types-of-entities"></a>Varlık türlerini
HALUK türlerde varlıklar sunar. önceden oluşturulmuş varlıklar, varlık ve liste varlıkları özel makine öğrendiniz.

| Ad | Etiketleyebilirsiniz | Açıklama |
| -- |--|--|
| **Önceden oluşturulmuş** <br/>[Özel](#prebuilt)| |  **Tanım**<br>Ortak kavramlar temsil eden yerleşik türleri. <br><br>**Liste**<br/>anahtar tümcecik numarası, sıra, sıcaklık, boyut, para, geçerlilik süresi, yüzde, e-posta, URL, telefon numarası ve anahtar tümcecik. <br><br>Önceden oluşturulmuş varlık adları ayrılmıştır. <br><br>Uygulamaya eklenen tüm önceden oluşturulmuş varlıklar döndürülür [endpoint](luis-glossary.md#endpoint) sorgu. Daha fazla bilgi için bkz: [önceden oluşturulmuş varlıklar](./Pre-builtEntities.md). <br/><br/>[Varlık için örnek yanıt](luis-concept-data-extraction.md#prebuilt-entity-data)|
|<!-- added week of 3/21/08 --> **Normal ifade**<br/>[RegEx](#regex)||**Tanım**<br>Biçimlendirilmiş ham utterance metin için özel normal ifade. Küçük büyük harf duyarlı ve kültürel değişken yok sayar.  <br><br>Bu varlık kelimeler ve aynı zamanda tutarlı olan çeşitlemeleri ile tutarlı bir şekilde biçimlendirilmiş ifadeler için uygundur.<br><br>Normal ifadeyle eşleşen yazım değişiklikleri sonra uygulanır. <br><br>Normal ifade birçok parantez kullanılarak gibi çok karmaşık ise, ifade modele eklemek mümkün değildir. <br><br>**Örnek**<br>`kb[0-9]{6,}` KB123456 eşleşir.<br/><br/>[Hızlı Başlangıç](luis-quickstart-intents-regex-entity.md)<br>[Varlık için örnek yanıt](luis-concept-data-extraction.md)|
| **Basit** <br/>[Makine öğrendiniz](#machine-learned) | ✔ | **Tanım**<br>Bir varlığın tek bir kavram açıklayan ve makine öğrenilen bağlamından öğrenilen genel bir varlıktır. Bağlam, sözcük seçimi, word yerleştirme ve utterance uzunluğu içerir.<br/><br/>Bu, kelimeler ve tutarlı bir şekilde biçimlendirilmemiş ancak aynı şeyi gösteren ifadeler için iyi bir varlıktır. <br/><br/>[Hızlı Başlangıç](luis-quickstart-primary-and-secondary-data.md)<br/>[Varlık için örnek yanıt](luis-concept-data-extraction.md#simple-entity-data)|  
| **Liste** <br/>[Tam eşleşme](#exact-match)|| **Tanım**<br>Liste varlık sisteminizde kendi synoymns birlikte ilgili sözcükler sabit, kapalı bir kümesini temsil eder. <br><br>Her bir liste varlık, bir veya daha fazla form olabilir. Aynı kavram temsil edecek şekilde çeşidi bilinen bir dizi için en iyi şekilde kullanılır.<br/><br/>HALUK listesi varlıklar için ek değerler bulmaz. Kullanım görmek için [anlamsal sözlük](luis-glossary.md#semantic-dictionary) geçerli listesini temel alan yeni sözcükleri yönelik öneriler bulmak için.<br/><br>Aynı değere sahip birden fazla listesi varlık varsa, her bir varlık uç nokta sorguda döndürülür. <br/><br/>[Hızlı Başlangıç](luis-quickstart-intent-and-list-entity.md)<br>[Varlık için örnek yanıt](luis-concept-data-extraction.md#list-entity-data)| 
| **Pattern.Any** <br/>[Karma](#mixed) | ✔|**Tanım**<br>Patterns.Any yalnızca bir deseninin şablonu utterance içinde olduğu varlık başlar ve biter işaretlemek için kullanılan bir değişken uzunlukta yer tutucudur.  <br><br>**Örnek**<br>Başlığa göre books utterance Ara verildiğinde, pattern.any tam başlık ayıklar. Pattern.Any kullanarak bir şablon utterance olan `Who wrote {BookTitle}[?]`.<br/><br/>[Öğretici](luis-tutorial-pattern.md)<br>[Varlık için örnek yanıt](luis-concept-data-extraction.md#composite-entity-data)|  
| **Bileşik** <br/>[Makine öğrendiniz](#machine-learned) | ✔|**Tanım**<br>Bileşik varlık ait diğer varlıkların önceden oluşturulmuş varlıklar gibi ve basit, yapılır. Ayrı varlıklar tam bir varlık oluşturur. Bileşik varlıklar listesi varlıklar izin verilmiyor. <br><br>**Örnek**<br>PlaneTicketOrder adlı bir Bileşik varlık alt varlıkları önceden oluşturulmuş olabilir `number` ve `ToLocation`. <br/><br/>[Öğretici](luis-tutorial-composite-entity.md)<br>[Varlık için örnek yanıt](luis-concept-data-extraction.md#composite-entity-data)|  
| **Hiyerarşik** <br/>[Makine öğrendiniz](#machine-learned) |✔ | **Tanım**<br>Bağlam öğrenilen varlıklar kategorisine buna hiyerarşik bir varlıktır.<br><br>**Örnek**<br>Hiyerarşik bir varlığı verilen `Location` olan `ToLocation` ve `FromLocation`, her alt göre belirlenebilir **bağlamı** utterance içinde. Utterance içinde `Book 2 tickets from Seattle to New York`, `ToLocation` ve `FromLocation` bağlam farklı sözcükler etrafında tabanlı. <br/><br/>**Kullanmayın**<br>Bağlam bağımsız olarak alt öğeleri için tam metin eşleşen sahip bir varlık arıyorsanız, liste varlık kullanmanız gerekir. Diğer varlık türleriyle için bir üst-alt ilişkisi arıyorsanız, Bileşik varlık kullanmanız gerekir.<br/><br/>[Hızlı Başlangıç](luis-quickstart-intent-and-hier-entity.md)<br>[Varlık için örnek yanıt](luis-concept-data-extraction.md#hierarchical-entity-data)|

<a name="prebuilt"></a>
**Önceden oluşturulmuş** varlıklardır HALUK tarafından sağlanan özel varlıklar. Bu varlıklar bazıları açık kaynak tanımlanan [tanıyıcıları metin](https://github.com/Microsoft/Recognizers-Text) projesi. Vardır birçok [örnekler](https://github.com/Microsoft/Recognizers-Text/tree/master/Specs) desteklenen kültürler için /Specs dizininde. Belirli kültür veya varlık şu anda desteklenmiyorsa, projeye katkıda. 

<a name="machine-learned"></a>
**Makine öğrenilen** varlıklar iş en iyi aracılığıyla sınandığında [endpoint sorguları](luis-concept-test.md#endpoint-testing) ve [endpoint utterances gözden geçirme](label-suggested-utterances.md). 

<a name="regex"></a>
**Normal ifade varlıkları** kullanıcı varlık tanımının bir parçası sağlar, normal bir ifade tarafından tanımlanır. 

<a name="exact-match"></a>
**Tam eşleşme** varlıklar varlıkta sağlanan metni eşleşen bir tam metin yapmak için kullanın.

<a name="mixed"></a>
**Karma** varlıklar, varlık algılama yöntemlerinin bir birleşimini kullanın.

## <a name="entity-limits"></a>Varlık sınırları
Gözden geçirme [sınırları](luis-boundaries.md#model-boundaries) bir modele ekleyebilirsiniz her varlık türü kaç anlamak için.

## <a name="composite-vs-hierarchical-entities"></a>Bileşik vs hiyerarşik varlıklar
Bileşik varlıkları ve hiyerarşik varlıkları hem üst-alt ilişkisi olan ve makine öğrendiniz. Machine learning farklı bağlamları (sözcükler düzenlenmesi) tabanlı varlıklar anlamak HALUK sağlar. Bileşik varlıklar farklı varlık türleri alt öğeleri olarak izin verdiğinden daha esnektir. Hiyerarşik bir varlığın alt yalnızca basit varlıklardır. 

|Tür|Amaç|Örnek|
|--|--|--|
|Hiyerarşik|Üst-alt Basit varlık|Location.Origin=New York<br>Location.Destination=London|
|Bileşik|Üst-alt varlıkları: önceden oluşturulmuş, liste basit, hiyerarşik| sayı = 3<br>Liste birinci sınıf =<br>prebuilt.datetimeV2=March 5|

## <a name="data-matching-multiple-entities"></a>Birden çok varlık eşleşen veri
Bir sözcük veya tümcecik birden fazla varlık eşleşirse, uç nokta sorgu her bir varlık döndürür. Önceden oluşturulmuş sayı varlık ve prebuild datetimeV2 ekleyin ve bir utterance varsa `create meeting on 2018/03/12 for lunch with wayne`, HALUK tüm varlıkları tanır ve JSON bitiş noktası yanıt bir parçası olarak bir dizi varlıkları döndürür: 

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
Bir sözcük veya tümcecik birden fazla listesi varlığı eşleşirse, uç nokta sorgu her bir liste varlık döndürür.

Sorgu için `when is the best time to go to red rock?`, ve word uygulamasına sahipse `red` birden fazla listesinde HALUK tüm varlıkları tanır ve JSON bitiş noktası yanıt bir parçası olarak bir dizi varlıkları döndürür: 

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

## <a name="if-you-need-more-than-the-maximum-number-of-entities"></a>Birden çok varlık sayısı gerekiyorsa 

Hiyerarşik ve bileşik varlıkları kullanmanız gerekebilir. Hiyerarşik varlıklar özelliklere sahiptir veya bir kategori üyeleri olan varlıklar arasındaki ilişki yansıtır. Alt varlıkları kendi üst öğenin kategori tüm üyeleridir. Örneğin, PlaneTicketClass adlı hiyerarşik bir varlık alt varlıkları EconomyClass ve FirstClass olabilir. Hiyerarşi, yalnızca bir derinlik düzeyini yayar.  

Bileşik varlık tamamını bölümlerini temsil eder. Örneğin, PlaneTicketOrder adlı bir Bileşik varlık alt varlıkları hava yolu, hedef, DepartureCity, DepartureDate ve PlaneTicketClass olabilir. Önceden var olan basit varlıklar, alt hiyerarşik varlıklar veya önceden oluşturulmuş varlıklar bileşik bir varlık oluşturun.  

HALUK değil makine-öğrenilen ancak HALUK uygulamanızın sabit değerler listesini belirtmenizi sağlar liste varlık türü de sağlar. Bkz: [HALUK sınırları](luis-boundaries.md) başvuru listesi varlık türünün sınırları gözden geçirin. 

Kabul hiyerarşik, bileşik ve listelemeye ve hala sınırdan daha ihtiyacınız varsa, desteğe başvurun. Bunu yapmak için sisteminizle ilgili ayrıntılı bilgi toplamak, Git [HALUK] [ LUIS] Web sitesine gidin ve ardından **Destek**. Destek Hizmetleri Azure aboneliğinize içeriyorsa, kişi [Azure teknik destek](https://azure.microsoft.com/support/options/). 

## <a name="best-practices"></a>En iyi uygulamalar

Oluşturma bir [varlık](luis-concept-entity-types.md) çağıran uygulama veya bot gerektiği zaman bazı parametrelerin veya bir eylem yürütmek için gerekli utterance verileri. Bir varlık bir kelime olduğundan veya gereksinim duyduğunuz utterance ifadeye--belki de işlevi için parametre olarak ayıklanır. 

Uygulamanıza eklemek için varlığı doğru türünü seçmek için bu verileri kullanıcılar tarafından nasıl girildiğini bilmeniz gerekir. Machine learning, kapalı listesi veya normal ifade gibi farklı bir mekanizma kullanarak her bir varlık türü bulunamadı. Emin değilseniz, basit bir varlıkla başlar ve bir sözcük veya tümceciği tüm utterances verilerin hiçbiri de dahil olmak üzere tüm hedefleri arasında temsil eder hedefi etiket.  

Uç nokta utterances burada bir varlık bulunabilir normal bir ifade olarak tanımlanan veya bir tam metin eşleşme ile yaygın kullanım bulmak için düzenli olarak gözden geçirin.  

Gözden geçirme bir parçası olarak, bir sinyal için HALUK kelimeler ve ifadeler, etki alanınıza önemli ancak tam eşleşme değildir ve HALUK yüksek güvenilirlik yok için bir tümcecik listesine eklemeyi düşünün.  

Bkz: [en iyi uygulamalar](luis-concept-best-practices.md) daha fazla bilgi için.

## <a name="next-steps"></a>Sonraki adımlar

Kavramları iyi hakkında bilgi edinin [utterances](luis-concept-utterance.md). 

Bkz: [varlıkları ekleyin](luis-how-to-add-entities.md) HALUK uygulamanıza varlıklar ekleme hakkında daha fazla bilgi için.

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website