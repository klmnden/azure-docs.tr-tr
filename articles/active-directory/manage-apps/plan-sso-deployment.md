---
title: Oturum açma bir Azure Active Directory tek dağıtımını planlama
description: Planlama, dağıtma ve kuruluşunuzda SSO yönetmenize yardımcı olacak kılavuz.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: conceptual
ms.workload: identity
ms.date: 05/22/2019
ms.author: baselden
ms.reviewer: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: e5278d504c43688bf064b869982938db52b1b1bf
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67164948"
---
# <a name="plan-a-single-sign-on-deployment"></a>Tek bir oturum açma dağıtımı planlama

Çoklu oturum açma (SSO) tüm uygulamaları ve bir kullanıcı yalnızca tek bir kullanıcı hesabı kullanarak bir kez oturum açarak gereken kaynaklara erişmesini anlamına gelir. SSO ile ikinci kez kimlik doğrulaması için gerekli olmadan kullanıcılar gereken tüm uygulamalara erişebilirsiniz.

## <a name="benefits-of-sso"></a>SSO avantajları

Çoklu oturum açma (SSO), Azure Active Directory'de (Azure AD) uygulamalara kullanıcılar oturum açtığında güvenlik ve kolaylık ekler. 

Pek çok kuruluş yazılım üzerinde bir hizmet (SaaS) uygulamaları, Salesforce, Office 365 ve kutusu gibi için son kullanıcı üretkenliğini olarak kullanır. Tarihsel olarak, BT personeliniz ayrı ayrı oluşturup her SaaS uygulaması ve her biri için bir parola hatırlaması gerekli kullanıcıları kullanıcı hesaplarını güncelleştirmek gerekli.

Azure marketi, kiracınızda tümleştirileceği kolaylaştıran 3000'den uygulamalarını önceden tümleştirilmiş SSO bağlantılarıyla sahiptir.

## <a name="licensing"></a>Lisanslama

