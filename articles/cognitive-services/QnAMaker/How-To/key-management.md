---
title: Kaynak ve anahtar yönetimi - soru-cevap Oluşturucu
titleSuffix: Azure Cognitive Services
description: Soru-cevap Oluşturucu hizmetinizi iki tür anahtarlar, Abonelik anahtarları ve uç nokta anahtarlar ilgilenir.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 03/04/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: b9be1db9be1d4dd57994e101c07ed430425a5912
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67447432"
---
# <a name="how-to-manage-keys-in-qna-maker"></a>Soru-cevap Oluşturucu anahtarlarını yönetme

Soru-cevap Oluşturucu hizmetinizi anahtarları, iki tür ile ilgilenen **Abonelik anahtarları** ve **uç nokta anahtarları**.

![Anahtar Yönetimi](../media/qnamaker-how-to-key-management/key-management.png)

1. **Abonelik anahtarları**: Bu anahtarlar kullanılır erişimi [soru-cevap Oluşturucu yönetim hizmeti API'leri](https://go.microsoft.com/fwlink/?linkid=2092179). Bu API'ler gerçekleştirmenize olanak tanır, Bilgi Bankası'nı düzenleyin.  

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
