---
title: Privileged Identity Management'ı kullanarak Azure kaynaklarında erişim incelemeler gerçekleştirmek | Microsoft Docs
description: Privileged Identity Management Azure kaynakları için erişim gözden geçirme başlatılacağı açıklanmaktadır
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.component: protection
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: b5c2f13386a996a6c7895bd4755b6cf609a5df72
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35233263"
---
# <a name="perform-access-reviews-in-azure-resources-by-using-privileged-identity-management"></a>Privileged Identity Management'ı kullanarak Azure kaynaklarında erişim incelemeler gerçekleştirin
Kullanıcılar ayrıcalıklı artık gerekmeyen erişimi olan, rol atamaları "eski" olur. Bu eski rol atamaları ile ilişkili riskini azaltmak için ayrıcalıklı rol Yöneticiler düzenli olarak rolleri gözden geçirmelisiniz. Bu belge, bir erişim gözden geçirme için Azure kaynaklarını ayrıcalıklı Kimlik Yönetimi (PIM) başlatmak için adımlar kapsanmaktadır.

PIM uygulama ana sayfasından gidin:

* **Erişim incelemeler** > **Ekle**

![Erişim incelemeler ekleme](media/azure-pim-resource-rbac/rbac-access-review-home.png)

Seçtiğinizde, **Ekle** düğmesini **erişim gözden geçirme oluşturmak** dikey penceresi görünür. Bu dikey penceredeki gözden geçirin ve kimin gözden mu karar bir role'ü seçin, bir ad ve süre sınırı gözden yapılandırın.

![Erişim gözden geçirmesi oluştur](media/azure-pim-resource-rbac/rbac-create-access-review.png)

### <a name="configure-the-review"></a>Gözden geçirme yapılandırın
Bir erişim gözden geçirme oluşturmak için önce ad ve bir başlangıç ve bitiş tarihi ayarlayın.

![Gözden geçirme - ekran görüntüsü yapılandırma](media/azure-pim-resource-rbac/rbac-access-review-setting-1.png)

Kullanıcıları için tamamlamak yeterince uzun gözden geçirme uzunluğu olun. Bitiş tarihinden önce son varsa, bunlar her zaman gözden erken durdurabilirsiniz.

### <a name="choose-a-role-to-review"></a>Gözden geçirmek için bir rol seçin
Her gözden geçirme sadece tek bir rol üzerinde odaklanmıştır. Belirli bir rol dikey penceresinden erişim gözden geçirme başlattığınız sürece, bir rol artık seçmeniz gerekir.

1. Git **rol üyeliğini gözden geçirin**
   
    ![Gözden geçirme rol üyeliğini - ekran görüntüsü](media/azure-pim-resource-rbac/rbac-access-review-setting-2.png)
2. Listeden bir rol seçin.

### <a name="decide-who-will-perform-the-review"></a>Gözden geçirme işlemini gerçekleştirecek karar verin
Bir gözden geçirme gerçekleştirmek için üç seçenek vardır. Gözden geçirme tamamlamak için başka birine atayabilir, kendiniz yapabileceğiniz veya her bir kullanıcı kendi access gözden geçirebilirsiniz.

1. Seçeneklerden birini seçin:
   
   * **Seçili kullanıcıları**: erişim gerek duyan bilmiyorsanız bu seçeneği kullanın. Bu seçenek ile tamamlamak için bir kaynak sahibi veya grup yöneticisi gözden geçirme atayabilirsiniz.
   * **Atanan (self)**: kendi rol atamalarını gözden kullanıcınız için bu seçeneği kullanın.
   
2. Git **seçin gözden geçirenler**.
   
    ![Gözden geçirenler - ekran görüntüsü seçin](media/azure-pim-resource-rbac/rbac-access-review-setting-3.png)

### <a name="start-the-review"></a>İncelemesi başlatma
Son olarak, kullanıcıların erişimi onaylamak için bir sebep sunmasını isteyebilirsiniz. İsterseniz, gözden geçirme açıklamasını ekleyin. Ardından **Başlat**.

Kullanıcılarınız kendileri için bekleyen bir erişim gözden geçirme olduğunu biliyor ve bunları Göster izin emin olun [erişim incelemesi gerçekleştirme](pim-resource-roles-perform-access-review.md).

## <a name="manage-the-access-review"></a>Erişim gözden geçirme yönetme
Gözden geçirenler kendi incelemeler tamamlarken, PIM Azure kaynakları panosunda ilerleme durumunu izleyebilirsiniz. Erişim hakları yok kadar dizinde değiştirilir [gözden tamamlandı](pim-resource-roles-complete-access-review.md).

Değerlendirme süresi bitene kadar gözden tamamlamak için kullanıcılara anımsatmak veya erişim incelemeler bölümünden erken gözden durdurun.

