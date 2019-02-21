---
title: SSL bağlantısı, PostgreSQL için Azure veritabanı'nda yapılandırma
description: Yönergeler ve bilgiler PostgreSQL ve SSL bağlantıları düzgün bir şekilde kullanmak için ilişkili uygulamalar için Azure veritabanı'nı yapılandırmak için.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 02/28/2018
ms.openlocfilehash: 13a1ed626e7741c90cf902c9ed01911985ca8424
ms.sourcegitcommit: 75fef8147209a1dcdc7573c4a6a90f0151a12e17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/20/2019
ms.locfileid: "56453452"
---
# <a name="configure-ssl-connectivity-in-azure-database-for-postgresql"></a>SSL bağlantısı, PostgreSQL için Azure veritabanı'nda yapılandırma
PostgreSQL için Azure veritabanı sunucunuzla istemci uygulamalarınız için Güvenli Yuva Katmanı (SSL) kullanarak PostgreSQL hizmetine bağlanma tercih eder. Veritabanı sunucunuzla istemci uygulamalarınız arasında SSL bağlantılarının zorunlu tutulması, sunucuya uygulamanız arasındaki veri akışını şifreleyerek "bağlantıyı izinsiz izleme" saldırılarına karşı korumaya yardımcı olur.

Varsayılan olarak, PostgreSQL veritabanı hizmeti SSL bağlantısını zorunlu tutacak şekilde yapılandırılır. İsteğe bağlı olarak, istemci uygulamanızın SSL bağlantısını desteklememesi durumunda veritabanı hizmetinize bağlanmak SSL zorunluluğunu devre dışı bırakabilirsiniz. 

## <a name="enforcing-ssl-connections"></a>SSL bağlantılarının zorlanması
Azure portalı ve CLI sağlanan PostgreSQL sunucuları için tüm Azure veritabanı için zorlama SSL bağlantıları, varsayılan olarak etkindir. 

Benzer şekilde, Azure portalında sunucunuzu "Bağlantı dizeleri" ayarları önceden tanımlanan bağlantı dizelerini SSL kullanarak veritabanı sunucunuza bağlanmak yaygın diller için gerekli parametreleri içerir. SSL parametresi örneğin bağlayıcı göre değişir "ssl = true" veya "sslmode = gerektirir" veya "sslmode = gerekli" yükleyeceklerdir.

## <a name="configure-enforcement-of-ssl"></a>SSL zorlama yapılandırın
İsteğe bağlı olarak SSL bağlantısını zorunlu devre dışı bırakabilirsiniz. Her zaman etkinleştirmek için Microsoft Azure önerir **SSL'yi zorunlu bağlantı** için Gelişmiş güvenlik ayarı.

### <a name="using-the-azure-portal"></a>Azure portalını kullanma
PostgreSQL için Azure veritabanı sunucunuza gidin ve tıklayarak **bağlantı güvenliği**. İki durumlu düğmeyi etkinleştirme veya devre dışı kullanın **SSL'yi zorunlu bağlantı** ayarı. ' A tıklayarak **Kaydet**. 

![Bağlantı güvenliği - devre dışı bırakma SSL zorlama](./media/concepts-ssl-connection-security/1-disable-ssl.png)

Ayar görüntüleyerek onaylayabilirsiniz **genel bakış** görmek için sayfayı **SSL zorlama durumu** göstergesi.

### <a name="using-azure-cli"></a>Azure CLI’yı kullanma
Etkinleştirmek veya devre dışı bırakabileceğiniz **ssl zorlama** parametresini kullanarak `Enabled` veya `Disabled` değerlerini sırasıyla Azure CLI.

```azurecli
az postgres server update --resource-group myresourcegroup --name mydemoserver --ssl-enforcement Enabled
```

