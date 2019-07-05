---
title: B2B - Azure Active Directory için bir AD FS ile doğrudan Federasyon ayarlama | Microsoft Docs
description: Konuklar, Azure AD uygulamaları için oturum açabilmeniz için doğrudan Federasyon kimlik sağlayıcısı olarak AD FS ayarlamayı öğrenin
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
ms.openlocfilehash: a8f709186f0ef17e037c4203118be07ea2d4f511
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67544328"
---
# <a name="example-direct-federation-with-active-directory-federation-services-ad-fs-preview"></a>Örnek: Active Directory Federasyon Hizmetleri (AD FS) ile doğrudan Federasyon (Önizleme)
|     |
| --- |
| Doğrudan Federasyon, Azure Active Directory genel Önizleme özelliğidir. Önizlemeler hakkında daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).|
|     |

Bu makalede nasıl ayarlandığı [doğrudan Federasyon](direct-federation.md) SAML 2.0 veya WS-Federasyon kimlik sağlayıcısı olarak Active Directory Federasyon Hizmetleri (AD FS) kullanarak. Doğrudan federasyonu'nu destekleyecek şekilde belirli öznitelikleri ve talepler kimlik sağlayıcısında yapılandırılmalıdır. Doğrudan Federasyon için bir kimlik sağlayıcısı yapılandırmak nasıl göstermek için Active Directory Federasyon Hizmetleri (AD FS) örnek olarak kullanacağız. AD FS SAML kimlik sağlayıcısı olarak hem bir WS-Federasyon kimlik sağlayıcısı olarak ayarlamak nasıl göstereceğiz.

> [!NOTE]
> Bu makalede, SAML ve WS-Federasyon için gösterim amacıyla AD FS ayarlanacağını açıklar. AD FS kimlik sağlayıcısı olduğu için doğrudan Federasyon tümleştirmeler WS-Federasyon protokol olarak kullanmanızı öneririz. 

## <a name="configure-ad-fs-for-saml-20-direct-federation"></a>SAML 2.0 doğrudan Federasyon için AD FS'yi yapılandırma
Azure AD B2B, aşağıda listelenen belirli gereksinimleri olan SAML protokolü kullanan kimlik sağlayıcıları ile federasyona eklemek için yapılandırılabilir. SAML yapılandırma adımlarını anlamak için bu bölümü AD FS SAML 2.0 için ayarlama işlemi gösterilmektedir. 

