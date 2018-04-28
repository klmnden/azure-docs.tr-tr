---
title: 'Azure Active Directory B2C: Özel ilke | Microsoft Docs'
description: Bir konu Azure Active Directory B2C özel ilkeler hakkında
services: active-directory-b2c
documentationcenter: ''
author: davidmu1
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 04/04/2017
ms.author: davidmu
ms.openlocfilehash: 22d34ac4128da1d1a9f20619aec2aaccc2425a21
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="azure-active-directory-b2c-custom-policies"></a>Azure Active Directory B2C: Özel ilkeler

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

## <a name="what-are-custom-policies"></a>Özel ilkeler nedir?

Özel ilkeler, Azure AD B2C kiracınızın davranışlarını tanımlayan yapılandırma dosyalarıdır. Oysa **yerleşik ilkeleri** önceden tanımlanmış en yaygın kimlik görevler için Azure AD B2C portalında özel ilkeler tam olarak görevleri yakın sınırsız sayıda tamamlamak için bir kimlik geliştirici tarafından düzenlenebilir. İçin okumaya özel ilkeler, sağa ve kimlik senaryonuz olup olmadığını belirleyin.

## <a name="comparing-built-in-policies-and-custom-policies"></a>Yerleşik ilkeleri ve özel ilkeler karşılaştırma

| | Yerleşik ilkeler | Özel ilkeler |
|-|-------------------|-----------------|
|Hedef Kullanıcılar | Tüm uygulama geliştiriciler ile veya olmadan kimlik uzmanlığı | Kimlik uzmanları: sistemleri tümleştiricileri, danışmanlarımızı ve şirket içi kimlik ekipler. Openıdconnect akışları ile deneyimliyseniz ve kimlik sağlayıcısı ve talep tabanlı kimlik doğrulaması anlama |
|Yapılandırma yöntemi | Azure portalıyla kullanıcı dostu bir kullanıcı Arabirimi | Doğrudan, XML dosyalarını düzenlemek ve Azure portalına karşıya yükleme |
|UI Özelleştirme | (Özel etki alanı gerektirir) HTML, CSS ve javascript desteği dahil olmak üzere, tam kullanıcı arabirimini özelleştirme<br><br>Özel dizeler ile birden çok dil desteği | Aynı |
| Öznitelik özelleştirme | Standart ve özel öznitelikler | Aynı |
|Belirteç ve oturum yönetimi | Özel belirteç ve birden çok oturum seçenekleri | Aynı |
|Kimlik Sağlayıcıları| **Bugün**: önceden tanımlanmış yerel, sosyal sağlayıcısı<br><br>**Gelecekte**: OIDC, SAML, standartlara dayalı OAuth | **Bugün**: OIDC, OAUTH, standartlara dayalı SAML<br><br>**Gelecekte**: WsFed |
|Kimlik görevler (örnekler) | Kaydolma veya yerel ve birçok sosyal hesap ile oturum<br><br>Self Servis Parola Sıfırlama<br><br>Profil düzenleme<br><br>Çok faktörlü kimlik doğrulama senaryoları<br><br>Belirteçleri ve oturumları özelleştirme<br><br>Erişim belirteci akışlar | Özel kimlik sağlayıcıları kullanarak yerleşik ilkeleri olarak aynı görevleri tamamlamak veya özel kapsamları kullanma<br><br>Sağlama kullanıcı sistemindeki başka bir zamanda kayıt<br><br>Kendi e-posta hizmeti sağlayıcısını kullanarak bir Hoş Geldiniz e-posta Gönder<br><br>Bir kullanıcı deposunun B2C dışında kullanın<br><br>Kullanıcı tarafından sağlanan güvenilir bir sistem API'si aracılığıyla bilgilerle doğrula |

## <a name="policy-files"></a>İlke dosyaları

Özel bir ilke birbirleriyle hiyerarşik bir zincirindeki başvuran bir veya birkaç XML biçimli dosya olarak temsil edilir. XML öğeleri tanımlayın: talep şema talep dönüşümleri, içerik tanımları, talep sağlayıcıları/teknik profilleri ve diğer öğeleri arasında Userjourney düzenleme adımlarının.

İlke dosyaları üç tür kullanılmasını öneririz:

- **TEMEL dosya**, çoğu tanımları ve tam bir örnek Azure sağlayan içerir.  Sorun giderme ve uzun süreli bakım, ilkelerinizin ile yardımcı olması için bu dosya için en az sayıda değişiklik yaparsanız öneririz
- **Uzantılar dosya** kiracınız için benzersiz yapılandırma değişikliklerini tutan
- **bir bağlı olan taraf (RP) dosyası** uygulama veya hizmet (diğer adıyla bağlı olan taraf) tarafından çağrılan tek görev odaklı dosyası olduğu.  Daha fazla bilgi için ilke dosya tanımları makalesini okuyun.  Benzersiz her görev kendi RP gerektirir ve gereksinimleri markalama bağlı olarak sayısı "kullanım örnekleri toplam sayısı x uygulamalarının toplam" olabilir.


Yukarıda gösterilen 3 dosya düzeni Azure AD B2C yerleşik ilkeleri izleyin, ancak Geliştirici Portalı arka planda uzantıları dosyasına değişiklikler yaparken, yalnızca bağlı olan taraf (RP) dosya görür.

## <a name="core-concepts-you-should-know-when-using-custom-policies"></a>Özel ilkeler kullanırken bilmeniz gereken temel kavramlar

### <a name="azure-active-directory-b2c"></a>Azure Active Directory B2C

Azure'nın müşteri kimlik ve erişim yönetimi (CIAM) hizmeti. Hizmet içerir:

1. Bir özel amaçlı Azure Active Directory erişilebilir Microsoft Graph ve yerel hesaplar ve Federasyon hesapları için kullanıcı verilerini tutan aracılığıyla biçiminde bir kullanıcı dizini 
2. Erişim **kimlik deneyimi Framework** hangi kullanıcıların ve varlıklar arasındaki güven düzenler ve bunlar arasında kimlik/erişim yönetim görevi tamamlamak için talep geçirir 
3. Kimliği belirteçleri, yenileme belirteçleri, erişim belirteçleri (ve eşdeğer SAML onaylar) verme ve bunları kaynakları korumak için doğrulama bir güvenlik belirteci hizmeti (STS).

Azure AD B2C, bir kimlik görevi başarmak sırayla kimlik sağlayıcıları, kullanıcıların, diğer sistemler ve yerel kullanıcı dizini ile etkileşim (örneğin bir kullanıcı oturum açma yeni bir kullanıcı kaydı, bir parola sıfırlama). Çok taraflı güven oluşturur ve bu adımları yürütür temel platform kimlik deneyimi Framework ve (bir kullanıcı gezisine veya güven framework ilkesi olarak da bilinir) bir ilke olarak adlandırılır açıkça tanımlayan aktörler, Eylemler, protokolleri ve tamamlamak için adımlar dizisi.

### <a name="identity-experience-framework"></a>Kimlik Deneyimi Altyapısı

Openıdconnect, OAuth, SAML, WSFed ve birkaç standart olmayan yorumlar (örneğin REST gibi standart protokol varlıkları (geniş çapta talep sağlayıcıları) arasında güven düzenler bir tam olarak yapılandırılabilir, ilke temelli, bulut tabanlı Azure platformu biçimleri API-based sistemi sistemi talep alışverişleri). I2E HTML, CSS ve javascript desteği kullanıcı dostu, beyaz etiketli deneyimleri oluşturur.  Bugün, kimlik deneyimi yalnızca Azure AD B2C hizmet bağlamında kullanılabilir ve CIAM için ilgili görevleri için öncelikli çerçevedir.

### <a name="built-in-policies"></a>Yerleşik ilkeler

Kullanılan kimlik (örn. kullanıcı kaydı, oturum açma, parola sıfırlama) görevler ve ilişkilerini de Azure AD B2C (için de önceden güvenilir taraflar etkileşimde en yaygın olarak gerçekleştirmek için Azure AD B2C davranışını doğrudan yapılandırma dosyalarını önceden tanımlanmış Örnek Facebook kimlik sağlayıcısı, LinkedIn, Microsoft Account, Google hesapları).  Gelecekte, yerleşik ilkeleri özelleştirmesi genellikle Azure Active Directory Premium, Active Directory/ADFS, Salesforce kimlik sağlayıcısı vb. gibi Kurumsal bölgedeki kimlik sağlayıcıları için de sağlayabilir.


### <a name="custom-policies"></a>Özel ilkeler

