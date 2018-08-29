---
title: Anahtarlar, gizli diziler ve sertifikalar hakkında
description: REST arabirimi ve KV Geliştirici Ayrıntıları'na genel bakış
services: key-vault
documentationcenter: ''
author: BryanLa
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: abd1b743-1d58-413f-afc1-d08ebf93828a
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2018
ms.author: bryanla
ms.openlocfilehash: f36e0e3ddc605d960ed764252308cbf09578832c
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2018
ms.locfileid: "43126151"
---
# <a name="about-keys-secrets-and-certificates"></a>Anahtarlar, parolalar ve sertifikalar hakkında
Azure Key Vault, depolamak ve Microsoft Azure ortamında şifreleme anahtarlarını kullanmak kullanıcıların sağlar. Key Vault, birden çok anahtar türleri ve algoritmalarını destekler ve yüksek değerli anahtarlar donanım güvenlik modülleri (HSM) kullanılmasına olanak tanır. Ayrıca, Key Vault gizli dizileri güvenli bir şekilde depolamak kullanıcıların sağlar. Gizli dizileri boyutlarının sekizli hiçbir belirli semantikler nesneleridir. Key Vault, anahtarları ve gizli anahtarları üzerine yapılandırılmıştır ve otomatik yenileme özelliğini ekleyin sertifikalarını da destekler.

Azure Key Vault hakkında daha fazla genel bilgi için bkz. [Azure anahtar kasası nedir?](/azure/key-vault/key-vault-whatis)

**Key Vault genel ayrıntıları**

