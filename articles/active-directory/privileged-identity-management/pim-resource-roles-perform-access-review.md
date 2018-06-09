---
title: Bir erişim gözden geçirme için Azure kaynaklarını Privileged Identity Management gerçekleştirmek | Microsoft Docs
description: Bu belgede bir erişim incelemesi PIM içinde Azure kaynakları için gerçekleştirme göre Kaynak rolü açıklanmaktadır.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: mwahl
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.component: protection
ms.date: 03/30/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 794875cbee91c8aa4c926dbaa6931b250201a1bd
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35233963"
---
# <a name="perform-an-access-review-in-pim-according-to-resource-role"></a>Bir erişim gözden geçirme PIM içinde göre Kaynak rolü gerçekleştirin.
Azure kaynakları için ayrıcalıklı Kimlik Yönetimi (PIM), kuruluşların Azure kaynaklarına ayrıcalıklı erişimi nasıl yönettiğini basitleştirir. 

Bir yönetici rolü atanmışsa, kuruluşunuzun ayrıcalıklı Rol Yöneticisi, düzenli olarak bu rol hala işiniz için ihtiyacınız onaylamak için isteyebilir. Bir bağlantı içeren bir e-posta alabilirsiniz veya doğrudan gidebilirsiniz [Azure portal](https://portal.azure.com). Atanan rollerinizi Self gözden gerçekleştirmek için bu makaledeki adımları izleyin.

Erişim incelemeler ilgilenen bir ayrıcalıklı rol yöneticisi değilseniz, daha fazla bilgi almak [erişim incelemesi başlatma](pim-resource-roles-start-access-review.md).

## <a name="add-the-privileged-identity-management-application"></a>Privileged Identity Management uygulamasını ekleme
Azure Active Directory (Azure AD) PIM uygulamada kullanabilirsiniz [Azure portal](https://portal.azure.com/) incelemeniz gerçekleştirmek için. Portalınızda uygulamaya sahip olmamaları durumunda başlamak için aşağıdaki adımları izleyin.

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Kullanıcı Azure Portalı'nı sağ üst köşesinde adlandırın ve burada şunları yapacaksınız dizini seçin seçin çalışıyor olabilir.
3. Seçin **tüm hizmetleri**ve **filtre** için arama kutusunu *Azure AD Privileged Identity Management*.
4. Denetleme **panoya Sabitle**ve ardından **oluşturma**. PIM uygulamasını açar.

## <a name="approve-or-deny-access"></a>Onaylamak veya erişim engelle
Onaylama ya da erişimini yalnızca İnceleme bu rolü veya kullanmaya devam olup olmadığını belirten. Seçin **Onayla** rolünde olmak istiyorsanız veya **reddetme** erişim artık ihtiyaç duymuyorsanız. Yalnızca İnceleme sonuçları uyguladığında durumunuzu değiştirir.

Bul ve erişim gözden geçirme tamamlamak için aşağıdaki adımları izleyin:
1. Azure AD PIM uygulamaya göz atın.
2. Seçin **gözden erişim** dikey.

   ![Gözden geçirme erişim dikey penceresinde seçili ile ekran görüntüsü, PIM uygulama](media/azure-pim-resource-rbac/rbac-access-review-complete.png)

3. Tamamlamak için istediğiniz gözden geçirme seçin. 
4. Ya da seçin **onaylama** veya **Reddet**. İçinde **neden kutusu sağlama**, kararınızı nedeni eklemeniz gerekir.

   ![Gözden geçirme ekran Ayrıntılar sayfası](media/azure-pim-resource-rbac/rbac-access-review-choice.png)
