---
title: Azure Haritalar ile birden fazla yol | Microsoft Docs
description: Azure Haritalar’ı kullanarak farklı seyahat modları için yolları bulma
author: walsehgal
ms.author: v-musehg
ms.date: 10/29/2018
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 67b68489f2e06b9149f842f293a769fa7f688be0
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50412712"
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
    <html>
    <head>
        <title>Multiple routes by mode of travel</title>
        
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

            #myMap {
                position: relative;
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

    Azure Harita Denetimi API’sinin bir bileşeni olan **atlas.Map**, görsel ve etkileşimli bir web haritası için denetim olanağı sunar.

4. Dosyayı kaydedin ve tarayıcınızda açın. Bu noktada, daha fazla geliştirebileceğiniz temel bir haritanız olur.

   ![Temel haritayı görüntüleme](./media/tutorial-prioritized-routes/basic-map.png)

## <a name="visualize-traffic-flow"></a>Trafik akışını görselleştirme

1. Haritaya trafik akışı görüntüsünü ekleyin. `map.events.add`, harita tamamen yüklendikten sonra haritaya eklenmiş olan tüm harita işlevlerinin yüklenmesini sağlar.

    ```JavaScript
    map.events.add("load", function() {
        // Add Traffic Flow to the Map
        map.setTraffic({
            flow: "relative"
        });
    });
    ```
    
     Haritaya, harita kaynakları tamamen yüklendikten sonra harekete geçirilecek bir yükleme olayı eklenir. Harita yükleme olayı işleyicisinde harita üzerindeki trafik akışı ayarı, serbest akışa göre belirlenen yol hızı olan `relative` olarak ayarlanır. Bunu trafik hızı olan `absolute` veya serbest akışla farklılık gösteren bağıl hız `relative-delay` olarak da ayarlayabilirsiniz.

2. **MapTruckRoute.html** dosyasını kaydedin ve tarayıcınızın sayfasını yenileyin. Geçerli trafik verileriyle birlikte Los Angeles sokaklarını görebilirsiniz.

   ![Trafik haritasını görüntüleme](./media/tutorial-prioritized-routes/traffic-map.png)

<a id="queryroutes"></a>

## <a name="define-how-the-route-will-be-rendered"></a>Rotanın nasıl işleneceğini tanımlama

Bu öğreticide iki rota hesaplanacak ve haritada işlenecektir. Rotalardan biri arabalar, diğeri de tırlar için kullanılabilir durumdaki yolları kullanacaktır. İşleme tamamlandığında rotanın başlangıcı ve bitişi için bir simgeye ek olarak her rota yolu için farklı renkte çizgiler görüntülenir.

