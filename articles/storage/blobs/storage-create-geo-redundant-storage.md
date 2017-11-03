---
title: "Uygulama verilerini azure'da yüksek oranda kullanılabilir hale | Microsoft Docs"
description: "Coğrafi olarak yedekli depolamaya okuma erişimi uygulama verilerinizin yüksek oranda kullanılabilir yapmak için kullanın"
services: storage
documentationcenter: 
author: georgewallace
manager: timlt
editor: 
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: csharp
ms.topic: tutorial
ms.date: 10/12/2017
ms.author: gwallace
ms.custom: mvc
ms.openlocfilehash: 547ca7843f53bd11fdb922af8e0ae77e38f813d9
ms.sourcegitcommit: a7c01dbb03870adcb04ca34745ef256414dfc0b3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2017
---
# <a name="make-your-application-data-highly-available-with-azure-storage"></a>Uygulama verilerinizi Azure storage ile yüksek oranda kullanılabilir yap

Bu öğretici bir dizi birini bir parçasıdır. Bu öğretici, uygulama verilerini azure'da yüksek oranda kullanılabilir hale gösterilmiştir. İşlemi tamamladığınızda, bir blob alır ve yükler bir konsol uygulamasına sahip bir [okuma erişimli coğrafi olarak yedekli](../common/storage-redundancy.md#read-access-geo-redundant-storage) (RA-GRS) depolama hesabı. RA-GRS işlemleri birincil sunucudan ikincil bölge'ye yineleyerek çalışır. Bu çoğaltma işlemi, verileri ikincil bölge sonuçta tutarlı olmasını sağlar. Uygulamanın kullandığı [devre kesici](/azure/architecture/patterns/circuit-breaker.md) bağlanmak için hangi uç noktaya belirlemek için desen. Hata benzetimi sırasında uygulama ikincil uç noktasına geçer.

Bölümünde bir dizi öğrenin nasıl yapılır:

> [!div class="checklist"]
> * Depolama hesabı oluşturma
> * Örneği indirme
> * Bağlantı dizesi ayarlama
> * Konsol uygulamasını çalıştırın

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için:

