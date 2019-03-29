---
author: ramonarguelles
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 1/29/2019
ms.author: rgarcia
ms.openlocfilehash: 355d89b1fd7d7cc8c9db58122d51c6ff7dff5f3b
ms.sourcegitcommit: f8c592ebaad4a5fc45710dadc0e5c4480d122d6f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58622595"
---
Unity içinde projeyi açın `Unity` klasör.

Açık **Build Settings** seçerek **dosya** > **Build Settings**.

İçinde **Platform** bölümünden **Android**. Değişiklik **Build System** için **Gradle** seçip **dışarı proje**.

Seçin **anahtar platformu** platforma değiştirmek için **Android**. Unity Android desteği bileşenleri eksik iseler yüklemenizi isteyebilir.

![Unity yapı Ayarları penceresi](./media/spatial-anchors-unity/unity-android-build-settings.png)

Kapat **Build Settings** penceresi.

### <a name="download-and-import-the-arcore-sdk-for-unity"></a>İndirme ve Unity için ARCore SDK içeri aktarma

İndirme `unitypackage` dosya [ARCore SDK'sı için Unity 1.5 sürümleri](https://github.com/google-ar/arcore-unity-sdk/releases/tag/v1.5.0). Unity proje geri seçin **varlıklar** > **paketini içeri aktar** > **özel paket** seçip `unitypackage` dosyası daha önce indirilen. İçinde **Unity paketini içeri aktar** iletişim kutusu, seçin ve tüm dosyaların seçili emin olun **alma**.
