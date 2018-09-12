---
title: Javascript kullanarak Language Understanding’de (LUIS) doğal dil metni analiz etme - Bilişsel Hizmetler - Azure Bilişsel Hizmetler | Microsoft Docs
description: Bu hızlı başlangıçta, kullanılabilir genel bir LUIS uygulamasını kullanarak konuşma metninden bir kullanıcının amacını belirleyin. Javascript kullanarak kullanıcının amacını genel uygulamanın HTTP tahmin uç noktasına metin olarak gönderin. Uç noktada, LUIS genel uygulamanın modelini uygulayarak anlam oluşturmak için doğal dil metnini analiz eder, böylece genel amacı belirler ve uygulamanın konu etki alanıyla ilgili verileri ayıklar.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 08/23/2018
ms.author: diberry
ms.openlocfilehash: 10210c3759611a77c4430a97896a19a6b97b9fa9
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/24/2018
ms.locfileid: "43772548"
---
# <a name="quickstart-analyze-text-using-javascript"></a>Hızlı Başlangıç: Javascript kullanarak metin analiz etme

[!include[Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

<a name="create-luis-subscription-key"></a>

[!include[Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-luis-repo-note.md)]

## <a name="prerequisites"></a>Ön koşullar

* [Visual Studio Code](https://code.visualstudio.com/)
* Genel uygulama kimliği: df67dcdb-c37d-46af-88e1-8b97951ca1c2


## <a name="get-luis-key"></a>LUIS anahtarını alma

[!include[Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-get-key-para.md)]

## <a name="analyze-text-with-browser"></a>Tarayıcı ile metin analiz etme

[!include[Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-browser-para.md)]

## <a name="analyze-text-with-javascript"></a>JavaScript ile metin analiz etme 

JavaScript kullanarak bir önceki adımda tarayıcı penceresinde gördüğünüz sonuçlara ulaşabilirsiniz. 

1. Aşağıdaki kodu kopyalayıp bir HTML dosyasına kaydedin:

   [!code-html[Console app code that calls a LUIS endpoint](~/samples-luis/documentation-samples/quickstarts/analyze-text/javascript/call-endpoint.html)]

2. Dosyayı herhangi bir tarayıcıda açın. LUIS uç noktası anahtarınızı forma girin ve **Gönder**’i seçin.

    ![Tarayıcıda görüntülenen Ev Otomasyonu uygulamasına ait LUIS sonuçlarını içeren HTML örneği](./media/luis-get-started-js-get-intent/html-results.png)

    Sonuç formun altında görüntülenir. 

## <a name="luis-keys"></a>LUIS anahtarları

[!include[Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-key-usage-para.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

Javascript dosyasını silin.

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Konuşma ekleme](luis-get-started-javascript-add-utterance.md)
