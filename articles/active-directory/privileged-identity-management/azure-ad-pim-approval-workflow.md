---
title: Azure Privileged Identity Management onay iş akışları | Microsoft Docs
description: Onay iş akışları ayrıcalıklı Kimlik Yönetimi'nın (PIM) hakkında bilgi edinin
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: protection
ms.date: 04/28/2017
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 135c789dc6e41e07bb939ece679756c8c42de2d1
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37085292"
---
# <a name="approvals"></a>Onaylar

## <a name="overview"></a>Genel Bakış

Privileged Identity Management için onayları sayesinde, etkinleştirme için onay gerektirmesini rollerini yapılandırın ve bir veya birden çok kullanıcı veya grup temsilci onaylayanlar olarak seçin. Rollerini yapılandırın ve onaylayanlar seçin öğrenmek için okuma tutun.


## <a name="new-terminology"></a>Yeni terminolojisi

*Uygun rol kullanıcı* – kuruluşunuzdaki uygun olarak bir Azure AD rolüne atanmış bir kullanıcı bir uygun rol olduğu (Rol etkinleştirmesi gerekir).

*Onaylayan temsilci* – bir temsilci onaylayan bir veya birden çok kişiler veya gruplar onaylama için sorumlu Azure AD içinde istek rolleri etkinleştirmek için.

## <a name="scenarios"></a>Senaryolar

Özel önizleme aşağıdaki senaryoları destekler:

**Bir ayrıcalıklı Rol Yöneticisi (PRA) olarak şunları yapabilirsiniz:**

