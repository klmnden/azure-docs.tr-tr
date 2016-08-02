<properties 
    pageTitle="Service Bus aracılı mesajlaşma REST öğreticisi | Microsoft Azure"
    description="Aracılı mesajlaşma REST öğreticisi."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="06/03/2016"
    ms.author="sethm" />

# Service Bus aracılı mesajlaşma REST öğreticisi

Bu öğretici, temel REST tabanlı Azure Service Bus kuyruğunun ve konu başlığının/aboneliğinin nasıl oluşturulacağını gösterir.

## 1. Adım: Ad alanı oluşturma

İlk adım bir hizmet ad alanı oluşturmak ve [Paylaşılan Erişim İmzası](service-bus-sas-overview.md) (SAS) anahtarı edinmektir. Hizmet ad alanı, Service Bus tarafından kullanıma sunulan her uygulama için bir uygulama sınırı sağlar. Hizmet ad alanı oluşturulduğunda sistem tarafından otomatik olarak bir SAS anahtarı oluşturulur. Hizmet ad alanı ve SAS anahtarı birleşimi ile Service Bus hizmetinin bir uygulamaya erişim kimliğini doğrulayan kimlik bilgisi sağlanır.

### Ad alanı oluşturma ve bir SAS anahtarı edinme

1. Hizmet ad alanı oluşturmak için [klasik Azure portalını][] ziyaret edin. Sol taraftaki **Service Bus** hizmetine ve ardından **Oluştur**'a tıklayın. Ad alanınız için bir ad yazıp onay işaretine tıklayın.

1. [klasik Azure portalını][] ana penceresinde, önceki adımda oluşturduğunuz ad alanı adına tıklayın.

1. **Yapılandır** sekmesine tıklayın.

1. **Paylaşılan erişim anahtarı oluşturucu** bölümünde, **RootManageSharedAccessKey** ilkesiyle ilişkili **Ana Anahtar**'ı not edin veya panoya kopyalayın  Bu değeri daha sonra bu öğreticide kullanacaksınız.

## Bir konsol istemcisi oluşturma

Service Bus kuyrukları, iletileri; ilk giren ilk çıkar özelliğine sahip bir kuyrukta depolamanıza olanak sağlar. Konu başlıkları ve abonelikler bir yayımlama/abone olma deseni uygular. Bir konu oluşturun ve ardından bu konu başlığıyla ilişkili bir veya birden çok abonelik oluşturun. İletiler konu başlığına gönderildiğinde, anında o konu başlığının abonelerine gönderilir.

Bu öğreticideki kod şunları gerçekleştirir:

- Service Bus ad alanı kaynaklarınıza erişmek için hizmet ad alanınızı ve [Paylaşılan Erişim İmzası](service-bus-sas-overview.md) (SAS) anahtarınızı kullanır.

- Bir kuyruk oluşturur, kuyruğa ileti gönderir ve kuyruktaki iletileri okur.

- Bir konu başlığı ve bu konu başlığına ait bir abonelik oluşturur ve abonelikteki iletileri gönderir ve okur.

- Service Bus hizmetinden, abonelik kuralları dahil olmak üzere tüm kuyruk, konu başlığı ve abonelik bilgilerini alır.

- Kuyruk, konu başlığı ve abonelik kaynaklarını siler.

Bu hizmet bir REST stili Web hizmeti olduğundan, özel türler bulunmaz; bunun nedeni de tüm değişimin dizeler içermesidir. Bu da, Visual Studio projesinin hiçbir Service Bus kitaplığına başvuru yapmaması gerektiğini ifade eder.

İlk adımda hizmet ad alanını ve kimlik bilgilerini aldıktan sonra, bir sonraki adımınız temel bir Visual Studio konsol uygulaması oluşturmaktır.

### Konsol uygulaması oluşturma

1. **Başlat**menüsünde programa sağ tıklayarak bir yönetici olarak Visual Studio'yu açın ve **Yönetici olarak çalıştır**'a tıklayın.

