---
author: ramonarguelles
ms.service: spatial-anchors
ms.topic: include
ms.date: 1/30/2019
ms.author: rgarcia
ms.openlocfilehash: b46a2b18309851bbe2934980137a53d2de6f6efc
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "67135318"
---
## <a name="set-up-your-device-in-unity"></a>Unity Cihazınızı ayarlama

[!INCLUDE [Open Unity Project](spatial-anchors-open-unity-project.md)]

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

Altında **sahneler oluşturun**, Sahne tüm bunların yanında bir onay işareti sahip olduğunuzdan emin olun.

Emin **dışarı proje** bir onay işareti yok. Seçin **derleme ve çalıştırma**. Kaydetmeniz istenir, `.apk` dosya. Bunun için herhangi bir ad seçebilirsiniz.

Uygulama başlar, içinde bir kez **seçin bir tanıtım** iletişim kutusunda, seçmek için sol veya sağ okları kullanın **LocalShare** seçenek ve dokunun **gidin!** . Uygulamayı'ndaki yönergeleri izleyin. Seçebileceğiniz **oluştur & Paylaşımı bağlantı** veya **paylaşılan bağlantı bulun**.

İlk senaryo, daha sonra aynı cihaz veya farklı bir bulunduğu bir bağlantı oluşturmanızı sağlar.
Aynı cihaz veya farklı bir uygulama zaten çalıştırdıysanız, ikinci senaryo, daha önce paylaşılan bağlayıcılarını bulundurmanıza olanak tanır. Senaryonuzu seçin sonra uygulamayı daha ayrıntılı yönergeler ne yapılacağını geçici yol gösterecektir. Örneğin, ortam bilgilerini toplamak için Cihazınızı yerleri istenir. Daha sonra dünyada bir yer işareti koyun kaydetmek bekleyin ve benzeri.

### <a name="deploy-to-an-ios-device"></a>Bir iOS cihazına dağıtma

Açık **Build Settings** seçerek **dosya** > **Build Settings**.

Altında **sahneler oluşturun**, Sahne tüm bunların yanında bir onay işareti sahip olduğunuzdan emin olun.

[!INCLUDE [Configure Xcode](spatial-anchors-unity-ios-xcode.md)]

Uygulama başlar, içinde bir kez **seçin bir tanıtım** iletişim kutusunda, seçmek için sol veya sağ okları kullanın **LocalShare** seçenek ve dokunun **gidin!** . Uygulamayı'ndaki yönergeleri izleyin. Seçebileceğiniz **oluştur & Paylaşımı bağlantı** veya **paylaşılan bağlantı bulun**.

İlk senaryo, daha sonra aynı cihaz veya farklı bir bulunduğu bir bağlantı oluşturmanızı sağlar.
Aynı cihaz veya farklı bir uygulama zaten çalıştırdıysanız, ikinci senaryo, daha önce paylaşılan bağlayıcılarını bulundurmanıza olanak tanır. Senaryonuzu seçin sonra uygulamayı daha ayrıntılı yönergeler ne yapılacağını geçici yol gösterecektir. Örneğin, ortam bilgilerini toplamak için Cihazınızı yerleri istenir. Daha sonra dünyada bir yer işareti koyun kaydetmek bekleyin ve benzeri.

Xcode'da, seçerek uygulamayı durdurun **Durdur**.
