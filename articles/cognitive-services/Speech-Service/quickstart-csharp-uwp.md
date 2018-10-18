---
title: "Hızlı Başlangıç: Bilişsel Hizmetler Konuşma SDK'sını kullanarak UWP uygulamasında C# dilinde konuşma tanıma"
titleSuffix: Microsoft Cognitive Services
description: Bilişsel Hizmetler Konuşma SDK'sını kullanarak UWP uygulamasında C# dilinde konuşma tanımayı öğrenin
services: cognitive-services
author: wolfma61
ms.service: cognitive-services
ms.component: speech-service
ms.topic: quickstart
ms.date: 09/24/2018
ms.author: wolfma
ms.openlocfilehash: d9a90869e060d2f8f1a1c522a4528e74841caada
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49339623"
---
# <a name="quickstart-recognize-speech-in-a-uwp-app-by-using-the-speech-sdk"></a>Hızlı Başlangıç: Konuşma SDK'sını kullanarak UWP uygulamasında konuşma tanıma

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Bu makalede Bilişsel Hizmetler[Konuşma SDK'sı](speech-sdk.md)'nı kullanarak bir C# Evrensel Windows Platformu (UWP) uygulaması oluşturacaksınız. Cihazınızın mikrofonundan gerçek zamanda konuşmayı metne dönüştüreceksiniz. Uygulama [Konuşma SDK'sı NuGet Paketi](https://aka.ms/csspeech/nuget) ve Microsoft Visual Studio 2017 (herhangi bir sürüm) ile geliştirilmiştir.

> [!NOTE]
> Evrensel Windows Platformu; PC, Xbox, Surface Hub ve diğer cihazlar dahil olmak üzere Windows 10 destekleyen tüm cihazlarda çalışan uygulamalar geliştirmenize olanak tanır.

## <a name="prerequisites"></a>Ön koşullar

Bu Hızlı Başlangıcı tamamlamak için bir Konuşma hizmeti abonelik anahtarınız olması gerekir. Anahtarı ücretsiz alabilirsiniz. Ayrıntılar için bkz. [Konuşma hizmetini ücretsiz olarak deneme](get-started.md).

## <a name="create-a-visual-studio-project"></a>Visual Studio projesi oluşturma

1. Visual Studio 2017'yi başlatın.

1. **Evrensel Windows Platformu geliştirmesi** iş yükünün bulunduğundan emin olun. Visual Studio yükleyicisini açmak için Visual Studio menü çubuğundan **Araçlar** > **Araçları ve Özellikleri Al**'ı seçin. Bu iş yükü zaten etkinse iletişim kutusunu kapatın. 

    ![Visual Studio yükleyicisinin İş yükleri sekmesi vurgulanmış olarak ekran görüntüsü](media/sdk/vs-enable-uwp-workload.png)

    Etkin değilse **.NET çoklu platform geliştirme**'nin yanındaki kutuyu ve iletişim kutusunun sağ alt köşesindeki **Değiştir**'i seçin. Yeni özelliğin yüklenmesi bir dakika sürer.

1. Boş bir Visual C# Evrensel Windows uygulaması oluşturun. İlk olarak, menüden **Dosya** > **Yeni** > **Proje** seçeneğini belirleyin. **Yeni Proje** iletişim kutusunda, sol bölmeden **Yüklü** > **Visual C#** > **Windows Evrensel**'i genişletin. Sonra **Boş Uygulama (Evrensel Windows)** seçeneğini belirleyin. Proje adı olarak *helloworld* girin.

    ![Yeni Proje iletişim kutusunun ekran görüntüsü](media/sdk/qs-csharp-uwp-01-new-blank-app.png)

1. Hız SDK’sı, uygulamanızın Windows 10 Fall Creators Update veya sonraki sürümü için derlenmiş olmasını gerektirir. Çıkan **Yeni Evrensel Windows Platformu Projesi** penceresinde **Windows 10 Fall Creators Update (10.0; Sürüm 16299)** seçeneğini **En düşük sürüm** olarak belirleyin. **Hedef sürümü** kutusunda, bu veya sonraki bir sürümü seçin ve ardından **Tamam**’a tıklayın.

    ![Yeni Evrensel Windows Platformu Projesi penceresinin ekran görüntüsü](media/sdk/qs-csharp-uwp-02-new-uwp-project.png)

1. 64 bit Windows işletim sistemini kullanıyorsanız, Visual Studio araç çubuğundaki açılan menüyü kullanarak yapı platformunuzu `x64` işletim sistemine geçirebilirsiniz. (64 bit Windows, 32 bit uygulamaları çalıştırabilir, bu yüzden dilerseniz `x86` olarak ayarlanmış şekilde bırakabilirsiniz.)

   ![x64 seçeneğinin vurgulandığı Visual Studio araç çubuğunun ekran görüntüsü](media/sdk/qs-csharp-uwp-03-switch-to-x64.png)

   > [!NOTE]
   > Konuşma SDK'sı yalnızca Intel uyumlu işlemcileri destekler. ARM şu anda desteklenmiyor.

1. [Konuşma SDK NuGet paketi](https://aka.ms/csspeech/nuget)'ni yükleyip başvurulara ekleyin. Çözüm Gezgini'nde çözüme sağ tıklayın ve **Çözüm için NuGet Paketlerini Yönet**'i seçin.

    ![Çözüm Gezgini'nin, Çözüm için NuGet Paketlerini Yönet seçeneğinin vurgulandığı ekran görüntüsü](media/sdk/qs-csharp-uwp-04-manage-nuget-packages.png)

1. Sağ üst köşede, **Paket Kaynağı** alanında **nuget.org**'u seçin. `Microsoft.CognitiveServices.Speech` paketini arayın ve **helloworld** projesine yükleyin.

    ![Çözüm için Paketleri Yönet iletişim kutusunun ekran görüntüsü](media/sdk/qs-csharp-uwp-05-nuget-install-1.0.0.png "NuGet paketini yükle")

1. NuGet paketinin yükleme işlemini başlatmak için görüntülenen lisansı kabul edin.

    ![Lisans Kabulü iletişim kutusunun ekran görüntüsü](media/sdk/qs-csharp-uwp-06-nuget-license.png "Lisansı kabul et")

1. Paket Yöneticisi konsolunda aşağıdaki çıkış satırı görüntülenir.

   ```text
   Successfully installed 'Microsoft.CognitiveServices.Speech 1.0.1' to helloworld
   ```

1. Uygulama, konuşma girdisi için mikrofon kullandığından, **Mikrofon** özelliğini projeye ekleyin. Çözüm Gezgini'nde, uygulama bildiriminizi düzenlemek için **Package.appxmanifest** dosyasına çift tıklayın. Ardından **Özellikler** sekmesine geçin, **Mikrofon** özelliği kutusunu seçin ve değişikliklerinizi kaydedin.

   ![Özellikler ve Mikrofonun vurgulandığı Visual Studio uygulama bildiriminin ekran görüntüsü](media/sdk/qs-csharp-uwp-07-capabilities.png)


## <a name="add-sample-code"></a>Örnek kodu ekleme

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

1. **Mikrofon girdisi ile konuşma tanıma**’yı seçin ve cihazınızın mikrofonuna İngilizce bir deyim ya da cümle söyleyin. Söyledikleriniz Konuşma hizmetine aktarılır ve metne dönüştürülür; metin pencerede görünür.

    ![Konuşma tanıma kullanıcı arabiriminin ekran görüntüsü](media/sdk/qs-csharp-uwp-11-ui-result.png)

[!INCLUDE [Download this sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
`quickstart/csharp-uwp` klasöründe bu örneği arayın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [C# için Konuşma SDK'sını kullanarak konuşmadaki amacı tanıma](how-to-recognize-intents-from-speech-csharp.md)

## <a name="see-also"></a>Ayrıca bkz.

- [Konuşmayı çevirme](how-to-translate-speech-csharp.md)
- [Akustik modelleri özelleştirme](how-to-customize-acoustic-models.md)
- [Dil modellerini özelleştirme](how-to-customize-language-model.md)
