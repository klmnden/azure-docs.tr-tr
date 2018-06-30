---
title: 'Hızlı Başlangıç: Bilişsel hizmetler konuşma C# SDK for Windows kullanarak konuşma tanı | Microsoft Docs'
description: Konuşma hizmeti için C# SDK kullanarak Konuşma tanıması öğrenin.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: wolfma61
manager: onano
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: wolfma
ms.openlocfilehash: 14e5110385667d0f2135251eef53ff20ada08444
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37111261"
---
# <a name="quickstart-recognize-speech-using-the-cognitive-services-speech-c-sdk"></a>Hızlı Başlangıç: Bilişsel hizmetler konuşma C# SDK kullanarak konuşma tanı

Bu makalede, metin konuşma transcribe için Bilişsel hizmetler konuşma SDK'sını kullanarak Windows'da bir C# konsol uygulaması oluşturun öğrenin.

## <a name="prerequisites"></a>Önkoşullar

* Konuşma hizmeti için bir abonelik anahtarı. Bkz: [konuşma hizmeti ücretsiz deneyebilir](get-started.md).
* Visual Studio 2017, Community Edition ya da daha yüksek.
* **.NET masaüstü geliştirme** Visual Studio iş yükü. İçinde etkinleştirebilirsiniz **Araçları** \> **alma araçları ve özelliklerinin**. 

## <a name="create-a-visual-studio-project"></a>Visual Studio projesi oluşturma

1. Visual Studio 2017 ' yeni bir Visual C# konsol uygulaması oluşturun. İçinde **yeni proje** iletişim kutusunda, sol bölmeden genişletin **yüklü** ve ardından **konsol uygulaması (.NET Framework)**. Proje adı girin *CsharpHelloSpeech*.

    ![Visual C# konsol uygulaması (.NET Framework) oluşturma](media/sdk/speechsdk-05-vs-cs-new-console-app.png "Visual C# konsol uygulaması oluşturma")

2. Yükleme ve başvuru [konuşma SDK NuGet paketini](https://aka.ms/csspeech/nuget). Çözüm Gezgini'nde çözüme sağ tıklayın ve seçin **çözüm için NuGet paketlerini Yönet**.

    ![Sağ tıklatın, çözüm için NuGet paketlerini Yönet](media/sdk/speechsdk-06-vs-cs-manage-nuget-packages.png "çözüm NuGet paketlerini yönetme")

3. Sağ üst köşedeki içinde **paket kaynağı** alan, select **Nuget.org**. Arayın ve yükleyin `Microsoft.CognitiveServices.Speech` paketini ve içine yükleyin **CsharpHelloSpeech** projesi.

    ![Microsoft.CognitiveServices.Speech NuGet paket yüklemesi](media/sdk/speechsdk-08-vs-cs-nuget-install.png "yükleme Nuget paketi")

4. Açılır lisans ekranında lisans kabul edin:

    ![Lisans kabul](media/sdk/speechsdk-09-vs-cs-nuget-license.png "lisans kabul et")

## <a name="create-a-platform-configuration-matching-your-pc-architecture"></a>PC Mimarinizi eşleşen bir platform yapılandırması oluştur

Bu bölümde, yeni bir platformu işlemci mimariniz eşleşen yapılandırmaya ekleyin.

1. Yapılandırma Yöneticisi'ni başlatın. Seçin **yapı** > **Configuration Manager**.

    ![Yapılandırma Yöneticisi'ni başlatma](media/sdk/speechsdk-12-vs-cs-cfg-manager-click.png "Yapılandırma Yöneticisi'ni başlatın")

2. İçinde **Configuration Manager** iletişim kutusunda, yeni bir platformu ekleyin. Gelen **etkin çözüm platformu** aşağı açılan listesinden, **yeni**.

    ![Yapılandırma Yöneticisi penceresi altında yeni bir platform eklemek](media/sdk/speechsdk-14-vs-cs-cfg-manager-new.png "Yapılandırma Yöneticisi penceresi altında yeni bir platform ekleme")

3. 64 bit Windows'ta çalıştırıyorsanız adlı yeni bir platform yapılandırması oluştur `x64`. 32 bit Windows çalıştırıyorsanız adlı yeni bir platform yapılandırması oluştur `x86`. Bu makalede, oluşturduğunuz bir `x64` platform yapılandırması. 

    ![64 bit Windows'ta "x64" adlı yeni bir platform eklemek](media/sdk/speechsdk-15-vs-cs-cfg-manager-add-x64.png "Ekle x64 platform")

## <a name="add-the-sample-code"></a>Örnek kod ekleme

1. İçinde `Program.cs` dosya Visual Studio projenizde, gövdesini değiştirin `Program` aşağıdaki sınıfı. Abonelik anahtarı kendi metninizle değiştirebilir ve değiştirme emin olun [bölge](regions.md) abonelikle ilişkili bir (örneğin, `westus` ücretsiz deneme aboneliği için).

    [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/Windows/quickstart-csharp/Program.cs#code)]

2. Kod yapıştırma sonra `Main()` yöntemi aşağıdaki ekran görüntüsünde gösterildiği gibi benzer olmalıdır:

    ![Kod yapıştırma sonra ana yöntemi](media/sdk/speechsdk-17-vs-cs-paste-code.png "son Main yöntemi")

3. Visual Studio'nun IntelliSense çözümlenemedi konuşma SDK'ın sınıfları başvuruları vurgular. Bu hatayı düzeltmek için aşağıdaki ekleyin `using` kodu başına deyimi (ya da el ile veya Visual Studio kullanarak [hızlı Eylemler](https://docs.microsoft.com/visualstudio/ide/quick-actions)).

    [!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/Windows/quickstart-csharp/Program.cs#usingstatement)]

    ![Eksik eklemek için hızlı eylemini kullanın deyimiyle](media/sdk/speechsdk-18-vs-cs-add-using.png "sorunları gidermek IntelliSense")

4. Vurgulamayı IntelliSense çözülmüş olduğundan emin olun ve projeye değişiklikleri kaydedin.

## <a name="build-and-run-the-sample"></a>Örneği derleme ve çalıştırma

1. Uygulamayı oluşturun. Menü çubuğundan seçin **yapı** > **yapı çözümü**. Kodu hatasız şimdi derlemek:

    ![Başarılı yapı](media/sdk/speechsdk-20-vs-cs-build.png "başarılı derleme")

2. Uygulamayı başlatın. Menü çubuğundan seçin **hata ayıklama** > **hata ayıklamayı Başlat**, veya basın **F5**. 

    ![Hata ayıklama içine uygulamayı başlatın](media/sdk/speechsdk-21-vs-cs-f5.png "hata ayıklama içine uygulamayı başlatın")

3. Şeyin (İngilizce) deyin isteyen, bir konsol penceresi açılır.
Tanıma sonucunu ekranında görüntülenir.

    ![Konsol çıkış sonra başarılı tanıma](media/sdk/speechsdk-22-cs-vs-console-output.png "başarılı tanıma sonra konsol çıkışı")

## <a name="download-code"></a>Kodu indirme

En son örnekleri için bkz [Bilişsel hizmetler konuşma SDK örnek GitHub deposunu](https://aka.ms/csspeech/samples).

## <a name="next-steps"></a>Sonraki adımlar

- [Konuşma Çevir](how-to-translate-speech.md)
- [Akustik modelleri özelleştirme](how-to-customize-acoustic-models.md)
- [Dil modelleri özelleştirme](how-to-customize-language-model.md)
