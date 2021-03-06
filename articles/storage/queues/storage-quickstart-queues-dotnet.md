---
title: "Hızlı Başlangıç: Azure Depolama'da bir kuyruk oluşturmak için .NET kullanma"
description: Bu hızlı başlangıçta, .NET için Azure depolama istemci kitaplığı bir kuyruk oluşturun ve iletileri eklemek için nasıl kullanılacağını öğrenin. Ardından, okuma ve kuyruktan iletileri işleme hakkında bilgi edinin.
services: storage
author: mhopkins-msft
ms.custom: mvc
ms.service: storage
ms.topic: quickstart
ms.date: 02/06/2018
ms.author: mhopkins
ms.reviewer: cbrooks
ms.openlocfilehash: c3743c62dcbdccc2a119cfec570df96c622390c7
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67540324"
---
# <a name="quickstart-use-net-to-create-a-queue-in-azure-storage"></a>Hızlı Başlangıç: Azure Depolama'da bir kuyruk oluşturmak için .NET kullanma

Bu hızlı başlangıçta, .NET için Azure depolama istemci kitaplığı bir kuyruk oluşturun ve iletileri eklemek için nasıl kullanılacağını öğrenin. Ardından, okuma ve kuyruktan iletileri işleme hakkında bilgi edinin. 

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [storage-quickstart-prereq-include](../../../includes/storage-quickstart-prereq-include.md)]

Ardından, işletim sisteminiz için .NET Core 2.0’ı indirip yükleyin. Windows kullanıyorsanız, isterseniz Visual Studio yükleyip .NET Framework kullanabilirsiniz. Ayrıca, işletim sisteminizle birlikte kullanmak için bir düzenleyici yüklemeyi seçebilirsiniz.

### <a name="windows"></a>Windows

