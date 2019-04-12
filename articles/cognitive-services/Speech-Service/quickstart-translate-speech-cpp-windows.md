---
title: 'Hızlı Başlangıç: Translate speech, C++ (Windows) - Speech Services'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, komut satırına metin çıktısı kullanıcı konuşma yakalamak ve başka bir dile çevirmek için basit bir C++ uygulaması oluşturacaksınız. Bu kılavuz, Windows kullanıcıları için tasarlanmıştır.
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 03/13/2019
ms.author: erhopf
ms.openlocfilehash: f7120e80d079723ed8265320ba4b38d76a825a00
ms.sourcegitcommit: 41015688dc94593fd9662a7f0ba0e72f044915d6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/11/2019
ms.locfileid: "59499827"
---
# <a name="quickstart-translate-speech-with-the-speech-sdk-for-c"></a>Hızlı Başlangıç: C++ için konuşma Speech SDK'sı ile Çevir

Bu hızlı başlangıçta, bilgisayarınızın mikrofondan gelen kullanıcı konuşma yakalar, konuşma çevirir ve gerçek zamanlı olarak komut satırında çevrilmiş metne dönüştürür basit bir C++ uygulaması oluşturacaksınız. Bu uygulama, 64 bit Windows üzerinde çalışacak şekilde tasarlanmıştır ve ile oluşturulmuş [konuşma SDK'sı NuGet paketi](https://aka.ms/csspeech/nuget) ve Microsoft Visual Studio 2017.

Konuşma çevirisi için kullanılabilen dilleri tam bir listesi için bkz. [dil desteği](language-support.md).

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıç şunları gerektirir:

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
* Konuşma hizmeti için bir Azure aboneliği anahtarı. [Ücretsiz edinin](get-started.md).

## <a name="create-a-visual-studio-project"></a>Visual Studio projesi oluşturma

[!INCLUDE[](../../../includes/cognitive-services-speech-service-quickstart-cpp-create-proj.md)]

## <a name="add-sample-code"></a>Örnek kod ekleme

1. *helloworld.cpp* kaynak dosyasını açın. İlk dahil etme deyiminin (`#include "stdafx.h"` veya `#include "pch.h"`) altındaki tüm kodu aşağıdakiyle değiştirin:

    [!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/speech-translation/cpp-windows/helloworld/helloworld.cpp#code)]

1. Aynı dosyada `YourSubscriptionKey` dizesini abonelik anahtarınız ile değiştirin.

1. `YourServiceRegion` dizesini aboneliğinizle ilişkili [bölge](regions.md) ile (örneğin ücretsiz deneme aboneliğinde `westus`) değiştirin.

1. Proje üzerindeki değişiklikleri kaydedin.

## <a name="build-and-run-the-app"></a>Uygulamayı derleme ve çalıştırma

1. Uygulamayı derleyin. Menü çubuğundan **Derle** > **Çözümü Derle**'yi seçin. Kodun hatasız olarak derlenmesi gerekir.

   ![Visual Studio uygulamasının, Çözümü Derle seçeneğinin vurgulandığı ekran görüntüsü](media/sdk/qs-cpp-windows-06-build.png)

1. Uygulamayı başlatın. Menü çubuğundan **Hata Ayıklama** > **Hata Ayıklamayı Başlat**'ı seçin veya **F5** tuşuna basın.

   ![Visual Studio uygulamasının, Hata Ayıklamayı Başlat seçeneğinin vurgulandığı ekran görüntüsü](media/sdk/qs-cpp-windows-07-start-debugging.png)

1. Bir şey söylemenizi isteyen bir konsol penceresi görünür. İngilizce bir deyim ya da cümle söyleyin. Konuşma konuşma tanıma Hizmeti'ne aktarılan, çevrilmiş ve transcribed aynı pencerede görünen metin.

   ![Başarılı çeviri sonra konsol çıktısı ekran görüntüsü](media/sdk/qs-translate-cpp-windows-output.png)

## <a name="next-steps"></a>Sonraki adımlar

Konuşma bir ses dosyasını okuma ve Sentezlenen konuşma olarak çevrilmiş metin çıktısı gibi ek örnekler Github'da kullanılabilir.

> [!div class="nextstepaction"]
> [GitHub üzerinde C++ örneklerini keşfedin](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Ayrıca bkz.

- [Akustik modelleri özelleştirme](how-to-customize-acoustic-models.md)
- [Dil modellerini özelleştirme](how-to-customize-language-model.md)
