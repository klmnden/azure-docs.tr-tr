---
title: Azure Active Directory kimlik koruması bildirimleri | Microsoft Docs
description: Bildirimleri araştırma etkinliklerinizi'ı nasıl desteklediğini öğrenin.
services: active-directory
keywords: Azure active directory kimlik koruması, cloud app discovery'yi, uygulamalar, güvenlik, risk, risk düzeyi, güvenlik açığı, güvenlik ilkesi yönetme
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 65ca79b9-4da1-4d5b-bebd-eda776cc32c7
ms.service: active-directory
ms.component: protection
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: e72cfab8886f28982e5451362704d6a7a9fe59e4
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34713822"
---
# <a name="azure-active-directory-identity-protection-notifications"></a>Azure Active Directory kimlik koruması bildirimleri

Azure AD kimlik koruması iki tür kullanıcı risk ve risk olaylarını yönetmenize yardımcı olmak için otomatik bildirim e-postaları gönderir:

- Risk altındaki kullanıcılar e-posta algılandı
- Haftalık Özet e-posta

Bu makalede her iki bildirim e-postaları bir bakış sağlar.


## <a name="users-at-risk-detected-email"></a>Risk altındaki kullanıcılar e-posta algılandı

Algılanan hesabına risk yanıt olarak, Azure AD kimlik koruması olan bir e-posta uyarı oluşturur. **algılanan risk altındaki kullanıcılar** konu olarak. E-posta için bir bağlantı içerir **[bayrak eklenen kullanıcılar için risk](active-directory-reporting-security-user-at-risk.md)** rapor. En iyi uygulama, risk altındaki kullanıcılar hemen araştırmanız gerekir.

![Risk altındaki kullanıcılar e-posta algılandı](./media/active-directory-identityprotection-notifications/01.png)


### <a name="configuration"></a>Yapılandırma

Bir yönetici olarak ayarlayabilirsiniz:

- **Bu e-posta nesil tetikler risk düzeyi** -varsayılan olarak, "Yüksek" riskli risk düzeyini ayarlayın.
- **Bu e-posta alıcılarını** -varsayılan olarak, tüm genel yöneticilere alıcılar içerir. Genel yönetici ayrıca diğer genel Yöneticiler, güvenlik yöneticileri güvenlik okuyucular alıcıları ekleyin.  


İlgili iletişim kutusunu açmak için **uyarıları** içinde **ayarları** bölümünü **kimlik koruması** sayfası.

![Risk altındaki kullanıcılar e-posta algılandı](./media/active-directory-identityprotection-notifications/05.png)


## <a name="weekly-digest-email"></a>Haftalık Özet e-posta

Haftalık Özet e-posta yeni risk olaylarını özetini içerir.  
Aşağıdakileri içerir:

- Risk altındaki kullanıcılar

- Kuşkulu etkinlikler

- Algılanan güvenlik açıkları

- Kimlik koruması ilgili raporlarda bağlantılar

    ![Düzeltme](./media/active-directory-identityprotection-notifications/400.png "düzeltme")

### <a name="configuration"></a>Yapılandırma

Yönetici olarak, Haftalık Özet e-posta gönderme devre dışı geçiş yapabilirsiniz.

![Kullanıcı riskleri](./media/active-directory-identityprotection-notifications/62.png "kullanıcı riskleri")

İlgili iletişim kutusunu açmak için **Haftalık Özet** içinde **ayarları** bölümünü **kimlik koruması** sayfası.

![Risk altındaki kullanıcılar e-posta algılandı](./media/active-directory-identityprotection-notifications/04.png)


## <a name="see-also"></a>Ayrıca bkz.

- [Azure Active Directory kimlik koruması](active-directory-identityprotection.md)