- [Windows için .NET Core](https://www.microsoft.com/net/download/windows) veya [.NET Framework](https://www.microsoft.com/net/download/windows) (Windows için Visual Studio ile birlikte gelir) yükleyin
- [Windows için Visual Studio](https://www.visualstudio.com/) yükleyin. .NET Core kullanıyorsanız, Visual Studio yüklenmesi isteğe bağlıdır.  

.NET Core ve .NET Framework arasında seçim yapma hakkında bilgi için bkz: [Sunucu uygulamaları için .NET Framework ve .NET Core arasında seçim yapma](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server).

### <a name="linux"></a>Linux

- [Linux için .NET Core](https://www.microsoft.com/net/download/linux) yükleyin
- İsteğe bağlı olarak [Visual Studio Code](https://www.visualstudio.com/) ve [C# uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp&dotnetid=963890049.1518206068) yükleyin

### <a name="macos"></a>Mac OS

- [macOS için .NET Core](https://www.microsoft.com/net/download/macos) yükleyin.
- İsteğe bağlı olarak [Mac için Visual Studio](https://www.visualstudio.com/vs/visual-studio-mac/) yükleyin

## <a name="download-the-sample-application"></a>Örnek uygulamayı indirin:

Bu hızlı başlangıçta kullanılan örnek uygulama, temel bir konsol uygulamasıdır. [GitHub](https://github.com/Azure-Samples/storage-queues-dotnet-quickstart) üzerindeki örnek uygulamayı inceleyebilirsiniz.

Uygulamanın bir kopyasını geliştirme ortamınıza indirmek için [Git](https://git-scm.com/)'i kullanın. 

```bash
git clone https://github.com/Azure-Samples/storage-queues-dotnet-quickstart.git
```

Bu komut, depoyu yerel Git klasörünüze kopyalar. Visual Studio çözümünü açmak için nelere *depolama-kuyruk-dotnet-quickstart* klasörünü açın ve çift tıklayarak *depolama-kuyruk-dotnet-quickstart.sln*. 

[!INCLUDE [storage-copy-connection-string-portal](../../../includes/storage-copy-connection-string-portal.md)]

## <a name="configure-your-storage-connection-string"></a>Depolama bağlantı dizelerinizi yapılandırma

Uygulamayı çalıştırmak istiyorsanız, depolama hesabınız için bağlantı dizesi sağlamanız gerekir. Örnek uygulama, bağlantı dizesini ortam değişkeninden okur ve Azure Depolama’ya yönelik istekleri yetkilendirmek için bu dizeyi kullanır.

Bağlantı dizenizi kopyaladıktan sonra uygulamayı çalıştıran yerel makine üzerindeki yeni bir ortam değişkenine yazın. Ortam değişkenini ayarlamak için bir konsol penceresi açın ve işletim sisteminizin yönergelerini izleyin. `<yourconnectionstring>` değerini gerçek bağlantı dizenizle değiştirin:

### <a name="windows"></a>Windows

```cmd
setx storageconnectionstring "<yourconnectionstring>"
```

Ortam değişkenini ekledikten sonra, konsol penceresi de dahil olmak üzere ortam değişkenini okumak için gereken tüm çalışan programları yeniden başlatmanız gerekebilir. Örneğin, düzenleyici olarak Visual Studio kullanıyorsanız, örneği çalıştırmadan önce Visual Studio’yu yeniden başlatın. 

### <a name="linux"></a>Linux

```bash
export storageconnectionstring=<yourconnectionstring>
```

Ortam değişkenini ekledikten sonra değişiklikleri uygulamak için konsol pencerenizden `source ~/.bashrc` çalıştırın.

### <a name="macos"></a>Mac OS

.bash_profile dosyanızı düzenleyin ve ortam değişkenini ekleyin:

```bash
export STORAGE_CONNECTION_STRING=<yourconnectionstring>
```

Ortam değişkenini ekledikten sonra değişiklikleri uygulamak için konsol pencerenizden `source .bash_profile` çalıştırın.

## <a name="run-the-sample"></a>Örneği çalıştırma

Örnek uygulama bir kuyruk oluşturur ve bir ileti ekler. Uygulama ilk iletiyi kuyruktan kaldırmadan göz atar. ardından iletiyi alır ve kuyruktan siler.

### <a name="windows"></a>Windows

Düzenleyici olarak Visual Studio kullanıyorsanız **F5** tuşuna basarak çalıştırın. 

Aksi takdirde uygulama dizininize gidip `dotnet run` komutuyla uygulamayı çalıştırın.

```
dotnet run
```

### <a name="linux"></a>Linux

Uygulama dizininize gidip `dotnet run` komutuyla uygulamayı çalıştırın.

```
dotnet run
```

### <a name="macos"></a>Mac OS

Uygulama dizininize gidip `dotnet run` komutuyla uygulamayı çalıştırın.

```
dotnet run
```

Örnek uygulama çıktısı aşağıdaki örneğe benzer:

```
Azure Queues - .NET Quickstart sample

Created queue 'quickstartqueues-3136fe9a-fa52-4b19-a447-8999a847da52'

Added message 'aa8fa95f-07ea-4df7-bf86-82b3f7debfb7' to queue 'quickstartqueues-3136fe9a-fa52-4b19-a447-8999a847da52'
Message insertion time: 2/7/2019 4:30:46 AM +00:00
Message expiration time: 2/14/2019 4:30:46 AM +00:00

Contents of peeked message 'aa8fa95f-07ea-4df7-bf86-82b3f7debfb7': Hello, World

Message 'aa8fa95f-07ea-4df7-bf86-82b3f7debfb7' becomes visible again at 2/7/2019 4:31:16 AM +00:00

Processed and deleted message 'aa8fa95f-07ea-4df7-bf86-82b3f7debfb7'

Press any key to delete the sample queue.
```

## <a name="understand-the-sample-code"></a>Örnek kodu anlama

Ardından, nasıl çalıştığını anlayabilmeniz için örnek kodu inceleyin.

### <a name="try-parsing-the-connection-string"></a>Bağlantı dizesini ayrıştırmayı deneyin

Örnek, ilk ortam değişkenini oluşturmak üzere ayrıştırılabilecek bir bağlantı dizesi içerip içermediğini denetler bir [CloudStorageAccount](/dotnet/api/microsoft.azure.cosmos.table.cloudstorageaccount) depolama hesabına işaret eden bir nesne. Bağlantı dizesinin geçerli olup olmadığını denetlemek için örnek kullanır [TryParse](/dotnet/api/microsoft.azure.cosmos.table.cloudstorageaccount.tryparse) yöntemi. **TryParse** başarılı olursa *storageAccount* değişkenini başlatır ve **true** değerini döndürür.

```csharp
// Retrieve the connection string for use with the application. The storage connection string is stored
// in an environment variable called storageconnectionstring, on the machine where the application is running.
// If the environment variable is created after the application is launched in a console or with Visual
// Studio, the shell needs to be closed and reloaded to take the environment variable into account.
string storageConnectionString = Environment.GetEnvironmentVariable("storageconnectionstring");

// Check whether the connection string can be parsed.
if (CloudStorageAccount.TryParse(storageConnectionString, out storageAccount))
{
    // If the connection string is valid, proceed with calls to Azure Queues here.
    ...    
}
else
{
    Console.WriteLine(
        "A connection string has not been defined in the system environment variables. " +
        "Add an environment variable named 'storageconnectionstring' with your storage " +
        "connection string as a value.");
}
```

### <a name="create-the-queue"></a>Kuyruk oluşturma

İlk olarak, örnek, bir kuyruk oluşturur ve bir ileti ekler. 

```csharp
// Create a queue called 'quickstartqueues' and append a GUID value so that the queue name 
// is unique in your storage account. 
queue = cloudQueueClient.GetQueueReference("quickstartqueues-" + Guid.NewGuid().ToString());
await queue.CreateAsync();

Console.WriteLine("Created queue '{0}'", queue.Name);
Console.WriteLine();
```

### <a name="add-a-message"></a>Bir ileti ekleyin

Ardından, örnek bir ileti sırası geri ekler. 

Bir ileti, UTF-8 kodlamalı bir XML isteği eklenebilir ve boyutu 64 KB'ye kadar olabilir bir biçimde olmalıdır. Bir ileti ikili veri içeriyorsa, öneririz, Base64 kodlama ileti.

Varsayılan olarak, en fazla süre için bir ileti yaşam 7 gün olarak ayarlanır. İletinin yaşam süresi pozitif bir sayı olarak belirtebilirsiniz.

```csharp
// Create a message and add it to the queue. Set expiration time to 14 days.
CloudQueueMessage message = new CloudQueueMessage("Hello, World");
await queue.AddMessageAsync(message, new TimeSpan(14,0,0,0), null, null, null);
Console.WriteLine("Added message '{0}' to queue '{1}'", message.Id, queue.Name);
Console.WriteLine("Message insertion time: {0}", message.InsertionTime.ToString());
Console.WriteLine("Message expiration time: {0}", message.ExpirationTime.ToString());
Console.WriteLine();
```

Dolmayan iletiye eklemek için `Timespan.FromSeconds(-1)` , çağrıda [AddMessageAsync](/dotnet/api/microsoft.azure.storage.queue.cloudqueue.addmessageasync).

```csharp
await queue.AddMessageAsync(message, TimeSpan.FromSeconds(-1), null, null, null);
```

### <a name="peek-a-message-from-the-queue"></a>Sıradan bir iletiye Gözat

Örnek, bir kuyruktan bir ileti Özet işlemi gösterilmektedir. Bir iletiye, ileti içeriğini okuyabilir. Ancak, başka bir istemci, daha sonra almak ve iletiyi ileti diğer istemcilere görünür kalır.

```csharp
// Peek at the message at the front of the queue. Peeking does not alter the message's 
// visibility, so that another client can still retrieve and process it. 
CloudQueueMessage peekedMessage = await queue.PeekMessageAsync();

// Display the ID and contents of the peeked message.
Console.WriteLine("Contents of peeked message '{0}': {1}", peekedMessage.Id, peekedMessage.AsString);
Console.WriteLine();
```

### <a name="dequeue-a-message"></a>Bir iletiyi sıradan çıkar

Örnek ayrıca, bir iletiyi sıradan çıkar işlemini de gösterir. Bir iletiyi sıradan çıkar, ileti kuyruğu önünden almak ve diğer istemciler için geçici olarak görünmez işleme. Varsayılan olarak, bir ileti 30 saniye görünmez kalır. Bu süre boyunca, kodunuzu ileti işleyebilir. Dequeueing iletiyi tamamlamak için böylece başka bir istemci aynı iletiyi sıradan çıkar değil hemen işlendikten sonra iletiyi silin.

Kodunuzun bir iletiyi bir donanım veya yazılım hatası nedeniyle başarısız olursa, görünmezlik süresi geçene sonra sonra ileti yeniden görünür hale gelir. Başka bir istemci aynı ileti alabilir ve yeniden deneyin.

```csharp
// Retrieve the message at the front of the queue. The message becomes invisible for 
// a specified interval, during which the client attempts to process it.
CloudQueueMessage retrievedMessage = await queue.GetMessageAsync();

// Display the time at which the message will become visible again if it is not deleted.
Console.WriteLine("Message '{0}' becomes visible again at {1}", retrievedMessage.Id, retrievedMessage.NextVisibleTime);
Console.WriteLine();

//Process and delete the message within the period of invisibility.
await queue.DeleteMessageAsync(retrievedMessage);
Console.WriteLine("Processed and deleted message '{0}'", retrievedMessage.Id);
Console.WriteLine();
```

### <a name="clean-up-resources"></a>Kaynakları temizleme

Örnek sıranın silerek oluşturduğu kaynakları temizler. Sıra silme, içerdiği tüm iletileri siler.

```csharp
Console.WriteLine("Press any key to delete the sample queue.");
Console.ReadLine();
Console.WriteLine("Deleting the queue and any messages it contains...");
Console.WriteLine();
if (queue != null)
{
    await queue.DeleteIfExistsAsync();
}
```

## <a name="resources-for-developing-net-applications-with-queues"></a>Kuyruklar ile .NET uygulamaları geliştirme kaynakları

Azure kuyrukları ile .NET geliştirmeye yönelik şu ek kaynaklara bakın:

### <a name="binaries-and-source-code"></a>İkili dosyalar ve kaynak kodu

- En son sürümüne yönelik NuGet paketlerini indirin [.NET için Azure depolama istemci kitaplığı](/dotnet/api/overview/azure/storage/client)
    - [Ortak](https://www.nuget.org/packages/Microsoft.Azure.Storage.Common/)
    - [Kuyruklar](https://www.nuget.org/packages/Azure.Storage.Queues/)
- GitHub üzerinde [.NET istemci kitaplığı kaynak kodunu](https://github.com/Azure/azure-storage-net) görüntüleyin.

### <a name="client-library-reference-and-samples"></a>İstemci kitaplığı başvurusu ve örnekleri

- .NET istemci kitaplığı hakkında daha fazla bilgi için bkz. [.NET API başvurusu](https://docs.microsoft.com/dotnet/api/overview/azure/storage).
- Keşfedin [kuyruk depolama örnekleri](https://azure.microsoft.com/resources/samples/?sort=0&service=storage&platform=dotnet&term=queues) .NET istemci kitaplığı kullanılarak yazılmış.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir kuyruktan gözlem ileti bir kuyruğa ileti eklemek ve sıradan çıkarmaya ve .NET kullanarak ileti işleme öğrendiniz. 

> [!div class="nextstepaction"]
> [Azure kuyruk depolama ile uygulamalar arasında iletişim](https://docs.microsoft.com/learn/modules/communicate-between-apps-with-azure-queue-storage/index)

- .NET Core hakkında daha fazla bilgi için bkz. [10 dakika içinde .NET kullanmaya başlama](https://www.microsoft.com/net/learn/get-started/).
