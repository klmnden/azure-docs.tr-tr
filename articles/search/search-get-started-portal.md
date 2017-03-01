---
title: "Portalda ilk Azure Search dizininizi oluşturma | Microsoft Belgeleri"
description: "Azure portalında önceden tanımlanmış örnek verileri kullanarak bir dizin oluşturun. Tam metin arama, filtreler, modeller, belirsiz arama, coğrafi arama ve daha fazlasını keşfedin."
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
tags: azure-portal
ms.assetid: 21adc351-69bb-4a39-bc59-598c60c8f958
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.date: 02/15/2017
ms.author: heidist
translationtype: Human Translation
ms.sourcegitcommit: cb0843ec739d11e997794a8217c95696c4e78d23
ms.openlocfilehash: 70999d615038e7a5a11a623a9eef3e08c09f5eb9


---
# <a name="build-and-query-your-first-azure-search-index-in-the-portal"></a>Portalda ilk Azure Search dizininizi oluşturma ve sorgulama

Azure portalında **Verileri içeri aktar** sihirbazını kullanarak hızla bir dizin oluşturmak için önceden tanımlanmış bir örnek veri kümesiyle çalışmaya başlayın. **Search Gezgini** ile tam metin arama, filtreler, modeller, belirsiz arama ve coğrafi aramayı keşfedin.  

Bu kodsuz giriş yazısı, hemen ilginç sorgular yazmaya başlayabilmeniz için önceden tanımlanmış verileri kullanmaya başlamanızı sağlar. Portal araçları kodun yerini alamayacak olsa da aşağıdaki görevler için kullanışlıdır:

+ Olabildiğince az artışla uygulamalı eğitim
+ **Verileri içeri aktarma**’da kod yazmadan önce bir dizin prototipi oluşturma
+ **Search gezgini**’nde test sorguları ve ayrıştırıcı söz dizimi
+ Hizmetinize yayımlanmış mevcut bir dizini görüntüleyin ve dizinin özniteliklerini arayın

**Tahmini Süre:** Yaklaşık 15 dakika sürer, ancak hesap veya hizmete kaydolunması da gerekiyorsa daha uzun sürebilir. 

