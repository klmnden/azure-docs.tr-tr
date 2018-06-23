---
title: Anahtar Yönetimi - Microsoft Bilişsel hizmetler | Microsoft Docs
titleSuffix: Azure
description: QnA Maker tuşlarınızı yönetme
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 05/07/2018
ms.author: saneppal
ms.openlocfilehash: b402187f4949dac34fa476648c81b980ba3efc96
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354077"
---
# <a name="key-management"></a>Anahtar Yönetimi

Anahtarlar, iki tür QnA Maker hizmetinizi ilgilenir **Abonelik anahtarları** ve **endpoint anahtarları**.

![Anahtar Yönetimi](../media/qnamaker-how-to-key-management/key-management.png)

1. **Abonelik anahtarları**: Bu anahtarları kullanılan erişimi [QnA Maker yönetim hizmeti API](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff). Bu API'leri, Bilgi Bankası çeşitli CRUD işlemleri gerçekleştirmenize olanak tanır.  

2. **Uç nokta anahtarları**: Bu anahtarlar için bir kullanıcı soru yanıt almak için Bilgi Bankası uç noktasına erişmek için kullanılır. Genellikle QnA Maker hizmet tüketen sohbet bot/uygulama kodunuzda bu uç noktaya kullanırsınız.
 
## <a name="subscription-keys"></a>Abonelik anahtarları
Görüntülemek ve abonelik anahtarlarınızı QnA Maker kaynak oluşturulduğu Azure portalından sıfırlayın. 
1. Azure portalında QnA Maker kaynağa gidin.

    ![QnA Maker kaynak listesi](../media/qnamaker-how-to-key-management/qnamaker-resource-list.png)

2. Git **anahtarları**.

    ![Abonelik anahtarı](../media/qnamaker-how-to-key-management/subscription-key.PNG)

## <a name="endpoint-keys"></a>Uç nokta anahtarları

Uç nokta anahtarları, gelen yönetilebilir [QnA Maker portal](https://qnamaker.ai).

1. Oturum [QnA Maker portal](https://qnamaker.ai)ve Git **anahtarları Yönet**.

    ![uç noktası anahtarı](../media/qnamaker-how-to-key-management/Endpoint-keys.png)

2. Görüntülemek veya anahtarlarınızı sıfırlayın.

    ![uç nokta Anahtar Yöneticisi](../media/qnamaker-how-to-key-management/Endpoint-keys1.png)

    >[!NOTE]
    >Tehlikede olduğunu düşünüyorsanız, anahtarlarınızı yenileyin. Bu, uygulama/Bot koduna karşılık gelen değişiklikler gerektirebilir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Farklı bir dilde Bilgi Bankası oluşturun](./language-knowledge-base.md)
