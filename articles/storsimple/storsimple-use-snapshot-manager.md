---
title: "StorSimple anlık görüntü Yöneticisi kullanıcı arabirimi | Microsoft Docs"
description: "StorSimple Snapshot Manager kullanıcı arabirimini tanımlar ve bu yedekleme işleri ve yedekleme kataloğu yönetmek için nasıl kullanılacağını açıklar."
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: 
ms.assetid: c7d91892-2881-41a2-a7a2-908dc3646493
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: v-sharos
ms.custom: 
ms.openlocfilehash: b48c507e38eb7cadff56259f617e336e4efe5708
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="use-storsimple-snapshot-manager-user-interface-to-manage-backup-jobs-and-backup-catalog"></a>Yedekleme işleri ve yedekleme kataloğu yönetmek için StorSimple anlık görüntü Yöneticisi'ni kullanın kullanıcı arabirimi

## <a name="overview"></a>Genel Bakış
StorSimple Snapshot Manager ele yedeklemeler ve yönetmek için kullanabileceğiniz bir sezgisel bir kullanıcı arabirimine sahiptir. Bu öğretici kullanıcı arabiriminde bir giriş sağlar ve bileşenlerin her birini kullanmayı açıklar. Ayrıntılı bir açıklaması StorSimple anlık görüntü Yöneticisi'nin için bkz: [StorSimple anlık görüntü Yöneticisi nedir?](storsimple-what-is-snapshot-manager.md)

### <a name="console-description"></a>Konsol açıklaması
Kullanıcı arabirimini görüntülemek için masaüstünüzdeki StorSimple Snapshot Manager simgesine tıklayın. Konsol penceresinde, aşağıdaki çizimde gösterildiği gibi görünür.

![StorSimple Snapshot Manager bölmeleri](./media/storsimple-use-snapshot-manager/HCS_SSM_gui_panes.png)

Konsol penceresinde beş temel öğe var. Her öğe eksiksiz bir açıklaması için uygun bağlantıyı tıklatın.

