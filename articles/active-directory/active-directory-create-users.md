---
title: "Azure Active Directory&quot;ye yeni kullanıcı ekleme | Microsoft Belgeleri"
description: "Azure Active Directory&quot;de yeni kullanıcıların eklenmesini veya kullanıcı bilgilerinin değiştirilmesini açıklar."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: e3673727-6bec-4fdc-87a4-d65b213c4c3c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/22/2016
ms.author: curtand
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: f3787f72dbd8ee865899b71538816d2e8d30af32


---
# <a name="add-new-users--or-users-with-microsoft-accounts-to-azure-active-directory"></a>Azure Active Directory'ye yeni kullanıcı veya Microsoft hesabı olan bir kullanıcı ekleme
Dizininizi doldurmak için kullanıcılar ekleyin. Bu makalede kuruluşunuzdaki yeni kullanıcıların ve Microsoft hesabına sahip kullanıcıların nasıl ekleneceği açıklanmaktadır. Azure Active Directory'de diğer dizinlerden kullanıcı ekleme veya iş ortağı şirketlerden kullanıcı ekleme hakkında daha fazla bilgi için bkz. [Azure Active Directory'de diğer dizinlerden veya iş ortağı şirketlerden kullanıcılar ekleme](active-directory-create-users-external.md). Eklenen kullanıcılar varsayılan olarak yönetici izinlerine sahip olmaz ancak bu kullanıcılara herhangi bir zamanda roller atayabilirsiniz.

