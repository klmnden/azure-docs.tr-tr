---
title: Hızlı Başlangıç - Azure uzamsal Çıpasıyla Unity Android uygulaması oluşturma | Microsoft Docs
description: Bu hızlı başlangıçta, bir Android uygulaması uzamsal bağlayıcılarını kullanarak Unity ile oluşturmayı öğrenin.
author: craigktreasure
manager: aliemami
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 02/24/2019
ms.topic: quickstart
ms.service: azure-spatial-anchors
ms.openlocfilehash: b35aafdad081a48c0d6048743f87e10c6a6b3a77
ms.sourcegitcommit: e88188bc015525d5bead239ed562067d3fae9822
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/24/2019
ms.locfileid: "56752339"
---
# <a name="quickstart-create-an-android-unity-app-with-azure-spatial-anchors"></a>Hızlı Başlangıç: Azure uzamsal Çıpasıyla Android Unity uygulama oluşturma

Bu hızlı başlangıçta Android Unity kullanarak uygulama oluşturma konusunu kapsar [Azure uzamsal bağlayıcılarını](../overview.md). Azure uzamsal bağlayıcılarını konumlarına cihazlar arasında zaman içinde kalıcı nesneler kullanarak karma gerçeklik deneyimleri oluşturmanıza olanak tanıyan platformlar arası Geliştirici hizmetidir. İşlemi tamamladığınızda, kaydedebilir ve uzamsal bağlantı geri çağırma Unity ile oluşturulmuş bir ARCore Android uygulaması oluşturmuş olacaksınız.

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

- Bir Windows veya Mac OS x ile makine <a href="https://unity3d.com/get-unity/download" target="_blank">Unity 2018.3 +</a> ve <a href="https://developer.android.com/studio/" target="_blank">Android Studio 3.3 +</a> yüklü.
- A <a href="https://developer.android.com/studio/debug/dev-options" target="_blank">etkin Geliştirici</a> ve <a href="https://developers.google.com/ar/discover/supported-devices" target="_blank">ARCore özellikli</a> Android cihaz.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="open-the-sample-project-in-unity"></a>Örnek Proje içinde Unity açın

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

[!INCLUDE [Android Unity Build Settings](../../../includes/spatial-anchors-unity-android-build-settings.md)]

## <a name="configure-account-identifier-and-key"></a>Hesap Kimliği ve anahtarını yapılandırma

İçinde **proje** bölmesinde gidin `Assets/AzureSpatialAnchorsPlugin/Examples` açın `AzureSpatialAnchorsBasicDemo.unity` Sahne dosyası.

[!INCLUDE [Configure Unity Scene](../../../includes/spatial-anchors-unity-configure-scene.md)]

Sahne seçerek Kaydet **dosya** -> **Kaydet**.

## <a name="export-the-android-studio-project"></a>Android Studio projesi dışarı aktarma

[!INCLUDE [Export Unity Project](../../../includes/spatial-anchors-unity-export-project-snip.md)]

Seçin **dışarı** bir iletişim kutusunu açın. Ardından, Android Studio projesi dışarı aktarmak için bir klasör seçin.

Dışarı aktarma tamamlandığında, bir klasör adında bir alt klasör ile verilen bir Android Studio projesi içeren görüntülenecek **HelloAR U3D**.

## <a name="deploy-the-android-application"></a>Android uygulaması dağıtma

Android Studio açıp seçin **var olan bir Android Studio projesini Aç**. Ardından, **HelloAR U3D** alt klasöründen dışarı aktarılan Android Studio projesi seçeneğine tıklayıp **Tamam**.

Açma sırasında Gradle sarmalayıcıyı kullanmak isteyen bir istem görüntülenir. Seçin **Tamam** Gradle sarmalayıcıyı kullanma ve projeyi açın.

Android cihazında güç, oturum açın ve bir USB kablosu kullanarak Bilgisayara bağlanın.

Seçin **çalıştırma** Android Studio araç çubuğundan.

![Android Studio dağıtma ve çalıştırma](./media/get-started-unity-android/android-studio-deploy-run.png)

Android cihazı seçin **dağıtım hedefini seçin** iletişim ve select **Tamam** Android cihazda uygulama çalıştırmak için.

Yerleştirin ve bir bağlantı geri çağırma uygulaması'ndaki yönergeleri izleyin.

Seçerek uygulama Durdur **Durdur** Android Studio araç çubuğundan.

![Android Studio Durdur](./media/get-started-unity-android/android-studio-stop.png)

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

[!INCLUDE [Next steps](../../../includes/spatial-anchors-quickstarts-nextsteps.md)]

> [!div class="nextstepaction"]
> [Öğretici: Cihazlar arasında paylaşımı uzamsal yer işaretleri](../tutorials/tutorial-share-anchors-across-devices.md)
