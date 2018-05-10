---
title: Koordinat Azure Haritalar ile ilgili bilgileri göster | Microsoft Docs
description: Bir kullanıcı bir koordinat seçtiğinde haritada bir adresi hakkındaki bilgileri görüntülemek nasıl
services: azure-maps
keywords: ''
author: jinzh-azureiot
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: article
ms.service: azure-maps
documentationcenter: ''
manager: timlt
ms.devlang: na
ms.custom: codepen
ms.openlocfilehash: bb8644724cc872a0a8bc331e76251218492fd93d
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="get-information-from-a-coordinate"></a>Bir koordinat bilgi alın

Bu makalede bir ters adresi arama yapmak ve fare tıklatma tıklatılan konumunun adresi açılan penceresinde Göster gösterilmektedir. 

## <a name="understand-the-code"></a>Kodu anlama

<iframe height='500' scrolling='no' title='Bir koordinat bilgi alın' src='//codepen.io/azuremaps/embed/ddXzoB/?height=516&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/ddXzoB/'>koordinat bilgi alma</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod ilk blok kod eşleme nesnesi oluşturur. Gördüğünüz [bir harita oluşturmak](./map-create.md) yönergeler için.

İkinci kod bloğunu fare imlecini stilini gösteren bir işaretçi güncelleştirir.

Üçüncü kod bloğunu popup oluşturur. Gördüğünüz [popup haritada eklemek](./map-add-popup.md) yönergeler için.

Son kod bloğu fare tıklamaları için bir olay dinleyicisi ekler. Fare tıklatma gönderir bir [XMLHttpRequest](https://xhr.spec.whatwg.org/) için [Azure eşlemeleri ters adres arama API](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse). Başarılı bir yanıt için tıklatılan konumu adresini toplar ve açılan içerik ve konum üzerinden tanımlar [setPopupOptions](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#setpopupoptions) açılan sınıfının işlevi

## <a name="next-steps"></a>Sonraki adımlar

Sınıfları ve bu makalede kullanılan yöntemleri hakkında daha fazla bilgi edinin: 
* [Geriye doğru adresi arama](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse)
* [eşleme](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest)
    * [addEventListener](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#addeventlistener)
* [Açılan](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest)
    * [setPopupOptions](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#setpopupoptions)
    * [Açık](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#open)
    * [Kapat](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#close)
