---
title: Bilgi bankaları - soru-cevap Oluşturucu geçirme
titleSuffix: Azure Cognitive Services
description: Bilgi Bankası geçiş, bir Bilgi Bankası dışarı aktarma ve ardından başka bir içeri aktarma gerektirir.
services: cognitive-services
author: tulasim88
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 04/08/2019
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: eac5e43c69cc09c5945316827a35f729c158d47a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61431251"
---
# <a name="migrate-a-knowledge-base-using-export-import"></a>İçeri dışarı aktarma kullanarak Bilgi Bankası geçirme

Bilgi Bankası geçiş, bir Bilgi Bankası dışarı aktarma ve ardından başka bir içeri aktarma gerektirir. 

## <a name="prerequisites"></a>Önkoşullar

* Oluşturma bir [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) başlamadan önce.
* Yeni bir [soru-cevap Oluşturucu hizmeti](../How-To/set-up-qnamaker-service-azure.md)

## <a name="migrate-a-knowledge-base-from-qna-maker"></a>Bilgi Bankası soru-cevap Oluşturucu ' geçiş
1. Oturum [soru-cevap Oluşturucu portalı](https://qnamaker.ai).
1. Geçirmek istediğiniz Bilgi Bankası'nı seçin.

1. Üzerinde **ayarları** sayfasında **dışarı aktarma, Bilgi Bankası** , Bilgi Bankası - içeriğini içeren bir .tsv dosyasını indirmek için yanıtları, meta verileri, soru ve veri kaynağı adları, oldukları gelen Ayıklanan.

1. Seçin **Bilgi Bankası oluşturma** üstteki menüden sonra boş bir Bilgi Bankası oluşturun. 

    ![Kümesi veri kaynakları](../media/qnamaker-how-to-create-kb/set-data-sources.png)

    - Hizmetinize vermek bir **adı.** Yinelenen adları desteklenir ve özel karakterler de desteklenir.

1. **Oluştur**’u seçin.

    ![KB oluşturma](../media/qnamaker-how-to-create-kb/create-kb.png)

1. Bu yeni Bilgi Bankası'açın **ayarları** sekmenize **alma Bilgi Bankası**. Bu soruları, yanıtlar ve meta verileri içe aktarır ve kendisinden ayıklandıkları veri kaynağı adlarını korur.

   ![Bilgi Bankası içeri aktarma](../media/qnamaker-how-to-migrate-kb/Import.png)

1. **Test** Test Masası'nı kullanarak yeni Bilgi Bankası. Bilgi edinmek için nasıl [bilgi bankanızı test](../How-To/test-knowledge-base.md).
1. **Yayımlama** Bilgi Bankası. Bilgi edinmek için nasıl [, Bilgi Bankası yayımlama](../Quickstarts/create-publish-knowledge-base.md#publish-the-knowledge-base).
1. Uç nokta uygulaması veya bot kodunuzda kullanın. Burada gördüğünüz nasıl [soru-cevap Robotu oluşturun](../Tutorials/create-qna-bot.md).

    ![Soru-cevap Oluşturucu değerleri](../media/qnamaker-how-to-migrate-kb/qnamaker-settings-kbid-key.png)

    Bu noktada, tüm Bilgi Bankası içerikleri - soruları yanıtlar ve kaynak dosyaları ve URL'leri adlarını yanı sıra meta veri yeni Bilgi Bankası'na aktarılır. 

## <a name="chat-logs-and-alterations"></a>Sohbet günlükleri ve değişiklikleri
Büyük küçük harf duyarsız değişiklikleri (eş anlamlılar) otomatik olarak içeri aktarılmaz. Kullanım [V2 API](https://aka.ms/qnamaker-v2-apis) eski bilgilerden değişiklikleri aktarmak ve [V4 API'leri](https://aka.ms/qnamaker-v4-apis) değişiklikleri yeni Bilgi Bankası'ndaki taşımak için.

Sohbet günlükleri, yeni Bilgi Bankası sohbet günlükleri depolamak için Application Insights kullandığından geçirmek için hiçbir yolu yoktur. 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi Bankası Düzenle](../How-To/edit-knowledge-base.md)
