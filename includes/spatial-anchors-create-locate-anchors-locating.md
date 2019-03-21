---
ms.openlocfilehash: 430302d8b1b2a01febbbe2f11057bb331ec80c28
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/18/2019
ms.locfileid: "57907810"
---
## <a name="locate-a-cloud-spatial-anchor"></a>Bir bulut uzamsal bağlayıcı bulun

Uzamsal uygulamasının bulut bağlayıcılarını bulmak için kendi tanımlayıcılarının bilmeniz gerekir. Yer işareti kimlikleri, uygulamanızın arka uç hizmetinde depolanan ve kendisine düzgün doğrulanabilir tüm cihazlar için erişilebilir olabilir. Bunu bir örnek [Öğreticisi: Uzamsal bağlayıcılarını cihazlar arasında paylaşmak](/azure/spatial-anchors/tutorials/tutorial-share-anchors-across-devices/).

AnchorLocateCriteria nesnesi, aradığınız ve, AnchorLocateCriteria sağlayarak oturum CreateWatcher yöntemi Çağır tanımlayıcıları olarak ayarlayın.
