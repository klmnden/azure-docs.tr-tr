<properties
   pageTitle="Olay işleme işlevi oluşturma | Microsoft Azure"
   description="Bir olay zamanlayıcısını temel alarak çalışan bir C# bir işlevi oluşturmak için Azure İşlevlerini kullanın."
   services="functions"
   documentationCenter="na"
   authors="ggailey777"
   manager="erikre"
   editor=""
   tags=""
   />

<tags
   ms.service="functions"
   ms.devlang="multiple"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="09/25/2016"
   ms.author="glenga"/>
   

# Olay işleyen bir Azure İşlevi oluşturma

Azure İşlevleri, çeşitli programlama dillerinde uygulanan zamanlanan veya tetiklenen kod birimleri oluşturmanızı sağlayan olay odaklı, isteğe bağlı bir hesaplama deneyimidir. Azure İşlevleri hakkında daha fazla bilgi edinmek için bkz. [Azure İşlevlerine Genel Bakış](functions-overview.md).

Bu konuda bir depolama kuyruğuna iletiler eklemek amacıyla bir olay zamanlayıcısını temel alarak çalışan yeni bir C# işlevi oluşturma işlemi gösterilmektedir. 

## Ön koşullar 

Bir işlev oluşturmadan önce etkin bir Azure hesabınız olması gerekir. Bir Azure hesabınız yoksa [ücretsiz hesaplar kullanılabilir](https://azure.microsoft.com/free/).

## Şablondan zamanlayıcı ile tetiklenen bir işlev oluşturma

Azure'da işlevlerinizin yürütülmesini bir işlev uygulaması barındırır. Bir işlev oluşturmadan önce etkin bir Azure hesabınız olması gerekir. Bir Azure hesabınız yoksa [ücretsiz hesaplar kullanılabilir](https://azure.microsoft.com/free/). 

1. [Azure İşlevleri portalına](https://functions.azure.com/signin) gidin ve Azure hesabınız ile oturum açın.

2. Kullanılacak bir işlev uygulamanız varsa **İşlev uygulamalarınızdan** seçin ve ardından **Aç**’a tıklayın. Yeni bir işlev uygulaması oluşturmak için yeni işlev uygulamanıza ait benzersiz bir **Ad** yazın veya oluşturulan adı kabul edin, tercih ettiğiniz **Bölge** seçeneğini belirleyin ve ardından **Oluştur + başla** düğmesine tıklayın. 

3. İşlev uygulamanızda **+ Yeni İşlev** > **TimerTrigger - C#** > **Oluştur** öğesine tıklayın. Bunun yapılması dakikada bir kez olan varsayılan zamanlama ile çalışan varsayılan bir ad oluşturur. 

    ![Zamanlayıcı ile tetiklenen yeni bir işlev oluşturma](./media/functions-create-an-event-processing-function/functions-create-new-timer-trigger.png)

4. Yeni işlevinizde **Tümleştir** sekmesi > **Yeni Çıktı** > **Azure Depolama Kuyruğu** > **Seç** öğesine tıklayın.

    ![Zamanlayıcı ile tetiklenen yeni bir işlev oluşturma](./media/functions-create-an-event-processing-function/functions-create-storage-queue-output-binding.png)

5. **Azure Storage Kuyruğu çıktısında** var olan bir **Storage hesabı bağlantısını** seçin veya yeni bir tane oluşturun, ardından **Kaydet**’e tıklayın. 

    ![Zamanlayıcı ile tetiklenen yeni bir işlev oluşturma](./media/functions-create-an-event-processing-function/functions-create-storage-queue-output-binding-2.png)

6. **Geliştir** sekmesine geri dönerek **Kod** penceresinde var olan C# betiğini aşağıdaki kodla değiştirin:

        using System;
        
        public static void Run(TimerInfo myTimer, out string outputQueueItem, TraceWriter log)
        {
            // Add a new scheduled message to the queue.
            outputQueueItem = $"Ping message added to the queue at: {DateTime.Now}.";
            
            // Also write the message to the logs.
            log.Info(outputQueueItem);
        }

    Bu kod, işlevin yürütüldüğü geçerli tarih ve saat ile birlikte kuyruğa yeni bir ileti ekler.

7. **Kaydet**’e tıklayın ve sonraki işlev yürütmesi için **Günlükler** penceresini izleyin.

8. (İsteğe bağlı) Depolama hesabına gidin ve iletilerin kuyruğa eklenmekte olduğunu doğrulayın.

9. **Tümleştir** sekmesine geri gidin ve zamanlama alanını `0 0 * * * *` olarak değiştirin. İşlev bundan sonra saatte bir kez çalışır. 

Bu işlem bir zamanlayıcı tetikleyici ile depolama kuyruğu çıkış bağlamanın çok basitleştirilmiş bir örneğidir. Daha fazla bilgi için [Azure İşlevleri zamanlayıcı tetikleyicisi](functions-bindings-timer.md) ve [Azure Storage için Azure İşlevleri tetikleyicileri ve bağlamaları](functions-bindings-storage.md) konularına bakın.

##Sonraki adımlar

Azure İşlevleri hakkında daha fazla bilgi edinmek için bu konulara göz atın.

+ [Azure İşlevleri geliştirici başvurusu](functions-reference.md)  
İşlevleri kodlamak ve tetikleyicileri ve bağlamaları tanımlamak için programcı başvurusu
+ [Azure İşlevlerini test etme](functions-test-a-function.md)  
İşlevlerinizi test etmek için çeşitli araçları ve teknikleri açıklar.
+ [Azure İşlevlerini ölçeklendirme](functions-scale.md)  
Dinamik hizmet planı dahil olmak üzere Azure İşlevlerinde kullanılabilen hizmet planlarını ve doğru planın nasıl seçileceğini açıklar.  

[AZURE.INCLUDE [Getting Started Note](../../includes/functions-get-help.md)]



<!--HONumber=Sep16_HO4-->


