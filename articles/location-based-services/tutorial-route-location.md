---
title: "Azure Konum Tabanlı Hizmetler ile yol tarifi alma | Microsoft Docs"
description: "Azure Konum Tabanlı Hizmetler ile istenen konuma yol tarifi alma"
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
ms.openlocfilehash: 7303347444952d9c09dc6c04eea5b962e18729b4
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
# <a name="route-to-a-point-of-interest-using-azure-location-based-services"></a>Azure Konum Tabanlı Hizmetler ile istenen konuma yol tarifi alma

Bu öğreticide, Azure Konum Tabanlı Hizmetler hesabı ile Yönlendirme Hizmeti SDK’nızı kullanarak istenen bir konuma nasıl yol tarifi alabileceğinizden bahsedilmektedir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Adres koordinatlarını öğrenme
> * İstenen konuma yol tarifi almak için Sorgu Yönlendirme Hizmeti

## <a name="prerequisites"></a>Ön koşullar

Devam etmeden önce, [Azure Konum Tabanlı Hizmetler hesabınızı oluşturduğunuzdan](./tutorial-search-location.md#createaccount) ve [hesabınızın abonelik anahtarını edindiğinizden](./tutorial-search-location.md#getkey) emin olun. Ayrıca, [Azure Konum Tabanlı Hizmetler’i kullanarak yakınlardaki bir konuma yol tarifi alma](./tutorial-search-location.md) öğreticisinde açıklanan Harita Denetimi ve Arama Hizmeti API’lerini nasıl kullanacağınızı da öğrenebilirsiniz.


<a id="getcoordinates"></a>

## <a name="get-address-coordinates"></a>Adres koordinatlarını öğrenme

Konum Tabanlı Hizmetler’in Harita Denetimi API’sinin ekli olduğu statik bir HTML sayfası oluşturmak için aşağıdaki adımları izleyin. 

1. Yerel makinenizde yeni bir dosya oluşturun ve bu dosyayı **MapRoute.html** olarak adlandırın. 
2. Aşağıdaki HTML bileşenlerini dosyaya ekleyin:

    ```HTML
    <!DOCTYPE html>
    <html lang="en">

    <head>
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width, user-scalable=no" />
        <title>Map Route</title>
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
    HTML üst bilgisinin, Azure Konum Tabanlı Hizmetler kitaplığı için CSS ve JavaScript dosyalarına yönelik kaynak konumlarını nasıl eklediğini dikkate alın. Ayrıca, HTML dosyasının gövdesindeki *betik* segmentinde yer alan Azure Konum Tabanlı Hizmet’in API’lerine erişmeye yönelik satır içi JavaScript kodunu da inceleyin.

3. HTML dosyasının *betik* bloğuna aşağıdaki JavaScript kodunu ekleyin. Betikteki Konum Tabanlı Hizmetler hesabınızın birincil anahtarını kullanın.

    ```JavaScript
    // Instantiate map to the div with id "map"
    var LBSAccountKey = "<_your account key_>";
    var map = new atlas.Map("map", {
        "subscription-key": LBSAccountKey
    });
    ```
    Azure Harita Denetimi API’sinin bir bileşeni olan **atlas.Map**, görsel ve etkileşimli bir web haritası için denetim olanağı sunar.

4. *Betik* bloğuna aşağıdaki JavaScript kodunu ekleyin. Bu işlem, yolu göstermek üzere Harita Denetimi’ne *linestrings* katmanı ekler:

    ```JavaScript
    // Initialize the linestring layer for routes on the map
    var routeLinesLayerName = "routes";
    map.addLinestrings([], {
        name: routeLinesLayerName,
        color: "#2272B9",
        width: 5,
        cap: "round",
        join: "round",
        before: "labels"
    });
    ```

5. Yol için başlangıç ve bitiş noktaları oluşturmak üzere aşağıdaki JavaScript kodunu ekleyin:

    ```JavaScript
    // Create the GeoJSON objects which represent the start and end point of the route
    var startPoint = new atlas.data.Point([-122.130137, 47.644702]);
    var startPin = new atlas.data.Feature(startPoint, {
        title: "Microsoft",
        icon: "pin-round-blue"
    });

    var destinationPoint = new atlas.data.Point([-122.3352, 47.61397]);
    var destinationPin = new atlas.data.Feature(destinationPoint, {
        title: "Contoso Oil & Gas",
        icon: "pin-blue"
    });
    ```
    Bu kod, yolun başlangıç ve bitiş noktalarını temsil eden iki [GeoJSON nesnesi](https://en.wikipedia.org/wiki/GeoJSON) oluşturur. Bitiş noktası, önceki [Azure Konum Tabanlı Hizmetler’i kullanarak yakınlarda bir konumu arama *öğreticisinde belirtilen bir* benzin istasyonunun](./tutorial-search-location.md) enlem/boylam kombinasyonudur.

6. Haritaya başlangıç ve bitiş noktalarını sabitlemek için aşağıdaki JavaScript kodunu ekleyin:

    ```JavaScript
    // Fit the map window to the bounding box defined by the start and destination points
    var swLon = Math.min(startPoint.coordinates[0], destinationPoint.coordinates[0]);
    var swLat = Math.min(startPoint.coordinates[1], destinationPoint.coordinates[1]);
    var neLon = Math.max(startPoint.coordinates[0], destinationPoint.coordinates[0]);
    var neLat = Math.max(startPoint.coordinates[1], destinationPoint.coordinates[1]);
    map.setCameraBounds({
        bounds: [swLon, swLat, neLon, neLat],
        padding: 50
    });

    // Add pins to the map for the start and end point of the route
    map.addPins([startPin, destinationPin], {
        name: "route-pins",
        textFont: "SegoeUi-Regular",
        textOffset: [0, -20]
    });
    ``` 
    **map.setCameraBounds** API’si, harita penceresini başlangıç ve bitiş noktalarının koordinatlarına göre ayarlar. **map.addPins** API’si, Harita denetimine görsel bileşen olarak nokta ekler.

7. **MapRoute.html** dosyasını makinenize kaydedin. 

<a id="getroute"></a>

## <a name="query-route-service-for-directions-to-point-of-interest"></a>İstenen konuma yol tarifi almak için Sorgu Yönlendirme Hizmeti

Bu bölümde, bir hedef için belirtilen başlangıç noktasından yol bulmak için Azure Konum Tabanlı Hizmetler’in Yönlendirme Hizmeti API’sini nasıl kullanabileceğiniz açıklanmaktadır. Yönlendirme Hizmeti, gerçek zamanlı trafik durumlarını da göz önünde bulundurarak iki konum arasındaki en hızlı, en kısa ve en ekonomik yolu planlamak amacıyla API’ler sağlar. Ayrıca, Azure’ın geçmişe ait kapsamlı trafik veritabanını kullanarak herhangi bir gün ve saat için yol süresini tahmin eder. Böylece kullanıcıların ileri bir tarih için yol tarifi alabilmesini sağlar. 

1. Yönlendirme Hizmeti’ni göstermek için, önceki bölümde oluşturulan **MapRoute.html** dosyasını açın ve aşağıdaki JavaScript kodunu *betik* bloğuna ekleyin.

    ```JavaScript
    // Perform a request to the route service and draw the resulting route on the map
    var xhttp = new XMLHttpRequest();
    xhttp.onreadystatechange = function () {
        if (this.readyState == 4 && this.status == 200) {
            var response = JSON.parse(xhttp.responseText);

            var route = response.routes[0];
            var routeCoordinates = [];
            for (var leg of route.legs) {
                var legCoordinates = leg.points.map((point) => [point.longitude, point.latitude]);
                routeCoordinates = routeCoordinates.concat(legCoordinates);
            }

            var routeLinestring = new atlas.data.LineString(routeCoordinates);
            map.addLinestrings([new atlas.data.Feature(routeLinestring)], { name: routeLinesLayerName });
        }
    };
    ```
    Bu kod parçacığı bir [XMLHttpRequest](https://xhr.spec.whatwg.org/) oluşturur ve gelen yanıtları ayrıştırmak için bir olay işleyicisi ekler. Başarılı bir yanıt için, döndürülen ilk yolun doğru parçaları için bir koordinat dizisi oluşturur. Daha sonra, haritanın *linestrings* katmanı için şu koordinat kümesini ekler.

2. Aşağıdaki kodu *betik* bloğuna ekleyerek XMLHttpRequest’i Azure Konum Tabanlı Hizmetler’in Yönlendirme Hizmeti’ne gönderin:

    ```JavaScript
    var url = "https://atlas.microsoft.com/route/directions/json?";
    url += "&api-version=1.0";
    url += "&subscription-key=" + LBSAccountKey;
    url += "&query=" + startPoint.coordinates[1] + "," + startPoint.coordinates[0] + ":" +
        destinationPoint.coordinates[1] + "," + destinationPoint.coordinates[0];

    xhttp.open("GET", url, true);
    xhttp.send();
    ```
    Yukarıdaki istek, belirtilme sırasına göre hesap anahtarınız ve başlangıç ile bitiş noktalarının koordinatları olan gerekli parametreleri gösterir. 

3. **MapRoute.html** dosyasını yerel olarak kaydedin, sonra dosyayı dilediğiniz web tarayıcısında açarak sonucu inceleyin. Konum Tabanlı Hizmetler API’leriyle başarılı bir şekilde bağlantı kurulması için aşağıdakine benzer bir harita görürsünüz. 

    ![Azure Harita Denetimi ve Yönlendirme Hizmeti](./media/tutorial-route-location/lbs-map-route.png)


## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Adres koordinatlarını öğrenme
> * İstenen konuma yol tarifi almak için Sorgu Yönlendirme Hizmeti

Azure Konum Tabanlı Hizmetler’i, ulaşım yöntemini temel alarak dilediğiniz yolu önceliklendirecek şekilde nasıl kullanabileceğinizi öğrenmek için [Azure Konum Tabanlı Hizmetler’i kullanarak farklı ulaşım yöntemleri için yol tarifi alma](./tutorial-prioritized-routes.md) öğreticisine geçin. 
