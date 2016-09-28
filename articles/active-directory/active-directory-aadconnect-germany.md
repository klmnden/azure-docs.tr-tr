<properties
    pageTitle="Microsoft Bulut Almanya’da Azure AD Connect"
    description="Azure AD Connect, şirket içi dizinlerinizi Azure Active Directory ile tümleştirir. Bu sayede Azure AD ile tümleşik Office 365, Azure ve SaaS uygulamaları için ortak bir kimlik oluşturabilirsiniz."
    keywords="Azure AD Connect’e giriş, Azure AD Connect’e genel bakış, Azure AD Connect nedir, active directory yükleme, Almanya, Black Forest"
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/08/2016"
    ms.author="billmath"/>


#Microsoft Bulut Almanya’da Azure AD Connect - Genel Önizleme

## Giriş
Azure AD Connect, şirket içi Active Directory’niz ile Azure Active Directory arasında eşitleme sağlar.
Şu anda [Microsoft Bulut Almanya](https://www.microsoft.com/de-de/cloud/deutschland/default.aspx)’daki senaryoların çoğunun operatör tarafından yürütülmesi gerekmektedir. Microsoft Bulut Almanya’yı kullanırken aşağıdakilerin farkında olmanız gerekir:


- Eşitlemenin başarıyla gerçekleştirilebilmesi için aşağıdaki URL'lerin bir ara sunucuda açılması gerekir:
    - *. microsoftonline.de
    - *.windows.net
    - + Sertifika İptal Listeleri

- Azure AD dizininizde oturum açarken, onmicrosoft.de etki alanında yer alan bir hesap kullanmanız gerekir.
- Aşağıdaki özellikler kullanılamaz:
    - Azure AD Connect Health
    - Otomatik güncelleştirmeler
    - Parola geri yazma

## İndir
Azure AD Connect’i portalın içerisindeki Azure AD Connect dikey penceresinden indirebilirsiniz.  Azure AD Connect dikey penceresini bulmak için aşağıdaki yönergeleri kullanın.

### Azure AD Connect Dikey Penceresi

Azure portalda oturum açtıktan sonra şunları yapın:

1. Gözat’a gidin
2.  Azure Active Directory'yi seçin
3.  Azure AD Connect'i seçin

Şunları görmeniz gerekir:

![Azure AD Connect Dikey Penceresi](media\active-directory-aadconnect-germany\germany1.png)

 
Aşağıdaki tabloda, dikey pencerede gösterilen özellikler açıklanmaktadır.


Başlık|Açıklama|
----- | ----- |
EŞİTLEME DURUMU|Eşitlemenin etkin mi yoksa devre dışı mı olduğunu gösterir.|
SON EŞİTLEME|Başarılı bir eşitlemenin tamamlandığı en son zaman.|
FEDERASYON ETKİ ALANLARI|Şu anda yapılandırılmış durumda olan federasyon etki alanlarının sayısını gösterir.|


## Yükleme
Azure AD Connect'i yüklemek için [buradaki](active-directory-aadconnect.md#install-azure-ad-connect) belgeleri kullanabilirsiniz.

## Gelişmiş özellikler ve Ek Bilgiler
Özel ayarlar veya gelişmiş yapılandırmalar hakkında ek bilgiler ya da kılavuzluk edinmek için [Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md) (Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme) makalesinden başlayın.  Bu sayfada, bilgiler ve ek rehberlik sağlayan diğer sayfalara bağlantılar bulabilirsiniz.



<!--HONumber=Sep16_HO3-->


