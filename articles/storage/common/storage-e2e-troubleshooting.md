---
title: Tanılama & ileti Çözümleyicisi ile Azure Storage sorunlarını giderme | Microsoft Docs
description: Azure Storage Analytics, AzCopy ve Microsoft Message Analyzer uçtan uca sorun giderme gösteren bir öğretici
services: storage
documentationcenter: dotnet
author: tamram
manager: timlt
ms.assetid: 643372a3-1c07-4e88-b4ef-042512a43086
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/15/2017
ms.author: tamram
ms.openlocfilehash: 324370ae18627a1985e6a40aec11ee2fa871e93b
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
ms.locfileid: "30323313"
---
# <a name="end-to-end-troubleshooting-using-azure-storage-metrics-and-logging-azcopy-and-message-analyzer"></a>Azure Storage ölçümleri ve günlüğe kaydetme, AzCopy ve ileti Çözümleyicisi'ni kullanarak uçtan uca sorun giderme
[!INCLUDE [storage-selector-portal-e2e-troubleshooting](../../../includes/storage-selector-portal-e2e-troubleshooting.md)]

Tanılama ve sorun giderme oluşturma ve Microsoft Azure Storage ile istemci uygulamalarını desteklemek için anahtar bir yetenektir. Azure uygulaması dağıtılmış yapısı nedeniyle, tanılama ve sorun giderme hataları ve performans sorunlarını geleneksel ortamlarda daha karmaşık olabilir.

Bu öğreticide, biz performansını etkiler ve uçtan uca bu hataları giderin belirli hataları belirlemek nasıl ekleyebileceğiniz gösterilmektedir istemci uygulaması en iyi duruma getirmek için Microsoft ve Azure Storage tarafından sağlanan araçları kullanarak.

Bu öğretici, uçtan uca bir sorun giderme senaryosu uygulamalı incelenmesi sağlar. Azure depolama uygulama sorunlarını giderme için bir ayrıntılı kavramsal kılavuzu için bkz: [izleme, tanılama ve Microsoft Azure Storage sorun giderme](storage-monitoring-diagnosing-troubleshooting.md).

## <a name="tools-for-troubleshooting-azure-storage-applications"></a>Azure Storage uygulamaları sorun giderme araçları
Microsoft Azure depolama kullanan istemci uygulamalar sorun giderme için ne zaman bir sorun oluştu ve sorunun nedeni ne olabilir belirlemek için Araçlar bileşimini kullanabilirsiniz. Bu araçlar şunları içerir:

* **Azure depolama çözümlemeleri**. [Azure Storage Analytics](/rest/api/storageservices/Storage-Analytics) ölçümleri ve Azure Storage için günlük kaydını sağlar.
  
  * **Depolama ölçümleri** işlem ölçümlerini ve depolama hesabınız için kapasite ölçümlerini izler. Ölçümleri kullanarak, uygulamanızın farklı ölçüleri çeşitli göre nasıl gerçekleştirmekte belirleyebilirsiniz. Bkz: [Storage Analytics Ölçüm tablosu şeması](/rest/api/storageservices/Storage-Analytics-Metrics-Table-Schema) depolama analizi tarafından izlenen ölçümleri türleri hakkında daha fazla bilgi için.
  * **Depolama günlük** her istek Azure Depolama Hizmetleri sunucu tarafı günlüğüne kaydeder. Günlük ayrıntılı veri yapılan işleme dahil olmak üzere her istek, işlem ve gecikme bilgileri durumunu izler. Bkz: [depolama Analytics günlük biçimi](/rest/api/storageservices/Storage-Analytics-Log-Format) günlüklerine depolama analizi tarafından yazılan istek ve yanıt verileri hakkında daha fazla bilgi için.

