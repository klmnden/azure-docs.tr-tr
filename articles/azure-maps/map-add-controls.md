---
title: Azure haritalar eşleme denetimleri ekleme | Microsoft Docs
description: Yakınlaştırma Denetimi, aralık denetimi, döndürme denetimi ve bir stil Seçici Azure haritalar eşlemesinde ekleme.
author: walsehgal
ms.author: v-musehg
ms.date: 10/08/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: c1f5dd329f34d64478d605c21d229d8c75a99300
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62108728"
---
# <a name="add-map-controls-to-azure-maps"></a>Azure haritalar için eşleme denetimleri ekleme

Bu makalede bir eşlemesi için eşleme denetimleri ekleme gösterilmektedir. Ayrıca tüm denetimleri ile bir harita oluşturmak nasıl öğrenin ve [stil seçiciyi](https://docs.microsoft.com/azure/azure-maps/choose-map-style).

## <a name="add-zoom-control"></a>Yakınlaştırma denetimi ekleme

<iframe height='500' scrolling='no' title='Yakınlaştırma denetimi ekleme' src='//codepen.io/azuremaps/embed/WKOQyN/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/WKOQyN/'>yakınlaştırma denetimi ekleme</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

İlk kod bloğu abonelik anahtarını ayarlar ve stili önceden ayarlamadan bir harita oluşturur. Bkz: [bir harita oluşturmak](./map-create.md) bir harita oluşturmak yönergeler.

Yakınlaştırma denetimi içine ve dışına harita yakınlaştırma olanağı ekler. İkinci kod bloğu atlas kullanarak bir yakınlaştırma denetimi oluşturur [ZoomControl](/javascript/api/azure-maps-control/atlas.control.zoomcontrol) ve haritanın kullanarak haritaya ekler [controls.add](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) yöntemi. Yakınlaştırma denetimi içinde haritasıdır **olay dinleyicisi** yükler harita tamamen yüklendikten sonra emin olmak için.

## <a name="add-pitch-control"></a>Aralık denetimi ekleme

<iframe height='500' scrolling='no' title='Aralık denetimi ekleme' src='//codepen.io/azuremaps/embed/xJrwaP/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/xJrwaP/'>aralık denetim ekleme</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

İlk kod bloğu abonelik anahtarını ayarlar ve stili önceden ayarlamadan bir harita oluşturur. Bkz: [bir harita oluşturmak](./map-create.md) bir harita oluşturmak yönergeler.

Aralık denetimi eşleme aralığını değiştirme özelliği ekler. İkinci kod bloğunu atlas kullanarak bir aralık denetimi oluşturur [PitchControl](/javascript/api/azure-maps-control/atlas.control.pitchcontrol) ve haritanın kullanarak haritaya ekler [controls.add](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) yöntemi. Aralık denetimi içinde haritasıdır **olay dinleyicisi** yükler harita tamamen yüklendikten sonra emin olmak için.

## <a name="add-compass-control"></a>Pusula denetim ekleme

<iframe height='500' scrolling='no' title='Döndürme denetimi ekleme' src='//codepen.io/azuremaps/embed/GBEoRb/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/GBEoRb/'>döndürme denetimi ekleme</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

İlk kod bloğu abonelik anahtarını ayarlar ve stili önceden ayarlamadan bir harita oluşturur. Bkz: [bir harita oluşturmak](./map-create.md) bir harita oluşturmak yönergeler.

İkinci kod bloğunu atlas kullanarak Compass denetim nesnesi oluşturur [Compass denetim](/javascript/api/azure-maps-control/atlas.control.compasscontrol). Ayrıca haritanın kullanarak haritaya pusula denetimi ekler [controls.add](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) yöntemi. Compass denetimi içinde haritasıdır **olay dinleyicisi** yükler harita tamamen yüklendikten sonra emin olmak için.

## <a name="a-map-with-all-controls"></a>Tüm denetimleri ile bir eşleme

<iframe height='500' scrolling='no' title='Tüm denetimler sahip bir eşleme' src='//codepen.io/azuremaps/embed/qyjbOM/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/qyjbOM/'>sahip tüm denetimler bir eşleme</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

İlk kod bloğu abonelik anahtarını ayarlar ve stili önceden ayarlamadan bir harita oluşturur. Bkz: [bir harita oluşturmak](./map-create.md) bir harita oluşturmak yönergeler.

İkinci kod bloğu atlas kullanarak Compass denetim nesnesi oluşturur [CompassControl](/javascript/api/azure-maps-control/atlas.control.compasscontrol) ve haritanın kullanarak haritaya ekler [controls.add](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) yöntemi.

Üçüncü kod bloğunu atlas kullanarak bir yakınlaştırma denetimi oluşturur [ZoomControl](/javascript/api/azure-maps-control/atlas.control.zoomcontrol) ve haritanın kullanarak haritaya ekler [controls.add](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) yöntemi.

Dördüncü kod bloğu atlas kullanarak bir aralık denetimi oluşturur [PitchControl](/javascript/api/azure-maps-control/atlas.control.pitchcontrol) ve haritanın kullanarak haritaya ekler [controls.add](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) yöntemi.

Son kod bloğunu atlas kullanarak bir stil Seçici nesnesi oluşturur. [StyleControl](/javascript/api/azure-maps-control/atlas.control.stylecontrol) ve haritanın kullanarak haritaya ekler [controls.add](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) yöntemi. Harita içinde eklenen tüm denetim nesneleri **olay dinleyicisi** harita tamamen yüklendikten sonra Yük emin olmak için.

Betikteki denetim nesnelerin sırasını harita üzerinde görünme sırasını belirler. Harita üzerinde denetimlerin sırasını değiştirmek için komut dosyasında kendi sırasını değiştirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan yöntemleri ve sınıfları hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Harita](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [Atlas](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas?view=azure-iot-typescript-latest)

Tam kod için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Pin ekleme](./map-add-pin.md)

> [!div class="nextstepaction"]
> [Açılan pencere Ekle](./map-add-popup.md)
