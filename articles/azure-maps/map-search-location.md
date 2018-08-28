---
title: Azure Haritalar ile arama sonuçlarını gösterme | Microsoft Docs
description: Bir arama gerçekleştirme isteği ile Azure haritalar sonra sonuçları Javascrip haritada görüntüleme
author: jingjing-z
ms.author: jinzh
ms.date: 08/26/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 540faa04573f7fa66b574f6743d42adfe855c571
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43095264"
---
# <a name="show-search-results-on-the-map"></a>Harita üzerinde arama sonuçlarını göster

Bu makalede ilgi konumu aramak ve arama sonuçlarını haritada gösterme gösterilmektedir. 

## <a name="understand-the-code"></a>Kodu anlama

<iframe height='305' scrolling='no' title='Arama sonuçları bir haritada (hizmet Modülü) göster' src='//codepen.io/azuremaps/embed/zLdYEB/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/zLdYEB/'>Show arama sonuçları bir haritada (hizmet Modülü)</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod, kod bloğunun ilk eşleme nesnesi oluşturur ve istemci hizmeti örneğini oluşturur. Gördüğünüz [bir harita oluşturmak](./map-create.md) yönergeler için.

İkinci kod bloğu, belirsiz arama kullanır [Azure haritalar belirsiz arama API'si](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) için ilgi noktası arama. Belirsiz arama API'si benzer girişler herhangi bir birleşimini başa çıkabilir. Belirsiz arama hizmetinden gelen yanıt ardından GeoJSON biçiminde ayrıştırılır ve PIN'leri ilgi çekici noktalar haritada gösterilecek eşlemesine eklenir. 

Haritanın kullanarak son blok kod eşlemesi için kamera sınırları ekler [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-control/models.cameraboundsoptions?view=azure-iot-typescript-latest) özelliği.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan yöntemleri ve sınıfları hakkında daha fazla bilgi edinin: 

* [Azure haritalar belirsiz arama API'si](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy)
* [Harita](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)
    * [addPins](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addpins)
    
Eşlenir eklemek daha fazla kod örnekleri için aşağıdaki makalelere bakın: 
* [Bir Koordinattan bilgi alma](./map-get-information-from-coordinate.md)
* [Yönergeleri A'dan B'ye yönleri gösterme](./map-route.md)
