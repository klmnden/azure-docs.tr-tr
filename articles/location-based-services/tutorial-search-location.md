---
title: "Azure Konum Tabanlı Hizmetler ile arama | Microsoft Docs"
description: "Azure Konum Tabanlı Hizmetler ile yakınlardaki istenen konumları arama"
services: location-based-services
keywords: 
author: kgremban
ms.author: kgremban
ms.date: 11/28/2017
ms.topic: tutorial
ms.service: location-based-services
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: f03cdd5491868b78de0514bb66184235dd7df5c2
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="search-nearby-points-of-interest-using-azure-location-based-services"></a>Azure Konum Tabanlı Hizmetler’i kullanarak yakınlardaki istenen konumları arama

Bu öğreticide, bir Azure Konum Tabanlı Hizmetler hesabı ayarlama ve sağlanan API’leri kullanarak istenen bir konumu arama işlemleri gösterilmektedir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure Konum Tabanlı Hizmetler hesabı oluşturma
> * Azure Konum Tabanlı Hizmetler hesabınızın birincil anahtarını öğrenme
> * Harita Denetimi API’sini kullanarak yeni web sayfası oluşturma
> * Arama Hizmeti’ni kullanarak yakınlardaki istenen konumu bulma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

## <a name="log-in-to-the-azure-portal"></a>Azure portalında oturum açma
[Azure Portal](https://portal.azure.com)’da oturum açın.

<a id="createaccount"></a>

## <a name="create-an-account-with-azure-location-based-services"></a>Azure Konum Tabanlı Hizmetler hesabı oluşturma

Yeni bir Konum Tabanlı Hizmetler hesabı oluşturmak için bu adımları izleyin.

1. [Azure portalının](https://portal.azure.com) sol üst köşesinde bulunan **Kaynak oluştur** öğesine tıklayın.
2. *Market’te Ara* kutusunda, **konum tabanlı hizmetler** yazın.
3. *Sonuçlar* sayfasından, **Konum Tabanlı Hizmetler (önizleme)** seçeneğine tıklayın. Haritanın altında görüntülenen **Oluştur** düğmesine tıklayın. 
4. **Konum Tabanlı Hizmetler Hesabı Oluştur** sayfasında aşağıdaki değerleri girin:
    - Yeni hesabınıza verilen *Ad*. 
    - Bu hesap için kullanmak istediğiniz *Abonelik*.
    - Bu hesap için *Kaynak grubu* adı. Kaynak grubu için *Yeni oluştur* veya *Mevcut olanı kullan* seçeneğini belirleyebilirsiniz.
    - *Kaynak grubu konumu*nu seçin.
    - *Önizleme Koşulları*’nı okuyun ve kabul etmek için onay kutusunu işaretleyin. 
    - Son olarak **Oluştur** düğmesine tıklayın.
   
    ![Portalda Konum Tabanlı Hizmetler Hesabı oluşturma](./media/tutorial-search-location/create-lbs-account.png)


<a id="getkey"></a>

## <a name="get-the-primary-key-for-your-account"></a>Hesabınızın birincil anahtarını alma

Konum Tabanlı Hizmetler hesabınız başarıyla oluşturulduktan sonra hesabı harita arama API’lerine bağlamak için şu adımları izleyin:

1. Portalda Konum Tabanlı Hizmetler hesabınızı açın.
2. Hesabınızın **AYARLAR** bölümüne gidip **Anahtarlar**’ı seçin.
3. **Birincil Anahtar**’ı panonuza kopyalayın. Sonraki adımlarda kullanmak üzere yerel olarak kaydedin. 

    ![Portalda Birincil Anahtar’ı alma](./media/tutorial-search-location/lbs-get-key.png)


<a id="createmap"></a>

## <a name="create-new-web-page-using-azure-map-control-api"></a>Azure Harita Denetimi API’sini kullanarak yeni web sayfası oluşturma
Azure Harita Denetimi API'si, Azure Konum Tabanlı Hizmetler’i web uygulamanızla kolayca tümleştirmenize olanak tanıyan kullanışlı bir istemci kitaplığıdır. Açık REST hizmet çağrılarının karmaşıklığını gizlemesinin yanı sıra, stili değiştirilebilen ve özelleştirilebilen bileşenlerle üretkenliğinizi arttırır. Aşağıdaki adımlarda, Konum Tabanlı Hizmetler’in Harita Denetimi API’sinin ekli olduğu statik bir HTML sayfası oluşturma işlemi gösterilmiştir. 

1. Yerel makinenizde yeni bir dosya oluşturun ve bu dosyayı **MapSearch.html** olarak adlandırın. 
2. Aşağıdaki HTML bileşenlerini dosyaya ekleyin:

    ```HTML
    <!DOCTYPE html>
    <html lang="en">

    <head>
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width, user-scalable=no" />
        <title>Map Search</title>

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
    HTML üst bilgisinin Azure Harita Denetimi kitaplığı tarafından barındırılan CSS ve JavaScript kaynak dosyalarını içerdiğine dikkat edin. HTML dosyasının *gövdesine* *Betik* segmentinin eklendiğine dikkat edin. Bu segment, Azure Konum Tabanlı Hizmetler API’lerine erişime yönelik satır içi JavaScript kodunu içerir.
 
3.  HTML dosyasının *betik* bloğuna aşağıdaki JavaScript kodunu ekleyin. Betikte Konum Tabanlı Hizmetler hesabınızın birincil anahtarını kullanın. 

    ```JavaScript
    // Instantiate map to the div with id "map"
    var LBSAccountKey = "<_your account key_>";
    var map = new atlas.Map("map", {
        "subscription-key": LBSAccountKey
    });
    ```
    Bu segment, Azure Konum Tabanlı Hizmetler hesap anahtarınız için Harita Denetimi API’sini başlatır. **Atlas**, Azure Harita Denetimi API’sini ve ilgili görsel bileşenleri içeren ad alanıdır. **atlas.Map**, görsel ve etkileşimli bir web haritası için gerekli denetimi sağlar. HTML sayfasını tarayıcıda açarak haritanın nasıl göründüğüne bakabilirsiniz. 

4. Harita Denetimi’ne bir arama raptiyeleri katmanı eklemek için *betik* bloğuna aşağıdaki JavaScript kodunu ekleyin:

    ```JavaScript
    // Initialize the pin layer for search results to the map
    var searchLayerName = "search-results";
    map.addPins([], {
        name: searchLayerName,
        cluster: false,
        icon: "pin-round-darkblue"
    });
    ```

5. Dosyayı makinenize kaydedin. 


<a id="usesearch"></a>

## <a name="use-search-service-to-find-nearby-point-of-interest"></a>Arama Hizmeti’ni kullanarak yakınlardaki istenen konumları bulma

Bu bölümde, Konum Tabanlı Hizmetler’in Harita Denetimi API’sini kullanarak haritanızda istediğiniz bir konumu nasıl bulabileceğiniz gösterilmiştir. Bu, geliştiricilerin adres, istenen nokta ve diğer coğrafi bilgileri araması için tasarlanmış bir RESTful API’dir. Arama Hizmeti, belirtilen bir adrese enlem ve boylam bilgileri atar. 

1. Arama Hizmeti’ni göstermek için, önceki bölümde oluşturulan **MapSearch.html** dosyasını açın ve *betik* bloğuna aşağıdaki JavaScript kodunu ekleyin. 
    ```JavaScript
    // Perform a request to the search service and create a pin on the map for each result
    var xhttp = new XMLHttpRequest();
    xhttp.onreadystatechange = function () {
        var searchPins = [];

        if (this.readyState === 4 && this.status === 200) {
            var response = JSON.parse(this.responseText);

            var poiResults = response.results.filter((result) => { return result.type === "POI" }) || [];

            searchPins = poiResults.map((poiResult) => {
                var poiPosition = [poiResult.position.lon, poiResult.position.lat];
                return new atlas.data.Feature(new atlas.data.Point(poiPosition), {
                    name: poiResult.poi.name,
                    address: poiResult.address.freeformAddress,
                    position: poiResult.position.lat + ", " + poiResult.position.lon
                });
            });

            map.addPins(searchPins, {
                name: searchLayerName
            });

            var lons = searchPins.map((pin) => { return pin.geometry.coordinates[0] });
            var lats = searchPins.map((pin) => { return pin.geometry.coordinates[1] });

            var swLon = Math.min.apply(null, lons);
            var swLat = Math.min.apply(null, lats);
            var neLon = Math.max.apply(null, lons);
            var neLat = Math.max.apply(null, lats);

            map.setCameraBounds({
                bounds: [swLon, swLat, neLon, neLat],
                padding: 50
            });
        }
    };
    ```
    Bu kod parçacığı bir [XMLHttpRequest](https://xhr.spec.whatwg.org/) oluşturur ve gelen yanıtları ayrıştırmak için bir olay işleyicisi ekler. Yanıtın başarılı olması için `searchPins` değişkeninde döndürülen her konumun adres, ad, enlem ve boylam bilgilerini toplar. Son olarak, bu konum koleksiyonunu raptiyeler olarak `map` denetimine ekler. 

2. *Betik* bloğuna aşağıdaki kodu ekleyerek Azure Konum Tabanlı Hizmetler’in Arama Hizmeti’ne XMLHttpRequest gönderin:

    ```JavaScript
    var url = "https://atlas.microsoft.com/search/fuzzy/json?";
    url += "&api-version=1.0";
    url += "&query=gasoline%20station";
    url += "&subscription-key=" + LBSAccountKey;
    url += "&lat=47.6292";
    url += "&lon=-122.2337";
    url += "&radius=100000";

    xhttp.open("GET", url, true);
    xhttp.send();
    ``` 
    Bu kod parçacığında adlı arama hizmeti temel arama API kullanan **benzer arama**. En belirsiz girdileri bile işleyebildiğinden, tüm adres veya *POI* belirteçlerini işler. Belirtilen enlem ve boylamdaki adres için, belirtilen yarıçap içindeki **petrol istasyonlarını** arar. Hesabınızın daha önce örnek dosyada sağlanan birincil anahtarını kullanarak Konum Tabanlı Hizmetler’e çağrı yapar. Bulunan konumlar için sonuçları enlem/boylam çiftleri olarak döndürür. HTML sayfasını tarayıcıda açarak arama raptiyelerini gözlemleyebilirsiniz. 

3. Arama Hizmeti tarafından döndürülen istenen noktalara yönelik açılan pencereler oluşturmak için *betik* bloğuna aşağıdaki satırları ekleyin:

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

4. Dosyayı kaydedin ve sonra dilediğiniz web tarayıcısında **MapSearch.html** dosyasını açarak sonucu inceleyin. Bu noktada, tarayıcıdaki haritada gösterilen herhangi bir arama raptiyesinin üzerine gelip beklediğinizde aşağıdakine benzeyen bilgi açılan pencereleri gösterilir. 

    ![Azure Harita Denetimi ve Arama Hizmeti](./media/tutorial-search-location/lbs-map-search.png)


## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Azure Konum Tabanlı Hizmetler hesabı oluşturma
> * Hesabınızın birincil anahtarını alma
> * Harita Denetimi API’sini kullanarak yeni web sayfası oluşturma
> * Arama Hizmeti’ni kullanarak yakınlardaki istenen konumları bulma

Azure Konum Tabanlı Hizmetler’i kullanarak istediğiniz konumun yol tarifini almayı öğrenmek için [Azure Konum Tabanlı Hizmetler ile istenen konuma yol tarifi alma](./tutorial-route-location.md) öğreticisine geçin. 
