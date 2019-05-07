---
title: Öğretici - Azure depolama kuyrukları'yla çalışma - Azure depolama
description: Kuyruklar ve ekleme oluşturmak için Azure Queue hizmetini kullanma hakkında bir öğretici alın ve iletilerini silin.
services: storage
author: mhopkins-msft
ms.author: mhopkins
ms.reviewer: cbrooks
ms.service: storage
ms.subservice: queues
ms.topic: tutorial
ms.date: 04/24/2019
ms.openlocfilehash: 81d7572f800f191791158f2c1f99e1f072980116
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65151060"
---
# <a name="tutorial-work-with-azure-storage-queues"></a>Öğretici: Azure depolama kuyrukları ile çalışma

Azure kuyruk depolama, dağıtılmış uygulamanın bileşenleri arasındaki iletişimi etkinleştirmek için sıraları bulut tabanlı uygular. Her bir kuyruk, bir gönderen bileşeni tarafından eklenir ve bir alıcı bileşeni tarafından işlenen iletilerin listesini tutar. Bir kuyruk, uygulamanızı hemen talebi karşılamak üzere ölçeklendirebilir. Bu makalede, Azure depolama kuyruğu ile çalışmak için temel adımlar gösterilir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
>
> - Azure Storage hesabı oluşturma
> - Uygulama oluşturma
> - Zaman uyumsuz kod için destek eklendi
> - Bir kuyruk oluşturma
> - Bir kuyruğa ileti Ekle
> - İletileri sıradan çıkarma
> - Boş bir kuyruk silme
> - Komut satırı bağımsız değişkenleri denetleyin
> - Uygulamayı derleme ve çalıştırma

## <a name="prerequisites"></a>Önkoşullar

