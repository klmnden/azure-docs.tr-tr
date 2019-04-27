---
title: Elastik veritabanı istemci Kitaplığı'nda kimlik bilgilerinin yönetimiyle | Microsoft Docs
description: Doğru düzeylerine salt okunur, elastik veritabanı uygulamaları için yönetici kimlik bilgileri, ayarlama
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 01/03/2019
ms.openlocfilehash: e6c7d682d9b16266208baeeff14168b3da157251
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60584888"
---
# <a name="credentials-used-to-access-the-elastic-database-client-library"></a>Elastik veritabanı istemci kitaplığı erişmek için kullanılan kimlik bilgileri

[Elastik veritabanı istemci Kitaplığı](sql-database-elastic-database-client-library.md) üç farklı türde kimlik bilgilerine erişmek için kullandığı [parça eşleme Yöneticisi](sql-database-elastic-scale-shard-map-management.md). İhtiyacınıza göre erişim olası en düşük düzeyi ile kimlik bilgileri kullanın.

* **Yönetim kimlik bilgilerini**: oluşturma veya düzenleme parça eşleme Yöneticisi. (Bkz [sözlüğü](sql-database-elastic-scale-glossary.md).)
* **Erişim kimlik bilgilerini**: parçaları hakkında bilgi edinmek için mevcut bir parça eşleme Yöneticisi erişmek için.
* **Bağlantı kimlik bilgilerini**: parçalara bağlanmak için.

Ayrıca bkz: [veritabanlarını ve oturum açma bilgileri Azure SQL veritabanı'nda yönetme](sql-database-manage-logins.md).

## <a name="about-management-credentials"></a>Yönetim kimlik bilgileri ayrıntıları

Yönetim kimlik bilgilerini oluşturmak için kullanılan bir **ShardMapManager** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager.shardmapmanager), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager)) parça eşlemesi işlemek uygulamalar için nesne. (Örneğin, [esnek veritabanı araçlarını kullanarak bir parça ekleme](sql-database-elastic-scale-add-a-shard.md) ve [verilere bağımlı yönlendirme](sql-database-elastic-scale-data-dependent-routing.md)). Esnek ölçek istemcisi kitaplığını kullanıcısı SQL oturum açma bilgileri ve SQL kullanıcılar oluşturur ve her genel parça eşleme veritabanını ve tüm parça veritabanlarına de okuma/yazma izinleri verilir emin olur. Bu kimlik bilgileri, parça eşlemesine değişiklikleri gerçekleştirildiğinde genel parça eşleme ve yerel parça eşlemesi korumak için kullanılır. Örneğin, parça eşleme Yöneticisi nesnesi oluşturmak için gereken yönetim kimlik bilgilerini kullanın (kullanarak **GetSqlShardMapManager** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager.shardmapmanagerfactory.getsqlshardmapmanager), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager)):

```java
// Obtain a shard map manager.
ShardMapManager shardMapManager = ShardMapManagerFactory.GetSqlShardMapManager(smmAdminConnectionString,ShardMapManagerLoadPolicy.Lazy);
```

Değişken **smmAdminConnectionString** yönetim kimlik bilgilerini içeren bir bağlantı dizesidir. Kullanıcı kimliği ve parola parça eşleme veritabanını hem tek parça okuma/yazma erişimi sağlar. Yönetim bağlantı dizesi, sunucu adını ve veritabanı adı, genel parça eşleme veritabanını tanımlamak için de içerir. Bu amaç için bir bağlantı dizesi şöyledir:

```java
"Server=<yourserver>.database.windows.net;Database=<yourdatabase>;User ID=<yourmgmtusername>;Password=<yourmgmtpassword>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;”
```

Değerler biçiminde kullanmayın "username@server" — Bunun yerine yalnızca "username" değerini kullanın.  Kimlik bilgileri parça eşleme Yöneticisi veritabanını ve farklı sunucularda olabilecek tek parça karşı çalışmalıdır olmasıdır.

## <a name="access-credentials"></a>Erişim kimlik bilgileri

Parça eşlemeleri yönetmek olmayan bir uygulamada bir parça eşleme Yöneticisi oluştururken genel parça eşlemesinde salt okunur izinlere sahip kimlik bilgilerini kullanın. Bu kimlik bilgileri altında genel parça eşlemesinden alındı bilgileri için kullanılan [verilere bağımlı yönlendirme](sql-database-elastic-scale-data-dependent-routing.md) ve parça eşlemesi önbelleğini doldurmak için. Aynı çağrı desen aracılığıyla sağlanan kimlik bilgileri **GetSqlShardMapManager**:

```java
// Obtain shard map manager.
ShardMapManager shardMapManager = ShardMapManagerFactory.GetSqlShardMapManager(smmReadOnlyConnectionString, ShardMapManagerLoadPolicy.Lazy);  
```

Kullanımına dikkat edin **smmReadOnlyConnectionString** adına bu erişimi için farklı kimlik bilgileri kullanımını yansıtacak şekilde **yönetici olmayan** kullanıcılar: Bu kimlik bilgilerini yazma izinleri üzerinde sağlamalıdır değil Genel parça eşleme.

## <a name="connection-credentials"></a>Bağlantı kimlik bilgileri

Ek kimlik bilgileri kullanırken gereken **OpenConnectionForKey** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapper.listshardmapper.openconnectionforkey), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey)) bir parçalama anahtarı ile ilişkili bir parçaya erişimi için yöntemi. Bu kimlik bilgilerinin parça üzerinde bulunan yerel parça eşleme tablolarını salt okunur erişim için gerekli izinleri sağlamanız gerekir. Bu, parça verilere bağımlı yönlendirme için bağlantı doğrulamayı gerçekleştirmek için gereklidir. Bu kod parçacığı verilere bağımlı yönlendirme bağlamında veri erişimi sağlar:

```csharp
using (SqlConnection conn = rangeMap.OpenConnectionForKey<int>(targetWarehouse, smmUserConnectionString, ConnectionOptions.Validate))
```

Bu örnekte, **smmUserConnectionString** kullanıcı kimlik bilgileri için bağlantı dizesini içerir. Azure SQL DB için kullanıcı kimlik bilgileri için bir bağlantı dizesi şu şekildedir:

```java
"User ID=<yourusername>; Password=<youruserpassword>; Trusted_Connection=False; Encrypt=True; Connection Timeout=30;”  
```

Yönetici kimlik bilgileriyle biçiminde değerler kullanmayın gibi "username@server". Bunun yerine, yalnızca "username" kullanın.  Ayrıca, bağlantı dizesini bir sunucu adını ve veritabanı adını içermediğini unutmayın. Çünkü **OpenConnectionForKey** çağrı doğru parça anahtarına göre bağlantı otomatik olarak yönlendirir. Bu nedenle, sunucu adını ve veritabanı adını sağlanmaz.

## <a name="see-also"></a>Ayrıca bkz.

[Azure SQL Veritabanında veritabanlarını ve oturum açma bilgilerini yönetme](sql-database-manage-logins.md)

[SQL Veritabanınızı güvenli hale getirme](sql-database-security-overview.md)

[Elastik veritabanı işleriyle çalışmaya başlama](sql-database-elastic-jobs-getting-started.md)

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]
