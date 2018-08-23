---
title: Tehdit Yönetimi Azure Active Directory B2C | Microsoft Docs
description: Hizmet reddi saldırılarını ve Azure Active Directory B2C parola saldırıları algılama ve Önleme teknikleri hakkında bilgi edinin.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/27/2016
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 1801fe9695aa15850d600300b957df2c7d7cd9ef
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2018
ms.locfileid: "42057444"
---
# <a name="azure-active-directory-b2c-threat-management"></a>Azure Active Directory B2C: Tehdit Yönetimi

Tehdit yönetimi, sistem ve ağ saldırılarına karşı koruma için planlamayı içerir. Hizmet reddi saldırılarını kaynakları hedeflenen kullanıcılara kullanımdan. Kaynaklara yetkisiz erişim için parola saldırılarına yol. Azure Active Directory B2C (Azure AD B2C), verilerinizi birden çok yolla bu tehditlere karşı korumanıza yardımcı olur yerleşik özelliklere sahiptir.

## <a name="denial-of-service-attacks"></a>Hizmet reddi saldırılarını

Azure AD B2C kullanır algılama ve Önleme teknikleri SYN tanımlama bilgileri ve temel alınan kaynakları hizmet reddi saldırılarına karşı korumak için oranı ve bağlantı sınırları gibi.

## <a name="password-attacks"></a>Parola saldırıları

Azure AD B2C azaltma teknikleri parola saldırıları için bir yerde de vardır. Parola deneme yanılma saldırıları ve sözlük parola saldırılarını azaltma içerir. Kullanıcılar tarafından ayarlanan parolaların makul karmaşık olması gerekmez. Çeşitli sinyalleri kullanarak, Azure AD B2C isteklerinin bütünlüğünü analiz eder. Azure AD B2C, hedeflenen kullanıcıların bilgisayar korsanlarının ve botnet akıllı bir şekilde ayırt etmek için tasarlanmıştır. Azure AD B2C, karmaşık bir strateji, bir saldırı olasılığını girilen parolaları temel hesapları kilitlemek için sağlar.

Daha fazla bilgi için ziyaret [Microsoft Trust Center](https://www.microsoft.com/trustcenter/default.aspx).
