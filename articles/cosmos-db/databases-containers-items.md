---
title: Azure Cosmos DB veritabanları, kapsayıcıları ve öğeleri ile çalışma
description: Bu makalede, Azure Cosmos DB veritabanları, kapsayıcıları ve öğeleri oluşturulacağı ve kullanılacağı açıklanır
author: dharmas
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/08/2018
ms.author: dharmas
ms.reviewer: sngun
ms.openlocfilehash: 6757f887376e1b399d6af18f114e203991c16a67
ms.sourcegitcommit: 9f87a992c77bf8e3927486f8d7d1ca46aa13e849
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/28/2018
ms.locfileid: "53807695"
---
# <a name="working-with-azure-cosmos-databases-containers-and-items"></a>Azure Cosmos veritabanı, kapsayıcıları ve öğeleri ile çalışma

Oluşturduktan sonra bir [Azure Cosmos DB hesabı](account-overview.md) Azure aboneliğiniz kapsamındaki verileri hesabınızdaki veritabanları, kapsayıcılar ve öğeleri oluşturarak yönetebilirsiniz. Bu makalede bu varlıkların açıklar: veritabanları, kapsayıcılar ve öğeleri. Aşağıdaki resimde, bir Azure Cosmos hesabında farklı varlık hiyerarşisi gösterilmektedir:

![Azure Cosmos hesabı varlıklar](./media/databases-containers-items/cosmos-entities.png)

## <a name="azure-cosmos-databases"></a>Azure Cosmos veritabanları

Bir veya daha fazla Azure Cosmos veritabanı hesabınız kapsamında oluşturabilirsiniz. Bir veritabanı için bir ad alanı benzerdir, Azure Cosmos kapsayıcıları kümesi için yönetim birimidir. Aşağıdaki tablo, bir Azure Cosmos veritabanı çeşitli API özel varlıklara nasıl eşleştiğini gösterir:

| **Azure Cosmos varlığı** | **SQL API'Sİ** | **Cassandra API'si** | **Azure Cosmos DB'nin MongoDB API'si** | **Gremlin API** | **Tablo API’si** |
| --- | --- | --- | --- | --- | --- |
|Azure Cosmos veritabanı | Database | Keyspace | Database | Database | NA |

> [!NOTE]
> Tablo API hesaplarıyla, varsayılan veritabanı tablonuzun ilk oluşturduğunuzda, Azure Cosmos hesabınızdaki otomatik olarak oluşturulur.

### <a name="operations-on-an-azure-cosmos-database"></a>Bir Azure Cosmos veritabanı üzerinde işlemler

Aşağıdaki Azure Cosmos API'lerini kullanarak bir Azure Cosmos veritabanıyla etkileşim kurabilirsiniz:

| **İşlem** | **Azure CLI**|**SQL API'Sİ** | **Cassandra API'si** | **Azure Cosmos DB'nin MongoDB API'si** | **Gremlin API** | **Tablo API’si** |
| --- | --- | --- | --- | --- | --- | --- |
|Tüm veritabanları listeleme| Evet | Evet | Evet (veritabanı için bir anahtar alanı eşlendi) | Evet | NA | NA |
|Okuma veritabanı| Evet | Evet | Evet (veritabanı için bir anahtar alanı eşlendi) | Evet | NA | NA |
|Yeni veritabanı oluştur| Evet | Evet | Evet (veritabanı için bir anahtar alanı eşlendi) | Evet | NA | NA |
|Veritabanını Güncelleştir| Evet | Evet | Evet (veritabanı için bir anahtar alanı eşlendi) | Evet | NA | NA |


## <a name="azure-cosmos-containers"></a>Azure Cosmos kapsayıcıları

