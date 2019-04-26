---
title: Azure kaynak rolleri için erişim gözden geçirmesi PIM - Azure Active Directory başlangıç | Microsoft Docs
description: Azure AD Privileged Identity Management (PIM) Azure kaynak rolleri için erişim gözden geçirmesi başlatmayı öğrenin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: pim
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 46903967b375d882dc3c7a62cd0b7f8b6059f8b3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60287068"
---
# <a name="start-an-access-review-for-azure-resource-roles-in-pim"></a>Azure kaynak rolleri için erişim gözden geçirmesi PIM'de Başlat
Kullanıcılar artık ihtiyacınız yoksa erişim ayrıcalıklı, rol atamaları "eski" olur. Bu eski rol atamaları ile ilişkili riskini azaltmak için ayrıcalıklı rol yöneticileri düzenli olarak rolleri gözden geçirmelisiniz. Bu belge, Azure Active Directory (Azure AD) Privileged Identity Management (PIM) erişim gözden geçirmesi başlatma adımlarını kapsar.

PIM uygulama ana sayfadan sayfaya gidin:

* **Erişim gözden geçirmeleriyle** > **Ekle**

![Erişim gözden geçirmelerine ekleme](media/azure-pim-resource-rbac/rbac-access-review-home.png)

Seçtiğinizde, **Ekle** düğmesi **erişim gözden geçirmesi Oluştur** dikey penceresi görünür. Bu dikey penceredeki gözden bir ad ve süre sınırı ile yapılandırın, gözden geçirin ve ardından kim gözden yapıyor karar bir rol seçin.

![Erişim gözden geçirmesi oluşturma](media/azure-pim-resource-rbac/rbac-create-access-review.png)

### <a name="configure-the-review"></a>Gözden geçirmeyi yapılandırma
Erişim gözden geçirmesi oluşturma için ilk adlandırın ve ardından bir başlangıç ve bitiş tarihi ayarlayın.

![Gözden geçirme - ekran görüntüsü yapılandırma](media/azure-pim-resource-rbac/rbac-access-review-setting-1.png)

Gözden geçirmeyi tamamlamak, kullanıcılar için yeterince uzun uzunluğunu olun. Bitiş tarihinden önce tamamlandığında, bunlar her zaman gözden erken durdurabilirsiniz.

### <a name="choose-a-role-to-review"></a>Gözden geçirmek için bir rol seçin
Her gözden geçirme, yalnızca bir rol üzerinde odaklanır. Erişim gözden geçirmesi özel rol dikey penceresinden çalışmaya sürece, bir rol artık seçmeniz gerekebilir.

1. Git **rol üyeliğini gözden geçirme**
   
    ![Gözden geçirme rol üyeliğini - ekran görüntüsü](media/azure-pim-resource-rbac/rbac-access-review-setting-2.png)
2. Listeden bir rol seçin.

### <a name="decide-who-will-perform-the-review"></a>Kimin gözden geçirmeyi gerçekleştirip karar verin
Bir gözden geçirme gerçekleştirmeye yönelik üç seçenek vardır. Gözden geçirmeyi tamamlamak için başka bir kişiye atayabilirsiniz, kendinize yapabileceğiniz veya her kullanıcının kendi erişimini gözden geçirebilirsiniz.

1. Seçeneklerden birini seçin:
   
   * **Seçili kullanıcıları**: Erişim gerek duyan bilmiyorsanız bu seçeneği kullanın. Bu seçenek belirtilmişse, gözden geçirmeyi tamamlamak için bir kaynak sahibi veya grup yöneticisi atayabilirsiniz.
   * **Atanan (kendi)**: Kullanıcılar kendi rol atamalarını gözden geçirmek için bu seçeneği kullanın.
   
2. Git **Gözden Geçiren seçin**.
   
    ![Gözden Geçiren - ekran görüntüsü seçin](media/azure-pim-resource-rbac/rbac-access-review-setting-3.png)

### <a name="start-the-review"></a>Değerlendirmesi başlatma
Son olarak, kullanıcılar erişim onaylamak için bir neden sağlamasını gerektirir. İsterseniz, gözden geçirme açıklaması ekleyin. Ardından **Başlat**.

Kullanıcılarınız kendileri için bekleyen bir erişim gözden geçirmesi olduğunu biliyor ve bunları Göster olanak sağlayın [erişim gözden geçirmesi gerçekleştirme](pim-resource-roles-perform-access-review.md).

## <a name="manage-the-access-review"></a>Erişim gözden geçirmesi yönetme
Gözden geçirenler, gözden geçirmeleri tamamladıkça, PIM Azure kaynakları panosunda ilerleme durumunu izleyebilirsiniz. Erişim hakları dizine kadar değişen [incelemesini tamamladı](pim-resource-roles-complete-access-review.md).

Değerlendirme süresi bitene kadar gözden geçirmelerini tamamlamak için kullanıcılara hatırlatın veya erken erişim gözden geçirmeleri bölümünden gözden geçirmesini durdur.

## <a name="next-steps"></a>Sonraki adımlar

- [PIM Azure kaynak rolleri için erişim değerlendirmesi tamamlama](pim-resource-roles-complete-access-review.md)
- [PIM hizmetinde Azure kaynak rollerimin erişim gözden geçirmesini gerçekleştirme](pim-resource-roles-perform-access-review.md)
- [Azure AD rolleri için erişim gözden geçirmesi PIM'de Başlat](pim-how-to-start-security-review.md)
