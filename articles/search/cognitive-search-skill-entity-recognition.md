---
title: Varlık tanıma bilişsel arama beceri - Azure Search
description: Bir Azure Search bilişsel arama ardışık metinden değişik tür varlıklar ayıklayın.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: f05161dbbfd9293cd7b1cbf447bb7ca1c313250c
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65023455"
---
#    <a name="entity-recognition-cognitive-skill"></a>Varlık tanıma bilişsel beceri

**Varlık tanıma** beceri farklı türde varlıkları metni ayıklar. Bu yetenek, makine öğrenimi modellerini tarafından sağlanan kullanan [metin analizi](https://docs.microsoft.com/azure/cognitive-services/text-analytics/overview) Bilişsel Hizmetler'e gösterdiğiniz.

> [!NOTE]
> Kapsam işleme sıklığını artırarak daha fazla belgelerin eklenmesi genişletmeniz veya daha fazla yapay ZEKA algoritmalarının eklenmesi gerekir [Faturalanabilir bir Bilişsel hizmetler kaynağı ekleme](cognitive-search-attach-cognitive-services.md). API'leri, Bilişsel hizmetler ve Azure Search'te belge çözme aşamasının bir parçası olarak görüntü ayıklama çağırırken ücretler tahakkuk. Metin ayıklama belgelerden için ücretlendirme yoktur.
>
> Yerleşik yetenek yürütülmesi sırasında mevcut ücretlendirilir [Bilişsel hizmetler ödeme-olarak-, Git fiyat](https://azure.microsoft.com/pricing/details/cognitive-services/). Görüntü ayıklama fiyatlandırma üzerinde açıklanmıştır [Azure fiyatlandırma sayfasını arama](https://go.microsoft.com/fwlink/?linkid=2042400).


## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.EntityRecognitionSkill

## <a name="data-limits"></a>Veri sınırları
Bir kaydın en büyük boyutu tarafından ölçülen 50.000 karakter arasında olmalıdır `String.Length`. Anahtar ifade ayıklayıcısı için göndermeden önce verileri bölün gerekiyorsa kullanmayı [metin bölme beceri](cognitive-search-skill-textsplit.md).

## <a name="skill-parameters"></a>Yetenek parametreleri

Parametreleri büyük küçük harfe duyarlıdır ve tümü isteğe bağlıdır.

| Parametre adı     | Açıklama |
|--------------------|-------------|
| kategoriler    | Ayıklanması gereken kategoriler dizisi.  Olası kategori türleri: `"Person"`, `"Location"`, `"Organization"`, `"Quantity"`, `"Datetime"`, `"URL"`, `"Email"`. Hiçbir kategori sağlanırsa, tüm türleri döndürülür.|
|defaultLanguageCode |  Giriş metni dil kodu. Aşağıdaki dillerde desteklenmektedir: `de, en, es, fr, it`|
|minimumPrecision | Kullanılmayan. Gelecekte kullanılmak üzere ayrılmış. |
|includeTypelessEntities | Metin, iyi bilinen bir varlık içerdiğinden, ancak desteklenen kategorilerden birini kategorilere olamaz, true olarak ayarlandığında, bunu bir parçası olarak döndürülecek `"entities"` karmaşık çıkış alanı. 
Bu, iyi bilinen ancak geçerli desteklenen "Kategoriler" bir parçası olarak sınıflandırılan değil varlıklardır. Örneği için bilinen bir varlık (ürün) "Windows 10" olan, ancak "Ürünler" Bugün desteklenen kategorileri değildir. Varsayılan değer `false` |


## <a name="skill-inputs"></a>Beceri girişleri

| Giriş adı      | Açıklama                   |
|---------------|-------------------------------|
| languageCode  | İsteğe bağlı. `"en"` varsayılan değerdir.  |
| metin          | Analiz edilecek metin.          |

## <a name="skill-outputs"></a>Beceri çıkışları

> [!NOTE]
> Tüm varlık kategorileri, tüm diller için desteklenir. Yalnızca _tr_, _es_ destek ayıklanmasıyla `"Quantity"`, `"Datetime"`, `"URL"`, `"Email"` türleri.

| Çıkış adı     | Açıklama                   |
|---------------|-------------------------------|
| Kişiler      | Her bir dizenin bir kişinin adını temsil ettiği bir dize dizisi. |
| konumlar  | Her bir dizenin bir konumu temsil ettiği bir dize dizisi. |
| organizations  | Bir kuruluş temsil ettiği her bir dizenin dize dizisi. |
| Miktar  | Her bir dizenin bir miktar temsil ettiği bir dize dizisi. |
| tarih/saat  | Her bir dizenin temsil ettiği bir tarih/saat (metnin göründüğü gibi) dize dizisi değeri. |
| URL'leri | Her dize bir URL temsil ettiği bir dize dizisi |
| e-postalar | Her bir dizenin bir e-posta temsil ettiği bir dize dizisi |
| namedEntities | Aşağıdaki alanları içeren bir dizi karmaşık türleri: <ul><li>category</li> <li>değer (gerçek varlık adı)</li><li>uzaklık (Bu metnin bulunduğu konumu)</li><li>güvenle (şimdilik kullanılmayan. -1 değerine ayarlanır)</li></ul> |
| varlıklar | Şu alanlara sahip bir metin ayıklanan varlıkları hakkında zengin bilgiler içeren karmaşık bir tür dizisi <ul><li> ad (gerçek varlık adı. Bu, "normalleştirilmiş" form temsil eder)</li><li> wikipediaId</li><li>wikipediaLanguage</li><li>wikipediaUrl (Wikipedia sayfasında varlık için bir bağlantı)</li><li>bingId</li><li>türü (tanınan bir varlığın kategori)</li><li>alt tür (yalnızca belirli kategorileri için kullanılabilir, bu varlık türü daha ayrıntılı bir görünümünü sağlar)</li><li> (içeren karmaşık bir koleksiyon) eşleşir<ul><li>Metin (varlık için ham metin)</li><li>uzaklık (konum burada bulundu)</li><li>uzunluk (ham varlık metnin uzunluğunu)</li></ul></li></ul> |

##  <a name="sample-definition"></a>Örnek tanımı

```json
  {
    "@odata.type": "#Microsoft.Skills.Text.EntityRecognitionSkill",
    "categories": [ "Person", "Email"],
    "defaultLanguageCode": "en",
    "includeTypelessEntities": true,
    "inputs": [
      {
        "name": "text",
        "source": "/document/content"
      }
    ],
    "outputs": [
      {
        "name": "persons",
        "targetName": "people"
      },
      {
        "name": "emails",
        "targetName": "contact"
      },
      {
        "name": "entities"
      }
    ]
  }
```
##  <a name="sample-input"></a>Örnek Giriş

```json
{
    "values": [
      {
        "recordId": "1",
        "data":
           {
             "text": "Contoso corporation was founded by John Smith. They can be reached at contact@contoso.com",
             "languageCode": "en"
           }
      }
    ]
}
```

##  <a name="sample-output"></a>Örnek çıktı

```json
{
  "values": [
    {
      "recordId": "1",
      "data" : 
      {
        "persons": [ "John Smith"],
        "emails":["contact@contoso.com"],
        "namedEntities": 
        [
          {
            "category":"Person",
            "value": "John Smith",
            "offset": 35,
            "confidence": -1
          }
        ],
        "entities":  
        [
          {
            "name":"John Smith",
            "wikipediaId": null,
            "wikipediaLanguage": null,
            "wikipediaUrl": null,
            "bingId": null,
            "type": "Person",
            "subType": null,
            "matches": [{
                "text": "John Smith",
                "offset": 35,
                "length": 10
            }]
          },
          {
            "name": "contact@contoso.com",
            "wikipediaId": null,
            "wikipediaLanguage": null,
            "wikipediaUrl": null,
            "bingId": null,
            "type": "Email",
            "subType": null,
            "matches": [
            {
                "text": "contact@contoso.com",
                "offset": 70,
                "length": 19
            }]
          },
          {
            "name": "Contoso",
            "wikipediaId": "Contoso",
            "wikipediaLanguage": "en",
            "wikipediaUrl": "https://en.wikipedia.org/wiki/Contoso",
            "bingId": "349f014e-7a37-e619-0374-787ebb288113",
            "type": null,
            "subType": null,
            "matches": [
            {
                "text": "Contoso",
                "offset": 0,
                "length": 7
            }]
          }
        ]
      }
    }
  ]
}
```


## <a name="error-cases"></a>Hata durumları
Belge için dil kodu desteklenmiyor, hata döndürülür ve varlık yok ayıklanır.

## <a name="see-also"></a>Ayrıca bkz.

+ [Önceden tanımlanmış beceriler](cognitive-search-predefined-skills.md)
+ [Bir beceri kümesi tanımlama](cognitive-search-defining-skillset.md)
