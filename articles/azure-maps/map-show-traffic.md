---
title: Azure Haritalar ile trafiği gösterme | Microsoft Docs
description: Bir Javascript harita üzerindeki trafik verileri görüntüleme
author: jingjing-z
ms.author: jinzh
ms.date: 11/10/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 7cd7c0dbb375dad78927183dbaffe574a0dc10c2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60768844"
---
# <a name="show-traffic-on-the-map"></a>Harita üzerinde trafiği Göster

Bu makalede haritada trafik ve olaylar bilgileri gösterecek şekilde gösterilmektedir.

## <a name="understand-the-code"></a>Kodu anlama

<iframe height='456' scrolling='no' title='Trafiği bir haritada gösterme' src='//codepen.io/azuremaps/embed/WMLRPw/?height=456&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/WMLRPw/'>trafiği bir haritada gösterme</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod, kod bloğunun ilk harita nesnesi oluşturur. Gördüğünüz [bir harita oluşturmak](map-create.md) yönergeler için.

İkinci kod bloğunu kullanır [setTraffic](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) haritanın işlevindeki [olay dinleyicisi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) harita üzerinde olayları ve trafik akışları işlemek için işlevi.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan yöntemleri ve sınıfları hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Harita](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

Tam kod örnekleri için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Kod örnek sayfası](https://aka.ms/AzureMapsSamples)

Kullanıcı deneyimlerini geliştirir:

> [!div class="nextstepaction"]
> [Fare olayları ile etkileşim eşleme](./map-events.md)

> [!div class="nextstepaction"]
> [Erişilebilir bir harita oluşturma](./map-accessibility.md)
