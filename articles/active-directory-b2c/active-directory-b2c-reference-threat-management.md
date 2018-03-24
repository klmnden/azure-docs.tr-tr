---
title: 'Azure Active Directory B2C: Tehdit Yönetimi | Microsoft Docs'
description: Hizmet reddi saldırılarını ve Azure Active Directory B2C parola saldırılarını algılama ve azaltma teknikleri hakkında bilgi edinin.
services: active-directory-b2c
documentationcenter: ''
author: davidmu1
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 03/27/2016
ms.author: davidmu
ms.openlocfilehash: 5ab699b0dccd772ec905699d94dedaca0eefcdad
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="azure-active-directory-b2c-threat-management"></a>Azure Active Directory B2C: Tehdit Yönetimi

Tehdit yönetimi, sistem ve ağ karşı saldırılarına karşı koruma için planlamayı içerir. Hizmet reddi saldırılarını kaynakları kullanılamıyor hedeflenen kullanıcılara yapabilir. Parola saldırılarını kaynaklara yetkisiz erişim sağlama. Azure Active Directory B2C (Azure AD B2C) verilerinizi birden çok yolla bu tehditlere karşı korumanıza yardımcı olabilecek yerleşik özelliklere sahiptir.

## <a name="denial-of-service-attacks"></a>Hizmet reddi saldırıları

Azure AD B2C algılama ve azaltma teknikler kullanır Eşitlemeye tanımlama bilgileri ve hizmet reddi saldırılarına karşı temel kaynakları korumak için oranı ve bağlantı sınırları ister.

## <a name="password-attacks"></a>Parola saldırıları

Azure AD B2C azaltma teknikleri parola saldırılarını için yerinde de vardır. Parola yanılma saldırıları ve sözlük parola saldırılarını azaltma içerir. Kullanıcılar tarafından ayarlanan parolaları makul karmaşık olması gerekir. Çeşitli sinyalleri kullanarak, Azure AD B2C isteklerinin bütünlüğünü analiz eder. Azure AD B2C, bilgisayar korsanlarını ve botnets hedeflenen kullanıcılar akıllıca ayırt etmek için tasarlanmıştır. Azure AD B2C, saldırının olasılığını içinde girilen parolalar göre kilit hesapları için Gelişmiş bir strateji sağlar.

Daha fazla bilgi için ziyaret [Microsoft Trust Center](https://www.microsoft.com/trustcenter/security/threatmanagement).
