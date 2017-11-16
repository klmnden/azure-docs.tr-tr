---
title: "Azure MFA ve üçüncü taraf VPN ile Gelişmiş senaryolar"
description: "Cisco, Citrix ve Juniper ile tümleştirmek Azure MFA için adım adım yapılandırma kılavuzları."
services: multi-factor-authentication
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: richagi
ms.assetid: 1f94a214-d6f6-48a8-8a12-006b5896ae45
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2017
ms.author: joflore
ms.openlocfilehash: 0e7406e00aea59f14a986bd1dd091d0968cc4579
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
# <a name="advanced-scenarios-with-azure-multi-factor-authentication-and-third-party-vpn-solutions"></a>Azure çok faktörlü kimlik doğrulama ve üçüncü taraf VPN çözümlerinin Gelişmiş senaryolar
Azure çok faktörlü kimlik doğrulaması çeşitli üçüncü taraf VPN çözümleri ile sorunsuz bir şekilde bağlanmak için kullanılabilir. Bu makalede, Cisco® ASA VPN Gereci, Citrix NetScaler SSL VPN Gereci ve Juniper ağları güvenli erişim/Pulse Secure bağlanmak güvenli SSL VPN Gereci odaklanır. Bu üç ortak cihazları yönelik olarak yapılandırma kılavuzları oluşturduk. Çok faktörlü kimlik doğrulama sunucusu ayrıca en RADIUS, LDAP, IIS veya AD FS talep tabanlı kimlik doğrulaması kullanan diğer sistemler ile tümleştirebilirsiniz. Daha ayrıntılı bilgi bulabilirsiniz [MFA sunucusu yapılandırmaları](multi-factor-authentication-get-started-server.md#next-steps).

## <a name="cisco-asa-vpn-appliance-and-azure-multi-factor-authentication"></a>Cisco ASA VPN Gereci ve Azure multi-Factor Authentication
Cisco AnyConnect® VPN oturum açmalar ve portala erişim için ek güvenlik sağlamak için Cisco® ASA VPN aygıtınızın Azure çok faktörlü kimlik doğrulaması tümleşir.  LDAP ya da RADIUS protokolü kullanabilirsiniz.  Ayrıntılı adım adım yapılandırma Kılavuzları'nı indirmek için aşağıdakilerden birini seçin.

| Yapılandırma Kılavuzu | Açıklama |
| --- | --- |
| [Cisco Anyconnect VPN ve Azure MFA yapılandırması LDAP ile ASA](http://download.microsoft.com/download/A/2/0/A201567C-C3DE-4227-AF89-4567A470899E/Cisco_ASA_Azure_MFA_LDAP.docx) | Cisco ASA VPN Gereci LDAP kullanarak Azure MFA ile tümleştirme |
| [Cisco Anyconnect VPN ve Azure MFA yapılandırması için RADIUS ile ASA](http://download.microsoft.com/download/4/5/7/4579C1CF-35B0-4FBE-8A1A-B49CB2CC0382/Cisco_ASA_Azure_MFA_RADIUS.docx) | Cisco ASA VPN aygıtınızın RADIUS kullanan Azure MFA ile tümleştirme |

## <a name="citrix-netscaler-ssl-vpn-and-azure-multi-factor-authentication"></a>Citrix NetScaler SSL VPN ve Azure çok faktörlü kimlik doğrulaması
Azure çok faktörlü kimlik doğrulaması Citrix NetScaler SSL VPN oturum açmalar ve portala erişim için ek güvenlik sağlamak için Citrix NetScaler SSL VPN aygıtınızın tümleşir.  LDAP ya da RADIUS protokolü kullanabilirsiniz.  Ayrıntılı adım adım yapılandırma Kılavuzları'nı indirmek için aşağıdakilerden birini seçin.

| Yapılandırma Kılavuzu | Açıklama |
| --- | --- |
| [LDAP için Citrix NetScaler SSL VPN ve Azure MFA yapılandırma](http://download.microsoft.com/download/2/4/E/24E1E722-72DF-471F-A88A-D1338DB1AF83/Citrix_NS_Azure_MFA_LDAP.docx) | Citrix NetScaler SSL VPN ile Azure MFA Gereci LDAP kullanarak tümleştirme |
| [RADIUS için Citrix NetScaler SSL VPN ve Azure MFA yapılandırma](http://download.microsoft.com/download/1/A/4/1A482764-4A63-45C2-A5EC-2B673ACCDD12/Citrix_NS_Azure_MFA_RADIUS.docx) | Citrix NetScaler SSL VPN aygıtınızın RADIUS kullanan Azure MFA ile tümleştirme |

## <a name="juniperpulse-secure-ssl-vpn-appliance-and-azure-multi-factor-authentication"></a>Juniper/Pulse Secure SSL VPN Gereci ve Azure multi-Factor Authentication
Juniper/Pulse Secure SSL VPN aygıtınızın Juniper/Pulse Secure SSL VPN oturum açmalar ve portala erişim için ek güvenlik sağlamak için Azure multi-Factor Authentication tümleşir.  LDAP ya da RADIUS protokolü kullanabilirsiniz.  Ayrıntılı adım adım yapılandırma Kılavuzları'nı indirmek için aşağıdakilerden birini seçin.

| Yapılandırma Kılavuzu | Açıklama |
| --- | --- |
| [LDAP için Juniper/Pulse güvenli SSL VPN ve Azure MFA yapılandırma](http://download.microsoft.com/download/6/5/8/6587B418-75B1-4FCB-84D4-984BC479309E/JuniperPulse_Azure_MFA_LDAP.docx) | Juniper/Pulse Secure SSL VPN ile Azure MFA Gereci LDAP kullanarak tümleştirme |
| [RADIUS için Juniper/Pulse güvenli SSL VPN ve Azure MFA yapılandırma](http://download.microsoft.com/download/7/9/A/79AB3DAD-4799-4379-B1DA-B95ABDF231DC/JuniperPulse_Azure_MFA_RADIUS.docx) | Juniper/Pulse Secure SSL VPN aygıtınızın RADIUS kullanan Azure MFA ile tümleştirme |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure çok faktörlü kimlik doğrulaması için NPS uzantılı varolan kimlik altyapınızı büyütmek](multi-factor-authentication-nps-extension.md)

- [Azure Multi-Factor Authentication ayarlarını yapılandırma](multi-factor-authentication-whats-next.md)