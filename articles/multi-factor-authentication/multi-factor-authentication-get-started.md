---
title: "Azure MFA bulutuyla sunucusunun karşılaştırılması | Microsoft Belgeleri"
description: "Neyi güvenli hale getirmeye çalışıyorum ve kullanıcılarım nerede yer alıyor sorularını kendinize sorarak multi-factor authentication güvenlik çözümünüzü seçin.  Sonra bulut, MFA Sunucusu ya da AD FS arasından seçim yapın."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: ec2270ea-13d7-4ebc-8a00-fa75ce6c746d
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/14/2016
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 72347099d980f2ca73f39f984787197e1f87e45a


---
# <a name="choose-the-azure-multifactor-authentication-solution-for-you"></a>Size uygun Azure Multi-Factor Authentication çözümünü seçin
Azure Multi-Factor Authentication’ın (MFA) birçok modeli olduğundan, hangi sürümü kullanmanın uygun olacağını belirlemek için birkaç soruyu yanıtlamamız gerekir.  Bu sorular şunlardır:

* [Neyi güvenli hale getirmeye çalışıyorum?](#what-am-i-trying-to-secure)
* [Kullanıcılar nerede bulunuyor?](#where-are-the-users-located)
* [Hangi özelliklere ihtiyacım var?](#what-featured-do-i-need)

Aşağıdaki bölümler bu yanıtların her birini belirlemede rehberlik sağlar.

## <a name="what-am-i-trying-to-secure"></a>Neyi güvenli hale getirmeye çalışıyorum?
Doğru iki aşamalı doğrulama çözümünü belirlemek için, önce ikinci bir kimlik doğrulama yöntemiyle neyi güvenli hale getirmeye çalıştığımız sorusunu yanıtlamalıyız.  Bu, Azure’da olan bir uygulama mi?  Veya uzaktan erişim sistemi mi?  Neyi güvenli hale getirmeye çalıştığımızı belirleyerek Multi-Factor Authentication’ın nerede etkinleştirilmesi gerektiği sorusunu yanıtlayabiliriz.  

| Neyi güvenli hale getirmeye çalışıyorsunuz? | Bulutta Multi-Factor Authentication | Multi-Factor Authentication Sunucusu |
| --- |:---:|:---:|
| Birinci taraf Microsoft uygulamaları |● |● |
| Uygulama galerisinde SaaS uygulamaları |● |● |
| Azure AD Uygulaması Proxy üzerinden yayımlanan IIS uygulamaları |● |● |
| Azure AD Uygulaması Proxy üzerinden yayımlanmayan IIS uygulamaları | |● |
| VPN, RDG gibi uzaktan erişim | |● |

## <a name="where-are-the-users-located"></a>Kullanıcılar nerede bulunuyor?
Kullanıcılarımızın nerede bulunduğuna bakmak, ister bulutta ister MFA Sunucusu kullanan şirket içinde olan doğru çözümün belirlenmesine yardımcı olur.

| Kullanıcı Konumu | Bulutta Multi-Factor Authentication | Multi-Factor Authentication Sunucusu |
| --- |:---:|:---:|
| Azure Active Directory |● | |
| AD FS ile federasyon kullanana Azure AD ve şirket içi AD |● |● |
| DirSync, Azure AD Sync, Azure AD Connect kullanan Azure AD ve şirket içi AD - parola eşitleme yok |● |● |
| DirSync, Azure AD Sync, Azure AD Connect kullanan Azure AD ve şirket içi AD - parola eşitleme ile |● | |
| Şirket içi Active Directory | |● |

## <a name="what-features-do-i-need"></a>Hangi özelliklere ihtiyacım var?
Aşağıdaki tabloda, bulutta Multi-Factor Authentication ile kullanılabilen özellikler ve Multi-Factor Authentication Sunucusu ile kullanılabilen özellikler karşılaştırılmıştır.

| Bulutta Multi-Factor Authentication | Multi-Factor Authentication Sunucusu |
| --- |:---:|:---:|
| İkinci öğe olarak mobil uygulama bildirimi |● |
| İkinci öğe olarak mobil uygulama doğrulama kodu |● |
| İkinci öğe olarak telefon araması |● |
| İkinci öğe olarak tek yönlü SMS |● |
| İkinci öğe olarak iki yönlü SMS | |
| İkinci öğe olarak Donanım Belirteçleri | |
| MFA'yı desteklemeyen istemciler için uygulama parolaları |● |
| Kimlik doğrulama yöntemleri üzerinde yönetici denetimi |● |
| PIN modu | |
| Sahtekarlık uyarısı |● |
| MFA Raporları |● |
| Bir Kerelik Atlama | |
| Telefon aramaları için özel karşılama |● |
| Telefon aramaları için özelleştirilebilir arayan kimliği |● |
| Güvenilen IP'ler |● |
| Güvenilen cihazlar için MFA hatırlama |● |
| Koşullu erişim |● |
| Önbellek | |

Artık bulutta multi-factor authentication mı yoksa şirket içi MFA Sunucusu mu kullanacağımızı belirlediğimize göre, Azure Multi-Factor Authentication’ı ayarlamaya ve kullanmaya başlayabiliriz. **Senaryonuzu temsil eden simgeyi seçin!**

<center>




[![Bulut](./media/multi-factor-authentication-get-started/cloud2.png)](multi-factor-authentication-get-started-cloud.md)  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[![Proofup](./media/multi-factor-authentication-get-started/server2.png)](multi-factor-authentication-get-started-server.md) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</center>



<!--HONumber=Nov16_HO2-->


