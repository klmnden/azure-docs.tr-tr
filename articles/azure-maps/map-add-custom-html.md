---
title: Azure haritalar için bir HTML işaretleyici Ekle | Microsoft Docs
description: Javascript harita için bir HTML işaret ekleme
author: jingjing-z
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 1c812a77429e13ea39b2f4946043c13e10aaf097
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60769677"
---
# <a name="add-html-markers-to-the-map"></a>HTML işaretçileri haritaya eklemek

Bu makalede bir HTML işaretçisi olarak eşlemesine bir görüntü dosyası gibi bir özel HTML ekleme gösterilmektedir.

> [!NOTE]
> HTML işaretçileri veri kaynaklarına bağlantı. Bunun yerine konum bilgileri işaretçiye doğrudan eklenir ve işaretin eşlenir eklenir `markers` özelliğinin bir [HtmlMarkerManager](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarkermanager?view=azure-iot-typescript-latest).

> [!IMPORTANT]
> İşleme için WebGL kullanan çoğu katmanlarında Azure haritalar Web denetimi, HTML işaretçileri işleme için geleneksel DOM öğeleri kullanın. Bu nedenle, daha fazla HTML işaretlerinin bir sayfa vardır, daha fazla DOM öğeleri eklendi. Birkaç yüz HTML işaretçileri ekledikten sonra performansı düşürebilir. Büyük veri kümeleri için verilerinizi Kümelemesi ya da sembol veya kabarcık katmanını kullanarak göz önünde bulundurun.

## <a name="add-an-html-marker"></a>Bir HTML işaretleyici Ekle

HtmlMarker sınıfın bir varsayılan stilde vardır. İşaret rengi ve metin seçenekleri ayarlayarak işaret özelleştirebilirsiniz. Varsayılan Stil HtmlMarker sınıfın rengi ve metin yer tutucu sahip bir SVG şablonudur. Hızlı bir özelleştirme HtmlMarker seçeneklerinde rengi ve metin özelliklerini ayarlayın. 

<br/>

<iframe height='500' scrolling='no' title='Bir HTML işaret Haritası Ekle' src='//codepen.io/azuremaps/embed/MVoeVw/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/MVoeVw/'>bir HTML işaret Haritası Ekle</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod, kod bloğunun ilk harita nesnesi oluşturur. Gördüğünüz [bir harita oluşturmak](./map-create.md) yönergeler için.

İkinci kod bloğunu ekler bir [HtmlMarker](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarker?view=azure-iot-typescript-latest) harita kullanarak [işaretçileri](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#markers) özelliği [harita](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) sınıfı. HtmlMarker içinde eşlemesine eklenir [olay dinleyicisi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) harita tamamen yüklendikten sonra görüntülendiğinden emin olmak için işlevi.

## <a name="create-svg-templated-html-marker"></a>SVG şablonlu HTML işaret oluşturma

Varsayılan `htmlContent` bir yerde klasörleri SVG şablonuyla bir Html işaretçidir `{color}` ve `{text}` da. Özel SVG dize oluşturma ve ayarlama,, bir SVG aynı bu yer tutucuları ekleyin `color` ve `text` işaretinin seçenekleri bu yer tutucuları, SVG güncelleştirin.

<br/>

<iframe height='500' scrolling='no' title='HTML işaretleyiciyle özel SVG şablonu' src='//codepen.io/azuremaps/embed/LXqMWx/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/LXqMWx/'>HTML işaret özel SVG şablonuyla</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="add-a-css-styled-html-marker"></a>CSS, HTML işaret Stili Ekle

HTML işaretçilerinin avantajlarından biri, CSS kullanarak gerçekleştirilebilir birçok harika özelleştirmeleri yoktur. Bu örnekte, HTML ve yerinde ve darbelerin bıraktığı animasyonlu bir PIN oluşturmak CSS HtmlMarker içeriğini oluşur.

<br/>

<iframe height='500' scrolling='no' title='HTML veri kaynağı' src='//codepen.io/azuremaps/embed/qJVgMx/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/qJVgMx/'>HTML veri kaynağı</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="draggable-html-markers"></a>Sürüklenebilir HTML işaretçileri

Bu örnek bir HTML işaret sürüklenebilir yapmak nasıl gösterir. HTML işaretçileri Destek `drag`, `dragstart` ve `dragend` olayları.

<br/>

<iframe height='500' scrolling='no' title='Sürüklenebilir HTML işaretçisi' src='//codepen.io/azuremaps/embed/wQZoEV/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/wQZoEV/'>sürüklenebilir HTML işaret</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="add-mouse-events-to-html-markers"></a>Fare olayları HTML işaretlerine ekleyin

Bu örnekler, fare ekleme ve olaylar için bir HTML işaret sürükleyin gösterir.

<br/>

<iframe height='500' scrolling='no' title='Fare olayları HTML işaretleyicileri ekleme' src='//codepen.io/azuremaps/embed/RqOKRz/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/RqOKRz/'>fare olayları HTML işaretlerine ekleyerek</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan yöntemleri ve sınıfları hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [HtmlMarker](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarker?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [HtmlMarkerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarkeroptions?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [HtmlMarkerManager](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarkermanager?view=azure-iot-typescript-latest)

Eşlenir eklemek daha fazla kod örnekleri için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Sembol katmanı Ekle](./map-add-pin.md)

> [!div class="nextstepaction"]
> [Kabarcık katmanı Ekle](./map-add-bubble-layer.md)