1. GetMap işlevinde haritanın başlatıldığı bölümün altına aşağıdaki JavaScript kodunu ekleyin.

    ```JavaScript
    //Wait until the map resources have fully loaded.
    map.events.add('load', function () {

        //Create a data source and add it to the map.
        datasource = new atlas.source.DataSource();
        map.sources.add(datasource);

        //Add a layer for rendering the route lines and have it render under the map labels.
        map.layers.add(new atlas.layer.LineLayer(datasource, null, {
            strokeColor: ['get', 'strokeColor'],
            strokeWidth: ['get', 'strokeWidth'],
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
    
    Haritaya, harita kaynakları tamamen yüklendikten sonra harekete geçirilecek bir yükleme olayı eklenir. Harita yükleme olayı işleyicisinde rota çizgilerine ek olarak başlangıç ve bitiş noktalarını depolamak için bir veri kaynağı oluşturulur. Bir çizgi katmanı oluşturulup veri kaynağına eklenir ve rota çizgisinin nasıl işleneceği tanımlanır. Rota çizgisi özelliğinden çizgi kalınlığını ve rengini almak için ifadeler kullanılır. Bu katmanın yalnızca GeoJSON LineString verilerini işlemesini sağlamak için bir filtre eklenir. Katman haritaya eklenirken bu katmanın harita etiketlerinin altında işlenmesi gerektiğini belirten `'labels'` değerine sahip ikinci bir parametre geçirilir. Bu parametre, rota çizgisinin yol etiketlerini kapatmamasını sağlar. Bir simge katmanı oluşturulur ve veri kaynağına eklenir. Bu katman, başlangıç ve bitiş noktalarının nasıl işleneceğini belirtir. Bu durumda her bir nokta nesnesinin özelliklerinden simge görüntüsünü ve metin etiketi bilgilerini almak için ifadeler eklenmiştir. 
    
2. Bu öğreticide başlangıç noktasını Seattle’daki Fabrikam adlı hayali şirket, varış noktasını ise bir Microsoft ofisi olarak ayarlayın. Harita yükleme olayı işleyicisinde aşağıdaki kodu ekleyin.

    ```JavaScript
    //Create the GeoJSON objects which represent the start and end point of the route.
    var startPoint = new atlas.data.Feature(new atlas.data.Point([-122.356099, 47.580045]), {
        title: 'Fabrikam, Inc.',
        icon: 'pin-round-blue'
    });

    var endPoint = new atlas.data.Feature(new atlas.data.Point([-122.201164, 47.616940]), {
        title: 'Microsoft - Lincoln Square',
        icon: 'pin-blue'
    });
    ```
    
    Bu kod, yolun başlangıç ve bitiş noktalarını temsil eden iki [GeoJSON nesnesi](https://en.wikipedia.org/wiki/GeoJSON) oluşturur. Her noktaya `title` ve `icon` özelliği eklenir.

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
    
    Başlangıç ve bitiş noktaları veri kaynağına eklenir. Başlangıç ve bitiş noktaları için sınırlayıcı kutu, `atlas.data.BoundingBox.fromData` işlevi kullanılarak hesaplanır. Bu sınırlayıcı kutu, `map.setCamera` işlevi ile harita kameraları görünümünü başlangıç ve bitiş noktası üzerinde ayarlamak için kullanılır. Simgelerin piksel boyutlarını telafi etmek için iç boşluk eklenir.

4. Dosyayı kaydedin ve iğneleri haritanızda görmek için tarayıcınızı yenileyin. Şimdi harita, Seattle üzerinde ortalanır ve başlangıç noktasının yuvarlak mavi raptiyeyle ve bitiş noktasının mavi raptiyeyle işaretlendiğini görebilirsiniz.

   ![Başlangıç ve bitiş noktaları ile haritayı görüntüleme](./media/tutorial-prioritized-routes/pins-map.png)

<a id="multipleroutes"></a>

## <a name="render-routes-prioritized-by-mode-of-travel"></a>Ulaşım yöntemine göre önceliklendirilmiş yolları işleme

Bu bölümde, bir hedef için belirtilen başlangıç noktasından ulaşım aracınıza bağlı olarak birden fazla yol tarifi almak için Azure Haritalar yol hizmeti API’sini nasıl kullanabileceğiniz açıklanmaktadır. Yönlendirme hizmeti, mevcut trafik koşullarını göz önünde bulundurarak iki konum arasındaki *en hızlı*, *en kısa*, *ekonomik* veya *heyecan verici* yolları planlamak için API’ler sağlar. Ayrıca, Azure’ın geçmişe ait kapsamlı trafik veritabanını kullanarak herhangi bir gün ve saat için yol süresini tahmin eder. Böylece kullanıcıların ileri bir tarih için yol tarifi alabilmesini sağlar. Daha fazla bilgi için bkz. [getRouteDirections](https://docs.microsoft.com/rest/api/maps/route/getroutedirections). Aşağıdaki kod bloklarının harita tamamen yüklendikten sonra yüklendiğinden emin olmak için tümü **map load eventListener** içine eklenmelidir.

1. Harita yükleme olayı işleyicisine aşağıdaki Javascript kodunu ekleyerek hizmet istemcisini başlatın.

    ```JavaScript
    //If the service client hasn't been created, create an instance of it.
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

