---
title: Azure Haritalar ile pin ekleme | Microsoft Docs
description: Bir Javascript harita için bir PIN ekleme
author: jingjing-z
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 0dafb09e1704e8e446b034975f0c25a740050599
ms.sourcegitcommit: a3a0f42a166e2e71fa2ffe081f38a8bd8b1aeb7b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2018
ms.locfileid: "43382593"
---
# <a name="add-pins-to-the-map"></a>PIN haritaya eklemek

Bu makalede bir PIN haritaya eklemek gösterilmektedir.  

## <a name="understand-the-code"></a>Kodu anlama

<iframe height='500' scrolling='no' title='Haritaya pin ekleme' src='//codepen.io/azuremaps/embed/ZrVpEa/?height=504&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/ZrVpEa/'>PIN bir haritaya eklemek</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod, kod bloğunun ilk harita nesnesi oluşturur. Gördüğünüz [bir harita oluşturmak](./map-create.md) yönergeler için.

İkinci kod bloğu içinde bir PIN oluşturulur ve eşlemesine eklenir. Bir PIN bir [özellik](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature?view=azure-iot-typescript-latest) , [noktası](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest) ile [PinProperties](https://docs.microsoft.com/javascript/api/azure-maps-control/models.pinproperties?view=azure-iot-typescript-latest) kendi özellik özelliği olarak. Kullanım `new atlas.data.Feature(new atlas.data.Point())` bir PIN oluşturun ve özelliklerini tanımlamak için. PIN dizisi bir PIN katmanıdır. Kullanım [addPins](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addpins) PIN katmanı haritaya eklemek ve PIN katman özelliklerini tanımlamak için harita sınıfın işlevi. Bir PIN katmanında özelliklerini görmek [PinLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/models.pinlayeroptions?view=azure-iot-typescript-latest). 

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan yöntemleri ve sınıfları hakkında daha fazla bilgi edinin: 
* [Harita](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)
    * [addPins](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addpins)
    
Eşlenir eklemek daha fazla kod örnekleri için aşağıdaki makalelere bakın: 
* [Açılan pencere Ekle](./map-add-popup.md)
* [Şekil ekleme](./map-add-shape.md)

