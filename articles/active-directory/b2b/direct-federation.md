---
title: B2B - Azure Active Directory için bir kimlik sağlayıcısı ile doğrudan Federasyon ayarlama | Microsoft Docs
description: Doğrudan Konuklar, Azure AD uygulamaları için oturum açabilmeniz SAML veya WS-Federasyon kimlik sağlayıcısı ile federasyona eklemek
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 07/01/2019
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: a4dadc68e78fbaa979751d5bcd04ef481c3ab886
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67544354"
---
# <a name="direct-federation-with-ad-fs-and-third-party-providers-for-guest-users-preview"></a>AD FS ve üçüncü taraf sağlayıcılar için konuk kullanıcılar (Önizleme) ile doğrudan Federasyon
|     |
| --- |
| Doğrudan Federasyon, Azure Active Directory genel Önizleme özelliğidir. Önizlemeler hakkında daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).|
|     |

Bu makalede, B2B işbirliği için başka bir kuruluşla doğrudan Federasyon kurmak açıklar. Kimlik sağlayıcısı (IDP) SAML 2.0 veya WS-Federasyon protokolünü destekleyen herhangi bir kuruluşla doğrudan Federasyon ayarlayabilirsiniz.
Bir iş ortağının IDP ile doğrudan Federasyon ayarladığınızda, bu etki alanından yeni Konuk kullanıcıları Azure AD kiracınız ile oturum açın ve sizinle İşbirliği yapmaya başlamak için kendi IDP tarafından yönetilen bir kuruluş hesabı kullanabilirsiniz. Gerekmez Konuk kullanıcının ayrı bir oluşturmak için Azure AD hesabı.
> [!NOTE]
> Federasyon Konuk kullanıcılar gerekir oturum Kiracı bağlamını içeren bir bağlantı kullanarak doğrudan (örneğin, `https://myapps.microsoft.com/?tenantid=<tenant id>` veya `https://portal.azure.com/<tenant id>`, doğrulanmış bir etki alanı söz konusu olduğunda veya `https://myapps.microsoft.com/\<verified domain>.onmicrosoft.com`). Kiracı bağlam içerirler sürece uygulamalarına ve kaynaklarına doğrudan bağlantılar da çalışır. Doğrudan Federasyon kullanıcıları hiçbir Kiracı bağlamına sahip ortak uç noktaları kullanarak oturum şu anda belirleyemiyoruz. Örneğin, kullanarak `https://myapps.microsoft.com`, `https://portal.azure.com`, veya `https://teams.microsoft.com` bir hataya neden olur.
 
## <a name="when-is-a-guest-user-authenticated-with-direct-federation"></a>Bir konuk kullanıcının ne zaman ile doğrudan Federasyon kimliği?
Bir kuruluşla doğrudan Federasyon ayarladıktan sonra tüm yeni Konuk kullanıcıları davet doğrudan Federasyon kullanılarak doğrulanır. Doğrudan Federasyon ayarı sizden davet zaten yararlandınız Konuk kullanıcılar için kimlik doğrulama yöntemini değişmez dikkat edin önemlidir. Bazı örnekler şunlardır:
 - Konuk kullanıcıları davet, ve daha sonra kuruluş ile doğrudan Federasyon ayarlamanıza kullandıysanız, bu konuk kullanıcılara, doğrudan Federasyon ayarlamadan önce kullandıkları aynı kimlik doğrulama yöntemini kullanmaya devam eder.
 - Konuk kullanıcıları davet ve iş ortağı kuruluşla doğrudan Federasyon ayarlayın ve ardından daha sonra iş ortağı kuruluş Azure AD'ye taşır, Konuk kullanıcılar davet zaten yararlandınız doğrudan Federasyon, doğrudan uzun kullanmaya devam eder kiracınızdaki Federasyon ilkesi var.
 - Bir iş ortağı kuruluşla doğrudan Federasyon silerseniz, şu anda doğrudan Federasyon kullanarak tüm Konuk kullanıcılar oturum açmanız mümkün olmayacaktır.

