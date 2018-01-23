---
title: "Yenilikler Azure Active Directory için sürüm notları | Microsoft Docs"
description: "Bilinen sorunlar, hata düzeltmeleri, kullanım dışı bırakılan işlevsellik ve yaklaşan değişiklikleri en son sürüm notlarını gibi Azure Active Directory (Azure AD) ile yenilikleri öğrenin."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
featureFlags: clicktale
ms.assetid: 06a149f7-4aa1-4fb9-a8ec-ac2633b031fb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/19/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: c7aab313e6c848c97447cde22752cfed945442df
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2018
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


## <a name="december-2017"></a>Aralık 2017
 

### <a name="terms-of-use-in-the-access-panel"></a>Erişim paneli kullanım koşulları

**Tür:** yeni özellik  
**Servis kategorisi:** kullanım koşulları  
**Ürün yetenek:** idare/uyumluluk
 
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
**Ürün yetenek:** idare
 
Yöneticiler için bir seçenek koşulları kabul etmeden önce Kullanım Koşulları'nı genişletin, kullanıcılar gerektirir.

Şunlardan birini seçin **üzerinde** veya **kapalı** kullanıcıların Kullanım Koşulları'nı genişletin. **Üzerinde** ayarı kabul etmeden önce kullanım koşullarını görüntülemek kullanıcıların gerektirir.

Daha fazla bilgi için bkz: [kullanım özelliği (Önizleme) Azure AD koşullarını](active-directory-tou.md).
 
---
 

### <a name="scoped-activation-for-eligible-role-assignments"></a>Uygun rol atamaları için kapsamlı etkinleştirme

**Tür:** yeni özellik  
**Servis kategorisi:** Privileged Identity Management  
**Ürün yetenek:** Privileged Identity Management
 
Uygun Azure kaynak rol atamalarını özgün atama varsayılan değerinden daha az otonomisi ile etkinleştirmek için kapsamlı etkinleştirmesini kullanabilirsiniz. Kiracınızda atadığınız bir aboneliğin sahibi bir örnektir. Kapsamlı etkinleştirme ile (örneğin, kaynak gruplarını ve sanal makineler) aboneliği kapsamında yer alan en fazla beş kaynaklar için sahip rolünü de etkinleştirebilirsiniz. Etkinleştirme kapsamı kritik Azure kaynaklarına istenmeyen değişiklikler Yürütülüyor olasılığını azaltabilir.

Daha fazla bilgi için bkz: [Azure AD Privileged Identity Management nedir?](active-directory-privileged-identity-management-configure.md).
 
---
 

### <a name="new-federated-apps-in-the-azure-ad-app-gallery"></a>Azure AD uygulama galerisinde yeni Federal apps

**Tür:** yeni özellik  
**Servis kategorisi:** Kurumsal uygulamaları  
**Ürün yetenek:** üçüncü taraf tümleştirme
 
Aralık 2017 ' aşağıdaki yeni uygulamalar Federasyonu Desteği ile uygulama galerisinde eklendi.

