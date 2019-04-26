---
title: Azure Active Directory B2C için sık sorulan sorular | Microsoft Docs
description: Sık Azure Active Directory B2C hakkında sorulan sorular (SSS).
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 7a30aecc3cc2259072ea33ae018c371a1f05741a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60318024"
---
# <a name="azure-ad-b2c-frequently-asked-questions-faq"></a>Azure AD B2C: Sık sorulan sorular (SSS) 
Bu sayfa, Azure Active Directory (Azure AD) B2C hakkında sık sorulan sorular yanıtlanmaktadır. Geri güncelleştirmeleri kontrol etmeyi unutmayın.

### <a name="why-cant-i-access-the-azure-ad-b2c-extension-in-the-azure-portal"></a>Azure portalında Azure AD B2C uzantısı neden erişemiyorum?
Neden Azure AD uzantısı, çalışmadığı için sık karşılaşılan iki nedeni vardır.  Azure AD B2C kullanıcı rolünüzün dizinde genel yönetici olmanız gerekir.  Erişimi olması düşünüyorsanız lütfen yöneticinize başvurun.  Genel yönetici ayrıcalıkları varsa, Azure AD B2C dizini ve Azure Active Directory dizin olduğundan emin olun.  Yönergeler için bkz: [Azure AD B2C kiracısı oluşturma](tutorial-create-tenant.md).

### <a name="can-i-use-azure-ad-b2c-features-in-my-existing-employee-based-azure-ad-tenant"></a>Azure AD B2C özellikleri var olan, çalışan tabanlı Azure AD kiracımı kullanabilir miyim?
Azure AD ve Azure AD B2C ayrı ürün teklifleri ve aynı kiracıda bir arada bulunamaz.  Azure AD kiracısı bir kuruluşu temsil eder.  Azure AD B2C kiracısı ile bağlı olan taraf uygulamaları kullanılacak kimlikleri koleksiyonunu temsil eder.  Özel ilkeleri ile (genel Önizleme), Azure AD B2C'yi bir kuruluşta çalışanlar kimlik doğrulaması sağlayan Azure AD'ye devredebilir.

### <a name="can-i-use-azure-ad-b2c-to-provide-social-login-facebook-and-google-into-office-365"></a>Azure AD B2C'yi (Facebook ve Google +) sosyal oturum açma adı sağlamak için Office 365'te kullanabilir miyim?
Azure AD B2C, Microsoft Office 365 için kullanıcıların kimliğini doğrulamak için kullanılamaz.  Azure AD SaaS uygulamalarına çalışan erişimi yönetmek için Microsoft'un çözümüdür ve lisans ve koşullu erişim gibi bu amaç için tasarlanmış özellikleri vardır.  Azure AD B2C, web ve mobil uygulamaları oluşturmak için bir kimlik ve erişim yönetim platformu sağlar.  Azure AD B2C, bir Azure AD kiracısına federasyona eklemek için yapılandırıldığında, Azure AD kiracısı üzerinde Azure AD B2C kullanan uygulamalara çalışan erişimi yönetir.

### <a name="what-are-local-accounts-in-azure-ad-b2c-how-are-they-different-from-work-or-school-accounts-in-azure-ad"></a>Yerel hesapları Azure AD B2C nedir? Bunların Azure AD'de iş veya Okul hesaplarını birbirinden farkı?
Bir Azure AD kiracısında kiracıya ait olan kullanıcıların oturum açma formu bir e-posta adresi `<xyz>@<tenant domain>`.  `<tenant domain>` Kiracı ya da ilk doğrulanmış etki alanları biridir `<...>.onmicrosoft.com` etki alanı. Bu hesap türü bir iş veya Okul hesabıdır.

Bir Azure AD B2C kiracısında uygulamaların çoğu kullanıcının herhangi bir rastgele bir e-posta adresi ile oturum açmak istediğiniz (örneğin, joe@comcast.net, bob@gmail.com, sarah@contoso.com, veya jim@live.com). Bu hesap türü, bir yerel hesaptır.  Rastgele kullanıcı adları yerel hesapları (örneğin, ALi, bob, sarah veya jim) destekliyoruz. Azure portalında Azure AD B2C için kimlik sağlayıcıları yapılandırma sırasında bu iki yerel hesap türünden birini seçebilirsiniz. Azure AD B2C kiracınıza tıklayın **kimlik sağlayıcıları** seçip **kullanıcıadı** altında yerel hesaplar. 

