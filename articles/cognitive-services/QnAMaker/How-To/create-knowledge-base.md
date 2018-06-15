---
title: Bilgi Bankası - QnA Maker - Azure Bilişsel hizmetler oluşturma | Microsoft Docs
description: Bilgi Bankası oluşturma
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 05/07/2018
ms.author: saneppal
ms.openlocfilehash: 93b64948ecc52feeb0f862f2b76ea898dce2333a
ms.sourcegitcommit: 680964b75f7fff2f0517b7a0d43e01a9ee3da445
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "35355938"
---
# <a name="create-a-knowledge-base"></a>Bilgi bankası oluşturma

QnA Maker çok basit için yerleşik bir Bilgi Bankası oluşturmak için mevcut veri kaynağı kolaylaştırır. Yeni QnA Maker Bilgi Bankası SSS sayfaları, ürün kılavuzları, yapılandırılmış belgeleri oluşturabilir veya bunları düzenleme şeklinde ekleyebilirsiniz.

## <a name="steps"></a>Adımlar

1. Başlamak için içine oturum [QnA Maker portal](https://qnamaker.ai) 'ı tıklatın ve azure kimlik ile **yeni hizmet oluşturma**.

    ![KB oluşturma ](../media/qnamaker-how-to-create-kb/create-new-service.png)

2. QnA Maker hizmeti zaten oluşturmadıysanız seçin **QnA hizmet oluşturma**. Aksi takdirde, adım 2'deki aşağı açılan listeler QnA Maker hizmeti seçin. Bilgi Bankası barındıracak QnA Maker hizmeti seçin.

    ![Kurulum QnA hizmeti](../media/qnamaker-how-to-create-kb/setup-qna-resource.png)

3. Bilgi Bankası oluşturmak için aşağıdaki bilgileri girin.

    ![Set veri kaynakları](../media/qnamaker-how-to-create-kb/set-data-sources.png)

    - Hizmetinizi vermek bir **adı.** Yinelenen adları desteklenir ve özel karakterler de desteklenir.
    - Gelen ayıklanan URL'leri yapıştırın. Desteklenen kaynaklardan türleri üzerinde daha fazla bilgi [burada](../Concepts/data-sources-supported.md).
    - Alternatif olarak, veri ayıklandığı dosyaları karşıya yükleme. Bkz: [fiyatlandırma bilgileri](https://aka.ms/qnamaker-pricing
) kaç belgeleri görmek için ekleyebilirsiniz.
    - El ile QnAs eklemek istiyorsanız, dosyaları bağlantılandırma atlayabilirsiniz.

4. **Oluştur**’u seçin.

    ![KB oluşturma](../media/qnamaker-how-to-create-kb/create-kb.png)

5. Ayıklanacak veriler için birkaç dakika sürer.

    ![Ayıklama](../media/qnamaker-how-to-create-kb/hang-tight-extraction.png)

6. Bilgi Bankası başarıyla oluşturulduysa, yönlendirilecek **Bilgi Bankası** sayfası.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi Bankası içeri aktarma](../Tutorials/migrate-knowledge-base.md)
