---
author: ramonarguelles
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 1/29/2019
ms.author: rgarcia
ms.openlocfilehash: 9798e5f76881be38fb27e1f428565caba6e50bf2
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "67135132"
---
Açık **Build Settings** seçerek **dosya** > **Build Settings**.

İçinde **Platform** bölümünden **Android**. Değişiklik **Build System** için **Gradle** olun **dışarı proje** onay kutusu bir onay işareti yok.

Seçin **anahtar platformu** platforma değiştirmek için **Android**. Unity Android desteği bileşenleri eksik iseler yüklemenizi isteyebilir.

![Unity yapı Ayarları penceresi](./media/spatial-anchors-unity/unity-android-build-settings.png)

Kapat **Build Settings** penceresi.

### <a name="download-and-import-the-arcore-sdk-for-unity"></a>İndirme ve Unity için ARCore SDK içeri aktarma

İndirme `unitypackage` dosya [Unity 1.7 için ARCore SDK sürümleri](https://github.com/google-ar/arcore-unity-sdk/releases/tag/v1.7.0). Unity proje geri seçin **varlıklar** > **paketini içeri aktar** > **özel paket** seçip `unitypackage` dosyası daha önce indirilen. İçinde **Unity paketini içeri aktar** iletişim kutusu, seçin ve tüm dosyaların seçili emin olun **alma**.
