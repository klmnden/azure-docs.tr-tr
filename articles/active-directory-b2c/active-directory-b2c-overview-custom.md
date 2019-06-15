---
title: Azure Active Directory B2C özel ilkeleri | Microsoft Docs
description: Azure Active Directory B2C özel ilkeleri hakkında bilgi edinin.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/20/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 2938ae075bbd4c38b686ca6654bede678f876857
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66509809"
---
# <a name="custom-policies-in-azure-active-directory-b2c"></a>Azure Active Directory B2C özel ilkeleri

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Özel ilkeler, Azure Active Directory (Azure AD) B2C kiracınızın davranışlarını tanımlayan yapılandırma dosyalarıdır. Kullanıcı akışları, en yaygın kimlik görevleri için Azure AD B2C Portalı'nda önceden tanımlanmıştır. Özel ilkeler, tam olarak birçok farklı görevleri tamamlamak için kimlik geliştiriciler tarafından düzenlenebilir.

## <a name="comparing-user-flows-and-custom-policies"></a>Kullanıcı akışları ve özel ilkeler karşılaştırma

| | Kullanıcı akışları | Özel ilkeler |
|-|-------------------|-----------------|
| Hedef kullanıcı | Tüm uygulama geliştiricilerin ile veya kimlik uzmanlığı gerektirmeden. | Kimlik uzmanları, sistem tümleştiricilerinin, danışmanlarımızı ve şirket içi kimlik ekipler. Bunlar, Openıdconnect akışlarıyla kendinizi rahat hissedene ve kimlik sağlayıcısı ve talep tabanlı kimlik doğrulaması anlayın. |
| Yapılandırma yöntemi | Azure portalı ile bir kullanıcı dostu kullanıcı arabirimi (UI). | XML dosyalarını doğrudan düzenleyerek ve ardından Azure portalında yükleme. |
| Kullanıcı arabirimini özelleştirme | HTML, CSS ve JavaScript dahil olmak üzere tüm kullanıcı Arabirimi özelleştirme.<br><br>Özel dizeleri ile birden çok dil desteği. | Aynı |
| Öznitelik özelleştirme | Standart ve özel öznitelikleri. | Aynı |
| Belirteç ve oturum yönetimi | Özel belirteç ve birden çok oturumu seçeneği. | Aynı |
| Kimlik Sağlayıcıları | Önceden tanımlanmış bir yerel veya sosyal sağlayıcısı ve Azure Active Directory kiracıları ile Federasyon gibi birçok OIDC kimlik sağlayıcıları. | OIDC standartlara dayalı, OAUTH ve SAML.  Kimlik doğrulaması da REST API'leri ile tümleştirme kullanarak mümkündür. |
| Kimlik görevleri | Kaydolma veya oturum açma ile yerel veya birçok sosyal hesap.<br><br>Self Servis parola sıfırlama.<br><br>Profil düzenleme.<br><br>Çok faktörlü kimlik doğrulaması.<br><br>Belirteçleri ve oturum özelleştirin.<br><br>Erişim belirteci akışlar. | Özel kimlik sağlayıcılarını kullanarak kullanıcı akışları olarak aynı görevleri tamamlamak veya özel kapsamlarını kullanın.<br><br>Bir kullanıcı hesabı, kayıt zamanında başka bir sistemde sağlayın.<br><br>Kendi e-posta hizmeti sağlayıcısı'nı kullanarak bir Hoş Geldiniz e-posta gönderin.<br><br>Bir kullanıcı deposunun dışında Azure AD B2C'yi kullanın.<br><br>Bir API kullanarak güvenilir bir sistem bilgileriyle sağlanan kullanıcı doğrulayın. |

## <a name="policy-files"></a>İlke dosyaları

İlke dosyaları bu üç tür kullanılır:

