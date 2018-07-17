---
title: "Hızlı Başlangıç: C# Bilişsel hizmetler konuşma SDK'sını kullanarak Windows üzerinde .NET Core altında konuşma tanıma | Microsoft Docs"
titleSuffix: Microsoft Cognitive Services
description: Bilişsel hizmetler konuşma SDK'sını kullanarak Windows üzerinde .NET Core altında C# ' de Konuşma tanıma öğrenin
services: cognitive-services
author: wolfma61
manager: onano
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 07/16/2018
ms.author: wolfma
ms.openlocfilehash: f00653d0c58f52c73126693422ab0a485540dd7a
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39072600"
---
# <a name="quickstart-recognize-speech-in-c-under-net-core-on-windows-using-the-speech-sdk"></a>Hızlı Başlangıç: C# Speech SDK'sı kullanarak Windows üzerinde .NET Core altında konuşma tanıma

Bu makalede, Konuşmayı metne dönüştürme özelliği Bilişsel hizmetler konuşma SDK'sını kullanarak Windows üzerinde .NET Core için bir C# konsol uygulaması oluşturulacağını öğrenin.
Uygulama dayanır [Microsoft Bilişsel hizmetler konuşma SDK'sı NuGet paketi](https://aka.ms/csspeech/nuget) ve Microsoft Visual Studio 2017.

## <a name="prerequisites"></a>Önkoşullar

* Konuşma hizmeti için bir abonelik anahtarı. Bkz: [konuşma hizmeti ücretsiz olarak deneyin](get-started.md).
* Bir çalışma mikrofon içeren bir Windows bilgisayar.
* [Microsoft Visual Studio 2017](https://www.visualstudio.com/), Community sürümü veya üzeri.
* **.NET Core çoklu platform geliştirme** Visual Studio'da iş yükü. İçinde etkinleştirebilirsiniz **Araçları** \> **araçları ve özellikleri Al**.

  ![.NET Core çoklu platform geliştirme etkinleştir](media/sdk/vs-enable-net-core-workload.png)

## <a name="create-a-visual-studio-project"></a>Visual Studio projesi oluşturma

1. Visual Studio 2017'de yeni Visual C# .NET Core konsol uygulaması oluşturun. İçinde **yeni proje** iletişim kutusunda, sol bölmeden genişletin **yüklü** \> **Visual C#** \> **.NET Core**seçip **konsol uygulaması (.NET Core)**. Proje adı olarak *helloworld*.

    ![Visual C# konsol uygulaması (.NET Core) oluşturma](media/sdk/qs-csharp-dotnetcore-windows-01-new-console-app.png "Visual C# konsol uygulaması (.NET Core) oluşturma")

1. Yükleme ve başvuru [konuşma SDK'sı NuGet paketi](https://aka.ms/csspeech/nuget). Çözüm Gezgini'nde çözüme sağ tıklayıp seçin **çözüm için NuGet paketlerini Yönet**.

    ![Sağ tıklatın, çözüm için NuGet paketlerini Yönet](media/sdk/qs-csharp-dotnetcore-windows-02-manage-nuget-packages.png "çözüm için NuGet paketlerini Yönet")

1. Sağ üst köşede, içinde **paket kaynağı** alanın, Seç **Nuget.org**. Arama ve yükleme `Microsoft.CognitiveServices.Speech` paketini ve yükleme içine **helloworld** proje.

    ![Microsoft.CognitiveServices.Speech NuGet paketini yüklemek](media/sdk/qs-csharp-dotnetcore-windows-03-nuget-install-0.5.0.png "yükleme Nuget paketi")

1. Açılır lisans ekranda lisansı kabul edin:

    ![Bu lisansı kabul](media/sdk/qs-csharp-dotnetcore-windows-04-nuget-license.png "lisansı kabul edin")

## <a name="add-the-sample-code"></a>Örnek kod ekleyin

1. İçinde `Program.cs` dosya Visual Studio projenize, tüm kodu aşağıdakiyle değiştirin:

    [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-dotnetcore-windows/helloworld/Program.cs#code)]

1. Dize değiştirin `YourSubscriptionKey` abonelik.

1. Dize değiştirin `YourServiceRegion` ile [bölge](regions.md) aboneliğinizle ilişkili (örneğin, `westus` ücretsiz deneme aboneliği için).

1. Değişiklikleri projeye kaydedin.

## <a name="build-and-run-the-sample"></a>Örneği derleme ve çalıştırma

1. Uygulamayı oluşturun. Menü çubuğundan seçin **derleme** > **Çözümü Derle**. Kod hatasız artık derlemeniz gerekir:

    ![Başarılı derleme](media/sdk/qs-csharp-dotnetcore-windows-05-build.png "başarılı derleme")

1. Uygulamayı başlatın. Menü çubuğundan seçin **hata ayıklama** > **hata ayıklamayı Başlat**, veya basın **F5**.

    ![Uygulamayı hata ayıklama içine başlatın](media/sdk/qs-csharp-dotnetcore-windows-06-start-debugging.png "INTO hata ayıklaması uygulamayı başlatın")

1. Bir şeyin (İngilizce) söyleyin isteyip istemediğinizi soran bir konsol penceresi açılır.
Tanıma sonucunu ekranında görüntülenir.

    ![Konsol çıktısı sonra başarılı tanıma](media/sdk/qs-csharp-dotnetcore-windows-07-console-output.png "başarılı tanıma sonra konsol çıktısı")

[!include[Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Bu örnekte arayın `quickstart/csharp-dotnetcore-windows` klasör.

## <a name="next-steps"></a>Sonraki adımlar

- [Konuşma Çevir](how-to-translate-speech.md)
- [Akustik model özelleştirme](how-to-customize-acoustic-models.md)
- [Dil modeli özelleştirme](how-to-customize-language-model.md)
