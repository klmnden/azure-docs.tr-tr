---
title: Azure Haritalar ile yönleri gösterme | Microsoft Docs
description: Bir Javascript harita üzerinde iki konum arasında yönergeleri görüntüleme
author: jingjing-z
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: codepen
ms.openlocfilehash: 52462c1c5a2a1a9698a2b51708e63b1bb1664f6e
ms.sourcegitcommit: b5ac31eeb7c4f9be584bb0f7d55c5654b74404ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/23/2018
ms.locfileid: "42745545"
---
# <a name="show-directions-from-a-to-b"></a>A'dan B'ye yönleri gösterme 

Bu makalede bir rota istekte bulunmak ve harita üzerinde yolu göstermek gösterilmektedir. 

## <a name="understand-the-code"></a>Kodu anlama

<iframe height='500' scrolling='no' title='A noktasından B noktasına yol tarifini haritada göster' src='//codepen.io/azuremaps/embed/zRyNmP/?height=469&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/zRyNmP/'>Göster yönergeleri A'dan B'ye bir haritada</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod, kod bloğunun ilk harita nesnesi oluşturur. Gördüğünüz [bir harita oluşturmak](./map-create.md) yönergeler için.

İkinci kod bloğunu oluşturur ve başlangıç ve bitiş noktasına rota temsil etmek için harita üzerinde iğne ekler. Gördüğünüz [haritada pin ekleme](map-add-pin.md) yönergeler için.

Üçüncü kod bloğunu kullanır [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamerabounds) harita sınırlayıcı kutu ayarlamak için eşleme sınıfının işlevini alarak başlangıç ve bitiş noktası yolu.

Dördüncü kod bloğunu gönderen bir [XMLHttpRequest](https://xhr.spec.whatwg.org/) için [Azure haritalar rota API'si](https://docs.microsoft.com/rest/api/maps/route/getroutedirections).

Son kod bloğunu gelen yanıtları ayrıştırmak için. Başarılı bir yanıt için her güzergah noktası için enlem ve boylam bilgilerini toplar. Her güzergah noktası için sonraki kendi güzergah noktası bağlanarak satırları bir dizi oluşturur. Rota işlemek için haritada tüm bu satırlara ekler. Gördüğünüz [harita üzerinde bir satır ekleyin](./map-add-shape.md#addALine) yönergeler için.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan yöntemleri ve sınıfları hakkında daha fazla bilgi edinin: 

* [Harita](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)
    * [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamerabounds)
    * [addLinestrings](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addlinestrings)
    * [addPins](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addpins)

Eşlenir eklemek daha fazla kod örnekleri için aşağıdaki makalelere bakın: 
* [Harita üzerinde trafiği Göster](./map-show-traffic.md)
* [Eşleme - fare olayları ile etkileşim kurma](./map-events.md)
