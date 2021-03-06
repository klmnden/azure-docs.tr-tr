---
title: Bilgi bankası oluşturma
titleSuffix: QnA Maker API - Azure Cognitive Services
description: Kullanım eklemek için soru-cevap Oluşturucu API'si hizmeti portalı chit Sohbeti ile Bilgi Bankası oluşturun. Bu, uygulamanızı kolaylaştırır ilgi çekici. Üst chit sohbet önceden doldurulmuş bir dizi, botun chit-sohbet için bir başlangıç noktası olarak, KB ekleyin ve zaman ve bunları sıfırdan yazma maliyet tasarrufu.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 05/10/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: 8c674d33d7f0958fbce8f080f43404c480c7410c
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67447452"
---
# <a name="quickstart-create-a-knowledge-base-using-the-qna-maker-api-service-portal"></a>Hızlı Başlangıç: Soru-cevap Oluşturucu API'si hizmeti portalını kullanarak Bilgi Bankası oluşturma

Soru-cevap Oluşturucu API'si hizmeti Portalı Bilgi Bankası oluştururken, mevcut veri kaynaklarınızı eklemek basit hale getirir. Yeni bir soru-cevap Oluşturucu Bilgi Bankası aşağıdaki belge türlerinden oluşturabilirsiniz:

<!-- added for scanability -->
* SSS sayfaları
* Ürünleri kılavuzları
* Yapılandırılmış belgeleri

Bilgi birikiminizi kullanıcılarınıza daha çok ilgi çekici hale getirmek için bir sohbet chit kişilik içerir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun. 

## <a name="create-a-new-knowledge-base"></a>Yeni bilgi bankası oluşturma

1. İçine oturum [soru-cevap Oluşturucu portalı](https://qnamaker.ai) seçin ve Azure kimlik bilgileri ile **Bilgi Bankası oluşturma**.

1. Soru-cevap Oluşturucu hizmetini zaten oluşturmadıysanız seçin **soru-cevap hizmeti oluşturma**. 

1. Bir Azure kiracısı, Azure abonelik adı ve listelerden soru-cevap Oluşturucu hizmeti ile ilişkili Azure kaynağı adı seçin **2. adım** soru-cevap Oluşturucu Portalı'nda. Bilgi Bankası barındıracak Azure soru-cevap Oluşturucu hizmetini seçin.

    ![Soru-cevap hizmetini ayarlama](../media/qnamaker-how-to-create-kb/setup-qna-resource.png)

1. Yeni Bilgi Bankası için bilgi bankanızı ve veri kaynakları adını girin.

    ![Kümesi veri kaynakları](../media/qnamaker-how-to-create-kb/set-data-sources.png)

    - Hizmetinize vermek bir **adı.** Yinelenen adları ve özel karakterler desteklenir.
    - URL için ayıklanan istediğiniz verileri ekleyin. Desteklenen kaynakları türleri hakkında daha fazla bilgi [burada](../Concepts/data-sources-supported.md).
    - Ayıklanan istediğiniz veri dosyalarını karşıya yükleyin. Bkz: [fiyatlandırma bilgileri](https://aka.ms/qnamaker-pricing) kaç belgeleri görmek için ekleyebilirsiniz.
    - Bankalarıyla el ile eklemek istiyorsanız, atlayabilirsiniz **4. adım** önceki görüntüde gösterildiği.

1. Ekleme **Chit sohbet** , KB. İnancı birini seçerek, botunuzun chit sohbet desteği eklemek seçin. 

    ![Sohbet chit KB olarak Ekle](../media/qnamaker-how-to-create-kb/create-kb-chit-chat.png)

1. Seçin **, KB oluşturma**.

    ![KB oluşturma](../media/qnamaker-how-to-create-kb/create-kb.png)

1. Ayıklanacak veri birkaç dakika sürer.

    ![Ayıklama](../media/qnamaker-how-to-create-kb/hang-tight-extraction.png)

1. Bilgi bankanızı başarıyla oluşturulduğunda yönlendirilirsiniz **Bilgi Bankası** sayfası.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilgi Bankası ile işiniz bittiğinde soru-cevap Oluşturucu Portalı'nda kaldırın.

## <a name="next-steps"></a>Sonraki adımlar

Maliyet tasarrufu ölçüler için yapabilecekleriniz [paylaşmak](upgrade-qnamaker-service.md?#share-existing-services-with-qna-maker) soru-cevap Oluşturucu için oluşturulan bazı ancak tüm Azure kaynakları.

> [!div class="nextstepaction"]
> [Sohbet chit kişisel Ekle](./chit-chat-knowledge-base.md)
