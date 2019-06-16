---
title: İzleme ve yönetme veri komut zincirlerini - Azure | Microsoft Docs
description: Azure veri fabrikası ve işlem hatlarını yönetmek ve izlemek için izleme ve yönetim uygulaması kullanmayı öğrenin.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.assetid: f3f07bc4-6dc3-4d4d-ac22-0be62189d578
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: 5b70edd4f65538b52c70881258bc500a34b04d80
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60826756"
---
# <a name="monitor-and-manage-azure-data-factory-pipelines-by-using-the-monitoring-and-management-app"></a>İzleme ve Azure Data Factory işlem hatlarını izleme ve yönetim uygulaması kullanarak yönetme
> [!div class="op_single_selector"]
> * [Azure portal/Azure PowerShell kullanma](data-factory-monitor-manage-pipelines.md)
> * [Kullanarak izleme ve yönetim uygulaması](data-factory-monitor-manage-app.md)
>
>

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [izlemek ve yönetmek, Data Factory işlem hatlarını](../monitor-visually.md).

Bu makalede izleme ve yönetim uygulaması izleme, yönetme ve Data Factory işlem hatlarınızı hata ayıklamak için nasıl kullanılacağını açıklar. Uygulama aşağıdaki videoyu izleyerek kullanmaya başlayabilirsiniz:

> [!NOTE]
> Portalda görmek videoda gösterilen kullanıcı arabirimi tam olarak eşleşmiyor olabilir. Biraz daha eski, ancak aynı kavramlar kalır. 

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Azure-Data-Factory-Monitoring-and-Managing-Big-Data-Piplines/player]
>

## <a name="launch-the-monitoring-and-management-app"></a>İzleme ve Yönetimi uygulamasını başlatma
İzleme ve yönetim uygulaması başlatmak için tıklayın **izleme ve yönetme** kutucuğundan **Data Factory** veri fabrikanızın dikey.

![Data Factory giriş sayfasında kutucuk izleme](./media/data-factory-monitor-manage-app/MonitoringAppTile.png)

Ayrı bir pencerede açmak izleme ve yönetim uygulaması görmeniz gerekir.  

![İzleme ve Yönetim uygulaması](./media/data-factory-monitor-manage-app/AppLaunched.png)

> [!NOTE]
> Web tarayıcısının "Yetkilendiriliyor..." durumunda takıldığını görürseniz Temizle **ve site verilerini üçüncü taraf tanımlama bilgilerini engelle** onay kutusu--veya seçili, Canlı oluşturmak için bir özel durum **login.microsoftonline.com**, ve sonra uygulamayı yeniden başlatmayı deneyin.


Orta bölmede etkinlik Windows listesinde bir etkinlik penceresi için bir etkinlik çalıştırmalarını görürsünüz. Örneğin, beş saate saatte bir çalışacak şekilde zamanlanan bir etkinlik varsa, beş veri dilimi ile ilişkili beş etkinlik pencereleri bakın. Etkinlik pencereleri alttaki listede görmüyorsanız, aşağıdakileri yapın:
 
- Güncelleştirme **başlangıç saati** ve **bitiş saati** filtreleri değerlerini işlem hattınızın başlangıç ve bitiş zamanlarını eşleşen ve ardından üstteki **Uygula** düğmesi.  
- Etkinlik Windows listesi otomatik olarak yenilenmez. Tıklayın **Yenile** araç çubuğunda düğme **etkinlik Windows** listesi.  

