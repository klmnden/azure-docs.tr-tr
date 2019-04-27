---
title: Azure SQL esnek ölçeklendirme ile ilgili SSS | Microsoft Docs
description: Sık sorulan Azure SQL veritabanı esnek Ölçeklendirmesi hakkında sorular.
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
ms.date: 01/25/2019
ms.openlocfilehash: b5ba5fadd229fa7119f9af791f7eaedbc984c92a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60584956"
---
# <a name="elastic-database-tools-frequently-asked-questions-faq"></a>Esnek veritabanı araçlarını sık sorulan sorular (SSS)

## <a name="if-i-have-a-single-tenant-per-shard-and-no-sharding-key-how-do-i-populate-the-sharding-key-for-the-schema-info"></a>Tek kiracılı parça ve hiçbir parçalama anahtarı başına sorularım varsa ne parçalama anahtarı için şema bilgileri doldurmak

Şema bilgisi nesnesi, yalnızca birleştirme senaryolarını bölmek için kullanılır. Doğal olarak tek kiracılı bir uygulama ise bölme birleştirme Aracı'nı gerektirmez ve böylece şema bilgisi nesnesi doldurmak için gerek yoktur.

## <a name="ive-provisioned-a-database-and-i-already-have-a-shard-map-manager-how-do-i-register-this-new-database-as-a-shard"></a>Bir veritabanı sağlanan ve parça eşleme Yöneticisi zaten, nasıl bu yeni veritabanını bir parça kaydedebilirim

Lütfen [elastik veritabanı istemci kitaplığını kullanarak bir uygulamaya bir parça ekleme](sql-database-elastic-scale-add-a-shard.md).

## <a name="how-much-do-elastic-database-tools-cost"></a>Esnek veritabanı araçlarını maliyeti ne kadar

Elastik veritabanı istemci kitaplığını kullanarak maliyetlerin hiçbirini tabi değildir. Maliyetler, yalnızca web/çalışan rolleri için bölünmüş Birleştirme aracı sağlama yanı sıra, parça ve parça eşleme Yöneticisi için kullandığınız Azure SQL veritabanları için tahakkuk eder.

## <a name="why-are-my-credentials-not-working-when-i-add-a-shard-from-a-different-server"></a>Neden kimlik bilgilerimi bir parça sunucusundan farklı bir sunucuya eklediğinizde çalışmıyor

Kimlik bilgileri biçiminde kullanma "kullanıcı kimliği =username@servername", bunun yerine yalnızca kullanın "kullanıcı kimliği kullanıcı adı =".  Ayrıca "username" oturum açma parça izinleri olduğundan emin olun.

## <a name="do-i-need-to-create-a-shard-map-manager-and-populate-shards-every-time-i-start-my-applications"></a>Parça eşleme Yöneticisi oluşturma ve uygulamalarım'ı her başlattığınızda parçalar doldurmak gerekiyor mu

Hayır; parça eşleme Yöneticisi oluşturma (örneğin, [ShardMapManagerFactory.CreateSqlShardMapManager](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.createsqlshardmapmanager)) tek seferlik bir işlemdir.  Uygulamanızın çağrı kullanması gerekir [ShardMapManagerFactory.TryGetSqlShardMapManager()](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.trygetsqlshardmapmanager) uygulama başlatma zaman.  Uygulama etki alanı başına yalnızca bir tür çağrısı var olmalıdır.

## <a name="i-have-questions-about-using-elastic-database-tools-how-do-i-get-them-answered"></a>Esnek veritabanı araçlarını kullanma hakkında sorularım, nasıl yanıtlanmış edinilir

Lütfen bize ulaşın [SQL veritabanının Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted).

## <a name="when-i-get-a-database-connection-using-a-sharding-key-i-can-still-query-data-for-other-sharding-keys-on-the-same-shard--is-this-by-design"></a>Bir parçalama anahtarı kullanarak bir veritabanı bağlantısı aldığımda hala aynı parça diğer parçalama anahtarları için sorgu veri gelebilecek.  Bu, bilinçli böyle

Esnek ölçeklendirme API'leri, parçalama anahtarı için doğru veritabanına bir bağlantı sağlar, ancak parçalama anahtarı filtreleme sağlamaz.  Ekleme **burada** sağlanan parçalama anahtarı için gerekiyorsa kapsamını kısıtlamak için sorgu yan tümcelerini.

## <a name="can-i-use-a-different-sql-database-edition-for-each-shard-in-my-shard-set"></a>My parça kümedeki farklı bir SQL veritabanı sürümünü her parça için kullanabilirim

Evet, bir parça bağımsız bir veritabanıdır ve bu nedenle bir parça Premium sürümü başka bir standart sürüm şöyle olabilir. Ayrıca, bir parça sürümü sırasında parça ömrünü birden çok kez yukarı veya aşağı ölçeklendirebilirsiniz.

## <a name="does-the-split-merge-tool-provision-or-delete-a-database-during-a-split-or-merge-operation"></a>Bir bölme ve birleştirme işlemi sırasında bir veritabanı bölme-Birleştirme aracı sağlama yapar (veya Sil)

Hayır. İçin **bölme** işlemleri hedef veritabanı ile uygun şema bulunmalı ve parça eşleme Yöneticisi ile kayıtlı olması.  İçin **birleştirme** işlemleri, parça parça eşleme Yöneticisi'nden silin ve sonra veritabanını silin.

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]