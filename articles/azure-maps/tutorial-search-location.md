---
title: Azure Haritalar ile arama | Microsoft Docs
description: Azure Haritalar’ı kullanarak yakınlardaki ilgi çekici noktayı arama
author: dsk-2015
ms.author: dkshir
ms.date: 08/23/2018
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: e30d84c70f786a5bea25073c70a29b63c9a00ae9
ms.sourcegitcommit: ebb460ed4f1331feb56052ea84509c2d5e9bd65c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/24/2018
ms.locfileid: "42917671"
---
# <a name="search-nearby-points-of-interest-using-azure-maps"></a>Azure Haritalar’ı kullanarak yakınlardaki ilgi çekici noktaları arama

Bu öğreticide, Azure Haritalar hesabı ayarlama ve sonra Haritalar API’lerini kullanarak ilgi çekici bir noktayı arama işlemleri gösterilmektedir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure Haritalar hesabı oluşturma
> * Haritalar hesabınızın birincil anahtarını alma
> * Harita denetimi API’sini kullanarak yeni bir web sayfası oluşturma
> * Haritalar arama hizmetini kullanarak yakınlardaki bir ilgi çekici noktayı bulma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

## <a name="log-in-to-the-azure-portal"></a>Azure portalında oturum açma
[Azure Portal](https://portal.azure.com)’da oturum açın.

<a id="createaccount"></a>

## <a name="create-an-account-with-azure-maps"></a>Azure Maps hesabı oluşturma

Aşağıdaki adımları uygulayarak yeni bir Haritalar hesabı oluşturun:

1. [Azure portalının](https://portal.azure.com) sol üst köşesinde bulunan **Kaynak oluştur** öğesine tıklayın.
2. *Market’te Ara* kutusuna **Haritalar** yazın.
3. *Sonuçlar* içinden **Haritalar**’ı seçin. Haritanın altında görüntülenen **Oluştur** düğmesine tıklayın. 
4. **Haritalar Hesabı Oluştur** sayfasında aşağıdaki değerleri girin:
    - Yeni hesabınıza verilen *Ad*. 
    - Bu hesap için kullanmak istediğiniz *Abonelik*.
    - Bu hesap için *Kaynak grubu* adı. Kaynak grubu için *Yeni oluştur* veya *Mevcut olanı kullan* seçeneğini belirleyebilirsiniz.
    - *Kaynak grubu konumu*nu seçin.
    - *Lisans*’ı ve *Gizlilik Bildirimi*’ni okuyun ve onay kutusunu işaretleyerek koşulları kabul edin. 
    - **Oluştur** düğmesine tıklayın.
   
    ![Portalda Haritalar hesabı oluşturma](./media/tutorial-search-location/create-account.png)


<a id="getkey"></a>

## <a name="get-the-primary-key-for-your-account"></a>Hesabınızın birincil anahtarını alma

Haritalar hesabınız başarıyla oluşturulduktan sonra, Haritalar API’lerini sorgulamanıza olanak sağlayan anahtarı alın.

1. Portalda Haritalar hesabınızı açın.
2. Ayarlar bölümünde **Anahtarlar**’ı seçin.
3. **Birincil Anahtar**’ı panonuza kopyalayın. Bu öğreticinin ilerleyen kısmında kullanmak üzere bunu yerel olarak kaydedin. 

    ![Portalda Birincil Anahtar’ı alma](./media/tutorial-search-location/get-key.png)


<a id="createmap"></a>

## <a name="create-a-new-map"></a>Yeni harita oluşturma 
Harita Denetimi API’si, Haritalar’ı web uygulamanızla kolayca tümleştirmenize olanak tanıyan kullanışlı bir istemci kitaplığıdır. Açık REST hizmet çağrılarının karmaşıklığını gizlemesinin yanı sıra, stili değiştirilebilen ve özelleştirilebilen bileşenlerle üretkenliğinizi arttırır. Aşağıdaki adımlarda, Harita Denetimi API’sinin tümleşik olduğu statik bir HTML sayfasının nasıl oluşturulacağı gösterilmektedir. 

1. Yerel makinenizde yeni bir dosya oluşturun ve bu dosyayı **MapSearch.html** olarak adlandırın. 
2. Aşağıdaki HTML bileşenlerini dosyaya ekleyin:

    ```HTML
    <!DOCTYPE html>
    <html lang="en">

    <head>
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width, user-scalable=no" />
        <title>Map Search</title>

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
    HTML üst bilgisinin Azure Harita Denetimi kitaplığı tarafından barındırılan CSS ve JavaScript kaynak dosyalarını içerdiğine dikkat edin. HTML dosyasının *gövdesine* *Betik* segmentinin eklendiğine dikkat edin. Bu segment, Azure Haritalar API’lerine erişime yönelik satır içi JavaScript kodunu içerir.
 
3. HTML dosyasının *betik* bloğuna aşağıdaki JavaScript kodunu ekleyin. **\<Hesap anahtarınız\>** dizesini, Haritalar hesabınızdan kopyaladığınız birincil anahtarla değiştirin.

    ```JavaScript
    // Instantiate map to the div with id "map"
    var MapsAccountKey = "<your account key>";
    var map = new atlas.Map("map", {
        "subscription-key": MapsAccountKey
    });
    ```
    Bu segment, Azure Haritalar hesap anahtarınız için Harita Denetimi API’sini başlatır. **Atlas**, API ve ilgili görsel bileşenleri içeren ad alanıdır. **Atlas.Map**, görsel ve etkileşimli bir web haritası için gerekli denetimi sağlar. 
    
4. Değişikliklerinizi dosyaya kaydedin ve HTML sayfasını bir tarayıcıda açın. Bu, **atlas.map** komutunu çağırıp hesap anahtarınızı sağlayarak oluşturabileceğiniz en temel haritadır. 

   ![Haritayı görüntüleme](./media/tutorial-search-location/basic-map.png)


<a id="usesearch"></a>

## <a name="add-search-capabilities"></a>Arama özellikleri ekleme

Bu bölümde, Haritalar Arama API’sini kullanarak haritanızda ilgi çekici bir noktayı nasıl bulabileceğiniz gösterilmektedir. Bu, geliştiricilerin adres, ilgi çekici nokta ve diğer coğrafi bilgileri araması için tasarlanmış bir RESTful API’dir. Arama hizmeti, belirtilen bir adrese enlem ve boylam bilgileri atar. Aşağıda açıklanan **Hizmet Modülü**, Haritalar Arama API'si ile konum bulmaya yönelik aramalarda kullanılabilir.

### <a name="service-module"></a>Hizmet Modülü

1. Arama sonuçlarını görüntülemek için haritanıza yeni bir katman ekleyin. Aşağıdaki JavaScript kodunu betik bloğunda, haritayı başlatan kodun arkasına ekleyin. 
    
    ```JavaScript
    // Initialize the pin layer for search results to the map
    var searchLayerName = "search-results";
    map.addPins([], {
        name: searchLayerName,
        cluster: false,
        icon: "pin-round-darkblue"
    });
    ```

2. İstemci hizmetini başlatmak için aşağıdaki Javascript kodunu betik bloğunda, haritayı başlatan kodun arkasına ekleyin.

    ```JavaScript
    var client = new atlas.service.Client(subscriptionKey);
    ```

3. Sorguyu oluşturmak için aşağıdaki betik bloğunu ekleyin. Bu, Arama Hizmetinin temel arama API'si olan Belirsiz Arama Hizmetini kullanır. Belirsiz Arama Hizmeti, adres ve ilgi çekici nokta (POI) belirteci kombinasyonları gibi belirsiz girişlerin çoğunu işler. Belirtilen yarıçap içinde olup yakında bulunan Benzin İstasyonlarını arar. Ardından yanıt GeoJSON biçimine ayrıştırılır, nokta özelliğine dönüştürülür ve bunlar haritaya iğne olarak eklenir. Betiğin son kısmı, Harita'nın [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-control/models.cameraboundsoptions?view=azure-iot-typescript-latest) özelliğini kullanarak harita için kamera sınırları ekler.

    ```JavaScript
    client.search.getSearchFuzzy("gasoline station", {
     lat: 47.6292,
     lon: -122.2337,
     radius: 100000
    }).then(response => {
       // Parse the response into GeoJSON 
       var geojsonResponse = new atlas.service.geojson.GeoJsonSearchResponse(response); 
 
       // Create the point features that will be added to the map as pins 
       var searchPins = geojsonResponse.getGeoJsonResults().features.map(poiResult => { 
           var poiPosition = [poiResult.properties.position.lon, poiResult.properties.position.lat]; 
           return new atlas.data.Feature(new atlas.data.Point(poiPosition), { 
                name: poiResult.properties.poi.name, 
                address: poiResult.properties.address.freeformAddress, 
                position: poiPosition[1] + ", " + poiPosition[0] 
           }); 
       }); 
 
       // Add pins to the map for each POI 
       map.addPins(searchPins, { 
           name: searchLayerName 
       }); 
 
       // Set the camera bounds 
       map.setCameraBounds({ 
           bounds: geojsonResponse.getGeoJsonResults().bbox, 
           padding: 50 
       ); 
    }); 
    ```
4. **MapSearch.html** dosyasını kaydedin ve tarayıcınızı yenileyin. Şimdi haritanın Seattle’da ortalandığını ve bölgedeki benzin istasyonu konumlarının mavi raptiyelerle işaretlendiğini görmeniz gerekir. 

   ![Arama sonuçlarıyla haritayı görüntüleme](./media/tutorial-search-location/pins-map.png)

5. Tarayıcınıza aşağıdaki HTTP İsteğini girerek, haritanın işlediği ham verileri görebilirsiniz. \<Hesap anahtarınızı\> birincil anahtarınızla değiştirin. 

   ```http
   https://atlas.microsoft.com/search/fuzzy/json?api-version=1.0&query=gasoline%20station&subscription-key=<your account key>&lat=47.6292&lon=-122.2337&radius=100000
   ```

Bu noktada MapSearch sayfası, belirsiz arama sorgusundan döndürülen ilgi çekici noktaların konumlarını görüntüleyebilir. Şimdi bazı etkileşimli özellikler ve konumlar hakkında daha fazla bilgi ekleyelim. 

## <a name="add-interactive-data"></a>Etkileşimli veri ekleme

Şu ana kadar oluşturduğumuz harita, arama sonuçları için yalnızca enlem/boylam verilerine bakar. Ancak Haritalar Arama hizmetinin döndürdüğü işlenmemiş JSON’a bakarsanız, bunun ad ve açık adres de dahil olmak üzere her bir benzin istasyonuyla ilgili ek bilgiler içerdiğini görürsünüz. Etkileşimli açılır kutularla haritada bu verilere yer verebilirsiniz. 

1. Arama Hizmeti tarafından döndürülen istenen noktalara yönelik açılan pencereler oluşturmak için *betik* bloğuna aşağıdaki satırları ekleyin:

    ```JavaScript
    // Add a popup to the map which will display some basic information about a search result on hover over a pin
    var popup = new atlas.Popup();
    map.addEventListener("mouseover", searchLayerName, (e) => {
        var popupContentElement = document.createElement("div");
        popupContentElement.style.padding = "5px";

        var popupNameElement = document.createElement("div");
        popupNameElement.innerText = e.features[0].properties.name;
        popupContentElement.appendChild(popupNameElement);

        var popupAddressElement = document.createElement("div");
        popupAddressElement.innerText = e.features[0].properties.address;
        popupContentElement.appendChild(popupAddressElement);

        var popupPositionElement = document.createElement("div");
        popupPositionElement.innerText = e.features[0].properties.position;
        popupContentElement.appendChild(popupPositionElement);

        popup.setPopupOptions({
            position: e.features[0].geometry.coordinates,
            content: popupContentElement
        });

        popup.open(map);
    });
    ```
    **atlas.Popup** API’si, haritada gerekli konuma sabitlenmiş bir bilgi penceresi sağlar. Bu kod parçacığı, açılan pencerenin içeriğini ve konumunu belirlemesinin yanı sıra _farenin_ açılan pencerenin üzerine gelmesini bekleyen bir `map` denetimi ekler. 

4. Dosyayı kaydedin ve tarayıcınızı yenileyin. Şimdi tarayıcıdaki haritada, herhangi bir arama raptiyesinin üzerine gelip beklediğinizde bilgi açılan pencereleri gösterilir. 

    ![Azure Harita Denetimi ve Arama Hizmeti](./media/tutorial-search-location/popup-map.png)


## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Azure Maps hesabı oluşturma
> * Hesabınızın birincil anahtarını alma
> * Harita Denetimi API’sini kullanarak yeni web sayfası oluşturma
> * Arama Hizmeti’ni kullanarak yakınlardaki istenen konumları bulma

Sonraki öğreticide, iki konum arasındaki bir yolun nasıl görüntüleneceği gösterilmektedir. 

> [!div class="nextstepaction"]
> [Bir hedefe yönlendirme](./tutorial-route-location.md)
