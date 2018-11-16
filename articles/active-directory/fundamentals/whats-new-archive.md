---
title: Yenilikler için arşiv? Azure Active Directory'de | Microsoft Docs
description: Etkinliğin 6 ay bu içerik kümesinin bölümünü içeren yeni sürüm notları genel bakışta nedir? 6 ay sonra öğeleri ana makalesinden kaldırılır ve Arşiv makalede yerleştirin.
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.component: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 11/14/2018
ms.author: lizross
ms.reviewer: dhanyahk
ms.openlocfilehash: 22e0f95b1afec39574309a8deed8a020145c38ec
ms.sourcegitcommit: 275eb46107b16bfb9cf34c36cd1cfb000331fbff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51708813"
---
# <a name="archive-for-whats-new-in-azure-active-directory"></a>Yenilikler için arşiv? Azure Active Directory'de

Birincil [yeni sürüm notları nedir](whats-new.md) makale, son 6 ay bilgilerinin tüm eski bilgileri bu makalede içerse de içerir.

Yeni sürüm notları nedir bilgilerle ilgili sağlar:

- En son sürümleri
- Bilinen sorunlar
- Hata düzeltmeleri
- Kullanım dışı işlev
- Değişiklikleri planları

---
 
## <a name="april-2018"></a>Nisan 2018 

### <a name="azure-ad-b2c-access-token-are-ga"></a>Azure AD B2C erişim belirteci olan genel kullanım

**Türü:** yeni özellik  
**Hizmet kategorisi:** B2C - tüketici Kimlik Yönetimi  
**Ürün özelliği:** B2B/B2C 

Azure AD B2C ile güvenliği sağlanan Web API'leri artık erişebilirsiniz erişim belirteçleri kullanarak. Özellik genel Önizleme için büyüyecek taşınıyor Azure AD B2C uygulamaları ve web API'leri yapılandırmak için kullanıcı Arabirimi deneyimi İyileştirildi ve başka küçük geliştirmeler yapıldı.
 
Daha fazla bilgi için [Azure AD B2C: istenen erişim belirteçleri](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-access-tokens).

---

### <a name="test-single-sign-on-configuration-for-saml-based-applications"></a>SAML tabanlı uygulamalar için çoklu oturum açma yapılandırması test

**Türü:** yeni özellik  
**Hizmet kategorisi:** kurumsal uygulamalar  
**Ürün özelliği:** SSO

SAML tabanlı SSO uygulamaları yapılandırırken, yapılandırma sayfasında tümleştirme test edebilirsiniz. Oturum açma sırasında bir hatayla karşılaşırsanız, hata test deneyiminde sağlayabilir ve Azure AD ile çözüm adımları belirli sorunu çözmek için sağlar.

Daha fazla bilgi için bkz.