|Ad|Tümleştirme türü|Açıklama|
|:-- |----------------|:----------|
|EFI dijital mağaza|SAML 2.0|[Web 2 yazdırma](https://go.microsoft.com/fwlink/?linkid=861685) uygulama.|
|Vodeclic|SAML 2.0|Kullanıcı erişimini yönetmek ve çoklu oturum açma ile etkinleştirmek için Azure AD kullanmak [Vodeclic](https://go.microsoft.com/fwlink/?linkid=863522). Varolan Vodeclic hesabı gerektirir.|
|Accredible|SAML 2.0|Kullanım [Accredible](https://go.microsoft.com/fwlink/?linkid=863523) oluşturmak için yönetmek ve sertifikalar, Göstergeler ve blockchain kimlik bilgilerini sunar.|
|FactSet|SAML 2.0|Çoklu oturum açma için [FactSet FDSWeb uygulama](https://go.microsoft.com/fwlink/?linkid=863525).|
|MobileIron Azure AD tümleştirme|SAML 2.0|Modern kuruluşların kullanabileceğiniz [MobileIron](https://go.microsoft.com/fwlink/?linkid=858027) güvenli hale getiren ve gizlilik ve güven korumak ederken mobil ve bulut hareket ederken bilgilerini yönetin.|
|GÖRÜNTÜ ÇALIŞIR|SAML 2.0|Kullanıcı erişimini yönetmek, kullanıcı hesapları sağlama ve çoklu oturum açma ile etkinleştirmek için Azure AD kullanmak [görüntü WORKS](https://go.microsoft.com/fwlink/?linkid=863517). Varolan bir görüntü WORKS aboneliği gerektirir.|
|SAML SSO GmbH çözünürlüğün Bitbucket için|SAML 2.0|[SSO Bitbucket](https://go.microsoft.com/fwlink/?linkid=863519) temsilciler kimlik doğrulaması Azure ad. Azure AD ile oturum açmış kullanıcıların Bitbucket doğrudan erişebilirsiniz. Kullanıcıların oluşturulur ve kolay bir şekilde verilerle SAML öznitelik güncelleştirildi.|
|SAML SSO GmbH çözünürlüğün Bambu için|SAML 2.0|[SSO Bambu](https://go.microsoft.com/fwlink/?linkid=863520) temsilciler kimlik doğrulaması Azure ad. Azure AD ile oturum açmış kullanıcıların Bambu doğrudan erişebilirsiniz.|
|Communifire|SAML 2.0|[Communifire](https://go.microsoft.com/fwlink/?linkid=861676) çalışanlar ve iş destekleyen modern, tam özellikli sosyal intranet yazılımdır.|
|MOBI|SAML 2.0|Kullanım [MOBI](https://go.microsoft.com/fwlink/?linkid=863521) merkezileştirmek için kavrama ve tüm aygıt ekosistemi denetim.|
|Reflektive|SAML 2.0|[Reflektive](https://go.microsoft.com/fwlink/?linkid=863518) performans yönetimi, gerçek zamanlı geri bildirim ve hedef belirleme için modern bir platformdur. |
|CybSafe|OpenID Connect & OAuth|Bu GCHQ onaylı siber tanıma platform Gelişmiş teknoloji ve veri analizi siber güvenlik ve veri koruma risk İnsan yönünü azaltmak için kullanır.|
|WebHR|OpenID Connect & OAuth|Bu sosyal İnsan Kaynakları yazılım 197 ülkede birden fazla 20.000 şirketler tarafından güvenilir.|
 |Zenegy Azure AD tümleştirme|OpenID Connect & OAuth|Bu uygulama ile Zenegy için oturum açmak için şirketinizin Azure AD kimlik bilgilerini kullanabilirsiniz.|
|Adobe Experience Manager|SAML 2.0|Web siteleri, mobil uygulamaları ve pazarlama içeriği ve varlıkları yönetmek için formlar oluşturmak için bu kapsamlı bir içerik yönetim platformu çözüm kullanabilirsiniz.|

 
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

Daha fazla bilgi için bkz: [koşullu erişim Azure AD'de](active-directory-conditional-access-azure-portal.md).

 
---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>Azure AD uygulama tabanlı koşullu erişim için yeni onaylanmış istemci uygulamaları

 
**Tür:** değişiklik planı  
**Servis kategorisi:** koşullu erişim  
**Ürün yetenek:** kimlik güvenlik ve koruma




Aşağıdaki uygulamalar listesine eklemek için planlanan [istemci uygulamaları onaylanan](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement):

- [Microsoft Kaizala](https://www.microsoft.com/garage/profiles/kaizala/)
- [Microsoft StaffHub](https://staffhub.office.com/what-it-is)


Daha fazla bilgi için bkz.

- [Onaylanmış istemci uygulama gereksinimi](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement)
- [Azure AD uygulama temelli koşullu erişim](active-directory-conditional-access-mam.md)


---

### <a name="terms-of-use-support-for-multiple-languages"></a>Kullanım Koşulları'nı birden çok dil desteği



**Tür:** yeni özellik    
**Servis kategorisi:** kullanım koşulları  
**Ürün yetenek:** idare/uyumluluk





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


Daha fazla bilgi için bkz: [şirket içi tümleştirme](active-directory-passwords-how-it-works.md#on-premises-integration).

 
---


### <a name="azure-ad-app-based-conditional-access"></a>Azure AD uygulama temelli koşullu erişim 



 
**Tür:** yeni özellik  
**Servis kategorisi:** Azure AD  
**Ürün yetenek:** kimlik güvenlik ve koruma





Şimdi Office 365 ve diğer Azure AD bağlı bulut uygulamalarına erişimi kısıtlayabilirsiniz [istemci uygulamaları onaylanan](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement) destekleyen Intune uygulama koruma ilkeleri kullanarak [Azure AD uygulama temelli koşullu erişim](active-directory-conditional-access-mam.md). Intune uygulama koruma ilkeleri yapılandırmak ve bu istemci uygulamaları üzerindeki şirket verilerini korumak için kullanılır.

Birleştirme tarafından [uygulama tabanlı](active-directory-conditional-access-mam.md) ile [aygıt tabanlı](active-directory-conditional-access-policy-connected-applications.md) koşullu erişim ilkeleri, kişisel verileri ve şirket cihazları korumak için esnekliğe sahip.

Aşağıdaki koşullar ve denetimleri artık uygulama bağlı olarak koşullu erişim ile kullanılmak üzere kullanılabilir:

**Desteklenen bir platform koşulu**

- iOS
- Android

**İstemci uygulamaları koşulu**

- Mobil uygulamalar ve masaüstü istemcileri

**Erişim denetimi**

- Onaylı istemci uygulaması gerektir


Daha fazla bilgi için bkz: [Azure AD uygulama temelli koşullu erişim](active-directory-conditional-access-mam.md).

 
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

Daha fazla bilgi için bkz: [Azure portalını kullanarak cihazları yönetme](device-management-azure-portal.md).



 
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

- [Intune ile macOS cihazlar için cihaz uyumluluk ilkesi oluşturma](https://aka.ms/macoscompliancepolicy)
- [Azure AD'de koşullu erişim](active-directory-conditional-access-azure-portal.md)


 
---

### <a name="network-policy-server-extension-for-azure-multi-factor-authentication"></a>Azure çok faktörlü kimlik doğrulaması için ağ ilkesi sunucusu uzantısı 


**Tür:** yeni özellik    
**Servis kategorisi:** çok faktörlü kimlik doğrulaması  
**Ürün yetenek:** kullanıcı kimlik doğrulaması




Azure çok faktörlü kimlik doğrulaması için ağ ilkesi sunucusu uzantısı, var olan sunucularınızı kullanarak, kimlik doğrulaması altyapınız için bulut tabanlı çok faktörlü kimlik doğrulama özellikleri ekler. Ağ İlkesi Sunucusu uzantısı ile var olan kimlik doğrulama akışı telefon araması, SMS mesajı ya da telefon uygulama doğrulama ekleyebilirsiniz. Yükleme, yapılandırma ve yeni sunucuları sağlamak zorunda değilsiniz. 

Bu uzantı, sanal özel ağ bağlantıları Azure çok faktörlü kimlik doğrulama sunucusu dağıtmadan korumak istediğiniz kuruluşlar için oluşturuldu. Ağ ilkesi sunucusu arasındaki RADIUS Azure çok faktörlü kimlik doğrulaması bulut tabanlı bir ikinci faktör kimlik doğrulaması sağlamak için bir bağdaştırıcı uzantısı görür federe veya kullanıcılar eşitlenir.


Daha fazla bilgi için bkz: [varolan ağ ilkesi sunucusu altyapınızı Azure çok faktörlü kimlik doğrulamasıyla tümleştirmek](../multi-factor-authentication/multi-factor-authentication-nps-extension.md).

 
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


Aşağıdaki uygulamalar listesine eklenen [istemci uygulamaları onaylanan](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement):

- Microsoft Planner
- Azure Information Protection 


Daha fazla bilgi için bkz.

- [Onaylanmış istemci uygulama gereksinimi](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement)
- [Azure AD uygulama temelli koşullu erişim](active-directory-conditional-access-mam.md)


---

### <a name="use-or-between-controls-in-a-conditional-access-policy"></a>Kullanım koşullu erişim ilkesi denetimlerinde arasında "veya" 


**Tür:** değiştirilen özelliği    
**Servis kategorisi:** koşullu erişim  
**Ürün yetenek:** kimlik güvenlik ve koruma

 
Şimdi kullanabilir "veya" (Seçili denetimleri birini gerektirir) koşullu erişim denetimleri için. Erişim denetimleri arasındaki ile "veya" ilkeleri oluşturmak için bu özelliği kullanın. Örneğin, uyumlu bir cihaz üzerinde olacak şekilde "veya" çok faktörlü kimlik doğrulaması kullanarak oturum açmak için kullanıcının gerektiren bir ilke oluşturmak için bu özelliği kullanabilirsiniz.

Daha fazla bilgi için bkz: [Azure AD koşullu erişim denetimleri](active-directory-conditional-access-controls.md).

 
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



Azure AD, bir HTML kullanıcı adı ve parola alanı işleme uygulamaları için otomatik oturum açma alan algılama destekler. Bu adımları belgelenmiştir [otomatik olarak oturum açma alanları bir uygulama için yakalama](application-config-sso-problem-configure-password-sso-non-gallery.md#how-to-manually-capture-sign-in-fields-for-an-application). Ekleyerek bu yeteneği bulabilirsiniz bir *olmayan galeri* uygulaması **kurumsal uygulamalar** sayfasındaki [Azure portal](http://aad.portal.azure.com). Ayrıca, yapılandırabileceğiniz **çoklu oturum açma** bu yeni uygulama modu **parola tabanlı çoklu oturum açma**, bir web URL'si girin ve sayfayı kaydedin.
 
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

Daha fazla bilgi için bkz: [MFA Azure portalında raporlama için başvuru](active-directory-reporting-activity-sign-ins-mfa.md). 


---

### <a name="terms-of-use"></a>Kullanım koşulları



**Tür:** yeni özellik  
**Servis kategorisi:** kullanım koşulları  
**Ürün yetenek:** idare  



Azure AD yasal veya uyumluluk gereksinimlerine yönelik ilgili bildirimler gibi bilgiler sunmak için kullanım koşulları kullanabilirsiniz.

Azure AD kullanım koşullarını aşağıdaki senaryolarda kullanabilirsiniz:

- Kuruluşunuzdaki tüm kullanıcılar için genel koşulları
- Belirli bir kullanıcının öznitelikleri (örneğin, Doktorlar nurses karşılaştırması) veya yurtiçi ve uluslararası çalışanlar, dinamik grupların tarafından yapılan temel kullanım koşulları
- Salesforce gibi yüksek etkili iş uygulamalarına erişmek için belirli kullanım koşulları

Daha fazla bilgi için bkz: [kullanım koşullarını Azure AD](active-directory-tou.md).


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

Daha fazla bilgi için bkz: [Azure kaynakları için Privileged Identity Management](privileged-identity-management/azure-pim-resource-rbac.md).


---

### <a name="access-reviews"></a>Erişim gözden geçirmeleri


**Tür:** yeni özellik  
**Servis kategorisi:** erişim gözden geçirme  
**Ürün yetenek:** idare  



Kuruluşlar erişim incelemeler (Önizleme), grup üyelikleri ve kurumsal uygulamalara erişim verimli bir şekilde yönetmek için kullanabilirsiniz: 

- Konuk kullanıcıların uygulamalara ve grup üyeliklerine erişimlerine ait erişim gözden geçirmelerini kullanarak bu kullanıcıların erişimini yeniden onaylayabilirsiniz. Gözden geçirenler verimli bir şekilde erişim incelemeler tarafından sağlanan Öngörüler göre erişim konuklar izin vermek için olup olmadığını devam karar verebilirsiniz.
- Erişim gözden geçirmeleri ile çalışanların uygulamalara erişimini ve grup üyeliklerini yeniden onaylayabilirsiniz.

Erişim gözden geçirmesi denetimlerini kuruluşunuza uygun programlarda toplayarak, uyumluluk veya riske duyarlı uygulamalar için gözden geçirmeleri takip edebilirsiniz.

Daha fazla bilgi için bkz: [Azure AD erişim incelemeleri](active-directory-azure-ad-controls-access-reviews-overview.md).


---

### <a name="hide-third-party-applications-from-my-apps-and-the-office-365-app-launcher"></a>Üçüncü taraf uygulamalardan My uygulamaları ve Office 365 uygulama Başlatıcı Gizle



**Tür:** yeni özellik  
**Servis kategorisi:** My uygulamalar  
**Ürün yetenek:** çoklu oturum açma  



Şimdi, kullanıcılarınızın portallar aracılığıyla yeni bir görünmesini uygulamaları daha iyi yönetebilirsiniz **uygulama Gizle** özelliği. Arka uç hizmetlerine veya yinelenen kutucukları ve dağınıklığı kullanıcıların uygulama launchers için uygulama kutucuklarına burada görünür durumda yardımcı olmak için uygulamaların gizleyebilirsiniz. İki durumlu bulunduğu **özellikleri** üçüncü taraf uygulama bölümü ve etiketli **kullanıcıya görünür?** Ayrıca, bir uygulamayı programlı olarak PowerShell aracılığıyla gizleyebilirsiniz. 

Daha fazla bilgi için bkz: [Azure AD'de kullanıcı deneyiminde bir üçüncü taraf uygulama Gizle](active-directory-coreapps-hide-third-party-app.md). 


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
