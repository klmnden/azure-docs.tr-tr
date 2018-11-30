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
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: abd64b7d2fa7930f5b6177c7ac037840da34dc18
ms.sourcegitcommit: 922f7a8b75e9e15a17e904cc941bdfb0f32dc153
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/27/2018
ms.locfileid: "52333590"
---
# <a name="azure-active-directory-report-retention-policies"></a>Azure Active Directory rapor saklama ilkeleri

Bu makalede, Azure Active Directory'de farklı bir etkinlik raporları için veri saklama ilkeleri hakkında bilgi edinin. 

### <a name="when-does-azure-ad-start-collecting-data"></a>Azure AD veri toplamayı ne zaman başlar?

| Azure AD sürümü | Koleksiyonu Başlat |
| :--              | :--   |
| Azure AD Premium P1 <br /> Azure AD Premium P2 | Ne zaman bir abonelik için kaydolun |
| Azure AD Ücretsiz <br /> Azure AD Basic | İlk açtığınızda [Azure Active Directory dikey](https://ms.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) veya [API'leri raporlama](https://aka.ms/aadreports)  |

---

### <a name="when-is-the-activity-data-available-in-the-azure-portal"></a>Azure portalında etkinlik verilerini ne zaman kullanılabilir?

- **Hemen** -, Azure portalında raporlar ile zaten çalışmaktadır.
- **2 saat içinde** - Azure portalında raporlama açmadıysanız.

---

### <a name="when-does-azure-ad-start-collecting-security-signal-data"></a>Azure AD güvenlik sinyal verileri toplama ne zaman başlar?  

Kullanılacak katılımı, güvenlik sinyalleri için toplama işlemi başlar **kimlik koruma Merkezi**. 

---

### <a name="how-long-does-azure-ad-store-the-data"></a>Azure AD verileri ne kadar süreyle depoluyor mu?

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
