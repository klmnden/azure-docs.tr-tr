<properties
    pageTitle="REST API kullanarak Azure Search Dizininizi sorgulama | Microsoft Azure | Barındırılan bulut arama hizmeti"
    description="Azure Search'te bir arama sorgusu oluşturun ve arama sonuçlarını filtrelemek ve sıralamak için arama parametrelerini kullanın."
    services="search"
    documentationCenter=""
    authors="ashmaka"
/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>


# REST API kullanarak Azure Search dizininizi sorgulama
> [AZURE.SELECTOR]
- [Genel Bakış](search-query-overview.md)
- [Portal](search-explorer.md)
- [.NET](search-query-dotnet.md)
- [REST](search-query-rest-api.md)

Bu makale, [Azure Search REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx)'sini kullanarak bir dizinin nasıl sorgulanacağını gösterir.

Bu kılavuzda başlamadan önce, [Azure Search dizini oluşturmuş](search-what-is-an-index.md) ve [bunu verilerle doldurmuş](search-what-is-data-import.md) olmanız gerekir.

## I. Azure Search hizmet sorgunuzun api anahtarını tanımlama
Azure Search REST API'sine karşı tüm arama işlemlerinin önemli bir bileşeni, sağladığınız hizmet için oluşturulan *api anahtarıdır*. İstek başına geçerli bir anahtara sahip olmak, isteği gönderen uygulama ve bunu işleyen hizmet arasında güven oluşturur.

