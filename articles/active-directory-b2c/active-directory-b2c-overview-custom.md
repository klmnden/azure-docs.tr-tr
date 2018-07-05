---
title: Azure Active Directory B2C özel ilkeleri | Microsoft Docs
description: Azure Active Directory B2C özel ilkeleri hakkında bilgi edinin.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/04/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 0724227da425eb2faeee9ac4ae8449782e62a241
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37445999"
---
# <a name="azure-active-directory-b2c-custom-policies"></a>Azure Active Directory B2C: Özel ilkeler

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

## <a name="what-are-custom-policies"></a>Özel ilkeler nedir?

Özel ilkeler, Azure AD B2C kiracınızın davranışlarını tanımlayan yapılandırma dosyalarıdır. Oysa **yerleşik ilkeleri** önceden tanımlanmıştır Azure AD B2C Portalı'nda yaygın kimlik görevler için özel ilkeler tam olarak bir neredeyse sınırsız sayıda görevleri tamamlamak için kimlik geliştiriciler tarafından düzenlenebilir. İçin okumaya devam özel ilkeler, kendiniz ve kimlik senaryonuz olup olmadığını belirlemek.

## <a name="comparing-built-in-policies-and-custom-policies"></a>Yerleşik ve özel ilkelerinin karşılaştırma

| | Yerleşik ilkeler | Özel ilkeler |
|-|-------------------|-----------------|
|Hedef kullanıcı | Tüm uygulama geliştiricilerin ile veya kimlik uzmanlığı gerektirmeden | Kimlik uzmanları: Sistem tümleştiricilerinin ortaklarımızı, danışmanlarımızı ve şirket içi kimlik ekipler. Openıdconnect akışlarıyla kendinizi rahat hissedene ve kimlik sağlayıcısı ve talep tabanlı kimlik doğrulaması'nı anlama |
|Yapılandırma yöntemi | Azure portalı ile kolay bir kullanıcı Arabirimi | XML dosyalarını doğrudan düzenleyerek ve ardından Azure portalına |
|Kullanıcı arabirimini özelleştirme | (Özel etki alanı gerektirir), HTML, CSS ve javascript desteği dahil olmak üzere, tam kullanıcı arabirimini özelleştirme<br><br>Özel dizeleri ile birden çok dil desteği | Aynı |
| Öznitelik özelleştirme | Standart ve özel öznitelikler | Aynı |
|Belirteç ve oturum yönetimi | Özel belirteç ve birden çok oturum seçenekleri | Aynı |
|Kimlik Sağlayıcıları| **Bugün**: önceden tanımlanmış yerel, sosyal sağlayıcısı<br><br>**Gelecekteki**: SAML, standartlara dayalı OIDC OAuth | **Bugün**: OAUTH, standartlara dayalı OIDC SAML<br><br>**Gelecekteki**: WsFed |
|Kimlik görevleri (örnekler) | Yerel ve birçok sosyal hesap ile oturum açma veya kaydolma<br><br>Self Servis Parola Sıfırlama<br><br>Profili Düzenle<br><br>Çok faktörlü kimlik doğrulama senaryoları<br><br>Belirteçleri ve oturum özelleştirme<br><br>Erişim belirteci akışlar | Özel kimlik sağlayıcılarını kullanarak yerleşik ilkeleri olarak aynı görevleri tamamlamak veya özel kapsamlarını kullanın<br><br>Kayıt zamanında başka bir sistem kullanıcı sağlama<br><br>Kendi e-posta hizmeti sağlayıcısı'nı kullanarak bir Hoş Geldiniz e-posta Gönder<br><br>B2C dışında bir kullanıcı deposu kullanma<br><br>Kullanıcı tarafından sağlanan güvenilir bir sistem API'si aracılığıyla bilgilerle doğrula |

## <a name="policy-files"></a>İlke dosyaları

Özel bir ilke birbirleriyle hiyerarşik bir zincirinde başvuran bir veya birden çok XML biçimli dosya olarak temsil edilir. XML öğeleri tanımlayın: şema talep, talep dönüşümleri, içerik tanımları, talep sağlayıcıları/teknik profilleri ve diğer öğeleri arasında kullanıcı yolculuğu düzenleme adımlarının.

İlke dosyaları üç tür kullanılmasını öneririz:

