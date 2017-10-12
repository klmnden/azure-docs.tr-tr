---
title: "Sorun giderme: İndirilen Azure Active Directory etkinlik günlüklerindeki eksik veriler | Microsoft Docs"
description: "İndirilen Azure Active Directory etkinlik günlüklerindeki eksik verilere yönelik bir çözüm sağlar."
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
ms.date: 07/15/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 3d56f89035da4d1a0074256b165663f81fc2b01e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
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