## <a name="ensure-your-application-or-framework-supports-ssl-connections"></a>SSL bağlantıları, uygulama veya framework desteklediği olun
PostgreSQL veritabanı hizmetlerini, Drupal ve Django, gibi kullanmak birçok ortak uygulama çerçeveleri SSL yüklemesi sırasında varsayılan olarak etkinleştirmez. SSL bağlantısını etkinleştirme yüklemeden sonra veya uygulamaya özgü CLI komutları aracılığıyla yapılmalıdır. PostgreSQL sunucunuz, SSL bağlantılarının zorlanması ve ilişkili uygulama düzgün yapılandırılmadığından, uygulama, veritabanı sunucunuza bağlanmak başarısız olabilir. SSL bağlantıları etkinleştirme hakkında bilgi için uygulamanızın belgelerine bakın.


## <a name="applications-that-require-certificate-verification-for-ssl-connectivity"></a>SSL bağlantısını için sertifika doğrulaması gerektiren uygulamalar
Bazı durumlarda, uygulamaları güvenli bir şekilde bağlanmak için bir güvenilen sertifika yetkilisi (CA) sertifika dosyasından (.cer) oluşturulan bir yerel sertifika dosyası gerektirir. .Cer dosyasını alın ve kod çözme sertifikası uygulamanıza bağlamak için aşağıdaki adımlara bakın.

### <a name="download-the-certificate-file-from-the-certificate-authority-ca"></a>Sertifika yetkilisi (CA) sertifika dosyasını indirin 
PostgreSQL sunucusu bulunduğu için Azure veritabanı ile SSL üzerinden iletişim kurması için gereken sertifikayı [burada](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt). Sertifika dosyası yerel olarak indirin.

### <a name="download-and-install-openssl-on-your-machine"></a>OpenSSL makinenize yükleyin ve indirin 
Uygulamanızın güvenli bir şekilde veritabanı sunucunuza bağlanmak gerekli sertifika dosyası kodunu çözmek için yerel bilgisayarınıza yüklemeniz gerekir.

