---
title: Azure Active Directory kimlik koruması bildirimleri | Microsoft Docs
description: Nasıl araştırma etkinliklerinizi bildirimlerini desteklemek öğrenin.
services: active-directory
keywords: Azure active directory kimlik koruması, bulut uygulaması bulma, yönetme, uygulamaları, güvenlik, risk, risk düzeyi, güvenlik açığı, güvenlik ilkesi
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: daveba
editor: ''
ms.assetid: 65ca79b9-4da1-4d5b-bebd-eda776cc32c7
ms.service: active-directory
ms.subservice: identity-protection
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2017
ms.author: joflore
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0065ec03695ee977133ae2ec43aafba7d5bfff78
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65784351"
---
# <a name="azure-active-directory-identity-protection-notifications"></a>Azure Active Directory kimlik koruması bildirimleri

Azure AD kimlik koruması, iki tür kullanıcı riski ve risk olaylarını yönetmenize yardımcı olmak için otomatik bildirim e-posta gönderir:

- Risk altındaki kullanıcılar e-posta algılandı
- Haftalık Özet e-postası

Bu makalede her iki bildirim e-postaları bir bakış sağlar.


## <a name="users-at-risk-detected-email"></a>Risk altındaki kullanıcılar e-posta algılandı

Algılanan bir hesap risk altında yanıt olarak, Azure AD kimlik koruması olan bir e-posta uyarı oluşturur. **algılanan risk altındaki kullanıcılar** nesnesi. E-posta bağlantısını içeren **[risk için işaretlenen kullanıcılar](../reports-monitoring/concept-user-at-risk.md)** rapor. En iyi uygulama, risk altındaki kullanıcılar hemen araştırmanız gerekir.

Bu uyarı için bir yapılandırma oluşturulacak uyarı istediğiniz hangi kullanıcı risk düzeyinde belirtmenizi sağlar. Kullanıcının risk düzeyi belirtmiş olduğunuz ulaştığında e-posta oluşturulur; Ancak, bunlar bu kullanıcı risk düzeyi için taşıma işlemi sonrasında yeni kullanıcıları bu kullanıcı için e-posta uyarıları algılanan risk almamayı. Örneğin, Orta kullanıcı riski ve John taşır için orta düzeyde riskli kullanıcı uyarmak için ilke ayarlarsanız, John için kullanıcıları algılanan risk e-posta alırsınız. Ancak, John sonra yüksek risk taşır veya ek risk olayları sahip ikinci bir risk algılandı uyarısı kullanıcıları almazsınız.

![Risk altındaki kullanıcılar e-posta algılandı](./media/notifications/01.png)


### <a name="configuration"></a>Yapılandırma

Bir yönetici olarak ayarlayabilirsiniz:

- **Bu e-posta oluşturulmasını tetikler kullanıcı risk düzeyi** -varsayılan olarak, "Yüksek" risk risk düzeyi ayarlanır.
- **Bu e-posta alıcılarını** -varsayılan olarak, tüm genel yöneticilere alıcılar içerir. Genel Yöneticiler, diğer genel Yöneticiler, güvenlik yöneticileri, güvenlik okuyucuları alıcı da ekleyebilirsiniz.  


İlgili iletişim kutusunu açmak için **uyarılar** içinde **ayarları** bölümünü **kimlik koruması** sayfası.

![Risk altındaki kullanıcılar e-posta algılandı](./media/notifications/05.png)


## <a name="weekly-digest-email"></a>Haftalık Özet e-postası

Haftalık Özet e-postası, yeni risk olayları bir özetini içerir.  
Aşağıdakileri içerir:

- Risk altındaki kullanıcılar

- Kuşkulu etkinlikler

- Algılanan güvenlik açıklarını

- Kimlik koruması ilgili raporlarda bağlantılar

    ![Düzeltme](./media/notifications/400.png "düzeltme")

### <a name="configuration"></a>Yapılandırma

Bir yönetici olarak bir Haftalık Özet e-posta gönderme devre dışı geçiş yapabilirsiniz.

![Kullanıcı risk](./media/notifications/62.png "kullanıcı risk")

İlgili iletişim kutusunu açmak için **Haftalık Özet** içinde **ayarları** bölümünü **kimlik koruması** sayfası.

![Risk altındaki kullanıcılar e-posta algılandı](./media/notifications/04.png)


## <a name="see-also"></a>Ayrıca bkz.

- [Azure Active Directory kimlik koruması](../active-directory-identityprotection.md)
