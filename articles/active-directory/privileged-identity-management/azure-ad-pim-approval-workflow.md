---
title: Azure Privileged Identity Management onay iş akışları | Microsoft Docs
description: Onay iş akışlarını Privileged Identity Management (PIM) içinde hakkında bilgi edinin
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: pim
ms.date: 04/28/2017
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 14d0cdc0bde1081f1a020c7039596a5b6880070f
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39619757"
---
# <a name="approvals"></a>Onaylar

## <a name="overview"></a>Genel Bakış

Privileged Identity Management için onaylarla rollerini etkinleştirme için onay gerektirecek şekilde yapılandırın ve bir veya birden çok kullanıcı veya grup onaylayanlara temsilci olarak seçin. Onaylayanları seçin ve rollerini yapılandırma hakkında bilgi edinmek için okumaya devam edin.


## <a name="new-terminology"></a>Yeni terminolojisi

*Uygun rol kullanıcı* – kuruluşunuzdaki uygun olarak bir Azure AD rolüne atanmış bir kullanıcı bir uygun rol olduğu (Rol etkinleştirme gerektirir).

*Onaylayan temsilci* – yetkilendirilmiş bir onaylayan bir veya birden çok kişi veya gruplara onaylama için sorumlu Azure AD içinde istediğinde rollerini etkinleştirmek için.

## <a name="scenarios"></a>Senaryolar

Özel önizleme aşağıdaki senaryoları destekler:

**Bir ayrıcalıklı Rol Yöneticisi (PRA) olarak şunları yapabilirsiniz:**

