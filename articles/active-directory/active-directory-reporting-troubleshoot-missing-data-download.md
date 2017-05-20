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
ms.date: 05/04/2017
ms.author: markvi
ms.translationtype: Human Translation
ms.sourcegitcommit: 9ae7e129b381d3034433e29ac1f74cb843cb5aa6
ms.openlocfilehash: 9109c698e4e8b43eeb7534c338adc99476012a3f
ms.contentlocale: tr-tr
ms.lasthandoff: 05/08/2017


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


