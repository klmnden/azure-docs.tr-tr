---
title: Azure Haritalar ile bir açılan pencere Ekle | Microsoft Docs
description: Javascript harita için açılır pencere ekleme
author: jingjing-z
ms.author: jinzh
ms.date: 11/09/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 0be10c155398133887fadb1fe9954068f3afb9d9
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2018
ms.locfileid: "51568123"
---
# <a name="add-a-popup-to-the-map"></a>Haritaya açılır pencere ekleme

Bu makalede bir harita noktasının bir açılır pencere ekleme gösterilmektedir.

## <a name="understand-the-code"></a>Kodu anlama

<a id="addAPopup"></a>

<iframe height='500' scrolling='no' title='Azure haritalar kullanan pop ekleme' src='//codepen.io/azuremaps/embed/MPRPvz/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/MPRPvz/'>Azure haritalar kullanan pop ekleme</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod, kod bloğunun ilk harita nesnesi oluşturur. Gördüğünüz [bir harita oluşturmak](./map-create.md) yönergeler için. Ayrıca HTML açılan içinde görüntülenecek içeriği oluşturur.

İkinci kod bloğunun bir veri kaynağı nesnesi kullanılarak oluşturur [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) sınıfı. Bir nokta bir [özellik](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature?view=azure-iot-typescript-latest) , [noktası](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest) sınıfı. Bir ad ve açıklama özellikleriyle bir noktası nesnesi daha sonra oluşturulur ve veri kaynağına eklenir.

A [sembol katman](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest) sarmalanmış noktası tabanlı veri işleme için metin veya simge kullanan [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) harita üzerinde simgeler olarak.  Sembol katman, üçüncü kod bloğu içinde oluşturulur. Veri kaynağı eşlemesine eklenen ardından sembol katmanı eklenir.

Dördüncü bloğu kod oluşturur bir [açılan nesne](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest) aracılığıyla `new atlas.Popup()`. Açılan pencere konumu ve pixelOffset gibi özelliklerdir parçası [PopupOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/models.popupoptions?view=azure-iot-typescript-latest). Açılan pencere Oluşturucusu veya yoluyla PopupOptions tanımlanabilir [setOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#setoptions-popupoptions-) açılan sınıfının işlevi. A `mouseover` olay dinleyicisi sembol katmanın sonra oluşturulur.

Son kod bloğu tarafından tetiklenen bir işlev oluşturur `mouseover` olay dinleyicisi. Bu içerik ve açılan özelliklerini ayarlar ve haritayı açılan pencere nesnesi ekler.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan yöntemleri ve sınıfları hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Harita](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [Açılan menüsü](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest)

Tam kod örnekleri için harika aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Şekil ekleme](./map-add-shape.md)

> [!div class="nextstepaction"]
> [Özel HTML ekleme](./map-add-custom-html.md)
