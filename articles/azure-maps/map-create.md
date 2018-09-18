---
title: Azure Haritalar ile bir eşlemesi oluşturma | Microsoft Docs
description: Bir Javascript harita oluşturma
author: jingjing-z
ms.author: jinzh
ms.date: 09/14/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 9759c4149c6b026837e550dcf3ab0a0156bbb736
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/17/2018
ms.locfileid: "45730018"
---
# <a name="create-a-map"></a>Harita oluşturma

Bu makalede bir harita oluşturma işlemini gösterir.  

## <a name="understand-the-code"></a>Kodu anlama

Bir harita oluşturmak iki yolu vardır. Merkez noktası belirterek harita kamera ayarlayabilir ve yakınlaştırma düzeyi. Güneybatı noktası sınırlama ve northeast noktası sınırlayıcı belirtme eşlemenin kamera sınırları ayarlayın.

<a id="setCameraOptions"></a>

### <a name="set-the-camera"></a>Kamera ayarlayın

<iframe height='310' scrolling='no' title='Bir harita CameraOptions aracılığıyla oluşturma' src='//codepen.io/azuremaps/embed/qxKBMN/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/qxKBMN/'>aracılığıyla bir harita oluşturmak `CameraOptions` </a>tarafından Azure konum tabanlı hizmetler (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kodda bir [harita nesnesi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) aracılığıyla oluşturulan `new atlas.Map()`. Harita özellikleri gibi merkezi ve yakınlaştırma düzeyi parçası olan [CameraOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/models.cameraoptions?view=azure-iot-typescript-latest). `CameraOptions` Map Oluşturucusu veya yoluyla tanımlanabilir [setCamera](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamera) eşlem sınıfı işlevi.

<a id="setCameraBoundsOptions"></a>

### <a name="set-the-camera-bounds"></a>Kamera sınırları ayarlayın

<iframe height='310' scrolling='no' title='Bir harita CameraBoundsOptions aracılığıyla oluşturma' src='//codepen.io/azuremaps/embed/ZrRbPg/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/ZrRbPg/'>aracılığıyla bir harita oluşturmak `CameraBoundsOptions` </a>Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kodda bir [harita nesnesi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) aracılığıyla oluşturulan `new atlas.Map()`. Sınırlayıcı kutu gibi haritasının özelliklerini parçası olan [CameraBoundsOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/models.cameraboundsoptions?view=azure-iot-typescript-latest). `CameraBoundsOptions` aracılığıyla tanımlanabilir [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamerabounds) eşlem sınıfı işlevi.

## <a name="try-out-the-code"></a>Kodu deneyin

Yukarıdaki örnek kod göz atın. JavaScript kodu düzenleyebileceğiniz **JS sekmesini** harita görünümünü değişiklikleri esas bakın ve sol **sonucu sekmesini** sağ. De tıklayabilirsiniz **CodePen üzerinde düzenleme** düğmesi ve CodePen kodu düzenleyin.

<a id="relatedReference"></a>

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan yöntemleri ve sınıfları hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Harita](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

Uygulamanıza işlev eklemek için kod örnekleri bakın:

> [!div class="nextstepaction"]
> [Harita stil seçin](choose-map-style.md)

> [!div class="nextstepaction"]
> [Eşleme denetimleri ekleme](map-add-controls.md)
