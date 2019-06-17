---
title: Azure haritalar arama hizmetini verimli bir şekilde kullanarak arama yapma | Microsoft Docs
description: Azure haritalar arama hizmetini kullanarak arama için en iyi yöntemler kullanmayı öğrenin
ms.author: v-musehg
ms.date: 04/08/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 9cb0f89b4a48d7139adb35dcef48c0115b005c57
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65205617"
---
# <a name="best-practices-to-use-azure-maps-search-service"></a>Azure haritalar arama hizmetini kullanmak için en iyi uygulamalar

Azure haritalar [arama hizmeti](https://docs.microsoft.com/rest/api/maps/search) API'leri çeşitli özelliklerle, örneğin, belirli bir konuma etrafında ilgi noktası (POI) veri arama için adres aramaya ilişkin içerir. Bu makalede, sizi Azure haritalar, arama hizmetleri aracılığıyla veri aramak için en iyi yöntemleri paylaşacak. Şunları öğrenirsiniz:

* Uygun eşleşmeleri döndürmek için sorgu oluşturma
* Sınırı arama sonuçları
* Çeşitli sonuç türleri arasındaki farkı öğrenin
* Adres Arama yanıt yapısı okuyun


## <a name="prerequisites"></a>Önkoşullar

Haritalar hizmetini API çağrıları yapmak için bir haritalar hesabı ve anahtarı gereklidir. Hesap oluşturma ve anahtar alma hakkında daha fazla bilgi için bkz: [Azure haritalar hesabı ve anahtarları yönetme](how-to-manage-account-keys.md).

> [!Tip]
> Arama hizmeti sorgulamak için kullanabileceğiniz [Postman uygulamasını](https://www.getpostman.com/apps) REST oluşturmak için çağrıları veya tercih ettiğiniz herhangi bir API geliştirme ortamında kullanabilirsiniz.


## <a name="best-practices-for-geocoding"></a>Coğrafi kodlama için en iyi uygulamalar

Azure haritalar Arama Hizmeti'ni kullanarak bir tam veya kısmi adresi için arama yaptığınızda, arama teriminizi alır ve adresi boylam ve enlem koordinatlarını döndürür. Bu işlem, coğrafi kodlama adı verilir. Bir ülkede geocode yeteneği, yol veri kapsamı ve coğrafi kodlama hizmeti coğrafi kodlama duyarlığını temel bağlıdır.

Bkz: [coğrafi kodlama kapsamı](https://docs.microsoft.com/azure/azure-maps/geocoding-coverage) ülke/bölgeye göre Azure haritalar coğrafi kodlama özellikleri hakkında daha fazla bilgi edinmek için.

### <a name="limit-search-results"></a>Sınırı arama sonuçları

   Bu bölümde, arama sonuçlarını sınırlamak için Azure haritalar, arama API'leri kullanmayı öğreneceksiniz. 

   > [!Note]
   > Tüm arama API'leri tam olarak aşağıda listelenen parametreleri destekler.

   **Coğrafi sapması arama sonuçları**

   Coğrafi sapması sırada sonuçlarınızı ilgili alanına, kullanıcı için her zaman ayrıntılı mümkün olan en fazla eklemelisiniz konumu girişi. Arama sonuçları kısıtlamak için aşağıdaki giriş türleri eklemeyi göz önünde bulundurun:

   1. Ayarlama `countrySet` parametresi, örneğin "US, FR". Varsayılan arama davranışı, büyük olasılıkla gereksiz sonuçları döndüren dünyaya arama yapmaktır. Sorgunuzu içermiyorsa `countrySet` parametresi, arama tutarsız sonuçlar döndürebilir. Örneğin, adında bir şehir için arama **Bellevue** adlı Şehir olduğundan ABD ve Fransa, sonuçlar verir **Bellevue** Fransa'da ve ABD.

   2. Kullanabileceğiniz `btmRight` ve `topleft` sınırlayıcı kümesi için parametreleri kutusuna arama harita üzerinde belirli bir alanı sınırlamak için.

   3. Sonuçları ilgi alanını etkilemek için tanımlayabileceğiniz `lat`ve `lon` koordine parametreleri ve RADIUS arama alanı kullanarak ayarlayın `radius` parametresi.


   **Belirsiz arama parametreleri**

   1. `minFuzzyLevel` Ve `maxFuzzyLevel`, uygun eşleşmeleri bile sorgu parametreleri tam olarak istenen bilgilerine karşılık gelen zaman döndürün Yardım. Varsayılan olarak çoğu arama sorguları `minFuzzyLevel=1` ve `maxFuzzyLevel=2` performans elde edin ve olağan dışı sonuçları azaltmak için. Bir arama terimi örneği Al "restrant", "restorana" ne zaman eşleştirilir `maxFuzzyLevel` 2 olarak ayarlanır. İstek ihtiyaçlara göre varsayılan belirsiz düzeyleri geçersiz kılınabilir. 

   2. Tam kümesini kullanarak döndürülecek sonuç türleri de belirtebilirsiniz `idxSet` parametresi. Bu amaç için dizinlerinin virgülle ayrılmış liste gönderebilirsiniz, öğe sırası önemli değildir. Desteklenen dizinler şunlardır:

       * `Addr` - **Adres aralıkları**: Bazı sokaklar için başlangıç ve bitiş Sokak ilişkilendirilmiş adresi noktaları vardır. Bu noktaları adres aralığı temsil edilir.
       * `Geo` - **Coğrafyalar**: Bir yönetici bölümü olan bir kara, yani temsil eden bir harita, ülke, durum, şehir alanlarını.
       * `PAD` - **Noktası adresi**:  Belirli bir adresi olan sokak adı ve numarası bir dizine, örneğin, Soquel Dr 2501 bulunabileceği bir haritada işaret eder. Doğruluk adresleri için kullanılabilir en üst düzeyidir.  
       * `POI` - **İlgilenilen noktaları**: Dikkat etmeniz ve ilginç noktaları bir haritada.  [Arama adresini alma](https://docs.microsoft.com/rest/api/maps/search/getsearchaddress) Poı'lere dönüş olmaz.  
       * `Str` - **Sokaklar**: Harita üzerinde sokaklar gösterimi.
       * `XStr` - **Sokaklar/kesişimlerini çapraz**:  Merkezleriyle gösterimi; iki sokaklar kesiştiği yerleştirir.


       **Kullanım örnekleri**:

       * idxSet POI (yalnızca arama ilgi noktalarını) = 

       * idxSet PANELİ, adres = (arama adresleri yalnızca doldurma noktası adres, adres = adres aralığı =)

### <a name="reverse-geocode-and-geography-entity-type-filter"></a>Ters geocode ve geography varlık türü filtresi

İle bir ters geocode araması yaparken [arama adres ters API](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse), hizmetin çokgenler yönetim alanları için döndürülecek yeteneği vardır. Parametresi sağlayarak `entityType` istekte belirtilen coğrafi konum varlık türleri Ara daraltabilirsiniz. Verilen yanıtları Coğrafya kimliği, hem de eşleşen varlık türü içerir. Birden fazla varlık getirmediğini, uç nokta döndüreceği **kullanılabilir küçük varlık**. Geometri kimliği, bu Coğrafya geometrisi almak için kullanılabilir döndürülen [Çokgen alma hizmeti](https://docs.microsoft.com/rest/api/maps/search/getsearchpolygon).

**Örnek istek:**

```HTTP
https://atlas.microsoft.com/search/address/reverse/json?api-version=1.0&subscription-key={subscription-key}&query=47.6394532,-122.1304551&language=en-US&entityType=Municipality
```

**Yanıt:**

```JSON
{
    "summary": {
        "queryTime": 8,
        "numResults": 1
    },
    "addresses": [
        {
            "address": {
                "routeNumbers": [],
                "countryCode": "US",
                "countrySubdivision": "WA",
                "countrySecondarySubdivision": "King",
                "countryTertiarySubdivision": "Seattle East",
                "municipality": "Redmond",
                "country": "United States",
                "countryCodeISO3": "USA",
                "freeformAddress": "Redmond, WA",
                "boundingBox": {
                    "northEast": "47.717105,-122.034537",
                    "southWest": "47.627016,-122.164998",
                    "entity": "position"
                },
                "countrySubdivisionName": "Washington"
            },
            "position": "47.639454,-122.130455",
            "dataSources": {
                "geometry": {
                    "id": "00005557-4100-3c00-0000-0000596ae571"
                }
            },
            "entityType": "Municipality"
        }
    ]
}
```

### <a name="search-results-language"></a>Arama sonuçları dili

`language` Parametresi verir ayarlamak hangi dil aramada sonuç döndürmedi. İstekte dil ayarlı değilse arama hizmeti otomatik olarak en yaygın dili ülke/bölge olarak varsayar. Ayrıca, belirtilen dil verilerinde mevcut olmadığı durumlarda, varsayılan dil kullanılır. Bkz: [desteklenen diller](https://docs.microsoft.com/azure/azure-maps/supported-languages) ülke/bölgeye göre Azure haritalar Hizmetleri göre desteklenen dillerin listesi.


### <a name="predictive-mode-auto-suggest"></a>Tahmine dayalı modu (otomatik öneri)

Kısmi sorgular için daha fazla eşleşme bulmak için `typeahead` parametresi, 'true' olarak ayarlanmalıdır. Sorgu kısmi bir giriş olarak yorumlanır ve arama Tahmine dayalı moduna girer. Aksi takdirde tüm ilgili bilgileri, geçirilen hizmet varsayar.

Aşağıdaki örnekte sorgu, aşağıdaki adresi hizmet arama "Microso" için sorgulanır ile görebilirsiniz `typeahead` parametresini **true**. Yanıt gözlemlerseniz, arama hizmeti sorgu kısmi bir sorgu olarak yorumlanır ve sonuçları için sorgu otomatik önerilen yanıt içerdiğini görebilirsiniz.

**Örnek sorgu:**

```HTTP
https://atlas.microsoft.com/search/address/json?subscription-key={subscription-key}&api-version=1.0&typeahead=true&countrySet=US&lat=47.6370891183&lon=-122.123736172&query=Microsoft
```

**Yanıt:**

```JSON
{
    "summary": {
        "query": "microsoft",
        "queryType": "NON_NEAR",
        "queryTime": 25,
        "numResults": 6,
        "offset": 0,
        "totalResults": 6,
        "fuzzyLevel": 1,
        "geoBias": {
            "lat": 47.6370891183,
            "lon": -122.123736172
        }
    },
    "results": [
        {
            "type": "Street",
            "id": "US/STR/p0/10294417",
            "score": 2.594,
            "dist": 327.546040632591,
            "address": {
                "streetName": "Microsoft Way",
                "municipalitySubdivision": "Redmond",
                "municipality": "Redmond",
                "countrySecondarySubdivision": "King",
                "countryTertiarySubdivision": "Seattle East",
                "countrySubdivision": "WA",
                "postalCode": "98052",
                "extendedPostalCode": "980526399,980528300",
                "countryCode": "US",
                "country": "United States Of America",
                "countryCodeISO3": "USA",
                "freeformAddress": "Microsoft Way, Redmond, WA 98052",
                "countrySubdivisionName": "Washington"
            },
            "position": {
                "lat": 47.63989,
                "lon": -122.12509
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.63748,
                    "lon": -122.12309
                },
                "btmRightPoint": {
                    "lat": 47.64223,
                    "lon": -122.13061
                }
            }
        },
        ...,
        ...,
        ...,
        ...,
        {
            "type": "Street",
            "id": "US/STR/p0/9063400",
            "score": 2.075,
            "dist": 3655467.6406921702,
            "address": {
                "streetName": "Microsoft Way",
                "municipalitySubdivision": "Yorkmount, Charlotte",
                "municipality": "Charlotte",
                "countrySecondarySubdivision": "Mecklenburg",
                "countryTertiarySubdivision": "Township 1 Charlotte",
                "countrySubdivision": "NC",
                "postalCode": "28217",
                "countryCode": "US",
                "country": "United States Of America",
                "countryCodeISO3": "USA",
                "freeformAddress": "Microsoft Way, Charlotte, NC 28217",
                "countrySubdivisionName": "North Carolina"
            },
            "position": {
                "lat": 35.14279,
                "lon": -80.91814
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 35.14267,
                    "lon": -80.91814
                },
                "btmRightPoint": {
                    "lat": 35.14279,
                    "lon": -80.91824
                }
            }
        }
    ]
}
```


### <a name="uri-encoding-to-handle-special-characters"></a>Özel karakterler işlemek için kodlama URI'si 

Sokak adresi platformlar arası, diğer bir deyişle, "1. Cadde No: & birleşim Sokak, Seattle", özel karakter bulmak için '&' gerekiyor isteği göndermeden önce kodlanmış olmalıdır. Bir URI karakter verisinde kodlama tüm karakterler, '%' karakterini kullanarak kodlanır ve bunların UTF-8 karaktere karşılık gelen iki karakterlik bir onaltılık değer öneririz.

**Kullanım örnekleri**:

Arama adresini alın:

```
query=1st Avenue & E 111th St, New York
```

 olarak kodlanmış:

```
query"=1st%20Avenue%20%26%20E%20111th%20St%2C%20New%20York
```


Farklı diller için kullanabileceğiniz farklı yöntemler şunlardır: 

JavaScript/TypeScript:
```Javascript
encodeURIComponent(query)
```

C#/VB:
```C#
Uri.EscapeDataString(query)
```

Java:
```Java
URLEncoder.encode(query, "UTF-8") 
```

Python:
```Python
import urllib.parse 
urllib.parse.quote(query)
```

C++:
```C++
#include <curl/curl.h>
curl_easy_escape(query)
```

PHP:
```PHP
urlencode(query)
```

Ruby:
```Ruby
CGI::escape(query) 
```

Swift:
```Swift
query.stringByAddingPercentEncodingWithAllowedCharacters(.URLHostAllowedCharacterSet()) 
```

Git:
```Go
import ("net/url") 
url.QueryEscape(query)
```


## <a name="best-practices-for-poi-search"></a>POI arama için en iyi uygulamalar

Noktası (POI) ilgi araması adıyla, örneğin, ada göre ara iş, POI sonuçları istemenizi sağlar. Biz kesin kullanmanızı öneriyoruz `countrySet` parametresinin varsayılan davranış büyük olasılıkla gereksiz sonuçları döndüren dünyaya aramak ve/veya uzun arama kez neden olacak şekilde kapsamı, uygulamanız gereken yere ülke belirtin.

### <a name="brand-search"></a>Marka arama

Sonuçları ve yanıt bilgileri ilgi artırmak için ilgi noktası (POI) arama yanıt yanıtları ayrıştırmak için daha fazla kullanılabilir marka bilgileri içerir.

Olalım bir [POI kategori arama](https://docs.microsoft.com/rest/api/maps/search/getsearchpoicategory) Microsoft yerleşkesindeki (Redmond, WA) yakın benzin istasyonlarımıza isteği. Yanıt gözlemlerseniz, döndürülen her POI marka bilgilerini görebilirsiniz.

**Örnek sorgu:**

```HTTP
https://atlas.microsoft.com/search/poi/json?subscription-key={subscription-key}&api-version=1.0&query=gas%20station&limit=3&lat=47.6413362&lon=-122.1327968
```

**Yanıt:**

```JSON
{
    "summary": {
        "query": "gas station",
        "queryType": "NON_NEAR",
        "queryTime": 206,
        "numResults": 3,
        "offset": 0,
        "totalResults": 742169,
        "fuzzyLevel": 1,
        "geoBias": {
            "lat": 47.6413362,
            "lon": -122.1327968
        }
    },
    "results": [
        {
            "type": "POI",
            "id": "US/POI/p0/245813",
            "score": 5.663,
            "dist": 1037.0280221303253,
            "info": "search:ta:840531000004190-US",
            "poi": {
                "name": "Chevron",
                "phone": "+(1)-(425)-6532200",
                "brands": [
                    {
                        "name": "Chevron"
                    }
                ],
                "url": "www.chevron.com",
                "classifications": [
                    {
                        "code": "PETROL_STATION",
                        "names": [
                            {
                                "nameLocale": "en-US",
                                "name": "petrol station"
                            }
                        ]
                    }
                ]
            },
            "address": {
                "streetNumber": "2444",
                "streetName": "Bel Red Rd",
                "municipalitySubdivision": "Northeast Bellevue, Bellevue",
                "municipality": "Bellevue",
                "countrySecondarySubdivision": "King",
                "countryTertiarySubdivision": "Seattle East",
                "countrySubdivision": "WA",
                "postalCode": "98007",
                "countryCode": "US",
                "country": "United States Of America",
                "countryCodeISO3": "USA",
                "freeformAddress": "2444 Bel Red Rd, Bellevue, WA 98007",
                "countrySubdivisionName": "Washington"
            },
            "position": {
                "lat": 47.63201,
                "lon": -122.13281
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.63291,
                    "lon": -122.13414
                },
                "btmRightPoint": {
                    "lat": 47.63111,
                    "lon": -122.13148
                }
            },
            "entryPoints": [
                {
                    "type": "main",
                    "position": {
                        "lat": 47.63223,
                        "lon": -122.13311
                    }
                }
            ]
        },
        ...,
        {
            "type": "POI",
            "id": "US/POI/p0/7727106",
            "score": 5.662,
            "dist": 1458.645407416307,
            "info": "search:ta:840539000488527-US",
            "poi": {
                "name": "BROWN BEAR CAR WASH",
                "phone": "+(1)-(425)-6442868",
                "brands": [
                    {
                        "name": "Texaco"
                    }
                ],
                "url": "www.texaco.com/",
                "classifications": [
                    {
                        "code": "PETROL_STATION",
                        "names": [
                            {
                                "nameLocale": "en-US",
                                "name": "petrol station"
                            }
                        ]
                    }
                ]
            },
            "address": {
                "streetNumber": "15248",
                "streetName": "Bel Red Rd",
                "municipalitySubdivision": "Redmond",
                "municipality": "Redmond",
                "countrySecondarySubdivision": "King",
                "countryTertiarySubdivision": "Seattle East",
                "countrySubdivision": "WA",
                "postalCode": "98052",
                "extendedPostalCode": "980525511",
                "countryCode": "US",
                "country": "United States Of America",
                "countryCodeISO3": "USA",
                "freeformAddress": "15248 Bel Red Rd, Redmond, WA 98052",
                "countrySubdivisionName": "Washington"
            },
            "position": {
                "lat": 47.62843,
                "lon": -122.13628
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.62933,
                    "lon": -122.13761
                },
                "btmRightPoint": {
                    "lat": 47.62753,
                    "lon": -122.13495
                }
            },
            "entryPoints": [
                {
                    "type": "main",
                    "position": {
                        "lat": 47.62826,
                        "lon": -122.13626
                    }
                }
            ]
        }
    ]
}
```


### <a name="airport-search"></a>Havaalanı arama

POI arama havaalanları resmi havaalanı kodu kullanarak aramayı destekler. Örneğin, **SEA** (Seattle Ankara International Airport). 

```HTTP
https://atlas.microsoft.com/search/poi/json?subscription-key={subscription-key}&api-version=1.0&query=SEA 
```

### <a name="nearby-search"></a>Yakındaki arama

Yalnızca belirli bir konuma etrafında POI sonuçları almak için [arama API'si yakındaki](https://docs.microsoft.com/rest/api/maps/search/getsearchnearby) doğru seçim olabilir. Bu uç noktaya, yalnızca POI sonuçları döndürür ve arama sorgu parametresi almaz. Sonuçları sınırlandırmak için RADIUS ayarlanması önerilir.

## <a name="understanding-the-responses"></a>Yanıtları anlama

Azure haritalar için bir adres arama isteği olalım [arama hizmetinizi](https://docs.microsoft.com/rest/api/maps/search) Seattle adresi. İstek URL'SİNDE dikkatle bakarsanız, biz ayarladığınız `countrySet` parametresi **ABD** adres Amerika Birleşik Devletleri, aranacak.

**Örnek sorgu:**

```HTTP
https://atlas.microsoft.com/search/address/json?subscription-key={subscription-key}&api-version=1&query=400%20Broad%20Street%2C%20Seattle%2C%20WA&countrySet=US
```

Daha fazla yanıt yapısı aşağıdaki göz şimdi vardır. Yanıtta sonuç nesnelerinin sonuç türleri farklıdır. Sonuç nesnelerini üç farklı türde sahibiz gördüğünüz dikkatle gözlemlerseniz "Noktası adresi", "Sokak" ve "Arası Sokak" olan. Bu adres arama Poı'lere döndürmeyen dikkat edin. `Score` Parametresi her yanıt nesnesi için diğer nesnelerin aynı Yanıt Puanları göreli eşleşen puanına gösterir. Bkz: [arama adresi alma](https://docs.microsoft.com/rest/api/maps/search/getsearchaddress) yanıt nesnesi parametreler hakkında daha fazla bilgi edinmek için.

**Sonuç, desteklenen türler:**

* **Noktası adresi:** Belirli bir adresi olan sokak adı ve numarası içeren bir haritadaki işaret eder. En yüksek düzeyde doğruluğu adresleri için kullanılabilir. 

* **Adres aralığı:**  Bazı sokaklar için başlangıç ve bitiş Sokak ilişkilendirilmiş adresi noktaları vardır. Bu noktaları adres aralığı temsil edilir. 

* **Coğrafi konum:** Bir yönetici bölümü olan bir kara, yani temsil eden bir harita, ülke, durum, şehir alanlarını. 

* **POI - (ilgi noktası):** Dikkat etmeniz ve ilginç noktaları bir haritada.

* **Sokak:** Harita üzerinde sokaklar gösterimi. Adresleri adresini içeren Sokak enlem/boylam koordinatını çözümlenir. Bina numarasını işlenmeyebilir. 

* **Çapraz Sokak:** Kesişimlerini. Merkezleriyle temsillerini; iki sokaklar kesiştiği yerleştirir.

**Yanıt:**

```JSON
{
    "summary": {
        "query": "400 broad street seattle wa",
        "queryType": "NON_NEAR",
        "queryTime": 129,
        "numResults": 6,
        "offset": 0,
        "totalResults": 6,
        "fuzzyLevel": 1
    },
    "results": [
        {
            "type": "Point Address",
            "id": "US/PAD/p0/43076024",
            "score": 9.894,
            "address": {
                "streetNumber": "400",
                "streetName": "Broad Street",
                "municipalitySubdivision": "Seattle, South Lake Union, Lower Queen Anne",
                "municipality": "Seattle",
                "countrySecondarySubdivision": "King",
                "countryTertiarySubdivision": "Seattle",
                "countrySubdivision": "WA",
                "postalCode": "98109",
                "countryCode": "US",
                "country": "United States Of America",
                "countryCodeISO3": "USA",
                "freeformAddress": "400 Broad Street, Seattle, WA 98109",
                "countrySubdivisionName": "Washington"
            },
            "position": {
                "lat": 47.62039,
                "lon": -122.34928
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.62129,
                    "lon": -122.35061
                },
                "btmRightPoint": {
                    "lat": 47.61949,
                    "lon": -122.34795
                }
            },
            "entryPoints": [
                {
                    "type": "main",
                    "position": {
                        "lat": 47.61982,
                        "lon": -122.34886
                    }
                }
            ]
        },
        {
            "type": "Street",
            "id": "US/STR/p0/2440854",
            "score": 8.129,
            "address": {
                "streetName": "Broad Street",
                "municipalitySubdivision": "Seattle, Westlake, South Lake Union",
                "municipality": "Seattle",
                "countrySecondarySubdivision": "King",
                "countryTertiarySubdivision": "Seattle",
                "countrySubdivision": "WA",
                "postalCode": "98109",
                "extendedPostalCode": "981094347,981094700,981094701,981094702",
                "countryCode": "US",
                "country": "United States Of America",
                "countryCodeISO3": "USA",
                "freeformAddress": "Broad Street, Seattle, WA 98109",
                "countrySubdivisionName": "Washington"
            },
            "position": {
                "lat": 47.62553,
                "lon": -122.33936
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.62545,
                    "lon": -122.33861
                },
                "btmRightPoint": {
                    "lat": 47.62574,
                    "lon": -122.33974
                }
            }
        },
        {
            "type": "Street",
            "id": "US/STR/p0/8450985",
            "score": 8.129,
            "address": {
                "streetName": "Broad Street",
                "municipalitySubdivision": "Seattle, Belltown",
                "municipality": "Seattle",
                "countrySecondarySubdivision": "King",
                "countryTertiarySubdivision": "Seattle",
                "countrySubdivision": "WA",
                "postalCode": "98109,98121",
                "extendedPostalCode": "981094991,981211117,981211237,981213206",
                "countryCode": "US",
                "country": "United States Of America",
                "countryCodeISO3": "USA",
                "freeformAddress": "Broad Street, Seattle, WA",
                "countrySubdivisionName": "Washington"
            },
            "position": {
                "lat": 47.61691,
                "lon": -122.35251
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.61502,
                    "lon": -122.35041
                },
                "btmRightPoint": {
                    "lat": 47.61857,
                    "lon": -122.35484
                }
            }
        },
        ...,
        ...,
        {
            "type": "Cross Street",
            "id": "US/XSTR/p1/3816818",
            "score": 6.759,
            "address": {
                "streetName": "Broad Street & Valley Street",
                "municipalitySubdivision": "South Lake Union, Seattle",
                "municipality": "Seattle",
                "countrySecondarySubdivision": "King",
                "countryTertiarySubdivision": "Seattle",
                "countrySubdivision": "WA",
                "postalCode": "98109",
                "countryCode": "US",
                "country": "United States Of America",
                "countryCodeISO3": "USA",
                "freeformAddress": "Broad Street & Valley Street, Seattle, WA 98109",
                "countrySubdivisionName": "Washington"
            },
            "position": {
                "lat": 47.62574,
                "lon": -122.33861
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.62664,
                    "lon": -122.33994
                },
                "btmRightPoint": {
                    "lat": 47.62484,
                    "lon": -122.33728
                }
            }
        }
    ]
}
```

### <a name="geometry"></a>Geometri

Yanıt türü olduğunda **geometri**, iade geometri kimliği içerebilir **veri kaynakları** "geometri" ve "id" altındaki nesne. Örneğin, [Çokgen alma hizmeti](https://docs.microsoft.com/rest/api/maps/search/getsearchpolygon) geometri veri gibi bir dizi varlık için bir şehir veya havaalanı anahat GeoJSON biçiminde istemenizi sağlar. Bu sınır veriler için kullanabileceğiniz [bölge sınırlaması](https://docs.microsoft.com/azure/azure-maps/tutorial-geofence) veya [geometri içinde arama Poı'lere](https://docs.microsoft.com/rest/api/maps/search/postsearchinsidegeometry).


[Adres Arama](https://docs.microsoft.com/rest/api/maps/search/getsearchaddress) veya [arama belirsiz](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) API yanıtları içerebilir **geometri kimliği** veri kaynakları nesnedeki "geometri" ve "id" altında döndürülür.


```JSON 
"dataSources": { 
        "geometry": { 
            "id": "00005557-4100-3c00-0000-000059690938" // The geometry ID is returned in the dataSources object under "geometry" and "id".
        }
} 
```

## <a name="next-steps"></a>Sonraki adımlar

* Bilgi [Azure haritalar arama hizmeti istekleri oluşturmak nasıl](https://docs.microsoft.com/azure/azure-maps/how-to-search-for-address).
* Azure haritalar'ı keşfedin [arama hizmeti API belgeleri](https://docs.microsoft.com/rest/api/maps/search). 