1. Yeni bir konsol uygulama projesi oluşturun. **Dosya** menüsüne tıklayın ve **Yeni** seçeneği belirleyin, ardından **Proje**'ye tıklayın. **Yeni proje** iletişim kutusunda, **Visual C#** öğesine tıklayıp (**Visual C#** görüntülenmiyorsa **Diğer Diller** kısmına bakın), **Konsol Uygulaması** şablonunu seçin, ardından **Microsoft.ServiceBus.Samples** olarak adlandırın. Varsayılan Konum'u kullanın. Projeyi oluşturmak için **Tamam**'a tıklayın.

1. Program.cs içinde `using` deyimlerinizin aşağıdaki gibi göründüğünden emin olun:

    ```
    using System;
    using System.Globalization;
    using System.IO;
    using System.Net;
    using System.Security.Cryptography;
    using System.Text;
    using System.Xml;
    ```

1. Gerekirse, program ad alanını Visual Studio varsayılanından `Microsoft.ServiceBus.Samples` olacak şekilde yeniden adlandırın.

1. `Program` sınıfında, aşağıdaki genel değişkenleri ekleyin:
    
    ```
    static string serviceNamespace;
    static string baseAddress;
    static string token;
    const string sbHostName = "servicebus.windows.net";
    ```

1. `Main()` kısmında, aşağıdaki kodu yapıştırın:

    ```
    Console.Write("Enter your service namespace: ");
    serviceNamespace = Console.ReadLine();
    
    Console.Write("Enter your SAS key: ");
    string SASKey = Console.ReadLine();
    
    baseAddress = "https://" + serviceNamespace + "." + sbHostName + "/";
    try
    {
        token = GetSASToken("RootManageSharedAccessKey", SASKey);
    
        string queueName = "Queue" + Guid.NewGuid().ToString();
    
        // Create and put a message in the queue
        CreateQueue(queueName, token);
        SendMessage(queueName, "msg1");
        string msg = ReceiveAndDeleteMessage(queueName);
    
        string topicName = "Topic" + Guid.NewGuid().ToString();
        string subscriptionName = "Subscription" + Guid.NewGuid().ToString();
        CreateTopic(topicName);
        CreateSubscription(topicName, subscriptionName);
        SendMessage(topicName, "msg2");
    
        Console.WriteLine(ReceiveAndDeleteMessage(topicName + "/Subscriptions/" + subscriptionName));
    
        // Get an Atom feed with all the queues in the namespace
        Console.WriteLine(GetResources("$Resources/Queues"));
    
        // Get an Atom feed with all the topics in the namespace
        Console.WriteLine(GetResources("$Resources/Topics"));
    
        // Get an Atom feed with all the subscriptions for the topic we just created
        Console.WriteLine(GetResources(topicName + "/Subscriptions"));
    
        // Get an Atom feed with all the rules for the topic and subscription we just created
        Console.WriteLine(GetResources(topicName + "/Subscriptions/" + subscriptionName + "/Rules"));
    
        // Delete the queue we created
        DeleteResource(queueName);
    
        // Delete the topic we created
        DeleteResource(topicName);
    
        // Get an Atom feed with all the topics in the namespace, it shouldn't have the one we created now
        Console.WriteLine(GetResources("$Resources/Topics"));
    
        // Get an Atom feed with all the queues in the namespace, it shouldn't have the one we created now
        Console.WriteLine(GetResources("$Resources/Queues"));
    }
    catch (WebException we)
    {
        using (HttpWebResponse response = we.Response as HttpWebResponse)
        {
            if (response != null)
            {
                Console.WriteLine(new StreamReader(response.GetResponseStream()).ReadToEnd());
            }
            else
            {
                Console.WriteLine(we.ToString());
            }
        }
    }
    
    Console.WriteLine("\nPress ENTER to exit.");
    Console.ReadLine();
    ```

## Yönetim kimlik bilgileri oluşturma

Sonraki adım, önceki adımda girdiğiniz SAS anahtarını ve ad alanını işleyen bir yöntem yazmaktır ve bu işlem bir SAS belirteci döndürür. Bu örnek, bir saat için geçerli olan bir SAS belirteci oluşturur.

### GetSASToken() yöntemi oluşturma

`Main()` yönteminden sonra `Program` sınıfında aşağıdaki kodu yapıştırın:

