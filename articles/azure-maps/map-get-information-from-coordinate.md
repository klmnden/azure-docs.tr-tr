---
title: Azure haritalar koordinatıyla ilgili bilgileri gösterme | Microsoft Docs
description: Bir kullanıcı bir koordinat seçtiğinde harita üzerinde bir adresi hakkında bilgi görüntüleme
author: jingjing-z
ms.author: jinzh
ms.date: 09/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: d1baa4adc555e65c4a25928d19f201dba6109142
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44157693"
---
# <a name="get-information-from-a-coordinate"></a>Bir koordinattan bilgi alma

Bu makalede bir ters adresi arama yapın ve bir fare tıklaması tıklandı konumunun adresini açılan pencerede göster gösterilmektedir.

Ters adresi arama yapmak için iki yolu vardır, biridir sorgulayarak [Azure haritalar ters adresi arama API'si](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) modülü ve diğer hizmet aracılığıyla olan yaparak bir [XMLHttpRequest](https://xhr.spec.whatwg.org/) API sorgulamak için adresi. Her ikisi de aşağıda ele alır.

## <a name="use-the-service-module-to-make-a-reverse-address-search"></a>Hizmeti modülü ters adresi arama yapma

### <a name="understand-the-code"></a>Kodu anlama

<iframe height='500' scrolling='no' title='(Hizmeti Modülü) bir Koordinattan bilgi alın' src='//codepen.io/azuremaps/embed/ejEYMZ/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/ejEYMZ/'>(hizmet Modülü) bir Koordinattan bilgi alma</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod, kod bloğunun ilk harita nesnesi oluşturur. Gördüğünüz [bir harita oluşturmak](./map-create.md) yönergeler için.

İkinci kod bloğunu satırda hizmeti istemcisi oluşturur.

Üçüncü kod bloğu için bir işaretçi fare imlecini stilini güncelleştirir.

Dördüncü kod bloğu bir açılır pencere oluşturur. Gördüğünüz [harita üzerinde bir açılır pencere ekleme](./map-add-popup.md) yönergeler için.

Son kod bloğu için fare tıklama olay dinleyicisi ekler. Fare tıklatın, bir arama sorgusu ile tıklatılan noktadan ' ortak ordinates oluşturur. Haritanın kullanıyorsa [getSearchAddressReverse](https://docs.microsoft.com/javascript/api/azure-maps-rest/services.search?view=azure-iot-typescript-latest#getsearchaddressreverse) ortak ordinates adresini sorgulamak için uç nokta.

Başarılı bir yanıt için tıklatılan konumu adresi toplar ve açılan içeriğini ve konumunu aracılığıyla tanımlar [setPopupOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#setpopupoptions) açılan sınıfının işlevi.

## <a name="use-xmlhttprequest-to-make-a-reverse-address-search"></a>XMLHTTPRequest ters adresi arama yapma

### <a name="understand-the-code"></a>Kodu anlama

<iframe height='500' scrolling='no' title='Bir koordinattan bilgi alma' src='//codepen.io/azuremaps/embed/ddXzoB/?height=516&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/ddXzoB/'>bir Koordinattan bilgi alma</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod, kod bloğunun ilk harita nesnesi oluşturur. Gördüğünüz [bir harita oluşturmak](./map-create.md) yönergeler için.

İkinci kod bloğu için bir işaretçi fare imlecini stilini güncelleştirir.

Üçüncü kod bloğunu bir açılır pencere oluşturur. Gördüğünüz [harita üzerinde bir açılır pencere ekleme](./map-add-popup.md) yönergeler için.

Son kod bloğu için fare tıklama olay dinleyicisi ekler. Bir fare tıklaması gönderdiği bir [XMLHttpRequest](https://xhr.spec.whatwg.org/) için [Azure haritalar ters adresi arama API'si](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse). Başarılı bir yanıt için tıklatılan konumu adresi toplar ve açılan içeriğini ve konumunu aracılığıyla tanımlar [setPopupOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#setpopupoptions) açılan sınıfının işlevi

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
