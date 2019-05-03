---
title: Görüntü analizi bilişsel arama beceri - Azure Search
description: Bir Azure Search zenginleştirme hattında ImageAnalysis bilişsel yeteneği kullanarak görüntü analizi aracılığıyla anlam metin ayıklayın.
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
ms.openlocfilehash: bb18c858a17e290a8ce2cc88dc1e7d88d21afe0f
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65021900"
---
#   <a name="image-analysis-cognitive-skill"></a>Görüntü analizi bilişsel beceri

**Görüntü analizi** beceri zengin görsel özellikleri görüntüsü içeriğine göre ayıklar. Örneğin, bir görüntüden bir açıklamalı alt yazı oluştur, etiketleri oluşturmak veya ünlüleri ve önemli yerleri belirlemek. Bu yetenek, makine öğrenimi modellerini tarafından sağlanan kullanan [görüntü işleme](https://docs.microsoft.com/azure/cognitive-services/computer-vision/home) Bilişsel Hizmetler'e gösterdiğiniz. 

> [!NOTE]
> Kapsam işleme sıklığını artırarak daha fazla belgelerin eklenmesi genişletmeniz veya daha fazla yapay ZEKA algoritmalarının eklenmesi gerekir [Faturalanabilir bir Bilişsel hizmetler kaynağı ekleme](cognitive-search-attach-cognitive-services.md). API'leri, Bilişsel hizmetler ve Azure Search'te belge çözme aşamasının bir parçası olarak görüntü ayıklama çağırırken ücretler tahakkuk. Metin ayıklama belgelerden için ücretlendirme yoktur.
>
> Yerleşik yetenek yürütülmesi sırasında mevcut ücretlendirilir [Bilişsel hizmetler ödeme-olarak-, Git fiyat](https://azure.microsoft.com/pricing/details/cognitive-services/). Görüntü ayıklama fiyatlandırma üzerinde açıklanmıştır [Azure fiyatlandırma sayfasını arama](https://go.microsoft.com/fwlink/?linkid=2042400).


## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Vision.ImageAnalysisSkill 

## <a name="skill-parameters"></a>Yetenek parametreleri

Parametreler büyük/küçük harfe duyarlıdır.

| Parametre adı     | Açıklama |
|--------------------|-------------|
| defaultLanguageCode   |  Döndürülecek dil belirten bir dize. Hizmet, belirli bir dilde tanıma sonuçları döndürür. Bu parametre belirtilmezse, varsayılan değer "en" dir. <br/><br/>Desteklenen diller şunlardır: <br/>*tr* -İngilizce (varsayılan) <br/> *zh* -Basitleştirilmiş Çince|
|visualFeatures |   Döndürülecek visual özelliği türlerini belirten bir dize dizisi. Geçerli görsel özellik türleri şunlardır:  <ul><li> *kategorileri* -görüntü içeriğini Bilişsel hizmetler tanımlanmış bir taksonomi göre kategorilere ayırır [belgeleri](https://docs.microsoft.com/azure/cognitive-services/computer-vision/category-taxonomy).</li><li> *etiketleri* -görüntünün bir kelimelerin görüntü içerikle ilgili ayrıntılı bir liste ile etiketler.</li><li>*Açıklama* -tam İngilizce bir cümle ile içerik görüntüsü açıklar.</li><li>*Yüzleri* -yüzleri mevcut olup olmadığını algılar. Varsa, koordinatları, cinsiyet ve yaş oluşturur.</li><li> *ImageType* -görüntüyü küçük resim veya çizim olup olmadığını algılar.</li><li>  *Renk* -baskın renk, Vurgu rengi belirler ve bir resmin siyah olup olmadığını beyaz.</li><li>*Yetişkin* -görüntü (çıplaklık veya seks Yasası gösterilmektedir) doğası gereği pornografik olup olmadığını algılar. Cinsel müstehcen içerik de algılandı.</li></ul> Görsel özellikleri adları büyük/küçük harfe duyarlıdır.|
| ayrıntılar   | Döndürülecek hangi etki alanına özgü ayrıntıları belirten bir dize dizisi. Geçerli görsel özellik türleri şunlardır: <ul><li>*Ünlüleri* -ünlüleri görüntüde algılanırsa tanımlar.</li><li>*Yer işareti* -görüntüde algılanırsa yer işareti tanımlar.</li></ul>
 |

## <a name="skill-inputs"></a>Beceri girişleri

| Giriş adı      | Açıklama                                          |
|---------------|------------------------------------------------------|
| image         | Karmaşık tür. "/ Belge/normalized_images" alan şu anda yalnızca çalışır, Azure Blob Dizin Oluşturucu tarafından üretilen olduğunda ```imageAction``` dışında bir değere ayarlanmış ```none```. Bkz: [örnek](#sample-output) daha fazla bilgi için.|



##  <a name="sample-definition"></a>Örnek tanımı
```json
{
    "@odata.type": "#Microsoft.Skills.Vision.ImageAnalysisSkill",
    "context": "/document/normalized_images/*",
    "visualFeatures": [
        "Tags",
        "Faces",
        "Categories",
        "Adult",
        "Description",
        "ImageType",
        "Color"
    ],
    "defaultLanguageCode": "en",
    "inputs": [
        {
            "name": "image",
            "source": "/document/normalized_images/*"
        }
    ],
    "outputs": [
        {
            "name": "categories",
            "targetName": "myCategories"
        },
        {
            "name": "tags",
            "targetName": "myTags"
        },
        {
            "name": "description",
            "targetName": "myDescription"
        },
        {
            "name": "faces",
            "targetName": "myFaces"
        },
        {
            "name": "imageType",
            "targetName": "myImageType"
        },
        {
            "name": "color",
            "targetName": "myColor"
        },
        {
            "name": "adult",
            "targetName": "myAdultCategory"
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
            "data": {
                "image": {
                    "data": "BASE64 ENCODED STRING OF A JPEG IMAGE",
                    "width": 500,
                    "height": 300,
                    "originalWidth": 5000,
                    "originalHeight": 3000,
                    "rotationFromOriginal": 90,
                    "contentOffset": 500
                }
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
            "data": {
                "categories": [
                    {
                        "name": "abstract_",
                        "score": 0.00390625
                    },
                    {
                        "name": "people_",
                        "score": 0.83984375,
                        "detail": {
                            "celebrities": [
                                {
                                    "name": "Satya Nadella",
                                    "faceBoundingBox": {
                                        "left": 597,
                                        "top": 162,
                                        "width": 248,
                                        "height": 248
                                    },
                                    "confidence": 0.999028444
                                }
                            ],
                            "landmarks": [
                                {
                                    "name": "Forbidden City",
                                    "confidence": 0.9978346
                                }
                            ]
                        }
                    }
                ],
                "adult": {
                    "isAdultContent": false,
                    "isRacyContent": false,
                    "adultScore": 0.0934349000453949,
                    "racyScore": 0.068613491952419281
                },
                "tags": [
                    {
                        "name": "person",
                        "confidence": 0.98979085683822632
                    },
                    {
                        "name": "man",
                        "confidence": 0.94493889808654785
                    },
                    {
                        "name": "outdoor",
                        "confidence": 0.938492476940155
                    },
                    {
                        "name": "window",
                        "confidence": 0.89513939619064331
                    }
                ],
                "description": {
                    "tags": [
                        "person",
                        "man",
                        "outdoor",
                        "window",
                        "glasses"
                    ],
                    "captions": [
                        {
                            "text": "Satya Nadella sitting on a bench",
                            "confidence": 0.48293603002174407
                        }
                    ]
                },
                "requestId": "0dbec5ad-a3d3-4f7e-96b4-dfd57efe967d",
                "metadata": {
                    "width": 1500,
                    "height": 1000,
                    "format": "Jpeg"
                },
                "faces": [
                    {
                        "age": 44,
                        "gender": "Male",
                        "faceBoundingBox": {
                            "left": 593,
                            "top": 160,
                            "width": 250,
                            "height": 250
                        }
                    }
                ],
                "color": {
                    "dominantColorForeground": "Brown",
                    "dominantColorBackground": "Brown",
                    "dominantColors": [
                        "Brown",
                        "Black"
                    ],
                    "accentColor": "873B59",
                    "isBwImg": false
                    },
                "imageType": {
                    "clipArtType": 0,
                    "lineDrawingType": 0
                }
            }
        }
    ]
}
```


## <a name="error-cases"></a>Hata durumları
Aşağıdaki hata durumlarda hiçbir öğe ayıklanır.

| Hata Kodu | Açıklama |
|------------|-------------|
| NotSupportedLanguage | Sağlanan dil desteklenmiyor. |
| InvalidImageUrl | Resim URL'si hatalı biçimlendirilmiş veya erişilebilir değil.|
| InvalidImageFormat | Giriş verileri geçerli bir görüntü değil. |
| InvalidImageSize | Girdi görüntüsünün çok büyük. |
| NotSupportedVisualFeature  | Belirtilen özellik türü geçerli değil. |
| NotSupportedImage | Desteklenmeyen görüntü, örneğin, çocuk pornografisi. |
| InvalidDetails | Desteklenmeyen özel etki alanı modeli. |

## <a name="see-also"></a>Ayrıca bkz.

+ [Önceden tanımlanmış beceriler](cognitive-search-predefined-skills.md)
+ [Bir beceri kümesi tanımlama](cognitive-search-defining-skillset.md)
+ [Dizin Oluşturucu (REST) oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-indexer)