- [Azure Active Directory uygulama galerisinde bulunmayan uygulamalar için çoklu oturum açmayı yapılandırma](https://docs.microsoft.com/azure/active-directory/active-directory-saas-custom-apps)
- [Azure Active Directory'de uygulamalar için SAML tabanlı çoklu oturum açma hata ayıklama](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-debugging)

---
 
### <a name="azure-ad-terms-of-use-now-has-per-user-reporting"></a>Azure AD kullanım koşulları, artık raporlama kullanıcı başına sahiptir

**Türü:** yeni özellik  
**Hizmet kategorisi:** kullanım koşulları  
**Ürün özelliği:** uyumluluk
 
Yöneticiler artık belirli bir ToU'ı seçin ve ToU ve hangi, tarih yer aldığı onay tüm kullanıcılar görür.

Daha fazla bilgi için [Azure AD kullanım koşulları özelliği](https://docs.microsoft.com/azure/active-directory/active-directory-tou).

---
 
### <a name="azure-ad-connect-health-risky-ip-for-ad-fs-extranet-lockout-protection"></a>Azure AD Connect Health'i: AD FS extranet kilitleme koruması riskli IP 

**Türü:** yeni özellik  
**Hizmet kategorisi:** diğer  
**Ürün özelliği:** izleme ve Raporlama

Bir saatlik veya günlük olarak hatalı U/P oturum açma eşiğini aşan IP algılama özelliğine destekler adresleri artık connect Health. Bu özellik tarafından sağlanan özellikler şunlardır:

- IP adresi ve bir saatlik/günlük olarak özelleştirilebilir eşik ile oluşturulan başarısız oturum açma sayısını gösteren kapsamlı bir rapor.
- E-posta tabanlı uyarılar belirli bir IP adresi, gösteren saatlik/günlük olarak hatalı U/P oturum açma eşiğini aştı.
- Verilerin ayrıntılı bir analiz yapmak için bir yükleme seçeneği

Daha fazla bilgi için [riskli IP raporu](https://aka.ms/aadchriskyip).

---
 
### <a name="easy-app-config-with-metadata-file-or-url"></a>Meta veri dosyası veya URL ile kolay uygulama yapılandırması

**Türü:** yeni özellik  
**Hizmet kategorisi:** kurumsal uygulamalar  
**Ürün özelliği:** SSO

Kurumsal uygulamalar sayfasında Yöneticiler SAML tabanlı oturum açma AAD galeri ve galeri dışı bir uygulama için yapılandırmak için bir SAML meta veri dosyasını karşıya yükleyebilirsiniz.

Ayrıca, Azure AD uygulama Federasyon meta veri URL'si ile hedeflenen uygulamaya SSO yapılandırmak için kullanabilirsiniz.

Daha fazla bilgi için [Azure Active Directory Uygulama galerisinde bulunmayan uygulamalar için çoklu oturum açma yapılandırma](https://docs.microsoft.com/azure/active-directory/active-directory-saas-custom-apps).

---

### <a name="azure-ad-terms-of-use-now-generally-available"></a>Azure AD kullanım koşulları artık kullanıma sunuldu

**Türü:** yeni özellik  
**Hizmet kategorisi:** kullanım koşulları  
**Ürün özelliği:** uyumluluk
 

Azure AD kullanım koşulları, genel Önizleme için kullanıma sunuldu taşıdık.

Daha fazla bilgi için [Azure AD kullanım koşulları özelliği](https://docs.microsoft.com/azure/active-directory/active-directory-tou).

---

### <a name="allow-or-block-invitations-to-b2b-users-from-specific-organizations"></a>İzin verme veya davetleri B2B kullanıcıları belirli kuruluşlardan engelleme

**Türü:** yeni özellik  
**Hizmet kategorisi:** B2B  
**Ürün özelliği:** B2B/B2C
 

Paylaşın ve Azure AD B2B işbirliği birlikte çalışmak istediğiniz hangi iş ortağı kuruluşlar artık belirtebilirsiniz. Bunu yapmak için özel bir listesini oluşturmak seçebileceğiniz izin ver veya Reddet etki alanları. Bu özellikleri kullanılarak bir etki alanı engellendiğinde, çalışanlar artık bu etki alanında davetleri kişilere gönderebilirsiniz.

Bu, onaylanan kullanıcılar için sorunsuz bir deneyim etkinleştirirken, kaynaklarınıza erişimi denetlemenize yardımcı olur.

B2B işbirliği bu özellik tüm Azure Active Directory müşterileri tarafından kullanılabilir ve koşullu erişim ve kimlik koruma gibi Azure AD Premium özellikleri ile birlikte daha ayrıntılı denetim için zaman ve dış iş kullanıcılar oturum nasıl kullanılabilir içinde ve erişim.

Daha fazla bilgi için [B2B kullanıcıları için izin verilenler veya Engellenenler davetleri belirli kuruluşlardan](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-allow-deny-list).

---
 
### <a name="new-federated-apps-available-in-azure-ad-app-gallery"></a>Yeni Federasyon uygulamaları kullanılabilir Azure AD uygulama galerisinde

**Türü:** yeni özellik  
**Hizmet kategorisi:** kurumsal uygulamalar  
**Ürün özelliği:** 3. taraf tümleştirme

Nisan 2018'de uygulama galerimizden Federasyon ile 13 bu yeni uygulamaları desteği ekledik:

Ölçüt HCM, [FiscalNote](https://docs.microsoft.com/azure/active-directory/active-directory-saas-fiscalnote-tutorial), [gizli sunucusu (şirket içi)](https://docs.microsoft.com/azure/active-directory/active-directory-saas-secretserver-on-premises-tutorial), [dinamik sinyal](https://docs.microsoft.com/azure/active-directory/active-directory-saas-dynamicsignal-tutorial), [mindWireless](https://docs.microsoft.com/azure/active-directory/active-directory-saas-mindwireless-tutorial), [Kuruluş Şeması Artık](https://docs.microsoft.com/azure/active-directory/active-directory-saas-orgchartnow-tutorial), [Ziflow](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ziflow-tutorial), [AppNeta Performans İzleyicisi'ni](https://docs.microsoft.com/azure/active-directory/active-directory-saas-appneta-tutorial), [Elium](https://docs.microsoft.com/azure/active-directory/active-directory-saas-elium-tutorial) , [Fluxx Labs](https://docs.microsoft.com/azure/active-directory/active-directory-saas-fluxxlabs-tutorial), [ Cisco bulut](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ciscocloud-tutorial), raf, [SafetyNet](https://docs.microsoft.com/azure/active-directory/active-directory-saas-safetynet-tutorial)

Uygulamalar hakkında daha fazla bilgi için bkz. [Azure Active Directory ile SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial).

Azure AD uygulama galerisinde uygulamanızı listeleme hakkında daha fazla bilgi için bkz. [uygulamanızı Azure Active Directory Uygulama galerisinde listeleyin](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing).

---
 
### <a name="grant-b2b-users-in-azure-ad-access-to-your-on-premises-applications-public-preview"></a>Şirket içi uygulamalarınıza (genel Önizleme) Azure AD erişim verme B2B kullanıcıları

**Türü:** yeni özellik  
**Hizmet kategorisi:** B2B  
**Ürün özelliği:** B2B/B2C

Azure ad iş ortağı kuruluşlardan Konuk kullanıcıları davet etmek için Azure Active Directory (Azure AD) B2B işbirliği özelliklerini kullanan bir kuruluş, artık bu B2B kullanıcıları şirket içi uygulamalara erişim sağlayabilir. Bu şirket içi uygulamaları, Kerberos Kısıtlı temsilci (KCD) ile SAML tabanlı kimlik doğrulaması veya tümleşik Windows kimlik doğrulaması (IWA) kullanabilirsiniz.

Daha fazla bilgi için [verme B2B kullanıcıları Azure AD'de, şirket içi uygulamalarınıza erişim](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-hybrid-cloud-to-on-premises).

---
 
### <a name="get-sso-integration-tutorials-from-the-azure-marketplace"></a>Azure Market'ten SSO tümleştirme öğreticilerine erişin

**Türü:** değiştirilen özellik  
**Hizmet kategorisi:** diğer  
**Ürün özelliği:** 3. taraf tümleştirme

Listelenen bir uygulama bildirimi [Azure Market](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps?page=1) destekleyen SAML tabanlı çoklu tıklayarak oturum, **şimdi edinin** bu uygulamayla ilişkili tümleştirme öğreticisiyle sağlar. 

---

### <a name="faster-performance-of-azure-ad-automatic-user-provisioning-to-saas-applications"></a>Azure AD SaaS uygulamaları için otomatik kullanıcı hazırlama, daha hızlı performans

**Türü:** değiştirilen özellik  
**Hizmet kategorisi:** uygulama sağlama  
**Ürün özelliği:** 3. taraf tümleştirme
 
Daha önce Azure AD kiracıları 100. 000'den birleşik kullanıcılar içeriyorsa sağlama bağlayıcılar (örneğin Salesforce, ServiceNow ve kutusu) SaaS uygulamaları için Azure Active Directory kullanıcı kullanan müşteriler yavaş performans karşılaşması ve Gruplar ve hangi kullanıcıların sağlanan belirlemek için kullanıcı ve Grup atamaları kullanıyordu.

2 Nisan 2018'de, Azure Active Directory ve hedef SaaS uygulamaları arasında ilk eşitlemeler gerçekleştirmek için gereken süreyi büyük ölçüde azaltmak Azure AD sağlama hizmeti için önemli performans geliştirmeleri dağıtıldı.

Sonuç olarak, birçok müşteri, ilk eşitleme geçen gün sayısı veya hiçbir zaman tamamlandı, artık yalnızca birkaç dakika veya saat içinde tamamlıyorsanız uygulamalara vardı.

Daha fazla bilgi için [sağlama sırasında ne olur?](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning#what-happens-during-provisioning)

---

### <a name="self-service-password-reset-from-windows-10-lock-screen-for-hybrid-azure-ad-joined-machines"></a>Self Servis parola sıfırlama için hibrit Azure AD'ye Windows 10 kilit ekranından makineler katılmış

**Türü:** değiştirilen özellik  
**Hizmet kategorisi:** Self Servis parola sıfırlama  
**Ürün özelliği:** kullanıcı kimlik doğrulaması
 
Hibrit Azure AD'ye katılmış olan makineler için destek eklemek için Windows 10 SSPR özelliğini güncelleştirdik. Bu özellik kullanılabilir Windows 10 RS4 bir Windows 10 makinenin kilit ekranında parolalarını sıfırlamalarına olanak tanır. Etkin ve Self Servis parola sıfırlama için kaydolan kullanıcılar, bu özelliği kullanabilir.

Daha fazla bilgi için [Azure AD parola sıfırlama oturum açma ekranından](https://docs.microsoft.com/azure/active-directory/authentication/tutorial-sspr-windows).

---

## <a name="march-2018"></a>Mart 2018
 
### <a name="certificate-expire-notification"></a>Sertifika sona bildirimi

**Türü:** düzeltildi  
**Hizmet kategorisi:** kurumsal uygulamalar  
**Ürün özelliği:** SSO
 
Galeri dışı uygulama dolmak üzere olduğu ya da Azure AD galeri için bir sertifika olduğunda bir bildirim gönderir. 

Bazı kullanıcılar, SAML tabanlı çoklu oturum açma için yapılandırılmış kurumsal uygulamalar için bildirim almadı. Bu sorun çözüldü. Azure AD 7, 30 ve 60 gün içinde sona erecek sertifikaları için bildirim gönderir. Bu olay denetim günlüklerinde görebilirsiniz. 

Daha fazla bilgi için bkz.

- [Federasyon çoklu oturum açma için Azure Active Directory'de sertifikaları yönetme](https://docs.microsoft.com/azure/active-directory/active-directory-sso-certs)
- [Azure Active Directory portalındaki denetim etkinlik raporları](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs)
 
---
 
### <a name="twitter-and-github-identity-providers-in-azure-ad-b2c"></a>Azure AD B2C, twitter ve GitHub kimlik sağlayıcıları

**Türü:** yeni özellik  
**Hizmet kategorisi:** B2C - tüketici Kimlik Yönetimi  
**Ürün özelliği:** B2B/B2C
 
Artık, bir Azure AD B2C kimlik sağlayıcısı olarak Twitter veya GitHub ekleyebilirsiniz. Twitter büyüyecek için genel preview'dan taşınıyor GitHub genel önizlemede yayınlanır.

Daha fazla bilgi için [Azure AD B2B işbirliği nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b).
 
---

### <a name="restrict-browser-access-using-intune-managed-browser-with-azure-ad-application-based-conditional-access-for-ios-and-android"></a>Azure AD uygulama tabanlı koşullu erişimle, iOS ve Android için Intune Managed Browser kullanarak tarayıcı erişimi kısıtlama

**Türü:** yeni özellik  
**Hizmet kategorisi:** koşullu erişim  
**Ürün özelliği:** kimlik güvenliği ve koruması
 
**Artık genel önizlemede!**

**Intune Managed Browser'da SSO:** çalışanlarınızın çoklu oturum açma (Microsoft Outlook gibi) için yerel istemcileri ve Intune Managed Browser tüm Azure AD bağlantılı uygulamalar için kullanabilirsiniz.

**Intune Managed Browser koşullu erişim desteği:** artık uygulama tabanlı koşullu erişim ilkelerini kullanarak Intune Managed browser'ı kullanmak çalışanların gerektirebilir.

Bu konuda hakkında daha fazla bilgiyi bizim [blog gönderisi](https://cloudblogs.microsoft.com/enterprisemobility/2018/03/15/the-intune-managed-browser-now-supports-azure-ad-sso-and-conditional-access/).

Daha fazla bilgi için bkz.

- [Uygulama tabanlı koşullu erişim Kurulumu](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

- [Yönetilen tarayıcı ilkelerini yapılandırma](https://aka.ms/managedbrowser)  

---
 
### <a name="app-proxy-cmdlets-in-powershell-ga-module"></a>Uygulama proxy'si GA Powershell modülündeki cmdlet'leri

**Türü:** yeni özellik  
**Hizmet kategorisi:** uygulama ara sunucusu  
**Ürün özelliği:** erişim denetimi
 
Uygulama proxy'si cmdlet'leri için desteği Powershell GA modülünde sunuldu! Bu Powershell modülleri - daha fazla bir yıl, bazı cmdlet'ler hale gelirse çalışmayı durdurabilir güncel gerektirir. 

Daha fazla bilgi için [AzureAD](https://docs.microsoft.com/powershell/module/Azuread/?view=azureadps-2.0).
 
---
 
### <a name="office-365-native-clients-are-supported-by-seamless-sso-using-a-non-interactive-protocol"></a>Office 365 için yerel istemcileri tarafından etkileşimli olmayan bir protokol kullanarak sorunsuz SSO desteklenir

**Türü:** yeni özellik  
**Hizmet kategorisi:** kimlik doğrulamaları (oturum açma bilgileri)  
**Ürün özelliği:** kullanıcı kimlik doğrulaması
 
Office 365 yerel istemci kullanarak kullanıcı (sürüm 16.0.8730.xxxx ve üstü) sorunsuz çoklu oturum açma kullanarak sessiz oturum açma deneyimi alın. Bu destek, Azure AD'ye etkileşimli olmayan bir protokol (WS-Trust) ekleyerek sağlanır.

Daha fazla bilgi için [nasıl sorunsuz çoklu oturum açma çalışma ile yerel bir istemcide oturum?](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso-how-it-works#how-does-sign-in-on-a-native-client-with-seamless-sso-work)
 
---

### <a name="users-get-a-silent-sign-on-experience-with-seamless-sso-if-an-application-sends-sign-in-requests-to-azure-ads-tenant-endpoints"></a>Bir uygulamayı Azure AD'nin Kiracı uç noktalar için oturum açma isteği gönderirse bir sessiz oturum açma deneyimini sorunsuz SSO ile kullanıcılar Al

**Türü:** yeni özellik  
**Hizmet kategorisi:** kimlik doğrulamaları (oturum açma bilgileri)  
**Ürün özelliği:** kullanıcı kimlik doğrulaması
 
Kullanıcılar bir sessiz oturum açma deneyimini sorunsuz SSO ile bir uygulama bildirimi alır (örneğin, `https://contoso.sharepoint.com`) oturum açma istekleri için Azure AD'nin Kiracı uç noktası; diğer bir deyişle, gönderir `https://login.microsoftonline.com/contoso.com/<..>` veya `https://login.microsoftonline.com/<tenant_ID>/<..>` - Azure AD'nin ortak uç nokta yerine (`https://login.microsoftonline.com/common/<...>`).

Daha fazla bilgi için [Azure Active Directory sorunsuz çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso). 

---
 
### <a name="need-to-add-only-one-azure-ad-url-instead-of-two-urls-previously-to-users-intranet-zone-settings-to-roll-out-seamless-sso"></a>Sorunsuz çoklu oturum açma almak için kullanıcıların Intranet bölgesi ayarlarını daha önce iki URL yerine yalnızca bir Azure AD URL'sini eklemeniz gerekir

**Türü:** yeni özellik  
**Hizmet kategorisi:** kimlik doğrulamaları (oturum açma bilgileri)  
**Ürün özelliği:** kullanıcı kimlik doğrulaması
 
Kullanıcılarınız için sorunsuz SSO alma için yalnızca bir Azure AD URL kullanıcıların intranet bölgesi ayarlarını Active Directory'de Grup İlkesi'ni kullanarak eklemeniz gerekir: `https://autologon.microsoftazuread-sso.com`. Daha önce müşterilerin iki URL'si eklemek için zorunluluğundaydı.

Daha fazla bilgi için [Azure Active Directory sorunsuz çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso). 
 
---
 
### <a name="new-federated-apps-available-in-azure-ad-app-gallery"></a>Yeni Federasyon uygulamaları kullanılabilir Azure AD uygulama Galerisi

**Türü:** yeni özellik  
**Hizmet kategorisi:** kurumsal uygulamalar  
**Ürün özelliği:** 3. taraf tümleştirme

Mart 2018'de bu 15 yeni Federasyon uygulamalarla uygulama galerimizden desteği ekledik:

[Boxcryptor](https://docs.microsoft.com/azure/active-directory/active-directory-saas-boxcryptor-tutorial), [CylancePROTECT](https://docs.microsoft.com/azure/active-directory/active-directory-saas-cylanceprotect-tutorial), Wrike, [SignalFx](https://docs.microsoft.com/azure/active-directory/active-directory-saas-signalfx-tutorial), Yardımcısı tarafından FirstAgenda, [YardiOne](https://docs.microsoft.com/azure/active-directory/active-directory-saas-yardione-tutorial), Vtiger CRM, inwink, [Genliğe](https://docs.microsoft.com/azure/active-directory/active-directory-saas-amplitude-tutorial), [Spacio](https://docs.microsoft.com/azure/active-directory/active-directory-saas-spacio-tutorial), [ContractWorks](https://docs.microsoft.com/azure/active-directory/active-directory-saas-contractworks-tutorial), [Bersin](https://docs.microsoft.com/azure/active-directory/active-directory-saas-bersin-tutorial), [Mercell](https://docs.microsoft.com/azure/active-directory/active-directory-saas-mercell-tutorial), [Trisotech dijital Kuruluş Sunucusu](https://docs.microsoft.com/azure/active-directory/active-directory-saas-trisotechdigitalenterpriseserver-tutorial), [Qumu bulut](https://docs.microsoft.com/azure/active-directory/active-directory-saas-qumucloud-tutorial).
 
Uygulamalar hakkında daha fazla bilgi için bkz. [Azure Active Directory ile SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial).

Azure AD uygulama galerisinde uygulamanızı listeleme hakkında daha fazla bilgi için bkz. [uygulamanızı Azure Active Directory Uygulama galerisinde listeleyin](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing). 

---
 
### <a name="pim-for-azure-resources-is-generally-available"></a>Azure kaynakları için PIM kullanıma sunuldu

**Türü:** yeni özellik  
**Hizmet kategorisi:** Privileged Identity Management  
**Ürün özelliği:** Privileged Identity Management
 
Dizin rolleri için Azure AD Privileged Identity Management kullanıyorsanız, artık PIM'ın süresi sınırlı erişim ve atama özellikleri abonelik, kaynak grupları, sanal makineler ve desteklenen herhangi bir kaynak gibi Azure kaynak rolleri için kullanabilirsiniz Azure Resource Manager tarafından. Just-ın-Time rolleri etkinleştirirken multi-Factor Authentication yürürlüğe ve onaylanan değişiklik windows ile koordinasyon halinde etkinleştirmeleri zamanlayabilirsiniz. Ayrıca, bu sürüm geliştirmeleri güncelleştirilmiş bir kullanıcı Arabirimi, onay iş akışları ve özelliği yakında sona erecek rolleri genişletmek ve süresi dolan rolleri yenileme dahil olmak üzere genel Önizleme sırasında kullanılabilir değil ekler.

Daha fazla bilgi için [(Önizleme) Azure kaynakları için PIM](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac)
 
---
 
### <a name="adding-optional-claims-to-your-apps-tokens-public-preview"></a>Uygulamaları belirteçlerinizi (genel Önizleme) için isteğe bağlı bir talep ekleme

**Türü:** yeni özellik  
**Hizmet kategorisi:** kimlik doğrulamaları (oturum açma bilgileri)  
**Ürün özelliği:** kullanıcı kimlik doğrulaması
 
Özel veya isteğe bağlı isteği Jwt'ler veya SAML belirteçlerini talep artık Azure AD uygulamanızı geçirebilirsiniz.  Bu, varsayılan boyutu veya Uygulanabilirlik kısıtlamaları nedeniyle bir belirteç olarak bulunmayan bir kullanıcı ya da Kiracı hakkında taleplerdir.  Şu anda genel önizlemede Azure AD uygulamaları v1.0 ve v2.0 uç noktalarda budur.  Bilgi için bkz. hangi talepleri temel eklenebilir ve bunları istemek için uygulama bildirimini düzenleme.  

Daha fazla bilgi için [isteğe bağlı Azure AD'de talep](https://docs.microsoft.com/azure/active-directory/develop/active-directory-optional-claims).
 
---
 
### <a name="azure-ad-supports-pkce-for-more-secure-oauth-flows"></a>Azure AD PKCE daha güvenli OAuth akışlar için destekler.

**Türü:** yeni özellik  
**Hizmet kategorisi:** kimlik doğrulamaları (oturum açma bilgileri)  
**Ürün özelliği:** kullanıcı kimlik doğrulaması
 
Azure AD belgeleri, OAuth 2.0 yetkilendirme kodu verme akışı sırasında daha güvenli iletişime izin veren PKCE desteği Not güncelleştirildi.  S256 hem düz metin code_challenges v1.0 ve v2.0 Uç noktalara desteklenir. 

Daha fazla bilgi için [bir yetkilendirme kodu istek](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-protocols-oauth-code#request-an-authorization-code). 
 
---
 
### <a name="support-for-provisioning-all-user-attribute-values-available-in-the-workday-getworkers-api"></a>Tüm kullanıcı öznitelik değerleri Workday Get_Workers API'SİNDE kullanılabilir sağlama desteği

**Türü:** yeni özellik  
**Hizmet kategorisi:** uygulama sağlama  
**Ürün özelliği:** 3. taraf tümleştirme
 
Gelen Workday'den Active Directory artık Azure AD için sağlama ve genel Önizleme çıkarma ve sağlama Workday Get_Workers API'SİNDE kullanılabilir tüm öznitelik değerleri destekler. Bu ek standart yüzlerce destekler ekler ve ilk sürümü Workday ile birlikte gelen olanlar dışında özel öznitelikler gelen bağlayıcı sağlama.

Daha fazla bilgi için bkz: [Workday kullanıcı özniteliklerinin listesi özelleştirme](https://docs.microsoft.com/azure/active-directory/active-directory-saas-workday-inbound-tutorial#customizing-the-list-of-workday-user-attributes)

---

### <a name="changing-group-membership-from-dynamic-to-static-and-vice-versa"></a>Grup üyeliğini dinamik olan statik ve tersi değiştirme

**Türü:** yeni özellik  
**Hizmet kategorisi:** Grup Yönetimi  
**Ürün özelliği:** işbirliği
 
Üyelik bir gruba nasıl yönetilir değiştirmek mümkündür. Bu gruba varolan herhangi bir başvuruyu hala geçerli olduğundan sistemde aynı grubu adını ve Kimliğini tutmak istediğinizde kullanışlıdır. Yeni grup oluşturma, bu başvuruları güncelleştirilmesi gerekir.
Bu işlevselliği desteklemek için Azure AD yönetim merkezini güncelleştirdik. Şimdi, müşterilerin var olan grupları dinamik üyelik atanan üyelik ve tersi dönüştürebilirsiniz. Mevcut PowerShell cmdlet'leri de yine kullanılabilir durumdadır.

Daha fazla bilgi için [statik ve tersi için dinamik üyeliği değiştirme](https://docs.microsoft.com/azure/active-directory/active-directory-groups-dynamic-membership-azure-portal#changing-dynamic-membership-to-static-and-vice-versa)

---

### <a name="improved-sign-out-behavior-with-seamless-sso"></a>Sorunsuz çoklu oturum açma ile geliştirilmiş oturum kapatma davranışı

**Türü:** değiştirilen özellik  
**Hizmet kategorisi:** kimlik doğrulamaları (oturum açma bilgileri)  
**Ürün özelliği:** kullanıcı kimlik doğrulaması
 
Kullanıcılar, Azure AD tarafından güvenli hale getirilmiş bir uygulamaya dışında açıkça imzalanmış olsa bile, daha önce bunlar otomatik olarak etki alanına katılmış cihazlarından bir Azure AD uygulaması, corpnet içinde yeniden erişmeye çalışıyorsanız geri sorunsuz SSO kullanarak oturum açmış olmanız. Bu değişiklik, oturumunuzu desteklenir.  Bu aynı veya farklı Azure seçmelerini sağlar geri, otomatik olarak sorunsuz SSO kullanarak oturum açmış yerine oturum açmak için bir AD hesabı.

Daha fazla bilgi için [Azure Active Directory sorunsuz çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso)
 
---
 
### <a name="application-proxy-connector-version-154020-released"></a>Uygulama Proxy Bağlayıcısı sürümü yayımlanan 1.5.402.0

**Türü:** değiştirilen özellik  
**Hizmet kategorisi:** uygulama ara sunucusu  
**Ürün özelliği:** kimlik güvenliği ve koruması
 
Bu bağlayıcı sürüm kademeli olarak Kasım kullanıma sunulacaktır. Bu yeni bağlayıcı sürüm, aşağıdaki değişiklikleri içerir:

- Bağlayıcı artık etki alanı düzeyi tanımlama bilgilerini bunun yerine alt etki alanı düzeyini ayarlar. Bu daha sorunsuz SSO bir deneyim sağlar ve yedek kimlik doğrulama istemleri önler.
- Öbekli kodlama istekleri için destek
- Geliştirilmiş bağlayıcı sistem durumu izleme 
- Birçok hata düzeltmesi ve kararlılık geliştirmeleri

Daha fazla bilgi için [anlamak Azure AD uygulama ara sunucusu bağlayıcıları](https://docs.microsoft.com/azure/active-directory/application-proxy-understand-connectors).
 
---

## <a name="february-2018"></a>Şubat 2018
 
### <a name="improved-navigation-for-managing-users-and-groups"></a>Kullanıcılar ve grupları yönetmek için Gezinti geliştirildi

**Türü:** değişiklik planı  
**Hizmet kategorisi:** dizin yönetimi  
**Ürün özelliği:** dizini

Kullanıcılar ve grupları yönetmek için gezinti deneyimini basitleştirilmiştir. Artık dizine genel bakış'tan doğrudan silinen kullanıcıların listesine daha kolay erişimi olan tüm kullanıcıların listesini gidebilirsiniz. Grup Yönetimi ayarlarına daha kolay erişim ile tüm grupların listesine doğrudan dizin genel bakış'tan da gidebilirsiniz. Ve ayrıca dizine genel bakış sayfasından bir kullanıcı, Grup, Kurumsal uygulama veya uygulama kaydını arayabilirsiniz. 

---

### <a name="availability-of-sign-ins-and-audit-reports-in-microsoft-azure-operated-by-21vianet-azure-china-21vianet"></a>Oturum açma işlemleri ve denetim kullanılabilirliğini raporlar 21Vianet tarafından işletilen Microsoft azure'da (Azure China 21Vianet)

**Türü:** yeni özellik  
**Hizmet kategorisi:** Azure Stack  
**Ürün özelliği:** izleme ve Raporlama

Azure AD etkinlik günlük raporlar tarafından (Azure China 21Vianet) örnekleri 21Vianet tarafından işletilen Microsoft azure'da kullanıma sunulmuştur. Aşağıdaki günlüklere dahil edilir:

- **Oturum açma işlemleri etkinlik günlüklerini** -tüm oturum açma işlemleri içerir, Kiracı ile ilişkilendirilen günlükleri.

- **Self Servis parola denetim günlükleri** -SSPR denetim günlükleri içerir.

- **Dizin Yönetimi denetim günlüklerini** -kullanıcı yönetimi, uygulama yönetimi ve diğerleri gibi tüm dizin yönetimiyle ilgili denetim günlükleri içerir.

Bu günlükleri ile ortamınızın nasıl çalıştığını içine öngörü sahibi olabilir. Sağlanan verilerle:

- Uygulama ve hizmetlerinizin kullanıcılarınız tarafından nasıl kullanıldığını saptayabilirsiniz.

- Kullanıcılarınızın işlerini yapmalarını engelleyen sorunları giderin.

Bu raporların nasıl kullanılacağı hakkında daha fazla bilgi için bkz. [Azure Active Directory raporlama](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal).

---

### <a name="use-report-reader-role-non-admin-role-to-view-azure-ad-activity-reports"></a>Azure AD Etkinlik raporlarını görüntülemek için "Rapor okuyucu" rolünü (yönetici olmayan rol) kullanın

**Türü:** yeni özellik  
**Hizmet kategorisi:** raporlama  
**Ürün özelliği:** izleme ve Raporlama

Yönetici olmayan rollerin Azure AD etkinlik erişmesini etkinleştirmek için müşterilerin geri bildirim parçası günlükleri gibi erişim oturum açma ve Azure portalı yanı sıra Graph Apı'lerimizi kullanarak içinde denetim etkinliği için "Rapor okuyucu" rolündeki kullanıcılar için özelliğini etkinleştirdik. 

Daha fazla bilgi için bu raporları kullanmak üzere nasıl [Azure Active Directory raporlama](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal). 

---

### <a name="employeeid-claim-available-as-user-attribute-and-user-identifier"></a>Kullanıcı özniteliği ve kullanıcı tanımlayıcısı olarak kullanılabilir EmployeeID talebi

**Türü:** yeni özellik  
**Hizmet kategorisi:** kurumsal uygulamalar  
**Ürün özelliği:** SSO
 
Yapılandırabileceğiniz **EmployeeID** üye kullanıcılar ve B2B Konukları uygulamalarda Kurumsal Uygulama UI'dan SAML tabanlı oturum açma için kullanıcı özniteliği ve kullanıcı tanımlayıcısı olarak.

Daha fazla bilgi için [Azure Active Directory'de kurumsal uygulamalar için SAML belirtecinde verilen talepleri özelleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization).

---

### <a name="simplified-application-management-using-wildcards-in-azure-ad-application-proxy"></a>Azure AD uygulama ara Sunucusu'nda joker kullanarak Basitleştirilmiş uygulama yönetimi

**Türü:** yeni özellik  
**Hizmet kategorisi:** uygulama ara sunucusu  
**Ürün özelliği:** kullanıcı kimlik doğrulaması
 
Uygulama dağıtımını kolaylaştırın ve yönetim yükünüzü azaltmak için artık joker karakter kullanan uygulamalar yayımlama olanağı desteklenmektedir. Bir joker uygulama yayımlamak için standart bir uygulama yayımlama akışını takip edin, ancak iç ve dış URL'lerinde bir joker karakter kullanın.

Daha fazla bilgi için [joker karakteri uygulamalarında Azure Active Directory Uygulama proxy'si](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-wildcard)

---

### <a name="new-cmdlets-to-support-configuration-of-application-proxy"></a>Uygulama Ara Sunucusu'nun yapılandırılmasını desteklemeye yönelik yeni cmdlet'ler

**Türü:** yeni özellik  
**Hizmet kategorisi:** uygulama ara sunucusu  
**Ürün özelliği:** platformu

AzureAD PowerShell Önizleme modülünün en son sürümü, müşterilerin PowerShell kullanarak uygulama Proxy uygulamaları yapılandırmanıza imkan tanıyan yeni cmdlet'ler içerir.

Yeni cmdlet'ler şunlardır: 

- Get-AzureADApplicationProxyApplication
- Get-AzureADApplicationProxyApplicationConnectorGroup
- Get-AzureADApplicationProxyConnector
- Get-AzureADApplicationProxyConnectorGroup
- Get-AzureADApplicationProxyConnectorGroupMembers
- Get-AzureADApplicationProxyConnectorMemberOf
- New-AzureADApplicationProxyApplication
- New-AzureADApplicationProxyConnectorGroup
- Remove-AzureADApplicationProxyApplication
- Remove-AzureADApplicationProxyApplicationConnectorGroup
- Remove-AzureADApplicationProxyConnectorGroup
- Set-AzureADApplicationProxyApplication
- Set-AzureADApplicationProxyApplicationConnectorGroup
- Set-AzureADApplicationProxyApplicationCustomDomainCertificate
- Set-AzureADApplicationProxyApplicationSingleSignOn
- Set-AzureADApplicationProxyConnector
- Set-AzureADApplicationProxyConnectorGroup

---
 
### <a name="new-cmdlets-to-support-configuration-of-groups"></a>Grupların yapılandırılmasını desteklemeye yönelik yeni cmdlet'ler

**Türü:** yeni özellik  
**Hizmet kategorisi:** uygulama ara sunucusu  
**Ürün özelliği:** platformu

AzureAD PowerShell modülünün en son sürümü Azure AD'de grupları yönetmek için cmdlet'leri içerir. Bu cmdlet'ler, daha önce AzureADPreview modülünde kullanılabilen ve şimdi AzureAD modülüne eklendi

Şimdi genel kullanılabilirlik sürümü olan Grup cmdlet'ler şunlardır: 

- Get-AzureADMSGroup
- New-AzureADMSGroup
- Remove-AzureADMSGroup
- Set-AzureADMSGroup
- Get-AzureADMSGroupLifecyclePolicy
- Yeni-AzureADMSGroupLifecyclePolicy
- Remove-AzureADMSGroupLifecyclePolicy
- Ekle-AzureADMSLifecyclePolicyGroup
- Remove-AzureADMSLifecyclePolicyGroup
- Reset-AzureADMSLifeCycleGroup   
- Get-AzureADMSLifecyclePolicyGroup

---
 
### <a name="a-new-release-of-azure-ad-connect-is-available"></a>Azure AD Connect'in yeni bir sürüm kullanılabilir

**Türü:** yeni özellik  
**Hizmet kategorisi:** AD eşitleme  
**Ürün özelliği:** platformu
 
Azure AD Connect, Azure AD arasında ve Windows Server Active Directory ve LDAP dahil olmak üzere şirket içi veri kaynakları verileri eşitlemek için tercih edilen bir araçtır.

>[!Important]
>Bu derleme şema ve eşitleme tanıtır kural değişiklikler. Azure AD Connect eşitleme hizmeti bir yükseltmeden sonra bir tam içeri aktarma ve tam eşitleme adımları tetikler. Bu davranışı değiştirmek konusunda daha fazla bilgi için bkz: [yükseltmeden sonra tam eşitleme erteleneceği nasıl](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-upgrade-previous-version#how-to-defer-full-synchronization-after-upgrade).

Bu sürüm aşağıdaki güncelleştirmeleri ve değişiklikleri sahiptir:

**Giderilen sorunlar**

- Sonraki sayfaya geçiş yaparken bölüm filtreleme sayfası için arka plan görevleri zamanlama penceresi düzeltmesi.

- Yapılandırmaveritabanı özel eylemi sırasında erişim ihlaline neden bir hata düzeltildi.

- SQL bağlantı zaman aşımı kurtarmak için bir hata düzeltildi.

- Burada SAN joker karakterler sertifikalarla Önkoşul denetimi başarısız bir hata düzeltildi.

- AAD bağlayıcı dışarı aktarma sırasında miiserver.exe kilitlenmeye neden olan bir hata düzeltildi.

- DC üzerinde çalışan AAD neden olduğunda günlüğe bir hatalı parola denemesi yapılandırmasını değiştirmek için Sihirbazı eriştikleri bir hata düzeltildi

**Yeni özellikler ve geliştirmeler**
 
- Uygulama telemetrisini - Yöneticiler, bu sınıf Aç/Kapat verilerinin geçiş yapabilirsiniz.

- Azure AD sistem durumu verilerini - yöneticiler kendi sistem durumu ayarlarını denetlemek için sistem durumu portalı ziyaret etmeniz gerekir. Hizmet İlkesi değiştirildi sonra aracılar okuyun ve onu zorla.

- Cihaz geri yazmayı yapılandırma eylemleri ve sayfa başlatma için bir ilerleme çubuğu ekledik.

- Geliştirilmiş genel tanılama HTML rapor ve tam veri toplama ZIP metin / HTML raporu.

- Geliştirilmiş otomatik güvenilirliğini yükseltin ve sunucusunun sistem durumunu belirlenebilir emin olmak için ek telemetri eklendi.

- Kullanılabilir AD Bağlayıcısı hesabındaki ayrıcalıklı hesaplara kısıtlayın. Yeni yüklemeler için sihirbazın ayrıcalıklı hesapları izinleri kısıtlar MSOL hesabı oluşturduktan sonra MSOL hesabına sahip. Değişiklikler express yüklemeleri ve otomatik olarak oluşturma hesabıyla özel yüklemeleri etkiler.

- Yükleyici SA ayrıcalık AADConnect temiz yüklemesini zorunlu tutulmayacak şekilde değiştirildi.

- Belirli bir nesnesi için eşitleme sorunlarını gidermek için yeni yardımcı program'ı seçin. Şu anda aşağıdakiler için yardımcı program denetler:

    - Azure AD kiracısında kullanıcı hesabı ile eşitlenmiş kullanıcı nesnesi arasındaki UserPrincipalName uyuşmazlığı.
  
    - Nesne filtreleme etki alanı nedeniyle eşitleme filtrelediyseniz
  
    - Nesne eşitleme filtreleme kuruluş birimi (OU) nedeniyle filtrelediyseniz

- Belirli bir kullanıcı hesabı için şirket içi Active Directory içinde depolanan geçerli parola karması eşitleme için yeni yardımcı program'ı seçin. Yardımcı programı, parola değişikliği gerektirmez. 

---
 
### <a name="applications-supporting-intune-app-protection-policies-added-for-use-with-azure-ad-application-based-conditional-access"></a>Azure AD uygulama tabanlı koşullu erişim ile destek Intune uygulama koruma ilkeleri için eklenen uygulamalar kullanın

**Türü:** değiştirilen özellik  
**Hizmet kategorisi:** koşullu erişim  
**Ürün özelliği:** kimlik güvenliği ve koruması

Uygulama tabanlı koşullu erişimi destekleyen daha fazla uygulama ekledik. Artık, Office 365 ve bu onaylı istemci uygulamalarını kullanarak diğer Azure AD bağlantılı bulut uygulamalarınıza erişimi alabilirsiniz.

Aşağıdaki uygulamalar Şubat sonuna eklenecek:

- Microsoft Power BI

- Microsoft Başlatıcısı

- Microsoft faturalama

Daha fazla bilgi için bkz.

- [Onaylı istemci uygulama gereksinimi](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement)
- [Azure AD uygulama tabanlı koşullu erişim](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

---

### <a name="terms-of-use-update-to-mobile-experience"></a>Mobil deneyimde kullanım koşulları güncelleştirmesi 

**Türü:** değiştirilen özellik  
**Hizmet kategorisi:** kullanım koşulları  
**Ürün özelliği:** uyumluluk

Kullanım koşulları görüntülendiğinde artık tıklayabilirsiniz **görüntüleme konusunda sorun mu yaşıyorsunuz? Buraya**. Bu bağlantıya tıkladığınızda kullanım Cihazınızda yerel olarak açılır. Belgenin yazı tipi boyutu veya cihazın ekran boyutu ne olursa olsun yakınlaştırma ve gerektiğinde belgeyi okuyalım. 

---
 
## <a name="january-2018"></a>Ocak 2018
 
### <a name="new-federated-apps-available-in-azure-ad-app-gallery"></a>Yeni Federasyon uygulamaları kullanılabilir Azure AD uygulama Galerisi 

**Türü:** yeni özellik  
**Hizmet kategorisi:** kurumsal uygulamalar  
**Ürün özelliği:** 3. taraf tümleştirme

Ocak 2018'de app Galerisi'nde aşağıdaki yeni uygulamaları ile Federasyonu Desteği eklendi:

[IBM OpenPages](https://go.microsoft.com/fwlink/?linkid=864698), [OneTrust gizlilik yönetim yazılımı](https://go.microsoft.com/fwlink/?linkid=861660), [Dealpath](https://go.microsoft.com/fwlink/?linkid=863526), [IriusRisk Federasyon dizin ve [uygunluk NetBenefits](https://go.microsoft.com/fwlink/?linkid=864701).

Uygulamalar hakkında daha fazla bilgi için bkz. [Azure Active Directory ile SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial).

Azure AD uygulama galerisinde uygulamanızı listeleme hakkında daha fazla bilgi için bkz. [uygulamanızı Azure Active Directory Uygulama galerisinde listeleyin](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing). 

---
 
### <a name="sign-in-with-additional-risk-detected"></a>Ek risk algılandı bilgilerinizle oturum açın

**Türü:** yeni özellik  
**Hizmet kategorisi:** kimlik koruması  
**Ürün özelliği:** kimlik güvenliği ve koruması

Algılanan risk olayı için alma öngörü için Azure AD aboneliğiniz bağlıdır. Azure AD Premium P2 sürümü ile temel alınan tüm algılamalar hakkında en ayrıntılı bilgileri alın.

Azure AD Premium P1 edition ile lisansınızı tarafından kapsanmaz algılamalar algılanan ek risk içeren oturum açma risk olayı olarak görünür.

Daha fazla bilgi için bkz. [Azure Active Directory risk olayları](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-risk-events).
 
---

### <a name="hide-office-365-applications-from-end-users-access-panels"></a>Son kullanıcı erişim panellerinde Office 365 uygulamalarından gizleme

**Türü:** yeni özellik  
**Hizmet kategorisi:** uygulamalarım  
**Ürün özelliği:** SSO

Artık, nasıl Office 365 uygulamaları üzerinde yeni bir kullanıcı ayarı ile kullanıcı erişim panellerinde görünmesi daha iyi yönetebilirsiniz. Bu seçenek Office uygulamalarını yalnızca Office Portalı'nda gösterilecek tercih ederseniz bir kullanıcının erişim panellerinde uygulamaları sayısını azaltmak için yararlıdır. Ayar bulunan **kullanıcı ayarları** ve olarak etiketlenen **kullanıcılar yalnızca Office 365 uygulamaları Office 365 portalında görebilir**.

Daha fazla bilgi için [bir uygulamadan Azure Active Directory'de kullanıcı deneyimini Gizle](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-hide-third-party-app).

---
 
### <a name="seamless-sign-into-apps-enabled-for-password-sso-directly-from-apps-url"></a>Uygulamanın URL'sini doğrudan parola SSO için etkinleştirildiği uygulamalar sorunsuz oturum 

**Türü:** yeni özellik  
**Hizmet kategorisi:** uygulamalarım  
**Ürün özelliği:** SSO

Uygulamalarım tarayıcı uzantısı aracılığıyla uygulamalarım çoklu oturum tarayıcınızda bir kısayol olarak özellik hakkında size uygun bir aracı kullanıma sunuldu. Kullanıcının yükledikten sonra tarayıcılarında bunları uygulamalarına hızlı erişim sağlayan bir waffle menüsü simgesi görürsünüz. Kullanıcılar artık yararlanabilirsiniz:

- Özelliği doğrudan uygulamanın oturum açma sayfasında parola tabanlı SSO uygulamaları için oturum açın
- Hızlı arama özelliğini kullanarak herhangi bir uygulama başlatın
- Uzantı son kullanılan uygulamalar için kısayollar
- Uzantı, Edge, Chrome ve Firefox için kullanılabilir.
 
Daha fazla bilgi için [My Apps güvenli oturum açma uzantısı](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction#my-apps-secure-sign-in-extension).

---

### <a name="azure-ad-administration-experience-in-azure-classic-portal-has-been-retired"></a>Azure AD yönetim Klasik Azure portalı deneyiminde Çekildi

**Türü:** kullanım dışı   
**Hizmet kategorisi:** Azure AD  
**Ürün özelliği:** dizini

8 Ocak 2018'den itibaren Azure AD Yönetim'den itibaren Klasik Azure portalı deneyiminde Çekildi. Bu, Azure Klasik Portalı'nın kendisini kullanımdan kaldırma ile birlikte yapıldı. Gelecekte, kullanmanız gereken [Azure AD yönetim merkezini](https://aad.portal.azure.com) tüm, portal tabanlı yönetim için Azure ad.
 
---

### <a name="the-phonefactor-web-portal-has-been-retired"></a>PhoneFactor web portalı kullanımdan kaldırıldı

**Türü:** kullanım dışı  
**Hizmet kategorisi:** Azure AD  
**Ürün özelliği:** dizini
 
8 Ocak 2018'den itibaren PhoneFactor web portalı kullanımdan kaldırıldı. Bu portalı MFA sunucusunun yönetimi için kullanılan, ancak bu işlevleri portal.azure.com adresindeki Azure portalında taşınmıştır. 

MFA yapılandırma şu konumdadır: **Azure Active Directory \> MFA sunucusu**
 
---
 
### <a name="deprecate-azure-ad-reports"></a>Azure AD raporlar kullanımdan kaldırma

**Türü:** kullanım dışı  
**Hizmet kategorisi:** raporlama  
**Ürün özelliği:** kimlik yaşam döngüsü yönetimi  


Genel kullanılabilirlik yeni API'ler hem etkinlik ve güvenlik raporları için rapor API'leri kullanıma sunuldu ve yeni Azure Active Directory Yönetim Konsolu ile altında "/ reports" uç nokta son 31 Aralık 2017 itibariyle devre dışı bırakılmış.

**Kullanılabilir nedir?**

Yeni Yönetici Konsolu geçiş işleminin bir parçası olarak 2 yeni API'ler Azure AD etkinlik günlüklerini almak için kullanılabilir gerçekleştirdik. Yeni API'leri daha zengin filtreleme ve sıralama daha zengin denetim ve oturum açma etkinlikleri sağlamaya ek olarak işlevselliği sunmaktadır. Güvenlik raporları ile önceden kullanılabilen veri artık Microsoft Graph API'si kimlik koruması risk olayları aracılığıyla erişilebilir.

Daha fazla bilgi için bkz.

- [Azure Active Directory raporlama API'SİYLE çalışmaya başlama](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started-azure-portal)

- [Microsoft Graph ve Azure Active Directory kimlik koruması ile çalışmaya başlama](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection-graph-getting-started)

---

## <a name="december-2017"></a>Aralık 2017

### <a name="terms-of-use-in-the-access-panel"></a>Erişim panelinde kullanım koşulları

**Türü:** yeni özellik  
**Hizmet kategorisi:** kullanım koşulları  
**Ürün özelliği:** uyumluluk
 
Artık erişim Masası'na gidin ve daha önce kabul ettiğiniz kullanım koşullarını görüntüleyin.

Şu adımları uygulayın:

1. Git [MyApps portalında](https://myapps.microsoft.com)ve oturum açın.

2. Sağ üst köşede, adınızı seçerek ve ardından **profili** listeden. 

3. Üzerinde **profili**seçin **kullanım koşullarını gözden geçir**. 

4. Kullanım koşullarını gözden geçirebilir, kabul edildi. 

Daha fazla bilgi için [kullanım özelliği (Önizleme) Azure AD kullanım koşulları](https://docs.microsoft.com/azure/active-directory/active-directory-tou).
 
---
 
### <a name="new-azure-ad-sign-in-experience"></a>Yeni Azure AD oturum açma deneyimi

**Türü:** yeni özellik  
**Hizmet kategorisi:** Azure AD  
**Ürün özelliği:** kullanıcı kimlik doğrulaması
 
Azure AD ve Microsoft hesabı kimlik sistemi kullanıcı arabirimleri, tutarlı bir görünüm olduğuna şekilde tasarlanmıştır. Ayrıca, Azure AD oturum açma sayfasının kullanıcı adının ilk olarak, ikinci bir ekranda kimlik bilgisi tarafından izlenen toplar.

Daha fazla bilgi için [yeni Azure AD oturum açma deneyiminin genel önizlemeye sunuldu](https://cloudblogs.microsoft.com/enterprisemobility/2017/08/02/the-new-azure-ad-signin-experience-is-now-in-public-preview/).
 
---
 
### <a name="fewer-sign-in-prompts-a-new-keep-me-signed-in-experience-for-azure-ad-sign-in"></a>Daha az oturum açma istemi: yeni "Oturumumu açık bırak" bir deneyimi için Azure AD oturum açma

**Türü:** yeni özellik  
**Hizmet kategorisi:** Azure AD  
**Ürün özelliği:** kullanıcı kimlik doğrulaması
 
**Oturumumu açık bırak** Azure AD oturum açma sayfasında onay kutusunu başarıyla kimlik doğrulama işleminden sonra yedekleme gösteren yeni bir İstemi ile değiştirildi. 

Yanıt, **Evet** bu istemi için hizmeti, bir sürekli yenileme belirteci sağlar. Bu seçili olduğunda aynı, davranıştır **Oturumumu açık bırak** eski deneyimi onay kutusu. Federasyon Hizmeti ile başarıyla kimlik doğrulama işleminden sonra Federasyon kiracıları için bu istemi gösterir.

Daha fazla bilgi için [daha az oturum açma istemi: yeni "Oturumumu açık bırak" deneyimi için Azure AD Önizleme aşamasındadır](https://cloudblogs.microsoft.com/enterprisemobility/2017/09/19/fewer-login-prompts-the-new-keep-me-signed-in-experience-for-azure-ad-is-in-preview/). 

---

### <a name="add-configuration-to-require-the-terms-of-use-to-be-expanded-prior-to-accepting"></a>Kabul etmeden önce genişletilecek kullanım koşullarını gerektirecek şekilde yapılandırması Ekle

**Türü:** yeni özellik  
**Hizmet kategorisi:** kullanım koşulları  
**Ürün özelliği:** uyumluluk
 
Yöneticiler için bir seçenek, kullanıcıların koşulları kabul etmeden önce kullanım koşullarını genişletmesini gerektirir.

Şunlardan birini seçin **üzerinde** veya **kapalı** kullanıcıların kullanım koşullarını genişletmesini gerekli kıl için. **Üzerinde** ayarı kabul etmeden önce kullanım koşullarını görüntülemek kullanıcıların gerektirir.

Daha fazla bilgi için [kullanım özelliği (Önizleme) Azure AD kullanım koşulları](https://docs.microsoft.com/azure/active-directory/active-directory-tou).
 
---

### <a name="scoped-activation-for-eligible-role-assignments"></a>Uygun rol atamaları için kapsamlı etkinleştirme

**Türü:** yeni özellik  
**Hizmet kategorisi:** Privileged Identity Management  
**Ürün özelliği:** Privileged Identity Management
 
Kapsamlı etkinleştirme, özgün atama varsayılan değerinden daha az bağımsız çalışma sınırı ile uygun bir Azure kaynak rol atamalarını etkinleştirmek için kullanabilirsiniz. Kiracınızda atadığınız bir abonelik sahibi olarak bir örnektir. Kapsamlı etkinleştirme'yle (örneğin, kaynak grupları ve sanal makineler) abonelik kapsamında yer alan en fazla beş kaynakları için sahip rolü etkinleştirebilirsiniz. Aktivasyon kapsamı, kritik Azure kaynakları için istenmeyen değişiklikleri yürütülmeden olasılığını düşürebilir.

Daha fazla bilgi için [Azure AD Privileged Identity Management nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure).
 
---
 
### <a name="new-federated-apps-in-the-azure-ad-app-gallery"></a>Azure AD uygulama Galerisi'nde yeni Federasyon uygulamaları

**Türü:** yeni özellik  
**Hizmet kategorisi:** kurumsal uygulamalar  
**Ürün özelliği:** 3. taraf tümleştirme

Aralık 2017'de, bu yeni Federasyon uygulamalarla uygulama galerimizden desteği ekledik:

[Accredible](https://go.microsoft.com/fwlink/?linkid=863523), Adobe Experience Manager [EFI dijital mağaza](https://go.microsoft.com/fwlink/?linkid=861685), [Communifire](https://go.microsoft.com/fwlink/?linkid=861676) CybSafe, [FactSet](https://go.microsoft.com/fwlink/?linkid=863525), [görüntü WORKS](https://go.microsoft.com/fwlink/?linkid=863517), [MOBI](https://go.microsoft.com/fwlink/?linkid=863521), [MobileIron Azure AD tümleştirme](https://go.microsoft.com/fwlink/?linkid=858027), [Reflektive](https://go.microsoft.com/fwlink/?linkid=863518), [GmbH çözünürlüğün Bamboo için SAML SSO](https://go.microsoft.com/fwlink/?linkid=863520), [GmbH çözünürlüğün Bitbucket için SAML SSO](https://go.microsoft.com/fwlink/?linkid=863519), [Vodeclic](https://go.microsoft.com/fwlink/?linkid=863522), WebHR, Zenegy Azure AD tümleştirmesi.

Uygulamalar hakkında daha fazla bilgi için bkz. [Azure Active Directory ile SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial).

Azure AD uygulama galerisinde uygulamanızı listeleme hakkında daha fazla bilgi için bkz. [uygulamanızı Azure Active Directory Uygulama galerisinde listeleyin](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing). 
 
---

### <a name="approval-workflows-for-azure-ad-directory-roles"></a>Azure AD Dizin rolleri için onay iş akışları

**Türü:** değiştirilen özellik  
**Hizmet kategorisi:** Privileged Identity Management  
**Ürün özelliği:** Privileged Identity Management
 
Azure AD Dizin rolleri için onay iş akışı genel kullanıma sunulmuştur.

Ayrıcalıklı rol kullanabilmeniz için önce onay iş akışı ile ayrıcalıklı rol yöneticileri rol etkinleştirme isteği uygun rolü üyeleri gerektirebilir. Birden çok kullanıcı ve grupları temsilcisi onay sorumlulukları olabilir. Uygun Rol üyeleri onayı tamamlandı ve rollerine etkin olduğunda bildirim alırsınız.

---
 
### <a name="pass-through-authentication-skype-for-business-support"></a>Geçişli kimlik doğrulaması: Skype for Business desteği

**Türü:** değiştirilen özellik  
**Hizmet kategorisi:** kimlik doğrulamaları (oturum açma bilgileri)  
**Ürün özelliği:** kullanıcı kimlik doğrulaması

Geçişli kimlik doğrulaması artık kullanıcı oturum açma işlemleri Skype Kurumsal çevrimiçi içeren modern kimlik doğrulamasını destekleyen iş istemci uygulamalarını ve karma topolojiler için destekler. 

Daha fazla bilgi için [Skype için modern kimlik doğrulaması ile desteklenen iş topolojiler](https://technet.microsoft.com/library/mt803262.aspx).
 
---

### <a name="updates-to-azure-ad-privileged-identity-management-for-azure-rbac-preview"></a>Azure RBAC (Önizleme) için Azure AD Privileged Identity Management güncelleştirmeleri

**Türü:** değiştirilen özellik  
**Hizmet kategorisi:** Privileged Identity Management  
**Ürün özelliği:** Privileged Identity Management
 
Azure AD Privileged Identity Management (PIM) Azure rol tabanlı Access Control (RBAC) için genel Önizleme yenileme ile şunları yapabilirsiniz:

* Yeterli yönetim kullanın.
* Kaynak rolleri etkinleştirmek için onay gerektirir.
* Azure RBAC rolleri ile hem Azure AD için onay gerektiren bir rolü ve gelecekteki bir etkinleştirme zamanlayın.
 
Daha fazla bilgi için [(Önizleme) Azure kaynakları için Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac).

---
 
## <a name="november-2017"></a>Kasım 2017
 
### <a name="access-control-service-retirement"></a>Erişim denetimi hizmeti devre dışı bırakma

**Türü:** değişiklik planı  
**Hizmet kategorisi:** erişim denetimi hizmeti  
**Ürün özelliği:** erişim denetimi hizmeti 

Azure Active Directory erişim denetimi (erişim denetimi hizmeti olarak da bilinir) geç 2018'de kullanımdan kaldırılacaktır. Önümüzdeki birkaç hafta içinde ayrıntılı zamanlama ve üst düzey geçiş kılavuzunu içeren daha fazla bilgi sağlanacaktır. Erişim denetimi hizmeti hakkında sorular ile bu sayfada açıklamaları bırakabilirsiniz ve bir takım üyesi yanıt.

---

### <a name="restrict-browser-access-to-the-intune-managed-browser"></a>Intune Managed Browser için tarayıcı erişimi kısıtlama 

**Türü:** değişiklik planı  
**Hizmet kategorisi:** koşullu erişim  
**Ürün özelliği:** kimlik güvenliği ve koruması

Onaylanmış bir uygulama Intune Managed Browser'ı kullanarak, Office 365 ve diğer Azure AD bağlantılı bulut uygulamaları için tarayıcı erişimini kısıtlayabilirsiniz. 

Artık uygulama tabanlı koşullu erişim için aşağıdaki koşul yapılandırabilirsiniz:

**İstemci uygulamaları:** tarayıcı

**Değişikliğin etkilerini nedir?**

Günümüzde bu durum kullandığınızda erişim engellenir. Önizleme mevcut değil, tüm erişim yönetilen tarayıcı uygulaması kullanımını gerektirir. 

Bu özellik ve yaklaşan blogları ve sürüm notları hakkında daha fazla bilgi arayın. 

Daha fazla bilgi için [koşullu erişim, Azure AD'de](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal).
 
---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>Azure AD uygulama tabanlı koşullu erişim için yeni onaylanmış istemci uygulamaları

**Türü:** değişiklik planı  
**Hizmet kategorisi:** koşullu erişim  
**Ürün özelliği:** kimlik güvenliği ve koruması

Aşağıdaki uygulamalar listede yer [onaylı istemci uygulamalar](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement):

- [Microsoft Kaizala](https://www.microsoft.com/garage/profiles/kaizala/)
- [Microsoft StaffHub](https://staffhub.office.com/what-it-is)

Daha fazla bilgi için bkz.

- [Onaylı istemci uygulama gereksinimi](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement)
- [Azure AD uygulama tabanlı koşullu erişim](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

---

### <a name="terms-of-use-support-for-multiple-languages"></a>Kullanım koşulları birden çok dil desteği

**Türü:** yeni özellik    
**Hizmet kategorisi:** kullanım koşulları  
**Ürün özelliği:** uyumluluk

Yöneticiler artık birden çok PDF belgeleri içeren yeni kullanım koşulları oluşturabilirsiniz. Bu PDF belgeleri karşılık gelen bir dil ile etiketleyebilirsiniz. Kullanıcılara, PDF, eşleşen dil tercihlerine göre ile gösterilir. Eşleşme yoksa varsayılan dilde gösterilir.

---
 
### <a name="real-time-password-writeback-client-status"></a>Gerçek zamanlı parola geri yazma istemcinizin durumunu

**Türü:** yeni özellik  
**Hizmet kategorisi:** Self Servis parola sıfırlama  
**Ürün özelliği:** kullanıcı kimlik doğrulaması

Artık, şirket içi parola geri yazma istemcinizin durumunu gözden geçirebilirsiniz. Bu seçenek kullanılabilir **şirket içi tümleştirme** bölümünü [parola sıfırlama](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/PasswordReset) sayfası. 

Şirket içi geri yazma istemciniz için bağlantınızda bir sorun varsa, size sağlayan bir hata iletisi görürsünüz:

- Şirket içi geri yazma istemcinize neden bağlanamıyorsunuz bilgi.
- Sorunun çözümlenmesinde yardımcı belgeleri bağlantısı. 

Daha fazla bilgi için [şirket tümleştirme](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-how-it-works#on-premises-integration).

---

### <a name="azure-ad-app-based-conditional-access"></a>Azure AD uygulama tabanlı koşullu erişim 
 
**Türü:** yeni özellik  
**Hizmet kategorisi:** Azure AD  
**Ürün özelliği:** kimlik güvenliği ve koruması

Artık Office 365 ve diğer Azure AD bağlantılı bulut uygulamaları için erişimi kısıtlayabilirsiniz [onaylı istemci uygulamalar](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement) kullanarak Intune uygulama koruma ilkelerini destekleyen [Azure AD uygulama tabanlı koşullu erişim](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access). Intune uygulama koruma ilkelerini yapılandırma ve bu istemci uygulamalarını üzerindeki şirket verilerini korumak için kullanılır.

Birleştirme tarafından [uygulama tabanlı](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access) ile [cihaz tabanlı](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-policy-connected-applications) koşullu erişim ilkeleri, kişisel verileri ve şirket cihazları korumak için esnekliğe sahip.

Aşağıdaki koşullar ve denetimleri artık uygulama tabanlı koşullu erişim ile kullanılmak üzere mevcuttur:

**Desteklenen platform koşulu**

- iOS
- Android

**İstemci uygulamaları koşulu**

- Mobil uygulamalar ve masaüstü istemcileri

**Erişim denetimi**

- Onaylı istemci uygulaması gerektir

Daha fazla bilgi için [Azure AD uygulama tabanlı koşullu erişim](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access).
 
---

### <a name="manage-azure-ad-devices-in-the-azure-portal"></a>Azure portalında Azure AD cihazları yönetme

**Türü:** yeni özellik  
**Hizmet kategorisi:** cihaz kaydı ve Yönetimi  
**Ürün özelliği:** kimlik güvenliği ve koruması

Artık tüm cihazlarınızı Azure AD'ye bağlı bulabilirsiniz ve cihaz ile ilgili etkinlikleri tek bir yerde. Tüm cihaz kimliklerini ve Azure Portalı'ndaki ayarları yönetmek için yeni bir yönetim deneyimi vardır. Bu sürümde, şunları yapabilirsiniz:

- Azure AD'de koşullu erişim için kullanılabilir olan tüm cihazlarınızı görüntüleyin.
- Azure hibrit içeren özelliklerini görüntüleme AD alanına katılmış cihazlar.
- Azure AD'ye katılmış cihazlar için BitLocker anahtarlarını bulmak, Cihazınızı ve Intune ile yönetme.
- Azure AD cihaz ile ilgili ayarları yönetin.

Daha fazla bilgi için [Azure portalını kullanarak cihazları yönetme](https://docs.microsoft.com/azure/active-directory/device-management-azure-portal).

---

### <a name="support-for-macos-as-a-device-platform-for-azure-ad-conditional-access"></a>MacOS cihaz platformu için Azure AD koşullu erişim desteği 

**Türü:** yeni özellik    
**Hizmet kategorisi:** koşullu erişim  
**Ürün özelliği:** kimlik güvenliği ve koruması 

Artık içerir (hariç macOS cihaz platformu koşul olarak Azure AD koşullu erişim ilkenizi veya istediğiniz). Desteklenen cihaz platformları için macOS'ın eklenmesiyle, şunları yapabilirsiniz:

- **Kaydetme ve Intune kullanarak macOS cihazlarını yönetin.** Benzer şekilde, iOS ve Android gibi diğer platformlarda, Şirket portalı uygulamasını birleşik kayıtları yapmak için macOS için kullanılabilir. Yeni Şirket portalı uygulaması macOS için Intune ile cihaz kaydetme ve Azure AD ile kaydetmek için kullanabilirsiniz.
- **MacOS cihazları Intune'da tanımlanan kuruluşunuzun uyumluluk ilkelerine uymaları emin olun.** Azure portalında Intune'da artık macOS cihazları için Uyumluluk ilkelerini ayarlayabilirsiniz. 
- **Yalnızca uyumlu macOS cihazlar için Azure AD'de, uygulamalara erişimi kısıtlayın.** MacOS koşullu erişim ilkesi yazma ayrı cihaz platformu seçeneği olarak sahiptir. Artık Azure'da ayarlanan hedeflenen uygulama için macOS özel koşullu erişim ilkeleri yazabilirsiniz.

Daha fazla bilgi için bkz.

- [Intune ile macOS cihazları için cihaz uyumluluğu ilkesi oluşturma](https://aka.ms/macoscompliancepolicy)
- [Azure AD'de koşullu erişim](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)
 
---

### <a name="network-policy-server-extension-for-azure-multi-factor-authentication"></a>Azure multi-Factor Authentication için ağ ilkesi sunucusu uzantısı 

**Türü:** yeni özellik    
**Hizmet kategorisi:** çok faktörlü kimlik doğrulaması  
**Ürün özelliği:** kullanıcı kimlik doğrulaması

Azure multi-Factor Authentication için ağ ilkesi sunucusu uzantısı, mevcut sunucularınızda kullanarak bulut tabanlı çok faktörlü kimlik doğrulama özellikleri kimlik doğrulama altyapınızı ekler. Ağ İlkesi Sunucusu uzantısı ile mevcut kimlik doğrulama akışınıza telefon araması, SMS mesajı ve telefon uygulaması doğrulama ekleyebilirsiniz. Yükleme, yapılandırma ve yeni sunuculara bakım gerekmez. 

Bu uzantı, sanal özel ağ bağlantılarıyla Azure multi-Factor Authentication Sunucusu'nu dağıtmadan korumak istediğiniz kuruluşlar için oluşturuldu. Ağ ilkesi sunucusu arasında RADIUS ve Azure multi-Factor Authentication'ı bulut tabanlı bir ikinci faktör kimlik doğrulaması sağlamak için bir bağdaştırıcı uzantısı görür federasyona eklenenler veya eşitlenmiş kullanıcıları.

Daha fazla bilgi için [mevcut ağ ilkesi sunucusu altyapınızı Azure multi-Factor Authentication ile tümleştirme](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-nps-extension).
 
---

### <a name="restore-or-permanently-remove-deleted-users"></a>Geri yükleme ya da kalıcı olarak silinen Kullanıcıları Kaldır

**Türü:** yeni özellik    
**Hizmet kategorisi:** kullanıcı yönetimi  
**Ürün özelliği:** dizini 

Azure AD Yönetim merkezinde artık şunları yapabilirsiniz:

- Silinen bir kullanıcıyı geri yükleyin. 
- Kullanıcı kalıcı olarak siler.

**Denemek için:**

1. Azure AD Yönetim merkezinde seçin [tüm kullanıcılar](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/All) içinde **Yönet** bölümü. 

2. Gelen **Göster** listesinden **kullanıcılar'yakın zamanda silinip**. 

3. Yakın zamanda silinen kullanıcılar, bir veya daha fazla seçin ve ardından ya da geri yükleyebilir veya kalıcı olarak silebilirsiniz.
 
---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>Azure AD uygulama tabanlı koşullu erişim için yeni onaylanmış istemci uygulamaları
 
**Türü:** değiştirilen özellik  
**Hizmet kategorisi:** koşullu erişim  
**Ürün özelliği:** kimlik güvenliği ve koruması

Aşağıdaki uygulamalar listesine eklenmiş olmasından [onaylı istemci uygulamalar](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement):

- Microsoft Planner
- Azure Information Protection 

Daha fazla bilgi için bkz.

- [Onaylı istemci uygulama gereksinimi](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement)
- [Azure AD uygulama tabanlı koşullu erişim](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

---

### <a name="use-or-between-controls-in-a-conditional-access-policy"></a>Kullanım "Veya" arasında bir koşullu erişim ilkesi denetimleri 

**Türü:** değiştirilen özellik    
**Hizmet kategorisi:** koşullu erişim  
**Ürün özelliği:** kimlik güvenliği ve koruması
 
Artık kullanabilir "veya" (Seçili denetimlerden birini gerektir) için koşullu erişim denetimleri. "Veya" arasında erişim denetim ilkeleri oluşturmak için bu özelliği kullanabilirsiniz. Örneğin, bir kullanıcı çok faktörlü kimlik doğrulaması kullanarak oturum açın "veya" uyumlu bir cihaz üzerinde olmasını gerektiren bir ilke oluşturmak için bu özelliği kullanabilirsiniz.

Daha fazla bilgi için [Azure AD koşullu erişim denetimleri](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-controls).
 
---

### <a name="aggregation-of-real-time-risk-events"></a>Gerçek zamanlı risk olaylarını toplama

**Türü:** değiştirilen özellik    
**Hizmet kategorisi:** kimlik koruması  
**Ürün özelliği:** kimlik güvenliği ve koruması

Azure AD kimlik koruması belirli bir gün aynı IP adresinden kaynaklanan tüm gerçek zamanlı risk olayları artık her bir risk olayı türü için toplanır. Bu değişiklik, risk olaylarının herhangi bir değişiklik kullanıcı güvenlik gösterilen birim sınırlar.

Temel alınan gerçek zamanlı algılama, kullanıcı oturum açtığı her zaman çalışır. Çok faktörlü kimlik doğrulaması veya erişimi engelleme için ayarlanmış bir oturum açma riski ilkesi varsa, yine de her riskli oturum açma sırasında tetiklenir.
 
---
 
## <a name="october-2017"></a>Ekim 2017

### <a name="deprecate-azure-ad-reports"></a>Azure AD raporlar kullanımdan kaldırma

**Türü:** değişiklik planı  
**Hizmet kategorisi:** raporlama  
**Ürün özelliği:** kimlik yaşam döngüsü yönetimi  

Azure portalı ile sağlar:

- Yeni bir Azure AD Yönetim Konsolu.
- Yeni API'ler için etkinlik ve güvenlik raporları.
 
Bu yeni özellikler nedeniyle/Reports uç nokta altında API'ler rapor kullanımdan kaldırılmıştır 10 Aralık 2017'de. 

---

### <a name="automatic-sign-in-field-detection"></a>Otomatik oturum açma alanı algılaması

**Türü:** düzeltildi   
**Hizmet kategorisi:** uygulamalarım  
**Ürün özelliği:** çoklu oturum açma  

Azure AD, bir HTML kullanıcı adı ve parola alanı işlemek uygulamalar için otomatik oturum açma alanı algılaması destekler. Bu adımları bölümünde belgelendirilen [nasıl otomatik olarak bir uygulama için oturum açma alanlarını Yakala](https://docs.microsoft.com/azure/active-directory/application-config-sso-problem-configure-password-sso-non-gallery#how-to-manually-capture-sign-in-fields-for-an-application). Bu özellik ekleyerek bulabilirsiniz bir *galeri dışı* uygulaması **kurumsal uygulamalar** sayfasını [Azure portalında](http://aad.portal.azure.com). Buna ek olarak, yapılandırabileceğiniz **çoklu oturum açma** bu yeni uygulama modu **parola tabanlı çoklu oturum açma**web URL'si girin ve ardından sayfanın kaydedin.
 
Bir hizmet sorunu nedeniyle bu işlevselliği geçici olarak devre dışı bırakıldı. Sorunun çözülüp çözülmediğini ve otomatik oturum açma alanı algılaması yeniden kullanılabilir.

---

### <a name="new-multi-factor-authentication-features"></a>Yeni çok faktörlü kimlik doğrulaması özellikleri

**Türü:** yeni özellik  
**Hizmet kategorisi:** çok faktörlü kimlik doğrulaması  
**Ürün özelliği:** kimlik güvenliği ve koruması  

Çok faktörlü kimlik doğrulaması (MFA), kuruluşunuzun koruma önemli bir parçasıdır. Kimlik bilgileri daha Uyarlamalı ve deneyimi daha sorunsuz hale getirmek için aşağıdaki özellikleri eklendi: 

- Çok faktörlü sınama sonuçları, MFA sonuçları programlı erişim içeren Azure AD oturum açma raporu doğrudan bütünleştirilmiştir.
- MFA yapılandırmanın daha derin bir şekilde Azure AD'ye yapılandırmaya tümleşik deneyimi Azure Portalı'nda.

Bu genel Önizleme ile MFA yönetim ve raporlama çekirdek Azure AD'ye yapılandırma deneyimi tümleşik bir parçası olan. Artık Azure AD deneyimi içinde MFA Yönetim Portalı işlevlerini yönetebilirsiniz.

Daha fazla bilgi için [MFA Azure Portalı'nda raporlama için başvuru](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-sign-ins-mfa). 

---

### <a name="terms-of-use"></a>Kullanım koşulları

**Türü:** yeni özellik  
**Hizmet kategorisi:** kullanım koşulları  
**Ürün özelliği:** uyumluluk  

Azure AD kullanım koşulları yasal gereksinimler veya uyumluluk gereksinimleriyle ilgili bildirimleri gibi bilgiler sunmak için kullanabilirsiniz.

Azure AD kullanım koşulları aşağıdaki senaryolarda kullanabilirsiniz:

- Genel, kuruluşunuzdaki tüm kullanıcılar için kullanım koşulları
- Belirli kullanıcı özniteliklerine (doktorlarla nurses karşılaştırması gibi) ya da yurtiçi ve uluslararası çalışanlara, dinamik gruplar tarafından yapılan temel kullanım koşulları
- Salesforce gibi yüksek etkili iş uygulamaları erişmek için belirli kullanım koşulları

Daha fazla bilgi için [Azure AD kullanım koşulları](https://docs.microsoft.com/azure/active-directory/active-directory-tou).

---

### <a name="enhancements-to-privileged-identity-management"></a>Privileged Identity Management geliştirmeleri

**Türü:** yeni özellik  
**Hizmet kategorisi:** Privileged Identity Management  
**Ürün özelliği:** Privileged Identity Management  

Azure AD Privileged Identity Management ile yönetebilir, denetleyebilir ve kuruluşunuz içinde (Önizleme) Azure kaynaklarına erişimi izleyin:

- Abonelikler
- Kaynak grupları
- Sanal makineler 

Azure RBAC işlevselliği kullandığı tüm kaynakları Azure portalındaki güvenlik ve Azure AD Privileged Identity Management sunduğu yaşam döngüsü yönetim özellikleri yararlanabilirsiniz.

Daha fazla bilgi için [Azure kaynakları için Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac).

---

### <a name="access-reviews"></a>Erişim gözden geçirmeleri

**Türü:** yeni özellik  
**Hizmet kategorisi:** erişim gözden geçirmeleri  
**Ürün özelliği:** uyumluluk  

Kuruluşların grup üyeliklerini ve kurumsal uygulamalara erişimi etkili bir şekilde yönetmek için erişim gözden geçirmeleri (Önizleme) kullanabilirsiniz: 

- Konuk kullanıcıların uygulamalara ve grup üyeliklerine erişimlerine ait erişim gözden geçirmelerini kullanarak bu kullanıcıların erişimini yeniden onaylayabilirsiniz. Gözden geçirenler, verimli bir şekilde erişim gözden geçirmeleri tarafından sağlanan bilgiler dayalı olarak erişimi Konukları izin vermek isteyip devam karar verebilirsiniz.
- Erişim gözden geçirmeleri ile çalışanların uygulamalara erişimini ve grup üyeliklerini yeniden onaylayabilirsiniz.

Erişim gözden geçirmesi denetimlerini kuruluşunuza uygun programlarda toplayarak, uyumluluk veya riske duyarlı uygulamalar için gözden geçirmeleri takip edebilirsiniz.

Daha fazla bilgi için [Azure AD erişim gözden geçirmeleri](https://docs.microsoft.com/azure/active-directory/active-directory-azure-ad-controls-access-reviews-overview).

---

### <a name="hide-third-party-applications-from-my-apps-and-the-office-365-app-launcher"></a>Üçüncü taraf uygulamalardan uygulamalarım ve Office 365 uygulama başlatıcısında Gizle

**Türü:** yeni özellik  
**Hizmet kategorisi:** uygulamalarım  
**Ürün özelliği:** çoklu oturum açma  

Şimdi yeni bir aracılığıyla kullanıcılarınızın portalı üzerinde görünen uygulamalar daha iyi yönetebilirsiniz **uygulama Gizle** özelliği. Arka uç hizmetlerine veya yinelenen kutucukları ve dağınıklık kullanıcıların uygulama launchers uygulama kutucuklarına burada görünmesi durumlarda yardımcı olması için uygulamanızı gizleyebilirsiniz. İki durumlu düğme bulunduğu **özellikleri** üçüncü taraf uygulama bölümünü ve etiketli **kullanıcıya görünür?** Ayrıca PowerShell üzerinden program aracılığıyla uygulama gizleyebilirsiniz. 

Daha fazla bilgi için [üçüncü taraf bir uygulamanın Azure AD'de kullanıcı deneyiminden gizleme](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-hide-third-party-app). 


**Kullanılabilir nedir?**

 Yeni Yönetici Konsolu, iki yeni API geçişi kapsamında günlüklerine Azure AD etkinlik almak için kullanılabilir. Yeni API'leri daha zengin filtreleme ve sıralama daha zengin denetim ve oturum açma etkinlikleri sağlamaya ek olarak işlevselliği sunmaktadır. Artık güvenlik raporları ile önceden kullanılabilen veri, Microsoft Graph kimlik koruma Risk olayları API aracılığıyla erişilebilir.


## <a name="september-2017"></a>Eylül 2017

### <a name="hotfix-for-identity-manager"></a>Identity Manager için düzeltme

**Türü:** değiştirilen özellik  
**Hizmet kategorisi:** Identity Manager  
**Ürün özelliği:** kimlik yaşam döngüsü yönetimi  

Bir düzeltme dökümü paket (derleme 4.4.1642.0), 25 Eylül 2017 Identity Manager 2016 Service Pack 1'den itibaren kullanılabilir. Bu döküm paketi:

- Sorunları giderir ve geliştirmeleri içerir.
- Identity Manager 2016 için derleme 4.4.1459.0 kadar tüm Identity Manager 2016 Service Pack 1 güncelleştirmeleri değiştirir toplu bir güncelleştirmesidir. 
- Identity Manager 2016 4.4.1302.0 derleme olmasını gerektirir. 

Daha fazla bilgi için [Identity Manager 2016 Service Pack 1 için düzeltme paketi (derleme 4.4.1642.0) kullanılabilir](https://support.microsoft.com/help/4021562). 

---