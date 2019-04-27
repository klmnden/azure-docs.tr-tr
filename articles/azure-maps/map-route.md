---
title: Azure Haritalar ile yönleri gösterme | Microsoft Docs
description: Bir Javascript harita üzerinde iki konum arasında yönergeleri görüntüleme
author: jingjing-z
ms.author: jinzh
ms.date: 3/7/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: codepen
ms.openlocfilehash: b8205383c25ba04212126e0e6ca1bd44e4efad1a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60768912"
---
# <a name="show-directions-from-a-to-b"></a>A'dan B'ye yönleri gösterme

Bu makalede bir rota istekte bulunmak ve harita üzerinde yolu göstermek gösterilmektedir.

Bunu yapmak için iki yolu vardır. İlk yolu sorgu [Azure haritalar rota API'si](https://docs.microsoft.com/rest/api/maps/route/getroutedirections) hizmeti modülü aracılığıyla. İkinci yol kullanmaktır [Fetch API'sini](https://fetch.spec.whatwg.org/) arama isteğin yapılacağı [Azure haritalar rota API'si](https://docs.microsoft.com/rest/api/maps/route/getroutedirections). Her iki yönde aşağıda ele alınmıştır.

## <a name="query-the-route-via-service-module"></a>Sorgu yönlendirme hizmeti modülü aracılığıyla

<iframe height='500' scrolling='no' title='Yönergeleri A'dan B'ye haritada (hizmet Modülü) göster' src='//codepen.io/azuremaps/embed/RBZbep/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/RBZbep/'>Göster yönergeleri A'dan B'ye haritada (hizmet Modülü)</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kodda, ilk kod bloğunu bir harita nesnesi oluşturur ve bir abonelik anahtarı kullanmak için kimlik doğrulama mekanizması ayarlar. Gördüğünüz [bir harita oluşturmak](./map-create.md) yönergeler için.

İkinci kod bloğunu oluşturur bir `SubscriptionKeyCredentialPolicy` abonelik anahtarını Azure haritalar için HTTP isteklerinde kimlik doğrulaması için. `atlas.service.MapsURL.newPipeline()` Alır `SubscriptionKeyCredential` ilke ve oluşturan bir [işlem hattı](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.pipeline?view=azure-iot-typescript-latest) örneği. `routeURL` Azure haritalar için URL'yi temsil [rota](https://docs.microsoft.com/rest/api/maps/route) operations.

Üçüncü kod bloğunu oluşturur ve ekler bir [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) eşleme nesnesi.

Başlangıç ve bitiş kodu dördüncü bloğu oluşturur [noktaları](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest) nesneleri ve bunları veri kaynağı nesneye ekler.

Bir satır bir [özellik](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature?view=azure-iot-typescript-latest) LineString. A [LineLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.linelayer?view=azure-iot-typescript-latest) işler satır içinde sarmalanmış nesneleri [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) haritada satırları olarak. Dördüncü kod bloğunu oluşturur ve bir çizgi katmanı haritaya ekler. Bir satır katmanında özelliklerini görmek [LinestringLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.linelayeroptions?view=azure-iot-typescript-latest).

A [sembol katman](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest) sarmalanmış noktası tabanlı veri işleme için metin veya simge kullanan [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) harita üzerinde simgeler olarak. Beşinci kod bloğunu oluşturur ve bir simge katmanı haritaya ekler.

Altıncı kod bloğunu parçası olan Azure haritalar yönlendirme hizmeti, sorgular, [hizmeti Modülü](https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas-service.min.js). [CalculateRouteDirections](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.routeurl?view=azure-iot-typescript-latest#methods) RouteURL yöntemi, başlangıç ve bitiş noktaları arasında bir tarifi almak için kullanılır. Kullanarak bir GeoJSON özellik koleksiyonundan yanıt ayıklanır `geojson.getFeatures()` yöntemi ve veri kaynağı için eklenir. Ardından harita üzerinde bir yol olarak yanıta işler. Haritayı bir satır ekleme hakkında daha fazla bilgi için bkz. [harita üzerinde bir satır ekleyin](./map-add-shape.md#addALine).

Son kod bloğunu kullanarak haritanın harita sınırları ayarlar [setCamera](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamera-cameraoptions---cameraboundsoptions---animationoptions-) özelliği.

Rota sorgu, veri kaynağı, simge ve çizgi katmanları ve kamera sınırları oluşturulur ve haritanın içinde ayarlamak [olay dinleyicisi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) harita tamamen yüklendikten sonra sonuçları görüntülendiğinden emin olmak için.

## <a name="query-the-route-via-fetch-api"></a>Sorgu Fetch API aracılığıyla yönlendirme

<iframe height='500' scrolling='no' title='A noktasından B noktasına yol tarifini haritada göster' src='//codepen.io/azuremaps/embed/zRyNmP/?height=469&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/zRyNmP/'>Göster yönergeleri A'dan B'ye bir haritada</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kodda, ilk kod bloğunu bir harita nesnesi oluşturur ve bir abonelik anahtarı kullanmak için kimlik doğrulama mekanizması ayarlar. Gördüğünüz [bir harita oluşturmak](./map-create.md) yönergeler için.

İkinci kod bloğunu oluşturur ve ekler bir [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) eşleme nesnesi.

Üçüncü kod bloğu, yolun başlangıç ve hedef noktaları oluşturur ve bunları veri kaynağına ekler. Gördüğünüz [haritada pin ekleme](map-add-pin.md) kullanma hakkında yönergeler için [addPins](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest).

A [LineLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.linelayer?view=azure-iot-typescript-latest) işler satır içinde sarmalanmış nesneleri [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) haritada satırları olarak. Dördüncü kod bloğunu oluşturur ve bir çizgi katmanı haritaya ekler. Bir satır katmanında özelliklerini görmek [LineLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.linelayeroptions?view=azure-iot-typescript-latest).

A [sembol katman](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest) sarmalanmış noktası tabanlı veri işleme için metin veya simge kullanan [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) harita üzerinde simgeler olarak. Beşinci kod bloğunu oluşturur ve bir simge katmanı haritaya ekler. Sembol katmanında özelliklerini görmek [SymbolLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.symbollayeroptions?view=azure-iot-typescript-latest).

Sonraki kod bloğu oluşturur `SouthWest` ve `NorthEast` işaret başlangıç ve hedef noktalarından ve ayarlar kullanarak haritanın harita sınırları [setCamera](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamera-cameraoptions---cameraboundsoptions---animationoptions-) özelliği.

Son kod bloğunu kullanır [Fetch API'sini](https://fetch.spec.whatwg.org/) arama isteğin yapılacağı [Azure haritalar rota API'si](https://docs.microsoft.com/rest/api/maps/route/getroutedirections). Yanıt ardından ayrıştırılır. Yanıt başarılı olduysa, enlem ve boylam bilgileri bu noktaları arasında bağlantı kurarak bir çizgi bir dizi oluşturmak için kullanılır. Satır verileri, yol haritasında işlemek için veri kaynağına sonra eklenir. Gördüğünüz [harita üzerinde bir satır ekleyin](./map-add-shape.md#addALine) yönergeler için.

Rota sorgu, veri kaynağı, simge ve çizgi katmanları ve kamera sınırları oluşturulur ve haritanın içinde ayarlamak [olay dinleyicisi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) harita tamamen yüklendikten sonra sonuçları görüntülendiğinden emin olmak için.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan yöntemleri ve sınıfları hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Harita](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

Tam kod örnekleri için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Harita üzerinde trafiği Göster](./map-show-traffic.md)

> [!div class="nextstepaction"]
> [Eşleme - fare olayları ile etkileşim kurma](./map-events.md)
