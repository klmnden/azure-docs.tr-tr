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
ms.component: compliance-reports
ms.date: 01/15/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: d0638404ec6f5b6d13aa207ef54913c1bd3ecc1a
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36232595"
---
# <a name="i-cant-find-any-data-in-the-azure-active-directory-activity-logs-i-downloaded"></a>Herhangi bir veri karşıdan Azure Active Directory etkinlik günlükleri bulunamıyor


## <a name="symptoms"></a>Belirtiler

Etkinlik günlüklerini (denetim veya oturum açma) indirdim ve seçtiğim süre için tüm kayıtları göremiyorum. Neden? 

 ![Raporlama](./media/active-directory-reporting-troubleshoot-missing-data-download/01.png)
 

## <a name="cause"></a>Nedeni

Azure portalında etkinlik günlükleri yüklediğinizde, biz 5000 kayıtları, en son ilk tarafından sıralanan Ölçekle sınırlayın. 

## <a name="resolution"></a>Çözüm

Belirli bir noktadaki bir milyon kaydı getirmek için [Azure AD Raporlama API’lerini](active-directory-reporting-api-getting-started.md) kullanabilirsiniz. Önerilen yaklaşımımız kayıtları artımlı bir şekilde bir süre (örneğin, günlük veya haftalık) almak için raporlama API'lerini çağırır bir zamanlama temelinde bir komut çalıştırmaktır.

## <a name="next-steps"></a>Sonraki adımlar
Bkz. [Azure Active Directory raporlama hakkında SSS](active-directory-reporting-faq.md).

