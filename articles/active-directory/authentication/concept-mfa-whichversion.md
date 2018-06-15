---
title: Azure MFA bulutu vea sunucusu arasında seçim yapma | Microsoft Docs
description: Neyi güvenli hale getirmeye çalışıyorum ve kullanıcılarım nerede yer alıyor sorularını kendinize sorarak multi-factor authentication güvenlik çözümünüzü seçin.
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: get-started-article
ms.date: 10/02/2017
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: richagi
ms.openlocfilehash: 8314d72aa2cc6787d3f65dd48cd693a0ac332c0a
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
ms.locfileid: "33866361"
---
# <a name="choose-the-azure-multi-factor-authentication-solution-for-you"></a>Size uygun Azure Multi-Factor Authentication çözümünü seçin
Azure Multi-Factor Authentication’ın (MFA) birçok modeli olduğundan, hangi sürümü kullanmanın uygun olacağını belirlemek için birkaç soruyu yanıtlamamız gerekir.  Bu sorular şunlardır:

* [Neyi güvenli hale getirmeye çalışıyorum?](#what-am-i-trying-to-secure)
* [Kullanıcılar nerede bulunuyor?](#where-are-the-users-located)
* [Hangi özelliklere ihtiyacım var?](#what-features-do-i-need)

Aşağıdaki bölümler bu yanıtların her birini belirlemede rehberlik sağlar.

## <a name="what-am-i-trying-to-secure"></a>Neyi güvenli hale getirmeye çalışıyorum?
Doğru iki aşamalı doğrulama çözümünü belirlemek için, önce ikinci bir kimlik doğrulama yöntemiyle neyi güvenli hale getirmeye çalıştığımız sorusunu yanıtlamalıyız.  Bu, Azure’da olan bir uygulama mi?  Veya uzaktan erişim sistemi mi?  Neyi güvenli hale getirmeye çalıştığımızı belirleyerek Multi-Factor Authentication’ın nerede etkinleştirilmesi gerektiği sorusunu yanıtlayabiliriz.  

| Neyi güvenli hale getirmeye çalışıyorsunuz? | Bulutta MFA | MFA Sunucusu |
| --- |:---:|:---:|
| Birinci taraf Microsoft uygulamaları |● |● |
| Uygulama galerisinde SaaS uygulamaları |● |  |
| Azure AD Uygulaması Proxy üzerinden yayımlanan web uygulamaları |● |  |
| Azure AD Uygulaması Proxy üzerinden yayımlanmayan IIS uygulamaları | |● |
| VPN, RDG gibi uzaktan erişim | ● | ● |

## <a name="where-are-the-users-located"></a>Kullanıcılar nerede bulunuyor?
Kullanıcılarımızın nerede bulunduğuna bakmak, ister bulutta ister MFA Sunucusu kullanan şirket içinde olan doğru çözümün belirlenmesine yardımcı olur.

| Kullanıcı Konumu | Bulutta MFA | MFA Sunucusu |
| --- |:---:|:---:|
| Azure Active Directory |● | |
| AD FS ile federasyon kullanana Azure AD ve şirket içi AD |● |● |
| DirSync, Azure AD Sync, Azure AD Connect kullanan Azure AD ve şirket içi AD - parola karma değeri eşitleme veya doğrudan kimlik doğrulaması yok |● |● |
| DirSync, Azure AD Sync, Azure AD Connect kullanan Azure AD ve şirket içi AD - parola karma değeri eşitleme veya doğrudan kimlik doğrulaması ile |● | |
| Şirket içi Active Directory | |● |

## <a name="what-features-do-i-need"></a>Hangi özelliklere ihtiyacım var?
Aşağıdaki tabloda, bulutta Multi-Factor Authentication ile kullanılabilen özellikler ve Multi-Factor Authentication Sunucusu ile kullanılabilen özellikler karşılaştırılmıştır.

| Özellik | Bulutta MFA | MFA Sunucusu |
| --- |:---:|:---:|
| İkinci öğe olarak mobil uygulama bildirimi | ● | ● |
| İkinci öğe olarak mobil uygulama doğrulama kodu | ● | ● |
| İkinci öğe olarak telefon araması | ● | ● |
| İkinci öğe olarak tek yönlü SMS | ● | ● |
| İkinci öğe olarak iki yönlü SMS | | ●  (Kullanım Dışı)| 
| İkinci öğe olarak Donanım Belirteçleri | | ● |
| MFA'yı desteklemeyen Office 365 istemcileri için uygulama parolaları | ● | |
| Kimlik doğrulama yöntemleri üzerinde yönetici denetimi | ● | ● |
| PIN modu | | ● |
| Sahtekarlık uyarısı |● | ● |
| MFA Raporları |● | ● |
| Bir Kerelik Atlama | | ● |
| Telefon aramaları için özel karşılama | ● | ● |
| Telefon aramaları için özelleştirilebilir arayan kimliği | ● | ● |
| Güvenilen IP'ler | ● | ● |
| Güvenilen cihazlar için MFA hatırlama | ● | |
| Koşullu erişim | ● | ● |
| Önbellek |  | ● |

## <a name="next-steps"></a>Sonraki adımlar

Bulutta ve şirket içindeki MFA Sunucusunda Azure Multi-Factor Authentication kullanmanın farklarını gördüğümüze göre artık Azure Multi-Factor Authentication’ı ayarlama ve kullanma zamanı gelmiş demektir. **Senaryonuzu temsil eden simgeyi seçin**

<center>

[![Bulutta MFA](./media/concept-mfa-whichversion/cloud2.png)](howto-mfa-getstarted.md)  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; [![MFA sunucusu](./media/concept-mfa-whichversion/server2.png)](howto-mfaserver-deploy.md) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </center>
