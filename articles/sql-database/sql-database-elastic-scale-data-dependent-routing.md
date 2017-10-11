---
title: "Azure SQL veritabanı ile yönlendirme bağımlı veri | Microsoft Docs"
description: "Veri bağımlı yönlendirme, Azure SQL veritabanında parçalı veritabanlarının bir özellik için .NET uygulamalarında ShardMapManager sınıfını kullanma"
services: sql-database
documentationcenter: 
manager: jhubbard
author: torsteng
editor: 
ms.assetid: cad09e15-5561-4448-aa18-b38f54cda004
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2017
ms.author: ddove
ms.openlocfilehash: 6b68bbb0133afd1493acdb58f79f3eeaf6a8d7cd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="data-dependent-routing"></a>Verilere bağımlı yönlendirme
**Veri bağımlı yönlendirme** uygun bir veritabanına isteği yönlendirmek için bir sorguda veri kullanma yeteneği. Bu temel düzeni parçalı veritabanları ile çalışırken. Özellikle parçalama anahtar sorgu parçası değilse, istek bağlamını isteği yönlendirmek için de kullanılabilir. Her özel bir sorgu veya veri bağımlı yönlendirme kullanarak bir uygulamayı işlemde istek başına tek bir veritabanına erişmek için sınırlıdır. Azure SQL veritabanı esnek araçlar için bu yönlendirme ile gerçekleştirilir  **[ShardMapManager sınıfı](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx)**  ADO.NET uygulamalarında.

Uygulama çeşitli bağlantı dizeleri ya da parçalı ortamında verileri farklı dilimleri ilişkili DB konumları izlemek zorunda değildir. Bunun yerine, [parça eşleme Yöneticisi](sql-database-elastic-scale-shard-map-management.md) tabanlı açar bağlantılar gerektiğinde, doğru veritabanlarına parça eşleme ve uygulamanın isteği hedefidir parçalama anahtarının değerini veriler üzerinde. Genellikle anahtarıdır *customer_id*, *tenant_id*, *date_key*, veya veritabanı isteğin temel parametresi bazı bir belirli tanıtıcı). 

