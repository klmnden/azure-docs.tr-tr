---
title: Kaynak ve anahtar yönetimi - soru-cevap Oluşturucu
titleSuffix: Azure Cognitive Services
description: Soru-cevap Oluşturucu hizmetinizi iki tür anahtarlar, Abonelik anahtarları ve uç nokta anahtarlar ilgilenir.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 11/26/2018
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: 25b23d280aca9ef13b8820596686a1f9dbecd2a1
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53085952"
---
# <a name="how-to-manage-keys-in-qna-maker"></a>Soru-cevap Oluşturucu anahtarlarını yönetme

Soru-cevap Oluşturucu hizmetinizi anahtarları, iki tür ile ilgilenen **Abonelik anahtarları** ve **uç nokta anahtarları**.

![Anahtar Yönetimi](../media/qnamaker-how-to-key-management/key-management.png)

1. **Abonelik anahtarları**: Bu anahtarlar kullanılır erişimi [soru-cevap Oluşturucu yönetim hizmeti API'leri](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff). Bu API'ler, Bilgi Bankası çeşitli CRUD işlemleri gerçekleştirmenize olanak tanır.  

2. **Uç nokta anahtarları**: Bu anahtarları bir kullanıcı soru için yanıt almak için Bilgi Bankası uç noktasına erişmek için kullanılır. Genellikle, soru-cevap Oluşturucu hizmeti, sohbet botu/uygulama kodunuzda bu uç nokta kullanmanız gerekir.
 
## <a name="subscription-keys"></a>Abonelik anahtarları
Görüntüleyebilir ve soru-cevap Oluşturucu kaynağın oluşturulduğu Azure portalından abonelik anahtarlarınızı sıfırlayın. 
1. Azure Portalı'nda soru-cevap Oluşturucu kaynağa gidin.

    ![Soru-cevap Oluşturucu kaynak listesi](../media/qnamaker-how-to-key-management/qnamaker-resource-list.png)

2. Git **anahtarları**.

    ![Abonelik anahtarı](../media/qnamaker-how-to-key-management/subscription-key.PNG)

## <a name="endpoint-keys"></a>Uç nokta anahtarları

Uç nokta anahtarları alanından yönetilebilir [soru-cevap Oluşturucu portalı](https://qnamaker.ai).

1. Oturum [soru-cevap Oluşturucu portalı](https://qnamaker.ai)ve Git **anahtarları Yönet**.

    ![Uç noktası anahtarı](../media/qnamaker-how-to-key-management/Endpoint-keys.png)

2. Görüntüleyin ya da anahtarlarınızı sıfırlayın.

    ![uç nokta Anahtar Yöneticisi](../media/qnamaker-how-to-key-management/Endpoint-keys1.png)

    >[!NOTE]
    >Anahtarlarınızı tehlikeye girmiş düşünüyorsanız yenileyin. Bu, ilgili uygulama/Bot kodunuzda değişiklikler gerektirebilir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Farklı bir dilde Bilgi Bankası oluşturma](./language-knowledge-base.md)
