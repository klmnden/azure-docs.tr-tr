---
title: MySQL için güvenli bir şekilde Azure veritabanına bağlanmak için SSL bağlantısını yapılandır
description: Azure veritabanı MySQL ve doğru SSL bağlantılarını kullanmak için ilişkili uygulamalar için düzgün şekilde hakkında yönergeler
services: mysql
author: ajlam
ms.author: andrela
editor: jasonwhowell
manager: kfile
ms.service: mysql-database
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: d51d11e0e41ad34f80dced7526883a4bd7c46ade
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="configure-ssl-connectivity-in-your-application-to-securely-connect-to-azure-database-for-mysql"></a>Uygulamanızda güvenli bir şekilde MySQL için Azure veritabanına bağlanmak için SSL bağlantısını yapılandır
Azure veritabanı için MySQL Azure veritabanınızı MySQL sunucusu için istemci uygulamaları için Güvenli Yuva Katmanı (SSL) kullanarak bağlanmayı desteklemektedir. Veritabanı sunucunuzla istemci uygulamalarınız arasında SSL bağlantılarının zorunlu tutulması, sunucuya uygulamanız arasındaki veri akışını şifreleyerek "bağlantıyı izinsiz izleme" saldırılarına karşı korumaya yardımcı olur.

## <a name="step-1-obtain-ssl-certificate"></a>1. adım: SSL sertifikası alın
Azure veritabanınızla MySQL sunucusu için SSL üzerinden iletişim kurmak için gerekli sertifikayı indirin [https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem) ve yerel diskinize (Bu sertifika dosyasını kaydedin öğretici c:\ssl örneğin kullanır).
**Microsoft Internet Explorer ve Microsoft Edge:** yükleme tamamlandıktan sonra sertifikayı BaltimoreCyberTrustRoot.crt.pem için yeniden adlandırın.

## <a name="step-2-bind-ssl"></a>2. adım: SSL bağlama
### <a name="connecting-to-server-using-the-mysql-workbench-over-ssl"></a>SSL üzerinden MySQL çalışma ekranı kullanarak sunucuya bağlanma
MySQL çalışma ekranı güvenli SSL üzerinden bağlanmak için yapılandırın. Kurulum yeni bağlantı iletişim kutusu gidin **SSL** sekmesi. İçinde **SSL CA dosyasını:** alanına, dosya konumunu **BaltimoreCyberTrustRoot.crt.pem**. 
![özelleştirilmiş döşeme kaydetme](./media/howto-configure-ssl/mysql-workbench-ssl.png) varolan bağlantılar için SSL bağlantı simgesine sağ tıklayarak bağlayın ve Düzenle'yi seçin. Ardından gidin **SSL** sekmesinde ve sertifika dosyası bağlayın.

### <a name="connecting-to-server-using-the-mysql-cli-over-ssl"></a>SSL üzerinden MySQL CLI kullanarak sunucuya bağlanma
SSL sertifikası bağlamak için başka bir yolu, aşağıdaki komutu çalıştırarak MySQL komut satırı arabirimini kullanmaktır:
```dos
mysql.exe -h mydemoserver.mysql.database.azure.com -u Username@mydemoserver -p --ssl-ca=c:\ssl\BaltimoreCyberTrustRoot.crt.pem
```

## <a name="step-3--enforcing-ssl-connections-in-azure"></a>3. adım: Azure SSL bağlantılarını zorlama 
### <a name="using-the-azure-portal"></a>Azure portalını kullanma
Azure Portalı'nı kullanarak, Azure veritabanınızı MySQL sunucusu için ziyaret edin ve ardından **bağlantı güvenliği**. Etkinleştirmek veya devre dışı bırakmak için iki durumlu düğme kullanmak **Zorla SSL bağlantısı** ayarlama ve ardından **kaydetmek**. Microsoft, her zaman etkinleştirmek için önerir **Zorla SSL bağlantısı** için Gelişmiş güvenlik ayarı.
![enable-ssl](./media/howto-configure-ssl/enable-ssl.png)

### <a name="using-azure-cli"></a>Azure CLI’yı kullanma
Etkinleştirmek veya devre dışı bırakabileceğiniz **ssl zorlama** etkin veya devre dışı değerleri sırasıyla Azure CLI kullanarak parametre.
```azurecli-interactive
az mysql server update --resource-group myresource --name mydemoserver --ssl-enforcement Enabled
```

## <a name="step-4-verify-the-ssl-connection"></a>Adım 4: SSL bağlantısını doğrulama
Mysql yürütme **durum** komutu SSL kullanarak MySQL sunucunuza bağlı doğrulayın:
```dos
mysql> status
```
Bağlantı göstermelidir çıkış gözden geçirerek şifrelenir onaylayın: **SSL: şifre kullanımda olan AES256 SHA** 

## <a name="sample-code"></a>Örnek kod
Güvenli bir bağlantı Azure veritabanı için MySQL için SSL üzerinden uygulamanızdan oluşturmak için aşağıdaki kod örneklerine bakın:

### <a name="php"></a>PHP
```php
$conn = mysqli_init();
mysqli_ssl_set($conn,NULL,NULL, "/var/www/html/BaltimoreCyberTrustRoot.crt.pem", NULL, NULL) ; 
mysqli_real_connect($conn, 'mydemoserver.mysql.database.azure.com', 'myadmin@mydemoserver', 'yourpassword', 'quickstartdb', 3306, MYSQLI_CLIENT_SSL, MYSQLI_CLIENT_SSL_DONT_VERIFY_SERVER_CERT);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
}
```
### <a name="python-mysqlconnector-python"></a>Python (MySQLConnector Python)
```python
try:
    conn=mysql.connector.connect(user='myadmin@mydemoserver', 
        password='yourpassword', 
        database='quickstartdb', 
        host='mydemoserver.mysql.database.azure.com', 
        ssl_ca='/var/www/html/BaltimoreCyberTrustRoot.crt.pem')
except mysql.connector.Error as err:
    print(err)
```
### <a name="python-pymysql"></a>Python (PyMySQL)
```python
conn = pymysql.connect(user = 'myadmin@mydemoserver', 
        password = 'yourpassword', 
        database = 'quickstartdb', 
        host = 'mydemoserver.mysql.database.azure.com', 
        ssl = {'ssl': {'ca': '/var/www/html/BaltimoreCyberTrustRoot.crt.pem'}})
```
### <a name="ruby"></a>Ruby
```ruby
client = Mysql2::Client.new(
        :host     => 'mydemoserver.mysql.database.azure.com', 
        :username => 'myadmin@mydemoserver',      
        :password => 'yourpassword',    
        :database => 'quickstartdb',
        :ssl_ca => '/var/www/html/BaltimoreCyberTrustRoot.crt.pem'
    )
```
### <a name="golang"></a>Golang
```go
rootCertPool := x509.NewCertPool()
pem, _ := ioutil.ReadFile("/var/www/html/BaltimoreCyberTrustRoot.crt.pem")
if ok := rootCertPool.AppendCertsFromPEM(pem); !ok {
    log.Fatal("Failed to append PEM.")
}
mysql.RegisterTLSConfig("custom", &tls.Config{RootCAs: rootCertPool, InsecureSkipVerify: true})
var connectionString string
connectionString = fmt.Sprintf("%s:%s@tcp(%s:3306)/%s?allowNativePasswords=true&tls=custom",'myadmin@mydemoserver' , 'yourpassword', 'mydemoserver.mysql.database.azure.com', 'quickstartdb')   
db, _ := sql.Open("mysql", connectionString)
```
### <a name="javajdbc"></a>JAVA(JDBC)
```java
# generate truststore and keystore in code
String importCert = " -import "+
    " -alias mysqlServerCACert "+
    " -file " + ssl_ca +
    " -keystore truststore "+
    " -trustcacerts " + 
    " -storepass password -noprompt ";
String genKey = " -genkey -keyalg rsa " +
    " -alias mysqlClientCertificate -keystore keystore " +
    " -storepass password123 -keypass password " + 
    " -dname CN=MS ";
sun.security.tools.keytool.Main.main(importCert.trim().split("\\s+"));
sun.security.tools.keytool.Main.main(genKey.trim().split("\\s+"));

# use the generated keystore and truststore 
System.setProperty("javax.net.ssl.keyStore","path_to_keystore_file");
System.setProperty("javax.net.ssl.keyStorePassword","password");
System.setProperty("javax.net.ssl.trustStore","path_to_truststore_file");
System.setProperty("javax.net.ssl.trustStorePassword","password");

url = String.format("jdbc:mysql://%s/%s?serverTimezone=UTC&useSSL=true", 'mydemoserver.mysql.database.azure.com', 'quickstartdb');
properties.setProperty("user", 'myadmin@mydemoserver');
properties.setProperty("password", 'yourpassword');
conn = DriverManager.getConnection(url, properties);
```
### <a name="javamariadb"></a>Java(MariaDB)
```java
# generate truststore and keystore in code
String importCert = " -import "+
    " -alias mysqlServerCACert "+
    " -file " + ssl_ca +
    " -keystore truststore "+
    " -trustcacerts " + 
    " -storepass password -noprompt ";
String genKey = " -genkey -keyalg rsa " +
    " -alias mysqlClientCertificate -keystore keystore " +
    " -storepass password123 -keypass password " + 
    " -dname CN=MS ";
sun.security.tools.keytool.Main.main(importCert.trim().split("\\s+"));
sun.security.tools.keytool.Main.main(genKey.trim().split("\\s+"));

# use the generated keystore and truststore 
System.setProperty("javax.net.ssl.keyStore","path_to_keystore_file");
System.setProperty("javax.net.ssl.keyStorePassword","password");
System.setProperty("javax.net.ssl.trustStore","path_to_truststore_file");
System.setProperty("javax.net.ssl.trustStorePassword","password");

url = String.format("jdbc:mariadb://%s/%s?useSSL=true&trustServerCertificate=true", 'mydemoserver.mysql.database.azure.com', 'quickstartdb');
properties.setProperty("user", 'myadmin@mydemoserver');
properties.setProperty("password", 'yourpassword');
conn = DriverManager.getConnection(url, properties);
```

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki çeşitli uygulama bağlantı seçenekleri gözden [Azure veritabanı için MySQL için bağlantı kitaplıkları](concepts-connection-libraries.md)
