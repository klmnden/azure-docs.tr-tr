---
ms.openlocfilehash: 52dfbfca5f79a7f92848ea39eddc00aa10f05ff1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60232621"
---
## <a name="locate-a-cloud-spatial-anchor"></a>Bir bulut uzamsal bağlayıcı bulun

Daha önce yüklenen bulut uzamsal bağlantı bulmak için Azure uzamsal bağlayıcılarını kitaplığını kullanarak prime nedenlerini biridir. Uzamsal uygulamasının bulut bağlayıcılarını bulmak için kendi tanımlayıcılarının bilmeniz gerekir. Yer işareti kimlikleri, uygulamanızın arka uç hizmeti ve düzgün şekilde doğrulanabilir tüm cihazlar için erişilebilir depolanabilir. Bunu bir örnek [Öğreticisi: Uzamsal bağlayıcılarını cihazlar arasında paylaşmak](/azure/spatial-anchors/tutorials/tutorial-share-anchors-across-devices/).

Örneği bir `AnchorLocateCriteria` nesne, aradığınız ve çağırmak tanımlayıcıları Kümesi `CreateWatcher` yöntemi sağlayarak oturum, `AnchorLocateCriteria`.
