---
title: Bağlı hizmetler (WebJob Proje) Azure depolama ve Visual Studio ile çalışmaya başlama
description: Visual Studio kullanarak bir depolama hesabına bağlandıktan sonra Visual Studio'da Azure WebJobs projesinde Azure tablo depolama ile çalışmaya başlamak nasıl bağlı hizmetler
services: storage
author: ghogen
manager: douge
ms.assetid: 061a6c46-0592-4e5d-aced-ab7498481cde
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 12/02/2016
ms.author: ghogen
ms.openlocfilehash: a9a4475465fefb01ec53e6e0eb814f9b8f192a1b
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59799343"
---
# <a name="getting-started-with-azure-storage-azure-webjob-projects"></a>Azure kullanmaya başlama depolama (Azure WebJob Proje)
[!INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Genel Bakış
Bu makalede, Azure Web işleri SDK'sı sürümünü kullanmayı gösteren C# kod örneği sağlanmıştır 1.x ile Azure tablo depolama hizmeti. Kod örnekleri kullan [WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/wiki) sürüm 1.x.

Azure Table storage hizmeti büyük miktarlarda yapısal veriyi depolamanızı sağlar. Kimliği doğrulanmış çağrılarından içindeki ve Azure Bulutu dışındaki kabul eden bir NoSQL veri deposu hizmetidir. Azure tabloları, yapılandırılmış ve ilişkisel olmayan verilerin depolanması için idealdir.  Bkz: [.NET kullanarak Azure tablo depolama ile çalışmaya başlama](../cosmos-db/tutorial-develop-table-dotnet.md#create-a-table) daha fazla bilgi için.

Bazı kod parçacıkları Göster **tablo** el ile diğer bir deyişle, tetikleyici özniteliklerinden biri kullanılmadığında çağrılan işlevlerde kullanılan öznitelik.

## <a name="how-to-add-entities-to-a-table"></a>Bir tabloya varlıklar ekleme
Bir tabloya varlıkları eklemek için **tablo** özniteliğini bir **ICollector<T>**  veya **IAsyncCollector<T>**  parametresi burada **T** eklemek istediğiniz varlıkların şema belirtir. Öznitelik oluşturucusunda tablonun adını belirten bir dize parametresi alır.

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

Genellikle türü ile kullandığınız **ICollector** türetildiği **TableEntity** veya uygulayan **ITableEntity**, ancak bu gerekli değildir. Aşağıdakilerden birini **kişi** önceki gösterilen kod ile iş sınıfları **giriş** yöntemi.

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

Ekleyebileceğiniz Azure depolama ile doğrudan API çalışmak istiyorsanız, bir **CloudStorageAccount** parametresi metodu imzası.

## <a name="real-time-monitoring"></a>Gerçek zamanlı izleme
Veri giriş işlevleri genellikle büyük miktarda veriyi işlemek için Web işleri SDK'sı Pano gerçek zamanlı izleme verileri sağlar. **Çağrı günlüğü** bölüm bildirir işlevi hala çalışıyorsa.

![Çalışan giriş işlevi](./media/vs-storage-webjobs-getting-started-tables/ingressrunning.png)

**Çağrı ayrıntıları** Raporları Sayfası işlevin ilerleme durumu (yazılan varlıkların sayısı) çalıştıran ve onu iptal etmek için bir fırsat sağlar.

![Çalışan giriş işlevi](./media/vs-storage-webjobs-getting-started-tables/ingressprogress.png)

İşlev sona erdiğinde **çağrı ayrıntıları** Raporları Sayfası yazılan satır sayısı.

![Giriş işlevi tamamlandı](./media/vs-storage-webjobs-getting-started-tables/ingresssuccess.png)

## <a name="how-to-read-multiple-entities-from-a-table"></a>Birden çok varlık bir tablodan okuma
Bir tablo okumak için kullandığınız **tablo** özniteliğini bir **Iqueryable<T>**  parametresi girildiği **T** türetildiği **TableEntity**veya uygulayan **ITableEntity**.

Aşağıdaki kod örneği okur ve tablosundan tüm satırları günlükleri **giriş** tablosu:

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

### <a name="how-to-read-a-single-entity-from-a-table"></a>Bir tablodan tek bir varlığı okuma
Var olan bir **tablo** öznitelik oluşturucusu için tek bir tablo varlığı bağlamak istediğinizde bölüm anahtarını ve satır anahtarı belirtmenizi sağlar iki ek parametrelerle.

Aşağıdaki kod örneği için bir tablo satır okuyan bir **kişi** kuyruk iletisini bölüm anahtarını ve satır anahtarı değerlerinin temel varlık:  

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


**Kişi** uygulamak Bu örnekte sınıf yok **ITableEntity**.

## <a name="how-to-use-the-net-storage-api-directly-to-work-with-a-table"></a>Doğrudan bir tablo ile çalışmak için depolama .NET API kullanma
Ayrıca **tablo** özniteliğini bir **CloudTable** nesnesi bir tablo ile çalışmak için daha fazla esneklik için.

Aşağıdaki kod örneği kullanan bir **CloudTable** için tek bir varlık eklemek için nesne *giriş* tablo.

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

Nasıl kullanılacağı hakkında daha fazla bilgi için **CloudTable** nesne, bkz: [.NET kullanarak Azure tablo depolama ile çalışmaya başlama](../storage/storage-dotnet-how-to-use-tables.md).

## <a name="related-topics-covered-by-the-queues-how-to-article"></a>Kuyruklar ile ilgili nasıl yapılır makalesi tarafından kapsanan ilgili konular
Bir kuyruk iletisi tarafından tetiklenen tablo işlem hakkında bilgi için veya tablo işlemeye özel olmayan Web işleri SDK'sı senaryoları için bkz: [bağlı hizmetler (WebJob Proje) Azure kuyruk depolama ve Visual Studio ile çalışmaya başlama ](../storage/vs-storage-webjobs-getting-started-queues.md).

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Azure tabloları ile çalışmak için ortak senaryolar nasıl ele alınacağını gösteren kod örnekleri sağlamıştır. Azure WebJobs ve WebJobs SDK'sı kullanma hakkında daha fazla bilgi için bkz. [Azure WebJobs belgeleri kaynakları](https://go.microsoft.com/fwlink/?linkid=390226).