```
private static string GetSASToken(string SASKeyName, string SASKeyValue)
{
  TimeSpan fromEpochStart = DateTime.UtcNow - new DateTime(1970, 1, 1);
  string expiry = Convert.ToString((int)fromEpochStart.TotalSeconds + 3600);
  string stringToSign = WebUtility.UrlEncode(baseAddress) + "\n" + expiry;
  HMACSHA256 hmac = new HMACSHA256(Encoding.UTF8.GetBytes(SASKeyValue));

  string signature = Convert.ToBase64String(hmac.ComputeHash(Encoding.UTF8.GetBytes(stringToSign)));
  string sasToken = String.Format(CultureInfo.InvariantCulture, "SharedAccessSignature sr={0}&sig={1}&se={2}&skn={3}",
      WebUtility.UrlEncode(baseAddress), WebUtility.UrlEncode(signature), expiry, SASKeyName);
  return sasToken;
}
```
## Kuyruk oluşturma

Sonraki adım, bir kuyruk oluşturmak için REST stili HTTP PUT komutunu kullanan bir yöntem yazmaktır.

Önceki adımda eklediğiniz `GetSASToken()` kodundan hemen sonra aşağıdaki kodu yapıştırın:

```
// Uses HTTP PUT to create the queue
private static string CreateQueue(string queueName, string token)
{
    // Create the URI of the new queue, note that this uses the HTTPS scheme
    string queueAddress = baseAddress + queueName;
    WebClient webClient = new WebClient();
    webClient.Headers[HttpRequestHeader.Authorization] = token;

    Console.WriteLine("\nCreating queue {0}", queueAddress);
    // Prepare the body of the create queue request
    var putData = @"<entry xmlns=""http://www.w3.org/2005/Atom"">
                          <title type=""text"">" + queueName + @"</title>
                          <content type=""application/xml"">
                            <QueueDescription xmlns:i=""http://www.w3.org/2001/XMLSchema-instance"" xmlns=""http://schemas.microsoft.com/netservices/2010/10/servicebus/connect"" />
                          </content>
                        </entry>";

    byte[] response = webClient.UploadData(queueAddress, "PUT", Encoding.UTF8.GetBytes(putData));
    return Encoding.UTF8.GetString(response);
}
```

## Kuyruğa bir ileti gönderme

Bu adımda, önceki adımda oluşturduğunuz kuyruğa ileti göndermek için REST stili HTTP POST komutu kullanan bir yöntem eklersiniz.

1. Önceki adımda eklediğiniz `CreateQueue()` kodundan hemen sonra aşağıdaki kodu yapıştırın:

    ```
    // Sends a message to the "queueName" queue, given the name and the value to enqueue
    // Uses an HTTP POST request.
    private static void SendMessage(string queueName, string body)
    {
        string fullAddress = baseAddress + queueName + "/messages" + "?timeout=60&api-version=2013-08 ";
        Console.WriteLine("\nSending message {0} - to address {1}", body, fullAddress);
        WebClient webClient = new WebClient();
        webClient.Headers[HttpRequestHeader.Authorization] = token;
    
        webClient.UploadData(fullAddress, "POST", Encoding.UTF8.GetBytes(body));
    }
    ```

1. Standart aracılı ileti özellikleri bir `BrokerProperties` HTTP üst bilgisinde yer alır. Aracı özellikleri JSON formatında seri haline getirilmelidir. 30 saniyelik **TimeToLive** değeri belirtmek ve iletiye "M1" ileti etiketi eklemek için önceki örnekte gösterilen `webClient.UploadData()` çağrısının hemen öncesine aşağıdaki kodu ekleyin:

    ```
    // Add brokered message properties "TimeToLive" and "Label"
    webClient.Headers.Add("BrokerProperties", "{ \"TimeToLive\":30, \"Label\":\"M1\"}");
    ```

    Aracılı ileti özelliklerinin eklenmiş olduğunu ve sonra da ekleneceğini unutmayın. Bu nedenle gönderme isteği, isteğin parçası olan tüm aracılı ileti özelliklerini destekleyen bir API sürümü belirtmelidir. Belirtilen API sürümü bir aracılı ileti özelliğini desteklemiyorsa bu özellik yoksayılır.

