---
title: Azure haritalar eşleme denetimleri ekleme | Microsoft Docs
description: Yakınlaştırma Denetimi, aralık denetimi, döndürme denetimi ve bir stil Seçici Azure haritalar eşlemesinde ekleme.
author: walsehgal
ms.author: v-musehg
ms.date: 08/29/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: ddbb621561d8fcef3637e036be004d17aea76f40
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46129310"
---
# <a name="add-map-controls-to-azure-maps"></a>Azure haritalar için eşleme denetimleri ekleme

Bu makalede bir eşlemesi için eşleme denetimleri ekleme gösterilmektedir. Ayrıca tüm denetimleri ile bir harita oluşturmak nasıl öğrenin ve [stil seçiciyi](https://docs.microsoft.com/azure/azure-maps/choose-map-style#adding-the-style-picker).

## <a name="add-zoom-control"></a>Yakınlaştırma denetimi ekleme

<iframe height='500' scrolling='no' title='Yakınlaştırma denetimi ekleme' src='//codepen.io/azuremaps/embed/WKOQyN/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/WKOQyN/'>yakınlaştırma denetimi ekleme</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod ilk kod bloğunda bir harita nesnesi oluşturur. Bkz: [bir harita oluşturmak](./map-create.md) bir harita oluşturmak yönergeler.

İkinci kod bloğu atlas kullanarak bir yakınlaştırma denetimi oluşturur [ZoomControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.zoomcontrol?view=azure-iot-typescript-latest).

Yakınlaştırma denetimi içine ve dışına harita yakınlaştırma olanağı ekler. Üçüncü blok haritanın kullanarak haritaya yakınlaştırma denetimi ekler [addControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addcontrol) yöntemi.

## <a name="add-pitch-control"></a>Aralık denetimi ekleme

<iframe height='500' scrolling='no' title='Aralık denetimi ekleme' src='//codepen.io/azuremaps/embed/xJrwaP/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/xJrwaP/'>aralık denetim ekleme</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod ilk bloğu, stil için gri tonlamalı kümesi ile bir harita nesnesi oluşturur. Bkz: [bir harita oluşturmak](./map-create.md) bir harita oluşturmak yönergeler.

İkinci kod bloğu atlas kullanarak bir aralık denetimi oluşturur [PitchControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.pitchcontrol?view=azure-iot-typescript-latest).

Aralık denetimi eşleme aralığını değiştirme özelliği ekler. Üçüncü blok aralığı denetimi kullanarak haritanın harita ekler [addControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addcontrol) yöntemi.

## <a name="add-compass-control"></a>Pusula denetim ekleme

<iframe height='500' scrolling='no' title='Döndürme denetimi ekleme' src='//codepen.io/azuremaps/embed/GBEoRb/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/GBEoRb/'>döndürme denetimi ekleme</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod ilk kod bloğunda bir harita nesnesi oluşturur. Bkz: [bir harita oluşturmak](./map-create.md) bir harita oluşturmak yönergeler.

İkinci kod bloğunu atlas kullanarak Compass denetim nesnesi oluşturur [Compass denetim](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.compasscontrol?view=azure-iot-typescript-latest#compasscontrol). Ayrıca haritanın kullanarak haritaya pusula denetimi ekler [addControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addcontrol) yöntemi.

## <a name="a-map-with-all-controls"></a>Tüm denetimleri ile bir eşleme

<iframe height='500' scrolling='no' title='Tüm denetimler sahip bir eşleme' src='//codepen.io/azuremaps/embed/qyjbOM/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/qyjbOM/'>sahip tüm denetimler bir eşleme</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod ilk kod bloğunda bir harita nesnesi oluşturur. Bkz: [bir harita oluşturmak](./map-create.md) bir harita oluşturmak yönergeler.

İkinci kod bloğu atlas kullanarak Compass denetim nesnesi oluşturur [CompassControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.compasscontrol?view=azure-iot-typescript-latest#compasscontrol) ve haritanın kullanarak haritaya ekler [addControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addcontrol) yöntemi.

Üçüncü kod bloğunu atlas kullanarak bir yakınlaştırma denetimi oluşturur [ZoomControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.zoomcontrol?view=azure-iot-typescript-latest) ve haritanın kullanarak haritaya ekler [addControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addcontrol) yöntemi.

Dördüncü kod bloğu atlas kullanarak bir aralık denetimi oluşturur [PitchControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.pitchcontrol?view=azure-iot-typescript-latest) ve haritanın kullanarak haritaya ekler [addControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addcontrol) yöntemi.

Son kod bloğunu atlas kullanarak bir stil Seçici nesnesi haritaya ekler [StyleControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.stylecontrol?view=azure-iot-typescript-latest#stylecontrol) ve haritanın kullanarak haritaya ekler [addControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addcontrol) yöntemi.

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