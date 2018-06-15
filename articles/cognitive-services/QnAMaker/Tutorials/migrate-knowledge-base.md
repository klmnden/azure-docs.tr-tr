---
title: Bilgi Bankası - Microsoft Bilişsel hizmetler içeri aktarma | Microsoft Docs
titleSuffix: Azure
description: Bilgi Bankası içeri aktarma
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 04/21/2018
ms.author: saneppal
ms.openlocfilehash: ce8f98f9bdb37d5f326e942fe5b5e815e5272c56
ms.sourcegitcommit: 680964b75f7fff2f0517b7a0d43e01a9ee3da445
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "35355941"
---
# <a name="migrate-a-knowledge-base-using-export-import"></a>Bir Bilgi Bankası dışa-içe aktarma kullanarak geçirme
QnA Maker 7 Mayıs 2018 üzerinde genel kullanılabilirlik duyurdu \\\build\ konferans. QnA Maker GA Azure üzerinde oluşturulmuş yeni bir mimariye sahip. Bilgi Bankası QnA Maker ücretsiz önizleme ile oluşturulan QnA Maker İST geçirilmesi gerekir QnA Maker Önizleme Kasım 2018 kullanım dışı kalacaktır. QnA Maker GA duyuru QnA Maker GA yapılan değişiklikler hakkında daha fazla bilgi için bkz: [blog gönderisi](https://aka.ms/qnamakerga-blog).

QnA Maker şimdi sahip bir [fiyatlandırma modeli](https://azure.microsoft.com/en-us/pricing/details/cognitive-services/qna-maker/).

Önkoşullar
> [!div class="checklist"]
> * Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.
> * Yeni bir kurulum [QnA Maker hizmeti](../How-To/set-up-qnamaker-service-azure.md)

## <a name="migrate-a-knowledge-base-from-qna-maker-preview-portal"></a>Bilgi Bankası QnA Maker Önizleme Portalı'ndan geçirme
1. Gidin [QnA Maker Önizleme portalı](https://aka.ms/qnamaker-old-portal
) ve tıklayın **Hizmetlerim**.
2. Bilgi Bankası'nın düzenleme simgesine tıklayarak geçirmek istediğiniz seçin.

    ![Bilgi Bankası Düzenle](../media/qnamaker-how-to-migrate-kb/preview-editkb.png)

3. Tıklayın **karşıdan Bilgi Bankası** , Bilgi Bankası - içeriğini içeren bir .tsv dosyasını karşıdan yüklemek için yanıtları, meta verileri, soru ve veri kaynağı adları bunlar ayıkladığınız gelen.

    ![Bilgi Bankası indirin](../media/qnamaker-how-to-migrate-kb/preview-download.png)

4. İçine oturum [QnA Maker portal](https://qnamaker.ai) 'ı tıklatın ve azure kimlik bilgileri ile **yeni hizmet oluşturma**.

    ![KB oluşturma ](../media/qnamaker-how-to-create-kb/create-new-service.png)
    
5. QnA Maker hizmeti zaten oluşturmadıysanız seçin **QnA hizmet oluşturma**. Aksi takdirde, adım 2'deki aşağı açılan listeler QnA Maker hizmeti seçin. Bilgi Bankası barındıracak QnA Maker hizmeti seçin.

    ![Kurulum QnA hizmeti](../media/qnamaker-how-to-create-kb/setup-qna-resource.png)

6. Boş bir Bilgi Bankası oluşturun 

    ![Set veri kaynakları](../media/qnamaker-how-to-create-kb/set-data-sources.png)

    - Hizmetinizi vermek bir **adı.** Yinelenen adları desteklenir ve özel karakterler de desteklenir.
    - Önizleme Bilgi Bankası verilerden kullanmak istediğiniz gibi karşıya yükleme dosyaları veya URL'leri atlayın. Şimdilik, boş bir Bilgi Bankası oluşturur.

7. **Oluştur**’u seçin.

    ![KB oluşturma](../media/qnamaker-how-to-create-kb/create-kb.png)

8. Bu yeni Bilgi Bankası'nda açmak **ayarları** sekmesinde ve tıklayın **alma Bilgi Bankası**. Bu sorular, yanıtlar ve meta verileri içe aktarır ve bunlar ayıklandığı veri kaynağı adları korur.

   ![Bilgi Bankası içeri aktarma](../media/qnamaker-how-to-migrate-kb/Import.png)

9. **Test** Test Masası'nı kullanarak yeni Bilgi Bankası. Bilgi edinmek için nasıl [, Bilgi Bankası test](../How-To/test-knowledge-base.md).
10. **Yayımlama** Bilgi Bankası. Bilgi edinmek için nasıl [, Bilgi Bankası yayımlama](../How-To/publish-knowledge-base.md).
11. Uç nokta aşağıda uygulama veya bot kodunuzda kullanabilirsiniz. Burada gördüğünüz nasıl [QnA bot oluşturma](../Tutorials/create-qna-bot.md).

    ![QnA Maker değerlerin](../media/qnamaker-tutorials-create-bot/qnamaker-settings-kbid-key.PNG)

Bu noktada, tüm Bilgi Bankası içeriği - sorular, yanıtlar ve meta veri, kaynak dosyaları ve URL'leri adları ile birlikte yeni Bilgi Bankası'na aktarılır. 

## <a name="chatlogs-and-alterations"></a>Chatlogs ve değişiklikleri
Değişiklikleri (anlamlıları) otomatik olarak içeri aktarılmaz. Kullanım [V2 API'leri](https://aka.ms/qnamaker-v2-apis) değişiklikleri Önizleme yığınından dışarı aktarmak için ve [V4 API'leri](https://aka.ms/qnamaker-v4-apis) yeni yığınında değiştirmek için.

Yeni yığını chatlogs depolamak için Application Insights kullandığından chatlogs, geçirmek için bir yolu yoktur. Gelen chatlogs ancak indirebilirsiniz [Önizleme portalı](https://aka.ms/qnamaker-old-portal).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi Bankası Düzenle](../How-To/edit-knowledge-base.md)
