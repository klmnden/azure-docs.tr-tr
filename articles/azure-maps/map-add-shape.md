---
title: Azure Haritalar ile bir şekil eklemek | Microsoft Docs
description: Javascript harita bir şekil ekleme
author: jingjing-z
ms.author: jinzh
ms.date: 10/30/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 801cbd6367e0843e43121b3582971a437984e863
ms.sourcegitcommit: 1d3353b95e0de04d4aec2d0d6f84ec45deaaf6ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50247717"
---
# <a name="add-a-shape-to-a-map"></a>Şekil Haritası ekleme

Bu makalede nasıl ekleneceğini gösterir bir [şekli](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest) harita üzerinde var olan bir şekil Haritası ve güncelleştirme özelliklerini.

<a id="addALine"></a>

## <a name="add-a-line"></a>Bir satır ekleyin

<iframe height='500' scrolling='no' title='Bir satır için bir eşleme ekleyin' src='//codepen.io/azuremaps/embed/qomaKv/?height=534&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/qomaKv/'>bir satır eklemek için bir harita</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod içindeki kod ilk bloğunu bir harita nesnesi oluşturur. Gördüğünüz [bir harita oluşturmak](./map-create.md) yönergeler için.

İkinci kod bloğu içinde bir veri kaynağı nesnesi kullanılarak oluşturulan [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) sınıfı. Bir satır bir [özellik](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature?view=azure-iot-typescript-latest) LineString. Ardından oluşturur ve bir satıra kaydırılacağını [şekli](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest) nesne sınıfını kullanarak `new atlas.Shape(new atlas.data.Feature((new atlas.data.LineString()))` ve veri kaynağına ekler.

A [LineLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.linelayer?view=azure-iot-typescript-latest) işler satır içinde sarmalanmış nesneleri [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest). Son kod bloğunu oluşturur ve bir çizgi katmanı haritaya ekler. Bir satır katmanında özelliklerini görmek [LinestringLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/models.linestringlayeroptions?view=azure-iot-typescript-latest). Veri kaynağı ve çizgi katmanı oluşturulur ve eşlemesine eklenen [olay dinleyicisi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) işlev eşlemesi tam olarak yüklendikten sonra satırı görüntülendiğinden emin olun.

<a id="addACircle"></a>

## <a name="add-a-circle"></a>Bir daire ekleyin

<iframe height='500' scrolling='no' title='Daire Harita Ekle' src='//codepen.io/azuremaps/embed/PRmzJX/?height=538&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/PRmzJX/'>bir daire harita eklemek</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod içindeki kod ilk bloğunu bir harita nesnesi oluşturur. Gördüğünüz [bir harita oluşturmak](./map-create.md) yönergeler için.

İkinci kod bloğu içinde bir veri kaynağı nesnesi kullanılarak oluşturulan [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) sınıfı. Bir daire olduğu bir [özellik](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature?view=azure-iot-typescript-latest) , [noktası](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest). Ardından oluşturur ve bir daire sarmalar [şekli](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest) nesne sınıfını kullanarak `new atlas.Shape(new atlas.data.Feature(new atlas.data.Point()))` ve veri kaynağına ekler.

A [PolygonLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonlayer?view=azure-iot-typescript-latest) sarmalanmış veri işler [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) harita üzerinde. Son kod bloğunu oluşturur ve bir Çokgen katmanı haritaya ekler. Bir çokgenin katmanında özelliklerini görmek [PolygonLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/models.polygonlayeroptions?view=azure-iot-typescript-latest). Veri kaynağı Çokgen katmanı oluşturulur ve eşlemesine eklenen [olay dinleyicisi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) işlev eşlemesi tam olarak yüklendikten sonra dairenin görüntülendiğinden emin olun.

<a id="addAPolygon"></a>

## <a name="add-a-polygon"></a>Bir çokgenin Ekle

Bir çokgenin haritaya eklemek iki farklı yolu vardır. Her ikisi de, aşağıdaki örneklerde açıklanmıştır.

### <a name="use-polygon-layer"></a>Çokgen katmanı kullan 

<iframe height='500' scrolling='no' title='Bir çokgenin bir haritaya eklemek ' src='//codepen.io/azuremaps/embed/yKbOvZ/?height=543&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/yKbOvZ/'>Çokgen bir haritaya eklemek </a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod, kod bloğunun ilk harita nesnesi oluşturur. Gördüğünüz [bir harita oluşturmak](./map-create.md) yönergeler için.

İkinci kod bloğu içinde bir veri kaynağı nesnesi kullanılarak oluşturulan [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) sınıfı. A [Çokgen](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.polygon?view=azure-iot-typescript-latest) olduğu bir [özellik](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature?view=azure-iot-typescript-latest) , [Çokgen](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.polygon?view=azure-iot-typescript-latest) sınıfı. `new atlas.Shape(new atlas.data.Feature(new atlas.data.Polygon()))` bir çokgenin ile sıralı oluşturmak için kullanılan koordinatları içinde sarmalanmış bir [şekli](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest) sınıf nesnesi. Şekil nesnesi daha sonra veri kaynağına eklenir.

