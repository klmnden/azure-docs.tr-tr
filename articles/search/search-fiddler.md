---
title: "Azure Search REST API&quot;lerini değerlendirmek ve test etmek için Fiddler&quot;ı kullanma | Microsoft Belgeleri"
description: "Kod içermeyen bir yaklaşımla Azure Search hizmetinin kullanılabilirliğini doğrulamak ve REST API&quot;leri denemek için Fiddler&quot;ı kullanın."
services: search
documentationcenter: 
author: HeidiSteen
manager: mblythe
editor: 
ms.assetid: 790e5779-c6a3-4a07-9d1e-d6739e6b87d2
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 10/27/2016
ms.author: heidist
translationtype: Human Translation
ms.sourcegitcommit: fc2f30569acc49dd383ba230271989eca8a14423
ms.openlocfilehash: d78ceb2b616b5c574bede1e2df21c3415a9d757a

---

# <a name="use-fiddler-to-evaluate-and-test-azure-search-rest-apis"></a>Azure Search REST API'lerini değerlendirmek ve test etmek için Fiddler'ı kullanma
> [!div class="op_single_selector"]
>
> * [Genel Bakış](search-query-overview.md)
> * [Arama Gezgini](search-explorer.md)
> * [Fiddler](search-fiddler.md)
> * [.NET](search-query-dotnet.md)
> * [REST](search-query-rest-api.md)
>
>

