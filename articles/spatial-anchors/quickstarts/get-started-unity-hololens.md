---
title: Hızlı Başlangıç - Azure uzamsal Çıpasıyla HoloLens Unity uygulaması oluşturma | Microsoft Docs
description: Bu hızlı başlangıçta, uzamsal bağlayıcılarını kullanarak Unity ile bir HoloLens uygulamaların nasıl oluşturulduğunu öğrenin.
author: craigktreasure
manager: aliemami
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 02/24/2019
ms.topic: quickstart
ms.service: azure-spatial-anchors
ms.openlocfilehash: ede1f031c3c38e714d076b861ba2abdad81c6702
ms.sourcegitcommit: 235cd1c4f003a7f8459b9761a623f000dd9e50ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/11/2019
ms.locfileid: "57726864"
---
# <a name="quickstart-create-a-hololens-unity-app-using-azure-spatial-anchors"></a>Hızlı Başlangıç: Azure uzamsal bağlayıcılarını kullanarak HoloLens Unity uygulama oluşturma

Bu hızlı başlangıçta, HoloLens Unity kullanarak uygulama oluşturma işlemini ele alınmaktadır [Azure uzamsal bağlayıcılarını](../overview.md). Azure uzamsal bağlayıcılarını konumlarına cihazlar arasında zaman içinde kalıcı nesneler kullanarak karma gerçeklik deneyimleri oluşturmanıza olanak tanıyan platformlar arası Geliştirici hizmetidir. İşlemi tamamladığınızda, kaydedebilir ve uzamsal bağlantı geri çağırma Unity ile oluşturulmuş bir HoloLens uygulaması oluşturmuş olacaksınız.

Şunları öğrenirsiniz:

> [!div class="checklist"]
> * Uzamsal bağlayıcılarını hesabı oluşturma
> * Unity derleme ayarlarını hazırlama
> * Uzamsal bağlayıcılarını hesap tanımlayıcısı ve hesap anahtarını yapılandırma
> * HoloLens Visual Studio projesini dışarı aktarma
> * Dağıtma ve HoloLens cihazda çalıştırma

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlamak için aşağıdakileri yaptığınızdan emin olun:

- Bir Windows makineyle <a href="https://unity3d.com/get-unity/download" target="_blank">Unity 2018.3 +</a> ve <a href="https://www.visualstudio.com/downloads/" target="_blank">Visual Studio 2017 +</a> ile yüklenen **Evrensel Windows platformu geliştirme** iş yükü.
- HoloLens cihazla [Geliştirici modu](https://docs.microsoft.com/windows/mixed-reality/using-visual-studio) etkin. Bu makale gerekir HoloLens cihazla [Windows 10 Ekim 2018 güncelleştirmesi](https://docs.microsoft.com/en-us/windows/mixed-reality/release-notes-october-2018) (RS5 olarak da bilinir). HoloLens üzerinde en son sürümüne güncelleştirmek için **ayarları** uygulama, Git **güncelleştirme ve güvenlik**, ardından **Güncelleştirmeleri denetle** düğmesi.
- Uygulamanızı ayarlamalısınız **SpatialPerception** özelliği altında **Build Settings**->**Player ayarları**->**yayımlama Ayarları**->**özellikleri**.
- Uygulamanızı etkinleştirmelisiniz **sanal gerçeklik desteklenen** ile **Windows karma gerçeklik SDK** altında **Build Settings**->**Playerayarları** -> **XR ayarları**.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="open-the-sample-project-in-unity"></a>Örnek Proje içinde Unity açın

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

Unity açın ve proje açmak `Unity` klasör.

Açık **Build Settings** seçerek **dosya** -> **Build Settings**.

İçinde **Platform** bölümünden **Evrensel Windows platformu**. Ardından **hedef cihaz** için **HoloLens**.

Seçin **anahtar platformu** platforma değiştirmek için **Evrensel Windows platformu**. Unity UWP desteği bileşenleri eksik iseler yüklemenizi isteyebilir.

![Unity derleme ayarları](./media/get-started-unity-hololens/unity-build-settings.png)

Kapat **Build Settings** penceresi.

## <a name="configure-account-identifier-and-key"></a>Hesap Kimliği ve anahtarını yapılandırma

İçinde **proje** bölmesinde gidin `Assets/AzureSpatialAnchorsPlugin/Examples` açın `AzureSpatialAnchorsBasicDemo.unity` Sahne dosyası.

[!INCLUDE [Configure Unity Scene](../../../includes/spatial-anchors-unity-configure-scene.md)]

Sahne seçerek Kaydet **dosya** -> **Kaydet**.

## <a name="export-the-hololens-visual-studio-project"></a>HoloLens Visual Studio projesini dışarı aktarma

[!INCLUDE [Export Unity Project](../../../includes/spatial-anchors-unity-export-project-snip.md)]

Seçin **derleme** bir iletişim kutusunu açın. Ardından, HoloLens Visual Studio projesini dışarı aktarmak için bir klasör seçin.

Dışarı aktarma tamamlandığında, dışarı aktarılan HoloLens projeyi içeren klasör görüntülenir.

## <a name="deploy-the-hololens-application"></a>HoloLens uygulamayı dağıtma

Klasörde çift tıklayarak `HelloAR U3D.sln` projeyi Visual Studio'da açın.

Değiştirme **çözüm yapılandırması** için **yayın**, değiştirme **çözüm platformu** için **x86**seçip **cihaz**  dağıtım hedef seçenekleri.

![Visual Studio yapılandırması](./media/get-started-unity-hololens/visual-studio-configuration.png)

HoloLens cihazda güç, oturum açın ve bir USB kablosu kullanarak Bilgisayara bağlanın.

Seçin **hata ayıklama** > **hata ayıklamayı Başlat** uygulamanızı dağıtma ve hata ayıklamaya başlayın.

Yerleştirin ve bir bağlantı geri çağırma uygulaması'ndaki yönergeleri izleyin.

Visual Studio'da seçerek uygulamayı durdurun **hata ayıklamayı Durdur** ya basarak **Shift + F5 tuşlarına basarak**.

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

[!INCLUDE [Next steps](../../../includes/spatial-anchors-quickstarts-nextsteps.md)]

> [!div class="nextstepaction"]
> [Öğretici: Cihazlar arasında paylaşımı uzamsal yer işaretleri](../tutorials/tutorial-share-anchors-across-devices.md)
