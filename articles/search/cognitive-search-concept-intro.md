---
title: Veri ayıklama, doğal dil Azure Search'te işleme bilişsel Ara | Microsoft Docs
description: Veri ayıklama, doğal dil işleme (NLP) ve Azure Search'te bilişsel becerileri kullanarak dizin aranabilir içeriği oluşturmak için görüntü işleme.
manager: cgronlun
author: HeidiSteen
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 05/04/2018
ms.author: heidist
ms.openlocfilehash: ca6c285348208a7ad24faf966073d641810039fc
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34641120"
---
# <a name="what-is-cognitive-search"></a>Bilişsel arama nedir?

Bilişsel arama özelliğidir Önizleme [Azure Search](search-what-is-azure-search.md), Orta Güney ABD ve dizin oluşturma iş yükleri için AI ekleyen Batı Avrupa, tüm katmanlarda kullanılabilir. Veri ayıklama, doğal dil işleme ve dizin oluşturma sırasında görüntü işleme yapılandırılmamış veya yapılamayan içeriğinde görünmeyen bilgileri bulur ve Azure Search'te aranabilir hale getirir.

AI tümleştirme olduğu aracılığıyla *bilişsel becerileri* kaynak belgeleri sıralı işlemlerinde, arama dizini yolu üzerinden zenginleştirmek. 

![Bilişsel arama ardışık düzeni diyagramı](./media/cognitive-search-intro/cogsearch-architecture.png "Bilişsel arama ardışık genel bakış")

Dizin oluşturma sırasında kullanılan becerileri önceden tanımlanmış veya özel olabilir:

+ [Yetenekler prefined](cognitive-search-predefined-skills.md) Bilişsel hizmetler API'leri kullanılan AI algoritmalar dayanır: [adlı varlık tanıma](cognitive-search-skill-named-entity-recognition.md), [anahtar tümcecik ayıklama](cognitive-search-skill-keyphrases.md), ve [OCR](cognitive-search-skill-ocr.md) yalnızca birkaç var. 

+ [Özel becerileri](cognitive-search-create-custom-skill-example.md) ihtiyaç duyduğunuz özel işleme için sizin tarafınızdan geliştirilebilir. Özel becerileri örnekleri Finans, bilimsel yayınlar veya tıp gibi belirli bir etki alanı hedefleme özel varlık modül veya belge sınıflandırıcı olabilir.

> [!NOTE]
> Bilişsel Arama, genel önizlemededir ve beceri yürütmesi şu anda ücretsiz olarak sunulur. İlerleyen zamanlarda bu özelliğin fiyatlandırması duyurulacaktır.

## <a name="components-of-cognitive-search"></a>Bilişsel arama bileşenleri

Bilişsel arama ardışık düzen dayanır [Azure Search *dizin oluşturucular* ](search-indexer-overview.md) gezinme veri kaynakları ve uçtan uca dizin işleme sağlar. Yetenekler şimdi kesintiye uğratan dizin oluşturucular için bağlı olan ve belgeleri skillset göre zenginleştirmek tanımlayın. Dizine sonra içerik arama istekleri aracılığıyla tüm erişebilirsiniz [sorgu Azure araması tarafından desteklenen türü](search-query-overview.md).  Dizin oluşturucular için yeniyseniz, bu bölümde adımlarda size yol gösterir.

### <a name="source-data-and-document-cracking-phase"></a>Veri kaynağı ve çözme aşaması belge

Ardışık Düzen başlangıcında elinizde yapılandırılmamış metin veya metin dışı içeriği (örneğin, görüntü ve taranan belge JPEG dosyaları). Verileri bir dizin oluşturucu tarafından erişilebilen bir Azure veri depolama hizmeti mevcut olmalıdır. Dizin oluşturucular "metin veri kaynağından ayıklamak için kaynak belgeleri çözebilir".

