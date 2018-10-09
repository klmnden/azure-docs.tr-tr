---
title: "Hızlı Başlangıç: Bilişsel Hizmetler Konuşma SDK'sını kullanarak Windows'da .NET Framework altında C# ile konuşma tanıma"
titleSuffix: Microsoft Cognitive Services
description: Bilişsel Hizmetler Konuşma SDK'sını kullanarak Windows'da .NET Framework altında C# ile konuşma tanımayı öğrenin
services: cognitive-services
author: wolfma61
ms.service: cognitive-services
ms.component: speech-service
ms.topic: quickstart
ms.date: 09/24/2018
ms.author: wolfma
ms.openlocfilehash: 32b484451c4ee2264c25cca92b1d03d91b955a29
ms.sourcegitcommit: 715813af8cde40407bd3332dd922a918de46a91a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47054005"
---
# <a name="quickstart-recognize-speech-in-c-under-net-framework-on-windows-by-using-the-speech-sdk"></a>Hızlı Başlangıç: Konuşma SDK'sını kullanarak Windows'da .NET Framework altında C# ile konuşma tanıma

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Bu makalede Windows'da .NET Framework için [Konuşma SDK'sı](speech-sdk.md)'nı kullanarak bir C# konsol uygulaması oluşturacaksınız. Bilgisayarınızın mikrofonundan gerçek zamanda konuşmayı yazıya dönüştüreceksiniz. Uygulama [Konuşma SDK'sı NuGet paketi](https://aka.ms/csspeech/nuget) ve Microsoft Visual Studio 2017 (herhangi bir sürüm) ile geliştirilmiştir.

## <a name="prerequisites"></a>Ön koşullar

Bu Hızlı Başlangıç'ı tamamlamak için bir Konuşma hizmeti abonelik anahtarınız olması gerekir. Anahtarı ücretsiz edinebilirsiniz. Ayrıntılar için bkz: [Konuşma hizmetini ücretsiz olarak deneme](get-started.md).

## <a name="create-a-visual-studio-project"></a>Visual Studio projesi oluşturma

[!INCLUDE [Create project ](../../../includes/cognitive-services-speech-service-create-speech-project-vs-csharp.md)]

## <a name="add-sample-code"></a>Örnek kodu ekleyin

1. `Program.cs` dosyasını açın ve tüm kodu aşağıdakiyle değiştirin.

    [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-dotnet-windows/helloworld/Program.cs#code)]

1. Aynı dosyada `YourSubscriptionKey` dizesini Konuşma hizmeti abonelik anahtarınız ile değiştirin.

1. Ayrıca `YourServiceRegion` dizesini de aboneliğinizle ilişkili [bölge](regions.md) ile (örneğin ücretsiz deneme aboneliğinde `westus`) değiştirin.

1. Değişiklikleri projeye kaydedin.

## <a name="build-and-run-the-app"></a>Uygulamayı derleme ve çalıştırma

1. Uygulamayı derleyin. Menü çubuğundan **Derle** > **Çözümü Derle**'yi seçin. Kodun artık hatasız derlenmesi gerekir.

    ![Visual Studio uygulamasının, Çözümü Derle seçeneği vurgulanmış olarak ekran görüntüsü](media/sdk/qs-csharp-dotnet-windows-08-build.png "Başarılı derleme")

1. Uygulamayı başlatın. Menü çubuğundan **Hata Ayıklama** > **Hata Ayıklamayı Başlat**'ı seçin veya **F5** tuşuna basın.

    ![Visual Studio uygulamasının, Hata Ayıklamayı Başlat seçeneği vurgulanmış olarak ekran görüntüsü](media/sdk/qs-csharp-dotnet-windows-09-start-debugging.png "Uygulamayı hata ayıklamada başlat")

1. Bir şey söylemenizi isteyen bir konsol penceresi görünür. İngilizce bir deyim ya da cümle söyleyin. Söyledikleriniz Konuşma hizmetine aktarılır ve metne dönüştürülür; metin aynı pencerede görünür.

    ![Başarılı tanımadan sonra konsol çıktısının ekran görüntüsü](media/sdk/qs-csharp-dotnet-windows-10-console-output.png "Başarılı tanımadan sonra konsol çıktısı")

[!INCLUDE [Download this sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
`quickstart/csharp-dotnet-windows` klasöründe bu örneği arayın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [C# için Konuşma SDK'sını kullanarak konuşmadaki amacı tanıma](how-to-recognize-intents-from-speech-csharp.md)

## <a name="see-also"></a>Ayrıca bkz.

- [Konuşmayı çevirme](how-to-translate-speech-csharp.md)
- [Akustik modelleri özelleştirme](how-to-customize-acoustic-models.md)
- [Dil modellerini özelleştirme](how-to-customize-language-model.md)
