---
title: "Azure konum tabanlı Hizmetleri Bul rotası | Microsoft Docs"
description: "Azure konum tabanlı Hizmetleri kullanarak ilgi noktasına yönlendirmek"
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
ms.openlocfilehash: f2be9ca98330866ac8b6fb12efd56efdc711eedf
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2017
---
# <a name="route-to-a-point-of-interest-using-azure-location-based-services"></a>Azure konum tabanlı Hizmetleri kullanarak ilgi noktasına yönlendirmek

Bu öğretici Azure konum tabanlı Hizmetleri hesabınızı ve rota hizmeti SDK'sı noktanızın ilgi rotayı bulmak için nasıl kullanılacağını gösterir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Adres koordinatlarını alma
> * Rota ilgi işaret edecek şekilde hizmeti yönergeleri için sorgu

## <a name="prerequisites"></a>Ön koşullar

Devam etmeden önce emin olun [Azure konum tabanlı Hizmetleri hesabınızı oluşturmak](./tutorial-search-location.md#createaccount), ve [hesabınız için abonelik anahtarı alma](./tutorial-search-location.md#getkey). Harita denetiminin ve arama hizmeti API'leri öğreticide anlatıldığı gibi nasıl kullanılacağını da gözlemleyebilirsiniz [arama ilgi çekici Azure konum tabanlı Hizmetleri kullanarak yakındaki](./tutorial-search-location.md).


<a id="getcoordinates"></a>

## <a name="get-address-coordinates"></a>Adres koordinatlarını alma

Konum tabanlı hizmetlerin Harita Denetim API'si ile katıştırılmış bir statik HTML sayfası oluşturmak için aşağıdaki adımları kullanın. 

1. Yerel makinenizde yeni bir dosya oluşturun ve adlandırın **MapRoute.html**. 
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
    HTML üstbilgisi Azure konum tabanlı Hizmetleri kitaplığı CSS ve JavaScript dosyaları için kaynak konumlarını nasıl katıştırır unutmayın. Ayrıca fark *komut dosyası* satır içi Azure konumu dayalı hizmetin API'lere erişim için JavaScript kodunu içeren HTML dosyası gövdesinde kesimi.

3. Şu JavaScript kodunu eklemek *betik* HTML dosyası bloğu. Yer tutucu Değiştir *< anahtar Ekle >* konum tabanlı Hizmetleri hesabınızın birincil anahtara sahip.

    ```JavaScript
    // Instantiate map to the div with id "map"
    var subscriptionKey = "<insert-key>";
    var map = new atlas.Map("map", {
        "subscription-key": subscriptionKey
    });
    ```
    **Atlas. Harita** denetim için bir görsel ve etkileşimli web eşleme sağlar ve Azure Harita Denetim API'si bir bileşenidir.

4. Şu JavaScript kodunu eklemek *betik* bloğu. Bu bir katmanı ekler *MultiPoint* yol göstermek için harita denetiminin için:

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

5. Başlangıç ve bitiş noktalarını rota oluşturmak için aşağıdaki JavaScript kodu ekleyin:

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
    Bu kod iki oluşturur [GeoJSON nesne](https://en.wikipedia.org/wiki/GeoJSON) başlangıç ve bitiş noktalarını rotanın temsil etmek için. Uç noktası biri için enlem/boylam birleşimidir *Akaryakıt istasyonları* önceki öğreticide aranır [arama ilgi çekici Azure konum tabanlı Hizmetleri kullanarak yakındaki](./tutorial-search-location.md).

6. Başlangıç ve bitiş noktaları için PIN eşlemesine eklemek için aşağıdaki JavaScript kodu ekleyin:

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
    API **map.setCameraBounds** Haritası penceresini başlangıç ve bitiş noktası koordinatları göre ayarlar. API **map.addPins** görsel bileşenleri olarak harita denetim noktaları ekler.

7. Kaydet **MapRoute.html** makinenizde dosya. 

<a id="getroute"></a>

## <a name="query-route-service-for-directions-to-point-of-interest"></a>Rota ilgi işaret edecek şekilde hizmeti yönergeleri için sorgu

Bu bölümde Azure konum tabanlı hizmetlerin rota hizmeti API'si bir hedef için belirtilen başlangıç noktasından rotayı bulmak için nasıl kullanılacağını gösterir. Rota hizmet en hızlı, kısa plana API'leri veya gerçek zamanlı trafiği koşulları dikkate iki konum arasında ekonomik yolu sağlar. Ayrıca, kullanıcıların Azure'nın kapsamlı geçmiş trafik veritabanı kullanarak ve her gün ve saat için rota sürelerini tahmin etmeye gelecekte yollar planlama imkan tanır. 

1. Açık **MapRoute.html** dosyası önceki bölümde oluşturulan ve şu JavaScript kodunu ekleyin *betik* rota hizmeti göstermeye bloğu.

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
    Bu kod parçacığını oluşturur bir [XMLHttpRequest](https://xhr.spec.whatwg.org/), ve gelen yanıtı ayrıştırılamadı olay işleyicisi ekler. Başarılı bir yanıt için bir dizi satır bölümü döndürülen ilk rotanın koordinatları oluşturur. Ardından haritanın koordinatları bu yol için bu kümesini ekler *MultiPoint* katmanı.

2. Aşağıdaki kodu ekleyin *betik* Azure konum tabanlı hizmetlerin rota hizmete XMLHttpRequest göndermeye engelle:

    ```JavaScript
    var url = "https://atlas.microsoft.com/route/directions/json?";
    url += "&api-version=1.0";
    url += "&subscription-key=" + subscriptionKey;
    url += "&query=" + startPoint.coordinates[1] + "," + startPoint.coordinates[0] + ":" +
        destinationPoint.coordinates[1] + "," + destinationPoint.coordinates[0];

    xhttp.open("GET", url, true);
    xhttp.send();
    ```
    Yukarıdaki istek hesabınızın abonelik anahtarı ve başlangıç ve bitiş noktası koordinatları olduğundan, gerekli parametreleri verildikleri sırada gösterilir. 

3. Kaydet **MapRoute.html** yerel olarak dosya sonra tercih ettiğiniz bir web tarayıcısında açın ve sonucu inceleyin. Konum tabanlı hizmetlerin API'leri ile başarılı bir bağlantı için aşağıdakine benzer bir harita görmeniz gerekir. 

    ![Azure harita denetiminin ve rota hizmeti](./media/tutorial-route-location/lbs-map-route.png)


## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Adres koordinatlarını alma
> * Rota ilgi işaret edecek şekilde hizmeti yönergeleri için sorgu

Öğreticisine devam [farklı Azure konum tabanlı Hizmetleri kullanarak seyahat modu yolları Bul](./tutorial-prioritized-routes.md) aktarım modu üzerinde temel Azure konum tabanlı Hizmetleri noktanızı ilgi alanı yolları önceliğini belirlemek için nasıl kullanılacağını öğrenin. 
