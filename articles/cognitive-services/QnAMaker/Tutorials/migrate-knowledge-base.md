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
ms.openlocfilehash: 8ff3c497372a761bd8a02ae81bc897c8ee297bd0
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65794878"
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
Büyük küçük harf duyarsız değişiklikleri (eş anlamlılar) otomatik olarak içeri aktarılmaz. Kullanım [V4 API'leri](https://go.microsoft.com/fwlink/?linkid=2092179) değişiklikleri yeni Bilgi Bankası'ndaki taşınır.

Sohbet günlükleri, yeni Bilgi Bankası sohbet günlükleri depolamak için Application Insights kullandığından geçirmek için hiçbir yolu yoktur. 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi Bankası Düzenle](../How-To/edit-knowledge-base.md)
