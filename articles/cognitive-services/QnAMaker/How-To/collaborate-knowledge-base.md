---
title: Bilgi Bankası - Microsoft Bilişsel hizmetler işbirliği | Microsoft Docs
titleSuffix: Azure
description: QnA Maker bilgi temel işbirliği yapma
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 05/07/2018
ms.author: saneppal
ms.openlocfilehash: e18d656236276595fc5186a6656349bf28974ead
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353951"
---
# <a name="collaborate-on-your-knowledge-base"></a>Bilgi Bankası işbirliği

Bilgi Bankası üzerinde işbirliği yapmak birden çok kişinin QnA Maker sağlar. Bu özellik Azure ile sağlanan [rol tabanlı erişim denetimi](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-control-configure). 

QnA Maker hizmetinizi biriyle paylaşmak için aşağıdaki adımları gerçekleştirin:

1. Azure portalında oturum açın ve QnA Maker kaynağa gidin.

    ![QnA Maker kaynak listesi](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-resource-list.PNG)

2. Git **erişim denetimi (IAM)** sekmesi.

    ![QnA Maker IAM](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam.PNG)

3. **Add (Ekle)** seçeneğini belirleyin.

    ![QnA Maker IAM Ekle](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add.PNG)

4. Seçin **sahibi** veya **katkıda bulunan** rol.

    ![QnA Maker IAM Rol Ekle](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add-role.PNG)

5. Paylaşmasına ve kaydetme tuşuna basarak istediğiniz e-posta girin.

    ![QnA Maker IAM e-posta ekleyin](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add-email.PNG)

Kişinin QnA Maker hizmetiniz ile paylaşılan, şimdi içine oturum [QnA Maker portal](https://qnamaker.ai) bu hizmetindeki tüm Bilgi Bankası görebilirsiniz.

Belirli bir Bilgi Bankası'nda QnA Maker hizmet paylaşamaz unutmayın. Daha ayrıntılı erişim kontrolü istiyorsanız, Bilgi Bankası arasında farklı QnA Maker Hizmetleri dağıtma göz önünde bulundurun.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi Bankası test](./test-knowledge-base.md)
