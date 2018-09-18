---
title: Azure Haritalar ile yönleri gösterme | Microsoft Docs
description: Bir Javascript harita üzerinde iki konum arasında yönergeleri görüntüleme
author: jingjing-z
ms.author: jinzh
ms.date: 09/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: codepen
ms.openlocfilehash: ed522779f5a86e38ee12a246cea9ac85d0379f9e
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/17/2018
ms.locfileid: "45729072"
---
# <a name="show-directions-from-a-to-b"></a>A'dan B'ye yönleri gösterme

Bu makalede bir rota istekte bulunmak ve harita üzerinde yolu göstermek gösterilmektedir.

Bunu yapmak için iki yolu vardır. İlk yolu sorgu [Azure haritalar rota API'si](https://docs.microsoft.com/rest/api/maps/route/getroutedirections) hizmeti modülü aracılığıyla. İkinci yol olmaktır bir [XMLHttpRequest](https://xhr.spec.whatwg.org/) API. Her iki yönde aşağıda ele alınmıştır.

## <a name="query-the-route-via-service-module"></a>Sorgu yönlendirme hizmeti modülü aracılığıyla

<iframe height='500' scrolling='no' title='Yönergeleri A'dan B'ye haritada (hizmet Modülü) göster' src='//codepen.io/azuremaps/embed/RBZbep/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/RBZbep/'>Göster yönergeleri A'dan B'ye haritada (hizmet Modülü)</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

İlk kod bloğunu bir harita nesnesi oluşturur. Gördüğünüz [bir harita oluşturmak](./map-create.md) yönergeler için.

İkinci kod bloğunu satırda hizmeti istemcisi oluşturur.

Üçüncü kod bloğu başlatır [dize katman satır](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addlinestrings) harita üzerinde.

Dördüncü kod bloğunu oluşturur ve başlangıç ve bitiş noktasına rota temsil etmek için harita üzerinde iğne ekler. Gördüğünüz [haritada pin ekleme](map-add-pin.md) kullanma hakkında yönergeler için [addPins](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addpins).

Sonraki kod bloğu kullanır [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamerabounds) harita sınırlayıcı kutu ayarlamak için eşleme sınıfının işlevini alarak başlangıç ve bitiş noktası yolu.

Altıncı kod bloğu bir rota sorgu oluşturur.

Azure haritalar yönlendirme hizmeti aracılığıyla son kod bloğunu sorgular [getRouteDirections](https://docs.microsoft.com/javascript/api/azure-maps-rest/services.route?view=azure-iot-typescript-latest#getroutedirections) başlangıç ve hedef noktası arasındaki bir yolu almak için yöntemi. GeoJSON biçiminde kullanarak yanıt ayrıştırılır [getGeoJsonRoutes](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.geojson.geojsonroutedirectionsresponse?view=azure-iot-typescript-latest#getgeojsonroutes) yöntemi. Rota işlemek için haritada tüm bu satırlara ekler. Daha fazla bilgi için [haritaya satır ekleme](./map-add-shape.md#addALine) konusuna bakabilirsiniz.

## <a name="query-the-route-via-xmlhttprequest"></a>Rota XMLHttpRequest üzerinden sorgulama

<iframe height='500' scrolling='no' title='A noktasından B noktasına yol tarifini haritada göster' src='//codepen.io/azuremaps/embed/zRyNmP/?height=469&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/zRyNmP/'>Göster yönergeleri A'dan B'ye bir haritada</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

İlk kod bloğunu bir harita nesnesi oluşturur. Gördüğünüz [bir harita oluşturmak](./map-create.md) yönergeler için.

İkinci kod bloğunu oluşturur ve başlangıç ve bitiş noktasına rota temsil etmek için harita üzerinde iğne ekler. Gördüğünüz [haritada pin ekleme](map-add-pin.md) kullanma hakkında yönergeler için [addPins](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addpins).

Üçüncü kod bloğunu kullanır [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamerabounds) harita sınırlayıcı kutu ayarlamak için eşleme sınıfının işlevini alarak başlangıç ve bitiş noktası yolu.

Dördüncü kod bloğunu gönderen bir [XMLHttpRequest](https://xhr.spec.whatwg.org/) için [Azure haritalar rota API'si](https://docs.microsoft.com/rest/api/maps/route/getroutedirections).

Son kod bloğunu gelen yanıtları ayrıştırmak için. Başarılı bir yanıt için her güzergah noktası için enlem ve boylam bilgilerini toplar. Her güzergah noktası için sonraki kendi güzergah noktası bağlanarak satırları bir dizi oluşturur. Rota işlemek için haritada tüm bu satırlara ekler. Gördüğünüz [harita üzerinde bir satır ekleyin](./map-add-shape.md#addALine) yönergeler için.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan yöntemleri ve sınıfları hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Harita](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

Tam kod örnekleri için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Harita üzerinde trafiği Göster](./map-show-traffic.md)

> [!div class="nextstepaction"]
> [Eşleme - fare olayları ile etkileşim kurma](./map-events.md)
