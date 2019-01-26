---
title: PIM'de görüntülemek için Azure AD Dizin rolleri denetim geçmişi | Microsoft Docs
description: Azure AD Privileged Identity Management (PIM) için Azure AD Dizin rolleri denetim geçmişi görüntülemeyi öğrenin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: pim
ms.date: 02/14/2017
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 191d384d51fe3aecbd967586516cfb198256af81
ms.sourcegitcommit: 97d0dfb25ac23d07179b804719a454f25d1f0d46
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/25/2019
ms.locfileid: "54911736"
---
# <a name="view-audit-history-for-azure-ad-directory-roles-in-pim"></a>Azure AD dizin rollerini PIM denetim geçmişini görüntüle
Tüm ayrıcalıklı roller için belirli bir süre içinde tüm kullanıcı atama ve etkinleştirme görmek için Privileged Identity Management (PIM) denetim Geçmişi'ni kullanabilirsiniz. Etkinlik, kiracınızda yönetici ve son kullanıcı eşitleme etkinliği de dahil olmak üzere tam denetim geçmişini görmek istiyorsanız kullanabileceğiniz [Azure Active Directory erişim ve kullanım raporlarını.](../reports-monitoring/overview-reports.md)

## <a name="navigate-to-audit-history"></a>Denetim geçmişi gidin
Gelen [Azure portalında](https://portal.azure.com) panoyu seçin **Azure AD Privileged Identity Management** uygulama. Tıklayarak denetim geçmişini buradan, erişim **ayrıcalıklı rollerimi Yönet** > **denetim geçmişi** PIM panosunda.

![](media/azure-ad-pim-approval-workflow/image021.png)

>[!NOTE]
Verileri eyleme göre sıralayın ve "Etkinleştirme onaylı" arayın


## <a name="audit-history-graph"></a>Denetim Geçmişi grafiği
Denetim geçmişi, toplam etkinleştirmelerin, günlük en fazla etkinleştirme ve günlük ortalama etkinleştirme bir çizgi grafikte görüntülemek için kullanabilirsiniz.  Denetim geçmişi birden fazla rol varsa verileri role göre filtre uygulayabilirsiniz.

Kullanım **zaman**, **eylem**, ve **rol** geçmişini sıralamak için düğmeler.

## <a name="audit-history-list"></a>Denetim geçmişi listesi
Denetim geçmişi listesinde sütunlar şunlardır:

* **İstek sahibi** -rol etkinleştirme ya da değişiklik isteyen kullanıcı.  "Azure sistem" değeri ise daha fazla bilgi için Azure denetim geçmişini denetleyin.
* **Kullanıcı** -etkinleştiren veya bir rolü atanmış olan kullanıcı.
* **Rol** -role atanmış veya kullanıcı tarafından etkinleştirildi.
* **Eylem** - istek sahibi tarafından gerçekleştirilen eylemler. Bu atama, atamayı kaldırma konusunda, etkinleştirme veya devre dışı bırakma içerebilir.
* **Zaman** - eylem oluştu.
* **Akıl** -etkinleştirme sırasında herhangi bir metin neden alana girildiyse, burada gösterilir.
* **Sona erme** - rol etkinleştirmesi için yalnızca ilgili.

## <a name="filter-audit-history"></a>Denetim geçmişi Filtrele
Denetim geçmişi tıklayarak gösterilir bilgileri filtreleyebilirsiniz **filtre** düğmesi.  **Güncelleştirme Grafik parametreleri dikey** görünür.

Filtreleri ayarladıktan sonra tıklayın **güncelleştirme** geçmişinde bulunan verilere filtre uygulamak için.  Veriler hemen görünmüyorsa, sayfayı yenileyin.

### <a name="change-the-date-range"></a>Tarih aralığını değiştirme
Kullanım **Bugün**, **geçen hafta**, **geçtiğimiz ay**, veya **özel** denetim geçmişinin zaman aralığını değiştirmek için düğmeler.

Seçtiğinizde **özel** düğmesi, sunulur bir **gelen** tarih alanı ve bir **için** tarih alanını geçmişi için tarih aralığı belirtin.  Tarih GG/AA/YYYY biçiminde girin veya tıkladığınızda **Takvim** simgesini ve bir takvimden tarih seçin.

### <a name="change-the-roles-included-in-the-history"></a>Geçmişi dahil rollerini değiştirme
İşaretleyin veya işaretini kaldırın **rol** eklemek veya Geçmişten dışlamak için her rolün yanındaki onay kutusunu.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Sonraki adımlar

- [PIM Azure kaynak rolleri için etkinlik ve denetim geçmişini görüntüle](azure-pim-resource-rbac.md)
