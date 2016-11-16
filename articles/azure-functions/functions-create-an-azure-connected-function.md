---
title: "Bir Azure hizmetine bağlanan Azure İşlevi oluşturma | Microsoft Belgeleri"
description: "Sunucusuz bir uygulama olup diğer Azure Hizmetleri ile etkileşim kuran bir Azure İşlevi oluşturun."
services: functions
documentationcenter: dev-center-name
author: yochay
manager: manager-alias
editor: 
tags: 
keywords: "azure işlevleri, işlevler, olay işleme, web kancaları, dinamik işlem, sunucusuz mimari"
ms.assetid: ab86065d-6050-46c9-a336-1bfc1fa4b5a1
ms.service: functions
ms.devlang: multiple
ms.topic: get-started-article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/25/2016
ms.author: rachelap@microsoft.com
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 9ffd199c9e3c621a808ade109ed044b6c9b689b7


---
# <a name="create-an-azure-function-which-binds-to-an-azure-service"></a>Bir Azure hizmetine bağlanan Azure İşlevi oluşturma
[!INCLUDE [Getting Started Note](../../includes/functions-getting-started.md)]

Bu kısa videoda bir Azure Kuyruğundaki iletileri dinleyen ve iletileri bir Azure Blob’a kopyalayan Azure İşlevi oluşturma hakkında bilgi edineceksiniz.