Bu adımları test etmek için bir Data Factory uygulamanız yoksa, öğreticiyi uygulamak: [Blob depolamadan SQL veritabanına veri kopyalama kullanarak veri fabrikası için](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

## <a name="understand-the-monitoring-and-management-app"></a>İzleme ve yönetim uygulaması anlama
Soldaki üç sekme bulunur: **Kaynak Gezgini**, **izleme görünümleriyle**, ve **uyarılar**. İlk sekme (**kaynak Gezgini**) varsayılan olarak seçilidir.

### <a name="resource-explorer"></a>Kaynak Gezgini
Aşağıdakilere bakın:

* Kaynak Gezgini **ağaç görünümü** sol bölmesinde.
* **Diyagram görünümü** ortadaki bölmesinde üst.
* **Etkinlik Windows** Orta bölmede alttaki liste.
* **Özellikleri**, **etkinlik penceresi Gezgini**, ve **betik** sağ bölmede sekme.

Kaynak Gezgini'nde bir ağaç görünümünde data factory'de tüm kaynakları (işlem hatları, veri kümeleri, bağlı hizmetler) bakın. Zaman içinde kaynak Gezgini, bir nesne seçin:

* İlişkili bir Data Factory varlığının diyagram Görünümü'nde vurgulanır.
* [Etkinlik pencereleri ilişkili](data-factory-scheduling-and-execution.md) etkinlik Windows listenin altındaki vurgulanır.  
* Özellikler penceresinde sağ bölmede seçili nesnenin özellikleri gösterilmektedir.
* JSON tanımı seçili nesnenin gösterilen varsa. Örneğin: bağlı hizmet, bir veri kümesi veya işlem hattı.

![Kaynak Gezgini](./media/data-factory-monitor-manage-app/ResourceExplorer.png)

Bkz: [zamanlama ve yürütme](data-factory-scheduling-and-execution.md) etkinlik pencereleri hakkında ayrıntılı kavramsal bilgi makalesi.

### <a name="diagram-view"></a>Diyagram Görünümü
Veri fabrikasının diyagram görünümünü, veri fabrikası ve varlıklarını yönetmek ve izlemek için tek bir panel sağlar. Diyagram Görünümü'nde bir (veri kümesi/işlem hattı) Data Factory varlığı seçtiğinizde:

* Data factory varlığı, ağaç görünümünde seçilir.
* İlgili etkinlik pencerelerini etkinlik Windows listesinde vurgulanır.
* Seçili nesnenin özelliklerini Özellikler penceresinde gösterilir.

İşlem hattı (duraklatılmış durumda değil) etkin olduğunda, yeşil bir çizgiyle gösterilmektedir:

![İşlem hattı çalıştırma](./media/data-factory-monitor-manage-app/PipelineRunning.png)

Duraklatma, sürdürme veya diyagram Görünümü'nde seçerek ve Komut çubuğundaki düğmeleri kullanarak bir işlem hattı sonlandırın.

![Komut çubuğunda duraklatın/sürdürün](./media/data-factory-monitor-manage-app/SuspendResumeOnCommandBar.png)
 
Diyagram görünümünde işlem hattının üç komut çubuğu düğme vardır. İşlem hattı duraklatmak için ikinci bir düğme kullanabilirsiniz. Duraklatma şu anda çalışan etkinlikler işlemi sonlandırmaz ve bunları tamamlanana kadar devam olanak tanır. Alan üçüncü düğmeye, işlem hattı duraklatır ve mevcut etkinlikler yürütme sona erer. İlk düğmeyi, işlem hattı sürdürür. İşlem hattınızı duraklatıldığında, işlem hattı rengini değiştirir. Örneğin, duraklatılmış bir işlem hattı aşağıdaki görüntüde şuna benzer: 

![İşlem hattı duraklatıldı](./media/data-factory-monitor-manage-app/PipelinePaused.png)

Ctrl tuşunu kullanarak çoklu seçim iki veya daha fazla işlem hattı kullanabilirsiniz. Birden fazla işlem hattını aynı anda duraklatın/sürdürün için komut çubuğu düğmelerini kullanabilirsiniz.

Ayrıca, bir işlem hattı sağ tıklayın ve askıya alma, sürdürme veya bir işlem hattı sonlandırmak için seçenekleri belirleyin. 

![İşlem hattı için bağlam menüsü](./media/data-factory-monitor-manage-app/right-click-menu-for-pipeline.png)

Tıklayın **ardışık düzeni Aç** işlem hattındaki tüm etkinlikleri görmek için Seçenekler. 

![İşlem hattı menüsünü açma](./media/data-factory-monitor-manage-app/OpenPipelineMenu.png)

Açık işlem hattı Görünümü'nde işlem hattındaki tüm etkinlikleri bakın. Bu örnekte, yalnızca bir etkinlik vardır: Kopyalama etkinliği. 

![Açık işlem hattı](./media/data-factory-monitor-manage-app/OpenedPipeline.png)

Önceki görünüme dönmek için üstteki içerik haritası menüsünde veri fabrikasının adını tıklayın.

İşlem hattı Görünümü'nde bir çıktı veri kümesini veya çıkış veri kümesi üzerinde fare taşıdığınızda seçtiğinizde söz konusu veri kümesi için etkinlik Windows açılır pencere görürsünüz.

![Etkinlik Windows açılır penceresi](./media/data-factory-monitor-manage-app/ActivityWindowsPopup.png)

İçinde ayrıntıları görmek için bir etkinlik penceresi tıklayabilirsiniz **özellikleri** penceresinin sağ bölmesinde.

![Etkinlik penceresinin Özellikler](./media/data-factory-monitor-manage-app/ActivityWindowProperties.png)

Sağ bölmede, geçiş **etkinlik penceresi Gezgini** daha fazla ayrıntı için sekmesinde.

![Etkinlik penceresi Gezgini](./media/data-factory-monitor-manage-app/ActivityWindowExplorer.png)

Ayrıca bkz: **değişkenleri çözümlenen** bir etkinlik için her çalıştırma denemesi için **denemeleri** bölümü.

![Çözümlenen değişkenleri](./media/data-factory-monitor-manage-app/ResolvedVariables.PNG)

Geçiş **betik** seçili nesne için JSON kod tanımını görmek için sekmesinde.   

![Komut dosyası sekmesi](./media/data-factory-monitor-manage-app/ScriptTab.png)

Etkinlik pencereleri üç yerde görebilirsiniz:

* Diyagram görünümünde (Orta bölme) etkinlik Windows açılır.
* Sağ bölmede etkinlik penceresi Gezgini.
* Alt bölmede etkinlik Windows liste.

Etkinlik Windows açılır ve etkinlik penceresi Gezgini, sol ve sağ ok kullanarak önceki hafta ve sonraki hafta gezinebilirsiniz.

![Etkinlik penceresi Gezgini sol/sağ okları](./media/data-factory-monitor-manage-app/ActivityWindowExplorerLeftRightArrows.png)

Diyagram görünümü sonunda, bu düğmeler bakın: Yakınlaştır, uzaklaştırma, yakınlaştırma sığacak şekilde, % 100 kilit Düzen yakınlaştırın. **Kilit Düzen** düğmesi engeller, yanlışlıkla tablolar ve işlem hatlarını diyagram Görünümü'nde taşınmasını. Bu varsayılan olarak açıktır. Uygulamayı kapatın ve diyagramda varlıkları taşıyabilirsiniz. Devre dışı bıraktığınızda, tablolar ve işlem hatlarını otomatik olarak yerleştirmek için Son düğmesini kullanabilirsiniz. Fare tekerleğini kullanarak veya Ayrıca yakınlaştırabilirsiniz.

![Diyagram görünümü yakınlaştırma komutları](./media/data-factory-monitor-manage-app/DiagramViewZoomCommands.png)

### <a name="activity-windows-list"></a>Etkinlik Pencereleri listesi
Orta bölmenin alt kısmındaki etkinlik Windows Kaynak Gezgini veya diyagram Görünümü'nde seçilen veri kümesi için tüm etkinlik pencerelerini listeler. Varsayılan olarak, en son etkinlik penceresinin en üstünde gördüğünüzü anlamına gelir, azalan sırada listesidir.

![Etkinlik Pencereleri listesi](./media/data-factory-monitor-manage-app/ActivityWindowsList.png)

Bu listede değil, otomatik olarak yenile Yenile düğmesini el ile yenilemek için araç çubuğunda kullanın.  

Etkinlik pencereleri aşağıdaki durumlardan biri olabilir:

<table>
<tr>
    <th align="left">Durum</th><th align="left">Alt durumu</th><th align="left">Açıklama</th>
</tr>
<tr>
    <td rowspan="8">Bekleniyor</td><td>ScheduleTime</td><td>Zaman çalıştırmaya ilişkin etkinlik penceresinin gelen edilmemiş.</td>
</tr>
<tr>
<td>DatasetDependencies</td><td>Yukarı Akış bağımlılıkları hazır değil.</td>
</tr>
<tr>
<td>ComputeResources</td><td>İşlem kaynakları kullanılamıyor.</td>
</tr>
<tr>
<td>ConcurrencyLimit</td> <td>Tüm etkinlik örnekleri diğer etkinlik pencereleri çalıştırmakla meşgul.</td>
</tr>
<tr>
<td>ActivityResume</td><td>Etkinlik duraklatıldı ve sürdürülene kadar etkinlik pencerelerini çalıştırılamaz.</td>
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
<td>Etkinlik penceresi işleniyor.</td>
</tr>
<tr>
<td rowspan="4">Başarısız</td><td>Zaman aşımına uğradı</td><td>Etkinlik yürütme etkinliği tarafından izin verilenden daha uzun sürdü.</td>
</tr>
<tr>
<td>İptal edildi</td><td>Etkinlik penceresi tarafından bir kullanıcı eylemi iptal edildi.</td>
</tr>
<tr>
<td>Doğrulama</td><td>Doğrulama başarısız oldu.</td>
</tr>
<tr>
<td>-</td><td>Etkinlik penceresi oluşturulan ya da doğrulanması başarısız oldu.</td>
</tr>
<td>Hazır</td><td>-</td><td>Etkinlik penceresi tüketimi için hazırdır.</td>
</tr>
<tr>
<td>Atlandı</td><td>-</td><td>Etkinlik penceresi işlenen değildi.</td>
</tr>
<tr>
<td>None</td><td>-</td><td>Bir etkinlik penceresi farklı bir durum ile kullanılan, ancak sıfırlandı.</td>
</tr>
</table>


Bir etkinlik penceresi listesinde'ye tıkladığınızda ayrıntılarını görmek **etkinlik Windows Explorer** veya **özellikleri** penceresini sağdaki.

![Etkinlik penceresi Gezgini](./media/data-factory-monitor-manage-app/ActivityWindowExplorer-2.png)

### <a name="refresh-activity-windows"></a>Etkinlik pencereleri Yenile
Ayrıntılarını otomatik olarak yenilenir olmayan, bu nedenle etkinlik pencereleri listesinden el ile yenilemek için komut çubuğunda Yenile düğmesine (ikinci düğme) kullanın.  

### <a name="properties-window"></a>Özellik penceresi
Özellikler penceresinde, izleme ve yönetim uygulaması en sağ bölmesinde bulunur.

![Özellik penceresi](./media/data-factory-monitor-manage-app/PropertiesWindow.png)

Kaynak Gezgini (ağaç görünümü), diyagram görünümü veya etkinlik Windows listedeki seçili öğenin özelliklerini gösterir.

### <a name="activity-window-explorer"></a>Etkinlik penceresi Gezgini
**Etkinlik penceresi Gezgini** izleme ve yönetim uygulaması en sağdaki bölmede bir penceredir. Bu etkinlik Windows açılır pencere veya etkinlik Windows listedeki seçili etkinlik penceresinin ayrıntılarını görüntüler.

![Etkinlik penceresi Gezgini](./media/data-factory-monitor-manage-app/ActivityWindowExplorer-3.png)

Takvim görünümünde üst tıklatarak için başka bir etkinlik penceresi arasında geçiş yapabilirsiniz. Ayrıca, etkinlik pencereleri önceki hafta veya sonraki hafta görmek için en üstünde sol ok sağ ok düğmelerini kullanabilirsiniz.

Etkinlik penceresi yeniden veya ayrıntılar bölmesini yenilemek için alt bölmede araç çubuğu düğmelerini kullanabilirsiniz.

### <a name="script"></a>Komut Dosyası
Kullanabileceğiniz **betik** (bağlı hizmet, veri kümesi veya işlem hattı) seçilen Data Factory varlığın JSON tanımını görüntüleme için sekmesinde.

![Komut dosyası sekmesi](./media/data-factory-monitor-manage-app/ScriptTab.png)

## <a name="use-system-views"></a>Sistem görünümleri kullanma
İzleme ve yönetim uygulaması önceden oluşturulmuş sistem görünümleri içerir (**son etkinlik pencereleri**, **başarısız etkinlik pencereleri**, **devam eden etkinlik pencereleri**) izin veren Veri fabrikanızın son/başarısız/Süren etkinlik pencerelerini görüntülemek için.

Geçiş **izleme görünümleri** sekmesini tıklatarak soldaki.

![İzleme görünümleri sekmesi](./media/data-factory-monitor-manage-app/MonitoringViewsTab.png)

Şu anda desteklenen üç sistem görünümleri vardır. Son Etkinlik pencereleri, başarısız olan etkinliği windows veya devam eden etkinlik pencereleri etkinlik Windows listesinde (Orta bölmenin altındaki) görmek için bir seçenek belirleyin.

Seçtiğinizde, **son etkinlik pencereleri** seçeneği, azalan tüm son etkinlik pencereleri görürsünüz **son zaman'ı denemek**.

Kullanabileceğiniz **başarısız etkinlik pencereleri** tüm başarısız etkinlik pencereleri listesinden görüntüleyin. İçinde ayrıntılarını görmek için listedeki bir başarısız etkinlik penceresi seçin **özellikleri** penceresi veya **etkinlik penceresi Gezgini**. Başarısız etkinlik penceresi için tüm günlükleri de indirebilirsiniz.

## <a name="sort-and-filter-activity-windows"></a>Etkinlik pencereleri sıralama ve filtreleme
Değişiklik **başlangıç saati** ve **bitiş saati** filtre etkinliği Windows Komut çubuğu ayarları. Başlangıç ve bitiş zamanı değiştirdikten sonra etkinlik Windows listeyi yenilemek için bitiş zamanı yanındaki düğmeye tıklayın.

![Başlangıç ve bitiş saatleri](./media/data-factory-monitor-manage-app/StartAndEndTimes.png)

> [!NOTE]
> Şu anda her zaman UTC biçiminde izleme ve yönetim uygulaması edilmektedir.
>
>

İçinde **etkinlik Windows liste**, bir sütun adına tıklayın (örneğin: Durumu).

![Etkinlik Windows listesi sütun menüsü](./media/data-factory-monitor-manage-app/ActivityWindowsListColumnMenu.png)

Bunu, aşağıdakileri yapabilirsiniz:

* Artan düzende sıralayın.
* Azalan düzende sırala.
* Bir veya daha fazla değerlerine göre (hazır, bekliyor vb.) filtreleyin.

Bir sütun üzerinde bir filtre belirttiğinizde, filtrelenmiş değerleri sütundaki değerleri gösterir, sütun etkin filtre düğmesine bakın.

![Etkinlik Windows listesinin bir sütun üzerinde filtreleme](./media/data-factory-monitor-manage-app/ActivityWindowsListFilterInColumn.png)

Filtreleri temizlemek için aynı açılır penceresini kullanabilirsiniz. Etkinlik Windows liste için tüm filtreleri temizlemek için Komut çubuğundaki filtreyi Temizle düğmesine tıklayın.

![Etkinlik Windows liste için tüm filtreleri temizle](./media/data-factory-monitor-manage-app/ClearAllFiltersActivityWindowsList.png)

## <a name="perform-batch-actions"></a>Toplu işlemleri
### <a name="rerun-selected-activity-windows"></a>Seçili etkinlik pencereleri yeniden çalıştırın
Bir etkinlik penceresi seçin, ilk komut çubuğunda düğme için aşağı oka tıklayın ve seçin **yeniden** / **ile Yukarı Akış işlem hattını yeniden çalıştırma**. Seçtiğinizde, **ile Yukarı Akış işlem hattını yeniden çalıştırma** seçeneği, onu tüm Yukarı Akış etkinlik pencerelerini de yeniden çalıştırır.
    ![Bir etkinlik penceresi yeniden çalıştırın](./media/data-factory-monitor-manage-app/ReRunSlice.png)

Ayrıca, listede birden çok etkinlik penceresi seçin ve bunları aynı anda yeniden çalıştırın. Etkinlik pencereleri durumu temelinde filtrelemek isteyebilirsiniz (örneğin: **Başarısız**)--ve başarısız olan etkinliği windows etkinlik pencerelerini başarısız olmasına neden olan sorunu düzelttikten sonra yeniden çalıştırın. Etkinlik pencereleri listesinden filtreleme hakkında ayrıntılar için aşağıdaki bölüme bakın.  

### <a name="pauseresume-multiple-pipelines"></a>Birden fazla işlem hattını duraklatın/sürdürün
Ctrl tuşunu kullanarak çoklu seçim yapılabilen iki veya daha fazla işlem hattı kullanabilirsiniz. Bunları duraklatın/sürdürün için (Bu kırmızı dikdörtgeninde aşağıdaki resimde vurgulanmıştır) komut çubuğu düğmelerini kullanabilirsiniz.

![Komut çubuğunda duraklatın/sürdürün](./media/data-factory-monitor-manage-app/SuspendResumeOnCommandBar.png)
