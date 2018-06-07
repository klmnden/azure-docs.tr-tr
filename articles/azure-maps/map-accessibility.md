---
title: Azure eşlemeleri erişilebilir bir uygulamayla yapma | Microsoft Docs
description: Azure haritalar kullanılarak erişilebilir bir uygulama oluşturma
services: azure-maps
keywords: ''
author: chgennar
ms.author: chgennar
ms.date: 05/18/2018
ms.topic: article
ms.service: azure-maps
documentationcenter: ''
manager: timlt
ms.devlang: na
ms.openlocfilehash: 2523287669628b8c58028cae9887743c20b6bc5e
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655381"
---
# <a name="building-an-accessible-application"></a>Erişilebilir uygulama oluşturma

Bu makalede, ekran okuyucusu tarafından kullanılan bir harita uygulamasının nasıl oluşturulacağını gösterir.

## <a name="understand-the-code"></a>Kodu anlama

<iframe height='500' scrolling='no' title='Erişilebilir bir uygulama' src='//codepen.io/azuremaps/embed/ZoVyZQ/?height=504&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/ZoVyZQ/'>erişilebilir bir uygulama</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Harita bazı erişilebilirlik özellikleri ile önceden oluşturulmuş. Bir kullanıcı için klavyeyi kullanma harita gidebilirsiniz ve ekran okuyucusu çalışıyorsa, harita durumuna değişiklikleri kullanıcıya bildirim gönderir. Örneğin, harita veya yatay olarak kaydırıldığında uzaklaştırılacağını kullanıcı haritanın yeni enlem ve boylam, yakınlaştırma ve yere göre bildirilir. Temel haritada yerleştirilen ek bilgileri ekran okuyucusu kullanıcılar için karşılık gelen metinsel bilgileri edinmeniz gerekir. Kullanarak [açılan](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest) bunu elde etmenin bir yolu. Yukarıdaki arama örnekte haritada yerleştirilen her PIN harita metinsel bilgilerle popup eklenir. Kullanarak [açılan'ın](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest) attach yöntemi sağlar açılan görsel olarak açılan haritada görüntülemeden ekran okuyucusu tarafından görülebilir.

## <a name="next-steps"></a>Sonraki adımlar

Açılan pencere sınıfı ve bu makalede kullanılan kendi yöntemleri hakkında daha fazla bilgi edinin:

* [Açılan](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest)
    * [Ekleme](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#attach)
    * [Kaldır](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#remove)
    * [Açık](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#open)
    * [Kapat](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#close)

Kullanıma bizim [kod örnek sayfası](http://aka.ms/AzureMapsSamples) daha fazla eşleme senaryoları için.
