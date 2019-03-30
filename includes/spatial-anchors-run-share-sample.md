---
author: ramonarguelles
ms.service: spatial-anchors
ms.topic: include
ms.date: 1/30/2019
ms.author: rgarcia
ms.openlocfilehash: 397a8a9b07b4d7a88d0345399ac4abcc3e738a82
ms.sourcegitcommit: 956749f17569a55bcafba95aef9abcbb345eb929
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58631170"
---
## <a name="set-up-your-device"></a>Cihazınızı kurma

Unity içinde projeyi açın `Unity` klasör.

![Unity penceresi](./media/spatial-anchors-unity/unity-window.png)

### <a name="set-up-an-android-device"></a>Bir Android cihazı ayarlama

[!INCLUDE [Android Unity Build Settings](spatial-anchors-unity-android-build-settings.md)]

### <a name="set-up-an-ios-device"></a>Bir iOS cihaz ayarlama

[!INCLUDE [iOS Unity Build Settings](spatial-anchors-unity-ios-build-settings.md)]

## <a name="configure-the-account-identifier-and-key"></a>Hesap Kimliği ve anahtarını yapılandırma

İçinde **proje** bölmesinde gidin `Assets/AzureSpatialAnchorsPlugin/Examples` açın `AzureSpatialAnchorsLocalSharedDemo.unity` Sahne dosyası.

[!INCLUDE [Configure Unity Scene](spatial-anchors-unity-configure-scene.md)]

İçinde **denetçisi** bölmesinde girin `Sharing Anchors Service url` (ASP.NET web uygulamanızdan Azure dağıtım) değeri olarak `Base Sharing Url`, değiştirmeyi `index.html` ile `api/anchors`. Şu şekilde görünmelidir: `https://<app_name>.azurewebsites.net/api/anchors`.

Sahne seçerek Kaydet **dosya** > **Kaydet**.

## <a name="deploy-to-your-device"></a>Cihazınıza dağıtma

### <a name="deploy-to-android-device"></a>Android cihazına dağıtma

Android Cihazınızda oturum açın ve bir USB kablosu kullanarak bilgisayarınıza bağlayın.

Açık **Build Settings** seçerek **dosya** > **Build Settings**.

Altında **sahneler oluşturun**, bir onay işareti koyun `AzureSpatialAnchorsPlugin/Examples/AzureSpatialAnchorsLocalSharedDemo` onay Sahne ve clear diğer tüm sahneler işaretler.

Emin **dışarı proje** bir onay işareti yok. Seçin **derleme ve çalıştırma**. Kaydetmeniz istenir, `.apk` dosya. Bunun için herhangi bir ad seçebilirsiniz.

Uygulamayı'ndaki yönergeleri izleyin. Seçebileceğiniz **oluştur & Paylaşımı bağlantı** veya **paylaşılan bağlantı bulun**. İlk senaryo, daha sonra aynı cihaz veya farklı bir bulunduğu bir bağlantı oluşturmanızı sağlar. Aynı cihaz veya farklı bir uygulama zaten çalıştırdıysanız, ikinci senaryo, daha önce paylaşılan bağlayıcılarını bulundurmanıza olanak tanır. Senaryonuzu seçin sonra uygulama için neler etrafında ek yönergeler ile yol gösterecektir. Örneğin, ortam bilgilerini toplamak için Cihazınızı hareket istenir. Daha sonra dünyada bir yer işareti koyun, bunu yükler bekleyin ve benzeri.

### <a name="deploy-to-an-ios-device"></a>Bir iOS cihazına dağıtma

Açık **Build Settings** seçerek **dosya** > **Build Settings**.

Altında **sahneler oluşturun**, bir onay işareti koyun `AzureSpatialAnchorsPlugin/Examples/AzureSpatialAnchorsLocalSharedDemo` onay Sahne ve clear diğer tüm sahneler işaretler.

[!INCLUDE [Configure Xcode](spatial-anchors-unity-ios-xcode.md)]

Uygulamayı'ndaki yönergeleri izleyin. Seçebileceğiniz **oluştur & Paylaşımı bağlantı** veya **paylaşılan bağlantı bulun**. İlk senaryo, daha sonra aynı cihaz veya farklı bir bulunduğu bir bağlantı oluşturmanızı sağlar. Aynı cihaz veya farklı bir uygulama zaten çalıştırdıysanız, ikinci senaryo, daha önce paylaşılan bağlayıcılarını bulundurmanıza olanak tanır. Senaryonuzu seçin sonra uygulama için neler etrafında ek yönergeler ile yol gösterecektir. Örneğin, ortam bilgilerini toplamak için Cihazınızı hareket istenir. Daha sonra dünyada bir yer işareti koyun, bunu yükler bekleyin ve benzeri.

Xcode'da, seçerek uygulamayı durdurun **Durdur**.
