---
title: Azure Search Hizmeti - Azure Search nedir
description: Azure arama, Microsoft gelen tam olarak yönetilen bir barındırılan bulut arama hizmetidir. Özellik açıklamaları, geliştirme iş akışı, diğer Microsoft arama ürünleri için Azure Search karşılaştırması ve kullanmaya nasıl başlayacağınızı gözden geçirin.
manager: cgronlun
author: HeidiSteen
services: search
ms.service: search
ms.topic: overview
ms.date: 05/02/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 7ba653795791f0e6a2c3d3c73ff574a3ed31f314
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67485228"
---
# <a name="what-is-azure-search"></a>Azure Search nedir?
Azure Search, geliştiricilere, web uygulamalarındaki, mobil uygulamalardaki ve kurumsal uygulamalardaki özel, heterojen içeriğe yönelik zengin arama deneyimi ekleme araçlarını ve API’lerini sunan, hizmet olarak arama bulut çözümüdür. Sorgu yürütme işlemi, kullanıcı tarafından tanımlanan bir dizine göre gerçekleştirilir.

+ Birden çok içerik türleri ve platformlar kaynaklanan yalnızca, verilerinizi içeren bir arama dizini oluşturun. 

+ Metin ve Özellikler görüntü dosyaları veya varlıkları ve ham metin anahtar ifadeleri ayıklamak için yapay ZEKA zenginleştirmelerinin yararlanın.

+ Sezgisel arama deneyimleri autocorrected arama terimleri "kastettiniz" modeli gezinme ve filtreleri, eş anlamlılar, otomatik tamamlama ve metin Analizi ile oluşturun. İşlevleri aracılığıyla ayarlama ve mantıksal artırma ilgi alın.

+ Belirli kullanım örnekleri için arama uygulamalar oluşturun. Coğrafi arama "Yakınımda Bul" deneyimini destekler. Çok dilli arama İngilizce olmayan tam metin araması için dil Çözümleyicileri aracılığıyla desteklenir.

Bilgi alma sürecinin karmaşıklığını maskeleyen basit bir [REST API’si](/rest/api/searchservice/) veya [.NET SDK’sı](search-howto-dotnet-sdk.md) aracılığıyla bu işlev sunulur. Azure portalı, API’lere ek olarak dizinlerinizin prototipini oluşturma ve dizinlerinizi sorgulama araçlarıyla birlikte yönetim ve içerik yönetimi desteği sağlar. Hizmet bulutta çalıştığından, altyapı ve kullanılabilirlik Microsoft tarafından yönetilir.

<a name="feature-drilldown"></a>

## <a name="feature-descriptions"></a>Özellik açıklamaları