- Platformlar arası ücretsiz bir kopyasını alma [Visual Studio Code](https://code.visualstudio.com/download) Düzenleyici.
- İndirme ve yükleme [.NET Core SDK'sı](https://dotnet.microsoft.com/download).
- Geçerli bir Azure aboneliğiniz yoksa, oluşturun bir [ücretsiz bir hesap](https://azure.microsoft.com/free/) başlamadan önce.

## <a name="create-an-azure-storage-account"></a>Azure Storage hesabı oluşturma

İlk olarak Azure depolama hesabı oluşturun. Bir depolama hesabı oluşturmak için adım adım yönergeler için bkz. [depolama hesabı oluşturma](../common/storage-quickstart-create-account.md?toc=%2Fazure%2Fstorage%2Fqueues%2Ftoc.json) hızlı başlangıç.

## <a name="create-the-app"></a>Uygulama oluşturma

Adlı bir .NET Core uygulaması oluşturmak **QueueApp**. Kolaylık olması için bu uygulamayı hem gönderin ve ileti kuyruğu üzerinden alın.

1. (Örneğin, CMD, PowerShell veya Azure CLI) olan bir konsol penceresinde `dotnet new` adıyla yeni bir konsol uygulaması oluşturmak için komut **QueueApp**. Bu komut, bir basit "Hello World" oluşturur C# tek bir dosya ile proje: **Program.cs**.

   ```console
   dotnet new console -n QueueApp
   ```

2. Yeni oluşturulan geçiş **QueueApp** klasörünü ve tüm iyi durumda olduğunu doğrulamak için uygulama oluşturma.

   ```console
   cd QueueApp
   ```

   ```console
   dotnet build
   ```

   Aşağıdakine benzer bir sonuç görmeniz gerekir:

   ```output
   C:\Tutorials>dotnet new console -n QueueApp
   The template "Console Application" was created successfully.

   Processing post-creation actions...
   Running 'dotnet restore' on QueueApp\QueueApp.csproj...
     Restore completed in 155.62 ms for C:\Tutorials\QueueApp\QueueApp.csproj.

   Restore succeeded.

   C:\Tutorials>cd QueueApp

   C:\Tutorials\QueueApp>dotnet build
   Microsoft (R) Build Engine version 16.0.450+ga8dc7f1d34 for .NET Core
   Copyright (C) Microsoft Corporation. All rights reserved.

     Restore completed in 40.87 ms for C:\Tutorials\QueueApp\QueueApp.csproj.
     QueueApp -> C:\Tutorials\QueueApp\bin\Debug\netcoreapp2.1\QueueApp.dll

   Build succeeded.
       0 Warning(s)
       0 Error(s)

   Time Elapsed 00:00:02.40

   C:\Tutorials\QueueApp>_
   ```

## <a name="add-support-for-asynchronous-code"></a>Zaman uyumsuz kod için destek eklendi

Kod, uygulamanın bulut kaynaklarını kullandığından, zaman uyumsuz olarak çalışır. Ancak, C#'s **zaman uyumsuz** ve **await** geçerli anahtar olmayan **ana** yöntemleri kadar C# 7.1. İçin derleyicinin bir bayrak aracılığıyla kolayca geçiş yapabilirsiniz **csproj** dosya.

1. Proje dizininde komut satırından yazın `code .` Visual Studio Code geçerli dizinde açın. Komut satırı penceresini açık tutun. Daha sonra çalıştırılabilmesi için daha fazla komut olacaktır. Eklemek isteyip istemediğiniz sorulursa C# derlemek ve hata ayıklama, için gerekli varlıkları tıklayın **Evet** düğmesi.

2. Açık **QueueApp.csproj** düzenleyicideki dosyada.

3. Ekleme `<LangVersion>7.1</LangVersion>` ilk içine **PropertyGroup** derleme dosyası içinde. Yalnızca eklediğinizden emin olun **LangVersion** olarak etiketleyin, **TargetFramework** yüklediğiniz .NET hangi sürümünün bağlı olarak farklı olabilir.

   ```xml
   <Project Sdk="Microsoft.NET.Sdk">

     <PropertyGroup>
       <OutputType>Exe</OutputType>
       <TargetFramework>netcoreapp2.1</TargetFramework>
       <LangVersion>7.1</LangVersion>
     </PropertyGroup>

   ...

   ```

4. Kaydet **QueueApp.csproj** dosya.

5. Açık **Program.cs** kaynak dosya ve güncelleştirme **ana** zaman uyumsuz olarak çalıştırmak için yöntemi. Değiştirin **void** ile bir **zaman uyumsuz görev** değeri döndürür.

   ```csharp
   static async Task Main(string[] args)
   ```

6. Kaydet **Program.cs** dosya.

## <a name="create-a-queue"></a>Bir kuyruk oluşturma

1. Yükleme **WindowsAzure. Depolama** projeye sahip paket `dotnet add package` komutu. Konsol penceresinde proje klasöründen aşağıdaki dotnet komutu yürütün.

   ```console
   dotnet add package WindowsAzure.Storage
   ```

2. Üst kısmındaki **Program.cs** dosyasında, aşağıdaki ad alanlarını ekleyin hemen sonra `using System;` deyimi. Bu uygulama, Azure Depolama'ya Bağlan ve kuyrukları ile çalışmak için bu ad alanlarında türleri kullanır.

   ```csharp
   using System.Threading.Tasks;
   using Microsoft.WindowsAzure.Storage;
   using Microsoft.WindowsAzure.Storage.Queue;
   ```

3. Kaydet **Program.cs** dosya.

### <a name="get-your-connection-string"></a>Bağlantı dizenizi alma

İstemci Kitaplığı, bağlantınızı kurmak için bir bağlantı dizesi kullanır. Bağlantı dizenizi kullanılabilir **ayarları** Azure portalında depolama hesabınızın bölümü.

1. Web tarayıcınızda oturum açın [Azure portalında](https://portal.azure.com/).

2. Azure portalda depolama hesabınıza gidin.

3. Seçin **erişim anahtarları**.

4. Tıklayın **kopyalama** sağındaki düğmeye **bağlantı dizesi** alan.

![Bağlantı dizesi](media/storage-tutorial-queues/get-connection-string.png)

Bağlantı dizesi bu biçimdedir:

   ```
   "DefaultEndpointsProtocol=https;AccountName=<your storage account name>;AccountKey=<your key>;EndpointSuffix=core.windows.net"
   ```

### <a name="add-the-connection-string-to-the-app"></a>Bağlantı dizesi uygulamaya ekleme

Depolama hesabına erişebilmesi için bu bağlantı dizesini uygulamaya ekleyin.

1. Visual Studio Code için geri geçiş yapın.

2. İçinde **Program** sınıfı, ekleme bir `private const string connectionString =` bağlantı dizesini tutan üyesi.

3. Eşittir işaretinden sonra Azure portalında daha önce kopyaladığınız dize değeri olarak yapıştırın. **ConnectionString** değeri hesabınız için benzersiz olacaktır.

4. "Hello World" koddan kaldırdığınızdan **ana**. Kodunuzu benzemelidir aşağıdaki ancak benzersiz bir bağlantı dizesi değerinizi.

   ```csharp
   namespace QueueApp
   {
       class Program
       {
           private const string connectionString = "DefaultEndpointsProtocol=https; ...";

           static async Task Main(string[] args)
           {
           }
       }
   }
   ```

5. Güncelleştirme **ana** oluşturmak için bir **CloudQueue** daha sonra Gönder geçirilen ve yöntemlerden almak nesnesidir.

   ```csharp
        static async Task Main(string[] args)
        {
            CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);
            CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
            CloudQueue queue = queueClient.GetQueueReference("mystoragequeue");
        }
   ```

6. Dosyayı kaydedin.

## <a name="insert-messages-into-the-queue"></a>İletilerin Kuyruğa Ekle

Kuyruğa ileti göndermek için yeni bir yöntem oluşturun. Aşağıdaki yöntemi ekleyin, **Program** sınıfı. Bu yöntem bir sıra başvuru alır ve ardından çağırarak zaten yoksa yeni bir sıra oluşturur [Createıfnotexistsasync](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.queue.cloudqueue.createifnotexistsasync?view=azure-dotnet). Daha sonra bu iletiyi çağırarak eklediğinde kuyruğun [AddMessageAsync](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.queue.cloudqueue.addmessageasync?view=azure-dotnet).

1. Aşağıdaki **SendMessageAsync** yönteme, **Program** sınıfı.

   ```csharp
   static async Task SendMessageAsync(CloudQueue theQueue, string newMessage)
   {
       bool createdQueue = await theQueue.CreateIfNotExistsAsync();

       if (createdQueue)
       {
           Console.WriteLine("The queue was created.");
       }

       CloudQueueMessage message = new CloudQueueMessage(newMessage);
       await theQueue.AddMessageAsync(message);
   }
   ```

2. Dosyayı kaydedin.

## <a name="dequeue-messages"></a>İletileri sıradan çıkarma

Adlı yeni bir yöntem oluşturma **ReceiveMessageAsync**. Bu yöntem çağırarak iletiyi kuyruktan alır [GetMessageAsync](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.queue.cloudqueue.getmessageasync?view=azure-dotnet). İletinin başarıyla alındığında, birden çok kez işlenen olmayan şekilde kuyruktan silmek önemlidir. İleti alındıktan sonra çağırarak kuyruktan Sil [DeleteMessageAsync](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.queue.cloudqueue.deletemessageasync?view=azure-dotnet).

1. Aşağıdaki **ReceiveMessageAsync** yönteme, **Program** sınıfı.

   ```csharp
   static async Task<string> ReceiveMessageAsync(CloudQueue theQueue)
   {
       bool exists = await theQueue.ExistsAsync();

       if (exists)
       {
           CloudQueueMessage retrievedMessage = await theQueue.GetMessageAsync();

           if (retrievedMessage != null)
           {
               string theMessage = retrievedMessage.AsString;
               await theQueue.DeleteMessageAsync(retrievedMessage);
               return theMessage;
           }
       }
   }
   ```

2. Dosyayı kaydedin.

## <a name="delete-an-empty-queue"></a>Boş bir kuyruk silme

Oluşturduğunuz kaynaklara hala gerekip gerekmediğini belirlemek için bir Proje sonunda en iyi bir uygulamadır. Kaynakları sol çalışan can para maliyeti. Sıranın var ancak boş, kullanıcı bunlar silmek isteyip istemediğinizi sorar.

1. Genişletin **ReceiveMessageAsync** boş kuyruk silmek için bir istem dahil etmek için yöntemi.

   ```csharp
   static async Task<string> ReceiveMessageAsync(CloudQueue theQueue)
   {
       bool exists = await theQueue.ExistsAsync();

       if (exists)
       {
           CloudQueueMessage retrievedMessage = await theQueue.GetMessageAsync();

           if (retrievedMessage != null)
           {
               string theMessage = retrievedMessage.AsString;
               await theQueue.DeleteMessageAsync(retrievedMessage);
               return theMessage;
           }
           else
           {
               Console.Write("The queue is empty. Attempt to delete it? (Y/N) ");
               string response = Console.ReadLine();

               if (response == "Y" || response == "y")
               {
                   await theQueue.DeleteIfExistsAsync();
                   return "The queue was deleted.";
               }
               else
               {
                   return "The queue was not deleted.";
               }
           }
       }
       else
       {
           return "The queue does not exist. Add a message to the command line to create the queue and store the message.";
       }
   }
   ```

2. Dosyayı kaydedin.

## <a name="check-for-command-line-arguments"></a>Komut satırı bağımsız değişkenleri denetleyin

Uygulamaya geçirilen komut satırı bağımsız değişkenleri varsa, bir ileti kuyruğuna eklenmesini oldukları varsayılır. Birlikte bir dize olmak için bağımsız değişkenler katılın. Bu dize çağırarak iletiyi kuyruğa Ekle **SendMessageAsync** daha önce eklediğimiz yöntem.

Komut satırı bağımsız değişken varsa, bir Al işlemi yürütün. Çağrı **ReceiveMessageAsync** sıradaki ilk iletiyi almak için yöntemi.

Son olarak, çağırarak çıkmadan önce kullanıcı girişini bekleme **Console.ReadLine**.

1. Genişletin **ana** için komut satırı bağımsız değişkenleri denetleyin ve kullanıcı girişini bekleme için yöntem.

   ```csharp
        static async Task Main(string[] args)
        {
            CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);
            CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
            CloudQueue queue = queueClient.GetQueueReference("mystoragequeue");

            if (args.Length > 0)
            {
                string value = String.Join(" ", args);
                await SendMessageAsync(queue, value);
                Console.WriteLine($"Sent: {value}");
            }
            else
            {
                string value = await ReceiveMessageAsync(queue);
                Console.WriteLine($"Received: {value}");
            }

            Console.Write("Press Enter...");
            Console.ReadLine();
        }
   ```

2. Dosyayı kaydedin.

## <a name="complete-code"></a>Tam kod

Aşağıda, bu proje için tam kodu verilmiştir.

   ```csharp
   using System;
   using System.Threading.Tasks;
   using Microsoft.WindowsAzure.Storage;
   using Microsoft.WindowsAzure.Storage.Queue;

   namespace QueueApp
   {
    class Program
    {
        // The string value is broken up for better onscreen formatting
        private const string connectionString = "DefaultEndpointsProtocol=https;" +
                                                "AccountName=<your storage account name>;" +
                                                "AccountKey=<your key>;" +
                                                "EndpointSuffix=core.windows.net";

        static async Task Main(string[] args)
        {
            CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);
            CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
            CloudQueue queue = queueClient.GetQueueReference("mystoragequeue");

            if (args.Length > 0)
            {
                string value = String.Join(" ", args);
                await SendMessageAsync(queue, value);
                Console.WriteLine($"Sent: {value}");
            }
            else
            {
                string value = await ReceiveMessageAsync(queue);
                Console.WriteLine($"Received {value}");
            }

            Console.Write("Press Enter...");
            Console.ReadLine();
        }

        static async Task SendMessageAsync(CloudQueue theQueue, string newMessage)
        {
            bool createdQueue = await theQueue.CreateIfNotExistsAsync();

            if (createdQueue)
            {
                Console.WriteLine("The queue was created.");
            }

            CloudQueueMessage message = new CloudQueueMessage(newMessage);
            await theQueue.AddMessageAsync(message);
        }

        static async Task<string> ReceiveMessageAsync(CloudQueue theQueue)
        {
            bool exists = await theQueue.ExistsAsync();

            if (exists)
            {
                CloudQueueMessage retrievedMessage = await theQueue.GetMessageAsync();

                if (retrievedMessage != null)
                {
                    string theMessage = retrievedMessage.AsString;
                    await theQueue.DeleteMessageAsync(retrievedMessage);
                    return theMessage;
                }
                else
                {
                    Console.Write("The queue is empty. Attempt to delete it? (Y/N) ");
                    string response = Console.ReadLine();

                    if (response == "Y" || response == "y")
                    {
                        await theQueue.DeleteIfExistsAsync();
                        return "The queue was deleted.";
                    }
                    else
                    {
                        return "The queue was not deleted.";
                    }
                }
            }
            else
            {
                return "The queue does not exist. Add a message to the command line to create the queue and store the message.";
            }
        }
    }
   }
   ```

## <a name="build-and-run-the-app"></a>Uygulamayı derleme ve çalıştırma

1. Proje dizininde komut satırından Projeyi derlemek için aşağıdaki dotnet komutu çalıştırın.

   ```console
   dotnet build
   ```

2. Proje başarıyla oluşturduktan sonra ilk iletiyi kuyruğa eklemek için aşağıdaki komutu çalıştırın.

   ```console
   dotnet run First queue message
   ```

Bu bir çıktı görmeniz gerekir:

   ```output
   C:\Tutorials\QueueApp>dotnet run First queue message
   The queue was created.
   Sent: First queue message
   Press Enter..._
   ```

3. Uygulamayı alır ve ilk iletinin kuyrukta kaldırmak için komut satırı bağımsız değişken olmadan çalıştırın.

   ```console
   dotnet run
   ```

4. Tüm iletileri kaldırılana kadar uygulama çalışmaya devam eder. Daha fazla bir kez çalıştırırsanız, sıranın boş ve sıra silmek için bir istem olduğunu belirten bir ileti alırsınız.

   ```output
   C:\Tutorials\QueueApp>dotnet run First queue message
   The queue was created.
   Sent: First queue message
   Press Enter...

   C:\Tutorials\QueueApp>dotnet run Second queue message
   Sent: Second queue message
   Press Enter...

   C:\Tutorials\QueueApp>dotnet run Third queue message
   Sent: Third queue message
   Press Enter...

   C:\Tutorials\QueueApp>dotnet run
   Received: First queue message
   Press Enter...

   C:\Tutorials\QueueApp>dotnet run
   Received: Second queue message
   Press Enter...

   C:\Tutorials\QueueApp>dotnet run
   Received: Third queue message
   Press Enter...

   C:\Tutorials\QueueApp>dotnet run
   The queue is empty. Attempt to delete it? (Y/N) Y
   Received: The queue was deleted.
   Press Enter...

   C:\Tutorials\QueueApp>_
   ```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

1. Bir kuyruk oluşturma
2. Eklemek ve iletileri kuyruktan kaldırın
3. Azure depolama kuyruğu silin

Daha fazla bilgi için Azure kuyrukları hızlı göz atın.

> [!div class="nextstepaction"]
> [Kuyruklar hızlı başlangıç](storage-quickstart-queues-portal.md)
