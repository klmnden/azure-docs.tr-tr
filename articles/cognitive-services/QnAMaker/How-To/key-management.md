---
title: Kaynak ve anahtar yönetimi - soru-cevap Oluşturucu
titleSuffix: Azure Cognitive Services
description: Soru-cevap Oluşturucu hizmetinizi iki tür anahtarlar, Abonelik anahtarları ve uç nokta anahtarlar ilgilenir.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 11/26/2018
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: 0d245215a61a526b448dd0aaafd6429b90b12b74
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55216139"
---
# <a name="how-to-manage-keys-in-qna-maker"></a>Soru-cevap Oluşturucu anahtarlarını yönetme

Soru-cevap Oluşturucu hizmetinizi anahtarları, iki tür ile ilgilenen **Abonelik anahtarları** ve **uç nokta anahtarları**.

![Anahtar Yönetimi](../media/qnamaker-how-to-key-management/key-management.png)

1. **Abonelik anahtarları**: Bu anahtarlar kullanılır erişimi [soru-cevap Oluşturucu yönetim hizmeti API'leri](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff). Bu API'ler gerçekleştirmenize olanak tanır, Bilgi Bankası'nı düzenleyin.  

2. **Uç nokta anahtarları**: Bu anahtarlar, kullanıcı soru için yanıt almak için Bilgi Bankası uç noktasına erişmek için kullanılır. Genellikle, bu uç nokta sohbet Robotu veya soru-cevap Oluşturucu hizmeti istemci uygulaması kodu kullanmanız gerekir.
 
## <a name="subscription-keys"></a>Abonelik anahtarları
Görüntüleyebilir ve soru-cevap Oluşturucu kaynağın oluşturulduğu Azure portalından abonelik anahtarlarınızı sıfırlayın. 
1. Azure Portalı'nda soru-cevap Oluşturucu kaynağa gidin.

    ![Soru-cevap Oluşturucu kaynak listesi](../media/qnamaker-how-to-key-management/qnamaker-resource-list.png)

2. Git **anahtarları**.

    ![Abonelik anahtarı](../media/qnamaker-how-to-key-management/subscription-key.PNG)

## <a name="endpoint-keys"></a>Uç nokta anahtarları

Uç nokta anahtarları alanından yönetilebilir [soru-cevap Oluşturucu portalı](https://qnamaker.ai).

1. Oturum [soru-cevap Oluşturucu portalı](https://qnamaker.ai)profilinize gidin ve ardından **hizmet ayarları**.

    ![Uç noktası anahtarı](../media/qnamaker-how-to-key-management/Endpoint-keys.png)

2. Görüntüleyin ya da anahtarlarınızı sıfırlayın.

    ![uç nokta Anahtar Yöneticisi](../media/qnamaker-how-to-key-management/Endpoint-keys1.png)

    >[!NOTE]
    >Anahtarlarınızı tehlikeye girmiş düşünüyorsanız yenileyin. Bu, istemci uygulamanız ya da bot kod karşılık gelen değişiklikler gerektirebilir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Farklı bir dilde Bilgi Bankası oluşturma](./language-knowledge-base.md)
