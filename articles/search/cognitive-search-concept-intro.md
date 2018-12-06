---
title: Bilişsel arama veri ayıklama, Azure Search'te AI doğal dil işleme | Microsoft Docs
description: İçerik ayıklama, doğal dil işlemeyi (NLP) ve görüntü dizin bilişsel beceriler ve yapay ZEKA algoritmaları kullanarak Azure Search aranabilir içeriği oluşturmak için işleme
manager: cgronlun
author: HeidiSteen
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 08/07/2018
ms.author: heidist
ms.openlocfilehash: 5d7f275be1f04658f9901aba9faca83375a9bbf5
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52956202"
---
# <a name="what-is-cognitive-search"></a>Bilişsel arama nedir?

Bilişsel arama, bir dizini oluşturma ardışık düzeni için yapay ZEKA algoritmalarının ekleyerek aranabilir aranabilir olmayan içerik bilgilerini oluşturur. Yapay ZEKA tümleştirmedir aracılığıyla *bilişsel beceriler*, kaynak belgeleri arama dizini yoldaki zenginleştirilmesi. 

**Doğal dil işleme** yetenekleri dahil [varlık tanıma](cognitive-search-skill-named-entity-recognition.md), dil algılama [anahtar ifade ayıklama](cognitive-search-skill-keyphrases.md), metin işleme ve yaklaşım algılama. Bu becerilere sahip yapılandırılmamış metinleri, aranabilir ve filtrelenebilir alanlara bir dizini eşlenmiş yapılandırılmış olur.

**Görüntü işleme** içerir [OCR](cognitive-search-skill-ocr.md) tanımlama ve [görsel özellikleri](cognitive-search-skill-image-analysis.md), yorumu gibi yüz algılama, görüntü, görüntü tanıma (ünlü kişiler ve yer işareti) veya öznitelikler, renkleri veya görüntü yönü gibi. Resim içeriği, Azure Search'ün tüm sorgu özelliklerini kullanarak aranabilir metin temsilleridir oluşturabilirsiniz.

![Bilişsel arama işlem hattı diyagramı](./media/cognitive-search-intro/cogsearch-architecture.png "Bilişsel arama işlem hattı genel bakış")

