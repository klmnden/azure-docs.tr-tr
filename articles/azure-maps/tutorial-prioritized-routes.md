---
title: Azure Haritalar ile birden fazla yol | Microsoft Docs
description: Azure Haritalar’ı kullanarak farklı seyahat modları için yolları bulma
author: walsehgal
ms.author: v-musehg
ms.date: 03/07/2019
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: db01c2f51e9069e8fc9ee979eacf746bee8dbdd2
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59260927"
---
# <a name="find-routes-for-different-modes-of-travel-using-azure-maps"></a>Azure Haritalar’ı kullanarak farklı seyahat modları için yolları bulma

Bu öğreticide, istediğiniz konuma yönelik ve ulaşım yönteminize göre önceliklendirilmiş yol tarifleri almak için Azure Haritalar hesabınız ile yol hizmetini nasıl kullanacağınız gösterilmektedir. Haritanızda, biri arabalar, diğeri ise yükseklik, ağırlık ya da tehlikeli yük nedeniyle yol kısıtlaması olabilecek kamyonlar için olmak üzere iki farklı yol görüntüleyin. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Harita denetimi API’sini kullanarak yeni bir web sayfası oluşturma
> * Haritanızda trafik akışını görselleştirme
> * Seyahat modunu bildiren yol sorguları oluşturma
> * Haritanızda birden fazla yol görüntüleme

## <a name="prerequisites"></a>Önkoşullar

