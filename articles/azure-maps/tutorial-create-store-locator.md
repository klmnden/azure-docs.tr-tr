---
title: Azure haritalar'ı kullanarak bir depolama Bulucu | Microsoft Docs
description: Azure haritalar'ı kullanarak bir depolama Bulucu oluşturun.
author: walsehgal
ms.author: v-musehg
ms.date: 11/15/2018
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 61fdaec79e563ba4d87e73b22aba52a5c3f8251b
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59270812"
---
# <a name="create-a-store-locator-by-using-azure-maps"></a>Azure haritalar'ı kullanarak bir depolama Bulucu

Bu öğreticide Azure haritalar'ı kullanarak bir basit deposu Bulucu oluşturma işleminde size kılavuzluk eder. Store bulucular yaygındır. Bu tür bir uygulama içinde kullanılan kavramlardan bir çoğunu diğer türlerde uygulamalar için geçerlidir. Müşterilere deposu Bulucunun sunan tüketicilere doğrudan satış çoğu işletmenin için bir zorunluluktur. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
    
> [!div class="checklist"]
> * Azure harita denetimi API'sini kullanarak yeni bir Web sayfası oluşturun.
> * Bir dosyadan özel veri yüklemek ve bir haritada görüntüler.
> * Azure haritalar arama hizmeti bir sorgu girin ya da bir adresini bulmak için kullanın.
> * Kullanıcının konumuna bir tarayıcıdan alın ve haritayı göster.
> * Harita üzerinde özel semboller oluşturmak için birden çok katmanları birleştirin.  
> * Veri noktaları kümesi.  
> * Haritaya yakınlaştırma denetimleri ekleyin.

<a id="Intro"></a>

