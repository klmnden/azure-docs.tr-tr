---
title: Azure Active Directory kimlik koruması tarafından algılanan güvenlik açıklarını | Microsoft Docs
description: Azure Active Directory kimlik koruması tarafından algılanan güvenlik açıklarını genel bakış.
services: active-directory
keywords: Azure active directory kimlik koruması, bulut bulma, yönetme, uygulamaları, güvenlik, risk, risk düzeyi, güvenlik açığı, güvenlik ilkesi
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: daveba
ms.assetid: 92233a5b-cb34-4d28-88cc-d5d29c0f3256
ms.service: active-directory
ms.subservice: identity-protection
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2018
ms.author: joflore
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6fec9693641ff5918f622ecceee3fb94828b508e
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58517904"
---
# <a name="vulnerabilities-detected-by-azure-active-directory-identity-protection"></a>Azure Active Directory kimlik koruması tarafından algılanan güvenlik açıklarını
Zayıf bir saldırgan tarafından kötüye kullanılmadan, ortamınızdaki güvenlik açıklarıdır. Kuruluşunuzun güvenlik duruşunu bu güvenlik açıklarına değinen ve bunları yararlanmasını saldırganların önlemeye öneririz.


![güvenlik açıklarını](./media/vulnerabilities/101.png "güvenlik açıkları")



Aşağıdaki bölümler, kimlik koruması tarafından bildirilen güvenlik açıklarını genel bir bakış sağlar.

## <a name="multi-factor-authentication-registration-not-configured"></a>Çok faktörlü kimlik doğrulaması kaydı yapılandırılmadı
Bu güvenlik açığını kuruluşunuzdaki Azure multi-Factor Authentication dağıtımını denetlemenize yardımcı olur. 

Azure çok faktörlü kimlik doğrulaması, ikinci bir kullanıcı kimlik doğrulaması için güvenlik katmanı sağlar. Bu basit bir oturum açma işlemi için kullanıcı taleplerini karşılarken, verilere ve uygulamalara erişimi korunmasına yardımcı. Çeşitli kolay doğrulama seçenekleri aracılığıyla güçlü kimlik doğrulama sunar — telefon araması, SMS mesajı veya mobil uygulama bildirimi ya da doğrulama kodu ve üçüncü taraf OATH belirteçleri.

Azure multi-Factor Authentication kullanıcı oturum açma işlemleri için ihtiyacınız olan öneririz. Çok faktörlü kimlik doğrulaması risk tabanlı koşullu erişim ilkeleri kimlik koruması kullanılabilen önemli bir rol oynar.

Daha fazla bilgi için bkz. [Azure Multi-Factor Authentication nedir?](../authentication/multi-factor-authentication.md)

## <a name="unmanaged-cloud-apps"></a>Yönetilmeyen bulut uygulamaları
Bu güvenlik açığını yönetilmeyen bulut uygulamaları kuruluşunuzdaki belirlemenize yardımcı olur.

Modern kuruluşlar içinde BT departmanlarının genellikle, kuruluştaki kullanıcıların işlerini yapmak için kullandığınız tüm bulut uygulamaları farkında değildir. Yöneticiler şirket verilerini, olası veri sızıntılarına ve diğer güvenlik risklerini yetkisiz erişimi ile ilgili endişelerini neden gerekir görmek daha kolaydır. 

Yönetilmeyen bulut uygulamaları keşfetmek için ve Azure Active Directory'yi kullanarak bu uygulamaları yönetmek için Cloud Discovery'yi dağıtma tavsiye ederiz.

Daha fazla bilgi için [Cloud Discovery](/cloud-app-security/set-up-cloud-discovery).

## <a name="security-alerts-from-privileged-identity-management"></a>Privileged Identity Management'tan Güvenlik Uyarıları
Bu güvenlik açığını keşfedin ve uyarılar, kuruluşunuzda ayrıcalıklı kimlikleri hakkında çözmenize yardımcı olur.  

Ayrıcalıklı işlemleri gerçekleştirmek kullanıcıları etkinleştirmek için Azure AD'de kullanıcılara geçici veya kalıcı ayrıcalıklı erişim vermek kuruluşların gereken kaynakları Azure veya Office 365 veya diğer SaaS uygulamalarını. Her biri bu ayrıcalıklı kullanıcılar, kuruluşunuzun saldırı yüzeyini artırır. Bu güvenlik açığını gereksiz ayrıcalıklı erişimi kullanıcıları tanımlamak ve azaltmak veya bunlar konusunda sizi uyarmayı riskini ortadan kaldırmak için uygun eylemde yardımcı olur. 

Kuruluşunuz, denetimi yönetmek için Azure AD Privileged Identity Management kullanır ve Office 365 veya Microsoft Intune gibi diğer Microsoft çevrimiçi hizmetlerine yanı sıra kimlikler ve bunların Azure ad'deki kaynaklara erişimi İzleyici ayrıcalıklı öneririz.

Daha fazla bilgi için [Azure AD Privileged Identity Management](../privileged-identity-management/pim-configure.md). 

## <a name="see-also"></a>Ayrıca bkz.

[Azure Active Directory kimlik koruması](../active-directory-identityprotection.md)

