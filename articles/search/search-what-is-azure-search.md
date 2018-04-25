---
title: Azure Search nedir? | Microsoft Docs
description: Azure Search, tam olarak yönetilen bir barındırılan bulut arama hizmetidir. Bu özellik genel bakışında daha fazla bilgi edinin.
manager: cgronlun
author: HeidiSteen
ms.service: search
ms.topic: overview
ms.date: 11/10/2017
ms.author: heidist
ms.openlocfilehash: df726e26b6a6f9ff28c1c38253875897b49b4155
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="what-is-azure-search"></a>Azure Search nedir?
Azure Search, geliştiricilere, web uygulamalarındaki, mobil uygulamalardaki ve kurumsal uygulamalardaki içeriğinize yönelik zengin arama deneyimi ekleme araçlarını ve API’lerini sunan, hizmet olarak arama bulut çözümüdür.

Bilgi alma sürecinin karmaşıklığını maskeleyen basit bir [REST API’si](/rest/api/searchservice/) veya [.NET SDK’sı](search-howto-dotnet-sdk.md) aracılığıyla bu işlev sunulur. Azure portalı, API’lere ek olarak dizinlerinizin prototipini oluşturma ve dizinlerinizi sorgulama araçlarıyla birlikte yönetim ve içerik yönetimi desteği sağlar. Hizmet bulutta çalıştığından, altyapı ve kullanılabilirlik Microsoft tarafından yönetilir.

<a name="feature-drilldown"></a>

## <a name="feature-summary"></a>Özellik özeti

