---
title: Azure haritalar koordinatıyla ilgili bilgileri gösterme | Microsoft Docs
description: Bir kullanıcı bir koordinat seçtiğinde harita üzerinde bir adresi hakkında bilgi görüntüleme
author: jingjing-z
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: b640346b0d6f490457e1e82a65c0d3f373d658d3
ms.sourcegitcommit: 17fe5fe119bdd82e011f8235283e599931fa671a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/11/2018
ms.locfileid: "42056163"
---
# <a name="get-information-from-a-coordinate"></a>Bir koordinattan bilgi alma

Bu makalede bir ters adresi arama yapın ve bir fare tıklaması tıklandı konumunun adresini açılan pencerede göster gösterilmektedir. 

## <a name="understand-the-code"></a>Kodu anlama

<iframe height='500' scrolling='no' title='Bir koordinattan bilgi alma' src='//codepen.io/azuremaps/embed/ddXzoB/?height=516&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/ddXzoB/'>bir Koordinattan bilgi alma</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod, kod bloğunun ilk harita nesnesi oluşturur. Gördüğünüz [bir harita oluşturmak](./map-create.md) yönergeler için.

İkinci kod bloğu için bir işaretçi fare imlecini stilini güncelleştirir.

Üçüncü kod bloğunu bir açılır pencere oluşturur. Gördüğünüz [harita üzerinde bir açılır pencere ekleme](./map-add-popup.md) yönergeler için.

Son kod bloğu için fare tıklama olay dinleyicisi ekler. Bir fare tıklaması gönderdiği bir [XMLHttpRequest](https://xhr.spec.whatwg.org/) için [Azure haritalar ters adresi arama API'si](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse). Başarılı bir yanıt için tıklatılan konumu adresi toplar ve açılan içeriğini ve konumunu aracılığıyla tanımlar [setPopupOptions](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#setpopupoptions) açılan sınıfının işlevi

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan yöntemleri ve sınıfları hakkında daha fazla bilgi edinin: 
* [Ters adresi arama](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse)
* [Harita](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest)
    * [addEventListener](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#addeventlistener)
* [Açılan menüsü](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest)
    * [setPopupOptions](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#setpopupoptions)
    * [açın](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#open)
    * [Kapat](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#close)

Eşlenir eklemek daha fazla kod örnekleri için aşağıdaki makalelere bakın: 
* [Yönergeleri A'dan B'ye yönleri gösterme](./map-route.md)
* [Trafiği gösterme](./map-show-traffic.md)
