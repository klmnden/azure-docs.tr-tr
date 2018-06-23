---
title: İngilizce olmayan Bilgi Bankası - QnA Maker - Azure Bilişsel hizmetler oluşturma | Microsoft Docs
description: İngilizce olmayan Bilgi Bankası oluşturma
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 05/07/2018
ms.author: saneppal
ms.openlocfilehash: 3fbd590229044af0daa60968fd8d556d539a58c9
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354052"
---
# <a name="language-support-of-knowledge-base-content-for-qna-maker"></a>QnA Maker için Bilgi Bankası içeriğinin dil desteği
QnA Maker birçok dilde Bilgi Bankası içeriği destekler. Ancak, her QnA Maker hizmet için tek bir dil Ayrılmış olması. Belirli bir QnA Maker hizmet hedefleme oluşturulan ilk Bilgi Bankası hizmetin dili ayarlar. Bkz: [burada](../Overview/languages-supported.md) desteklenen dillerin listesi.

Dil ayıklanırken veri kaynakları içerikten otomatik olarak tanınır. Bu hizmetinde yeni bir QnA Maker hizmeti ve bir yeni Bilgi Bankası oluşturduktan sonra dil doğru şekilde ayarlandığını doğrulayabilirsiniz.

1. Gidin [Azure Portal](https://portal.azure.com/).

2. Seçin **kaynak grupları** ve dağıtılan ve select QnA Maker hizmet olduğu kaynak grubuna gidin **Azure Search** kaynak.

    ![Azure Search kaynak seçin](../media/qnamaker-how-to-language-kb/select-azsearch.png)

3. Seçin **testkb** dizini. Bu Azure Search dizini her zaman oluşturulan ilk hangisi olduğunu ve bu hizmetindeki tüm Bilgi Bankası kaydedilmiş içeriğini içerir. 

    ![KB Test seçin](../media/qnamaker-how-to-language-kb/select-testkb.png)

4. Seçin **alanları** testkb ayrıntıları gösteren bölüm.

    ![Alanları Seç](../media/qnamaker-how-to-language-kb/selectfields.png)

5. Onay kutusunu için **Çözümleyicisi** dil ayrıntılarını görmek için.

    ![Çözümleyici seçin](../media/qnamaker-how-to-language-kb/select-analyzer.png)

6. Belirli bir dil Çözümleyicisi ayarlandığını bulmanız gerekir. Bu dil otomatik olarak Bilgi Bankası oluşturma adımında algılandı. Bu dil kaynağı oluşturulduktan sonra değiştirilemez.

    ![Seçili Çözümleyicisi](../media/qnamaker-how-to-language-kb/selected-analyzer.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Bot hizmetiyle QnA bot oluşturma](../Tutorials/create-qna-bot.md)
