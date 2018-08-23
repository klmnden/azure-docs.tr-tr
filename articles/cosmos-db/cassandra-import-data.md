---
title: Cassandra verilerini Azure Cosmos DB’ye aktarma | Microsoft Docs
description: Cassandra verilerini Azure Cosmos DB içine kopyalamak için CQL Kopyala komutunu kullanmayı öğrenin.
services: cosmos-db
author: kanshiG
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-cassandra
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 11/15/2017
ms.author: govindk
ms.custom: mvc
ms.openlocfilehash: b53328875f2242faba369dea0df655bc78117009
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2018
ms.locfileid: "41920636"
---
# <a name="azure-cosmos-db-import-cassandra-data"></a>Azure Cosmos DB: Cassandra verilerini içeri aktarma

Bu öğreticide Cassandra Sorgu Dili (CQL) COPY komutu kullanılarak Cassandra verilerini Azure Cosmos DB’ye içeri aktarma yönergeleri sağlanmaktadır. 

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Bağlantı dizenizi alma
> * Cqlsh COPY komutunu kullanarak verileri içeri aktarma
> * Spark bağlayıcısını kullanarak içeri aktarma 

# <a name="prerequisites"></a>Ön koşullar

* [Apache Cassandra](http://cassandra.apache.org/download/)’yı yükleyin ve özellikle *cqlsh* öğesinin bulunduğundan emin olun.
* Aktarım hızını artırma: Veri geçişinizin süresi, Tablolarınız için sağladığınız aktarım hızı miktarına bağlıdır. Büyük veri geçişleri için aktarım hızını artırdığınızdan emin olun. Geçişi tamamladıktan sonra maliyet tasarrufu sağlamak için aktarım hızını azaltın. [Azure portalında](https://portal.azure.com) aktarım hızını artırma hakkında daha fazla bilgi için bkz. [Azure Cosmos DB kapsayıcıları için aktarım hızını ayarlama](set-throughput.md).
* SSL’yi etkinleştir: Azure Cosmos DB sıkı güvenlik gereksinimleri ve standartlarına sahiptir. Hesabınız ile etkileşim kurarken SSL’yi etkinleştirdiğinizden emin olun. SSH ile CQL kullandığınızda, SSL bilgilerini sağlama seçeneğine sahip olursunuz. 

## <a name="find-your-connection-string"></a>Bağlantı dizenizi bulma

1. [Azure portalında](https://portal.azure.com), en solda **Azure Cosmos DB** seçeneğine tıklayın.

2. **Abonelikler** bölmesinde, hesabınızın adını seçin.

3. **Bağlantı Dizesi**’ne tıklayın. Sağ bölme, hesabınıza başarıyla bağlanmak için gereken tüm bilgileri içerir.

    ![Bağlantı dizesi sayfası](./media/cassandra-import-data/keys.png)

## <a name="use-cqlsh-copy"></a>cqlsh COPY kullanma

Cassandra verilerini Cassandra API’si ile birlikte kullanmak üzere Azure Cosmos DB’ye içeri aktarmak için aşağıdaki kılavuzu kullanın:

1. Portaldan bağlantı bilgilerini kullanarak cqhsh oturumu açın.
2. Yerel verileri Apache Cassandra API uç noktasına kopyalamak için [CQL COPY komutunu](http://cassandra.apache.org/doc/latest/tools/cqlsh.html#cqlsh) kullanın. Gecikme sorunlarını en aza indirmek için kaynak ve hedefin aynı veri merkezinde olduğundan emin olun.

### <a name="guide-for-moving-data-with-cqlsh"></a>Cqlsh ile veri taşıma kılavuzu

1. Tablonuzu önceden oluşturup ölçeklendirin:
    * Varsayılan olarak, Azure Cosmos DB yeni bir Cassandra API tablosunu saniyede 1.000 istek birimiyle (RU/s) sağlar (CQL tabanlı oluşturma 400 RU/s ile sağlanır). Cqlsh kullanarak geçişe başlamadan önce, [Azure portal](https://portal.azure.com) veya cqlsh’den tüm tablolarınızı önceden oluşturun. 

    * [Azure portalından](https://portal.azure.com), geçiş süresi boyunca tablolarınızın aktarım hızını varsayılan değerden (400 veya 1000 RU/s) 10.000 RU/s değerine artırın. Daha yüksek aktarım hızı ile, hız sınırlamayı önleyebilir ve daha kısa sürede geçişi tamamlayabilirsiniz. Azure Cosmos DB’de saatlik faturalandırma ile, maliyet tasarrufu sağlamak için geçiş işleminden hemen sonra aktarım hızını azaltabilirsiniz.

2. Bir işlem için RU ücretini belirleyin. Bunu istediğiniz Azure Cosmos DB Cassandra API SDK’sını kullanarak yapabilirsiniz. Bu örnekte .NET sürümünün RU ücretleri gösterilmektedir. 

    ```csharp
    var tableInsertStatement = table.Insert(sampleEntity);
    var insertResult = await tableInsertStatement.ExecuteAsync();

    foreach (string key in insertResult.Info.IncomingPayload)
            {
                byte[] valueInBytes = customPayload[key];
                string value = Encoding.UTF8.GetString(valueInBytes);
                Console.WriteLine($“CustomPayload:  {key}: {value}”);
            }
 
    ``` 

3. Makinenizden Azure Cosmos DB hizmetine gecikmeyi belirleyin. Bir Azure Veri merkezindeyseniz, gecikme düşük bir tek basamaklı milisaniyelik sayı olmalıdır. Azure Veri Merkezi’nin dışındaysanız, konumunuzdan yaklaşık uzaklığı almak için psping veya azurespeed.com kullanabilirsiniz.   

4. İyi performans sağlayan parametreler (NUMPROCESS, INGESTRATE, MAXBATCHSIZE veya MINBATCHSIZE) için uygun değerleri hesaplayın. 

5. Son geçiş komutunu çalıştırın. Bu komutun çalıştırılması, cqlsh’yi bağlantı dizesini bilgilerini kullanarak başlattığınızı varsayar.

   ```
   COPY exampleks.tablename FROM filefolderx/*.csv 
   ```

## <a name="use-spark-to-import-data"></a>Verileri içeri aktarmak için Spark kullanma

Azure sanal makinelerinde mevcut bir kümede bulunan veriler için, verileri Spark kullanarak içeri aktarmak da uygun bir seçenektir. Bu, Spark’ın bir kez veya normal alma için aracı olarak ayarlanmasını gerektirir. 

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide aşağıdaki görevleri tamamlamayı öğrendiniz:

> [!div class="checklist"]
> * Bağlantı dizenizi alma
> * Cql copy komutunu kullanarak verileri içeri aktarma
> * Spark bağlayıcısını kullanarak içeri aktarma 

Şimdi Azure Cosmos DB hakkında daha fazla bilgi için Kavramlar bölümüne geçebilirsiniz. 

> [!div class="nextstepaction"]
>[Azure Cosmos DB'deki ayarlanabilir tutarlılık düzeyleri](../cosmos-db/consistency-levels.md)
