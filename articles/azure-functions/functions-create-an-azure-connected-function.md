---
title: "Azure hizmetlerine bağlanan işlev oluşturma | Microsoft Docs"
description: "Diğer Azure hizmetlerine bağlanan sunucusuz bir uygulama oluşturmak için Azure İşlevleri’ni kullanın."
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
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 03/01/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 65964a322f0adab4f648fb350bedb77b46bf9054
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="use-azure-functions-to-create-a-function-that-connects-to-other-azure-services"></a>Diğer Azure hizmetlerine bağlanan bir işlev oluşturmak için Azure İşlevleri’ni kullanma

Bu konu başlığında, bir Azure Depolama Kuyruğundaki iletileri dinleyen ve iletileri bir Azure Depolama tablosundaki satırlara kopyalayan Azure İşlevleri içinde işlev oluşturma hakkında bilgi edineceksiniz. İletilerin kuyruğa yüklenmesi için zamanlayıcı ile tetiklenen bir işlev kullanılır. İkinci bir işlev ise iletileri kuyruktan okuyarak tabloya yazar. Hem kuyruk hem de tablo, bağlama tanımları temel alınarak Azure İşlevleri tarafından oluşturulur. 

İşleri daha ilginç hale getirmek için, bir işlev JavaScript dilinde yazılırken diğer işlev C# kodunda yazılır. Bu durum bir işlev uygulamasının nasıl çeşitli dillerde işlevleri olabileceğini gösterir. 

