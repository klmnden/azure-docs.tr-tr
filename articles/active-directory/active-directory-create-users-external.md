<properties
    pageTitle="Azure Active Directory'de diğer dizinlerden veya iş ortağı şirketlerden kullanıcılar ekleme | Microsoft Azure"
    description="Azure Active Directory'de dış ve yeni konuk kullanıcılar dahil olmak üzere kullanıcıların eklenmesini veya kullanıcı bilgilerinin değiştirilmesini açıklar."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/02/2016"
    ms.author="curtand"/>

# Azure Active Directory'de diğer dizinlerden veya iş ortağı şirketlerden kullanıcılar ekleme

Bu makalede Azure Active Directory'de diğer dizinlerden kullanıcıların eklenmesi veya iş ortağı şirketlerden kullanıcıların eklenmesi açıklanmaktadır. Kuruluşunuzdaki yeni kullanıcıların ve Microsoft hesabına sahip kullanıcıların eklenmesi hakkında daha fazla bilgi için bkz. [Azure Active Directory'ye yeni kullanıcı ekleme](active-directory-create-users.md). Eklenen kullanıcılar varsayılan olarak yönetici izinlerine sahip olmaz ancak bu kullanıcılara herhangi bir zamanda roller atayabilirsiniz.

## Kullanıcı ekleme

