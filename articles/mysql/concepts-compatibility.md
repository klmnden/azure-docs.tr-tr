---
title: MySQL sürücüleri ve Yönetim Araçları uyumluluğu
description: Bu makalede, MySQL sürücüleri ve MySQL için Azure veritabanı ile uyumlu olan yönetim araçlarını açıklar.
services: mysql
author: ajlam
ms.author: andrela
editor: jasonwhowell
manager: kfile
ms.service: mysql
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 9e56c2bd65f8a9a517a7cdebe02a1d051c689df6
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2018
ms.locfileid: "49985906"
---
# <a name="mysql-drivers-and-management-tools-compatible-with-azure-database-for-mysql"></a>MySQL sürücüleri ve MySQL için Azure veritabanı ile uyumlu yönetim araçları
Bu makalede, MySQL için Azure veritabanı ile uyumlu olan Yönetim Araçları ve sürücüleri açıklanır.

## <a name="mysql-drivers"></a>MySQL sürücüleri
MySQL için Azure veritabanı, dünyanın en popüler topluluk sürümünü MySQL veritabanı kullanır. Bu nedenle, diller ve sürücü programlama birçok farklı ile uyumludur. Hedef üç en son sürümlerini MySQL sürücüleri desteklemek üzere ve işlevsellik ve kullanılabilirliği MySQL sürücülerin sürekli olarak geliştirmek için açık kaynak topluluğundan yazarlar çabalarını devam. Aşağıdaki tabloda, test ve MySQL 5.6 ve 5.7 için Azure veritabanı ile uyumlu olacak şekilde bulunan sürücülerin listesini sağlanır:

| **Sürücü** | **Bağlantılar** | **Uyumlu sürümler** | **Serisiyle uyumsuz sürümleri** | **Notlar** |
| :-------- | :------------------------ | :----------- | :---------------------- | :--------------------------------------- |
| PHP | https://secure.php.net/downloads.php | 5.5 5.6 7.x | 5.3 | SSL MySQLi ile PHP 7.0 bağlantı için bağlantı dizesinde MYSQLI_CLIENT_SSL_DONT_VERIFY_SERVER_CERT ekleyin. <br> ```mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306, NULL, MYSQLI_CLIENT_SSL_DONT_VERIFY_SERVER_CERT);```<br> PDO kümesi: ```PDO::MYSQL_ATTR_SSL_VERIFY_SERVER_CERT``` seçeneği false.|
| .NET | [Github'da MySqlConnector](https://github.com/mysql-net/MySqlConnector) <br> [Nuget paketinden yükleme](https://www.nuget.org/packages/MySqlConnector/) | 0.27 ve sonra | 0.26.5 ve önce | |
| Nodejs |  [Github'da MySQLjs](https://github.com/mysqljs/mysql/releases) <br> Npm yükleme paketi:<br> Çalıştırma `npm install mysql` npm | 2.15 | 2.14.1 ve önce | |
| GİT | https://github.com/go-sql-driver/mysql/releases | 1.3 | 1.2 ve önce | AllowNativePasswords kullanın bağlantı dizesinde true = |
| Python | https://pypi.python.org/pypi/mysql-connector-python | 1.2.3, 2.0, 2.1, 2.2 | 1.2.2 ve önce | |
| Java | https://downloads.mariadb.org/connector-java/ | 2.1 2.0 1.6 | 1.5.5 ve önce | |

## <a name="management-tools"></a>Yönetim Araçları
Bir veritabanı yönetim aracı için de uyumluluk avantajı genişletir. Veritabanı işleme kullanıcı izinleri sınırlar içinde çalıştığı sürece, mevcut araçlarınızı, MySQL için Azure veritabanı ile çalışmaya devam etmelidir. Test ve MySQL 5.6 ve 5.7 için Azure veritabanı ile uyumlu olacak şekilde bulunan üç yaygın veritabanı yönetim araçları aşağıdaki tabloda listelenmiştir:

|                                     | **MySQL Workbench 6.x ve üstü** | **Navicat 12** | **PHPMyAdmin 4.x ve üstü** |
| :---------------------------------- | :----------------------------- | :------------- | :-------------------------|
| Oluşturma, güncelleştirme, okuma, yazma, silme | X | X | X |
| SSL bağlantısı | X | X | X |
| SQL sorgu otomatik tamamlama | X | X |  |
| İçeri ve dışarı aktarma verileri | X | X | X |
| Birden çok biçimde dışarı aktarma | X | X | X |
| Yedekleme ve Geri Yükleme |  | X |  |
| Sunucu parametreleri görüntüleme | X | X | X |
| İstemci bağlantılarını görüntüle | X | X | X |
