---
title: SSL bağlantısı - tek bir sunucu PostgreSQL için Azure veritabanı'nda yapılandırma
description: Yönergeler ve bilgiler tek sunucu ve SSL bağlantıları düzgün bir şekilde kullanmak için ilişkili uygulamalar - PostgreSQL için Azure veritabanı yapılandırılamadı.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 06/27/2019
ms.openlocfilehash: 686adfb2998eff10ef4b9f378163b164ba970c56
ms.sourcegitcommit: aa66898338a8f8c2eb7c952a8629e6d5c99d1468
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67461836"
---
# <a name="configure-ssl-connectivity-in-azure-database-for-postgresql---single-server"></a>SSL bağlantısı - tek bir sunucu PostgreSQL için Azure veritabanı'nda yapılandırma
PostgreSQL için Azure veritabanı sunucunuzla istemci uygulamalarınız için Güvenli Yuva Katmanı (SSL) kullanarak PostgreSQL hizmetine bağlanma tercih eder. Veritabanı sunucunuz ve istemci uygulamalarınız arasında SSL bağlantılarının zorlanması sunucuya uygulamanız arasındaki veri akışını şifreleyerek "adam-in--middle" saldırılarına karşı korumaya yardımcı olur.

Varsayılan olarak, PostgreSQL veritabanı hizmeti SSL bağlantısını zorunlu tutacak şekilde yapılandırılır. İstemci uygulamanızın SSL bağlantısını desteklememesi durumunda SSL zorunluluğunu devre dışı seçebilirsiniz. 

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
PostgreSQL veritabanı hizmetlerini kullanan bazı uygulama çerçeveleri SSL yüklemesi sırasında varsayılan olarak etkinleştirmez. PostgreSQL sunucunuza SSL bağlantılarını zorlar, ancak uygulama SSL için yapılandırılmamış uygulama, veritabanı sunucunuza bağlanmak başarısız olabilir. SSL bağlantıları etkinleştirme hakkında bilgi için uygulamanızın belgelerine bakın.


## <a name="applications-that-require-certificate-verification-for-ssl-connectivity"></a>SSL bağlantısını için sertifika doğrulaması gerektiren uygulamalar
Bazı durumlarda, uygulamaları güvenli bir şekilde bağlanmak için bir güvenilen sertifika yetkilisi (CA) sertifika dosyasından (.cer) oluşturulan bir yerel sertifika dosyası gerektirir. PostgreSQL sunucusu konumu için Azure veritabanı'na bağlanmak için sertifikayı https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem. Sertifika dosyasını indirin ve tercih edilen konumunuza kaydedin. 

### <a name="connect-using-psql"></a>Psql kullanarak bağlan
Aşağıdaki örnek, psql komut satırı yardımcı programını kullanarak PostgreSQL sunucunuza bağlanmak gösterilmektedir. Kullanım `sslmode=verify-full` SSL sertifika doğrulama zorlamak için bağlantı dizesi ayarı. Yerel sertifika dosyası yoluna geçirmeniz `sslrootcert` parametresi.

Psql bağlantı dizesi örneği aşağıdadır:
```
psql "sslmode=verify-full sslrootcert=BaltimoreCyberTrustRoot.crt host=mydemoserver.postgres.database.azure.com dbname=postgres user=myusern@mydemoserver"
```

> [!TIP]
> Onaylamak için geçirilen değer `sslrootcert` kaydettiğiniz sertifika dosyası yolu eşleşir.


## <a name="next-steps"></a>Sonraki adımlar
Çeşitli uygulama bağlantı seçenekleri gözden [PostgreSQL için Azure veritabanı için bağlantı kitaplıkları](concepts-connection-libraries.md).
