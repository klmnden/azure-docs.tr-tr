---
title: Hedefi Al,C#
titleSuffix: Language Understanding - Azure Cognitive Services
description: Bu C# hızlı başlangıç, bir kullanıcının engellemekse damıtarak konuşma bağlamında kullanılabilen metinden belirlemek için kullanılabilir bir genel LUIS uygulaması kullanın.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: quickstart
ms.date: 01/23/2019
ms.author: diberry
ms.openlocfilehash: 81a05ed3e981c2a35221830a16b5859ee5c1bd4f
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55225525"
---
# <a name="quickstart-get-intent-using-c"></a>Hızlı Başlangıç: Hedefi kullanılarak AlC#

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

    ![Visual Studio'da yeni bir konsol uygulaması oluşturma](media/luis-get-started-cs-get-intent/visual-studio-console-app.png)

2. Visual Studio projesinde Çözüm Gezgini'nde **Başvuru ekle**'yi ve ardından Derlemeler sekmesinden **System.Web**'i seçin.

    ![Başvuru Ekle seçin ve ardından System.Web derlemeleri sekmesinden seçin](media/luis-get-started-cs-get-intent/add-system-dot-web-to-project.png)

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
