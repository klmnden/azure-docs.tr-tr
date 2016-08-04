<properties
    pageTitle="Kullanıcılarınız için Azure AD'ye Katılımı ayarlama | Microsoft Azure"
    description="Yöneticilerin, şirket içi dizin ve cihaz kaydı için Azure AD Katılımını nasıl ayarlayacakları açıklanmaktadır."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="stevenpo"
    editor=""
    tags="azure-classic-portal"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="02/26/2016"
    ms.author="femila"/>

# Kuruluşunuzda Azure AD'ye Katılımı ayarlama

Azure Active Directory Katılımını (Azure AD Katılımı) ayarlamadan önce kullanıcıların şirket içi dizinini buluta eşitlemeniz veya Azure AD'de yönetilen hesaplar oluşturmanız gerekir.

Şirket içi kullanıcılarınızı Azure AD'ye eşitlemeye ilişkin ayrıntılı yönergeler için bkz. [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md).


Azure AD'de el ile kullanıcı oluşturmak ve yönetmek için bkz. [Azure AD'de kullanıcı yönetimi](https://msdn.microsoft.com/library/azure/hh967609.aspx).

## Cihaz kaydı oluşturma
1. Azure portalında yönetici olarak oturum açın.
2. Sol bölmede **Active Directory**'yi seçin.
3. **Dizin** sekmesinde dizininizi seçin.
4. **Yapılandır** sekmesini seçin.
5. **Cihazlar** bölümüne gidin.
6. **Cihazlar** sekmesinde şunları ayarlayın:  
   * **MAXIMUM NUMBER OF DEVICES PER USER (KULLANICI BAŞINA MAKSİMUM CİHAZ SAYISI)**: Kullanıcıların Azure AD'de sahip olabileceği maksimum cihaz sayısını belirleyin.  Bu kotayı dolduran bir kullanıcı, var olan cihazlarının bir veya birden fazlasını kaldırmadan yeni cihaz ekleyemez.
   * **REQUIRE MULTI-FACTOR AUTH TO JOIN DEVICES (CİHAZLARIN KATILIMI İÇİN MULTI-FACTOR AUTHENTICATION'I GEREKLİ KIL)**: Kullanıcıların cihazlarını Azure AD'ye eklemeleri için ikinci bir kimlik doğrulama faktörü sağlamalarının gerekip gerekmediğini belirleyin. Azure Multi-Factor Authentication hakkında daha fazla bilgi edinmek için bkz. [Bulutta Azure Multi-Factor Authentication ile çalışmaya başlama](multi-factor-authentication-get-started-cloud/).
   * **USERS MAY AZURE AD JOIN DEVICES (KULLANICILAR AZURE AD'YE CİHAZ KATABİLİR)**: Cihazlarını Azure AD'ye ekleme izni olan kullanıcıları ve grupları seçin.
   * **ADDITIONAL ADMINISTRATORS ON AZURE AD JOINED DEVICES (AZURE AD'YE KATILAN CİHAZLAR İÇİN EK YÖNETİCİLER)**: Azure AD Premium veya Enterprise Mobility Suite (EMS) ile hangi kullanıcılara cihaz için yerel yönetici haklarının verileceğini belirleyebilirsiniz. Varsayılan olarak genel yöneticilere ve cihaz sahiplerine yerel yönetici hakları verilir.

<center>![Cihaz kaydı oluşturma](./media/active-directory-azureadjoin/active-directory-aadjoin-configure-devices.png) </center>

Kullanıcılarınız için Azure AD'ye Katılımı ayarladığınızda kurumsal veya kişisel cihazları üzerinden Azure AD'ye bağlanabilirler.

Kullanıcılarınızın Azure AD Katılımını ayarlamalarını sağlamak üzere şu üç senaryoyu kullanabilirsiniz:

- Kullanıcılar, şirketlerine ait cihazları doğrudan Azure AD'ye ekler.
- Kullanıcılar, şirketlerine ait bir cihazı şirket içi Active Directory'deki etki alanına ekler ve ardından cihazı Azure AD'ye genişletir.
- Kullanıcılar, kişisel bir cihazdan Windows'a iş veya okul hesabı ekler.

## Ek bilgiler
* [Kurumlar için Windows 10: Cihazları iş için kullanmanın yolları](active-directory-azureadjoin-windows10-devices-overview.md)
* [Azure Active Directory Join ile bulut işlevlerini Windows 10 cihazlarına genişletme](active-directory-azureadjoin-user-upgrade.md)
* [Azure AD Katılımı kullanım senaryoları hakkında bilgi edinin](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Windows 10 deneyiminden faydalanmak için, etki alanına katılan cihazları Azure AD'ye bağlama](active-directory-azureadjoin-devices-group-policy.md)
* [Azure AD'ye Katılımı ayarlama](active-directory-azureadjoin-setup.md)



<!----HONumber=Jun16_HO2-->


