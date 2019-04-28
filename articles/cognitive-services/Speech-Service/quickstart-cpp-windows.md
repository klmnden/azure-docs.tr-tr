---
title: 'Hızlı Başlangıç: C++ (Windows) - konuşma Hizmetleri konuşma tanıma'
titleSuffix: Azure Cognitive Services
description: Konuşma SDK'sını kullanarak Windows masaüstünde c++ Konuşma tanımayı öğrenmesine
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 12/13/2018
ms.author: wolfma
ms.openlocfilehash: a3fe14436a569a8354e4bfd58a9ba164881d8fe2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61462497"
---
# <a name="quickstart-recognize-speech-in-c-on-windows-by-using-the-speech-sdk"></a>Hızlı Başlangıç: Konuşma SDK'sını kullanarak Windows üzerinde c++ konuşma tanıma

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Bu makalede Windows için bir C++ konsol uygulaması oluşturacaksınız. Bilgisayarınızın mikrofonundan gerçek zamanda konuşmayı metne dönüştürmek için Bilişsel Hizmetler [Konuşma SDK'sı](speech-sdk.md)'nı kullanırsınız. Uygulama [Konuşma SDK'sı NuGet paketi](https://aka.ms/csspeech/nuget) ve Microsoft Visual Studio 2017 (herhangi bir sürüm) ile geliştirilmiştir.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlamak için bir konuşma Hizmetleri abonelik anahtarı ihtiyacınız vardır. Anahtarı ücretsiz alabilirsiniz. Bkz: [konuşma Hizmetleri ücretsiz olarak deneyin](get-started.md) Ayrıntılar için.

## <a name="create-a-visual-studio-project"></a>Visual Studio projesi oluşturma

[!INCLUDE [Quickstart C++ project](../../../includes/cognitive-services-speech-service-quickstart-cpp-create-proj.md)]

## <a name="add-sample-code"></a>Örnek kod ekleme

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

1. Bir şey söylemenizi isteyen bir konsol penceresi görünür. İngilizce bir deyim ya da cümle söyleyin. Konuşma konuşma hizmetlere iletilen ve transcribed aynı pencerede görünen metin.

   ![Başarılı tanıma sonrasında konsol çıktısının ekran görüntüsü](media/sdk/qs-cpp-windows-08-console-output-release.png)

## <a name="next-steps"></a>Sonraki adımlar

Konuşma bir ses dosyasından okuma gibi ek örnekler Github'da kullanılabilir.

> [!div class="nextstepaction"]
> [GitHub üzerinde C++ örneklerini keşfedin](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Ayrıca bkz.

- [Akustik modelleri özelleştirme](how-to-customize-acoustic-models.md)
- [Dil modellerini özelleştirme](how-to-customize-language-model.md)