## <a name="add-a-user"></a>Kullanıcı ekleme
1. Dizin için genel yönetici olan bir hesapla [klasik Azure portalında](https://manage.windowsazure.com) oturum açın.
2. **Active Directory**'yi seçin ve ardından kuruluş dizininizin adını seçin.
3. **Users (Kullanıcılar)** sekmesini seçin ve ardından komut çubuğunda **Add User (Kullanıcı Ekle)** seçeneğini belirleyin.
4. **Tell us about this user (Bu kullanıcı hakkındaki görüşlerinizi bize bildirin)** sayfasında, **Type of user (Kullanıcı türü)** kısmında aşağıdaki seçeneklerden birini belirleyin:
   
   * **New user in your organization (Kuruluşunuzdaki yeni kullanıcı)** - dizininize yeni bir kullanıcı hesabı ekler.
   * **User with an existing Microsoft account (Var olan bir Microsoft hesabı olan kullanıcı)** - Var olan bir Microsoft tüketici hesabını dizininize ekler (örneğin, bir Outlook hesabı)
5. **Type of User (Kullanıcı Türü)** seçeneğine bağlı olarak, bir kullanıcı adı (yeni kullanıcı için) veya bir e-posta adresi (Microsoft hesabı olan bir kullanıcı için) girin.
6. **Profile (Profil)** sayfasında bir ad ve soyad, kolay ad ve **Roles (Roller)** listesinden bir kullanıcı rolü sağlayın. Kullanıcı ve yönetici rolleri hakkında daha fazla bilgi için bkz. [Azure AD'de yönetici rolü atama](active-directory-assign-admin-roles.md). Kullanıcı için **Enable Multi-Factor Authentication (Multi-Factor Authentication'ı Etkinleştir)** seçeneğinin belirlenip belirlenmeyeceğini belirtin.
7. **Get temporary password (Geçici parola alma)** sayfasında, **Create (Oluştur)** seçeneğini belirleyin.

> [!IMPORTANT]
> Kuruluşunuz birden fazla etki alanı kullanıyorsa bir kullanıcı hesabını eklerken aşağıdakileri bilmeniz gerekir:
> 
> * Etki alanları arasında aynı kullanıcı asıl adına (UPN) sahip kullanıcı hesaplarını eklemek için örnek olarak **önce** geoffgrisso@contoso.onmicrosoft.com,, **ardından** geoffgrisso@contoso.com. ekleyin.
> * geoffgrisso@contoso.onmicrosoft.com. eklemeden önce geoffgrisso@contoso.com **eklemeyin**. Bu sıra önemlidir, sıralamanın geri alınması ise çok uğraşmayı gerektirebilir.
> 
> 

## <a name="change-user-information"></a>Kullanıcı bilgilerini değiştirme
Nesne kimliği dışındaki tüm kullanıcı özniteliklerini değiştirebilirsiniz.

1. Dizininizi açın.
2. **Users (Kullanıcılar)** sekmesini ve ardından değiştirmek istediğiniz kullanıcının görünen adını seçin.
3. Değişikliklerinizi tamamlayın ve ardından **Save (Kaydet)** düğmesine tıklayın.

Değiştirdiğiniz kullanıcı şirket içi Active Directory hizmetinizle eşitlenmişse bu yordamı kullanarak kullanıcı bilgilerini değiştiremezsiniz. Kullanıcıyı değiştirmek için şirket içi Active Directory yönetim araçlarınızı kullanın.

## <a name="guest-user-management-and-limitations"></a>Konuk kullanıcı yönetimi ve sınırlamalar
Konuk hesapları; SharePoint belgeleri, uygulamaları veya diğer Azure kaynaklarına erişmek için dizininize davet edilen, diğer dizinlerdeki kullanıcılardır. Dizininizdeki bir kullanıcı hesabının temel alınan UserType özniteliği "Konuk" olarak ayarlanmıştır. Normal kullanıcıların (özellikle de dizininizin üyelerinin) temel alınan UserType özniteliği "Üye"dir.

Konuklar, dizinde sınırlı bir haklar kümesine sahiptir. Bu haklar, Konukların dizindeki diğer kullanıcılara ait bilgileri keşfetme becerisini sınırlandırır. Ancak konuk kullanıcılar çalışmakta oldukları kaynaklarla ilişkili olan kullanıcılarla ve gruplarla etkileşim kurmaya devam edebilir. Konuk kullanıcılar şunları yapabilir:

* Atanmış oldukları bir Azure aboneliği ile ilişkili diğer kullanıcıları ve grupları görme
* Ait oldukları grupların üyelerini görme
* Dizinde bulunan tam e-posta adresini bildikleri diğer kullanıcıları arama
* Aradıkları kullanıcılara ait yalnızca sınırlı bir öznitelik kümesini görme; bu öznitelikler görünen ad, e-posta adresi, kullanıcı asıl adı (UPN) ve küçük resim fotoğrafı ile sınırlıdır
* Dizinde doğrulanan etki alanlarının bir listesini alma
* Uygulamalara onay verme, bu uygulamalara dizininizdeki Üyelerin sahip olduklarıyla aynı erişimi sağlama

## <a name="set-guest-user-access-policies"></a>Konuk kullanıcı erişim ilkeleri ayarlama
Bir dizinin **Configure (Yapılandır)** sekmesinde, konuk kullanıcıların erişimini denetlemeyi sağlayan seçenekler bulunur. Bu seçenekler yalnızca klasik Azure portalında bir dizin genel yöneticisi tarafından değiştirilebilir. Şu anda bir PowerShell veya API yöntemi bulunmamaktadır.

Klasik Azure portalında **Configure (Yapılandır)** sekmesini açmak için **Active Directory**'yi seçin ve ardından dizinin adını seçin.

![Azure Active Directory'deki Configure (Yapılandır) sekmesi][1]

Ardından konuk kullanıcıların erişimini denetleme seçeneklerini düzenleyebilirsiniz.

![konuk kullanıcılara yönelik erişim denetimi seçenekleri][2]

## <a name="whats-next"></a>Sırada ne var?
* [Azure Active Directory'de diğer dizinlerden veya iş ortağı şirketlerden kullanıcılar ekleme](active-directory-create-users-external.md)
* [Azure AD'yi yönetme](active-directory-administer.md)
* [Azure AD'de parolaları yönetme](active-directory-manage-passwords.md)
* [Azure AD'de grupları yönetme](active-directory-manage-groups.md)

<!--Image references-->
[1]: ./media/active-directory-create-users/RBACDirConfigTab.png
[2]: ./media/active-directory-create-users/RBACGuestAccessControls.png



<!--HONumber=Dec16_HO1-->


