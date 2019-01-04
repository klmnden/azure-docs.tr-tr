---
title: Azure Search'te bir beceri kümesi bir Bilişsel hizmetler kaynağı ilişkilendirmek
description: Azure Search'te bilişsel bir beceri kümesi bir Bilişsel hizmetler hepsi bir arada abonelik ekleme yönergeleri
manager: cgronlun
author: LuisCabrer
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 12/05/2018
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: 52efa685bba330879365f56e547881d62a52a185
ms.sourcegitcommit: da69285e86d23c471838b5242d4bdca512e73853
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2019
ms.locfileid: "54000164"
---
# <a name="associate-a-cognitive-services-resource-with-a-skillset-in-azure-search"></a>Azure Search'te bir beceri kümesi bir Bilişsel hizmetler kaynağı ilişkilendirmek 

Bilişsel arama ayıklar ve Azure Search aranabilir hale getirmek için veri zenginleştirir. Ayıklama ve zenginleştirme adımları diyoruz *bilişsel beceriler*. İçeriği dizin oluşturma sırasında adlı dizi tanımlanmış bir *beceri kümesi*. Bir beceri kümesi kullanabilirsiniz [önceden tanımlanmış beceriler](cognitive-search-predefined-skills.md) veya özel becerileri. Daha fazla bilgi için [örnek: özel bir yetenek oluşturmak](cognitive-search-create-custom-skill-example.md).

