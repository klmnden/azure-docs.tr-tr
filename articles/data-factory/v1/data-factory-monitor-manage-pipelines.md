---
title: İzleme ve ardışık düzen Azure portal ve PowerShell kullanarak yönetme | Microsoft Docs
description: Azure data factory'leri ve oluşturduğunuz ardışık düzen yönetmek ve izlemek için Azure portalı ve Azure PowerShell kullanmayı öğrenin.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.assetid: 9b0fdc59-5bbe-44d1-9ebc-8be14d44def9
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/30/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: b6cfe6ba510f1e7ed1b448d99fb8a71bb94053e8
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34620692"
---
# <a name="monitor-and-manage-azure-data-factory-pipelines-by-using-the-azure-portal-and-powershell"></a>İzleme ve Azure portalı ve PowerShell kullanarak Azure Data Factory işlem hatlarını yönetme
> [!div class="op_single_selector"]
> * [Azure portal/Azure PowerShell kullanma](data-factory-monitor-manage-pipelines.md)
> * [Kullanılarak izleme ve yönetim uygulaması](data-factory-monitor-manage-app.md)

> [!NOTE]
> Bu makale, Data Factory’nin genel kullanıma açık olan (GA) 1. sürümü için geçerlidir. Önizlemede değil, Data Factory hizmetinin 2 sürümünü kullanıyorsanız bkz [izlemek ve 2 sürümündeki Data Factory işlem hatlarını yönetmek](../monitor-visually.md).

Bu makalede, izleme, yönetme ve işlem hatlarınızı Azure portalı ve PowerShell kullanarak hata ayıklama açıklar.

> [!IMPORTANT]
> İzleme ve yönetim uygulama izleme ve veri işlem hatlarınızı yönetmek ve sorunları gidermek için daha iyi destek sağlar. Uygulamayı kullanma hakkında daha fazla bilgi için bkz: [izlemek ve Data Factory işlem hatlarını izleme ve yönetim uygulaması kullanarak yönetmek](data-factory-monitor-manage-app.md). 

> [!IMPORTANT]
> Azure Data Factory sürüm 1 şimdi kullanan yeni [Azure altyapı uyarı İzleyicisi](../../monitoring-and-diagnostics/monitor-alerts-unified-usage.md). Eski uyarı altyapı kullanım dışıdır. Sonuç olarak, mevcut uyarılarınızı 1 veri fabrikaları artık çalışma sürümü için yapılandırılmış. Mevcut uyarılarınızı v1 veri fabrikaları için otomatik olarak geçirilmez. Bu uyarılar yeni uyarı altyapı oluşturmanız gerekebilir. Azure portal ve select oturum açma **İzleyici** yeni uyarılar ölçümleri (örneğin, başarısız çalışır veya başarılı çalıştırır) sürümünüz için 1 veri fabrikaları oluşturmak için.

## <a name="understand-pipelines-and-activity-states"></a>Ardışık Düzen ve etkinlik durumlarını anlama
Azure Portalı'nı kullanarak şunları yapabilirsiniz:

* Veri fabrikanızın diyagramı olarak görüntüleyin.
* Bir ardışık düzendeki etkinlik görünümü.
* Giriş ve çıkış veri kümeleri görüntüleyin.

Bu bölümde, nasıl bir veri kümesi dilim başka bir duruma bir durumdan diğerine geçer de açıklanmaktadır.   

### <a name="navigate-to-your-data-factory"></a>Veri fabrikanızın gidin
1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Tıklatın **veri fabrikaları** soldaki menüde. Göremiyorsanız, tıklatın **daha fazla hizmet >** ve ardından **veri fabrikaları** altında **INTELLİGENCE + ANALİZ** kategorisi.

   ![Tümüne Gözat > veri fabrikaları](./media/data-factory-monitor-manage-pipelines/browseall-data-factories.png)
3. Üzerinde **veri fabrikaları** dikey penceresinde, ilgilendiğiniz data factory seçin.

    ![Veri fabrikası seçme](./media/data-factory-monitor-manage-pipelines/select-data-factory.png)

   Data factory giriş sayfasını görmeniz gerekir.

   ![Veri fabrikası dikey penceresi](./media/data-factory-monitor-manage-pipelines/data-factory-blade.png)