3. USHazmatClass2 sınıfında yük taşıyan bir kamyon için rota istemek ve sonuçları görüntülemek için aşağıdaki JavaScript kodunu ekleyin:

    ```JavaScript
    //Execute the truck route query then add the route to the map.
    client.route.getRouteDirections(routeQuery, {
        travelMode: 'truck',
        vehicleWidth: 2,
        vehicleHeight: 2,
        vehicleLength: 5,
        vehicleLoadType: 'USHazmatClass2'
    }).then(function (response) {
        // Parse the response into GeoJSON
        var geoJsonResponse = new atlas.service.geojson.GeoJsonRouteDirectionsResponse(response);

        //Get the route line and add some style properties to it.
        var routeLine = geoJsonResponse.getGeoJsonRoutes().features[0];
        routeLine.properties.strokeColor = '#2272B9';
        routeLine.properties.strokeWidth = 9;

        //Add the route line to the data source. We want this to render below the car route which will likely be added to the data source faster, so insert it at index 0.
        datasource.add(routeLine, 0);
    });
    ```
    Yukarıdaki kod parçacığı, [getRouteDirections](https://docs.microsoft.com/javascript/api/azure-maps-rest/services.route?view=azure-iot-typescript-latest#getroutedirections) yöntemi aracılığıyla Azure Haritalar yönlendirme hizmetini sorgular ve sonra [getGeoJsonRouteDirectionsResponse](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.geojson.geojsonroutedirectionsresponse?view=azure-iot-typescript-latest) kullanarak yanıtı GeoJSON biçiminde ayrıştırır. Daha sonra döndürülen yola yönelik koordinat dizisi oluşturur ve bu diziyi haritanın veri kaynağına ekler. Ayrıca veri kaynağındaki diğer çizgilerden önce oluşturulmasını sağlamak için bir 0 dizini de ekler. Bunun nedeni, tır rotası hesaplama işlemlerinin araba rotası hesaplama işlemlerinden genellikle daha yavaş gerçekleştirilmesi ve tır rotasının araba rotasından sonra veri kaynağına eklenmesi durumunda üstünde işlenecek olmasıdır. Tır rotası çizgisine mavi renkli bir vuruş rengi ve 9 piksellik vuruş genişliği olmak üzere iki özellik eklenir. 

4. Bir arabanın yolunu istemek ve sonuçları göstermek için aşağıdaki JavaScript kodunu ekleyin:

    ```JavaScript
    //Execute the car route query then add the route to the map.
    client.route.getRouteDirections(routeQuery).then(function (response) {
        // Parse the response into GeoJSON
        var geoJsonResponse = new atlas.service.geojson.GeoJsonRouteDirectionsResponse(response);

        //Get the route line and add some style properties to it.
        var routeLine = geoJsonResponse.getGeoJsonRoutes().features[0];
        routeLine.properties.strokeColor = '#B76DAB';
        routeLine.properties.strokeWidth = 5;

        //Add the route line to the data source.
        datasource.add(routeLine);
    });
    ```
    Bu kod parçacığı kamyon rota sorgusunu bir araba için kullanır. [getRouteDirections](https://docs.microsoft.com/javascript/api/azure-maps-rest/services.route?view=azure-iot-typescript-latest#getroutedirections) yöntemi aracılığıyla Azure Haritalar yönlendirme hizmetini sorgular ve sonra [getGeoJsonRouteDirectionsResponse](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.geojson.geojsonroutedirectionsresponse?view=azure-iot-typescript-latest) kullanarak yanıtı GeoJSON biçiminde ayrıştırır. Daha sonra döndürülen yola yönelik koordinat dizisi oluşturur ve bu diziyi veri kaynağına ekler. Araba rotası çizgisine mor renkli bir vuruş rengi ve 5 piksellik vuruş genişliği olmak üzere iki özellik eklenir. 

5. **MapTruckRoute.html** dosyasını kaydedin ve tarayıcınızı yenileyerek sonucu gözlemleyin. Haritalar API’leriyle başarılı bir şekilde bağlantı kurulması için aşağıdakine benzer bir harita görürsünüz.

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

> [Azure Haritalar ile birden fazla yol](https://github.com/Azure-Samples/AzureMapsCodeSamples/blob/master/AzureMapsCodeSamples/Tutorials/truckRoute.html)

[Burada canlı örneği inceleyin](https://azuremapscodesamples.azurewebsites.net/?sample=Multiple%20routes%20by%20mode%20of%20travel)

Azure Haritalar'ın kapsamı ve özellikleri hakkında daha fazla bilgi edinmek için:

> [!div class="nextstepaction"]
> [Yakınlaştırma düzeyleri ve kutucuk kılavuzu](zoom-levels-and-tile-grid.md)

Daha fazla kod örneği ve etkileşimli bir kodlama deneyimi için:

> [!div class="nextstepaction"]
> [Harita denetimini kullanma](how-to-use-map-control.md)
