---
title: Şifreleme - Microsoft tehdit modelleme aracı - Azure | Microsoft Docs
description: Tehdit modelleme Aracı kullanıma sunulan tehdit azaltma
services: security
documentationcenter: na
author: jegeib
manager: jegeib
editor: jegeib
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/07/2017
ms.author: jegeib
ms.openlocfilehash: 608792d8389a87bad3521d3a48947b20dd036d67
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62121226"
---
# <a name="security-frame-cryptography--mitigations"></a>Güvenlik çerçevesi: Şifreleme | Risk azaltma işlemleri 

| Ürün/hizmet | Makale |
| --------------- | ------- |
| **Web uygulaması** | <ul><li>[Yalnızca onaylanan simetrik blok şifreleme özelliklerinden ve anahtar uzunluklarını kullanın](#cipher-length)</li><li>[Blok şifreleme modları ve başlatma vektörleri simetrik şifrelemeleri kullanım Onaylandı](#vector-ciphers)</li><li>[Onaylanan Asimetrik algoritmalar, anahtar uzunlukları ve doldurma kullanın](#padding)</li><li>[Onaylanan rastgele sayı oluşturucuları kullanma](#numgen)</li><li>[Simetrik stream şifrelemeleri kullanmayın](#stream-ciphers)</li><li>[Onaylanan MAC/HMAC/anahtarlı karma algoritmaları kullanın](#mac-hash)</li><li>[Yalnızca onaylanan şifreleme karması işlevlerini kullanma](#hash-functions)</li></ul> |
| **Veritabanı** | <ul><li>[Veritabanındaki verileri şifrelemek için güçlü şifreleme algoritmalarını kullanın](#strong-db)</li><li>[SSIS paketlerini şifrelenir ve dijital olarak imzalanması](#ssis-signed)</li><li>[Dijital imza kritik veritabanı güvenliği sağlanabilen öğelere Ekle](#securables-db)</li><li>[Şifreleme anahtarlarını korumak için SQL server EKM kullanın](#ekm-keys)</li><li>[Şifreleme anahtarları için veritabanı altyapısı açığa değil olursa AlwaysEncrypted özelliğini kullanın](#keys-engine)</li></ul> |
| **IOT cihaz** | <ul><li>[Şifreleme anahtarlarını güvenli bir şekilde IOT cihazında Store](#keys-iot)</li></ul> | 
| **IOT bulut ağ geçidi** | <ul><li>[Yeterli uzunlukta rastgele simetrik anahtar kimlik doğrulaması için IOT Hub oluşturma](#random-hub)</li></ul> | 
| **Dynamics CRM mobil istemci** | <ul><li>[Bir cihaz yönetim ilkesini bir PIN gerektirir ve Uzak sağlayan yerinde silinmeden önceki emin olun.](#pin-remote)</li></ul> | 
| **Dynamics CRM Outlook istemcisi** | <ul><li>[Bir cihaz Yönetimi İlkesi parolası/PIN/otomatik kilit gerektirir ve tüm verileri (örneğin, BitLocker) şifreler yerinde olduğundan emin olun](#bitlocker)</li></ul> | 
| **Kimlik sunucusu** | <ul><li>[İmzalama anahtarı kimlik sunucusu kullanılırken alınır emin olun](#rolled-server)</li><li>[Şifreleme açısından güçlü bir istemci kimliği, istemci gizli anahtarı olduğundan emin olun kimlik sunucusunda kullanılan](#client-server)</li></ul> | 

## <a id="cipher-length"></a>Yalnızca onaylanan simetrik blok şifreleme özelliklerinden ve anahtar uzunluklarını kullanın

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | <p>Ürünler, yalnızca simetrik blok şifreleme özelliklerinden ve açıkça kuruluşunuzdaki Crypto Advisor tarafından onaylanan ilişkili anahtar uzunluklarını kullanmanız gerekir. Microsoft onaylı simetrik algoritmaları aşağıdaki blok şifreleme özelliklerinden şunlardır:</p><ul><li>Yeni kod için AES-128, 192 AES ve AES-256'yı kabul edilir</li><li>Var olan kod ile geriye dönük uyumluluk için üç anahtar 3DES kabul edilebilir</li><li>Simetrik blok şifreleme özelliklerinden kullanarak ürünleri için:<ul><li>Gelişmiş Şifreleme Standardı (AES) için yeni kodu gereklidir</li><li>Geriye dönük uyumluluk için mevcut kod üç anahtar Üçlü Veri Şifreleme Standardı (3DES) verilse</li><li>RC2, DES, 2 anahtar 3DES, DESX ve Skipjack, dahil olmak üzere tüm diğer blok şifreleme özelliklerinden, yalnızca eski verilerin şifresini çözmek için kullanılabilir ve şifreleme için kullanılan değiştirilmelidir</li></ul></li><li>En küçük 128 bit anahtar uzunluğu simetrik blok şifreleme algoritmaları için gereklidir. Yalnızca blok şifreleme algoritması AES yeni kod için önerilen (AES-128, 192 AES ve AES-256'yı tüm kabul edilebilir)</li><li>Üç anahtar 3DES mevcut kodu zaten kullanımda, şu anda kabul edilebilir; AES geçiş önerilir. DES, DESX, RC2 ve Skipjack artık güvenli olarak kabul edilir. Bu algoritmalar yalnızca geriye dönük uyumluluk için mevcut verilerin şifresini çözmek için kullanılabilir ve veri önerilen blok şifreleme kullanılarak yeniden şifrelenmesini</li></ul><p>Tüm simetrik blok şifreleme özelliklerinden bir uygun başlatma vektörü (IV) kullanılmasını gerektiren bir onaylanan şifreleme modu ile kullanılması gerektiğini lütfen unutmayın. Uygun bir IV, genellikle rastgele bir sayı ve hiçbir zaman bir sabit değer</p><p>Kuruluşunuzun Crypto tablosu gözden geçirdikten sonra (yeni veriler yazılırken) yerine var olan verilerin okunması için eski veya başka türlü onaylanmamış şifreleme algoritmalarının ve kullanımını daha küçük olan anahtar uzunluklarını izin verilebilir. Ancak, bu gereksinim karşı bir özel durum için dosya gerekir. Zayıf şifreleme verileri okumak için kullanılan ek olarak, Kurumsal dağıtımlarda, ürünleri uyarı Yöneticiler dikkate almanız gerekir. Bu tür uyarılar, açıklayıcı ve eyleme dönüştürülebilir olmalıdır. Bazı durumlarda, Grup İlkesi zayıf şifreleme kullanımını denetlemek uygun olabilir</p><p>(Tercih sırasına göre) izin verilen .NET algoritmaları için yönetilen bir şifreleme çevikliği</p><ul><li>AesCng (FIPS ile uyumlu)</li><li>AuthenticatedAesCng (FIPS ile uyumlu)</li><li>AESCryptoServiceProvider (FIPS ile uyumlu)</li><li>AESManaged (FIPS uyumlu olmayan)</li></ul><p>Bu algoritmalar hiçbiri aracılığıyla belirtilebilir unutmayın `SymmetricAlgorithm.Create` veya `CryptoConfig.CreateFromName` machine.config dosyasına değişiklikler yapmadan yöntemleri. Ayrıca, .NET .NET 3.5 önce sürümlerinde AES olarak adlandırıldığına dikkat edin `RijndaelManaged`, ve `AesCng` ve `AuthenticatedAesCng` olan > CodePlex aracılığıyla kullanılabilir ve temel işletim sistemi, CNG gerektirir</p>

## <a id="vector-ciphers"></a>Blok şifreleme modları ve başlatma vektörleri simetrik şifrelemeleri kullanım Onaylandı

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Tüm simetrik blok şifreleme özelliklerinden bir onaylı simetrik şifreleme modu ile kullanılması gerekir. Yalnızca onaylanan CBC ve CTS modlarıdır. Özellikle elektronik kod kitap (ECB) modu kaçınılmalıdır; ECB kuruluşunuzun Crypto tablosu gözden geçirme gerektirir. Tüm kullanım OFB, CFB, CTRL, CCM, ve GCM veya başka bir şifreleme modu kuruluşunuzun Crypto tablosu tarafından gözden geçirilmesi gerekir. "CTRL gibi şifrelemeleri modları, akış" blok şifreleme özelliklerinden ile aynı başlatma vektörü (IV) yeniden şifrelenmiş verilerin açığa neden olabilir. Tüm simetrik blok şifreleme özelliklerinden uygun başlatma vektörü (IV) ile de kullanılması gerekir. Uygun bir IV şifreleme açısından güçlü bir rastgele sayı ve hiçbir zaman bir sabit değer var. |

## <a id="padding"></a>Onaylanan Asimetrik algoritmalar, anahtar uzunlukları ve doldurma kullanın

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | <p>Yasaklanmış şifreleme algoritmalarının kullanımını, önemli bir risk için ürün güvenlik tanıtır ve kaçınılmalıdır. Ürün bu şifreleme algoritmaları kullanmanız gerekir ve anahtar uzunlukları ve doldurma kuruluşunuzun Crypto tablosu tarafından açıkça onayladığınız ilişkili.</p><ul><li>**RSA -** şifreleme, anahtar değişimi ve imza için kullanılabilir. RSA Şifreleme yalnızca OAEP veya RSA KEM doldurma modları kullanmanız gerekir. PKCS #1 v1.5 doldurma modu yalnızca uyumluluk için mevcut kodu kullanabilir. Bir null doldurmaya kullanımını açıkça yasaklanmış. Anahtarlar > 2048 bit = yeni kod için gereklidir. Varolan kodu anahtarlarını < 2048 bit yalnızca geriye dönük uyumluluk kuruluşunuzun Crypto tablosu tarafından bir gözden geçirme sonra destekleyebilir. Anahtarları < 1024 bit yalnızca eski verilerin şifresini çözme ve doğrulama için kullanılabilir ve şifreleme veya imzalama işlemleri için değiştirilen ise kullanılması gerekir</li><li>**ECDSA -** yalnızca imza için kullanılan. İle ECDSA > 256 bit anahtara = yeni kod için gereklidir. İmzaları ECDSA tabanlı üç onaylanan NIST eğrisi birini kullanmalıdır (p-256, p-384 veya P521). Kapsamlı analiz eğrileri yalnızca bir gözden geçirme, kuruluşunuzun Crypto panosuyla sonra kullanılabilir.</li><li>**ECDH -** yalnızca anahtar değişimi için kullanılabilir. İle ECDH > 256 bit anahtara = yeni kod için gereklidir. Anahtar değişimi ECDH tabanlı üç onaylanan NIST eğrisi birini kullanmalıdır (p-256, p-384 veya P521). Kapsamlı analiz eğrileri yalnızca bir gözden geçirme, kuruluşunuzun Crypto panosuyla sonra kullanılabilir.</li><li>**DSA -** inceleme ve onaya kuruluşunuzun Crypto panosundan sonra kabul edilebilir. Kuruluşunuzun Crypto tablosu gözden geçirme zamanlamak için Güvenlik Danışmanı başvurun. DSA kullanımınız onaylanırsa, anahtar uzunluğu 2048 bitten verilenlerin gerekeceğini unutmayın. CNG Windows 8'den itibaren 2048 bit ve büyük anahtar uzunluklarını destekler.</li><li>**Diffie-Hellman -** oturum anahtarı yönetimi için yalnızca kullanılabilir. Anahtar uzunluğu > 2048 bit = yeni kod için gereklidir. Varolan kodu anahtar uzunlukları < 2048 bit yalnızca geriye dönük uyumluluk kuruluşunuzun Crypto tablosu tarafından bir gözden geçirme sonra destekleyebilir. Anahtarları < 1024 bit kullanılamaz.</li><ul>

## <a id="numgen"></a>Onaylanan rastgele sayı oluşturucuları kullanma

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | <p>Ürünler, onaylanan rastgele sayı üreteçlerinin kullanmanız gerekir. C çalışma zamanı işlevi rand, .NET Framework sınıfı System.Random veya GetTickCount gibi sistem işlevlerini gibi sözde rastgele işlevleri bu nedenle, hiçbir zaman bu tür kod içinde kullanılması gerekir. Çift Eliptik Eğri rasgele sayı üreteci (DUAL_EC_DRBG) algoritması kullanılamaz</p><ul><li>**CNG -** BCryptGenRandom (arayan [PASSIVE_LEVEL] 0'dan büyük herhangi bir IRQL konumunda çalışabilir sürece önerilen BCRYPT_USE_SYSTEM_PREFERRED_RNG bayrağı kullanın)</li><li>**CAPI -** cryptGenRandom</li><li>**Win32/64-** RtlGenRandom (yeni uygulamaları BCryptGenRandom veya CryptGenRandom kullanmalıdır) * rand_s * (çekirdek modu için) SystemPrng</li><li>**. NET -** RNGCryptoServiceProvider veya RNGCng</li><li>**Windows Store Apps -** Windows.Security.Cryptography.CryptographicBuffer.GenerateRandom veya. GenerateRandomNumber</li><li>**Apple OS X (10.7+)/iOS(2.0+) -** int SecRandomCopyBytes (SecRandomRef rasgele size_t sayısı, uint8_t \*bayt)</li><li>**Apple OS X (< 10.7)-** rastgele sayılar almak için/dev/rastgele kullanın</li><li>**Java(including Google Android Java Code) -** java.security.SecureRandom sınıfı. (Jelibon) Android 4.3 için geliştiricilerin gerekir geçici çözüm önerilen Android izleyin ve uygulamalarını açıkça PRNG /dev/urandom veya /dev/random entropi ile başlatmak için güncelleştirme gerektiğini unutmayın</li></ul>|

## <a id="stream-ciphers"></a>Simetrik stream şifrelemeleri kullanmayın

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | RC4 gibi simetrik stream şifrelemeleri kullanılmamalıdır. Simetrik stream şifrelemeleri yerine bir blok şifreleme, özellikle anahtar uzunluğu en az 128 bit AES ürünleri kullanmanız gerekir. |

## <a id="mac-hash"></a>Onaylanan MAC/HMAC/anahtarlı karma algoritmaları kullanın

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | <p>Ürünleri onaylanmış ileti kimlik doğrulama kodu (MAC) veya karma tabanlı ileti kimlik doğrulama kodu (HMAC) algoritmaları yalnızca kullanmanız gerekir.</p><p>Bir ileti kimlik doğrulama kodu (MAC), hem gönderen güvenilirliğini ve gizli bir anahtar kullanarak iletinin bütünlüğünü doğrulamak, alıcının izin veren bir iletiye eklenmiş bir bilgi parçasıdır. Karma tabanlı bir ya da MAC kullanımını ([HMAC](https://csrc.nist.gov/publications/nistpubs/800-107-rev1/sp800-107-rev1.pdf)) veya [blok-şifre tabanlı MAC](https://csrc.nist.gov/publications/nistpubs/800-38B/SP_800-38B.pdf) olarak verilse uzun tüm temel alınan karma veya simetrik şifreleme algoritmalar kullanmak için ayrıca onaylanan; şu anda bu içerir (Bu AES'ye dayalı) şifre tabanlı MAC HMAC SHA2 işlevleri (HMAC SHA256, HMAC SHA384 ve SHA512 HMAC) ve CMAC/OMAC1 ve OMAC2 engelleyin.</p><p>HMAC SHA1 kullanımını platform uyumluluğu için izin verilen ancak kuruluşunuzun şifre gözden geçirme sürecinden geçer ve bu yordamı için bir özel dosya için gerekli olacaktır. Küçüktür 128 bit HMAC'ler kesilmesi izin verilmez. Müşteri kullanmadan önce bir anahtar ve veri Onaylanmadı ve kuruluşunuzun Crypto tablosu geçmeleri gerekir karma yöntemleri gözden kullanma.</p>|

## <a id="hash-functions"></a>Yalnızca onaylanan şifreleme karması işlevlerini kullanma

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | <p>Ürünler, SHA-2 karma algoritma ailesi, (SHA256, SHA384 ve SHA512) kullanmanız gerekir. Daha kısa bir karma gerekliyse daha kısa MD5 karma değeri, düşünülerek tasarlanan bir veri yapısı sığdırmak için 128 bit çıkış uzunluk gibi ürün ekipleri (genellikle SHA256) SHA2 karmaları birini kesin. SHA384 SHA512 kesilmiş bir sürümü olduğunu unutmayın. Küçüktür 128 bit güvenlik amacıyla şifreleme karmalarını kesilmesi izin verilmez. Yeni kod, MD2, MD4, MD5, SHA-0, SHA-1 veya RIPEMD karma algoritmaları kullanmamalıdır. Karma çakışmaları için bu algoritmaları, bilgisayarlarda mümkündür, etkili bir şekilde bunları keser.</p><p>.NET yönetilen şifreleme Çeviklik için karma algoritma (tercih sırasına göre) izin verilen:</p><ul><li>SHA512Cng (FIPS ile uyumlu)</li><li>SHA384Cng (FIPS ile uyumlu)</li><li>SHA256Cng (FIPS ile uyumlu)</li><li>SHA512Managed (FIPS uyumlu olmayan) (kullanın SHA512 HashAlgorithm.Create veya CryptoConfig.CreateFromName çağrı algoritması adı olarak)</li><li>SHA384Managed (FIPS uyumlu olmayan) (kullanın SHA384 HashAlgorithm.Create veya CryptoConfig.CreateFromName çağrı algoritması adı olarak)</li><li>SHA256Managed (FIPS uyumlu olmayan) (kullanın SHA256 HashAlgorithm.Create veya CryptoConfig.CreateFromName çağrı algoritması adı olarak)</li><li>SHA512CryptoServiceProvider (FIPS ile uyumlu)</li><li>SHA256CryptoServiceProvider (FIPS ile uyumlu)</li><li>SHA384CryptoServiceProvider (FIPS ile uyumlu)</li></ul>| 

## <a id="strong-db"></a>Veritabanındaki verileri şifrelemek için güçlü şifreleme algoritmalarını kullanın

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [Bir şifreleme algoritması seçme](https://technet.microsoft.com/library/ms345262(v=sql.130).aspx) |
| **Adımları** | Yetkisiz kullanıcılar tarafından kolayca alınamaz veri dönüşümleri şifreleme algoritmalarını tanımlar. Yöneticiler ve geliştiriciler DES, Üçlü DES, TRIPLE_DES_3KEY, RC2, RC4, 128 bit RC4'ü, DESX, 128 bit AES, 192 bitlik AES ve 256 bit AES dahil olmak üzere çeşitli algoritmalar arasından seçmek SQL Server sağlar |

## <a id="ssis-signed"></a>SSIS paketlerini şifrelenir ve dijital olarak imzalanması

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [Dijital imzalar paketlerle kaynağını tanımlamak](https://msdn.microsoft.com/library/ms141174.aspx), [tehdit ve güvenlik açığı azaltma (Integration Services)](https://msdn.microsoft.com/library/bb522559.aspx) |
| **Adımları** | Bir paketi tek tek veya paket oluşturulan kuruluş kaynağıdır. Bilinmeyen veya güvenilmeyen bir kaynaktan bir paket çalıştıran riskli olabilir. SSIS paketlerini yetkisiz üzerinde oynanmasını önlemek için dijital imzalar kullanılmalıdır. Ayrıca, depolama/aktarım sırasında paketleri gizliliğini emin olmak için SSIS paketlerini şifrelenmesi gerekir |

## <a id="securables-db"></a>Dijital imza kritik veritabanı güvenliği sağlanabilen öğelere Ekle

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [İmza Ekle (Transact-SQL)](https://msdn.microsoft.com/library/ms181700) |
| **Adımları** | Güvenli kılınabilir kritik bir veritabanının bütünlüğünü doğrulanacak sahip olduğu durumlarda, dijital imzalar kullanılmalıdır. Veritabanı güvenliği sağlanabilir öğeler bir saklı yordam, işlev, derleme veya tetikleyici gibi dijital olarak imzalanabilir. Bu yararlı olabilir, bir örneği aşağıda verilmiştir: Bir ISV (bağımsız yazılım satıcısı) bir yazılım bir müşterileri için sunulan destek sağlamıştır bize söyleyin. Destek sağlamadan önce ISV yazılım güvenliği sağlanabilir veritabanı yanlışlıkla veya kötü amaçlı bir girişimi tarafından değiştirilmediğini emin olmak istersiniz. Güvenli kılınabilir dijital olarak imzalanmışsa, ISV bütünlüğünü doğrulamanız ve dijital imzasını doğrulayın.| 

## <a id="ekm-keys"></a>Şifreleme anahtarlarını korumak için SQL server EKM kullanın

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [SQL Server Genişletilebilir anahtar yönetimi (EKM)](https://msdn.microsoft.com/library/bb895340), [Azure anahtar kasası (SQL Server) ile Genişletilebilir anahtar yönetimi](https://msdn.microsoft.com/library/dn198405) |
| **Adımları** | SQL Server Genişletilebilir anahtar yönetimi şifreleme anahtarlarını, akıllı kart, USB cihazı veya EKM/HSM modülü gibi devre dışı bir aygıt depolanması veritabanı dosyaları korumak sağlar. Bu veri koruma (dışında sysadmin grubunun üyeleri) veritabanı yöneticilerinden sağlar. Yalnızca veritabanı kullanıcısı üzerinde dış EKM/HSM modülü erişimi olduğunu şifreleme anahtarları kullanılarak veri şifrelenebilir. |

## <a id="keys-engine"></a>Şifreleme anahtarları için veritabanı altyapısı açığa değil olursa AlwaysEncrypted özelliğini kullanın

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | SQL Azure, şirket içi |
| **Öznitelikler**              | SQL sürümü - V12, MsSQL2016 |
| **Başvuruları**              | [(Veritabanı altyapısı) her zaman şifreli](https://msdn.microsoft.com/library/mt163865) |
| **Adımları** | Her zaman şifreli, kredi kartı numaraları veya Ulusal Kimlik numaraları (örneğin ABD sosyal güvenlik numaraları) gibi Azure SQL veritabanı veya SQL Server veritabanlarında depolanan hassas verileri korumak için tasarlanmış bir özelliktir. Her zaman şifreli, istemci uygulamaları içindeki hassas verileri şifrelemek ve hiçbir zaman şifreleme anahtarları (SQL veritabanı veya SQL Server) veritabanı altyapısı açığa etmesine olanak tanır. Sonuç olarak, her zaman şifreli verileri (ve görüntüleyebileceğini) arasında bir ayrım sağlar ve kişilere yönetenler (ama hiçbir erişimi olması gerekir) |

## <a id="keys-iot"></a>Şifreleme anahtarlarını güvenli bir şekilde IOT cihazında Store

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | IoT Cihazı | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Cihaz işletim sistemi - Windows IOT Core, cihaz bağlantısı - Azure IOT cihaz SDK'ları |
| **Başvuruları**              | [Windows IOT Core TPM](https://developer.microsoft.com/windows/iot/docs/tpm), [Windows IOT Core TPM ayarlama](https://docs.microsoft.com/windows/iot-core/secure-your-device/setuptpm), [Azure IOT cihaz SDK'sı TPM](https://github.com/Azure/azure-iot-hub-vs-cs/wiki/Device-Provisioning-with-TPM) |
| **Adımları** | Simetrik ya da sertifika özel anahtarları güvenli bir şekilde bir donanımda depolama gibi akıllı kart ya da TPM yongaları korumalı. Windows 10 IoT Core TPM kullanıcısı destekler ve kullanılabilecek birkaç uyumlu TPM'ler vardır: https://docs.microsoft.com/windows/iot-core/secure-your-device/tpm#discrete-tpm-dtpm. Donanım veya ayrık TPM kullanmanız önerilir. Bir yazılım TPM yalnızca geliştirme ve test amacıyla kullanılmalıdır. TPM kullanılamıyorsa, ve anahtarları hazırlandıktan sonra belirteci oluşturan kod içindeki herhangi bir önemli bilgi sabit kodlama olmadan yazılmalıdır. | 

### <a name="example"></a>Örnek
```
TpmDevice myDevice = new TpmDevice(0);
// Use logical device 0 on the TPM 
string hubUri = myDevice.GetHostName(); 
string deviceId = myDevice.GetDeviceId(); 
string sasToken = myDevice.GetSASToken(); 

var deviceClient = DeviceClient.Create( hubUri, AuthenticationMethodFactory. CreateAuthenticationWithToken(deviceId, sasToken), TransportType.Amqp); 
```
Cihaz birincil anahtarı görülebileceği gibi kodda mevcut değil. Bunun yerine, TPM yuvasındaki 0 içinde depolanır. TPM cihazını IOT Hub'ına bağlanmak için kullanılır, kısa süreli bir SAS belirteci oluşturur. 

## <a id="random-hub"></a>Yeterli uzunlukta rastgele simetrik anahtar kimlik doğrulaması için IOT Hub oluşturma

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | IOT bulut ağ geçidi | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Ağ geçidi seçim - Azure IOT hub'ı |
| **Başvuruları**              | Yok  |
| **Adımları** | IOT hub'ı bir cihaz kimliği kayıt defteri içerir ve bir cihaz sağlanırken, otomatik olarak rastgele bir simetrik anahtar oluşturur. Kimlik doğrulaması için kullanılan anahtarı oluşturmak için Azure IOT Hub kimlik kayıt defteri'nın bu özelliği kullanmak için önerilir. IOT Hub cihazı oluşturulurken belirtilen anahtar için de sağlar. Bir anahtarı dışında bir IOT Hub cihaz sağlama sırasında oluşturulur, rastgele bir simetrik anahtar veya en az 256 bit oluşturmak için önerilir. |

## <a id="pin-remote"></a>Bir cihaz yönetim ilkesini bir PIN gerektirir ve Uzak sağlayan yerinde silinmeden önceki emin olun.

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Dynamics CRM mobil istemci | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Bir cihaz yönetim ilkesini bir PIN gerektirir ve Uzak sağlayan yerinde silinmeden önceki emin olun. |

## <a id="bitlocker"></a>Bir cihaz Yönetimi İlkesi parolası/PIN/otomatik kilit gerektirir ve tüm verileri (örneğin, BitLocker) şifreler yerinde olduğundan emin olun

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Dynamics CRM Outlook istemcisi | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Bir cihaz Yönetimi İlkesi parolası/PIN/otomatik kilit gerektirir ve tüm verileri (örneğin, BitLocker) şifreler yerinde olduğundan emin olun |

## <a id="rolled-server"></a>İmzalama anahtarı kimlik sunucusu kullanılırken alınır emin olun

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Kimlik sunucusu | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [Kimlik sunucusu - anahtarları, imzalar ve şifreleme](https://identityserver.github.io/Documentation/docsv2/configuration/crypto.html) |
| **Adımları** | İmzalama anahtarı kimlik sunucusu kullanılırken alınır emin olun. Bağlantı başvuruları bölümünde nasıl bu kesintiler kimlik sunucusuna bağlı olan uygulamaların neden olmadan planlanmalıdır açıklar. |

## <a id="client-server"></a>Şifreleme açısından güçlü bir istemci kimliği, istemci gizli anahtarı olduğundan emin olun kimlik sunucusunda kullanılan

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Kimlik sunucusu | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | <p>Şifreleme açısından güçlü bir istemci kimliği, istemci gizli anahtarı olduğundan emin olun kimlik sunucusu kullanılır. İstemci Kimliğini ve parolasını oluştururken aşağıdaki yönergeler kullanılmalıdır:</p><ul><li>İstemci kimliği rastgele bir GUID oluştur</li><li>Kriptografik rastgele 256 bit anahtar gizli dizi oluşturma</li></ul>|
