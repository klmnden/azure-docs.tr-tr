---
title: "Kullanıcılar Azure veritabanı'nda PostgreSQL sunucusu oluşturun."
description: "Bu makalede, Azure veritabanı PostgreSQL sunucusu ile etkileşim kurmak için yeni kullanıcı hesapları nasıl oluşturabileceğiniz açıklanmaktadır."
services: postgresql
author: jasonwhowell
ms.author: jasonh
editor: jasonwhowell
manager: jhubbard
ms.service: postgresql-database
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 87a73929185112190d5dd6698e014db225ebc08e
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="create-users-in-azure-database-for-postgresql-server"></a>Kullanıcılar Azure veritabanı'nda PostgreSQL sunucusu oluşturun. 
Bu makalede, Azure veritabanı PostgreSQL sunucu için kullanıcıların nasıl oluşturabileceğiniz açıklanmaktadır.

## <a name="the-server-admin-account"></a>Sunucu yöneticisi hesabı
İlk Azure veritabanınızı PostgreSQL için oluşturduğunuz zaman, bir sunucu yöneticisi kullanıcı adı ve parola sağlanan. Daha fazla bilgi için izlediğiniz [Hızlı Başlangıç](quickstart-create-server-database-portal.md) adım adım bir yaklaşım görmek için. Sunucu Yöneticisi kullanıcı adı özel bir adı olduğundan, seçilen sunucu yöneticisi kullanıcı adı Azure portalından bulabilirsiniz.

Azure veritabanı PostgreSQL sunucu için tanımlanan 3 varsayılan rolleri ile oluşturulur. Komutunu çalıştırarak bu rolleri görebilirsiniz: `SELECT rolname FROM pg_roles;`
- azure_pg_admin
- azure_superuser
- Sunucu yönetici kullanıcısı

Sunucu yönetici kullanıcısı azure_pg_admin rolünün bir üyesi değil. Ancak, sunucu yönetici hesabı azure_superuser rolünün parçası değil. Bu hizmet yöneten bir PaaS hizmet olduğundan, yalnızca Microsoft süper kullanıcı rolünün bir parçasıdır. 

