---
title: Azure kaynakları için Privileged Identity Management'ta erişim değerlendirmesi gerçekleştirme | Microsoft Docs
description: Bu belge, kaynak rolüne göre Azure kaynakları için PIM'de erişim gözden geçirmesi gerçekleştirmeyi açıklar.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: protection
ms.date: 03/30/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: fc4499e56d3508086365a353d5fa3f2bb42082b7
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37447311"
---
# <a name="perform-an-access-review-in-pim-according-to-resource-role"></a>Kaynak rolüne göre PIM'de erişim değerlendirmesi gerçekleştirme
Azure kaynakları için Privileged Identity Management (PIM), kuruluşların azure'daki kaynaklara ayrıcalıklı erişimi yönetme sürecini basitleştirir. 

Bir yönetici rolüne atanırsa, kuruluşunuzun ayrıcalıklı Rol Yöneticisi yine de bu rol için işinizi gerektiğini düzenli olarak doğrulamanızı isteyebilir. Bir bağlantısını içeren bir e-posta alabilir veya doğrudan gidebilirsiniz [Azure portalında](https://portal.azure.com). Bir kendi kendini gözden geçirin, atanan rollerinin gerçekleştirmek için bu makaledeki adımları izleyin.

Erişim gözden geçirmelerine ilgilenen bir ayrıcalıklı rol yöneticisi değilseniz, daha fazla bilgi edinin [erişim gözden geçirmesi başlatma](pim-resource-roles-start-access-review.md).

## <a name="add-the-privileged-identity-management-application"></a>Privileged Identity Management uygulamasını ekleme
Azure Active Directory (Azure AD) PIM uygulamasında kullanabileceğiniz [Azure portalında](https://portal.azure.com/) , gözden geçirmek için. Portalınızda uygulamanız yoksa, başlamak için aşağıdaki adımları izleyin.

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Kullanıcı Azure portalının sağ üst köşedeki adlandırın ve burada yapacaklarınız dizini seçin seçin çalışıyor olabilir.
3. Seçin **tüm hizmetleri**ve **filtre** aramak için arama kutusunu *Azure AD Privileged Identity Management*.
4. Denetleme **panoya Sabitle**ve ardından **Oluştur**. PIM uygulamasını açar.

## <a name="approve-or-deny-access"></a>Onaylayın veya reddedin erişim
Onaylayabilir ya da erişimi reddetmek yalnızca Gözden Geçiren, yine de bu rolü veya kullanıp kullanmadığını belirten. Seçin **Onayla** rolünde kalmak istiyorsanız veya **Reddet** erişim artık ihtiyacınız yoksa. Gözden Geçiren sonuçlar yalnızca geçerli olduğu durumlarda durumunuzu değiştirir.

Erişim değerlendirmesi tamamlama ve bulmak için aşağıdaki adımları izleyin:
1. Azure AD PIM uygulamaya göz atın.
2. Seçin **erişimi gözden geçir** dikey penceresi.

   ![Seçili gözden geçirme erişim dikey penceresi ekran görüntüsü, PIM uygulamanızdan](media/azure-pim-resource-rbac/rbac-access-review-complete.png)

3. Gözden geçirmeyi tamamlamak istiyorsanız seçin. 
4. Seçin ya da **onaylama** veya **Reddet**. İçinde **neden kutusu sağlama**, kararınız için bir neden eklemeniz gerekir.

   ![Ekran görüntüsü, gözden geçirme Ayrıntıları sayfası](media/azure-pim-resource-rbac/rbac-access-review-choice.png)