Bu makalede, şunların nasıl ilişkilendirileceğini bir [Azure Bilişsel Hizmetler ](https://azure.microsoft.com/services/cognitive-services/) bilişsel standartlarındaki şu olan kaynak.

Seçtiğiniz Bilişsel hizmetler kaynağı yerleşik bilişsel beceriler güç. Bu kaynak, faturalandırma için de kullanılır. Yerleşik bilişsel beceriler kullanarak gerçekleştirmek istediğiniz zenginleştirmelerinin seçtiğiniz Bilişsel hizmetler kaynağı göre faturalandırılır. Bilişsel hizmetler kaynağı kullanarak aynı görevi gerçekleştirilen gibi aynı oranda faturalandırılır. Bkz: [Bilişsel hizmet fiyatlandırma](https://azure.microsoft.com/pricing/details/cognitive-services/).

> [!NOTE]
> 21 aralık 2018'den itibaren Bilişsel hizmetler kaynağı bir Azure Search beceri kümesi ile ilişkilendirebilirsiniz. Bu beceri yürütmesi için ücret olanak tanır. Bu tarihte, biz de belge çözme aşamasının bir parçası olarak görüntü ayıklama için ücretlendirme başladı. Metin ayıklama belgelerden devam hiçbir ek ücret ödemeden sunulacak.
>
> Yerleşik yetenek yürütülmesini ücretlendirilir [Bilişsel hizmetler ödeme-olarak-, Git fiyat](https://azure.microsoft.com/pricing/details/cognitive-services/). Görüntü ayıklama fiyatlandırma Önizleme fiyatlandırması üzerinden ücretlendirilir ve üzerinde açıklanmıştır [Azure fiyatlandırma sayfasını arama](https://go.microsoft.com/fwlink/?linkid=2042400).

## <a name="limits-when-no-cognitive-services-resource-is-selected"></a>Bilişsel hizmetler kaynak yok seçildiğinde sınırları
Bilişsel hizmetler abonelik ile becerilerinizi ilişkilendirme, 1 Şubat 2019 başlayarak, belgeler için az sayıda (günde 20 belgeleri) ücretsiz yalnızca zenginleştirmek mümkün olacaktır. 

## <a name="associate-a-cognitive-services-resource-with-a-new-skillset"></a>Bilişsel hizmetler kaynağı yeni bir beceri kümesi ile ilişkilendirme

1. Bir parçası olarak [verileri içeri aktarma](search-import-data-portal.md) veri kaynağına bağlandıktan sonra sihirbaz, Git **Ekle bilişsel arama** isteğe bağlı bir adım. Bu sihirbazın ikinci adımıdır.

1. Genişletin **ekleme Bilişsel Hizmetler** bölümü. Bu adım aynı bölgede Azure Search hizmeti olarak sahip olduğunuz tüm Bilişsel hizmetler kaynakları gösterir.

   ![Genişletilmiş ekleme Bilişsel hizmetler bölümüne](./media/cognitive-search-attach-cognitive-services/attach1.png "genişletilmiş Bilişsel hizmetler ekleme")

1. Mevcut bir Bilişsel hizmetler kaynağı veya seçin, **yeni Bilişsel hizmetler kaynağı oluşturma**. Seçerseniz **serbest (sınırlı Zenginleştirmelerinin)**, belgeler için az sayıda (günde 20 belgeleri) ücretsiz yalnızca zenginleştirmek mümkün olacaktır. Seçerseniz **yeni Bilişsel hizmetler kaynağı oluşturma**, kaynak oluşturabileceğiniz yeni bir sekmede açılır. 

1. Yeni bir kaynak oluşturduysanız seçin **Yenile** Bilişsel hizmetler kaynakları listesini yenileyin ve ardından kaynak'ı seçin. 

   ![Bilişsel hizmetler kaynağı seçili](./media/cognitive-search-attach-cognitive-services/attach2.png "seçili Bilişsel Hizmet kaynağı")

1. Genişletin **ekleme Zenginleştirmelerinin** verileriniz üzerinde çalıştırmak istediğiniz belirli bilişsel beceriler seçmek için bölüm ve akışının geri kalanıyla devam edin.

## <a name="associate-a-cognitive-services-resource-with-an-existing-skillset"></a>Bilişsel hizmetler kaynağı var olan bir beceri kümesi ile ilişkilendirme

1. Üzerinde **hizmetine genel bakış** sayfasında **uzmanlık becerileri** sekmesi.

   ![Uzmanlık becerileri sekmesini](./media/cognitive-search-attach-cognitive-services/attach-existing1.png "uzmanlık becerileri sekmesi")

1. Değiştirmek istediğiniz beceri kümesi seçin. Bu adım, bir beceri kümesi düzenleyebileceğiniz bir dikey pencere açılır.

1. Mevcut bir Bilişsel hizmetler kaynağı veya seçin, **yeni Bilişsel hizmetler kaynağı oluşturma**. Seçerseniz **serbest (sınırlı Zenginleştirmelerinin)**, belgeler için az sayıda (günde 20 belgeleri) ücretsiz yalnızca zenginleştirmek mümkün olacaktır. Seçerseniz **yeni Bilişsel hizmetler kaynağı oluşturma**, kaynak oluşturabileceğiniz yeni bir sekmede açılır.

   <n/> 
   <img src="./media/cognitive-search-attach-cognitive-services/attach-existing2.png" width="350">

1. Yeni bir kaynak oluşturduysanız seçin **Yenile** Bilişsel hizmetler kaynakları listesini yenileyin ve ardından kaynak'ı seçin.

1. Seçin **Tamam** değişikliklerinizi onaylamak için.

## <a name="associate-a-cognitive-services-resource-programmatically"></a>Bilişsel hizmetler kaynağı program aracılığıyla ilişkilendirme

Becerilerine program aracılığıyla tanımlarken ekleme bir `cognitiveServices` bölümü. Bölümünde, Bilişsel hizmetler kaynağın beceri kümesi ile ilişkilendirmek istediğiniz anahtarı içerir. De `@odata.type`ve `#Microsoft.Azure.Search.CognitiveServicesByKey`. Aşağıdaki örnek, bu deseni gösterir.

```http
PUT https://[servicename].search.windows.net/skillsets/[skillset name]?api-version=2017-11-11-Preview
api-key: [admin key]
Content-Type: application/json
```
```json
{
    "name": "skillset name",
    "skills": 
    [
      {
        "@odata.type": "#Microsoft.Skills.Text.NamedEntityRecognitionSkill",
        "categories": [ "Organization" ],
        "defaultLanguageCode": "en",
        "inputs": [
          {
            "name": "text", "source": "/document/content"
          }
        ],
        "outputs": [
          {
            "name": "organizations", "targetName": "organizations"
          }
        ]
      }
    ],
    "cognitiveServices": {
        "@odata.type": "#Microsoft.Azure.Search.CognitiveServicesByKey",
        "description": "mycogsvcs",
        "key": "your key goes here"
    }
}
```
## <a name="example-estimate-the-cost-of-document-cracking-and-enrichment"></a>Örnek: Belge çözme ve zenginleştirme maliyetini tahmin etmek
Bu belge türünde zenginleştirmek için maliyetinin ne tahmini isteyebilirsiniz. Aşağıdaki alıştırmada, yalnızca bir örneği olmakla birlikte size yardımcı olabilir.

1.000 PDF olduğunu düşünün. Ortalama olarak, bu belgelerin her 6 sayfaları olduğunu tahmin edin. Her sayfada 1 görüntü vardır. Ortalama olarak yaklaşık 3000 karakter başına sayfa vardır. 

Şimdi, zenginleştirme işlem hattının bir parçası aşağıdaki adımları gerçekleştirmek istediğiniz varsayalım:
1. Belge çözme işleminin bir parçası olarak içerik ve görüntüleri belgeden ayıklayın.
1. Zenginleştirme bir parçası olarak her ayıklanan sayfaları için optik karakter tanıma (OCR) kullanın, tüm sayfalar için metin birleştirmek ve birleşik metin kuruluşların tüm görüntülerin her ayıklayın.

Şimdi ne kadar bu 1.000 belge, adım adım alma mal olacağını tahmin:

1. Belge çözme için 6000 görüntüleri birleştirilmiş bir dizi ayıklayın. $ $6.00, maliyeti 1 ayıklanan, her 1.000 görüntüleri için kullanılır.

2. Her 6000 bu görüntüleri metni ayıkla. İngilizce, OCR bilişsel beceri en iyi algoritmayı (DescribeText) kullanır. Analiz edilecek 1000 görüntü başına 2,50 maliyetini varsayıldığında, bu adım için $15,00 ödersiniz.

3. Varlık ayıklama için 3 metin kayıtları sayfa başına toplam gerekir. (Her kaydı 1000 karakter olabilir.) Sayfa başına üç metin kayıtları * 6000 sayfaları 18, 000 metin kayıtları =. 2,00 başına 1.000 metin kayıtları varsayılarak bu adımı $36.00 maliyeti.

Hepsi bir araya getirildiğinde, $ bu yapısı açıklanan becerilerine sahip 1.000 PDF belgeleri alma 57.00 ödersiniz. Bu alıştırmada biz en pahalı işlem fiyatına varsayılır. Bu fiyatlandırma Mezun nedeniyle daha düşük silinmiş. Bkz: [Bilişsel hizmetler fiyatlandırması](https://azure.microsoft.com/pricing/details/cognitive-services).



## <a name="next-steps"></a>Sonraki adımlar
+ [Azure Search'ü fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/search/)
+ [Bir beceri kümesi tanımlama](cognitive-search-defining-skillset.md)
+ [Beceri kümesi (REST) oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-skillset)
+ [Zenginleştirilmiş alanlarını eşleme](cognitive-search-output-field-mapping.md)
