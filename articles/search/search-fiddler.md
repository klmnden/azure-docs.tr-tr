---
title: Fiddler veya Postman içindeki REST API'lerini keşfetme (Azure Search REST) | Microsoft Docs
description: Azure Search için HTTP istekleri ve REST API çağrıları oluşturmak için Fiddler veya Postman kullanma.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: quickstart
ms.date: 04/20/2018
ms.author: heidist
ms.openlocfilehash: eba41086da645c2ff5cee65f9395267227cb1c11
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
ms.locfileid: "32190194"
---
# <a name="explore-azure-search-rest-apis-using-fiddler-or-postman"></a>Fiddler veya Postman kullanarak Azure Search REST API'lerini keşfetme

[Azure Search REST API](https://docs.microsoft.com/rest/api/searchservice)'lerini keşfetmenin en kolay yöntemlerinden biri Fiddler veya Postman kullanarak HTTP isteklerini düzenlemek ve yanıtları incelemektir. Doğru araçlar ve bu yönergelerden yararlanarak herhangi bir kod yazmadan önce istek gönderebilir ve yanıtları görüntüleyebilirsiniz.

> [!div class="checklist"]
> * Web API'si test aracı indirme
> * Arama hizmetiniz için API anahtarı ve uç noktası alma
> * İstek üst bilgilerini yapılandırma
> * Dizin oluşturma
> * Dizin yükleme
> * Dizin arama

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun ve [Azure Search hizmetine kaydolun](search-create-service-portal.md).

## <a name="download-and-install-tools"></a>Araçları indirme ve yükleme

Aşağıdaki araçlar web içeriği geliştirmek için yaygın bir şekilde kullanılmaktadır ancak başka bir araç hakkında deneyiminiz varsa bu makaledeki talimatlar o araç için de geçerli olacaktır.

+ [Postman masaüstü uygulaması](https://www.getpostman.com/)
+ [Telerik Fiddler](http://www.telerik.com/fiddler)

## <a name="get-the-api-key-and-endpoint"></a>API anahtarını ve uç noktasını alma

REST çağrıları için her istekte hizmet URL'sinin ve bir erişim anahtarının iletilmesi gerekir. İkisini de içeren bir arama hizmeti oluşturulur. Bu nedenle aboneliğinize Azure Search hizmetini eklediyseniz gerekli bilgileri almak için aşağıdaki adımları izleyin:

1. Azure portalında panodan arama hizmeti sayfasını açın veya hizmet listesinden [hizmetinizi bulun](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices).
2. Uç noktayı **Genel Bakış** > **Temel Bileşenler** > **URL** yolundan bulabilirsiniz. Örnek uç nokta `https://my-service-name.search.windows.net` şeklinde görünebilir.
3. API anahtarını **Ayarlar** > **Anahtarlar** yolundan alın. Birlikte kullanabilmeniz için iki yönetici anahtarı mevcuttur. Yönetici anahtarları hizmetinize dizin oluşturma ve yükleme için gerekli olan yazma izinlerini verir. Yazma işlemleri için birincil veya ikincil anahtarı kullanabilirsiniz.

## <a name="configure-request-headers"></a>İstek üst bilgilerini yapılandırma

Her araç oturum için istek üst bilgisini içerir. Bu nedenle URL uç noktasını, API sürümünü, API anahtarını ve içerik türünü yalnızca bir kez girmeniz gerekir.

Tam URL aşağıdaki örneğe benzer olmalıdır. Sizinkinde farklı olarak **`my-app`** yer tutucusunun adı için geçerli bir değer bulunmalıdır: `https://my-app.search.windows.net/indexes/hotels?api-version=2017-11-11`

Hizmet URL'si birleşimi aşağıdaki öğeleri içerir:

+ HTTPS ön eki.
+ Portaldan alınan Hizmet URL'si.
+ Hizmetinizde nesne oluşturan bir işlem olan kaynak. Bu adımda hotels adlı bir dizin kullanılmıştır.
+ API sürümü, geçerli sürüm için "?api-version=2017-11-11" olarak belirtilen küçük harfli bir dize. [API sürümleri](search-api-versions.md) düzenli olarak güncelleştirilir. api-version parametresini her isteğe dahil etmeniz hangisinin kullanıldığıyla ilgili tam denetim sahibi olmanızı sağlar.  

İstek üst bilgisi önceki bölümde açıklanan içerik türü ve API anahtarı olmak üzere iki öğeyi içerir:

         content-type: application/json
         api-key: <placeholder>

### <a name="fiddler"></a>Fiddler

Aşağıdaki ekran görüntüsü gibi görünen bir istek düzenleyin. **PUT** fiilini seçin. Fiddler `User-Agent=Fiddler` ekler. Altındaki yeni satırlara iki ek istek üst bilgisi yapıştırabilirsiniz. Hizmetinize ait yönetici erişim anahtarını kullanarak hizmetinizin içerik türünü ve API anahtarını dahil edin.

![Fiddler isteği üst bilgisi][1]

> [!Tip]
> Gerçekleştirdiğiniz görevlerle ilgili olmayan ek HTTP etkinliğini gizlemek için web trafiğini kapatabilirsiniz. Fiddler'da **File** (Dosya) menüsüne gidin ve **Capture Traffic** (Trafiği Yakala) seçeneğini kapatın. 

### <a name="postman"></a>Postman

Aşağıdaki ekran görüntüsü gibi görünen bir istek düzenleyin. **PUT** fiilini seçin. 

![Postman isteği üst bilgisi][6]

## <a name="create-the-index"></a>Dizini oluşturma

İstek gövdesi dizin tanımını içerir. İstek gövdesini eklediğinizde dizininizi oluşturan istek tamamlanır.

İstek içinde dizin adından sonraki en önemli bileşen, alanlar koleksiyonudur. Alanlar koleksiyonu dizin şemasını tanımlar. Her alanın türünü belirtin. Dize alanları tam metin araması için kullanılır. Bu nedenle içerikte arama yapılabilmesini istiyorsanız sayısal verileri dize olarak ayarlamak isteyebilirsiniz.

Alan öznitelikleri izin verilen eylemi belirler. REST API'leri varsayılan olarak birçok eyleme izin verir. Örneğin tüm dizelerde arama, getirme, filtreleme ve modelleme özellikleri varsayılan olarak etkindir. Genellikle öznitelikleri yalnızca bir davranışı kapatmak istediğinizde ayarlamanız gerekir. Öznitelikler hakkında daha fazla bilgi için bkz. [Dizin Oluşturma (REST)](https://docs.microsoft.com/rest/api/searchservice/create-index).

          {
         "name": "hotels",  
         "fields": [
           {"name": "hotelId", "type": "Edm.String", "key":true, "searchable": false},
           {"name": "baseRate", "type": "Edm.Double"},
           {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
           {"name": "hotelName", "type": "Edm.String"},
           {"name": "category", "type": "Edm.String"},
           {"name": "tags", "type": "Collection(Edm.String)"},
           {"name": "parkingIncluded", "type": "Edm.Boolean"},
           {"name": "smokingAllowed", "type": "Edm.Boolean"},
           {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
           {"name": "rating", "type": "Edm.Int32"},
           {"name": "location", "type": "Edm.GeographyPoint"}
          ]
         }


Bu isteği gönderdiğinizde dizinin başarıyla oluşturulduğunu belirten HTTP 201 yanıtı almanız gerekir. Bu eylemi portaldan doğrulayabilirsiniz ancak portal sayfasındaki yenileme aralıkları nedeniyle bilgilerin güncellenmesi bir-iki dakika sürebilir.

HTTP 504 yanıtı alırsanız HTTPS'yi belirten URL'yi doğrulayın. HTTP 400 veya 404 yanıtı görürseniz kopyala-yapıştır hatası olmadığını doğrulamak için istek gövdesini kontrol edin. HTTP 403 genelde api anahtarı ile ilgili bir sorunu gösterir (geçersiz anahtar veya api anahtarının nasıl belirtildiğine ilişkin söz dizimi sorunu).

### <a name="fiddler"></a>Fiddler

Aşağıdaki ekran görüntüsünde gösterildiği şekilde dizin tanımını istek gövdesine kopyalayın ve ardından sağ üst köşedeki **Execute** (Yürüt) öğesine tıklayarak tamamlanan isteği gönderin.

![Fiddler isteği gövdesi][7]

### <a name="postman"></a>Postman

Aşağıdaki ekran görüntüsünde gösterildiği şekilde dizin tanımını istek gövdesine kopyalayın ve ardından sağ üst köşedeki **Send** (Gönder) öğesine tıklayarak tamamlanan isteği gönderin.

![Postman isteği gövdesi][8]

## <a name="load-documents"></a>Belge yükleme

Dizini oluşturma ve dizini doldurma ayrı adımlardır. Azure Search'te dizin, arama yapılabilecek ve JSON belgeleri olarak iletebileceğiniz tüm verileri içerir. Bu işlemde kullanılacak API'yi gözden geçirmek için bkz. [Belge ekleme, güncelleştirme veya silme (REST)](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents).

+ Bu adım için fiili **POST** olarak değiştirin.
+ Uç noktayı `/docs/index` içerecek şekilde değiştirin. Tam URL `https://my-app.search.windows.net/indexes/hotels/docs/index?api-version=2017-11-11` şeklinde görünmelidir
+ İstek üst bilgilerini değiştirmeyin. 

İstek Gövdesi, oteller dizinine eklenecek dört belge içerir.

         {
         "value": [
         {
             "@search.action": "upload",
             "hotelId": "1",
             "baseRate": 199.0,
             "description": "Best hotel in town",
             "hotelName": "Fancy Stay",
             "category": "Luxury",
             "tags": ["pool", "view", "wifi", "concierge"],
             "parkingIncluded": false,
             "smokingAllowed": false,
             "lastRenovationDate": "2010-06-27T00:00:00Z",
             "rating": 5,
             "location": { "type": "Point", "coordinates": [-122.131577, 47.678581] }
           },
           {
             "@search.action": "upload",
             "hotelId": "2",
             "baseRate": 79.99,
             "description": "Cheapest hotel in town",
             "hotelName": "Roach Motel",
             "category": "Budget",
             "tags": ["motel", "budget"],
             "parkingIncluded": true,
             "smokingAllowed": true,
             "lastRenovationDate": "1982-04-28T00:00:00Z",
             "rating": 1,
             "location": { "type": "Point", "coordinates": [-122.131577, 49.678581] }
           },
           {
             "@search.action": "upload",
             "hotelId": "3",
             "baseRate": 279.99,
             "description": "Surprisingly expensive",
             "hotelName": "Dew Drop Inn",
             "category": "Bed and Breakfast",
             "tags": ["charming", "quaint"],
             "parkingIncluded": true,
             "smokingAllowed": false,
             "lastRenovationDate": null,
             "rating": 4,
             "location": { "type": "Point", "coordinates": [-122.33207, 47.60621] }
           },
           {
             "@search.action": "upload",
             "hotelId": "4",
             "baseRate": 220.00,
             "description": "This could be the one",
             "hotelName": "A Hotel for Everyone",
             "category": "Basic hotel",
             "tags": ["pool", "wifi"],
             "parkingIncluded": true,
             "smokingAllowed": false,
             "lastRenovationDate": null,
             "rating": 4,
             "location": { "type": "Point", "coordinates": [-122.12151, 47.67399] }
           }
          ]
         }

Birkaç saniye içinde, oturum listesinde bir HTTP 200 yanıtı görmeniz gerekir. Bu, belgelerin başarıyla oluşturulduğunu belirtir. 

207 yanıtı alırsanız en az bir belge karşıya yüklenemedi. 404 yanıtı alırsanız üst bilgi veya istek gövdesinde söz dizimi hatanız vardır. Uç noktayı `/docs/index` içerecek şekilde değiştirdiğinizden emin olun.

> [!Tip]
> Seçili veri kaynaklarında dizin oluşturma için gerekli kodları sadeleştiren ve miktarını azaltan alternatif *dizin oluşturucu* yaklaşımını kullanabilirsiniz. Daha fazla bilgi için bkz. [Dizin oluşturucu işlemleri](https://docs.microsoft.com/rest/api/searchservice/indexer-operations).

### <a name="fiddler"></a>Fiddler

Fiili **POST** olarak değiştirin. URL'yi `/docs/index` içerecek şekilde değiştirin. Aşağıdaki ekran görüntüsünde gösterilen şekilde belgeleri istek gövdesine kopyalayın ve isteği yürütün.

![Fiddler isteği yükü][9]

### <a name="postman"></a>Postman

Fiili **POST** olarak değiştirin. URL'yi `/docs/index` içerecek şekilde değiştirin. Aşağıdaki ekran görüntüsünde gösterilen şekilde belgeleri istek gövdesine kopyalayın ve isteği yürütün.

![Postman isteği yükü][10]

## <a name="query-the-index"></a>Dizini sorgulama
Şimdi dizin ve belgeler karşıya yüklendiğine göre, bunlara sorgu gönderebilirsiniz. Bu API hakkında daha fazla bilgi için bkz. [Arama Belgeleri (REST)](https://docs.microsoft.com/rest/api/searchservice/search-documents)  

+ Bu adım için fiili **GET** olarak değiştirin.
+ Uç noktayı arama dizeleri dahil olmak üzere sorgu parametrelerini içerecek şekilde değiştirin. Sorgu URL'si `https://my-app.search.windows.net/indexes/hotels/docs?search=motel&$count=true&api-version=2017-11-11` gibi görünmelidir
+ İstek üst bilgilerini değiştirmeyin

Bu sorgu "motel" terimini arar ve arama sonuçlarındaki belgelerin sayısını döndürür. **Send** (Gönder) öğesine tıkladığınızda görünen istek ve yanıt aşağıdaki Postman ekran görüntüsüne benzer olmalıdır. Durum kodu 200 olmalıdır.

 ![Postman sorgusu yanıtı][11]

### <a name="tips-for-running-our-sample-queries-in-fiddler"></a>Örnek sorgularımızı Fiddler'da çalıştırma ipuçları

Aşağıdaki örnek sorgu MSDN'de [Search Dizin işlemi (Azure Search API)](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) makalesinde yer alır. Bu makaledeki örnek sorguların çoğu, Fiddler'da izin verilmeyen boşluklar içerir. Sorgu dizesini yapıştırmadan ve Fiddler'da sorguyu denemeden önce, her bir boşluğu + karakteri ile değiştirin.

**Boşluklar değiştirilmeden önce (in lastRenovationDate desc):**

        GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate desc&api-version=2017-11-11

**Boşluklar + ile değiştirildikten sonra (in lastRenovationDate+desc):**

        GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate+desc&api-version=2017-11-11

## <a name="query-index-properties"></a>Sorgu dizini özellikleri
Ayrıca, belge sayısını ve depolama tüketimini almak için sistem bilgilerini sorgulayabilirsiniz: `https://my-app.search.windows.net/indexes/hotels/stats?api-version=2017-11-11`

Postman uygulamasında isteğinizin aşağıdakine benzer olması ve yanıtta belge sayısı ile kullanılan alanın bayt cinsinden değerinin belirtilmesi gerekir.

 ![Postman sistem sorgusu][12]

api-version söz diziminin farklı olduğuna dikkat edin. Bu istek için api-version parametresine `?` ekleyin. ? işareti URL yolunu sorgu dizesinden ayırırken & işareti sorgu dizesindeki "ad=değer" çiftlerini ayırır. Bu sorgu için api-version, sorgu dizesindeki ilk ve tek öğedir.

Bu API hakkında daha fazla bilgi için bkz. [Dizin İstatistiklerini Alma (REST)](https://docs.microsoft.com/rest/api/searchservice/get-index-statistics).


### <a name="tips-for-viewing-index-statistic-in-fiddler"></a>Dizin istatistiklerini Fiddler'da görüntülemek için ipuçları

Fiddler'da **Inspectors** (Denetçiler) sekmesine tıklayın, **Headers** (Üst Bilgiler) sekmesine tıklayın ve ardından JSON biçimini seçin. Belge sayısını ve depolama boyutunu (KB) görmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

REST istemcileri plansız keşifler için çok önemlidir. REST API'lerinin nasıl çalıştığını öğrendiğinizde göre kod kullanarak ilerleyebilirsiniz. Sonraki adımları için aşağıdaki bağlantılara bakın:

+ [Dizin oluşturma (REST)](search-create-index-rest-api.md)
+ [Veri içeri aktarma (REST)](search-import-data-rest-api.md)
+ [Dizin arama (REST)](search-query-rest-api.md)

<!--Image References-->
[1]: ./media/search-fiddler/fiddler-url.png
[2]: ./media/search-fiddler/AzureSearch_Fiddler2_PostDocs.png
[3]: ./media/search-fiddler/AzureSearch_Fiddler3_Query.png
[4]: ./media/search-fiddler/AzureSearch_Fiddler4_QueryResults.png
[5]: ./media/search-fiddler/AzureSearch_Fiddler5_QueryStats.png
[6]: ./media/search-fiddler/postman-url.png
[7]: ./media/search-fiddler/fiddler-request.png
[8]: ./media/search-fiddler/postman-request.png
[9]: ./media/search-fiddler/fiddler-docs.png
[10]: ./media/search-fiddler/postman-docs.png
[11]: ./media/search-fiddler/postman-query.png
[12]: ./media/search-fiddler/postman-system-query.png