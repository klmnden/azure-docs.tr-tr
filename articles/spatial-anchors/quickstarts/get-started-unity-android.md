---
title: Hızlı Başlangıç - Azure uzamsal Çıpasıyla bir Unity Android uygulaması oluşturma | Microsoft Docs
description: Bu hızlı başlangıçta, bir Android uygulaması uzamsal bağlayıcılarını kullanarak Unity ile oluşturmayı öğrenin.
author: craigktreasure
manager: aliemami
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 02/24/2019
ms.topic: quickstart
ms.service: azure-spatial-anchors
ms.openlocfilehash: 642abfb99b40d67802b7194ad225ebcd2872a72b
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "67135072"
---
# <a name="quickstart-create-a-unity-android-app-with-azure-spatial-anchors"></a>Hızlı Başlangıç: Azure uzamsal Çıpasıyla bir Unity Android uygulaması oluşturma

Bu hızlı başlangıçta kullanarak bir Unity Android uygulaması oluşturmak nasıl etkinleştireceğinizi de açıklar [Azure uzamsal bağlayıcılarını](../overview.md). Azure uzamsal bağlayıcılarını konumlarına cihazlar arasında zaman içinde kalıcı nesneler kullanarak karma gerçeklik deneyimleri oluşturmanıza olanak tanıyan platformlar arası Geliştirici hizmetidir. İşlemi tamamladığınızda, kaydedebilir ve uzamsal bağlantı geri çağırma Unity ile oluşturulmuş bir ARCore Android uygulaması oluşturmuş olacaksınız.

Şunları öğrenirsiniz:

> [!div class="checklist"]
> * Uzamsal bağlayıcılarını hesabı oluşturma
> * Unity derleme ayarlarını hazırlama
> * İndirme ve Unity için ARCore SDK içeri aktarma
> * Uzamsal bağlayıcılarını hesap tanımlayıcısı ve hesap anahtarını yapılandırma
> * Android Studio projesi dışarı aktarma
> * Bir Android cihazda çalıştırma ve dağıtma

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlamak için aşağıdakileri yaptığınızdan emin olun:

- Bir Windows veya Mac OS x ile makine <a href="https://unity3d.com/get-unity/download" target="_blank">Unity 2018.3 +</a> ve <a href="https://developer.android.com/studio/" target="_blank">Android Studio 3.3 +</a>.
  - Windows üzerinde çalışır, ayrıca gerekir <a href="https://git-scm.com/download/win" target="_blank">Git için Windows</a>.
  - Macos'ta çalıştırılıyorsa, Git HomeBrew yüklü alın. Tek satırlık bir Terminal içinde aşağıdaki komutu girin: `/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`. Ardından çalıştırın `brew install git`.
- A <a href="https://developer.android.com/studio/debug/dev-options" target="_blank">etkin Geliştirici</a> ve <a href="https://developers.google.com/ar/discover/supported-devices" target="_blank">ARCore özellikli</a> Android cihaz.
- Uygulamanızın sürümünü kullanmalısınız **1.7** Unity için ARCore SDK'sının.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="download-and-open-the-unity-sample-project"></a>İndir ve Unity örnek proje Aç

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

[!INCLUDE [Open Unity Project](../../../includes/spatial-anchors-open-unity-project.md)]

[!INCLUDE [Android Unity Build Settings](../../../includes/spatial-anchors-unity-android-build-settings.md)]

## <a name="configure-account-identifier-and-key"></a>Hesap Kimliği ve anahtarını yapılandırma

İçinde **proje** bölmesinde gidin `Assets/AzureSpatialAnchorsPlugin/Examples` açın `AzureSpatialAnchorsBasicDemo.unity` Sahne dosyası.

[!INCLUDE [Configure Unity Scene](../../../includes/spatial-anchors-unity-configure-scene.md)]

Sahne seçerek Kaydet **dosya** -> **Kaydet**.

## <a name="export-the-android-studio-project"></a>Android Studio projesi dışarı aktarma

[!INCLUDE [Export Unity Project](../../../includes/spatial-anchors-unity-export-project-snip.md)]

Olun **dışarı proje** onay kutusu bir onay işareti yok. Tıklayın **derleme ve çalıştırma**. Kaydetmeniz istenir, `.apk` dosyası için bir ad seçim yapabilirsiniz.

Yerleştirin ve bir bağlantı geri çağırma uygulaması'ndaki yönergeleri izleyin.

> [!NOTE]
> Kamera arka planı olarak (veya diğer doku örneği yerine boş, mavi görürsünüz) görmüyorsanız, uygulama çalıştırırken daha sonra büyük olasılıkla Unity varlıkları yeniden almanız gerekir. Uygulamayı durdurun. Unity üstteki menüden seçin **varlıklar -> eşleşmediğinden yeniden içe aktarım tüm**. Ardından, uygulamayı yeniden çalıştırın.

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

[!INCLUDE [Next steps](../../../includes/spatial-anchors-quickstarts-nextsteps.md)]

> [!div class="nextstepaction"]
> [Öğretici: Cihazlar arasında paylaşımı uzamsal yer işaretleri](../tutorials/tutorial-share-anchors-across-devices.md)