#### <a name="diagram-view-of-your-data-factory"></a>Veri fabrikanızın diyagram görünümü
**Diyagramı** data factory görünümünü bölmeden data factory ve varlıklarını yönetmek ve izlemek için cam sağlar. Görmek için **diyagramı** 'ı tıklatın, görüntüleyin, veri fabrikası **diyagramı** data factory giriş sayfasında.

![Diyagram görünümü](./media/data-factory-monitor-manage-pipelines/diagram-view.png)

Yakınlaştırma, yakınlaştırabilir, sığacak, % 100 Yaklaştır, diyagramın düzenini kilitlemek ve ardışık düzen ve veri kümeleri otomatik Konumlandır yakınlaştırma. Veri çizgileri bilgileri de görebilirsiniz (diğer bir deyişle, seçilen öğelerin Yukarı Akış ve aşağı akış öğelerini göster).

### <a name="activities-inside-a-pipeline"></a>Bir işlem hattı içindeki etkinlikler
1. Ardışık Düzen sağ tıklayın ve ardından **açık işlem hattı** ardışık etkinlikler için girdi ve çıktı veri kümeleri ile birlikte tüm etkinliklerin görmek için. Bu özellik, hattınızı birden fazla etkinlik içerir ve tek bir ardışık işletimsel çizgileri anlamak istediğinizde yararlı olur.

    ![İşlem hattı menüsünü açma](./media/data-factory-monitor-manage-pipelines/open-pipeline-menu.png)     
2. Aşağıdaki örnekte, bir kopyalama etkinliği bir girdi ve çıktı düzenindeki bakın. 

    ![Bir işlem hattı içindeki etkinlikler](./media/data-factory-monitor-manage-pipelines/activities-inside-pipeline.png)
3. Giriş sayfasına geri data Factory tıklayarak gidebilirsiniz **veri fabrikası** sol üst köşesinde içerik haritası bağlantıyı.

    ![Veri Fabrikası için geri gidin](./media/data-factory-monitor-manage-pipelines/navigate-back-to-data-factory.png)

### <a name="view-the-state-of-each-activity-inside-a-pipeline"></a>Bir işlem hattı içindeki her etkinlik durumunu görüntüleme
Herhangi bir etkinlik tarafından üretilen veri kümeleri durumunu görüntüleyerek bir etkinlik geçerli durumunu görüntüleyebilirsiniz.

Çift tıklatarak **OutputBlobTable** içinde **diyagramı**, farklı etkinlik çalışması içinde bir ardışık düzen tarafından üretilen tüm dilimleri görebilirsiniz. Kopya etkinliği için başarıyla son sekiz saat çalıştırılmış ve dilimlerinin görebilirsiniz **hazır** durumu.  

![Ardışık Düzen durumu](./media/data-factory-monitor-manage-pipelines/state-of-pipeline.png)

Data factory veri kümesi dilimleri aşağıdaki durumlardan biri olabilir:

<table>
<tr>
    <th align="left">Durum</th><th align="left">Alt durum</th><th align="left">Açıklama</th>
</tr>
<tr>
    <td rowspan="8">Bekleniyor</td><td>ScheduleTime</td><td>Saat dilimi çalıştırmak gelen kurmadı.</td>
