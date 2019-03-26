---
title: Azure Active Directory rapor saklama ilkeleri | Microsoft Docs
description: Azure Active Directory rapor verilerini bekletme ilkeleri
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 183e53b0-0647-42e7-8abe-3e9ff424de12
ms.service: active-directory
ms.devlang: ''
ms.topic: reference
ms.tgt_pltfrm: ''
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 126eee1664d810850f63e702abe78b241649d665
ms.sourcegitcommit: 70550d278cda4355adffe9c66d920919448b0c34
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58438827"
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

### <a name="how-soon-can-i-see-activities-data-after-getting-a-premium-license"></a>Etkinlikler verileri bir premium lisansı aldıktan sonra en kısa sürede nasıl görebilirim?

Etkinlikleri veriler, ücretsiz lisansa zaten varsa, daha sonra bunu hemen yükseltmeyi görebilirsiniz. Herhangi bir veri yoksa, raporlarda görünmesi için bir premium lisansı yükselttikten sonra verilerin bir veya iki gün sürebilir.

---

### <a name="can-i-see-last-months-data-after-getting-an-azure-ad-premium-license"></a>Son aya ilişkin verileri bir Azure AD premium lisansı aldıktan sonra görebilir miyim?

(Deneme sürümü dahil) bir premium sürümü için yakın zamanda geçtiyseniz, veri yedekleme 7 gün için başlangıçta görebilirsiniz. Verileri toplanır, son 30 güne ilişkin verileri görebilirsiniz.

---

### <a name="when-does-azure-ad-start-collecting-security-signal-data"></a>Azure AD güvenlik sinyal verileri toplama ne zaman başlar?  

Kullanılacak katılımı, güvenlik sinyalleri için toplama işlemi başlar **kimlik koruma Merkezi**. 

---

### <a name="how-long-does-azure-ad-store-the-data"></a>Azure AD verileri ne kadar süreyle depoluyor mu?

**Etkinlik raporları**    

| Rapor                 | Azure AD Ücretsiz | Azure AD Basic | Azure AD Premium P1 | Azure AD Premium P2 |
| :--                    | :--           | :--            | :--                 | :--                 |
| Dizin Denetimi        | 7 gün        |  7 gün        | 30 gün             | 30 gün             |
| Oturum Açma Etkinliği       | Yok           |  Yok           | 30 gün             | 30 gün             |
| Azure MFA kullanımı        | 30 gün       |  30 gün       | 30 gün             | 30 gün             |

Azure İzleyicisi'ni kullanarak bir Azure depolama hesabına yönlendirerek yukarıda özetlenen varsayılan saklama süresinden daha uzun denetim ve oturum açma etkinliği verileri koruyabilirsiniz. Daha fazla bilgi için [arşiv Azure AD için bir Azure depolama hesabı günlükleri](quickstart-azure-monitor-route-logs-to-storage-account.md).

**Güvenlik sinyalleri**

| Rapor         | Azure AD Ücretsiz | Azure AD Basic | Azure AD Premium P1 | Azure AD Premium P2 |
| :--            | :--           | :--            | :--                 | :--                 |
| Risk altındaki kullanıcılar  | 7 gün        | 7 gün         | 30 gün             | 90 gün             |
| Riskli oturum açma işlemleri | 7 gün        | 7 gün         |  30 gün            | 90 gün             |

---