-   [Standartlarını destekleyen](#BKMK_Standards)
-   [Veri türleri](#BKMK_DataTypes)  
-   [Nesne tanımlayıcıları ve sürüm oluşturma](#BKMK_ObjId)  

**Anahtarlar hakkında**

-   [Anahtarları ve anahtar türleri](#BKMK_KeyTypes)  
-   [RSA algoritmalarını](#BKMK_RSAAlgorithms)  
-   [RSA HSM algoritmaları](#BKMK_RSA-HSMAlgorithms)  
-   [Şifreleme koruma](#BKMK_Cryptographic)
-   [Anahtar işlemleri](#BKMK_KeyOperations)  
-   [Anahtar öznitelikleri](#BKMK_KeyAttributes)  
-   [Anahtarını etiketleri](#BKMK_Keytags)  

**Gizli dizileri hakkında** 

-   [Gizli anahtarlarla çalışma](#BKMK_WorkingWithSecrets)  
-   [Gizli dizi öznitelikleri](#BKMK_SecretAttrs)  
-   [Gizli etiketleri](#BKMK_SecretTags)  
-   [Gizli anahtar erişim denetimi](#BKMK_SecretAccessControl)  

**Sertifikalar hakkında**

-   [Sertifika oluşturma](#BKMK_CompositionOfCertificate)  
-   [Sertifika öznitelikler ve etiketler](#BKMK_CertificateAttributesAndTags)  
-   [Sertifika İlkesi](#BKMK_CertificatePolicy)  
-   [Sertifikayı veren](#BKMK_CertificateIssuer)  
-   [Sertifika kişileri](#BKMK_CertificateContacts)  
-   [Sertifika erişim denetimi](#BKMK_CertificateAccessControl)  

--

## <a name="key-vault-general-details"></a>Key Vault genel ayrıntıları

Aşağıdaki bölümlerde, Azure Key Vault hizmeti, uygulama ilgili genel bilgiler sunar.

###  <a name="BKMK_Standards"></a> Standartlarını destekleyen

JavaScript nesne gösterimi (JSON) ve JavaScript nesne imzalama ve şifreleme (JOSE) belirtimleri önemli bilgiler var.  

-   [JSON Web anahtarı (JWK)](http://tools.ietf.org/html/draft-ietf-jose-json-web-key)  
-   [JSON Web şifreleme (jwe ile)](http://tools.ietf.org/html/draft-ietf-jose-json-web-encryption)  
-   [JSON Web algoritmalar (JWA)](http://tools.ietf.org/html/draft-ietf-jose-json-web-algorithms)  
-   [JSON Web imza (JWS)](http://tools.ietf.org/html/draft-ietf-jose-json-web-signature)  

### <a name="BKMK_DataTypes"></a> veri türleri

Başvurmak [JOSE belirtimleri](#BKMK_Standards) anahtarlarını, şifreleme ve imzalama için ilgili veri türleri için.  

-   **algoritma** -anahtar işlemi, örneğin, RSA1_5 için desteklenen bir algoritması  
-   **ciphertext değerli** -metin sekizlik tabanda Base64URL kullanılarak kodlanır, şifre  
-   **Özet-değer** -Base64URL kullanılarak kodlanan bir karma algoritması çıktısı  
-   **anahtar türü** -desteklenen anahtar türleri, örneğin RSA biri.  
-   **düz metin değeri** -Base64URL kullanılarak kodlanır, düz metin sekizli  
-   **İmza değeri** - çıkış Base64URL kullanılarak kodlanan bir imza algoritması  
-   **base64URL** -bir Base64URL [RFC4648] kodlanmış ikili değer  
-   **Boole** -true veya false  
-   **Kimlik** - bir kimlik gelen Azure Active Directory (AAD).  
-   **IntDate** - 1970'ten saniye sayısını temsil eden bir JSON ondalık değeri-01-kadar belirtilen UTC tarihi/saati UTC 01T0:0:0Z. Tarih ile ilgili ayrıntılar için bkz. [RFC3339] / genel saatleri ve özellikle UTC.  

###  <a name="BKMK_ObjId"></a> Nesne tanımlayıcıları ve sürüm oluşturma

Azure anahtar Kasası'nda depolanan nesnelere sürümleri, yeni bir örneğini bir nesne oluşturulur ve her sürümde bir benzersiz tanımlayıcı ve URL korur. Bir nesne ilk oluşturulduğunda benzersiz sürüm tanımlayıcısı verilir ve nesnenin geçerli sürümü olarak işaretlenir. Aynı nesne adı ile yeni bir örneğinin oluşturulmasını benzersiz sürüm tanımlayıcısı yeni nesne sağlar ve geçerli sürüme duruma neden olur.  

Azure anahtar Kasası'nda nesneleri geçerli tanımlayıcısı veya bir sürüme özgü tanımlayıcısı kullanarak çözülebilir. Örneğin, "MasterKey" ada sahip bir anahtar göz önünde bulundurulduğunda, geçerli bir tanımlayıcı işlemler gerçekleştirme sistemi en son sürüme neden olur. Sürüme özgü tanımlayıcıyı işlemler gerçekleştirme, nesne belirli bir sürümünü kullanmak sistemi neden olur.  

Nesneler iki nesne sisteminde, coğrafi konum, bağımsız olarak aynı URL'ye sahip olacak bir URL kullanarak Azure Key Vault içinde benzersiz şekilde tanımlanır. Tam URL bir nesne için nesne tanımlayıcısı olarak adlandırılır ve Key Vault, nesne türünü tanımlayan bir önek bölümü oluşur, bir kullanıcı nesne adı ve bir nesne sürümü sağlandı. Nesne adı büyük/küçük harfe ve değişmez. Nesne sürümü içermeyen tanımlayıcıları temel tanımlayıcı olarak adlandırılır.  

Daha fazla bilgi için [kimlik doğrulaması, istekleri ve yanıtları](authentication-requests-and-responses.md)

Bir nesne tanımlayıcı genel biçimi aşağıdaki gibidir:  

`https://{keyvault-name}.vault.azure.net/{object-type}/{object-name}/{object-version}`  

Konumlar:  

|||  
|-|-|  
|`keyvault-name`|Microsoft Azure anahtar kasası hizmetindeki bir anahtar kasası adı.<br /><br /> Key Vault adları, kullanıcı tarafından seçilen ve genel olarak benzersiz.<br /><br /> Anahtar Kasası adı, 3-24 karakter uzunluğundaki bir dize olmalı ve yalnızca 0-9, a-z, A-Z ve - karakterlerini içermelidir.|  
|`object-type`|"Anahtarlar" veya "gizli" nesnenin türü.|  
|`object-name`|Bir `object-name` için bir kullanıcı tarafından sağlanan ad ve bir Key Vault içinde benzersiz olmalıdır. Adı bir dize 1-127 olmalıdır karakter uzunluğunda içeren yalnızca 0-9, a-z, A-Z ve -.|  
|`object-version`|Bir `object-version` olduğu bir sistem tarafından oluşturulan, isteğe bağlı olarak bir nesnenin benzersiz bir sürüm adresi için kullanılan 32 karakterli dize tanımlayıcı.|  

## <a name="key-vault-keys"></a>Key Vault anahtarları

###  <a name="BKMK_KeyTypes"></a> Anahtarları ve anahtar türleri

Azure Key vault'taki şifreleme anahtarları, JSON Web anahtarı [JWK] nesneler olarak temsil edilir. Temel belirtimler JWK/JWA ayrıca Azure anahtar kasası uygulama için benzersiz anahtar türleri etkinleştirmek için genişletilir, örneğin güvenli nakliyatı etkinleştirmek için HSM satıcı (Thales) belirli paketleme kullanarak Azure Key Vault için anahtar alma gibi anahtarları, Bunlar, yalnızca Azure anahtar kasası Hsm'lerine içinde kullanılabilir.  

- **"Soft" anahtarları**: bir anahtar, anahtar kasası tarafından yazılımda işlenir ancak bir HSM'de bir sistem anahtarı kullanılarak, bekleme sırasında şifrelenir. İstemciler mevcut bir RSA veya EC anahtarını içeri aktarın veya Azure anahtar kasası bir oluşturma isteği.
- **"Sabit" anahtarları**: anahtar bir HSM (donanım güvenlik modülü) işlenir. Bu anahtarları Azure Key Vault HSM güvenlik Dünyaları birinde korunur (yalıtım sağlamak için bir güvenlik Dünyası Coğrafya başına yoktur). İstemciler bir RSA veya EC anahtarı geçici bir form veya uyumlu bir HSM CİHAZDAN vererek alma veya Azure anahtar kasası bir oluşturma isteği. Bu anahtar türü T öznitelik ekler HSM anahtar malzemesi yürütmek için için JWK edinin.

     Coğrafi sınırlar hakkında daha fazla bilgi için bkz. [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/privacy/)  

RSA ve Eliptik Eğri anahtarlar yalnızca Azure Key Vault destekler. gelecek sürümlerde destekleyebilir diğer anahtar türleri gibi simetrik.

-   **EC**: "Soft" Eliptik Eğri anahtar.
-   **HSM EC**: "Sabit" Eliptik Eğri anahtar.
-   **RSA**: "Soft" RSA anahtarı.
-   **RSA HSM**: "Sabit" RSA anahtarı.

Azure Key Vault RSA anahtarları 2048, 3072 ve 4096 boyutlarını destekler ve Eliptik Eğri anahtarlarını yazın P-256, p-384, p-521 ve P-256_K.

### <a name="BKMK_Cryptographic"></a> Şifreleme koruma

Azure anahtar Kasası'nı kullandığından, şifreleme modüllerini HSM veya yazılım, FIPS doğrulanmış olduğunu belirtir. FIPS modunda çalışacak şekilde özel herhangi bir şey yapmanız gerekmez. Varsa, **oluşturma** veya **alma** olarak HSM korumalı anahtarlar, bunlar garanti edilir HSM'ler, FIPS 140-2 Düzey 2 veya daha yüksek doğrulanmış içinde işlenecek. Varsa, **oluşturma** veya **alma** anahtarı yazılım korumalı içinde şifreleme modüllerine işlenene sonra olarak doğrulanmış FIPS 140-2 Düzey 1 veya daha yüksek. Daha fazla bilgi için [anahtarları ve anahtar türleri](#BKMK_KeyTypes).

###  <a name="BKMK_ECAlgorithms"></a> EC algoritmaları
 Aşağıdaki algoritması tanımlayıcıları EC ve EC-HSM anahtarları Azure Key vault'ta desteklenir. 

#### <a name="signverify"></a>OTURUM VE DOĞRULAYIN

-   **ES256** - ECDSA SHA-256 özetleyen ve anahtarları eğri p-256 ile oluşturulur. Bu algoritma [RFC7518] açıklanmıştır.
-   **ES256K** - ECDSA SHA-256 özetleyen ve anahtarları P-256_K eğri ile oluşturulur. Standardizasyon algoritmasıdır.
-   **ES384** - ECDSA için SHA-384 özetleyen ve anahtarları p-384 eğri ile oluşturulur. Bu algoritma [RFC7518] açıklanmıştır.
-   **ES512** - ECDSA için SHA-512 özetleyen ve anahtarları, p-521 eğrisini ile oluşturulur. Bu algoritma [RFC7518] açıklanmıştır.

###  <a name="BKMK_RSAAlgorithms"></a> RSA algoritmalarını  
 RSA ve RSA HSM anahtarları Azure Key vault'ta aşağıdaki algoritması tanımlayıcılar desteklenir.  

#### <a name="wrapkeyunwrapkey-encryptdecrypt"></a>/ UNWRAPKEY, WRAPKEY ŞİFRELEME/ŞİFRE ÇÖZME

-   **RSA1_5** -RSAES PKCS1 V1_5 [RFC3447] anahtar şifreleme  
-   **RSA OAEP** - en iyi asimetrik şifreleme (OAEP) [RFC3447] bölümü A.2.1, RFC 3447 tarafından belirtilen varsayılan parametreleri olan doldurma kullanarak RSAES. Bu varsayılan parametreleri, SHA-1 ile SHA-1 karma işlevi ve MGF1 maskesi oluşturma işlevi kullanıyor.  

#### <a name="signverify"></a>OTURUM VE DOĞRULAYIN

-   **RS256** - RSASSA-PKCS-v1_5 SHA-256'yı kullanarak. Sağlanan uygulama Özet değeri SHA-256 kullanılarak hesaplanması gerekir ve uzunluğu 32 bayt olması gerekir.  
-   **RS384** - RSASSA-PKCS-v1_5 SHA-384 kullanarak. Sağlanan uygulama Özet değeri SHA-384'ı kullanarak hesaplanan gerekir ve 48 bayt uzunluğunda olmalıdır.  
-   **RS512** - RSASSA-PKCS-v1_5 SHA-512 kullanarak. Sağlanan uygulama Özet değeri SHA-512'ı kullanarak hesaplanan olmalıdır ve uzunluğu 64 baytın olmalıdır.  
-   **RSNULL** -bakın [RFC2437], bir özel kullanım belirli TLS senaryoları etkinleştirmek için örneği.  

###  <a name="BKMK_KeyOperations"></a> Anahtar işlemleri

Azure Key Vault anahtar nesneler üzerinde aşağıdaki işlemleri destekler:  

-   **Oluşturma**: Azure anahtar Kasası'nda bir anahtar oluşturmak bir istemci sağlar. Anahtar değerini Azure Key Vault tarafından oluşturulan ve depolanan ve istemciye serbest bırakılmaz. Asimetrik (ve gelecekte Eliptik Eğri ve simetrik) anahtar Azure anahtar Kasası'nda oluşturulabilir.  
-   **İçeri aktarma**: var olan bir anahtar Azure anahtar Kasası'na içeri aktarmak bir istemci sağlar. Asimetrik (ve gelecekte Eliptik Eğri ve simetrik) anahtarları farklı paketleme yöntemler JWK yapısı içinde bir dizi kullanarak Azure Key Vault için aktarılabilir.  
-   **Güncelleştirme**: daha önce Azure Key Vault içinde depolanan bir anahtar ile ilişkili meta verileri (anahtar öznitelikleri) değiştirmek için yeterli izinlere sahip bir istemci sağlar.  
-   **Silme**: Azure Key Vault'tan bir anahtar silmek için yeterli izinlere sahip bir istemci sağlar.  
-   **Liste**: bir istemci belirli bir Azure anahtar Kasası'nda tüm anahtarların listelenmesi sağlar.  
-   **Sürümleri Listele**: tüm belirli bir Azure anahtar Kasası'nda belirli bir anahtarın sürümlerini listeleme istemciye sağlar.  
-   **Alma**: genel bir Azure anahtar Kasası'nda belirli bir anahtarın bölümlerini almak bir istemci sağlar.  
-   **Yedekleme**: korumalı bir formda bir anahtar verir.  
-   **Geri yükleme**: daha önce yedeklenen bir anahtarı içeri aktarır.  

Daha fazla bilgi için [anahtarı anahtar kasası REST API Başvurusu işlemlerinde](/rest/api/keyvault).  

Bir anahtar Azure anahtar Kasası'nda oluşturulduktan sonra aşağıdaki şifreleme işlemleri anahtar kullanılarak gerçekleştirilebilir:  

-   **Oturum ve Doğrula**: Azure anahtar kasası içeriğini imza oluşturmanın bir parçası karma desteklemediğinden kesinlikle, bu işlem "imza karması" veya "karma doğrulama" olur. Uygulamaları, verileri yerel olarak oturum açmanız ve sonra Azure Key Vault oturum karma istek karma. İmzalı karmalarının doğrulama [genel] anahtar malzemesi için erişiminiz olmayabilir uygulamalar için bir kolaylık işlem olarak desteklenir. Biz öneririz için en iyi uygulama performansı, yerel olarak gerçekleştirilen işlemler doğrulayın.  
-   **Anahtar şifreleme / kaydırma**: Azure Key Vault'ta depolanan bir anahtar, başka bir anahtar, bir simetrik içerik şifreleme anahtarı (CEK) genellikle korumak için kullanılabilir. Azure Key Vault anahtarı asimetrik olduğunda, anahtar şifreleme kullanılır, örneğin RSA OAEP ve WRAPKEY/UNWRAPKEY işlemleri için şifreleme/şifre çözme eşdeğerdir. Anahtar Azure anahtar Kasası'nda simetrik anahtarı sarmalama kullanılır; Örneğin AES-KW. [Genel] anahtar malzemesi için erişiminiz olmayabilir uygulamalar için bir kolaylık olarak WRAPKEY işlemi desteklenmez. en iyi uygulama performansını WRAPKEY işlemler yerel olarak gerçekleştirilir, önerilir.  
-   **Şifreleme ve şifre çözme**: Azure Key Vault'ta depolanan bir anahtar şifreleme veya tek bir blok boyutu, seçilen şifreleme algoritması ve anahtar türü tarafından belirlenir verilerin şifresini çözmek için kullanılabilir. Şifreleme işlemi [genel] anahtar malzemesi için erişiminiz olmayabilir uygulamalar için kolaylık sağlamak için sağlanır; Bu, en iyi uygulama performansını önerilir, şifreleme işlemlerini yerel olarak gerçekleştirilir.  

WRAPKEY/UNWRAPKEY asimetrik anahtarlarla şifreleme/şifre çözme işlemi eşdeğerdir gibi gereksiz görünebilir, ancak ayrı operasyon kullanımını sağlamak önemli anlamsal olarak kabul edilir ve bu işlemleri ve tutarlılık yetkilendirme ayrımı ne zaman diğer anahtar türleri, hizmet tarafından desteklenir.  

Azure Key Vault, dışa aktarma işlemleri desteklemiyor: bir anahtar sağlandıktan sonra sistemde ayıklanamıyor veya kendi anahtar malzemesi değiştirdi.  Ancak, kullanıcılar Azure anahtar kasası kullanan diğer için kendi anahtarını gerektirebilir kullanım veya Azure anahtar Kasası'nda silindikten sonra korumalı bir form anahtarı içeri/dışarı aktarma için yedekleme ve geri yükleme işlemleri kullanabilir. Yedekleme işlemi tarafından oluşturulan anahtarları Azure Key Vault dışında kullanılamaz. Alternatif olarak, içeri aktarma işlemi birden çok Azure Key Vault örneği karşı kullanılabilir.  

Kullanıcılar, herhangi bir Azure Key Vault JWK key_ops özelliğini kullanarak anahtarı başına temelinde destekleyen şifreleme işlemlerini kısıtlayabilirsiniz.  

JWK nesneleri hakkında daha fazla bilgi için bkz. [JSON Web anahtarı (JWK)](http://tools.ietf.org/html/draft-ietf-jose-json-web-key).  

###  <a name="BKMK_KeyAttributes"></a> Anahtar öznitelikleri

Anahtar malzemesi ek olarak, aşağıdaki öznitelikleri belirtilebilir. Bir JSON isteği, öznitelikleri anahtar sözcüğü ve küme ayraçları, ' {' '}', belirtilen öznitelikleri olsa bile gereklidir.  

- *Etkin*: Boole, isteğe bağlı, varsayılan **true**. Anahtarın etkin ve şifreleme işlemleri için kullanılabilir olup olmadığını belirtir. *Etkin* özniteliği ile birlikte kullanılır *nbf* ve *exp*. Bir işlemi oluştuğunda arasında *nbf* ve *exp*, varsa yalnızca verilmez *etkin* ayarlanır **true**. İşlemler dışındaki *nbf* / *exp* penceresi otomatik olarak izin verilmez, belirli işlem türleri altında dışında [belirli koşullar](#BKMK_key-date-time-ctrld-ops).
- *NBF*: IntDate, isteğe bağlı, varsayılan sunuldu. *Nbf* (değil önce) öznitelik tanımlar, önce anahtarı değil kullanılmalıdır altında belirli işlem türlerini hariç şifreleme işlemleri için zaman [belirli koşullar](#BKMK_key-date-time-ctrld-ops). İşlenmesini *nbf* özniteliği gerektirir geçerli tarih/saat sonra olmanız gerekir veya eşit olmayan-listelenen tarih/saat önce *nbf* özniteliği. Azure Key Vault için saat eğriltme hesap için genellikle en fazla birkaç dakika bazı küçük leeway için sağlayabilir. Değerini IntDate değerini içeren bir sayı olmalıdır.  
- *exp*: IntDate, isteğe bağlı, varsayılan değer "sonsuz". *Exp* (süre) öznitelik tanımlar ve sonrasında, bu anahtar gerekir altında belirli işlem türlerini hariç şifreleme işlemi için kullanılan süre sonu zamanı [belirli koşullar](#BKMK_key-date-time-ctrld-ops). İşlenmesini *exp* özniteliği gerektirir süre sonu tarihi/saati listelenen önce geçerli tarih/saat olmalıdır *exp* özniteliği. Azure Key Vault için saat eğriltme hesap için genellikle en fazla birkaç dakika bazı küçük leeway için sağlayabilir. Değerini IntDate değerini içeren bir sayı olmalıdır.  

Anahtar öznitelikleri içeren bir yanıta dahil ek salt okunur özniteliği vardır:  

- *oluşturulan*: IntDate, isteğe bağlı. *Oluşturulan* öznitelik, bu anahtarın sürümü ne zaman oluşturulduğunu gösterir. Bu değer, bu öznitelik eklenmesini önce oluşturulan anahtarları için null. Değerini IntDate değerini içeren bir sayı olmalıdır.  
- *güncelleştirilmiş*: IntDate, isteğe bağlı. *Güncelleştirilmiş* öznitelik, bu anahtarın sürümü ne zaman güncelleştirildiği gösterir. Bu değer, bu öznitelik eklenmesini önce en son güncelleştirildiği anahtarları için null. Değerini IntDate değerini içeren bir sayı olmalıdır.  

IntDate ve diğer veri türleri hakkında daha fazla bilgi için bkz. [veri türleri](#BKMK_DataTypes)  

#### <a name="BKMK_key-date-time-ctrld-ops"></a> Tarih-saat denetimli işlemleri

Not henüz geçerli ve süresi dolan anahtarlar, bu dış *nbf* / *exp* penceresinde için çalışır **şifresini**, **sarmalamadan çıkarma** ve **doğrulayın** işlemleri (403, iade olmaz Yasak). Stratejinin değil ancak geçerli durum kullanmak için üretim kullanılmadan önce test edilecek bir anahtar izin vermektir. Süresi dolmuş kullanmaya yönelik bir stratejinin anahtarının geçerli olduğu zaman, oluşturulan veri kurtarma işlemleri sağlamaktır. Key Vault ilkelerini kullanarak bir anahtar veya güncelleştirerek erişimini devre dışı bırakabilirsiniz ayrıca *etkin* anahtar özniteliğiyle **false**.

Veriler üzerinde daha fazla bilgi için bkz: türleri, [veri türleri](#BKMK_DataTypes).

Diğer olası öznitelikleri hakkında daha fazla bilgi için bkz. [JSON Web anahtarı (JWK)](http://tools.ietf.org/html/draft-ietf-jose-json-web-key).

### <a name="BKMK_Keytags"></a> Anahtarını etiketleri

Ek uygulamaya özgü meta veri etiketleri biçiminde belirtebilirsiniz. Azure Key Vault, en fazla 15 etiket, her biri olan 256 karakterlik bir ad ve 256 karakter değeri destekler.  

>[!Note]
>Bunlar varsa etiketleri çağıran tarafından okunabilir *listesi* veya *alma* söz konusu nesne türü; izni anahtarları, parola veya sertifikalara.

###  <a name="BKMK_KeyAccessControl"></a> Anahtar erişim denetimi

Key Vault tarafından yönetilen anahtarlar için erişim denetimi, anahtar kapsayıcısı olarak davranan bir Key Vault düzeyinde sağlanır. Anahtarları için erişim denetimi İlkesi aynı Key Vault'ta ' gizli diziler için farklı bir erişim denetimi İlkesi yoktur. Kullanıcılar, anahtarları tutmak için bir veya daha fazla kasalarını oluşturabilir ve senaryoya uygun segmentlere ayırma ve anahtar yönetimi korumak için gereklidir. Anahtarlar için erişim denetimi, gizli dizileri için erişim denetimi bağımsızdır.  

Aşağıdaki izinleri, üzerinde verilebilecek bir kullanıcı / hizmet sorumlusu aralıklarla bir kasasındaki anahtarları erişim denetim girişi. Bu izinleri bir anahtar nesnesi üzerinde izin verilen işlemleri yakından yansıtabilirsiniz:  

-   *oluşturma*: yeni anahtarları oluşturma
-   *Alma*: bir anahtar yanı sıra, özniteliklerini ortak bölümünü okuyun
-   *Liste*: anahtar kasasında depolanan bir anahtarın sürümlerini ve anahtarlar listesi
-   *Güncelleştirme*: anahtarı için öznitelikleri güncelleştir
-   *silme*: anahtar nesnesini silme
-   *oturum*: özetleyen imzalamak için anahtar kullanımı
-   *doğrulama*: anahtar özetler doğrulamak için kullanılır
-   *wrapKey*: anahtar bir simetrik anahtar korumak için kullanılır
-   *unwrapKey*: korumasını kaldırmak için anahtar kullanımı sarmalanmış simetrik anahtarlar
-   *şifreleme*: anahtar bir rastgele bayt dizisini korumak için kullanılır
-   *şifre çözme*: bayt dizisini korumasını kaldırmak için kullanma
-   *içeri aktarma*: bir anahtar kasasına bir anahtar aktarma
-   *Yedekleme*: anahtar kasasında bir anahtarı yedekleme
-   *geri yükleme*: yedeklenen bir key vault için anahtarı geri yükleme
-   *tüm*: tüm izinler

## <a name="key-vault-secrets"></a>Key Vault gizli dizileri 

###  <a name="BKMK_WorkingWithSecrets"></a> Gizli anahtarlarla çalışma

Azure Key Vault gizli dizileri en fazla 25 k bayt her sekizli dizileri var. Azure Key Vault hizmeti, gizli dizileri için tüm semantikler sağlamaz; yalnızca verileri kabul eder, şifreler ve bunu bir gizli dizi tanımlayıcısı, "gizli daha sonraki bir zamanda almak için kullanılabilir kimliği" döndüren depolar.  

Yüksek oranda gizli veriler için ek Azure Key Vault'ta depolanan veriler için koruma katmanları istemciler düşünmelisiniz; Örneğin önceden verileri şifreleyerek ayrı koruma anahtarı kullanma.  

Azure Key Vault, ayrıca bir contentType alanı için gizli dizilerini destekler. İstemciler, içerik türü "contentType" gizli verileri yorumlama alındığı sırada yardımcı olmak üzere gizli dizi belirtebilirsiniz. Bu alan en fazla uzunluğu 255 karakterdir. Önceden tanımlanmış değerler yoktur. Önerilen kullanım için gizli verileri yorumlama ipucu gibidir. Örneğin, bir uygulama gizli dizi olarak hem parola hem de sertifikaları depolamak ardından belirtmek için bu alanı kullanın. Önceden tanımlanmış değerler yoktur.  

###  <a name="BKMK_SecretAttrs"></a> Gizli dizi öznitelikleri

Gizli veriler ek olarak, aşağıdaki öznitelikleri belirtilebilir:  

- *exp*: IntDate, isteğe bağlı, varsayılan **sonsuza kadar**. *Exp* (süre) öznitelik tanımlar ve sonrasında, gizli verilerin alınması GEREKTİĞİNİ değil, sona erme zamanı hariç [belirli durumlarda](#BKMK_secret-date-time-ctrld-ops). Bu alan için olan **bilgilendirici** yalnızca belirli bir gizli dizi kullanılamaz anahtar kasası hizmeti, kullanıcıları bilgilendirir gibi amacıyla. Değerini IntDate değerini içeren bir sayı olmalıdır.   
- *NBF*: IntDate, isteğe bağlı, varsayılan **artık**. *Nbf* (değil önce) öznitelik tanımlar önüne gizli verileri SecurityDescriptor, zaman hariç [belirli durumlarda](#BKMK_secret-date-time-ctrld-ops). Bu alan için olan **bilgilendirici** yalnızca amacıyla. Değerini IntDate değerini içeren bir sayı olmalıdır. 
- *Etkin*: Boole, isteğe bağlı, varsayılan **true**. Bu öznitelik, gizli veriler alınabilir olup olmadığını belirtir. Etkinleştirilmiş bir öznitelik ile birlikte kullanılır ve *exp* arasında bir işlem olduğunda oluşur ve bu exp, yalnızca etkinleştirilirse ayarlanır **true**. İşlemler dışındaki *nbf* ve *exp* penceresi olan otomatik olarak izin verilmeyen, içinde hariç [belirli durumlarda](#BKMK_secret-date-time-ctrld-ops).  

Gizli dizi öznitelikleri içeren bir yanıta dahil ek salt okunur özniteliği vardır:  

- *oluşturulan*: IntDate, isteğe bağlı. Oluşturulan öznitelik, bu gizli dizi sürümü ne zaman oluşturulduğunu gösterir. Bu değer, bu öznitelik eklenmesini önce oluşturulan gizli dizileri için null. Değerini IntDate değerini içeren bir sayı olmalıdır.  
- *güncelleştirilmiş*: IntDate, isteğe bağlı. Bu gizli dizi sürümü ne zaman güncelleştirildiği güncelleştirilmiş öznitelik gösterir. Bu değer, bu öznitelik eklenmesini önce en son güncelleştirildiği gizli dizileri için null. Değerini IntDate değerini içeren bir sayı olmalıdır.

#### <a name="BKMK_secret-date-time-ctrld-ops"></a> Tarih-saat denetimli işlemleri

Bir parolanın **alma** işlemi dışında değil henüz geçerli ve süresi dolan gizli diziler çalışır *nbf* / *exp* penceresi. Bir parolanın çağırma **alma** değil henüz geçerli bir gizli dizi için bir işlemi test amaçları için kullanılabilir. Alınıyor (**alma**ing) süresi dolmuş bir parola kurtarma işlemleri için kullanılabilir.

Veriler üzerinde daha fazla bilgi için bkz: türleri, [veri türleri](#BKMK_DataTypes).  

###  <a name="BKMK_SecretAccessControl"></a> Gizli anahtar erişim denetimi

Bu gizli dizileri kapsayıcı gibi davranır bir Key Vault düzeyinde erişim denetimi için Azure anahtar Kasası'nda yönetilen gizli dizileri sağlanır. Parolalar için aynı anahtar kasasındaki anahtarlar için erişim denetimi ilkesini farklı bir erişim denetimi İlkesi yoktur. Kullanıcılar, gizli tutmak için bir veya daha fazla kasalarını oluşturabilir ve senaryoya uygun segmentlere ayırma ve gizli dizilerinin Yönetimi korumak için gereklidir. Erişim denetimleri için gizli dizileri, anahtarlar için erişim denetimi bağımsızdır.  

Aşağıdaki izinleri, bir asıl başına temelinde bir kasasındaki gizli dizileri erişim denetim girişi ve kullanılabilir bir gizli dizi nesnesinde izin verilen işlemleri yakından yansıtma:  

-   *ayarlama*: yeni gizli dizileri oluşturma  
-   *Alma*: gizli dizi okumak  
-   *Liste*: gizli dizileri ya da Key Vault'ta depolanan bir gizli anahtarın sürümlerini listeler.  
-   *silme*: gizli anahtarı silme  
-   *tüm*: tüm izinler  

Gizli anahtarlarla çalışma hakkında daha fazla bilgi için bkz. [Key Vault REST API Başvurusu'ndaki gizli dizi işlemleri](/rest/api/keyvault).  

###  <a name="BKMK_SecretTags"></a> Gizli etiketleri  
Ek uygulamaya özgü meta veri etiketleri biçiminde belirtebilirsiniz. Azure Key Vault, en fazla 15 etiket, her biri olan 256 karakterlik bir ad ve 256 karakter değeri destekler.  

>[!Note]
>Bunlar varsa etiketleri çağıran tarafından okunabilir *listesi* veya *alma* söz konusu nesne türü; izni anahtarları, parola veya sertifikalara.

## <a name="key-vault-certificates"></a>Anahtar kasası sertifikaları

Key Vault sertifikaları destek sağlar, x509 yönetimi için sertifikaları ve aşağıdaki davranışları:  

-   Sertifikayı içeri aktarma veya Key Vault oluşturma işlemi aracılığıyla bir sertifika oluşturmak bir sertifika sahibinin sağlar. Sertifika yetkilisi sertifikalarını oluşturulur ve bu otomatik olarak imzalanan hem de içerir.
-   Güvenli Depolama ve Yönetimi X509 uygulamak bir anahtar kasası sertifikası sahibi sağlayan sertifika özel anahtar malzemesi etkileşim olmadan.  
-   Bir sertifika yaşam döngüsünü yönetmek için Key Vault yönlendiren bir ilke oluşturmak bir sertifika sahibinin sağlar.  
-   Kişi hakkında bilgi için bildirim yaşam döngüsü olayları geçerlilik süresi ve sertifikanın yenilenmesini sağlamak sertifika sahipleri sağlar.  
-   Seçili verenler - Key Vault iş ortağı X509 ile Otomatik yenilemeyi destekler sertifika sağlayıcıları / sertifika yetkilileri.

>[!Note]
>Olmayan iş Birliği yaparak sağlayıcıları/yetkilileri de izin verilir ancak otomatik yenileme özelliğini desteklemez.

###  <a name="BKMK_CompositionOfCertificate"></a> Sertifika oluşturma

Anahtar kasası sertifikası oluşturulurken bir adreslenebilir anahtar ve gizli dizi de aynı ada sahip oluşturulur. Key Vault anahtarı anahtar işlemleri ve Key Vault gizli sertifika değeri bir gizli dizi olarak alınmasına olanak sağlar. Anahtar kasası sertifikası ortak x509 de içeren sertifika meta verileri.  

Sertifikaların sürümü ve tanımlayıcı, anahtar ve gizli dizi benzer. Key Vault sertifikası yanıtta bir adreslenebilir anahtar ve gizli anahtar kasası sertifikası sürümüyle oluşturulan belirli bir sürümü kullanılabilir.
 
![Karmaşık nesneler Cetificates kullanılabilir](media/azure-key-vault.png)

###  <a name="BKMK_CertificateExportableOrNonExportableKey"></a> Anahtar dışarı aktarılabilir veya olmayan dışarı aktarılabilir

Anahtar kasası sertifikası oluşturulduğunda, sertifikayı oluşturmak için kullanılan ilke anahtarı verilebilir belirtilmişse, adreslenebilir gizli anahtarından PFX veya PEM biçiminde özel anahtarla alınabilir. Key Vault sertifikası oluşturmak için kullanılan ilkeyi aktarılamaz olmanın anahtarı belirtilmişse, ardından özel anahtar değerinin bir gizli dizi alınırken bir parçası değil.  

Adreslenebilir anahtar KV sertifikaları aktarılamaz daha ilgili hale gelir. Gelen adreslenebilir KV anahtarının operations eşlenen *keyusage* KV sertifika ilkesi KV sertifikayı oluşturmak için kullanılan alan.  

İki anahtar türleri desteklenir: *RSA* veya *RSA HSM* sertifikalara sahip. Dışarı aktarılabilir yalnızca RSA HSM tarafından desteklenmeyen RSA ile izin verilir.  

###  <a name="BKMK_CertificateAttributesAndTags"></a> Sertifika öznitelikler ve etiketler

Sertifika meta veriler ek olarak, bir adreslenebilir anahtar ve, adreslenebilir bir gizli dizi, anahtar kasası sertifikası öznitelikler ve etiketler içerir.  

#### <a name="attributes"></a>Öznitelikler

Sertifika öznitelikleri, öznitelikleri adreslenebilir anahtar ve gizli dizi KV sertifika oluşturduğunuzda oluşturulan yansıtılır.  

Bir Key Vault sertifika aşağıdaki özniteliklere sahiptir:  

-   *Etkin*: Boole, isteğe bağlı, varsayılan **true**. Bu öznitelik, gizli ya da çalıştırılabilir bir anahtar olarak sertifika verileri alınabilir, belirtmek için belirtilebilir. Bu ile birlikte kullanılan *nbf* ve *exp* bir işlemi oluştuğunda arasında *nbf* ve *exp*, etkinleştirildiğinde yalnızca verilir ayarlanmış true. İşlemler dışındaki *nbf* ve *exp* penceresi otomatik olarak izin verilmiyor.  

Yanıta dahil ek salt okunur özniteliği vardır:

-   *oluşturulan*: IntDate: sertifikayı bu sürüm ne zaman oluşturulduğunu gösterir.  
-   *güncelleştirilmiş*: IntDate: sertifikayı bu sürüm ne zaman güncelleştirildiği gösterir.  
-   *exp*: IntDate: x509 sona erme tarihi değerini içeren sertifika.  
-   *NBF*: IntDate: x509 tarih değerini içeren sertifika.  

> [!Note] 
> Key Vault sertifikanın süresi dolarsa adreslenebilir anahtarıdır ve gizli hale çalışamaz.  

#### <a name="tags"></a>Etiketler

 İstemci belirtilen benzer etiketler anahtarları ve gizli anahtar-değer çiftlerinin dictionary'si.  

 > [!Note]
> Bunlar varsa etiketleri çağıran tarafından okunabilir *listesi* veya *alma* söz konusu nesne türü; izni anahtarları, parola veya sertifikalara.

###  <a name="BKMK_CertificatePolicy"></a> Sertifika İlkesi

Bir sertifika ilkesi oluşturup bir KV sertifikanın yaşam döngüsünü yönetme hakkında bilgi içerir. Özel anahtara sahip bir sertifikayı anahtar kasasına içeri aktarıldığında, varsayılan bir ilke x509 okuyarak oluşturulan sertifika.  

Sıfırdan KV sertifika oluşturulurken bir ilke bu KV sertifika sürümü veya sonraki KV sertifika sürümü oluşturma hakkında bilgilendirmek için Key Vault sağlanması gerekir. İlke oluşturulduktan sonra art arda ile gerekli olmasa oluşturma işlemleri bir sonraki KV sertifika sürümleri oluşturmak için.  

Yalnızca bir örneğini KV sertifika tüm sürümleri için bir ilke yoktur.  

Yüksek düzeyde, bir sertifika ilkesi aşağıdakileri içerir:  

-   Sertifika Özellikleri X509: içeren konu adı, konu alternatif adları vb. x x509 oluşturmak için kullanılan sertifika isteği.  
-   Anahtar özellikleri: anahtar türü içeren anahtar uzunluğu, dışarı aktarılabilir ve anahtar alanları yeniden kullanabilirsiniz. Bu alanlar, anahtar kasasına bir anahtar oluşturmak nasıl isteyin.  
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

###  <a name="BKMK_CertificateIssuer"></a> Sertifikayı veren

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

Sertifikaları Portalı'ndan veren nesneleri oluşturma hakkında daha fazla bilgi için bkz. [Key Vault Certificates blogu](http://aka.ms/kvcertsblog)  

Key Vault farklı veren sağlayıcı yapılandırması ile birden çok veren nesneleri oluşturulmasını sağlar. Veren nesne oluşturulduktan sonra bir veya birden çok sertifika ilkeleri adı başvurulabilir. Veren nesneye başvuran bildirir x509 isterken veren nesne belirtilen yapılandırma kullanmak için Key Vault sertifika oluşturma ve yenileme sırasında CA sağlayıcısından sertifika.  

Veren nesneleri kasaya oluşturulur ve yalnızca aynı kasaya KV sertifikaları ile kullanılabilir.  

###  <a name="BKMK_CertificateContacts"></a> Sertifika kişileri

Sertifika kişileri sertifika ömrü olaylar tarafından tetiklenen bildirimleri göndermek için kişi bilgilerini içerir. Kişi bilgileri tarafından tüm sertifikalar anahtar kasasında paylaşılır. Anahtar Kasası'nda herhangi bir sertifika için bir olay için belirtilen tüm kişilere bildirim gönderilir.  

Ardından bir sertifika ilkesi için Otomatik yenilemeyi ayarlarsanız, aşağıdaki olaylar üzerinde bir bildirim gönderilir.  

-   Önce sertifika yenileme
-   Sertifika başarıyla yenilendi belirten veya bir hata varsa, sertifikanın el ile yenileme gerekliliği sertifika yenileme sonra.  

 Bir sertifika ilkesi el ile olarak ayarlanmışsa yenilenen (e-posta yalnızca) sonra bir bildirim gönderilir, sertifikayı yenilemek için saat olduğunda.  

###  <a name="BKMK_CertificateAccessControl"></a> Sertifika erişim denetimi

 Erişim denetimi sertifikalar için Key Vault tarafından yönetildiğinden ve bu sertifikaların kapsayıcı gibi davranır bir Key Vault düzeyinde sağlanır. Sertifikalar için anahtarları ve gizli anahtarları aynı anahtar Kasası'nda için erişim denetimi ilkesini farklı bir erişim denetimi İlkesi yoktur. Kullanıcıların, sertifikaları tutmak için bir veya daha fazla kasalarını oluşturabilir ve senaryoya uygun segmentlere ayırma ve yönetim sertifikaları sağlamak için gereklidir.  

 Bir asıl başına temelinde, gizli dizileri erişim denetim girişi bir key vault ile yakından yansıtmalar bir gizli dizi nesnesinde izin verilen işlemleri üzerinde aşağıdaki izinlere kullanılabilir:  

-   *Alma*: get Geçerli sertifika sürümü veya herhangi bir sürümünü bir sertifika verir. 
-   *Liste*: geçerli bir sertifika veya sertifika sürümleri listesini sağlar.  
-   *silme*: sertifika, ilkesini ve tüm sürümlerini silme sağlar  
-   *oluşturma*: sağlar anahtar kasası sertifikası oluşturun.  
-   *içeri aktarma*: bir anahtar kasası sertifikayı içeri aktarma sertifika malzeme sağlar.  
-   *Güncelleştirme*: bir sertifikanın güncelleştirilmesi sağlar.  
-   *managecontacts*: Key Vault sertifika kişileri yönetilmesine izin verir  
-   *getissuers*: get, bir sertifika verenler sağlar  
-   *listissuers*: sertifika verenler listesini sağlar.  
-   *setissuers*: sağlayan oluşturma veya güncelleştirme Key Vault sertifika verenler  
-   *deleteissuers*: Key Vault sertifika verenler silme sağlar  
-   *tüm*: tüm izinleri sağlar  

Daha fazla bilgi için [sertifika anahtar kasası REST API Başvurusu'ndaki işlemleri](/rest/api/keyvault). 

## <a name="see-also"></a>Ayrıca Bkz.

- [Kimlik doğrulama istekleri ve yanıtları](authentication-requests-and-responses.md)
- [Key Vault sürümleri](key-vault-versions.md)
- [Anahtar kasası Geliştirici Kılavuzu](/azure/key-vault/key-vault-developers-guide)
