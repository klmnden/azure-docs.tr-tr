---
title: Azure MFA ve üçüncü taraf VPN'ler - Azure Active Directory ile Gelişmiş senaryolar
description: Cisco, Citrix ve Juniper ile tümleştirmek Azure MFA için adım adım yapılandırma kılavuzları.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: acbf27ca6f5b58d5c3cebb28698304c130381a7a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60414928"
---
# <a name="advanced-scenarios-with-azure-multi-factor-authentication-and-third-party-vpn-solutions"></a>Azure multi-Factor Authentication ve üçüncü taraf VPN çözümleriyle Gelişmiş senaryolar

Azure multi-Factor Authentication, çeşitli üçüncü taraf VPN çözümleriyle sorunsuz bir şekilde bağlanmak için kullanılabilir. Bu makalede, Windows® Cisco ASA VPN Gereci, Citrix NetScaler SSL VPN Gereci ve Juniper ağları güvenli erişim/Pulse Secure bağlanmak güvenli SSL VPN Gereci odaklanır. Bu üç yaygın gereçler yönelik olarak yapılandırma kılavuzları oluşturduk. Multi-Factor Authentication sunucusu, ayrıca en RADIUS, LDAP, IIS veya AD FS için talep tabanlı kimlik doğrulaması kullanan diğer sistemler ile tümleştirebilirsiniz. Daha fazla bilgi bulabilirsiniz [MFA sunucusu yapılandırmaları](howto-mfaserver-deploy.md#next-steps).

## <a name="cisco-asa-vpn-appliance-and-azure-multi-factor-authentication"></a>Cisco ASA VPN Gereci ve Azure multi-Factor Authentication
Cisco AnyConnect® VPN oturum açma bilgileri ve portal erişimi için ek güvenlik sağlamak için Windows® Cisco ASA VPN Gereci ile Azure multi-Factor Authentication'ı tümleştirir.  RADIUS veya LDAP protokolünü kullanabilirsiniz.  Ayrıntılı adım adım yapılandırma Kılavuzları'nı indirmek için aşağıdakilerden birini seçin.

| Yapılandırma Kılavuzu | Açıklama |
| --- | --- |
| [Cisco Anyconnect VPN ve Azure MFA yapılandırmasını LDAP ile ASA](https://download.microsoft.com/download/A/2/0/A201567C-C3DE-4227-AF89-4567A470899E/Cisco_ASA_Azure_MFA_LDAP.docx) | Cisco ASA VPN cihazınızla LDAP kullanarak Azure MFA ile tümleştirin |
| [Cisco ASA ile RADIUS Anyconnect VPN ve Azure mfa'yı yapılandırma](https://download.microsoft.com/download/4/5/7/4579C1CF-35B0-4FBE-8A1A-B49CB2CC0382/Cisco_ASA_Azure_MFA_RADIUS.docx) | Cisco ASA VPN cihazınızla RADIUS kullanan Azure MFA ile tümleştirin |

## <a name="citrix-netscaler-ssl-vpn-and-azure-multi-factor-authentication"></a>Citrix NetScaler SSL VPN ve Azure çok faktörlü kimlik doğrulaması
Citrix NetScaler SSL VPN oturum açma bilgileri ve portal erişimi için ek güvenlik sağlamak için Citrix NetScaler SSL VPN Gereci ile Azure multi-Factor Authentication'ı tümleştirir.  RADIUS veya LDAP protokolünü kullanabilirsiniz.  Ayrıntılı adım adım yapılandırma Kılavuzları'nı indirmek için aşağıdakilerden birini seçin.

| Yapılandırma Kılavuzu | Açıklama |
| --- | --- |
| [LDAP için Citrix NetScaler SSL VPN ve Azure mfa'yı yapılandırma](https://download.microsoft.com/download/2/4/E/24E1E722-72DF-471F-A88A-D1338DB1AF83/Citrix_NS_Azure_MFA_LDAP.docx) | Citrix NetScaler SSL VPN LDAP kullanarak Azure mfa'yı Gereci ile tümleştirin |
| [RADIUS için Citrix NetScaler SSL VPN ve Azure mfa'yı yapılandırma](https://download.microsoft.com/download/1/A/4/1A482764-4A63-45C2-A5EC-2B673ACCDD12/Citrix_NS_Azure_MFA_RADIUS.docx) | Azure MFA RADIUS kullanan Citrix NetScaler SSL VPN cihazınızla tümleştirin |

## <a name="juniperpulse-secure-ssl-vpn-appliance-and-azure-multi-factor-authentication"></a>Juniper/Pulse Secure SSL VPN Gereci ve Azure multi-Factor Authentication
Juniper/Pulse Secure SSL VPN oturum açma bilgileri ve portal erişimi için ek güvenlik sağlamak için gerecinizin Juniper/Pulse Secure SSL VPN ile Azure multi-Factor Authentication'ı tümleştirir.  RADIUS veya LDAP protokolünü kullanabilirsiniz.  Ayrıntılı adım adım yapılandırma Kılavuzları'nı indirmek için aşağıdakilerden birini seçin.

| Yapılandırma Kılavuzu | Açıklama |
| --- | --- |
| [LDAP Juniper/Pulse güvenli SSL VPN ve Azure mfa'yı yapılandırma](https://download.microsoft.com/download/6/5/8/6587B418-75B1-4FCB-84D4-984BC479309E/JuniperPulse_Azure_MFA_LDAP.docx) | Juniper/Pulse Secure SSL VPN LDAP kullanarak Azure mfa'yı Gereci ile tümleştirin |
| [RADIUS için Juniper/Pulse güvenli SSL VPN ve Azure mfa'yı yapılandırma](https://download.microsoft.com/download/7/9/A/79AB3DAD-4799-4379-B1DA-B95ABDF231DC/JuniperPulse_Azure_MFA_RADIUS.docx) | Azure MFA RADIUS kullanan gerecinizin Juniper/Pulse Secure SSL VPN tümleştirin |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure multi-Factor Authentication için NPS uzantısı ile mevcut kimlik doğrulama altyapınızı genişletmek](howto-mfa-nps-extension.md)

- [Azure Multi-Factor Authentication ayarlarını yapılandırma](howto-mfa-mfasettings.md)
