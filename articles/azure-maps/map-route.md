---
title: Azure Haritalar ile yönergeleri Göster | Microsoft Docs
description: Javascript harita üzerinde iki konum arasında yönergeleri görüntüleme
services: azure-maps
keywords: ''
author: jinzh-azureiot
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: article
ms.service: azure-maps
documentationcenter: ''
manager: timlt
ms.devlang: na
ms.custom: codepen
ms.openlocfilehash: 9007afd1bc4d2361addc2a554fab1330174e88e7
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="show-directions-from-a-to-b"></a>Yönergeleri A'dan B'ye Göster 

Bu makalede bir rota istek ve rota haritasında Göster gösterilmektedir. 

## <a name="understand-the-code"></a>Kodu anlama

<iframe height='500' scrolling='no' title='Yönergeleri A'dan B'ye bir haritasında Göster' src='//codepen.io/azuremaps/embed/zRyNmP/?height=469&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/zRyNmP/'>Göster yönergeleri A'dan B'ye bir haritada</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod ilk blok kod eşleme nesnesi oluşturur. Gördüğünüz [bir harita oluşturmak](./map-create.md) yönergeler için.

İkinci kod bloğunu oluşturur ve başlangıç ve bitiş noktası yolu temsil etmek için harita üzerinde PIN'ler ekler. Gördüğünüz [haritada bir PIN eklemek](map-add-pin.md) yönergeler için.

Üçüncü kod bloğunu kullanan [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#setcamerabounds) harita sınırlama kutusu ayarlamak için harita sınıfının işlevini temel başlangıç ve bitiş noktası yolu.

Dördüncü kod bloğunu gönderir bir [XMLHttpRequest](https://xhr.spec.whatwg.org/) için [Azure haritalar rota API'si](https://docs.microsoft.com/rest/api/maps/route/getroutedirections).

Gelen yanıt kodu son bloğunu ayrıştırır. Başarılı yanıt, her waypoint enlem ve boylam bilgi toplar. Bir dizi satırları kendi sonraki waypoint her waypoint bağlanarak oluşturur. Bu satırlar rota oluşturmak için harita üzerine ekler. Gördüğünüz [haritada bir satır ekleyin](./map-add-shape.md#addALine) yönergeler için.

## <a name="next-steps"></a>Sonraki adımlar

Sınıfları ve bu makalede kullanılan yöntemleri hakkında daha fazla bilgi edinin: 

* [eşleme](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest)
    * [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#setcamerabounds)
    * [addLinestrings](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#addlinestrings)
    * [addPins](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#addpins)