Uygulamaları için kullanıcı hesapları, her zaman kaydolma veya oturum açma kullanıcı akışı, bir kayıt kullanıcı akış yoluyla veya Azure AD Graph API'sini kullanarak oluşturulmalıdır. Azure portalında oluşturulan kullanıcı hesaplarını, yalnızca Kiracı yönetmek için kullanılır.

### <a name="which-social-identity-providers-do-you-support-now-which-ones-do-you-plan-to-support-in-the-future"></a>Hangi sosyal kimlik sağlayıcıları artık destekliyorsunuz? Hangilerinin gelecekte desteklemeyi planlıyor musunuz?
Şu anda Facebook, Google +, LinkedIn, Amazon, Twitter (Önizleme), WeChat (Önizleme), Weibo (Önizleme) ve h destekliyoruz (Önizleme). Müşteri talebine göre diğer popüler sosyal kimlik sağlayıcıları için destek ekleyeceğiz.

Azure AD B2C'yi de için destek ekledi [özel ilkeler](active-directory-b2c-overview-custom.md).  Bunlar [özel ilkeler](active-directory-b2c-overview-custom.md) destekleyen herhangi bir kimlik sağlayıcısı ile kendi ilkeleri oluşturmak için geliştirici izin [Openıd Connect](https://openid.net/specs/openid-connect-core-1_0.html) veya SAML. 

Göz atarak özel ilkeleri kullanmaya başlama bizim [özel ilke başlangıç paketi](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack).

### <a name="can-i-configure-scopes-to-gather-more-information-about-consumers-from-various-social-identity-providers"></a>Çeşitli sosyal kimlik sağlayıcılarını tüketiciler hakkında daha fazla bilgi toplamak için kapsamları yapılandırabilirim?
Hayır. Sosyal kimlik sağlayıcıları desteklenen kümemizdeki için kullanılan varsayılan kapsamları misiniz:

* Facebook: e-posta
* Google +: e-posta
* Microsoft hesabı: openıd e-posta profili
* Amazon: Profil
* LinkedIn: r_emailaddress, r_basicprofile

### <a name="does-my-application-have-to-be-run-on-azure-for-it-work-with-azure-ad-b2c"></a>Uygulamamı Azure AD B2C ile çalışmak için Azure üzerinde çalıştırılacak var mı?
Hayır, uygulamanızda herhangi bir yerde (bulutta veya şirket içi) barındırabilir. Azure AD B2C ile etkileşim kurmak için gerekli olan genel olarak erişilebilir uç noktaları üzerinde HTTP isteklerini gönderip olanağı.

### <a name="i-have-multiple-azure-ad-b2c-tenants-how-can-i-manage-them-on-the-azure-portal"></a>Birden çok Azure AD B2C kiracıları var. Nasıl bunları Azure portalında yönetebilirim?
Azure portalının sol tarafındaki menüde 'Azure AD B2C' açmadan önce yönetmek istediğiniz dizine geçiş yapmanız gerekir.  Kimliğinizi Azure portalının sağ üst kısımdaki tıklayarak dizinleri değiştirin ve ardından bir dizin, açılan menüde görüntülenen seçin.

### <a name="how-do-i-customize-verification-emails-the-content-and-the-from-field-sent-by-azure-ad-b2c"></a>Nasıl doğrulama e-postaları özelleştirebilirim (içerik ve "den:" alanı) Azure AD B2C tarafından gönderilen?
Kullanabileceğiniz [şirket markası özelliğini](../active-directory/fundamentals/customize-branding.md) doğrulama e-postaları içeriğini özelleştirmek için. Özellikle, bu iki öğenin e-postanın özelleştirilebilir:

* **Başlık logosu**: Sağ alt tarafında gösterilir.
* **Arka plan rengi**: En üstünde gösterilir.

    ![Özelleştirilmiş bir doğrulama e-postanın ekran görüntüsü](./media/active-directory-b2c-faqs/company-branded-verification-email.png)

Azure AD B2C kiracısı oluştururken sağladığınız Azure AD B2C Kiracı adının e-posta imza içeriyor. Bu yönergeleri kullanarak adını değiştirebilirsiniz:

