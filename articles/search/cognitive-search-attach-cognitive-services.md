---
title: Bir beceri kümesi - Azure Search ile Bilişsel hizmetler kaynağı ekleme
description: Azure Search'te bir bilişsel zenginleştirme ardışık düzenine bir Bilişsel hizmetler hepsi bir arada abonelik ekleme yönergeleri.
manager: cgronlun
author: LuisCabrer
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: 44f16b3334b991e071fa85ca4cffbc0837f0a6ec
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66244422"
---
# <a name="attach-a-cognitive-services-resource-with-a-skillset-in-azure-search"></a>Azure Search'te bir beceri kümesi ile bir Bilişsel hizmetler kaynağı ekleme 

Yapay ZEKA algoritmaları sürücü [bilişsel dizinleme işlem hatları](cognitive-search-concept-intro.md) Azure Search'te belge zenginleştirme için kullanılır. Bu algoritmalar dahil olmak üzere Azure Bilişsel hizmetler kaynakları dayalı [görüntü işleme](https://azure.microsoft.com/services/cognitive-services/computer-vision/) görüntü analizi ve optik karakter tanıma (OCR) ve [metin analizi](https://azure.microsoft.com/services/cognitive-services/text-analytics/) varlık tanıma anahtar ifade ayıklama ve diğer zenginleştirmelerinin. Belge zenginleştirme amacıyla Azure Search tarafından kullanılan algoritmalar içine sarmalanır ve bir *beceri*, yerleştirilmiş bir *beceri kümesi*ve başvurduğu bir *dizin oluşturucu* sırasında Dizin oluşturma.

Belgeler sınırlı sayıda ücretsiz zenginleştirebilirsiniz. Veya Faturalanabilir bir Bilişsel hizmetler kaynağa eklediğiniz bir *beceri kümesi* daha büyük ve daha sık iş yükleri için. Bu makalede, Azure arama sırasında belgeleri zenginleştirmek için Faturalanabilir bir Bilişsel hizmetler kaynağı ekleme öğreneceksiniz [dizin](search-what-is-an-index.md).

