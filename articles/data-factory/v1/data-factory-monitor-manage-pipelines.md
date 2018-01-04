---
title: "İzleme ve ardışık düzen Azure portal ve PowerShell kullanarak yönetme | Microsoft Docs"
description: "Azure data factory'leri ve oluşturduğunuz ardışık düzen yönetmek ve izlemek için Azure portalı ve Azure PowerShell kullanmayı öğrenin."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 9b0fdc59-5bbe-44d1-9ebc-8be14d44def9
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2017
ms.author: spelluru
robots: noindex
ms.openlocfilehash: ccc0755385d2f170939e5c19f32b168132b6839b
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2017
---
# <a name="monitor-and-manage-azure-data-factory-pipelines-by-using-the-azure-portal-and-powershell"></a>İzleme ve Azure portalı ve PowerShell kullanarak Azure Data Factory işlem hatlarını yönetme
> [!div class="op_single_selector"]
> * [Azure portal/Azure PowerShell kullanma](data-factory-monitor-manage-pipelines.md)
> * [Kullanılarak izleme ve yönetim uygulaması](data-factory-monitor-manage-app.md)

> [!NOTE]
> Bu makale, Data Factory’nin genel kullanıma açık olan (GA) 1. sürümü için geçerlidir. Önizlemede değil, Data Factory hizmetinin 2 sürümünü kullanıyorsanız bkz [izlemek ve 2 sürümündeki Data Factory işlem hatlarını yönetmek](../monitor-visually.md).

> [!IMPORTANT]
> İzleme ve yönetim uygulama izleme ve veri işlem hatlarınızı yönetmek ve sorunları gidermek için daha iyi destek sağlar. Uygulamayı kullanma hakkında daha fazla bilgi için bkz: [izlemek ve Data Factory işlem hatlarını izleme ve yönetim uygulaması kullanarak yönetmek](data-factory-monitor-manage-app.md). 


Bu makalede, izleme, yönetme ve işlem hatlarınızı Azure portalı ve PowerShell kullanarak hata ayıklama açıklar. Makaleyi uyarıları oluşturma ve hatalar hakkında bilgi edinin bilgiler de sağlar.

## <a name="understand-pipelines-and-activity-states"></a>Ardışık Düzen ve etkinlik durumlarını anlama
Azure Portalı'nı kullanarak şunları yapabilirsiniz:

* Veri fabrikanızın diyagramı olarak görüntüleyin.
* Bir ardışık düzendeki etkinlik görünümü.
* Giriş ve çıkış veri kümeleri görüntüleyin.

Bu bölümde, nasıl bir veri kümesi dilim başka bir duruma bir durumdan diğerine geçer de açıklanmaktadır.   

### <a name="navigate-to-your-data-factory"></a>Veri fabrikanızın gidin
1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Tıklatın **veri fabrikaları** soldaki menüde. Göremiyorsanız, tıklatın **daha fazla hizmet >**ve ardından **veri fabrikaları** altında **INTELLİGENCE + ANALİZ** kategorisi.

   ![Tümüne Gözat > veri fabrikaları](./media/data-factory-monitor-manage-pipelines/browseall-data-factories.png)
3. Üzerinde **veri fabrikaları** dikey penceresinde, ilgilendiğiniz data factory seçin.

    ![Veri fabrikası seçme](./media/data-factory-monitor-manage-pipelines/select-data-factory.png)

   Data factory giriş sayfasını görmeniz gerekir.

   ![Data factory dikey penceresi](./media/data-factory-monitor-manage-pipelines/data-factory-blade.png)

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
<td>Atlandı</td><td>Hiçbiri</td><td>Dilimin işlenmekte olan değil.</td>
</tr>
<tr>
<td>Hiçbiri</td><td>-</td><td>Bir dilim farklı bir durum ile var olmuş ancak sıfırlandı.</td>
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