Alternatif olarak, bu [Azure Search’e Genel Bakış videosunun](https://channel9.msdn.com/Events/Connect/2016/138) üçüncü dakikasından izlemeye başlayarak bu öğreticideki adımların 6 dakikalık bir gösterimini izleyebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Bu öğretici, bir [Azure aboneliği](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) ve [Azure Search hizmeti](search-create-service-portal.md) kullanıldığını varsayar. 

## <a name="find-your-service"></a>Hizmetinizi bulma
1. [Azure portalında](https://portal.azure.com) oturum açın.
2. Azure Search hizmetinizin hizmet panosunu açın. Hizmet kutucuğunu panonuza sabitlemediyseniz hizmetinizi şu şekilde bulabilirsiniz: 
   
   * Atlama Çubuğu’nda soldaki gezinti bölmesinin en altında bulunan **Diğer hizmetler**’e tıklayın.
   * Aboneliğinizde kullanılabilen arama hizmetlerinin listesini görmek için arama kutusuna *arama* yazın. Hizmetiniz listede görünür. 

## <a name="check-for-space"></a>Alan denetleme
Birçok müşteri ücretsiz hizmetle başlar. Bu sürüm üç dizin, üç veri kaynağı ve üç dizin oluşturucu ile sınırlıdır. Başlamadan önce ek öğeler için yeriniz olduğundan emin olun. Bu öğreticide her nesneden birer tane oluşturulur. 

> [!TIP] 
> Hizmet panosundaki kutucuklar, şu anda kaç dizin, dizin oluşturucu ve veri kaynağına sahip olduğunuzu gösterir. Dizin oluşturucu kutucuğu, başarı ve başarısızlık göstergelerini gösterir. Dizin oluşturucu sayısını görüntülemek için kutucuğa tıklayın. 
>
> ![Dizin oluşturucular ve veri kaynakları için kutucuklar][1]
>

## <a name="a-namecreate-indexa-create-an-index-and-load-data"></a><a name="create-index"></a> Dizin oluşturma ve verileri yükleme
Arama sorguları, belirli arama davranışlarını iyileştirmek için kullanılan aranabilir verileri, meta verileri ve yapıları içeren bir *dizinde* yinelenir.

Bu görevin portal tabanlı kalmasını sağlamak için **Verileri içeri aktar** sihirbazı kullanılarak gezilebilen yerleşik bir örnek veri kümesi kullanıyoruz. 

#### <a name="step-1-start-the-import-data-wizard"></a>1. Adım: Verileri içeri aktarma sihirbazını başlatma
1. Azure Search hizmeti panonuzda, bir dizini hem oluşturan hem de dolduran bir sihirbazı başlatmak için komut çubuğundaki **Veri içeri aktar** seçeneğine tıklayın.
   
    ![Verileri içeri aktar komutu][2]

2. Sihirbazda **Veri Kaynağı** > **Örnekler** > **realestate-us-sample** seçeneğine tıklayın. Bu veri kaynağı önceden bir ad, tür ve bağlantı bilgileriyle adlandırılır. Oluşturulan kaynak, diğer içeri aktarma işlemlerinde yeniden kullanılabilecek bir “mevcut veri kaynağı” olur.

    ![Örnek veri kümesi seçme][9]

3. Kullanmak için **Tamam**’a tıklayın.

#### <a name="step-2-define-the-index"></a>2. Adım: Dizini tanımlama
Dizin oluşturma işlemi genellikle el ile veya kod tabanlı olarak gerçekleştirilir, ancak sihirbaz içinde gezinebileceği tüm veri kaynakları için dizin oluşturabilir. Dizin için en azından bir ad ve alan koleksiyonu gerekir ve her belgenin benzersiz olarak tanımlanabilmesi için bir alanın belge anahtarı olarak işaretlenmiş olması gerekir.

Alanların veri türleri ve öznitelikleri vardır. Üstteki onay kutuları, alanın nasıl kullanılacağını denetleyen *dizin öznitelikleridir*. 

* **Alınabilir**, arama sonuçları listesinde çıktığı anlamına gelir. Örneğin, alanlar yalnızca filtre ifadelerinde kullanıldığında bu onay kutusunun işaretini kaldırarak alanları arama sonuçları için kapsam dışı olarak işaretleyebilirsiniz. 
* **Filtrelenebilir**, **Sıralanabilir** ve **Modellenebilir** seçeneği bir alanın filtre, sıralama veya model gezinti yapısında kullanılıp kullanılamayacağını belirler. 
* **Aranabilir**, bir alanın tam metin aramasına dahil olduğu anlamına gelir. Dizelerde arama yapılabilir. Sayısal alanlar ve Boolean alanları genellikle aranamaz olarak işaretlenir. 

Varsayılan olarak sihirbaz tarafından anahtar alanının temeli olarak benzersiz tanımlayıcıların bulunması için veri kaynağı taranır. Dizelere alınabilir ve aranabilir öznitelikler atanmıştır. Tam sayılara alınabilir, filtrelenebilir, sıralanabilir ve modellenebilir öznitelikler atanmıştır.

  ![Emlak dizini oluşturuldu][3]

Dizini oluşturmak için **Tamam**’a tıklayın.

#### <a name="step-3-define-the-indexer"></a>3. Adım: Dizin oluşturucuyu tanımlama
**Verileri içeri aktarma** sihirbazından çıkmadan **Dizin Oluşturucu** > **Ad**’a tıklayın ve dizin oluşturucu için bir ad yazın. 

Bu nesne, yürütülebilir bir işlemi tanımlar. Bunu yinelenen bir zamanlamaya göre çalıştırabilirsiniz, ancak şimdilik dizin oluşturucunun **Tamam**’a tıkladığınızda bir kere çalışması için varsayılan seçeneği kullanın.  

  ![emlak dizini oluşturucu][8]

## <a name="check-progress"></a>İlerleme durumunu denetleme
Verilerin içeri aktarılmasını izlemek için hizmet panosuna dönün, sayfayı aşağı kaydırın ve **Dizin Oluşturucular** kutucuğuna tıklayarak dizin oluşturucuların listesini açın. Yeni oluşturulan ve durumu “sürüyor” ya da başarılı olan dizin oluşturucuyu ve dizine eklenen belge sayısını görebilirsiniz.

   ![Dizin oluşturucu ilerleme durumu iletisi][4]

## <a name="a-namequery-indexa-query-the-index"></a><a name="query-index"></a> Dizini sorgulama
Artık sorgulamaya hazır bir arama dizininiz var. **Arama gezgini**, portalda yerleşik bir sorgu aracıdır. Arama sonuçlarının beklediğiniz gibi olduğunu doğrulayabilmeniz için bir arama kutusu sağlar. 

   ![Search gezgini komutu][5]

> [!TIP]
> [Azure Search’e Genel Bakış videosunu](https://channel9.msdn.com/Events/Connect/2016/138) 6 dakika 8 saniye ileri alarak aşağıdaki adımların gösterimini izleyebilirsiniz.
>

1. Komut çubuğunda **Arama gezgini**'ne tıklayın.

2. *realestate-us-sample* öğesine geçmek için komut çubuğundan **Dizini değiştir**’e tıklayın.

   ![Dizin ve API komutları][6]

3. Hangi REST API’lerin kullanılabildiğini görmek için komut çubuğundan **API sürümünü ayarla**’ya tıklayın. Önizleme API’leri ile henüz genel kullanıma sunulmamış yeni özelliklere erişebilirsiniz. Yönlendirilmezseniz genel kullanıma sunulan sürümü (2016-09-01) kullanın. 

    > [!NOTE]
    > [Azure Search REST API](https://docs.microsoft.com/rest/api/searchservice/search-documents) ve [.NET kitaplığı](search-howto-dotnet-sdk.md#core-scenarios) birbirine tamamen eşdeğerdir, ancak **Arama gezgini** yalnızca REST çağrılarını işleyebilir. Hem [basit sorgu söz dizimi](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) hem de [tam Lucene sorgu ayrıştırıcısına](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) yönelik söz dizimlerinin yanı sıra [Belgede Arama](https://docs.microsoft.com/rest/api/searchservice/search-documents) işlemlerinde kullanılabilen arama parametrelerini kabul eder.
    > 
    > **Search gezgini** sonuçları JSON biçiminde döndürülür. Bu biçim ayrıntılı olmakla birlikte, belgelerin yoğun bir yapısı varsa okunması zordur. Belgelerinize bağlı olarak, önemli öğeleri ayrıştırmak amacıyla arama sonuçlarını işleyen kod yazmanız gerekebilir.

4. Arama kutusuna aşağıdaki sorgu dizelerini girin ve **Ara**’ya tıklayın.

  ![Arama sorgusu örneği][7]

**`search=seattle`** Bu durumda anahtar sözcük arama girişi yapmak için `search` parametresi kullanılmıştır ve belgedeki aranabilir alanların herhangi birinde Seattle ifadesini içeren Washington eyaletinin King County bölgesindeki listelemeler döndürülmüştür.

**`search=seattle&facet=beds`** `facet` parametresi, bir kullanıcı arabirimi denetimine geçirebileceğiniz bir gezinti yapısı döndürür. Kategorileri ve bir sayımı döndürür. Bu durumda, kategoriler yatak odası sayısını temel alır ve her birine yönelik belgelerin ya da eşleşmelerin sayımını içerir. `"beds"` dizinde filtrelenebilen ve modellenebilen bir alan olarak işaretlendiğinden bir model olarak belirtilebilir ve içerdiği değerler (sayısal, 1’den 5’e kadar), listelemelerin gruplar (3 yatak odalı, 4 yatak odalı listelemeler) halinde kategorilere ayrılması için uygundur.  Arama parametrelerini eklemek için `&` sembolü kullanılır.

**`search=seattle&filter=bed gt 3`** `filter` parametresi, sağladığınız ölçütlerle eşleşen sonuçları döndürür. Bu durumda yatak odası sayısı 3’ten büyük olanlar. Filtre söz dizimi bir OData yapısıdır. Daha fazla bilgi edinmek için bkz. [OData söz dizimini filtreleme](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search).

**`search=granite countertops&highlight=description`** İsabet vurguları, belirli bir alanda eşleşme bulunduğunda anahtar sözcükle eşleşen metinlere biçimlendirme ekler. Arama teriminiz uzun bir açıklamanın belirsiz bir yerindeyse, terimi bulmayı kolaylaştırmak için isabet vurgulama ekleyebilirsiniz. Bu durumda, açıklama alanında biçimlendirilen `"granite countertops"` ifadesini görmek daha kolaydır.

**`search=mice&highlight=description`** Tam metin arama, benzer semantiğe sahip sözcük biçimlerini bulur. Bu durumda, “sıçan” anahtar sözcüğüyle yapılan bir aramanın sonuçları, fare istilasına uğramış evler için “fare” sözcüğünün vurgulandığı metinleri içerir. Dilbilimsel analiz nedeniyle sonuçlarda aynı kelimenin farklı biçimleri görüntülenebilir. Azure Search, Lucene ve Microsoft’tan 56 çözümleyiciyi destekler. Azure Search tarafından standart olarak Lucene çözümleyici kullanılır. 

**`search=samamish`** Seattle bölgesindeki Samammish platosu için 'samamish' yazılması örneğindeki gibi yazım hatası yapılan normal aramalarda bir eşleşme döndürülmez. Yazım hatalarını işlemek için bir sonraki örnekte açıklanan belirsiz aramayı kullanabilirsiniz.

**`search=samamish~&queryType=full`** `~` sembolünü belirttiğinizde ve tam sorgu ayrıştırıcıyı kullandığınızda belirsiz arama etkinleştirilir ve `~` söz dizimi yorumlanıp doğru bir şekilde ayrıştırılır. Basit sorgu ayrıştırıcı daha hızlı olduğu için varsayılan olarak kullanılır, ancak belirsiz arama, normal ifadeler, yakınlık araması ya da diğer gelişmiş sorgu türlerini kullanmanız gerekiyorsa tam sorgu ayrıştırıcıyı tercih edebilirsiniz. Tam sorgu ayrıştırıcı tarafından etkinleştirilen sorgu senaryoları hakkında daha fazla bilgi edinmek için bkz. [Azure Search’te Lucene sorgu söz dizimi](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search).

**`search=*`** Boş aramalar her şeyi döndürür. Dizininizdeki toplam belge sayısını görmek ya da bir sonraki bölümde açıklandığı gibi tüm belgeler üzerinde filtreleme veya modelleme uygulamak istiyorsanız boş sorgu kullanabilirsiniz.

**`search=*&filter=geo.distance(location,geography'POINT(-122.13+47.64)')+le+10`** Koordinat içeren bir alanda [edm.GeographyPoint veri türü](https://docs.microsoft.com/rest/api/searchservice/supported-data-types) aracılığıyla jeo-uzamsal arama desteklenir. Bu sorgu tüm sonuçları konumsal verilere göre filtreler ve belirli bir noktaya 10 kilometreden daha yakın olan sonuçlar (enlem ve boylam koordinatları olarak belirtilir) döndürülür. Coğrafi arama, [OData söz dizimini filtrele](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search) seçeneğinde belirtilen bir tür filtredir. 

Normalde filtre ifadeleri bir `$` karakteriyle $filtre olarak belirtilir. Search Gezgini’nde `$` karakterini atmanız gerekir.

Arama uygulamanız ‘yakınımda bul’ özelliği içeriyorsa ya da harita navigasyonu kullanıyorsa jeo-uzamsal arama kullanışlıdır. Ancak tam metin arama değildir. Bir şehir veya ülkede ada göre arama yapmak için kullanıcı gereksinimleriniz varsa koordinatlara ek olarak şehir veya bölge adlarını içeren alanlar ekleyin.

## <a name="next-steps"></a>Sonraki adımlar

+ Az önce oluşturduğunuz nesnelerden dilediğinizi değiştirin. Sihirbazı bir kez çalıştırdıktan sonra, geri dönüp bileşenleri tek tek görüntüleyebilir veya değiştirebilirsiniz: dizin, dizin oluşturucu veya veri kaynağı. Alanın veri türünü değiştirme gibi bazı düzenlemelere dizinde izin verilmez ancak çoğu özellik ve ayar değiştirilebilir.

  Bileşenlerin tek tek görüntülemek için panonuzda **Dizin**, **Dizin Oluşturucu** veya **Veri Kaynakları** kutucuğuna tıklayarak var olan nesnelerin bir listesini görüntüleyin. Yeniden derleme gerektirmeyen dizin düzenleme işlemleri hakkında daha fazla bilgi edinmek için bkz. [Dizini Güncelleştirme (Azure Search REST API’si)](https://docs.microsoft.com/rest/api/searchservice/update-index).

+ Araçları ve adımları diğer veri kaynaklarıyla kullanın. `realestate-us-sample` örnek veri kümesi, Azure Search’ün içinde gezinebileceği bir Azure SQL Veritabanı’ndan alınmıştır. Azure Search, Azure SQL Veritabanı’nın yanı sıra Azure Tablo depolama, Blob depolama, bir Azure sanal makinesindeki SQL Server ve DocumentDB’de gezinebilir. Sihirbazda bu veri kaynaklarının tamamı desteklenir. Kodda bir *dizin oluşturucu* kullanarak bir dizini kolayca oluşturup doldurabilirsiniz.

+ Diğer tüm veri kaynakları bir gönderme modeli aracılığıyla desteklenir. Bu modelde, yeni ve değiştirilmiş satır kümeleri kodunuz tarafından JSON biçiminde dizininize gönderir. Daha fazla bilgi edinmek için bkz. [Azure Search’te belge ekleme, güncelleştirme veya silme](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents).

Aşağıdaki bağlantıları ziyaret ederek burada bahsedilen diğer özellikler hakkında daha fazla bilgi edinin:

* [Dizin oluşturuculara genel bakış](search-indexer-overview.md)
* [Dizin Oluşturma (dizin özniteliklerine yönelik ayrıntılı bir açıklama içerir)](https://docs.microsoft.com/rest/api/searchservice/create-index)
* [Arama Gezgini](search-explorer.md)
* [Search Belgeleri (sorgu söz dizimi örneklerini içerir)](https://docs.microsoft.com/rest/api/searchservice/search-documents)


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


<!--HONumber=Feb17_HO3-->


