---
title: İşlem ve Azure Search'te görüntülerden metni ayıklayın | Microsoft Docs
description: Ardışık Düzen işlemi ve ayıklama metin ve görüntüleri bilişsel diğer bilgileri Azure Search'te arayın.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: 77fbd69aad6c78ecd5c933d8017c980afaa661a3
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
#  <a name="how-to-process-and-extract-information-from-images-in-cognitive-search-scenarios"></a>İşlem ve bilişsel arama senaryolarda görüntülerden bilgi ayıklamak nasıl

Bilişsel arama görüntüler ve görüntü dosyaları ile çalışma için çeşitli özellikleri vardır. Belge çözme sırasında kullandığınız *imageAction* fotoğraf veya resimleri Durma işareti Word'de "Durdur" gibi alfasayısal metni içeren metin ayıklamak için parametre. Görüntü, fotoğraf bir dandelion ya da "Sarı" renk için "dandelion" gibi bir metin gösterimini oluşturma diğer senaryolar içerir. Ayrıca, görüntünün boyutuna gibi hakkındaki meta verileri ayıklayabilirsiniz.

Bu makalede daha ayrıntılı olarak işlenirken resim kapsayan ve görüntüleri bilişsel arama ardışık düzende çalışmak için yönergeler sağlanmaktadır.

<a name="get-normalized-images"></a>

## <a name="get-normalized-images"></a>Normalleştirilmiş görüntüleri alma

Belge çözme bir parçası olarak, yeni bir görüntü dosyalarını veya dosyalarında katıştırılmış resimleri işlemek için dizin oluşturucu yapılandırma parametreleri kümesini vardır. Bu parametreleri daha aşağı akış işleme görüntülerde normalleştirmek için kullanılır. Görüntüleri normalleştirme bunları daha tekdüzen yapar. Görüntülerin büyük bir maksimum yükseklik ve genişliğine tüketilebilir yapmak için boyutlandırılır. Meta veriler üzerinde yönlendirme sağlayan görüntüler için görüntüyü döndürme dikey yükleme için ayarlanır. Meta veri ayarlamalar her görüntü için oluşturulan bir karmaşık türde yakalanır. 

Görüntü normalleştirmeyi kapatamazsınız. Görüntüleri yineleme becerileri normalleştirilmiş görüntüsü bekler.

| Yapılandırma parametresi | Açıklama |
|--------------------|-------------|
| imageAction   | Katıştırılmış Resim veya görüntü dosyaları karşılaştığında hiçbir işlem yapılmadı "hiçbiri" ayarlayın. <br/>Belge çözme bir parçası olarak bir dizi normalleştirilmiş görüntüleri oluşturmak için "generateNormalizedImages" ayarlayın. Bu görüntüleri de sağlanmaktadır *normalized_images* alan. <br/>Varsayılan olarak "yok." kullanılır Bu yapılandırma yalnızca "dataToExtract" "contentAndMetadata" olarak ayarlandığında veri kaynakları, blob ilgili |
|  normalizedImageMaxWidth | Oluşturulan normalleştirilmiş görüntüleri için en büyük genişliği (piksel cinsinden). Varsayılan değer 2000'dir.|
|  normalizedImageMaxHeight | Oluşturulan normalleştirilmiş görüntüleri için maksimum yüksekliği (piksel cinsinden). Varsayılan değer 2000'dir.|

> [!NOTE]
> Ayarlarsanız *imageAction* özelliği "hiçbiri" dışında her şey için edemeyecek ayarlamak *parsingMode* "varsayılan" dışında bir şey özelliğine.  Yalnızca bu iki özelliklerden biri için varsayılan olmayan bir değer dizin oluşturucu yapılandırmanızda ayarlamanız.

Ayarlama **parsingMode** parametresi `json` (tek bir belge olarak her bir blob dizini oluşturmak için) veya `jsonArray` (bloblarınızın JSON dizileri içerir ve ayrı bir belge olarak kabul edilmesi için bir dizinin her bir öğesine ihtiyacınız varsa).

Varsayılan değer 2000 piksel normalleştirilmiş görüntüleri maksimum genişlik ve yükseklik tarafından desteklenen en büyük boyutlar dayanır [OCR yetenek](cognitive-search-skill-ocr.md) ve [görüntü analiz yetenek](cognitive-search-skill-image-analysis.md). Maksimum sınırı artırmak istiyorsanız, işleme büyük görüntülerinde başarısız olabilir.


