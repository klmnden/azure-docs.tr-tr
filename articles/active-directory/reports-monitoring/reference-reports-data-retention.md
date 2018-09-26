---
title: Azure Active Directory rapor saklama ilkeleri | Microsoft Docs
description: Azure Active Directory rapor verilerini bekletme ilkeleri
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 183e53b0-0647-42e7-8abe-3e9ff424de12
ms.service: active-directory
ms.devlang: ''
ms.topic: reference
ms.tgt_pltfrm: ''
ms.workload: identity
ms.component: report-monitor
ms.date: 05/10/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 68028fd1ba116251860e5c370e9e9ce61fd314bb
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47107115"
---
# <a name="azure-active-directory-report-retention-policies"></a>Azure Active Directory rapor saklama ilkeleri


Bu makalede, Azure Active Directory'de farklı bir etkinlik raporları için veri saklama ile birlikte en yaygın soruların yanıtlarını sağlar. 

### <a name="q-how-can-you-get-the-collection-of-activity-data-started"></a>S: nasıl çalışmaya etkinlik verileri alabilir?

**Y:**

| Azure AD sürümü | Koleksiyonu Başlat |
| :--              | :--   |
| Azure AD Premium P1 <br /> Azure AD Premium P2 | Ne zaman bir abonelik için kaydolun |
| Azure AD Ücretsiz | İlk açtığınızda [Azure Active Directory dikey](https://ms.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) veya [API'leri raporlama](https://aka.ms/aadreports)  |

---
### <a name="q-when-is-your-activity-data-available-in-the-azure-portal"></a>S: ne zaman etkinlik verilerinizi Azure portalında kullanılabilir mi?

**Y:**

- **Hemen** -, Azure portalında raporlar ile zaten çalışmaktadır.
- **2 saat içinde** - Azure portalında raporlama açmadıysanız.

---

### <a name="q-how-can-you-get-the-collection-of-security-signals-started"></a>S: nasıl çalışmaya güvenlik sinyal koleksiyonu alabilir?  

**Y:** , kimlik koruma merkezi kullanılacak katılımı, güvenlik sinyalleri için toplama işlemi başlar. 


---

### <a name="q-for-how-long-is-the-collected-data-stored"></a>S: toplanan verileri için ne kadar süreyle saklanır?

**Y:**

**Etkinlik raporları**    

| Rapor                 | Azure AD Ücretsiz | Azure AD Premium P1 | Azure AD Premium P2 |
| :--                    | :--           | :--                 | :--                 |
| Dizin Denetimi        | 7 gün        | 30 gün             | 30 gün             |
| Oturum Açma Etkinliği       | Yok           | 30 gün             | 30 gün             |
| Azure MFA kullanımı        | 30 gün       | 30 gün             | 30 gün             |

**Güvenlik sinyalleri**

| Rapor         | Azure AD Ücretsiz | Azure AD Premium P1 | Azure AD Premium P2 |
| :--            | :--           | :--                 | :--                 |
| Risk altındaki kullanıcılar  | 7 gün        | 30 gün             | 90 gün             |
| Riskli oturum açma işlemleri | 7 gün        | 30 gün             | 90 gün             |

---
