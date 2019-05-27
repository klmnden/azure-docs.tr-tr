---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 09/13/2018
ms.author: wolfma
ms.openlocfilehash: afe6f1493c7fa8272c67f23d6708ad6e4eea9381
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66145496"
---
1. Visual Studio 2017'yi başlatın.

1. Visual Studio'da menü çubuğundan seçin **Araçlar > Araçları edinme** emin olun **.NET Masaüstü geliştirmesinden** iş yükü kullanılabilir. İş yükü yüklü olmadığından, onay kutusunu işaretleyin ve ardından tıklayın **Değiştir** yüklemeyi başlatmak için. Bu, indirmek ve yüklemek için birkaç dakika sürebilir.

   Varsa yanındaki onay kutusunu **.NET Masaüstü geliştirmesinden** olduğu belirlenirse, iletişim kutusu artık kapatabilirsiniz.

   ![.NET masaüstü geliştirmesini etkinleştirme](~/articles/cognitive-services/speech-service/media/sdk/vs-enable-net-desktop-workload.png)

1. Ardından, bir proje oluşturalım. Menü çubuğundan seçin **Dosya > Yeni > Proje**. İletişim kutusu göründüğünde, bu bölümler sol panelden genişletin **yüklü > Visual C# > Windows Masaüstü** seçip **konsol uygulaması (.NET Framework)**. Bu projenin adı *helloworld*.

    ![Visual C# Konsol Uygulaması (.NET Framework) oluşturma](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-01-new-console-app.png "Visual C# Konsol Uygulaması (.NET Framework) oluşturma")

1. Proje ayarlama, yüklememiz gerekir [konuşma SDK'sı NuGet paketi](https://aka.ms/csspeech/nuget) ve bizim kodda başvuru. Çözüm Gezgini'nde bulun ve helloworld üzerinde sağ tıklayın. Menüden **NuGet paketlerini Yönet...** .

   ![Çözüm için NuGet Paketlerini Yönet](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-02-manage-nuget-packages.png "Çözüm için NuGet Paketlerini Yönet")'e sağ tıklayın

1. NuGet Paket Yöneticisi'nin sağ üst köşedeki bulun **paket kaynağı** açılan emin olun **nuget.org** seçilir. Ardından seçin **Gözat** araması `Microsoft.CognitiveServices.Speech` paketini ve en son kararlı sürümünü yükleyin.

   ![Microsoft.CognitiveServices.Speech NuGet Paketini yükleyin](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-03-nuget-install-1.0.0.png "NuGet Paketini yükleyin")

1. Tüm anlaşmalar ve lisansları yüklemeyi başlatmak için kabul edin.

   ![Lisansı kabul edin](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-04-nuget-license.png "Lisansı kabul edin")

    Paket yüklendikten sonra Paket Yöneticisi konsolunda bir onay görünür.

1. Sonraki adım, konsol uygulaması derleme ve çalıştırma için kullanmakta olduğunuz bilgisayarın mimarisiyle eşleşen bir platform yapılandırmasını oluşturmaktır. Menü çubuğundan seçin **derleme** > **Configuration Manager...** .

    ![Yapılandırma yöneticisini başlatma](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-05-cfg-manager-click.png "Yapılandırma yöneticisini başlatma")

1. İçinde **Configuration Manager** iletişim kutusunda, bulun **etkin çözüm platformu** seçin ve açılır listede **yeni**.

    ![Configuration Manager penceresinin altından yeni bir platform ekleyin](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-06-cfg-manager-new.png "Configuration Manager penceresinin altından yeni bir platform ekleyin")

1. 64 bit Windows, sorulduğunda çalıştırıyorsanız **yazın veya seçin yeni platformu**, `x64`. 32 bit Windows çalıştırıyorsanız seçin `x86`. İşlemi tamamladığınızda, tıklayın **Tamam**.

    ![64 bit Windows üzerinde "x64" adlı yeni bir platform ekleyin](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-07-cfg-manager-add-x64.png "x64 platformunu ekleyin")