1. Özel ileti özellikleri bir anahtar-değer çiftleri kümesi olarak tanımlanır. Her bir özel özellik kendi TPPT üst bilgisinde depolanır. "Öncelik" ve "Müşteri" özel özelliklerini eklemek için önceki örnekte gösterilen `webClient.UploadData()` çağrısından hemen önce aşağıdaki kodu ekleyin:

    ```
    // Add custom properties "Priority" and "Customer".
    webClient.Headers.Add("Priority", "High");
    webClient.Headers.Add("Customer", "12345");
    ```

## Kuyruktan bir ileti alma ve silme

Bir sonraki adım, kuyruktan ileti almak ve silmek için REST stili HTTP DELETE komutu kullanan bir yöntem eklemektir.

Önceki adımda eklediğiniz `SendMessage()` kodundan hemen sonra aşağıdaki kodu yapıştırın:

```
// Receives and deletes the next message from the given resource (queue, topic, or subscription)
// using the resourceName and an HTTP DELETE request
private static string ReceiveAndDeleteMessage(string resourceName)
{
    string fullAddress = baseAddress + resourceName + "/messages/head" + "?timeout=60";
    Console.WriteLine("\nRetrieving message from {0}", fullAddress);
    WebClient webClient = new WebClient();
    webClient.Headers[HttpRequestHeader.Authorization] = token;

    byte[] response = webClient.UploadData(fullAddress, "DELETE", new byte[0]);
    string responseStr = Encoding.UTF8.GetString(response);

    Console.WriteLine(responseStr);
    return responseStr;
}
```

## Konu başlığı ve abonelik oluşturma

Sonraki adım, bir konu başlığı oluşturmak için REST stili HTTP PUT komutunu kullanan bir yöntem yazmaktır. Ardından, bu konu başlığına bir abonelik oluşturan yöntemi yazın.

### Konu başlığı oluşturma

Önceki adımda eklediğiniz `ReceiveAndDeleteMessage()` kodundan hemen sonra aşağıdaki kodu yapıştırın:

```
// Using an HTTP PUT request.
private static string CreateTopic(string topicName)
{
    var topicAddress = baseAddress + topicName;
    WebClient webClient = new WebClient();
    webClient.Headers[HttpRequestHeader.Authorization] = token;

    Console.WriteLine("\nCreating topic {0}", topicAddress);
    // Prepare the body of the create queue request
    var putData = @"<entry xmlns=""http://www.w3.org/2005/Atom"">
                                  <title type=""text"">" + topicName + @"</title>
                                  <content type=""application/xml"">
                                    <TopicDescription xmlns:i=""http://www.w3.org/2001/XMLSchema-instance"" xmlns=""http://schemas.microsoft.com/netservices/2010/10/servicebus/connect"" />
                                  </content>
                                </entry>";

    byte[] response = webClient.UploadData(topicAddress, "PUT", Encoding.UTF8.GetBytes(putData));
    return Encoding.UTF8.GetString(response);
}
```

### Abonelik oluşturma

Aşağıdaki kod, önceki adımda oluşturduğunuz konu başlığına bir abonelik oluşturur. `CreateTopic()` tanımından hemen sonra aşağıdaki kodu ekleyin:

```
private static string CreateSubscription(string topicName, string subscriptionName)
{
    var subscriptionAddress = baseAddress + topicName + "/Subscriptions/" + subscriptionName;
    WebClient webClient = new WebClient();
    webClient.Headers[HttpRequestHeader.Authorization] = token;

    Console.WriteLine("\nCreating subscription {0}", subscriptionAddress);
    // Prepare the body of the create queue request
    var putData = @"<entry xmlns=""http://www.w3.org/2005/Atom"">
                                  <title type=""text"">" + subscriptionName + @"</title>
                                  <content type=""application/xml"">
                                    <SubscriptionDescription xmlns:i=""http://www.w3.org/2001/XMLSchema-instance"" xmlns=""http://schemas.microsoft.com/netservices/2010/10/servicebus/connect"" />
                                  </content>
                                </entry>";

    byte[] response = webClient.UploadData(subscriptionAddress, "PUT", Encoding.UTF8.GetBytes(putData));
    return Encoding.UTF8.GetString(response);
}
```

## İleti kaynaklarını alma

Bu adımda, ileti özelliklerini alan kodu eklersiniz ve ardından önceki adımlarda oluşturduğunuz mesajlaşma kaynaklarını silersiniz.

### Belirtilen kaynakları içeren Atom akışı alma

