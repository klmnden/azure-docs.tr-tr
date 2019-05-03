---
title: Bir beceri kümesi - Azure Search ile Bilişsel hizmetler kaynağı ekleme
description: Azure Search'te bir bilişsel zenginleştirme ardışık düzenine bir Bilişsel hizmetler hepsi bir arada abonelik ekleme yönergeleri.
manager: cgronlun
author: LuisCabrer
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: bad64f439d45581f8f4b55ea1ac849db1e27cb76
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65024595"
---
# <a name="attach-a-cognitive-services-resource-with-a-skillset-in-azure-search"></a>Azure Search'te bir beceri kümesi ile bir Bilişsel hizmetler kaynağı ekleme 

Yapay ZEKA algoritmaları sürücü [bilişsel dizinleme işlem hatları](cognitive-search-concept-intro.md) Azure Search'te yapılandırılmamış verileri işlemek için kullanılır. Bu algoritmalar dayalı [Bilişsel hizmetler kaynakları](https://azure.microsoft.com/services/cognitive-services/)de dahil olmak üzere [görüntü işleme](https://azure.microsoft.com/services/cognitive-services/computer-vision/) görüntü analizi ve optik karakter tanıma (OCR) ve [metin analizi](https://azure.microsoft.com/services/cognitive-services/text-analytics/)varlık tanıma, anahtar ifade ayıklama ve diğer zenginleştirmelerinin.

Ücretsiz olarak sınırsız sayıda belgeyi zenginleştirebilir veya daha büyük ve daha sık iş yükleri için faturalandırılabilir Bilişsel Hizmetler kaynağı ekleyebilirsiniz. Bu makalede, Bilişsel hizmetler kaynağı bilişsel becerilerinizi sırasında veri zenginleştirme ile ilişkilendirilecek öğrenin [Azure arama dizini oluşturma](search-what-is-an-index.md).

Bilişsel hizmetler API'leri için ilgisi olmayan beceriler işlem hattınızı oluşuyorsa, Bilişsel hizmetler kaynağı hala iliştirin. Bu durumda geçersiz kılma işlemleri yapılması **ücretsiz** zenginleştirmelerinin günde küçük bir miktar için sınırlar kaynak. Bilişsel hizmetler API'leri için bağlı olmayan yetenekleri için ek ücret yoktur. Bu yetenekleri içerir: [özel becerileri](cognitive-search-create-custom-skill-example.md), [metin birleştirme](cognitive-search-skill-textmerger.md), [metin ayırıcısı](cognitive-search-skill-textsplit.md), ve [shaper](cognitive-search-skill-shaper.md).

