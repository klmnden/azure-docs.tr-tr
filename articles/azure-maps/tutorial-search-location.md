---
title: Azure Haritalar ile arama | Microsoft Docs
description: Azure Haritalar’ı kullanarak yakınlardaki ilgi çekici noktayı arama
author: walsehgal
ms.author: v-musehg
ms.date: 12/14/2018
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 1396260ef703ce22f8e0309bd2c8df691d0af86e
ms.sourcegitcommit: 818d3e89821d101406c3fe68e0e6efa8907072e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2019
ms.locfileid: "54120470"
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
    * Yeni hesabınıza verilen *Ad*.
    * Bu hesap için kullanmak istediğiniz *Abonelik*.
    * Bu hesap için *Kaynak grubu* adı. Kaynak grubu için *Yeni oluştur* veya *Mevcut olanı kullan* seçeneğini belirleyebilirsiniz.
    * *Kaynak grubu konumu*nu seçin.
    * *Lisans*’ı ve *Gizlilik Bildirimi*’ni okuyun ve onay kutusunu işaretleyerek koşulları kabul edin.
    * **Oluştur** düğmesine tıklayın.

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
   <html>
   <head>
      <title>Map Search</title>
      <meta charset="utf-8" />
      <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />

      <!-- Add references to the Azure Maps Map control JavaScript and CSS files. -->
      <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/css/atlas.min.css?api-version=1" type="text/css" />
      <script src="https://atlas.microsoft.com/sdk/js/atlas.min.js?api-version=1"></script>

      <!-- Add a reference to the Azure Maps Services Module JavaScript file. -->
      <script src="https://atlas.microsoft.com/sdk/js/atlas-service.js?api-version=1"></script>

      <script>      
         var map, datasource, client, popup;

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

   HTML üst bilgisinin Azure Harita Denetimi kitaplığı tarafından barındırılan CSS ve JavaScript kaynak dosyalarını içerdiğine dikkat edin. Sayfanın gövdesinde bulunan ve sayfa yüklendiğinde `GetMap` işlevini çağıracak olan `onload` olayına dikkat edin. Bu işlev, Azure Haritalar API’lerine erişime yönelik satır içi JavaScript kodunu içerir.

3. HTML dosyasının `GetMap` işlevine aşağıdaki JavaScript kodunu ekleyin. **\<Azure Haritalar Anahtarınız\>** dizesini, Haritalar hesabınızdan kopyaladığınız birincil anahtarla değiştirin.

   ```JavaScript
   //Add your Azure Maps subscription key to the map SDK. Get an Azure Maps key at https://azure.com/maps
   atlas.setSubscriptionKey('<Your Azure Maps Key>');

   //Initialize a map instance.
   map = new atlas.Map('myMap');
   ```

   Bu segment, Azure Haritalar hesap anahtarınız için Harita Denetimi API’sini başlatır. **atlas**, API ve ilgili görsel bileşenleri içeren ad alanıdır. **atlas.Map**, görsel ve etkileşimli bir web haritası için gerekli denetimi sağlar. 

4. Değişikliklerinizi dosyaya kaydedin ve HTML sayfasını bir tarayıcıda açın. Bu, **atlas.map** komutunu çağırıp hesap anahtarınızı kullanarak oluşturabileceğiniz en temel haritadır.

   ![Haritayı görüntüleme](./media/tutorial-search-location/basic-map.png)

5. `GetMap` işlevinde haritanın başlatıldığı bölümün altına aşağıdaki JavaScript kodunu ekleyin. 

   ```JavaScript
   //Wait until the map resources have fully loaded.
   map.events.add('load', function () {

      //Create a data source and add it to the map.
      datasource = new atlas.source.DataSource();
      map.sources.add(datasource);

      //Add a layer for rendering point data.
      var resultLayer = new atlas.layer.SymbolLayer(datasource, null, {
         iconOptions: {
            iconImage: 'pin-round-darkblue',
            anchor: 'center',
            allowOverlap: true
         }
      });
      map.layers.add(resultLayer);

      //Create a popup but leave it closed so we can update it and display it later.
      popup = new atlas.Popup();

      //Add a mouse over event to the result layer and display a popup when this event fires.
      map.events.add('mouseover', resultLayer, showPopup);
   });
   ```

   Haritaya, harita kaynakları tamamen yüklendikten sonra harekete geçirilecek bir yükleme olayı eklenir. Harita yükleme olayı işleyicisinde sonuç verilerin depolanacağı bir veri kaynağı oluşturulur. Bir simge katmanı oluşturulur ve veri kaynağına eklenir. Bu katman, veri kaynağındaki sonuç verilerin nasıl işleneceğini belirtir. Bu örnekte sonuç koordinatlarının üzerinde ortalanmış olan ve diğer simgelerin çakışmasına izin verilen koyu mavi yuvarlak raptiye simgesi şeklindedir. 

<a id="usesearch"></a>

## <a name="add-search-capabilities"></a>Arama özellikleri ekleme

Bu bölümde, Haritalar Arama API’sini kullanarak haritanızda ilgi çekici bir noktayı nasıl bulabileceğiniz gösterilmektedir. Bu, geliştiricilerin adres, ilgi çekici nokta ve diğer coğrafi bilgileri araması için tasarlanmış bir RESTful API’dir. Arama hizmeti, belirtilen bir adrese enlem ve boylam bilgileri atar. Aşağıda açıklanan **Hizmet Modülü**, Haritalar Arama API'si ile konum bulmaya yönelik aramalarda kullanılabilir.

### <a name="service-module"></a>Hizmet Modülü

1. Harita yükleme olayı işleyicisine aşağıdaki Javascript kodunu ekleyerek istemci hizmetini başlatın.

    ```JavaScript
    //Create an instance of the services client.
     client = new atlas.service.Client(atlas.getSubscriptionKey());
    ```

