---
title: Azure Haritalar ile trafiği gösterme | Microsoft Docs
description: Bir Javascript harita üzerindeki trafik verileri görüntüleme
author: jingjing-z
ms.author: jinzh
ms.date: 09/14/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 6d5c721ab84c28bae9415dceeaa09fd12cc05824
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/17/2018
ms.locfileid: "45733034"
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

> [!div class="nextstepaction"]
> [Harita](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

Tam kod örnekleri için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Kod örnek sayfası](http://aka.ms/AzureMapsSamples)

Kullanıcı deneyimlerini geliştirir:

> [!div class="nextstepaction"]
> [Fare olayları ile etkileşim eşleme](./map-events.md)

> [!div class="nextstepaction"]
> [Erişilebilir bir harita oluşturma](./map-accessibility.md)
