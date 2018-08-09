---
title: Azure Active Directory kavram kavramı playbook yapı taşları | Microsoft Docs
description: Keşfetmeye ve hızlıca kimlik ve erişim yönetimi senaryoları uygulama
services: active-directory
keywords: Azure active directory, playbook, kavram kanıtı, PT
documentationcenter: ''
author: dstefanMSFT
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: dstefan
ms.openlocfilehash: d093aa6119b5ab316e5ffba77806e10cd067b032
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39626820"
---
# <a name="azure-active-directory-proof-of-concept-playbook-building-blocks"></a>Azure Active Directory kavram playbook prova: yapı taşları

## <a name="catalog-of-roles"></a>Rollerin Kataloğu

| Rol | Açıklama | Kavram kanıtı (PoC) kavramı sorumluluk |
| --- | --- | --- |
| **Kimlik mimarisi / geliştirme ekibi** | Bu genellikle çözüm tasarlar, prototipleri uygular, onayları sürücüleri ve son olarak devre dışı işlemler için uygulamalı bir Ekiptir | Ortamlar sağlarlar ve yönetilebilirlik açısından farklı senaryolar olanları değerlendirme |
| **Şirket içi kimlik operasyon ekibinin** | Farklı kimlik kaynakları şirket içi yönetir: Active Directory ormanları, LDAP dizinleri, ik sistemleri ve Federasyon kimlik sağlayıcıları. | Şirket içi erişmeyi PoC senaryolar için gerekli kaynakları.<br/>Bunlar mümkün olduğunca az söz konusu|
| **Teknik uygulama sahipleri** | Farklı bulut uygulamaları ve Hizmetleri Azure AD ile tümleştirilebilen teknik sahipleri | SaaS uygulamaları (test etmek için örnekler) olası ayrıntılarını sağlayın |
| **Azure AD genel Yöneticisi** | Azure AD yapılandırmasının yönetir | Eşitleme hizmeti yapılandırmak için kimlik bilgilerini sağlayın. Genellikle aynı kimlik mimarisi PoC sırasında ekip ancak işlem aşamasında ayrı|
| **Veritabanı ekip** | Veritabanı altyapısı sahipleri | SQL ortamı (ADFS veya Azure AD Connect) erişim için belirli bir senaryoyu hazırlıklar sağlar.<br/>Bunlar mümkün olduğunca az söz konusu |
| **Ağ ekibi** | Ağ altyapısı sahipleri | Eşitleme sunucularının düzgün bir şekilde veri kaynaklarına erişmek ve bulut Hizmetleri (güvenlik duvarı kuralları, açılan bağlantı noktaları, IPSec kuralları vb.) ağ düzeyinde gerekli erişim sağlayın |
| **Güvenlik ekibi** | Güvenlik stratejisi tanımlar, çeşitli kaynaklardan güvenlik raporları analiz eder ve bulguları üzerinde aşağıdaki. | Hedef güvenlik değerlendirme senaryoları sağlar. |

## <a name="common-prerequisites-for-all-building-blocks"></a>Tüm yapı taşlarını için genel Önkoşullar

Azure AD Premium ile istediğiniz POC için gereken bazı ön koşullar aşağıda verilmiştir.

