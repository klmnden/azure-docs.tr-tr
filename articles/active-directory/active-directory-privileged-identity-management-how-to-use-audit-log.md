---
title: Azure AD Privileged Identity Management denetim günlüğünü kullanma | Microsoft Docs
description: Azure Privileged Identity Management uzantısı'nda denetim günlüğü kullanmayı öğrenin.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: article
ms.workload: identity
ms.component: users-groups-roles
ms.date: 02/14/2017
ms.author: curtand
ms.custom: pim
ms.openlocfilehash: 20fd9c5ee90947cc2d3816a0590d4780408baa2f
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="using-the-audit-log-in-pim"></a>PIM içinde denetim günlüğünü kullanma
Belirli bir süre içinde tüm kullanıcı atamaları ve etkinleştirmeler görmek için ayrıcalıklı Kimlik Yönetimi (PIM) denetim günlüğünü kullanabilirsiniz. Yönetici, son kullanıcı ve eşitleme etkinliği de dahil olmak üzere, Kiracı etkinliğinde tam denetim geçmişini görmek istiyorsanız kullanabileceğiniz [Azure Active Directory'ye erişim ve kullanım raporları.](active-directory-view-access-usage-reports.md)

## <a name="navigate-to-the-audit-log"></a>Denetim günlüğü gidin
Gelen [Azure portal](https://portal.azure.com) Pano, select **Azure AD Privileged Identity Management** uygulama. Buradan, Denetim günlüğü tıklayarak erişim **yönetmek ayrıcalıklı rolleri** > **denetim geçmişi** PIM panosunda.

## <a name="the-audit-log-graph"></a>Denetim günlüğü grafiği
Toplam etkinleştirmeler, günde en çok etkinleştirmeleri ve gün başına ortalama etkinleştirmeleri bir çizgi grafiğinde görüntülemek için denetim günlüğünü kullanabilirsiniz.  Denetim geçmişi birden fazla rol ise, veri role göre de filtre uygulayabilirsiniz.

Kullanım **zaman**, **eylem**, ve **rol** günlük sıralamak için düğmeler.

## <a name="the-audit-log-list"></a>Denetim günlüğü listesi
Denetim günlüğü listesindeki sütun şunlardır:

* **İstek sahibi** -rol etkinleştirme veya değiştirme isteyen kullanıcı.  Değer "Azure sistemi" ise, daha fazla bilgi için Azure denetim günlüğünü denetleyin.
* **Kullanıcı** -etkinleştiren veya bir role atanmış olan kullanıcı.
* **Rol** -role atanmış veya kullanıcı tarafından etkinleştirilmiş.
* **Eylem** - istek sahibi tarafından gerçekleştirilen eylemler. Bu atama, atamayı kaldırma konusunda, etkinleştirme veya devre dışı bırakma içerebilir.
* **Zaman** - eylem oluştuğunda.
* **Akıl** -herhangi bir metin neden alana etkinleştirme sırasında girilen varsa, burada gösterir.
* **Sona erme** - rol etkinleştirmesi için yalnızca ilgili.

## <a name="filter-the-audit-log"></a>Denetim günlüğü Filtrele
Denetim günlüğüne tıklayarak görünür bilgileri filtreleyebilirsiniz **filtre** düğmesi.  **Güncelleştirme Grafik Parametreler dikey** görünür.

Filtreleri ayarladıktan sonra tıklayın **güncelleştirme** günlük verileri filtrelemek için.  Veriler hemen görünmüyorsa, sayfayı yenileyin.

### <a name="change-the-date-range"></a>Tarih aralığını değiştirme
Kullanım **Bugün**, **geçen hafta**, **geçen ay**, veya **özel** denetim günlüğünü zaman aralığını değiştirmek için düğmeler.

Seçtiğinizde **özel** düğmesi, size sunulur bir **gelen** tarih alanı ve bir **için** tarih alanı günlüğü için tarih aralığını belirtin.  Tarih GG/AA/YYYY biçiminde girin veya tıklayın **Takvim** simgeyi ve bir takvimden tarihi seçin.

### <a name="change-the-roles-included-in-the-log"></a>Günlüğüne dahil rollerini değiştirme
İşaretleyin veya işaretini kaldırın **rol** eklemek veya günlükten dışlamak için her rolün yanındaki onay kutusunu.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

