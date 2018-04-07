---
title: Azure işlevlerini Azure akış analizi işleri çalıştırma
description: Bu makalede, Azure işlevleri, olay sürücü iş yükleri için Stream Analytics işlerini çıkış havuzu olarak yapılandırmak açıklar.
services: stream-analytics
author: jasonwhowell
ms.author: jasonh
ms.reviewer: jasonh
manager: kfile
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/19/2017
ms.openlocfilehash: a8eebfa0c40caa455eb20431e5cf4acb8eeb248c
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="run-azure-functions-from-azure-stream-analytics-jobs"></a>Azure işlevlerini Azure akış analizi işleri çalıştırma 

Akış analizi işine çıkış havuzlarını biri olarak işlevleri yapılandırarak Azure Stream Analytics'ten Azure işlevleri çalıştırabilirsiniz. Azure veya üçüncü taraf hizmetleri gerçekleşen olaylar tarafından tetiklenen kodları uygulama olanak sağlayan bir olay denetimli, isteğe bağlı işlem deneyimi işlevlerdir. Tetikleyiciler için yanıt özelliği işlevlerin Stream Analytics işleri için doğal bir çıktı kolaylaştırır.

Akış analizi işlevleri HTTP Tetikleyicileri çağırır. Stream Analytics sorgularına dayalı olayları tetiklenebilir şekilde işlevleri çıkış bağdaştırıcısı akış analizi için işlevleri bağlanmasına olanak sağlar. 

Bu öğretici için Stream Analytics bağlanma gösterir [Azure Redis önbelleği](../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md), kullanarak [Azure işlevleri](../azure-functions/functions-overview.md). 

## <a name="configure-a-stream-analytics-job-to-run-a-function"></a>İşlevi çalıştırmak için bir akış analizi işi yapılandırın 

Bu bölümde, Azure Redis önbelleği için verileri yazar işlevi çalıştırmak için bir akış analizi işi yapılandırmak gösterilmiştir. Stream Analytics işi Azure Event Hubs'tan gelen olayları okur ve işlevi çağıran bir sorguyu çalıştırır. Bu işlev, Stream Analytics işten verileri okur ve Azure Redis önbelleği için yazar.

![Azure Hizmetleri arasındaki ilişkileri gösteren diyagram](./media/stream-analytics-with-azure-functions/image1.png)

