---
title: Azure Haritalar ile erişilebilir bir uygulama yapma | Microsoft Docs
description: Azure haritalar kullanan erişilebilir bir uygulama oluşturma
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
ms.openlocfilehash: 537a8c80dc0d1fcb2f536d0e30200de19a2111a4
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2018
ms.locfileid: "37867062"
---
# <a name="building-an-accessible-application"></a>Erişilebilir bir uygulama oluşturma

Bu makalede, ekran okuyucu tarafından kullanılabilecek bir harita uygulaması oluşturma işlemini göstermektedir.

## <a name="understand-the-code"></a>Kodu anlama

<iframe height='500' scrolling='no' title='Erişilebilir bir uygulama yapın' src='//codepen.io/azuremaps/embed/ZoVyZQ/?height=504&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/ZoVyZQ/'>erişilebilir bir uygulama</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Bazı erişilebilirlik özellikleri ile önceden oluşturulmuş haritasıdır. Bir kullanıcı için klavyeyi kullanma harita gidebilirsiniz ve ekran okuyucu çalışıyorsa, eşleme durumunu yapılan değişikliklerin kullanıcıya bildirir. Örneğin, haritayı veya yatay olarak kaydırıldığında uzaklaştırılacağını kullanıcı haritanın yeni enlem, boylam, yakınlaştırma ve Yerleşim Yeri bildirilir. Temel harita üzerinde yerleştirilen ek bilgiler, ekran okuyucusu kullanıcıları için karşılık gelen metin tabanlı bilgiler olması gerekir. Kullanarak [açılan](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest) bunu yapmanın bir yoludur. Yukarıdaki arama örnekte açılır penceresi ile metin tabanlı bilgiler harita üzerinde yerleştirilen her PIN eşlemesine eklenir. Kullanarak [açılan'ın](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest) ekleme yöntemi açılan görsel olarak haritada görüntülemeden ekran okuyucu tarafından görülebilmesi açılan sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Açılan pencere sınıfı ve bu makalede kullanılan yöntemlerinden hakkında daha fazla bilgi edinin:

* [Açılan menüsü](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest)
    * [ekleme](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#attach)
    * [Kaldır](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#remove)
    * [açın](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#open)
    * [Kapat](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#close)

Kullanıma sunduğumuz [kod örnek sayfası](http://aka.ms/AzureMapsSamples) daha fazla eşleme senaryolar için.
