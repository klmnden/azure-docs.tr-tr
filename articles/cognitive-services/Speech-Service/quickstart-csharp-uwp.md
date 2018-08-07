---
title: "Hızlı Başlangıç: Bilişsel hizmetler konuşma SDK'sını kullanarak bir UWP uygulamasında C# ' de Konuşma tanıma"
titleSuffix: Microsoft Cognitive Services
description: Bilişsel hizmetler konuşma SDK'sını kullanarak bir UWP uygulamasında Konuşma tanımayı öğrenmesine
services: cognitive-services
author: wolfma61
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 07/16/2018
ms.author: wolfma
ms.openlocfilehash: d1245b07ac0de680c13542b9af86b25bdf517c21
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2018
ms.locfileid: "39576143"
---
# <a name="quickstart-recognize-speech-in-a-uwp-app-using-the-speech-sdk"></a>Hızlı Başlangıç: bir UWP uygulamasında Speech SDK'sı kullanarak konuşma tanıma

[!include[Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Bu makalede, Konuşmayı metne dönüştürme özelliği Bilişsel hizmetler konuşma SDK'sını kullanarak bir evrensel Windows Platformu (UWP) uygulamanın nasıl oluşturulacağını öğrenin.
Uygulamanın oluşturulduğu [Microsoft Bilişsel hizmetler konuşma SDK'sı NuGet paketi](https://aka.ms/csspeech/nuget) ve Microsoft Visual Studio 2017.

> [!NOTE]
> Evrensel Windows platformu, Windows 10 bilgisayarları, Xbox, Surface Hub ve diğer cihazları dahil olmak üzere, destekleyen herhangi bir cihaz üzerinde çalışan uygulamalar geliştirmenize olanak tanır. Konuşma SDK'yı kullanarak uygulamaları, Windows uygulama sertifikası Kiti (WACK) henüz geçmeyin. Uygulamanızı, ancak şu anda Windows Store için gönderilen dışarıdan yükleme için mümkündür.

## <a name="prerequisites"></a>Önkoşullar

* Konuşma hizmeti için bir abonelik anahtarı. Bkz: [konuşma hizmeti ücretsiz olarak deneyin](get-started.md).
* Bir Windows PC (Windows 10 Fall Creators Update veya üzeri) ile çalışma mikrofon.
* [Microsoft Visual Studio 2017](https://www.visualstudio.com/), Community sürümü veya üzeri.
* **Evrensel Windows platformu geliştirme** Visual Studio.You iş yükünde içinde etkinleştirebilir **Araçları** \> **araçları ve özellikleri Al**.

  ![Evrensel Windows platformu geliştirme etkinleştir](media/sdk/vs-enable-uwp-workload.png)

## <a name="create-a-visual-studio-project"></a>Visual Studio projesi oluşturma

1. Visual Studio 2017'de yeni bir Visual C# Windows Evrensel boş uygulaması oluşturun. İçinde **yeni proje** iletişim kutusunda, sol bölmeden genişletin **yüklü** \> **Visual C#** \> **WindowsEvrensel** seçip **boş uygulama (Evrensel Windows)**. Proje adı olarak *helloworld*.

    ![](media/sdk/qs-csharp-uwp-01-new-blank-app.png)

1. İçinde **yeni evrensel Windows platformu projesi** açılarak Seç penceresi **Windows 10 Fall Creators Update (10.0; 16299 yapı)** olarak **en düşük sürüm**ve bu ya da sonraki bir sürümünü olarak **hedef sürümü**, ardından **Tamam**.

    ![](media/sdk/qs-csharp-uwp-02-new-uwp-project.png)

1. Bir 64 bit Windows yüklemesinde çalıştırıyorsanız, yapı platformunuz geçiş yapabilirsiniz `x64`.

   ![Yapı platformunu x64 geçiş](media/sdk/qs-csharp-uwp-03-switch-to-x64.png)

   > [!NOTE]
   > Şu anda Speech SDK'sı Intel uyumlu işlemci yanı sıra, ARM değil de destekler.

1. Yükleme ve başvuru [konuşma SDK'sı NuGet paketi](https://aka.ms/csspeech/nuget). Çözüm Gezgini'nde çözüme sağ tıklayıp seçin **çözüm için NuGet paketlerini Yönet**.

    ![Sağ tıklatın, çözüm için NuGet paketlerini Yönet](media/sdk/qs-csharp-uwp-04-manage-nuget-packages.png)

1. Sağ üst köşede, içinde **paket kaynağı** alanın, Seç **Nuget.org**. Arama ve yükleme `Microsoft.CognitiveServices.Speech` paketini ve yükleme içine **helloworld** proje.

    ![Microsoft.CognitiveServices.Speech NuGet paketini yüklemek](media/sdk/qs-csharp-uwp-05-nuget-install-0.5.0.png "yükleme Nuget paketi")

1. Görüntülenen lisansı kabul edin.

    ![Bu lisansı kabul](media/sdk/qs-csharp-uwp-06-nuget-license.png "lisansı kabul edin")

1. Paket Yöneticisi Konsolu'nda aşağıdaki çıktı satırı görüntülenir.

   ```text
   Successfully installed 'Microsoft.CognitiveServices.Speech 0.5.0' to helloworld
   ```

## <a name="add-the-sample-code"></a>Örnek kod ekleyin

1. Çözüm Gezgini'nde çift tıklayarak **Package.appxmanifest** uygulama bildiriminizi düzenlenecek.
   Seçin **özellikleri** sekmesinde, onay kutusunu seçip **mikrofon** özelliği ve değişikliklerinizi kaydedin.

   ![](media/sdk/qs-csharp-uwp-07-capabilities.png)

1. Uygulamanızın kullanıcı arabirimi çift tıklayarak düzenleme `MainPage.xaml` Çözüm Gezgini'nde. 

    Tasarımcının XAML görünümünde, aşağıdaki XAML kod parçacığı kılavuz etiketine ekleyin (arasında `<Grid>` ve `</Grid>`).

   [!code-xml[UI elements](~/samples-cognitive-services-speech-sdk/quickstart/csharp-uwp/helloworld/MainPage.xaml#StackPanel)]

1. XAML kodu arka plan çift tıklayarak düzenleme `MainPage.xaml.cs` Çözüm Gezgini'nde (altında gruplandırılmış `MainPage.xaml` öğesi).
   Bu dosyadaki tüm kodu aşağıdakiyle değiştirin.

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-uwp/helloworld/MainPage.xaml.cs#code)]

1. İçinde `SpeechRecognitionFromMicrophone_ButtonClicked` işleyicisi dizesini değiştirin `YourSubscriptionKey` abonelik.

1. İçinde `SpeechRecognitionFromMicrophone_ButtonClicked` işleyicisi dizesini değiştirin `YourServiceRegion` ile [bölge](regions.md) aboneliğinizle ilişkili (örneğin, `westus` ücretsiz deneme aboneliği için).

1. Tüm değişiklikleri projeye kaydedin.

## <a name="build-and-run-the-sample"></a>Örneği derleme ve çalıştırma

1. Uygulamayı oluşturun. Menü çubuğundan seçin **derleme** > **Çözümü Derle**. Kod hatasız artık derlemeniz gerekir.

    ![Başarılı derleme](media/sdk/qs-csharp-uwp-08-build.png "başarılı derleme")

1. Uygulamayı başlatın. Menü çubuğundan seçin **hata ayıklama** > **hata ayıklamayı Başlat**, veya basın **F5**.

    ![Uygulamayı hata ayıklama içine başlatın](media/sdk/qs-csharp-uwp-09-start-debugging.png "INTO hata ayıklaması uygulamayı başlatın")

1. GUI penceresi açılır. İlk **etkinleştirme mikrofon** düğmesine ve görüntülenen iletideki izin isteği kabul etmiş olursunuz.

    ![Uygulamayı hata ayıklama içine başlatın](media/sdk/qs-csharp-uwp-10-access-prompt.png "INTO hata ayıklaması uygulamayı başlatın")

1. Tıklayın **mikrofon giriş ile Konuşma tanıma** ve cihazınızın mikrofona kısa ifade. Tanınan metin penceresinde görünür.

    ![](media/sdk/qs-csharp-uwp-11-ui-result.png)

[!include[Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Bu örnekte arayın `quickstart/csharp-uwp` klasör.

## <a name="next-steps"></a>Sonraki adımlar

- [Konuşma Çevir](how-to-translate-speech-csharp.md)
- [Akustik model özelleştirme](how-to-customize-acoustic-models.md)
- [Dil modeli özelleştirme](how-to-customize-language-model.md)
