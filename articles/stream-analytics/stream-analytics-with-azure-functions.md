---
title: "Azure işlevleri ile Azure akış analizi işleri çalıştırma | Microsoft Docs"
description: "Akış analitik işleri için çıkış havuzu olarak Azure işlevi yapılandırma konusunda bilgi edinin."
keywords: "Çıkış, veri akış verileri Azure işlevi"
documentationcenter: 
services: stream-analytics
author: SnehaGunda
manager: kfile
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 12/19/2017
ms.author: sngun
ms.openlocfilehash: adc147fc9f47e78cc0a2fcaf53f064bcfa5eee2c
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2017
---
# <a name="run-azure-functions-with-azure-stream-analytics-jobs"></a>Azure işlevleri ile Azure akış analizi işleri çalıştırma 
 
> [!IMPORTANT]
> Bu işlev önizlemede değil.

Akış analizi işine çıkış havuzlarını biri olarak Azure işlevleri yapılandırarak Azure akış Analizi ile Azure işlevleri çalıştırabilirsiniz. Azure işlevi, bir olay yönelimli, Azure veya üçüncü taraf hizmetleri gerçekleşen olaylar tarafından tetiklenen kodları uygulama olanak sağlayan isteğe bağlı işlem deneyimi kullanılabilir. Tetikleyiciler için yanıt Özelliği Azure işlevinin Azure Stream Analytics işi için doğal bir çıktı kolaylaştırır.

Azure Stream Analytics HTTP Tetikleyicileri Azure işlevi çağırır. Azure işlevi çıkış bağdaştırıcısı Stream Analytics sorgularına dayalı olayları tetiklenebilir şekilde Azure işlevleri Stream Analytics bağlanmasına olanak sağlar. 

Bu öğretici için Azure Stream Analytics bağlanma gösterir [Azure Redis önbelleği](../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md) kullanarak [Azure işlevleri](../azure-functions/functions-overview.md). 

## <a name="configure-stream-analytics-job-to-run-an-azure-function"></a>Bir Azure işlevi çalıştırmak için Stream Analytics işi yapılandırın 

Bu bölümde, Azure Redis önbelleği için verileri Yazar bir Azure işlevi çalıştırmak için bir akış analizi işi yapılandırmak gösterilmiştir. Stream Analytics işi olayları olay Hub'ından okur, Azure işlevi çağıran bir sorgu yürütür. Bu Azure işlevi Stream Analytics işten verileri okur ve Azure Redis önbelleği için yazar.

![Öğretici göstermek için grafiği](./media/stream-analytics-with-azure-functions/image1.png)

