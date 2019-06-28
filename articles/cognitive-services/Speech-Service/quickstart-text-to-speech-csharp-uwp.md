---
title: 'Hızlı Başlangıç: Konuşma sentezlemek C# (UWP) - konuşma Hizmetleri'
titleSuffix: Azure Cognitive Services
description: Bu makalede, oluşturduğunuz bir C# Bilişsel hizmetler konuşma SDK'sı kullanarak evrensel Windows Platformu (UWP) uygulama. Cihazınızın Konuşmacı gerçek zamanlı olarak metinden konuşmaya sentezlemek. Uygulama, konuşma SDK'sı NuGet paketi ve Microsoft Visual Studio 2017 ile oluşturulmuştur.
services: cognitive-services
author: yinhew
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 6/24/2019
ms.author: yinhew
ms.openlocfilehash: c11fbff883d2699bdd0a107fc462524d92ee410d
ms.sourcegitcommit: c63e5031aed4992d5adf45639addcef07c166224
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67467449"
---
# <a name="quickstart-synthesize-speech-in-a-uwp-app-by-using-the-speech-sdk"></a>Hızlı Başlangıç: Bir UWP uygulamasında konuşma Speech SDK'sı kullanarak sentezlemek

Hızlı Başlangıçlar ücret karşılığında ayrıca [konuşma tanıma](quickstart-csharp-uwp.md), [konuşma çevirisi](quickstart-translate-speech-uwp.md) ve [ses öncelikli sanal asistan](quickstart-virtual-assistant-csharp-uwp.md).

Bu makalede, geliştirdiğiniz bir C# Evrensel Windows Platformu (UWP; Windows sürüm 1709 daha sonra) uygulama Bilişsel hizmetler kullanarak [Speech SDK'sı](speech-sdk.md). Program, cihazınızın Konuşmacı gerçek zamanlı olarak metinden konuşmaya oluşturur. Uygulama [Konuşma SDK'sı NuGet Paketi](https://aka.ms/csspeech/nuget) ve Microsoft Visual Studio 2017 (herhangi bir sürüm) ile geliştirilmiştir.

> [!NOTE]
> Evrensel Windows Platformu; PC, Xbox, Surface Hub ve diğer cihazlar dahil olmak üzere Windows 10 destekleyen tüm cihazlarda çalışan uygulamalar geliştirmenize olanak tanır.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıç şunları gerektirir:

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
* Konuşma hizmeti için bir Azure aboneliği anahtarı. [Ücretsiz edinin](get-started.md).

## <a name="create-a-visual-studio-project"></a>Visual Studio projesi oluşturma

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-uwp-create-proj.md)]

## <a name="add-sample-code"></a>Örnek kodu ekleme

1. Uygulamanın kullanıcı arabirimini XAML kullanılarak tanımlanır. `MainPage.xaml` dosyasını Çözüm Gezgini'nde açın. Tasarımcının XAML görünümünde, aşağıdaki XAML kod parçacığını Kılavuz etiketine ekleyin (`<Grid>` ve `</Grid>` arasında).

   [!code-xml[UI elements](~/samples-cognitive-services-speech-sdk/quickstart/text-to-speech/csharp-uwp/helloworld/MainPage.xaml#StackPanel)]

1. Arka plan kod kaynak dosyasını açın `MainPage.xaml.cs` (`MainPage.xaml` altında gruplandırılmış olarak bulabilirsiniz). İçindeki tüm kodu aşağıdakiyle değiştirin.

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/text-to-speech/csharp-uwp/helloworld/MainPage.xaml.cs#code)]

1. Bu dosyadaki `Speak_ButtonClicked` işleyicisinde `YourSubscriptionKey` dizesini abonelik anahtarınız ile değiştirin.

1. `Speak_ButtonClicked` işleyicisinde `YourServiceRegion` dizesini aboneliğinizle ilişkili [bölge](regions.md) ile (örneğin ücretsiz deneme aboneliğinde `westus`) değiştirin.

1. Değişiklikleri projeye kaydedin.

## <a name="build-and-run-the-app"></a>Uygulamayı derleme ve çalıştırma

1. Uygulamayı derleyin. Menü çubuğundan **Derle** > **Çözümü Derle**'yi seçin. Kodun artık hatasız derlenmesi gerekir.

    ![Visual Studio uygulamasının, Çözümü Derle seçeneği vurgulanmış olarak ekran görüntüsü](media/sdk/qs-csharp-uwp-08-build.png "Başarılı derleme")

1. Uygulamayı başlatın. Menü çubuğundan **Hata Ayıklama** > **Hata Ayıklamayı Başlat**'ı seçin veya **F5** tuşuna basın.

    ![Visual Studio uygulamasının, Hata Ayıklamayı Başlat seçeneği vurgulanmış olarak ekran görüntüsü](media/sdk/qs-csharp-uwp-09-start-debugging.png "Uygulamayı hata ayıklamada başlatma")

1. Aşağıdaki metin kutusuna metin girin ve tıklayın **konuşma**. Metni konuşma hizmetlere iletilen ve konuşma tanıma, hoparlöründen için oluşturulan.

    ![Konuşma sentezi kullanıcı arabiriminin ekran görüntüsü](media/sdk/qs-tts-csharp-uwp-ui-result.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Keşfedin C# github'da örnekleri](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Ayrıca bkz.

- [Ses tiplerini özelleştirme](how-to-customize-voice-font.md)
- [Kayıt ses örnekleri](record-custom-voice-samples.md)
