---
title: Azure Haritalar ile yol bulma | Microsoft Docs
description: Azure Haritalar’ı kullanarak ilgi çekici noktaya yönlendirme
author: kgremban
ms.author: kgremban
ms.date: 05/07/2018
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: fc5dfafec303a439d8a1092771fd2247ab305172
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34601352"
---
# <a name="route-to-a-point-of-interest-using-azure-maps"></a>Azure Haritalar’ı kullanarak ilgi çekici noktaya yönlendirme

Bu öğreticide, Azure Haritalar hesabınız ile Yönlendirme Hizmeti SDK’nızı kullanarak ilgi çekici noktanıza nasıl yol tarifi alabileceğiniz gösterilmektedir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Harita denetimi API’sini kullanarak yeni bir web sayfası oluşturma
> * Adres koordinatlarını ayarlama
> * İstenen konuma yol tarifi almak için Sorgu Yönlendirme Hizmeti

## <a name="prerequisites"></a>Ön koşullar

Devam etmeden önce, [Azure Haritalar hesabınızı oluşturmak](./tutorial-search-location.md#createaccount) ve [hesabınızın abonelik anahtarını almak](./tutorial-search-location.md#getkey) için önceki öğreticide yer alan adımları izleyin. 

<a id="getcoordinates"></a>

## <a name="create-a-new-map"></a>Yeni harita oluşturma 
Aşağıdaki adımlarda, Harita Denetimi API’sinin tümleşik olduğu statik bir HTML sayfasının nasıl oluşturulacağı gösterilmektedir. 

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
    HTML üst bilgisi, Azure Haritalar kitaplığı için CSS ve JavaScript dosyalarına yönelik kaynak konumlarını ekler. HTML dosyasının gövdesindeki *betik* segmenti, Azure Haritalar API’lerine erişmeye yönelik satır içi JavaScript kodunu içerir.

3. HTML dosyasının *betik* bloğuna aşağıdaki JavaScript kodunu ekleyin. **\<Hesap anahtarınız\>** dizesini, Haritalar hesabınızdan kopyaladığınız birincil anahtarla değiştirin.

    ```JavaScript
    // Instantiate map to the div with id "map"
    var MapsAccountKey = "<your account key>";
    var map = new atlas.Map("map", {
        "subscription-key": MapsAccountKey
    });
    ```
    Azure Harita Denetimi API’sinin bir bileşeni olan **atlas.Map**, görsel ve etkileşimli bir web haritası için denetim olanağı sunar.

4. Dosyayı kaydedin ve tarayıcınızda açın. Bu noktada, daha fazla geliştirebileceğiniz temel bir haritanız olur. 

   ![Temel haritayı görüntüleme](./media/tutorial-route-location/basic-map.png)

## <a name="set-start-and-end-points"></a>Başlangıç ve bitiş noktalarını ayarlama

Bu öğretici için başlangıç noktası olarak Microsoft’u ve hedef nokta olarak Seattle’da bir benzin istasyonunu ayarlayın. 

1. **MapRoute.html** dosyasının aynı *betik* bloğuna aşağıdaki JavaScript kodunu ekleyerek yolun başlangıç ve bitiş noktalarını oluşturun:

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
    Bu kod, yolun başlangıç ve bitiş noktalarını temsil eden iki [GeoJSON nesnesi](https://en.wikipedia.org/wiki/GeoJSON) oluşturur. 

2. Haritaya başlangıç ve bitiş noktalarını sabitlemek için aşağıdaki JavaScript kodunu ekleyin:

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
    **map.setCameraBounds**, harita penceresini başlangıç ve bitiş noktalarının koordinatlarına göre ayarlar. **map.addPins** API’si, Harita denetimine görsel bileşen olarak nokta ekler.

7. **MapRoute.html** dosyasını kaydedin ve tarayıcınızı yenileyin. Şimdi harita, Seattle üzerinde ortalanır ve başlangıç noktasının yuvarlak mavi raptiyeyle ve bitiş noktasının mavi raptiyeyle işaretlendiğini görebilirsiniz.

   ![Başlangıç ve bitiş noktaları işaretlenmiş şekilde haritayı görüntüleme](./media/tutorial-route-location/map-pins.png)

<a id="getroute"></a>

## <a name="get-directions"></a>Yol tarifini alma

Bu bölümde, belirtilen bir başlangıç noktasından bir hedefe giden yolu bulmak için Haritalar yönlendirme hizmeti API’sinin nasıl kullanılacağı gösterilmektedir. Yönlendirme hizmeti, iki konum arasındaki *en hızlı*, *en kısa*, *ekonomik* veya *heyecan verici* yolları planlamak için API’ler sağlar. Ayrıca, Azure’ın geçmişe ait kapsamlı trafik veritabanını kullanarak herhangi bir gün ve saat için yol süresini tahmin eder. Böylece kullanıcıların ileri bir tarih için yol tarifi alabilmesini sağlar. Daha fazla bilgi için bkz. [Yol tariflerini alma](https://docs.microsoft.com/rest/api/maps/route/getroutedirections).


1. İlk olarak, rota yolunu veya *linestring*’i görüntülemek için haritaya yeni bir katman ekleyin. *Betik* bloğuna aşağıdaki JavaScript kodunu ekleyin:

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

2. Bir [XMLHttpRequest](https://xhr.spec.whatwg.org/) oluşturun ve olay işleyici ekleyerek Haritalar yönlendirme hizmeti tarafından gönderilen JSON yanıtını ayrıştırın. Bu kod, yolun doğru parçaları için bir koordinat dizisi oluşturur, daha sonra haritanın linestrings katmanına koordinat kümesini ekler. 

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

3. Aşağıdaki kodu ekleyerek sorguyu derleyin ve Haritalar yönlendirme hizmetine XMLHttpRequest gönderin:

    ```JavaScript
    var url = "https://atlas.microsoft.com/route/directions/json?";
    url += "api-version=1.0";
    url += "&subscription-key=" + MapsAccountKey;
    url += "&query=" + startPoint.coordinates[1] + "," + startPoint.coordinates[0] + ":" +
        destinationPoint.coordinates[1] + "," + destinationPoint.coordinates[0];

    xhttp.open("GET", url, true);
    xhttp.send();
    ```

3. **MapRoute.html** dosyasını kaydedin ve web tarayıcınızı yenileyin. Haritalar API’leriyle başarılı bir şekilde bağlantı kurulması için aşağıdakine benzer bir harita görürsünüz. 

    ![Azure Harita Denetimi ve Yönlendirme Hizmeti](./media/tutorial-route-location/map-route.png)


## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Harita denetimi API’sini kullanarak yeni bir web sayfası oluşturma
> * Adres koordinatlarını ayarlama
> * İlgi çekici noktaya yol tarifi almak için yönlendirme hizmetini sorgulama

Sonraki öğreticide, seyahat modu veya kargo türü gibi kısıtlamalarla bir yol sorgusu oluşturma ve aynı harita üzerinde birden fazla yolu görüntüleme işlemleri gösterilmektedir. 

> [!div class="nextstepaction"]
> [Farklı seyahat modları için yolları bulma](./tutorial-prioritized-routes.md)
