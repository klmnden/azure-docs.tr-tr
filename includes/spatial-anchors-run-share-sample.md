---
author: ramonarguelles
ms.service: spatial-anchors
ms.topic: include
ms.date: 1/30/2019
ms.author: rgarcia
ms.openlocfilehash: bfeb8bddf5fe3b4a76e662aed6c5a07439d2f1cf
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "57908645"
---
## <a name="set-up-your-device"></a>Cihazınızı kurma

### <a name="set-up-an-android-device"></a>Bir Android cihazı ayarlama

[!INCLUDE [Android Unity Build Settings](spatial-anchors-unity-android-build-settings.md)]

### <a name="set-up-an-ios-device"></a>Bir iOS cihaz ayarlama

[!INCLUDE [iOS Unity Build Settings](spatial-anchors-unity-ios-build-settings.md)]

## <a name="configure-account-identifier-and-key"></a>Hesap Kimliği ve anahtarını yapılandırma

İçinde **proje** bölmesinde gidin `Assets/AzureSpatialAnchorsPlugin/Examples` açın `AzureSpatialAnchorsLocalSharedDemo.unity` Sahne dosyası.

[!INCLUDE [Configure Unity Scene](spatial-anchors-unity-configure-scene.md)]

Ayrıca, **denetçisi** bölmesinde girin `Sharing Anchors Service url` (ASP.NET web uygulamanızdan Azure dağıtım) değeri olarak `Base Sharing Url`, değiştirmeyi `index.html` ile `api/anchors`. Bu nedenle, gibi görünmelidir: `https://<app_name>.azurewebsites.net/api/anchors`.

Sahne seçerek Kaydet **dosya** -> **Kaydet**.

## <a name="to-deploy-to-an-android-device"></a>Bir Android cihazına dağıtmak için

Android Cihazınızda oturum açın ve bir USB kablosu kullanarak Bilgisayara bağlanın.

Açık **Build Settings** seçerek **dosya** -> **Build Settings**.

Altında **sahneler oluşturun**, bir onay işareti koyun `AzureSpatialAnchorsPlugin/Examples/AzureSpatialAnchorsLocalSharedDemo` Sahne ve clear diğer tüm sahneler işaretlerini denetleyin.

Olun **dışarı proje** onay kutusu bir onay işareti yok. Tıklayın **derleme ve çalıştırma**. Kaydetmeniz istenir, `.apk` dosyası için bir ad seçim yapabilirsiniz.

Uygulamayı'ndaki yönergeleri izleyin. Seçebileceğiniz **oluştur & Paylaşımı bağlantı**, veya **paylaşılan bağlantı bulun**. İlk seçenek, aynı cihaza ya da farklı bir daha sonra bulunan bir bağlantı oluşturmanıza olanak sağlar. Aynı cihaz veya farklı bir uygulama daha önce çalıştırdıysanız olan ikinci seçenek, daha önce paylaşılan bağlayıcılarını bulundurmanıza olanak tanır.

## <a name="to-deploy-to-an-ios-device"></a>Bir iOS cihazına dağıtmak için

Açık **Build Settings** seçerek **dosya** -> **Build Settings**.

Altında **sahneler oluşturun**, bir onay işareti koyun `AzureSpatialAnchorsPlugin/Examples/AzureSpatialAnchorsLocalSharedDemo` Sahne ve clear diğer tüm sahneler işaretlerini denetleyin.

[!INCLUDE [Configure Xcode](spatial-anchors-unity-ios-xcode.md)]

Uygulamayı'ndaki yönergeleri izleyin. Seçebileceğiniz **oluştur & Paylaşımı bağlantı**, veya **paylaşılan bağlantı bulun**. İlk seçenek, aynı cihaza ya da farklı bir daha sonra bulunan bir bağlantı oluşturmanıza olanak sağlar. Aynı cihaz veya farklı bir uygulama daha önce çalıştırdıysanız olan ikinci seçenek, daha önce paylaşılan bağlayıcılarını bulundurmanıza olanak tanır.

Xcode içindeki tuşlarına basarak uygulamayı durdurun **Durdur**.
