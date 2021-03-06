---
title: 'Hızlı Başlangıç: Azure portal - Azure Search kullanarak dizin sorgulama oluşturma ve yükleme'
description: Verileri İçeri Aktarma Sihirbazı'nı, Azure portalında oluşturma, yükleme ve İlk Azure Search dizininizi sorgulama için kullanın.
author: lobrien
manager: cgronlun
tags: azure-portal
services: search
ms.service: search
ms.topic: tutorial
ms.date: 07/01/2019
ms.author: laobri
ms.custom: seodec2018
ms.openlocfilehash: 2a4d7435383f740dc386a740062e66cd2d3585b0
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67798839"
---
# <a name="quickstart-create-an-azure-search-index-using-the-azure-portal"></a>Hızlı Başlangıç: Azure portalını kullanarak bir Azure Search dizini oluşturma
> [!div class="op_single_selector"]
> * [Portal](search-get-started-portal.md)
> * [PowerShell](search-get-started-powershell.md)
> * [Postman](search-get-started-postman.md)
> * [Python](search-get-started-python.md)
> * [C#](search-get-started-dotnet.md)

Bir hızlı mvc'deki için Azure Search kavramları, Azure portalında yerleşik araçları deneyebilirsiniz. Sihirbazlar ve düzenleyicileri .NET ve REST API'leri ile tam eşlik sağlamaz, ancak hızlı bir şekilde kod gerektirmeyen bir giriş, dakika içinde örnek veriler ilgi çekici sorguları yazma başlayabilirsiniz.

> [!div class="checklist"]
> * Azure üzerinde barındırılan ücretsiz ortak örnek veri kümesi ile başlayın
> * Çalıştırma **verileri içeri aktarma** Azure Search'te veri yükleme ve bir dizin oluşturmak için Sihirbazı
> * Portalda dizin oluşturma ilerleme durumunu izleyin
> * Varolan bir dizini ve onu değiştirmek için seçenekleri görüntüleme
> * Tam metin arama, filtreler, modeller, belirsiz arama ve ile coğrafi aramayı keşfedin **arama Gezgini**

Araçları çok sınırlama, önünde bir [. NET'te Azure Search programlamaya kod tabanlı giriş](search-howto-dotnet-sdk.md) veya [REST API çağrıları yapmak için Postman](search-get-started-postman.md). Bu [Azure Search’e Genel Bakış videosunun](https://channel9.msdn.com/Events/Connect/2016/138) üçüncü dakikasından izlemeye başlayarak bu öğreticideki adımların 6 dakikalık bir gösterimini izleyebilirsiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun. 

## <a name="prerequisites"></a>Önkoşullar

[Azure Search hizmeti oluşturma](search-create-service-portal.md) veya [mevcut bir hizmet bulma](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) geçerli aboneliğinizdeki. Bu Hızlı Başlangıç için ücretsiz bir hizmet kullanabilirsiniz. 

### <a name="check-for-space"></a>Alan denetleme

Birçok müşteri ücretsiz hizmetle başlar. Bu sürüm üç dizin, üç veri kaynağı ve üç dizin oluşturucu ile sınırlıdır. Başlamadan önce ek öğeler için yeriniz olduğundan emin olun. Bu öğreticide her nesneden birer tane oluşturulur.

Hizmet panosundaki bölümleri kaç dizin, dizin oluşturucular ve veri kaynaklarına sahip olduğunuzu gösterir. 

![Dizin, dizin oluşturucular ve veri kaynaklarının listesi](media/search-get-started-portal/tiles-indexers-datasources.png)

## <a name="create-index"></a> Dizin oluşturma ve verileri yükleme

Arama sorguları aranabilir veriler, meta veriler ve arama davranışlarını iyileştiren ek yapılar içeren bir [*dizin*](search-what-is-an-index.md) kullanır.

Bu öğreticide **Verileri içeri aktar** sihirbazı aracılığıyla bir [*dizin oluşturucu*](search-indexer-overview.md) kullanılarak gezilebilen yerleşik bir örnek veri kümesi kullanacağız. Dizin oluşturucu, kaynağa özgü bir gezgindir ve desteklenen Azure veri kaynaklarındaki meta verileri ve içeriği okuyabilir. Normalde, dizin oluşturucular programlı olarak kullanılır, ancak bloblarda erişim Portalı'nda **verileri içeri aktarma** Sihirbazı. 

### <a name="step-1---start-the-import-data-wizard-and-create-a-data-source"></a>1\. adım - Veri Alma Sihirbazı'nı başlatın ve bir veri kaynağı oluşturun

1. Azure Search Hizmeti panosunda **verileri içeri aktarma** oluşturmak ve search dizinini doldurmak için komut çubuğunda.

   ![Verileri içeri aktar komutu](media/search-get-started-portal/import-data-cmd.png)

2. Sihirbazı'nda tıklatın **verilerinize bağlanın** > **örnekleri** > **hotels örnek**. Bu veri kaynağı yerleşiktir. Kendi veri kaynağı oluşturuyorsanız, ad, tür ve bağlantı bilgilerini belirtmek gerekir. Oluşturulan kaynak, diğer içeri aktarma işlemlerinde yeniden kullanılabilecek bir “mevcut veri kaynağı” olur.

   ![Örnek veri kümesi seçme](media/search-get-started-portal/import-datasource-sample.png)

3. Bir sonraki sayfasına devam edin.

   ![Bilişsel arama için sonraki sayfa düğmesi](media/search-get-started-portal/next-button-add-cog-search.png)

### <a name="step-2---skip-cognitive-skills"></a>Adım 2 - Atla Bilişsel beceriler

Oluşturma Sihirbazı'nı destekleyen bir [bilişsel beceriler işlem hattı](cognitive-search-concept-intro.md) Bilişsel Hizmetleri yapay ZEKA algoritmaları dizin içine ekleme için. 

Biz şimdilik bu adımı atlayın ve doğrudan geçin **hedef dizini Özelleştir**.

   ![Bilişsel beceri adımını atlama](media/search-get-started-portal/skip-cog-skill-step.png)

> [!TIP]
> Yapay ZEKA dizin örneği de adım adım bir [hızlı](cognitive-search-quickstart-blob.md) veya [öğretici](cognitive-search-tutorial-blob.md).

### <a name="step-3---configure-index"></a>3\. adım: dizin yapılandırın

Genellikle dizin oluşturma, veriler yüklenmeden önce tamamlanan kod tabanlı bir alıştırma, oluşur. Ancak, bu öğreticide da anlaşılacağı gibi sihirbaz gezinebileceği tüm veri kaynağı için temel bir dizin oluşturabilirsiniz. Dizin için en azından bir ad ve alan koleksiyonu gerekir ve her belgenin benzersiz olarak tanımlanabilmesi için bir alanın belge anahtarı olarak işaretlenmiş olması gerekir. Ayrıca, otomatik tamamlama veya önerilen sorgular istiyorsanız dil Çözümleyicileri veya öneri araçları belirtebilirsiniz.

Alanların veri türleri ve öznitelikleri vardır. Üstteki onay kutuları, alanın nasıl kullanılacağını denetleyen *dizin öznitelikleridir*.

* **Alınabilir**, arama sonuçları listesinde çıktığı anlamına gelir. Örneğin, yalnızca filtre ifadelerinde kullanılan alanları için bu onay kutusunu temizleyerek dışı arama sonuçları için ayrı alanlar olarak işaretleyebilirsiniz.
* **Anahtar** belgenin benzersiz tanımlayıcısı. Her zaman bir dize olduğu ve gerekli değildir.
* **Filtrelenebilir**, **sıralanabilir**, ve **modellenebilir** alanları bir filtre, sıralama veya çok yönlü gezinme yapısı kullanılıp kullanılmayacağını belirler.
* **Aranabilir**, bir alanın tam metin aramasına dahil olduğu anlamına gelir. Dizelerde arama yapılabilir. Sayısal alanlar ve Boolean alanları genellikle aranamaz olarak işaretlenir.

Depolama gereksinimleri seçiminizi sonucunda değişiklik yok. Örneğin, ayarlarsanız **alınabilir** depolama gereksinimleri değil Yukarı Git özniteliği birden çok alan.

Varsayılan olarak sihirbaz tarafından anahtar alanının temeli olarak benzersiz tanımlayıcıların bulunması için veri kaynağı taranır. *Dizeleri* olarak öznitelikli **alınabilir** ve **aranabilir**. *Tamsayıları* olarak öznitelikli **alınabilir**, **filtrelenebilir**, **sıralanabilir**, ve **modellenebilir**.

1. Varsayılanları kabul edin. 

   Varolan bir otel veri kaynağını kullanarak ikinci bir kez Sihirbazı yeniden çalıştırın, varsayılan öznitelikler içeren dizin yapılandırılmaz. İleride içeri aktarmalar özniteliklerinde el ile seçmeniz gerekir. 

   ![Oluşturulan Oteller dizinini](media/search-get-started-portal/hotelsindex.png)

2. Bir sonraki sayfasına devam edin.

   ![Sonraki sayfaya dizin oluşturucu oluşturma](media/search-get-started-portal/next-button-create-indexer.png)

### <a name="step-4---configure-indexer"></a>4\. adım: dizin oluşturucuyu yapılandırma

**Verileri içeri aktarma** sihirbazından çıkmadan **Dizin Oluşturucu** > **Ad**’a tıklayın ve dizin oluşturucu için bir ad yazın.

Bu nesne, yürütülebilir bir işlemi tanımlar. Yinelenen bir zamanlamaya göre put ancak şimdilik dizin oluşturucunun bir kez çalıştırmak için varsayılan seçeneği kullanmanız hemen.

Tıklayın **Gönder** oluşturmak ve aynı anda dizin oluşturucuyu çalıştırmak için.

  ![Hotels dizin oluşturucu](media/search-get-started-portal/hotels-indexer.png)

## <a name="monitor-progress"></a>İlerlemeyi İzle

Sihirbaz burada ilerleme durumunu izleyebilirsiniz. dizin oluşturucular listeye almanız gerekir. Kendi kendine gezintiye genel bakış sayfasında ve tıklayın Git **dizin oluşturucular**.

Portalın sayfanın birkaç dakika sürebilir ve durumunu gösteren ile listesinde yeni oluşturulan dizin oluşturucu görmeniz gerekir "Sürüyor" ya da başarılı dizine eklenen belge sayısını.

   ![Dizin oluşturucu ilerleme durumu iletisi](media/search-get-started-portal/indexers-inprogress.png)

## <a name="view-the-index"></a>Dizini görüntüleme

Ana hizmet sayfası, Azure Search hizmetinizde oluşturulan kaynakların bağlantılarını sağlar.  Yeni oluşturduğunuz dizin görüntülemek için tıklayın **dizinleri** bağlantılar listesinden. 

   ![Hizmet panosundaki dizinler listesi](media/search-get-started-portal/indexes-list.png)

Bu listeden tıklayarak *hotels örnek* yeni dizin, dizin şemasını görüntülemek oluşturulmuş. ve isteğe bağlı olarak yeni alanlar ekleyin. 

**Alanları** sekmesi dizin şemasını gösterir. Yeni bir alan girin için listenin en alt kısma. Çoğu durumda, var olan alanları değiştiremezsiniz. Var olan alanlar Azure Search'te fiziksel olarak temsil edildiğinden kod ile dahi değişiklik yapılamaz. Temelde varolan bir alanı değiştirmek için özgün bırakmadan yeni bir dizin oluşturun.

   ![Örnek dizin tanımı](media/search-get-started-portal/sample-index-def.png)

Puanlama profilleri ve CORS seçenekleri gibi diğer yapılar herhangi bir noktada eklenebilir.

Dizin tasarımı sırasında düzenleme yapabileceğiniz ve yapamayacağınız alanları kavramak için birkaç dakikanızı ayırarak dizin tanımı seçeneklerini görüntüleyin. Gri renkli seçenekler, bir değerin değiştirme veya silme işlemleri için uygun olmadığını gösterir. 

## <a name="query-index"></a> Arama Gezgini'ni kullanarak sorgulama

Artık yerleşik [**Arama gezgini**](search-explorer.md) sorgu sayfasını kullanarak sorgulayabileceğiniz bir arama dizinine sahipsiniz. Bu sayfada rastgele sorgu dizelerini test etmek için kullanabileceğiniz bir arama kutusu bulunur.

**Arama Gezgini** işlemek için yalnızca donatılmış [REST API istekleri](https://docs.microsoft.com/rest/api/searchservice/search-documents), ancak her ikisi için söz dizimini kabul [Basit Sorgu söz dizimi](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) ve [tam Lucene sorgu ayrıştırıcısına](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search), kullanılabilen tüm arama parametrelerini artı [arama belge REST API'si](https://docs.microsoft.com/rest/api/searchservice/search-documents#bkmk_examples) operations.

> [!TIP]
> [Azure Search’e Genel Bakış videosunu](https://channel9.msdn.com/Events/Connect/2016/138) 6 dakika 8 saniye ileri alarak aşağıdaki adımların gösterimini izleyebilirsiniz.
>

1. Komut çubuğunda **Arama gezgini**'ne tıklayın.

   ![Search gezgini komutu](media/search-get-started-portal/search-explorer-cmd.png)

2. Gelen **dizin** açılır listesinde, seçin *hotels örnek*. Tıklayın **API sürümü** hangi REST API'lerin kullanılabildiğini görmek için açılır. Aşağıdaki sorgular için genel kullanıma sunulan sürümü (2019-05-06) kullanın.

   ![Dizin ve API komutları](media/search-get-started-portal/search-explorer-changeindex.png)

3. Aşağıdaki sorgu dizeleri arama çubuğunda yapıştırın ve tıklayın **arama**.

   ![Sorgu dizesi ve arama düğmesi](media/search-get-started-portal/search-explorer-query-string-example.png)

## <a name="example-queries"></a>Örnek sorgular

Hüküm ve ifadeler, Bing veya Google arama veya tam olarak belirtilen sorgu ifadeleri neler için benzer girebilirsiniz. Sonuçları ayrıntılı JSON belgeleri olarak döndürülür.

### <a name="simple-query-with-top-n-results"></a>İlk N sonucu içeren basit sorgu

#### <a name="example-string-query-searchspa"></a>Örnek (dize sorgusu): `search=spa`

* **Arama** parametresi tam metin araması, bu durumda, bir anahtar sözcük arama girişi için kullanılan otel verileri içeren olanlar için döndüren *spa* belgedeki aranabilir alanların herhangi içinde.

* **Search gezgini** sonuçları JSON biçiminde döndürülür. Bu biçim ayrıntılı olmakla birlikte, belgelerin yoğun bir yapısı varsa okunması zordur. Bu kasıtlıdır; Tüm belgeyi görünürlük özellikle test sırasında geliştirme amacıyla önemlidir. Daha iyi bir kullanıcı deneyimi sunmak için [arama sonuçlarını işleyerek](search-pagination-page-layout.md) önemli öğeleri öne çıkaran bir kod yazmanız gerekir.

* Belgeler, dizinde "alınabilir" olarak işaretlenmiş tüm alanlardan oluşur. Portalda dizin özniteliklerini görüntülemek için tıklayın *hotels örnek* içinde **dizinleri** listesi.

#### <a name="example-parameterized-query-searchspacounttruetop10"></a>Örnek (Parametreli sorgu): `search=spa&$count=true&$top=10`

* **&** simgesi, herhangi bir sırada belirtilebilen arama parametreleri eklemek için kullanılır.

* **$Count = true** parametresi, döndürülen tüm belgelerin toplam sayısını döndürür. Bu değer arama sonuçlarının en üstüne yakın bir konumda görünür. **$count=true** tarafından bildirilen değişiklikleri izleyerek filtre sorgularını doğrulayabilirsiniz. Daha küçük sayılar filtrenizin çalıştığını gösterir.

* **$Top = 10** en yüksek dereceye sahip 10 belgelerin döndürür. Azure Search varsayılan olarak ilk 50 en iyi eşleşmeyi döndürür. **$top** ile bu miktarı artırabilir veya azaltabilirsiniz.

### <a name="filter-query"></a> Sorguyu filtreleme

**$filter** parametresini eklediğinizde, arama isteklerine filtreler de eklenir. 

#### <a name="example-filtered-searchbeachfilterrating-gt-4"></a>Örnek (filtrelenmiş): `search=beach&$filter=Rating gt 4`

* **$filter** parametresi, sağladığınız ölçütlerle eşleşen sonuçları döndürür. Bu durumda 4'ten büyük derecelendirmeleri.

* Filtre söz dizimi bir OData yapısıdır. Daha fazla bilgi edinmek için bkz. [OData söz dizimini filtreleme](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search).

### <a name="facet-query"></a> Sorguyu modelleme

Model filtreleri arama isteklerine dahil edilir. Sağladığınız model değeriyle eşleşen belgelerin toplam sayısını döndürmek için facet parametresini kullanabilirsiniz.

#### <a name="example-faceted-with-scope-reduction-searchfacetcategorytop2"></a>Örnek (kapsamı azaltılarak modellenmiş): `search=*&facet=Category&$top=2`

* **search=** * boş bir aramadır. Boş aramalar her şeyi arar. Boş sorgu göndermenin nedenlerinden biri, belge kümesinin tamamını filtrelemek veya görüntülemektir. Örneğin dizindeki tüm hotels oluşması için içeren bir gezinme yapısı isteyebilirsiniz.
* **facet**, bir kullanıcı arabirimi denetimine geçirebileceğiniz bir gezinti yapısı döndürür. Kategorileri ve bir sayımı döndürür. Bu durumda, kategoriler rahatça adlı bir alan dayalı *kategori*. Azure Search'te toplama yoktur ancak her kategorideki belge sayısını veren `facet` ile tahmini bir toplama gerçekleştirebilirsiniz.

* **$top=2** iki belge getirir ve sonuçları azaltmak veya artırmak için `top` kullanabileceğinizi gösterir.

#### <a name="example-facet-on-numeric-values-searchspafacetrating"></a>Örnek (modeli sayısal değerleri üzerinde): `search=spa&facet=Rating`

* Bu sorgu için yapılan metin aramasında derecelendirmesine olan *spa*. Terim *derecelendirme* alan olarak işaretlendiğinden odalı, ve dizin ve (sayısal, 1 ile 5), içerdiği değerlerin listelerini gruplar halinde kategorilere ayırmak için uygun bir model belirtilebilir.

* Yalnızca filtrelenebilir alanlardan görünüm oluşturulabilir. Yalnızca getirilebilir alanlar sonuçlarda döndürülebilir.

* *Derecelendirme* çift duyarlıklı kayan nokta bir alandır ve kesin değerine göre gruplandırma olacaktır. Aralığa göre gruplama hakkında daha fazla bilgi için (örneğin, "3 yıldız değerlendirmelerinde," "4 yıldız değerlendirmelerinde," vb.), bkz: [Azure Arama'da çok yönlü navigasyon uygulamak nasıl](https://docs.microsoft.com/azure/search/search-faceted-navigation#filter-based-on-a-range).


### <a name="highlight-query"></a> Arama sonuçlarını vurgulama

İsabet vurgulama, belirli bir alanda eşleşme bulunduğunda anahtar sözcükle eşleşen metinlere biçimlendirme eklenmesini ifade eder. Arama teriminiz uzun bir açıklamanın belirsiz bir yerindeyse, terimi bulmayı kolaylaştırmak için isabet vurgulama ekleyebilirsiniz.

#### <a name="example-highlighter-searchbeachhighlightdescription"></a>Örnek (vurgulama): `search=beach&highlight=Description`

* Bu örnekte, biçimlendirilmiş word *Plaj* açıklama alanında daha kolay olan.

#### <a name="example-linguistic-analysis-searchbeacheshighlightdescription"></a>Örnek (dilbilimsel analiz): `search=beaches&highlight=Description`

* Tam metin araması sözcük biçimlerini temel farklılığı tanır. Bu durumda, vurgulanan metni "Plaj" sözcük "dalışı" anahtar sözcük araması için yanıt, aranabilir alanları olan Oteller için arama sonuçlarını içerir. Dilbilimsel analiz nedeniyle sonuçlarda aynı kelimenin farklı biçimleri görüntülenebilir. 

* Azure Search, Lucene ve Microsoft’tan 56 çözümleyiciyi destekler. Azure Search tarafından standart olarak Lucene çözümleyici kullanılır.

### <a name="fuzzy-search"></a> Belirsiz aramayı deneme

Varsayılan olarak, sorgu terimleri gibi yazım *seatle* "Seattle" için eşleşme bölgesindeki döndürmek başarısız. Aşağıdaki örnek sonuç döndürmez.

#### <a name="example-misspelled-term-unhandled-searchseatle"></a>Örnek (yanlış yazılmış terim, işlenmiyor): `search=seatle`

Yazım hatalarını işlemek için belirsiz aramayı kullanabilirsiniz. Belirsiz arama, tam Lucene sorgu söz dizimini kullandığınızda etkinleştirilir ve bunun için yapmanız gereken iki işlem vardır: sorguda **queryType=full** belirtin ve arama sorgusunun sonuna **~** ekleyin.

#### <a name="example-misspelled-term-handled-searchseatlequerytypefull"></a>Örnek (yanlış yazılmış terim, işleniyor): `search=seatle~&queryType=full`

Bu örnekte, artık "Seattle" eşleşmeleri ile içeren belgeleri döndürür.

**queryType** belirtildiğinde varsayılan basit sorgu ayrıştırıcı kullanılır. Basit sorgu ayrıştırıcı daha hızlıdır ancak belirsiz arama, normal ifadeler, yakınlık araması ya da diğer gelişmiş sorgu türlerini kullanmanız gerekiyorsa tam söz dizimi gereklidir.

Belirsiz arama ve joker karakter araması, arama sonucunu etkiler. Bu sorgu biçimlerinde dilbilimsel analiz gerçekleştirilmez. Belirsiz arama ve joker karakter araması özelliklerini kullanmadan önce [Azure Search'te tam metin araması nasıl çalışır?](search-lucene-query-architecture.md#stage-2-lexical-analysis) sayfasına gidin ve sözcük analiziyle ilgili özel durumların anlatıldığı bölümü inceleyin.

Tam sorgu ayrıştırıcı tarafından etkinleştirilen sorgu senaryoları hakkında daha fazla bilgi edinmek için bkz. [Azure Search’te Lucene sorgu söz dizimi](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search).

### <a name="geo-search"></a> Jeo-uzamsal aramayı deneme

Koordinat içeren bir alanda [edm.GeographyPoint veri türü](https://docs.microsoft.com/rest/api/searchservice/supported-data-types) aracılığıyla jeo-uzamsal arama desteklenir. Coğrafi arama, [OData söz dizimini filtrele](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search) seçeneğinde belirtilen bir tür filtredir.

#### <a name="example-geo-coordinate-filters-searchcounttruefiltergeodistancelocationgeographypoint-12212-4767-le-5"></a>Örnek (coğrafi koordinat filtreleri): `search=*&$count=true&$filter=geo.distance(Location,geography'POINT(-122.12 47.67)') le 5`

Örnek sorgu tüm sonuçları konumsal verilere göre filtreler ve belirli bir noktaya 5 kilometreden daha yakın olan sonuçlar (enlem ve boylam koordinatları olarak belirtilir) döndürülür. **$count** ekleyerek mesafeyi veya koordinatları değiştirdiğinizde döndürülen sonuç sayısını görebilirsiniz.

Arama uygulamanız "yakınımda bul" özelliği içeriyorsa ya da harita navigasyonu kullanıyorsa jeo-uzamsal arama kullanışlıdır. Ancak tam metin arama değildir. Bir şehir veya ülkede/bölgede adına göre aramak için kullanıcı gereksinimleriniz varsa koordinatlara ek olarak şehir veya ülke/bölge adlarını içeren alanlar ekleyin.

## <a name="takeaways"></a>Paketler

Bu öğreticide, Azure portalını kullanarak Azure Search için hızlı bir giriş sağlanır.

**Verileri içeri aktar** sihirbazını kullanarak arama dizini oluşturmayı öğrendiniz. [Dizin oluşturucuların](search-indexer-overview.md) yanı sıra [yayımlanmış dizinler için desteklenen değişiklikler](https://docs.microsoft.com/rest/api/searchservice/update-index) gibi dizin tasarımıyla ilgili temel iş akışı hakkında bilgi edindiniz.

Azure portalda **Arama gezginini** kullanarak filtreler, sonuç vurgulama, belirsiz arama ve coğrafi arama gibi önemli özelliklerin gösterildiği örneklerle temel sorgu söz dizimini öğrendiniz.

Portalda dizin, dizin oluşturucular ve veri kaynaklarını bulma ayrıca öğrendiniz. Gelecekte yeni verilerle karşılaştığınızda portalı kullanarak tanımlarını veya alan koleksiyonlarını hızlı bir şekilde denetleyebilirsiniz.

## <a name="clean-up"></a>Temizleme

Kendi aboneliğinizde çalışırken, oluşturduğunuz kaynakları hala gerekip gerekmediğini belirlemek için iyi bir fikir sonunda, bir proje var. Kaynakları sol çalışan can para maliyeti. Kaynakları tek tek silmek ya da tüm kaynak kümesini silmek için kaynak grubunu silin.

Bulabilir ve Portalı'nda kaynaklarını yönetme kullanarak **tüm kaynakları** veya **kaynak grupları** sol gezinti bölmesindeki bağlantıyı.

Ücretsiz bir hizmet kullanıyorsanız, üç dizin, dizin oluşturucular ve veri kaynağı için sınırlı olduğunu unutmayın. Bireysel öğeleri limiti altında kalmak için portalda silebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

Programlama araçlarını kullanarak Azure Search hakkında daha fazla şey keşfedebilirsiniz:

* [.NET SDK'sını kullanarak bir dizin oluşturun](https://docs.microsoft.com/azure/search/search-create-index-dotnet)
* [REST API'lerini kullanarak dizin oluşturma](https://docs.microsoft.com/azure/search/search-create-index-rest-api)
* [Postman veya fiddler'ı ve Azure Search REST API'lerini kullanarak dizini oluşturma](search-get-started-postman.md)
