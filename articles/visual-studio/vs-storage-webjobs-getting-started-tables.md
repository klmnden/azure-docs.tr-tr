---
title: "Azure depolama ve Visual Studio ile çalışmaya başlama bağlı Hizmetleri (Web işi projeler)"
description: "Visual Studio kullanarak bir depolama hesabı bağlandıktan sonra Visual Studio'da Azure Web işleri projesinde Azure tablo depolaması ile çalışmaya başlamak nasıl bağlı Hizmetleri"
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: 061a6c46-0592-4e5d-aced-ab7498481cde
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: kraigb
ms.openlocfilehash: 4e0c77e08bff971277a09d6066f259db84617616
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="getting-started-with-azure-storage-azure-webjob-projects"></a>Azure ile çalışmaya başlama depolama (Azure Web işi projeler)
[!INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Genel Bakış
Bu makalede gösteren C# kod örnekleri Göster Azure WebJobs SDK sürümü kullanmak nasıl sağlar 1.x Azure tablo depolama hizmeti ile. Kod örnekleri kullan [WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/wiki) sürüm 1.x.

Azure Table depolama birimi hizmeti, büyük miktarlarda yapılandırılmış verileri depolamak sağlar. Kimliği doğrulanmış çağrılarından içinden ve dışından Azure bulut kabul eden bir NoSQL veri deposu hizmetidir. Azure tabloları, yapılandırılmış ve ilişkisel olmayan verilerin depolanması için idealdir.  Bkz: [.NET kullanarak Azure Table storage'ı kullanmaya başlama](../cosmos-db/table-storage-how-to-use-dotnet.md#create-a-table) daha fazla bilgi için.

Kod parçacıkları Göster bazıları **tablo** el ile diğer bir deyişle, tetikleyici özniteliklerinden biri kullanılarak değil çağrılır işlevlerde kullanılan öznitelik.

## <a name="how-to-add-entities-to-a-table"></a>Bir tabloya varlıklar ekleme
Bir tabloya varlıkları eklemek için kullanın **tablo** ile öznitelik bir **ICollector<T>**  veya **IAsyncCollector<T>**  parametresi nerede **T** eklemek istediğiniz varlıklar şeması belirtir. Öznitelik oluşturucunun tablonun adını belirten bir dize parametresi alan.

Aşağıdaki kod örneği ekler **kişi** adlı bir tablo varlıklara *giriş*.

        [NoAutomaticTrigger]
        public static void IngressDemo(
            [Table("Ingress")] ICollector<Person> tableBinding)
        {
            for (int i = 0; i < 100000; i++)
            {
                tableBinding.Add(
                    new Person() {
                        PartitionKey = "Test",
                        RowKey = i.ToString(),
                        Name = "Name" }
                    );
            }
        }

Genellikle türü ile kullandığınız **ICollector** türetilen **TableEntity** veya uygulayan **ITableEntity**, ancak gerekli değildir. Aşağıdakilerden birini **kişi** önceki gösterilen kodu ile çalışma sınıfları **giriş** yöntemi.

        public class Person : TableEntity
        {
            public string Name { get; set; }
        }

        public class Person
        {
            public string PartitionKey { get; set; }
            public string RowKey { get; set; }
            public string Name { get; set; }
        }

Ekleyebileceğiniz doğrudan Azure storage ile API çalışmak isterseniz, bir **CloudStorageAccount** yöntem imzası parametresi.

## <a name="real-time-monitoring"></a>Gerçek zamanlı izleme
Veri giriş işlevleri genellikle büyük miktarda veriyi işlemek için Web işleri SDK'si Pano gerçek zamanlı izleme verileri sağlar. **Çağırma günlüğü** bölüm bildirir işlevi halen çalışmakta olup olmadığını.

![Çalışan giriş işlevi](./media/vs-storage-webjobs-getting-started-tables/ingressrunning.png)

**Çağırma ayrıntıları** Raporları Sayfası işlevin ilerleme durumu (yazılmış varlıkların sayısı) çalıştıran ve onu abort fırsatı sunar.

![Çalışan giriş işlevi](./media/vs-storage-webjobs-getting-started-tables/ingressprogress.png)

İşlev sona erdiğinde, **çağırma ayrıntıları** Raporları Sayfası yazılmış satırların sayısı.

![Tamamlanmış Giriş işlevi](./media/vs-storage-webjobs-getting-started-tables/ingresssuccess.png)

## <a name="how-to-read-multiple-entities-from-a-table"></a>Birden çok varlık bir tablodan okumak nasıl
Bir tablo okumak için kullandığınız **tablo** ile öznitelik bir **Iqueryable<T>**  parametresi girildiği **T** türetilen **TableEntity** veya uygulayan **ITableEntity**.

Aşağıdaki kod örneği okur ve tüm satırları günlüklerini **giriş** tablosu:

        public static void ReadTable(
            [Table("Ingress")] IQueryable<Person> tableBinding,
            TextWriter logger)
        {
            var query = from p in tableBinding select p;
            foreach (Person person in query)
            {
                logger.WriteLine("PK:{0}, RK:{1}, Name:{2}",
                    person.PartitionKey, person.RowKey, person.Name);
            }
        }

### <a name="how-to-read-a-single-entity-from-a-table"></a>Tek bir varlık tablodan okumak nasıl
Var olan bir **tablo** öznitelik oluşturucunun tek tablo varlığa bağlamak istediğinizde bölüm anahtarını ve satır anahtarını belirtmenize olanak sağlayan iki ek parametrelere sahip.

Aşağıdaki kod örneği için bir tablo satır okuyan bir **kişi** bir kuyruk iletisi alınan bölüm anahtarını ve satır anahtarı değerleri temel varlık:  

        public static void ReadTableEntity(
            [QueueTrigger("inputqueue")] Person personInQueue,
            [Table("persontable","{PartitionKey}", "{RowKey}")] Person personInTable,
            TextWriter logger)
        {
            if (personInTable == null)
            {
                logger.WriteLine("Person not found: PK:{0}, RK:{1}",
                        personInQueue.PartitionKey, personInQueue.RowKey);
            }
            else
            {
                logger.WriteLine("Person found: PK:{0}, RK:{1}, Name:{2}",
                        personInTable.PartitionKey, personInTable.RowKey, personInTable.Name);
            }
        }


**Kişi** uygulamak Bu örnekte sınıfı yok **ITableEntity**.

## <a name="how-to-use-the-net-storage-api-directly-to-work-with-a-table"></a>Bir tablo ile doğrudan çalışmak için .NET depolama API kullanma
Aynı zamanda **tablo** ile öznitelik bir **CloudTable** nesne bir tablo ile çalışma daha fazla esneklik için.

Aşağıdaki kod örneği kullanan bir **CloudTable** tek bir varlık eklemek için nesne *giriş* tablo.

        public static void UseStorageAPI(
            [Table("Ingress")] CloudTable tableBinding,
            TextWriter logger)
        {
            var person = new Person()
                {
                    PartitionKey = "Test",
                    RowKey = "100",
                    Name = "Name"
                };
            TableOperation insertOperation = TableOperation.Insert(person);
            tableBinding.Execute(insertOperation);
        }

Nasıl kullanılacağı hakkında daha fazla bilgi için **CloudTable** nesne için bkz: [.NET kullanarak Azure Table storage'ı kullanmaya başlama](../storage/storage-dotnet-how-to-use-tables.md).

## <a name="related-topics-covered-by-the-queues-how-to-article"></a>Kuyruklar nasıl yapılır makalesi tarafından kapsanan ilgili konular
Bir kuyruk iletisi tarafından tetiklenen tablo işleme nasıl ele alınacağını hakkında bilgi için veya tablo işlemeye özel olmayan Web işleri SDK'si senaryoları için bkz: [bağlı Hizmetleri (Web işi projeleri) Azure kuyruk depolama ve Visual Studio ile çalışmaya başlama](../storage/vs-storage-webjobs-getting-started-queues.md).

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede Azure tabloları ile çalışmak için genel senaryolar nasıl ele alınacağını gösteren kod örnekleri sağlamıştır. Azure Web işleri ve WebJobs SDK nasıl kullanılacağı hakkında daha fazla bilgi için bkz: [Azure Web işleri belge kaynakları](http://go.microsoft.com/fwlink/?linkid=390226).

