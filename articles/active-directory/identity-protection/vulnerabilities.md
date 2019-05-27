---
title: Azure Active Directory kimlik koruması tarafından algılanan güvenlik açıklarını
description: Azure Active Directory kimlik koruması tarafından algılanan güvenlik açıklarını genel bakış.
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: article
ms.date: 04/09/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0e792551f4cac857f56454c67d527e01cb9c4281
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66113118"
---
# <a name="vulnerabilities-detected-by-azure-active-directory-identity-protection"></a>Azure Active Directory kimlik koruması tarafından algılanan güvenlik açıklarını

Bir saldırgan tarafından kötüye kullanılmadan bir ortamda zayıf güvenlik açıklarıdır. Yöneticiler, kuruluşunuzun güvenlik duruşunu bu güvenlik açıklarına öneririz.

![Kimlik koruması tarafından bildirilen güvenlik açıkları](./media/vulnerabilities/identity-protection-vulnerabilities.png)

Aşağıdaki bölümler, kimlik koruması tarafından bildirilen güvenlik açıklarını genel bir bakış sağlar.

## <a name="multi-factor-authentication-registration-not-configured"></a>Çok faktörlü kimlik doğrulaması kaydı yapılandırılmadı

Bu güvenlik açığını Azure multi-Factor Authentication'ın dağıtım kuruluşunuzdaki değerlendirmek yardımcı olur.

Azure multi-Factor Authentication ikinci bir kullanıcı kimlik doğrulaması için güvenlik katmanı sağlar. Bu basit bir oturum açma işlemi için kullanıcı taleplerini karşılarken, verilere ve uygulamalara erişimi korunmasına yardımcı. Azure multi-Factor Authentication gibi kullanımı kolay doğrulama seçenekleri sağlar:

* Telefon araması
* SMS Mesajı
* Mobil uygulama bildirimi
* OTP doğrulama kodu

Azure multi-Factor Authentication kullanıcı oturum açma işlemleri için ihtiyacınız olan öneririz. Çok faktörlü kimlik doğrulaması risk tabanlı koşullu erişim ilkeleri kimlik koruması kullanılabilen önemli bir rol oynar.

Daha fazla bilgi için bkz. [Azure Multi-Factor Authentication nedir?](../authentication/multi-factor-authentication.md)

## <a name="unmanaged-cloud-apps"></a>Yönetilmeyen bulut uygulamaları

Bu güvenlik açığını yönetilmeyen bulut uygulamaları kuruluşunuzdaki belirlemenize yardımcı olur.

BT personeli, genellikle, kuruluş içindeki tüm bulut uygulamalarının farkında değildir. Yöneticiler şirket verilerini, olası veri sızıntılarına ve diğer güvenlik risklerini yetkisiz erişimi ile ilgili endişelerini neden gerekir görmek daha kolaydır.

Yönetilmeyen bulut uygulamaları keşfetmek için ve Azure Active Directory'yi kullanarak bu uygulamaları yönetmek için Cloud Discovery dağıtma öneririz.

Daha fazla bilgi için [Cloud Discovery](/cloud-app-security/set-up-cloud-discovery).

## <a name="security-alerts-from-privileged-identity-management"></a>Privileged Identity Management'tan Güvenlik Uyarıları

Bu güvenlik açığını keşfedin ve uyarılar, kuruluşunuzda ayrıcalıklı kimlikleri hakkında çözmenize yardımcı olur.  

Ayrıcalıklı işlemleri gerçekleştirmek kullanıcıları etkinleştirmek için Azure AD'de kullanıcılara geçici veya kalıcı ayrıcalıklı erişim vermek kuruluşların gereken kaynakları Azure veya Office 365 veya diğer SaaS uygulamalarını. Her biri bu ayrıcalıklı kullanıcılar, kuruluşunuzun saldırı yüzeyini artırır. Bu güvenlik açığını gereksiz ayrıcalıklı erişimi kullanıcıları tanımlamak ve azaltmak veya bunlar konusunda sizi uyarmayı riskini ortadan kaldırmak için uygun eylemde yardımcı olur.

Biz yönetmek için Azure AD Privileged Identity Management kuruluşların kullanması önerilir denetlemek ve Azure AD'de ayrıcalıklı kimlikleri ve bunun yanı sıra Office 365 veya Microsoft Intune gibi diğer Microsoft çevrimiçi hizmetlerine izleyin.

Daha fazla bilgi için [Azure AD Privileged Identity Management](../privileged-identity-management/pim-configure.md).

## <a name="see-also"></a>Ayrıca bkz.

[Azure Active Directory kimlik koruması](../active-directory-identityprotection.md)
