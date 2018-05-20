---
title: Analiz bilişsel arama nitelik (Azure Search) görüntü | Microsoft Docs
description: Bir Azure Search iyileştirmesini ardışık düzeninde ImageAnalysis bilişsel yeteneği kullanarak görüntü analiz aracılığıyla anlamsal metin çıkarın.
services: search
manager: pablocas
author: luiscabrer
documentationcenter: ''
ms.assetid: ''
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: b426fd253b436c71235f006cc41881f0c0c67703
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
#   <a name="image-analysis-cognitive-skill"></a>Görüntü analiz bilişsel nitelik

**Görüntü analiz** yetenek zengin bir görüntü içeriğine göre görsel özellikleri ayıklar. Örneğin, bir resim yazısı görüntüden oluşturmak, etiketleri oluşturmak veya çok ünlüler ve bilinen yerler belirlemek.

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Vision.ImageAnalysisSkill 

## <a name="skill-parameters"></a>Yetenek parametreleri

Parametreleri büyük/küçük harfe duyarlıdır.

| Parametre adı     | Açıklama |
|--------------------|-------------|
| defaultLanguageCode   |  Döndürülecek dil gösteren bir dize. Hizmet, belirtilen bir dilde tanıma sonuçlarını döndürür. Bu parametre belirtilmezse, varsayılan değer "tr" dir. <br/><br/>Desteklenen diller şunlardır: <br/>*tr* -İngilizce (varsayılan) <br/> *zh* -Basitleştirilmiş Çince|
|visualFeatures |   Döndürülecek görsel özellik türleri belirten bir dizeler dizisi. Geçerli görsel özellik türleri şunlardır:  <ul><li> *Kategoriler* -görüntü içeriği Bilişsel hizmetler tanımlı bir sınıflandırma göre kategorilere ayıran [belgelerine](https://docs.microsoft.com/azure/cognitive-services/computer-vision/category-taxonomy).</li><li> *Etiketler* -ayrıntılı görüntü içerikle ilgili sözcüklerin listesini görüntüyle etiketler.</li><li>*Açıklama* -görüntü ile İngilizce tam bir cümle içeriği açıklar.</li><li>*Yazıtipleri* -yüzeyleri mevcut olup olmadığını algılar. Varsa, koordinatları, cinsiyetiniz ve yaş oluşturur.</li><li> *ImageType* -görüntü küçük resim veya çizgi çizme olup olmadığını algılar.</li><li>   *Renk* -Aksan rengi, baskın rengi belirler ve bir görüntü siyah olup beyaz.</li><li>*Yetişkin* -görüntü (çıplak görüntüler veya seks act gösterilmektedir) doğası gereği pornografik olup olmadığını algılar. Cinsel müstehcen içerik de algılandı.</li></ul> Görsel özellik adları büyük/küçük harfe duyarlıdır.|
| ayrıntılar   | Döndürülecek hangi etki alanına özgü ayrıntıları gösteren bir dizeler dizisi. Geçerli görsel özellik türleri şunlardır: <ul><li>*Çok ünlüler* -çok ünlüler görüntüde algıladıysa tanımlar.</li><li>*Yer işaretlerini* -görüntüde algıladıysa işaretleri tanımlar.</li></ul>
 |

## <a name="skill-inputs"></a>Yetenek girişleri

| Giriş adı      | Açıklama                                          |
|---------------|------------------------------------------------------|
| görüntü         | Karmaşık türü. "/ Belge/normalized_images" alan şu anda yalnızca çalışır Azure Blob Oluşturucu tarafından üretilen zaman ```imageAction``` ayarlanır ```generateNormalizedImages```. Bkz: [örnek](#sample-output) daha fazla bilgi için.|



##  <a name="sample-definition"></a>Örnek tanımı

```json
{
    "@odata.type": "#Microsoft.Skills.Vision.ImageAnalysisSkill",
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
                "url": "https://storagesample.blob.core.windows.net/sample-container/image.jpg"
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
                                    "faceRectangle": {
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
                    "faceRectangle": {
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
                    "isBWImg": false
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
Aşağıdaki hata durumlarda öğe ayıklanır.

| Hata Kodu | Açıklama |
|------------|-------------|
| NotSupportedLanguage | Sağlanan dili desteklenmiyor. |
| InvalidImageUrl | Resim URL'si hatalı biçimlendirilmiş veya erişilebilir değil.|
| InvalidImageFormat | Giriş verileri geçerli bir görüntü değil. |
| InvalidImageSize | Giriş resim çok büyük. |
| NotSupportedVisualFeature  | Belirtilen özellik türü geçerli değil. |
| NotSupportedImage | Desteklenmeyen bir görüntü, örneğin, çocuk pornografisi. |
| InvalidDetails | Desteklenmeyen etki alanına özgü modeli. |

## <a name="see-also"></a>Ayrıca bkz.

+ [Önceden tanımlanmış yetenekleri](cognitive-search-predefined-skills.md)
+ [Bir skillset tanımlama](cognitive-search-defining-skillset.md)
