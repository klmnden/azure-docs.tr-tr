---
title: 'Öğretici: Blob storage - Azure depolama ile yüksek oranda kullanılabilir bir uygulama oluşturun'
description: Okuma erişimli coğrafi olarak yedekli depolamayı kullanarak uygulama verilerinizi yüksek oranda kullanılabilir yapma
services: storage
author: tamram
ms.service: storage
ms.topic: tutorial
ms.date: 01/03/2019
ms.author: tamram
ms.reviewer: artek
ms.custom: mvc
ms.subservice: blobs
ms.openlocfilehash: 24869981595cd68eb833f7b176e17a2683127945
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65752425"
---
# <a name="tutorial-build-a-highly-available-application-with-blob-storage"></a>Öğretici: Blob Depolama ile yüksek oranda kullanılabilir bir uygulama oluşturun

Bu öğretici, bir dizinin birinci bölümüdür. İçinde uygulama verilerinizi azure'da yüksek oranda kullanılabilir hale getirme hakkında bilgi edinin.

Bu öğreticiyi tamamladıktan sonra yükler ve bir blobun alan bir konsol uygulaması gerekir bir [okuma erişimli coğrafi olarak yedekli](../common/storage-redundancy-grs.md#read-access-geo-redundant-storage) (RA-GRS) depolama hesabı.

RA-GRS, işlem birincil bir bölgeden ikincil bir bölgeye çoğaltarak çalışır. Bu çoğaltma işlemi, ikincil bölgedeki verilerin nihai olarak tutarlı olmasını sağlar. Uygulamanın kullandığı [devre kesici](/azure/architecture/patterns/circuit-breaker) belirlemek hangi uç noktaya bağlanmak için uç noktalar olarak hataları arasında otomatik olarak geçiş desen ve kurtarmalar benzetimi.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

Serinin birinci bölümünde şunları öğrenirsiniz:

> [!div class="checklist"]
> * Depolama hesabı oluşturma
> * Bağlantı dizesini ayarlama
> * Konsol uygulamasını çalıştırma

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için:

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

* [Visual Studio 2017](https://www.visualstudio.com/downloads/)’yi aşağıdaki iş yükleri ile yükleyin:
  - **Azure geliştirme**

  ![Azure geliştirme (Web ve Bulut altında)](media/storage-create-geo-redundant-storage/workloads.png)

# <a name="pythontabpython"></a>[Python](#tab/python)

* [Python](https://www.python.org/downloads/)’ı yükleyin
* [Python için Azure Depolama SDK’sını](https://github.com/Azure/azure-storage-python) indirip yükleme

# <a name="java-v10-sdktabjava-v10"></a>[Java V10 SDK](#tab/java-v10)

* [Maven](https://maven.apache.org/download.cgi)’ı yükleyip komut satırından çalışacak şekilde yapılandırma
* [JDK](https://www.oracle.com/technetwork/java/javase/downloads/index.html)’yı yükleme ve yapılandırma

# <a name="nodejstabnodejs"></a>[Node.js](#tab/nodejs)

* Yükleme [Node.js](https://nodejs.org).

---

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Depolama hesabı, Azure Storage veri nesnelerinizi depolamak ve erişmek için benzersiz bir ad alanı sağlar.

Okuma erişimli coğrafi olarak yedekli depolama hesabı oluşturmak için aşağıdaki adımları izleyin:

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** düğmesini seçin.
2. Seçin **depolama** gelen **yeni** sayfası.
3. Seçin **depolama hesabı - blob, dosya, tablo, kuyruk** altında **öne çıkan**.
4. Aşağıdaki bilgileri kullanarak depolama hesabı formunu alttaki resimde gösterildiği gibi doldurun ve **Oluştur**’u seçin:

   | Ayar       | Önerilen değer | Açıklama |
   | ------------ | ------------------ | ------------------------------------------------- |
   | **Ad** | mystorageaccount | Depolama hesabınız için benzersiz bir değer |
   | **Dağıtım modeli** | Resource Manager  | Resource Manager en son özellikleri içerir.|
   | **Hesap türü** | StorageV2 | Hesap türleri hakkında ayrıntılı bilgi almak için bkz. [depolama hesabı türleri](../common/storage-introduction.md#types-of-storage-accounts) |
   | **Performans** | Standart | Standart, örnek senaryo için yeterli olacaktır. |
   | **Çoğaltma**| Okuma erişimli coğrafi olarak yedekli depolama (RA-GRS) | Örneğin çalışması için bunun seçilmesi gereklidir. |
   |**Abonelik** | aboneliğiniz |Abonelikleriniz hakkında daha ayrıntılı bilgi için bkz. [Abonelikler](https://account.azure.com/Subscriptions). |
   |**ResourceGroup** | myResourceGroup |Geçerli kaynak grubu adları için bkz. [Adlandırma kuralları ve kısıtlamalar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). |
   |**Konum** | Doğu ABD | Konum seçin. |

![depolama hesabı oluşturma](media/storage-create-geo-redundant-storage/createragrsstracct.png)

## <a name="download-the-sample"></a>Örneği indirme

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

[Örnek projeyi indirin](https://github.com/Azure-Samples/storage-dotnet-circuit-breaker-pattern-ha-apps-using-ra-grs/archive/master.zip) ve storage-dotnet-circuit-breaker-pattern-ha-apps-using-ra-grs.zip dosyasını ayıklayın (sıkıştırmasını açın). Geliştirme ortamına uygulamanın bir kopyasını indirmek için [git](https://git-scm.com/) de kullanılabilir. Örnek proje bir konsol uygulaması içerir.

```bash
git clone https://github.com/Azure-Samples/storage-dotnet-circuit-breaker-pattern-ha-apps-using-ra-grs.git
```

# <a name="pythontabpython"></a>[Python](#tab/python)

[Örnek projeyi indirin](https://github.com/Azure-Samples/storage-python-circuit-breaker-pattern-ha-apps-using-ra-grs/archive/master.zip) ve storage-python-circuit-breaker-pattern-ha-apps-using-ra-grs.zip dosyasını ayıklayın (sıkıştırmasını açın). Geliştirme ortamına uygulamanın bir kopyasını indirmek için [git](https://git-scm.com/) de kullanılabilir. Örnek proje temel bir Python uygulaması içerir.

```bash
git clone https://github.com/Azure-Samples/storage-python-circuit-breaker-pattern-ha-apps-using-ra-grs.git
```

# <a name="java-v10-sdktabjava-v10"></a>[Java V10 SDK](#tab/java-v10)

[Örnek projeyi indirin](https://github.com/Azure-Samples/storage-java-V10-ha-ra-grs) ve storage-java-ragrs.zip dosyasını ayıklayın. Geliştirme ortamına uygulamanın bir kopyasını indirmek için [git](https://git-scm.com/) de kullanılabilir. Örnek proje, temel bir Java uygulaması içerir.

```bash
git clone https://github.com/Azure-Samples/storage-java-V10-ha-ra-grs
```

# <a name="nodejstabnodejs"></a>[Node.js](#tab/nodejs)

[Örnek projeyi indirin](https://github.com/Azure-Samples/storage-node-v10-ha-ra-grs) ve dosyanın sıkıştırmasını açın. Geliştirme ortamına uygulamanın bir kopyasını indirmek için [git](https://git-scm.com/) de kullanılabilir. Örnek Proje temel bir Node.js uygulaması içerir.

```bash
git clone https://github.com/Azure-Samples/storage-java-V10-ha-ra-grs
```

---

## <a name="configure-the-sample"></a>Örnek yapılandırma

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

Uygulamada, depolama hesabınız için bağlantı dizesi sağlamanız gerekir. Bu bağlantı dizesini uygulamayı çalıştıran yerel makine üzerindeki bir ortam değişkeninde depolayabilirsiniz. Ortam değişkenini oluşturmak için İşletim Sisteminize bağlı olarak aşağıdaki örneklerden birini izleyin.

Azure portalında depolama hesabınıza gidin. Depolama hesabınızdaki **Ayarlar** bölümünde **Erişim anahtarları**’nı seçin. Birincil veya ikincil anahtardaki **bağlantı dizesini** kopyalayın. İşletim sisteminize göre aşağıdaki komutlardan birini çalıştırın değiştirerek \<yourconnectionstring\> gerçek bağlantı dizenizle. Bu komut, yerel makinede bir ortam değişkeni kaydeder. Yeniden yükleninceye kadar Windows içinde ortam değişkeni kullanılamıyor **komut istemi** veya kullanmakta olduğunuz Kabuk.

### <a name="linux"></a>Linux

```
export storageconnectionstring=<yourconnectionstring>
```

### <a name="windows"></a>Windows

```powershell
setx storageconnectionstring "<yourconnectionstring>"
```

# <a name="pythontabpython"></a>[Python](#tab/python)

Uygulamada, depolama hesabı kimlik bilgilerinizi sağlamanız gerekir. Bu bilgiler, uygulamayı çalıştıran yerel makine üzerinde ortam değişkenleri içindeki depolayabilirsiniz. Ortam değişkenlerini oluşturmak için işletim sisteminize bağlı olarak aşağıdaki örneklerden birini izleyin.

Azure portalında depolama hesabınıza gidin. Depolama hesabınızdaki **Ayarlar** bölümünde **Erişim anahtarları**’nı seçin. Yapıştırma **depolama hesabı adı** ve **anahtarı** değerlerini değiştirerek aşağıdaki komutları \<youraccountname\> ve \<accountkey\>yer tutucu. Bu komut, yerel makineye ortam değişkenlerini kaydeder. Yeniden yükleninceye kadar Windows içinde ortam değişkeni kullanılamıyor **komut istemi** veya kullanmakta olduğunuz Kabuk.

### <a name="linux"></a>Linux

```
export accountname=<youraccountname>
export accountkey=<youraccountkey>
```

### <a name="windows"></a>Windows

```powershell
setx accountname "<youraccountname>"
setx accountkey "<youraccountkey>"
```

# <a name="java-v10-sdktabjava-v10"></a>[Java V10 SDK](#tab/java-v10)

Bu örnek, güvenli bir şekilde adını ve anahtarını depolama hesabınızın depolama gerektirir. Bunları örneği çalıştıran makinede yerel ortam değişkenlerini Store. Linux veya Windows örnek, işletim sisteminize bağlı olarak, ortam değişkenlerini oluşturmak için kullanın. Yeniden yükleninceye kadar Windows içinde ortam değişkeni kullanılamıyor **komut istemi** veya kullanmakta olduğunuz Kabuk.

### <a name="linux-example"></a>Linux örneği

```
export AZURE_STORAGE_ACCOUNT="<youraccountname>"
export AZURE_STORAGE_ACCESS_KEY="<youraccountkey>"
```

### <a name="windows-example"></a>Windows örneği

```powershell
setx AZURE_STORAGE_ACCOUNT "<youraccountname>"
setx AZURE_STORAGE_ACCESS_KEY "<youraccountkey>"
```

# <a name="nodejstabnodejs"></a>[Node.js](#tab/nodejs)

Bu örneği çalıştırmak için depolama hesabı kimlik bilgilerinizle eklemelisiniz `.env.example` yeniden adlandırın ve dosya `.env`.

```
AZURE_STORAGE_ACCOUNT_NAME=<replace with your storage account name>
AZURE_STORAGE_ACCOUNT_ACCESS_KEY=<replace with your storage account access key>
```

Bu bilgiler, depolama hesabınıza gidin ve seçerek Azure portalında bulabilirsiniz **erişim anahtarları** içinde **ayarları** bölümü.

Gerekli bağımlılıkları da yüklemeniz gerekir. Bunu yapmak için bir komut istemi açın, örnek klasörüne gidin ve sonra girin `npm install`.

---

## <a name="run-the-console-application"></a>Konsol uygulamasını çalıştırma

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

Visual Studio'da **F5** veya **Başlat** uygulama hata ayıklamayı başlatmak için. Visual studio otomatik olarak eksik yapılandırdıysanız, NuGet paketlerini geri yüklemeler ziyaret [yükleme ve paket geri yükleme paketleri yeniden yükleme](https://docs.microsoft.com/nuget/consume-packages/package-restore#package-restore-overview) daha fazla bilgi için.

Bir konsol penceresi açılır ve uygulama çalışmaya başlar. Uygulama, çözümdeki **HelloWorld.png** resmini depolama hesabına yükler. Uygulama, resmin ikincil RA-GRS uç noktasında çoğaltıldığını denetler. Ardından, resmi 999 kereye kadar indirmeye başlar. Her okuma tarafından temsil edilen bir **P** veya **S**. Burada, **P** birincil uç nokta ve **S** ikincil uç nokta demektir.

![Çalışan konsol uygulaması](media/storage-create-geo-redundant-storage/figure3.png)

Örnek kodda, [DownloadToFileAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblob.downloadtofileasync) yöntemini kullanarak depolama hesabından bir resim indirmek için `Program.cs` dosyasındaki `RunCircuitBreakerAsync` görevi kullanılmaktadır. İndirme işlemi öncesinde bir [OperationContext](/dotnet/api/microsoft.azure.cosmos.table.operationcontext) (İşlem Bağlamı) tanımlanır. İşlem bağlamı, indirme işlemi başarıyla tamamlandığında veya indirme işlemi başarısız olup yeniden denendiğinde başlatılan olay işleyicilerini tanımlar.

# <a name="pythontabpython"></a>[Python](#tab/python)

Uygulamayı bir terminalde veya komut isteminde çalıştırmak için **circuitbreaker.py** dizinine gidip `python circuitbreaker.py` komutunu girin. Uygulama, çözümdeki **HelloWorld.png** resmini depolama hesabına yükler. Uygulama, resmin ikincil RA-GRS uç noktasında çoğaltıldığını denetler. Ardından, resmi 999 kereye kadar indirmeye başlar. Her okuma tarafından temsil edilen bir **P** veya **S**. Burada, **P** birincil uç nokta ve **S** ikincil uç nokta demektir.

![Çalışan konsol uygulaması](media/storage-create-geo-redundant-storage/figure3.png)

Örnek kodda, [get_blob_to_path](https://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html) yöntemini kullanarak depolama hesabından bir resim indirmek için `circuitbreaker.py` dosyasındaki `run_circuit_breaker` yöntemi kullanılmaktadır.

Depolama nesnesi yeniden deneme işlevi, doğrusal bir yeniden deneme ilkesine ayarlıdır. Yeniden deneme işlevi, isteklerin yeniden denenip denenmeyeceğini belirler ve isteği yeniden denemeden önce kaç saniye bekleneceğini belirtir. Birincile yapılan istek başarısız olursa aynı isteğin ikincile yeniden denenmesini istiyorsanız **retry\_to\_secondary** değerini true olarak ayarlayın. Örnek uygulamada depolama nesnesinin `retry_callback` işlevinde özel bir yeniden deneme ilkesi tanımlanmıştır.

İndirme işlemi öncesinde Hizmet nesnesinin [retry_callback](https://docs.microsoft.com/python/api/azure.storage.common.storageclient.storageclient?view=azure-python) ve [response_callback](https://docs.microsoft.com/python/api/azure.storage.common.storageclient.storageclient?view=azure-python) işlevleri tanımlanır. Bu işlevler, indirme işlemi başarıyla tamamlandığında veya indirme işlemi başarısız olup yeniden denendiğinde başlatılan olay işleyicilerini tanımlar.

# <a name="java-v10-sdktabjava-v10"></a>[Java V10 SDK](#tab/java-v10)

Örneği çalıştırmak için komut satırında Maven'i kullanın.

1. bir kabuk açın ve **storage-blobs-java-v10-quickstart** komutuyla kopyalanmış dizininize içinde.
2. `mvn compile exec:java` yazın.

Bu örnek, varsayılan dizininizde bir sınama dosyası oluşturur. Windows kullanıcıları için bu dizindir **AppData\Local\Temp**. Örnek daha sonra girebilirsiniz komutların aşağıdaki seçenekler sunar:

- Girin **P** blob koyma işlemi yürütmek için bu geçici bir dosya, depolama hesabına yükler.
- Girin **L** blobu listeleme işlemi gerçekleştirmek için şu anda kapsayıcıdaki blobları listeler.
- Girin **G** blob alma işlemi gerçekleştirmek için bu dosya depolama hesabınızdan yerel makinenize indirir.
- Girin **D** blob silme işlemi yürütmek için bu blob depolama hesabınızdan siler.
- Girin **E** örnek kapatmak için bu da tüm kaynakları oluşturulan örnek siler.

Bu örnekte uygulamayı Windows'da çalıştırdığınızda elde edeceğiniz çıkış gösterilmiştir.

```
Created quickstart container
Enter a command
(P)utBlob | (L)istBlobs | (G)etBlob | (D)eleteBlobs | (E)xitSample
# Enter a command :
P
Uploading the sample file into the container: https://<storageaccount>.blob.core.windows.net/quickstart
# Enter a command :
L
Listing blobs in the container: https://<storageaccount>.blob.core.windows.net/quickstart
Blob name: SampleBlob.txt
# Enter a command :
G
Get the blob: https://<storageaccount>.blob.core.windows.net/quickstart/SampleBlob.txt
The blob was downloaded to C:\Users\<useraccount>\AppData\Local\Temp\downloadedFile13097087873115855761.txt
# Enter a command :
D
Delete the blob: https://<storageaccount>.blob.core.windows.net/quickstart/SampleBlob.txt

# Enter a command :
>> Blob deleted: https://<storageaccount>.blob.core.windows.net/quickstart/SampleBlob.txt
E
Cleaning up the sample and exiting!
```

Örneğin denetimi sizdedir; bu nedenle, kodu çalıştırmasını sağlamak için komutları girin. Girdi büyük/küçük harfe duyarlıdır.

# <a name="nodejstabnodejs"></a>[Node.js](#tab/nodejs)

Örneği çalıştırmak için bir komut istemi açın, örnek klasörüne gidin ve sonra girin `node index.js`.

Örnek, Blob Depolama hesabında bir kapsayıcı oluşturur, yükler **HelloWorld.png** kapsayıcıya daha sonra tekrar tekrar ikincil bölgeye kapsayıcı ve görüntü çoğaltılıp çoğaltılmadığını denetler. Çoğaltma işleminden sonra girmesini ister **D** veya **Q** (ve sonra ENTER) indirin veya çıkmak için. Çıkış aşağıdaki örneğe benzer olmalıdır:

```
Created container successfully: newcontainer1550799840726
Uploaded blob: HelloWorld.png
Checking to see if container and blob have replicated to secondary region.
[0] Container has not replicated to secondary region yet: newcontainer1550799840726 : ContainerNotFound
[1] Container has not replicated to secondary region yet: newcontainer1550799840726 : ContainerNotFound
...
[31] Container has not replicated to secondary region yet: newcontainer1550799840726 : ContainerNotFound
[32] Container found, but blob has not replicated to secondary region yet.
...
[67] Container found, but blob has not replicated to secondary region yet.
[68] Blob has replicated to secondary region.
Ready for blob download. Enter (D) to download or (Q) to quit, followed by ENTER.
> D
Attempting to download blob...
Blob downloaded from primary endpoint.
> Q
Exiting...
Deleted container newcontainer1550799840726
```

---

## <a name="understand-the-sample-code"></a>Örnek kodu anlama

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

### <a name="retry-event-handler"></a>Yeniden deneme olay işleyicisi

`OperationContextRetrying` olay işleyicisi, resmin indirilmesi işlemi başarısız olmuşsa ve yeniden denenecek şekilde ayarlanmışsa çağrılır. Uygulamada tanımlı yeniden deneme sayısı üst sınırına ulaşılmışsa isteğin [LocationMode](/dotnet/api/microsoft.azure.storage.blob.blobrequestoptions.locationmode) değeri `SecondaryOnly` olarak değiştirilir. Bu ayar, uygulamayı resmi ikincil uç noktadan indirmeyi denemeye zorlar. Birincil uç nokta süresiz olarak yeniden denenmediği için bu yapılandırma resmin istenmesinde harcanan süreyi azaltmış olur.

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

`OperationContextRequestCompleted` olay işleyicisi, resmin indirilmesi işlemi başarılı olduğunda çağrılır. Uygulama ikincil uç noktayı kullanıyorsa bu uç noktayı 20 kereye kadar daha kullanmaya devam eder. 20 kereden sonra uygulama [LocationMode](/dotnet/api/microsoft.azure.storage.blob.blobrequestoptions.locationmode) değerini yeniden `PrimaryThenSecondary` olarak ayarlar ve birincil uç noktayı tekrar dener. Bir istek başarılı olursa uygulama birincil uç noktadan okumaya devam eder.

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

# <a name="pythontabpython"></a>[Python](#tab/python)

### <a name="retry-event-handler"></a>Yeniden deneme olay işleyicisi

`retry_callback` olay işleyicisi, resmin indirilmesi işlemi başarısız olmuşsa ve yeniden denenecek şekilde ayarlanmışsa çağrılır. Uygulamada tanımlı yeniden deneme sayısı üst sınırına ulaşılmışsa isteğin [LocationMode](https://docs.microsoft.com/python/api/azure.storage.common.models.locationmode?view=azure-python) değeri `SECONDARY` olarak değiştirilir. Bu ayar, uygulamayı resmi ikincil uç noktadan indirmeyi denemeye zorlar. Birincil uç nokta süresiz olarak yeniden denenmediği için bu yapılandırma resmin istenmesinde harcanan süreyi azaltmış olur.

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

`response_callback` olay işleyicisi, resmin indirilmesi işlemi başarılı olduğunda çağrılır. Uygulama ikincil uç noktayı kullanıyorsa bu uç noktayı 20 kereye kadar daha kullanmaya devam eder. 20 kereden sonra uygulama [LocationMode](https://docs.microsoft.com/python/api/azure.storage.common.models.locationmode?view=azure-python) değerini yeniden `PRIMARY` olarak ayarlar ve birincil uç noktayı tekrar dener. Bir istek başarılı olursa uygulama birincil uç noktadan okumaya devam eder.

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

# <a name="java-v10-sdktabjava-v10"></a>[Java V10 SDK](#tab/java-v10)

Java V10 SDK'sı ile geri çağırma işleyicilerinin tanımlanması ve SDK'sı artık V7 SDK'dan bazı temel farklar vardır. İkincil sahibiz LocationMode yerine **işlem hattı**. İkincil bir işlem hattı aracılığıyla tanımlayabilir **RequestRetryOptions** ve tanımlanmışsa, verilerinizi birincil ardışık düzeninden ulaşmak başarısız olursa otomatik olarak ikincil ardışık düzenine geçmek uygulama izin verir.

```java
// We create pipeline options here so that they can be easily used between different pipelines
PipelineOptions myOptions = new PipelineOptions();
myOptions.withRequestRetryOptions(new RequestRetryOptions(RetryPolicyType.EXPONENTIAL, 3, 10, 500L, 1000L, accountName + "-secondary.blob.core.windows.net"));
// We are using a default pipeline here, you can learn more about it at https://github.com/Azure/azure-storage-java/wiki/Azure-Storage-Java-V10-Overview
final ServiceURL serviceURL = new ServiceURL(new URL("https://" + accountName + ".blob.core.windows.net"), StorageURL.createPipeline(creds, myOptions));
```

# <a name="nodejstabnodejs"></a>[Node.js](#tab/nodejs)

Node.js V10 SDK ile geri çağırma işleyicilerinin gereksizdir. Bunun yerine, örnek yeniden deneme seçeneklerini ve ikincil uç noktaya ile yapılandırılmış bir işlem hattı oluşturur. Bu uygulamanın verilerinizi birincil ardışık düzeninden ulaşmak başarısız olursa otomatik olarak ikincil ardışık düzenine geçmek sağlar.

```javascript
const accountName = process.env.AZURE_STORAGE_ACCOUNT_NAME;
const storageAccessKey = process.env.AZURE_STORAGE_ACCOUNT_ACCESS_KEY;
const sharedKeyCredential = new SharedKeyCredential(accountName, storageAccessKey);

const primaryAccountURL = `https://${accountName}.blob.core.windows.net`;
const secondaryAccountURL = `https://${accountName}-secondary.blob.core.windows.net`;

const pipeline = StorageURL.newPipeline(sharedKeyCredential, {
  retryOptions: {
    maxTries: 3,
    tryTimeoutInMs: 10000,
    retryDelayInMs: 500,
    maxRetryDelayInMs: 1000,
    secondaryHost: secondaryAccountURL
  }
});
```

---

## <a name="next-steps"></a>Sonraki adımlar

Bölümünde bir dizi uygulamaya RA-GRS depolama hesapları ile yüksek oranda kullanılabilir yapma hakkında bilgi edindiniz.

Bir hata simülasyonu yapıp uygulamanızı ikincil RA-GRS uç noktasını kullanmaya zorlamayı öğrenmek için serinin ikinci bölümüne geçin.

> [!div class="nextstepaction"]
> [Birincil depolama uç noktanız ile bağlantıda bir hata simülasyonu yapma](storage-simulate-failure-ragrs-account-app.md)
