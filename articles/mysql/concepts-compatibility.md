---
title: "MySQL sürücüleri ve Yönetim Araçları uyumluluk | Microsoft Docs"
description: "MySQL sürücüleri ve Yönetim Araçları Azure veritabanı için MySQL ile uyumlu"
services: mysql
author: seanli1988
ms.author: seanli
editor: jasonwhowell
manager: jhubbard
ms.service: mysql-database
ms.topic: article
ms.date: 10/27/2017
ms.openlocfilehash: 4df0dcc7d0f2bde24c1a7e12eea6fa142612a0e7
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
---
## <a name="mysql-drivers"></a>MySQL sürücüleri
Azure veritabanı için MySQL dünyanın en popüler community sürümü MySQL veritabanı kullanır. Bu nedenle, programlama dilleri ve sürücüleri çeşitli ile uyumlu değil. Hedef üç en son sürümleri MySQL sürücüleri desteklemek için ve açık kaynak topluluğu yazarların çabayla sürekli işlevselliği ve MySQL sürücüleri kullanılabilirliğini artırmak için devam edin. Aşağıdaki tabloda, test ve Azure veritabanı için MySQL 5.6 ve 5.7 ile uyumlu olacak şekilde bulunan sürücülerin listesini sağlanır:

| **Sürücü** | **Bağlantılar** | **Uyumlu sürümleri** | **Ekseninin sürümleri** | **Notlar** |
| :-------- | :------------------------ | :----------- | :---------------------- | :--------------------------------------- |
| PHP | http://php.NET/downloads.php | 5.5 5.6 7.x | 5.3 | SSL MySQLi ile PHP 7.0 bağlantı için bağlantı dizesinde MYSQLI_CLIENT_SSL_DONT_VERIFY_SERVER_CERT ekleyin. <br> ```mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306, NULL, MYSQLI_CLIENT_SSL_DONT_VERIFY_SERVER_CERT);```<br> PDO kümesi: ```PDO::MYSQL_ATTR_SSL_VERIFY_SERVER_CERT``` seçeneği false.|
| .NET | [MySqlConnector github'da]: https://github.com/mysql-net/MySqlConnector/releases <br> [Yükleme paketinden Nuget]:<br> https://www.nuget.org/Packages/MySqlConnector/ | 0.27 ve sonra | 0.26.5 ve önce | |
| Nodejs |  [MySQLjs github'da]:<br> https://github.com/mysqljs/MySQL/Releases <br> [Yükleme paketinden NPM]:<br> "Npm yükleme mysql" NPM çalıştırın | 2.15 | 2.14.1 ve önce | |
| GİT | https://github.com/go-SQL-Driver/MySQL/Releases | 1.3 | 1.2 ve önce | AllowNativePasswords kullanmak = true bağlantı dizesindeki |
| Python | https://pypi.Python.org/pypi/MySQL-Connector-Python | 1.2.3, 2.0, 2.1, 2.2 | 1.2.2 ve önce | |
| Java | https://downloads.mariadb.org/Connector-Java/ | 2.1 2.0 1.6 | 1.5.5 ve önce | |

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