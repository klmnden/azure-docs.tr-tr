---
title: Kullanıcılar, MySQL sunucusu için Azure veritabanı'nda oluşturma
description: Bu makalede, MySQL için Azure veritabanı ile etkileşim kurmak için yeni kullanıcı hesaplarını nasıl oluşturabileceğiniz açıklanır.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 02/28/2018
ms.openlocfilehash: 4cd2305ed3d7f88f6c3825d8f7cdb5d81f9a0f6b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61460193"
---
# <a name="create-users-in-azure-database-for-mysql-server"></a>Kullanıcılar, MySQL sunucusu için Azure veritabanı'nda oluşturma 
Bu makalede, bir MySQL sunucusu için Azure veritabanı'nda kullanıcılara nasıl oluşturabileceğiniz açıklanır.

MySQL için Azure veritabanınızı ilk oluşturduğunuzda, bir Sunucu Yöneticisi oturum açma kullanıcı adı ve parola sağlanan. Daha fazla bilgi için izleyebileceğiniz [hızlı](quickstart-create-mysql-server-database-using-azure-portal.md). Sunucu Yöneticisi oturum açma kullanıcı adını Azure portalında bulabilirsiniz.

Sunucu yönetici kullanıcısı belirli ayrıcalıklara listelendiği gibi sunucunuzun alır: SEÇİN, EKLE, GÜNCELLEŞTİR, SİL, OLUŞTURMA, BIRAKMA, YENİDEN, İŞLEM, BAŞVURULARI, INDEX, ALTER, VERİTABANLARI, GÖSTER GEÇİCİ TABLOLAR OLUŞTURMA, KİLİTLEME TABLOLAR, YÜRÜTÜN, İKİNCİL ÇOĞALTMA, ÇOĞALTMA İSTEMCİSİ, OLUŞTUR, GÖRÜNTÜLE, GÖRÜNÜMÜ GÖSTER, RUTİN OLUŞTURMAK, ALTER YORDAMI, KULLANICI OLUŞTURMA , OLAY TETİKLEYİCİSİ

MySQL sunucusu için Azure veritabanı oluşturulduktan sonra ek kullanıcılar oluşturma ve yönetici erişim vereceğinizi ilk sunucu yöneticisi kullanıcı hesabını kullanabilirsiniz. Ayrıca, sunucu yöneticisi hesabı, tek veritabanı şemalarını erişimi daha az ayrıcalıklı kullanıcı oluşturmak için kullanılabilir.

## <a name="how-to-create-additional-admin-users-in-azure-database-for-mysql"></a>Ek yönetici kullanıcılara, MySQL için Azure veritabanı'nda oluşturma
1. Bağlantı bilgileri ve yönetici kullanıcı adını alın.
   Veritabanı sunucusuna bağlanmak için tam sunucu adı ve yönetici oturum açma kimlik bilgileri gerekir. Sunucu adını ve sunucu oturum açma bilgilerini kolayca bulabilirsiniz **genel bakış** sayfası veya **özellikleri** Azure portalında sayfası. 

2. Veritabanı sunucunuza bağlanmak için yönetici hesabı ve parolayı kullanın. MySQL Workbench, mysql.exe, HeidiSQL veya diğerleri gibi tercih edilen istemci aracını kullanın. 
   Bağlanmak nasıl emin değilseniz bkz [bağlanmak ve veri sorgulamak için kullanım MySQL Workbench](./connect-workbench.md)

3. Düzenleyin ve aşağıdaki SQL kodu çalıştırın. Yer tutucu değerini için yeni kullanıcı adınızı değiştirin `new_master_user`. Bu söz dizimi listelenen tüm veritabanı şemalarını ayrıcalıklar verir ( *.* ) kullanıcı adına (Bu örnekte new_master_user). 

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

## <a name="how-to-create-database-users-in-azure-database-for-mysql"></a>Nasıl MySQL için Azure veritabanı'nda veritabanı kullanıcıları oluşturma

1. Bağlantı bilgileri ve yönetici kullanıcı adını alın.
   Veritabanı sunucusuna bağlanmak için tam sunucu adı ve yönetici oturum açma kimlik bilgileri gerekir. Sunucu adını ve sunucu oturum açma bilgilerini kolayca bulabilirsiniz **genel bakış** sayfası veya **özellikleri** Azure portalında sayfası. 

2. Veritabanı sunucunuza bağlanmak için yönetici hesabı ve parolayı kullanın. MySQL Workbench, mysql.exe, HeidiSQL veya diğerleri gibi tercih edilen istemci aracını kullanın. 
   Bağlanmak nasıl emin değilseniz bkz [bağlanmak ve veri sorgulamak için kullanım MySQL Workbench](./connect-workbench.md)

3. Düzenleyin ve aşağıdaki SQL kodu çalıştırın. Yer tutucu değerini değiştirin `db_user` hedeflenen yeni bir kullanıcı adı ve yer tutucu değerini `testdb` veritabanınızın adı ile.

   Bu sql kod söz dizimi testdb örneklerde adlı yeni bir veritabanı oluşturur. MySQL hizmeti yeni bir kullanıcı oluşturur ve yeni veritabanı şemasına tüm ayrıcalıkları verir (testdb.\*) söz konusu kullanıcı için. 

   ```sql
   CREATE DATABASE testdb;
   
   CREATE USER 'db_user'@'%' IDENTIFIED BY 'StrongPassword!';
   
   GRANT ALL PRIVILEGES ON testdb . * TO 'db_user'@'%';
   
   FLUSH PRIVILEGES;
   ```

4. Veritabanı içinde bir onayları doğrulayın.
   ```sql
   USE testdb;
   
   SHOW GRANTS FOR 'db_user'@'%';
   ```

5. Yeni kullanıcı adı ve parola kullanarak belirli bir veritabanı belirtme sunucuya oturum açın. Bu örnek, mysql komut satırını gösterir. Bu komutla birlikte, kullanıcı adı için parola istenir. Kendi sunucu adı, veritabanı adı ve kullanıcı adını değiştirin.

   ```azurecli-interactive
   mysql --host mydemoserver.mysql.database.azure.com --database testdb --user db_user@mydemoserver -p
   ```

## <a name="next-steps"></a>Sonraki adımlar
Bunların bağlanmasına imkan yeni kullanıcılar Makine IP adresleri için Güvenlik Duvarı'nı açın: [Oluşturma ve Azure portalını kullanarak Azure veritabanı için MySQL güvenlik duvarı kurallarını yönetme](howto-manage-firewall-using-portal.md) veya [Azure CLI](howto-manage-firewall-using-cli.md).

Kullanıcı hesabı yönetimi hakkında daha fazla bilgi için MySQL ürün belgeleri için bkz. [kullanıcı hesabı Yönetimi](https://dev.mysql.com/doc/refman/5.7/en/user-account-management.html), [verme söz dizimi](https://dev.mysql.com/doc/refman/5.7/en/grant.html), ve [ayrıcalıkları](https://dev.mysql.com/doc/refman/5.7/en/privileges-provided.html).
