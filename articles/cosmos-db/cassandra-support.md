---
title: Azure Cosmos DB Cassandra API'si tarafından desteklenen Apache Cassandra özellikleri ve komutları
description: Azure Cosmos DB Cassandra API'sinde Apache Cassandra özellik desteği hakkında bilgi edinin
author: kanshiG
ms.author: govindk
ms.reviewer: sngun
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: overview
ms.date: 09/24/2018
ms.openlocfilehash: 46eea21e1eafce1696ed1cf77a1f334798f0bc17
ms.sourcegitcommit: 04716e13cc2ab69da57d61819da6cd5508f8c422
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2019
ms.locfileid: "58848407"
---
# <a name="apache-cassandra-features-supported-by-azure-cosmos-db-cassandra-api"></a>Azure Cosmos DB Cassandra API'si tarafından desteklenen Apache Cassandra özellikleri 

Azure Cosmos DB, Microsoft'un genel olarak dağıtılmış çok modelli veritabanı hizmetidir. Azure Cosmos DB Cassandra API'si ile, Cassandra Sorgu Dili (CQL) v4 [kablo protokolü](https://github.com/apache/cassandra/blob/trunk/doc/native_protocol_v4.spec) uyumlu açık kaynak Cassandra istemci [sürücüleri](https://cassandra.apache.org/doc/latest/getting_started/drivers.html?highlight=driver) aracılığıyla iletişim kurabilirsiniz. 

Azure Cosmos DB Cassandra API’sini kullanarak Apache Cassandra API’leri avantajlarının yanı sıra Azure Cosmos DB’nin sunduğu kurumsal özelliklerin keyfini çıkarabilirsiniz. [Genel dağıtım](distribute-data-globally.md), [otomatik ölçek genişletme bölümlemesi](partition-data.md), kullanılabilirlik ve gecikme süresi garantileri, REST’te şifreleme, yedeklemeler ve çok daha fazlası kurumsal özelliklere dahildir.

## <a name="cassandra-protocol"></a>Cassandra protokolü 

Azure Cosmos DB Cassandra API'si CQL’nin **v4** sürümüyle uyumludur. Desteklenen CQL komutları, araçlar, sınırlamalar ve özel durumlar aşağıda listelenmiştir. Bu protokolleri anlayan bir istemci sürücüsü Azure Cosmos DB Cassandra API'sine bağlanabilir.

## <a name="cassandra-driver"></a>Cassandra sürücü

Cassandra sürücülerinin aşağıdaki sürümleri, Azure Cosmos DB Cassandra API'si tarafından desteklenir:

* [Java 3.5+](https://github.com/datastax/java-driver)  
* [C# 3.5+](https://github.com/datastax/csharp-driver)  
* [Nodejs 3.5+](https://github.com/datastax/nodejs-driver)  
* [Python 3.15+](https://github.com/datastax/python-driver)  
* [C++ 2.9](https://github.com/datastax/cpp-driver)   
* [PHP 1.3](https://github.com/datastax/php-driver)  
* [Gocql](https://github.com/gocql/gocql)  
 
## <a name="cql-data-types"></a>CQL veri türleri 

Azure Cosmos DB Cassandra API'si aşağıdaki CQL veri türlerini destekler:

* ascii  
* bigint  
* blob  
* boole  
* counter  
* date  
* decimal  
* double  
* float  
* frozen  
* inet  
* int  
* list  
* set  
* smallint  
* metin  
* time  
* timestamp  
* timeuuid  
* tinyint  
* tuple  
* uuid  
* varchar  
* varint  
* tuples  
* udts  
* map  

## <a name="cql-functions"></a>CQL işlevleri

Azure Cosmos DB Cassandra API'si aşağıdaki CQL işlevlerini destekler:

* Belirteç  
* Blob dönüşüm işlevleri 
  * typeAsBlob(value)  
  * blobAsType(value)
* UUID ve timeuuid işlevleri 
  * dateOf()  
  * now()  
  * minTimeuuid()  
  * unixTimestampOf()  
  * toDate(timeuuid)  
  * toTimestamp(timeuuid)  
  * toUnixTimestamp(timeuuid)  
  * toDate(timestamp)  
  * toUnixTimestamp(timestamp)  
  * toTimestamp(date)  
  * toUnixTimestamp(date)  


## <a name="cassandra-query-language-limits"></a>Cassandra Sorgu Dili sınırları

Azure Cosmos DB Cassandra API'sinin bir tabloda depolanan verilerin boyutuna dair herhangi bir sınırlaması yoktur. Yüzlerce terabayt veya Petabaytlarca verinin depolanabilmesinin yanı sıra bölüm anahtarı sınırları da kabul edilir. Benzer şekilde her varlık ve satır eşdeğerinin sütun sayısı üzerinde sınırı yoktur, ancak varlığın toplam boyutu 2 MB’ı aşmamalıdır.

## <a name="tools"></a>Araçlar 

Azure Cosmos DB Cassandra API'si bir yönetilen hizmet platformudur. Kümeyi yönetmek için Atık Toplayıcı, Java Sanal Makinesi (JVM) ve düğüm aracı gibi yönetim ek yükü veya yardımcı programları gerekmez. İkili CQLv4 uyumluluğunu kullanan cqlsh gibi araçları destekler. 

* Azure portalın veri gezgini, ölçümler, günlük tanılaması, PowerShell ve CLI, hesabı yönetmek için desteklenen diğer mekanizmalardır.

## <a name="cql-shell"></a>CQL Kabuğu  

CQLSH komut satırı yardımcı programı, Apache Cassandra 3.1.1 ile birlikte gelir ve aşağıdaki ortam değişkenleri etkin olarak çalışır:

Aşağıdaki komutları çalıştırmadan önce [cacerts deposuna bir Baltimore kök sertifikası ekleyin](https://docs.microsoft.com/java/azure/java-sdk-add-certificate-ca-store?view=azure-java-stable#to-add-a-root-certificate-to-the-cacerts-store). 

**Windows:** 

```bash
set SSL_VERSION=TLSv1_2 
SSL_CERTIFICATE=<path to Baltimore root ca cert>
set CQLSH_PORT=10350 
cqlsh <YOUR_ACCOUNT_NAME>.cassandra.cosmosdb.azure.com 10350 -u <YOUR_ACCOUNT_NAME> -p <YOUR_ACCOUNT_PASSWORD> --ssl 
```
**Unix/Linux/Mac:**

```bash
export SSL_VERSION=TLSv1_2 
export SSL_CERTFILE=<path to Baltimore root ca cert>
cqlsh <YOUR_ACCOUNT_NAME>.cassandra.cosmosdb.azure.com 10350 -u <YOUR_ACCOUNT_NAME> -p <YOUR_ACCOUNT_PASSWORD> --ssl 
```

## <a name="cql-commands"></a>CQL komutları

Azure Cosmos DB, Cassandra API'si hesaplarında aşağıdaki veritabanı komutlarını destekler.

* CREATE KEYSPACE 
* CREATE TABLE 
* ALTER TABLE 
* USE 
* INSERT 
* SELECT 
* UPDATE 
* BATCH- Yalnızca günlüğe kaydedilmemiş komutlar desteklenir 
* DELETE

CQLV4 uyumlu SDK aracılığıyla yürütülen tüm crud işlemleri; hata, kullanılan istek birimleri ve etkinlik kimliği ile ek bilgileri döndürür. Sağlanan kaynakların fazla kullanımından kaçınmak amacıyla silme ve güncelleştirme komutlarının kaynak idaresiyle işlenmesi gerekir. 
* Not: gc_grace_seconds değeri belirtilmişse sıfır olmalıdır.

```csharp
var tableInsertStatement = table.Insert(sampleEntity); 
var insertResult = await tableInsertStatement.ExecuteAsync(); 
 
foreach (string key in insertResult.Info.IncomingPayload) 
        { 
            byte[] valueInBytes = customPayload[key]; 
            double value = Encoding.UTF8.GetString(valueInBytes); 
            Console.WriteLine($“CustomPayload:  {key}: {value}”); 
        } 
```

## <a name="consistency-mapping"></a>Tutarlılık eşleme 

Azure Cosmos DB Cassandra API'si okuma işlemleri için tutarlılık sunar. Tüm yazma işlemleri, hesap tutarlılığından bağımsız olarak her zaman yazma performansı SLA'ları ile yazılır.

## <a name="permission-and-role-management"></a>İzin ve rol yönetimi

Azure Cosmos DB, rol tabanlı erişim denetimini (RBAC) ve [Azure portal](https://portal.azure.com) aracılığıyla edinilebilecek okuma-yazma ve salt-okuma parolalarını/anahtarlarını destekler. Azure Cosmos DB henüz veri düzlemi etkinlikleri için kullanıcıları ve rolleri desteklememektedir. 

## <a name="planned-support"></a>Planlı destek 
* Anahtar alanı oluşturma komutundaki bölge adı şu an için yoksayılmaktadır. Veri dağıtımı temel alınan Cosmos DB platformunda gerçekleştirilmekte ve portal ya da PowerShell ile hesapta kullanıma sunulmaktadır. 





## <a name="next-steps"></a>Sonraki adımlar

- Java uygulaması kullanarak [Cassandra API hesabı, veritabanı ve tablo oluşturmaya](create-cassandra-api-account-java.md) başlama

