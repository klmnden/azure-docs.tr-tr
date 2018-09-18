---
title: Azure Active Directory B2B işbirliği ile ilgili SSS | Microsoft Docs
description: Azure Active Directory B2B işbirliği hakkında sık sorulan soruların yanıtlarını alın.
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: reference
ms.date: 05/11/2018
ms.author: mimart
author: msmimart
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: b4407ac6b7a0d9fdbf52b84fb94223c32868f0c5
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "45983861"
---
# <a name="azure-active-directory-b2b-collaboration-faqs"></a>Azure Active Directory B2B işbirliği hakkında SSS

Bu Azure Active Directory (Azure AD) işletmeler arası (B2B) işbirliği hakkında sık sorulan sorular (SSS) düzenli olarak yeni konular içerecek şekilde güncelleştirildi.

### <a name="can-we-customize-our-sign-in-page-so-it-is-more-intuitive-for-our-b2b-collaboration-guest-users"></a>Bizim B2B işbirliği Konuk kullanıcıları için daha sezgisel, bu nedenle biz bizim oturum açma sayfasını özelleştirebilir miyim?
Kesinlikle! Bkz. bizim [bu özellik hakkındaki blog gönderisini](https://blogs.technet.microsoft.com/enterprisemobility/2017/04/07/improving-the-branding-logic-of-azure-ad-login-pages/). Kuruluşunuzun oturum açma sayfasını özelleştirme hakkında daha fazla bilgi için bkz. [şirket markası oturum açmanız ve erişim paneli sayfalarına ekleme](../fundamentals/customize-branding.md).

### <a name="can-b2b-collaboration-users-access-sharepoint-online-and-onedrive"></a>B2B işbirliği kullanıcıları, SharePoint Online ve OneDrive erişebilir miyim?
Evet. Ancak becerisidir Kişi Seçici'yi kullanarak mevcut Konuk kullanıcıları, SharePoint Online'da aramak için **kapalı** varsayılan olarak. Var olan konuk kullanıcılar için arama yapma seçeneğini etkinleştirmek için ayarlanmış **ShowPeoplePickerSuggestionsForGuestUsers** için **üzerinde**. Kiracı düzeyinde veya site koleksiyonu düzeyinde bu ayarı etkinleştirebilirsiniz. Bu ayar Set-SPOTenant ve Set-SPOSite cmdlet'lerini kullanarak değiştirebilirsiniz. Bu cmdlet'ler ile üyeleri tüm mevcut Konuk kullanıcıları dizinde arama yapabilirsiniz. Değişiklikler Kiracı kapsamında zaten sağlanmış SharePoint Online siteleri etkilemez.

### <a name="is-the-csv-upload-feature-still-supported"></a>CSV karşıya yükleme özelliği hala destekleniyor mu?
Evet. .Csv dosyasını karşıya yükleme özelliğini kullanma hakkında daha fazla bilgi için bkz. [bu PowerShell örneği](code-samples.md).

### <a name="how-can-i-customize-my-invitation-emails"></a>Davet e-postaları nasıl özelleştirebilirim?
Kullanarak davet eden süreci hakkında neredeyse her şeyi özelleştirebilirsiniz [B2B davet API'leri](customize-invitation-api.md).

### <a name="can-guest-users-reset-their-multi-factor-authentication-method"></a>Konuk kullanıcılar, çok faktörlü kimlik doğrulama yöntemleri sıfırlayabilir?
Evet. Konuk kullanıcıları çok faktörlü kimlik doğrulaması yöntemi, normal kullanıcılarla aynı şekilde sıfırlayabilirsiniz.

### <a name="which-organization-is-responsible-for-multi-factor-authentication-licenses"></a>Hangi bir kuruluş için çok faktörlü kimlik doğrulaması lisans sorumlu mu?
Davet eden kuruluştan multi-Factor authentication gerçekleştirir. Davet eden kuruluştan kuruluş çok faktörlü kimlik doğrulaması kullanan B2B kullanıcıları için yeterince lisansa sahip olduğundan emin olmanız gerekir.

### <a name="what-if-a-partner-organization-already-has-multi-factor-authentication-set-up-can-we-trust-their-multi-factor-authentication-and-not-use-our-own-multi-factor-authentication"></a>Ne zaten bir iş ortağı kuruluşta çok faktörlü kimlik doğrulaması ayarlama var? Biz kullanıcıdan çok faktörlü kimlik doğrulamasını güven ve kendi çok faktörlü kimlik doğrulaması kullanmayacak?
Dışlanacak belirli iş ortakları, sonra da, seçebilmeniz için bu özelliği gelecekteki sürümlerde sunulması planlanmaktadır, (davet eden kuruluşun) çok faktörlü kimlik doğrulaması.

### <a name="how-can-i-use-delayed-invitations"></a>Gecikmeli davetleri nasıl kullanabilirim?
Bir kuruluş ardından Davetleri Gönder B2B işbirliği kullanıcıları ekleyin ve onları gerektiği gibi uygulamalara sağlamak isteyebilirsiniz. B2B işbirliği davet API ekleme iş akışı özelleştirmek için kullanabilirsiniz.

### <a name="can-i-make-guest-users-visible-in-the-exchange-global-address-list"></a>Konuk kullanıcılar Exchange Genel adres listesinde görünür yapmak?
Evet. Varsayılan olarak, Konuk nesneleri kuruluşunuzun genel adres listesinde görünür değildir, ancak onları görünür yapmak için Azure Active Directory PowerShell kullanabilirsiniz. Ayrıntılar için bkz **Konuk nesneler genel adres listesinde görünür yapabilirsiniz?** içinde [Konuk erişimi Office 365 gruplarında](https://support.office.com/article/guest-access-in-office-365-groups-bfc7a840-868f-4fd6-a390-f347bf51aff6#PickTab=FAQ).

### <a name="can-i-make-a-guest-user-a-limited-administrator"></a>Konuk kullanıcı sınırlı yönetici duruma getirebilirsiniz?
Kesinlikle. Daha fazla bilgi için [bir role Konuk kullanıcı ekleme](add-guest-to-role.md).

### <a name="does-azure-ad-b2b-collaboration-allow-b2b-users-to-access-the-azure-portal"></a>Azure AD B2B işbirliği, B2B kullanıcıları, Azure portalına erişmek izin veriyor mu?
B2B işbirliği kullanıcıları, sınırlı yönetici veya genel Yönetici rolüne atanmış bir kullanıcı yoksa, Azure portalına erişim gerekmez. Ancak, sınırlı yönetici veya genel Yönetici rolüne atanan B2B işbirliği kullanıcıları portala erişebilirsiniz. Ayrıca, bu yönetici rollerinden biri atanmış bir Konuk kullanıcı portalı erişirse, kullanıcı deneyimi bazı kısımlarını erişmek mümkün olabilir. Konuk kullanıcı rolü dizinde bazı izinlere sahiptir.

### <a name="can-i-block-access-to-the-azure-portal-for-guest-users"></a>Ben Azure portalında konuk kullanıcıların erişimini engelleyebilir miyim?
Evet! Bu ilkeyi yapılandırırken, üyeleri ve Yöneticiler için erişimi yanlışlıkla engelleyen kaçınmak dikkatli olun.
Konuk kullanıcı erişimini engellemek için [Azure portalında](https://portal.azure.com), Windows Azure Klasik dağıtım modeli API'SİNDE bir koşullu erişim ilkesi kullanın:
1. Değiştirme **tüm kullanıcılar** yalnızca üyeleri içeren grup.
  ![değiştirme grubu ekran görüntüsü](media/faq/modify-all-users-group.png)
2. Konuk kullanıcıları içeren dinamik bir grup oluşturun.
  ![Grup ekran oluşturma](media/faq/group-with-guest-users.png)
3. Bir koşullu erişim ilkesi için konuk kullanıcıları engelle portal erişimini aşağıdaki videoda gösterildiği gibi ayarlayın:
  
  > [!VIDEO https://channel9.msdn.com/Blogs/Azure/b2b-block-guest-user/Player] 

### <a name="does-azure-ad-b2b-collaboration-support-multi-factor-authentication-and-consumer-email-accounts"></a>Azure AD B2B işbirliği, çok faktörlü kimlik doğrulaması ve tüketici e-posta hesaplarını destekliyor mu?
Evet. Çok faktörlü kimlik doğrulaması ve tüketici e-posta hesapları hem de Azure AD B2B işbirliği için desteklenir.

### <a name="do-you-plan-to-support-password-reset-for-azure-ad-b2b-collaboration-users"></a>Azure AD B2B işbirliği kullanıcılar için parola sıfırlamayı desteklemeyi planlıyor musunuz?
Evet. Bir iş ortağı kuruluştan davet B2B kullanıcısı için Self Servis parola sıfırlama (SSPR) için önemli ayrıntılar aşağıda verilmiştir:
 
* SSPR yalnızca B2B kullanıcısının kimliği kiracısı gerçekleşir.
* Bir Microsoft hesabı kimliği kiracısı ise Microsoft hesabı SSPR mekanizması kullanılır.
* Bir tam zamanında (JIT) veya "viral" Kiracı kimliği kiracısı, parola sıfırlama e-posta gönderilir.
* Diğer kiracılar için standart SSPR işlem B2B kullanıcıları için izlenir. Kaynak bağlamında B2B kullanıcıları için üye SSPR gibi kiralama engellenir. 

### <a name="is-password-reset-available-for-guest-users-in-a-just-in-time-jit-or-viral-tenant-who-accepted-invitations-with-a-work-or-school-email-address-but-who-didnt-have-a-pre-existing-azure-ad-account"></a>Parola bir tam zamanında (JIT) Konuk kullanıcılar için kullanılabilir sıfırlama veya "viral" Kiracı kimin kabul davetleri bir iş veya Okul e-posta adresi, ancak kullanan önceden var olan bir Azure AD hesabı gerekmedi?
Evet. JIT kiralama ile kullanıcının parolasını sıfırlamak bir kullanıcı izin veren bir parola sıfırlama e-posta gönderilebilir.

### <a name="does-microsoft-dynamics-365-provide-online-support-for-azure-ad-b2b-collaboration"></a>Microsoft Dynamics 365, Azure AD B2B işbirliği için çevrimiçi destek sağlar mı?
Evet, Dynamics 365 (çevrimiçi), Azure AD B2B işbirliği için destek sağlar. Daha fazla bilgi için Dynamics 365 bkz [ile Azure AD B2B işbirliği kullanıcıları davet](https://docs.microsoft.com/dynamics365/customer-engagement/admin/invite-users-azure-active-directory-b2b-collaboration).

### <a name="what-is-the-lifetime-of-an-initial-password-for-a-newly-created-b2b-collaboration-user"></a>Yeni oluşturulan B2B işbirliği kullanıcısı için bir başlangıç parolası ömrünü nedir?
Azure AD, sabit bir dizi karakter, parola gücünü ve eşit olarak tüm Azure AD için uygulama kilitleme gereksinimleri bulut kullanıcı hesapları hesabı sahiptir. Bulut kullanıcı hesapları olan başka bir kimlik sağlayıcısı ile gibi Federasyon olmayan hesaplar 
* Microsoft hesabı
* Facebook
* Active Directory Federasyon Hizmetleri
* Başka bir bulut kiracısı (için B2B işbirliği)

Federasyon hesaplar için parola ilkesi şirket içi kiralama ve kullanıcının Microsoft hesabı ayarları uygulanan ilkeyi bağlıdır.

### <a name="an-organization-might-want-to-have-different-experiences-in-their-applications-for-tenant-users-and-guest-users-is-there-standard-guidance-for-this-is-the-presence-of-the-identity-provider-claim-the-correct-model-to-use"></a>Bir kuruluş, Kiracı ve Konuk kullanıcılar için uygulamalarında farklı deneyimler sahip olmak isteyebilirsiniz. Bu standart yönergeler var mı? Kimlik sağlayıcısının varlığı talep kullanılacak doğru modeli mi?
 Konuk kullanıcı, kimlik doğrulaması için herhangi bir kimlik sağlayıcısına kullanabilirsiniz. Daha fazla bilgi için [B2B işbirliği kullanıcısı özelliklerini](user-properties.md). Kullanım **UserType** kullanıcı deneyimi belirlemek için özellik. **UserType** talep şu anda dahil değildir belirteç. Uygulamaları, kullanıcının dizini sorgulamak ve UserType almak için Graph API'si kullanmanız gerekir.

### <a name="where-can-i-find-a-b2b-collaboration-community-to-share-solutions-and-to-submit-ideas"></a>B2B işbirliği topluluk çözümleri paylaşın ve fikirleri göndermek için nerede bulabilirim?
B2B işbirliği geliştirmek için geri bildirim için sürekli dinliyoruz. Kullanıcı paylaşmak için davet ediyoruz senaryoları, en iyi uygulamalar ve Azure AD B2B işbirliği hakkında beğendiğiniz özellikler. Tartışmaya katılın [Microsoft Tech Community](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-B2B/bd-p/AzureAD_B2b).
 
Ayrıca, fikirlerinizi ve gelecek özellikleri için oy göndermek için davet ediyoruz [B2B işbirliği fikirleri](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-B2B-Ideas/idb-p/AzureAD_B2B_Ideas).

### <a name="can-we-send-an-invitation-that-is-automatically-redeemed-so-that-the-user-is-just-ready-to-go-or-does-the-user-always-have-to-click-through-to-the-redemption-url"></a>Kullanıcı "hemen kullanıma hazır" yani otomatik olarak kullanıldıktan, davetiye gönderebiliriz? Veya kullanıcı her zaman tıklatarak alma URL'sine sahip?
Kullanıcı arabirimini, PowerShell betiklerini kullanarak, davet eden bir iş ortağı kuruluştaki diğer kullanıcılar davet edebilir veya API'leri. Ardından, davet eden konuk kullanıcıyla paylaşılan bir uygulamada bir doğrudan bağlantı gönderebilirsiniz. Çoğu durumda, artık davet e-postasını açın ve kullanım URL'yi gerek yoktur. Daha fazla bilgi için [Azure Active Directory B2B işbirliği Davetiyesi kullanımı](redemption-experience.md).

### <a name="how-does-b2b-collaboration-work-when-the-invited-partner-is-using-federation-to-add-their-own-on-premises-authentication"></a>Davet edilen iş ortağı kendi şirket içi kimlik doğrulaması eklemek için Federasyon kullanırken, B2B işbirliği nasıl çalışır?
İş ortağı için şirket içi kimlik altyapınızı federe bir Azure AD kiracınız varsa, şirket içi çoklu oturum açma (SSO) otomatik olarak sağlanır. İş ortağı, Azure AD kiracısı yoksa, yeni kullanıcılar için bir Azure AD hesabı oluşturulur. 

### <a name="i-thought-azure-ad-b2b-didnt-accept-gmailcom-and-outlookcom-email-addresses-and-that-b2c-was-used-for-those-kinds-of-accounts"></a>Azure AD B2B, gmail.com ve outlook.com e-posta adresleri kabul etmedi ve B2C bu hesap türü için kullanılan düşündüm?
B2B ve iş işletmeden müşteriye (B2C) işbirliği açısından kimlikleri desteklenen arasındaki farklar kaldırıyoruz. Kullanılan kimlik B2B kullanarak veya B2C kullanma arasında seçim için geçerli bir nedeniniz değil. İşbirliği seçeneğinizi seçme hakkında daha fazla bilgi için bkz: [karşılaştırma B2B işbirliği ve Azure Active Directory B2C](compare-with-b2c.md).

### <a name="what-applications-and-services-support-azure-b2b-guest-users"></a>Hangi uygulamaların ve hizmetlerin Azure B2B Konuk kullanıcıları destekliyor?
Azure B2B Konuk kullanıcı tüm Azure AD tümleşik uygulamalarını destekler. 

### <a name="can-we-force-multi-factor-authentication-for-b2b-guest-users-if-our-partners-dont-have-multi-factor-authentication"></a>İş Ortaklarımızın çok faktörlü kimlik doğrulaması yoksa, biz B2B Konuk kullanıcılar için çok faktörlü kimlik doğrulamasını zorlayabilir miyim?
Evet. Daha fazla bilgi için [B2B işbirliği kullanıcıları için koşullu erişim](conditional-access.md).

### <a name="in-sharepoint-you-can-define-an-allow-or-deny-list-for-external-users-can-we-do-this-in-azure"></a>SharePoint'te bir dış kullanıcılar için "izin ver" veya "Reddet" listesi tanımlayabilirsiniz. Azure'da bu yapabiliriz?
Evet. Azure AD B2B işbirliği destekler listeleri izin ve reddetme listelerini. 

### <a name="what-licenses-do-we-need-to-use-azure-ad-b2b"></a>Hangi lisansları, Azure AD B2B kullanmak biz gerekir mi?
Hangi Azure AD B2B kullanılacak kuruluş gereksinimlerinize lisansları hakkında daha fazla bilgi için bkz: [Azure Active Directory B2B işbirliği lisanslama Kılavuzu](licensing-guidance.md).

### <a name="next-steps"></a>Sonraki adımlar

- [Azure AD B2B işbirliği nedir?](what-is-b2b.md)

