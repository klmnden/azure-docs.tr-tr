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
ms.collection: M365-identity-device-management
ms.openlocfilehash: cfb56ea81abeeba83bee73356c682b3e9fae866f
ms.sourcegitcommit: ab6fa92977255c5ecbe8a53cac61c2cd2a11601f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58292927"
---
# <a name="i-cant-find-all-the-data-in-the-azure-active-directory-activity-logs-i-downloaded"></a>Tüm verileri karşıdan Azure Active Directory etkinlik günlüğünde bulamıyorum

## <a name="symptoms"></a>Belirtiler

Etkinlik günlüklerini (denetim veya oturum açma) indirdim ve seçtiğim süre için tüm kayıtları göremiyorum. Neden? 

 ![Raporlama](./media/troubleshoot-missing-data-download/01.png)
 
## <a name="cause"></a>Nedeni

Azure Portal'da etkinlik günlüklerini indirdiğinizde ölçek en son gerçekleşen en başta göre sıralanmış, 250.000 kayıtlara sınırlıyoruz. 

## <a name="resolution"></a>Çözüm

Belirli bir noktadaki bir milyon kaydı getirmek için [Azure AD Raporlama API’lerini](concept-reporting-api.md) kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Active Directory SSS raporları](reports-faq.md)

