---
title: Hızlı Başlangıç - bir Unity oluşturma HoloLens uygulamasıyla Azure uzamsal bağlayıcılarını | Microsoft Docs
description: Bu hızlı başlangıçta, uzamsal bağlayıcılarını kullanarak Unity ile bir HoloLens uygulamaların nasıl oluşturulduğunu öğrenin.
author: craigktreasure
manager: aliemami
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 02/24/2019
ms.topic: quickstart
ms.service: azure-spatial-anchors
ms.openlocfilehash: c29819d817138f2512420584947763247837a9ea
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2019
ms.locfileid: "67135198"
---
# <a name="quickstart-create-a-unity-hololens-app-that-uses-azure-spatial-anchors"></a>Hızlı Başlangıç: Azure uzamsal bağlayıcıları kullanan bir Unity HoloLens uygulaması oluşturma

Bu hızlı başlangıçta, kullanan bir Unity HoloLens uygulamayı oluşturacaksınız [Azure uzamsal bağlayıcılarını](../overview.md). Uzamsal yer işaretleri, konumlarına cihazlar arasında zaman içinde kalıcı nesneler ile karma gerçeklik deneyimleri oluşturmanıza olanak tanır platformlar arası Geliştirici hizmettir. İşlemi tamamladığınızda, kaydedebilir ve uzamsal bağlantı geri çağırma Unity ile oluşturulmuş bir HoloLens uygulaması oluşturmuş olacaksınız.

Şunları öğrenirsiniz:

- Bir uzamsal bağlayıcılarını hesabı oluşturun.
- Unity derleme ayarlarını hazırlayın.
- Uzamsal bağlayıcılarını hesap tanımlayıcısı ve hesap anahtarını yapılandırın.
- HoloLens Visual Studio projesini dışarı aktarın.
- Uygulamayı dağıtabiliyorum ve bir HoloLens cihazda çalıştırın.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlamak için:


- Bir Windows bilgisayara ihtiyacınız <a href="https://unity3d.com/get-unity/download" target="_blank">Unity 2018.3</a> veya üzeri ve <a href="https://www.visualstudio.com/downloads/" target="_blank">Visual Studio 2019</a> veya üzeri yüklü. Visual Studio yüklemenizin içermelidir **Evrensel Windows platformu geliştirme** iş yükü. Yükleme <a href="https://git-scm.com/download/win" target="_blank">Windows için Git</a>.
- Bir HoloLens cihazda ihtiyacınız [Geliştirici modu](https://docs.microsoft.com/windows/mixed-reality/using-visual-studio) etkin. [Windows 10 Ekim 2018 güncelleştirmesi](https://docs.microsoft.com/windows/mixed-reality/release-notes-october-2018) (RS5 olarak da bilinir), cihazda yüklü olması gerekir. HoloLens üzerinde en son sürümüne güncelleştirmek için **ayarları** uygulama, Git **güncelleştirme ve güvenlik**ve ardından **Güncelleştirmeleri denetle**.
- Uygulamanız üzerinde etkinleştirmeniz gerekir. **SpatialPerception** yeteneği. Bu ayar **Build Settings** > **Player ayarları** > **yayımlama ayarları**  >   **Özellikleri**.
- Uygulamanız üzerinde etkinleştirmeniz gerekir. **sanal gerçeklik desteklenen** ile **Windows karma gerçeklik SDK**. Bu ayar **Build Settings** > **Player ayarları** > **XR ayarları**.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="download-and-open-the-unity-sample-project"></a>İndir ve Unity örnek proje Aç

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

[!INCLUDE [Open Unity Project](../../../includes/spatial-anchors-open-unity-project.md)]

Açık **Build Settings** seçerek **dosya** > **Build Settings**.

İçinde **Platform** bölümünden **Evrensel Windows platformu**. Değişiklik **hedef cihaz** için **HoloLens**.

Seçin **anahtar platformu** platforma değiştirmek için **Evrensel Windows platformu**. Unity UWP desteği bileşenleri eksik iseler yüklemenizi isteyebilir.

![Unity yapı Ayarları penceresi](./media/get-started-unity-hololens/unity-build-settings.png)

Kapat **Build Settings** penceresi.

## <a name="configure-the-account-identifier-and-key"></a>Hesap Kimliği ve anahtarını yapılandırma

İçinde **proje** bölmesinde, Git `Assets/AzureSpatialAnchorsPlugin/Examples` açın `AzureSpatialAnchorsBasicDemo.unity` Sahne dosyası.

[!INCLUDE [Configure Unity Scene](../../../includes/spatial-anchors-unity-configure-scene.md)]

Sahne seçerek Kaydet **dosya** > **Kaydet**.

## <a name="export-the-hololens-visual-studio-project"></a>HoloLens Visual Studio projesini dışarı aktarma

[!INCLUDE [Export Unity Project](../../../includes/spatial-anchors-unity-export-project-snip.md)]

Seçin **yapı**. İletişim kutusunda, HoloLens Visual Studio projesini dışarı aktarmak bir klasör seçin.

Dışarı aktarma tamamlandığında, dışarı aktarılan HoloLens proje içeren bir klasör görünür.

## <a name="deploy-the-hololens-application"></a>HoloLens uygulamayı dağıtma

Klasörde çift **HelloAR U3D.sln** projeyi Visual Studio'da açın.

Değiştirme **çözüm yapılandırması** için **yayın**, değiştirme **çözüm platformu** için **x86**seçip **cihaz**  dağıtım hedef seçenekleri.

HoloLens 2 kullanıyorsanız kullanın **ARM** olarak **çözüm platformu**, yerine **x86**.

   ![Visual Studio yapılandırması](./media/get-started-unity-hololens/visual-studio-configuration.png)

HoloLens cihazı açın, oturum açın ve cihazı bir USB kablosu kullanarak Bilgisayara bağlayın.

Seçin **hata ayıklama** > **hata ayıklamayı Başlat** uygulamanızı dağıtma ve hata ayıklamaya başlayın.

Yerleştirin ve bir bağlantı geri çağırma uygulaması'ndaki yönergeleri izleyin.

Visual Studio'da ya da seçerek uygulamayı durdurun **hata ayıklamayı Durdur** veya Shift + F5.

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

[!INCLUDE [Next steps](../../../includes/spatial-anchors-quickstarts-nextsteps.md)]

> [!div class="nextstepaction"]
> [Öğretici: Uzamsal bağlayıcılarını cihazlarda paylaşın](../tutorials/tutorial-share-anchors-across-devices.md)
