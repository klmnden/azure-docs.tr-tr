---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/26/2018
ms.author: tamram
ms.openlocfilehash: 042aedf1a043cd89d74ff099554642d38a3c7dd3
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62122998"
---
## <a name="what-is-table-storage"></a>Tablo depolama nedir
Azure Tablo depolama, büyük miktarlarda yapısal veriyi depolar. Bu hizmet, Azure bulutunun içinden ve dışından gelen kimliği doğrulanmış çağrıları kabul eden bir NoSQL olmayan veri deposudur. Azure tabloları, yapılandırılmış ve ilişkisel olmayan verilerin depolanması için idealdir. Tablo depolamanın yaygın kullanımları şunlardır:

* Web ölçekli uygulamalara hizmet verebilen yapılandırılmış verilerin TB depolaması
* Karmaşık katılımların, yabancı anahtarların veya depolanan yordamların gerekmediği ve hızlı erişim için normal olmayabilen veri kümelerinin depolanması
* Kümelenmiş dizin kullanarak hızlı veri sorgulaması
* WCF Veri Hizmeti .NET Kitaplıklarıyla OData protokolü ve LINQ sorgularını kullanarak verilere erişilmesi

Yapılandırılmış ve ilişkisel olmayan verilerin büyük kümelerini depolamak ve sorgulamak için Tablo depolamayı kullanabilirsiniz; tablolara talep arttıkça da ölçekleneceklerdir.

## <a name="table-storage-concepts"></a>Tablo depolama kavramları
Tablo depolamada şu bileşenler bulunur:

![Tablo depolama bileşen diyagramı][Table1]

* **URL biçimi:** Azure tablo depolama hesapları, bu biçimi kullanın: `http://<storage account>.table.core.windows.net/<table>`

  Azure Cosmos DB Tablo API’si hesapları şu biçimi kullanır: `http://<storage account>.table.cosmosdb.azure.com/<table>`  

  Bu adres biçimini OData protokolüyle birlikte kullanarak doğrudan Azure tablolarını işaret edebilirsiniz. Daha fazla bilgi için bkz. [OData.org][OData.org].
* **Hesaplar:** Tüm Azure depolama erişimi bir depolama hesabı üzerinden yapılır. Depolama hesabı kapasitesi hakkında ayrıntılı bilgi için bkz. [Azure Storage Ölçeklenebilirlik ve Performans Hedefleri](../articles/storage/common/storage-scalability-targets.md). 

    Tüm Azure Cosmos DB erişimi bir Tablo API’si hesabı üzerinden yapılır. Tablo API’si hesabı oluşturmaya ilişkin ayrıntılar için bkz. [Tablo API’si hesabı oluşturma](../articles/cosmos-db/create-table-dotnet.md#create-a-database-account).
* **Tablo**: Bir varlık koleksiyonunu bir tablodur. Tablolar varlıklardaki şemayı zorlamaz; bu da, tek tabloda farklı özellik kümelerine sahip varlıklar olabileceği anlamına gelir.  
* **Varlık**: Bir dizi özellik, veritabanı satırına benzer bir varlıktır. Azure Depolama’daki bir varlığın boyutu en fazla 1 MB olabilir. Azure Cosmos DB’deki bir varlığın boyutu en fazla 2 MB olabilir.
* **Özellikler**: Bir özellik, bir ad-değer çiftidir. Her varlıkta verileri depolayacak en çok 252 özellik olabilir. Her varlıkta ayrıca, bölüm anahtarını, satır anahtarını ve zaman damgasını belirten üç sistem özelliği bulunur. Aynı bölüm anahtarına sahip varlıklar daha hızlı sorgulanabilir ve atomik işlemlere eklenir/güncelleştirilir. Varlığın satır anahtarı bölüm içinde kendine ait benzersiz tanımlayıcıdır.

Tabloları ve özellikleri adlandırma hakkında daha fazla bilgi için bkz. [Tablo Hizmeti Veri Modelini Anlama](/rest/api/storageservices/Understanding-the-Table-Service-Data-Model).

[Table1]: ./media/storage-table-concepts-include/table1.png
[OData.org]: http://www.odata.org/
