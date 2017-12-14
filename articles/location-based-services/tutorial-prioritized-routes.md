---
title: "Azure konum tabanlı Hizmetleri olan birden çok yol | Microsoft Docs"
description: "Farklı Azure konum tabanlı Hizmetleri kullanarak seyahat modu yolları Bul"
services: location-based-services
keywords: 
author: dsk-2015
ms.author: dkshir
ms.date: 11/28/2017
ms.topic: tutorial
ms.service: location-based-services
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 7d8eb900bdc90a391d4121b7bfb863fc274fc564
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2017
---
# <a name="find-routes-for-different-modes-of-travel-using-azure-location-based-services"></a>Farklı Azure konum tabanlı Hizmetleri kullanarak seyahat modu yolları Bul

Bu öğretici Azure konum tabanlı Hizmetleri hesabınızı ve rota hizmeti SDK'sı noktanızın seyahat modunuzu göre öncelik ilgi rotayı bulmak için nasıl kullanılacağını gösterir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Rota hizmeti sorgunuzu yapılandırın
> * Seyahat modu tarafından öncelik yollar oluşturma

## <a name="prerequisites"></a>Ön koşullar

Devam etmeden önce emin olun [Azure konum tabanlı Hizmetleri hesabınızı oluşturmak](./tutorial-search-location.md#createaccount), ve [hesabınız için abonelik anahtarı alma](./tutorial-search-location.md#getkey). Harita denetiminin ve arama hizmeti API'leri öğreticide anlatıldığı gibi nasıl kullanılacağını da gözlemleyebilirsiniz [arama ilgi çekici Azure konum tabanlı Hizmetleri kullanarak yakındaki](./tutorial-search-location.md), yanı sıra temel rota hizmeti API'ları kullanımını öğrenin öğreticide ele alınan [Azure konum tabanlı Hizmetleri kullanarak ilgi rotaya](./tutorial-route-location.md).


<a id="queryroutes"></a>

## <a name="configure-your-route-service-query"></a>Rota hizmeti sorgunuzu yapılandırın

Konum tabanlı hizmetlerin Harita Denetim API'si ile katıştırılmış bir statik HTML sayfası oluşturmak için aşağıdaki adımları kullanın. 

1. Yerel makinenizde yeni bir dosya oluşturun ve adlandırın **MapTruckRoute.html**. 
2. Aşağıdaki HTML bileşenlerini dosyaya ekleyin:

    ```HTML
    <!DOCTYPE html>
    <html lang="en">

    <head>
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width, user-scalable=no" />
        <title>Map Truck Route</title>
        <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/css/atlas.min.css?api-version=1.0" type="text/css" />
        <script src="https://atlas.microsoft.com/sdk/js/atlas.min.js?api-version=1.0"></script>
        <style>
            html,
            body {
                width: 100%;
                height: 100%;
                padding: 0;
                margin: 0;
            }

            #map {
                width: 100%;
                height: 100%;
            }
        </style>
    </head>
    
    <body>
        <div id="map"></div>
        <script>
            // Embed Map Control JavaScript code here
        </script>
    </body>

    </html>
    ```
    HTML üstbilgisi Azure konum tabanlı Hizmetleri kitaplığı CSS ve JavaScript dosyaları için kaynak konumlarını katıştırır unutmayın. Ayrıca fark *betik* segment satır içi Azure Harita Denetim API'si erişmek için JavaScript kodu içeren HTML, gövdesine eklenir.
3. Şu JavaScript kodunu eklemek *betik* HTML dosyası bloğu. Yer tutucu Değiştir *< anahtar Ekle >* konum tabanlı Hizmetleri hesabınızın birincil anahtara sahip.

    ```JavaScript
    // Instantiate map to the div with id "map"
    var subscriptionKey = "<insert-key>";
    var map = new atlas.Map("map", {
        "subscription-key": subscriptionKey
    });
    ```
    **Atlas. Harita** denetim için bir görsel ve etkileşimli web eşleme sağlar ve Azure Harita Denetim API'si bir bileşenidir.

4. Şu JavaScript kodunu eklemek *betik* bloğu, trafik akışı görünen eşlemesine eklemek için:

    ```JavaScript
    // Add Traffic Flow to the Map
    map.setTraffic({
        flow: "relative"
    });
    ```
    Bu kod trafik akışı ayarlar `relative`, serbest akış göreli yol hızına olduğu. Ayrıca ayarlamanız `absolute` yol hızına veya `relative-delay` serbest akış göreli hızından nerede farklı görüntüler. 

5. Başlangıç ve bitiş noktalarını rotanın için PIN oluşturmak için aşağıdaki JavaScript kodu ekleyin:

    ```JavaScript
    // Create the GeoJSON objects which represent the start and end point of the route
    var startPoint = new atlas.data.Point([-122.356099, 47.580045]);
    var startPin = new atlas.data.Feature(startPoint, {
        title: "Fabrikam, Inc.",
        icon: "pin-round-blue"
    });

    var destinationPoint = new atlas.data.Point([-122.130137, 47.644702]);
    var destinationPin = new atlas.data.Feature(destinationPoint, {
        title: "Microsoft",
        icon: "pin-blue"
    });
    ```
    Bu kod iki oluşturur [GeoJSON nesne](https://en.wikipedia.org/wiki/GeoJSON) başlangıç ve bitiş noktalarını rotanın temsil etmek için. 

6. Katmanları eklemek için şu JavaScript kodunu eklemek *MultiPoint* taşıma, örneğin, Modu'na bağlı yolları görüntüler için harita denetimine _araba_ ve _kamyon_.

    ```JavaScript
    // Place route layers on the map
    var carRouteLayerName = "car-route";
    map.addLinestrings([], {
        name: carRouteLayerName,
        color: "#B76DAB",
        width: 5,
        cap: "round",
        join: "round",
        before: "labels"
    });

    var truckRouteLayerName = "truck-route";
    map.addLinestrings([], {
        name: truckRouteLayerName,
        color: "#2272B9",
        width: 9,
        cap: "round",
        join: "round",
        before: carRouteLayerName
    });
    ```

7. Başlangıç ve bitiş noktalarını eşlemesine eklemek için aşağıdaki JavaScript kodu ekleyin:

    ```JavaScript
    // Fit the map window to the bounding box defined by the start and destination points
    var swLon = Math.min(startPoint.coordinates[0], destinationPoint.coordinates[0]);
    var swLat = Math.min(startPoint.coordinates[1], destinationPoint.coordinates[1]);
    var neLon = Math.max(startPoint.coordinates[0], destinationPoint.coordinates[0]);
    var neLat = Math.max(startPoint.coordinates[1], destinationPoint.coordinates[1]);
    map.setCameraBounds({
        bounds: [swLon, swLat, neLon, neLat],
        padding: 100
    });

    // Add pins to the map for the start and end point of the route
    map.addPins([startPin, destinationPin], {
        name: "route-pins",
        textFont: "SegoeUi-Regular",
        textOffset: [0, -20]
    });
    ``` 
    API **map.setCameraBounds** Haritası penceresini başlangıç ve bitiş noktası koordinatları göre ayarlar. API **map.addPins** görsel bileşenleri olarak harita denetim noktaları ekler.

8. Kaydet **MapTruckRoute.html** makinenizde dosya. 

<a id="multipleroutes"></a>

## <a name="render-routes-prioritized-by-mode-of-travel"></a>Seyahat modu tarafından öncelik yollar oluşturma

Bu bölümde Azure konum tabanlı hizmetlerin rota hizmeti API'si, aktarım modu üzerinde temel bir hedefe birden çok yol verilen başından noktası bulmak için nasıl kullanılacağını gösterir. Rota hizmet en hızlı, kısa plana API'leri veya gerçek zamanlı trafiği koşulları dikkate iki konum arasında ekonomik yolu sağlar. Ayrıca, kullanıcıların Azure'nın kapsamlı geçmiş trafik veritabanı kullanarak ve her gün ve saat için rota sürelerini tahmin etmeye gelecekte yollar planlama imkan tanır. 

1. Açık **MapTruckRoute.html** dosyası önceki bölümde oluşturulan ve şu JavaScript kodunu ekleyin *betik* rota hizmetini kullanarak bir kamyon için rota almak için blok.

    ```JavaScript
    // Perform a request to the route service and draw the resulting truck route on the map
    var xhttpTruck = new XMLHttpRequest();
    xhttpTruck.onreadystatechange = function () {
        if (this.readyState == 4 && this.status == 200) {
            var response = JSON.parse(this.responseText);

            var route = response.routes[0];
            var routeCoordinates = [];
            for (var leg of route.legs) {
                var legCoordinates = leg.points.map((point) => [point.longitude, point.latitude]);
                routeCoordinates = routeCoordinates.concat(legCoordinates);
            }

            var routeLinestring = new atlas.data.LineString(routeCoordinates);
            map.addLinestrings([new atlas.data.Feature(routeLinestring)], {
                name: truckRouteLayerName
            });
        }
    };

    var truckRouteUrl = "https://atlas.microsoft.com/route/directions/json?";
    truckRouteUrl += "&api-version=1.0";
    truckRouteUrl += "&subscription-key=" + subscriptionKey;
    truckRouteUrl += "&query=" + startPoint.coordinates[1] + "," + startPoint.coordinates[0] + ":" +
        destinationPoint.coordinates[1] + "," + destinationPoint.coordinates[0];
    truckRouteUrl += "&travelMode=truck";
    truckRouteUrl += "&vehicleWidth=2";
    truckRouteUrl += "&vehicleHeight=2";
    truckRouteUrl += "&vehicleLength=5";
    truckRouteUrl += "&vehicleLoadType=USHazmatClass2";

    xhttpTruck.open("GET", truckRouteUrl, true);
    xhttpTruck.send();
    ```
    Bu kod parçacığını oluşturur bir [XMLHttpRequest](https://xhr.spec.whatwg.org/), ve gelen yanıtı ayrıştırılamadı olay işleyicisi ekler. Başarılı bir yanıt için bu koordinatları döndürülen yol için bir dizi oluşturur ve haritanın ekler `truckRouteLayerName` katmanı. 
    
    Bu kod parçacığını yol için belirtilen başlangıç ve bitiş noktası hesabınızın abonelik anahtarı için rota almak için hizmet ayrıca sorgusu gönderir. Aşağıdaki isteğe bağlı parametreleri ağır kamyon için yol göstermek için kullanılır:-parametresi `travelMode=truck` seyahat modunu belirtir *kamyon*. Diğer modlar desteklenen seyahat *ücreti*, *veri yolu*, *van*, *motosikletinizin*ve varsayılan *araba* . 
        -Parametreleri `vehicleWidth`, `vehicleHeight`, ve `vehicleLength` metre araç boyutları belirtmek ve yalnızca seyahat modunda olup olmadığını dikkate *kamyon*. 
        - `vehicleLoadType` Kargo tehlikeli ve bazı yollar üzerinde kısıtlı olarak sınıflandırır. Bu da şu anda yalnızca değerlendirilir *kamyon* modu. 

2. Rota hizmetini kullanarak bir araba için rota almak için aşağıdaki JavaScript kodu ekleyin:

    ```JavaScript
    // Perform a request to the route service and draw the resulting car route on the map
    var xhttpCar = new XMLHttpRequest();
    xhttpCar.onreadystatechange = function () {
        if (this.readyState == 4 && this.status == 200) {
            var response = JSON.parse(this.responseText);

            var route = response.routes[0];
            var routeCoordinates = [];
            for (var leg of route.legs) {
                var legCoordinates = leg.points.map((point) => [point.longitude, point.latitude]);
                routeCoordinates = routeCoordinates.concat(legCoordinates);
            }

            var routeLinestring = new atlas.data.LineString(routeCoordinates);
            map.addLinestrings([new atlas.data.Feature(routeLinestring)], {
                name: carRouteLayerName
            });
        }
    };

    var carRouteUrl = "https://atlas.microsoft.com/route/directions/json?";
    carRouteUrl += "&api-version=1.0";
    carRouteUrl += "&subscription-key=" + subscriptionKey;
    carRouteUrl += "&query=" + startPoint.coordinates[1] + "," + startPoint.coordinates[0] + ":" +
        destinationPoint.coordinates[1] + "," + destinationPoint.coordinates[0];

    xhttpCar.open("GET", carRouteUrl, true);
    xhttpCar.send();
    ```
    Bu kod parçacığını başka oluşturur [XMLHttpRequest](https://xhr.spec.whatwg.org/), ve gelen yanıtı ayrıştırılamadı olay işleyicisi ekler. Başarılı bir yanıt için bu koordinatları döndürülen yol için bir dizi oluşturur ve haritanın ekler `carRouteLayerName` katmanı. 
    
    Bu kod parçacığında belirtilen başlangıç ve bitiş noktası hesabınızın abonelik anahtarı için rota almak için rota hizmeti de sorgusu gönderir. Diğer bir parametreleri kullanıldığından rota için varsayılan modu seyahat olan *araba* döndürülür. 

3. Kaydet **MapTruckRoute.html** yerel olarak dosya sonra tercih ettiğiniz bir web tarayıcısında açın ve sonucu inceleyin. Konum tabanlı hizmetlerin API'leri ile başarılı bir bağlantı için aşağıdakine benzer bir harita görmeniz gerekir. 

    ![Azure yol hizmetiyle öncelikli yolları](./media/tutorial-prioritized-routes/lbs-prioritized-routes.png)

    Araba rotası mor durumdayken kamyon rota mavi renkte olduğuna dikkat edin.

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Rota hizmeti sorgunuzu yapılandırın
> * Seyahat modu tarafından öncelik yollar oluşturma

İle devam **kavramları** ve **nasıl yapılır** makaleler Azure konum tabanlı Hizmetleri SDK derinlemesine öğrenin. 
