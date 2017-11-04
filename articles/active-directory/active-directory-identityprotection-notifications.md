---
title: "Azure Active Directory kimlik koruması bildirimleri | Microsoft Docs"
description: "Bildirimleri araştırma etkinliklerinizi'ı nasıl desteklediğini öğrenin."
services: active-directory
keywords: "Azure active directory kimlik koruması, cloud app discovery'yi, uygulamalar, güvenlik, risk, risk düzeyi, güvenlik açığı, güvenlik ilkesi yönetme"
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 65ca79b9-4da1-4d5b-bebd-eda776cc32c7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/17/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: abc0f3926905295a9cf239146cce7fc57da7eb29
ms.sourcegitcommit: 6acb46cfc07f8fade42aff1e3f1c578aa9150c73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2017
---
# <a name="azure-active-directory-identity-protection-notifications"></a>Azure Active Directory kimlik koruması bildirimleri
Azure AD kimlik koruması iki tür kullanıcı risk ve risk olaylarını yönetmenize yardımcı olmak için otomatik bildirim e-postaları gönderir:

* Kullanıcı uyarı e-posta güvenliği
* Haftalık Özet e-posta

## <a name="user-compromised-alert-email"></a>Kullanıcı uyarı e-posta güvenliği
Azure AD Identity Protection tehlikeye gibi bir hesap belirlediğinde kullanıcı güvenliği aşılmış bir e-posta uyarısı oluşturulur. E-posta Identity Protection panosunda risk rapor için işaretlenmiş kullanıcılar için bir bağlantı içerir. Güvenliği aşılmış hesapları bildirimleri hemen araştırın öneririz.

## <a name="weekly-digest-email"></a>Haftalık Özet e-posta
Haftalık Özet e-posta yeni risk olaylarını özetini içerir.<br>
Aşağıdakileri içerir:

* Risk altındaki kullanıcılar
* Kuşkulu etkinlikler
* Algılanan güvenlik açıkları
* Kimlik koruması ilgili raporlarda bağlantılar

<br>
![Düzeltme](./media/active-directory-identityprotection-notifications/400.png "düzeltme")
<br>

Haftalık Özet e-posta gönderme devre dışı geçiş yapabilirsiniz.
<br><br>
![Kullanıcı riskleri](./media/active-directory-identityprotection-notifications/62.png "kullanıcı riskleri")
<br>

**İlgili yapılandırma iletişim kutusunu açmak için**:

1. Üzerinde **Azure AD Identity Protection** dikey penceresinde tıklatın **ayarları**.
   <br><br>
   ![Kullanıcı risk ilkesine](./media/active-directory-identityprotection-notifications/401.png "kullanıcı risk İlkesi")
   <br>
2. İçinde **genel** 'yi tıklatın **bildirimleri**.
   <br><br>
   ![Kullanıcı risk ilkesine](./media/active-directory-identityprotection-notifications/405.png "kullanıcı risk İlkesi")
   <br>

## <a name="see-also"></a>Ayrıca bkz.
* [Azure Active Directory kimlik koruması](active-directory-identityprotection.md)