Bir Azure Cosmos ölçeklenebilirlik için hem sağlanan aktarım hızı birimi ve depolama öğelerinin kapsayıcıdır. Bir kapsayıcı yatay olarak bölümlenir ve ardından birden çok bölgeye çoğaltılır. Sağlama üzerindeki olduğunu hem de otomatik olarak kapsayıcı ve aktarım hızı ekleme öğeleri mantıksal bölümler bölüm anahtarına göre bir dizi dağıtılmış. Bölümlendirme ve bölüm anahtarı hakkında daha fazla bilgi için bkz: [mantıksal bölümler](partition-data.md) makalesi. 

Bir Azure Cosmos kapsayıcı oluştururken, aktarım hızı şu modlardan birini yapılandırın:

* **Adanmış sağlanan aktarım hızı** modu: Bir kapsayıcı sağlanan aktarım hızı için özel olarak ayrılır ve SLA'lar ile desteklenir. Daha fazla bilgi için bkz. [nasıl sağlanacağı bir Azure Cosmos kapsayıcısında aktarım hızını](how-to-provision-container-throughput.md).

* **Paylaşılan sağlanan aktarım hızı** modu: Bu kapsayıcıların sağlanan aktarım hızı (adanmış sağlanan aktarım hızı ile yapılandırılmış kapsayıcıların dışında) aynı veritabanında diğer kapsayıcılar ile paylaşın. Diğer bir deyişle, veritabanı sağlanan aktarım hızını "paylaşılan" tüm kapsayıcılar arasında paylaşılır. Daha fazla bilgi için bkz. [nasıl bir Azure Cosmos veritabanı'nda sağlanan aktarım hızı yapılandırma](how-to-provision-database-throughput.md).

Bir Azure Cosmos kapsayıcı ölçeklendirebilir, aktarım hızı modları, kapsayıcılar ile ya da "paylaşılan" veya "ayrılmış" oluşturduğunuz sağlanan.

Bir Azure Cosmos kapsayıcı öğeleri şemadan kapsayıcıdır. Bir kapsayıcı içindeki öğeleri rastgele şemalar bulunabilir. Örneğin, bir kişiyi temsil eden bir öğe, bir otomobilin temsil eden bir öğe aynı kapsayıcıda yerleştirilebilir. Varsayılan olarak, herhangi bir açık dizin veya şema yönetimi gerektirmeden bir kapsayıcıya eklediğiniz tüm öğeleri bir otomatik olarak dizine. Bir kapsayıcı dizin oluşturma ilkesini yapılandırarak, dizin oluşturma davranışını özelleştirebilirsiniz. 

Seçilen öğelerde bir Azure Cosmos kapsayıcısındaki veya sistem dışı öğelerin düzgün bir şekilde temizlemek tüm kapsayıcı yaşam süresi (TTL) ayarlayabilirsiniz. Azure Cosmos DB, otomatik olarak bu süre dolduğunda öğeleri siler. Ayrıca, bir sorgu kapsayıcısı üzerinde gerçekleştirilen sabit bir sınır içinde süresi dolan öğeleri döndürmüyor garanti eder. Daha fazla bilgi için bkz. [TTL kapsayıcınızın yapılandırma](how-to-time-to-live.md).

Değişiklik akışı kullanarak, her biri mantıksal bölüm kapsayıcınızın için yönetilen işlem günlüğüne abone olabilirsiniz. Değişiklik akışı ile birlikte kapsayıcısı üzerinde gerçekleştirilen tüm güncelleştirmelerin günlük sağlar önce ve sonra öğeleri görüntüler. Bkz: [değişiklik kullanarak reaktif uygulamalar oluşturmak nasıl akış](change-feed.md). Bekletme süresi boyunca değişiklik akışı değişiklik kapsayıcı üzerindeki ilke akışı kullanarak da yapılandırabilirsiniz. 

Azure Cosmos kapsayıcınızı saklı yordamlar, Tetikleyiciler, kullanıcı tanımlı işlevler (UDF'ler) ve birleştirme yordamları kaydedebilirsiniz. 

Azure Cosmos kapsayıcınızın benzersiz bir anahtar belirtebilirsiniz. Benzersiz anahtar bir ilke oluşturarak, mantıksal bölüm anahtarı başına bir veya daha fazla değerlerin benzersiz olmasını sağlamak. Bir kapsayıcı benzersiz bir anahtar ilke oluşturulduktan sonra tüm yeni veya güncelleştirilmiş öğeleri benzersiz anahtar kısıtlaması tarafından belirtilen değerleri yinelenen değerlere sahip oluşturulmasını engeller. Daha fazla bilgi için bkz. [benzersiz anahtar kısıtlamaları](unique-keys.md).

Bir Azure Cosmos kapsayıcı API özel varlıklara gibi özelleştirilmiş:

| **Azure Cosmos varlığı** | **SQL API'Sİ** | **Cassandra API'si** | **Azure Cosmos DB'nin MongoDB API'si** | **Gremlin API** | **Tablo API’si** |
| --- | --- | --- | --- | --- | --- |
|Azure Cosmos kapsayıcı | Koleksiyon | Tablo | Koleksiyon | Graf | Tablo |

### <a name="properties-of-an-azure-cosmos-container"></a>Bir Azure Cosmos kapsayıcı özellikleri

Bir Azure Cosmos kapsayıcısı, sistem tarafından tanımlanan özellikler kümesi içerir. API seçime bağlı olarak, bunlardan bazıları doğrudan açık olabilir değil. Aşağıdaki tabloda, desteklenen sistem tarafından tanımlanan özellikler listesini açıklanmaktadır:

| **Sistem tarafından tanımlanan özelliği** | **Sistem oluşturulan veya kullanıcı ayarlanabilir** | **Amacı** | **SQL API'Sİ** | **Cassandra API'si** | **Azure Cosmos DB'nin MongoDB API'si** | **Gremlin API** | **Tablo API’si** |
| --- | --- | --- | --- | --- | --- | --- | --- |
|__rid | Sistem tarafından oluşturulan | Kapsayıcı benzersiz tanıtıcısı | Evet | Hayır | Hayır | Hayır | Hayır |
|__etag | Sistem tarafından oluşturulan | İyimser eşzamanlılık denetimi için kullanılan varlık etiketi | Evet | Hayır | Hayır | Hayır | Hayır |
|__ts | Sistem tarafından oluşturulan | Kapsayıcının son güncelleştirilen zaman damgası | Evet | Hayır | Hayır | Hayır | Hayır |
|__self | Sistem tarafından oluşturulan | Kapsayıcının adreslenebilir URI'si | Evet | Hayır | Hayır | Hayır | Hayır |
|id | Kullanıcı tarafından yapılandırılabilir | Kullanıcı tanımlı kapsayıcının benzersiz adı | Evet | Evet | Evet | Evet | Evet |
|indexingPolicy | Kullanıcı tarafından yapılandırılabilir | Dizin yolu, kendi duyarlık ve bir tutarlılık modelini değiştirme olanağı sağlar. | Evet | Hayır | Hayır | Hayır | Evet |
|TimeToLive | Kullanıcı tarafından yapılandırılabilir | Öğeleri belirli bir zaman aralığına sonra otomatik olarak bir kapsayıcıdan silme olanağı sağlar. Daha fazla ayrıntı için [Time To Live](time-to-live.md) makalesi. | Evet | Hayır | Hayır | Hayır | Evet |
|changeFeedPolicy | Kullanıcı tarafından yapılandırılabilir | Bir kapsayıcı içindeki öğelerde yapılan değişiklikleri okumak için kullanılır. Daha fazla ayrıntı için [değişiklik akışını](change-feed.md) makalesi. | Evet | Hayır | Hayır | Hayır | Evet |
|uniqueKeyPolicy | Kullanıcı tarafından yapılandırılabilir | Benzersiz anahtarlara sahip mantıksal bölüm içindeki bir veya daha fazla değer benzersizliğini emin olun. Daha fazla bilgi için [benzersiz anahtarlar](unique-keys.md) makalesi. | Evet | Hayır | Hayır | Hayır | Evet |

### <a name="operations-on-an-azure-cosmos-container"></a>Bir Azure Cosmos kapsayıcısı üzerinde işlemler

Bir Azure Cosmos kapsayıcı herhangi bir Azure Cosmos API'lerini kullanarak şu işlemleri destekler.

| **İşlem** | **Azure CLI** | **SQL API'Sİ** | **Cassandra API'si** | **Azure Cosmos DB'nin MongoDB API'si** | **Gremlin API** | **Tablo API’si** |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Bir veritabanı kapsayıcılarda listeleme | Evet* | Evet | Evet | Evet | NA | NA |
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

|**Sistem tarafından tanımlanan özelliği** | **Sistem oluşturulan veya kullanıcı ayarlanabilir**| **Amacı** | **SQL API'Sİ** | **Cassandra API'si** | **Azure Cosmos DB'nin MongoDB API'si** | **Gremlin API** | **Tablo API’si** |
| --- | --- | --- | --- | --- | --- | --- | --- |
|__id | Sistem tarafından oluşturulan | Öğenin benzersiz tanıtıcısı | Evet | Hayır | Hayır | Hayır | Hayır |
|__etag | Sistem tarafından oluşturulan | İyimser eşzamanlılık denetimi için kullanılan varlık etiketi | Evet | Hayır | Hayır | Hayır | Hayır |
|__ts | Sistem tarafından oluşturulan | Öğesinin son güncelleştirilen zaman damgası | Evet | Hayır | Hayır | Hayır | Hayır |
|__self | Sistem tarafından oluşturulan | Öğenin adreslenebilir URI'si | Evet | Hayır | Hayır | Hayır | Hayır |
|id | Ya da | Mantıksal bölüm içindeki kullanıcı tanımlı benzersiz adı. Kullanıcı Kimliği belirtmiyorsa, sistemin bir otomatik olarak oluşturur. | Evet | Evet | Evet | Evet | Evet |
|Kullanıcı tanımlı isteğe bağlı özellikler | Kullanıcı tanımlı | Yerel API gösterimi (JSON, BSON, CQL, vb.) temsil edilen kullanıcı tanımlı Özellikler | Evet | Evet | Evet | Evet | Evet |

### <a name="operations-on-items"></a>Öğeleri üzerinde işlemler

Azure Cosmos öğesi herhangi bir Azure Cosmos API'leri kullanılarak gerçekleştirilebilir aşağıdaki işlemleri destekler.

| **İşlem** | **Azure CLI** | **SQL API'Sİ** | **Cassandra API'si** | **Azure Cosmos DB'nin MongoDB API'si** | **Gremlin API** | **Tablo API’si** |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Ekleme, değiştirme, silme, Upsert, okuma | Hayır | Evet | Evet | Evet | Evet | Evet |

## <a name="next-steps"></a>Sonraki adımlar

Şimdi Azure Cosmos hesabındaki aktarım hızına veya diğer kavramlar hakkında bilgi edinmek için geçebilirsiniz:

* [Bir Azure Cosmos veritabanı üzerinde sağlanan aktarım hızı yapılandırma](how-to-provision-database-throughput.md)
* [Sağlanan aktarım hızı bir Azure Cosmos kapsayıcı yapılandırma](how-to-provision-container-throughput.md)
* [Mantıksal bölümleri](partition-data.md)
* [Azure Cosmos kapsayıcısını TTL yapılandırma](how-to-time-to-live.md)
* [Değişiklik akışı kullanan reaktif uygulamalar oluşturma](change-feed.md)
* [Azure Cosmos kapsayıcınızı benzersiz anahtar kısıtlaması yapılandırma](unique-keys.md)
