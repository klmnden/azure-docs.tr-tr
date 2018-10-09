---
title: Önizleme bilgi bankalarından - soru-cevap Oluşturucu geçirme
titleSuffix: Azure Cognitive Services
description: Bilgi Bankası içeri aktarma
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: tulasim
ms.openlocfilehash: a06a04ba992c8d7e9691e4838d38faaafd48de7a
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2018
ms.locfileid: "48853921"
---
# <a name="migrate-a-knowledge-base-using-export-import"></a>İçeri dışarı aktarma kullanarak Bilgi Bankası geçirme
Soru-cevap Oluşturucu 7 Mayıs 2018'den genel kullanılabilirliğini duyurduk \\\build\ konferans. Soru-cevap Oluşturucu GA, Azure'u temel alan yeni bir mimariye sahiptir. Soru-cevap Oluşturucu ücretsiz önizleme ile oluşturulan bilgi bankaları için soru-cevap Oluşturucu büyüyecek geçirilmesi gerekir Soru-cevap Oluşturucu Önizleme Kasım 2018'de kullanımdan kaldırılacaktır. Soru-cevap Oluşturucu GA değişiklikler hakkında daha fazla bilgi için bkz: soru-cevap Oluşturucu GA duyurusundan [blog gönderisi](https://aka.ms/qnamakerga-blog).

Soru-cevap Oluşturucu artık sahip bir [fiyatlandırma modeli](https://azure.microsoft.com/en-us/pricing/details/cognitive-services/qna-maker/).

Önkoşullar
> [!div class="checklist"]
> * Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.
> * Yeni bir kurulum [soru-cevap Oluşturucu hizmeti](../How-To/set-up-qnamaker-service-azure.md)

## <a name="migrate-a-knowledge-base-from-qna-maker-preview-portal"></a>Bilgi Bankası soru-cevap Oluşturucu Önizleme portalından geçirme
1. Gidin [soru-cevap Oluşturucu Önizleme portalı](https://aka.ms/qnamaker-old-portal
) tıklayın **Hizmetlerim**.
2. Düzenleme simgesine tıklayarak geçirmek istediğiniz Bilgi Bankası'nı seçin.

    ![Bilgi Bankası Düzenle](../media/qnamaker-how-to-migrate-kb/preview-editkb.png)

3. Tıklayarak **indirme Bilgi Bankası** , Bilgi Bankası - içeriğini içeren bir .tsv dosyasını indirmek için yanıtları, meta verileri, soru ve veri kaynağı adları, ayıklandıkları öğesinden.

    ![Bilgi Bankası'nı indirin](../media/qnamaker-how-to-migrate-kb/preview-download.png)

4. İçine oturum [soru-cevap Oluşturucu portalı](https://qnamaker.ai) tıklayın ve azure kimlik bilgileri ile **Bilgi Bankası oluşturma**.
    
5. Soru-cevap Oluşturucu hizmetini zaten oluşturmadıysanız seçin **soru-cevap hizmeti oluşturma**. Aksi takdirde, 2. adım açılan menülerde soru-cevap Oluşturucu hizmetini seçin. Bilgi Bankası barındıracak soru-cevap Oluşturucu hizmeti seçin.

    ![Soru-cevap hizmetini ayarlama](../media/qnamaker-how-to-create-kb/setup-qna-resource.png)

6. Boş bir Bilgi Bankası oluşturma 

    ![Kümesi veri kaynakları](../media/qnamaker-how-to-create-kb/set-data-sources.png)

    - Hizmetinize vermek bir **adı.** Yinelenen adları desteklenir ve özel karakterler de desteklenir.
    - Önizleme bankanızı verilerini kullanmak istediğiniz dosyalar veya URL'ler karşıya yüklemeyi atlayın. Şimdilik, boş bir Bilgi Bankası oluşturacaksınız.

7. **Oluştur**’u seçin.

    ![KB oluşturma](../media/qnamaker-how-to-create-kb/create-kb.png)

8. Bu yeni Bilgi Bankası'açın **ayarları** sekmesini ve tıklayarak **alma Bilgi Bankası**. Bu soruları, yanıtlar ve meta verileri içe aktarır ve kendisinden ayıklandıkları veri kaynağı adlarını korur.

   ![Bilgi Bankası içeri aktarma](../media/qnamaker-how-to-migrate-kb/Import.png)

9. **Test** Test Masası'nı kullanarak yeni Bilgi Bankası. Bilgi edinmek için nasıl [bilgi bankanızı test](../How-To/test-knowledge-base.md).
10. **Yayımlama** Bilgi Bankası. Bilgi edinmek için nasıl [, Bilgi Bankası yayımlama](../How-To/publish-knowledge-base.md).
11. Uygulama veya bot kodunuzda aşağıdaki uç noktayı kullanın. Burada gördüğünüz nasıl [soru-cevap Robotu oluşturun](../Tutorials/create-qna-bot.md).

    ![Soru-cevap Oluşturucu değerleri](../media/qnamaker-tutorials-create-bot/qnamaker-settings-kbid-key.PNG)

Bu noktada, tüm Bilgi Bankası içerikleri - soruları yanıtlar ve kaynak dosyaları ve URL'leri adlarını yanı sıra meta veri yeni Bilgi Bankası'na aktarılır. 

## <a name="chatlogs-and-alterations"></a>Chatlogs ve değişiklikleri
Değişiklikleri (eş anlamlılar) otomatik olarak içeri aktarılmaz. Kullanım [V2 API'leri](https://aka.ms/qnamaker-v2-apis) değişiklikleri Önizleme yığından dışarı aktarmak için ve [V4 API'leri](https://aka.ms/qnamaker-v4-apis) yeni yığına değiştirilecek.

Yeni yığın chatlogs depolamak için Application Insights kullandığından, chatlogs geçirmek için hiçbir yolu yoktur. Ancak chatlogs dan indirebileceğiniz [Önizleme portalı](https://aka.ms/qnamaker-old-portal).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi Bankası Düzenle](../How-To/edit-knowledge-base.md)
