---
title: "Hızlı Başlangıç: Bilişsel Hizmetler Konuşma SDK'sını kullanarak Windows'da .NET Core altında C# ile konuşma tanıma"
titleSuffix: Microsoft Cognitive Services
description: Bilişsel Hizmetler Konuşma SDK'sını kullanarak Windows'da .NET Core altında C# ile konuşma tanımayı öğrenin
services: cognitive-services
author: wolfma61
ms.service: cognitive-services
ms.component: speech-service
ms.topic: quickstart
ms.date: 09/24/2018
ms.author: wolfma
ms.openlocfilehash: 2cb89606986645d567136655d5ab3f07223ba70d
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/28/2018
ms.locfileid: "47434781"
---
# <a name="quickstart-recognize-speech-in-c-under-net-core-on-windows-by-using-the-speech-sdk"></a>Hızlı Başlangıç: Konuşma SDK'sını kullanarak Windows'da .NET Core altında C# ile konuşma tanıma

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Bu makalede Bilişsel Hizmetler[Konuşma SDK'sı](speech-sdk.md)'nı kullanarak Windows'da .NET Core için bir C# konsol uygulaması oluşturacaksınız. Bilgisayarınızın mikrofonundan gerçek zamanda konuşmayı metne dönüştüreceksiniz. Uygulama [Konuşma SDK'sı NuGet paketi](https://aka.ms/csspeech/nuget) ve Microsoft Visual Studio 2017 (herhangi bir sürüm) ile geliştirilmiştir.

> [!NOTE]
> .NET Core, [.NET Standard](https://docs.microsoft.com/dotnet/standard/net-standard) teknik özelliklerini gerçekleştiren açık kaynaklı, platformlar arası bir .NET platformudur.

Bu Hızlı Başlangıç'ı tamamlamak için bir Konuşma hizmeti abonelik anahtarınız olması gerekir. Anahtarı ücretsiz edinebilirsiniz. Ayrıntılar için bkz: [Konuşma hizmetini ücretsiz olarak deneme](get-started.md).


## <a name="create-a-visual-studio-project"></a>Visual Studio projesi oluşturma

1. Visual Studio 2017'yi başlatın.

1. **.NET platformlar arası geliştirme** iş yükünün bulunduğundan emin olun. Visual Studio yükleyicisini açmak için Visual Studio menü çubuğundan **Araçlar** > **Araçları ve Özellikleri Al**'ı seçin. Bu iş yükü zaten etkinse iletişim kutusunu kapatın.

    ![Visual Studio yükleyicisinin İş yükleri sekmesi vurgulanmış olarak ekran görüntüsü](media/sdk/vs-enable-net-core-workload.png)

    Etkin değilse **.NET Core platformlar arası geliştirme**'nin yanındaki kutuyu işaretleyin ve iletişim kutusunun sağ alt köşesindeki **Değiştir**'i seçin. Yeni özelliğin yüklenme birkaç saniye sürer.

1. Yeni bir Visual C# .NET Core Konsol Uygulaması oluşturun. **Yeni Proje** iletişim kutusunda, sol panelden **Yüklü** > **Visual C#** > **.NET Core**'u genişletin. Ardından **Console App (.NET Core)** seçeneğine tıklayın. Proje adı olarak *helloworld* girin.

    ![Yeni Project iletişim kutusunun ekran görüntüsü](media/sdk/qs-csharp-dotnetcore-windows-01-new-console-app.png "Visual C# Konsol Uygulaması (.NET Core) Oluştur")

1. [Konuşma SDK NuGet paketi](https://aka.ms/csspeech/nuget)'ni yükleyip başvurulara ekleyin. Çözüm Gezgini'nde çözüme sağ tıklayın ve ardından **Çözüm için NuGet Paketlerini Yönet**'i seçin.

    ![Çözüm Gezgini'nin, Çözüm için NuGet Paketlerini Yönet vurgulanmış olarak ekran görüntüsü](media/sdk/qs-csharp-dotnetcore-windows-02-manage-nuget-packages.png "Çözüm için NuGet Paketlerini Yönet")

1. Sağ üst köşede, **Paket Kaynağı** alanında **nuget.org**'u seçin. `Microsoft.CognitiveServices.Speech` paketini arayın ve **helloworld** projesine yükleyin.

    ![Çözüm için Paketleri Yönet iletişim kutusunun ekran görüntüsü](media/sdk/qs-csharp-dotnetcore-windows-03-nuget-install-1.0.0.png "NuGet paketini yükle")

1. NuGet paketinin yükleme işlemini başlatmak için görüntülenen lisansı kabul edin.

    ![Lisans Kabulü iletişim kutusunun ekran görüntüsü](media/sdk/qs-csharp-dotnetcore-windows-04-nuget-license.png "Lisansı kabul et")

Paket yüklendikten sonra Paket Yöneticisi konsolunda bir onay görünür.


## <a name="add-sample-code"></a>Örnek kodu ekleyin

1. `Program.cs` dosyasını açın ve tüm kodu aşağıdakiyle değiştirin.

    [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-dotnetcore-windows/helloworld/Program.cs#code)]

1. Aynı dosyada `YourSubscriptionKey` dizesini abonelik anahtarınız ile değiştirin.

1. Ayrıca `YourServiceRegion` dizesini de aboneliğinizle ilişkili [bölge](regions.md) ile (örneğin ücretsiz deneme aboneliğinde `westus`) değiştirin.

1. Değişiklikleri projeye kaydedin.

## <a name="build-and-run-the-app"></a>Uygulamayı derleme ve çalıştırma

1. Uygulamayı derleyin. Menü çubuğundan **Derle** > **Çözümü Derle**'yi seçin. Kodun hatasız derlenmesi gerekir.

    ![Visual Studio uygulamasının, Çözümü Derle seçeneği vurgulanmış olarak ekran görüntüsü](media/sdk/qs-csharp-dotnetcore-windows-05-build.png "Başarılı derleme")

1. Uygulamayı başlatın. Menü çubuğundan **Hata Ayıklama** > **Hata Ayıklamayı Başlat**'ı seçin veya **F5** tuşuna basın.

    ![Visual Studio uygulamasının, Hata Ayıklamayı Başlat seçeneği vurgulanmış olarak ekran görüntüsü](media/sdk/qs-csharp-dotnetcore-windows-06-start-debugging.png "Uygulamayı hata ayıklamada başlatma")

1. Bir şey söylemenizi isteyen bir konsol penceresi görünür. İngilizce bir deyim ya da cümle söyleyin. Söyledikleriniz Konuşma hizmetine aktarılır ve metne dönüştürülür; metin aynı pencerede görünür.

    ![Başarılı tanımadan sonra konsol çıktısının ekran görüntüsü](media/sdk/qs-csharp-dotnetcore-windows-07-console-output.png "Başarılı tanımadan sonra konsol çıktısı")

[!INCLUDE [Download this sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
`quickstart/csharp-dotnetcore-windows` klasöründe bu örneği arayın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [C# için Konuşma SDK'sını kullanarak konuşmadaki amacı tanıma](how-to-recognize-intents-from-speech-csharp.md)

## <a name="see-also"></a>Ayrıca bkz.

- [Konuşmayı çevirme](how-to-translate-speech-csharp.md)
- [Akustik modelleri özelleştirme](how-to-customize-acoustic-models.md)
- [Dil modellerini özelleştirme](how-to-customize-language-model.md)
