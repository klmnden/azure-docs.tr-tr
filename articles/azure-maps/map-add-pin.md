---
title: Azure haritalar için Sembol katman ekleyin | Microsoft Docs
description: Javascript harita için simgeler ekleme
author: rbrundritt
ms.author: richbrun
ms.date: 12/2/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 2580f1177bf9e6e3a92934f88a5d8ab51894e8d9
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59269495"
---
# <a name="add-a-symbol-layer-to-a-map"></a>Bir sembol katmanı haritaya eklemek

Bu makalede, bir harita üzerinde bir sembol katmanı olarak bir veri kaynağından veri noktası nasıl işleyebilen gösterilmektedir. Sembol katmanları WebGL kullanılarak işlenir ve noktaları HTML işaretçileri daha çok daha büyük kümeleri destekleyen ancak geleneksel CSS ve HTML öğeleri için stil desteklemez.  

> [!TIP]
> Sembol katmanları varsayılan olarak, bir veri kaynağındaki tüm geometriler koordinatlarını işlemez. Özellikleri ayarlama katmanı yalnızca noktası geometri işler gibi sınırlamak için `filter` katmana özelliği `['==', ['geometry-type'], 'Point']` veya `['any', ['==', ['geometry-type'], 'Point'], ['==', ['geometry-type'], 'MultiPoint']]` MultiPoint özellikleri de dahil etmek istiyorsanız.

## <a name="add-a-symbol-layer"></a>Sembol katmanı ekleme

<iframe height='500' scrolling='no' title='Anahtar konumu Sabitle' src='//codepen.io/azuremaps/embed/ZqJjRP/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/ZqJjRP/'>anahtar konumu Sabitle</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod ilk bloğu bir harita nesnesi oluşturur. Gördüğünüz [bir harita oluşturmak](./map-create.md) yönergeler için.

İkinci kod bloğu içinde bir veri kaynağı nesnesi kullanılarak oluşturulan [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) sınıfı. İçeren bir [özellik] bir [noktası](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest) ile geometri kaydırılır [şekli](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest) güncelleştirmek kolaylaştırmak için sınıf sonra oluşturulan ve veri kaynağına eklendi.

Üçüncü blok kod oluşturur bir [olay dinleyicisi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) ve noktanın koordinatları üzerine fare tıklatın şeklin sınıfını kullanarak güncelleştirmeleri [setCoordinates](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest) yöntemi.

A [sembol katman](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest) sarmalanmış noktası tabanlı veri işleme için metin veya simge kullanan [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) harita üzerinde simgeler olarak.  Veri kaynağı, click olay dinleyicisi ve sembol katmanı oluşturulur ve eşlemesine eklenen `ready` [olay dinleyicisi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) noktası yüklenir ve erişilecek hazır sonra eşleme görüntülendiğinden emin olmak için işlevi.

> [!TIP]
> Varsayılan olarak, performans için çakışan sembolleri gizleyerek sembol katmanları simgeleri işleme iyileştirin. Gizli sembolleri yakınlaştırma gibi görünebilir. Bu özellik devre dışı bırakın ve her zaman tüm sembolleri işlemek için ayarlanmış `allowOverlap` özelliği `iconOptions` seçenekleri `true`.

## <a name="add-a-custom-icon-to-a-symbol-layer"></a>Özel bir simge için simge katman ekleyin

Sembol katmanları, WebGL kullanılarak işlenir. Simge görüntüleri gibi tüm bu kaynaklar olarak WebGL bağlamına yüklü olması gerekir. Bu örnek, kaynak eşleme için özel bir simge eklemek ve harita üzerinde özel bir simge ile veri noktası oluşturmak için kullanmak nasıl gösterir. `textField` Sembol katmanın özelliği belirtilmesi bir ifade gerektirir. Bu durumda, sıcaklık özelliği oluşturmak istiyoruz, ancak bir sayı olduğundan, bir dizeye dönüştürülecek gerekiyor. Buna ek olarak "° F" eklenecek istiyoruz. Bir ifade, bunu yapmak için kullanılabilir; `['concat', ['to-string', ['get', 'temperature']], '°F']`. 

<br/>

<iframe height='500' scrolling='no' title='Özel simge görüntü simgesi' src='//codepen.io/azuremaps/embed/WYWRWZ/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/WYWRWZ/'>özel simge görüntü simgesi</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="customize-a-symbol-layer"></a>Bir sembol katmanı özelleştirme 

Sembol katmanı, birçok stil seçenekler sunar. Buraya çeşitli stil seçeneklerini göz test etmek için bir araçtır.

<br/>

<iframe height='700' scrolling='no' title='Sembol Katman Seçenekleri' src='//codepen.io/azuremaps/embed/PxVXje/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/PxVXje/'>simge katmanlarını</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan yöntemleri ve sınıfları hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [SymbolLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [SymbolLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.symbollayeroptions?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [IconOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.iconoptions?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [TexTOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.textoptions?view=azure-iot-typescript-latest)

Daha fazla kod örneği, eşlenir eklemek için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Açılır pencere ekleme](map-add-popup.md)

> [!div class="nextstepaction"]
> [Veri odaklı stili ifadeleri kullanma](data-driven-style-expressions-web-sdk.md)

> [!div class="nextstepaction"]
> [Şekil ekleme](map-add-shape.md)

> [!div class="nextstepaction"]
> [Baloncuk katmanı ekleme](map-add-bubble-layer.md)

> [!div class="nextstepaction"]
> [HTML oluşturucular ekleme](map-add-bubble-layer.md)
