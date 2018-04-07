---
title: Bir erişim gözden geçirme PIM içinde Azure kaynakları için başlangıç | Microsoft Docs
description: Privileged Identity Management Azure kaynakları için erişim gözden geçirme başlatılacağı açıklanmaktadır
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/02/2018
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 61ed4e82e0b782b423668564dae6efb272967702
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="privileged-identity-management---resource-role---start-access-review"></a>Privileged Identity Management - Kaynak rolü - başlangıç erişim gözden geçirme
Kullanıcılar ayrıcalıklı artık gerekmeyen erişimi olan, rol atamaları "eski" olur. Bu eski rol atamaları ile ilişkili riski azaltmak için ayrıcalıklı rol Yöneticiler düzenli olarak kullanıcılara verilen rolleri gözden geçirmelisiniz. Bu belge, bir erişim gözden geçirme için Azure kaynaklarını ayrıcalıklı Kimlik Yönetimi (PIM) başlatmak için adımlar kapsanmaktadır.

PIM uygulama ana sayfasından gidin:

* **Erişim incelemeler** > **Ekle**

![](media/azure-pim-resource-rbac/rbac-access-review-home.png)

Tıkladığınızda **Ekle** düğmesini **erişim gözden geçirme oluşturmak** dikey penceresi görünür. Bu dikey pencerede, oluşturacağız gözden bir ad ve süre sınırı yapılandırmak, gözden geçirmek ve gözden geçirme işlemini gerçekleştirecek karar vermek için bir rol seçin.

![](media/azure-pim-resource-rbac/rbac-create-access-review.png)

### <a name="configure-the-review"></a>Gözden geçirme yapılandırın
Bir erişim gözden geçirme oluşturmak için ad ve bir başlangıç ve bitiş tarihi ayarlamanız gerekir.

![Gözden geçirme - ekran görüntüsü yapılandırma](media/azure-pim-resource-rbac/rbac-access-review-setting-1.png)

Kullanıcıları için tamamlamak yeterince uzun gözden geçirme uzunluğu olun. Bitiş tarihinden önce son varsa, her zaman gözden erken durdurabilirsiniz.

### <a name="choose-a-role-to-review"></a>Gözden geçirmek için bir rol seçin
Her gözden geçirme sadece tek bir rol üzerinde odaklanmıştır. Belirli bir rol dikey penceresinden erişim gözden geçirme başlattığınız sürece, bir rolü artık seçim gerekir.

1. Gidin **rol üyeliğini gözden geçirin**
   
    ![Gözden geçirme rol üyeliğini - ekran görüntüsü](media/azure-pim-resource-rbac/rbac-access-review-setting-2.png)
2. Listeden bir rol seçin.

### <a name="decide-who-will-perform-the-review"></a>Gözden geçirme işlemini gerçekleştirecek karar verin
Bir gözden geçirme gerçekleştirmek için üç seçenek vardır. Gözden geçirme tamamlamak için başka birine atayabilir, kendiniz yapabileceğiniz veya kendi access gözden her kullanıcının olabilir.

1. Seçeneklerden birini seçin:
   
   * **Seçili kullanıcıları**: erişim gerek duyan bilmiyorsanız bu seçeneği kullanın. Bu seçenek ile tamamlamak için bir kaynak sahibi veya grup yöneticisi gözden geçirme atayabilirsiniz.
   * **Atanan (self)**: kendi rol atamalarını gözden kullanıcınız için bu seçeneği kullanın.
   
2. Gidin **gözden geçirenler seçin**
   
    ![Gözden geçirenler - ekran görüntüsü seçin](media/azure-pim-resource-rbac/rbac-access-review-setting-3.png)

### <a name="start-the-review"></a>İncelemesi başlatma
Son olarak, kullanıcılar erişimleri onaylıyorsanız bir gerekçe sağlamasını gerektiren seçeneğiniz vardır. Gözden geçirme açıklamasını isterseniz ekleyin ve seçin **Başlat**.

Kullanıcılarınız kendileri için bekleyen bir erişim gözden geçirme olduğunu biliyor ve bunları Göster izin emin olun [erişim incelemesi gerçekleştirme](pim-resource-roles-perform-access-review.md).

## <a name="manage-the-access-review"></a>Erişim gözden geçirme yönetme
Gözden geçirenler kendi incelemeler PIM Azure kaynakları panosundaki erişim incelemeler bölümü tamamlarken ilerleme durumunu izleyebilirsiniz. Erişim hakları yok kadar dizinde değiştirilecek [gözden tamamlandıktan](pim-resource-roles-complete-access-review.md).

Değerlendirme süresi bitene kadar gözden tamamlamak için kullanıcılara anımsatmak veya erişim incelemeler bölümünden erken gözden durdurun.

