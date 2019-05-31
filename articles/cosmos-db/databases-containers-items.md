---
title: Veritabanları, kapsayıcıları ve Azure Cosmos DB'de öğeleri ile çalışma
description: Bu makalede, veritabanları, kapsayıcılar ve öğeleri oluşturma ve Azure Cosmos DB'de açıklar.
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: rimman
ms.reviewer: sngun
ms.openlocfilehash: 574dd9fd6189b6d0f1e5d455146d6d083ad7ff77
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66389459"
---
# <a name="work-with-databases-containers-and-items-in-azure-cosmos-db"></a>Veritabanları, kapsayıcıları ve Azure Cosmos DB'de öğeleri ile çalışma

Oluşturduktan sonra bir [Azure Cosmos DB hesabı](account-overview.md) Azure aboneliğiniz kapsamındaki verileri hesabınızdaki veritabanları, kapsayıcılar ve öğeleri oluşturarak yönetebilirsiniz. Bu makalede bu varlıkların açıklar. 

Aşağıdaki görüntüde farklı varlık hiyerarşisi, bir Azure Cosmos DB hesabını gösterir:

![Azure Cosmos hesabı varlıklar](./media/databases-containers-items/cosmos-entities.png)

## <a name="azure-cosmos-databases"></a>Azure Cosmos veritabanları

Bir veya birden çok Azure Cosmos veritabanı hesabınız kapsamında oluşturabilirsiniz. Bir veritabanı için bir ad alanı benzerdir. Bir veritabanı için Azure Cosmos kapsayıcıları bir dizi yönetim birimidir. Aşağıdaki tablo, bir Azure Cosmos veritabanı çeşitli API özel varlıklara nasıl eşleştiğini gösterir:

| Azure Cosmos varlığı | SQL API’si | Cassandra API’si | MongoDB için Azure Cosmos DB API | Gremlin API | Tablo API’si |
| --- | --- | --- | --- | --- | --- |
|Azure Cosmos veritabanı | Database | Keyspace | Database | Database | NA |

> [!NOTE]
> Tablo API'si hesapları ile ilk tablonuzun oluşturduğunuzda varsayılan veritabanı, Azure Cosmos hesabınızdaki otomatik olarak oluşturulur.

### <a name="operations-on-an-azure-cosmos-database"></a>Bir Azure Cosmos veritabanı üzerinde işlemler

Azure Cosmos API'leri ile bir Azure Cosmos veritabanı aşağıdaki tabloda açıklandığı gibi etkileşim kurabilirsiniz:

| İşlem | Azure CLI | SQL API’si | Cassandra API’si | MongoDB için Azure Cosmos DB API | Gremlin API | Tablo API’si |
| --- | --- | --- | --- | --- | --- | --- |
|Tüm veritabanları listeleme| Evet | Evet | Evet (veritabanı için bir anahtar alanı eşlendi) | Evet | NA | NA |
|Okuma veritabanı| Evet | Evet | Evet (veritabanı için bir anahtar alanı eşlendi) | Evet | NA | NA |
|Yeni veritabanı oluştur| Evet | Evet | Evet (veritabanı için bir anahtar alanı eşlendi) | Evet | NA | NA |
|Veritabanını Güncelleştir| Evet | Evet | Evet (veritabanı için bir anahtar alanı eşlendi) | Evet | NA | NA |


## <a name="azure-cosmos-containers"></a>Azure Cosmos kapsayıcıları

Bir Azure Cosmos kapsayıcı ölçeklenebilirlik hem sağlanan aktarım hızı ve depolama birimidir. Bir kapsayıcı yatay olarak bölümlenir ve ardından birden çok bölgeye çoğaltılır. Kapsayıcı ve aktarım hızı üzerinde sağlamak için eklediğiniz öğeleri otomatik olarak bir dizi bölüm anahtarına göre mantıksal bölümler arasında dağıtılır. Bölümlendirme ve bölüm anahtarları hakkında daha fazla bilgi için bkz: [verileri bölümleme](partition-data.md). 

Bir Azure Cosmos kapsayıcısı oluşturduğunuzda, aktarım hızı şu modlardan birini yapılandırın:

