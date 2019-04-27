---
title: Azure haritalar noktası verilerinde kümeleme | Microsoft Docs
description: Nasıl yapılır küme noktası verilerini Web SDK'sı
author: rbrundritt
ms.author: richbrun
ms.date: 03/27/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendleton
ms.custom: codepen
ms.openlocfilehash: d4dc6f0c8fd2dff74a1997c9dca5a31abc70c03a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60795939"
---
# <a name="clustering-point-data"></a>Veri noktası kümeleme

Harita üzerinde çok sayıda veri noktanız görselleştirirken kullanılacak noktaları birbirleriyle çakışmamalıdır, harita derli toplu görünen ve görmek ve kullanmak zor hale gelir. Veri noktası kümeleme bu kullanıcı deneyimini geliştirmek için kullanılabilir. Veri noktası kümeleme birbirine yakın olan noktası verilerini birleştirmek ve onları kümelenmiş tek veri noktası olarak haritada temsil eden işlemidir. Haritayı kullanıcı yakınlaştırır gibi kümeler, tek tek veri noktalarını parçalayın.

## <a name="enabling-clustering-on-a-data-source"></a>Bir veri kaynağında kümelemeyi etkinleştirmeden

Kümeleme kolayca etkinleştirilebilir `DataSource` ayarlayarak sınıfı `cluster` seçeneği true. Ayrıca, bir kümede birleştirilecek noktalarının seçilecek piksel RADIUS kullanılarak ayarlanabilir `clusterRadius` ve yakınlaştırma düzeyi, kümeleme mantığı kullanılarak devre dışı bırakmak belirtilebilir `clusterMaxZoom` seçeneği. Bir veri kaynağında kümelemeyi etkinleştirme ilişkin bir örnek aşağıda verilmiştir.

```javascript
//Create a data source and enable clustering.
var datasource = new atlas.source.DataSource(null, {
    //Tell the data source to cluster point data.
    cluster: true,

    //The radius in pixels to cluster points together.
    clusterRadius: 45,

    //The maximium zoom level in which clustering occurs.
    //If you zoom in more than this, all points are rendered as symbols.
    clusterMaxZoom: 15 
});
```

> [!TIP]
> İki veri noktaları yere birbirine yakınsa, kümenin hiçbir zaman kullanıcı yakınlaştırır ne kadar yakın olursa olsun uzaklıkta keser mümkündür. Bunu ele almak için ayarlayabileceğiniz `clusterMaxZoom` seçenek veri kaynağının kümeleme mantığını devre dışı bırakın ve yalnızca her şeyi görüntülemek için yakınlaştırma düzeyinde belirtir.

`DataSource` Sınıfı kümelemesiyle ilgili aşağıdaki yöntemleri de vardır:

| Yöntem | Dönüş türü | Açıklama |
|--------|-------------|-------------|
| getClusterChildren(clusterId: number) | Promise&lt;özellik&lt;geometri herhangi&gt; \| şekli&gt; | Sonraki yakınlaştırma düzeyini kümede belirli alt öğeleri alır. Bu alt şekiller ve subclusters bir birleşimi olabilir. Subclusters özellikleri ClusteredProperties eşleşen özelliklere sahip olacaktır. |
| getClusterExpansionZoom(clusterId: number) | Promise&lt;numarası&gt; | Başlangıçtan kümeyi genişleterek başlatır veya parçalama yakınlaştırma düzeyi hesaplar. |
| getClusterLeaves (Lclusterıd: sayı, sınırı: sayı, offset: sayı) | Promise&lt;özellik&lt;geometri herhangi&gt; \| şekli&gt; | Bir kümedeki tüm noktalarını alır. Ayarlama `limit` noktalarının bir alt kümesi döndürür ve `offset` sayfasına noktaları üzerinden. |

## <a name="display-clusters-using-a-bubble-layer"></a>Kabarcık Katman'ı kullanarak kümeleri görüntüleme

Kabarcık katman kolayca RADIUS ölçeklendirin ve bunları kümedeki noktalarının sayısını ifade kullanarak temel rengini değiştirmek gibi kümelenmiş noktaları oluşturmak için harika bir yoludur. Kabarcık Katman'ı kullanarak kümeleri görüntülenirken kümelenmemiş veri noktaları işlemeye yönelik ayrı bir katmana de kullanmalısınız. Genellikle kabarcıkların üzerine ve kümenin boyutunu görüntülemek de kullanışlı olur. Bir sembol katman metin ve herhangi bir simge ile bu davranışı elde etmek için kullanılabilir. 

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Küme temel Kabarcık katmanı" src="//codepen.io/azuremaps/embed/qvzRZY/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Kalem bkz <a href='https://codepen.io/azuremaps/pen/qvzRZY/'>katman temel Kabarcık Kümeleme</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="display-clusters-using-a-symbol-layer"></a>Görüntü, simge katmanını kullanarak kümeleri

