---
title: 'Sorun giderme: Veri indirilen Azure Active Directory etkinlik günlüklerindeki eksik | Microsoft Docs'
description: İndirilen Azure Active Directory etkinlik günlüklerindeki eksik verilere yönelik bir çözüm sağlar.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
editor: ''
ms.assetid: ffce7eb1-99da-4ea7-9c4d-2322b755c8ce
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 3cf6296a5cf6bdf8697344595c0ead0908659c3f
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55197475"
---
# <a name="i-cant-find-all-the-data-in-the-azure-active-directory-activity-logs-i-downloaded"></a>Tüm verileri karşıdan Azure Active Directory etkinlik günlüğünde bulamıyorum

## <a name="symptoms"></a>Belirtiler

Etkinlik günlüklerini (denetim veya oturum açma) indirdim ve seçtiğim süre için tüm kayıtları göremiyorum. Neden? 

 ![Raporlama](./media/troubleshoot-missing-data-download/01.png)
 
## <a name="cause"></a>Nedeni

Azure Portal'da etkinlik günlüklerini indirdiğinizde, en son gerçekleşen en başta tarafından sıralanan 5000 kayıt ölçek sınırlıyoruz. 

## <a name="resolution"></a>Çözüm

Belirli bir noktadaki bir milyon kaydı getirmek için [Azure AD Raporlama API’lerini](concept-reporting-api.md) kullanabilirsiniz. Bizim önerdiğimiz yaklaşım olmaktır [bir zamanlamaya göre Çalıştır](tutorial-signin-logs-download-script.md) bir sürede (örneğin, günlük veya haftalık) artımlı bir şekilde kayıt getirilecek raporlama API'lerini çağırır. 

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Active Directory SSS raporları](reports-faq.md)

