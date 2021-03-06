---
title: Hızlı Başlangıç - Azure uzamsal Çıpasıyla bir Unity iOS uygulaması oluşturma | Microsoft Docs
description: Bu hızlı başlangıçta, bir iOS uygulamasını uzamsal bağlayıcılarını kullanarak Unity ile oluşturmayı öğrenin.
author: craigktreasure
manager: aliemami
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 02/24/2019
ms.topic: quickstart
ms.service: azure-spatial-anchors
ms.openlocfilehash: 37856c0833ecde1478d4bd588b8e3122e8eac0ca
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "67135253"
---
# <a name="quickstart-create-a-unity-ios-app-with-azure-spatial-anchors"></a>Hızlı Başlangıç: Azure uzamsal Çıpasıyla bir Unity iOS uygulaması oluşturma

Bu hızlı başlangıçta bir Unity iOS uygulamasını kullanarak oluşturmak nasıl etkinleştireceğinizi de açıklar [Azure uzamsal bağlayıcılarını](../overview.md). Azure uzamsal bağlayıcılarını konumlarına cihazlar arasında zaman içinde kalıcı nesneler kullanarak karma gerçeklik deneyimleri oluşturmanıza olanak tanıyan platformlar arası Geliştirici hizmetidir. İşlemi tamamladığınızda, kaydedebilir ve uzamsal bağlantı geri çağırma Unity ile oluşturulan ARKit iOS uygulamasına sahip olacaksınız.

Şunları öğrenirsiniz:

> [!div class="checklist"]
> * Uzamsal bağlayıcılarını hesabı oluşturma
> * Unity derleme ayarlarını hazırlama
> * İndirme ve Unity ARKit eklenti içeri aktarma
> * Uzamsal bağlayıcılarını hesap tanımlayıcısı ve hesap anahtarını yapılandırma
> * Xcode projesi dışarı aktarma
> * Dağıtma ve iOS cihazda çalıştırma

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlamak için aşağıdakileri yaptığınızdan emin olun:

- Bir macOS makineyle <a href="https://unity3d.com/get-unity/download" target="_blank">Unity 2018.3 +</a>, <a href="https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12" target="_blank">Xcode 10</a>, ve <a href="https://cocoapods.org" target="_blank">CocoaPods</a> yüklü.
- HomeBrew yüklü Git. Tek satırlık bir Terminal içinde aşağıdaki komutu girin: `/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`. Ardından çalıştırın `brew install git`.
- Etkin bir geliştirici <a href="https://developer.apple.com/documentation/arkit/verifying_device_support_and_user_permission" target="_blank">ARKit uyumlu</a> iOS cihaz.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="download-and-open-the-unity-sample-project"></a>İndir ve Unity örnek proje Aç

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

[!INCLUDE [Open Unity Project](../../../includes/spatial-anchors-open-unity-project.md)]

[!INCLUDE [iOS Unity Build Settings](../../../includes/spatial-anchors-unity-ios-build-settings.md)]

## <a name="configure-account-identifier-and-key"></a>Hesap Kimliği ve anahtarını yapılandırma

İçinde **proje** bölmesinde gidin `Assets/AzureSpatialAnchorsPlugin/Examples` açın `AzureSpatialAnchorsBasicDemo.unity` Sahne dosyası.

[!INCLUDE [Configure Unity Scene](../../../includes/spatial-anchors-unity-configure-scene.md)]

Sahne seçerek Kaydet **dosya** -> **Kaydet**.

## <a name="export-the-xcode-project"></a>Xcode projesi dışarı aktarma

[!INCLUDE [Export Unity Project](../../../includes/spatial-anchors-unity-export-project-snip.md)]

[!INCLUDE [Configure Xcode](../../../includes/spatial-anchors-unity-ios-xcode.md)]

Yerleştirin ve bir bağlantı geri çağırma uygulaması'ndaki yönergeleri izleyin.

> [!NOTE]
> Kamera arka planı olarak (veya diğer doku örneği yerine boş, mavi görürsünüz) görmüyorsanız, uygulama çalıştırırken daha sonra büyük olasılıkla Unity varlıkları yeniden almanız gerekir. Uygulamayı durdurun. Unity üstteki menüden seçin **varlıklar -> yeniden içeri tüm**. Ardından, uygulamayı yeniden çalıştırın.

Xcode içindeki tuşlarına basarak uygulamayı durdurun **Durdur**.

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

[!INCLUDE [Next steps](../../../includes/spatial-anchors-quickstarts-nextsteps.md)]

> [!div class="nextstepaction"]
> [Öğretici: Cihazlar arasında paylaşımı uzamsal yer işaretleri](../tutorials/tutorial-share-anchors-across-devices.md)
