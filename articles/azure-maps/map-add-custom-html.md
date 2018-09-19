---
title: Azure haritalar içinde özel html ekleme | Microsoft Docs
description: Bir Javascript harita için özel html ekleme
author: jingjing-z
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: e5cfbc7ddc10edf9b21afce73e3b7f8795fcdac9
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46121966"
---
# <a name="add-custom-html-to-the-map"></a>Eşleme için özel HTML ekleme

Bu makalede bir görüntü dosyası gibi özel bir HTML haritaya eklemek gösterilmektedir.

## <a name="understand-the-code"></a>Kodu anlama

<iframe height='466' scrolling='no' title='Bir eşleme - png özel html ekleme' src='//codepen.io/azuremaps/embed/MVoeVw/?height=466&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/MVoeVw/'>haritaya - png özel html ekleme</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod, kod bloğunun ilk harita nesnesi oluşturur. Gördüğünüz [bir harita oluşturmak](./map-create.md) yönergeler için.

İkinci kod bloğunu bir görüntüden bir HTML öğesi oluşturur.

Son kod bloğunu kullanır [addHtml](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addhtml) harita belirtilen konuma görüntüsü eklemek için eşleme sınıfının işlevi.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan yöntemleri ve sınıfları hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Harita](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

Eşlenir eklemek daha fazla kod örnekleri için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Arama sonuçlarını göster](./map-search-location.md)

> [!div class="nextstepaction"]
> [Bir Koordinattan bilgi alma](./map-get-information-from-coordinate.md)