Tüm bu senaryolar, Konuk kullanıcının kimlik doğrulama yöntemi, dizinden Konuk kullanıcı hesabı silme ve bunları reinviting güncelleştirebilirsiniz.

Doğrudan Federasyon, örneğin contoso.com ve fabrikam.com etki alanı ad alanlarına bağlıdır. AD FS'den veya üçüncü taraf IDP doğrudan Federasyon yapılandırmasıyla belirlerken, kuruluşlar, bir veya daha fazla etki alanı ad alanları için bu Idp'yi ilişkilendirin. 

## <a name="end-user-experience"></a>Son kullanıcı deneyimi 
Doğrudan Federasyon ile Konuk kullanıcılar kendi Kurumsal hesap kullanarak Azure AD kiracınız ile oturum açın. Paylaşılan kaynaklara erişme ve oturum açma için istenir, doğrudan Federasyon kullanıcıları için kendi IDP yönlendirilir. Başarılı oturum açma işleminden sonra kaynaklara erişmek için Azure AD'ye döndürülür. Federasyon kullanıcıları yenileme belirteçleri 12 saat boyunca geçerlidir doğrudan [varsayılan geçiş yenileme belirteci için uzunluk](../develop/active-directory-configurable-token-lifetimes.md#exceptions) Azure AD'de. Federasyon IDP SSO etkin ise kullanıcı SSO bir deneyim ve hiçbir oturum açma istemi ilk kimlik doğrulamasından sonra görmezsiniz.

## <a name="limitations"></a>Sınırlamalar

### <a name="dns-verified-domains-in-azure-ad"></a>Azure AD etki alanı DNS doğrulandı
Federasyon yalnızca etki alanları için izin verilen doğrudan ***değil*** Azure AD'de DNS doğrulandı. DNS doğrulandı olmadığından yönetilmeyen (e-posta adresi doğrulanan veya "viral") Azure AD kiracıları için doğrudan Federasyon izin verilir.
### <a name="authentication-url"></a>Kimlik doğrulama URL'si
Doğrudan Federasyon yalnızca kimlik doğrulama iş URL'SİNİN etki alanını hedef etki alanına eşleştiği veya kimlik doğrulama URL'si (Bu liste değiştirilebilir değer) Bu izin verilen kimlik sağlayıcılarından birini olduğu ilkeleri için izin verilir:
-   Accounts.Google.com
-   pingidentity.com
-   Login.pingone.com
-   okta.com
-   my.salesforce.com
-   Federation.exostar.com
-   Federation.exostartest.com

Örneğin, doğrudan Federasyon için ayarlarken **fabrikam.com**, kimlik doğrulama URL'si `https://fabrikam.com/adfs` doğrulama geçer. Bir konak aynı etki alanında da, örneğin geçer `https://sts.fabrikam.com/adfs`. Ancak, kimlik doğrulama URL'si `https://fabrikamconglomerate.com/adfs` veya `https://fabrikam.com.uk/adfs` için aynı etki alanında geçirmeniz gerekmez.

