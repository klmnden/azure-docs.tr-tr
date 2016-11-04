---
title: Service Bus aracılı mesajlaşma .NET öğreticisi | Microsoft Docs
description: Aracılı mesajlaşma .NET öğreticisi.
services: service-bus
documentationcenter: na
author: sethmanheim
manager: timlt
editor: ''

ms.service: service-bus
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/27/2016
ms.author: sethm

---
# <a name="service-bus-brokered-messaging-.net-tutorial"></a>Service Bus aracılı mesajlaşma .NET öğreticisi
Azure Service Bus iki kapsamlı mesajlaşma çözümü sunar. Bunlardan birincisi, birçok aktarım protokollerini ve SOAP, WS_* ve REST dahil olmak üzere Web hizmeti standartlarını destekleyen, bulutta çalışan merkezi "geçiş" hizmetinin kullanıldığı çözümdür. İstemcinin, şirket içi hizmete doğrudan bağlantısının olmasına veya hizmetin nerede bulunduğunu bilmesine gerek yoktur. Ayrıca şirket içi hizmet için güvenlik duvarında gelen bağlantı noktalarının açık olması gerekmez.

İkinci mesajlaşma çözümü ise "aracılı" mesajlaşma işlevleri sağlar. Bunlar, Service Bus mesajlaşma altyapısını kullanan; yayımlama ve abone olma, geçici ayırma ve yük dengeleme özelliklerini destekleyen zaman uyumsuz veya ayrılmış mesajlaşma özellikleri olarak düşünülebilir. Ayrılmış iletişim birçok avantaj sunar. Örneğin, sunucular ve istemciler gerektiğinde bağlantı kurabilir ve kendi işlemlerini zaman uyumsuz olarak gerçekleştirebilir.

Bu öğretici, Service Bus aracılı mesajlaşmanın temel bileşenlerinden biri olan kuyruklarla ilgili genel bir bakış ve uygulamalı deneyim sağlamak üzere tasarlanmıştır. Bu öğreticide bulunan konu başlıklarını sırasıyla çalıştıktan sonra bir ileti listesi dolduran, kuyruk oluşturan ve iletileri bu kuyruğa gönderen bir uygulamaya sahip olursunuz. Son olarak, uygulama bu kuyruktan iletileri alır ve görüntüler. Ardından, kaynaklarını temizler ve çıkış yapar. Service Bus Geçişi kullanan bir uygulamanın nasıl oluşturulacağını açıklayan ilgili öğretici için bkz. [Service Bus geçişli mesajlaşma öğreticisi](../service-bus-relay/service-bus-relay-tutorial.md).

## <a name="introduction-and-prerequisites"></a>Giriş ve önkoşullar
Kuyruklar, bir veya birden çok rakip tüketiciye İlk Giren İlk Çıkar (FIFO) yöntemine göre ileti teslimi sunar. FIFO, genellikle iletilerin sıraya alındığı zamana bağlı bir düzende alıcılar tarafından alınıp işleneceği ve her iletinin tek bir ileti tüketicisi tarafından alınıp işleneceği anlamına gelir. Kuyrukları kullanmanın en büyük avantajlarından biri de uygulama bileşenlerinin *zamana bağlı olarak ayrılmasıdır*. Diğer bir deyişle, iletiler sürekli olarak kuyrukta depolandığından üreticilerin ve tüketicilerin iletileri aynı anda almasına ve göndermesine gerek yoktur. Buna benzer başka bir avantaj ise üreticilerin ve tüketicilerin iletileri farklı hızlarda almasına ve göndermesine olanak sağlayan *yük dengeleme* özelliğidir.

Aşağıda, öğretici başlamadan önce uygulamanız gereken bazı yönetim ve önkoşul adımları yer almaktadır. İlk adımda bir hizmet ad alanı oluşturulur ve paylaşılan erişim imzası (SAS) anahtarı edinilir. Ad alanı, Service Bus tarafından kullanıma sunulan her uygulama için bir uygulama sınırı sağlar. Hizmet ad alanı oluşturulduğunda sistem tarafından otomatik olarak bir SAS anahtarı oluşturulur. Hizmet ad alanı ve SAS anahtarı bileşimi ile kimlik bilgisi oluşur. Service Bus hizmeti, bir uygulamaya yönelik erişim için kimlik doğrulaması yapmak üzere bu kimlik bilgisini kullanır.

