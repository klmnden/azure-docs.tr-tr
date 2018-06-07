---
title: Açılan pencere Azure Haritalar ile ekleme | Microsoft Docs
description: Javascript haritaya popup ekleme
author: jingjing-z
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: d9eeac28b204af0bb7d8e204762aae854055b33a
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34599312"
---
# <a name="add-a-popup-to-the-map"></a>Açılan pencere eşlemeye ekleyin

Bu makalede haritaya popup ekleme gösterilmektedir.  

## <a name="understand-the-code"></a>Kodu anlama

<a id="addAPopup"></a>

<iframe height='500' scrolling='no' title='Açılan pencere için bir harita Ekle' src='//codepen.io/azuremaps/embed/zRyKxj/?height=545&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/zRyKxj/'>açılan bir harita Ekle</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod ilk blok kod eşleme nesnesi oluşturur. Gördüğünüz [bir harita oluşturmak](./map-create.md) yönergeler için.

İkinci kod bloğunu bir PIN oluşturur ve eşlemeye ekleyin. Gördüğünüz [eşlemesine bir PIN eklemek](./map-add-pin.md) yönergeler için.

Üçüncü kod bloğunu popup içinde görüntülenecek içerik oluşturur. Açılan içeriği HTML öğesidir. 

Kod dördüncü bloğu oluşturur bir [açılan nesne](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest) aracılığıyla `new atlas.Popup()`. İçerik ve konum gibi açılan özellikleri parçası olan [PopupOptions](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popupoptions?view=azure-iot-typescript-latest). Açılan Oluşturucusu veya aracılığıyla PopupOptions tanımlanabilir [setPopupOptions](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#setpopupoptions) açılan sınıfının işlevi.

Kodu son bloğunu kullanır [addEventListener](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#addeventlistener) PIN ve kullandığı fare üzerinde olayı dinlemek için harita sınıfının işlevini [açmak](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#open) olay oluşursa açılan açmak için açılan sınıfının işlevi.


## <a name="next-steps"></a>Sonraki adımlar

Sınıfları ve bu makalede kullanılan yöntemleri hakkında daha fazla bilgi edinin: 

* [eşleme](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest)
    * [addPins](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#addpins)
    * [addEventListener](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#addeventlistener)
* [Açılan](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest)
    * [setPopupOptions](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#setpopupoptions)
    * [Açık](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#open)
    * [Kapat](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#close)
    
Eşlemeleri eklemek daha fazla kod örnekleri için aşağıdaki makalelere bakın: 
* [Bir şekli Ekle](./map-add-shape.md)
* [Özel HTML Ekle](./map-add-custom-html.md)
