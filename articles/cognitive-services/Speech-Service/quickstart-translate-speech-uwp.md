---
title: 'Hızlı Başlangıç: Konuşma tanıma, çeviri C# (UWP) - konuşma Hizmetleri'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, komut satırına metin çıktısı kullanıcı konuşma yakalamak ve başka bir dile çevirmek için basit bir evrensel Windows Platformu (UWP) uygulaması oluşturacaksınız. Bu kılavuz, Windows kullanıcıları için tasarlanmıştır.
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 03/13/2019
ms.author: erhopf
ms.openlocfilehash: 3b8f0c93e596185f4446d4058641b2efc45ce4a5
ms.sourcegitcommit: c63e5031aed4992d5adf45639addcef07c166224
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67467080"
---
# <a name="quickstart-translate-speech-with-the-speech-sdk-for-c-uwp"></a>Hızlı Başlangıç: Konuşma Speech SDK'sı ile çevir C# (UWP)

Hızlı Başlangıçlar ücret karşılığında ayrıca [konuşma metin](quickstart-csharp-uwp.md), [metin okuma](quickstart-text-to-speech-csharp-uwp.md) ve [ses öncelikli sanal asistan](quickstart-virtual-assistant-csharp-uwp.md).

Bu hızlı başlangıçta, bilgisayarınızın mikrofondan gelen kullanıcı konuşma yakalar, konuşma çevirir ve gerçek zamanlı olarak komut satırında çevrilmiş metne dönüştürür basit bir evrensel Windows Platformu (UWP) uygulaması oluşturacaksınız. Bu uygulama, 64 bit Windows üzerinde çalışacak şekilde tasarlanmıştır ve ile oluşturulmuş [konuşma SDK'sı NuGet paketi](https://aka.ms/csspeech/nuget) ve Microsoft Visual Studio 2017.

Konuşma çevirisi için kullanılabilen dilleri tam bir listesi için bkz. [dil desteği](language-support.md).

> [!NOTE]
> UWP, Windows 10 bilgisayarları, Xbox, Surface Hub ve diğer cihazları dahil olmak üzere, destekleyen herhangi bir cihaz üzerinde çalışan uygulamalar geliştirmenize olanak tanır.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıç şunları gerektirir:

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
* Konuşma hizmeti için bir Azure aboneliği anahtarı. [Ücretsiz edinin](get-started.md).

## <a name="create-a-visual-studio-project"></a>Visual Studio projesi oluşturma

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-uwp-create-proj.md)]

## <a name="add-sample-code"></a>Örnek kodu ekleme

1. Uygulamanın kullanıcı arabirimini XAML kullanılarak tanımlanır. `MainPage.xaml` dosyasını Çözüm Gezgini'nde açın. Tasarımcının XAML görünümünde arasında aşağıdaki XAML kod parçacığı Ekle `<Grid>` ve `</Grid>`.

    [!code-xml[UI elements](~/samples-cognitive-services-speech-sdk/quickstart/speech-translation/csharp-uwp/helloworld/MainPage.xaml#StackPanel)]

1. Arka plan kod kaynak dosyasını açın `MainPage.xaml.cs` (`MainPage.xaml` altında gruplandırılmış olarak bulabilirsiniz). İçindeki tüm kodu aşağıdakiyle değiştirin.

    [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/speech-translation/csharp-uwp/helloworld/MainPage.xaml.cs#code)]

1. Bu dosyadaki `SpeechTranslationFromMicrophone_ButtonClicked` işleyicisinde `YourSubscriptionKey` dizesini abonelik anahtarınız ile değiştirin.

1. `SpeechTranslationFromMicrophone_ButtonClicked` işleyicisinde `YourServiceRegion` dizesini aboneliğinizle ilişkili [bölge](regions.md) ile (örneğin ücretsiz deneme aboneliğinde `westus`) değiştirin.

1. Değişiklikleri projeye kaydedin.

## <a name="build-and-run-the-app"></a>Uygulamayı derleme ve çalıştırma

1. Uygulamayı derleyin. Menü çubuğundan **Derle** > **Çözümü Derle**'yi seçin. Kodun artık hatasız derlenmesi gerekir.

    ![Visual Studio uygulamasının, Çözümü Derle seçeneği vurgulanmış olarak ekran görüntüsü](media/sdk/qs-csharp-uwp-08-build.png "Başarılı derleme")

1. Uygulamayı başlatın. Menü çubuğundan **Hata Ayıklama** > **Hata Ayıklamayı Başlat**'ı seçin veya **F5** tuşuna basın.

    ![Visual Studio uygulamasının, Hata Ayıklamayı Başlat seçeneği vurgulanmış olarak ekran görüntüsü](media/sdk/qs-csharp-uwp-09-start-debugging.png "Uygulamayı hata ayıklamada başlatma")

1. Bir pencere açılır. **Mikrofonu Etkinleştir**’i seçin ve çıkan izin isteğini kabul edin.

    ![İzin isteğinin ekran görüntüsü](media/sdk/qs-csharp-uwp-10-access-prompt.png "Uygulamayı hata ayıklamada başlat")

1. **Mikrofon girdisi ile konuşma tanıma**’yı seçin ve cihazınızın mikrofonuna İngilizce bir deyim ya da cümle söyleyin. Söyledikleriniz Konuşma hizmetine aktarılır ve metne dönüştürülür; metin pencerede görünür.

    ![Konuşma tanıma kullanıcı arabiriminin ekran görüntüsü](media/sdk/qs-translate-csharp-uwp-ui-result.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Keşfedin C# github'da örnekleri](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Ayrıca bkz.

- [Akustik modelleri özelleştirme](how-to-customize-acoustic-models.md)
- [Dil modellerini özelleştirme](how-to-customize-language-model.md)