Önceki adımda eklediğiniz `CreateSubscription()` yönteminden hemen sonra aşağıdaki kodu ekleyin:

```
private static string GetResources(string resourceAddress)
{
    string fullAddress = baseAddress + resourceAddress;
    WebClient webClient = new WebClient();
    webClient.Headers[HttpRequestHeader.Authorization] = token;
    Console.WriteLine("\nGetting resources from {0}", fullAddress);
    return FormatXml(webClient.DownloadString(fullAddress));
}
```

### Mesajlaşma varlıklarını silme

Önceki adımda eklediğiniz kodun hemen ardından aşağıdaki kodu ekleyin:

```
private static string DeleteResource(string resourceName)
{
    string fullAddress = baseAddress + resourceName;
    WebClient webClient = new WebClient();
    webClient.Headers[HttpRequestHeader.Authorization] = token;

    Console.WriteLine("\nDeleting resource at {0}", fullAddress);
    byte[] response = webClient.UploadData(fullAddress, "DELETE", newbyte[0]);
    return Encoding.UTF8.GetString(response);
}
```

### Atom akışı biçimlendirme

`GetResources()` yöntemi, daha okunur olması için alınan Atom akışını yeniden biçimlendiren `FormatXml()` yöntemine yönelik bir çağrı içerir. Aşağıda verilenler `FormatXml()` kodunun açıklamasıdır; önceki bölümde eklediğiniz `DeleteResource()` kodundan hemen sonra bu kodu ekleyin:

```
// Formats the XML string to be more human-readable; intended for display purposes
private static string FormatXml(string inputXml)
{
    XmlDocument document = new XmlDocument();
    document.Load(new StringReader(inputXml));

    StringBuilder builder = new StringBuilder();
    using (XmlTextWriter writer = new XmlTextWriter(new StringWriter(builder)))
    {
        writer.Formatting = Formatting.Indented;
        document.Save(writer);
    }

    return builder.ToString();
}
```

## Uygulamayı derleme ve çalıştırma

Artık uygulamayı derleyebilir ve çalıştırabilirsiniz. Visual Studio'da **Derleme** menüsünde **Çözümü Derle**'ye tıklayın veya F6'ya basın.

### Uygulamayı çalıştırma

Herhangi bir hata yoksa uygulamayı çalıştırmak için F5'e basın. İstendiğinde, ilk adımda aldığınız ad alanını, SAS anahtarı adını ve SAS anahtarı değerini girin.

### Örnek

Öğreticideki adımların tümü uygulandıktan sonra görüneceğinden, aşağıdaki örnek eksiksiz koddur.

