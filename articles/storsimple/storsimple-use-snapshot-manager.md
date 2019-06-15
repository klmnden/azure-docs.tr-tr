---
title: StorSimple Snapshot Manager kullanıcı arabirimini | Microsoft Docs
description: StorSimple Snapshot Manager kullanıcı arabirimini açıklar ve bunu yedekleme işleri ve yedekleme kataloğunu yönetmek için nasıl kullanıldığını açıklar.
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: ''
ms.assetid: c7d91892-2881-41a2-a7a2-908dc3646493
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: v-sharos
ms.custom: ''
ms.openlocfilehash: 46225e5a332e035e4d1cc256e71c4b5d8686fd47
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60845332"
---
# <a name="use-storsimple-snapshot-manager-user-interface-to-manage-backup-jobs-and-backup-catalog"></a>Yedekleme işleri ve yedekleme kataloğunu yönetmek için kullanıcı arabirimi StorSimple Snapshot Manager'ı kullanın

## <a name="overview"></a>Genel Bakış
StorSimple Snapshot Manager alıp yedeklemeler yönetmek için kullanabileceğiniz bir sezgisel bir kullanıcı arabirimine sahiptir. Bu öğretici, kullanıcı arabiriminin bir giriş sağlar ve ardından her bileşenlerinin nasıl kullanılacağını açıklar. Ayrıntılı bir açıklaması StorSimple Snapshot Manager'ın için bkz: [StorSimple Snapshot Manager nedir?](storsimple-what-is-snapshot-manager.md)

### <a name="console-description"></a>Konsol açıklaması
Kullanıcı arabirimini görüntülemek için Masaüstü StorSimple Snapshot Manager simgesine tıklayın. Konsol penceresinde, aşağıdaki çizimde gösterildiği gibi görünür.

![StorSimple Snapshot Manager bölmeleri](./media/storsimple-use-snapshot-manager/HCS_SSM_gui_panes.png)

Konsol penceresinde beş ana öğesine sahip. Her öğe için eksiksiz bir açıklaması için uygun olan bağlantıya tıklayın.

