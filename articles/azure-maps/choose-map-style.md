---
title: Azure haritalar stili planlanabilecek harita | Microsoft Docs
description: Azure haritalar hakkında bilgi edinin stili ilgili işlevler.
author: walsehgal
ms.author: v-musehg
ms.date: 10/08/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: ffed12b9184c7b6a690c30db9826f031fe6c9f9b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60795893"
---
# <a name="choose-a-map-style-in-azure-maps"></a>Azure haritalar harita stil seçin

Azure haritalar seçebileceğiniz dört farklı eşlemeler stilleri sahiptir. Eşleme stilleri hakkında daha fazla bilgi için bkz. [eşleme stilleri Azure eşlemelerinde desteklenen](./supported-map-styles.md). Bu makale stil harita üzerindeki yükü ayarlamak, yeni bir stil ve stil seçici denetimi kullanmak için stil işlevlerini kullanma.

## <a name="set-style-on-map-load"></a>Harita yükleme kümesi stili

<iframe height='500' scrolling='no' title='Harita üzerindeki yükü stili ayarlanıyor' src='//codepen.io/azuremaps/embed/WKOQRq/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/WKOQRq/'>harita üzerindeki yükü stili ayarlanıyor</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod bloğu abonelik anahtarını ayarlar ve ayarlamak için grayscale_dark stili bir harita nesnesi oluşturur. Bkz: [bir harita oluşturmak](./map-create.md) bir harita oluşturmak yönergeler.

## <a name="update-the-style"></a>Güncelleştirme stili

<iframe height='500' scrolling='no' title='Stil güncelleştiriliyor' src='//codepen.io/azuremaps/embed/yqXYzY/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/yqXYzY/'>stili güncelleştirme</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod bloğu abonelik anahtarını ayarlar ve stili önceden ayarlamadan bir harita oluşturur. Bkz: [bir harita oluşturmak](./map-create.md) bir harita oluşturmak yönergeler.

İkinci kod bloğu haritanın kullanan [setStyle](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) harita stili için uydu ayarlamak için yöntemi.

## <a name="add-the-style-picker"></a>Stil seçiciyi Ekle

<iframe height='500' scrolling='no' title='Stil seçiciyi ekleme' src='//codepen.io/azuremaps/embed/OwgyvG/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/OwgyvG/'>stil seçiciyi ekleme</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod ilk kod bloğunda abonelik anahtarını ayarlar ve bir harita nesnesi oluşturur, harita stili grayscale_dark için önceden ayarlanmış. Bkz: [bir harita oluşturmak](./map-create.md) bir harita oluşturmak yönergeler.

İkinci kod bloğu atlas kullanarak bir stil seçicisini yapıları [StyleControl](/javascript/api/azure-maps-control/atlas.control.stylecontrol) Oluşturucusu.

Stil seçimi eşlemesi için stil seçiciyi sağlar. Üçüncü kod bloğu stil seçiciyi kullanarak haritanın harita ekler [controls.add](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) yöntemi. Stil seçiciyi içinde haritasıdır **olay dinleyicisi** harita tamamen yüklendikten sonra yüklendikten emin olmak için.

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
