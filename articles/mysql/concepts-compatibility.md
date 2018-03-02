---
title: "MySQL sürücüleri ve Yönetim Araçları uyumluluğu"
description: "Bu makalede, Azure veritabanı için MySQL ile uyumlu olan Yönetim Araçları ve MySQL sürücüleri açıklanmaktadır."
services: mysql
author: ajlam
ms.author: andrela
editor: jasonwhowell
manager: kfile
ms.service: mysql-database
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 5fc13ef07b61feb9e9fdd73123a09daa61f6aaf3
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="mysql-drivers-and-management-tools-compatible-with-azure-database-for-mysql"></a>MySQL sürücüleri ve Yönetim Araçları Azure veritabanı için MySQL ile uyumlu
Bu makalede, Azure veritabanı için MySQL ile uyumlu olan Yönetim Araçları ve sürücüleri açıklanmaktadır.

## <a name="mysql-drivers"></a>MySQL sürücüleri
Azure veritabanı için MySQL dünyanın en popüler community sürümü MySQL veritabanı kullanır. Bu nedenle, programlama dilleri ve sürücüleri çeşitli ile uyumlu değil. Hedef üç en son sürümleri MySQL sürücüleri desteklemek için ve açık kaynak topluluğu yazarların çabayla sürekli işlevselliği ve MySQL sürücüleri kullanılabilirliğini artırmak için devam edin. Aşağıdaki tabloda, test ve Azure veritabanı için MySQL 5.6 ve 5.7 ile uyumlu olacak şekilde bulunan sürücülerin listesini sağlanır:

| **Driver** | **Bağlantılar** | **Uyumlu sürümleri** | **Ekseninin sürümleri** | **Notlar** |
| :-------- | :------------------------ | :----------- | :---------------------- | :--------------------------------------- |
| PHP | http://php.net/downloads.php | 5.5 5.6 7.x | 5.3 | SSL MySQLi ile PHP 7.0 bağlantı için bağlantı dizesinde MYSQLI_CLIENT_SSL_DONT_VERIFY_SERVER_CERT ekleyin. <br> ```mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306, NULL, MYSQLI_CLIENT_SSL_DONT_VERIFY_SERVER_CERT);```<br> PDO kümesi: ```PDO::MYSQL_ATTR_SSL_VERIFY_SERVER_CERT``` seçeneği false.|
| .NET | [Github'da MySqlConnector](https://github.com/mysql-net/MySqlConnector) <br> [Nuget paket yüklemesi](https://www.nuget.org/packages/MySqlConnector/) | 0.27 ve sonra | 0.26.5 ve önce | |
| Nodejs |  [Github'da MySQLjs](https://github.com/mysqljs/mysql/releases) <br> NPM yükleme paketinden:<br> Çalıştırma `npm install mysql` NPM gelen | 2.15 | 2.14.1 ve önce | |
| GİT | https://github.com/go-sql-driver/mysql/releases | 1.3 | 1.2 ve önce | AllowNativePasswords kullanmak = true bağlantı dizesindeki |
| Python | https://pypi.python.org/pypi/mysql-connector-python | 1.2.3, 2.0, 2.1, 2.2 | 1.2.2 ve önce | |
| Java | https://downloads.mariadb.org/connector-java/ | 2.1 2.0 1.6 | 1.5.5 ve önce | |

## <a name="management-tools"></a>Yönetim Araçları
Bir veritabanı yönetim aracı için de uyumluluk avantajı genişletir. Veritabanı işleme kullanıcı izinlerini sınırlar içinde çalıştığı sürece, varolan araçlarınızı MySQL için Azure veritabanı ile çalışmaya devam etmesi gerekir. Test ve Azure veritabanı için MySQL 5.6 ve 5.7 ile uyumlu olacak şekilde bulunan üç genel bir veritabanı yönetim aracı aşağıdaki tabloda listelenmiştir:

|                                     | **MySQL çalışma ekranı 6.x ve sonraki sürümü** | **Navicat 12** | **PHPMyAdmin 4.x ve sonraki sürümü** |
| :---------------------------------- | :----------------------------- | :------------- | :-------------------------|
| Oluştur, Güncelleştir, okuma, yazma, silme | X | X | X |
| SSL bağlantısı | X | X | X |
| SQL sorgu otomatik tamamlama | X | X |  |
| İçeri ve dışarı aktarma veri | X | X | X |
| Birden çok biçim dışarı aktarma | X | X | X |
| Yedekleme ve Geri Yükleme |  | X |  |
| Sunucu parametreleri görüntüleme | X | X | X |
| İstemci bağlantılarını görüntüleme | X | X | X |
