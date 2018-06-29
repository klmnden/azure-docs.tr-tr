---
title: Azure Active Directory B2C için sık sorulan sorular | Microsoft Docs
description: Sık Azure Active Directory B2C hakkında sorulan sorular (SSS).
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 08/16/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 11335079373b627bab923f63eda148b0a74307ea
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37100530"
---
# <a name="azure-ad-b2c-frequently-asked-questions-faq"></a>Azure AD B2C: Sık sorulan sorular (SSS) 
Bu sayfa, Azure Active Directory (Azure AD) B2C hakkında sık sorulan sorular yanıtlanmaktadır. Geri Güncelleştirmeler denetleniyor tutun.

### <a name="can-i-use-azure-ad-b2c-features-in-my-existing-employee-based-azure-ad-tenant"></a>My var olan, çalışan tabanlı Azure AD kiracısı Azure AD B2C özellikleri kullanabilir miyim?
Azure AD ve Azure AD B2C ayrı ürün teklifleri ve aynı Kiracı içinde bir arada bulunamaz.  Azure AD kiracısı bir kuruluşu temsil eder.  Azure AD B2C kiracısı ile bağlı olan taraf uygulamaları kullanılacak kimlikleri koleksiyonunu temsil eder.  Özel ilkelerinde ile (genel Önizleme), Azure AD B2C, Azure ad kimlik doğrulama çalışanların bir kuruluşta izin vererek devredebilir.

### <a name="can-i-use-azure-ad-b2c-to-provide-social-login-facebook-and-google-into-office-365"></a>Azure AD B2C sosyal oturum açma (Facebook ve Google +) sağlamak için Office 365'te kullanabilir miyim?
Azure AD B2C, Microsoft Office 365 için kullanıcıların kimliğini doğrulamak için kullanılamaz.  Azure AD SaaS uygulamalara çalışan erişimi yönetmek için Microsoft çözümü ve lisans ve koşullu erişim gibi bu amaç için tasarlanmış özellikler vardır.  Azure AD B2C, web ve mobil uygulamaları oluşturmak için bir kimlik ve erişim yönetim platformu sağlar.  Azure AD B2C için Azure AD kiracısı birleştirmek için yapılandırıldığında, Azure AD kiracısı Azure AD B2C kullanan uygulamalara çalışan erişimi yönetir.

### <a name="what-are-local-accounts-in-azure-ad-b2c-how-are-they-different-from-work-or-school-accounts-in-azure-ad"></a>Azure AD B2C yerel hesaplarında nelerdir? Nasıl bunlar Azure AD içinde iş veya Okul hesapları farklı misiniz?
Bir Azure AD kiracısında kiracıya ait olan kullanıcıların oturum açma formu bir e-posta adresi `<xyz>@<tenant domain>`.  `<tenant domain>` Kiracı ya da ilk doğrulanmış etki alanlarına biri `<...>.onmicrosoft.com` etki alanı. Bu hesap türü bir iş veya Okul hesabıdır.

Bir Azure AD B2C kiracısı çoğu uygulamanın herhangi rastgele e-posta adresiyle oturum açmak için kullanıcının istediğiniz (örneğin, joe@comcast.net, bob@gmail.com, sarah@contoso.com, veya jim@live.com). Bu hesap türü, bir yerel hesaptır.  Ayrıca isteğe bağlı bir kullanıcı adları yerel hesaplar (örneğin, Can, bob, sarah veya jim) destekliyoruz. Azure AD B2C kimlik sağlayıcıları Azure portalında yapılandırırken bu iki yerel hesap türünü birini seçebilirsiniz. Azure AD B2C kiracınızda tıklatın **kimlik sağlayıcıları** ve ardından **kullanıcıadı** yerel hesaplar. 

Uygulamaları için kullanıcı hesapları, her zaman bir kayıt ilkesi, kaydolma veya oturum aracılığıyla oluşturulmalıdır-İlkesi'nde veya Azure AD Graph API kullanarak. Azure portalında oluşturulan kullanıcı hesapları yalnızca Kiracı yönetmek için kullanılır.

