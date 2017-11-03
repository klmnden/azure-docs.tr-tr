---
title: "Azure Active Directory kavram playbook yapı taşlarını kanıtını | Microsoft Docs"
description: "Keşfetmek ve hızlı bir şekilde kimlik ve erişim yönetimi senaryoları uygulayan"
services: active-directory
keywords: "Azure active directory, playbook, kavram kanıtı, PT"
documentationcenter: 
author: dstefanMSFT
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: dstefan
ms.openlocfilehash: bdbdebe069b3150bed4aa26f1f6e677a66f75f32
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-active-directory-proof-of-concept-playbook-building-blocks"></a>Azure Active Directory kavram playbook kanıtını: yapı taşları

## <a name="catalog-of-roles"></a>Rollerin Kataloğu

| Rol | Açıklama | Prototip (PT) sorumluluk kanıtı |
| --- | --- | --- |
| **Kimlik mimarisi / geliştirme ekibi** | Bu genellikle çözümü tasarlarken, prototipleri uygulayan, onayları sürücüler ve son olarak kapatmak için işlemleri aktarır bir Ekiptir | Ortamlar sağlarlar ve olanları yönetilebilirlik açısından farklı senaryolar değerlendirme |
| **Şirket içi kimlik işletim ekibi** | Farklı kimliğe kaynakları şirket içi yönetir: Active Directory ormanları, LDAP dizinleri, ik sistemleri ve Federasyon kimlik sağlayıcıları. | Şirket içi erişim sağlamak PT senaryoları için gerekli kaynakları.<br/>Bunlar mümkün olduğunca az söz konusu|
| **Uygulama teknik sahipleri** | Teknik sahipleri farklı bulut uygulamaları ve Azure AD ile tümleştirme hizmetleri | SaaS uygulamaları (test etmek için örnekler) büyük olasılıkla ayrıntılarını sağlayın |
| **Azure AD genel yönetici** | Azure AD yapılandırmasını yönetir | Eşitleme hizmeti yapılandırmak için kimlik bilgilerini sağlayın. Genellikle aynı kimlik mimari PoC sırasında ekip ancak operations aşamasında ayrı|
| **Veritabanı ekibi** | Veritabanı altyapısı sahipleri | SQL ortamı (ADFS veya Azure AD Connect) erişim için belirli bir senaryoyu hazırlıklar sağlar.<br/>Bunlar mümkün olduğunca az söz konusu |
| **Ağ ekibi** | Ağ altyapısı sahipleri | Eşitleme sunucularının düzgün veri kaynaklarına erişmek ve bulut hizmetlerine (güvenlik duvarı kuralları, açılan bağlantı noktaları, IPSec kuralları vb.) ağ düzeyinde gerekli erişim sağlayın |
| **Güvenlik ekibi** | Güvenlik stratejisi tanımlar, güvenlik raporları çeşitli kaynaklardan analiz eder ve bulguları üzerinde aşağıdaki. | Değerlendirme senaryoları hedef güvenlik sağlar |

## <a name="common-prerequisites-for-all-building-blocks"></a>Tüm yapı taşlarını ortak önkoşulları

Azure AD Premium herhangi POC için gereken bazı ön koşullar aşağıda verilmiştir.