</tr>
<tr>
<td>DatasetDependencies</td><td>Yukarı Akış bağımlılıkları hazır değil.</td>
</tr>
<tr>
<td>ComputeResources</td><td>İşlem kaynakları kullanılabilir değil.</td>
</tr>
<tr>
<td>ConcurrencyLimit</td> <td>Diğer tüm etkinlik örnekleri diğer dilimleri çalıştırıyor.</td>
</tr>
<tr>
<td>ActivityResume</td><td>Etkinlik duraklatıldı ve etkinlik sürdürülene kadar dilimler çalıştırılamaz.</td>
</tr>
<tr>
<td>Yeniden Dene</td><td>Etkinlik yürütme yeniden deneniyor.</td>
</tr>
<tr>
<td>Doğrulama</td><td>Doğrulama henüz başlatılmadı.</td>
</tr>
<tr>
<td>ValidationRetry</td><td>Doğrulama işleminin yeniden gerçekleştirilmesi bekleniyor.</td>
</tr>
<tr>
<tr>
<td rowspan="2">İlerliyor</td><td>Doğrulanıyor</td><td>Doğrulama devam ediyor.</td>
</tr>
<td>-</td>
<td>Dilim işleniyor.</td>
</tr>
<tr>
<td rowspan="4">Başarısız</td><td>Süresi sona erdi</td><td>Etkinlik yürütme etkinlik tarafından izin daha uzun sürdü.</td>
</tr>
<tr>
<td>İptal edildi</td><td>Dilim kullanıcı eylemi tarafından iptal edildi.</td>
</tr>
<tr>
<td>Doğrulama</td><td>Doğrulama başarısız oldu.</td>
</tr>
<tr>
<td>-</td><td>Dilim oluşturulan doğrulandı ve/veya başarısız oldu.</td>
</tr>
<td>Hazır</td><td>-</td><td>Dilim kullanıma hazır.</td>
</tr>
<tr>
<td>Atlandı</td><td>None</td><td>Dilimin işlenmekte olan değil.</td>
</tr>
<tr>
<td>None</td><td>-</td><td>Bir dilim farklı bir durum ile var olmuş ancak sıfırlandı.</td>
</tr>
</table>



Bir dilim girişi tıklayarak bir dilim ilgili ayrıntıları görüntüleyebilirsiniz **en son güncelleştirilen dilimler** dikey.

![Dilim ayrıntıları](./media/data-factory-monitor-manage-pipelines/slice-details.png)

Dilim birden çok kez yürütülmüşse, birden çok satır bkz **etkinlik çalışır** listesi. Çalışma girişi tıklayarak Çalıştır etkinliği hakkında ayrıntılı bilgi görüntüleyebileceğiniz **etkinlik çalışır** listesi. Liste varsa bir hata iletisi ile birlikte tüm günlük dosyalarını, gösterir. Bu özellik görüntülemek ve günlükleri, veri fabrikası ayrılmak zorunda kalmadan hata ayıklama yararlı olur.

![Etkinlik çalışma ayrıntıları](./media/data-factory-monitor-manage-pipelines/activity-run-details.png)

Dilim eklenti yoksa **hazır** durumu, hazır değil ve geçerli dilimin yürütülmesini engelleyen Yukarı Akış dilimleri görebilirsiniz **hazır olmayan Yukarı Akış dilimleri** listesi. Bu özellik, dilim olduğunda yararlıdır **bekleyen** durumu ve dilim bekleyen Yukarı Akış bağımlılıkları anlamak istediğinizde.

![Hazır olmayan yukarı akış dilimleri](./media/data-factory-monitor-manage-pipelines/upstream-slices-not-ready.png)

### <a name="dataset-state-diagram"></a>Veri kümesi durumu diyagramı
Veri Fabrikası dağıtmak ve ardışık düzen geçerli bir etkin döneme sahip sonra dataset geçiş bir durumdan diğerine dilimler. Şu anda, dilim durumu aşağıdaki durumu diyagramı aşağıdaki gibidir:

![Durum Diyagramı](./media/data-factory-monitor-manage-pipelines/state-diagram.png)

Veri fabrikasında veri kümesi durumu geçişi akışı aşağıdaki gibidir: bekleme içinde-ilerleme/Sürüyor (doğrulama) -> hazır/başarısız->.

Dilim başlayacağını bir **bekleyen** yürütülmeden önce karşılanması gereken önkoşulları bekleme durumu. Ardından, yürütme etkinliği başlar ve dilim girmeyeceğini bir **sürüyor** durumu. Etkinlik yürütme başarılı veya başarısız. Dilim olarak işaretlenmiş **hazır** veya **başarısız**bağlı olarak yürütmenin sonucu.

Gelen geri dönmek için dilim sıfırlayabilirsiniz **hazır** veya **başarısız** durumunu **bekleyen** durumu. Dilim durumuna da işaretleyebilirsiniz **atla**, yürütme ve dilim işlenmiyor etkinlik engeller.

## <a name="pause-and-resume-pipelines"></a>Ardışık Düzen Durdur
Azure PowerShell kullanarak işlem hatlarınızı yönetebilirsiniz. Örneğin, duraklatma ve ardışık düzen Azure PowerShell cmdlet'lerini çalıştırarak sürdürme. 

