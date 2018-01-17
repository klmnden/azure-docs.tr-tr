---
title: "Azure Active Directory kimlik koruması tarafından algılanan güvenlik açığı | Microsoft Docs"
description: "Azure Active Directory kimlik koruması tarafından algılanan güvenlik açığı genel bakış."
services: active-directory
keywords: "Azure active directory kimlik koruması, cloud app discovery'yi, uygulamalar, güvenlik, risk, risk düzeyi, güvenlik açığı, güvenlik ilkesi yönetme"
documentationcenter: 
author: MarkusVi
manager: mtillman
ms.assetid: 92233a5b-cb34-4d28-88cc-d5d29c0f3256
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 65b1ae76794c812f9fcf2955d09e023195ef6342
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2018
---
# <a name="vulnerabilities-detected-by-azure-active-directory-identity-protection"></a>Azure Active Directory kimlik koruması tarafından algılanan güvenlik açığı
Güvenlik açıkları zayıf bir saldırgan tarafından yararlanılabilir ortamınızda giderilmiştir. Kuruluşunuzun güvenlik tutumunu artırmak için bu güvenlik açıklarına ve bunları yararlanmasını saldırganların önlemeye öneririz.


![Güvenlik Açıkları](./media/active-directory-identityprotection-vulnerabilities/101.png "güvenlik açıkları")



Aşağıdaki bölümler kimlik koruması tarafından bildirilen güvenlik açıkları genel bir bakış sağlar.

## <a name="multi-factor-authentication-registration-not-configured"></a>Çok faktörlü kimlik doğrulaması kaydı yapılandırılmadı
Bu güvenlik açığından, kuruluşunuzda Azure multi-Factor Authentication dağıtımı denetlemenize yardımcı olur. 

Azure çok faktörlü kimlik doğrulaması ikinci bir kullanıcı kimlik doğrulaması için güvenlik katmanı sağlar. Bu basit bir oturum açma işlemi için kullanıcı talebine buluştururken veri ve uygulamalara erişimi korunmasına yardımcı. Kolay doğrulama seçeneklerini çeşitli aracılığıyla güçlü kimlik doğrulaması sunar — telefon araması, SMS mesajı veya mobil uygulama bildirimi veya doğrulama kodu ve 3. taraf OATH belirteçleri.

Azure multi-Factor Authentication kullanıcı oturum açma işlemleri için gerekli öneririz. Çok faktörlü kimlik doğrulaması risk bağlı olarak koşullu erişim ilkelerindeki kimlik koruması kullanılabilir önemli bir rol oynar.

Daha fazla ayrıntı için bkz: [Azure multi-Factor Authentication nedir?](../multi-factor-authentication/multi-factor-authentication.md)

## <a name="unmanaged-cloud-apps"></a>Yönetilmeyen bulut uygulamaları
Bu güvenlik açığından kuruluşunuzdaki yönetilmeyen bulut uygulamaları tanımlamasına yardımcı olur.

Modern kuruluşlarda, BT departmanları genellikle kuruluşlarındaki kullanıcılar işlerini yapmak için kullandığınız tüm bulut uygulamaları farkında değildir. Yöneticileri Kurumsal verileri, olası veri sıkıntılarına ve diğer güvenlik risklerine yetkisiz erişimi endişeniz neden olurdu görmek kolaydır. 

Kuruluşunuzun Cloud App Discovery yönetilmeyen bulut uygulamaları bulmak ve Azure Active Directory'yi kullanarak bu uygulamaları yönetmek için dağıtmanızı öneririz.

Daha fazla ayrıntı için bkz: [Cloud App Discovery ile yönetilmeyen bulut uygulamaları bulma](active-directory-cloudappdiscovery-whatis.md).

## <a name="security-alerts-from-privileged-identity-management"></a>Privileged Identity Management'tan Güvenlik Uyarıları
Bu güvenlik açığından bulmak ve uyarılar, kuruluşunuzda ayrıcalıklı kimlikleri hakkında çözümlemenize yardımcı olur.  

Ayrıcalıklı işlemleri gerçekleştirmek kullanıcıları etkinleştirmek için Azure AD'de kullanıcıları geçici veya kalıcı ayrıcalıklı erişim vermek kuruluş gerekir Azure veya Office 365 kaynakları veya diğer SaaS uygulamaları. Her bu ayrıcalıklı kullanıcılar, kuruluşunuzun saldırı yüzeyini artırır. Bu güvenlik açığından gereksiz ayrıcalıklı erişimi kullanıcıları tanımlamak ve azaltın veya bunlar teşkil riski ortadan kaldırmak için uygun eylemi gerçekleştirin yardımcı olur. 

Kuruluşunuz, denetimi yönetmek için Azure AD Privileged Identity Management kullanır ve Office 365 veya Microsoft Intune gibi diğer Microsoft online services yanı sıra kimlikleri ve bunların Azure ad'deki kaynaklara erişimi İzleyici ayrıcalıklı öneririz.

Daha fazla ayrıntı için bkz: [Azure AD Privileged Identity Management](active-directory-privileged-identity-management-configure.md). 

## <a name="see-also"></a>Ayrıca bkz.
* [Azure Active Directory kimlik koruması](active-directory-identityprotection.md)

