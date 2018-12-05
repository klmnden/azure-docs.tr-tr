---
title: "Öğretici: Azure Cosmos DB Cassandra API'SİNİN hesabına verilerinizi geçirme"
description: Bu öğreticide, Azure Cosmos DB Cassandra API'SİNİN hesabına Apache Cassandra veri kopyalamak için CQL kopyalama komutu ve Spark kullanmayı öğrenin.
author: kanshiG
ms.service: cosmos-db
ms.component: cosmosdb-cassandra
ms.author: govindk
ms.topic: tutorial
ms.date: 12/03/2018
ms.reviewer: sngun
Customer intent: As a developer, I want to migrate my existing Cassandra workloads to Azure Cosmos DB so that the overhead to manage resources, clusters, and garbage collection is automatically handled by Azure Cosmos DB.
ms.openlocfilehash: 604cab3bed73366ce28c8bb35b63df6379985cfb
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52867477"
---
# <a name="tutorial-migrate-your-data-to-cassandra-api-account-in-azure-cosmos-db"></a>Öğretici: Azure Cosmos DB'de Cassandra API hesabı, verilerinizi geçirme

Bir geliştirici olarak, şirket içinde çalışan mevcut Cassandra iş yüklerini olabilir veya bulutta bulunan ve bunları Azure'a geçirmek isteyebilirsiniz. Azure Cosmos DB Cassandra API'SİNİN hesabına tür iş yüklerinin geçirebilirsiniz. Bu öğretici, Cassandra API hesabı Azure Cosmos DB'de Apache Cassandra verileri geçirmek kullanılabilir farklı seçenekler hakkında yönergeler sağlar.

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Geçiş planlaması
> * Geçiş önkoşulları
> * cqlsh COPY komutunu kullanarak verileri geçirme
> * Spark'ı kullanarak verileri geçirme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites-for-migration"></a>Geçiş önkoşulları

* **Aktarım hızınızı ihtiyaçlarınızı tahmin etmenize:** veri geçiriyorsanız Cassandra API'si için Azure Cosmos DB hesabı önce iş yükünüz aktarım hızı gereksinimlerini tahmin etmelidir. Genel olarak, CRUD işlemlerine gereken ortalama aktarım hızıyla başlamanız ve ardından Ayıklama Dönüştürme Yükleme (ETL) için veya öngörülemeyen işlemler için gereken fazladan aktarım hızını eklemeniz önerilir. Geçişi planlamak için şu ayrıntılara ihtiyacınız vardır: 

   * **Mevcut veri boyutu veya tahmini veri boyutu:** Minimum veritabanı boyutunu ve aktarım hızı gereksinimini tanımlar. Yeni uygulama için veri boyutu tahmini yapıyorsanız, verilerin satırlara düzgün dağıtıldığını varsayabilir ve veri boyutuyla çarparak değeri tahmin edebilirsiniz. 

   * **Gerekli aktarım hızı:** (sorgu/get) okuma ve yazma (update/delete/INSERT) aktarım hızı. Bu değer hem gerekli istek birimlerini hem de eylemsizlik durumunda veri boyutu hesaplamak için gereklidir.  

   * **Şema:** Cassandra cqlsh ve dışarı aktarma şema aracılığıyla mevcut Cassandra kümenize bağlanın: 

     ```bash
     cqlsh [IP] "-e DESC SCHEMA" > orig_schema.cql
     ```

   Var olan iş yükü gereksinimlerinize tanımladıktan sonra bir Azure Cosmos hesabı, veritabanı ve kapsayıcıları toplanan performans gereksinimlerine göre oluşturmanız gerekir.  

   * **Bir işlem RU ücreti belirlemek:** RU'ları Cassandra API tarafından desteklenen SDK'ları kullanarak belirleyebilirsiniz. Bu örnekte .NET sürümünün RU ücretleri gösterilmektedir.

     ```csharp
     var tableInsertStatement = table.Insert(sampleEntity);
     var insertResult = await tableInsertStatement.ExecuteAsync();

     foreach (string key in insertResult.Info.IncomingPayload)
       {
          byte[] valueInBytes = customPayload[key];
          string value = Encoding.UTF8.GetString(valueInBytes);
          Console.WriteLine($"CustomPayload:  {key}: {value}");
       }
     ```