- **TEMEL dosya**, çoğu tanımları ve eksiksiz bir örnek Azure sağlayan içerir.  Sorun giderme ve uzun süreli bakım ilkelerinizin yardımcı olmak için bu dosyaya yaptığınız değişiklikleri en az sayıda öneririz
- **bir uzantı dosyası** kiracınız için benzersiz yapılandırma değişikliklerini içeren
- **bir bağlı olan taraf (RP) dosyası** doğrudan uygulamaya veya hizmete (diğer adıyla bağlı olan taraf) tarafından çağrılan diğer bir deyişle tek görev odaklı dosyası.  Daha fazla bilgi için ilke dosyası tanımları makalesini okuyun.  Benzersiz her görev kendi RP gerektirir ve markalama gereksinimlerine bağlı olarak sayı "uygulama kullanım örneklerinin toplam sayısı x toplam." olabilir


Yukarıda gösterilen üç dosya deseni Azure AD B2C'yi yerleşik ilkeleri izleyin, ancak Geliştirici Portalı arka planda uzantıları dosyasına değişiklikler yaparken, yalnızca bağlı olan taraf (RP) dosya görür.

## <a name="core-concepts-you-should-know-when-using-custom-policies"></a>Özel ilkeler kullanırken bilmeniz gereken temel kavramlar

### <a name="azure-active-directory-b2c"></a>Azure Active Directory B2C

Azure'nın Müşteri Kimliği ve erişim yönetimi (CIAM) hizmeti. Hizmet içerir:

1. Bir özel amaçlı Azure Active Directory ile Microsoft Graph ve kullanıcı verilerini yerel hesaplar hem de Federasyon hesapları için tutan erişilebilir biçiminde bir kullanıcı dizini 
2. Erişim **kimlik deneyimi çerçevesi** , kullanıcılar ve varlıklar arasındaki güven düzenleyen ve talep bir kimlik/erişim yönetim görevini tamamlamak için bunlar arasında geçirir. 
3. Kaynakları korumak için onları doğrulamak ve kimliği belirteçleri, yenileme belirteçleri, erişim belirteçleri (ve eşdeğer SAML onaylamalarını) veren bir güvenlik belirteci hizmeti (STS).

Azure AD B2C kimlik görevi başarmak için sırada kimlik sağlayıcıları, kullanıcılar, diğer sistemler ve yerel kullanıcı dizini etkileşim (örneğin bir kullanıcı oturum açma yeni bir kullanıcı kaydı, parola sıfırlama). Çok taraflı güven oluşturur ve bu adımları yürüten temel platform kimlik deneyimi çerçevesi ve bir ilke (bir kullanıcı yolculuğu ya da bir güven framework ilkesi olarak da bilinir) olarak adlandırılır açıkça tanımlar aktörleri, eylemleri, protokoller ve tamamlamak için adımlar dizisi.

### <a name="identity-experience-framework"></a>Kimlik Deneyimi Altyapısı

Openıdconnect, OAuth, SAML, WSFed ve birkaç standart olmayan yorumlar (örneğin REST gibi standart Protokolü (geniş kapsamda talep sağlayıcıları) varlıklar arasında güven düzenleyen bir tam olarak yapılandırılabilir, ilke odaklı, bulut tabanlı Azure platformu biçimleri API-based sistemi sistemi talep değişimleri). HTML, CSS ve javascript desteği kullanıcı dostu, beyaz etiketli deneyimleri I2E oluşturur.  Bugün, kimlik deneyimi çerçevesi için CIAM ilgili görevler için öncelikli ve yalnızca Azure AD B2C hizmetinde bağlamında kullanılabilir.

### <a name="built-in-policies"></a>Yerleşik ilkeler