Azure Search'te bilişsel beceriler Bilişsel hizmetler API'leri kullanılan aynı AI algoritmalarını dayanır: [adlandırılmış varlık tanıma API'si](cognitive-search-skill-named-entity-recognition.md), [anahtar tümcecik ayıklama API](cognitive-search-skill-keyphrases.md), ve [OCR API](cognitive-search-skill-ocr.md) birkaçı şunlardır. 

Doğal dil ile görüntü işleme uygulanır veri alımı aşaması sırasında Azure Search'te arama yapılabilir bir dizin bir belgenin birleşimde parçası olma sonuçlarla. Veri kaynağı bir Azure veri kümesi ve ardından hangisi kullanarak bir dizini oluşturma ardışık düzeni gönderilen [yerleşik yetenekler](cognitive-search-predefined-skills.md) ihtiyacınız. Yerleşik yetenek yeterli değilse oluşturabilir ekleme ve mimari Genişletilebilir olduğundan [özel becerileri](cognitive-search-create-custom-skill-example.md) özel işleme tümleştirmek için. Örnekler, Finans, bilimsel yayınlar veya ilaç gibi belirli bir etki alanı hedefleyen bir özel varlık modül veya belge sınıflandırıcı olabilir.

> [!NOTE]
> Bilişsel Arama, genel önizleme aşamasındadır. Şu anda becerileri yürütme ve görüntü ayıklama ve normalleştirme ücretsiz olarak sunulmaktadır. İlerleyen zamanlarda bu özelliklerin fiyatları duyurulacaktır. 

## <a name="components-of-cognitive-search"></a>Bilişsel arama bileşenleri

Bilişsel arama, bir önizleme özelliğidir [Azure Search](search-what-is-azure-search.md), tüm katmanlarda Güney Orta ABD ve Batı Avrupa'da kullanılabilir. 

Bilişsel arama işlem hattı dayanır [Azure Search *dizin oluşturucular* ](search-indexer-overview.md) gezinme veri kaynakları ve uçtan uca dizin işleme sağlar. Becerileri, artık kesintiye dizin oluşturucular için bağlı olan ve belgeleri becerilerine göre zenginleştirilmesi tanımlayın. Dizine sonra içerik arama istekleri aracılığıyla tüm erişebileceğiniz [sorgu türü Azure arama tarafından desteklenen](search-query-overview.md).  Dizin oluşturucular için yeni başladıysanız, bu bölümdeki adımlarda size yol gösterir.

### <a name="source-data-and-document-cracking-phase"></a>Veri kaynağı ve belge çözme aşaması

İşlem hattı başlangıcında, sahip olduğunuz yapılandırılmamış metin veya metin olmayan içerik (örneğin, görüntü ve taranan belgeleri JPEG dosyaları). Verileri bir Azure veri depolama hizmetindeki bir dizin oluşturucu tarafından erişilebilen mevcut olması gerekir. Dizin oluşturucular "kaynak belgelerini veri kaynağından metin ayıklamak için çözebilir".

![Aşama çözme belge](./media/cognitive-search-intro/document-cracking-phase-blowup.png "belge çözme")

 Azure blob depolama, Azure tablo depolama, Azure SQL veritabanı ve Azure Cosmos DB desteklenen kaynakları içerir. Metin tabanlı içeriğin aşağıdaki dosya türlerinden ayıklanan: PDF, Word, PowerPoint, CSV dosyaları. Tam liste için bkz. [desteklenen biçimler](search-howto-indexing-azure-blob-storage.md#supported-document-formats).

### <a name="cognitive-skills-and-enrichment-phase"></a>Bilişsel beceriler ve zenginleştirme aşaması

Zenginleştirme olduğunu adım *bilişsel beceriler* atomik işlemler gerçekleştirme. Örneğin, bir PDF gelen metin içeriğini aldıktan sonra varlık tanıma, dil algılama veya yerel olarak kaynak olarak mevcut olmayan yeni dizininize alanların üretmek için anahtar ifade ayıklama uygulayabilirsiniz. Tamamen becerileri, işlem hattında kullanılan koleksiyonu olarak da adlandırılır bir *beceri kümesi*.  

![Zenginleştirme aşaması](./media/cognitive-search-intro/enrichment-phase-blowup.png "zenginleştirme aşaması")

Bir beceri kümesi dayanır [bilişsel beceriler önceden tanımlanmış](cognitive-search-predefined-skills.md) veya [özel becerileri](cognitive-search-create-custom-skill-example.md) sağlayın ve beceri kümesi için bağlanın. Bir beceri kümesi en az veya çok karmaşık olabilir ve yalnızca işlem türü, aynı zamanda işlemlerin sırasını belirler. Bir beceri kümesi ayrıca bir Dizin Oluşturucu bölümünde zenginleştirme işlem hattı tam olarak belirten olarak tanımlanan alan eşlemeleri. Bu parçaların hepsini bir araya getirmek hakkında daha fazla bilgi için bkz. [bir beceri kümesi tanımlama](cognitive-search-defining-skillset.md).

Dahili olarak, işlem hattı bir zenginleştirilmiş bir belge koleksiyonu oluşturur. Hangi parçalarının zenginleştirilmiş belgeleri search dizininizi dizinlenebilir alanlarına eşlenmelidir karar verebilirsiniz. Örneğin, anahtar ifadeleri ayıklama ve varlık tanıma yetenekleri uyguladıysanız, ardından bu yeni alanlar zenginleştirilmiş belgeyi bir parçası olacak ve dizininizi alanlarında eşlenebilir. Bkz: [ek açıklamaları](cognitive-search-concept-annotations-syntax.md) giriş/çıkış formations hakkında daha fazla bilgi edinmek için.

### <a name="search-index-and-query-based-access"></a>Arama dizini ve sorgu tabanlı erişim

İşleme tamamlandığında, zenginleştirilmiş belgeler, metin-Azure Search'te arama yapılabilen oluşan bir arama topluluğunuza sahip. [Dizini sorgulama](search-query-overview.md) geliştiriciler ve kullanıcılar nasıl işlem hattı tarafından oluşturulan zenginleştirilmiş içeriğin eriştiği. 

![Arama simgesine diziniyle](./media/cognitive-search-intro/search-phase-blowup.png "arama simgesine sahip dizini")

Diğer Azure arama için oluşturacağınız gibi dizinidir: ek ile özel çözümleyiciler, belirsiz arama sorguları çağırmak, filtrelenen arama Ekle veya arama sonuçlarını şekillendirmek için Puanlama modelleri ile denemeler yapın.

Dizinler, alanların özniteliklerini tanımlayan bir dizin şemasını oluşturulur ve diğer yapıları Puanlama profilleri gibi belirli bir dizine eklenmiş ve eş anlamlı eşler. Dizin tanımlanan doldurulmuş sonra yeni ve güncelleştirilmiş kaynak belgeleri seçmek için artımlı olarak dizine ekleyebilir. Bazı değişiklikleri tam yeniden derleme gerektirir. Şema tasarımına tutarlı olana kadar küçük bir veri kümesi kullanmanız gerekir. Daha fazla bilgi için bkz. [Yeniden dizin derleme](search-howto-reindex.md).

<a name="feature-concepts"></a>

## <a name="key-features-and-concepts"></a>Önemli özellikler ve kavramlar

| Kavram | Açıklama| Bağlantılar |
|---------|------------|-------|
| Beceri kümesi | Bir üst düzey becerileri koleksiyonunu içeren kaynak adı. Bir beceri kümesi zenginleştirme işlem hattı ' dir. Bir dizin oluşturucu tarafından dizin oluşturma sırasında çağrılır. | [Bir beceri kümesi tanımlama](cognitive-search-defining-skillset.md) |
| Bilişsel beceri | Bir işlem hattındaki zenginleştirme atomik bir dönüştürme. Genellikle, bu ayıklar veya yapısı algılar ve bu nedenle Anlayışınızı giriş verileri çoğaltan bir bileşendir. Neredeyse her zaman, çıktı metin tabanlı ve işleme doğal dil işleme veya ayıklar ya da metin, görüntü girişler oluşturur, görüntü işleme. Bir beceri çıktısını bir alanda dizin eşlendi veya bir aşağı akış zenginleştirme için girdi olarak kullanılan. Bir beceri ya da önceden tanımlanmış ve Microsoft veya özel tarafından sağlanan: oluşturulur ve sizin tarafınızdan dağıtılır. | [Önceden tanımlanmış beceriler](cognitive-search-predefined-skills.md) |
| Veri ayıklama | Arka planda işleme ancak bilişsel arama, adlandırılmış varlık tanıma beceri ilişkin çeşitli en yaygın veri (varlık) bu bilgileri yerel olarak sağlamaz bir kaynaktan ayıklamak için kullanılır. | [Adlandırılmış varlık tanıma beceri](cognitive-search-skill-named-entity-recognition.md)| 
| Görüntü işleme | Bir görüntüden bir önemli yer tanıma özelliği gibi metin çıkarsar veya bir görüntüden metin ayıklar. Ortak örnekler OCR taranan belgeleri (JPEG) dosyasından karakterleri kaldırarak ya da bir sokak ad Cadde işareti içeren bir fotoğraf algılamayı içerir. | [Görüntü analizi beceri](cognitive-search-skill-image-analysis.md) veya [OCR beceri](cognitive-search-skill-ocr.md)
| Doğal dil işleme | Öngörü ve metin girişi hakkında bilgi işleme metin. Dil algılama ve yaklaşım analizi anahtar ifade ayıklama doğal dil işleme altında kalan becerileri var.  | [Anahtar tümcecik ayıklama beceri](cognitive-search-skill-keyphrases.md), [dil algılama beceri](cognitive-search-skill-language-detection.md), [yaklaşım analizi beceri](cognitive-search-skill-sentiment.md) |
| Belge çözme | Ayıklama veya dizin oluşturma sırasında metin olmayan kaynaklardan gelen metin içeriğini oluşturma işlemi. Optik karakter tanıma (OCR) bir örnek verilmiştir, ancak genellikle dizin oluşturucu uygulama dosyalarından içeriği ayıklar gibi için çekirdek dizin oluşturucu işlevi başvuruyor. Kaynak dosya konumu ve alan eşlemelerini sağlama dizin oluşturucu tanımı sağlayarak veri kaynağı çözme belgedeki her iki anahtar etkenler şunlardır. | Bkz: [dizin oluşturucular](search-indexer-overview.md) |
| Şekillendirme | Metin parçaları daha büyük bir yapısına birleştirmek veya tersine daha da aşağı akış işleme için yönetilebilir bir boyutu daha büyük metin parçalara ayırmanız. | [Shaper beceri](cognitive-search-skill-shaper.md), [metin birleşme beceri](cognitive-search-skill-textmerger.md), [beceri metni Böl](cognitive-search-skill-textsplit.md) |
| Zenginleştirilmiş belgeleri | Bir hatanın iç yapısı, kod içinde doğrudan erişilebilir değil. Zenginleştirilmiş belgeleri işleme sırasında oluşturulur ancak yalnızca son çıktı için bir arama dizinini kalıcıdır. Alan eşlemelerini hangi veri öğeleri dizine eklenen belirleyin. | Bkz: [zenginleştirilmiş eriştiğini](cognitive-search-tutorial-blob.md#access-enriched-document). |
| Dizinleyici |  Bir dış veri kaynağından aranabilir verileri ve meta verileri ayıklayan ve dizin ve belge çözme ve veri kaynağı arasındaki alan alan eşlemeleri göre bir dizini dolduran bir Gezgin. Bilişsel arama zenginleştirmelerinin için dizin oluşturucu bir beceri kümesi çağırır ve zenginleştirme çıkış dizini hedef alanlara ilişkilendirme alan eşlemeleri içerir. Dizin Oluşturucu tanımı tüm yönergeleri ve işlem hattı işlemleri için başvurular içerir ve dizin oluşturucu programını çalıştırdığınızda, işlem hattı çağrılır. | [Dizin Oluşturucular](search-indexer-overview.md) |
| Veri Kaynağı  | Azure'da desteklenen türlerin bir dış veri kaynağına bağlanmak için bir dizin oluşturucu tarafından kullanılan nesne. | Bkz: [dizin oluşturucular](search-indexer-overview.md) |
| Dizin oluşturma | Azure Search alan yapısı ve kullanım tanımlayan bir dizin şemasını yerleşik kalıcı arama topluluğunuza. | [Azure Search'te dizinler](search-what-is-an-index.md) | 


## <a name="where-do-i-start"></a>Nereden başlamalıyım?

**1. adım: bir arama hizmeti API'lerini sağlayan bir bölgede oluşturun.** 

+ Batı Orta ABD
+ Orta Güney ABD
+ Doğu ABD
+ Doğu ABD 2
+ Batı ABD 2
+ Orta Kanada
+ Batı Avrupa
+ Birleşik Krallık Güney
+ Kuzey Avrupa
+ Güney Brezilya
+ Güneydoğu Asya
+ Orta Hindistan
+ Avustralya Doğu

**2. adım: İş Akışı Yöneticisi için uygulamalı deneyim**

+ [Hızlı Başlangıç (portal)](cognitive-search-quickstart-blob.md)
+ [Öğretici (HTTP istek)](cognitive-search-tutorial-blob.md)
+ [Örnek özel becerileri (C#)](cognitive-search-create-custom-skill-example.md)

**3. adım: (yalnızca REST) API gözden geçirin.**

Şu anda yalnızca REST API'leri de sağlanır. Kullanım `api-version=2017-11-11-Preview` tüm isteklerde. Bilişsel arama çözümü oluşturmak için aşağıdaki API'leri kullanın. Yalnızca iki API eklendiğinde veya bilişsel arama için genişletilmiş. Diğer API'leri genel kullanıma sunulan sürümleri aynı söz dizimini sahip.

| REST API | Açıklama |
|-----|-------------|
| [Veri kaynağı oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-data-source)  | Zenginleştirilmiş belgeleri oluşturmak için kullanılan kaynak verileri sağlayan bir dış veri kaynağı tanımlayan bir kaynaktır.  |
| [Beceri kümesi oluşturma (API Sürüm 2017-11-11-Preview =)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)  | Bir kaynak kullanımını koordine [önceden tanımlanmış beceriler](cognitive-search-predefined-skills.md) ve [özel bilişsel beceriler](cognitive-search-custom-skill-interface.md) dizin oluşturma sırasında bir zenginleştirme hattında kullanılan. |
| [Dizin oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-index)  | Azure Search dizini ifade şema. Kaynak veri alanları veya alanları (örneğin, bir alan için kuruluş adlarını varlık tanıma tarafından oluşturulan) zenginleştirme aşaması sırasında üretilen dizin alanları eşleyin. |
| [Dizin Oluşturucu Oluşturma (API Sürüm 2017-11-11-Preview =)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)  | Bir kaynak dizin oluşturma sırasında kullanılan bileşenleri tanımlama: bir veri kaynağı, bir beceri kümesi, kaynak ve aracı veri yapılarını alan ilişkilendirme hedef dizin ve dizin de dahil olmak üzere. Dizin Oluşturucu veri alımı ve zenginleştirme tetikleyicisi çalışıyor. Çıkış bir arama topluluğunuza uzmanlık becerileri ile zenginleştirilmiş kaynak verilerle doldurulmuş dizin şemasını temel alınır.  |

**Denetim listesi: Normal bir iş akışı**

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
+ [Hızlı Başlangıç: portal Kılavuzu'nda bilişsel aramayı deneme](cognitive-search-quickstart-blob.md)
+ [Öğretici: bilişsel arama API'leri ile bilgi edinin.](cognitive-search-tutorial-blob.md)
