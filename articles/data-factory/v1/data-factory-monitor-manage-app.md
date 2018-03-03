---
title: "İzleme ve veri ardışık - Azure yönetme | Microsoft Docs"
description: "Azure data factory'leri ve ardışık düzen yönetmek ve izlemek için izleme ve yönetim uygulaması kullanmayı öğrenin."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: f3f07bc4-6dc3-4d4d-ac22-0be62189d578
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: 4d4371b1372a7ed492faacf16813ae3e3f4c4697
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="monitor-and-manage-azure-data-factory-pipelines-by-using-the-monitoring-and-management-app"></a>İzleme ve Azure Data Factory işlem hatlarını izleme ve yönetim uygulaması kullanarak yönetme
> [!div class="op_single_selector"]
> * [Azure portal/Azure PowerShell kullanma](data-factory-monitor-manage-pipelines.md)
> * [Kullanılarak izleme ve yönetim uygulaması](data-factory-monitor-manage-app.md)
>
>

> [!NOTE]
> Bu makale, Data Factory’nin genel kullanıma açık olan (GA) 1. sürümü için geçerlidir. Önizlemede değil, Data Factory hizmetinin 2 sürümünü kullanıyorsanız bkz [izlemek ve 2 sürümündeki Data Factory işlem hatlarını yönetmek](../monitor-visually.md).

Bu makalede izleme ve yönetim uygulama izleme, yönetme ve Data Factory işlem hatlarınızı hata ayıklamak için nasıl kullanılacağını açıklar. Ayrıca hatalarında bildirim almak için uyarı oluşturma hakkında bilgi sağlar. Aşağıdaki videoyu izleyerek uygulamayı kullanmaya başlayabilirsiniz:

> [!NOTE]
> Portalı'nda bkz videoda gösterilen kullanıcı arabirimi tam olarak eşleşmiyor olabilir. Biraz daha eski olduğu, ancak kavramları aynı kalır. 

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Azure-Data-Factory-Monitoring-and-Managing-Big-Data-Piplines/player]
>

## <a name="launch-the-monitoring-and-management-app"></a>İzleme ve yönetim uygulama başlatma
İzleme ve yönetim uygulaması başlatmak için tıklatın **İzleyici & Yönet** döşemesinin **Data Factory** veri fabrikanızın dikey.

![Data Factory giriş sayfasında izleme kutucuğu](./media/data-factory-monitor-manage-app/MonitoringAppTile.png)

Ayrı bir pencerede açmak izleme ve yönetim uygulaması görmeniz gerekir.  

![İzleme ve Yönetim uygulaması](./media/data-factory-monitor-manage-app/AppLaunched.png)

> [!NOTE]
> Web tarayıcısının "Yetkilendiriliyor..." durumunda takıldığını görürseniz, temizleyin **üçüncü taraf tanımlama bilgilerini engelleyebilir ve site verileri** onay kutusu--veya seçili, Canlı oluşturmak için bir özel durum **login.microsoftonline.com**, ve ardından uygulamayı açmayı yeniden deneyin.


Orta bölmede etkinlik Windows listesinde, her bir etkinlik çalışması için bir etkinlik penceresini görürsünüz. Örneğin, beş saatlik saatte bir çalışacak şekilde zamanlanmış etkinlik varsa, beş veri dilimi ile ilişkili beş etkinlik windows bakın. Etkinlik pencerelerine alttaki listede görmüyorsanız, aşağıdakileri yapın:
 
- Güncelleştirme **başlangıç zamanı** ve **bitiş saati** filtreleri hattınızı başlangıç ve bitiş zamanları eşleşen ve ardından üst **Uygula** düğmesi.  
- Etkinlik Windows listesi otomatik olarak yenilenmez. Tıklatın **yenileme** araç çubuğunda **etkinlik Windows** listesi.  

