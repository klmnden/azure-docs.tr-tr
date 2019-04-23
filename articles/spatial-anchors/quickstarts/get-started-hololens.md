---
title: Hızlı Başlangıç - Azure uzamsal Çıpasıyla HoloLens uygulaması oluşturma | Microsoft Docs
description: Bu hızlı başlangıçta, uzamsal bağlayıcılarını kullanarak bir HoloLens uygulamasının nasıl oluşturulacağını öğrenin.
author: craigktreasure
manager: aliemami
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 02/24/2019
ms.topic: quickstart
ms.service: azure-spatial-anchors
ms.openlocfilehash: 7a9eedfe851bb8b5519d0831f47cf6787345a1f8
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59996297"
---
# <a name="quickstart-create-a-hololens-app-with-azure-spatial-anchors-in-cwinrt-and-directx"></a>Hızlı Başlangıç: C + Azure uzamsal bağlayıcıları ile bir HoloLens uygulaması oluşturma +/ WinRT ve DirectX

Bu hızlı başlangıçta HoloLens kullanarak uygulama oluşturma konusunu kapsar [Azure uzamsal bağlayıcılarını](../overview.md) içinde C++/WinRT ve DirectX. Azure uzamsal bağlayıcılarını konumlarına cihazlar arasında zaman içinde kalıcı nesneler kullanarak karma gerçeklik deneyimleri oluşturmanıza olanak tanıyan platformlar arası Geliştirici hizmetidir. İşlemi tamamladığınızda, kaydedebilir ve uzamsal bağlantı Hatırlayacağınız bir HoloLens uygulaması gerekir.

Şunları öğrenirsiniz:

> [!div class="checklist"]
> * Uzamsal bağlayıcılarını hesabı oluşturma
> * Uzamsal bağlayıcılarını hesap tanımlayıcısı ve hesap anahtarını yapılandırma
> * Dağıtma ve HoloLens cihazda çalıştırma

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlamak için aşağıdakileri yaptığınızdan emin olun:

- Bir Windows makineyle <a href="https://www.visualstudio.com/downloads/" target="_blank">Visual Studio 2017 +</a> ile yüklenen **Evrensel Windows platformu geliştirme** iş yükü ve **Windows 10 SDK (10.0.17763.0 ya da daha yeni)** bileşeni ve <a href="https://git-scm.com/download/win" target="_blank">Git için Windows</a>.
- [ C++WinRT Visual Studio Uzantısı (VSIX)](https://aka.ms/cppwinrt/vsix) for Visual Studio yüklü [Visual Studio Market](https://marketplace.visualstudio.com/).
- HoloLens cihazla [Geliştirici modu](https://docs.microsoft.com/windows/mixed-reality/using-visual-studio) etkin. Bu makale gerekir HoloLens cihazla [Windows 10 Ekim 2018 güncelleştirmesi](https://docs.microsoft.com/windows/mixed-reality/release-notes-october-2018 ) (RS5 olarak da bilinir). HoloLens üzerinde en son sürümüne güncelleştirmek için **ayarları** uygulama, Git **güncelleştirme ve güvenlik**, ardından **Güncelleştirmeleri denetle** düğmesi.
- Uygulamanızı ayarlamalısınız **spatialPerception** AppX bildirimi özelliği.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="open-the-sample-project"></a>Örnek projeyi açın

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

Açık `HoloLens\DirectX\SampleHoloLens.sln` Visual Studio'da.

## <a name="configure-account-identifier-and-key"></a>Hesap Kimliği ve anahtarını yapılandırma

Sonraki adım uygulamayı hesap tanımlayıcısı ve hesap anahtarını kullanacak şekilde yapılandırmaktır. Bir metin düzenleyiciye kopyaladığınız zaman [uzamsal bağlayıcılarını kaynağı ayarı](#create-a-spatial-anchors-resource).

Açık `HoloLens\DirectX\SampleHoloLens\ViewController.cpp`.

Bulun `SpatialAnchorsAccountKey` değiştirin ve alan `Set me` ile hesap anahtarı.

Bulun `SpatialAnchorsAccountId` değiştirin ve alan `Set me` hesap tanımlayıcı ile.

## <a name="deploy-the-app-to-your-hololens"></a>HoloLens için uygulama dağıtma

Değiştirme **çözüm yapılandırması** için **yayın**, değiştirme **çözüm platformu** için **x86**seçip **cihaz**  dağıtım hedef seçenekleri.

HoloLens 2 kullanıyorsanız kullanın **ARM** olarak **çözüm platformu**, yerine **x86**.

![Visual Studio yapılandırması](./media/get-started-hololens/visual-studio-configuration.png)

HoloLens cihazda güç, oturum açın ve bir USB kablosu kullanarak Bilgisayara bağlanın.

Seçin **hata ayıklama** > **hata ayıklamayı Başlat** uygulamanızı dağıtma ve hata ayıklamaya başlayın.

Yerleştirin ve bir bağlantı geri çağırma uygulaması'ndaki yönergeleri izleyin.

Visual Studio'da seçerek uygulamayı durdurun **hata ayıklamayı Durdur** ya basarak **Shift + F5 tuşlarına basarak**.

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

[!INCLUDE [Next steps](../../../includes/spatial-anchors-quickstarts-nextsteps.md)]

> [!div class="nextstepaction"]
> [Öğretici: Cihazlar arasında paylaşımı uzamsal yer işaretleri](../tutorials/tutorial-share-anchors-across-devices.md)
