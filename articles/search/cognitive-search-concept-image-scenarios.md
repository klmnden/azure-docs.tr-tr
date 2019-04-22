---
title: Bilişsel arama - Azure Search görüntüleri işleme ve ayıklama metinden
description: İşleme ve ayıklama metin ve görüntüleri bilişsel diğer bilgi işlem hatları Azure Search'te arama.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: f1491d6b87816dfc70e94e01653567bda101d045
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58916980"
---
#  <a name="how-to-process-and-extract-information-from-images-in-cognitive-search-scenarios"></a>Bilişsel arama senaryolarda görüntülerdeki bilgileri işleme ve ayıklama nasıl

Bilişsel arama, görüntü ve resim dosyaları ile çalışma için çeşitli özellikleri vardır. Belge çözme sırasında kullandığınız *imageAction* fotoğraf veya resimleri Durma işareti "Durdur" sözcüğü gibi alfasayısal metni içeren metin ayıklamak için parametre. Diğer senaryolar için bir fotoğraf bir dandelion ya da "Sarı" rengi "dandelion" gibi bir görüntü metin gösterimi oluşturma içerir. Görüntünün boyutuna gibi hakkındaki meta verileri de ayıklayabilirsiniz.

Bu makalede, görüntü işleme daha ayrıntılı olarak ele alınmaktadır ve bilişsel arama ardışık düzeninde görüntüleri ile çalışmak için yönergeler sağlar.

<a name="get-normalized-images"></a>

## <a name="get-normalized-images"></a>Normalleştirilmiş görüntü alma

Belge çözme işleminin bir parçası olarak yeni bir görüntü dosya veya görüntü dosyaları katıştırılmış işlemek için dizin oluşturucuyu yapılandırma parametreleri kümesi vardır. Bu parametreler, görüntüleri daha da aşağı akış işleme için'leri normalleştirmek için kullanılır. Görüntüleri normalleştirme bunların daha tekdüzen sağlar. Büyük resimler için bir maksimum yükseklik ve genişlik kullanılabilir hale getirmek için yeniden boyutlandırılır. Meta veriler üzerinde yönlendirme sağlanarak görüntüleri için görüntü döndürme dikey yükleme için ayarlanır. Meta veri ayarlamalar her görüntü için oluşturulan karmaşık bir tür içinde yakalanır. 

Görüntü normalleştirmeyi kapatamazsınız. Görüntüleri yineleme becerileri normalleştirilmiş görüntüsü bekler.

| Yapılandırma parametresi | Açıklama |
|--------------------|-------------|
| imageAction   | Katıştırılmış görüntüler veya görüntü dosyaları karşılaştığında, hiçbir işlem yapılmadı "none" ayarlayın. <br/>Belge kırma bir parçası olarak bir dizi normalleştirilmiş görüntüleri oluşturmak için "generateNormalizedImages için" ayarlayın.<br/>Veri kaynağınızdaki PDF için bir çıkış görüntüye her sayfanın burada işlenir normalleştirilmiş görüntüleri bir dizi oluşturmak için "generateNormalizedImagePerPage için" ayarlayın.  İşlevselliğini PDF olmayan dosya türleri için "generateNormalizedImages" ile aynıdır.<br/>"None" olmayan herhangi bir seçenek için görüntü içinde kullanıma sunulacak *normalized_images* alan. <br/>"None". varsayılan değer Bu yapılandırma yalnızca "dataToExtract" "contentAndMetadata" olarak ayarlandığında veri kaynakları, blob testlerinizle ilgili olabilecek <br/>En fazla 1000 görüntülerin belirli bir belgeden ayıklanır. Bir belgede 1000'den fazla görüntü varsa, ilk 1000 ayıklanır ve bir uyarı oluşturulur. |
|  normalizedImageMaxWidth | Oluşturulan normalleştirilmiş görüntüleri için en büyük genişliği (piksel cinsinden). Varsayılan değer 2000'dir.|
|  normalizedImageMaxHeight | Oluşturulan normalleştirilmiş görüntüleri için en fazla yükseklik (piksel cinsinden). Varsayılan değer 2000'dir.|

> [!NOTE]
> Ayarlarsanız *imageAction* özellik "none" dışında bir olmayacak ayarlayamaz *parsingMode* özelliğini "varsayılan" dışında her şey.  Yalnızca iki bu özelliklerden biri için varsayılan olmayan bir değeri, dizin oluşturucu yapılandırmasında ayarlayabilir.

Ayarlama **parsingMode** parametresi `json` (her blob olarak tek bir belge dizini oluşturmak için) veya `jsonArray` (JSON dizileri içeren, blobları ve ayrı bir belge olarak kabul edilmesi için bir dizideki her öğe ihtiyacınız varsa).

Varsayılan değer 2000 piksel normalleştirilmiş görüntüleri en fazla genişlik ve yükseklik tarafından desteklenen en büyük boyutlar dayalı [OCR beceri](cognitive-search-skill-ocr.md) ve [görüntü analizi beceri](cognitive-search-skill-image-analysis.md). İşleme, maksimum sınırı artırmak istiyorsanız, daha büyük görüntülerinde başarısız olabilir.


