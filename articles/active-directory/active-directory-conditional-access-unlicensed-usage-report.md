---
title: "Lisanssız kullanım raporu | Microsoft Docs"
description: "Kullanmakta olduğunuz lisanssız kullanıcıları tanımlamak lisanssız kullanım raporu yardımcı olur, Azure AD özelliklerini Ücretli."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 92138f43-9528-4c8a-b834-66a47da476e3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: markvi
ms.openlocfilehash: c0b4f455f067e825362bdecc02ea62d7984f0d31
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="unlicensed-usage-report"></a>Lisanssız kullanım raporu
Kullanmakta olduğunuz lisanssız kullanıcıları tanımlamak lisanssız kullanım raporu yardımcı olur, Azure AD özelliklerini Ücretli. Bu, daha iyi hale getirmek için satın aldığınız lisansların kullanmanız ve Ek lisanslar gerekebilir tanımlamak için çalıştığınızı sağlar. 

Rapor son 30 gün içinde Ücretli özellikler etkin kullanımını gösterir. 

## <a name="report-structure"></a>Rapor yapısı
| Sütun adı | Açıklama |
|:--- |:--- |
| Lisanssız kullanıcı |Kullanıcının adı |
| Özellik |Özellik adı. Örneğin: koşullu erişim |
| Erişilen uygulama |Özelliği ile erişilen uygulamanın adı. Örneğin: Office 365 SharePoint Online |

> [!NOTE]
> Bir kullanıcı hesabı silindiyse 'Lisanssız kullanıcı' sütunu, 1003000090D8B285 gibi bir kimliği kullanılarak doldurulur.
> 
> 

## <a name="conditional-access-feature"></a>Koşullu erişim özelliği
Bir Azure AD Premium lisansı yoksa uygulanan koşullu erişim ilkesine sahip bir hizmeti eriştiklerinde lisanssız kullanıcılar işaretlenir. 

Bu MFA için geçerlidir / aygıt yanı sıra konumu ilkeleri ilkeler, Intune kullanın.

## <a name="see-also"></a>Ayrıca bkz.
* [Office 365 ve diğer Azure Active Directory ile koşullu erişim kullanarak bağlı uygulamalar](active-directory-conditional-access.md)
* [Azure AD koşullu erişimi kullanmaya başlama](active-directory-conditional-access-azuread-connected-apps.md) 

