---
title: "PHP'den MySQL için Azure Veritabanı'na bağlanma | Microsoft Docs"
description: "Bu hızlı başlangıçta, MySQL için Azure Veritabanı'na bağlanmak ve buradan veri sorgulamak için kullanabileceğiniz birkaç PHP kod örneği sağlanmıştır."
services: mysql
author: mswutao
ms.author: wuta
manager: jhubbard
editor: jasonwhowell
ms.service: mysql
ms.custom: mvc
ms.topic: quickstart
ms.date: 09/22/2017
ms.openlocfilehash: 2af5871e8bf67070c83b5faebc1f9e44b0de609e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-database-for-mysql-use-php-to-connect-and-query-data"></a>MySQL için Azure Veritabanı: PHP'yi kullanarak bağlanma ve veri sorgulama
Bu hızlı başlangıçta [PHP](http://php.net/manual/intro-whatis.php) uygulaması kullanarak MySQL için Azure Veritabanı'na nasıl bağlanacağınız gösterilmiştir. Ayrıca veritabanında veri sorgulamak, eklemek, güncelleştirmek ve silmek için SQL deyimlerini nasıl kullanacağınız da gösterilmiştir. Bu konu ile PHP kullanarak geliştirme tanıdık ve MySQL için Azure veritabanı ile çalışmaya yeni varsayar.

## <a name="prerequisites"></a>Ön koşullar
Bu hızlı başlangıçta, başlangıç noktası olarak şu kılavuzlardan birinde oluşturulan kaynaklar kullanılmaktadır:
- [Azure portalını kullanarak MySQL için Azure Veritabanı sunucusu oluşturma](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [Azure CLI kullanarak MySQL için Azure Veritabanı sunucusu oluşturma](./quickstart-create-mysql-server-database-using-azure-cli.md)

## <a name="install-php"></a>PHP'yi yükleme
PHP'yi kendi sunucunuza yükleyin veya PHP içeren bir Azure [web uygulaması](../app-service/app-service-web-overview.md) oluşturun.

### <a name="macos"></a>macOS
- Karşıdan [PHP 7.1.4 sürüm](http://php.net/downloads.php).
- PHP yükleme ve başvurulacak [PHP el ile](http://php.net/manual/install.macosx.php) daha fazla yapılandırma.

### <a name="linux-ubuntu"></a>Linux (Ubuntu)
- Karşıdan [PHP 7.1.4 olmayan iş parçacığı güvenli (x64) sürüm](http://php.net/downloads.php).
- PHP yükleme ve başvurulacak [PHP el ile](http://php.net/manual/install.unix.php) daha fazla yapılandırma.

### <a name="windows"></a>Windows
- Karşıdan [PHP 7.1.4 olmayan iş parçacığı güvenli (x64) sürüm](http://windows.php.net/download#php-7.1).
- PHP yükleme ve başvurulacak [PHP el ile](http://php.net/manual/install.windows.php) daha fazla yapılandırma.

## <a name="get-connection-information"></a>Bağlantı bilgilerini alma
MySQL için Azure Veritabanı'na bağlanmak üzere gereken bağlantı bilgilerini alın. Tam sunucu adına ve oturum açma kimlik bilgilerine ihtiyacınız vardır.

1. [Azure Portal](https://portal.azure.com/)’da oturum açın.
2. Sol bölmede **Tüm kaynaklar**’a tıklayın ve ardından oluşturduğunuz sunucuyu arayın (örneğin, **myserver4demo**).
3. Sunucunun adına tıklayın.
4. Sunucunun seçin **özellikleri** sayfasında ve sonra Not **sunucu adı** ve **sunucu yönetici oturum açma adı**.
 ![MySQL için Azure Veritabanı sunucu adı](./media/connect-php/1_server-properties-name-login.png)
5. Sunucu oturum açma bilgilerinizi unutursanız gidin **genel bakış** sunucu yönetici oturum açma adı görüntülemek için sayfa ve gerekirse, parola sıfırlama.

## <a name="connect-and-create-a-table"></a>Bağlanma ve tablo oluşturma
Bağlanmak ve kullanarak bir tablo oluşturmak için aşağıdaki kodu kullanın **CREATE TABLE** SQL deyimi. 

Kod, PHP'de bulunan **MySQL Improved extension** (mysqli) sınıfını kullanır. Kod yöntemlerini çağıran [mysqli_init](http://php.net/manual/mysqli.init.php) ve [mysqli_real_connect](http://php.net/manual/mysqli.real-connect.php) Mysql'e bağlanmak için. Ardından sorgu çalıştırmak için [mysqli_query](http://php.net/manual/mysqli.query.php) yöntemini çağırır. Daha sonra bağlantıyı kapatmak için [mysqli_close](http://php.net/manual/mysqli.close.php) yöntemini çağırır.

host, username, password ve db_name parametre değerlerini kendi değerlerinizle değiştirin. 

```php
<?php
$host = 'myserver4demo.mysql.database.azure.com';
$username = 'myadmin@myserver4demo';
$password = 'your_password';
$db_name = 'your_database';

//Establishes the connection
$conn = mysqli_init();
mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
}

// Run the create table query
if (mysqli_query($conn, '
CREATE TABLE Products (
`Id` INT NOT NULL AUTO_INCREMENT ,
`ProductName` VARCHAR(200) NOT NULL ,
`Color` VARCHAR(50) NOT NULL ,
`Price` DOUBLE NOT NULL ,
PRIMARY KEY (`Id`)
);
')) {
printf("Table created\n");
}

//Close the connection
mysqli_close($conn);
?>
```

## <a name="insert-data"></a>Veri ekleme
Aşağıdaki kodu kullanın ve kullanarak veri ekleme bir **Ekle** SQL deyimi.

Kod, PHP'de bulunan **MySQL Improved extension** (mysqli) sınıfını kullanır. Kod, [mysqli_prepare](http://php.net/manual/mysqli.prepare.php) yöntemini kullanarak hazır bir INSERT deyimi oluşturur, ardından [mysqli_stmt_bind_param](http://php.net/manual/mysqli-stmt.bind-param.php) yöntemini kullanarak, eklenen her bir sütun değerine ilişkin parametreleri bağlar. Yöntemini kullanarak kod deyimini çalıştırır [mysqli_stmt_execute](http://php.net/manual/mysqli-stmt.execute.php) ve daha sonra yöntemini kullanarak deyimi kapatır [mysqli_stmt_close](http://php.net/manual/mysqli-stmt.close.php).

host, username, password ve db_name parametre değerlerini kendi değerlerinizle değiştirin. 

```php
<?php
$host = 'myserver4demo.mysql.database.azure.com';
$username = 'myadmin@myserver4demo';
$password = 'your_password';
$db_name = 'your_database';

//Establishes the connection
$conn = mysqli_init();
mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
}

//Create an Insert prepared statement and run it
$product_name = 'BrandNewProduct';
$product_color = 'Blue';
$product_price = 15.5;
if ($stmt = mysqli_prepare($conn, "INSERT INTO Products (ProductName, Color, Price) VALUES (?, ?, ?)")) {
mysqli_stmt_bind_param($stmt, 'ssd', $product_name, $product_color, $product_price);
mysqli_stmt_execute($stmt);
printf("Insert: Affected %d rows\n", mysqli_stmt_affected_rows($stmt));
mysqli_stmt_close($stmt);
}

// Close the connection
mysqli_close($conn);
?>
```

## <a name="read-data"></a>Verileri okuma
Aşağıdaki kodu kullanın ve kullanarak verileri okuyun bir **seçin** SQL deyimi.  Kod, PHP'de bulunan **MySQL Improved extension** (mysqli) sınıfını kullanır. Kod yöntemini kullanır [mysqli_query](http://php.net/manual/mysqli.query.php) yöntemi ve sql sorgusu gerçekleştirmek [mysqli_fetch_assoc](http://php.net/manual/mysqli-result.fetch-assoc.php) elde edilen satırları getirilemedi.

host, username, password ve db_name parametre değerlerini kendi değerlerinizle değiştirin. 

```php
<?php
$host = 'myserver4demo.mysql.database.azure.com';
$username = 'myadmin@myserver4demo';
$password = 'your_password';
$db_name = 'your_database';

//Establishes the connection
$conn = mysqli_init();
mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
}

//Run the Select query
printf("Reading data from table: \n");
$res = mysqli_query($conn, 'SELECT * FROM Products');
while ($row = mysqli_fetch_assoc($res)) {
var_dump($row);
}

//Close the connection
mysqli_close($conn);
?>
```

## <a name="update-data"></a>Verileri güncelleştirme
Bağlanıp kullanarak verileri güncelleştirmek için aşağıdaki kodu kullanın bir **güncelleştirme** SQL deyimi.

Kod, PHP'de bulunan **MySQL Improved extension** (mysqli) sınıfını kullanır. Kod, [mysqli_prepare](http://php.net/manual/mysqli.prepare.php) yöntemini kullanarak hazır bir UPDATE deyimi oluşturur, ardından [mysqli_stmt_bind_param](http://php.net/manual/mysqli-stmt.bind-param.php) yöntemini kullanarak, güncelleştirilen her bir sütun değerine ilişkin parametreleri bağlar. Yöntemini kullanarak kod deyimini çalıştırır [mysqli_stmt_execute](http://php.net/manual/mysqli-stmt.execute.php) ve daha sonra yöntemini kullanarak deyimi kapatır [mysqli_stmt_close](http://php.net/manual/mysqli-stmt.close.php).

host, username, password ve db_name parametre değerlerini kendi değerlerinizle değiştirin. 

```php
<?php
$host = 'myserver4demo.mysql.database.azure.com';
$username = 'myadmin@myserver4demo';
$password = 'your_password';
$db_name = 'your_database';

//Establishes the connection
$conn = mysqli_init();
mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
}

//Run the Update statement
$product_name = 'BrandNewProduct';
$new_product_price = 15.1;
if ($stmt = mysqli_prepare($conn, "UPDATE Products SET Price = ? WHERE ProductName = ?")) {
mysqli_stmt_bind_param($stmt, 'ds', $new_product_price, $product_name);
mysqli_stmt_execute($stmt);
printf("Update: Affected %d rows\n", mysqli_stmt_affected_rows($stmt));

//Close the connection
mysqli_stmt_close($stmt);
}

mysqli_close($conn);
?>
```


## <a name="delete-data"></a>Verileri silme
Aşağıdaki kodu kullanın ve kullanarak verileri okuyun bir **silmek** SQL deyimi. 

Kod, PHP'de bulunan **MySQL Improved extension** (mysqli) sınıfını kullanır. Kod, [mysqli_prepare](http://php.net/manual/mysqli.prepare.php) yöntemini kullanarak hazır bir DELETE deyimi oluşturur, ardından [mysqli_stmt_bind_param](http://php.net/manual/mysqli-stmt.bind-param.php) yöntemini kullanarak deyimdeki WHERE yan tümcesine ilişkin parametreleri bağlar. Yöntemini kullanarak kod deyimini çalıştırır [mysqli_stmt_execute](http://php.net/manual/mysqli-stmt.execute.php) ve daha sonra yöntemini kullanarak deyimi kapatır [mysqli_stmt_close](http://php.net/manual/mysqli-stmt.close.php).

host, username, password ve db_name parametre değerlerini kendi değerlerinizle değiştirin. 

```php
<?php
$host = 'myserver4demo.mysql.database.azure.com';
$username = 'myadmin@myserver4demo';
$password = 'your_password';
$db_name = 'your_database';

//Establishes the connection
$conn = mysqli_init();
mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
}

//Run the Delete statement
$product_name = 'BrandNewProduct';
if ($stmt = mysqli_prepare($conn, "DELETE FROM Products WHERE ProductName = ?")) {
mysqli_stmt_bind_param($stmt, 's', $product_name);
mysqli_stmt_execute($stmt);
printf("Delete: Affected %d rows\n", mysqli_stmt_affected_rows($stmt));
mysqli_stmt_close($stmt);
}

//Close the connection
mysqli_close($conn);
?>
```

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Azure'da PHP ve MySQL web uygulaması oluşturma](../app-service/app-service-web-tutorial-php-mysql.md?toc=%2fazure%2fmysql%2ftoc.json)
