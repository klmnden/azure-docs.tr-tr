---
title: Azure Search nedir | Microsoft Docs
description: "Azure arama, tam olarak yönetilen barındırılan bulut arama hizmetidir. Bu özellik genel bakışı daha fazla bilgi edinin."
services: search
manager: jhubbard
author: ashmaka
documentationcenter: 
ms.assetid: 50bed849-b716-4cc9-bbbc-b5b34e2c6153
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 06/26/2017
ms.author: ashmaka
ms.openlocfilehash: f1a3e91a5de1962d2c060e030b3d662b0f4c1da8
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2017
---
# <a name="what-is-azure-search"></a>Azure Search nedir?
Azure arama geliştiricilere API'ler sağlar ve web, mobil ve kurumsal uygulamalarda, veriler üzerinde zengin arama deneyimi eklemek için Araçlar bir bulut hizmet olarak arama çözümüdür.

Basit bir işlevselliği kullanıma sunulan [REST API](/rest/api/searchservice/) veya [.NET SDK'sı](search-howto-dotnet-sdk.md) arama teknolojisi devralınmış karmaşıklığını maskeleyerek. API'ları yanı sıra Azure portalı yönetim ve prototip oluşturma desteği sağlar. Altyapı ve kullanılabilirlik Microsoft tarafından yönetilir.

<a name="feature-drilldown"></a>

## <a name="feature-summary"></a>Özellik Özeti

| Kategori | Özellikler |
|----------|----------|
|Tam metin araması ve metin analizi | [**Tam metin araması** ](search-lucene-query-architecture.md) çoğu arama tabanlı uygulamalar için birincil kullanım örneği değil. Desteklenen bir söz dizimi kullanılarak sorguları şeklide. <br/><br/>[Basit Sorgu söz dizimi](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) mantıksal işleçler, tümcecik arama işleçleri, sonek işleçleri, öncelik işleçleri sağlar.<br/><br/>[Lucene sorgu söz dizimi](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) benzer arama, yakınlık araması, terim artırma ve normal ifadeler için uzantıları ile basit sözdizimi tüm işlemleri içerir.| 
| Veri tümleştirmesi | JSON veri yapısı gönderilen sağlanan azure Search dizinlerini herhangi bir kaynaktan verileri kabul etmek. <br/><br/> İsteğe bağlı olarak, azure'da desteklenen veri kaynakları için kullanabileceğiniz [ **dizin oluşturucular** ](search-indexer-overview.md) otomatik olarak gezinmek için [Azure SQL veritabanı](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md), [Azure Cosmos DB](search-howto-index-documentdb.md), veya [Azure Blob Depolama](search-howto-indexing-azure-blob-storage.md) search dizininizi eşitlemek için birincil veri deposuyla içerik. Azure Blob dizin oluşturucular gerçekleştirebilir *belge çözme* için [ana dosya biçimleri dizin](search-howto-indexing-azure-blob-storage.md), Microsoft Office, PDF ve HTML belgeleri de dahil olmak üzere. |
| Arama analizi | [**Özel sözcük çözümleyiciler** ](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search) ses eşleşen kullanarak karmaşık arama sorguları ve normal ifadeler için kullanılabilir. |
| Dil desteği | [**Dil Çözümleyicileri** ](https://docs.microsoft.com/rest/api/searchservice/language-support) Lucene Microsoft'un doğal dil işlemciler akıllıca dile özgü linguistics işlemek için 56 farklı dillerde kullanılabilir yanı sıra fiil zamanlarını, cinsiyetiniz, düzensiz çoğul adlar (örneğin, 'fareler' ve ' fare'), word XML'deki bileşik, sözcük bölme (için dilleri boşluk) ve benzeri. |
| Coğrafi arama | Azure arama akıllıca, filtreleri, işler ve coğrafi konumları görüntüler. Arama sonucu yakınlık fiziksel konuma göre verileri araştırmak kullanıcıların sağlar. [Bu videoyu izleyin](https://channel9.msdn.com/Shows/Data-Exposed/Azure-Search-and-Geospatial-Data) veya [Bu örnek gözden](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs) daha fazla bilgi için. |
| Kullanıcı deneyimi özellikleri | [**Arama önerileri** ](https://docs.microsoft.com/rest/api/searchservice/suggesters) arama çubuğunda yazarken tamamlanan sorgular için etkinleştirilebilir. Kullanıcıların kısmi arama giriş girerken gerçek belgeleri dizininize önerilir. <br/><br/>[**Modellenmiş bir gezinmede** ](https://docs.microsoft.com/azure/search/search-faceted-navigation) tek sorgu parametresi etkinleştirilir. Azure arama kategorilerini liste arka plan kod olarak bağımsız (örneğin, fiyat-range veya marka katalog öğeleri filtrelemek için) filtrelemesi için kullanabileceğiniz bir modellenmiş bir gezinmede yapısı döndürür. <br/><br/> [**Filtreler** ](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search) modellenmiş bir gezinmede uygulamanızın UI dahil, sorgu formülasyonu geliştirmek ve temel alınarak kullanıcı veya Geliştirici belirtilen ölçütleri filtrelemek için kullanılabilir. OData sözdizimini kullanarak filtreleri oluşturun.<br/><br/> [**İsabet vurgulama** ](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) metin eşleşen bir anahtar sözcüğü arama sonuçlarında biçimlendirme uygular. Hangi alanların vurgulanan parçacıkları dönüş seçebilirsiniz.<br/><br/>[**Sıralama** ](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) dizin şemasını aracılığıyla birden çok alan için sunulan ve bir tek arama parametresiyle sorgu zamanında yükseğe.<br/><br/> [**Disk belleği** ](search-pagination-page-layout.md) ve arama sonuçlarınızı azaltma kolay ince bizi denetimiyle arama sonuçlarınızı Azure Search sunar.  
| İlgi Düzeyi | [**Basit Puanlama** ](/rest/api/searchservice/add-scoring-profiles-to-a-search-index) Azure Search'ün anahtar avantajdır. Puanlama profilleri ilgi belgelerde kendilerini değerlerin bir işlevi olarak model oluşturmak için kullanılır. Örneğin, yeni ürünleri isteyebilir veya arama sonuçlarında daha yüksek görünmesi ürünleri indirimli. İzlenen ve ayrı olarak depolanan müşteri arama tercihlerinize göre dayalı kişiselleştirilmiş Puanlama için etiketler kullanarak Puanlama profilleri de oluşturabilirsiniz. |
| İzleme ve Raporlama | [**Arama trafiği analytics** ](search-traffic-analytics.md) toplanan ve hangi kullanıcıların arama kutusuna yazmaya Öngörüler kilidini açmak için analiz edilir. <br/><br/>Sorguları ikinci, gecikme ve azaltma her ölçümleri yakalanan ve ek yapılandırma gerektirmeden portal sayfalarında bildirdi. Ayrıca İzleyici dizin kolayca ve gerektiğinde kapasite ayarlayabilmesi belge sayar. Daha fazla bilgi için bkz: [Hizmet Yönetimi](search-manage.md) |
| Prototip oluşturma ve denetleme araçları | Portalı'nda kullanabileceğiniz [ **verilerini İçeri Aktar Sihirbazı** ](search-import-data-portal.md) dizin oluşturucular, dizin designer bir dizin oluşturan göze yapılandırmak için ve [ **arama Gezgini** ](search-explorer.md) sorguları sınamak ve puanlama profilleri daraltın. Bir dizin şemasını görüntülemek için de açabilirsiniz. |
| Altyapı | **Yüksek oranda kullanılabilir platform** son derece güvenilir arama servis deneyimi sağlar. Düzgün şekilde genişletilmiş zaman [Azure Search sunar % 99,9 SLA](https://azure.microsoft.com/support/legal/sla/search/v1_0/).<br/><br/> **Tam olarak yönetilen ve ölçeklenebilir** bir uçtan uca çözümü olarak Azure arama kesinlikle hiçbir altyapı Yönetimi gerektirir. Hizmetiniz daha fazla belge depolama, daha yüksek sorgu yüklerinin veya her ikisi de işlemek için iki boyut ölçeklendirme tarafından gereksinimlerinize uyarlanabilir.

## <a name="how-to-use-azure-search"></a>Azure Search kullanma
### <a name="step-1-provision-service"></a>1. adım: Sağlama hizmeti
Bir Azure Search Hizmeti dönmesi [Azure portal](https://portal.azure.com/) aracılığıyla veya [Azure kaynak yönetimi API](/rest/api/searchmanagement/). Diğer aboneleriyle paylaşılan ücretsiz hizmet seçebilirsiniz veya [katmanı Ücretli](https://azure.microsoft.com/pricing/details/search/) yalnızca hizmetiniz tarafından kullanılan kaynakları dedicates. Ücretli katmanlarda iki boyut bir hizmetini ölçeklendirebilirsiniz: 

- Yoğun sorgu yükünü işlemek için kapasite büyümeye çoğaltmaları ekleyin.   
- Daha fazla belge için depolama büyümeye bölümleri ekleyin. 

Belge depolama ve sorgu işleme ayrı olarak işleme üretim gereksinimlerine göre resourcing ayarlama.

### <a name="step-2-create-index"></a>2. adım: dizin oluşturma
Aranabilir içeriği yükleyebilir önce ilk Azure Search dizini tanımlamanız gerekir. Verilerinizi tutan ve arama sorguları kabul edebileceği bir veritabanı tablosu gibi dizinidir. Arama, benzer bir veritabanındaki alanlara istediğiniz belgelerinin yapısını yansıtacak şekilde eşlemek için dizin şemasını tanımlayın.

Azure portalında oluşturulan ya da program aracılığıyla kullanarak bir şema [.NET SDK'sı](search-howto-dotnet-sdk.md) veya [REST API](/rest/api/searchservice/).

### <a name="step-3-index-data"></a>3. adım: Dizin verileri
Bir dizin tanımla sonra içerik yüklemek hazırsınız. İtme veya çekme modeli kullanabilirsiniz.

Çekme modeli, dış veri kaynaklarından verileri alır. Üzerinden desteklenen *dizin oluşturucular* kolaylaştırmak ve bağlanma, okuma ve verileri seri hale getirme gibi veri alımı yönlerini otomatik hale getirme. [Dizin oluşturucular](/rest/api/searchservice/Indexer-operations) Azure Cosmos DB, Azure SQL Database, Azure Blob Storage ve SQL Server bir Azure VM ile barındırılan için kullanılabilir. Bir dizin oluşturucu için isteğe bağlı veya zamanlanan veri yenileme yapılandırabilirsiniz.

Gönderme modeli, SDK veya bir dizine güncelleştirilmiş belgeleri göndermek için kullanılan REST API'leri aracılığıyla sağlanır. JSON biçimini kullanarak herhangi bir veri kümesinden alınan veri gönderebilir. Bkz: [ekleme, güncelleştirme veya silme belgeleri](/rest/api/searchservice/addupdate-or-delete-documents) veya [.NET SDK'sını kullanma)](search-howto-dotnet-sdk.md) veri Yükleme Kılavuzu.

### <a name="step-4-search"></a>Adım 4: arama
Bir dizin doldurduktan sonra şunları yapabilirsiniz [arama sorguları göndermek](/rest/api/searchservice/Search-Documents) REST API veya .NET SDK'sı ile basit HTTP isteklerini kullanarak hizmet uç için.

## <a name="how-azure-search-compares"></a>Azure Search nasıl karşılaştırır

Müşteriler genellikle Azure Search'te arama ile ilgili diğer çözümlerle nasıl karşılaştırır isteyin. Aşağıdaki tabloda farklar özetlenmektedir.

| Karşılaştırılan | Temel farklılıklar |
|--|--|
|Bing | [Bing Web arama API](https://docs.microsoft.com/azure/cognitive-services/bing-web-search/) gönderdiğiniz eşleşen koşulları aratıp dizinlerinde arar. Dizinleri HTML, XML ve diğer web içerikleri genel sitelere yerleşik olarak bulunur. [Bing özel arama](https://docs.microsoft.com/azure/cognitive-services/bing-custom-search/) tek tek web sitelerinin kapsamlı içerik türleri için web aynı Gezgin teknolojisi sunar.<br/><br/>Azure Search verileri ve belgeleri, genellikle çeşitli kaynaklardan sahip olduğunuz doldurulan tanımlamak dizin arar. Azure arama Gezgini capabilies bazı veri kaynakları için sahip [dizin oluşturucular](search-indexer-overview.md), ancak dizini şemanızı tek, birleştirilmiş bir aranabilir kaynak uyan herhangi bir JSON belgesi gönderebilir. |
|Veritabanı arama | [SQL Server tam metin araması](https://docs.microsoft.com/sql/relational-databases/search/full-text-search) içerik SQL tablolardaki DBMS iç içindir. <br/><br/>Azure arama heterojen kaynaklardan içeriği depolar ve özelleştirilmiş metin dil ve özel analizi gibi işleme özellikleri sunar. [Tam metin arama motoru](search-lucene-query-architecture.md) Azure Search'te Apache Lucene, bilgi alma standart endüstri üzerine inşa edilmiştir. <br/><br/>Kaynak kullanımını, yalnızca başka bir ton noktasıdır. Doğal dil arama pkı'ya yoğun görülür. Ayrılmış bir çözüm için yük boşaltma arama işlem kaynakları korur. Harici hale getirerek aramada sorgu toplu eşleştirmek için ölçek kolayca ayarlayabilirsiniz.|
|Ayrılmış arama çözümü | Şirket içi veya buluta hizmet çözümlerine ayrılmış arama tam spektrumun işlevsellikle çözümdür. Arama teknolojileri genellikle dizin oluşturma ve sorgu ardışık düzen üzerinde denetim sağlar, daha zengin sorgu erişmek ve sözdizimi, filtreleme derece ve uygunluğu ve kendi kendine yönlendirilmiş ve akıllı arama özellikleri üzerinden denetleyin. <br/><br/>Hizmet veya tek başına sunucu olarak şirket içinde veya bir sanal makine üzerinde barındırılan bir bulut olarak sunulan ayrılmış arama çözümleri bulabilirsiniz. İstiyorsanız, bir bulut hizmeti doğru seçimdir bir [anahtar teslim çözümüyle en az ek yükü ve Bakım ve ayarlanabilir ölçek](#cloud-service-advantage). <br/><br/>Bulut kip içinde birkaç sağlayıcıları karşılaştırılabilir Temel özelliklerle tam metin araması, coğrafi arama ve belirli bir düzeyde arama girişleri belirsizlik işleme yeteneği sunar. Genellikle, sahip bir [özel özellik](#feature-drilldown), veya kolaylığı ve genel Basitlik API'leri, araçları ve en iyi sığacak şekilde belirler yönetimi. |

Bulut sağlayıcıda Azure arama içerik depoları ve arama bilgileri alma ve içerik gezinti için öncelikle bağlı uygulamaları için Azure üzerinde veritabanları üzerinde tam metin arama iş yükleri için güçlü olur. Anahtar gücü şunları içerir:

+ Dizin oluşturma katmanında Azure veri tümleştirme (gezginleri)
+ Merkezi Yönetim için Azure portalı
+ Azure ölçek, güvenilirlik ve dünya çapındaki kullanılabilirliği
+ Çözümleyiciler 56 dillerde düz tam metin araması için ile dil ve özel analizi
+ [Çekirdek özellikleri için arama merkezli ortak uygulamalar](#feature-drilldown): Puanlama, olduğunu, öneriler, eş anlamlıları, coğrafi arama ve daha fazla.

> [!Note]
> Azure olmayan veri kaynakları tam olarak desteklenir, ancak dizin oluşturucular yerine daha kod Kullanımı Yoğun gönderme yöntemi kullanır. API'leri kullanarak Azure Search dizini için herhangi bir JSON belge koleksiyonu iletebildiğiniz.

Müşterilerimizin arasında Azure arama özellikleri yelpazedeki yararlanamaz çevrimiçi katalogları, iş programlar ve belge bulma uygulamaları içerir.

## <a name="rest-api--net-sdk"></a>REST API | .net SDK'sı

Portalda çok sayıda görevler gerçekleştirilebilir olsa da, Azure Search arama işlevini mevcut uygulamalarınızı tümleştirmek için isteyen geliştiriciler için yöneliktir. Aşağıdaki programlama arabirimleri kullanılabilir.

|Platform |Açıklama |
|-----|------------|
|[REST](/rest/api/searchservice/) | Tüm programlama platform ve dili, Xamarin, Java ve JavaScript gibi tarafından desteklenen HTTP komutları|
|[.NET SDK](search-howto-dotnet-sdk.md) | C# ve .NET Framework'ü hedefleme diğer yönetilen kod dilleri verimli kodlama REST API için .NET sarmalayıcı sunar |

## <a name="free-trial"></a>Ücretsiz deneme
Azure aboneleri için [ücretsiz katmanı hizmetinde sağlamak](search-create-service-portal.md).

Bir abone değilseniz, yapabilecekleriniz [ücretsiz bir Azure hesabı açabilirsiniz](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F). Ücretli Azure hizmetlerini denemek için krediler alırsınız. Kullanıldıktan sonra hesabı sürdürebilir ve kullanmak [Azure Hizmetleri serbest](https://azure.microsoft.com/free/). Açıkça ayarlarınızı değiştirip ücretlendirme sürece kredi kartınızdan asla ücret kesilir.

Alternatif olarak, [MSDN abone Avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F): MSDN aboneliğiniz size kredi verir Ücretli Azure hizmetlerinizi kullanabildiğiniz her ay. 

## <a name="how-to-get-started"></a>Nasıl kullanmaya başlarım

1. Bir hizmet oluşturma [ücretsiz katmanı](search-create-service-portal.md).

2. Bir veya daha fazla aşağıdaki öğreticiler adım. 

  + [.NET SDK'sını kullanma](search-howto-dotnet-sdk.md) yönetilen kodda ana adımları gösterir.  
  + [REST API'si ile çalışmaya başlama](https://github.com/Azure-Samples/search-rest-api-getting-started) REST API kullanarak aynı adımlar gösterilmektedir.  
  + [Portalda, ilk dizininizi oluşturma](search-get-started-portal.md) yerleşik dizin oluşturma ve prototip özelliklerini kullanma.   

Arama motorları, mobil uygulamaları, web ve kurumsal veri depoları içindeki bilgileri alma ortak sürücülerdir. Azure Search'te bir arama deneyimi büyük ticari web sitelerinde benzer oluşturmak için Araçlar verir.

Liam Cavanagh program Yöneticisi'nden bu 9 dakikalık videoda, bir arama motoru tümleştirme uygulamanızı nasıl yararlanabilir öğrenin. Kısa gösterileri Azure Search ve normal bir iş akışı benzer anahtar özellikleri kapsar. 

>[!VIDEO https://channel9.msdn.com/Events/Connect/2016/138/player]
 
+ Kapak anahtar özellikleri ve kullanım örnekleri 0-3 dakika.
+ 3-4 dakika kapsayan hizmet sağlama. 
+ 4-6 dakika yerleşik Gayrimenkul dataset kullanarak dizini oluşturmak için kullanılan veri içeri aktarma Sihirbazı'nı ele alınmaktadır.
+ 6-9 dakika arama Gezgini ve çeşitli sorguları ele alınmaktadır.


