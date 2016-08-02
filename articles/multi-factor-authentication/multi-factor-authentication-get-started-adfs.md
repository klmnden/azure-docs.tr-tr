<properties 
    pageTitle="Azure Multi-Factor Authentication ve Active Directory Federasyon Hizmetlerini kullanmaya başlama" 
    description="Bu, nasıl Azure MFA ve AD FS kullanmaya başlayacağınızı açıklayan Azure Multi-Factor Authentication sayfasıdır." 
    services="multi-factor-authentication" 
    documentationCenter="" 
    authors="billmath" 
    manager="stevenpo" 
    editor="curtland"/>

<tags 
    ms.service="multi-factor-authentication" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" ms.topic="get-started-article" 
    ms.date="05/12/2016" 
    ms.author="billmath"/>

# Azure Multi-Factor Authentication ve Active Directory Federasyon Hizmetlerini kullanmaya başlama



<center>![Bulut](./media/multi-factor-authentication-get-started-adfs/adfs.png)</center>

Kuruluşunuz şirket için Active Directory’nizi AD FS kullanan Azure Active Directory ile birleştirdiyse, Azure Multi-Factor Authentication için aşağıdaki 2 seçenek kullanılabilir.

- Azure Multi-Factor Authentication veya Active Directory Federasyon Hizmetleri’ni kullanarak bulut kaynaklarını güvenli hale getirme 
- Azure Multi-Factor Authentication Sunucusu kullanarak bulut ve şirket içi kaynakları güvenli hale getirme 

Aşağıdaki tabloda kaynakları Azure Multi-Factor Authentication ile AD FS arasında kimlik doğrulama deneyimi özetlenmiştir.

|Kimlik Doğrulama Deneyimi - Tarayıcı Tabanlı Uygulamalar|Kimlik Doğrulama Deneyimi - Tarayıcı Tabanlı olmayan Uygulamalar
:------------- | :------------- | :------------- |
Azure AD’yi Azure Multi-Factor Authentication kullanarak güvenli hale getirme |<li>Kimlik doğrulamanın 1. öğesi AD FS kullanarak şirket içi gerçekleştirilir.</li> <li>2. öğe bulut kimlik doğrulaması kullanarak yapılan telefon tabanlı yöntemdir.</li>|Son kullanıcılar oturum açmak için uygulama parolaları kullanabilir.
Active Directory Federasyon Hizmetleri’ni kullanarak Azure AD kaynaklarını güvenli hale getirme |<li>Kimlik doğrulamanın 1. öğesi AD FS kullanarak şirket içi gerçekleştirilir.</li><li>2. öğe talebi onaylanmasıyla şirket içi gerçekleştirilir.</li>|Son kullanıcılar oturum açmak için uygulama parolaları kullanabilir.

Federasyon kullanıcıları için uygulama parolaları ile ilgili uyarılar: 

- Uygulama Parolası bulut kimlik doğrulaması kullanarak doğrulanır ve bu nedenle federasyonları atlar. Federasyon yalnızca Uygulama Parolası ayarlanırken etkin şekilde kullanılır.
- Şirket için İstemci Erişimi Denetimi ayarları Uygulama Parolası tarafından onaylanmaz.
- Uygulama Parolası için şirket için kimlik doğrulaması günlüğe kaydetme özelliğini kaybedersiniz.
- Hesap devre dışı bırakma/silme, bulut kimliğinde uygulama parolasının dizin eşitlemesi, geciktirilmesi, devre dışı bırakılması/silinmesi için 3 saate kadar sürebilir.

Azure Multi-Factor Authentication ya da AD FS ile Azure Multi-Factor Authentication Sunucusu kurulumu hakkında bilgi edinmek için aşağıdakilere bakın:

- [Azure Multi-Factor Authentication ve AD FS kullanarak bulut kaynaklarını güvenli hale getirme](multi-factor-authentication-get-started-adfs-cloud.md)
- [Windows Server 2012 R2 AD FS ile Azure Multi-Factor Authentication Sunucusu kullanarak bulut ve şirket içi kaynakları güvenli hale getirme](multi-factor-authentication-get-started-adfs-w2k12.md)
- [AD FS 2.0 ile Azure Multi-Factor Authentication Sunucusu kullanarak bulut ve şirket içi kaynakları güvenli hale getirme](multi-factor-authentication-get-started-adfs-adfs2.md)







 


<!---HONumber=Jun16_HO2-->


