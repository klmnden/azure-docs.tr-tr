---
title: 'Hızlı Başlangıç: hedefi yararlanın-C#'
titleSuffix: Language Understanding - Azure Cognitive Services
description: Bu hızlı başlangıçta, kullanılabilir genel bir LUIS uygulamasını kullanarak konuşma metninden bir kullanıcının amacını belirleyin. C# kullanarak kullanıcının amacını genel uygulamanın HTTP tahmin uç noktasına metin olarak gönderin. Uç noktada, LUIS genel uygulamanın modelini uygulayarak anlam oluşturmak için doğal dil metnini analiz eder, böylece genel amacı belirler ve uygulamanın konu etki alanıyla ilgili verileri ayıklar.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: c498554bea0722f81d5dd52d77c0bc88d16e2332
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53015074"
---
# <a name="quickstart-get-intent-using-c"></a>Hızlı Başlangıç: C# kullanarak amacı alma

[!INCLUDE [Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

<a name="create-luis-subscription-key"></a>

## <a name="prerequisites"></a>Önkoşullar

* [Visual Studio Community 2017 sürümü](https://visualstudio.microsoft.com/vs/community/)
* C# programlama dili (VS Community 2017 sürümünde bulunur)
* Genel uygulama kimliği: df67dcdb-c37d-46af-88e1-8b97951ca1c2


[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-luis-repo-note.md)]

## <a name="get-luis-key"></a>LUIS anahtarını alma

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-get-key-para.md)]

## <a name="get-intent-with-browser"></a>Amacı tarayıcı ile alma

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-browser-para.md)]

## <a name="get-intent-programmatically"></a>Amacı programlamayla alma

Önceki bölümde tarayıcı penceresinde gördüğünüz sonuçları almak için C# ile tahmin uç noktası GET [API](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee78)'sini kullanın. 

1. Visual Studio’da yeni bir konsol uygulaması oluşturun. 

    ![LUIS kullanıcı ayarları menüsüne erişime](media/luis-get-started-cs-get-intent/visual-studio-console-app.png)

2. Visual Studio projesinde Çözüm Gezgini'nde **Başvuru ekle**'yi ve ardından Derlemeler sekmesinden **System.Web**'i seçin.

    ![LUIS kullanıcı ayarları menüsüne erişime](media/luis-get-started-cs-get-intent/add-system-dot-web-to-project.png)

3. Program.cs içeriğini şu kodla değiştirin:
    
   [!code-csharp[Console app code that calls a LUIS endpoint](~/samples-luis/documentation-samples/quickstarts/analyze-text/csharp/Program.cs)]

4. `YOUR_KEY` değerini LUIS anahtarınızla değiştirin.

5. Konsol uygulamasını derleyin ve çalıştırın. Tarayıcı penceresinde daha önce gördüğünüz JSON kodunu görüntüler.

    ![Konsol penceresi, LUIS uygulamasından gelen JSON sonucunu görüntüler](./media/luis-get-started-cs-get-intent/console-turn-on.png)

## <a name="luis-keys"></a>LUIS anahtarları

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-key-usage-para.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu hızlı başlangıcı tamamladıktan sonra Visual Studio projesini kapatın ve proje dizinini dosya sisteminden kaldırın. 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Konuşma ekleme ve C# ile eğitme](luis-get-started-cs-add-utterance.md)