## <a name="watch-the-video"></a>Videoyu izleme
>[!VIDEO https://channel9.msdn.com/Series/Windows-Azure-Web-Sites-Tutorials/Create-an-Azure-Function-which-binds-to-an-Azure-service/player]
>
>

## <a name="create-an-input-queue-trigger-function"></a>Giriş kuyruğu tetikleme işlevi oluşturma
Bu işlevin amacı, bir kuyruğa her 10 saniyede bir ileti yazmaktır. Bunu gerçekleştirmek için işlevi ve ileti kuyruklarını oluşturmalı ve yeni oluşturulan kuyruklara ileti yazan kodu eklemelisiniz.

1. Azure Portal’a gidip Azure İşlev Uygulamanızı bulun.
2. **Yeni İşlev** > **TimerTrigger - Düğüm**’e tıklayın. İşlevi **FunctionsBindingsDemo1** olarak adlandırın
3. Zamanlama için "0/10 * * * * *" değerini girin. Bu değer bir cron ifadesi biçimindedir. Bu, zamanlayıcıyı 10 saniyede bir çalışacak şekilde zamanlar.
4. İşlevi profili oluşturmak için **Oluştur** düğmesine tıklayın.
   
    ![Tetikleyici zamanlayıcı işlevleri ekleme](./media/functions-create-an-azure-connected-function/new-trigger-timer-function.png)
5. Günlükteki etkinlikleri görüntüleyerek işlevin çalıştığını doğrulayın. Günlük bölmesini görüntülemek için sağ üst köşedeki **Günlükler** bağlantısına tıklamanız gerekebilir.
   
   ![Günlüğü görüntüleyerek işlevin çalıştığını doğrulayın](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-view-log.png)

### <a name="add-a-message-queue"></a>İleti kuyruğu ekleme
1. **Tümleştir** sekmesine gidin.
2. **Yeni Çıkış** > **Azure Depolama Kuyruğu** > **Seç**’i seçin.
3. **İleti parametresi adı** metin kutusuna **myQueueItem** yazın.
4. Bir depolama hesabı seçin veya mevcut depolama hesabınız yoksa **Yeni**’ye tıklayarak bir depolama hesabı oluşturun.
5. **Kuyruk adı** metin kutusuna **functions-bindings** yazın.
6. **Kaydet** düğmesine tıklayın.  
   
   ![Tetikleyici zamanlayıcı işlevleri ekleme](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-integrate-tab.png)

### <a name="write-to-the-message-queue"></a>İleti kuyruğuna yazma
1. **Geliştir** sekmesine dönün ve aşağıdaki kodu mevcut koddan sonra gelecek şekilde işleve ekleyin:
   
    ```javascript
   
    function myQueueItem() 
      {
        return {
        msg: "some message goes here",
        time: "time goes here"
      }
    }
   
    ```
2. Mevcut işlev kodunu 1. Adım’da eklenen kodu çağıracak şekilde değiştirin. İşlevin 9. satırına yakın bir yerde bulunan *if* deyiminden sonra aşağıdaki kodu ekleyin.
   
    ```javascript
   
    var toBeQed = myQueueItem();
    toBeQed.time = timeStamp;
    context.bindings.myQueue = toBeQed;
   
    ```
   
    Bu kod bir **myQueueItem** oluşturur ve bu öğenin **zaman** özelliğini geçerli timeStamp olarak ayarlar. Daha sonra yeni kuyruk öğesini bağlamın myQueue bağlamasına ekler.
3. **Kaydet ve Çalıştır**’a tıklayın.
4. Visual Studio’da kuyruğu görüntüleyerek kodun çalıştığını doğrulayın.
   
   * Visual Studio’yu açın ve **Görünüm** > **Cloud** **Explorer**’a gidin.
   * myQueue kuyruğunu oluştururken depolama hesabını ve kullandığınız **functions-bindings** kuyruğunu bulun. Satırlar halinde günlük verileri görmelisiniz. Visual Studio üzerinden Azure’da oturum açmanız gerekebilir.  

## <a name="create-an-output-queue-trigger-function"></a>Çıkış kuyruğu tetikleme işlevi oluşturma
1. **Yeni İşlev** > **QueueTrigger - C#** seçeneğine tıklayın. İşlevi **FunctionsBindingsDemo2** olarak adlandırın. Aynı işlev uygulamasında farklı diller (bu durumda Node ve C#) kullanabileceğinizi unutmayın.
2. **Kuyruk adı** alanına **functions-bindings** yazın.
3. Kullanmak üzere bir depolama hesabı seçin veya yeni hesap oluşturun.
4. **Oluştur**'a tıklayın
5. Hem işlevin günlüğünü hem de Visual Studio’yu görüntüleyip güncelleştirme olup olmadığına bakarak yeni işlevin çalıştığını doğrulayın. İşlevin günlüğü, işlevin çalıştığını ve öğelerin kuyruktan çıkarıldığını gösterir. İşlev bir giriş tetikleyicisi olarak **functions-bindings** çıkış kuyruğuna bağlı olduğundan, Visual Studio’daki **functions-bindings** Kuyruğu öğelerin kaldırıldığını göstermelidir. Kuyruktan çıkarıldılar.   
   
   ![Çıkış kuyruğu zamanlayıcısı işlevleri ekleme](./media/functions-create-an-azure-connected-function/functionsbindingsdemo2-integrate-tab.png)   

### <a name="modify-the-queue-item-type-from-json-to-object"></a>JSON olan kuyruk öğe türünü nesne olarak değiştirme
1. **FunctionsBindingsDemo2**’deki kodu aşağıdaki kodla değiştirin:    
   
    ```cs
   
    using System;
   
    public static void Run(QItem myQueueItem, ICollector<TableItem> myTable, TraceWriter log)
    {
      TableItem myItem = new TableItem
      {
        PartitionKey = "key",
        RowKey = Guid.NewGuid().ToString(),
        Time = DateTime.Now.ToString("hh.mm.ss.ffffff"),
        Msg = myQueueItem.Msg,
        OriginalTime = myQueueItem.Time    
      };
      log.Verbose($"C# Queue trigger function processed: {myQueueItem.Msg} | {myQueueItem.Time}");
    }
   
    public class TableItem
    {
      public string PartitionKey {get; set;}
      public string RowKey {get; set;}
      public string Time {get; set;}
      public string Msg {get; set;}
      public string OriginalTime {get; set;}
    }
   
    public class QItem
    {
      public string Msg { get; set;}
      public string Time { get; set;}
    }
   
    ```
   
    Bu kod, kuyrukları okumak ve kuyruklara yazmak için kullandığınız **TableItem** ve **QItem** şeklinde iki sınıf içerir. Ayrıca, **Çalıştır** işlevi bir **string** ve **TraceWriter** yerine **QItem** ve **TraceWriter**’ı kabul edecek şekilde değiştirildi. 
2. **Kaydet** düğmesine tıklayın.
3. Günlüğü denetleyerek kodun çalıştığını doğrulayın. Azure işlevlerinin nesneyi sizin yerinize otomatik olarak seri duruma getirdiğini ve seri durumdan çıkardığına, böylece kuyruğa verilerin etrafından dolaşarak nesne odaklı bir biçimde erişmeyi kolaylaştırdığına dikkat edin. 

## <a name="store-messages-in-an-azure-table"></a>İletileri Azure Tablosunda depolama
Kuyrukların birlikte çalışmasını sağladığınıza göre artık kuyruk verilerinin kalıcı olarak depolanması için bir Azure tablosu ekleme zamanı geldi.

1. **Tümleştir** sekmesine gidin.
2. Çıkış için bir Azure Depolama Tablosu oluşturun ve bu tabloyu **myTable** olarak adlandırın.
3. "Veriler hangi tabloya yazılmalı?" sorusunu **functionsbindings** olarak yanıtlayın.
4. **{project-id}** olan **PartitionKey** ayarını **{partition}** olarak değiştirin.
5. Bir depolama hesabı seçin veya yeni hesap oluşturun.
6. **Kaydet** düğmesine tıklayın.
7. **Geliştir** sekmesine gidin.
8. Bir Azure tablosunu temsil edecek bir **TableItem** sınıfı oluşturun ve Çalıştır işlevini yeni oluşturulan TableItem öğesini kabul edecek şekilde değiştirin. Bunun çalışabilmesi için **PartitionKey** ve **RowKey** özelliklerini kullanmanız gerektiğine dikkat edin.
   
    ```cs
   
    public static void Run(QItem myQueueItem, ICollector<TableItem> myTable, TraceWriter log)
    {    
      TableItem myItem = new TableItem
      {
        PartitionKey = "key",
        RowKey = Guid.NewGuid().ToString(),
        Time = DateTime.Now.ToString("hh.mm.ss.ffffff"),
        Msg = myQueueItem.Msg,
        OriginalTime = myQueueItem.Time    
      };
   
      log.Verbose($"C# Queue trigger function processed: {myQueueItem.RowKey} | {myQueueItem.Msg} | {myQueueItem.Time}");
    }
   
    public class TableItem
    {
      public string PartitionKey {get; set;}
      public string RowKey {get; set;}
      public string Time {get; set;}
      public string Msg {get; set;}
      public string OriginalTime {get; set;}
    }
    ```
9. **Kaydet** düğmesine tıklayın.
10. Visual Studio’da işlevin günlüklerini görüntüleyerek kodun çalıştığını doğrulayın. Visual Studio’da doğrulama gerçekleştirmek için **Cloud Explorer**’ı kullanarak **functionbindings** Azure Tablosuna gidin ve tablonun satırlar içerdiğini doğrulayın.

[!INCLUDE [Getting Started Note](../../includes/functions-bindings-next-steps.md)]

[!INCLUDE [Getting Started Note](../../includes/functions-get-help.md)]




<!--HONumber=Nov16_HO2-->


