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
ms.openlocfilehash: a6c8a8aa954379036ce566a205b8cb4e97952727
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60769558"
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

Dördüncü bloğu kod oluşturur bir [açılan nesne](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest) aracılığıyla `new atlas.Popup()`. Açılan pencere konumu ve pixelOffset gibi özelliklerdir parçası [PopupOptions](/javascript/api/azure-maps-control/atlas.popupoptions). Açılan pencere Oluşturucusu veya yoluyla PopupOptions tanımlanabilir [setOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#setoptions-popupoptions-) açılan sınıfının işlevi. A `mouseover` olay dinleyicisi sembol katmanın sonra oluşturulur.

Son kod bloğu tarafından tetiklenen bir işlev oluşturur `mouseover` olay dinleyicisi. Bu içerik ve açılan özelliklerini ayarlar ve haritayı açılan pencere nesnesi ekler.

## <a name="reusing-a-popup-with-multiple-points"></a>Açılır penceresi ile birden çok nokta yeniden kullanma

Pek çok nokta vardır ve yalnızca bir kerede bir açılan pencere göstermek istersiniz, en iyi yaklaşım bir açılan pencere oluşturmak ve bunu her noktası özelliği için bir açılan pencere oluşturmak yerine yeniden oluşturmaktır. Bunu yaparak uygulama tarafından oluşturulan DOM öğe sayısını önemli ölçüde daha iyi performans sağlayan azalır. Bu örnek 3 nokta özellikleri oluşturur. Herhangi biri tıklarsanız noktası özellik içeriğe sahip bir açılır pencere görüntülenir.

<br/>

<iframe height='500' scrolling='no' title='Açılır penceresi ile birden çok PIN yeniden kullanma' src='//codepen.io/azuremaps/embed/rQbjvK/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/rQbjvK/'>açılır penceresi ile birden çok PIN yeniden</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan yöntemleri ve sınıfları hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Açılan menüsü](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [PopupOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popupoptions?view=azure-iot-typescript-latest)

Tam kod örnekleri için harika aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Sembol katmanı Ekle](./map-add-pin.md)

> [!div class="nextstepaction"]
> [Bir HTML işaretleyici Ekle](./map-add-custom-html.md)

> [!div class="nextstepaction"]
> [Şekil ekleme](./map-add-shape.md)