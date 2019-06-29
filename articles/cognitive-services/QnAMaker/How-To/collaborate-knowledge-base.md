---
title: Bilgi Bankası - soru-cevap Oluşturucu işbirliği yapma
titleSuffix: Azure Cognitive Services
description: Soru-cevap Oluşturucu, bir Bilgi Bankası üzerinde işbirliği yapmak birden çok kişinin sağlar. Bu özellik, Azure rol tabanlı erişim denetimi ile sağlanır.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 01/14/2019
ms.author: diberry
ms.openlocfilehash: 757b791f632e6d7ecb15a0a40384061f0658a6ff
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67447507"
---
# <a name="collaborate-on-your-knowledge-base"></a>Bilgi bankanızı üzerinde işbirliği yapın

Soru-cevap Oluşturucu, bir Bilgi Bankası üzerinde işbirliği yapmak birden çok kişinin sağlar. Bu özellik, Azure ile sağlanan [rol tabanlı erişim denetimi](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure). 

Soru-cevap Oluşturucu hizmetinizi biriyle paylaşmak için aşağıdaki adımları gerçekleştirin:

1. Azure portalında oturum açın ve soru-cevap Oluşturucu kaynağınıza gidin.

    ![Soru-cevap Oluşturucu kaynak listesi](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-resource-list.PNG)

2. Git **erişim denetimi (IAM)** sekmesi.

    ![QnA Maker IAM](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam.PNG)

3. **Add (Ekle)** seçeneğini belirleyin.

    ![Soru-cevap Oluşturucu IAM Ekle](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add.PNG)

4. Seçin **sahibi** veya **katkıda bulunan** rol. Rol tabanlı erişim denetimi aracılığıyla salt okunur erişim izni veremiyor. Sahibi ve katkıda bulunan rolü, soru-cevap Oluşturucu hizmetini sağa okuma-yazma erişimi vardır.

    ![Soru-cevap Oluşturucu IAM Rol Ekle](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add-role.PNG)

5. Paylaşın ve Kaydet'e basın istediğiniz e-posta girin.

    ![Soru-cevap Oluşturucu IAM e-posta Ekle](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add-email.PNG)

Soru-cevap Oluşturucu hizmetinizle paylaştığınız kişi, artık oturum açtığı [soru-cevap Oluşturucu portalı](https://qnamaker.ai) bu hizmet, tüm bilgi bankalarından görebilir.

Unutmayın, bir soru-cevap Oluşturucu hizmetinde belirli bir Bilgi Bankası paylaşamazsınız. Daha ayrıntılı erişim kontrolü isterseniz, farklı bir soru-cevap Oluşturucu hizmetlerinde, bilgi bankalarından dağıtma göz önünde bulundurun.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi Bankası test](./test-knowledge-base.md)