1. Oturum [Azure portalında](https://portal.azure.com/) genel Yöneticisi olarak.
1. Açık **Azure Active Directory** dikey penceresi.
1. Tıklayın **özellikleri** sekmesi.
1. Değişiklik **adı** alan.
1. Sayfanın üst kısmından **Kaydet**'e tıklayın.

Şu anda değiştirme olanağı yoktur "den:" e-posta ile sekmesindeki.

### <a name="how-can-i-migrate-my-existing-user-names-passwords-and-profiles-from-my-database-to-azure-ad-b2c"></a>Nasıl my mevcut kullanıcı adları, parolalar ve profilleri my veritabanından Azure AD B2C'ye geçişini sağlayabilir miyim?
Azure AD Graph API, geçiş aracı yazmak için kullanabilirsiniz. Bkz: [Kullanıcı Geçiş Kılavuzu](active-directory-b2c-user-migration.md) Ayrıntılar için.

### <a name="what-password-user-flow-is-used-for-local-accounts-in-azure-ad-b2c"></a>Hangi parola kullanıcı akışını Azure AD B2C'de yerel hesaplar için kullanılır?
Yerel hesaplar için Azure AD B2C parola kullanıcı akışı ilkesi için Azure AD temel alır. Azure AD B2C kaydolma, kaydolma veya oturum açma ve parola sıfırlama kullanıcı akışları "güçlü" parola gücünü kullanın ve parolaları dolmasın. Okuma [Azure AD parola ilkesi](/previous-versions/azure/jj943764(v=azure.100)) daha fazla ayrıntı için. Hesap kilitlemeleri uygulayın ve parolaları hakkında daha fazla bilgi için bkz: [yönettiği kaynaklar ve Azure Active Directory B2C verilerinde tehditleri](active-directory-b2c-reference-threat-management.md).

### <a name="can-i-use-azure-ad-connect-to-migrate-consumer-identities-that-are-stored-on-my-on-premises-active-directory-to-azure-ad-b2c"></a>Şirket içi Active Directory dizinimde Azure AD B2C'ye depolanan müşteri kimliklerini geçirmek için Azure AD Connect kullanabilir miyim?
Hayır, Azure AD Connect, Azure AD B2C ile çalışmak için tasarlanmamıştır. Kullanmayı [Azure AD Graph API'si](active-directory-b2c-devquickstarts-graph-dotnet.md) kullanıcı geçişi için.  Bkz: [Kullanıcı Geçiş Kılavuzu](active-directory-b2c-user-migration.md) Ayrıntılar için.

### <a name="can-my-app-open-up-azure-ad-b2c-pages-within-an-iframe"></a>Uygulamamı Azure AD B2C sayfaları iFrame içinde yukarı açabilir miyim?
Hayır, güvenlik nedenleriyle, Azure AD B2C sayfaları iFrame içinde açılamaz.  Hizmetimiz IFRAMES engellemek için tarayıcı ile iletişim kurar.  Güvenlik topluluğu içinde genel ve OAUTH2 belirtimi tıklatın jacking riski nedeniyle kimlik deneyimi için iframe kullanarak karşı önerilir.

### <a name="does-azure-ad-b2c-work-with-crm-systems-such-as-microsoft-dynamics"></a>Azure AD B2C gibi Microsoft Dynamics CRM sistemleri ile çalışır mı?
Microsoft Dynamics 365 portalı ile tümleştirme kullanılabilir.  Bkz: [yapılandırma Dynamics 365 portalında Azure AD B2C kimlik doğrulaması için kullanılacak](https://docs.microsoft.com/dynamics365/customer-engagement/portals/azure-ad-b2c).

### <a name="does-azure-ad-b2c-work-with-sharepoint-on-premises-2016-or-earlier"></a>Azure AD B2C mu iş ile SharePoint şirket içi 2016 veya önceki?
Azure AD B2C, SharePoint dış iş ortağı paylaşımı senaryo için tasarlanmamıştır; bkz: [Azure AD B2B](https://cloudblogs.microsoft.com/enterprisemobility/2015/09/15/learn-all-about-the-azure-ad-b2b-collaboration-preview/) yerine.

### <a name="should-i-use-azure-ad-b2c-or-b2b-to-manage-external-identities"></a>Dış kimlikler yönetmek için Azure AD B2C'yi veya B2B kullanmalı mıyım?
Bu makaleyi okuyun [dış kimlikler](../active-directory/active-directory-b2b-compare-external-identities.md) Dış kimlik senaryolarınız için uygun özellikleri uygulama hakkında daha fazla bilgi için.

### <a name="what-reporting-and-auditing-features-does-azure-ad-b2c-provide-are-they-the-same-as-in-azure-ad-premium"></a>Azure AD B2C, hangi raporlama ve denetimi özellikleri sağlar mı? Bunlar, Azure AD Premium olduğu gibi aynı mı?
Hayır, Azure AD B2C aynı dizi rapor şeklinde Azure AD Premium desteklemez. Ancak birçok commonalities vardır:

* **Oturum açma raporları** her oturum açma kaydı ile sınırlı ayrıntıları sağlayın.
* **Denetim raporları** hem yönetici etkinliği, hem de uygulama etkinlik içerir. 
* **Kullanım raporları** kullanıcı sayısı, oturum açma sayısı ve MFA'ın birim içerir. 

### <a name="can-i-localize-the-ui-of-pages-served-by-azure-ad-b2c-what-languages-are-supported"></a>Ben Azure AD B2C tarafından sunulan sayfaları UI yerelleştirebilir mi? Hangi dillerin destekleniyor mu?
Evet!  Hakkında bilgi edinin [dil özelleştirme](active-directory-b2c-reference-language-customization.md), genel Önizleme aşamasında olduğu.  36 dillerin çevirileri sunuyoruz ve ihtiyaçlarınıza uyacak şekilde herhangi bir dize geçersiz kılabilirsiniz.

### <a name="can-i-use-my-own-urls-on-my-sign-up-and-sign-in-pages-that-are-served-by-azure-ad-b2c-for-instance-can-i-change-the-url-from-loginmicrosoftonlinecom-to-logincontosocom"></a>Azure AD B2C tarafından sunulan my kaydolma ve oturum açma sayfalarında kendi URL'leri kullanabilir miyim? Örneğin, URL login.microsoftonline.com login.contoso.com için değiştirebilirim?
Şu anda değil. Yol haritamızda özelliğidir. Etki alanınızdaki doğrulama **etki alanları** sekmesi Azure portalında bu hedefe gerçekleştirmek değil.

### <a name="how-do-i-delete-my-azure-ad-b2c-tenant"></a>Azure AD B2C kiracıma nasıl silebilirim?
Azure AD B2C kiracınızı silmek için aşağıdaki adımları izleyin:

1. Tüm kullanıcı akışları (ilke) Azure AD B2C kiracınızdaki silin.
1. Azure AD B2C kiracınızda kayıtlı olan tüm uygulamaları silin.
1. Şimdi oturum açın [Azure portalında](https://portal.azure.com/) Abonelik Yöneticisi olarak. (Aynı iş veya Okul hesabı veya Azure'a kaydolmak için kullandığınız aynı Microsoft hesabını kullanın.)
1. Geçiş için Azure AD B2C kiracısı silmek istediğiniz.
2. Soldaki Active Directory menüsüne gidin.
3. **Kullanıcı ve gruplar**'ı seçin.
4. (Şu anda olarak oturumunuz Abonelik Yöneticisi kullanıcı dahil değil) sırayla her bir kullanıcı seçin. Tıklayın **Sil** tıklayın ve sayfanın alt kısmındaki **Evet** istendiğinde.
5. Tıklayın **uygulama kayıtları**.
6. Uygulama adı seçin **b2c-extensions-app**. Tıklayın **Sil** tıklatıp **Evet** istendiğinde.
7. **Genel Bakış**’ı seçin.
8. Tıklayın **silme directory**. İşlemi tamamlamak için ekrandaki yönergeleri izleyin.

### <a name="can-i-get-azure-ad-b2c-as-part-of-enterprise-mobility-suite"></a>Enterprise Mobility Suite'in parçası olarak Azure AD B2C alabilir miyim?
Hayır, Azure AD B2C, bir Kullandıkça Öde Azure hizmeti olan ve Enterprise Mobility Suite'in bir parçası değil.

### <a name="how-do-i-report-issues-with-azure-ad-b2c"></a>Azure AD B2C ile ilgili sorunları nasıl bildirebilirim?
Bkz: [Azure Active Directory B2C için dosya desteği istekleri](active-directory-b2c-support.md).
