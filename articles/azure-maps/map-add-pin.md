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
ms.openlocfilehash: a4d1a54e94b3228c64352bf08cd8cc69820a5e2d
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58500058"
---
# <a name="add-a-symbol-layer-to-a-map"></a>Bir sembol katmanı haritaya eklemek

Bu makalede, bir harita üzerinde bir sembol katmanı olarak bir veri kaynağından veri noktası nasıl işleyebilen gösterilmektedir. Sembol katmanları WebGL kullanılarak işlenir ve HTML işaretçileri daha önemli ölçüde daha fazla veri noktası destekler, ancak geleneksel CSS ve HTML öğeleri için stil desteklemez.  

> [!TIP]
> Sembol katmanları varsayılan olarak, bir veri kaynağındaki tüm geometriler koordinatlarını işlemez. Özellikleri ayarlama katmanı yalnızca noktası geometri işler gibi sınırlamak için `filter` katmana özelliği `['==', '$type', 'Point']`

## <a name="add-a-symbol-layer"></a>Sembol katmanı ekleme

<iframe height='500' scrolling='no' title='Anahtar konumu Sabitle' src='//codepen.io/azuremaps/embed/ZqJjRP/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/ZqJjRP/'>anahtar konumu Sabitle</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod ilk bloğu bir harita nesnesi oluşturur. Gördüğünüz [bir harita oluşturmak](./map-create.md) yönergeler için.

İkinci kod bloğu içinde bir veri kaynağı nesnesi kullanılarak oluşturulan [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) sınıfı. İçeren bir [özellik] bir [noktası](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest) ile geometri kaydırılır [şekli](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest) güncelleştirmek kolaylaştırmak için sınıf sonra oluşturulan ve veri kaynağına eklendi.

Üçüncü blok kod oluşturur bir [olay dinleyicisi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) ve noktanın koordinatları üzerine fare tıklatın şeklin sınıfını kullanarak güncelleştirmeleri [setCoordinates](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest) yöntemi.

A [sembol katman](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest) sarmalanmış noktası tabanlı veri işleme için metin veya simge kullanan [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) harita üzerinde simgeler olarak.  Veri kaynağı, click olay dinleyicisi ve sembol katmanı oluşturulur ve eşlemesine eklenen [olay dinleyicisi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) işlev eşlemesi tam olarak yüklendikten sonra noktası görüntülendiğinden emin olun.

> [!TIP]
> Varsayılan olarak, performans için çakışan sembolleri gizleyerek sembol katmanları simgeleri işleme iyileştirin. Gizli sembolleri yakınlaştırma gibi görünebilir. Bu özellik devre dışı bırakın ve her zaman tüm sembolleri işlemek için ayarlanmış `allowOverlap` özelliği `iconOptions` seçenekleri `true`.

## <a name="add-a-custom-icon-to-a-symbol-layer"></a>Özel bir simge için simge katman ekleyin

Sembol katmanları, WebGL kullanılarak işlenir. Simge görüntüleri gibi tüm bu kaynaklar olarak WebGL bağlamına yüklü olması gerekir. Bu örnek bir özel sembol simgesi eşleme kaynakları ekleme ve harita üzerinde özel bir simge ile veri noktası oluşturmak için kullanın gösterir. `textField` Sembol katmanın özelliği belirtilmesi bir ifade gerektirir. Bu durumda, sıcaklık özelliği noktası özelliğinin metin değeri olarak işlenecek istiyoruz. Bu ifade ile sağlanabilir: `['get', 'temperature']`. 

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
> [Açılan pencere Ekle](./map-add-popup.md)

> [!div class="nextstepaction"]
> [Şekil ekleme](./map-add-shape.md)

> [!div class="nextstepaction"]
> [Kabarcık katmanı Ekle](./map-add-bubble-layer.md)

> [!div class="nextstepaction"]
> [HTML oluşturucular ekleme](./map-add-bubble-layer.md)
