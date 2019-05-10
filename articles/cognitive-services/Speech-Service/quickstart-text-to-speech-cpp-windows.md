---
title: 'Hızlı Başlangıç: Okuma, C++ (Windows) - konuşma Hizmetleri sentezlemek'
titleSuffix: Azure Cognitive Services
description: Windows Masaüstü c++ konuşma Speech SDK'sı kullanarak sentezlemek öğrenin
services: cognitive-services
author: yinhew
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 4/03/2019
ms.author: yinhew
ms.openlocfilehash: 7e9aebd3a49182f84e05473da9ed166499f34a28
ms.sourcegitcommit: e6d53649bfb37d01335b6bcfb9de88ac50af23bd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65465702"
---
# <a name="quickstart-synthesize-speech-in-c-on-windows-by-using-the-speech-sdk"></a>Hızlı Başlangıç: Konuşma SDK'sını kullanarak Windows üzerinde c++ konuşma sentezlemek

Hızlı Başlangıçlar ücret karşılığında ayrıca [konuşma tanıma](quickstart-cpp-windows.md) ve [konuşma çevirisi](quickstart-translate-speech-cpp-windows.md).

Bu makalede Windows için bir C++ konsol uygulaması oluşturacaksınız. Bilişsel Hizmetler'i kullanma [Speech SDK'sı](speech-sdk.md) sentezlemek konuşma gerçek zamanlı metin ve konuşma bilgisayarınızın hoparlöründen yürütmek için. Uygulama [Konuşma SDK'sı NuGet paketi](https://aka.ms/csspeech/nuget) ve Microsoft Visual Studio 2017 (herhangi bir sürüm) ile geliştirilmiştir.

Bu makalede açıklanan özellik kullanılabilir [Speech SDK'sı 1.5.0](https://www.nuget.org/packages/Microsoft.CognitiveServices.Speech/1.5.0).

Diller/ses konuşma sentezi için kullanılabilen tam bir listesi için bkz. [dil desteği](language-support.md#text-to-speech).

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlamak için bir konuşma Hizmetleri abonelik anahtarı ihtiyacınız vardır. Anahtarı ücretsiz alabilirsiniz. Bkz: [konuşma Hizmetleri ücretsiz olarak deneyin](get-started.md) Ayrıntılar için.

## <a name="create-a-visual-studio-project"></a>Visual Studio projesi oluşturma

[!INCLUDE [Quickstart C++ project](../../../includes/cognitive-services-speech-service-quickstart-cpp-create-proj.md)]

## <a name="add-sample-code"></a>Örnek kod ekleyin

1. *helloworld.cpp* kaynak dosyasını açın. İlk dahil etme deyiminin (`#include "stdafx.h"` veya `#include "pch.h"`) altındaki tüm kodu aşağıdakiyle değiştirin:

   [!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/text-to-speech/cpp-windows/helloworld/helloworld.cpp#code)]

1. Aynı dosyada `YourSubscriptionKey` dizesini abonelik anahtarınız ile değiştirin.

1. `YourServiceRegion` dizesini aboneliğinizle ilişkili [bölge](regions.md) ile (örneğin ücretsiz deneme aboneliğinde `westus`) değiştirin.

1. Proje üzerindeki değişiklikleri kaydedin.

## <a name="build-and-run-the-app"></a>Uygulamayı derleme ve çalıştırma

1. Uygulamayı derleyin. Menü çubuğundan **Derle** > **Çözümü Derle**'yi seçin. Kodun hatasız olarak derlenmesi gerekir.

   ![Visual Studio uygulamasının, Çözümü Derle seçeneğinin vurgulandığı ekran görüntüsü](media/sdk/qs-cpp-windows-06-build.png)

1. Uygulamayı başlatın. Menü çubuğundan **Hata Ayıklama** > **Hata Ayıklamayı Başlat**'ı seçin veya **F5** tuşuna basın.

   ![Visual Studio uygulamasının, Hata Ayıklamayı Başlat seçeneğinin vurgulandığı ekran görüntüsü](media/sdk/qs-cpp-windows-07-start-debugging.png)

1. Bir metin yazın isteyip istemediğinizi soran bir konsol penceresi görünür. Birkaç sözcük veya bir cümle yazın. Yazdığınız metni konuşma hizmetlere iletilen ve konuşma tanıma, hoparlöründen için oluşturulan.

   ![Başarılı sentezi sonra konsol çıktısı ekran görüntüsü](media/sdk/qs-tts-cpp-windows-console-output.png)

## <a name="next-steps"></a>Sonraki adımlar

Konuşma bir ses dosyasına kaydetme gibi ek örnekler, Github'da kullanılabilir.

> [!div class="nextstepaction"]
> [GitHub üzerinde C++ örneklerini keşfedin](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Ayrıca bkz.

- [Ses tiplerini özelleştirme](how-to-customize-voice-font.md)
- [Kayıt ses örnekleri](record-custom-voice-samples.md)
