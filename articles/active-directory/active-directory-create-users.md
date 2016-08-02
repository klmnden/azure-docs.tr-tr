<properties
    pageTitle="Azure Active Directory'ye yeni kullanıcı ekleme | Microsoft Azure"
    description="Azure Active Directory'de yeni kullanıcıların eklenmesini veya kullanıcı bilgilerinin değiştirilmesini açıklar."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="stevenpo"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="03/31/2016"
    ms.author="curtand;viviali"/>

# Azure Active Directory'ye yeni kullanıcı veya Microsoft hesabı olan bir kullanıcı ekleme

Dizininizi doldurmak için kullanıcılar ekleyin. Bu makalede kuruluşunuzdaki yeni kullanıcıların ve Microsoft hesabına sahip kullanıcıların nasıl ekleneceği açıklanmaktadır. Azure Active Directory'de diğer dizinlerden kullanıcı ekleme veya iş ortağı şirketlerden kullanıcı ekleme hakkında daha fazla bilgi için bkz. [Azure Active Directory'de diğer dizinlerden veya iş ortağı şirketlerden kullanıcılar ekleme](active-directory-create-users-external.md). Eklenen kullanıcılar varsayılan olarak yönetici izinlerine sahip olmaz ancak bu kullanıcılara herhangi bir zamanda roller atayabilirsiniz.

## Kullanıcı ekleme

1. Dizin için genel yönetici olan bir hesapla [klasik Azure portalında](https://manage.windowsazure.com) oturum açın.
2. **Active Directory**'yi seçin ve ardından kuruluş dizininizin adını seçin.
3. **Kullanıcılar** sekmesini seçin ve ardından komut çubuğunda **Kullanıcı Ekle**'yi seçin.
4. **Bu kullanıcı hakkındaki görüşlerinizi bize bildirin** sayfasında, **Kullanıcı türü** kısmında aşağıdaki seçeneklerden birini belirleyin:

    - **Kuruluşunuzdaki yeni kullanıcı** – dizininize yeni bir kullanıcı hesabı ekler.
    - **Var olan bir Microsoft hesabı olan kullanıcı** - Var olan bir Microsoft tüketici hesabını dizininize ekler (örneğin, bir Outlook hesabı)

5. **Kullanıcı türüne** bağlı olarak, bir kullanıcı adı (yeni kullanıcı için) veya bir e-posta adresi (Microsoft hesabı olan bir kullanıcı için) girin.
6. **Profil** sayfasında bir ad ve soyad, kolay ad ve **Roller** listesinden bir kullanıcı rolü sağlayın. Kullanıcı ve yönetici rolleri hakkında daha fazla bilgi için bkz. [Azure AD'de yönetici rolü atama](active-directory-assign-admin-roles.md). Kullanıcı için **Multi-Factor Authentication'ın Etkinleştirilip Etkinleştirilmeyeceğini** belirtin.
7. **Geçici parola al** sayfasında, **Oluştur**'u seçin.

> [AZURE.IMPORTANT] Kuruluşunuz birden fazla etki alanı kullanıyorsa bir kullanıcı hesabını eklerken aşağıdakileri bilmeniz gerekir:
>
> - Etki alanları arasında aynı kullanıcı asıl adına (UPN) sahip kullanıcı hesaplarını eklemek İÇİN örnek olarak **önce** geoffgrisso@contoso.onmicrosoft.com'u ve **ardından** geoffgrisso@contoso.com'u ekleyin.
> - geoffgrisso@contoso.com'u geoffgrisso@contoso.onmicrosoft.com'dan önce **eklemeyin**. Bu sıra önemlidir, sıralamanın geri alınması ise çok uğraşmayı gerektirebilir.

## Kullanıcı bilgilerini değiştirme

Nesne kimliği dışındaki tüm kullanıcı özniteliklerini değiştirebilirsiniz.

1. Dizininizi açın.
2. **Kullanıcılar** sekmesini ve ardından değiştirmek istediğiniz kullanıcının görünen adını seçin.
3. Değişikliklerinizi tamamlayın ve ardından **Kaydet**'e tıklayın.

Değiştirdiğiniz kullanıcı şirket içi Active Directory hizmetinizle eşitlenmişse bu yordamı kullanarak kullanıcı bilgilerini değiştiremezsiniz. Kullanıcıyı değiştirmek için şirket içi Active Directory yönetim araçlarınızı kullanın.

## Konuk kullanıcı yönetimi ve sınırlamalar

Konuk hesapları; SharePoint belgeleri, uygulamaları veya diğer Azure kaynaklarına erişmek için dizininize davet edilen, diğer dizinlerdeki kullanıcılardır. Dizininizdeki bir kullanıcı hesabının temel alınan UserType özniteliği "Konuk" olarak ayarlanmıştır. Normal kullanıcıların (özellikle de dizininizin üyelerinin) temel alınan UserType özniteliği "Üye"dir.

Konuklar, dizinde sınırlı bir haklar kümesine sahiptir. Bu haklar, Konukların dizindeki diğer kullanıcılara ait bilgileri keşfetme becerisini sınırlandırır. Ancak konuk kullanıcılar çalışmakta oldukları kaynaklarla ilişkili olan kullanıcılarla ve gruplarla etkileşim kurmaya devam edebilir. Konuk kullanıcılar şunları yapabilir:

- Atanmış oldukları bir Azure aboneliği ile ilişkili diğer kullanıcıları ve grupları görme
- Ait oldukları grupların üyelerini görme
- Dizinde bulunan tam e-posta adresini bildikleri diğer kullanıcıları arama
- Aradıkları kullanıcılara ait yalnızca sınırlı bir öznitelik kümesini görme; bu öznitelikler görünen ad, e-posta adresi, kullanıcı asıl adı (UPN) ve küçük resim fotoğrafı ile sınırlıdır
- Dizinde doğrulanan etki alanlarının bir listesini alma
- Uygulamalara onay verme, bu uygulamalara dizininizdeki Üyelerin sahip olduklarıyla aynı erişimi sağlama

## Konuk kullanıcı erişim ilkeleri ayarlama

Bir dizinin **Yapılandır** sekmesinde, konuk kullanıcıların erişimini denetlemeyi sağlayan seçenekler bulunur. Bu seçenekler yalnızca klasik Azure portalında bir dizin genel yöneticisi tarafından değiştirilebilir. Şu anda bir PowerShell veya API yöntemi bulunmamaktadır.

Klasik Azure portalında **Yapılandır** sekmesini açmak için **Active Directory**'yi seçin ve ardından dizinin adını seçin.

![Azure Active Directory'deki Yapılandır sekmesi][1]

Ardından konuk kullanıcıların erişimini denetleme seçeneklerini düzenleyebilirsiniz.

![konuk kullanıcılara yönelik erişim denetimi seçenekleri][2]


## Sırada ne var?

- [Azure Active Directory'de diğer dizinlerden veya iş ortağı şirketlerden kullanıcılar ekleme](active-directory-create-users-external.md)
- [Azure AD'yi yönetme](active-directory-administer.md)
- [Azure AD'de parolaları yönetme](active-directory-manage-passwords.md)
- [Azure AD'de grupları yönetme](active-directory-manage-groups.md)

<!--Image references-->
[1]: ./media/active-directory-create-users/RBACDirConfigTab.png
[2]: ./media/active-directory-create-users/RBACGuestAccessControls.png



<!--HONumber=Jun16_HO2-->


