---
title: 'Hızlı Başlangıç: .NET Framework (Windows) - konuşma Hizmetleri konuşma tanıma'
titleSuffix: Azure Cognitive Services
description: Windows için .NET Framework ve Konuşma Tanıma SDK'sını kullanarak bir konuşmayı metne dönüştürme konsol uygulaması oluşturmak için bu kılavuzu kullanın. İşiniz bittiğinde konuşmayı metne gerçek zamanlı dönüştürmek için bilgisayarınızın mikrofonunu kullanabilirsiniz.
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/05/2019
ms.author: wolfma
ms.openlocfilehash: d8738357a3bad6626ef6d79248aef1c4d2cb1ead
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67603083"
---
# <a name="quickstart-recognize-speech-with-the-speech-sdk-for-net-framework-windows"></a>Hızlı Başlangıç: .NET Framework (Windows) için Speech SDK'sı ile Konuşma tanıma

Hızlı Başlangıçlar ücret karşılığında ayrıca [metin okuma](quickstart-text-to-speech-dotnet-windows.md) ve [konuşma çevirisi](quickstart-translate-speech-dotnetframework-windows.md).

İsterseniz, farklı programlama dili ve/veya ortamı seçin:<br/>
[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Windows için .NET Framework ve Konuşma Tanıma SDK'sını kullanarak bir konuşmayı metne dönüştürme konsol uygulaması oluşturmak için bu kılavuzu kullanın. İşiniz bittiğinde konuşmayı metne gerçek zamanlı dönüştürmek için bilgisayarınızın mikrofonunu kullanabilirsiniz.

Bir hızlı örnek için: (olmadan kendiniz aşağıda gösterildiği gibi Visual Studio projesi oluşturma)

En son bilgileri edinin [Bilişsel hizmetler konuşma SDK örnekleri](https://github.com/Azure-Samples/cognitive-services-speech-sdk) github'dan.

## <a name="prerequisites"></a>Önkoşullar

Bu projeyi tamamlamak için şunlar gerekir:

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
* Konuşma hizmeti için bir abonelik anahtarı. [Ücretsiz edinin](get-started.md).
* Bilgisayarınızın mikrofonuna erişim

## <a name="create-a-visual-studio-project"></a>Visual Studio projesi oluşturma

[!INCLUDE [Create project](../../../includes/cognitive-services-speech-service-create-speech-project-vs-csharp.md)]

## <a name="add-sample-code"></a>Örnek kod ekleme

1. `Program.cs` dosyasını açın otomatik olarak oluşturulan kodu bu örnekle değiştirin:

    [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-dotnet-windows/helloworld/Program.cs#code)]

1. Bulun ve dize değiştirin `YourSubscriptionKey` konuşma Hizmetleri abonelik anahtarınız ile.

1. `YourServiceRegion` dizesini bulun ve aboneliğinizle ilişkili [bölge](regions.md) ile değiştirin. Örneğin, ücretsiz denemeyi kullanıyorsanız bölge `westus` olur.

1. Projedeki değişiklikleri kaydedin.

## <a name="build-and-run-the-app"></a>Uygulamayı derleme ve çalıştırma

1. Menü çubuğundan **Derle** > **Çözümü Derle**'yi seçin. Kodun artık hatasız derlenmesi gerekir.

    ![Visual Studio uygulamasının, Çözümü Derle seçeneği vurgulanmış olarak ekran görüntüsü](media/sdk/qs-csharp-dotnet-windows-08-build.png "Başarılı derleme")

1. Uygulamayı başlatmak için menü çubuğundan **Hata Ayıklama** > **Hata Ayıklamayı Başlat**'ı seçin veya **F5** tuşuna basın.

    ![Visual Studio uygulamasının, Hata Ayıklamayı Başlat seçeneği vurgulanmış olarak ekran görüntüsü](media/sdk/qs-csharp-dotnet-windows-09-start-debugging.png "Uygulamayı hata ayıklamada başlatma")

1. Konuşmanızı isteyen bir konsol penceresi açılır. Şimdi, İngilizce bir şeyler söyleyin. Konuşma konuşma hizmetlere iletilen ve gerçek zamanlı olarak metne transcribed. Sonuç konsola yazdırılır.

    ![Başarılı tanımadan sonra konsol çıktısının ekran görüntüsü](media/sdk/qs-csharp-dotnet-windows-10-console-output.png "Başarılı tanımadan sonra konsol çıktısı")

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Keşfedin C# github'da örnekleri](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Ayrıca bkz.

- [Akustik modelleri özelleştirme](how-to-customize-acoustic-models.md)
- [Dil modellerini özelleştirme](how-to-customize-language-model.md)