> [!NOTE] 
> Diyagram görünümü, duraklatma ve sürdürme ardışık düzen desteklemez. Bir kullanıcı arabirimi kullanmak istiyorsanız, izleme ve yönetme uygulaması kullanın. Uygulamayı kullanma hakkında daha fazla bilgi için bkz: [izlemek ve Data Factory işlem hatlarını izleme ve yönetim uygulaması kullanarak yönetmek](data-factory-monitor-manage-app.md) makalesi. 

Duraklat/işlem hatları kullanarak askıya alabilirsiniz **Suspend-AzureRmDataFactoryPipeline** PowerShell cmdlet'i. Bu cmdlet, bir sorun düzeltilene kadar hatlarınızı çalıştırmak istemediğiniz yararlıdır. 

```powershell
Suspend-AzureRmDataFactoryPipeline [-ResourceGroupName] <String> [-DataFactoryName] <String> [-Name] <String>
```
Örneğin:

```powershell
Suspend-AzureRmDataFactoryPipeline -ResourceGroupName ADF -DataFactoryName productrecgamalbox1dev -Name PartitionProductsUsagePipeline
```

Ardışık düzene sorun çözüldükten sonra aşağıdaki PowerShell komutunu çalıştırarak askıya alınmış ardışık düzen devam edebilirsiniz:

```powershell
Resume-AzureRmDataFactoryPipeline [-ResourceGroupName] <String> [-DataFactoryName] <String> [-Name] <String>
```
Örneğin:

```powershell
Resume-AzureRmDataFactoryPipeline -ResourceGroupName ADF -DataFactoryName productrecgamalbox1dev -Name PartitionProductsUsagePipeline
```

## <a name="debug-pipelines"></a>Ardışık Düzen hata ayıklama
Azure Data Factory hata ayıklama ve ardışık düzen Azure portalı ve Azure PowerShell kullanarak sorun giderme için zengin özellikleri sağlar.

> [!NOTE] 
> İzleme ve yönetim uygulaması kullanarak troubleshot hataları daha kolay olur. Uygulamayı kullanma hakkında daha fazla bilgi için bkz: [izlemek ve Data Factory işlem hatlarını izleme ve yönetim uygulaması kullanarak yönetmek](data-factory-monitor-manage-app.md) makalesi. 

### <a name="find-errors-in-a-pipeline"></a>Ardışık düzeninde hataları bulma
Ardışık düzeninde etkinlik çalıştırma başarısız olursa, ardışık düzen tarafından üretilen veri kümesi bir hata nedeniyle başarısız durumda. Hata ayıklama ve aşağıdaki yöntemleri kullanarak Azure Data factory'de hatalarında sorun giderme.

#### <a name="use-the-azure-portal-to-debug-an-error"></a>Bir hata ayıklama için Azure portalını kullanma
1. Üzerinde **tablo** dikey penceresinde sahip sorun dilimi tıklatın **durum** kümesine **başarısız**.

   ![Sorun dilim ile tablo dikey penceresi](./media/data-factory-monitor-manage-pipelines/table-blade-with-error.png)
2. Üzerinde **veri dilimi** dikey penceresinde, etkinlik başarısız Çalıştır'ı tıklatın.

   ![Veri dilimi bir hata ile](./media/data-factory-monitor-manage-pipelines/dataslice-with-error.png)
3. Üzerinde **etkinlik çalışma ayrıntıları** dikey penceresinde, Hdınsight işleme ile ilişkili olan dosyaları karşıdan yükleyebilirsiniz. Tıklatın **karşıdan** hata hakkında ayrıntılar içeren hata günlüğü dosyası karşıdan yüklemek durum/stderr için.

   ![Etkinlik ayrıntıları dikey penceresinde hatası ile çalıştırma](./media/data-factory-monitor-manage-pipelines/activity-run-details-with-error.png)     

