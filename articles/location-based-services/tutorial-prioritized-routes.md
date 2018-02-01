---
title: "Azure Konum Tabanlı Hizmetler ile birden çok yol tarifi | Microsoft Docs"
description: "Azure Konum Tabanlı Hizmetler’i kullanarak farklı ulaşım yöntemleri için yol tarifi alma"
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
ms.openlocfilehash: 78e911d17fe8c468cf89ec1477f1c5144e6669b6
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
# <a name="find-routes-for-different-modes-of-travel-using-azure-location-based-services"></a>Azure Konum Tabanlı Hizmetler’i kullanarak farklı ulaşım yöntemleri için yol tarifi alma

Bu öğreticide, istediğiniz konuma yönelik ve ulaşım yönteminize göre önceliklendirilmiş yol tarifleri almak için Azure Konum Tabanlı Hizmetler hesabınız ile Yönlendirme Hizmeti SDK’sını nasıl kullanacağınız gösterilmektedir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Yönlendirme Hizmeti sorgunuzu yapılandırma
> * Ulaşım yöntemine göre önceliklendirilmiş yolları işleme

## <a name="prerequisites"></a>Ön koşullar

Devam etmeden önce, [Azure Konum Tabanlı Hizmetler hesabınızı oluşturduğunuzdan](./tutorial-search-location.md#createaccount) ve [hesabınızdan bir anahtar edindiğinizden](./tutorial-search-location.md#getkey) emin olun. Ayrıca, Harita Denetimi ile Arama Hizmeti API’lerini [Azure Konum Tabanlı Hizmetler’i kullanarak yakınlardaki bir konumu arama](./tutorial-search-location.md) öğreticisinde açıklandığı şekilde de kullanabilirsiniz. Dilerseniz, Yönlendirme Hizmeti API’lerini [Azure Konum Tabanlı Hizmetler’i kullanarak yakınlardaki bir konuma yol tarifi alma](./tutorial-route-location.md) öğreticisinde açıklandığı şekilde kullanmayı da öğrenebilirsiniz.


<a id="queryroutes"></a>

## <a name="configure-your-route-service-query"></a>Yönlendirme Hizmeti sorgunuzu yapılandırma

Konum Tabanlı Hizmetler’in Harita Denetimi API’sinin ekli olduğu statik bir HTML sayfası oluşturmak için aşağıdaki adımları izleyin. 

1. Yerel makinenizde yeni bir dosya oluşturun ve bu dosyayı **MapTruckRoute.html** olarak adlandırın. 
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
    HTML üst bilgisinin, Azure Konum Tabanlı Hizmetler kitaplığı için CSS ve JavaScript dosyalarına yönelik kaynak konumlarını eklediğini göz önünde bulundurun. Ayrıca, HTML’nin gövdesine eklenen *betik* segmentinde yer alan Harita Denetimi API’sine erişmeye yönelik satır içi JavaScript kodunu da inceleyin.
3. HTML dosyasının *betik* bloğuna aşağıdaki JavaScript kodunu ekleyin. *<insert-key>* yer tutucusunu Konum Tabanlı Hizmetler hesabınızın birincil anahtarıyla değiştirin.

    ```JavaScript
    // Instantiate map to the div with id "map"
    var LBSAccountKey = "<_your account key_>";
    var map = new atlas.Map("map", {
        "subscription-key": LBSAccountKey
    });
    ```
    Azure Harita Denetimi API’sinin bir bileşeni olan **atlas.Map**, görsel ve etkileşimli bir web haritası için denetim olanağı sunar.

4. Trafik akışı görünümünü haritaya eklemek için, aşağıdaki JavaScript kodunu *betik* bloğuna ekleyin:

    ```JavaScript
    // Add Traffic Flow to the Map
    map.setTraffic({
        flow: "relative"
    });
    ```
    Bu kod, trafik akışını `relative` olacak şekilde ayarlar. Bu, serbest akışa göre trafik hızıdır. Bunu trafik hızı olan `absolute` veya serbest akışla farklılık gösteren bağıl hız `relative-delay` olarak da ayarlayabilirsiniz. 

5. Yolun başlangıç ve bitiş noktalarını sabitlemek için aşağıdaki JavaScript kodunu ekleyin:

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
    Bu kod, yolun başlangıç ve bitiş noktalarını temsil eden iki [GeoJSON nesnesi](https://en.wikipedia.org/wiki/GeoJSON) oluşturur. 

6. _Otomobil_ veya _kamyon_ gibi ulaşım yöntemlerini temel alan yol tariflerini görüntülemek üzere Harita Denetimi’ne *linestrings* katmanları eklemek için aşağıdaki JavaScript kodunu ekleyin.

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

7. Haritaya başlangıç ve bitiş noktalarını eklemek için aşağıdaki JavaScript kodunu ekleyin:

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
    **map.setCameraBounds** API’si, harita penceresini başlangıç ve bitiş noktalarının koordinatlarına göre ayarlar. **map.addPins** API’si, Harita denetimine görsel bileşen olarak nokta ekler.

8. **MapTruckRoute.html** dosyasını makinenize kaydedin. 

<a id="multipleroutes"></a>

## <a name="render-routes-prioritized-by-mode-of-travel"></a>Ulaşım yöntemine göre önceliklendirilmiş yolları işleme

Bu bölümde, bir hedef için belirtilen başlangıç noktasından ulaşım aracınıza bağlı olarak birden fazla yol tarifi almak için Azure Konum Tabanlı Hizmetler’in Yönlendirme Hizmeti API’sini nasıl kullanabileceğiniz açıklanmaktadır. Yönlendirme Hizmeti, gerçek zamanlı trafik durumlarını da göz önünde bulundurarak iki konum arasındaki en hızlı, en kısa ve en ekonomik yolu planlamak amacıyla API’ler sağlar. Ayrıca, Azure’ın geçmişe ait kapsamlı trafik veritabanını kullanarak herhangi bir gün ve saat için yol süresini tahmin eder. Böylece kullanıcıların ileri bir tarih için yol tarifi alabilmesini sağlar. 

1. Yönlendirme Hizmeti’nden kamyon için yol tarifi almak mı istiyorsunuz? Önceki bölümde oluşturulan **MapTruckRoute.html** dosyasını açın ve aşağıdaki JavaScript kodunu *betik* bloğuna ekleyin.

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
    truckRouteUrl += "&subscription-key=" + LBSAccountKey;
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
    Bu kod parçacığı bir [XMLHttpRequest](https://xhr.spec.whatwg.org/) oluşturur ve gelen yanıtları ayrıştırmak için bir olay işleyicisi ekler. Bu, başarılı bir yanıt için, döndürülen yola yönelik koordinat dizisi oluşturur ve bu diziyi haritanın `truckRouteLayerName` katmanına ekler. 
    
    Kod parçacığı, hesap anahtarınız için belirli bir başlangıç ve bitiş noktasına yönelik yol tarifi almak üzere, sorguyu aynı zamanda Yönlendirme Hizmeti’ne de gönderir. Aşağıdaki isteğe bağlı parametreler, ağır bir kamyon için yol tarifini belirlemek için kullanılır: `travelMode=truck` parametresi, ulaşım aracını *kamyon* olarak belirler. Desteklenen diğer ulaşım araçları *taksi*, *otobüs*, *kamyonet*, *motosiklet* ve varsayılan olan *otomobildir*.  
        `vehicleWidth`, `vehicleHeight` ve `vehicleLength` parametreleri, aracın boyutunu metre cinsinden belirtir ve yalnızca ulaşım aracının *kamyon* olması durumunda hesaba katılır.  
        - `vehicleLoadType`, yükün tehlikeli olduğunu belirtir ve aracın bazı yollara çıkmasını kısıtlar. - Bu seçenek de yalnızca ulaşım aracının *kamyon* olması durumunda hesaba katılır.  

2. Yönlendirme Hizmeti’ni kullanarak otomobile yönelik yol tarifi almak için aşağıdaki JavaScript kodunu ekleyin:

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
    carRouteUrl += "&subscription-key=" + LBSAccountKey;
    carRouteUrl += "&query=" + startPoint.coordinates[1] + "," + startPoint.coordinates[0] + ":" +
        destinationPoint.coordinates[1] + "," + destinationPoint.coordinates[0];

    xhttpCar.open("GET", carRouteUrl, true);
    xhttpCar.send();
    ```
    Bu kod parçacığı başka bir [XMLHttpRequest](https://xhr.spec.whatwg.org/) oluşturur ve gelen yanıtları ayrıştırmak için bir olay işleyicisi ekler. Bu, başarılı bir yanıt için, döndürülen yola yönelik koordinat dizisi oluşturur ve bu diziyi haritanın `carRouteLayerName` katmanına ekler. 
    
    Kod parçacığı, hesap anahtarınız için belirli bir başlangıç ve bitiş noktasına yönelik yol tarifi almak üzere sorguyu aynı zamanda Yönlendirme Hizmeti’ne de gönderir. Başka bir parametre kullanılmadığından, varsayılan ulaşım aracı olan *otomobil* için hesaplanan yol tarifi döndürülür. 

3. **MapTruckRoute.html** dosyasını yerel olarak kaydedin, ardından dosyayı dilediğiniz web tarayıcısında açarak sonucu inceleyin. Konum Tabanlı Hizmetler API’leriyle başarılı bir şekilde bağlantı kurulması için aşağıdakine benzer bir harita görürsünüz. 

    ![Azure Yönlendirme Hizmeti ile önceliklendirilen yollar](./media/tutorial-prioritized-routes/lbs-prioritized-routes.png)

    Kamyon yolunun mavi, otomobil yolunun mor renkli olduğunu göz önünde bulundurun.

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Yönlendirme Hizmeti sorgunuzu yapılandırma
> * Ulaşım yöntemine göre önceliklendirilmiş yolları işleme

Azure Konum Tabanlı Hizmetler SDK’sı hakkında daha fazla bilgi edinmek için **Kavramlar** ve **Nasıl Yapılır** makalelerini inceleyin. 
