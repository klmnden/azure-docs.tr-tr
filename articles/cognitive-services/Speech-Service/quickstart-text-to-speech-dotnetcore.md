---
title: 'Hızlı Başlangıç: Konuşma sentezlemek C# (.NET Core) - konuşma Hizmetleri'
titleSuffix: Azure Cognitive Services
description: Konuşma dilinde sentezlemek öğrenin C# Speech SDK'sı kullanarak Windows üzerinde .NET Core altında
services: cognitive-services
author: yinhew
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 6/24/2019
ms.author: yinhew
ms.openlocfilehash: 7b4a018e38ca625e38dc1658a95d3ce0e677f711
ms.sourcegitcommit: c63e5031aed4992d5adf45639addcef07c166224
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67467428"
---
# <a name="quickstart-synthesize-speech-with-the-speech-sdk-for-net-core"></a>Hızlı Başlangıç: Konuşma Speech SDK'sı ile .NET Core için sentezlemek

Hızlı Başlangıçlar ücret karşılığında ayrıca [konuşma tanıma](quickstart-csharp-dotnetcore-windows.md) ve [konuşma çevirisi](quickstart-translate-speech-dotnetcore-windows.md).

Bu makalede Bilişsel Hizmetler[Konuşma SDK'sı](speech-sdk.md)'nı kullanarak Windows'da .NET Core için bir C# konsol uygulaması oluşturacaksınız. Bilgisayarınızın Konuşmacı gerçek zamanlı olarak metinden konuşmaya sentezlemek. Uygulama [Konuşma SDK'sı NuGet paketi](https://aka.ms/csspeech/nuget) ve Microsoft Visual Studio 2017 (herhangi bir sürüm) ile geliştirilmiştir.

> [!NOTE]
> .NET Core, [.NET Standard](https://docs.microsoft.com/dotnet/standard/net-standard) teknik özelliklerini gerçekleştiren açık kaynaklı, platformlar arası bir .NET platformudur.

Bu hızlı başlangıcı tamamlamak için bir konuşma Hizmetleri abonelik anahtarı ihtiyacınız vardır. Anahtarı ücretsiz alabilirsiniz. Bkz: [konuşma Hizmetleri ücretsiz olarak deneyin](get-started.md) Ayrıntılar için.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıç şunları gerektirir:

* [.NET Core SDK](https://dotnet.microsoft.com/download)
* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
* Konuşma hizmeti için bir Azure aboneliği anahtarı. [Ücretsiz edinin](get-started.md).

## <a name="create-a-visual-studio-project"></a>Visual Studio projesi oluşturma

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-dotnetcore-create-proj.md)]

## <a name="add-sample-code"></a>Örnek kodu ekleme

1. `Program.cs` dosyasını açın ve tüm kodu aşağıdakiyle değiştirin.

    [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/text-to-speech/csharp-dotnetcore/helloworld/Program.cs#code)]

1. Aynı dosyada `YourSubscriptionKey` dizesini abonelik anahtarınız ile değiştirin.

1. Ayrıca `YourServiceRegion` dizesini de aboneliğinizle ilişkili [bölge](regions.md) ile (örneğin ücretsiz deneme aboneliğinde `westus`) değiştirin.

1. Proje üzerindeki değişiklikleri kaydedin.

## <a name="build-and-run-the-app"></a>Uygulamayı derleme ve çalıştırma

1. Uygulamayı derleyin. Menü çubuğundan **Derle** > **Çözümü Derle**'yi seçin. Kodun hatasız derlenmesi gerekir.

    ![Visual Studio uygulamasının, Çözümü Derle seçeneği vurgulanmış olarak ekran görüntüsü](media/sdk/qs-csharp-dotnetcore-windows-05-build.png "Başarılı derleme")

1. Uygulamayı başlatın. Menü çubuğundan **Hata Ayıklama** > **Hata Ayıklamayı Başlat**'ı seçin veya **F5** tuşuna basın.

    ![Visual Studio uygulamasının, Hata Ayıklamayı Başlat seçeneği vurgulanmış olarak ekran görüntüsü](media/sdk/qs-csharp-dotnetcore-windows-06-start-debugging.png "Uygulamayı hata ayıklamada başlatma")

1. Bir metin yazın isteyip istemediğinizi soran bir konsol penceresi görünür. Birkaç sözcük veya bir cümle yazın. Yazdığınız metni konuşma hizmetlere iletilen ve konuşma tanıma, hoparlöründen için oluşturulan.

    ![Başarılı sentezi sonra konsol çıktısı ekran görüntüsü](media/sdk/qs-tts-csharp-dotnet-windows-console-output.png "başarılı birleştirmesinden sonra konsol çıktısı")

## <a name="next-steps"></a>Sonraki adımlar

Ses dosyası, konuşma sentezlemek nasıl gibi ek örnekler Github'da kullanılabilir.

> [!div class="nextstepaction"]
> [Keşfedin C# github'da örnekleri](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Ayrıca bkz.

- [Ses tiplerini özelleştirme](how-to-customize-voice-font.md)
- [Kayıt ses örnekleri](record-custom-voice-samples.md)
