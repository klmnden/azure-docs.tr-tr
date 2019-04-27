---
title: Azure haritalar için Kabarcık katman ekleyin | Microsoft Docs
description: Javascript haritaya Kabarcık katmanı ekleme
author: rbrundritt
ms.author: richbrun
ms.date: 10/30/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: f2c4c6b8655d5efb993a2dedf536000ac94328c2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60769694"
---
# <a name="add-a-bubble-layer-to-a-map"></a>Kabarcık katmanı haritaya eklemek

Bu makalede bir haritada Kabarcık katmanı olarak bir veri kaynağından veri noktası nasıl oluşturabileceği açıklanır. Kabarcık katmanları noktaları sabit piksel RADIUS ile harita üzerinde daireler olarak işleyin. 

> [!TIP]
> Kabarcık katmanları varsayılan olarak, bir veri kaynağındaki tüm geometriler koordinatlarını işlenir. Özellikleri ayarlama katmanı yalnızca noktası geometri işler gibi sınırlamak için `filter` katmana özelliği `['==', ['geometry-type'], 'Point']` veya `['any', ['==', ['geometry-type'], 'Point'], ['==', ['geometry-type'], 'MultiPoint']]` MultiPoint özellikleri de dahil etmek istiyorsanız.

## <a name="add-a-bubble-layer"></a>Baloncuk katmanı ekleme

<iframe height='500' scrolling='no' title='BubbleLayer veri kaynağı' src='//codepen.io/azuremaps/embed/mzqaKB/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/mzqaKB/'>BubbleLayer DataSource</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod, kod bloğunun ilk harita nesnesi oluşturur. Gördüğünüz [bir harita oluşturmak](./map-create.md) yönergeler için.

İkinci bloğundaki kod, bir dizi [noktası](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest) nesneleri tanımlanır ve eklenen [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) nesne.

A [Kabarcık katman](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.bubblelayer?view=azure-iot-typescript-latest) sarmalanmış noktası tabanlı veri işler [veri kaynağı](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) haritada daireler. Son kod bloğunu Kabarcık katman oluşturur ve onu haritaya ekler. Kabarcık katmanında özelliklerini görmek [BubbleLayerOptions](/javascript/api/azure-maps-control/atlas.bubblelayeroptions).

Nokta nesneleri dizisi, veri kaynağı ve kabarcık katmanı oluşturulur ve eşlemesine eklenen [olay dinleyicisi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) işlev eşlemesi tam olarak yüklendikten sonra dairenin görüntülendiğinden emin olun.

## <a name="show-labels-with-a-bubble-layer"></a>Kabarcık katman etiketlerini göster

<iframe height='500' scrolling='no' title='Çok katmanlı bir veri kaynağı' src='//codepen.io/azuremaps/embed/rqbQXy/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/rqbQXy/'>çok katmanlı bir veri kaynağı</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod haritada nasıl görselleştirebileceğinizi ve etiket verilerini gösterir. Yukarıdaki kod ilk bloğu bir harita nesnesi oluşturur. Gördüğünüz [bir harita oluşturmak](./map-create.md) yönergeler için.

İkinci kod bloğu oluşturur bir [noktası](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest) nesne. Ardından bir veri kaynağı nesnesi kullanılarak oluşturur [veri kaynağı](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) sınıfı ve veri kaynağına noktası ekler.

A [Kabarcık katman](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.bubblelayer?view=azure-iot-typescript-latest) sarmalanmış noktası tabanlı veri işler [veri kaynağı](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) haritada daireler. Üçüncü kod bloğunu Kabarcık katman oluşturur ve onu haritaya ekler. Kabarcık katmanında özelliklerini görmek [BubbleLayerOptions](/javascript/api/azure-maps-control/atlas.bubblelayeroptions).

A [sembol katman](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest) sarmalanmış noktası tabanlı veri işleme için metin veya simge kullanan [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) harita üzerinde simgeler olarak. Son kod bloğunu oluşturur ve kabarcık metin etiketini işler eşlemesine bir sembol katmanı ekler. Sembol katmanında özelliklerini görmek [SymbolLayerOptions](/javascript/api/azure-maps-control/atlas.symbollayeroptions).

Veri kaynağı katmanları oluşturulur ve eşlemesine eklenen [olay dinleyicisi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) işlev eşlemesi tam olarak yüklendikten sonra veri görüntülendiğinden emin olun.

## <a name="customize-a-bubble-layer"></a>Kabarcık katman özelleştirme

Kabarcık katman yalnızca birkaç Stil seçeneği vardır. Burada, bir aracı deneyebilirsiniz.

<br/>

<iframe height='700' scrolling='no' title='Kabarcık Katman Seçenekleri' src='//codepen.io/azuremaps/embed/eQxbGm/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/eQxbGm/'>Kabarcık Katman seçeneklerini</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan yöntemleri ve sınıfları hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [BubbleLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.bubblelayer?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [BubbleLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.bubblelayeroptions?view=azure-iot-typescript-latest)

Daha fazla kod örneği, eşlenir eklemek için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Sembol katmanı Ekle](map-add-pin.md)

> [!div class="nextstepaction"]
> [Veri odaklı stili ifadeleri kullanma](data-driven-style-expressions-web-sdk.md)