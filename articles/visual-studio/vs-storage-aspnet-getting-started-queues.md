---
title: Azure kuyruk depolama ve Visual Studio bağlı Hizmetleri (ASP.NET) ile çalışmaya başlama | Microsoft Docs
description: Nasıl Visual Studio bağlı Hizmetler'i kullanarak bir depolama hesabına bağlandıktan sonra Visual Studio'da ASP.NET projesinde Azure kuyruk depolama kullanmaya başlama
services: storage
author: ghogen
manager: douge
ms.assetid: 94ca3413-5497-433f-abbe-836f83a9de72
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 12/23/2016
ms.author: ghogen
ms.openlocfilehash: 3f3a649308449a8ba4e1dbaebd4db4afbf731c5b
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62110494"
---
# <a name="get-started-with-azure-queue-storage-and-visual-studio-connected-services-aspnet"></a>Azure kuyruk depolama ve Visual Studio bağlı Hizmetleri (ASP.NET) kullanmaya başlama

[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Genel Bakış

Azure kuyruk depolama, uygulama bileşenleri arasında bulut Mesajlaşma sağlar. Ölçeklendirmek üzere uygulama tasarlarken, uygulama bileşenleri birbirinden bağımsız şekilde ölçeklenebilmek için genellikle birbirinden ayrılır. Kuyruk depolama bulutta, masaüstünde, şirket içi sunucuda veya mobil bir cihazda çalışan uygulama bileşenleri arasındaki iletişim için zaman uyumsuz mesajlaşma sunar. Kuyruk depolama ayrıca zaman uyumsuz görevlerin yönetilmesini ve süreç iş akışlarının oluşturulmasını destekler.

Bu öğretici, Azure kuyruk depolama varlıkları kullanarak bazı genel senaryolar için ASP.NET kodunun nasıl yazılacağını gösterir. Bu senaryolar, bir Azure kuyruk oluşturma ve ekleme, değiştirme, okuma ve kuyruk iletilerine kaldırma gibi ortak görevleri içerir.

## <a name="prerequisites"></a>Önkoşullar

* [Microsoft Visual Studio](https://www.visualstudio.com/downloads/)
* [Azure depolama hesabı](../storage/common/storage-quickstart-create-account.md)

[!INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/vs-storage-aspnet-getting-started-create-azure-account.md)]

[!INCLUDE [storage-development-environment-include](../../includes/vs-storage-aspnet-getting-started-setup-dev-env.md)]

### <a name="create-an-mvc-controller"></a>MVC denetleyicisi oluşturma 

1. İçinde **Çözüm Gezgini**, sağ **denetleyicileri**, bağlam menüsünden seçin **Ekle -> denetleyicisi**.

    ![ASP.NET MVC uygulaması için denetleyici ekleme](./media/vs-storage-aspnet-getting-started-queues/add-controller-menu.png)

1. Üzerinde **İskele Ekle** iletişim kutusunda **MVC 5 denetleyici - boş**seçip **Ekle**.

    ![MVC denetleyici türü belirtin](./media/vs-storage-aspnet-getting-started-queues/add-controller.png)

1. Üzerinde **denetleyici Ekle** iletişim kutusunda, denetleyici adı *QueuesController*seçip **Ekle**.

    ![MVC Denetleyici adı](./media/vs-storage-aspnet-getting-started-queues/add-controller-name.png)

1. Aşağıdaki *kullanarak* yönergelerini `QueuesController.cs` dosyası:

    ```csharp
    using Microsoft.Azure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Queue;
    ```
   ## <a name="create-a-queue"></a>Bir kuyruk oluşturma

Aşağıdaki adımlar, bir kuyruk oluşturulacağını göstermektedir:

> [!NOTE]
> 
> Bu bölümdeki adımları tamamladığınızdan varsayar [geliştirme ortamını ayarlama](#set-up-the-development-environment). 

1. `QueuesController.cs` dosyasını açın. 

1. Adlı bir yöntem ekleyin **CreateQueue** döndüren bir **actionresult öğesini**.

    ```csharp
    public ActionResult CreateQueue()
    {
        // The code in this section goes here.

        return View();
    }
    ```

1. İçinde **CreateQueue** yöntemi almak bir **CloudStorageAccount** depolama hesap bilgilerini temsil eden nesne. Azure hizmet yapılandırmasından bir depolama bağlantı dizesi ve depolama hesabı bilgileri almak için aşağıdaki kodu kullanın: (Değişiklik  *&lt;depolama hesabı adı >* erişmek için Azure depolama hesabı adı.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. Alma bir **CloudQueueClient** nesnesi bir kuyruk hizmeti istemcisi temsil eder.
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```
1. Alma bir **CloudQueue** istenen kuyruk adı bir başvuru temsil eden nesne. **CloudQueueClient.GetQueueReference** yöntemi, kuyruk depolama doğrulamasını yapmaz. Sıranın var olup olmadığını başvurusu döndürülür. 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. Çağrı **CloudQueue.CreateIfNotExists** kuyruk henüz yoksa, oluşturmak için yöntemi. **CloudQueue.CreateIfNotExists** yöntemi döndürür **true** kuyruk yok ve başarıyla oluşturuldu. Aksi takdirde, **false** döndürülür.    

    ```csharp
    ViewBag.Success = queue.CreateIfNotExists();
    ```

1. Güncelleştirme **ViewBag** ile Kuyruğun adı.

    ```csharp
    ViewBag.QueueName = queue.Name;
    ```

1. İçinde **Çözüm Gezgini**, genişletin **görünümleri** klasörüne sağ tıklayın **kuyrukları**ve bağlam menüsünden seçin **Ekle -> View**.

1. Üzerinde **Görünüm Ekle** iletişim kutusunda girin **CreateQueue** görünüm adını ve seçin için **Ekle**.

1. Açık `CreateQueue.cshtml`ve aşağıdaki kod parçacığı gibi görünüyor şekilde değiştirin:

    ```csharp
    @{
        ViewBag.Title = "Create Queue";
    }
    
    <h2>Create Queue results</h2>

    Creation of @ViewBag.QueueName @(ViewBag.Success == true ? "succeeded" : "failed")
    ```

1. İçinde **Çözüm Gezgini**, genişletme **görünümleri, paylaşılan ->** klasörü ve açık `_Layout.cshtml`.

1. En son **Html.ActionLink**, aşağıdaki **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Create queue", "CreateQueue", "Queues")</li>
    ```

1. Uygulamayı çalıştırmak ve seçmek **kuyruk Oluştur** aşağıdaki ekran görüntüsüne benzer bir sonuç görmek için:
  
    ![Kuyruk oluştur](./media/vs-storage-aspnet-getting-started-queues/create-queue-results.png)

    Daha önce de belirtildiği **CloudQueue.CreateIfNotExists** yöntemi döndürür **true** yalnızca kuyruk yok ve oluşturulur. Bu nedenle, sıranın mevcut olduğunda uygulama çalıştırırsanız, yöntem döndürür **false**. Uygulama birden çok kez çalıştırmak için uygulamayı yeniden çalıştırmadan önce sıranın silmeniz gerekir. Aracılığıyla bir sırayı silmek yapılabilir **CloudQueue.Delete** yöntemi. Kuyruk kullanarak silebilirsiniz [Azure portalında](https://go.microsoft.com/fwlink/p/?LinkID=525040) veya [Microsoft Azure Depolama Gezgini](../vs-azure-tools-storage-manage-with-storage-explorer.md).  

## <a name="add-a-message-to-a-queue"></a>Kuyruğa bir ileti ekleyin

Kaydederler [bir kuyruk oluşturan](#create-a-queue), iletileri bu kuyruğa ekleyebilirsiniz. Bu bölümde, bir kuyruğa bir ileti eklerken size kılavuzluk eder *test kuyruk*. 

> [!NOTE]
> 
> Bu bölümdeki adımları tamamladığınızdan varsayar [geliştirme ortamını ayarlama](#set-up-the-development-environment). 

1. `QueuesController.cs` dosyasını açın.

1. Adlı bir yöntem ekleyin **AddMessage** döndüren bir **actionresult öğesini**.

    ```csharp
    public ActionResult AddMessage()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. İçinde **AddMessage** yöntemi almak bir **CloudStorageAccount** depolama hesap bilgilerini temsil eden nesne. Azure hizmet yapılandırmasından bir depolama bağlantı dizesi ve depolama hesabı bilgileri almak için aşağıdaki kodu kullanın: (Değişiklik  *&lt;depolama hesabı adı >* erişmek için Azure depolama hesabı adı.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Alma bir **CloudQueueClient** nesnesi bir kuyruk hizmeti istemcisi temsil eder.
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. Alma bir **CloudQueueContainer** kuyruğuna başvuru temsil eden nesne. 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. Oluşturma **CloudQueueMessage** kuyruğa eklemek istediğiniz iletiyi temsil eden nesne. A **CloudQueueMessage** nesne oluşturulabilir, bir dizeden (UTF-8 biçiminde) veya bayt dizisi.

    ```csharp
    CloudQueueMessage message = new CloudQueueMessage("Hello, Azure Queue Storage");
    ```

1. Çağrı **CloudQueue.AddMessage** messaged kuyruğa eklemek için yöntemi.

    ```csharp
    queue.AddMessage(message);
    ```

1. Birkaç oluşturup **ViewBag** görünümünde görüntülenmesi için özellikleri.

    ```csharp
    ViewBag.QueueName = queue.Name;
    ViewBag.Message = message.AsString;
    ```

1. İçinde **Çözüm Gezgini**, genişletin **görünümleri** klasörüne sağ tıklayın **kuyrukları**ve bağlam menüsünden seçin **Ekle -> View**.

1. Üzerinde **Görünüm Ekle** iletişim kutusunda girin **AddMessage** görünüm adını ve seçin için **Ekle**.

1. Açık `AddMessage.cshtml`ve aşağıdaki kod parçacığı gibi görünüyor şekilde değiştirin:

    ```csharp
    @{
        ViewBag.Title = "Add Message";
    }
    
    <h2>Add Message results</h2>
    
    The message '@ViewBag.Message' was added to the queue '@ViewBag.QueueName'.
    ```

1. İçinde **Çözüm Gezgini**, genişletme **görünümleri, paylaşılan ->** klasörü ve açık `_Layout.cshtml`.

1. En son **Html.ActionLink**, aşağıdaki **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Add message", "AddMessage", "Queues")</li>
    ```

1. Uygulamayı çalıştırmak ve seçmek **ileti Ekle** aşağıdaki ekran görüntüsüne benzer bir sonuç görmek için:
  
    ![İleti Ekle](./media/vs-storage-aspnet-getting-started-queues/add-message-results.png)

İki bölüm - [iletiyi kuyruktan kaldırmadan okuma](#read-a-message-from-a-queue-without-removing-it) ve [okuyun ve iletiyi bir kuyruktan Kaldır](#read-and-remove-a-message-from-a-queue) -kuyruktan iletileri okumak işlemini göstermektedir.    

## <a name="read-a-message-from-a-queue-without-removing-it"></a>İletiyi kuyruktan kaldırmadan okuyun

Bu bölümde, (ilk ileti okuma kaldırmadan) kuyruğa alınan iletinin peek gösterilmektedir.  

> [!NOTE]
> 
> Bu bölümdeki adımları tamamladığınızdan varsayar [geliştirme ortamını ayarlama](#set-up-the-development-environment). 

1. `QueuesController.cs` dosyasını açın.

1. Adlı bir yöntem ekleyin **PeekMessage** döndüren bir **actionresult öğesini**.

    ```csharp
    public ActionResult PeekMessage()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. İçinde **PeekMessage** yöntemi almak bir **CloudStorageAccount** depolama hesap bilgilerini temsil eden nesne. Azure hizmet yapılandırmasından bir depolama bağlantı dizesi ve depolama hesabı bilgileri almak için aşağıdaki kodu kullanın: (Değişiklik  *&lt;depolama hesabı adı >* erişmek için Azure depolama hesabı adı.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Alma bir **CloudQueueClient** nesnesi bir kuyruk hizmeti istemcisi temsil eder.
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. Alma bir **CloudQueueContainer** kuyruğuna başvuru temsil eden nesne. 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. Çağrı **CloudQueue.PeekMessage** kuyruktan kaldırmadan kuyruktaki ilk iletinin okumak için yöntem. 

    ```csharp
    CloudQueueMessage message = queue.PeekMessage();
    ```

1. Güncelleştirme **ViewBag** iki değerlerle: sıra adı ve okundu ileti. **CloudQueueMessage** nesne, nesnenin değerini almak için iki özellik sunar: **CloudQueueMessage.AsBytes** ve **CloudQueueMessage.AsString**. **AsString** (Bu örnekte kullanılan) bir dize döndürür ancak **AsBytes** bir bayt dizisi döndürür.

    ```csharp
    ViewBag.QueueName = queue.Name; 
    ViewBag.Message = (message != null ? message.AsString : "");
    ```

1. İçinde **Çözüm Gezgini**, genişletin **görünümleri** klasörüne sağ tıklayın **kuyrukları**ve bağlam menüsünden seçin **Ekle -> View**.

1. Üzerinde **Görünüm Ekle** iletişim kutusunda girin **PeekMessage** görünüm adını ve seçin için **Ekle**.

1. Açık `PeekMessage.cshtml`ve aşağıdaki kod parçacığı gibi görünüyor şekilde değiştirin:

    ```csharp
    @{
        ViewBag.Title = "PeekMessage";
    }
    
    <h2>Peek Message results</h2>
    
    <table border="1">
        <tr><th>Queue</th><th>Peeked Message</th></tr>
        <tr><td>@ViewBag.QueueName</td><td>@ViewBag.Message</td></tr>
    </table>    
    ```

1. İçinde **Çözüm Gezgini**, genişletme **görünümleri, paylaşılan ->** klasörü ve açık `_Layout.cshtml`.

1. En son **Html.ActionLink**, aşağıdaki **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Peek message", "PeekMessage", "Queues")</li>
    ```

1. Uygulamayı çalıştırmak ve seçmek **Özet iletisi** aşağıdaki ekran görüntüsüne benzer bir sonuç görmek için:
  
    ![İletiye Gözat](./media/vs-storage-aspnet-getting-started-queues/peek-message-results.png)

## <a name="read-and-remove-a-message-from-a-queue"></a>Okuyun ve iletiyi bir kuyruktan kaldırın

Bu bölümde, okuma ve kuyruktan bir ileti kaldırma konusunda bilgi edinin.   

> [!NOTE]
> 
> Bu bölümdeki adımları tamamladığınızdan varsayar [geliştirme ortamını ayarlama](#set-up-the-development-environment). 

1. `QueuesController.cs` dosyasını açın.

1. Adlı bir yöntem ekleyin **ReadMessage** döndüren bir **actionresult öğesini**.

    ```csharp
    public ActionResult ReadMessage()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. İçinde **ReadMessage** yöntemi almak bir **CloudStorageAccount** depolama hesap bilgilerini temsil eden nesne. Azure hizmet yapılandırmasından bir depolama bağlantı dizesi ve depolama hesabı bilgileri almak için aşağıdaki kodu kullanın: (Değişiklik  *&lt;depolama hesabı adı >* erişmek için Azure depolama hesabı adı.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Alma bir **CloudQueueClient** nesnesi bir kuyruk hizmeti istemcisi temsil eder.
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. Alma bir **CloudQueueContainer** kuyruğuna başvuru temsil eden nesne. 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. Çağrı **CloudQueue.GetMessage** sıradaki ilk iletiye okumak için yöntem. **CloudQueue.GetMessage** hale getirdiğini ileti görünmez 30 saniye (varsayılan), böylece başka bir kod değiştirebilir veya ileti, onu işlenirken silebilirsiniz iletileri okuyan herhangi bir kod için. İleti görünmez süre miktarını değiştirmek için değiştirme **visibilityTimeout** parametresi için geçirilen **CloudQueue.GetMessage** yöntemi.

    ```csharp
    // This message will be invisible to other code for 30 seconds.
    CloudQueueMessage message = queue.GetMessage();     
    ```

1. Çağrı **CloudQueueMessage.Delete** iletiyi kuyruktan silmek için yöntemi.

    ```csharp
    queue.DeleteMessage(message);
    ```

1. Güncelleştirme **ViewBag** ileti silindi ve kuyruğun adı.

    ```csharp
    ViewBag.QueueName = queue.Name;
    ViewBag.Message = message.AsString;
    ```
 
1. İçinde **Çözüm Gezgini**, genişletin **görünümleri** klasörüne sağ tıklayın **kuyrukları**ve bağlam menüsünden seçin **Ekle -> View**.

1. Üzerinde **Görünüm Ekle** iletişim kutusunda girin **ReadMessage** görünüm adını ve seçin için **Ekle**.

1. Açık `ReadMessage.cshtml`ve aşağıdaki kod parçacığı gibi görünüyor şekilde değiştirin:

    ```csharp
    @{
        ViewBag.Title = "ReadMessage";
    }
    
    <h2>Read Message results</h2>
    
    <table border="1">
        <tr><th>Queue</th><th>Read (and Deleted) Message</th></tr>
        <tr><td>@ViewBag.QueueName</td><td>@ViewBag.Message</td></tr>
    </table>
    ```

1. İçinde **Çözüm Gezgini**, genişletme **görünümleri, paylaşılan ->** klasörü ve açık `_Layout.cshtml`.

1. En son **Html.ActionLink**, aşağıdaki **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Read/Delete message", "ReadMessage", "Queues")</li>
    ```

1. Uygulamayı çalıştırmak ve seçmek **okuma/silme iletisi** aşağıdaki ekran görüntüsüne benzer bir sonuç görmek için:
  
    ![İleti okuma ve silme](./media/vs-storage-aspnet-getting-started-queues/read-message-results.png)

## <a name="get-the-queue-length"></a>Kuyruk uzunluğu alma

Bu bölümde, kuyruk uzunluğu (ileti sayısı) almak nasıl gösterir. 

> [!NOTE]
> 
> Bu bölümdeki adımları tamamladığınızdan varsayar [geliştirme ortamını ayarlama](#set-up-the-development-environment). 

1. `QueuesController.cs` dosyasını açın.

1. Adlı bir yöntem ekleyin **GetQueueLength** döndüren bir **actionresult öğesini**.

    ```csharp
    public ActionResult GetQueueLength()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. İçinde **ReadMessage** yöntemi almak bir **CloudStorageAccount** depolama hesap bilgilerini temsil eden nesne. Azure hizmet yapılandırmasından bir depolama bağlantı dizesi ve depolama hesabı bilgileri almak için aşağıdaki kodu kullanın: (Değişiklik  *&lt;depolama hesabı adı >* erişmek için Azure depolama hesabı adı.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Alma bir **CloudQueueClient** nesnesi bir kuyruk hizmeti istemcisi temsil eder.
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. Alma bir **CloudQueueContainer** kuyruğuna başvuru temsil eden nesne. 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. Çağrı **CloudQueue.FetchAttributes** sıranın öznitelikleri (uzunluğu dahil) almak için yöntemi. 

    ```csharp
    queue.FetchAttributes();
    ```

1. Erişim **CloudQueue.ApproximateMessageCount** kuyruğun uzunluğu alınacağı özellik.
 
    ```csharp
    int? nMessages = queue.ApproximateMessageCount;
    ```

1. Güncelleştirme **ViewBag** kuyruk uzunluğunu ve ada sahip.

    ```csharp
    ViewBag.QueueName = queue.Name;
    ViewBag.Length = nMessages;
    ```
 
1. İçinde **Çözüm Gezgini**, genişletin **görünümleri** klasörüne sağ tıklayın **kuyrukları**ve bağlam menüsünden seçin **Ekle -> View**.

1. Üzerinde **Görünüm Ekle** iletişim kutusunda girin **GetQueueLength** görünüm adını ve seçin için **Ekle**.

1. Açık `GetQueueLengthMessage.cshtml`ve aşağıdaki kod parçacığı gibi görünüyor şekilde değiştirin:

    ```csharp
    @{
        ViewBag.Title = "GetQueueLength";
    }
    
    <h2>Get Queue Length results</h2>
    
    The queue '@ViewBag.QueueName' has a length of (number of messages): @ViewBag.Length
    ```

1. İçinde **Çözüm Gezgini**, genişletme **görünümleri, paylaşılan ->** klasörü ve açık `_Layout.cshtml`.

1. En son **Html.ActionLink**, aşağıdaki **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Get queue length", "GetQueueLength", "Queues")</li>
    ```

1. Uygulamayı çalıştırmak ve seçmek **kuyruk uzunluğu alma** aşağıdaki ekran görüntüsüne benzer bir sonuç görmek için:
  
    ![Kuyruk uzunluğu alma](./media/vs-storage-aspnet-getting-started-queues/get-queue-length-results.png)


## <a name="delete-a-queue"></a>Bir kuyruk silme
Bu bölümde, bir kuyruk silme gösterilmektedir. 

> [!NOTE]
> 
> Bu bölümdeki adımları tamamladığınızdan varsayar [geliştirme ortamını ayarlama](#set-up-the-development-environment). 

1. `QueuesController.cs` dosyasını açın.

1. Adlı bir yöntem ekleyin **DeleteQueue** döndüren bir **actionresult öğesini**.

    ```csharp
    public ActionResult DeleteQueue()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. İçinde **DeleteQueue** yöntemi almak bir **CloudStorageAccount** depolama hesap bilgilerini temsil eden nesne. Azure hizmet yapılandırmasından bir depolama bağlantı dizesi ve depolama hesabı bilgileri almak için aşağıdaki kodu kullanın: (Değişiklik  *&lt;depolama hesabı adı >* erişmek için Azure depolama hesabı adı.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Alma bir **CloudQueueClient** nesnesi bir kuyruk hizmeti istemcisi temsil eder.
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. Alma bir **CloudQueueContainer** kuyruğuna başvuru temsil eden nesne. 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. Çağrı **CloudQueue.Delete** yöntemi tarafından temsil edilen bir sırayı silmek için **CloudQueue** nesne.

    ```csharp
    queue.Delete();
    ```

1. Güncelleştirme **ViewBag** kuyruk uzunluğunu ve ada sahip.

    ```csharp
    ViewBag.QueueName = queue.Name;
    ```
 
1. İçinde **Çözüm Gezgini**, genişletin **görünümleri** klasörüne sağ tıklayın **kuyrukları**ve bağlam menüsünden seçin **Ekle -> View**.

1. Üzerinde **Görünüm Ekle** iletişim kutusunda girin **DeleteQueue** görünüm adını ve seçin için **Ekle**.

1. Açık `DeleteQueue.cshtml`ve aşağıdaki kod parçacığı gibi görünüyor şekilde değiştirin:

    ```csharp
    @{
        ViewBag.Title = "DeleteQueue";
    }
    
    <h2>Delete Queue results</h2>
    
    @ViewBag.QueueName deleted.
    ```

1. İçinde **Çözüm Gezgini**, genişletme **görünümleri, paylaşılan ->** klasörü ve açık `_Layout.cshtml`.

1. En son **Html.ActionLink**, aşağıdaki **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Delete queue", "DeleteQueue", "Queues")</li>
    ```

1. Uygulamayı çalıştırmak ve seçmek **kuyruk uzunluğu alma** aşağıdaki ekran görüntüsüne benzer bir sonuç görmek için:
  
    ![Kuyruğu silme](./media/vs-storage-aspnet-getting-started-queues/delete-queue-results.png)

## <a name="next-steps"></a>Sonraki adımlar
Azure’da veri depolama ile ilgili ek seçenekler hakkında daha fazla bilgi edinmek için daha fazla özellik kılavuzu görüntüleyin.

  * [Azure blob depolama ve Visual Studio bağlı Hizmetleri (ASP.NET) kullanmaya başlama](../storage/vs-storage-aspnet-getting-started-blobs.md)
  * [Azure tablo depolama ve Visual Studio bağlı Hizmetleri (ASP.NET) kullanmaya başlama](vs-storage-aspnet-getting-started-tables.md)
