---
author: erhopf
ms.service: cognitive-services
ms.topic: include
ms.date: 2/20/2019
ms.author: erhopf
ms.openlocfilehash: fe2978e176b986164ebb01fddbd29481f8a117bd
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66145422"
---
1. Visual Studio 2017'yi başlatın.

1. **Evrensel Windows Platformu geliştirmesi** iş yükünün bulunduğundan emin olun. Visual Studio yükleyicisini açmak için Visual Studio menü çubuğundan **Araçlar** > **Araçları ve Özellikleri Al**'ı seçin. Bu iş yükü zaten etkinse iletişim kutusunu kapatın.

    ![Visual Studio yükleyicisinin İş yükleri sekmesi vurgulanmış olarak ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/vs-enable-uwp-workload.png)

    Etkin değilse **.NET çoklu platform geliştirme**'nin yanındaki kutuyu ve iletişim kutusunun sağ alt köşesindeki **Değiştir**'i seçin. Yeni özelliğin yüklenmesi bir dakika sürer.

1. Boş bir Visual C# Evrensel Windows uygulaması oluşturun. İlk olarak, menüden **Dosya** > **Yeni** > **Proje** seçeneğini belirleyin. **Yeni Proje** iletişim kutusunda, sol bölmeden **Yüklü** > **Visual C#** > **Windows Evrensel**'i genişletin. Sonra **Boş Uygulama (Evrensel Windows)** seçeneğini belirleyin. Proje adı olarak *helloworld* girin.

    ![Yeni Proje iletişim kutusunun ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-uwp-01-new-blank-app.png)

1. Speech SDK'sı, uygulamanızı Windows 10 Fall Creators Update veya üzeri oluşturulması gerekir. Çıkan **Yeni Evrensel Windows Platformu Projesi** penceresinde **Windows 10 Fall Creators Update (10.0; Sürüm 16299)** seçeneğini **En düşük sürüm** olarak belirleyin. **Hedef sürümü** kutusunda, bu veya sonraki bir sürümü seçin ve ardından **Tamam**’a tıklayın.

    ![Yeni Evrensel Windows Platformu Projesi penceresinin ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-uwp-02-new-uwp-project.png)

1. 64 bit Windows işletim sistemini kullanıyorsanız, Visual Studio araç çubuğundaki açılan menüyü kullanarak yapı platformunuzu `x64` işletim sistemine geçirebilirsiniz. (64 bit Windows, 32 bit uygulamaları çalıştırabilir, bu yüzden dilerseniz `x86` olarak ayarlanmış şekilde bırakabilirsiniz.)

   ![x64 seçeneğinin vurgulandığı Visual Studio araç çubuğunun ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-uwp-03-switch-to-x64.png)

   > [!NOTE]
   > Speech SDK'sı yalnızca Intel uyumlu işlemci destekler. ARM şu anda desteklenmiyor.

1. [Konuşma SDK NuGet paketi](https://aka.ms/csspeech/nuget)'ni yükleyip başvurulara ekleyin. Çözüm Gezgini'nde çözüme sağ tıklayın ve **Çözüm için NuGet Paketlerini Yönet**'i seçin.

    ![Çözüm Gezgini'nin, Çözüm için NuGet Paketlerini Yönet seçeneğinin vurgulandığı ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-uwp-04-manage-nuget-packages.png)

1. Sağ üst köşede, **Paket Kaynağı** alanında **nuget.org**'u seçin. `Microsoft.CognitiveServices.Speech` paketini arayın ve **helloworld** projesine yükleyin.

    ![Çözüm için Paketleri Yönet iletişim kutusunun ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-uwp-05-nuget-install-1.0.0.png "NuGet paketini yükle")

1. NuGet paketinin yükleme işlemini başlatmak için görüntülenen lisansı kabul edin.

    ![Lisans Kabulü iletişim kutusunun ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-uwp-06-nuget-license.png "Lisansı kabul et")

1. Paket Yöneticisi konsolunda aşağıdaki çıkış satırı görüntülenir.

   ```text
   Successfully installed 'Microsoft.CognitiveServices.Speech 1.5.0' to helloworld
   ```

1. Uygulama, konuşma girdisi için mikrofon kullandığından, **Mikrofon** özelliğini projeye ekleyin. Çözüm Gezgini'nde, uygulama bildiriminizi düzenlemek için **Package.appxmanifest** dosyasına çift tıklayın. Ardından **Özellikler** sekmesine geçin, **Mikrofon** özelliği kutusunu seçin ve değişikliklerinizi kaydedin.

   ![Özellikler ve Mikrofonun vurgulandığı Visual Studio uygulama bildiriminin ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-uwp-07-capabilities.png)
