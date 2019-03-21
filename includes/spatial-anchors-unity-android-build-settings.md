---
author: ramonarguelles
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 1/29/2019
ms.author: rgarcia
ms.openlocfilehash: 53f480b8858e2bbe7d4699d8637ecaa5ab0c08a3
ms.sourcegitcommit: 8a59b051b283a72765e7d9ac9dd0586f37018d30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58305319"
---
Unity içinde projeyi açın `Unity` klasör.

Açık **Build Settings** seçerek **dosya** > **Build Settings**.

İçinde **Platform** bölümünden **Android**. Değişiklik **Build System** için **Gradle** seçip **dışarı proje**.

Seçin **anahtar platformu** platforma değiştirmek için **Android**. Unity Android desteği bileşenleri eksik iseler yüklemenizi isteyebilir.

![Unity yapı Ayarları penceresi](./media/spatial-anchors-unity/unity-android-build-settings.png)

Kapat **Build Settings** penceresi.

### <a name="download-and-import-the-arcore-sdk-for-unity"></a>İndirme ve Unity için ARCore SDK içeri aktarma

İndirme `unitypackage` dosya [Unity için ARCore SDK sürümleri](https://github.com/google-ar/arcore-unity-sdk/releases/tag/v1.5.0). Unity proje geri seçin **varlıklar** > **paketini içeri aktar** > **özel paket** seçip `unitypackage` dosyası daha önce indirilen. İçinde **Unity paketini içeri aktar** iletişim kutusu, seçin ve tüm dosyaların seçili emin olun **alma**.