A [PolygonLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonlayer?view=azure-iot-typescript-latest) sarmalanmış veri işler [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) harita üzerinde. Son kod bloğunu oluşturur ve bir Çokgen katmanı haritaya ekler. Bir çokgenin katmanında özelliklerini görmek [PolygonLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/models.polygonlayeroptions?view=azure-iot-typescript-latest). Veri kaynağı Çokgen katmanı oluşturulur ve eşlemesine eklenen [olay dinleyicisi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) işlevini Çokgen harita tamamen yüklendikten sonra görüntülendiğinden emin olun.

### <a name="use-polygon-and-line-layer"></a>Çokgen ve çizgi katmanı kullan

<iframe height='500' scrolling='no' title='Çokgen eklemek için Çokgen ve çizgi katmanı' src='//codepen.io/azuremaps/embed/aRyEPy/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/aRyEPy/'>Çokgen eklemek için Çokgen ve çizgi katmanı</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod, kod bloğunun ilk harita nesnesi oluşturur. Gördüğünüz [bir harita oluşturmak](./map-create.md) yönergeler için.

İkinci kod bloğu içinde bir veri kaynağı nesnesi kullanılarak oluşturulan [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) sınıfı. A [Çokgen](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.polygon?view=azure-iot-typescript-latest) olduğu bir [özellik](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature?view=azure-iot-typescript-latest) , [Çokgen](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.polygon?view=azure-iot-typescript-latest). `new atlas.Shape(new atlas.data.Feature(new atlas.data.Polygon()))` bir çokgenin ile sıralı oluşturmak için kullanılan koordinatları içinde sarmalanmış bir [şekli](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest) sınıf nesnesi. Şekil nesnesi daha sonra veri kaynağına eklenir.

A [PolygonLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonlayer?view=azure-iot-typescript-latest) sarmalanmış veri işler [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) harita üzerinde. Bir çokgenin katmanında özelliklerini görmek [PolygonLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/models.polygonlayeroptions?view=azure-iot-typescript-latest). A [LineLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.linelayer?view=azure-iot-typescript-latest) satırları dizisidir. Bir satır katmanında özelliklerini görmek [LinestringLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/models.linestringlayeroptions?view=azure-iot-typescript-latest). Çokgen ve çizgi katmanları üçüncü kod bloğu oluşturur.

Son kod bloğunu haritaya Çokgen ve çizgi katmanları ekler. Veri kaynağı katmanları oluşturulur ve eşlemesine eklenen [olay dinleyicisi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) işlevini Çokgen harita tamamen yüklendikten sonra görüntülendiğinden emin olun.

## <a name="update-a-shape"></a>Bir şekli güncelleştirmek

Bir şekil sınıf sarmalar bir [geometri](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.geometry?view=azure-iot-typescript-latest) veya [özellik](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature?view=azure-iot-typescript-latest) ve güncelleştirme ve Bakım daha kolay hale getirir.
`new Shape(data: Feature<data.Geometry, any>)` bir şekil nesnesi oluşturur ve belirtilen özellik ile başlatır.

<br>

<iframe height='500' scrolling='no' title='Şekil özelliklerini güncelleştir' src='//codepen.io/azuremaps/embed/ZqMeQY/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/ZqMeQY/'>güncelleştirme şekli özellikleri</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod ilk bloğu bir harita nesnesi oluşturur. Gördüğünüz [bir harita oluşturmak](./map-create.md) yönergeler için.

Bir nokta bir [özellik](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature?view=azure-iot-typescript-latest) , [noktası](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest) sınıfı. İkinci kod bloğunu HTML kaydırıcı öğesinin RADIUS değeri başlatır ve ardından oluşturur ve bir noktası nesnesini saran bir [şekli](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest) sınıf nesnesi.

Üçüncü kod bloğu HTML aralık kaydırıcı öğesinden sonraki değeri alır ve Şekil sınıfı kullanarak RADIUS değerini değiştiren bir işlev oluşturur [addProperty](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest#addproperty) yöntemi.

Dördüncü kod bloğu içinde bir veri kaynağı nesnesi kullanılarak oluşturulan [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) sınıfı. Noktadan sonra veri kaynağı eklenir.

A [PolygonLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonlayer?view=azure-iot-typescript-latest) sarmalanmış veri işler [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) harita üzerinde. Çokgen katmanı üçüncü kod bloğu oluşturur. Bir çokgenin katmanında özelliklerini görmek [PolygonLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/models.polygonlayeroptions?view=azure-iot-typescript-latest). Veri kaynağı, click olay dinleyicisi ve Çokgen katmanı oluşturulur ve eşlemesine eklenen [olay dinleyicisi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) işlev eşlemesi tam olarak yüklendikten sonra noktası görüntülendiğinden emin olun.

## <a name="next-steps"></a>Sonraki adımlar

Eşlenir eklemek daha fazla kod örnekleri için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Özel HTML ekleme](./map-add-custom-html.md)

> [!div class="nextstepaction"]
> [Arama sonuçlarını göster](./map-search-location.md)
