---
author: erhopf
ms.service: cognitive-services
ms.topic: include
ms.date: 2/20/2019
ms.author: erhopf
ms.openlocfilehash: b5a24d83faef5a895317f162178f8bd588a1466d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60620156"
---
1. Visual Studio 2017'yi başlatın.

1. **C++ ile masaüstü geliştirme** iş yükünün kullanılabilir olduğundan emin olun. Visual Studio yükleyicisini açmak için Visual Studio menü çubuğundan **Araçlar** > **Araçları ve Özellikleri Al**'ı seçin. Bu iş yükü zaten etkinse sonraki adıma atlayın.

    ![Visual Studio İş Yükleri’nin ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/vs-enable-cpp-workload.png)

    Aksi takdirde, **C++ ile masaüstü geliştirme**’nin yanındaki kutuyu işaretleyin.

1. **NuGet paket yöneticisi** bileşeninin kullanılabilir olduğundan emin olun. Visual Studio yükleyicisi iletişim kutusunun **Bağımsız bileşenler** sekmesine geçin ve etkin değilse **NuGet paket yöneticisi**’ni seçin.

      ![Visual Studio Bağımsız bileşenler sekmesinin ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/vs-enable-nuget-package-manager.png)

1. C++ iş yükünü veya NuGet’i etkinleştirmeniz gerekiyorsa, **Değiştir**’i seçin (iletişim kutusunun sağ alt köşesinde). Yeni özelliklerin yüklenmesi bir dakika sürer. Her iki özellik de zaten etkinse, iletişim kutusunu kapatmanız yeterlidir.

1. Yeni bir Visual C++ Windows Masaüstü Windows Konsol Uygulaması oluşturun. İlk olarak, menüden **Dosya** > **Yeni** > **Proje** seçeneğini belirleyin. **Yeni Proje** iletişim kutusunda, sol bölmeden **Yüklü** > **Visual C++** > **Windows Masaüstü**'nü genişletin. Ardından **Windows Konsol Uygulaması**’nı seçin. Proje adı olarak *helloworld* girin.

    ![Yeni Proje iletişim kutusunun ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/qs-cpp-windows-01-new-console-app.png)

1. 64 bit Windows işletim sistemini kullanıyorsanız, Visual Studio araç çubuğundaki açılan menüyü kullanarak yapı platformunuzu `x64` işletim sistemine geçirebilirsiniz. (Windows’un 64 bit sürümleri 32 bit uygulamaları çalıştırabildiğinden bu bir gereksinim değildir.)

    ![x64 seçeneğinin vurgulandığı Visual Studio araç çubuğunun ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/qs-cpp-windows-02-switch-to-x64.png)

1. Çözüm Gezgini'nde çözüme sağ tıklayın ve **Çözüm için NuGet Paketlerini Yönet**'i seçin.

    ![Çözüm Gezgini'nin, Çözüm için NuGet Paketlerini Yönet seçeneğinin vurgulandığı ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/qs-cpp-windows-03-manage-nuget-packages.png)

1. Sağ üst köşede, **Paket Kaynağı** alanında **nuget.org**'u seçin. `Microsoft.CognitiveServices.Speech` paketini arayın ve **helloworld** projesine yükleyin.

    ![Çözüm için Paketleri Yönet iletişim kutusunun ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/qs-cpp-windows-04-nuget-install-1.0.0.png)

    > [!NOTE]
    > Bilişsel Hizmetler Konuşma SDK'sının geçerli sürümü: `1.4.0`.

1. NuGet paketinin yükleme işlemini başlatmak için görüntülenen lisansı kabul edin.

    ![Lisans Kabulü iletişim kutusunun ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/qs-cpp-windows-05-nuget-license.png)

Paket yüklendikten sonra Paket Yöneticisi konsolunda bir onay görünür.
