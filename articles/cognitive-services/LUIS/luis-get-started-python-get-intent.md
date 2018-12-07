---
title: 'Hızlı Başlangıç: amaca dönüştürme-Python Al'
titleSuffix: Language Understanding - Azure Cognitive Services
description: Bu hızlı başlangıçta, amaç ve varlıkları döndürmek için bir LUIS uç noktasına konuşma iletin.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 6121448906960ee3da41a16eac974f15ebdb46b5
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53015526"
---
# <a name="quickstart-get-intent-using-python"></a>Hızlı Başlangıç: Python kullanarak amacı alma
Bu hızlı başlangıçta, amaç ve varlıkları döndürmek için bir LUIS uç noktasına konuşma iletin.

[!INCLUDE [Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

## <a name="prerequisites"></a>Önkoşullar

* [Python 3.6](https://www.python.org/downloads/) veya üzeri.
* [Visual Studio Code](https://code.visualstudio.com/)

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-luis-repo-note.md)]

## <a name="get-luis-key"></a>LUIS anahtarını alma

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-get-key-para.md)]

## <a name="get-intent-with-browser"></a>Amacı tarayıcı ile alma

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-browser-para.md)]

## <a name="get-intent--programmatically"></a>Amacı programlamayla alma

Python kullanarak bir önceki adımda tarayıcı penceresinde gördüğünüz sonuçlara ulaşabilirsiniz.

1. Aşağıdaki kod parçacıklarından birini `quickstart-call-endpoint.py` adlı bir dosyaya kopyalayın:

   [!code-python[Console app code that calls a LUIS endpoint for Python 2.7](~/samples-luis/documentation-samples/quickstarts/analyze-text/python/2.x/quickstart-call-endpoint-2-7.py)]

   [!code-python[Console app code that calls a LUIS endpoint for Python 3.6](~/samples-luis/documentation-samples/quickstarts/analyze-text/python/3.x/quickstart-call-endpoint-3-6.py)]

2. `Ocp-Apim-Subscription-Key` alanının değerini LUIS uç nokta anahtarınızla değiştirin.

3. `pip install requests` ile bağımlılıkları yükleyin.

4. `python ./quickstart-call-endpoint.py` ile betiği çalıştırın. Tarayıcı penceresinde daha önce gördüğünüz JSON kodunu görüntüler.

## <a name="luis-keys"></a>LUIS anahtarları

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-key-usage-para.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme
Python dosyasını silin. 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Konuşma ekleme](luis-get-started-python-add-utterance.md)
