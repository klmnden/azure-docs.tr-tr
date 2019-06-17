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
ms.openlocfilehash: f61c7a939902ee5d02b2e9ba896c7555968f9d0d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60769524"
---
# <a name="add-a-shape-to-a-map"></a>Şekil Haritası ekleme

Bu makalede geometriler ile ilgili satır, Çokgen katmanı kullanarak haritada nasıl oluşturulacağını gösterir. Azure haritalar Web SDK'sını da tanımlandığı gibi daire geometriler oluşturmayı destekler [genişletilmiş GeoJSON şema](extend-geojson.md#circle). Tüm özellik geometriler da kolayca ile sarmalanmış durumunda güncelleştirilebilir [şekli](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest) sınıfı.

<a id="addALine"></a>

## <a name="add-lines-to-the-map"></a>Haritaya satırları ekleyin

`LineString` ve `MultiLineString` özellikleri yollarını ve harita üzerinde dokümanda temsil etmek için kullanılır.

### <a name="add-a-line"></a>Bir satır ekleyin

<iframe height='500' scrolling='no' title='Bir satır için bir eşleme ekleyin' src='//codepen.io/azuremaps/embed/qomaKv/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/qomaKv/'>bir satır eklemek için bir harita</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod içindeki kod ilk bloğunu bir harita nesnesi oluşturur. Gördüğünüz [bir harita oluşturmak](./map-create.md) yönergeler için.

İkinci kod bloğu içinde bir veri kaynağı nesnesi kullanılarak oluşturulan [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) sınıfı. A [LineString](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.linestring?view=azure-iot-typescript-latest) nesne oluşturulur ve veri kaynağına eklenir.

A [LineLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.linelayer?view=azure-iot-typescript-latest) işler satır içinde sarmalanmış nesneleri [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest). Son kod bloğunu oluşturur ve bir çizgi katmanı haritaya ekler. Bir satır katmanında özelliklerini görmek [LineLayerOptions](/javascript/api/azure-maps-control/atlas.linelayeroptions?view=azure-iot-typescript-latest). Veri kaynağı ve çizgi katmanı oluşturulur ve eşlemesine eklenen [olay işleyicisi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) harita tamamen yüklendikten sonra satır görüntülendiğinden emin olmak için.

### <a name="add-symbols-along-a-line"></a>Bir çizgi simgeleri ekleme

Bu örnek, bir satırı oku simgesi haritaya eklemek gösterilmektedir. Bir simge katmanını kullanarak ayarladığınızda "satır" için "yerleştirme" seçeneği, bu çizgi simgeleri işlemek ve simgeleri Döndür (0 dereceye sağ =).

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Satır boyunca ok Göster" src="//codepen.io/azuremaps/embed/drBJwX/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Kalem bkz <a href='https://codepen.io/azuremaps/pen/drBJwX/'>satır boyunca Show ok</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

### <a name="line-stroke-gradient"></a> Bir vuruş gradyan için bir satır ekleyin

Bir satır için bir tek bir vuruş rengi uygulayabilmeniz için olmasının yanı sıra bir satır ile sonraki bir doğru parçası durumundan gösterilecek renkler bir gradyan da doldurabilirsiniz. Örneğin, satır gradyanlar değişiklikleri saat ve uzaklık veya farklı Sıcaklıkların nesnelerin bağlı satır boyunca temsil etmek için kullanılabilir. Bir satır için bu özelliği uygulamak için veri kaynağı olmalıdır `lineMetrics` seçeneğini true olarak ayarlanmış ve bir renk gradyanı ifadesi ardından geçirilebilir `strokeColor` satırı seçeneği. Gradyan fırça darbesi ifade zorunda başvuru `['line-progress']` ifade hesaplanan satırı ölçümleri gösteren veri ifadesi.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Gradyan fırça darbesi satırla" src="//codepen.io/azuremaps/embed/wZwWJZ/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Kalem bkz <a href='https://codepen.io/azuremaps/pen/wZwWJZ/'>vuruş gradyan satırla</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

### <a name="customize-a-line-layer"></a>Çizgi katmanı özelleştirme

Satır birkaç stil seçeneklerini katman. Burada, bir aracı deneyebilirsiniz.

<br/>

<iframe height='700' scrolling='no' title='Çizgi katmanı seçenekleri' src='//codepen.io/azuremaps/embed/GwLrgb/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/GwLrgb/'>satır Katman seçeneklerini</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

<a id="addAPolygon"></a>

## <a name="add-a-polygon-to-the-map"></a>Bir çokgenin haritaya eklemek

`Polygon` ve `MultiPolygon` özellikleri genellikle bir harita üzerinde bir alanı göstermek için kullanılır. 

### <a name="use-a-polygon-layer"></a>Çokgen katmanı kullan 

Çokgen katmanı Çokgen bölümünü işler. 

