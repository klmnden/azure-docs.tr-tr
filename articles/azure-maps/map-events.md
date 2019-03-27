---
title: Azure Haritalar ile fare olayları işlemek | Microsoft Docs
description: Harita olayları ile etkileşimli bir Javascript harita yapma
author: jingjing-z
ms.author: jinzh
ms.date: 11/29/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 4fce8eae25942d098bb3f3277938bfaa3dafa00b
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58499208"
---
# <a name="interact-with-the-map---mouse-events"></a>Eşleme - fare olayları ile etkileşim kurma

Bu makalede nasıl kullanılacağını gösterir [harita sınıfı](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) [olayları](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) olayları harita üzerinde ve harita farklı katmanlardaki vurgulamak için özellik. Ayrıca, harita sınıf olayları özelliği bir HTML işaretçisi ile etkileşim kurduğunuzda olayları vurgulamak için nasıl kullanılacağını gösterir.

## <a name="interact-with-the-map"></a>Haritayla etkileşim kurabilir

<iframe height='600' scrolling='no' title='Harita – fare olayları ile etkileşim kurma' src='//codepen.io/azuremaps/embed/bLZEWd/?height=600&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/bLZEWd/'>Haritası – fare olayları ile etkileşimde bulunma</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Harita yukarıdaki yürütmek ve sağ tarafta vurgulanan ilişkili fare olayları görebilirsiniz. Tıklayabilirsiniz **JS sekmesini** görüntüleme ve JavaScript kodunu düzenleyin. De tıklayabilirsiniz **CodePen üzerinde düzenleme** düğmesi ve CodePen üzerinde kod düzenleyin.

## <a name="interact-with-map-layers"></a>Harita katmanları ile etkileşim kurma

<iframe height='600' scrolling='no' title='Harita – katman olayları etkileşim kurma' src='//codepen.io/azuremaps/embed/bQRRPE/?height=600&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/bQRRPE/'>Haritası – katman olayları etkileşim</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod, sembol katman ile etkileşime geçtiğimiz sırada yukarı hazırlanın olayları adını vurgular. Tüm sembol, kabarcık, satır ve Çokgen katmanı aynı olay kümesini destekler. Döşeme katmanı bu olayları desteklemez.

## <a name="interact-with-html-marker"></a>HTML işaretçisi ile etkileşim kurma

<iframe height='500' scrolling='no' title='Harita - HTML işaret olayları etkileşim kurma' src='//codepen.io/azuremaps/embed/VVzKJY/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/VVzKJY/'>eşleme - HTML işaret olayları etkileşim</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kod, bir HTML işaretçiye Javascript harita olaylar ekler. Ayrıca, HTML işaretçisi ile etkileşime geçtiğimiz sırada yukarı hazırlanın olayları adını vurgular.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan yöntemleri ve sınıfları hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Harita](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

Tam kod örnekleri için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Azure haritalar Hizmetleri Modülünü Kullanma](./how-to-use-services-module.md)

> [!div class="nextstepaction"]
> [Kod örnek sayfası](https://aka.ms/AzureMapsSamples)