> [! Not} izleme ve yönetim uygulaması kullanarak troubleshot hataları daha kolaydır. Uygulamayı kullanma hakkında daha fazla bilgi için bkz: [izlemek ve Data Factory işlem hatlarını izleme ve yönetim uygulaması kullanarak yönetmek](data-factory-monitor-manage-app.md) makalesi. 

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

## <a name="create-alerts"></a>Uyarı oluşturma
Azure günlükleri kullanıcı olayları bir Azure kaynak olduğunda (örneğin, veri fabrikası) oluşturulan, güncelleştirilmiş veya silinmiş. Bu olaylara bağlı olarak uyarıları oluşturabilirsiniz. Veri Fabrikası çeşitli ölçümleri yakalamak ve ölçümleri uyarıları oluşturmak için kullanabilirsiniz. Gerçek zamanlı izleme için olayları kullanın ve ölçümleri geçmiş amaçları için kullanmak öneririz.

### <a name="alerts-on-events"></a>Uyarı olayları
Azure olayları, Azure kaynaklarınızı neler olduğunu içine yararlı bilgiler sağlar. Azure Data Factory kullanırken, olaylar oluşturulur zaman:

* Veri Fabrikası oluşturulduğunda, silinmiş veya güncelleştirildiğinde.
* Veri işleme ("çalışır") olarak başlatıldı veya tamamlandı.
* İsteğe bağlı Hdınsight kümesi oluşturulduğunda veya kaldırılmış.

Bu kullanıcı olaylarına uyarılar oluşturabilir ve aboneliğin coadministrators ve yönetici e-posta bildirimleri gönderecek şekilde yapılandırın. Ayrıca, koşullar karşılandığında e-posta bildirimleri alması gereken kullanıcıların ek e-posta adresi belirtebilirsiniz. Bu özellik, veri fabrikası sürekli izlemek istemediğiniz ve hatalar hakkında bilgi edinin istediğinizde yararlıdır.

> [!NOTE]
> Şu anda, portal olayları uyarıları göstermez. Kullanım [izleme ve yönetim uygulaması](data-factory-monitor-manage-app.md) tüm uyarıları görmek için.


#### <a name="specify-an-alert-definition"></a>Bir uyarı tanımı belirtin
Bir uyarı tanımı belirtmek için uyarı almak istediğiniz işlemleri açıklayan bir JSON dosyası oluşturun. Aşağıdaki örnekte, bir e-posta bildirimi RunFinished işlem için uyarı gönderir. Belirli olması için bir e-posta bildirimi data factory çalıştırılmasıyla tamamladı ve çalıştırma başarısız oldu, gönderilen (durum = FailedExecution).

```JSON
{
    "contentVersion": "1.0.0.0",
     "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "parameters": {},
    "resources":
    [
        {
            "name": "ADFAlertsSlice",
            "type": "microsoft.insights/alertrules",
            "apiVersion": "2014-04-01",
            "location": "East US",
            "properties":
            {
                "name": "ADFAlertsSlice",
                "description": "One or more of the data slices for the Azure Data Factory has failed processing.",
                "isEnabled": true,
                "condition":
                {
                    "odata.type": "Microsoft.Azure.Management.Insights.Models.ManagementEventRuleCondition",
                    "dataSource":
                    {
                        "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleManagementEventDataSource",
                        "operationName": "RunFinished",
                        "status": "Failed",
                        "subStatus": "FailedExecution"   
                    }
                },
                "action":
                {
                    "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleEmailAction",
                    "customEmails": [ "<your alias>@contoso.com" ]
                }
            }
        }
    ]
}
```

Kaldırabileceğiniz **subStatus** belirli hata hakkında uyarı almak istemiyorsanız, JSON tanımından.

Bu örnek, aboneliğinizdeki tüm veri fabrikaları için uyarı ayarlar. Belirli veri fabrikası için ayarlanmış olması için uyarıyı istiyorsanız, veri fabrikası belirtebilirsiniz **resourceUri** içinde **dataSource**:

```JSON
"resourceUri" : "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.DATAFACTORY/DATAFACTORIES/<dataFactoryName>"
```

Aşağıdaki tabloda kullanılabilir işlemleri ve durumları (ve alt durumlar) listesini sağlar.

| İşlem adı | Durum | Alt durum |
| --- | --- | --- |
| RunStarted |Başlatıldı |Başlangıç |
| RunFinished |Başarısız / başarılı oldu |FailedResourceAllocation<br/><br/>Başarılı oldu<br/><br/>FailedExecution<br/><br/>Süresi sona erdi<br/><br/>< iptal edildi<br/><br/>FailedValidation<br/><br/>terk |
| OnDemandClusterCreateStarted |Başlatıldı | |
| OnDemandClusterCreateSuccessful |Başarılı oldu | |
| OnDemandClusterDeleted |Başarılı oldu | |

Bkz: [uyarı kuralı oluştur](https://msdn.microsoft.com/library/azure/dn510366.aspx) örnekte kullanılan JSON öğeleri hakkında ayrıntılı bilgi için.

#### <a name="deploy-the-alert"></a>Uyarı dağıtma
Uyarı dağıtmak için Azure PowerShell cmdlet'ini kullanın **New-AzureRmResourceGroupDeployment**, aşağıdaki örnekte gösterildiği gibi:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName adf -TemplateFile .\ADFAlertFailedSlice.json  
```

Kaynak grubuna dağıtım başarıyla tamamlandıktan sonra aşağıdaki iletiler görürsünüz:

```
VERBOSE: 7:00:48 PM - Template is valid.
WARNING: 7:00:48 PM - The StorageAccountName parameter is no longer used and will be removed in a future release.
Please update scripts to remove this parameter.
VERBOSE: 7:00:49 PM - Create template deployment 'ADFAlertFailedSlice'.
VERBOSE: 7:00:57 PM - Resource microsoft.insights/alertrules 'ADFAlertsSlice' provisioning status is succeeded

DeploymentName    : ADFAlertFailedSlice
ResourceGroupName : adf
ProvisioningState : Succeeded
Timestamp         : 10/11/2014 2:01:00 AM
Mode              : Incremental
TemplateLink      :
Parameters        :
Outputs           :
```

> [!NOTE]
> Kullanabileceğiniz [uyarı kuralı oluştur](https://msdn.microsoft.com/library/azure/dn510366.aspx) bir uyarı kuralı oluşturmak için REST API. JSON yükü JSON örnektekine benzer.  


#### <a name="retrieve-the-list-of-azure-resource-group-deployments"></a>Azure kaynak grubu dağıtımı listesini alma
Dağıtılan Azure kaynak grubu dağıtımı listesini almak için cmdlet'i kullanın **Get-AzureRmResourceGroupDeployment**, aşağıdaki örnekte gösterildiği gibi:

```powershell
Get-AzureRmResourceGroupDeployment -ResourceGroupName adf
```

```
DeploymentName    : ADFAlertFailedSlice
ResourceGroupName : adf
ProvisioningState : Succeeded
Timestamp         : 10/11/2014 2:01:00 AM
Mode              : Incremental
TemplateLink      :
Parameters        :
Outputs           :
```

#### <a name="troubleshoot-user-events"></a>Kullanıcı olaylarına sorun giderme
1. Tıkladıktan sonra oluşturulan tüm olayları görebilirsiniz **ölçümleri ve işlemleri** döşeme.

    ![Ölçümleri ve işlemleri bölmesi](./media/data-factory-monitor-manage-pipelines/metrics-and-operations-tile.png)
2. Tıklatın **olayları** döşeme olaylara bakın.

    ![Olaylar bölmesi](./media/data-factory-monitor-manage-pipelines/events-tile.png)
3. Üzerinde **olayları** dikey penceresinde, olaylar, filtrelenmiş olayları ve benzeri ayrıntılarını görebilirsiniz.

    ![Olaylar dikey penceresi](./media/data-factory-monitor-manage-pipelines/events-blade.png)
4. Tıklatın bir **işlemi** işlemleri listesinde bir hataya neden olur.

    ![Bir işlem seçin](./media/data-factory-monitor-manage-pipelines/select-operation.png)
5. Tıklatın bir **hata** hata ayrıntılarını görmek için olay.

    ![Olay hatası](./media/data-factory-monitor-manage-pipelines/operation-error-event.png)

Bkz: [Azure Insight cmdlet'leri](https://msdn.microsoft.com/library/mt282452.aspx) eklemek için kullanabileceğiniz PowerShell cmdlet'leri almak veya uyarıları kaldırın. İşte kullanmanın bazı örnekler **Get-AlertRule** cmdlet:

```powershell
get-alertrule -res $resourceGroup -n ADFAlertsSlice -det
```

```
Properties :
Action      : Microsoft.Azure.Management.Insights.Models.RuleEmailAction
Condition   :
DataSource :
EventName             :
Category              :
Level                 :
OperationName         : RunFinished
ResourceGroupName     :
ResourceProviderName  :
ResourceId            :
Status                : Failed
SubStatus             : FailedExecution
Claims                : Microsoft.Azure.Management.Insights.Models.RuleManagementEventClaimsDataSource
Condition      :
Description : One or more of the data slices for the Azure Data Factory has failed processing.
Status      : Enabled
Name:       : ADFAlertsSlice
Tags       :
$type          : Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage
Id: /subscriptions/<subscription ID>/resourceGroups/<resource group name>/providers/microsoft.insights/alertrules/ADFAlertsSlice
Location   : West US
Name       : ADFAlertsSlice
```

```powershell
Get-AlertRule -res $resourceGroup
```
```
Properties : Microsoft.Azure.Management.Insights.Models.Rule
Tags       : {[$type, Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage]}
Id         : /subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/microsoft.insights/alertrules/FailedExecutionRunsWest0
Location   : West US
Name       : FailedExecutionRunsWest0

Properties : Microsoft.Azure.Management.Insights.Models.Rule
Tags       : {[$type, Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage]}
Id         : /subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/microsoft.insights/alertrules/FailedExecutionRunsWest3
Location   : West US
Name       : FailedExecutionRunsWest3
```

```powershell
Get-AlertRule -res $resourceGroup -Name FailedExecutionRunsWest0
```

```
Properties : Microsoft.Azure.Management.Insights.Models.Rule
Tags       : {[$type, Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage]}
Id         : /subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/microsoft.insights/alertrules/FailedExecutionRunsWest0
Location   : West US
Name       : FailedExecutionRunsWest0
```

Ayrıntılar ve Get-AlertRule cmdlet örnekleri görmek için aşağıdaki get-help komutları çalıştırın.

```powershell
get-help Get-AlertRule -detailed
```

```powershell
get-help Get-AlertRule -examples
```


Portal dikey penceresinde uyarı oluşturma olaylarını görme ancak e-posta bildirimleri almadığınız, belirtilen e-posta adresi dış göndericilerden e-postaları için ayarlanmış olup olmadığını denetleyin. Uyarı e-postalar, e-posta ayarlarınız tarafından engellenmiş.

### <a name="alerts-on-metrics"></a>Ölçümleri uyarılar
Veri fabrikasında çeşitli ölçümleri yakalamak ve ölçümleri uyarılar oluşturabilir. İzleme ve uyarılar dilimleri için aşağıdaki ölçümleri, veri fabrikası oluşturun:

* **Başarısız çalıştırır**
* **Başarılı çalıştırır**

Bu ölçümler yararlıdır ve veri fabrikasında genel başarılı ve başarısız çalışır bir bakış elde size yardımcı. Olduğundan her zaman dilimi Çalıştır ölçümleri gösterilen. Saat başlangıcında, bu ölçümleri toplanır ve depolama hesabınıza gönderilir. Ölçümleri etkinleştirmek için bir depolama hesabı ayarlama.

#### <a name="enable-metrics"></a>Ölçümleri etkinleştir
Ölçümleri etkinleştirmek için aşağıdakiler arasından tıklatın **Data Factory** dikey penceresinde:

**İzleme** > **ölçüm** > **tanılama ayarlarını** > **tanılama**

![Tanılama bağlantı](./media/data-factory-monitor-manage-pipelines/diagnostics-link.png)

Üzerinde **tanılama** dikey penceresinde tıklatın **üzerinde**, depolama hesabını seçin ve'ı tıklatın **kaydetmek**.

![Tanılama dikey penceresi](./media/data-factory-monitor-manage-pipelines/diagnostics-blade.png)

Görünür olmasını ölçümler bir saat kadar sürebilir **izleme** dikey çünkü ölçümleri toplama saatlik olur.

### <a name="set-up-an-alert-on-metrics"></a>Ölçümler hakkında bir uyarı ayarlama
Tıklatın **Data Factory ölçümleri** döşeme:

![Veri Fabrikası ölçümleri kutucuğu](./media/data-factory-monitor-manage-pipelines/data-factory-metrics-tile.png)

Üzerinde **ölçüm** dikey penceresinde tıklatın **+ uyarı Ekle** araç çubuğunda.
![Data factory ölçüm dikey penceresi > Uyarı Ekle](./media/data-factory-monitor-manage-pipelines/add-alert.png)

Üzerinde **uyarı kuralı eklemek** sayfasında, aşağıdaki adımları uygulayın ve tıklayın **Tamam**.

* Uyarı için bir ad girin (örnek: "uyarı başarısız oldu").
* Uyarı için bir açıklama girin (örnek: "bir hata oluştuğunda bir e-posta Gönder").
* Bir ("Çalıştığında başarısız oldu" vs. ölçümünü seçin "Başarılı çalışır").
* Bir koşul ve bir eşik değeri belirtin.   
* Süreyi belirtin.
* Bir e-posta sahipleri, Katkıda Bulunanlar ve okuyucular gönderilmesi gerekip gerekmediğini belirtin.

![Data factory ölçüm dikey penceresi > Uyarı kuralı Ekle](./media/data-factory-monitor-manage-pipelines/add-an-alert-rule.png)

Uyarı kuralı başarıyla eklendi, dikey penceresi kapanır ve yeni uyarı gördüğünüz sonra **ölçüm** dikey.

![Data factory ölçüm dikey penceresi > eklenen yeni uyarı](./media/data-factory-monitor-manage-pipelines/failed-alert-in-metric-blade.png)

Uyarıları sayısını görmeniz gerekir **uyarı kuralları** döşeme. Tıklatın **uyarı kuralları** döşeme.

![Data factory ölçüm dikey penceresi - uyarı kuralları](./media/data-factory-monitor-manage-pipelines/alert-rules-tile-rules.png)

Üzerinde **uyarı kuralları** dikey penceresinde, var olan tüm uyarıları görebilir. Bir uyarı eklemek için tıklatın **uyarı Ekle** araç çubuğunda.

![Uyarı kuralları dikey penceresi](./media/data-factory-monitor-manage-pipelines/alert-rules-blade.png)

### <a name="alert-notifications"></a>Uyarı bildirimleri
Uyarı kuralı koşulu ile eşleşen sonra uyarı etkinleştirildikten bildiren bir e-posta almanız gerekir. Sorunun çözümlendiğini ve uyarı koşulu artık eşleşmiyor sonra uyarı çözümlendiğinde bildiren bir e-posta alırsınız.

Bu davranış için bir uyarı kuralı niteleyen her hatasında bir bildirim burada gönderilen olaylar farklıdır.

### <a name="deploy-alerts-by-using-powershell"></a>PowerShell kullanarak uyarıları dağıtma
Ölçümler için uyarı olayları için yaptığınız şekilde dağıtabilirsiniz.

**Uyarı tanımı**

```JSON
{
    "contentVersion" : "1.0.0.0",
    "$schema" : "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "parameters" : {},
    "resources" : [
    {
            "name" : "FailedRunsGreaterThan5",
            "type" : "microsoft.insights/alertrules",
            "apiVersion" : "2014-04-01",
            "location" : "East US",
            "properties" : {
                "name" : "FailedRunsGreaterThan5",
                "description" : "Failed Runs greater than 5",
                "isEnabled" : true,
                "condition" : {
                    "$type" : "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.ThresholdRuleCondition, Microsoft.WindowsAzure.Management.Mon.Client",
                    "odata.type" : "Microsoft.Azure.Management.Insights.Models.ThresholdRuleCondition",
                    "dataSource" : {
                        "$type" : "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.RuleMetricDataSource, Microsoft.WindowsAzure.Management.Mon.Client",
                        "odata.type" : "Microsoft.Azure.Management.Insights.Models.RuleMetricDataSource",
                        "resourceUri" : "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<resourceGroupName
>/PROVIDERS/MICROSOFT.DATAFACTORY/DATAFACTORIES/<dataFactoryName>",
                        "metricName" : "FailedRuns"
                    },
                    "threshold" : 5.0,
                    "windowSize" : "PT3H",
                    "timeAggregation" : "Total"
                },
                "action" : {
                    "$type" : "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.RuleEmailAction, Microsoft.WindowsAzure.Management.Mon.Client",
                    "odata.type" : "Microsoft.Azure.Management.Insights.Models.RuleEmailAction",
                    "customEmails" : ["abhinav.gpt@live.com"]
                }
            }
        }
    ]
}
```

Değiştir *Subscriptionıd*, *resourceGroupName*, ve *dataFactoryName* uygun değerlerle örnekteki.

*metricName* şu anda iki değer destekler:

* FailedRuns
* SuccessfulRuns

**Uyarı dağıtma**

Uyarı dağıtmak için Azure PowerShell cmdlet'ini kullanın **New-AzureRmResourceGroupDeployment**, aşağıdaki örnekte gösterildiği gibi:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName adf -TemplateFile .\FailedRunsGreaterThan5.json
```

Başarılı dağıtım sonrasında iletiden görmeniz gerekir:

```
VERBOSE: 12:52:47 PM - Template is valid.
VERBOSE: 12:52:48 PM - Create template deployment 'FailedRunsGreaterThan5'.
VERBOSE: 12:52:55 PM - Resource microsoft.insights/alertrules 'FailedRunsGreaterThan5' provisioning status is succeeded


DeploymentName    : FailedRunsGreaterThan5
ResourceGroupName : adf
ProvisioningState : Succeeded
Timestamp         : 7/27/2015 7:52:56 PM
Mode              : Incremental
TemplateLink      :
Parameters        :
Outputs           
```

Aynı zamanda **Ekle AlertRule** bir uyarı kuralı dağıtmak için cmdlet. Bkz: [Ekle AlertRule](https://msdn.microsoft.com/library/mt282468.aspx) ayrıntı ve örnekler için konu.  

## <a name="move-a-data-factory-to-a-different-resource-group-or-subscription"></a>Veri Fabrikası farklı bir kaynak grubuna veya aboneliğe taşıma
Kullanarak farklı bir kaynak grubunda veya farklı bir abonelik için bir veri fabrikası taşıyabilirsiniz **taşıma** veri fabrikanızın giriş sayfasında düğme çubuğu komutu.

![Veri Fabrikası taşıma](./media/data-factory-monitor-manage-pipelines/MoveDataFactory.png)

Ayrıca, ilgili kaynakları (örneğin, data factory ile ilişkili olan uyarılar), veri fabrikası birlikte taşıyabilirsiniz.

![Taşıma kaynaklar iletişim kutusu](./media/data-factory-monitor-manage-pipelines/MoveResources.png)