> [!NOTE]
> Faturalanabilir olayların Azure Search'te belge çözme aşamasının bir parçası olarak, Bilişsel hizmetler API'leri ve görüntü ayıklama çağrılarını içerir. Bilişsel hizmetler çağırmayın becerileri veya metin ayıklama belgelerden için ücret alınmaz.
>
> Faturalanabilir becerilerinden yürütmesi, adresindeki [Bilişsel hizmetler ödeme-olarak-, Git fiyat](https://azure.microsoft.com/pricing/details/cognitive-services/). Görüntü ayıklama fiyatlandırma için bkz [Azure fiyatlandırma sayfasını arama](https://go.microsoft.com/fwlink/?linkid=2042400).

## <a name="same-region-requirement"></a>Aynı bölgede gereksinimi

Azure Search ve Azure Bilişsel hizmetler aynı bölge içinde mevcut zorunlu kılarız. Aksi takdirde, çalışma zamanında bu iletiyi alırsınız: `"Provided key is not a valid CognitiveServices type key for the region of your search service."` 

Bir hizmet bölgeler arasında taşımak için hiçbir yolu yoktur. Bu hatayı alırsanız Azure Search ile aynı bölgede yeni bir Bilişsel hizmetler kaynağı oluşturmanız gerekir.

## <a name="use-free-resources"></a>Ücretsiz kaynakları kullan

Bilişsel arama öğretici ve hızlı başlangıç alıştırmalar tamamlamak için sınırlı, ücretsiz işleme seçeneği kullanabilirsiniz.

Ücretsiz (sınırlı zenginleştirmelerinin) kaynakları, abonelik başına günde 20 belgelere kısıtlanır.

1. Veri İçeri Aktarma sihirbazını açın:

   ![Veri İçeri Aktarma sihirbazını açın](media/search-get-started-portal/import-data-cmd2.png "veri içeri aktarma sihirbazını açın")

1. Veri kaynağı seçin ve devam **Ekle bilişsel arama (isteğe bağlı)** . Bu sihirbaz hakkında adım adım kılavuz için bkz: [içeri aktarma, dizin ve portal araçlarını kullanarak sorgu](search-get-started-portal.md).

1. Genişletin **ekleme Bilişsel Hizmetler** seçip **serbest (sınırlı zenginleştirmelerinin)** :

   ![Genişletilmiş ekleme Bilişsel hizmetler bölümüne](./media/cognitive-search-attach-cognitive-services/attach1.png "genişletilmiş Bilişsel Hizmetleri ekleme bölümü")

1. Sonraki adıma devam **ekleme Zenginleştirmelerinin**. Portalda mevcut becerileri açıklaması için bkz [2. adım: Bilişsel yetenekler Ekle](cognitive-search-quickstart-blob.md#create-the-enrichment-pipeline) bilişsel arama hızlı başlangıç.

## <a name="use-billable-resources"></a>Faturalanabilir kaynaklarını kullanma

Gün başına 20'den fazla zenginleştirmelerinin oluşturmak, iş yükleri için Faturalanabilir bir Bilişsel hizmetler kaynağı eklemek emin olun. Bilişsel hizmetler API'leri çağırmak istiyorsanız bile her zaman Faturalanabilir bir Bilişsel hizmetler kaynağı eklediğiniz öneririz. Kaynak ekleme, günlük limiti geçersiz kılar.

Bilişsel hizmetler API'leri çağıran yetenekleri için ücret ödersiniz. İçin faturalandırılırsınız değil [özel becerileri](cognitive-search-create-custom-skill-example.md), becerileri gibi veya [metin birleştirme](cognitive-search-skill-textmerger.md), [metin ayırıcısı](cognitive-search-skill-textsplit.md), ve [shaper](cognitive-search-skill-shaper.md), API tabanlı değildir.

1. Veri Alma Sihirbazı'nı açın, bir veri kaynağı seçin ve devam **Ekle bilişsel arama (isteğe bağlı)** .

1. Genişletin **ekleme Bilişsel Hizmetler** seçip **yeni Bilişsel hizmetler kaynağı oluşturma**. Yeni bir sekme açar, böylece kaynak oluşturabilirsiniz:

   ![Bilişsel hizmetler kaynağı oluşturma](./media/cognitive-search-attach-cognitive-services/cog-services-create.png "Bilişsel hizmetler kaynağı oluşturma")

1. İçinde **konumu** listesinde, Azure Search hizmetinizin bulunduğu bölgeyi seçin. Performansla ilgili nedenlerden dolayı bu bölgeyi kullandığınızdan emin olun. Bu bölge kullanarak da bölgeler arasında giden bant genişliği ücretleri da hükümsüz kılar.

1. İçinde **fiyatlandırma katmanı** listesinden **S0** Azure Search tarafından kullanılan önceden tanımlanmış beceriler geri görme ve dil özellikleri dahil olmak üzere, Bilişsel hizmetler özelliklerinin hepsi bir arada koleksiyonu almak için.

   S0 katmanı için hızları için belirli iş yükleri bulabileceğiniz [Cognitive Services fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/cognitive-services/).
  
   + İçinde **seçin teklif** listesinde, emin olun **Bilişsel Hizmetler** seçilir.
   + Altında **dil** özelliklerini fiyatlar **metin analizi standart** AI dizin oluşturma için geçerlidir.
   + Altında **işleme** özelliklerini fiyatlar **bilgisayar işleme S1** uygulayın.

1. Seçin **Oluştur** yeni Bilişsel hizmetler kaynağı sağlamak.

1. Veri Alma Sihirbazı'nı içeren bir önceki sekmeye dönün. Seçin **Yenile** Bilişsel hizmetler kaynağı gösterecek ve ardından kaynağı seçin:

   ![Bilişsel hizmetler kaynağı seçin](./media/cognitive-search-attach-cognitive-services/attach2.png "Bilişsel hizmetler kaynağı seçin")

1. Genişletin **ekleme Zenginleştirmelerinin** bölümü verileriniz üzerinde çalıştırmak istediğiniz belirli bilişsel yetenekleri seçin. Sihirbazı tamamlayın. Portalda mevcut becerileri açıklaması için bkz [2. adım: Bilişsel yetenekler Ekle](cognitive-search-quickstart-blob.md#create-the-enrichment-pipeline) bilişsel arama hızlı başlangıç.

## <a name="attach-an-existing-skillset-to-a-cognitive-services-resource"></a>Bilişsel hizmetler kaynağı için mevcut bir beceri kümesi ekleme

Mevcut bir beceri kümesi varsa, yeni veya farklı bir Bilişsel hizmetler kaynağı için ekleyebilirsiniz.

1. Üzerinde **hizmetine genel bakış** sayfasında **uzmanlık becerileri**:

   ![Uzmanlık becerileri sekmesini](./media/cognitive-search-attach-cognitive-services/attach-existing1.png "uzmanlık becerileri sekmesi")

1. Beceri kümesi adını seçin ve sonra mevcut bir kaynağı seçin veya yeni bir tane oluşturun. Seçin **Tamam** değişikliklerinizi onaylamak için.

   ![Beceri kümesi kaynak listesi](./media/cognitive-search-attach-cognitive-services/attach-existing2.png "beceri kümesi kaynak listesi")

   Unutmayın **serbest (sınırlı zenginleştirmelerinin)** seçenek sınırlar, 20 belgelere günlük ve kullanabileceğiniz **yeni Bilişsel hizmetler kaynağı oluşturma** yeni Faturalanabilen kaynak sağlamak için. Yeni bir kaynak oluşturursanız seçin **Yenile** Bilişsel hizmetler kaynakları listesini yenileyin ve ardından kaynak'ı seçin.

## <a name="attach-cognitive-services-programmatically"></a>Bilişsel hizmetler program aracılığıyla ekleme

Becerilerine program aracılığıyla tanımlarken ekleme bir `cognitiveServices` becerilerine bölümüne. Bu bölümde, Bilişsel hizmetler kaynağın beceri kümesi ile ilişkilendirmek istediğiniz anahtarı içerir. Kaynak Azure Search kaynağınız ile aynı bölgede olması gerektiğini unutmayın. De `@odata.type`ve `#Microsoft.Azure.Search.CognitiveServicesByKey`.

Aşağıdaki örnek, bu deseni gösterir. Bildirim `cognitiveServices` tanımının sonunda bölüm.

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

Bilişsel arama dizinleme ile ilişkili maliyetleri tahmin etmek için bazı sayılar çalıştırabilmeniz için ortalama bir belge benzer bir fikir ile başlayın. Örneğin, yaklaşık:

+ 1.000 PDF.
+ Altı her sayfa.
+ Bir görüntü her sayfada (6000 görüntüleri için).
+ Sayfa başına 3000 karakter.

Her PDF, resim ve metin ayıklama optik karakter tanıma (OCR) görüntüleri, belge çözme ve varlık tanıma, kuruluşların oluşan bir işlem hattı varsayılır.

Bu makalede gösterilen Fiyatlar, varsayımsal içindir. Bunlar, tahmin işlemi göstermek için kullanılırlar. Maliyetlerinizi daha düşük olabilir. İşlemler için gerçek fiyat, bakın [Bilişsel hizmetler fiyatlandırması](https://azure.microsoft.com/pricing/details/cognitive-services).

1. Metin ve resim içerikle çözme belge için metin ayıklama şu anda ücretsiz olarak kullanılabilir. 6000 görüntüler için ayıklanan her 1000 görüntü için 1 ABD Doları varsayılır. Bu adım için 6.00 $ maliyetini olmasıdır.

2. İngilizce 6000 görüntülerin OCR için en iyi algoritmayı (DescribeText) OCR bilişsel beceri kullanır. Analiz edilecek 1000 görüntü başına 2,50 maliyetini varsayıldığında, bu adım için $15,00 ödersiniz.

3. Varlık ayıklama için üç metin kayıtları sayfa başına toplam gerekir. Her bir kaydın 1.000 karakterdir. 6000 sayfaları tarafından çarpılan sayfa başına üç metin kayıtları 18, 000 metin kayıtları eşittir. 2,00 başına 1.000 metin kayıtları varsayılarak bu adımı $36.00 maliyeti.

Hepsi bir araya getirildiğinde, yaklaşık $ 1.000 PDF belgeleri açıklanan becerilerine olan bu tür alımı için 57.00 ödersiniz.

## <a name="next-steps"></a>Sonraki adımlar
+ [Azure Search'ü fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/search/)
+ [Bir beceri kümesi tanımlama](cognitive-search-defining-skillset.md)
+ [Beceri kümesi (REST) oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-skillset)
+ [Zenginleştirilmiş alanlarını eşleme](cognitive-search-output-field-mapping.md)
