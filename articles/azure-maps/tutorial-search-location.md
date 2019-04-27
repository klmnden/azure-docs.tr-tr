---
title: Azure Haritalar ile arama | Microsoft Docs
description: Azure Haritalar’ı kullanarak yakınlardaki ilgi çekici noktayı arama
author: walsehgal
ms.author: v-musehg
ms.date: 03/07/2019
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 125e0c4331eea105ffc201bd1f5f26bdbec1c553
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60692049"
---
# <a name="search-nearby-points-of-interest-using-azure-maps"></a>Azure Haritalar’ı kullanarak yakınlardaki ilgi çekici noktaları arama

Bu öğreticide, Azure Haritalar hesabı ayarlama ve sonra Haritalar API’lerini kullanarak ilgi çekici bir noktayı arama işlemleri gösterilmektedir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure Haritalar hesabı oluşturma
> * Haritalar hesabınızın birincil anahtarını alma
> * Harita denetimi API’sini kullanarak yeni bir web sayfası oluşturma
> * Haritalar arama hizmetini kullanarak yakınlardaki bir ilgi çekici noktayı bulma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com) oturum açın.

<a id="createaccount"></a>

## <a name="create-an-account-with-azure-maps"></a>Azure Maps hesabı oluşturma

Aşağıdaki adımları uygulayarak yeni bir Haritalar hesabı oluşturun:

