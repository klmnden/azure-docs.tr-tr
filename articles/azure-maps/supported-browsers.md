---
title: Web SDK Desteklenen tarayıcılar - Azure haritalar | Microsoft Docs
description: Azure haritalar Web SDK'sı için desteklenen tarayıcılar hakkında bilgi edinin
author: rbrundritt
ms.author: richbrun
ms.date: 03/25/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendleton
ms.openlocfilehash: 84c5dbcf5073ba8c0ae662af019cde590a9adf10
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64686640"
---
# <a name="web-sdk-supported-browsers"></a>Web SDK destekleyen tarayıcılar

Azure haritalar Web SDK'sı olarak adlandırılan bir yardımcı işlevini sağlar [atlas.isSupported](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas?view=azure-iot-typescript-latest#issupported-boolean-). Bu işlev, bir web tarayıcısı WebGL özellikleri yükleniyor ve harita denetimini işleme desteklemek için gereken en düşük ayarlanıp ayarlanmadığını algılar. İşlev ilişkin bir örnek aşağıda verilmiştir:

```
if(!atlas.isSupported()) {
    alert('Your browser is not supported by Azure Maps');
} else if(!atlas.isSupported(true)) {
    alert('Your browser is supported by Azure Maps, but may have major performance caveats.');
} else {
    // Your browser is supported. Add your map code here.
}
```

## <a name="desktop"></a>Masaüstü

Azure haritalar Web SDK'sı aşağıdaki masaüstü tarayıcıları destekler:

- Microsoft Edge (geçerli ve önceki sürüm)
- Google Chrome (geçerli ve önceki sürüm)
- Mozilla Firefox (geçerli ve önceki sürüm)
- Apple Safari (Mac OS X) (geçerli ve önceki sürüm)

Ayrıca bkz: [hedef eski tarayıcılar](#Target-Legacy-Browsers) bu makalenin ilerleyen bölümlerinde.

## <a name="mobile"></a>Cep telefonu

Azure haritalar Web SDK'sı aşağıdaki mobil tarayıcılar destekler:

- Android
  - Geçerli sürümü Chrome Android 6.0 ve üzeri
  - Chrome WebView Android 6.0 ve üzeri
- iOS
  - Geçerli ve önceki ana sürümü üzerinde iOS mobil Safari
  - UIWebView ve geçerli ve önceki ana sürümü üzerinde iOS WKWebView
  - Geçerli sürümü Chrome iOS için

> [!TIP]
> Bir WebView denetimi kullanarak mobil uygulamasına bir harita ekleme yapıyorsanız, kullanmayı tercih edebilirsiniz [Azure haritalar Web SDK'sının npm paketini](https://www.npmjs.com/package/azure-maps-control) yerine Azure içerik teslim üzerinde barındırılan SDK sürümü başvurma Ağ. SDK, kullanıcının cihazında zaten olması ve çalışma zamanında indirilmesi gerekmez çünkü bu yaklaşım yükleme süresini azaltır.

## <a name="nodejs"></a>Node.js

Aşağıdaki Web SDK'sı modüller, node.js'de de desteklenir:

- Hizmetleri modülü ([belgeleri](how-to-use-services-module.md) | [npm modülünü](https://www.npmjs.com/package/azure-maps-rest))

## <a name="Target-Legacy-Browsers"></a>Eski tarayıcılar

Yalnızca bu desteği sınırlı veya, WebGL desteklemeyen eski tarayıcılar hedeflemek isteyebilirsiniz. Azure haritalar Hizmetleri gibi bir açık kaynak harita denetimi ile birlikte kullanmanızı tavsiye ederiz Böyle durumlarda [Leaflet](https://leafletjs.com/). Bir örneği aşağıda verilmiştir:

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Azure haritalar + Leaflet" src="//codepen.io/azuremaps/embed/GeLgyx/?height=500&theme-id=0&default-tab=html,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Kalem bkz <a href='https://codepen.io/azuremaps/pen/GeLgyx/'>Azure haritalar + Leaflet</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>


## <a name="next-steps"></a>Sonraki adımlar

Azure haritalar Web SDK'sı hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Harita denetimi](how-to-use-map-control.md)

> [!div class="nextstepaction"]
> [Hizmetleri modülü](how-to-use-services-module.md)
