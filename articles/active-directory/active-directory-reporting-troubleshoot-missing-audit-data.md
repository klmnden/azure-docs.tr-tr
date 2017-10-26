---
title: "Sorun giderme: Azure Active Directory etkinlik günlüklerindeki eksik veriler | Microsoft Docs"
description: "Azure Active Directory’de kullanılabilen çeşitli raporları listeler"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 7cbe4337-bb77-4ee0-b254-3e368be06db7
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/21/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 4e63c9b48125e3c7283be5fe88cab211509f88f4
ms.sourcegitcommit: 4ed3fe11c138eeed19aef0315a4f470f447eac0c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2017
---
# <a name="i-cant-find-some-actions-that-i-performed-in-the-azure-active-directory-activity-log"></a>Azure Active Directory etkinlik günlüğünde gerçekleştirdiğim bazı eylemleri bulamıyorum


## <a name="symptoms"></a>Belirtiler

Azure portalında bazı eylemler gerçekleştirdim ve bu eylemlerin denetim günlüklerini `Activity logs > Audit Logs` dikey penceresinde görmeyi umuyordum, ancak bulamıyorum.

 ![Raporlama](./media/active-directory-reporting-troubleshoot-missing-audit-data/01.png)
 

## <a name="cause"></a>Nedeni

Eylemler, Etkinlik Denetim günlüğünde hemen görünmez. Denetim günlüklerinin portalda görünmesi, işlemin gerçekleştirilmesinden sonra 15 dakika ile 1 saat arasında sürebilir.

## <a name="resolution"></a>Çözüm

15 dakika bekleyin ve eylemlerin günlükte görüntülenip görüntülenmediğine bakın. Hala görmüyorsanız, lütfen bir destek bileti gönderin ve sorunu inceleyelim.


## <a name="next-steps"></a>Sonraki adımlar
Bkz. [Azure Active Directory raporlama hakkında SSS](active-directory-reporting-faq.md).