Güvenilir kişilere ilişkilerini de önceden tanımlanmış (Azure AD B2C ' en yaygın olarak gerçekleştirmek için Azure AD B2C davranışını doğrudan dosyaları kullanılan kimlik görevler (diğer bir deyişle, kullanıcı kaydı, oturum açma, parola sıfırlama) ve etkileşim yapılandırmasıyla önceden tanımlanmış Örneğin Facebook kimlik sağlayıcısı, LinkedIn, Microsoft Account, Google hesapları).  Gelecekte, yerleşik ilkeleri özelleştirmesi genellikle Azure Active Directory Premium, Active Directory/ADFS, Salesforce kimlik sağlayıcısı vb. gibi Kurumsal bölgedeki kimlik sağlayıcıları için de sağlayabilir.


### <a name="custom-policies"></a>Özel ilkeler

Azure AD B2C kiracınızda kimlik deneyimi çerçevesi davranışını tanımlayan yapılandırma dosyaları. Özel bir ilke olarak (örneğin bir uygulama) bağlı olan taraf tarafından çağrıldığında kimlik deneyimi çerçevesi tarafından yürütülen (ilke dosyaları tanımları bakın) bir veya birden çok XML dosyaları erişilebilir. Özel ilkeler doğrudan bir neredeyse sınırsız sayıda görevleri tamamlamak için kimlik geliştiriciler tarafından düzenlenebilir. Özel ilkeler yapılandırma geliştiriciler güvenilir ilişkiler meta veri uç noktalarını eklemek için dikkatli ayrıntılı olarak tanımlamanız gerekir, tam talep tanımları exchange ve gizli dizileri, anahtarlar ve sertifikalar her bir kimlik sağlayıcısı tarafından gerektiği şekilde yapılandırın.

## <a name="policy-file-definitions-for-identity-experience-framework-trust-frameworks"></a>Kimlik deneyimi çerçevesi güven çerçeveler için ilke dosyası tanımları

### <a name="policy-files"></a>İlke dosyaları

Özel bir ilke birbirleriyle hiyerarşik bir zincirinde başvuran bir veya birden çok XML biçimli dosya olarak temsil edilir. XML öğeleri tanımlayın: şema talep, talep dönüşümleri, içerik tanımları, talep sağlayıcıları/teknik profilleri ve diğer öğeleri arasında kullanıcı yolculuğu düzenleme adımlarının.  İlke dosyaları üç tür kullanılmasını öneririz:

- **TEMEL dosya** çoğu tanımları ve eksiksiz bir örnek Azure sağlayan içerir.  Sorun giderme ve uzun süreli bakım ilkelerinizin yardımcı olmak için bu dosyaya yaptığınız değişiklikleri en az sayıda öneririz
- **bir uzantı dosyası** kiracınız için benzersiz yapılandırma değişikliklerini içeren
- **bir bağlı olan taraf (RP) dosyası** doğrudan uygulamaya veya hizmete (diğer adıyla bağlı olan taraf) tarafından çağrılan tek görev odaklı dosyasıdır.  Daha fazla bilgi için ilke dosyası tanımları makalesini okuyun.  Benzersiz her görev kendi RP gerektirir ve markalama gereksinimlerine bağlı olarak sayı "toplam uygulama kullanım örneklerinin toplam sayısı x" olabilir.

![İlke dosya türleri](media/active-directory-b2c-overview-custom/active-directory-b2c-overview-custom-policy-files.png)

| İlke dosyası türü | Örnek dosya adı | Önerilen kullanım | Devraldığı |
|---------------------|--------------------|-----------------|---------------|
| TEMEL |TrustFrameworkBase.xml<br><br>Mytenant.onmicrosoft.com B2C 1A_BASE1.xml | Çekirdek talep şeması, talep dönüştürmeleri, talep sağlayıcıları ve Microsoft tarafından yapılandırılan kullanıcı yolculuklarından içerir<br><br>Bu dosya için en az değişiklik | None |
| Uzantı (Dış) | TrustFrameworkExtensions.xml<br><br>Mytenant.onmicrosoft.com-B2C-1A_EXT.xml | TEMEL dosya burada yaptığınız değişiklikleri birleştirin<br><br>Değiştirilen talep sağlayıcıları<br><br>Değiştirilen kullanıcı yolculuklarından<br><br>Kendi özel şema tanımları | TEMEL dosya |
| Bağlı olan taraf (RP) | B2C_1A_sign_up_sign_in.xml| Belirteç şekli ve oturumu burada ayarları| Extensions(ext) dosyası |

### <a name="inheritance-model"></a>Devralma modeli

Bir uygulama RP ilke dosyası çağırdığında, B2C içinde kimlik deneyimi çerçevesi tüm öğeleri tabanından sonra UZANTILARI ekleyecek ve son olarak geçerli ilke etkin derlemek için RP ilke dosyasından.  Öğeleri aynı türde ve RP dosya adı UZANTILARI içinde geçersiz kılınır ve UZANTILARI temel geçersiz kılar.

**Yerleşik ilkeler** Azure AD B2C'de, yukarıda gösterilen 3-dosya deseni izleyin, ancak Geliştirici Portalı arka planda uzantı dosyasına değişiklikler yaparken, yalnızca bağlı olan taraf (RP) dosya görür.  Tüm Azure AD B2C'in Azure B2C takım denetimi altında olan ve sık sık güncelleştirilir temel ilke dosyası paylaşır.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Özel ilkeleri kullanmaya başlama](active-directory-b2c-get-started-custom.md)
