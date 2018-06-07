---
title: Bir şekli Azure eşlemeleri ekleme | Microsoft Docs
description: Javascript haritaya bir şekli ekleme
author: jingjing-z
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: dec9b7289927365faa9c58522df2571db99f0494
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34599380"
---
# <a name="add-a-shape-to-a-map"></a>Bir şekli bir harita Ekle

Bu makalede, bir satır, bir daire ve Çokgen eşlemeye ekleme gösterilmektedir. 

<a id="addALine"></a>

## <a name="add-a-line"></a>Bir satır ekleyin

<iframe height='500' scrolling='no' title='Bir satır için bir harita Ekle' src='//codepen.io/azuremaps/embed/qomaKv/?height=534&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/qomaKv/'>bir satır için bir harita Ekle</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod ilk blok kod eşleme nesnesi oluşturur. Gördüğünüz [bir harita oluşturmak](./map-create.md) yönergeler için.

İkinci kod bloğunda, bir satır oluşturulur. Bir satır bir [özelliği](https://docs.microsoft.com/javascript/api/azure-maps-javascript/feature?view=azure-iot-typescript-latest) , LineString, özellik olarak LineStringProperties ile. Kullanım `new atlas.data.Feature(new atlas.data.LineString())` bir satır oluşturun ve özelliklerini tanımlamak için. 

Satırları bir dizi satır katmanıdır. Kodu son bloğunu kullanan [addLineStrings](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#addlinestrings) satır katman eşlemeye ekleyin ve satır katman özelliklerini tanımlamak için harita sınıfının işlevi. Bir satır katmanında özelliklerini görmek [LinestringLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-javascript/linestringlayeroptions?view=azure-iot-typescript-latest).

<a id="addACircle"></a>

## <a name="add-a-circle"></a>Bir daire ekleyin

<iframe height='500' scrolling='no' title='Bir daire için bir harita Ekle' src='//codepen.io/azuremaps/embed/PRmzJX/?height=538&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/PRmzJX/'>haritaya bir daire eklemek</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod ilk blok kod eşleme nesnesi oluşturur. Gördüğünüz [bir harita oluşturmak](./map-create.md) yönergeler için.

Kod ikinci bloğunda bir daire oluşturulur. Bir daire bir [özelliği](https://docs.microsoft.com/javascript/api/azure-maps-javascript/feature?view=azure-iot-typescript-latest) , [noktası](https://docs.microsoft.com/javascript/api/azure-maps-javascript/point?view=azure-iot-typescript-latest) ile [CircleProperties](https://docs.microsoft.com/javascript/api/azure-maps-javascript/circleproperties?view=azure-iot-typescript-latest) özelliği özelliği olarak. Kullanım `new atlas.data.Feature(new atlas.data.Point())` bir daire oluşturmak ve özelliklerini tanımlamak için.

Bir daire katman daireler dizisidir. Kodu son bloğunu kullanan [addCircle](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#addcircles) daire katman eşlemeye ekleyin ve daire katman özelliklerini tanımlamak için harita sınıfının işlevi. Bir daire katmanında özelliklerini görmek [CircleLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-javascript/circlelayeroptions?view=azure-iot-typescript-latest).

<a id="addAPolygon"></a>

## <a name="add-a-polygon"></a>Çokgen Ekle
<iframe height='500' scrolling='no' title='Çokgen için bir harita Ekle ' src='//codepen.io/azuremaps/embed/yKbOvZ/?height=543&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/yKbOvZ/'>çokgen için bir harita Ekle </a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod ilk blok kod eşleme nesnesi oluşturur. Gördüğünüz [bir harita oluşturmak](./map-create.md) yönergeler için.

Kod ikinci bloğunda Çokgen oluşturulur. Bir Çokgen bir [özelliği](https://docs.microsoft.com/javascript/api/azure-maps-javascript/feature?view=azure-iot-typescript-latest) , [Çokgen](https://docs.microsoft.com/javascript/api/azure-maps-javascript/polygon?view=azure-iot-typescript-latest) ile [PolygonProperties](https://docs.microsoft.com/javascript/api/azure-maps-javascript/polygonproperties?view=azure-iot-typescript-latest) özelliği özelliği olarak. Kullanım `new atlas.data.Feature(new atlas.data.Polygon())` Çokgen oluşturmak ve özelliklerini tanımlamak için. Çokgen Oluşturucusu Çokgen yolunda sıralı koordinatlarını sağlar.

Çokgen katman çokgenler dizisidir. Kodu son bloğunu kullanan [addPolygons](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#addpolygons) Çokgen katman eşlemeye ekleyin ve özelliklerini tanımlamak için harita sınıfının işlevi. Çokgen katmanında özelliklerini görmek [PolygonLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-javascript/polygonlayeroptions?view=azure-iot-typescript-latest). 

## <a name="next-steps"></a>Sonraki adımlar
Eşlemeleri eklemek daha fazla kod örnekleri için aşağıdaki makalelere bakın:
* [Özel HTML Ekle](./map-add-custom-html.md)
* [Arama sonuçlarını göster](./map-search-location.md)