İmageAction belirtin, [dizin oluşturucu tanımı](ref-create-indexer.md) gibi:

```json
{
  //...rest of your indexer definition goes here ...
  "parameters":
  {
    "configuration": 
    {
        "dataToExtract": "contentAndMetadata",
        "imageAction": "generateNormalizedImages"
    }
  }
}
```

Zaman *imageAction* "generateNormalizedImages" ayarlamak yeni *normalized_images* alanı görüntüleri bir dizi içerir. Her görüntünün aşağıdaki üyeleri olan bir karmaşık türü şöyledir:

| Görüntü üyesi       | Açıklama                             |
|--------------------|-----------------------------------------|
| veriler               | Base64 ile kodlanmış JPEG biçiminde normalleştirilmiş görüntünün dizesi.   |
| Genişlik              | Normalleştirilmiş görüntüsünün piksel cinsinden genişliği. |
| Yükseklik             | Normalleştirilmiş görüntüsünün piksel cinsinden yüksekliği. |
| originalWidth      | Görüntünün normalleştirme önce özgün genişliği. |
| originalHeight      | Görüntünün normalleştirme önce özgün yüksekliği. |
| rotationFromOriginal |  Yönünün döndürme normalleştirilmiş görüntüsü oluşturmak için oluştu derece cinsinden. 0 derece-360 derece arasında bir değer. Bu adım, bir kamera veya tarayıcı tarafından oluşturulan görüntüsünden meta verileri okur. Genellikle birden fazla 90 derece. |
| contentOffset |Burada görüntü ayıklanan içerik alan içerisindeki karakter uzaklığı. Bu alan yalnızca katıştırılmış görüntüler dosyalarıyla için geçerlidir. |

 Örnek değeri *normalized_images*:
```json
[
  {
    "data": "BASE64 ENCODED STRING OF A JPEG IMAGE",
    "width": 500,
    "height": 300,
    "originalWidth": 5000,  
    "originalHeight": 3000,
    "rotationFromOriginal": 90,
    "contentOffset": 500  
  }
]
```

## <a name="image-related-skills"></a>Görüntü ile ilgili yetenekleri

Görüntüleri bir giriş olarak ele iki yerleşik bilişsel becerileri vardır: [OCR](cognitive-search-skill-ocr.md) ve [görüntü analiz](cognitive-search-skill-image-analysis.md). 

Şu anda bu becerileri yalnızca belge çözme adımda oluşturulan görüntülerle çalışır. Bu nedenle, yalnızca desteklenen giriş olduğunu `"/document/normalized_images"`.

### <a name="image-analysis-skill"></a>Görüntü analiz nitelik

[Görüntü analiz yetenek](cognitive-search-skill-image-analysis.md) zengin bir görüntü içeriğine göre görsel özellikleri ayıklar. Örneği için bir resim yazısı görüntüden oluşturmak, etiketleri oluşturmak veya çok ünlüler ve bilinen yerler belirleyin.

### <a name="ocr-skill"></a>OCR nitelik

[OCR yetenek](cognitive-search-skill-ocr.md) jpg formatından, PNG'ler ve bit eşlemler gibi görüntü dosyalarından metin ayıklar. Metin ayıklayabilirsiniz düzeni bilgilerinin yanı sıra. Düzen bilgilerini her tanımlanan dizeler için sınırlayıcı kutuları sağlar.

OCR yetenek metin görüntülerinizi algılamak için kullanılacak algoritmayı seçmenize olanak sağlar. Şu anda bu iki algoritmalar, yazdırılan metin için bir ve başka bir el yazısı metni destekler.

## <a name="embedded-image-scenario"></a>Katıştırılmış Resim senaryosu

Yaygın bir senaryo, aşağıdaki adımları gerçekleştirerek tüm dosya içeriklerini, metin ve resim başlangıç noktasının metin içeren tek bir dize oluşturma içerir:  

