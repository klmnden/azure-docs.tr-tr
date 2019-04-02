---
title: Azure Cosmos DB veritabanları, kapsayıcıları ve öğeleri ile çalışma
description: Bu makalede, Azure Cosmos DB veritabanları, kapsayıcıları ve öğeleri oluşturulacağı ve kullanılacağı açıklanır
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/31/2019
ms.author: rimman
ms.reviewer: sngun
ms.openlocfilehash: f3bec1b279c07e62e246ebfa933b3942e38406de
ms.sourcegitcommit: 09bb15a76ceaad58517c8fa3b53e1d8fec5f3db7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58762914"
---
# <a name="work-with-databases-containers-and-items"></a>Veritabanları, kapsayıcılar ve öğelerle çalışma

Oluşturduktan sonra bir [Azure Cosmos hesabı](account-overview.md) Azure aboneliğiniz kapsamındaki verileri hesabınızdaki veritabanları, kapsayıcılar ve öğeleri oluşturarak yönetebilirsiniz. Bu makalede bu varlıkların açıklar: veritabanları, kapsayıcılar ve öğeleri. Aşağıdaki resimde, bir Azure Cosmos hesabında farklı varlık hiyerarşisi gösterilmektedir:

![Azure Cosmos hesabı varlıklar](./media/databases-containers-items/cosmos-entities.png)

## <a name="azure-cosmos-databases"></a>Azure Cosmos veritabanları

Bir veya daha fazla Azure Cosmos veritabanı hesabınız kapsamında oluşturabilirsiniz. Bir veritabanı için bir ad alanı benzerdir. Azure Cosmos kapsayıcıları kümesi için yönetim birimidir. Aşağıdaki tablo, bir Azure Cosmos veritabanı çeşitli API özel varlıklara nasıl eşleştiğini gösterir:

| **Azure Cosmos varlığı** | **SQL API'Sİ** | **Cassandra API'si** | **Azure Cosmos DB'nin MongoDB API'si** | **Gremlin API** | **Tablo API’si** |
| --- | --- | --- | --- | --- | --- |
|Azure Cosmos veritabanı | Database | Keyspace | Database | Database | NA |

> [!NOTE]
> Tablo API'si hesapları ile varsayılan veritabanı ilk tablonuzun oluşturduğunuzda, Azure Cosmos hesabınızdaki otomatik olarak oluşturulur.

### <a name="operations-on-an-azure-cosmos-database"></a>Bir Azure Cosmos veritabanı üzerinde işlemler

Azure Cosmos API'leri ile bir Azure Cosmos veritabanı gibi etkileşim kurabilirsiniz:

| **İşlem** | **Azure CLI**|**SQL API'Sİ** | **Cassandra API'si** | **Azure Cosmos DB'nin MongoDB API'si** | **Gremlin API** | **Tablo API’si** |
| --- | --- | --- | --- | --- | --- | --- |
|Tüm veritabanları listeleme| Evet | Evet | Evet (veritabanı için bir anahtar alanı eşlendi) | Evet | NA | NA |
|Okuma veritabanı| Evet | Evet | Evet (veritabanı için bir anahtar alanı eşlendi) | Evet | NA | NA |
|Yeni veritabanı oluştur| Evet | Evet | Evet (veritabanı için bir anahtar alanı eşlendi) | Evet | NA | NA |
|Veritabanını Güncelleştir| Evet | Evet | Evet (veritabanı için bir anahtar alanı eşlendi) | Evet | NA | NA |


## <a name="azure-cosmos-containers"></a>Azure Cosmos kapsayıcıları

Bir Azure Cosmos kapsayıcı, sağlanan aktarım hızı hem de depolama için ölçeklenebilirlik birimidir. Bir kapsayıcı yatay olarak bölümlenir ve ardından birden çok bölgeye çoğaltılır. Kapsayıcı ve aktarım hızı üzerinde sağlamak için eklediğiniz öğeleri otomatik olarak bir dizi bölüm anahtarına göre mantıksal bölümler arasında dağıtılır. Bölümlendirme ve bölüm anahtarları hakkında daha fazla bilgi için bkz: [bu](partition-data.md) makalesi. 

Bir Azure Cosmos kapsayıcı oluştururken, aktarım hızı şu modlardan birini yapılandırın:

* **Adanmış sağlanan aktarım hızı** modu: Bir kapsayıcı sağlanan aktarım hızı bu kapsayıcı için özel olarak ayrılmış ve SLA'lar ile desteklenir. Daha fazla bilgi için bkz. [nasıl sağlanacağı bir Azure Cosmos kapsayıcısında aktarım hızını](how-to-provision-container-throughput.md).

* **Paylaşılan sağlanan aktarım hızı** modu: Bu kapsayıcıların sağlanan aktarım hızı (adanmış sağlanan aktarım hızı ile yapılandırılmış kapsayıcıların dışında) aynı veritabanında diğer kapsayıcılarla paylaşın. Diğer bir deyişle, veritabanı sağlanan aktarım hızını tüm "paylaşılan aktarım hızı" kapsayıcılar arasında paylaşılır. Daha fazla bilgi için bkz. [nasıl bir Azure Cosmos veritabanı'nda sağlanan aktarım hızı yapılandırma](how-to-provision-database-throughput.md).

Bir Azure Cosmos kapsayıcı ölçeklendirebilir, aktarım hızı modları, kapsayıcılar ile ya da "paylaşılan" veya "ayrılmış" oluşturduğunuz sağlanan.

Bir Azure Cosmos kapsayıcı öğeleri şemadan kapsayıcıdır. Bir kapsayıcı içindeki öğeleri rastgele şemalar bulunabilir. Örneğin, bir kişiyi temsil eden bir öğe, bir otomobilin temsil eden bir öğe yerleştirilebilir *aynı kapsayıcı*. Varsayılan olarak, herhangi bir açık dizin veya şema yönetimi gerektirmeden bir kapsayıcıya eklediğiniz tüm öğeleri bir otomatik olarak dizine. Dizin oluşturma davranışı yapılandırarak özelleştirebileceğiniz [dizin oluşturma ilkesi](index-overview.md) üzerinde bir kapsayıcı. 

Ayarlayabileceğiniz [yaşam süresi (TTL)](time-to-live.md) bir Azure Cosmos kapsayıcısındaki veya sistem dışı öğelerin düzgün bir şekilde temizlemek kapsayıcının tamamı için seçili öğeler üzerinde. Azure Cosmos DB, otomatik olarak bu süre dolduğunda öğeleri siler. Ayrıca, bir sorgu kapsayıcısı üzerinde gerçekleştirilen sabit bir sınır içinde süresi dolan öğeleri döndürmüyor garanti eder. Daha fazla bilgi için bkz. [TTL kapsayıcınızın yapılandırma](how-to-time-to-live.md).

Kullanarak [değişiklik akışı](change-feed.md), her kapsayıcınızın mantıksal bölümler için yönetilen işlem günlüğüne abone olabilirsiniz. Değişiklik akışı ile birlikte kapsayıcısı üzerinde gerçekleştirilen tüm güncelleştirmelerin günlük sağlar önce ve sonra öğeleri görüntüler. Bkz: [değişiklik akışı kullanarak reaktif uygulamalar oluşturmak nasıl](serverless-computing-database.md). Ayrıca bekletme süresini değiştirmek akışı için değişiklik kapsayıcı üzerindeki ilke akışı kullanarak yapılandırabilirsiniz. 

Kaydedebileceğiniz [saklı yordamlar, Tetikleyiciler, kullanıcı tanımlı işlevler (UDF'ler)](stored-procedures-triggers-udfs.md) ve [birleştirme yordamları](how-to-manage-conflicts.md#create-a-custom-conflict-resolution-policy-with-a-stored-procedure) Azure Cosmos kapsayıcınızı ile. 

Belirtebileceğiniz bir [benzersiz anahtar kısıtlaması](unique-keys.md) Azure Cosmos kapsayıcınızı üzerinde. Benzersiz anahtar bir ilke oluşturarak, mantıksal bölüm anahtarı başına bir veya daha fazla değerlerin benzersiz olmasını sağlamak. Bir kapsayıcı benzersiz bir anahtar ilke oluşturulduktan sonra tüm yeni veya güncelleştirilmiş öğeleri benzersiz anahtar kısıtlaması tarafından belirtilen değerleri yinelenen değerlere sahip oluşturulmasını engeller. Daha fazla bilgi için bkz. [benzersiz anahtar kısıtlamaları](unique-keys.md).

Bir Azure Cosmos kapsayıcı API özel varlıklara gibi özelleştirilmiş:

| **Azure Cosmos varlığı** | **SQL API'Sİ** | **Cassandra API'si** | **Azure Cosmos DB'nin MongoDB API'si** | **Gremlin API** | **Tablo API’si** |
| --- | --- | --- | --- | --- | --- |
|Azure Cosmos kapsayıcı | Koleksiyon | Tablo | Koleksiyon | Graf | Tablo |

### <a name="properties-of-an-azure-cosmos-container"></a>Bir Azure Cosmos kapsayıcı özellikleri

Bir Azure Cosmos kapsayıcısı, sistem tarafından tanımlanan özellikler kümesi içerir. API seçime bağlı olarak, bunlardan bazıları doğrudan açık olabilir değil. Aşağıdaki tabloda sistem tarafından tanımlanan özellikler listesini açıklanmaktadır:

| **Sistem tarafından tanımlanan özelliği** | **Oluşturulan veya kullanıcı tarafından yapılandırılabilen sistem** | **Amacı** | **SQL API'Sİ** | **Cassandra API'si** | **Azure Cosmos DB'nin MongoDB API'si** | **Gremlin API** | **Tablo API’si** |
| --- | --- | --- | --- | --- | --- | --- | --- |
|_rid | Sistem tarafından oluşturulan | Kapsayıcı benzersiz tanıtıcısı | Evet | Hayır | Hayır | Hayır | Hayır |
|_etag | Sistem tarafından oluşturulan | İyimser eşzamanlılık denetimi için kullanılan varlık etiketi | Evet | Hayır | Hayır | Hayır | Hayır |
|_ts | Sistem tarafından oluşturulan | Kapsayıcının son güncelleştirilen zaman damgası | Evet | Hayır | Hayır | Hayır | Hayır |
|_self | Sistem tarafından oluşturulan | Kapsayıcının adreslenebilir URI'si | Evet | Hayır | Hayır | Hayır | Hayır |
|id | Kullanıcı tarafından yapılandırılabilir | Kullanıcı tanımlı kapsayıcının benzersiz adı | Evet | Evet | Evet | Evet | Evet |
|indexingPolicy | Kullanıcı tarafından yapılandırılabilir | Dizin yolu, dizin türü ve dizin modunu değiştirme olanağı sağlar. | Evet | Hayır | Hayır | Hayır | Evet |
|TimeToLive | Kullanıcı tarafından yapılandırılabilir | Öğeleri belirli bir zaman aralığına sonra otomatik olarak bir kapsayıcıdan silme olanağı sağlar. Daha fazla ayrıntı için [Time To Live](time-to-live.md) makalesi. | Evet | Hayır | Hayır | Hayır | Evet |
|changeFeedPolicy | Kullanıcı tarafından yapılandırılabilir | Bir kapsayıcı içindeki öğelerde yapılan değişiklikleri okumak için kullanılır. Daha fazla ayrıntı için [değişiklik akışı](change-feed.md) makalesi. | Evet | Hayır | Hayır | Hayır | Evet |
|uniqueKeyPolicy | Kullanıcı tarafından yapılandırılabilir | Mantıksal bölüm içindeki bir veya daha fazla değerlerin benzersiz olmasını sağlamak için kullanılır. Daha fazla bilgi için [benzersiz anahtar kısıtlamaları](unique-keys.md) makalesi. | Evet | Hayır | Hayır | Hayır | Evet |

### <a name="operations-on-an-azure-cosmos-container"></a>Bir Azure Cosmos kapsayıcısı üzerinde işlemler

Bir Azure Cosmos kapsayıcı herhangi bir Azure Cosmos API'lerini kullanarak şu işlemleri destekler.

| **İşlem** | **Azure CLI** | **SQL API'Sİ** | **Cassandra API'si** | **Azure Cosmos DB'nin MongoDB API'si** | **Gremlin API** | **Tablo API’si** |
| --- | --- | --- | --- | --- | --- | --- |
| Bir veritabanı kapsayıcılarda listeleme | Evet | Evet | Evet | Evet | NA | NA |
| Bir kapsayıcı okuyun | Evet | Evet | Evet | Evet | NA | NA |
| Yeni bir kapsayıcı oluşturma | Evet | Evet | Evet | Evet | NA | NA |
| Kapsayıcıyı güncelleştir | Evet | Evet | Evet | Evet | NA | NA |
| Kapsayıcıyı Sil | Evet | Evet | Evet | Evet | NA | NA |

## <a name="azure-cosmos-items"></a>Azure Cosmos öğeleri

API seçime bağlı olarak, bir Azure Cosmos öğesi ya da bir belge bir koleksiyonda, bir tabloda bir satır veya grafikteki bir düğümü/edge temsil edebilir. Aşağıdaki tablo, Azure Cosmos öğeye API özel varlıklar arasındaki eşlemeyi gösterir:

| **Cosmos varlık** | **SQL API'Sİ** | **Cassandra API'si** | **Azure Cosmos DB'nin MongoDB API'si** | **Gremlin API** | **Tablo API’si** |
| --- | --- | --- | --- | --- | --- |
|Azure Cosmos öğesi | Belge | Satır | Belge | Düğüm veya kenar | Öğe |

### <a name="properties-of-an-item"></a>Bir öğenin özellikleri

Her Azure Cosmos öğesi aşağıdaki sistem tanımlı özelliklerine sahiptir. API seçime bağlı olarak, bunlardan bazıları doğrudan açık olabilir değil.

|**Sistem tarafından tanımlanan özelliği** | **Oluşturulan veya kullanıcı tarafından yapılandırılabilen sistem**| **Amacı** | **SQL API'Sİ** | **Cassandra API'si** | **Azure Cosmos DB'nin MongoDB API'si** | **Gremlin API** | **Tablo API’si** |
| --- | --- | --- | --- | --- | --- | --- | --- |
|_id | Sistem tarafından oluşturulan | Öğenin benzersiz tanıtıcısı | Evet | Hayır | Hayır | Hayır | Hayır |
|_etag | Sistem tarafından oluşturulan | İyimser eşzamanlılık denetimi için kullanılan varlık etiketi | Evet | Hayır | Hayır | Hayır | Hayır |
|_ts | Sistem tarafından oluşturulan | Zaman damgası'öğesinin son güncelleştirme | Evet | Hayır | Hayır | Hayır | Hayır |
|_self | Sistem tarafından oluşturulan | Öğenin adreslenebilir URI'si | Evet | Hayır | Hayır | Hayır | Hayır |
|id | Ya da | Mantıksal bölüm içindeki kullanıcı tanımlı benzersiz adı. Kullanıcı Kimliği belirtmiyorsa, sistemin bir otomatik olarak oluşturur. | Evet | Evet | Evet | Evet | Evet |
|Kullanıcı tanımlı isteğe bağlı özellikler | Kullanıcı tanımlı | Yerel API gösterimi (JSON, BSON, CQL, vb.) temsil edilen kullanıcı tanımlı Özellikler | Evet | Evet | Evet | Evet | Evet |

### <a name="operations-on-items"></a>Öğeleri üzerinde işlemler

Azure Cosmos öğesi herhangi bir Azure Cosmos API'leri kullanılarak gerçekleştirilebilir aşağıdaki işlemleri destekler.

| **İşlem** | **Azure CLI** | **SQL API'Sİ** | **Cassandra API'si** | **Azure Cosmos DB'nin MongoDB API'si** | **Gremlin API** | **Tablo API’si** |
| --- | --- | --- | --- | --- | --- | --- |
| Ekleme, değiştirme, silme, Upsert, okuma | Hayır | Evet | Evet | Evet | Evet | Evet |

## <a name="next-steps"></a>Sonraki adımlar

Şimdi aşağıdaki kavramları öğrenmeniz geçebilirsiniz:

* [Bir Azure Cosmos veritabanı üzerinde sağlanan aktarım hızı yapılandırma](how-to-provision-database-throughput.md)
* [Sağlanan aktarım hızı bir Azure Cosmos kapsayıcısını yapılandırma](how-to-provision-container-throughput.md)
* [Mantıksal bölümleri](partition-data.md)
* [Azure Cosmos kapsayıcısını TTL yapılandırma](how-to-time-to-live.md)
* [Değişiklik akışı kullanan reaktif uygulamalar oluşturma](change-feed.md)
* [Azure Cosmos kapsayıcınızı benzersiz anahtar kısıtlaması yapılandırma](unique-keys.md)
