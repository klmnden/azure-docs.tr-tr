---
title: Azure Haritalar ile bir şekil eklemek | Microsoft Docs
description: Javascript harita bir şekil ekleme
author: jingjing-z
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: b1fe17adc80fc7f93f1511d577b1dc363e36e2e3
ms.sourcegitcommit: b5ac31eeb7c4f9be584bb0f7d55c5654b74404ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/23/2018
ms.locfileid: "42746010"
---
# <a name="add-a-shape-to-a-map"></a>Şekil Haritası ekleme

Bu makalede bir satır, bir daire ve bir Çokgen haritaya eklemek gösterilmektedir. 

<a id="addALine"></a>

## <a name="add-a-line"></a>Bir satır ekleyin

<iframe height='500' scrolling='no' title='Bir satır için bir eşleme ekleyin' src='//codepen.io/azuremaps/embed/qomaKv/?height=534&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/qomaKv/'>bir satır eklemek için bir harita</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod, kod bloğunun ilk harita nesnesi oluşturur. Gördüğünüz [bir harita oluşturmak](./map-create.md) yönergeler için.

İkinci kod bloğu içinde bir satır oluşturulur. Bir satır bir [özellik](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.atlas.data.feature?view=azure-iot-typescript-latest) , LineString, özellik olarak LineStringProperties ile. Kullanım `new atlas.data.Feature(new atlas.data.LineString())` bir satır oluşturun ve özelliklerini tanımlamak için. 

Çizgi katmanı satırları dizisidir. Son kod bloğunu kullanır [addLineStrings](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addlinestrings) çizgi katmanı haritaya eklemek ve çizgi katmanı özelliklerini tanımlamak için harita sınıfın işlevi. Bir satır katmanında özelliklerini görmek [LinestringLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/models.linestringlayeroptions?view=azure-iot-typescript-latest).

<a id="addACircle"></a>

## <a name="add-a-circle"></a>Bir daire ekleyin

<iframe height='500' scrolling='no' title='Daire Harita Ekle' src='//codepen.io/azuremaps/embed/PRmzJX/?height=538&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/PRmzJX/'>bir daire harita eklemek</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod, kod bloğunun ilk harita nesnesi oluşturur. Gördüğünüz [bir harita oluşturmak](./map-create.md) yönergeler için.

İkinci kod bloğunda bir daire oluşturulur. Bir daire olduğu bir [özellik](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.atlas.data.feature?view=azure-iot-typescript-latest) , [noktası](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.atlas.data.point?view=azure-iot-typescript-latest) ile [CircleProperties](https://docs.microsoft.com/javascript/api/azure-maps-control/modelscircleproperties?view=azure-iot-typescript-latest) kendi özellik özelliği olarak. Kullanım `new atlas.data.Feature(new atlas.data.Point())` bir daire oluşturmak ve özelliklerini tanımlamak için.

Bir daire katmanı daireler dizisidir. Son kod bloğunu kullanır [addCircle](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addcircles) daire katmanı haritaya eklemek ve daire katman özelliklerini tanımlamak için harita sınıfın işlevi. Bir daire katmanında özelliklerini görmek [CircleLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/models.circlelayeroptions?view=azure-iot-typescript-latest).

<a id="addAPolygon"></a>

## <a name="add-a-polygon"></a>Bir çokgenin Ekle
<iframe height='500' scrolling='no' title='Bir çokgenin bir haritaya eklemek ' src='//codepen.io/azuremaps/embed/yKbOvZ/?height=543&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/yKbOvZ/'>Çokgen bir haritaya eklemek </a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod, kod bloğunun ilk harita nesnesi oluşturur. Gördüğünüz [bir harita oluşturmak](./map-create.md) yönergeler için.

Bir çokgenin ikinci kod bloğunda oluşturulur. Bir Çokgen bir [özellik](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.atlas.data.feature?view=azure-iot-typescript-latest) , [Çokgen](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.atlas.data.polygon?view=azure-iot-typescript-latest) ile [PolygonProperties](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.atlas.data.polygonproperties?view=azure-iot-typescript-latest) kendi özellik özelliği olarak. Kullanım `new atlas.data.Feature(new atlas.data.Polygon())` Çokgen oluşturun ve özelliklerini tanımlamak için. Sıralı koordinatlarını Çokgen Oluşturucusu Çokgen yolu sağlayın.

Çokgen katmanı çokgenler dizisidir. Son kod bloğunu kullanır [addPolygons](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addpolygons) Çokgen katmanı haritaya eklemek ve özelliklerini tanımlamak için eşleme sınıfının işlevi. Bir çokgenin katmanında özelliklerini görmek [PolygonLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/models.polygonlayeroptions?view=azure-iot-typescript-latest). 

## <a name="next-steps"></a>Sonraki adımlar
Eşlenir eklemek daha fazla kod örnekleri için aşağıdaki makalelere bakın:
* [Özel HTML ekleme](./map-add-custom-html.md)
* [Arama sonuçlarını göster](./map-search-location.md)


