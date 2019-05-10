---
title: 'Hızlı Başlangıç: Konuşma tanıma, çeviri C# (.NET Core Windows)'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, komut satırına metin çıktısı kullanıcı konuşma yakalamak ve başka bir dile çevirmek için basit bir .NET Core uygulaması oluşturacaksınız. Bu kılavuz, Windows kullanıcıları için tasarlanmıştır.
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 03/13/2019
ms.author: erhopf
ms.openlocfilehash: fa516c5050bfd53add761bea56c74a58057263c7
ms.sourcegitcommit: e6d53649bfb37d01335b6bcfb9de88ac50af23bd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65465696"
---
# <a name="quickstart-translate-speech-with-the-speech-sdk-for-net-core"></a>Hızlı Başlangıç: Konuşma Speech SDK'sı ile .NET Core için çevir

Hızlı Başlangıçlar ücret karşılığında ayrıca [konuşma metin](quickstart-csharp-dotnet-windows.md) ve [metin okuma](quickstart-text-to-speech-dotnet-windows.md).

Bu hızlı başlangıçta, bilgisayarınızın mikrofondan gelen kullanıcı konuşma yakalar, konuşma çevirir ve gerçek zamanlı olarak komut satırında çevrilmiş metne dönüştürür basit bir .NET Core uygulaması oluşturacaksınız. Bu uygulama, 64 bit Windows üzerinde çalışacak şekilde tasarlanmıştır ve ile oluşturulmuş [konuşma SDK'sı NuGet paketi](https://aka.ms/csspeech/nuget) ve Microsoft Visual Studio 2017.

Konuşma çevirisi için kullanılabilen dilleri tam bir listesi için bkz. [dil desteği](language-support.md).

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıç şunları gerektirir:

* [.NET Core SDK](https://dotnet.microsoft.com/download)
* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
* Konuşma hizmeti için bir Azure aboneliği anahtarı. [Ücretsiz edinin](get-started.md).

## <a name="create-a-visual-studio-project"></a>Visual Studio projesi oluşturma

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-dotnetcore-create-proj.md)]

## <a name="add-sample-code"></a>Örnek kod ekleyin

1. `Program.cs` dosyasını açın ve tüm kodu aşağıdakiyle değiştirin.

    [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/speech-translation/csharp-dotnetcore/helloworld/Program.cs#code)]

1. Aynı dosyada `YourSubscriptionKey` dizesini abonelik anahtarınız ile değiştirin.

1. Ayrıca `YourServiceRegion` dizesini de aboneliğinizle ilişkili [bölge](regions.md) ile (örneğin ücretsiz deneme aboneliğinde `westus`) değiştirin.

1. Proje üzerindeki değişiklikleri kaydedin.

## <a name="build-and-run-the-app"></a>Uygulamayı derleme ve çalıştırma

1. Uygulamayı derleyin. Menü çubuğundan **Derle** > **Çözümü Derle**'yi seçin. Kodun hatasız derlenmesi gerekir.

    ![Visual Studio uygulamasının, Çözümü Derle seçeneği vurgulanmış olarak ekran görüntüsü](media/sdk/qs-csharp-dotnetcore-windows-05-build.png "Başarılı derleme")

1. Uygulamayı başlatın. Menü çubuğundan **Hata Ayıklama** > **Hata Ayıklamayı Başlat**'ı seçin veya **F5** tuşuna basın.

    ![Visual Studio uygulamasının, Hata Ayıklamayı Başlat seçeneği vurgulanmış olarak ekran görüntüsü](media/sdk/qs-csharp-dotnetcore-windows-06-start-debugging.png "Uygulamayı hata ayıklamada başlatma")

1. Bir şey söylemenizi isteyen bir konsol penceresi görünür. İngilizce bir deyim ya da cümle söyleyin. Konuşma konuşma tanıma Hizmeti'ne aktarılan çevrilmiş ve transcribed aynı pencerede görünen metin.

    ![Başarılı çeviri sonra konsol çıktısı ekran görüntüsü](media/sdk/qs-translate-csharp-dotnetcore-windows-output.png "başarılı çeviri sonra konsol çıktısı")


## <a name="next-steps"></a>Sonraki adımlar

Konuşma bir ses dosyasını okuma ve Sentezlenen konuşma olarak çevrilmiş metin çıktısı gibi ek örnekler Github'da kullanılabilir.

> [!div class="nextstepaction"]
> [Keşfedin C# github'da örnekleri](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Ayrıca bkz.

- [Akustik modelleri özelleştirme](how-to-customize-acoustic-models.md)
- [Dil modellerini özelleştirme](how-to-customize-language-model.md)
