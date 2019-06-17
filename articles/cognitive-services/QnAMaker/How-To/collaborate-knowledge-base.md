---
title: Bilgi Bankası - soru-cevap Oluşturucu işbirliği yapma
titleSuffix: Azure Cognitive Services
description: Soru-cevap Oluşturucu, bir Bilgi Bankası üzerinde işbirliği yapmak birden çok kişinin sağlar. Bu özellik, Azure rol tabanlı erişim denetimi ile sağlanır.
services: cognitive-services
author: tulasim88
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 01/14/2019
ms.author: tulasim
ms.openlocfilehash: ca754f197a46fc41b6f1b432611a2177ec0afafa
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61374855"
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
