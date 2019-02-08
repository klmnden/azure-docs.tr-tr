---
title: Azure Haritalar ile yol bulma | Microsoft Docs
description: Azure Haritalar’ı kullanarak ilgi çekici noktaya yönlendirme
author: walsehgal
ms.author: v-musehg
ms.date: 11/14/2018
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 7fb382c6c57972484eff80c0d7355319a1f9178a
ms.sourcegitcommit: e51e940e1a0d4f6c3439ebe6674a7d0e92cdc152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55893560"
---
# <a name="route-to-a-point-of-interest-using-azure-maps"></a>Azure Haritalar’ı kullanarak ilgi çekici noktaya yönlendirme

Bu öğreticide, Azure Haritalar hesabınız ile Yönlendirme Hizmeti SDK’nızı kullanarak ilgi çekici noktanıza nasıl yol tarifi alabileceğiniz gösterilmektedir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Harita denetimi API’sini kullanarak yeni bir web sayfası oluşturma
> * Adres koordinatlarını ayarlama
> * İstenen konuma yol tarifi almak için Sorgu Yönlendirme Hizmeti

## <a name="prerequisites"></a>Önkoşullar

Devam etmeden önce, [Azure Haritalar hesabınızı oluşturmak](./tutorial-search-location.md#createaccount) ve [hesabınızın abonelik anahtarını almak](./tutorial-search-location.md#getkey) için önceki öğreticide yer alan adımları izleyin.

<a id="getcoordinates"></a>

## <a name="create-a-new-map"></a>Yeni harita oluşturma

Aşağıdaki adımlarda, Harita Denetimi API’sinin tümleşik olduğu statik bir HTML sayfasının nasıl oluşturulacağı gösterilmektedir.

1. Yerel makinenizde yeni bir dosya oluşturun ve bu dosyayı **MapRoute.html** olarak adlandırın.
2. Aşağıdaki HTML bileşenlerini dosyaya ekleyin:

    ```HTML
    <!DOCTYPE html>
    <html>
    <head>
        <title>Map Route</title>
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />
        
        <!-- Add references to the Azure Maps Map control JavaScript and CSS files. -->
        <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/css/atlas.min.css?api-version=1" type="text/css" />
        <script src="https://atlas.microsoft.com/sdk/js/atlas.min.js?api-version=1"></script>

        <!-- Add a reference to the Azure Maps Services Module JavaScript file. -->
        <script src="https://atlas.microsoft.com/sdk/js/atlas-service.js?api-version=1"></script>
        
        <script>
            var map, datasource, client;

            function GetMap() {
                //Add Map Control JavaScript code here.
            }
        </script>
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
    <body onload="GetMap()">
        <div id="myMap"></div>
    </body>
    </html>
    ```
    
    HTML üst bilgisinin Azure Harita Denetimi kitaplığı tarafından barındırılan CSS ve JavaScript kaynak dosyalarını içerdiğine dikkat edin. Sayfanın gövdesinde bulunan ve sayfa yüklendiğinde `GetMap` işlevini çağıracak olan `onload` olayına dikkat edin. Bu işlev, Azure Haritalar API’lerine erişime yönelik satır içi JavaScript kodunu içerir. 

3. `GetMap` işlevine aşağıdaki JavaScript kodunu ekleyin. **\<Azure Haritalar Anahtarınız\>** dizesini, Haritalar hesabınızdan kopyaladığınız birincil anahtarla değiştirin.

    ```JavaScript
    //Add your Azure Maps subscription key to the map SDK. 
    atlas.setSubscriptionKey('<Your Azure Maps Key>');

    //Initialize a map instance.
    map = new atlas.Map('myMap');
    ```

    Azure Harita Denetimi API’sinin bir bileşeni olan `atlas.Map`, görsel ve etkileşimli bir web haritası için denetim olanağı sunar.

4. Dosyayı kaydedin ve tarayıcınızda açın. Bu noktada, daha fazla geliştirebileceğiniz temel bir haritanız olur.

   ![Temel haritayı görüntüleme](./media/tutorial-route-location/basic-map.png)

## <a name="define-how-the-route-will-be-rendered"></a>Rotanın nasıl işleneceğini tanımlama

Bu öğreticide rota başlangıcı ve bitişi için bir simge ve rota yolu için bir çizgi kullanılarak basit bir rota işlenecektir.

1. GetMap işlevinde haritanın başlatıldığı bölümün altına aşağıdaki JavaScript kodunu ekleyin.

    ```JavaScript
    //Wait until the map resources have fully loaded.
    map.events.add('load', function () {

        //Create a data source and add it to the map.
        datasource = new atlas.source.DataSource();
        map.sources.add(datasource);

        //Add a layer for rendering the route lines and have it render under the map labels.
        map.layers.add(new atlas.layer.LineLayer(datasource, null, {
            strokeColor: '#2272B9',
            strokeWidth: 5,
            lineJoin: 'round',
            lineCap: 'round',
            filter: ['==', '$type', 'LineString']
        }), 'labels');

        //Add a layer for rendering point data.
        map.layers.add(new atlas.layer.SymbolLayer(datasource, null, {
            iconOptions: {
                image: ['get', 'icon'],
                allowOverlap: true
           },
            textOptions: {
                textField: ['get', 'title'],
                offset: [0, 1.2]
            },
            filter: ['==', '$type', 'Point']
        }));
    });
    ```
    
    Haritaya, harita kaynakları tamamen yüklendikten sonra harekete geçirilecek bir yükleme olayı eklenir. Harita yükleme olayı işleyicisinde rota çizgisine ek olarak başlangıç ve bitiş noktalarını depolamak için bir veri kaynağı oluşturulur. Bir çizgi katmanı oluşturulup veri kaynağına eklenir ve rota çizgisinin nasıl işleneceği tanımlanır. Rota çizgisi mavi renkli olarak 5 piksel genişliğinde işlenecek ve birleşim yerleri ile bitiş uçları yuvarlatılacaktır. Bu katmanın yalnızca GeoJSON LineString verilerini işlemesini sağlamak için bir filtre eklenir. Katman haritaya eklenirken bu katmanın harita etiketlerinin altında işlenmesi gerektiğini belirten `'labels'` değerine sahip ikinci bir parametre geçirilir. Bu parametre, rota çizgisinin yol etiketlerini kapatmamasını sağlar. Bir simge katmanı oluşturulur ve veri kaynağına eklenir. Bu katman, başlangıç ve bitiş noktalarının nasıl işleneceğini belirtir. Bu durumda her bir nokta nesnesinin özelliklerinden simge görüntüsünü ve metin etiketi bilgilerini almak için ifadeler eklenmiştir. 
    
2. Bu öğretici için başlangıç noktası olarak Microsoft’u ve bitiş noktası olarak Seattle’da bir benzin istasyonunu ayarlayın. Harita yükleme olayı işleyicisinde aşağıdaki kodu ekleyin.

    ```JavaScript
    //Create the GeoJSON objects which represent the start and end point of the route.
    var startPoint = new atlas.data.Feature(new atlas.data.Point([-122.130137, 47.644702]), {
        title: 'Microsoft',
        icon: 'pin-round-blue'
    });

    var endPoint = new atlas.data.Feature(new atlas.data.Point([-122.3352, 47.61397]), {
        title: 'Contoso Oil & Gas',
        icon: 'pin-blue'
    });    
    ```

    Bu kod, yolun başlangıç ve bitiş noktalarını temsil eden iki [GeoJSON noktası nesnesi](https://en.wikipedia.org/wiki/GeoJSON) oluşturur. Her noktaya `title` ve `icon` özelliği eklenir.
    
3. Ardından haritaya başlangıç ve bitiş noktalarını sabitlemek için aşağıdaki JavaScript kodunu ekleyin:

    ```JavaScript
    //Add the data to the data source.
    datasource.add([startPoint, endPoint]);
    
    //Fit the map window to the bounding box defined by the start and end positions.
    map.setCamera({
        bounds: atlas.data.BoundingBox.fromData([startPoint, endPoint]),
        padding: 100
    });
    ```
    
    Başlangıç ve bitiş noktaları veri kaynağına eklenir. Başlangıç ve bitiş noktaları için sınırlayıcı kutu, `atlas.data.BoundingBox.fromData` işlevi kullanılarak hesaplanır. Bu sınırlayıcı kutu, **map.setCamera** işlevi ile harita kameraları görünümünü başlangıç ve bitiş noktası üzerinde ayarlamak için kullanılır. Simgelerin piksel boyutlarını telafi etmek için iç boşluk eklenir.

3. **MapRoute.html** dosyasını kaydedin ve tarayıcınızı yenileyin. Şimdi harita, Seattle üzerinde ortalanır ve başlangıç noktasının yuvarlak mavi raptiyeyle ve bitiş noktasının mavi raptiyeyle işaretlendiğini görebilirsiniz.

   ![Başlangıç ve bitiş noktaları işaretlenmiş şekilde haritayı görüntüleme](./media/tutorial-route-location/map-pins.png)

<a id="getroute"></a>

## <a name="get-directions"></a>Yol tarifini alma

Bu bölümde, belirtilen bir başlangıç noktasından bir hedefe giden yolu bulmak için Haritalar yönlendirme hizmeti API’sinin nasıl kullanılacağı gösterilmektedir. Yönlendirme hizmeti, iki konum arasındaki *en hızlı*, *en kısa*, *ekonomik* veya *heyecan verici* yolları planlamak için API’ler sağlar. Ayrıca, Azure’ın geçmişe ait kapsamlı trafik veritabanını kullanarak herhangi bir gün ve saat için yol süresini tahmin eder. Böylece kullanıcıların ileri bir tarih için yol tarifi alabilmesini sağlar. Daha fazla bilgi için bkz. [Yol tariflerini alma](https://docs.microsoft.com/rest/api/maps/route/getroutedirections). Aşağıdaki işlevlerin harita tamamen yüklendikten sonra yüklendiğinden emin olmak için tümü **map load eventListener** içine eklenmelidir.

1. Harita yükleme olayı işleyicisine aşağıdaki Javascript kodunu ekleyerek hizmet istemcisini başlatın.

    ```JavaScript
    //If the service client hasn't already been created, create an instance.
    if (!client) {
        client = new atlas.service.Client(atlas.getSubscriptionKey());
    }
    ```

2. Yol sorgu dizesi oluşturmak için aşağıdaki kod bloğunu ekleyin.
    ```JavaScript
    //Create the route request with the query being the start and end point in the format 'startLongitude,startLatitude:endLongitude,endLatitude'.
    var routeQuery = startPoint.geometry.coordinates[1] +
        ',' +
        startPoint.geometry.coordinates[0] +
        ':' +
        endPoint.geometry.coordinates[1] +
        ',' +
        endPoint.geometry.coordinates[0];
    ```

3. Yolu almak için betiğe aşağıdaki kod bloğunu ekleyin. [getRouteDirections](https://docs.microsoft.com/javascript/api/azure-maps-rest/services.route?view=azure-iot-typescript-latest) yöntemi aracılığıyla Azure Haritalar yönlendirme hizmetini sorgular ve sonra [getGeoJsonRoutes](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.geojson.geojsonroutedirectionsresponse?view=azure-iot-typescript-latest) kullanarak yanıtı GeoJSON biçiminde ayrıştırır. Ardından veri kaynağı yanıtına rota çizgisi eklenerek haritada otomatik olarak işlenmesi sağlanır.

    ```JavaScript
    //Execute the car route query then add the route to the map once a response is received.
    client.route.getRouteDirections(routeQuery).then(function (response) {
        // Parse the response into GeoJSON
        var geoJsonResponse = new atlas.service.geojson.GeoJsonRouteDirectionsResponse(response);

        //Add the route line to the data source.
        datasource.add(geoJsonResponse.getGeoJsonRoutes().features[0]);
    });
    ```

5. **MapRoute.html** dosyasını kaydedin ve web tarayıcınızı yenileyin. Haritalar API’leriyle başarılı bir şekilde bağlantı kurulması için aşağıdakine benzer bir harita görürsünüz.

    ![Azure Harita Denetimi ve Yönlendirme Hizmeti](./media/tutorial-route-location/map-route.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Harita denetimi API’sini kullanarak yeni bir web sayfası oluşturma
> * Adres koordinatlarını ayarlama
> * İlgi çekici noktaya yol tarifi almak için yönlendirme hizmetini sorgulama

Bu öğreticiye ait kod örneğine şuradan erişebilirsiniz:

> [Azure Haritalar ile yol bulma](https://github.com/Azure-Samples/AzureMapsCodeSamples/blob/master/AzureMapsCodeSamples/Tutorials/route.html)

[Burada canlı örneği inceleyin](https://azuremapscodesamples.azurewebsites.net/?sample=Route%20to%20a%20destination)

Sonraki öğreticide, seyahat modu veya kargo türü gibi kısıtlamalarla bir yol sorgusu oluşturma ve aynı harita üzerinde birden fazla yolu görüntüleme işlemleri gösterilmektedir.

> [!div class="nextstepaction"]
> [Farklı seyahat modları için yolları bulma](./tutorial-prioritized-routes.md)
