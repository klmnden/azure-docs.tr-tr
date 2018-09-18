---
title: Azure haritalar koordinatıyla ilgili bilgileri gösterme | Microsoft Docs
description: Bir kullanıcı bir koordinat seçtiğinde harita üzerinde bir adresi hakkında bilgi görüntüleme
author: jingjing-z
ms.author: jinzh
ms.date: 09/08/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: bf44437f4d0b60a5d56c2be29418b7132346da2e
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/17/2018
ms.locfileid: "45732303"
---
# <a name="get-information-from-a-coordinate"></a>Bir koordinattan bilgi alma

Bu makale tıklandı açılan konumunun adresini gösteren bir ters adresi arama yapma.

Ters adresi arama yapmak için iki yol vardır. Bir yolu Sorgulanacak [Azure haritalar ters adresi arama API'si](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) hizmeti modülü aracılığıyla. Yapmak için diğer yol ise bir [XMLHttpRequest](https://xhr.spec.whatwg.org/) bir adresini bulmak için API. Her iki yönde, aşağıda alanında.

## <a name="make-a-reverse-search-request-via-service-module"></a>Hizmeti modülü aracılığıyla geriye doğru arama istekte

<iframe height='500' scrolling='no' title='(Hizmeti Modülü) bir Koordinattan bilgi alın' src='//codepen.io/azuremaps/embed/ejEYMZ/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/ejEYMZ/'>(hizmet Modülü) bir Koordinattan bilgi alma</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

İlk kod bloğunu bir harita nesnesi oluşturur. Gördüğünüz [bir harita oluşturmak](./map-create.md) yönergeler için.

İkinci kod bloğunu satırda hizmeti istemcisi oluşturur.

Üçüncü kod bloğu için bir işaretçi fare imlecini stilini güncelleştirir.

Dördüncü kod bloğu kullanarak bir açılan pencere oluşturur [açın](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#open). Gördüğünüz [harita üzerinde bir açılır pencere ekleme](./map-add-popup.md) yönergeler için.

Son kod bloğunu [olay dinleyicisi ekler](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addeventlistener) fareye tıklayana için. Fare tıklatın, bir arama sorgusu ile tıklatılan noktadan ' ortak ordinates oluşturur. Haritanın kullanıyorsa [getSearchAddressReverse](https://docs.microsoft.com/javascript/api/azure-maps-rest/services.search?view=azure-iot-typescript-latest#getsearchaddressreverse) ortak ordinates adresini sorgulamak için uç nokta.

Başarılı bir yanıt için tıklatılan konumu adresi toplar ve açılan içeriğini ve konumunu aracılığıyla tanımlar [setPopupOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#setpopupoptions) açılan sınıfının işlevi.

## <a name="make-a-reverse-search-request-via-xmlhttprequest"></a>XMLHttpRequest aracılığıyla geriye doğru arama istekte

<iframe height='500' scrolling='no' title='Bir koordinattan bilgi alma' src='//codepen.io/azuremaps/embed/ddXzoB/?height=516&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/ddXzoB/'>bir Koordinattan bilgi alma</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

İlk kod bloğunu bir harita nesnesi oluşturur. Gördüğünüz [bir harita oluşturmak](./map-create.md) yönergeler için.

İkinci kod bloğu için bir işaretçi fare imlecini stilini güncelleştirir.

Kullanarak bir açılan pencere üçüncü kod bloğunu oluşturur [açın](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#open). Gördüğünüz [harita üzerinde bir açılır pencere ekleme](./map-add-popup.md) yönergeler için.

Son kod bloğu için fare tıklama olay dinleyicisi ekler. Bir fare tıklaması gönderdiği bir [XMLHttpRequest](https://xhr.spec.whatwg.org/) için [Azure haritalar ters adresi arama API'si](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse). Başarılı bir yanıt için tıklatılan konumu adresi toplar ve açılan içeriğini ve konumunu aracılığıyla tanımlar [setPopupOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#setpopupoptions) açılan sınıfının işlevi

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
