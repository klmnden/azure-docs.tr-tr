---
title: Bir PIN ile Azure eşlemeleri ekleme | Microsoft Docs
description: Javascript haritaya bir PIN ekleme
author: jingjing-z
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 094abe08c0c88c7561185675ceb8529be2c87a0a
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35294657"
---
# <a name="add-pins-to-the-map"></a>PIN eşlemeye ekleyin

Bu makalede bir eşlemesi için bir PIN eklemek nasıl gösterir.  

## <a name="understand-the-code"></a>Kodu anlama

<iframe height='500' scrolling='no' title='Bir PIN bir harita Ekle' src='//codepen.io/azuremaps/embed/ZrVpEa/?height=504&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/ZrVpEa/'>haritaya bir PIN eklemek</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod ilk blok kod eşleme nesnesi oluşturur. Gördüğünüz [bir harita oluşturmak](./map-create.md) yönergeler için.

Kod ikinci bloğunda bir PIN oluşturulur ve eşlemeye eklenir. Bir PIN bir [özelliği](https://docs.microsoft.com/javascript/api/azure-maps-javascript/feature?view=azure-iot-typescript-latest) , [noktası](https://docs.microsoft.com/javascript/api/azure-maps-javascript/point?view=azure-iot-typescript-latest) ile [PinProperties](https://docs.microsoft.com/javascript/api/azure-maps-javascript/pinproperties?view=azure-iot-typescript-latest) özelliği özelliği olarak. Kullanım `new atlas.data.Feature(new atlas.data.Point())` bir PIN oluşturmak ve özelliklerini tanımlamak için. Bir PIN katman PIN'ler dizisidir. Kullanım [addPins](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#addpins) eşlemeye PIN bir katman ekleyin ve PIN katman özelliklerini tanımlamak için harita sınıfının işlevi. Bir PIN katmanında özelliklerini görmek [PinLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-javascript/pinlayeroptions?view=azure-iot-typescript-latest). 

## <a name="next-steps"></a>Sonraki adımlar

Sınıfları ve bu makalede kullanılan yöntemleri hakkında daha fazla bilgi edinin: 
* [eşleme](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest)
    * [addPins](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#addpins)
    
Eşlemeleri eklemek daha fazla kod örnekleri için aşağıdaki makalelere bakın: 
* [Açılan pencere ekleme](./map-add-popup.md)
* [Bir şekli Ekle](./map-add-shape.md)