2. Ardından arama sorgusunu oluşturmak için aşağıdaki betik bloğunu ekleyin. Bu, Arama Hizmetinin temel arama API'si olan Belirsiz Arama Hizmetini kullanır. Belirsiz Arama Hizmeti adres, yer ve ilgi çekici nokta (POI) gibi çoğu belirsiz girişi işler. Bu kod, belirtilen yarıçap içinde olup yakında bulunan Benzin İstasyonlarını arar. Yanıt GeoJSON biçiminde ayrıştırılıp veri kaynağına eklenir ve bunun sonucunda veriler otomatik olarak simge katmanı aracılığıyla harita üzerinde işlenir. Betiğin son bölümü haritanın [setCamera](/javascript/api/azure-maps-control/atlas.map#setcamera-cameraoptions---cameraboundsoptions---animationoptions-) özelliğini kullanarak sonuçların sınırlayıcı kutusuna göre harita kamera görünümünü ayarlar. Sınırlayıcı kutu koordinatlara göre hesaplandığından simgelerin piksel boyutlarını telafi eden bir iç boşluk eklenir. 
 
   ```JavaScript
   //Execute a POI search query then add the results to the map.
    client.search.getSearchPOI('gasoline station', {
        lat: 47.6292,
        lon: -122.2337,
        radius: 100000
    }).then(response => {
        //Parse the response into GeoJSON so that the map can understand.
        var geojsonResponse = new atlas.service.geojson.GeoJsonSearchResponse(response);
        var results = geojsonResponse.getGeoJsonResults();

        //Add the results to the data source so they can be rendered. 
        datasource.add(results);

        // Set the camera bounds
        map.setCamera({
            bounds: results.bbox,
            padding: 50
        });
    });
   ```
 
3. **MapSearch.html** dosyasını kaydedin ve tarayıcınızı yenileyin. Şimdi haritanın Seattle’da ortalandığını ve bölgedeki benzin istasyonu konumlarının mavi raptiyelerle işaretlendiğini görmeniz gerekir.

   ![Arama sonuçlarıyla haritayı görüntüleme](./media/tutorial-search-location/pins-map.png)

4. Tarayıcınıza aşağıdaki HTTP İsteğini girerek, haritanın işlediği ham verileri görebilirsiniz. \<Azure Haritalar Anahtarınız\> değerini birincil anahtarınızla değiştirin.

   ```http
   https://atlas.microsoft.com/search/fuzzy/json?api-version=1.0&query=gasoline%20station&subscription-key=<Your Azure Maps Key>&lat=47.6292&lon=-122.2337&radius=100000
   ```

Bu noktada MapSearch sayfası, belirsiz arama sorgusundan döndürülen ilgi çekici noktaların konumlarını görüntüleyebilir. Şimdi bazı etkileşimli özellikler ve konumlar hakkında daha fazla bilgi ekleyelim.

## <a name="add-interactive-data"></a>Etkileşimli veri ekleme

Şu ana kadar oluşturduğumuz harita, arama sonuçları için yalnızca boylam/enlem verilerine bakar. Ancak Haritalar Arama hizmetinin döndürdüğü işlenmemiş JSON’a bakarsanız, bunun ad ve açık adres de dahil olmak üzere her bir benzin istasyonuyla ilgili ek bilgiler içerdiğini görürsünüz. Etkileşimli açılır kutularla haritada bu verilere yer verebilirsiniz.

1. Belirsiz arama hizmetini sorgulamak için harita yükleme olayı işleyicisinde kodun sonrasına aşağıdaki kod satırlarını ekleyin. Bu kod bir açılır pencere örneği oluşturur ve simge katmanına fare ile üzerine gelindiğinde gerçekleştirilecek bir olay ekler.

    ```JavaScript
   //Create a popup but leave it closed so we can update it and display it later.
    popup = new atlas.Popup();

    //Add a mouse over event to the result layer and display a popup when this event fires.
    map.events.add('mouseover', resultLayer, showPopup);
    ```
    
    **atlas.Popup** API’si, haritada gerekli konuma sabitlenmiş bir bilgi penceresi sağlar. 
      
2. *script* etiketinde `GetMap` işlevinin sonrasına aşağıdaki kodu ekleyerek fareyle üzerine gelinen sonuç bilgilerinin açılan pencerede görünmesini sağlayın. 

   ```JavaScript
   function showPopup(e) {
        //Get the properties and coordinates of the first shape that the event occurred on.
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

2. Dosyayı kaydedin ve tarayıcınızı yenileyin. Şimdi tarayıcıdaki haritada, herhangi bir arama raptiyesinin üzerine gelip beklediğinizde bilgi açılan pencereleri gösterilir.

    ![Azure Harita Denetimi ve Arama Hizmeti](./media/tutorial-search-location/popup-map.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Azure Maps hesabı oluşturma
> * Hesabınızın birincil anahtarını alma
> * Harita Denetimi API’sini kullanarak yeni web sayfası oluşturma
> * Arama Hizmeti’ni kullanarak yakınlardaki istenen konumları bulma

Bu öğreticiye ait kod örneğine şuradan erişebilirsiniz:

> [Azure Haritalar ile konum arama](https://github.com/Azure-Samples/AzureMapsCodeSamples/blob/master/AzureMapsCodeSamples/Tutorials/search.html)

[Burada canlı örneği inceleyin](https://azuremapscodesamples.azurewebsites.net/?sample=Search%20for%20points%20of%20interest)

Sonraki öğreticide, iki konum arasındaki bir yolun nasıl görüntüleneceği gösterilmektedir.

> [!div class="nextstepaction"]
> [Bir hedefe yönlendirme](./tutorial-route-location.md)
