---
title: Azure haritalar için bir görüntü katmanı ekleme | Microsoft Docs
description: Javascript harita için bir görüntü katmanı ekleme
author: rbrundritt
ms.author: richbrun
ms.date: 12/3/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 5396fefca3a60dea7a503f8b4e84cc575753ea30
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60769575"
---
# <a name="add-an-image-layer-to-a-map"></a>Bir görüntü katmanı haritaya eklemek

Bu makalede, harita koordinatlarına sabit kümesi görüntüye nasıl bindirebilirsiniz gösterilmektedir. İçinde harita üzerinde bir görüntü planlamanızda yapılır birçok senaryo vardır. Genellikle eşlemeleri yayılan görüntü türleri bazı örnekleri aşağıda verilmiştir;

* Dronlarla hayat kurtarma yakalanan görüntü.
* Yapı kat.
* Geçmişe yönelik veya diğer özel Harita görüntüler.
* İş sahalarında planlar.
* Hava durumu radar görüntüler.

> [!TIP]
> Bir [ImageLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.imagelayer?view=azure-iot-typescript-latest) hızlı bir harita üzerindeki resmin kaplamak için kolay bir yoludur. Ancak, görüntünün büyükse, yüklemek tarayıcı uğraşıyor. Bu durumda, görüntünüzü döşemesine ayırma ve bunları Haritası olarak yükleme göz önünde bir [TileLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.tilelayer?view=azure-iot-typescript-latest).

## <a name="add-an-image-layer"></a>Görüntü katmanı ekleme

Bu örnek nasıl görüntüsü kaplama gösterir bir [1922, Newark New Jersey'deki eşlemesinden](https://www.lib.utexas.edu/maps/historical/newark_nj_1922.jpg) harita üzerinde.

<br/>

<iframe height='500' scrolling='no' title='Basit görüntü katmanı' src='//codepen.io/azuremaps/embed/eQodRo/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/eQodRo/'>basit görüntü katmanı</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod, kod bloğunun ilk harita nesnesi oluşturur. Gördüğünüz [bir harita oluşturmak](./map-create.md) yönergeler için.

İkinci kod bloğu içinde bir [ImageLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.imagelayer?view=azure-iot-typescript-latest) bir URL bir görüntü ve dört bir yanından biçimde koordinatları iletilmesiyle oluşturulur `[Top Left Corner, Top Right Corner, Bottom Right Corner, Bottom Left Corner]`.

## <a name="import-a-kml-ground-overlay"></a>KML zemin kaplama içeri aktarma

Bu örnek, bir görüntü katmanı olarak KML zemin kaplama bilgileri haritaya kaplama gösterilmektedir. KML çığır katmanları Kuzey, Güney, Doğu ve Batı koordinatları ve burada görüntü olarak katman koordinatları görüntünün her köşe için bekliyor yönünün döndürme, sağlar. Bu örnekte KML zemin kaplama cathedral ve gelen kaynaklı Chartres değil [Wikimedia](https://commons.wikimedia.org/wiki/File:Chartres.svg/overlay.kml).

<br/>

<iframe height='500' scrolling='no' title='Görüntü katmanı olarak KML zemin kaplama' src='//codepen.io/azuremaps/embed/EOJgpj/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/EOJgpj/'>KML plan görüntü katmanı olarak katmana</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kodu statik kullanan `getCoordinatesFromEdges` işlevi [ImageLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.imagelayer?view=azure-iot-typescript-latest) Kuzey görüntüden dört köşelerini hesaplamak için sınıf, Güney, Doğu Batı ve döndürme bilgilerini KML katmana raporlar.


## <a name="customize-an-image-layer"></a>Görüntü katmanı özelleştirme

Görüntü katmanı, birçok stil seçeneği yoktur. Burada, bir aracı deneyebilirsiniz.

<br/>

<iframe height='700' scrolling='no' title='Görüntü katmanı seçenekleri' src='//codepen.io/azuremaps/embed/RqOGzx/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/RqOGzx/'>görüntü katmanlarını</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan yöntemleri ve sınıfları hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [ImageLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.imagelayer?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [ImageLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.imagelayeroptions?view=azure-iot-typescript-latest)

Daha fazla kod örneği, eşlenir eklemek için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Döşeme katmanı Ekle](./map-add-tile-layer.md)