### <a name="create-a-service-namespace-and-obtain-a-sas-key"></a>Hizmet ad alanı oluşturma ve bir SAS anahtarı edinme
İlk adım bir hizmet ad alanı oluşturmak ve [Paylaşılan Erişim İmzası](../service-bus/service-bus-sas-overview.md) (SAS) anahtarı edinmektir. Ad alanı, Service Bus tarafından kullanıma sunulan her uygulama için bir uygulama sınırı sağlar. Hizmet ad alanı oluşturulduğunda sistem tarafından otomatik olarak bir SAS anahtarı oluşturulur. Hizmet ad alanı ve SAS anahtarı birleşimi ile Service Bus hizmetinin bir uygulamaya erişim kimliğini doğrulayan kimlik bilgisi sağlanır.

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

Sonraki adımda Visual Studio projesi oluşturulur ve kesin tür belirtilmiş [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) .NET[Liste](https://msdn.microsoft.com/library/6sh2ey19.aspx) nesnesine virgülle ayrılmış ileti listesini yükleyen iki yardımcı işlev yazılır.

### <a name="create-a-visual-studio-project"></a>Visual Studio projesi oluşturma
1. Başlat menüsünde programa sağ tıklayıp Visual Studio'yu yönetici olarak açın ve **Yönetici olarak çalıştır**'a tıklayın.
2. Yeni bir konsol uygulama projesi oluşturun. **Dosya** menüsüne tıklayın, **Yeni** seçeneği belirleyin ve ardından **Proje**'ye tıklayın. **Yeni Proje** iletişim kutusunda, **Visual C#** öğesine (**Visual C#** görüntülenmiyorsa **Diğer Diller** kısmına bakın) tıklayın, ardından **Konsol Uygulaması** şablonuna tıklayıp **QueueSample** olarak adlandırın. Varsayılan **Konum**'u kullanın. Projeyi oluşturmak için **Tamam**'a tıklayın.
3. Service Bus kitaplıklarını projenize eklemek için NuGet paket yöneticisini kullanın:
   
   1. Çözüm Gezgini'nde **QueueSample** projesine sağ tıklayın ve ardından **NuGet Paketlerini Yönet**'e tıklayın.
   2. **Nuget Paketlerini Yönet** iletişim kutusunda, **Gözat** sekmesine tıklayın ve **Azure Service Bus** hizmetini aratıp **Yükle**'ye tıklayın.
      <br />
4. Çözüm Gezgini'nde, Program.cs dosyasına çift tıklayarak dosyayı Visual Studio düzenleyicisinde açın. `QueueSample` olan varsayılan ad alanı adını `Microsoft.ServiceBus.Samples` olarak değiştirin.
   
    ```
    Microsoft.ServiceBus.Samples
    {
        ...
    ```
5. `using` deyimlerini aşağıdaki kodda gösterildiği şekilde değiştirin.
   
    ```
    using System;
    using System.Collections.Generic;
    using System.Data;
    using System.IO;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.ServiceBus.Messaging;
    ```
6. Data.csv adlı bir metin dosyası oluşturun ve aşağıdaki virgülle ayrılmış metni kopyalayın.
   
    ```
    IssueID,IssueTitle,CustomerID,CategoryID,SupportPackage,Priority,Severity,Resolved
    1,Package lost,1,1,Basic,5,1,FALSE
    2,Package damaged,1,1,Basic,5,1,FALSE
    3,Product defective,1,2,Premium,5,2,FALSE
    4,Product damaged,2,2,Premium,5,2,FALSE
    5,Package lost,2,2,Basic,5,2,TRUE
    6,Package lost,3,2,Basic,5,2,FALSE
    7,Package damaged,3,7,Premium,5,3,FALSE
    8,Product defective,3,2,Premium,5,3,FALSE
    9,Product damaged,4,6,Premium,5,3,TRUE
    10,Package lost,4,8,Basic,5,3,FALSE
    11,Package damaged,5,4,Basic,5,4,FALSE
    12,Product defective,5,4,Basic,5,4,FALSE
    13,Package lost,6,8,Basic,5,4,FALSE
    14,Package damaged,6,7,Premium,5,5,FALSE
    15,Product defective,6,2,Premium,5,5,FALSE
    ```
   
    Data.csv dosyasını kaydedip kapatın ve nereye kaydettiğinizi unutmayın.
7. Çözüm Gezgini'nde projenizin adına (bu örnekte, **QueueSample**) sağ tıkladıktan sonra **Ekle**'ye ve ardından **Var Olan Öğe**'ye tıklayın.
8. 1. adımda oluşturduğunuz Data.csv dosyasına gözatın. Dosyaya ve ardından **Ekle**'ye tıklayın. Dosya türü listesinde **Tüm Dosyalar(*.*)** seçeneğinin belirlendiğinden emin olun.

### <a name="create-a-method-that-parses-a-list-of-messages"></a>İleti listesini ayrıştıran bir yöntem oluşturma
1. `Main()` yönteminden önce, `Program` sınıfında iki değişken bildirin: Bunlardan ilki, Data.csv dosyasındaki iletilerin listesini içeren **DataTable** türüdür. Diğeri ise [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) olarak kesin tür belirtilmiş List nesnesi türünde olmalıdır. İkinci değişken, öğreticide sonraki adımlarda kullanılacak olan aracılı iletilerin listesidir.
   
    ```
    namespace Microsoft.ServiceBus.Samples
    {
        class Program
        {
   
            private static DataTable issues;
            private static List<BrokeredMessage> MessageList;
    ```
2. `Main()` dışında, Data.csv dosyasındaki ileti listesini ayrıştırıp iletileri burada gösterildiği şekilde bir [DataTable](https://msdn.microsoft.com/library/azure/system.data.datatable.aspx) tablosuna yükleyen bir `ParseCSV()` yöntemi tanımlayın. Yöntem bir **DataTable** nesnesi döndürür.
   
    ```
    static DataTable ParseCSVFile()
    {
        DataTable tableIssues = new DataTable("Issues");
        string path = @"..\..\data.csv";
        try
        {
            using (StreamReader readFile = new StreamReader(path))
            {
                string line;
                string[] row;
   
                // create the columns
                line = readFile.ReadLine();
                foreach (string columnTitle in line.Split(','))
                {
                    tableIssues.Columns.Add(columnTitle);
                }
   
                while ((line = readFile.ReadLine()) != null)
                {
                    row = line.Split(',');
                    tableIssues.Rows.Add(row);
                }
            }
        }
        catch (Exception e)
        {
            Console.WriteLine("Error:" + e.ToString());
        }
   
        return tableIssues;
    }
    ```
3. `Main()` yöntemine, `ParseCSVFile()` yöntemini çağıran bir deyim ekleyin:
   
    ```
    public static void Main(string[] args)
    {
   
        // Populate test data
        issues = ParseCSVFile();
   
    }
    ```

### <a name="create-a-method-that-loads-the-list-of-messages"></a>İleti listesini yükleyen bir yöntem oluşturma
1. `Main()` dışında, `ParseCSVFile()` tarafından döndürülen **DataTable** nesnesini alan ve tabloyu kesin tür belirtilmiş aracılı ileti listesine yükleyen bir `GenerateMessages()` yöntemi tanımlayın. Ardından, yöntem aşağıdaki örnekte olduğu gibi **List** nesnesini döndürür. 
   
    ```
    static List<BrokeredMessage> GenerateMessages(DataTable issues)
    {
        // Instantiate the brokered list object
        List<BrokeredMessage> result = new List<BrokeredMessage>();
   
        // Iterate through the table and create a brokered message for each row
        foreach (DataRow item in issues.Rows)
        {
            BrokeredMessage message = new BrokeredMessage();
            foreach (DataColumn property in issues.Columns)
            {
                message.Properties.Add(property.ColumnName, item[property]);
            }
            result.Add(message);
        }
        return result;
    }
    ```
2. `Main()` içinde, `ParseCSVFile()` öğesine yapılan çağrının hemen ardından, bağımsız değişken olarak `ParseCSVFile()` öğesinden edinilen dönüş değerini kullanarak `GenerateMessages()` yöntemini çağırmak üzere bir deyim ekleyin:
   
    ```
    public static void Main(string[] args)
    {
   
        // Populate test data
        issues = ParseCSVFile();
        MessageList = GenerateMessages(issues);
    }
    ```

### <a name="obtain-user-credentials"></a>Kullanıcı kimlik bilgilerini alma
1. İlk olarak, bu değerleri tutmak için üç genel dize değişkeni oluşturun. Önceki değişkenleri bildirdikten hemen sonra bu değişkenleri bildirin. Örnek:
   
    ```
    namespace Microsoft.ServiceBus.Samples
    {
        public class Program
        {
   
            private static DataTable issues;
            private static List<BrokeredMessage> MessageList; 
   
            // Add these variables
            private static string ServiceNamespace;
            private static string sasKeyName = "RootManageSharedAccessKey";
            private static string sasKeyValue;
            …
    ```
2. Ardından, hizmet ad alanını ve SAS anahtarını kabul edip depolayan bir işlev oluşturun. Bu yöntemi `Main()` dışına ekleyin. Örneğin: 
   
    ```
    static void CollectUserInput()
    {
        // User service namespace
        Console.Write("Please enter the namespace to use: ");
        ServiceNamespace = Console.ReadLine();
   
        // Issuer key
        Console.Write("Enter the SAS key to use: ");
        sasKeyValue = Console.ReadLine();
    }
    ```
3. `Main()` içinde, `GenerateMessages()` öğesine yapılan çağrının hemen ardından, `CollectUserInput()` yöntemini çağıran bir deyim ekleyin:
   
    ```
    public static void Main(string[] args)
    {
   
        // Populate test data
        issues = ParseCSVFile();
        MessageList = GenerateMessages(issues);
   
        // Collect user input
        CollectUserInput();
    }
    ```

### <a name="build-the-solution"></a>Çözümü derleme
Visual Studio'daki **Derle** menüsünde çalışmanızın o ana kadarki doğruluğunu onaylamak üzere **Çözümü Derle**'ye tıklayın veya **Ctrl+Shift+B**'ye basın.

## <a name="create-management-credentials"></a>Yönetim kimlik bilgileri oluşturma
Bu adımda, paylaşılan erişim imzası (SAS) kimlik bilgilerini oluşturmak üzere kullanacağınız yönetim işlemlerini tanımlayın. Uygulamanızın yetkilendirilmesi için bu kimlik bilgileri kullanılır.

1. Bu öğretici, daha anlaşılır olması için tüm kuyruk işlemlerini ayrı yöntemlerde bulundurur. `Main()` yönteminin ardından, `Program` sınıfında zaman uyumsuz bir `Queue()` yöntemi oluşturun. Örneğin:
   
    ```
    public static void Main(string[] args)
    {
    …
    }
    static async Task Queue()
    {
    }
    ```
2. Bir sonraki adımda [TokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.aspx) nesnesi kullanılarak bir SAS kimlik bilgisi oluşturulur. Oluşturma yöntemi, SAS anahtar adını ve `CollectUserInput()` yönteminde alınan değeri kullanır. `Queue()` yöntemine aşağıdaki kodu ekleyin:
   
    ```
    static async Task Queue()
    {
        // Create management credentials
        TokenProvider credentials = TokenProvider.CreateSharedAccessSignatureTokenProvider(sasKeyName,sasKeyValue);
    }
    ```
3. Önceki adımda bağımsız değişken olarak alınan yönetim kimlik bilgilerini ve ad alanı adını içeren bir URI'ye sahip yeni bir ad alanı yönetim nesnesi oluşturun. Önceki adımda eklenen kodun hemen ardından bu kodu ekleyin. `<yourNamespace>` öğesini hizmet ad alanınızın adıyla değiştirdiğinizden emin olun:
   
    ```
    NamespaceManager namespaceClient = new NamespaceManager(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);
    ```

### <a name="example"></a>Örnek
Bu noktada kodunuzun şu şekilde olması gerekir:

```
using System;
using System.Collections.Generic;
using System.Data;
using System.IO;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.ServiceBus.Messaging;

namespace Microsoft.ServiceBus.Samples
{
  public class Program
  {
    private static DataTable issues;
    private static List<BrokeredMessage> MessageList;
    private static string ServiceNamespace;
    private static string sasKeyName = "RootManageSharedAccessKey";
    private static string sasKeyValue;

    static void Main(string[] args)
    {
      // Populate test data
      issues = ParseCSVFile();
      MessageList = GenerateMessages(issues);

      // Collect user input
      CollectUserInput();

      // Add this call
      Task.WaitAll(Queue());
    }

    static async Task Queue()
    {
      // Create management credentials
      TokenProvider credentials = TokenProvider.CreateSharedAccessSignatureTokenProvider(sasKeyName, sasKeyValue);
      NamespaceManager namespaceClient = new NamespaceManager(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);
    }

    static DataTable ParseCSVFile()
    {
      DataTable tableIssues = new DataTable("Issues");
      string path = @"..\..\data.csv";
      try
      {
        using (StreamReader readFile = new StreamReader(path))
        {
          string line;
          string[] row;

          // create the columns
          line = readFile.ReadLine();
          foreach (string columnTitle in line.Split(','))
          {
            tableIssues.Columns.Add(columnTitle);
          }

          while ((line = readFile.ReadLine()) != null)
          {
            row = line.Split(',');
            tableIssues.Rows.Add(row);
          }
        }
      }
      catch (Exception e)
      {
        Console.WriteLine("Error:" + e.ToString());
      }

      return tableIssues;
    }

    static List<BrokeredMessage> GenerateMessages(DataTable issues)
    {
      // Instantiate the brokered list object
      List<BrokeredMessage> result = new List<BrokeredMessage>();

      // Iterate through the table and create a brokered message for each row
      foreach (DataRow item in issues.Rows)
      {
        BrokeredMessage message = new BrokeredMessage();
        foreach (DataColumn property in issues.Columns)
        {
          message.Properties.Add(property.ColumnName, item[property]);
        }
        result.Add(message);
      }
      return result;
    }

    static void CollectUserInput()
    {
      // User service namespace
      Console.Write("Please enter the service namespace to use: ");
      ServiceNamespace = Console.ReadLine();

      // Issuer key
      Console.Write("Please enter the issuer key to use: ");
      sasKeyValue = Console.ReadLine();
    }
  }
}
```

## <a name="send-messages-to-the-queue"></a>Kuyruğa ileti gönderme
Bu adımda, bir kuyruk oluşturun ve ardından bu aracılı iletiler listesinde bulunan iletileri kuyruğa gönderin.

### <a name="create-queue-and-send-messages-to-the-queue"></a>Kuyruk oluşturma ve kuyruğa ileti gönderme
1. İlk olarak kuyruğu oluşturun. Örneğin, bunu `myQueue` olarak adlandırın ve son adımda `Queue()` yöntemine eklediğiniz yönetim işlemlerinin hemen ardından belirtin:
   
    ```
    QueueDescription myQueue;
   
    if (namespaceClient.QueueExists("IssueTrackingQueue"))
    {
        namespaceClient.DeleteQueue("IssueTrackingQueue");
    }
   
    myQueue = namespaceClient.CreateQueue("IssueTrackingQueue");
    ```
2. `Queue()` yönteminde bağımsız değişken olarak, Service Bus URI'si yeni oluşturulmuş bir mesajlaşma fabrikası nesnesi oluşturun. Son adımda eklediğiniz yönetim işlemlerinin hemen ardından aşağıdaki kodu ekleyin. `<yourNamespace>` öğesini hizmet ad alanınızın adıyla değiştirdiğinizden emin olun:
   
    ```
    MessagingFactory factory = MessagingFactory.Create(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);
    ```
3. Ardından, [QueueClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx) sınıfını kullanarak kuyruk nesnesi oluşturun. Son adımda eklediğiniz kodun hemen ardından aşağıdaki kodu ekleyin:
   
    ```
    QueueClient myQueueClient = factory.CreateQueueClient("IssueTrackingQueue");
    ```
4. Ardından, önceden oluşturduğunuz aracılı ileti listesinde döngüye giren kodu ekleyin. Böylece her ileti kuyruğa gönderilir. Bir önceki adımla yer alan `CreateQueueClient()` bildiriminden hemen sonra aşağıdaki kodu ekleyin.
   
    ```
    // Send messages
    Console.WriteLine("Now sending messages to the queue.");
    for (int count = 0; count < 6; count++)
    {
        var issue = MessageList[count];
        issue.Label = issue.Properties["IssueTitle"].ToString();
        await myQueueClient.SendAsync(issue);
        Console.WriteLine(string.Format("Message sent: {0}, {1}", issue.Label, issue.MessageId));
    }
    ```

## <a name="receive-messages-from-the-queue"></a>Kuyruktan ileti alma
Bu adımda, önceki adımda oluşturduğunuz kuyruktan ileti listesini alın.

### <a name="create-a-receiver-and-receive-messages-from-the-queue"></a>Alıcı oluşturma ve kuyruktan ileti alma
`Queue()` yönteminde, kuyrukta gezinip [QueueClient.ReceiveAsync](https://msdn.microsoft.com/library/azure/dn130423.aspx) yöntemini kullanarak iletileri alın. Bu işlem sonunda her ileti konsola yazdırılır. Önceki adımda eklediğiniz kodun hemen ardından aşağıdaki kodu ekleyin:

```
Console.WriteLine("Now receiving messages from Queue.");
BrokeredMessage message;
while ((message = await myQueueClient.ReceiveAsync(new TimeSpan(hours: 0, minutes: 1, seconds: 5))) != null)
    {
        Console.WriteLine(string.Format("Message received: {0}, {1}, {2}", message.SequenceNumber, message.Label, message.MessageId));
        message.Complete();

        Console.WriteLine("Processing message (sleeping...)");
        Thread.Sleep(1000);
    }
```

`Thread.Sleep` öğesinin yalnızca ileti işleme benzetimi için kullanıldığını ve bunun gerçek bir mesajlaşma uygulamasında gerekli olmayacağını unutmayın.

### <a name="end-the-queue-method-and-clean-up-resources"></a>Queue yöntemini sonlandırma ve kaynakları temizleme
Önceki kodun hemen ardından, ileti fabrikasını ve kuyruk kaynaklarını temizlemek üzere aşağıdaki kodu ekleyin:

```
factory.Close();
myQueueClient.Close();
namespaceClient.DeleteQueue("IssueTrackingQueue");
```

### <a name="call-the-queue-method"></a>Queue yöntemini çağırma
Son adımda `Main()` öğesinden `Queue()` yöntemini çağıran bir deyim eklenir. Aşağıda vurgulanan kod satırını Main() yönteminin sonuna ekleyin:

```
public static void Main(string[] args)
{
    // Collect user input
    CollectUserInput();

    // Populate test data
    issues = ParseCSVFile();
    MessageList = GenerateMessages(issues);

    // Add this call
    Queue();
}
```

### <a name="example"></a>Örnek
Aşağıdaki kod, **QueueSample** uygulamasının tamamını içerir.

```
using System;
using System.Collections.Generic;
using System.Data;
using System.IO;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.ServiceBus.Messaging;

namespace Microsoft.ServiceBus.Samples
{
    public class Program
    {
        private static DataTable issues;
        private static List<BrokeredMessage> MessageList;

        // Add these variables
        private static string ServiceNamespace;
        private static string sasKeyName = "RootManageSharedAccessKey";
        private static string sasKeyValue;

        static void Main(string[] args)
        {

            // Populate test data
            issues = ParseCSVFile();
            MessageList = GenerateMessages(issues);

            // Collect user input
            CollectUserInput();

            Queue();

        }

        static async Task Queue()
        {
            // Create management credentials
            TokenProvider credentials = TokenProvider.CreateSharedAccessSignatureTokenProvider(sasKeyName, sasKeyValue);
            NamespaceManager namespaceClient = new NamespaceManager(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);

            QueueDescription myQueue;

            if (namespaceClient.QueueExists("IssueTrackingQueue"))
            {
                namespaceClient.DeleteQueue("IssueTrackingQueue");
            }

            myQueue = namespaceClient.CreateQueue("IssueTrackingQueue");

            MessagingFactory factory = MessagingFactory.Create(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);

            QueueClient myQueueClient = factory.CreateQueueClient("IssueTrackingQueue");

            // Send messages
            Console.WriteLine("Now sending messages to the queue.");
            for (int count = 0; count < 6; count++)
            {
                var issue = MessageList[count];
                issue.Label = issue.Properties["IssueTitle"].ToString();
                await myQueueClient.SendAsync(issue);
                Console.WriteLine(string.Format("Message sent: {0}, {1}", issue.Label, issue.MessageId));
            }

            Console.WriteLine("Now receiving messages from Queue.");
            BrokeredMessage message;
            while ((message = await myQueueClient.ReceiveAsync(new TimeSpan(hours: 0, minutes: 1, seconds: 5))) != null)
            {
                Console.WriteLine(string.Format("Message received: {0}, {1}, {2}", message.SequenceNumber, message.Label, message.MessageId));
                message.Complete();

                Console.WriteLine("Processing message (sleeping...)");
                Thread.Sleep(1000);
            }

            factory.Close();
            myQueueClient.Close();
            namespaceClient.DeleteQueue("IssueTrackingQueue");


        }

        static void CollectUserInput()
        {
            // User service namespace
            Console.Write("Please enter the namespace to use: ");
            ServiceNamespace = Console.ReadLine();

            // Issuer key
            Console.Write("Enter the SAS key to use: ");
            sasKeyValue = Console.ReadLine();
        }

        static List<BrokeredMessage> GenerateMessages(DataTable issues)
        {
            // Instantiate the brokered list object
            List<BrokeredMessage> result = new List<BrokeredMessage>();

            // Iterate through the table and create a brokered message for each row
            foreach (DataRow item in issues.Rows)
            {
                BrokeredMessage message = new BrokeredMessage();
                foreach (DataColumn property in issues.Columns)
                {
                    message.Properties.Add(property.ColumnName, item[property]);
                }
                result.Add(message);
            }
            return result;
        }

        static DataTable ParseCSVFile()
        {
            DataTable tableIssues = new DataTable("Issues");
            string path = @"..\..\data.csv";
            try
            {
                using (StreamReader readFile = new StreamReader(path))
                {
                    string line;
                    string[] row;

                    // create the columns
                    line = readFile.ReadLine();
                    foreach (string columnTitle in line.Split(','))
                    {
                        tableIssues.Columns.Add(columnTitle);
                    }

                    while ((line = readFile.ReadLine()) != null)
                    {
                        row = line.Split(',');
                        tableIssues.Rows.Add(row);
                    }
                }
            }
            catch (Exception e)
            {
                Console.WriteLine("Error:" + e.ToString());
            }

            return tableIssues;
        }
    }
}
```

## <a name="build-and-run-the-queuesample-application"></a>QueueSample uygulamasını derleme ve çalıştırma
Önceki adımları tamamladığınıza göre artık **QueueSample** uygulamasını derleyip çalıştırabilirsiniz.

### <a name="build-the-queuesample-application"></a>QueueSample uygulamasını derleme
Visual Studio'da **Derle** menüsünde **Çözümü Derle**'ye tıklayın veya **Ctrl + Shift + B**'ye basın. Hatalarla karşılaşırsanız lütfen önceki adımın sonunda sunulan tam örneğe bakarak kodunuzun doğru olduğunu doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticiyle birlikte Service Bus aracılı mesajlaşma işlevlerini kullanarak nasıl Service Bus istemci uygulaması ve hizmeti oluşturacağınızı gördünüz. Service Bus [Geçişi](service-bus-messaging-overview.md#Relayed-messaging) işlevinin kullanıldığı benzer bir öğretici için bkz. [Service Bus geçişli mesajlaşma öğreticisi](../service-bus-relay/service-bus-relay-tutorial.md).

[Service Bus](https://azure.microsoft.com/services/service-bus/) hakkında daha fazla bilgi edinmek için aşağıdaki konu başlıklarına bakın.

* [Service Bus mesajlaşma hizmetine genel bakış](service-bus-messaging-overview.md)
* [Service Bus ile ilgili temel bilgiler](../service-bus/service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus mimarisi](../service-bus/service-bus-architecture.md)

<!--HONumber=Oct16_HO3-->


