---
title: Anahtarları, gizli ve sertifikaları hakkında
description: REST arabirimi ve KV Geliştirici Ayrıntıları'na genel bakış
services: key-vault
documentationcenter: ''
author: lleonard-msft
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: abd1b743-1d58-413f-afc1-d08ebf93828a
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2018
ms.author: alleonar
ms.openlocfilehash: dd1bb6117c0360e67783434c980c56b5f6ae7f9f
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37110255"
---
# <a name="about-keys-secrets-and-certificates"></a>Anahtarları, gizli ve sertifikaları hakkında
Azure anahtar kasası, depolamak ve Microsoft Azure ortamı içindeki şifreleme anahtarları kullanmak kullanıcıların sağlar. Anahtar kasası birden çok anahtar türleri ve algoritmaları destekler ve yüksek değerli anahtarlar için donanım güvenlik modülleri (HSM) kullanılmasına olanak verir. Ayrıca, anahtar kasası gizli güvenli bir şekilde kullanıcıları sağlar. Gizli sınırlı boyutu sekizli hiçbir belirli semantiği ile nesneleridir. Anahtar kasası, anahtarları ve gizli anahtarları üzerine inşa ve otomatik yenileme özelliği eklemek sertifikalarını da destekler.

Azure anahtar kasası hakkında daha fazla genel bilgi için bkz: [Azure anahtar kasası nedir?](/azure/key-vault/key-vault-whatis)

**Anahtar kasası genel ayrıntıları**