### <a name="which-social-identity-providers-do-you-support-now-which-ones-do-you-plan-to-support-in-the-future"></a>Hangi sosyal kimlik sağlayıcıları, artık destekliyor musunuz? Hangilerinin gelecekte destek planlıyor musunuz?
Şu anda Facebook, Google +, LinkedIn, Amazon, Twitter (Önizleme), WeChat (Önizleme), Weibo (Önizleme) ve h destekliyoruz (Önizleme). Müşteri talebe göre diğer popüler sosyal kimlik sağlayıcıları için destek ekleyeceğiz.

Azure AD B2C için destek de ekledi [özel ilkeler](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview-custom).  Bunlar [özel ilkeler](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview-custom) destekleyen herhangi bir kimlik sağlayıcısı ile kendi ilke oluşturmak bir geliştirici izin [Openıd Connect](http://openid.net/specs/openid-connect-core-1_0.html) veya SAML. 

Kullanıma göre özel ilkelerini kullanmaya başlama bizim [özel ilke başlangıç paketi](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack).

### <a name="can-i-configure-scopes-to-gather-more-information-about-consumers-from-various-social-identity-providers"></a>Çeşitli sosyal kimlik sağlayıcılardan tüketicileri hakkında daha fazla bilgi toplamak üzere kapsamını yapılandırabilir miyim?
Hayır, ancak bu özellik üzerinde bizim yol haritası. Desteklenen bizim sosyal kimlik sağlayıcıları kümesi için kullanılan varsayılan kapsamları şunlardır:

* Facebook: e-posta
* Google +: e-posta
* Microsoft hesabı: openıd e-posta profili
* Amazon: profili
* LinkedIn: r_emailaddress, r_basicprofile

### <a name="does-my-application-have-to-be-run-on-azure-for-it-work-with-azure-ad-b2c"></a>Uygulamam Azure üzerinde çalışmak için Azure AD B2C ile çalışacak şekilde var mı?
Hayır, uygulamanızda herhangi bir yerde (Bulut veya şirket içi) barındırabilir. Azure AD B2C ile etkileşim kurmak için gereken tek şey genel olarak erişilebilir uç noktaları HTTP isteklerini gönderip yeteneği.

### <a name="i-have-multiple-azure-ad-b2c-tenants-how-can-i-manage-them-on-the-azure-portal"></a>Birden çok Azure AD B2C Kiracı var. Nasıl bunları Azure Portal'da yönetebilirim?
Azure portalının sol tarafındaki menüde 'Azure AD B2C' açmadan önce yönetmek istediğiniz dizine geçmesi gerekir.  Kimliğinizi Azure portalının sağ üst tıklatarak dizinleri geçiş sonra aşağı açılan dizininde görüntülenen seçin.  Bir adım adım için görüntülerle görün [gitmek için Azure AD B2C ayarlarını](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).

### <a name="how-do-i-customize-verification-emails-the-content-and-the-from-field-sent-by-azure-ad-b2c"></a>Nasıl doğrulama e-postaları özelleştirebilirim (içerik ve "den:" alan) Azure AD B2C tarafından gönderilen?
Kullanabileceğiniz [şirket markası özelliğini](../active-directory/fundamentals/customize-branding.md) doğrulama e-posta içeriğini özelleştirmek için. Özellikle, bu iki öğenin e-postanın özelleştirilebilir:

* **Kapak sayfası logosu**: sağ alt köşesinde gösterilir.
* **Arka plan rengi**: en üstte gösterilen.

    ![Özelleştirilmiş doğrulama e-posta ekran görüntüsü](./media/active-directory-b2c-faqs/company-branded-verification-email.png)

E-posta imza Azure AD B2C kiracısı ilk oluşturduğunuzda belirttiğiniz Azure AD B2C kiracının adını içerir. Bu yönergeleri kullanarak adı değiştirebilirsiniz:

1. Oturum [Azure portal](https://portal.azure.com/) genel yönetici olarak.
1. Açık **Azure Active Directory** dikey.
1. Tıklatın **özellikleri** sekmesi.
1. Değişiklik **adı** alan.
1. Tıklatın **kaydetmek** sayfanın üst kısmındaki.

Şu anda değiştirmek için bir yolu yoktur "den:" e-posta alan. Oy [feedback.azure.com](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/15334335-fully-customizable-verification-emails) doğrulama e-posta gövdesi özelleştirme ilgilendiğiniz.

### <a name="how-can-i-migrate-my-existing-user-names-passwords-and-profiles-from-my-database-to-azure-ad-b2c"></a>Nasıl my varolan kullanıcı adları, parolalar ve profilleri my veritabanından Azure AD B2C'ye geçişini sağlayabilir miyim?
Geçiş Aracı yazmak için Azure AD grafik API'sini kullanabilirsiniz. Bkz: [Kullanıcı Geçiş Kılavuzu](active-directory-b2c-user-migration.md) Ayrıntılar için.

### <a name="what-password-policy-is-used-for-local-accounts-in-azure-ad-b2c"></a>Parola ilkeleri, Azure AD B2C'de yerel hesaplar için kullanılır?
Yerel hesaplar için Azure AD B2C parola ilkesi ilkesi için Azure AD temel alır. Azure AD B2C kaydı, kayıt veya oturum açma ve parola ilkeleri kullanır "güçlü" parola gücünü sıfırlamak ve parolaları süresi sona ermiyor. Okuma [Azure AD parola ilkesi](https://msdn.microsoft.com/library/azure/jj943764.aspx) daha fazla ayrıntı için.

### <a name="can-i-use-azure-ad-connect-to-migrate-consumer-identities-that-are-stored-on-my-on-premises-active-directory-to-azure-ad-b2c"></a>Azure AD B2C my şirket içi Active Directory'de depolanan tüketici kimliği geçirmek için Azure AD Connect kullanabilir miyim?
Hayır, Azure AD Connect, Azure AD B2C ile çalışmak için tasarlanmamıştır. Kullanmayı [grafik API'si](active-directory-b2c-devquickstarts-graph-dotnet.md) kullanıcı geçişi için.  Bkz: [Kullanıcı Geçiş Kılavuzu](active-directory-b2c-user-migration.md) Ayrıntılar için.

### <a name="can-my-app-open-up-azure-ad-b2c-pages-within-an-iframe"></a>Uygulamam IFRAME içinde Azure AD B2C sayfalar yukarı açabilir miyim?
Hayır, güvenlik nedenleriyle, Azure AD B2C sayfaları IFRAME içinde açılamaz.  Hizmetimizi IFRAMES engellemek için tarayıcı ile iletişim kurar.  Genel ve OAUTH2 belirtimi güvenlik topluluğu tıklatın jacking riskini nedeniyle kimlik deneyimi için IFRAMES kullanmanızı karşı öneririz.

### <a name="does-azure-ad-b2c-work-with-crm-systems-such-as-microsoft-dynamics"></a>Azure AD B2C gibi Microsoft Dynamics CRM sistemleri ile çalışır mı?
Microsoft Dynamics 365 portalı ile tümleştirme kullanılabilir.  Bkz: [Azure AD B2C kimlik doğrulaması için kullanılacak Dynamics 365 Portal Yapılandırma](https://docs.microsoft.com/dynamics365/customer-engagement/portals/azure-ad-b2c).

### <a name="does-azure-ad-b2c-work-with-sharepoint-on-premises-2016-or-earlier"></a>Azure AD B2C mu iş SharePoint şirket içi 2016 veya önceki?
Azure AD B2C SharePoint dış iş ortağı paylaşımı senaryo için tasarlanmamıştır; bkz: [Azure AD B2B](https://cloudblogs.microsoft.com/enterprisemobility/2015/09/15/learn-all-about-the-azure-ad-b2b-collaboration-preview/) yerine.

### <a name="should-i-use-azure-ad-b2c-or-b2b-to-manage-external-identities"></a>Azure AD B2C veya B2B dış kimlikleri yönetmek için kullanmalıyım?
Bu makaleyi okuyun [dış kimlikler](../active-directory/active-directory-b2b-compare-external-identities.md) Dış kimlik senaryolarınız için uygun özellikleri uygulama hakkında daha fazla bilgi için.

### <a name="what-reporting-and-auditing-features-does-azure-ad-b2c-provide-are-they-the-same-as-in-azure-ad-premium"></a>Hangi raporlama ve Özellikler Denetim Azure AD B2C sağlar? Azure AD Premium ile aynı oldukları?
Hayır, Azure AD B2C Azure AD Premium aynı kümesi raporları desteklemiyor. Ancak birçok commonalities vardır:

* **Oturum açma raporları** her oturum açma kaydı ile sınırlı ayrıntıları sağlayın.
* **Denetim raporları** hem yönetici etkinliği, hem de uygulama etkinlik içerir. 
* **Kullanım raporları** kullanıcı sayısı, oturum açma sayısı ve MFA hacmi içerir. 

### <a name="can-i-localize-the-ui-of-pages-served-by-azure-ad-b2c-what-languages-are-supported"></a>Azure AD B2C tarafından sunulan sayfalardaki UI yerelleştirme? Hangi dilleri destekleniyor mu?
Evet!  Hakkında bilgi edinin [dil özelleştirme](active-directory-b2c-reference-language-customization.md), genel önizlemede değil.  36 diller için çeviriler sağladığımız ve gereksinimlerinize uygun olarak herhangi bir dize geçersiz kılabilirsiniz.

### <a name="can-i-use-my-own-urls-on-my-sign-up-and-sign-in-pages-that-are-served-by-azure-ad-b2c-for-instance-can-i-change-the-url-from-loginmicrosoftonlinecom-to-logincontosocom"></a>Azure AD B2C tarafından sunulan my kaydolma ve oturum açma sayfalarında kendi URL'leri kullanabilir miyim? Örneğin, URL login.microsoftonline.com login.contoso.com için değiştirebilirim?
Şu anda değil. Bu özellik bizim yol haritası üzerinde kullanılabilir. Etki alanınızda doğrulama **etki alanları** Azure portalında sekmesinde bu hedef gerçekleştirmek değil.

### <a name="how-do-i-delete-my-azure-ad-b2c-tenant"></a>My Azure AD B2C kiracısı nasıl silebilirim?
Azure AD B2C kiracınızın silmek için aşağıdaki adımları izleyin:

1. Aşağıdaki adımları izleyin [Azure AD B2C ayarlarına gidin](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) Azure portalındaki.
1. Gidin **uygulamaları**, **kimlik sağlayıcıları**, ve **tüm ilkeler** ve bunların her birini tüm girişleri silin.
1. Şimdi oturum [Azure portal](https://portal.azure.com/) Abonelik Yöneticisi olarak. (Aynı iş veya Okul hesabı veya Azure'a kaydolmak için kullandığınız aynı Microsoft hesabı kullanın.)
1. Anahtar, silmek istediğiniz Azure AD B2C kiracısı.
2. Soldaki Active Directory menüsüne gidin.
3. Seçin **kullanıcılar ve gruplar**.
4. Her kullanıcı Aç (dışlamak, şu anda olarak oturum açtınız Abonelik Yöneticisi kullanıcı) seçin. Tıklatın **silmek** tıklatın ve sayfanın altındaki **Evet** istendiğinde.
5. Tıklatın **uygulama kayıtlar**.
6. Adlı uygulama seçin **b2c uzantıları uygulaması**. Tıklatın **silmek** tıklatıp **Evet** istendiğinde.
7. **Genel Bakış**’ı seçin.
8. Tıklatın **Delete directory**. İşlemi tamamlamak için ekrandaki yönergeleri izleyin.

### <a name="can-i-get-azure-ad-b2c-as-part-of-enterprise-mobility-suite"></a>Azure AD B2C Enterprise Mobility Suite'in parçası olarak alabilir miyim?
Hayır, Azure AD B2C Kullandıkça Öde Azure hizmeti ve Enterprise Mobility Suite'in parçası değil.

### <a name="how-do-i-report-issues-with-azure-ad-b2c"></a>Azure AD B2C ile ilgili sorunları nasıl rapor edebilirim?
Bkz: [Azure Active Directory B2C için dosya desteği istekleri](active-directory-b2c-support.md).
