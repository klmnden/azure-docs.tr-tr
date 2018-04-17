---
title: Uygulama verilerini Azure'da yüksek oranda kullanılabilir hale getirme | Microsoft Belgeleri
description: Okuma erişimli coğrafi olarak yedekli depolamayı kullanarak uygulama verilerinizi yüksek oranda kullanılabilir yapma
services: storage
author: tamram
manager: jeconnoc
ms.service: storage
ms.workload: web
ms.topic: tutorial
ms.date: 03/26/2018
ms.author: tamram
ms.custom: mvc
ms.openlocfilehash: 86fb0ae7c9ee5a2856c81603a4e08ae7016b022f
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="make-your-application-data-highly-available-with-azure-storage"></a>Azure depolama ile uygulama verilerinizi yüksek oranda kullanılabilir hale getirme

Bu öğretici, uygulama verilerinizi Azure’da yüksek oranda kullanılabilir hale getirmeyi açıklayan bir serinin ilk bölümüdür. Öğreticiyi tamamladığınızda [okuma erişimli coğrafi olarak yedekli](../common/storage-redundancy-grs.md#read-access-geo-redundant-storage) (RA-GRS) bir depolama hesabına blob yükleyen ve buradan blob alan bir konsol uygulamanız olur. RA-GRS, birincil bölgedeki işlemleri ikincil bölgede çoğaltarak çalışır. Bu çoğaltma işlemi, ikincil bölgedeki verilerin nihai olarak tutarlı olmasını sağlar. Uygulama, hangi uç noktaya bağlanılacağını belirlemek için [Devre Kesici](/azure/architecture/patterns/circuit-breaker) düzenini kullanır. Bir hatanın simülasyonu yapıldığında uygulama ikincil uç noktaya geçer.

Serinin birinci bölümünde şunları öğrenirsiniz:

> [!div class="checklist"]
> * Depolama hesabı oluşturma
> * Örneği indirme
> * Bağlantı dizesini ayarlama
> * Konsol uygulamasını çalıştırma

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için:
 
# <a name="net-tabdotnet"></a>[.NET] (#sekme/dotnet)
* [Visual Studio 2017](https://www.visualstudio.com/downloads/)’yi aşağıdaki iş yükleri ile yükleyin:
  - **Azure geliştirme**

  ![Azure geliştirme (Web ve Bulut altında)](media/storage-create-geo-redundant-storage/workloads.png)

* (İsteğe bağlı) [Fiddler](https://www.telerik.com/download/fiddler)’ı indirip yükleme
 
# <a name="python-tabpython"></a>[Python] (#sekme/python) 

* [Python](https://www.python.org/downloads/)’ı yükleyin
* [Python için Azure Depolama SDK’sını](https://github.com/Azure/azure-storage-python) indirip yükleme
* (İsteğe bağlı) [Fiddler](https://www.telerik.com/download/fiddler)’ı indirip yükleme

# <a name="java-tabjava"></a>[Java] (#tab/java)

* [Maven](http://maven.apache.org/download.cgi)’ı yükleyip komut satırından çalışacak şekilde yapılandırma
* [JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html)’yı yükleme ve yapılandırma

---

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="log-in-to-the-azure-portal"></a>Azure portalında oturum açma

[Azure Portal](https://portal.azure.com/)’da oturum açın.

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Depolama hesabı, Azure Storage veri nesnelerinizi depolamak ve bunlara erişmek için benzersiz ad alanı sağlar.

Okuma erişimli coğrafi olarak yedekli depolama hesabı oluşturmak için aşağıdaki adımları izleyin:

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** düğmesini seçin.

2. **Yeni** sayfasında **Depolama**’yı seçin ve **Öne Çıkanlar**’ın altında **Depolama hesabı - blob, dosya, tablo, kuyruk**’u seçin.
3. Aşağıdaki bilgileri kullanarak depolama hesabı formunu alttaki resimde gösterildiği gibi doldurun ve **Oluştur**’u seçin:

   | Ayar       | Önerilen değer | Açıklama |
   | ------------ | ------------------ | ------------------------------------------------- |
   | **Ad** | mystorageaccount | Depolama hesabınız için benzersiz bir değer |
   | **Dağıtım modeli** | Resource Manager  | Resource Manager en son özellikleri içerir.|
   | **Hesap türü** | StorageV2 | Hesap türleri hakkında ayrıntılı bilgi almak için bkz. [depolama hesabı türleri](../common/storage-introduction.md#types-of-storage-accounts) |
   | **Performans** | Standart | Standart, örnek senaryo için yeterli olacaktır. |
   | **Çoğaltma**| Okuma erişimli coğrafi olarak yedekli depolama (RA-GRS) | Örneğin çalışması için bunun seçilmesi gereklidir. |
   |**Güvenli aktarım gerekir** | Devre dışı| Bu senaryo için güvenli aktarım gerekli değildir. |
   |**Abonelik** | aboneliğiniz |Abonelikleriniz hakkında daha ayrıntılı bilgi için bkz. [Abonelikler](https://account.windowsazure.com/Subscriptions). |
   |**ResourceGroup** | myResourceGroup |Geçerli kaynak grubu adları için bkz. [Adlandırma kuralları ve kısıtlamalar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). |
   |**Konum** | Doğu ABD | Konum seçin. |

![depolama hesabı oluşturma](media/storage-create-geo-redundant-storage/createragrsstracct.png)

## <a name="download-the-sample"></a>Örneği indirme

# <a name="net-tabdotnet"></a>[.NET] (#sekme/dotnet)

[Örnek projeyi indirin](https://github.com/Azure-Samples/storage-dotnet-circuit-breaker-pattern-ha-apps-using-ra-grs/archive/master.zip) ve storage-dotnet-circuit-breaker-pattern-ha-apps-using-ra-grs.zip dosyasını ayıklayın (sıkıştırmasını açın). Geliştirme ortamına uygulamanın bir kopyasını indirmek için [git](https://git-scm.com/) de kullanılabilir. Örnek proje bir konsol uygulaması içerir.

```bash
git clone https://github.com/Azure-Samples/storage-dotnet-circuit-breaker-pattern-ha-apps-using-ra-grs.git 
```
# <a name="python-tabpython"></a>[Python] (#sekme/python) 

[Örnek projeyi indirin](https://github.com/Azure-Samples/storage-python-circuit-breaker-pattern-ha-apps-using-ra-grs/archive/master.zip) ve storage-python-circuit-breaker-pattern-ha-apps-using-ra-grs.zip dosyasını ayıklayın (sıkıştırmasını açın). Geliştirme ortamına uygulamanın bir kopyasını indirmek için [git](https://git-scm.com/) de kullanılabilir. Örnek proje temel bir Python uygulaması içerir.

```bash
git clone https://github.com/Azure-Samples/storage-python-circuit-breaker-pattern-ha-apps-using-ra-grs.git
```

# <a name="java-tabjava"></a>[Java] (#tab/java)
[Örnek projeyi indirin](https://github.com/Azure-Samples/storage-java-ha-ra-grs) ve storage-java-ragrs.zip dosyasını ayıklayın. Geliştirme ortamına uygulamanın bir kopyasını indirmek için [git](https://git-scm.com/) de kullanılabilir. Örnek proje, temel bir Java uygulaması içerir.

```bash
git clone https://github.com/Azure-Samples/storage-java-ha-ra-grs.git
```
---


## <a name="set-the-connection-string"></a>Bağlantı dizesini ayarlama

Uygulamada, depolama hesabınız için bağlantı dizesi sağlamanız gerekir. Bu bağlantı dizesini uygulamayı çalıştıran yerel makine üzerindeki bir ortam değişkeninde depolamanız önerilir. Ortam değişkenini oluşturmak için İşletim Sisteminize bağlı olarak aşağıdaki örneklerden birini izleyin.

Azure portalında depolama hesabınıza gidin. Depolama hesabınızdaki **Ayarlar** bölümünde **Erişim anahtarları**’nı seçin. Birincil veya ikincil anahtardaki **bağlantı dizesini** kopyalayın. İşletim Sisteminize göre aşağıdaki komutlardan birini çalıştırarak \<yourconnectionstring\> değerini kendi bağlantı dizenizle değiştirin. Bu komut, yerel makinede bir ortam değişkeni kaydeder. Bu ortam değişkeni, Windows’da **Komut İstemi** veya kullanmakta olduğunuz kabuk yeniden yüklenene kadar kullanılamaz. Aşağıdaki örnekte **\<storageConnectionString\>**’i değiştirin:

# <a name="linux-tablinux"></a>[Linux] (#sekme/linux) 
export storageconnectionstring=\<yourconnectionstring\> 

# <a name="windows-tabwindows"></a>[Windows] (#sekme/windows) 
setx storageconnectionstring "\<yourconnectionstring\>"

---


## <a name="run-the-console-application"></a>Konsol uygulamasını çalıştırma

# <a name="net-tabdotnet"></a>[.NET] (#sekme/dotnet)
Visual Studio'da **F5**’e basarak veya **Başlat**’ı seçerek uygulamada hata ayıklamaya başlayın. Visual Studio, yapılandırılmışsa eksik NuGet paketlerini otomatik olarak geri yükler. Daha fazla bilgi edinmek için [Paket geri yükleme özelliğiyle paketleri yükleme ve yeniden yükleme](https://docs.microsoft.com/nuget/consume-packages/package-restore#package-restore-overview) makalesini ziyaret edin.

Bir konsol penceresi açılır ve uygulama çalışmaya başlar. Uygulama, çözümdeki **HelloWorld.png** resmini depolama hesabına yükler. Uygulama, resmin ikincil RA-GRS uç noktasında çoğaltıldığını denetler. Ardından, resmi 999 kereye kadar indirmeye başlar. Her okuma işlemi bir **P** veya **S** ile temsil edilir. Burada, **P** birincil uç nokta ve **S** ikincil uç nokta demektir.

![Çalışan konsol uygulaması](media/storage-create-geo-redundant-storage/figure3.png)

Örnek kodda, [DownloadToFileAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob.downloadtofileasync?view=azure-dotnet) yöntemini kullanarak depolama hesabından bir resim indirmek için `Program.cs` dosyasındaki `RunCircuitBreakerAsync` görevi kullanılmaktadır. İndirme işlemi öncesinde bir [OperationContext](/dotnet/api/microsoft.windowsazure.storage.operationcontext?view=azure-dotnet) (İşlem Bağlamı) tanımlanır. İşlem bağlamı, indirme işlemi başarıyla tamamlandığında veya indirme işlemi başarısız olup yeniden denendiğinde başlatılan olay işleyicilerini tanımlar.

# <a name="python-tabpython"></a>[Python] (#sekme/python) 
Uygulamayı bir terminalde veya komut isteminde çalıştırmak için **circuitbreaker.py** dizinine gidip `python circuitbreaker.py` komutunu girin. Uygulama, çözümdeki **HelloWorld.png** resmini depolama hesabına yükler. Uygulama, resmin ikincil RA-GRS uç noktasında çoğaltıldığını denetler. Ardından, resmi 999 kereye kadar indirmeye başlar. Her okuma işlemi bir **P** veya **S** ile temsil edilir. Burada, **P** birincil uç nokta ve **S** ikincil uç nokta demektir.

![Çalışan konsol uygulaması](media/storage-create-geo-redundant-storage/figure3.png)

Örnek kodda, [get_blob_to_path](https://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html) yöntemini kullanarak depolama hesabından bir resim indirmek için `circuitbreaker.py` dosyasındaki `run_circuit_breaker` yöntemi kullanılmaktadır. 

Depolama nesnesi yeniden deneme işlevi, doğrusal bir yeniden deneme ilkesine ayarlıdır. Yeniden deneme işlevi, isteklerin yeniden denenip denenmeyeceğini belirler ve isteği yeniden denemeden önce kaç saniye bekleneceğini belirtir. Birincile yapılan istek başarısız olursa aynı isteğin ikincile yeniden denenmesini istiyorsanız **retry\_to\_secondary** değerini true olarak ayarlayın. Örnek uygulamada depolama nesnesinin `retry_callback` işlevinde özel bir yeniden deneme ilkesi tanımlanmıştır.
 
İndirme işlemi öncesinde Hizmet nesnesinin [retry_callback](https://docs.microsoft.com/en-us/python/api/azure.storage.common.storageclient.storageclient?view=azure-python) ve [response_callback](https://docs.microsoft.com/en-us/python/api/azure.storage.common.storageclient.storageclient?view=azure-python) işlevleri tanımlanır. Bu işlevler, indirme işlemi başarıyla tamamlandığında veya indirme işlemi başarısız olup yeniden denendiğinde başlatılan olay işleyicilerini tanımlar.  

# <a name="java-tabjava"></a>[Java] (#tab/java)
İndirilen uygulama klasörü kapsamlı bir terminal veya komut istemi açarak uygulamayı çalıştırabilirsiniz. Buradan `mvn compile exec:java` komutunu girerek uygulamayı çalıştırın. Uygulama, dizinden depolama hesabınıza **HelloWorld.png** görüntüsünü yükler ve görüntünün ikincil RA-GRS uç noktasına çoğaltılıp çoğaltılmadığını denetler. Denetim tamamlandıktan sonra uygulama art arda görüntüyü indirmeye başlar, bir yandan da içinden indirme işlemini yaptığı uç noktaya geri bildirimde bulunur.

Depolama nesnesi yeniden deneme işlevi, doğrusal bir yeniden deneme ilkesini kullanacak şekilde ayarlıdır. Yeniden deneme işlevi, isteklerin yeniden denenip denenmeyeceğini belirler ve her bir yeniden denemeden önce kaç saniye bekleneceğini belirtir. **BlobRequestOptions** öğenizin **LocationMode** özelliği, **PRIMARY\_THEN\_SECONDARY** olarak ayarlanır. Bu, uygulamanın **HelloWorld.png** dosyasını indirmeye çalışırken birincil konuma ulaşamaması durumunda otomatik olarak ikincil konuma geçmesine olanak sağlar.

---

## <a name="understand-the-sample-code"></a>Örnek kodu anlama

# <a name="net-tabdotnet"></a>[.NET] (#sekme/dotnet)

### <a name="retry-event-handler"></a>Yeniden deneme olay işleyicisi

`OperationContextRetrying` olay işleyicisi, resmin indirilmesi işlemi başarısız olmuşsa ve yeniden denenecek şekilde ayarlanmışsa çağrılır. Uygulamada tanımlı yeniden deneme sayısı üst sınırına ulaşılmışsa isteğin [LocationMode](/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions.locationmode?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_LocationMode) değeri `SecondaryOnly` olarak değiştirilir. Bu ayar, uygulamayı resmi ikincil uç noktadan indirmeyi denemeye zorlar. Birincil uç nokta süresiz olarak yeniden denenmediği için bu yapılandırma resmin istenmesinde harcanan süreyi azaltmış olur.
 
```csharp
private static void OperationContextRetrying(object sender, RequestEventArgs e)
{
    retryCount++;
    Console.WriteLine("Retrying event because of failure reading the primary. RetryCount = " + retryCount);

    // Check if we have had more than n retries in which case switch to secondary.
    if (retryCount >= retryThreshold)
    {

        // Check to see if we can fail over to secondary.
        if (blobClient.DefaultRequestOptions.LocationMode != LocationMode.SecondaryOnly)
        {
            blobClient.DefaultRequestOptions.LocationMode = LocationMode.SecondaryOnly;
            retryCount = 0;
        }
        else
        {
            throw new ApplicationException("Both primary and secondary are unreachable. Check your application's network connection. ");
        }
    }
}
```

### <a name="request-completed-event-handler"></a>İstek tamamlandı olay işleyicisi

`OperationContextRequestCompleted` olay işleyicisi, resmin indirilmesi işlemi başarılı olduğunda çağrılır. Uygulama ikincil uç noktayı kullanıyorsa bu uç noktayı 20 kereye kadar daha kullanmaya devam eder. 20 kereden sonra uygulama [LocationMode](/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions.locationmode?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_LocationMode) değerini yeniden `PrimaryThenSecondary` olarak ayarlar ve birincil uç noktayı tekrar dener. Bir istek başarılı olursa uygulama birincil uç noktadan okumaya devam eder.
 
```csharp
private static void OperationContextRequestCompleted(object sender, RequestEventArgs e)
{
    if (blobClient.DefaultRequestOptions.LocationMode == LocationMode.SecondaryOnly)
    {
        // You're reading the secondary. Let it read the secondary [secondaryThreshold] times, 
        //    then switch back to the primary and see if it's available now.
        secondaryReadCount++;
        if (secondaryReadCount >= secondaryThreshold)
        {
            blobClient.DefaultRequestOptions.LocationMode = LocationMode.PrimaryThenSecondary;
            secondaryReadCount = 0;
        }
    }
}
```

# <a name="python-tabpython"></a>[Python] (#sekme/python) 

### <a name="retry-event-handler"></a>Yeniden deneme olay işleyicisi

`retry_callback` olay işleyicisi, resmin indirilmesi işlemi başarısız olmuşsa ve yeniden denenecek şekilde ayarlanmışsa çağrılır. Uygulamada tanımlı yeniden deneme sayısı üst sınırına ulaşılmışsa isteğin [LocationMode](https://docs.microsoft.com/en-us/python/api/azure.storage.common.models.locationmode?view=azure-python) değeri `SECONDARY` olarak değiştirilir. Bu ayar, uygulamayı resmi ikincil uç noktadan indirmeyi denemeye zorlar. Birincil uç nokta süresiz olarak yeniden denenmediği için bu yapılandırma resmin istenmesinde harcanan süreyi azaltmış olur.  

```python
def retry_callback(retry_context):
    global retry_count
    retry_count = retry_context.count
    sys.stdout.write("\nRetrying event because of failure reading the primary. RetryCount= {0}".format(retry_count))
    sys.stdout.flush()

    # Check if we have more than n-retries in which case switch to secondary
    if retry_count >= retry_threshold:

        # Check to see if we can fail over to secondary.
        if blob_client.location_mode != LocationMode.SECONDARY:
            blob_client.location_mode = LocationMode.SECONDARY
            retry_count = 0
        else:
            raise Exception("Both primary and secondary are unreachable. "
                            "Check your application's network connection.")
```

### <a name="request-completed-event-handler"></a>İstek tamamlandı olay işleyicisi

`response_callback` olay işleyicisi, resmin indirilmesi işlemi başarılı olduğunda çağrılır. Uygulama ikincil uç noktayı kullanıyorsa bu uç noktayı 20 kereye kadar daha kullanmaya devam eder. 20 kereden sonra uygulama [LocationMode](https://docs.microsoft.com/en-us/python/api/azure.storage.common.models.locationmode?view=azure-python) değerini yeniden `PRIMARY` olarak ayarlar ve birincil uç noktayı tekrar dener. Bir istek başarılı olursa uygulama birincil uç noktadan okumaya devam eder.

```python
def response_callback(response):
    global secondary_read_count
    if blob_client.location_mode == LocationMode.SECONDARY:

        # You're reading the secondary. Let it read the secondary [secondaryThreshold] times,
        # then switch back to the primary and see if it is available now.
        secondary_read_count += 1
        if secondary_read_count >= secondary_threshold:
            blob_client.location_mode = LocationMode.PRIMARY
            secondary_read_count = 0
```

# <a name="java-tabjava"></a>[Java] (#tab/java)

Java ile, **BlobRequestOptions** öğenizin **LocationMode** özelliği, **PRIMARY\_THEN\_SECONDARY** olarak ayarlanırsa geri çağırma işleyicilerinin tanımlanması gerekmez. Bu, uygulamanın **HelloWorld.png** dosyasını indirmeye çalışırken birincil konuma ulaşamaması durumunda otomatik olarak ikincil konuma geçmesine olanak sağlar.

```java
    BlobRequestOptions myReqOptions = new BlobRequestOptions();
    myReqOptions.setRetryPolicyFactory(new RetryLinearRetry(deltaBackOff, maxAttempts));
    myReqOptions.setLocationMode(LocationMode.PRIMARY_THEN_SECONDARY);
    blobClient.setDefaultRequestOptions(myReqOptions);

    blob.downloadToFile(downloadedFile.getAbsolutePath(),null,blobClient.getDefaultRequestOptions(),opContext);
```
---


## <a name="next-steps"></a>Sonraki adımlar

Serinin bu ilk bölümünde RA-GRS depolama hesaplarını kullanarak bir uygulamayı yüksek oranda kullanılabilir hale getirme hakkında bilgi edinip aşağıdakilerin nasıl yapılacağını öğrendiniz:

> [!div class="checklist"]
> * Depolama hesabı oluşturma
> * Örneği indirme
> * Bağlantı dizesini ayarlama
> * Konsol uygulamasını çalıştırma

Bir hata simülasyonu yapıp uygulamanızı ikincil RA-GRS uç noktasını kullanmaya zorlamayı öğrenmek için serinin ikinci bölümüne geçin.

> [!div class="nextstepaction"]
> [Birincil depolama uç noktanız ile bağlantıda bir hata simülasyonu yapma](storage-simulate-failure-ragrs-account-app.md)