#### <a name="for-linux-os-x-or-unix"></a>Linux, OS X veya Unix için
OpenSSL kitaplıkları doğrudan kaynak kodunda sağlanır [OpenSSL Software Foundation](https://www.openssl.org). Aşağıdaki yönergeler OpenSSL Linux bilgisayarınıza yüklemek gereken adımlarda size kılavuzluk eder. Bu makalede, Ubuntu 12.04 üzerinde çalışmak için bilinen ve daha yüksek komutları kullanır.

Bir terminal oturumu açın ve OpenSSL indirin.
```bash
wget http://www.openssl.org/source/openssl-1.1.0e.tar.gz
``` 
İndirilen paketteki dosyaları ayıklayın.
```bash
tar -xvzf openssl-1.1.0e.tar.gz
```
Dizin dosyaları ayıkladığınız girin. Varsayılan olarak, aşağıdaki gibi olması gerekir.

```bash
cd openssl-1.1.0e
```
Aşağıdaki komutu yürüterek OpenSSL yapılandırın. Bir klasördeki dosyaları /usr/local/openssl farklı istiyorsanız, aşağıdaki uygun şekilde değiştirmek emin olun.

```bash
./config --prefix=/usr/local/openssl --openssldir=/usr/local/openssl
```
OpenSSL'ın düzgün yapılandırıldığından, sertifikanızı dönüştürmek için derlemeniz gerekir. Derlemek için aşağıdaki komutu çalıştırın:

```bash
make
```
Derleme tamamlandıktan sonra aşağıdaki komutu çalıştırarak yürütülebilir olarak OpenSSL yüklemeye hazırsınız:
```bash
make install
```
Sisteminizde OpenSSL başarıyla yükledikten onaylamak için onay ve aşağıdaki komutu aynı çıktı alacağınızdan emin olmak için çalıştırın.

```bash
/usr/local/openssl/bin/openssl version
```
Başarılı olursa şu iletiyi görmeniz gerekir.
```bash
OpenSSL 1.1.0e 7 Apr 2014
```

#### <a name="for-windows"></a>Windows için
Bir Windows Bilgisayara OpenSSL yüklemek, aşağıdaki şekillerde yapılabilir:
1. **(Önerilen)**  Penceresi 10'daki yerleşik Windows Bash işlevi kullanarak ve üzeri, OpenSSL varsayılan olarak yüklenir. Windows 10 için Windows Bash işlevselliğini etkinleştirmek yönergeler bulunabilir [burada](https://msdn.microsoft.com/commandline/wsl/install_guide).
2. Topluluk tarafından sağlanan bir Win32/64 uygulamasını indirirken size. OpenSSL Software Foundation sağlamaz veya belirli bir Windows Yükleyici gibi bir tavsiyede sırada kullanılabilir yükleyicileri listesini sağladıkları [burada](https://wiki.openssl.org/index.php/Binaries).

### <a name="decode-your-certificate-file"></a>Sertifika dosyanız kodunu çözme
İndirilen kök CA'ın dosya şifrelenmiş biçimindedir. OpenSSL sertifikası dosyanın kodunu çözmek için kullanın. Bunu yapmak için bu OpenSSL komutu çalıştırın:

```dos
openssl x509 -inform DER -in BaltimoreCyberTrustRoot.crt -text -out root.crt
```

### <a name="connecting-to-azure-database-for-postgresql-with-ssl-certificate-authentication"></a>Azure veritabanı'na PostgreSQL için SSL sertifika kimlik doğrulaması ile bağlanma
Sertifikanızı başarıyla çözülmüş, artık veritabanı sunucusuna güvenli bir şekilde SSL üzerinden bağlanabilirsiniz. Sunucu sertifika doğrulaması izin vermek için sertifika, kullanıcıların giriş dizinine dosya ~/.postgresql/root.crt yerleştirilmesi gerekir. (Microsoft Windows üzerinde dosya % APPDATA%\postgresql\root.crt olarak adlandırılır.). PostgreSQL için Azure veritabanı'na bağlanma yönergeleri sağlar.

#### <a name="using-psql-command-line-utility"></a>Psql komut satırı yardımcı programını kullanma
Aşağıdaki örnek, başarıyla psql komut satırı yardımcı programını kullanarak PostgreSQL sunucunuza bağlanmak gösterilmektedir. Kullanım `root.crt` dosya oluşturulduğunda ve `sslmode=verify-ca` veya `sslmode=verify-full` seçeneği.

PostgreSQL komut satırı arabirimi kullanarak, aşağıdaki komutu yürütün:
```bash
psql "sslmode=verify-ca sslrootcert=root.crt host=mydemoserver.postgres.database.azure.com dbname=postgres user=mylogin@mydemoserver"
```
Başarılı olursa şu çıktıyı alırsınız:
```bash
Password for user mylogin@mydemoserver:
psql (9.6.2)
WARNING: Console code page (437) differs from Windows code page (1252)
     8-bit characters might not work correctly. See psql reference
     page "Notes for Windows users" for details.
SSL connection (protocol: TLSv1.2, cipher: ECDHE-RSA-AES256-SHA384, bits: 256, compression: off)
Type "help" for help.

postgres=>
```

#### <a name="using-pgadmin-gui-tool"></a>PgAdmin GUI aracını kullanarak
PgAdmin SSL üzerinden güvenli bir şekilde bağlanmak için 4 yapılandırılması, gerektirir ayarlamanızı `SSL mode = Verify-CA` veya `SSL mode = Verify-Full` şu şekilde:

![PgAdmin - connection - SSL modu görüntüsü gerektirir](./media/concepts-ssl-connection-security/2-pgadmin-ssl.png)

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki çeşitli uygulama bağlantı seçenekleri gözden [PostgreSQL için Azure veritabanı için bağlantı kitaplıkları](concepts-connection-libraries.md).
