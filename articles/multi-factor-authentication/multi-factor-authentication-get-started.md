---
title: Azure Multi-Factor Authentication - Kullanmaya Başlama
description: Neyi güvenli hale getirmeye çalışıyorum ve kullanıcılarım nerede yer alıyor sorularını kendinize sorarak multi-factor authentication güvenlik çözümünüzü seçin.  Sonra bulut, MFA Sunucusu ya da AD FS arasından seçim yapın.
services: multi-factor-authentication
documentationcenter: ''
author: kgremban
manager: femila
editor: curtland

ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/15/2016
ms.author: kgremban

---
# Sizin için multi-factor güvenlik çözümünü seçtik
Çünkü Hangi sürümün kullanmaya uygun olduğunu anlamak için belirlememiz gereken, Azure Multi-Factor Authentication’a ilişkin birkaç özellik vardır.  Bunlar şunlardır:

* [Neyi güvenli hale getirmeye çalışıyorum?](#what-am-i-trying-to-secure)
* [Kullanıcılar nerede bulunuyor?](#where-are-the-users-located)

Aşağıdaki bölümler bunların her birini belirlemede rehberlik sağlar.

## Neyi güvenli hale getirmeye çalışıyorum?
Doğru multi-factor authentication çözümünü belirlemek için, önce ikinci bir kimlik doğrulama yöntemiyle neyi güvenli hale getirmeye çalıştığınız sorusunu yanıtlamalıyız.  Bu, Azure’da olan bir uygulama mi?  Veya örneğin bir uzaktan erişim sistemi mi?  Neyi güvenli hale getirmeye çalıştığımızı belirleyerek multi-factor authentication’ın nerede etkinleştirilmesi gerektiği sorusunun yanıtını görelim.  

| Neyi güvenli hale getirmeye çalışıyorsunuz? | Bulutta Multi-Factor Authentication | Multi-Factor Authentication Sunucusu |
| --- |:---:|:---:|
| Birinci taraf Microsoft uygulamaları |* |* |
| Uygulama galerisinde Saas uygulamaları |* |* |
| Azure AD Uygulaması Proxy üzerinden yayımlanan IIS uygulamaları |* |* |
| Azure AD Uygulaması Proxy üzerinden yayımlanmayan IIS uygulamaları | |* |
| VPN, RDG gibi uzaktan erişim | |* |

## Kullanıcılar nerede bulunuyor?
Sonra, kullanıcılarımızın bulunduğu yere bağlı olarak, kullanılacak doğru çözümün MFA Sunucusu kullanan bulutta ya da şirket içi multi-factor authentication olduğunu belirleyebiliriz.

| Kullanıcı Konumu | Çözüm |
| --- |:--- |
| Azure Active Directory |Bulutta Multi-Factor Authentication |
| AD FS ile federasyon kullanana Azure AD ve şirket içi AD |Hem bulutta MFA hem de MFA Sunucusu kullanılabilir seçeneklerdir |
| DirSync, Azure AD Sync, Azure AD Connect kullanan Azure AD ve şirket içi AD - parola eşitleme yok |Hem bulutta MFA hem de MFA Sunucusu kullanılabilir seçeneklerdir |
| DirSync, Azure AD Sync, Azure AD Connect kullanan Azure AD ve şirket içi AD - parola eşitleme ile |Bulutta Multi-Factor Authentication |
| Şirket içi Active Directory |Multi-Factor Authentication Sunucusu |

Aşağıdaki tabloda bulutta Multi-Factor Authentication ile Multi-Factor Authentication Sunucusu özellikleri karşılaştırması verilmiştir.

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

Artık bulutta multi-factor authentication mı yoksa şirket içi MFA Sunucusu mu kullanacağımızı belirlediğimize göre, Azure Multi-Factor Authentication’ı ayarlamaya ve kullanmaya başlayabiliriz.   **Senaryonuz temsil eden simgeyi seçin!**

<center>




[![Bulut](./media/multi-factor-authentication-get-started/cloud2.png)](multi-factor-authentication-get-started-cloud.md) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[![Proofup](./media/multi-factor-authentication-get-started/server2.png)](multi-factor-authentication-get-started-server.md) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</center>



<!--HONumber=Sep16_HO4-->