Doğrudan Federasyon ayarlamak için aşağıdaki öznitelikleri SAML 2.0 yanıtındaki kimlik sağlayıcısından alınmış olması gerekir. Bu öznitelikler, bağlama çevrimiçi güvenlik belirteci hizmeti XML dosyasını veya bunları el ile girerek yapılandırılabilir. Adımda 12 [test AD FS örneği oluşturma](https://medium.com/in-the-weeds/create-a-test-active-directory-federation-services-3-0-instance-on-an-azure-virtual-machine-9071d978e8ed) AD FS bitiş noktaları bulma veya oluşturmayı, meta veri URL'si, örneğin açıklayan `https://fs.iga.azure-test.net/federationmetadata/2007-06/federationmetadata.xml`. 

|Öznitelik  |Değer  |
|---------|---------|
|AssertionConsumerService     |`https://login.microsoftonline.com/login.srf`         |
|Hedef kitle     |`urn:federation:MicrosoftOnline`         |
|Veren     |İş ortağı IDP, örneğin URI'sini veren `http://www.example.com/exk10l6w90DHM0yi...`         |

Aşağıdaki talep kimlik sağlayıcısı tarafından verilen SAML 2.0 belirtecin, yapılandırılmış olması gerekir:


|Öznitelik  |Değer  |
|---------|---------|
|Nameıd biçimi     |`urn:oasis:names:tc:SAML:2.0:nameid-format:persistent`         |
|emailaddress     |`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`         |


Sonraki bölümde, gerekli öznitelikler ve talepleri AD FS kullanarak örnek bir SAML 2.0 kimlik sağlayıcısı olarak nasıl yapılandırılacağı gösterilmektedir.

### <a name="before-you-begin"></a>Başlamadan önce

AD FS sunucusu zaten ayarlanmalıdır yukarı ve bu yordama başlamadan önce çalışmıyor. AD FS sunucusu kurma konusunda yardım için bkz. [Azure sanal makinesinde bir test AD FS 3.0 örneği oluşturma](https://medium.com/in-the-weeds/create-a-test-active-directory-federation-services-3-0-instance-on-an-azure-virtual-machine-9071d978e8ed).

### <a name="add-the-claim-description"></a>Talep açıklaması ekleme

1. AD FS sunucunuza seçin **Araçları** > **AD FS Yönetimi**.
2. Gezinti bölmesinde seçin **hizmet** > **talep açıklamaları**.
3. Altında **eylemleri**seçin **talep açıklaması ekleme**.
4. İçinde **talep açıklaması ekleme** penceresinde aşağıdaki değerleri belirtin:

   - **Görünen ad**: Kalıcı tanımlayıcı
   - **Talep tanıtıcısı**: `urn:oasis:names:tc:SAML:2.0:nameid-format:persistent` 
   - Onay kutusunu seçin **bu talep açıklaması Federasyon meta verilerinde Federasyon Hizmeti'nin kabul edebileceği bir talep türü olarak Yayımla**.
   - Onay kutusunu seçin **bu talep açıklaması Federasyon meta verilerinde Federasyon Hizmeti'nin gönderebileceği bir talep türü olarak Yayımla**.

5. **Tamam**’a tıklayın.

### <a name="add-the-relying-party-trust-and-claim-rules"></a>Bağlı olan taraf güveni ekleme ve talep kuralları

1. AD FS sunucusunda Git **Araçları** > **AD FS Yönetimi**.
2. Gezinti bölmesinde seçin **güven ilişkileri** > **bağlı olan taraf güvenleri**.
3. Altında **eylemleri**seçin **bağlı olan taraf güveni Ekle**. 
4. İçin ekleme bağlı olan taraf güveni Sihirbazı'nda **veri kaynağı Seç**, bu seçeneği kullanın **çevrimiçi veya yerel ağda yayımlanan bağlı olan taraf hakkındaki verileri içeri aktar**. Bu Federasyon meta verileri URL'sini - belirleyen https://nexus.microsoftonline-p.com/federationmetadata/saml20/federationmetadata.xml. Diğer varsayılan seçimleri bırakın. Seçin **Kapat**.
5. **Talep kurallarını Düzenle** Sihirbazı açılır.
6. İçinde **talep kurallarını Düzenle** seçin **Kuralı Ekle**. İçinde **kural türü seçin**seçin **LDAP özniteliklerini talep olarak Gönder**. **İleri**’yi seçin.
7. İçinde **talep kuralını yapılandırın**, aşağıdaki değerleri belirtin: 

   - **Talep kuralı adı**: E-posta talebi kuralı 
   - **Öznitelik deposu**: Active Directory 
   - **LDAP özniteliği**: E-posta adresleri 
   - **Giden talep türü**: E-posta adresi

8. **Son**’u seçin.
9. **Talep kurallarını Düzenle** penceresi, Yeni kuralın gösterilir. **Uygula**'ya tıklayın. 
10. **Tamam**’a tıklayın.  

### <a name="create-an-email-transform-rule"></a>Bir e-posta dönüştürme kuralı oluşturma
1. Git **talep kurallarını Düzenle** tıklatıp **Kuralı Ekle**. İçinde **kural türü seçin**seçin **gelen talebi Dönüştür** tıklatıp **sonraki**. 
2. İçinde **talep kuralını yapılandırın**, aşağıdaki değerleri belirtin: 

   - **Talep kuralı adı**: E-posta dönüştürme kuralı 
   - **Gelen talep türü**: E-posta adresi 
   - **Giden talep türü**: Ad kimliği 
   - **Giden ad kimliği biçimi**: Kalıcı tanımlayıcı 
   - **Tüm talep değerlerini geçir**’i seçin.

3. **Son**'a tıklayın. 
4. **Talep kurallarını Düzenle** penceresi, yeni kurallar gösterilir. **Uygula**'ya tıklayın. 
5. **Tamam** düğmesine tıklayın. AD FS sunucusu, SAML 2.0 protokolünü kullanarak doğrudan Federasyon için yapılandırılmıştır.

## <a name="configure-ad-fs-for-ws-fed-direct-federation"></a>WS-Federasyon doğrudan Federasyon için AD FS'yi yapılandırma 
Azure AD B2B, aşağıda listelenen belirli gereksinimleri olan WS-Federasyon protokolünü kullanan kimlik sağlayıcıları ile federasyona eklemek için yapılandırılabilir. Şu anda Azure AD ile uyumluluk eklemek için AD FS ve Shibboleth iki WS-Federasyon sağlayıcıları test edilmiştir. Burada, Active Directory Federasyon Hizmetleri (AD FS) WS-Federasyon kimlik sağlayıcısının örnek olarak kullanacağız. Azure AD ile uyumlu bir WS-Federasyon sağlayıcısı arasında bağlı olan taraf güveni oluşturma hakkında daha fazla bilgi için Azure AD kimlik sağlayıcısı uyumluluk belgelerini indirin.

Doğrudan Federasyon ayarlamak için aşağıdaki öznitelikleri WS-Federasyon iletisi kimlik sağlayıcısından alınmış olması gerekir. Bu öznitelikler, bağlama çevrimiçi güvenlik belirteci hizmeti XML dosyasını veya bunları el ile girerek yapılandırılabilir. Adımda 12 [test AD FS örneği oluşturma](https://medium.com/in-the-weeds/create-a-test-active-directory-federation-services-3-0-instance-on-an-azure-virtual-machine-9071d978e8ed) AD FS bitiş noktaları bulma veya oluşturmayı, meta veri URL'si, örneğin açıklayan `https://fs.iga.azure-test.net/federationmetadata/2007-06/federationmetadata.xml`.
 
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

Sonraki bölümde, gerekli öznitelikleri yapılandırma gösterir ve WS-Federasyon kimlik sağlayıcısının örnek olarak AD FS kullanarak talep.

### <a name="before-you-begin"></a>Başlamadan önce
AD FS sunucusu zaten ayarlanmalıdır yukarı ve bu yordama başlamadan önce çalışmıyor. AD FS sunucusu kurma konusunda yardım için bkz. [Azure sanal makinesinde bir test AD FS 3.0 örneği oluşturma](https://medium.com/in-the-weeds/create-a-test-active-directory-federation-services-3-0-instance-on-an-azure-virtual-machine-9071d978e8ed).


### <a name="add-the-relying-party-trust-and-claim-rules"></a>Bağlı olan taraf güveni ekleme ve talep kuralları 
1. AD FS sunucusunda Git **Araçları** > **AD FS Yönetimi**. 
1. Gezinti bölmesinde seçin **güven ilişkileri** > **bağlı olan taraf güvenleri**. 
1. Altında **eylemleri**seçin **bağlı olan taraf güveni Ekle**.  
1. İçin bağlı olan taraf güveni Sihirbazı Ekle **veri kaynağı Seç**, bu seçeneği kullanın **çevrimiçi veya yerel ağda yayımlanan bağlı olan taraf hakkındaki verileri içeri aktar**. Bu Federasyon meta verileri URL'sini belirtin: `https://nexus.microsoftonline-p.com/federationmetadata/2007-06/federationmetadata.xml`.  Diğer varsayılan seçimleri bırakın. Seçin **Kapat**.
1. **Talep kurallarını Düzenle** Sihirbazı açılır. 
1. İçinde **talep kurallarını Düzenle** seçin **Kuralı Ekle**. İçinde **kural türü seçin**seçin **talepleri özel kural kullanarak Gönder**. *İleri*’yi seçin. 
1. İçinde **talep kuralını yapılandırın**, aşağıdaki değerleri belirtin:

   - **Talep kuralı adı**: Sorunu sabit kimlik  
   - **Özel kural**: `c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"] => issue(store = "Active Directory", types = ("http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID"), query = "samAccountName={0};objectGUID;{1}", param = regexreplace(c.Value, "(?<domain>[^\\]+)\\(?<user>.+)", "${user}"), param = c.Value);`

1. **Son**’u seçin. 
1. **Talep kurallarını Düzenle** penceresi, Yeni kuralın gösterilir. **Uygula**'ya tıklayın.  
1. Aynı **talep kurallarını Düzenle** seçin **Kuralı Ekle**. İçinde **Cohose kural türü**seçin **LDAP özniteliklerini talep olarak Gönder**. **İleri**’yi seçin.
1. İçinde **talep kuralını yapılandırın**, aşağıdaki değerleri belirtin: 

   - **Talep kuralı adı**: E-posta talebi kuralı  
   - **Öznitelik deposu**: Active Directory  
   - **LDAP özniteliği**: E-posta adresleri  
   - **Giden talep türü**: E-posta adresi 

1.  **Son**’u seçin. 
1.  **Talep kurallarını Düzenle** penceresi, Yeni kuralın gösterilir. **Uygula**'ya tıklayın.  
1.  **Tamam** düğmesine tıklayın. AD FS sunucusu, WS-Federasyon kullanarak doğrudan Federasyon için yapılandırılmıştır.

## <a name="next-steps"></a>Sonraki adımlar
Ardından, artıracaksınız [doğrudan Federasyon Azure AD'de yapılandırırken](direct-federation.md#step-2-configure-direct-federation-in-azure-ad) Azure AD portalında veya PowerShell kullanarak. 