Bölümüne geçin [Canlı deposu Bulucu örneği](https://azuremapscodesamples.azurewebsites.net/?sample=Simple%20Store%20Locator) veya [kaynak kodu](https://github.com/Azure-Samples/AzureMapsCodeSamples/tree/master/AzureMapsCodeSamples/Tutorials/Simple%20Store%20Locator). 

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticideki adımları tamamlamak için önce yapmanız [Azure haritalar hesabınızda oluşturma](./tutorial-search-location.md#createaccount) ve [hesabınız için bir abonelik anahtarı edinirler](./tutorial-search-location.md#getkey).

## <a name="design"></a>Tasarım

Kodun içine kullanmaya başlamadan önce bir tasarım ile başlamak için iyi bir fikirdir. Depolama Bulucu olmasını istediğiniz kadar basit veya karmaşık olabilir. Bu öğreticide, bir basit deposu Bulucu oluştururuz. Süreç boyunca isterseniz bazı işlevlerini genişletin yardımcı olması için bazı ipuçları ekliyoruz. Kahve Contoso adlı kurgusal bir şirkete yönelik bir depo Bulucu oluştururuz. Bu öğreticide ekleriz deposu Bulucu genel düzeninin bir Tel Çerçeve aşağıdaki şekilde gösterilmiştir:

<br/>
<center>

![Contoso kahve kahve Dükkanı konumları için depolama Bulucunun Tel Çerçeve](./media/tutorial-create-store-locator/SimpleStoreLocatorWireframe.png)</center>

Bu depolama Bulucu kullanışlılığını en üst düzeye çıkarmak için bir kullanıcının ekran genişliği geniş 700 pikselden küçük olduğunda ayarlayan esnek bir düzene ekliyoruz. Esnek bir düzene deposu Bulucu görüntüleyerek küçük ekranda, mobil cihazda gibi kullanmayı kolaylaştırır. Küçük ekran düzeninin bir Tel Çerçeve şu şekildedir:  

<br/>
<center>

![Mobil cihazda Tel Çerçeve Contoso kahve depolamak Bulucu](./media/tutorial-create-store-locator/SimpleStoreLocatorMobileWireframe.png)</center>

Tel Çerçeve oldukça basit bir uygulama gösterir. Uygulama, bir arama kutusu, yakındaki depolarının bir listesi, bazı işaretçiler (simge) sahip bir eşlem ve kullanıcı bir işaret seçtiğinde, ek bilgileri görüntüleyen bir pencere vardır. Daha ayrıntılı olarak, bu Öğreticide bu depolama Bulucu ekleriz özellikleri şunlardır:

* İçeri aktarılan sekmeyle ayrılmış veri dosyasındaki tüm konumlarda haritada yüklenir.
* Kullanıcı Kaydır ve harita yakınlaştırma, bir arama yapın ve My konumu GPS düğmesini seçin.
* Sayfa düzeni, cihaz ekran genişliği göre ayarlar.  
* Mağaza logosu bir başlık gösterilir.  
* Kullanıcı, bir arama kutusu ve Ara düğmesine, adresi, şehir veya posta kodu gibi bir konumu aramak için kullanabilirsiniz. 
* A `keypress` arama kutusuna eklenen olayı, kullanıcı Enter bastığında bir arama tetikler. Bu işlev sık gözden kaçan, ancak daha iyi bir kullanıcı deneyimi oluşturur.
* Harita hareket ettiğinde, her konumun uzaklık harita Merkezi'nden hesaplanır. Sonuç listesinde en yakın konumları haritanın en üstünde görüntülemek için güncelleştirilir.  
* Sonuçlar listesinde bir sonucu seçtiğinizde, haritada seçilen konum ortalanır ve konumu hakkındaki bilgiler açılan bir pencerede görüntülenir.  
* Harita üzerinde belirli bir konum seçildiğinde, bir açılır pencere tetikler.
* Kullanıcı uzaklaştırılır, konumları kümelerinde gruplandırılır. Kümeleri bir daire daire içine bir sayı ile temsil edilir. Kullanıcının yakınlaştırma düzeyini değiştikçe, form ve ayrı kümeleri.
* Küme seçme harita iki düzeyde yakınlaştırır ve kümenin konumu ortalar.

<a id="create a data-set"></a>

## <a name="create-the-store-location-dataset"></a>Depolama konumu veri kümesi oluşturma

Depolama Bulucu uygulaması ekibiyiz önce biz haritada görüntülemek istiyoruz depolarının bir veri kümesi oluşturmanız gerekir. Bu öğreticide, bir kurgusal Contoso kahve adlı kafeterya için bir veri kümesi kullanıyoruz. Bu basit deposu Bulucu için veri kümesi, bir Excel çalışma kitabında yönetilir. Veri kümesini içeren Contoso kahve kahve Dükkanı konumları arasında dokuz farklı ülkede yayılan 10,213: Amerika Birleşik Devletleri, Kanada, Birleşik Krallık, Fransa, Almanya, İtalya, Hollanda, Danimarka ve İspanya. Verileri şuna benzer bir ekran görüntüsü aşağıda verilmiştir:

<br/>
<center>

![Bir Excel çalışma kitabındaki verileri depola Bulucu ekran görüntüsü](./media/tutorial-create-store-locator/StoreLocatorDataSpreadsheet.png)</center>

Yapabilecekleriniz [Excel çalışma kitabını indirin](https://github.com/Azure-Samples/AzureMapsCodeSamples/tree/master/AzureMapsCodeSamples/Tutorials/Simple%20Store%20Locator/data). 

Verilerin ekran arıyorsanız, aşağıdaki gözlemlere yapabilirsiniz:
    
* Konum bilgileri kullanarak depolandığı **AddressLine**, **Şehir**, **belediye** (ilçe) **AdminDivision** (bölge) , **Posta kodu** (posta kodu), ve **Ülke** sütunları.  
* **Enlem** ve **boylam** sütunları her Contoso kahve kahve Dükkanı konum koordinatlarını içerir. Koordinatlar Information yoksa, Azure haritalar arama hizmetleri konum koordinatlarını belirlemek için kullanabilirsiniz.
* Bazı ek sütunlar için kahve dükkanları ilgili meta veriler içerir: bir telefon numarası, Wi-Fi etkin nokta ve Tekerlekli erişilebilirlik ve açılış ve kapanış 24 saat biçiminde kez deposu için Boolean sütunlar. Konum verilerinizi daha ilgili meta veriler içeren kendi sütunlarınızı oluşturabilirsiniz.

> [!Note]
> Azure haritalar küresel Mercator izdüşümünü "EPSG:3857" verileri işleyen, ancak "EPSG:4325" WGS84 datum kullanan verileri okur. 

Uygulama için veri kümesi göstermek için birçok yolu vardır. Tek bir veritabanı'na veri yükleme ve verileri sorgular ve sonuçları kullanıcının tarayıcısına gönderen bir web hizmeti kullanıma sunmak için bir yaklaşımdır. Bu seçenek, büyük veri kümeleri için veya sık sık güncelleştirilir veri kümeleri için idealdir. Ancak, bu seçenek, önemli ölçüde daha fazla geliştirme iş gerektirir ve daha yüksek bir maliyet vardır. 

Bu veri kümesi tarayıcı kolayca ayrıştırabilen bir düz metin dosyasına dönüştürmek başka bir yaklaşımdır. Uygulama geri kalanı ile dosya barındırılabilir. Bu seçenek işleri basitleştiriyor ancak kullanıcının indirdiği tüm veriler için daha küçük veri kümeleri için yalnızca iyi bir seçenek değildir. Veri dosyası boyutu 1 MB'den küçük olduğundan bu veri kümesi için düz metin dosyası kullanırız.  

Çalışma kitabı bir düz metin dosyasına dönüştürmek için çalışma kitabını sekmeyle ayrılmış bir dosya kaydedin. Her sütun, sütunlar bizim kodda kolay ayrıştırılır olmasını sağlayan bir sekme karakteri tarafından sınırlandırılmıştır. Virgülle ayrılmış değer (CSV) biçimini kullanabilirsiniz, ancak bu seçenek daha fazla ayrıştırma mantık gerektirir. Çevresinde virgül olan herhangi bir alan, tırnak işaretleri ile sarmalanmış. Sekmeyle ayrılmış bir Excel dosyasında bu verileri dışarı aktarmak için seçin **Kaydet**. İçinde **farklı kaydetme türü** aşağı açılan listesinden **metin (delimited)(*.txt) sekmesinde**. Dosya adı *ContosoCoffee.txt*. 

<br/>
<center>

![Ekran görüntüsü türü iletişim kutusu olarak Kaydet](./media/tutorial-create-store-locator/SaveStoreDataAsTab.png)</center>

Metin dosyasını Not Defteri'nde açın, aşağıdaki şekilde için benzer:

<br/>
<center>

![Sekmeyle ayrılmış bir veri kümesi gösteren bir not defteri dosyası ekran görüntüsü](./media/tutorial-create-store-locator/StoreDataTabFile.png)</center>


## <a name="set-up-the-project"></a>Projesi kurun

Projeyi oluşturmak için kullanabileceğiniz [Visual Studio](https://visualstudio.microsoft.com) veya tercih ettiğiniz Kod Düzenleyicisi. Proje klasörünüzdeki üç dosyayı oluşturma: *index.html*, *index.css*, ve *index.js*. Düzen, stil ve mantıksal uygulama için bu dosyaları tanımlayın. Adlı bir klasör oluşturun *veri* ve ekleme *ContosoCoffee.txt* klasörüne. Adlı başka bir klasör oluşturun *görüntüleri*. On görüntüleri bu uygulamada simgeler, düğmeler ve harita üzerinde işaretçileri için kullanırız. Yapabilecekleriniz [bu görüntüleri indirin](https://github.com/Azure-Samples/AzureMapsCodeSamples/tree/master/AzureMapsCodeSamples/Tutorials/Simple%20Store%20Locator/data). Proje klasörünüzdeki, aşağıdaki şekilde gibi görünmelidir:

<br/>
<center>

![Basit Store Bulucu proje klasörünün ekran görüntüsü](./media/tutorial-create-store-locator/StoreLocatorVSProject.png)</center>

## <a name="create-the-user-interface"></a>Kullanıcı arabirimi oluşturma

Kullanıcı arabirimi oluşturmak için kodu ekleyin. *index.html*:

1. Aşağıdaki `meta` için etiketler `head` , *index.html*. Etiketleri (UTF-8) karakter kümesi tanımlamak, Internet Explorer ve Microsoft Edge tarayıcı en son sürümleri kullanın söyleyin ve hızlı yanıt veren düzenleri için de geçerlidir bir Görünüm penceresi belirtin.

    ```HTML
    <meta charset="utf-8">
    <meta http-equiv="x-ua-compatible" content="IE=Edge">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
    ```

1. Azure haritalar web denetimi JavaScript ve CSS dosyalarındaki başvuruları ekleyin:

    ```HTML
    <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.css" type="text/css">
    <script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.js"></script>
    ```

1. Azure haritalar Hizmetleri modülüne bir başvuru ekleyin. Azure haritalar REST Hizmetleri sarmalar ve kullanımı kolay javascript'teki yaptığına bir JavaScript kitaplığı modülüdür. Modül, arama işlevselliği destekleyen için kullanışlıdır.

    ```HTML
    <script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas-service.min.js"></script>
    ```

1. Başvuruları Ekle *index.js* ve *index.css*:

    ```HTML
    <link rel="stylesheet" href="index.css" type="text/css">
    <script src="index.js"></script>
    ```

1. Belge gövdesinde ekleyin bir `header` etiketi. İçinde `header` etiketinde, logo ve şirket adını ekleyin.

    ```HTML
    <header>
        <img src="images/Logo.png" />
        <span>Contoso Coffee</span>
    </header>
    ```

1. Ekleme bir `main` etiket ve metin kutusu ve arama düğmesini içeren bir arama panelini oluşturun. Ayrıca, ekleme `div` harita, liste bölmesi ve My konumu GPS düğmesi için başvuruları.

    ```HTML
    <main>
        <div class="searchPanel">
            <div>
                <input id="searchTbx" type="search" placeholder="Find a store" />
                <button id="searchBtn" title="Search"></button>
            </div>
        </div>
        <div id="listPanel"></div>
        <div id="myMap"></div>
        <button id="myLocationBtn" title="My Location"></button>
    </main>
    ```

İşiniz bittiğinde, *index.html* gibi görünmelidir [Bu örnek index.html dosyası](https://github.com/Azure-Samples/AzureMapsCodeSamples/blob/master/AzureMapsCodeSamples/Tutorials/Simple%20Store%20Locator/index.html).

Sonraki adım, CSS stilleri tanımlamaktır. CSS stilleri uygulama bileşenlerini nasıl düzenlendiği ve uygulamanın görünümünü tanımlayın. Açık *index.css* ve aşağıdaki kodu ekleyin. `@media` Stili ekran genişliği 700 pikselden küçük olduğunda kullanılacak alternatif stil seçeneklerini tanımlar.  

   ```CSS
    html, body {
        padding: 0;
        margin: 0;
        font-family: Gotham, Helvetica, sans-serif;
        overflow-x: hidden;
    }

    header {
        width: calc(100vw - 10px);
        height: 30px;
        padding: 15px 0 20px 20px;
        font-size: 25px;
        font-style: italic;
        font-family: "Comic Sans MS", cursive, sans-serif;
        line-height: 30px;
        font-weight: bold;
        color: white;
        background-color: #007faa;
    }

    header span {
        vertical-align: middle;
    }

    header img {
        height: 30px;
        vertical-align: middle;
    }

    .searchPanel {
        position: relative;
        width: 350px;
    }

    .searchPanel div {
        padding: 20px;
    }

    .searchPanel input {
        width: calc(100% - 50px);
        font-size: 16px;
        border: 0;
        border-bottom: 1px solid #ccc;
    }

    #listPanel {
        position: absolute;
        top: 135px;
        left: 0px;
        width: 350px;
        height: calc(100vh - 135px);
        overflow-y: auto;
    }

    #myMap { 
        position: absolute;
        top: 65px;
        left: 350px;
        width: calc(100vw - 350px);
        height: calc(100vh - 65px);
    }

    .statusMessage {
        margin: 10px;
    }

    #myLocationBtn, #searchBtn {
        margin: 0;
        padding: 0;
        border: none;
        border-collapse: collapse;
        width: 32px;
        height: 32px; 
        text-align: center;
        cursor: pointer;
        line-height: 32px;
        background-repeat: no-repeat;
        background-size: 20px;
        background-position: center center;
        z-index: 200;
    }

    #myLocationBtn {
        position: absolute;
        top: 150px;
        right: 10px;
        box-shadow: 0px 0px 4px rgba(0,0,0,0.16);
        background-color: white;
        background-image: url("images/GpsIcon.png");
    }

    #myLocationBtn:hover {
        background-image: url("images/GpsIcon-hover.png");
    }

    #searchBtn {
        background-color: transparent;
        background-image: url("images/SearchIcon.png");
    }

    #searchBtn:hover {
        background-image: url("images/SearchIcon-hover.png");
    }

    .listItem {
        height: 50px;
        padding: 20px;
        font-size: 14px;
    }

    .listItem:hover {
        cursor: pointer;
        background-color: #f1f1f1;
    }

    .listItem-title {
        color: #007faa;
        font-weight: bold;
    }

    .storePopup {
        min-width: 150px;
    }

    .storePopup .popupTitle {
        border-top-left-radius: 4px;
        border-top-right-radius: 4px;
        padding: 8px;
        height: 30px;
        background-color: #007faa;
        color: white;
        font-weight: bold;
    }

    .storePopup .popupSubTitle {
        font-size: 10px;
        line-height: 12px;
    }

    .storePopup .popupContent {
        font-size: 11px;
        line-height: 18px;
        padding: 8px;
    }

    .storePopup img {
        vertical-align:middle;
        height: 12px;
        margin-right: 5px;
    }

    /* Adjust the layout of the page when the screen width is less than 700 pixels. */
    @media screen and (max-width: 700px) {
        .searchPanel {
            width: 100vw;
        }

        #listPanel {
            top: 385px;
            width: 100%;
            height: calc(100vh - 385px);
        }

        #myMap {
            width: 100vw;
            height: 250px;
            top: 135px;
            left: 0px;
        }

        #myLocationBtn {
            top: 220px;
        }
    }

    .mapCenterIcon {
        display: block;
        width: 10px;
        height: 10px;
        border-radius: 50%;
        background: orange;
        border: 2px solid white;
        cursor: pointer;
        box-shadow: 0 0 0 rgba(0, 204, 255, 0.4);
        animation: pulse 3s infinite;
    }

    @keyframes pulse {
        0% {
            box-shadow: 0 0 0 0 rgba(0, 204, 255, 0.4);
        }

        70% {
            box-shadow: 0 0 0 50px rgba(0, 204, 255, 0);
        }

        100% {
            box-shadow: 0 0 0 0 rgba(0, 204, 255, 0);
        }
    }
   ```

Çalıştırırsanız uygulama artık, üst bilgi, arama kutusuna ve Ara düğmesine görürsünüz ancak henüz yüklenmedi çünkü harita görünür değildir. Bir arama yapmak çalışırsanız, hiçbir şey olmaz. Depolama Bulucu tüm işlevine erişmek için sonraki bölümde açıklanan JavaScript mantığının ayarlamanız gerekir.

## <a name="wire-the-application-with-javascript"></a>Kablo JavaScript ile uygulama

Bu noktada, her şeyi kullanıcı arabiriminin ayarlanır. Şimdi, yüklemek ve verileri ayrıştırmak için JavaScript ekleyin ve ardından harita üzerinde veri işlemek ihtiyacımız var. Başlamak için açık *index.js* ve kodu, aşağıdaki adımlarda açıklandığı gibi ekleyin.

1. Ayarları güncelleştirme daha kolay hale getirmek için genel seçenekleri ekleyin. Ayrıca, eşleme, bir açılır pencere, bir veri kaynağı, bir simge katman, merkezi bir arama alanını ve Azure haritalar arama hizmeti istemcisi örneği görüntüleyen bir HTML işaret değişkenleri tanımlayın.

    ```JavaScript
    //The maximum zoom level to cluster data point data on the map.
    var maxClusterZoomLevel = 11;

    //The URL to the store location data.
    var storeLocationDataUrl = 'data/ContosoCoffee.txt';

    //The URL to the icon image.
    var iconImageUrl = 'images/CoffeeIcon.png';
    var map, popup, datasource, iconLayer, centerMarker, searchURL;
    ```

1. Kodu *index.js*. Aşağıdaki kod Haritası'nı başlatır, ekler bir [olay dinleyicisi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) sayfa işlemi tamamlanana kadar bekler yüklemeyi bağlayan eşleme yüklenmesini izlemek için olayları ve arama ve konum My düğmesi güçlendirir.

   Kullanıcının sorgu karşı bir belirsiz arama, kullanıcı, arama düğmesini seçtiğinde veya kullanıcı arama kutusuna bir konuma girdikten sonra ENTER'a bastığında başlatılır. Geçirmek için Ülke ISO 2 değerlerinin bir dizideki `countrySet` Bu ülkeler arama sonuçlarını sınırlamak için seçeneği. Aranacak ülkeleri sınırlama, döndürülen sonuçları doğruluğunu artırmak yardımcı olur. 
  
   Arama tamamlandığında ilk sonucu alın ve bu alan üzerinde eşleme kamera ayarlayın. Kullanıcının, My konumu düğmeyi seçtiğinde, kullanıcının konumunu alma ve eşleme konumları merkezi tarayıcı yerleşik HTML5 coğrafi konum API kullanın.  

   > [!Tip]
   > Açılan pencereler kullandığınızda, tek bir oluşturmak en iyi `Popup` örneği ve onun içeriğini ve konumunu güncelleştirerek örneği yeniden. İçin her `Popup`örneği birden çok DOM öğeleri sayfasına eklenir, kodunuza ekleyin. Daha fazla DOM öğeleri bir sayfaya izlemek için tarayıcıyı sahip daha fazla şey var. Çok fazla öğe varsa, tarayıcı yavaş olabilir.

    ```JavaScript
    function initialize() {
        //Initialize a map instance.
        map = new atlas.Map('myMap', {
            center: [-90, 40],
            zoom: 2,

            //Add your Azure Maps subscription key to the map SDK.
            authOptions: {
                authType: 'subscriptionKey',
                subscriptionKey: '<Your Azure Maps Key>'
            }
        });

        //Create a pop-up window, but leave it closed so we can update it and display it later.
        popup = new atlas.Popup();

        //Use SubscriptionKeyCredential with a subscription key
        const subscriptionKeyCredential = new atlas.service.SubscriptionKeyCredential(atlas.getSubscriptionKey());

        //Use subscriptionKeyCredential to create a pipeline
        const pipeline = atlas.service.MapsURL.newPipeline(subscriptionKeyCredential, {
            retryOptions: { maxTries: 4 }, // Retry options
        });

        //Create an instance of the SearchURL client.
        searchURL = new atlas.service.SearchURL(pipeline);

        //If the user selects the search button, geocode the value the user passed in.
        document.getElementById('searchBtn').onclick = performSearch;

        //If the user presses Enter in the search box, perform a search.
        document.getElementById('searchTbx').onkeyup = function(e) {
            if (e.keyCode === 13) {
                performSearch();
            }
        };

        //If the user selects the My Location button, use the Geolocation API to get the user's location. Center and zoom the map on that location.
        document.getElementById('myLocationBtn').onclick = setMapToUserLocation;

        //Wait until the map resources are ready.
        map.events.add('ready', function() {

            //Add your post-map load functionality.

        });
    }

    //Create an array of country ISO 2 values to limit searches to. 
    var countrySet = ['US', 'CA', 'GB', 'FR','DE','IT','ES','NL','DK'];

    function performSearch() {
        var query = document.getElementById('searchTbx').value;

        //Perform a fuzzy search on the users query.
        searchURL.searchFuzzy(atlas.service.Aborter.timeout(3000), query, {
            //Pass in the array of country ISO2 for which we want to limit the search to.
            countrySet: countrySet
        }).then(results => {
            //Parse the response into GeoJSON so that the map can understand.
            var data = results.geojson.getFeatures();

            if (data.features.length > 0) {
                //Set the camera to the bounds of the results.
                map.setCamera({
                    bounds: data.features[0].bbox,
                    padding: 40
                });
            } else {
                document.getElementById('listPanel').innerHTML = '<div class="statusMessage">Unable to find the location you searched for.</div>';
            }
        });
    }

    function setMapToUserLocation() {
        //Request the user's location.
        navigator.geolocation.getCurrentPosition(function(position) {
            //Convert the Geolocation API position to a longitude and latitude position value that the map can interpret and center the map over it.
            map.setCamera({
                center: [position.coords.longitude, position.coords.latitude],
                zoom: maxClusterZoomLevel + 1
            });
        }, function(error) {
            //If an error occurs when the API tries to access the user's position information, display an error message.
            switch (error.code) {
                case error.PERMISSION_DENIED:
                    alert('User denied the request for geolocation.');
                    break;
                case error.POSITION_UNAVAILABLE:
                    alert('Position information is unavailable.');
                    break;
                case error.TIMEOUT:
                    alert('The request to get user position timed out.');
                    break;
                case error.UNKNOWN_ERROR:
                    alert('An unknown error occurred.');
                    break;
            }
        });
    }

    //Initialize the application when the page is loaded.
    window.onload = initialize;
    ```

1. Haritanın içinde `ready` olay dinleyicisi, yakınlaştırma denetimi ve arama alanının görüntülemek için bir HTML işaret ekleyin.

    ```JavaScript
    //Add a zoom control to the map.
    map.controls.add(new atlas.control.ZoomControl(), {
        position: 'top-right'
    });

    //Add an HTML marker to the map to indicate the center to use for searching.
    centerMarker = new atlas.HtmlMarker({
        htmlContent: '<div class="mapCenterIcon"></div>',
        position: map.getCamera().center
    });

    map.markers.add(centerMarker);
    ```

1. Haritanın içinde `ready` olay dinleyicisi, bir veri kaynağı ekleyin. Sonra veri kümesi yüklenemedi ve bir çağrı yapın. Veri kaynağında kümeleme etkinleştirin. Verileri kaynak grupları noktaları bir kümede birlikte çakışan kümeleme. Kümeler ayrı kullanıcı olarak tek tek noktaları halinde yakınlaştırır. Bu deneyimi daha akıcı bir kullanıcının yaptığı ve performansı artırır.

    ```JavaScript
    //Create a data source, add it to the map, and then enable clustering.
    datasource = new atlas.source.DataSource(null, {
        cluster: true,
        clusterMaxZoom: maxClusterZoomLevel - 1
    });

    map.sources.add(datasource);

    //Load all the store data now that the data source is defined.  
    loadStoreData();
    ```

1. Haritanın kümesinde yükledikten sonra `ready` katmanları verileri işlemek için bir dizi olay dinleyicisi tanımlayın. Kabarcık katman, kümelenmiş veri noktaları işlemek için kullanılır. Bir sembol katman, her kümede Kabarcık katmanının noktalarının sayısını işlemek için kullanılır. İkinci bir sembol katmanı harita üzerinde tek tek konumları için özel bir simge oluşturur.

   Ekleme `mouseover` ve `mouseout` kullanıcı bir küme veya harita üzerinde simge üzerine geldiğinde fare imlecini değiştirmek için Kabarcık ve simge katmanlarını olayları. Ekleme bir `click` küme Kabarcık katmana olay. Bu `click` olay iki düzeyi haritada yakınlaştırıldığını ve kullanıcı herhangi bir küme seçtiğinde harita üzerinde bir küme ortalar. Ekleme bir `click` simgesi katmana olay. Bu `click` olay, bir kullanıcı bir tek konum simgeyi seçtiğinde bir kahve Dükkanı ayrıntılarını gösteren bir açılır pencere görüntüler. Haritaya harita Taşıma tamamlandığında izlemek için bir olay ekleyin. Bu olayı tetikler, liste bölmesi öğeleri güncelleştirin.  

    ```JavaScript
    //Create a bubble layer to render clustered data points.
    var clusterBubbleLayer = new atlas.layer.BubbleLayer(datasource, null, {
        radius: 12,
        color: '#007faa',
        strokeColor: 'white',
        strokeWidth: 2,
        filter: ['has', 'point_count'] //Only render data points that have a point_count property; clusters have this property.
    });

    //Create a symbol layer to render the count of locations in a cluster.
    var clusterLabelLayer = new atlas.layer.SymbolLayer(datasource, null, {
        iconOptions: {
            image: 'none' //Hide the icon image.
        },

        textOptions: {
            textField: '{point_count_abbreviated}',
            size: 12,
            font: ['StandardFont-Bold'],
            offset: [0, 0.4],
            color: 'white'
        }
    });

    map.layers.add([clusterBubbleLayer, clusterLabelLayer]);

    //Load a custom image icon into the map resources.
    map.imageSprite.add('myCustomIcon', iconImageUrl).then(function() {

    //Create a layer to render a coffee cup symbol above each bubble for an individual location.
    iconLayer = new atlas.layer.SymbolLayer(datasource, null, {
        iconOptions: {
            //Pass in the ID of the custom icon that was loaded into the map resources.
            image: 'myCustomIcon',

            //Optionally, scale the size of the icon.
            font: ['SegoeUi-Bold'],

            //Anchor the center of the icon image to the coordinate.
            anchor: 'center',

            //Allow the icons to overlap.
            allowOverlap: true
        },

        filter: ['!', ['has', 'point_count']] //Filter out clustered points from this layer.
    });

    map.layers.add(iconLayer);

    //When the mouse is over the cluster and icon layers, change the cursor to a pointer.
    map.events.add('mouseover', [clusterBubbleLayer, iconLayer], function() {
        map.getCanvasContainer().style.cursor = 'pointer';
    });

    //When the mouse leaves the item on the cluster and icon layers, change the cursor back to the default (grab).
    map.events.add('mouseout', [clusterBubbleLayer, iconLayer], function() {
        map.getCanvasContainer().style.cursor = 'grab';
    });

    //Add a click event to the cluster layer. When the user selects a cluster, zoom into it by two levels.  
    map.events.add('click', clusterBubbleLayer, function(e) {
        map.setCamera({
            center: e.position,
            zoom: map.getCamera().zoom + 2
        });
    });

    //Add a click event to the icon layer and show the shape that was selected.
    map.events.add('click', iconLayer, function(e) {
        showPopup(e.shapes[0]);
    });

    //Add an event to monitor when the map is finished rendering the map after it has moved.
    map.events.add('render', function() {
        //Update the data in the list.
        updateListItems();
    });
    ```

1. Kahve Dükkanı dataset yüklendiğinde önce indirilmelidir. Ardından, metin dosyasının satıra bölmeniz gerekir. İlk satırı üst bilgi bilgileri içerir. Kod takibini kolaylaştırmak için biz üstbilgi sonra her bir özellik hücre dizinini aramak için kullanabiliriz nesneye ayrıştırılamıyor. Sonra ilk satırda kalan satırlar döngü ve noktası özelliği oluşturun. Veri kaynağına noktası özelliğini ekleyin. Son olarak, liste bölmesi güncelleştirin.

    ```JavaScript
    function loadStoreData() {

    //Download the store location data.
    fetch(storeLocationDataUrl)
        .then(response => response.text())
        .then(function(text) {

            //Parse the tab-delimited file data into GeoJSON features.
            var features = [];

            //Split the lines of the file.
            var lines = text.split('\n');

            //Grab the header row.
            var row = lines[0].split('\t');

            //Parse the header row and index each column to make the code for parsing each row easier to follow.
            var header = {};
            var numColumns = row.length;
            for (var i = 0; i < row.length; i++) {
                header[row[i]] = i;
            }

            //Skip the header row and then parse each row into a GeoJSON feature.
            for (var i = 1; i < lines.length; i++) {
                row = lines[i].split('\t');

                //Ensure that the row has the correct number of columns.
                if (row.length >= numColumns) {

                    features.push(new atlas.data.Feature(new atlas.data.Point([parseFloat(row[header['Longitude']]), parseFloat(row[header['Latitude']])]), {
                        AddressLine: row[header['AddressLine']],
                        City: row[header['City']],
                        Municipality: row[header['Municipality']],
                        AdminDivision: row[header['AdminDivision']],
                        Country: row[header['Country']],
                        PostCode: row[header['PostCode']],
                        Phone: row[header['Phone']],
                        StoreType: row[header['StoreType']],
                        IsWiFiHotSpot: (row[header['IsWiFiHotSpot']].toLowerCase() === 'true') ? true : false,
                        IsWheelchairAccessible: (row[header['IsWheelchairAccessible']].toLowerCase() === 'true') ? true : false,
                        Opens: parseInt(row[header['Opens']]),
                        Closes: parseInt(row[header['Closes']])
                    }));
                }
            }

            //Add the features to the data source.
            datasource.add(new atlas.data.FeatureCollection(features));

            //Initially, update the list items.
            updateListItems();
        });
    }
    ```

1. Liste bölmesi güncelleştirildiğinde geçerli harita görünümü tüm noktası özelliklerinde Merkezi'nden harita uzaklık hesaplanır. Özellikleri ardından uzaklık göre sıralanır. Her konum listesi Masası'nda görüntülenecek HTML oluşturulur.

    ```JavaScript
    var listItemTemplate = '<div class="listItem" onclick="itemSelected(\'{id}\')"><div class="listItem-title">{title}</div>{city}<br />Open until {closes}<br />{distance} miles away</div>';

    function updateListItems() {
        //Hide the center marker.
        centerMarker.setOptions({
            visible: false
        });

        //Get the current camera and view information for the map.
        var camera = map.getCamera();
        var listPanel = document.getElementById('listPanel');

        //Get all the shapes that have been rendered in the bubble layer.
        var data = map.layers.getRenderedShapes(map.getCamera().bounds, [iconLayer]);

        data.forEach(function(shape) {
            if (shape instanceof atlas.Shape) {
                //Calculate the distance from the center of the map to each shape, and then store the data in a distance property.  
                shape.distance = atlas.math.getDistanceTo(camera.center, shape.getCoordinates(), 'miles');
            }
        });

        //Sort the data by distance.
        data.sort(function(x, y) {
            return x.distance - y.distance;
        });

        //Check to see whether the user is zoomed out a substantial distance. If they are, tell the user to zoom in and to perform a search or select the My Location button.
        if (camera.zoom < maxClusterZoomLevel) {
            //Close the pop-up window; clusters might be displayed on the map.  
            popup.close(); 
            listPanel.innerHTML = '<div class="statusMessage">Search for a location, zoom the map, or select the My Location button to see individual locations.</div>';
        } else {
            //Update the location of the centerMarker property.
            centerMarker.setOptions({
                position: camera.center,
                visible: true
            });

            //List the ten closest locations in the side panel.
            var html = [], properties;

            /*
            Generating HTML for each item that looks like this:
            <div class="listItem" onclick="itemSelected('id')">
                <div class="listItem-title">1 Microsoft Way</div>
                Redmond, WA 98052<br />
                Open until 9:00 PM<br />
                0.7 miles away
            </div>
            */

            data.forEach(function(shape) {
                properties = shape.getProperties();
                html.push('<div class="listItem" onclick="itemSelected(\'', shape.getId(), '\')"><div class="listItem-title">',
                properties['AddressLine'],
                '</div>',
                //Get a formatted addressLine2 value that consists of City, Municipality, AdminDivision, and PostCode.
                getAddressLine2(properties),
                '<br />',

                //Convert the closing time to a format that is easier to read.
                getOpenTillTime(properties),
                '<br />',

                //Route the distance to two decimal places.  
                (Math.round(shape.distance * 100) / 100),
                ' miles away</div>');
            });

            listPanel.innerHTML = html.join('');

            //Scroll to the top of the list panel in case the user has scrolled down.
            listPanel.scrollTop = 0;
        }
    }

    //This converts a time that's in a 24-hour format to an AM/PM time or noon/midnight string.
    function getOpenTillTime(properties) {
        var time = properties['Closes'];
        var t = time / 100;
        var sTime;

        if (time === 1200) {
            sTime = 'noon';
        } else if (time === 0 || time === 2400) {
            sTime = 'midnight';
        } else {
            sTime = Math.round(t) + ':';

            //Get the minutes.
            t = (t - Math.round(t)) * 100;

            if (t === 0) {
                sTime += '00';
            } else if (t < 10) {
                sTime += '0' + t;
            } else {
                sTime += Math.round(t);
            }

            if (time < 1200) {
                sTime += ' AM';
            } else {
                sTime += ' PM';
            }
        }

        return 'Open until ' + sTime;
    }

    //Create an addressLine2 string that contains City, Municipality, AdminDivision, and PostCode.
    function getAddressLine2(properties) {
        var html = [properties['City']];

        if (properties['Municipality']) {
            html.push(', ', properties['Municipality']);
        }

        if (properties['AdminDivision']) {
            html.push(', ', properties['AdminDivision']);
        }

        if (properties['PostCode']) {
            html.push(' ', properties['PostCode']);
        }

        return html.join('');
    }
    ```

1. Kullanıcı listesi panelindeki bir öğe seçtiğinde, öğe için ilgili şekli veri kaynağından elde edilir. Bir açılır pencere şeklinde depolanan özellik bilgileri temel alan oluşturulur. Harita şeklin ortalanır. Harita pikselden 700'den az ise, açılır pencere görünür olacak şekilde harita görünümünü uzaklığı.

    ```JavaScript
    //When a user selects a result in the side panel, look up the shape by its ID value and display the pop-up window.
    function itemSelected(id) {
        //Get the shape from the data source by using its ID.  
        var shape = datasource.getShapeById(id);
        showPopup(shape);

        //Center the map over the shape on the map.
        var center = shape.getCoordinates();
        var offset;

        //If the map is less than 700 pixels wide, then the layout is set for small screens.
        if (map.getCanvas().width < 700) {
            //When the map is small, offset the center of the map relative to the shape so that there is room for the popup to appear.
            offset = [0, -80];
        }

        map.setCamera({
            center: center,
            centerOffset: offset
        });
    }

    function showPopup(shape) {
        var properties = shape.getProperties();

        /* Generating HTML for the pop-up window that looks like this:

            <div class="storePopup">
                <div class="popupTitle">
                    3159 Tongass Avenue
                    <div class="popupSubTitle">Ketchikan, AK 99901</div>
                </div>
                <div class="popupContent">
                    Open until 22:00 PM<br/>
                    <img title="Phone Icon" src="images/PhoneIcon.png">
                    <a href="tel:1-800-XXX-XXXX">1-800-XXX-XXXX</a>
                    <br>Amenities:
                    <img title="Wi-Fi Hotspot" src="images/WiFiIcon.png">
                    <img title="Wheelchair Accessible" src="images/WheelChair-small.png">
                </div>
            </div>
        */

        var html = ['<div class="storePopup">'];
        html.push('<div class="popupTitle">',
            properties['AddressLine'],
            '<div class="popupSubTitle">',
            getAddressLine2(properties),
            '</div></div><div class="popupContent">',

            //Convert the closing time to a format that's easier to read.
            getOpenTillTime(properties),

            //Route the distance to two decimal places.  
            '<br/>', (Math.round(shape.distance * 100) / 100),
            ' miles away',
            '<br /><img src="images/PhoneIcon.png" title="Phone Icon"/><a href="tel:',
            properties['Phone'],
            '">',  
            properties['Phone'],
            '</a>'
        );

        if (properties['IsWiFiHotSpot'] || properties['IsWheelchairAccessible']) {
            html.push('<br/>Amenities: ');

            if (properties['IsWiFiHotSpot']) {
                html.push('<img src="images/WiFiIcon.png" title="Wi-Fi Hotspot"/>')
            }

            if (properties['IsWheelchairAccessible']) {
                html.push('<img src="images/WheelChair-small.png" title="Wheelchair Accessible"/>')
            }
        }

        html.push('</div></div>');

        //Update the content and position of the pop-up window for the specified shape information.
        popup.setOptions({

            //Create a table from the properties in the feature.
            content:  html.join(''),
            position: shape.getCoordinates()
        });

        //Open the pop-up window.
        popup.open(map);
    }
    ```

Artık, tam olarak işlevsel deposu Bulucunun var. Bir web tarayıcısında açın *index.html* dosya deposu Bulucu için. Kümeler harita üzerinde göründüğünde, bir konum için arama kutusunu kullanarak, My konumu düğmesini seçerek, bir küme seçerek veya bireysel konumları görmek için harita üzerinde yakınlaştırma tarafından arama yapabilirsiniz.

İlk kez bir kullanıcı konum My düğmesini seçtiğinde, tarayıcı kullanıcının konuma erişmek için izin isteyen bir güvenlik uyarısı görüntüler. Kullanıcı konumlarını paylaşmak uygunsa, kullanıcının bulunduğu konum eşlemesi yakınlaştırır ve yakında kahve dükkanları gösterilir. 

<br/>
<center>

![Kullanıcının ekran görüntüsü tarayıcı istek kullanıcının konuma erişmek için](./media/tutorial-create-store-locator/GeolocationApiWarning.png)</center>

Yakınlaştırdığınızda yeterli kahve Dükkanı konumlarını içeren bir alanda kapatmak için ayrı ayrı konumlara kümeler. Harita üzerinde simgelerden birini seçin veya bu konuma bilgilerini gösteren bir açılır pencere görmek için yan panelinde bir öğe seçin.

<br/>
<center>

![Tamamlanan depolama Bulucu ekran görüntüsü](./media/tutorial-create-store-locator/FinishedSimpleStoreLocator.png)</center>

700'den az piksel genişliğinde için tarayıcı penceresini yeniden boyutlandırdığınızda veya mobil bir cihazda uygulamayı açmak, daha iyi olmasını düzen değişiklikleri daha küçük ekranlar için uygundur. 

<br/>
<center>

![Küçük ekranlı sürüm deposu Bulucu ekran görüntüsü](./media/tutorial-create-store-locator/FinishedSimpleStoreLocatorSmallScreen.png)</center>

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure haritalar'ı kullanarak bir temel deposu Bulucu oluşturma konusunda bilgi edinin. Bu öğreticide oluşturduğunuz depolama Bulucu ihtiyacınız olan tüm işlevlere sahip olabilir. Depolama Bulucu için özellikleri ekleyebilir veya daha fazla Gelişmiş özelliği için daha özel bir kullanıcı deneyimi kullanın: 

> [!div class="checklist"]
> * Etkinleştirme [önerileri yazarken](https://azuremapscodesamples.azurewebsites.net/?sample=Search%20Autosuggest%20and%20JQuery%20UI) arama kutusuna.  
> * Ekleme [desteklemek için birden çok dil](https://azuremapscodesamples.azurewebsites.net/?sample=Map%20Localization). 
> * Kullanıcıya izin [filtre bir yol boyunca konumları](https://azuremapscodesamples.azurewebsites.net/?sample=Filter%20Data%20Along%20Route). 
> * Ekleme yeteneği [filtreleri ayarlayın](https://azuremapscodesamples.azurewebsites.net/?sample=Filter%20Symbols%20by%20Property). 
> * Bir sorgu dizesi kullanarak bir ilk arama değeri belirtmek için destek eklendi. Bu seçenek, depolama Konumlandırıcı, eklediğinizde, kullanıcıların yer işareti ekleyebilirsiniz ve aramalar paylaşın. Ayrıca, başka bir sayfadan aramaları bu sayfaya geçirilecek için kolay bir yöntemini sağlar.  
> * Depolama Bulucu olarak dağıtma bir [Azure App Service Web uygulaması](https://docs.microsoft.com/azure/app-service/app-service-web-get-started-html). 
> * Bir veritabanı ve yakında konumları için arama verilerinizi Store. Daha fazla bilgi için bkz. [SQL Server uzamsal veri türleri özeti](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-types-overview?view=sql-server-2017) ve [en yakın komşu için uzamsal veri sorgulama](https://docs.microsoft.com/sql/relational-databases/spatial/query-spatial-data-for-nearest-neighbor?view=sql-server-2017).

Bu öğreticiye ait kod örneğine şuradan erişebilirsiniz:

> [Azure haritalar'ı kullanarak bir depolama Bulucu](https://github.com/Azure-Samples/AzureMapsCodeSamples/tree/master/AzureMapsCodeSamples/Tutorials/Simple%20Store%20Locator)

[Burada Canlı örnek bakın](https://azuremapscodesamples.azurewebsites.net/index.html?sample=Simple%20Store%20Locator)

Azure Haritalar'ın kapsamı ve özellikleri hakkında daha fazla bilgi edinmek için:

> [!div class="nextstepaction"]
> [Yakınlaştırma düzeyleri ve kutucuk kılavuzu](zoom-levels-and-tile-grid.md)

Daha fazla kod örneği ve etkileşimli bir kodlama deneyimi için:

> [!div class="nextstepaction"]
> [Harita Denetimi'ni kullanma](how-to-use-map-control.md)

> [!div class="nextstepaction"]
> [Veri odaklı stili ifadeleri kullanma](data-driven-style-expressions-web-sdk.md)