Bu senaryoyu [Kanal 9’daki bir videoda](https://channel9.msdn.com/Series/Windows-Azure-Web-Sites-Tutorials/Create-an-Azure-Function-which-binds-to-an-Azure-service/player) görebilirsiniz.

## <a name="create-a-function-that-writes-to-the-queue"></a>Kuyruğa yazan bir işlev oluşturma

Bir depolama kuyruğuna bağlanabilmeniz için ileti kuyruğunu yükleyen bir işlev oluşturmanız gerekir. Bu JavaScript işlevi, 10 saniyede bir kuyruğa ileti yazan zamanlama tetikleyicisini kullanır. Azure hesabınız yoksa [Azure İşlevleri Deneme](https://functions.azure.com/try) deneyimini inceleyin veya [ücretsiz Azure hesabınızı oluşturun](https://azure.microsoft.com/free/).

1. Azure portalına gidin ve işlev uygulamanızı bulun.

2. **Yeni İşlev** > **TimerTrigger-JavaScript** seçeneğine tıklayın. 

3. İşlevi **FunctionsBindingsDemo1** olarak adlandırın, **Zamanlama** için cron değeri olarak `0/10 * * * * *` girin ve ardından **Oluştur**’a tıklayın.
   
    ![Zamanlayıcı ile tetiklenen işlev ekleme](./media/functions-create-an-azure-connected-function/new-trigger-timer-function.png)

    10 saniyede bir çalışan zamanlayıcı tetiklemeli bir işlev oluşturdunuz.

5. **Geliştir** sekmesinde **Günlükler**’e tıklayın ve günlükteki etkinliği görüntüleyin. 10 saniyede bir yazılmış bir günlük girişi göreceksiniz.
   
    ![İşlevin çalıştığını doğrulamak için günlüğü görüntüleme](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-view-log.png)

## <a name="add-a-message-queue-output-binding"></a>İleti kuyruğu çıktı bağlaması ekleme

1. **Tümleştir** sekmesinde **Yeni Çıktı** > **Azure Kuyruk Depolama** > **Seç** seçeneğini belirtin.

    ![Tetikleyici zamanlayıcı işlevi ekleme](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-integrate-tab.png)

2. **İleti parametre adı** için `myQueueItem`, **Kuyruk adı** için `functions-bindings` girin, var olan bir **Depolama hesabı bağlantısı** seçin veya **yeni** seçeneğine tıklayarak bir depolama hesabı bağlantısı oluşturup **Kaydet**’e tıklayın.  

    ![Depolama kuyruğu için çıktı bağlama oluşturma](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-integrate-tab2.png)

1. **Geliştir** sekmesine geri dönerek aşağıdaki kodu işleve ekleyin:
   
    ```javascript
   
    function myQueueItem() 
    {
        return {
            msg: "some message goes here",
            time: "time goes here"
        }
    }
   
    ```
2. İşlevin 9. satırına yakın bir yerde bulunan *if* deyimini bulun ve bu deyimin sonuna aşağıdaki kodu ekleyin.
   
    ```javascript
   
    var toBeQed = myQueueItem();
    toBeQed.time = timeStamp;
    context.bindings.myQueueItem = toBeQed;
   
    ```  
   
    Bu kod bir **myQueueItem** oluşturur ve bu öğenin **zaman** özelliğini geçerli timeStamp olarak ayarlar. Daha sonra yeni kuyruk, öğesini bağlamın **myQueueItem** bağlamasına eklenir.

3. **Kaydet ve Çalıştır**’a tıklayın.

## <a name="view-storage-updates-by-using-storage-explorer"></a>Depolama Gezgini'ni kullanarak depolama güncelleştirmelerini görüntüleme
Oluşturduğunuz kuyruktaki iletileri görüntüleyerek, işlevinizin çalışıp çalışmadığını doğrulayabilirsiniz.  Visual Studio'daki Bulut Gezgini'ni kullanarak depolama kuyruğunuza bağlanabilirsiniz. Ancak, portal Microsoft Azure Depolama Gezgini’ni kullanarak depolama hesabınıza bağlanmayı kolaylaştırır.

1. **Tümleştir** sekmesinde kuyruk çıktı bağlaması > **Belgeler**’e tıklayın, ardından depolama hesabınıza ait Bağlantı Dizesini gösterin ve değeri kopyalayın. Depolama hesabınıza bağlanmak için bu değeri kullanın.

    ![Azure Depolama Gezgini’ni İndir](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-integrate-tab3.png)


2. Henüz yapmadıysanız [Microsoft Azure Depolama Gezgini](http://storageexplorer.com)’ni indirip yükleyin. 
 
3. Depolama Gezgini'nde, Azure Depolama’ya bağlan simgesine tıklayın, bağlantı dizesini alana yapıştırın ve sihirbazı tamamlayın.

    ![Depolama Gezgini ile bağlantı ekleme](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-storage-explorer.png)

4. **Yerel ve bağlı** altındaki **Depolama Hesapları** > depolama hesabınız > **Kuyruklar** > **functions-bindings** öğesini genişletin ve iletilerin kuyruğa yazıldığını doğrulayın.

    ![Kuyruktaki iletilerin görünümü](./media/functions-create-an-azure-connected-function/functionsbindings-azure-storage-explorer.png)

    Kuyruk yoksa veya boşsa, işlev bağlamanız veya kodunuz ile ilgili bir sorun olabilir.

## <a name="create-a-function-that-reads-from-the-queue"></a>Kuyruktan okuyan bir işlev oluşturma

İletileriniz artık kuyruğa eklendiğine göre, kuyruktan iletileri okuyan ve bir Azure Depolama tablosuna kalıcı olarak yazan başka bir işlev oluşturabilirsiniz.

1. **Yeni İşlev** > **QueueTrigger-CSharp** seçeneğine tıklayın. 
 
2. İşlevi `FunctionsBindingsDemo2` olarak adlandırın, **Kuyruk adı** alanına **functions-bindings** girin, var olan bir depolama hesabı seçin ya da bir tane oluşturun ve sonra **Oluştur**’a tıklayın.

    ![Çıkış kuyruğu zamanlayıcı işlevi ekleme](./media/functions-create-an-azure-connected-function/function-demo2-new-function.png) 

3. (İsteğe bağlı) Depolama Gezgini’nde önceki gibi yeni kuyruğu görüntüleyerek yeni işlevin çalıştığını doğrulayabilirsiniz. Ayrıca Visual Studio’daki Bulut Gezgini’ni kullanabilirsiniz.  

4. (İsteğe bağlı) **functions-bindings** kuyruğunu yenileyin ve öğelerin kuyruktan kaldırıldığına dikkat edin. İşlev **functions-bindings** kuyruğuna bir giriş tetikleyicisi olarak bağlı olduğundan ve işlev kuyruğu okuduğundan kaldırma gerçekleşir. 
 
## <a name="add-a-table-output-binding"></a>Tablo çıktı bağlaması ekleme

1. FunctionsBindingsDemo2 içinde **Tümleştir** > **Yeni Çıkış** > **Azure Tablo Depolama** > **Seç** seçeneklerine tıklayın.

    ![Bir Azure Depolama tablosuna bağlama ekleme](./media/functions-create-an-azure-connected-function/functionsbindingsdemo2-integrate-tab.png) 

2. **Tablo adı** için `functionbindings` ve **Tablo parametre adı** için `myTable` girin, bir **Depolama hesabı bağlantısı** seçin ya da yeni bir tane oluşturun ve ardından **Kaydet**’e tıklayın.

    ![Depolama tablo bağlamasını yapılandırma](./media/functions-create-an-azure-connected-function/functionsbindingsdemo2-integrate-tab2.png)
   
3. **Geliştir** sekmesinde var olan işlev kodunu aşağıdakiyle değiştirin:
   
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
        
        // Add the item to the table binding collection.
        myTable.Add(myItem);
    
        log.Verbose($"C# Queue trigger function processed: {myItem.RowKey} | {myItem.Msg} | {myItem.Time}");
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
    **TableItem** sınıfı, depolama sınıfındaki bir satırı temsil eder ve öğeyi **TableItem** nesnelerinin `myTable` koleksiyonuna eklemeniz gerekir. **PartitionKey** ve **RowKey** özelliklerini tabloya eklemek için ayarlamanız gerekir.

4. **Kaydet** düğmesine tıklayın.  Son olarak, Depolama gezginindeki tabloyu veya Visual Studio Bulut Gezgini'ni görüntüleyerek işlevin çalıştığını doğrulayabilirsiniz.

5. (İsteğe bağlı) Depolama Gezgini'ndeki depolama hesabınızda **Tablolar** > **functionsbindings** öğesini genişletin ve satırların tabloya eklendiğini doğrulayın. Aynı işlemi Visual Studio’daki Bulut Gezgini’nde de yapabilirsiniz.

    ![Tablodaki satırların görünümü](./media/functions-create-an-azure-connected-function/functionsbindings-azure-storage-explorer2.png)

    Tablo yoksa veya boşsa, işlev bağlamanız veya kodunuz ile ilgili bir sorun olabilir. 
 
[!INCLUDE [More binding information](../../includes/functions-bindings-next-steps.md)]

## <a name="next-steps"></a>Sonraki adımlar
Azure İşlevleri hakkında daha fazla bilgi edinmek için, aşağıdaki konulara bakın:

* [Azure İşlevleri geliştirici başvurusu](functions-reference.md)  
  İşlevleri kodlamak ve tetikleyicileri ve bağlamaları tanımlamak için programcı başvurusu
* [Azure İşlevlerini test etme](functions-test-a-function.md)  
  İşlevlerinizi test etmek için çeşitli araçları ve teknikleri açıklar.
* [Azure İşlevlerini ölçeklendirme](functions-scale.md)  
  Tüketim barındırma planı dahil olmak üzere, Azure İşlevlerinde kullanılabilen hizmet planlarını ve doğru planın nasıl seçileceğini açıklar. 

[!INCLUDE [Getting help note](../../includes/functions-get-help.md)]