### <a name="signing-certificate-renewal"></a>İmzalama sertifikasını yenileme
Meta veri URL'si kimlik sağlayıcı ayarları belirtirseniz, belirtecin süresi dolduğunda, Azure AD imzalama sertifikasını otomatik olarak yenilenecektir. Ancak, sertifika, geçerlilik sonu zamanından önce herhangi bir nedenle döndürüldüğüne veya meta veri URL'si sağlamıyorsa, Azure AD yenilemek mümkün olmayacaktır. Bu durumda, imzalama sertifikasını el ile güncellemeniz gerekecektir.
## <a name="frequently-asked-questions"></a>Sık sorulan sorular
### <a name="can-i-set-up-direct-federation-with-an-unmanaged-email-verified-tenant"></a>(E-posta adresi doğrulanan) yönetilmeyen bir kiracı ile doğrudan Federasyon oluşturan ayarlayabilir miyim? 
Evet. Etki alanını doğruladıysanız taşınmadığından ve Kiracı undergone edilmemiş bir [yönetici devralma işlemini](../users-groups-roles/domains-admin-takeover.md), doğrudan Federasyon ayarlayabilirsiniz. Bir kullanıcı B2B davet redeems ya da şu anda mevcut olmayan bir etki alanı kullanmak için Azure AD Self Servis kayıt gerçekleştirir, yönetilmeyen veya e-posta adresi doğrulanan, kiracılar oluşturulur. Bu etki alanlarıyla doğrudan Federasyon ayarlayabilirsiniz. Bir DNS doğrulanmış etki alanı, Azure portalında veya PowerShell aracılığıyla doğrudan Federasyon kurmak çalışırsanız, bir hata görürsünüz.
### <a name="if-direct-federation-and-email-one-time-passcode-authentication-are-both-enabled-which-method-takes-precedence"></a>Her ikisi de doğrudan Federasyon ve e-posta bir kerelik geçiş kodu kimlik doğrulaması etkinleştirilip etkinleştirilmediğini, hangi yöntemin öncelik kazanır?
Bir iş ortağı kuruluşla doğrudan Federasyon oluşturulduğunda, bu e-posta bir kerelik geçiş kodu kimlik doğrulaması için söz konusu kuruluştaki yeni Konuk kullanıcıları önceliklidir. Konuk kullanıcı davet doğrudan Federasyon ayarlamadan önce bir kerelik geçiş kodu kimlik doğrulaması kullanarak alınma tarihinden itibaren bir kerelik geçiş kodu kimlik doğrulaması kullanmaya devam edeceğiz. 
### <a name="does-direct-federation-address-sign-in-issues-due-to-a-partially-synced-tenancy"></a>Federasyon oturum açma sorunları nedeniyle kısmen eşitlenmiş bir kiralama doğrudan mu?
Hayır, [bir kerelik geçiş kodu e-posta](one-time-passcode.md) özelliğinin bu senaryoda kullanılması gerekir. Burada şirket içi kullanıcı kimliklerinizi tam olarak buluta eşitlenmesini olmayan bir iş ortağı Azure AD kiracısı için bir "kısmen kiralama eşitlenen" ifade eder. Konuk kimliği bulutta henüz mevcut olmayan ancak kullanan B2B davetini dener oturum açmanız mümkün olmayacaktır. Bir kerelik geçiş kodu özellik oturum açmak bu Konuk çalıştırmasına olanak tanır. Doğrudan federasyon özelliği burada Konuk kendi IDP tarafından yönetilen bir kuruluş hesabı varsa, ancak kuruluş hiçbir Azure AD hiç faaliyet senaryoları ele alır.

## <a name="step-1-configure-the-partner-organizations-identity-provider"></a>1\. adım: İş ortağı kuruluşun kimlik sağlayıcısı yapılandırma
İlk olarak, iş ortağı kuruluşunuzun gerekli talepler ve bağlı olan taraf Güvenleri ile kendi kimlik sağlayıcısı yapılandırması gerektiğini belirtir. 

> [!NOTE]
> Doğrudan Federasyon için bir kimlik sağlayıcısı yapılandırmak nasıl göstermek için Active Directory Federasyon Hizmetleri (AD FS) örnek olarak kullanacağız. Makaleye göz atın [doğrudan Federasyon ile AD FS yapılandırma](direct-federation-adfs.md), hazırlık doğrudan Federasyon için SAML 2.0 veya WS-Federasyon kimlik sağlayıcısı olarak AD FS yapılandırma örnekleri sağlar.

### <a name="saml-20-configuration"></a>SAML 2.0 yapılandırması

Azure AD B2B, aşağıda listelenen belirli gereksinimleri olan SAML protokolü kullanan kimlik sağlayıcıları ile federasyona eklemek için yapılandırılabilir. SAML kimlik sağlayıcısı ve Azure AD arasında güven oluşturma hakkında daha fazla bilgi için bkz. [çoklu oturum açma için SAML 2.0 kimlik sağlayıcısı (IDP) kullanan](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-fed-saml-idp).  

