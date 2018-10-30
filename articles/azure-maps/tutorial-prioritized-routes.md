---
title: Azure Haritalar ile birden fazla yol | Microsoft Docs
description: Azure Haritalar’ı kullanarak farklı seyahat modları için yolları bulma
author: walsehgal
ms.author: v-musehg
ms.date: 10/22/2018
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 864f662cd6be3c5929166db92f2dad92b9c6586e
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2018
ms.locfileid: "49648216"
---
# <a name="find-routes-for-different-modes-of-travel-using-azure-maps"></a>Azure Haritalar’ı kullanarak farklı seyahat modları için yolları bulma

Bu öğreticide, istediğiniz konuma yönelik ve ulaşım yönteminize göre önceliklendirilmiş yol tarifleri almak için Azure Haritalar hesabınız ile yol hizmetini nasıl kullanacağınız gösterilmektedir. Haritanızda, biri arabalar, diğeri ise yükseklik, ağırlık ya da tehlikeli yük nedeniyle yol kısıtlaması olabilecek kamyonlar için olmak üzere iki farklı yol görüntüleyin. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Harita denetimi API’sini kullanarak yeni bir web sayfası oluşturma
> * Haritanızda trafik akışını görselleştirme
> * Seyahat modunu bildiren yol sorguları oluşturma
> * Haritanızda birden fazla yol görüntüleme

## <a name="prerequisites"></a>Ön koşullar

