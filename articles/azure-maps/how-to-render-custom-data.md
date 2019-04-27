---
title: Azure haritalar içinde bir hücresel harita üzerinde özel veri işlemek nasıl | Microsoft Docs
description: Azure haritalar içinde bir hücresel harita üzerinde özel veri işleme.
author: walsehgal
ms.author: v-musehg
ms.date: 02/12/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 46f08aaa33563f620e7a011620730249e903f7b7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60794647"
---
# <a name="render-custom-data-on-a-raster-map"></a>Hücresel harita üzerinde özel verisi işleme

Bu makalede nasıl kullanılacağını açıklar [statik Görüntü hizmeti](https://docs.microsoft.com/rest/api/maps/render/getmapimage) hücresel harita üzerinde yer paylaşımları izin vermek için görüntü oluşturma işlevselliğe sahip. Görüntü oluşturma özel Raptiyeler, etiketler ve geometri yer paylaşımları gibi ek veriler içeren bir ızgara kutucuk geri alma özelliğini içerir.

Özel Raptiyeler, etiketler ve geometri katmanları işlemek için Postman uygulamasını kullanabilirsiniz. Azure haritalar kullanabileceğiniz [veri hizmeti API'lerini](https://docs.microsoft.com/rest/api/maps/data) depolamak ve yer paylaşımları işlemek için.


## <a name="prerequisites"></a>Önkoşullar

### <a name="create-an-azure-maps-account"></a>Azure Haritalar hesabı oluşturma

Bu makalede yer alan yordamları tamamlamak için önce yapmanız [bir Azure haritalar hesabı oluştur](how-to-manage-account-keys.md) S1 fiyatlandırma katmanını içinde.

## <a name="render-pushpins-with-labels-and-a-custom-image"></a>Raptiyeler etiketleri ve özel bir görüntü ile işleme

> [!Note]
> Bu bölümdeki yordamda bir fiyatlandırma katmanı S0 veya S1 Azure haritalar hesabı gerektirir.

Azure haritalar S0 katmanı destekleyen tek bir örneği yalnızca hesap `pins` parametresi. Özel bir görüntü ile bir URL isteği belirtilen en fazla beş Raptiyeler işleme olanak tanır.

Etiketleri ve özel bir görüntü ile Raptiyeler işlemek için aşağıdaki adımları tamamlayın:

1. İstekleri depolanacağı bir koleksiyon oluşturun. Postman uygulamasında seçin **yeni**. İçinde **Yeni Oluştur** penceresinde **koleksiyon**. Koleksiyonu adlandırın ve seçin **Oluştur** düğmesi. 

2. İsteği oluşturmak için Seç **yeni** yeniden. İçinde **Yeni Oluştur** penceresinde **istek**. Girin bir **istek adı** Raptiyeler için isteği kaydedin ve ardından konum olarak önceki adımda oluşturduğunuz koleksiyonu seçin **Kaydet**.
    
    ![Postman içinde bir isteği oluştur](./media/tutorial-geofence/postman-new.png)

3. Oluşturucu sekmesinde GET HTTP yöntemini seçin ve bir GET isteği oluşturmak için aşağıdaki URL'yi girin.

    ```HTTP
    https://atlas.microsoft.com/map/static/png?subscription-key={subscription-key}&api-version=1.0&layer=basic&style=main&zoom=12&center=-73.98,%2040.77&pins=custom%7Cla15+50%7Cls12%7Clc003b61%7C%7C%27CentralPark%27-73.9657974+40.781971%7C%7Chttp%3A%2F%2Fazuremapscodesamples.azurewebsites.net%2FCommon%2Fimages%2Fpushpins%2Fylw-pushpin.png
    ```
    Elde edilen görüntü aşağıda verilmiştir:

    ![Bir etiketi içeren özel bir Raptiye](./media/how-to-render-custom-data/render-pins.png)


## <a name="get-data-from-azure-maps-data-storage"></a>Azure haritalar veri depolamadan veri alma

> [!Note]
> Bu bölümdeki yordamda bir fiyatlandırma katmanında S1 Azure haritalar hesabı gerektirir.

Kullanarak yolu ve PIN konum bilgileri edinebilirsiniz [verileri karşıya yükleme API](https://docs.microsoft.com/rest/api/maps/data/uploadpreview). Yol ve PIN verileri yüklemek için aşağıdaki adımları izleyin.

1. Postman uygulamasında, önceki bölümde oluşturulan koleksiyona yeni bir sekmede açmak. Oluşturucu sekmesinde POST HTTP yöntemini seçin ve bir POST isteğinde bulunmak için aşağıdaki URL'yi girin:

    ```HTTP
    https://atlas.microsoft.com/mapData/upload?subscription-key={subscription-key}&api-version=1.0&dataFormat=geojson
    ```

2. Üzerinde **Params** sekmesinde, POST isteği URL'sini kullanılan aşağıdaki anahtar/değer çiftleri girin. Değiştirin `subscription-key` Azure haritalar abonelik anahtarınız ile değeri.
    
    ![Postman'deki params anahtar/değer](./media/how-to-render-custom-data/postman-key-vals.png)

3. Üzerinde **gövdesi** sekmesinde ham giriş biçimi seçin ve JSON giriş biçimi açılır listeden seçin. Bu JSON verileri olarak karşıya yüklenecek sağlar:
    
    ```JSON
    {
      "type": "FeatureCollection",
      "features": [
        {
          "type": "Feature",
          "properties": {},
          "geometry": {
            "type": "Polygon",
            "coordinates": [
              [
                [
                  -73.98235,
                  40.76799
                ],
                [
                  -73.95785,
                  40.80044
                ],
                [
                  -73.94928,
                  40.7968
                ],
                [
                  -73.97317,
                  40.76437
                ],
                [
                  -73.98235,
                  40.76799
                ]
              ]
            ]
          }
        },
        {
          "type": "Feature",
          "properties": {},
          "geometry": {
            "type": "LineString",
            "coordinates": [
              [
                -73.97624731063843,
                40.76560773817073
              ],
              [
                -73.97914409637451,
                40.766826609362575
              ],
              [
                -73.98513078689575,
                40.7585866048861
              ]
            ]
          }
        }
      ]
    }
    ```

4. Seçin **Gönder** ve yanıt üst bilgisi gözden geçirin. Erişim veya gelecekte kullanım için verileri indirmek için kullanılan URI location üst bilgisini içerir. Ayrıca bir benzersiz içerdiği `udId` karşıya yüklenen veriler için.  

   ```HTTP
   https://atlas.microsoft.com/mapData/{udId}/status?api-version=1.0&subscription-key={Subscription-key}
   ```

5. Kullanım `udId` eşleme özelliklerini işlemek için verileri karşıya yükleme API'sinden alınan değer. Bunu yapmak için önceki bölümde oluşturulan koleksiyona yeni bir sekme açın. Oluşturucu sekmesinde GET HTTP yöntemini seçin ve bir GET isteği oluşturmak için bu URL'yi girin:

    ```HTTP
    https://atlas.microsoft.com/map/static/png?subscription-key={subscription-key}&api-version=1.0&layer=basic&style=main&zoom=12&center=-73.96682739257812%2C40.78119135317995&pins=default|la-35+50|ls12|lc003C62|co9B2F15||'Times Square'-73.98516297340393 40.758781646381024|'Central Park'-73.96682739257812 40.78119135317995&path=lc0000FF|fc0000FF|lw3|la0.80|fa0.30||udid-{udId}
    ```

6. Elde edilen görüntü aşağıda verilmiştir:

    ![Azure haritalar veri depolamadan veri alma](./media/how-to-render-custom-data/uploaded-path.png)

## <a name="render-a-polygon-with-color-and-opacity"></a>Bir çokgenin renk ve saydamlık ile işleme

> [!Note]
> Bu bölümdeki yordamda bir fiyatlandırma katmanında S1 Azure haritalar hesabı gerektirir.


Bir görünüm stili değiştiricilerini kullanarak değiştirebileceğiniz [yol parametresi](https://docs.microsoft.com/rest/api/maps/render/getmapimage#uri-parameters).

1. Postman uygulamasında daha önce oluşturduğunuz koleksiyonu yeni bir sekmede açmak. Oluşturucu sekmesinde GET HTTP yöntemini seçin ve renk ve saydamlık sahip bir çokgenin işlemek için bir GET isteği yapılandırmak için aşağıdaki URL'yi girin:
    
    ```HTTP
    https://atlas.microsoft.com/map/static/png?api-version=1.0&style=main&layer=basic&sku=S1&zoom=14&height=500&Width=500&center=-74.040701, 40.698666&path=lc0000FF|fc0000FF|lw3|la0.80|fa0.50||-74.03995513916016 40.70090237454063|-74.04082417488098 40.70028420372218|-74.04113531112671 40.70049568385827|-74.04298067092896 40.69899904076542|-74.04271245002747 40.69879568992435|-74.04367804527283 40.6980961582905|-74.04364585876465 40.698055487620714|-74.04368877410889 40.698022951066996|-74.04168248176573 40.696444909137|-74.03901100158691 40.69837271818651|-74.03824925422668 40.69837271818651|-74.03809905052185 40.69903971085914|-74.03771281242369 40.699340668780984|-74.03940796852112 40.70058515602143|-74.03948307037354 40.70052821920425|-74.03995513916016 40.70090237454063
    &subscription-key={subscription--key}
    ```

Elde edilen görüntü aşağıda verilmiştir:

![Donuk bir Çokgen işleme](./media/how-to-render-custom-data/opaque-polygon.png)


## <a name="render-a-circle-and-pushpins-with-custom-labels"></a>Bir daire ve Raptiyeler özel etiketler ile işleme

> [!Note]
> Bu bölümdeki yordamda bir fiyatlandırma katmanında S1 Azure haritalar hesabı gerektirir.


Raptiyeler ve bunların etiketleri büyütmek veya küçültmek kullanarak yapabileceğiniz `sc` ölçek stil değiştiricisi. Bu değiştirici, sıfırdan büyük bir değer alır. Bir 1 standart ölçek değeri. PIN 1'den büyük değerler daha büyük hale getirir ve değerleri 1'den küçük bunları daha küçük hale getirir. Stil değiştiriciler hakkında daha fazla bilgi için bkz. [statik Görüntü hizmeti yol parametreleri](https://docs.microsoft.com/rest/api/maps/render/getmapimage#uri-parameters).


Bir daire ve özel etiketlerle Raptiyeler işlemek için aşağıdaki adımları izleyin:

1. Postman uygulamasında daha önce oluşturduğunuz koleksiyonu yeni bir sekmede açmak. Oluşturucu sekmesinde GET HTTP yöntemini seçin ve bir GET isteği oluşturmak için bu URL'yi girin:

    ```HTTP
    https://atlas.microsoft.com/map/static/png?api-version=1.0&style=main&layer=basic&zoom=14&height=700&Width=700&center=-122.13230609893799,47.64599069048016&path=lcFF0000|lw2|la0.60|ra1000||-122.13230609893799 47.64599069048016&pins=default|la15+50|al0.66|lc003C62|co002D62||'Microsoft Corporate Headquarters'-122.14131832122801  47.64690503939462|'Microsoft Visitor Center'-122.136828 47.642224|'Microsoft Conference Center'-122.12552547454833 47.642940335653996|'Microsoft The Commons'-122.13687658309935  47.64452336193245&subscription-key={subscription-key}
    ```

Elde edilen görüntü aşağıda verilmiştir:

![Özel Raptiyeler daire işleme](./media/how-to-render-custom-data/circle-custom-pins.png)

## <a name="next-steps"></a>Sonraki adımlar


* Keşfedin [Azure haritalar alma harita görüntü API'si](https://docs.microsoft.com/rest/api/maps/render/getmapimage) belgeleri.
* Azure haritalar veri hizmeti hakkında daha fazla bilgi için bkz. [service belgeleri](https://docs.microsoft.com/rest/api/maps/data).

