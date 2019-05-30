---
title: Bilişsel arama, veri ayıklama, doğal dil yapay ZEKA işlem - Azure Search
description: İçerik ayıklama, doğal dil işlemeyi (NLP) ve görüntü dizin bilişsel beceriler ve yapay ZEKA algoritmaları kullanarak Azure Search aranabilir içeriği oluşturmak için işleme.
manager: cgronlun
author: HeidiSteen
services: search
ms.service: search
ms.devlang: NA
ms.topic: overview
ms.date: 05/28/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 8af927bee11d66c473707b603951fa693f6840e3
ms.sourcegitcommit: 8c49df11910a8ed8259f377217a9ffcd892ae0ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66299024"
---
# <a name="what-is-cognitive-search-in-azure-search"></a>"Bilişsel arama" Azure Search nedir?

Bilişsel arama, Azure Search, resimler, BLOB'ları ve diğer yapılandırılmamış veri kaynakları Azure Search dizini daha aranabilir yapmak için içerik zenginleştirilmesi - metin ayıklamak için kullanılan bir yapay ZEKA özelliğidir. Ayıklama ve zenginleştirme aracılığıyla uygulanır *bilişsel beceriler* bir dizinleme işlem hattına bağlı. Yapay ZEKA zenginleştirmelerinin aşağıdaki yollarla desteklenir: 

+ **Doğal dil işleme** yetenekleri dahil [varlık tanıma](cognitive-search-skill-entity-recognition.md), [dil algılama](cognitive-search-skill-language-detection.md), [anahtar ifade ayıklama](cognitive-search-skill-keyphrases.md), metin düzenleme ve [duyarlılık algılama](cognitive-search-skill-sentiment.md). Bu becerilere sahip yapılandırılmamış metin aranabilir ve filtrelenebilir alanlara bir dizin olarak eşlenmiş yeni biçimlerini kabul edilebilir.

+ **Görüntü işleme** yetenekleri dahil [optik karakter tanıma (OCR)](cognitive-search-skill-ocr.md) tanımlama ve [görsel özellikleri](cognitive-search-skill-image-analysis.md)yüz algılama, görüntü yorumu, resim tanıma (gibi ünlü kişiler ve yer işareti) veya renk veya görüntü yönü gibi öznitelikleri. Resim içeriği, Azure Search'ün tüm sorgu özelliklerini kullanarak aranabilir metin temsilleridir oluşturabilirsiniz.

![Bilişsel arama işlem hattı diyagramı](./media/cognitive-search-intro/cogsearch-architecture.png "Bilişsel arama işlem hattı genel bakış")