> [!NOTE]
> İşlem, daha fazla belgelerin eklenmesi veya daha fazla yapay ZEKA algoritmalarının ekleme sıklığı artırarak kapsamı genişletin gibi Faturalanabilir bir Bilişsel hizmetler kaynağı eklemek gerekir. API'leri, Bilişsel hizmetler ve Azure Search'te belge çözme aşamasının bir parçası olarak görüntü ayıklama çağırırken ücretler tahakkuk. Metin ayıklama belgelerden için ücretlendirme yoktur.
>
> Yerleşik yetenek yürütülmesi sırasında mevcut ücretlendirilir [Bilişsel hizmetler ödeme-olarak-, Git fiyat](https://azure.microsoft.com/pricing/details/cognitive-services/). Görüntü ayıklama fiyatlandırma üzerinde açıklanmıştır [Azure fiyatlandırma sayfasını arama](https://go.microsoft.com/fwlink/?linkid=2042400).

## <a name="use-free-resources"></a>Ücretsiz kaynakları kullan

Bilişsel arama öğretici ve hızlı başlangıç alıştırmalar tamamlamak için sınırlı, ücretsiz işleme seçeneği kullanabilirsiniz. 

**(Sınırlı Zenginleştirmelerinin) ücretsiz** abonelik başına günde 20 belgelere kısıtlanmıştır.

1. Açık **verileri içeri aktarma** Sihirbazı.

   ![İçeri aktarma, veri komut](media/search-get-started-portal/import-data-cmd2.png "veri komut içeri aktarma")

1. Veri kaynağı seçin ve devam **bilişsel arama Ekle (isteğe bağlı)**. Bu sihirbaz hakkında adım adım kılavuz için bkz: [içeri aktarma, dizin ve portal araçlarını kullanarak sorgu](search-get-started-portal.md).

1. Genişletin **ekleme Bilişsel Hizmetler** seçip **serbest (sınırlı Zenginleştirmelerinin)**.

   ![Genişletilmiş ekleme Bilişsel hizmetler bölümüne](./media/cognitive-search-attach-cognitive-services/attach1.png "genişletilmiş Bilişsel hizmetler ekleme")

Sonraki adıma devam **zenginleştirmelerinin ekleme**. Portalda mevcut becerileri açıklaması için bkz ["2. adım: Bilişsel becerisi add"](cognitive-search-quickstart-blob.md#create-the-enrichment-pipeline) bilişsel arama hızlı başlangıç.

## <a name="use-billable-resources"></a>Faturalanabilir kaynaklarını kullanma

20'den fazla zenginleştirmelerinin günlük numaralandırma iş yükleri için Faturalanabilir bir Bilişsel hizmetler kaynağı eklemek gerekir. 

Bilişsel hizmetler API'leri çağırmak için yetenekler yalnızca ücretlendirilir. API tabanlı olmayan beceriler ister [özel becerileri](cognitive-search-create-custom-skill-example.md), [metin birleştirme](cognitive-search-skill-textmerger.md), [metin ayırıcısı](cognitive-search-skill-textsplit.md), ve [shaper](cognitive-search-skill-shaper.md) becerileri faturalandırılmaz.

1. Açık **verileri içeri aktarma** Sihirbazı, bir veri kaynağı seçin ve devam **bilişsel arama Ekle (isteğe bağlı)**. 

1. Genişletin **ekleme Bilişsel Hizmetler** seçip **yeni Bilişsel hizmetler kaynağı oluşturma**. Böylece kaynak oluşturabilir, yeni bir sekmede açılır. 

   ![Bilişsel hizmetler kaynağı oluşturma](./media/cognitive-search-attach-cognitive-services/cog-services-create.png "Bilişsel hizmetler kaynağı oluşturma")

1. Aynı bölgede Azure Search, bölgeler arasında giden bant genişliğini ücretlerden kaçınmak için konum seçin.

1. Fiyatlandırma katmanında seçin **S0** Azure Search tarafından kullanılan önceden tanımlanmış beceriler geri görme ve dil özellikleri dahil olmak üzere, Bilişsel hizmetler özelliklerinin hepsi bir arada koleksiyonu almak için. 

   S0 katmanı için hızları için belirli iş yükleri bulabileceğiniz [Cognitive Services fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/cognitive-services/).
  
   + İçinde **seçin teklif**, emin olun *Bilişsel Hizmetler* seçilir.
   + Dil özellikleri Fiyatlar altında *metin analizi standart* AI dizin oluşturma için geçerlidir. 
   + Görüntü Özellikleri Fiyatlar altında *bilgisayar işleme S1* uygulanır.

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
PUT https://[servicename].search.windows.net/skillsets/[skillset name]?api-version=2019-05-06
api-key: [admin key]
Content-Type: application/json
```
```json
{
    "name": "skillset name",
    "skills": 
    [
      {
        "@odata.type": "#Microsoft.Skills.Text.EntityRecognitionSkill",
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

Resim ve metin ayıklama, optik karakter tanıma (OCR) görüntüleri ile her PDF belgesi engellemelerine ve varlık tanıma, kuruluşların oluşan bir işlem hattı varsayılır. 

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
