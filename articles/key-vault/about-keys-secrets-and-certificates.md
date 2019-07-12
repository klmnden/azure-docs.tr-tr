---
title: Azure Key Vault anahtarlara, parolalara ve sertifikalara - Azure Key Vault hakkında
description: Azure anahtar kasası REST arabirimi ve geliştirici ayrıntılarını anahtarlara, parolalara ve sertifikalara genel bakış.
services: key-vault
author: msmbaldwin
manager: barbkess
tags: azure-resource-manager
ms.service: key-vault
ms.topic: conceptual
ms.date: 01/07/2019
ms.author: mbaldwin
ms.openlocfilehash: 52a0bc1b07ebf1aed55551e37ecc122ff393c0f7
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67703913"
---
# <a name="about-keys-secrets-and-certificates"></a>Anahtarlar, parolalar ve sertifikalar hakkında

Azure Key Vault, Microsoft Azure uygulamalarını ve birden fazla gizli dizi/anahtar verileri depolamak ve kullanıcılar sağlar:

- Şifreleme anahtarları: Birden çok anahtar türleri ve algoritmalarını destekler ve yüksek değerli anahtarlar donanım güvenlik modülleri (HSM) kullanılmasına olanak tanır. 
- Gizli diziler: Parola ve veritabanı bağlantı dizeleri gibi gizli dizileri güvenli olarak depolanmasını sağlar.
- Sertifikaları: Anahtarları ve gizli anahtarları üzerine yapılandırılmıştır ve otomatik yenileme özelliğini ekleyin sertifikalarını da destekler.
- Azure Depolama: Bir Azure depolama hesabı anahtarları, sizin için yönetebilir. Dahili olarak, anahtar kasası bir Azure depolama hesabı ile (eşitleme) anahtarları listeleme ve (döndürme) yeniden anahtarlarını düzenli aralıklarla. 

Key Vault hakkında daha fazla genel bilgi için bkz. [Azure anahtar kasası nedir?](/azure/key-vault/key-vault-whatis)

## <a name="azure-key-vault"></a>Azure Key Vault

Aşağıdaki bölümlerde, Key Vault hizmeti, uygulama ilgili genel bilgiler sunar.

### <a name="supporting-standards"></a>Standartlarını destekleyen

JavaScript nesne gösterimi (JSON) ve JavaScript nesne imzalama ve şifreleme (JOSE) belirtimleri önemli bilgiler var.  

