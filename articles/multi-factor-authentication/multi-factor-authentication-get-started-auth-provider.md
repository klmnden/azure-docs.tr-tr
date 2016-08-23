<properties 
    pageTitle="Microsoft Azure Multi-Factor Auth Sağlayıcısını kullanmaya başlama" 
    description="Azure Multi-Factor Auth Sağlayıcısı oluşturma hakkında bilgi edinin." 
    services="multi-factor-authentication" 
    documentationCenter="" 
    authors="billmath" 
    manager="stevenpo" 
    editor="curtand"/>

<tags 
    ms.service="multi-factor-authentication" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="06/29/2016" 
    ms.author="billmath"/>



# Azure Multi-Factor Auth Sağlayıcısını kullanmaya başlama
Çok faktörlü kimlik doğrulaması Azure Active Directory ve Office 365 kullanıcılarına sahip genel yöneticiler için varsayılan olarak kullanılabilir durumdadır. Ancak, [gelişmiş özelliklerden](multi-factor-authentication-whats-next.md) yararlanmak isterseniz Azure MFA’nın tam sürümünü satın almanız gerekir. 

> [AZURE.NOTE]  Azure Multi-Factor Auth Sağlayıcısı Azure MFA tam sürümünün sağladığı özelliklerden yararlanmak için kullanılır. **Azure MFA, Azure AD Premium veya EMS lisansı olmayan** kullanıcılara yöneliktir.  Azure MFA, Azure AD Premium ve EMS varsayılan olarak Azure MFA’nın tam sürümünü içerir.  Lisanslarınız varsa bir Azure Multi-Factor Auth Sağlayıcısı gerekli değildir. 

SDK’yı indirmek isterseniz Azure Multi-Factor Auth sağlayıcısı gereklidir.

> [AZURE.IMPORTANT]  SDK’yı indirmek isterseniz Azure MFA, AAD Premium veya EMS lisanslarınız olsa bile bir Azure Multi-Factor Auth Sağlayıcısı oluşturmanız gerekir.  Bu amaç için bir Azure Multi-Factor Auth Sağlayıcısı oluşturursanız ve zaten lisanslarınız varsa, Sağlayıcıyı **Etkin Kullanıcı Başına** modeliyle oluşturmanız ve Azure MFA, Azure AD Premium veya EMS lisanslarını içeren dizine Sağlayıcıyı bağlamanız gerekir.  Bunun yapılması, sahip olduğunuz lisans sayısından fazla sayıda kullanıcı SDK’yı kullanmadıkça sizden ücret alınmamasını sağlar.
 
Azure Multi-Factor Auth Sağlayıcısı oluşturmak için aşağıdaki adımları kullanın.

## Multi-Factor Auth Sağlayıcısı oluşturmak için
--------------------------------------------------------------------------------

1. **Klasik Azure portalında** Yönetici olarak oturum açın.
2. Sol taraftaki **Active Directory** öğesini seçin.
3. Active Directory sayfasının en üst kısmındaki **Multi-Factor Authentication Sağlayıcıları**’na tıklayın.
![MFA Sağlayıcısı oluşturma](./media/multi-factor-authentication-get-started-auth-provider/authprovider1.png)
4. Alt kısımda **Yeni**’ye tıklayın.
![MFA Sağlayıcısı oluşturma](./media/multi-factor-authentication-get-started-auth-provider/authprovider2.png)
5. **App Services** altında **Multi-Factor Auth Sağlayıcısı**
![MFA Sağlayıcısı Oluşturma öğesini seçin](./media/multi-factor-authentication-get-started-auth-provider/authprovider3.png)
6. **Hızlı Oluştur**’u seçin.
![MFA Sağlayıcısı oluşturma](./media/multi-factor-authentication-get-started-auth-provider/authprovider4.png)
5. Aşağıdaki alanları doldurun ve **Oluştur**’u seçin.
    1. **Ad** – Multi-Factor Auth Sağlayıcısının adı.
    2. **Kullanım Modeli** – Multi-Factor Authentication Sağlayıcısının kullanım modeli.
        - Kimlik Doğrulaması Başına – kimlik doğrulaması başına ücretlendirilen satın alma modeli. Genellikle tüketiciyle karşılaşan uygulamada Azure Multi-Factor Authentication kullanan senaryolar için kullanılır.
        - Etkin Kullanıcı Başına - etkin kullanıcı başına ücretlendirilen satın alma modeli. Genellikle Office 365 gibi uygulamalara çalışan erişimi için kullanılır.
    2. **Dizin** – Multi-Factor Authentication Sağlayıcısının ilişkili olduğu Azure Active Directory kiracısı. Lütfen aşağıdakilere dikkat edin:
        - Bir Multi-Factor Auth Sağlayıcısı oluşturmak için Azure AD dizini gerekli değildir.  Yalnızca Azure Multi-Factor Authentication Sunucusunu veya SDK’sını kullanmayı planlıyorsanız kutuyu boş bırakmanız yeterlidir.
        - Gelişmiş özelliklerden yararlanmak için Multi-Factor Auth Sağlayıcısının bir Azure AD dizini ile ilişkili olması gerekir.
        - Azure AD Connect, AAD Sync veya DirSync yalnızca şirket içi Active Directory ortamınızı bir Azure AD diziniyle eşitliyorsanız gereklidir.  Yalnızca eşitlenmemiş bir Azure AD dizini kullanıyorsanız bunun yapılması gerekli değildir. 
![MFA Sağlayıcısı oluşturma](./media/multi-factor-authentication-get-started-auth-provider/authprovider5.png)    
5. Oluştur’a tıkladıktan sonra Multi-Factor Authentication Sağlayıcısı oluşturulur ve şu iletiyi görmeniz gerekir: **Multi-Factor Authentication Sağlayıcısı başarıyla oluşturuldu**. **Tamam**’a tıklayın.
![MFA Sağlayıcısı oluşturma](./media/multi-factor-authentication-get-started-auth-provider/authprovider6.png)    


<!--HONumber=Aug16_HO1-->


