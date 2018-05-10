---
title: Azure Eşlemleriyle bir harita oluşturmak | Microsoft Docs
description: Bir Javascript eşlemesi oluşturma
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
ms.openlocfilehash: 9a7c611860a6d32f82d6714d945c62e6c562f91f
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="create-a-map"></a>Bir eşleme oluşturma

Bu makalede bir harita oluşturmak nasıl gösterir.  

## <a name="understand-the-code"></a>Kodu anlama

Bir harita oluşturabileceğiniz iki yolu vardır. Orta noktası belirterek harita kamera ayarlamak ve yakınlaştırma düzeyi veya noktası ve noktası sınırlayıcı Kuzeydoğu sınırlayıcı Güneybatı belirterek harita kamera sınırları ayarlayın.

<a id="setCameraOptions"></a>

### <a name="setting-the-camera"></a>Kamera ayarlama

<iframe height='310' scrolling='no' title='CameraOptions aracılığıyla bir harita oluşturmak' src='//codepen.io/azuremaps/embed/qxKBMN/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/qxKBMN/'>CameraOptions aracılığıyla bir harita oluşturmak</a> tarafından Azure lb (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod bir [harita nesnesi](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest) ile oluşturulan `new atlas.Map()`. Harita özellikleri merkezi ve yakınlaştırma düzeyi gibi parçası olan [CameraOptions](https://docs.microsoft.com/javascript/api/azure-maps-javascript/cameraoptions?view=azure-iot-typescript-latest). Map Oluşturucusu veya aracılığıyla CameraOptions tanımlanabilir [setCamera](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#setcamera) harita sınıfının işlevi.

<a id="setCameraBoundsOptions"></a>

### <a name="setting-the-camera-bounds"></a>Kamera sınırları ayarlama

<iframe height='310' scrolling='no' title='CameraBoundsOptions aracılığıyla bir harita oluşturmak' src='//codepen.io/azuremaps/embed/ZrRbPg/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/ZrRbPg/'>CameraBoundsOptions aracılığıyla bir harita oluşturmak</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod bir [harita nesnesi](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest) aracılığıyla oluşturulan `new atlas.Map()`. Sınırlayıcı kutu gibi eşlemesi özellikleri parçası olan [CameraBoundsOptions](https://docs.microsoft.com/javascript/api/azure-maps-javascript/cameraboundsoptions?view=azure-iot-typescript-latest). CameraBoundsOptions aracılığıyla tanımlanabilir [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#setcamerabounds) harita sınıfının işlevi.

## <a name="try-out-the-code"></a>Kodu deneyin 

Yukarıdaki örnek kod göz atın. JavaScript kodu sol JS sekmesinde düzenleyin ve sağ taraftaki Sonuç sekmesindeki harita görünümü değişiklikleri görmek. Ayrıca, "CodePen üzerinde Düzenle" düğmesine tıklayın ve CodePen kodda düzenleyin. 

<a id="relatedReference"></a>

## <a name="next-steps"></a>Sonraki adımlar

Sınıfları ve bu makalede kullanılan yöntemleri hakkında daha fazla bilgi edinin: 
* [eşleme](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest)
    * [setCamera](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#setcamera)
    * [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#setcamerabounds)

