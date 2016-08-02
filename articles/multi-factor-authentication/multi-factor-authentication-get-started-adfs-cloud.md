<properties 
    pageTitle="Azure Multi-Factor Authentication ve AD FS ile bulut kaynaklarını güvenli hale getirme" 
    description="Bu, bulutta nasıl Azure MFA ve AD FS kullanmaya başlayacağınızı açıklayan Azure Multi-Factor Authentication sayfasıdır." 
    services="multi-factor-authentication" 
    documentationCenter="" 
    authors="billmath" 
    manager="stevenpo" 
    editor="curtland"/>

<tags 
    ms.service="multi-factor-authentication" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="05/12/2016" 
    ms.author="billmath"/>

# Azure Multi-Factor Authentication ve AD FS ile bulut kaynaklarını güvenli hale getirme

Kuruluşunuz Azure Active Directory ile birleştiriliyorsa ve Azure AD tarafından erişilen kaynaklara sahipseniz, bu kaynakları güvenli hale getirmek için Azure Multi-Factor Authentication ya da Active Directory Federasyon Hizmetleri’ni kullanabilirsiniz. Azure Active Directory kaynaklarını Azure Multi-Factor Authentication ya da Active Directory Federasyon Hizmetleri ile güvenli hale getirmek için aşağıdaki yordamları kullanın.

## AD FS kullanarak Azure AD kaynaklarını güvenli hale getirmek için aşağıdakileri yapın: 



1. Kullanıcıların hesabı etkinleştirmesi için [multi-factor authentication’ı açma](active-directory/multi-factor-authentication-get-started-cloud.md#turn-on-multi-factor-authentication-for-users) bölümünde açıklanan adımları kullanın.
2. Bir talep kuralı ayarlamak için aşağıdaki yordamı kullanın:

![Bulut](./media/multi-factor-authentication-get-started-adfs-cloud/adfs1.png)

-   AD FS Yönetim Konsolu'nu başlatın.
-   Bağlı Olan Taraf Güvenleri’ne gidin ve Bağlı Olan Taraf Güveni’ne sağ tıklayın. Talep Kurallarını Düzenle... seçeneğini seçin.
-   Kural Ekle... seçeneğine tıklayın.
-   Açılır menüde, Talepleri Özel bir Kural Kullanarak Gönder’i seçin ve İleri’ye tıklayın.
-   Talep kuralı için bir ad girin.
-   Özel kural altında: aşağıdakileri ekleyin:


        => issue(Type = "http://schemas.microsoft.com/claims/authnmethodsreferences", Value = "http://schemas.microsoft.com/claims/multipleauthn");

    Karşılık gelen talep:

        <saml:Attribute AttributeName="authnmethodsreferences" AttributeNamespace="http://schemas.microsoft.com/claims">
        <saml:AttributeValue>http://schemas.microsoft.com/claims/multipleauthn</saml:AttributeValue>
        </saml:Attribute>
- Tamam'a tıklayın. Son'a tıklayın. AD FS Yönetim Konsolu'nu kapatın.

Kullanıcılar şirket içi yöntemi (örneğin, akıllı kart) kullanarak oturum açmayı tamamlayabilir.

## Federasyon kullanıcıları için Güvenilen IP'ler
Güvenilen IP'ler yöneticilerin, belirli IP adresleri ya da kendi intranetlerinden kaynaklanan taleplere sahip federasyon kullanıcıları için multi-factor authentication’ı atlamasına izin verir. Aşağıdaki bölümlerde, federasyon kullanıcıları intranetinden gelen talepler için Azure Multi-Factor Authentication Güvenilen IP’lerinin federasyon kullanıcılarıyla yapılandırılması ve multi-factor authentication’ın atlanması açıklanmıştır.  Buna, AD FS’yi Kurumsal Ağın İçinden talep türü ile gelen talep şablonu için geçiş ya da filtre kullanacak şekilde yapılandırarak ulaşılır.  Bu örnekte Güvenilen Taraf Güvenlerimiz için Office 365 kullanılmıştır.

### AD FS talep kurallarını yapılandırma

Yapmamız gereken ilk şey, AD FS taleplerini yapılandırmaktır. Bir Kurumsal Ağın İçinden talep türü ve diğeri kullanıcılarımızı oturum açık şekilde tutmak için olmak üzere iki talep kuralı oluşturacağız.

1. AD FS Yönetimi'ni açın.
2. Solda, Bağlı Olan Taraf Güvenleri’ni seçin.
3. Ortada, Microsoft Office 365 Kimlik Platformu’na sağ tıklayın ve **Talep Kurallarını Düzenle…**’yi seçin.
![Bulut ](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip1.png)
4. Verme Dönüştürme Kuralları’nda **Kural Ekle**’ye tıklayın.
![Bulut](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip2.png)
5. Dönüştürme Kuralı Ekleme Sihirbazı’nda, açılır menüde Gelen Talep için Geçiş ya da Filtre’yi seçin ve İleri’ye tıklayın.
![Bulut](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip3.png)
6. Talep kuralı adının yanındaki kutuda kuralınıza bir ad verin. Örneğin: InsideCorpNet.
7. Gelen talep türü yanındaki Açılır menüde, Kurumsal Ağın İçinden seçeneğini seçin.
![Bulut](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip4.png)
8. Son'a tıklayın.
9. Verme Dönüştürme Kuralları’nda **Kural Ekle**’ye tıklayın.
10. Dönüştürme Kuralı Ekleme Sihirbazı’nda, açılır menüde Talepleri Özel bir Kural Kullanarak Gönder’i seçin ve İleri’ye tıklayın.
11. Talep kuralı adı altındaki kutuya: Kullanıcıların Oturumlarını Açık Tut’u girin.
12. Özel kural kutusuna şunu girin:
        
        c:[Type == "http://schemas.microsoft.com/2014/03/psso"]
            => issue(claim = c);
![Bulut](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip5.png)
13. **Son**'a tıklayın.
14. **Uygula**'ya tıklayın.
15. **Tamam**’a tıklayın.
16. AD FS Yönetimi'ni kapatın.



### Azure Multi-Factor Authentication Güvenilen IP’leri Federasyon Kullanıcıları ile Yapılandırma
Talepler yapıldığına göre, güvenilen IP’leri yapılandırabiliriz.

1. Azure Yönetim Portalı’nda oturumu açın.
2. Solda Active Directory'ye tıklayın.
3. Dizin altında Güvenilen IP’leri ayarlamak istediğiniz dizine tıklayın.
4. Seçtiğiniz Dizinde Yapılandır'a tıklayın.
5. Multi-factor authentication bölümünde, Hizmet ayarlarını yönet'e tıklayın.
6. Hizmet Ayarları sayfasında, Güvenilen IP'ler altında, **Benim intranetimden gelen federasyon kullanıcılarının istekleri için**seçeneğini seçin.
![Bulut.](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip6.png)
7. Kaydet’e tıklayın.
8. Güncelleştirmeler uygulandıktan sonra Kapat'a tıklayın.


Bu kadar! Bu noktada, birleştirilmiş Office 365 kullanıcıları yalnızca talep kurumsal intranet dışından kaynaklandığı zaman MFA kullanmalıdır.









<!---HONumber=Jun16_HO2-->


