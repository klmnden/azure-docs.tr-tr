---
title: "Azure SQL esnek ölçeklendirme ile ilgili SSS | Microsoft Docs"
description: "Azure SQL Database esnek ölçeği hakkında sık sorulan sorular."
services: sql-database
documentationcenter: 
manager: craigg
author: stevestein
ms.service: sql-database
ms.custom: scale out apps
ms.topic: article
ms.date: 10/24/2016
ms.author: sstein
ms.openlocfilehash: 575167293d9b4685ecc38303414b69074c366ab2
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="elastic-database-tools-faq"></a>Esnek veritabanı araçlarını SSS
#### <a name="if-i-have-a-single-tenant-per-shard-and-no-sharding-key-how-do-i-populate-the-sharding-key-for-the-schema-info"></a>Tek Kiracı başına parça ve hiçbir parçalama anahtarı varsa, nasıl şema bilgilerini parçalama anahtar doldurmak?
Şema bilgisi nesnesi yalnızca birleştirme senaryoları bölmek için kullanılır. Bir uygulama kendiliğinden tek Kiracı ise bölünmüş birleştirme Aracı'nı gerektirmez ve böylece şema bilgileri nesnesi doldurmak için gerek yoktur.

#### <a name="ive-provisioned-a-database-and-i-already-have-a-shard-map-manager-how-do-i-register-this-new-database-as-a-shard"></a>Bir veritabanı sağlanan ve bir parça eşleme Yöneticisi zaten, bu yeni veritabanını bir parça nasıl kaydettirebilirim?
Lütfen bakın  **[esnek veritabanı istemci kitaplığı kullanılarak uygulamaya bir parça ekleme](sql-database-elastic-scale-add-a-shard.md)**. 

#### <a name="how-much-do-elastic-database-tools-cost"></a>Esnek veritabanı araçlarını ne kadar maliyet?
Esnek veritabanı istemci kitaplığı kullanılarak maliyetlerin tabi değildir. Yalnızca için bölünmüş Birleştirme aracı sağlamak web/çalışan rolleri yanı sıra, parça ve parça eşleme Yöneticisi için kullandığınız Azure SQL veritabanları için maliyetleri tahakkuk.

#### <a name="why-are-my-credentials-not-working-when-i-add-a-shard-from-a-different-server"></a>Neden bir parça sunucusundan farklı bir sunucuya eklediğinizde kimlik bilgilerimi çalışmayan?
Kimlik bilgileri biçiminde kullanmayın "kullanıcı kimliği =username@servername", bunun yerine yalnızca kullanın "kullanıcı kimliği kullanıcı adı =".  Ayrıca, "kullanıcıadı" oturum açma üzerinde parça izinlere sahip olduğundan emin olun.

#### <a name="do-i-need-to-create-a-shard-map-manager-and-populate-shards-every-time-i-start-my-applications"></a>Parça eşleme Yöneticisi oluşturmak ve uygulamalarım Başlat her zaman parça doldurmak gerekiyor mu?
Hayır — parça eşleme Yöneticisi oluşturma (örneğin,  **[ShardMapManagerFactory.CreateSqlShardMapManager](http://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.createsqlshardmapmanager.aspx)**) tek seferlik bir işlemdir.  Uygulamanız çağrı kullanacak  **[ShardMapManagerFactory.TryGetSqlShardMapManager()](http://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.trygetsqlshardmapmanager.aspx)**  uygulama başlatma zaman.  Uygulama etki alanı başına yalnızca bir tür çağrısı var olmalıdır.

#### <a name="i-have-questions-about-using-elastic-database-tools-how-do-i-get-them-answered"></a>I esnek veritabanı araçlarını kullanma hakkında sorularınız varsa, bunları yanıtlanan nasıl sağlarım?
Lütfen bize üzerinde ulaşmak [Azure SQL veritabanı Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted).

#### <a name="when-i-get-a-database-connection-using-a-sharding-key-i-can-still-query-data-for-other-sharding-keys-on-the-same-shard--is-this-by-design"></a>Bir parçalama anahtarı kullanarak bir veritabanı bağlantısı aldığınızda, ı hala aynı parça diğer parçalama anahtarları için sorgu verileri görüntüleyebilirsiniz.  Bu tasarım gereği mi?
Esnek ölçeklendirme API'leri için parçalama anahtarınızı doğru veritabanına bir bağlantı sağlar, ancak parçalama anahtar filtreleme sağlamaz.  Ekleme **burada** yan tümceleri sorgunuzu gerekirse sağlanan parçalama anahtarına kapsamını kısıtlamak için.

#### <a name="can-i-use-a-different-azure-database-edition-for-each-shard-in-my-shard-set"></a>Farklı bir Azure veritabanı sürümü için her parça my parça kümesinde kullanabilir miyim?
Evet, bir parça tek tek bir veritabanıdır ve bu nedenle bir parça Premium edition başka Standard edition olsa da olabilir. Ayrıca, bir parça sürümü sırasında parça ömrü birden çok kez yukarı veya aşağı ölçeklendirebilirsiniz.

#### <a name="does-the-split-merge-tool-provision-or-delete-a-database-during-a-split-or-merge-operation"></a>Bir bölme veya birleştirme işlemi sırasında bir veritabanı bölünmüş Birleştirme aracı sağlama yapar (veya Sil)?
Hayır. İçin **bölme** işlemleri, hedef veritabanı ile uygun şema bulunmalı ve parça eşleme Yöneticisi ile kayıtlı olmalıdır.  İçin **birleştirme** işlemleri parça parça eşleme Yöneticisi'nden silin ve sonra veritabanını silin.

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