1. [Azure portalının](https://portal.azure.com) sol üst köşesinde bulunan **Kaynak oluştur** öğesine tıklayın.
2. *Market’te Ara* kutusuna **Haritalar** yazın.
3. *Sonuçlar* içinden **Haritalar**’ı seçin. Haritanın altında görüntülenen **Oluştur** düğmesine tıklayın.
4. **Haritalar Hesabı Oluştur** sayfasında aşağıdaki değerleri girin:
    * Bu hesap için kullanmak istediğiniz *Abonelik*.
    * Bu hesap için *Kaynak grubu* adı. Kaynak grubu için *Yeni oluştur* veya *Mevcut olanı kullan* seçeneğini belirleyebilirsiniz.
    * Yeni hesabınıza verilen *Ad*.
    * *Fiyatlandırma katmanı* bu hesap için.
    * *Lisans*’ı ve *Gizlilik Bildirimi*’ni okuyun ve onay kutusunu işaretleyerek koşulları kabul edin.
    * **Oluştur** düğmesine tıklayın.

![Portalda Haritalar hesabı oluşturma](./media/tutorial-search-location/create-account.png)

<a id="getkey"></a>

## <a name="get-the-primary-key-for-your-account"></a>Hesabınızın birincil anahtarını alma

Haritalar hesabınız başarıyla oluşturulduktan sonra, Haritalar API’lerini sorgulamanıza olanak sağlayan anahtarı alın.

1. Portalda Haritalar hesabınızı açın.
2. Ayarlar bölümünde **kimlik doğrulaması**.
3. **Birincil Anahtar**’ı panonuza kopyalayın. Bu öğreticinin ilerleyen kısmında kullanmak üzere bunu yerel olarak kaydedin.

![Portalda Birincil Anahtar’ı alma](./media/tutorial-search-location/get-key.png)

<a id="createmap"></a>

## <a name="create-a-new-map"></a>Yeni harita oluşturma

Harita Denetimi API’si, Haritalar’ı web uygulamanızla kolayca tümleştirmenize olanak tanıyan kullanışlı bir istemci kitaplığıdır. Açık REST hizmet çağrılarının karmaşıklığını gizlemesinin yanı sıra, stili değiştirilebilen ve özelleştirilebilen bileşenlerle üretkenliğinizi arttırır. Aşağıdaki adımlarda, Harita Denetimi API’sinin tümleşik olduğu statik bir HTML sayfasının nasıl oluşturulacağı gösterilmektedir.

1. Yerel makinenizde yeni bir dosya oluşturun ve bu dosyayı **MapSearch.html** olarak adlandırın.
2. Aşağıdaki HTML bileşenlerini dosyaya ekleyin:

   ```HTML
    <!DOCTYPE html>
    <html>
    <head>
        <title>Map Search</title>
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

        <!-- Add references to the Azure Maps Map control JavaScript and CSS files. -->
        <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.css" type="text/css">
        <script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.js"></script>

        <!-- Add a reference to the Azure Maps Services Module JavaScript file. -->
        <script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas-service.min.js"></script>

        <script>
        function GetMap(){
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

   HTML üst bilgisinin Azure Harita Denetimi kitaplığı tarafından barındırılan CSS ve JavaScript kaynak dosyalarını içerdiğine dikkat edin. Sayfanın gövdesinde bulunan ve sayfa yüklendiğinde `GetMap` işlevini çağıracak olan `onload` olayına dikkat edin. `GetMap` İşlevi satır içi JavaScript kodunu Azure haritalar API'lere içerir.

3. HTML dosyasının `GetMap` işlevine aşağıdaki JavaScript kodunu ekleyin. Dize değiştirin `<Your Azure Maps Key>` haritalar hesabınızdan kopyaladığınız birincil anahtara sahip.

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

   Bu segment, Azure Haritalar hesap anahtarınız için Harita Denetimi API’sini başlatır. `atlas` API ve ilgili görsel bileşenleri içeren ad alanıdır. `atlas.Map` bir görsel ve etkileşimli bir web haritası için denetim sağlar.

4. Değişikliklerinizi dosyaya kaydedin ve HTML sayfasını bir tarayıcıda açın. Bu çağrı yaparak yapabileceğiniz, en temel haritasıdır `atlas.Map` hesap anahtarını kullanarak.

   ![Haritayı görüntüleme](./media/tutorial-search-location/basic-map.png)

5. `GetMap` işlevinde haritanın başlatıldığı bölümün altına aşağıdaki JavaScript kodunu ekleyin.

    ```JavaScript
    //Wait until the map resources are ready.
    map.events.add('ready', function() {

        //Create a data source and add it to the map.
        datasource = new atlas.source.DataSource();
        map.sources.add(datasource);

        //Add a layer for rendering point data.
        var resultLayer = new atlas.layer.SymbolLayer(datasource, null, {
            iconOptions: {
                image: 'pin-round-darkblue',
                anchor: 'center',
                allowOverlap: true
            },
            textOptions: {
                anchor: "top"
            }
        });

        map.layers.add(resultLayer);
    });
    ```

   Bu kod kesimi içinde bir `ready` olay kaynak eşleme yüklendikten ve erişilecek eşleme hazır olduğunda ateşlenir eşlemesine eklenir. Haritadaki `ready` olay işleyicisi, bir veri kaynağı sonuç verilerini depolamak için oluşturulur. Bir simge katmanı oluşturulur ve veri kaynağına eklenir. Bu katman, veri kaynağındaki sonuç verilerin nasıl işleneceğini belirtir. Bu örnekte sonuç koordinatlarının üzerinde ortalanmış olan ve diğer simgelerin çakışmasına izin verilen koyu mavi yuvarlak raptiye simgesi şeklindedir. Harita katmanları için sonuç katmanı eklendi.

<a id="usesearch"></a>

## <a name="add-search-capabilities"></a>Arama özellikleri ekleme

Bu bölümde, haritalar işlemi gösterilir [arama API'si](https://docs.microsoft.com/rest/api/maps/search) haritanızda ilgi noktası bulunamıyor. Bu, geliştiricilerin adres, ilgi çekici nokta ve diğer coğrafi bilgileri araması için tasarlanmış bir RESTful API’dir. Arama hizmeti, belirtilen bir adrese enlem ve boylam bilgileri atar. Aşağıda açıklanan **Hizmet Modülü**, Haritalar Arama API'si ile konum bulmaya yönelik aramalarda kullanılabilir.

### <a name="service-module"></a>Hizmet Modülü

1. Haritadaki `ready` olay işleyicisi aşağıdaki Javascript kodunu ekleyerek arama hizmeti URL'si oluşturun.

    ```JavaScript
   // Use SubscriptionKeyCredential with a subscription key
   var subscriptionKeyCredential = new atlas.service.SubscriptionKeyCredential(atlas.getSubscriptionKey());

   // Use subscriptionKeyCredential to create a pipeline
   var pipeline = atlas.service.MapsURL.newPipeline(subscriptionKeyCredential);

   // Construct the SearchURL object
   var searchURL = new atlas.service.SearchURL(pipeline); 
   ```

   `SubscriptionKeyCredential` Oluşturur bir `SubscriptionKeyCredentialPolicy` abonelik anahtarını Azure haritalar için HTTP isteklerinde kimlik doğrulaması için. `atlas.service.MapsURL.newPipeline()` Alır `SubscriptionKeyCredential` ilke ve oluşturan bir [işlem hattı](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.pipeline?view=azure-iot-typescript-latest) örneği. `searchURL` Azure haritalar için URL'yi temsil [arama](https://docs.microsoft.com/rest/api/maps/search) operations.

2. Ardından arama sorgusunu oluşturmak için aşağıdaki betik bloğunu ekleyin. Bu, Arama Hizmetinin temel arama API'si olan Belirsiz Arama Hizmetini kullanır. Belirsiz Arama Hizmeti adres, yer ve ilgi çekici nokta (POI) gibi çoğu belirsiz girişi işler. Bu kod sağlanan enlem ve boylamdan belirtilen yarıçap içindeki yakındaki yakınlarda arar. Kullanarak bir GeoJSON özellik koleksiyonundan yanıt ayıklanır `geojson.getFeatures()` yöntemi ve sembol katmanı aracılığıyla harita üzerinde işlenen verileri otomatik olarak sonuçlanan veri kaynağı eklenir. Betiğin son bölümü haritanın [setCamera](/javascript/api/azure-maps-control/atlas.map#setcamera-cameraoptions---cameraboundsoptions---animationoptions-) özelliğini kullanarak sonuçların sınırlayıcı kutusuna göre harita kamera görünümünü ayarlar.

    ```JavaScript
    var query =  'gasoline-station';
    var radius = 9000;
    var lat = 47.64452336193245;
    var lon = -122.13687658309935;

    searchURL.searchPOI(atlas.service.Aborter.timeout(10000), query, {
        limit: 10,
        lat: lat,
        lon: lon,
        radius: radius
    }).then((results) => {

        // Extract GeoJSON feature collection from the response and add it to the datasource
        var data = results.geojson.getFeatures();
        datasource.add(data);

        // set camera to bounds to show the results
        map.setCamera({
            bounds: data.bbox,
            zoom: 10
        });
    });
    ```

3. **MapSearch.html** dosyasını kaydedin ve tarayıcınızı yenileyin. Harita üzerinde Seattle alanında yakınlarda konumlarını işaretlemek gidiş-mavi iğne ortalanır görmelisiniz.

   ![Arama sonuçlarıyla haritayı görüntüleme](./media/tutorial-search-location/pins-map.png)

4. Tarayıcınıza aşağıdaki HTTP İsteğini girerek, haritanın işlediği ham verileri görebilirsiniz. \<Azure Haritalar Anahtarınız\> değerini birincil anahtarınızla değiştirin.

   ```http
   https://atlas.microsoft.com/search/poi/json?api-version=2&query=gasoline%20station&subscription-key=<subscription-key>&lat=47.6292&lon=-122.2337&radius=100000
   ```

Bu noktada MapSearch sayfası, belirsiz arama sorgusundan döndürülen ilgi çekici noktaların konumlarını görüntüleyebilir. Şimdi bazı etkileşimli özellikler ve konumlar hakkında daha fazla bilgi ekleyelim.

## <a name="add-interactive-data"></a>Etkileşimli veri ekleme

Şu ana kadar oluşturduğumuz harita, arama sonuçları için yalnızca boylam/enlem verilerine bakar. Ancak Haritalar Arama hizmetinin döndürdüğü işlenmemiş JSON’a bakarsanız, bunun ad ve açık adres de dahil olmak üzere her bir benzin istasyonuyla ilgili ek bilgiler içerdiğini görürsünüz. Etkileşimli açılır kutularla haritada bu verilere yer verebilirsiniz.

1. Aşağıdaki kod satırlarını haritada ekleme `ready` belirsiz arama hizmeti sorgulamak için kodu sonra olay işleyicisi. Bu kod bir açılır pencere örneği oluşturur ve simge katmanına fare ile üzerine gelindiğinde gerçekleştirilecek bir olay ekler.

    ```JavaScript
   //Create a popup but leave it closed so we can update it and display it later.
    popup = new atlas.Popup();

    //Add a mouse over event to the result layer and display a popup when this event fires.
    map.events.add('mouseover', resultLayer, showPopup);
    ```

    API `sup` sağlayan bir bilgi penceresi haritada gerekli konuma sabitlenmiş. 

2. İçinde *betik* etiketi, sonra `GetMap` işlev, açılan sonucu bilgilerinde üzerinden moused göstermek için aşağıdaki kodu ekleyin.

    ```JavaScript
    function showPopup(e) {
        //Get the properties and coordinates of the first shape that the event occured on.

        var p = e.shapes[0].getProperties();
        var position = e.shapes[0].getCoordinates();

        //Create HTML from properties of the selected result.
        var html = ['<div style="padding:5px"><div><b>', p.poi.name,
            '</b></div><div>', p.address.freeformAddress,
            '</div><div>', position[1], ', ', position[0], '</div></div>'];

        //Update the content and position of the popup.
        popup.setPopupOptions({
            content: html.join(''),
            position: position
        });

        //Open the popup.
        popup.open(map);
    }
    ```

3. Dosyayı kaydedin ve tarayıcınızı yenileyin. Şimdi tarayıcıdaki haritada, herhangi bir arama raptiyesinin üzerine gelip beklediğinizde bilgi açılan pencereleri gösterilir.

    ![Azure Harita Denetimi ve Arama Hizmeti](./media/tutorial-search-location/popup-map.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Azure Maps hesabı oluşturma
> * Hesabınızın birincil anahtarını alma
> * Harita Denetimi API’sini kullanarak yeni web sayfası oluşturma
> * Arama Hizmeti’ni kullanarak yakınlardaki istenen konumları bulma

> [!div class="nextstepaction"]
> [Tam kaynak kodu görüntüle](https://github.com/Azure-Samples/AzureMapsCodeSamples/blob/master/AzureMapsCodeSamples/Tutorials/search.html)

> [!div class="nextstepaction"]
> [Canlı örnek görünümü](https://azuremapscodesamples.azurewebsites.net/?sample=Search%20for%20points%20of%20interest)

Sonraki öğreticide, iki konum arasındaki bir yolun nasıl görüntüleneceği gösterilmektedir.

> [!div class="nextstepaction"]
> [Bir hedefe yönlendirme](./tutorial-route-location.md)
