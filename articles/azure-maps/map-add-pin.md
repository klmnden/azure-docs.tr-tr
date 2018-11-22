---
title: Bir simge eklemek ve işaretçileri olan Azure haritalar | Microsoft Docs
description: Simgeler ve işaretçileri için bir Javascript harita ekleme
author: walsehgal
ms.author: v-musehg
ms.date: 10/30/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: c56ac35f49c364b7b0f2ad26b82b178411419414
ms.sourcegitcommit: 022cf0f3f6a227e09ea1120b09a7f4638c78b3e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2018
ms.locfileid: "52282694"
---
# <a name="add-symbols-and-markers-to-a-map"></a>Simgeler ve işaretli bir haritaya eklemek

Bu makalede, semboller ve işaretli bir veri kaynağını kullanan bir haritaya eklemek gösterilmektedir.

## <a name="add-a-symbol-marker"></a>Simgenin işaret Ekle

<iframe height='500' scrolling='no' title='Anahtar konumu Sabitle' src='//codepen.io/azuremaps/embed/ZqJjRP/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/ZqJjRP/'>anahtar konumu Sabitle</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod ilk bloğu bir harita nesnesi oluşturur. Gördüğünüz [bir harita oluşturmak](./map-create.md) yönergeler için.

İkinci kod bloğu içinde bir veri kaynağı nesnesi kullanılarak oluşturulan [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) sınıfı. Bir noktadan sonra oluşturulur ve veri kaynağına eklenir. Bir nokta bir [özellik](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature?view=azure-iot-typescript-latest) , [noktası](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest).

Üçüncü blok kod oluşturur bir [olay dinleyicisi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) ve noktanın koordinatları üzerine fare tıklatın şeklin sınıfını kullanarak güncelleştirmeleri [setCoordinates](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest#setcoordinates) yöntemi.

A [sembol katman](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest) sarmalanmış noktası tabanlı veri işleme için metin veya simge kullanan [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) harita üzerinde simgeler olarak.  Veri kaynağı, click olay dinleyicisi ve sembol katmanı oluşturulur ve eşlemesine eklenen [olay dinleyicisi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) işlev eşlemesi tam olarak yüklendikten sonra noktası görüntülendiğinden emin olun.

## <a name="add-a-custom-symbol"></a>Özel bir simge Ekle

<iframe height='500' scrolling='no' title='HTML veri kaynağı' src='//codepen.io/azuremaps/embed/qJVgMx/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/qJVgMx/'>HTML veri kaynağı</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod, kod bloğunun ilk harita nesnesi oluşturur. Gördüğünüz [bir harita oluşturmak](./map-create.md) yönergeler için.

İkinci kod bloğunu ekler bir [HtmlMarker](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarker?view=azure-iot-typescript-latest) harita kullanarak [işaretçileri](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#markers) özelliği [harita](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) sınıfı. HtmlMarker içinde eşlemesine eklenir [olay dinleyicisi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) harita tamamen yüklendikten sonra görüntülendiğinden emin olmak için işlevi.

## <a name="add-bubble-markers"></a>Kabarcık işaretçileri Ekle

<iframe height='500' scrolling='no' title='BubbleLayer veri kaynağı' src='//codepen.io/azuremaps/embed/mzqaKB/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/mzqaKB/'>BubbleLayer DataSource</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod, kod bloğunun ilk harita nesnesi oluşturur. Gördüğünüz [bir harita oluşturmak](./map-create.md) yönergeler için.

İkinci kod bloğu içinde bir dizi konumları tanımlanır ve [MultiPoint](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.multipoint?view=azure-iot-typescript-latest) nesnesi oluşturulur. Kullanarak bir veri kaynağı nesnesi oluşturulur [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) nesne sınıfı ve MultiPoint veri kaynağına eklenir.

A [Kabarcık katman](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.bubblelayer?view=azure-iot-typescript-latest) sarmalanmış noktası tabanlı veri işler [veri kaynağı](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) haritada daireler. Son kod bloğunu Kabarcık katman oluşturur ve onu haritaya ekler. Kabarcık katmanında özelliklerini görmek [BubbleLayerOptions](/javascript/api/azure-maps-control/atlas.bubblelayeroptions).

MultiPoint nesne veri kaynağı ve kabarcık katman oluşturulur ve eşlemesine eklenen [olay dinleyicisi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) işlev eşlemesi tam olarak yüklendikten sonra dairenin görüntülendiğinden emin olun.

## <a name="add-bubble-markers-with-label"></a>Kabarcık işaretçileri etiketi Ekle

<iframe height='500' scrolling='no' title='Çok katmanlı bir veri kaynağı' src='//codepen.io/azuremaps/embed/rqbQXy/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/rqbQXy/'>çok katmanlı bir veri kaynağı</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod haritada nasıl görselleştirebileceğinizi ve etiket verilerini gösterir. Yukarıdaki kod ilk bloğu bir harita nesnesi oluşturur. Gördüğünüz [bir harita oluşturmak](./map-create.md) yönergeler için.

İkinci kod bloğu oluşturur bir [noktası](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest) nesne. Ardından bir veri kaynağı nesnesi kullanılarak oluşturur [veri kaynağı](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) sınıfı ve veri kaynağına noktası ekler.

A [Kabarcık katman](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.bubblelayer?view=azure-iot-typescript-latest) sarmalanmış noktası tabanlı veri işler [veri kaynağı](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) haritada daireler. Üçüncü kod bloğunu Kabarcık katman oluşturur ve onu haritaya ekler. Kabarcık katmanında özelliklerini görmek [BubbleLayerOptions](/javascript/api/azure-maps-control/atlas.bubblelayeroptions).

A [sembol katman](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest) sarmalanmış noktası tabanlı veri işleme için metin veya simge kullanan [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) harita üzerinde simgeler olarak. Son kod bloğunu oluşturur ve kabarcık metin etiketini işler eşlemesine bir sembol katmanı ekler. Sembol katmanında özelliklerini görmek [SymbolLayerOptions](/javascript/api/azure-maps-control/atlas.symbollayeroptions).

Veri kaynağı katmanları oluşturulur ve eşlemesine eklenen [olay dinleyicisi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) işlev eşlemesi tam olarak yüklendikten sonra veri görüntülendiğinden emin olun.


## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan yöntemleri ve sınıfları hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Harita](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

Daha fazla kod örneği, eşlenir eklemek için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Açılan pencere Ekle](./map-add-popup.md)

> [!div class="nextstepaction"]
> [Şekil ekleme](./map-add-shape.md)