Aşağıdaki adımlar, bu görevi gerçekleştirmek için gereklidir:
* [Akış analizi işi Event Hubs ile giriş olarak oluşturun.](#create-stream-analytics-job-with-event-hub-as-input)  
* [Bir Azure Redis önbelleği örneği oluşturma](#create-an-azure-redis-cache)  
* [Azure Redis önbelleği için veri yazabilirsiniz Azure işlevlerinde bir işlev oluşturun](#create-an-azure-function-that-can-write-data-to-the-redis-cache)    
* [İşlevi ile Stream Analytics işi çıkış olarak güncelleştirme](#update-the-stream-analytic-job-with-azure-function-as-output)  
* [Azure Redis önbelleği için sonuçları denetleyin](#check-redis-cache-for-results)  

## <a name="create-a-stream-analytics-job-with-event-hubs-as-input"></a>Akış analizi işi Event Hubs ile giriş olarak oluşturun.

İzleyin [gerçek zamanlı sahtekarlık algılama](stream-analytics-real-time-fraud-detection.md) bir event hub oluşturmak, olay Oluşturucu uygulamayı başlatın ve bir Stream Analytics işi oluşturmak için öğretici. (Sorgu ve çıktı oluşturmak için aşağıdaki adımları atlayın. Bunun yerine, işlevleri çıkış ayarlamak için aşağıdaki bölümlere bakın.)

## <a name="create-an-azure-redis-cache-instance"></a>Bir Azure Redis önbelleği örneği oluşturma

1. Açıklanan adımları kullanarak Azure Redis Önbelleği'nde bir önbellek oluşturma [bir önbellek oluşturma](../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache).  

2. Önbellek altında oluşturduktan sonra **ayarları**seçin **erişim tuşları**. Not **birincil bağlantı dizesi**.

   ![Ekran görüntüsü, Azure Redis önbelleği bağlantı dizesi](./media/stream-analytics-with-azure-functions/image2.png)

## <a name="create-a-function-in-azure-functions-that-can-write-data-to-azure-redis-cache"></a>Verileri Azure Redis önbelleğine yazabilecek Azure işlevlerinde bir işlev oluşturun

1. Bkz: [bir işlev uygulaması oluşturma](../azure-functions/functions-create-first-azure-function.md#create-a-function-app) işlevleri belgelerine bölümü. Bu işlev uygulaması oluşturmak nasıl aracılığıyla anlatılmaktadır ve bir [HTTP tetiklemeli işlevin Azure işlevlerinde](../azure-functions/functions-create-first-azure-function.md#create-function), CSharp dili kullanarak.  

2. Gözat **run.csx** işlevi. Aşağıdaki kod ile güncelleştirin. (Değiştirdiğinizden emin olun "\<redis önbelleği bağlantı dizenizi Buraya\>" önceki bölümde alınan Azure Redis önbelleği birincil bağlantı dizesiyle.)  

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
      var connection = ConnectionMultiplexer.Connect("<your redis cache connection string goes here>");
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

   Stream Analytics işlevinden "HTTP istek varlığı çok büyük" özel durum aldığında, işlevlere gönderir toplu boyutunu azaltır. İşlevinizi, Stream Analytics büyük boyutlu toplu göndermez denetlemek için aşağıdaki kodu kullanın. İşlevde kullanılan en büyük toplu iş sayısı ve boyutu değerlerini Stream Analytics portalda girdiğiniz değerleri ile tutarlı olduğundan emin olun.

   ```csharp
   if (dataArray.ToString().Length > 262144)
      {        
        return new HttpResponseMessage(HttpStatusCode.RequestEntityTooLarge);
      }
   ```

3. Tercih ettiğiniz bir metin düzenleyicisinde adlı bir JSON dosyası oluşturun **project.json**. Aşağıdaki kodu kullanın ve yerel bilgisayarınıza kaydedin. Bu dosya C# işlevi tarafından gerekli NuGet Paket bağımlılıklarını içerir.  
   
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
 
4. Azure portalına geri dönün. Gelen **Platform özellikleri** sekmesinde, işlevinizi için göz atın. Altında **geliştirme araçları**seçin **App Service Düzenleyicisi**. 
 
   ![Uygulama hizmeti Düzenleyicisi'nin ekran görüntüsü](./media/stream-analytics-with-azure-functions/image3.png)

5. Uygulama hizmeti Düzenleyicisi'nde, kök dizininizde sağ tıklayın ve karşıya **project.json** dosya. Karşıya yükleme başarılı olduktan sonra sayfayı yenileyin. Adlı bir otomatik olarak oluşturulur dosya görmelisiniz **böylece project.lock.json**. Otomatik olarak oluşturulur dosya project.json dosyasında belirtilen bir .dll dosyaları başvurular içeriyor.  

   ![Uygulama hizmeti Düzenleyicisi'nin ekran görüntüsü](./media/stream-analytics-with-azure-functions/image4.png)

 

## <a name="update-the-stream-analytics-job-with-the-function-as-output"></a>İşlevi ile Stream Analytics işi çıkış olarak güncelleştirme

1. Azure Portal'da, akış analizi işi'ni açın.  

2. İşlevinizi için gözatın ve seçin **genel bakış** > **çıkışları** > **Ekle**. Yeni bir çıktı eklemek için seçin **Azure işlevi** havuz seçeneği için. Aşağıdaki özelliklerle yeni işlevler çıkış bağdaştırıcısı kullanılabilir:  

   |**Özellik adı**|**Açıklama**|
   |---|---|
   |Çıkış diğer adı| İşin sorgu başvuru çıktı için kullandığınız bir kolay ad. |
   |İçeri aktarma seçeneği| Geçerli aboneliğe ilişkin işlevi kullanın veya başka bir abonelikte işlevi bulunuyorsa ayarları el ile belirtin. |
   |İşlev Uygulaması| İşlevler uygulamanızın adı. |
   |İşlev| İşlevler uygulamanızda (run.csx işlevinizin adı) işlevinin adı.|
   |En büyük toplu iş boyutu|İşleve gönderilen her çıktı toplu boyut üst sınırını ayarlar. Varsayılan olarak, bu değer 256 KB olarak ayarlanır.|
   |En büyük toplu iş sayısı|İşleve gönderilen her yığında olayların maksimum sayısını belirtir. Varsayılan değer 100'dür. Bu özellik isteğe bağlıdır.|
   |Anahtar|Başka bir abonelik işlevinden kullanmanıza olanak sağlar. İşlevinizi erişmek için anahtar değeri sağlayın. Bu özellik isteğe bağlıdır.|

3. Çıkış diğer adları için bir ad sağlayın. Bu öğreticide, biz adlandırın **saop1** (tercih ettiğiniz herhangi bir ad kullanabilirsiniz). Diğer ayrıntıları doldurun.  

4. Stream Analytics işiniz açın ve aşağıdaki güncelleştirme sorgusu. ("Saop1" metin çıkış havuzunun farklı adlandırdıysanız değiştirdiğinizden emin olun.)  

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

5. Komut satırında aşağıdaki komutu çalıştırarak telcodatagen.exe uygulamayı başlatın (biçimini kullanın `telcodatagen.exe [#NumCDRsPerHour] [SIM Card Fraud Probability] [#DurationHours]`):  
   
   **telcodatagen.exe 1000 .2 2**
    
6.  Stream Analytics işi başlatın.

## <a name="check-azure-redis-cache-for-results"></a>Azure Redis önbelleği için sonuçları denetleyin

1. Azure Portalı'na gidin ve Azure Redis önbelleği bulun. Seçin **konsol**.  

2. Kullanım [Redis önbelleği komutları](https://redis.io/commands) verilerinizi Redis Önbelleği'nde olduğu doğrulanamadı. (Komutu biçimini alır Get {anahtar}.) Örneğin:

   **Get "12/19/2017 21:32:24 - 123414732"**

   Bu komut, belirtilen anahtar için değer Yazdır:

   ![Ekran görüntüsü, Azure Redis önbelleği çıkış](./media/stream-analytics-with-azure-functions/image5.png)

## <a name="known-issues"></a>Bilinen sorunlar

Azure portalında Maksimum toplu iş boyutu sıfırlamaya çalıştığınızda / en büyük toplu iş sayısı (varsayılan), boş değere geri değerine önceden girilen değerini değiştirir. El ile bu durumda bu alanları için varsayılan değerleri girin.

