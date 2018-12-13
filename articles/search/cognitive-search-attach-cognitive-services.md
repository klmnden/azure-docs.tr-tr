---
title: Bilişsel hizmetler becerilerinizi için - Azure Search Ekle
description: Bilişsel hizmetler-bir abonelik için Bilişsel bir beceri kümesi ekleme yönergeleri
manager: cgronlun
author: HeidiSteen
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 12/05/2018
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: 7f604d1b94e51db11e6d6992b7f070d64ae869dd
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53317245"
---
# <a name="associate-cognitive-services-resource-with-a-skillset"></a>Bilişsel hizmetler kaynağı bir beceri kümesi ile ilişkilendirme 

> [!NOTE]
> 21 aralık 2018 tarihinden itibaren Bilişsel hizmetler kaynağı bir Azure Search beceri kümesi ile ilişkilendirmek mümkün olmayacak. Bu beceri yürütmesi için ücretlendirme başlatmak için bize izin verir. Bu tarihte, biz de belge çözme aşamasının bir parçası olarak görüntü ayıklama için başlayacağız. Belgelerden metin ayıklama işlemi ek masraf olmadan sağlanmaya devam edecektir.
>
> Var olan konumunda yerleşik yetenek yürütülmesini ücretlendirilir [Bilişsel hizmetler ödeme-olarak-, Git fiyat](https://azure.microsoft.com/pricing/details/cognitive-services/) . Görüntü ayıklama fiyatlandırma Önizleme fiyatıyla ücretlendirilirsiniz ve üzerinde açıklanmıştır [Azure fiyatlandırma sayfasını arama](https://go.microsoft.com/fwlink/?linkid=2042400).


Bilişsel arama ayıklar ve Azure Search aranabilir hale getirmek için veri zenginleştirir. Ayıklama ve zenginleştirme adımları diyoruz *bilişsel beceriler*. İçeriği dizin oluşturma sırasında adlı dizi tanımlanmış bir *beceri kümesi*. Bir beceri kümesi kullanabilirsiniz [önceden tanımlanmış beceriler](cognitive-search-predefined-skills.md) veya özel becerileri (bkz [örnek: özel bir yetenek oluşturmak](cognitive-search-create-custom-skill-example.md) daha fazla bilgi için).

Bu makalede, şunların nasıl ilişkilendirileceğini [Bilişsel Hizmetler ](https://azure.microsoft.com/services/cognitive-services/) bilişsel standartlarındaki şu kaynağa.

Seçtiğiniz Bilişsel hizmetler kaynağı yerleşik bilişsel beceriler güç. Bu kaynak, faturalandırma için de kullanılır. Yerleşik bilişsel beceriler kullanılarak gerçekleştirdiğiniz tüm zenginleştirmelerinin seçtiğiniz Bilişsel hizmetler kaynağı göre faturalandırılır. Bir Bilişsel hizmetler kaynağı kullanarak aynı görevi gerçekleştirilen gibi aynı oranda faturalandırılır. Bkz: [Bilişsel hizmet fiyatlandırma](https://azure.microsoft.com/pricing/details/cognitive-services/).

## <a name="limits-when-no-cognitive-services-resource-is-selected"></a>Bilişsel hizmetler kaynak yok seçildiğinde sınırları
1 Şubat 2019 ile becerilerinizi bir Bilişsel hizmetler abonelik ilişkilendirmezseniz başlayarak, yalnızca belgeleri ücretsiz (20 belgeleri için günde) az sayıda zenginleştirmek mümkün olmayacak. 

## <a name="associating-a-cognitive-services-resource-with-a-new-skillset"></a>Bilişsel hizmetler kaynağı yeni bir beceri kümesi ile ilişkilendirme

1. Bir parçası olarak *verileri içeri aktarma* deneyimi gitmek için veri kaynağına bağlandıktan sonra *Ekle bilişsel arama* isteğe bağlı bir adım. 

1. Genişletin *ekleme Bilişsel Hizmetler* bölümü. Bu, arama hizmetiniz ile aynı bölgedeki sahip herhangi bir Bilişsel hizmet kaynakları gösterir. 
![Genişletilmiş Bilişsel hizmet ekleme](./media/cognitive-search-attach-cognitive-services/attach1.png "genişletilmiş Bilişsel hizmetler ekleme")

1. Mevcut bir Bilişsel hizmetler kaynağı seçin veya *yeni Bilişsel hizmetler kaynağı oluşturma*. Seçerseniz *serbest (sınırlı Zenginleştirmelerinin) kaynak*, yalnızca belgeleri ücretsiz (20 belgeleri için günde) az sayıda zenginleştirmek mümkün olacaktır. Üzerinde tıkladıysanız *yeni Bilişsel hizmetler kaynağı oluşturma*, yeni bir sekmede açılır, Bilişsel hizmetler kaynağı oluşturmanıza olanak tanıyacak. 

1. Yeni bir kaynak oluşturduysanız *Yenile* Bilişsel hizmetler kaynakları listesini yenileyin ve kaynak'ı seçin. 
![Bilişsel Hizmet kaynağı seçili](./media/cognitive-search-attach-cognitive-services/attach2.png "Bilişsel hizmet kaynak seçildi")

1. Bunu yaptıktan sonra genişletebilirsiniz *ekleme Zenginleştirmelerinin* bölümü verileriniz üzerinde çalıştırın ve akışının geri kalanıyla devam etmek istediğiniz belirli bilişsel yetenekleri seçin.

## <a name="associating-a-cognitive-services-resource-with-an-existing-skillset"></a>Bilişsel hizmetler kaynağı var olan bir beceri kümesi ile ilişkilendirme

1. Hizmete genel bakış sayfasında *uzmanlık becerileri* sekmesi. ![Uzmanlık becerileri sekmesini](./media/cognitive-search-attach-cognitive-services/attach-existing1.png "uzmanlık becerileri sekmesi")

1. *Tıklayın* değiştirmek istediğiniz beceri kümesi üzerinde. Bu bir beceri kümesi düzenlemenize olanak sağlayacak bir dikey pencere açar.

1. Mevcut bir Bilişsel hizmetler kaynağı seçin veya *yeni Bilişsel hizmetler kaynağı oluşturma*. Seçerseniz *serbest (sınırlı Zenginleştirmelerinin) kaynak*, yalnızca belgeleri ücretsiz (20 belgeleri için günde) az sayıda zenginleştirmek mümkün olacaktır. Üzerinde tıkladıysanız *yeni Bilişsel hizmetler kaynağı oluşturma*, yeni bir sekmede açılır, Bilişsel hizmetler kaynağı oluşturmanıza olanak tanıyacak. <n/> 
<img src="./media/cognitive-search-attach-cognitive-services/attach-existing2.png" width="350">

1. Yeni bir kaynak oluşturduysanız *Yenile* Bilişsel hizmetler kaynakları listesini yenileyin ve kaynak'ı seçin.
1. Tıklayın *Tamam* değişikliklerinizi onaylamak için

## <a name="associating-a-cognitive-services-resource-programmatically"></a>Bilişsel hizmetler kaynağı program aracılığıyla ilişkilendirme

Becerilerine program aracılığıyla tanımlarken ekleme bir `cognitiveServices` bölümü. Bölüm anahtarı beceri kümesi ile ilişkilendirmek istediğiniz Bilişsel hizmetler kaynağı içermelidir yanı sıra @odata.type, ayarlanması "#Microsoft.Azure.Search.CognitiveServicesByKey için". Bu düzen aşağıdaki örnekte gösterilmektedir.

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
        "@odata.type": "#Microsoft.Azure.Search.CognitiveServicesByKey"
        "description": "mycogsvcs",
        "key": "your key goes here"
    }
}
```
## <a name="example-estimating-the-cost-of-document-cracking-and-enrichment"></a>Örnek: Belge çözme ve zenginleştirme maliyetini tahmin etme
Ne kadar verilen türde bir belge zenginleştirmek için maliyetlerini tahmin etmek isteyebilirsiniz. Aşağıdaki alıştırmada, yalnızca bir örnek olmakla birlikte size yardımcı olabilir.

1000 PDF sunuyoruz ve her biri bu belgeleri ortalama 6 her sayfa, tahmin hayal edin. Bunların her biri bu alıştırma için sayfa başına tek bir görüntü olduğunu varsayalım. Şimdi de, ortalama Imagine, sayfa başına yaklaşık 3000 karakterler vardır. 

Şimdi, zenginleştirme işlem hattının bir parçası aşağıdaki adımları gerçekleştirmek istediğimizi varsayalım:
1. Belge çözme işleminin bir parçası olarak içerik ve görüntüleri belgeden ayıklayın.
1. Parçası olarak zenginleştirme, OCR her sayfalarının ayıklanan, tüm sayfalar için metin birleştirmek ve birleşik metin kuruluşların tüm görüntülerin her ayıklayın.

Şimdi ne kadar bu belgeler, adım adım alma mal olacağını tahmin edin.

1000 belgeler için:

1. Belge çözme, biz 6000 görüntüleri birleştirilmiş bir dizi ayıklamak. $ $6.00 bize maliyeti 1 ayıklanan, her 1000 görüntü için kullanılır.

2. Biz her 6000 bu görüntülerin ayıklamak. İngilizce, OCR bilişsel beceri en iyi algoritmayı (DescribeText) kullanır. Analiz edilecek 1000 görüntü başına 2,50 maliyetini varsayıldığında, biz Bu adım için $15,00 ödersiniz.

3. Varlık ayıklama için biz 3 metin kayıtları (her kayıt, 1.000 karakterdir) ile ilgili sayfa başına toplam gerekir. 3 metin kayıtları/sayfası * 6000 sayfaları 18, 000 metin kayıtları =. Varsayılarak 2,00 $ / 1000 metin kayıtları, bu adımı maliyet bize $36.00.

Hepsi bir araya getirildiğinde, $ açıklanan becerilerine 1000 pdf belgelerinin bu yapısı alma 57.00 ödersiniz.  Bu alıştırmada biz en pahalı işlem fiyatına varsayıldı, fiyatlandırma Mezun nedeniyle daha düşük bırakılmış. Bkz: [Bilişsel hizmetler fiyatlandırması](https://azure.microsoft.com/pricing/details/cognitive-services).



## <a name="next-steps"></a>Sonraki adımlar
+ [Azure arama fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/search/)
+ [Bir beceri kümesi tanımlama](cognitive-search-defining-skillset.md)
+ [Beceri kümesi (REST) oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-skillset)
+ [Zenginleştirilmiş alanlarını eşleme](cognitive-search-output-field-mapping.md)
