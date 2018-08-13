---
title: "Öğretici: Portal kullanarak Azure Search'te dizin oluşturma, sorgu ve filtreleme | Microsoft Docs"
description: Bu öğreticide Azure portalı ve önceden tanımlanmış örnek verileri kullanarak Azure Search’te bir dizin oluşturacaksınız. Tam metin arama, filtreler, modeller, belirsiz arama, coğrafi arama ve daha fazlasını keşfedin.
author: HeidiSteen
manager: cgronlun
tags: azure-portal
services: search
ms.service: search
ms.topic: tutorial
ms.date: 07/10/2018
ms.author: heidist
ms.openlocfilehash: aac579da3aaf6ab1507bbc12d79a5b183a82d665
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39592764"
---
# <a name="tutorial-use-built-in-tools-for-azure-search-indexing-and-queries"></a>Öğretici: Azure Search'te dizin oluşturma ve sorgular için yerleşik araçları kullanma

Azure Search kavramlarına hızlı bir genel bakış için Azure portaldaki Azure Search hizmet sayfasında bulunan yerleşik araçları kullanabilirsiniz. Bu araçlar, .NET ve REST API'lerinin tüm işlevlerine sahip olmayabilir. Ancak sihirbazlar ve düzenleyiciler, kod yazmadan Azure Search'ü kullanmaya başlamanızı ve doğrudan örnek veri kümesine ilgi çekici sorgular yazmanızı sağlar.

> [!div class="checklist"]
> * Genel örnek verilerle başlayın ve **Verileri içeri aktar** sihirbazıyla otomatik olarak bir Azure Search dizini oluşturun.
> * Azure Search'te yayımlanmış tüm dizinlerin şemasını ve özniteliklerini görüntüleyin.
> * **Search Gezgini** ile tam metin arama, filtreler, modeller, belirsiz arama ve coğrafi aramayı keşfedin.  

