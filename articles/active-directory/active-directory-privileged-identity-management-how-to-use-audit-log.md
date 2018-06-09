---
title: Azure AD Privileged Identity Management denetim günlüğünü kullanma | Microsoft Docs
description: Azure Privileged Identity Management uzantısı'nda denetim günlüğü kullanmayı öğrenin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: article
ms.workload: identity
ms.component: protection
ms.date: 02/14/2017
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 05a70874367d578beee1dc605510f9370b6abd42
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35233001"
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

