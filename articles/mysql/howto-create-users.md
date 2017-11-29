---
title: "Kullanıcılar Azure veritabanı'nda MySQL sunucusu için oluşturun. | Microsoft Docs"
description: "Bu makalede, MySQL sunucusu için bir Azure veritabanıyla etkileşim kurmak için yeni kullanıcı hesapları nasıl oluşturabileceğiniz açıklanmaktadır."
services: mysql
author: jasonwhowell
ms.author: jasonh
editor: jasonwhowell
manager: jhubbard
ms.service: mysql-database
ms.topic: article
ms.date: 11/27/2017
ms.openlocfilehash: 8adb74e11570ac60ad3b898b737cff4699f2bbf1
ms.sourcegitcommit: 310748b6d66dc0445e682c8c904ae4c71352fef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2017
---
# <a name="create-users-in-azure-database-for-mysql-server"></a>Kullanıcılar Azure veritabanı'nda MySQL sunucusu için oluşturma 
Bu makalede, MySQL sunucusu için bir Azure veritabanındaki kullanıcıların nasıl oluşturabileceğiniz açıklanmaktadır.

MySQL için öncelikle Azure veritabanınızı oluşturduğunuzda, bir Sunucu Yöneticisi oturum açma kullanıcı adı ve parola sağlanan. Daha fazla bilgi için izlediğiniz [Hızlı Başlangıç](quickstart-create-mysql-server-database-using-azure-portal.md). Sunucu yönetici oturum açma kullanıcı adınızı Azure portalından bulabilirsiniz.

Sunucu yönetici kullanıcısı sunucunuz için belirli ayrıcalıklara listelendiği gibi alır: seçin, Ekle, Güncelleştir, Sil, oluşturma, bırakma, yeniden yükleme, işlem, başvuruları, dizin, ALTER, Göster veritabanları, geçici tablolar oluşturma, kilit tablo, yürütme, çoğaltma bağımlı, çoğaltma İSTEMCİ, GÖRÜNÜM OLUŞTURUN, GÖRÜNÜMÜ GÖSTER, RUTİN OLUŞTURMA, YORDAMI ALTER, KULLANICI, OLAY, TETİKLEYİCİ OLUŞTURMA

MySQL sunucusu için Azure veritabanı oluşturulduktan sonra ek kullanıcılar oluşturma ve bunları yönetim erişim vermek için ilk sunucu yönetici kullanıcı hesabını kullanabilirsiniz. Ayrıca, sunucu yönetici hesabı, tek tek veritabanı şemalarını erişimi daha az ayrıcalıklı kullanıcıları oluşturmak için kullanılabilir.

## <a name="how-to-create-additional-admin-users-in-azure-database-for-mysql"></a>Ek yönetici kullanıcılar Azure veritabanı'nda MySQL için oluşturma
1. Bağlantı bilgileri ve yönetici kullanıcı adını alır.
   Veritabanı sunucusuna bağlanmak için tam sunucu adı ve yönetici oturum açma kimlik bilgileri gerekir. Sunucu adını ve oturum açma bilgilerini sunucusundan kolayca bulabilirsiniz **genel bakış** sayfa veya **özellikleri** Azure portalında sayfası. 

2. Veritabanı sunucusuna bağlanmak için yönetici hesabı ve parolayı kullanın. MySQL çalışma ekranı, mysql.exe, HeidiSQL veya diğerleri gibi tercih edilen istemci aracını kullanın. 
   Bağlanmak nasıl emin değilseniz bkz [kullanım MySQL çalışma ekranı bağlanmak ve verileri sorgulamak için](./connect-workbench.md)