* **Azure portal**. Depolama hesabınız için ölçümleri ve günlük yapılandırabilirsiniz [Azure portal](https://portal.azure.com). Ayrıca, grafikler ve uygulamanızı zaman içinde nasıl gerçekleştirmekte gösteren grafikleri görüntüleyin ve uygulamanız için belirtilen bir ölçüm beklenenden farklı gerçekleştirirse sizi bilgilendirmek üzere uyarılar yapılandırın.
  
    Bkz: [Azure portalında bir depolama hesabını izleme](storage-monitor-storage-account.md) Azure portalında izlemeyi yapılandırma hakkında bilgi için.
* **AzCopy**. Microsoft Message Analyzer kullanarak analiz için yerel bir dizine günlük BLOB'ları kopyalamak için AzCopy kullanabilmeniz için Azure Storage için sunucu günlüklerine BLOB olarak depolanır. Bkz: [AzCopy komut satırı yardımcı programı ile veri aktarma](storage-use-azcopy.md) AzCopy hakkında daha fazla bilgi.
* **Microsoft Message Analyzer**. İleti Çözümleyicisi günlük dosyalarını kullanır ve filtre, arama ve Grup günlük veri, hata ve performans sorunlarını analiz etmek için kullanabileceğiniz yararlı ayarlar kolaylaştırır görsel bir biçimde günlük verileri görüntüleyen bir araçtır. Bkz: [Microsoft Message Analyzer işletim kılavuzu](http://technet.microsoft.com/library/jj649776.aspx) ileti Çözümleyicisi hakkında daha fazla bilgi.

## <a name="about-the-sample-scenario"></a>Örnek senaryo hakkında
Bu öğreticide, biz burada düşük yüzde başarı oranı Azure depolama çağıran bir uygulama için Azure Storage ölçümlerini gösterir bir senaryo inceleyeceğiz. Düşük yüzde başarı oranı ölçümü (olarak gösterilen **PercentSuccess** içinde [Azure portal](https://portal.azure.com) ve ölçümleri tablolardaki), başarılı, ancak 299 büyük bir HTTP durum kodu döndürür işlemlerini izler. Sunucu tarafı depolama günlük dosyalarında bir işlem durumuyla işlemlerini kaydedilir **ClientOtherErrors**. Düşük yüzde başarı ölçüm hakkında daha fazla ayrıntı için bkz: [ölçümleri Göster düşük PercentSuccess veya analytics günlük girdilerine sahip ClientOtherErrors işlem durumundaki işlemlerini](storage-monitoring-diagnosing-troubleshooting.md#metrics-show-low-percent-success).

Azure depolama işlemleri HTTP durum kodları 299 büyük normal işlevleri bir parçası olarak döndürebilir. Ancak bazı durumlarda bu hatalar istemci uygulamanızın performansı iyileştirebilir olabileceğini gösteriyor.

Bu senaryoda, biz % 100 altındaki herhangi bir şey olması düşük yüzde başarı oranı ele alacağız. Ancak, farklı bir ölçüm düzeyi gereksinimlerinize göre seçebilirsiniz. Uygulamanızın veya test sırasında bir taban çizgisi dayanıklılık için temel performans ölçümlerini oluşturmanızı öneririz. Örneğin, karar verebilir temel test üzerinde % 90 veya % 85 tutarlı yüzde başarı oranını uygulamanızın olması gerekir. Ölçüm verilerini gösterir, uygulamanın o numarasından deviating sonra artırma neden olabilecek araştırabilirsiniz.

Biz yüzde başarı oranı ölçüm %100 olduğunu kurduktan sonra bizim Örnek senaryo için biz ölçümlere ilişkilendirmek ve bunları ne düşük yüzde başarı oranı nedenini için kullanan hatalarını bulmak için günlüklerini inceleyin. Özellikle 400 aralığında hataları inceleyeceğiz. Ardından biz daha yakından (bulunamadı) 404 hatalarını araştırın.

### <a name="some-causes-of-400-range-errors"></a>400-range hataların bazı nedenler
Aşağıdaki örnekler örnekleme Azure Blob Storage ve bunların olası nedenleri istekler için bazı 400-range hata sayısı gösterilir. Bu hata, yanı sıra 300 aralığı ve 500 aralığı hataları hiçbirini düşük yüzde başarı oranı için katkıda bulunabilir.

Aşağıdaki listelerde gölgeden uzak tam olduğuna dikkat edin. Bkz: [durum ve hata kodları](http://msdn.microsoft.com/library/azure/dd179382.aspx) hataları her depolama hizmetleri için özel ve genel Azure Storage hataları hakkında ayrıntılı bilgi için MSDN'de.

**Durum kodu 404 (bulunamadı) örnekleri**

Blob veya kapsayıcı bulunmadığı için bir kapsayıcı veya blob karşı okuma işlemi başarısız olduğunda oluşur.

* Bir kapsayıcı veya blob bu isteği önce başka bir istemci tarafından silindiyse oluşur.
* Kapsayıcı veya blob var olup olmadığını denetledikten sonra oluşturan bir API çağrısı kullanılıyorsa oluşur. CreateIfNotExists API'ları kapsayıcısı veya blob varlığını denetlemek için önce bir HEAD çağrı olun; yoksa, 404 hatası döndürülür ve ardından kapsayıcı veya blob yazmak için ikinci PUT çağrı yapılır.

**Durum kodu 409 (Çakışma) örnekleri**

* Yeni kapsayıcı veya blob, varlık için önce denetlemeden oluşturmak için bir oluşturma API kullanırsanız ve bir kapsayıcı veya bu adı taşıyan blob zaten oluşur.
* Bir kapsayıcı silindi ve silme işlemi tamamlanmadan önce aynı ada sahip yeni bir kapsayıcı oluşturma girişimi varsa ortaya çıkar.
* Bir kapsayıcı veya blob kira belirtin ve zaten mevcut bir kira ise oluşur.

**Durum kodu 412 (önkoşul başarısız oldu) örnekleri**

* Koşullu üstbilgisi tarafından belirtilen koşulu karşılanmadı oluşur.
* Belirtilen kira kimliği kapsayıcı veya blob kira Kimliğiyle eşleşmiyor oluşur.

## <a name="generate-log-files-for-analysis"></a>Analiz için günlük dosyaları oluşturma
Bunlardan herhangi biri ile çalışmayı tercih ancak bu öğreticide, ileti Çözümleyicisi üç farklı türde günlük dosyaları ile çalışma için kullanırız:

* **Sunucu günlüğü**, Azure Storage günlüğü etkinleştirdiğinizde oluşturuldu. Sunucu günlüğü bir Azure Storage Hizmetleri - blob, kuyruk, tablo ve dosya karşı olarak adlandırılan her işlemi hakkındaki verileri içerir. Sunucu günlüğü, hangi işlemi çağrıldı ve hangi durum kodu döndürülen yanı sıra diğer ayrıntılarını istek ve yanıt gösterir.
* **.NET istemci günlüğü**, .NET uygulama içinde istemci tarafı günlüğe etkinleştirdiğinizde oluşturuldu. İstemci günlük nasıl istemci isteği hazırlar ve alır ve yanıt işlediği hakkında ayrıntılı bilgiler içerir.
* **HTTP ağ izleme günlüğü**, hangi verileri toplar Azure Storage'a karşı işlemleri dahil olmak üzere HTTP/HTTPS istek ve yanıt verileri. Bu öğreticide, biz ileti Çözümleyicisi aracılığıyla ağ izleme oluşturacaksınız.

### <a name="configure-server-side-logging-and-metrics"></a>Sunucu tarafı günlüğe kaydetme ve ölçümleri yapılandırın
Böylece çözümlemek için istemci uygulamasında verileri sahibiz ilk olarak, size Azure Storage günlüğe kaydetme ve ölçümleri, yapılandırmanız gerekir. Aracılığıyla günlüğe kaydetme ve çeşitli şekillerde - ölçümlerini yapılandırabilirsiniz [Azure portal](https://portal.azure.com), PowerShell kullanarak veya program aracılığıyla. Bkz: [depolama ölçümlerini etkinleştirme ve ölçüm verilerini görüntüleme](http://msdn.microsoft.com/library/azure/dn782843.aspx) ve [depolama günlüğünü etkinleştirme ve erişim günlüğü verilerini](http://msdn.microsoft.com/library/azure/dn782840.aspx) günlüğe kaydetme ve ölçümleri yapılandırma hakkında ayrıntılı bilgi için MSDN'de.

**Azure portalı üzerinden**

Yapılandırmak için günlüğe kaydetme ve depolama için ölçümleri kullanarak hesap [Azure portal](https://portal.azure.com), yönergeleri [Azure portalında bir depolama hesabını izleme](storage-monitor-storage-account.md).

> [!NOTE]
> Azure portalını kullanarak dakika ölçümleri ayarlamak mümkün değildir. Ancak, bunları bu öğreticinin amaçları ve uygulamanız ile performans sorunları incelemeye ayarlamanızı öneririz. Aşağıda gösterildiği gibi PowerShell veya program aracılığıyla depolama istemci kitaplığı kullanarak dakika ölçümleri ayarlayabilirsiniz.
> 
> Azure portalı dakika ölçümleri, saatlik ölçümleri yalnızca görüntüleyemiyor unutmayın.
> 
> 

**Via PowerShell**

Azure PowerShell ile çalışmaya başlamak için bkz: [Azure PowerShell'i yükleme ve yapılandırma nasıl](/powershell/azure/overview).

1. Kullanım [Add-AzureAccount](/powershell/module/azure/add-azureaccount?view=azuresmps-3.7.0) cmdlet PowerShell penceresine Azure kullanıcı hesabınızı eklemek için:
   
    ```powershell
    Add-AzureAccount
    ```

2. İçinde **Microsoft Azure'da oturum aç** penceresinde, e-posta adresi ve hesabınızla ilişkili parolayı yazın. Azure, kimlik bilgilerini doğrulayıp kaydeder ve pencereyi kapatır.
3. PowerShell penceresinde aşağıdaki komutları çalıştırarak öğretici için kullandığınız depolama hesabı için varsayılan depolama hesabı ayarlayın:
   
    ```powershell
    $SubscriptionName = 'Your subscription name'
    $StorageAccountName = 'yourstorageaccount'
    Set-AzureSubscription -CurrentStorageAccountName $StorageAccountName -SubscriptionName $SubscriptionName
    ```

4. Blob hizmeti için depolama günlük kaydını etkinleştir:
   
    ```powershell
    Set-AzureStorageServiceLoggingProperty -ServiceType Blob -LoggingOperations Read,Write,Delete -PassThru -RetentionDays 7 -Version 1.0
    ```

5. Ayarlanacak emin Blob hizmeti için depolama ölçümlerini etkinleştirme **- MetricsType** için `Minute`:
   
    ```powershell
    Set-AzureStorageServiceMetricsProperty -ServiceType Blob -MetricsType Minute -MetricsLevel ServiceAndApi -PassThru -RetentionDays 7 -Version 1.0
    ```

### <a name="configure-net-client-side-logging"></a>.NET istemci-tarafı günlüğünü yapılandırma
.NET uygulaması için istemci tarafı günlüğe kaydetmeyi yapılandırmak için uygulamanın yapılandırma dosyasında (web.config veya app.config) .NET tanılama etkinleştirin. Bkz: [istemci-tarafı .NET depolama istemci kitaplığı ile oturum](http://msdn.microsoft.com/library/azure/dn782839.aspx) ve [istemci-tarafı Java için Microsoft Azure depolama SDK'sı günlüğü](http://msdn.microsoft.com/library/azure/dn782844.aspx) ayrıntılı bilgi için MSDN'de.

İstemci-tarafı günlük nasıl istemci isteği hazırlar ve alır ve yanıt işlediği hakkında ayrıntılı bilgi içerir.

Depolama istemcisi kitaplığı istemci-tarafı günlük verileri uygulamanın yapılandırma dosyasında (web.config veya app.config) belirtilen konumda depolanır.

### <a name="collect-a-network-trace"></a>Bir ağ izlemesi Topla
İstemci uygulamanızı çalışırken bir HTTP/HTTPS ağ izleme toplamak için ileti Çözümleyicisi'ni kullanabilirsiniz. İleti Çözümleyicisi kullanır [Fiddler](http://www.telerik.com/fiddler) arka uçtaki. Ağ izleme toplamak önce şifrelenmemiş HTTPS trafiği kaydetmek için fiddler'ı yapılandırmanızı öneririz:

1. Yükleme [Fiddler](http://www.telerik.com/download/fiddler).
2. Fiddler'ı başlatın.
3. Seçin **Araçlar | Fiddler seçenekleri**.
4. Seçenekleri iletişim kutusunda emin **yakalama HTTPS bağlanır** ve **şifresini HTTPS trafiği** her ikisi de, aşağıda gösterildiği gibi seçilir.

![Fiddler seçeneklerini yapılandırma](./media/storage-e2e-troubleshooting/fiddler-options-1.png)

Öğretici için toplamak ve bir ağ izlemesi ilk ileti Çözümleyicisi'nde kaydedin, sonra izleme ve günlüklerini çözümlemek için bir analysis oturumu oluşturun. İleti Çözümleyicisi'nde bir ağ izlemesi toplamak için:

1. İleti Çözümleyicisi'nde seçin **dosya | Hızlı İzleme | Şifrelenmemiş HTTPS**.
2. İzleme hemen başlar. Seçin **durdurmak** biz yalnızca izleme depolama trafiği için yapılandırabilmeniz izlemeyi durdurmak için.
3. Seçin **Düzenle** izleme oturumu düzenlemek için.
4. Seçin **yapılandırma** sağındaki bağlantı **Microsoft Pef WebProxy** ETW sağlayıcı.
5. İçinde **Gelişmiş ayarları** iletişim kutusunda, tıklatın **sağlayıcı** sekmesi.
6. İçinde **Hostname filtre** alanında, boşlukla ayrılmış depolama noktalarınızı belirtin. Örneğin, aşağıdaki gibi noktalarınızı belirtebilirsiniz; değiştirme `storagesample` depolama hesabınızın adını için:

    ```   
    storagesample.blob.core.windows.net storagesample.queue.core.windows.net storagesample.table.core.windows.net
    ```

7. İletişim kutusundan çıkmak ve tıklayın **yeniden** böylece yalnızca Azure depolama ağ trafiğini izleme dahil, yerinde hostname filtreli izleme toplamaya başlamak için.

> [!NOTE]
> Ağ izleme toplama tamamladıktan sonra şifre çözme HTTPS trafiği için Fiddler'da değiştirmiş olabilecek ayarları geri kesinlikle öneririz. Fiddler Seçenekleri iletişim kutusunda seçimini **yakalama HTTPS bağlanır** ve **şifresini HTTPS trafiği** onay kutuları.
> 
> 

Bkz: [ağ izleme özelliklerini kullanmayı](http://technet.microsoft.com/library/jj674819.aspx) daha ayrıntılı bilgi için TechNet'teki.

## <a name="review-metrics-data-in-the-azure-portal"></a>Azure portalında ölçümleri verileri gözden geçirin
Uygulamanızı bir süre çalıştıran sonra görünür ölçümler grafiklerde inceleyebilirsiniz [Azure portal](https://portal.azure.com) hizmetiniz bir nasıl gerçekleştirmekte izlemek için.

İlk olarak, Azure portalında depolama hesabınıza gidin. Varsayılan olarak, bir izleme grafik ile **başarı oranı** ölçüm hesabı dikey penceresinde görüntülenir. Daha önce farklı ölçümleri görüntülemek için grafiği değiştirdiyseniz eklemek **başarı oranı** ölçüm.

Şimdi görürsünüz **başarı oranı** izleme grafikte, diğer bir ölçümleri birlikte eklediğiniz. Biz sonraki ileti Çözümleyicisi'ndeki günlükleri çözümleyerek araştırmak senaryosunda yüzde başarı biraz % 100 aşağıda hızıdır.

Ekleme ve ölçümleri grafikleri özelleştirme hakkında daha fazla bilgi için bkz: [ölçümler grafiklerde özelleştirme](storage-monitor-storage-account.md#customize-metrics-charts).

> [!NOTE]
> Depolama ölçümleri etkinleştirdikten sonra Azure Portalı'nda görünmesi ölçümleri verileriniz için biraz zaman alabilir. Geçerli saat sona ermeden önceki saat saatlik ölçümleri Azure portalında görüntülenmez olmasıdır. Ayrıca, dakika ölçümleri Azure portalında şu anda görüntülenmiyor. Bu nedenle bağlı olarak ölçümleri etkinleştirdiğinizde, ölçüm verilerini görmek için iki saat sürebilir.
> 
> 

## <a name="use-azcopy-to-copy-server-logs-to-a-local-directory"></a>Yerel bir dizine sunucu günlükleri kopyalamak için AzCopy kullanın
Ölçümleri tablolara yazılan sırasında azure depolama BLOB'ları için sunucu günlüğü verileri yazar. Günlük BLOB'lar iyi bilinen kullanılabilir `$logs` depolama hesabınız için kapsayıcı. Böylece, araştırmak istediğiniz zaman aralığını kolayca bulabilir günlük BLOB'lar yıl, ay, gün ve saat tarafından hiyerarşik olarak adlandırılır. Örneğin, `storagesample` hesabı, 01/02/2015, 8-9'da, gelen günlük BLOB'lar için kapsayıcı `https://storagesample.blob.core.windows.net/$logs/blob/2015/01/08/0800`. Bu kapsayıcı tek tek bloblar, sıralı olarak, itibaren adlandırıldığından `000000.log`.

Bu sunucu tarafı günlük dosyaları bir konumla yerel makinenize indirmek için AzCopy komut satırı aracını kullanabilirsiniz. Örneğin, klasöre 2 Ocak 2015 tarihinde gerçekleşen blobu işlemleri için günlük dosyalarını indirmek için aşağıdaki komutu kullanabilirsiniz `C:\Temp\Logs\Server`; Değiştir `<storageaccountname>` depolama hesabınızın adıyla ve `<storageaccountkey>` hesap erişim anahtarı ile:

```azcopy
AzCopy.exe /Source:http://<storageaccountname>.blob.core.windows.net/$logs /Dest:C:\Temp\Logs\Server /Pattern:"blob/2015/01/02" /SourceKey:<storageaccountkey> /S /V
```
İndirmek için AzCopy kullanılabilir [Azure indirmeleri](https://azure.microsoft.com/downloads/) sayfası. AzCopy kullanma hakkında daha fazla bilgi için bkz: [AzCopy komut satırı yardımcı programı ile veri aktarma](storage-use-azcopy.md).

Sunucu tarafı günlüklerini indirme hakkında ek bilgi için bkz: [karşıdan depolama günlüğü günlük verileri](http://msdn.microsoft.com/library/azure/dn782840.aspx#DownloadingStorageLogginglogdata).

## <a name="use-microsoft-message-analyzer-to-analyze-log-data"></a>Günlük verileri çözümlemek için Microsoft Message Analyzer'ı kullanın
Microsoft Message Analyzer, yakalama, görüntüleme ve trafik, olaylar ve sorun giderme ve tanılama senaryolarda diğer sistem veya uygulama iletileri Mesajlaşma Protokolü çözümlemek için kullanılan bir araçtır. İleti Çözümleyicisi'ni de yüklemek için toplama ve günlük verileri çözümlemek sağlar ve izleme dosyaları kaydedilir. İleti Çözümleyicisi hakkında daha fazla bilgi için bkz: [Microsoft Message Analyzer işletim kılavuzu](http://technet.microsoft.com/library/jj649776.aspx).

İleti Çözümleyicisi varlıklar Azure depolama için sunucu, istemci ve ağ günlüklerini analiz etmenize yardım içerir. Bu bölümde, biz depolama günlüklerine düşük yüzde başarı sorunu gidermek için bu araçları kullanmak nasıl ele alacağız.

### <a name="download-and-install-message-analyzer-and-the-azure-storage-assets"></a>İleti Çözümleyicisi ve Azure depolama varlıkları yükleyip
1. Karşıdan [ileti Çözümleyicisi](http://www.microsoft.com/download/details.aspx?id=44226) Microsoft Yükleme Merkezi'ndeki ve yükleyiciyi çalıştırın.
2. İleti Çözümleyicisi'ni başlatın.
3. Gelen **Araçları** menüsünde, select **varlık Yöneticisi**. İçinde **varlık Yöneticisi** iletişim kutusunda **indirmeleri**, sonra filtre **Azure Storage**. Aşağıdaki resimde gösterildiği gibi Azure depolama varlıkları görürsünüz.
4. Tıklatın **eşitleme tüm görüntülenen öğelerin** Azure depolama varlıklar yüklemek için. Kullanılabilir Varlıklar şunları içerir:
   * **Azure depolama renk kurallarını:** Azure depolama renk kuralları belirli bir izleme bilgileri içeren iletileri vurgulamak için renk, metin ve yazı tipi stillerini kullanan özel filtreler tanımlamak etkinleştirin.
   * **Azure depolama grafikler:** Azure depolama grafikleri olan sunucu günlüğü verileri grafik önceden tanımlanmış grafikler. Şu anda Azure Storage grafikler kullanmak için yalnızca sunucu günlüğü analiz kılavuza yükleyebilir olduğunu unutmayın.
   * **Azure depolama ayrıştırıcıları:** Azure Storage ayrıştırıcıları analiz kılavuzunda görüntülemek için Azure Storage istemci, sunucu ve HTTP günlükleri ayrıştırılamıyor.
   * **Azure depolama filtreler:** Azure depolama filtreleri verilerinizi analiz kılavuzunda sorgulamak için kullanabileceğiniz önceden tanımlanmış ölçütleri şunlardır.
   * **Azure depolama görünüm düzenler:** Azure depolama görünüm düzenler: önceden tanımlanmış sütun düzenleri ve analiz kılavuzunda gruplandırmaları.
5. Varlıkları yükledikten sonra ileti Çözümleyicisi'ni yeniden başlatın.

![İleti Çözümleyicisi varlık Yöneticisi](./media/storage-e2e-troubleshooting/mma-start-page-1.png)

> [!NOTE]
> Tüm bu öğreticinin amaçları doğrultusunda gösterilen ve Azure Storage varlıkları yükleyin.
> 
> 

### <a name="import-your-log-files-into-message-analyzer"></a>Günlük dosyalarınızın ileti çözümleyicisine alma
Tüm kaydedilmiş günlük dosyalarınızı (sunucu tarafı, istemci tarafı ve ağ), Microsoft Message Analyzer tek bir oturumda çözümleme için içine aktarabilirsiniz.

1. Üzerinde **dosya** Microsoft Message Analyzer'nde menüsünü **yeni oturum**ve ardından **boş oturum**. İçinde **yeni oturum** iletişim kutusunda, analiz oturumunuz için bir ad girin. İçinde **oturumun ayrıntılarına** paneli, tıklayın **dosyaları** düğmesi.
2. İleti Çözümleyicisi tarafından oluşturulan ağ izleme verilerini yüklemek için tıklayın **dosyaları Ekle**göz atın, web izleme oturumunuzda .matp dosyanızı kaydedildiği konum için .matp dosyasını seçin ve tıklatın **açık**.
3. Sunucu tarafı günlük verilerini yüklemek için tıklayın **dosyaları Ekle**, sunucu tarafı günlüklerinizi indirdiğiniz konuma gözatın, çözümlemek ve istediğiniz zaman aralığını için günlük dosyalarını seçin **açık**. Ardından **oturumun ayrıntılarına** paneli, Ayarla **metin günlüğü Yapılandırması** her sunucu tarafı günlük dosyası için açılan **AzureStorageLog** Microsoft Message Analyzer günlük dosyasını doğru ayrıştırma emin olmak için.
4. İstemci-tarafı günlük verilerini yüklemek için tıklayın **dosyaları Ekle**, istemci-tarafı günlüklerinizi kaydettiğiniz konuma göz atın, çözümlemek ve günlük dosyaları seçin **açık**. Ardından **oturumun ayrıntılarına** paneli, Ayarla **metin günlüğü Yapılandırması** her istemci-tarafı günlük dosyası için açılan **AzureStorageClientDotNetV4** Microsoft Message Analyzer günlük dosyasını doğru ayrıştırma emin olmak için.
5. Tıklatın **Başlat** içinde **yeni oturum** yüklemek ve günlük verilerini ayrıştırmak için iletişim kutusu. İleti Çözümleyicisi çözümleme kılavuzunda günlük verilerini görüntüler.

Aşağıdaki resimde, sunucu, istemci ve ağ izleme günlük dosyaları ile yapılandırılmış bir örnek oturumu gösterilmektedir.

![İleti Çözümleyicisi oturum yapılandırma](./media/storage-e2e-troubleshooting/configure-mma-session-1.png)

İleti Çözümleyicisi belleğe günlük dosyalarını yükler unutmayın. Çok sayıda günlük verileri varsa, ileti Çözümleyicisi'nden en iyi performansı alabilmek için filtre uygulamak istediğiniz.

İlk olarak, gözden geçirme ilgilendiğiniz zaman aralığını belirleyin ve bu zaman dilimi olabildiğince küçük tutun. Çoğu durumda dakika veya saat en çok bir süre gözden geçirmek isteyeceksiniz. En küçük kümesini gereksinimlerinizi karşılayan günlükleri alın.

Büyük miktarda günlük veri hala varsa, oturum filtrelemek için önce günlük verilerinizi yüklenmeye belirtmek isteyebilirsiniz. İçinde **oturum filtre** kutusunda **Kitaplığı** önceden tanımlanmış bir filtre seçmek için düğmesini; Örneğin, tercih **genel zaman filtresi ı** Azure depolama biriminden filtreler bir zaman aralığında filtre uygulamak için. Başlangıç ve zaman damgası görmek istediğiniz aralığı için bitiş belirtmek için filtre ölçütlerini daha sonra düzenleyebilirsiniz. Bir özel durum kodu de filtre uygulayabilirsiniz; Örneğin, yalnızca durum kodu 404 olduğu günlük girişlerini yük seçebilirsiniz.

Microsoft Message Analyzer günlük verilerini alma hakkında daha fazla bilgi için bkz: [ileti verilerini alma](http://technet.microsoft.com/library/dn772437.aspx) TechNet'te.

### <a name="use-the-client-request-id-to-correlate-log-file-data"></a>Günlük dosyası verilerini ilişkilendirmek için istemci İstek Kimliği'ni kullanın
Azure Storage istemci kitaplığı, benzersiz istemci istek kimliği her istek için otomatik olarak oluşturur. İleti Çözümleyicisi içindeki tüm üç günlüklerini arasında verilerin bağıntısını kurmaya kullanabilmek için bu değer istemci günlüğü, sunucu günlüğü ve ağ izleme için yazılmıştır. Bkz: [istemci istek kimliği](storage-monitoring-diagnosing-troubleshooting.md#client-request-id) istemci hakkında ek bilgi için kimliği isteyin.

Aşağıdaki bölümler ilişkilendirmek için önceden yapılandırılmış ve özel yerleşim görünümleri kullanmayı açıklar ve Grup verileri istemci istek kimliği temel alınarak

### <a name="select-a-view-layout-to-display-in-the-analysis-grid"></a>Analiz ızgarada gösterilecek bir görünüm düzeni seçin
İleti Çözümleyicisi için depolama varlıkların yararlı gruplandırmaları ve sütunları farklı senaryolar için verilerinizle görüntülemek için kullanabileceğiniz önceden yapılandırılmış görünümler Azure depolama görünüm düzenleri içerir. Ayrıca, özel görünüm düzenleri oluşturabilir ve bunları yeniden kullanmak üzere kaydedin.

Resmin gösterir altında **görünüm düzeni** menüsünde seçerek kullanılabilir **görünüm düzeni** araç şeridinden. Azure Storage için Görünüm düzenleri altında gruplanan **Azure Storage** menüde düğümü. Arayabilirsiniz `Azure Storage` düzenleri yalnızca Azure depolama alanında filtrelemek için arama kutusunu görüntüleyin. Sık kullanılan yapmak ve menüsünün üstünde görüntülemek için bir görünüm düzeni yanındaki yıldız öğesini de seçebilirsiniz.

![Düzen menüsü görüntüleme](./media/storage-e2e-troubleshooting/view-layout-menu.png)

Başından itibaren seçin **ClientRequestID ve modül göre gruplandırılmış**. Bu görünüm düzeni üç günlüğü verileri ilk istemci istek kimliği, sonra bir kaynak günlük dosyasını günlük (veya **Modülü** ileti Çözümleyicisi). Bu görünüm ile belirli bir istemci istek kimliği detaya ve tüm üç günlük dosyaları için istemci istek kimliği verilerden bakın

Resim gösterir Aşağıda örnek günlük verilerle görüntülenen bir sütun alt kümesini bu görünümü uygulanır. Belirli bir istemci istek kimliği için istemci günlüğü, sunucu günlüğü ve ağ izleme verilerini analiz kılavuz görüntüler görebilirsiniz.

![Azure depolama görünüm düzeni](./media/storage-e2e-troubleshooting/view-layout-client-request-id-module.png)

> [!NOTE]
> Farklı günlük dosyaları farklı sütuna sahip birden çok günlük dosyası verilerini analiz kılavuzunda görüntülendiğinde, belirli bir satır için herhangi bir veri bazı sütunları içermeyebilir şekilde. Örneğin, yukarıdaki resimde, istemci günlük satırları için tüm verileri gösterme **zaman damgası**, **TimeElapsed**, **kaynak**, ve **hedef** sütunları, çünkü bu sütun istemci günlüğünde yok, ancak ağ izleme yok. Benzer şekilde, **zaman damgası** sütun sunucu günlüğünden zaman damgası veri görüntüler, ancak hiçbir veri görüntülenen **TimeElapsed**, **kaynak**, ve **hedef** sunucu günlüğü parçası olmayan sütunları.
> 
> 

Azure Storage görünüm düzenleri kullanmanın yanı sıra, aynı zamanda tanımlamak ve kendi görünüm düzenleri kaydedin. Verileri gruplandırma diğer istediğiniz alanları seçin ve gruplandırma özel düzeniniz de bir parçası olarak kaydedin.

### <a name="apply-color-rules-to-the-analysis-grid"></a>Analiz kılavuza renk kuralları uygula
Depolama varlıklar hataları çözümleme kılavuzunda farklı türlerini tanımlamak üzere bir görsel anlamına gelir teklif renk kurallarını da içerir. Yalnızca sunucu günlüğü ve ağ izleme için görünmesi için önceden tanımlanmış renk kuralları HTTP hataları için geçerlidir.

Renk kurallarını uygulamak için seçin **renk kurallarını** araç şeridinden. Azure Storage renk kurallarını menüde görürsünüz. Öğretici için seçin **istemci hataları (durum kodu 400-499 arasında)**, aşağıdaki resimde gösterildiği gibi.

![Azure depolama görünüm düzeni](./media/storage-e2e-troubleshooting/color-rules-menu.png)

Azure Storage renk kurallarını kullanarak ek olarak, aynı zamanda tanımlamak ve kendi renk kurallarını kaydedin.

### <a name="group-and-filter-log-data-to-find-400-range-errors"></a>400-range hatalarını bulmak için Grup ve filtre günlük verileri
Ardından, grubu ve tüm hataları 400 aralıkta bulmak için günlük verileri filtreleyin.

1. Bulun **StatusCode** analiz kılavuz sütunu sağ sütun başlığını ve select **grup**.
2. Ardından, gruplandırma **ClientRequestId** sütun. Verileri analiz kılavuzunda şimdi durum kodu ve istemci istek kimliği tarafından düzenlenir görürsünüz
3. Zaten görüntülenmiyorsa, Görünüm filtresi araç penceresi görüntüler. Araç şeridinde seçin **aracı Windows**, ardından **Görünüm Filtresi**.
4. Yalnızca 400-range hataları görüntülemek için günlük verilerini filtrelemek için aşağıdaki filtre ölçütü eklemek **Görünüm Filtresi** penceresi ve tıklatın **Uygula**:

    ```   
    (AzureStorageLog.StatusCode >= 400 && AzureStorageLog.StatusCode <=499) || (HTTP.StatusCode >= 400 && HTTP.StatusCode <= 499)
    ```

Aşağıdaki resimde bu gruplandırma ve filtre sonuçlarını gösterir. Genişletme **ClientRequestID** durum kodu 409, gruplandırma altına alan Örneğin, bu durum kodunda sonuçlanan bir işlem gösterir.

![Azure depolama görünüm düzeni](./media/storage-e2e-troubleshooting/400-range-errors1.png)

Bu filtre uygulandıktan sonra göreceğiniz istemci günlüğü satırları dışlanır, istemci olarak günlük içermemesi bir **StatusCode** sütun. Başından itibaren biz sunucu ve 404 hatalarını bulmak için ağ izleme günlüklerini gözden geçirin ve bunları neden istemci işlemleri incelemek için istemci günlüğüne sonra getireceğiz.

> [!NOTE]
> Üzerindeki filtre **StatusCode** sütun ve hala durum kodunu olduğu null günlük girişlerini içeren filtre için bir ifade eklerseniz, istemci günlük dahil olmak üzere, tüm üç günlükleri görüntüleme verileri. Bu filtre ifadesi oluşturmak için kullanın:
> 
> <code>&#42;StatusCode >= 400 or !&#42;StatusCode</code>
> 
> Bu filtre tüm satırları istemciden günlük ve yalnızca sunucu günlüğü ve HTTP günlüğü satırlarından durum kodu 400'den büyük olduğu döndürür. İstemci istek kimliği ve modül göre gruplandırılmış görünümü Düzen uygularsanız, arama kaydırın veya istediğiniz tüm üç günlüklerini burada gösterilir olanları bulmak için günlük girişlerini aracılığıyla.   
> 
> 

### <a name="filter-log-data-to-find-404-errors"></a>404 hatalarını bulmak için filtre günlük verileri
Depolama varlıklar hataları veya aradığınız eğilimleri bulmak için günlük verileri daraltmak için kullanabileceğiniz önceden tanımlanmış filtreler aşağıdakileri içerir. Ardından, şu iki önceden tanımlanmış filtre uygulamak: sunucu ve ağ izleme günlükleri 404 hataları filtreleyen ve verileri belirtilen zaman aralığı filtreleyen bir.

1. Zaten görüntülenmiyorsa, Görünüm filtresi araç penceresi görüntüler. Araç şeridinde seçin **aracı Windows**, ardından **Görünüm Filtresi**.
2. Görünüm Filtresi penceresinde seçin **Kitaplığı**ve arama `Azure Storage` Azure Storage bulmak için filtreler. İçin filtreyi seçin **404 (bulunamadı) tüm günlüklerde iletileri**.
3. Görüntü **Kitaplığı** menü yeniden bulun ve seçin **genel zaman filtresi**.
4. Filtre, görüntülemek istediğiniz aralığı için gösterilen zaman damgaları düzenleyin. Bu, analiz etmek için veri aralığını daraltmak için yardımcı olur.
5. Filtre aşağıdaki örneğe benzer görünmelidir. Tıklatın **Uygula** analiz kılavuza filtre uygulamak için.

    ```   
    ((AzureStorageLog.StatusCode == 404 || HTTP.StatusCode == 404)) And
    (#Timestamp >= 2014-10-20T16:36:38 and #Timestamp <= 2014-10-20T16:36:39)
    ```

    ![Azure depolama görünüm düzeni](./media/storage-e2e-troubleshooting/404-filtered-errors1.png)

### <a name="analyze-your-log-data"></a>Günlük verileri analiz etme
Gruplandırılmış ve verilerinizi filtre göre üretilen 404 hatalarını tek tek isteklerin ayrıntıları inceleyebilirsiniz. Geçerli Görünüm düzende veri günlüğü kaynağı tarafından sonra istemci istek kimliği göre gruplandırılır. Biz isteklerinde, 404 StatusCode alanın içerdiği filtre olduğundan, yalnızca sunucu ve ağ izleme verilerini, istemci günlük verilerini göreceğiz.

Aşağıdaki resimde belirli bir istek blob mevcut olmadığından bir Blob alma işlemi bir 404 burada verdiğini gösterir. Bazı sütunları Standart görünümden ilgili verileri görüntülemek için kaldırılmış unutmayın.

![Filtrelenen sunucu ve ağ izleme günlükleri](./media/storage-e2e-troubleshooting/server-filtered-404-error.png)

Ardından, biz bu istemci istek kimliği hata oluştuğunda istemci sürüyordu hangi eylemleri görmek için istemci günlük verileri ile ilişkilendirilmesi. İkinci bir sekmede açtığında istemci günlüğü verilerini görüntülemek bu oturum için yeni bir analiz Izgara Görünümü görüntüleyebilirsiniz:

1. İlk olarak, değerini kopyalayın **ClientRequestId** panoya alan. Bunu yapmak için her iki satır seçilmesi, bulma **ClientRequestId** alan, veri değeri sağ tıklayarak ve seçme **Kopyala 'ClientRequestId'**.
2. Araç şeridinde seçin **yeni Viewer**seçeneğini belirleyip **analiz kılavuz** yeni bir sekme açın. Yeni sekmesi tüm verileri gruplandırma, filtre veya renk kurallarını olmadan, günlük dosyalarında gösterir.
3. Araç şeridinde seçin **görünüm düzeni**seçeneğini belirleyip **tüm .NET istemci sütunları** altında **Azure Storage** bölümü. Bu görünüm düzeni, sunucu ve ağ izleme günlükleri yanı sıra günlük istemci verileri gösterir. Varsayılan olarak üzerinde sıralanır **MessageNumber** sütun.
4. Ardından, istemci günlük istemci istek kimliği için arama Araç şeridinde seçin **iletileri Bul'u**, istemci istek kimliği üzerinde bir özel filtre belirtmek **Bul** alan. Kendi istemci istek kimliği belirleme filtresi için şu sözdizimini kullanın:

    ```
    *ClientRequestId == "398bac41-7725-484b-8a69-2a9e48fc669a"
    ```

İleti Çözümleyicisi'ni bulur ve burada arama ölçütleriyle eşleşen istemci isteği kimliği ilk günlük girişi seçer İstemci günlüğü vardır her istemci istek kimliği için birden çok girişi, üzerinde gruplandırmak istediğiniz şekilde **ClientRequestId** hepsini bir araya görmeyi kolaylaştırmak için alan. Aşağıdaki resimde tüm iletileri istemci günlük için belirtilen istemci istek kimliği gösterir

![İstemci günlük gösteren 404 hataları](./media/storage-e2e-troubleshooting/client-log-analysis-grid1.png)

Bu iki sekme görünümü düzenleri gösterilen verileri kullanarak ne hataya neden belirlemek için istek verileri analiz edebilirsiniz. Ayrıca, önceki bir olayı 404 hatası neden, görmek için bunu öncesinde istekleri da bakabilirsiniz. Örneğin, bu istemci İstek Kimliği'blob silinmiş olup olmadığını veya hata nedeniyle bir kapsayıcı veya blob CreateIfNotExists API'yi çağıran istemci uygulaması ise belirlemek için önceki istemci günlük girişlerini gözden geçirebilirsiniz. İstemci günlüğünde blob'un adresinde bulabilirsiniz **açıklama** alan; sunucu ve ağ izleme günlükleri, bu bilgileri görünür **Özet** alan.

404 hatası verdiğini blob adresi öğrendikten sonra daha fazla araştırabilirsiniz. Günlük girişlerini aynı blob işlemleri ile ilişkili diğer iletiler için arama yaparsanız, istemci varlık daha önce silinmiş olup olmadığını kontrol edebilirsiniz.

## <a name="analyze-other-types-of-storage-errors"></a>Başka tür depolama hataları çözümleme
İleti Çözümleyicisi günlük verilerinizi çözümlemek için kullandıysanız, başka tür görünümünü kullanarak hataları çözümleyebilirsiniz düzenleri, renk kurallarını ve arama ve filtreleme. Aşağıdaki tablolarda, karşılaşabileceğiniz bazı sorunlar ve bunları bulmak için kullanabileceğiniz filtreleme ölçütlerini listeler. Filtreler ve dil filtreleme ileti Çözümleyicisi oluşturma ile ilgili daha fazla bilgi için bkz: [ileti verileri filtreleme](http://technet.microsoft.com/library/jj819365.aspx).

| Araştırmak için... | Filtre ifadesi kullan... | İfade günlüğüne uygular (istemci, sunucu, ağ, tüm) |
| --- | --- | --- |
| Bir kuyruk iletisi Teslimde beklenmeyen gecikme |AzureStorageClientDotNetV4.Description   contains "Retrying failed operation." |İstemci |
| HTTP PercentThrottlingError artış |HTTP. Response.StatusCode 500 == &#124; &#124; HTTP. Response.StatusCode 503 == |Ağ |
| İçinde PercentTimeoutError artırın |HTTP. Response.StatusCode 500 == |Ağ |
| (Tümü) PercentTimeoutError içinde artırın |* StatusCode 500 == |Tümü |
| İçinde PercentNetworkError artırın |AzureStorageClientDotNetV4.EventLogEntry.Level   < 2 |İstemci |
| HTTP 403 (Yasak iletileri) |HTTP. Response.StatusCode 403 == |Ağ |
| HTTP 404 (bulunamadı) iletileri |HTTP. Response.StatusCode 404 == |Ağ |
| 404 (Tümü) |*StatusCode   == 404 |Tümü |
| Paylaşılan erişim imzası (SAS) yetkilendirme sorunu |AzureStorageLog.RequestStatus "SASAuthorizationError" == |Ağ |
| HTTP 409 (Çakışma) iletileri |HTTP. Response.StatusCode 409 == |Ağ |
| 409 (Tümü) |*StatusCode   == 409 |Tümü |
| Düşük PercentSuccess veya analytics günlük girişlerini ClientOtherErrors işlem durumundaki işlemlerini sahip |AzureStorageLog.RequestStatus "ClientOtherError" == |Sunucu |
| Nagle uyarı |((AzureStorageLog.EndToEndLatencyMS-AzureStorageLog.ServerLatencyMS) > (AzureStorageLog.ServerLatencyMS * 1.5)) ve (AzureStorageLog.RequestPacketSize < 1460) ve (AzureStorageLog.EndToEndLatencyMS - AzureStorageLog.ServerLatencyMS > = 200) |Sunucu |
| Sunucu ve ağ günlüklerine zaman aralığı |#Timestamp > = 2014-10-20T16:36:38 ve #Timestamp < = 2014-10-20T16:36:39 |Sunucu, ağ |
| Sunucu günlüklerindeki zaman aralığı |AzureStorageLog.Timestamp > = 2014-10-20T16:36:38 ve AzureStorageLog.Timestamp < = 2014-10-20T16:36:39 |Sunucu |

## <a name="next-steps"></a>Sonraki adımlar
Azure storage'da sorun giderme uçtan uca senaryoları hakkında daha fazla bilgi için şu kaynaklara bakın:

* [Microsoft Azure Depolama izleme, tanılama ve sorun giderme](storage-monitoring-diagnosing-troubleshooting.md)
* [Depolama Analizi](http://msdn.microsoft.com/library/azure/hh343270.aspx)
* [Azure portalında bir depolama hesabını izleme](storage-monitor-storage-account.md)
* [AzCopy Komut Satırı Yardımcı Programı ile veri aktarımı](storage-use-azcopy.md)
* [Microsoft Message Analyzer işletim kılavuzu](http://technet.microsoft.com/library/jj649776.aspx)
