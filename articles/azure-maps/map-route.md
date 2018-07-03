---
title: Azure Haritalar ile yönergeleri Göster | Microsoft Docs
description: Javascript harita üzerinde iki konum arasında yönergeleri görüntüleme
author: jingjing-z
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: codepen
ms.openlocfilehash: 5e9ab73ddc16517e17894cddd9bc102f3941f00c
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "35759802"
---
# <a name="show-directions-from-a-to-b"></a>A'dan B'ye yönleri gösterme 

Bu makalede bir rota istek ve rota haritasında Göster gösterilmektedir. 

## <a name="understand-the-code"></a>Kodu anlama

<iframe height='500' scrolling='no' title='A noktasından B noktasına yol tarifini haritada göster' src='//codepen.io/azuremaps/embed/zRyNmP/?height=469&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/zRyNmP/'>Göster yönergeleri A'dan B'ye bir haritada</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod ilk blok kod eşleme nesnesi oluşturur. Gördüğünüz [bir harita oluşturmak](./map-create.md) yönergeler için.

İkinci kod bloğunu oluşturur ve başlangıç ve bitiş noktası yolu temsil etmek için harita üzerinde PIN'ler ekler. Gördüğünüz [haritada bir PIN eklemek](map-add-pin.md) yönergeler için.

Üçüncü kod bloğunu kullanan [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#setcamerabounds) harita sınırlama kutusu ayarlamak için harita sınıfının işlevini temel başlangıç ve bitiş noktası yolu.

Dördüncü kod bloğunu gönderir bir [XMLHttpRequest](https://xhr.spec.whatwg.org/) için [Azure haritalar rota API'si](https://docs.microsoft.com/rest/api/maps/route/getroutedirections).

Gelen yanıt kodu son bloğunu ayrıştırır. Başarılı yanıt, her waypoint enlem ve boylam bilgi toplar. Bir dizi satırları kendi sonraki waypoint her waypoint bağlanarak oluşturur. Bu satırlar rota oluşturmak için harita üzerine ekler. Gördüğünüz [haritada bir satır ekleyin](./map-add-shape.md#addALine) yönergeler için.

## <a name="next-steps"></a>Sonraki adımlar

Sınıfları ve bu makalede kullanılan yöntemleri hakkında daha fazla bilgi edinin: 

* [Eşleme](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest)
    * [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#setcamerabounds)
    * [addLinestrings](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#addlinestrings)
    * [addPins](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#addpins)

Eşlemeleri eklemek daha fazla kod örnekleri için aşağıdaki makalelere bakın: 
* [Trafik haritasında Göster](./map-show-traffic.md)
* [Harita - fare olayları etkileşim](./map-events.md)