Devam etmeden önce, [Azure Haritalar hesabınızı oluşturmak](./tutorial-search-location.md#createaccount) ve [hesabınızın abonelik anahtarını almak](./tutorial-search-location.md#getkey) için ilk öğreticide yer alan adımları izleyin.

## <a name="create-a-new-map"></a>Yeni harita oluşturma

Aşağıdaki adımlarda, Harita Denetimi API’sinin tümleşik olduğu statik bir HTML sayfasının nasıl oluşturulacağı gösterilmektedir.

1. Yerel makinenizde yeni bir dosya oluşturun ve bu dosyayı **MapTruckRoute.html** olarak adlandırın.
2. Aşağıdaki HTML bileşenlerini dosyaya ekleyin:

    ```HTML
    <!DOCTYPE html>
    <html lang="en">

    <head>
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width, user-scalable=no" />
        <title>Map Truck Route</title>
        <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/css/atlas.min.css?api-version=1" type="text/css" />
        <script src="https://atlas.microsoft.com/sdk/js/atlas.min.js?api-version=1"></script>
        <script src="https://atlas.microsoft.com/sdk/js/atlas-service.min.js?api-version=1"></script>
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
    HTML üst bilgisi, Azure Haritalar kitaplığı için CSS ve JavaScript dosyalarına yönelik kaynak konumlarını ekler. HTML dosyasının gövdesindeki *betik* segmenti, haritanın satır içi JavaScript kodunu içerir.

3. HTML dosyasının *betik* bloğuna aşağıdaki JavaScript kodunu ekleyin. **\<Hesap anahtarınız\>** dizesini, Haritalar hesabınızdan kopyaladığınız birincil anahtarla değiştirin. Haritaya nereye odaklanacağını söylemezseniz tam dünya görünümünü görürsünüz. Bu kod, haritanın merkez noktasını ayarlar ve varsayılan olarak belirli bir alana odaklanabilmeniz için bir yakınlaştırma düzeyi bildirir.

    ```JavaScript
    // Instantiate map to the div with id "map"
    var mapCenterPosition = [-73.985708, 40.75773];
    atlas.setSubscriptionKey("<your account key>");
    var map = new atlas.Map("map", {
      center: mapCenterPosition,
      zoom: 11
    });
    ```
    Azure Harita Denetimi API’sinin bir bileşeni olan **atlas.Map**, görsel ve etkileşimli bir web haritası için denetim olanağı sunar.

4. Dosyayı kaydedin ve tarayıcınızda açın. Bu noktada, daha fazla geliştirebileceğiniz temel bir haritanız olur. 

   ![Temel haritayı görüntüleme](./media/tutorial-prioritized-routes/basic-map.png)

## <a name="visualize-traffic-flow"></a>Trafik akışını görselleştirme

1. Haritaya trafik akışı görüntüsünü ekleyin.  **map.events.add**, harita tamamen yüklendikten sonra haritaya eklenmiş olan tüm harita işlevlerinin yüklenmesini sağlar.

    ```JavaScript
    map.events.add("load", function() {
        // Add Traffic Flow to the Map
        map.setTraffic({
            flow: "relative"
        });
    });
    ```
    Bu kod, trafik akışını `relative` olacak şekilde ayarlar. Bu, serbest akışa göre trafik hızıdır. Bunu trafik hızı olan `absolute` veya serbest akışla farklılık gösteren bağıl hız `relative-delay` olarak da ayarlayabilirsiniz.

2. **MapTruckRoute.html** dosyasını kaydedin ve tarayıcınızın sayfasını yenileyin. Geçerli trafik verileriyle birlikte Los Angeles sokaklarını görebilirsiniz.

   ![Trafik haritasını görüntüleme](./media/tutorial-prioritized-routes/traffic-map.png)

<a id="queryroutes"></a>

## <a name="set-start-and-end-points"></a>Başlangıç ve bitiş noktalarını ayarlama

Bu öğreticide başlangıç noktasını Seattle’daki Fabrikam adlı hayali şirket, varış noktasını ise bir Microsoft ofisi olarak ayarlayın.

1. Yolun başlangıç ve bitiş noktalarını sabitlemek için aşağıdaki JavaScript kodunu ekleyin:

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

2. Haritaya başlangıç ve bitiş noktalarını eklemek için aşağıdaki JavaScript kodunu ekleyin:

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
    
    map.events.add("load", function() { 
        // Add pins to the map for the start and end point of the route
        map.addPins([startPin, destinationPin], {
            name: "route-pins",
            textFont: "SegoeUi-Regular",
            textOffset: [0, -20]
        });
    });
    ```
    **map.setCameraBounds** çağrısı, harita penceresini başlangıç ve bitiş noktalarının koordinatlarına göre ayarlar. **map.events.add**, harita tamamen yüklendikten sonra haritaya eklenmiş olan tüm harita işlevlerinin yüklenmesini sağlar. **map.addPins** API’si, Harita denetimine görsel bileşen olarak nokta ekler.

3. Dosyayı kaydedin ve iğneleri haritanızda görmek için tarayıcınızı yenileyin. Bildirdiğiniz haritanın merkez noktasının Los Angeles’ta olmasına rağmen **map.setCameraBounds**, başlangıç ve bitiş noktalarını göstermek için görünümü taşımıştır.

   ![Başlangıç ve bitiş noktaları ile haritayı görüntüleme](./media/tutorial-prioritized-routes/pins-map.png)

<a id="multipleroutes"></a>

## <a name="render-routes-prioritized-by-mode-of-travel"></a>Ulaşım yöntemine göre önceliklendirilmiş yolları işleme

Bu bölümde, bir hedef için belirtilen başlangıç noktasından ulaşım aracınıza bağlı olarak birden fazla yol tarifi almak için Azure Haritalar yol hizmeti API’sini nasıl kullanabileceğiniz açıklanmaktadır. Yönlendirme hizmeti, mevcut trafik koşullarını göz önünde bulundurarak iki konum arasındaki *en hızlı*, *en kısa*, *ekonomik* veya *heyecan verici* yolları planlamak için API’ler sağlar. Ayrıca, Azure’ın geçmişe ait kapsamlı trafik veritabanını kullanarak herhangi bir gün ve saat için yol süresini tahmin eder. Böylece kullanıcıların ileri bir tarih için yol tarifi alabilmesini sağlar. Daha fazla bilgi için bkz. [Yol tariflerini alma](https://docs.microsoft.com/rest/api/maps/route/getroutedirections). Aşağıdaki kod bloklarının harita tamamen yüklendikten sonra yüklendiğinden emin olmak için tümü **map load eventListener** içine eklenmelidir.

1. İlk olarak, rota yolunu veya *linestring*’i görüntülemek için haritaya yeni bir katman ekleyin. Bu öğreticide, her biri kendi stiline sahip olan **araba yolu** ve **kamyon yolu** şeklinde iki farklı yol vardır. *Betik* bloğuna aşağıdaki JavaScript kodunu ekleyin:

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

2. Bir kamyonun yolunu istemek ve sonuçları harita üzerinde göstermek için aşağıdaki JavaScript kodunu *betik* bloğuna ekleyin:

    ```JavaScript
    // Instantiate the service client  
    var client = new atlas.service.Client(MapsAccountKey);

    // Construct the route query string
    var routeQuery = startPoint.coordinates[1] +
        "," +
        startPoint.coordinates[0] +
        ":" +
        destinationPoint.coordinates[1] +
        "," +
        destinationPoint.coordinates[0];

    // Execute the truck route query then add the route to the map once a response is received  
    client.route.getRouteDirections(routeQuery, {
        travelMode: "truck",
        vehicleWidth: 2,
        vehicleHeight: 2,
        vehicleLength: 5,
        vehicleLoadType: "USHazmatClass2"
    }).then(response => {
        // Parse the response into GeoJSON
        var geoJsonResponse = new atlas.service.geojson
            .GeoJsonRouteDirectionsResponse(response);

        // Get the first in the array of routes and add it to the map
        map.addLinestrings([geoJsonResponse.getGeoJsonRoutes().features[0]], {
            name: truckRouteLayerName
        });
    });
    ```
    Yukarıdaki kod parçacığı bir hizmet istemcisi başlatır ve rota sorgusu dizesi oluşturur. Ardından [getRouteDirections](https://docs.microsoft.com/javascript/api/azure-maps-rest/services.route?view=azure-iot-typescript-latest#getroutedirections) yöntemi aracılığıyla Azure Haritalar yönlendirme hizmetini sorgular ve sonra [getGeoJsonRouteDirectionsResponse](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.geojson.geojsonroutedirectionsresponse?view=azure-iot-typescript-latest) kullanarak yanıtı GeoJSON biçiminde ayrıştırır. Daha sonra döndürülen yola yönelik koordinat dizisi oluşturur ve bu diziyi haritanın `truckRouteLayerName` katmanına ekler.

3. Bir arabanın yolunu istemek ve sonuçları göstermek için aşağıdaki JavaScript kodunu ekleyin:

    ```JavaScript
    // Execute the car route query then add the route to the map once a response is received  
    client.route.getRouteDirections(routeQuery).then(response => {
        // Parse the response into GeoJSON
        var geoJsonResponse = new atlas.service.geojson
            .GeoJsonRouteDiraectionsResponse(response);

        // Get the first in the array of routes and add it to the map 
        map.addLinestrings([geoJsonResponse.getGeoJsonRoutes().features[0]], {
            name: carRouteLayerName
        });
    });
    ```
    Bu kod parçacığı kamyon rota sorgusunu bir araba için kullanır. [getRouteDirections](https://docs.microsoft.com/javascript/api/azure-maps-rest/services.route?view=azure-iot-typescript-latest#getroutedirections) yöntemi aracılığıyla Azure Haritalar yönlendirme hizmetini sorgular ve sonra [getGeoJsonRouteDirectionsResponse](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.geojson.geojsonroutedirectionsresponse?view=azure-iot-typescript-latest) kullanarak yanıtı GeoJSON biçiminde ayrıştırır. Daha sonra döndürülen yola yönelik koordinat dizisi oluşturur ve bu diziyi haritanın `carRouteLayerName` katmanına ekler.

4. **MapTruckRoute.html** dosyasını kaydedin ve tarayıcınızı yenileyerek sonucu gözlemleyin. Haritalar API’leriyle başarılı bir şekilde bağlantı kurulması için aşağıdakine benzer bir harita görürsünüz.

    ![Azure Yönlendirme Hizmeti ile önceliklendirilen yollar](./media/tutorial-prioritized-routes/prioritized-routes.png)

    Kamyon yolu mavi ve daha kalınken araba yolu mor ve daha ince gösterilir. Araba yolu konut alanlarının altındaki tünellerden geçen I-90 üzerinden Washington Gölü’nü geçer ve bu nedenle tehlikeli atık yükünü kısıtlar. USHazmatClass2 yük tipini belirten kamyon yolu ise farklı bir otoyol kullanmak üzere doğru şekilde yönlendirilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Harita denetimi API’sini kullanarak yeni bir web sayfası oluşturma
> * Haritanızda trafik akışını görselleştirme
> * Seyahat modunu bildiren yol sorguları oluşturma
> * Haritanızda birden fazla yol görüntüleme

Bu öğreticiye ait kod örneğine şuradan erişebilirsiniz:

> [Azure Haritalar ile birden fazla yol](https://github.com/Azure-Samples/azure-maps-samples/blob/master/src/truckRoute.html)

Azure Haritalar'ın kapsamı ve özellikleri hakkında daha fazla bilgi edinmek için:

> [!div class="nextstepaction"]
> [Yakınlaştırma düzeyleri ve kutucuk kılavuzu](zoom-levels-and-tile-grid.md)

Daha fazla kod örneği ve etkileşimli bir kodlama deneyimi için:

> [!div class="nextstepaction"]
> [Harita denetimini kullanma](how-to-use-map-control.md)