Bu adımları ile test etmek için bir veri fabrikası uygulaması yoksa, öğreticiyi yapın: [veri kopyalama Blob depolama alanından SQL Data Factory kullanarak veritabanına](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

## <a name="understand-the-monitoring-and-management-app"></a>İzleme ve yönetim uygulaması anlama
Soldaki bölmede üç sekme vardır: **kaynak Gezgini**, **izleme görünümleri**, ve **uyarıları**. İlk sekme (**kaynak Gezgini**) varsayılan olarak seçilidir.

### <a name="resource-explorer"></a>Kaynak Gezgini
Aşağıdakilere bakın:

* Kaynak Gezgini **ağaç görünümü** sol bölmede.
* **Diyagram görünümü** Orta bölmede üstünde.
* **Etkinlik Windows** Orta bölmede altındaki listesi.
* **Özellikleri**, **etkinlik penceresini Explorer**, ve **betik** sağ bölmedeki sekmeleri.

Kaynak Gezgini, ağaç görünümünde veri fabrikasında tüm kaynakları (komut zincirleri, veri kümeleri, bağlı hizmetler) bakın. Ne zaman kaynak Gezgini, bir nesne seçin:

* İlişkili veri fabrikası Varlık diyagram Görünümü'nde vurgulanır.
* [Etkinlik pencerelerine ilişkili](data-factory-scheduling-and-execution.md) altındaki etkinlik Windows listesinde vurgulanır.  
* Sağ bölmede Özellikleri penceresinde seçili nesnenin özelliklerini gösterilmektedir.
* Seçilen nesneyi JSON tanımını gösterilen varsa. Örneğin: bağlı hizmet, bir veri kümesi veya işlem hattı.

![Kaynak Gezgini](./media/data-factory-monitor-manage-app/ResourceExplorer.png)

Bkz: [zamanlama ve yürütme](data-factory-scheduling-and-execution.md) etkinlik windows hakkında ayrıntılı kavramsal bilgi için makalenin.

### <a name="diagram-view"></a>Diyagram Görünümü
Veri Fabrikası diyagram görünümünü bölmeden bir veri fabrikası ve varlıklarını yönetmek ve izlemek için cam sağlar. Diyagram Görünümü'nde bir Data Factory varlığı (dataset/ardışık düzeni) seçtiğinizde:

* Veri Fabrikası varlık ağaç görünümünde seçilir.
* İlişkili etkinlik windows etkinlik Windows listesinde vurgulanır.
* Özellikler penceresinde seçili nesnenin özelliklerini gösterilmektedir.

Ardışık Düzen (duraklatılmış durumda değil) etkin olduğunda, yeşil bir çizgiyle gösterilir:

![Çalışan işlem hattı](./media/data-factory-monitor-manage-app/PipelineRunning.png)

Duraklatma, sürdürme veya diyagram görünümünde seçerek ve komut çubuğunda düğmeleri kullanarak bir ardışık düzen sonlandırmak.

![Komut çubuğunda Duraklat/Sürdür](./media/data-factory-monitor-manage-app/SuspendResumeOnCommandBar.png)
 
Diyagram görünümünde ardışık düzeni için üç komut çubuğu düğmelerini vardır. Ardışık Düzen duraklatmak için ikinci düğmesini kullanabilirsiniz. Duraklatma çalışmakta etkinlikleri sonlandırmak değil ve bunları tamamlanıncaya kadar devam olanak tanır. Üçüncü düğme ardışık duraklatır ve onun etkinlikleri yürütme varolan sona erer. İlk düğme ardışık sürdürür. İşlem hattınızı duraklatıldığında, ardışık düzen rengini değiştirir. Örneğin, duraklatılmış bir ardışık düzen aşağıdaki görüntüde şuna benzer: 

![Ardışık Düzen duraklatıldı](./media/data-factory-monitor-manage-app/PipelinePaused.png)

Ctrl tuşunu kullanarak çoklu seçim iki veya daha fazla ardışık düzen olabilir. Komut çubuğu düğmelerini Duraklat/birden çok ardışık düzen aynı anda Sürdür için kullanabilirsiniz.

Ayrıca, bir işlem hattı sağ tıklatın ve askıya alma, sürdürme veya bir işlem hattı sonlandırmak için seçenekleri seçin. 

![Ardışık düzeni için bağlam menüsü](./media/data-factory-monitor-manage-app/right-click-menu-for-pipeline.png)

Tıklatın **açık işlem hattı** işlem hattındaki tüm etkinlikleri görmek için seçeneği. 

![İşlem hattı menüsünü açma](./media/data-factory-monitor-manage-app/OpenPipelineMenu.png)

Açılan ardışık düzen Görünümü'nde, işlem hattındaki tüm etkinlikleri görürsünüz. Bu örnekte, yalnızca bir etkinlik yok: kopyalama etkinliği. 

![Açılan ardışık düzen](./media/data-factory-monitor-manage-app/OpenedPipeline.png)

Önceki görünüme geri dönmek için veri fabrikası adı üstteki içerik haritası menüsünde tıklatın.

Bir çıkış veri kümesini veya çıkış veri kümesinde farenizi taşıdığınızda seçtiğinizde ardışık düzen görünümünde, bu veri kümesi için etkinlik Windows açılır pencere görürsünüz.

![Etkinlik Windows açılır penceresi](./media/data-factory-monitor-manage-app/ActivityWindowsPopup.png)

İçinde için ayrıntıları görmek için bir etkinlik penceresini tıklayabilirsiniz **özellikleri** penceresinin sağ bölmesinde.

![Etkinlik penceresini özellikleri](./media/data-factory-monitor-manage-app/ActivityWindowProperties.png)

Sağ bölmede geçmek **etkinlik penceresini Explorer** daha fazla ayrıntı için sekmesi.

![Etkinlik penceresini Gezgini](./media/data-factory-monitor-manage-app/ActivityWindowExplorer.png)

Ayrıca bkz: **değişkenleri çözülmüş** bir etkinliğin her çalıştırma denemesi için **denemeleri** bölümü.

![Çözümlenen değişkenleri](./media/data-factory-monitor-manage-app/ResolvedVariables.PNG)

Geçiş **betik** sekmesi seçili nesne için JSON betik tanımına bakın.   

![Komut dosyası sekmesi](./media/data-factory-monitor-manage-app/ScriptTab.png)

Etkinlik pencerelerine üç yerde görebilirsiniz:

* Diyagram görünümünde (Orta bölme) etkinlik Windows açılır.
* Sağ bölmede etkinlik penceresini Gezgini.
* Alt bölme etkinlik Windows listesinde.

Etkinlik pencerelerine açılır ve etkinlik penceresini Gezgini sol ve sağ ok kullanarak önceki hafta ve sonraki hafta gezinebilirsiniz.

![Etkinlik penceresini Explorer sol/sağ ok](./media/data-factory-monitor-manage-app/ActivityWindowExplorerLeftRightArrows.png)

Diyagram görünümü en altında bu düğmeleri Bkz: Yakınlaştır, uzaklaştırma, yakınlaştırma sığacak şekilde, yakınlaştırma % 100, kilit düzeni. **Kilit düzeni** düğmesi önler yanlışlıkla tablolar ve ardışık düzen diyagram Görünümü'nde taşınmasını. Bu varsayılan olarak açıktır. Uygulamayı kapatın ve varlıkları diyagramında hareket ettirin. Devre dışı bıraktığınızda, tablolar ve ardışık düzen otomatik Konumlandır için Son düğmesini kullanabilirsiniz. Fare tekerleği kullanarak içeri veya dışarı ayrıca yakınlaştırabilirsiniz.

![Diyagram görünümü yakınlaştırma komutları](./media/data-factory-monitor-manage-app/DiagramViewZoomCommands.png)

### <a name="activity-windows-list"></a>Etkinlik Pencereleri listesi
Etkinlik Windows orta bölmesinde altındaki tüm etkinlik windows kaynak Gezgini ya da diyagram görünümünde seçilen veri kümesi için görüntüler. Varsayılan olarak, en son etkinlik pencerenin en üstünde gördüğünüz anlamına gelir, azalan sırada listesidir.

![Etkinlik Pencereleri listesi](./media/data-factory-monitor-manage-app/ActivityWindowsList.png)

Bu liste otomatik yenileme değil, bu nedenle el ile yenilemek için araç çubuğunda Yenile düğmesini kullanın.  

Etkinlik pencerelerine aşağıdaki durumlardan biri olabilir:

<table>
<tr>
    <th align="left">Durum</th><th align="left">Alt durum</th><th align="left">Açıklama</th>
</tr>
<tr>
    <td rowspan="8">Bekleniyor</td><td>ScheduleTime</td><td>Zaman çalıştırmak ilişkin etkinlik penceresinin gelen kurmadı.</td>
</tr>
<tr>
<td>DatasetDependencies</td><td>Yukarı Akış bağımlılıkları hazır değil.</td>
</tr>
<tr>
<td>ComputeResources</td><td>İşlem kaynakları kullanılabilir değil.</td>
</tr>
<tr>
<td>ConcurrencyLimit</td> <td>Tüm etkinlik örnekleri diğer etkinlik windows çalıştıran meşgul.</td>
</tr>
<tr>
<td>ActivityResume</td><td>Etkinlik duraklatıldı ve sürdürülene kadar etkinlik windows çalıştıramaz.</td>
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
<td>Etkinlik penceresinin işleniyor.</td>
</tr>
<tr>
<td rowspan="4">Başarısız</td><td>Süresi sona erdi</td><td>Etkinlik yürütme etkinlik tarafından izin daha uzun sürdü.</td>
</tr>
<tr>
<td>İptal edildi</td><td>Etkinlik penceresinin kullanıcı eylemi tarafından iptal edildi.</td>
</tr>
<tr>
<td>Doğrulama</td><td>Doğrulama başarısız oldu.</td>
</tr>
<tr>
<td>-</td><td>Etkinlik penceresinin oluşturulan ya da doğrulanması başarısız oldu.</td>
</tr>
<td>Hazır</td><td>-</td><td>Etkinlik penceresinin tüketimi için hazırdır.</td>
</tr>
<tr>
<td>Atlandı</td><td>-</td><td>Etkinlik penceresinin işlenen değildi.</td>
</tr>
<tr>
<td>Hiçbiri</td><td>-</td><td>Bir etkinlik penceresini farklı bir durum ile var olmuş ancak sıfırlanmış.</td>
</tr>
</table>


Listenin etkinliği penceresindeki tıklattığınızda içinde ayrıntılarını görmek **etkinlik Windows Explorer** veya **özellikleri** penceresini sağdaki.

![Etkinlik penceresini Gezgini](./media/data-factory-monitor-manage-app/ActivityWindowExplorer-2.png)

### <a name="refresh-activity-windows"></a>Etkinlik pencerelerine Yenile
Ayrıntıları otomatik olarak yenilenir değil, bu nedenle etkinlik windows listesini el ile yenilemek için komut çubuğunda (ikinci düğme) Yenile düğmesini kullanın.  

### <a name="properties-window"></a>Özellik penceresi
Özellikler penceresini izleme ve yönetim uygulaması en sağ bölmesinde bulunur.

![Özellik penceresi](./media/data-factory-monitor-manage-app/PropertiesWindow.png)

Kaynak Gezgini (ağaç görünümü), diyagram görünümü veya etkinlik Windows liste seçili öğenin özelliklerini görüntüler.

### <a name="activity-window-explorer"></a>Etkinlik penceresini Gezgini
**Etkinlik penceresini Explorer** izleme ve yönetim uygulaması en sağdaki bölmede bir penceredir. Etkinlik Windows açılır penceresi veya etkinlik Windows listedeki seçili etkinlik penceresinin hakkındaki ayrıntıları görüntüler.

![Etkinlik penceresini Gezgini](./media/data-factory-monitor-manage-app/ActivityWindowExplorer-3.png)

Üst Takvim görünümünde tıklayarak başka bir etkinlik penceresine geçiş yapabilirsiniz. Etkinlik Windows'dan önceki haftanın ya da sonraki hafta görmek için sayfanın üstünde sol ok/sağ ok düğmelerini de kullanabilirsiniz.

Etkinlik penceresinin yeniden çalıştırın veya ayrıntılar bölmesinde yenilemek için alt bölmesinde araç çubuğu düğmelerini kullanın.

### <a name="script"></a>Betik
Kullanabileceğiniz **betik** seçili Data Factory varlığı (bağlantılı hizmet, veri kümesi veya işlem hattı) JSON tanımını görüntülemek için.

![Komut dosyası sekmesi](./media/data-factory-monitor-manage-app/ScriptTab.png)

## <a name="use-system-views"></a>Sistem görünümleri kullanma
İzleme ve yönetim uygulaması önceden derlenmiş sistem görünümleri içerir (**son etkinlik windows**, **başarısız etkinliği windows**, **ilerleme durumu etkinlik windows**), izin ver en son/başarısız/Süren etkinlik windows veri fabrikanızın görüntülemek için.

Geçiş **izleme görünümleri** sekmesini tıklatarak soldaki.

![İzleme görünümleri sekmesi](./media/data-factory-monitor-manage-app/MonitoringViewsTab.png)

Şu anda desteklenen üç sistem görünümleri vardır. Son Etkinlik windows, başarısız etkinliği windows veya devam eden etkinlik windows etkinlik Windows listesinde (orta bölmesinde altındaki) görmek için bir seçenek belirleyin.

Seçtiğinizde, **son etkinlik windows** seçeneği, azalan sırada olan tüm son etkinlik windows görmek **zaman'son deneme**.

Kullanabileceğiniz **başarısız etkinliği windows** listesindeki tüm başarısız etkinliği windows görmek için görünümü. İçinde ayrıntılarını görmek için listedeki bir başarısız etkinliği penceresi seçin **özellikleri** penceresi veya **etkinlik penceresini Explorer**. Ayrıca, başarısız olan etkinliği penceresi için herhangi bir günlük indirebilirsiniz.

## <a name="sort-and-filter-activity-windows"></a>Sıralama ve filtreleme etkinliği windows
Değişiklik **başlangıç zamanı** ve **bitiş saati** filtre etkinlik Windows Komut çubuğunda ayarlar. Başlangıç ve bitiş zamanı değiştirdikten sonra etkinlik Windows listesini yenilemek için bitiş zamanı yanındaki düğmesini tıklatın.

![Başlangıç ve bitiş zamanlarını](./media/data-factory-monitor-manage-app/StartAndEndTimes.png)

> [!NOTE]
> Şu anda, izleme ve yönetim uygulaması UTC biçiminde her zaman uygun.
>
>

İçinde **etkinlik Windows listesi**, bir sütun adına tıklayın (örneğin: durum).

![Etkinlik Windows liste sütun menüsü](./media/data-factory-monitor-manage-app/ActivityWindowsListColumnMenu.png)

Aşağıdakileri yapabilirsiniz:

* Artan sırada Sırala.
* Azalan sırada sıralayın.
* Bir veya daha fazla değerlerle (hazır, bekliyor vb.) filtreleyin.

Bir sütunda bir filtre belirttiğinizde, filtrelenmiş değerleri sütundaki değerleri gösterir bu sütun için etkin filtre düğmesini bakın.

![Etkinlik pencerelerine listesinin bir sütuna filtre](./media/data-factory-monitor-manage-app/ActivityWindowsListFilterInColumn.png)

Aynı açılır pencere Filtreleri temizlemek için kullanabilirsiniz. Etkinlik pencerelerine listesi için tüm filtreleri temizlemek için komut çubuğunda Filtreyi düğmesini tıklatın.

![Etkinlik pencerelerine listesi için tüm filtreleri temizlemek](./media/data-factory-monitor-manage-app/ClearAllFiltersActivityWindowsList.png)

## <a name="perform-batch-actions"></a>Toplu işlemleri
### <a name="rerun-selected-activity-windows"></a>Seçili etkinlik windows yeniden çalıştırın
Bir etkinlik penceresi seçin, ilk komut çubuğu düğmesi için aşağı oka tıklayın ve seçin **yeniden Çalıştır** / **ile upstream ardışık düzeninde yeniden**. Seçtiğinizde, **ile upstream ardışık düzeninde yeniden** seçeneği, tüm Yukarı Akış etkinliği windows de yeniden çalıştırır.
    ![Bir etkinlik penceresi yeniden](./media/data-factory-monitor-manage-app/ReRunSlice.png)

Ayrıca, birden çok etkinlik windows listeden seçin ve bunları aynı anda yeniden çalıştırın. Etkinlik windows durumlarına göre filtre uygulamak istediğiniz (örneğin: **başarısız**)--ve başarısız olan etkinliği windows etkinlik windows başarısız olmasına neden olan sorunu düzelttikten sonra yeniden çalıştırın. Etkinlik pencerelerine listesinde filtreleme hakkında ayrıntılar için aşağıdaki bölüme bakın.  

### <a name="pauseresume-multiple-pipelines"></a>Birden çok ardışık düzen Duraklat/Sürdür
Ctrl tuşunu kullanarak çoklu iki veya daha fazla ardışık düzen olabilir. Bunları Duraklat/Sürdür için (hangi aşağıdaki görüntüde kırmızı dikdörtgen içinde vurgulanır) komut çubuğu düğmelerini kullanabilirsiniz.

![Komut çubuğunda Duraklat/Sürdür](./media/data-factory-monitor-manage-app/SuspendResumeOnCommandBar.png)

## <a name="create-alerts"></a>Uyarı oluşturma
**Uyarıları** sayfası, bir uyarı ve görünüm/düzenleme/silme mevcut uyarıları oluşturmanıza olanak sağlar. Sizin de devre dışı bırak/uyarı etkinleştirebilirsiniz. Uyarılar sayfasında görmek için tıklatın **uyarıları** sekmesi.

![Uyarılar sekmesi](./media/data-factory-monitor-manage-app/AlertsTab.png)

### <a name="to-create-an-alert"></a>Bir uyarı oluşturmak için
1. Tıklatın **eklemek uyarı** bir uyarı eklemek için. Gördüğünüz **ayrıntıları** sayfası.

    ![Uyarılar - ayrıntılar sayfası oluşturma](./media/data-factory-monitor-manage-app/CreateAlertDetailsPage.png)
2. Belirtin **adı** ve **açıklama** uyarı ve tıklatın **sonraki**. Görmeniz gerekir **filtreleri** sayfası.

    ![Uyarılar - filtreleri sayfa oluşturma](./media/data-factory-monitor-manage-app/CreateAlertFiltersPage.png)
3. Seçin **olay**, **durum**, ve **substatus** (isteğe bağlı) bir Data Factory hizmeti uyarı oluşturup tıklatın istediğiniz **sonraki**. Görmeniz gerekir **alıcılar** sayfası.

    ![Uyarılar - alıcılar sayfa oluşturma](./media/data-factory-monitor-manage-app/CreateAlertRecipientsPage.png)
4. Seçin **abonelik yöneticileri e-posta** seçeneği ve/veya girin bir **ek yönetici e-posta**, tıklatıp **son**. Uyarı listesinde görmeniz gerekir.

    ![Uyarılar listesinde](./media/data-factory-monitor-manage-app/AlertsList.png)

Uyarılar listesinde düzenleme/silme/devre dışı bırak/etkinleştir bir uyarı uyarı ile ilişkili düğmelerini kullanın.

### <a name="eventstatussubstatus"></a>Alt olay/durumu/durum
Aşağıdaki tabloda kullanılabilir olayları ve durumları (ve alt durumlar) listesini sağlar.

| Olay adı | Durum | Alt durum |
| --- | --- | --- |
| Başlatılan Çalıştır etkinliği |Başlatıldı |Başlatılıyor |
| Tamamlanan etkinlik |Başarılı oldu |Başarılı oldu |
| Tamamlanan etkinlik |Başarısız |Başarısız olan kaynak ayırma<br/><br/>Başarısız yürütme<br/><br/>Zaman Aşımı<br/><br/>Başarısız doğrulamayı<br/><br/>Abandoned |
| İsteğe bağlı HDI kümesi oluşturma başlatıldı |Başlatıldı |-|
| İsteğe bağlı HDI kümesi başarıyla oluşturuldu |Başarılı oldu |-|
| İsteğe bağlı HDI kümesi silindi |Başarılı oldu |-|

### <a name="to-edit-delete-or-disable-an-alert"></a>Düzenleme, silme veya bir uyarıyı devre dışı bırakmak için

Düzenleme, silme veya devre dışı bir uyarı (kırmızı ile vurgulanan) aşağıdaki düğmelerini kullanın.

![Uyarıları düğmeleri](./media/data-factory-monitor-manage-app/AlertButtons.png)
