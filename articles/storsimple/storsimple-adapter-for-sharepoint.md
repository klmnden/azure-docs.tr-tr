---
title: SharePoint için StorSimple bağdaştırıcısı yükleme | Microsoft Docs
description: Yükleme ve yapılandırma veya bir SharePoint sunucu grubundaki SharePoint için StorSimple bağdaştırıcısı kaldırma açıklar.
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: ''
ms.assetid: 36c20b75-f2e5-4184-a6b5-9c5e618f79b2
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/06/2017
ms.author: v-sharos
ms.openlocfilehash: 2e1b231a5cf13d2655ff66c7e48752729c580f48
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
ms.locfileid: "32193827"
---
# <a name="install-and-configure-the-storsimple-adapter-for-sharepoint"></a>Yükleme ve SharePoint için StorSimple bağdaştırıcısını yapılandırın
## <a name="overview"></a>Genel Bakış
SharePoint için StorSimple bağdaştırıcısı, Microsoft Azure StorSimple esnek depolama ve SharePoint server grupları için veri koruması sağlamanıza olanak sağlayan bir bileşendir. İkili büyük nesne (BLOB) içerik SQL Server içerik veritabanları için Microsoft Azure StorSimple karma bulut depolama aygıtı taşımak için bağdaştırıcı kullanabilirsiniz.

SharePoint için StorSimple bağdaştırıcısı Uzak BLOB Depolama (KKY) sağlayıcısı olarak çalışır ve bir StorSimple cihaz tarafından yedeklenen bir dosya sunucusunda (biçiminde BLOB) yapılandırılmamış SharePoint içeriği depolamak için SQL Server Uzak BLOB Depolama özelliğini kullanır.

> [!NOTE]
> SharePoint için StorSimple bağdaştırıcısı SharePoint Server 2010 uzaktan BLOB Depolama (KKY) destekler. SharePoint Server 2010 dış BLOB Depolama (EBS) desteklemez.