* **Adanmış sağlanan aktarım hızı modu**: Bir kapsayıcı sağlanan aktarım hızı bu kapsayıcı için özel olarak ayrılmış ve SLA'lar ile desteklenir. Daha fazla bilgi için bkz. [nasıl sağlanacağı bir Azure Cosmos kapsayıcısında aktarım hızını](how-to-provision-container-throughput.md).

* **Paylaşılan sağlanan aktarım hızı modu**: Bu kapsayıcıların sağlanan aktarım hızı (adanmış sağlanan aktarım hızı ile yapılandırılmış kapsayıcılar dışında) aynı veritabanında diğer kapsayıcılarla paylaşın. Diğer bir deyişle, veritabanı sağlanan aktarım hızını tüm "paylaşılan aktarım hızı" kapsayıcılar arasında paylaşılır. Daha fazla bilgi için bkz. [nasıl sağlanacağı bir Azure Cosmos veritabanı aktarım hızını](how-to-provision-database-throughput.md).

> [!NOTE]
> Paylaşılan ve ayrılmış üretilen iş yalnızca veritabanı ve kapsayıcı oluştururken yapılandırabilirsiniz. Kapsayıcı oluşturulduktan sonra adanmış aktarım hızı modundan paylaşılan aktarım modu (ve tersi) geçmek için yeni bir kapsayıcı oluşturmak ve yeni kapsayıcıya verileri geçirmeniz gerekir. Azure Cosmos DB değişiklik akışı özelliğini kullanarak verileri geçirebilirsiniz.

Adanmış veya paylaşılan sağlanan aktarım hızı modlarını kullanarak kapsayıcıları oluşturma olup olmadığını bir Azure Cosmos kapsayıcısı, ölçeklendirebilir.

Bir Azure Cosmos kapsayıcı öğeleri şemadan kapsayıcıdır. Öğeleri bir kapsayıcıda rastgele şemalar bulunabilir. Örneğin, bir kişiyi temsil eder bir öğe ve bir otomobili temsil eder bir öğe yerleştirilebilir *aynı kapsayıcı*. Varsayılan olarak, bir kapsayıcıya eklediğiniz tüm öğeleri açık dizin veya şema yönetimi gerektirmeden otomatik olarak dizinlenir. Dizin oluşturma davranışı yapılandırarak özelleştirebileceğiniz [dizin oluşturma ilkesi](index-overview.md) üzerinde bir kapsayıcı. 

Ayarlayabileceğiniz [yaşam süresi (TTL)](time-to-live.md) seçilen öğelerde bir Azure Cosmos kapsayıcı ya da sistemden öğelerin düzgün bir şekilde temizlemeye kapsayıcının tamamı. Azure Cosmos DB, otomatik olarak bu süre dolduğunda öğeleri siler. Ayrıca, bir sorgu kapsayıcısı üzerinde gerçekleştirilen sabit bir sınır içinde süresi dolan öğeleri döndürmüyor garanti eder. Daha fazla bilgi için bkz. [kapsayıcınızı TTL yapılandırma](how-to-time-to-live.md).

Kullanabileceğiniz [değişiklik akışını](change-feed.md) kapsayıcınızı mantıksal her bölüm için yönetilen işlem günlüğüne abone olmak için. Değişiklik akışı ile birlikte kapsayıcısı üzerinde gerçekleştirilen tüm güncelleştirmelerin günlük sağlar önce ve sonra görüntülerini öğeleri. Daha fazla bilgi için [kullanarak reaktif uygulamalar oluşturun, değişiklik akışı](serverless-computing-database.md). Ayrıca, değişiklik akışı kapsayıcı üzerindeki ilke değişikliği kullanarak akışı için bekletme süresini yapılandırabilirsiniz. 

