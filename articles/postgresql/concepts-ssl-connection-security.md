---
title: SSL bağlantısı - tek bir sunucu PostgreSQL için Azure veritabanı'nda yapılandırma
description: Yönergeler ve bilgiler tek sunucu ve SSL bağlantıları düzgün bir şekilde kullanmak için ilişkili uygulamalar - PostgreSQL için Azure veritabanı yapılandırılamadı.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 56611267872ca79d7d2fe3a08c9b9f49a9b1840b
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65067422"
---
# <a name="configure-ssl-connectivity-in-azure-database-for-postgresql---single-server"></a>SSL bağlantısı - tek bir sunucu PostgreSQL için Azure veritabanı'nda yapılandırma
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

### <a name="install-a-cert-decoder-on-your-machine"></a>Bir sertifika kod çözücü makinenize yükleyin. 
Kullanabileceğiniz [OpenSSL](https://github.com/openssl/openssl) uygulamanızın güvenli bir şekilde veritabanı sunucunuza bağlanmak gerekli sertifika dosyasının kodu çözülemedi. OpenSSL yüklemek öğrenmek için bkz. [OpenSSL yükleme yönergeleri](https://github.com/openssl/openssl/blob/master/INSTALL). 


### <a name="decode-your-certificate-file"></a>Sertifika dosyanız kodunu çözme
İndirilen kök CA'ın dosya şifrelenmiş biçimindedir. OpenSSL sertifikası dosyanın kodunu çözmek için kullanın. Bunu yapmak için bu OpenSSL komutu çalıştırın:

```
openssl x509 -inform DER -in BaltimoreCyberTrustRoot.crt -text -out root.crt
```

### <a name="connecting-to-azure-database-for-postgresql-with-ssl-certificate-authentication"></a>Azure veritabanı'na PostgreSQL için SSL sertifika kimlik doğrulaması ile bağlanma
Sertifikanızı başarıyla çözülmüş, artık veritabanı sunucusuna güvenli bir şekilde SSL üzerinden bağlanabilirsiniz. Sunucu sertifika doğrulaması izin vermek için sertifika, kullanıcıların giriş dizinine dosya ~/.postgresql/root.crt yerleştirilmesi gerekir. (Microsoft Windows üzerinde dosya % APPDATA%\postgresql\root.crt olarak adlandırılır.). 

#### <a name="connect-using-psql"></a>Psql kullanarak bağlan
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

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki çeşitli uygulama bağlantı seçenekleri gözden [PostgreSQL için Azure veritabanı için bağlantı kitaplıkları](concepts-connection-libraries.md).