Daha fazla bilgi için bkz: [ölçeklendirme çıkışı SQL Server ile veri bağımlı yönlendirme](https://technet.microsoft.com/library/cc966448.aspx).

## <a name="download-the-client-library"></a>İstemci kitaplığını yükle
Sınıfını almak için yükleme [esnek veritabanı istemci Kitaplığı](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/). 

## <a name="using-a-shardmapmanager-in-a-data-dependent-routing-application"></a>Bir ShardMapManager bir veri bağımlı yönlendirme uygulamasında kullanma
Uygulamaları örneği **ShardMapManager** başlatılması sırasında fabrikada çağrısı kullanarak  **[GetSQLShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx)**. Bu örnekte, hem bir **ShardMapManager** ve belirli bir **ShardMap** içerdiği başlatılır. Bu örnek GetSqlShardMapManager gösterir ve [GetRangeShardMap](https://msdn.microsoft.com/library/azure/dn824173.aspx) yöntemleri.

    ShardMapManager smm = ShardMapManagerFactory.GetSqlShardMapManager(smmConnnectionString, 
                      ShardMapManagerLoadPolicy.Lazy);
    RangeShardMap<int> customerShardMap = smm.GetRangeShardMap<int>("customerMap"); 

### <a name="use-lowest-privilege-credentials-possible-for-getting-the-shard-map"></a>Parça eşleme için olası en düşük ayrıcalıklı kimlik bilgileri kullanın
Bir uygulama parça eşleme düzenleme değil, Fabrika yönteminde kullanılan kimlik bilgileri yalnızca salt okunur izinleri olmalıdır **genel parça eşleme** veritabanı. Bu kimlik bilgileri bağlantıları parça eşleme Yöneticisi'ni açmak için kullanılan kimlik bilgileri genellikle farklıdır. Ayrıca bkz. [esnek veritabanı istemci kitaplığına erişmek için kullanılan kimlik bilgileri](sql-database-elastic-scale-manage-credentials.md). 

## <a name="call-the-openconnectionforkey-method"></a>OpenConnectionForKey yöntemini çağırın
 **[ShardMap.OpenConnectionForKey yöntemi](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey.aspx)**  bir ADO.Net bağlantı komutları değerine göre uygun veritabanı verme için hazır döndürür **anahtar** parametre. Parça bilgi uygulama tarafından önbelleğe alınmış **ShardMapManager**, bu istekleri genellikle bir veritabanı araması karşı içermeyen **genel parça eşleme** veritabanı. 

    // Syntax: 
    public SqlConnection OpenConnectionForKey<TKey>(
        TKey key,
        string connectionString,
        ConnectionOptions options
    )


* **Anahtar** parametresi arama anahtarı olarak parça eşlemeye istek için uygun veritabanı belirlemek için kullanılır. 
* **ConnectionString** yalnızca kullanıcı kimlik bilgilerini istenen bağlantı için geçirmek için kullanılır. Hiçbir veritabanı adı veya sunucu adı bu dahil edilen *connectionString* yöntemi ve veritabanı sunucusu kullanarak belirlediğinden **ShardMap**. 
*  **[ConnectionOptions](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.connectionoptions.aspx)**  ayarlanmalı **ConnectionOptions.Validate** burada parça eşleyen bir ortam değişebilir ve satır diğer veritabanlarına sonucu olarak taşırsanız Bölünmüş veya birleştirme işlemleri. Bu hedefte yerel parça eşleme için kısa bir sorgu içerir bağlantı uygulamaya teslim edilmeden önce veritabanı (değil genel parça eşleme için). 

(Önbellek yanlış olduğunu belirten) yerel parça eşleme karşı doğrulama başarısız olursa, parça eşleme Yöneticisi arama yeni doğru değerini edinme, önbellek, güncelleştirme ve elde ve uygun veritabanı dönmek için genel parça harita sorgulama bağlantı. 

Kullanım **ConnectionOptions.None** uygulamanın çevrimiçi durumdayken yalnızca zaman parça eşleme değişiklikleri beklenmez. Bu durumda, önbelleğe alınan değer her zaman doğru olması için kabul edilebilir ve hedef veritabanına ek gidiş dönüş doğrulama çağrısı güvenle atlanabilir. Veritabanı trafiğini azaltır. **ConnectionOptions** bir değer parçalama değişiklikleri beklenen olup olmadığını belirtmek için bir yapılandırma dosyası veya bir zaman aralığında sırasında değil de ayarlanabilir.  

Bu örnek bir tamsayı anahtarının değerini kullanır **CustomerID**kullanarak bir **ShardMap** adlı nesne **customerShardMap**.  

    int customerId = 12345; 
    int newPersonId = 4321; 

    // Connect to the shard for that customer ID. No need to call a SqlConnection 
    // constructor followed by the Open method.
    using (SqlConnection conn = customerShardMap.OpenConnectionForKey(customerId, 
        Configuration.GetCredentialsConnectionString(), ConnectionOptions.Validate)) 
    { 
        // Execute a simple command. 
        SqlCommand cmd = conn.CreateCommand(); 
        cmd.CommandText = @"UPDATE Sales.Customer 
                            SET PersonID = @newPersonID 
                            WHERE CustomerID = @customerID"; 

        cmd.Parameters.AddWithValue("@customerID", customerId); 
        cmd.Parameters.AddWithValue("@newPersonID", newPersonId); 
        cmd.ExecuteNonQuery(); 
    }  

**OpenConnectionForKey** yöntemi doğru veritabanına yeni bir zaten açık bağlantı döndürür. Bu şekilde kullanılan bağlantılar hala ADO.Net bağlantı havuzu tam avantajından yararlanmak. İşlemler ve istekleri tarafından bir parça aynı anda karşılanabilir sürece, bu uygulamada zaten ADO.Net kullanarak gerekli yalnızca değişikliği olmalıdır. 

 **[OpenConnectionForKeyAsync yöntemi](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkeyasync.aspx)**  da uygulamanızı ADO.Net ile zaman uyumsuz programlama kullanım yaparsa kullanılabilir. Davranışını ADO eşdeğer yönlendirme bağımlı veridir. NET'in  **[Connection.OpenAsync](https://msdn.microsoft.com/library/hh223688\(v=vs.110\).aspx)**  yöntemi.

## <a name="integrating-with-transient-fault-handling"></a>Geçici hata işleme ile tümleştirme
Veri erişimi uygulamaları bulutta geliştirilmesi konusunda en iyi uygulama, uygulama tarafından geçici hataları yakalanır ve işlemleri hata atmadan önce birkaç kez denenir emin olmaktır. Geçici hata bulut uygulamaları için işleme sırasında ele alınmıştır [geçici hata işleme](https://msdn.microsoft.com/library/dn440719\(v=pandp.60\).aspx). 

Geçici hata işleme, veri bağımlı yönlendirme desenle doğal olarak bulunabilir. Tüm veri erişim isteği dahil olmak üzere yeniden denemek için önemli gereksinimdir **kullanarak** veri bağımlı yönlendirme bağlantısı elde bloğu. Yukarıdaki örnekte (vurgulanmış Not değiştirin) aşağıdaki gibi yeniden yazılmıştır. 

### <a name="example---data-dependent-routing-with-transient-fault-handling"></a>Örnek - veri geçici hata işleme ile yönlendirme bağımlı
<pre><code>int customerId = 12345; 
int newPersonId = 4321; 

<span style="background-color:  #FFFF00">Configuration.SqlRetryPolicy.ExecuteAction(() =&gt; </span> 
<span style="background-color:  #FFFF00">    { </span>
        // Connect to the shard for a customer ID. 
        using (SqlConnection conn = customerShardMap.OpenConnectionForKey(customerId,  
        Configuration.GetCredentialsConnectionString(), ConnectionOptions.Validate)) 
        { 
            // Execute a simple command 
            SqlCommand cmd = conn.CreateCommand(); 

            cmd.CommandText = @&quot;UPDATE Sales.Customer 
                            SET PersonID = @newPersonID 
                            WHERE CustomerID = @customerID&quot;; 

            cmd.Parameters.AddWithValue(&quot;@customerID&quot;, customerId); 
            cmd.Parameters.AddWithValue(&quot;@newPersonID&quot;, newPersonId); 
            cmd.ExecuteNonQuery(); 

            Console.WriteLine(&quot;Update completed&quot;); 
        } 
<span style="background-color:  #FFFF00">    }); </span> 
</code></pre>


Esnek veritabanı örnek uygulamasını derlerken geçici hata işleme uygulanması için gerekli paketleri otomatik olarak yüklenir. Paketleri de edinilebilir ayrı olarak [Kurumsal Library - geçici hata işleme uygulama blok](http://www.nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/). Sürüm 6.0 veya üstü kullanın. 

## <a name="transactional-consistency"></a>İşlem tutarlılığı
İşlem özellikleri tüm işlemler için bir parça yerel olarak sağlanır. Örneğin, bağlantı için hedef parça kapsamında veri bağımlı yönlendirme üzerinden gönderilen işlemleri yürütün. Şu anda bir işlem içinde birden çok bağlantı kaydetme için sağlanan özelliği yoktur ve bu nedenle parça üzerinde gerçekleştirilen işlemler için işlem garanti vardır.

## <a name="next-steps"></a>Sonraki adımlar
Bir parça detach veya bir parça yeniden eklemek için bkz: [parça eşleme sorunları düzeltmek için RecoveryManager sınıfını kullanma](sql-database-elastic-database-recovery-manager.md)

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