-   [JSON Web anahtarı (JWK)](https://tools.ietf.org/html/draft-ietf-jose-json-web-key)  
-   [JSON Web Encryption (JWE)](http://tools.ietf.org/html/draft-ietf-jose-json-web-encryption)  
-   [JSON Web algoritmalar (JWA)](http://tools.ietf.org/html/draft-ietf-jose-json-web-algorithms)  
-   [JSON Web imza (JWS)](https://tools.ietf.org/html/draft-ietf-jose-json-web-signature)  

### <a name="data-types"></a>Veri türleri

Anahtarlar, şifreleme ve imzalama için ilgili veri türleri için JOSE belirtimlerine bakın.  

-   **algoritma** -anahtar işlemi, örneğin, RSA1_5 için desteklenen bir algoritması  
-   **ciphertext değerli** -metin sekizlik tabanda Base64URL kullanılarak kodlanır, şifre  
-   **Özet-değer** -Base64URL kullanılarak kodlanan bir karma algoritması çıktısı  
-   **anahtar türü** -bir desteklenen anahtar türleri, örneğin RSA (Rivest-Shamir-Adleman).  
-   **düz metin değeri** -Base64URL kullanılarak kodlanır, düz metin sekizli  
-   **İmza değeri** - çıkış Base64URL kullanılarak kodlanan bir imza algoritması  
-   **base64URL** -bir Base64URL [RFC4648] kodlanmış ikili değer  
-   **Boole** -true veya false  
-   **Kimlik** - bir kimlik gelen Azure Active Directory (AAD).  
-   **IntDate** - 1970'ten saniye sayısını temsil eden bir JSON ondalık değeri-01-kadar belirtilen UTC tarihi/saati UTC 01T0:0:0Z. Özellikle ilişkin ayrıntılarla ilgili genel date/times RFC3339 ve UTC bakın.  

### <a name="objects-identifiers-and-versioning"></a>Nesne tanımlayıcıları ve sürüm oluşturma

Yeni bir örneğini bir nesne oluşturulduğunda, anahtar Kasası'nda depolanan nesnelere tutulur. Her sürüm, bir benzersiz tanımlayıcı ve URL atanır. Bir nesne ilk oluşturulduğunda verilen benzersiz sürüm tanımlayıcısı ve nesnenin geçerli sürümü olarak işaretlenmiş. Aynı nesne adı ile yeni bir örneğinin oluşturulmasını, yeni nesne geçerli sürümü duruma neden olan bir benzersiz sürüm tanımlayıcısı sağlar.  

Anahtar Kasası'nda nesneleri geçerli tanımlayıcısı veya bir sürüme özgü tanımlayıcısı kullanarak çözülebilir. Örneğin, verili bir anahtarın adıyla `MasterKey`, geçerli bir tanımlayıcı işlemler gerçekleştirme sistemi en son sürüme neden olur. Sürüme özgü tanımlayıcıyı işlemler gerçekleştirme, nesne belirli bir sürümünü kullanmak sistemi neden olur.  

Nesneleri bir URL kullanarak Key Vault içinde benzersiz şekilde tanımlanır. İki nesne sistemde coğrafi konumdan bağımsız olarak aynı URL'ye sahip. Tam URL bir nesne için nesne tanımlayıcısı olarak adlandırılır. Key Vault, nesne türü tanımlayan bir önek URL oluşuyorsa, kullanıcı tarafından sağlanan nesne adı ve bir nesne sürümü. Nesne adı büyük/küçük harfe ve değişmez. Nesne sürümü içermez tanımlayıcıları temel tanımlayıcı olarak adlandırılır.  

Daha fazla bilgi için [kimlik doğrulaması, istekleri ve yanıtları](authentication-requests-and-responses.md)

Bir nesne tanımlayıcı genel biçimi aşağıdaki gibidir:  

`https://{keyvault-name}.vault.azure.net/{object-type}/{object-name}/{object-version}`  

Konumlar:  

|||  
|-|-|  
|`keyvault-name`|Microsoft Azure anahtar kasası hizmetindeki bir anahtar kasası adı.<br /><br /> Key Vault adları, kullanıcı tarafından seçilen ve genel olarak benzersiz.<br /><br /> Key Vault adı yalnızca 0-9, içeren bir 3-24 karakter dizesi olmalıdır a-z, A-Z ve -.|  
|`object-type`|"Anahtarlar" veya "gizli" nesnenin türü.|  
|`object-name`|Bir `object-name` için bir kullanıcı tarafından sağlanan ad ve bir Key Vault içinde benzersiz olmalıdır. Ad yalnızca 0-9, içeren 1-127 karakter dizesi olmalıdır a-z, A-Z ve -.|  
|`object-version`|Bir `object-version` bir sistem tarafından oluşturulmuş, isteğe bağlı olarak bir nesnenin benzersiz bir sürüm adresi için kullanılan 32 karakterli dize tanımlayıcı.|  

## <a name="key-vault-keys"></a>Key Vault anahtarları

### <a name="keys-and-key-types"></a>Anahtarları ve anahtar türleri

Şifreleme anahtarları Key vault'ta JSON Web anahtarı [JWK] nesneler olarak temsil edilir. Temel belirtimler JWK/JWA da anahtar kasası uygulama için benzersiz anahtar türleri etkinleştirmek için genişletilir. Örneğin, anahtarları HSM satıcıya özgü paketleme kullanarak içeri aktarma, anahtar kasası Hsm'lerde yalnızca kullanılan anahtarların güvenli taşıma sağlar.  

- **"Soft" anahtarları**: Bir anahtarı anahtar kasası tarafından yazılımda işlenir ancak bir HSM'de bir sistem anahtarı kullanılarak, bekleme sırasında şifrelenir. İstemciler mevcut bir RSA veya EC (Eliptik Eğri) anahtarını içeri aktarın veya Key Vault bir oluşturma isteği.
- **"Sabit" anahtarları**: Bir anahtar bir HSM (donanım güvenlik modülü) işlenir. Bu anahtarlar, anahtar kasası HSM güvenlik Dünyaları birinde korunur (bir güvenlik yalıtımı sağlamak dünya Coğrafya başına yoktur). İstemciler, bir RSA veya EC anahtarı, esnek bir biçimde veya uyumlu bir HSM CİHAZDAN vererek alabilirsiniz. İstemciler, bir anahtar oluşturmak için Key Vault ayrıca isteyebilir. Bu anahtar türü T öznitelik ekler HSM anahtar malzemesi yürütmek için için JWK edinin.

     Coğrafi sınırlar hakkında daha fazla bilgi için bkz. [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/privacy/)  

Key Vault RSA ve Eliptik Eğri anahtarlar yalnızca destekler. 

-   **EC**: "Soft" Elliptic Curve key.
-   **HSM EC**: "Sabit" Eliptik Eğri anahtar.
-   **RSA**: "Soft" RSA anahtarı.
-   **RSA HSM**: "Sabit" RSA anahtarı.

Key Vault, 2048, 3072 ve 4096 boyuttaki RSA anahtarlarını destekliyor. Key Vault destekler Eliptik Eğri anahtar türleri P-256, p-384, p-521 ve P-256_K (SECP256K1).

### <a name="cryptographic-protection"></a>Şifreleme koruma

Anahtar Kasası'nı kullandığından, şifreleme modüllerini HSM veya yazılım, FIPS doğrulanmış (Federal Bilgi işleme standartları) olduğunu belirtir. FIPS modunda çalışacak şekilde özel herhangi bir şey yapmanız gerekmez. Anahtarları **oluşturulan** veya **içeri** HSM korumalı olan 140-2 Düzey 2 FIPS doğrulanmış HSM'nin içinde işlenir. Anahtarları **oluşturulan** veya **içeri** doğrulanmış FIPS 140-2 Düzey 1 şifreleme modüllerine içinde yazılım korumalı olarak işlenir. Daha fazla bilgi için [anahtarları ve anahtar türleri](#keys-and-key-types).

###  <a name="ec-algorithms"></a>EC algoritmaları
 Aşağıdaki algoritması tanımlayıcıları EC ve EC-HSM anahtarları Key vault'ta desteklenir. 

#### <a name="curve-types"></a>Eğri türleri

-   **P-256** -p-256, tanımlanan NIST eğrisi [DSS FIPS PUB 186 4](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.186-4.pdf).
-   **P-256_K** -sn eğri SECP256K1, tanımlanan [sn 2: Eliptik Eğri Domain parametreleri önerilen](https://www.secg.org/sec2-v2.pdf).
-   **P-384** -p-384 tanımlanan NIST eğrisi [DSS FIPS PUB 186 4](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.186-4.pdf).
-   **P-521** -p-521 tanımlanan NIST eğrisi [DSS FIPS PUB 186 4](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.186-4.pdf).

#### <a name="signverify"></a>OTURUM VE DOĞRULAYIN

-   **ES256** - ECDSA SHA-256 özetleyen ve anahtarları eğri p-256 ile oluşturulur. Bu algoritma anlatıldığı [RFC7518](https://tools.ietf.org/html/rfc7518).
-   **ES256K** - ECDSA SHA-256 özetleyen ve anahtarları P-256_K eğri ile oluşturulur. Standardizasyon algoritmasıdır.
-   **ES384** - ECDSA için SHA-384 özetleyen ve anahtarları p-384 eğri ile oluşturulur. Bu algoritma anlatıldığı [RFC7518](https://tools.ietf.org/html/rfc7518).
-   **ES512** - ECDSA için SHA-512 özetleyen ve anahtarları, p-521 eğrisini ile oluşturulur. Bu algoritma anlatıldığı [RFC7518](https://tools.ietf.org/html/rfc7518).

###  <a name="rsa-algorithms"></a>RSA algoritmalarını  
 RSA ve RSA HSM anahtarları Key vault'ta aşağıdaki algoritması tanımlayıcılar desteklenir.  

#### <a name="wrapkeyunwrapkey-encryptdecrypt"></a>/ UNWRAPKEY, WRAPKEY ŞİFRELEME/ŞİFRE ÇÖZME

-   **RSA1_5** -RSAES PKCS1 V1_5 [RFC3447] anahtar şifreleme  
-   **RSA OAEP** - en iyi asimetrik şifreleme (OAEP) [RFC3447] bölümü A.2.1, RFC 3447 tarafından belirtilen varsayılan parametreleri olan doldurma kullanarak RSAES. Bu varsayılan parametreleri, SHA-1 ile SHA-1 karma işlevi ve MGF1 maskesi oluşturma işlevi kullanıyor.  

#### <a name="signverify"></a>OTURUM VE DOĞRULAYIN

-   **RS256** - RSASSA-PKCS-v1_5 SHA-256'yı kullanarak. Sağlanan uygulama Özet değeri SHA-256 kullanılarak hesaplanması gerekir ve uzunluğu 32 bayt olması gerekir.  
-   **RS384** - RSASSA-PKCS-v1_5 SHA-384 kullanarak. Sağlanan uygulama Özet değeri SHA-384'ı kullanarak hesaplanan gerekir ve 48 bayt uzunluğunda olmalıdır.  
-   **RS512** - RSASSA-PKCS-v1_5 SHA-512 kullanarak. Sağlanan uygulama Özet değeri SHA-512'ı kullanarak hesaplanan olmalıdır ve uzunluğu 64 baytın olmalıdır.  
-   **RSNULL** -bakın [RFC2437], bir özel kullanım belirli TLS senaryoları etkinleştirmek için örneği.  

###  <a name="key-operations"></a>Anahtar işlemleri

Key Vault anahtar nesneler üzerinde aşağıdaki işlemleri destekler:  

-   **Oluşturma**: Anahtar Kasası'nda bir anahtar oluşturmak bir istemci sağlar. Anahtar değerini Key Vault tarafından oluşturulan ve tutulan ve istemciye serbest değil. Asimetrik anahtarlar, anahtar Kasası'nda oluşturulabilir.  
-   **İçeri aktarma**: Mevcut bir anahtarı anahtar Kasası'na içeri aktarmak bir istemci sağlar. Asimetrik anahtarlar, farklı paketleme yöntemler JWK yapısı içinde bir dizi kullanarak Key Vault alınabilir. 
-   **Güncelleştirme**: Key Vault içinde daha önce depolanan bir anahtar ile ilişkili meta verileri (anahtar öznitelikleri) değiştirmek için yeterli izinlere sahip bir istemci sağlar.  
-   **Silme**: Key Vault'tan bir anahtar silmek için yeterli izinlere sahip bir istemci sağlar.  
-   **Liste**: Bir istemci belirli bir anahtar Kasası'nda tüm anahtarların listelenmesi sağlar.  
-   **Sürümleri Listele**: Tüm belirli bir anahtar Kasası'nda belirli bir anahtarın sürümlerini listeleme istemciye sağlar.  
-   **Alma**: Belirli bir anahtarı bir anahtar Kasası'nda genel bölümlerini almak bir istemci sağlar.  
-   **Yedekleme**: Korumalı bir form anahtarında dışarı aktarır.  
-   **Geri yükleme**: Daha önce yedeklenen bir anahtarı içeri aktarır.  

Daha fazla bilgi için [anahtarı anahtar kasası REST API Başvurusu işlemlerinde](/rest/api/keyvault).  

Bir anahtarı anahtar Kasası'nda oluşturulduktan sonra aşağıdaki şifreleme işlemleri anahtar kullanılarak gerçekleştirilebilir:  

-   **Oturum açın ve doğrulama**: Kesinlikle, Key Vault içeriğini imza oluşturmanın bir parçası karma desteklemediğinden bu işlem "imza karması" veya "karma doğrulama" olur. Uygulamalar verileri yerel olarak imzalanmasını karma ardından bu Key Vault oturum karma istek. İmzalı karmalarının doğrulama [genel] anahtar malzemesi için erişiminiz olmayabilir uygulamalar için bir kolaylık işlemi olarak desteklenir. En iyi uygulama performansını işlemlerini yerel olarak gerçekleştirildiğinden emin olun.  
-   **Anahtar şifreleme / kaydırma**: Key Vault'ta depolanan bir anahtar, başka bir anahtar, bir simetrik içerik şifreleme anahtarı (CEK) genellikle korumak için kullanılabilir. Anahtar şifreleme anahtarı anahtar Kasası'nda asimetrik olduğunda kullanılır. Örneğin, RSA OAEP ve WRAPKEY/UNWRAPKEY işlemleri için şifreleme/şifre çözme eşdeğerdir. Simetrik anahtarı anahtar Kasası'nda, sarmalama anahtar kullanılır. Örneğin, AES-KW. WRAPKEY işlemi [genel] anahtar malzemesi için erişiminiz olmayabilir uygulamalar için bir kolaylık olarak desteklenir. En iyi uygulama performansını WRAPKEY işlemlerini yerel olarak gerçekleştirilmelidir.  
-   **Şifreleme ve şifre çözme**: Key Vault'ta depolanan bir anahtar şifreleme veya tek bir blok verilerin şifresini çözmek için kullanılabilir. Blok boyutu, seçilen şifreleme algoritması ve anahtar türü tarafından belirlenir. Şifreleme işlemi [genel] anahtar malzemesi için erişiminiz olmayabilir uygulamalar için bir kolaylık olması için sağlanmıştır. En iyi uygulama performansı için şifreleme işlemlerini yerel olarak gerçekleştirilmelidir.  

(İşlemi şifreleme/şifre çözme için eşdeğer olduğu gibi) WRAPKEY/UNWRAPKEY asimetrik anahtarlarla gereksiz görünebilir, ayrı işlem kullanımı önemlidir. Ayrımın semantic sağlar ve bu işlemleri ve diğer anahtar türleri, hizmet tarafından desteklendiğinde tutarlılık yetkilendirme ayrımı.  

Key Vault, dışa aktarma işlemleri desteklemez. Bir anahtar sistemde sağlandıktan sonra bunu ayıklanamıyor veya kendi anahtar malzemesi değiştirilemiyor. Ancak, diğer kullanmak için silinmiş durumda gibi olarak sonra Key Vault kullanıcıları kendi anahtar gerektirebilir. Bu durumda, bunlar korumalı bir form anahtarında içeri/dışarı aktarma için yedekleme ve geri yükleme işlemleri kullanabilir. Yedekleme işlemi tarafından oluşturulan anahtarları Key Vault dışında kullanılamaz. Alternatif olarak, içeri aktarma işlemi birden çok Key Vault örneği karşı kullanılabilir.  

Kullanıcılar, herhangi bir Key Vault JWK key_ops özelliğini kullanarak anahtarı başına temelinde desteklediği şifreleme işlemleri kısıtlayabilir.  

JWK nesneleri hakkında daha fazla bilgi için bkz. [JSON Web anahtarı (JWK)](https://tools.ietf.org/html/draft-ietf-jose-json-web-key).  

###  <a name="key-attributes"></a>Anahtar öznitelikleri

Anahtar malzemesi ek olarak, aşağıdaki öznitelikleri belirtilebilir. Bir JSON isteği, öznitelikleri anahtar sözcüğü ve küme ayraçları, ' {' '}', belirtilen öznitelikleri olsa bile gereklidir.  

- *Etkin*: Boole, isteğe bağlı, varsayılan **true**. Anahtarın etkin ve şifreleme işlemleri için kullanılabilir olup olmadığını belirtir. *Etkin* özniteliği ile birlikte kullanılır *nbf* ve *exp*. Bir işlemi oluştuğunda arasında *nbf* ve *exp*, varsa yalnızca verilmez *etkin* ayarlanır **true**. İşlemler dışındaki *nbf* / *exp* penceresi otomatik olarak izin verilmez, belirli işlem türleri altında dışında [belirli koşullar](#date-time-controlled-operations).
- *NBF*: IntDate, isteğe bağlı, varsayılan sunulmuştur. *Nbf* (değil önce) öznitelik tanımlar, önce anahtarı değil kullanılmalıdır altında belirli işlem türlerini hariç şifreleme işlemleri için zaman [belirli koşullar](#date-time-controlled-operations). İşlenmesini *nbf* özniteliği gerektirir geçerli tarih/saat sonra olmanız gerekir veya eşit olmayan-listelenen tarih/saat önce *nbf* özniteliği. Bazı küçük leeway için Key Vault sağlayabilir, normalde saati hesap için en fazla birkaç dakika eğme. Değerini IntDate değerini içeren bir sayı olmalıdır.  
- *exp*: IntDate, isteğe bağlı, varsayılan "sonsuz" dir. *Exp* (süre) öznitelik tanımlar ve sonrasında, bu anahtar gerekir altında belirli işlem türlerini hariç şifreleme işlemi için kullanılan süre sonu zamanı [belirli koşullar](#date-time-controlled-operations). İşlenmesini *exp* özniteliği gerektirir süre sonu tarihi/saati listelenen önce geçerli tarih/saat olmalıdır *exp* özniteliği. Bazı küçük leeway için Key Vault sağlayabilir, genellikle saat için hesap için en fazla birkaç dakika eğme. Değerini IntDate değerini içeren bir sayı olmalıdır.  

Anahtar öznitelikleri içeren bir yanıta dahil ek salt okunur özniteliği vardır:  

- *oluşturulan*: IntDate, isteğe bağlı. *Oluşturulan* öznitelik, bu anahtarın sürümü ne zaman oluşturulduğunu gösterir. Değer, bu öznitelik eklenmesini önce oluşturulan anahtarları için null. Değerini IntDate değerini içeren bir sayı olmalıdır.  
- *güncelleştirilmiş*: IntDate, isteğe bağlı. *Güncelleştirilmiş* öznitelik, bu anahtarın sürümü ne zaman güncelleştirildiği gösterir. Değer, bu öznitelik eklenmesini önce en son güncelleştirildiği anahtarları için null. Değerini IntDate değerini içeren bir sayı olmalıdır.  

IntDate ve diğer veri türleri hakkında daha fazla bilgi için bkz. [veri türleri](#data-types)  

#### <a name="date-time-controlled-operations"></a>Tarih-saat denetimli işlemleri

Not henüz geçerli ve anahtarlar, dış süresi *nbf* / *exp* penceresinde için çalışır **şifresini**, **sarmalamadan çıkarma**ve **doğrulayın** işlemleri (403, iade olmaz Yasak). Stratejinin değil ancak geçerli durum kullanmak için üretim kullanılmadan önce test edilecek bir anahtar izin vermektir. Süresi dolmuş kullanmaya yönelik bir stratejinin anahtarının geçerli olduğu zaman, oluşturulan veri kurtarma işlemleri sağlamaktır. Key Vault ilkelerini kullanarak bir anahtar veya güncelleştirerek erişimini devre dışı bırakabilirsiniz ayrıca *etkin* anahtar özniteliğiyle **false**.

Veri türleri hakkında daha fazla bilgi için bkz. [veri türleri](#data-types).

Diğer olası öznitelikleri hakkında daha fazla bilgi için bkz. [JSON Web anahtarı (JWK)](https://tools.ietf.org/html/draft-ietf-jose-json-web-key).

### <a name="key-tags"></a>Anahtarını etiketleri

Ek uygulamaya özgü meta veri etiketleri biçiminde belirtebilirsiniz. Key Vault, en fazla 15 etiket, her biri olan 256 karakterlik bir ad ve 256 karakter değeri destekler.  

>[!Note]
>Bunlar varsa etiketleri çağıran tarafından okunabilir *listesi* veya *alma* söz konusu nesne türü (anahtarlar, parola veya sertifikalara) izni.

###  <a name="key-access-control"></a>Anahtar erişim denetimi

Key Vault tarafından yönetilen anahtarlar için erişim denetimi, anahtar kapsayıcısı olarak davranan bir Key Vault düzeyinde sağlanır. Anahtarlar için erişim denetimi İlkesi aynı Key Vault'ta ' gizli diziler için erişim denetimi ilkesini farklı. Kullanıcılar, anahtarları tutmak için bir veya daha fazla kasalarını oluşturabilir ve senaryoya uygun segmentlere ayırma ve anahtar yönetimi tutmaları zorunludur. Anahtarlar için erişim denetimi, gizli dizileri için erişim denetimi bağımsızdır.  

Aşağıdaki izinleri, üzerinde verilebilecek bir kullanıcı / hizmet sorumlusu aralıklarla bir kasasındaki anahtarları erişim denetim girişi. Bu izinleri bir anahtar nesnesi üzerinde izin verilen işlemleri yakından yansıtır.  Anahtar Kasası'nda asıl hizmetine erişim izni verme tek seferlik bir işlemdir ve tüm Azure abonelikleri için aynı kalır. İstediğiniz sayıda sertifikaları dağıtmak için kullanabilirsiniz. 

- Anahtar yönetimi işlemleri için izinleri
  - *Alma*: Bir anahtar yanı sıra, özniteliklerini ortak bölümünü okuyun
  - *Liste*: Bir anahtar Kasası'nda depolanan bir anahtarın sürümlerini ve anahtarlar listesi
  - *Güncelleştirme*: Bir anahtar için öznitelikleri güncelleştir
  - *Oluşturma*: Yeni anahtarlar oluşturun
  - *İçeri aktarma*: Bir anahtar kasasına bir anahtar aktarma
  - *Silme*: Anahtar nesnesi Sil
  - *Kurtarma*: Silinen bir anahtar kurtarma
  - *Yedekleme*: Bir anahtarı bir anahtar kasasına yedekleme
  - *Geri yükleme*: Yedeklenen bir key vault için anahtarı geri yükleme

- Şifreleme işlemleri için izinleri
  - *şifre çözme*: Bayt dizisi korumasını kaldırmak için kullanma
  - *şifreleme*: Bir rastgele bayt dizisini korunacak anahtarını kullanın
  - *unwrapKey*: Sarmalanan simetrik anahtarlar korumasını kaldırmak için kullanma
  - *wrapKey*: Simetrik anahtar korumak için anahtarı kullanın.
  - *doğrulama*: Anahtar özetler doğrulamak için kullanılır  
  - *oturum*: Özetler imzalamak için anahtar kullanın.
    
- Ayrıcalıklı işlemleri için izinleri
  - *Temizleme*: Temizle (kalıcı) silinen bir anahtar

Anahtarları ile çalışma hakkında daha fazla bilgi için bkz. [anahtarı anahtar kasası REST API Başvurusu işlemlerinde](/rest/api/keyvault). İzinler oluşturma hakkında daha fazla bilgi için bkz: [kasaları - oluştur veya Güncelleştir](/rest/api/keyvault/vaults/createorupdate) ve [kasaları - güncelleştirme erişim ilkesi](/rest/api/keyvault/vaults/updateaccesspolicy). 

## <a name="key-vault-secrets"></a>Key Vault gizli dizileri 

### <a name="working-with-secrets"></a>Gizli anahtarlarla çalışma

Bir geliştiricinin bakış açısından, anahtar kasası API'leri kabul edin ve gizli anahtar değerleri dize olarak döndürür. Dahili olarak, anahtar kasası depolar ve gizli dizileri ile en fazla 25 k bayt her sekizlik tabanda (8-bit bayt), bir dizisi yönetir. Key Vault hizmeti, gizli dizileri için semantiği sağlamaz. Yalnızca verileri kabul eder, şifreler, depoladığı ve bir gizli dizi tanımlayıcısı ("id") döndürür. Tanımlayıcı, daha sonraki bir zamanda gizli diziyi almak için kullanılabilir.  

Yüksek oranda gizli veriler için ek veri koruma katmanları istemciler dikkate almanız gerekir. Key Vault'ta depolama önce bir ayrı koruma anahtarı kullanılarak veri şifrelenirken bir örnektir.  

Key Vault, ayrıca bir contentType alanı için gizli dizilerini destekler. İstemciler, içerik türünü gizli verileri yorumlama alındığı sırada yardımcı olmak için bir gizli anahtarı belirtebilir. Bu alan en fazla uzunluğu 255 karakterdir. Önceden tanımlanmış değerler yoktur. Önerilen kullanım için gizli verileri yorumlama ipucu gibidir. Örneği için bir uygulama hem parolalar ve sertifikalar gizli dizi olarak depolamak ve ardından ayırt etmek için bu alanı kullanın. Önceden tanımlanmış değerler yoktur.  

### <a name="secret-attributes"></a>Gizli dizi öznitelikleri

Gizli veriler ek olarak, aşağıdaki öznitelikleri belirtilebilir:  

- *exp*: IntDate, isteğe bağlı, varsayılan **sonsuza kadar**. *Exp* (süre) öznitelik tanımlar ve sonrasında, gizli verilerin alınması GEREKTİĞİNİ değil, sona erme zamanı hariç [belirli durumlarda](#date-time-controlled-operations). Bu alan için olan **bilgilendirici** yalnızca belirli bir gizli dizi kullanılamaz anahtar kasası hizmeti, kullanıcıları bilgilendirir gibi amacıyla. Değerini IntDate değerini içeren bir sayı olmalıdır.   
- *NBF*: IntDate, isteğe bağlı, varsayılan **artık**. *Nbf* (değil önce) öznitelik tanımlar önüne gizli verileri SecurityDescriptor, zaman hariç [belirli durumlarda](#date-time-controlled-operations). Bu alan için olan **bilgilendirici** yalnızca amacıyla. Değerini IntDate değerini içeren bir sayı olmalıdır. 
- *Etkin*: Boole, isteğe bağlı, varsayılan **true**. Bu öznitelik, gizli veriler alınıp alınamayacağını belirtir. Etkinleştirilmiş bir öznitelik ile birlikte kullanılan *nbf* ve *exp* bir işlemi oluştuğunda arasında *nbf* ve *exp*, yalnızca olacaktır etkinleştirilirse izin kümesine olduğu **true**. İşlemler dışındaki *nbf* ve *exp* penceresi olan otomatik olarak izin verilmeyen, içinde hariç [belirli durumlarda](#date-time-controlled-operations).  

Gizli dizi öznitelikleri içeren bir yanıta dahil ek salt okunur özniteliği vardır:  

- *oluşturulan*: IntDate, isteğe bağlı. Oluşturulan öznitelik, bu gizli dizi sürümü ne zaman oluşturulduğunu gösterir. Bu değer, bu öznitelik eklenmesini önce oluşturulan gizli dizileri için null. Değerini IntDate değerini içeren bir sayı olmalıdır.  
- *güncelleştirilmiş*: IntDate, isteğe bağlı. Bu gizli dizi sürümü ne zaman güncelleştirildiği güncelleştirilmiş öznitelik gösterir. Bu değer, bu öznitelik eklenmesini önce en son güncelleştirildiği gizli dizileri için null. Değerini IntDate değerini içeren bir sayı olmalıdır.

#### <a name="date-time-controlled-operations"></a>Tarih-saat denetimli işlemleri

Bir parolanın **alma** işlemi dışında değil henüz geçerli ve süresi dolan gizli diziler çalışır *nbf* / *exp* penceresi. Bir parolanın çağırma **alma** değil henüz geçerli bir gizli dizi için bir işlemi test amaçları için kullanılabilir. Alınıyor (**alma**eğerlendirmesi) süresi dolmuş bir parola kurtarma işlemleri için kullanılabilir.

Veri türleri hakkında daha fazla bilgi için bkz. [veri türleri](#data-types).  

### <a name="secret-access-control"></a>Parola erişim denetimi

Anahtar Kasası'nda yönetilen gizli dizileri için erişim denetimi, bu gizli dizileri içeren anahtar kasası düzeyinde sağlanır. Erişim denetimi ilkesi için gizli dizileri, aynı anahtar kasasındaki anahtarlar için erişim denetimi ilkesini farklı. Kullanıcılar, gizli tutmak için bir veya daha fazla kasalarını oluşturabilir ve senaryoya uygun segmentlere ayırma ve gizli dizilerinin Yönetimi tutmaları zorunludur.   

Aşağıdaki izinleri, bir asıl başına temelinde bir kasasındaki gizli dizileri erişim denetim girişi ve kullanılabilir bir gizli dizi nesnesinde izin verilen işlemleri yakından yansıtma:  

- Gizli dizi yönetimi işlemleri için izinleri
  - *Alma*: Gizli dizi okumak  
  - *Liste*: Gizli dizileri ya da Key Vault'ta depolanan bir gizli anahtarın sürümlerini listeler.  
  - *ayarlama*: Gizli anahtar oluşturma  
  - *Silme*: Gizli anahtarı silme  
  - *Kurtarma*: Silinen bir gizli dizi Kurtar
  - *Yedekleme*: Bir anahtar kasasındaki gizli dizi yedekleyin
  - *Geri yükleme*: Yedeklenen bir gizli bir anahtar kasası için yedekleme geri yükleme

- Ayrıcalıklı işlemleri için izinleri
  - *Temizleme*: Temizle (kalıcı) silinen bir gizli dizi

Gizli anahtarlarla çalışma hakkında daha fazla bilgi için bkz. [Key Vault REST API Başvurusu'ndaki gizli dizi işlemleri](/rest/api/keyvault). İzinler oluşturma hakkında daha fazla bilgi için bkz: [kasaları - oluştur veya Güncelleştir](/rest/api/keyvault/vaults/createorupdate) ve [kasaları - güncelleştirme erişim ilkesi](/rest/api/keyvault/vaults/updateaccesspolicy). 

### <a name="secret-tags"></a>Gizli etiketleri  
Ek uygulamaya özgü meta veri etiketleri biçiminde belirtebilirsiniz. Key Vault, en fazla 15 etiket, her biri olan 256 karakterlik bir ad ve 256 karakter değeri destekler.  

>[!Note]
>Bunlar varsa etiketleri çağıran tarafından okunabilir *listesi* veya *alma* söz konusu nesne türü (anahtarlar, parola veya sertifikalara) izni.

## <a name="key-vault-certificates"></a>Anahtar kasası sertifikaları

Key Vault sertifikaları destek sağlar, x509 yönetimi için sertifikaları ve aşağıdaki davranışları:  

-   Sertifikayı içeri aktarma veya Key Vault oluşturma işlemi aracılığıyla bir sertifika oluşturmak bir sertifika sahibinin sağlar. İçerir hem de otomatik olarak imzalanan ve sertifika yetkilisi sertifikalarını oluşturulur.
-   Güvenli Depolama ve Yönetimi X509 uygulamak bir anahtar kasası sertifikası sahibi sağlayan sertifika özel anahtar malzemesi etkileşim olmadan.  
-   Bir sertifika yaşam döngüsünü yönetmek için Key Vault yönlendiren bir ilke oluşturmak bir sertifika sahibinin sağlar.  
-   Kişi hakkında bilgi için bildirim yaşam döngüsü olayları geçerlilik süresi ve sertifikanın yenilenmesini sağlamak sertifika sahipleri sağlar.  
-   Seçili verenler - Key Vault iş ortağı X509 ile Otomatik yenilemeyi destekler sertifika sağlayıcıları / sertifika yetkilileri.

>[!Note]
>Olmayan iş Birliği yaparak sağlayıcıları/yetkilileri de izin verilir ancak otomatik yenileme özelliğini desteklemez.

### <a name="composition-of-a-certificate"></a>Sertifika oluşturma

Anahtar kasası sertifikası oluşturulurken bir adreslenebilir anahtar ve gizli dizi de aynı ada sahip oluşturulur. Key Vault anahtarı anahtar işlemleri ve Key Vault gizli sertifika değeri bir gizli dizi olarak alınmasına olanak sağlar. Anahtar kasası sertifikası ortak x509 de içeren sertifika meta verileri.  

Sertifikaların sürümü ve tanımlayıcı, anahtar ve gizli dizi benzer. Key Vault sertifikası yanıtta bir adreslenebilir anahtar ve gizli anahtar kasası sertifikası sürümüyle oluşturulan belirli bir sürümü kullanılabilir.
 
![Karmaşık nesneler sertifikalardır](media/azure-key-vault.png)

### <a name="exportable-or-non-exportable-key"></a>Anahtar dışarı aktarılabilir veya olmayan dışarı aktarılabilir

Anahtar kasası sertifikası oluşturulduğunda, adreslenebilir gizli anahtarından PFX veya PEM biçiminde özel anahtarla alınabilir. Bir sertifika oluşturmak için kullanılan ilke anahtarı verilebilir belirtmeniz gerekir. İlkeyi dışarı aktarılabilir olmayan gösteriyorsa, özel anahtar değerinin bir gizli dizi alınırken bir parçası değil.  

Adreslenebilir anahtar KV sertifikaları aktarılamaz daha ilgili hale gelir. Gelen adreslenebilir KV anahtarının operations eşlenen *keyusage* KV sertifika ilkesi KV sertifikayı oluşturmak için kullanılan alan.  

İki anahtar türleri desteklenir: *RSA* veya *RSA HSM* sertifikalara sahip. Dışarı aktarılabilir yalnızca RSA HSM tarafından desteklenmeyen RSA ile izin verilir.  

### <a name="certificate-attributes-and-tags"></a>Sertifika öznitelikler ve etiketler

Sertifika meta verileri, bir adreslenebilir anahtar ve adreslenebilir gizli dizi ek olarak, anahtar kasası sertifikası öznitelikler ve etiketler de içerir.  

#### <a name="attributes"></a>Öznitelikler

Sertifika öznitelikleri, öznitelikleri adreslenebilir anahtar ve gizli dizi KV sertifika oluşturduğunuzda oluşturulan yansıtılır.  

Bir Key Vault sertifika aşağıdaki özniteliklere sahiptir:  

-   *Etkin*: Boole, isteğe bağlı, varsayılan **true**. Sertifika verileri gizli ya da çalıştırılabilir bir anahtar olarak alınabilir, belirtmek için belirtilebilir. Ayrıca ile birlikte kullanılan *nbf* ve *exp* bir işlemi oluştuğunda arasında *nbf* ve *exp*ve etkinleştirildiğinde yalnızca izin verilir ayarlanmış true. İşlemler dışındaki *nbf* ve *exp* penceresi otomatik olarak izin verilmiyor.  

Yanıta dahil ek salt okunur özniteliği vardır:

-   *oluşturulan*: IntDate: Bu sürüm sertifikanın oluşturulduğu gösterir.  
-   *güncelleştirilmiş*: IntDate: sertifikayı bu sürüm ne zaman güncelleştirildiği gösterir.  
-   *exp*: IntDate: x509 sona erme tarihi değerini içeren sertifika.  
-   *NBF*: IntDate: x509 tarih değerini içeren sertifika.  

> [!Note] 
> Key Vault sertifikanın süresi dolarsa adreslenebilir anahtarıdır ve gizli hale çalışamaz.  

#### <a name="tags"></a>Tags

 İstemci belirtilen benzer etiketler anahtarları ve gizli anahtar-değer çiftlerinin dictionary'si.  

 > [!Note]
> Bunlar varsa etiketleri çağıran tarafından okunabilir *listesi* veya *alma* söz konusu nesne türü (anahtarlar, parola veya sertifikalara) izni.

### <a name="certificate-policy"></a>Sertifika İlkesi

Bir sertifika ilkesi oluşturup bir Key Vault sertifikanın yaşam döngüsünü yönetme hakkında bilgi içerir. Özel anahtara sahip bir sertifikayı anahtar kasasına içeri aktarıldığında, varsayılan bir ilke x509 okuyarak oluşturulan sertifika.  

Anahtar kasası sertifikası sıfırdan oluşturulduğunda, bir ilke sağlanması gerekir. İlke bu Key Vault sertifikası sürümü veya sonraki Key Vault sertifikası sürümü oluşturulacağını belirtir. İlke oluşturulduktan sonra art arda ile gerekli değildir oluşturma işlemleri gelecekteki sürümleri. Anahtar kasası sertifikası tüm sürümleri için bir ilke yalnızca bir örneği vardır.  

Bir sertifika ilkesi, yüksek düzeyde, aşağıdaki bilgileri içerir:  

-   X509 özellikleri sertifika: Konu adı ve konu alternatif adlarını x x509 oluşturmak için kullanılan diğer özellikleri içeren sertifika isteği.  
-   Anahtar özellikleri: anahtar türü içeren anahtar dışarı aktarılabilir, uzunluğu ve anahtar alanları yeniden kullanabilirsiniz. Bu alanlar, anahtar kasasına bir anahtar oluşturmak nasıl isteyin.  
-   Gizli dizi özellikleri: içerik türünü gizli dizi olarak sertifika almak için gizli değer üretmek için adreslenebilir gizli gibi gizli dizi özelliklerini içerir.  
-   Yaşam süresi eylemler: KV sertifika ömrü eylemleri içerir. Her yaşam süresi eylem içerir:  

     - Tetikleyicisi: gün önce sona erme veya yaşam süresi aralık yüzdesi belirtilen  

     - Eylem: eylem türü – belirtme *emailContacts* veya *otomatik yenileme*  

-   Veren: Parametreleri x509 sertifikaları için kullanılacak sertifikayı veren hakkında.  
-   İlke öznitelikleri: ilke ile ilişkili öznitelikleri içerir.  

#### <a name="x509-to-key-vault-usage-mapping"></a>Key Vault kullanım eşleme X509

Aşağıdaki tabloda, anahtar kullanım ilkesi anahtar etkin anahtar işlemleri için bir anahtar kasası sertifikası oluşturmanın bir parçası oluşturulan x509 eşlemeyi temsil eder.

|**X509 anahtar kullanımı bayrakları**|**Key Vault anahtar işlemleri**|**Varsayılan davranış**|
|----------|--------|--------|
|DataEncipherment|Şifreleme, şifre çözme| Yok |
|DecipherOnly|şifre çözme| Yok  |
|DigitalSignature|oturum, doğrulayın| Sertifika oluşturma zamanında bir kullanım belirtimi olmadan bir key Vault varsayılan | 
|EncipherOnly|şifrele| Yok |
|KeyCertSign|oturum, doğrulayın|Yok|
|KeyEncipherment|wrapKey, unwrapKey| Sertifika oluşturma zamanında bir kullanım belirtimi olmadan bir key Vault varsayılan | 
|İnkar edilemez|oturum, doğrulayın| Yok |
|crlsign|oturum, doğrulayın| Yok |

### <a name="certificate-issuer"></a>Sertifikayı veren

Bir Key Vault sertifikası nesnesi sipariş x509 sertifikaları için seçilen sertifika veren sağlayıcısıyla iletişim kurmak için kullanılan bir yapılandırma tutar.  

-   Key Vault, sertifika veren sağlayıcıları SSL sertifikaları için aşağıdaki ile iş ortakları

|**Sağlayıcı adı**|**Konumlar**|
|----------|--------|
|DigiCert|Desteklenen, genel Bulut ve Azure kamu tüm anahtar kasası hizmet konumu|
|GlobalSign|Desteklenen, genel Bulut ve Azure kamu tüm anahtar kasası hizmet konumu|

Sertifikayı veren bir anahtar Kasası'nda oluşturulabilmesi için önce aşağıdaki önkoşul adımları 1 ve 2 başarıyla gerçekleştirilmesi gerekir.  

1. Sertifika yetkilisi (CA) sağlayıcıları ekleme  

    -   Bir kuruluş yöneticisi yerleşik olmalıdır (ör şirket. Contoso) en az bir CA sağlayıcısıyla yayımlayabilirsiniz.  

2. İstek sahibinin kimlik bilgilerini kaydetme (ve yenileme) SSL sertifikaları Key Vault için yönetici oluşturur  

    -   Yapılandırma Sağlayıcısı'nın veren nesne anahtar kasasını oluşturmak için kullanılacak sağlar  

Sertifikaları Portalı'ndan veren nesneleri oluşturma hakkında daha fazla bilgi için bkz. [Key Vault Certificates blogu](https://aka.ms/kvcertsblog)  

Key Vault farklı veren sağlayıcı yapılandırması ile birden çok veren nesneleri oluşturulmasını sağlar. Veren nesne oluşturulduktan sonra bir veya birden çok sertifika ilkeleri adı başvurulabilir. Veren nesneye başvuran bildirir x509 isterken veren nesne belirtilen yapılandırma kullanmak için Key Vault sertifika oluşturma ve yenileme sırasında CA sağlayıcısından sertifika.  

Veren nesneleri kasaya oluşturulur ve yalnızca aynı kasaya KV sertifikaları ile kullanılabilir.  

### <a name="certificate-contacts"></a>Sertifika kişileri

Sertifika kişileri sertifika ömrü olaylar tarafından tetiklenen bildirimleri göndermek için kişi bilgilerini içerir. Kişi bilgileri tarafından tüm sertifikalar anahtar kasasında paylaşılır. Anahtar Kasası'nda herhangi bir sertifika için bir olay için belirtilen tüm kişilere bildirim gönderilir.  

Ardından bir sertifika ilkesi için otomatik yenileme ayarlarsanız, aşağıdaki olaylar üzerinde bir bildirim gönderilir.  

- Önce sertifika yenileme
- Sertifika başarıyla yenilendi belirten veya bir hata varsa, sertifikanın el ile yenileme gerekliliği sertifika yenileme sonra.  

  (E-posta yalnızca) el ile olarak ayarlanmış bir Sertifika İlkesi yenilendiğinde, sertifikayı yenilemek için saat olduğunda bir bildirim gönderilir.  

### <a name="certificate-access-control"></a>Sertifika erişim denetimi

 Erişim denetimi sertifikalar için Key Vault tarafından yönetildiğinden ve içeren bu sertifikaları Key Vault tarafından sağlanır. Sertifikalar için erişim denetimi İlkesi, anahtarları ve gizli anahtarları aynı anahtar Kasası'nda için erişim denetim ilkeleri'nden farklıdır. Kullanıcılar senaryoya uygun segmentlere ayırma ve yönetim sertifikaları sağlamak için sertifikaları tutmak için bir veya daha fazla kasalarını oluşturabilir.  

 Bir asıl başına temelinde, gizli dizileri erişim denetim girişi bir key vault ile yakından yansıtmalar bir gizli dizi nesnesinde izin verilen işlemleri üzerinde aşağıdaki izinlere kullanılabilir:  

- Sertifika yönetimi işlemleri için izinleri
  - *Alma*: Geçerli sertifika sürümü veya herhangi bir sürümünü bir sertifika alın 
  - *Liste*: Geçerli bir sertifika veya sertifika sürümleri listesi  
  - *Güncelleştirme*: Bir sertifikayı güncelleştirmek
  - *Oluşturma*: Bir anahtar kasası sertifikası oluşturma
  - *İçeri aktarma*: Sertifika malzemeleri bir Key Vault sertifikayı içeri aktarma
  - *Silme*: Bir sertifikanın, ilke ve tüm sürümlerini Sil  
  - *Kurtarma*: Silinen bir sertifika Kurtar
  - *Yedekleme*: Sertifikayı key vault'ta yedekleme
  - *Geri yükleme*: Bir anahtar kasasına bir yedeklediğiniz sertifikayı geri
  - *managecontacts*: Key Vault sertifika kişileri Yönet  
  - *manageissuers*: Key Vault sertifika yetkilileri/verenler yönetme
  - *getissuers*: Bir sertifika yetkilileri/verenler Al
  - *listissuers*: Bir sertifika yetkilileri/verenler listesi  
  - *setissuers*: Bir Key Vault sertifika yetkilileri/verenler güncelle  
  - *deleteissuers*: Bir Key Vault sertifika yetkilileri/verenler Sil  
 
- Ayrıcalıklı işlemleri için izinleri
  - *Temizleme*: Temizle (kalıcı) bir sertifika silindi

Daha fazla bilgi için [sertifika anahtar kasası REST API Başvurusu'ndaki işlemleri](/rest/api/keyvault). İzinler oluşturma hakkında daha fazla bilgi için bkz: [kasaları - oluştur veya Güncelleştir](/rest/api/keyvault/vaults/createorupdate) ve [kasaları - güncelleştirme erişim ilkesi](/rest/api/keyvault/vaults/updateaccesspolicy).

## <a name="azure-storage-account-key-management"></a>Azure depolama hesabı anahtarı yönetimi

Key Vault, Azure depolama hesabı anahtarlarını yönetebilirsiniz:

- Dahili olarak, anahtar kasası (eşitleme) Azure depolama hesabı anahtarları listeleyebilirsiniz. 
- Key Vault oluşturur (döndürür) anahtarlarını düzenli aralıklarla.
- Anahtar değerler hiçbir zaman yanıt arayana döndürülür.
- Depolama hesapları hem de klasik depolama hesabı anahtarlarını Key Vault yönetir.

Daha fazla bilgi için [Azure Key Vault depolama hesabı anahtarları](key-vault-ovw-storage-keys.md)

### <a name="storage-account-access-control"></a>Depolama hesabı erişim denetimi

Aşağıdaki izinleri, kullanıcı veya uygulama asıl yetkilendirirken yönetilen bir depolama hesabı üzerinde işlem gerçekleştirmek için kullanılabilir:  

- Yönetilen depolama hesabı ve SaS tanımı işlemleri için izinleri
  - *Alma*: Bir depolama hesabı bilgilerini alır 
  - *Liste*: Key Vault tarafından yönetilen depolama hesapları listeleme
  - *Güncelleştirme*: Bir depolama hesabı güncelleştirme
  - *Silme*: Bir depolama hesabını silme  
  - *Kurtarma*: Silinen bir depolama hesabı Kurtar
  - *Yedekleme*: Bir depolama hesabı yedekleme
  - *Geri yükleme*: Bir yedekleme depolama hesabı, bir anahtar Kasası'na geri yükleme
  - *ayarlama*: Bir depolama hesabı güncelle
  - *üretir*: Bir depolama hesabı için belirtilen bir anahtar değeri yeniden oluştur
  - *getsas*: Bir depolama hesabı için bir SAS tanımı hakkında bilgi alın
  - *listsas*: Bir depolama hesabı için depolama SAS tanımları listesi
  - *deletesas*: Bir depolama hesabından bir SAS tanımını sil
  - *setsas*: Bir yeni SAS tanımı/öznitelikler için bir depolama hesabı güncelle

- Ayrıcalıklı işlemleri için izinleri
  - *Temizleme*: Temizle (kalıcı) yönetilen bir depolama hesabı

Daha fazla bilgi için [depolama hesabı anahtar kasası REST API Başvurusu işlemlerinde](/rest/api/keyvault). İzinler oluşturma hakkında daha fazla bilgi için bkz: [kasaları - oluştur veya Güncelleştir](/rest/api/keyvault/vaults/createorupdate) ve [kasaları - güncelleştirme erişim ilkesi](/rest/api/keyvault/vaults/updateaccesspolicy).

## <a name="see-also"></a>Ayrıca Bkz.

- [Kimlik doğrulama istekleri ve yanıtları](authentication-requests-and-responses.md)
- [Key Vault Geliştirici Kılavuzu](/azure/key-vault/key-vault-developers-guide)
