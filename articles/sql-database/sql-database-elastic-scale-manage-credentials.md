---
title: "Esnek veritabanı istemci Kitaplığı'nda kimlik bilgilerini yönetme | Microsoft Docs"
description: "Doğru düzeyde kimlik bilgileri, yönetici salt okunur, esnek veritabanı uygulamaları için nasıl kurulur"
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: 72e0edaf-795e-4856-84a5-6594f735fb7e
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: be041a2c1095452efa722a29f0b7a25aee1bc464
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2017
---
# <a name="credentials-used-to-access-the-elastic-database-client-library"></a>Esnek veritabanı istemci kitaplığına erişmek için kullanılan kimlik bilgileri
[Esnek veritabanı istemci Kitaplığı](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/) erişmek için kullandığı kimlik bilgilerini üç farklı türde [parça eşleme Yöneticisi](sql-database-elastic-scale-shard-map-management.md). Gereken bağlı olarak, kimlik bilgisi erişim olası en düşük düzeyde ile kullanın.

* **Yönetim kimlik bilgilerini**: oluşturmak veya bir parça eşleme Yöneticisi düzenleme. (Bkz [sözlüğü](sql-database-elastic-scale-glossary.md).) 
* **Erişim kimlik bilgilerini**: parça hakkında bilgi edinmek için mevcut bir parça eşleme Yöneticisi'ne erişmek için.
* **Bağlantı kimlik bilgilerini**: parça için bağlanmak için. 

Ayrıca bkz. [veritabanları ve Azure SQL veritabanında oturumları yönetme](sql-database-manage-logins.md). 

## <a name="about-management-credentials"></a>Yönetim kimlik bilgileri ayrıntıları
Yönetim kimlik bilgileri oluşturmak için kullanılan bir [ **ShardMapManager** ](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx) nesne parça eşlemeleri işleyen uygulamalar için. (Örneğin, [esnek veritabanı araçlarını kullanarak bir parça ekleme](sql-database-elastic-scale-add-a-shard.md) ve [veri bağımlı yönlendirme](sql-database-elastic-scale-data-dependent-routing.md)). Esnek ölçeklendirme istemci kitaplığı kullanıcı SQL oturum açma bilgileri ve SQL kullanıcılar oluşturur ve her genel parça eşleme veritabanını ve tüm parça veritabanlarını da okuma/yazma izinleri verilir emin olur. Bu kimlik bilgileri, parça eşleme değişiklikleri gerçekleştirildiğinde genel parça eşleme ve yerel parça eşlemeleri korumak için kullanılır. Örneğin, parça eşleme Yöneticisi nesnesi oluşturmak için yönetim kimlik bilgilerini kullanın (kullanarak [ **GetSqlShardMapManager**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx): 

    // Obtain a shard map manager. 
    ShardMapManager shardMapManager = ShardMapManagerFactory.GetSqlShardMapManager( 
            smmAdminConnectionString, 
            ShardMapManagerLoadPolicy.Lazy 
    ); 

Değişkeni **smmAdminConnectionString** yönetim kimlik bilgilerini içeren bir bağlantı dizesi. Kullanıcı kimliği ve parola parça eşleme veritabanı ve tek tek parça okuma/yazma erişimi sağlar. Yönetim bağlantı dizesini de sunucu adını ve genel parça eşleme veritabanını tanımlamak için veritabanı adını içerir. Bu amaç için tipik bağlantı dizesi şöyledir:

     "Server=<yourserver>.database.windows.net;Database=<yourdatabase>;User ID=<yourmgmtusername>;Password=<yourmgmtpassword>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;” 

Değerler biçiminde kullanmayın "username@server" — yalnızca "username" değerini kullanın.  Bu durum, kimlik bilgileri parça eşleme manager veritabanı ve farklı sunucularda olabilir tek tek parça karşı çalışmanız gerekir çünkü.

## <a name="access-credentials"></a>Erişim kimlik bilgileri
Bir parça parça eşlemeleri yönetme olmayan bir uygulamada harita Yöneticisi oluştururken, genel parça harita üzerinde salt okuma izinlerine sahip kimlik bilgileri kullanın. Bu kimlik bilgileri altında genel parça eşlemesinden alındı bilgileri için kullanılan [veri bağımlı yönlendirme](sql-database-elastic-scale-data-dependent-routing.md) ve istemci üzerindeki parça eşleme önbelleğini doldurmak için. Aynı çağrı düzeni aracılığıyla sağlanan kimlik bilgileri **GetSqlShardMapManager**: 

    // Obtain shard map manager. 
    ShardMapManager shardMapManager = ShardMapManagerFactory.GetSqlShardMapManager( 
            smmReadOnlyConnectionString, 
            ShardMapManagerLoadPolicy.Lazy
    );  

Kullanımına dikkat edin **smmReadOnlyConnectionString** adına bu erişimi için farklı kimlik bilgileri kullanımını yansıtacak şekilde **yönetici olmayan** kullanıcılar: Bu kimlik bilgileri yazma izinleri sağlamalıdır değil Genel parça eşleme. 

## <a name="connection-credentials"></a>Bağlantı kimlik bilgileri
Ek kimlik bilgileri kullanılırken gereklidir [ **OpenConnectionForKey** ](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey.aspx) parçalama anahtar ile ilişkili bir parça erişim yöntemi. Bu kimlik bilgileri parça üzerinde bulunan yerel parça eşleme tablolarını salt okunur erişim izinlerini sağlamanız gerekir. Bu, veri bağımlı üzerinde parça yönlendirme için bağlantı doğrulama gerçekleştirmek için gereklidir. Bu kod parçacığını veri erişimini veri bağımlı yönlendirme bağlamında sağlar: 

    using (SqlConnection conn = rangeMap.OpenConnectionForKey<int>( 
    targetWarehouse, smmUserConnectionString, ConnectionOptions.Validate)) 

Bu örnekte, **smmUserConnectionString** kullanıcı kimlik bilgileri için bağlantı dizesini içerir. Azure SQL DB için kullanıcı kimlik bilgileri için tipik bağlantı dizesini şöyledir: 

    "User ID=<yourusername>; Password=<youruserpassword>; Trusted_Connection=False; Encrypt=True; Connection Timeout=30;”  

Yönetici kimlik bilgileriyle biçiminde değerler kullanmayın gibi "username@server". Bunun yerine, yalnızca "username" kullanın.  Ayrıca, bağlantı dizesi bir sunucu adı ve veritabanı adı içermediğini unutmayın. Çünkü **OpenConnectionForKey** çağrısı anahtarına göre doğru parça bağlantısı otomatik olarak yönlendirir. Bu nedenle, sunucu adını ve veritabanı adı sağlanmadı. 

## <a name="see-also"></a>Ayrıca bkz.
[Azure SQL Veritabanında veritabanlarını ve oturum açma bilgilerini yönetme](sql-database-manage-logins.md)

[SQL Veritabanınızı güvenli hale getirme](sql-database-security-overview.md)

[Esnek veritabanı işlerine Başlarken](sql-database-elastic-jobs-getting-started.md)

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

