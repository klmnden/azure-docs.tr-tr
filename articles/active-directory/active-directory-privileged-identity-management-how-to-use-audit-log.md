---
title: Azure AD Privileged Identity Management'a denetim günlüğünü kullanma | Microsoft Docs
description: Azure Privileged Identity Management uzantısı'nda denetim günlüğü kullanmayı öğrenin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: protection
ms.date: 02/14/2017
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 7dfd14246622f297a42c6b7975539fec2882bbe0
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37448178"
---
# <a name="using-the-audit-log-in-pim"></a>PIM'de denetim günlüğünü kullanma
Privileged Identity Management (PIM) denetim günlüğü, belirli bir süre içinde tüm kullanıcı atama ve etkinleştirme görmek için kullanabilirsiniz. Etkinlik, kiracınızda yönetici ve son kullanıcı eşitleme etkinliği de dahil olmak üzere tam denetim geçmişini görmek istiyorsanız kullanabileceğiniz [Azure Active Directory erişim ve kullanım raporlarını.](active-directory-view-access-usage-reports.md)

## <a name="navigate-to-the-audit-log"></a>Denetim günlüğüne gidin
Gelen [Azure portalında](https://portal.azure.com) panoyu seçin **Azure AD Privileged Identity Management** uygulama. Buradan, Denetim günlüğüne tıklayarak erişim **ayrıcalıklı rollerimi Yönet** > **denetim geçmişi** PIM panosunda.

## <a name="the-audit-log-graph"></a>Denetim günlüğü grafiği
Denetim günlüğü, toplam etkinleştirmelerin, günlük en fazla etkinleştirme ve günlük ortalama etkinleştirme bir çizgi grafikte görüntülemek için kullanabilirsiniz.  Denetim geçmişi birden fazla rol varsa verileri role göre filtre uygulayabilirsiniz.

Kullanım **zaman**, **eylem**, ve **rol** günlük sıralamak için düğmeler.

## <a name="the-audit-log-list"></a>Denetim günlüğü listesi
Denetim günlüğü listesi sütunları şunlardır:

* **İstek sahibi** -rol etkinleştirme ya da değişiklik isteyen kullanıcı.  "Azure sistem" değeri ise daha fazla bilgi için Azure denetim günlüğünü denetleyin.
* **Kullanıcı** -etkinleştiren veya bir rolü atanmış olan kullanıcı.
* **Rol** -role atanmış veya kullanıcı tarafından etkinleştirildi.
* **Eylem** - istek sahibi tarafından gerçekleştirilen eylemler. Bu atama, atamayı kaldırma konusunda, etkinleştirme veya devre dışı bırakma içerebilir.
* **Zaman** - eylem oluştu.
* **Akıl** -etkinleştirme sırasında herhangi bir metin neden alana girildiyse, burada gösterilir.
* **Sona erme** - rol etkinleştirmesi için yalnızca ilgili.

## <a name="filter-the-audit-log"></a>Denetim günlüğünü Filtrele
Denetim günlüğüne tıklayarak gösterilir bilgileri filtreleyebilirsiniz **filtre** düğmesi.  **Güncelleştirme Grafik parametreleri dikey** görünür.

Filtreleri ayarladıktan sonra tıklayın **güncelleştirme** günlük verileri filtrelemek için.  Veriler hemen görünmüyorsa, sayfayı yenileyin.

### <a name="change-the-date-range"></a>Tarih aralığını değiştirme
Kullanım **Bugün**, **geçen hafta**, **geçtiğimiz ay**, veya **özel** denetim günlüğü zaman aralığını değiştirmek için düğmeler.

Seçtiğinizde **özel** düğmesi, sunulur bir **gelen** tarih alanı ve bir **için** tarih alanını günlüğünün tarih aralığı belirtin.  Tarih GG/AA/YYYY biçiminde girin veya tıkladığınızda **Takvim** simgesini ve bir takvimden tarih seçin.

### <a name="change-the-roles-included-in-the-log"></a>Oturum açma dahil rolleri değiştirme
İşaretleyin veya işaretini kaldırın **rol** eklemek veya günlükten dışlamak için her rolün yanındaki onay kutusunu.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

