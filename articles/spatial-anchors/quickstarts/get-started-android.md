---
title: Hızlı Başlangıç - Azure uzamsal Çıpasıyla Android uygulaması oluşturma | Microsoft Docs
description: Bu hızlı başlangıçta, uzamsal bağlayıcılarını kullanarak bir Android uygulamasının nasıl oluşturulacağını öğrenin.
author: craigktreasure
manager: aliemami
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 02/24/2019
ms.topic: quickstart
ms.service: azure-spatial-anchors
ms.openlocfilehash: 03589745e6e9b40b937c49162e99035ce6c81423
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65757843"
---
# <a name="quickstart-create-an-android-app-with-azure-spatial-anchors"></a>Hızlı Başlangıç: Azure uzamsal Çıpasıyla Android uygulaması oluşturma

Bu hızlı başlangıçta kullanarak bir Android uygulaması oluşturmak nasıl etkinleştireceğinizi de açıklar [Azure uzamsal bağlayıcılarını](../overview.md) ya da Java veya C++/NDK. Azure uzamsal bağlayıcılarını konumlarına cihazlar arasında zaman içinde kalıcı nesneler kullanarak karma gerçeklik deneyimleri oluşturmanıza olanak tanıyan platformlar arası Geliştirici hizmetidir. İşlemi tamamladığınızda, kaydedin ve uzamsal bağlantı Hatırlayacağınız bir ARCore Android uygulaması oluşturmuş olacaksınız.

Şunları öğrenirsiniz:

> [!div class="checklist"]
> * Uzamsal bağlayıcılarını hesabı oluşturma
> * Uzamsal bağlayıcılarını hesap tanımlayıcısı ve hesap anahtarını yapılandırma
> * Bir Android cihazda çalıştırma ve dağıtma

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlamak için aşağıdakileri yaptığınızdan emin olun:

- Bir Windows veya Mac OS x ile makine <a href="https://developer.android.com/studio/" target="_blank">Android Studio 3.3 +</a>.
  - Windows üzerinde çalışır, ayrıca gerekir <a href="https://git-scm.com/download/win" target="_blank">Git için Windows</a>.
  - Macos'ta çalıştırılıyorsa, Git HomeBrew yüklü alın. Tek satırlık bir Terminal içinde aşağıdaki komutu girin: `/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`. Ardından çalıştırın `brew install git`.
  - NDK örneği oluşturmak için Android Studio'da CMake 3.6 SDK Tools ve NDK yüklemek gerekir.
- A <a href="https://developer.android.com/studio/debug/dev-options" target="_blank">etkin Geliştirici</a> ve <a href="https://developers.google.com/ar/discover/supported-devices" target="_blank">ARCore özellikli</a> Android cihaz.
- Uygulamanızı ARCore 1.7 hedeflemesi gerekir.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="open-the-sample-project"></a>Örnek projeyi açın

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

Android NDK örnek oluşturuyorsanız, indirmeniz gerekir `arcore_c_api.h` gelen [burada](https://raw.githubusercontent.com/google-ar/arcore-android-sdk/v1.7.0/libraries/include/arcore_c_api.h) ve yerleştirebilir `Android\NDK\libraries\include`.

Android Studio’yu açın.

# <a name="javatabopenproject-java"></a>[Java](#tab/openproject-java)

Seçin **var olan bir Android Studio projesini Aç** konumundaki projeyi seçip `Android/Java/`.

# <a name="ndktabopenproject-ndk"></a>[NDK](#tab/openproject-ndk)

Seçin **var olan bir Android Studio projesini Aç** konumundaki projeyi seçip `Android/NDK/`.

***

## <a name="configure-account-identifier-and-key"></a>Hesap Kimliği ve anahtarını yapılandırma

Sonraki adım uygulamayı hesap tanımlayıcısı ve hesap anahtarını kullanacak şekilde yapılandırmaktır. Bir metin düzenleyiciye kopyaladığınız zaman [uzamsal bağlayıcılarını kaynağı ayarı](#create-a-spatial-anchors-resource).

# <a name="javatabopenproject-java"></a>[Java](#tab/openproject-java)

Açık `Android/Java/app/src/main/java/com/microsoft/sampleandroid/AzureSpatialAnchorsActivity.java`.

Bulun `SpatialAnchorsAccountKey` değiştirin ve alan `Set me` ile hesap anahtarı.

Bulun `SpatialAnchorsAccountId` değiştirin ve alan `Set me` hesap tanımlayıcı ile.

# <a name="ndktabopenproject-ndk"></a>[NDK](#tab/openproject-ndk)

Açık `Android/NDK/app/src/main/cpp/AzureSpatialAnchorsApplication.cpp`.

Bulun `SpatialAnchorsAccountKey` değiştirin ve alan `Set me` ile hesap anahtarı.

Bulun `SpatialAnchorsAccountId` değiştirin ve alan `Set me` hesap tanımlayıcı ile.

***

## <a name="deploy-the-app-to-your-android-device"></a>Android cihazınıza uygulaması dağıtma

Android cihazında güç, oturum açın ve bir USB kablosu kullanarak Bilgisayara bağlanın.

Seçin **çalıştırma** Android Studio araç çubuğundan.

![Android Studio dağıtma ve çalıştırma](./media/get-started-android/android-studio-deploy-run.png)

Android cihazı seçin **dağıtım hedefini seçin** iletişim ve select **Tamam** Android cihazda uygulama çalıştırmak için.

Yerleştirin ve bir bağlantı geri çağırma uygulaması'ndaki yönergeleri izleyin.

Seçerek uygulama Durdur **Durdur** Android Studio araç çubuğundan.

![Android Studio Durdur](./media/get-started-android/android-studio-stop.png)

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

[!INCLUDE [Next steps](../../../includes/spatial-anchors-quickstarts-nextsteps.md)]

> [!div class="nextstepaction"]
> [Öğretici: Cihazlar arasında paylaşımı uzamsal yer işaretleri](../tutorials/tutorial-share-anchors-across-devices.md)
