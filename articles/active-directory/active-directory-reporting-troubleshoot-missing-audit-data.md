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
ms.date: 07/15/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.translationtype: Human Translation
ms.sourcegitcommit: 9ae7e129b381d3034433e29ac1f74cb843cb5aa6
ms.openlocfilehash: 18af51e95a283a5cd33688484a0d7477eb4b957d
ms.contentlocale: tr-tr
ms.lasthandoff: 05/08/2017

---

# Azure Active Directory etkinlik günlüğünde gerçekleştirdiğim bazı eylemleri bulamıyorum
<a id="i-cant-find-some-actions-that-i-performed-in-the-azure-active-directory-activity-log" class="xliff"></a>


## Belirtiler
<a id="symptoms" class="xliff"></a>

Azure portalında bazı eylemler gerçekleştirdim ve bu eylemlerin denetim günlüklerini `Activity logs > Audit Logs` dikey penceresinde görmeyi umuyordum, ancak bulamıyorum.

 ![Raporlama](./media/active-directory-reporting-troubleshoot-missing-audit-data/01.png)
 

## Nedeni
<a id="cause" class="xliff"></a>

Eylemler, Etkinlik Denetim günlüğünde hemen görünmez. Denetim günlüklerinin portalda görünmesi, işlemin gerçekleştirilmesinden sonra 15 dakika ile 1 saat arasında sürebilir.

## Çözüm
<a id="resolution" class="xliff"></a>

15 dakika bekleyin ve eylemlerin günlükte görüntülenip görüntülenmediğine bakın. Hala görmüyorsanız, lütfen bir destek bileti gönderin ve sorunu inceleyelim.


## Sonraki adımlar
<a id="next-steps" class="xliff"></a>
Bkz. [Azure Active Directory raporlama hakkında SSS](active-directory-reporting-faq.md).


