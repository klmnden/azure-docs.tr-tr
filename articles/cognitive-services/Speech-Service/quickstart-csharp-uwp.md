---
title: 'Hızlı Başlangıç: Konuşma tanıma C# (UWP) - konuşma Hizmetleri'
titleSuffix: Azure Cognitive Services
description: Bu makalede, oluşturduğunuz bir C# Bilişsel hizmetler konuşma SDK'sı kullanarak evrensel Windows Platformu (UWP) uygulama. Cihazınızın mikrofonundan gerçek zamanda konuşmayı metne dönüştüreceksiniz. Uygulama, konuşma SDK'sı NuGet paketi ve Microsoft Visual Studio 2017 ile oluşturulmuştur.
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 12/06/2018
ms.author: wolfma
ms.custom: seodec18
ms.openlocfilehash: 674bb3f22f7b8c7c7ea3b52c2f27d862a9961fcb
ms.sourcegitcommit: e6d53649bfb37d01335b6bcfb9de88ac50af23bd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65466243"
---
# <a name="quickstart-recognize-speech-in-a-uwp-app-by-using-the-speech-sdk"></a>Hızlı Başlangıç: Speech SDK'sı kullanarak bir UWP uygulamasında konuşma tanıma

Hızlı Başlangıçlar ücret karşılığında ayrıca [konuşma çevirisi](quickstart-translate-speech-uwp.md) ve [ses öncelikli sanal asistan](quickstart-virtual-assistant-csharp-uwp.md).

İsterseniz, farklı programlama dili ve/veya ortamı seçin:<br/>
[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Bu makalede, geliştirdiğiniz bir C# Evrensel Windows Platformu (UWP; Windows sürüm 1709 daha sonra) uygulama Bilişsel hizmetler kullanarak [Speech SDK'sı](speech-sdk.md). Program, cihazınızın mikrofondan gerçek zamanlı olarak metne konuşma tanıma özelliği. Uygulama [Konuşma SDK'sı NuGet Paketi](https://aka.ms/csspeech/nuget) ve Microsoft Visual Studio 2017 (herhangi bir sürüm) ile geliştirilmiştir.

> [!NOTE]
> Evrensel Windows Platformu; PC, Xbox, Surface Hub ve diğer cihazlar dahil olmak üzere Windows 10 destekleyen tüm cihazlarda çalışan uygulamalar geliştirmenize olanak tanır.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıç şunları gerektirir:

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
* Konuşma hizmeti için bir Azure aboneliği anahtarı. [Ücretsiz edinin](get-started.md).

## <a name="create-a-visual-studio-project"></a>Visual Studio projesi oluşturma

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-uwp-create-proj.md)]

## <a name="add-sample-code"></a>Örnek kod ekleyin

1. Uygulamanın kullanıcı arabirimini XAML kullanılarak tanımlanır. `MainPage.xaml` dosyasını Çözüm Gezgini'nde açın. Tasarımcının XAML görünümünde, aşağıdaki XAML kod parçacığını Kılavuz etiketine ekleyin (`<Grid>` ve `</Grid>` arasında).

   [!code-xml[UI elements](~/samples-cognitive-services-speech-sdk/quickstart/csharp-uwp/helloworld/MainPage.xaml#StackPanel)]

1. Arka plan kod kaynak dosyasını açın `MainPage.xaml.cs` (`MainPage.xaml` altında gruplandırılmış olarak bulabilirsiniz). İçindeki tüm kodu aşağıdakiyle değiştirin.

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-uwp/helloworld/MainPage.xaml.cs#code)]

1. Bu dosyadaki `SpeechRecognitionFromMicrophone_ButtonClicked` işleyicisinde `YourSubscriptionKey` dizesini abonelik anahtarınız ile değiştirin.

1. `SpeechRecognitionFromMicrophone_ButtonClicked` işleyicisinde `YourServiceRegion` dizesini aboneliğinizle ilişkili [bölge](regions.md) ile (örneğin ücretsiz deneme aboneliğinde `westus`) değiştirin.

1. Değişiklikleri projeye kaydedin.

## <a name="build-and-run-the-app"></a>Uygulamayı derleme ve çalıştırma

1. Uygulamayı derleyin. Menü çubuğundan **Derle** > **Çözümü Derle**'yi seçin. Kodun artık hatasız derlenmesi gerekir.

    ![Visual Studio uygulamasının, Çözümü Derle seçeneği vurgulanmış olarak ekran görüntüsü](media/sdk/qs-csharp-uwp-08-build.png "Başarılı derleme")

1. Uygulamayı başlatın. Menü çubuğundan **Hata Ayıklama** > **Hata Ayıklamayı Başlat**'ı seçin veya **F5** tuşuna basın.

    ![Visual Studio uygulamasının, Hata Ayıklamayı Başlat seçeneği vurgulanmış olarak ekran görüntüsü](media/sdk/qs-csharp-uwp-09-start-debugging.png "Uygulamayı hata ayıklamada başlatma")

1. Bir pencere açılır. **Mikrofonu Etkinleştir**’i seçin ve çıkan izin isteğini kabul edin.

    ![İzin isteğinin ekran görüntüsü](media/sdk/qs-csharp-uwp-10-access-prompt.png "Uygulamayı hata ayıklamada başlat")

1. **Mikrofon girdisi ile konuşma tanıma**’yı seçin ve cihazınızın mikrofonuna İngilizce bir deyim ya da cümle söyleyin. Konuşma konuşma hizmetlere iletilen ve transcribed pencerede görünen metin.

    ![Konuşma tanıma kullanıcı arabiriminin ekran görüntüsü](media/sdk/qs-csharp-uwp-11-ui-result.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Keşfedin C# github'da örnekleri](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Ayrıca bkz.

- [Konuşmayı çevirme](how-to-translate-speech-csharp.md)
- [Akustik modelleri özelleştirme](how-to-customize-acoustic-models.md)
- [Dil modellerini özelleştirme](how-to-customize-language-model.md)
