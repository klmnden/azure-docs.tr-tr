---
title: Azure haritalar koordinatıyla ilgili bilgileri gösterme | Microsoft Docs
description: Bir kullanıcı bir koordinat seçtiğinde harita üzerinde bir adresi hakkında bilgi görüntüleme
author: jingjing-z
ms.author: jinzh
ms.date: 11/15/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: ada579af44d1d0b4ea08a8ae9eadbec386e44f08
ms.sourcegitcommit: 8899e76afb51f0d507c4f786f28eb46ada060b8d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/16/2018
ms.locfileid: "51823019"
---
# <a name="get-information-from-a-coordinate"></a>Bir koordinattan bilgi alma

Bu makale tıklandı açılan konumunun adresini gösteren bir ters adresi arama yapma.

Ters adresi arama yapmak için iki yol vardır. Bir yolu Sorgulanacak [Azure haritalar ters adresi arama API'si](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) hizmeti modülü aracılığıyla. Yapmak için diğer yol ise bir [XMLHttpRequest](https://xhr.spec.whatwg.org/) bir adresini bulmak için API. Her iki yönde, aşağıda alanında.

## <a name="make-a-reverse-search-request-via-service-module"></a>Hizmeti modülü aracılığıyla geriye doğru arama istekte

<iframe height='500' scrolling='no' title='(Hizmeti Modülü) bir Koordinattan bilgi alın' src='//codepen.io/azuremaps/embed/ejEYMZ/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/ejEYMZ/'>(hizmet Modülü) bir Koordinattan bilgi alma</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

İlk kod bloğunu bir harita nesnesi oluşturur. Gördüğünüz [bir harita oluşturmak](./map-create.md) yönergeler için.

İkinci kod bloğunu satırda bir istemci hizmeti başlatır.

Üçüncü kod bloğunu bir işaretçiye fare imlecini stilini güncelleştirir ve [açılan](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#open) nesne. Gördüğünüz [harita üzerinde bir açılır pencere ekleme](./map-add-popup.md) yönergeler için.

Dördüncü kod bloğunu ekler bir [olay dinleyicisi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) fareye tıklayana için. Fare tıklatın, Tıklatılan noktadan koordinatlarını bir arama sorgusu oluşturur. Haritanın kullanır [getSearchAddressReverse](https://docs.microsoft.com/javascript/api/azure-maps-rest/services.search?view=azure-iot-typescript-latest#getsearchaddressreverse) koordinatları adresini sorgulamak için uç nokta.

Başarılı bir yanıt için tıklatılan konumu adresi toplar ve açılan içeriğini ve konumunu aracılığıyla tanımlar [setOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#setoptions-popupoptions-) açılan sınıfının işlevi.

İmleç, bir açılan pencere nesnesi ve tıklama olayı değişikliği tüm oluşturulan haritanın içinde [yük olay dinleyicisi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) koordinatları bilgiler alınabilir önce harita yükleri tam olarak emin olmak için.

## <a name="make-a-reverse-search-request-via-xmlhttprequest"></a>XMLHttpRequest aracılığıyla geriye doğru arama istekte

<iframe height='500' scrolling='no' title='Bir koordinattan bilgi alma' src='//codepen.io/azuremaps/embed/ddXzoB/?height=516&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/ddXzoB/'>bir Koordinattan bilgi alma</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod ilk bloğu bir harita nesnesi oluşturur. Gördüğünüz [bir harita oluşturmak](./map-create.md) yönergeler için.

İkinci kod bloğunu bir işaretçiye fare imlecini stilini güncelleştirir ve [açılan](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#open) nesne. Gördüğünüz [harita üzerinde bir açılır pencere ekleme](./map-add-popup.md) yönergeler için.

Üçüncü kod bloğu için fare tıklama olay dinleyicisi ekler. Bir fare tıklaması gönderdiği bir [XMLHttpRequest](https://xhr.spec.whatwg.org/) için [Azure haritalar ters adresi arama API'si](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) sorgulamak için tıklatılan koordinatları adresi. Başarılı bir yanıt için tıklatılan konumu adresi toplar ve açılan içeriğini ve konumunu aracılığıyla tanımlar [setOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#setoptions-popupoptions-) açılan sınıfının işlevi.

İmleç, bir açılan pencere nesnesi ve tıklama olayı değişikliği tüm oluşturulan haritanın içinde [yük olay dinleyicisi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) koordinatları bilgiler alınabilir önce harita yükleri tam olarak emin olmak için.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan yöntemleri ve sınıfları hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Harita](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [Açılan menüsü](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest)

Tam kod örnekleri için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Yönergeleri A'dan B'ye yönleri gösterme](./map-route.md)

> [!div class="nextstepaction"]
> [Trafiği gösterme](./map-show-traffic.md)