<iframe height='500' scrolling='no' title='Bir çokgenin bir haritaya eklemek ' src='//codepen.io/azuremaps/embed/yKbOvZ/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/yKbOvZ/'>Çokgen bir haritaya eklemek </a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod, kod bloğunun ilk harita nesnesi oluşturur. Gördüğünüz [bir harita oluşturmak](./map-create.md) yönergeler için.

İkinci kod bloğu içinde bir veri kaynağı nesnesi kullanılarak oluşturulan [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) sınıfı. A [Çokgen](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.polygon?view=azure-iot-typescript-latest) koordinatları dizisinden oluşturulur ve veri kaynağına eklendi. 

A [PolygonLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonlayer?view=azure-iot-typescript-latest) sarmalanmış veri işler [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) harita üzerinde. Son kod bloğunu oluşturur ve bir Çokgen katmanı haritaya ekler. Bir çokgenin katmanında özelliklerini görmek [PolygonLayerOptions](/javascript/api/azure-maps-control/atlas.polygonlayeroptions?view=azure-iot-typescript-latest). Veri kaynağı Çokgen katmanı oluşturulur ve eşlemesine eklenen [olay işleyicisi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) Çokgen harita tamamen yüklendikten sonra görüntülendiğinden emin olmak için.

### <a name="use-a-polygon-and-line-layer-together"></a>Çokgen ve çizgi katmanı birlikte kullanın

Bir çokgenin anahat işlemek için bir çizgi katmanı kullanılabilir. 

<iframe height='500' scrolling='no' title='Çokgen eklemek için Çokgen ve çizgi katmanı' src='//codepen.io/azuremaps/embed/aRyEPy/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/aRyEPy/'>Çokgen eklemek için Çokgen ve çizgi katmanı</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod, kod bloğunun ilk harita nesnesi oluşturur. Gördüğünüz [bir harita oluşturmak](./map-create.md) yönergeler için.

İkinci kod bloğu içinde bir veri kaynağı nesnesi kullanılarak oluşturulan [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) sınıfı. A [Çokgen](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.polygon?view=azure-iot-typescript-latest) koordinatları dizisinden oluşturulur ve veri kaynağına eklendi. 

A [PolygonLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonlayer?view=azure-iot-typescript-latest) sarmalanmış veri işler [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) harita üzerinde. Bir çokgenin katmanında özelliklerini görmek [PolygonLayerOptions](/javascript/api/azure-maps-control/atlas.polygonlayeroptions?view=azure-iot-typescript-latest). A [LineLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.linelayer?view=azure-iot-typescript-latest) satırları dizisidir. Bir satır katmanında özelliklerini görmek [LineLayerOptions](/javascript/api/azure-maps-control/atlas.linelayeroptions?view=azure-iot-typescript-latest). Çokgen ve çizgi katmanları üçüncü kod bloğu oluşturur.

Son kod bloğunu haritaya Çokgen ve çizgi katmanları ekler. Veri kaynağı katmanları oluşturulur ve eşlemesine eklenen [olay işleyicisi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) Çokgen harita tamamen yüklendikten sonra görüntülendiğinden emin olmak için.

> [!TIP]
> Varsayılan olarak çizgi katmanları çokgenler yanı sıra veri kaynağındaki satırı koordinatlarını işlenir. Özellikleri ayarlama katmanı yalnızca LineString işler gibi sınırlamak için `filter` katmana özelliği `['==', ['geometry-type'], 'LineString']` veya `['any', ['==', ['geometry-type'], 'LineString'], ['==', ['geometry-type'], 'MultiLineString']]` MultiLineString özellikleri de dahil etmek istiyorsanız.

### <a name="fill-a-polygon-with-a-pattern"></a>Bir çokgenin bir desen ile doldurun.

Bir çokgenin bir renk ile doldurma yanı sıra bir görüntü düzeni de kullanılabilir. Bir görüntü desen haritalar resim sprite kaynakları yükleyin ve ardından bu görüntü sayesinde başvuru `fillPattern` Çokgen katmanı özelliği.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Çokgenin dolgu deseni" src="//codepen.io/azuremaps/embed/JzQpYX/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Kalem bkz <a href='https://codepen.io/azuremaps/pen/JzQpYX/'>çokgenin dolgu deseni</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

### <a name="customize-a-polygon-layer"></a>Çokgen katmanı özelleştirme

Çokgen katmanı, yalnızca birkaç Stil seçeneği yoktur. Burada, bir aracı deneyebilirsiniz.

<br/>

<iframe height='700' scrolling='no' title='LXvxpg' src='//codepen.io/azuremaps/embed/LXvxpg/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/LXvxpg/'>LXvxpg</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

<a id="addACircle"></a>

## <a name="add-a-circle-to-the-map"></a>Haritayı bir daire ekleyin