| Önkoşul | Kaynaklar |
| --- | --- |
| Geçerli bir Azure aboneliği ile tanımlanan azure AD kiracısı | [Bir Azure Active Directory kiracısı edinme](develop/quickstart-create-new-tenant.md)<br/>**Not:** Azure AD Premium lisansına sahip bir ortam zaten varsa, giderek sınırı sıfır olan bir abonelik edinebilirsiniz https://aka.ms/accessaad <br/>Daha fazla bilgi edinin: https://blogs.technet.microsoft.com/enterprisemobility/2016/02/26/azure-ad-mailbag-azure-subscriptions-and-azure-ad-2/ ve https://technet.microsoft.com/library/dn832618.aspx |
| Tanımlanan ve doğrulanmış etki alanları | [Azure Active Directory'ye özel etki alanı adı ekleme](active-directory-domains-add-azure-portal.md)<br/>**Not:** Power BI gibi bazı iş yükleri bir azure AD kiracısı altında kapsar sağlamış. Belirli bir etki alanı için bir kiracı ile ilişkili olup olmadığını denetlemek için gidin https://login.microsoftonline.com/{domain}/v2.0/.well-known/openid-configuration. Etki alanı zaten bir kiracıya atanan sonra başarılı bir yanıt almak ve konuşturabilirsiniz çözümüyse gerekli. Bu durumda, Ek Yardım için Microsoft ile iletişime geçin. Devralma seçenekleri hakkında daha fazla bilgi: [Azure için Self Servis kaydolma nedir?](users-groups-roles/directory-self-service-signup.md) |
| Azure AD Premium veya EMS deneme etkin | [Azure Active Directory Premium bir ay boyunca ücretsiz](https://azure.microsoft.com/trial/get-started-active-directory/) |
| Azure AD Premium veya EMS lisanslarınız PoC kullanıcılara atadığınız | [Kendiniz ve kullanıcılarınızın Azure Active Directory lisansı](active-directory-licensing-get-started-azure-portal.md) |
| Azure AD genel yönetici kimlik bilgileri | [Azure Active Directory’de yönetici rolü atama](users-groups-roles/directory-assign-admin-roles.md) |
| İsteğe bağlı ancak önerilir: bir geri dönüş olarak paralel bir laboratuvar ortamı | [Azure AD Connect Önkoşulları](./connect/active-directory-aadconnect-prerequisites.md) |

## <a name="directory-synchronization---password-hash-sync-phs---new-installation"></a>Dizin eşitleme - parola karma eşitlemesi (PHS) - yeni yükleme

Yaklaşık tamamlama süresi: 1. 000'den az PoC kullanıcılar için bir saat

### <a name="pre-requisites"></a>Ön koşullar

| Önkoşul | Kaynaklar |
| --- | --- |
| Azure AD çalıştıran sunucuya bağlanma | [Azure AD Connect Önkoşulları](./connect/active-directory-aadconnect-prerequisites.md) |
| POC kullanıcılar, aynı etki alanında ve bir güvenlik grubu ve OU parçası hedef | [Azure AD Connect özel yüklemesi](./connect/active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering) |
| Azure AD Connect POC için gereken özellikleri tanımlanır. | [Azure Active Directory ile Active Directory connect - eşitleme yapılandırma özellikleri](./connect/active-directory-aadconnect.md#configure-sync-features) |
| Şirket içi kimlik bilgileri gerekli ve bulut ortamları  | [Azure AD Connect: Hesaplar ve izinler](./connect/active-directory-aadconnect-accounts-permissions.md) |

### <a name="steps"></a>Adımlar

| Adım | Kaynaklar |
| --- | --- |
| Azure AD Connect'in en son sürümünü indirin | [Microsoft Azure Active Directory Connect'i indirin](https://www.microsoft.com/download/details.aspx?id=47594) |
| En basit yol ile Azure AD Connect'i yükleme: Express <br/>1. Hedef eşitleme döngü süresini en aza indirmek için OU filtreleme<br/>2. Hedef kullanıcı kümesine şirket içi grubunda seçin.<br/>3. Diğer POC Temalar gerekli özellikleri dağıtma | [Azure AD Connect: Özel yükleme: etki alanı ve OU filtreleme](./connect/active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering) <br/>[Azure AD Connect: Özel yükleme: Grup tabanlı filtreleme](./connect/active-directory-aadconnect-get-started-custom.md#sync-filtering-based-on-groups)<br/>[Azure AD Connect: şirket içi kimliklerinizi Azure Active Directory ile tümleştirme: eşitleme özelliklerini yapılandırma](./connect/active-directory-aadconnect.md#configure-sync-features) |
| Azure AD Connect kullanıcı arabirimini açar ve çalıştırma profilleri tamamlanmış (içeri aktarma, eşitleme ve dışarı aktarma) | [Azure AD Connect eşitleme: Scheduler](./connect/active-directory-aadconnectsync-feature-scheduler.md) |
| Açık [Azure AD yönetim portalında](https://ms.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/), "Tüm kullanıcılar" dikey penceresine gidin, "Windows Server'dan AD" geliyormuş gibi düzgün bir şekilde işaretlenmiş kullanıcılar görüntülenir, görmek ve "Yetki kaynağı" Sütun Ekle | [Azure AD Yönetim Portalı](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) |

### <a name="considerations"></a>Dikkat edilmesi gerekenler

1. Parola karma eşitlemesi sırasında güvenlik konuları arayın [burada](./connect/active-directory-aadconnectsync-implement-password-hash-synchronization.md).  Ardından pilot üretim kullanıcılar için parola karması eşitlemesi kesin bir şekilde bir seçenek değilse, aşağıdaki seçenekleri göz önünde bulundurun:
   * Test kullanıcıları üretim etki alanında oluşturun. Başka bir hesap eşitleme olduğundan emin olun
   * UAT ortama taşıma
2.  Daha sonra amacınızın Federasyon istiyorsanız, maliyetleri POC ötesinde şirket içi kimlik sağlayıcısı ile bir Federasyon çözümü ilişkili anlamak ve aradığınız avantajları karşı ölçüye faydalı olur:
    * Yüksek kullanılabilirlik için tasarlama zorunda olduğu kritik yolu
    * Bir şirket içi hizmeti kapasite planlaması için ihtiyacınız olan
    * Bir şirket içi hizmeti İzleyici/korumak/düzeltme eki için ihtiyacınız olan

Daha fazla bilgi edinin: [anlama Office 365 kimlik ve Azure Active Directory - Federal Kimlik](https://support.office.com/article/Understanding-Office-365-identity-and-Azure-Active-Directory-06a189e7-5ec6-4af2-94bf-a22ea225a7a9#bk_federated)

## <a name="branding"></a>Markalama

Yaklaşık tamamlama süresi: 15 dakika

### <a name="pre-requisites"></a>Ön koşullar

| Önkoşul | Kaynaklar |
| --- | --- |
| Varlıkları (görüntüler, logolar, vb.); En iyi görselleştirme için emin olun, önerilen boyut varlıkları vardır. | [Şirket markası, Azure Active Directory oturum açma sayfasına ekleme](active-directory-branding-custom-signon-azure-portal.md) |
| İsteğe bağlı: ortamı bir ADFS sunucusu varsa, erişim sunucusuna web temasını özelleştirmek için | [Oturum açma AD FS kullanıcı özelleştirme](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/ad-fs-user-sign-in-customization) |
| Son kullanıcı oturum açma deneyimi gerçekleştirmek üzere istemci bilgisayara |  |
| İsteğe bağlı: mobil cihazları doğrulamak deneyimi |  |

### <a name="steps"></a>Adımlar

| Adım | Kaynaklar |
| --- | --- |
| Azure AD yönetim portalına gidin | [Azure AD yönetim portalında - şirket markası](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/LoginTenantBranding) |
| Varlıkları için oturum açma sayfasını (hero logosu, küçük logo, etiketler, vb.) karşıya yükleyin. AD FS varsa, AD FS oturum açma sayfaları ile aynı varlıkları isteğe bağlı olarak Hizala | [Oturum açma ve erişim paneli sayfalarınıza şirket markası ekleme: özelleştirilebilir öğeler](fundamentals/customize-branding.md) |
| Birkaç dakika değişikliğin tam olarak etkili olması bekleyin |  |
| POC kullanıcı kimlik bilgisi ile oturum https://myapps.microsoft.com |  |
| Görünümü tarayıcıda onaylayın | [Oturum açma ve erişim paneli sayfalarınıza şirket markası ekleme](fundamentals/customize-branding.md) |
| İsteğe bağlı olarak, görünümü diğer aygıtları onaylayın |  |

### <a name="considerations"></a>Dikkat edilmesi gerekenler

Eski görünümü özelleştirme sonrasında kalırsa tarayıcı istemci önbelleği temizleyin ve işlemi yeniden deneyin.

## <a name="group-based-licensing"></a>Grup tabanlı lisanslama

Yaklaşık tamamlama süresi: 10 dakika

### <a name="pre-requisites"></a>Ön koşullar

| Önkoşul | Kaynaklar |
| --- | --- |
| Tüm POC kullanıcılar bir güvenlik grubu (Bulut veya şirket içi) bir parçasıdır | [Bir grup oluşturun ve Azure Active Directory'de üye ekleme](fundamentals/active-directory-groups-create-azure-portal.md) |

### <a name="steps"></a>Adımlar

| Adım | Kaynaklar |
| --- | --- |
| Azure AD yönetim portalında lisansları dikey penceresine gidin | [Azure AD yönetim portalında: lisanslama](https://portal.azure.com/#blade/Microsoft_AAD_IAM/LicensesMenuBlade/Products) |
| Lisanslar POC kullanıcılarla güvenlik grubuna atayın. | [Kullanıcılar Azure Active Directory'de bir grup için lisans atama](users-groups-roles/licensing-groups-assign.md) |

### <a name="considerations"></a>Dikkat edilmesi gerekenler

Herhangi bir sorun olması durumunda Git [senaryoları, sınırlamalar ve bilinen sorunlar Azure Active Directory'de lisanslama yönetmek için grupları kullanma](users-groups-roles/licensing-group-advanced.md)

## <a name="saas-federated-sso-configuration"></a>SaaS Federasyon SSO yapılandırma

Yaklaşık bir saat tamamlamaya: 60 dakika

### <a name="pre-requisites"></a>Ön koşullar

| Önkoşul | Kaynaklar |
| --- | --- |
| Test ortamı SaaS uygulamasının kullanılabilir. Bu kılavuzda, örnek olarak servicenow'ı kullanırız.<br/>Mevcut veri kalitesi ve eşlemeleri gezinme çubuğunda uyuşmazlıkları en aza indirmek için bir test örneği kullanmak için önerilir. | Git https://developer.servicenow.com/app.do#! / test örneği alma işlemini başlatmak için giriş |
| Servicenow'ı yönetim konsoluna yönetici erişimi | [Öğretici: Azure Active Directory ServiceNow ile tümleştirme](saas-apps/servicenow-tutorial.md) |
| Hedef Kullanıcılar uygulamaya atamak için ayarlayın. PoC kullanıcıları içeren bir güvenlik grubu önerilir. <br/>Grubu oluşturmayı yapmak uygun değilse, ardından kullanıcılar için doğrudan uygulama PoC için atayın | [Kurumsal bir uygulamayı Azure Active Directory'de bir kullanıcı veya grup atama](manage-apps/assign-user-or-group-access-portal.md) |

### <a name="steps"></a>Adımlar

| Adım | Kaynaklar |
| --- | --- |
| Microsoft Documentation tüm aktörler için öğreticinin paylaşın  | [Öğretici: Azure Active Directory ServiceNow ile tümleştirme](saas-apps/servicenow-tutorial.md) |
| Bir çalışma toplantı ayarlamak ve her aktör öğretici adımlarını izleyin. | [Öğretici: Azure Active Directory ServiceNow ile tümleştirme](saas-apps/servicenow-tutorial.md) |
| Uygulama grubu Önkoşullarda atayın. POC kapsamı içinde koşullu erişimi varsa, daha sonra yeniden ziyaret ve MFA'yı eklemek ve benzer. <br/>Bu sağlama işleminde (yapılandırılmışsa) başlatır unutmayın |  [Kurumsal bir uygulamayı Azure Active Directory'de bir kullanıcı veya grup atama](manage-apps/assign-user-or-group-access-portal.md) <br/>[Bir grup oluşturun ve Azure Active Directory'de üye ekleme](fundamentals/active-directory-groups-create-azure-portal.md) |
| ServiceNow uygulama galerisinden eklemek için Azure AD Yönetim Portalı kullanın| [Azure AD Yönetim Portalı: Kurumsal uygulamalar](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/Overview) <br/>[Azure Active Directory'de Kurumsal Uygulama Yönetimi'ndeki yenilikler](active-directory-enterprise-apps-whats-new-azure-portal.md) |
| ServiceNow uygulama dikey "çoklu oturum açma", "SAML tabanlı oturum açma" etkinleştirme |  |
| ServiceNow URL'niz ile "İşareti bulunan URL" ve "Tanımlayıcı" alanları doldurun<br/>"Yeni sertifikayı etkin hale getirmek için" onay kutusunu işaretleyin<br/>ve ayarları kaydedin |  |
| ServiceNow yapılandırmanız için özelleştirilmiş yönergeleri görüntülemek için bölmenin altındaki "Yapılandırma ServiceNow" dikey penceresini açın |  |
| ServiceNow'ı yapılandırmak için yönergeleri izleyin |  |
| ServiceNow uygulama "Hazırlama" dikey penceresinde "Otomatik" sağlamayı etkinleştir | [Yeni Azure portalında Kurumsal uygulamaları için sağlama kullanıcı hesabı yönetme](manage-apps/configure-automatic-user-provisioning-portal.md) |
| Sağlama tamamlanırken birkaç dakika bekleyin.  Bu arada, sağlama raporlarda denetleyebilirsiniz. |  |
| Oturum https://myapps.microsoft.com/ erişimi olan bir test kullanıcısı olarak | [Erişim paneli nedir?](user-help/active-directory-saas-access-panel-introduction.md) |
| Az önce oluşturulan uygulama kutucuğuna tıklayın. Erişimi onaylama |  |
| İsteğe bağlı olarak, uygulama kullanım raporlarını kontrol edebilirsiniz. Bu nedenle raporlarında trafiği görmek için bir süre beklemeniz gerekir biraz gecikme süresi olduğuna dikkat edin. | [Azure Active Directory portalındaki oturum açma etkinlik raporları: yönetilen uygulamaların kullanımı](reports-monitoring/concept-sign-ins.md#usage-of-managed-applications)<br/>[Azure Active Directory rapor bekletme ilkeleri](reports-monitoring/reference-reports-data-retention.md) |

### <a name="considerations"></a>Dikkat edilmesi gerekenler

1. Yukarıda [öğretici](saas-apps/servicenow-tutorial.md) eski Azure'a başvuran AD yönetim deneyimi. Ancak PoC temel [hızlı](active-directory-enterprise-apps-whats-new-azure-portal.md#quickstart-get-going-with-your-new-application-right-away) karşılaşırsınız.
2. Hedef uygulama galerisinde mevcut değilse, "kendi uygulamanızı getir" kullanabilirsiniz. Daha fazla bilgi edinin: [Azure Active Directory'de Kurumsal Uygulama Yönetimi'nde yenilikler: tek bir yerden özel uygulamalar ekleme](active-directory-enterprise-apps-whats-new-azure-portal.md#add-custom-applications-from-one-place)

## <a name="saas-password-sso-configuration"></a>SaaS parola SSO yapılandırma

Yaklaşık tamamlama süresi: 15 dakika

### <a name="pre-requisites"></a>Ön koşullar

| Önkoşul | Kaynaklar |
| --- | --- |
| SaaS uygulamaları için test ortamı. Parola SSO HipChat ve Twitter örneğidir. Başka herhangi bir uygulama için tam html formuyla oturum açma sayfasının URL'sini gerekir. | [Microsoft Azure Marketi'nde twitter](https://azuremarketplace.microsoft.com/marketplace/apps/aad.twitter)<br/>[Microsoft Azure Marketi'nde HipChat](https://azuremarketplace.microsoft.com/marketplace/apps/aad.hipchat) |
| Uygulamaları için hesapları test edin. | [Twitter için kaydolun](https://twitter.com/signup?lang=en)<br/>[Ücretsiz olarak kaydolun: HipChat](https://www.hipchat.com/sign_up) |
| Hedef Kullanıcılar uygulamaya atamak için ayarlayın. Bir güvenlik grubu kullanıcıları bulunan önerilir. | [Kurumsal bir uygulamayı Azure Active Directory'de bir kullanıcı veya grup atama](manage-apps/assign-user-or-group-access-portal.md) |
| Internet Explorer, Chrome veya Firefox için erişim paneli uzantısını dağıtmak için bir bilgisayar için yerel yönetici erişimi | [IE için erişim paneli uzantısı](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access%20Panel%20Extension.msi)<br/>[Erişim paneli uzantısını Chrome için](https://go.microsoft.com/fwLink/?LinkID=311859&clcid=0x409)<br/>[Erişim paneli uzantısı için Firefox](https://go.microsoft.com/fwLink/?LinkID=626998&clcid=0x409) |

### <a name="steps"></a>Adımlar

| Adım | Kaynaklar |
| --- | --- |
| Tarayıcı uzantısı yükleme | [IE için erişim paneli uzantısı](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access%20Panel%20Extension.msi)<br/>[Erişim paneli uzantısını Chrome için](https://go.microsoft.com/fwLink/?LinkID=311859&clcid=0x409)<br/>[Erişim paneli uzantısı için Firefox](https://go.microsoft.com/fwLink/?LinkID=626998&clcid=0x409) |
| Uygulama galerisinden yapılandırma | [Azure Active Directory'de Kurumsal Uygulama Yönetimi'nde yenilikler: yeni ve geliştirilmiş uygulama Galerisi](active-directory-enterprise-apps-whats-new-azure-portal.md#improvements-to-the-azure-active-directory-application-gallery) |
| Parola SSO yapılandırma | [Yeni Azure portalında kurumsal uygulamalar için çoklu oturum açmayı yönetme: parola tabanlı oturum açma](manage-apps/configure-single-sign-on-portal.md#password-based-sign-on) |
| Uygulama grubu Önkoşullarda atayın | [Kurumsal bir uygulamayı Azure Active Directory'de bir kullanıcı veya grup atama](manage-apps/assign-user-or-group-access-portal.md) |
| Oturum https://myapps.microsoft.com/ erişimi olan bir test kullanıcısı olarak |  |
| Az önce oluşturulan uygulama kutucuğuna tıklayın. | [Erişim paneli nedir?: parola tabanlı SSO kimlik sağlama olmadan](user-help/active-directory-saas-access-panel-introduction.md#password-based-sso-without-identity-provisioning) |
| Uygulama kimlik bilgileri sağlayın | [Erişim paneli nedir?: parola tabanlı SSO kimlik sağlama olmadan](user-help/active-directory-saas-access-panel-introduction.md#password-based-sso-without-identity-provisioning) |
| Tarayıcıyı kapatın ve oturum açma yineleyin. Bu sefer orada kullanıcı uygulamaya sorunsuz erişim görmeniz gerekir. |  |
| İsteğe bağlı olarak, uygulama kullanım raporlarını kontrol edebilirsiniz. Bu nedenle raporlarında trafiği görmek için bir süre beklemeniz gerekir biraz gecikme süresi olduğuna dikkat edin. | [Azure Active Directory portalındaki oturum açma etkinlik raporları: yönetilen uygulamaların kullanımı](reports-monitoring/concept-sign-ins.md#usage-of-managed-applications)<br/>[Azure Active Directory rapor bekletme ilkeleri](reports-monitoring/reference-reports-data-retention.md) |

### <a name="considerations"></a>Dikkat edilmesi gerekenler

Hedef uygulama galerisinde mevcut değilse, "kendi uygulamanızı getir" kullanabilirsiniz. Daha fazla bilgi edinin: [Azure Active Directory'de Kurumsal Uygulama Yönetimi'nde yenilikler: tek bir yerden özel uygulamalar ekleme](active-directory-enterprise-apps-whats-new-azure-portal.md#add-custom-applications-from-one-place)

 Aşağıdaki gereksinimleri göz önünde bulundurun:
   * Uygulama bir bilinen oturum açma URL'si olması gerekir
   * Oturum açma sayfasında bir HTML formuna tarayıcı uzantıları otomatik olarak doldurmak bir daha fazla metin alanları içermelidir. En azından, kullanıcı adı ve parola içermelidir.

## <a name="saas-shared-accounts-configuration"></a>SaaS paylaşılan hesapları yapılandırma

Yaklaşık tamamlama süresi: 30 dakika

### <a name="pre-requisites"></a>Ön koşullar

| Önkoşul | Kaynaklar |
| --- | --- |
| Hedef uygulama ve önceden tam oturum açma URL'leri listesi. Örneğin, Twitter kullanabilirsiniz. | [Microsoft Azure Marketi'nde twitter](https://azuremarketplace.microsoft.com/marketplace/apps/aad.twitter)<br/>[Twitter için kaydolun](https://twitter.com/signup?lang=en) |
| Bu SaaS uygulaması için kimlik bilgisi paylaşılan. | [Azure AD kullanarak hesapları paylaşma](active-directory-sharing-accounts.md)<br/>[Azure AD parola toplama devretme Facebook, Twitter ve LinkedIn şimdi önizlemede otomatik! -Enterprise Mobility + Security blogu] (https://blogs.technet.microsoft.com/enterprisemobility/2015/02/20/azure-ad-automated-password-roll-over-for-facebook-twitter-and-linkedin-now-in-preview/ ) |
| Hesabın aynısını erişecek en az iki takım üyeleri için kimlik bilgileri. Güvenlik grubunun bir parçası olmaları gerekir. | [Kurumsal bir uygulamayı Azure Active Directory'de bir kullanıcı veya grup atama](manage-apps/assign-user-or-group-access-portal.md) |
| Internet Explorer, Chrome veya Firefox için erişim paneli uzantısını dağıtmak için bir bilgisayar için yerel yönetici erişimi | [IE için erişim paneli uzantısı](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access%20Panel%20Extension.msi)<br/>[Erişim paneli uzantısını Chrome için](https://go.microsoft.com/fwLink/?LinkID=311859&clcid=0x409)<br/>[Erişim paneli uzantısı için Firefox](https://go.microsoft.com/fwLink/?LinkID=626998&clcid=0x409) |

### <a name="steps"></a>Adımlar

| Adım | Kaynaklar |
| --- | --- |
| Tarayıcı uzantısı yükleme | [IE için erişim paneli uzantısı](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access%20Panel%20Extension.msi)<br/>[Erişim paneli uzantısını Chrome için](https://go.microsoft.com/fwLink/?LinkID=311859&clcid=0x409)<br/>[Erişim paneli uzantısı için Firefox](https://go.microsoft.com/fwLink/?LinkID=626998&clcid=0x409) |
| Uygulama galerisinden yapılandırma | [Azure Active Directory'de Kurumsal Uygulama Yönetimi'nde yenilikler: yeni ve geliştirilmiş uygulama Galerisi](active-directory-enterprise-apps-whats-new-azure-portal.md#improvements-to-the-azure-active-directory-application-gallery) |
| Parola SSO yapılandırma | [Yeni Azure portalında kurumsal uygulamalar için çoklu oturum açmayı yönetme: parola tabanlı oturum açma](manage-apps/configure-single-sign-on-portal.md#password-based-sign-on) |
| Grubun kimlik bilgilerini atarken Önkoşullarda tanımlanan uygulama atama | [Kurumsal bir uygulamayı Azure Active Directory'de bir kullanıcı veya grup atama](manage-apps/assign-user-or-group-access-portal.md) |
| Bu erişim uygulama olarak farklı kullanıcı olarak oturum **aynı hesabı paylaşılan.**  |  |
| İsteğe bağlı olarak, uygulama kullanım raporlarını kontrol edebilirsiniz. Bu nedenle raporlarında trafiği görmek için bir süre beklemeniz gerekir biraz gecikme süresi olduğuna dikkat edin. | [Azure Active Directory portalındaki oturum açma etkinlik raporları: yönetilen uygulamaların kullanımı](reports-monitoring/concept-sign-ins.md#usage-of-managed-applications)<br/>[Azure Active Directory rapor bekletme ilkeleri](reports-monitoring/reference-reports-data-retention.md) |


### <a name="considerations"></a>Dikkat edilmesi gerekenler

Hedef uygulama galerisinde mevcut değilse, "kendi uygulamanızı getir" kullanabilirsiniz. Daha fazla bilgi edinin: [Azure Active Directory'de Kurumsal Uygulama Yönetimi'nde yenilikler: tek bir yerden özel uygulamalar ekleme](active-directory-enterprise-apps-whats-new-azure-portal.md#add-custom-applications-from-one-place)

 Aşağıdaki gereksinimleri göz önünde bulundurun:
   * Uygulama bir bilinen oturum açma URL'si olması gerekir
   * Oturum açma sayfasında bir HTML formuna tarayıcı uzantıları otomatik olarak doldurmak bir daha fazla metin alanları içermelidir. En azından, kullanıcı adı ve parola içermelidir.

## <a name="app-proxy-configuration"></a>Uygulama Ara sunucusu yapılandırma

Yaklaşık tamamlama süresi: 20 dakika

### <a name="pre-requisites"></a>Ön koşullar

| Önkoşul | Kaynaklar |
| --- | --- |
| Microsoft Azure AD basic veya premium aboneliği ve genel Yöneticisi olduğunuz bir Azure AD dizini | [Azure Active Directory sürümleri](fundamentals/active-directory-whatis.md) |
| Bir web uygulaması için uzaktan erişimi yapılandırmak için istediğiniz şirket içinde barındırılan |  |
| Uygulama Ara sunucusu bağlayıcısını yüklemek Windows Server 2012 R2 veya Windows 8.1 veya sonraki bir sürümünü çalıştıran bir sunucu | [Azure AD uygulama ara sunucusu bağlayıcıları anlama](manage-apps/application-proxy-connectors.md) |
| Yolda bir güvenlik duvarı varsa, bağlayıcı uygulama ara sunucusuna HTTPS (TCP) istekleri yapabilmeleri için açık olduğundan emin olun. | [Azure portalında uygulama ara sunucusunu etkinleştirme: uygulama ara sunucusu önkoşulları](manage-apps/application-proxy-enable.md#application-proxy-prerequisites) |
| Kuruluşunuz internet'e bağlanmak için proxy sunucuları kullanıyorsa, sınav zamanı blog göz sonrası çalışma nasıl yapılandırılacakları hakkında ayrıntılı bilgi için var olan şirket içi proxy sunucuları | [Mevcut şirket içi proxy sunucuları ile çalışma](manage-apps/application-proxy-configure-connectors-with-proxy-servers.md) |


### <a name="steps"></a>Adımlar

| Adım | Kaynaklar |
| --- | --- |
| Bir bağlayıcı sunucuya yükleyin. | [Azure portalında uygulama ara sunucusunu etkinleştirme: yükleme ve kaydetme Bağlayıcısı](manage-apps/application-proxy-enable.md#install-and-register-a-connector) |
| Azure ad uygulama proxy'si uygulaması olarak şirket içi uygulama yayımlama | [Azure AD uygulama ara sunucusu kullanarak uygulama yayımlama](manage-apps/application-proxy-publish-azure-portal.md) |
| Test kullanıcıları atama | [Azure AD uygulama ara sunucusu kullanarak uygulama yayımlama: bir test kullanıcısı Ekle](manage-apps/application-proxy-publish-azure-portal.md#add-a-test-user) |
| İsteğe bağlı olarak, kullanıcılarınız için çoklu oturum açma deneyimini yapılandırın | [Azure AD uygulama proxy'si ile çoklu oturum açma sağlayın](manage-apps/application-proxy-configure-single-sign-on-password-vaulting.md) |
| MyApps portalında atanan kullanıcı olarak oturum açarak uygulamayı test etme | https://myapps.microsoft.com |

### <a name="considerations"></a>Dikkat edilmesi gerekenler

1. Bağlayıcıyı şirket ağınızda koyarak öneririz, ancak durumlar vardır, bulutta yerleştirme daha iyi performans görürsünüz. Daha fazla bilgi edinin: [Azure Active Directory Uygulama proxy'si kullanılırken ağ topolojisi hakkında önemli noktalar](manage-apps/application-proxy-network-topology.md)
2. Daha fazla güvenlik ayrıntıları ve nasıl bu özellikle güvenli uzaktan erişim sağlar. yalnızca giden bağlantılar saklayarak çözüm için bkz: [Azure AD uygulama proxy'si kullanarak uygulamalara uzaktan erişim için güvenlik konuları](manage-apps/application-proxy-security.md)

## <a name="generic-ldap-connector-configuration"></a>Genel LDAP Bağlayıcısı yapılandırması

Yaklaşık bir saat tamamlamaya: 60 dakika

> [!IMPORTANT]
> Bu, FIM/MIM bazı bilgisi gerektiren, Gelişmiş bir yapılandırmadır. Üretim ortamında, ki bu yapılandırma hakkında sorular öneri kullandıysanız geçtikleri [Premier Destek](https://support.microsoft.com/premier).

### <a name="pre-requisites"></a>Ön koşullar

| Önkoşul | Kaynaklar |
| --- | --- |
| Azure AD Connect yüklenir ve yapılandırılır. | Yapı Taşı: [dizin eşitleme - parola karması eşitleme](#directory-synchronization--password-hash-sync-phs--new-installation) |
| ADLDS örneği toplantı gereksinimleri | [Genel LDAP Bağlayıcısı teknik başvurusu: Genel LDAP Bağlayıcısı genel bakış](https://docs.microsoft.com/microsoft-identity-manager/reference/microsoft-identity-manager-2016-connector-genericldap#overview-of-the-generic-ldap-connector) |
| Kullanıcılar kullanan iş yükleri ve bu iş yükleri ile ilişkili öznitelikleri listesi | [Azure AD Connect eşitleme: Azure Active Directory ile eşitlenen öznitelikler](./connect/active-directory-aadconnectsync-attributes-synchronized.md) |


### <a name="steps"></a>Adımlar

| Adım | Kaynaklar |
| --- | --- |
| Genel LDAP Bağlayıcısı Ekle | [Genel LDAP Bağlayıcısı teknik başvurusu: yeni bir bağlayıcı oluşturun](https://docs.microsoft.com/microsoft-identity-manager/reference/microsoft-identity-manager-2016-connector-genericldap#create-a-new-connector) |
| (Tam içeri aktarma, değişikliği içeri aktarma, tam eşitleme, delta eşitleme, dışarı aktarma) oluşturulan bağlayıcısının çalıştırma profillerini oluşturma | [Bir yönetim Aracısı çalıştırma profili oluşturma](https://technet.microsoft.com/library/jj590219(v=ws.10).aspx)<br/> [Azure AD Connect eşitleme hizmeti yöneticisiyle bağlayıcıları kullanma](./connect/active-directory-aadconnectsync-service-manager-ui-connectors.md)|
| Tam içeri aktarma profilini çalıştırmak ve bağlayıcı alanı nesne olmadığını doğrulayın | [Bağlayıcı alanı nesnesi arayın](https://technet.microsoft.com/library/jj590287(v=ws.10).aspx)<br/>[Bağlayıcıları kullanarak Azure AD Connect eşitleme hizmeti yöneticisiyle: arama bağlayıcı alanı](./connect/active-directory-aadconnectsync-service-manager-ui-connectors.md#search-connector-space) |
| Eşitleme kuralları oluşturabilir ve böylece meta veri deposu nesneleri iş yükleri için gerekli özniteliklere sahip. | [Azure AD Connect eşitleme: varsayılan yapılandırmanın değiştirilmesine yönelik en iyi uygulamalar: değişiklikleri eşitleme kuralları](./connect/active-directory-aadconnectsync-best-practices-changing-default-configuration.md#changes-to-synchronization-rules)<br/>[Azure AD Connect eşitleme: anlama, bildirim temelli sağlama](./connect/active-directory-aadconnectsync-understanding-declarative-provisioning.md)<br/>[Azure AD Connect eşitleme: bildirim temelli sağlama ifadelerini anlama](./connect/active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md) |
| Tam eşitleme döngüsünü Başlat | [Azure AD Connect eşitleme: Scheduler: Zamanlayıcı'yı başlatma](./connect/active-directory-aadconnectsync-feature-scheduler.md#start-the-scheduler) |
| Sorun giderme hataları durumunda yapın | [Azure AD ile eşitlenmeyen bir nesneyle ilgili sorunları giderme](./connect/active-directory-aadconnectsync-troubleshoot-object-not-syncing.md) |
| LDAP kullanıcı oturum açma ve uygulama erişimi olduğunu doğrulayın | https://myapps.microsoft.com |

### <a name="considerations"></a>Dikkat edilmesi gerekenler

> [!IMPORTANT]
> Bu, FIM/MIM bazı bilgisi gerektiren, Gelişmiş bir yapılandırmadır. Üretim ortamında, ki bu yapılandırma hakkında sorular öneri kullandıysanız geçtikleri [Premier Destek](https://support.microsoft.com/premier).

## <a name="groups---delegated-ownership"></a>Temsilci gruplar - sahipliği

Yaklaşık tamamlama süresi: 10 dakika

### <a name="pre-requisites"></a>Ön koşullar

| Önkoşul | Kaynaklar |
| --- | --- |
| SaaS uygulaması (Federasyon SSO veya parola SSO) zaten yapılandırıldı | Yapı Taşı: [SaaS Federasyon SSO yapılandırma](#saas-federated-sso-configuration) |
| Bulut uygulaması #1'na erişim atanan Grup tanımlanır | Yapı Taşı: [SaaS Federasyon SSO yapılandırma](#saas-federated-sso-configuration) <br/>[Bir grup oluşturun ve Azure Active Directory'de üye ekleme](fundamentals/active-directory-groups-create-azure-portal.md) |
| Grup sahibi için kimlik bilgileri kullanılabilir | [Azure Active Directory grupları ile kaynaklara erişimi yönetme](fundamentals/active-directory-manage-groups.md) |
| Kimlik bilgilerini kullanarak uygulamalara bilgi çalışanı için tanımlanan | [Erişim paneli nedir?](user-help/active-directory-saas-access-panel-introduction.md) |


### <a name="steps"></a>Adımlar

| Adım | Kaynaklar |
| --- | --- |
| Uygulamaya erişim izni verilen grubu tanımlayın ve verilen sahibi yapılandırma grubu| [Azure Active Directory'de bir grubun ayarlarını yönetme ](fundamentals/active-directory-groups-settings-azure-portal.md) |
| Grup sahibi olarak oturum açın, grup üyeliği gruplar sekmesinde erişim paneli içinde bakın | [Azure Active Directory grupları Yönetimi sayfası](https://account.activedirectory.windowsazure.com/r#/groups) |
| Test etmek istediğiniz bilgi çalışanı Ekle |  |
| Bilgi çalışanı olarak oturum açın, kutucuk kullanılabilir onaylayın | [Erişim paneli nedir?](user-help/active-directory-saas-access-panel-introduction.md) |

### <a name="considerations"></a>Dikkat edilmesi gerekenler

Uygulama sağlamayı etkinleştirilmiş sahipse, sağlama uygulaması bilgi çalışanı olarak erişmeden önce tamamlanması için birkaç dakika beklemeniz gerekebilir.

## <a name="saas-and-identity-lifecycle"></a>SaaS ve kimlik yaşam döngüsü

### <a name="pre-requisites"></a>Ön koşullar

| Önkoşul | Kaynaklar |
| --- | --- |
| SaaS uygulaması (Federasyon SSO veya parola SSO) zaten yapılandırıldı | Yapı Taşı: [SaaS Federasyon SSO yapılandırma](#saas-federated-sso-configuration) |
| Bulut uygulaması #1'na erişim atanan Grup tanımlanır | Yapı Taşı: [SaaS Federasyon SSO yapılandırma](#saas-federated-sso-configuration) <br/>[Bir grup oluşturun ve Azure Active Directory'de üye ekleme](fundamentals/active-directory-groups-create-azure-portal.md) |
| Kimlik bilgilerini kullanarak uygulamalara bilgi çalışanı için tanımlanan | [Erişim paneli nedir?](user-help/active-directory-saas-access-panel-introduction.md) |


### <a name="steps"></a>Adımlar

| Adım | Kaynaklar |
| --- | --- |
| Kullanıcı, uygulamayı atanan grubundan kaldırın. | [Azure Active Directory kiracınızdaki kullanıcılar için Grup üyeliğini yönetme](fundamentals/active-directory-groups-members-azure-portal.md) |
| Sağlamayı için birkaç dakika bekleyin | [SaaS uygulama kullanıcı sağlama Azure AD'de otomatik: otomatik sağlama iş nasıl yapar?](active-directory-saas-app-provisioning.md#how-does-automatic-provisioning-work) |
| Ayrı bir tarayıcı oturumu, bilgi çalışanı my apps portalı olarak oturum açın ve kutucuğa eksik onaylayın | http://myapps.microsoft.com |


### <a name="considerations"></a>Dikkat edilmesi gerekenler

PoC senaryoya leavers ve/veya Mazeret senaryoları tahmin. Kullanıcı devre dışı, şirket içi AD veya kaldırıldı, artık SaaS uygulaması için oturum açmak için bir yolu yoktur.

## <a name="self-service-access-to-application-management"></a>Self Servis uygulama yönetimine erişimi

Yaklaşık tamamlama süresi: 10 dakika

### <a name="pre-requisites"></a>Ön koşullar

| Önkoşul | Kaynaklar |
| --- | --- |
| Uygulamalara erişimi güvenlik grubunun bir parçası isteyecek POC kullanıcıları belirleyin | Yapı Taşı: [SaaS Federasyon SSO yapılandırma](#saas-federated-sso-configuration) |
| Hedef uygulama dağıtıldı | Yapı Taşı: [SaaS Federasyon SSO yapılandırma](#saas-federated-sso-configuration) |

### <a name="steps"></a>Adımlar

| Adım | Kaynaklar |
| --- | --- |
| Azure AD yönetim portalında kurumsal uygulamalar dikey penceresine gidin | [Azure AD yönetim portalında: Kurumsal uygulamalar](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/AllApps/menuId/) |
| Ön koşullar uygulamasından self servis ile yapılandırma | [Azure Active Directory'de Kurumsal Uygulama Yönetimi'nde yenilikler: Self Servis uygulama erişimi yapılandırma](active-directory-enterprise-apps-whats-new-azure-portal.md#configure-self-service-application-access) |
| My apps portalına bilgi çalışanı olarak oturum açın | http://myapps.microsoft.com |
| Fark "+ uygulama Ekle" düğmesine op sayfanın. Uygulama erişim elde etmek için kullanın |  |

### <a name="considerations"></a>Dikkat edilmesi gerekenler

Seçilen uygulamalar sağlama gereksinimleri olabilir, bu nedenle hemen uygulamaya gidip bazı hatalara neden olabilir. Seçilen uygulamayı azure AD'ye sağlama destekliyorsa ve yapılandırıldığı, bunu bir fırsat olarak çalışan uçtan uca tüm akışını göstermek için kullanabilirsiniz. Görmek için Yapı Taşı [SaaS Federasyon SSO yapılandırma](#saas-federated-sso-configuration) daha fazla öneri için

## <a name="self-service-password-reset"></a>Self Servis Parola Sıfırlama

Yaklaşık tamamlama süresi: 15 dakika

### <a name="pre-requisites"></a>Ön koşullar

| Önkoşul | Kaynaklar |
| --- | --- |
| Kiracınızda parola Self Servis yönetimini etkinleştirin. | [Azure Active Directory parola sıfırlama için BT yöneticileri](user-help/active-directory-passwords-update-your-own-password.md) |
| Parola geri yazma özelliğini şirket içi parolaları yönetmek etkinleştirin. Bunu gerektiren belirli Azure AD Not sürümleri bağlanma | [Parola Geri Yazma önkoşulları](authentication/howto-sspr-writeback.md) |
| Bu işlevselliği kullanmak ve bunların güvenlik grubunun üyesi olduğundan emin olun PoC kullanıcıları belirleyin. Kullanıcılar Yönetici olmayanlar özelliği tam olarak göstermek için olmalıdır | [Özelleştirme: Azure AD parola yönetimi: parola sıfırlama için erişimi kısıtla](authentication/howto-sspr-writeback.md) |


### <a name="steps"></a>Adımlar

| Adım | Kaynaklar |
| --- | --- |
| Azure AD yönetim portalına gidin: parola sıfırlama | [Azure AD yönetim portalında: Parola sıfırlama](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/PasswordReset) |
| Parola sıfırlama İlkesi belirler. POC amacıyla telefon araması ve soru- cevap kullanabilirsiniz Erişim paneli oturum açma gerekmiyor gibi kaydını etkinleştirmek için önerilir |  |
| Oturumu Kapat ve bir bilgi çalışanı oturum açın |  |
| 2. adım yapılandırıldığı gibi Self Servis parola sıfırlama verileri sağlayın | https://aka.ms/ssprsetup |
| Tarayıcıyı kapatın |  |
| 4. adımda kullanılan bilgi çalışanı olarak oturum açma işlemini Başlat |  |
| Parola sıfırlama | [Kendi parolanızı güncelleştirme: parolamı sıfırlama](user-help/active-directory-passwords-update-your-own-password.md) |
| Yeni parolanızı şirket içi kaynaklar için de Azure ad oturum deneyin |  |

### <a name="considerations"></a>Dikkat edilmesi gerekenler

1. Azure AD Connect yükseltme, uyuşmazlıkları neden olacaksa, ardından karşı bulut hesapları kullanmayı deneyin veya ayrı bir ortam karşı bir tanıtım yapın
2. Farklı bir ilke Yöneticiler sahiptir ve parolayı sıfırlamak için yönetici hesabı kullanarak PoC taint ve kullanıcılarda kafa karışıklığına neden. Sıfırlama işlemleri sınamak için normal bir kullanıcı hesabı kullandığınızdan emin olun


## <a name="azure-multi-factor-authentication-with-phone-calls"></a>Azure multi-Factor Authentication aramaları ile

Yaklaşık tamamlama süresi: 10 dakika

### <a name="pre-requisites"></a>Ön koşullar

| Önkoşul | Kaynaklar |
| --- | --- |
| MFA kullanacak POC kullanıcıları belirleyin  |  |
| MFA testini iyi alımını ile telefon  | [Azure Multi-Factor Authentication nedir?](authentication/multi-factor-authentication.md) |

### <a name="steps"></a>Adımlar

| Adım | Kaynaklar |
| --- | --- |
| Azure AD yönetim portalında "Kullanıcılar ve Gruplar" dikey penceresine gidin | [Azure AD yönetim portalında: Kullanıcılar ve gruplar](https://portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/Overview/menuId/) |
| "Tüm kullanıcılar" dikey penceresini seçin |  |
| Üst Çubuk Seç "Çok faktörlü kimlik doğrulaması" düğmesi | Doğrudan Azure mfa'yı portalı URL'si: https://aka.ms/mfaportal |
| "Kullanıcı" ayarlarında PoC kullanıcıları seçin ve bunları için mfa'yı etkinleştirme | [Azure Multi-Factor Authentication’da Kullanıcı Durumları](authentication/howto-mfa-userstates.md) |
| PoC kullanıcı ve kavram işlemimiz bir kılavuz olarak oturum açın  |  |

### <a name="considerations"></a>Dikkat edilmesi gerekenler

1. Bu yapı taşı açık olarak bir kullanıcı için mfa'yı tüm oturum açma bilgilerini ayarlama PoC adımları. MFA hakkında daha fazla etkileşim kurun. koşullu erişim ve kimlik koruması gibi diğer araçları vardır hedeflenen senaryoları. Bu POC üretime taşırken dikkate alınması gereken bir şey olacaktır.
2. Bu yapı taşı PoC adımlarda, telefon aramaları expedience için mfa'yı yöntemi olarak açıkça kullanıyor. POC üretim ortamına geçiş gibi uygulamalar gibi kullanılması önerilir [Microsoft Authenticator](user-help/microsoft-authenticator-app-how-to.md) , mümkün olduğunda ikinci öğe olarak.
Daha fazla bilgi edinin: [taslak NIST özel yayını 800-63B](https://pages.nist.gov/800-63-3/sp800-63b.html)

## <a name="mfa-conditional-access-for-saas-applications"></a>MFA SaaS uygulamaları için koşullu erişim

Yaklaşık tamamlama süresi: 10 dakika

### <a name="pre-requisites"></a>Ön koşullar

| Önkoşul | Kaynaklar |
| --- | --- |
| İlkesinin hedefleyeceği PoC kullanıcıları belirleyin. Bu kullanıcılar, koşullu erişim ilkesi kapsamı için bir güvenlik grubu olmalıdır | [SaaS Federasyon SSO yapılandırma](#saas-federated-sso-configuration) |
| SaaS uygulaması zaten yapılandırıldı |  |
| PoC kullanıcı uygulamaya zaten atanmış |  |
| POC kullanıcı kimlik bilgileri kullanılabilir |  |
| POC kullanıcı MFA için kayıtlı. Bir telefon ile iyi almayı kullanma | https://aka.ms/ssprsetup |
| Cihaz iç ağdaki. Yapılandırılmış iç adres aralığındaki IP adresi | IP adresiniz bulun: https://www.bing.com/search?q=what%27s+my+ip |
| Cihaz dış ağdaki (mobil taşıyıcı ağı kullanarak bir telefon olabilir) |  |

### <a name="steps"></a>Adımlar

| Adım | Kaynaklar |
| --- | --- |
| Azure AD yönetim portalına gidin: koşullu erişim dikey penceresi | [Azure AD yönetim portalında: Koşullu erişim](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/Policies) |
| Koşullu erişim ilkesi oluşturun:<br/>-"Kullanıcılar ve Gruplar" altında hedef PoC kullanıcılar<br/>-"Bulut uygulamaları" altındaki target PoC uygulama<br/>-Güvenilen olanları "Koşullarda", "Konum" -> dışında tüm konumların hedef **Not:** güvenilen IP'ler yapılandırılmış [MFA portalı](https://account.activedirectory.windowsazure.com/UserManagement/MfaSettings.aspx)<br/>-"Verme" altında çok faktörlü kimlik doğrulaması gerektir | [Koşullu erişim ilkenizi oluşturun](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-mfa#create-your-conditional-access-policy) |
| Erişim kurumsal ağ içinde uygulama | [Koşullu erişim ilkenizi test](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-mfa#test-your-conditional-access-policy) |
| Ortak ağ erişimi uygulama | [Koşullu erişim ilkenizi test](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-mfa#test-your-conditional-access-policy) |

### <a name="considerations"></a>Dikkat edilmesi gerekenler

Federasyon kullanıyorsanız talep ile iç/dış kurumsal ağ durumunu iletişim kurmak için şirket içi kimlik sağlayıcısı (IDP) kullanabilirsiniz. Değerlendirmek ve büyük kuruluşlarda yönetmek için karmaşık olabilen IP adreslerinin listesi yönetmenize gerek kalmadan bu tekniği kullanabilirsiniz. Bu kurulum, "Ağ dolaşım" senaryosu (bir kafe gibi konumları iç ağdan ve oturum açmış anahtarları çalışırken oturum bir kullanıcı) için hesap ve sonuçlarını anladığınızdan emin olun. Daha fazla bilgi edinin: [Azure multi-Factor Authentication ve AD FS ile bulut kaynaklarını güvenli hale getirme: güvenilen IP'leri Federasyon kullanıcıları için](authentication/howto-mfa-adfs.md#trusted-ips-for-federated-users)

## <a name="privileged-identity-management-pim"></a>Privileged Identity Management (PIM)

Yaklaşık tamamlama süresi: 15 dakika

### <a name="pre-requisites"></a>Ön koşullar

| Önkoşul | Kaynaklar |
| --- | --- |
| PIM için POC parçası olacak bir genel yönetici tanımlayın | [Azure AD Privileged Identity Management'ı kullanmaya başlayın](privileged-identity-management/pim-getting-started.md) |
| Güvenlik Yöneticisi olacak genel yönetici tanımlayın | [Azure AD Privileged Identity Management'ı kullanmaya başlayın](privileged-identity-management/pim-getting-started.md)<br/> [Azure Active Directory PIM farklı yönetim rolleri](privileged-identity-management/pim-roles.md) |
| İsteğe bağlı: Genel Yöneticiler e-posta bildirimleri PIM kullanmak için e-posta erişimi olup olmadığını onaylayın | [Azure AD Privileged Identity Management nedir?: rol etkinleştirme ayarlarını yapılandırma](privileged-identity-management/pim-configure.md#configure-the-role-activation-settings)


### <a name="steps"></a>Adımlar

| Adım | Kaynaklar |
| --- | --- |
| Oturum açma https://portal.azure.com bir genel yönetici (GA) ve önyükleme PIM dikey olarak. Bu adımı gerçekleştirir genel yönetici güvenlik yöneticisi olarak sağlanmış.  Bu aktör GA1 adlandıralım | [İçinde Azure AD Privileged Identity Management Güvenlik Sihirbazı'nı kullanarak](privileged-identity-management/pim-security-wizard.md) |
| Genel yönetici tanımlamak ve kalıcı uygun şekilde taşıyın. Bu, 1. adımda daha anlaşılır olması için kullanılan hesaptan farklı bir yönetici olmanız gerekir. Bu aktör GA2 adlandıralım | [Azure AD Privileged Identity Management: nasıl kullanıcı rolü ekleme veya kaldırma](privileged-identity-management/pim-how-to-add-role-to-user.md)<br/>[Azure AD Privileged Identity Management nedir?: rol etkinleştirme ayarlarını yapılandırma](privileged-identity-management/pim-configure.md#configure-the-role-activation-settings)  |
| Şimdi, oturum için GA2 olarak https://portal.azure.com ve "Kullanıcı ayarları" değiştirmeyi deneyin. Bazı seçenekler gri unutmayın. | |
| Şimdi gidin yeni bir sekmede ve 3. adım ile aynı oturumda https://portal.azure.com ve PIM dikey pencereyi panoya ekleyin. | [Etkinleştirmek veya Azure AD Privileged Identity Management içinde rol devre dışı bırakma: Privileged Identity Management uygulamasını ekleme](privileged-identity-management/pim-how-to-activate-role.md#add-the-privileged-identity-management-application) |
| Genel yönetici rolü etkinleştirme isteği | [Nasıl etkinleştirme veya devre dışı rolleri Azure AD Privileged Identity Management: rol etkinleştirme](privileged-identity-management/pim-how-to-activate-role.md#activate-a-role) |
| GA2 hiçbir zaman MFA için kaydolduysanız, kayıt için Azure mfa'yı gerekli olduğunu unutmayın |  |
| Adım 3'teki özgün sekmesine geri dönün ve tarayıcıyı yenile düğmesine tıklayın. Artık "kullanıcı ayarları" erişiminiz olduğunu unutmayın. | |
| İsteğe bağlı olarak, genel Yöneticiler e-posta etkin olması, GA1 ve GA2'ın gelen kutusunu kontrol edin ve bildirim etkinleştirilmekte olan rolünün bakın |  |
| 8 denetim geçmişini denetleyin ve raporun GA2 ayrıcalıkların onaylamak için gösterilen gözlemleyin. | [Azure AD Privileged Identity Management nedir?: rol etkinliği gözden geçirin](privileged-identity-management/pim-configure.md#review-role-activity) |

### <a name="considerations"></a>Dikkat edilmesi gerekenler

Bu özellik Azure AD Premium P2 ve/veya EMS E5 parçasıdır

## <a name="discovering-risk-events"></a>Risk olaylarını bulma

Yaklaşık tamamlama süresi: 20 dakika

### <a name="pre-requisites"></a>Ön koşullar

| Önkoşul | Kaynaklar |
| --- | --- |
| Tor tarayıcısı olan bir cihaz indirilmesi ve yüklenmesi | [Tor tarayıcı indirin](https://www.torproject.org/projects/torbrowser.html.en#downloads) |
| Oturum açma yapmak için POC kullanıcıya erişim | [Azure Active Directory kimlik koruması Kılavuzu](identity-protection/playbook.md) |

### <a name="steps"></a>Adımlar

| Adım | Kaynaklar |
| --- | --- |
| Açık tor tarayıcı | [Tor tarayıcı indirin](https://www.torproject.org/projects/torbrowser.html.en#downloads) |
| Oturum https://myapps.microsoft.com POC kullanıcı hesabıyla | [Azure Active Directory kimlik koruması Kılavuzu: Risk olaylarının benzetimini](identity-protection/playbook.md#simulating-risk-events) |
| 5-7 dakika bekleyin |  |
| İçin genel yönetici olarak oturum https://portal.azure.com ve kimlik koruma dikey penceresini açın | https://aka.ms/aadipgetstarted |
| Risk olayları dikey penceresini açın. "Oturum açma işlemleri anonim IP adreslerinden" altında bir giriş görmeniz gerekir  | [Azure Active Directory kimlik koruması Kılavuzu: Risk olaylarının benzetimini](identity-protection/playbook.md#simulating-risk-events) |

### <a name="considerations"></a>Dikkat edilmesi gerekenler

Bu özellik Azure AD Premium P2 ve/veya EMS E5 parçasıdır

## <a name="deploying-sign-in-risk-policies"></a>Oturum açma risk ilkelerini dağıtma

Yaklaşık tamamlama süresi: 10 dakika

### <a name="pre-requisites"></a>Ön koşullar

| Önkoşul | Kaynaklar |
| --- | --- |
| Tor tarayıcısı olan bir cihaz indirilmesi ve yüklenmesi | [Tor tarayıcı indirin](https://www.torproject.org/projects/torbrowser.html.en#downloads) |
| Erişim Günlüğü'ndeki test yapmak için POC kullanıcı olarak |  |
| POC kullanıcı MFA ile kaydedilir. Bir telefon ile iyi alma kullandığınızdan emin olun | Yapı Taşı: [Azure çok faktörlü kimlik doğrulama telefon çağrıları](#azure-multi-factor-authentication-with-phone-calls) |


### <a name="steps"></a>Adımlar

| Adım | Kaynaklar |
| --- | --- |
| İçin genel yönetici olarak oturum https://portal.azure.com ve kimlik koruma dikey penceresini açın | https://aka.ms/aadipgetstarted |
| Oturum açma riski İlkesi şu şekilde etkinleştirin:<br/>-Atanan: POC kullanıcı<br/>-Koşullar: Oturum açma riski Orta büyüklükte ya da daha yüksek (oturum açma anonim konumdan kabul orta düzeyde risk düzeyi olarak)<br/>-Denetimleri: MFA gerektirme | [Azure Active Directory kimlik koruması Kılavuzu: oturum açma riski](identity-protection/playbook.md) |
| Açık tor tarayıcı | [Tor tarayıcı indirin](https://www.torproject.org/projects/torbrowser.html.en#downloads) |
| Oturum https://myapps.microsoft.com PoC kullanıcı hesabıyla |  |
| MFA testini dikkat edin. | [Azure AD kimlik koruması ile karşılaştığında oturum açma: oturum açma riskli kurtarma](identity-protection/flows.md#risky-sign-in-recovery)

### <a name="considerations"></a>Dikkat edilmesi gerekenler

Bu özellik Azure AD Premium P2 ve/veya EMS E5 parçasıdır. Ziyaret risk olayları hakkında daha fazla bilgi edinmek için: [Azure Active Directory risk olayları](reports-monitoring/concept-risk-events.md)

## <a name="configuring-certificate-based-authentication"></a>Sertifika tabanlı kimlik doğrulamasını yapılandırma

Yaklaşık tamamlanma süresi: 20 dakika

### <a name="pre-requisites"></a>Ön koşullar

| Önkoşul | Kaynaklar |
| --- | --- |
| Sağlanan kullanıcı sertifikadan (Windows, iOS veya Android) Kuruluş PKI ile cihaz | [Kullanıcı sertifikalarını dağıtma](https://msdn.microsoft.com/library/cc770857.aspx) |
| Azure AD etki alanı AD FS ile Federasyon | [Azure AD Connect ve federasyon](./connect/active-directory-aadconnectfed-whatis.md)<br/>[Active Directory Sertifika Hizmetleri'ne Genel Bakış](https://technet.microsoft.com/library/hh831740.aspx)|
| İOS cihazları için Microsoft Authenticator uygulamasının yüklü olması | [Microsoft Authenticator uygulamasını kullanmaya başlama](user-help/microsoft-authenticator-app-how-to.md) |

### <a name="steps"></a>Adımlar

| Adım | Kaynaklar |
| --- | --- |
| "Sertifika kimlik doğrulamasını" ADFS etkinleştirin | [Kimlik doğrulama ilkeleri yapılandırın: birincil kimlik doğrulaması, Windows Server 2012 R2'de genel olarak yapılandırmak için](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/configure-authentication-policies#to-configure-primary-authentication-globally-in-windows-server-2012-r2) |
| İsteğe bağlı: Sertifika kimlik doğrulamasını Azure AD'de Exchange Active Sync istemcileri için etkinleştirme | [Azure Active Directory’de sertifika tabanlı kimlik doğrulamayı kullanmaya başlama](active-directory-certificate-based-authentication-get-started.md) |
| Erişim paneli gidin ve kullanıcı sertifikası kullanılarak kimlik doğrulaması | https://myapps.microsoft.com |

### <a name="considerations"></a>Dikkat edilmesi gerekenler

Ziyaret uyarılar bu dağıtım hakkında daha fazla bilgi edinmek için: [ADFS: sertifika kimlik doğrulaması ile Azure AD & Office 365](https://blogs.msdn.microsoft.com/samueld/2016/07/19/adfs-certauth-aad-o365/)



> [!NOTE]
> Kullanıcı sertifikası elinde korunmalıdır. Ya da cihazları yönetmeye durumunda akıllı kart PIN ile.



[!INCLUDE [active-directory-playbook-toc](../../includes/active-directory-playbook-steps.md)]
