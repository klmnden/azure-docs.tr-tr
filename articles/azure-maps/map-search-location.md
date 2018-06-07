---
title: Arama sonuçlarını Azure Haritalar ile göster | Microsoft Docs
description: Bir arama yapmak nasıl Azure Eşlemleriyle isteği sonra Javascrip haritada sonuçları görüntüleme
author: jingjing-z
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: b77737b16b23ed00c8f12f84e6a8558a665a7d15
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34599890"
---
# <a name="show-search-results-on-the-map"></a>Arama sonuçları haritasında Göster

Bu makalede bir arama isteği oluşturun ve arama sonuçlarını haritasında Göster gösterilmektedir. 

## <a name="understand-the-code"></a>Kodu anlama

<iframe height='500' scrolling='no' title='Arama sonuçları bir haritasında Göster' src='//codepen.io/azuremaps/embed/KQbaeM/?height=519&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/KQbaeM/'>Göster arama sonuçları bir haritada</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod ilk blok kod eşleme nesnesi oluşturur. Gördüğünüz [bir harita oluşturmak](./map-create.md) yönergeler için.

İkinci kod bloğunu oluşturur ve haritayı aramanın katman sabitler ekler. Gördüğünüz [haritada bir PIN eklemek](./map-add-pin.md) yönergeler için.

Üçüncü kod bloğunu gönderir bir [XMLHttpRequest](https://xhr.spec.whatwg.org/) için [Azure eşlemeleri belirsiz arama API](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy).

Gelen yanıt kodu son bloğunu ayrıştırır. Başarılı yanıt, döndürülen her konumun enlem ve boylam bilgi toplar. Tüm konumu noktaları PIN'ler eşlemesine ekler ve tüm PIN'ler oluşturmak için harita sınırları ayarlar.


## <a name="next-steps"></a>Sonraki adımlar

Sınıfları ve bu makalede kullanılan yöntemleri hakkında daha fazla bilgi edinin: 

* [Benzer arama API Azure eşlemeleri](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy)
* [eşleme](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest)
    * [addPins](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#addpins)
    
Eşlemeleri eklemek daha fazla kod örnekleri için aşağıdaki makalelere bakın: 
* [Bir koordinat bilgi alın](./map-get-information-from-coordinate.md)
* [Yönergeleri A'dan B'ye Göster](./map-route.md)
