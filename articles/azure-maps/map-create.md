---
title: Azure Haritalar ile bir eşlemesi oluşturma | Microsoft Docs
description: Bir Javascript harita oluşturma
author: jingjing-z
ms.author: jinzh
ms.date: 10/30/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 222fc5e9083c03ff0d4e31927363c5f517cf32a9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62108608"
---
# <a name="create-a-map"></a>Harita oluşturma

Bu makalede, bir harita oluşturmak ve bir harita animasyon için yol gösterir.  

## <a name="understand-the-code"></a>Kodu anlama

Bir harita oluşturmak iki yolu vardır. Merkez noktası belirterek harita kamera ayarlayabilirsiniz ve yakınlaştırma düzeyi veya kamera sınırları harita southwest belirtme ve northeast noktaları sınırlayıcı ayarlayabilirsiniz.

<a id="setCameraOptions"></a>

### <a name="set-the-camera"></a>Kamera ayarlayın

<iframe height='500' scrolling='no' title='Bir harita CameraOptions aracılığıyla oluşturma' src='//codepen.io/azuremaps/embed/qxKBMN/?height=543&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/qxKBMN/'>aracılığıyla bir harita oluşturmak `CameraOptions` </a>tarafından Azure konum tabanlı hizmetler (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kodda bir [harita nesnesi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) aracılığıyla oluşturulan `new atlas.Map()` ve yakınlaştırma ve merkez ayarlayın. Harita özellikleri gibi merkezi ve yakınlaştırma düzeyi parçası olan [CameraOptions](/javascript/api/azure-maps-control/atlas.cameraoptions).

<a id="setCameraBoundsOptions"></a>

### <a name="set-the-camera-bounds"></a>Kamera sınırları ayarlayın

<iframe height='500' scrolling='no' title='Bir harita CameraBoundsOptions aracılığıyla oluşturma' src='//codepen.io/azuremaps/embed/ZrRbPg/?height=543&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/ZrRbPg/'>aracılığıyla bir harita oluşturmak `CameraBoundsOptions` </a>Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kodda bir [harita nesnesi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) aracılığıyla oluşturulan `new atlas.Map()`. Harita özellikleri gibi `CameraBoundsOptions` aracılığıyla tanımlanabilir [setCamera](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) eşlem sınıfı işlevi. Sınırları ve doldurma özellikleri kullanılarak ayarlanır `setCamera`.

### <a name="animate-map-view"></a>Harita görünümünü animasyon ekleme

<iframe height='500' scrolling='no' title='Harita görünümünü animasyon ekleme' src='//codepen.io/azuremaps/embed/WayvbO/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/WayvbO/'>animasyon harita görünümünü</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kodda, ilk kod bloğu oluşturur bir [harita nesnesi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) aracılığıyla `new atlas.Map()`. Harita özellikleri gibi merkezi ve yakınlaştırma düzeyi parçası olan [CameraOptions](/javascript/api/azure-maps-control/atlas.cameraoptions). `CameraOptions` Map Oluşturucusu veya yoluyla tanımlanabilir [setCamera](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) eşlem sınıfı işlevi. [Harita stili](https://docs.microsoft.com/azure/azure-maps/supported-map-styles) ayarlanır `road`.

Harita görünümünü değişiklik tanımlanarak canlandırın bir Canlandır harita işlevi ikinci kod bloğu oluşturur [AnimationOptions](/javascript/api/azure-maps-control/atlas.animationoptions) aracılığıyla [setCamera](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) işlevi. İşlevi, her tıklatma bağlı bir rastgele yakınlaştırma düzeyi oluşturmak için 'Eşlemesi' animasyon düğme tarafından tetiklenir.

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
