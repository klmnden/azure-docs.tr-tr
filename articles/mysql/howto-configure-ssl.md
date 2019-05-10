---
title: Güvenli bir MySQL için Azure veritabanı'na bağlanmak için SSL bağlantısı yapılandırma
description: MySQL ve SSL bağlantıları doğru bir şekilde kullanmak için ilişkili uygulamalar için Azure veritabanı düzgün bir şekilde nasıl yapılandıracağınızı öğrenmek için yönergeler
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 01/24/2019
ms.openlocfilehash: f7346d5f40e0fe7dd4dbe892e96549f7ff181cb2
ms.sourcegitcommit: 8fc5f676285020379304e3869f01de0653e39466
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65511003"
---
# <a name="configure-ssl-connectivity-in-your-application-to-securely-connect-to-azure-database-for-mysql"></a>Güvenli bir MySQL için Azure veritabanı'na bağlanmak üzere uygulamanızda SSL bağlantısı yapılandırma
MySQL için Azure veritabanı, Güvenli Yuva Katmanı (SSL) kullanarak istemci uygulamaları için Azure veritabanınızı MySQL sunucusuna bağlanmayı destekler. Veritabanı sunucunuzla istemci uygulamalarınız arasında SSL bağlantılarının zorunlu tutulması, sunucuya uygulamanız arasındaki veri akışını şifreleyerek "bağlantıyı izinsiz izleme" saldırılarına karşı korumaya yardımcı olur.

## <a name="step-1-obtain-ssl-certificate"></a>1. Adım: SSL sertifikası alın
Azure veritabanınızı MySQL sunucusunun SSL üzerinden iletişim kurmak için gereken sertifikayı indirin [ https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem ](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem) ve sertifika dosyasını (Bu öğreticide c:\ssl örneğin) yerel sürücünüze kaydedin.
**Microsoft Internet Explorer ve Microsoft Edge için:** İndirme tamamlandıktan sonra sertifika BaltimoreCyberTrustRoot.crt.pem için yeniden adlandırın.

## <a name="step-2-bind-ssl"></a>2. Adım: SSL bağlama
### <a name="connecting-to-server-using-the-mysql-workbench-over-ssl"></a>SSL üzerinden MySQL Workbench kullanarak sunucuya bağlanma
MySQL Workbench, SSL üzerinden güvenli bir şekilde bağlanmak için yapılandırın. Yeni bağlantı oluştur iletişim kutusu gidin **SSL** sekmesi. İçinde **SSL CA dosyası:** dosya konumunu girin **BaltimoreCyberTrustRoot.crt.pem**. 
![özelleştirilmiş kutucuk kaydetme](./media/howto-configure-ssl/mysql-workbench-ssl.png) varolan bağlantılar için SSL bağlantısı simgeye tıklanarak bağlayın ve Düzenle'yi seçin. Ardından gidin **SSL** sekme ve sertifika dosyası bağlayın.

### <a name="connecting-to-server-using-the-mysql-cli-over-ssl"></a>SSL üzerinden MySQL CLI'yı kullanarak sunucuya bağlanma
SSL sertifikası bağlama başka bir yolu, aşağıdaki komutları çalıştırarak MySQL komut satırı arabirimini kullanmaktır. 

```bash
mysql.exe -h mydemoserver.mysql.database.azure.com -u Username@mydemoserver -p --ssl-mode=REQUIRED --ssl-ca=c:\ssl\BaltimoreCyberTrustRoot.crt.pem
```

> [!NOTE]
> Windows üzerinde MySQL komut satırı arabirimi kullanarak, bir hata alabilir `SSL connection error: Certificate signature check failed`. Bu meydana gelirse, değiştirin `--ssl-mode=REQUIRED --ssl-ca={filepath}` parametrelerle `--ssl`.

## <a name="step-3--enforcing-ssl-connections-in-azure"></a>3. adım:  Azure'da SSL bağlantılarının zorlanması 
### <a name="using-the-azure-portal"></a>Azure portalını kullanma
Azure portalını kullanarak MySQL için Azure veritabanı sunucunuza ziyaret edin ve ardından **bağlantı güvenliği**. İki durumlu düğmeyi etkinleştirme veya devre dışı kullanın **SSL'yi zorunlu bağlantı** ayarlama ve ardından **Kaydet**. Microsoft, her zaman etkinleştirmeyi önerir **SSL'yi zorunlu bağlantı** için Gelişmiş güvenlik ayarı.
![ssl etkinleştir](./media/howto-configure-ssl/enable-ssl.png)

### <a name="using-azure-cli"></a>Azure CLI’yı kullanma
Etkinleştirmek veya devre dışı bırakabileceğiniz **ssl zorlama** etkin veya devre dışı değerleri sırasıyla Azure CLI'yi kullanarak parametre.
```azurecli-interactive
az mysql server update --resource-group myresource --name mydemoserver --ssl-enforcement Enabled
```

## <a name="step-4-verify-the-ssl-connection"></a>4. Adım: SSL bağlantısını doğrulama
Mysql yürütme **durumu** SSL kullanarak MySQL sunucusuna bağladınız doğrulamak için komut:
```dos
mysql> status
```
Bağlantı göstermelidir çıkış inceleyerek şifrelenir onaylayın:  **SSL: AES256 SHA şifredir kullanılıyor** 

## <a name="sample-code"></a>Örnek kod
Güvenli bir bağlantı için Azure veritabanı için MySQL SSL üzerinden uygulamanızı oluşturmak için aşağıdaki kod örneklerine bakın:

### <a name="php"></a>PHP
```php
$conn = mysqli_init();
mysqli_ssl_set($conn,NULL,NULL, "/var/www/html/BaltimoreCyberTrustRoot.crt.pem", NULL, NULL) ; 
mysqli_real_connect($conn, 'mydemoserver.mysql.database.azure.com', 'myadmin@mydemoserver', 'yourpassword', 'quickstartdb', 3306, MYSQLI_CLIENT_SSL);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
}
```
### <a name="php-using-pdo"></a>PHP (PDO kullanarak)
```phppdo
$options = array(
    PDO::MYSQL_ATTR_SSL_CA => '/var/www/html/BaltimoreCyberTrustRoot.crt.pem'
);
$db = new PDO('mysql:host=mydemoserver.mysql.database.azure.com;port=3306;dbname=databasename', 'username@mydemoserver', 'yourpassword', $options);
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
mysql.RegisterTLSConfig("custom", &tls.Config{RootCAs: rootCertPool})
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

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki çeşitli uygulama bağlantı seçenekleri gözden [MySQL için Azure veritabanı için bağlantı kitaplıkları](concepts-connection-libraries.md)