| Önkoşul | Kaynaklar |
| --- | --- |
| Geçerli bir Azure aboneliği ile tanımlanan azure AD kiracısı | [Azure Active Directory kiracısı alma](active-directory-howto-tenant.md)<br/>**Not:** zaten Azure AD Premium lisansına sahip bir ortamınız varsa, sıfır cap abonelik için https://aka.ms/accessaad giderek alabilirsiniz <br/>Hakkında daha fazla bilgi edinin: https://blogs.technet.microsoft.com/enterprisemobility/2016/02/26/azure-ad-mailbag-azure-subscriptions-and-azure-ad-2/ ve https://technet.microsoft.com/library/dn832618.aspx |
| Tanımlanan ve doğrulanan etki alanları | [Özel etki alanı adını Azure Active Directory'ye ekleme](active-directory-domains-add-azure-portal.md)<br/>**Not:** Power BI gibi bazı iş yükleri bir azure AD Kiracı kapsar altında sağlamış. Belirli bir etki alanı için bir kiracı ilişkili olup olmadığını denetlemek için https://login.microsoftonline.com/ {domain}/v2.0/.well-known/openid-configuration. gidin Etki alanı için bir kiracı zaten atanmış sonra başarılı bir yanıt almak ve devralır gerekli. Bu durumda, Ek Yardım için Microsoft ile iletişime geçin. Devralma seçenekleri hakkında daha fazla bilgi edinin: [Azure için Self Servis kaydolma nedir?](active-directory-self-service-signup.md) |
| Azure AD Premium veya EMS deneme etkin | [Azure Active Directory Premium bir ay süreyle ücretsiz](https://azure.microsoft.com/trial/get-started-active-directory/) |
| Azure AD Premium veya EMS lisanslarını PoC kullanıcılara atadığınız | [Kendinizin ve kullanıcılarınızın Azure Active Directory'de lisans](active-directory-licensing-get-started-azure-portal.md) |
| Azure AD genel yönetici kimlik bilgileri | [Azure Active Directory'de yönetici rolleri atama](active-directory-assign-admin-roles-azure-portal.md) |
| İsteğe bağlıdır ancak önerilir: bir geri dönüş olarak paralel laboratuvar ortamı | [Azure AD Connect Önkoşulları](./connect/active-directory-aadconnect-prerequisites.md) |

## <a name="directory-synchronization---password-hash-sync-phs---new-installation"></a>Dizin eşitleme - parola karması eşitlemesi (PHS) - yeni yükleme

Yaklaşık tam süresi: 1. 000'den az PoC kullanıcılar için bir saat

### <a name="pre-requisites"></a>Ön koşullar

| Önkoşul | Kaynaklar |
| --- | --- |
| Azure AD çalıştırmak için sunucuya bağlanın | [Azure AD Connect Önkoşulları](./connect/active-directory-aadconnect-prerequisites.md) |
| Aynı etki alanında ve bir güvenlik grubu ve OU parçası POC kullanıcıları hedeflemek | [Azure AD Connect özel yüklemesi](./connect/active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering) |
| Azure AD Connect POC için gerekli özellikler tanımlanır | [Azure Active Directory ile Active Directory connect - eşitleme yapılandırma özellikleri](./connect/active-directory-aadconnect.md#configure-sync-features) |
| Şirket içi kimlik bilgileri gerekli ve bulut ortamları  | [Azure AD Connect: Hesaplar ve izinler](./connect/active-directory-aadconnect-accounts-permissions.md) |

### <a name="steps"></a>Adımlar

| Adım | Kaynaklar |
| --- | --- |
| Azure AD Connect'in en son sürümünü indirme | [Microsoft Azure Active Directory Connect indirin](https://www.microsoft.com/download/details.aspx?id=47594) |
| En basit yolu Azure AD Connect'i yüklemek: Express <br/>1. Hedef eşitleme döngüsü süresini en aza indirmek için OU filtreleme<br/>2. Kullanıcılar hedef kümesi şirket içi grubunda seçin.<br/>3. Diğer POC temalar tarafından gerekli özellikleri dağıtma | [Azure AD Connect: Özel yükleme: etki alanı ve OU filtreleme](./connect/active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering) <br/>[Azure AD Connect: Özel yükleme: Grup tabanlı filtreleme](./connect/active-directory-aadconnect-get-started-custom.md#sync-filtering-based-on-groups)<br/>[Azure AD Connect: şirket içi kimliklerinizi Azure Active Directory ile tümleştirme: eşitleme özellikleri yapılandırma](./connect/active-directory-aadconnect.md#configure-sync-features) |
| Azure AD Connect kullanıcı arabirimini açın ve çalışan profilleri tamamlanmış (içeri aktarma, eşitleme ve dışa aktarma) | [Azure AD Connect eşitleme: Scheduler](./connect/active-directory-aadconnectsync-feature-scheduler.md) |
| Açık [Azure AD yönetim portalında](https://ms.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/), "Tüm kullanıcılar" dikey penceresine gidin, "Kaynak yetki başlangıcı" sütun ve kullanıcılara görünür, bkz: "Windows Server'dan AD" geliyormuş gibi doğru işaretlenmiş ekleme | [Azure AD Yönetim Portalı](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) |

### <a name="considerations"></a>Dikkat edilmesi gerekenler

1. Parola karma eşitlemesi güvenlik konuları Ara [burada](./connect/active-directory-aadconnectsync-implement-password-synchronization.md).  Pilot üretim kullanıcılar için parola karma eşitlemesi tam olarak bir seçenek değilse, aşağıdaki seçenekleri göz önünde bulundurun:
   * Test kullanıcıları üretim etki alanında oluşturun. Başka bir hesap eşitleme emin olun
   * Bir UAT ortamına taşıma
2.  Federasyon istediğiniz, maliyetleri şirket içi kimlik sağlayıcısı POC ötesinde birleştirilmiş bir çözüm ilişkili anlamak ve, aradığınız avantajları karşı ölçmek için faydalı olur:
    * Yüksek kullanılabilirlik için tasarım böylece kritik yolunda olduğundan
    * Bu kapasite planı için gereken bir şirket içi hizmet olduğundan
    * Bu İzleyici/korumak/düzeltme eki için gereken bir şirket içi hizmet olduğundan

Daha fazla bilgi edinin: [anlama Office 365 kimlik ve Azure Active Directory - Federal Kimlik](https://support.office.com/article/Understanding-Office-365-identity-and-Azure-Active-Directory-06a189e7-5ec6-4af2-94bf-a22ea225a7a9#bk_federated)

## <a name="branding"></a>Markalama

Yaklaşık tam süresi: 15 dakika

### <a name="pre-requisites"></a>Ön koşullar

| Önkoşul | Kaynaklar |
| --- | --- |
| Varlıklar (görüntüleri, logolar, vb.); En iyi görselleştirme için emin olun ve varlıkları önerilen boyutlar vardır. | [Şirket, oturum açma sayfasına Azure Active Directory'de markası ekleme](active-directory-branding-custom-signon-azure-portal.md) |
| İsteğe bağlı: ortam bir ADFS sunucusu varsa, erişim sunucusuna web teması özelleştirmek için | [AD FS kullanıcı oturum açma özelleştirme](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/ad-fs-user-sign-in-customization) |
| Son kullanıcı oturum açma deneyimi gerçekleştirmek için istemci bilgisayar |  |
| İsteğe bağlı: Mobil cihazların deneyimi doğrulamak üzere |  |

### <a name="steps"></a>Adımlar

| Adım | Kaynaklar |
| --- | --- |
| Azure AD Yönetim Portalı'na gidin | [Azure AD yönetim portalında - şirket markası](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/LoginTenantBranding) |
| Oturum açma sayfası (kahramanı logosu, küçük logo, etiketler, vb.) için varlıkların karşıya yükleyin. AD FS varsa, ADFS oturum açma sayfaları ile aynı varlıkları isteğe bağlı olarak Hizala | [Oturum açma ve erişim paneli sayfalarınıza şirket markası ekleme: özelleştirilebilir öğeler](active-directory-add-company-branding.md) |
| Birkaç dakika değişikliğin tam olarak etkili olması bekleyin |  |
| Https://myapps.microsoft.com için POC kullanıcı kimlik bilgileriyle oturum |  |
| Görünüm tarayıcıda onaylayın | [Oturum açma ve erişim paneli sayfalarınıza şirket markası ekleme](active-directory-add-company-branding.md) |
| İsteğe bağlı olarak, görünüm diğer aygıtları onaylayın |  |

### <a name="considerations"></a>Dikkat edilmesi gerekenler

Eski Görünüm özelleştirme sonrasında kalırsa tarayıcı istemci önbelleğini temizlemek ve işlemi yeniden deneyin.

## <a name="group-based-licensing"></a>Lisans grup tabanlı

Yaklaşık tam süre: 10 dakika

### <a name="pre-requisites"></a>Ön koşullar

| Önkoşul | Kaynaklar |
| --- | --- |
| Tüm POC kullanıcılar bir güvenlik grubu (Bulut veya şirket içi) bir parçasıdır | [Bir grup oluşturun ve Azure Active Directory'de üye ekleme](active-directory-groups-create-azure-portal.md) |

### <a name="steps"></a>Adımlar

| Adım | Kaynaklar |
| --- | --- |
| Azure AD yönetim portalında lisansları dikey penceresine gidin | [Azure AD yönetim portalında: lisanslama](https://portal.azure.com/#blade/Microsoft_AAD_IAM/LicensesMenuBlade/Products) |
| Lisanslar POC kullanıcılarla güvenlik grubuna atayın. | [Kullanıcıların Azure Active Directory'deki bir gruba lisansları atama](active-directory-licensing-group-assignment-azure-portal.md) |

### <a name="considerations"></a>Dikkat edilmesi gerekenler

Sorunları durumunda Git [senaryoları, sınırlamaları ve Azure Active Directory'de lisans işlemlerini yönetmek için grupları kullanma bilinen sorunlar](active-directory-licensing-group-advanced.md)

## <a name="saas-federated-sso-configuration"></a>SaaS Federasyon SSO yapılandırma

Yaklaşık tam süresi: 60 dakika

### <a name="pre-requisites"></a>Ön koşullar

| Önkoşul | Kaynaklar |
| --- | --- |
| Kullanılabilir SaaS uygulamasının test ortamı. Bu kılavuzda, örnek olarak ServiceNow kullanırız.<br/>Mevcut veri kalitesini ve eşlemeleri gezinme çubuğunda uyuşmazlık en aza indirmek için bir test örneği kullanmak için önerilir. | Https://developer.servicenow.com/app.do# gidin! / ev bir test örneği alma işlemini başlatmak için |
| ServiceNow yönetim konsoluna yönetici erişimi | [Öğretici: ServiceNow Azure Active Directory Tümleştirme](active-directory-saas-servicenow-tutorial.md) |
| Hedef uygulamaya atamak için kullanıcıları oluşturun. PoC kullanıcıları içeren bir güvenlik grubu önerilir. <br/>Grup oluşturma uygun değilse, ardından kullanıcılar için doğrudan uygulama PoC için atayın | [Bir kuruluş uygulama Azure Active Directory'de bir kullanıcı veya grup atayın](active-directory-coreapps-assign-user-azure-portal.md) |

### <a name="steps"></a>Adımlar

| Adım | Kaynaklar |
| --- | --- |
| Tüm aktörler için öğretici Microsoft Documentation gelen paylaşma  | [Öğretici: ServiceNow Azure Active Directory Tümleştirme](active-directory-saas-servicenow-tutorial.md) |
| Bir çalışma toplantı ayarlayın ve her aktör ile öğretici adımları izleyin. | [Öğretici: ServiceNow Azure Active Directory Tümleştirme](active-directory-saas-servicenow-tutorial.md) |
| Uygulama önkoşulları belirlenen grubu atayın. POC kapsamda koşullu erişimi varsa, daha sonra yeniden ziyaret ve MFA ekleyin ve benzer. <br/>Bu (yapılandırılmışsa) sağlama işleminde kazandırın unutmayın |  [Bir kuruluş uygulama Azure Active Directory'de bir kullanıcı veya grup atayın](active-directory-coreapps-assign-user-azure-portal.md) <br/>[Bir grup oluşturun ve Azure Active Directory'de üye ekleme](active-directory-groups-create-azure-portal.md) |
| Galeriden ServiceNow uygulama eklemek için Azure AD Yönetim Portalı kullanın| [Azure AD Yönetim Portalı: Kurumsal uygulamalar](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/Overview) <br/>[Azure Active Directory'de Kurumsal Uygulama Yönetimi'nde yenilikler](active-directory-enterprise-apps-whats-new-azure-portal.md) |
| "ServiceNow uygulamasının dikey"çoklu oturum açmayı"SAML tabanlı oturum açmayı" etkinleştirin |  |
| ServiceNow URL'niz ile "Oturum üzerinde URL'si" ve "Tanımlayıcı" alanları doldurun<br/>"Yeni sertifika etkin hale getirmek için" onay kutusunu işaretleyin<br/>ve ayarları kaydedin |  |
| Servicenow'ı yapılandırmak özelleştirilmiş yönergeler görüntülemek için bölmenin altındaki "Yapılandırma ServiceNow" dikey penceresini açın |  |
| Servicenow'ı yapılandırmak için yönergeleri izleyin |  |
| ServiceNow uygulama "Hazırlama" dikey penceresinde "Otomatik" sağlamayı etkinleştir | [Yeni Azure portalına kurumsal uygulamalar için sağlama kullanıcı hesabı yönetme](active-directory-enterprise-apps-manage-provisioning.md) |
| Sağlama işlemini tamamlarken birkaç dakika bekleyin.  Bu arada, sağlama raporları kontrol edebilirsiniz |  |
| Https://myapps.microsoft.com/ için erişimi olan bir sınama kullanıcısı oturum açın | [Erişim paneli nedir?](active-directory-saas-access-panel-introduction.md) |
| Yeni oluşturduğunuz uygulama kutucuğuna tıklayın. Erişimi onaylayın |  |
| İsteğe bağlı olarak, uygulama kullanım raporlarını kontrol edebilirsiniz. Bu nedenle raporlarında trafiği görmek için bir süre beklemeniz gerekir bazı gecikme olduğuna dikkat edin. | [Azure Active Directory portalında oturum açma etkinliği raporları: yönetilen uygulamaların kullanımı](active-directory-reporting-activity-sign-ins.md#usage-of-managed-applications)<br/>[Azure Active Directory rapor bekletme ilkeleri](active-directory-reporting-retention.md) |

### <a name="considerations"></a>Dikkat edilmesi gerekenler

1. Yukarıdaki [öğretici](active-directory-saas-servicenow-tutorial.md) başvuran eski Azure AD yönetim deneyimi. Ancak PoC temel [Hızlı Başlangıç](active-directory-enterprise-apps-whats-new-azure-portal.md#quick-start-get-going-with-your-new-application-right-away) karşılaşırsınız.
2. Hedef uygulama galerisinde mevcut değilse, "kendi uygulamanızı getir" kullanabilirsiniz. Daha fazla bilgi edinin: [Kurumsal Uygulama Yönetimi Azure Active Directory'deki yenilikler: tek bir yerden özel uygulamalar ekleme](active-directory-enterprise-apps-whats-new-azure-portal.md#add-custom-applications-from-one-place)

## <a name="saas-password-sso-configuration"></a>SaaS parola SSO yapılandırma

Yaklaşık tam süresi: 15 dakika

### <a name="pre-requisites"></a>Ön koşullar

| Önkoşul | Kaynaklar |
| --- | --- |
| Test ortamı SaaS uygulamaları için. Parola SSO HipChat ve Twitter örneğidir. Başka herhangi bir uygulama için tam html formu oturum açma sayfası URL'sini gerekir. | [Microsoft Azure Marketi twitter](https://azuremarketplace.microsoft.com/marketplace/apps/aad.twitter)<br/>[Microsoft Azure Market'te HipChat](https://azuremarketplace.microsoft.com/marketplace/apps/aad.hipchat) |
| Uygulamaları için hesapları sınayın. | [Twitter için kaydolun](https://twitter.com/signup?lang=en)<br/>[Ücretsiz kaydolun: HipChat](https://www.hipchat.com/sign_up) |
| Hedef uygulamaya atamak için kullanıcıları oluşturun. Bir güvenlik grubu kullanıcıları bulunan önerilir. | [Bir kuruluş uygulama Azure Active Directory'de bir kullanıcı veya grup atayın](active-directory-coreapps-assign-user-azure-portal.md) |
| Internet Explorer, Chrome veya Firefox için erişim paneli uzantısı dağıtacak bir bilgisayar için yerel yönetici erişimi | [IE için erişim paneli uzantısı](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access%20Panel%20Extension.msi)<br/>[Chrome için erişim paneli uzantısı](https://go.microsoft.com/fwLink/?LinkID=311859&clcid=0x409)<br/>[Firefox için erişim paneli uzantısı](https://go.microsoft.com/fwLink/?LinkID=626998&clcid=0x409) |

### <a name="steps"></a>Adımlar

| Adım | Kaynaklar |
| --- | --- |
| Tarayıcı uzantısı yükleyin | [IE için erişim paneli uzantısı](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access%20Panel%20Extension.msi)<br/>[Chrome için erişim paneli uzantısı](https://go.microsoft.com/fwLink/?LinkID=311859&clcid=0x409)<br/>[Firefox için erişim paneli uzantısı](https://go.microsoft.com/fwLink/?LinkID=626998&clcid=0x409) |
| Galeriden uygulamayı yapılandırma | [Azure Active Directory'de Kurumsal Uygulama Yönetimi'nde yenilikler: yeni ve geliştirilmiş uygulama Galerisi](active-directory-enterprise-apps-whats-new-azure-portal.md#improvements-to-the-azure-active-directory-application-gallery) |
| Parola SSO yapılandırın | [Yeni Azure portalında Kurumsal uygulamaları için çoklu oturum açmayı yönetme: parola tabanlı oturum açma](active-directory-enterprise-apps-manage-sso.md#password-based-sign-on) |
| Uygulama önkoşulları belirlenen grubu atayın | [Bir kuruluş uygulama Azure Active Directory'de bir kullanıcı veya grup atayın](active-directory-coreapps-assign-user-azure-portal.md) |
| Https://myapps.microsoft.com/ için erişimi olan bir sınama kullanıcısı oturum açın |  |
| Yeni oluşturduğunuz uygulama kutucuğuna tıklayın. | [Erişim paneli nedir?: parola tabanlı SSO kimlik sağlama olmadan](active-directory-saas-access-panel-introduction.md#password-based-sso-without-identity-provisioning) |
| Uygulama kimlik bilgileri sağlayın | [Erişim paneli nedir?: parola tabanlı SSO kimlik sağlama olmadan](active-directory-saas-access-panel-introduction.md#password-based-sso-without-identity-provisioning) |
| Tarayıcıyı kapatın ve oturum açma yineleyin. Bu sefer orada kullanıcı uygulamaya sorunsuz erişim görmeniz gerekir. |  |
| İsteğe bağlı olarak, uygulama kullanım raporlarını kontrol edebilirsiniz. Bu nedenle raporlarında trafiği görmek için bir süre beklemeniz gerekir bazı gecikme olduğuna dikkat edin. | [Azure Active Directory portalında oturum açma etkinliği raporları: yönetilen uygulamaların kullanımı](active-directory-reporting-activity-sign-ins.md#usage-of-managed-applications)<br/>[Azure Active Directory rapor bekletme ilkeleri](active-directory-reporting-retention.md) |

### <a name="considerations"></a>Dikkat edilmesi gerekenler

Hedef uygulama galerisinde mevcut değilse, "kendi uygulamanızı getir" kullanabilirsiniz. Daha fazla bilgi edinin: [Kurumsal Uygulama Yönetimi Azure Active Directory'deki yenilikler: tek bir yerden özel uygulamalar ekleme](active-directory-enterprise-apps-whats-new-azure-portal.md#add-custom-applications-from-one-place)

 Aşağıdaki gereksinimleri göz önünde bulundurun:
   * Uygulama bilinen oturum açma URL'si olması gerekir
   * Oturum açma sayfasında bir HTML formuna tarayıcı uzantıları otomatik olarak doldurmak bir daha fazla metin alanları ile içermelidir. En azından, kullanıcı adı ve parola içermelidir.

## <a name="saas-shared-accounts-configuration"></a>SaaS paylaşılan hesaplarını yapılandırma

Yaklaşık tam süresi: 30 dakika

### <a name="pre-requisites"></a>Ön koşullar

| Önkoşul | Kaynaklar |
| --- | --- |
| Hedef uygulama ve tam oturum açma URL'leri vaktinden listesi. Örnek olarak, Twitter kullanabilirsiniz. | [Microsoft Azure Marketi twitter](https://azuremarketplace.microsoft.com/marketplace/apps/aad.twitter)<br/>[Twitter için kaydolun](https://twitter.com/signup?lang=en) |
| Bu SaaS uygulaması için kimlik bilgisi paylaşılan. | [Azure AD kullanarak hesapları paylaşma](active-directory-sharing-accounts.md)<br/>[Azure AD parola toplama devretme Facebook, Twitter ve LinkedIn önizlemeye otomatik! -Enterprise Mobility and Security Blog] (https://blogs.technet.microsoft.com/enterprisemobility/2015/02/20/azure-ad-automated-password-roll-over-for-facebook-twitter-and-linkedin-now-in-preview/) |
| Aynı hesap erişecek en az iki takım üyeleri için kimlik bilgileri. Güvenlik grubunun bir parçası olmalıdır. | [Bir kuruluş uygulama Azure Active Directory'de bir kullanıcı veya grup atayın](active-directory-coreapps-assign-user-azure-portal.md) |
| Internet Explorer, Chrome veya Firefox için erişim paneli uzantısı dağıtacak bir bilgisayar için yerel yönetici erişimi | [IE için erişim paneli uzantısı](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access%20Panel%20Extension.msi)<br/>[Chrome için erişim paneli uzantısı](https://go.microsoft.com/fwLink/?LinkID=311859&clcid=0x409)<br/>[Firefox için erişim paneli uzantısı](https://go.microsoft.com/fwLink/?LinkID=626998&clcid=0x409) |

### <a name="steps"></a>Adımlar

| Adım | Kaynaklar |
| --- | --- |
| Tarayıcı uzantısı yükleyin | [IE için erişim paneli uzantısı](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access%20Panel%20Extension.msi)<br/>[Chrome için erişim paneli uzantısı](https://go.microsoft.com/fwLink/?LinkID=311859&clcid=0x409)<br/>[Firefox için erişim paneli uzantısı](https://go.microsoft.com/fwLink/?LinkID=626998&clcid=0x409) |
| Galeriden uygulamayı yapılandırma | [Azure Active Directory'de Kurumsal Uygulama Yönetimi'nde yenilikler: yeni ve geliştirilmiş uygulama Galerisi](active-directory-enterprise-apps-whats-new-azure-portal.md#improvements-to-the-azure-active-directory-application-gallery) |
| Parola SSO yapılandırın | [Yeni Azure portalında Kurumsal uygulamaları için çoklu oturum açmayı yönetme: parola tabanlı oturum açma](active-directory-enterprise-apps-manage-sso.md#password-based-sign-on) |
| Uygulama kimlik bilgilerini atarken önkoşulları belirlenen grubu atayın | [Bir kuruluş uygulama Azure Active Directory'de bir kullanıcı veya grup atayın](active-directory-coreapps-assign-user-azure-portal.md) |
| Bu erişim uygulama olarak farklı kullanıcı olarak oturum **aynı hesabı paylaşılan.**  |  |
| İsteğe bağlı olarak, uygulama kullanım raporlarını kontrol edebilirsiniz. Bu nedenle raporlarında trafiği görmek için bir süre beklemeniz gerekir bazı gecikme olduğuna dikkat edin. | [Azure Active Directory portalında oturum açma etkinliği raporları: yönetilen uygulamaların kullanımı](active-directory-reporting-activity-sign-ins.md#usage-of-managed-applications)<br/>[Azure Active Directory rapor bekletme ilkeleri](active-directory-reporting-retention.md) |


### <a name="considerations"></a>Dikkat edilmesi gerekenler

Hedef uygulama galerisinde mevcut değilse, "kendi uygulamanızı getir" kullanabilirsiniz. Daha fazla bilgi edinin: [Kurumsal Uygulama Yönetimi Azure Active Directory'deki yenilikler: tek bir yerden özel uygulamalar ekleme](active-directory-enterprise-apps-whats-new-azure-portal.md#add-custom-applications-from-one-place)

 Aşağıdaki gereksinimleri göz önünde bulundurun:
   * Uygulama bilinen oturum açma URL'si olması gerekir
   * Oturum açma sayfasında bir HTML formuna tarayıcı uzantıları otomatik olarak doldurmak bir daha fazla metin alanları ile içermelidir. En azından, kullanıcı adı ve parola içermelidir.

## <a name="app-proxy-configuration"></a>Uygulama Proxy Yapılandırması

Yaklaşık tam süresi: 20 dakika

### <a name="pre-requisites"></a>Ön koşullar

| Önkoşul | Kaynaklar |
| --- | --- |
| Bir Microsoft Azure AD basic veya premium aboneliği ve bir genel yönetici olan bir Azure AD dizini | [Azure Active Directory sürümleri](active-directory-editions.md) |
| Uzaktan erişim için yapılandırmak istediğiniz şirket içi bir web uygulaması barındırılan |  |
| Uygulama Ara sunucusu Bağlayıcısı'nı yükleyebilmek için Windows Server 2012 R2 veya Windows 8.1 veya sonraki bir sürümü çalıştıran bir sunucu | [Azure AD uygulama proxy'si bağlayıcılar anlama](application-proxy-understand-connectors.md) |
| Yolda bir güvenlik duvarı varsa, bağlayıcı uygulama ara sunucusuna HTTPS (TCP) istekleri yapabilen, açık olduğundan emin olun | [Azure portalında uygulama ara sunucusunu etkinleştirme: uygulama ara sunucusu önkoşulları](active-directory-application-proxy-enable.md#application-proxy-prerequisites) |
| Kuruluşunuzun internet'e bağlanmak için proxy sunucuları kullanıyorsa, Al blog göz sonrası çalışma bunların nasıl yapılandırılacağı hakkında ayrıntılı bilgi için mevcut şirket içi proxy sunucuları ile | [Mevcut şirket içi proxy sunucuları ile çalışma](application-proxy-working-with-proxy-servers.md) |


### <a name="steps"></a>Adımlar

| Adım | Kaynaklar |
| --- | --- |
| Bir bağlayıcı sunucuya yükleyin | [Azure portalında uygulama ara sunucusunu etkinleştirme: yükleme ve kaydetme bağlayıcı](active-directory-application-proxy-enable.md#install-and-register-a-connector) |
| Azure AD uygulama proxy'si uygulama olarak, şirket içi uygulama yayımlama | [Azure AD uygulama proxy'si ile uygulama yayımlama](application-proxy-publish-azure-portal.md) |
| Test kullanıcıları atayın | [Azure AD uygulama proxy'si ile uygulama yayımlama: bir test kullanıcısı Ekle](application-proxy-publish-azure-portal.md#add-a-test-user) |
| İsteğe bağlı olarak, çoklu oturum açma deneyimini kullanıcılarınız için yapılandırın | [Çoklu oturum açma ile Azure AD uygulama proxy'si sağlayın](application-proxy-sso-azure-portal.md) |
| MyApps portal atanan kullanıcı olarak oturum açarak uygulamayı test etme | https://myapps.microsoft.com |

### <a name="considerations"></a>Dikkat edilmesi gerekenler

1. Bağlayıcıyı şirket ağınızda koyma öneririz, ancak alırken durumlarda bulutta yerleştirme daha iyi performans görürsünüz. Daha fazla bilgi edinin: [Azure Active Directory Uygulama proxy'si kullanırken ağ topolojisi hakkında önemli noktalar](application-proxy-network-topology-considerations.md)
2. Daha fazla güvenlik ayrıntıları ve nasıl bu özellikle güvenli uzaktan erişim sağlar için yalnızca giden bağlantılar koruma tarafından çözüm bakın: [Azure AD uygulama proxy'si kullanarak uygulamalar uzaktan erişim için güvenlik konuları](application-proxy-security-considerations.md)

## <a name="generic-ldap-connector-configuration"></a>Genel LDAP Bağlayıcısı yapılandırması

Yaklaşık tam süresi: 60 dakika

> [!IMPORTANT]
> Bu, FIM/MIM aşina gerektiren gelişmiş bir yapılandırmadır. Üretimde Biz bu yapılandırma hakkında sorular öneri kullandıysanız geçtikleri [Premier Destek](https://support.microsoft.com/premier).

### <a name="pre-requisites"></a>Ön koşullar

| Önkoşul | Kaynaklar |
| --- | --- |
| Azure AD Connect yüklenir ve yapılandırılır. | Yapı Taşı: [dizin eşitleme - parola karması eşitlemesi](#directory-synchronization--password-hash-sync-phs--new-installation) |
| ADLDS örneği toplantı gereksinimleri | [Genel LDAP Bağlayıcısı teknik başvuru: Genel LDAP Bağlayıcısı'nı genel bakış](./connect/active-directory-aadconnectsync-connector-genericldap.md#overview-of-the-generic-ldap-connector) |
| Kullanıcıların kullanmadığınız iş yükleri ve bu iş yükleri ile ilişkili öznitelikleri listesi | [Azure AD Connect eşitleme: öznitelikleri eşitlenmiş Azure Active Directory'ye](./connect/active-directory-aadconnectsync-attributes-synchronized.md) |


### <a name="steps"></a>Adımlar

| Adım | Kaynaklar |
| --- | --- |
| Genel LDAP Bağlayıcısı Ekle | [Genel LDAP Bağlayıcısı teknik başvuru: yeni bir bağlayıcı oluşturun](./connect/active-directory-aadconnectsync-connector-genericldap.md#create-a-new-connector) |
| (Tam içeri aktarma, delta içeri aktarma, tam eşitleme, delta eşitleme, dışa aktarma) oluşturulan Bağlayıcısı için çalıştırma profillerini oluşturma | [Bir yönetim aracı çalıştırma profili oluştur](https://technet.microsoft.com/library/jj590219(v=ws.10).aspx)<br/> [Azure AD Connect Eşitleme Hizmeti Yöneticisi ile bağlayıcıları kullanma](./connect/active-directory-aadconnectsync-service-manager-ui-connectors.md)|
| Tam içeri aktarma profili çalıştırın ve bağlayıcı alanı nesne olduğunu doğrulayın | [Bağlayıcı alanı nesne arayın](https://technet.microsoft.com/library/jj590287(v=ws.10).aspx)<br/>[Azure AD Connect Eşitleme Hizmeti Yöneticisi ile bağlayıcıları kullanma: arama bağlayıcı alanı](./connect/active-directory-aadconnectsync-service-manager-ui-connectors.md#search-connector-space) |
| Böylece meta veri deposu nesneleri iş yükleri için gerekli öznitelikler eşitleme kuralları oluşturma | [Azure AD Connect eşitleme: en iyi uygulamalar varsayılan yapılandırmasını değiştirmek için: eşitleme kurallarına değiştirir](./connect/active-directory-aadconnectsync-best-practices-changing-default-configuration.md#changes-to-synchronization-rules)<br/>[Azure AD Connect eşitleme: bildirim temelli hazırlama anlama](./connect/active-directory-aadconnectsync-understanding-declarative-provisioning.md)<br/>[Azure AD Connect eşitleme: bildirim temelli hazırlama ifadeleri anlama](./connect/active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md) |
| Tam eşitleme döngüsü Başlat | [Azure AD Connect eşitleme: Zamanlayıcı: Zamanlayıcı'yı başlatma](./connect/active-directory-aadconnectsync-feature-scheduler.md#start-the-scheduler) |
| Sorun giderme hataları durumunda yapın | [Azure AD ile eşitlenmeyen bir nesneyle ilgili sorunları giderme](./connect/active-directory-aadconnectsync-troubleshoot-object-not-syncing.md) |
| LDAP kullanıcı oturum açma ve uygulamaya erişim olduğunu doğrulayın | https://myapps.microsoft.com |

### <a name="considerations"></a>Dikkat edilmesi gerekenler

> [!IMPORTANT]
> Bu, FIM/MIM aşina gerektiren gelişmiş bir yapılandırmadır. Üretimde Biz bu yapılandırma hakkında sorular öneri kullandıysanız geçtikleri [Premier Destek](https://support.microsoft.com/premier).

## <a name="groups---delegated-ownership"></a>Grupları - temsilci sahipliği

Yaklaşık tam süre: 10 dakika

### <a name="pre-requisites"></a>Ön koşullar

| Önkoşul | Kaynaklar |
| --- | --- |
| SaaS uygulaması (Federasyon SSO veya parola SSO) zaten yapılandırıldı | Yapı Taşı: [SaaS Federasyon SSO yapılandırma](#saas-federated-sso-configuration) |
| #1 uygulamada erişimin atandığı bulut Grup tanımlanır | Yapı Taşı: [SaaS Federasyon SSO yapılandırma](#saas-federated-sso-configuration) <br/>[Bir grup oluşturun ve Azure Active Directory'de üye ekleme](active-directory-groups-create-azure-portal.md) |
| Grup sahibi için kimlik bilgilerini kullanılabilir | [Azure Active Directory grupları ile kaynaklara erişimi yönetme](active-directory-manage-groups.md) |
| Uygulamalara erişen bilgi çalışanı kimlik bilgileri tanımlanan | [Erişim paneli nedir?](active-directory-saas-access-panel-introduction.md) |


### <a name="steps"></a>Adımlar

| Adım | Kaynaklar |
| --- | --- |
| Uygulamaya erişim izni grubunu tanımlayın ve verilen sahibi yapılandırma grubu| [Azure Active Directory'deki bir gruba ayarlarını yönetme](active-directory-groups-settings-azure-portal.md) |
| Grup sahibi olarak oturum açın, bkz: Grup üyeliğini grupları sekmesinde erişim paneli | [Azure Active Directory grupları Management sayfası](https://account.activedirectory.windowsazure.com/r/#/groups) |
| Test etmek istediğiniz bilgi çalışanı Ekle |  |
| Bilgi çalışanı olarak oturum açın, döşeme kullanılabilir onaylayın | [Erişim paneli nedir?](active-directory-saas-access-panel-introduction.md) |

### <a name="considerations"></a>Dikkat edilmesi gerekenler

Uygulama etkin sağlama varsa, bilgi çalışanı olarak uygulamadan erişmeden önce tamamlamak için sağlama için birkaç dakika beklemeniz gerekebilir.

## <a name="saas-and-identity-lifecycle"></a>SaaS ve kimlik yaşam döngüsü

### <a name="pre-requisites"></a>Ön koşullar

| Önkoşul | Kaynaklar |
| --- | --- |
| SaaS uygulaması (Federasyon SSO veya parola SSO) zaten yapılandırıldı | Yapı Taşı: [SaaS Federasyon SSO yapılandırma](#saas-federated-sso-configuration) |
| #1 uygulamada erişimin atandığı bulut Grup tanımlanır | Yapı Taşı: [SaaS Federasyon SSO yapılandırma](#saas-federated-sso-configuration) <br/>[Bir grup oluşturun ve Azure Active Directory'de üye ekleme](active-directory-groups-create-azure-portal.md) |
| Uygulamalara erişen bilgi çalışanı kimlik bilgileri tanımlanan | [Erişim paneli nedir?](active-directory-saas-access-panel-introduction.md) |


### <a name="steps"></a>Adımlar

| Adım | Kaynaklar |
| --- | --- |
| Kullanıcı uygulamayı atandığı grubundan kaldırın | [Azure Active Directory kiracınızdaki kullanıcıların Grup üyeliğini yönetme](active-directory-groups-members-azure-portal.md) |
| Sağlamayı kaldırma özelliklerini için birkaç dakika bekleyin | [SaaS, kullanıcı uygulama sağlama Azure AD'de otomatik: otomatik sağlama işi ne yapar?](active-directory-saas-app-provisioning.md#how-does-automatic-provisioning-work) |
| Ayrı bir tarayıcı oturumunda uygulamaları portalımı için bilgi çalışanı olarak oturum açın ve bu kutucuğu eksik onaylayın | http://myapps.microsoft.com |


### <a name="considerations"></a>Dikkat edilmesi gerekenler

Leavers PoC senaryoya ve/veya Mazeret senaryoları tahmin. Kullanıcı devre dışı, şirket içi AD veya kaldırılmış artık SaaS uygulamasına oturum açmak için bir yolu yoktur.

## <a name="self-service-access-to-application-management"></a>Self Servis uygulama yönetimine erişimi

Yaklaşık tam süre: 10 dakika

### <a name="pre-requisites"></a>Ön koşullar

| Önkoşul | Kaynaklar |
| --- | --- |
| Uygulamalara erişimi güvenlik grubunun bir parçası olarak isteyeceğini POC kullanıcıları tanımlayın | Yapı Taşı: [SaaS Federasyon SSO yapılandırma](#saas-federated-sso-configuration) |
| Dağıtılan hedef uygulama | Yapı Taşı: [SaaS Federasyon SSO yapılandırma](#saas-federated-sso-configuration) |

### <a name="steps"></a>Adımlar

| Adım | Kaynaklar |
| --- | --- |
| Azure AD yönetim portalında kurumsal uygulamalar dikey penceresine gidin | [Azure AD yönetim portalında: Kurumsal uygulamalar](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/AllApps/menuId/) |
| Ön koşullar uygulamadan kendini hizmeti ile yapılandırma | [Azure Active Directory'de Kurumsal Uygulama Yönetimi'nde yenilikler: Self Servis uygulama erişimi yapılandırma](active-directory-enterprise-apps-whats-new-azure-portal.md#configure-self-service-application-access) |
| Uygulamaları portalımı için bilgi çalışanı olarak oturum açın | http://myapps.microsoft.com |
| Fark "+ uygulama Ekle" op sayfasının düğmesini. Uygulamaya erişmek için kullanın |  |

### <a name="considerations"></a>Dikkat edilmesi gerekenler

Seçilen uygulamaların gereksinimlerini sağlama olabilir, bu nedenle hemen uygulamaya giderek bazı hatalara neden olabilir. Seçilen uygulama azure ad ile sağlama destekliyorsa ve yapılandırıldığından, bu bir fırsat olarak uçtan uca çalışan tüm akışını göstermek için kullanabilirsiniz. Yapı bloğu için bkz: [SaaS Federasyon SSO yapılandırma](#saas-federated-sso-configuration) daha fazla önerileri için

## <a name="self-service-password-reset"></a>Self Servis parola sıfırlama

Yaklaşık tam süresi: 15 dakika

### <a name="pre-requisites"></a>Ön koşullar

| Önkoşul | Kaynaklar |
| --- | --- |
| Kiracınızda Self Servis parola yönetimini etkinleştirin. | [Azure Active Directory parola BT yöneticileri için sıfırlama](active-directory-passwords.md) |
| Parola şirket içi parolaları yönetmek geri yazma etkinleştirin. Bunu gerektiren belirli Azure AD Not sürümleri Bağlan | [Parola Geri Yazma önkoşulları](active-directory-passwords-writeback.md) |
| Bu işlevselliği kullanmak ve bir güvenlik grubunun üyesi olduğundan emin olun PoC kullanıcıları belirleyin. Kullanıcıların özelliği tam olarak göstermek için yönetici olmayanlar olması gerekir | [Özelleştirme: Azure AD parola yönetimi: parola sıfırlama için erişimi kısıtla](active-directory-passwords-writeback.md) |


### <a name="steps"></a>Adımlar

| Adım | Kaynaklar |
| --- | --- |
| Azure AD Yönetim Portalı'na gidin: parola sıfırlama | [Azure AD yönetim portalında: Parola sıfırlama](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/PasswordReset) |
| Parola sıfırlama İlkesi belirler. POC amacıyla, telefon araması ve soru- cevap kullanabilirsiniz Oturum açma erişim paneli gerekli kaydını etkinleştirmek için önerilir. |  |
| Oturumu kapatın ve bir bilgi çalışanı oturum açın |  |
| 2. adım yapılandırıldığı gibi Self Servis parola sıfırlama verileri kaynağı | http://aka.MS/ssprsetup |
| Tarayıcıyı kapatın |  |
| 4. adımda kullanılan bilgi çalışanı olarak oturum açma işlemini Başlat |  |
| Parola sıfırlama | [Kendi parolanızı güncelleştirin: parolamı sıfırla](active-directory-passwords-update-your-own-password.md) |
| Yeni parolanızı şirket içindeki kaynaklar için de Azure ad oturum açma deneyin |  |

### <a name="considerations"></a>Dikkat edilmesi gerekenler

1. Azure AD Connect yükseltme uyuşmazlık neden olacaksa, ardından karşı bulut hesapları kullanmayı düşünün veya ayrı bir ortamda karşı demo olun
2. Yöneticiler başka bir ilke olması ve parolayı sıfırlamak için yönetici hesabı kullanarak PoC taint ve karışıklığa yol. Sıfırlama işlemleri test etmek için normal bir kullanıcı hesabı kullandığınızdan emin olun


## <a name="azure-multi-factor-authentication-with-phone-calls"></a>Azure multi-Factor Authentication telefon çağrısı ile

Yaklaşık tam süre: 10 dakika

### <a name="pre-requisites"></a>Ön koşullar

| Önkoşul | Kaynaklar |
| --- | --- |
| MFA kullanacağı POC kullanıcıları tanımlayın  |  |
| MFA testini iyi alımını ile telefon  | [Azure Multi-Factor Authentication nedir?](../multi-factor-authentication/multi-factor-authentication.md) |

### <a name="steps"></a>Adımlar

| Adım | Kaynaklar |
| --- | --- |
| Azure AD Yönetim Portalı'nda "Kullanıcılar ve Gruplar" dikey penceresine gidin | [Azure AD yönetim portalında: Kullanıcılar ve gruplar](https://portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/Overview/menuId/) |
| "Tüm kullanıcılar" dikey seçin |  |
| Üst Seç "Çok faktörlü kimlik doğrulaması" düğme çubuğu | URL için Azure MFA portalı doğrudan: https://aka.ms/mfaportal |
| "Kullanıcı" ayarlarında PoC kullanıcıları seçin ve MFA için etkinleştirme | [Azure Multi-Factor Authentication’da Kullanıcı Durumları](../multi-factor-authentication/multi-factor-authentication-get-started-user-states.md) |
| PoC kullanıcı ve güçlü işlemiyle ilerlemesi olarak oturum açın  |  |

### <a name="considerations"></a>Dikkat edilmesi gerekenler

1. Bu yapı taşı açıkça MFA bir kullanıcı için tüm oturum ayarlama PoC adımları. MFA hakkında daha fazla bilgi devreye koşullu erişim ve kimlik koruması gibi diğer araçları vardır hedeflenen senaryoları. Bu POC üretime taşırken dikkate alınacak olacaktır.
2. Bu yapı taşı PoC adımlarda expedience MFA yöntemi olarak telefon aramaları açıkça kullanıyor. Üretime POC geçiş gibi uygulamalar gibi kullanmanızı öneririz [Microsoft Authenticator](../multi-factor-authentication/end-user/microsoft-authenticator-app-how-to.md) , mümkün olduğunda ikinci öğe olarak.
Daha fazla bilgi edinin: [taslak NIST özel yayını 800-63B](https://pages.nist.gov/800-63-3/sp800-63b.html)

## <a name="mfa-conditional-access-for-saas-applications"></a>SaaS uygulamaları için MFA koşullu erişim

Yaklaşık tam süre: 10 dakika

### <a name="pre-requisites"></a>Ön koşullar

| Önkoşul | Kaynaklar |
| --- | --- |
| İlke hedeflemek için PoC kullanıcıları belirleyin. Bu kullanıcılar koşullu erişim ilkesi kapsam için bir güvenlik grubu olmalıdır | [SaaS Federasyon SSO yapılandırma](#saas-federated-sso-configuration) |
| SaaS uygulamasına zaten yapılandırıldı |  |
| PoC kullanıcıların uygulamayı zaten atanmış |  |
| POC kullanıcı kimlik bilgilerini kullanılabilir |  |
| POC kullanıcı MFA için kayıtlı. Bir telefon ile iyi almayı kullanma | http://aka.MS/ssprsetup |
| İç ağ aygıtı. İç adres aralığındaki yapılandırılmış IP adresi | IP adresi Bul: https://www.bing.com/search?q=what%27s+my+ip |
| (Taşıyıcı ağını kullanarak bir telefon olabilir) dış ağ aygıtı |  |

### <a name="steps"></a>Adımlar

| Adım | Kaynaklar |
| --- | --- |
| Azure AD yönetim portalında gidin: koşullu erişim dikey penceresi | [Azure AD yönetim portalında: Koşullu erişim](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/Policies) |
| Koşullu erişim ilkesi oluşturun:<br/>-Hedef PoC kullanıcılar "Kullanıcılar ve Gruplar" altında<br/>-"Bulut uygulamalarını" altında hedef PoC uygulama<br/>-Güvenilen olanları "Koşullar" altında "Konumları" -> dışında tüm konumların hedef **Not:** güvenilen IP'leri yapılandırılmış [MFA portalı](https://account.activedirectory.windowsazure.com/UserManagement/MfaSettings.aspx)<br/>-Çok faktörlü kimlik doğrulaması "Verme" altında gerektir | [Azure Active Directory'de koşullu erişimi kullanmaya başlama: İlke yapılandırma adımları](active-directory-conditional-access-azure-portal-get-started.md#policy-configuration-steps) |
| Erişim kurumsal ağ içinde uygulama | [Azure Active Directory'de koşullu erişimi kullanmaya başlama: ilkeyi test etme](active-directory-conditional-access-azure-portal-get-started.md#testing-the-policy) |
| Ortak ağ erişim uygulamadan | [Azure Active Directory'de koşullu erişimi kullanmaya başlama: ilkeyi test etme](active-directory-conditional-access-azure-portal-get-started.md#testing-the-policy) |

### <a name="considerations"></a>Dikkat edilmesi gerekenler

Federasyon kullanıyorsanız, talep iç/dış şirket ağı durumuyla iletişim kurmak için şirket içi kimlik sağlayıcıyı (IDP) kullanabilirsiniz. Değerlendirmek ve büyük kuruluşlarda yönetmek için karmaşık olabilecek IP adreslerinin listesi yönetmek zorunda kalmadan bu tekniği kullanabilirsiniz. Bu kurulum, "Ağ gezici" senaryo (iç ağdan ve oturum açmış anahtarları sırasında bir kafe gibi konumları günlüğü bir kullanıcı) için hesap ve etkilerini anladığınızdan emin olun. Daha fazla bilgi edinin: [Azure multi-Factor Authentication ve AD FS ile bulut kaynakları güvenliğini sağlama: güvenilen IP'leri Federasyon kullanıcıları için](../multi-factor-authentication/multi-factor-authentication-get-started-adfs-cloud.md#trusted-ips-for-federated-users)

## <a name="privileged-identity-management-pim"></a>Privileged Identity Management (PIM)

Yaklaşık tam süresi: 15 dakika

### <a name="pre-requisites"></a>Ön koşullar

| Önkoşul | Kaynaklar |
| --- | --- |
| PIM POC parçası olacak genel yönetici tanımlayın | [Azure AD Privileged Identity Management kullanmaya başlayın](active-directory-privileged-identity-management-getting-started.md) |
| Güvenlik Yöneticisi olacak genel yönetici tanımlayın | [Azure AD Privileged Identity Management kullanmaya başlayın](active-directory-privileged-identity-management-getting-started.md)<br/> [Azure Active Directory PIM farklı yönetim rolleri](active-directory-privileged-identity-management-roles.md) |
| İsteğe bağlı: Genel yönetici e-posta bildirimleri PIM çalışmak için e-posta erişimi olup olmadığını onaylayın | [Azure AD Privileged Identity Management nedir?: rol etkinleştirme ayarlarını yapılandırma](active-directory-privileged-identity-management-configure.md#configure-the-role-activation-settings)


### <a name="steps"></a>Adımlar

| Adım | Kaynaklar |
| --- | --- |
| Https://portal.azure.com genel yönetici (GA) ve önyükleme PIM dikey olarak oturum açın. Bu adımı gerçekleştirir genel yönetici Güvenlik Yöneticisi sağlanmış.  Şimdi bu aktör GA1 çağırın | [Azure AD Privileged Identity Management Güvenlik Sihirbazı'nı kullanma](active-directory-privileged-identity-management-security-wizard.md) |
| Genel yönetici tanımlayın ve bunları kalıcı için uygun taşıyın. Bu, 1. adımda daha anlaşılır olması için kullanılan bir ayrı bir yönetici olmanız gerekir. Şimdi bu aktör GA2 çağırın | [Azure AD Privileged Identity Management: nasıl'ın bir kullanıcı rolü ekleme veya kaldırma](active-directory-privileged-identity-management-how-to-add-role-to-user.md)<br/>[Azure AD Privileged Identity Management nedir?: rol etkinleştirme ayarlarını yapılandırma](active-directory-privileged-identity-management-configure.md#configure-the-role-activation-settings)  |
| Şimdi, https://portal.azure.com için GA2 oturum açın ve "Kullanıcı ayarları" değiştirmeyi deneyin. Bazı seçenekler gri unutmayın. | |
| Yeni bir sekme ve aynı oturum 3. adım olarak, artık https://portal.azure.com için gidin ve PIM dikey için Pano ekleyin. | [Etkinleştirmek veya Azure AD Privileged Identity Management rollerinde devre dışı bırakma: Privileged Identity Management uygulamasını ekleme](active-directory-privileged-identity-management-how-to-activate-role.md#add-the-privileged-identity-management-application) |
| Genel yönetici rolüne etkinleştirme isteği | [Etkinleştirmek veya Azure AD Privileged Identity Management rollerinde devre dışı bırakma: rol etkinleştirme](active-directory-privileged-identity-management-how-to-activate-role.md#activate-a-role) |
| GA2 hiçbir zaman MFA için kaydolduysanız, kaydı Azure MFA için gerekli olduğunu unutmayın |  |
| Adım 3'te özgün sekmesine geri dönün ve tarayıcıda Yenile düğmesini tıklatın. Şimdi "Kullanıcı ayarları" değiştirmek için erişim gerektiğini unutmayın | |
| İsteğe bağlı olarak, genel Yöneticiler e-posta etkinleştirilmiş varsa, bilgisayarınızda GA1 ve GA2'ın gelen kutusunu kontrol edin ve etkinleştirilmekte olan rol bildirim görebilirsiniz |  |
| 8 denetim geçmişi denetleyin ve raporun GA2 ayrıcalıkların onaylamak için gösterilen gözlemleyin. | [Azure AD Privileged Identity Management nedir?: rol etkinliği gözden geçirin](active-directory-privileged-identity-management-configure.md#review-role-activity) |

### <a name="considerations"></a>Dikkat edilmesi gerekenler

Bu özellik Azure AD Premium P2 ve/veya EMS E5 parçasıdır

## <a name="discovering-risk-events"></a>Risk olaylarını keşfetme

Yaklaşık tam süresi: 20 dakika

### <a name="pre-requisites"></a>Ön koşullar

| Önkoşul | Kaynaklar |
| --- | --- |
| Tor tarayıcı aygıtla indirilir ve yüklenir | [Tor tarayıcı yükleyin](https://www.torproject.org/projects/torbrowser.html.en#downloads) |
| Oturum açma yapmak için POC kullanıcıya erişim | [Azure Active Directory kimlik koruması Kılavuzu](active-directory-identityprotection-playbook.md) |

### <a name="steps"></a>Adımlar

| Adım | Kaynaklar |
| --- | --- |
| Açık tor tarayıcı | [Tor tarayıcı yükleyin](https://www.torproject.org/projects/torbrowser.html.en#downloads) |
| Https://myapps.microsoft.com POC kullanıcı hesabıyla oturum açın | [Azure Active Directory kimlik koruması Kılavuzu: Risk olaylarını benzetimini yapma](active-directory-identityprotection-playbook.md#simulating-risk-events) |
| 5-7 dakika bekleyin |  |
| Https://portal.azure.com için genel yönetici olarak oturum açın ve kimlik koruması dikey penceresini açın | https://aka.MS/aadipgetstarted |
| Risk olaylar dikey penceresini açın. "Oturum açma işlemleri anonim IP adreslerinden" altında bir girdi görmeniz gerekir  | [Azure Active Directory kimlik koruması Kılavuzu: Risk olaylarını benzetimini yapma](active-directory-identityprotection-playbook.md#simulating-risk-events) |

### <a name="considerations"></a>Dikkat edilmesi gerekenler

Bu özellik Azure AD Premium P2 ve/veya EMS E5 parçasıdır

## <a name="deploying-sign-in-risk-policies"></a>Oturum açma riski ilkelerini dağıtma

Yaklaşık tam süre: 10 dakika

### <a name="pre-requisites"></a>Ön koşullar

| Önkoşul | Kaynaklar |
| --- | --- |
| Tor tarayıcı aygıtla indirilir ve yüklenir | [Tor tarayıcı yükleyin](https://www.torproject.org/projects/torbrowser.html.en#downloads) |
| Erişim Günlüğü'nde test yapmak için POC kullanıcı olarak |  |
| POC kullanıcı MFA ile kaydedilir. Bir telefon ile iyi alma kullandığınızdan emin olun | Yapı Taşı: [Azure çok faktörlü kimlik doğrulama telefon aramaları](#azure-multi-factor-authentication-with-phone-calls) |


### <a name="steps"></a>Adımlar

| Adım | Kaynaklar |
| --- | --- |
| Https://portal.azure.com için genel yönetici olarak oturum açın ve kimlik koruması dikey penceresini açın | https://aka.MS/aadipgetstarted |
| Oturum açma risk ilkesine aşağıdaki gibi etkinleştirin:<br/>-Atanan: POC kullanıcı<br/>-Koşullar: Oturum açma riski Orta veya yüksek (oturum açma anonim konumdan kabul Orta risk düzeyi)<br/>-Denetimleri: MFA gerektirir | [Azure Active Directory kimlik koruması Kılavuzu: oturum açma riski](active-directory-identityprotection-playbook.md#sign-in-risk) |
| Açık tor tarayıcı | [Tor tarayıcı yükleyin](https://www.torproject.org/projects/torbrowser.html.en#downloads) |
| Https://myapps.microsoft.com PoC kullanıcı hesabıyla oturum açın |  |
| MFA testini dikkat edin | [Azure AD kimlik koruması ile karşılaştığında oturum açma: oturum açma riskli kurtarma](active-directory-identityprotection-flows.md#risky-sign-in-recovery)

### <a name="considerations"></a>Dikkat edilmesi gerekenler

Bu özellik Azure AD Premium P2 ve/veya EMS E5 parçasıdır. Risk olaylar hakkında daha fazla ziyaret öğrenin: [Azure Active Directory risk olayları](active-directory-reporting-risk-events.md)

## <a name="configuring-certificate-based-authentication"></a>Sertifika tabanlı kimlik doğrulamasını yapılandırma

Yaklaşık tamamlanma süresi: 20 dakika

### <a name="pre-requisites"></a>Ön koşullar

| Önkoşul | Kaynaklar |
| --- | --- |
| Sağlanan kullanıcı sertifikadan (Windows, iOS veya Android) Kuruluş PKI aygıtla | [Kullanıcı sertifikalarını dağıtma](https://msdn.microsoft.com/library/cc770857.aspx) |
| Azure AD etki alanı ADFS ile Federasyon | [Azure AD Connect ve federasyon](./connect/active-directory-aadconnectfed-whatis.md)<br/>[Active Directory Sertifika Hizmetleri'ne Genel Bakış](https://technet.microsoft.com/library/hh831740.aspx)|
| İOS cihazları için Microsoft Authenticator uygulamasının yüklü olması | [Microsoft Authenticator uygulaması ile çalışmaya başlama](../multi-factor-authentication/end-user/microsoft-authenticator-app-how-to.md) |

### <a name="steps"></a>Adımlar

| Adım | Kaynaklar |
| --- | --- |
| "Sertifika kimlik doğrulamasını" ADFS etkinleştirin | [Kimlik doğrulama ilkelerini yapılandırmasını: Windows Server 2012 R2'de genel birincil kimlik doğrulaması yapılandırmak için](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/configure-authentication-policies#to-configure-primary-authentication-globally-in-windows-server-2012-r2) |
| İsteğe bağlı: Sertifika kimlik doğrulaması Azure AD'de Exchange Active Sync istemciler için etkinleştirme | [Azure Active Directory'de sertifika tabanlı kimlik doğrulaması kullanmaya başlama](active-directory-certificate-based-authentication-get-started.md) |
| Erişim Masası'na gidin ve kullanıcı sertifikası kullanılarak kimlik doğrulaması | https://myapps.microsoft.com |

### <a name="considerations"></a>Dikkat edilmesi gerekenler

Bu dağıtımın uyarılar hakkında daha fazla ziyaret öğrenin: [ADFS: sertifika kimlik doğrulaması Azure AD ile & Office 365](https://blogs.msdn.microsoft.com/samueld/2016/07/19/adfs-certauth-aad-o365/)



> [!NOTE]
> Kullanıcı sertifikası elinde korunmalıdır. Aygıtları yönetme tarafından ya da akıllı kartlar durumunda PIN ile.



[!INCLUDE [active-directory-playbook-toc](../../includes/active-directory-playbook-steps.md)]