- **Azure AD lisanslama** -önceden tümleştirilmiş SaaS uygulamaları için SSO ücretsizdir. Ancak, dizin ve dağıtmak istediğiniz özellikleri nesne sayısını Ek lisanslar gerektirebilir. Lisans gereksinimlerinin tam listesi için bkz: [Azure Active Directory fiyatlandırma](https://azure.microsoft.com/pricing/details/active-directory/).
- **Uygulama lisanslama** -iş gereksinimlerinizi karşılamak SaaS uygulamalarınız için uygun lisanslara ihtiyacınız vardır. Uygulamaya atanmış kullanıcılar uygulamadaki kendi rolleri için uygun lisanslara sahip olup olmadığını belirlemek için uygulama sahibi çalışın. Azure AD rollerine göre otomatik sağlama yönetiyorsa, Azure AD'de atanan rollerini uygulamada ait lisans sayısı ile hizalamanız gerekir. Hatalı uygulamaya ait lisans sayısını sağlama/güncelleştirme sırasında bir kullanıcının hatalara neden olabilir.

## <a name="plan-your-sso-team"></a>SSO takımınızın planlama

- **Doğru proje katılımcıları etkileşim kurun** - teknoloji başarısız, projeler, genellikle etkisi, sonuçları ve sorumlulukları eşleşmeyen beklentileri kaynaklanır. Bu sorunları önlemek için [doğru proje katılımcıları ilgi çekici emin olun](https://aka.ms/deploymentplans) ve proje katılımcılarının rollerinin anlayın.
- **İletişimi planlama** -iletişim herhangi bir yeni hizmeti başarısı için önemlidir. Proaktif bir şekilde iletişim kurmak, kullanıcılarınızın deneyimlerini nasıl değiştireceğini, ne zaman değişir ve bunların ilgili sorun yaşıyorsanız destek elde etmek nasıl sonlandıran. Seçenekleri gözden [uygulamalarını son kullanıcılara kendi SSO nasıl erişecek etkin](end-user-experiences.md)ve seçiminizi eşleştirilecek iletişimlerin ustalıkla işleyin. 

## <a name="plan-your-sso-protocol"></a>SSO protokolünüzü planlama

Güvenlik, güvenilirlik, Federasyon protokollerini temel alan bir SSO uygulama artırır ve son kullanıcı deneyimleri ve uygulamak daha kolaydır. Birçok uygulama, Azure AD ile önceden tümleştirilmiş [adım adım kılavuzları kullanılabilir](../saas-apps/tutorial-list.md). Üzerinde bulabilirsiniz bizim [Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/). Her bir SSO yöntemi hakkında ayrıntılı bilgi makalesinde bulunabilir [Azure Active Directory'de uygulamalar için çoklu oturum açma](what-is-single-sign-on.md).

Kullanıcılarınız için çoklu oturum açma uygulamalarınıza olarak etkinleştirmek için iki temel yol vardır:

- **Federasyon çoklu oturum açma ile** Azure AD, Azure AD hesabını kullanarak uygulama için kullanıcının kimliğini doğrular. Bu yöntem destek fikirlerini modu, çoklu oturum açma SAML 2.0, WS-Federation ve Openıd Connect gibi protokolleri ve uygulamalar için desteklenir. Ne zaman bir uygulama, parola tabanlı SSO ve ADFS yerine destekleyen Azure AD ile Federasyon SSO kullanmanızı öneririz.

- **Parola tabanlı çoklu oturum açma ile** kullanıcılar uygulamada bir kullanıcı adı ve parola ile ilk kez oturum oldukları erişim. İlk oturum açma işleminden sonra Azure AD kullanıcı ve uygulama parolasını sağlar. Parola tabanlı çoklu oturum açma güvenli uygulama parola depolama ve bir web tarayıcısı uzantısı veya mobil uygulama kullanarak yeniden yürütme sağlar. Bu seçenek, uygulama tarafından sağlanan mevcut oturum açma işleminden yararlanır, yönetici parolaları yönetmek etkinleştirir ve kullanıcının parolasını bilmesini gerektirmez.

### <a name="considerations-for-federation-based-sso"></a>Federasyon tabanlı SSO için dikkat edilmesi gerekenler

- **Openıd Connect ve OAuth kullanarak** - bağlandığınızı desteklediği için uygulama kullanırsanız, OIDC/OAuth 2.0 yöntemi bu uygulama, SSO'yu etkinleştirmek üzere. Bu yöntem, daha az yapılandırma gerektirir ve daha zengin bir kullanıcı deneyimi sağlar. Daha fazla bilgi için [OAuth 2.0](../develop/v2-oauth2-auth-code-flow.md), [Openıd Connect 1.0](../develop/v2-protocols-oidc.md), ve [Azure Active Directory Geliştirici Kılavuzu](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide).
- **SAML tabanlı SSO için uç nokta yapılandırmaları** -SAML kullanırsanız, Geliştiricileriniz uygulama yapılandırmadan önce belirli bilgiler gerekecektir. Daha fazla bilgi için bkz. [temel SAML seçenekleri](configure-single-sign-on-portal.md).
- **Sertifika yönetimi için SAML tabanlı SSO** - Federasyon SSO etkinleştirdiğinizde için uygulama, Azure AD varsayılan olarak üç yıl boyunca geçerli bir sertifika oluşturur. Gerekirse, sertifika sona erme tarihini özelleştirebilirsiniz. Tarihlerinden önce sertifikaları yenilemek için yerinde bir işlemler olduğundan emin olun. Daha fazla bilgi için bkz. [Azure AD sertifikaların yönetilmesi](https://docs.microsoft.com/azure/active-directory/active-directory-sso-certs).

### <a name="considerations-for-password-based-sso"></a>Parola tabanlı SSO için dikkat edilmesi gerekenler

Azure AD için parola tabanlı SSO kullanarak oturum açma formları doldurmak güvenli bir şekilde kimlik bilgilerini almak ve bir tarayıcı uzantısı dağıtma gerektirir. Uygun ölçekte uzantısını dağıtmak için bir mekanizma tanımlamak [Desteklenen tarayıcılar](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction). Şu seçenekler mevcuttur:

- [Internet Explorer için Grup İlkesi](https://azure.microsoft.com/documentation/articles/active-directory-saas-ie-group-policy/)
- [Internet Explorer için Sistem Merkezi Yapılandırma Yöneticisi (SCCM)](https://docs.microsoft.com/sccm/core/clients/deploy/deploy-clients-to-windows-computers)
- [Kullanıcı karşıdan yükleme ve yapılandırma için Chrome, Firefox, Microsoft Edge ve IE güdümlü](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

Daha fazla bilgi için bkz. [yapılandırmak için parola tek oturum açma nasıl](https://docs.microsoft.com/azure/active-directory/application-config-sso-how-to-configure-password-sso-non-gallery).

#### <a name="capturing-login-forms-metadata-for-applications-that-arent-in-the-gallery"></a>Oturum açma forms meta verileri galerisinde bulunmayan uygulamalar için yakalama

Microsoft, bir web uygulamasında (kullanıcı adı ve parola alanları yakalayarak) vaulting parolası yakalama meta verileri destekler. Forms meta verileri yakalamak için uygulamayı yapılandırma işlemi sırasında oturum açma URL'sine gidin. Uygulama sahibi tam oturum açma URL'sini isteyin. Bu bilgiler, uygulama oturum açma sırasında Azure AD kimlik eşleme oturum açma işlemi sırasında kullanılır.

Daha fazla bilgi için bkz. [uygulama erişimi ve Azure AD ile SSO nedir?-parola tabanlı SSO](https://azure.microsoft.com/documentation/articles/active-directory-appssoaccess-whatis/).

#### <a name="indications-that-metadata-in-forms-needs-to-be-recaptured"></a>Göstergelerden Forms'ta meta verilerin edilmesine gerekiyor

Uygulamaları kendi HTML değiştirdiğimde, değişikliklerin ayarlamak için meta veriler yeniden yakalayın gerekebilir. HTML düzenini değişiklik olduğunu gösteren ortak belirtileri şunlardır:

- Kullanıcılar bu uygulamayı tıklayarak reporting "oturum açma sayfasını takılı"
- Kullanıcı adı veya parola doldurulmuş değilse raporlama kullanıcıları

#### <a name="shared-accounts"></a>Paylaşılan hesaplar

Oturum açma açısından bakıldığında, paylaşılan hesaplar ile uygulamaları bireysel kullanıcılar için parola SSO kullanan bir galeri uygulamanın farklı değildir. Ancak, paylaşılan hesaplarını kullanmak planlama ve uygulama yapılandırma geliyordu gereken bazı ek adımlar vardır:

1. Aşağıdakileri belgelemek uygulama iş kullanıcılarıyla çalışır:
   1. Uygulama kullanan bir kuruluştaki kullanıcılara bir dizi
   1. Var olan bir dizi kimlik bilgisi kullanıcı kümesini ile ilişkili uygulama 
1. Kullanıcı kümesi ve kimlik bilgileri her birleşimi için bulutta veya şirket gereksinimlerinize göre bir güvenlik grubu oluşturun.
1. Paylaşılan kimlik bilgilerini sıfırlayın. Azure AD'de uygulama dağıtıldıktan sonra kişilerin paylaşılan hesabının parolasını gerekmez. Azure AD parola depolayacak olduğundan, çok uzun ve karmaşık olmasını ayarlamayı göz önünde bulundurun. 
1. Uygulama destekliyorsa otomatik parola geçişi yapılandırın. Bu şekilde, paylaşılan hesabının parolasını bile gerçekleştiren ilk kurulum yönetici öğrenmiş olacaksınız. 

## <a name="plan-your-authentication-method"></a>Kimlik doğrulama yönteminizi planlama

Doğru kimlik doğrulama yöntemi seçme içinde bir Azure AD karma kimlik çözümünü ayarlama önemli bir ilk karar değil. Kullanıcıların bulutta sağlayan Azure AD Connect kullanarak yapılandırılan kimlik doğrulama yöntemini uygulayın.

Bir kimlik doğrulama yöntemi seçmek için zaman, mevcut altyapı, karmaşıklığı ve maliyeti, seçtiğiniz uygulama göz önünde bulundurmanız gerekir. Bu faktörlerin her kuruluş için farklıdır ve zaman içinde değişebilir. Kendi senaryonuza en yakından eşleşen birini seçmeniz gerekir. Daha fazla bilgi için [, Azure Active Directory karma kimlik çözümü için doğru kimlik doğrulama yöntemini seçin](https://docs.microsoft.com/azure/security/azure-ad-choose-authn).

## <a name="plan-your-security-and-governance"></a>Güvenlik ve idare planlama 

Ağ duvarlar ile KCG cihazları açılımı giderek porous ve daha az etkili hale gelmiştir ve bulut uygulamalarında kimlik yeni birincil pivot güvenlik dikkat ve yatırım olmasıdır. 

### <a name="plan-access-reviews"></a>Erişim gözden geçirmeleri planlama

[Erişim gözden Geçirmeleriyle](https://docs.microsoft.com/azure/active-directory/governance/create-access-review) , kuruluşların grup üyeliğini yönetme, erişim kurumsal uygulamalar ve rol atamalarını verimli bir şekilde olanak sağlar. Kullanıcı erişimini yalnızca doğru kişilere erişim devam emin olmak için düzenli olarak gözden geçirmek planlamanız gerekir.

Erişim gözden geçirmeleri ayarlanırken planlamak için önemli konuların bazıları şunlardır:

1. İş gereksinimlerinize en uygun bir temposu erişim gözden geçirmeleri için tanımlayıcı. Aylık, yıllık veya isteğe bağlı bir alıştırma olarak bu bir haftalık bir kez kadar sık olabilir.

1. Uygulama erişim raporları gözden geçirenleri temsil eden grupları oluşturun. Uygulamayı ve hedef kullanıcılar ve kullanım örnekleri ile en bilinen proje katılımcıları, erişim gözden geçirmeleri katılımcıları olmasını sağlamak ihtiyacınız olacak

1. Erişim gözden geçirmesi tamamlama yerine artık erişmesi gereken kullanıcılar için uygulama erişim izinleri almayı içerir. Reddedilen kullanıcıların olası destek isteklerini işlemek için planlayın. Silinen bir kullanıcıyı hangi sırada kullanıcılar bir yönetici tarafından gerekirse geri yüklenebilir 30 gün boyunca Azure AD'de silinen kalır. 30 gün sonra bu kullanıcı kalıcı olarak silinir. Bu süre dolmadan önce Azure Active Directory portalı kullanarak, bir genel yönetici açıkça kalıcı olarak yakın zamanda silinen bir kullanıcıyı silebilirsiniz.

### <a name="plan-auditing"></a>Denetim planı

Azure AD sağlar [teknik ve iş öngörüleri içeren raporlar](https://azure.microsoft.com/documentation/articles/active-directory-view-access-usage-reports/). 

Hem güvenlik hem de Etkinlik raporlarını kullanılabilir. Güvenlik raporları, risk ve riskli oturum açma işlemleri için işaretlenmiş kullanıcıları göster. Etkinlik raporlarını gerçekleşen oturum açma etkinlik ve denetim izleri tüm oturumlarının sağlayarak kuruluşunuzdaki kullanıcıların davranışını anlamanıza yardımcı olur. Risk yönetme, üretkenliği artırmak ve uyumluluğu izlemek için raporları kullanabilirsiniz.

| Rapor türü | Erişim gözden geçirmesi | Güvenlik raporları | Oturum açma raporu |
|-------------|---------------|------------------|----------------|
| Gözden geçirmek için kullanın | Uygulama izinlerini ve kullanım. | Riskli olabilecek hesaplar | Kullanan uygulamalar erişiyor |
| Olası eylemler | Erişimini denetle; izinlerini iptal etme | Erişimi iptal; Güvenlik sıfırlama zorla | Erişimi iptal et |

Azure AD, çoğu denetim verilerini 30 gün boyunca tutar ve Azure Yönetim Portalı aracılığıyla ya da analiz sistemlerinizi indirmek, bir API aracılığıyla sunulur.

### <a name="consider-using-microsoft-cloud-application-security"></a>Microsoft bulut uygulama güvenliği kullanmayı düşünün

Microsoft Cloud App Security (MCAS) bir bulut erişim güvenlik aracısı (CASB) çözümüdür. Bulut uygulamaları ve Hizmetleri görünürlük sunar, tanımlamak ve logic'in mücadele etmek için Gelişmiş analiz sağlar ve verilerinizi nasıl geçerse denetlemenizi sağlar.

MCAS dağıtımı yapmanızı sağlar:

- Cloud Discovery, eşleme ve bulut ortamınızın ve kuruluşunuzun kullandığı bulut uygulamaları belirlemek için kullanın.
- Bulutunuzdaki uygulamaları tasdik etme ve Çalıştır-tasdikli
- Görünürlüğü ve İdaresi için bağlanan uygulamalar için sağlayıcı API'lerinden yararlanan, dağıtımı kolay uygulama bağlayıcıları kullanma
- Erişim ve içinde bulut uygulamalarınızda etkinlikleri üzerinde gerçek zamanlı görünürlük ve denetim için koşullu erişim uygulaması denetimi koruma kullanma
- Ayar ve sürekli olarak ince ayar yapmak, ilkeleri tanıyarak sürekli denetime sahip yardımcı olur.

Microsoft bulut uygulama güvenliği (MCAS) oturum denetimi herhangi bir tarayıcıda büyük herhangi bir platformdaki tüm işletim sistemlerinde kullanılabilir. Mobil uygulamalar ve Masaüstü uygulamaları da engellenen izin veya. Azure AD ile yerel olarak tümleştirerek, tüm SAML ile yapılandırılan uygulamalar veya Open ID Connect uygulamalarla Azure AD'de çoklu oturum açma, dahil olmak üzere desteklenebilir [uygulamaları birkaç öne çıkan](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad).

MCAS hakkında daha fazla bilgi için bkz: [Microsoft Cloud App Security genel bakış](https://docs.microsoft.com/cloud-app-security/what-is-cloud-app-security). MCAS abonelik kullanıcı tabanlı bir hizmettir. Lisans ayrıntıları inceleyebilirsiniz [MCAS lisanslama veri sayfasını](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE2NXYO).

### <a name="use-conditional-access"></a>Koşullu erişim kullanma

Koşullu erişim ile bulut uygulamalarınız için ölçüt tabanlı erişim denetimi kararları otomatikleştirebilirsiniz.

Koşullu erişim ilkeleri, ilk-faktörlü kimlik doğrulaması tamamlandıktan sonra uygulanır. Bu nedenle, koşullu erişim gibi senaryolar için ilk satırı savunma, hizmet reddi (DoS) saldırıları gibi çalışır, ancak bu olayları gelen sinyalleri erişimini belirlemek için kullanabilirsiniz amaçlanmamıştır. Oturum açma risk düzeyini. Örneğin istek ve benzeri konumunu kullanılabilir. Koşullu erişim hakkında daha fazla bilgi için bkz. [genel](https://docs.microsoft.com/azure/active-directory/conditional-access/plan-conditional-access) ve [dağıtım planı](https://docs.microsoft.com/azure/active-directory/conditional-access/plan-conditional-access).

## <a name="azure-sso-technical-requirements"></a>Azure SSO teknik gereksinimleri

Aşağıdaki bölümde, gerekli bırakmaya, uç noktaları, talep eşleme, gerekli öznitelikleri, sertifikalar ve kullanılan protokoller dahil olmak üzere uygulamanızın belirli yapılandırma gereksinimlerini ayrıntıları. Çoklu oturum AÇMAYA yapılandırmak için bu bilgilere ihtiyacınız olacak [Azure AD portalında](https://portal.azure.com/).

### <a name="authentication-mechanism-details"></a>Kimlik doğrulama mekanizması ayrıntıları

Tüm önceden tümleştirilmiş SaaS uygulamaları, Microsoft bir öğretici sağlar ve bu bilgileri gerekmez. Uygulama, uygulama marketinde yoksa / galeri, aşağıdaki veri parçasını toplamak gerekebilir:

- **Varsa uygulamanın kullandığı SSO için geçerli kimlik sağlayıcısı** - örneğin: AD FS, PingFederate, okta'yı
- **Hedef uygulama tarafından desteklenen protokolleri** - Örneğin, SAML 2.0, Openıd Connect, OAuth, form tabanlı kimlik doğrulama, WS-Federasyon, WS-Trust
- **Azure AD ile yapılandırılan Protokolü** - Örneğin, SAML 2.0 veya 1.1, Openıd Connect, OAuth, form tabanlı WS-Federasyon

### <a name="attribute-requirements"></a>Öznitelik gereksinimleri

Önceden yapılandırılmış bir dizi öznitelikleri ve Azure AD kullanıcı nesneleri ve her SaaS uygulamasının kullanıcı nesneleri arasında öznitelik eşlemelerini yoktur. Bazı uygulamalar, diğer grupları gibi nesnelerin türlerini yönetin. Uygulamanız için Azure AD'den kullanıcı öznitelikleri eşleme planlama ve [varsayılan öznitelik eşlemelerini özelleştirme](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes) göre iş gereksinimleriniz.

### <a name="certificate-requirements"></a>Sertifika gereksinimleri

Uygulama sertifikası güncel olmalıdır veya uygulamaya erişmek kullanıcıların riski yoktur. Çoğu SaaS uygulaması sertifikalarını 36 ay için uygundur. Uygulama dikey penceresinde bu sertifika süre değiştirin. Sona erme belge ve sertifika yenileme bilgilerinizi nasıl yöneteceğiniz biliyorsanız emin olun. 

Sertifikalarınızı yönetmek için iki yolu vardır. 

- **Otomatik sertifika aktarma** -Microsoft destekler [Azure AD'de imzalama anahtarı geçiş işlemi](https://docs.microsoft.com/azure/active-directory/develop/active-directory-signing-key-rollover). Bu sertifikaları yönetmek için tercih ettiğimiz olsa da, tüm ISV bu senaryoyu destekler.

- **El ile güncelleştirme** -her uygulamanın nasıl bunu tanımlananlara göre süresi, kendi sertifika sahiptir. Uygulamanın sertifikanın süresi dolmadan önce yeni bir sertifika oluşturmalı ve ISV gönderin. Bu bilgiler, Federasyon meta verileri çekilebilir. [Federasyon meta verileri burada daha fazlasını okuyun.](https://docs.microsoft.com/azure/active-directory/develop/active-directory-federation-metadata)

## <a name="implement-sso"></a>Uygulama SSO

Aşağıdaki aşamaları için planlamak ve kuruluşunuzdaki çözümünüzü dağıtmak için kullanın:

### <a name="user-configuration-for-sso"></a>SSO için Kullanıcı Yapılandırması

- **Test kullanıcılarını belirleme**

   Uygulama sahibine başvurun ve en az üç test kullanıcıları uygulama içinde oluşturulacağı isteyin. Birincil tanımlayıcı olarak kullanacağınız bilgilerin doğru doldurulur ve Azure AD'de kullanılabilir bir özniteliği ile eşleşen emin olun. Çoğu durumda bu SAML tabanlı uygulamalar için "Nameıd" eşleyecektir. JWT belirteçleri için olduğu "preferred_username."
   
   Kullanıcı Azure AD'de ya da bulut tabanlı bir kullanıcı olarak el ile oluşturabilir veya Azure AD Connect eşitleme altyapısını kullanarak şirket içi kullanıcı eşitleme. Uygulamaya gönderilen talepleri bilgilerinin eşleştiğinden emin olun.

- **SSO yapılandırma**

   Gelen [uygulamaların listesini](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)bulun ve uygulamanız için SSO'yu öğreticisini açmak ve ardından SaaS uygulamanız başarılı bir şekilde yapılandırmak için öğreticinin adımlarını izleyin.

   Uygulamanızı bulamazsanız, bkz. [özel uygulama belgeleri](https://docs.microsoft.com/azure/active-directory/application-config-sso-how-to-configure-federated-sso-non-gallery). Bu, Azure AD Galerisi'nde bulunmayan bir uygulama ekleme konusunda size yol gösterir.

   Kullanarak kurumsal uygulama için SAML belirtecinde verilen talepleri isteğe bağlı olarak kullanabileceğiniz [Microsoft'un Rehber belgeleri](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping). Bu, uygulamanın SAML yanıtını almak beklediğiniz şekilde eşlenir emin olun. Yapılandırma sırasında sorunlarla karşılaşırsanız, kılavuzumuzu kullanmak [SSO hata ayıklama tümleştirme için nasıl](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-debugging).

Özel uygulama ekleme bir Azure AD Premium P1 veya P2 lisansı özelliğidir.

### <a name="provide-sso-change-communications-to-end-users"></a>Son kullanıcılara SSO Değiştir iletişim olanağı

İletişim planınızı uygulayın. Bir değişiklik geliyor, onu ulaştığı zaman artık yapmanız gerekenler ve nasıl Yardım aranacağını bilmeniz, son kullanıcılarınız izin vererek emin olun.

### <a name="verify-end-user-scenarios-for-sso"></a>Son kullanıcı senaryolarını doğrulamak için SSO

Aşağıdaki test çalışmaları şirkete üzerinde testler için kullanabileceğiniz ve SSO yapılandırmalarınızı emin olmak için kişisel cihazlarını beklendiği gibi çalıştığını. Aşağıdaki senaryolarda, bir kullanıcı bir uygulama URL'sine gidin ve hizmet sağlayıcısı (SP tarafından başlatılan kimlik doğrulama akışı) tarafından başlatılan bir kimlik doğrulama akışı aracılığıyla varsayılır.

| Senaryo | Kullanıcının SP tarafından başlatılan kimlik doğrulama akışını beklenen sonuç |
|----------|---------------------------------------------------|
| IE sırada corpnet'teki uygulamayla oturum açın. | Tümleşik Windows kimlik doğrulaması (IWA) ile ek istem oluşur. |
| Kapalı corpnet durumdayken yeni oturum açma denemesi ile IE uygulamayla oturum açın. | AD FS sunucusu form tabanlı yazın. Kullanıcı başarıyla oturum açtığında ve MFA için tarayıcı ister. |
| Kapalı corpnet sırada geçerli bir oturum ile IE ile uygulamada oturum açma ve hiçbir zaman MFA gerçekleştirdi. | Kullanıcı istemi ilk faktörü almaz. Kullanıcı MFA için istem alır. |
| Kapalı corpnet sırada geçerli bir oturum ile IE ile uygulamada oturum açma ve MFA bu oturuma zaten gerçekleştirdi. | Kullanıcı istemi ilk faktörü almaz. Kullanıcı MFA almaz. Uygulamaya kullanıcı SSOs. |
| Kapalı corpnet sırada geçerli bir oturum ile Chrome/Firefox/Safari ile uygulamada oturum açma ve MFA bu oturuma zaten gerçekleştirdi. | Kullanıcı istemi ilk faktörü almaz. Kullanıcı MFA almaz. Uygulamaya SSO'ın kullanıcı. |
| Kapalı corpnet durumdayken yeni oturum açma denemesi ile Chrome/Firefox/Safari ile uygulamasına oturum açma için. | AD FS sunucusu form tabanlı yazın. Kullanıcı başarıyla oturum açtığında ve MFA için tarayıcı ister. |
| Chrome/geçerli bir oturum ile Kurumsal ağ içindeyken Firefox'ta uygulamayla oturum açın. | Kullanıcı istemi ilk faktörü almaz. Kullanıcı MFA almaz. Uygulamaya SSO'ın kullanıcı. |
| Uygulama yeni bir oturum açma denemesi ile uygulama mobil uygulaması ile oturum açın. | AD FS sunucusu form tabanlı yazın. Kullanıcı başarıyla oturum açtığında ve MFA için ADAL istemci ister. |
| Yetkisiz bir kullanıcı oturum açma URL'sini içeren uygulamaya oturum dener. | AD FS sunucusu form tabanlı yazın. Kullanıcı, ilk faktör ile oturum açma başarısız olur. |
| Yetkili bir kullanıcı oturum açma girişiminde ancak yanlış bir parola girer. | Kullanıcı, uygulama URL'sine gider ve hatalı kullanıcı adı/parola alır. |
| Yetkili bir kullanıcı bir e-postadaki bağlantıya tıklar ve kimliği zaten doğrulanmış. | Kullanıcı, URL tıklar ve uygulamaya ek komut istemi ile imzalanmış. |
| Yetkili bir kullanıcı bir e-postadaki bağlantıya tıklar ve henüz doğrulanmamış. | Kullanıcı, URL tıklar ve ilk faktörle kimlik doğrulaması yapmak komut istemi. |
| Uygulamaya uygulama mobil uygulama ile kullanıcı oturum yetkili (SP tarafından başlatılan) ile yeni bir oturum açma girişimi. | AD FS sunucusu form tabanlı yazın. Kullanıcı başarıyla oturum açtığında ve MFA için ADAL istemci ister. |
| Yetkisiz bir kullanıcı, uygulama oturum açma URL'si (SP tarafından başlatılan) ile oturum açmak çalışır. | AD FS sunucusu form tabanlı yazın. Kullanıcı, ilk faktör ile oturum açma başarısız olur. |
| Yetkili bir kullanıcı oturum açma girişiminde ancak yanlış bir parola girer.| Kullanıcı, uygulama URL'sine gider ve hatalı kullanıcı adı/parola alır. |
| Yetkili bir kullanıcı oturumunu kapatır ve yeniden oturum açması. | Oturum kapatma URL'si yapılandırdıysanız, kullanıcının tüm hizmetleri ve kimlik doğrulaması için istem dışı günlüğe kaydedilir. |
| Yetkili bir kullanıcı oturumunu kapatır ve yeniden oturum açması. | Oturum kapatma URL'si yapılandırılmamışsa, kullanıcı geri mevcut Azure AD tarayıcı oturumunda mevcut bir belirteç kullanarak otomatik olarak kaydedilir. |
| Yetkili bir kullanıcı bir e-postadaki bağlantıya tıklar ve kimliği zaten doğrulanmış. | Kullanıcı, URL tıklar ve uygulamaya ek komut istemi ile imzalanmış. |
| Yetkili bir kullanıcı bir e-postadaki bağlantıya tıklar ve henüz doğrulanmamış. | Kullanıcı, URL tıklar ve ilk faktörle kimlik doğrulaması yapmak komut istemi. |

## <a name="manage-sso"></a>SSO yönetme

Bu bölümde, SSO başarılı bir şekilde yönetmek için öneriler ve gereksinimleri özetlenmektedir.

### <a name="required-administrative-roles"></a>Gerekli yönetim rolleri

Her zaman Azure Active Directory'ye gerekli görev gerçekleştirmek için kullanılabilecek en az izinlere sahip bir rol kullanın. Microsoft öneriyor [kullanılabilen farklı rolleri gözden](https://docs.microsoft.com/azure/active-directory/active-directory-assign-admin-roles-azure-portal) ve bu uygulama için her kişi için ihtiyaçlarını karşılamak için doğru olanı seçin. Bazı roller geçici olarak uygulanmış ve dağıtım tamamlandıktan sonra kaldırılmış gerekebilir.

| Kişi| Roller | Azure AD rolüne (gerekliyse) |
|--------|-------|-----------------------------|
| Yardım Masası Yöneticisi | Katman 1 destek | None |
| Kimlik Yönetimi | Yapılandırma ve Azure AD sorunlarını etkisi, hata ayıklama | Genel yönetici |
| Uygulama Yöneticisi | Uygulamada, yapılandırma izinlerine sahip kullanıcılar kullanıcı kanıtlama | None |
| Altyapı yöneticileri | Sertifika aktarma sahibi | Genel yönetici |
| İş sahibi/Paydaş | Uygulamada, yapılandırma izinlerine sahip kullanıcılar kullanıcı kanıtlama | None |

Kullanmanızı öneririz [Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure) ek denetim, Denetim ve erişim gözden directory izinleri olan kullanıcılar için sağlamak için rollerinizin yönetmek için (PIM).

### <a name="sso-certificate-lifecycle-management"></a>SSO sertifika yaşam döngüsü yönetimi

Doğru rolleri tanımlar ve imzalama sertifikası Azure AD arasında yaşam döngüsü yönetimi ile görevli dağıtım listesi e-posta önemlidir ve uygulamanın çoklu oturum açma ile yapılandırılmış. Yerinde öneririz anahtar rolleri bazıları şunlardır:

- Sahibi uygulama'ndaki kullanıcı özelliklerini güncelleştirme
- Nöbet uygulama onarım desteği için sahibi
- Sertifikayla ilgili değişiklik bildirimlerinin yakından izlenen bir e-posta dağıtım listesi

Bir sertifika en uzun kullanım ömrünü üç yıldır. Azure AD arasında bir sertifika değişiklik nasıl ele alacağız üzerinde bir işlem oluşturma öneririz ve uygulamanızı. Bu, engellemek veya kesinti nedeniyle sertifikanın sona ermesinden en aza indirmek ya da sertifika geçişe zorlamanızı yardımcı olabilir.

### <a name="rollback-process"></a>Geri alma işlemi

Test çalışmalarınızı dayalı testi tamamladıktan sonra uygulamanız ile üretime geçmenin zamanı geldi. Üretim aşamasına geçmeniz için üretim kiracınızda, planlı ve test edilmiş yapılandırmaları uygulamak ve kullanıcılarınıza dağıtmadan anlamına gelir. Ancak, dağıtımınızı planlandığı gibi Git değil durumunda yapmanız gerekenler planlamak önemlidir. SSO yapılandırma dağıtımı sırasında başarısız olursa, hiçbir kesinti azaltmak ve kullanıcılara etkisini azaltmak nasıl anlamanız gerekir.

En iyi stratejinizi uygulamadaki kimlik doğrulama yöntemleri kullanılabilirliğini belirler. Her zaman tam olarak bir sorunla karşılaşırsanız, dağıtımınızı çalışır durumda özgün oturum açma yapılandırması duruma geri dönmek nasıl uygulama sahipleri için belgelere ayrıntılı sahip olun.

- **Uygulamanızı birden çok kimlik sağlayıcısını destekleyip desteklemediğini**örnek LDAP ve AD FS ve Ping silmeyin için dağıtım sırasında mevcut SSO yapılandırması. Bunun yerine, daha sonra geri geçiş yapmanız durumunda geçiş sırasında devre dışı. 

- **Uygulamanızı birden çok Idp'yi desteklemiyorsa** ancak kullanıcılara form tabanlı kimlik doğrulaması (kullanıcı adı/parola) kullanarak oturum açın, yeni SSO yapılandırma dağıtımı başarısız olursa kullanıcılar için bu yaklaşım geri dönebilir emin olun.

### <a name="access-management"></a>Erişim Yönetimi

Kaynaklara erişimi yönetirken ölçeklendirilmiş bir yaklaşım seçme öneririz. Genel yaklaşımları içerecek eşitleyerek Azure AD Connect şirket içi grupları kullanan [Azure AD'deki dinamik gruplar oluşturma üzerinde kullanıcı özniteliklerine dayalı](https://docs.microsoft.com/azure/active-directory/active-directory-groups-dynamic-membership-azure-portal), ya da oluşturma [Self Servis grupları](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management) Azure AD'de Kaynak sahibi tarafından yönetilebilir.

### <a name="monitor-security"></a>İzleme güvenliği

Biz, SaaS uygulama güvenliği farklı yönlerini gözden geçirin, normal bir temposu ayarlama önerilir ve gerekli düzeltici eylemleri gerçekleştirmek.

### <a name="troubleshooting"></a>Sorun giderme

Sorun giderme senaryoları aşağıdaki bağlantılar sunar. Bu senaryolar ve bunları gidermek için adımları içerir. destek personelinize belirli bir kılavuz oluşturmak isteyebilirsiniz.

#### <a name="consent-issues"></a>İzin sorunları

- [Onay beklenmeyen hata](https://docs.microsoft.com/azure/active-directory/manage-apps/application-sign-in-unexpected-user-consent-prompt)

- [Kullanıcı onayı hatası](https://docs.microsoft.com/azure/active-directory/manage-apps/application-sign-in-unexpected-user-consent-error)

#### <a name="sign-in-issues"></a>Oturum açma sorunları

- [Özel bir portaldan oturum açmada sorun](https://docs.microsoft.com/azure/active-directory/manage-apps/application-sign-in-other-problem-deeplink)

- [Özel bölmeden oturum açma sorunları](https://docs.microsoft.com/azure/active-directory/manage-apps/application-sign-in-other-problem-access-panel)

- [Uygulama oturum açma sayfasında hata](https://docs.microsoft.com/azure/active-directory/manage-apps/application-sign-in-problem-application-error)

- [Microsoft uygulamasına oturum sorunu](https://docs.microsoft.com/azure/active-directory/manage-apps/application-sign-in-problem-first-party-microsoft)

#### <a name="sso-issues-for-applications-listed-in-the-azure-application-gallery"></a>Azure uygulama galerisinde listelenmesini uygulamaları için SSO sorunları

- [Azure uygulama galerisinde bulunan uygulamalar için parola SSO sorun listelenen](https://docs.microsoft.com/azure/active-directory/manage-apps/application-sign-in-problem-password-sso-gallery) 

- [Azure uygulama galerisinde listelenmesini uygulamaları için Federasyon SSO ile ilgili sorun](https://docs.microsoft.com/azure/active-directory/manage-apps/application-sign-in-problem-federated-sso-gallery)   

#### <a name="sso-issues-for-applications-not-listed-in-the-azure-application-gallery"></a>Azure uygulama Galerisi'nde listelenmeyen uygulamaları için SSO sorunları

- [Azure uygulama Galerisi'nde listelenmeyen uygulamaları için SSO parola ile ilgili sorun](https://docs.microsoft.com/azure/active-directory/manage-apps/application-sign-in-problem-password-sso-non-gallery) 

- [Azure uygulama Galerisi'nde listelenmeyen uygulamaları için Federasyon SSO ile ilgili sorun](https://docs.microsoft.com/azure/active-directory/manage-apps/application-sign-in-problem-federated-sso-non-gallery)

## <a name="next-steps"></a>Sonraki adımlar

[SAML tabanlı SSO hata ayıklama](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-debugging)

[Uygulamaları için PowerShell aracılığıyla talep eşleme](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping)

[SAML belirtecinde verilen talepleri özelleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization)

[Çoklu oturum açma SAML Protokolü](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference)

[Çoklu Oturum Kapatma SAML protokolü](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-out-protocol-reference)

[Azure AD B2B](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b) (dış kullanıcılar için iş ortakları ve satıcılar gibi)

[Azure AD koşullu erişim](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)

[Azure kimlik koruması](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection)

[SSO erişimi](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

[Uygulaması SSO Öğreticisi](https://docs.microsoft.com/azure/active-directory/saas-apps/tutorial-list)
