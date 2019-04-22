---
title: 'Hızlı Başlangıç: Okuma, .NET Framework (Windows) - konuşma Hizmetleri sentezlemek'
titleSuffix: Azure Cognitive Services
description: Windows ve Speech SDK'sı için .NET framework kullanarak bir metin okuma konsol uygulaması oluşturmak için bu kılavuzu kullanın. Tamamlandığında, sentezlemek konuşma metin ve konuşma tanıma, konuşmacı gerçek zamanlı üzerinde dinleyin.
services: cognitive-services
author: yinhew
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 4/03/2019
ms.author: yinhew
ms.openlocfilehash: a013189e45b1c1c8eeb88d62a718d495c0c415a2
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59012251"
---
# <a name="quickstart-synthesize-speech-with-the-speech-sdk-for-net-framework-windows"></a>Hızlı Başlangıç: Konuşma Speech SDK'sı ile sentezlemek için .NET Framework (Windows)

Windows ve Speech SDK'sı için .NET framework kullanarak bir metin okuma konsol uygulaması oluşturmak için bu kılavuzu kullanın. Tamamlandığında, sentezlemek konuşma metin ve konuşma tanıma, konuşmacı gerçek zamanlı üzerinde dinleyin.

Bir hızlı örnek için: (olmadan kendiniz aşağıda gösterildiği gibi Visual Studio projesi oluşturma)

En son bilgileri edinin [Bilişsel hizmetler konuşma SDK örnekleri](https://github.com/Azure-Samples/cognitive-services-speech-sdk) github'dan.

## <a name="prerequisites"></a>Önkoşullar

Bu projeyi tamamlamak için şunlar gerekir:

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
* Konuşma hizmeti için bir abonelik anahtarı. [Ücretsiz edinin](get-started.md).
* Konuşmacı (veya kulaklık) kullanılabilir.

## <a name="create-a-visual-studio-project"></a>Visual Studio projesi oluşturma

[!INCLUDE [Create project](../../../includes/cognitive-services-speech-service-create-speech-project-vs-csharp.md)]

## <a name="add-sample-code"></a>Örnek kod ekleme

1. `Program.cs` dosyasını açın otomatik olarak oluşturulan kodu bu örnekle değiştirin:

    [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/text-to-speech/csharp-dotnet-windows/helloworld/Program.cs#code)]

1. Bulun ve dize değiştirin `YourSubscriptionKey` konuşma Hizmetleri abonelik anahtarınız ile.

1. `YourServiceRegion` dizesini bulun ve aboneliğinizle ilişkili [bölge](regions.md) ile değiştirin. Örneğin, ücretsiz denemeyi kullanıyorsanız bölge `westus` olur.

1. Projedeki değişiklikleri kaydedin.

## <a name="build-and-run-the-app"></a>Uygulamayı derleme ve çalıştırma

1. Menü çubuğundan **Derle** > **Çözümü Derle**'yi seçin. Kodun artık hatasız derlenmesi gerekir.

    ![Visual Studio uygulamasının, Çözümü Derle seçeneği vurgulanmış olarak ekran görüntüsü](media/sdk/qs-csharp-dotnet-windows-08-build.png "Başarılı derleme")

1. Uygulamayı başlatmak için menü çubuğundan **Hata Ayıklama** > **Hata Ayıklamayı Başlat**'ı seçin veya **F5** tuşuna basın.

    ![Visual Studio uygulamasının, Hata Ayıklamayı Başlat seçeneği vurgulanmış olarak ekran görüntüsü](media/sdk/qs-csharp-dotnet-windows-09-start-debugging.png "Uygulamayı hata ayıklamada başlatma")

1. Bir metin yazın isteyip istemediğinizi soran bir konsol penceresi görünür. Birkaç sözcük veya bir cümle yazın. Yazdığınız metni konuşma hizmetlere iletilen ve konuşma tanıma, hoparlöründen için oluşturulan.

    ![Başarılı tanımadan sonra konsol çıktısının ekran görüntüsü](media/sdk/qs-tts-csharp-dotnet-windows-console-output.png "Başarılı tanımadan sonra konsol çıktısı")

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Keşfedin C# github'da örnekleri](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Ayrıca bkz.

- [Ses tiplerini özelleştirme](how-to-customize-voice-font.md)
- [Kayıt ses örnekleri](record-custom-voice-samples.md)
