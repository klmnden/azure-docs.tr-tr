---
title: "Öğretici: Azure Stream Analytics işleri ile Azure işlevleri'ni çalıştırma | Microsoft Docs"
description: Bu öğreticide, Stream Analytics işlerine bir çıktı havuzu olarak Azure İşlevlerini yapılandırma hakkında bilgi edineceksiniz.
services: stream-analytics
author: mamccrea
ms.service: stream-analytics
ms.topic: tutorial
ms.custom: mvc
ms.workload: data-services
ms.date: 06/05/2019
ms.author: mamccrea
ms.reviewer: jasonh
ms.openlocfilehash: 5aa2616bfbfd4b31d3e5e5aeee71da8fd511faed
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67066749"
---
# <a name="tutorial-run-azure-functions-from-azure-stream-analytics-jobs"></a>Öğretici: Azure Stream Analytics işlerinden Azure İşlevleri’ni çalıştırma 

İşlevleri Stream Analytics işinin çıktı havuzlarından biri olarak yapılandırarak, Azure Stream Analytics'ten Azure İşlevleri’ni çalıştırabilirsiniz. İşlevler, Azure veya üçüncü taraf hizmetlerinde gerçekleşen olayların tetiklediği kodu uygulamanıza olanak tanıyan olay odaklı, isteğe bağlı bir işlem deneyimidir. İşlevlerin tetikleyicilere yanıt vermeye yönelik bu özelliği, hizmeti Stream Analytics işleri için doğal bir çıktı haline getirmektedir.

Stream Analytics, HTTP tetikleyicileri aracılığıyla İşlevleri çağırır. İşlevlerin çıkış bağdaştırıcısı, kullanıcıların Stream Analytics sorgularına göre olayların tetiklenmesini sağlayacak şekilde Stream Analytics’e İşlevleri bağlamasına olanak tanır. 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Oluşturma ve bir Stream Analytics işi çalıştırma
> * Redis örneği için bir Azure önbelleği oluşturma
> * Azure İşlevi oluşturma
> * Azure önbelleği için Redis için sonuçları denetleyin.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="configure-a-stream-analytics-job-to-run-a-function"></a>İşlev çalıştırmak için bir Stream Analytics işi yapılandırma 

Bu bölümde, Azure önbelleği için Redis veri yazan bir işlev çalıştırmak için bir Stream Analytics işini yapılandırma işlemi gösterilmektedir. Stream Analytics işi Azure Event Hubs’dan olayları okur ve işlevi çağıran bir sorgu çalıştırır. Bu işlev Stream Analytics işinden verileri okur ve Azure önbelleği için Redis yazar.

![Azure hizmetleri arasındaki ilişkileri gösteren diyagram](./media/stream-analytics-with-azure-functions/image1.png)

## <a name="create-a-stream-analytics-job-with-event-hubs-as-input"></a>Girdi olarak Event Hubs ile bir Stream Analytics işi oluşturma

Bir olay hub’ı oluşturmak, olay oluşturucu uygulamasını başlamak ve bir Stream Analytics işi oluşturmak için [Gerçek zamanlı sahtekarlık algılama](stream-analytics-real-time-fraud-detection.md) öğreticisini takip edin. Sorgu ve çıktı oluşturma adımlarını atlayın. Bunun yerine, bir Azure işlevler çıkışını ayarlamak için aşağıdaki bölümlere bakın.

## <a name="create-an-azure-cache-for-redis-instance"></a>Redis örneği için bir Azure önbelleği oluşturma

1. Önbellek açıklanan adımları kullanarak Azure Cache Redis için oluşturmak [bir önbellek oluşturma](../azure-cache-for-redis/cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache).  

2. Önbelleği oluşturduktan sonra **Ayarlar** altında **Erişim Anahtarları**’nı seçin. **Birincil bağlantı dizesi**’ni not edin.

   ![Redis bağlantı dizesi için Azure önbellek ekran görüntüsü](./media/stream-analytics-with-azure-functions/image2.png)

## <a name="create-a-function-in-azure-functions-that-can-write-data-to-azure-cache-for-redis"></a>Azure işlevleri, verileri Azure önbelleği için Redis yazabilen bir işlev oluşturma

