---
title: İzleme ve işlem hatları, PowerShell ve Azure portalını kullanarak yönetme | Microsoft Docs
description: Azure veri fabrikası ve oluşturduğunuz işlem hatlarını yönetmek ve izlemek için Azure PowerShell ve Azure Portalı'nı kullanmayı öğrenin.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.assetid: 9b0fdc59-5bbe-44d1-9ebc-8be14d44def9
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/30/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: 64fae56bfc95b62bd60444d49100689845f64278
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66123145"
---
# <a name="monitor-and-manage-azure-data-factory-pipelines-by-using-the-azure-portal-and-powershell"></a>İzleme ve Azure portalı ve PowerShell kullanarak Azure Data Factory işlem hatlarını yönetme
> [!div class="op_single_selector"]
> * [Azure portal/Azure PowerShell kullanma](data-factory-monitor-manage-pipelines.md)
> * [Kullanarak izleme ve yönetim uygulaması](data-factory-monitor-manage-app.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [izlemek ve yönetmek, Data Factory işlem hatlarını](../monitor-visually.md).

Bu makalede, izleme, yönetme ve işlem hatlarınızı Azure portal ve PowerShell kullanarak hata ayıklama açıklar.

> [!IMPORTANT]
> İzleme ve yönetim uygulaması izleme ve veri işlem hatlarınızı yönetme ve her türlü sorunu gidermek için daha iyi destek sağlar. Uygulamayı kullanma hakkında daha fazla ayrıntı için bkz. [izlemek ve Data Factory işlem hatlarını izleme ve yönetme uygulamasını kullanarak yönetmek](data-factory-monitor-manage-app.md). 

> [!IMPORTANT]
> Azure Data Factory sürüm 1 şimdi kullanan yeni [Azure İzleyici'de altyapı uyarı](../../monitoring-and-diagnostics/monitor-alerts-unified-usage.md). Eski uyarı altyapı kullanım dışı bırakılmıştır. Sonuç olarak, mevcut uyarılarınızı sürüm 1 veri fabrikaları artık çalışmıyor yapılandırılmış. Mevcut uyarılarınızı v1 veri fabrikaları için otomatik olarak geçirilmez. Bu uyarılar yeni uyarı altyapı oluşturmanız gerekebilir. Azure portal ve select oturum **İzleyici** yeni uyarılar ölçümleri (örneğin, başarısız çalıştırmaları veya başarılı çalıştırmalar) sürümünüz için 1 veri fabrikası oluşturmak için.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="understand-pipelines-and-activity-states"></a>İşlem hattı ve etkinlik durumlarını anlama
Azure portalını kullanarak şunları yapabilirsiniz:

* Veri fabrikanızın diyagram görüntüleyin.
* Bir işlem hattı içindeki etkinlikleri görüntüleyin.
* Giriş ve çıkış veri kümelerini görüntüleyin.

Bu bölümde, nasıl bir veri kümesi dilim bir durumdan başka bir duruma geçiş de açıklanmaktadır.   

### <a name="navigate-to-your-data-factory"></a>Veri fabrikanıza gidin
1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Tıklayın **veri fabrikaları** soldaki menüsünde. Görmüyorsanız, tıklayın **diğer hizmetler >** ve ardından **veri fabrikaları** altında **ZEKA + ANALİZ** kategorisi.

   ![Tümüne Gözat > veri fabrikaları](./media/data-factory-monitor-manage-pipelines/browseall-data-factories.png)
3. Üzerinde **veri fabrikaları** dikey penceresinde, ilgilendiğiniz veri fabrikası'nı seçin.

    ![Veri fabrikası seçme](./media/data-factory-monitor-manage-pipelines/select-data-factory.png)

   Veri fabrikasının giriş sayfasını görmeniz gerekir.

   ![Veri fabrikası dikey penceresi](./media/data-factory-monitor-manage-pipelines/data-factory-blade.png)

#### <a name="diagram-view-of-your-data-factory"></a>Veri fabrikanızın diyagram görünümü
**Diyagram** görünümü bir data Factory veri fabrikasına ve varlıklarını yönetmek ve izlemek için tek bir panel sağlar. Görmek için **diyagram** görünümünde veri fabrikanızı, tıklayın **diyagram** veri fabrikasının giriş sayfasında.

![Diyagram görünümü](./media/data-factory-monitor-manage-pipelines/diagram-view.png)

Yakınlaştırmak, uzaklaştırabilir, uygun, % 100 Yakınlaştır, diyagramın düzenini kilitleyin ve işlem hatlarını ve veri kümeleri otomatik olarak konumlandırma Yakınlaştır. Veri kökenini bilgileri de görebilirsiniz (yani, seçilen öğelerin yukarı ve aşağı akış öğelerini göster).

### <a name="activities-inside-a-pipeline"></a>Bir işlem hattı içindeki etkinlikleri
1. İşlem hattı sağ tıklayın ve ardından **ardışık düzeni Aç** etkinlikler için girdi ve çıktı veri kümeleri ile birlikte, işlem hattındaki tüm etkinlikleri görmek için. Bu özellik, birden fazla etkinlik, işlem hattı içerir ve tek bir işlem hattının işlem hatlarınız anlamak istediğinizde yararlıdır.

    ![İşlem hattı menüsünü açma](./media/data-factory-monitor-manage-pipelines/open-pipeline-menu.png)     
2. Aşağıdaki örnekte, bir kopyalama etkinliği bir girdi ve çıktı ile işlem hattını görürsünüz. 

    ![Bir işlem hattı içindeki etkinlikleri](./media/data-factory-monitor-manage-pipelines/activities-inside-pipeline.png)
3. Ana sayfaya geri data Factory tıklayarak gidebilirsiniz **veri fabrikası** yer alan içerik haritasındaki sol üst köşesinde bağlantı.

    ![Data factory'ye geri gidin](./media/data-factory-monitor-manage-pipelines/navigate-back-to-data-factory.png)

### <a name="view-the-state-of-each-activity-inside-a-pipeline"></a>Bir işlem hattı içindeki her bir etkinlik durumunu görüntüleme
Herhangi bir etkinlik tarafından oluşturulan veri kümelerini durumunu görüntüleyerek bir etkinliğin geçerli durumunu görüntüleyebilirsiniz.

Çift tıklayarak **OutputBlobTable** içinde **diyagram**, bir işlem hattı içindeki farklı etkinlik çalıştırmalarını tarafından üretilen tüm dilimleri görebilirsiniz. Kopyalama etkinliği için başarıyla son sekiz saat çalıştı ve dilimlerinin içinde gördüğünüz **hazır** durumu.  

![Ardışık Düzen durumu](./media/data-factory-monitor-manage-pipelines/state-of-pipeline.png)

Data factory veri kümesi dilimleri aşağıdaki durumlardan birine sahip olabilir:

<table>
<tr>
    <th align="left">Eyalet</th><th align="left">Alt durumu eşleştirildi</th><th align="left">Açıklama</th>
</tr>
<tr>
    <td rowspan="8">Bekleniyor</td><td>ScheduleTime</td><td>Dilimin çalıştırılma zamanı gelen edilmemiş.</td>
</tr>
<tr>
<td>DatasetDependencies</td><td>Yukarı Akış bağımlılıkları hazır değil.</td>
</tr>
<tr>
<td>ComputeResources</td><td>İşlem kaynakları kullanılamıyor.</td>
</tr>
<tr>
<td>ConcurrencyLimit</td> <td>Tüm etkinlik örnekleri diğer dilimleri çalıştırıyor.</td>
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
<td>ValidationRetry</td><td>Doğrulama denenmesi için bekliyor.</td>
</tr>
<tr>
<tr>
<td rowspan="2">Devam ediyor</td><td>Doğrulama</td><td>Doğrulama işlemi devam ediyor.</td>
</tr>
<td>-</td>
<td>Dilim işleniyor.</td>
</tr>
<tr>
<td rowspan="4">Başarısız</td><td>Zaman aşımına uğradı</td><td>Etkinlik yürütme etkinliği tarafından izin verilenden daha uzun sürdü.</td>
</tr>
<tr>
<td>İptal edildi</td><td>Dilim kullanıcı eylemiyle iptal edildi.</td>
</tr>
<tr>
<td>Doğrulama</td><td>Doğrulama başarısız oldu.</td>
</tr>
<tr>
<td>-</td><td>Dilim oluşturulan doğrulandı ve/veya başarısız oldu.</td>
</tr>
<td>Hazır</td><td>-</td><td>Dilim kullanıma hazır olur.</td>
</tr>
<tr>
<td>Atlandı</td><td>None</td><td>Dilimin işlenmekte olan değildir.</td>
</tr>
<tr>
<td>None</td><td>-</td><td>Farklı bir durum ile bir dilim kullanılır, ancak sıfırlandı.</td>
</tr>
</table>



Bir dilim girişi tıklayarak bir dilim ayrıntılarını görüntüleyebilirsiniz **en son güncelleştirilen dilimler** dikey penceresi.

![Dilimi ayrıntıları](./media/data-factory-monitor-manage-pipelines/slice-details.png)

Dilim birden çok kez yürütüldü birden çok satır görürsünüz **etkinlik çalıştırmalarını** listesi. Çalıştırma girdiye tıklayarak çalıştırın bir etkinliği hakkında ayrıntılı bilgi görüntüleyebileceğiniz **etkinlik çalıştırmalarını** listesi. Varsa bir hata iletisi ile birlikte tüm günlük dosyaları, liste gösterir. Bu özellik, veri fabrikanıza ayrılmak zorunda kalmadan hata ayıklama günlüklerini görüntülemek yararlıdır.

![Etkinlik çalışma ayrıntıları](./media/data-factory-monitor-manage-pipelines/activity-run-details.png)

Dilim değilse **hazır** durumu, hazır değil ve geçerli dilimin yürütülmesini engelleyen Yukarı Akış dilimleri görebilirsiniz **hazır olmayan Yukarı Akış dilimleri** listesi. Bu özellik, dilim olduğunda yararlıdır **bekleyen** duruma ve istediğinizde dilimi bekliyor Yukarı Akış bağımlılıkları anlamak.

![Hazır olmayan Yukarı Akış dilimleri](./media/data-factory-monitor-manage-pipelines/upstream-slices-not-ready.png)

### <a name="dataset-state-diagram"></a>Veri kümesi durum diyagramı
Veri Fabrikası dağıtabiliyorum ve işlem hatlarını geçerli etkin bir süre sonra veri kümesi geçiş bir durumdan diğerine böler. Dilim durumu şu anda aşağıdaki durum diyagramı aşağıdaki gibidir:

![Durum Diyagramı](./media/data-factory-monitor-manage-pipelines/state-diagram.png)

Data factory'de veri kümesi durumu geçiş akışı aşağıdaki gibidir: Bekletme -> içinde-ilerleme / (doğrulama) sürüyor hazır/başarısız oldu->.

Dilim başlatılacağı bir **bekleyen** yürütülmeden önce karşılanması gereken önkoşulları bekleme durumu. Ardından etkinlik çalıştırmaya başlar ve dilim girmeyeceğini bir **sürüyor** durumu. Etkinlik yürütme başarılı veya başarısız. Dilim olarak işaretlenmiş **hazır** veya **başarısız**göre yürütmenin sonucu.

Öğesinden geri dönmek için dilim sıfırlayabilirsiniz **hazır** veya **başarısız** durumunu **bekleyen** durumu. Dilim durumu da işaretleyebilirsiniz **atla**, Etkinlik yürütme ve dilim işleme değil engeller.

## <a name="pause-and-resume-pipelines"></a>Duraklatma ve sürdürme işlem hatları
İşlem hatlarınızı Azure PowerShell kullanarak yönetebilirsiniz. Örneğin, duraklatma ve Azure PowerShell cmdlet'lerini çalıştırarak işlem hatları sürdürün. 

> [!NOTE] 
> Diyagram görünümü, duraklatma ve sürdürme işlem hatları desteklemez. Bir kullanıcı arabirimi kullanmak istiyorsanız, izleme ve yönetme uygulaması kullanın. Uygulamayı kullanma hakkında daha fazla ayrıntı için bkz. [izlemek ve Data Factory işlem hatlarını izleme ve yönetme uygulamasını kullanarak yönetmek](data-factory-monitor-manage-app.md) makalesi. 

Duraklatma/işlem hatları kullanarak askıya alabilirsiniz **Suspend-AzDataFactoryPipeline** PowerShell cmdlet'i. Bu cmdlet, bir sorun düzeltilene kadar işlem hatlarınızı çalıştırmak istemediğiniz zaman yararlıdır. 

```powershell
Suspend-AzDataFactoryPipeline [-ResourceGroupName] <String> [-DataFactoryName] <String> [-Name] <String>
```
Örneğin:

```powershell
Suspend-AzDataFactoryPipeline -ResourceGroupName ADF -DataFactoryName productrecgamalbox1dev -Name PartitionProductsUsagePipeline
```

İşlem hattı çalıştırmasıyla sorun çözüldükten sonra aşağıdaki PowerShell komutunu çalıştırarak askıya alınmış işlem hattı devam edebilir:

```powershell
Resume-AzDataFactoryPipeline [-ResourceGroupName] <String> [-DataFactoryName] <String> [-Name] <String>
```
Örneğin:

```powershell
Resume-AzDataFactoryPipeline -ResourceGroupName ADF -DataFactoryName productrecgamalbox1dev -Name PartitionProductsUsagePipeline
```

## <a name="debug-pipelines"></a>İşlem hatları hata ayıklama
Azure Data Factory, Azure portalı ve Azure PowerShell kullanarak komut zincirlerinin sorunlarını giderme ve hata ayıklama için zengin özellikler sunar.

> [!NOTE] 
> İzleme ve yönetim uygulaması kullanarak troubleshot hataları daha kolaydır. Uygulamayı kullanma hakkında daha fazla ayrıntı için bkz. [izlemek ve Data Factory işlem hatlarını izleme ve yönetme uygulamasını kullanarak yönetmek](data-factory-monitor-manage-app.md) makalesi. 

### <a name="find-errors-in-a-pipeline"></a>Bir işlem hattında hataları bulun
Bir işlem hattında etkinlik çalıştırma başarısız olursa, işlem hattı tarafından üretilen veri kümesi bir hata nedeniyle başarısız durumda. Hata ayıklama ve aşağıdaki yöntemleri kullanarak Azure Data factory'de hatalarını giderme.

#### <a name="use-the-azure-portal-to-debug-an-error"></a>Bir hata ayıklama için Azure portalını kullanma
1. Üzerinde **tablo** dikey penceresinde bulunan sorun dilimine tıklayın **durumu** kümesine **başarısız**.

   ![Tablo dikey penceresinin sorun dilim](./media/data-factory-monitor-manage-pipelines/table-blade-with-error.png)
2. Üzerinde **veri dilimi** dikey penceresinde etkinliğin başarısız Çalıştır'ı tıklatın.

   ![Veri dilimi hata](./media/data-factory-monitor-manage-pipelines/dataslice-with-error.png)
3. Üzerinde **etkinlik çalıştırması ayrıntıları** dikey penceresinde, HDInsight işlemeyle ilişkili olan dosyaları karşıdan yükleyebilirsiniz. Tıklayın **indirme** hata hakkındaki ayrıntılar içeren hata günlük dosyasını indirmek durum/stderr için.

   ![Etkinlik çalıştırma hatası ile ayrıntıları dikey penceresi](./media/data-factory-monitor-manage-pipelines/activity-run-details-with-error.png)     

#### <a name="use-powershell-to-debug-an-error"></a>Bir hata ayıklama için PowerShell kullanma
1. **PowerShell**’i başlatın.
2. Çalıştırma **Get-AzDataFactorySlice** dilimleri ve bunların durumlarını görmek için komutu. Durumunu içeren bir dilim görmeniz gerekir **başarısız**.        

    ```powershell   
    Get-AzDataFactorySlice [-ResourceGroupName] <String> [-DataFactoryName] <String> [-DatasetName] <String> [-StartDateTime] <DateTime> [[-EndDateTime] <DateTime> ] [-Profile <AzureProfile> ] [ <CommonParameters>]
    ```   
   Örneğin:

    ```powershell   
    Get-AzDataFactorySlice -ResourceGroupName ADF -DataFactoryName LogProcessingFactory -DatasetName EnrichedGameEventsTable -StartDateTime 2014-05-04 20:00:00
    ```

   Değiştirin **StartDateTime** değerlerini işlem hattınızın başlangıç saatine sahip. 
3. Şimdi **Get-AzDataFactoryRun** etkinliği hakkında ayrıntılı bilgi almak için cmdlet çalıştırma için dilim.

    ```powershell   
    Get-AzDataFactoryRun [-ResourceGroupName] <String> [-DataFactoryName] <String> [-DatasetName] <String> [-StartDateTime]
    <DateTime> [-Profile <AzureProfile> ] [ <CommonParameters>]
    ```

    Örneğin:

    ```powershell   
    Get-AzDataFactoryRun -ResourceGroupName ADF -DataFactoryName LogProcessingFactory -DatasetName EnrichedGameEventsTable -StartDateTime "5/5/2014 12:00:00 AM"
    ```

    StartDateTime, önceki adımda not ettiğiniz hata/sorun dilimin başlangıç zamanı değeridir. Tarih-saat çift tırnak içine alınmalıdır.
4. Aşağıdakine benzer hata ayrıntılarını içeren bir çıktı görmeniz gerekir:

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
5. Çalıştırabileceğiniz **Kaydet AzDataFactoryLog** kimliği değeri çıktısını görmek ve günlük dosyalarını kullanarak karşıdan bir cmdlet'le **- DownloadLogsoption** cmdlet'i için.

    ```powershell
    Save-AzDataFactoryLog -ResourceGroupName "ADF" -DataFactoryName "LogProcessingFactory" -Id "841b77c9-d56c-48d1-99a3-8c16c3e77d39" -DownloadLogs -Output "C:\Test"
    ```

## <a name="rerun-failures-in-a-pipeline"></a>İşlem hattındaki hataları yeniden çalıştırma

> [!IMPORTANT]
> Hataları gidermek ve başarısız olan dilimler izleme ve yönetim uygulaması kullanarak yeniden çalıştırın. daha kolaydır. Uygulamayı kullanma hakkında daha fazla ayrıntı için bkz. [izlemek ve Data Factory işlem hatlarını izleme ve yönetme uygulamasını kullanarak yönetmek](data-factory-monitor-manage-app.md). 

### <a name="use-the-azure-portal"></a>Azure portalı kullanma
Sorun giderme ve hata ayıklama hataları ardışık düzeninde sonra hata dilimi gezinme ve tıklayarak hataları çalıştırabilirsiniz **çalıştırma** komut çubuğunda düğme.

![Başarısız bir dilimi yeniden çalıştırın](./media/data-factory-monitor-manage-pipelines/rerun-slice.png)

(Örneğin, veriler kullanılabilir değilse) durumda dilim doğrulama bir ilke hatası nedeniyle başarısız oldu, hatayı düzeltin ve tekrar tıklayarak doğrulama **doğrulama** komut çubuğunda düğme.

![Hataları düzeltin ve doğrulama](./media/data-factory-monitor-manage-pipelines/fix-error-and-validate.png)

### <a name="use-azure-powershell"></a>Azure PowerShell kullanma
Kullanarak, hataları yeniden çalıştırabilirsiniz **kümesi AzDataFactorySliceStatus** cmdlet'i. Bkz: [kümesi AzDataFactorySliceStatus](https://docs.microsoft.com/powershell/module/az.datafactory/set-azdatafactoryslicestatus) konu sözdizimi ve cmdlet ile ilgili diğer ayrıntıları.

**Örnek:**

Aşağıdaki örnek tablosunun tüm dilimleri durumunu 'DAWikiAggregatedData' 'Azure data factory'de 'WikiADF' Bekliyor' ayarlar.

'Güncelleştirme 'türü, 'tablosu için her bir dilimi ve tüm bağımlı (Yukarı Akış) tabloları durumları 'Bekliyor' ayarlandığından anlamına Upstreamınpipeline için', ayarlanır. Bu parametre için diğer olası değer 'Bireysel' dir.

```powershell
Set-AzDataFactorySliceStatus -ResourceGroupName ADF -DataFactoryName WikiADF -DatasetName DAWikiAggregatedData -Status Waiting -UpdateType UpstreamInPipeline -StartDateTime 2014-05-21T16:00:00 -EndDateTime 2014-05-21T20:00:00
```
## <a name="create-alerts-in-the-azure-portal"></a>Azure portalında uyarı oluşturma

1.  Azure portal ve select oturum **İzleyicisi -> Uyarılar** uyarılar sayfasını açın.

    ![Uyarılar sayfasını açın.](media/data-factory-monitor-manage-pipelines/v1alerts-image1.png)

2.  Seçin **+ yeni uyarı kuralı** yeni bir uyarı oluşturmak için.

    ![Yeni bir uyarı oluştur](media/data-factory-monitor-manage-pipelines/v1alerts-image2.png)

3.  Tanımlama **Uyarı koşulu**. (Seçtiğinizden emin olun **veri fabrikaları** içinde **kaynak türüne göre filtre** alan.) İçin değerler belirtebilirsiniz **boyutları**.

    ![-Hedefi seçme gibi uyarı koşulunu tanımlama](media/data-factory-monitor-manage-pipelines/v1alerts-image3.png)

    ![Uyarı koşulunu tanımlama - uyarı ölçütü Ekle](media/data-factory-monitor-manage-pipelines/v1alerts-image4.png)

    ![Uyarı koşulunu tanımlama - uyarı mantığı ekleyin](media/data-factory-monitor-manage-pipelines/v1alerts-image5.png)

4.  Tanımlama **uyarı ayrıntıları**.

    ![Uyarı ayrıntılarını tanımlama](media/data-factory-monitor-manage-pipelines/v1alerts-image6.png)

5.  Tanımlama **eylem grubu**.

    ![Eylem grubunu tanımlama - yeni bir eylem grubu oluştur](media/data-factory-monitor-manage-pipelines/v1alerts-image7.png)

    ![Eylem grubunu - kümesi özellikleri tanımlama](media/data-factory-monitor-manage-pipelines/v1alerts-image8.png)

    ![-Yeni eylem grubu oluşturulan eylem grubunu tanımlama](media/data-factory-monitor-manage-pipelines/v1alerts-image9.png)

## <a name="move-a-data-factory-to-a-different-resource-group-or-subscription"></a>Veri fabrikası, bir farklı kaynak grubuna veya aboneliğe taşıma
Kullanarak farklı bir kaynak grubunda veya farklı bir abonelik için bir veri fabrikası taşıyabilirsiniz **taşıma** veri fabrikanızın giriş sayfasında düğme çubuğu komutu.

![Veri Fabrikası Taşı](./media/data-factory-monitor-manage-pipelines/MoveDataFactory.png)

Ayrıca, tüm ilgili kaynakları (örneğin, data factory ile ilişkili olan uyarılar), veri fabrikası ile birlikte taşıyabilirsiniz.

![İletişim kutusu kaynakları Taşı](./media/data-factory-monitor-manage-pipelines/MoveResources.png)
