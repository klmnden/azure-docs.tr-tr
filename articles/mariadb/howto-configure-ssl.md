---
title: Güvenli bir şekilde MariaDB için Azure veritabanı'na bağlanmak için SSL bağlantısı yapılandırma
description: MariaDB ve SSL bağlantıları doğru bir şekilde kullanmak için ilişkili uygulamalar için Azure veritabanı düzgün bir şekilde nasıl yapılandıracağınızı öğrenmek için yönergeler
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 01/24/2019
ms.openlocfilehash: 6de16b7264c7ae7ead06b4e131e7fa46c664cedd
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64573345"
---
# <a name="configure-ssl-connectivity-in-your-application-to-securely-connect-to-azure-database-for-mariadb"></a>Güvenli bir şekilde MariaDB için Azure veritabanı'na bağlanmak üzere uygulamanızda SSL bağlantısı yapılandırma
MariaDB için Azure veritabanı, Güvenli Yuva Katmanı (SSL) kullanarak istemci uygulamalar MariaDB için Azure veritabanı sunucunuza bağlanmayı destekler. Veritabanı sunucunuzla istemci uygulamalarınız arasında SSL bağlantılarının zorunlu tutulması, sunucuya uygulamanız arasındaki veri akışını şifreleyerek "bağlantıyı izinsiz izleme" saldırılarına karşı korumaya yardımcı olur.

## <a name="obtain-ssl-certificate"></a>SSL sertifikası alın
MariaDB sunucudan için Azure veritabanı ile SSL üzerinden iletişim kurması için gereken sertifikayı indirin [ https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem ](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem) ve sertifika dosyasını (Bu öğreticide c:\ssl örneğin) yerel sürücünüze kaydedin.
**Microsoft Internet Explorer ve Microsoft Edge için:** İndirme tamamlandıktan sonra sertifika BaltimoreCyberTrustRoot.crt.pem için yeniden adlandırın.

## <a name="bind-ssl"></a>SSL bağlama
### <a name="connecting-to-server-using-the-mysql-workbench-over-ssl"></a>SSL üzerinden MySQL Workbench kullanarak sunucuya bağlanma
MySQL Workbench, SSL üzerinden güvenli bir şekilde bağlanmak için yapılandırın. Yeni bağlantı oluştur iletişim kutusu gidin **SSL** sekmesi. İçinde **SSL CA dosyası:** dosya konumunu girin **BaltimoreCyberTrustRoot.crt.pem**. 
![özelleştirilmiş kutucuk kaydetme](./media/howto-configure-ssl/mysql-workbench-ssl.png) varolan bağlantılar için SSL bağlantısı simgeye tıklanarak bağlayın ve Düzenle'yi seçin. Ardından gidin **SSL** sekme ve sertifika dosyası bağlayın.

### <a name="connecting-to-server-using-the-mysql-cli-over-ssl"></a>SSL üzerinden MySQL CLI'yı kullanarak sunucuya bağlanma
SSL sertifikası bağlama başka bir yolu, aşağıdaki komutları çalıştırarak MySQL komut satırı arabirimini kullanmaktır. 

```bash
mysql.exe -h mydemoserver.mariadb.database.azure.com -u Username@mydemoserver -p --ssl-mode=REQUIRED --ssl-ca=c:\ssl\BaltimoreCyberTrustRoot.crt.pem
```

> [!NOTE]
> Windows üzerinde MySQL komut satırı arabirimi kullanarak, bir hata alabilir `SSL connection error: Certificate signature check failed`. Bu meydana gelirse, değiştirin `--ssl-mode=REQUIRED --ssl-ca={filepath}` parametrelerle `--ssl`.

## <a name="enforcing-ssl-connections-in-azure"></a>Azure'da SSL bağlantılarının zorlanması 
### <a name="using-the-azure-portal"></a>Azure portalını kullanma
Azure portalını kullanarak, MariaDB için Azure veritabanı sunucunuza ziyaret edin ve ardından **bağlantı güvenliği**. İki durumlu düğmeyi etkinleştirme veya devre dışı kullanın **SSL'yi zorunlu bağlantı** ayarlama ve ardından **Kaydet**. Microsoft, her zaman etkinleştirmeyi önerir **SSL'yi zorunlu bağlantı** için Gelişmiş güvenlik ayarı.
![ssl etkinleştir](./media/howto-configure-ssl/enable-ssl.png)

