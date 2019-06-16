---
title: MariaDB sürücüleri ve Yönetim Araçları uyumluluğu için Azure veritabanı
description: Bu makalede, MariaDB sürücüleri ve MariaDB için Azure veritabanı ile uyumlu olan yönetim araçlarını açıklar.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 03/19/2019
ms.openlocfilehash: 7a3d9a5f87a565625052fc54e3ecccc99fd928a7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61386840"
---
# <a name="mariadb-drivers-and-management-tools-compatible-with-azure-database-for-mariadb"></a>MariaDB sürücüleri ve araçlarını MariaDB için Azure veritabanı ile uyumlu

Bu makalede, sürücüler ve MariaDB için Azure veritabanı ile uyumlu olan yönetim araçlarını açıklar.

## <a name="mariadb-drivers"></a>MariaDB sürücüleri

MariaDB için Azure veritabanı, MariaDB server topluluk sürümünü kullanır. Bu nedenle, diller ve sürücü programlama birçok farklı ile uyumludur. Mariadb'nin API ve protokol MySQL tarafından Kullanılanlar ile uyumludur. Bu MySQL ile bağlayıcılardan ile MariaDB çalışması gerektiği anlamına gelir.

Hedef üç en son sürümlerini MariaDB sürücüleri desteklemek üzere ve işlevsellik ve kullanılabilirliği MariaDB sürücülerin sürekli olarak geliştirmek için açık kaynak topluluğundan yazarlar çabalarını devam. Aşağıdaki tabloda, test ve MariaDB 10.2 için Azure veritabanı ile uyumlu olacak şekilde bulunan sürücülerin listesini sağlanır:

**Sürücü** | **Bağlantılar** | **Uyumlu sürümler** | **Uyumsuz sürümleri** | **Notlar**
---|---|---|---|---
PHP | https://secure.php.net/downloads.php | 5.5, 5.6, 7.x | 5.3 | SSL MySQLi ile PHP 7.0 bağlantı için bağlantı dizesinde MYSQLI_CLIENT_SSL_DONT_VERIFY_SERVER_CERT ekleyin. <br> ```mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306, NULL, MYSQLI_CLIENT_SSL_DONT_VERIFY_SERVER_CERT);```<br> PDO kümesi: ```PDO::MYSQL_ATTR_SSL_VERIFY_SERVER_CERT``` seçeneği false.
.NET | [Github'da MySqlConnector](https://github.com/mysql-net/MySqlConnector) <br> [Nuget paketinden yükleme](https://www.nuget.org/packages/MySqlConnector/) | 0.27 ve sonra | 0.26.5 ve önce |
MySQL Connector/NET | [MySQL Connector/NET](https://github.com/mysql/mysql-connector-net) | 8.0, 7.0, 6.10 |  | Bir kodlama hata bağlantıları bazı UTF8 olmayan Windows sistemlerinde başarısız olmasına neden olabilir.
Node.js |  [Github'da MySQLjs](https://github.com/mysqljs/mysql/) <br> Npm yükleme paketi:<br> Çalıştırma `npm install mysql` npm | 2.15 | 2.14.1 ve önce
GİT | https://github.com/go-sql-driver/mysql/releases | 1.3, 1.4 | 1.2 ve önce | Kullanım `allowNativePasswords=true` sürüm 1.3 için bağlantı dizesinde. Sürüm 1.4 bir düzeltme içeriyor ve `allowNativePasswords=true` artık gerekli değildir.
Python | https://pypi.python.org/pypi/mysql-connector-python | 1.2.3, 2.0, 2.1, 2.2 | 1.2.2 ve önce |
Java | https://downloads.mariadb.org/connector-java/ | 2.1, 2.0, 1.6 | 1.5.5 ve önce |

## <a name="management-tools"></a>Yönetim Araçları

Bir veritabanı yönetim aracı için de uyumluluk avantajı genişletir. Veritabanı işleme kullanıcı izinleri sınırlar içinde çalıştığı sürece, mevcut araçlarınızı MariaDB için Azure veritabanı ile çalışmaya devam etmelidir. Test ve MariaDB 10.2 için Azure veritabanı ile uyumlu olacak şekilde bulunan üç yaygın veritabanı yönetim araçları aşağıdaki tabloda listelenmiştir:

| | **MySQL Workbench 6.x ve üstü** | **Navicat 12** | **PHPMyAdmin 4.x ve üstü**
---|---|---|---
Oluşturma, güncelleştirme, okuma, yazma, silme | X | X | X
SSL bağlantısı | X | X | X
SQL sorgu otomatik tamamlama | X | X |
İçeri ve dışarı aktarma verileri | X | X | X
Birden çok biçimde dışarı aktarma | X | X | X
Yedekleme ve Geri Yükleme |  | X |
Sunucu parametreleri görüntüleme | X | X | X
İstemci bağlantılarını görüntüle | X | X | X

## <a name="next-steps"></a>Sonraki adımlar

- [MariaDB için Azure Veritabanı bağlantı sorunlarını giderme](howto-troubleshoot-common-connection-issues.md)