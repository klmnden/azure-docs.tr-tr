---
title: 'Öğretici: Blob storage - Azure depolama ile yüksek oranda kullanılabilir bir uygulama oluşturun'
description: Okuma erişimli coğrafi olarak yedekli depolamayı kullanarak uygulama verilerinizi yüksek oranda kullanılabilir yapma
services: storage
author: tamram
ms.service: storage
ms.topic: tutorial
ms.date: 01/03/2019
ms.author: tamram
ms.custom: mvc
ms.subservice: blobs
ms.openlocfilehash: c4e81d9be09855cde986bfd21f8f688fa7d1341e
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58793728"
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

# <a name="java-v7-sdktabjava-v7"></a>[Java V7 SDK'sı](#tab/java-v7)

* [Maven](https://maven.apache.org/download.cgi)’ı yükleyip komut satırından çalışacak şekilde yapılandırma
* [JDK](https://www.oracle.com/technetwork/java/javase/downloads/index.html)’yı yükleme ve yapılandırma

# <a name="java-v10-sdktabjava-v10"></a>[Java V10 SDK](#tab/java-v10)

* [Maven](http://maven.apache.org/download.cgi)’ı yükleyip komut satırından çalışacak şekilde yapılandırma
* [JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html)’yı yükleme ve yapılandırma

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
   |**Abonelik** | aboneliğiniz |Abonelikleriniz hakkında daha ayrıntılı bilgi için bkz. [Abonelikler](https://account.windowsazure.com/Subscriptions). |
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

# <a name="java-v7-sdktabjava-v7"></a>[Java V7 SDK'sı](#tab/java-v7)

[Örnek projeyi indirin](https://github.com/Azure-Samples/storage-java-ha-ra-grs) ve storage-java-ragrs.zip dosyasını ayıklayın. Geliştirme ortamına uygulamanın bir kopyasını indirmek için [git](https://git-scm.com/) de kullanılabilir. Örnek proje, temel bir Java uygulaması içerir.

```bash
git clone https://github.com/Azure-Samples/storage-java-ha-ra-grs.git
```

# <a name="java-v10-sdktabjava-v10"></a>[Java V10 SDK](#tab/java-v10)

[Örnek projeyi indirin](https://github.com/Azure-Samples/storage-java-V10-ha-ra-grs) ve storage-java-ragrs.zip dosyasını ayıklayın. Geliştirme ortamına uygulamanın bir kopyasını indirmek için [git](https://git-scm.com/) de kullanılabilir. Örnek proje, temel bir Java uygulaması içerir.

```bash
git clone https://github.com/Azure-Samples/storage-java-V10-ha-ra-grs
```

---

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

Uygulamada, depolama hesabınız için bağlantı dizesi sağlamanız gerekir. Uygulamayı çalıştıran yerel makine üzerindeki bu bağlantı dizesi bir ortam değişkeninde depolamanız önerilir. Ortam değişkenini oluşturmak için İşletim Sisteminize bağlı olarak aşağıdaki örneklerden birini izleyin.

Azure portalında depolama hesabınıza gidin. Depolama hesabınızdaki **Ayarlar** bölümünde **Erişim anahtarları**’nı seçin. Birincil veya ikincil anahtardaki **bağlantı dizesini** kopyalayın. İşletim Sisteminize göre aşağıdaki komutlardan birini çalıştırarak \<yourconnectionstring\> değerini kendi bağlantı dizenizle değiştirin. Bu komut, yerel makinede bir ortam değişkeni kaydeder. Yeniden yükleninceye kadar Windows içinde ortam değişkeni kullanılamıyor **komut istemi** veya kullanmakta olduğunuz Kabuk. Aşağıdaki örnekte **\<storageConnectionString\>**’i değiştirin:

### <a name="linux"></a>Linux

```
export storageconnectionstring=\<yourconnectionstring\>
```

### <a name="windows"></a>Windows

```PowerShell
setx storageconnectionstring "\<yourconnectionstring\>"
```

# <a name="pythontabpython"></a>[Python](#tab/python)

Uygulamada, depolama hesabınız için bağlantı dizesi sağlamanız gerekir. Uygulamayı çalıştıran yerel makine üzerindeki bu bağlantı dizesi bir ortam değişkeninde depolamanız önerilir. Ortam değişkenini oluşturmak için İşletim Sisteminize bağlı olarak aşağıdaki örneklerden birini izleyin.

Azure portalında depolama hesabınıza gidin. Depolama hesabınızdaki **Ayarlar** bölümünde **Erişim anahtarları**’nı seçin. Birincil veya ikincil anahtardaki **bağlantı dizesini** kopyalayın. İşletim Sisteminize göre aşağıdaki komutlardan birini çalıştırarak \<yourconnectionstring\> değerini kendi bağlantı dizenizle değiştirin. Bu komut, yerel makinede bir ortam değişkeni kaydeder. Yeniden yükleninceye kadar Windows içinde ortam değişkeni kullanılamıyor **komut istemi** veya kullanmakta olduğunuz Kabuk. Aşağıdaki örnekte **\<storageConnectionString\>**’i değiştirin:

### <a name="linux"></a>Linux

```
export storageconnectionstring=\<yourconnectionstring\>
```

### <a name="windows"></a>Windows

```PowerShell
setx storageconnectionstring "\<yourconnectionstring\>"
```

# <a name="java-v7-sdktabjava-v7"></a>[Java V7 SDK'sı](#tab/java-v7)

Uygulamada, depolama hesabınız için bağlantı dizesi sağlamanız gerekir. Uygulamayı çalıştıran yerel makine üzerindeki bu bağlantı dizesi bir ortam değişkeninde depolamanız önerilir. Ortam değişkenini oluşturmak için İşletim Sisteminize bağlı olarak aşağıdaki örneklerden birini izleyin.

Azure portalında depolama hesabınıza gidin. Depolama hesabınızdaki **Ayarlar** bölümünde **Erişim anahtarları**’nı seçin. Birincil veya ikincil anahtardaki **bağlantı dizesini** kopyalayın. İşletim Sisteminize göre aşağıdaki komutlardan birini çalıştırarak \<yourconnectionstring\> değerini kendi bağlantı dizenizle değiştirin. Bu komut, yerel makinede bir ortam değişkeni kaydeder. Yeniden yükleninceye kadar Windows içinde ortam değişkeni kullanılamıyor **komut istemi** veya kullanmakta olduğunuz Kabuk. Aşağıdaki örnekte **\<storageConnectionString\>**’i değiştirin:

### <a name="linux"></a>Linux

```
export storageconnectionstring=\<yourconnectionstring\>
```

### <a name="windows"></a>Windows

```PowerShell
setx storageconnectionstring "\<yourconnectionstring\>"
```

# <a name="java-v10-sdktabjava-v10"></a>[Java V10 SDK](#tab/java-v10)

Bu örnek, güvenli bir şekilde adını ve anahtarını depolama hesabınızın depolama gerektirir. Bunları örneği çalıştıran makinede yerel ortam değişkenlerini Store. Linux veya Windows örnek, işletim sisteminize bağlı olarak, ortam değişkenlerini oluşturmak için kullanın. Yeniden yükleninceye kadar Windows içinde ortam değişkeni kullanılamıyor **komut istemi** veya kullanmakta olduğunuz Kabuk.

### <a name="linux-example"></a>Linux örneği

```
export AZURE_STORAGE_ACCOUNT="<youraccountname>"
export AZURE_STORAGE_ACCESS_KEY="<youraccountkey>"
```

### <a name="windows-example"></a>Windows örneği

```
setx AZURE_STORAGE_ACCOUNT "<youraccountname>"
setx AZURE_STORAGE_ACCESS_KEY "<youraccountkey>"
```

---

## <a name="run-the-console-application"></a>Konsol uygulamasını çalıştırma

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

Visual Studio'da **F5** veya **Başlat** uygulama hata ayıklamayı başlatmak için. Visual studio otomatik olarak eksik yapılandırdıysanız, NuGet paketlerini geri yüklemeler ziyaret [yükleme ve paket geri yükleme paketleri yeniden yükleme](https://docs.microsoft.com/nuget/consume-packages/package-restore#package-restore-overview) daha fazla bilgi için.

Bir konsol penceresi açılır ve uygulama çalışmaya başlar. Uygulama, çözümdeki **HelloWorld.png** resmini depolama hesabına yükler. Uygulama, resmin ikincil RA-GRS uç noktasında çoğaltıldığını denetler. Ardından, resmi 999 kereye kadar indirmeye başlar. Her okuma tarafından temsil edilen bir **P** veya **S**. Burada, **P** birincil uç nokta ve **S** ikincil uç nokta demektir.

![Çalışan konsol uygulaması](media/storage-create-geo-redundant-storage/figure3.png)

Örnek kodda, [DownloadToFileAsync](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.blob.cloudblob.downloadtofileasync?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudBlob_DownloadToFileAsync_System_String_System_IO_FileMode_Microsoft_WindowsAzure_Storage_AccessCondition_Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_Microsoft_WindowsAzure_Storage_OperationContext_) yöntemini kullanarak depolama hesabından bir resim indirmek için `Program.cs` dosyasındaki `RunCircuitBreakerAsync` görevi kullanılmaktadır. İndirme işlemi öncesinde bir [OperationContext](/dotnet/api/microsoft.windowsazure.storage.operationcontext?view=azure-dotnet) (İşlem Bağlamı) tanımlanır. İşlem bağlamı, indirme işlemi başarıyla tamamlandığında veya indirme işlemi başarısız olup yeniden denendiğinde başlatılan olay işleyicilerini tanımlar.

# <a name="pythontabpython"></a>[Python](#tab/python)

Uygulamayı bir terminalde veya komut isteminde çalıştırmak için **circuitbreaker.py** dizinine gidip `python circuitbreaker.py` komutunu girin. Uygulama, çözümdeki **HelloWorld.png** resmini depolama hesabına yükler. Uygulama, resmin ikincil RA-GRS uç noktasında çoğaltıldığını denetler. Ardından, resmi 999 kereye kadar indirmeye başlar. Her okuma tarafından temsil edilen bir **P** veya **S**. Burada, **P** birincil uç nokta ve **S** ikincil uç nokta demektir.

![Çalışan konsol uygulaması](media/storage-create-geo-redundant-storage/figure3.png)

Örnek kodda, [get_blob_to_path](https://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html) yöntemini kullanarak depolama hesabından bir resim indirmek için `circuitbreaker.py` dosyasındaki `run_circuit_breaker` yöntemi kullanılmaktadır.

Depolama nesnesi yeniden deneme işlevi, doğrusal bir yeniden deneme ilkesine ayarlıdır. Yeniden deneme işlevi, isteklerin yeniden denenip denenmeyeceğini belirler ve isteği yeniden denemeden önce kaç saniye bekleneceğini belirtir. Birincile yapılan istek başarısız olursa aynı isteğin ikincile yeniden denenmesini istiyorsanız **retry\_to\_secondary** değerini true olarak ayarlayın. Örnek uygulamada depolama nesnesinin `retry_callback` işlevinde özel bir yeniden deneme ilkesi tanımlanmıştır.

İndirme işlemi öncesinde Hizmet nesnesinin [retry_callback](https://docs.microsoft.com/python/api/azure.storage.common.storageclient.storageclient?view=azure-python) ve [response_callback](https://docs.microsoft.com/python/api/azure.storage.common.storageclient.storageclient?view=azure-python) işlevleri tanımlanır. Bu işlevler, indirme işlemi başarıyla tamamlandığında veya indirme işlemi başarısız olup yeniden denendiğinde başlatılan olay işleyicilerini tanımlar.

# <a name="java-v7-sdktabjava-v7"></a>[Java V7 SDK'sı](#tab/java-v7)

İndirilen uygulama klasörü kapsamlı bir terminal veya komut istemi açarak uygulamayı çalıştırabilirsiniz. Buradan `mvn compile exec:java` komutunu girerek uygulamayı çalıştırın. Uygulama, dizinden depolama hesabınıza **HelloWorld.png** görüntüsünü yükler ve görüntünün ikincil RA-GRS uç noktasına çoğaltılıp çoğaltılmadığını denetler. Denetim tamamlandıktan sonra uygulama art arda görüntüyü indirmeye başlar, bir yandan da içinden indirme işlemini yaptığı uç noktaya geri bildirimde bulunur.

Depolama nesnesi yeniden deneme işlevi, doğrusal bir yeniden deneme ilkesini kullanacak şekilde ayarlıdır. Yeniden deneme işlevi, isteklerin yeniden denenip denenmeyeceğini belirler ve her bir yeniden denemeden önce kaç saniye bekleneceğini belirtir. **BlobRequestOptions** öğenizin **LocationMode** özelliği, **PRIMARY\_THEN\_SECONDARY** olarak ayarlanır. Bu, uygulamanın **HelloWorld.png** dosyasını indirmeye çalışırken birincil konuma ulaşamaması durumunda otomatik olarak ikincil konuma geçmesine olanak sağlar.

# <a name="java-v10-sdktabjava-v10"></a>[Java V10 SDK](#tab/java-v10)

Örneği çalıştırmak için komut satırında Maven'i kullanın.

1. bir kabuk açın ve **storage-blobs-java-v10-quickstart** komutuyla kopyalanmış dizininize içinde.
2. `mvn compile exec:java` yazın.

Bu örnek, varsayılan dizininizde bir sınama dosyası oluşturur, bu dizin için windows kullanıcıları **AppData\Local\Temp**. Örnek daha sonra girebilirsiniz komutların aşağıdaki seçenekler sunar:

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

---

## <a name="understand-the-sample-code"></a>Örnek kodu anlama

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

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

# <a name="java-v7-sdktabjava-v7"></a>[Java V7 SDK'sı](#tab/java-v7)

Java ile, **BlobRequestOptions** öğenizin **LocationMode** özelliği, **PRIMARY\_THEN\_SECONDARY** olarak ayarlanırsa geri çağırma işleyicilerinin tanımlanması gerekmez. Bu, uygulamanın **HelloWorld.png** dosyasını indirmeye çalışırken birincil konuma ulaşamaması durumunda otomatik olarak ikincil konuma geçmesine olanak sağlar.

```java
    BlobRequestOptions myReqOptions = new BlobRequestOptions();
    myReqOptions.setRetryPolicyFactory(new RetryLinearRetry(deltaBackOff, maxAttempts));
    myReqOptions.setLocationMode(LocationMode.PRIMARY_THEN_SECONDARY);
    blobClient.setDefaultRequestOptions(myReqOptions);

    blob.downloadToFile(downloadedFile.getAbsolutePath(),null,blobClient.getDefaultRequestOptions(),opContext);
```

# <a name="java-v10-sdktabjava-v10"></a>[Java V10 SDK](#tab/java-v10)

Java V10 SDK'sı ile geri çağırma işleyicilerinin tanımlanması hala gereksizdir ve SDK'sı artık V7 SDK'dan bazı temel farklar vardır. İkincil sahibiz LocationMode yerine **işlem hattı**. İkincil bir işlem hattı aracılığıyla tanımlayabilir **RequestRetryOptions** ve tanımlanmışsa, verilerinizi birincil ardışık düzeninden ulaşmak başarısız olursa otomatik olarak ikincil ardışık düzenine geçmek uygulama izin verir.

```java
// We create pipeline options here so that they can be easily used between different pipelines
PipelineOptions myOptions = new PipelineOptions();
myOptions.withRequestRetryOptions(new RequestRetryOptions(RetryPolicyType.EXPONENTIAL, 3, 10, 500L, 1000L, accountName + "-secondary.blob.core.windows.net"));
// We are using a default pipeline here, you can learn more about it at https://github.com/Azure/azure-storage-java/wiki/Azure-Storage-Java-V10-Overview
final ServiceURL serviceURL = new ServiceURL(new URL("https://" + accountName + ".blob.core.windows.net"), StorageURL.createPipeline(creds, myOptions));
```

---

## <a name="next-steps"></a>Sonraki adımlar

Bölümünde bir dizi uygulamaya RA-GRS depolama hesapları ile yüksek oranda kullanılabilir yapma hakkında bilgi edindiniz.

Bir hata simülasyonu yapıp uygulamanızı ikincil RA-GRS uç noktasını kullanmaya zorlamayı öğrenmek için serinin ikinci bölümüne geçin.

> [!div class="nextstepaction"]
> [Birincil depolama uç noktanız ile bağlantıda bir hata simülasyonu yapma](storage-simulate-failure-ragrs-account-app.md)
