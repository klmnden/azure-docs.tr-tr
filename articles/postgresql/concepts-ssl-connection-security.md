---
title: "SSL bağlantısı Azure veritabanı'nda PostgreSQL için yapılandırma | Microsoft Docs"
description: "Yönergeleri ve bilgileri Azure veritabanı PostgreSQL ve düzgün şekilde SSL bağlantılarını kullanmak için ilişkili uygulamalar için yapılandırılır."
services: postgresql
author: JasonMAnderson
ms.author: janders
editor: jasonwhowell
manager: jhubbard
ms.service: postgresql
ms.custom: 
ms.topic: article
ms.date: 05/15/2017
ms.openlocfilehash: 685aa4c2f75b7c3260ca737f7c786157480b2d90
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="configure-ssl-connectivity-in-azure-database-for-postgresql"></a>SSL bağlantısı Azure veritabanı'nda PostgreSQL için yapılandırın.
Azure veritabanı PostgreSQL için istemci uygulamalarınızı Güvenli Yuva Katmanı (SSL) kullanarak PostgreSQL hizmetine bağlanma tercih eder. Veritabanı sunucunuz ve istemci uygulamalarınız arasında SSL bağlantılarını zorlamayı "ortadaki adam" saldırılarına karşı uygulamanız ile sunucu arasındaki veri akışını şifreleyerek korunmasına yardımcı.

Varsayılan olarak, PostgreSQL veritabanı hizmeti bir SSL bağlantısı gerektiren yapılandırılır. İsteğe bağlı olarak, istemci uygulamanız SSL bağlantısını desteklemiyorsa, veritabanı hizmete bağlanmak için SSL gerektirme devre dışı bırakabilirsiniz. 

## <a name="enforcing-ssl-connections"></a>SSL bağlantıları zorlama
CLI ve Azure portalı sağlanan PostgreSQL sunucuları için tüm Azure veritabanı için SSL bağlantılarını zorlama varsayılan olarak etkindir. 

Benzer şekilde, SSL kullanarak, veritabanı sunucunuza bağlanmak yaygın diller için gerekli parametreler sunucunuz Azure portalında altındaki "Bağlantı dizelerini" Ayarları'nda önceden tanımlanmış bağlantı dizeleri içerir. SSL parametre örneğin bağlayıcı göre değişir "ssl = true" veya "sslmode = gerektirir" veya "sslmode = gerekli" ve diğer Çeşitleme.

## <a name="configure-enforcement-of-ssl"></a>SSL zorlama yapılandırın
İsteğe bağlı olarak zorunlu SSL bağlantısını devre dışı bırakabilirsiniz. Microsoft Azure önerir her zaman etkinleştirmek için **Zorla SSL bağlantısı** için Gelişmiş güvenlik ayarı.

### <a name="using-the-azure-portal"></a>Azure portalını kullanma
Azure veritabanınız PostgreSQL sunucu için ziyaret edin ve tıklatın **bağlantı güvenliği**. Etkinleştirmek veya devre dışı bırakmak için iki durumlu düğme kullanmak **Zorla SSL bağlantısı** ayarı. Daha sonra **Kaydet**'e tıklayın. 

![Bağlantı güvenliği - devre dışı bırak SSL zorla](./media/concepts-ssl-connection-security/1-disable-ssl.png)

Ayar görüntüleyerek onaylayabilirsiniz **genel bakış** görmek için sayfayı **SSL zorlama durumu** göstergesi.

### <a name="using-azure-cli"></a>Azure CLI’yı kullanma
Etkinleştirmek veya devre dışı bırakabileceğiniz **ssl zorlama** parametresini kullanarak `Enabled` veya `Disabled` Azure CLI sırasıyla değerleri.

```azurecli
az postgres server update --resource-group myresourcegroup --name mypgserver-20170401 --ssl-enforcement Enabled
```

## <a name="ensure-your-application-or-framework-supports-ssl-connections"></a>Uygulama veya framework destekler, SSL bağlantılarını emin olun
Veritabanı Hizmetleri, Drupal ve Django gibi PostgreSQL kullanmak birçok yaygın uygulama çerçeveleri SSL yüklemesi sırasında varsayılan olarak etkinleştirmez. SSL bağlantısı etkinleştirme yüklemeden sonra veya uygulamaya özgü CLI komutları aracılığıyla yapılmalıdır. PostgreSQL sunucunuz SSL bağlantılarını zorlama ve ilişkili uygulama düzgün yapılandırılmamış, uygulama, veritabanı sunucunuza bağlanmaya başarısız olabilir. SSL bağlantıları etkinleştirme hakkında bilgi edinmek için uygulamanızın belgelerine bakın.


