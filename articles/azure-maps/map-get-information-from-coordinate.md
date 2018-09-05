---
title: Azure haritalar koordinatıyla ilgili bilgileri gösterme | Microsoft Docs
description: Bir kullanıcı bir koordinat seçtiğinde harita üzerinde bir adresi hakkında bilgi görüntüleme
author: jingjing-z
ms.author: jinzh
ms.date: 08/31/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 331e687c40f21b0bf6074239969848c632682773
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2018
ms.locfileid: "43667490"
---
# <a name="get-information-from-a-coordinate"></a>Bir koordinattan bilgi alma

Bu makalede bir ters adresi arama yapın ve bir fare tıklaması tıklandı konumunun adresini açılan pencerede göster gösterilmektedir.

## <a name="understand-the-code"></a>Kodu anlama

<iframe height='500' scrolling='no' title='(Hizmeti Modülü) bir Koordinattan bilgi alın' src='//codepen.io/azuremaps/embed/ejEYMZ/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/ejEYMZ/'>(hizmet Modülü) bir Koordinattan bilgi alma</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod, kod bloğunun ilk harita nesnesi oluşturur. Gördüğünüz [bir harita oluşturmak](./map-create.md) yönergeler için.

İkinci kod bloğunu satırda hizmeti istemcisi oluşturur.

Üçüncü kod bloğu için bir işaretçi fare imlecini stilini güncelleştirir.

Dördüncü kod bloğu bir açılır pencere oluşturur. Gördüğünüz [harita üzerinde bir açılır pencere ekleme](./map-add-popup.md) yönergeler için.

Son kod bloğu için fare tıklama olay dinleyicisi ekler. Fare tıklatın, bir arama sorgusu ile tıklatılan noktadan ' ortak ordinates oluşturur. Haritanın kullanıyorsa [getSearchAddressReverse](https://docs.microsoft.com/javascript/api/azure-maps-rest/services.search?view=azure-iot-typescript-latest#getsearchaddressreverse) ortak ordinates adresini sorgulamak için uç nokta.

Başarılı bir yanıt için tıklatılan konumu adresi toplar ve açılan içeriğini ve konumunu aracılığıyla tanımlar [setPopupOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#setpopupoptions) açılan sınıfının işlevi.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan yöntemleri ve sınıfları hakkında daha fazla bilgi edinin: 
* [Ters adresi arama](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse)
* [Harita](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)
    * [addEventListener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addeventlistener)
* [Açılan menüsü](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest)
    * [setPopupOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#setpopupoptions)
    * [açın](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#open)
    * [Kapat](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#close)

Eşlenir eklemek daha fazla kod örnekleri için aşağıdaki makalelere bakın:
* [Yönergeleri A'dan B'ye yönleri gösterme](./map-route.md)
* [Trafiği gösterme](./map-show-traffic.md)
