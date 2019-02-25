---
author: ramonarguelles
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 1/29/2019
ms.author: rgarcia
ms.openlocfilehash: 662aced6df27febdf29f2645725962763e89cfa2
ms.sourcegitcommit: e88188bc015525d5bead239ed562067d3fae9822
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/24/2019
ms.locfileid: "56752308"
---
Unity açın ve proje açmak `Unity` klasör.

Açık **Build Settings** seçerek **dosya** -> **Build Settings**.

İçinde **Platform** bölümünden **Android**. Ardından **Build System** için **Gradle** ve **dışarı proje** seçeneği.

Seçin **anahtar platformu** platforma değiştirmek için **Android**. Unity Android desteği bileşenleri eksik iseler yüklemenizi isteyebilir.

![Unity derleme ayarları](./media/spatial-anchors-unity/unity-android-build-settings.png)

Kapat **Build Settings** penceresi.

### <a name="download-and-import-the-arcore-sdk-for-unity"></a>İndirme ve Unity için ARCore SDK içeri aktarma

İndirme `unitypackage` dosya [Unity için ARCore SDK sürümleri](https://github.com/google-ar/arcore-unity-sdk/releases/tag/v1.5.0). Unity proje geri seçin **varlıklar** -> **paketini içeri aktar** -> **özel paket...**  seçip `unitypackage` daha önce indirilen dosya. İçinde **Unity paketini içeri aktar** iletişim kutusunda, seçin ve dosyaların tümünü seçili emin olun **alma**.
