---
author: ramonarguelles
ms.service: spatial-anchors
ms.topic: include
ms.date: 1/30/2019
ms.author: rgarcia
ms.openlocfilehash: 563c2bd561328561d30acee6910b70d53ef64c6b
ms.sourcegitcommit: 8a59b051b283a72765e7d9ac9dd0586f37018d30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58305372"
---
## <a name="set-up-your-device"></a>Cihazınızı kurma

### <a name="set-up-an-android-device"></a>Bir Android cihazı ayarlama

[!INCLUDE [Android Unity Build Settings](spatial-anchors-unity-android-build-settings.md)]

### <a name="set-up-an-ios-device"></a>Bir iOS cihaz ayarlama

[!INCLUDE [iOS Unity Build Settings](spatial-anchors-unity-ios-build-settings.md)]

## <a name="configure-the-account-identifier-and-key"></a>Hesap Kimliği ve anahtarını yapılandırma

İçinde **proje** bölmesinde gidin `Assets/AzureSpatialAnchorsPlugin/Examples` açın `AzureSpatialAnchorsLocalSharedDemo.unity` Sahne dosyası.

[!INCLUDE [Configure Unity Scene](spatial-anchors-unity-configure-scene.md)]

İçinde **denetçisi** bölmesinde girin `Sharing Anchors Service url` (ASP.NET web uygulamanızdan Azure dağıtım) değeri olarak `Base Sharing Url`, değiştirmeyi `index.html` ile `api/anchors`. Şu şekilde görünmelidir: `https://<app_name>.azurewebsites.net/api/anchors`.

Sahne seçerek Kaydet **dosya** > **Kaydet**.

## <a name="to-deploy-the-app-to-an-android-device"></a>Uygulamasını bir Android cihazına dağıtmak için

Android Cihazınızda oturum açın ve bir USB kablosu kullanarak bilgisayarınıza bağlayın.

Açık **Build Settings** seçerek **dosya** > **Build Settings**.

Altında **sahneler oluşturun**, bir onay işareti koyun `AzureSpatialAnchorsPlugin/Examples/AzureSpatialAnchorsLocalSharedDemo` onay Sahne ve clear diğer tüm sahneler işaretler.

Emin **dışarı proje** bir onay işareti yok. Seçin **derleme ve çalıştırma**. Kaydetmeniz istenir, `.apk` dosya. Bunun için herhangi bir ad seçebilirsiniz.

Uygulamayı'ndaki yönergeleri izleyin. Seçebileceğiniz **oluştur & Paylaşımı bağlantı** veya **paylaşılan bağlantı bulun**. İlk seçenek, daha sonra aynı cihaz veya farklı bir bulunduğu bir bağlantı oluşturmanızı sağlar. Aynı cihaz veya farklı bir uygulama zaten çalıştırdıysanız, ikinci seçenek, daha önce paylaşılan bağlayıcılarını bulundurmanıza olanak tanır.

## <a name="to-deploy-the-app-to-an-ios-device"></a>Uygulamayı bir iOS cihazına dağıtmak için

Açık **Build Settings** seçerek **dosya** > **Build Settings**.

Altında **sahneler oluşturun**, bir onay işareti koyun `AzureSpatialAnchorsPlugin/Examples/AzureSpatialAnchorsLocalSharedDemo` onay Sahne ve clear diğer tüm sahneler işaretler.

[!INCLUDE [Configure Xcode](spatial-anchors-unity-ios-xcode.md)]

Uygulamayı'ndaki yönergeleri izleyin. Seçebileceğiniz **oluştur & Paylaşımı bağlantı** veya **paylaşılan bağlantı bulun**. İlk seçenek, daha sonra aynı cihaz veya farklı bir bulunduğu bir bağlantı oluşturmanızı sağlar. Aynı cihaz veya farklı bir uygulama zaten çalıştırdıysanız, ikinci seçenek, daha önce paylaşılan bağlayıcılarını bulundurmanıza olanak tanır.

Xcode'da, seçerek uygulamayı durdurun **Durdur**.