- **Temel dosya** -tanımları çoğunu içerir. Bu dosya ile sorun giderme ve uzun süreli bakım ilkelerinizin yardımcı olmak için en az bir dizi değişiklik yapmak önerilir.
- **Dosya uzantıları** -kiracınız için benzersiz yapılandırma değişikliklerini tutar.
- **Bağlı olan taraf (RP) dosya** -uygulama veya hizmet tarafından çağrılan tek görev odaklı dosyası (bir bağlı olan taraf da bilinir). Benzersiz her görev kendi RP gerektirir ve gereksinimleri marka bağlı olarak, sayı "uygulama kullanım örneklerinin toplam sayısı x toplam." olabilir

Azure AD B2C kullanıcı akışlarında yukarıda gösterilen üç dosya deseni izleyin, ancak Azure portalında arka planda uzantıları dosyasına değişiklikler yaparken Geliştirici yalnızca RP dosya görür.

## <a name="custom-policy-core-concepts"></a>Özel ilke temel kavramlar

Azure'da müşteri kimlik ve erişim yönetimi (CIAM) hizmeti içerir:

- Bir kullanıcı, Microsoft Graph'ı kullanarak erişilebilir olduğundan ve yerel hesaplar hem de Federasyon hesapları için kullanıcı verilerini tutan dizin.
- Erişim **kimlik deneyimi çerçevesi** , kullanıcılar ve varlıklar arasındaki güven düzenler ve talep bir kimlik veya erişim yönetim görevini tamamlamak için bunlar arasında geçirir. 
- Kimliği belirteçleri, yenileme belirteçleri, erişim belirteçleri (ve eşdeğer SAML onaylamalarını) sorunları ve bunları kaynakları korumak için doğrulama güvenlik belirteci hizmeti (STS).

Azure AD B2C kimlik görevi başarmak için sırada kimlik sağlayıcıları, kullanıcılar, diğer sistemler ve yerel kullanıcı dizini ile etkileşime girer. Örneğin, bir kullanıcının oturumunu, yeni bir kullanıcı kaydı veya parola sıfırlama. Kimlik deneyimi çerçevesi ve (bir kullanıcı yolculuğu ya da bir güven framework ilkesi olarak da bilinir) bir ilke çok taraflı güven oluşturur ve aktör, eylemleri, protokoller ve bir dizi adımdan tamamlamak için açıkça tanımlar.

Kimlik deneyimi çerçevesi Openıdconnect, OAuth, SAML, WSFed ve birkaç standart olmayan yorumlar, örneğin REST gibi standart protokol biçimlerde varlıklar arasında güven düzenleyen bir tam olarak yapılandırılabilir, ilke odaklı, bulut tabanlı Azure platformudur API tabanlı--sisteme değişimleri talepleri. Framework, HTML ve CSS destekleyen kullanıcı dostu, beyaz etiketli deneyimler oluşturur.

Özel ilke bir veya hiyerarşi zincirinde birbirine başvuran daha fazla XML biçimindeki dosyadan oluşabilir. XML öğeleri, talep şema, talep dönüştürmeleri, içerik tanımlarını, talep sağlayıcıları, teknik profiller ve diğer öğeleri arasında kullanıcı yolculuğu düzenleme adımları tanımlayın. Özel bir ilke, bağlı olan taraf tarafından çağrıldığında kimlik deneyimi çerçevesi tarafından yürütülen bir veya birden çok XML dosyası olarak erişilebilir. Özel ilkeler yapılandırma geliştiriciler güvenilir ilişkiler meta veri uç noktalarını eklemek için dikkatli ayrıntılı olarak tanımlamanız gerekir, tam talep tanımları exchange ve gizli dizileri, anahtarlar ve sertifikalar her bir kimlik sağlayıcısı tarafından gerektiği şekilde yapılandırın.

### <a name="inheritance-model"></a>Devralma modeli

Bir uygulama RP ilke dosyası çağırdığında, Azure AD B2C'de kimlik deneyimi çerçevesi tüm öğeleri temel dosya, uzantıları dosyasından ve ardından geçerli ilke etkin derlemek için RP ilke dosyası ekler.  Öğeleri aynı türde ve RP dosya adı uzantıları ve uzantıları geçersiz kılma temel geçersiz kılar.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Özel ilkeleri kullanmaya başlama](active-directory-b2c-get-started-custom.md)
