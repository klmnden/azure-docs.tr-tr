---
title: Verilerinizi Azure Cosmos DB Cassandra API hesabına geçirme
description: CQL Copy komutuyla Spark'ı kullanarak verileri Apache Cassandra'dan Azure Cosmos DB Cassandra API'ye kopyalamayı öğrenin.
services: cosmos-db
author: kanshiG
ms.service: cosmos-db
ms.component: cosmosdb-cassandra
ms.author: govindk
ms.topic: tutorial
ms.date: 09/24/2018
ms.reviewer: sngun
ms.openlocfilehash: 0bf5e47513ded4b2c65e7291db497e53a42776a8
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46976185"
---
# <a name="migrate-your-data-to-azure-cosmos-db-cassandra-api-account"></a>Verilerinizi Azure Cosmos DB Cassandra API hesabına geçirme

Bu öğreticide Apache Cassandra verilerini Azure Cosmos DB Cassandra API'ye geçirme yönergeleri sağlanır. 

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Geçiş planlaması
> * Geçiş önkoşulları
> * cqlsh COPY komutunu kullanarak verileri geçirme
> * Spark'ı kullanarak verileri geçirme 

## <a name="plan-for-migration"></a>Geçiş planlaması

Verileri Azure Cosmos DB Cassandra API'ye geçirmeden önce, iş yükünüzün aktarım hızı gereksinimlerini tahmin etmelisiniz. Genel olarak, CRUD işlemlerine gereken ortalama aktarım hızıyla başlamanız ve ardından Ayıklama Dönüştürme Yükleme (ETL) için veya öngörülemeyen işlemler için gereken fazladan aktarım hızını eklemeniz önerilir. Geçişi planlamak için şu ayrıntılara ihtiyacınız vardır: 

* **Mevcut veri boyutu veya tahmini veri boyutu:** Minimum veritabanı boyutunu ve aktarım hızı gereksinimini tanımlar. Yeni uygulama için veri boyutu tahmini yapıyorsanız, verilerin satırlara düzgün dağıtıldığını varsayabilir ve veri boyutuyla çarparak değeri tahmin edebilirsiniz. 

* **Gerekli aktarım hızı:** Yaklaşık okuma (sorgulama/alma) ve yazma (güncelleştirme/silme/ekleme) aktarım hızı. Bu değer hem gerekli istek birimlerini hem de eylemsizlik durumunda veri boyutu hesaplamak için gereklidir.  

* **Şemayı alma:** Mevcut Cassandra kümenize cqlsh ile bağlanın ve şemayı Cassandra'dan dışarı aktarın: 

  ```bash
  cqlsh [IP] "-e DESC SCHEMA" > orig_schema.cql
  ```

Mevcut iş yükünüzün gereksinimlerini tanımladıktan sonra, toplanan aktarım hızı gereksinimlerine göre bir Azure Cosmos DB hesabı, veritabanı ve kapsayıcılar oluşturmanız gerekir.  

* **Bir işlem için RU ücretini saptama:** İstediğiniz Azure Cosmos DB Cassandra API SDK'sını kullanarak RU'ları saptayabilirsiniz. Bu örnekte .NET sürümünün RU ücretleri gösterilmektedir.

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

## <a name="prerequisites-for-migration"></a>Geçiş önkoşulları

* **Azure Cosmos DB Cassandra API hesabında tabloları oluşturma:** Verileri geçirmeye başlamadan önce, Azure portalından veya cqlsh'den tüm tablolarınızı oluşturun.

* **Aktarım hızını artırma:** Veri geçişinizin süresi, Azure Cosmos DB'deki tablolar için sağladığınız aktarım hızı miktarına bağlıdır. Geçiş süresince aktarım hızını artırın. Daha yüksek aktarım hızı ile, hız sınırlamayı önleyebilir ve daha kısa sürede geçişi tamamlayabilirsiniz. Geçişi tamamladıktan sonra maliyet tasarrufu sağlamak için aktarım hızını azaltın. Aktarım hızını artırma hakkında daha fazla bilgi için Azure Cosmos DB kapsayıcıları için [aktarım hızını ayarlama](set-throughput.md) konusuna bakın. Ayrıca Azure Cosmos DB hesabınızın kaynak veritabanınızla aynı bölgede olması da önerilir. 

* **SSL’yi etkinleştirme:** Azure Cosmos DB sıkı güvenlik gereksinimleri ve standartlarına sahiptir. Hesabınız ile etkileşim kurarken SSL’yi etkinleştirdiğinizden emin olun. SSH ile CQL kullandığınızda, SSL bilgilerini sağlama seçeneğine sahip olursunuz.

## <a name="options-to-migrate-data"></a>Verileri geçirme seçenekleri

Verileri mevcut Cassandra iş yüklerinden Azure Cosmos DB'ye taşırken şu seçenekleri kullanabilirsiniz:

* [cqlsh COPY komutunu kullanma](#using-cqlsh-copy-command)  
* [Spark'ı kullanma](#using-spark) 

## <a name="migrate-data-using-cqlsh-copy-command"></a>cqlsh COPY komutunu kullanarak verileri geçirme

[CQL COPY komutu](http://cassandra.apache.org/doc/latest/tools/cqlsh.html#cqlsh), yerel verileri Azure Cosmos DB Cassandra API hesabına kopyalamak için kullanılır. Verileri kopyalamak için aşağıdaki adımları kullanın:

1. Cassandra API hesabınızın bağlantı dizesi bilgilerini alın:

   * [Azure portalında](https://portal.azure.com) oturum açın ve Azure Cosmos DB hesabınıza gidin.

   * cqlsh'den Cassandra API hesabınıza bağlanmak için ihtiyacınız olan tüm bilgilerin yer aldığı **Bağlantı Dizesi** bölmesini açın.

2. Portaldan bağlantı bilgilerini kullanarak cqhsh oturumu açın.

3. Yerel verileri Cassandra API hesabına kopyalamak için CQL COPY komutunu kullanın.

   ```bash
   COPY exampleks.tablename FROM filefolderx/*.csv 
   ```

## <a name="migrate-data-using-spark"></a>Spark'ı kullanarak verileri geçirme 

Verileri Azure Cosmos DB Cassandra API'ye Spark'la geçirmek için aşağıdaki adımları kullanın:

- Bir [Azure Databricks](cassandra-spark-databricks.md) veya [HDInsight kümesi](cassandra-spark-hdinsight.md) sağlayın 

- Verileri hedef Cassandra API uç noktasına taşımak için [tablo kopyalama işlemini](cassandra-spark-table-copy-ops.md) kullanın 

Azure sanal makinelerindeki veya başka herhangi bir buluttaki mevcut kümede bekleyen verileriniz varsa, verilerin Spark işleri kullanılarak geçirilmesi önerilir. Bu, Spark’ın bir kez veya düzenli alma için aracı olarak ayarlanmasını gerektirir. Şirket içi ile Azure arasında Express Route bağlantısı kullanarak bu geçişi hızlandırabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide verilerinizi Azure Cosmos DB Cassandra API hesabına geçirmeyi öğrendiniz. Şimdi Azure Cosmos DB hakkında daha fazla bilgi için kavramlar bölümüne geçebilirsiniz. 

> [!div class="nextstepaction"]
> [Azure Cosmos DB'deki ayarlanabilir tutarlılık düzeyleri](../cosmos-db/consistency-levels.md)