Devam etmeden önce, [Azure Haritalar hesabınızı oluşturmak](./tutorial-search-location.md#createaccount) ve [hesabınızın abonelik anahtarını almak](./tutorial-search-location.md#getkey) için ilk öğreticide yer alan adımları izleyin.

## <a name="create-a-new-map"></a>Yeni harita oluşturma

Aşağıdaki adımlarda, Harita Denetimi API’sinin tümleşik olduğu statik bir HTML sayfasının nasıl oluşturulacağı gösterilmektedir.

1. Yerel makinenizde yeni bir dosya oluşturun ve bu dosyayı **MapTruckRoute.html** olarak adlandırın.
2. Aşağıdaki HTML bileşenlerini dosyaya ekleyin:

    ```HTML
    <!DOCTYPE html>
    <html>
    <head>
        <title>Map Route</title>
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

        <!-- Add references to the Azure Maps Map control JavaScript and CSS files. -->
        <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.css" type="text/css">
        <script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.js"></script>

        <!-- Add a reference to the Azure Maps Services Module JavaScript file. -->
        <script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas-service.min.js"></script>

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

3. `GetMap` işlevine aşağıdaki JavaScript kodunu ekleyin. Dize değiştirin `<Your Azure Maps Key>` haritalar hesabınızdan kopyaladığınız birincil anahtara sahip.

    ```JavaScript
    //Instantiate a map object
    var map = new atlas.Map("myMap", {
        //Add your Azure Maps subscription key to the map SDK. Get an Azure Maps key at https://azure.com/maps
        authOptions: {
            authType: 'subscriptionKey',
            subscriptionKey: '<Your Azure Maps Key>'
        }
    });
    ```

    `atlas.Map` Sınıfı bir görsel ve etkileşimli bir web haritası için denetim sağlar ve Azure harita denetimi API'SİNİN bir bileşenidir.

4. Dosyayı kaydedin ve tarayıcınızda açın. Bu noktada, daha fazla geliştirebileceğiniz temel bir haritanız olur.

   ![Temel haritayı görüntüleme](./media/tutorial-prioritized-routes/basic-map.png)

## <a name="visualize-traffic-flow"></a>Trafik akışını görselleştirme

1. Haritaya trafik akışı görüntüsünü ekleyin. Haritalar `ready` olay eşlemeleri kaynakları yüklenir ve güvenli bir şekilde etkileşim hazır olana kadar bekler.

    ```javascript
    map.events.add("ready", function() {
        // Add Traffic Flow to the Map
        map.setTraffic({
            flow: "relative"
        });
    });
    ```

    Haritadaki `ready` olay işleyicisi, trafik akışı harita üzerinde ayarı `relative`, serbest akışa göre trafik hızı olan. Bunu trafik hızı olan `absolute` veya serbest akışla farklılık gösteren bağıl hız `relative-delay` olarak da ayarlayabilirsiniz.

2. **MapTruckRoute.html** dosyasını kaydedin ve tarayıcınızın sayfasını yenileyin. Haritayla etkileşim kurabilir ve Los Angeles'ta yakınlaştırmak, geçerli trafiği verilerle sokaklar görmeniz gerekir.

   ![Trafik haritasını görüntüleme](./media/tutorial-prioritized-routes/traffic-map.png)

<a id="queryroutes"></a>

## <a name="define-how-the-route-will-be-rendered"></a>Rotanın nasıl işleneceğini tanımlama

Bu öğreticide iki rota hesaplanacak ve haritada işlenecektir. Rotalardan biri arabalar, diğeri de tırlar için kullanılabilir durumdaki yolları kullanacaktır. İşleme tamamlandığında rotanın başlangıcı ve bitişi için bir simgeye ek olarak her rota yolu için farklı renkte çizgiler görüntülenir.

1. Eşleme başlatılıyor sonra aşağıdaki JavaScript kodunu ekleyin maps'a `ready` olay işleyicisi.

    ```JavaScript
    //Wait until the map resources have fully loaded.
    map.events.add('ready', function () {

        //Create a data source and add it to the map.
        datasource = new atlas.source.DataSource();
        map.sources.add(datasource);

        //Add a layer for rendering the route lines and have it render under the map labels.
        map.layers.add(new atlas.layer.LineLayer(datasource, null, {
            strokeColor: ['get', 'strokeColor'],
            strokeWidth: ['get', 'strokeWidth'],
            lineJoin: 'round',
            lineCap: 'round'
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
            filter: ['any', ['==', ['geometry-type'], 'Point'], ['==', ['geometry-type'], 'MultiPoint']] //Only render Point or MultiPoints in this layer.
        }));
    });
    ```
    
    MAPS `ready` olay işleyicisi, bir veri kaynağı yolu satırları ve bunun yanı sıra başlangıç ve bitiş noktalarını depolamak için oluşturulur. Bir çizgi katmanı oluşturulup veri kaynağına eklenir ve rota çizgisinin nasıl işleneceği tanımlanır. Rota çizgisi özelliğinden çizgi kalınlığını ve rengini almak için ifadeler kullanılır. Katman haritaya eklenirken bu katmanın harita etiketlerinin altında işlenmesi gerektiğini belirten `'labels'` değerine sahip ikinci bir parametre geçirilir. Bu parametre, rota çizgisinin yol etiketlerini kapatmamasını sağlar. Bir simge katmanı oluşturulur ve veri kaynağına eklenir. Bu katman, başlangıç ve bitiş noktalarının nasıl işleneceğini belirtir. Bu durumda her bir nokta nesnesinin özelliklerinden simge görüntüsünü ve metin etiketi bilgilerini almak için ifadeler eklenmiştir. 
    
2. Bu öğreticide başlangıç noktasını Seattle’daki Fabrikam adlı hayali şirket, varış noktasını ise bir Microsoft ofisi olarak ayarlayın. MAPS `ready` olay işleyicisine aşağıdaki kodu ekleyin.

    ```JavaScript
    //Create the GeoJSON objects which represent the start and end point of the route.
    var startPoint = new atlas.data.Feature(new atlas.data.Point([-122.356099, 47.580045]), {
        title: 'Fabrikam, Inc.',
        icon: 'pin-blue'
    });

    var endPoint = new atlas.data.Feature(new atlas.data.Point([-122.201164, 47.616940]), {
        title: 'Microsoft - Lincoln Square',
        icon: 'pin-round-blue'
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

    Başlangıç ve bitiş noktaları veri kaynağına eklenir. Başlangıç ve bitiş noktaları için sınırlayıcı kutu, `atlas.data.BoundingBox.fromData` işlevi kullanılarak hesaplanır. Bu sınırlama kutusu harita kameralar görünümünü kullanarak tüm rota üzerinden ayarlamak için kullanılan `map.setCamera` işlevi. Simgelerin piksel boyutlarını telafi etmek için iç boşluk eklenir.

4. Dosyayı kaydedin ve iğneleri haritanızda görmek için tarayıcınızı yenileyin. Şimdi harita, Seattle üzerinde ortalanır ve başlangıç noktasının yuvarlak mavi raptiyeyle ve bitiş noktasının mavi raptiyeyle işaretlendiğini görebilirsiniz.

   ![Başlangıç ve bitiş noktaları ile haritayı görüntüleme](./media/tutorial-prioritized-routes/pins-map.png)

<a id="multipleroutes"></a>

## <a name="render-routes-prioritized-by-mode-of-travel"></a>Ulaşım yöntemine göre önceliklendirilmiş yolları işleme

Bu bölümde, yolların belirtilen başlangıç noktasından ulaşım aracınıza üzerinde tabanlı uç noktasına bulmak için Maps yönlendirme hizmeti API'sini kullanmayı gösterir. Yönlendirme hizmeti, mevcut trafik koşullarını göz önünde bulundurarak iki konum arasındaki *en hızlı*, *en kısa*, *ekonomik* veya *heyecan verici* yolları planlamak için API’ler sağlar. Ayrıca, Azure’ın geçmişe ait kapsamlı trafik veritabanını kullanarak herhangi bir gün ve saat için yol süresini tahmin eder. Böylece kullanıcıların ileri bir tarih için yol tarifi alabilmesini sağlar. Daha fazla bilgi için bkz. [getRouteDirections](https://docs.microsoft.com/rest/api/maps/route/getroutedirections). Aşağıdaki kod bloklarının harita tamamen yüklendikten sonra yüklendiğinden emin olmak için tümü **map load eventListener** içine eklenmelidir.

1. GetMap işlevinde, aşağıdaki Javascript kodunu ekleyin.

    ```JavaScript
    // Use SubscriptionKeyCredential with a subscription key
    var subscriptionKeyCredential = new atlas.service.SubscriptionKeyCredential(atlas.getSubscriptionKey());

    // Use subscriptionKeyCredential to create a pipeline
    var pipeline = atlas.service.MapsURL.newPipeline(subscriptionKeyCredential);

    // Construct the RouteURL object
    var routeURL = new atlas.service.RouteURL(pipeline);
    ```

   `SubscriptionKeyCredential` Oluşturur bir `SubscriptionKeyCredentialPolicy` abonelik anahtarını Azure haritalar için HTTP isteklerinde kimlik doğrulaması için. `atlas.service.MapsURL.newPipeline()` Alır `SubscriptionKeyCredential` ilke ve oluşturan bir [işlem hattı](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.pipeline?view=azure-iot-typescript-latest) örneği. `routeURL` Azure haritalar için URL'yi temsil [rota](https://docs.microsoft.com/rest/api/maps/route) operations.

2. Kimlik bilgileri ve URL'yi aşağıdaki JavaScript ekleme ayarlandıktan sonra başından uç noktası USHazmatClass2 taşıyan bir kamyon için bir yol oluşturmak için kod Kargo sınıflandırılan ve sonuçları görüntülemek.

    ```JavaScript
    //Start and end point input to the routeURL
    var coordinates= [[startPoint.geometry.coordinates[0], startPoint.geometry.coordinates[1]], [endPoint.geometry.coordinates[0], endPoint.geometry.coordinates[1]]];

    //Make a search route request for a truck vehicle type
    routeURL.calculateRouteDirections(atlas.service.Aborter.timeout(10000), coordinates,{
        travelMode: 'truck',
        vehicleWidth: 2,
        vehicleHeight: 2,
        vehicleLength: 5,
        vehicleLoadType: 'USHazmatClass2'
    }).then((directions) => {
        //Get data features from response
        var data = directions.geojson.getFeatures();

        //Get the route line and add some style properties to it.  
        var routeLine = data.features[0];
        routeLine.properties.strokeColor = '#2272B9';
        routeLine.properties.strokeWidth = 9;

        //Add the route line to the data source. We want this to render below the car route which will likely be added to the data source faster, so insert it at index 0.
        datasource.add(routeLine, 0);
    });
    ```

    Yukarıdaki Bu kod parçacığında bir Azure haritalar yönlendirme hizmeti aracılığıyla sorgular [getRouteDirections](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.models.routedirectionsrequestbody?view=azure-iot-typescript-latest) yöntemi. Rota satırı kullanarak ayıklanan yanıtından GeoJSON özellik koleksiyondan ayıklanır `geojson.getFeatures()` yöntemi. Rota satırın ardından veri kaynağına eklenir. Ayrıca, herhangi bir veri kaynağı satırlarında önce işlenen emin olmak için 0 dizinini ekler. Bunun nedeni, tır rotası hesaplama işlemlerinin araba rotası hesaplama işlemlerinden genellikle daha yavaş gerçekleştirilmesi ve tır rotasının araba rotasından sonra veri kaynağına eklenmesi durumunda üstünde işlenecek olmasıdır. İki özellik, kamyon rotası satırı yeşilin güzel bir tonunda mavi ve bir darbe genişliği dokuz piksel bir vuruş rengi eklenir.

3. Bir otomobil için bir yol oluşturmak ve sonuçları görüntülemek için aşağıdaki JavaScript kodunu ekleyin.

    ```JavaScript
    routeURL.calculateRouteDirections(atlas.service.Aborter.timeout(10000), coordinates).then((directions) => {

        //Get data features from response
        var data = directions.geojson.getFeatures();

        //Get the route line and add some style properties to it.  
        var routeLine = data.features[0];
        routeLine.properties.strokeColor = '#B76DAB';
        routeLine.properties.strokeWidth = 5;

        //Add the route line to the data source. We want this to render below the car route which will likely be added to the data source faster, so insert it at index 0.  
        datasource.add(routeLine);
    });
    ```

    Yukarıdaki Bu kod parçacığında bir Azure haritalar yönlendirme hizmeti aracılığıyla sorgular [getRouteDirections](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.models.routedirectionsrequestbody?view=azure-iot-typescript-latest) yöntemi. Rota satırı kullanarak ayıklanan yanıtından GeoJSON özellik koleksiyondan ayıklanır `geojson.getFeatures()` yöntemi. Rota satırın ardından veri kaynağına eklenir. İki özellik, araba rotası satırı gölge mor ve bir darbe genişliği beş piksel olan bir vuruş rengi eklenir.  

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

> [Azure Haritalar ile birden çok yol](https://github.com/Azure-Samples/AzureMapsCodeSamples/blob/master/AzureMapsCodeSamples/Tutorials/truckRoute.html)

[Burada Canlı örnek bakın](https://azuremapscodesamples.azurewebsites.net/?sample=Multiple%20routes%20by%20mode%20of%20travel)

Sonraki öğretici, Azure haritalar'ı kullanarak bir basit deposu Bulucu oluşturma işlemini gösterir.

> [!div class="nextstepaction"]
> [Azure haritalar'ı kullanarak bir depolama Bulucu](./tutorial-create-store-locator.md)

> [!div class="nextstepaction"]
> [Veri odaklı stili ifadeleri kullanma](data-driven-style-expressions-web-sdk.md)