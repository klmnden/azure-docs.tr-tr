---
title: Hızlı Başlangıç - Azure uzamsal Çıpasıyla iOS uygulaması oluşturma | Microsoft Docs
description: Bu hızlı başlangıçta, uzamsal bağlayıcılarını kullanarak bir iOS uygulamasının nasıl oluşturulacağını öğrenin.
author: craigktreasure
manager: aliemami
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 02/24/2019
ms.topic: quickstart
ms.service: azure-spatial-anchors
ms.openlocfilehash: 56360238db8632e74a95c057a7fe643b5cea3151
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67206825"
---
# <a name="quickstart-create-an-ios-app-with-azure-spatial-anchors-in-either-swift-or-objective-c"></a>Hızlı Başlangıç: Azure uzamsal bağlantıları, Swift veya Objective-C ile bir iOS uygulaması oluşturma

Bu hızlı başlangıçta kullanarak bir iOS uygulaması oluşturmak nasıl etkinleştireceğinizi de açıklar [Azure uzamsal bağlayıcılarını](../overview.md) Swift veya Objective-c Azure uzamsal bağlayıcılarını konumlarına cihazlar arasında zaman içinde kalıcı nesneler kullanarak karma gerçeklik deneyimleri oluşturmanıza olanak tanıyan platformlar arası Geliştirici hizmetidir. İşiniz bittiğinde, kaydedebilir ve uzamsal bağlantı geri çağırma ARKit iOS uygulamasının sahip olacaksınız.

Şunları öğrenirsiniz:

> [!div class="checklist"]
> * Uzamsal bağlayıcılarını hesabı oluşturma
> * Uzamsal bağlayıcılarını hesap tanımlayıcısı ve hesap anahtarını yapılandırma
> * Dağıtma ve iOS cihazda çalıştırma

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlamak için aşağıdakileri yaptığınızdan emin olun:

- Bir geliştirici etkin macOS makineyle <a href="https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12" target="_blank">Xcode 10 +</a> ve <a href="https://cocoapods.org" target="_blank">CocoaPods</a> yüklü.
- HomeBrew yüklü Git. Tek satırlık bir Terminal içinde aşağıdaki komutu girin: `/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`. Ardından çalıştırın `brew install git`.
- Etkin bir geliştirici <a href="https://developer.apple.com/documentation/arkit/verifying_device_support_and_user_permission" target="_blank">ARKit uyumlu</a> iOS cihaz.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="open-the-sample-project"></a>Örnek projeyi açın

Terminalde, aşağıdaki eylemleri gerçekleştirmek için kullanın.

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

CocoaPods kullanarak gerekli pod'ların yükleyin:

# <a name="swifttabopenproject-swift"></a>[Swift](#tab/openproject-swift)

`iOS/Swift/` sayfasına gidin.

```bash
cd ./iOS/Swift/
```

# <a name="objective-ctabopenproject-objc"></a>[Objective-C](#tab/openproject-objc)

`iOS/Objective-C/` sayfasına gidin.

```bash
cd ./iOS/Objective-C/
```

---

Çalıştırma `pod install --repo-update` projenin Cocoapods'u yüklemek üzere.

Artık `.xcworkspace` xcode'da.

# <a name="swifttabopenproject-swift"></a>[Swift](#tab/openproject-swift)

```bash
open ./SampleSwift.xcworkspace
```

# <a name="objective-ctabopenproject-objc"></a>[Objective-C](#tab/openproject-objc)

```bash
open ./SampleObjC.xcworkspace
```

---

## <a name="configure-account-identifier-and-key"></a>Hesap Kimliği ve anahtarını yapılandırma

Sonraki adım uygulamayı hesap tanımlayıcısı ve hesap anahtarını kullanacak şekilde yapılandırmaktır. Bir metin düzenleyiciye kopyaladığınız zaman [uzamsal bağlayıcılarını kaynağı ayarı](#create-a-spatial-anchors-resource).

# <a name="swifttabopenproject-swift"></a>[Swift](#tab/openproject-swift)

Açık `iOS/Swift/SampleSwift/ViewControllers/BaseViewController.swift`.

Bulun `spatialAnchorsAccountKey` değiştirin ve alan `Set me` ile hesap anahtarı.

Bulun `spatialAnchorsAccountId` değiştirin ve alan `Set me` hesap tanımlayıcı ile.

# <a name="objective-ctabopenproject-objc"></a>[Objective-C](#tab/openproject-objc)

Açık `iOS/Objective-C/SampleObjC/BaseViewController.m`.

Bulun `SpatialAnchorsAccountKey` değiştirin ve alan `Set me` ile hesap anahtarı.

Bulun `SpatialAnchorsAccountId` değiştirin ve alan `Set me` hesap tanımlayıcı ile.

---

## <a name="deploy-the-app-to-your-ios-device"></a>İOS cihazınıza uygulaması dağıtma

İOS cihazı ayarlama ve Mac bağlayın **etkin düzeni** iOS cihazınıza.

![Cihazı seçin](./media/get-started-ios/select-device.png)

Seçin **oluşturun ve ardından geçerli düzenini çalıştırın**.

![Dağıtma ve çalıştırma](./media/get-started-ios/deploy-run.png)

> [!NOTE]
> Görürseniz bir `library not found for -lPods-SampleObjC` hata büyük olasılıkla açtığınız `.xcodeproj` yerine dosya `.xcworkspace`. Açık `.xcworkspace` ve yeniden deneyin.

Xcode içindeki tuşlarına basarak uygulamayı durdurun **Durdur**.

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

[!INCLUDE [Next steps](../../../includes/spatial-anchors-quickstarts-nextsteps.md)]

> [!div class="nextstepaction"]
> [Öğretici: Cihazlar arasında paylaşımı uzamsal yer işaretleri](../tutorials/tutorial-share-anchors-across-devices.md)
