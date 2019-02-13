---
title: Azure haritalar içinde hücresel harita üzerinde özel veri işlemek nasıl | Microsoft Docs
description: Azure haritalar içinde hücresel harita üzerinde özel veri işleme.
author: walsehgal
ms.author: v-musehg
ms.date: 02/12/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 9b3c0f7b1ff56cb269f6852be8fd2affeca8b8f1
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2019
ms.locfileid: "56143864"
---
# <a name="render-custom-data-on-raster-map"></a>Hücresel harita üzerinde özel verisi işleme

Bu makalede nasıl kullanılacağını gösterir [statik Görüntü hizmeti](https://docs.microsoft.com/rest/api/maps/render/getmapimage) hücresel harita üzerinde yer paylaşımları izin vermek için görüntü oluşturma işlevselliğe sahip. Görüntü oluşturma ızgara kutucuk özel Raptiyeler, etiketler ve geometri yer paylaşımları gibi ek veriler içeren geri alma özelliğini içerir. Özel Raptiyeler, etiketler ve geometri işlemek için postman uygulama kullanacağız. Postman uygulamasını açın, yeni | Yeni Oluştur, seçin ve bir koleksiyonu veya kaydetmek için klasör adını ve Kaydet'e tıklayın.

Azure haritalar kullanacağız [veri hizmeti API'lerini](https://docs.microsoft.com/rest/api/maps/data) depolamak ve yer paylaşımları işlemek için. 


## <a name="prerequisites"></a>Önkoşullar

### <a name="create-an-azure-maps-account"></a>Azure Haritalar hesabı oluşturma 

Bu kılavuzdaki adımları takip etmek için ilk görmek ihtiyacınız [hesapları ve anahtarları yönetme](how-to-manage-account-keys.md) oluşturup fiyatlandırma katmanı, hesap aboneliğiniz S1 ile yönetebilirsiniz.

## <a name="render-pushpins-with-labels-and-custom-image"></a>Raptiyeler etiketleri ve özel görüntü işleme

> [!Note]
> Bu örnek, bir Azure haritalar hesabı fiyatlandırma katmanı S0 veya S1 ile gerektirir. 

Azure haritalar hesabı S0 SKU yalnızca tek bir örneğini destekler `pins` özel görüntü URL'si istekte belirtilen en fazla 5 Raptiyeler işleme izin vererek parametresi.

Anında iletme işlemek için PIN etiketleri ve özel görüntü ile aşağıdaki adımları izleyin:

1. Postman uygulamasını açıp yeni tıklayın | Yeni oluştur ve istek'i seçin. İşleme Raptiyeler için bir istek adı girin, bir koleksiyon veya kaydetmek için klasör seçin ve Kaydet'e tıklayın.
    
    ![Bölge sınırlarının Postman kullanarak karşıya yükleme](./media/tutorial-geofence/postman-new.png)

2. Oluşturucu sekmesinde GET HTTP yöntemini seçin ve bir GET isteği oluşturmak için aşağıdaki URL'yi girin.

    ```HTTP
    https://atlas.microsoft.com/map/static/png?subscription-key={subscription-key}&api-version=1.0&layer=basic&style=main&zoom=12&center=-73.98,%2040.77&pins=custom%7Cla15+50%7Cls12%7Clc003b61%7C%7C%27CentralPark%27-73.9657974+40.781971%7C%7Chttp%3A%2F%2Fazuremapscodesamples.azurewebsites.net%2FCommon%2Fimages%2Fpushpins%2Fylw-pushpin.png
    ```
    Aşağıda, size yanıt görüntüsüdür.

    ![Özel Raptiyeler etiketleri ile işleme](./media/how-to-render-custom-data/render-pins.png)


## <a name="get-data-from-azure-maps-data-storage"></a>Azure haritalar veri depolamadan veri alma

> [!Note]
> Bu örnek Azure haritalar hesabı fiyatlandırma katmanı S1 ile gerektirir.

Yol ve PIN konum bilgileri de aracılığıyla edinilebilir [verileri karşıya yükleme API](https://docs.microsoft.com/rest/api/maps/mapdata/upload). Yol ve PIN verileri yüklemek için aşağıdaki adımları izleyin.

1. Postman uygulamasında aynı koleksiyonda yukarıda oluşturduğunuz yeni bir sekme açın. Oluşturucu sekmesinde POST HTTP yöntemini seçin ve bir POST isteğinde bulunmak için aşağıdaki URL'yi girin:

    ```HTTP
    https://atlas.microsoft.com/mapData/upload?subscription-key={subscription-key}&api-version=1.0&dataFormat=geojson
    ```

2. Params tıklayın ve POST isteği URL'sini kullanılacak aşağıdaki anahtar/değer çiftlerini girin. Abonelik anahtarı değerini Azure haritalar abonelik anahtarınız ile değiştirin.
    
    ![Anahtar-değer parametreleri: Postman](./media/how-to-render-custom-data/postman-key-vals.png)

3. Tıklayın **gövdesi** sonra ham giriş biçimini seçin ve JSON giriş biçimi açılır listeden seçin. Aşağıdaki JSON verileri olarak karşıya yüklenecek sağlar:
    
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

4. Gönder ve yanıt üst bilgisi gözden geçirin. Location üst bilgisini gelecekte kullanım için veri indirin veya URI içeriyor. Ayrıca bir benzersiz içerdiği `udId` karşıya yüklenen veriler için.   

  ```HTTP
  https://atlas.microsoft.com/mapData/{udId}/status?api-version=1.0&subscription-key={Subscription-key}
  ```



5. Değerini kullanmak `udid` eşleme özelliklerini işlemek için verileri karşıya yükleme API'sinden alınan. Bunu yapmak için yukarıda oluşturduğunuz aynı koleksiyonda yeni bir sekme açın. Oluşturucu sekmesinde GET HTTP yöntemini seçin ve bir GET isteği oluşturmak için aşağıdaki URL'yi girin:

    ```HTTP
    https://atlas.microsoft.com/map/static/png?subscription-key={subscription-key}&api-version=1.0&layer=basic&style=main&zoom=12&center=-73.96682739257812%2C40.78119135317995&pins=default|la-35+50|ls12|lc003C62|co9B2F15||'Times Square'-73.98516297340393 40.758781646381024|'Central Park'-73.96682739257812 40.78119135317995&path=lc0000FF|fc0000FF|lw3|la0.80|fa0.30||udid-{udId}
    ```

6. Yanıt aşağıdaki gibi görünmelidir:

    ![Karşıya yüklenen verileri işleme](./media/how-to-render-custom-data/uploaded-path.png)

## <a name="render-polygon-with-color-and-opacity"></a>Poligon renk ve saydamlık ile işleme

> [!Note]
> Bu örnek Azure haritalar hesabı fiyatlandırma katmanı S1 ile gerektirir.

Bir görünüm stili değiştiricilerini kullanarak değiştirebileceğiniz [yol parametresi](https://docs.microsoft.com/rest-staging/api/maps/render/getmapimage#uri-parameters).

1. Postman uygulamasında aynı koleksiyonda yukarıda oluşturduğunuz yeni bir sekme açın. Oluşturucu sekmesinde GET HTTP yöntemini seçin ve renk ve saydamlık sahip bir çokgenin işlemek için bir GET isteği oluşturmak için aşağıdaki URL'yi girin:
    
    ```HTTP
    https://atlas.microsoft.com/map/static/png?api-version=1.0&style=main&layer=basic&sku=S1&zoom=14&height=500&Width=500&center=-74.040701, 40.698666&path=lc0000FF|fc0000FF|lw3|la0.80|fa0.50||-74.03995513916016 40.70090237454063|-74.04082417488098 40.70028420372218|-74.04113531112671 40.70049568385827|-74.04298067092896 40.69899904076542|-74.04271245002747 40.69879568992435|-74.04367804527283 40.6980961582905|-74.04364585876465 40.698055487620714|-74.04368877410889 40.698022951066996|-74.04168248176573 40.696444909137|-74.03901100158691 40.69837271818651|-74.03824925422668 40.69837271818651|-74.03809905052185 40.69903971085914|-74.03771281242369 40.699340668780984|-74.03940796852112 40.70058515602143|-74.03948307037354 40.70052821920425|-74.03995513916016 40.70090237454063
    &subscription-key={subscription--key}
    ```

Yanıt aşağıdaki gibi görünmelidir:

![Donuk Çokgen işleme](./media/how-to-render-custom-data/opaque-polygon.png)


## <a name="render-polygon-with-circle-and-push-pins-with-custom-labels"></a>Çokgen özel etiketler ile daire ve anında iletme PIN ile işleme

> [!Note]
> Bu örnek Azure haritalar hesabı fiyatlandırma katmanı S1 ile gerektirir.

Raptiyeler ve bunların etiketleri daha büyük veya küçük 'sc' ölçek stil değiştiricisini kullanarak yapabilirsiniz. Sıfırdan büyük bir değer budur. Bir 1 standart ölçek değeri. PIN 1'den büyük değerler daha büyük hale getirir ve değerleri 1'den küçük bunları daha küçük hale getirir. Stili hakkında daha fazla bilgi için bkz. değiştiriciler, [statik Görüntü hizmeti yol parametreleri](https://docs.microsoft.com/rest/api/maps/render/getmapimage#uri-parameters).

Bir çokgenin özel etiketler ile daire ve anında iletme PIN ile işlemek için aşağıdaki adımları izleyin:

1. Postman uygulamasında aynı koleksiyonda yukarıda oluşturduğunuz yeni bir sekme açın. Oluşturucu sekmesinde GET HTTP yöntemini seçin ve bir GET isteği oluşturmak için aşağıdaki URL'yi girin:

    ```HTTP
    https://atlas.microsoft.com/map/static/png?api-version=1.0&style=main&layer=basic&zoom=14&height=700&Width=700&center=-122.13230609893799,47.64599069048016&path=lcFF0000|lw2|la0.60|ra1000||-122.13230609893799 47.64599069048016&pins=default|la15+50|al0.66|lc003C62|co002D62||'Microsoft Corporate Headquarters'-122.14131832122801  47.64690503939462|'Microsoft Visitor Center'-122.136828 47.642224|'Microsoft Conference Center'-122.12552547454833 47.642940335653996|'Microsoft The Commons'-122.13687658309935  47.64452336193245&subscription-key={subscription-key}
    ```

Yanıt aşağıdaki gibi görünmelidir:

![Özel PIN'ler daire işleme](./media/how-to-render-custom-data/circle-custom-pins.png)

## <a name="next-steps"></a>Sonraki adımlar

* Keşfedin [Azure haritalar alma harita görüntü API'si](https://docs.microsoft.com/rest/api/maps/search) belgeleri.
* Azure haritalar veri hizmeti işlevleri hakkında daha fazla bilgi için bkz: [belgeleri hizmet](https://docs.microsoft.com/rest/api/maps/data).