Veri yoğunluklu görmek istiyorsanız ancak bu istenen deneyimi olmayabilir otomatik olarak gizlenir varsayılan simge katmanını kullanarak noktası verileri görselleştirme, çakışma diğer daha net bir deneyim oluşturmak için simgeler, haritada işaret eder. Ayarı `allowOverlap` seçeneği sembol katmanların `iconOptions` özelliğini `true` bu deneyimi devre dışı bırakır, ancak görüntülenen tüm sembolleri neden olur. Kümeleme kullanarak tüm veri yoğunluklu bir temiz iyi kullanıcı deneyimi oluşturulurken görmenizi sağlar. Bu örnekte, kümeler ve tek tek veri noktalarını göstermek için özel bir simge kullanılır.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Kümelenmiş sembol katmanı" src="//codepen.io/azuremaps/embed/Wmqpzz/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Kalem bkz <a href='https://codepen.io/azuremaps/pen/Wmqpzz/'>kümelenmiş sembol katman</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="clustering-and-the-heat-maps-layer"></a>Kümeleme ve ısı haritaları katmanı

Isı haritaları yoğunluklu verilerin haritada görüntülemek için harika bir yoludur. Bu görselleştirme çok sayıda veri noktası kendi işleyebilir, ancak daha fazla veri noktaları kümelenir ve küme boyutu ısı Haritası ağırlığı kullanılırsa işleyebilir. Ayarlama `weight` ısı Haritası katmana seçeneği `['get', 'point_count']` Bunu başarmak için. Küme RADIUS küçük olduğunda, ısı Haritası kümelenmemiş veri noktalarını kullanma ısı Haritası neredeyse aynı görünür, ancak çok daha iyi sonuç verecektir. Ancak, küme RADIUS daha küçük, daha doğru ısı Haritası olması ancak küçük bir performans avantajı.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Küme ağırlıklı ısı Haritası" src="//codepen.io/azuremaps/embed/VRJrgO/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Kalem bkz <a href='https://codepen.io/azuremaps/pen/VRJrgO/'>küme ağırlıklı ısı Haritası</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="mouse-events-on-clustered-data-points"></a>Kümelenmiş verileri noktalarında fare olayları

Kümelenmiş bir veri noktasının kümelenmiş veri noktalarını içeren bir katmanda fare olaylar meydana geldiğinde, olaya bir GeoJSON noktası özelliği nesnesi olarak döndürülür. Bu nokta özelliği şu özelliklere sahip:

| Özellik adı | Tür | Açıklama |
|---------------|------|-------------|
| küme | boole | Özellik bir kümeyi temsil edip etmediğini belirtir. |
| cluster_id | string | Veri kaynağı ile kullanılabilecek kümesi için benzersiz bir kimlik `getClusterExpansionZoom`, `getClusterChildren`, ve `getClusterLeaves` yöntemleri. |
| point_count | number | Kümeyi içeren nokta sayısı. |
| point_count_abbreviated | string | Uzun olursa kısaltmasıdır point_count değeri bir dize. (örneğin, 4 K 4.000 olur) |

Bu örnekte, küme noktaları oluşturur ve bir tıklama olayı ekler Kabarcık katmanındaki alır, tetiklendiğinde, hesaplama ve harita yakınlaştırma, küme bozar parçalayın kullanarak bir sonraki yakınlaştırma düzeyi `getClusterExpansionZoom` yöntemi `DataSource` sınıfı ve `cluster_id` özelliği tıklandı kümelenmiş veri noktası. 

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Küme getClusterExpansionZoom" src="//codepen.io/azuremaps/embed/moZWeV/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Kalem bkz <a href='https://codepen.io/azuremaps/pen/moZWeV/'>küme getClusterExpansionZoom</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="display-cluster-area"></a>Görüntü kümesi alanı 

Bir alan üzerinde Küme temsil eden nokta verisini yayılır. Fare bir küme, noktaları (Yapraklar) içerir dışbükey Kabuk hesaplamak için kullanılan ve alanı göstermek için haritada görüntülenen tek tek veri üzerine gelindiğinde, bu örnekte. Bir kümede yer alan tüm noktaları kullanarak veri kaynağına alınabilir `getClusterLeaves` yöntemi. Bir dizi noktaları sarmalayan bir Çokgen kullanılarak hesaplanan ve esnek bant gibi bir dışbükey Kabuk olduğu `atlas.math.getConvexHull` yöntemi.

<br/>

 <iframe height="500" style="width: 100%;" scrolling="no" title="Küme alan dışbükey Kabuk" src="//codepen.io/azuremaps/embed/QoXqWJ/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Kalem bkz <a href='https://codepen.io/azuremaps/pen/QoXqWJ/'>küme alan dışbükey Kabuk</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan yöntemleri ve sınıfları hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Veri kaynağı sınıfı](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [DataSourceOptions nesnesi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.datasourceoptions?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [Atlas.Math ad alanı](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.math?view=azure-iot-typescript-latest)

Uygulamanıza işlev eklemek için kod örnekleri bakın:

> [!div class="nextstepaction"]
> [Kabarcık katmanı Ekle](map-add-bubble-layer.md)

> [!div class="nextstepaction"]
> [Sembol katmanı Ekle](map-add-pin.md)

> [!div class="nextstepaction"]
> [Isı Haritası katman ekleyin](map-add-heat-map-layer.md)
