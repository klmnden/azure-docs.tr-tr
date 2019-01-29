---
title: Azure Active Directory kimlik koruması bildirimleri | Microsoft Docs
description: Nasıl araştırma etkinliklerinizi bildirimlerini desteklemek öğrenin.
services: active-directory
keywords: Azure active directory kimlik koruması, bulut uygulaması bulma, yönetme, uygulamaları, güvenlik, risk, risk düzeyi, güvenlik açığı, güvenlik ilkesi
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 65ca79b9-4da1-4d5b-bebd-eda776cc32c7
ms.service: active-directory
ms.subservice: conditional-access
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: f4a3e00b346bd1e2f84218ea19ee80a7c66bc8e4
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55164546"
---
# <a name="azure-active-directory-identity-protection-notifications"></a>Azure Active Directory kimlik koruması bildirimleri

Azure AD kimlik koruması, iki tür kullanıcı riski ve risk olaylarını yönetmenize yardımcı olmak için otomatik bildirim e-posta gönderir:

- Risk altındaki kullanıcılar e-posta algılandı
- Haftalık Özet e-postası

Bu makalede her iki bildirim e-postaları bir bakış sağlar.


## <a name="users-at-risk-detected-email"></a>Risk altındaki kullanıcılar e-posta algılandı

Algılanan bir hesap risk altında yanıt olarak, Azure AD kimlik koruması olan bir e-posta uyarı oluşturur. **algılanan risk altındaki kullanıcılar** nesnesi. E-posta bağlantısını içeren **[risk için işaretlenen kullanıcılar](../reports-monitoring/concept-user-at-risk.md)** rapor. En iyi uygulama, risk altındaki kullanıcılar hemen araştırmanız gerekir.

![Risk altındaki kullanıcılar e-posta algılandı](./media/notifications/01.png)


### <a name="configuration"></a>Yapılandırma

Bir yönetici olarak ayarlayabilirsiniz:

- **Bu e-posta oluşturulmasını tetikler risk düzeyi** -varsayılan olarak, "Yüksek" risk risk düzeyi ayarlanır.
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