1. Hizmetinizin api anahtarlarını bulmak için [Azure Portal](https://portal.azure.com/)'da oturum açmanız gerekir
2. Azure Search hizmetinizin dikey penceresine gidin
3. "Anahtarlar" simgesine tıklayın

Hizmetiniz, *yönetici anahtarlarına* ve *sorgu anahtarlarına* sahiptir.

 - Birincil ve ikincil *yönetici anahtarlarınız*; hizmeti yönetme, dizinler, dizin oluşturucular ve veri kaynakları ekleme ve silme de dahil olmak üzere her türlü işlem için tüm hakları verir. Birincil anahtarı yeniden oluşturmaya karar verirseniz ikincil anahtarı kullanmaya devam edebilmeniz ve tam tersini yapabilmeniz için iki anahtar vardır.
 - *Sorgu anahtarları*, dizinler ve belgeler için salt okunur erişim verir ve genellikle, arama istekleri gönderen istemci uygulamalarına dağıtılır.

Bir dizini sorgulama amacıyla, sorgu anahtarlarınızdan birini kullanabilirsiniz. Yönetici anahtarlarınız da sorgular için kullanılabilir ancak uygulama kodunuzda bir sorgu anahtarı kullanmanız gerekir. Böylece [En az ayrıcalık prensibi](https://en.wikipedia.org/wiki/Principle_of_least_privilege) daha iyi takip edilmiş olur.

## II. Sorgunuzu düzenleme
[REST API kullanarak dizininizi aramanın](https://msdn.microsoft.com/library/azure/dn798927.aspx) iki yolu bulunur. Bu yollardan biri, sorgu parametrelerinizin istek gövdesindeki bir JSON nesnesinde tanımlanacağı bir HTTP POST isteği göndermektir. Diğer yol ise sorgu parametrelerinizin istek URL'si içinde tanımlanacağı bir HTTP GET isteği göndermektir. POST'un sorgu parametrelerinin boyutu açısından GET'ten daha [esnek sınırlara](https://msdn.microsoft.com/library/azure/dn798927.aspx) sahip olduğuna dikkat edin. Bu nedenle, GET'i kullanmanın daha kullanışlı olduğu özel durumlar olmadığı sürece POST kullanmanızı öneririz.

POST ve GET için *hizmet adınızı*, *dizin adını* ve uygun *API sürümünü* (bu belgenin yayımlandığı sırada geçerli API sürümü `2015-02-28`) istek URL'sinde sağlamanız gerekir. GET için sorgu parametrelerini URL'nin sonundaki *sorgu dizesine* sağlarsınız. URL biçimi için aşağıya bakın:

    https://[service name].search.windows.net/indexes/[index name]/docs?[query string]&api-version=2015-02-28

POST için biçim aynıdır ancak sorgu dizesi parametrelerinde yalnızca api sürümü olur.



#### Örnek Sorgular

Burada "hotels" adlı bir dizinde birkaç örnek sorgu verilmiştir. Bu sorgular, hem GET hem de POST biçiminde gösterilir.

Tüm dizinde "budget" terimi araması yapın ve yalnızca `hotelName` alanını döndürün:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=budget&$select=hotelName&api-version=2015-02-28

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2015-02-28
{
    "search": "budget",
    "select": "hotelName"
}
```

Geceliği 150 dolardan ucuz oteller bulmak için dizinde bir filtre uygulayıp `hotelId` ve `description` sonuçlarını döndürün:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=*&$filter=baseRate lt 150&$select=hotelId,description&api-version=2015-02-28

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2015-02-28
{
    "search": "*",
    "filter": "baseRate lt 150",
    "select": "hotelId,description"
}
```

Tüm dizinde arama yapın, belirli bir alana göre (`lastRenovationDate`) azalan sıralamada sıralama yapın, ilk iki sonucu alın ve yalnızca `hotelName` ve `lastRenovationDate` öğesinin gösterilmesini sağlayın:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=*&$top=2&$orderby=lastRenovationDate desc&$select=hotelName,lastRenovationDate&api-version=2015-02-28

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2015-02-28
{
    "search": "*",
    "orderby": "lastRenovationDate desc",
    "select": "hotelName,lastRenovationDate",
    "top": 2
}
```

## III. HTTP isteğinizi gönderme
Artık HTTP istek URL'nizin (GET için) veya gövdenizin (POST için) parçası olarak sorgunuzu düzenlediğinize göre, istek üst bilgilerinizi tanımlayıp sorgunuzu gönderebilirsiniz.

#### İstek ve İstek Üst Bilgileri
GET için iki, POST için ise üç istek üst bilgisi tanımlamanız gerekir:
1. `api-key` üst bilgisi, yukarıdaki 1. adımda bulduğunuz sorgu anahtarına ayarlanmalıdır. `api-key` üst bilgisi olarak bir yönetici anahtarı da kullanabileceğinizi unutmayın ancak dizinlere ve belgelere açık bir şekilde salt okunur erişimi verdiğinden, bir sorgu anahtarı kullanmanızı öneririz.
2. `Accept` üst bilgisi `application/json` şeklinde ayarlanmalıdır.
3. Yalnızca POST için `Content-Type` üst bilgisi `application/json` olarak ayarlanmalıdır.

"Motel" terimini arayan basit bir sorgu kullanarak Azure Search REST API'sini kullanıp "hotels" dizininde arama yapmaya yönelik bir HTTP GET isteği için aşağıya bakın:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=motel&api-version=2015-02-28
Accept: application/json
api-key: [query key]
```

Aşağıda bu sefer HTTP POST kullanılmış şekilde aynı örnek sorgu verilmiştir:

```
POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2015-02-28
Content-Type: application/json
Accept: application/json
api-key: [query key]

{
    "search": "motel"
}
```

Başarılı bir sorgu isteği, `200 OK` Durum Koduna sonucunu verir ve arama sonuçları, yanıt gövdesinde JSON olarak döndürülür. Yukarıdaki sorgunun sonuçları, "hotels" dizininin [REST API kullanarak Azure Search'te Veri İçeri Aktarma](search-import-data-rest-api.md)'da verilen örnek verilerle doldurulduğu varsayıldığında şöyledir (JSON'un netlik sağlaması için biçimlendirildiğini unutmayın).

```JSON
{
    "value": [
        {
            "@search.score": 0.59600675,
            "hotelId": "2",
            "baseRate": 79.99,
            "description": "Cheapest hotel in town",
            "description_fr": "Hôtel le moins cher en ville",
            "hotelName": "Roach Motel",
            "category": "Budget",
            "tags":["motel", "budget"],
            "parkingIncluded": true,
            "smokingAllowed": true,
            "lastRenovationDate": "1982-04-28T00:00:00Z",
            "rating": 1,
            "location": {
                "type": "Point",
                "coordinates": [-122.131577, 49.678581],
                "crs": {
                    "type":"name",
                    "properties": {
                        "name": "EPSG:4326"
                    }
                }
            }
        }
    ]
}
```

Daha fazla bilgi edinmek için lütfen [Search Belgeleri](https://msdn.microsoft.com/library/azure/dn798927.aspx)'nin "Yanıt" bölümünü ziyaret edin. Hata durumunda döndürülebilen diğer HTTP durum kodları hakkında daha fazla bilgi için bkz. [HTTP durum kodları (Azure Search)](https://msdn.microsoft.com/library/azure/dn798925.aspx).



<!--HONumber=Sep16_HO3-->


