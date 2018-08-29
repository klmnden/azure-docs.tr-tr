---
title: "Hızlı Başlangıç: Bilişsel hizmetler konuşma SDK'sını kullanarak Windows masaüstünde c++ konuşma tanıma"
titleSuffix: Microsoft Cognitive Services
description: Bilişsel hizmetler konuşma SDK'sını kullanarak Windows masaüstünde c++ Konuşma tanımayı öğrenmesine
services: cognitive-services
author: wolfma61
ms.service: cognitive-services
ms.technology: Speech
ms.topic: article
ms.date: 07/16/2018
ms.author: wolfma
ms.openlocfilehash: 9fae855de2a746084f4775f194e04c6dbe09f684
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2018
ms.locfileid: "43127300"
---
# <a name="quickstart-recognize-speech-in-c-on-windows-desktop-using-the-speech-sdk"></a>Hızlı Başlangıç: c++ konuşma Speech SDK'sı kullanarak Windows masaüstünde tanıması

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Biz Windows Masaüstü için konuşma SDK'sının kullanır tabanlı C++ konsol uygulaması oluşturma işlemini açıklar.
Uygulama dayanır [Microsoft Bilişsel hizmetler konuşma SDK'sı NuGet paketi](https://aka.ms/csspeech/nuget) ve Microsoft Visual Studio 2017.

## <a name="prerequisites"></a>Önkoşullar

* Konuşma hizmeti için bir abonelik anahtarı. Bkz: [konuşma hizmeti ücretsiz olarak deneyin](get-started.md).
* Bir çalışma mikrofon içeren bir Windows bilgisayar.
* [Microsoft Visual Studio 2017](https://www.visualstudio.com/), Community sürümü veya üzeri.
* **C++ ile masaüstü geliştirme** Visual Studio'da iş yükü ve **NuGet Paket Yöneticisi** Visual Studio'daki bileşen.
  Her ikisini de etkinleştirebilirsiniz **Araçları** \> **araçları ve özellikleri Al**altında **iş yükleri** ve **tek tek bileşenler** sekmeleri , sırasıyla:

  ![C++ iş yükünde ile masaüstü geliştirme etkinleştir](media/sdk/vs-enable-cpp-workload.png)

  ![Visual Studio'da NuGet Paket Yöneticisi'ni etkinleştir ](media/sdk/vs-enable-nuget-package-manager.png)

## <a name="create-a-visual-studio-project"></a>Visual Studio projesi oluşturma

Visual Studio 2017'de yeni bir Visual C++ Windows Masaüstü Windows konsol uygulaması oluşturun. İçinde **yeni proje** iletişim kutusunda, sol bölmeden genişletin **yüklü** \> **Visual C++** \> **WindowsMasaüstü** seçip **Windows konsol uygulaması**. Proje adı olarak *helloworld*.

![Visual C++ Windows Masaüstü Windows konsol uygulaması oluşturun](media/sdk/qs-cpp-windows-01-new-console-app.png)

Bir 64 bit Windows yüklemesinde çalıştırıyorsanız, yapı platformunuz için isteğe bağlı olarak geçiş `x64`:

![Yapı platformunu x64 geçiş](media/sdk/qs-cpp-windows-02-switch-to-x64.png)

## <a name="install-and-reference-the-speech-sdk-nuget-package"></a>Yükleme ve konuşma SDK'sı NuGet paketi başvurusu

Çözüm Gezgini'nde çözüme sağ tıklayın ve tıklayarak **çözüm için NuGet paketlerini Yönet**.

![Sağ tıklatın, çözüm için NuGet paketlerini Yönet](media/sdk/qs-cpp-windows-03-manage-nuget-packages.png)

Sağ üst köşede, içinde **paket kaynağı** alanında, "Nuget.org" seçin.
Gelen **Gözat** sekmesinde, "Microsoft.CognitiveServices.Speech" paketini arayın, seçin ve denetleme **proje** ve **helloworld** sağ ve seçim kutuları **Yükleme** helloworld projeye yüklemek için.

> [!NOTE]
> Bilişsel hizmetler konuşma SDK'ın geçerli sürümü `0.6.0`.

![Microsoft.CognitiveServices.Speech NuGet paketini yükle](media/sdk/qs-cpp-windows-04-nuget-install-0.5.0.png)

Açılır lisans ekranda lisansı kabul edin:

![Lisansı kabul edin](media/sdk/qs-cpp-windows-05-nuget-license.png)

## <a name="add-the-sample-code"></a>Örnek kod ekleyin

1. Varsayılan başlangıç kodunuzu aşağıdaki biriyle değiştirin:

   [!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/cpp-windows/helloworld/helloworld.cpp#code)]

1. Dize değiştirin `YourSubscriptionKey` abonelik.

1. Dize değiştirin `YourServiceRegion` ile [bölge](regions.md) aboneliğinizle ilişkili (örneğin, `westus` ücretsiz deneme aboneliği için).

1. Değişiklikleri projeye kaydedin.

## <a name="build-and-run-the-sample"></a>Örneği derleme ve çalıştırma

1. Uygulamayı oluşturun. Menü çubuğundan seçin **derleme** > **Çözümü Derle**. Kod hatasız artık derlemeniz gerekir:

   ![Başarılı derleme](media/sdk/qs-cpp-windows-06-build.png)

1. Uygulamayı başlatın. Menü çubuğundan seçin **hata ayıklama** > **hata ayıklamayı Başlat**, veya basın **F5**.

   ![INTO hata ayıklaması uygulamayı başlatın](media/sdk/qs-cpp-windows-07-start-debugging.png)

1. Bir şeyin (İngilizce) söyleyin isteyip istemediğinizi soran bir konsol penceresi açılır.
   Tanıma sonucunu ekranında görüntülenir.

   ![Başarılı tanıma sonra konsol çıktısı](media/sdk/qs-cpp-windows-08-console-output-release.png)

[!INCLUDE [Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Bu örnekte arayın `quickstart/cpp-windows` klasör.

## <a name="next-steps"></a>Sonraki adımlar

* [Örneklerimizi Al](speech-sdk.md#get-the-samples)