-   [belirli roller onayını etkinleştir](#enable-approval-for-specific-roles)

-   [Onaylayan kullanıcılara ve/veya grupları istekleri onaylanacak belirtin](#specify-approver-users-and/or-groups-to-approve-requests)

-   [tüm ayrıcalıklı rolleri için istek ve onay geçmişini görüntüleme](#view-request-and-approval-history-for-all-privileged-roles)

**Belirlenen onaylayıcı olarak, şunları yapabilirsiniz:**

-   [Bekleyen onaylar (istek) görüntüleme](#view-pending-approvals-requests)

-   [onaylama veya reddetme rol yükseltme (tek ve/veya toplu) istekleri](#approve-or-reject-requests-for-role-elevation-single-and/or-bulk)

-   [my onay/reddetme için gerekçe](#provide-justification-for-my-approval/rejection) 

**Uygun bir Role kullanıcı olarak şunları yapabilirsiniz:**

-   [onay gerektiren bir rolü etkinleştirme isteği](#request-activation-of-a-role-that-requires-approval)

-   [İsteğiniz etkinleştirme durumunu görüntüleyin](#view-the-status-of-your-request-to-activate)

-   [etkinleştirme onaylanırsa Azure AD'de Görevinizi tamamlamak](#complete-your-task-in-azure-ad-if-activation-was-approved)

### <a name="navigation"></a>Gezinme

Biz onayları desteklemek için Gezinti güncelleştirdik

![](media/azure-ad-pim-approval-workflow/image001.png)

Giriş sayfasında varsayılan PIM ve yeni onayları belgeleri hakkında bilgi için uygun erişim sağlar.

![](media/azure-ad-pim-approval-workflow/image002.png)

PIM, 'My denetim Geçmişi' tüm kullanıcılar için yeni bir bölüm de ekledik. Burada tüm bilgileri kimliğinizi ilgili bulabilirsiniz. Bu seçenek, tüm bekleyen ve tamamlanmış istekleri, çözümlemeniz istekleri hakkında yaptığınız kararları ve tüm son rol etkinleştirme tek bir konumda içerir.

![](media/azure-ad-pim-approval-workflow/image003.png)

### <a name="enable-approval-for-specific-roles"></a>Belirli roller onayını etkinleştir

Belirli bir rol için onay etkinleştirmek için önce sol gezinti bölmesinden Directory rolleri seçin.

![](media/azure-ad-pim-approval-workflow/image004.png)

Bulma ve Dizin rolleri sol gezinti bölmesinde ayarlarını seçin

![](media/azure-ad-pim-approval-workflow/image006.png)

Ayrıcalıklı rolleri seçin:

![](media/azure-ad-pim-approval-workflow/image009.png)

Seçin "Etkinleştir" onay bölümüne gerektirir:

![](media/azure-ad-pim-approval-workflow/image011.png)

Bir kez etkinleştirildikten sonra dikey aşağıdaki ayrıntıları gösterecek şekilde genişletir:

![](media/azure-ad-pim-approval-workflow/image013.png)

>[!NOTE]
VERMEYİN herhangi onaylayanlar belirtirseniz, PRA(s) varsayılan approver(s) haline gelir. PRA(s) Bu rol için tüm etkinleştirme isteklerinin onaylanması gerekir.

### <a name="specify-approver-users-andor-groups-to-approve-requests"></a>Onaylayan kullanıcılara ve/veya grupları istekleri onaylanacak belirtin

Onay temsilci seçmek için "Select onaylayanlar" seçeneğine tıklayın:

![](media/azure-ad-pim-approval-workflow/image015.png)

Select onaylayanlar dikey yüklediğinde, belirli bir kullanıcı veya üstünde arama çubuğunu kullanarak veya önceden doldurulmuş haldedir listeden seçerek grubu arayın ve sonra "bittiğinde," Seç:

![](media/azure-ad-pim-approval-workflow/image017.png)

Not: Bir defada birden çok kullanıcı veya grup seçebilirsiniz.

Seçiminiz seçili onaylayanlar listesinde aşağıda görüldüğü gibi görünür:

![](media/azure-ad-pim-approval-workflow/image019.png)

Onaylayıcı kaldırmak için Kaldır düğmesini adının yanındaki kutuyu tıklamanız yeterlidir.

Ek onaylayanlar eklemek için işlemi yineleyin.

## <a name="view-request-and-approval-history-for-all-privileged-roles"></a>Tüm ayrıcalıklı rolleri için istek ve onay geçmişini görüntüleme

Tüm ayrıcalıklı rolleri istek ve onay geçmişini görüntülemek için panodan denetim geçmişi seçin:

![](media/azure-ad-pim-approval-workflow/image021.png)

>[!NOTE]
Eyleme göre verileri sıralamak ve "Etkinleştirme onaylanmış" bakın

### <a name="view-pending-approvals-requests"></a>Bekleyen onaylar (istek) görüntüleme

Onayınızı bekleyen istek olduğunda, bir temsilci onaylayan e-posta bildirimleri alırsınız. PIM Portalı'nda bu istekleri görüntülemek için Panoda (Yeni Gezinti) "Beklemedeki onay istekleri" sekmesini sol gezinti çubuğunda seçin.

![](media/azure-ad-pim-approval-workflow/image023.png)

Burada, onay bekleyen isteklerin listesini görürsünüz:

![](media/azure-ad-pim-approval-workflow/image024.png)

### <a name="approve-or-reject-requests-for-role-elevation-single-andor-bulk"></a>Onaylama veya reddetme rol yükseltme (tek ve/veya toplu) istekleri

Onaylamak veya reddetmek istediğiniz isteklerini seçin ve kararınızı ile karşılık gelen eylem çubuğunda düğmesini tıklatın:

![](media/azure-ad-pim-approval-workflow/image025.png)

### <a name="provide-justification-for-my-approvalrejection"></a>My onay/reddetme için gerekçe

Bu onaylamak veya birden çok isteği aynı anda reddetmek için yeni bir dikey pencere açılır. Kararınızı için bir gerekçe girin ve Onayla (veya reddetme) alt veya dikey:

![](media/azure-ad-pim-approval-workflow/image029.png)

İstek işlemi tamamlandığında, durum simgesinde yaptığınız karar yansıtılacaktır (Bu örnekte, onaylama karardır):

![](media/azure-ad-pim-approval-workflow/image031.png)

### <a name="request-activation-of-a-role-that-requires-approval"></a>Onay gerektiren bir rolü etkinleştirme isteği

İşlem rol etkinleştirmesi için aynı kalır gibi onay gerektiren bir rolün etkinleştirme isteyen eski PIM gezinti ya da yeni gezinti başlatılabilir. Yalnızca bir rolü etkinleştirmek için roller listeden seçin:

![](media/azure-ad-pim-approval-workflow/image033.png)

Ayrıcalıklı rol çok faktörlü kimlik doğrulaması gerektiriyorsa, önce bu görevi tamamlamak için istenir:

![](media/azure-ad-pim-approval-workflow/image035.png)

Tamamlandıktan sonra Etkinleştir'i tıklatın ve bir gerekçe (gerekliyse):

![](media/azure-ad-pim-approval-workflow/image037.png)

İstek sahibi onay bekleyen istek ise bir bildirim görürsünüz:

![](media/azure-ad-pim-approval-workflow/image039.png)

### <a name="view-the-status-of-your-request-to-activate"></a>İsteğiniz etkinleştirme durumunu görüntüleyin

Etkinleştirmek için bekleyen bir isteğiniz durumunu görüntüleme yeni gezinti bölmesinden erişilmesi gerekir. Sol Gezinti Çubuğu'ndan "İsteklerim" sekmesini seçin:

![](media/azure-ad-pim-approval-workflow/image041.png)

İstek durumu "Bekliyor" varsayılan olarak, ancak tüm görmek için geçiş yapabilir veya reddedilen istek sayısı.

### <a name="complete-your-task-in-azure-ad-if-activation-was-approved"></a>Etkinleştirme onaylanırsa Azure AD'de Görevinizi tamamlamak

İstek onaylandıktan sonra rol etkindir ve bu rol gerektiren herhangi bir iş ile devam edebilirsiniz.

![](media/azure-ad-pim-approval-workflow/image043.png)

## <a name="next-steps"></a>Sonraki adımlar

Görüşleriniz bizim için değerlidir. Lütfen bizimle burada açıklamaları ya da geribildirim paylaşmak çekinmeyin!
