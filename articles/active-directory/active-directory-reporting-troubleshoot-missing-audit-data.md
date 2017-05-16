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
ms.date: 05/04/2017
ms.author: markvi
ms.translationtype: Human Translation
ms.sourcegitcommit: 9ae7e129b381d3034433e29ac1f74cb843cb5aa6
ms.openlocfilehash: 18af51e95a283a5cd33688484a0d7477eb4b957d
ms.contentlocale: tr-tr
ms.lasthandoff: 05/08/2017


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