![Aşama çözme belge](./media/cognitive-search-intro/document-cracking-phase-blowup.png "belge çözme")

 Desteklenen kaynaklardan Azure blob depolama, Azure table storage, Azure SQL Database ve Azure Cosmos DB içerir. Metin tabanlı içerik aşağıdaki dosya türlerinden ayıklanan: PDF, Word, PowerPoint, CSV dosyaları. Tam listesi için bkz: [desteklenen biçimler](search-howto-indexing-azure-blob-storage.md#supported-document-formats).

### <a name="cognitive-skills-and-enrichment-phase"></a>Bilişsel becerileri ve iyileştirmesini aşaması

İyileştirmesini olduğu aracılığıyla *bilişsel becerileri* atomik işlemlerini gerçekleştirme. Örneğin, bir PDF metin içeriği bulduktan sonra varlık tanıma dil algılama veya yerel kaynak kullanılabilir değil yeni alanlar dizininizdeki üretmek için anahtar tümcecik ayıklama uygulayabilirsiniz. Ardışık düzeninde kullanılan becerileri koleksiyonunu tamamen adlı bir *skillset*.  

![İyileştirmesini aşaması](./media/cognitive-search-intro/enrichment-phase-blowup.png "iyileştirmesini aşaması")

Bir skillset dayanır [bilişsel becerileri önceden tanımlanmış](cognitive-search-predefined-skills.md) veya [özel becerileri](cognitive-search-create-custom-skill-example.md) sağlamak ve skillset bağlanın. Bir skillset en düşük veya yüksek oranda karmaşık olabilir ve yalnızca işlem türü, aynı zamanda işlemlerin sırasını belirler. Bir skillset artı bir dizin oluşturucu parçası tam olarak iyileştirmesini ardışık düzen belirtir olarak tanımlanan alan eşlemelerini. Tüm bu bilgilerin bir araya getirmek hakkında daha fazla bilgi için bkz: [bir skillset tanımlamak](cognitive-search-defining-skillset.md).

Dahili olarak, ardışık düzen zenginleştirilmiş belge koleksiyonu oluşturur. Search dizininizi dizine alanlara zenginleştirilmiş belgeleri hangi kısımlarının eşlenmelidir karar verebilirsiniz. Örneğin, anahtar tümcecikleri ayıklama ve varlık tanıma becerileri uyguladıysanız, ardından yeni alanlarla zenginleştirilmiş belge parçası olacak ve dizininizi alanlarında eşlenebilir. Bkz: [ek açıklamaları](cognitive-search-concept-annotations-syntax.md) giriş/çıkış durum oluşumlarıyla hakkında daha fazla bilgi edinmek için.

### <a name="search-index-and-query-based-access"></a>Arama dizini ve sorgu tabanlı erişim

İşlem bittiğinde zenginleştirilmiş belgeleri, metin-Azure Search'te yapılabilen oluşan bir arama gövde sahip. [Dizini sorgulama](search-query-overview.md) geliştiriciler ve kullanıcılar ardışık düzen tarafından oluşturulan zenginleştirilmiş içeriğin nasıl eriştiği. 

![Dizin arama simgesine ile](./media/cognitive-search-intro/search-phase-blowup.png "arama simgesine sahip dizini")

Diğer Azure arama için oluşturacağınız gibi dizindir: ek ile özel çözümleyiciler, benzer arama sorguları çağırma, filtrelenmiş arama ekleme veya arama sonuçlarını yeniden şekillendirmek için profilleri Puanlama ile deneyin.

Dizin öznitelikleri, alanları tanımlayan bir dizin şemasını oluşturulur ve diğer yapıların profilleri Puanlama gibi belirli bir dizine bağlı ve eş eşler. Bir dizin tanımlı ve doldurulmuş sonra yeni ve güncelleştirilmiş kaynak belgeleri seçmek için artımlı olarak dizin oluşturabilirsiniz. Bazı değişiklikler tam yeniden gerektirir. Şema tasarımına tutarlı olana kadar küçük bir veri kümesi kullanmanız gerekir. Daha fazla bilgi için bkz. [Yeniden dizin derleme](search-howto-reindex.md).

<a name="feature-concepts"></a>

## <a name="key-features-and-concepts"></a>Önemli özellikler ve kavramlar

| Kavram | Açıklama| Bağlantılar |
|---------|------------|-------|
| Skillset | Bir üst düzey becerileri koleksiyonunu içeren kaynak adı. Bir skillset iyileştirmesini ardışık düzen ' dir. Bir dizin oluşturucu tarafından dizin oluşturma sırasında çağrılır. | [Bir skillset tanımlayın](cognitive-search-defining-skillset.md) |
| Bilişsel nitelik | Bir iyileştirmesini ardışık atomik bir dönüşüm. Genellikle, bu ayıklar veya yapısı oluşturur ve bu nedenle giriş verilerini anlama güçlendirir bir bileşendir. Neredeyse her zaman, çıktı metin tabanlı ve işleme doğal dil işleme veya ayıklar veya metin görüntü girişlerinde oluşturur görüntü işleme. Bir yetenek çıktısını bir dizin alanına eşlenen veya bir aşağı akış iyileştirmesini için girdi olarak kullanılır. Bir yetenek ya da önceden tanımlanmış ve Microsoft veya özel tarafından sağlanan: oluşturulan ve sizin tarafınızdan dağıtılabilir. | [Önceden tanımlanmış yetenekleri](cognitive-search-predefined-skills.md) |
| Veri ayıklama | Çok çeşitli işleme ancak bilişsel arama, adlandırılmış varlık tanıma yetenek ilgili çoğunlukla bu bilgileri yerel olarak sağlamaz bir kaynaktan verileri (bir varlık) çıkarmak için kullanılan kapsar. | [Adlandırılmış varlık tanıma nitelik](cognitive-search-skill-named-entity-recognition.md)| 
| Görüntü işleme | Bir yer işareti tanıması yeteneği gibi bir görüntüden metin oluşturur veya bir görüntüden metin ayıklar. Yaygın örnek OCR taranan belge (JPEG) dosyasından karakterleri kaldırma veya Sokak işareti içeren bir fotoğraf Sokak adlarında tanıma için verilebilir. | [Görüntü analiz yetenek](cognitive-search-skill-image-analysis.md) veya [OCR nitelik](cognitive-search-skill-ocr.md)
| Doğal dil işleme | Öngörüler ve metin girişleri hakkında bilgi için işleme metin. Dil algılama, düşünceleri analiz ve anahtar tümcecik ayıklama doğal dil işleme altında kalan becerileri verilebilir.  | [Anahtar tümcecik ayıklama yetenek](cognitive-search-skill-keyphrases.md), [dil algılama yetenek](cognitive-search-skill-language-detection.md), [düşünceleri analiz nitelik](cognitive-search-skill-sentiment.md) |
| Belge çözme | Ayıklama veya dizin oluşturma sırasında metin dışı kaynaklardan gelen metin içeriği oluşturma işlemi. Optik karakter tanıma örneğidir ancak genellikle dizin oluşturucu uygulama dosyalardan içerik ayıklayarak gibi çekirdek dizin oluşturucu işlevi başvuruyor. Her iki önemli faktör çözme belgedeki kaynak dosya konumu ve alan eşlemelerini sağlama dizin oluşturucu tanımı sağlayan veri kaynağı var. | Bkz: [dizin oluşturucular](search-indexer-overview.md) |
| Şekillendirme | Metin parçaları daha büyük bir yapının birleştirebilir veya tam tersi aşağı akış işleme daha kolay yönetilebilir bir boyuta büyük metin parçalara bölmek. | [Shaper yetenek](cognitive-search-skill-shaper.md), [metin birleşme yetenek](cognitive-search-skill-textmerger.md), [metnini yetenek bölme](cognitive-search-skill-textsplit.md) |
| Zenginleştirilmiş belgeleri | Bir geçici iç yapısı, kodda doğrudan erişilebilir değil. Zenginleştirilmiş belgeleri işleme sırasında oluşturulur, ancak yalnızca son çıkışları bir arama dizini kalıcı. Alan eşlemelerini hangi veri öğeleri dizine eklenecek belirler. | Bkz: [zenginleştirilmiş belgeleri erişme](cognitive-search-tutorial-blob.md#access-enriched-document). |
| Dizinleyici |  Bir dış veri kaynağından aranabilir verileri ve meta verileri ayıklar ve dizin ile veri kaynağınız belge çözme arasındaki alan alan eşlemelerini göre bir dizini dolduran bir Gezgin. Bilişsel arama enrichments için dizin oluşturucu bir skillset çağırır ve iyileştirmesini çıktı dizini hedef alanlara ilişkilendirme alan eşlemelerini içerir. Dizin Oluşturucu tanımı tüm yönergeleri ve ardışık düzen işlemlerinde başvurular içeriyor ve ardışık düzen dizin oluşturucu çalıştırıldığında çağrılır. | [Dizin Oluşturucular](search-indexer-overview.md) |
| Veri Kaynağı  | Azure üzerinde desteklenen türlerinin dış veri kaynağına bağlanmak için bir dizin oluşturucu tarafından kullanılan nesne. | Bkz: [dizin oluşturucular](search-indexer-overview.md) |
| Dizin oluşturma | Azure Search'te, alan yapısı ve kullanım tanımlayan bir dizin şemasını yerleşik kalıcı arama gövde. | [Azure Search'te dizin](search-what-is-an-index.md) | 


## <a name="where-do-i-start"></a>Nereden başlamalıyım?

**1. adım: API'ler sağlayan bir bölgede bir arama hizmeti oluşturma** 

+ Orta Güney ABD
+ Batı Avrupa

**2. adım: iş akışı ana için uygulamalı deneyim**

+ [Hızlı Başlangıç (portal)](cognitive-search-quickstart-blob.md)
+ [Öğretici (HTTP istek sayısı)](cognitive-search-tutorial-blob.md)
+ [Örnek özel becerileri (C#)](cognitive-search-create-custom-skill-example.md)

**3. adım: (yalnızca REST) API gözden geçirin**

Şu anda yalnızca REST API'leri sağlanır. Kullanım `api-version=2017-11-11-Preview` tüm isteklerinde. Bilişsel arama çözümü oluşturmak için aşağıdaki API'leri kullanın. Yalnızca iki API'leri eklenemez veya genişletilmiş bilişsel arayın. Diğer API'lara genel olarak kullanılabilir sürümleri ile aynı söz dizimini vardır.

| REST API | Açıklama |
|-----|-------------|
| [Veri kaynağı oluşturun](https://docs.microsoft.com/rest/api/searchservice/create-data-source)  | Zenginleştirilmiş belgeleri oluşturmak için kullanılan kaynak verileri sağlayan bir dış veri kaynağı tanımlayan bir kaynaktır.  |
| [Skillset oluşturun (API sürümü 2017-11-11-Önizleme =)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)  | Bir kaynak kullanımını Eşgüdümleme [becerileri önceden tanımlanmış](cognitive-search-predefined-skills.md) ve [özel bilişsel becerileri](cognitive-search-custom-skill-interface.md) iyileştirmesini ardışık düzeninde dizin oluşturma sırasında kullanılan. |
| [Dizin oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-index)  | Azure Search dizini ifade şema. Dizin alanları kaynak veri alanlarında veya alanları (örneğin, bir alan varlık tanıma tarafından oluşturulan kuruluş adları için) iyileştirmesini aşamasında üretilen eşleyin. |
| [Dizin Oluşturucu yapın (API sürümü 2017-11-11-Önizleme =)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)  | Bir kaynak dizin oluşturma sırasında kullanılan bileşenler tanımlama: bir veri kaynağı, bir skillset, hedef dizin kaynak ve ara veri yapılarını alanı ilişkilerini ve dizin dahil olmak üzere. Dizin Oluşturucu veri alımı ve iyileştirmesini için tetikleyici çalışıyor. Çıktı skillsets zenginleştirilmiş kaynak verilerle doldurulur dizin şemasını dayalı bir arama gövde şeklindedir.  |

**Denetim listesi: Normal bir iş akışı**

1. Alt Azure kaynak verilerinizi temsili bir örnek. Alır zaman dizin küçük, temsili bir veri kümesiyle başlatın ve çözümünüzü geliştikçe sonra onu artımlı olarak derleme.

1. Oluşturma bir [veri kaynağı nesnesi](https://docs.microsoft.com/rest/api/searchservice/create-data-source) Azure Search'te'nın veri alma için bir bağlantı dizesi girin.

1. Oluşturma bir [skillset](https://docs.microsoft.com/rest/api/searchservice/create-skillset) iyileştirmesini adımlara.

1. Tanımlamak [dizin şemasını](https://docs.microsoft.com/rest/api/searchservice/create-index). *Alanları* koleksiyon kaynak veri alanlarını içerir. İyileştirmesini sırasında oluşturulan içeriği için oluşturulan değerleri tutmak için ek alanlar çıkışı saplama.

1. Tanımlamak [dizin oluşturucu](https://docs.microsoft.com/rest/api/searchservice/create-skillset) veri kaynağı, skillset ve dizin başvuruyor.

1. Dizin oluşturucu içinde eklemek *outputFieldMappings*. Bu bölümde dizin şemasında (4. adım) giriş alanları (3. adımında) skillset çıktısını eşler.

1. Gönderme *oluşturma dizin oluşturucu* oluşturduğunuz (istek gövdesindeki bir dizin oluşturucu tanımıyla bir POST isteği) Azure Search'te dizin oluşturucu ifade etmek için istek. Bu adım nasıl ardışık çağırma dizin oluşturucusunu çalıştırmak bağlıdır.

1. Sonuçları değerlendirin ve güncelleştirme skillsets, şema veya dizin oluşturucu yapılandırma kodu değiştirmek için sorgular çalıştırın.

1. [Dizin Oluşturucu sıfırlama](search-howto-reindex.md) önce ardışık düzeni yeniden oluşturma.

Belirli sorular veya sorunlar hakkında daha fazla bilgi için bkz: [sorun giderme ipuçları](cognitive-search-concept-troubleshooting.md).

## <a name="next-steps"></a>Sonraki adımlar

+ [Bilişsel arama belgeleri](cognitive-search-resources-documentation.md)
+ [Hızlı Başlangıç: portal kılavuzda bilişsel aramayı deneyin](cognitive-search-quickstart-blob.md)
+ [Öğretici: API bilişsel arama öğrenin](cognitive-search-tutorial-blob.md)