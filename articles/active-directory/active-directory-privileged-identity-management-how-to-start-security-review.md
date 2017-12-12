---
title: "Bir erişim incelemesi başlatma | Microsoft Docs"
description: "Azure Privileged Identity Management uygulaması ile ayrıcalıklı kimlikleri için bir erişim gözden geçirme oluşturmayı öğrenin."
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: 3e52b731-55f4-4c8a-ba87-9fd34033f52f
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/04/2017
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: f57a32ca1914d18540289ebb05421a7ae9618094
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="how-to-start-an-access-review-in-azure-ad-privileged-identity-management"></a>Azure AD Privileged Identity Management bir erişim incelemesi başlatma
Kullanıcılar ayrıcalıklı artık gerekmeyen erişimi olan, rol atamaları "eski" olur. Bu eski rol atamaları ile ilişkili riski azaltmak için ayrıcalıklı rol Yöneticiler düzenli olarak kullanıcılara verilen rolleri gözden geçirmelisiniz. Bu belgede Azure AD Privileged Identity Management (PIM) erişim gözden geçirme başlangıç adımları kapsar.

## <a name="start-an-access-review"></a>Bir erişim incelemesi başlatma
> [!NOTE]
> Azure portalı panonuza PIM uygulama eklemediyseniz, adımlara bakın [Azure Privileged Identity Management ile çalışmaya başlama](active-directory-privileged-identity-management-getting-started.md)
> 
> 

PIM uygulama ana sayfasından erişim gözden geçirme başlatmak için üç yolu vardır:

* **Erişim incelemeler** > **Ekle**
* **Rolleri** > **gözden geçirme** düğmesi
* Rol listesinden gözden geçirilmesi için belirli bir rol seçin > **gözden geçirme** düğmesi

Tıkladığınızda **gözden** düğmesini **erişim incelemesi başlatma** dikey penceresi görünür. Bu dikey pencerede, oluşturacağız gözden bir ad ve süre sınırı yapılandırmak, gözden geçirmek ve gözden geçirme işlemini gerçekleştirecek karar vermek için bir rol seçin.

![-Ekran görüntüsü bir erişim incelemesi başlatma][1]

### <a name="configure-the-review"></a>Gözden geçirme yapılandırın
Bir erişim gözden geçirme oluşturmak için ad ve bir başlangıç ve bitiş tarihi ayarlamanız gerekir.

![Gözden geçirme - ekran görüntüsü yapılandırma][2]

Kullanıcıları için tamamlamak yeterince uzun gözden geçirme uzunluğu olun. Bitiş tarihinden önce son varsa, her zaman gözden erken durdurabilirsiniz.

### <a name="choose-a-role-to-review"></a>Gözden geçirmek için bir rol seçin
Her gözden geçirme sadece tek bir rol üzerinde odaklanmıştır. Belirli bir rol dikey penceresinden erişim gözden geçirme başlattığınız sürece, bir rolü artık seçim gerekir.

1. Gidin **rol üyeliğini gözden geçirin**
   
    ![Gözden geçirme rol üyeliğini - ekran görüntüsü][3]
2. Listeden bir rol seçin.

### <a name="decide-who-will-perform-the-review"></a>Gözden geçirme işlemini gerçekleştirecek karar verin
Bir gözden geçirme gerçekleştirmek için üç seçenek vardır. Gözden geçirme tamamlamak için başka birine atayabilir, kendiniz yapabileceğiniz veya kendi access gözden her kullanıcının olabilir.

1. Gidin **gözden geçirenler seçin**
   
    ![Gözden geçirenler - ekran görüntüsü seçin][4]
2. Seçeneklerden birini seçin:
   
   * **Select İnceleme**: erişim gerek duyan bilmiyorsanız bu seçeneği kullanın. Bu seçenek ile tamamlamak için bir kaynak sahibi veya grup yöneticisi gözden geçirme atayabilirsiniz.
   * **Bana**: erişim iş nasıl gözden geçirir önizlemek veya olamaz kişilerin adına gözden geçirmek istediğiniz istiyorsanız kullanışlıdır.
   * **Üyeleri kendilerini gözden**: kendi rol atamalarını gözden kullanıcınız için bu seçeneği kullanın.

### <a name="start-the-review"></a>İncelemesi başlatma
Son olarak, kullanıcılar erişimleri onaylıyorsanız bir gerekçe sağlamasını gerektiren seçeneğiniz vardır. Gözden geçirme açıklamasını isterseniz ekleyin ve seçin **Başlat**.

Kullanıcılarınız kendileri için bekleyen bir erişim gözden geçirme olduğunu biliyor ve bunları Göster izin emin olun [erişim incelemesi gerçekleştirme](active-directory-privileged-identity-management-how-to-perform-security-review.md).

## <a name="manage-the-access-review"></a>Erişim gözden geçirme yönetme
Gözden geçirenler kendi incelemeler erişim incelemeler bölümünde Azure AD PIM panosunda tamamlarken ilerleme durumunu izleyebilirsiniz. Erişim hakları yok kadar dizinde değiştirilecek [gözden tamamlandıktan](active-directory-privileged-identity-management-how-to-complete-review.md).

Değerlendirme süresi bitene kadar gözden tamamlamak için kullanıcılara anımsatmak veya erişim incelemeler bölümünden erken gözden durdurun.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="pim-table-of-contents"></a>PIM İçindekiler
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-start-security-review/PIM_start_review.png
[2]: ./media/active-directory-privileged-identity-management-how-to-start-security-review/PIM_review_configure.png
[3]: ./media/active-directory-privileged-identity-management-how-to-start-security-review/PIM_review_role.png
[4]: ./media/active-directory-privileged-identity-management-how-to-start-security-review/PIM_review_reviewers.png