* [Menü çubuğu](#menu-bar) 
* [Araç çubuğu](#tool-bar) 
* [Kapsam bölmesi](#scope-pane) 
* [Sonuçlar Bölmesi](#results-pane) 
* [Eylemler bölmesi](#actions-pane) 

Ayrıca, StorSimple Snapshot Manager'ı destekleyen [klavye gezintisi ve kısayolları sayısı](#keyboard-navigation-and-shortcuts).

### <a name="console-accessibility"></a>Konsolu erişilebilirlik
StorSimple Snapshot Manager kullanıcı arabirimi, Windows işletim sistemi ve Microsoft Yönetim Konsolu (MMC), hem de tarafından bazı StorSimple Snapshot Manager – özel klavye kısayollarını sağlanan erişilebilirlik özelliklerini destekler. 

* Windows erişilebilirlik özellikleri açıklaması için Git [Windows için klavye kısayolları](https://support.microsoft.com/kb/126449). 
* MMC erişilebilirlik özellikleri açıklaması için Git [MMC 3.0 için erişilebilirlik](https://technet.microsoft.com/library/cc766075.aspx)
* StorSimple Snapshot Manager erişilebilirlik özellikleri açıklaması için Git [klavye gezintisi ve kısayolları](#keyboard-navigation-and-shortcuts).

## <a name="menu-bar"></a>Menü çubuğu
Konsol penceresinin üst menü çubuğu içerir [dosya](#file-menu), [eylem](#action-menu), [görünümü](#view-menu), [Sık Kullanılanlar](#favorites-menu), [penceresi](#window-menu), ve [yardımcı](#help-menu) menüleri.

Bu menüde kullanılabilir komutların bir listesini görmek için menü çubuğunda herhangi bir öğeye tıklayın. Aşağıdaki örnekte gösterildiği **görünümü** seçili menü çubuğundaki menü.

![Seçili Görünüm menüsü](./media/storsimple-use-snapshot-manager/HCS_SSM_View_menu.png)

### <a name="file-menu"></a>Dosya menüsü
**Dosya** menüsü, standart Microsoft Yönetim Konsolu (MMC) komutlarını içerir.

#### <a name="menu-access"></a>Menü erişimi
Görüntülemek için **dosya** menüsünde tıklatın **dosya** menü çubuğundaki. Aşağıdaki menü görünür.

![StorSimple Snapshot Manager Dosya menüsü](./media/storsimple-use-snapshot-manager/HCS_SSM_FileMenu.png) 

#### <a name="menu-description"></a>Menü açıklaması
Aşağıdaki tabloda görüntülenen öğeler açıklanmaktadır **dosya** menüsü.

| Menü öğesi | Açıklama |
|:--- |:--- |
| Yeni |Tıklayın **yeni** için StorSimple anlık görüntü yöneticiyi temel alan yeni bir konsol oluşturur. |
| Açık |Tıklayın **açın** varolan konsolunu açın. |
| Kaydet |Tıklayın **Kaydet** geçerli konsol kaydetmek için. |
| Farklı kaydet |Tıklayın **Kaydet** geçerli konsol yeni, yeniden adlandırılmış bir örneğini oluşturmak için. Kullanım **Kaydet** seçeneği bir görünüm özelleştirin ve daha sonra alma işlemi için kaydedin. Örneğin, belirli sunuculara noktası StorSimple Snapshot Manager ek bileşenler oluşturabilirsiniz. |
| Ekle/eklentisini Kaldır |Tıklayın **Ekle/Kaldır ek bileşenini** ek bileşenler Ekle / Kaldır ve düğümleri düzenlemek için **kapsam** bölmesi. Daha fazla bilgi için Git [Ekle, Kaldır ve düzenleme eklentileri ve Uzantıları MMC 3.0](https://technet.microsoft.com/library/cc722035.aspx). |
| Seçenekler |Tıklayın **seçenekleri** console simgesine değiştirmek için kullanıcı erişim modları ve izinleri belirtmek veya kullanılabilir disk alanını artırmak için konsolu dosyaları silin. |
| Dosya yolları listesi |En son açtığınız bir dosyayı yeniden açmak için numaralandırılmış listesindeki bir yol tıklayın. |
| Çıkış |Tıklayın **çıkış** kapatmak için **dosya** menüsü. |

### <a name="action-menu"></a>Eylem menüsü
Kullanım **eylem** menüsünde kullanılabilir eylemler seçin. Öğeler de yaptığınız seçime bağlıdır **kapsam** bölmesinde veya **sonuçları** bölmesi.

#### <a name="menu-access"></a>Menü erişimi
Görüntülenecek **eylem** menüsü, aşağıdakilerden birini yapın:

* Bir öğeyi sağ **kapsam** bölmesinde veya **sonuçları** bölmesi.
* Bir öğe seçin **kapsam** bölmesinde veya **sonuçları** bölmesi ve ardından **eylem** menü çubuğundaki. 

Örneğin, üst düğümü seçerseniz **kapsam** bölmesinde ve ardından sağ tıklatın veya tıklatın **eylem** aşağıdaki menü menü çubuğunda görünür.

![StorSimple Snapshot Manager eylem menüsü](./media/storsimple-use-snapshot-manager/HCS_SSM_Action_menu.png)

**Eylemleri** bölmesinde (sağdaki konsolunun) içeren eylem aynı listesi **eylem** menüsü. Ayrıca, **eylemleri** bölmesidir **görünümü** özel bir görünüm oluşturmanıza olanak sağlayan menü seçenekleri **sonuçları** bölmesi.

![Görünüm menüsü eylemler bölmesini açın](./media/storsimple-use-snapshot-manager/HCS_SSM_ActionsPane_Results.png)

#### <a name="menu-description"></a>Menü açıklaması
Aşağıdaki tabloda StorSimple Snapshot Manager eylemleri alfabetik bir listesini içerir. 

* **Eylem** sütunu, düğümleri ve sonuçları üzerinde gerçekleştirebileceğiniz eylemleri listeler. 
* **Gezinti** sütun açıklar nasıl görüntüleneceğini uygun **eylem** menü eylemi seçebilmeniz için. Bazı eylemler birden çok görünür **eylem** menüleri. Bu eylemler için birini **Gezinti** seçeneğini maddeler. 
* **Açıklama** sütun üzerinde her eylemi kullanmayı açıklar **eylem** menü veya Eylemler bölmesinde ve neler yaptığını açıklar.

> [!NOTE]
> **Eylemleri** bölmesi ve **eylem** menüleri içeren ek seçenekler gibi **görünümü**, **buradan yeni pencere**,  **Yenileme**, **listesini dışarı aktarma**, ve **yardımcı**. Bu seçenekler MMC bir parçası olarak kullanılabilir ve StorSimple Snapshot Manager için özel değildir. Tablo, bu seçeneklerin açıklamaları içerir.
> 
> 

| Eylem | Gezinti | Açıklama |
|:--- |:--- |:--- |
| Kimlik doğrulaması |Tıklayın **cihazları** düğümü, bir cihaza sağ tıklayın ve **sonuçları** bölmesi. |Tıklayın **doğrulaması** cihaz için yapılandırdığınız parolayı girmek için. |
| Kopyala |Genişletin **yedekleme kataloğunu**, genişletme **bulut anlık görüntüleri**tarihli yedekle'ye tıklayın ve ardından bir birimde seçin **sonuçları** bölmesi. |Tıklayın **kopya** bulut anlık görüntüsü bir kopyasını oluşturun ve sizin belirlediğiniz bir konumda depolayın. |
| Bir cihaz yapılandırma |Sağ **cihazları** düğümü. |Tıklayın **bir cihaz yapılandırma** tek veya birden çok cihaz Windows konağına bağlanmak için yapılandırmak için. |
| Yedekleme ilkesi oluşturma |Aşağıdakilerden birini yapın:<ul><li>Sağ **yedekleme ilkeleri**.</li><li>' A tıklayın veya genişletme **birim gruplarını**ve ardından bir birim grubu sağ tıklayın.</li><li>' A tıklayın veya genişletme **yedekleme kataloğu**ve ardından bir birim grubu sağ tıklayın.</li></ul> |Tıklayın **yedekleme İlkesi Oluştur** bir birim grubu için zamanlanmış bir yedekleme yapılandırmak için. |
| Birim grubu oluştur |Aşağıdakilerden birini yapın:<ul><li>Tıklayın **birimleri** düğümünü ve ardından bir birimde sağ tıklayarak **sonuçları** bölmesinde.</li><li>Sağ **birim gruplarını** düğümü.</li></ul> |Tıklayın **birim grubu oluştur** birim için bir birim grubu atamak için. |
| Sil |Bir düğüm veya sonuç tıklayın (Bu öğe birçok görünür **eylem** menüleri ve **eylemleri** bölmeleri.) |Tıklayın **Sil** düğüm veya seçtiğiniz sonuç silinemedi. Onay iletişim kutusunda görüntülendiğinde, onaylayın veya silme işlemi iptal edin. |
| Ayrıntılar |Tıklayın **cihazları** düğümü ve bir cihaza sağ tıklatın **sonuçları** bölmesi. |Tıklayın **ayrıntıları** bir cihaz yapılandırma ayrıntılarını görmek için. |
| Düzenle |Tıklayın **yedekleme ilkeleri**ve ardından bir ilkede sağ tıklayarak **sonuçları** bölmesi. |Tıklayın **Düzenle** bir birim grubu için yedekleme zamanlamasını değiştirmek için. |
| Listeyi dışarı aktar |Herhangi bir düğüm veya sonuç tıklayın (Bu öğe tüm görünür **eylem** menüleri ve **eylemleri** bölmeleri.) |Tıklayın **listeyi dışarı aktar** listesini bir virgülle ayrılmış değer (CSV) dosyasına kaydetmek için. Analiz için bir elektronik tablo uygulamaya daha sonra bu dosyayı içeri aktarabilirsiniz. |
| Help |Herhangi bir düğüm veya sonuç et (Bu öğe tüm görünür **eylem** menüleri ve **eylemleri** bölmeleri.) |Tıklayın **yardımcı** çevrimiçi Yardım içinde ayrı bir tarayıcı penceresi açın. |
| Buradan yeni pencere |Herhangi bir düğüm veya sonuç tıklayın (Bu öğe tüm görünür **eylem** menüleri ve **eylemleri** bölmeleri.) |Tıklayın **buradan yeni pencere** yeni bir StorSimple Snapshot Manager penceresini açın. |
| Yenile |Herhangi bir düğüm veya sonuç tıklayın (Bu öğe tüm görünür **eylem** menüleri ve **eylemleri** bölmeleri.) |Tıklayın **Yenile** şu anda görüntülenen StorSimple Snapshot Manager pencerenin güncelleştirilecek. |
| Cihaz Yenile |Tıklayın **cihazları** düğümü, bir cihaza sağ tıklayın ve **sonuçları** bölmesi. |Tıklayın **Yenile cihaz** belirli bağlı bir cihaz, StorSimple Snapshot Manager ile eşitlemek için. |
| Cihazlarını Yenile |Sağ **cihazları** düğümü. |Tıklayın **Yenile cihazları** StorSimple Snapshot Manager ile bağlı cihazlarınızın listesini eşitlenecek. |
| Birimleri yeniden tara |Sağ **birimleri** düğümü. |Tıklayın **birimleri yeniden tara** görünür birimlerin listesini güncelleştirmek için **sonuçları** bölmesi. |
| Geri Yükleme |Genişletin **yedekleme kataloğunu**, bir birim grubu genişletin, **yerel anlık görüntüleri** veya **bulut anlık görüntüleri**ve ardından yedekleme sağ tıklayın. |Tıklayın **geri** seçilen yedeğin verilerle geçerli birim grubu verileri değiştirmek için. |
| Yedekleyin |Aşağıdakilerden birini yapın:<ul><li>Genişletin **birim gruplarını**ve ardından bir birim grubu sağ tıklayın.</li><li>Genişletin **yedekleme kataloğu**ve ardından bir birim grubu sağ tıklayın.</li></ul> |Tıklayın **ele yedekleme** yedekleme işini hemen başlatmak için. |
| İçeri aktarmalar ekranını Değiştir |İçinde üst düğümünü sağ tıklatın **kapsam** bölmesinde ( **StorSimple Snapshot Manager** örnekler düğümünde). |Tıklayın **içeri aktarmalar ekranını Değiştir** StorSimple cihaz Yöneticisi hizmeti panosundan içeri aktarıldı, ilişkili yedeklemeler ve birim gruplarını gizlemek veya göstermek için. |

### <a name="view-menu"></a>Görünüm menüsü
Kullanım **görünümü** özel görünüm oluşturmak için menü **sonuçları** bölmesinde içeriği. **Görünümü** menüyü içeren **sütunları Ekle/Kaldır** ve **Özelleştir** seçenekleri.

#### <a name="menu-access"></a>Menü erişimi
Erişebileceğiniz **görünümü** menü çubuğunun ya da menüsünde **eylemleri** bölmesi.

![StorSimple Snapshot Manager Görünüm menüsü](./media/storsimple-use-snapshot-manager/HCS_SSM_View_menu.png) 

#### <a name="menu-description"></a>Menü açıklaması
Aşağıdaki tabloda görüntülenen öğeler açıklanmaktadır **görünümü** menüsü.

| Menü öğesi | Açıklama |
|:--- |:--- |
| Sütun Ekle/Kaldır |Tıklayın **sütunları Ekle/Kaldır** içindeki sütun ekleme veya kaldırma için **sonuçları** bölmesi. |
| Özelleştir |Tıklayın **Özelleştir** StorSimple Snapshot Manager konsol penceresinde öğeleri göstermek veya gizlemek için. |

### <a name="favorites-menu"></a>Sık Kullanılanlar menüsü
Kullanma **Sık Kullanılanlar** eklemek, kaldırmak ve sayfa görüntülemeleri ve sık kullandığınız görevleri düzenlemek için menü. 

#### <a name="menu-access"></a>Menü erişimi
Erişebildiğiniz **Sık Kullanılanlar** menü çubuğundaki menü.

![StorSimple Snapshot Manager Sık Kullanılanlar menüsü](./media/storsimple-use-snapshot-manager/HCS_SSM_FavoritesMenu.png)

#### <a name="menu-description"></a>Menü açıklaması
Aşağıdaki tabloda görüntülenen öğeler açıklanmaktadır **Sık Kullanılanlar** menüsü.

| Menü öğesi | Açıklama |
|:--- |:--- |
| Sık Kullanılanlara Ekle |Tıklayın **Sık Kullanılanlara Ekle** geçerli görünüm için Sık Kullanılanlar listenize eklemek için. |
| Sık Kullanılanları Düzenle |Tıklayın **sık kullanılanları** Sık Kullanılanlar klasörünüzün içeriğini düzenlemek için. |

### <a name="window-menu"></a>Pencere menüsü
Kullanım **penceresi** ekleyin ve StorSimple Snapshot Manager konsol pencerelerini yeniden düzenlemek için menü.

#### <a name="menu-access"></a>Menü erişimi
Erişebildiğiniz **penceresi** menü çubuğundaki menü.

![StorSimple Snapshot Manager Pencere menüsü](./media/storsimple-use-snapshot-manager/HCS_SSM_WindowMenu.png)

Şu anda windows açın menüsünün alt kısmındaki numaralı liste gösterir. Pencereyi önplana getirmek için bu listedeki herhangi bir pencereye tıklayın. 

#### <a name="menu-description"></a>Menü açıklaması
Aşağıdaki tabloda, Pencere menüsünde görünen öğeler açıklanmaktadır.

| Menü öğesi | Açıklama |
|:--- |:--- |
| Yeni Pencere |Tıklayın **yeni pencere** (var olan pencereyi) ek olarak yeni bir konsol penceresi açın. |
| Basamakla |Tıklayın **Cascade** açık konsol pencerelerinin basamaklı stil içinde görüntülenecek. |
| Yatay Döşe |Tıklayın **Yatay Döşe** açık konsol pencerelerinin bir kutucuk (veya geniş) biçiminde görüntülemek için. |
| Simgeleri Düzenle |Windows açın ve bunları en aza indirmek ve ardından Masaüstü dağılmış birden çok konsol varsa **Düzenle simgeleri** ekranın alt kısmındaki yatay bir satırda yerleştirmek üzere. |

### <a name="help-menu"></a>Yardım menüsü
Kullanım **yardımcı** StorSimple Snapshot Manager ve MMC kullanılabilir çevrimiçi Yardımı görüntülemek için menü. Ayrıca, sisteminizde yüklü olan MMC ve StorSimple Snapshot Manager yazılımı sürümleri hakkında bilgi görüntüleyebilirsiniz. 

Erişebildiğiniz **yardımcı** menü çubuğundaki menü. StorSimple Snapshot Manager Yardım konuları da erişebilirsiniz **eylemleri** bölmesi.

![StorSimple Snapshot Manager Yardım menüsü](./media/storsimple-use-snapshot-manager/HCS_SSM_HelpMenu.png)

#### <a name="menu-description"></a>Menü açıklaması
Aşağıdaki tabloda Yardım menüsünden görünen öğeleri açıklar.

| Menü öğesi | Açıklama |
|:--- |:--- |
| StorSimple Snapshot Manager'ı Yardım |Tıklayın **yardımcı StorSimple Snapshot Manager üzerinde** StorSimple Snapshot Manager Yardım'ı ayrı bir pencerede açmak için. |
| Yardım konuları |Tıklayın **Yardım konuları** MMC çevrimiçi Yardım ayrı bir pencerede açmak için. |
| TechCenter Web Site |Tıklayın **TechCenter Web sitesi** Microsoft TechNet Tech Center giriş sayfası ayrı bir pencerede açmak için. |
| Microsoft Yönetim Konsolu hakkında |Tıklayın **Microsoft Yönetim Konsolu hakkında** hangi Microsoft Yönetim Konsolu sürümü, sisteminizde yüklü görmek için. |
| StorSimple Snapshot Manager hakkında |Tıklayın **hakkında StorSimple Snapshot Manager** sisteminizde ek bileşenini hangi sürümünün yüklü görmek için. |

## <a name="tool-bar"></a>Araç çubuğu
Menü çubuğunun altında bulunan araç çubuğunun, gezinti ve görev simgeler içerir. Her simge, belirli bir görev için bir kısayoldur.

### <a name="icon-descriptions"></a>Simge açıklamaları
Aşağıdaki tabloda, araç çubuğundaki simgelerinin göründüğünü açıklanmaktadır. 

| Simge | Açıklama |
|:--- |:--- |
| ![Sol Ok](./media/storsimple-use-snapshot-manager/HCS_SSM_LeftArrow.png) |Önceki sayfaya dönmek için sol ok simgesine tıklayın. |
| ![Sağ ok](./media/storsimple-use-snapshot-manager/HCS_SSM_RightArrow.png) |Sonraki sayfaya gitmek için sağ oka tıklayın (gri ok ise eylem kullanılamaz). |
| ![Simgesi](./media/storsimple-use-snapshot-manager/HCS_SSM_Up.png) |Konsol ağacında bir düzey yukarı git yukarı simgesini ( **kapsam** bölmesinde). |
| ![Konsol ağacında Göster/Gizle](./media/storsimple-use-snapshot-manager/HCS_SSM_ShowConsoleTree.png) |Göstermek veya gizlemek için Göster/Gizle Konsol ağacında simgesine tıklayarak **kapsam** bölmesi. |
| ![Listeyi dışarı aktar](./media/storsimple-use-snapshot-manager/HCS_SSM_ExportListIcon.png) |Bir liste, belirttiğiniz bir CSV dosyasına aktarmak için listeyi dışarı aktar simgesine tıklayın. |
| ![Yardım simgesi](./media/storsimple-use-snapshot-manager/HCS_SSM_HelpIcon.png) |Bir çevrimiçi MMC Yardım konusunu açmak için Yardım simgesine tıklayın. |
| ![Eylemler Bölmesini Göster/Gizle](./media/storsimple-use-snapshot-manager/HCS_SSM_ShowAction.png) |Göster/Gizle tıklayın **eylemleri** göstermek veya gizlemek için bölmesi simge **eylemleri** bölmesi. |

## <a name="scope-pane"></a>Kapsam bölmesi
**Kapsam** StorSimple Snapshot Manager kullanıcı arabirimini en soldaki bölmede bölmesidir. Konsolu (ya da düğümü) ağacı içerir ve birincil gezinme için StorSimple Snapshot Manager mekanizmadır. 

### <a name="scope-pane-structure"></a>Kapsam bölmesi yapısı
**Kapsam** bölmesi, bir ağaç yapısında düzenlenmiş bir dizi tıklanabilir nesneleri (düğümler) içerir. 

![Kapsam bölmesi](./media/storsimple-use-snapshot-manager/HCS_SSM_Scope_pane.png) 

* Genişletmek veya daraltmak için bir düğüm için düğüm adının yanındaki ok simgesine tıklayın.
* Durum veya bir düğüm içeriğini görüntülemek için düğüm adına tıklayın. Bilgilerin görünür **sonuçları** bölmesi. 

**Kapsam** bölmesi, aşağıdaki düğümleri içerir: 

* [Cihazlar düğümü](#devices-node) 
* [Birimleri düğümü](#volumes-node) 
* [Birim grupları düğümü](#volume-groups-node) 
* [Yedekleme ilkelerini düğümü](#backup-policies-node) 
* [Yedekleme kataloğu düğümü](#backup-catalog-node) 
* [İşleri düğümü](#jobs-node) 

### <a name="scope-pane-tasks"></a>Kapsam bölmesi görevleri
Kullanabileceğiniz **kapsam** belirli bir düğümde bir eylemi tamamlamak için bölmesi. Bir görev seçmek için aşağıdakilerden birini yapın:

* Düğüme sağ tıklayın ve sonra görüntülenen menüden görevini seçin.
* Düğümüne tıklayın ve ardından **eylem** menü çubuğundaki. Görev, görünen menüden seçim yapın.
* Düğümüne tıklayın ve sonra eylemde seçin **eylemleri** bölmesi.

Bir düğüm seçin ve görev listesini görmek için aşağıdaki yöntemlerden birini kullanın, o düğümde gerçekleştirilebilecek eylemler gösterilir.

### <a name="devices-node"></a>Cihazlar düğümü
**Cihazları** StorSimple cihazını ve StorSimple Snapshot Manager için bağlı bir StorSimple sanal cihazları düğümünü temsil eder. Bu düğümüne bağlanın ve bir cihaz seçin ve onun ilişkili birimleri, birimleri grupları ve var olan yedek kopyaları aktarın. Birden çok cihazı tek bir ana bilgisayara bağlanabilir.

* Düğümü genişletmek için oku yanındaki simgeye **cihazları**.
* Kullanılabilir Eylemler menüsünü görmek için sağ **cihazları** düğümü veya genişletilmiş görüntülenir düğümlerinden herhangi birine sağ tıklayın.
* Yapılandırılan cihazların listesini görmek için tıklayın **cihazları** içinde **kapsam** bölmesi. Cihazlar listesinde, her bir cihaz hakkında bilgi ile birlikte görünür **sonuçları** bölmesi.

### <a name="volumes-node"></a>Birimleri düğümü
**Birimleri** düğümü karşılık gelen iSCSI ve bir cihaz bulunan yoluyla bulunan dahil olmak üzere, ana bilgisayar tarafından bağlanan birimler sürücülere temsil eder. Kullanılabilir birimlerin listesini görüntülemek ve ayrı birimleri birim gruplara atamak için bu düğümü kullanın.

* Düğümü genişletmek için oku yanındaki simgeye **birimleri**.
* Kullanılabilir Eylemler menüsünü görmek için sağ **birimleri** düğümü veya genişletilmiş görüntülenir düğümlerinden herhangi birine sağ tıklayın.
* Birimlerin listesini görmek için tıklayın **birimleri** içinde **kapsam** bölmesi. Her birim ile ilgili bilgi ile birlikte birimlerin listesini görünür **sonuçları** bölmesi.

### <a name="volume-groups-node"></a>Birim grupları düğümü
Birim grupları olarak da bilinen tutarlılık gruplarıdır. Her bir birim grubu yedekleme işlemleri sırasında uygulama tutarlılık sağlamaya yardımcı olan bir uygulamayla ilgili birimlerin havuzudur. Kullanım **birim gruplarını** düğümü bu grupları yapılandırmak ve etkileşimli olarak yedek alabilir veya yedekleme zamanlamaları oluşturun. 

* Düğümü genişletmek için oku yanındaki simgeye **birim gruplarını**.
* Kullanılabilir Eylemler menüsünü görmek için sağ **birim gruplarını** düğümü veya genişletilmiş görüntülenir düğümlerinden herhangi birine sağ tıklayın.
* Birim gruplarını listesini görmek için tıklayın **birim gruplarını** içinde **kapsam** bölmesi. Her birim grubu hakkında bilgi ile birlikte birim grupları listesinde görünür **sonuçları** bölmesi.

### <a name="backup-policies-node"></a>Yedekleme ilkelerini düğümü
Yedekleme ilkelerini olan iş zamanlamaları, yerel ve bulut anlık görüntüleri. Kullanım **yedekleme ilkelerini** ne sıklıkta yedekleme oluşturulur ve ne kadar yedek belirtmek için düğüm tutulacak. 

* Düğümü genişletmek için oku yanındaki simgeye **yedekleme ilkeleri**.
* Kullanılabilir Eylemler menüsünü görmek için sağ **yedekleme ilkeleri** düğümü veya genişletilmiş görüntülenir düğümlerinden herhangi birine sağ tıklayın.
* Yedekleme ilkelerinin bir listesini görmek için tıklayın **yedekleme ilkelerini** içinde **kapsam** bölmesi. Her ilke hakkında bilgi ile birlikte yedekleme ilkeleri listesinde görünür **sonuçları** bölmesi.

> [!NOTE]
> En fazla 64 yedek koruyabilirsiniz.


### <a name="backup-catalog-node"></a>Yedekleme kataloğu düğümü
**Yedekleme kataloğu** düğümü, şirket içi ve şirket dışı yedeklemeler Azure StorSimple birimlerin bir listesini içerir. Bu düğüm, birim grup tarafından düzenlenir ve her bir birim grubu kapsayıcısı yerel anlık görüntüler için ayrı yapıları içerir ( **yerel anlık görüntü**düğümüne) ve bulut anlık görüntüleri ( **bulut anlık görüntüleri** düğümü). Genişletildiğinde, her bir birim grubu kapsayıcısını etkileşimli olarak veya yapılandırılmış bir ilke tarafından gerçekleştirilen başarılı yedeklemeler listelenir.

* Düğümü genişletmek için oku yanındaki simgeye **yedekleme kataloğu**.
* Kullanılabilir Eylemler menüsünü görmek için sağ **yedekleme kataloğu** düğümü veya genişletilmiş görüntülenir düğümlerinden herhangi birine sağ tıklayın.
* Yedekleme anlık görüntülerin bir listesini görmek için tıklayın **yedekleme kataloğunu** içinde **kapsam** bölmesi. Her anlık görüntü hakkında bilgi ile birlikte anlık görüntülerin listesini görünür **sonuçları** bölmesi.

### <a name="local-snapshots-node"></a>Yerel anlık görüntüleri düğümü
**Yerel anlık görüntüleri** düğümü bir özel birim grubu yerel anlık görüntüleri listeler. Düğümü altında bulunan **yedekleme kataloğu** düğümünde **kapsam** bölmesi. Yerel anlık görüntüler, Azure StorSimple cihazında depolanan toplu veri noktası zamanında kopyalarıdır. Genellikle, bu yedekleme türüne oluşturulabilir ve hızlı bir şekilde geri yüklendi. Yerel bir yedek kopya gibi bir yerel anlık görüntü kullanabilirsiniz.

* Düğümü genişletmek için oku yanındaki simgeye **yerel anlık görüntüleri**.
* Kullanılabilir Eylemler menüsünü görmek için sağ **yerel anlık görüntüleri** düğümü veya genişletilmiş görüntülenir düğümlerinden herhangi birine sağ tıklayın.
* Yerel anlık görüntülerin bir listesini görmek için tıklayın **yerel anlık görüntüleri** içinde **kapsam** bölmesi. Her anlık görüntü hakkında bilgi ile birlikte anlık görüntülerin listesini görünür **sonuçları** bölmesi.

### <a name="cloud-snapshots-node"></a>Bulut anlık görüntüleri düğümü
**Bulut anlık görüntüleri** düğümü bir özel birim grubu için bulut anlık görüntülerini listeler. Düğümü altında bulunan **yedekleme kataloğu** düğümünde **kapsam** bölmesi. Bulut anlık görüntüleri, bulutta depolanan toplu veri noktası zamanında kopyalarıdır. Bir bulut anlık görüntüsünü, farklı ve şirket dışı depolama sisteminde çoğaltılan bir anlık görüntü eşdeğerdir. Bulut anlık görüntüleri, olağanüstü durum kurtarma senaryolarda özellikle yararlıdır.

* Düğümü genişletmek için oku yanındaki simgeye **bulut anlık görüntüleri**.
* Kullanılabilir Eylemler menüsünü görmek için sağ **bulut anlık görüntüleri** düğümü veya genişletilmiş görüntülenir düğümlerinden herhangi birine sağ tıklayın.
* Bulut anlık görüntülerin bir listesini görmek için tıklayın **bulut anlık görüntüleri** içinde **kapsam** bölmesi. Her anlık görüntü hakkında bilgi ile birlikte anlık görüntülerin listesini görünür **sonuçları** bölmesi.

### <a name="jobs-node"></a>İşleri düğümü
**İşleri** düğümü, yedekleme işleri zamanlanmış, çalışan ve son tamamlanan hakkında bilgiler içerir. 

* Düğümü genişletmek için oku yanındaki simgeye **işleri**.
* Kullanılabilir Eylemler menüsünü görmek için sağ **işleri** düğümü veya genişletilmiş görüntülenir düğümlerinden herhangi birine sağ tıklayın.
* Zamanlanan işlerin bir listesini görmek için genişletin **işleri** düğümünü ve ardından **zamanlanmış**. Önceden yapılandırılmış işler ve her işle ilgili bilgileri listesi görünür **sonuçları** bölmesi. 
* Tamamlanmış işlerin bir listesini görmek için genişletin **işleri** düğümünü ve ardından **son 24 saat**. Son 24 saat içinde tamamlanmış işlerin bir listesini görünür **sonuçları** bölmesi. **Sonuçları** bölmesinde ayrıca her tamamlanan iş hakkında bilgiler içerir.
* Şu anda çalışan işlerin listesini görmek için genişletin **işleri** düğümünü ve ardından **çalıştıran**. İşler ve her işle ilgili bilgileri şu anda çalışan listesi görünür **sonuçları** bölmesi.

## <a name="results-pane"></a>Sonuçlar Bölmesi
**Sonuçları** StorSimple Snapshot Manager kullanıcı arabirimini Orta bölmede bölmesidir. Listeler ve seçtiğiniz içinde düğüm için ayrıntılı durum bilgisi içeren **kapsam** bölmesi.

### <a name="example"></a>Örnek
Aşağıdaki örnek görmek için tıklayın **birim gruplarını** düğümünde **kapsam** bölmesi. **Sonuçları** bölmesinde birim gruplarını her grubu ayrıntılarını içeren bir listesini görüntüler.

![Sonuçlar Bölmesi](./media/storsimple-use-snapshot-manager/HCS_SSM_Results_pane.png) 

Gösterilen ayrıntıları yapılandırabileceğiniz **sonuçları** bölmesinde: bir düğümüne sağ tıklayın **kapsam** bölmesinde tıklayın **görünümü**ve ardından **sütunları Ekle/Kaldır** .

## <a name="actions-pane"></a>Eylemler bölmesi
**Eylemleri** StorSimple Snapshot Manager kullanıcı arabirimini sağ bölmede bölmesidir. Bir menü düğümü, görünümü veya seçtiğiniz veri gerçekleştirebileceğiniz işlemleri içeren **kapsam** bölmesinde veya **sonuçları** bölmesi. **Eylemleri** bölmesinde olarak aynı komutları içeren **eylem** öğelerinde kullanılabilir olan menüleri **kapsam** bölmesi ve **sonuçları** bölme. Tablodaki her eylem bir açıklaması için bkz. **eylem** menü bölümü.

### <a name="examples"></a>Örnekler
Aşağıdaki örnekte, görmek için **kapsam** bölmesini genişletin **işleri** düğüm ve tıklayın **zamanlanmış**. **Eylemleri** bölmesi için kullanılabilir eylemleri görüntüler **zamanlanmış** düğümü.

![Eylemler bölmesi zamanlanmış işleri örneği](./media/storsimple-use-snapshot-manager/HCS_SSM_ActionsPane.png) 

Daha fazla seçenek görmek için **kapsam** bölmesinde genişletin **işleri** düğümü tıklayın **zamanlanmış**ve ardından zamanlanmış bir işi tıklayın **sonuçları** bölme. **Eylemleri** bölmesi, aşağıdaki örnekte gösterildiği gibi zamanlanmış işi için kullanılabilir eylemleri görüntüler.

![Eylemler bölmesi iş eylemleri örneği](./media/storsimple-use-snapshot-manager/HCS_SSM_ActionsPane_Results.png)

## <a name="keyboard-navigation-and-shortcuts"></a>Klavye ile gezintiyi ve kısayolları
StorSimple Snapshot Manager erişilebilirlik özellikleri Windows işletim sisteminin ve Microsoft Yönetim Konsolu (MMC) sağlar. Ayrıca, bazı klavye gezintisi özelliklerinde ve aşağıdaki bölümlerde açıklandığı gibi StorSimple Snapshot Manager için belirli kısayollar de içerir.

* [Klavye gezinti tuşları](#keyboard-navigation-keys) 
* [Menü çubuğu kısayol tuşları](#menu-bar-shortcut-keys) 
* [Kapsam bölmesi kısayol tuşları](#scope-pane-shortcut-keys) 

### <a name="keyboard-navigation-keys"></a>Klavye gezinti tuşları
Aşağıdaki tabloda StorSimple Snapshot Manager kullanıcı arabirimini gezinmek için kullanabileceğiniz anahtarlar açıklanmaktadır. 

| Gezinti anahtarı | Eylem |
|:--- |:--- |
| Aşağı ok tuşu |Dikey bir menü veya bölmesinde bir sonraki öğeye taşımak için aşağı ok tuşunu kullanın. |
| Enter |Bir eylem tamamlayın ve ardından sonraki adıma devam etmek için Enter tuşuna basın. Örneğin, seçmek için Enter tuşuna basarak **sonraki**, **Tamam**, veya **Oluştur**ve ardından sihirbazda bir sonraki adıma geçin. |
| Esc |Bir menüyü Kapat veya iptal edip bir sayfasını kapatmak için Esc tuşuna basın. |
| F1 |Şu andaki etkin pencere Yardım konusunda görüntülemek için F1 tuşuna basın. |
| F5 |Bir düğümü yenilemek için F5 tuşuna basın. |
| F6 |Sunucudan taşımak için F6 tuşuna **kapsam** bölmesine **sonuçları** bölmesi. |
| F10 |Menü çubuğuna Git F10 tuşuna basın. |
| Sol Ok tuşu |Bir menü çubuğu seçeneğinden önceki seçeneği yatay olarak taşımak için sol ok tuşunu kullanın. Menü çubuğunda, bir önceki öğeye taşıdığınızda, önceki öğenin eylem (veya içerik) menüsünde görünür. |
| Sağ ok tuşu |Yatay bir menü çubuğu seçeneğinden geçiş için sağ ok tuşunu kullanın. Menü çubuğunda, sonraki öğeye taşıdığınızda, yeni öğe için eylem (veya içerik) menüsünde görünür. |
| SEKME tuşu |Konsolunda bir sonraki pencereye veya bir sayfa İleri seçimi veya metin kutusuna gitmek için SEKME tuşunu kullanın. |
| Yukarı Ok tuşu |Dikey bir menü veya bölmesinde önceki öğeyi taşımak için yukarı ok tuşunu kullanın. |

### <a name="menu-bar-shortcut-keys"></a>Menü çubuğu kısayol tuşları
Menü çubuğu için kısayol tuş birleşimleri aşağıdaki tabloda açıklanmaktadır. Kısayol tuşlarına basın ve menüsü açılır sonra menü kısayol tuşları (altı çizili anahtarlar menüsünde) kullanabilirsiniz. Menü çubuğu hakkında daha fazla bilgi için Git [menü çubuğu](#menu-bar).

| Kısayol | Sonuç | Menü kısayol tuşu | Sonuç |
|:--- |:--- |:--- |:--- |
| ALT+F |Açılır **dosya** menüsü. |N |Yeni bir konsol örneği açılır. |
|  |O |Açılır **Yönetimsel Araçlar** sayfası. | |
|  |S |StorSimple Snapshot Manager konsolu kaydeder. | |
|  |A |Açılır **Kaydet** sayfası. | |
|  |M |Açılır **Ekle/Kaldır ek bileşenini** sayfası. | |
|  |P |Açılır **seçenekleri** sayfası. | |
|  |H |Çevrimiçi Yardımı'nı açar. | |
| ALT+A |Açılır **eylem** menüsü. |I |Açar ve içeri aktarma görüntüleme seçenekleri kapatır. |
|  |W |Yeni bir StorSimple Snapshot Manager konsolu açılır. | |
|  |F |StorSimple Snapshot Manager Konsolu güncelleştirir. | |
|  |L |Açılır **listeyi dışarı aktar** sayfası. | |
|  |H |Çevrimiçi Yardımı'nı açar. | |
| ALT+V |Açılır **görünümü** menüsü. |A |Açılır **sütunları Ekle/Kaldır** sayfası. |
|  |U |Açılır **Görünümü Özelleştir** sayfası. | |
| ALT+O |Açılır **Sık Kullanılanlar** menüsü. |A |Açılır **Sık Kullanılanlara Ekle** sayfası. |
|  |O |Açılır **sık kullanılanları** sayfası. | |
| ALT+W |Açılır **penceresi** menüsü. |N |Başka bir StorSimple Snapshot Manager penceresi açılır. |
|  |C |Tüm açık konsol pencerelerinin, basamaklı stil içinde görüntüler. | |
|  |T |Tüm açık konsol pencerelerinin kılavuz düzende görüntüler. | |
|  |I |Ekranın alt kısmındaki yatay bir satırda simgeleri yerleştirir. | |
| ALT+H |Açılır **yardımcı** menüsü. |H |Çevrimiçi Yardımı'nı açar. |
|  |T |Microsoft TechNet Tech Center web sayfası açılır. | |
|  |A |Açılır **Microsoft Yönetim Konsolu hakkında** sayfası. | |

### <a name="scope-pane-shortcut-keys"></a>Kapsam bölmesi kısayol tuşları
Aşağıdaki tablolarda kısayol tuş birleşimleri her düğüm için Göster **kapsam** bölmesi. 

* [Cihazlar düğümü kısayol tuşları](#devices-node-shortcut-keys)
* [Birimleri düğümün kısayol tuşları](#volumes-node-shortcut-keys)
* [Birim grupları düğümü kısayol tuşları](#volume-groups-node-shortcut-keys)
* [Yedekleme ilkeleri düğümün kısayol tuşları](#backup-policies-node-shortcut-keys)
* [Yedekleme kataloğu düğümü kısayol tuşları](#backup-catalog-node-shortcut-keys)
* [İşleri düğümün kısayol tuşları](#jobs-node-shortcut-keys)

#### <a name="devices-node-shortcut-keys"></a>Cihazlar düğümü kısayol tuşları
| Kısayol menüsü | Sonuç |
|:--- |:--- |
| C |Açılır **bir cihaz yapılandırma** sayfası. |
| D |Cihazları ve cihaz ayrıntılarını listesini yeniler. |
| V |Açılır **görünümü** menüsü. |
| W |Odaklanan yeni bir StorSimple Snapshot Manager konsolu açılır **ayrıntıları** düğümü. |
| F |StorSimple Snapshot Manager Konsolu güncelleştirir. |
| L |Açılır **listeyi dışarı aktar** sayfası. |
| H |Çevrimiçi Yardımı'nı açar. |

#### <a name="volumes-node-shortcut-keys"></a>Birimleri düğümün kısayol tuşları
| Kısayol menüsü | Sonuç |
|:--- |:--- |
| V |Birimlerin listesini güncelleştirir. |
| V (iki kez basın) |Açılır **görünümü** menüsü. |
| W |Odaklanan yeni bir StorSimple Snapshot Manager konsolu açılır **birimleri** düğümü. |
| F |StorSimple Snapshot Manager Konsolu güncelleştirir. |
| L |Açılır **listeyi dışarı aktar** sayfası. |
| H |Çevrimiçi Yardımı'nı açar. |

#### <a name="volume-groups-node-shortcut-keys"></a>Birim grupları düğümü kısayol tuşları
| Kısayol menüsü | Sonuç |
|:--- |:--- |
| G |Açılır **bir birim grubu oluştur** sayfası. |
| V |Açılır **görünümü** menüsü. |
| W |Odaklanan yeni bir StorSimple Snapshot Manager konsolu açılır **birim gruplarını** düğümü. |
| F |StorSimple Snapshot Manager Konsolu güncelleştirir. |
| L |Açılır **listeyi dışarı aktar** sayfası. |
| H |Çevrimiçi Yardımı'nı açar. |

#### <a name="backup-policies-node-shortcut-keys"></a>Yedekleme ilkeleri düğümün kısayol tuşları
| Kısayol menüsü | Sonuç |
|:--- |:--- |
| B |Açılır **bir ilke oluşturmak** sayfası. |
| V |Açılır **görünümü** menüsü. |
| W |Odaklanan yeni bir StorSimple Snapshot Manager konsolu açılır **birim gruplarını** düğümü. |
| F |StorSimple Snapshot Manager Konsolu güncelleştirir. |
| L |Açılır ** listeyi dışarı aktar ** sayfası. |
| H |Çevrimiçi Yardımı'nı açar. |

#### <a name="backup-catalog-node-shortcut-keys"></a>Yedekleme kataloğu düğümü kısayol tuşları
| Kısayol menüsü | Sonuç |
|:--- |:--- |
| W |Odaklanan yeni bir StorSimple Snapshot Manager konsolu açılır **birim gruplarını** düğümü. |
| F |StorSimple Snapshot Manager Konsolu güncelleştirir. |
| H |Çevrimiçi Yardımı'nı açar. |

#### <a name="jobs-node-shortcut-keys"></a>İşleri düğümün kısayol tuşları
| Kısayol menüsü | Sonuç |
|:--- |:--- |
| V |Açılır **görünümü** menüsü. |
| W |Odaklanan yeni bir StorSimple Snapshot Manager konsolu açılır **işleri** düğümü. |
| F |StorSimple Snapshot Manager Konsolu güncelleştirir. |
| L |Açılır **listeyi dışarı aktar** sayfası. |
| H |Çevrimiçi Yardım'ı açar |

## <a name="next-steps"></a>Sonraki adımlar
* Bilgi edinmek için nasıl [StorSimple çözümünüzü yönetmek için StorSimple Snapshot Manager kullanmak](storsimple-snapshot-manager-admin.md).
* Bilgi edinmek için nasıl [bağlanmak ve cihazları yönetmek için StorSimple Snapshot Manager kullanmak](storsimple-snapshot-manager-manage-devices.md).

