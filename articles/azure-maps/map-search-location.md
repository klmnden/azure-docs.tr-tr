---
title: Azure Haritalar ile arama sonuçlarını gösterme | Microsoft Docs
description: Bir arama gerçekleştirme isteği ile Azure haritalar sonra sonuçları bir Javascript harita üzerinde görüntüleyebilirsiniz
author: jingjing-z
ms.author: jinzh
ms.date: 09/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: cf27864d691fe2fe13c9483348fb2abed121874d
ms.sourcegitcommit: a4e4e0236197544569a0a7e34c1c20d071774dd6
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51713512"
---
# <a name="show-search-results-on-the-map"></a>Harita üzerinde arama sonuçlarını göster

Bu makalede ilgi konumu aramak ve arama sonuçlarını haritada gösterme gösterilmektedir.

İlgilendiğiniz bir konumu aramak için iki yolu vardır. Arama istekte bulunmak için bir hizmet modülü kullanmak bir yoludur. Arama isteği aracılığıyla yapmak için diğer yol ise bir [XMLHttpRequest](https://xhr.spec.whatwg.org/) için [Azure haritalar belirsiz arama API'si](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy). Her iki yönde aşağıda ele alınmıştır.

## <a name="make-a-search-request-via-service-module"></a>Bir arama hizmeti modülü aracılığıyla istekte

<iframe height='500' scrolling='no' title='Arama sonuçları bir haritada (hizmet Modülü) göster' src='//codepen.io/azuremaps/embed/zLdYEB/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/zLdYEB/'>Show arama sonuçları bir haritada (hizmet Modülü)</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

İlk bloğu kod eşleme nesnesi oluşturur ve istemci hizmetini başlatır. Gördüğünüz [bir harita oluşturmak](./map-create.md) yönergeler için.

İkinci kod bloğunu kullanır [Azure haritalar belirsiz arama API'si](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) için ilgi noktası arama. Belirsiz arama API'si benzer girişler herhangi bir birleşimini başa çıkabilir. GeoJSON biçiminde kullanarak belirsiz arama hizmetinden gelen yanıt ayrıştırılır [getGeoJsonSearchResponse](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.geojson.geojsonsearchresponse?view=azure-iot-typescript-latest#geojsonsearchresponse) yöntemi. PIN, ardından ilgi çekici noktalar haritada gösterilecek eşlemesine eklenir.

Haritanın kullanarak son blok kod eşlemesi için kamera sınırları ayarlar [setCameraBounds](/javascript/api/azure-maps-control/atlas.map#setcamerabounds-cameraboundsoptions-) özelliği.

## <a name="make-a-search-request-via-xmlhttprequest"></a>Arama isteği aracılığıyla XMLHttpRequest olun

<iframe height='500' scrolling='no' title='Arama sonuçları bir haritada gösterme' src='//codepen.io/azuremaps/embed/KQbaeM/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/KQbaeM/'>Show arama sonuçları bir haritada</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

İlk kod bloğunu bir harita nesnesi oluşturur. Gördüğünüz [bir harita oluşturmak](./map-create.md) yönergeler için.

İkinci kod bloğu arama sonuçları katmanı haritaya ekler. Arama sonuçları katman haritada sabitler gibi arama sonuçlarını görüntüler. PIN kullanarak eklenir [addPins](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addpins).

Üçüncü kod bloğunu gönderen bir [XMLHttpRequest](https://xhr.spec.whatwg.org/) için [Azure haritalar belirsiz arama API'si](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) ilgi noktası için aranacak. Belirsiz arama API'si benzer girişler herhangi bir birleşimini başa çıkabilir.

Son kod bloğunu yanıt ayrıştırır ve ayarlar harita kamera sınırları haritanın kullanarak ayarlar [setCameraBounds](/javascript/api/azure-maps-control/atlas.map#setcamerabounds-cameraboundsoptions-) sonucu PIN'ler işlemek için.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinin **belirsiz arama**:

> [!div class="nextstepaction"]
> [Azure haritalar belirsiz arama API'si](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy)

Bu makalede kullanılan yöntemleri ve sınıfları hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Harita](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

Tam kod örnekleri için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Bir Koordinattan bilgi alma](./map-get-information-from-coordinate.md)
<!-- Comment added to suppress false positive warning -->
> [!div class="nextstepaction"]
> [Yönergeleri A'dan B'ye yönleri gösterme](./map-route.md)