| Core&nbsp;search&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  | Özellikler |
|-------------------|----------|
|Serbest biçimli metin arama | [**Tam metin araması** ](search-lucene-query-architecture.md) çoğu arama tabanlı uygulamalar için birincil kullanım örneği olan. Desteklenen bir söz dizimi kullanılarak sorgular formüle edilebilir. <br/><br/>[**Basit sorgu söz dizimi**](query-simple-syntax.md), mantıksal işleçler, tümcecik arama işleçleri, sonek işleçleri, öncelik işleçleri sağlar.<br/><br/>[**Lucene sorgu söz dizimi**](query-lucene-syntax.md), belirsiz arama, yakınlık araması, terimle yükseltme ve düzenli ifadeler için uzantılarla birlikte basit söz diziminde tüm işlemleri içerir.|
| İlgi düzeyi | [**Basit puanlama**](index-add-scoring-profiles.md), Azure Search’ün temel avantajıdır. Belgelerdeki değer işlevi olarak ilgi düzeyini modellemek için puanlama profilleri kullanılır. Örneğin, yeni ürünlerin veya indirimli ürünlerin arama sonuçlarında daha yukarıda görüntülenmesini isteyebilirsiniz. Ayrı olarak izleyip depoladığınız müşteri arama tercihlerine göre kişiselleştirilmiş puanlama için etiketleri kullanarak da puanlama profilleri derleyebilirsiniz. |
| Coğrafi arama | Azure Search, coğrafi konumları işler, filtreler ve görüntüler. Kullanıcıların, bir arama sonucunun fiziksel bir konuma göre yakınlığına göre verileri bulmasını sağlar. Daha fazla bilgi edinmek için [bu videoyu izleyin](https://channel9.msdn.com/Shows/Data-Exposed/Azure-Search-and-Geospatial-Data) veya [bu örneği gözden geçirin](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs). |
| Filtreler ve modeller | [**Modellenmiş gezinti**](search-faceted-navigation.md), tek bir sorgu parametresi aracılığıyla etkinleştirilir. Azure Search, kendinden yönlendirmeli filtreleme için (örneğin, fiyat aralığına veya markaya göre katalog öğelerini filtrelemek için) bir kategori listesinin ardında kod olarak kullanabileceğiniz çok yönlü bir gezinti yapısını döndürür. <br/><br/> [**Filtreler**](query-odata-filter-orderby-syntax.md), uygulamanın kullanıcı arabiriminde çok yönlü gezintiye yer vermek, sorgu oluşumunu geliştirmek ve kullanıcı veya geliştirici tarafından belirtilen ölçütlere göre filtreleme yapmak için kullanılabilir. OData söz dizimini kullanarak filtreler oluşturun. |
| Kullanıcı deneyimi özellikleri | [**Otomatik Tamamlama** ](search-autocomplete-tutorial.md) arama çubuğunda yazarken tamamlanan sorgular için etkinleştirilebilir. <br/><br/>[**Arama önerileri**](https://docs.microsoft.com/rest/api/searchservice/suggesters) de arama çubuğuna girilen kısmi metinler için kullanılabilir ancak sonuçlar sorgu terimi yerine dizininizdeki gerçek belgeler olur. <br/><br/>[**Eş anlamlılar**](search-synonyms.md), kullanıcının alternatif terim belirtmesine gerek kalmadan bir sorguyu kapsamını genişleten eşdeğer terimlerle ilişkilendirir. <br/><br/>[**İsabet vurgulama**](https://docs.microsoft.com/rest/api/searchservice/Search-Documents), arama sonuçlarında eşleşen bir anahtar sözcüğe metin biçimlendirmesi uygular. Hangi alanların vurgulanan kod parçacıklarını döndürdüğünü seçebilirsiniz.<br/><br/>Dizin şeması aracılığıyla birden fazla alan için [**Sıralama**](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) sunulur ve sonra tek bir arama parametresi ile sorgu zamanında açılıp kapatılır.<br/><br/> Azure Search’ün arama sonuçlarınız üzerinde sunduğu hassas kontrol sayesinde arama sonuçlarınızı [**disk belleğine almak**](search-pagination-page-layout.md) ve azaltmak çok kolaydır.  <br/><br/>|

| Yapay ZEKA&nbsp;zenginleştirme&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;       | Özellikler |
|-------------------|----------|
|Zenginleştirilmiş AI belgeleri | [**Bilişsel arama** ](cognitive-search-concept-intro.md) için resim ve metin analizi, metin bilgileri ham içeriği ayıklamak için bir dizini oluşturma ardışık düzeni için uygulanabilir. [Yerleşik yeteneklere](cognitive-search-predefined-skills.md) örnek olarak optik karakter tanıma (taranmış JPEG'leri aranabilir duruma getirme), varlık tanıma (bir kuruluşu, adı veya konumu tanıma) ve anahtar ifade tanıma verilebilir. İşlem hattına ekleme yapmak için [özel yetenek kodu da yazabilirsiniz](cognitive-search-create-custom-skill-example.md). |
| Analiz ve tüketimi için saklı zenginleştirmelerinin| [**Bilgi Bankası store (Önizleme)** ](knowledge-store-concept-intro.md) yapay ZEKA tabanlı dizinleme uzantısıdır. Bir arka uç olarak Azure depolama ile dizin oluşturma sırasında oluşturulan zenginleştirmelerinin kaydedebilirsiniz. Bu yapılar, daha iyi uzmanlık becerileri tasarlayın veya şekil oluşturup biçimsiz veya belirsiz verilerinizden yapısı yardımcı olması için kullanılabilir. Bu hedef belirli iş yükleri veya kullanıcıların bu yapıları projeksiyonlar oluşturabilirsiniz. Ayrıca doğrudan, ayıklanan verileri analiz etmek veya diğer uygulamalarda yük.<br/><br/> |

| Veri&nbsp;içeri aktarma ve dizin | Özellikler |
|----------------------------------|----------|
| Veri kaynakları | Azure Search dizinleri, JSON veri yapısı olarak gönderilmesi şartıyla her kaynaktan gelen verileri kabul eder. <br/><br/> [**Dizin oluşturucular** ](search-indexer-overview.md) desteklenen Azure veri kaynakları ve tanıtıcı JSON seri hale getirme veri alımı otomatikleştirin. Bağlanma [Azure SQL veritabanı](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md), [Azure Cosmos DB](search-howto-index-cosmosdb.md), veya [Azure Blob Depolama](search-howto-indexing-azure-blob-storage.md) birincil veri depolarında aranabilir içeriği ayıklamak için. Azure Blob dizin oluşturucuları, Microsoft Office, PDF ve HTML belgeleri de dahil, [başlıca dosya biçimlerinden metin ayıklamak](search-howto-indexing-azure-blob-storage.md) için *belge çözme* işlemini gerçekleştirebilir. |
| Hiyerarşik ve iç içe veri yapıları | [**Karmaşık türler** ](search-howto-complex-data-types.md) ve koleksiyonları neredeyse her türlü Azure Search dizini olarak JSON yapısı modeli izin verir. Bire çok ve çoka çok kardinalite koleksiyonlar, karmaşık türleri ve karmaşık türler yerel olarak ifade edilebilir.|
| Dil çözümleme | Çözümleyiciler, dizin oluşturma ve arama işlemleri sırasında metin işleme için kullanılan bileşenlerdir. İki tür vardır. <br/><br/>[**Özel sözcük temelli çözümleyiciler**](index-add-custom-analyzers.md), fonetik eşleştirme ve düzenli ifadeler kullanılarak yapılan karmaşık arama sorguları için kullanılır. <br/><br/>Lucene veya Microsoft’un [**dil çözümleyicileri**](index-add-language-analyzers.md), zaman kipleri, cinsiyet belirteçleri, düzensiz çoğul adlar (İngilizce’deki 'mouse' ve 'mice' gibi), sözcüğü bileşenlerine ayırma, sözcüklere bölme (boşluk içermeyen diller için) vb. gibi dile özgü linguistik durumları akıllıca işlemek için kullanılır. <br/><br/>|


| Platform&nbsp;düzeyi&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| Özellikler |
|-------------------|----------|
| Prototip oluşturma ve inceleme araçları | Portalda, dizin oluşturucuları yapılandırmak için [**Veri içeri aktarma sihirbazını**](search-import-data-portal.md), dizini öne çıkarmak için dizin tasarımcısını ve sorguları test edip puanlama profillerini daraltmak için [**Arama gezgini**](search-explorer.md)’ni kullanabilirsiniz. Şemasını görüntülemek için herhangi bir dizini de açabilirsiniz. |
| İzleme ve tanılama | [**İzleme özelliklerini etkinleştirme** ](search-monitor-usage.md) ölçümleri-a her zaman portalda görünür olan bakışta ötesine gidin. Ek bir yapılandırma gerekmeden saniye başına sorgu sayısı, gecikme süresi ve azaltma ölçümleri toplanıp portal sayfalarında raporlanır. <br/><br/>[**Arama trafiği analizi** ](search-traffic-analytics.md) başka bir izleme, sunucu tarafı burada alternatiftir ve istemci tarafı veriler toplanır ve hangi kullanıcıların arama kutusuna yazmaya hakkında Öngörüler analiz edilir. |
| Sunucu tarafı şifrelemesi | [**Microsoft tarafından yönetilen şifreleme bekleyen** ](search-security-overview.md#encrypted-transmission-and-storage) iç depolama katmanı oluşturulmuştur ve değiştirilemeyen. Varsayılan şifreleme ile isteğe bağlı olarak, ek [ **müşteri tarafından yönetilen bir şifreleme anahtarları (Önizleme)** ](search-security-manage-encryption-keys.md). Oluşturduğunuz ve Azure anahtar Kasası'nda yönetme anahtarlar dizinleri şifrelemek için kullanılır ve Azure Search'te eş anlamlı eşler. |
| Altyapı | **Yüksek oranda kullanılabilir platform**, son derece güvenilir arama hizmeti deneyimi sağlar. Düzgün şekilde ölçeklendirildiğinde [Azure Search, %99,9 SLA sunar](https://azure.microsoft.com/support/legal/sla/search/v1_0/).<br/><br/> Uçtan uca çözüm olarak **tam olarak yönetilen ve ölçeklendirilebilir** Azure Search kesinlikle bir altyapı yönetimi gerektirmez. Hizmetiniz daha fazla belge depolamayı, daha yüksek sorgu yüklerini veya her ikisini birden işlemek için iki boyutta ölçeklendirilerek ihtiyaçlarınıza göre uyarlanabilir.<br/><br/>|

## <a name="how-to-use-azure-search"></a>Azure Search’ü kullanma
### <a name="step-1-provision-service"></a>1\. adım: Sağlama hizmeti
[Azure portalında](https://portal.azure.com/) veya [Azure Resource Management API’si](/rest/api/searchmanagement/) aracılığıyla Azure Search hizmeti sağlayabilirsiniz. Diğer abonelerle paylaşılan ücretsiz hizmeti veya yalnızca hizmetiniz tarafından kullanılan kaynakları ayıran [ücretli katmanı](https://azure.microsoft.com/pricing/details/search/) seçebilirsiniz. Ücretli katmanlar için bir hizmeti iki boyutta ölçeklendirebilirsiniz: 

- Çoğaltmalar ekleyerek yoğun sorgu yüklerini işlemek için kapasitenizi büyütün.   
- Bölümler ekleyerek daha fazla belge için depolamayı büyütün. 

Belge depolamayı ve sorgu aktarım hızını ayrı olarak işleyerek üretim gereksinimlerine göre kaynak sağlamayı kalibre edebilirsiniz.

### <a name="step-2-create-index"></a>2\. adım: Dizin oluşturma
Aranabilir içeriği karşıya yükleyebilmeniz için önce bir Azure Search dizini tanımlamanız gerekir. Dizin, verilerinizi bulunduran ve arama sorgularını kabul edebilen bir veritabanı tablosuna benzer. Bir veritabanındaki alanlara benzer şekilde, aramak istediğiniz belgelerin yapısını yansıtmak için eşlenecek dizin şemasını tanımlarsınız.

Azure portalında veya [.NET SDK](search-howto-dotnet-sdk.md) ya da [REST API](/rest/api/searchservice/) kullanılarak programlama yoluyla bir şema oluşturulabilir.

### <a name="step-3-load-data"></a>3\. adım: Veri yükleme
Bir dizin tanımladıktan sonra içeriği karşıya yüklemeye hazır olursunuz. Bir itme veya çekme modeli kullanabilirsiniz.

Çekme modeli, dış veri kaynaklarından verileri alır. Verilere bağlanma, verileri okuma ve seri hale getirme gibi veri alımı işlemlerini kolaylaştıran ve otomatikleştiren *dizin oluşturucular* aracılığıyla desteklenir. [Dizin oluşturucular](/rest/api/searchservice/Indexer-operations), bir Azure sanal makinesinde barındırılan Azure Cosmos DB, Azure SQL Veritabanı, Azure Blob Depolama ve SQL Server için kullanılabilir. İsteğe bağlı veya zamanlanan veri yenileme için dizin oluşturucuyu yapılandırabilirsiniz.

Güncelleştirilmiş belgeleri dizine göndermek için kullanılan SDK veya REST API’leri aracılığıyla itme modeli sağlanır. JSON biçimini kullanarak hemen hemen her veri kümesinden verileri itebilirsiniz. Verileri yüklemeye ilişkin kılavuz için bkz. [Belgeler ekleme, güncelleştirme veya silme](/rest/api/searchservice/addupdate-or-delete-documents) veya [.NET SDK’sını kullanma](search-howto-dotnet-sdk.md).

### <a name="step-4-search"></a>4\. Adım: Ara
Bir dizin doldurulduktan sonra, REST API’si veya .NET SDK’sı ile basit HTTP isteklerini kullanarak hizmet uç noktanıza [arama sorguları düzenleyebilirsiniz](/rest/api/searchservice/Search-Documents).

## <a name="how-it-compares"></a>Karşılaştırma

Müşteriler genellikle Azure Search’ün diğer aramayla ilgili çözümlerle karşılaştırmasını öğrenmek ister. Aşağıdaki tabloda temel farklılıklar özetlenmiştir.

| Karşılaştırılan | Temel farklılıklar |
|-------------|-----------------|
|Bing | [Bing Web Araması API'si](https://docs.microsoft.com/azure/cognitive-services/bing-web-search/), Bing.com adresindeki dizinlerde gönderdiğiniz eşleşen terimleri arar. Dizinler, HTML, XML ve genel sitelerdeki diğer web içeriklerinden derlenir. Aynı temel üzerine oluşturulan [Bing Özel Arama](https://docs.microsoft.com/azure/cognitive-services/bing-custom-search/), tek tek web sitelerine kapsamı belirlenmiş şekilde web içeriği türleri için aynı gezgin teknolojisini sunar.<br/><br/>Azure Search, çoğu zaman çeşitli kaynaklardan sahip olduğunuz veri ve belgelerle doldurulmuş şekilde, tanımladığınız bir dizini arar. Azure Search, [dizin oluşturucular](search-indexer-overview.md) aracılığıyla bazı veri kaynakları için gezgin yeteneklerine sahiptir, ancak dizin şemanıza uygun herhangi bir JSON belgesini tek bir birleştirilmiş aranabilir kaynağa itebilirsiniz. |
|Veritabanı araması | Birçok veritabanı platformu yerleşik bir arama deneyimi içerir. SQL Server'ın [tam metin araması](https://docs.microsoft.com/sql/relational-databases/search/full-text-search) vardır. Cosmos DB ve benzeri teknolojilerin sorgulanabilir dizinleri vardır. Arama ile depolamayı birleştiren ürünler değerlendirilirken, hangisinin tercih edileceğini saptamak zor olabilir. Birçok çözüm, her ikisi de kullanın: DBMS depolama ve Azure arama için özel arama özellikleri.<br/><br/>DBMS Ara karşılaştırıldığında, Azure Search heterojen kaynaklardan içerikleri depolar ve özelleştirilmiş metin gibi (dallanma başsözcüğe, sözcük biçimlerini) işleme dil algılayan metin işleme özellikleri sunar [56 diller](https://docs.microsoft.com/rest/api/searchservice/language-support). Ayrıca yanlış yazılmış sözcükleri otomatik düzeltme, [eş anlamlılar](https://docs.microsoft.com/rest/api/searchservice/synonym-map-operations), [öneriler](https://docs.microsoft.com/rest/api/searchservice/suggestions), [puanlama denetimleri](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index), [modeller](https://docs.microsoft.com/azure/search/search-filters-facets) ve [özel belirteçlere ayırma](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search) özelliklerini de destekler. Azure Search’teki [tam metin araması motoru](search-lucene-query-architecture.md), bilgi alımında sektör standardı olan Apache Lucene’de yerleşiktir. Azure Search'te veriler tersine çevrilen dizin biçiminde kalıcı olur ama nadiren gerçek bir veri depolamanın yerini alır. Daha fazla bilgi için bu [forum gönderisine](https://stackoverflow.com/questions/40101159/can-azure-search-be-used-as-a-primary-database-for-some-data) bakın. <br/><br/>Kaynak kullanımı, bu kategorideki başka bir çekim noktasıdır. Dizin oluşturma ve bazı sorgu işlemleri çoğunlukla yoğun bilgi işlem içerir. Arama DBMS'den buluttaki ayrılmış bir çözüme aktarıldığında işlem yürütme için sistem kaynakları korunur. Üstelik, aramayı harici hale getirerek, sorgu hacmiyle eşleşecek şekilde kolayca ölçeği ayarlayabilirsiniz.|
|Ayrılmış arama çözümü | Tüm işlevleriyle birlikte ayrılmış arama kullanmaya karar verdiğinizi varsayarak, son kategorik karşılaştırma şirket içi çözümlerle bulut hizmetleri arasındadır. Birçok arama teknolojisi dizin oluşturma ve sorgu işlem hatları üzerinde kontrol, daha zengin sorgulama ve filtreleme söz dizimine erişim, derece ve ilgi düzeyi üzerinde kontrol ve kendinden yönlendirmeli ve akıllı arama özellikleri sunar. <br/><br/>Minimum ek yük ve Bakım ve teslim ile anahtar teslim bir çözüm istiyorsanız bir bulut hizmeti doğru seçimdir. <br/><br/>Bulut paradigması içinde birçok sağlayıcı, tam metin araması, coğrafi arama ve arama girişlerindeki belirli bir belirsizlik düzeyini işleme yeteneğiyle birlikte karşılaştırılabilir temel özellikler sunar. Genellikle bu [özel bir özellik olup](#feature-drilldown) en iyi uygunluğu belirleyen yönetimin, araçların ve API’lerin kolaylaştırılmasını sağlar. |

Bulut sağlayıcıları arasında Azure Search, Azure’daki içerik depoları ve veritabanları üzerinde tam metin arama iş yükleri için, öncelikli olarak hem bilgi alımı hem de içerik gezintisi için aramayı kullanan uygulamalar için en güçlü seçenektir. 

Temel güçlü yönleri şunlardır:

+ Dizin oluşturma katmanında Azure veri tümleştirmesi (gezginler)
+ Merkezi yönetim için Azure portalı
+ Azure ölçekleme, güvenilirlik ve birinci sınıf kullanılabilirlik
+ 56 dilde güçlü metin araması için çözümleyicilerle birlikte linguistik ve özel analiz
+ [Arama odaklı uygulamalarda ortak olan temel özellikler](#feature-drilldown): puanlama, modelleme, öneriler, eş anlamlılar, coğrafi arama ve daha fazlası.

> [!Note]
> Azure olmayan veri kaynakları tam olarak desteklenir, ancak dizin oluşturucular yerine daha kod kullanımı yoğun bir itme yöntemini kullanır. API’leri kullanarak herhangi bir JSON belge koleksiyonunun bir Azure Search dizinine kanalını oluşturabilirsiniz.

Müşterilerimiz arasında, Azure Search’teki en zengin özelliklerden yararlanabilenler, çevrimiçi kataloglar, iş kolu programları ve belge bulma uygulamalarıdır.

## <a name="rest-api--net-sdk"></a>REST API'Sİ | .NET SDK'SI

Portalda birçok görev gerçekleştirilebilse de Azure Search, mevcut uygulamalarda arama işlevselliğini tümleştirmek isteyen geliştiriciler için tasarlanmıştır. Aşağıdaki programlama arabirimleri kullanılabilir.

|Platform |Açıklama |
|-----|------------|
|[REST](/rest/api/searchservice/) | Xamarin, Java ve JavaScript gibi, programlama platformu ve dili tarafından desteklenen HTTP komutları|
|[.NET SDK](search-howto-dotnet-sdk.md) | REST API’si için .NET sarmalayıcı, C# dilinde ve .NET Framework’ü hedefleyen diğer yönetilen kod dillerinde verimli kodlama sunar |

## <a name="free-trial"></a>Ücretsiz deneme sürümü
Azure aboneleri, [Ücretsiz katmanda bir hizmet sağlayabilir](search-create-service-portal.md).

Abone değilseniz, [ücretsiz olarak bir Azure hesabı açabilirsiniz](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F). Ücretli Azure hizmetlerini denemek için krediler alırsınız. Krediler bittikten sonra hesabı tutabilir ve [ücretsiz Azure hizmetlerini](https://azure.microsoft.com/free/) kullanabilirsiniz. Açıkça ayarlarınızı değiştirip ücretlendirme istemediğiniz sürece kredi kartınız asla ücretlendirilmez.

Alternatif olarak, [MSDN abone Avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F): MSDN aboneliğiniz, ücretli Azure hizmetlerinizi kullanabildiğiniz her ay size kredi verir. 

## <a name="how-to-get-started"></a>Nasıl kullanmaya başlarım

1. [Ücretsiz hizmet](search-create-service-portal.md) oluşturun. Tüm hızlı başlangıçlar ve öğreticiler ücretsiz hizmetle tamamlanabilir.

2. [Dizinleme ve sorgulama için yerleşik araçları kullanma öğreticisindeki](search-get-started-portal.md) adımları izleyin. Önemli kavramları öğrenin ve portalın sağladığı bilgileri inceleyin.

3. .NET veya REST API'sini kullanarak kod yazmaya yönelin:

   + [.NET SDK’sını kullanma](search-howto-dotnet-sdk.md), yönetilen kodda ana iş akışını gösterir.  
   + [REST API'si ile çalışmaya başlama](https://github.com/Azure-Samples/search-rest-api-getting-started), REST API’sinin kullanımıyla aynı adımları gösterir. Bu hızlı başlangıçta, Postman veya Fiddler, REST API'leri çağırmak için de kullanabilirsiniz: [Azure Search REST API'lerini keşfetme](search-get-started-postman.md).

## <a name="watch-this-video"></a>Bu videoyu izleyin

Arama motorları, mobil uygulamalarda, web’de ve kurumsal veri depolarında bilgi alımını sağlayan genel tetikleyicilerdir. Azure Search size büyük ticari web sitelerindekine benzer bir arama deneyimi oluşturma araçları sunar.

Program yöneticisi Liam Cavanagh’ın bu 9 dakikalık videosunda, arama motorunu tümleştirmenin uygulamanıza nasıl fayda sağlayabileceğini öğrenebilirsiniz. Kısa deneme sürümlerinde, Azure Search’teki temel özellikler ve tipik bir iş akışının nasıl göründüğü ele alınır. 

>[!VIDEO https://channel9.msdn.com/Events/Connect/2016/138/player]
 
+ 0\.-3. dakikada, temel özellikler ve kullanım durumları ele alınmaktadır.
+ 3\.-4. dakikada, hizmet sağlama ele alınmaktadır. 
+ 4\.-6. dakikada, yerleşik emlak veri kümesini kullanarak bir dizin oluşturmak için kullanılan Veri İçeri Aktarma sihirbazı ele alınmaktadır.
+ 6\.-9. dakikada, Arama gezgini ve çeşitli sorgular ele alınmaktadır.