İçinde imageAction belirtin, [dizin oluşturucu tanımı](https://docs.microsoft.com/rest/api/searchservice/create-indexer) gibi:

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

Zaman *imageAction* bir değere diğer sonra "none" ayarlanmış yeni *normalized_images* alanı görüntüleri dizisi içerir. Her görüntü aşağıdaki üyeleri içerir karmaşık bir türdür:

| Görüntü üyesi       | Açıklama                             |
|--------------------|-----------------------------------------|
| veriler               | BASE64 kodlamalı dize JPEG biçiminde bir normalleştirilmiş görüntüsü.   |
| Genişlik              | Normalleştirilmiş görüntüsünün piksel cinsinden genişliği. |
| Yükseklik             | Normalleştirilmiş görüntüsünün piksel cinsinden yüksekliği. |
| originalWidth      | Normalleştirme önce görüntünün özgün genişliği. |
| originalHeight      | Normalleştirme önce görüntünün özgün yüksekliği. |
| rotationFromOriginal |  Saat yönünün döndürme normalleştirilmiş görüntüyü oluşturmaya oluştu derece cinsinden. 0 derece ve 360 derece arasında bir değer. Bu adım bir kamera veya tarayıcı tarafından oluşturulan görüntü meta verileri okur. Genellikle 90 derece katı. |
| contentOffset |Gelen görüntü burada ayıklanan içerik alandaki karakter uzaklığı. Bu alan yalnızca katıştırılmış görüntüler ile dosyaları için geçerlidir. |

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

## <a name="image-related-skills"></a>Görüntü ile ilgili beceriler

Görüntüleri girdi olarak ele iki yerleşik bilişsel beceriler vardır: [OCR](cognitive-search-skill-ocr.md) ve [görüntü analizi](cognitive-search-skill-image-analysis.md). 

Şu anda bu yetenekler yalnızca belge çözme adımda oluşturulan görüntüleri ile çalışır. Bu nedenle, yalnızca desteklenen giriştir `"/document/normalized_images"`.

### <a name="image-analysis-skill"></a>Görüntü analizi beceri

[Görüntü analizi beceri](cognitive-search-skill-image-analysis.md) zengin görsel özellikleri görüntüsü içeriğine göre ayıklar. Örneğin, bir görüntüden bir açıklamalı alt yazı oluştur, etiketleri oluşturmak veya ünlüleri ve önemli yerleri belirlemek.

### <a name="ocr-skill"></a>OCR beceri

[OCR beceri](cognitive-search-skill-ocr.md) görüntü dosyaları jpg formatından PNG'ler ve bit eşlemler gibi metin ayıklar. Metin ayıklayabilmeniz için Düzen bilgilerinin yanı sıra. Her tanımlanan dizeler için sınırlama kutusu ilişkin düzen bilgilerini sağlar.

OCR beceri metin görüntülerinizi algılamak için kullanılacak algoritmayı seçmenizi sağlar. Şu anda bu iki algoritması, yazdırılan metin için ve başka bir elle yazılmış metinlerde destekler.

## <a name="embedded-image-scenario"></a>Katıştırılmış Resim senaryosu

Yaygın bir senaryo, aşağıdaki adımları gerçekleştirerek tüm dosya içeriğini, hem metin ve resim başlangıç noktasının metin içeren tek bir dize oluşturmayı içerir:  

1. [Normalized_images ayıklayın](#get-normalized-images)
1. OCR beceri kullanarak çalıştırma `"/document/normalized_images"` giriş
1. Bu görüntüleri metin gösterimi dosyasından ayıklanan ham metni ile birleştirin. Kullanabileceğiniz [metin birleştirme](cognitive-search-skill-textmerger.md) beceri hem metin öbekleri büyük tek bir dize olarak birleştirilecek.

Aşağıdaki örnek becerilerine oluşturur bir *merged_text* belgenizin metinsel içeriği içeren alan. Ayrıca, her katıştırılmış görüntüler OCRed metni içerir. 

#### <a name="request-body-syntax"></a>İstek gövdesi sözdizimi
```json
{
  "description": "Extract text from images and merge with content text to produce merged_text",
  "skills":
  [
    {
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
          "name": "mergedText", "targetName" : "merged_text"
        }
      ]
    }
  ]
}
```

Merged_text alana sahip olduğunuza göre dizin oluşturucu Tanımınızda aranabilir bir alanı eşleyebilirsiniz. Tüm metin görüntüleri dahil olmak üzere dosyalarınızın içeriği, arama yapılabilir.

## <a name="visualize-bounding-boxes-of-extracted-text"></a>Ayıklanan metin kutuları sınırlayıcı görselleştirin

Başka bir yaygın bir senaryo arama sonuçları Düzen bilgileri görselleştirmenin. Örneğin, arama sonuçlarındaki bir parçası olarak bir metin parçası görüntüdeki bulunduğu vurgulamak isteyebilirsiniz.

OCR adım normalleştirilmiş görüntülerinde gerçekleştirilir olduğundan, Düzen koordinatları normalleştirilmiş görüntü alanındadır. Normalleştirilmiş görüntü görüntülenirken koordinatları varlığını genellikle bir sorun değildir, ancak bazı koşullarda özgün resmin görüntülemek isteyebilirsiniz. Bu durumda, her bir düzende koordinat noktası için özgün görüntü koordinat sistemi dönüştürün. 

Bir yardımcı, özgün koordinat, normalleştirilmiş koordinatlarına dönüştüren gerekiyorsa aşağıdaki algoritmadan kullanabilirsiniz:

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
+ [Dizin Oluşturucu (REST) oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-indexer)
+ [Görüntü beceri analiz edin](cognitive-search-skill-image-analysis.md)
+ [OCR beceri](cognitive-search-skill-ocr.md)
+ [Metin birleştirme beceri](cognitive-search-skill-textmerger.md)
+ [Bir beceri kümesi tanımlama](cognitive-search-defining-skillset.md)
+ [Zenginleştirilmiş alanlarını eşleme](cognitive-search-output-field-mapping.md)
