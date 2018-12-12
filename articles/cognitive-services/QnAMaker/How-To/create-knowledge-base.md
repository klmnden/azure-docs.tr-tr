---
title: Bilgi Bankası - soru-cevap Oluşturucu Oluşturma
titleSuffix: Azure Cognitive Services
description: Botunuz için Sohbet chit ekleyerek daha damıtarak konuşma bağlamında kullanılabilen ve ilgi çekici kolaylaştırır. Soru-cevap Oluşturucu üst chit-sohbet, önceden doldurulmuş bir dizi, KB kolayca eklemenize olanak sağlar. Bu, botun chit-sohbet için bir başlangıç noktası olabilir ve bunları sıfırdan yazmak maliyeti ve zaman Kaydet.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: 059e959aec34503f31cbf87266d0633865cbff46
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53098755"
---
# <a name="create-a-knowledge-base-in-qna-maker"></a>Soru-cevap Oluşturucu Bilgi Bankası oluşturma

Soru-cevap Oluşturucu Bilgi Bankası oluştururken, mevcut veri kaynaklarınızı eklemek basit hale getirir. Yeni bir soru-cevap Oluşturucu Bilgi Bankası aşağıdaki belge türlerinden oluşturabilirsiniz:

<!-- added for scanability -->
* SSS sayfaları
* Ürünleri kılavuzları
* Yapılandırılmış belgeleri

## <a name="steps"></a>Adımlar

1. İçine oturum [soru-cevap Oluşturucu portalı](https://qnamaker.ai) seçin ve Azure kimlik bilgileri ile **Bilgi Bankası oluşturma**.

2. Soru-cevap Oluşturucu hizmetini zaten oluşturmadıysanız seçin **soru-cevap hizmeti oluşturma**. 

3. Bir Azure kiracısı, Azure abonelik adı ve listelerden soru-cevap Oluşturucu hizmeti ile ilişkili Azure kaynağı adı seçin **2. adım** soru-cevap Oluşturucu Portalı'nda. Bilgi Bankası barındıracak Azure soru-cevap Oluşturucu hizmetini seçin.

    ![Soru-cevap hizmetini ayarlama](../media/qnamaker-how-to-create-kb/setup-qna-resource.png)

4. Yeni Bilgi Bankası için bilgi bankanızı ve veri kaynakları adını girin.

    ![Kümesi veri kaynakları](../media/qnamaker-how-to-create-kb/set-data-sources.png)

    - Hizmetinize vermek bir **adı.** Yinelenen adları ve özel karakterler desteklenir.
    - URL için ayıklanan istediğiniz verileri ekleyin. Desteklenen kaynakları türleri hakkında daha fazla bilgi [burada](../Concepts/data-sources-supported.md).
    - Ayıklanan istediğiniz veri dosyalarını karşıya yükleyin. Bkz: [fiyatlandırma bilgileri](https://aka.ms/qnamaker-pricing) kaç belgeleri görmek için ekleyebilirsiniz.
    - Bankalarıyla el ile eklemek istiyorsanız, atlayabilirsiniz **4. adım** önceki görüntüde gösterildiği.

5. Ekleme **Chit sohbet** , KB. 3 önceden tanımlanmış inancı birini seçerek, botunuzun chit sohbet desteği eklemek seçin. 

    <!-- TBD: add back in when chit chat how-to is merged
    ![Add chit-chat to KB ](../media/qnamaker-how-to-chitchat/create-kb-chit-chat.png)
    -->

6. Seçin **, KB oluşturma**.

    ![KB oluşturma](../media/qnamaker-how-to-create-kb/create-kb.png)

7. Ayıklanacak veri birkaç dakika sürer.

    ![Ayıklama](../media/qnamaker-how-to-create-kb/hang-tight-extraction.png)

8. Bilgi bankanızı başarıyla oluşturulduğunda yönlendirilirsiniz **Bilgi Bankası** sayfası.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Sohbet chit kişisel Ekle](./chit-chat-knowledge-base.md)
