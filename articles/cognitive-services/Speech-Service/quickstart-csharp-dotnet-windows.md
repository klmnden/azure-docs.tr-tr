---
title: 'Hızlı Başlangıç: .NET Framework (Windows) - konuşma Hizmetleri konuşma tanıma'
titleSuffix: Azure Cognitive Services
description: Windows için .NET Framework ve Konuşma Tanıma SDK'sını kullanarak bir konuşmayı metne dönüştürme konsol uygulaması oluşturmak için bu kılavuzu kullanın. İşiniz bittiğinde konuşmayı metne gerçek zamanlı dönüştürmek için bilgisayarınızın mikrofonunu kullanabilirsiniz.
services: cognitive-services
author: wolfma61
manager: cgronlun
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 12/13/2018
ms.author: wolfma
ms.openlocfilehash: 9d01a6658a62abdc84a18c3e5dbec7cb88aabc98
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55208169"
---
# <a name="quickstart-recognize-speech-with-the-speech-sdk-for-net-framework-windows"></a>Hızlı Başlangıç: .NET Framework (Windows) için Speech SDK'sı ile Konuşma tanıma

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Windows için .NET Framework ve Konuşma Tanıma SDK'sını kullanarak bir konuşmayı metne dönüştürme konsol uygulaması oluşturmak için bu kılavuzu kullanın. İşiniz bittiğinde konuşmayı metne gerçek zamanlı dönüştürmek için bilgisayarınızın mikrofonunu kullanabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Bu projeyi tamamlamak için şunlar gerekir:

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
* Konuşma hizmeti için bir abonelik anahtarı. [Ücretsiz edinin](get-started.md).
* Bilgisayarınızın mikrofonuna erişim

## <a name="create-a-visual-studio-project"></a>Visual Studio projesi oluşturma

[!INCLUDE [Create project ](../../../includes/cognitive-services-speech-service-create-speech-project-vs-csharp.md)]

## <a name="add-sample-code"></a>Örnek kod ekleme

1. `Program.cs` dosyasını açın otomatik olarak oluşturulan kodu bu örnekle değiştirin:

    [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-dotnet-windows/helloworld/Program.cs#code)]

1. `YourSubscriptionKey` dizesini bulun ve Konuşma Tanıma Hizmeti abonelik anahtarınızla değiştirin.

1. `YourServiceRegion` dizesini bulun ve aboneliğinizle ilişkili [bölge](regions.md) ile değiştirin. Örneğin, ücretsiz denemeyi kullanıyorsanız bölge `westus` olur.

1. Projedeki değişiklikleri kaydedin.

## <a name="build-and-run-the-app"></a>Uygulamayı derleme ve çalıştırma

1. Menü çubuğundan **Derle** > **Çözümü Derle**'yi seçin. Kodun artık hatasız derlenmesi gerekir.

    ![Visual Studio uygulamasının, Çözümü Derle seçeneği vurgulanmış olarak ekran görüntüsü](media/sdk/qs-csharp-dotnet-windows-08-build.png "Başarılı derleme")

1. Uygulamayı başlatmak için menü çubuğundan **Hata Ayıklama** > **Hata Ayıklamayı Başlat**'ı seçin veya **F5** tuşuna basın.

    ![Visual Studio uygulamasının, Hata Ayıklamayı Başlat seçeneği vurgulanmış olarak ekran görüntüsü](media/sdk/qs-csharp-dotnet-windows-09-start-debugging.png "Uygulamayı hata ayıklamada başlatma")

1. Konuşmanızı isteyen bir konsol penceresi açılır. Şimdi, İngilizce bir şeyler söyleyin. Konuşmanız Konuşma Tanıma Hizmeti'ne iletilir ve gerçek zamanlı olarak metne dönüştürülür. Sonuç konsola yazdırılır.

    ![Başarılı tanımadan sonra konsol çıktısının ekran görüntüsü](media/sdk/qs-csharp-dotnet-windows-10-console-output.png "Başarılı tanımadan sonra konsol çıktısı")

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Keşfedin C# github'da örnekleri](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Ayrıca bkz.

- [Akustik modelleri özelleştirme](how-to-customize-acoustic-models.md)
- [Dil modellerini özelleştirme](how-to-customize-language-model.md)
