---
title: MongoDB için Azure Cosmos DB'nin API'SİNDE depolanan verileri yönetmek için özel komutlar
description: Bu makalede, MongoDB için Azure Cosmos DB'nin API'SİNDE depolanan verileri yönetmek için özel komutları kullanmayı açıklar.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/26/2019
ms.author: sngun
ms.openlocfilehash: 238ba2722fef52d4607a7832113c03c097ef90b3
ms.sourcegitcommit: 3341598aebf02bf45a2393c06b136f8627c2a7b8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58807256"
---
# <a name="use-custom-commands-to-manage-data-stored-in-azure-cosmos-dbs-api-for-mongodb"></a>MongoDB için Azure Cosmos DB'nin API'SİNDE depolanan verileri yönetmek için özel komutları kullanma 

Azure Cosmos DB, Microsoft'un genel olarak dağıtılmış çok modelli veritabanı hizmetidir. Azure Cosmos DB API'si ile açık kaynak kullanarak MongoDB için iletişim kurabilir [MongoDB istemcisi sürücüsünü](https://docs.mongodb.org/ecosystem/drivers). Azure Cosmos DB MongoDB API'si için bağlı kalarak mevcut istemci sürücülerin kullanımını etkinleştirir. [MongoDB kablo protokolüne](https://docs.mongodb.org/manual/reference/mongodb-wire-protocol).

MongoDB için Azure Cosmos DB'nin API'sini kullanarak, Cosmos DB genel dağıtım, otomatik parçalanmasını, yüksek kullanılabilirlik, gecikme süresi garanti eder, otomatik, yedeklemeler, bekleyen şifreleme gibi avantajlardan yararlanabilirsiniz ve çok daha fazlasını yatırımlarınızı koruma while MongoDB uygulamanızda.

## <a name="mongodb-protocol-support"></a>MongoDB için protokol desteği

Varsayılan olarak, Azure Cosmos DB API, MongoDB MongoDB daha fazla ayrıntı için sunucu sürümü 3.2, uyumlu olduğu için bkz. [desteklenen özellikleri ve söz dizimi](mongodb-feature-support.md). Özellikleri veya MongoDB 3.4 sürümü eklenen sorgu işleçleri şu anda bir Azure Cosmos DB MongoDB API'si önizleme olarak kullanılabilir. Aşağıdaki özel komutları belirli işlevleri Azure Cosmos DB, MongoDB için Azure Cosmos DB'nin API'SİNDE depolanan veriler üzerinde CRUD işlemleri gerçekleştirirken destekler:

* [Veritabanı oluşturma](#create-database)
* [Veritabanını Güncelleştir](#update-database)
* [Veritabanı Al](#get-database)
* [Koleksiyon oluşturma](#create-collection)
* [Koleksiyonu güncelleştir](#update-collection)
* [Koleksiyonu alma](#get-collection)

## <a id="create-database"></a> Veritabanı oluşturma

Create database özel komutu yeni bir MongoDB veritabanı oluşturur. Veritabanı adı karşı komutu yürütülmeden veritabanları bağlamdan kullanılır. CreateDatabase komut biçimi aşağıdaki gibidir:

```
{
  customAction: "CreateDatabase",
  offerThroughput: <Throughput that you want to provision on the database>
}
```

Komut içinde Parametreler aşağıdaki tabloda açıklanmaktadır:

|**Alan**|**Tür** |**Açıklama** |
|---------|---------|---------|
| Özel   |  string  |   Ad özel komut, "CreateDatabase" olmalıdır.      |
| offerThroughput | int  | Veritabanı üzerinde ayarladığınız sağlanan aktarım hızı. Bu parametre isteğe bağlıdır. |

### <a name="output"></a>Çıktı

Varsayılan özel komut yanıtı döndürür. Bkz: [varsayılan çıkış](#default-output) çıktıyı parametrelerle için özel komutu.

### <a name="examples"></a>Örnekler

**Veritabanı oluşturma**

"Test" adlı bir veritabanı oluşturmak için aşağıdaki komutu kullanın:

```shell
use test
db.runCommand({customAction: "CreateDatabase"});
```

**Aktarım hızı ile veritabanı oluşturma**

"Test" ve sağlanan aktarım hızı 1000 RU adlı bir veritabanı oluşturmak için aşağıdaki komutu kullanın:

```shell
use test
db.runCommand({customAction: "CreateDatabase", offerThroughput: 1000 });
```

## <a id="update-database"></a> Veritabanını Güncelleştir

Güncelleştirme veritabanı özel komut belirtilen veritabanıyla ilişkili özelliklerini güncelleştirir. Şu anda yalnızca "offerThroughput" özelliğini güncelleştirebilirsiniz.

```
{
  customAction: "UpdateDatabase",
  offerThroughput: <New throughput that you want to provision on the database> 
}
```

Komut içinde Parametreler aşağıdaki tabloda açıklanmaktadır:

|**Alan**|**Tür** |**Açıklama** |
|---------|---------|---------|
| Özel    |    string     |   Özel komut adı. "UpdateDatabase" olmalıdır.      |
|  offerThroughput   |  int       |     Veritabanı üzerinde ayarlamak istediğiniz yeni sağlanan aktarım hızı.    |

### <a name="output"></a>Çıktı

Varsayılan özel komut yanıtı döndürür. Bkz: [varsayılan çıkış](#default-output) çıktıyı parametrelerle için özel komutu.

### <a name="examples"></a>Örnekler

**Güncelleştirme bir veritabanıyla ilişkili sağlanan aktarım hızı**

Sağlanan aktarım hızı bir veritabanının 1200 RU'ları için "test" adı ile güncelleştirmek için aşağıdaki komutu kullanın:

```shell
use test
db.runCommand({customAction: "UpdateDatabase", offerThroughput: 1200 });
```

## <a id="get-database"></a> Veritabanı Al

Get veritabanı özel komut veritabanı nesnesi döndürür. Veritabanı adı, veritabanı bağlamında karşı komutu yürütülmeden kullanılır.

```
{
  customAction: "GetDatabase"
}
```

Komut içinde Parametreler aşağıdaki tabloda açıklanmaktadır:


|**Alan**|**Tür** |**Açıklama** |
|---------|---------|---------|
|  Özel   |   string      |   Özel komut adı. "Getcollection" olmalıdır|
        
### <a name="output"></a>Çıktı

Komut başarılı olursa, yanıt bir belgesiyle aşağıdaki alanları içerir:

|**Alan**|**Tür** |**Açıklama** |
|---------|---------|---------|
|  `ok`   |   `int`     |   Yanıt durumu. 1 == başarılı. 0 hata ==.      |
| `database`    |    `string`        |   Veritabanının adı.      |
|   `provisionedThroughput`  |    `int`      |    Veritabanı üzerinde ayarlanır sağlanan aktarım hızı. Bu isteğe bağlı bir yanıt parametredir.     |

Komut başarısız olursa, varsayılan bir özel komut yanıt döndürülür. Bkz: [varsayılan çıkış](#default-output) çıktıyı parametrelerle için özel komutu.

### <a name="examples"></a>Örnekler

**Veritabanı Al**

"Test" adlı bir veritabanı için veritabanı nesnesini almak için aşağıdaki komutu kullanın:

```shell
use test
db.runCommand({customAction: "GetDatabase"});
```

## <a id="create-collection"></a> Koleksiyon oluşturma

Koleksiyon özel oluşturma komutu, yeni bir MongoDB koleksiyonu oluşturur. Veritabanı adı karşı komutu yürütülmeden veritabanları bağlamdan kullanılır. CreateCollection komut biçimi aşağıdaki gibidir:

```
{
  customAction: "CreateCollection",
  collection: <Collection Name>,
  offerThroughput: <Throughput that you want to provision on the collection>,
  shardKey: <Shard key path>  
}
```

Komut içinde Parametreler aşağıdaki tabloda açıklanmaktadır:

|**Alan**|**Tür** |**Açıklama** |
|---------|---------|---------|
| Özel    | string | Özel komut adı. "CreateDatabase" olmalıdır     |
| koleksiyon      | string | Koleksiyon adı                                   |
| offerThroughput | int    | Veritabanını ayarlamak için sağlanan aktarım hızı'ı seçin. İsteğe bağlı bir parametredir |
| shardKey        | string | Parçalı koleksiyon oluşturmak için parça anahtarı yolu. İsteğe bağlı bir parametredir |

### <a name="output"></a>Çıktı

Varsayılan özel komut yanıtı döndürür. Bkz: [varsayılan çıkış](#default-output) çıktıyı parametrelerle için özel komutu.

### <a name="examples"></a>Örnekler

**Unsharded koleksiyon oluşturma**

Adı "testCollection" ve sağlanan aktarım hızı 1000 RU ile unsharded koleksiyonu oluşturmak için aşağıdaki komutu kullanın: 

```shell
use test
db.runCommand({customAction: "CreateCollection", collection: "testCollection", offerThroughput: 1000});
``` 

**Parçalı koleksiyon oluşturma**

Adı "testCollection" ve sağlanan aktarım hızı 1000 RU ile bir parçalı koleksiyon oluşturmak için aşağıdaki komutu kullanın:

```shell
use test
db.runCommand({customAction: "CreateCollection", collection: "testCollection", offerThroughput: 1000, shardKey: "a.b" });
```

## <a id="update-collection"></a> Koleksiyonu güncelleştir

Güncelleştirme koleksiyonu özel komut belirtilen koleksiyonla ilişkili özelliklerini güncelleştirir.

```
{
  customAction: "UpdateCollection",
  collection: <Name of the collection that you want to update>,
  offerThroughput: <New throughput that you want to provision on the collection> 
}
```

Komut içinde Parametreler aşağıdaki tabloda açıklanmaktadır:

|**Alan**|**Tür** |**Açıklama** |
|---------|---------|---------|
|  Özel   |   string      |   Özel komut adı. "UpdateCollection" olmalıdır.      |
|  koleksiyon   |   string      |   Koleksiyonun adı.       |
| offerThroughput   |int|   Koleksiyonda ayarlamak için sağlanan aktarım hızı'ı seçin.|

## <a name="output"></a>Çıktı

Varsayılan özel komut yanıtı döndürür. Bkz: [varsayılan çıkış](#default-output) çıktıyı parametrelerle için özel komutu.

### <a name="examples"></a>Örnekler

**Güncelleştirme bir koleksiyonla ilişkilendirilen sağlanan aktarım hızı**

Adı "testCollection" ile bir koleksiyon için sağlanan aktarım hızına 1200 RU'ları güncelleştirmek için aşağıdaki komutu kullanın:

```shell
use test
db.runCommand({customAction: "UpdateCollection", collection: "testCollection", offerThroughput: 1200 });
```

## <a id="get-collection"></a> Koleksiyonu alma

Get koleksiyon özel komutu, koleksiyon nesnesini döndürür.

```
{
  customAction: "GetCollection",
  collection: <Name of the collection>
}
```

Komut içinde Parametreler aşağıdaki tabloda açıklanmaktadır:


|**Alan**|**Tür** |**Açıklama** |
|---------|---------|---------|
| Özel    |   string      |   Özel komut adı. "Belirtilmiş" olmalıdır.      |
| koleksiyon    |    string     |    Koleksiyonun adı.     |

### <a name="output"></a>Çıktı

Komut başarılı olursa, yanıt bir belgesiyle aşağıdaki alanları içerir.


|**Alan**|**Tür** |**Açıklama** |
|---------|---------|---------|
|  `ok`   |    `int`     |   Yanıt durumu. 1 == başarılı. 0 hata ==.      |
| `database`    |    `string`     |   Veritabanının adı.      |
| `collection`    |    `string`     |    Koleksiyonun adı.     |
|  `shardKeyDefinition`   |   `document`      |  Bir parça anahtarı kullanılan dizin belirtimi belgesi. Bu isteğe bağlı bir yanıt parametredir.       |
|  `provisionedThroughput`   |   `int`      |    Koleksiyonda ayarlamak için sağlanan aktarım hızı'ı seçin. Bu isteğe bağlı bir yanıt parametredir.     |

Komut başarısız olursa, varsayılan bir özel komut yanıt döndürülür. Bkz: [varsayılan çıkış](#default-output) çıktıyı parametrelerle için özel komutu.

### <a name="examples"></a>Örnekler

**Koleksiyonu alma**

"TestCollection" adlı bir koleksiyon için koleksiyon nesnesini almak için aşağıdaki komutu kullanın:

```shell
use test
db.runCommand({customAction: "GetCollection", collection: "testCollection"});
```

## <a id="default-output"></a> Varsayılan özel komut çıktısı

Belirtilmezse, özel bir yanıt bir belgesiyle aşağıdaki alanları içerir:

|**Alan**|**Tür** |**Açıklama** |
|---------|---------|---------|
|  `ok`   |    `int`     |   Yanıt durumu. 1 == başarılı. 0 hata ==.      |
| `code`    |   `int`      |   Komut başarısız oldu, yalnızca döndürülen (yani Tamam == 0). MongoDB hata kodunu içerir. Bu isteğe bağlı bir yanıt parametredir.      |
|  `errMsg`   |  `string`      |    Komut başarısız oldu, yalnızca döndürülen (yani Tamam == 0). Kolay bir hata iletisi içerir. Bu isteğe bağlı bir yanıt parametredir.      |

## <a name="next-steps"></a>Sonraki adımlar

Ardından aşağıdaki Azure Cosmos DB kavramları öğrenmeniz geçebilirsiniz: 

* [Azure Cosmos DB'yi dizine ekleme](../cosmos-db/index-policy.md)
* [Yaşam süresi otomatik olarak Azure Cosmos DB'de verilerle süresi dolacak](../cosmos-db/time-to-live.md)