* [Menü çubuğu](#menu-bar) 
* [Araç çubuğu](#tool-bar) 
* [Kapsam bölmesi](#scope-pane) 
* [Sonuçlar Bölmesi](#results-pane) 
* [Eylemler bölmesi](#actions-pane) 

Ayrıca, StorSimple Snapshot Manager destekler [klavye gezinti ve bir dizi kısayol](#keyboard-navigation-and-shortcuts).

### <a name="console-accessibility"></a>Konsolu erişilebilirlik
StorSimple anlık görüntü Yöneticisi kullanıcı arabirimi, Windows işletim sistemi ve Microsoft Yönetim Konsolu (MMC), yanı sıra tarafından bazı StorSimple Snapshot Manager – özel klavye kısayolları sağlanan erişilebilirlik özelliklerini destekler. 

* Windows erişilebilirlik özellikleri açıklaması için Git [Windows için klavye kısayolları](https://support.microsoft.com/kb/126449). 
* MMC erişilebilirlik özellikleri açıklaması için Git [MMC 3.0 için erişilebilirlik](https://technet.microsoft.com/library/cc766075.aspx)
* StorSimple Snapshot Manager erişilebilirlik özellikleri açıklaması için Git [klavye gezinme ve kısayollar](#keyboard-navigation-and-shortcuts).

## <a name="menu-bar"></a>Menü çubuğu
Menü çubuğu konsol penceresinin üst içerir [dosya](#file-menu), [eylem](#action-menu), [Görünüm](#view-menu), [Sık Kullanılanlar](#favorites-menu), [penceresi](#window-menu), ve [yardımcı](#help-menu) menüleri.

Bu menüde kullanılabilir komutların listesini görmek için menü çubuğunda herhangi bir öğeyi tıklatın. Aşağıdaki örnekte gösterildiği **Görünüm** menü çubuğunda seçilen menüsü.

![Seçili Görünüm menüsü](./media/storsimple-use-snapshot-manager/HCS_SSM_View_menu.png)

### <a name="file-menu"></a>Dosya menüsü
**Dosya** menüsü, standart Microsoft Yönetim Konsolu (MMC) komutlarını içerir.

#### <a name="menu-access"></a>Menü erişimi
Görüntülemek için **dosya** menüsünde tıklatın **dosya** menü çubuğunda. Aşağıdaki menüsü görüntülenir.

![StorSimple Snapshot Manager Dosya menüsü](./media/storsimple-use-snapshot-manager/HCS_SSM_FileMenu.png) 

#### <a name="menu-description"></a>Menü açıklaması
Aşağıdaki tablo görünür öğeleri açıklar **dosya** menüsü.

| Menü öğesi | Açıklama |
|:--- |:--- |
| Yeni |Tıklatın **yeni** StorSimple anlık görüntü yöneticiyi temel alan yeni bir konsol oluşturmak için. |
| Açık |Tıklatın **açmak** varolan bir konsolu açın. |
| Kaydet |Tıklatın **kaydetmek** geçerli konsol kaydetmek için. |
| Farklı kaydet |Tıklatın **Kaydet** geçerli konsol yeni, yeniden adlandırılmış bir örneğini oluşturmak için. Kullanım **Kaydet** bir görünümü özelleştirmek ve sonraki alımını kaydetmek için seçeneği. Örneğin, belirli sunuculara noktası StorSimple Snapshot Manager ek bileşenleri oluşturabilirsiniz. |
| Ekleyin veya ek bileşenini Kaldır |Tıklatın **Ekle/Kaldır ek bileşenini** ek bileşen Ekle veya Kaldır ve düğümler düzenlemek için **kapsam** bölmesi. Daha fazla bilgi için Git [Ekle, Kaldır ve Düzenle eklentileri ve Uzantıları MMC 3.0](https://technet.microsoft.com/library/cc722035.aspx). |
| Seçenekler |Tıklatın **seçenekleri** konsol simgesi değiştirmek için kullanıcı erişim modları ve izinleri belirtin veya kullanılabilir disk alanını artırmak için konsol dosyaları silin. |
| Dosya yolları listesi |En son açtığınız bir dosyayı yeniden açmak için numaralandırılmış listesindeki bir yol'ı tıklatın. |
| Çıkış |Tıklatın **çıkış** kapatmak için **dosya** menüsü. |

### <a name="action-menu"></a>Eylem menüsü
Kullanım **eylem** menüsünde kullanılabilir eylemlerden seçin. Öğeler, yaptığınız seçime bağlıdır **kapsam** bölmesinde veya **sonuçları** bölmesi.

#### <a name="menu-access"></a>Menü erişimi
Görüntülemek için **eylem** menüsü, aşağıdakilerden birini yapın:

* Bir öğeyi sağ **kapsam** bölmesinde veya **sonuçları** bölmesi.
* Bir öğe seçin **kapsam** bölmesinde veya **sonuçları** bölmesi ve ardından **eylem** menü çubuğunda. 

Örneğin, en üst düğümü seçerseniz **kapsam** bölmesinde ve ardından sağ tıklayın veya **eylem** aşağıdaki menü menü çubuğunda görünür.

![StorSimple Snapshot Manager eylem menüsü](./media/storsimple-use-snapshot-manager/HCS_SSM_Action_menu.png)

**Eylemler** bölmesinde (sağ konsolunun) Eylemler aynı listesini içeren **eylem** menüsü. Ayrıca, **Eylemler** bölmesidir **Görünüm** özel görünüm oluşturmak etkinleştirmeniz menü seçeneklerini **sonuçları** bölmesi.

![Görünüm menüsü açık eylemler bölmesi](./media/storsimple-use-snapshot-manager/HCS_SSM_ActionsPane_Results.png)

#### <a name="menu-description"></a>Menü açıklaması
Aşağıdaki tabloda StorSimple Snapshot Manager Eylemler alfabetik bir listesini içerir. 

* **Eylem** sütunu, düğümleri ve sonuçları üzerinde gerçekleştirebileceğiniz eylemleri listeler. 
* **Gezinti** sütun açıklar uygun görüntülemek nasıl **eylem** menü eylemi seçebilmeniz için. Bazı eylemler birden çok görünür **eylem** menüleri. Bu Eylemler, seçin **Gezinti** madde işaretli listede seçeneği. 
* **Açıklama** sütun üzerinde her eylem kullanmayı açıklar **eylem** menüsü veya Eylemler bölmesinde ve ne yaptığını açıklar.

> [!NOTE]
> **Eylemler** bölmesinde ve **eylem** menüleri içeren ek seçenekler gibi **Görünüm**, **buradan yeni pencere**, **yenileme**, **Liste Ver**, ve **yardımcı**. Bu seçenekler MMC bir parçası olarak kullanılabilir ve StorSimple Snapshot Manager özel değildir. Tablo, bu seçeneklerin açıklamaları içerir.
> 
> 

| Eylem | Gezinme | Açıklama |
|:--- |:--- |:--- |
| Kimlik doğrulaması |' I tıklatın **aygıtları** düğümü ve bir cihaza sağ **sonuçları** bölmesi. |Tıklatın **kimlik doğrulama** aygıt için yapılandırılmış parola girmesini. |
| Kopyala |Genişletme **yedekleme kataloğu**, genişletin **bulut anlık görüntüleri**tarihli yedekleme tıklayın ve ardından bir birim seçin **sonuçları** bölmesi. |Tıklatın **kopya** bir bulut anlık görüntüsü bir kopyasını oluşturun ve belirlediğiniz bir yerde saklayın. |
| Bir aygıt yapılandırma |Sağ **aygıtları** düğümü. |Tıklatın **bir aygıt yapılandırma** tek bir cihaz ya da Windows ana bilgisayara bağlanmak için birden çok aygıt yapılandırmak için. |
| Yedekleme ilkesi oluşturma |Aşağıdakilerden birini yapın:<ul><li>Sağ **yedekleme ilkeleri**.</li><li>' I tıklatın veya genişletme **birim grupları**ve bir birim grubunu sağ tıklatın.</li><li>' I tıklatın veya genişletme **yedekleme kataloğu**ve bir birim grubunu sağ tıklatın.</li></ul> |Tıklatın **yedekleme İlkesi Oluştur** bir birim grubu için zamanlanmış bir yedekleme yapılandırmak için. |
| Birim grubu oluştur |Aşağıdakilerden birini yapın:<ul><li>Tıklatın **birimleri** düğümü, bir birimi sağ tıklatın **sonuçları** bölmesi.</li><li>Sağ **birim grupları** düğümü.</li></ul> |Tıklatın **birim grubu oluştur** bir birim grubu birimler atamak için. |
| Sil |Bir düğüm veya sonucu tıklayın (Bu öğe birçok görünür **eylem** menüleri ve **Eylemler** bölmeleri.) |Tıklatın **silmek** düğümü veya seçtiğiniz sonuç silmek için. Onay iletişim kutusu göründüğünde, onaylayın veya silme işlemini iptal edebilirsiniz. |
| Ayrıntılar |' I tıklatın **aygıtları** düğümünü ve bir aygıtı sağ tıklatın **sonuçları** bölmesi. |Tıklatın **ayrıntıları** bir aygıt için yapılandırma ayrıntılarını görmek için. |
| Düzenle |Tıklatın **yedekleme ilkeleri**, ilke sağ tıklatın **sonuçları** bölmesi. |Tıklatın **Düzenle** bir birim grubu için yedekleme zamanlamasını değiştirmek için. |
| Listeyi dışarı aktar |Herhangi bir düğüm veya sonuç tıklatın (tüm bu öğe görünür **eylem** menüleri ve **Eylemler** bölmeleri.) |Tıklatın **Liste Ver** listesini bir virgülle ayrılmış değer (CSV) dosyasına kaydetmek için. Çözümleme için bir elektronik tablo uygulamaya sonra bu dosyayı içeri aktarabilirsiniz. |
| Yardım |Bir düğüm veya sonucu'ı tıklatın. (Tüm bu öğe görünür **eylem** menüleri ve **Eylemler** bölmeleri.) |Tıklatın **yardımcı** çevrimiçi Yardım içinde ayrı bir tarayıcı penceresi açın. |
| Buradan yeni pencere |Herhangi bir düğüm veya sonuç tıklatın (tüm bu öğe görünür **eylem** menüleri ve **Eylemler** bölmeleri.) |Tıklatın **buradan yeni pencere** yeni bir StorSimple Snapshot Manager penceresini açın. |
| Yenileme |Herhangi bir düğüm veya sonuç tıklatın (tüm bu öğe görünür **eylem** menüleri ve **Eylemler** bölmeleri.) |Tıklatın **yenileme** şu anda görüntülenen StorSimple Snapshot Manager penceresi güncelleştirmek için. |
| Cihaz Yenile |' I tıklatın **aygıtları** düğümü ve bir cihaza sağ **sonuçları** bölmesi. |Tıklatın **yenileme aygıt** belirli bir bağlı cihaz StorSimple Snapshot Manager ile eşitlemek için. |
| Cihazlarını Yenile |Sağ **aygıtları** düğümü. |Tıklatın **yenileme aygıtları** StorSimple Snapshot Manager ile bağlı cihazlar listenize eşitlenecek. |
| Birimleri yeniden tara |Sağ **birimleri** düğümü. |Tıklatın **birimleri yeniden tara** görünür birimlerin listesini güncelleştirmek için **sonuçları** bölmesi. |
| Geri Yükleme |Genişletme **yedekleme kataloğu**, bir birim grubu genişletin, **yerel anlık görüntüleri** veya **bulut anlık görüntüleri**ve bir yedek sağ tıklatın. |Tıklatın **geri** seçili yedekleme verilerle geçerli birim grubu verileri değiştirmek için. |
| Yedekleyin |Aşağıdakilerden birini yapın:<ul><li>Genişletme **birim grupları**ve bir birim grubunu sağ tıklatın.</li><li>Genişletme **yedekleme kataloğu**ve bir birim grubunu sağ tıklatın.</li></ul> |Tıklatın **ele yedekleme** bir yedekleme işi hemen başlatmak için. |
| İçeri aktarmalar ekranını Değiştir |En üst düğüme sağ **kapsam** bölmesinde ( **StorSimple Snapshot Manager** örnekler düğümünde). |Tıklatın **içeri aktarmalar ekranını Değiştir** birim grupları ve StorSimple cihaz Yöneticisi hizmeti panodan aktarılan ilişkili yedeklemelerinizi göstermek veya gizlemek için. |

### <a name="view-menu"></a>Görünüm menüsü
Kullanım **Görünüm** özel görünüm oluşturmak için menüsünden **sonuçları** bölmesinde içeriği. **Görünüm** menüsü içerir **Sütun Ekle/Kaldır** ve **Özelleştir** seçenekleri.

#### <a name="menu-access"></a>Menü erişimi
Erişebileceğiniz **Görünüm** menüsünde çubuk veya içinde **Eylemler** bölmesi.

![StorSimple Snapshot Manager Görünüm menüsü](./media/storsimple-use-snapshot-manager/HCS_SSM_View_menu.png) 

#### <a name="menu-description"></a>Menü açıklaması
Aşağıdaki tablo görünür öğeleri açıklar **Görünüm** menüsü.

| Menü öğesi | Açıklama |
|:--- |:--- |
| Sütun Ekle/Kaldır |Tıklatın **Sütun Ekle/Kaldır** eklemek veya sütunları kaldırmak için **sonuçları** bölmesi. |
| Özelleştirme |Tıklatın **Özelleştir** StorSimple Snapshot Manager konsol penceresinde öğeleri göstermek veya gizlemek için. |

### <a name="favorites-menu"></a>Sık Kullanılanlar menüsü
Kullanmak **Sık Kullanılanlar** eklemek, kaldırmak ve sayfa görünümleri ve sık kullandığınız görevleri düzenlemek için menüsü. 

#### <a name="menu-access"></a>Menü erişimi
Erişebileceğiniz **Sık Kullanılanlar** menü çubuğundaki menüsü.

![StorSimple Snapshot Manager Sık Kullanılanlar menüsü](./media/storsimple-use-snapshot-manager/HCS_SSM_FavoritesMenu.png)

#### <a name="menu-description"></a>Menü açıklaması
Aşağıdaki tablo görünür öğeleri açıklar **Sık Kullanılanlar** menüsü.

| Menü öğesi | Açıklama |
|:--- |:--- |
| Sık Kullanılanlara Ekle |Tıklatın **Sık Kullanılanlara Ekle** geçerli görünümü Sık Kullanılanlar listenize eklemek için. |
| Sık kullanılanları düzenleme |Tıklatın **kullanılanları** , Sık Kullanılanlar klasörünün içeriğini düzenlemek için. |

### <a name="window-menu"></a>Pencere menüsü
Kullanım **penceresi** ekleyin ve StorSimple Snapshot Manager konsol pencerelerinin yeniden düzenlemek için menüsü.

#### <a name="menu-access"></a>Menü erişimi
Erişebileceğiniz **penceresi** menü çubuğundaki menüsü.

![StorSimple Snapshot Manager Pencere menüsü](./media/storsimple-use-snapshot-manager/HCS_SSM_WindowMenu.png)

Numaralı liste menünün altındaki şu anda windows açmak gösterir. Herhangi bir pencerede pencereyi ön alana getirmek için bu listeyi tıklatın. 

#### <a name="menu-description"></a>Menü açıklaması
Aşağıdaki tablo görünür öğeleri Pencere menüsünden açıklar.

| Menü öğesi | Açıklama |
|:--- |:--- |
| Yeni Pencere |Tıklatın **yeni pencere** (ek olarak mevcut pencere) yeni bir konsol penceresi açın. |
| CASCADE |Tıklatın **Cascade** açık Konsolu windows geçişli stil içinde görüntülemek için. |
| Yatay Döşe |Tıklatın **yatay döşeme** açık Konsolu windows kutucuğu (veya kılavuz) biçimde görüntülemek için. |
| Simgeleri Düzenle |Windows açın ve bunları en aza indirmek ve ardından masaüstünüzü dağılmış birden çok konsol varsa **Düzenle simgeleri** ekranın en altındaki yatay bir satırda yerleştirmek üzere. |

### <a name="help-menu"></a>Yardım menüsü
Kullanım **yardımcı** StorSimple Snapshot Manager ve MMC kullanılabilir çevrimiçi yardımını görüntülemek için menüsü. Sisteminizde yüklü olan MMC ve StorSimple Snapshot Manager yazılımı sürümleri hakkında bilgi de görüntüleyebilirsiniz. 

Erişebileceğiniz **yardımcı** menü çubuğundaki menüsü. StorSimple Snapshot Manager Yardım konuları da erişebilirsiniz **Eylemler** bölmesi.

![StorSimple Snapshot Manager Yardım menüsünü](./media/storsimple-use-snapshot-manager/HCS_SSM_HelpMenu.png)

#### <a name="menu-description"></a>Menü açıklaması
Aşağıdaki tablo görünür öğeleri Yardım menüsünden açıklar.

| Menü öğesi | Açıklama |
|:--- |:--- |
| StorSimple Snapshot Manager Yardımı |Tıklatın **yardımcı StorSimple Snapshot Manager** StorSimple Snapshot Manager Yardımı ayrı bir pencerede açmak için. |
| Yardım konuları |Tıklatın **Yardım konuları** MMC çevrimiçi Yardım'ı ayrı bir pencerede açmak için. |
| TechCenter Web sitesi |Tıklatın **TechCenter Web sitesi** Microsoft TechNet Tech Center giriş sayfası ayrı bir pencerede açmak için. |
| Microsoft Yönetim Konsolu hakkında |Tıklatın **hakkında Microsoft Yönetim Konsolu** , sisteminizde yüklü Microsoft Yönetim Konsolu sürümünü görmek için. |
| StorSimple Snapshot Manager hakkında |Tıklatın **hakkında StorSimple Snapshot Manager** sisteminizde yüklü ek bileşenini sürümünü görmek için. |

## <a name="tool-bar"></a>Araç çubuğu
Menü çubuğu bulunan araç çubuğu gezinti ve görev simgeleri içerir. Belirli bir görev için bir kısayol her simgedir.

### <a name="icon-descriptions"></a>Simge açıklamaları
Aşağıdaki tablo görünür simgeler araç çubuğundaki açıklar. 

| Simgesi | Açıklama |
|:--- |:--- |
| ![Sol Ok](./media/storsimple-use-snapshot-manager/HCS_SSM_LeftArrow.png) |Önceki sayfaya dönmek için sol ok simgesine tıklayın. |
| ![Sağ ok](./media/storsimple-use-snapshot-manager/HCS_SSM_RightArrow.png) |Sonraki sayfaya gitmek için sağ oka tıklayın (oku gri ise, eylem kullanılamaz). |
| ![Yukarı simgesi](./media/storsimple-use-snapshot-manager/HCS_SSM_Up.png) |Konsol ağacında bir düzey yukarı gitmek için yukarı simgesini tıklatın ( **kapsam** bölmesi). |
| ![Konsol Ağacını Göster/Gizle](./media/storsimple-use-snapshot-manager/HCS_SSM_ShowConsoleTree.png) |Göstermek veya gizlemek için Göster/Gizle Konsol ağacında simgesini tıklatın **kapsam** bölmesi. |
| ![Listeyi dışarı aktar](./media/storsimple-use-snapshot-manager/HCS_SSM_ExportListIcon.png) |Bir liste belirttiğiniz bir CSV dosyasına dışarı aktarmak için listeyi dışarı aktar simgesine tıklayın. |
| ![Yardım simgesi](./media/storsimple-use-snapshot-manager/HCS_SSM_HelpIcon.png) |Bir çevrimiçi MMC Yardım konusunu açmak için Yardım simgesine tıklayın. |
| ![Eylemler Bölmesini Göster/Gizle](./media/storsimple-use-snapshot-manager/HCS_SSM_ShowAction.png) |Göster/Gizle'yi **Eylemler** göstermek veya gizlemek için bölmesinde simgesi **Eylemler** bölmesi. |

## <a name="scope-pane"></a>Kapsam bölmesi
**Kapsam** bölmesidir StorSimple Snapshot Manager kullanıcı arabirimini soldaki bölmede. Konsol (ya da düğümü) ağacı içerir ve birincil gezinti için StorSimple Snapshot Manager mekanizmadır. 

### <a name="scope-pane-structure"></a>Kapsam bölmesi yapısı
**Kapsam** bölmesi, bir ağaç yapısı olarak düzenlenmiş bir dizi tıklanabilir nesneleri (düğümler) içerir. 

![Kapsam bölmesi](./media/storsimple-use-snapshot-manager/HCS_SSM_Scope_pane.png) 

* Genişletmek veya bir düğüm daraltmak için düğüm adının yanındaki ok simgesini tıklatın.
* Durum veya bir düğümün içeriğini görüntülemek için düğümü adına tıklayın. Bilgilerin görünür **sonuçları** bölmesi. 

**Kapsam** bölmesi, aşağıdaki düğümleri içerir: 

* [Cihazlar düğümü](#devices-node) 
* [Birimleri düğümü](#volumes-node) 
* [Birim grupları düğümü](#volume-groups-node) 
* [Yedekleme ilkeleri düğümü](#backup-policies-node) 
* [Yedekleme kataloğu düğümü](#backup-catalog-node) 
* [İşlerini düğümü](#jobs-node) 

### <a name="scope-pane-tasks"></a>Kapsam bölmesi görevleri
Kullanabileceğiniz **kapsam** belirli bir düğümde bir eylemi tamamlamak için bölmesi. Bir görev seçmek için aşağıdakilerden birini yapın:

* Düğüme sağ tıklayın ve ardından, açılan menüsünden görevini seçin.
* Düğümüne tıklayın ve ardından **eylem** menü çubuğunda. Görevi görüntülenen menüsünden seçin.
* Düğümünü tıklatın ve sonra eylemde seçin **Eylemler** bölmesi.

Bir düğüm seçin ve görev listesini görmek için bu yöntemlerin herhangi biriyle kullanın, bu düğüm üzerinde gerçekleştirilen eylemler gösterilmektedir.

### <a name="devices-node"></a>Cihazlar düğümü
**Aygıtları** StorSimple cihazları ve StorSimple Snapshot Manager bağlı StorSimple sanal cihazlar düğümünü temsil eder. Bağlanmak ve bir aygıtı yapılandırmak için bu düğümü seçin ve ilişkili birimler, birimler grupları ve varolan yedek kopyaları içeri aktarın. Birden çok cihazı tek bir ana bilgisayara bağlanabilir.

* Düğümü genişletmek için ok simgesine yanındaki tıklayın **aygıtları**.
* Kullanılabilir eylem menüsü görmek için sağ tıklatın **aygıtları** düğümü veya herhangi bir genişletilmiş görüntülenir düğüme sağ tıklayın.
* Yapılandırılmış cihazların bir listesini görmek için tıklatın **aygıtları** içinde **kapsam** bölmesi. Her bir cihaz hakkında bilgi ile birlikte aygıtların listesi görünür **sonuçları** bölmesi.

### <a name="volumes-node"></a>Birimleri düğümü
**Birimleri** düğümü karşılık gelen iSCSI ve bu aygıt üzerinden bulunan yoluyla bulunan dahil olmak üzere ana bilgisayar tarafından bağlanan birimler sürücülere temsil eder. Kullanılabilir birimlerin listesini görüntülemek ve Birim gruplarına bireysel birimler atamak için bu düğümü kullanın.

* Düğümü genişletmek için ok simgesine yanındaki tıklayın **birimleri**.
* Kullanılabilir eylem menüsü görmek için sağ tıklatın **birimleri** düğümü veya herhangi bir genişletilmiş görüntülenir düğüme sağ tıklayın.
* Birimlerin listesini görmek için tıklatın **birimleri** içinde **kapsam** bölmesi. Her birim hakkında bilgi ile birlikte birimlerin listesini görünür **sonuçları** bölmesi.

### <a name="volume-groups-node"></a>Birim grupları düğümü
Birim grupları olarak da bilinen tutarlılık gruplarıdır. Her birim grubu yedekleme işlemleri sırasında uygulama tutarlılığı sağlamak için yardımcı olan bir uygulama ile ilgili birimlerin havuzudur. Kullanım **birim grupları** bu grupları yapılandırmak ve etkileşimli olarak yedek alabilir veya yedekleme zamanlamaları oluşturmak için düğüm. 

* Düğümü genişletmek için ok simgesine yanındaki tıklayın **birim grupları**.
* Kullanılabilir eylem menüsü görmek için sağ tıklatın **birim grupları** düğümü veya herhangi bir genişletilmiş görüntülenir düğüme sağ tıklayın.
* Birim gruplarının bir listesini görmek için tıklatın **birim grupları** içinde **kapsam** bölmesi. Her birim grubu hakkında bilgi ile birlikte toplu gruplarının listesini görünür **sonuçları** bölmesi.

### <a name="backup-policies-node"></a>Yedekleme ilkeleri düğümü
Yedekleme ilkelerdir iş zamanlamalarını, yerel ve bulut anlık görüntüleri. Kullanım **yedekleme ilkeleri** bir yedekleme oluşturulduğunda ne sıklıkta ve ne kadar süreyle yedekleme belirtmek için düğüm tutulacak. 

* Düğümü genişletmek için ok simgesine yanındaki tıklayın **yedekleme ilkeleri**.
* Kullanılabilir eylem menüsü görmek için sağ tıklatın **yedekleme ilkeleri** düğümü veya herhangi bir genişletilmiş görüntülenir düğüme sağ tıklayın.
* Yedekleme ilkelerinin bir listesini görmek için tıklatın **yedekleme ilkeleri** içinde **kapsam** bölmesi. Her İlkesi hakkında bilgi ile birlikte yedekleme ilkelerinin listesi görünür **sonuçları** bölmesi.

> [!NOTE]
> En fazla 64 yedeklemeleri tutabilirsiniz.


### <a name="backup-catalog-node"></a>Yedekleme kataloğu düğümü
**Yedekleme kataloğu** düğümü yerinde ve şirket dışı birimlerin yedekleri, Azure StorSimple listesini içerir. Bu düğüm birim grubu tarafından düzenlenir ve yerel anlık görüntüler için ayrı yapıları her birim grubu kapsayıcısı içerir ( **yerel anlık görüntü**s düğüm) ve bulut anlık görüntüleri ( **bulut anlık görüntüleri** düğüm). Her birim grubu kapsayıcısı genişletildiğinde, etkileşimli olarak veya yapılandırılmış bir ilke tarafından gerçekleştirilen tüm başarılı yedeklemeler listelenir.

* Düğümü genişletmek için ok simgesine yanındaki tıklayın **yedekleme kataloğu**.
* Kullanılabilir eylem menüsü görmek için sağ tıklatın **yedekleme kataloğu** düğümü veya herhangi bir genişletilmiş görüntülenir düğüme sağ tıklayın.
* Yedekleme anlık görüntülerinin listesini görmek için tıklatın **yedekleme kataloğu** içinde **kapsam** bölmesi. Her anlık görüntü hakkında bilgi ile birlikte bir anlık görüntü listesi görünür **sonuçları** bölmesi.

### <a name="local-snapshots-node"></a>Yerel anlık görüntüleri düğümü
**Yerel anlık görüntüleri** düğümü bir özel birim grubu yerel anlık görüntüleri listeler. Düğümü altında bulunan **yedekleme kataloğu** düğümünde **kapsam** bölmesi. Yerel anlık görüntüleri zaman içinde nokta Azure StorSimple cihazında depolanmış birim verilerini kopyalarıdır. Genellikle, bu yedekleme türü oluşturulabilir ve hızlı bir şekilde geri. Yerel bir yedek kopya gibi yerel bir anlık görüntü kullanabilirsiniz.

* Düğümü genişletmek için ok simgesine yanındaki tıklayın **yerel anlık görüntüleri**.
* Kullanılabilir eylem menüsü görmek için sağ tıklatın **yerel anlık görüntüleri** düğümü veya herhangi bir genişletilmiş görüntülenir düğüme sağ tıklayın.
* Yerel anlık görüntüleri listesini görmek için tıklatın **yerel anlık görüntüleri** içinde **kapsam** bölmesi. Her anlık görüntü hakkında bilgi ile birlikte bir anlık görüntü listesi görünür **sonuçları** bölmesi.

### <a name="cloud-snapshots-node"></a>Bulut anlık görüntüleri düğümü
**Bulut anlık görüntüleri** düğümü bir özel birim grubu için bulut anlık görüntüleri listeler. Düğümü altında bulunan **yedekleme kataloğu** düğümünde **kapsam** bölmesi. Bulut anlık görüntüleri zaman içinde nokta bulutta depolanan birim verilerini kopyalarıdır. Bir bulut anlık görüntüsü, farklı, şirket dışı depolama sisteminizde çoğaltılan bir anlık görüntü eşdeğerdir. Bulut anlık görüntüleri olağanüstü durum kurtarma senaryolarında özellikle kullanışlıdır.

* Düğümü genişletmek için ok simgesine yanındaki tıklayın **bulut anlık görüntüleri**.
* Kullanılabilir eylem menüsü görmek için sağ tıklatın **bulut anlık görüntüleri** düğümü veya herhangi bir genişletilmiş görüntülenir düğüme sağ tıklayın.
* Bulut anlık görüntüleri listesini görmek için tıklatın **bulut anlık görüntüleri** içinde **kapsam** bölmesi. Her anlık görüntü hakkında bilgi ile birlikte bir anlık görüntü listesi görünür **sonuçları** bölmesi.

### <a name="jobs-node"></a>İşlerini düğümü
**İşleri** düğümü zamanlanmış, çalışan ve son tamamlanan yedekleme işleri hakkında bilgiler içerir. 

* Düğümü genişletmek için ok simgesine yanındaki tıklayın **işleri**.
* Kullanılabilir eylem menüsü görmek için sağ tıklatın **işleri** düğümü veya herhangi bir genişletilmiş görüntülenir düğüme sağ tıklayın.
* Zamanlanan işler listesini görmek için genişletin **işleri** düğümünü ve ardından **zamanlanmış**. Önceden yapılandırılmış işler ve her bir iş hakkında bilgi listesi içinde görünür **sonuçları** bölmesi. 
* Son zamanlarda tamamlanmış işlerin bir listesini görmek için genişletin **işleri** düğümünü ve ardından **son 24 saat**. Son 24 saat içindeki tamamlanmış işlerin bir listesini görünür **sonuçları** bölmesi. **Sonuçları** bölmesinde her tamamlanmış bir iş hakkında bilgiler de içerir.
* Şu anda çalışan işlerin bir listesini görmek için genişletin **işleri** düğümünü ve ardından **çalıştıran**. İşler ve her işle ilgili bilgileri şu anda çalışan listesini görünür **sonuçları** bölmesi.

## <a name="results-pane"></a>Sonuçlar Bölmesi
**Sonuçları** bölmesidir StorSimple Snapshot Manager kullanıcı arabirimini Orta bölmede. Listeler ve seçtiğiniz içinde düğüm için ayrıntılı durum bilgilerini içeren **kapsam** bölmesi.

### <a name="example"></a>Örnek
Aşağıdaki örnek görmek için tıklatın **birim grupları** düğümünde **kapsam** bölmesi. **Sonuçları** bölmesi her grup hakkında ayrıntılarla birim gruplarının bir listesini görüntüler.

![Sonuçlar Bölmesi](./media/storsimple-use-snapshot-manager/HCS_SSM_Results_pane.png) 

Gösterilen ayrıntıları yapılandırın **sonuçları** bölmesi: bir düğümüne sağ tıklayın **kapsam** bölmesinde tıklatın **Görünüm**ve ardından **Sütun Ekle/Kaldır**.

## <a name="actions-pane"></a>Eylemler bölmesi
**Eylemler** bölmesidir StorSimple Snapshot Manager kullanıcı arabirimini sağ bölmede. Düğüm, görünümü veya seçtiğiniz veri gerçekleştirebileceğiniz işlemler menüsüne içerdiği **kapsam** bölmesinde veya **sonuçları** bölmesi. **Eylemler** bölmesi komutlarla aynı komutları içerir **eylem** öğeler için kullanılabilir menüleri **kapsam** bölmesinde ve **sonuçları** bölmesi. Tablodaki her eylem açıklaması için bkz **eylem** menü bölümü.

### <a name="examples"></a>Örnekler
Aşağıdaki örnekte, görmek için **kapsam** bölmesinde genişletin **işleri** düğümü ve tıklatın **zamanlanmış**. **Eylemler** bölmesi için kullanılabilir eylemleri görüntüler **zamanlanmış** düğümü.

![Eylemler bölmesi zamanlanmış işler örneği](./media/storsimple-use-snapshot-manager/HCS_SSM_ActionsPane.png) 

Daha fazla seçenek görmek için **kapsam** bölmesinde, genişletin **işleri** düğümü tıklatın **zamanlanmış**ve zamanlanmış bir iş'i tıklatın **sonuçları** bölmesi. **Eylemler** bölmesi aşağıdaki örnekte gösterildiği gibi zamanlanmış işi için kullanılabilir eylemleri görüntüler.

![Eylemler bölmesi iş eylemleri örneği](./media/storsimple-use-snapshot-manager/HCS_SSM_ActionsPane_Results.png)

## <a name="keyboard-navigation-and-shortcuts"></a>Klavye gezinme ve kısayollar
StorSimple Snapshot Manager erişilebilirlik özellikleri Windows işletim sisteminin ve Microsoft Yönetim Konsolu (MMC) etkinleştirir. Ayrıca, bazı klavye gezinti özellikleri ve aşağıdaki bölümlerde açıklandığı gibi StorSimple Snapshot Manager için belirli kısayolları de içerir.

* [Klavye gezinti tuşları](#keyboard-navigation-keys) 
* [Menü çubuğu kısayol tuşları](#menu-bar-shortcut-keys) 
* [Kapsam bölmesi kısayol tuşları](#scope-pane-shortcut-keys) 

### <a name="keyboard-navigation-keys"></a>Klavye gezinti tuşları
Aşağıdaki tabloda StorSimple Snapshot Manager kullanıcı arabiriminde gezinme için kullanabileceğiniz tuşları açıklanmaktadır. 

| Gezinti anahtarı | Eylem |
|:--- |:--- |
| Aşağı ok tuşu |Menü veya bölmesindeki bir sonraki öğeye dikey olarak taşımak için aşağı ok tuşunu kullanın. |
| Girin |Bir eylem tamamlayın ve ardından sonraki adıma devam etmek için Enter tuşuna basın. Örneğin, seçmek için Enter tuşuna basabilirsiniz **sonraki**, **Tamam**, veya **oluşturma**ve bir Sihirbazı'nda bir sonraki adıma gidin. |
| ESC |Bir menüyü kapatın veya iptal edin ve bir sayfayı kapatmak için Esc tuşuna basın. |
| F1 |Şu anda etkin pencereyi Yardım konusunda görüntülemek için F1 tuşuna basın. |
| F5 |Bir düğümü yenilemek için F5 tuşuna basın. |
| F6 |Sunucudan taşımak için F6 tuşuna **kapsam** bölmesine **sonuçları** bölmesi. |
| F10 |Menü çubuğu gitmek için F10 tuşuna basın. |
| Sol Ok tuşu |Menü çubuğu seçeneğinden önceki seçeneğine yatay olarak taşımak için sol ok tuşunu kullanın. Menü çubuğunda, önceki öğeye taşıdığınızda, önceki öğe için eylem (veya içerik) menüsü görüntülenir. |
| Sağ ok tuşu |Yatay bir menü çubuğu seçeneğinden diğerine taşımak için sağ ok tuşunu kullanın. Menü çubuğunda sonraki öğeye taşıdığınızda, yeni öğe için eylem (veya içerik) menüsü görüntülenir. |
| SEKME tuşu |Konsolunda sonraki bölme ya da bir sayfa sonraki seçim ya da metin kutusuna taşımak için SEKME tuşunu kullanın. |
| Yukarı Ok tuşu |Önceki öğeye bir menü veya Bölmesi Dikey olarak taşımak için yukarı ok tuşunu kullanın. |

### <a name="menu-bar-shortcut-keys"></a>Menü çubuğu kısayol tuşları
Aşağıdaki tabloda, menü çubuğundaki için kısayol tuş birleşimleri açıklanmaktadır. Kısayol tuşlarını kullanın ve menüsü açılır sonra menü kısayol tuşları (altı çizili anahtarları menüsünde) kullanabilirsiniz. Menü çubuğu hakkında daha fazla bilgi için Git [menü çubuğu](#menu-bar).

| Kısayol | Sonuç | Menü kısayol tuşu | Sonuç |
|:--- |:--- |:--- |:--- |
| ALT + F |Açılır **dosya** menüsü. |N |Yeni bir konsol örneğini açar. |
|  |O |Açılır **Yönetimsel Araçlar** sayfası. | |
|  |S |StorSimple Snapshot Manager konsolunu kaydeder. | |
|  |A |Açılır **Kaydet** sayfası. | |
|  |M |Açılır **Ekle/Kaldır ek bileşenini** sayfası. | |
|  |P |Açılır **seçenekleri** sayfası. | |
|  |H |Çevrimiçi Yardım'ı açar. | |
| ALT + A |Açılır **eylem** menüsü. |I |İçeri aktarma görüntüleme seçeneği açar ve kapatır. |
|  |W |Yeni bir StorSimple Snapshot Manager konsolu açılır. | |
|  |F |StorSimple Snapshot Manager konsol güncelleştirir. | |
|  |L |Açılır **Liste Ver** sayfası. | |
|  |H |Çevrimiçi Yardım'ı açar. | |
| ALT + V |Açılır **Görünüm** menüsü. |A |Açılır **Sütun Ekle/Kaldır** sayfası. |
|  |U |Açılır **Görünümü Özelleştir** sayfası. | |
| ALT + O |Açılır **Sık Kullanılanlar** menüsü. |A |Açılır **Sık Kullanılanlara Ekle** sayfası. |
|  |O |Açılır **kullanılanları** sayfası. | |
| ALT + W |Açılır **penceresi** menüsü. |N |Başka bir StorSimple Snapshot Manager penceresi açılır. |
|  |C |Tüm açık konsol pencerelerinin geçişli stil içinde görüntüler. | |
|  |T |Tüm açık konsol pencerelerinin bir kılavuz düzeni görüntüler. | |
|  |I |Yatay bir satırda ekranın altındaki simgeleri yerleştirir. | |
| ALT + H |Açılır **yardımcı** menüsü. |H |Çevrimiçi Yardım'ı açar. |
|  |T |Microsoft TechNet Tech Center web sayfasını açar. | |
|  |A |Açılır **Microsoft Yönetim Konsolu hakkında** sayfası. | |

### <a name="scope-pane-shortcut-keys"></a>Kapsam bölmesi kısayol tuşları
Aşağıdaki tablolarda kısayol tuş bileşimlerini her düğüm için Göster **kapsam** bölmesi. 

* [Cihazlar düğümü kısayol tuşları](#devices-node-shortcut-keys)
* [Birimleri düğümü kısayol tuşları](#volumes-node-shortcut-keys)
* [Birim grupları düğümü kısayol tuşları](#volume-groups-node-shortcut-keys)
* [Yedekleme ilkeleri düğümü kısayol tuşları](#backup-policies-node-shortcut-keys)
* [Yedekleme kataloğu düğümü kısayol tuşları](#backup-catalog-node-shortcut-keys)
* [İşlerini düğümü kısayol tuşları](#jobs-node-shortcut-keys)

#### <a name="devices-node-shortcut-keys"></a>Cihazlar düğümü kısayol tuşları
| Menü kısayolu | Sonuç |
|:--- |:--- |
| C |Açılır **bir aygıt yapılandırma** sayfası. |
| D |Cihazları ve cihaz ayrıntılarını listesini yeniler. |
| V |Açılır **Görünüm** menüsü. |
| W |Odaklanan yeni bir StorSimple Snapshot Manager konsolunu açar **ayrıntıları** düğümü. |
| F |StorSimple Snapshot Manager konsol güncelleştirir. |
| L |Açılır **Liste Ver** sayfası. |
| H |Çevrimiçi Yardım'ı açar. |

#### <a name="volumes-node-shortcut-keys"></a>Birimleri düğümü kısayol tuşları
| Menü kısayolu | Sonuç |
|:--- |:--- |
| V |Birimlerin listesini güncelleştirir. |
| V (iki kez basın) |Açılır **Görünüm** menüsü. |
| W |Odaklanan yeni bir StorSimple Snapshot Manager konsolunu açar **birimleri** düğümü. |
| F |StorSimple Snapshot Manager konsol güncelleştirir. |
| L |Açılır **Liste Ver** sayfası. |
| H |Çevrimiçi Yardım'ı açar. |

#### <a name="volume-groups-node-shortcut-keys"></a>Birim grupları düğümü kısayol tuşları
| Menü kısayolu | Sonuç |
|:--- |:--- |
| G |Açılır **bir birim grubu oluşturun** sayfası. |
| V |Açılır **Görünüm** menüsü. |
| W |Odaklanan yeni bir StorSimple Snapshot Manager konsolunu açar **birim grupları** düğümü. |
| F |StorSimple Snapshot Manager konsol güncelleştirir. |
| L |Açılır **Liste Ver** sayfası. |
| H |Çevrimiçi Yardım'ı açar. |

#### <a name="backup-policies-node-shortcut-keys"></a>Yedekleme ilkeleri düğümü kısayol tuşları
| Menü kısayolu | Sonuç |
|:--- |:--- |
| B |Açılır **bir ilke oluşturmak** sayfası. |
| V |Açılır **Görünüm** menüsü. |
| W |Odaklanan yeni bir StorSimple Snapshot Manager konsolunu açar **birim grupları** düğümü. |
| F |StorSimple Snapshot Manager konsol güncelleştirir. |
| L |Açılır ** liste ver ** sayfası. |
| H |Çevrimiçi Yardım'ı açar. |

#### <a name="backup-catalog-node-shortcut-keys"></a>Yedekleme kataloğu düğümü kısayol tuşları
| Menü kısayolu | Sonuç |
|:--- |:--- |
| W |Odaklanan yeni bir StorSimple Snapshot Manager konsolunu açar **birim grupları** düğümü. |
| F |StorSimple Snapshot Manager konsol güncelleştirir. |
| H |Çevrimiçi Yardım'ı açar. |

#### <a name="jobs-node-shortcut-keys"></a>İşlerini düğümü kısayol tuşları
| Menü kısayolu | Sonuç |
|:--- |:--- |
| V |Açılır **Görünüm** menüsü. |
| W |Odaklanan yeni bir StorSimple Snapshot Manager konsolunu açar **işleri** düğümü. |
| F |StorSimple Snapshot Manager konsol güncelleştirir. |
| L |Açılır **Liste Ver** sayfası. |
| H |Çevrimiçi Yardım'ı açar |

## <a name="next-steps"></a>Sonraki adımlar
* Bilgi edinmek için nasıl [StorSimple çözümünüzün yönetmek için StorSimple Snapshot Manager kullanmak](storsimple-snapshot-manager-admin.md).
* Bilgi edinmek için nasıl [bağlanmak ve cihazları yönetmek için StorSimple Snapshot Manager kullanmak](storsimple-snapshot-manager-manage-devices.md).

