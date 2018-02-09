---
title: "AD FS şirket içi uygulamalarını Azure'a geçirin. | Microsoft Docs"
description: "Bu belge federasyon SaaS uygulamalarına odaklanarak, kuruluşların şirket içi uygulamaların Azure AD'ye nasıl geçirildiğini anlamalarına yardımcı olmayı hedefler."
services: active-directory
author: billmath
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/29/2018
ms.author: billmath
ms.openlocfilehash: ec0731534da2543d48bedc575bf882b790fa136b
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="migrate-ad-fs-on-premises-apps-to-azure"></a>AD FS şirket içi uygulamalarını Azure'a geçirme 

Bu belge, kuruluşların şirket içi uygulamaların Azure AD'ye nasıl geçirildiğini anlamalarına yardımcı olmayı hedefler.  Özellikle federasyon SaaS uygulamalarına odaklanır.  Bu belgede, adım adım yönergeler sağlanmaz.  Şirket içi yapılandırmaların Azure AD'ye nasıl dönüştürüldüğünü anlayarak geçişi başarmanıza yardımcı olmak için kavramsal rehberlik sağlanır. Ayrıca, en yaygın senaryoların gereksinimleri de bu belgenin kapsamındadır.

## <a name="introduction"></a>Giriş

Kullanıcı hesaplarının bulunduğu şirket içi bir dizininiz varsa, büyük olasılıkla en az bir veya iki uygulamanız vardır.  Bu uygulamalar da, kullanıcıların söz konusu kimliklerle oturum açarak erişmeleri için yapılandırılmıştır.

Ayrıca siz de çoğu kuruluş gibiyseniz, büyük olasılıkla bulut uygulamaları ve kimliklerini benimseme yolunda ilerliyorsunuz demektir.  Belki de Office 365 ve Azure AD Connect ile hızla yol alıyorsunuzdur.  Tüm iş yükleri için değil önemli bazı iş yükleri için bulut tabanlı SaaS uygulamalarını kurmuş da olabilirsiniz.  

Birçok kuruluşun doğrudan Active Directory Federation Service (AD FS) gibi bir şirket içi oturum açma hizmetine federasyon oluşturan SaaS veya özel iş kolu (LoB) uygulamaları, ayrıca Office 365 ve Azure AD tabanlı uygulamaları vardır.  Bu geçiş kılavuzunda şirket içi uygulamaların Azure AD'ye neden ve nasıl geçirildiği açıklanır.

>[!NOTE]
>Bu kılavuzda SaaS uygulaması yapılandırma ve geçişi hakkında ayrıntılı bilgi ve özel LoB uygulamaları hakkında üst düzey bilgi sağlanır.  Gelecekte özel LoB uygulamalarına yönelik daha ayrıntılı bir kılavuz sağlanması planlanmaktadır.

Şekil 1: Doğrudan şirket içine bağlı uygulamalar ![şirket içi](media/migrate-adfs-apps-to-azure/migrate1.png)

Figure 2: Azure AD yoluyla federasyon oluşturan uygulamalar ![Azure](media/migrate-adfs-apps-to-azure/migrate2.png)

## <a name="why-migrate-apps-to-azure-ad"></a>Uygulamalar neden Azure AD’ye geçirilir?

Zaten AD FS, Ping veya başka bir şirket içi kimlik doğrulama sağlayıcısı kullanan bir kuruluşa, uygulamaları Azure AD'ye geçirmek şu avantajları sağlar:

**Daha güvenli erişim**
- [Azure AD Koşullu Erişim](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal) kullanarak Multi-Factor Authentication (MFA) dahil ayrıntılı uygulama başına erişim denetimlerini yapılandırın.  İlkeler, bugün Office 365'le uyguluyor olabileceğiniz yolla SaaS uygulamalarına ve özel uygulamalara uygulanabilir
- Tehditleri algılamak ve riskli trafiği tanımlayan makine öğrenme ve buluşsal yöntemler temelinde oturum açma bilgilerini korumak için, [Azure AD Kimlik Koruması](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) ile Azure AD'nin yerleşik ve sürekli gelişen özelliklerinden yararlanın

**Azure AD B2B işbirliği**
- SaaS uygulamalarında oturum açma işlemi Azure AD temelinde yapıldığında, [Azure AD B2B işbirliğiyle](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b) iş ortaklarına bulut kaynakları için erişim verebilirsiniz.