Aşağıdaki adımlar, bu görevi gerçekleştirmek için gereklidir:
* [Akış analizi işine giriş olarak Event Hub ile oluşturun.](#create-stream-analytics-job-with-event-hub-as-input)  
* [Bir Azure Redis önbelleği oluşturun.](#create-an-azure-redis-cache)  
* [Veri Redis önbelleğine yazabilecek bir Azure işlevi oluşturma.](#create-an-azure-function-that-can-write-data-to-the-redis-cache)    
* [Akış analitik iş çıktısı olarak Azure işlevi ile güncelleştirin.](#update-the-stream-analytic-job-with-azure-function-as-output)  
* [Redis önbelleği için sonuçları denetleyin.](#check-redis-cache-for-results)  

## <a name="create-stream-analytics-job-with-event-hub-as-input"></a>Stream Analytics işi, girdi olarak Event Hub ile oluşturma

İzleyin [gerçek zamanlı sahtekarlık algılama](stream-analytics-real-time-fraud-detection.md) bir event hub oluşturmak, olay Oluşturucu uygulamayı başlatın ve bir Azure akış analizi oluşturmak için öğretici (sorgu ve çıktı oluşturmak için adımları atlayın, bunun yerine Kurulum bir Sonraki bölümde Azure işlevleri çıktı.)

## <a name="create-an-azure-redis-cache"></a>Azure Redis Cache oluşturma

1. Açıklanan adımları kullanarak bir Azure Redis önbelleği oluşturma [bir önbellek oluşturma](../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache) Redis önbelleği makalenin bölümüne.  

2. Redis önbelleği oluşturulduktan sonra oluşturulan önbelleğine gidin > **erişim tuşları** > ve Not **birincil bağlantı dizesi**.

   ![Redis önbelleği bağlantı dizesi](./media/stream-analytics-with-azure-functions/image2.png)

## <a name="create-an-azure-function-that-can-write-data-to-the-redis-cache"></a>Veri Redis önbelleğine yazabilecek bir Azure işlevi oluşturma

1. Kullanım [bir işlev uygulaması oluşturma](../azure-functions/functions-create-first-azure-function.md#create-a-function-app) Azure işlev uygulaması oluşturmak için Azure işlevleri belgelerin bölüm ve bir [HTTP tetiklemeli Azure işlevi](../azure-functions/functions-create-first-azure-function.md#create-function) (diğer adıyla Web kancası) kullanarak **CSharp** dili.  

2. Gidin **run.csx** işlev ve aşağıdaki kod ile güncelleştirin (değiştirdiğinizden emin olun "\<redis önbelleği bağlantı dizenizi Buraya\>" Redis Önbelleği'nin birincil bağlantı dizesiyle metin Bu, önceki bölümde alınır):  

   ```c#
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

   Azure Stream Analytics Azure işlevinden 413 (Http istek varlığı çok büyük) özel durum aldığında, Azure işlevleri gönderir toplu boyutunu azaltır. Azure işlevinizi Azure akış analizi büyük boyutlu toplu göndermez denetlemek için aşağıdaki kodu kullanın. İşlevde kullanılan en büyük toplu iş sayısı ve boyutu değerlerini Stream Analytics portalda girdiğiniz değerleri ile tutarlı olduğundan emin olun.

   ```c#
   if (dataArray.ToString().Length > 262144)
      {        
        return new HttpResponseMessage(HttpStatusCode.RequestEntityTooLarge);
      }
   ```

3. Tercih ettiğiniz bir metin düzenleyicisinde adlı bir JSON dosyası oluşturun **project.json** aşağıdaki kodla ve onu yerel bilgisayarınıza kaydedin. Bu dosya c# işlevi tarafından gerekli NuGet Paket bağımlılıklarını içerir.  
   
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
 
4. Azure portalına dönün > Azure işlevinizi Git > gelen **Platform özellikleri** sekmesini > tıklayın **App Service Düzenleyicisi** altında bulunan **geliştirme araçları**. 
 
   ![Uygulama hizmeti Düzenleyicisi ekranı](./media/stream-analytics-with-azure-functions/image3.png)

5. Uygulama hizmeti Düzenleyicisi'nde, kök dizininizde sağ tıklayın ve karşıya **project.json** dosya. Karşıya yükleme başarılı olduktan sonra sayfayı yenileyin, adlı bir otomatik olarak oluşturulur dosya görmelisiniz **böylece project.lock.json**.  Otomatik olarak oluşturulur dosya Project.json dosyasında belirtilen DLL'leri başvurular içeriyor.  

   ![Project.JSON dosyası karşıya yükle](./media/stream-analytics-with-azure-functions/image4.png)

 

## <a name="update-the-stream-analytic-job-with-azure-function-as-output"></a>Güncelleştirme çıktı olarak akış analitik iş ile Azure işlevi

1. Azure portalı, Azure akış analizi işi'ni açın.  

2. Azure işlevinizi gidin > **genel bakış** > **çıkışları** > **Ekle** yeni bir çıkış > seçin **Azureişlevi** havuz seçeneği için. Aşağıdaki özelliklere sahip yeni Azure işlevi çıkış bağdaştırıcısı kullanılabilir:  

   |**Özellik adı**|**Açıklama**|
   |---|---|
   |Çıkış diğer adları| İşin sorgu başvuru çıktı için kullandığınız bir kolay ad. |
   |İçe aktarma seçeneği| Geçerli aboneliğe ilişkin azure işlevi kullanın veya diğer Abonelik işlevi bulunuyorsa ayarları el ile belirtin. |
   |İşlev Uygulaması| Azure işlevi uygulamanızın adı. |
   |İşlev| İşlev uygulamanızda (run.csx işlevinizin adı) işlevinin adı.|
   |En büyük toplu iş boyutu|Bu özellik, Azure işlevinizi gönderilen her çıktı toplu iş boyutu üst sınırı ayarlamak için kullanılır. Varsayılan olarak, bu değer 256 KB olarak ayarlanır.|
   |En büyük toplu iş sayısı|Bu özellik, Azure işleve gönderilen her yığında olayların maksimum sayısını belirtmenizi sağlar. Varsayılan Maksimum toplu iş sayısı değeri 100'dür. Bu özellik isteğe bağlıdır.|
   |Anahtar|Bu özellik, başka bir abonelik Azure işlevinden kullanmanıza olanak sağlar. İşlevinizi erişmek için anahtar değeri sağlayın. Bu özellik isteğe bağlıdır.|

3. Çıkış diğer adları için bir ad sağlayın. Bu öğreticide, biz adlandırın **saop1** (tercih ettiğiniz herhangi bir ad kullanabilirsiniz) ve diğer ayrıntıları doldurun.  

4. Stream Analytics işiniz açın ve sorgu (çıkış havuzunun farklı adlandırdıysanız "saop1" metin değiştirdiğinizden emin olun) şu şekilde güncelleştirin:  

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

5. Komut satırında aşağıdaki komutu çalıştırarak telcodatagen.exe uygulamayı başlatın (komut biçimi - alır `telcodatagen.exe [#NumCDRsPerHour] [SIM Card Fraud Probability] [#DurationHours]`)  
   
   **telcodatagen.exe 1000.2 2**
    
6.  Akış analizi işi başlatın.

## <a name="check-redis-cache-for-results"></a>Redis önbelleği için sonuçları denetleyin

1. Azure Portalı'na gidin ve Redis önbelleği Bul > seçin **konsol**.  

2. Kullanım [Redis önbelleği komutları](https://redis.io/commands) verilerinizi Redis Önbelleği'nde olduğu doğrulanamadı. ({Anahtarı} biçimi-Get komutu alır). Örneğin:

   **Al "19/12/2017 21:32:24-123414732"**

   Bu komut, belirtilen anahtar için değer Yazdır:

   ![Redis önbelleği çıkış](./media/stream-analytics-with-azure-functions/image5.png)

## <a name="known-issues"></a>Bilinen sorunlar

* Azure portalında Maksimum toplu iş boyutu sıfırlamaya çalıştığınızda / en büyük toplu iş sayısı değeri empty(default) için geri değerine önceden girilen değiştirir. Kasıtlı olarak bu durumda bu alanları için varsayılan değerleri girin.

## <a name="next-steps"></a>Sonraki adımlar