## <a name="applications-that-require-certificate-verification-for-ssl-connectivity"></a>Sertifika doğrulaması için SSL bağlantısı gerektiren uygulamalar
Bazı durumlarda, uygulamaları güvenli bir şekilde bağlanmak için bir güvenilen sertifika yetkilisi (CA) sertifika dosyasından (.cer) oluşturulan bir yerel sertifika dosyası gerektirir. .Cer dosyasını edinme, kod çözme sertifikası ve uygulamanıza bağlamak için aşağıdaki adımları bakın.

### <a name="download-the-certificate-file-from-the-certificate-authority-ca"></a>Sertifika yetkilisi (CA) sertifika dosyasını indirin 
PostgreSQL sunucu bulunuyor Azure veritabanınızla SSL üzerinden iletişim kurması için gereken sertifikayı [burada](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt). Sertifika dosyası yerel olarak yükleyin.

### <a name="download-and-install-openssl-on-your-machine"></a>OpenSSL makinenize yükleyip 
Güvenli, veritabanı sunucunuza bağlanmak, uygulamanız için gerekli sertifika dosyası kodunu çözmek için yerel bilgisayarınızda OpenSSL yüklemeniz gerekir.

#### <a name="for-linux-os-x-or-unix"></a>Linux, OS X veya Unix için
OpenSSL kitaplıkları doğrudan kaynak kodunda sağlanır [OpenSSL yazılım Foundation](http://www.openssl.org). Aşağıdaki yönergeler OpenSSL Linux bilgisayarınıza yüklemek gereken adımlarda size kılavuzluk eder. Bu makalede komutları Ubuntu 12.04 üzerinde çalışmak için bilinen ve daha yüksek kullanır.

Bir terminal oturumu açın ve OpenSSL yükleyin
```bash
wget http://www.openssl.org/source/openssl-1.1.0e.tar.gz
``` 
Yükleme paketinden dosyaları ayıklayın
```bash
tar -xvzf openssl-1.1.0e.tar.gz
```
Dizin dosyaları ayıkladığınız girin. Varsayılan olarak, aşağıdaki gibi olması gerekir.

```bash
cd openssl-1.1.0e
```
Aşağıdaki komutu çalıştırarak OpenSSL yapılandırın. Bir klasördeki dosyaları /usr/local/openssl farklı istiyorsanız, aşağıdaki uygun şekilde değiştirdiğinizden emin olun.

```bash
./config --prefix=/usr/local/openssl --openssldir=/usr/local/openssl
```
OpenSSL'ın düzgün yapılandırıldığından, sertifikanızı dönüştürmek için derleme gerekir. Derlemek için aşağıdaki komutu çalıştırın:

```bash
make
```
Derleme işlemi tamamlandıktan sonra aşağıdaki komutu çalıştırarak bir yürütülebilir dosya OpenSSL yüklemeye hazırsınız:
```bash
make install
```
Sisteminizde OpenSSL başarıyla yükledikten onaylamak için onay ve aşağıdaki komutu, aynı çıktı alırsınız emin olmak için çalıştırın.

```bash
/usr/local/openssl/bin/openssl version
```
Başarılı olursa şu iletiyi görmeniz gerekir.
```bash
OpenSSL 1.1.0e 7 Apr 2014
```

#### <a name="for-windows"></a>Windows için
Bir Windows Bilgisayarına OpenSSL yükleme aşağıdaki şekillerde yapılabilir:
1. **(Önerilen)**  Penceresi 10'daki yerleşik Windows için Bash işlevini kullanarak ve yukarıdaki OpenSSL varsayılan olarak yüklenir. Windows 10 için Windows Bash işlevselliğini etkinleştirmek yönergeler bulunabilir [burada](https://msdn.microsoft.com/en-us/commandline/wsl/install_guide).
2. Topluluk tarafından sağlanan bir Win32/64 uygulama indiriliyor aracılığıyla. OpenSSL yazılım Foundation sağlamaz veya herhangi belirli Windows Installer'lar onaylamaz olsa da, kullanılabilir yükleyicileri listesini sağladıkları [burada](https://wiki.openssl.org/index.php/Binaries)

### <a name="decode-your-certificate-file"></a>Sertifika dosyanızın kod çözme
İndirilen kök CA'ın dosya şifreli biçimindedir. Sertifika dosyası çözecek OpenSSL kullanın. Bunu yapmak için bu OpenSSL komutu çalıştırın:

```dos
OpenSSL>x509 -inform DER -in BaltimoreCyberTrustRoot.cer -text -out root.crt
```

### <a name="connecting-to-azure-database-for-postgresql-with-ssl-certificate-authentication"></a>Azure veritabanına PostgreSQL için SSL sertifikası kimlik doğrulaması ile bağlanma
Sertifikanızı başarıyla kodunu çözdü olduğunuza göre şimdi veritabanı sunucunuz için güvenli bir şekilde SSL üzerinden bağlayabilirsiniz. Sunucu sertifika doğrulaması izin vermek için kullanıcının giriş dizini içinde dosya ~/.postgresql/root.crt Sertifika yerleştirilmelidir. (Microsoft Windows dosya % APPDATA%\postgresql\root.crt olarak adlandırılır.). Aşağıdakiler için PostgreSQL Azure veritabanına bağlanmak için yönergeler sağlar.

> [!NOTE]
> Şu anda olduğunda bir bilinen sorun kullanırsanız "sslmode doğrulayın tam =" hizmet, bağlantı bağlantı şu hatayı vererek başarısız olur: _için sunucu sertifikası "&lt;bölge&gt;. Control.Database.Windows.NET"(ve diğer adları 7) ana bilgisayar adı eşleşmiyor"&lt;servername&gt;. postgres.database.azure.com "._
> Varsa "sslmode doğrulayın tam =" olan gerekli, lütfen sunucu adlandırma kuralı kullanmak  **&lt;servername&gt;. database.windows.net** bağlantı dizesi ana bilgisayar adı olarak. Bu sınırlama gelecekte kaldırmak planlıyoruz. Diğer kullanarak bağlantı [SSL modları](https://www.postgresql.org/docs/9.6/static/libpq-ssl.html#LIBPQ-SSL-SSLMODE-STATEMENTS) tercih edilen konak adlandırma kuralını kullanmaya devam etmelidir  **&lt;servername&gt;. postgres.database.azure.com**.

#### <a name="using-psql-command-line-utility"></a>Psql komut satırı yardımcı programını kullanma
Aşağıdaki örnek başarıyla psql komut satırı yardımcı programını kullanarak PostgreSQL sunucunuza bağlanmak nasıl gösterir. Kullanım `root.crt` oluşturulan dosya ve `sslmode=verify-ca` veya `sslmode=verify-full` seçeneği.

PostgreSQL komut satırı arabirimi kullanarak, aşağıdaki komutu yürütün:
```bash
psql "sslmode=verify-ca sslrootcert=root.crt host=mypgserver-20170401.postgres.database.azure.com dbname=postgres user=mylogin@mypgserver-20170401"
```
Başarılı olursa, aşağıdaki çıkış alırsınız:
```bash
Password for user mylogin@mypgserver-20170401:
psql (9.6.2)
WARNING: Console code page (437) differs from Windows code page (1252)
     8-bit characters might not work correctly. See psql reference
     page "Notes for Windows users" for details.
SSL connection (protocol: TLSv1.2, cipher: ECDHE-RSA-AES256-SHA384, bits: 256, compression: off)
Type "help" for help.

postgres=>
```

#### <a name="using-pgadmin-gui-tool"></a>PgAdmin GUI aracını kullanma
PgAdmin güvenli SSL üzerinden bağlanmak için 4 yapılandırma gerektirir ayarlamak `SSL mode = Verify-CA` veya `SSL mode = Verify-Full` gibi:

![Ekran görüntüsü - bağlantı - pgAdmin SSL modu gerektirir](./media/concepts-ssl-connection-security/2-pgadmin-ssl.png)

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki çeşitli uygulama bağlantı seçenekleri gözden [PostgreSQL için Azure veritabanı için bağlantı kitaplıkları](concepts-connection-libraries.md)
