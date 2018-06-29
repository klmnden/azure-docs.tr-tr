---
title: C++ ve Windows için konuşma SDK Quickstart | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Hızlı bir şekilde yardımcı olmak için bilgi ve kod örnekleri get konuşma SDK'sı Windows ve Bilişsel hizmetler C++ kullanmaya başlayın.
services: cognitive-services
author: wolfma61
manager: onano
ms.service: cognitive-services
ms.technology: Speech
ms.topic: article
ms.date: 06/07/2018
ms.author: wolfma
ms.openlocfilehash: 0bcdc3c4357cb8985fad16c607957bffad4a2b8c
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37049239"
---
# <a name="quickstart-for-c-and-windows"></a>C++ ve Windows için hızlı başlangıç

Bilişsel hizmetler konuşma SDK'ın geçerli sürümü `0.4.0`.

Biz Windows Masaüstü için konuşma SDK'sına yararlanır C++ tabanlı bir konsol uygulaması oluşturmak nasıl açıklar.
Uygulama dayanır [Microsoft Bilişsel Services SDK'sı NuGet paketi](https://www.nuget.org/packages/Microsoft.CognitiveServices.Speech) ve Microsoft Visual Studio 2017.

> [!NOTE]
> C++ ve Linux için hızlı başlangıç için arıyorsanız, Git [burada](quickstart-cpp-linux.md).<br>
> C# ve Windows için hızlı başlangıç için arıyorsanız, Git [burada](quickstart-csharp-windows.md).

> [!NOTE]
> Bu hızlı başlangıç çalışma mikrofon ile bir bilgisayar gerektirir.<br>
> Belirli bir ses giriş dosyasından konuşma tanıdığı bir örnek için bkz: [örnek](speech-to-text-sample.md#speech-recognition-from-a-file).

> [!NOTE]
> Visual Studio yüklemenizin içerdiğinden emin olmak **C++ ile masaüstü geliştirme** iş yükü.
> Emin değilseniz, denetlemek ve düzeltmek için bu adımları kullanın: içinde Visual Studio 2017, select **Araçları** \> **alma araçları ve özelliklerinin** ve seçerekkullanıcıhesabıdenetimikomutistemionaylamak**Evet**.
> İçinde **iş yükleri** sekmesinde, varsa **C++ ile masaüstü geliştirme** değil kümesi onay kutusunun yanında varsa, ayarlayın ve tıklayın **Değiştir** değişiklikleri kaydetmek için.

[!include[Get a Subscription Key](includes/get-subscription-key.md)]

## <a name="creating-an-empty-console-application-project"></a>Bir boş konsol uygulama projesi oluşturma

Visual Studio 2017 ' "CppHelloSpeech" adlı yeni bir Visual C++ Windows Masaüstü Windows konsol uygulaması oluşturun:

![Visual C++ Windows Masaüstü Windows konsol uygulaması oluşturun](media/sdk/speechsdk-05-vs-cpp-new-console-app.png)

İsteğe bağlı olarak 64-bit Windows yüklemesinde çalıştırıyorsanız, yapı platformunuz geçiş `x64`:

![Anahtar x64 yapı platformu](media/sdk/speechsdk-07-vs-cpp-switch-to-x64.png)

## <a name="install-and-reference-the-speech-sdk-nuget-package"></a>Yükleme ve konuşma SDK NuGet paketi başvurusu

> [!NOTE]
> NuGet Paket Yöneticisi için Visual Studio 2017 yüklemenizi etkinleştirildiğinden emin olun.
> Visual Studio 2017 içinde seçin **Araçları** \> **alma araçları ve özelliklerinin** ve seçerek kullanıcı hesabı denetimi komut istemi onaylamak **Evet**. Ardından **bileşenleri tek tek** sekmesini tıklatın ve Ara **NuGet Paket Yöneticisi** altında **kod Araçları**.
> Onay kutusunun solunda ayarlanmamışsa ayarlayın ve tıklayın emin olun **Değiştir** değişiklikler kaydedilemiyor.
>
> ![Visual Studio'da NuGet paket Yöneticisini Etkinleştir ](media/sdk/speechsdk-05-vs-enable-nuget-package-manager.png)

Çözüm Gezgini'nde çözüme sağ tıklayın ve tıklayın **çözüm için NuGet paketlerini Yönet**.

![Sağ tıklatın, çözüm için NuGet paketlerini Yönet](media/sdk/speechsdk-09-vs-cpp-manage-nuget-packages.png)

Sağ üst köşedeki içinde **paket kaynağı** alan, "Nuget.org"'ı seçin.
Gelen **Gözat** sekmesinde, aramak için "Microsoft.CognitiveServices.Speech" paketi, onu seçin ve denetleme **proje** ve **CppHelloSpeech** sağdaki kutuları ve seçin **yükleme** CppHelloSpeech projeye yüklemeye.

![Microsoft.CognitiveServices.Speech NuGet paketini yükleyin](media/sdk/speechsdk-11-vs-cpp-manage-nuget-install.png)

Açılır lisans ekranında lisans kabul edin:

![Lisans kabul et](media/sdk/speechsdk-12-vs-cpp-manage-nuget-license.png)

## <a name="add-the-sample-code"></a>Örnek kod ekleme

Varsayılan başlangıç kodunuzu aşağıdaki biriyle değiştirin:

[!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/Windows/quickstart-cpp/CppHelloSpeech.cpp#code)]

> [!IMPORTANT]
> Abonelik anahtarı elde biriyle değiştirin. <br>
> Bölge, bölgesinden değiştirin [konuşma hizmeti REST API'si](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/rest-apis), örneğin 'westus' ile değiştirin.

![Abonelik anahtarınızı ekleme](media/sdk/sub-key-recognize-speech-cpp.png)

## <a name="build-and-run-the-sample"></a>Örneği derleme ve çalıştırma

Kodu hatasız şimdi derlemek:

![Başarılı derleme](media/sdk/speechsdk-16-vs-cpp-build.png)

Başlat düğmesi veya F5 klavye kısayolunu kullanarak hata ayıklayıcı altında programı başlatın:

![Hata ayıklama içine uygulamayı başlatın](media/sdk/speechsdk-17-vs-cpp-f5.png)

Bir konsol penceresi, şeyin (İngilizce) deyin isteyen pop.
Tanıma sonucunu ekranında görüntülenir.

![Başarılı tanıma sonra konsol çıkışı](media/sdk/speechsdk-18-vs-cpp-console-output-release.png)

## <a name="downloading-the-sample"></a>Örnek indirme

En son örnekleri için bkz [Bilişsel hizmetler konuşma SDK örnek GitHub deposunu](https://aka.ms/csspeech/samples).

## <a name="next-steps"></a>Sonraki adımlar

* Ziyaret [örnekleri sayfa](samples.md) ek örnekler için.
