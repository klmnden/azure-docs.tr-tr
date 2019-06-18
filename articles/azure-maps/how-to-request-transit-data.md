---
title: Azure haritalar aktarım sırasında verileri nasıl | Microsoft Docs
description: Azure haritalar Mobility hizmetini kullanarak genel aktarım sırasında verileri isteyin.
author: walsehgal
ms.author: v-musehg
ms.date: 06/05/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.custom: mvc
ms.openlocfilehash: e8250763153f7c5b71f3906a560365dadfd55694
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66735579"
---
# <a name="request-public-transit-data-using-the-azure-maps-mobility-service"></a>Azure haritalar Mobility hizmetini kullanarak genel aktarım sırasında verileri istek 

Bu makalede Azure haritalar'ı kullanmayı gösterir [Mobility hizmeti](https://aka.ms/AzureMapsMobilityService) genel aktarım isteği gibi duruyor, veriler yol bilgileri ve zaman tahminleri seyahat.

Bu makalede, öğreneceksiniz, nasıl yapılır:

* Metro alan Kimliğini kullanarak alma [Metro alan API Al](https://aka.ms/AzureMapsMobilityMetro)
* Yakındaki geçiş durdurur kullanarak istek [alma yakında geçiş](https://aka.ms/AzureMapsMobilityNearbyTransit) hizmeti.
* Sorgu [alma geçiş rotalar API'si](https://aka.ms/AzureMapsMobilityTransitRoute) genel geçişi kullanarak bir rota planlamak için.
* Transit yönlendirme geometri ve rota kullanarak ayrıntılı zamanlama isteği [alma aktarım programı API](https://aka.ms/ https://azure.microsoft.com/services/azure-maps/).


## <a name="prerequisites"></a>Önkoşullar

Azure haritalar genel aktarım API çağrıları yapmak için bir haritalar hesabı ve anahtarı gereklidir. Hesap oluşturma ve anahtar alma hakkında daha fazla bilgi için bkz: [Azure haritalar hesabı ve anahtarları yönetme](how-to-manage-account-keys.md).

Bu makalede [Postman uygulamasını](https://www.getpostman.com/apps) REST çağrılarını oluşturulacak. Tercih ettiğiniz herhangi bir API geliştirme ortamında kullanabilirsiniz.


## <a name="get-a-metro-area-id"></a>Metro alan kimliği Al

Belirli bir metropoliten alanında ihtiyaç duyacağınız için geçiş bilgi istemek için `metroId` aktarım sırasında verileri istek istediğiniz alanı. [Metro alan API'sini kullanmaya başlamak](https://aka.ms/AzureMapsMobilityMetro) Azure haritalar Mobility hizmetinin kullanılabildiği metro alanları istemenizi sağlar. Yanıt ayrıntıları gibi dahil `metroId`, `metroName` ve metro alan geometri GeoJSON biçiminde bir temsili.

Metro alanını Seattle Ankara metro alan kimliği için alma isteği olalım İstek Kimliği için metro bir alan için aşağıdaki adımları tamamlayın:

1. İstekleri depolanacağı bir koleksiyon oluşturun. Postman uygulamasında seçin **yeni**. İçinde **Yeni Oluştur** penceresinde **koleksiyon**. Koleksiyonu adlandırın ve seçin **Oluştur** düğmesi.

2. İsteği oluşturmak için Seç **yeni** yeniden. İçinde **Yeni Oluştur** penceresinde **istek**. Girin bir **istek adı** isteği, istek kaydedin ve ardından konum olarak önceki adımda oluşturduğunuz koleksiyonu seçin **Kaydet**.
    
    ![Postman içinde bir isteği oluştur](./media/how-to-request-transit-data/postman-new.png)

3. Oluşturucu sekmesinde GET HTTP yöntemini seçin ve bir GET isteği oluşturmak için aşağıdaki URL'yi girin.

    ```HTTP
    https://atlas.microsoft.com/mobility/metroArea/id/json?subscription-key={subscription-key}&api-version=1.0&query=47.63096,-122.126
    ```

4. Başarılı bir isteği sonra şu yanıtı alırsınız:

    ```JSON
    {
        "results": [
            {
                "metroId": "522",
                "metroName": "Seattle–Tacoma–Bellevue, WA",
                "geometry": {
                    "type": "Polygon",
                    "coordinates": [
                        [
                            [
                                -121.99604,
                                47.16147
                            ],
                            [
                                -121.97051,
                                47.17222
                            ],
                            [
                                -121.96308,
                                47.17671
                            ],
                            [
                                -121.95725,
                                47.18314
                            ],
                            ...,
                            ...,
                            ...,
                            ...,
                            [
                                -122.18711,
                                47.15571
                            ],
                            [
                                -122.01525,
                                47.16008
                            ],
                            [
                                -122.00553,
                                47.15919
                            ],
                            [
                                -121.99604,
                                47.16147
                            ]
                        ]
                    ]
                },
                "viewport": {
                    "topLeftPoint": {
                        "latitude": 48.5853,
                        "longitude": -124.80934
                    },
                    "btmRightPoint": {
                        "latitude": 46.90534,
                        "longitude": -121.55032
                    }
                }
            }
        ]
    }
    ```

5. Kopyalama `metroId`, daha sonra kullanmak üzere.

## <a name="request-nearby-transit-stops"></a>Geçiş durakları iste

Azure haritalar [alma yakında geçiş](https://aka.ms/AzureMapsMobilityNearbyTransit) hizmeti geçiş nesneleri arama olanak tanır, örneğin, genel aktarım durdurur ve belirli bir konuma geçiş nesne ayrıntıları döndüren etrafında bisiklet paylaşılan. Yakında genel aktarım 300 ölçümleri RADIUS içinde durdurur arama hizmetine bir istek sonraki bulunacağız geçici konuma verilir. İstekte eklememiz gerektiğini `metroId` daha önce aldığınız.

İsteğin yapılacağı [alma yakında geçiş](https://aka.ms/AzureMapsMobilityNearbyTransit), aşağıdaki adımları izleyin:

1. Postman içinde tıklayın **yeni istek** | **GET isteği** ve adlandırın **alma yakında durdurur**.

2. Oluşturucu sekmesinde **alma** HTTP yöntemi, API uç noktası için aşağıdaki istek URL'sini girin ve tıklatın **Gönder**.

    ```HTTP
    https://atlas.microsoft.com/mobility/transit/nearby/json?subscription-key={subscription-key}&api-version=1.0&metroId=522&query=47.63096,-122.126&radius=300&objectType=stop
    ```

3. Sonra başarılı bir istek, yanıt yapısı bir aşağıdaki gibi görünmelidir:

    ```JSON
    {
        "results": [
            {
                "id": "2060603",
                "type": "Stop",
                "objectDetails": {
                    "stopKey": "71300",
                    "stopName": "Ne 24th St & 162nd Ave Ne",
                    "mainTransitType": "BUS",
                    "mainAgencyId": 5872,
                    "mainAgencyName": "Metro Transit"
                },
                "position": {
                    "latitude": 47.631504,
                    "longitude": -122.125275
                },
                "viewport": {
                    "topLeftPoint": {
                        "latitude": 47.63241381296315,
                        "longitude": -122.12659096560266
                    },
                    "btmRightPoint": {
                        "latitude": 47.630594172088166,
                        "longitude": -122.12395908007201
                    }
                }
            },
            {
                "id": "2061020",
                "type": "Stop",
                "objectDetails": {
                    "stopKey": "68372",
                    "stopName": "Ne 24th St & 160th Ave Ne",
                    "mainTransitType": "BUS",
                    "mainAgencyId": 5872,
                    "mainAgencyName": "Metro Transit"
                },
                "position": {
                    "latitude": 47.631409,
                    "longitude": -122.127136
                },
                "viewport": {
                    "topLeftPoint": {
                        "latitude": 47.632318791818726,
                        "longitude": -122.12845199584025
                    },
                    "btmRightPoint": {
                        "latitude": 47.63049919323126,
                        "longitude": -122.12582004983427
                    }
                }
            },
            {
                "id": "2060604",
                "type": "Stop",
                "objectDetails": {
                    "stopKey": "71310",
                    "stopName": "Ne 24th St & 160th Ave Ne",
                    "mainTransitType": "BUS",
                    "mainAgencyId": 5872,
                    "mainAgencyName": "Metro Transit"
                },
                "position": {
                    "latitude": 47.631565,
                    "longitude": -122.127808
                },
                "viewport": {
                    "topLeftPoint": {
                        "latitude": 47.632474784183636,
                        "longitude": -122.12912401149087
                    },
                    "btmRightPoint": {
                        "latitude": 47.630655200865796,
                        "longitude": -122.12649203418405
                    }
                }
            }
        ]
    }    
    ```

Dikkatli bir şekilde yanıt yapısı gözlemlerseniz, bu parametreleri her geçiş nesne gibi içerdiğini görebilirsiniz `id`, `type`, `stopName`, `mainTransitType`, `mainAgencyName` ve nesnenin konumunu (koordinatları).

Anlamak amacıyla kullanacağız `id` bir sonraki bölümde bizim rota için kaynak olarak bus durdurur.  


## <a name="request-a-transit-route"></a>İstek bir geçiş yolu

Azure haritalar [alma geçiş rotalar API'si](https://aka.ms/AzureMapsMobilityTransitRoute) bir kaynak ve hedef arasında en iyi olası yolu seçenekleri döndüren seyahat planlama sağlar. Seyahat modlarında, walking Bisiklete ve genel geçiş de dahil olmak üzere çeşitli hizmetidir. Sonraki size Seattle'alanı iğne en yakın veri yolu durdurma gelen bir yol arar.

### <a name="get-location-coordinates-for-destination"></a>İçin hedef konum koordinatlarını öğrenme

Konumu almak amacıyla alanı iğne koordinatları sağlar Azure haritalar kullanan [belirsiz arama hizmeti](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy).

Belirsiz arama hizmetine bir istekte bulunmak için aşağıdaki adımları izleyin:

1. Postman içinde tıklayın **yeni istek** | **GET isteği** ve adlandırın **konum koordinatlarını almak**.

2.  Oluşturucu sekmesinde **alma** HTTP yöntemi, aşağıdaki istek URL'sini girin ve tıklatın **Gönder**.
 
    ```HTTP
    https://atlas.microsoft.com/search/fuzzy/json?subscription-key={subscription-key}&api-version=1.0&query=space needle
    ```
    
3. Yanıtında dikkatle bakarsanız, birden fazla konumda sonuçları alanı iğne için içerir ve her biri altında için konum koordinatlarını bilgileri de içerir **konumu**. Kopyalama `lat` ve `lon` ilk sonuç için konum.
    
   ```JSON
   {
        "summary": {
            "query": "space needle",
            "queryType": "NON_NEAR",
            "queryTime": 61,
            "numResults": 8,
            "offset": 0,
            "totalResults": 24,
            "fuzzyLevel": 1
        },
        "results": [
            {
                "type": "POI",
                "id": "US/POI/p0/8309323",
                "score": 4.674,
                "info": "search:ta:840539000511573-US",
                "poi": {
                    "name": "Space Needle",
                    "phone": "+(1)-(206)-9052100",
                    "url": "www.spaceneedle.com",
                    "categories": [
                        "important tourist attraction",
                        "monument"
                    ],
                    "classifications": [
                        {
                            "code": "IMPORTANT_TOURIST_ATTRACTION",
                            "names": [
                                {
                                    "nameLocale": "en-US",
                                    "name": "important tourist attraction"
                                },
                                {
                                    "nameLocale": "en-US",
                                    "name": "monument"
                                }
                            ]
                        }
                    ]
                },
                "address": {
                    "streetNumber": "400",
                    "streetName": "Broad St",
                    "municipalitySubdivision": "South Lake Union, Seattle, Lower Queen Anne",
                    "municipality": "Seattle",
                    "countrySecondarySubdivision": "King",
                    "countryTertiarySubdivision": "Seattle",
                    "countrySubdivision": "WA",
                    "postalCode": "98109",
                    "countryCode": "US",
                    "country": "United States Of America",
                    "countryCodeISO3": "USA",
                    "freeformAddress": "400 Broad St, Seattle, WA 98109",
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
            ...,
            ...,
            ...
            
        ]
    }
    ``` 
    

### <a name="request-route"></a>İstek yolu

Rota istekte bulunmak için aşağıdaki adımları tamamlayın:

1. Postman içinde tıklayın **yeni istek** | **GET isteği** ve adlandırın **rota Al bilgileri**.

2. Oluşturucu sekmesinde **alma** HTTP yöntemi, API uç noktası için aşağıdaki istek URL'sini girin ve tıklatın **Gönder**.

    Biz belirterek Service Bus genel geçiş yolları isteyecek `modeType` ve `transitType` parametreleri. İstek URL'si önceki bölümlerde alınan konumları içerir. Olarak `originType` artık **stopId** ve `destionationType` sahibiz **konumu**.

    Bkz [URI parametreleri listesi](https://aka.ms/AzureMapsMobilityTransitRoute#uri-parameters) isteğinizi kullanabileceğiniz [alma geçiş rotalar API'si](https://aka.ms/AzureMapsMobilityTransitRoute). 
  
    ```HTTP
    https://atlas.microsoft.com/mobility/transit/route/json?subscription-key={subscription-key}&api-version=1.0&metroId=522&originType=stopId&origin=2060603&destionationType=position&destination=47.62039,-122.34928&modeType=publicTransit&transitType=bus
    ```

3. Başarılı bir istek üzerine yanıt yapısı bir aşağıdaki gibi görünmelidir:

    ```JSON
    {
        "results": [
            {
                "itineraryId": "302c38dd-6585-4fa1-bf78-44ebbc183e0c---2019040384C30774B4B94F178E7748644A476596:0---522",
                "departureTime": "2019-04-03T14:21:34-07:00",
                "arrivalTime": "2019-04-03T15:15:53-07:00",
                "travelTimeInSeconds": 3259,
                "numberOfLegs": 10,
                "legs": [
                    {
                        "legType": "Walk",
                        "legStartTime": "2019-04-03T14:21:34-07:00",
                        "legEndTime": "2019-04-03T14:28:19-07:00",
                        "caption": "156th Avenue Northeast",
                        "lengthInMeters": 497
                    },
                    {
                        "legType": "Wait",
                        "legStartTime": "2019-04-03T14:28:19-07:00",
                        "legEndTime": "2019-04-03T14:29:20-07:00",
                        "caption": "245"
                    },
                    {
                        "legType": "Bus",
                        "legStartTime": "2019-04-03T14:29:20-07:00",
                        "legEndTime": "2019-04-03T14:32:00-07:00",
                        "caption": "245",
                        "lengthInMeters": 1350
                    },
                    {
                        "legType": "Walk",
                        "legStartTime": "2019-04-03T14:32:01-07:00",
                        "legEndTime": "2019-04-03T14:33:07-07:00",
                        "caption": "156th Avenue Northeast",
                        "lengthInMeters": 63
                    },
                    {
                        "legType": "Wait",
                        "legStartTime": "2019-04-03T14:33:07-07:00",
                        "legEndTime": "2019-04-03T14:38:00-07:00",
                        "caption": "545"
                    },
                    {
                        "legType": "Bus",
                        "legStartTime": "2019-04-03T14:38:00-07:00",
                        "legEndTime": "2019-04-03T14:59:47-07:00",
                        "caption": "545",
                        "lengthInMeters": 16441
                    },
                    {
                        "legType": "Walk",
                        "legStartTime": "2019-04-03T14:59:48-07:00",
                        "legEndTime": "2019-04-03T15:03:53-07:00",
                        "caption": "Denny Way",
                        "lengthInMeters": 308
                    },
                    {
                        "legType": "Wait",
                        "legStartTime": "2019-04-03T15:03:53-07:00",
                        "legEndTime": "2019-04-03T15:07:26-07:00",
                        "caption": "8"
                    },
                    {
                        "legType": "Bus",
                        "legStartTime": "2019-04-03T15:07:26-07:00",
                        "legEndTime": "2019-04-03T15:12:12-07:00",
                        "caption": "8",
                        "lengthInMeters": 1057
                    },
                    {
                        "legType": "Walk",
                        "legStartTime": "2019-04-03T15:12:13-07:00",
                        "legEndTime": "2019-04-03T15:15:53-07:00",
                        "caption": "47.6205,-122.3493",
                        "lengthInMeters": 268
                    }
                ]
            },
            ...,
            {
                "itineraryId": "302c38dd-6585-4fa1-bf78-44ebbc183e0c---2019040384C30774B4B94F178E7748644A476596:2---522",
                "departureTime": "2019-04-03T14:21:34-07:00",
                "arrivalTime": "2019-04-03T15:19:18-07:00",
                "travelTimeInSeconds": 3464,
                "numberOfLegs": 10,
                "legs": [
                    {
                        "legType": "Walk",
                        "legStartTime": "2019-04-03T14:21:34-07:00",
                        "legEndTime": "2019-04-03T14:28:19-07:00",
                        "caption": "156th Avenue Northeast",
                        "lengthInMeters": 497
                    },
                    {
                        "legType": "Wait",
                        "legStartTime": "2019-04-03T14:28:19-07:00",
                        "legEndTime": "2019-04-03T14:29:20-07:00",
                        "caption": "245"
                    },
                    {
                        "legType": "Bus",
                        "legStartTime": "2019-04-03T14:29:20-07:00",
                        "legEndTime": "2019-04-03T14:32:00-07:00",
                        "caption": "245",
                        "lengthInMeters": 1350
                    },
                    {
                        "legType": "Walk",
                        "legStartTime": "2019-04-03T14:32:01-07:00",
                        "legEndTime": "2019-04-03T14:33:07-07:00",
                        "caption": "156th Avenue Northeast",
                        "lengthInMeters": 63
                    },
                    {
                        "legType": "Wait",
                        "legStartTime": "2019-04-03T14:33:07-07:00",
                        "legEndTime": "2019-04-03T14:38:00-07:00",
                        "caption": "545"
                    },
                    {
                        "legType": "Bus",
                        "legStartTime": "2019-04-03T14:38:00-07:00",
                        "legEndTime": "2019-04-03T15:01:00-07:00",
                        "caption": "545",
                        "lengthInMeters": 17400
                    },
                    {
                        "legType": "Walk",
                        "legStartTime": "2019-04-03T15:01:01-07:00",
                        "legEndTime": "2019-04-03T15:04:59-07:00",
                        "caption": "3rd Avenue",
                        "lengthInMeters": 269
                    },
                    {
                        "legType": "Wait",
                        "legStartTime": "2019-04-03T15:04:59-07:00",
                        "legEndTime": "2019-04-03T15:09:14-07:00",
                        "caption": "33"
                    },
                    {
                        "legType": "Bus",
                        "legStartTime": "2019-04-03T15:09:14-07:00",
                        "legEndTime": "2019-04-03T15:12:52-07:00",
                        "caption": "33,24",
                        "lengthInMeters": 947
                    },
                    {
                        "legType": "Walk",
                        "legStartTime": "2019-04-03T15:12:53-07:00",
                        "legEndTime": "2019-04-03T15:19:18-07:00",
                        "caption": "47.6205,-122.3493",
                        "lengthInMeters": 474
                    }
                ]
            }
        ]
    }
    ```

4. Dikkatle uyun varsa birden çok **bus** yanıt yollar. Her yol benzersiz olan **programı kimliği** ve yolun her oluşturan açıklayan bir özeti. Size en hızlı yolu kullanmak için Ayrıntılar sonraki isteyecek `itineraryId` yanıt.

## <a name="request-fastest-route-itinerary"></a>İstek en hızlı yolu programı

Azure haritalar [alma aktarım programı](https://aka.ms/AzureMapsMobilityTransitItinerary) hizmet istek verilerini rotanın kullanarak belirli bir rota için izin verir **programı kimliği** tarafından döndürülen [alma geçiş rotalar API'si](https://aka.ms/AzureMapsMobilityTransitRoute) hizmeti. İstekte bulunmak için aşağıdaki adımları tamamlayın:

1. Postman içinde tıklayın **yeni istek** | **GET isteği** ve adlandırın **alma geçiş bilgileri**.

2. Oluşturucu sekmesinde **alma** HTTP yöntemi, API uç noktası için aşağıdaki istek URL'sini girin ve tıklatın **Gönder**.

    Ayarlayacağız `detailType` parametresi **geometri** böylece genel aktarım sırasında hem de bırakma tarafından Aç gezinme izlemek ve bisiklet rotanın Bacak durdurma bilgileri yanıt içerir.

    ```HTTP
    https://atlas.microsoft.com/mobility/transit/itinerary/json?api-version=1.0&subscription-key={subscription-key}&query={itineraryId}&detailType=geometry
    ```
    
3. Başarılı bir istek üzerine yanıt yapısı bir aşağıdaki gibi görünmelidir:

    ```JSON
    {
    "departureTime": "2019-05-01T11:16:56-07:00",
    "arrivalTime": "2019-05-01T12:23:45-07:00",
    "legs": [
                {
                    "legType": "Walk",
                    "legStartTime": "2019-05-01T11:16:56-07:00",
                    "legEndTime": "2019-05-01T11:24:06-07:00",
                    "walkingSteps": [
                        {
                            "direction": {
                                "relativeDirection": "left"
                            },
                            "streetName": "Northeast 24th Street"
                        },
                        {
                            "direction": {
                                "relativeDirection": "right"
                            },
                            "streetName": "156th Avenue Northeast"
                        }
                    ],
                    "walkingOrigin": {
                        "latitude": 47.63096,
                        "longitude": -122.126
                    },
                    "walkingDestination": {
                        "latitude": 47.631843,
                        "longitude": -122.132294
                    },
                    "geometry": {
                        "type": "LineString",
                        "coordinates": [
                            [
                                -122.126,
                                47.63096
                            ],
                            [
                                -122.12645,
                                47.63099
                            ],
                            ...,
                            ...,
                            [
                                -122.1323,
                                47.63184
                            ]
                        ]
                    }
                },
                {
                    "legType": "Wait",
                    "legStartTime": "2019-05-01T11:24:06-07:00",
                    "legEndTime": "2019-05-01T11:25:07-07:00",
                    "lineGroup": {
                        "lineGroupId": 666074,
                        "agencyId": 5872,
                        "agencyName": "Metro Transit",
                        "lineNumber": "245",
                        "caption1": "Kirkland Transit Center - Crossroads - Factoria",
                        "caption2": "245 Kirkland Transit Center - Crossroads - Factoria",
                        "color": "347E5D",
                        "transitType": "Bus"
                    },
                    "line": {
                        "lineId": 2756624,
                        "lineGroupId": 666074,
                        "direction": "forward",
                        "agencyId": 5872,
                        "lineNumber": "245",
                        "destination": "Kirkland Crossroads"
                    },
                    "stops": [
                        {
                            "stopId": 2061109,
                            "stopKey": "68788",
                            "stopName": "156th Ave NE & NE 24th St",
                            "position": {
                                "latitude": 47.631844,
                                "longitude": -122.132248
                            },
                            "mainTransitType": "Bus",
                            "mainAgencyId": 5872
                        },
                        {
                            "stopId": 2061059,
                            "stopKey": "68498",
                            "stopName": "156th Ave NE & Overlake Transit Center - Bay 8",
                            "position": {
                                "latitude": 47.643986,
                                "longitude": -122.132187
                            },
                            "mainTransitType": "Bus",
                            "mainAgencyId": 5872
                        }
                    ],
                    "waitOnVehicle": "false"
                },
                {
                    "legType": "Bus",
                    "legStartTime": "2019-05-01T11:25:07-07:00",
                    "legEndTime": "2019-05-01T11:30:00-07:00",
                    "lineGroup": {
                        "lineGroupId": 666074,
                        "agencyId": 5872,
                        "agencyName": "Metro Transit",
                        "lineNumber": "245",
                        "caption1": "Kirkland Transit Center - Crossroads - Factoria",
                        "caption2": "245 Kirkland Transit Center - Crossroads - Factoria",
                        "color": "347E5D",
                        "transitType": "Bus"
                    },
                    "line": {
                        "lineId": 2756624,
                        "lineGroupId": 666074,
                        "direction": "forward",
                        "agencyId": 5872,
                        "lineNumber": "245",
                        "destination": "Kirkland Crossroads"
                    },
                    "stops": [
                        {
                            "stopId": 2061109,
                            "stopKey": "68788",
                            "stopName": "156th Ave NE & NE 24th St",
                            "position": {
                                "latitude": 47.631844,
                                "longitude": -122.132248
                            },
                            "mainTransitType": "Bus",
                            "mainAgencyId": 5872
                        },
                        ...,
                        ...,
                        {
                            "stopId": 2061059,
                            "stopKey": "68498",
                            "stopName": "156th Ave NE & Overlake Transit Center - Bay 8",
                            "position": {
                                "latitude": 47.643986,
                                "longitude": -122.132187
                            },
                            "mainTransitType": "Bus",
                            "mainAgencyId": 5872
                        }
                    ],
                    "geometry": {
                        "type": "LineString",
                        "coordinates": [
                            [
                                -122.13235,
                                47.63184
                            ],
                            ...,
                            ...,
                            [
                                -122.1323,
                                47.64398
                            ]
                        ]
                    }
                },
                ...,
                ...,
                ...,
                {
                    "legType": "Tram",
                    "legStartTime": "2019-05-01T12:20:00-07:00",
                    "legEndTime": "2019-05-01T12:22:00-07:00",
                    "lineGroup": {
                        "lineGroupId": 4083239,
                        "agencyId": 1360766,
                        "agencyName": "Seattle Monorail",
                        "lineNumber": "Monorail",
                        "caption1": "Seattle Center - Westlake Center",
                        "caption2": "MONORAIL Seattle Center - Westlake Center",
                        "color": "00AEEF",
                        "transitType": "Tram"
                    },
                    "line": {
                        "lineId": 3769726,
                        "lineGroupId": 4083239,
                        "direction": "backward",
                        "agencyId": 1360766,
                        "lineNumber": "Monorail",
                        "destination": "Seattle Center"
                    },
                    "stops": [
                        {
                            "stopId": 32962125,
                            "stopName": "Westlake Station",
                            "position": {
                                "latitude": 47.611417,
                                "longitude": -122.337089
                            },
                            "mainTransitType": "Tram",
                            "mainAgencyId": 1360766
                        },
                        {
                            "stopId": 32962134,
                            "stopName": "Seattle Center",
                            "position": {
                                "latitude": 47.62123,
                                "longitude": -122.349746
                            },
                            "mainTransitType": "Tram",
                            "mainAgencyId": 1360766
                        }
                    ],
                    "geometry": {
                        "type": "LineString",
                        "coordinates": [
                            [
                                -122.3369,
                                47.61201
                            ],
                            ...,
                            ...,
                            [
                                -122.34973,
                                47.6212
                            ]
                        ]
                    }
                },
                {
                    "legType": "PathWayWalk",
                    "legStartTime": "2019-05-01T12:22:00-07:00",
                    "legEndTime": "2019-05-01T12:23:45-07:00"
                }
          ]
    }
    ```

## <a name="next-steps"></a>Sonraki adımlar

Mobility hizmetini kullanarak gerçek zamanlı veri isteği işlemleri gerçekleştirmeyi öğreneceksiniz:

> [!div class="nextstepaction"]
> [Gerçek zamanlı veri isteği nasıl](how-to-request-real-time-data.md)

Azure haritalar Mobility hizmeti API belgelerini keşfedin

> [!div class="nextstepaction"]
> [Mobility hizmeti API'si belgeleri](https://aka.ms/AzureMapsMobilityService)

