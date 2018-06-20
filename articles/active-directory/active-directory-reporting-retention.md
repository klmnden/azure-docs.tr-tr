---
title: Azure Active Directory rapor bekletme ilkeleri | Microsoft Docs
description: Rapor verileri Azure Active Directory'de bekletme ilkeleri
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
ms.component: compliance-reports
ms.date: 05/10/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 8edce7d1a633148cf361deec0c3d09ef4c16ac9b
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36225187"
---
# <a name="azure-active-directory-report-retention-policies"></a>Azure Active Directory rapor bekletme ilkeleri


Bu makalede, Azure Active Directory'de farklı etkinlik raporları için veri saklama ile birlikte en yaygın sorulara yanıtlar sağlar. 

### <a name="q-how-can-you-get-the-collection-of-activity-data-started"></a>S: nasıl başlatılan etkinliği veri koleksiyonunu alabilir miyim?

**A:**

| Azure AD Edition | Koleksiyon Başlat |
| :--              | :--   |
| Azure AD Premium P1 <br /> Azure AD Premium P2 | Ne zaman bir abonelik için kaydolun |
| Azure AD Ücretsiz | İlk açtığınızda [Azure Active Directory dikey](https://ms.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) veya [API'leri raporlama](https://aka.ms/aadreports)  |

---
### <a name="q-when-is-your-activity-data-available-in-the-azure-portal"></a>S: etkinlik verilerinizi Azure portalında kullanılabilir olduğunda?

**A:**

- **Hemen** -, Azure portalında raporları ile birlikte zaten çalışmaktadır.
- **2 saat içinde** - Azure portalında raporlama açmadıysanız.

---

### <a name="q-how-can-you-get-the-collection-of-security-signals-started"></a>S: nasıl başlatılan güvenlik sinyal koleksiyonu alabilir miyim?  

**Y:** , kimlik koruma Merkezi'ni kullanmayı katılımı güvenlik sinyalleri için toplama işlemi başlatılır. 


---

### <a name="q-for-how-long-is-the-collected-data-stored"></a>S: toplanan veriler ne kadar süreyle depolanır?

**A:**

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
