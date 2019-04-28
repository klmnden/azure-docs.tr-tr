---
title: MariaDB için Azure veritabanı uygulamaları bağlama
description: Bu belge, uygulamalarının (C#) ADO.NET, JDBC, Node.js, ODBC, PHP, Python ve Ruby dahil olmak üzere MariaDB için Azure veritabanı ile bağlanmak şu anda desteklenen bağlantı dizelerini listeler.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 70cd25ff63101fa2a477cde2502d5d286b289366
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61039693"
---
# <a name="how-to-connect-applications-to-azure-database-for-mariadb"></a>MariaDB için Azure veritabanı uygulamalara bağlanma
Bu konu, şablonları ve örnekler ile birlikte MariaDB için Azure veritabanı tarafından desteklenmeyen bağlantı dize türleri listeler. Bağlantı dizenizi farklı parametreler ve ayarları olabilir.

- Sertifikayı edinmek için bkz: [SSL yapılandırma](./howto-configure-ssl.md).
- {your_host} [servername].mariadb.database.azure.com =
- {your_user}@{servername} UserID biçimi kimlik doğrulaması için doğru =.  UserId yalnızca kullanırsanız, kimlik doğrulaması başarısız olur.

## <a name="adonet"></a>ADO.NET
```csharp
Server={your_host}; Port=3306; Database={your_database}; Uid={username@servername}; Pwd={your_password}; SslMode=Preferred;
```

Bu örnekte sunucu adı olduğunu `mydemoserver`, veritabanı adı `wpdb`, kullanıcı adı `WPAdmin`, ve parola `mypassword!2`. Sonuç olarak, bağlantı dizesi olmalıdır:

```csharp
Server= "mydemoserver.mariadb.database.azure.com"; Port=3306; Database= "wpdb"; Uid= "WPAdmin@mydemoserver"; Pwd="mypassword!2"; SslMode=Required;
```

## <a name="jdbc"></a>JDBC
```java
String url ="jdbc:mariadb://{your_host}:3306/{your_database}?useSSL=true&trustServerCertificate=true"; myDbConn = DriverManager.getConnection(url, "{username@servername}", {your_password});
```

## <a name="nodejs"></a>Node.js
```javascript
var conn = mysql.createConnection({host: "{your_host}", user: "{your_username}", password: {your_password}, database: {your_database}, port: 3306, ssl:{ca:fs.readFileSync({ca-cert filename})}});
```

## <a name="odbc"></a>ODBC
```cpp
DRIVER={MARIADB ODBC 3.0 Driver}; Server="{your_host}"; Port=3306; Database={your_database}; Uid="{username@servername}"; Pwd={your_password}; sslca={ca-cert filename}; sslverify=1;
```

## <a name="php"></a>PHP
```php
$con=mysqli_init(); mysqli_ssl_set($con, NULL, NULL, {ca-cert filename}, NULL, NULL); mysqli_real_connect($con, "{your_host}", "{username@servername}", {your_password}, {your_database}, 3306);
```

## <a name="python"></a>Python
```python
cnx = mysql.connector.connect(user="{username@servername}", password={your_password}, host="{your_host}", port=3306, database={your_database}, ssl_ca={ca-cert filename}, ssl_verify_cert=true)
```

## <a name="ruby"></a>Ruby
```ruby
client = Mysql2::Client.new(username: "{username@servername}", password: {your_password}, database: {your_database}, host: "{your_host}", port: 3306, sslca:{ca-cert filename}, sslverify:false, sslcipher:'AES256-SHA')
```

## <a name="get-the-connection-string-details-from-the-azure-portal"></a>Bağlantı dizesi ayrıntılarını Azure portaldan alın.
İçinde [Azure portalında](https://portal.azure.com)MariaDB için Azure veritabanı sunucunuza gidin ve ardından **bağlantı dizeleri** Örneğiniz için dize listesini almak için: ![Azure portalında bağlantı dizeleri bölmesi](./media/howto-connection-strings/connection-strings-on-portal.png)

Dizeyi bağlantı parametrelerini sürücü, sunucu ve diğer veritabanı gibi ayrıntıları sağlar. Bu örnekler, veritabanı adı, parola ve benzeri gibi kendi parametrelerini kullanmak için değiştirin. Ardından, kodlarınızı ve uygulamalarınızı sunucusuna bağlanmak için şu dizeyi kullanabilirsiniz.

<!-- 
## Next steps
- For more information about connection libraries, see [Concepts - Connection libraries](./concepts-connection-libraries.md).
- -->