| Kategori | Özellikler |
|----------|----------|
|Tam metin araması ve metin analizi | Çoğu arama tabanlı uygulamalar için öncelikli olarak [tam metin araması](search-lucene-query-architecture.md) kullanılır. Desteklenen bir söz dizimi kullanılarak sorgular formüle edilebilir. <br/><br/>[**Basit sorgu söz dizimi**](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search), mantıksal işleçler, tümcecik arama işleçleri, sonek işleçleri, öncelik işleçleri sağlar.<br/><br/>[**Lucene sorgu söz dizimi**](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search), belirsiz arama, yakınlık araması, terimle yükseltme ve düzenli ifadeler için uzantılarla birlikte basit söz diziminde tüm işlemleri içerir.| 
| Veri tümleştirmesi | Azure Search dizinleri, JSON veri yapısı olarak gönderilmesi şartıyla her kaynaktan gelen verileri kabul eder. <br/><br/> İsteğe bağlı olarak, Azure’da desteklenen veri kaynakları için [**dizin oluşturucuları**](search-indexer-overview.md) kullanarak [Azure SQL Veritabanı](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md), [Azure Cosmos DB](search-howto-index-cosmosdb.md) veya [Azure Blob depolama](search-howto-indexing-azure-blob-storage.md)’da otomatik olarak gezinebilir ve böylece arama dizininizin içeriğini birincil veri deponuzla eşitleyebilirsiniz. Azure Blob dizin oluşturucuları, Microsoft Office, PDF ve HTML belgeleri de dahil, [başlıca dosya biçimlerinin dizinini oluşturmak](search-howto-indexing-azure-blob-storage.md) için *belge çözme* işlemini gerçekleştirebilir. |
| Dil çözümleme | Çözümleyiciler, dizin oluşturma ve arama işlemleri sırasında metin işleme için kullanılan bileşenlerdir. İki tür vardır. <br/><br/>[**Özel sözcük temelli çözümleyiciler**](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search), fonetik eşleştirme ve düzenli ifadeler kullanılarak yapılan karmaşık arama sorguları için kullanılır. <br/><br/>Lucene veya Microsoft’un [**dil çözümleyicileri**](https://docs.microsoft.com/rest/api/searchservice/language-support), zaman kipleri, cinsiyet belirteçleri, düzensiz çoğul adlar (İngilizce’deki 'mouse' ve 'mice' gibi), sözcüğü bileşenlerine ayırma, sözcüklere bölme (boşluk içermeyen diller için) vb. gibi dile özgü linguistik durumları akıllıca işlemek için kullanılır. |
| Coğrafi arama | Azure Search, coğrafi konumları işler, filtreler ve görüntüler. Kullanıcıların, bir arama sonucunun fiziksel bir konuma göre yakınlığına göre verileri bulmasını sağlar. Daha fazla bilgi edinmek için [bu videoyu izleyin](https://channel9.msdn.com/Shows/Data-Exposed/Azure-Search-and-Geospatial-Data) veya [bu örneği gözden geçirin](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs). |
| Kullanıcı deneyimi özellikleri | Arama çubuğunda yazarken tamamlanan sorgular için [**arama önerileri**](https://docs.microsoft.com/rest/api/searchservice/suggesters) etkinleştirilebilir. Kullanıcılar kısmi arama girişleri yaptıkça dizininizdeki gerçek belgeler önerilir. <br/><br/>[**Modellenmiş gezinti**](https://docs.microsoft.com/azure/search/search-faceted-navigation), tek bir sorgu parametresi aracılığıyla etkinleştirilir. Azure Search, kendinden yönlendirmeli filtreleme için (örneğin, fiyat aralığına veya markaya göre katalog öğelerini filtrelemek için) bir kategori listesinin ardında kod olarak kullanabileceğiniz çok yönlü bir gezinti yapısını döndürür. <br/><br/> [**Filtreler**](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search), uygulamanın kullanıcı arabiriminde çok yönlü gezintiye yer vermek, sorgu oluşumunu geliştirmek ve kullanıcı veya geliştirici tarafından belirtilen ölçütlere göre filtreleme yapmak için kullanılabilir. OData söz dizimini kullanarak filtreler oluşturun.<br/><br/> [**İsabet vurgulama**](https://docs.microsoft.com/rest/api/searchservice/Search-Documents), arama sonuçlarında eşleşen bir anahtar sözcüğe metin biçimlendirmesi uygular. Hangi alanların vurgulanan kod parçacıklarını döndürdüğünü seçebilirsiniz.<br/><br/>Dizin şeması aracılığıyla birden fazla alan için [**Sıralama**](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) sunulur ve sonra tek bir arama parametresi ile sorgu zamanında açılıp kapatılır.<br/><br/> Azure Search’ün arama sonuçlarınız üzerinde sunduğu hassas kontrol sayesinde arama sonuçlarınızı [**disk belleğine almak**](search-pagination-page-layout.md) ve azaltmak çok kolaydır.  
| İlgi düzeyi | [**Basit puanlama**](/rest/api/searchservice/add-scoring-profiles-to-a-search-index), Azure Search’ün temel avantajıdır. Belgelerdeki değer işlevi olarak ilgi düzeyini modellemek için puanlama profilleri kullanılır. Örneğin, yeni ürünlerin veya indirimli ürünlerin arama sonuçlarında daha yukarıda görüntülenmesini isteyebilirsiniz. Ayrı olarak izleyip depoladığınız müşteri arama tercihlerine göre kişiselleştirilmiş puanlama için etiketleri kullanarak da puanlama profilleri derleyebilirsiniz. |
| İzleme ve raporlama | Kullanıcıların arama kutusuna yazdıklarından öngörüleri açığa çıkarmak için [**arama trafiği analizi**](search-traffic-analytics.md) toplanıp analiz edilir. <br/><br/>Ek bir yapılandırma gerekmeden saniye başına sorgu sayısı, gecikme süresi ve azaltma ölçümleri toplanıp portal sayfalarında raporlanır. Ayrıca gerektiği şekilde kapasiteye ayarlayabilmeniz için dizin ve belge sayılarını da kolayca izleyebilirsiniz. Daha fazla bilgi için bkz. [Hizmet yönetimi](search-manage.md) |
| Prototip oluşturma ve inceleme araçları | Portalda, dizin oluşturucuları yapılandırmak için [**Veri içeri aktarma sihirbazını**](search-import-data-portal.md), dizini öne çıkarmak için dizin tasarımcısını ve sorguları test edip puanlama profillerini daraltmak için [**Arama gezgini**](search-explorer.md)’ni kullanabilirsiniz. Şemasını görüntülemek için herhangi bir dizini de açabilirsiniz. |
| Altyapı | **Yüksek oranda kullanılabilir platform**, son derece güvenilir arama hizmeti deneyimi sağlar. Düzgün şekilde ölçeklendirildiğinde [Azure Search, %99,9 SLA sunar](https://azure.microsoft.com/support/legal/sla/search/v1_0/).<br/><br/> Uçtan uca çözüm olarak **tam olarak yönetilen ve ölçeklendirilebilir** Azure Search kesinlikle bir altyapı yönetimi gerektirmez. Hizmetiniz daha fazla belge depolamayı, daha yüksek sorgu yüklerini veya her ikisini birden işlemek için iki boyutta ölçeklendirilerek ihtiyaçlarınıza göre uyarlanabilir.

## <a name="how-to-use-azure-search"></a>Azure Search’ü kullanma
### <a name="step-1-provision-service"></a>1. Adım: Hizmeti sağlama
[Azure portalında](https://portal.azure.com/) veya [Azure Resource Management API’si](/rest/api/searchmanagement/) aracılığıyla Azure Search hizmeti sağlayabilirsiniz. Diğer abonelerle paylaşılan ücretsiz hizmeti veya yalnızca hizmetiniz tarafından kullanılan kaynakları ayıran [ücretli katmanı](https://azure.microsoft.com/pricing/details/search/) seçebilirsiniz. Ücretli katmanlar için bir hizmeti iki boyutta ölçeklendirebilirsiniz: 

- Çoğaltmalar ekleyerek yoğun sorgu yüklerini işlemek için kapasitenizi büyütün.   
- Bölümler ekleyerek daha fazla belge için depolamayı büyütün. 

Belge depolamayı ve sorgu aktarım hızını ayrı olarak işleyerek üretim gereksinimlerine göre kaynak sağlamayı kalibre edebilirsiniz.

### <a name="step-2-create-index"></a>2. Adım: Dizin oluşturma
Aranabilir içeriği karşıya yükleyebilmeniz için önce bir Azure Search dizini tanımlamanız gerekir. Dizin, verilerinizi bulunduran ve arama sorgularını kabul edebilen bir veritabanı tablosuna benzer. Bir veritabanındaki alanlara benzer şekilde, aramak istediğiniz belgelerin yapısını yansıtmak için eşlenecek dizin şemasını tanımlarsınız.

Azure portalında veya [.NET SDK](search-howto-dotnet-sdk.md) ya da [REST API](/rest/api/searchservice/) kullanılarak programlama yoluyla bir şema oluşturulabilir.

### <a name="step-3-index-data"></a>3. Adım: Verilerin dizinini oluşturma
Bir dizin tanımladıktan sonra içeriği karşıya yüklemeye hazır olursunuz. Bir itme veya çekme modeli kullanabilirsiniz.

Çekme modeli, dış veri kaynaklarından verileri alır. Verilere bağlanma, verileri okuma ve seri hale getirme gibi veri alımı işlemlerini kolaylaştıran ve otomatikleştiren *dizin oluşturucular* aracılığıyla desteklenir. [Dizin oluşturucular](/rest/api/searchservice/Indexer-operations), bir Azure sanal makinesinde barındırılan Azure Cosmos DB, Azure SQL Veritabanı, Azure Blob Depolama ve SQL Server için kullanılabilir. İsteğe bağlı veya zamanlanan veri yenileme için dizin oluşturucuyu yapılandırabilirsiniz.

Güncelleştirilmiş belgeleri dizine göndermek için kullanılan SDK veya REST API’leri aracılığıyla itme modeli sağlanır. JSON biçimini kullanarak hemen hemen her veri kümesinden verileri itebilirsiniz. Verileri yüklemeye ilişkin kılavuz için bkz. [Belgeler ekleme, güncelleştirme veya silme](/rest/api/searchservice/addupdate-or-delete-documents) veya [.NET SDK’sını kullanma](search-howto-dotnet-sdk.md).

### <a name="step-4-search"></a>4. Adım: Arama
Bir dizin doldurulduktan sonra, REST API’si veya .NET SDK’sı ile basit HTTP isteklerini kullanarak hizmet uç noktanıza [arama sorguları düzenleyebilirsiniz](/rest/api/searchservice/Search-Documents).

## <a name="how-azure-search-compares"></a>Azure Search karşılaştırması

Müşteriler genellikle Azure Search’ün diğer aramayla ilgili çözümlerle karşılaştırmasını öğrenmek ister. Aşağıdaki tabloda temel farklılıklar özetlenmiştir.

| Karşılaştırılan | Temel farklılıklar |
|--|--|
|Bing | [Bing Web Araması API'si](https://docs.microsoft.com/azure/cognitive-services/bing-web-search/), Bing.com adresindeki dizinlerde gönderdiğiniz eşleşen terimleri arar. Dizinler, HTML, XML ve genel sitelerdeki diğer web içeriklerinden derlenir. [Bing Özel Arama](https://docs.microsoft.com/azure/cognitive-services/bing-custom-search/), tek tek web sitelerine kapsamı belirlenmiş şekilde web içeriği türleri için aynı gezgin teknolojisini sunar.<br/><br/>Azure Search, çoğu zaman çeşitli kaynaklardan sahip olduğunuz veri ve belgelerle doldurulmuş şekilde, tanımladığınız bir dizini arar. Azure Search, [dizin oluşturucular](search-indexer-overview.md) aracılığıyla bazı veri kaynakları için gezgin yeteneklerine sahiptir, ancak dizin şemanıza uygun herhangi bir JSON belgesini tek bir birleştirilmiş aranabilir kaynağa itebilirsiniz. |
|Veritabanı araması | [SQL Server tam metin araması](https://docs.microsoft.com/sql/relational-databases/search/full-text-search), SQL tablolarında, DBMS’de yerleşik olan içerikler içindir. <br/><br/>Azure Search, heterojen kaynaklardan içerikleri depolar ve linguistik ve özel analiz gibi özel metin işleme özellikleri sunar. Azure Search’teki [tam metin araması motoru](search-lucene-query-architecture.md), bilgi alımında sektör standardı olan Apache Lucene’de yerleşiktir. <br/><br/>Kaynak kullanımı, başka bir çekim noktasıdır. Doğal dil araması çoğu zaman işlem açısından yoğundur. Arama, ayrılmış bir çözüme aktarıldığında işlem yürütme için kaynaklar korunur. Aramayı harici hale getirerek, sorgu hacmiyle eşleşecek şekilde kolayca ölçeği ayarlayabilirsiniz.|
|Ayrılmış arama çözümü | Şirket içi veya bulut hizmeti çözümleri, tüm işlevlere sahip ayrılmış arama çözümleridir. Arama teknolojileri genellikle dizin oluşturma ve sorgu işlem hatları üzerinde kontrol, daha zengin sorgulama ve filtreleme söz dizimine erişim, derece ve ilgi düzeyi üzerinde kontrol ve kendinden yönlendirmeli ve akıllı arama özellikleri sunar. <br/><br/>Şirket içinde veya sanal makinede barındırılan tek başına sunucu olarak veya bulut hizmeti olarak sunulan ayrılmış arama çözümleri bulabilirsiniz. [Minimum ek yük ve bakım ile anahtar teslim bir çözüm](#cloud-service-advantage) istiyorsanız bulut hizmeti doğru seçimdir. <br/><br/>Bulut paradigması içinde birçok sağlayıcı, tam metin araması, coğrafi arama ve arama girişlerindeki belirli bir belirsizlik düzeyini işleme yeteneğiyle birlikte karşılaştırılabilir temel özellikler sunar. Genellikle bu [özel bir özellik olup](#feature-drilldown) en iyi uygunluğu belirleyen yönetimin, araçların ve API’lerin kolaylaştırılmasını sağlar. |

Bulut sağlayıcıları arasında Azure Search, Azure’daki içerik depoları ve veritabanları üzerinde tam metin arama iş yükleri için, öncelikli olarak hem bilgi alımı hem de içerik gezintisi için aramayı kullanan uygulamalar için en güçlü seçenektir. Temel güçlü yönleri şunlardır:

+ Dizin oluşturma katmanında Azure veri tümleştirmesi (gezginler)
+ Merkezi yönetim için Azure portalı
+ Azure ölçekleme, güvenilirlik ve birinci sınıf kullanılabilirlik
+ 56 dilde güçlü metin araması için çözümleyicilerle birlikte linguistik ve özel analiz
+ [Arama odaklı uygulamalarda ortak olan temel özellikler](#feature-drilldown): puanlama, modelleme, öneriler, eş anlamlılar, coğrafi arama ve daha fazlası.

> [!Note]
> Azure olmayan veri kaynakları tam olarak desteklenir, ancak dizin oluşturucular yerine daha kod kullanımı yoğun bir itme yöntemini kullanır. API’leri kullanarak herhangi bir JSON belge koleksiyonunun bir Azure Search dizinine kanalını oluşturabilirsiniz.

Müşterilerimiz arasında, Azure Search’teki en zengin özelliklerden yararlanabilenler, çevrimiçi kataloglar, iş kolu programları ve belge bulma uygulamalarıdır.

## <a name="rest-api--net-sdk"></a>REST API’si | .Net SDK’sı

Portalda birçok görev gerçekleştirilebilse de Azure Search, mevcut uygulamalarda arama işlevselliğini tümleştirmek isteyen geliştiriciler için tasarlanmıştır. Aşağıdaki programlama arabirimleri kullanılabilir.

|Platform |Açıklama |
|-----|------------|
|[REST](/rest/api/searchservice/) | Xamarin, Java ve JavaScript gibi, programlama platformu ve dili tarafından desteklenen HTTP komutları|
|[.NET SDK](search-howto-dotnet-sdk.md) | REST API’si için .NET sarmalayıcı, C# dilinde ve .NET Framework’ü hedefleyen diğer yönetilen kod dillerinde verimli kodlama sunar |

## <a name="free-trial"></a>Ücretsiz deneme sürümü
Azure aboneleri, [Ücretsiz katmanda bir hizmet sağlayabilir](search-create-service-portal.md).

Abone değilseniz, [ücretsiz olarak bir Azure hesabı açabilirsiniz](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F). Ücretli Azure hizmetlerini denemek için krediler alırsınız. Krediler bittikten sonra hesabı tutabilir ve [ücretsiz Azure hizmetlerini](https://azure.microsoft.com/free/) kullanabilirsiniz. Açıkça ayarlarınızı değiştirip ücretlendirme istemediğiniz sürece kredi kartınız asla ücretlendirilmez.

Alternatif olarak [MSDN abone avantajlarını etkinleştirebilirsiniz](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F): MSDN aboneliğiniz, ücretli Azure hizmetlerinizi kullanabildiğiniz her ay size kredi verir. 

## <a name="how-to-get-started"></a>Nasıl kullanmaya başlarım

1. [Ücretsiz katmanda](search-create-service-portal.md) bir hizmet oluşturun.

2. Aşağıdaki öğreticilerden biri veya daha fazlasında ilerleyin. 

  + [.NET SDK’sını kullanma](search-howto-dotnet-sdk.md), yönetilen kodda ana adımları gösterir.  
  + [REST API'si ile çalışmaya başlama](https://github.com/Azure-Samples/search-rest-api-getting-started), REST API’sinin kullanımıyla aynı adımları gösterir.  
  + Yerleşik dizin oluşturma ve prototip özelliklerini kullanarak [portalda ilk dizininizi oluşturun](search-get-started-portal.md).   

Arama motorları, mobil uygulamalarda, web’de ve kurumsal veri depolarında bilgi alımını sağlayan genel tetikleyicilerdir. Azure Search size büyük ticari web sitelerindekine benzer bir arama deneyimi oluşturma araçları sunar.

Program yöneticisi Liam Cavanagh’ın bu 9 dakikalık videosunda, arama motorunu tümleştirmenin uygulamanıza nasıl fayda sağlayabileceğini öğrenebilirsiniz. Kısa deneme sürümlerinde, Azure Search’teki temel özellikler ve tipik bir iş akışının nasıl göründüğü ele alınır. 

>[!VIDEO https://channel9.msdn.com/Events/Connect/2016/138/player]
 
+ 0.-3. dakikada, temel özellikler ve kullanım durumları ele alınmaktadır.
+ 3.-4. dakikada, hizmet sağlama ele alınmaktadır. 
+ 4.-6. dakikada, yerleşik emlak veri kümesini kullanarak bir dizin oluşturmak için kullanılan Veri İçeri Aktarma sihirbazı ele alınmaktadır.
+ 6.-9. dakikada, Arama Gezgini ve çeşitli sorgular ele alınmaktadır.