1. [Normalized_images Ayıkla](#get-normalized-images)
1. Yetenek OCR kullanarak çalıştırmak `"/document/normalized_images"` giriş olarak
1. Bu görüntüleri metin gösterimi dosyasından ayıklanan ham metni ile birleştirin. Kullanabileceğiniz [metin birleştirme](cognitive-search-skill-textmerger.md) her iki metin öbekleri tek büyük bir dize halinde birleştirmek için yetenek.

Aşağıdaki örnek skillset oluşturur bir *merged_text* belgenizin metinsel içeriğini içeren alan. Ayrıca, her katıştırılmış görüntüler OCRed metni içerir. 

#### <a name="request-body-syntax"></a>İstek gövdesi sözdizimi
```json
{
  "description": "Extract text from images and merge with content text to produce merged_text",
  "skills":
  [
    {
        "name": "OCR skill",
        "description": "Extract text (plain and structured) from image.",
        "@odata.type": "#Microsoft.Skills.Vision.OcrSkill",
        "context": "/document/normalized_images/*",
        "defaultLanguageCode": "en",
        "detectOrientation": true,
        "inputs": [
          {
            "name": "image",
            "source": "/document/normalized_images/*"
          }
        ],
        "outputs": [
          {
            "name": "text"
          }
        ]
    },
    {
      "@odata.type": "#Microsoft.Skills.Text.MergeSkill",
      "description": "Create merged_text, which includes all the textual representation of each image inserted at the right location in the content field.",
      "context": "/document",
      "insertPreTag": " ",
      "insertPostTag": " ",
      "inputs": [
        {
          "name":"text", "source": "/document/content"
        },
        {
          "name": "itemsToInsert", "source": "/document/normalized_images/*/text"
        },
        {
          "name":"offsets", "source": "/document/normalized_images/*/contentOffset" 
        }
      ],
      "outputs": [
        {
          "name": "mergedText", "targetname" : "merged_text"
        }
      ]
    }
  ]
}
```

Merged_text alana sahip olduğunuza göre dizin oluşturucu tanımınızı aranabilir alan olarak eşleştirebilirsiniz. Tüm görüntüleri metin dahil olmak üzere, dosyaların içeriğini aranabilir olacaktır.

## <a name="visualize-bounding-boxes-of-extracted-text"></a>Ayıklanan metin kutuları sınırlayıcı Görselleştirme

Başka bir yaygın bir senaryo, arama sonuçları düzeni bilgileri görselleştirme. Örneğin, bir parça metin arama sonuçlarınızı bir parçası olarak bir resim bulunduğu vurgulayın isteyebilirsiniz.

Normalleştirilmiş görüntülerinde OCR adım gerçekleştirilir olduğundan, Düzen koordinatları normalleştirilmiş görüntü alanındadır. Normalleştirilmiş resim görüntülerken, koordinatları varlığını genellikle bir sorun değildir, ancak bazı durumlarda özgün görüntüsüne görüntülemek isteyebilirsiniz. Bu durumda, her bir düzende koordinat noktası özgün resim koordinat sistemi dönüştürün. 

Özgün koordinat, normalleştirilmiş koordinatlara dönüştürmek isterseniz, bir yardımcı, aşağıdaki algoritması kullanabilirsiniz:

```csharp
        /// <summary>
        ///  Converts a point in the normalized coordinate space to the original coordinate space.
        ///  This method assumes the rotation angles are multiples of 90 degrees.
        /// </summary>
        public static Point GetOriginalCoordinates(Point normalized,
                                    int originalWidth,
                                    int originalHeight,
                                    int width,
                                    int height,
                                    double rotationFromOriginal)
        {
            Point original = new Point();
            double angle = rotationFromOriginal % 360;

            if (angle == 0 )
            {
                original.X = normalized.X;
                original.Y = normalized.Y;
            } else if (angle == 90)
            {
                original.X = normalized.Y;
                original.Y = (width - normalized.X);
            } else if (angle == 180)
            {
                original.X = (width -  normalized.X);
                original.Y = (height - normalized.Y);
            } else if (angle == 270)
            {
                original.X = height - normalized.Y;
                original.Y = normalized.X;
            }

            double scalingFactor = (angle % 180 == 0) ? originalHeight / height : originalHeight / width;
            original.X = (int) (original.X * scalingFactor);
            original.Y = (int)(original.Y * scalingFactor);

            return original;
        }
```

## <a name="see-also"></a>Ayrıca bkz.
+ [Dizin Oluşturucu (REST) oluşturma](ref-create-indexer.md)
+ [Görüntü yetenek Çözümle](cognitive-search-skill-image-analysis.md)
+ [OCR nitelik](cognitive-search-skill-ocr.md)
+ [Metin birleştirme nitelik](cognitive-search-skill-textmerger.md)
+ [Bir skillset tanımlama](cognitive-search-defining-skillset.md)
+ [Zenginleştirilmiş alanlarını eşleme](cognitive-search-output-field-mapping.md)