```
using System;
using System.Globalization;
using System.IO;
using System.Net;
using System.Security.Cryptography;
using System.Text;
using System.Xml;

namespace Microsoft.ServiceBus.Samples
{
    class Program
    {
        static string serviceNamespace;
        static string baseAddress;
        static string token;
        const string sbHostName = "servicebus.windows.net";

        static void Main(string[] args)
        {
            Console.Write("Enter your service namespace: ");
            serviceNamespace = Console.ReadLine();

            Console.Write("Enter your SAS key: ");
            string SASKey = Console.ReadLine();

            baseAddress = "https://" + serviceNamespace + "." + sbHostName + "/";
            try
            {
                token = GetSASToken("RootManageSharedAccessKey", SASKey);

                string queueName = "Queue" + Guid.NewGuid().ToString();

                // Create and put a message in the queue
                CreateQueue(queueName, token);
                SendMessage(queueName, "msg1");
                string msg = ReceiveAndDeleteMessage(queueName);

                string topicName = "Topic" + Guid.NewGuid().ToString();
                string subscriptionName = "Subscription" + Guid.NewGuid().ToString();
                CreateTopic(topicName);
                CreateSubscription(topicName, subscriptionName);
                SendMessage(topicName, "msg2");

                Console.WriteLine(ReceiveAndDeleteMessage(topicName + "/Subscriptions/" + subscriptionName));

                // Get an Atom feed with all the queues in the namespace
                Console.WriteLine(GetResources("$Resources/Queues"));

                // Get an Atom feed with all the topics in the namespace
                Console.WriteLine(GetResources("$Resources/Topics"));

                // Get an Atom feed with all the subscriptions for the topic we just created
                Console.WriteLine(GetResources(topicName + "/Subscriptions"));

                // Get an Atom feed with all the rules for the topic and subscription we just created
                Console.WriteLine(GetResources(topicName + "/Subscriptions/" + subscriptionName + "/Rules"));

                // Delete the queue we created
                DeleteResource(queueName);

                // Delete the topic we created
                DeleteResource(topicName);

                // Get an Atom feed with all the topics in the namespace, it shouldn't have the one we created now
                Console.WriteLine(GetResources("$Resources/Topics"));

                // Get an Atom feed with all the queues in the namespace, it shouldn't have the one we created now
                Console.WriteLine(GetResources("$Resources/Queues"));
            }
            catch (WebException we)
            {
                using (HttpWebResponse response = we.Response as HttpWebResponse)
                {
                    if (response != null)
                    {
                        Console.WriteLine(new StreamReader(response.GetResponseStream()).ReadToEnd());
                    }
                    else
                    {
                        Console.WriteLine(we.ToString());
                    }
                }
            }

            Console.WriteLine("\nPress ENTER to exit.");
            Console.ReadLine();
        }

        private static string GetSASToken(string SASKeyName, string SASKeyValue)
        {
            TimeSpan fromEpochStart = DateTime.UtcNow - new DateTime(1970, 1, 1);
            string expiry = Convert.ToString((int)fromEpochStart.TotalSeconds + 3600);
            string stringToSign = WebUtility.UrlEncode(baseAddress) + "\n" + expiry;
            HMACSHA256 hmac = new HMACSHA256(Encoding.UTF8.GetBytes(SASKeyValue));

            string signature = Convert.ToBase64String(hmac.ComputeHash(Encoding.UTF8.GetBytes(stringToSign)));
            string sasToken = String.Format(CultureInfo.InvariantCulture, "SharedAccessSignature sr={0}&sig={1}&se={2}&skn={3}",
                WebUtility.UrlEncode(baseAddress), WebUtility.UrlEncode(signature), expiry, SASKeyName);
            return sasToken;
        }

        // Uses HTTP PUT to create the queue
        private static string CreateQueue(string queueName, string token)
        {
            // Create the URI of the new queue, note that this uses the HTTPS scheme
            string queueAddress = baseAddress + queueName;
            WebClient webClient = new WebClient();
            webClient.Headers[HttpRequestHeader.Authorization] = token;

            Console.WriteLine("\nCreating queue {0}", queueAddress);
            // Prepare the body of the create queue request
            var putData = @"<entry xmlns=""http://www.w3.org/2005/Atom"">
                                  <title type=""text"">" + queueName + @"</title>
                                  <content type=""application/xml"">
                                    <QueueDescription xmlns:i=""http://www.w3.org/2001/XMLSchema-instance"" xmlns=""http://schemas.microsoft.com/netservices/2010/10/servicebus/connect"" />
                                  </content>
                                </entry>";

            byte[] response = webClient.UploadData(queueAddress, "PUT", Encoding.UTF8.GetBytes(putData));
            return Encoding.UTF8.GetString(response);
        }

        // Sends a message to the "queueName" queue, given the name and the value to enqueue
        // Uses an HTTP POST request.
        private static void SendMessage(string queueName, string body)
        {
            string fullAddress = baseAddress + queueName + "/messages" + "?timeout=60&api-version=2013-08 ";
            Console.WriteLine("\nSending message {0} - to address {1}", body, fullAddress);
            WebClient webClient = new WebClient();
            webClient.Headers[HttpRequestHeader.Authorization] = token;
            // Add brokered message properties “TimeToLive” and “Label”.
            webClient.Headers.Add("BrokerProperties", "{ \"TimeToLive\":30, \"Label\":\"M1\"}");
            // Add custom properties “Priority” and “Customer”.
            webClient.Headers.Add("Priority", "High");
            webClient.Headers.Add("Customer", "12345");
            webClient.UploadData(fullAddress, "POST", Encoding.UTF8.GetBytes(body));

        }

        // Receives and deletes the next message from the given resource (queue, topic, or subscription)
        // using the resourceName and an HTTP DELETE request.
        private static string ReceiveAndDeleteMessage(string resourceName)
        {
            string fullAddress = baseAddress + resourceName + "/messages/head" + "?timeout=60";
            Console.WriteLine("\nRetrieving message from {0}", fullAddress);
            WebClient webClient = new WebClient();
            webClient.Headers[HttpRequestHeader.Authorization] = token;

            byte[] response = webClient.UploadData(fullAddress, "DELETE", new byte[0]);
            string responseStr = Encoding.UTF8.GetString(response);

            Console.WriteLine(responseStr);
            return responseStr;
        }

        // Using an HTTP PUT request.
        private static string CreateTopic(string topicName)
        {
            var topicAddress = baseAddress + topicName;
            WebClient webClient = new WebClient();
            webClient.Headers[HttpRequestHeader.Authorization] = token;

            Console.WriteLine("\nCreating topic {0}", topicAddress);
            // Prepare the body of the create queue request
            var putData = @"<entry xmlns=""http://www.w3.org/2005/Atom"">
                                  <title type=""text"">" + topicName + @"</title>
                                  <content type=""application/xml"">
                                    <TopicDescription xmlns:i=""http://www.w3.org/2001/XMLSchema-instance"" xmlns=""http://schemas.microsoft.com/netservices/2010/10/servicebus/connect"" />
                                  </content>
                                </entry>";

            byte[] response = webClient.UploadData(topicAddress, "PUT", Encoding.UTF8.GetBytes(putData));
            return Encoding.UTF8.GetString(response);
        }

        private static string CreateSubscription(string topicName, string subscriptionName)
        {
            var subscriptionAddress = baseAddress + topicName + "/Subscriptions/" + subscriptionName;
            WebClient webClient = new WebClient();
            webClient.Headers[HttpRequestHeader.Authorization] = token;

            Console.WriteLine("\nCreating subscription {0}", subscriptionAddress);
            // Prepare the body of the create queue request
            var putData = @"<entry xmlns=""http://www.w3.org/2005/Atom"">
                                  <title type=""text"">" + subscriptionName + @"</title>
                                  <content type=""application/xml"">
                                    <SubscriptionDescription xmlns:i=""http://www.w3.org/2001/XMLSchema-instance"" xmlns=""http://schemas.microsoft.com/netservices/2010/10/servicebus/connect"" />
                                  </content>
                                </entry>";

            byte[] response = webClient.UploadData(subscriptionAddress, "PUT", Encoding.UTF8.GetBytes(putData));
            return Encoding.UTF8.GetString(response);
        }

        private static string GetResources(string resourceAddress)
        {
            string fullAddress = baseAddress + resourceAddress;
            WebClient webClient = new WebClient();
            webClient.Headers[HttpRequestHeader.Authorization] = token;
            Console.WriteLine("\nGetting resources from {0}", fullAddress);
            return FormatXml(webClient.DownloadString(fullAddress));
        }

        private static string DeleteResource(string resourceName)
        {
            string fullAddress = baseAddress + resourceName;
            WebClient webClient = new WebClient();
            webClient.Headers[HttpRequestHeader.Authorization] = token;

            Console.WriteLine("\nDeleting resource at {0}", fullAddress);
            byte[] response = webClient.UploadData(fullAddress, "DELETE", new byte[0]);
            return Encoding.UTF8.GetString(response);
        }

        // Formats the XML string to be more human-readable; intended for display purposes
        private static string FormatXml(string inputXml)
        {
            XmlDocument document = new XmlDocument();
            document.Load(new StringReader(inputXml));

            StringBuilder builder = new StringBuilder();
            using (XmlTextWriter writer = new XmlTextWriter(new StringWriter(builder)))
            {
                writer.Formatting = Formatting.Indented;
                document.Save(writer);
            }

            return builder.ToString();
        }
    }
}
```

## Sonraki adımlar

Daha fazla bilgi edinmek için şu makalelere bakın:

- [Service Bus mesajlaşma hizmetine genel bakış](service-bus-messaging-overview.md)
- [Azure Service Bus ile ilgili temel bilgiler](service-bus-fundamentals-hybrid-solutions.md)
- [Service Bus geçişi REST öğreticisi](service-bus-relay-rest-tutorial.md)

[klasik Azure portalını]: http://manage.windowsazure.com



<!---HONumber=Jun16_HO2-->


