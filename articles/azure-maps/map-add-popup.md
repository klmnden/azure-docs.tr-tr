---
title: Azure Haritalar ile bir açılan pencere Ekle | Microsoft Docs
description: Javascript harita için açılır pencere ekleme
author: jingjing-z
ms.author: jinzh
ms.date: 09/17/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 76a7e230491d5e524a1d73437a56d12594cfebe2
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46127446"
---
# <a name="add-a-popup-to-the-map"></a>Haritaya açılır pencere ekleme

Bu makalede bir açılır pencere haritaya eklemek nasıl gösterir.  

## <a name="understand-the-code"></a>Kodu anlama

<a id="addAPopup"></a>

<iframe height='500' scrolling='no' title='Haritaya açılır pencere ekleme' src='//codepen.io/azuremaps/embed/zRyKxj/?height=545&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/zRyKxj/'>açılan bir haritaya eklemek</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod, kod bloğunun ilk harita nesnesi oluşturur. Gördüğünüz [bir harita oluşturmak](./map-create.md) yönergeler için.

İkinci kod bloğunu PIN oluşturur ve haritaya ekleyebilirsiniz. Gördüğünüz [PIN haritaya eklemek](./map-add-pin.md) yönergeler için.

Üçüncü kod bloğu içinde bir açılır pencere görüntülenecek içeriği oluşturur. Açılan içerik HTML öğesidir.

Dördüncü bloğu kod oluşturur bir [açılan nesne](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest) aracılığıyla `new atlas.Popup()`. Açılan pencerenin içeriğini ve konumunu gibi özelliklerdir parçası [PopupOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/models.popupoptions?view=azure-iot-typescript-latest). Açılan pencere Oluşturucusu veya yoluyla PopupOptions tanımlanabilir [setPopupOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#setpopupoptions) açılan sınıfının işlevi.

Son kod bloğunu kullanır [addEventListener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addeventlistener) işlevi kullanır ve PIN'leri fareyi üzerine getirme olay dinlemek için eşleme sınıfının [açın](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#open) işlevi açılan sınıfın bir olay oluşursa, açılır penceresini aç.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan yöntemleri ve sınıfları hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Harita](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [Açılan menüsü](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest)

Tam kod örnekleri için harika aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Şekil ekleme](./map-add-shape.md)

> [!div class="nextstepaction"]
> [Özel HTML ekleme](./map-add-custom-html.md)