#### <a name="use-powershell-to-debug-an-error"></a>Bir hata ayıklama için PowerShell kullanma
1. **PowerShell**’i başlatın.
2. Çalıştırma **Get-AzureRmDataFactorySlice** dilimler ve bunların durumlarını görmek için komutu. Durumundaki bir dilim görmeniz gerekir **başarısız**.        

    ```powershell   
    Get-AzureRmDataFactorySlice [-ResourceGroupName] <String> [-DataFactoryName] <String> [-DatasetName] <String> [-StartDateTime] <DateTime> [[-EndDateTime] <DateTime> ] [-Profile <AzureProfile> ] [ <CommonParameters>]
    ```   
   Örneğin:

    ```powershell   
    Get-AzureRmDataFactorySlice -ResourceGroupName ADF -DataFactoryName LogProcessingFactory -DatasetName EnrichedGameEventsTable -StartDateTime 2014-05-04 20:00:00
    ```

   Değiştir **StartDateTime** hattınızı başlangıç saati ile. 
3. Şimdi Çalıştır **Get-AzureRmDataFactoryRun** etkinliği hakkında ayrıntılı bilgi almak için cmdlet'i çalıştırmak için dilim.

    ```powershell   
    Get-AzureRmDataFactoryRun [-ResourceGroupName] <String> [-DataFactoryName] <String> [-DatasetName] <String> [-StartDateTime]
    <DateTime> [-Profile <AzureProfile> ] [ <CommonParameters>]
    ```

    Örneğin:

    ```powershell   
    Get-AzureRmDataFactoryRun -ResourceGroupName ADF -DataFactoryName LogProcessingFactory -DatasetName EnrichedGameEventsTable -StartDateTime "5/5/2014 12:00:00 AM"
    ```

    StartDateTime, önceki adımda not ettiğiniz hata/sorun dilim için başlangıç zamanı değeridir. Tarih-saat çift tırnak içine alınabilir.
4. Çıktı aşağıdakine benzer hata ayrıntılarıyla birlikte görmeniz gerekir:

    ```   
    Id                      : 841b77c9-d56c-48d1-99a3-8c16c3e77d39
    ResourceGroupName       : ADF
    DataFactoryName         : LogProcessingFactory3
    DatasetName               : EnrichedGameEventsTable
    ProcessingStartTime     : 10/10/2014 3:04:52 AM
    ProcessingEndTime       : 10/10/2014 3:06:49 AM
    PercentComplete         : 0
    DataSliceStart          : 5/5/2014 12:00:00 AM
    DataSliceEnd            : 5/6/2014 12:00:00 AM
    Status                  : FailedExecution
    Timestamp               : 10/10/2014 3:04:52 AM
    RetryAttempt            : 0
    Properties              : {}
    ErrorMessage            : Pig script failed with exit code '5'. See wasb://        adfjobs@spestore.blob.core.windows.net/PigQuery
                                    Jobs/841b77c9-d56c-48d1-99a3-
                8c16c3e77d39/10_10_2014_03_04_53_277/Status/stderr' for
                more details.
    ActivityName            : PigEnrichLogs
    PipelineName            : EnrichGameLogsPipeline
    Type                    :
    ```
5. Çalıştırabilirsiniz **Kaydet AzureRmDataFactoryLog** çıkışı görmek ve günlük dosyalarını kullanarak indirmek kimliği değeri cmdlet'iyle **- DownloadLogsoption** cmdlet'i için.

    ```powershell
    Save-AzureRmDataFactoryLog -ResourceGroupName "ADF" -DataFactoryName "LogProcessingFactory" -Id "841b77c9-d56c-48d1-99a3-8c16c3e77d39" -DownloadLogs -Output "C:\Test"
    ```

## <a name="rerun-failures-in-a-pipeline"></a>Ardışık düzeninde hataları yeniden çalıştırın

> [!IMPORTANT]
> Hataları gidermek ve başarısız dilimler izleme ve yönetim uygulaması kullanarak yeniden kolaydır. Uygulamayı kullanma hakkında daha fazla bilgi için bkz: [izlemek ve Data Factory işlem hatlarını izleme ve yönetim uygulaması kullanarak yönetmek](data-factory-monitor-manage-app.md). 

### <a name="use-the-azure-portal"></a>Azure portalı kullanma
Sorun giderme ve hata ayıklama hataları ardışık düzeninde sonra hata dilim gezinme ve tıklayarak hataları çalıştırabilirsiniz **çalıştırmak** komut çubuğundan düğme.

![Başarısız bir dilimi yeniden çalıştırın](./media/data-factory-monitor-manage-pipelines/rerun-slice.png)

(Örneğin, veri kullanılamıyorsa) durumda dilim doğrulama bir ilke hatası nedeniyle başarısız oldu, hata düzeltme ve tıklayarak doğrulamak **doğrulama** komut çubuğundan düğme.

