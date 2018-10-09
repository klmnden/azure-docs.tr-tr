---
title: Azure haritalar stili planlanabilecek harita | Microsoft Docs
description: Azure haritalar hakkında bilgi edinin stili ilgili işlevler.
author: walsehgal
ms.author: v-musehg
ms.date: 10/05/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: d0614f55b9666f6d5d7e95529fd78fdf1c19e615
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2018
ms.locfileid: "48857627"
---
# <a name="choose-a-map-style-in-azure-maps"></a>Azure haritalar harita stil seçin

Azure haritalar seçebileceğiniz dört farklı eşlemeler stilleri sahiptir. Eşleme stilleri hakkında daha fazla bilgi için bkz. [eşleme stilleri Azure eşlemelerinde desteklenen](./supported-map-styles.md). Bu makale stil harita üzerindeki yükü ayarlamak, yeni bir stil ve stil seçici denetimi kullanmak için stil işlevlerini kullanma.

## <a name="set-style-on-map-load"></a>Harita yükleme kümesi stili

<iframe height='500' scrolling='no' title='Harita üzerindeki yükü stili ayarlanıyor' src='//codepen.io/azuremaps/embed/WKOQRq/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/WKOQRq/'>harita üzerindeki yükü stili ayarlanıyor</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod bloğu abonelik anahtarını ayarlar ve stil gri tonlamaya kümesi bir harita nesnesi oluşturur. Bkz: [bir harita oluşturmak](./map-create.md) bir harita oluşturmak yönergeler.

## <a name="update-the-style"></a>Güncelleştirme stili

<iframe height='500' scrolling='no' title='Stil güncelleştiriliyor' src='//codepen.io/azuremaps/embed/yqXYzY/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/yqXYzY/'>stili güncelleştirme</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod bloğu abonelik anahtarını ayarlar ve stili önceden ayarlamadan bir harita oluşturur. Bkz: [bir harita oluşturmak](./map-create.md) bir harita oluşturmak yönergeler.

Haritanın ikinci kod saati kullanır [setStyle](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setstyle) harita stili için uydu ayarlamak için yöntemi.

## <a name="add-the-style-picker"></a>Stil seçiciyi Ekle

<iframe height='500' scrolling='no' title='Stil seçiciyi ekleme' src='//codepen.io/azuremaps/embed/OwgyvG/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/OwgyvG/'>stil seçiciyi ekleme</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod ilk kod bloğunda abonelik anahtarını ayarlar ve bir harita nesnesi oluşturur, harita stili grayscale_dark için önceden ayarlanmış. Bkz: [bir harita oluşturmak](./map-create.md) bir harita oluşturmak yönergeler.

İkinci kod bloğu atlas kullanarak bir stil seçicisini yapıları [StyleControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.stylecontrol?view=azure-iot-typescript-latest#stylecontrol) Oluşturucusu.

Stil seçimi eşlemesi için stil seçiciyi sağlar. Üçüncü kod bloğu stil seçiciyi kullanarak haritanın harita ekler [control.add](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addcontrol) yöntemi. Tüm denetimler eşlemeleri yüklendikten sonra tam olarak yüklenmeden emin olmak için harita olay dinleyicisi içinde olan tüm denetimler.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan yöntemleri ve sınıfları hakkında daha fazla bilgi için:

> [!div class="nextstepaction"]
> [Harita](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

Denetim, eşlenir ekleyin:

> [!div class="nextstepaction"]
> [Eşleme denetimleri ekleme](./map-add-controls.md)

Bir harita raptiyesini ekleyin:

> [!div class="nextstepaction"]
> [Pin ekleme](./map-add-pin.md)