* SharePoint için StorSimple bağdaştırıcısı indirmek için Git [SharePoint için StorSimple bağdaştırıcısı] [ 1] Microsoft Download Center'da.
* KKY ve KKY için sınırlamalar planlama hakkında daha fazla bilgi için Git [SharePoint 2013'te KKY kullanmaya karar vermeden] [ 2] veya [planlama KKY (SharePoint Server 2010)][3].

Bu genel bakışta kalan SharePoint ve yükleyip bağdaştırıcısı yapılandırmadan önce bilincinde olmanız gereken SharePoint kapasite ve performans sınırlamaları için StorSimple bağdaştırıcısı rolü kısaca açıklanmaktadır. Bu bilgileri gözden geçirdikten sonra Git [SharePoint yükleme için StorSimple bağdaştırıcısı](#storsimple-adapter-for-sharepoint-installation) bağdaştırıcı kurma başlamak için.

### <a name="storsimple-adapter-for-sharepoint-benefits"></a>SharePoint avantajları için StorSimple bağdaştırıcısı
Bir SharePoint sitesi içerik yapılandırılmamış BLOB veri bir veya daha fazla içerik veritabanları olarak depolanır. Varsayılan olarak, bu veritabanlarını SQL Server çalıştıran ve SharePoint sunucu grubunda bulunan bilgisayarlara barındırılır. BLOB'lar hızlı bir şekilde şirket içi depolama büyük miktarlarda tüketen boyutunu artırabilirsiniz. Bu nedenle, başka bir, daha az maliyetli depolama çözümü bulmak isteyebilirsiniz. SQL Server Uzak Blob Depolama (dosya sistemi, SQL Server veritabanının dışında BLOB içeriği depolamak olanak sağlayan KKY) adlı bir teknoloji sağlar. KKY, BLOB dosya sisteminde SQL Server çalıştıran bilgisayarda bulunabilir veya başka bir sunucu bilgisayarda dosya sistemindeki depolanabilir.

KKY KKY SharePoint etkinleştirmek için SharePoint için StorSimple bağdaştırıcısı gibi bir KKY sağlayıcısını kullanmanızı gerektirir. BLOB'ları Microsoft Azure StorSimple sistemi tarafından yedeklenen bir sunucuya taşımak izin vererek KKY, birlikte SharePoint için StorSimple bağdaştırıcısı çalışır. Microsoft Azure StorSimple sonra BLOB verilerini yerel olarak veya bulutta kullanıma dayalı depolar. (Katman 1 veya etkin verileri genellikle denir) çok etkin BLOB'ları yerel olarak bulunur. Daha az aktif veri ve Arşiv verileri bulutta bulunur. Bir içerik veritabanında KKY etkinleştirdikten sonra SharePoint'te oluşturulan tüm yeni BLOB içerik StorSimple cihazında alan ve içerik veritabanında depolanır.

KKY Microsoft Azure StorSimple uygulamasını aşağıdaki avantajları sağlar:

* Ayrı bir sunucuya taşıma BLOB içeriğinin tarafından SQL Server'da SQL Server yanıtlama hızını iyileştirebilen, sorgu yükünü azaltabilir. 
* Azure StorSimple veri boyutunu düşürmek için yinelenenleri kaldırma ve sıkıştırmasını kullanır.
* Azure StorSimple formdaki yerel ve bulut anlık görüntüleri veri koruması sağlar. StorSimple cihazında veritabanı yerleştirirseniz, ayrıca, içerik veritabanı ve BLOB'lar birlikte kilitlenmeyle tutarlı bir şekilde yedekleyebilirsiniz. (Cihaza içerik veritabanını taşıma yalnızca StorSimple 8000 serisi cihaz için desteklenir. Bu özellik için 5000 veya 7000 Serisi desteklenmiyor.)
* Azure StorSimple yük devretme, dosya ve birim kurtarma (test kurtarma dahil) ve verilerin hızlı geri yükleme gibi olağanüstü durum kurtarma özellikleri içerir.
* SharePoint içeriği öğe düzeyinde kurtarma gerçekleştirmek için BLOB verilerini StorSimple anlık görüntü ile veri kurtarma yazılım Kroll Ontrack PowerControls gibi kullanabilirsiniz. (Bu veri kurtarma ayrı bir satın alma yazılımıdır.)
* SharePoint için StorSimple bağdaştırıcısı merkezi bir konumdan tüm SharePoint çözümünüzü yönetmenizi sağlayan SharePoint Merkezi Yönetim Portalı'na takılan.

BLOB içeriğinin dosya sistemine taşıma diğer maliyet tasarrufu ve avantajları sağlayabilir. Örneğin, KKY kullanarak pahalı Katman 1 depolama gereksinimini azaltır ve içerik veritabanını küçültür çünkü KKY SharePoint sunucu grubunda gerekli veritabanlarının sayısını azaltabilir. Ancak, veritabanı boyutu sınırları gibi diğer faktörlere ve KKY olmayan içerik miktarı da etkileyebilecek depolama gereksinimleri. KKY kullanmanın yararları ve maliyetleri hakkında daha fazla bilgi için bkz: [planlama KKY (SharePoint Foundation 2010)] [ 4] ve [SharePoint 2013'te KKY kullanmaya karar vermeden][5].

### <a name="capacity-and-performance-limits"></a>Kapasite ve performans sınırları
SharePoint çözümünüzde KKY kullanmayı önce test edilmiş performans ve kapasite limitlerini SharePoint Server 2010 ve SharePoint Server 2013 ve bu sınırları için kabul edilebilir performans ne ilişkili haberdar olmanız gerekir. Daha fazla bilgi için bkz: [yazılım sınırları ve SharePoint 2013 için sınırları](https://technet.microsoft.com/library/cc262787.aspx).

KKY yapılandırmadan önce aşağıdakileri gözden geçirin:

* (İçerik veritabanı boyutu) ek olarak ilişkili externalized BLOB boyutu içeriğinin toplam boyutu SharePoint tarafından desteklenen KKY boyut sınırı aşmadığından emin olun. Bu sınır 200 GB'tır. 
  
    **Ölçü içerik veritabanını ve BLOB boyutu**
  
  1. Bu sorgu, merkezi yönetim WFE üzerinde çalıştırın. SharePoint Yönetim Kabuğu'nu başlatın ve içerik veritabanları boyutunu almak için aşağıdaki Windows PowerShell komutu girin:
     
     `Get-SPContentDatabase | Select-Object -ExpandProperty DiskSizeRequired`
     
      Bu adım disk üzerinde içerik veritabanı boyutunu alır.
  2. Aşağıdaki SQL sorgularını birini her içerik veritabanı üzerindeki SQL server kutusunda SQL Management Studio'da çalıştırın ve sonuç sayı olarak elde edilen adım 1 ekleyin.
     
     SharePoint 2013 üzerinde içerik veritabanları, girin:
     
     `SELECT SUM([Size]) FROM [ContentDatabaseName].[dbo].[DocStreams] WHERE [Content] IS NULL`
     
     SharePoint 2010 içerik veritabanlarında, girin:
     
     `SELECT SUM([Size]) FROM [ContentDatabaseName].[dbo].[AllDocs] WHERE [Content] IS NULL`
     
     Bu adım externalized BLOB'lar boyutunu alır.
* Tüm BLOB ve veritabanı içeriği yerel olarak StorSimple cihazında saklamanızı öneririz. StorSimple cihazı yüksek kullanılabilirlik için iki düğümlü bir küme değil. BLOB'ları ve içerik veritabanları StorSimple cihazında yerleştirme yüksek düzeyde kullanılabilirlik sağlar.
  
    StorSimple cihazı içerik veritabanını taşımak için geleneksel SQL Server Geçiş en iyi uygulamaları kullanın. KKY yalnızca veritabanındaki tüm BLOB içeriği dosya paylaşımına taşındıktan sonra veritabanını taşıyın. StorSimple cihazı içerik veritabanını taşımak seçerseniz, birincil bir birim olarak aygıtta içerik veritabanı depolama yapılandırmanızı öneririz.
* Microsoft Azure katmanlı birimleri kullanıyorsanız, StorSimple içinde, cihaz Microsoft Azure bulut depolama alanına katmanlı değil StorSimple üzerinde yerel olarak depolanan içerik güvence altına almak için bir yolu yoktur. Bu nedenle, yerel olarak sabitlenmiş StorSimple birimlerini SharePoint KKY birlikte kullanmanızı öneririz. Bu, tüm BLOB içeriğinin StorSimple cihazında yerel olarak kalır ve Microsoft Azure'a taşınmaz garanti eder.
* StorSimple cihazında içerik veritabanları depolamayın KKY destekleyen geleneksel SQL Server yüksek kullanılabilirlik en iyi uygulamaları kullanın. SQL Server Kümelemesi KKY destekler, SQL Server güncelleştirilirken yansıtmayı desteklemez. 

> [!WARNING]
> KKY etkinleştirmediyseniz, StorSimple cihazı içerik veritabanını taşıma önermiyoruz. Bu sınanmamış bir yapılandırmadır.

## <a name="storsimple-adapter-for-sharepoint-installation"></a>StorSimple bağdaştırıcısı SharePoint yükleme için
SharePoint için StorSimple bağdaştırıcısı yükleyebilmeniz için önce StorSimple cihazı yapılandırma ve SharePoint sunucu grubu ve SQL Server örneğini oluşturmada tüm önkoşulları karşıladığından emin olun. Bu öğretici, yükleme ve SharePoint için StorSimple bağdaştırıcısı yükseltme yordamları yanı sıra yapılandırma gereksinimlerini açıklar.

## <a name="configure-prerequisites"></a>Önkoşulları yapılandırma
SharePoint için StorSimple bağdaştırıcısı yükleyebilmeniz için önce StorSimple cihazı, SharePoint sunucu grubu ve SQL Server örneğini oluşturmada aşağıdaki önkoşulları karşıladığından emin olun.

### <a name="system-requirements"></a>Sistem gereksinimleri
SharePoint için StorSimple bağdaştırıcısı aşağıdaki donanım ve yazılım çalışır:

* Desteklenen işletim sistemi-Windows Server 2008 R2 SP1, Windows Server 2012 veya Windows Server 2012 R2
* Desteklenen SharePoint sürümleri – SharePoint Server 2010 veya SharePoint Server 2013
* Desteklenen SQL Server sürümleri – SQL Server 2008 Enterprise Edition, SQL Server 2008 R2 Enterprise Edition veya SQL Server 2012 Enterprise Edition
* StorSimple cihaz-desteklenen StorSimple 8000 serisi, StorSimple 7000 Serisi veya StorSimple 5000 Serisi.

### <a name="storsimple-device-configuration-prerequisites"></a>StorSimple cihaz yapılandırma önkoşulları
StorSimple cihazı blok cihazı ve bu nedenle veri barındırılabilen bir dosya sunucusu gerekir. SharePoint grubundan var olan bir sunucu yerine ayrı bir sunucu kullanmanızı öneririz. Bu dosya sunucusu içerik veritabanlarını barındıran SQL Server bilgisayarı aynı yerel ağda (LAN) olmalıdır.

> [!TIP]
> * Yüksek kullanılabilirlik için SharePoint grubunu yapılandırmak, yüksek kullanılabilirlik için dosya sunucusu da dağıtmanız gerekir.
> * StorSimple cihazında içerik veritabanını depolamayın KKY desteği geleneksel yüksek kullanılabilirlik en iyi uygulamaları kullanın. SQL Server Kümelemesi KKY destekler, SQL Server güncelleştirilirken yansıtmayı desteklemez. 


SQL Server bilgisayarınızdan erişilebilir ve StorSimple Cihazınızı doğru şekilde yapılandırıldığını ve SharePoint dağıtımınızı destekleyecek şekilde uygun birimleri yapılandırıldığından emin olun. Git [şirket içi StorSimple Cihazınızı dağıtma](storsimple-8000-deployment-walkthrough-u2.md) , henüz dağıtılan ve StorSimple Cihazınızı yapılandırılmışsa. StorSimple cihazı IP adresini not edin; Bu SharePoint yükleme sırasında StorSimple bağdaştırıcısı gerekir.

Buna ek olarak, BLOB externalization için kullanılacak birim aşağıdaki gereksinimleri karşıladığından emin olun:

* Birim 64 KB ayırma birimi boyutu ile biçimlendirilmiş olması gerekir.
* Web front end (WFE) ve uygulama sunucuları, birimin bir Evrensel Adlandırma Kuralı (UNC) yolu üzerinden erişebilir olması gerekir.
* SharePoint sunucu grubu birime yazmak için yapılandırılmış olması gerekir.

> [!NOTE]
> Yükleyip bağdaştırıcısı yapılandırdıktan sonra tüm BLOB externalization StorSimple cihazı üzerinden gitmesi gerekir (aygıt birimleri SQL Server'a sunmak ve depolama katmanları yönetme). Diğer hedefler için BLOB externalization kullanamazsınız.


StorSimple Snapshot Manager BLOB ve veritabanı veri anlık görüntülerini almak için kullanmayı planlıyorsanız, böylece SQL yazıcı hizmeti, Windows Birim Gölge Kopyası Hizmeti (VSS) uygulamak için kullanabileceğiniz StorSimple Snapshot Manager veritabanı sunucusunda yüklediğinizden emin olun.

> [!IMPORTANT]
> StorSimple Snapshot Manager SharePoint VSS yazıcısı desteklemez ve SharePoint verilerinin uygulamayla tutarlı anlık görüntülerini alamıyor. Bir SharePoint senaryosunda, StorSimple Snapshot Manager yalnızca kilitlenme tutarlı yedeklemeler sağlar.


## <a name="sharepoint-farm-configuration-prerequisites"></a>SharePoint grubu yapılandırma önkoşulları
SharePoint sunucu grubunuzu doğru şu şekilde yapılandırıldığından emin olun:

* SharePoint sunucu grubunuzu sağlam durumda olduğunu doğrulayın ve aşağıdakileri denetleyin:
* Tüm SharePoint WFE ve uygulama sunucuları grupta kayıtlı çalıştıran ve üzerinde StorSimple bağdaştırıcısı SharePoint için yükleyeceğiniz sunucusundan ping.
* SharePoint Süreölçer hizmeti (SPTimerV3 veya SPTimerV4), her bir WFE sunucusunda ve uygulama sunucusu çalışıyor.
* SharePoint Süreölçer hizmeti ve SharePoint Merkezi Yönetim sitesi altında çalışan IIS uygulama havuzu yönetici ayrıcalıklarına sahip.
* Internet Explorer Artırılmış güvenlik bağlamı (IE ESC) devre dışı bırakıldığından emin olun. IE ESC'yi devre dışı bırakmak için aşağıdaki adımları izleyin:
  
  1. Internet Explorer'ın tüm örneklerini kapatın.
  2. Sunucu Yöneticisi'ni başlatın.
  3. Sol bölmede **yerel sunucu**.
  4. Sağ bölmede yanına **IE Artırılmış Güvenlik Yapılandırması**, tıklatın **üzerinde**.
  5. Altında **Yöneticiler**, tıklatın **devre dışı**.
  6. **Tamam**’a tıklayın.

## <a name="remote-blob-storage-rbs-prerequisites"></a>Uzak BLOB Depolama (KKY) Önkoşullar
SQL Server'ın desteklenen bir sürümünü kullandığınızdan emin olun. Yalnızca aşağıdaki sürümleri, desteklenen ve KKY kullanmak için:

* SQL Server 2008 Enterprise Edition
* SQL Server 2008 R2 Enterprise Edition
* SQL Server 2012 Enterprise Edition

BLOB'ları, SQL Server için StorSimple cihazı sunan birimlerde externalized. Hiçbir bir hedef BLOB externalization için desteklenir.

Tüm önkoşul yapılandırma adımlarını tamamladıktan sonra Git [SharePoint için StorSimple bağdaştırıcısı](#install-the-storsimple-adapter-for-sharepoint).

## <a name="install-the-storsimple-adapter-for-sharepoint"></a>SharePoint için StorSimple bağdaştırıcısı yükleme
SharePoint için StorSimple bağdaştırıcısı yüklemek için aşağıdaki adımları kullanın. Yazılımı yeniden yüklüyorsanız, bkz: [yükseltme veya SharePoint için StorSimple bağdaştırıcısını yeniden](#upgrade-or-reinstall-the-storsimple-adapter-for-sharepoint). Yükleme için gereken süreyi, SharePoint sunucu grubundaki SharePoint veritabanlarının toplam sayısına bağlı olarak değişir.

[!INCLUDE [storsimple-install-sharepoint-adapter](../../includes/storsimple-install-sharepoint-adapter.md)]

## <a name="configure-rbs"></a>KKY yapılandırın
SharePoint için StorSimple bağdaştırıcısı yükledikten sonra KKY aşağıdaki yordamda açıklandığı gibi yapılandırın.

> [!TIP]
> SharePoint için StorSimple bağdaştırıcısı etkinleştirilmesi veya SharePoint grubundaki her içerik veritabanını devre dışı bırakılması KKY izin vererek SharePoint Merkezi Yönetim sayfasına takılan. Ancak, etkinleştirme veya içerik veritabanını KKY devre dışı bırakma, Grup yapılandırmanıza bağlı olarak, kısa bir süre içinde SharePoint web ön uç (WFE) kullanılabilirliğini kesintiye uğratabilir bir IIS sıfırlama neden olur. (Bir ön uç yük dengeleyici, geçerli bir sunucu iş yükü ve benzeri, kullanımı gibi Etkenler sınırlamak veya bu kesintiyi ortadan kaldırmak.) Kullanıcıların kesilme korumak için etkinleştirme veya KKY yalnızca bir planlı bakım penceresi sırasında devre dışı öneririz.


[!INCLUDE [storsimple-sharepoint-adapter-configure-rbs](../../includes/storsimple-sharepoint-adapter-configure-rbs.md)]

## <a name="configure-garbage-collection"></a>Çöp toplama yapılandırın
Bir SharePoint sitesinden silinen nesneleri, bunlar otomatik olarak KKY depolama biriminden silinmez. Bunun yerine, bir zaman uyumsuz, arka plan bakım program yalnız bırakılmış BLOB dosya Mağazası'ndan siler. Sistem yöneticileri düzenli olarak çalıştırmak için bu işlemi zamanlayabilir veya bunu gerektiğinde başlatabilirsiniz.

KKY etkinleştirdiğinizde bu bakım programı (Microsoft.Data.SqlRemoteBlobs.Maintainer.exe) tüm SharePoint WFE sunucuları ve uygulama sunucuları otomatik olarak yüklenir. Program şu konuma yüklenir: *önyükleme sürücüsü*: \Program SQL Uzak Blob Depolama 10.50\Maintainer\

Yapılandırma ve Bakım programı kullanma hakkında daha fazla bilgi için bkz: [korumak KKY SharePoint Server 2013'te][8].

> [!IMPORTANT]
> KKY Bakımcı yoğun kaynak programdır. SharePoint grubunda yalnızca hafif dönemlerde çalıştıracak şekilde zamanlamanız gerekir.


### <a name="delete-orphaned-blobs-immediately"></a>Yalnız bırakılmış BLOB'ları hemen silme
Yalnız bırakılmış BLOB'ları hemen silmeniz gerekirse, aşağıdaki yönergeleri kullanabilirsiniz. Bu yönergeleri nasıl bu aşağıdaki bileşenleri ile bir SharePoint 2013 ortamda yapılabilir bir örnek olduğuna dikkat edin:

* WSS_Content içerik veritabanı adıdır.
* SQL Server SHRPT13 SQL12\SHRPT13 adıdır.
* Web uygulaması adı SharePoint – 80 ' dir.

[!INCLUDE [storsimple-sharepoint-adapter-garbage-collection](../../includes/storsimple-sharepoint-adapter-garbage-collection.md)]

## <a name="upgrade-or-reinstall-the-storsimple-adapter-for-sharepoint"></a>Yükseltme veya SharePoint için StorSimple bağdaştırıcısı yeniden yükleyin
SharePoint server'ı yükseltmeniz ve SharePoint için StorSimple bağdaştırıcısı yeniden yükleyin veya yalnızca yükseltme veya varolan bir SharePoint sunucu grubuna bağdaştırıcıda yeniden yüklemek için aşağıdaki yordamı kullanın.

> [!IMPORTANT]
> SharePoint yazılım ve/veya yükseltme yükseltin veya SharePoint için StorSimple bağdaştırıcısını yeniden denemeden önce aşağıdaki bilgileri gözden geçirin:
> 
> * KKY dış depolama birimine önceden taşındı herhangi bir dosya yeniden yükleme tamamlandıktan ve KKY özelliği yeniden etkinleştirilene kadar kullanılamaz. Kullanıcı etkilerine sınırlamak için herhangi bir yükseltme veya planlı bakım penceresi sırasında yeniden gerçekleştirin.
> * Yükseltme/yeniden yükleme için gereken süre, SharePoint sunucu grubundaki SharePoint veritabanlarına toplam sayısına bağlı olarak değişebilir.
> * Yükseltme/yeniden yükleme tamamlandıktan sonra içerik veritabanları için KKY etkinleştirmeniz gerekir. Bkz: [yapılandırma KKY](#configure-rbs) daha fazla bilgi için.
> * Veritabanları (büyük), 200'den çok fazla sayıda olan bir SharePoint grubu için yapılandırdığınız KKY **SharePoint Yönetim Merkezi** sayfa zaman aşımına olabilir. Bu gerçekleşirse, sayfayı yenileyin. Bu yapılandırma işlemi etkilemez.


[!INCLUDE [storsimple-upgrade-sharepoint-adapter](../../includes/storsimple-upgrade-sharepoint-adapter.md)]

## <a name="storsimple-adapter-for-sharepoint-removal"></a>StorSimple bağdaştırıcısı SharePoint kaldırma
Aşağıdaki yordamlar, BLOB'lar için SQL Server veritabanlarını geri dönün ve sonra SharePoint için StorSimple bağdaştırıcısı kaldırma açıklar. 

> [!IMPORTANT]
> Bağdaştırıcı yazılımı kaldırmadan önce BLOB içerik veritabanları için geri dönün. gerekir.


### <a name="before-you-begin"></a>Başlamadan önce
Verileri için SQL Server veritabanlarını geri dönün ve bağdaştırıcı kaldırma işlemi başlamadan önce aşağıdaki bilgileri toplayın:

* KKY etkin olduğu tüm veritabanlarının adlarını
* Yapılandırılmış BLOB deposu UNC yolu

### <a name="move-the-blobs-back-to-the-content-databases"></a>BLOB'ları içerik veritabanları için geri dönün
SharePoint yazılım için StorSimple bağdaştırıcısı kaldırmadan önce tüm externalized BLOB'ları geri SQL Server içerik veritabanlarına geçirmeniz gerekir. Tüm BLOB'lar içerik veritabanlarına geri taşımadan önce SharePoint için StorSimple bağdaştırıcısı kaldırmayı denerseniz, aşağıdaki uyarı iletisini görürsünüz.

![Uyarı iletisi](./media/storsimple-adapter-for-sharepoint/sasp1.png)

#### <a name="to-move-the-blobs-back-to-the-content-databases"></a>BLOB'ları içerik veritabanlarına geri taşımak için
1. Externalized nesnelerin her biri indirin.
2. Açık **SharePoint Yönetim Merkezi** sayfasında ve Gözat **sistem ayarlarını**.
3. Altında **Azure StorSimple**, tıklatın **StorSimple bağdaştırıcısı yapılandırma**.
4. Üzerinde **StorSimple bağdaştırıcısını yapılandırın** sayfasında, **devre dışı** her bir dış BLOB depolama alanından kaldırmak istediğiniz içerik veritabanı aşağıdaki düğmesine. 
5. Nesneleri SharePoint'ten silin ve ardından yeniden yükleyin.

Alternatif olarak, Microsoft kullanabilirsiniz` RBS Migrate()` PowerShell cmdlet'i ile SharePoint dahil. Daha fazla bilgi için bkz: [içine veya dışına KKY İçerik Geçişi](https://technet.microsoft.com/library/ff628255.aspx).

BLOB'ları içerik veritabanını geri taşıdıktan sonra sonraki adıma gidin: [bağdaştırıcı kaldırma](#uninstall-the-adapter).

### <a name="uninstall-the-adapter"></a>Bağdaştırıcı Kaldırma
BLOB'lar için SQL Server veritabanlarını geri taşıdıktan sonra SharePoint için StorSimple bağdaştırıcısı kaldırmak için aşağıdaki seçeneklerden birini kullanın.

#### <a name="to-use-the-installation-program-to-uninstall-the-adapter"></a>Bağdaştırıcıyı kaldırmak için yükleme programını kullanmak için
1. Web ön uç (WFE) sunucusuna oturum açmak için yönetici ayrıcalıklarına sahip bir hesap kullanın.
2. StorSimple bağdaştırıcısı SharePoint yükleyici için çift tıklayın. Kurulum Sihirbazı'nı başlatır.
   
    ![Kurulum Sihirbazı](./media/storsimple-adapter-for-sharepoint/sasp2.png)
3. **İleri**’ye tıklayın. Aşağıdaki sayfası görüntülenir.
   
    ![Kurulum Sihirbazı'nı Kaldır sayfası](./media/storsimple-adapter-for-sharepoint/sasp3.png)
4. Tıklatın **kaldırmak** kaldırma işlemini seçin. Aşağıdaki sayfası görüntülenir.
   
    ![Kurulum Sihirbazı onay sayfası](./media/storsimple-adapter-for-sharepoint/sasp4.png)
5. Tıklatın **kaldırmak** kaldırma işlemini onaylamak için. Aşağıdaki ilerleme durumu sayfası görüntülenir.
   
    ![Kurulum Sihirbazı'nı ilerleme durumu sayfası](./media/storsimple-adapter-for-sharepoint/sasp5.png)
6. Kaldırma tamamlandığında Son'a sayfası görüntülenir. Tıklatın **son** Kurulum Sihirbazı'nı kapatın.

#### <a name="to-use-the-control-panel-to-uninstall-the-adapter"></a>Bağdaştırıcıyı kaldırmak için Denetim Masası'nı kullanmak için
1. Denetim Masası'nı açın ve ardından **programlar ve Özellikler**.
2. Seçin **SharePoint için StorSimple bağdaştırıcısı**ve ardından **kaldırma**.

## <a name="next-steps"></a>Sonraki adımlar
[StorSimple hakkında daha fazla bilgi](storsimple-overview.md).

<!--Reference links-->
[1]: https://www.microsoft.com/download/details.aspx?id=44073
[2]: https://technet.microsoft.com/library/ff628583(v=office.15).aspx
[3]: https://technet.microsoft.com/library/ff628583(v=office.14).aspx
[4]: https://technet.microsoft.com/library/ff628569(v=office.14).aspx
[5]: https://technet.microsoft.com/library/ff628583(v=office.15).aspx
[8]: https://technet.microsoft.com/library/ff943565.aspx
