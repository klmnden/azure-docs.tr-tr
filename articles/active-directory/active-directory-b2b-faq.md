---
title: "Azure Active Directory B2B işbirliği ile ilgili SSS | Microsoft Docs"
description: "Azure Active Directory B2B işbirliği hakkında sık sorulan soruların yanıtlarını alın."
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 05/23/2017
ms.author: sasubram
ms.openlocfilehash: 9f0ee9174c925f9338dc69bc5560255d66b30493
ms.sourcegitcommit: 094061b19b0a707eace42ae47f39d7a666364d58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2017
---
# <a name="azure-active-directory-b2b-collaboration-faqs"></a>Azure Active Directory B2B işbirliği ile ilgili SSS

Bu Azure Active Directory (Azure AD) işletmeden işletmeye (B2B) işbirliği hakkında sık sorulan sorular (SSS) düzenli olarak yeni konular içerecek şekilde güncelleştirildi.

### <a name="is-azure-ad-b2b-collaboration-available-in-the-azure-classic-portal"></a>Azure AD B2B işbirliği Azure Klasik Portalı'nda var mı?
Hayır. Azure AD B2B işbirliği özellikleri yalnızca [Azure portal](https://portal.azure.com) ve [erişim paneli](https://myapps.microsoft.com/). 

### <a name="can-we-customize-our-sign-in-page-so-it-is-more-intuitive-for-our-b2b-collaboration-guest-users"></a>Bizim B2B işbirliği Konuk kullanıcılar için daha sezgisel böylece biz oturum açma sayfamızı özelleştirebilir miyim?
Kesinlikle! Bkz: bizim [blog gönderisi bu özellik hakkında](https://blogs.technet.microsoft.com/enterprisemobility/2017/04/07/improving-the-branding-logic-of-azure-ad-login-pages/). Kuruluşunuzun oturum açma sayfasını özelleştirme hakkında daha fazla bilgi için bkz: [şirket oturum açmak için markası ve erişim paneli sayfaları ekleme](customize-branding.md).

### <a name="can-b2b-collaboration-users-access-sharepoint-online-and-onedrive"></a>B2B işbirliği kullanıcılar SharePoint Online ve OneDrive erişebilir mi?
Evet. Ancak, kişi seçici kullanarak mevcut Konuk kullanıcılar için SharePoint Online arama yeteneğidir **kapalı** varsayılan olarak. Varolan Konuk kullanıcılar için arama seçeneğini etkinleştirmek için ayarlanmış **ShowPeoplePickerSuggestionsForGuestUsers** için **üzerinde**. Bu ayar Kiracı düzeyinde veya site koleksiyonu düzeyinde açabilirsiniz. Set-SPOTenant ve Set-SPOSite cmdlet'leri kullanarak bu ayarı değiştirebilirsiniz. Bu cmdlet ile üyeleri tüm mevcut Konuk kullanıcılar dizinde arama yapabilirsiniz. Kiracı kapsamındaki değişiklikleri zaten sağlanmış SharePoint Online siteleri etkilemez.

### <a name="is-the-csv-upload-feature-still-supported"></a>CSV karşıya yükleme özellik hala desteklenmektedir?
Evet. .Csv dosyasını karşıya yükleme özelliğini kullanma hakkında daha fazla bilgi için bkz: [bu PowerShell örnek](active-directory-b2b-code-samples.md).

### <a name="how-can-i-customize-my-invitation-emails"></a>My davet e-postaları nasıl özelleştirebilir miyim?
Davet eden işlemiyle ilgili neredeyse her şeyi kullanarak özelleştirebileceğiniz [B2B davet API'leri](active-directory-b2b-api.md).

### <a name="can-an-invited-external-user-leave-the-organization-after-being-invited"></a>Davet edilen bir dış kullanıcı davet edilen sonra kuruluş bırakabilirsiniz?
Davet kuruluş yönetici kullanıcıların dizinden B2B işbirliği Konuk kullanıcı silebilir, ancak Konuk kullanıcı davet kuruluşunuz dizininizin başlarına bırakamazsınız. 

### <a name="can-guest-users-reset-their-multi-factor-authentication-method"></a>Konuk kullanıcılar kendi çok faktörlü kimlik doğrulama yöntemini sıfırlayabilir?
Evet. Konuk kullanıcılar kendi çok faktörlü kimlik doğrulama yöntemi, normal kullanıcılara aynı şekilde sıfırlayabilirsiniz.

### <a name="which-organization-is-responsible-for-multi-factor-authentication-licenses"></a>Hangi bir kuruluş için çok faktörlü kimlik doğrulaması lisans sorumlu mu?
Davet kuruluş çok faktörlü kimlik doğrulaması gerçekleştirir. Davet kuruluş kuruluş çok faktörlü kimlik doğrulaması kullanan kendi B2B kullanıcıları için yeterince lisansa sahip olduğundan emin olmanız gerekir.

### <a name="what-if-a-partner-organization-already-has-multi-factor-authentication-set-up-can-we-trust-their-multi-factor-authentication-and-not-use-our-own-multi-factor-authentication"></a>Ne zaten bir iş ortağı kuruluşta çok faktörlü kimlik doğrulamasını ayarlamak var? Biz, çok faktörlü kimlik doğrulama güven ve kullanabilirsiniz kendi çok faktörlü kimlik doğrulamasını kullanmamak?
Sonra da, belirli iş ortaklarının ayarlayacağım seçebilmeniz için bu özellik için gelecekteki bir sürüm, planlanmış, çok faktörlü kimlik doğrulaması (davet kuruluşun).

### <a name="how-can-i-use-delayed-invitations"></a>Gecikmeli davetleri nasıl kullanabilir miyim?
Bir kuruluş Davetleri Gönder B2B işbirliği kullanıcıları eklemek ve onları gerektiği gibi uygulamalara sağlamak isteyebilirsiniz. B2B İşbirliği davetini API ekleme iş akışı özelleştirmek için kullanabilirsiniz.

### <a name="can-i-make-a-guest-user-a-limited-administrator"></a>Konuk kullanıcı sınırlı bir yönetici hale getirebilir?
Kesinlikle. Daha fazla bilgi için bkz: [Konuk kullanıcılar eklemeyi](active-directory-users-assign-role-azure-portal.md).

### <a name="does-azure-ad-b2b-collaboration-allow-b2b-users-to-access-the-azure-portal"></a>Azure AD B2B işbirliği B2B kullanıcıların Azure portalı erişmesine izin veriyor mu?
Sınırlı Yöneticisi veya genel Yönetici rolüne atanmış bir kullanıcı sürece, B2B işbirliği kullanıcılar Azure portalına erişim gerektiren olmaz. Ancak, sınırlı Yöneticisi veya genel Yönetici rolüne atanan B2B işbirliği kullanıcılar portala erişebilirsiniz. Bu yönetici rollerini atanmadı bir Konuk kullanıcı portalı erişirse, ayrıca, kullanıcı deneyimi belirli bölümlerine erişmek olabilir. Konuk kullanıcı rolü dizinde bazı izinlere sahiptir.

### <a name="can-i-block-access-to-the-azure-portal-for-guest-users"></a>Azure portalına konuk kullanıcıların erişimini engelleyebilir miyim?
Evet! Bu ilkeyi yapılandırırken, üyeleri ve yöneticilerin yanlışlıkla engelleme erişim kaçınmak için dikkatli olun.
Konuk kullanıcı erişimi engellemek için [Azure portal](https://portal.azure.com), Windows Azure Klasik dağıtım modeli API'si bir koşullu erişim ilkesini kullanın:
1. Değiştirme **tüm kullanıcılar** yalnızca üyeleri içeren grup.
  ![Grup ekran değiştirme](media/active-directory-b2b-faq/modify-all-users-group.png)
2. Konuk kullanıcılar içeren dinamik bir grup oluşturun.
  ![Grup ekran oluşturma](media/active-directory-b2b-faq/group-with-guest-users.png)
3. Bir koşullu erişim ilkesi blok konuk kullanıcılara portalı erişmesini aşağıdaki videoda gösterilen şekilde ayarlayın:
  
  > [!VIDEO https://channel9.msdn.com/Blogs/Azure/b2b-block-guest-user/Player] 

### <a name="does-azure-ad-b2b-collaboration-support-multi-factor-authentication-and-consumer-email-accounts"></a>Azure AD B2B işbirliği, çok faktörlü kimlik doğrulaması ve tüketici e-posta hesaplarına destekliyor mu?
Evet. Çok faktörlü kimlik doğrulama ve tüketici e-posta hesaplarına hem de Azure AD B2B işbirliği için desteklenir.

### <a name="do-you-plan-to-support-password-reset-for-azure-ad-b2b-collaboration-users"></a>Parola sıfırlama Azure AD B2B işbirliği kullanıcılar için destek planlıyor musunuz?
Evet. Bir iş ortağı kuruluştan davet B2B kullanıcı için Self Servis parola sıfırlama (SSPR) için önemli ayrıntıları aşağıdadır:
 
* SSPR yalnızca B2B kullanıcı kimliğinin Kiracı oluşur.
* Bir Microsoft hesabı kimlik Kiracı ise, Microsoft hesabı SSPR mekanizması kullanılır.
* Kimlik Kiracı bir tam zamanında (JIT) veya "viral" Kiracı, bir parola sıfırlama e-posta gönderilir.
* Diğer kiracıları için B2B kullanıcılar için standart SSPR süreç izlenir. Kaynak bağlamında B2B kullanıcılar için üye SSPR gibi Kiracı engellendi. 

### <a name="is-password-reset-available-for-guest-users-in-a-just-in-time-jit-or-viral-tenant-who-accepted-invitations-with-a-work-or-school-email-address-but-who-didnt-have-a-pre-existing-azure-ad-account"></a>Parolayı bir tam zamanında (JIT) Konuk kullanıcılar için kullanılabilir sıfırlamak veya "viral" Kiracı kabul davetleri bir iş veya Okul e-posta adresi, ancak önceden var olan bir Azure AD hesabının sahip oldu?
Evet. JIT Kiracı parolasını sıfırlamak bir kullanıcı izin veren bir parola sıfırlama posta gönderilebilir.

### <a name="does-microsoft-dynamics-crm-provide-online-support-for-azure-ad-b2b-collaboration"></a>Microsoft Dynamics CRM online desteği için Azure AD B2B işbirliği sağlar?
Şu anda, Microsoft Dynamics CRM, Azure AD B2B işbirliği için çevrimiçi destek sağlamaz. Ancak, bu gelecekte destekleyen planlıyoruz.

### <a name="what-is-the-lifetime-of-an-initial-password-for-a-newly-created-b2b-collaboration-user"></a>Yeni oluşturulan B2B işbirliği kullanıcı için bir başlangıç parolası ömrü nedir?
Azure AD karakteri, parola gücünü ve kullanıcı hesaplarını eşit tüm Azure AD uygulama kilitleme gereksinimleri bulut hesabı sabit bir dizi vardır. Bulut kullanıcı hesaplarıdır başka bir kimlik sağlayıcısıyla gibi Federasyon olmayan hesaplar 
* Microsoft hesabı
* Facebook
* Active Directory Federasyon Hizmetleri
* Başka bir bulut Kiracı (için B2B işbirliği)

Federasyon hesaplar için parola ilkesi şirket içi kiralama ve kullanıcının Microsoft hesap ayarları uygulanan ilkeyi bağlıdır.

### <a name="an-organization-might-want-to-have-different-experiences-in-their-applications-for-tenant-users-and-guest-users-is-there-standard-guidance-for-this-is-the-presence-of-the-identity-provider-claim-the-correct-model-to-use"></a>Bir kuruluş, Kiracı ve Konuk kullanıcılar için kendi uygulamalarında farklı deneyimleri sahip olmak isteyebilirsiniz. Bu standart yönergeler var mı? Kimlik sağlayıcısı varlığını talep kullanılacak doğru modeli mi?
 Konuk kullanıcı kimlik doğrulaması için herhangi bir kimlik sağlayıcıyı kullanabilirsiniz. Daha fazla bilgi için bkz: [B2B işbirliği kullanıcının özelliklerini](active-directory-b2b-user-properties.md). Kullanım **UserType** kullanıcı deneyimi belirlemek için özellik. **UserType** talep şu anda eklenmedi belirteç. Uygulamalar, kullanıcının dizinine sorgulamak ve UserType almak için grafik API'sini kullanmanız gerekir.

### <a name="where-can-i-find-a-b2b-collaboration-community-to-share-solutions-and-to-submit-ideas"></a>B2B işbirliği topluluk çözümleri paylaşmak için ve fikirleri göndermek için nereden bulabilirim?
Biz, B2B işbirliğinin geliştirmek için geri bildirim için sürekli dinliyor. Kullanıcı paylaşmak için davet ediyoruz senaryoları, en iyi yöntemler ve Azure AD B2B işbirliği hakkında ister. Tartışma katılın [Microsoft teknik topluluk](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-B2B/bd-p/AzureAD_B2b).
 
Ayrıca fikir ve gelecekteki özellikleri için oy göndermek için davet ediyoruz [B2B işbirliği fikirleri](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-B2B-Ideas/idb-p/AzureAD_B2B_Ideas).

### <a name="can-we-send-an-invitation-that-is-automatically-redeemed-so-that-the-user-is-just-ready-to-go-or-does-the-user-always-have-to-click-through-to-the-redemption-url"></a>Böylece kullanıcı "gitmek yalnızca" hazır otomatik olarak kullanılan, bir davet gönderebiliriz? Veya kullanıcı her zaman kullanım URL'sine tıklatarak sahip mi?
Davet kuruluşunuzdaki da ortağı kuruluştaki bir üyesi olan bir kullanıcı tarafından gönderilen davetleri B2B kullanıcı tarafından kullanım gerektirmez.

İş ortağı kuruluştan bir kullanıcı davet kuruluşa katılmak için davet öneririz. [Kaynak kuruluşta Konuk davet eden rolüne bu kullanıcıyı eklemek](active-directory-b2b-add-guest-to-role.md). Bu kullanıcı oturum açma kullanıcı arabirimini, PowerShell komut dosyaları kullanarak iş ortağı kuruluştaki diğer kullanıcılar'ı davet edebilirsiniz veya API'leri. Ardından, belirli bir kuruluş B2B işbirliği kullanıcılardan kendi davetleri kullanmak için gerekmez.

### <a name="how-does-b2b-collaboration-work-when-the-invited-partner-is-using-federation-to-add-their-own-on-premises-authentication"></a>Davet edilen iş ortağı kendi şirket içi kimlik doğrulaması eklemek için Federasyon kullanırken, B2B işbirliğinin nasıl çalışır?
İş ortağı için şirket içi kimlik doğrulaması altyapısı federe bir Azure AD kiracısı varsa, şirket içi çoklu oturum açma (SSO) otomatik olarak sağlanır. İş ortağı Azure AD kiracısı yoksa, yeni kullanıcılar için bir Azure AD hesabı oluşturulur. 

### <a name="i-thought-azure-ad-b2b-didnt-accept-gmailcom-and-outlookcom-email-addresses-and-that-b2c-was-used-for-those-kinds-of-accounts"></a>I, Azure AD B2B gmail.com ve outlook.com e-posta adresleri kabul etmediğiniz ve B2C bu hesap türü için kullanılan zorlayıcı?
Biz B2B ve işletmeden müşteriye (B2C) işbirliği açısından kimlikleri desteklenen arasındaki farklar kaldırmış olursunuz. Kullanılan kimlik B2B veya B2C kullanarak arasında seçmek için iyi bir nedenle değil. İşbirliği seçeneğinizi seçme hakkında daha fazla bilgi için bkz: [karşılaştırmak B2B işbirliği ve Azure Active Directory B2C](active-directory-b2b-compare-b2c.md).

### <a name="what-applications-and-services-support-azure-b2b-guest-users"></a>Hangi uygulamaların ve hizmetlerin Azure B2B Konuk kullanıcılar destekliyor?
Tüm Azure AD ile tümleşik uygulamalar Azure B2B Konuk kullanıcılar destekler. 

### <a name="can-we-force-multi-factor-authentication-for-b2b-guest-users-if-our-partners-dont-have-multi-factor-authentication"></a>Çok faktörlü kimlik doğrulaması ortaklarımızın yoksa biz B2B Konuk kullanıcılar için çok faktörlü kimlik doğrulamasını zorlayabilir miyim?
Evet. Daha fazla bilgi için bkz: [B2B işbirliği kullanıcılar için koşullu erişim](active-directory-b2b-mfa-instructions.md).

### <a name="in-sharepoint-you-can-define-an-allow-or-deny-list-for-external-users-can-we-do-this-in-azure"></a>SharePoint'te dış kullanıcılar için bir "izin ver" veya "Reddet" listesi tanımlayabilirsiniz. Azure'da biz bunu yapabilirsiniz?
Evet. Azure AD B2B işbirliği destekler listeleri izin verme ve reddetme listelerini. 

### <a name="what-licenses-do-we-need-to-use-azure-ad-b2b"></a>Hangi lisansları Azure AD B2B kullanmak ihtiyacımız var?
Hangi Azure AD B2B kullanmak için kuruluş gereksinimlerinize lisansları hakkında daha fazla bilgi için bkz: [Kılavuzu lisans Azure Active Directory B2B işbirliği](active-directory-b2b-licensing.md).

### <a name="next-steps"></a>Sonraki adımlar

Azure AD B2B işbirliği ile ilgili diğer makalelerimize göz atın:

* [Azure AD B2B işbirliği nedir?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Yöneticileri Azure AD B2B işbirliği kullanıcıların nasıl eklenir?](active-directory-b2b-admin-add-users.md)
* [Bilgi çalışanları B2B işbirliği kullanıcıların nasıl eklenir?](active-directory-b2b-iw-add-users.md)
* [B2B işbirliği davet e-posta öğeleri](active-directory-b2b-invitation-email.md)
* [B2B işbirliği davet kullanım](active-directory-b2b-redemption-experience.md)
* [Azure AD B2B işbirliği lisanslama](active-directory-b2b-licensing.md)
* [Azure AD B2B işbirliği sorunlarını giderme](active-directory-b2b-troubleshooting.md)
* [Azure AD B2B işbirliği API ve özelleştirme](active-directory-b2b-api.md)
* [B2B işbirliği kullanıcıları için çok faktörlü kimlik doğrulaması](active-directory-b2b-mfa-instructions.md)
* [B2B işbirliği kullanıcıları davet olmadan ekleme](active-directory-b2b-add-user-without-invite.md)
* [Azure AD'de uygulama yönetimi için makale dizini](active-directory-apps-index.md)