1. İşlevler belgesinin [İşlev uygulaması oluşturma](../azure-functions/functions-create-first-azure-function.md#create-a-function-app) bölümüne bakın. Bu bölümde CSharp dili kullanılarak bir işlev uygulaması ve [Azure İşlevleri’nde HTTP ile tetiklenen işlev](../azure-functions/functions-create-first-azure-function.md#create-function) oluşturma adımları gösterilir.  

2. **run.csx** işlevine göz atın. Aşağıdaki kodla güncelleştirin. Değiştirin **"\<, Azure önbelleği için Redis bağlantı dizesi buraya\>"** Azure Cache önceki bölümde aldığınız Redis birincil bağlantı dizesi ile. 

    ```csharp
    using System;
    using System.Net;
    using System.Threading.Tasks;
    using StackExchange.Redis;
    using Newtonsoft.Json;
    using System.Configuration;

    public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
    {
        log.Info($"C# HTTP trigger function processed a request. RequestUri={req.RequestUri}");
    
        // Get the request body
        dynamic dataArray = await req.Content.ReadAsAsync<object>();

        // Throw an HTTP Request Entity Too Large exception when the incoming batch(dataArray) is greater than 256 KB. Make sure that the size value is consistent with the value entered in the Stream Analytics portal.

        if (dataArray.ToString().Length > 262144)
        {
            return new HttpResponseMessage(HttpStatusCode.RequestEntityTooLarge);
        }
        var connection = ConnectionMultiplexer.Connect("<your Azure Cache for Redis connection string goes here>");
        log.Info($"Connection string.. {connection}");
    
        // Connection refers to a property that returns a ConnectionMultiplexer
        IDatabase db = connection.GetDatabase();
        log.Info($"Created database {db}");
    
        log.Info($"Message Count {dataArray.Count}");

        // Perform cache operations using the cache object. For example, the following code block adds few integral data types to the cache
        for (var i = 0; i < dataArray.Count; i++)
        {
            string time = dataArray[i].time;
            string callingnum1 = dataArray[i].callingnum1;
            string key = time + " - " + callingnum1;
            db.StringSet(key, dataArray[i].ToString());
            log.Info($"Object put in database. Key is {key} and value is {dataArray[i].ToString()}");
       
            // Simple get of data types from the cache
            string value = db.StringGet(key);
            log.Info($"Database got: {value}");
        }

        return req.CreateResponse(HttpStatusCode.OK, "Got");
    }

   ```

   Stream Analytics işlevden "HTTP İstek Varlığı Çok Büyük" özel durumunu aldığında İşlevler’e gönderdiği toplu işlerin boyutunu azaltır. Aşağıdaki kod, Stream Analytics toplu işler göndermediğinden göndermez sağlar. İşlevde kullanılan en büyük toplu iş sayı ve boyut değerlerinin Stream Analytics portalına girilen değerlerle tutarlı olduğundan emin olun.

    ```csharp
    if (dataArray.ToString().Length > 262144)
        {
            return new HttpResponseMessage(HttpStatusCode.RequestEntityTooLarge);
        }
   ```

3. Tercih ettiğiniz bir metin düzenleyicisinde **project.json** adlı bir JSON dosyası oluşturun. Aşağıdaki kodu yapıştırın ve dosyayı yerel bilgisayarınıza kaydedin. Bu dosya, C# işlevinin gerektirdiği NuGet paket bağımlılıklarını içerir.  
   
    ```json
    {
        "frameworks": {
            "net46": {
                "dependencies": {
                    "StackExchange.Redis":"1.1.603",
                    "Newtonsoft.Json": "9.0.1"
                }
            }
        }
    }

   ```
 
4. Azure portalına geri dönün. **Platform özellikleri** sekmesinden işlevinize göz atın. **Geliştirme Araçları** altında **App Service Düzenleyicisi**’ni seçin. 
 
   ![App Service Düzenleyicisi ekran görüntüsü](./media/stream-analytics-with-azure-functions/image3.png)

5. App Service Düzenleyicisi’nde kök dizininize sağ tıklayın ve **project.json** dosyasını karşıya yükleyin. Karşıya yükleme başarılı olduktan sonra sayfayı yenileyin. Şu anda **project.lock.json** adlı otomatik olarak oluşturulmuş bir dosya görmeniz gerekir. Otomatik olarak oluşturulan dosya, project.json dosyasındaki .dll dosyalarının başvurularını içerir.  

   ![App Service Düzenleyicisi ekran görüntüsü](./media/stream-analytics-with-azure-functions/image4.png)

## <a name="update-the-stream-analytics-job-with-the-function-as-output"></a>Çıktı olarak işlevle Stream Analytics işini güncelleştirme

1. Stream Analytics işinizi Azure portalında açın.  

2. İşlevinize göz atın ve **Genel Bakış** > **Çıktılar** > **Ekle**’yi seçin. Yeni bir çıktı eklemek için havuz seçeneği olarak **Azure İşlevi**’ni seçin. İşlevlerin çıkış bağdaştırıcısı aşağıdaki özelliklere sahiptir:  

   |**Özellik adı**|**Açıklama**|
   |---|---|
   |Çıktı diğer adı| İşin sorgusunda çıktıya başvurmak için kullandığınız kolay ad. |
   |İçeri aktarma seçeneği| Geçerli abonelikteki işlevi kullanabilir veya işlev başka bir abonelikte bulunuyorsa ayarları el ile sağlayabilirsiniz. |
   |İşlev Uygulaması| İşlevler uygulamanızın adı. |
   |İşlev| İşlevler uygulamanızdaki işlevin adı (run.csx işlevinizin adı).|
   |En Büyük Toplu İş Boyutu|Bayt cinsinden işlevinize gönderilen her çıktı toplu işinin en büyük boyutunu ayarlar. Varsayılan olarak, bu değer 262.144 bayt (256 KB) ayarlanır.|
   |En Büyük Toplu İş Sayısı|İşleve gönderilen her toplu işteki en büyük olay sayısını belirtir. Varsayılan değer 100’dür. Bu özellik isteğe bağlıdır.|
   |Anahtar|Başka bir abonelikteki işlevi kullanmanıza olanak sağlar. İşlevinize erişmek için anahtar değerini sağlayın. Bu özellik isteğe bağlıdır.|

3. Çıktı diğer adı için bir ad belirtin. Bu öğreticide, adlı **saop1**, ancak kendi tercih ettiğiniz herhangi bir ad kullanabilirsiniz. Diğer ayrıntıları girin.

4. Stream Analytics işinizi açın ve sorguyu aşağıdaki gibi güncelleştirin. Çıkış havuz adı değil **saop1**, sorguda değiştirmeyi unutmayın.  

   ```sql
    SELECT
            System.Timestamp as Time, CS1.CallingIMSI, CS1.CallingNum as CallingNum1,
            CS2.CallingNum as CallingNum2, CS1.SwitchNum as Switch1, CS2.SwitchNum as Switch2
        INTO saop1
        FROM CallStream CS1 TIMESTAMP BY CallRecTime
           JOIN CallStream CS2 TIMESTAMP BY CallRecTime
            ON CS1.CallingIMSI = CS2.CallingIMSI AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5
        WHERE CS1.SwitchNum != CS2.SwitchNum
   ```

5. Komut satırında aşağıdaki komutu çalıştırarak telcodatagen.exe uygulamasını başlatın. Komut biçimi kullanır `telcodatagen.exe [#NumCDRsPerHour] [SIM Card Fraud Probability] [#DurationHours]`.  
   
   ```cmd
   telcodatagen.exe 1000 0.2 2
   ```
    
6.  Stream Analytics işini başlatın.

## <a name="check-azure-cache-for-redis-for-results"></a>Azure önbelleği için Redis için sonuçları denetleyin.

1. Azure portalına gidin ve, Azure önbelleği için Redis bulun. **Konsol**’u seçin.  

2. Kullanım [Azure önbelleği için Redis komutları](https://redis.io/commands) verilerinizi Azure önbelleği için Redis olduğunu doğrulayın. (Komut Get {key} biçimini alır.) Örneğin:

   **Get "19.12.2017 21:32:24 - 123414732"**

   Bu komut, belirtilen anahtarın değerini yazdırmalıdır:

   ![Redis çıkış için Azure önbellek ekran görüntüsü](./media/stream-analytics-with-azure-functions/image5.png)
   
## <a name="error-handling-and-retries"></a>Hata işleme ve yeniden deneme
Azure İşlevleri'ne olay gönderme sırasında hatayla karşılaşılması halinde Stream Analytics işlemi başarıyla tamamlamayı dener. Ancak aşağıda belirtilen hatalarda yeniden deneme girişiminde bulunulmaz:

 1. HttpRequestExceptions
 2. İstek Varlığı Çok Büyük (Http hata kodu 413)
 3. ApplicationExceptions

## <a name="known-issues"></a>Bilinen sorunlar

Azure portalında En Büyük Toplu İş Boyutu/En Büyük Toplu İş Sayısı değerini boş (varsayılan) olarak sıfırlamaya çalıştığınızda değer kaydedildikten sonra daha önce girilen değerle değiştirilir. Bu durumda bu alanların varsayılan değerlerini el ile girin.

Kullanımını [Http yönlendirme](https://docs.microsoft.com/sandbox/functions-recipes/routes?tabs=csharp) Azure işlevleriniz üzerinde Stream Analytics tarafından şu anda desteklenmiyor.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu, akış işini ve tüm ilgili kaynakları silin. İşin silinmesi, iş tarafından kullanılan akış birimlerinin faturalanmasını önler. İşi gelecekte kullanmayı planlıyorsanız, durdurup daha sonra gerektiğinde yeniden başlatabilirsiniz. Bu işi kullanmaya devam etmeyecekseniz aşağıdaki adımları kullanarak bu hızlı başlangıçla oluşturulan tüm kaynakları silin:

1. Azure portalında sol taraftaki menüden, **Kaynak grupları**'na ve ardından oluşturduğunuz kaynağın adına tıklayın.  
2. Kaynak grubu sayfanızda, **Sil**'e tıklayın, metin kutusuna silinecek kaynağın adını yazın ve ardından **Sil**'e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir Azure İşlevi çalıştıran basit bir Stream Analytics işi oluşturdunuz. Stream Analytics işleri hakkında daha fazla bilgi almak için sonraki öğreticiye geçin:

> [!div class="nextstepaction"]
> [Stream Analytics işlerinde JavaScript kullanıcı tanımlı işlevleri çalıştırma](stream-analytics-javascript-user-defined-functions.md)