* [Visual Studio 2017](https://www.visualstudio.com/downloads/)’yi aşağıdaki iş yükleri ile yükleyin:
  - **Azure geliştirme**

  ![Azure geliştirilme (Web ve bulut)](media/storage-create-geo-redundant-storage/workloads.png)

* İndirme ve yükleme [Fiddler](https://www.telerik.com/download/fiddler)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="log-in-to-the-azure-portal"></a>Azure portalında oturum açma

[Azure Portal](https://portal.azure.com/)’da oturum açın.

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Bir depolama hesabı depolamak ve Azure storage veri nesnelerinizi erişmek için benzersiz bir ad alanı sağlar.

Okuma erişimli coğrafi olarak yedekli depolama hesabı oluşturmak için aşağıdaki adımları izleyin:

1. Seçin **yeni** düğme Azure portalında sol üst köşesinde bulundu.

2. Seçin **depolama** gelen **yeni** sayfasında ve seçin **depolama hesabı - blob, dosya, tablo, kuyruk** altında **öne çıkan**.
3. Aşağıdaki görüntü ve select gösterildiği gibi aşağıdaki bilgileri ile depolama hesabı formu doldurun **oluşturma**:

   | Ayar       | Önerilen değer | Açıklama |
   | ------------ | ------------------ | ------------------------------------------------- |
   | **Ad** | mystorageaccount | Depolama hesabınız için benzersiz bir değer |
   | **Dağıtım modeli** | Resource Manager  | Kaynak Yöneticisi'ni, en son özellikler içerir.  |
   | **Hesap türü** | Genel amaçlı | Hesap türleri hakkında daha fazla bilgi için bkz: [türlerde depolama hesapları](../common/storage-introduction.md#types-of-storage-accounts) |
   | **Performans** | Standart | Standart Örnek senaryo için yeterli olur. |
   | **Çoğaltma**| Okuma erişimli coğrafi olarak yedekli depolama (RA-GRS) | Bu örnek çalışması gereklidir. |
   |**Gerekli güvenli aktarımı** | Devre dışı| Güvenli aktarımı bu senaryo için gerekli değildir. |
   |**Abonelik** | Aboneliğinizi |Abonelikleriniz hakkında daha ayrıntılı bilgi için bkz. [Abonelikler](https://account.windowsazure.com/Subscriptions). |
   |**Kaynak grubu** | myResourceGroup |Geçerli kaynak grubu adları için bkz. [Adlandırma kuralları ve kısıtlamalar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). |
   |**Konum** | Doğu ABD | Bir konum seçin. |

![Depolama hesabı oluşturma](media/storage-create-geo-redundant-storage/figure1.png)

## <a name="download-the-sample"></a>Örneği indirme

[Örnek Proje indirme](https://github.com/Azure-Samples/storage-dotnet-circuit-breaker-pattern-ha-apps-using-ra-grs/archive/master.zip).

Extract (sıkıştırmasını açın) storage-dotnet-circuit-breaker-pattern-ha-apps-using-ra-grs.zip dosyası.
Örnek Proje bir konsol uygulaması içerir.

## <a name="set-the-connection-string"></a>Bağlantı dizesi ayarlama

Açık *storage-dotnet-circuit-breaker-pattern-ha-apps-using-ra-grs* konsol uygulaması Visual Studio'da.

Altında **appSettings** düğümünde **App.config** dosya, değerini _StorageConnectionString_ depolama hesabı bağlantı dizesi ile. Bu değer seçerek alınır **erişim anahtarları** altında **ayarları** Azure portalında depolama hesabınızdaki. Kopya **bağlantı dizesi** birincil veya ikincil anahtarı ve yapıştırın **App.config** dosya. Seçin **kaydetmek**, tamamlandıktan sonra dosyayı kaydetmek için.

![Uygulama yapılandırma dosyası](media/storage-create-geo-redundant-storage/figure2.png)

## <a name="run-the-console-application"></a>Konsol uygulamasını çalıştırın

Visual Studio'da basın **F5** veya seçin **Başlat** uygulama hata ayıklama başlatılamıyor. Visual studio otomatik olarak eksik Nuget paketlerini geri yüklemeler yapılandırdıysanız, ziyaret etmek için [yükleme ve paket geri yüklemesi paketlerle yeniden yükleme](https://docs.microsoft.com/nuget/consume-packages/package-restore#package-restore-overview) daha fazla bilgi için. 

Bir konsol penceresi açılır ve uygulama başlar çalışıyor. Uygulamayı yükler **HelloWorld.png** çözümü bir görüntüden depolama hesabı. Uygulama görüntüsü emin olmak için ikincil RA-GRS uç noktasına çoğaltıldığını denetler. Ardından, 999 kereye kadar görüntü yükleme başlar. Her okuma tarafından representated bir **P** veya **S**. Burada **P** birincil uç noktasını temsil eder ve **S** ikincil uç noktasını temsil eder.

![Çalışan konsol uygulaması](media/storage-create-geo-redundant-storage/figure3.png)

Örnek kodda `RunCircuitBreakerAsync` içinde görev `Program.cs` dosyası kullanarak depolama hesabı bir görüntüsünü karşıdan yüklemek için kullanılan [DownloadToFileAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob.downloadtofileasync?view=azure-dotnet) yöntemi. İndirme önce bir [OperationContext](/dotnet/api/microsoft.windowsazure.storage.operationcontext?view=azure-dotnet) tanımlanır. Bir yükleme işlemi başarıyla tamamlandığında veya bir yükleme başarısız olur ve olay işleyicileri, işlem bağlamı tanımlar yeniden deneniyor.

### <a name="retry-event-handler"></a>Olay işleyicisi yeniden deneyin

`Operation_context_Retrying` Olay işleyicisi çağrılır görüntü yükleme başarısız olur ve çok rety ayarlayın. Uygulama içinde tanımlanmış olan yeniden deneme sayısı üst sınırına, [LocationMode](/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions.locationmode?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_LocationMode) isteği değiştirilir `SecondaryOnly`. Bu ayar, ikincil uç noktasından görüntü indirmeye uygulamaya zorlar. Bu yapılandırma, birincil endpoint süresiz olarak denenmez gibi görüntü istemek için harcanan süre azaltır.

```csharp
private static void Operation_context_Retrying(object sender, RequestEventArgs e)
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

### <a name="request-completed-event-handler"></a>Tamamlanan olay işleyicisi isteği

`Operation_context_RequestCompleted` Olay işleyicisi görüntü yükleme başarılı olduğunda çağrılır. Uygulama ikincil uç kullanıyorsanız, uygulama Bu uç noktaya kadar 20 kez kullanmaya devam eder. 20 kez sonra uygulama ayarlar [LocationMode](/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions.locationmode?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_LocationMode) geri `PrimaryThenSecondary` ve birincil uç nokta yeniden dener. İstek başarılı olursa, birincil uç noktasından okumak uygulama devam eder.

```csharp
private static void Operation_context_RequestCompleted(object sender, RequestEventArgs e)
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

## <a name="next-steps"></a>Sonraki adımlar

Bölümünde serilerinden biri, bir uygulamanın nasıl gibi RA-GRS depolama hesapları ile yüksek oranda kullanılabilir yapma hakkında öğrenilen:

> [!div class="checklist"]
> * Depolama hesabı oluşturma
> * Örneği indirme
> * Bağlantı dizesi ayarlama
> * Konsol uygulamasını çalıştırın

Serinin arızanın benzetimini gerçekleştirin ve ikincil RA-GRS uç kullanmak için uygulamanızı zorlama hakkında bilgi almak için iki parçası için ilerleyin.

> [!div class="nextstepaction"]
> [Birincil storage uç noktanız bağlantısında arızanın benzetimini gerçekleştirin](storage-simulate-failure-ragrs-account-app.md)