Araçlar sizi çok sınırlıyorsa [.NET ile Azure Search'te programlamaya kod tabanlı giriş](search-howto-dotnet-sdk.md) veya [REST API çağrıları için web testi araçları](search-fiddler.md) konularını inceleyebilirsiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun. Bu [Azure Search’e Genel Bakış videosunun](https://channel9.msdn.com/Events/Connect/2016/138) üçüncü dakikasından izlemeye başlayarak bu öğreticideki adımların 6 dakikalık bir gösterimini izleyebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

[Bir Azure Search hizmeti oluşturun](search-create-service-portal.md) veya geçerli aboneliğinizin altında var olan bir hizmeti bulun.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Azure Search hizmetinizin hizmet panosunu açın. Hizmet kutucuğunu panonuza sabitlemediyseniz hizmetinizi şu şekilde bulabilirsiniz:

   * Jumpbar’da, sol gezinti bölmesinde **Tüm hizmetler**’e tıklayın.
   * Aboneliğinizde kullanılabilen arama ile ilgili hizmetlerin listesini görmek için arama kutusuna *arama* yazın. **Arama hizmetleri**'ne tıklayın. Hizmetiniz listede görünür.

### <a name="check-for-space"></a>Alan denetleme

Birçok müşteri ücretsiz hizmetle başlar. Bu sürüm üç dizin, üç veri kaynağı ve üç dizin oluşturucu ile sınırlıdır. Başlamadan önce ek öğeler için yeriniz olduğundan emin olun. Bu öğreticide her nesneden birer tane oluşturulur.

> [!TIP]
> Hizmet panosundaki kutucuklar, şu anda kaç dizin, dizin oluşturucu ve veri kaynağına sahip olduğunuzu gösterir. Dizin oluşturucu kutucuğu, başarı ve başarısızlık göstergelerini gösterir. Dizin oluşturucu sayısını görüntülemek için kutucuğa tıklayın.
>
> ![Dizin oluşturucular ve veri kaynakları için kutucuklar][1]
>

## <a name="create-index"></a> Dizin oluşturma ve verileri yükleme

Arama sorguları aranabilir veriler, meta veriler ve arama davranışlarını iyileştiren ek yapılar içeren bir [*dizin*](search-what-is-an-index.md) kullanır.

Bu öğreticide **Verileri içeri aktar** sihirbazı aracılığıyla bir [*dizin oluşturucu*](search-indexer-overview.md) kullanılarak gezilebilen yerleşik bir örnek veri kümesi kullanacağız. Dizin oluşturucu, kaynağa özgü bir gezgindir ve desteklenen Azure veri kaynaklarındaki meta verileri ve içeriği okuyabilir. Bu dizin oluşturuculara portalın **Verileri içeri aktar** sihirbazından ulaşabilirsiniz. Daha sonra programlama ile dizin oluşturucuları bağımsız kaynaklar olarak oluşturabilir ve yönetebilirsiniz.

### <a name="step-1-start-the-import-data-wizard"></a>1. Adım: Verileri içeri aktarma sihirbazını başlatma

1. Sihirbazı başlatmak için Azure Search hizmet panosunun komut çubuğundan **Verileri içeri aktar**'a tıklayın. Bu sihirbaz, arama dizini oluşturma ve doldurma konusunda size yardımcı olur.

    ![Verileri içeri aktar komutu][2]

2. Sihirbazda **Verilerinize bağlanın** > **Örnekler** > **realestate-us-sample** yolunu izleyin. Bu veri kaynağı önceden bir ad, tür ve bağlantı bilgileriyle adlandırılır. Oluşturulan kaynak, diğer içeri aktarma işlemlerinde yeniden kullanılabilecek bir “mevcut veri kaynağı” olur.

    ![Örnek veri kümesi seçme][9]

3. Kullanmak için **Tamam**’a tıklayın.

### <a name="skip-cognitive-skills"></a>Bilişsel yetenekleri atlama

**Verileri içeri aktar** sihirbazı, dizin oluşturma aşamasına özel yapay zeka algoritmaları eklemenizi sağlayan isteğe bağlı bir bilişsel yetenekler adımı sunar. Şimdilik bu adımı atlayıp **Hedef dizini özelleştir** adımına geçin.

> [!TIP]
> Azure Search'teki yeni bilişsel arama özelliğini denemek isterseniz [bilişsel arama hızlı başlangıcına](cognitive-search-quickstart-blob.md) veya [öğreticisine](cognitive-search-tutorial-blob.md) göz atabilirsiniz.

   ![Bilişsel beceri adımını atlama][11]

### <a name="step-2-define-the-index"></a>2. Adım: Dizini tanımlama

Dizin oluşturma genelde kod ile gerçekleştirilen bir aşamadır. Bu öğreticide sihirbaz, gezinebileceği tüm veri kaynakları için dizin oluşturabilir. Dizin için en azından bir ad ve alan koleksiyonu gerekir ve her belgenin benzersiz olarak tanımlanabilmesi için bir alanın belge anahtarı olarak işaretlenmiş olması gerekir.

Alanların veri türleri ve öznitelikleri vardır. Üstteki onay kutuları, alanın nasıl kullanılacağını denetleyen *dizin öznitelikleridir*.

* **Alınabilir**, arama sonuçları listesinde çıktığı anlamına gelir. Örneğin, alanlar yalnızca filtre ifadelerinde kullanıldığında bu onay kutusunun işaretini kaldırarak alanları arama sonuçları için kapsam dışı olarak işaretleyebilirsiniz.
* **Filtrelenebilir**, **Sıralanabilir** ve **Modellenebilir** seçeneği bir alanın filtre, sıralama veya model gezinti yapısında kullanılıp kullanılamayacağını belirler.
* **Aranabilir**, bir alanın tam metin aramasına dahil olduğu anlamına gelir. Dizelerde arama yapılabilir. Sayısal alanlar ve Boolean alanları genellikle aranamaz olarak işaretlenir.

Varsayılan olarak sihirbaz tarafından anahtar alanının temeli olarak benzersiz tanımlayıcıların bulunması için veri kaynağı taranır. Dizelere alınabilir ve aranabilir öznitelikler atanmıştır. Tam sayılara alınabilir, filtrelenebilir, sıralanabilir ve modellenebilir öznitelikler atanmıştır.

  ![Emlak dizini oluşturuldu][3]

Dizini oluşturmak için **Tamam**’a tıklayın.

### <a name="step-3-define-the-indexer"></a>3. Adım: Dizin oluşturucuyu tanımlama

**Verileri içeri aktarma** sihirbazından çıkmadan **Dizin Oluşturucu** > **Ad**’a tıklayın ve dizin oluşturucu için bir ad yazın.

Bu nesne, yürütülebilir bir işlemi tanımlar. Bunu yinelenen bir zamanlamaya göre çalıştırabilirsiniz, ancak şimdilik dizin oluşturucunun **Tamam**’a tıkladığınızda bir kere çalışması için varsayılan seçeneği kullanın.  

  ![emlak dizini oluşturucu][8]

### <a name="check-progress"></a>İlerleme durumunu denetleme

Verilerin içeri aktarılmasını izlemek için hizmet panosuna dönün, sayfayı aşağı kaydırın ve **Dizin Oluşturucular** kutucuğuna tıklayarak dizin oluşturucuların listesini açın. Yeni oluşturulan ve durumu “sürüyor” ya da başarılı olan dizin oluşturucuyu ve dizine eklenen belge sayısını görebilirsiniz.

   ![Dizin oluşturucu ilerleme durumu iletisi][4]

### <a name="step-4-view-the-index"></a>4. Adım: Dizini görüntüleme

Hizmet panosundaki kutucuklar, kaynaklardaki çeşitli nesnelerle ilgili özet bilginin yanı sıra ayrıntılı bilgilere erişme imkanı sunar. **Dizinler** kutucuğu, bir önceki adımda oluşturduğunuz *realestate-us-sample* dahil olmak üzere var olan dizinleri listeler.

*realestate-us-sample* dizinine tıklayarak portal seçeneklerini görüntüleyebilirsiniz. **Alan Ekle/Düzenle** seçeneği, yeni alan oluşturmanızı ve özniteliklerini düzenlemenizi sağlar. Var olan alanlar Azure Search'te fiziksel olarak temsil edildiğinden kod ile dahi değişiklik yapılamaz. Var olan bir alanı tamamen değiştirmek için yeni bir tane oluşturun ve özgün alanı bırakın.

   ![Örnek dizin tanımı][10]

Puanlama profilleri ve CORS seçenekleri gibi diğer yapılar herhangi bir noktada eklenebilir.

Dizin tasarımı sırasında düzenleme yapabileceğiniz ve yapamayacağınız alanları kavramak için birkaç dakikanızı ayırarak dizin tanımı seçeneklerini görüntüleyin. Gri renkli seçenekler, bir değerin değiştirme veya silme işlemleri için uygun olmadığını gösterir. Benzer şekilde Çözümleyici ve Öneri aracı onay kutularını da şimdilik atlayabilirsiniz.

## <a name="query-index"></a> Dizini sorgulama

Artık yerleşik [**Arama gezgini**](search-explorer.md) sorgu sayfasını kullanarak sorgulayabileceğiniz bir arama dizinine sahipsiniz. Bu sayfada rastgele sorgu dizelerini test etmek için kullanabileceğiniz bir arama kutusu bulunur.

> [!TIP]
> [Azure Search’e Genel Bakış videosunu](https://channel9.msdn.com/Events/Connect/2016/138) 6 dakika 8 saniye ileri alarak aşağıdaki adımların gösterimini izleyebilirsiniz.
>

1. Komut çubuğunda **Arama gezgini**'ne tıklayın.

   ![Search gezgini komutu][5]

2. *realestate-us-sample* öğesine geçmek için komut çubuğundan **Dizini değiştir**’e tıklayın. Hangi REST API’lerin kullanılabildiğini görmek için komut çubuğundan **API sürümünü ayarla**’ya tıklayın. Aşağıdaki sorgular için genel kullanıma sunulan sürümü (2017-11-11) kullanın.

   ![Dizin ve API komutları][6]

3. Arama kutusuna aşağıdaki sorgu dizelerini girin ve **Ara**’ya tıklayın.

    > [!NOTE]
    > **Arama gezgini** yalnızca [REST API isteğini](https://docs.microsoft.com/rest/api/searchservice/search-documents) işleyebilecek şekilde tasarlanmıştır. Hem [basit sorgu söz dizimi](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) hem de [tam Lucene sorgu ayrıştırıcısına](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) yönelik söz dizimlerinin yanı sıra [Belgede Arama](https://docs.microsoft.com/rest/api/searchservice/search-documents) işlemlerinde kullanılabilen arama parametrelerini kabul eder.
    >

### <a name="simple-query-with-top-n-results"></a>İlk N sonucu içeren basit sorgu

#### <a name="example-string-searchseattle"></a>Örnek (dize): `search=seattle`

* Bu durumda tam metin aramak üzere anahtar sözcük arama girişi yapmak için **search** parametresi kullanılmıştır ve belgedeki aranabilir alanların herhangi birinde *Seattle* ifadesini içeren Washington eyaletinin King County bölgesindeki listelemeler döndürülmüştür.

* **Search gezgini** sonuçları JSON biçiminde döndürülür. Bu biçim ayrıntılı olmakla birlikte, belgelerin yoğun bir yapısı varsa okunması zordur. Bu kasıtlı olarak yapılmıştır. Özellikle test aşamasında belgenin tamamının görünür durumda olması geliştirme açısından önemlidir. Daha iyi bir kullanıcı deneyimi sunmak için [arama sonuçlarını işleyerek](search-pagination-page-layout.md) önemli öğeleri öne çıkaran bir kod yazmanız gerekir.

* Belgeler, dizinde "alınabilir" olarak işaretlenmiş tüm alanlardan oluşur. Portalda dizin özniteliklerini görüntülemek için **Dizinler** kutucuğundaki *realestate-us-sample* öğesine tıklayın.

#### <a name="example-parameterized-searchseattlecounttruetop100"></a>Örnek (parametreli): `search=seattle&$count=true&$top=100`

* **&** simgesi, herhangi bir sırada belirtilebilen arama parametreleri eklemek için kullanılır.

* **$count=true** parametresi, döndürülen tüm belgelerin toplam sayısını döndürür. Bu değer arama sonuçlarının en üstüne yakın bir konumda görünür. **$count=true** tarafından bildirilen değişiklikleri izleyerek filtre sorgularını doğrulayabilirsiniz. Daha küçük sayılar filtrenizin çalıştığını gösterir.

* **$top=100**, tüm belgelerin arasından en yüksek puana sahip 100 belgeyi döndürür. Azure Search varsayılan olarak ilk 50 en iyi eşleşmeyi döndürür. **$top** ile bu miktarı artırabilir veya azaltabilirsiniz.

### <a name="filter-query"></a> Sorguyu filtreleme

**$filter** parametresini eklediğinizde, arama isteklerine filtreler de eklenir. 

#### <a name="example-filtered-searchseattlefilterbeds-gt-3"></a>Örnek (filtrelenmiş): `search=seattle&$filter=beds gt 3`

* **$filter** parametresi, sağladığınız ölçütlerle eşleşen sonuçları döndürür. Bu durumda yatak odası sayısı 3’ten büyük olanlar.

* Filtre söz dizimi bir OData yapısıdır. Daha fazla bilgi edinmek için bkz. [OData söz dizimini filtreleme](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search).

### <a name="facet-query"></a> Sorguyu modelleme

Model filtreleri arama isteklerine dahil edilir. Sağladığınız model değeriyle eşleşen belgelerin toplam sayısını döndürmek için facet parametresini kullanabilirsiniz.

#### <a name="example-faceted-with-scope-reduction-searchfacetcitytop2"></a>Örnek (kapsamı azaltılarak modellenmiş): `search=*&facet=city&$top=2`

* **search=*** boş bir aramadır. Boş aramalar her şeyi arar. Boş sorgu göndermenin nedenlerinden biri, belge kümesinin tamamını filtrelemek veya görüntülemektir. Örneğin, dizindeki tüm şehirleri içeren bir gezinme yapısı görünümü oluşturmak isteyebilirsiniz.

* **facet**, bir kullanıcı arabirimi denetimine geçirebileceğiniz bir gezinti yapısı döndürür. Kategorileri ve bir sayımı döndürür. Bu durumda, kategoriler şehir sayısını temel alır. Azure Search'te toplama yoktur ancak her kategorideki belge sayısını veren `facet` ile tahmini bir toplama gerçekleştirebilirsiniz.

* **$top=2** iki belge getirir ve sonuçları azaltmak veya artırmak için `top` kullanabileceğinizi gösterir.

#### <a name="example-facet-on-numeric-values-searchseattlefacetbeds"></a>Örnek (sayısal değerlerle modelleme): `search=seattle&facet=beds`**

* Bu sorgu *Seattle* için yapılan metin aramasında yatakların görünümüdür. *beds* terimi alan dizinde getirilebilen, filtrelenebilen ve görünüm oluşturulabilen bir alan olarak işaretlendiğinden bir model olarak belirtilebilir ve içerdiği değerler (sayısal, 1'den 5'e kadar), listelemelerin gruplar (3 yatak odalı, 4 yatak odalı listelemeler) halinde kategorilere ayrılması için uygundur.

* Yalnızca filtrelenebilir alanlardan görünüm oluşturulabilir. Yalnızca getirilebilir alanlar sonuçlarda döndürülebilir.

### <a name="highlight-query"></a> Arama sonuçlarını vurgulama

İsabet vurgulama, belirli bir alanda eşleşme bulunduğunda anahtar sözcükle eşleşen metinlere biçimlendirme eklenmesini ifade eder. Arama teriminiz uzun bir açıklamanın belirsiz bir yerindeyse, terimi bulmayı kolaylaştırmak için isabet vurgulama ekleyebilirsiniz.

#### <a name="example-highlighter-searchgranite-countertopshighlightdescription"></a>Örnek (vurgulama): `search=granite countertops&highlight=description`

* Bu örnekte biçimlendirilmiş *granit mutfak tezgahları* tümceciği açıklama alanında daha kolay göze çarpar.

#### <a name="example-linguistic-analysis-searchmicehighlightdescription"></a>Örnek (dilbilimsel analiz): `search=mice&highlight=description`

* Tam metin arama, benzer semantiğe sahip sözcük biçimlerini bulur. Bu durumda, “sıçan” anahtar sözcüğüyle yapılan bir aramanın sonuçları, fare istilasına uğramış evler için “fare” sözcüğünün vurgulandığı metinleri içerir. Dilbilimsel analiz nedeniyle sonuçlarda aynı kelimenin farklı biçimleri görüntülenebilir.

* Azure Search, Lucene ve Microsoft’tan 56 çözümleyiciyi destekler. Azure Search tarafından standart olarak Lucene çözümleyici kullanılır.

### <a name="fuzzy-search"></a> Belirsiz aramayı deneme

Varsayılan olarak Seattle bölgesindeki Samammish platosu yerine *samamish* yazılması örneğindeki gibi yazım hatası yapılan sorgu terimleri için normal aramalarda bir eşleşme döndürülmez. Aşağıdaki örnek sonuç döndürmez.

#### <a name="example-misspelled-term-unhandled-searchsamamish"></a>Örnek (yanlış yazılmış terim, işlenmiyor): `search=samamish`

Yazım hatalarını işlemek için belirsiz aramayı kullanabilirsiniz. Belirsiz arama, tam Lucene sorgu söz dizimini kullandığınızda etkinleştirilir ve bunun için yapmanız gereken iki işlem vardır: sorguda **queryType=full** belirtin ve arama sorgusunun sonuna **~** ekleyin.

#### <a name="example-misspelled-term-handled-searchsamamishquerytypefull"></a>Örnek (yanlış yazılmış terim, işleniyor): `search=samamish~&queryType=full`

Bu örnekte ardık "Sammamish" ile ilgili belgeler döndürülür.

**queryType** belirtildiğinde varsayılan basit sorgu ayrıştırıcı kullanılır. Basit sorgu ayrıştırıcı daha hızlıdır ancak belirsiz arama, normal ifadeler, yakınlık araması ya da diğer gelişmiş sorgu türlerini kullanmanız gerekiyorsa tam söz dizimi gereklidir.

Belirsiz arama ve joker karakter araması, arama sonucunu etkiler. Bu sorgu biçimlerinde dilbilimsel analiz gerçekleştirilmez. Belirsiz arama ve joker karakter araması özelliklerini kullanmadan önce [Azure Search'te tam metin araması nasıl çalışır?](search-lucene-query-architecture.md#stage-2-lexical-analysis) sayfasına gidin ve sözcük analiziyle ilgili özel durumların anlatıldığı bölümü inceleyin.

Tam sorgu ayrıştırıcı tarafından etkinleştirilen sorgu senaryoları hakkında daha fazla bilgi edinmek için bkz. [Azure Search’te Lucene sorgu söz dizimi](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search).

### <a name="geo-search"></a> Jeo-uzamsal aramayı deneme

Koordinat içeren bir alanda [edm.GeographyPoint veri türü](https://docs.microsoft.com/rest/api/searchservice/supported-data-types) aracılığıyla jeo-uzamsal arama desteklenir. Coğrafi arama, [OData söz dizimini filtrele](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search) seçeneğinde belirtilen bir tür filtredir.

#### <a name="example-geo-coordinate-filters-searchcounttruefiltergeodistancelocationgeographypoint-122121513-47673988-le-5"></a>Örnek (coğrafi koordinat filtreleri): `search=*&$count=true&$filter=geo.distance(location,geography'POINT(-122.121513 47.673988)') le 5`

Örnek sorgu tüm sonuçları konumsal verilere göre filtreler ve belirli bir noktaya 5 kilometreden daha yakın olan sonuçlar (enlem ve boylam koordinatları olarak belirtilir) döndürülür. **$count** ekleyerek mesafeyi veya koordinatları değiştirdiğinizde döndürülen sonuç sayısını görebilirsiniz.

Arama uygulamanız "yakınımda bul" özelliği içeriyorsa ya da harita navigasyonu kullanıyorsa jeo-uzamsal arama kullanışlıdır. Ancak tam metin arama değildir. Bir şehir veya ülkede ada göre arama yapmak için kullanıcı gereksinimleriniz varsa koordinatlara ek olarak şehir veya bölge adlarını içeren alanlar ekleyin.

## <a name="takeaways"></a>Paketler

Bu öğreticide Azure portal ile Azure Search kullanımına hızlı bir giriş yaptınız.

**Verileri içeri aktar** sihirbazını kullanarak arama dizini oluşturmayı öğrendiniz. [Dizin oluşturucuların](search-indexer-overview.md) yanı sıra [yayımlanmış dizinler için desteklenen değişiklikler](https://docs.microsoft.com/rest/api/searchservice/update-index) gibi dizin tasarımıyla ilgili temel iş akışı hakkında bilgi edindiniz.

Azure portalda **Arama gezginini** kullanarak filtreler, sonuç vurgulama, belirsiz arama ve coğrafi arama gibi önemli özelliklerin gösterildiği örneklerle temel sorgu söz dizimini öğrendiniz.

Ayrıca portal panosundaki arama dizini, dizin oluşturucu ve veri kaynakları kutucuklarını kullanmayı öğrendiniz. Gelecekte yeni verilerle karşılaştığınızda portalı kullanarak tanımlarını veya alan koleksiyonlarını hızlı bir şekilde denetleyebilirsiniz.

## <a name="clean-up"></a>Temizleme

Azure Search hizmetini ilk kez bu öğreticide kullandıysanız Azure Search hizmetini içeren kaynak grubunu seçin. Değilse hizmet listesinden doğru kaynak grubunu bulun ve silin.

## <a name="next-steps"></a>Sonraki adımlar

Programlama araçlarını kullanarak Azure Search hakkında daha fazla şey keşfedebilirsiniz:

* [.NET SDK'sını kullanarak dizin oluşturma](https://docs.microsoft.com/azure/search/search-create-index-dotnet)
* [REST API'lerini kullanarak dizin oluşturma](https://docs.microsoft.com/azure/search/search-create-index-rest-api)
* [Azure Search REST API'lerini çağırmak için Postman veya Fiddler gibi web test araçlarını](search-fiddler.md) kullanma

<!--Image references-->
[1]: ./media/search-get-started-portal/tiles-indexers-datasources2.png
[2]: ./media/search-get-started-portal/import-data-cmd2.png
[3]: ./media/search-get-started-portal/realestateindex2.png
[4]: ./media/search-get-started-portal/indexers-inprogress2.png
[5]: ./media/search-get-started-portal/search-explorer-cmd2.png
[6]: ./media/search-get-started-portal/search-explorer-changeindex-se2.png
[7]: ./media/search-get-started-portal/search-explorer-query2.png
[8]: ./media/search-get-started-portal/realestate-indexer2.png
[9]: ./media/search-get-started-portal/import-datasource-sample2.png
[10]: ./media/search-get-started-portal/sample-index-def.png
[11]: ./media/search-get-started-portal/skip-cog-skill-step.png