Azure AD B2C kiracınızda kimlik deneyimi Framework davranışını tanımlamak yapılandırma dosyaları. Özel bir ilke, kimlik deneyimi bağlı olan taraf (örneğin bir uygulama) tarafından çağrıldığında Framework tarafından yürütülen (ilke dosyaları tanımları bakın) bir veya birkaç XML dosyaları olarak erişilebilir. Özel ilkeler doğrudan görevleri yakın sınırsız sayıda tamamlamak için bir kimlik geliştirici tarafından düzenlenebilir. Özel ilkeler yapılandırma geliştiriciler güvenilir ilişkiler meta veri uç noktalarını içerecek şekilde dikkatli ayrıntılı olarak tanımlamanız gerekir, tam talep tanımları exchange ve gizli anahtarları, anahtarlar ve sertifikalar her kimlik sağlayıcısı tarafından gerektiği şekilde yapılandırın.

## <a name="policy-file-definitions-for-identity-experience-framework-trustframeworks"></a>Kimlik deneyimi Framework Trustframeworks ilke dosyası tanımları

### <a name="policy-files"></a>İlke dosyaları

Özel bir ilke birbirleriyle hiyerarşik bir zincirindeki başvuran bir veya birkaç XML biçimli dosya olarak temsil edilir. XML öğeleri tanımlayın: talep şema talep dönüşümleri, içerik tanımları, talep sağlayıcıları/teknik profilleri ve diğer öğeleri arasında kullanıcı gezisine düzenleme adımlarının.  İlke dosyaları üç tür kullanılmasını öneririz:

- **TEMEL dosya** çoğu tanımları ve tam bir örnek Azure sağlayan içerir.  Sorun giderme ve uzun süreli bakım, ilkelerinizin ile yardımcı olması için bu dosya için en az sayıda değişiklik yaparsanız öneririz
- **Uzantılar dosya** kiracınız için benzersiz yapılandırma değişikliklerini tutan
- **bir bağlı olan taraf (RP) dosyası** uygulama veya hizmet (diğer adıyla bağlı olan taraf) tarafından çağrılan tek görev odaklı dosyasıdır.  Daha fazla bilgi için ilke dosya tanımları makalesini okuyun.  Benzersiz her görev kendi RP gerektirir ve gereksinimleri markalama bağlı olarak sayısı "kullanım örnekleri toplam sayısı x uygulamalarının toplam" olabilir.

![İlke dosya türleri](media/active-directory-b2c-overview-custom/active-directory-b2c-overview-custom-policy-files.png)

| İlke dosya türü | Örnek dosya adı | Önerilen kullanın | Öğesinden devralınan |
|---------------------|--------------------|-----------------|---------------|
| TEMEL |TrustFrameworkBase.xml<br><br>Mytenant.onmicrosoft.com B2C 1A_BASE1.xml | Çekirdek talep şeması, talep dönüştürmeleri, talep sağlayıcıları ve Microsoft tarafından yapılandırılan kullanıcı Yolculuklar içerir<br><br>Bu dosyada küçük değişiklikler yapmak | None |
| Uzantı (EXT) | TrustFrameworkExtensions.xml<br><br>Mytenant.onmicrosoft.com-B2C-1A_EXT.xml | TEMEL dosya burada yaptığınız değişiklikler birleştirin<br><br>Değiştirilen talep sağlayıcıları<br><br>Değiştirilen kullanıcı Yolculuklar<br><br>Kendi özel şema tanımları | TEMEL dosya |
| Bağlı olan taraf (RP) | B2C_1A_sign_up_sign_in.xml| Belirteci şekli ve oturum burada ayarlarını değiştirme| Extensions(ext) dosyası |

### <a name="inheritance-model"></a>Devralma modeli

Bir uygulama RP ilke dosyasını aradığında, B2C kimlik deneyimi Framework'te tüm öğeleri temel sonra uzantılardan ekleyecek ve son geçerli İlkesi yürürlükte derlemek için RP İlkesi dosyasından.  Öğeleri aynı türde ve RP dosya adı UZANTILARI de kılar ve UZANTILARI temel geçersiz kılar.

**Yerleşik ilkeleri** Azure AD B2C'de yukarıda gösterilen 3 dosya düzeni izleyin, ancak Geliştirici Portalı arka planda EXTenstions dosyasına değişiklikler yaparken, yalnızca bağlı olan taraf (RP) dosya görür.  Tüm Azure AD B2C Azure B2C takım denetiminde ve sık sık güncelleştirilen bir temel ilke dosyası paylaşır.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Özel ilkelerini kullanmaya başlama](active-directory-b2c-get-started-custom.md)