Azure haritalar kullanan bir genişletilmiş belirtildiği gibi daireler için bir tanım sağlayan GeoJSON şema sürümü [burada](extend-geojson.md#circle). Oluşturarak, bir daire harita üzerinde işlenebilecek bir `Point` olan özellik bir `subType` özellik değeriyle `"Circle"` ve `radius` ölçümleri de radius temsil eden sayı olan özelliği. Örneğin:

```javascript
{
    "type": "Feature",
    "geometry": {
        "type": "Point",
        "coordinates": [-122.126986, 47.639754]
    },
    "properties": {
        "subType": "Circle",
        "radius": 100
    }
}  
```

Azure haritalar Web SDK'sı bunlar dönüştürür `Pooint` içine özellikleri `Polygon` perde özellikleri ve burada gösterildiği gibi Çokgen ve çizgi katmanları kullanarak haritada işlenebilir.

<iframe height='500' scrolling='no' title='Daire Harita Ekle' src='//codepen.io/azuremaps/embed/PRmzJX/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/PRmzJX/'>bir daire harita eklemek</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod içindeki kod ilk bloğunu bir harita nesnesi oluşturur. Gördüğünüz [bir harita oluşturmak](./map-create.md) yönergeler için.

İkinci kod bloğu içinde bir veri kaynağı nesnesi kullanılarak oluşturulan [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) sınıfı. Bir daire olduğu bir [özellik](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature?view=azure-iot-typescript-latest) , [noktası](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest) ve bir `subType` özelliğini `"Circle"` ve `radius` ölçümleri özellik değeri. Noktası özelliği ile bir `subType` , `"Circle"` eklenen bir veri kaynağı için döngüsel bir Çokgen harita içinde içine dönüştürülür.

A [PolygonLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonlayer?view=azure-iot-typescript-latest) sarmalanmış veri işler [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) harita üzerinde. Son kod bloğunu oluşturur ve bir Çokgen katmanı haritaya ekler. Bir çokgenin katmanında özelliklerini görmek [PolygonLayerOptions](/javascript/api/azure-maps-control/atlas.polygonlayeroptions?view=azure-iot-typescript-latest). Veri kaynağı Çokgen katmanı oluşturulur ve eşlemesine eklenen [olay işleyicisi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) daire harita tamamen yüklendikten sonra görüntülendiğinden emin olmak için.

## <a name="make-a-geometry-easy-to-update"></a>Geometri güncelleştirmek kolay hale getirir

A `Shape` sarar sınıfı bir [geometri](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.geometry?view=azure-iot-typescript-latest) veya [özellik](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature?view=azure-iot-typescript-latest) ve güncelleştirme ve Bakım daha kolay hale getirir.
`new Shape(data: Feature<data.Geometry, any>)` bir şekil nesnesi oluşturur ve belirtilen özellik ile başlatır.

<br/>

<iframe height='500' scrolling='no' title='Şekil özelliklerini güncelleştir' src='//codepen.io/azuremaps/embed/ZqMeQY/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/ZqMeQY/'>güncelleştirme şekli özellikleri</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod ilk bloğu bir harita nesnesi oluşturur. Gördüğünüz [bir harita oluşturmak](./map-create.md) yönergeler için.

Bir nokta bir [özellik](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature?view=azure-iot-typescript-latest) , [noktası](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest) sınıfı. İkinci kod bloğunu HTML kaydırıcı öğesinin RADIUS değeri başlatır ve ardından oluşturur ve bir noktası nesnesini saran bir [şekli](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest) sınıf nesnesi.

Üçüncü kod bloğu HTML aralık kaydırıcı öğesinden sonraki değeri alır ve Şekil sınıfı kullanarak RADIUS değerini değiştiren bir işlev oluşturur [addProperty](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest) yöntemi.

Dördüncü kod bloğu içinde bir veri kaynağı nesnesi kullanılarak oluşturulan [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) sınıfı. Noktadan sonra veri kaynağı eklenir.

A [PolygonLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonlayer?view=azure-iot-typescript-latest) sarmalanmış veri işler [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) harita üzerinde. Çokgen katmanı üçüncü kod bloğu oluşturur. Bir çokgenin katmanında özelliklerini görmek [PolygonLayerOptions](/javascript/api/azure-maps-control/atlas.polygonlayeroptions?view=azure-iot-typescript-latest). Veri kaynağı, tıklama olay işleyicisine ve Çokgen katmanı oluşturulur ve eşlemesine eklenen [olay işleyicisi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) harita tamamen yüklendikten sonra noktası görüntülendiğinden emin olmak için.

## <a name="next-steps"></a>Sonraki adımlar

Eşlenir eklemek daha fazla kod örnekleri için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Veri odaklı stili ifadeleri kullanma](data-driven-style-expressions-web-sdk.md)
