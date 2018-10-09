---
title: PHP Hızlı Başlangıç - amacı tahmin etme - LUIS
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, kullanılabilir genel bir LUIS uygulamasını kullanarak konuşma metninden bir kullanıcının amacını belirleyin. PHP kullanarak kullanıcının amacını genel uygulamanın HTTP tahmin uç noktasına metin olarak gönderin. Uç noktada, LUIS genel uygulamanın modelini uygulayarak anlam oluşturmak için doğal dil metnini analiz eder, böylece genel amacı belirler ve uygulamanın konu etki alanıyla ilgili verileri ayıklar.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 87c37de611056c5672ed0910afdd344325a0c6f7
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47036904"
---
# <a name="quickstart-get-intent-using-php"></a>Hızlı Başlangıç: PHP kullanarak amacı alma

[!INCLUDE [Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

<a name="create-luis-subscription-key"></a>

## <a name="prerequisites"></a>Ön koşullar

* [PHP](http://php.net/) programlama dili
* [Visual Studio Code](https://code.visualstudio.com/)
* Genel uygulama kimliği: df67dcdb-c37d-46af-88e1-8b97951ca1c2


[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-luis-repo-note.md)]

## <a name="get-luis-key"></a>LUIS anahtarını alma

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-get-key-para.md)]

## <a name="get-intent-with-browser"></a>Amacı tarayıcı ile alma

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-browser-para.md)]

## <a name="get-intent-programmatically"></a>Amacı programlamayla alma 

PHP kullanarak bir önceki adımda tarayıcı penceresinde gördüğünüz sonuçlara ulaşabilirsiniz. 

1. Aşağıdaki kodu kopyalayıp `endpoint-call.php` dosya adıyla kaydedin:

   [!code-php[PHP code that calls a LUIS endpoint](~/samples-luis/documentation-samples/quickstarts/analyze-text/php/endpoint-call.php)]

2. `"YOUR-KEY"` öğesini uç nokta anahtarınızla değiştirin.

3. `php endpoint-call.php` ile PHP uygulamasını çalıştırın. Tarayıcı penceresinde daha önce gördüğünüz JSON kodunu görüntüler.

## <a name="luis-keys"></a>LUIS anahtarları

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-key-usage-para.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

PHP dosyasını silin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Konuşma ekleme](luis-get-started-php-add-utterance.md)