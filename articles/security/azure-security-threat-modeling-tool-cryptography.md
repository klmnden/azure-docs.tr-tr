---
title: Şifreleme - Microsoft tehdit modelleme aracı - Azure | Microsoft Docs
description: Azaltıcı Etkenler tehdit modelleme Aracı kullanıma sunulan tehditleri
services: security
documentationcenter: na
author: RodSan
manager: RodSan
editor: RodSan
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: rodsan
ms.openlocfilehash: 5e5d487c4c793a49ce1d4ac17f6fcd672e09bb90
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30911667"
---
# <a name="security-frame-cryptography--mitigations"></a>Güvenlik çerçevesi: Şifreleme | Azaltıcı Etkenler 
| Ürün/hizmet | Makale |
| --------------- | ------- |
| **Web uygulaması** | <ul><li>[Yalnızca onaylanan simetrik bloğu şifreler ve anahtar uzunluklarını kullanın](#cipher-length)</li><li>[Blok şifreleme modları ve simetrik şifre başlatma vektörleri kullanım onaylanır](#vector-ciphers)</li><li>[Onaylanan Asimetrik algoritmalar, anahtar uzunlukları ve doldurma kullanın](#padding)</li><li>[Onaylanan rastgele sayı oluşturucuları kullanma](#numgen)</li><li>[Simetrik akış şifrelemeleri kullanmayın](#stream-ciphers)</li><li>[Onaylanan MAC/HMAC/anahtarlı karma algoritmaları kullan](#mac-hash)</li><li>[Yalnızca onaylanan şifreleme karma işlevlerini kullanma](#hash-functions)</li></ul> |
| **Veritabanı** | <ul><li>[Veritabanındaki verileri şifrelemek için güçlü şifreleme algoritmaları kullanır](#strong-db)</li><li>[SSIS paketleri şifrelenir ve dijital olarak imzalanmış](#ssis-signed)</li><li>[Dijital imza kritik veritabanı güvenliği sağlanabilir öğelerle Ekle](#securables-db)</li><li>[Şifreleme anahtarları korumak için SQL server EKM kullanın](#ekm-keys)</li><li>[Şifreleme anahtarları için veritabanı altyapısı açığa değil olursa AlwaysEncrypted özelliğini kullanın](#keys-engine)</li></ul> |
| **IOT cihaz** | <ul><li>[IOT cihaz üzerinde şifreleme anahtarlarını güvenli bir şekilde saklayın](#keys-iot)</li></ul> | 
| **IOT bulut ağ geçidi** | <ul><li>[IOT Hub'ına kimlik doğrulaması için yeterli uzunlukta rastgele simetrik anahtar oluşturma](#random-hub)</li></ul> | 
| **Dynamics CRM mobil istemcisi** | <ul><li>[Bir cihaz yönetim ilkesi kullanım PIN gerektirir ve Uzaktan sağlayan yerinde silme emin olun](#pin-remote)</li></ul> | 
| **Dynamics CRM Outlook istemcisi** | <ul><li>[Bir cihaz yönetim ilkesi, bir parola/PIN/otomatik kilit gerektirir ve tüm verileri (örneğin Bitlocker) şifreler yerinde olduğundan emin olun](#bitlocker)</li></ul> | 
| **Identity Server** | <ul><li>[İmzalama anahtarları kimlik sunucusu kullanırken alınır emin olun](#rolled-server)</li><li>[Şifreleme açısından güçlü bir istemci kimliği, gizli olmasını kimlik sunucusunda kullanılan](#client-server)</li></ul> | 

## <a id="cipher-length"></a>Yalnızca onaylanan simetrik bloğu şifreler ve anahtar uzunluklarını kullanın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | <p>Ürünler, yalnızca şifreleri simetrik engelleme ve kuruluşunuzdaki Crypto Danışmanı'nı tarafından açıkça onaylanan ilişkili anahtar uzunluklarını kullanmanız gerekir. Microsoft onaylı simetrik algoritmaları aşağıdaki blok şifrelemeler şunlardır:</p><ul><li>AES-128, 192 AES ve AES 256 için yeni kod kabul edilebilir</li><li>Var olan kodu ile geriye dönük uyumluluk için üç anahtar 3DES kabul edilebilir</li><li>Ürünler için simetrik blok şifrelemeler kullanma:<ul><li>Gelişmiş Şifreleme Standardı (AES) için yeni kod gereklidir</li><li>Üç anahtar Üçlü Veri Şifreleme Standardı (3DES) geriye dönük uyumluluk için var olan kodda izin verilir</li><li>RC2, DES, 2 anahtarı 3DES, DESX ve Skipjack, de dahil olmak üzere tüm diğer blok şifrelemeleri, yalnızca eski verilerin şifresini çözmek için kullanılabilir ve şifreleme için kullanılan konulmalıdır</li></ul></li><li>Simetrik blok şifreleme algoritmaları için 128 bit minimum anahtar uzunluğu gereklidir. Yeni kod AES için önerilen yalnızca blok şifreleme algoritması (AES-128, 192 AES ve AES 256 tüm kabul edilebilir)</li><li>Üç anahtar 3DES var olan kodda zaten kullanımda, şu anda kabul edilebilir; AES geçiş önerilir. DES, DESX, RC2 ve Skipjack artık güvenli olarak kabul edilir. Bu algoritmalar yalnızca geriye dönük uyumluluk amacıyla varolan verilerin şifresini çözmek için kullanılabilir ve veriler bir önerilen blok şifreleme kullanarak yeniden şifrelenmelidir</li></ul><p>Lütfen tüm simetrik blok şifrelemeler uygun başlatma vektörü (IV) kullanılmasını gerektiren bir onaylanan şifreleme modu ile kullanılması gerektiğini unutmayın. Uygun bir IV. genellikle rastgele bir sayı ve hiçbir zaman sabit bir değer</p><p>Kuruluşunuzun Crypto Panosu gözden geçirdikten sonra (aksine yeni veri yazma) var olan verileri okumak için eski veya başka türlü onaylanmamış şifreleme algoritmalarının ve kullanımı daha küçük anahtar uzunluklarını izin verilir. Ancak, bu gereksinim karşı bir özel durum için dosya gerekir. Zayıf şifreleme verileri okumak için kullanıldığında, ayrıca, Kurumsal dağıtımlarda, ürünleri uyarı Yöneticiler göz önünde bulundurmalısınız. Bu tür uyarılar, açıklayıcı ve eyleme dönüştürülebilir olmalıdır. Bazı durumlarda, Grup İlkesi zayıf şifreleme kullanımını denetlemek uygun olabilir</p><p>İzin verilen .NET algoritmaları yönetilen şifre çeviklik (tercih sırasına göre)</p><ul><li>AesCng (FIPS uyumlu)</li><li>AuthenticatedAesCng (FIPS uyumlu)</li><li>AESCryptoServiceProvider (FIPS uyumlu)</li><li>AESManaged (FIPS-uyumlu olmayan)</li></ul><p>Lütfen bu algoritmalar hiçbiri aracılığıyla belirtilebilir Not `SymmetricAlgorithm.Create` veya `CryptoConfig.CreateFromName` machine.config dosyasının değişiklik yapmadan yöntemleri. Ayrıca, .NET 3.5 önce .NET sürümlerinin AES adlandırdığınız unutmayın `RijndaelManaged`, ve `AesCng` ve `AuthenticatedAesCng` olan > CodePlex aracılığıyla kullanılabilir ve temel işletim sisteminde CNG gerektirir</p>

## <a id="vector-ciphers"></a>Blok şifreleme modları ve simetrik şifre başlatma vektörleri kullanım onaylanır

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Tüm simetrik blok şifrelemeler onaylanan simetrik şifreleme modu ile kullanılması gerekir. Yalnızca onaylanan modları CBC ve CTS ' dir. Özellikle, işlem elektronik kod kitap (ECB) modunu kaçınılmalıdır; Kuruluşunuzun Crypto Panosu gözden ECB kullanımını gerektirir. OFB, CFB, CTRL, CCM ve GCM tüm kullanımını veya başka bir şifreleme modu, kuruluşunuzun Crypto Kurulunca gözden geçirilmesi gerekir. Blok şifrelemeler "şifrelemeleri modları, CTRL gibi akış" ile aynı başlatma vektörü (IV) yeniden şifrelenmiş verilerin açığa neden olabilir. Tüm simetrik blok şifrelemeler da bir uygun başlatma vektörü (IV) kullanılması gerekir. Uygun bir IV şifreleme açısından güçlü, rastgele ve hiçbir zaman sabit bir değer sayısıdır. |

## <a id="padding"></a>Onaylanan Asimetrik algoritmalar, anahtar uzunlukları ve doldurma kullanın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | <p>Engellenenler şifreleme algoritmalarının kullanımını ürün güvenliğini önemli riski tanıtır ve kaçınılması gerekir. Ürünler ve ilişkili kuruluşunuzun Crypto Kurulunca açıkça onaylanmasını anahtar uzunlukları ve doldurma yalnızca bu şifreleme algoritmaları kullanmanız gerekir.</p><ul><li>**RSA -** şifreleme, anahtar değişimi ve imza için kullanılabilir. RSA Şifreleme yalnızca OAEP veya RSA KEM doldurma modu kullanmanız gerekir. Var olan kodu modu yalnızca uyumluluk için doldurma PKCS #1 v1.5 kullanabilir. Null doldurma kullanımını açıkça yasaklanmış. Anahtarları > 2048 bit = için yeni kod gereklidir. Var olan kodu anahtarlarını < için 2048 bit yalnızca geriye dönük uyumluluk bir gözden geçirme, kuruluşunuzun Crypto Panosu tarafından sonra destekleyebilir. Anahtarları < 1024 bit yalnızca şifre çözme ve doğrulama eski verileri için kullanılabilir ve değiştirilen IF şifreleme veya imzalama işlemleri için kullanılması gerekir</li><li>**ECDSA -** yalnızca imza için kullanılan. İle ECDSA > 256 bit anahtara = için yeni kod gereklidir. ECDSA dayanan imzalar üç NIST onaylanmış eğrilerden birini kullanmalıdır (p-256, p-384 ya da P521). Baştan sona analiz Eğriler yalnızca bir gözden geçirme, kuruluşunuzun Crypto panosu ile sonra kullanılabilir.</li><li>**ECDH -** yalnızca anahtar değişimi için kullanılabilir. İle ECDH > 256 bit anahtara = için yeni kod gereklidir. Anahtar değişimi ECDH tabanlı üç NIST onaylanmış eğrilerden birini kullanmalıdır (p-256, p-384 ya da P521). Baştan sona analiz Eğriler yalnızca bir gözden geçirme, kuruluşunuzun Crypto panosu ile sonra kullanılabilir.</li><li>**DSA -** sonra gözden geçirin ve kuruluşunuzun Crypto Panosu onayını kabul edilebilir olabilir. Kuruluşunuzun Crypto Panosu gözden zamanlamak için Güvenlik Danışmanı başvurun. DSA kullanımınız onaylanırsa az 2048 bit uzunluğunda anahtarları kullanımını engellemek gerektiğine dikkat edin. CNG Windows 8'dan sonra anahtar uzunluğu 2048 bit ve büyük destekler.</li><li>**Diffie-Hellman -** oturum anahtar yönetimi için yalnızca kullanılabilir. Anahtar uzunluğu > 2048 bit = için yeni kod gereklidir. Var olan kodu anahtar uzunluklarını < için 2048 bit yalnızca geriye dönük uyumluluk bir gözden geçirme, kuruluşunuzun Crypto Panosu tarafından sonra destekleyebilir. Anahtarları < 1024 bit kullanılamaz.</li><ul>

## <a id="numgen"></a>Onaylanan rastgele sayı oluşturucuları kullanma

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | <p>Ürünler onaylanan rastgele sayı oluşturucuları kullanmanız gerekir. C çalışma zamanı işlevi rand, .NET Framework sınıf System.Random veya GetTickCount gibi sistem işlevleri gibi geçici rastgele işlevleri bu nedenle, hiçbir zaman böyle kodda kullanılmalıdır. Çift Eliptik Eğri rastgele sayı oluşturucu (DUAL_EC_DRBG) algoritması engellendi</p><ul><li>**CNG -** BCryptGenRandom (arayan [PASSIVE_LEVEL] 0'dan büyük herhangi IRQL konumunda çalışabilir sürece önerilen BCRYPT_USE_SYSTEM_PREFERRED_RNG bayrağı kullanın)</li><li>**CAPI -** cryptGenRandom</li><li>**Win32/64-** (yeni uygulamalar kullanması gereken BCryptGenRandom veya CryptGenRandom) RtlGenRandom * rand_s * SystemPrng (için çekirdek modu)</li><li>**. NET -** RNGCryptoServiceProvider veya RNGCng</li><li>**Windows mağazası uygulamaları -** Windows.Security.Cryptography.CryptographicBuffer.GenerateRandom veya. GenerateRandomNumber</li><li>**Apple OS X (10.7+)/iOS(2.0+) -** int SecRandomCopyBytes (SecRandomRef rasgele size_t sayısı, uint8_t \*bayt)</li><li>**Apple OS X (< 10.7)-**  /dev/rastgele rastgele sayılar almak için kullanın</li><li>**Java(including Google Android Java Code) -** java.security.SecureRandom sınıfı. (Jelly Çekirdeklere) Android 4.3 için geliştiricilere gerekir geçici çözüm önerilen Android izleyin ve açıkça PRNG entropi /dev/urandom veya /dev/random ile başlatmak için kendi uygulamalarını güncelleştirmelerini unutmayın</li></ul>|

## <a id="stream-ciphers"></a>Simetrik akış şifrelemeleri kullanmayın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | RC4 gibi simetrik akış şifrelemeleri kullanılmamalıdır. Simetrik akış şifrelemeleri yerine ürünleri blok şifre, özellikle AES en az 128 bit anahtar uzunluğu ile kullanmanız gerekir. |

## <a id="mac-hash"></a>Onaylanan MAC/HMAC/anahtarlı karma algoritmaları kullan

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | <p>Ürünler, ileti kimlik doğrulama kodu (MAC) veya karma tabanlı ileti kimlik doğrulama kodu (HMAC) algoritmaları onaylanmış yalnızca kullanmanız gerekir.</p><p>Bir ileti kimlik doğrulama kodu (MAC), hem Gönderenin özgünlüğünü ve gizli bir anahtar kullanarak ileti bütünlüğünü doğrulamak, alıcı izin veren bir iletisine bilgi parçasıdır. Karma tabanlı bir ya da MAC kullanımını ([HMAC](http://csrc.nist.gov/publications/nistpubs/800-107-rev1/sp800-107-rev1.pdf)) veya [blok şifre tabanlı MAC](http://csrc.nist.gov/publications/nistpubs/800-38B/SP_800-38B.pdf) olarak izin verilir algoritmaları uzun tüm temel karma veya simetrik şifreleme da kullanım için onaylanan; şu anda bu içerir (HMAC SHA256, HMAC SHA384 ve SHA512 HMAC) HMAC SHA2 işlevleri ve CMAC/OMAC1 ve OMAC2 (bunlar AES'ye dayalı) şifre tabanlı Mac'ler engelleyin.</p><p>HMAC SHA1 kullanımını platform uyumluluğu için izin verilen olabilir, ancak bu yordam için bir özel dosya ve kuruluşunuzun şifre gözden geçirme uygulanabilecek gerekecektir. HMAC'ler kesilmesi değerinden 128 bit izin verilmez. Bir anahtar ve veri Onaylanmadı ve kuruluşunuzun Crypto Panosu uygulanabilecek gerekir karma yöntemlerini kullanım önce gözden müşteri kullanma.</p>|

## <a id="hash-functions"></a>Yalnızca onaylanan şifreleme karma işlevlerini kullanma

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | <p>Ürünler (SHA256, SHA384 ve SHA512) karma algoritması SHA-2 ailesini kullanmanız gerekir. Daha kısa bir karma gerekliyse unutmayın, kısa MD5 karma tasarlanmış bir veri yapısı sığması için 128-bit çıkış uzunluk gibi ürün ekipleriyle SHA2 karmaları (genellikle SHA256) birini kesmek. SHA384 SHA512 kesilmiş bir sürümü olduğunu unutmayın. Küçüktür 128 bit güvenlik amacıyla şifreleme karmalarını kesilmesi izin verilmez. Yeni kod MD2, MD4, MD5, SHA-0, SHA-1 veya RIPEMD karma algoritmaları kullanmaması gerekir. Karma çakışmaları, etkili bir şekilde bunları kıran Bu algoritmalar, bilgisayarlarda mümkündür.</p><p>.NET karma algoritmaları yönetilen şifreleme çevikliği için izin verilen (tercih sırasına göre):</p><ul><li>SHA512Cng (FIPS uyumlu)</li><li>SHA384Cng (FIPS uyumlu)</li><li>SHA256Cng (FIPS uyumlu)</li><li>SHA512Managed (FIPS-uyumlu olmayan) (kullanın SHA512 HashAlgorithm.Create veya CryptoConfig.CreateFromName çağrıları algoritma adı olarak)</li><li>SHA384Managed (FIPS-uyumlu olmayan) (kullanın SHA384 HashAlgorithm.Create veya CryptoConfig.CreateFromName çağrıları algoritma adı olarak)</li><li>SHA256Managed (FIPS-uyumlu olmayan) (kullanın SHA256 HashAlgorithm.Create veya CryptoConfig.CreateFromName çağrıları algoritma adı olarak)</li><li>SHA512CryptoServiceProvider (FIPS uyumlu)</li><li>SHA256CryptoServiceProvider (FIPS uyumlu)</li><li>SHA384CryptoServiceProvider (FIPS uyumlu)</li></ul>| 

## <a id="strong-db"></a>Veritabanındaki verileri şifrelemek için güçlü şifreleme algoritmaları kullanır

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [Bir şifreleme algoritması seçme](https://technet.microsoft.com/library/ms345262(v=sql.130).aspx) |
| **Adımları** | Şifreleme algoritmaları yetkisiz kullanıcılar tarafından kolayca alınamaz veri dönüşümleri tanımlayın. SQL Server Yöneticiler ve geliştiriciler DES, Üçlü DES, TRIPLE_DES_3KEY, RC2, RC4, 128-bit RC4'ü, DESX, 128 bit AES, 192 bit AES ve 256 bit AES gibi birkaç algoritmaları kümelerini seçmenizi sağlar. |

## <a id="ssis-signed"></a>SSIS paketleri şifrelenir ve dijital olarak imzalanmış

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [Dijital imzalar paketlerle tanımlamaya](https://msdn.microsoft.com/library/ms141174.aspx), [tehdit ve güvenlik açığı azaltma (Integration Services)](https://msdn.microsoft.com/library/bb522559.aspx) |
| **Adımları** | Bir paket paketin oluşturulduğu kuruluş ya da tek kaynağıdır. Bilinmeyen veya güvenilmeyen bir kaynaktan bir paketi çalıştıran riskli olabilir. Yetkisiz SSIS paketleri üzerinde oynanmasını önlemek için dijital imzalar kullanılmalıdır. Ayrıca, depolama/aktarım sırasında paketleri gizliliğini sağlamak için SSIS paketleri şifrelenmesi gerekir |

## <a id="securables-db"></a>Dijital imza kritik veritabanı güvenliği sağlanabilir öğelerle Ekle

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [İmza (Transact-SQL) Ekle](https://msdn.microsoft.com/library/ms181700) |
| **Adımları** | Güvenli kılınabilir bir kritik veritabanının bütünlüğünü doğrulanacak sahip olduğu durumlarda, dijital imzalar kullanılmalıdır. Veritabanı güvenliği sağlanabilir öğelerle saklı yordam, işlev, derleme veya tetikleyici gibi dijital olarak imzalanabilir. Bu yararlı olabilir, bir örneği aşağıda verilmiştir: ISV (bağımsız yazılım satıcısı) Destek müşterilerine birine teslim bir yazılım için sağlanan bize söyleyin. Destek sağlamadan önce ISV yazılımda güvenliği sağlanabilir bir veritabanı yanlışlıkla veya kötü amaçlı bir girişim tarafından değiştirilmediğini emin olmak istersiniz. Güvenli kılınabilir dijital olarak imzalı değilse, ISV dijital imzasını doğrulamak ve bütünlüğünü doğrulayamadı.| 

## <a id="ekm-keys"></a>Şifreleme anahtarları korumak için SQL server EKM kullanın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [SQL Server Genişletilebilir anahtar yönetimi (EKM)](https://msdn.microsoft.com/library/bb895340), [Azure anahtar kasası (SQL Server) kullanarak Genişletilebilir anahtar yönetimi](https://msdn.microsoft.com/library/dn198405) |
| **Adımları** | SQL Server Genişletilebilir anahtar yönetimi akıllı kart, USB aygıtı veya EKM/HSM modülü gibi devre dışı bir aygıt depolanması için veritabanı dosyalarını koruyun şifreleme anahtarları sağlar. Bu ayrıca Veritabanı Yöneticileri (dışında sysadmin grubunun üyeleri) karşı veri koruması sağlar. Dış EKM/HSM modülü, yalnızca veritabanı kullanıcı erişimi olduğunu şifreleme anahtarları kullanılarak veri şifrelenebilir. |

## <a id="keys-engine"></a>Şifreleme anahtarları için veritabanı altyapısı açığa değil olursa AlwaysEncrypted özelliğini kullanın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | SQL Azure, OnPrem |
| **Öznitelikleri**              | SQL sürümü - V12, MsSQL2016 |
| **Başvuruları**              | [(Veritabanı altyapısı) her zaman şifreli](https://msdn.microsoft.com/library/mt163865) |
| **Adımları** | Her zaman şifreli, kredi kartı numaraları veya Ulusal tanımlama numaraları (örneğin ABD sosyal güvenlik numarası), gibi Azure SQL Database veya SQL Server veritabanlarına depolanan önemli verileri korumak için tasarlanmış bir özelliktir. Her zaman şifreli istemcilerin istemci uygulamalar içinde hassas verileri şifrelemek ve hiçbir zaman veritabanı altyapısı (SQL veritabanı veya SQL Server) için şifreleme anahtarları açığa olanak sağlar. Sonuç olarak, her zaman şifreli verileri kendi (ve görüntüleyebileceğini) olanlar arasında ayrım sağlar ve kişilere verileri yönetme (ancak hiçbir erişimi olması gerekir) |

## <a id="keys-iot"></a>IOT cihaz üzerinde şifreleme anahtarlarını güvenli bir şekilde saklayın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | IOT cihaz | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Cihaz işletim sistemi - Windows IOT Core, cihaz bağlantısı - Azure IOT cihaz SDK'ları |
| **Başvuruları**              | [Windows IOT Core TPM'de](https://developer.microsoft.com/windows/iot/docs/tpm), [Windows IOT Core TPM'de ayarlama](https://developer.microsoft.com/windows/iot/win10/setuptpm), [Azure IOT cihaz SDK'sı TPM](https://github.com/Azure/azure-iot-hub-vs-cs/wiki/Device-Provisioning-with-TPM) |
| **Adımları** | Simetrik ya da sertifika özel anahtarlarını güvenli bir donanım TPM veya akıllı kart yongaları gibi depolama korumalı. Windows 10 IOT Core TPM kullanıcı destekler ve kullanılabilecek birkaç uyumlu TPM'ler: https://developer.microsoft.com/windows/iot/win10/tpm. Bellenim veya ayrık TPM kullanılması önerilir. Bir yazılım TPM yalnızca geliştirme ve sınama amacıyla kullanılmalıdır. TPM kullanılabilir ve anahtarları sağlanan sonra belirteci oluşturur kodu içindeki herhangi bir önemli bilgi sabit kodlama olmadan yazılması gerekir. | 

### <a name="example"></a>Örnek
```
TpmDevice myDevice = new TpmDevice(0);
// Use logical device 0 on the TPM 
string hubUri = myDevice.GetHostName(); 
string deviceId = myDevice.GetDeviceId(); 
string sasToken = myDevice.GetSASToken(); 

var deviceClient = DeviceClient.Create( hubUri, AuthenticationMethodFactory. CreateAuthenticationWithToken(deviceId, sasToken), TransportType.Amqp); 
```
Görüldüğü gibi aygıt birincil anahtarı kodda mevcut değil. Bunun yerine, TPM yuva 0 konumunda depolanır. TPM aygıtı sonra IOT Hub'ına bağlanmak için kullanılan kısa süreli bir SAS belirteci oluşturur. 

## <a id="random-hub"></a>IOT Hub'ına kimlik doğrulaması için yeterli uzunlukta rastgele simetrik anahtar oluşturma

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | IOT bulut ağ geçidi | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Ağ geçidi seçim - Azure IOT Hub |
| **Başvuruları**              | Yok  |
| **Adımları** | IOT Hub cihaz kimlik kayıt defteri içerir ve bir cihaz sağlarken, otomatik olarak rastgele bir simetrik anahtar oluşturur. Kimlik doğrulaması için kullanılan anahtar oluşturmak için Azure IOT Hub kimlik kayıt defteri, bu özelliği kullanmak için önerilir. IOT hub'ı da cihaz oluşturulurken belirtilmesi bir anahtar sağlar. Bir anahtarı dışında IOT Hub cihaz sağlama sırasında oluşturulur, rastgele bir simetrik anahtar veya en az 256 bit oluşturmak için önerilir. |

## <a id="pin-remote"></a>Bir cihaz yönetim ilkesi kullanım PIN gerektirir ve Uzaktan sağlayan yerinde silme emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Dynamics CRM mobil istemcisi | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Bir cihaz yönetim ilkesi kullanım PIN gerektirir ve Uzaktan sağlayan yerinde silme emin olun |

## <a id="bitlocker"></a>Bir cihaz yönetim ilkesi, bir parola/PIN/otomatik kilit gerektirir ve tüm verileri (örneğin Bitlocker) şifreler yerinde olduğundan emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Dynamics CRM Outlook istemcisi | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Bir cihaz yönetim ilkesi, bir parola/PIN/otomatik kilit gerektirir ve tüm verileri (örneğin Bitlocker) şifreler yerinde olduğundan emin olun |

## <a id="rolled-server"></a>İmzalama anahtarları kimlik sunucusu kullanırken alınır emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Kimlik sunucusu | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [Kimlik sunucusu - anahtarları, imzalar ve şifreleme ](https://identityserver.github.io/Documentation/docsv2/configuration/crypto.html) |
| **Adımları** | İmzalama anahtarları kimlik sunucusu kullanırken alınır emin olun. Başvurular bölümündeki bağlantıyı nasıl bu kesintileri kimlik sunucusuna bağlı olan uygulamalara neden olmadan planlanmalıdır açıklanmaktadır. |

## <a id="client-server"></a>Şifreleme açısından güçlü bir istemci kimliği, gizli olmasını kimlik sunucusunda kullanılan

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Kimlik sunucusu | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | <p>Şifreleme açısından güçlü bir istemci kimliği, gizli olmasını kimlik sunucusunda kullanılan. Aşağıdaki yönergeler, bir istemci kimliği ve parolası oluşturulurken kullanılmalıdır:</p><ul><li>İstemci kimliği olarak rastgele bir GUID oluşturmak</li><li>Gizlilik olarak şifreleme açısından rastgele bir 256 bit anahtar oluştur</li></ul>|
