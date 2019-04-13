---
title: MySQL için Azure veritabanı uygulamaları bağlama
description: Bu belge, uygulamalarının (C#) ADO.NET, JDBC, Node.js, ODBC, PHP, Python ve Ruby dahil olmak üzere MySQL için Azure veritabanı ile bağlanmak şu anda desteklenen bağlantı dizelerini listeler.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 02/28/2018
ms.openlocfilehash: 503b7764ba7958a5448903b217b00e204ffcbf61
ms.sourcegitcommit: 1c2cf60ff7da5e1e01952ed18ea9a85ba333774c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/12/2019
ms.locfileid: "59527413"
---
# <a name="how-to-connect-applications-to-azure-database-for-mysql"></a>MySQL için Azure veritabanı uygulamalara bağlanma
Bu konu, şablonları ve örnekler ile birlikte MySQL için Azure veritabanı tarafından desteklenmeyen bağlantı dize türleri listeler. Bağlantı dizenizi farklı parametreler ve ayarları olabilir.

- Sertifikayı edinmek için bkz: [SSL yapılandırma](./howto-configure-ssl.md).
- {your_host} = \<servername>.mysql.database.azure.com
- {your_user}@{servername} UserID biçimi kimlik doğrulaması için doğru =.  UserId yalnızca kullanırsanız, kimlik doğrulaması başarısız olur.

## <a name="adonet"></a>ADO.NET
```ado.net
Server={your_host};Port={your_port};Database={your_database};Uid={username@servername};Pwd={your_password};[SslMode=Required;]
```

Bu örnekte sunucu adı olduğunu `mydemoserver`, veritabanı adı `wpdb`, kullanıcı adı `WPAdmin`, ve parola `mypassword!2`. Sonuç olarak, bağlantı dizesi olmalıdır:

```ado.net
Server= "mydemoserver.mysql.database.azure.com"; Port=3306; Database= "wpdb"; Uid= "WPAdmin@mydemoserver"; Pwd="mypassword!2"; SslMode=Required;
```

## <a name="jdbc"></a>JDBC
```jdbc
String url ="jdbc:mysql://%s:%s/%s[?verifyServerCertificate=true&useSSL=true&requireSSL=true]",{your_host},{your_port},{your_database}"; myDbConn = DriverManager.getConnection(url, {username@servername}, {your_password}";
```

## <a name="nodejs"></a>Node.js
```node.js
var conn = mysql.createConnection({host: {your_host}, user: {username@servername}, password: {your_password}, database: {your_database}, Port: {your_port}[, ssl:{ca:fs.readFileSync({ca-cert filename})}}]);
```

## <a name="odbc"></a>ODBC
```odbc
DRIVER={MySQL ODBC 5.3 UNICODE Driver};Server={your_host};Port={your_port};Database={your_database};Uid={username@servername};Pwd={your_password}; [sslca={ca-cert filename}; sslverify=1; Option=3;]
```

## <a name="php"></a>PHP
```php
$con=mysqli_init(); [mysqli_ssl_set($con, NULL, NULL, {ca-cert filename}, NULL, NULL);] mysqli_real_connect($con, {your_host}, {username@servername}, {your_password}, {your_database}, {your_port});
```

## <a name="python"></a>Python
```python
cnx = mysql.connector.connect(user={username@servername}, password={your_password}, host={your_host}, port={your_port}, database={your_database}[, ssl_ca={ca-cert filename}, ssl_verify_cert=true])
```

## <a name="ruby"></a>Ruby
```ruby
client = Mysql2::Client.new(username: {username@servername}, password: {your_password}, database: {your_database}, host: {your_host}, port: {your_port}[, sslca:{ca-cert filename}, sslverify:false, sslcipher:'AES256-SHA'])
```

## <a name="get-the-connection-string-details-from-the-azure-portal"></a>Bağlantı dizesi ayrıntılarını Azure portaldan alın.
İçinde [Azure portalında](https://portal.azure.com), MySQL için Azure veritabanı sunucunuza gidin ve ardından **bağlantı dizeleri** Örneğiniz için dize listesini almak için: ![Azure portalında bağlantı dizeleri bölmesi](./media/howto-connection-strings/connection-strings-on-portal.png)

Dizeyi bağlantı parametrelerini sürücü, sunucu ve diğer veritabanı gibi ayrıntıları sağlar. Bu örnekler, veritabanı adı, parola ve benzeri gibi kendi parametrelerini kullanmak için değiştirin. Ardından, kodlarınızı ve uygulamalarınızı sunucusuna bağlanmak için şu dizeyi kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
- Bağlantı kitaplıkları hakkında daha fazla bilgi için bkz. [kavramlar - bağlantı kitaplıkları](./concepts-connection-libraries.md).