1. Dizin için genel yönetici olan bir hesapla [klasik Azure portalında](https://manage.windowsazure.com) oturum açın.

2. **Active Directory**'yi seçin ve ardından dizininizi açın.

3. **Users (Kullanıcılar)** sekmesini seçin ve ardından komut çubuğunda **Add User (Kullanıcı Ekle)** seçeneğini belirleyin.

4. **Tell us about this user (Bu kullanıcı hakkındaki görüşlerinizi bize bildirin)** sayfasında, **Type of user (Kullanıcı türü)** kısmında aşağıdaki seçeneklerden birini belirleyin:

    - **User in another Azure AD directory (Başka bir Azure AD dizinindeki kullanıcı)** - kaynağı başka bir Azure AD dizini olan bir kullanıcı hesabını dizininize ekler. Başka bir dizindeki bir kullanıcıyı yalnızca bu dizinin de bir kullanıcısı olduğunuzda ekleyebilirsiniz.
    - **Users in partner companies (İş ortağı şirketlerindeki kullanıcılar)** - iş ortağı şirketi kullanıcılarını dizininize davet etmek ve yetkilendirmek için kullanılır (bkz. [Azure Active Directory B2B işbirliği](active-directory-b2b-what-is-azure-ad-b2b.md)). [E-posta adreslerini belirterek bir CSV dosyasını karşıya yüklemeniz](active-directory-b2b-references-csv-file-format.md) gerekir.

6. **Profile (Profil)** sayfasında bir ad ve soyad, kolay ad ve **Roles (Roller)** listesinden bir kullanıcı rolü sağlayın. Kullanıcı ve yönetici rolleri hakkında daha fazla bilgi için bkz. [Azure AD'de yönetici rolü atama](active-directory-assign-admin-roles.md). Kullanıcı için **Enable Multi-Factor Authentication (Multi-Factor Authentication'ı Etkinleştir)** seçeneğinin belirlenip belirlenmeyeceğini belirtin.

7. **Get temporary password (Geçici parola alma)** sayfasında, **Create (Oluştur)** seçeneğini belirleyin.

> [AZURE.IMPORTANT] Kuruluşunuz birden fazla etki alanı kullanıyorsa bir kullanıcı hesabını eklerken aşağıdakileri bilmeniz gerekir:
>
> - Etki alanları arasında aynı kullanıcı asıl adına (UPN) sahip kullanıcı hesaplarını eklemek İÇİN örnek olarak **önce** geoffgrisso@contoso.onmicrosoft.com'u ve **ardından** geoffgrisso@contoso.com'u ekleyin.
> - geoffgrisso@contoso.com'u geoffgrisso@contoso.onmicrosoft.com'dan önce **eklemeyin**. Bu sıra önemlidir, sıralamanın geri alınması ise çok uğraşmayı gerektirebilir.

Bilgilerini değiştirdiğiniz bir kullanıcının kimliği şirket içi Active Directory hizmetinizle eşitlenmişse kullanıcı bilgilerini klasik Azure portalında değiştiremezsiniz. Kullanıcı bilgilerini değiştirmek için şirket içi Active Directory yönetim araçlarınızı kullanın.

## Dış kullanıcılar ekleme

Aynı zamanda ait olduğunuz başka bir Azure AD dizininden veya bir CSV dosyasını karşıya yükleyerek iş ortağı şirketlerden kullanıcılar ekleyebilirsiniz. **Type of User (Kullanıcı Türü)** için bir dış kullanıcı eklerken **User in another Microsoft Azure AD directory (Başka bir Microsoft Azure AD dizinindeki kullanıcı)** veya **Users in partner companies (İş ortağı şirketlerindeki kullanıcılar)** seçeneğini belirtin.

Her iki türdeki kullanıcıların da kaynağı başka bir dizindir ve **dış kullanıcılar** olarak eklenirler. Dış kullanıcılar, yeni hesaplar ve kimlik bilgileri ekleme gereksinimleri olmadan bir dizindeki diğer kullanıcılarla işbirliği yapabilir. Dış kullanıcılar oturum açarken giriş dizinleriyle kimlik doğrulaması yapar ve bu kimlik doğrulaması eklenmiş oldukları diğer tüm dizinlerde de geçerli olur.

## Dış kullanıcı yönetimi ve sınırlamalar

Başka bir dizinden kendi dizininize kullanıcı eklediğinizde bu kullanıcı sizin dizininizde bir dış kullanıcı olur. Görünen ad ve kullanıcı adı bu kullanıcının giriş dizininden kopyalanır ve sizin dizininizdeki dış kullanıcı için kullanılır. Daha sonrasında dış kullanıcı hesabının özellikleri tamamen bağımsız olur. Kullanıcının kendi giriş dizininde özellikleri değiştirilirse bu değişiklikler dizininizdeki dış kullanıcı hesabına yayılmaz.

İki hesap arasındaki tek bağlantı, kullanıcının her zaman kendi giriş dizininde veya Microsoft hesabında kimlik doğrulaması yapmasıdır. Bir dış kullanıcı için parolayı sıfırlama veya çok faktörlü kimlik doğrulaması seçeneğini görmemenizin nedeni budur. Şu anda kullanıcı oturum açtığı zaman değerlendirilen tek ilke, giriş dizininin veya Microsoft hesabının kimlik doğrulama ilkesidir.

> [AZURE.NOTE]
> Dış kullanıcıyı dizinde yine de devre dışı bırakabilirsiniz, bu durumda dizininize erişim engellenir.

Bir kullanıcı kendi giriş dizininde silinirse veya Microsoft hesabını iptal ederse sizin dizininizde dış kullanıcı var olmaya devam eder. Ancak dizininizdeki kullanıcı bir giriş dizininde veya Microsoft hesabında kimlik doğrulaması yapamadığı için kaynaklara erişemez.

### Şu anda Azure AD dış kullanıcılarının erişimini destekleyen hizmetler

- **Klasik Azure portalı**: Birden çok dizinin yöneticisi olan bir dış kullanıcının bu dizinlerin her birini yönetmesine izin verir.
- **SharePoint Online**: Dış paylaşım etkinleştirilmişse bir dış kullanıcının SharePoint Online yetkili kaynaklarına erişmesine izin verir.
- **Dynamics CRM**: Kullanıcı PowerShell yoluyla lisanslanmışsa bir dış kullanıcının Dynamics CRM'deki yetkili kaynaklara erişmesine izin verir.
- **Dynamics AX**: Kullanıcı PowerShell yoluyla lisanslanmışsa bir dış kullanıcının Dynamics AX'teki yetkili kaynaklara erişmesine izin verir. [Azure AD dış kullanıcıları](#known-limitations-of-azure-ad-external-users) ve [Konuk kullanıcılar](#guest-user-management-and-limitations) için geçerli olan sınırlamalar Dynamics AX'teki dış kullanıcılar için de geçerlidir.

### Azure AD dış kullanıcılarının bilinen sınırlamaları

- Yönetici olan dış kullanıcılar, iş ortağı şirketlerdeki kullanıcıları kendi dizinlerinin (B2B işbirliği) dışındaki dizinlere ekleyemez
- Dış kullanıcılar kendi dizinlerinin dışındaki dizinlerde çok kiracılı uygulamalara onay veremez
- PowerBI şu anda dış kullanıcıların erişimini desteklememektedir
- Office Portalı dış kullanıcıların lisanslanmasına desteklemez
- Azure AD PowerShell'de dış kullanıcılar kendi giriş dizinlerinde oturum açar ve dış kullanıcı oldukları dizinleri yönetemezler


## Sırada ne var?

- [Azure Active Directory'ye yeni kullanıcı ekleme](active-directory-create-users.md)
- [Azure AD'yi yönetme](active-directory-administer.md)
- [Azure AD'de parolaları yönetme](active-directory-manage-passwords.md)
- [Azure AD'de grupları yönetme](active-directory-manage-groups.md)



<!--HONumber=Aug16_HO4-->


