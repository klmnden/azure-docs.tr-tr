---
title: Azure SQL veri ambarı için Fivetran hızlı başlangıç | Microsoft Docs
description: Hızlı bir şekilde Fivetran ve Azure SQL veri ambarı ile çalışmaya başlayın.
services: sql-data-warehouse
author: mlee3gsd
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: integration
ms.date: 10/12/2018
ms.author: martinle
ms.reviewer: igorstan
ms.openlocfilehash: d829ee67d516892283fa31d9180336d768170ac1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65857010"
---
# <a name="get-started-quickly-with-fivetran-and-sql-data-warehouse"></a>Hızlı bir şekilde Fivetran ve SQL veri ambarı ile çalışmaya başlama

Bu hızlı başlangıçta, Azure SQL veri ambarı ile çalışmaya yeni bir Fivetran kullanıcı ayarlama işlemi açıklanmaktadır. Makale, mevcut bir SQL veri ambarı örneğini sahibi olduğunuzu varsayar.

## <a name="set-up-a-connection"></a>Bağlantı kurma

1. SQL veri ambarı'na bağlanmak için kullandığınız veritabanı adı ve tam sunucu adını bulun.
    
    Bu bilgileri bulmak için Yardım gerekiyorsa bkz [Azure SQL Data Warehouse'a bağlanma](sql-data-warehouse-connect-overview.md).

2. Kurulum Sihirbazı'nda, doğrudan veya bir SSH tüneli kullanarak veritabanınıza bağlanın isteyip istemediğinizi seçin.

   Veritabanına doğrudan bağlanmak isterseniz, erişime izin vermek için bir güvenlik duvarı kuralı oluşturmanız gerekir. Bu yöntem için basit ve en güvenli yöntemdir.

   SSH tüneli kullanarak bağlanmayı seçerseniz Fivetran ağınızdaki ayrı bir sunucuya bağlanır. Sunucu, veritabanı için bir SSH tüneli sağlar. Erişilemez bir alt ağdaki bir sanal ağ üzerindeki veritabanınız varsa, bu yöntem kullanmanız gerekir.

3. IP adresi Ekle **52.0.2.4** , sunucu düzeyinde güvenlik duvarı Fivetran SQL Data Warehouse Örneğiniz için gelen bağlantılara izin vermesi için.

   Daha fazla bilgi için [sunucu düzeyinde güvenlik duvarı kuralı oluşturma](create-data-warehouse-portal.md#create-a-server-level-firewall-rule).

## <a name="set-up-user-credentials"></a>Kullanıcı kimlik bilgilerini ayarla

1. SQL Server Management Studio veya tercih ettiğiniz bir aracı kullanarak, Azure SQL veri ambarı'na bağlanın. Sunucu yönetici kullanıcı olarak oturum açın. Ardından, bir kullanıcı için Fivetran oluşturmak için aşağıdaki SQL komutlarını çalıştırın:
    - Ana veritabanında: 
    
      ```
      CREATE LOGIN fivetran WITH PASSWORD = '<password>'; 
      ```

    - SQL veri ambarı veritabanında:

      ```
      CREATE USER fivetran_user_without_login without login;
      CREATE USER fivetran FOR LOGIN fivetran;
      GRANT IMPERSONATE on USER::fivetran_user_without_login to fivetran;
      ```

2. Fivetran kullanıcı ambarınıza aşağıdaki izinleri verin:

    ```
    GRANT CONTROL to fivetran;
    ```

    Denetim izni, bir kullanıcı dosyaları Azure Blob depolamadan PolyBase kullanarak yüklenirken kullanılan veritabanı kapsamlı kimlik bilgilerini oluşturmak için gereklidir.

3. Fivetran uygun kaynak sınıfı ekleyin. Kaynak sınıfı kullandığınız bir columnstore dizini oluşturmak için gereken belleği bağlıdır. Örneğin, Marketo ve Salesforce çok sayıda sütun ve büyük veri hacmi nedeniyle yüksek bir kaynak sınıfında gerektiren gibi ürünleri ile tümleştirme ürünleri kullanın. Daha yüksek bir kaynak sınıfı columnstore dizinleri oluşturmak için daha fazla bellek gerektirir.

    Statik kaynak sınıfları kullanmanızı öneririz. İle başlayabilirsiniz `staticrc20` kaynak sınıfı. `staticrc20` Kaynak sınıfı, her kullanıcı için 200 MB ayırır, bağımsız olarak performans düzeyini kullanırsınız. Columnstore dizini oluşturma ilk kaynak sınıf düzeyinde başarısız olursa, kaynak sınıfı artırın.

    ```
    EXEC sp_addrolemember '<resource_class_name>', 'fivetran';
    ```

    Hakkında daha fazla bilgi için okuma [bellek ve eşzamanlılık sınırları](memory-and-concurrency-limits.md) ve [kaynak sınıfları](sql-data-warehouse-memory-optimizations-for-columnstore-compression.md#ways-to-allocate-more-memory).


## <a name="sign-in-to-fivetran"></a>Fivetran için oturum açın

Fivetran için oturum açmak için SQL veri ambarı erişmek için kullandığınız kimlik bilgilerini girin: 

* Konak (sunucu adı).
* Bağlantı noktası.
* Veritabanı.
* Kullanıcı (kullanıcı adı olmalıdır **fivetran\@_sunucu_adı_**  burada *sunucu_adı* Azure URI ana bilgisayarınız bir parçasıdır: ***sunucu_adı *. Database.Windows.NET**).
* Parola.
