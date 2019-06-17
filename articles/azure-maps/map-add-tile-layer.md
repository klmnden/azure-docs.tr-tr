---
title: Azure haritalar için bir döşeme katmanı Ekle | Microsoft Docs
description: Javascript harita için bir döşeme katmanı ekleme
author: rbrundritt
ms.author: richbrun
ms.date: 12/3/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 3a773c24993d229f20df698113ff7535fea634ca
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60769235"
---
# <a name="add-a-tile-layer-to-a-map"></a>Döşeme katmanı haritaya eklemek

Bu makalede, bir döşeme katmanı haritaya nasıl bindirebilirsiniz gösterilmektedir. Döşeme katmanları görüntüleri Azure haritalar temel harita kutucukları üzerine eklemek sağlar. Azure haritalar döşeme sistemi hakkında daha fazla bilgi bulunabilir [yakınlaştırma düzeyleri ve döşeme Kılavuzu](zoom-levels-and-tile-grid.md) belgeleri.

Kutucuklarda bir sunucudan bir kutucuk. Katman yük. Bu görüntüleri önceden işlenmiş ve gibi herhangi bir görüntü döşeme katmanı anlayan bir adlandırma kuralını veya hareket halindeyken görüntüleri oluşturur dinamik bir hizmeti kullanarak bir sunucuda depolanır. Kutucuk üç farklı Azure haritalar tarafından desteklenen hizmet adlandırma kuralları vardır [TileLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.tilelayer?view=azure-iot-typescript-latest) sınıfı; 

* X, Y yakınlaştırma gösterimi - yakınlaştırma düzeyini temel alan, x sütun ve y döşeme kutucuğu satır konumu.
* Quadkey gösterimi - birleşimi x, y, yakınlaştırma bilgileri bir kutucuk için benzersiz bir tanımlayıcıdır tek bir dize değeri.
* Görüntü biçimi belirtmek için sınırlayıcı kutu - sınırlama kutusu koordinatları kullanılabilir `{west},{south},{east},{north}` yaygın olarak kullanılan tarafından [Web eşleme Hizmetleri (WMS)](https://www.opengeospatial.org/standards/wms).

> [!TIP]
> A [TileLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.tilelayer?view=azure-iot-typescript-latest) harita üzerinde büyük veri kümeleri görselleştirmek için harika bir yoludur. Yalnızca bir görüntüden bir döşeme katmanı oluşturulabilir ancak vektör verilerini de bir döşeme katmanı çok işlenebilir. Döşeme katmanı olarak vektör verilerini işleyerek harita denetiminin yalnızca temsil ettikleri vektör verilerini dosya boyutu çok küçük olabilen kutucukları yüklemesi gerekir. Bu teknik, milyonlarca satır harita üzerinde veri işlemek için gereken çoğu tarafından kullanılır.

Bir döşeme katmanı geçirilen kutucuk URL'si TileJSON kaynak veya aşağıdaki parametreleri kullanan bir kutucuk URL şablonu bir http/https URL'si olması gerekir: 

* `{x}` -x kutucuğun konumu. Ayrıca gerekir `{y}` ve `{z}`.
* `{y}` -Kutucuğu Y konumu. Ayrıca gerekir `{x}` ve `{z}`.
* `{z}` -Kutucuğu yakınlaştırma düzeyi. Ayrıca gerekir `{x}` ve `{y}`.
* `{quadkey}` -Bing Haritalar döşeme sistemi adlandırma kuralı temelinde quadkey tanımlayıcısı Döşe.
* `{bbox-epsg-3857}` -Bir sınırlama kutusu dize biçiminde `{west},{south},{east},{north}` EPSG 3857 uzamsal başvuru sistemi içinde.
* `{subdomain}` -Belirtilmişse alt etki alanı değerlerin ekleneceği bir yer tutucu.

## <a name="add-a-tile-layer"></a>Kutucuk katmanı ekleme

 Bu örnek, bir dizi kullanan x, y, yakınlaştırma döşeme sistemi kutucukla için işaret eden bir döşeme katmanı oluşturma işlemi gösterilmektedir. Gelen bir hava durumu radar katmana bu döşeme katmanı kaynağıdır [Iowa ortam Mesonet, Iowa State Üniversitesi](https://mesonet.agron.iastate.edu/ogc/).

<br/>

<iframe height='500' scrolling='no' title='Döşeme katmanı kullanarak X, Y ve Z' src='//codepen.io/azuremaps/embed/BGEQjG/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/BGEQjG/'>döşeme katmanı kullanarak X, Y ve Z</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod, kod bloğunun ilk harita nesnesi oluşturur. Gördüğünüz [bir harita oluşturmak](./map-create.md) yönergeler için.

İkinci kod bloğu içinde bir [TileLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.tilelayer?view=azure-iot-typescript-latest) biçimlendirilmiş bir URL bir kutucuk hizmeti, döşeme boyutunu ve yarı saydam yapmak için bir opaklık geçirerek oluşturulur. Buna ek olarak, döşeme katmanı haritaya eklerken aşağıdaki eklendikten `labels` etiketlerin yine de açıkça görünür, böylece katman.

## <a name="customize-a-tile-layer"></a>Döşeme katmanı özelleştirme

Döşeme katmanı yalnızca çok sayıda bir stil seçeneği vardır. Burada, bir aracı deneyebilirsiniz.

<br/>

<iframe height='700' scrolling='no' title='Döşeme katmanı özelliklerini' src='//codepen.io/azuremaps/embed/xQeRWX/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/xQeRWX/'>Kutucuğuna Katman seçeneklerini</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan yöntemleri ve sınıfları hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [TileLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.tilelayer?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [TileLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.tilelayeroptions?view=azure-iot-typescript-latest)

Daha fazla kod örneği, eşlenir eklemek için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Görüntü katmanı Ekle](./map-add-image-layer.md)