### <a name="using-azure-cli"></a>Azure CLI’yı kullanma
Etkinleştirmek veya devre dışı bırakabileceğiniz **ssl zorlama** etkin veya devre dışı değerleri sırasıyla Azure CLI'yi kullanarak parametre.
```azurecli-interactive
az mariadb server update --resource-group myresource --name mydemoserver --ssl-enforcement Enabled
```

## <a name="verify-the-ssl-connection"></a>SSL bağlantısını doğrulama
Mysql yürütme **durumu** MariaDB sunucunuza SSL kullanarak bağlı olduğunuzu doğrulamak için komut:
```sql
status
```
Bağlantı göstermelidir çıkış inceleyerek şifrelenir onaylayın:  **SSL: AES256 SHA şifredir kullanılıyor** 

## <a name="sample-code"></a>Örnek kod
Güvenli bir bağlantı için Azure veritabanı MariaDB için SSL üzerinden uygulamanızı oluşturmak için aşağıdaki kod örneklerine bakın:

### <a name="php"></a>PHP
```php
$conn = mysqli_init();
mysqli_ssl_set($conn,NULL,NULL, "/var/www/html/BaltimoreCyberTrustRoot.crt.pem", NULL, NULL) ; 
mysqli_real_connect($conn, 'mydemoserver.mariadb.database.azure.com', 'myadmin@mydemoserver', 'yourpassword', 'quickstartdb', 3306, MYSQLI_CLIENT_SSL, MYSQLI_CLIENT_SSL_DONT_VERIFY_SERVER_CERT);
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
        host='mydemoserver.mariadb.database.azure.com', 
        ssl_ca='/var/www/html/BaltimoreCyberTrustRoot.crt.pem')
except mysql.connector.Error as err:
    print(err)
```
### <a name="python-pymysql"></a>Python (PyMySQL)
```python
conn = pymysql.connect(user = 'myadmin@mydemoserver', 
        password = 'yourpassword', 
        database = 'quickstartdb', 
        host = 'mydemoserver.mariadb.database.azure.com', 
        ssl = {'ssl': {'ca': '/var/www/html/BaltimoreCyberTrustRoot.crt.pem'}})
```
### <a name="ruby"></a>Ruby
```ruby
client = Mysql2::Client.new(
        :host     => 'mydemoserver.mariadb.database.azure.com', 
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
mysql.RegisterTLSConfig("custom", &tls.Config{RootCAs: rootCertPool})
var connectionString string
connectionString = fmt.Sprintf("%s:%s@tcp(%s:3306)/%s?allowNativePasswords=true&tls=custom",'myadmin@mydemoserver' , 'yourpassword', 'mydemoserver.mariadb.database.azure.com', 'quickstartdb') 
db, _ := sql.Open("mysql", connectionString)
```
### <a name="java-jdbc"></a>Java (JDBC)
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

url = String.format("jdbc:mysql://%s/%s?serverTimezone=UTC&useSSL=true", 'mydemoserver.mariadb.database.azure.com', 'quickstartdb');
properties.setProperty("user", 'myadmin@mydemoserver');
properties.setProperty("password", 'yourpassword');
conn = DriverManager.getConnection(url, properties);
```
### <a name="java-mariadb"></a>Java (MariaDB)
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

url = String.format("jdbc:mariadb://%s/%s?useSSL=true&trustServerCertificate=true", 'mydemoserver.mariadb.database.azure.com', 'quickstartdb');
properties.setProperty("user", 'myadmin@mydemoserver');
properties.setProperty("password", 'yourpassword');
conn = DriverManager.getConnection(url, properties);
```

### <a name="net-mysqlconnector"></a>.NET (MySqlConnector)
```csharp
var builder = new MySqlConnectionStringBuilder
{
    Server = "mydemoserver.mysql.database.azure.com",
    UserID = "myadmin@mydemoserver",
    Password = "yourpassword",
    Database = "quickstartdb",
    SslMode = MySqlSslMode.VerifyCA,
    CACertificateFile = "BaltimoreCyberTrustRoot.crt.pem",
};
using (var connection = new MySqlConnection(builder.ConnectionString))
{
    connection.Open();
}
```

<!-- 
## Next steps
Review various application connectivity options following [Connection libraries for Azure Database for MariaDB](concepts-connection-libraries.md)
-->
