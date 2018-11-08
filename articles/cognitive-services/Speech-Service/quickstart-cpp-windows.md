---
title: "Hızlı Başlangıç: Konuşma Tanıma Hizmeti SDK'sını kullanarak Windows üzerinde C++ dilinde konuşma tanıma"
titleSuffix: Azure Cognitive Services
description: Konuşma Tanıma Hizmeti SDK'sını kullanarak Windows üzerinde C++ dilinde konuşma tanımayı öğrenin
services: cognitive-services
author: wolfma61
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: quickstart
ms.date: 11/06/2018
ms.author: wolfma
ms.openlocfilehash: 8947ba3f39cebf51e956db0d841e393963832bc4
ms.sourcegitcommit: 1b186301dacfe6ad4aa028cfcd2975f35566d756
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2018
ms.locfileid: "51218753"
---
# <a name="quickstart-recognize-speech-in-c-on-windows-by-using-the-speech-sdk"></a>Hızlı Başlangıç: Konuşma SDK'sını kullanarak Windows üzerinde C++ dilinde konuşma tanıma

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Bu makalede Windows için bir C++ konsol uygulaması oluşturacaksınız. Bilgisayarınızın mikrofonundan gerçek zamanda konuşmayı metne dönüştürmek için Bilişsel Hizmetler [Konuşma SDK'sı](speech-sdk.md)'nı kullanırsınız. Uygulama [Konuşma SDK'sı NuGet paketi](https://aka.ms/csspeech/nuget) ve Microsoft Visual Studio 2017 (herhangi bir sürüm) ile geliştirilmiştir.

## <a name="prerequisites"></a>Ön koşullar

Bu Hızlı Başlangıcı tamamlamak için bir Konuşma hizmeti abonelik anahtarınız olması gerekir. Anahtarı ücretsiz alabilirsiniz. Ayrıntılar için bkz. [Konuşma hizmetini ücretsiz olarak deneme](get-started.md).

## <a name="create-a-visual-studio-project"></a>Visual Studio projesi oluşturma

1. Visual Studio 2017'yi başlatın.

1. **C++ ile masaüstü geliştirme** iş yükünün kullanılabilir olduğundan emin olun. Visual Studio yükleyicisini açmak için Visual Studio menü çubuğundan **Araçlar** > **Araçları ve Özellikleri Al**'ı seçin. Bu iş yükü zaten etkinse sonraki adıma atlayın. 

    ![Visual Studio İş Yükleri’nin ekran görüntüsü](media/sdk/vs-enable-cpp-workload.png)

    Aksi takdirde, **C++ ile masaüstü geliştirme**’nin yanındaki kutuyu işaretleyin. 

1. **NuGet paket yöneticisi** bileşeninin kullanılabilir olduğundan emin olun. Visual Studio yükleyicisi iletişim kutusunun **Bağımsız bileşenler** sekmesine geçin ve etkin değilse **NuGet paket yöneticisi**’ni seçin.

      ![Visual Studio Bağımsız bileşenler sekmesinin ekran görüntüsü](media/sdk/vs-enable-nuget-package-manager.png)

1. C++ iş yükünü veya NuGet’i etkinleştirmeniz gerekiyorsa, **Değiştir**’i seçin (iletişim kutusunun sağ alt köşesinde). Yeni özelliklerin yüklenmesi bir dakika sürer. Her iki özellik de zaten etkinse, iletişim kutusunu kapatmanız yeterlidir.

1. Yeni bir Visual C++ Windows Masaüstü Windows Konsol Uygulaması oluşturun. İlk olarak, menüden **Dosya** > **Yeni** > **Proje** seçeneğini belirleyin. **Yeni Proje** iletişim kutusunda, sol bölmeden **Yüklü** > **Visual C++** > **Windows Masaüstü**'nü genişletin. Ardından **Windows Konsol Uygulaması**’nı seçin. Proje adı olarak *helloworld* girin.

    ![Yeni Proje iletişim kutusunun ekran görüntüsü](media/sdk/qs-cpp-windows-01-new-console-app.png)

1. 64 bit Windows işletim sistemini kullanıyorsanız, Visual Studio araç çubuğundaki açılan menüyü kullanarak yapı platformunuzu `x64` işletim sistemine geçirebilirsiniz. (Windows’un 64 bit sürümleri 32 bit uygulamaları çalıştırabildiğinden bu bir gereksinim değildir.)

    ![x64 seçeneğinin vurgulandığı Visual Studio araç çubuğunun ekran görüntüsü](media/sdk/qs-cpp-windows-02-switch-to-x64.png)

1. Çözüm Gezgini'nde çözüme sağ tıklayın ve **Çözüm için NuGet Paketlerini Yönet**'i seçin.

    ![Çözüm Gezgini'nin, Çözüm için NuGet Paketlerini Yönet seçeneğinin vurgulandığı ekran görüntüsü](media/sdk/qs-cpp-windows-03-manage-nuget-packages.png)

1. Sağ üst köşede, **Paket Kaynağı** alanında **nuget.org**'u seçin. `Microsoft.CognitiveServices.Speech` paketini arayın ve **helloworld** projesine yükleyin.

    ![Çözüm için Paketleri Yönet iletişim kutusunun ekran görüntüsü](media/sdk/qs-cpp-windows-04-nuget-install-1.0.0.png)

    > [!NOTE]
    > Bilişsel Hizmetler Konuşma SDK'sının geçerli sürümü: `1.1.0`.

1. NuGet paketinin yükleme işlemini başlatmak için görüntülenen lisansı kabul edin.

    ![Lisans Kabulü iletişim kutusunun ekran görüntüsü](media/sdk/qs-cpp-windows-05-nuget-license.png)

Paket yüklendikten sonra Paket Yöneticisi konsolunda bir onay görünür.

## <a name="add-sample-code"></a>Örnek kodu ekleme

1. *helloworld.cpp* kaynak dosyasını açın. İlk dahil etme deyiminin (`#include "stdafx.h"` veya `#include "pch.h"`) altındaki tüm kodu aşağıdakiyle değiştirin:

   [!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/cpp-windows/helloworld/helloworld.cpp#code)]

1. Aynı dosyada `YourSubscriptionKey` dizesini abonelik anahtarınız ile değiştirin.

1. `YourServiceRegion` dizesini aboneliğinizle ilişkili [bölge](regions.md) ile (örneğin ücretsiz deneme aboneliğinde `westus`) değiştirin.

1. Proje üzerindeki değişiklikleri kaydedin.

## <a name="build-and-run-the-app"></a>Uygulamayı derleme ve çalıştırma

1. Uygulamayı derleyin. Menü çubuğundan **Derle** > **Çözümü Derle**'yi seçin. Kodun hatasız olarak derlenmesi gerekir.

   ![Visual Studio uygulamasının, Çözümü Derle seçeneğinin vurgulandığı ekran görüntüsü](media/sdk/qs-cpp-windows-06-build.png)

1. Uygulamayı başlatın. Menü çubuğundan **Hata Ayıklama** > **Hata Ayıklamayı Başlat**'ı seçin veya **F5** tuşuna basın.

   ![Visual Studio uygulamasının, Hata Ayıklamayı Başlat seçeneğinin vurgulandığı ekran görüntüsü](media/sdk/qs-cpp-windows-07-start-debugging.png)

1. Bir şey söylemenizi isteyen bir konsol penceresi görünür. İngilizce bir deyim ya da cümle söyleyin. Söyledikleriniz Konuşma hizmetine aktarılır ve metne dönüştürülür; metin aynı pencerede görünür.

   ![Başarılı tanıma sonrası konsol çıktısının ekran görüntüsü](media/sdk/qs-cpp-windows-08-console-output-release.png)

[!INCLUDE [Download this sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
`quickstart/cpp-windows` klasöründe bu örneği arayın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [C++ için Konuşma SDK'sını kullanarak konuşmadaki amacı tanıma](how-to-recognize-intents-from-speech-cpp.md)

## <a name="see-also"></a>Ayrıca bkz.

- [Konuşmayı çevirme](how-to-translate-speech-csharp.md)
- [Akustik modelleri özelleştirme](how-to-customize-acoustic-models.md)
- [Dil modellerini özelleştirme](how-to-customize-language-model.md)
