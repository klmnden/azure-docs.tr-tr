---
title: Azure Cosmos Veritabanına Cassandra veri alma | Microsoft Docs
description: Azure Cosmos Veritabanına Cassandra verileri kopyalamak için CQL Copy komutu kullanmayı öğrenin.
services: cosmos-db
author: govindk
manager: kfile
documentationcenter: ''
ms.assetid: eced5f6a-3f56-417a-b544-18cf000af33a
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/15/2017
ms.author: govindk
ms.custom: mvc
ms.openlocfilehash: 64f60e6beb5451d8f5acd382ca8e5672a2d096f6
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="azure-cosmos-db-import-cassandra-data"></a>Azure Cosmos DB: Alma Cassandra veri

Bu öğretici yönergeleri Cassandra verileri Azure Cosmos Veritabanına alma Cassandra sorgu dili (CQL) kopyalama komutunu kullanarak sağlar. 

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Bağlantı dizesi alınıyor
> * Cqlsh kopyalama komutunu kullanarak veri içeri aktarma
> * Spark Bağlayıcısı'nı kullanarak içeri aktarma 

# <a name="prerequisites"></a>Önkoşullar

* Yükleme [Apache Cassandra](http://cassandra.apache.org/download/) ve özellikle olun *cqlsh* mevcuttur.
* Verimliliğini artırmak: veri geçişinizi süresini tablolarınız için sağlanan işleme miktarına bağlıdır. Büyük veri geçişler verimliliğini artırmak emin olun. Geçişi tamamladıktan sonra maliyet tasarrufu sağlamak verimliliği azaltır. Performansı artırma hakkında daha fazla bilgi için [Azure portal](https://portal.azure.com), bkz: [Azure Cosmos DB kapsayıcıları için kümesi işleme](set-throughput.md).
* SSL'yi etkinleştirin: Azure Cosmos DB sıkı güvenlik gereksinimlerine ve standartları vardır. Hesabınız ile etkileşim kurarken SSL'yi emin olun. SSH ile CQL kullandığınızda, SSL bilgileri sağlamak için bir seçeneğiniz vardır. 

## <a name="find-your-connection-string"></a>Bağlantı dizenizi Bul

1. İçinde [Azure portal](https://portal.azure.com), üzerinde şu ana kadar sol tıklatın **Azure Cosmos DB**.

2. İçinde **abonelikleri** bölmesinde, hesabınızın adını seçin.

3. Tıklatın **bağlantı dizesi**. Sağ bölmede başarıyla hesabınıza bağlanmak için gereken tüm bilgileri içerir.

    ![Bağlantı dizesi sayfası](./media/cassandra-import-data/keys.png)

## <a name="use-cqlsh-copy"></a>Cqlsh kopyalama kullanın

Cassandra API ile kullanmak için Azure Cosmos Veritabanına Cassandra veri almak için aşağıdaki yönergeleri kullanın:

1. Cqhsh portalından bağlantı bilgilerini kullanarak oturum açın.
2. Kullanım [CQL COPY komutu](http://cassandra.apache.org/doc/latest/tools/cqlsh.html#cqlsh) yerel veri Apache Cassandra API uç noktasına kopyalamak için. Kaynak ve hedef aynı veri merkezinde gecikmesi sorunları en aza indirmek için olduğundan emin olun.

### <a name="guide-for-moving-data-with-cqlsh"></a>Cqlsh verilerle taşıma için kılavuz

1. Önceden oluşturma ve tablonuzu ölçeği:
    * Varsayılan olarak, Azure Cosmos DB hükümleri yeni bir Cassandra API 1.000 istek birimleri (RU/s (CQL tabanlı oluşturma 400 RU/s ile sağlanır)) saniyede içeren tablo. Cqlsh kullanarak Geçişe başlamadan önce tüm tablolardan önceden oluştur [Azure portal](https://portal.azure.com) veya cqlsh. 

    * Gelen [Azure portal](https://portal.azure.com), tablolarınızı verimini varsayılan işleme (400 veya 1000 RU/s) 10.000 RU/s için geçiş süresince artırın. Daha yüksek işleme ile azaltma önlemek ve daha kısa sürede geçirilir. Azure Cosmos DB'de saatlik faturalandırma ile maliyet tasarrufu sağlamak hemen geçişten sonra işleme azaltabilir.

2. Bir işlemin RU ücret belirler. Tercih ettiğiniz Azure Cosmos DB Cassandra API SDK kullanarak bunu yapabilirsiniz. Bu örnek RU ücretleri alınıyor .NET sürümünü gösterir. 

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

3. Azure Cosmos DB hizmetine makinenizden gecikme süresini belirler. Bir Azure veri merkezi içinde varsa, gecikme düşük tek basamaklı milisaniyelik bir sayı olmalıdır. Azure veri merkezi dışında varsa, daha sonra psping veya azurespeed.com yaklaşık gecikme bulunduğunuz yerden almak için kullanabilirsiniz.   

4. Uygun iyi performans sağlayan parametrelerin değerlerini (NUMPROCESS, INGESTRATE, MAXBATCHSIZE veya MINBATCHSIZE) hesaplar. 

5. Son geçiş komutunu çalıştırın. Bu komutu çalıştırmak bağlantı dizesi bilgilerini kullanarak cqlsh başlatılmışsa varsayar.

   ```
   COPY exampleks.tablename FROM filefolderx/*.csv 
   ```

## <a name="use-spark-to-import-data"></a>Veri almak için Spark kullanma

Azure sanal makineleri varolan bir kümede bulunan verileri için Spark kullanarak veriyi içeri aktarma uygun bir seçenek değil. Bu, Spark aracı gibi bir kez veya normal alımı için ayarlanmasını gerektirir. 

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, aşağıdaki görevlerin nasıl öğrendiniz:

> [!div class="checklist"]
> * Bağlantı dizesi alma
> * Cql kopyalama komutunu kullanarak veri içeri aktar
> * Spark Bağlayıcısı'nı kullanarak içeri aktarma 

Artık Azure Cosmos DB hakkında daha fazla bilgi için kavramları bölümüne geçebilirsiniz. 

> [!div class="nextstepaction"]
>[İnce ayarlanabilir veri tutarlılık düzeylerini Azure Cosmos veritabanı](../cosmos-db/consistency-levels.md)