![Hataları giderin ve doğrulama](./media/data-factory-monitor-manage-pipelines/fix-error-and-validate.png)

### <a name="use-azure-powershell"></a>Azure PowerShell kullanma
Kullanarak hataları çalıştırabilirsiniz **Set-AzureRmDataFactorySliceStatus** cmdlet'i. Bkz: [Set-AzureRmDataFactorySliceStatus](https://msdn.microsoft.com/library/mt603522.aspx) sözdizimi ve cmdlet ile ilgili diğer ayrıntıları için konu.

**Örnek:**

Aşağıdaki örnek tablosunun tüm dilimleri durumunu 'DAWikiAggregatedData' 'Azure veri fabrikası 'WikiADF' bekleyen' ayarlar.

'Güncelleştirme 'türü 'tablosu için her dilimi ve tüm bağımlı (Yukarı Akış) tabloları durumları 'Bekleyen' ayarlandığından anlamı Upstreamınpipeline için', ayarlanır. Bu parametre için diğer olası değer 'Bireysel' dir.

```powershell
Set-AzureRmDataFactorySliceStatus -ResourceGroupName ADF -DataFactoryName WikiADF -DatasetName DAWikiAggregatedData -Status Waiting -UpdateType UpstreamInPipeline -StartDateTime 2014-05-21T16:00:00 -EndDateTime 2014-05-21T20:00:00
```
## <a name="create-alerts-in-the-azure-portal"></a>Azure portalında uyarılar oluştur

1.  Azure portal ve select oturum açma **İzleyicisi -> Uyarılar** uyarıları sayfasını açın.

    ![Uyarılar sayfasında açın.](media/data-factory-monitor-manage-pipelines/v1alerts-image1.png)

2.  Seçin **+ yeni uyarı kuralı** yeni bir uyarı oluşturmak için.

    ![Yeni bir uyarı oluştur](media/data-factory-monitor-manage-pipelines/v1alerts-image2.png)

3.  Tanımlamak **Uyarı koşulu**. (Seçtiğinizden emin olun **veri fabrikaları** içinde **kaynak türüne göre filtre** alan.) Ayrıca değerlerini belirtebilirsiniz **boyutları**.

    ![Uyarı koşulu - Select hedef tanımlayın](media/data-factory-monitor-manage-pipelines/v1alerts-image3.png)

    ![Uyarı koşulu tanımla - Uyarı ölçütleri ekleme](media/data-factory-monitor-manage-pipelines/v1alerts-image4.png)

    ![Uyarı koşulu tanımla - uyarı mantığı ekleyin](media/data-factory-monitor-manage-pipelines/v1alerts-image5.png)

4.  Tanımlamak **uyarı ayrıntıları**.

    ![Uyarı ayrıntılarını tanımlayın](media/data-factory-monitor-manage-pipelines/v1alerts-image6.png)

5.  Tanımlamak **eylem grubu**.

    ![Eylem grubunu tanımlayın - yeni bir eylem grubu oluşturun.](media/data-factory-monitor-manage-pipelines/v1alerts-image7.png)

    ![Eylem Grup - özelliklerini ayarlama tanımlayın](media/data-factory-monitor-manage-pipelines/v1alerts-image8.png)

    ![Eylem Grup - oluşturulan yeni eylem Grup tanımlayın](media/data-factory-monitor-manage-pipelines/v1alerts-image9.png)

## <a name="move-a-data-factory-to-a-different-resource-group-or-subscription"></a>Veri Fabrikası farklı bir kaynak grubuna veya aboneliğe taşıma
Kullanarak farklı bir kaynak grubunda veya farklı bir abonelik için bir veri fabrikası taşıyabilirsiniz **taşıma** veri fabrikanızın giriş sayfasında düğme çubuğu komutu.

![Veri Fabrikası taşıma](./media/data-factory-monitor-manage-pipelines/MoveDataFactory.png)

Ayrıca, ilgili kaynakları (örneğin, data factory ile ilişkili olan uyarılar), veri fabrikası birlikte taşıyabilirsiniz.

![Taşıma kaynaklar iletişim kutusu](./media/data-factory-monitor-manage-pipelines/MoveResources.png)