3. Düzenleyebilir ve aşağıdaki SQL kodu çalıştırın. Yer tutucu değerini için yeni kullanıcı adınızı değiştirin `new_master_user`. Bu sözdiziminin tüm veritabanı şemalarını listelenen ayrıcalıklarını verir (*.*) kullanıcı adına (Bu örnekte new_master_user). 

   ```sql
   CREATE USER 'new_master_user'@'%' IDENTIFIED BY 'StrongPassword!';
   
   GRANT SELECT, INSERT, UPDATE, DELETE, CREATE, DROP, RELOAD, PROCESS, REFERENCES, INDEX, ALTER, SHOW DATABASES, CREATE TEMPORARY TABLES, LOCK TABLES, EXECUTE, REPLICATION SLAVE, REPLICATION CLIENT, CREATE VIEW, SHOW VIEW, CREATE ROUTINE, ALTER ROUTINE, CREATE USER, EVENT, TRIGGER ON *.* TO 'new_master_user'@'%' WITH GRANT OPTION; 
   
   FLUSH PRIVILEGES;
   ```

4. Verir doğrulayın 
   ```sql
   USE sys;
   
   SHOW GRANTS FOR 'new_master_user'@'%';
   ```

## <a name="how-to-create-database-users-in-azure-database-for-mysql"></a>Veritabanı kullanıcıları Azure veritabanı'nda MySQL için oluşturma

1. Bağlantı bilgileri ve yönetici kullanıcı adını alır.
   Veritabanı sunucusuna bağlanmak için tam sunucu adı ve yönetici oturum açma kimlik bilgileri gerekir. Sunucu adını ve oturum açma bilgilerini sunucusundan kolayca bulabilirsiniz **genel bakış** sayfa veya **özellikleri** Azure portalında sayfası. 

2. Veritabanı sunucusuna bağlanmak için yönetici hesabı ve parolayı kullanın. MySQL çalışma ekranı, mysql.exe, HeidiSQL veya diğerleri gibi tercih edilen istemci aracını kullanın. 
   Bağlanmak nasıl emin değilseniz bkz [kullanım MySQL çalışma ekranı bağlanmak ve verileri sorgulamak için](./connect-workbench.md)

3. Düzenleyebilir ve aşağıdaki SQL kodu çalıştırın. Yer tutucu değerini değiştirin `db_user` hedeflenen yeni bir kullanıcı adı ve yer tutucu değerini `testdb` kendi veritabanı adıyla.

   Bu sql kod sözdizimini testdb örnek amacıyla adlı yeni bir veritabanı oluşturur. MySQL hizmeti yeni bir kullanıcı oluşturur ve yeni veritabanı şeması tüm ayrıcalıkları verir (testdb.\*) bu kullanıcı için. 

   ```sql
   CREATE DATABASE testdb;
   
   CREATE USER 'db_user'@'%' IDENTIFIED BY 'StrongPassword!';
   
   GRANT ALL PRIVILEGES ON testdb . * TO 'db_user'@'%';
   
   FLUSH PRIVILEGES;
   ```

4. Veritabanı içinde verir doğrulayın.
   ```sql
   USE testdb;
   
   SHOW GRANTS FOR 'db_user'@'%';
   ```

5. Yeni bir kullanıcı adı ve parola kullanarak belirlenen veritabanı belirtme sunucuya oturum açın. Bu örnek, mysql komut satırı gösterir. Bu komutla, kullanıcı adı için parola istenir. Kendi sunucu adı, veritabanı adı ve kullanıcı adı ile değiştirin.

   ```azurecli-interactive
   mysql --host myserver4demo.mysql.database.azure.com --database testdb --user db_user@myserver4demo -p
   ```

## <a name="next-steps"></a>Sonraki adımlar
Bunların bağlanmasına etkinleştirmek için yeni kullanıcılar makinelerin IP adresleri için Güvenlik Duvarı'nı açın: [oluşturma ve Azure portalını kullanarak Azure veritabanı için MySQL güvenlik duvarı kurallarını yönetme](howto-manage-firewall-using-portal.md) veya [Azure CLI](howto-manage-firewall-using-cli.md).

Kullanıcı hesabı yönetimi ile ilgili daha fazla bilgi için MySQL için ürün belgelerine bakın [kullanıcı hesabı Yönetimi](https://dev.mysql.com/doc/refman/5.7/en/user-account-management.html), [GRANT sözdizimi](https://dev.mysql.com/doc/refman/5.7/en/grant.html), ve [ayrıcalıkları](https://dev.mysql.com/doc/refman/5.7/en/privileges-provided.html).