-   [Standartlarını destekleyen](about-keys-secrets-and-certificates.md#BKMK_Standards)
-   [Veri türleri](about-keys-secrets-and-certificates.md#BKMK_DataTypes)  
-   [Nesne tanımlayıcıları ve sürüm oluşturma](about-keys-secrets-and-certificates.md#BKMK_ObjId)  

**Anahtarlar hakkında**

-   [Anahtarları ve anahtar türleri](about-keys-secrets-and-certificates.md#BKMK_KeyTypes)  
-   [RSA algoritmaları](about-keys-secrets-and-certificates.md#BKMK_RSAAlgorithms)  
-   [RSA HSM algoritmaları](about-keys-secrets-and-certificates.md#BKMK_RSA-HSMAlgorithms)  
-   [Şifreleme koruma](about-keys-secrets-and-certificates.md#BKMK_Cryptographic)
-   [Anahtar işlemleri](about-keys-secrets-and-certificates.md#BKMK_KeyOperations)  
-   [Anahtar öznitelikleri](about-keys-secrets-and-certificates.md#BKMK_KeyAttributes)  
-   [Anahtar etiketleri](about-keys-secrets-and-certificates.md#BKMK_Keytags)  

**Gizli anahtarları hakkında** 

-   [Gizli anahtarlarla çalışma](about-keys-secrets-and-certificates.md#BKMK_WorkingWithSecrets)  
-   [Gizli öznitelikleri](about-keys-secrets-and-certificates.md#BKMK_SecretAttrs)  
-   [Gizli etiketleri](about-keys-secrets-and-certificates.md#BKMK_SecretTags)  
-   [Gizli erişim denetimi](about-keys-secrets-and-certificates.md#BKMK_SecretAccessControl)  

**Sertifikalar hakkında**

-   [Sertifika oluşturma](#BKMK_CompositionOfCertificate)  
-   [Sertifika öznitelikleri ve etiketleri](#BKMK_CertificateAttributesAndTags)  
-   [Sertifika İlkesi](#BKMK_CertificatePolicy)  
-   [Sertifikayı veren](#BKMK_CertificateIssuer)  
-   [Sertifika kişiler](#BKMK_CertificateContacts)  
-   [Sertifika erişim denetimi](#BKMK_CertificateAccessControl)  

--

## <a name="key-vault-general-details"></a>Anahtar kasası genel ayrıntıları

Aşağıdaki bölümlerde Azure anahtar kasası hizmetindeki uygulaması arasında ilgili genel bilgiler sunar.

###  <a name="BKMK_Standards"></a> Standartlarını destekleyen

JavaScript nesne gösterimi (JSON) ve JavaScript nesnesi imzalama ve şifreleme (JOSE) belirtimleri önemli arka plan bilgilerdir.  

-   [JSON Web anahtarı (JWK)](http://tools.ietf.org/html/draft-ietf-jose-json-web-key)  
-   [JSON Web şifreleme (JWE)](http://tools.ietf.org/html/draft-ietf-jose-json-web-encryption)  
-   [JSON Web algoritmalar (JWA)](http://tools.ietf.org/html/draft-ietf-jose-json-web-algorithms)  
-   [JSON Web imzası (JWS)](http://tools.ietf.org/html/draft-ietf-jose-json-web-signature)  

### <a name="BKMK_DataTypes"></a> Veri türleri

Başvurmak [JOSE belirtimleri](#BKMK_Standards) anahtarlarını, şifreleme ve imzalama için ilgili veri türleri için.  

-   **algoritma** -Örneğin, RSA1_5 anahtar bir işlem için desteklenen bir algoritma  
-   **ciphertext değeri** -Base64URL kullanılarak kodlanmış metin sekizli şifre  
-   **Özet-değer** -Base64URL kullanılarak kodlanan bir karma algoritması çıktısı  
-   **anahtar türü** -desteklenen anahtar türleri, örneğin RSA biri.  
-   **düz metin değeri** -Base64URL kullanılarak kodlanmış düz metin sekizli  
-   **İmza değeri** - Base64URL kullanılarak kodlanan bir imza algoritmasının çıkış  
-   **base64URL** -bir Base64URL [RFC4648] kodlanmış ikili değer  
-   **Boolean** -true veya false  
-   **Kimlik** - bir kimlik gelen Azure Active Directory (AAD).  
-   **IntDate** - 1970'ten saniye sayısını temsil eden bir JSON ondalık değer-01-01T0:0:0Z UTC belirtilen UTC tarih/saat kadar. [RFC3339] tarihi ile ilgili ayrıntılar için bkz: / Genel zaman ve özellikle UTC.  

###  <a name="BKMK_ObjId"></a> Nesneleri, tanımlayıcılarını ve sürüm oluşturma

Azure anahtar kasası içinde depolanan nesneleri nesneyi yeni bir örneğini oluşturulur ve benzersiz bir tanımlayıcı ve URL her sürümüne sahip olduğunda sürümleri korur. Bir nesnenin ilk oluşturulduğunda benzersiz sürüm tanıtıcısını verilir ve nesnesinin geçerli sürümü olarak işaretlenir. Aynı nesne adı ile yeni bir örneği oluşturulmasını benzersiz sürüm tanıtıcısını yeni nesnesi sağlar ve geçerli sürümü duruma neden olur.  

Azure anahtar kasası nesneleri geçerli tanımlayıcı veya bir sürüme özgü tanımlayıcıyı kullanarak çözülebilir. Örneğin, "MasterKey" adlı bir anahtarı verildiğinde, geçerli tanımlayıcıyla işlemlerini gerçekleştirme kullanılabilir en son sürümü kullanmak sistemin neden olur. Sürüme özgü tanımlayıcıyla işlemlerini gerçekleştirme sistemi nesnenin bu belirli sürümü kullanmaya neden olur.  

Nesneleri coğrafi konum, bağımsız olarak sistem hiç iki nesne aynı URL'ye sahip olacağı şekilde bir URL'yi kullanarak Azure anahtar kasası içinde benzersiz şekilde tanımlanır. Bir nesne için tam URL nesne tanımlayıcısı olarak adlandırılır ve anahtar kasası, nesne türünü tanımlayan bir önek bölümü oluşur, bir kullanıcı sağlanan nesne adı ve bir nesne sürümü. Nesne adı büyük küçük harf duyarsız ve değişmez. Nesne sürümü dahil etmeyin tanımlayıcıları temel tanımlayıcıları adlandırılır.  

Daha fazla bilgi için bkz: [kimlik doğrulaması, istekleri ve yanıtları](authentication-requests-and-responses.md)

Bir nesne tanımlayıcı genel biçimi aşağıdaki gibidir:  

`https://{keyvault-name}.vault.azure.net/{object-type}/{object-name}/{object-version}`  

Konumlar:  

|||  
|-|-|  
|`keyvault-name`|Microsoft Azure anahtar kasası hizmetindeki bir anahtar kasası adı.<br /><br /> Anahtar kasası adları, kullanıcı tarafından seçilen ve genel benzersiz.<br /><br /> Anahtar kasası adı uzunluğu yalnızca içeren içinde bir dize 3-24 karakter uzunluğunda olmalıdır (0-9, a-z, A-Z ve -).|  
|`object-type`|"Anahtarlar" veya "gizli" nesnenin türü.|  
|`object-name`|Bir `object-name` için bir kullanıcı tarafından sağlanan ad ve bir anahtar kasası içinde benzersiz olmalıdır. Adı bir dize 1-127 olmalıdır karakterden içeren yalnızca 0-9, a-z, A-Z ve -.|  
|`object-version`|Bir `object-version` bir sistem tarafından oluşturulan, isteğe bağlı olarak bir nesneyi benzersiz bir sürümünü gidermek için kullanılan 32 karakter dizesi tanımlayıcısı.|  

## <a name="key-vault-keys"></a>Anahtar kasası anahtarları

###  <a name="BKMK_KeyTypes"></a> Anahtarları ve anahtar türleri

Şifreleme anahtarları Azure anahtar Kasası'JSON Web anahtarı [JWK] nesneler olarak temsil edilir. Temel JWK/JWA belirtimleri ayrıca Azure anahtar kasası uygulama için benzersiz anahtar türlerini etkinleştirmek için genişletilir, örneğin Azure anahtar kasası güvenli nakliyatı etkinleştirmek için HSM satıcı (Thales) belirli paketleme kullanarak anahtarları alma gibi anahtarları o Azure anahtar kasası HSM'ler yalnızca kullanılabilir.  

İlk Azure anahtar kasası sürüm yalnızca RSA anahtarları destekler; gelecek sürümlerde simetrik ve Eliptik Eğri gibi diğer anahtar türlerini destekleyebilir.  

-   **RSA**: 2048 bit RSA anahtarı. Anahtar kasası tarafından yazılımda işlenir ancak bir HSM bir sistem anahtarı kullanarak REST şifrelenmiş olarak depolanır "yumuşak" anahtar budur. İstemcileri, mevcut bir RSA anahtarı almak veya Azure anahtar kasası bir oluşturma isteği.  
-   **RSA HSM**: bir HSM işlenen bir RSA anahtarı. HSM RSA anahtarları Azure anahtar kasası HSM güvenlik Dünyaları birinde korunur (yalıtım korumak için bir güvenlik Dünyası Coğrafya başına yoktur). İstemcileri, yumuşak formunda veya uyumlu bir HSM aygıttan vererek bir RSA anahtarı alma veya Azure anahtar kasası bir oluşturma isteği. Bu anahtar türü T öznitelik ekler HSM anahtar malzemesi taşımak için JWK edinin.  

     Coğrafi sınırlar hakkında daha fazla bilgi için bkz: [Microsoft Azure Trust Center](https://azure.microsoft.com/en-us/support/trust-center/privacy/)  

###  <a name="BKMK_RSAAlgorithms"></a> RSA algoritmaları  
 Aşağıdaki algoritması tanımlayıcıları RSA anahtarları Azure anahtar Kasası'ile desteklenir.  

#### <a name="wrapkeyunwrapkey-encryptdecrypt"></a>WRAPKEY/UNWRAPKEY, ŞİFRELEME/ŞİFRE ÇÖZME

-   **RSA1_5** -RSAES PKCS1 V1_5 [RFC3447] anahtar şifrelemesi  
-   **RSA OAEP** - en iyi asimetrik şifreleme (OAEP) [RFC3447] bölümü A.2.1 içinde RFC 3447 tarafından belirtilen varsayılan parametrelerle doldurma kullanarak RSAES. Bu varsayılan parametreler, SHA-1 ile SHA-1 karma işlevi ve MGF1 maskesi nesil işlevi kullanıyor.  

#### <a name="signverify"></a>OTURUM/DOĞRULAYIN

-   **RS256** - RSASSA-PKCS-v1_5 SHA-256 kullanan. Sağlanan uygulama Özet değer SHA-256 kullanılarak hesaplanan gerekir ve 32 bayt uzunluğunda olmalıdır.  
-   **RS384** - RSASSA-PKCS-v1_5 SHA-384 kullanma. Sağlanan uygulama Özet değer SHA-384 kullanılarak hesaplanan gerekir ve 48 bayt uzunluğunda olmalıdır.  
-   **RS512** - RSASSA-PKCS-v1_5 SHA-512 kullanma. Sağlanan uygulama Özet değer SHA-512 kullanılarak hesaplanan gerekir ve uzunluğu 64 baytın olması gerekir.  
-   **RSNULL** -bakın [RFC2437], bir özel kullanım belirli TLS senaryoları etkinleştirmek için örneği.  

###  <a name="BKMK_RSA-HSMAlgorithms"></a> RSA HSM algoritmaları  
Aşağıdaki algoritması tanımlayıcıları HSM RSA anahtarları Azure anahtar Kasası'ile desteklenir.  

### <a name="BKMK_Cryptographic"></a> Şifreleme koruma

Azure anahtar kasası kullanan, şifreleme modüllerini HSM veya yazılımı olup FIPS doğrulandı. FIPS modunda çalıştırmak için özel bir şey yapmanız gerekmez. Varsa, **oluşturma** veya **alma** HSM korumalı anahtarlar, bunlar garanti edilir FIPS 140-2 Düzey 2 veya daha yüksek doğrulanmış HSM'ler içinde işlenecek. Varsa, **oluşturma** veya **alma** yazılım korumalı şifreleme modüllerini içinde işlenir sonra anahtarlara doğrulanmış FIPS 140-2 Düzey 1 veya daha yüksek. Daha fazla bilgi için bkz: [anahtarları ve anahtar türleri](about-keys-secrets-and-certificates.md#BKMK_KeyTypes).

#### <a name="wrapunwrap-encryptdecrypt"></a>KAYDIRMA VE KAYDIRMA, ŞİFRELEME/ŞİFRE ÇÖZME

-   **RSA1_5** -RSAES PKCS1 V1_5 [RFC3447] anahtar şifrelemesi.  
-   **RSA OAEP** - en iyi asimetrik şifreleme (OAEP) [RFC3447] bölümü A.2.1 içinde RFC 3447 tarafından belirtilen varsayılan parametrelerle doldurma kullanarak RSAES. Bu varsayılan parametreler, SHA-1 ile SHA-1 karma işlevi ve MGF1 maskesi nesil işlevi kullanıyor.  

 #### <a name="signverify"></a>OTURUM/DOĞRULAYIN  

-   **RS256** - RSASSA-PKCS-v1_5 SHA-256 kullanan. Sağlanan uygulama Özet değer SHA-256 kullanılarak hesaplanan gerekir ve 32 bayt uzunluğunda olmalıdır.  
-   **RS384** - RSASSA-PKCS-v1_5 SHA-384 kullanma. Sağlanan uygulama Özet değer SHA-384 kullanılarak hesaplanan gerekir ve 48 bayt uzunluğunda olmalıdır.  
-   **RS512** - RSASSA-PKCS-v1_5 SHA-512 kullanma. Sağlanan uygulama Özet değer SHA-512 kullanılarak hesaplanan gerekir ve uzunluğu 64 baytın olması gerekir.  
-   RSNULL: Bakın [RFC2437], bir özel kullanım belirli TLS senaryoları etkinleştirmek için örneği.  

###  <a name="BKMK_KeyOperations"></a> Anahtar işlemleri

Azure anahtar kasası anahtar nesneler üzerinde aşağıdaki işlemleri destekler:  

-   **Oluşturma**: Azure anahtar Kasası ' bir anahtar oluşturmak bir istemcinin sağlar. Anahtarın değerini Azure anahtar kasası tarafından oluşturulan ve saklanan ve istemciye serbest bırakılmaz. Asimetrik (ve gelecekte Eliptik Eğri ve simetrik) anahtarları Azure anahtar kasası oluşturulabilir.  
-   **Alma**: Azure anahtar Kasası'na mevcut bir anahtarı içeri aktarmak bir istemcinin sağlar. Asimetrik (ve gelecekte Eliptik Eğri ve simetrik) anahtarları Azure anahtar kasası çok sayıda JWK yapı içinde farklı paketleme yöntemlerini kullanarak için aktarılabilir.  
-   **Güncelleştirme**: daha önce Azure anahtar kasası içinde depolanan bir anahtarla ilişkili meta verileri (anahtar öznitelikleri) değiştirmek için yeterli izinlere sahip bir istemcinin sağlar.  
-   **Silme**: Azure anahtar Kasası'nı bir anahtarı silmek için yeterli izinlere sahip bir istemcinin sağlar.  
-   **Liste**: belirli bir Azure anahtar Kasası'nda tüm anahtarları listelemek bir istemcinin sağlar.  
-   **Liste sürümleri**: belirli bir Azure anahtar Kasası'nda belirli bir anahtarı'nin tüm sürümleri listelemek bir istemcinin sağlar.  
-   **Alma**: Azure anahtar Kasası'nda belirli bir anahtarı ortak bölümlerini almak bir istemcinin sağlar.  
-   **Yedekleme**: korumalı bir form anahtarında dışa aktarır.  
-   **Geri yükleme**: daha önce yedeklenen anahtarı alır.  

Daha fazla bilgi için bkz: [anahtar işlemleri](/rest/api/keyvault/key-operations).  

Bir anahtar Azure anahtar kasası oluşturulduktan sonra aşağıdaki şifreleme işlemleri anahtarı kullanılarak gerçekleştirilebilir:  

-   **Oturum ve Doğrula**: Azure anahtar kasası imza oluşturmanın bir parçası içeriği karma desteklemediğinden, kesinlikle, bu işlem "oturum karma" veya "karma doğrula" olur. Uygulamalar verileri yerel olarak oturum açmanız ve bu Azure anahtar kasası oturum karma istek karma. İmzalı karmaları doğrulanması [public] anahtar malzemesi erişiminiz olmayabilir uygulamalar için bir kolaylık işlemi olarak desteklenir; Biz, en iyi uygulama performansını öneririz, yerel olarak gerçekleştirilen işlemler doğrulayın.  
-   **Anahtar şifrelemesi / kaydırma**: Azure anahtar kasasında depolanan bir anahtarı başka bir anahtarı, bir simetrik içerik şifreleme anahtarı (CEK) genellikle korumak üzere kullanılabilir. Azure anahtar kasası anahtar asimetrik olduğunda anahtar şifrelemesi kullanılır, örneğin RSA OAEP ve WRAPKEY/UNWRAPKEY işlemleri için şifreleme/şifre çözme eşdeğerdir. Azure anahtar kasası anahtar simetrik olduğunda kaydırma anahtarı kullanılır; Örneğin AES-KW. [Public] anahtar malzemesi erişiminiz olmayabilir uygulamalar için kolaylık WRAPKEY işlemi desteklenmiyor; en iyi uygulama performansını WRAPKEY işlemleri yerel olarak gerçekleştirilir, önerilir.  
-   **Şifrelemek ve şifresini**: Azure anahtar kasasında depolanan bir anahtarı şifrelemek veya tek bir blok boyutu, seçilen şifreleme algoritması ve anahtar türü tarafından belirlenir verilerin şifresini çözmek için kullanılabilir. Şifreleme işlemi [public] anahtar malzemesi erişiminiz olmayabilir uygulamalar için kolaylık sağlamak için sağlanır; en iyi uygulama performansı için önerilir, şifreleme işlemlerini yerel olarak yapılmalıdır.  

Şifreleme/şifre çözme işlemi eşdeğerdir gibi asimetrik anahtarlar kullanılarak WRAPKEY/UNWRAPKEY gereksiz görünebilir, ancak ayrı işlemleri kullanımını sağlamak önemli anlamsal olarak kabul edilir ve bu işlemler ve tutarlılık yetkilendirme ayrımı ne zaman diğer anahtar türleri hizmet tarafından desteklenir.  

Azure anahtar kasası verme işlemlerini desteklemiyor: bir anahtar sağlandıktan sonra sistemde çıkarılamıyor veya anahtar malzemesini değiştirilemiyor.  Ancak, kendi anahtarını diğer için ihtiyacı Azure anahtar kasası kullanıcılarının kullanım veya Azure anahtar Kasası ' silindikten sonra yedekleme ve geri yükleme işlemlerini korumalı bir form anahtarında dışa aktarma/içe aktarma için kullanabilir. Yedekleme işlemi tarafından oluşturulan anahtarları Azure anahtar kasası dışında kullanılamaz. Alternatif olarak, içeri aktarma işlemi birden çok Azure anahtar kasası örnekleri karşı kullanılabilir.  

Kullanıcılar herhangi birini Azure anahtar kasası JWK nesnesinin key_ops özelliğini kullanarak bir anahtar başına temelinde destekleyen şifreleme işlemlerini kısıtlayabilirsiniz.  

JWK nesneleri hakkında daha fazla bilgi için bkz: [JSON Web anahtarı (JWK)](http://tools.ietf.org/html/draft-ietf-jose-json-web-key).  

###  <a name="BKMK_KeyAttributes"></a> Anahtar öznitelikleri

Aşağıdaki öznitelikler anahtar malzemesini ek olarak belirtilebilir. JSON istek, öznitelik anahtar sözcüğü ve küme ayraçları ' {' '}', belirtilen özniteliklere olsa bile gereklidir.  

- *Etkin*: boolean, isteğe bağlı, varsayılan **doğru**. Anahtar etkin ve şifreleme işlemleri için kullanışlı olup olmadığını belirtir. *Etkin* özniteliği ile birlikte kullanılır *nbf* ve *exp*. Bir işlem oluştuğunda arasında *nbf* ve *exp*, varsa yalnızca verilecek *etkin* ayarlanır **doğru**. İşlemleri dışında *nbf* / *exp* penceresi otomatik olarak izin verilmez, belirli işlemi türleri altında dışında [belirli koşullar](about-keys-secrets-and-certificates.md#BKMK_key-date-time-ctrld-ops).
- *NBF*: IntDate, isteğe bağlı, varsayılan değer şimdi. *Nbf* (önce değil) öznitelik tanımlar öncesinde anahtar olmayan kullanılmalıdır belirli işlemi türleri altında dışında şifreleme işlemleri için zaman [belirli koşullar](about-keys-secrets-and-certificates.md#BKMK_key-date-time-ctrld-ops). İşlenmesini *nbf* özniteliği gerektirir geçerli tarih/saat sonra olması gerekir veya eşit değil-tarih/saat içinde listelenen önce *nbf* özniteliği. Azure anahtar kasası için saat eğriltme hesap için genellikle en fazla birkaç dakika bazı küçük leeway için sağlayabilir. Değerini içeren bir IntDate değeri bir sayı olmalıdır.  
- *exp*: IntDate, isteğe bağlı, varsayılan değer "sonsuza kadar". *Exp* (sona erme saati) öznitelik tanımlar ve hangi gerekir belirli işlemi türleri altında dışında şifreleme işlemi için kullanılır ve sonrasında süre sonu zamanı [belirli koşullar](about-keys-secrets-and-certificates.md#BKMK_key-date-time-ctrld-ops). İşlenmesini *exp* özniteliği gerektirir süre sonu tarihi/saati listelenen önce geçerli tarih/saat olmalıdır *exp* özniteliği. Azure anahtar kasası için saat eğriltme hesap için genellikle en fazla birkaç dakika bazı küçük leeway için sağlayabilir. Değerini içeren bir IntDate değeri bir sayı olmalıdır.  

Anahtar öznitelikleri içeren bir yanıta dahil ek salt okunur öznitelikleri şunlardır:  

- *oluşturulan*: IntDate, isteğe bağlı. *Oluşturulan* öznitelik, anahtar bu sürümü ne zaman oluşturulduğunu gösterir. Bu değer, bu öznitelik toplama önce oluşturulan anahtarları için null. Değerini içeren bir IntDate değeri bir sayı olmalıdır.  
- *güncelleştirilmiş*: IntDate, isteğe bağlı. *Güncelleştirilmiş* öznitelik, anahtar bu sürümü güncelleştirildiği gösterir. Bu değer, bu öznitelik toplama önce en son güncelleştirildiği anahtarları için null. Değerini içeren bir IntDate değeri bir sayı olmalıdır.  

IntDate ve diğer veri türleri hakkında daha fazla bilgi için bkz: [veri türleri](about-keys-secrets-and-certificates.md#BKMK_DataTypes)  

#### <a name="BKMK_key-date-time-ctrld-ops"></a> Tarih-saat denetimli işlemleri

Not henüz geçerli ve süresi dolan anahtarları, bu dış *nbf* / *exp* penceresinde için çalışır **şifresini**, **unwrap** ve **doğrulayın** işlemleri (403, dönüş olmaz Yasak). Not henüz geçerli durumu kullanma stratejinin üretim kullanılmadan önce bağlanmanın bir anahtar izin vermektir. Süresi dolmuş durumu kullanma stratejinin anahtarının geçerli olduğu zaman, oluşturulan veri kurtarma işlemleri izin vermektir. Ayrıca, anahtar kasası ilkelerini kullanarak bir anahtar veya güncelleştirerek erişimi devre dışı bırakabilirsiniz *etkin* anahtar özniteliği **false**.

Veriler hakkında daha fazla bilgi için bkz: türleri, [veri türleri](about-keys-secrets-and-certificates.md#BKMK_DataTypes).

Diğer olası öznitelikleri hakkında daha fazla bilgi için bkz: [JSON Web anahtarı (JWK)](http://tools.ietf.org/html/draft-ietf-jose-json-web-key).

### <a name="BKMK_Keytags"></a> Anahtar etiketleri

Ek uygulamaya özgü meta veriler etiketleri formunda belirtebilirsiniz. Azure anahtar kasası her biri olan 256 karakterlik bir ad ve 256 karakter değeri en fazla 15 etiketlerini destekler.  

>[!Note]
>Bunlar varsa etiketleri çağıran tarafından okunabilir *listesi* veya *almak* bu nesne türü; izni anahtarları, gizli veya sertifikalar.

###  <a name="BKMK_KeyAccessControl"></a> Anahtar erişim denetimi

Anahtar kasası tarafından yönetilen anahtarlar için erişim denetimi, anahtarları kapsayıcı olarak davranan bir anahtar kasası düzeyinde sağlanır. Anahtarları için aynı anahtar kasasına gizli anahtarları için erişim denetimi İlkesi farklıdır bir erişim denetimi İlkesi yok. Kullanıcılar anahtarları tutmak için bir veya daha fazla kasalarını oluşturabilir ve senaryo uygun kesimleme ve anahtar yönetimi sağlamak için gereklidir. Anahtarlar için erişim denetimi, gizli anahtarları için erişim denetimi bağımsızdır.  

Aşağıdaki izinleri, üzerinde verilebilir bir kullanıcı başına / service bir kasadaki anahtarları erişim denetimi girişi asıl temel. Bu izinleri yakından anahtar bir nesne üzerinde izin işlemleri yansıtma:  

-   *oluşturma*: yeni anahtarlar oluşturma
-   *Alma*: bir anahtar ve özniteliklerini genel bölümü okuyun
-   *Liste*: anahtarları veya anahtar kasasında depolanan bir anahtarın sürümlerini listeler
-   *Güncelleştirme*: bir anahtar özniteliklerini güncelleştir
-   *silme*: anahtar nesne silme
-   *oturum*: imzalamak için anahtar özetleyen kullanın
-   *doğrulama*: anahtar özetler doğrulamak için kullanın
-   *wrapKey*: simetrik anahtar korumak için tuşunu kullanın
-   *unwrapKey*: korumasını kaldırmak için anahtar kullanım Sarmalanan simetrik anahtarlar
-   *şifreleme*: rasgele bir bayt dizisi korumak için tuşunu kullanın
-   *şifresini*: bayt dizisi korumasını kaldırmak için tuşunu kullanın
-   *Alma*: bir anahtar Kasası'na bir anahtarı alma
-   *Yedekleme*: anahtar kasasında bir anahtarı yedekleme
-   *geri yükleme*: bir anahtar kasası için anahtarı yedeklenen bir geri yükleme
-   *tüm*: tüm izinleri

## <a name="key-vault-secrets"></a>Anahtar kasası gizli 

###  <a name="BKMK_WorkingWithSecrets"></a> Gizli anahtarlarla çalışma

Gizli anahtarları Azure anahtar Kasası'nda 25 k bayt her boyut sınırı olan sekizli sıraları ' dir. Azure anahtar kasası hizmetindeki tüm semantiği için gizli sağlamaz; Bunu yalnızca verileri kabul eder, şifreler ve, gizli bir tanımlayıcı, "gizli daha sonraki bir zamanda almak için kullanılabilir kimlik" döndürme depolar.  

Yüksek oranda gizli verileri için istemcileri Azure anahtar kasasında depolanan verilerin korumasını ek Katmanlar göz önünde bulundurmanız gerekir; Örneğin verileri önceden şifreleyerek ayrı koruma anahtarı kullanma.  

Azure anahtar kasası bir contentType alanı için gizlilikler de destekler. İstemciler, içerik türü, "contentType" gizli verileri yorumlama alındığı sırada yardımcı olması için bir gizli anahtarı belirtebilir. Bu alan en fazla uzunluğu 255 karakterdir. Önceden tanımlanmış değerler yoktur. Gizli verileri yorumlama için ipucu olarak önerilen kullanımdır bakın. Örneğin, bir uygulama gizlilikleri olarak parolaları ve sertifikaları depolamak sonra belirtmek için bu alanı kullanın. Önceden tanımlanmış değerler yoktur.  

###  <a name="BKMK_SecretAttrs"></a> Gizli öznitelikleri

Gizli verilere ek olarak aşağıdaki öznitelikler belirtilebilir:  

- *exp*: IntDate, isteğe bağlı, varsayılan **sonsuza kadar**. *Exp* (sona erme saati) öznitelik tanımlar ve hangi gerekir gizli verilerin alınması, sonrasında süre sonu zamanı hariç [belirli durumlarda](about-keys-secrets-and-certificates.md#BKMK_secret-date-time-ctrld-ops). İşlenmesini *exp* özniteliği gerektirir süre sonu tarihi/saati listelenen önce geçerli tarih/saat olmalıdır *exp* özniteliği. Azure anahtar kasası için saat eğriltme hesap için genellikle en fazla birkaç dakika bazı küçük leeway için sağlayabilir. Değerini içeren bir IntDate değeri bir sayı olmalıdır.  
- *NBF*: IntDate, isteğe bağlı, varsayılan **şimdi**. *Nbf* (önce değil) öznitelik tanımlar öncesinde gizli verilerin gerekir değil alınamıyor, zaman hariç [belirli durumlarda](about-keys-secrets-and-certificates.md#BKMK_secret-date-time-ctrld-ops). İşlenmesini *nbf* özniteliği gerektirir geçerli tarih/saat sonra olması gerekir veya eşit değil-tarih/saat içinde listelenen önce *nbf* özniteliği. Azure anahtar kasası için saat eğriltme hesap için genellikle en fazla birkaç dakika bazı küçük leeway için sağlayabilir. Değerini içeren bir IntDate değeri bir sayı olmalıdır.  
- *Etkin*: boolean, isteğe bağlı, varsayılan **doğru**. Bu öznitelik gizli verileri alınıp olup olmadığını belirtir. Etkin özniteliği ile birlikte kullanılır ve *exp* bir işlem oluştuğunda arasında ve exp, yalnızca etkinleştirilirse ayarlanır **doğru**. İşlemleri dışında *nbf* ve *exp* penceresi olan otomatik olarak izin verilmeyen, içinde dışında [belirli durumlarda](about-keys-secrets-and-certificates.md#BKMK_secret-date-time-ctrld-ops).  

Gizli öznitelikleri içeren bir yanıta dahil ek salt okunur öznitelikleri şunlardır:  

- *oluşturulan*: IntDate, isteğe bağlı. Oluşturulan öznitelik gizli bu sürüm ne zaman oluşturulduğunu gösterir. Bu değer, bu öznitelik toplama önce oluşturulan gizli anahtarları için null. Değerini içeren bir IntDate değeri bir sayı olmalıdır.  
- *güncelleştirilmiş*: IntDate, isteğe bağlı. Bu sürüm gizli güncelleştirildiği güncelleştirilmiş özniteliği belirtir. Bu değer, bu öznitelik toplama önce en son güncelleştirildiği gizli anahtarları için null. Değerini içeren bir IntDate değeri bir sayı olmalıdır.

#### <a name="BKMK_secret-date-time-ctrld-ops"></a> Tarih-saat denetimli işlemleri

Bir parolanın **almak** işlemi dışında değil henüz geçerli ve süresi dolan gizli çalışır *nbf* / *exp* penceresi. Bir parolanın çağırma **almak** işleminde değil henüz geçerli bir gizli test amaçları için kullanılabilir. Alma (**almak**lık) süresi dolmuş bir parola kurtarma işlemleri için kullanılabilir.

Veriler hakkında daha fazla bilgi için bkz: türleri, [veri türleri](about-keys-secrets-and-certificates.md#BKMK_DataTypes).  

###  <a name="BKMK_SecretAccessControl"></a> Gizli erişim denetimi

Azure anahtar kasası yönetilen gizli anahtarları için erişim denetimi, bu gizli kapsayıcı olarak davranan bir anahtar kasası düzeyinde sağlanır. Gizli anahtarları için aynı anahtar kasasındaki anahtarlar için erişim denetimi İlkesi farklıdır bir erişim denetimi İlkesi yok. Kullanıcılar, gizli tutmak için bir veya daha fazla kasalarını oluşturabilir ve senaryo uygun kesimleme ve gizli anahtarları yönetimini sağlamak için gereklidir. Erişim denetimlerini gizli anahtarları için erişim denetimi bağımsızdır.  

Aşağıdaki izinleri, asıl başına temelinde, bir kasadaki gizli erişim denetim girdisi olarak kullanılabilir ve yakından gizli bir nesne üzerinde izin işlemleri yansıtma:  

-   *ayarlama*: yeni gizli anahtarları oluştur  
-   *Alma*: bir gizlilik okuma  
-   *Liste*: gizli veya anahtar kasasında depolanan bir gizli anahtarın sürümlerini listeler  
-   *silme*: gizli anahtarı silme  
-   *tüm*: tüm izinleri  

Gizli anahtarlarla çalışma hakkında daha fazla bilgi için bkz: [gizli anahtar işlemleri](/rest/api/keyvault/secret-operations).  

###  <a name="BKMK_SecretTags"></a> Gizli etiketleri  
Ek uygulamaya özgü meta veriler etiketleri formunda belirtebilirsiniz. Azure anahtar kasası her biri olan 256 karakterlik bir ad ve 256 karakter değeri en fazla 15 etiketlerini destekler.  

>[!Note]
>Bunlar varsa etiketleri çağıran tarafından okunabilir *listesi* veya *almak* bu nesne türü; izni anahtarları, gizli veya sertifikalar.

## <a name="key-vault-certificates"></a>Anahtar kasası sertifikaları

Anahtar kasası sertifikaları destek sağlar, x509 yönetimi için sertifikaları ve aşağıdaki davranışları:  

-   Bir anahtar kasası oluşturma işlemi veya varolan bir sertifikayı alma aracılığıyla bir sertifika oluşturmak bir sertifika sahibinin verir. Bu otomatik olarak imzalanan hem de içerir ve sertifika yetkilisi sertifikaları oluşturulur.
-   Güvenli Depolama ve X509 Yönetimi uygulamak bir anahtar kasası sertifika sahibinin verir sertifikaları özel anahtar malzemesi etkileşim olmadan.  
-   Bir sertifika yaşam döngüsünü yönetmek için anahtar kasası yönlendiren bir ilke oluşturmak bir sertifika sahibinin verir.  
-   Kişi hakkında bilgi için bildirim yaşam döngüsü olayları geçerlilik süresi ve sertifikanın yenilenmesini sağlamak sertifika sahipleri sağlar.  
-   Otomatik olarak yenilenmesi destekler seçili verenler - anahtar kasası iş ortağı X509 ile sertifika sağlayıcıları / sertifika yetkilileri.

>[!Note]
>Olmayan ortaklık sağlayıcıları/yetkilileri de izin verilir ancak otomatik yenileme özelliği desteklemez.

###  <a name="BKMK_CompositionOfCertificate"></a> Sertifika oluşturma

Ayrıca adreslenebilir anahtarı ve gizli bir anahtar kasası sertifika oluşturulduğunda, aynı ada sahip oluşturulur. Anahtar kasası anahtar anahtar işlemleri ve anahtar kasası gizli sertifika değeri alma gizli olarak olanak sağlar. Bir anahtar kasası sertifikası ortak x509 de içeren meta veri sertifika.  

Sertifikaları sürümü ve tanımlayıcı, anahtarları ve gizli anahtarları benzer. Anahtar kasası sertifika yanıtta bir adreslenebilir anahtarı ve gizli anahtar kasası sertifika sürümüyle oluşturulmuş belirli bir sürümü kullanılabilir.
 
![Cetificates karmaşık nesneleridir](media/azure-key-vault.png)

###  <a name="BKMK_CertificateExportableOrNonExportableKey"></a> Dışarı aktarılabilir veya olmayan verilebilir anahtarı

Bir anahtar kasası sertifika oluşturulduğunda, sertifikayı oluşturmak için kullanılan ilkeyi anahtarı verilebilir belirtilmişse, adreslenebilir gizli anahtarından PFX veya PEM biçiminde özel anahtarla alınabilir. Anahtar kasası sertifika oluşturmak için kullanılan ilkeyi anahtarının aktarılamaz belirtilmişse, ardından özel anahtarı bir gizlilik olarak alınırken değerinin bir parçası değil.  

Adreslenebilir anahtar aktarılamaz KV sertifikalarla daha ilgili hale gelir. Öğesinden adreslenebilir KV anahtarın işlemleri eşlenen *keyusage* KV sertifikayı oluşturmak için kullanılan KV sertifika ilkesi alanı.  

İki tür anahtar desteklenir – *RSA* veya *RSA HSM* sertifikalarla. Dışarı aktarılabilir yalnızca RSA, RSA HSM tarafından desteklenmeyen ile izin verilir.  

###  <a name="BKMK_CertificateAttributesAndTags"></a> Sertifika öznitelikleri ve etiketleri

Sertifika meta verileri ek olarak, adreslenebilir bir anahtarı ve bir adreslenebilir gizli bir anahtar kasası sertifika öznitelikler ve etiketleri olarak var.  

#### <a name="attributes"></a>Öznitelikler

Sertifika öznitelikleri adreslenebilir anahtarı ve gizli KV sertifika oluşturulduğunda öznitelikleri yansıtılır.  

Bir anahtar kasası sertifika aşağıdaki özniteliklere sahiptir:  

-   *Etkin*: boolean, isteğe bağlı, varsayılan **doğru**. Bu öznitelik, sertifika verileri gizli ya da bir anahtar olarak çalıştırılabilir olarak alınabilir olmadığını belirtmek için belirtilebilir. Bu ile birlikte kullanılır *nbf* ve *exp* bir işlem oluştuğunda arasında *nbf* ve *exp*, etkinleştirildiğinde yalnızca verilecek ayarlanmış true. İşlemleri dışında *nbf* ve *exp* penceresi otomatik olarak izin verilmez.  

Yanıta dahil ek salt okunur öznitelikleri şunlardır:

-   *oluşturulan*: IntDate: Bu sürümü sertifikanın oluşturulduğu gösterir.  
-   *güncelleştirilmiş*: IntDate: sertifikanın bu sürümü güncelleştirildiği gösterir.  
-   *exp*: IntDate: x509 bitiş tarihlerini değerini içeren sertifika.  
-   *NBF*: IntDate: x509 tarih değerini içeren sertifika.  

> [!Note] 
> Bir anahtar kasası sertifikanın süresi dolarsa adreslenebilir anahtarıdır ve gizli çalışamaz duruma gelebilir.  

#### <a name="tags"></a>Etiketler

 İstemci belirtilen anahtar değer çifti, anahtarları ve gizli anahtarları etiketlerinde benzer sözlüğü.  

 > [!Note]
> Bunlar varsa etiketleri çağıran tarafından okunabilir *listesi* veya *almak* bu nesne türü; izni anahtarları, gizli veya sertifikalar.

###  <a name="BKMK_CertificatePolicy"></a> Sertifika İlkesi

Bir sertifika ilkesi oluşturma ve KV sertifika yaşam döngüsü yönetme konusunda bilgi içerir. Sertifikayı özel anahtarla anahtar kasasının aktarıldığında, varsayılan bir ilke x509 okuyarak oluşturulan sertifika.  

KV sertifika sıfırdan oluşturulduğunda, bir ilke bu KV sertifika sürümü veya sonraki KV sertifika sürüm oluşturma hakkında bilgilendirmek için anahtar kasasına sağlanmalıdır. Bir ilke kurulduktan sonra art arda ile gerekli değildir sonraki KV sertifika sürümleri oluşturmak için işlem oluşturun.  

Bir ilke KV sertifika tüm sürümleri için yalnızca bir örneği vardır.  

Yüksek bir düzeyde bir sertifika ilkesi aşağıdakileri içerir:  

-   X509 sertifika özellikleri: içeren konu adı, konu alternatif adlarını vb. bir x509 oluşturmak için kullanılan sertifika isteği.  
-   Anahtar özellikler: anahtar türü içeren anahtar uzunluğu, dışarı aktarılabilir ve anahtar alanları yeniden kullanabilirsiniz. Bu alanların bir anahtar oluşturmak nasıl anahtar kasası isteyin.  
-   Gizli özellikler: gizli olarak sertifika almak için gizli değer üretmek için adreslenebilir gizli içerik türü gibi gizli özellikler içerir.  
-   Ömür eylemler: KV sertifika ömrü eylemleri içerir. Her ömrü eylemi içerir:  

     - Tetikleyici: süre sonu veya yaşam süresi aralığı yüzdesi önceki gün aracılığıyla belirtilen  

     - Eylem: eylem türü – belirtme *emailContacts* veya *autoRenew*  

-   Sertifikayı veren: Parametreleri x509 sertifikaları için kullanılacak sertifikayı veren hakkında.  
-   İlke öznitelikleri: ilkeyle ilişkili öznitelikleri içerir  

#### <a name="x509-to-key-vault-usage-mapping"></a>Anahtar kasası kullanım eşlemesi X509

Aşağıdaki tabloda, anahtar kullanım ilkesi anahtar etkili anahtar işlemleri için bir anahtar kasası sertifika oluşturmanın bir parçası oluşturulan x509 eşlemeyi temsil eder.

|**X509 anahtar kullanımı bayrakları**|**Anahtar kasası anahtar ops**|**Varsayılan davranış**|
|----------|--------|--------|
|DataEncipherment|Şifreleme, şifre çözme| Yok |
|DecipherOnly|şifre çözme| Yok  |
|DigitalSignature|oturum, doğrulayın| Anahtar kasası varsayılan sertifika oluşturma zamanında kullanım belirtimi olmadan | 
|EncipherOnly|şifrele| Yok |
|KeyCertSign|oturum, doğrulayın|Yok|
|KeyEncipherment|wrapKey, unwrapKey| Anahtar kasası varsayılan sertifika oluşturma zamanında kullanım belirtimi olmadan | 
|İnkar edilemez|oturum, doğrulayın| Yok |
|crlsign|oturum, doğrulayın| Yok |

###  <a name="BKMK_CertificateIssuer"></a> Sertifikayı veren

Bir anahtar kasası sertifika nesnesi sipariş x509 sertifikalar için seçilen sertifika veren sağlayıcıyla iletişim kurmak için kullanılan bir yapılandırma tutar.  

-   SSL sertifikaları için sertifika veren sağlayıcıları aşağıdaki ile anahtar kasası ortakları

|**Sağlayıcı adı**|**Konumlar**|
|----------|--------|
|DigiCert|Tüm anahtar kasası hizmet konumları genel bulut Azure kamu desteklenir|
|GlobalSign|Tüm anahtar kasası hizmet konumları genel bulut Azure kamu desteklenir|

Sertifikayı veren bir anahtar kasası oluşturulabilmesi için önce aşağıdaki önkoşul adımları 1 ve 2 başarıyla gerçekleştirilmesi gerekir.  

1. Sertifika yetkilisi (CA) sağlayıcıları için yerleşik  

    -   Bir kuruluş yönetici yerleşik gerekir (örneğin şirketi Contoso) en az bir CA sağlayıcısı ile.  

2. Yönetici SSL sertifikalarını kaydetmek (ve yenilemek) anahtar kasası için talep sahibinin kimlik bilgilerini oluşturur  

    -   Anahtar kasasına Sağlayıcısı'nın veren nesne oluşturmak için kullanılacak yapılandırma sağlar  

Sertifikaları Portalı'ndan veren nesneleri oluşturma hakkında daha fazla bilgi için bkz: [anahtar kasası sertifikaları blogu](http://aka.ms/kvcertsblog)  

Anahtar kasası farklı veren sağlayıcı yapılandırması ile birden çok veren nesneleri oluşturulmasını sağlar. Bir veren nesne oluşturulduktan sonra bir veya birden çok sertifika ilkelerine adını başvurulabilir. Anahtar kasası x509 isterken veren nesne belirtildiği gibi yapılandırma kullanmanız bildirir veren nesneye başvuran sertifikası oluşturma ve yenileme sırasında CA sağlayıcısından sertifika.  

Veren nesneleri kasaya oluşturulur ve yalnızca aynı kasadaki KV sertifikalarla kullanılabilir.  

###  <a name="BKMK_CertificateContacts"></a> Sertifika kişiler

Sertifika kişiler sertifika ömrü olaylar tarafından tetiklenen bildirimleri göndermek için kişi bilgilerini içerir. Kişi bilgileri tarafından tüm sertifikalar anahtar kasasını paylaşılır. Bir bildirim anahtar kasasında herhangi bir sertifika için bir olay için belirtilen tüm kişileri gönderilir.  

Otomatik yenileme için bir sertifika ilkesi ayarlarsanız, bir bildirim aşağıdaki olayları gönderilir.  

-   Sertifika yenileme önce
-   Sertifika başarıyla yenilendi belirten ya da bir hata varsa, el ile sertifikanın yenilenmesini gerektiren sertifika yenileme sonra.  

 Bir sertifikanın ilke el ile olarak ayarlanmışsa, yenilenen (yalnızca e-postalar) sonra bir bildirim gönderilir sertifikayı yenilemek için zaman olduğunda.  

###  <a name="BKMK_CertificateAccessControl"></a> Sertifika erişim denetimi

 Erişim denetimi sertifikaları için anahtar kasası tarafından yönetilen ve bu sertifikaları kapsayıcı olarak davranan bir anahtar kasası düzeyinde sağlanır. Anahtarları ve gizli anahtarları aynı anahtar kasasında için erişim denetimi İlkesi farklıdır sertifikaları için bir erişim denetimi İlkesi yok. Kullanıcıların sertifikalar tutmak için bir veya daha fazla kasalarını oluşturabilir ve senaryo uygun kesimleme ve sertifika yönetimi sağlamak için gereklidir.  

 Aşağıdaki izinleri, asıl başına temelinde, bir anahtar kasası ve yakından yansıtmalar gizli bir nesne üzerinde izin işlemleri gizli erişim denetim girdisi olarak kullanılabilir:  

-   *Alma*: get Geçerli sertifika sürümü veya herhangi bir sürümünü bir sertifika sağlar 
-   *Liste*: geçerli sertifikaların listesi bir sertifika sürümlerini sağlar  
-   *silme*: delete bir sertifika, kendi ilke ve tüm alt sürümlerinin sağlar  
-   *oluşturma*: sağlayan bir anahtar kasası sertifikası oluşturun.  
-   *Alma*: bir anahtar kasası sertifika alma sertifika malzeme sağlar.  
-   *Güncelleştirme*: sertifika güncelleştirilmesini sağlar.  
-   *manageconnects*: anahtar kasası sertifika kişiler yönetilmesine izin verir  
-   *getissuers*: bir sertifikanın verenler get sağlar  
-   *listissuers*: sertifikanın sertifika verenlerin listesini sağlar  
-   *setissuers*: verir oluşturun veya güncelleştirin anahtar kasası sertifika verenler  
-   *deleteissuers*: delete anahtar kasası sertifika verenler sağlar  
-   *tüm*: tüm izinleri verir  

## <a name="additional-information-for-certificates"></a>Sertifikalar için ek bilgiler

- [Sertifikalar ve ilkeleri](/rest/api/keyvault/certificates-and-policies)
- [Sertifika verenler](/rest/api/keyvault/certificate-issuers)
- [Sertifika kişiler](/rest/api/keyvault/certificate-contacts)

## <a name="see-also"></a>Ayrıca Bkz.

- [Kimlik doğrulama, istekleri ve yanıtları](authentication-requests-and-responses.md)
- [Anahtar kasası sürümleri](key-vault-versions.md)
- [Anahtar kasası Geliştirici Kılavuzu](/azure/key-vault/key-vault-developers-guide)
