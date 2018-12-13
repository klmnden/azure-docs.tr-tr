---
title: Bilişsel arama beceri varlık tanıma - Azure Search adlı
description: Adlandırılmış varlıklar kişi, konum ve kuruluş için bir Azure Search bilişsel arama ardışık metinden ayıklayın.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: e63354152f8821c5ce975563639c8b87fb332bd4
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53313998"
---
#    <a name="named-entity-recognition-cognitive-skill"></a>Adlandırılmış varlık tanıma bilişsel beceri

**Adlandırılmış varlık tanıma** beceri adlandırılmış varlıklar metni ayıklar. Var olan varlıkları içeren türler `person`, `location` ve `organization`.

> [!NOTE]
> <ul>
> <li>
> 21 aralık 2018 tarihinden itibaren Bilişsel hizmetler kaynağı bir Azure Search beceri kümesi ile ilişkilendirmek mümkün olmayacak. Bu beceri yürütmesi için ücretlendirme başlatmak için bize izin verir. Bu tarihte, biz de belge çözme aşamasının bir parçası olarak görüntü ayıklama için başlayacağız. Belgelerden metin ayıklama işlemi ek masraf olmadan sağlanmaya devam edecektir.>
> Var olan konumunda yerleşik yetenek yürütülmesini ücretlendirilir [Bilişsel hizmetler ödeme-olarak-, Git fiyat](https://azure.microsoft.com/pricing/details/cognitive-services/) . Görüntü ayıklama fiyatlandırma Önizleme fiyatıyla ücretlendirilirsiniz ve üzerinde açıklanmıştır [Azure fiyatlandırma sayfasını arama](https://go.microsoft.com/fwlink/?linkid=2042400). Bilgi [daha fazla](cognitive-search-attach-cognitive-services.md).</li>
> <li> Adlandırılmış varlık tanıma beceri "kullanım dışı" olarak kabul edilir ve resmi olarak 2019 Feburary 15 başlangıç desteklenmez. Listelenen önerilere uyun <a href="cognitive-search-skill-deprecated.md">kullanım dışı Bilişsel arama yetenekleri</a> sayfası için desteklenen bir yetenek geçirmek için</li>

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.NamedEntityRecognitionSkill

## <a name="data-limits"></a>Veri sınırları
Bir kaydın en büyük boyutu tarafından ölçülen 50.000 karakter arasında olmalıdır `String.Length`. Anahtar ifade ayıklayıcısı için göndermeden önce verileri bölün gerekiyorsa kullanmayı [metin bölme beceri](cognitive-search-skill-textsplit.md).

## <a name="skill-parameters"></a>Yetenek parametreleri

Parametreler büyük/küçük harfe duyarlıdır.

| Parametre adı     | Açıklama |
|--------------------|-------------|
| kategoriler    | Ayıklanması gereken kategoriler dizisi.  Olası kategori türleri: `"Person"`, `"Location"`, `"Organization"`. Hiçbir kategori sağlanırsa, tüm türleri döndürülür.|
|defaultLanguageCode |  Giriş metni dil kodu. Aşağıdaki dillerde desteklenmektedir: `de, en, es, fr, it`|
| minimumPrecision  | 0 ile 1 arasında bir sayı. Duyarlık bu değerden düşükse, varlık döndürülmez. Varsayılan değer 0'dır.|

## <a name="skill-inputs"></a>Beceri girişleri

| Adı girin      | Açıklama                   |
|---------------|-------------------------------|
| languageCode  | İsteğe bağlı. `"en"` varsayılan değerdir.  |
| metin          | Analiz edilecek metin.          |

## <a name="skill-outputs"></a>Beceri çıkışları

| Çıkış adı     | Açıklama                   |
|---------------|-------------------------------|
| Kişiler      | Her bir dizenin bir kişinin adını temsil ettiği bir dize dizisi. |
| konumlar  | Her bir dizenin bir konumu temsil ettiği bir dize dizisi. |
| organizations  | Bir kuruluş temsil ettiği her bir dizenin dize dizisi. |
| varlıklar | Karmaşık bir tür dizisi. Her bir karmaşık türü, aşağıdaki alanları içerir: <ul><li>Kategori (`"person"`, `"organization"`, veya `"location"`)</li> <li>değer (gerçek varlık adı)</li><li>uzaklık (Bu metnin bulunduğu konumu)</li><li>güvenle (0 ve 1 arasında bir değer güvenin değerin gerçek bir varlık olduğunu gösterir)</li></ul> |

##  <a name="sample-definition"></a>Örnek tanımı

```json
  {
    "@odata.type": "#Microsoft.Skills.Text.NamedEntityRecognitionSkill",
    "categories": [ "Person", "Location", "Organization"],
    "defaultLanguageCode": "en",
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
             "text": "This is the loan application for Joe Romero, he is a Microsoft employee who was born in Chile and then moved to Australia… Ana Smith is provided as a reference.",
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
        "persons": [ "Joe Romero", "Ana Smith"],
        "locations": ["Chile", "Australia"],
        "organizations":["Microsoft"],
        "entities":  
        [
          {
            "category":"person",
            "value": "Joe Romero",
            "offset": 33,
            "confidence": 0.87
          },
          {
            "category":"person",
            "value": "Ana Smith",
            "offset": 124,
            "confidence": 0.87
          },
          {
            "category":"location",
            "value": "Chile",
            "offset": 88,
            "confidence": 0.99
          },
          {
            "category":"location",
            "value": "Australia",
            "offset": 112,
            "confidence": 0.99
          },
          {
            "category":"organization",
            "value": "Microsoft",
            "offset": 54,
            "confidence": 0.99
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
+ [Varlık tanıma beceri](cognitive-search-skill-entity-recognition.md)
