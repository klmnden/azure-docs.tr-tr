---
title: "Azure kuyruk depolama ve Visual Studio bağlı Hizmetleri (ASP.NET) kullanmaya başlama | Microsoft Docs"
description: "Visual Studio bağlantılı hizmetler kullanarak bir depolama hesabı bağlandıktan sonra Visual Studio'da ASP.NET projesinde Azure kuyruk depolama kullanarak nereden başlayacaksınız"
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: 94ca3413-5497-433f-abbe-836f83a9de72
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/23/2016
ms.author: kraigb
ms.openlocfilehash: 4687e5dfce72583728068c176d86d100313badf6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-azure-queue-storage-and-visual-studio-connected-services-aspnet"></a>Azure kuyruk depolama ve Visual Studio bağlı Hizmetleri (ASP.NET) kullanmaya başlama
[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Genel Bakış

Azure kuyruk depolama uygulama bileşenleri arasında Mesajlaşma bulut sağlar. Ölçeklendirmek üzere uygulama tasarlarken, uygulama bileşenleri birbirinden bağımsız şekilde ölçeklenebilmek için genellikle birbirinden ayrılır. Kuyruk depolama bulutta, masaüstünde, şirket içi sunucuda veya mobil bir cihazda çalışan uygulama bileşenleri arasındaki iletişim için zaman uyumsuz mesajlaşma sunar. Kuyruk depolama ayrıca zaman uyumsuz görevlerin yönetilmesini ve süreç iş akışlarının oluşturulmasını destekler.

Bu öğretici Azure kuyruk depolama varlıkları kullanarak bazı genel senaryolar için ASP.NET kodunun nasıl yazılacağını gösterir. Bu senaryolar, bir Azure kuyruk oluşturma ve ekleme, değiştirme, okuma ve iletileri kuyruğa kaldırma gibi genel görevleri içerir.

##<a name="prerequisites"></a>Ön koşullar

* [Microsoft Visual Studio](https://www.visualstudio.com/downloads/)
* [Azure depolama hesabı](../storage/common/storage-create-storage-account.md#create-a-storage-account)

[!INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/vs-storage-aspnet-getting-started-create-azure-account.md)]

[!INCLUDE [storage-development-environment-include](../../includes/vs-storage-aspnet-getting-started-setup-dev-env.md)]

### <a name="create-an-mvc-controller"></a>Bir MVC denetleyicisi oluşturun. 

1. İçinde **Çözüm Gezgini**, sağ **denetleyicileri**ve bağlam menüsünden seçin **Ekle -> denetleyicisi**.

    ![Bir ASP.NET MVC uygulamasına denetleyici ekleme](./media/vs-storage-aspnet-getting-started-queues/add-controller-menu.png)

1. Üzerinde **İskele Ekle** iletişim kutusunda **MVC 5 denetleyici - boş**seçip **Ekle**.

    ![MVC denetleyicisi türünü belirtin](./media/vs-storage-aspnet-getting-started-queues/add-controller.png)

1. Üzerinde **denetleyici Ekle** iletişim kutusunda, denetleyici adı *QueuesController*seçip **Ekle**.

    ![MVC Denetleyici adı](./media/vs-storage-aspnet-getting-started-queues/add-controller-name.png)

1. Aşağıdakileri ekleyin *kullanarak* yönergeleri `QueuesController.cs` dosyası:

    ```csharp
    using Microsoft.Azure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Queue;
    ```
## <a name="create-a-queue"></a>Bir kuyruk oluşturma

Aşağıdaki adımlar bir sıranın nasıl oluşturulacağını gösterir:

> [!NOTE]
> 
> Bu bölümdeki adımları tamamladığınızdan varsayar [geliştirme ortamını ayarlama](#set-up-the-development-environment). 

1. `QueuesController.cs` dosyasını açın. 

1. Adlı bir yöntem ekleyin **CreateQueue** döndüren bir **ActionResult**.

    ```csharp
    public ActionResult CreateQueue()
    {
        // The code in this section goes here.

        return View();
    }
    ```

1. İçinde **CreateQueue** yöntemi, get bir **CloudStorageAccount** depolama hesabı bilgilerini temsil eden nesne. Azure hizmet yapılandırmasından depolama bağlantı dizesi ve depolama hesabı bilgilerini almak için aşağıdaki kodu kullanın: (değişiklik  *&lt;depolama hesabı adı >* Azure depolama hesabı adını eriştiğiniz.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. Alma bir **CloudQueueClient** nesnesi, bir kuyruk hizmeti istemcisi temsil eder.
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```
1. Alma bir **CloudQueue** istenen sıra adı için bir başvuru temsil eden nesne. **CloudQueueClient.GetQueueReference** yöntemi kuyruk depolama doğrulamasını yapmaz. Sıranın var olup olmadığına bakılmaksızın başvuru döndürülür. 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. Çağrı **CloudQueue.CreateIfNotExists** henüz yoksa, kuyruk oluşturmak için yöntem. **CloudQueue.CreateIfNotExists** yöntemi döndürür **true** sıranın var olmadığından ve başarıyla oluşturuldu. Aksi takdirde, **false** döndürülür.    

    ```csharp
    ViewBag.Success = queue.CreateIfNotExists();
    ```

1. Güncelleştirme **ViewBag** sıra adı.

    ```csharp
    ViewBag.QueueName = queue.Name;
    ```

1. İçinde **Çözüm Gezgini**, genişletin **görünümleri** klasörünü sağ tıklatın **sıraları**ve bağlam menüsünden seçin **Ekle -> Görünüm**.

1. Üzerinde **Görünüm Ekle** iletişim kutusunda, girin **CreateQueue** Görünüm adı ' nı seçip için **Ekle**.

1. Açık `CreateQueue.cshtml`ve aşağıdaki kod parçacığını gibi görünüyor şekilde değiştirin:

    ```csharp
    @{
        ViewBag.Title = "Create Queue";
    }
    
    <h2>Create Queue results</h2>

    Creation of @ViewBag.QueueName @(ViewBag.Success == true ? "succeeded" : "failed")
    ```

1. İçinde **Çözüm Gezgini**, genişletin **görünümleri -> paylaşılan** klasörü ve açık `_Layout.cshtml`.

1. Son sonra **Html.ActionLink**, aşağıdakileri ekleyin **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Create queue", "CreateQueue", "Queues")</li>
    ```

1. Uygulamayı çalıştırmak ve seçmek **Oluşturma sırası** aşağıdaki ekran görüntüsüne benzer sonuçlar görmek için:
  
    ![Kuyruk oluşturma](./media/vs-storage-aspnet-getting-started-queues/create-queue-results.png)

    Daha önce belirtildiği gibi **CloudQueue.CreateIfNotExists** yöntemi döndürür **doğru** yalnızca sıranın yok ve oluşturulur. Bu nedenle, sıranın mevcut olduğunda uygulama çalıştırırsanız, yöntem döndürür **false**. Birden çok kez uygulamayı çalıştırmak için uygulamayı yeniden çalıştırmadan önce sıranın silmeniz gerekir. Aracılığıyla sıra silme yapılabilir **CloudQueue.Delete** yöntemi. Kuyruğu kullanarak silebilirsiniz [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) veya [Microsoft Azure Storage Gezgini](../vs-azure-tools-storage-manage-with-storage-explorer.md).  

## <a name="add-a-message-to-a-queue"></a>Kuyruğa bir ileti Ekle

Seçtiğiniz sonra [bir kuyruk oluşturan](#create-a-queue), bu kuyruğa iletileri ekleyebilirsiniz. Bu bölümde, bir sıraya bir ileti eklerken size yol gösterir *sınama sırası*. 

> [!NOTE]
> 
> Bu bölümdeki adımları tamamladığınızdan varsayar [geliştirme ortamını ayarlama](#set-up-the-development-environment). 

1. `QueuesController.cs` dosyasını açın.

1. Adlı bir yöntem ekleyin **AddMessage** döndüren bir **ActionResult**.

    ```csharp
    public ActionResult AddMessage()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. İçinde **AddMessage** yöntemi, get bir **CloudStorageAccount** depolama hesabı bilgilerini temsil eden nesne. Azure hizmet yapılandırmasından depolama bağlantı dizesi ve depolama hesabı bilgilerini almak için aşağıdaki kodu kullanın: (değişiklik  *&lt;depolama hesabı adı >* Azure depolama hesabı adını eriştiğiniz.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Alma bir **CloudQueueClient** nesnesi, bir kuyruk hizmeti istemcisi temsil eder.
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. Alma bir **CloudQueueContainer** sıranın başvuru temsil eden nesne. 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. Oluşturma **CloudQueueMessage** sıraya eklemek istediğiniz iletiyi temsil eden nesne. A **CloudQueueMessage** bir dizeden (UTF-8 biçiminde) veya bir bayt dizisi nesne oluşturulabilir.

    ```csharp
    CloudQueueMessage message = new CloudQueueMessage("Hello, Azure Queue Storage");
    ```

1. Çağrı **CloudQueue.AddMessage** messaged sıraya eklemek için yöntem.

    ```csharp
    queue.AddMessage(message);
    ```

1. Birkaç oluşturup **ViewBag** görünümü görüntülemek özelliklerini.

    ```csharp
    ViewBag.QueueName = queue.Name;
    ViewBag.Message = message.AsString;
    ```

1. İçinde **Çözüm Gezgini**, genişletin **görünümleri** klasörünü sağ tıklatın **sıraları**ve bağlam menüsünden seçin **Ekle -> Görünüm**.

1. Üzerinde **Görünüm Ekle** iletişim kutusunda, girin **AddMessage** Görünüm adı ' nı seçip için **Ekle**.

1. Açık `AddMessage.cshtml`ve aşağıdaki kod parçacığını gibi görünüyor şekilde değiştirin:

    ```csharp
    @{
        ViewBag.Title = "Add Message";
    }
    
    <h2>Add Message results</h2>
    
    The message '@ViewBag.Message' was added to the queue '@ViewBag.QueueName'.
    ```

1. İçinde **Çözüm Gezgini**, genişletin **görünümleri -> paylaşılan** klasörü ve açık `_Layout.cshtml`.

1. Son sonra **Html.ActionLink**, aşağıdakileri ekleyin **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Add message", "AddMessage", "Queues")</li>
    ```

1. Uygulamayı çalıştırmak ve seçmek **Ekle ileti** aşağıdaki ekran görüntüsüne benzer sonuçlar görmek için:
  
    ![Mesaj ekleyin.](./media/vs-storage-aspnet-getting-started-queues/add-message-results.png)

İki bölüm - [kaldırmadan bir sıradan ileti okumak](#read-a-message-from-a-queue-without-removing-it) ve [ekleme ve kaldırma bir iletiyi bir kuyruktan okunur](#read-and-remove-a-message-from-a-queue) -kuyruktan iletileri okumak nasıl gösterilmektedir.    

## <a name="read-a-message-from-a-queue-without-removing-it"></a>Bir ileti kuyruktan kaldırmadan okuma

Bu bölümde, (ilk iletiyi kaldırmadan okuma) kuyruğa alınan iletinin peek göstermektedir.  

> [!NOTE]
> 
> Bu bölümdeki adımları tamamladığınızdan varsayar [geliştirme ortamını ayarlama](#set-up-the-development-environment). 

1. `QueuesController.cs` dosyasını açın.

1. Adlı bir yöntem ekleyin **PeekMessage** döndüren bir **ActionResult**.

    ```csharp
    public ActionResult PeekMessage()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. İçinde **PeekMessage** yöntemi, get bir **CloudStorageAccount** depolama hesabı bilgilerini temsil eden nesne. Azure hizmet yapılandırmasından depolama bağlantı dizesi ve depolama hesabı bilgilerini almak için aşağıdaki kodu kullanın: (değişiklik  *&lt;depolama hesabı adı >* Azure depolama hesabı adını eriştiğiniz.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Alma bir **CloudQueueClient** nesnesi, bir kuyruk hizmeti istemcisi temsil eder.
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. Alma bir **CloudQueueContainer** sıranın başvuru temsil eden nesne. 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. Çağrı **CloudQueue.PeekMessage** sıradaki ilk iletiyi sıradan kaldırarak olmadan okumak için yöntem. 

    ```csharp
    CloudQueueMessage message = queue.PeekMessage();
    ```

1. Güncelleştirme **ViewBag** iki değerlerle: kuyruk adı ve okundu ileti. **CloudQueueMessage** nesne nesnenin değeri almak için iki özellik sunar: **CloudQueueMessage.AsBytes** ve **CloudQueueMessage.AsString**. **AsString** (Bu örnekte kullanılan) bir dize döndürür sırada **AsBytes** bir bayt dizisi döndürür.

    ```csharp
    ViewBag.QueueName = queue.Name; 
    ViewBag.Message = (message != null ? message.AsString : "");
    ```

1. İçinde **Çözüm Gezgini**, genişletin **görünümleri** klasörünü sağ tıklatın **sıraları**ve bağlam menüsünden seçin **Ekle -> Görünüm**.

1. Üzerinde **Görünüm Ekle** iletişim kutusunda, girin **PeekMessage** Görünüm adı ' nı seçip için **Ekle**.

1. Açık `PeekMessage.cshtml`ve aşağıdaki kod parçacığını gibi görünüyor şekilde değiştirin:

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

1. İçinde **Çözüm Gezgini**, genişletin **görünümleri -> paylaşılan** klasörü ve açık `_Layout.cshtml`.

1. Son sonra **Html.ActionLink**, aşağıdakileri ekleyin **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Peek message", "PeekMessage", "Queues")</li>
    ```

1. Uygulamayı çalıştırmak ve seçmek **gözlem ileti** aşağıdaki ekran görüntüsüne benzer sonuçlar görmek için:
  
    ![İletiye Gözat](./media/vs-storage-aspnet-getting-started-queues/peek-message-results.png)

## <a name="read-and-remove-a-message-from-a-queue"></a>Okuma ve bir ileti kuyruktan kaldırma

Bu bölümde, okuma ve bir ileti kuyruktan kaldırma öğrenin.   

> [!NOTE]
> 
> Bu bölümdeki adımları tamamladığınızdan varsayar [geliştirme ortamını ayarlama](#set-up-the-development-environment). 

1. `QueuesController.cs` dosyasını açın.

1. Adlı bir yöntem ekleyin **ReadMessage** döndüren bir **ActionResult**.

    ```csharp
    public ActionResult ReadMessage()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. İçinde **ReadMessage** yöntemi, get bir **CloudStorageAccount** depolama hesabı bilgilerini temsil eden nesne. Azure hizmet yapılandırmasından depolama bağlantı dizesi ve depolama hesabı bilgilerini almak için aşağıdaki kodu kullanın: (değişiklik  *&lt;depolama hesabı adı >* Azure depolama hesabı adını eriştiğiniz.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Alma bir **CloudQueueClient** nesnesi, bir kuyruk hizmeti istemcisi temsil eder.
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. Alma bir **CloudQueueContainer** sıranın başvuru temsil eden nesne. 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. Çağrı **CloudQueue.GetMessage** sıradaki ilk iletiyi okumak için yöntem. **CloudQueue.GetMessage** yöntemi yapar ileti görünmez başka bir kod değiştirmek veya ileti, onu işlenirken silmek böylece iletileri okuyan herhangi bir kod için 30 saniye (varsayılan). İleti görünmez süre miktarını değiştirmek için değiştirmek **visibilityTimeout** için geçirilen parametre **CloudQueue.GetMessage** yöntemi.

    ```csharp
    // This message will be invisible to other code for 30 seconds.
    CloudQueueMessage message = queue.GetMessage();     
    ```

1. Çağrı **CloudQueueMessage.Delete** iletiyi sıradan silmek için yöntem.

    ```csharp
    queue.DeleteMessage(message);
    ```

1. Güncelleştirme **ViewBag** silinmiş ileti ve sıra adı.

    ```csharp
    ViewBag.QueueName = queue.Name;
    ViewBag.Message = message.AsString;
    ```
 
1. İçinde **Çözüm Gezgini**, genişletin **görünümleri** klasörünü sağ tıklatın **sıraları**ve bağlam menüsünden seçin **Ekle -> Görünüm**.

1. Üzerinde **Görünüm Ekle** iletişim kutusunda, girin **ReadMessage** Görünüm adı ' nı seçip için **Ekle**.

1. Açık `ReadMessage.cshtml`ve aşağıdaki kod parçacığını gibi görünüyor şekilde değiştirin:

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

1. İçinde **Çözüm Gezgini**, genişletin **görünümleri -> paylaşılan** klasörü ve açık `_Layout.cshtml`.

1. Son sonra **Html.ActionLink**, aşağıdakileri ekleyin **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Read/Delete message", "ReadMessage", "Queues")</li>
    ```

1. Uygulamayı çalıştırmak ve seçmek **okuma/silme iletisi** aşağıdaki ekran görüntüsüne benzer sonuçlar görmek için:
  
    ![Okuma ve silme iletisi](./media/vs-storage-aspnet-getting-started-queues/read-message-results.png)

## <a name="get-the-queue-length"></a>Kuyruk uzunluğu alma

Bu bölümde, kuyruk uzunluğu (iletilerinin sayısı) alma gösterilmektedir. 

> [!NOTE]
> 
> Bu bölümdeki adımları tamamladığınızdan varsayar [geliştirme ortamını ayarlama](#set-up-the-development-environment). 

1. `QueuesController.cs` dosyasını açın.

1. Adlı bir yöntem ekleyin **GetQueueLength** döndüren bir **ActionResult**.

    ```csharp
    public ActionResult GetQueueLength()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. İçinde **ReadMessage** yöntemi, get bir **CloudStorageAccount** depolama hesabı bilgilerini temsil eden nesne. Azure hizmet yapılandırmasından depolama bağlantı dizesi ve depolama hesabı bilgilerini almak için aşağıdaki kodu kullanın: (değişiklik  *&lt;depolama hesabı adı >* Azure depolama hesabı adını eriştiğiniz.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Alma bir **CloudQueueClient** nesnesi, bir kuyruk hizmeti istemcisi temsil eder.
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. Alma bir **CloudQueueContainer** sıranın başvuru temsil eden nesne. 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. Çağrı **CloudQueue.FetchAttributes** (uzunluğu dahil) kuyruğun öznitelikleri alma yöntemi. 

    ```csharp
    queue.FetchAttributes();
    ```

6. Erişim **CloudQueue.ApproximateMessageCount** sıra uzunluğu alınacağı özellik.
 
    ```csharp
    int? nMessages = queue.ApproximateMessageCount;
    ```

1. Güncelleştirme **ViewBag** sırası uzunluğu ve ada sahip.

    ```csharp
    ViewBag.QueueName = queue.Name;
    ViewBag.Length = nMessages;
    ```
 
1. İçinde **Çözüm Gezgini**, genişletin **görünümleri** klasörünü sağ tıklatın **sıraları**ve bağlam menüsünden seçin **Ekle -> Görünüm**.

1. Üzerinde **Görünüm Ekle** iletişim kutusunda, girin **GetQueueLength** Görünüm adı ' nı seçip için **Ekle**.

1. Açık `GetQueueLengthMessage.cshtml`ve aşağıdaki kod parçacığını gibi görünüyor şekilde değiştirin:

    ```csharp
    @{
        ViewBag.Title = "GetQueueLength";
    }
    
    <h2>Get Queue Length results</h2>
    
    The queue '@ViewBag.QueueName' has a length of (number of messages): @ViewBag.Length
    ```

1. İçinde **Çözüm Gezgini**, genişletin **görünümleri -> paylaşılan** klasörü ve açık `_Layout.cshtml`.

1. Son sonra **Html.ActionLink**, aşağıdakileri ekleyin **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Get queue length", "GetQueueLength", "Queues")</li>
    ```

1. Uygulamayı çalıştırmak ve seçmek **alma sırası uzunluğu** aşağıdaki ekran görüntüsüne benzer sonuçlar görmek için:
  
    ![Kuyruk uzunluğu alma](./media/vs-storage-aspnet-getting-started-queues/get-queue-length-results.png)


## <a name="delete-a-queue"></a>Bir kuyruk silme
Bu bölümde, bir kuyruk silme göstermektedir. 

> [!NOTE]
> 
> Bu bölümdeki adımları tamamladığınızdan varsayar [geliştirme ortamını ayarlama](#set-up-the-development-environment). 

1. `QueuesController.cs` dosyasını açın.

1. Adlı bir yöntem ekleyin **DeleteQueue** döndüren bir **ActionResult**.

    ```csharp
    public ActionResult DeleteQueue()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. İçinde **DeleteQueue** yöntemi, get bir **CloudStorageAccount** depolama hesabı bilgilerini temsil eden nesne. Azure hizmet yapılandırmasından depolama bağlantı dizesi ve depolama hesabı bilgilerini almak için aşağıdaki kodu kullanın: (değişiklik  *&lt;depolama hesabı adı >* Azure depolama hesabı adını eriştiğiniz.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Alma bir **CloudQueueClient** nesnesi, bir kuyruk hizmeti istemcisi temsil eder.
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. Alma bir **CloudQueueContainer** sıranın başvuru temsil eden nesne. 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. Çağrı **CloudQueue.Delete** yöntemi tarafından temsil edilen sıra silmek için **CloudQueue** nesnesi.

    ```csharp
    queue.Delete();
    ```

1. Güncelleştirme **ViewBag** sırası uzunluğu ve ada sahip.

    ```csharp
    ViewBag.QueueName = queue.Name;
    ```
 
1. İçinde **Çözüm Gezgini**, genişletin **görünümleri** klasörünü sağ tıklatın **sıraları**ve bağlam menüsünden seçin **Ekle -> Görünüm**.

1. Üzerinde **Görünüm Ekle** iletişim kutusunda, girin **DeleteQueue** Görünüm adı ' nı seçip için **Ekle**.

1. Açık `DeleteQueue.cshtml`ve aşağıdaki kod parçacığını gibi görünüyor şekilde değiştirin:

    ```csharp
    @{
        ViewBag.Title = "DeleteQueue";
    }
    
    <h2>Delete Queue results</h2>
    
    @ViewBag.QueueName deleted.
    ```

1. İçinde **Çözüm Gezgini**, genişletin **görünümleri -> paylaşılan** klasörü ve açık `_Layout.cshtml`.

1. Son sonra **Html.ActionLink**, aşağıdakileri ekleyin **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Delete queue", "DeleteQueue", "Queues")</li>
    ```

1. Uygulamayı çalıştırmak ve seçmek **alma sırası uzunluğu** aşağıdaki ekran görüntüsüne benzer sonuçlar görmek için:
  
    ![Kuyruğu silin](./media/vs-storage-aspnet-getting-started-queues/delete-queue-results.png)

## <a name="next-steps"></a>Sonraki adımlar
Azure’da veri depolama ile ilgili ek seçenekler hakkında daha fazla bilgi edinmek için daha fazla özellik kılavuzu görüntüleyin.

  * [Azure blob depolama ve Visual Studio bağlı Hizmetleri (ASP.NET) kullanmaya başlama](../storage/vs-storage-aspnet-getting-started-blobs.md)
  * [Azure tablo depolaması ve Visual Studio bağlı Hizmetleri (ASP.NET) kullanmaya başlama](vs-storage-aspnet-getting-started-tables.md)
