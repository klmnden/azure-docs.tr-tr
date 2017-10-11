---
title: "Azure Active Directory B2C: Tehdit Yönetimi | Microsoft Docs"
description: "Hizmet reddi saldırılarını ve Azure Active Directory B2C parola saldırılarını algılama ve azaltma teknikleri hakkında bilgi edinin."
services: active-directory-b2c
documentationcenter: 
author: vigunase
manager: Ajith Alexander
editor: 
ms.assetid: 6df79878-65cb-4dfc-98bb-2b328055bc2e
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2016
ms.author: 
ms.openlocfilehash: 9472cb01eb713e297053727b1a314293574bb657
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="azure-active-directory-b2c-threat-management"></a>Azure Active Directory B2C: Tehdit Yönetimi

Tehdit yönetimi, sistem ve ağ karşı saldırılarına karşı koruma için planlamayı içerir. Hizmet reddi saldırılarını kaynakları kullanılamıyor hedeflenen kullanıcılara yapabilir. Parola saldırılarını kaynaklara yetkisiz erişim sağlama. Azure Active Directory B2C (Azure AD B2C) verilerinizi birden çok yolla bu tehditlere karşı korumanıza yardımcı olabilecek yerleşik özelliklere sahiptir.

## <a name="denial-of-service-attacks"></a>Hizmet reddi saldırıları

Azure AD B2C algılama ve azaltma teknikler kullanır Eşitlemeye tanımlama bilgileri ve hizmet reddi saldırılarına karşı temel kaynakları korumak için oranı ve bağlantı sınırları ister.

## <a name="password-attacks"></a>Parola saldırıları

Azure AD B2C azaltma teknikleri parola saldırılarını için yerinde de vardır. Parola yanılma saldırıları ve sözlük parola saldırılarını azaltma içerir. Kullanıcılar tarafından ayarlanan parolaları makul karmaşık olması gerekir. Çeşitli sinyalleri kullanarak, Azure AD B2C isteklerinin bütünlüğünü analiz eder. Azure AD B2C, bilgisayar korsanlarını ve botnets hedeflenen kullanıcılar akıllıca ayırt etmek için tasarlanmıştır. Azure AD B2C, saldırının olasılığını içinde girilen parolalar göre kilit hesapları için Gelişmiş bir strateji sağlar.

Daha fazla bilgi için ziyaret [Microsoft Trust Center](https://www.microsoft.com/trustcenter/security/threatmanagement).