> [!NOTE]
> Not hedef etki alanını Federasyon doğrudan Azure AD'de DNS doğrulandı olmamalıdır. Kimlik doğrulama URL'si etki alanını hedef etki alanına eşleşmelidir veya izin verilen kimlik sağlayıcısı etki alanı olması gerekir. Bkz: [sınırlamaları](#limitations) ayrıntıları bölümü. 

#### <a name="required-saml-20-attributes-and-claims"></a>Gerekli SAML 2.0 öznitelikleri ve talepler
Aşağıdaki tablolar, özel öznitelikler ve üçüncü taraf kimlik sağlayıcısında yapılandırılmalıdır talep gereksinimleri gösterir. Doğrudan Federasyon ayarlamak için aşağıdaki öznitelikleri SAML 2.0 yanıtındaki kimlik sağlayıcısından alınmış olması gerekir. Bu öznitelikler, bağlama çevrimiçi güvenlik belirteci hizmeti XML dosyasını veya bunları el ile girerek yapılandırılabilir.

SAML 2.0 Idp'yi yanıttan için gerekli öznitelikler:

|Öznitelik  |Değer  |
|---------|---------|
|AssertionConsumerService     |`https://login.microsoftonline.com/login.srf`         |
|Hedef kitle     |`urn:federation:MicrosoftOnline`         |
|Veren     |İş ortağı IDP, örneğin URI'sini veren `http://www.example.com/exk10l6w90DHM0yi...`         |


IDP tarafından verilen SAML 2.0 belirteç için gerekli talep:

|Öznitelik  |Değer  |
|---------|---------|
|Nameıd biçimi     |`urn:oasis:names:tc:SAML:2.0:nameid-format:persistent`         |
|emailaddress     |`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`         |

### <a name="ws-fed-configuration"></a>WS-Federasyon yapılandırması 
Azure AD B2B, aşağıda listelenen bazı belirli gereksinimlerine WS-Federasyon protokolünü kullanan kimlik sağlayıcıları ile federasyona eklemek için yapılandırılabilir. Şu anda Azure AD ile uyumluluk eklemek için AD FS ve Shibboleth iki WS-Federasyon sağlayıcıları test edilmiştir. "WS protokolleri kullanılarak STS tümleştirme kağıt" Azure AD ile uyumlu bir WS-Federasyon sağlayıcısı arasında bağlı olan taraf güveni oluşturma hakkında daha fazla bilgi için bkz bulunan [Azure AD kimlik sağlayıcısı uyumluluk belgeleri](https://www.microsoft.com/download/details.aspx?id=56843).

> [!NOTE]
> Hedef etki alanını Federasyon doğrudan Azure AD'de DNS doğrulandı olmamalıdır. Kimlik doğrulama URL'si etki alanını hedef etki alanına veya etki alanının izin verilen kimlik sağlayıcısı eşleşmelidir. Bkz: [sınırlamaları](#limitations) ayrıntıları bölümü. 

#### <a name="required-ws-fed-attributes-and-claims"></a>Gerekli WS-Federasyon öznitelikleri ve talepler

Aşağıdaki tablolar, özel öznitelikler ve üçüncü taraf WS-Federasyon kimlik sağlayıcısında yapılandırılmalıdır talep gereksinimleri gösterir. Doğrudan Federasyon ayarlamak için aşağıdaki öznitelikleri WS-Federasyon iletisi kimlik sağlayıcısından alınmış olması gerekir. Bu öznitelikler, bağlama çevrimiçi güvenlik belirteci hizmeti XML dosyasını veya bunları el ile girerek yapılandırılabilir.

Idp'nin WS-Federasyon iletisinden gerekli öznitelikleri:
 
|Öznitelik  |Değer  |
|---------|---------|
|PassiveRequestorEndpoint     |`https://login.microsoftonline.com/login.srf`         |
|Hedef kitle     |`urn:federation:MicrosoftOnline`         |
|Veren     |İş ortağı IDP, örneğin URI'sini veren `http://www.example.com/exk10l6w90DHM0yi...`         |

IDP tarafından verilen WS-Federasyon belirteç için gerekli talep:

|Öznitelik  |Değer  |
|---------|---------|
|Immutableıd     |`http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID`         |
|emailaddress     |`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`         |

## <a name="step-2-configure-direct-federation-in-azure-ad"></a>2\. adım: Azure AD'de doğrudan Federasyonu yapılandırma 
Ardından, 1. adımda Azure AD'de yapılandırılmış kimlik sağlayıcısı ile Federasyon yapılandıracaksınız. Azure AD portalı veya PowerShell kullanabilirsiniz. Bu, doğrudan Federasyon İlkesi uygulanmadan önce 5-10 dakika sürebilir. Bu süre boyunca doğrudan Federasyon etki alanı için bir davetini denemeyin. Aşağıdaki öznitelikler gereklidir:
- İş ortağı IDP URI'sini veren
- İş ortağı (yalnızca https desteklenir) IDP pasif kimlik doğrulama uç noktası
- Sertifika

### <a name="to-configure-direct-federation-in-the-azure-ad-portal"></a>Azure AD portalında doğrudan Federasyon yapılandırmak için

1. [Azure Portal](https://portal.azure.com/) gidin. Sol bölmede **Azure Active Directory**’yi seçin. 
2. Seçin **kuruluş ilişkileri**.
3. Seçin **kimlik sağlayıcıları**ve ardından **yeni SAML/WS-Federasyon IDP**.

    ![Yeni SAML veya WS-Federasyon IDP ekleme düğmesinin Ekran görüntüsü gösterildiği](media/direct-federation/new-saml-wsfed-idp.png)

4. Üzerinde **yeni SAML/WS-Federasyon IDP** sayfasındaki **kimlik sağlayıcısı Protokolü**seçin **SAML** veya **WS-FEDERASYON**.

    ![SAML veya WS-Federasyon IDP sayfasında ayrıştırma düğmesinin gösterildiği ekran görüntüsü](media/direct-federation/new-saml-wsfed-idp-parse.png)

5. Doğrudan Federasyon için hedef etki alanı adı, iş ortağı kuruluşunuzun etki alanı adı girin
6. Meta veri ayrıntıları doldurmak için bir meta veri dosyasını karşıya yükleyebilirsiniz. Giriş meta verileri el ile kullanmayı tercih ederseniz, aşağıdaki bilgileri girin:
   - İş ortağı IDP etki alanı adı
   - İş ortağı IDP varlık kimliği
   - İş ortağı IDP Edilgen İstek sahibi uç noktası
   - Sertifika
   > [!NOTE]
   > Meta veri URL'si, kullanmanızı kesinlikle öneririz ancak isteğe bağlıdır. Meta veri URL'sini sağlarsanız, belirtecin süresi dolduğunda, Azure AD'ye otomatik olarak imzalama sertifikasının yenileyebilirsiniz. Sertifika geçerlilik sonu zamanından önce herhangi bir nedenle döndürüldüğüne veya meta veri URL'si sağlamazsanız, Azure AD yenilemek mümkün olmayacaktır. Bu durumda, imzalama sertifikasını el ile güncellemeniz gerekecektir.

7. **Kaydet**’i seçin. 

### <a name="to-configure-direct-federation-in-azure-ad-using-powershell"></a>PowerShell kullanarak Azure AD'de doğrudan Federasyon yapılandırmak için

1. Azure AD PowerShell graf modülü için en son sürümünü yükleyin ([AzureADPreview](https://www.powershellgallery.com/packages/AzureADPreview)). (Ayrıntılı adımlar gerekiyorsa, bu hızlı başlangıçta Konuk kullanıcı ekleme bölüm içerir [son AzureADPreview modülünü yükleme](b2b-quickstart-invite-powershell.md#install-the-latest-azureadpreview-module).) 
2. Şu komutu çalıştırın: 
   ```powershell
   Connect-AzureAD
   ```
1. Oturum açma isteminde yönetilen genel yönetici hesabıyla oturum açın. 
2. Federasyon meta veri dosyasından değerleri değiştirerek aşağıdaki komutları çalıştırın. AD FS sunucusu ve okta'yı Federasyon federationmetadata.xml, örneğin dosyasıdır: `https://sts.totheclouddemo.com/federationmetadata/2007-06/federationmetadata.xml`. 

   ```powershell
   $federationSettings = New-Object Microsoft.Open.AzureAD.Model.DomainFederationSettings
   $federationSettings.PassiveLogOnUri ="https://sts.totheclouddemo.com/adfs/ls/"
   $federationSettings.LogOffUri = $federationSettings.PassiveLogOnUri
   $federationSettings.IssuerUri = "http://sts.totheclouddemo.com/adfs/services/trust"
   $federationSettings.MetadataExchangeUri="https://sts.totheclouddemo.com/adfs/services/trust/mex"
   $federationSettings.SigningCertificate= <Replace with X509 signing cert’s public key>
   $federationSettings.PreferredAuthenticationProtocol="WsFed" OR "Samlp"
   $domainName = <Replace with domain name>
   New-AzureADExternalDomainFederation -ExternalDomainName $domainName  -FederationSettings $federationSettings
   ```

## <a name="step-3-test-direct-federation-in-azure-ad"></a>3\. adım: Azure AD'de Federasyon doğrudan test
Artık doğrudan Federasyon kurulumunuzu yeni B2B Konuk kullanıcı davet ederek test edin. Ayrıntılar için bkz [ekleyin, Azure AD B2B işbirliği kullanıcılarını Azure portalında](add-users-administrator.md).
 
## <a name="how-do-i-edit-a-direct-federation-relationship"></a>Bir doğrudan Federasyon ilişkisi nasıl düzenleyebilirim?

1. [Azure Portal](https://portal.azure.com/) gidin. Sol bölmede **Azure Active Directory**’yi seçin. 
2. Seçin **kuruluş ilişkileri**.
3. Seçin **kimlik sağlayıcıları**
4. Altında **SAML/WS-Federasyon kimlik sağlayıcılarını**, sağlayıcıyı seçin.
5. Kimlik sağlayıcısı Ayrıntılar bölmesinde değerlerini güncelleştirin.
6. **Kaydet**’i seçin.


## <a name="how-do-i-remove-direct-federation"></a>Doğrudan Federasyon nasıl kaldırabilirim?
Doğrudan Federasyon kurulumunuzu kaldırabilirsiniz. Bunu yaparsanız, davetlerini zaten yararlandınız doğrudan Federasyon Konuk kullanıcılar oturum açmanız mümkün olmayacaktır. Ancak bunları erişim kaynaklarınıza yeniden dizinden silerek ve bunları reinviting tanıyabilirsiniz. Azure AD portalında bir kimlik sağlayıcısı ile doğrudan Federasyon kaldırmak için:

1. [Azure Portal](https://portal.azure.com/) gidin. Sol bölmede **Azure Active Directory**’yi seçin. 
2. Seçin **kuruluş ilişkileri**.
3. Seçin **kimlik sağlayıcıları**.
4. Kimlik sağlayıcısı seçin ve ardından **Sil**. 
5. Seçin **Evet** silme işlemini onaylamak için. 

PowerShell kullanarak bir kimlik sağlayıcısı ile doğrudan Federasyon kaldırmak için:
1. Azure AD PowerShell graf modülü için en son sürümünü yükleyin ([AzureADPreview](https://www.powershellgallery.com/packages/AzureADPreview)).
2. Şu komutu çalıştırın: 
   ```powershell
   Connect-AzureAD
   ```
3. Oturum açma isteminde yönetilen genel yönetici hesabıyla oturum açın. 
4. Aşağıdaki komutu girin:
   ```powershell
   Remove-AzureADExternalDomainFederation -ExternalDomainName  $domainName
   ```
