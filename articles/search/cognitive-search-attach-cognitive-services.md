---
title: Bilişsel hizmetler kaynağı bir beceri kümesi ile - Azure Search ilişkilendirin
description: Azure Search'te bir bilişsel zenginleştirme ardışık düzenine bir Bilişsel hizmetler hepsi bir arada abonelik ekleme yönergeleri.
manager: cgronlun
author: LuisCabrer
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 01/07/2018
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: 315e050f9ff8768cfeb8ff5417834e8e163ce334
ms.sourcegitcommit: a512360b601ce3d6f0e842a146d37890381893fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2019
ms.locfileid: "54231436"
---
# <a name="associate-a-cognitive-services-resource-with-a-skillset-in-azure-search"></a>Azure Search'te bir beceri kümesi bir Bilişsel hizmetler kaynağı ilişkilendirmek 

Sürüş yapay ZEKA algoritmalarının [bilişsel arama işlem hatları](cognitive-search-concept-intro.md) yapılandırılmamış verileri işlemek için temel alan [ **Bilişsel hizmetler kaynakları**](https://azure.microsoft.com/services/cognitive-services/). Kaynakları ister [ **görüntü işleme** ](https://azure.microsoft.com/services/cognitive-services/computer-vision/) görüntü analizi ve optik karakter tanıma (OCR) metin ve görüntü dosyaları dışında yapısı ayıklanmasında sağlar ancak [ **metin Analytics** ](https://azure.microsoft.com/services/cognitive-services/text-analytics/) doğal dil işleme birkaç varlık tanıma ve anahtar ifade ayıklama gibi sağlar.

Belgeler sınırlı sayıda ücretsiz zenginleştirin ya da daha büyük ve daha sık iş yükleri için Faturalanabilir bir Bilişsel hizmetler kaynağı ekleyin. Bu makalede, Bilişsel hizmetler kaynağı bilişsel becerilerinizi dizin oluşturma sırasında veri zenginleştirme ile ilişkilendirmek nasıl öğrenin.

İşlem hattınızı özel oluşuyorsa [özel becerileri](cognitive-search-create-custom-skill-example.md), Bilişsel hizmetler kaynağı eklemek gerekmez.

> [!NOTE]
> 21 aralık 2018'den itibaren Bilişsel hizmetler kaynağı bir Azure Search beceri kümesi ile ilişkilendirebilirsiniz. Bu beceri yürütmesi için ücret olanak tanır. Bu tarihte, biz de belge çözme aşamasının bir parçası olarak görüntü ayıklama için ücretlendirme başladı. Metin ayıklama belgelerden hiçbir ek ücret ödemeden sunulmaya devam eder.
>
> Yürütülmesini [yerleşik bilişsel beceriler](cognitive-search-predefined-skills.md) ücretlendirilir [Bilişsel hizmetler ödeme-olarak-, Git fiyat](https://azure.microsoft.com/pricing/details/cognitive-services/), adresindeki Azure Search bağımsız olarak görev gerçekleştirilen gibi aynı oranı. Görüntü ayıklama fiyatlandırma Önizleme fiyatlandırması üzerinden ücretlendirilir ve üzerinde açıklanmıştır [Azure fiyatlandırma sayfasını arama](https://go.microsoft.com/fwlink/?linkid=2042400).


## <a name="use-free-resources"></a>Ücretsiz kaynakları kullan

Bilişsel arama öğretici ve hızlı başlangıç alıştırmalar tamamlamak için yeterli günlük aralıklarla 20 belge zenginleştirmelerinin için de yetki verir bir sınırlı, ücretsiz işleme seçeneği kullanabilirsiniz. 

> [!Important]
> 1 Şubat 2019 başlangıç **serbest (sınırlı Zenginleştirmelerinin)** günde 20 belgelere sınırlıdır. 

1. Açık **verileri içeri aktarma** Sihirbazı.

   ![İçeri aktarma, veri komut](media/search-get-started-portal/import-data-cmd2.png "veri komut içeri aktarma")

1. Veri kaynağı seçin ve devam **bilişsel arama Ekle (isteğe bağlı)**. Bu sihirbaz hakkında adım adım kılavuz için bkz: [içeri aktarma, dizin ve portal araçlarını kullanarak sorgu](search-get-started-portal.md).

1. Genişletin **ekleme Bilişsel Hizmetler** seçip **serbest (sınırlı Zenginleştirmelerinin)**.

   ![Genişletilmiş ekleme Bilişsel hizmetler bölümüne](./media/cognitive-search-attach-cognitive-services/attach1.png "genişletilmiş Bilişsel hizmetler ekleme")

Sonraki adıma devam **zenginleştirmelerinin ekleme**. Portalda mevcut becerileri açıklaması için bkz ["2. adım: Bilişsel becerisi add"](cognitive-search-quickstart-blob.md#create-the-enrichment-pipeline) bilişsel arama hızlı başlangıç.

## <a name="use-billable-resources"></a>Faturalanabilir kaynaklarını kullanma

20'den fazla belgeleri günlük numaralandırma iş yükleri için Faturalanabilir bir Bilişsel hizmetler kaynağı gerekir.

1. İçinde **verileri içeri aktarma** sihirbazını **ekleme Bilişsel Hizmetler**, mevcut bir kaynağı seçin veya **yeni Bilişsel hizmetler kaynağı oluşturma**.

1. İçin **yeni Bilişsel hizmetler kaynağı oluşturma**, böylece kaynak oluşturabilir, yeni bir sekme açılır. Kaynağın benzersiz bir ad verin.

1. Azure Search olarak aynı konumu seçin. Şu anda, bilişsel beceriler dizin oluşturma bu bölgelerde desteklenir:

  * Batı Orta ABD
  * Orta Güney ABD
  * Doğu ABD
  * Doğu ABD 2
  * Batı ABD 2
  * Orta Kanada
  * Batı Avrupa
  * Birleşik Krallık Güney
  * Kuzey Avrupa
  * Güney Brezilya
  * Güneydoğu Asya
  * Orta Hindistan
  * Avustralya Doğu

1. Hepsi bir arada fiyatlandırma katmanını seçin **S0**. Bu katman, önceden tanımlanmış beceriler bilişsel arama, yedekleme görme ve dil özellikleri sunar.

    ![Yeni Bilişsel hizmetler kaynağı oluşturma](./media/cognitive-search-attach-cognitive-services/cog-services-create.png "yeni Bilişsel hizmetler kaynağı oluşturma")

1. Tıklayın **Oluştur** yeni Bilişsel hizmetler kaynağı sağlamak. 

1. Önceki içeren sekmesi döndürmek **verileri içeri aktarma** Sihirbazı. Tıklayın **Yenile** Bilişsel hizmetler kaynağı Göster ve ardından kaynak'ı seçin.

   ![Bilişsel hizmetler kaynağı seçili](./media/cognitive-search-attach-cognitive-services/attach2.png "seçili Bilişsel Hizmet kaynağı")

1. Genişletin **ekleme Zenginleştirmelerinin** verileriniz üzerinde çalıştırmak istediğiniz belirli bilişsel beceriler seçmek için bölüm ve akışının geri kalanıyla devam edin. Portalda mevcut becerileri açıklaması için bkz ["2. adım: Bilişsel becerisi add"](cognitive-search-quickstart-blob.md#create-the-enrichment-pipeline) bilişsel arama hızlı başlangıç.

## <a name="attach-an-existing-skillset-to-a-cognitive-services-resource"></a>Bilişsel hizmetler kaynağı için mevcut bir beceri kümesi ekleme

Mevcut bir beceri kümesi varsa, yeni veya farklı bir Bilişsel hizmetler kaynağı için ekleyebilirsiniz.

1. Üzerinde **hizmetine genel bakış** sayfasında **uzmanlık becerileri**.

   ![Uzmanlık becerileri sekmesini](./media/cognitive-search-attach-cognitive-services/attach-existing1.png "uzmanlık becerileri sekmesi")

1. Beceri kümesi, adına tıklayın ve sonra mevcut bir kaynağı seçin veya yeni bir tane oluşturun. Tıklayın **Tamam** değişikliklerinizi onaylamak için. 

   ![Beceri kümesi kaynak listesi](./media/cognitive-search-attach-cognitive-services/attach-existing2.png "beceri kümesi kaynak listesi")

Sözcüğünün **serbest (sınırlı Zenginleştirmelerinin)** sınırlı günlük 20 belgelere ve **yeni Bilişsel hizmetler kaynağı oluşturma** yeni Faturalanabilen kaynak sağlamak için kullanılır. Yeni bir kaynak oluşturursanız seçin **Yenile** Bilişsel hizmetler kaynakları listesini yenileyin ve ardından kaynak'ı seçin.

## <a name="attach-cognitive-services-programmatically"></a>Bilişsel hizmetler program aracılığıyla ekleme

Becerilerine program aracılığıyla tanımlarken ekleme bir `cognitiveServices` becerilerine bölümüne. Bölümünde, Bilişsel hizmetler kaynağın beceri kümesi ile ilişkilendirmek istediğiniz anahtarı içerir. Kaynak Azure Search ile aynı bölgede olması gerektiğini unutmayın. De `@odata.type`ve `#Microsoft.Azure.Search.CognitiveServicesByKey`. 

Aşağıdaki örnek, bu deseni gösterir. CognitiveServices bölümü tanımı alt kısmındaki dikkat edin.

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
        "key": "<your key goes here>"
    }
}
```

## <a name="example-estimate-costs"></a>Örnek: Maliyet tahmini yapın

Bilişsel arama dizinleme ile ilişkili maliyetleri tahmin etmek için bazı sayılar çalıştırabilmeniz için ortalama bir belge benzer bir fikir ile başlayın. Örneğin, tahmin amacıyla, yaklaşık:

+ 1000 PDF'ler
+ Altı sayfaları
+ Sayfa (6000 görüntüleri) başına tek bir görüntü
+ Sayfa başına 3000 karakter

Resim ve metin ayıklama görüntüleri, optik karakter tanıma (OCR) ile her PDF çözme ve adlandırılmış varlık tanıma, kuruluşların belge oluşan bir işlem hattı varsayılır. 

Bu alıştırmada, işlem başına en pahalı fiyat kullanıyoruz. Gerçek maliyet kademeli fiyatlandırması nedeniyle daha düşük olabilir. Bkz: [Bilişsel hizmetler fiyatlandırması](https://azure.microsoft.com/pricing/details/cognitive-services).

1. Metin ve resim içerikle çözme belge için metin ayıklama şu anda ücretsiz olarak kullanılabilir. 6000 görüntüler için 6.00 Bu adım için maliyet ayıklanan, her 1000 görüntü için 1 ABD Doları varsayılır.

2. İngilizce 6000 görüntülerin OCR için en iyi algoritmayı (DescribeText) OCR bilişsel beceri kullanır. Analiz edilecek 1000 görüntü başına 2,50 maliyetini varsayıldığında, bu adım için $15,00 ödersiniz.

3. Varlık ayıklama için 3 metin kayıtları sayfa başına toplam gerekir. Her bir kaydın 1.000 karakterdir. Sayfa başına üç metin kayıtları * 6000 sayfaları 18, 000 metin kayıtları =. 2,00 başına 1.000 metin kayıtları varsayılarak bu adımı $36.00 maliyeti.

Hepsi bir araya getirildiğinde, yaklaşık $ bu yapısı açıklanan becerilerine sahip 1.000 PDF belgeleri almak için 57.00 ödersiniz. 

## <a name="next-steps"></a>Sonraki adımlar
+ [Azure Search'ü fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/search/)
+ [Bir beceri kümesi tanımlama](cognitive-search-defining-skillset.md)
+ [Beceri kümesi (REST) oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-skillset)
+ [Zenginleştirilmiş alanlarını eşleme](cognitive-search-output-field-mapping.md)
