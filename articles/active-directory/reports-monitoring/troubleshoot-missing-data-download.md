---
title: 'Sorun giderme: İndirilen Azure Active Directory etkinlik günlüklerindeki eksik veriler | Microsoft Docs'
description: İndirilen Azure Active Directory etkinlik günlüklerindeki eksik verilere yönelik bir çözüm sağlar.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: ffce7eb1-99da-4ea7-9c4d-2322b755c8ce
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 01/15/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 9138da42eeb87e45b86be10aff67792ee6de09b4
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "42056555"
---
# <a name="i-cant-find-any-data-in-the-azure-active-directory-activity-logs-i-downloaded"></a>Herhangi bir veri karşıdan Azure Active Directory etkinlik günlüğünde bulamıyorum


## <a name="symptoms"></a>Belirtiler

Etkinlik günlüklerini (denetim veya oturum açma) indirdim ve seçtiğim süre için tüm kayıtları göremiyorum. Neden? 

 ![Raporlama](./media/troubleshoot-missing-data-download/01.png)
 

## <a name="cause"></a>Nedeni

Azure Portal'da etkinlik günlüklerini indirdiğinizde, en son gerçekleşen en başta tarafından sıralanan 5000 kayıt ölçek sınırlıyoruz. 

## <a name="resolution"></a>Çözüm

Belirli bir noktadaki bir milyon kaydı getirmek için [Azure AD Raporlama API’lerini](concept-reporting-api.md) kullanabilirsiniz. Bizim önerdiğimiz yaklaşım, bir zamanlamaya göre bir sürede (örneğin, günlük veya haftalık) artımlı bir şekilde kayıt getirilecek raporlama API'lerini çağıran bir komut çalıştırmaktır.

## <a name="next-steps"></a>Sonraki adımlar
Bkz. [Azure Active Directory raporlama hakkında SSS](reports-faq.md).

