---
title: "Sorun giderme: İndirilen Azure Active Directory etkinlik günlüklerindeki eksik veriler - önizleme | Microsoft Belgeleri"
description: "İndirilen Azure Active Directory etkinlik günlüklerindeki eksik veriler önizlemesine yönelik bir çözüm sağlar."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: ffce7eb1-99da-4ea7-9c4d-2322b755c8ce
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/09/2017
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: 8a531f70f0d9e173d6ea9fb72b9c997f73c23244
ms.openlocfilehash: e0d65edcb7c14114565402038b0958c3a2ffb477
ms.lasthandoff: 03/10/2017


---

# <a name="i-cant-find-any-data-in-the-azure-active-directory-activity-logs-i-have-downloaded"></a>İndirdiğim Azure Active Directory etkinlik günlüklerinde hiçbir veri bulamıyorum


## <a name="symptoms"></a>Belirtiler

Etkinlik günlüklerini (denetim veya oturum açma) indirdim ve seçtiğim süre için tüm kayıtları göremiyorum. Neden? 

 ![Raporlama](./media/active-directory-reporting-troubleshoot-missing-data-download/01.png)
 

## <a name="cause"></a>Nedeni

Azure portalında etkinlik günlüklerini indirdiğinizde ölçek, en yeniye göre sıralanmış 120.000 kayıtla sınırlanır. 

## <a name="resolution"></a>Çözüm

Belirli bir noktadaki bir milyon kaydı getirmek için [Azure AD Raporlama API’lerini](active-directory-reporting-api-getting-started.md) kullanabilirsiniz. Kayıtları belirli bir süre içinde (örn. günlük veya haftalık) artımlı bir şekilde getirmek üzere, belirli bir zamanlamaya göre raporlama API’lerini çağıran bir betik çalıştırmanız önerilen bir yaklaşımdır.

## <a name="next-steps"></a>Sonraki adımlar
Bkz. [Azure Active Directory raporlama hakkında SSS](active-directory-reporting-faq.md).