Azure Search'te bilişsel beceriler makine öğrenimi modelleri, Bilişsel hizmetler API'lerini temel alır: [Görüntü işleme](https://docs.microsoft.com/azure/cognitive-services/computer-vision/) ve [metin analizi](https://docs.microsoft.com/azure/cognitive-services/text-analytics/overview). 

Doğal dil ile görüntü işleme uygulanır veri alımı aşaması sırasında Azure Search'te arama yapılabilir bir dizin bir belgenin birleşimde parçası olma sonuçlarla. Veri kaynağı bir Azure veri kümesi ve ardından hangisi kullanarak bir dizini oluşturma ardışık düzeni gönderilen [yerleşik yetenekler](cognitive-search-predefined-skills.md) ihtiyacınız. Yerleşik yetenek yeterli değilse oluşturabilir ekleme ve mimari Genişletilebilir olduğundan [özel becerileri](cognitive-search-create-custom-skill-example.md) özel işleme tümleştirmek için. Örnekler, Finans, bilimsel yayınlar veya ilaç gibi belirli bir etki alanı hedefleyen bir özel varlık modül veya belge sınıflandırıcı olabilir.

> [!NOTE]
> Kapsam işleme sıklığını artırarak daha fazla belgelerin eklenmesi genişletmeniz veya daha fazla yapay ZEKA algoritmalarının eklenmesi gerekir [Faturalanabilir bir Bilişsel hizmetler kaynağı ekleme](cognitive-search-attach-cognitive-services.md). API'leri, Bilişsel hizmetler ve Azure Search'te belge çözme aşamasının bir parçası olarak görüntü ayıklama çağırırken ücretler tahakkuk. Metin ayıklama belgelerden için ücretlendirme yoktur.
>
> Yerleşik yetenek yürütülmesi sırasında mevcut ücretlendirilir [Bilişsel hizmetler ödeme-olarak-, Git fiyat](https://azure.microsoft.com/pricing/details/cognitive-services/). Görüntü ayıklama fiyatlandırma üzerinde açıklanmıştır [Azure fiyatlandırma sayfasını arama](https://go.microsoft.com/fwlink/?linkid=2042400).
## <a name="components-of-cognitive-search"></a>Bilişsel arama bileşenleri

Bilişsel arama işlem hattı dayanır [Azure Search *dizin oluşturucular* ](search-indexer-overview.md) gezinme veri kaynakları ve uçtan uca dizin işleme sağlar. Becerileri, artık kesintiye dizin oluşturucular için bağlı olan ve belgeleri becerilerine göre zenginleştirilmesi tanımlayın. Dizine sonra içerik arama istekleri aracılığıyla tüm erişebileceğiniz [sorgu türü Azure arama tarafından desteklenen](search-query-overview.md).  Dizin oluşturucular için yeni başladıysanız, bu bölümdeki adımlarda size yol gösterir.

### <a name="step-1-connection-and-document-cracking-phase"></a>1. adım: Bağlantı ve belge aşaması çözme

İşlem hattı başlangıcında, sahip olduğunuz yapılandırılmamış metin veya metin olmayan içerik (örneğin, görüntü ve taranan belgeleri JPEG dosyaları). Verileri bir Azure veri depolama hizmetindeki bir dizin oluşturucu tarafından erişilebilen mevcut olması gerekir. Dizin oluşturucular "kaynak belgelerini veri kaynağından metin ayıklamak için çözebilir".

![Aşama çözme belge](./media/cognitive-search-intro/document-cracking-phase-blowup.png "belge çözme")

 Azure blob depolama, Azure tablo depolama, Azure SQL veritabanı ve Azure Cosmos DB desteklenen kaynakları içerir. Aşağıdaki dosya türlerinden bir metin tabanlı içeriğin açılmasını: PDF, Word, PowerPoint, CSV dosyaları. Tam liste için bkz. [desteklenen biçimler](search-howto-indexing-azure-blob-storage.md#supported-document-formats).

### <a name="step-2-cognitive-skills-and-enrichment-phase"></a>2. adım: Bilişsel beceriler ve zenginleştirme aşaması

Zenginleştirme olduğunu adım *bilişsel beceriler* atomik işlemler gerçekleştirme. Örneğin, bir PDF gelen metin içeriğini aldıktan sonra varlık tanıma, dil algılama veya yerel olarak kaynak olarak mevcut olmayan yeni dizininize alanların üretmek için anahtar ifade ayıklama uygulayabilirsiniz. Tamamen becerileri, işlem hattında kullanılan koleksiyonu olarak da adlandırılır bir *beceri kümesi*.  

![Zenginleştirme aşaması](./media/cognitive-search-intro/enrichment-phase-blowup.png "zenginleştirme aşaması")

Bir beceri kümesi dayanır [bilişsel beceriler önceden tanımlanmış](cognitive-search-predefined-skills.md) veya [özel becerileri](cognitive-search-create-custom-skill-example.md) sağlayın ve beceri kümesi için bağlanın. Bir beceri kümesi en az veya çok karmaşık olabilir ve yalnızca işlem türü, aynı zamanda işlemlerin sırasını belirler. Bir beceri kümesi ayrıca bir Dizin Oluşturucu bölümünde zenginleştirme işlem hattı tam olarak belirten olarak tanımlanan alan eşlemeleri. Bu parçaların hepsini bir araya getirmek hakkında daha fazla bilgi için bkz. [bir beceri kümesi tanımlama](cognitive-search-defining-skillset.md).

Dahili olarak, işlem hattı bir zenginleştirilmiş bir belge koleksiyonu oluşturur. Hangi parçalarının zenginleştirilmiş belgeleri search dizininizi dizinlenebilir alanlarına eşlenmelidir karar verebilirsiniz. Örneğin, anahtar ifadeleri ayıklama ve varlık tanıma yetenekleri uyguladıysanız, ardından bu yeni alanlar zenginleştirilmiş belgeyi bir parçası olacak ve dizininizi alanlarında eşlenebilir. Bkz: [ek açıklamaları](cognitive-search-concept-annotations-syntax.md) giriş/çıkış formations hakkında daha fazla bilgi edinmek için.

#### <a name="add-a-knowledgestore-element-to-save-enrichments"></a>Zenginleştirmelerinin kaydetmek için bir knowledgeStore öğesi ekleyin

[Arama hizmeti REST API-version = 2019-05-06](search-api-preview.md) uzmanlık becerileri ile bir Azure depolama bağlantı ve zenginleştirmelerinin nasıl depolandığını anlatan tahminleri sağlayan bir knowledgeStore tanımı genişletir. 

Bir beceri kümesi için bir Bilgi Bankası depolama ekleme senaryoları dışında tam metin araması, zenginleştirmelerinin gösterimini proje olanağı sağlar. Daha fazla bilgi için [ne bilgi deposudur](knowledge-store-concept-intro.md).

### <a name="step-3-search-index-and-query-based-access"></a>3. adım: Arama dizini ve sorgu tabanlı erişim

İşleme tamamlandığında, zenginleştirilmiş belgeler, metin-Azure Search'te arama yapılabilen oluşan bir arama dizini sahip. [Dizini sorgulama](search-query-overview.md) geliştiriciler ve kullanıcılar nasıl işlem hattı tarafından oluşturulan zenginleştirilmiş içeriğin eriştiği. 

![Arama simgesine diziniyle](./media/cognitive-search-intro/search-phase-blowup.png "arama simgesine sahip dizini")

Diğer Azure arama için oluşturacağınız gibi dizinidir: ek ile özel çözümleyiciler, belirsiz arama sorguları çağırmak, filtrelenen arama Ekle veya arama sonuçlarını şekillendirmek için Puanlama modelleri ile denemeler yapın.

Dizinler, alanların özniteliklerini tanımlayan bir dizin şemasını oluşturulur ve diğer yapıları Puanlama profilleri gibi belirli bir dizine eklenmiş ve eş anlamlı eşler. Dizin tanımlanan doldurulmuş sonra yeni ve güncelleştirilmiş kaynak belgeleri seçmek için artımlı olarak dizine ekleyebilir. Bazı değişiklikleri tam yeniden derleme gerektirir. Şema tasarımına tutarlı olana kadar küçük bir veri kümesi kullanmanız gerekir. Daha fazla bilgi için bkz. [Yeniden dizin derleme](search-howto-reindex.md).

<a name="feature-concepts"></a>

## <a name="key-features-and-concepts"></a>Önemli özellikler ve kavramlar

| Kavram | Açıklama| Bağlantılar |
|---------|------------|-------|
| Beceri kümesi | Bir üst düzey becerileri koleksiyonunu içeren kaynak adı. Bir beceri kümesi zenginleştirme işlem hattı ' dir. Bir dizin oluşturucu tarafından dizin oluşturma sırasında çağrılır. | [Bir beceri kümesi tanımlama](cognitive-search-defining-skillset.md) |
| Bilişsel beceri | Bir işlem hattındaki zenginleştirme atomik bir dönüştürme. Genellikle, bu ayıklar veya yapısı algılar ve bu nedenle Anlayışınızı giriş verileri çoğaltan bir bileşendir. Neredeyse her zaman, çıktı metin tabanlı ve işleme doğal dil işleme veya ayıklar ya da metin, görüntü girişler oluşturur, görüntü işleme. Bir beceri çıktısını bir alanda dizin eşlendi veya bir aşağı akış zenginleştirme için girdi olarak kullanılan. Bir beceri ya da önceden tanımlanmış ve Microsoft veya özel tarafından sağlanan: oluşturulur ve sizin tarafınızdan dağıtılır. | [Önceden tanımlanmış beceriler](cognitive-search-predefined-skills.md) |
| Veri ayıklama | Arka planda işleme ancak bilişsel arama, varlık tanıma beceri ilişkin çeşitli en yaygın veri (varlık) bu bilgileri yerel olarak sağlamaz bir kaynaktan ayıklamak için kullanılır. | [Varlık tanıma beceri](cognitive-search-skill-entity-recognition.md)| 
| Görüntü işleme | Bir görüntüden bir önemli yer tanıma özelliği gibi metin çıkarsar veya bir görüntüden metin ayıklar. Ortak örnekler OCR taranan belgeleri (JPEG) dosyasından karakterleri kaldırarak ya da bir sokak ad Cadde işareti içeren bir fotoğraf algılamayı içerir. | [Görüntü analizi beceri](cognitive-search-skill-image-analysis.md) veya [OCR beceri](cognitive-search-skill-ocr.md)
| Doğal dil işleme | Öngörü ve metin girişi hakkında bilgi işleme metin. Dil algılama ve yaklaşım analizi anahtar ifade ayıklama doğal dil işleme altında kalan becerileri var.  | [Anahtar tümcecik ayıklama beceri](cognitive-search-skill-keyphrases.md), [dil algılama beceri](cognitive-search-skill-language-detection.md), [yaklaşım analizi beceri](cognitive-search-skill-sentiment.md) |
| Belge çözme | Ayıklama veya dizin oluşturma sırasında metin olmayan kaynaklardan gelen metin içeriğini oluşturma işlemi. Optik karakter tanıma (OCR) bir örnek verilmiştir, ancak genellikle dizin oluşturucu uygulama dosyalarından içeriği ayıklar gibi için çekirdek dizin oluşturucu işlevi başvuruyor. Kaynak dosya konumu ve alan eşlemelerini sağlama dizin oluşturucu tanımı sağlayarak veri kaynağı çözme belgedeki her iki anahtar etkenler şunlardır. | Bkz: [dizin oluşturucular](search-indexer-overview.md) |
| Şekillendirme | Metin parçaları daha büyük bir yapısına birleştirmek veya tersine daha da aşağı akış işleme için yönetilebilir bir boyutu daha büyük metin parçalara ayırmanız. | [Shaper beceri](cognitive-search-skill-shaper.md), [metin birleşme beceri](cognitive-search-skill-textmerger.md), [beceri metni Böl](cognitive-search-skill-textsplit.md) |
| Zenginleştirilmiş belgeleri | Bir arama dizinini yansıtılan son çıktı işleme sırasında oluşturulan geçici iç, bir yapıya sahiptir. Hangi zenginleştirmelerinin gerçekleştirilen bir beceri kümesi belirler. Alan eşlemelerini hangi veri öğeleri dizine eklenen belirleyin. İsteğe bağlı olarak, Depolama Gezgini, Power BI veya Azure Blob depolama alanına bağlanan herhangi bir aracı gibi araçları kullanarak zenginleştirilmiş belgeleri keşfedin ve kalıcı hale getirmek için bir Bilgi Bankası deposu oluşturabilirsiniz. | Bkz: [bilgi store (Önizleme)](knowledge-store-concept-intro.md). |
| Dizinleyici |  Bir dış veri kaynağından aranabilir verileri ve meta verileri ayıklayan ve dizin ve belge çözme ve veri kaynağı arasındaki alan alan eşlemeleri göre bir dizini dolduran bir Gezgin. Bilişsel arama zenginleştirmelerinin için dizin oluşturucu bir beceri kümesi çağırır ve zenginleştirme çıkış dizini hedef alanlara ilişkilendirme alan eşlemeleri içerir. Dizin Oluşturucu tanımı tüm yönergeleri ve işlem hattı işlemleri için başvurular içerir ve dizin oluşturucu programını çalıştırdığınızda, işlem hattı çağrılır. | [Dizin Oluşturucular](search-indexer-overview.md) |
| Veri Kaynağı  | Azure'da desteklenen türlerin bir dış veri kaynağına bağlanmak için bir dizin oluşturucu tarafından kullanılan nesne. | Bkz: [dizin oluşturucular](search-indexer-overview.md) |
| Dizin oluşturma | Azure Search, alan yapısı ve kullanım tanımlayan bir dizin şemasını yerleşik bir kalıcı arama dizin. | [Azure Search'te dizinler](search-what-is-an-index.md) | 

<a name="where-do-i-start"></a>

## <a name="where-do-i-start"></a>Nereden başlamalıyım?

**1. adım: [Bir Azure Search kaynağı oluşturun](search-create-service-portal.md)** 

**2. adım: Bazı hızlı başlangıç kılavuzlarımız ve örnekler için uygulamalı deneyim deneyin**

+ [Hızlı Başlangıç (portal)](cognitive-search-quickstart-blob.md)
+ [Öğretici (HTTP istek)](cognitive-search-tutorial-blob.md)
+ [Örnek özel becerileri (C#)](cognitive-search-create-custom-skill-example.md)

Öğrenme amacıyla ücretsiz hizmeti öneririz, ancak bu ücretsiz işlem sayısı günde 20 belgelere sınırlı olduğunu unutmayın. Hızlı Başlangıç ve öğretici bir gün içinde çalıştırmak için daha küçük bir dosya kullanın (10 belgeleri) her iki alıştırmalarda sığacak şekilde ayarlayın.

**3. adım: API gözden geçirin**

REST kullanarak `api-version=2019-05-06` istekleri veya .NET SDK'sı. 

Bu adım, bir bilişsel arama çözümü oluşturmak için REST API'lerini kullanır. Yalnızca iki API eklendiğinde veya bilişsel arama için genişletilmiş. Diğer API'leri genel kullanıma sunulan sürümleri aynı söz dizimini sahip.

| REST API | Açıklama |
|-----|-------------|
| [Veri Kaynağı Oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-data-source)  | Zenginleştirilmiş belgeleri oluşturmak için kullanılan kaynak verileri sağlayan bir dış veri kaynağı tanımlayan bir kaynaktır.  |
| [Beceri kümesi oluşturma (api sürümü 2019-05-06 =)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)  | Bir kaynak kullanımını koordine [önceden tanımlanmış beceriler](cognitive-search-predefined-skills.md) ve [özel bilişsel beceriler](cognitive-search-custom-skill-interface.md) dizin oluşturma sırasında bir zenginleştirme hattında kullanılan. |
| [Dizin oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-index)  | Azure Search dizini ifade şema. Kaynak veri alanları veya alanları (örneğin, bir alan için kuruluş adlarını varlık tanıma tarafından oluşturulan) zenginleştirme aşaması sırasında üretilen dizin alanları eşleyin. |
| [Create Indexer (api-version=2019-05-06)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)  | Bir kaynak dizin oluşturma sırasında kullanılan bileşenleri tanımlama: bir veri kaynağı, bir beceri kümesi, kaynak ve aracı veri yapılarını alan ilişkilendirme hedef dizin ve dizin de dahil olmak üzere. Dizin Oluşturucu veri alımı ve zenginleştirme tetikleyicisi çalışıyor. Çıktı, uzmanlık becerileri ile zenginleştirilmiş kaynak verilerle doldurulmuş dizin şemasını temel arama dizinidir.  |

**Denetim listesi: Tipik bir iş akışı**

1. Alt Azure kaynak verilerinizi temsili bir örnek. Gerçekleştirilen işlemlerin zaman dizin ile küçük, temsili bir veri kümesi başlatın ve çözümünüzü geliştikçe daha sonra bunları artımlı olarak oluşturmak.

1. Oluşturma bir [veri kaynağı nesnesi](https://docs.microsoft.com/rest/api/searchservice/create-data-source) Azure Search veri alma için bir bağlantı dizesini belirtin.

1. Oluşturma bir [beceri kümesi](https://docs.microsoft.com/rest/api/searchservice/create-skillset) zenginleştirme adımlarla.

1. Tanımlama [dizin şeması](https://docs.microsoft.com/rest/api/searchservice/create-index). *Alanları* koleksiyon kaynak veri alanlarını içerir. Ek alanları zenginleştirme sırasında oluşturulan içeriği için oluşturulan değerleri tutmak için saplama.

1. Tanımlama [dizin oluşturucu](https://docs.microsoft.com/rest/api/searchservice/create-skillset) veri kaynağı, beceri kümesi ve dizin başvuruyor.

1. Dizin oluşturucu içinde Ekle *outputFieldMappings*. Bu bölüm, giriş alanları (4. adım) dizin şemasında becerilerine (3 adımda) çıktısını eşler.

1. Gönderme *dizin oluşturucu oluşturma* oluşturduğunuz (Dizin Oluşturucu tanımını istek gövdesine sahip bir POST isteği) Azure Search dizin oluşturucu ifade etmek için istek. Bu adım, nasıl işlem hattı yürütmesini dizin oluşturucuyu Çalıştır ' dir.

1. Sonuçları değerlendirin ve kod güncelleştirme olanakları, şema veya dizin oluşturucu yapılandırmasını değiştirmek için sorgular çalıştırın.

1. [Dizin oluşturucuyu Sıfırla](search-howto-reindex.md) işlem hattını yeniden önce.

Belirli bir soru veya sorunlarınız hakkında daha fazla bilgi için bkz. [sorun giderme ipuçları](cognitive-search-concept-troubleshooting.md).

## <a name="next-steps"></a>Sonraki adımlar

+ [Bilişsel arama belgeleri](cognitive-search-resources-documentation.md)
+ [Hızlı Başlangıç: Portal Kılavuzu'nda bilişsel aramayı deneme](cognitive-search-quickstart-blob.md)
+ [Öğretici: Bilişsel arama API'leri öğrenin](cognitive-search-tutorial-blob.md)
+ [Bilgi Bankası deposuna genel bakış](knowledge-store-concept-intro.md)
+ [Bilgi Bankası store gözden geçirme](knowledge-store-howto.md)
