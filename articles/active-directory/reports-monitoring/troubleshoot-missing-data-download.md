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
ms.component: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: ea7ef4eb75cc7ffdf951e8d8799e898bfc31948b
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54818209"
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