**Azure AD'nin daha kolay yönetici deneyimi ve ek özellikleri**
- SaaS uygulamaları için bir kimlik sağlayıcısı olan Azure AD, uygulama başına belirteç imzalama sertifikaları, [yapılandırılabilir sertifika sona erme tarihleri](https://docs.microsoft.com/azure/active-directory/active-directory-sso-certs) ve Azure AD kimlikleri temelinde kullanıcı hesaplarının [otomatik sağlanması](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning) (önemli Galeri uygulamalarında) gibi ek özellikleri de destekler

**Şirket içi bir kimlik sağlayıcının avantajlarını koruma**
- Azure AD'nin avantajlarını elde ederken kimlik doğrulama için şirket içi çözümü kullanmaya devam edebilirsiniz, çünkü şirket içi Multi-Factor Authentication (MFA) çözümleri, günlük ve denetim gibi avantajlar korunur 

**Şirket içi kimlik doğrulayıcıyı devre dışı bırakmaya hemen başlama**
- Şirket içi kimlik doğrulama ürününü devre dışı bırakmak isteyen kuruluşlarda, uygulamaları Azure AD'ye geçirmek işleri biraz azalttığından daha kolay geçiş yapılmasını sağlar 

## <a name="mapping-types-of-apps-on-premises-to-types-of-apps-in-azure-ad"></a>Şirket içi uygulama türlerini Azure AD'deki uygulama türleriyle eşleme
Uygulamaların çoğu, kullandıkları oturum açma türüne göre birkaç kategoriden birine girer.  Bu kategoriler, uygulamanın Azure AD'de nasıl temsil edildiğini belirler.

Kısacası, SAML 2.0 uygulamaları Azure AD uygulama galerisi yoluyla veya galeri dışı uygulamalar olarak Azure AD ile tümleştirilebilir.  OAuth 2.0 veya OpenID Connect kullanan uygulamalar Azure AD ile benzer biçimde, “uygulama kayıtları" olarak tümleştirilebilir.  Daha ayrıntılı bilgi için okumaya devam edin.

### <a name="federated-saas-apps-vs-custom-lob-apps"></a>Federasyon SaaS uygulamaları - özel LoB uygulamaları
Federasyon uygulamaları, listelenen kategorilere giren uygulamaları içerir.

- SaaS uygulamaları 
    - Kullanıcılarınız Salesforce, ServiceNow veya Workday gibi SaaS uygulamalarında oturum açıyorsa ve AD FS veya Ping gibi bir şirket içi kimlik sağlayıcısıyla tümleştiriyorsanız, SAS uygulamaları için federasyon oturum açma işlemini kullanıyorsunuz demektir.
    - Uygulamalar federasyon oturum açma işlemi için genellikle SAML 2.0 protokolünü kullanır.
    - Bu kategoriye giren uygulamalar Azure AD ile galeriden veya galeri dışı uygulamalar şeklinde Kurumsal Uygulamalar olarak tümleştirilebilir.
- Özel LoB uygulamaları
    - Bu terim kuruluşunuzun içinde geliştirilen veya veri merkezinize yüklenmiş standart paketli ürün olarak sağlanan SaaS dışı uygulamaları belirtir.  Bu kategori, SharePoint uygulamalarını ve Windows Identity Foundation (WIF) üzerinde oluşturulan uygulamaları içerir.
    - Uygulamalar federasyon oturum açma işlemi için SAML 2.0, WS-Federasyon, OAuth veya OpenID Connect kullanabilir
    - Oauth 2.0, OpenID Connect veya WS-Federasyon kullanan özel uygulamalar Azure AD ile Uygulama kayıtları olarak ve SAML 2.0 veya WS-Federasyon kullanan özel uygulamalar da Kurumsal uygulamaların içinde galeri dışı uygulamalar olarak tümleştirilebilir

### <a name="non-federated-apps"></a>Federasyon olmayan uygulamalar
Bunlara ek olarak, federasyon olmayan uygulamalar Azure AD Uygulama Ara Sunucusu ve bununla ilgili özellikler kullanılarak Azure AD ile tümleştirilebilir.  Özellikler hakkında daha fazla bilgi için aşağıdaki bağlantıları izleyin:
- Doğrudan Active Directory'ye Windows Integrated Auth (WIA) kullanan uygulamalar
    - Bu uygulamalar, [Azure AD Uygulama Ara Sunucusu](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal) yoluyla Azure AD'yle tümleştirilebilir
- Bir aracı yoluyla çoklu oturum açma sağlayıcınızla tümleştirilen ve yetkilendirme için üst bilgi kullanan uygulamalar
    - Oturum açma işlemi için yüklü bir aracıyı kullanan ve üst bilgiye dayalı yetkilendirme yapan şirket içi uygulamaları, [Ping Access for AzureAD](https://blogs.technet.microsoft.com/enterprisemobility/2017/06/15/ping-access-for-azure-ad-is-now-generally-available-ga/) ile Azure AD Uygulama Ara Sunucusu kullanılarak Azure AD tabanlı oturum açma için yapılandırılabilir

## <a name="translating-on-premises-federated-apps-to-azure-ad"></a>Şirket içi federasyon uygulamalarını Azure AD'ye çevirme 
Neyse ki AD FS ile Azure AD'nin çalışmaları birbirine benzer; dolayısıyla güven yapılandırma, oturum açma ve oturum kapatma URL'leri ve tanımlayıcı gibi kavramlar her ikisi için de geçerlidir.  Öte yandan, geçişi yapmadan önce anlamanız gereken bazı küçük farklılıklar da vardır.

Tabloda, çevirinize yardımcı olmak için AD FS, Azure AD ve SaaS uygulamalarında paylaşılan bazı önemli fikirleri eşleştirdik. 

### <a name="representing-the-app-in-azure-ad-or-ad-fs"></a>Azure AD veya AD FS'de uygulamaları gösterme
Geçiş işlemi, uygulamanın şirket içinde nasıl yapılandırıldığını değerlendirerek ve bu yapılandırmayı Azure AD'ye eşleyerek başlar.  Aşağıda, AD FS bağlı olan taraf yapılandırma öğelerinin Azure AD'de bunlara karşılık gelen öğelerle eşlemesini bulabilirsiniz.  
- AD FS terimi: Bağlı Olan Taraf veya Bağlı Olan Taraf Güveni
- Azure AD terimi: Kurumsal uygulama veya Uygulama kaydı (uygulamanın türüne bağlı olarak)

|Uygulama Yapılandırma Öğesi|Açıklama|AD FS yapılandırması içinde|Azure AD yapılandırmasında buna karşılık gelen konum|SAML Belirteç öğesi|
|-----|-----|-----|-----|-----|
|Uygulama oturum açma URL'si|Bu uygulamanın oturum açma sayfasını URL'si. Burası, kullanıcının SP tarafından başlatılmış bir SAML akışında uygulamada oturum açma başlatmak için gideceği yerdir.|Yok|Azure AD'de oturum açma url'si Azure Portal'ın içinde, uygulamanın Çoklu Oturum Açma Özelliklerinde oturum açma URL'si olarak yapılandırılır.</br></br>(Oturum açma URL'sini görmek için Gelişmiş URL ayarlarını göster'e tıklamanız gerekebilir)||
|Uygulama Yanıt URL'si|IdP’nin pespektifinden uygulamanın URL'si.  Burası, kullanıcı IDP'de oturum açtıktan sonra kullanıcının ve belirtecin gönderildiği yerdir.</br></br>  Bazen SAML onay belgesi tüketici uç noktası olarak da adlandırılır.|Uygulamanın AD FS Bağlı Olan Taraf Güveni'nde bulunur.  Bağlı olan tarafa sağ tıklayın ve “Özellikler” -> “Uç Noktalar” sekmesini seçin.|Azure AD'de yanıt url'si Azure Portal'ın içinde, uygulamanın Çoklu Oturum Açma Özelliklerinde Yanıt URL'si olarak yapılandırılır.</br></br>(Yanıt URL'sini görmek için Gelişmiş URL ayarlarını göster'e tıklamanız gerekebilir)|SAML belirtecindeki Destination öğesiyle eşlenir.</br></br>  Örnek değer:  https://contoso.my.salesforce.com|
|Uygulama Oturum Kapatma URL'si|Kullanıcı uygulamada oturumunu kapattığında, IDP'nin kullanıcı oturumu açtığı diğer tüm uygulamalarda da oturumu kapatmak için “oturum kapatma temizleme” isteklerinin gönderildiği URL.|AD FS Yönetimi'nde, Bağlı Olan Taraf Güvenleri'nin altında bulunur.  RP'ye sağ tıklayın ve “Özellikler” öğesini seçin -> “Uç Noktalar” sekmesine tıklayın|Yok – Azure AD, tüm uygulamalarda oturumun kapatılması anlamına gelen “çoklu oturum kapatma” işlemini desteklemez.  Kullanıcının yalnızca Azure AD oturumunu kapatır.|NA|
|Uygulama Tanımlayıcısı|IdP’nin perspektifinden uygulamanın tanımlayıcısı Tanımlayıcı için çoğunlukla oturum açma URL değeri kullanılır (ama her zaman kullanılmaz)</br></br>  Bazen uygulama bunu “Varlık Kimliği" olarak adlandırır.|AD FS'de bu Bağlı Olan Taraf Kimliğidir: Bağlı olan taraf güvenine sağ tıklayın ve “Özellikler” öğesini seçin -> “Tanımlayıcılar” sekmesine tıklayın|Azure AD'de, tanımlayıcı Azure Portal'ın içinde uygulamanın Çoklu Oturum Açma Özellikleri'nde, Etki Alanı ve URL'ler altında Tanımlayıcı olarak yapılandırılır (“Gelişmiş URL ayarlarını göster” onay kutusuna tıklamanız gerekebilir)|SAML belirtecindeki Audience öğesine karşılık gelir|
|Uygulama Federasyon Meta Verileri|Uygulamanın federasyon meta verilerinin konumu.  IdP tarafından, Uç Noktalar veya şifreleme sertifikaları gibi belirli yapılandırma ayarlarını otomatik olarak güncelleştirmek için kullanılır.|Uygulamanın Federasyon Meta Veri URL'si, uygulamaya ilişkin AD FS Bağlı Olan Taraf Güveni'nde bulunur.  Güvene sağ tıklayın, Özellikler'i seçin ve ardından İzleme sekmesine tıklayın.|Yok - Azure AD uygulama federasyon meta verilerini doğrudan kullanmayı desteklemez|NA|
|Kullanıcı tanımlayıcısı / NameID|Azure AD'den veya AD FS'den kullanıcı tanımlayıcınızı uygulamanıza benzersiz olarak göstermek için kullanılan öznitelik.</br></br>  Normalde kullanıcının UPN'si veya e-posta adresi.|Bu AD FS'de, bağlı olan tarafta bir talep kuralı olarak bulunur.  Çoğu durumda, "nameidentifier" ile biten bir türdeki talebi gönderen talep kuralıdır|Azure AD'de, Kullanıcı Tanımlayıcısı Azure Portal'ın içinde uygulamanın Çoklu Oturum Açma Özellikleri'ndeki Kullanıcı Öznitelikleri başlığının altında bulunur.</br></br>Varsayılan olarak UPN kullanılır.|IDP'den uygulamaya SAML belirtecindeki “NameID” öğesi olarak iletilir.|
|Uygulamaya gönderilecek diğer Talepler|Kullanıcı Tanımlayıcısı / NameID öğesine ek olarak, IDP'den uygulamaya başka talep bilgileri de (örneğin ad, soyadı, e-posta adresi ve kullanıcının üye olduğu gruplar) yaygın olarak gönderilir|Bu AD FS'de, bağlı olan tarafta diğer talep kuralları olarak bulunur.|Azure AD'de, Azure Portal'ın içinde uygulamanın Çoklu Oturum Açma Özellikleri'ndeki Kullanıcı öznitelikleri başlığının altında bulunur; Görünüm'e tıklayın ve diğer tüm kullanıcı özniteliklerini düzenleyin.|| 

### <a name="representing-azure-ad-as-an-identity-provider-idp-in-a-saas-app"></a>SaaS uygulamasında Azure AD'yi Kimlik Sağlayıcısı (IdP) olarak gösterme
Geçiş kapsamında, uygulamanın Azure AD'ye (şirket içi kimlik sağlayıcısı yerine) işaret edecek şekilde yapılandırılması gerekir.  Bu bölüm, özel/LOB uygulamalarına değil öncelikli olarak SAML protokolü kullanan SaaS uygulamalarına odaklanır. Bununla birlikte, açıklanan kavramlar özel/LOB uygulamalarını da kapsayacak şekilde genişletilebilir. 

Genel hatlarıyla, SaaS uygulamasını Azure AD'ye gösterecek birkaç önemli nokta vardır
- Kimlik Sağlayıcısını Veren:  https&#58;//sts.windows.net/{kiracı-kimliği}/
- Kimlik Sağlayıcısı Oturum Açma URL'si: https&#58;//login.microsoftonline.com/{kiracı-kimliği}/saml2
- Kimlik Sağlayıcısı Oturum Kapatma URL'si: https&#58;//login.microsoftonline.com/{kiracı-kimliği}/saml2 
- Federasyon meta verilerinin konumu:  https&#58;//login.windows.net/{kiracı-kimliği} <kiracı-kimliği>/federationmetadata/2007-06/federationmetadata.xml?appid={<uygulama-kimliği} 

burada {kiracı-kimliği} yerine, Azure Portal'da Azure Active Directory -> Özellikler altında “Dizin Kimliği” olarak bulunan sizin kiracı kimliğiniz kullanılır ve {uygulama-kimliği} yerine de uygulamanın Özellikleri altında “Uygulama Kimliği” olarak bulunan uygulamanızın kimliği kullanılır

Tabloda, uygulamada SSO ayarlarını yapılandırmaya yönelik önemli IdP yapılandırma öğeleri ve bunların AD FS ve Azure AD'deki değerleri veya konumları daha ayrıntılı açıklanır.  Tablonun referans çerçevesi, kimlik doğrulama isteklerini nereye göndereceğini ve alınan belirteçleri nasıl doğrulayacağını bilmesi gereken SaaS uygulamasıdır.

|Yapılandırma öğesi|Açıklama|AD FS|Azure AD|
|---|---|---|---|
|IdP </br>oturum açma </br>URL'si|uygulamanın perspektifinden IdP'nin oturum açma URL'si (kullanıcının oturum açmak için yeniden yönlendirildiği konum).|AD FS oturum açma URL'si AD FS federasyon hizmeti adının arkasına “/adfs/ls/” eklenerek oluşturulur; örneğin: https&#58;//fs.contoso.com/adfs/ls/|Azure AD için buna karşılık gelen değer de aynı modele uyar; burada {kiracı-kimliği} yerine Azure Portal'da Azure Active Directory -> Özellikler'in altında “Dizin Kimliği" olarak bulunan kendi kiracı kimliğiniz kullanılır.</br></br>SAML-P protokolünü kullanan uygulamalar için: https&#58;//login.microsoftonline.com</br>/{kiracı-kimliği}/saml2 </br></br>WS-Federasyon protokolünü kullanan uygulamalar için: https&#58;//login.microsoftonline.com</br>/{kiracı-kimliği}/wsfed|
|IdP </br>Oturumu Kapatma </br>URL'si|Uygulamanın perspektifinden IdP'nin oturumu kapatma URL'si (uygulamada "oturumu kapatmayı" seçen kullanıcının yeniden yönlendirildiği konum).|AD FS için, oturumu kapatma URL'si oturum açma URL'siyle aynı olabileceği gibi, aynı URL'nin sonuna “wa=wsignout1.0” eklenmiş hali de olabilir: örneğin, https&#58;//fs.contoso.com/adfs/ls /?wa=wsignout1.0|Azure AD'de buna karşılık gelen değer, uygulamanın SAML 2.0 oturumu kapatma işlemini destekleyip desteklemediğine bağlıdır.</br></br>Uygulama SAML oturumu kapatma işlemini destekliyorsa, değer aynı modele uyar; burada {kiracı-kimliği} yerine Azure Portal'da Azure Active Directory -> Özellikler'in altında “Dizin Kimliği" olarak bulunan kiracı kimliği kullanılır. https&#58;//login.microsoftonline.com</br>/{kiracı-kimliği}/saml2</br></br>Uygulama SAML oturumu kapatma işlemini desteklemiyorsa: https&#58;//login.microsoftonline.com</br>/common /wsfederation?wa=wsignout1.0|
|Belirteç </br>İmzalama </br>Sertifika|IDP'nin verilen belirteçleri imzalamak için özel anahtarını kullandığı sertifika.  Belirtecin, uygulamanın güvenmek üzere yapılandırıldığı IDP'den geldiğini doğrular.|AD FS belirteç imzalama sertifikası AD FS Management'ta Sertifikalar altında bulunur.|Azure AD'de, belirteç imzalama sertifikası Azure Portal'ın içinde uygulamanın Çoklu Oturum Açma Özelliklerindeki SAML İmzalama Sertifikası başlığı altında bulunur. Sertifikayı buradan indirip uygulamaya yükleyebilirsiniz.</br></br>  Uygulamanın birden çok sertifikası varsa, tüm sertifikalar federasyon meta veri xml dosyasında bulunur.|
|Tanımlayıcı / </br>“Veren”|Uygulamanın perspektifinden IdP'nin tanımlayıcısı (bazen “Veren” veya “Veren Kimliği” olarak da adlandırılır)</br></br>SAML belirtecinde, değer “Issuer” öğesi olarak gösterilir|AD FS için tanımlayıcı genellikle AD FS Management'ta Hizmet -> Federasyon Hizmeti Özelliklerini Düzenle'nin altında yer alan federasyon hizmeti tanımlayıcısıdır.  Örneğin: http&#58;//fs.contoso.com/adfs/services/trust|Azure AD için buna karşılık gelen değer de aynı modele uyar; burada {kiracı-kimliği} değeri yerine Azure Portal'da Azure Active Directory -> Özellikler'in altında “Dizin Kimliği" olarak bulunan kiracı kimliği kullanılır.  https&#58;//sts.windows.net/{kiracı-kimliği}/|
|IdP </br>Federasyon </br>Meta Veriler|IDP'nin genel kullanıma açık federasyon meta verilerinin konumu.  (Federasyon meta verileri bazı uygulamalar tarafından yönetici yapılandırma URL'lerine, tanımlayıcıya ve bağımsız olarak belirteç imzalama sertifikasına alternatif olarak kullanılır)|AD FS federasyon meta verileri URL'sini AD FS Management'ta Hizmet -> Uç Noktalar -> Meta Veriler -> Tür: Federasyon Meta Verileri altında bulabilirsiniz; örneğin: https&#58;//fs.contoso.com/ FederationMetadata/2007-06/</br>FederationMetadata.xml|Azure AD'de buna karşılık gelen değer şu modele uyar: https&#58;//login.microsoftonline.com</br>/{KiracıEtkiAlanıAdı}/FederationMetadata/2007-06/</br>FederationMetadata.xml burada {KiracıEtkiAlanıAdı} değerinin yerine kiracınızın “contoso.onmicrosoft.com” biçimindeki adı kullanılır </br></br>Azure AD'de federasyon meta verileri hakkında [daha fazla bilgi](https://docs.microsoft.com/azure/active-directory/develop/active-directory-federation-metadata).

## <a name="migrating-saas-apps"></a>SaaS uygulamalarını geçirme
SaaS uygulamalarını AD FS'den veya başka bir kimlik sağlayıcısından Azure AD'ye geçirme, bugün el ile yapılan bir işlemdir. Uygulamaya özgü yönergeler için [Galeride bulunan SaaS uygulamalarını tümleştirme konusundaki öğreticilerin listesine bakın](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list).

Tümleştirme öğreticilerinde bir yeşil alan tümleştirmesi yaptığınız varsayılır.  Uygulamalarınızı planlar, değerlendirir ve bunların tam geçişini yaparken bilmeniz gereken, geçişe özgü birkaç önemli kavram vardır.  
- Bazı uygulamalar kolayca geçirilebilirken, özel talepler gibi daha karmaşık gereksinimleri olan uygulamalar Azure AD ve/veya Azure AD Connect'te ek yapılandırma gerektirebilir
- En yaygın senaryolarda, bir uygulama için yalnızca NameId talebi ve diğer yaygın kullanıcı tanımlayıcısı talepleri gerekir; ek talep gerekip gerekmediğini saptamak için, AD FS'den veya üçüncü taraf kimlik sağlayıcınızdan hangi talepleri verdiğinizi inceleyin
- Uygulama taleplerinin gerekli olduğunu belirledikten sonra, bunların Azure AD'de kullanılabilir olduğundan emin olmanız gerekir.  Gerekli bir özniteliğin, örneğin samAccountName özniteliğinin Azure AD'ye eşitlendiğinden emin olmak için Azure AD Connect eşitleme yapılandırmasını gözden geçirmelisiniz
- Öznitelikler Azure AD'de kullanılabilir olduğunda, bu öznitelikleri verilen belirteçlere talep olarak dahil etmek için Azure AD'de talep verme kuralları ekleyin.  Azure AD'de Çoklu oturum açma özellikleri içinde yapılır.

### <a name="assessing-what-can-be-migrated"></a>Nelerin geçirilebileceğini değerlendirme
SAML 2.0 uygulamaları Azure AD uygulama galerisi yoluyla veya galeri dışı uygulamalar olarak Azure AD ile tümleştirilebilir.  

Azure AD'de yapılandırmak için ek adımlar gerektiren bazı yapılandırmalar vardır ve bazıları da henüz desteklenmemektedir.  Nelerin taşınabileceğini saptamak için, uygulamalarınızdan her birinin geçerli yapılandırmasına, özellikle de şunlara bakın:
- Yapılandırılmış talep kuralları (verme aktarım kuralları)
- SAML NameID biçimi ve özniteliği
- Verilen SAML belirteci sürümleri
- Verme yetkilendirme kuralları veya erişim denetim ilkeleri veya çok faktörlü kimlik doğrulaması (ek kimlik doğrulaması) kuralları gibi diğer yapılandırmalar

#### <a name="what-can-be-migrated-today"></a>Bugün neler geçirilebilir
Bugün kolayca geçirilebilecek uygulamalar, standart yapılandırma öğelerini ve taleplerini kullanan SAML 2.0 uygulamalarıdır.  Bu uygulamalar şunlardan oluşabilir
- kullanıcı asıl adı
- e-posta adresi
- Verilen Ad
- Soyadı
- Azure AD posta özniteliği, posta öneki, employeeid, 1 – 15 uzantı öznitelikleriveya şirket içi SamAccountName dahil olmak üzere SAML NameID gibi alternatif öznitelik (bkz. [NameIdentifier talebini düzenleme)](./develop/active-directory-saml-claims-customization.md)
- Özel talepler (desteklenen talep eşlemeleri hakkında bilgi için [buradaki](active-directory-claims-mapping.md) ve [buradaki](./develop/active-directory-saml-claims-customization.md) belgeye bakın)

Özel taleplere ve nameID öğelerine ek olarak, geçiş işlemi kapsamında Azure AD'de başka yapılandırma adımları gerektiren yapılandırmalar:
- AD FS'deki özel yetkilendirme veya MFA kuralları ([Azure AD koşullu erişim](active-directory-conditional-access-azure-portal.md) özelliği kullanılarak yapılandırılır)
- Azure AD'de PowerShell kullanılarak uygulamalar birden çok SAML uç noktasıyla yapılandırılabilir (Bu özellik portalda kullanılamaz)
- SAML 1.1 sürümü belirteçlerini gerektiren SharePoint uygulamaları gibi WS-Federasyon uygulamalarının PowerShell kullanılarak el ile yapılandırılması gerekir

#### <a name="appsconfigurations-not-supported-in-azure-ad-today"></a>Bugün Azure AD'de desteklenmeyen uygulamalar/yapılandırmalar
Aşağıdaki özellikleri gerektiren uygulamalar bugün geçirilemez.  Bu özellikleri gerektiren uygulamalarınız varsa, aşağıdaki yorum bölümünde geri bildirim sağlayın.
- Protokol Özellikleri
    - Tüm oturum açılan uygulamalarda SAML Çoklu Oturum Kapatma (SLO) desteği
    - WS-Trust ActAs deseni desteği
    - SAML yapı çözümlemesi 
    - İmzalı SAML isteklerinde imza doğrulaması (imzalı isteklerin kabul edildiğine ama imzanın doğrulanmadığına dikkat edin)
 - Belirteç Özellikleri 
     - SAML Belirteci Şifreleme 
     - SAML protokol yanıtları için SAML sürüm 1.1 belirteçler 
- Belirteç içindeki Talep Özellikleri
    - Şirket içi grup adlarını talep olarak verme
    - Azure AD dışındaki depolardan gelen talepler
    - Karmaşık talep verme dönüştürme kuralları (desteklenen talep eşlemeleri için bu belgeye ve bu belgeye bakın)
    - Dizin uzantılarını talep olarak verme
    - NameID biçiminin özel belirtimi
    - Çok değerli öznitelikler verme

>[!NOTE]
>Azure AD, bu alanda başka özellikler eklenerek sürekli geliştirilmektedir. Bu belgeyi düzenli aralıklarla güncelleştiriyoruz. 

### <a name="configuring-azure-ad"></a>Azure AD'yi yapılandırma    
#### <a name="configure-single-sign-on-sso-settings-for-the-saas-app"></a>SaaS uygulaması için çoklu oturum açma (SSO) ayarlarını yapılandırma

Azure AD'de SAML oturum açma ayarlarını uygulamanızın gerektirdiği gibi yapılandırma işlemi, aşağıda gösterildiği gibi uygulamanın Çoklu oturum açma özellikleri içinde, Kullanıcı özniteliklerinin altında gerçekleştirilir:

![](media/migrate-adfs-apps-to-azure/migrate3.png)
- Güvenlik belirtecinde talep olarak gönderilecek öznitelikleri görmek için ‘Diğer tüm kullanıcı özniteliklerini görüntüle ve düzenle’ye tıklayın

![](media/migrate-adfs-apps-to-azure/migrate4.png)
- Düzenlemek için belirli bir özniteliğin satırına tıklayın veya yeni öznitelik eklemek için ‘Öznitelik ekle’ye tıklayın. 
![](media/migrate-adfs-apps-to-azure/migrate5.png)

#### <a name="assign-users-to-the-app"></a>Uygulamaya kullanıcı atama
Azure AD içindeki kullanıcılarınızın belirli bir SaaS uygulamasında oturum açabilmeleri için, onlara Azure AD'nin içinden erişim verilmelidir.

Azure AD portalında kullanıcıları atamak için, portalın içinde SaaS uygulamasının ekranına gidin ve kenar çubuğunda “Kullanıcılar ve gruplar” öğesine tıklayın. Kullanıcı veya grup eklemek için, ekranın en üstündeki “Kullanıcı ekle” öğesine tıklayın. 

![](media/migrate-adfs-apps-to-azure/migrate6.png) 

![](media/migrate-adfs-apps-to-azure/migrate7.png) Erişimi doğrulamak için, kullanıcının oturum açarken söz konusu SaaS uygulamasını [erişim panelinde](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) görmesi gerekir; bu, http://myapps.microsoft.com adresinde bulunur. Örneğin, kullanıcının hem Salesforce hem de ServiceNow'a erişimi başarıyla atanmıştır.

![](media/migrate-adfs-apps-to-azure/migrate8.png)

### <a name="configuring-the-saas-app"></a>SaaS uygulamasını yapılandırma
Şirket içi federasyondan Azure AD'ye tam geçiş işlemi, üzerinde çalıştığınız SaaS uygulamasının birden çok Kimlik Sağlayıcı (IdP) destekleyip desteklemediğine bağlıdır.  Aşağıda, birden çok IdP desteğiyle ilgili sık karşılaşılan bazı sorular verilmiştir:

   **S: Bir uygulamanın birden çok IdP'yi desteklemesi ne anlama gelir?**
    
   Y: Birden çok IdP'yi destekleyen SaaS uygulamaları, oturum açma deneyimindeki değişikliği işlemeden önce yeni IdP (bizim durumumuzda Azure AD) hakkındaki tüm bilgileri girmenize olanak tanır.  Yapılandırma bittikten sonra, Azure AD'ye işaret etmek için uygulamanın kimlik doğrulama yapılandırmasına geçebilirsiniz.

   S: SaaS uygulamasının birden çok IdP'yi desteklemesi neden önemlidir?

   Y: Birden çok IdP desteklenmiyorsa, yöneticinin hizmet veya bakım kesintisi olarak kısa bir süre ayırması ve bu sürede Azure AD'yi uygulamanın yeni IdP'si olarak yapılandırması gerekecektir. Bu kesinti sırasında, kullanıcılara hesaplarında oturum açamayacakları bildirilmelidir.

   Uygulama birden çok IdP'yi desteklemiyorsa, ek IdP'nin yapılandırması önceden yapılabilir ve böylelikle yönetici Azure tam geçişi sırasında doğrudan IdP'yi değiştirebilir.

   Buna ek olarak, uygulama birden çok IdP'yi destekliyorsa ve oturum açma için kimlik doğrulamasını birden çok IdP'nin eşzamanlı olarak işlemesini seçerseniz, kullanıcıya oturum açma sayfasında kimlik doğrulaması için IdP seçme olanağı sağlanır.

#### <a name="example-multiple-idp-support"></a>Örnek: birden çok IdP desteği
Örneğin Salesforce'ta, IDP yapılandırması Settings -> Company Settings -> My Domain -> Authentication Configuration altında bulunabilir.

![](media/migrate-adfs-apps-to-azure/migrate9.png)

Yapılandırma daha önce Identity-> Single sign-on settings altında oluşturulduğundan, kimlik doğrulama yapılandırması için IdP'nizi diyelim ki AD FS'den Azure AD'ye geçirebilirsiniz. 

![](media/migrate-adfs-apps-to-azure/migrate10.png)

### <a name="optional-configure-user-provisioning-in-azure-ad"></a>İsteğe bağlı: Azure AD'de kullanıcı sağlamayı yapılandırma
İsteğe bağlı olarak, belirli bir SaaS uygulaması için Azure AD'nin kullanıcı sağlamayı doğrudan işlemesini isterseniz, Azure AD'de kurumsal uygulamalar için kullanıcı hesabı sağlamayı yönetme hakkındaki belgeye bakın.

## <a name="next-steps"></a>Sonraki adımlar

- [Uygulamaları Azure Active Directory ile yönetme](active-directory-enable-sso-scenario.md)
- [Uygulamalara erişimi yönetme](active-directory-managing-access-to-apps.md)
- [Azure AD Connect Federasyonu](active-directory-aadconnectfed-whatis.md)