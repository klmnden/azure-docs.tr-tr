---
author: ramonarguelles
ms.service: spatial-anchors
ms.topic: include
ms.date: 1/30/2019
ms.author: rgarcia
ms.openlocfilehash: 5f1e0153b1f919bc9d7e921d2a1b3ae745b2b01f
ms.sourcegitcommit: e88188bc015525d5bead239ed562067d3fae9822
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/24/2019
ms.locfileid: "56752163"
---
## <a name="open-the-sample-project-in-unity"></a>Örnek Proje içinde Unity açın

[!INCLUDE [Clone Sample Repo](spatial-anchors-clone-sample-repository.md)]

## <a name="to-set-up-for-an-android-device"></a>Bir Android cihazını ayarlamak için

[!INCLUDE [Android Unity Build Settings](spatial-anchors-unity-android-build-settings.md)]

## <a name="to-set-up-for-an-ios-device"></a>Bir iOS cihazını ayarlamak için

[!INCLUDE [iOS Unity Build Settings](spatial-anchors-unity-ios-build-settings.md)]

## <a name="configure-account-identifier-and-key"></a>Hesap Kimliği ve anahtarını yapılandırma

İçinde **proje** bölmesinde gidin `Assets/AzureSpatialAnchorsPlugin/Examples` açın `AzureSpatialAnchorsLocalSharedDemo.unity` Sahne dosyası.

[!INCLUDE [Configure Unity Scene](spatial-anchors-unity-configure-scene.md)]

Ayrıca, **denetçisi** bölmesinde girin `Sharing Anchors Service url` (ASP.NET web uygulamanızdan Azure dağıtım) değeri olarak `Base Sharing Url`, değiştirmeyi `index.html` ile `api/anchors`. Bu nedenle, gibi görünmelidir: `https://<app_name>.azurewebsites.net/api/anchors`.

Sahne seçerek Kaydet **dosya** -> **Kaydet**.

## <a name="to-deploy-to-an-android-device"></a>Bir Android cihazına dağıtmak için

Android cihazında güç, oturum açın ve bir USB kablosu kullanarak Bilgisayara bağlanın.

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
