---
title: Bilgi Bankası - soru-cevap Oluşturucu Oluşturma
titleSuffix: Azure Cognitive Services
description: Botunuz için Sohbet chit ekleyerek daha damıtarak konuşma bağlamında kullanılabilen ve ilgi çekici kolaylaştırır. Soru-cevap Oluşturucu üst chit-sohbet, önceden doldurulmuş bir dizi, KB kolayca eklemenize olanak sağlar. Bu, botun chit-sohbet için bir başlangıç noktası olabilir ve bunları sıfırdan yazmak maliyeti ve zaman Kaydet.
services: cognitive-services
author: nstulasi
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: saneppal
ms.openlocfilehash: 66705a0afdcdb04fe49bea0bfc03286df3611071
ms.sourcegitcommit: f983187566d165bc8540fdec5650edcc51a6350a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45543367"
---
# <a name="create-a-knowledge-base"></a>Bilgi bankası oluşturma

Soru-cevap Oluşturucu çok ekleme Bilgi Bankası oluşturmak için var olan veri kaynaklarınızı kolaylaştırır. Yeni soru-cevap Oluşturucu Bilgi Bankası, SSS sayfaları, ürünlerini el kitabı, yapılandırılmış belgeler oluşturma veya düzenleme şeklinde ekleyin.

## <a name="steps"></a>Adımlar

1. Başlamak için oturum [soru-cevap Oluşturucu portalı](https://qnamaker.ai) tıklayın ve azure kimlik bilgileri ile **yeni hizmet oluşturma**.

    ![KB oluşturma ](../media/qnamaker-how-to-create-kb/create-new-service.png)

2. Soru-cevap Oluşturucu hizmetini zaten oluşturmadıysanız seçin **soru-cevap hizmeti oluşturma**. Aksi takdirde, 2. adım açılan menülerde soru-cevap Oluşturucu hizmetini seçin. Bilgi Bankası barındıracak soru-cevap Oluşturucu hizmeti seçin.

    ![Soru-cevap hizmetini ayarlama](../media/qnamaker-how-to-create-kb/setup-qna-resource.png)

3. Bilgi Bankası oluşturmak için aşağıdaki bilgileri girin.

    ![Kümesi veri kaynakları](../media/qnamaker-how-to-create-kb/set-data-sources.png)

    - Hizmetinize vermek bir **adı.** Yinelenen adları desteklenir ve özel karakterler de desteklenir.
    - Gelen ayıklanan URL'ler yapıştırın. Desteklenen kaynakları türleri hakkında daha fazla bilgi [burada](../Concepts/data-sources-supported.md).
    - Alternatif olarak, verilerin ayıklandığı dosyaları karşıya yükleyin. Bkz: [fiyatlandırma bilgileri](https://aka.ms/qnamaker-pricing
) kaç belgeleri görmek için ekleyebilirsiniz.
    - Bankalarıyla el ile eklemek istiyorsanız, dosyaları bağlantılandırma atlayabilirsiniz.

4. **Oluştur**’u seçin.

    ![KB oluşturma](../media/qnamaker-how-to-create-kb/create-kb.png)

5. Ayıklanacak veri birkaç dakika sürer.

    ![Ayıklama](../media/qnamaker-how-to-create-kb/hang-tight-extraction.png)

6. Bilgi bankanızı başarıyla oluşturulduysa, yönlendirilecek **Bilgi Bankası** sayfası.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi Bankası içeri aktarma](../Tutorials/migrate-knowledge-base.md)