-   [belirli roller onayını etkinleştir](#enable-approval-for-specific-roles)

-   [Onaylayanın kullanıcı ve/veya grupları istekleri onaylamak için belirtin.](#specify-approver-users-and/or-groups-to-approve-requests)

-   [tüm ayrıcalıklı roller için istek ve onay geçmişini görüntüleme](#view-request-and-approval-history-for-all-privileged-roles)

**Belirtilen bir onaylayan olarak şunları yapabilirsiniz:**

-   [Bekleyen onaylar (istek) görüntüleme](#view-pending-approvals-requests)

-   [Rol yükseltme (tek ve/veya toplu) istekleri onaylayabileceğiniz veya](#approve-or-reject-requests-for-role-elevation-single-and/or-bulk)

-   [my onay/reddetme için gerekçe sağlayın](#provide-justification-for-my-approval/rejection) 

**Uygun bir Role kullanıcı olarak şunları yapabilirsiniz:**

-   [Etkinleştirme isteği onay gerektiren bir rolü](#request-activation-of-a-role-that-requires-approval)

-   [İsteğiniz etkinleştirme durumunu görüntüleyin](#view-the-status-of-your-request-to-activate)

-   [Etkinleştirme onaylanırsa göreviniz, Azure AD'de tamamlayın.](#complete-your-task-in-azure-ad-if-activation-was-approved)

### <a name="navigation"></a>Gezinme

Gezinti onaylar destekleyecek şekilde güncelleştirdik.

![](media/azure-ad-pim-approval-workflow/image001.png)

Giriş sayfası varsayılan PIM ve yeni bir onayları belgeler hakkında bilgi için uygun erişim sağlar.

![](media/azure-ad-pim-approval-workflow/image002.png)

PIM, 'My denetim Geçmişi' tüm kullanıcılar için yeni bir bölüm de ekledik. Burada tüm bilgiler, kimliğinizi ilgili bulabilirsiniz. Bu, tek bir kullanışlı yerde tüm bekleyen ve tamamlanmış istekleri, çözümlemeniz istekleri hakkında yaptığınız herhangi bir karar ve tüm geçmiş rol etkinleştirmeleri içerir.

![](media/azure-ad-pim-approval-workflow/image003.png)

### <a name="enable-approval-for-specific-roles"></a>belirli roller onayını etkinleştir

Belirli bir rol için onay etkinleştirmek için sol gezinti bölmesinden ilk dizin rollerini seçin.

![](media/azure-ad-pim-approval-workflow/image004.png)

Bulma ve Dizin rolleri sol gezinti bölmesinde ayarlarını seçin

![](media/azure-ad-pim-approval-workflow/image006.png)

Ayrıcalıklı rolleri seçin:

![](media/azure-ad-pim-approval-workflow/image009.png)

Seçin "Etkinleştir" onay bölümüne gerektirir:

![](media/azure-ad-pim-approval-workflow/image011.png)

Etkinleştirildikten sonra dikey penceresinde aşağıdaki bilgileri gösterecek şekilde genişletir:

![](media/azure-ad-pim-approval-workflow/image013.png)

>[!NOTE]
PRA(s) yok tüm onaylayanlar belirtirseniz, onaylayanlar varsayılan haline gelir. Bu rol için tüm etkinleştirme istekleri onaylamak için pra(s) gerekli olacak.

### <a name="specify-approver-users-andor-groups-to-approve-requests"></a>Onaylayanın kullanıcı ve/veya grupları istekleri onaylamak için belirtin.

Onay temsilci seçmek için "Select onaylayanları" seçeneğine tıklayın:

![](media/azure-ad-pim-approval-workflow/image015.png)

Onaylayanları seçin dikey penceresi yüklendiğinde, belirli kullanıcı veya grup üstündeki arama çubuğunu kullanarak ya da önceden doldurulmuş listeden seçerek arayın ve sonra "bittiğinde seçin" düğmesine tıklayın:

![](media/azure-ad-pim-approval-workflow/image017.png)

Not: Aynı anda birden çok kullanıcılar veya gruplar seçebilirsiniz.

Seçiminizi listesinde seçili onaylayanlar, aşağıda görüldüğü gibi görünür:

![](media/azure-ad-pim-approval-workflow/image019.png)

Approver kaldırmak için adlarının yanındaki Kaldır düğmesine tıklamanız yeterlidir.

Öğesini eklemek için işlemi tekrarlayın.

## <a name="view-request-and-approval-history-for-all-privileged-roles"></a>tüm ayrıcalıklı roller için istek ve onay geçmişini görüntüleme

Tüm ayrıcalıklı roller için istek ve onay geçmişini görüntülemek için panodan denetim Geçmişi'ni seçin:

![](media/azure-ad-pim-approval-workflow/image021.png)

>[!NOTE]
Verileri eyleme göre sıralayın ve "Etkinleştirme onaylı" arayın

### <a name="view-pending-approvals-requests"></a>Bekleyen onaylar (istek) görüntüleme

Onayınızı bekleyen istek olduğunda, bir temsilci onaylayan olarak e-posta bildirimleri alırsınız. PIM Portalı'nda bu istekleri görüntülemek için Panoda (Yeni Gezinti) "Bekleyen onay isteklerini" sekmesi sol gezinti çubuğunda seçin.

![](media/azure-ad-pim-approval-workflow/image023.png)

Burada, onay bekleyen istek listesi görürsünüz:

![](media/azure-ad-pim-approval-workflow/image024.png)

### <a name="approve-or-reject-requests-for-role-elevation-single-andor-bulk"></a>Rol yükseltme (tek ve/veya toplu) istekleri onaylayabileceğiniz veya

İstekleri onaylamak veya reddetmek istediğiniz seçin ve kararınızı ile karşılık gelen Eylem çubuğu düğmesine tıklayın:

![](media/azure-ad-pim-approval-workflow/image025.png)

### <a name="provide-justification-for-my-approvalrejection"></a>my onay/reddetme için gerekçe sağlayın

Bu onaylamak veya aynı anda birden fazla isteği reddetmek için yeni bir dikey pencere açar. Kararınız için bir gerekçe girin ve onaylayın (veya Reddet) alt ya da dikey penceresinde:

![](media/azure-ad-pim-approval-workflow/image029.png)

İstek işlemi tamamlandığında, durum simgesinde yaptığınız karar yansıtır (Bu örnekte, onaylama kararıdır):

![](media/azure-ad-pim-approval-workflow/image031.png)

### <a name="request-activation-of-a-role-that-requires-approval"></a>Etkinleştirme isteği onay gerektiren bir rolü

Rol etkinleştirmesi için işlem aynı kalır gibi onay gerektiren bir rolü etkinleştirmesi isteyen eski PIM gezinti ya da yeni gezintiyi başlatılabilir. Etkinleştirmek için roller listesinden bir rol seçmeniz yeterlidir:

![](media/azure-ad-pim-approval-workflow/image033.png)

Ayrıcalıklı rol için çok faktörlü kimlik doğrulaması gerektiriyorsa, bu görevin tamamlanması istenir:

![](media/azure-ad-pim-approval-workflow/image035.png)

Tamamlandığında, Etkinleştir'i tıklatın ve (gerekirse) bir gerekçe sağlayın:

![](media/azure-ad-pim-approval-workflow/image037.png)

İstek sahibinin istek onayı beklemede olan bir bildirim görürsünüz:

![](media/azure-ad-pim-approval-workflow/image039.png)

### <a name="view-the-status-of-your-request-to-activate"></a>İsteğiniz etkinleştirme durumunu görüntüleyin

Yeni Gezinti bölmesinden etkinleştirmek için bekleyen bir istek durumunu görüntüleme erişilmelidir. Sol gezinti çubuğundan "İsteklerim" sekmesini seçin:

![](media/azure-ad-pim-approval-workflow/image041.png)

İstek durumu "Bekliyor" varsayılan olarak, ancak tüm görmek için geçiş yapabilir veya reddedilen istek sayısı.

### <a name="complete-your-task-in-azure-ad-if-activation-was-approved"></a>Etkinleştirme onaylanırsa göreviniz, Azure AD'de tamamlayın.

İstek onaylandıktan sonra rol etkin ve bu rolü gerektiren herhangi bir iş ile devam edebilirsiniz.

![](media/azure-ad-pim-approval-workflow/image043.png)

## <a name="next-steps"></a>Sonraki adımlar

Geri bildiriminiz bizim için değerlidir. Lütfen bizimle buraya yorum veya geri bildirim paylaşma çekinmeyin!