PostgreSQL altyapısı ayrıcalıkları veritabanı nesnelerine erişimi denetlemek için bölümünde açıklandığı gibi kullanır [PostgreSQL ürün belgelerine](https://www.postgresql.org/docs/current/static/sql-createrole.html). PostgreSQL için Azure veritabanı'nda sunucu yönetici kullanıcısı bu ayrıcalıklar verilir: oturum açma, NOSUPERUSER, DEVRAL, CREATEDB, CREATEROLE, NOREPLICATION

Sunucu yönetici kullanıcı hesabı, ek kullanıcılar oluşturma ve azure_pg_admin rolde bu kullanıcılara vermek için kullanılabilir. Ayrıca, sunucu yöneticisi hesabı daha az ayrıcalıklı kullanıcılar ve tek tek veritabanları ve şemaları erişimi roller oluşturmak için kullanılabilir.

## <a name="how-to-create-additional-admin-users-in-azure-database-for-postgresql"></a>Ek yönetici kullanıcılar için PostgreSQL Azure veritabanı'nda oluşturma
1. Bağlantı bilgileri ve yönetici kullanıcı adını alır.
   Veritabanı sunucusuna bağlanmak için tam sunucu adı ve yönetici oturum açma kimlik bilgileri gerekir. Sunucu adını ve oturum açma bilgilerini sunucusundan kolayca bulabilirsiniz **genel bakış** sayfa veya **özellikleri** Azure portalında sayfası. 

2. Veritabanı sunucusuna bağlanmak için yönetici hesabı ve parolayı kullanın. PgAdmin veya psql gibi tercih edilen istemci aracını kullanın.
   Bağlanmak nasıl emin değilseniz bkz [psql bulut Kabuğu'nda kullanarak PostgreSQL veritabanına bağlan](./quickstart-create-server-database-portal.md#connect-to-the-postgresql-database-by-using-psql-in-cloud-shell)

3. Düzenleyebilir ve aşağıdaki SQL kodu çalıştırın. Yer tutucu değerini < new_user > için yeni kullanıcı adınızı değiştirin ve yer tutucu parola kendi güçlü bir parola ile değiştirin. 

   ```sql
   CREATE ROLE <new_user> WITH LOGIN NOSUPERUSER INHERIT CREATEDB CREATEROLE NOREPLICATION PASSWORD '<StrongPassword!>';
   
   GRANT azure_pg_admin TO <new_user>;
   ```

## <a name="how-to-create-database-users-in-azure-database-for-postgresql"></a>Veritabanı kullanıcıları Azure veritabanı'nda PostgreSQL için oluşturma

1. Bağlantı bilgileri ve yönetici kullanıcı adını alır.
   Veritabanı sunucusuna bağlanmak için tam sunucu adı ve yönetici oturum açma kimlik bilgileri gerekir. Sunucu adını ve oturum açma bilgilerini sunucusundan kolayca bulabilirsiniz **genel bakış** sayfa veya **özellikleri** Azure portalında sayfası. 

2. Veritabanı sunucusuna bağlanmak için yönetici hesabı ve parolayı kullanın. PgAdmin veya psql gibi tercih edilen istemci aracını kullanın.

3. Düzenleyebilir ve aşağıdaki SQL kodu çalıştırın. Yer tutucu değerini değiştirin `<db_user>` hedeflenen yeni bir kullanıcı adı ve yer tutucu değerini `<newdb>` kendi veritabanı adıyla. Yer tutucu parola kendi güçlü bir parola ile değiştirin. 

   Bu sql kod sözdizimini testdb, örnek amacıyla adlı yeni bir veritabanı oluşturur. Ardından PostgreSQL hizmetinde yeni bir kullanıcı oluşturur ve o kullanıcı için yeni veritabanı ayrıcalıklar verir bağlanın. 

   ```sql
   CREATE DATABASE <newdb>;
   
   CREATE ROLE <db_user> WITH LOGIN NOSUPERUSER INHERIT CREATEDB NOCREATEROLE NOREPLICATION PASSWORD '<StrongPassword!>';
   
   GRANT CONNECT ON DATABASE testdb TO <db_user>;
   ```

4. Bir yönetici hesabı kullanarak, veritabanındaki nesneleri güvenli hale getirmek için ek ayrıcalıkları gerekebilir. Başvurmak [PostgreSQL belgelerine](https://www.postgresql.org/docs/current/static/ddl-priv.html) veritabanı rolleri ve ayrıcalıklar üzerinde daha ayrıntılı bilgi için. Örneğin: 
   ```sql
   GRANT ALL PRIVILEGES ON DATABASE <newdb> TO <db_user>;
   ```

5. Yeni bir kullanıcı adı ve parola kullanarak belirlenen veritabanı belirtme sunucunuza oturum açın. Bu örnek psql komut satırı gösterir. Bu komutla, kullanıcı adı için parola istenir. Kendi sunucu adı, veritabanı adı ve kullanıcı adı ile değiştirin.

   ```azurecli-interactive
   psql --host=mydemoserver.postgres.database.azure.com --port=5432 --username=db_user@mydemoserver --dbname=newdb
   ```

## <a name="next-steps"></a>Sonraki adımlar
Bunların bağlanmasına etkinleştirmek için yeni kullanıcılar makinelerin IP adresleri için Güvenlik Duvarı'nı açın: [oluşturma ve Azure portalını kullanarak Azure veritabanı PostgreSQL güvenlik duvarı kurallarını yönetme](howto-manage-firewall-using-portal.md) veya [Azure CLI](howto-manage-firewall-using-cli.md).

Kullanıcı hesabı yönetimi ile ilgili daha fazla bilgi için PostgreSQL ürün belgelerine bakın [veritabanı rolleri ve ayrıcalıkları](https://www.postgresql.org/docs/current/static/user-manag.html), [GRANT sözdizimi](https://www.postgresql.org/docs/current/static/sql-grant.html), ve [ayrıcalıkları](https://www.postgresql.org/docs/current/static/ddl-priv.html).