Kaydedebileceğiniz [saklı yordamlar, Tetikleyiciler, kullanıcı tanımlı işlevler (UDF'ler)](stored-procedures-triggers-udfs.md), ve [birleştirme yordamları](how-to-manage-conflicts.md) Azure Cosmos kapsayıcınız için. 

Belirtebileceğiniz bir [benzersiz anahtar kısıtlaması](unique-keys.md) Azure Cosmos kapsayıcınızı üzerinde. Benzersiz anahtar bir ilke oluşturarak, mantıksal bölüm anahtarı başına bir veya daha fazla değerlerin benzersiz olmasını sağlamak. Bir benzersiz anahtar İlkesi'ni kullanarak bir kapsayıcı oluşturursanız, hiçbir yeni veya güncelleştirilmiş öğeleri benzersiz anahtar kısıtlaması tarafından belirtilen değerleri yinelenen değerleri ile oluşturulabilir. Daha fazla bilgi için bkz. [benzersiz anahtar kısıtlamaları](unique-keys.md).

Aşağıdaki tabloda gösterildiği gibi bir Azure Cosmos kapsayıcı API özel varlıklara özelleştirilmiştir:

| Azure Cosmos varlığı | SQL API’si | Cassandra API’si | MongoDB için Azure Cosmos DB API | Gremlin API | Tablo API’si |
| --- | --- | --- | --- | --- | --- |
|Azure Cosmos kapsayıcı | Koleksiyon | Tablo | Koleksiyon | Graf | Tablo |

### <a name="properties-of-an-azure-cosmos-container"></a>Bir Azure Cosmos kapsayıcı özellikleri

Bir Azure Cosmos kapsayıcısı, sistem tarafından tanımlanan özellikler kümesi içerir. Kullandığınız API bağlı olarak, bazı özellikleri doğrudan gösterilmeyen. Aşağıdaki tabloda sistem tarafından tanımlanan özellikler listesini açıklanmaktadır:

| Sistem tarafından tanımlanan bir özellik | Sistem tarafından oluşturulan veya kullanıcı tarafından yapılandırılabilir | Amaç | SQL API’si | Cassandra API’si | MongoDB için Azure Cosmos DB API | Gremlin API | Tablo API’si |
| --- | --- | --- | --- | --- | --- | --- | --- |
|\_Kimliği | Sistem tarafından oluşturulan | Kapsayıcı benzersiz tanıtıcısı | Evet | Hayır | Hayır | Hayır | Hayır |
|\_ETag | Sistem tarafından oluşturulan | İyimser eşzamanlılık denetimi için kullanılan varlık etiketi | Evet | Hayır | Hayır | Hayır | Hayır |
|\_TS | Sistem tarafından oluşturulan | Kapsayıcının son güncelleştirilen zaman damgası | Evet | Hayır | Hayır | Hayır | Hayır |
|\_Kendi kendine | Sistem tarafından oluşturulan | Kapsayıcının adreslenebilir URI'si | Evet | Hayır | Hayır | Hayır | Hayır |
|id | Kullanıcı tarafından yapılandırılabilir | Kullanıcı tanımlı kapsayıcının benzersiz adı | Evet | Evet | Evet | Evet | Evet |
|indexingPolicy | Kullanıcı tarafından yapılandırılabilir | Dizin yolu, dizin türü ve dizin modunu değiştirme olanağı sağlar | Evet | Hayır | Hayır | Hayır | Evet |
|timeToLive | Kullanıcı tarafından yapılandırılabilir | Öğeleri ayarlanmış bir süre sonra otomatik olarak bir kapsayıcıdan silme olanağı sağlar. Ayrıntılar için bkz [yaşam süresi](time-to-live.md). | Evet | Hayır | Hayır | Hayır | Evet |
|changeFeedPolicy | Kullanıcı tarafından yapılandırılabilir | Bir kapsayıcı içindeki öğelerde yapılan değişiklikleri okumak için kullanılır. Ayrıntılar için bkz [değişiklik akışı](change-feed.md). | Evet | Hayır | Hayır | Hayır | Evet |
|uniqueKeyPolicy | Kullanıcı tarafından yapılandırılabilir | Mantıksal bir bölümü bir veya daha fazla değerlerin benzersiz olmasını sağlamak için kullanılır. Daha fazla bilgi için [benzersiz anahtar kısıtlamaları](unique-keys.md). | Evet | Hayır | Hayır | Hayır | Evet |

### <a name="operations-on-an-azure-cosmos-container"></a>Bir Azure Cosmos kapsayıcısı üzerinde işlemler

Azure Cosmos API'lerinin kullandığınızda bir Azure Cosmos kapsayıcı aşağıdaki işlemleri destekler:

| İşlem | Azure CLI | SQL API’si | Cassandra API’si | MongoDB için Azure Cosmos DB API | Gremlin API | Tablo API’si |
| --- | --- | --- | --- | --- | --- | --- |
| Bir veritabanı kapsayıcılarda listeleme | Evet | Evet | Evet | Evet | NA | NA |
| Bir kapsayıcı okuyun | Evet | Evet | Evet | Evet | NA | NA |
| Yeni bir kapsayıcı oluşturma | Evet | Evet | Evet | Evet | NA | NA |
| Bir kapsayıcıyı güncelleştir | Evet | Evet | Evet | Evet | NA | NA |
| Kapsayıcı silme | Evet | Evet | Evet | Evet | NA | NA |

## <a name="azure-cosmos-items"></a>Azure Cosmos öğeleri

Kullandığınız API bağlı olarak, bir Azure Cosmos öğesi belgeye bir koleksiyondaki bir satır, tablo veya bir düğüm veya kenar grafikteki temsil edebilir. Aşağıdaki tablo, bir Azure Cosmos öğe API özel varlıklara eşleme gösterir:

| Cosmos varlık | SQL API’si | Cassandra API’si | MongoDB için Azure Cosmos DB API | Gremlin API | Tablo API’si |
| --- | --- | --- | --- | --- | --- |
|Azure Cosmos öğesi | Belge | Satır | Belge | Düğüm veya kenar | Öğe |

### <a name="properties-of-an-item"></a>Bir öğenin özellikleri

Her Azure Cosmos öğesi aşağıdaki sistem tanımlı özelliklerine sahiptir. Kullandığınız API bağlı olarak, bunlardan bazıları doğrudan gösterilmeyen.

| Sistem tarafından tanımlanan bir özellik | Sistem tarafından oluşturulan veya kullanıcı tarafından yapılandırılabilir| Amaç | SQL API’si | Cassandra API’si | MongoDB için Azure Cosmos DB API | Gremlin API | Tablo API’si |
| --- | --- | --- | --- | --- | --- | --- | --- |
|\_Kimliği | Sistem tarafından oluşturulan | Öğenin benzersiz tanıtıcısı | Evet | Hayır | Hayır | Hayır | Hayır |
|\_ETag | Sistem tarafından oluşturulan | İyimser eşzamanlılık denetimi için kullanılan varlık etiketi | Evet | Hayır | Hayır | Hayır | Hayır |
|\_TS | Sistem tarafından oluşturulan | Zaman damgası öğesinin son güncelleştirme | Evet | Hayır | Hayır | Hayır | Hayır |
|\_Kendi kendine | Sistem tarafından oluşturulan | Öğenin adreslenebilir URI'si | Evet | Hayır | Hayır | Hayır | Hayır |
|id | Ya da | Mantıksal bir bölüme kullanıcı tanımlı benzersiz adı. Kullanıcı Kimliği belirtmiyorsa, sistem otomatik olarak bir oluşturur. | Evet | Evet | Evet | Evet | Evet |
|Kullanıcı tanımlı isteğe bağlı özellikler | Kullanıcı tanımlı | Kullanıcı tanımlı özellikler (JSON, BSON ve CQL dahil) API yerel gösteriminde temsil | Evet | Evet | Evet | Evet | Evet |

### <a name="operations-on-items"></a>Öğeleri üzerinde işlemler

Azure Cosmos öğeyi aşağıdaki işlemleri destekler. Herhangi bir Azure Cosmos API'leri işlemleri gerçekleştirmek için kullanabilirsiniz.

| İşlem | Azure CLI | SQL API’si | Cassandra API’si | MongoDB için Azure Cosmos DB API | Gremlin API | Tablo API’si |
| --- | --- | --- | --- | --- | --- | --- |
| Ekleme, değiştirme, silme, Upsert, okuma | Hayır | Evet | Evet | Evet | Evet | Evet |

## <a name="next-steps"></a>Sonraki adımlar

Bu görevler ve kavramları hakkında bilgi edinin:

* [Bir Azure Cosmos veritabanı aktarım hızını sağlama](how-to-provision-database-throughput.md)
* [Bir Azure Cosmos kapsayıcısında aktarım hızını sağlama](how-to-provision-container-throughput.md)
* [Mantıksal bölümleri ile çalışma](partition-data.md)
* [Bir Azure Cosmos kapsayıcısını TTL yapılandırın](how-to-time-to-live.md)
* [Değişiklik akışı kullanarak reaktif uygulamalar oluşturun](change-feed.md)
* [Azure Cosmos kapsayıcınızı benzersiz bir anahtar kısıtlaması yapılandırın](unique-keys.md)
