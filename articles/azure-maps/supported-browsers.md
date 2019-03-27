---
title: Desteklenen Web tarayıcıları - Azure haritalar | Microsoft Docs
description: Azure haritalar Web SDK'sı için desteklenen web tarayıcıları hakkında bilgi edinin
author: rbrundritt
ms.author: richbrun
ms.date: 03/25/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendleton
ms.openlocfilehash: 2678fa7c9290c7ec0a27288e7739fde4bb0870fd
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58501279"
---
# <a name="supported-web-browsers"></a>Desteklenen web tarayıcıları

Azure haritalar Web SDK'sı yardımcı işlevini sağlar [atlas.isSupported](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas?view=azure-iot-typescript-latest#issupported-boolean-) en az bir web tarayıcısı olup olmadığını algılamak için WebGL özellikleri yükleniyor ve harita denetiminin işleme desteklemek için gereklidir. 

```
if(!atlas.isSupported()) {
    alert('Your browser is not supported by Azure Maps');
} else if(!atlas.isSupported(true)) {
    alert('Your browser is supported by Azure Maps, but may have major performance caveats.');
} else {
    //Add your map code here.
}
```

Azure haritalar Web SDK'sı, aşağıdaki web tarayıcılarını destekler.

## <a name="desktop"></a>Masaüstü

- Microsoft Edge geçerli ve önceki sürümü 
- Chrome geçerli ve önceki sürümü 
- Firefox geçerli ve önceki sürümü 
- Safari (Mac OS X) geçerli ve önceki sürümü 

Ayrıca bkz: [hedef eski tarayıcılar](#Target-Legacy-Browsers).

## <a name="mobile"></a>Cep telefonu

-  Android
    * Geçerli sürümünde Chrome Android 6.0 ve üzeri
    * Chrome WebView üzerinde Android 6.0 ve üzeri
- iOS
    * Geçerli ve önceki ana sürümü üzerinde iOS mobil Safari
    * UIWebView ve geçerli ve önceki ana sürümü üzerinde iOS WKWebView
    * Geçerli sürümü Chrome iOS için

> [!TIP]
> Bir eşlem içinde bir mobil uygulama görünüm WebView denetiminde ekliyorsanız kullanarak bulabilirsiniz [Azure haritalar Web SDK'sının npm paketini](https://www.npmjs.com/package/azure-maps-control) tercih SDK'sının barındırılan CDN sürümüne başvurmayı sürdürmesi için. SDK'sı kullanıcının cihazda zaten olması ve çalışma zamanında yüklenmesine gerek kalmayacak şekilde bu yükleme süresini azaltır.

## <a name="nodejs"></a>Node.js

Aşağıdaki Web SDK'sı modüller, node.js'de de desteklenir:

- Hizmetleri modülü ([belgeleri](how-to-use-services-module.md) | [npm modülünü](https://www.npmjs.com/package/azure-maps-rest))

## <a name="Target-Legacy-Browsers"></a>Eski tarayıcılar

Desteklemediği veya sınırlı destek WebGL için eski web tarayıcıları hedeflemek gerekiyorsa gibi Azure haritalar Hizmetleri bir açık kaynak harita denetimi ile birlikte kullanmak önerilir [leaflet](https://leafletjs.com/). 

<iframe height="500" style="width: 100%;" scrolling="no" title="Azure haritalar + Leaflet" src="//codepen.io/azuremaps/embed/GeLgyx/?height=500&theme-id=0&default-tab=html,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Kalem bkz <a href='https://codepen.io/azuremaps/pen/GeLgyx/'>Azure haritalar + Leaflet</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>