Bu makale, [Telerik'ten ücretsiz indirme](http://www.telerik.com/fiddler) yoluyla erişilebilen Fiddler'ın, kod yazmaya gerek olmaksızın Azure Search REST API'sini kullanarak HTTP istekleri göndermek ve yanıtları görüntülemek için nasıl kullanıldığını açıklar. Azure Search; Microsoft Azure'da barındırılan, .NET ve REST API'ler aracılığıyla kolayca programlanabilen ve tamamen yönetilen bir bulut arama hizmetidir. Azure Search hizmeti REST API'leri [MSDN](https://msdn.microsoft.com/library/azure/dn798935.aspx)'de açıklanmaktadır.

Aşağıdaki adımlarda dizin oluşturur, belgeleri karşıya yükler, dizini sorgular ve ardından hizmet bilgisi için sistemi sorgularsınız.

Bu adımları tamamlamak için Azure Search hizmeti ve `api-key` gerekir. Kullanmaya başlama hakkında yönergeler için bkz. [Portalda Azure Search hizmeti oluşturma](search-create-service-portal.md).

## <a name="create-an-index"></a>Dizin oluşturma
1. Fiddler'ı başlatın. **Dosya** menüsünde, geçerli görevle ilişkisi olmayan yabancı HTTP etkinliğini gizlemek için **Trafiği Yakala**'yı devre dışı bırakın.
2. **Oluşturucu** sekmesinde, aşağıdaki ekran görüntüsü gibi görünen bir istek düzenleyin.

      ![][1]
3. **PUT**'u seçin.
4. Hizmet URL'sini, istek özniteliklerini ve api sürümünü belirten bir URL girin. Dikkate alınması gereken birkaç işaretçi:

   * HTTPS'yi önek olarak kullanın.
   * İstek özniteliği "indexes/hotels"dir. Bu, Search'e "oteller" adlı bir dizin oluşturmasını belirtir.
   * API sürümü küçük harfle yazılır, "?api-version=2016-09-01" olarak belirtilir. Azure Search düzenli olarak güncelleştirme dağıttığından, API sürümleri önemlidir. Nadir durumlarda, hizmet güncelleştirmesi API'de bozucu bir değişikliğe neden olabilir. Bu nedenle Azure Search, her bir istek için api sürümünü gerekli kılar. Böylece, hangisinin kullanıldığına ilişkin tam denetiminiz olur.

     Tam URL aşağıdaki örneğe benzemelidir.

             https://my-app.search.windows.net/indexes/hotels?api-version=2016-09-01
5. Ana bilgisayarı ve api anahtarını hizmetiniz için geçerli olan değerlerle değiştirerek istek üst bilgisini belirtin.

         User-Agent: Fiddler
         host: my-app.search.windows.net
         content-type: application/json
         api-key: 1111222233334444
6. İstek Gövdesine dizin tanımını oluşturan alanları yapıştırın.

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
7. **Yürüt**’e tıklayın.

Birkaç saniye içinde, dizinin başarıyla oluşturulduğunu belirten HTTP 201 yanıtını oturum listesinde görmeniz gerekir.

HTTP 504 yanıtı alırsanız HTTPS'yi belirten URL'yi doğrulayın. HTTP 400 veya 404 yanıtı görürseniz kopyala-yapıştır hatası olmadığını doğrulamak için istek gövdesini kontrol edin. HTTP 403 genelde api anahtarı ile ilgili bir sorunu gösterir (geçersiz anahtar veya api anahtarının nasıl belirtildiğine ilişkin söz dizimi sorunu).

## <a name="load-documents"></a>Belge yükleme
**Oluşturucu** sekmesinde, belgeleri gönderme isteğiniz aşağıdaki gibi görünür. İstek gövdesi 4 otel için arama verilerini içerir.

   ![][2]

1. **POST**'u seçin.
2. HTTPS ile başlayan, sırasıyla hizmet URL'niz ve "/indexes/<'indexname'>/docs/index?api-version=2016-09-01" ile devam eden bir URL girin. Tam URL aşağıdaki örneğe benzemelidir.

         https://my-app.search.windows.net/indexes/hotels/docs/index?api-version=2016-09-01
3. İstek Üst Bilgisi önceki ile aynı olmalıdır. Ana bilgisayarı ve api anahtarını hizmetiniz için geçerli olan değerlerle değiştirdiğinizi unutmayın.

         User-Agent: Fiddler
         host: my-app.search.windows.net
         content-type: application/json
         api-key: 1111222233334444
4. İstek Gövdesi, oteller dizinine eklenecek dört belge içerir.

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
5. **Yürüt**’e tıklayın.

Birkaç saniye içinde, oturum listesinde bir HTTP 200 yanıtı görmeniz gerekir. Bu, belgelerin başarıyla oluşturulduğunu belirtir. 207 yanıtı alırsanız en az bir belge karşıya yüklenemedi. 404 yanıtı alırsanız üst bilgi veya istek gövdesinde söz dizimi hatanız var.

## <a name="query-the-index"></a>Dizini sorgulama
Şimdi dizin ve belgeler karşıya yüklendiğine göre, bunlara sorgu gönderebilirsiniz.  **Oluşturucu** sekmesinde, hizmetinizi sorgulayan **GET** komutu aşağıdaki ekran görüntüsüne benzer görünür.

   ![][3]

1. **GET**'i seçin.
2. HTTPS ile başlayan, sırasıyla hizmet URL'niz, "/indexes/<'indexname'>/docs?" ve sorgu parametreleri ile devam eden bir URL girin. Örnek olarak, örnek ana bilgisayar adını hizmetiniz için geçerli olan bir tane ile değiştirerek aşağıdaki URL'yi kullanın.

         https://my-app.search.windows.net/indexes/hotels/docs?search=motel&facet=category&facet=rating,values:1|2|3|4|5&api-version=2016-09-01

   Bu sorgu "motel" terimini arar ve derecelendirmeler için model kategorileri alır.
3. İstek Üst Bilgisi önceki ile aynı olmalıdır. Ana bilgisayarı ve api anahtarını hizmetiniz için geçerli olan değerlerle değiştirdiğinizi unutmayın.

         User-Agent: Fiddler
         host: my-app.search.windows.net
         content-type: application/json
         api-key: 1111222233334444

Yanıt kodu 200 olmalıdır ve yanıt çıkışı aşağıdaki ekran görüntüsüne benzer görünmelidir.

   ![][4]

Aşağıdaki örnek sorgu MSDN'de [Search Dizin işlemi (Azure Search API)](http://msdn.microsoft.com/library/dn798927.aspx) içinde yer alır. Bu konu başlığındaki örnek sorguların çoğu, Fiddler'da izin verilmeyen boşluklar içerir. Sorgu dizesini yapıştırmadan ve Fiddler'da sorguyu denemeden önce, her bir boşluğu + karakteri ile değiştirin.

**Boşluklar değiştirilmeden önce:**

        GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate desc&api-version=2016-09-01

**Boşluklar + ile değiştirildikten sonra:**

        GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate+desc&api-version=2016-09-01

## <a name="query-the-system"></a>Sistemi sorgulama
Ayrıca, belge sayısını ve depolama tüketimini almak için sistemi sorgulayabilirsiniz. **Oluşturucu** sekmesinde isteğiniz aşağıdakine benzer görünür ve yanıt, belge sayısı ve kullanılan alan için bir sayı döndürür.

 ![][5]

1. **GET**'i seçin.
2. Hizmet URL'nizi içeren ve ardından "/indexes/hotels/stats?api-version=2016-09-01" ile devam eden bir URL girin:

         https://my-app.search.windows.net/indexes/hotels/stats?api-version=2016-09-01
3. Ana bilgisayarı ve api anahtarını hizmetiniz için geçerli olan değerlerle değiştirerek istek üst bilgisini belirtin.

         User-Agent: Fiddler
         host: my-app.search.windows.net
         content-type: application/json
         api-key: 1111222233334444
4. İstek gövdesini boş bırakın.
5. **Yürüt**'e tıklayın. Oturum listesinde HTTP 200 durum kodu görmeniz gerekir. Komutunuz için gönderilen girişi seçin.
6. **Denetçiler** sekmesine tıklayın, **Üst Bilgiler** sekmesine tıklayın ve ardından JSON biçimini seçin. Belge sayısını ve depolama boyutunu (KB) görmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar
Azure Search hizmetini kod içermeyen bir yaklaşımla yönetmek ve kullanmak için bkz. [Azure'da Search hizmetinizi yönetme](search-manage.md).

<!--Image References-->
[1]: ./media/search-fiddler/AzureSearch_Fiddler1_PutIndex.png
[2]: ./media/search-fiddler/AzureSearch_Fiddler2_PostDocs.png
[3]: ./media/search-fiddler/AzureSearch_Fiddler3_Query.png
[4]: ./media/search-fiddler/AzureSearch_Fiddler4_QueryResults.png
[5]: ./media/search-fiddler/AzureSearch_Fiddler5_QueryStats.png



<!--HONumber=Nov16_HO3-->


