---
title: Azure Haritalar ile trafiği gösterme | Microsoft Docs
description: Bir Javascript harita üzerindeki trafik verileri görüntüleme
author: jingjing-z
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 2499822db587dbf47dccedf6039d0fb5823c58b5
ms.sourcegitcommit: b5ac31eeb7c4f9be584bb0f7d55c5654b74404ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/23/2018
ms.locfileid: "42746087"
---
# <a name="show-traffic-on-the-map"></a>Harita üzerinde trafiği Göster

Bu makalede haritada trafik ve olaylar bilgileri gösterecek şekilde gösterilmektedir. 

## <a name="understand-the-code"></a>Kodu anlama

<iframe height='456' scrolling='no' title='Trafiği bir haritada gösterme' src='//codepen.io/azuremaps/embed/WMLRPw/?height=456&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/WMLRPw/'>trafiği bir haritada gösterme</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod, kod bloğunun ilk harita nesnesi oluşturur. Gördüğünüz [bir harita oluşturmak](map-create.md) yönergeler için.

İkinci kod bloğunu kullanır [setTraffic](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#settraffic) harita üzerinde olayları ve trafik akışları işlemek için eşleme sınıfının işlevi.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan yöntemleri ve sınıfları hakkında daha fazla bilgi edinin: 
* [Harita](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)
    * [setTraffic](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#settraffic)

Eşlenir eklemek daha fazla kod örnekleri için aşağıdaki makalelere bakın: 
* [Harita – fare olayları ile etkileşim kurma](./map-events.md)
* [Erişilebilir bir harita oluşturma](./map-accessibility.md)

Kullanıma sunduğumuz [kod örnek sayfası](http://aka.ms/AzureMapsSamples) daha fazla eşleme senaryolar için.
