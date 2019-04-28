---
author: ramonarguelles
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 1/29/2019
ms.author: rgarcia
ms.openlocfilehash: 228f445dda2724985154723a292adb8215a5ad68
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60342507"
---
Açık **Build Settings** seçerek **dosya** > **Build Settings**.

İçinde **Platform** bölümünden **Android**. Değişiklik **Build System** için **Gradle** seçip **dışarı proje**.

Seçin **anahtar platformu** platforma değiştirmek için **Android**. Unity Android desteği bileşenleri eksik iseler yüklemenizi isteyebilir.

![Unity yapı Ayarları penceresi](./media/spatial-anchors-unity/unity-android-build-settings.png)

Kapat **Build Settings** penceresi.

### <a name="download-and-import-the-arcore-sdk-for-unity"></a>İndirme ve Unity için ARCore SDK içeri aktarma

İndirme `unitypackage` dosya [Unity 1.7 için ARCore SDK sürümleri](https://github.com/google-ar/arcore-unity-sdk/releases/tag/v1.7.0). Unity proje geri seçin **varlıklar** > **paketini içeri aktar** > **özel paket** seçip `unitypackage` dosyası daha önce indirilen. İçinde **Unity paketini içeri aktar** iletişim kutusu, seçin ve tüm dosyaların seçili emin olun **alma**.
