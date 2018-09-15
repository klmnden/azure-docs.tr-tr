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
ms.openlocfilehash: 0e292bfd38402d5a36be217746fd352ce4627177
ms.sourcegitcommit: ab9514485569ce511f2a93260ef71c56d7633343
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2018
ms.locfileid: "45633325"
---
# <a name="create-a-map"></a>Harita oluşturma

Bu makalede bir harita oluşturma işlemini gösterir.  

## <a name="understand-the-code"></a>Kodu anlama

Bir harita oluşturmak iki yolu vardır. Merkez noktasını harita kamera ayarlamak ve yakınlaştırma düzeyi veya nokta ve sınırlama noktası Kuzeydoğu sınırlayıcı southwest belirterek harita kamera sınırları ayarlayın.

<a id="setCameraOptions"></a>

### <a name="setting-the-camera"></a>Kamera ayarlama

<iframe height='310' scrolling='no' title='Bir harita CameraOptions aracılığıyla oluşturma' src='//codepen.io/azuremaps/embed/qxKBMN/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/qxKBMN/'>CameraOptions aracılığıyla bir harita oluşturmak</a> Azure LBS tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kodda bir [harita nesnesi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) aracılığıyla oluşturulan `new atlas.Map()`. Harita özellikleri gibi merkezi ve yakınlaştırma düzeyi parçası olan [CameraOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/models.cameraoptions?view=azure-iot-typescript-latest). Map Oluşturucusu veya yoluyla CameraOptions tanımlanabilir [setCamera](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamera) eşlem sınıfı işlevi.

<a id="setCameraBoundsOptions"></a>

### <a name="setting-the-camera-bounds"></a>Kamera sınırları ayarlama

<iframe height='310' scrolling='no' title='Bir harita CameraBoundsOptions aracılığıyla oluşturma' src='//codepen.io/azuremaps/embed/ZrRbPg/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/ZrRbPg/'>CameraBoundsOptions aracılığıyla bir harita oluşturmak</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kodda bir [harita nesnesi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) aracılığıyla oluşturulan `new atlas.Map()`. Sınırlayıcı kutu gibi haritasının özelliklerini parçası olan [CameraBoundsOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/models.cameraboundsoptions?view=azure-iot-typescript-latest). CameraBoundsOptions aracılığıyla tanımlanabilir [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamerabounds) eşlem sınıfı işlevi.

## <a name="try-out-the-code"></a>Kodu deneyin

Yukarıdaki örnek kod göz atın. Sol taraftaki JS sekmesinde JavaScript kodunu düzenleyin ve sağdaki sonucu sekmesindeki harita görünümü değişiklikleri görmek. Ayrıca, "üzerinde CodePen Düzenle" düğmesine tıklayın ve CodePen kodunu düzenleyin.

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
