---
title: Yenilikler Azure Active Directory için sürüm notları | Microsoft Docs
description: Bilinen sorunlar, hata düzeltmeleri, kullanım dışı bırakılan işlevsellik ve yaklaşan değişiklikleri en son sürüm notlarını gibi Azure Active Directory (Azure AD) ile yenilikleri öğrenin.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
featureFlags:
- clicktale
ms.assetid: 06a149f7-4aa1-4fb9-a8ec-ac2633b031fb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: b7ad535976508cb195991c374995b0a0b6e45e10
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="whats-new-in-azure-active-directory"></a>Azure Active Directory'de yenilikler nelerdir?


> Abone tarafından Azure Active Directory (Azure AD) Yenilikler ile güncelliği [ ![RSS](./media/whats-new/feed-icon-16x16.png)](https://docs.microsoft.com/api/search/rss?search=%22whats%20new%20in%20azure%20active%20directory%22&locale=en-us) [akış](https://docs.microsoft.com/api/search/rss?search=%22whats%20new%20in%20azure%20active%20directory%22&locale=en-us).



Azure AD geliştirmeleri düzenli olarak alır. En son gelişmeler ile güncel kalmak için bu makalede, ile hakkında bilgi sağlar:

-   En son sürümleri
-   Bilinen sorunlar
-   Hata düzeltmeleri
-   Kullanım dışı bırakılan işlevsellik
-   Değişiklikleri planları

Bu sayfayı aylık güncelleştirilir, böylece, düzenli aralıklarla yeniden ziyaret.


## <a name="april-2018"></a>Nisan 2018
 


### <a name="azure-ad-b2c-access-token-are-ga"></a>Azure AD B2C erişim belirteci olan GA

**Tür:** yeni özellik  
**Servis kategorisi:** B2C - tüketici Kimlik Yönetimi  
**Ürün yetenek:** B2B/B2C
 

Azure AD B2C tarafından güvence altına artık Web API'nin erişebilir erişim belirteçleri kullanarak. Bu özellik için İST genel Önizlemesi'nden taşıma Azure AD B2C uygulamaları ve web API'nin yapılandırmak için UI deneyimi geliştirilmiştir ve diğer ikincil geliştirmeler yapılmıştır.
 
Daha fazla bilgi için bkz: [Azure AD B2C: isteyen erişim belirteçleri](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-access-tokens).


---
 

### <a name="test-single-sign-on-configuration-for-saml-based-applications"></a>SAML tabanlı uygulamalar için çoklu oturum açma yapılandırmayı test etme

**Tür:** yeni özellik  
**Servis kategorisi:** Kurumsal uygulamaları  
**Ürün yetenek:** SSO
 

SSO uygulamaları SAML yapılandırma temel yapılandırma sayfasında tümleştirme test etmek kullanabilirsiniz. Oturum açma sırasında bir hatayla karşılaşırsanız, test deneyimini hata sağlayabilir ve Azure AD belirli sorunu çözmek için çözüm adımları sağlar.

Daha fazla bilgi için bkz.

- [Azure Active Directory uygulama galerisinde bulunmayan uygulamalar için çoklu oturum açmayı yapılandırma](https://docs.microsoft.com/azure/active-directory/active-directory-saas-custom-apps)
- [SAML tabanlı çoklu oturum açma Azure Active Directory uygulamalarında hata ayıklama](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-debugging)


---
 
### <a name="azure-ad-terms-of-use-now-has-per-user-reporting"></a>Azure AD Kullanım Koşulları'nı şimdi kullanıcı raporlama başına sahiptir

**Tür:** yeni özellik  
**Servis kategorisi:** kullanım koşulları  
**Ürün yetenek:** uyumluluk
 

Yöneticiler artık verilen ToU seçin ve izin ToU ve ne, tarih yer aldığı verdiği tüm kullanıcılar görür.


Daha fazla bilgi için bkz: [kullanım koşullarını Azure AD özelliği](https://docs.microsoft.com/azure/active-directory/active-directory-tou).
 

---
 
### <a name="azure-ad-connect-health-risky-ip-for-ad-fs-extranet-lockout-protection"></a>Azure AD Connect Health: AD FS extranet kilitleme koruma riskli IP 

**Tür:** yeni özellik  
**Servis kategorisi:** diğer  
**Ürün yetenek:** izleme ve Raporlama
 

Günlük veya saatlik düzenli olarak başarısız U/P oturumlarının bir eşiği aşması destekler IP algılama yeteneği adresleri artık sistem durumu bağlayın. Bu özellik tarafından sağlanan özellikler şunlardır:

- IP adresi ve saatlik/günlük düzenli olarak özelleştirilebilir eşik ile oluşturulan başarısız oturum açma sayısı gösteren kapsamlı rapor.
- E-posta tabanlı uyarılar belirli bir IP adresi zaman gösteren bir saatlik/günlük temelinde başarısız U/P oturumlarının eşiğini aşmış.
- Ayrıntılı bir analiz için verileri yapmak için bir yükleme seçeneği


Daha fazla bilgi için bkz: [riskli IP rapor](https://aka.ms/aadchriskyip).

 

---
 

### <a name="easy-app-config-with-metadata-file-or-url"></a>Meta veri dosyası veya URL ile kolay uygulama yapılandırması

**Tür:** yeni özellik  
**Servis kategorisi:** Kurumsal uygulamaları  
**Ürün yetenek:** SSO
 

Kurumsal uygulamalar sayfası, yöneticiler SAML tabanlı oturum açma AAD Galerisi ve olmayan galeri uygulama için yapılandırmak için bir SAML meta veri dosyası karşıya yükleyebilirsiniz.

Ayrıca, Azure AD uygulama Federasyon meta veri URL'sini SSO hedeflenen uygulama ile yapılandırmak için kullanabilirsiniz.

Daha fazla bilgi için bkz: [Azure Active Directory Uygulama galerisinde bulunmayan uygulamalar için çoklu oturum açma yapılandırma](https://docs.microsoft.com/azure/active-directory/active-directory-saas-custom-apps).
 

---
 

### <a name="azure-ad-terms-of-use-now-generally-available"></a>Azure AD koşulları artık genel olarak kullanılabilir

**Tür:** yeni özellik  
**Servis kategorisi:** kullanım koşulları  
**Ürün yetenek:** uyumluluk
 

Azure AD Kullanım Koşulları'nı genel Önizlemesi'nden için genel olarak kullanılabilir taşınmıştır.

Daha fazla bilgi için bkz: [kullanım koşullarını Azure AD özelliği](https://docs.microsoft.com/azure/active-directory/active-directory-tou).

 

---
 

### <a name="allow-or-block-invitations-to-b2b-users-from-specific-organizations"></a>İzin verme veya belirli kuruluşlardan B2B kullanıcılara davet engelleme

**Tür:** yeni özellik  
**Servis kategorisi:** B2B  
**Ürün yetenek:** B2B/B2C
 

Artık, paylaşabilir ve Azure AD B2B işbirliği birlikte çalışmak istediğiniz hangi ortak kuruluşlar belirtebilirsiniz. Bunu yapmak için özel listesini oluşturmak seçebileceğiniz izin ver veya Reddet etki alanları. Bu özellikleri kullanarak bir etki alanı engellendiğinde, çalışanların kişilere davetiye artık bu etki alanında gönderebilirsiniz.

Bu, onaylanan kullanıcılar için sorunsuz bir deneyim etkinleştirirken, kaynaklara erişimi denetlemek için yardımcı olur.

Bu B2B işbirliği özelliği tüm Azure Active Directory müşteriler için kullanılabilir ve koşullu erişim ve kimlik koruması gibi Azure AD Premium özellikleri ile birlikte konusunda daha ayrıntılı denetim için ne zaman ve dış iş kullanıcılar oturum nasıl kullanılabilir içinde ve erişim sahibi olursunuz.

Daha fazla bilgi için bkz: [B2B kullanıcılara izin verilenler veya Engellenenler davetleri belirli kuruluşların](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-allow-deny-list).

 

---
 

### <a name="new-federated-apps-available-in-azure-ad-app-gallery"></a>Yeni Federal apps mevcut Azure AD uygulama galerisinde

**Tür:** yeni özellik  
**Servis kategorisi:** Kurumsal uygulamaları  
**Ürün yetenek:** 3 taraf tümleştirme
 

Nisan 2018 bizim uygulama galerisinde Federasyon ile aşağıdaki 13 yeni uygulamaları desteği ekledik:



Ölçüt HCM, [FiscalNote](https://docs.microsoft.com/azure/active-directory/active-directory-saas-fiscalnote-tutorial), [gizli sunucu (şirket içi)](https://docs.microsoft.com/azure/active-directory/active-directory-saas-secretserver-on-premises-tutorial), [dinamik sinyal](https://docs.microsoft.com/azure/active-directory/active-directory-saas-dynamicsignal-tutorial), [mindWireless](https://docs.microsoft.com/azure/active-directory/active-directory-saas-mindwireless-tutorial), [Kuruluş Şeması Şimdi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-orgchartnow-tutorial), [Ziflow](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ziflow-tutorial), [AppNeta Performans İzleyicisi'ni](https://docs.microsoft.com/azure/active-directory/active-directory-saas-appneta-tutorial), [Elium](https://docs.microsoft.com/azure/active-directory/active-directory-saas-elium-tutorial) , [Fluxx Labs](https://docs.microsoft.com/azure/active-directory/active-directory-saas-fluxxlabs-tutorial), [ Cisco bulut](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ciscocloud-tutorial), raf, [SafetyNet](https://docs.microsoft.com/azure/active-directory/active-directory-saas-safetynet-tutorial)



 Burada kullanılabilen öğreticileri listesini bulabilirsiniz: [ https://aka.ms/appstutorial ](https://aka.ms/appstutorial).

Daha fazla bilgi için bkz: [uygulamanızı Azure Active Directory Uygulama galerisinde listelemek](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing).


 

---
 
### <a name="grant-b2b-users-in-azure-ad-access-to-your-on-premises-applications-public-preview"></a>Şirket içi uygulamalarınıza (genel Önizleme) Azure AD erişim ver B2B kullanıcılar

**Tür:** yeni özellik  
**Servis kategorisi:** B2B  
**Ürün yetenek:** B2B/B2C
 

Azure AD iş ortağı kuruluşlardan konuk kullanıcılara davet etmek için Azure Active Directory (Azure AD) B2B işbirliği özelliklerinden kullanan kuruluş olarak, artık bu B2B kullanıcılara şirket içi uygulamalara erişim sağlayabilir. Bu şirket içi uygulamaları Kerberos Kısıtlı temsilci (KCD) ile SAML tabanlı kimlik doğrulaması veya tümleşik Windows kimlik doğrulaması (IWA) kullanabilirsiniz.

Daha fazla bilgi için bkz: [Grant B2B kullanıcılar Azure AD'de şirket içi uygulamalarınıza erişme](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-hybrid-cloud-to-on-premises)
 

---
 
### <a name="get-sso-integration-tutorials-from-the-azure-marketplace"></a>SSO tümleştirme öğreticileri Azure Marketi'nden alın

**Tür:** değiştirilen özelliği  
**Servis kategorisi:** diğer  
**Ürün yetenek:** 3 taraf tümleştirme
 

Listede bir uygulama, [Azure Market](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps?page=1) destekleyen SAML tabanlı çoklu oturum açma tıklatarak özelliğini, **Şimdi Al** uygulama ile ilişkili tümleştirme öğretici sağlar. 


---

### <a name="faster-performance-of-azure-ad-automatic-user-provisioning-to-saas-applications"></a>Azure AD SaaS uygulamaları için otomatik kullanıcı sağlamayı daha hızlı performans

**Tür:** değiştirilen özelliği  
**Servis kategorisi:** uygulama sağlama  
**Ürün yetenek:** 3 taraf tümleştirme
 

Azure AD kiracıları 100.000 birleşik kullanıcılar içeriyorsa daha önce Azure Active Directory kullanıcı bağlayıcıları SaaS uygulamaları (örneğin Salesforce, ServiceNow ve kutusunu) için hazırlama kullanan müşteriler çok yavaş performans karşılaşması ve grupları ve kullanıcı ve Grup atamaları hangi kullanıcıların sağlanan belirlemek için kullandığınız.

2 Nisan çok önemli performans geliştirmeleri için Azure Active Directory ve hedef SaaS uygulamaları arasındaki ilk eşitlemeler gerçekleştirmek için gerekli süreyi büyük ölçüde azaltabilir Azure AD sağlama hizmeti dağıtıldı.


Sonuç olarak, birçok müşteri, çok sayıda gün sürdü veya hiçbir zaman tamamlandı, artık bir kaç dakika veya saat içinde Tamamlanıyor uygulamalara ilk eşitlemeler vardı.

Daha fazla bilgi için bkz: [sağlama işlemi sırasında ne olur?](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning#what-happens-during-provisioning)

---
 

### <a name="self-service-password-reset-from-windows-10-lock-screen-for-hybrid-azure-ad-joined-machines"></a>Self Servis parola sıfırlama karma Azure AD için Windows 10 kilit ekranından makineler alanına katılmış

**Tür:** değiştirilen özelliği  
**Servis kategorisi:** Self Servis parola sıfırlama  
**Ürün yetenek:** kullanıcı kimlik doğrulaması
 

Biz karma Azure AD alanına katılmış olan makineler için destek eklemek için Windows 10 SSPR özelliği güncelleştirdiniz. Bu özellik kullanılabilir Windows 10 RS4 bir Windows 10 makinenin kilit ekranı parolalarını sıfırlamasına izin verir. Bu özellik etkin ve Self Servis parola sıfırlama için kayıtlı kullanıcılar kullanabilir.

Daha fazla bilgi için bkz: [Azure AD parola oturum açma ekranından sıfırlama](https://docs.microsoft.com/azure/active-directory/authentication/tutorial-sspr-windows).
 

---



## <a name="march-2018"></a>Mart 2018
 

### <a name="certificate-expire-notification"></a>Sertifika sona bildirim

**Tür:** sabit  
**Servis kategorisi:** Kurumsal uygulamaları  
**Ürün yetenek:** SSO
 
Azure AD bir galeri için bir sertifika olduğunda bir bildirim gönderir veya dolmak üzere galeri olmayan uygulamasıdır. 

Bazı kullanıcılar SAML tabanlı çoklu oturum açma için yapılandırılmış Kurumsal uygulamaları için bildirim almadı. Bu sorun çözüldü. Azure AD, 7, 30 ve 60 gün içinde süresi dolacak sertifikalara bildirim gönderir. Ere, bu durumda bir denetim günlüklerini görmeyebilir. 

Daha fazla bilgi için bkz.

- [Federasyon tek oturum açma için Azure Active Directory'de sertifikaları yönetme](https://docs.microsoft.com/azure/active-directory/active-directory-sso-certs)
- [Azure Active Directory portalında denetim etkinlik raporları](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs)

 
---
 

### <a name="twitter-and-github-identity-providers-in-azure-ad-b2c"></a>Azure AD B2C'de twitter ve GitHub kimlik sağlayıcıları

**Tür:** yeni özellik  
**Servis kategorisi:** B2C - tüketici Kimlik Yönetimi  
**Ürün yetenek:** B2B/B2C
 
Azure AD B2C, kimlik sağlayıcısı Twitter veya GitHub artık ekleyebilirsiniz. Twitter İST için genel Önizlemesi'nden taşıma GitHub genel Önizleme'de serbest bırakılır.


Daha fazla bilgi için bkz: [Azure AD B2B işbirliği nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b).
 
---


### <a name="restrict-browser-access-using-intune-managed-browser-with-azure-ad-application-based-conditional-access-for-ios-and-android"></a>İOS ve Android için Intune Managed Browser Azure AD uygulama tabanlı ile koşullu erişim kullanarak tarayıcı erişimi kısıtlama

**Tür:** yeni özellik  
**Servis kategorisi:** koşullu erişim  
**Ürün yetenek:** kimlik güvenlik ve koruma
 

**Artık genel önizlemede!**

**Intune yönetilen tarayıcı SSO:** çalışanlarınızın çoklu oturum açma (Microsoft Outlook gibi) yerel istemcileri ve Intune yönetilen tarayıcı tüm Azure AD bağlı uygulamaları için kullanabilirsiniz.

**Intune yönetilen tarayıcı koşullu erişim desteği:** çalışanların uygulama bağlı olarak koşullu erişim ilkelerini kullanarak Intune yönetilen tarayıcı kullanımı için şimdi gerektirebilir.

Bu konu hakkında daha fazla bilgiyi bizim [blog gönderisi](https://cloudblogs.microsoft.com/enterprisemobility/2018/03/15/the-intune-managed-browser-now-supports-azure-ad-sso-and-conditional-access/).

Daha fazla bilgi için bkz.

- [Koşullu erişim uygulama tabanlı kurulum](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-mam)

- [Yönetilen tarayıcı ilkelerini yapılandırma](https://aka.ms/managedbrowser)  



---
 

### <a name="app-proxy-cmdlets-in-powershell-ga-module"></a>Uygulama proxy'si cmdlet'leri Powershell GA Modülü

**Tür:** yeni özellik  
**Servis kategorisi:** uygulama proxy'si  
**Ürün yetenek:** erişim denetimi
 
Destek için uygulama proxy'si cmdlet'leri Powershell GA modülünde sunulmuştur! Lütfen bu Powershell modülleri hakkında güncel kalmak ihtiyaç duyduğunuz Not - arkasındaki bir yıldan duruma gelirse, bazı cmdlet'ler çalışmamaya başlayabilir. 


Daha fazla bilgi için bkz: [Azuread'i](https://docs.microsoft.com/powershell/module/Azuread/?view=azureadps-2.0).
 
---
 
### <a name="office-365-native-clients-are-supported-by-seamless-sso-using-a-non-interactive-protocol"></a>Office 365 yerel istemciler tarafından etkileşimli olmayan protokolünü kullanarak sorunsuz SSO desteklenir

**Tür:** yeni özellik  
**Servis kategorisi:** kimlik doğrulamaları (oturum açma bilgileri)  
**Ürün yetenek:** kullanıcı kimlik doğrulaması
 
Office 365 yerel istemci kullanarak kullanıcı (sürüm 16.0.8730.xxxx ve üstü) sorunsuz SSO kullanarak bir sessiz oturum açma deneyimi alın. Bu destek ekleyerek etkileşimli olmayan bir iletişim kuralı (WS-Trust) Azure AD ile sağlanır.

Daha fazla bilgi için bkz: [nasıl oturum sorunsuz SSO iş yerel bir istemcide açma?](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso-how-it-works#how-does-sign-in-on-a-native-client-with-seamless-sso-work).

 
---
 

### <a name="users-get-a-silent-sign-on-experience-with-seamless-sso-if-an-application-sends-sign-in-requests-to-azure-ads-tenanted-endpoints"></a>Bir uygulamayı Azure AD kiralanan uç noktaları için oturum açma isteği gönderirse kullanıcılar bir sessiz oturum açma deneyimi, sorunsuz SSO alır.

**Tür:** yeni özellik  
**Servis kategorisi:** kimlik doğrulamaları (oturum açma bilgileri)  
**Ürün yetenek:** kullanıcı kimlik doğrulaması
 
Kullanıcıların alırsanız bir sessiz oturum açma deneyimi, sorunsuz SSO ile bir uygulama (örneğin, `https://contoso.sharepoint.com`) Azure AD kiralanan Uç noktalara - diğer bir deyişle, oturum açma istekleri gönderir `https://login.microsoftonline.com/contoso.com/<..>` veya `https://login.microsoftonline.com/<tenant_ID>/<..>` - Azure AD ortak uç nokta yerine (`https://login.microsoftonline.com/common/<...>` ).

Daha fazla bilgi için bkz: [Azure Active Directory sorunsuz çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso). 

---
 

### <a name="need-to-add-only-one-azure-ad-url-instead-of-two-urls-previously-to-users-intranet-zone-settings-to-roll-out-seamless-sso"></a>Kullanıcıların Intranet bölgesi ayarlarını sorunsuz SSO alma için daha önce iki URL'leri yerine yalnızca bir Azure AD URL eklemeniz gerekir

**Tür:** yeni özellik  
**Servis kategorisi:** kimlik doğrulamaları (oturum açma bilgileri)  
**Ürün yetenek:** kullanıcı kimlik doğrulaması
 
Kullanıcılarınız için sorunsuz SSO alma için yalnızca bir Azure AD URL'si kullanıcıların intranet bölgesi ayarlarını Active Directory'de Grup İlkesi kullanılarak eklemeniz gerekir: `https://autologon.microsoftazuread-sso.com`. Daha önce müşterilerin iki URL'leri eklemek için gerekli olmuştur.

Daha fazla bilgi için bkz: [Azure Active Directory sorunsuz çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso). 
 
---
 

### <a name="new-federated-apps-available-in-azure-ad-app-gallery"></a>Yeni Federal Apps mevcut Azure AD uygulama galerisinde

**Tür:** yeni özellik  
**Servis kategorisi:** Kurumsal uygulamaları  
**Ürün yetenek:** 3 taraf tümleştirme
 
Mart 2018 bizim uygulama galerisinde Federasyon ile aşağıdaki 15 yeni uygulamaları desteği ekledik:

[Boxcryptor](https://docs.microsoft.com/azure/active-directory/active-directory-saas-boxcryptor-tutorial), [CylancePROTECT](https://docs.microsoft.com/azure/active-directory/active-directory-saas-cylanceprotect-tutorial), Wrike, [SignalFx](https://docs.microsoft.com/azure/active-directory/active-directory-saas-signalfx-tutorial), Yardımcısı FirstAgenda tarafından [YardiOne](https://docs.microsoft.com/azure/active-directory/active-directory-saas-yardione-tutorial), Vtiger CRM, inwink, [Genliğidir](https://docs.microsoft.com/azure/active-directory/active-directory-saas-amplitude-tutorial), [Spacio](https://docs.microsoft.com/azure/active-directory/active-directory-saas-spacio-tutorial), [ContractWorks](https://docs.microsoft.com/azure/active-directory/active-directory-saas-contractworks-tutorial), [Bersin](https://docs.microsoft.com/azure/active-directory/active-directory-saas-bersin-tutorial), [Mercell](https://docs.microsoft.com/azure/active-directory/active-directory-saas-mercell-tutorial), [Trisotech dijital Enterprise Server](https://docs.microsoft.com/azure/active-directory/active-directory-saas-trisotechdigitalenterpriseserver-tutorial), [Qumu bulut](https://docs.microsoft.com/azure/active-directory/active-directory-saas-qumucloud-tutorial).
 
Tüm uygulamalar için burada bulabilirsiniz: [https://aka.ms/appstutorial](https://aka.ms/appstutorial)


 
---
 

### <a name="pim-for-azure-resources-is-generally-available"></a>PIM Azure kaynakları için genel olarak kullanılabilir

**Tür:** yeni özellik  
**Servis kategorisi:** Privileged Identity Management  
**Ürün yetenek:** Privileged Identity Management
 
Dizin rolleri için Azure AD Privileged Identity Management kullanıyorsanız, artık PIM'in zaman sınırlı erişim ve atama yetenekleri abonelikler, kaynak grupları, sanal makineler ve desteklenen herhangi bir kaynağa gibi Azure kaynak rolleri için kullanabilirsiniz Azure kaynak yöneticisi tarafından. Çok faktörlü kimlik doğrulaması rolleri zaman içinde sadece etkinleştirirken zorlamak ve onaylanan değişiklik windows birlikte etkinleştirmeleri zamanlayabilirsiniz. Ayrıca, bu sürüm geliştirmeleri güncelleştirilmiş bir kullanıcı Arabirimi, onay iş akışları ve özelliği süresi yakında doluyor rolleri genişletmek ve süresi dolan rolleri yenilemek için de dahil olmak üzere genel Önizleme sırasında kullanılamaz ekler.

Daha fazla bilgi için bkz: [PIM için Azure kaynaklarını (Önizleme)](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac)
 
---
 

### <a name="adding-optional-claims-to-your-apps-tokens-public-preview"></a>Uygulamaları belirteçlerinizi (genel Önizleme) için isteğe bağlı talep ekleme

**Tür:** yeni özellik  
**Servis kategorisi:** kimlik doğrulamaları (oturum açma bilgileri)  
**Ürün yetenek:** kullanıcı kimlik doğrulaması
 
İstek özel veya isteğe bağlı Jwt'ler veya SAML belirteçleri talepleri artık Azure AD uygulaması açabilir.  Bu varsayılan boyutu veya Uygulanabilirlik kısıtlamaları nedeniyle belirteçte dahil edilmeyen kullanıcı veya Kiracı hakkında taleplerdir.  Şu anda v1.0 ve v2.0 uç noktalarda Azure AD uygulamalarınız için genel önizlemede budur.  Bilgi için bkz: hangi taleplerine eklenebilir ve bunları istemek için uygulama bildiriminizi düzenleme.  

Daha fazla bilgi için bkz: [isteğe bağlı Azure AD içinde talep](https://docs.microsoft.com/azure/active-directory/develop/active-directory-optional-claims).
 
---
 

### <a name="azure-ad-supports-pkce-for-more-secure-oauth-flows"></a>Azure AD PKCE daha güvenli OAuth akışlar için destekler.

**Tür:** yeni özellik  
**Servis kategorisi:** kimlik doğrulamaları (oturum açma bilgileri)  
**Ürün yetenek:** kullanıcı kimlik doğrulaması
 
Azure AD belgeleri OAuth 2.0 yetkilendirme kodu verme akışı sırasında daha güvenli iletişim için verir PKCE desteği not etmek için güncelleştirilmiştir.  S256 ve düz metin code_challenges v1.0 ve v2.0 uç noktaları üzerinde desteklenir. 

Daha fazla bilgi için bkz: isteği bir yetkilendirme kodu[](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-protocols-oauth-code#request-an-authorization-code). 

 
---
 

### <a name="support-for-provisioning-all-user-attribute-values-available-in-the-workday-getworkers-api"></a>Tüm kullanıcı öznitelik değerleri Workday Get_Workers API kullanılabilir sağlama desteği

**Tür:** yeni özellik  
**Servis kategorisi:** uygulama sağlama  
**Ürün yetenek:** 3 taraf tümleştirme
 
Gelen Workday'deki Active Directory ve Azure AD şimdi sağlama genel önizleme özelliği ayıklamak ve tüm öznitelik değerleri Workday Get_Workers API kullanılabilir sağlama destekler. Bu ek standart yüzlerce için desteklenen ekler ve özel öznitelikler gününün ilk sürüm ile birlikte gelen açıkladıklarımızın çok ötesinde gelen bağlayıcı sağlama.

Daha fazla bilgi için bkz: [Workday kullanıcı özniteliklerinin listesi özelleştirme](https://docs.microsoft.com/azure/active-directory/active-directory-saas-workday-inbound-tutorial#customizing-the-list-of-workday-user-attributes)

---



### <a name="changing-group-membership-from-dynamic-to-static-and-vice-versa"></a>Grup üyeliğini dinamik gelen statik ve tersi yönde değiştirme

**Tür:** yeni özellik  
**Servis kategorisi:** Grup Yönetimi  
**Ürün yetenek:** işbirliği
 
Bir gruptaki üyelik nasıl yönetilir değiştirmek mümkündür. Grubuna varolan tüm başvuruların hala geçerli olduğundan için aynı grup adı ve kimliği sistemde tutmak istediğiniz gerektiğinde bu faydalıdır; Yeni grup oluşturma bu başvuruları güncelleştirilmesi gerekir.
Bu işlevsellik şu desteği eklemek için Azure AD Yönetim Merkezi güncelleştirdik. Şimdi, müşterilerin mevcut grupları dinamik üyeliğinden atanan üyeliği ve tam tersini dönüştürebilirsiniz. Varolan PowerShell cmdlet'leri de hala kullanılabilir.

Daha fazla bilgi için bkz: [statik ve tam tersini dinamik üyelik değiştirme](https://docs.microsoft.com/azure/active-directory/active-directory-groups-dynamic-membership-azure-portal#changing-dynamic-membership-to-static-and-vice-versa)

 

 
---
 

### <a name="improved-sign-out-behavior-with-seamless-sso"></a>Sorunsuz SSO ile geliştirilmiş oturum kapatma davranışı

**Tür:** değiştirilen özelliği  
**Servis kategorisi:** kimlik doğrulamaları (oturum açma bilgileri)  
**Ürün yetenek:** kullanıcı kimlik doğrulaması
 
Kullanıcılar, Azure AD tarafından güvenli hale getirilmiş bir uygulamaya dışında açıkça imzalı olsa bile, daha önce bunlar otomatik olarak yeniden kendi corpnet içinde Azure AD uygulaması etki alanına katılmış cihazlarından erişmeleri çalışıyorsanız geri sorunsuz SSO kullanarak imzalanması. Bu değişiklikle, oturum kapatma desteklenir.  Bu aynı veya farklı Azure seçmelerini sağlar AD hesabı geri, otomatik olarak sorunsuz SSO kullanarak oturum açmayı yerine oturum açın.

Daha fazla bilgi için bkz: [Azure Active Directory sorunsuz çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso)

 
---
 

### <a name="application-proxy-connector-version-154020-released"></a>Uygulama Proxy Bağlayıcısı sürümü yayımlanan 1.5.402.0

**Tür:** değiştirilen özelliği  
**Servis kategorisi:** uygulama proxy'si  
**Ürün yetenek:** kimlik güvenlik ve koruma
 
Bu bağlayıcı sürüm kademeli olarak Kasım alınmakta. Bu yeni Bağlayıcısı sürüm aşağıdaki değişiklikleri içerir:

- Bağlayıcı artık etki alanı düzeyi tanımlama bilgilerini bunun yerine alt etki alanı düzeyi ayarlar. Bu sorunsuz SSO bir deneyim sağlar ve yedek kimlik doğrulama istekleri önler.
- Öbekli kodlama istekleri için destek
- Geliştirilmiş bağlayıcı sistem durumu izleme 
- Çeşitli hata düzeltmeleri ve kararlılık iyileştirmeleri

Daha fazla bilgi için bkz: [anlamak Azure AD uygulama proxy'si Bağlayıcılar](https://docs.microsoft.com/azure/active-directory/application-proxy-understand-connectors).

 
---
 

 



## <a name="february-2018"></a>Şubat 2018
 

### <a name="improved-navigation-for-managing-users-and-groups"></a>Kullanıcıları ve grupları yönetmek için geliştirilmiş gezinme

**Tür:** değişiklik planı  
**Servis kategorisi:** dizin yönetimi  
**Ürün yetenek:** dizini
 

Kullanıcıları ve grupları yönetmek için gezinme deneyimi basitleştirilmiştir. Artık dizin genel bakış'tan doğrudan silinen kullanıcıların listesini daha kolay erişimi olan tüm kullanıcıların listesini gidebilirsiniz. Grup Yönetimi Ayarları kolay erişim olan tüm gruplar listesine doğrudan dizin genel bakış'tan de gidebilirsiniz. Ve ayrıca dizin genel bakış sayfasında, bir kullanıcı, Grup, Kurumsal uygulama veya uygulama kaydı için arama yapabilirsiniz.
 

---


### <a name="availability-of-sign-ins-and-audit-reports-in-microsoft-azure-operated-by-21vianet-azure-china-21vianet"></a>Oturum açma işlemleri ve denetim kullanılabilirliğini raporlar 21Vianet tarafından işletilen Microsoft Azure (Azure Çin'de 21Vianet)

**Tür:** yeni özellik  
**Servis kategorisi:** Sovereign Bulutlar  
**Ürün yetenek:** izleme ve Raporlama
 

Azure AD etkinlik günlüğü raporları şimdi 21Vianet (Azure Çin'de 21Vianet) örnekleri tarafından işletilen Microsoft Azure mevcuttur. Günlükleri eklenmiştir:

- **Oturum açma işlemleri etkinlik günlükleri** -tüm oturum açma işlemleri içerir, Kiracı ile ilişkilendirilen günlükleri.

- **Self Servis parola denetim günlüklerini** -tüm SSPR denetim günlüklerini içerir.

- **Dizin Yönetimi denetim günlüklerini** -ilişkili tüm dizin yönetimi içerir denetim günlüklerini kullanıcı yönetimi, uygulama yönetimi ve diğerleri gibi.

Bu günlükler ile ortamınızı nasıl çalıştığını içine Öngörüler elde edebilirsiniz. Sağlanan verilerle:

- Uygulamaları ve Hizmetleri kullanıcılarınız tarafından nasıl yararlanılmıştır belirler.

- Kullanıcılarınızın işlerini alma engelleme sorunları giderin.

Bu raporların nasıl kullanılacağı hakkında daha fazla bilgi için bkz: [Azure Active Directory raporlama](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal).
 

---


### <a name="use-report-reader-role-non-admin-role-to-view-azure-ad-activity-reports"></a>Azure AD etkinlik raporları görüntülemek için "Rapor okuyucu" rolünü (yönetici olmayan rolü) kullanın

**Tür:** yeni özellik  
**Servis kategorisi:** raporlama  
**Ürün yetenek:** izleme ve Raporlama
 

Parçası olmayan yönetici rolleri Azure AD etkinlik erişimi etkinleştirmek için müşteri geri bildirim günlükleri gibi biz erişim gerçekleştirilen oturum açma ve Azure Portal yanı sıra bizim grafik API'lerini kullanarak içinde denetim etkinlik için "Rapor okuyucu" rolündeki kullanıcılar için özelliğini etkinleştirdiniz. 

Daha fazla bilgi için bu raporları kullanma hakkında [Azure Active Directory raporlama](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal). 

---
 


### <a name="employeeid-claim-available-as-user-attribute-and-user-identifier"></a>Kullanıcı özniteliği ve kullanıcı tanımlayıcısı olarak kullanılabilir EmployeeID talep

**Tür:** yeni özellik  
**Servis kategorisi:** Kurumsal uygulamaları  
**Ürün yetenek:** SSO
 

Yapılandırabileceğiniz **EmployeeID** üye kullanıcılar ve uygulamalarda SAML tabanlı oturum açma kullanıcı Arabirimi Kurumsal uygulamadan B2B konuklar için kullanıcı özniteliği ve kullanıcı tanımlayıcısı olarak.

Daha fazla bilgi için bkz: [Azure Active Directory'de kurumsal uygulamalar için SAML belirtecinde verilen talepler özelleştiriliyor](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization).
 

---


### <a name="simplified-application-management-using-wildcards-in-azure-ad-application-proxy"></a>Azure AD uygulama proxy'si joker karakterler kullanarak Basitleştirilmiş uygulama yönetimi

**Tür:** yeni özellik  
**Servis kategorisi:** uygulama proxy'si  
**Ürün yetenek:** kullanıcı kimlik doğrulaması
 

Uygulama dağıtımı kolaylaştırmak ve yönetim yükünüzü azaltmak için artık joker karakterler kullanarak uygulama Yayımlama özelliği desteklenmektedir. Joker uygulama yayımlamak için standart uygulama yayımlama akış izleyin, ancak iç ve dış URL'ler içinde bir joker karakter kullanın.

Daha fazla bilgi için bkz: [joker uygulamalarında Azure Active Directory Uygulama proxy'si](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-wildcard)

 

---
 
### <a name="new-cmdlets-to-support-configuration-of-application-proxy"></a>Uygulama proxy'si yapılandırmasını desteklemek için yeni cmdlet'leri

**Tür:** yeni özellik  
**Servis kategorisi:** uygulama proxy'si  
**Ürün yetenek:** Platform
 

En son sürümünü Azuread'i PowerShell Önizleme modülü uygulama Proxy PowerShell kullanarak uygulamalarını yapılandırmak müşterilerin sağlayan yeni cmdlet'ler içerir.

Yeni cmdlet'leri şunlardır: 

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
 

### <a name="new-cmdlets-to-support-configuration-of-groups"></a>Grupları yapılandırmasını desteklemek için yeni cmdlet'leri

**Tür:** yeni özellik  
**Servis kategorisi:** uygulama proxy'si  
**Ürün yetenek:** Platform
 

En son sürümü Azuread'i PowerShell modülünün Azure AD'de grupları yönetmek için cmdlet'leri içerir. Bu cmdlet'ler AzureADPreview modülünde önceden kullanılabilir ve şimdi Azuread'i modülü eklenir

Şimdi sürüm genel kullanılabilirlik için olan Grup cmdlet'leri şunlardır: 

- Get-AzureADMSGroup
- New-AzureADMSGroup
- Remove-AzureADMSGroup
- Set-AzureADMSGroup
- Get-AzureADMSGroupLifecyclePolicy
- AzureADMSGroupLifecyclePolicy yeni
- Remove-AzureADMSGroupLifecyclePolicy
- Ekleme AzureADMSLifecyclePolicyGroup
- Remove-AzureADMSLifecyclePolicyGroup
- Sıfırlama AzureADMSLifeCycleGroup   
- Get-AzureADMSLifecyclePolicyGroup
 

---
 
### <a name="a-new-release-of-azure-ad-connect-is-available"></a>Azure AD Connect yeni bir sürüm kullanılabilir

**Tür:** yeni özellik  
**Servis kategorisi:** AD eşitleme  
**Ürün yetenek:** Platform
 

Azure AD Connect, Azure AD arasında ve Windows Server Active Directory ve LDAP dahil olmak üzere şirket içi veri kaynakları ile veri eşitlemek için tercih edilen bir araçtır.

**Önemli**
 
Bu yapı şema ve eşitleme tanıtır kural değişiklikleri. Azure AD Connect eşitleme hizmeti bir yükseltmeden sonra bir tam içeri aktarma ve tam eşitleme adımları tetikler. Bu davranış değiştirme hakkında daha fazla bilgi için bkz: [yükselttikten sonra tam eşitleme erteleme nasıl](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-upgrade-previous-version#how-to-defer-full-synchronization-after-upgrade).

Bu sürüm aşağıdaki güncelleştirmeler ve değişiklikler bulunur:

**Giderilen sorunlar**

- Sonraki sayfaya geçiş yaparken Paritition filtreleme sayfası için arka plan görevleri zamanlama penceresi düzeltin.
- ConfigDB özel eylemi sırasında erişim ihlali neden olan bir hata sabit.
- SQL bağlantı zaman aşımı kurtarmak için bir hata sabit.
- Bir hata olduğu SAN joker karakterlerle sertifikaları Önkoşul denetimi başarısız sabit.
- AAD bağlayıcı dışa aktarma sırasında miiserver.exe kilitlenme neden olan bir hata sabit.
- AAD çalıştırırken DC'de oturum hangi hatalı parola denemesi connect yapılandırmasını değiştirmek için Sihirbazı hatanın düzeltildiğini

**Yeni özellikleri ve geliştirmeleri**

- GDPR için biz (telemetri, sistem durumu, vb.), Microsoft ile paylaşılır müşteri veri türlerini ayrıntılı çevrimiçi belgeleri bağlantıları ve tercihleriniz değiştirmek için bir şekilde sağlamak göstermek için gereklidir.  Bu iade şunları ekler:
    - Veri paylaşımı ve gizlilik bildirimi temiz EULA sayfası yükleyin.

    - Veri paylaşımı ve gizlilik bildirimini yükseltme sayfasında.

    - Yeni bir ek görevi **gizlilik ayarları** burada kullanıcısı değiştirebilir tercihlerini.
 
- Uygulama telemetri - Yöneticiler bu sınıfın açık/kapalı veri geçiş yapabilirsiniz.

- Azure AD sistem durumu verileri - yöneticiler kendi sistem durumu ayarlarını denetlemek için sistem durumu portalını ziyaret edin gerekir. Hizmet İlkesi değiştirilmişse, aracıları okuyun ve onu zorlayın.

- Cihaz geri yazma yapılandırma işlemleri ve sayfa başlatma için bir ilerleme çubuğu eklendi.

- Genel tanılama HTML raporu ve ZIP metin tam veri toplama ile geliştirilmiş / HTML raporu.

- Geliştirilmiş güvenilirliğini otomatik yükseltme ve sunucusunun sistem durumunu belirlenebilir emin olmak için ek telemetri eklenir.

- Kullanılabilir AD Bağlayıcısı hesabındaki ayrıcalıklı hesaplara kısıtlayın. Yeni yüklemeler için sihirbazın hesapları ayrıcalıklı izinleri kısıtlar MSOL hesabı oluşturduktan sonra MSOL hesabında sahip. Değişiklikler express yüklemeleri ve otomatik olarak oluşturma hesabıyla özel yüklemeler etkiler.

- Yükleyici AADConnect temiz yüklemesini SA ayrıcalığına gerektirmeyecek şekilde değiştirildi.

- Belirli bir nesnesi için eşitleme sorunlarını gidermek için yeni yardımcı program'ı seçin. Şu anda, yardımcı program aşağıdakileri denetler:

    - Eşitlenen kullanıcı nesnesi ve Azure AD Kiracı Kullanıcı hesabında arasında UserPrincipalName uyuşmazlığı.
  
    - Etki alanı filtreleme nedeniyle gelen nesne filtre durumunda
  
    - Nesne filtreleme kuruluş birimi (OU) nedeniyle gelen filtre durumunda

- Belirli bir kullanıcı hesabı için şirket içi Active Directory içinde depolanan geçerli parola karması eşitlemek için yeni yardımcı program'ı seçin. Yardımcı program parola değişikliği gerekli değildir. 
 

---
 

### <a name="applications-supporting-intune-app-protection-policies-added-for-use-with-azure-ad-application-based-conditional-access"></a>Uygulamalar için destek Intune App koruma ilkeleri eklenen Azure AD uygulama tabanlı koşullu erişim ile kullanma

**Tür:** değiştirilen özelliği  
**Servis kategorisi:** koşullu erişim  
**Ürün yetenek:** kimlik güvenlik ve koruma
 

Uygulama tabanlı koşullu erişimi destekleyen daha fazla uygulama ekledik. Şimdi, Office 365 ve bu onaylanmış istemci uygulamalarını kullanarak diğer Azure AD bağlı bulut uygulamalarına erişim alabilirsiniz.

Aşağıdaki uygulamalar Şubat sonuna eklenir 

- Microsoft PowerBI

- Microsoft Başlatıcısı

- Microsoft faturalama

Daha fazla bilgi için bkz.

- [Onaylanmış istemci uygulama gereksinimi](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement)
- [Azure AD uygulama temelli koşullu erişim](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-mam)

 

---
 

### <a name="terms-of-use-update-to-mobile-experience"></a>Mobil deneyim için kullanım koşullarını güncelleştirin 

**Tür:** değiştirilen özelliği  
**Servis kategorisi:** kullanım koşulları  
**Ürün yetenek:** uyumluluk
 

Kullanım koşulları görüntülendiğinde, şimdi tıklayabilirsiniz **sorun görüntüleme sahip mi? Burayı**. Bu bağlantıya tıkladıklarında, Cihazınızda yerel kullanım koşullarını açar. Belge yazı tipi boyutu veya aygıt ekran boyutunu bağımsız olarak, yakınlaştırma ve gerektiğinde belgeyi okuyun. 
 

---
 
## <a name="january-2018"></a>Ocak 2018
 

### <a name="new-federated-apps-available-in-azure-ad-app-gallery"></a>Yeni Federal Apps mevcut Azure AD uygulama galerisinde 

**Tür:** yeni özellik  
**Servis kategorisi:** Kurumsal uygulamaları  
**Ürün yetenek:** 3 taraf tümleştirme
 

Ocak 2018 içinde Federasyon desteği aşağıdaki yeni uygulamalarla uygulama galerisinde eklendi:

[IBM OpenPages](https://go.microsoft.com/fwlink/?linkid=864698), [OneTrust gizlilik yönetim yazılımı](https://go.microsoft.com/fwlink/?linkid=861660), [Dealpath](https://go.microsoft.com/fwlink/?linkid=863526), [IriusRisk federe dizin](https://go.microsoft.com/fwlink/?linkid=864699) ve [doğruluk NetBenefits](https://go.microsoft.com/fwlink/?linkid=864701).

Tüm kullanılabilir öğreticileri eksiksiz bir genel bakış için bkz: [Azure Active Directory ile SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial).
 

---
 


### <a name="sign-in-with-additional-risk-detected"></a>Ek risk içeren oturumlar algılandı

**Tür:** yeni özellik  
**Servis kategorisi:** kimlik koruması  
**Ürün yetenek:** kimlik güvenlik ve koruma
 

Algılanan risk olayı için alma Insight Azure AD aboneliğinizi bağlıdır. Azure AD Premium P2 Edition'la tüm temel algılamaların hakkında en ayrıntılı bilgi alın.

Azure AD Premium P1 Edition'la lisansınız tarafından kapsanmayan algılamaların ile ek risk algılanan risk olayı olarak oturum açma görünür.

Daha fazla bilgi için bkz. [Azure Active Directory risk olayları](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-risk-events).
 

---

### <a name="hide-office-365-applications-from-end-users-access-panels"></a>Son kullanıcının erişim paneller uygulamalardan Office 365 Gizle

**Tür:** yeni özellik  
**Servis kategorisi:** My uygulamalar  
**Ürün yetenek:** SSO
 

Artık, yeni bir kullanıcı ayarı yoluyla, kullanıcının erişim paneller üzerinde nasıl Office 365 uygulamaları görünmesini daha iyi yönetebilirsiniz. Bu seçenek, yalnızca Office uygulamaları Office portalındaki göstermek tercih ederseniz, bir kullanıcının erişim paneller uygulamalarında miktarını azaltmak için yararlıdır. Ayar bulunan **kullanıcı ayarları** ve etiketli **kullanıcılar yalnızca Office 365 uygulamaları Office 365 portalında görür**.
 

Daha fazla bilgi için bkz: [bir uygulamayı Azure Active Directory'de kullanıcı deneyimini Gizle](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-hide-third-party-app).

---
 


### <a name="seamless-sign-into-apps-enabled-for-password-sso-directly-from-apps-url"></a>Sorunsuz oturum doğrudan uygulamanın URL'den parola SSO etkin uygulamalar açın 

**Tür:** yeni özellik  
**Servis kategorisi:** My uygulamalar  
**Ürün yetenek:** SSO
 

My uygulamaları tarayıcı uzantısı tarayıcınızda bir kısayol olarak yeteneği üzerinde My uygulamaları çoklu oturum verir kullanışlı bir aracı üzerinden kullanıma sunulmuştur. Kullanıcının yükleme tarayıcısında waffle simgesini görürsünüz sonra bunları uygulamaları için hızlı erişim sağlar. Kullanıcılar artık yararlanabilirsiniz:

- Uygulamanın oturum açma sayfasından doğrudan parola SSO tabanlı uygulamalarda oturum açmak için özelliği
- Hızlı arama özelliğini kullanarak herhangi bir uygulama başlatma
- Uzantı son kullanılan uygulamalar için kısayollar
- Uzantı Edge, Chrome ve Firefox için kullanılabilir.
 
Daha fazla bilgi için bkz: [My uygulamaları güvenli oturum açma uzantısı](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction#my-apps-secure-sign-in-extension).

---

### <a name="azure-ad-administration-experience-in-azure-classic-portal-has-been-retired"></a>Azure AD yönetim deneyimi Klasik Azure portalındaki Çekildi

**Tür:** kullanım dışı   
**Servis kategorisi:** Azure AD  
**Ürün yetenek:** dizini
 

8 Ocak 2018'den itibaren Azure AD Yönetimi itibariyle Klasik Azure portalındaki deneyimi Çekildi. Bu, Klasik Azure portalı kendisini bırakma birlikte yapıldı. İleride, kullanmanız gereken [Azure AD Yönetim Merkezi](https://aad.portal.azure.com) tüm, portal tabanlı yönetim için Azure ad.
 
---

### <a name="the-phonefactor-web-portal-has-been-retired"></a>PhoneFactor web portalı kullanımdan kaldırıldı

**Tür:** kullanım dışı  
**Servis kategorisi:** Azure AD  
**Ürün yetenek:** dizini
 

8 Ocak 2018 itibariyle, PhoneFactor web Portalı'nı devre dışı bırakılmış. Bu portalı MFA sunucusunun yönetimi için kullanılan, ancak bu işlevlerin portal.azure.com Azure portalında içine taşınmıştır. 

MFA yapılandırma bulunur: **Azure Active Directory \> MFA sunucusu**
 
---
 
### <a name="deprecate-azure-ad-reports"></a>Azure AD raporları Kaldır


**Tür:** kullanım dışı  
**Servis kategorisi:** raporlama  
**Ürün yetenek:** kimlik yaşam döngüsü yönetimi  


Yeni Azure Active Directory Yönetim Konsolu etkinliği ve güvenlik raporları için raporu API'leri artık kullanılabilir yeni API'leri ve genel kullanılabilirlik ile altında "/ Raporları" uç nokta 31 Aralık 2017 sonuna sürümünden itibaren kullanımdan kaldırılmıştır.


**Kullanılabilir nedir?**

Yeni yönetim konsoludur geçiş işleminin bir parçası olarak, biz yerine Azure AD etkinlik günlükleri almak için kullanılabilir 2 yeni API'leri yaptınız. Yeni API kümesi daha zengin filtreleme ve sıralama daha zengin denetim ve oturum açma etkinliklerini sağlama ek işlevsellik sağlar. Güvenlik raporları önceden mevcut verileri artık Microsoft Graph kimlik koruması risk olayları API aracılığıyla erişilebilir.

Daha fazla bilgi için bkz.

- [Azure Active raporlama API'si Directory ile çalışmaya başlama](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started-azure-portal)

- [Azure Active Directory kimlik koruması ve Microsoft Graph kullanmaya başlama](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection-graph-getting-started)


---


## <a name="december-2017"></a>Aralık 2017
 

### <a name="terms-of-use-in-the-access-panel"></a>Erişim paneli kullanım koşulları

**Tür:** yeni özellik  
**Servis kategorisi:** kullanım koşulları  
**Ürün yetenek:** uyumluluk
 
Şimdi erişim Masası'na gidin ve önceden kabul kullanım şartlarını görüntüleyin.

Şu adımları uygulayın:

1. Git [MyApps portal](https://myapps.microsoft.com)ve oturum açın.

2. Sağ üst köşedeki adınızın seçin ve ardından **profil** listeden. 

3. Üzerinde **profil**seçin **kullanım koşulları gözden**. 

4. Kullanım koşullarını gözden geçirebilirsiniz artık kabul. 

Daha fazla bilgi için bkz: [kullanım özelliği (Önizleme) Azure AD koşullarını](https://docs.microsoft.com/azure/active-directory/active-directory-tou).
 
---
 

### <a name="new-azure-ad-sign-in-experience"></a>Yeni Azure AD oturum açma deneyimi

**Tür:** yeni özellik  
**Servis kategorisi:** Azure AD  
**Ürün yetenek:** kullanıcı kimlik doğrulaması
 
Azure AD ve Microsoft hesabı kimlik sistemi Uı'lar tutarlı bir görünüm olduğundan şekilde tasarlanmıştır. Ayrıca, Azure AD oturum açma sayfasında kullanıcı adı ilk olarak, ikinci bir ekranda kimlik bilgisi tarafından izlenen toplar.

Daha fazla bilgi için bkz: [yeni Azure AD oturum açma deneyiminin nasıl olduğu artık genel önizlemede](https://cloudblogs.microsoft.com/enterprisemobility/2017/08/02/the-new-azure-ad-signin-experience-is-now-in-public-preview/).
 
---
 

### <a name="fewer-sign-in-prompts-a-new-keep-me-signed-in-experience-for-azure-ad-sign-in"></a>Daha az sayıda oturum açma komut istemleri: Azure AD oturum açma için yeni "Oturumumu açık bırak" deneyimi

**Tür:** yeni özellik  
**Servis kategorisi:** Azure AD  
**Ürün yetenek:** kullanıcı kimlik doğrulaması
 
**Oturumumu açık bırak** onay kutusunu Azure AD oturum açma sayfasında, kimlik doğrulamasını başarıyla sonra gösteren yukarı yeni bir istemiyle değişti. 

Yanıt, **Evet** bu komut istemini hizmeti, bir sürekli yenileme belirteci sağlar. Bu davranış seçili olduğunda aynıdır **Oturumumu açık bırak** eski deneyimi onay kutusuna. Başarıyla Federasyon Hizmeti ile kimlik doğrulaması sonra Federasyon kiracıları için bu istemi gösterir.

Daha fazla bilgi için bkz: [daha az sayıda oturum açma komut istemleri: yeni "Oturumumu açık bırak" Azure AD önizlemede deneyimidir](https://cloudblogs.microsoft.com/enterprisemobility/2017/09/19/fewer-login-prompts-the-new-keep-me-signed-in-experience-for-azure-ad-is-in-preview/). 

---
 

### <a name="add-configuration-to-require-the-terms-of-use-to-be-expanded-prior-to-accepting"></a>Kabul etmeden önce genişletilecek kullanım koşullarını gerektirecek şekilde yapılandırması Ekle

**Tür:** yeni özellik  
**Servis kategorisi:** kullanım koşulları  
**Ürün yetenek:** uyumluluk
 
Yöneticiler için bir seçenek koşulları kabul etmeden önce Kullanım Koşulları'nı genişletin, kullanıcılar gerektirir.

Şunlardan birini seçin **üzerinde** veya **kapalı** kullanıcıların Kullanım Koşulları'nı genişletin. **Üzerinde** ayarı kabul etmeden önce kullanım koşullarını görüntülemek kullanıcıların gerektirir.

Daha fazla bilgi için bkz: [kullanım özelliği (Önizleme) Azure AD koşullarını](https://docs.microsoft.com/azure/active-directory/active-directory-tou).
 
---
 

### <a name="scoped-activation-for-eligible-role-assignments"></a>Uygun rol atamaları için kapsamlı etkinleştirme

**Tür:** yeni özellik  
**Servis kategorisi:** Privileged Identity Management  
**Ürün yetenek:** Privileged Identity Management
 
Uygun Azure kaynak rol atamalarını özgün atama varsayılan değerinden daha az otonomisi ile etkinleştirmek için kapsamlı etkinleştirmesini kullanabilirsiniz. Kiracınızda atadığınız bir aboneliğin sahibi bir örnektir. Kapsamlı etkinleştirme ile (örneğin, kaynak gruplarını ve sanal makineler) aboneliği kapsamında yer alan en fazla beş kaynaklar için sahip rolünü de etkinleştirebilirsiniz. Etkinleştirme kapsamı kritik Azure kaynaklarına istenmeyen değişiklikler Yürütülüyor olasılığını azaltabilir.

Daha fazla bilgi için bkz: [Azure AD Privileged Identity Management nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure).
 
---
 

### <a name="new-federated-apps-in-the-azure-ad-app-gallery"></a>Azure AD uygulama galerisinde yeni Federal apps

**Tür:** yeni özellik  
**Servis kategorisi:** Kurumsal uygulamaları  
**Ürün yetenek:** 3 taraf tümleştirme
 
Aralık 2017 ' aşağıdaki yeni uygulamalar Federasyonu Desteği ile uygulama galerisinde eklendi:

[Accredible](https://go.microsoft.com/fwlink/?linkid=863523), Adobe deneyimi Yöneticisi [EFI dijital mağaza](https://go.microsoft.com/fwlink/?linkid=861685), [Communifire](https://go.microsoft.com/fwlink/?linkid=861676) CybSafe, [FactSet](https://go.microsoft.com/fwlink/?linkid=863525), [görüntü WORKS](https://go.microsoft.com/fwlink/?linkid=863517), [MOBI](https://go.microsoft.com/fwlink/?linkid=863521), [MobileIron Azure AD tümleştirme](https://go.microsoft.com/fwlink/?linkid=858027), [Reflektive](https://go.microsoft.com/fwlink/?linkid=863518), [SAML SSO GmbH çözünürlüğün Bambu için](https://go.microsoft.com/fwlink/?linkid=863520), [SAML SSO GmbH çözünürlüğün Bitbucket için](https://go.microsoft.com/fwlink/?linkid=863519), [Vodeclic](https://go.microsoft.com/fwlink/?linkid=863522), WebHR, Zenegy Azure AD tümleştirme.

Tüm kullanılabilir öğreticileri eksiksiz bir genel bakış için bkz: [Azure Active Directory ile SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial).

 
---
 

### <a name="approval-workflows-for-azure-ad-directory-roles"></a>Azure AD directory rolleri onay iş akışları

**Tür:** değiştirilen özelliği  
**Servis kategorisi:** Privileged Identity Management  
**Ürün yetenek:** Privileged Identity Management
 
Azure AD directory rolleri için onay iş akışı genel kullanıma açıktır.

Ayrıcalıklı rol kullanmadan önce onay iş akışıyla ayrıcalıklı rol yöneticileri rol etkinleştirme isteği için uygun rolü üyesi gerektirebilir. Birden çok kullanıcı ve grupları temsilci onay sorumlulukları olabilir. Uygun Rol üyeleri onay tamamlandı ve rolleri etkin olduğunda alın.

---
 

### <a name="pass-through-authentication-skype-for-business-support"></a>Doğrudan kimlik doğrulama: Skype iş desteği

**Tür:** değiştirilen özelliği  
**Servis kategorisi:** kimlik doğrulamaları (oturum açma bilgileri)  
**Ürün yetenek:** kullanıcı kimlik doğrulaması


Şimdi doğrudan kimlik doğrulama, çevrimiçi içerir modern kimlik doğrulamasını destekleyen iş istemci uygulamaları ve karma topolojiler için Skype kullanıcı oturum açma işlemlerine destekler. 

Daha fazla bilgi için bkz: [Skype Kurumsal topolojileri modern kimlik doğrulaması ile desteklenen](https://technet.microsoft.com/library/mt803262.aspx).
 
---
 

### <a name="updates-to-azure-ad-privileged-identity-management-for-azure-rbac-preview"></a>Azure RBAC (Önizleme) için Azure AD Privileged Identity Management güncelleştirmeleri

**Tür:** değiştirilen özelliği  
**Servis kategorisi:** Privileged Identity Management  
**Ürün yetenek:** Privileged Identity Management
 
Azure AD Privileged Identity Management (PIM) Azure rol tabanlı erişim denetimi (RBAC) için genel Önizleme yenilenmesini ile şunları yapabilirsiniz:

* Tam yetecek kadar yönetim kullanın.
* Kaynak rolleri etkinleştirmek için onay gerektirir.
* Gelecekteki bir etkinleştirme hem Azure AD için onay gerektiren bir rolü ve Azure RBAC rollerini planlayın.

 
Daha fazla bilgi için bkz: [Azure kaynakları (Önizleme) için Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac).

 
---
 
## <a name="november-2017"></a>Kasım 2017
 
### <a name="access-control-service-retirement"></a>Erişim denetimi hizmetini devre dışı bırakma



**Tür:** değişiklik planı  
**Servis kategorisi:** erişim denetimi hizmeti  
**Ürün yetenek:** erişim denetimi hizmeti 


 Azure Active Directory erişim denetimi (erişim denetimi hizmeti olarak da bilinir) içinde geç 2018 kullanımdan kaldırılacaktır. Ayrıntılı zamanlama ve üst düzey Geçiş Kılavuzu içeren daha fazla bilgi sonraki birkaç hafta içinde sağlanacaktır. Erişim denetimi hizmeti ile ilgili herhangi bir sorunuz ile bu sayfada yorum bırakabilir ve bir ekip üyesine yanıt.

---

### <a name="restrict-browser-access-to-the-intune-managed-browser"></a>Intune yönetilen tarayıcı için tarayıcı erişimi kısıtlama 


**Tür:** değişiklik planı  
**Servis kategorisi:** koşullu erişim  
**Ürün yetenek:** kimlik güvenlik ve koruma




Onaylanan bir uygulama olarak Intune yönetilen tarayıcı kullanarak, Office 365 ve diğer Azure AD bağlı bulut uygulamaları için tarayıcı erişimi kısıtlayabilirsiniz. 

Uygulama bağlı olarak koşullu erişim için aşağıdaki koşul artık yapılandırabilirsiniz:

**İstemci uygulamaları:** tarayıcı

**Değişiklik etkisi nedir?**

Günümüzde, bu koşulu kullandığınızda erişimi engellenir. Önizleme kullanılabilir olduğunda, tüm erişim yönetilen tarayıcı uygulaması kullanımını gerektirir. 

Bu yetenek ve yaklaşan blogları ve sürüm notları hakkında daha fazla bilgi arayın. 

Daha fazla bilgi için bkz: [koşullu erişim Azure AD'de](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal).

 
---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>Azure AD uygulama tabanlı koşullu erişim için yeni onaylanmış istemci uygulamaları

 
**Tür:** değişiklik planı  
**Servis kategorisi:** koşullu erişim  
**Ürün yetenek:** kimlik güvenlik ve koruma




Aşağıdaki uygulamalar listesine eklemek için planlanan [istemci uygulamaları onaylanan](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement):

- [Microsoft Kaizala](https://www.microsoft.com/garage/profiles/kaizala/)
- [Microsoft StaffHub](https://staffhub.office.com/what-it-is)


Daha fazla bilgi için bkz.

- [Onaylanmış istemci uygulama gereksinimi](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement)
- [Azure AD uygulama temelli koşullu erişim](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-mam)


---

### <a name="terms-of-use-support-for-multiple-languages"></a>Kullanım Koşulları'nı birden çok dil desteği



**Tür:** yeni özellik    
**Servis kategorisi:** kullanım koşulları  
**Ürün yetenek:** uyumluluk





Yöneticiler artık birden çok PDF belgelerini içeren yeni kullanım koşulları oluşturabilirsiniz. Bu PDF belgeleri karşılık gelen dil olan etiketleyebilirsiniz. Kullanıcılara PDF tercihlerine göre eşleşen dil ile gösterilir. Eşleşme yoksa, varsayılan dil gösterilir.


---
 

### <a name="real-time-password-writeback-client-status"></a>Gerçek zamanlı parola geri yazma istemci durumu



**Tür:** yeni özellik  
**Servis kategorisi:** Self Servis parola sıfırlama  
**Ürün yetenek:** kullanıcı kimlik doğrulaması


 

Artık, şirket içi parola geri yazma istemci durumunu gözden geçirebilirsiniz. Bu seçenek kullanılabilir **şirket içi tümleştirme** bölümünü [parola sıfırlama](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/PasswordReset) sayfası. 

Şirket içi geri yazma istemciye bağlantınızda bir sorun varsa, sizinle sağlayan bir hata iletisi görüntülenir:

- Şirket içi geri yazma istemciniz neden bağlanamıyor hakkında bilgi.
- Bir bağlantı belgelerine sorunun çözümlenmesinde yardımcı olur. 


Daha fazla bilgi için bkz: [şirket içi tümleştirme](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-how-it-works#on-premises-integration).

 
---


### <a name="azure-ad-app-based-conditional-access"></a>Azure AD uygulama temelli koşullu erişim 



 
**Tür:** yeni özellik  
**Servis kategorisi:** Azure AD  
**Ürün yetenek:** kimlik güvenlik ve koruma





Şimdi Office 365 ve diğer Azure AD bağlı bulut uygulamalarına erişimi kısıtlayabilirsiniz [istemci uygulamaları onaylanan](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement) destekleyen Intune uygulama koruma ilkeleri kullanarak [Azure AD uygulama temelli koşullu erişim](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-mam). Intune uygulama koruma ilkeleri yapılandırmak ve bu istemci uygulamaları üzerindeki şirket verilerini korumak için kullanılır.

Birleştirme tarafından [uygulama tabanlı](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-mam) ile [aygıt tabanlı](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-policy-connected-applications) koşullu erişim ilkeleri, kişisel verileri ve şirket cihazları korumak için esnekliğe sahip.

Aşağıdaki koşullar ve denetimleri artık uygulama bağlı olarak koşullu erişim ile kullanılmak üzere kullanılabilir:

**Desteklenen bir platform koşulu**

- iOS
- Android

**İstemci uygulamaları koşulu**

- Mobil uygulamalar ve masaüstü istemcileri

**Erişim denetimi**

- Onaylı istemci uygulaması gerektir


Daha fazla bilgi için bkz: [Azure AD uygulama temelli koşullu erişim](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-mam).

 
---

### <a name="manage-azure-ad-devices-in-the-azure-portal"></a>Azure portalında Azure AD aygıtları yönetme



**Tür:** yeni özellik  
**Servis kategorisi:** cihaz kaydı ve Yönetimi  
**Ürün yetenek:** kimlik güvenlik ve koruma

 



Tüm cihazlar için Azure AD bağlı artık bulabilirsiniz ve cihaz ilgili etkinlikleri tek bir yerde. Tüm cihaz kimliklerini ve Azure portalında ayarlarını yönetmek için yeni bir yönetim deneyimi yoktur. Bu sürümde, şunları yapabilirsiniz:

- Azure AD içinde koşullu erişim için uygun olan tüm cihazlarınıza görüntüleyin.
- Karma Azure dahil özellikleri görüntüle AD alanına katılmış aygıtlar.
- Azure AD alanına katılmış cihazlar için BitLocker anahtarları bulmak, Cihazınızı Intune ve daha fazla ile yönetebilirsiniz.
- Azure AD cihaz ilgili ayarlarını yönetin.

Daha fazla bilgi için bkz: [Azure portalını kullanarak cihazları yönetme](https://docs.microsoft.com/azure/active-directory/device-management-azure-portal).



 
---

### <a name="support-for-macos-as-a-device-platform-for-azure-ad-conditional-access"></a>MacOS olarak Azure AD koşullu erişim için bir cihaz platform desteği 



**Tür:** yeni özellik    
**Servis kategorisi:** koşullu erişim  
**Ürün yetenek:** kimlik güvenlik ve koruma 
 

Artık içerir (dışlamak macOS cihaz platformu koşul olarak Azure AD koşullu erişim ilkenizi veya). Desteklenen cihaz platformlarının macOS eklenmesi ile şunları yapabilirsiniz:

- **Kaydedebilir ve macOS cihazları Intune kullanarak yönetebilirsiniz.** İOS ve Android gibi diğer platformlarda benzeyen, şirket portal uygulaması birleşik kayıtları yapmak için macOS için kullanılabilir. Bir cihazı Intune hizmetine kaydetmek ve Azure AD ile kaydetmek için yeni şirket portalı uygulamasını macOS için kullanabilirsiniz.
- **MacOS cihazları Intune'a tanımlanan kuruluşunuzun uyumluluk ilkelerine uymaları emin olun.** Azure portalındaki Intune'da şimdi macOS cihazları için Uyumluluk ilkeleri ayarlayabilirsiniz. 
- **Erişimi yalnızca uyumlu macOS cihazlara Azure AD'de uygulamalara sınırlayın.** Koşullu erişim ilkesi yazma macOS birbirinden ayrı cihaz platformu seçeneği olarak sahiptir. Artık Azure ayarlamak hedeflenen uygulama için macOS özgü koşullu erişim ilkeleri yazabilirsiniz.

Daha fazla bilgi için bkz.

- [Intune ile macOS cihazları için cihaz uyumluluğu ilkesi oluşturma](https://aka.ms/macoscompliancepolicy)
- [Azure AD'de koşullu erişim](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)


 
---

### <a name="network-policy-server-extension-for-azure-multi-factor-authentication"></a>Azure çok faktörlü kimlik doğrulaması için ağ ilkesi sunucusu uzantısı 


**Tür:** yeni özellik    
**Servis kategorisi:** çok faktörlü kimlik doğrulaması  
**Ürün yetenek:** kullanıcı kimlik doğrulaması




Azure çok faktörlü kimlik doğrulaması için ağ ilkesi sunucusu uzantısı, var olan sunucularınızı kullanarak, kimlik doğrulaması altyapınız için bulut tabanlı çok faktörlü kimlik doğrulama özellikleri ekler. Ağ İlkesi Sunucusu uzantısı ile var olan kimlik doğrulama akışı telefon araması, SMS mesajı ya da telefon uygulama doğrulama ekleyebilirsiniz. Yükleme, yapılandırma ve yeni sunucuları sağlamak zorunda değilsiniz. 

Bu uzantı, sanal özel ağ bağlantıları Azure çok faktörlü kimlik doğrulama sunucusu dağıtmadan korumak istediğiniz kuruluşlar için oluşturuldu. Ağ ilkesi sunucusu arasındaki RADIUS Azure çok faktörlü kimlik doğrulaması bulut tabanlı bir ikinci faktör kimlik doğrulaması sağlamak için bir bağdaştırıcı uzantısı görür federe veya kullanıcılar eşitlenir.


Daha fazla bilgi için bkz: [varolan ağ ilkesi sunucusu altyapınızı Azure çok faktörlü kimlik doğrulamasıyla tümleştirmek](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-nps-extension).

 
---

### <a name="restore-or-permanently-remove-deleted-users"></a>Geri yüklemek veya silinen kullanıcılar kalıcı olarak kaldırma


**Tür:** yeni özellik    
**Servis kategorisi:** kullanıcı yönetimi  
**Ürün yetenek:** dizini 



Azure AD Yönetim merkezinde, şunları yapabilirsiniz:

- Silinmiş bir kullanıcı geri yükleyin. 
- Bir kullanıcıyı kalıcı olarak sil.


**Deneyin için:**

1. Azure AD Yönetim Merkezi'nde seçin [tüm kullanıcılar](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/All) içinde **Yönet** bölümü. 

2. Gelen **Göster** listesinde **son kullanıcılar'ı silinmiş**. 

3. Bir veya daha fazla yakın zamanda silinmiş kullanıcı seçin ve ardından ya da geri yükleyebilir veya kalıcı olarak silmek.

 
---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>Azure AD uygulama tabanlı koşullu erişim için yeni onaylanmış istemci uygulamaları

 
**Tür:** değiştirilen özelliği  
**Servis kategorisi:** koşullu erişim  
**Ürün yetenek:** kimlik güvenlik ve koruma


Aşağıdaki uygulamalar listesine eklenen [istemci uygulamaları onaylanan](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement):

- Microsoft Planlayıcısı
- Azure Information Protection 


Daha fazla bilgi için bkz.

- [Onaylanmış istemci uygulama gereksinimi](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement)
- [Azure AD uygulama temelli koşullu erişim](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-mam)


---

### <a name="use-or-between-controls-in-a-conditional-access-policy"></a>Kullanım koşullu erişim ilkesi denetimlerinde arasında "veya" 


**Tür:** değiştirilen özelliği    
**Servis kategorisi:** koşullu erişim  
**Ürün yetenek:** kimlik güvenlik ve koruma

 
Şimdi kullanabilir "veya" (Seçili denetimleri birini gerektirir) koşullu erişim denetimleri için. Erişim denetimleri arasındaki ile "veya" ilkeleri oluşturmak için bu özelliği kullanın. Örneğin, uyumlu bir cihaz üzerinde olacak şekilde "veya" çok faktörlü kimlik doğrulaması kullanarak oturum açmak için kullanıcının gerektiren bir ilke oluşturmak için bu özelliği kullanabilirsiniz.

Daha fazla bilgi için bkz: [Azure AD koşullu erişim denetimleri](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-controls).

 
---

### <a name="aggregation-of-real-time-risk-events"></a>Gerçek zamanlı risk olaylarını toplama


**Tür:** değiştirilen özelliği    
**Servis kategorisi:** kimlik koruması  
**Ürün yetenek:** kimlik güvenlik ve koruma


Azure AD Identity Protection ' belirli bir günde aynı IP adresinden kaynaklanan tüm gerçek zamanlı risk olaylarını şimdi her risk olay türü için toplanır. Bu değişiklik, kullanıcı güvenlik herhangi bir değişiklik gösterilen risk olayı sınırlar.

Temel alınan gerçek zamanlı algılama kullanıcı oturum açtığında her zaman çalışır. Çok faktörlü kimlik doğrulaması veya erişimi engelleme için ayarlanmış bir oturum açma riski güvenlik ilkesi varsa, her riskli oturum açma sırasında hala tetiklenir.

 
---
 




## <a name="october-2017"></a>Ekim 2017


### <a name="deprecate-azure-ad-reports"></a>Azure AD raporları Kaldır


**Tür:** değişiklik planı  
**Servis kategorisi:** raporlama  
**Ürün yetenek:** kimlik yaşam döngüsü yönetimi  



Azure portal ile sağlar:

- Yeni bir Azure AD Yönetim Konsolu.
- Etkinlik ve güvenlik raporları yeni API'ler.
 
Bu yeni özellikleri nedeniyle raporun API'leri/Reports endpoint altında kullanımdan 10 Aralık 2017 üzerinde. 

---

### <a name="automatic-sign-in-field-detection"></a>Otomatik oturum açma alan algılama


**Tür:** sabit   
**Servis kategorisi:** My uygulamalar  
**Ürün yetenek:** çoklu oturum açma  



Azure AD, bir HTML kullanıcı adı ve parola alanı işleme uygulamaları için otomatik oturum açma alan algılama destekler. Bu adımları belgelenmiştir [otomatik olarak oturum açma alanları bir uygulama için yakalama](https://docs.microsoft.com/azure/active-directory/application-config-sso-problem-configure-password-sso-non-gallery#how-to-manually-capture-sign-in-fields-for-an-application). Ekleyerek bu yeteneği bulabilirsiniz bir *olmayan galeri* uygulaması **kurumsal uygulamalar** sayfasındaki [Azure portal](http://aad.portal.azure.com). Ayrıca, yapılandırabileceğiniz **çoklu oturum açma** bu yeni uygulama modu **parola tabanlı çoklu oturum açma**, bir web URL'si girin ve sayfayı kaydedin.
 
Bir hizmet sorunu nedeniyle bu işlevselliği geçici olarak devre dışı bırakıldı. Sorunu Çözümlendi ve otomatik oturum açma alan algılama yeniden kullanılabilir.

---

### <a name="new-multi-factor-authentication-features"></a>Yeni çok faktörlü kimlik doğrulaması özellikleri


**Tür:** yeni özellik  
**Servis kategorisi:** çok faktörlü kimlik doğrulaması  
**Ürün yetenek:** kimlik güvenlik ve koruma  



Çok faktörlü kimlik doğrulaması (MFA), kuruluşunuzun koruma önemli bir parçasıdır. Kimlik bilgileri daha Uyarlamalı ve deneyimi daha kolay hale getirmek için aşağıdaki özellikler eklendi: 

- Çok faktörlü sınama sonuçları, doğrudan MFA sonuçları program erişimi içeren Azure AD oturum açma rapor bütünleştirilmiştir.
- MFA yapılandırma daha derine Azure AD yapılandırmaya tümleşik deneyimi Azure portalında.

Bu genel önizlemede, MFA yönetim ve raporlama bir tümleşik çekirdek Azure AD yapılandırma deneyimi parçasıdır. Şimdi, Azure AD deneyimi içinde MFA Yönetim Portalı işlevselliğini yönetebilirsiniz.

Daha fazla bilgi için bkz: [MFA Azure portalında raporlama için başvuru](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-sign-ins-mfa). 


---

### <a name="terms-of-use"></a>Kullanım koşulları



**Tür:** yeni özellik  
**Servis kategorisi:** kullanım koşulları  
**Ürün yetenek:** uyumluluk  



Azure AD yasal veya uyumluluk gereksinimlerine yönelik ilgili bildirimler gibi bilgiler sunmak için kullanım koşulları kullanabilirsiniz.

Azure AD kullanım koşullarını aşağıdaki senaryolarda kullanabilirsiniz:

- Kuruluşunuzdaki tüm kullanıcılar için genel koşulları
- Belirli bir kullanıcının öznitelikleri (örneğin, Doktorlar nurses karşılaştırması) veya yurtiçi ve uluslararası çalışanlar, dinamik grupların tarafından yapılan temel kullanım koşulları
- Salesforce gibi yüksek etkili iş uygulamalarına erişmek için belirli kullanım koşulları

Daha fazla bilgi için bkz: [kullanım koşullarını Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-tou).


---

### <a name="enhancements-to-privileged-identity-management"></a>Ayrıcalıklı kimlik yönetimi geliştirmeleri


**Tür:** yeni özellik  
**Servis kategorisi:** Privileged Identity Management  
**Ürün yetenek:** Privileged Identity Management  


Azure AD Privileged Identity Management'ı yönetin, denetleyin ve (Önizleme) Azure kaynaklarına erişimi için kuruluşunuzdaki izleyin:

- Abonelikler
- Kaynak grupları
- Sanal makineler 

Azure portalındaki Azure RBAC işlevselliği kullanan tüm kaynakları tüm sunmak için Azure AD Privileged Identity Management sahip yaşam döngüsü yönetimi özellikleri ve güvenlik yararlanabilir.

Daha fazla bilgi için bkz: [Azure kaynakları için Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac).


---

### <a name="access-reviews"></a>Erişim gözden geçirmeleri


**Tür:** yeni özellik  
**Servis kategorisi:** erişim gözden geçirme  
**Ürün yetenek:** uyumluluk  



Kuruluşlar erişim incelemeler (Önizleme), grup üyelikleri ve kurumsal uygulamalara erişim verimli bir şekilde yönetmek için kullanabilirsiniz: 

- Konuk kullanıcıların uygulamalara ve grup üyeliklerine erişimlerine ait erişim gözden geçirmelerini kullanarak bu kullanıcıların erişimini yeniden onaylayabilirsiniz. Gözden geçirenler verimli bir şekilde erişim incelemeler tarafından sağlanan Öngörüler göre erişim konuklar izin vermek için olup olmadığını devam karar verebilirsiniz.
- Erişim gözden geçirmeleri ile çalışanların uygulamalara erişimini ve grup üyeliklerini yeniden onaylayabilirsiniz.

Erişim gözden geçirmesi denetimlerini kuruluşunuza uygun programlarda toplayarak, uyumluluk veya riske duyarlı uygulamalar için gözden geçirmeleri takip edebilirsiniz.

Daha fazla bilgi için bkz: [Azure AD erişim incelemeleri](https://docs.microsoft.com/azure/active-directory/active-directory-azure-ad-controls-access-reviews-overview).


---

### <a name="hide-third-party-applications-from-my-apps-and-the-office-365-app-launcher"></a>Üçüncü taraf uygulamalardan My uygulamaları ve Office 365 uygulama Başlatıcı Gizle



**Tür:** yeni özellik  
**Servis kategorisi:** My uygulamalar  
**Ürün yetenek:** çoklu oturum açma  



Şimdi, kullanıcılarınızın portallar aracılığıyla yeni bir görünmesini uygulamaları daha iyi yönetebilirsiniz **uygulama Gizle** özelliği. Arka uç hizmetlerine veya yinelenen kutucukları ve dağınıklığı kullanıcıların uygulama launchers için uygulama kutucuklarına burada görünür durumda yardımcı olmak için uygulamaların gizleyebilirsiniz. İki durumlu bulunduğu **özellikleri** üçüncü taraf uygulama bölümü ve etiketli **kullanıcıya görünür?** Ayrıca, bir uygulamayı programlı olarak PowerShell aracılığıyla gizleyebilirsiniz. 

Daha fazla bilgi için bkz: [Azure AD'de kullanıcı deneyiminde bir üçüncü taraf uygulama Gizle](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-hide-third-party-app). 


**Kullanılabilir nedir?**

 Yeni Yönetici Konsolu, iki yeni API geçişi bir parçası olarak günlükleri Azure AD etkinlik almak için kullanılabilir. Yeni API kümesi daha zengin filtreleme ve sıralama daha zengin denetim ve oturum açma etkinliklerini sağlama ek işlevsellik sağlar. Şimdi güvenlik raporları önceden mevcut verileri Microsoft Graph kimlik koruması Risk olayları API'si aracılığıyla erişilebilir.


## <a name="september-2017"></a>Eylül 2017

### <a name="hotfix-for-identity-manager"></a>Identity Manager için düzeltme


**Tür:** değiştirilen özelliği  
**Servis kategorisi:** Identity Manager  
**Ürün yetenek:** kimlik yaşam döngüsü yönetimi  



Düzeltme Toplaması Paketi (yapı 4.4.1642.0) 25 Eylül 2017 Identity Manager 2016 Service Pack 1'dan sonra kullanılabilir. Bu döküm paketi:

- Sorunları giderir ve geliştirmeleri içerir.
- Identity Manager 2016 için yapı 4.4.1459.0 kadar tüm Identity Manager 2016 Service Pack 1 güncelleştirmeleri değiştirir birikmeli bir güncelleştirmedir. 
- Identity Manager 2016 4.4.1302.0 yapı olmasını gerektirir. 

Daha fazla bilgi için bkz: [Identity Manager 2016 Service Pack 1 için düzeltme paketi (yapı 4.4.1642.0) kullanılabilir](https://support.microsoft.com/help/4021562). 

---
