---
title: Azure haritalar koordinatıyla ilgili bilgileri gösterme | Microsoft Docs
description: Bir kullanıcı bir koordinat seçtiğinde harita üzerinde bir adresi hakkında bilgi görüntüleme
author: jingjing-z
ms.author: jinzh
ms.date: 3/7/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 50f906a9d8a0dc19f5eb47bef4cb68f4703f020f
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59256065"
---
# <a name="get-information-from-a-coordinate"></a>Bir koordinattan bilgi alma

Bu makale tıklandı açılan konumunun adresini gösteren bir ters adresi arama yapma.

Ters adresi arama yapmak için iki yol vardır. Bir yolu Sorgulanacak [Azure haritalar ters adresi arama API'si](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) hizmeti modülü aracılığıyla. Yazılımınız için diğer yol ise [Fetch API'sini](https://fetch.spec.whatwg.org/) için istekte bulunmak için [Azure haritalar ters adresi arama API'si](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) bir adresi bulunamıyor. Her iki yönde, aşağıda alanında.

## <a name="make-a-reverse-search-request-via-service-module"></a>Hizmeti modülü aracılığıyla geriye doğru arama istekte

<iframe height='500' scrolling='no' title='(Hizmeti Modülü) bir Koordinattan bilgi alın' src='//codepen.io/azuremaps/embed/ejEYMZ/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/ejEYMZ/'>(hizmet Modülü) bir Koordinattan bilgi alma</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kodda, ilk kod bloğunu bir harita nesnesi oluşturur ve bir abonelik anahtarı kullanmak için kimlik doğrulama mekanizması ayarlar. Gördüğünüz [bir harita oluşturmak](./map-create.md) yönergeler için.

İkinci kod bloğunu oluşturur bir `SubscriptionKeyCredentialPolicy` abonelik anahtarını Azure haritalar için HTTP isteklerinde kimlik doğrulaması için. Ardından `atlas.service.MapsURL.newPipeline()` alır `SubscriptionKeyCredential` ilke ve oluşturan bir [işlem hattı](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.pipeline?view=azure-iot-typescript-latest) örneği. `searchURL` Azure haritalar için URL'yi temsil [arama](https://docs.microsoft.com/rest/api/maps/search) operations.

Üçüncü kod bloğunu bir işaretçiye fare imlecini stilini güncelleştirir ve oluşturur bir [açılan](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#open) nesne. Gördüğünüz [harita üzerinde bir açılır pencere ekleme](./map-add-popup.md) yönergeler için.

Dördüncü kod bloğunu bir fare tıklaması ekler [olay dinleyicisi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events). Tetiklendiğinde, Tıklatılan noktadan koordinatlarını bir arama sorgusu oluşturur. Ardından hizmet modülün kullanır [getSearchAddressReverse](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchurl?view=azure-iot-typescript-latest#searchaddressreverse-aborter--geojson-position--searchaddressreverseoptions-) sorgu yöntemine [arama adres ters API alma](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) koordinatları adresi için. Kullanarak bir GeoJSON özellik koleksiyonundan yanıt ayıklanır `geojson.getFeatures()` yöntemi.

Beşinci kod bloğunu HTML açılan içerik tıklandı koordinat konumu yanıt adresini görüntülemek için ayarlar.

İmleç, bir açılan pencere nesnesi ve tıklama olayı değişikliği tüm oluşturulan haritanın içinde [yük olay dinleyicisi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) koordinatları bilgiler alınabilir önce harita yükleri tam olarak emin olmak için.

## <a name="make-a-reverse-search-request-via-fetch-api"></a>Getirme API aracılığıyla geriye doğru arama istekte

Getirme kullanarak o konuma ters geocode istekte bulunmak için harita üzerinde tıklayın.

<iframe height='500' scrolling='no' title='Bir koordinattan bilgi alma' src='//codepen.io/azuremaps/embed/ddXzoB/?height=516&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/ddXzoB/'>bir Koordinattan bilgi alma</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kodda, ilk kod bloğunu bir harita nesnesi oluşturur ve bir abonelik anahtarı kullanmak için kimlik doğrulama mekanizması ayarlar. Gördüğünüz [bir harita oluşturmak](./map-create.md) yönergeler için.

İkinci kod bloğunu bir işaretçiye fare imlecini stilini güncelleştirir ve [açılan](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#open) nesne. Gördüğünüz [harita üzerinde bir açılır pencere ekleme](./map-add-popup.md) yönergeler için.

Üçüncü kod bloğu için fare tıklama olay dinleyicisi ekler. Fare tıklatın, bunu kullanan [Fetch API'sini](https://fetch.spec.whatwg.org/) Sorgulanacak [Azure haritalar ters adresi arama API'si](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) tıklandı koordinatları adresi. Başarılı bir yanıt için tıklatılan konumu adresi toplar ve açılan içeriğini ve konumunu aracılığıyla tanımlar [setOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#setoptions-popupoptions-) açılan sınıfının işlevi.

İmleç, bir açılan pencere nesnesi ve tıklama olayı değişikliği tüm oluşturulan haritanın içinde [yük olay dinleyicisi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) koordinatları bilgiler alınabilir önce harita yükleri tam olarak emin olmak için.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan yöntemleri ve sınıfları hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Eşleme](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [Açılan menüsü](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest)

Tam kod örnekleri için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [A'dan B'ye yönleri gösterme](./map-route.md)

> [!div class="nextstepaction"]
> [Trafiği gösterme](./map-show-traffic.md)
