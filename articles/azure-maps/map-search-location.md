---
title: Azure Haritalar ile arama sonuçlarını gösterme | Microsoft Docs
description: Bir arama gerçekleştirme isteği ile Azure haritalar sonra sonuçları bir Javascript harita üzerinde görüntüleyebilirsiniz
author: jingjing-z
ms.author: jinzh
ms.date: 08/31/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 7d4eb5f9be4a6bcefe4b544d3f97a9b9391c0d81
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2018
ms.locfileid: "43665786"
---
# <a name="show-search-results-on-the-map"></a>Harita üzerinde arama sonuçlarını göster

Bu makalede ilgi konumu aramak ve arama sonuçlarını haritada gösterme gösterilmektedir. 

## <a name="understand-the-code"></a>Kodu anlama

<iframe height='500' scrolling='no' title='Arama sonuçları bir haritada (hizmet Modülü) göster' src='//codepen.io/azuremaps/embed/zLdYEB/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/zLdYEB/'>Show arama sonuçları bir haritada (hizmet Modülü)</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod, kod bloğunun ilk eşleme nesnesi oluşturur ve istemci hizmeti örneğini oluşturur. Gördüğünüz [bir harita oluşturmak](./map-create.md) yönergeler için.

İkinci kod bloğu, belirsiz arama kullanır [Azure haritalar belirsiz arama API'si](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) için ilgi noktası arama. Belirsiz arama API'si benzer girişler herhangi bir birleşimini başa çıkabilir. GeoJSON biçiminde kullanarak belirsiz arama hizmetinden gelen yanıt ayrıştırılır [getGeoJsonSearchResponse](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.geojson.geojsonsearchresponse?view=azure-iot-typescript-latest#geojsonsearchresponse) yöntemi. PIN, ardından ilgi çekici noktalar haritada gösterilecek eşlemesine eklenir.

Haritanın kullanarak son blok kod eşlemesi için kamera sınırları ekler [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-control/models.cameraboundsoptions?view=azure-iot-typescript-latest) özelliği.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan yöntemleri ve sınıfları hakkında daha fazla bilgi edinin: 

* [Azure haritalar belirsiz arama API'si](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy)
* [Harita](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)
    * [addPins](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addpins)
    
Eşlenir eklemek daha fazla kod örnekleri için aşağıdaki makalelere bakın: 
* [Bir Koordinattan bilgi alma](./map-get-information-from-coordinate.md)
* [Yönergeleri A'dan B'ye yönleri gösterme](./map-route.md)
