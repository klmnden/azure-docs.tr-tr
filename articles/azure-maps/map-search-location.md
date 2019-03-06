---
title: Azure Haritalar ile arama sonuçlarını gösterme | Microsoft Docs
description: Bir arama gerçekleştirme isteği ile Azure haritalar sonra sonuçları bir Javascript harita üzerinde görüntüleyebilirsiniz
author: jingjing-z
ms.author: jinzh
ms.date: 11/15/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 0db64916ca84737a6a713c3d2544b2f48c3682c0
ms.sourcegitcommit: 94305d8ee91f217ec98039fde2ac4326761fea22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2019
ms.locfileid: "57403100"
---
# <a name="show-search-results-on-the-map"></a>Harita üzerinde arama sonuçlarını göster

Bu makalede ilgi konumu aramak ve arama sonuçlarını haritada gösterme gösterilmektedir.

İlgilendiğiniz bir konumu aramak için iki yolu vardır. Arama istekte bulunmak için bir hizmet modülü kullanmak bir yoludur. Arama isteği aracılığıyla yapmak için diğer yol ise bir [XMLHttpRequest](https://xhr.spec.whatwg.org/) için [Azure haritalar belirsiz arama API'si](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy). Her iki yönde aşağıda ele alınmıştır.

## <a name="make-a-search-request-via-service-module"></a>Bir arama hizmeti modülü aracılığıyla istekte

<iframe height='500' scrolling='no' title='Arama sonuçları bir haritada (hizmet Modülü) göster' src='//codepen.io/azuremaps/embed/zLdYEB/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/zLdYEB/'>Show arama sonuçları bir haritada (hizmet Modülü)</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod, kod bloğunun ilk eşleme nesnesi oluşturur ve istemci hizmetini başlatır. Gördüğünüz [bir harita oluşturmak](./map-create.md) yönergeler için.

İkinci kod bloğunu kullanır `getSearchFuzzy` yönteminde [hizmeti Modülü](https://atlas.microsoft.com/sdk/js/atlas-service.js?api-version=1). Bu serbest biçimli metin arama yoluyla gerçekleştirmenize olanak tanıyan [belirsiz arama rest API](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) için ilgi noktası arama. Belirsiz arama API'si benzer girişler herhangi bir birleşimini başa çıkabilir. GeoJSON biçiminde kullanarak belirsiz arama hizmetinden gelen yanıt ayrıştırılır `getGeoJsonSearchResponse` yöntemi. 

Üçüncü kod bloğunun bir veri kaynağı nesnesi kullanılarak oluşturur [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) sınıfı ve arama sonuçları ekleyin. A [sembol katman](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest) sarmalanmış noktası tabanlı veri işleme için metin veya simge kullanan [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) harita üzerinde simgeler olarak.  Bir sembol katmanı sonra oluşturulur ve ardından eşlemesine eklenen sembol katmanı, veri kaynağı eklenir.

Haritanın kullanarak son blok kod eşlemesi için kamera sınırları ayarlar [setCamera](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamera-cameraoptions---cameraboundsoptions---animationoptions-) özelliği.

Arama isteği, veri kaynağı ve sembol katman ve kamera sınırları oluşturulur ve haritanın içinde ayarlamak [olay dinleyicisi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) harita tamamen yüklendikten sonra sonuçları görüntülendiğinden emin olmak için.


## <a name="make-a-search-request-via-xmlhttprequest"></a>Arama isteği aracılığıyla XMLHttpRequest olun

<iframe height='500' scrolling='no' title='Arama sonuçları bir haritada gösterme' src='//codepen.io/azuremaps/embed/KQbaeM/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/KQbaeM/'>Show arama sonuçları bir haritada</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod, kod bloğunun ilk harita nesnesi oluşturur. Gördüğünüz [bir harita oluşturmak](./map-create.md) yönergeler için.

İkinci kod bloğunu gönderen bir [XMLHttpRequest](https://xhr.spec.whatwg.org/) için [Azure haritalar belirsiz arama API'si](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) ilgi noktası için aranacak. Belirsiz arama API'si benzer girişler herhangi bir birleşimini başa çıkabilir. 

Üçüncü kod bloğunu arama yanıt ayrıştırır ve sonuçları sınırları hesaplamak için bir dizide saklar. Ardından, arama sonuçlarını döndürür.

Dördüncü kod bloğunun bir veri kaynağı nesnesi kullanılarak oluşturur [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) sınıfı ve arama sonuçları ekleyin. A [sembol katman](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest) sarmalanmış noktası tabanlı veri işleme için metin veya simge kullanan [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) harita üzerinde simgeler olarak. Bir sembol katmanı sonra oluşturulur ve ardından eşlemesine eklenen sembol katmanı, veri kaynağı eklenir.

Son blok kod oluşturur bir [sınırlayıcı kutu](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.boundingbox?view=azure-iot-typescript-latest) Sonuçlar dizisi kullanarak nesne ve sonra haritanın kullanarak kamera sınırları eşlemesi için ayarlar [setCamera](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamera-cameraoptions---cameraboundsoptions---animationoptions-). Bunu ardından işler sonucu PIN.

Arama isteği, veri kaynağı ve sembol katman ve kamera sınırları haritanın içinde ayarlanan [olay dinleyicisi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) harita tamamen yüklendikten sonra sonuçları görüntülendiğinden emin olmak için.

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
