---
title: Azure haritalar stili planlanabilecek harita | Microsoft Docs
description: Azure haritalar hakkında bilgi edinin stili ilgili işlevler.
author: walsehgal
ms.author: v-musehg
ms.date: 08/31/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 160752cd0467ef307f7a45b1e0d703c7ddd5d773
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "44720812"
---
# <a name="choose-a-map-style-in-azure-maps"></a>Azure haritalar harita stil seçin
Azure haritalar seçebileceğiniz dört farklı eşlemeler stilleri sahiptir. Eşleme stilleri hakkında daha fazla bilgi için bkz. [eşleme stilleri Azure eşlemelerinde desteklenen](./supported-map-styles.md). Bu makale stil harita üzerindeki yükü ayarlamak, yeni bir stil ve stil seçici denetimi kullanmak için stil işlevlerini kullanma.

## <a name="setting-style-on-map-load"></a>Harita üzerindeki yükü stili ayarlanıyor

<iframe height='500' scrolling='no' title='Harita üzerindeki yükü stili ayarlanıyor' src='//codepen.io/azuremaps/embed/WKOQRq/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/WKOQRq/'>harita üzerindeki yükü stili ayarlanıyor</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod, stil için gri tonlamalı kümesi ile bir harita nesnesi oluşturur. Bkz: [bir harita oluşturmak](./map-create.md) bir harita oluşturmak yönergeler.

## <a name="updating-the-style"></a>Stil güncelleştiriliyor

<iframe height='500' scrolling='no' title='Stil güncelleştiriliyor' src='//codepen.io/azuremaps/embed/yqXYzY/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/yqXYzY/'>stili güncelleştirme</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kodda kod ilk bloğu stili önceden ayarlamadan bir harita nesnesi oluşturur. Bkz: [bir harita oluşturmak](./map-create.md) bir harita oluşturmak yönergeler.

Haritanın ikinci kod saati kullanır [setStyle](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setstyle) harita stili için uydu ayarlamak için yöntemi.

## <a name="adding-the-style-picker"></a>Stil seçiciyi ekleme

<iframe height='500' scrolling='no' title='Stil seçiciyi ekleme' src='//codepen.io/azuremaps/embed/OwgyvG/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/OwgyvG/'>stil seçiciyi ekleme</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod ilk kod bloğunda stili önceden ayarlamadan bir harita nesnesi oluşturur. Bkz: [bir harita oluşturmak](./map-create.md) bir harita oluşturmak yönergeler.

İkinci kod bloğu atlas kullanarak bir stil seçicisini yapıları [StyleControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.stylecontrol?view=azure-iot-typescript-latest#stylecontrol) Oluşturucusu.

Stil seçimi eşlemesi için stil seçiciyi sağlar. Üçüncü kod bloğu stil seçiciyi kullanarak haritanın harita ekler [addControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addcontrol) yöntemi.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan yöntemleri ve sınıfları hakkında daha fazla bilgi edinin: 
* [Harita](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)
    * [setStyle](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setstyle)
    * [addControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addcontrol)

* [Atlas](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas?view=azure-iot-typescript-latest)
    * [StyleControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.stylecontrol?view=azure-iot-typescript-latest#stylecontrol)
    
Eşlenir eklemek daha fazla kod örnekleri için aşağıdaki makalelere bakın:
* [Eşleme denetimleri ekleme](./map-add-controls.md)
* [Pin ekleme](./map-add-pin.md)