* **Gerekli aktarım hızını ayırma** Gereksinimleriniz arttıkça Azure Cosmos DB depolamayı ve aktarım hızını otomatik olarak ölçeklendirilebilir. Aktarım hızı gereksinimlerinizi tahmin etmek için [Azure Cosmos DB istek birimi hesaplayıcısını](https://www.documentdb.com/capacityplanner) kullanabilirsiniz. 

* **Tablo Cassandra API hesabı oluşturma:** veri geçişine başlamadan önce tüm tablolara Azure portalından veya cqlsh önceden oluşturun. Veritabanı düzeyinde aktarım hızına sahip bir Azure Cosmos hesap geçiriyorsanız, Azure Cosmos kapsayıcı oluştururken bir bölüm anahtarı sağlamanız emin olun.

* **Aktarım hızını artırma:** Veri geçişinizin süresi, Azure Cosmos DB'deki tablolar için sağladığınız aktarım hızı miktarına bağlıdır. Geçiş süresince aktarım hızını artırın. Daha yüksek aktarım hızı ile, hız sınırlamayı önleyebilir ve daha kısa sürede geçişi tamamlayabilirsiniz. Geçişi tamamladıktan sonra maliyet tasarrufu sağlamak için aktarım hızını azaltın. Ayrıca, Azure Cosmos hesabının kaynak veritabanınızı aynı bölgede olması önerilir. 

* **SSL’yi etkinleştirme:** Azure Cosmos DB sıkı güvenlik gereksinimleri ve standartlarına sahiptir. Hesabınız ile etkileşim kurarken SSL’yi etkinleştirdiğinizden emin olun. SSH ile CQL kullandığınızda, SSL bilgilerini sağlama seçeneğine sahip olursunuz.

## <a name="options-to-migrate-data"></a>Verileri geçirme seçenekleri

Verileri mevcut Cassandra iş yüklerinden Azure Cosmos DB'ye taşırken şu seçenekleri kullanabilirsiniz:

* [cqlsh COPY komutunu kullanma](#using-cqlsh-copy-command)  
* [Spark'ı kullanma](#using-spark) 

## <a name="migrate-data-using-cqlsh-copy-command"></a>cqlsh COPY komutunu kullanarak verileri geçirme

[CQL kopyalama komutu](http://cassandra.apache.org/doc/latest/tools/cqlsh.html#cqlsh) Azure Cosmos DB'de Cassandra API hesabı için yerel veri kopyalamak için kullanılır. Verileri kopyalamak için aşağıdaki adımları kullanın:

1. Cassandra API hesabınızın bağlantı dizesi bilgilerini alın:

   * Oturum [Azure portalında](https://portal.azure.com)ve Azure Cosmos hesabınıza gidin.

   * cqlsh'den Cassandra API hesabınıza bağlanmak için ihtiyacınız olan tüm bilgilerin yer aldığı **Bağlantı Dizesi** bölmesini açın.

2. Portaldan bağlantı bilgilerini kullanarak cqhsh oturumu açın.

3. Yerel verileri Cassandra API hesabına kopyalamak için CQL COPY komutunu kullanın.

   ```bash
   COPY exampleks.tablename FROM filefolderx/*.csv 
   ```

## <a name="migrate-data-using-spark"></a>Spark'ı kullanarak verileri geçirme 

Verileri Spark ile Cassandra API'si hesabına geçirmek için aşağıdaki adımları kullanın:

- Sağlama bir [Azure Databricks kümesiyle](cassandra-spark-databricks.md) veya [HDInsight kümesi](cassandra-spark-hdinsight.md) 

- Kullanarak hedef Cassandra API uç noktası için veri taşıma [Tablo kopyalama işlemi](cassandra-spark-table-copy-ops.md) 

Geçiş Spark işleri'ni kullanarak verileri var olan bir kümedeki Azure sanal makineleri veya diğer herhangi bir buluttaki bulunan veriler varsa önerilen seçenek olan. Bu seçenek, Spark'ın aracı olarak bir saat veya normal alımı için ayarlanması gerekir. Bu geçiş, şirket içi ve Azure arasında Azure ExpressRoute bağlantısı kullanarak hızlandırabilirsiniz. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık ihtiyaç duyulan, kaynak grubu, Azure Cosmos hesabı ve tüm ilgili kaynakları silin. Bunu yapmak için select sanal makinenin kaynak grubunu seçin. **Sil**ve ardından silmek için kaynak grubunun adını onaylayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure Cosmos DB'de Cassandra API hesabı, verilerinizi geçirmeyi öğrendiniz. Şimdi diğer Azure Cosmos DB kavramları hakkında bilgi edinmek için şu makaleye geçebilirsiniz:

> [!div class="nextstepaction"]
> [Azure Cosmos DB'deki ayarlanabilir tutarlılık düzeyleri](../cosmos-db/consistency-levels.md)


