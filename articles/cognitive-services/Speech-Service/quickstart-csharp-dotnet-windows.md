---
title: "Hızlı Başlangıç: C# altında .NET Framework Bilişsel hizmetler konuşma SDK'sını kullanarak Windows üzerinde konuşma tanıma"
titleSuffix: Microsoft Cognitive Services
description: C# altında .NET Framework Bilişsel hizmetler konuşma SDK'sını kullanarak Windows üzerinde Konuşma tanımayı öğrenmesine
services: cognitive-services
author: wolfma61
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 07/16/2018
ms.author: wolfma
ms.openlocfilehash: 8b257a6f4c32b4013ac0478d82dc1f7f32675b9b
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2018
ms.locfileid: "39578169"
---
# <a name="quickstart-recognize-speech-in-c-under-net-framework-on-windows-using-the-speech-sdk"></a>Hızlı Başlangıç: C# altında .NET Framework Speech SDK'sı kullanarak Windows üzerinde konuşma tanıma

[!include[Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Bu makalede, Konuşmayı metne dönüştürme özelliği Bilişsel hizmetler konuşma SDK'sını kullanarak Windows üzerinde .NET Framework için C# konsol uygulaması oluşturulacağını öğrenin.
Uygulamanın oluşturulduğu [Microsoft Bilişsel hizmetler konuşma SDK'sı NuGet paketi](https://aka.ms/csspeech/nuget) ve Microsoft Visual Studio 2017.

## <a name="prerequisites"></a>Önkoşullar

* Konuşma hizmeti için bir abonelik anahtarı. Bkz: [konuşma hizmeti ücretsiz olarak deneyin](get-started.md).
* Bir çalışma mikrofon içeren bir Windows bilgisayar.
* Visual Studio 2017, Community sürümü veya üzeri.
* **.NET Masaüstü geliştirmesinden** Visual Studio'da iş yükü. İçinde etkinleştirebilirsiniz **Araçları** \> **araçları ve özellikleri Al**.

  ![.NET masaüstü geliştirme etkinleştir](media/sdk/vs-enable-net-desktop-workload.png)

  ![.NET Core çoklu platform geliştirme etkinleştir](media/sdk/vs-enable-net-desktop-workload.png)

## <a name="create-a-visual-studio-project"></a>Visual Studio projesi oluşturma

1. Visual Studio 2017'de yeni bir Visual C# konsol uygulaması oluşturun. İçinde **yeni proje** iletişim kutusunda, sol bölmeden genişletin **yüklü** \> **Visual C#** \> **WindowsMasaüstü** seçip **konsol uygulaması (.NET Framework)**. Proje adı olarak *helloworld*.

    ![Visual C# konsol uygulaması (.NET Framework) oluşturma](media/sdk/qs-csharp-dotnet-windows-01-new-console-app.png "Visual C# konsol uygulaması (.NET Framework) oluşturma")

1. Yükleme ve başvuru [konuşma SDK'sı NuGet paketi](https://aka.ms/csspeech/nuget). Çözüm Gezgini'nde çözüme sağ tıklayıp seçin **çözüm için NuGet paketlerini Yönet**.

    ![Sağ tıklatın, çözüm için NuGet paketlerini Yönet](media/sdk/qs-csharp-dotnet-windows-02-manage-nuget-packages.png "çözüm için NuGet paketlerini Yönet")

1. Sağ üst köşede, içinde **paket kaynağı** alanın, Seç **Nuget.org**. Arama `Microsoft.CognitiveServices.Speech` paketini ve yükleme içine **helloworld** proje.

    ![Microsoft.CognitiveServices.Speech NuGet paketini yüklemek](media/sdk/qs-csharp-dotnet-windows-03-nuget-install-0.5.0.png "yükleme Nuget paketi")

1. Görüntülenen lisansı kabul edin.

    ![Bu lisansı kabul](media/sdk/qs-csharp-dotnet-windows-04-nuget-license.png "lisansı kabul edin")

1. Paket Yöneticisi Konsolu'nda aşağıdaki çıktı satırı görüntülenir.

   ```text
   Successfully installed 'Microsoft.CognitiveServices.Speech 0.5.0' to helloworld
   ```

## <a name="create-a-platform-configuration-matching-your-pc-architecture"></a>PC mimarinizle eşleşen bir platform yapılandırmasını oluşturma

Bu bölümde, yeni bir platform, işlemci mimarisiyle eşleşen yapılandırmasına ekleyin.

1. Configuration Manager'ı başlatın. Seçin **derleme** > **Configuration Manager**.

    ![Configuration manager başlatma](media/sdk/qs-csharp-dotnet-windows-05-cfg-manager-click.png "Yapılandırma Yöneticisi'ni başlatma")

1. İçinde **Configuration Manager** iletişim kutusunda, yeni bir platform ekleyin. Gelen **etkin çözüm platformu** aşağı açılan listesinden **yeni**.

    ![Configuration manager pencerenin altında yeni bir platform Ekle](media/sdk/qs-csharp-dotnet-windows-06-cfg-manager-new.png "configuration manager pencerenin altında yeni bir platform Ekle")

1. 64 bit Windows çalıştırıyorsanız, adlı yeni bir platform yapılandırmasını oluşturma `x64`. 32 bit Windows çalıştırıyorsanız, adlı yeni bir platform yapılandırmasını oluşturma `x86`. Bu makalede, oluşturduğunuz bir `x64` platform yapılandırma.

    ![64 bit Windows üzerinde "x64" adlı yeni bir platform Ekle](media/sdk/qs-csharp-dotnet-windows-07-cfg-manager-add-x64.png "Ekle x64 platformu")

## <a name="add-the-sample-code"></a>Örnek kod ekleyin

1. Açık `Program.cs` ve tüm kodu aşağıdakiyle değiştirin.

    [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-dotnet-windows/helloworld/Program.cs#code)]

1. Dize değiştirin `YourSubscriptionKey` abonelik.

1. Dize değiştirin `YourServiceRegion` ile [bölge](regions.md) aboneliğinizle ilişkili (örneğin, `westus` ücretsiz deneme aboneliği için).

1. Değişiklikleri projeye kaydedin.

## <a name="build-and-run-the-sample"></a>Örneği derleme ve çalıştırma

1. Uygulamayı oluşturun. Menü çubuğundan seçin **derleme** > **Çözümü Derle**. Kod hatasız artık derlemeniz gerekir.

    ![Başarılı derleme](media/sdk/qs-csharp-dotnet-windows-08-build.png "başarılı derleme")

1. Uygulamayı başlatın. Menü çubuğundan seçin **hata ayıklama** > **hata ayıklamayı Başlat**, veya basın **F5**.

    ![Uygulamayı hata ayıklama içine başlatın](media/sdk/qs-csharp-dotnet-windows-09-start-debugging.png "INTO hata ayıklaması uygulamayı başlatın")

1. Söyleyin bir şey (İngilizce) isteyip istemediğinizi soran bir konsol penceresi görünür. Tanınan metin ardından aynı pencerede görünür.

    ![Konsol çıktısı sonra başarılı tanıma](media/sdk/qs-csharp-dotnet-windows-10-console-output.png "başarılı tanıma sonra konsol çıktısı")

[!include[Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Bu örnekte arayın `quickstart/csharp-dotnet-windows` klasör.

## <a name="next-steps"></a>Sonraki adımlar

- [Konuşma Çevir](how-to-translate-speech-csharp.md)
- [Akustik model özelleştirme](how-to-customize-acoustic-models.md)
- [Dil modeli özelleştirme](how-to-customize-language-model.md)
