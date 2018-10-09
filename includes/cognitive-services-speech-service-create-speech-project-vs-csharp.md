---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 09/13/2018
ms.author: wolfma
ms.openlocfilehash: 8ff8e8341b6f39f66c2cc8014d41d3d3a2918d2b
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/28/2018
ms.locfileid: "47454407"
---
1. Visual Studio 2017'yi başlatın.
 
1. **.NET masaüstü ortamı** iş yükünün bulunduğundan emin olun. Visual Studio yükleyicisini açmak için Visual Studio menü çubuğundan **Araçlar** \> **Araçları ve Özellikleri Al**'ı seçin. Bu iş yükü zaten etkinse iletişim kutusunu kapatın. 

    Değilse, **.NET masaüstü geliştirmesi**'nin yanındaki onay kutusunu işaretleyin, ardından iletişim kutusunun sağ alt köşesindeki **Değiştir** düğmesine tıklayın. Yeni özelliğin yüklenme birkaç saniye sürer.

    ![.NET masaüstü geliştirmesini etkinleştirme](~/articles/cognitive-services/speech-service/media/sdk/vs-enable-net-desktop-workload.png)

1. Yeni bir Visual C# Konsol Uygulaması oluşturun. **Yeni Proje** iletişim kutusunda, sol bölmeden **Yüklü** \> **Visual C#** \> **Windows Masaüstü**'nü genişletin ve **Konsol Uygulaması (.NET Framework)** seçeneğini işaretleyin. Proje adı olarak *helloworld* girin.

    ![Visual C# Konsol Uygulaması (.NET Framework) oluşturma](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-01-new-console-app.png "Visual C# Konsol Uygulaması (.NET Framework) oluşturma")

1. [Konuşma SDK NuGet paketi](https://aka.ms/csspeech/nuget)'ni yükleyip başvurulara ekleyin. Çözüm Gezgini'nde çözüme sağ tıklayın ve **Çözüm için NuGet Paketlerini Yönet**'i seçin.

    ![Çözüm için NuGet Paketlerini Yönet](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-02-manage-nuget-packages.png "Çözüm için NuGet Paketlerini Yönet")'e sağ tıklayın

1. Sağ üst köşede, **Paket Kaynağı** alanında **nuget.org**'u seçin. `Microsoft.CognitiveServices.Speech` paketini arayın ve **helloworld** projesine yükleyin.

    ![Microsoft.CognitiveServices.Speech NuGet Paketini yükleyin](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-03-nuget-install-1.0.0.png "NuGet Paketini yükleyin")

1. NuGet paketinin yükleme işlemini başlatmak için görüntülenen lisansı kabul edin.

    ![Lisansı kabul edin](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-04-nuget-license.png "Lisansı kabul edin")

    Paket yüklendikten sonra Paket Yöneticisi konsolunda bir onay görünür.

1. Configuration Manager aracılığıyla PC mimariniz ile eşleşen bir platform yapılandırması oluşturun. **Derle** > **Configuration Manager**'ı seçin.

    ![Yapılandırma yöneticisini başlatma](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-05-cfg-manager-click.png "Yapılandırma yöneticisini başlatma")

1. **Configuration Manager** iletişim kutusunda yeni bir platform yükleyin. Aşağı açılan **Etkin çözüm platformu** listesinden **Yeni**'yi seçin.

    ![Configuration Manager penceresinin altından yeni bir platform ekleyin](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-06-cfg-manager-new.png "Configuration Manager penceresinin altından yeni bir platform ekleyin")

1. 64 bit Windows çalıştırıyorsanız `x64` adlı yeni bir platform yapılandırması oluşturun. 32 bit Windows çalıştırıyorsanız `x86` adlı yeni bir platform yapılandırması oluşturun.

    ![64 bit Windows üzerinde "x64" adlı yeni bir platform ekleyin](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-07-cfg-manager-add-x64.png "x64 platformunu ekleyin")


