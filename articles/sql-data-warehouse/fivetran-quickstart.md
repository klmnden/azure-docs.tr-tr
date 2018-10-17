---
title: Azure SQL veri ambarı ile Fivetran hızlı başlangıç | Microsoft Docs
description: Hızlı bir şekilde Fivetran ve Azure SQL veri ambarı ile çalışmaya başlayın.
services: sql-data-warehouse
author: hirokib
manager: jrj
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 10/12/2018
ms.author: elbutter
ms.reviewer: craigg
ms.openlocfilehash: 8e738becfe356908af5baffc0ebf225916b2616e
ms.sourcegitcommit: 8e06d67ea248340a83341f920881092fd2a4163c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49355540"
---
# <a name="get-started-quickly-with-fivetran-and-sql-data-warehouse"></a>Hızlı bir şekilde Fivetran ve SQL veri ambarı ile çalışmaya başlama

Bu hızlı başlangıçta, önceden mevcut olan bir SQL veri ambarı örneğini zaten sahip olduğunuzu varsayar.

## <a name="setup-connection"></a>Bağlantı kur

1. Azure SQL veri ambarı'na bağlanmak için veritabanı adı ve tam sunucu adını bulun.

   [Sunucu adını ve veritabanı adını Portalı'ndan bulmak nasıl?](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-connect-overview)

2. Kurulum Sihirbazı'nda, doğrudan veya bir SSH tüneli üzerinden veritabanınıza bağlanmak istediğinize karar verin.

   Veritabanına doğrudan bağlanmak karar verirseniz, erişime izin vermek için bir güvenlik duvarı kuralı oluşturmanız gerekir. Bu yöntem için basit ve en güvenli yöntemdir.

   SSH tüneli bağlanmak karar verirseniz, Fivetran ağınızdaki bir SSH tüneli veritabanınıza sağlayan ayrı bir sunucuya bağlanır. Bu yöntem, veritabanınız erişilemez bir alt ağdaki bir sanal ağ üzerindeki ise gereklidir.

3. "52.0.2.4" Ekle Fivetran Azure SQL veri ambarı için gelen bağlantılara izin vermesi, sunucu düzeyi Güvenlik Duvarı'nda IP adresi.

   [Sunucu düzeyinde güvenlik duvarı ekleme?](https://docs.microsoft.com/azure/sql-data-warehouse/create-data-warehouse-portal#create-a-server-level-firewall-rule)

## <a name="setup-user-credentials"></a>Kullanıcı kimlik bilgilerini ayarlayın

Sunucu yönetici kullanıcı olarak SQL Server Management Studio veya istediğiniz aracı kullanarak Azure SQL veri ambarı'na bağlanmak ve Fivetran için bir kullanıcı oluşturmak için aşağıdaki SQL komutlarını yürütün:

Ana veritabanında: ` CREATE LOGIN fivetran WITH PASSWORD = '<password>'; `

SQL veri ambarı veritabanında:

```
CREATE USER fivetran_user_without_login without login;
CREATE USER fivetran FOR LOGIN fivetran;
GRANT IMPERSONATE on USER::fivetran_user_without_login to fivetran;
```

Kullanıcı fivetran oluşturulduktan sonra Ambarınızı aşağıdaki izinleri verin:

```
GRANT CONTROL to fivetran;
```

Columnstore dizini oluşturma için bellek gereksinimi bağlı olarak oluşturulan kullanıcı için uygun kaynak sınıfı ekleyin. Örneğin, Marketo ve Salesforce gibi tümleştirmeler büyük nedeniyle daha yüksek kaynak sınıfı gerekir sütun sayısı / daha büyük columnstore dizinleri oluşturmak için daha fazla bellek gerektiren veri hacmi.

Statik kaynak sınıfları kullanılması önerilir. Kaynak sınıfı ile başlayabilirsiniz `staticrc20`, hangi ayırır 200 MB kullanıcının kullandığınız performans düzeyini ne olursa olsun. Geçerli kaynak sınıfıyla columnstore dizini oluşturma başarısız olursa, kaynak sınıfı artırmanız gerekir.

```
EXEC sp_addrolemember '<resource_class_name>', 'fivetran';
```

Daha fazla bilgi için içeren belgelere göz atın [bellek ve eşzamanlılık sınırları](https://docs.microsoft.com/azure/sql-data-warehouse/memory-and-concurrency-limits#data-warehouse-limits) ve [kaynak sınıfları](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-memory-optimizations-for-columnstore-compression#ways-to-allocate-more-memory)

Denetim izni, PolyBase kullanarak, Blob depolama alanından dosyaları yüklenirken kullanılan veritabanı kapsamlı kimlik bilgilerini oluşturmak için gereklidir.

Azure SQL veri ambarı erişmek için kimlik bilgilerini girin

1. Konak (sunucu adı)
2. Bağlantı noktası
3. Database
4. Kullanıcı (kullanıcı adı olmalıdır `fivetran@<server_name>` burada `<server_name>` azure konak urı'niz parçasıdır: `<server_name>.database.windows.net`)
5. Parola