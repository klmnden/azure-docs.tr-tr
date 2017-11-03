---
title: StorSimple 8000 serisi NetBackup olan yedekleme hedefi olarak | Microsoft Docs
description: "VERITAS NetBackup StorSimple yedekleme hedefi yapılandırmayla açıklar."
services: storsimple
documentationcenter: 
author: harshakirank
manager: matd
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/15/2017
ms.author: hkanna
ms.openlocfilehash: b1878c181a77ac6d54654fc55228907743243c45
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="storsimple-as-a-backup-target-with-netbackup"></a>Yedekleme hedefi olarak StorSimple NetBackup ile

## <a name="overview"></a>Genel Bakış

Azure StorSimple bir Microsoft karma bulut depolama çözümüdür. StorSimple üstel veri büyümesi karmaşıklığını şirket içi çözüm bir uzantısı olarak Azure storage hesabı kullanarak ve şirket içi depolama ve bulut depolama arasında otomatik olarak veri katmanlama giderir.

Bu makalede, VERITAS NetBackup ve her iki çözüm de tümleştirmek için en iyi yöntemler StorSimple tümleştirme tartışın. Biz de en iyi StorSimple ile tümleştirmek için VERITAS NetBackup ayarlama konusunda önerilerde. VERITAS en iyi yöntemler, yedekleme mimarlar ve Yöneticiler için tek tek yedekleme gereksinimlerini ve hizmet düzeyi sözleşmelerine (SLA) karşılayacak şekilde VERITAS NetBackup ayarlamak en iyi yolu erteleyin.

Biz yapılandırma adımları ve temel kavramları göstermeye karşın, bu makalede halinde bir adım adım yapılandırma veya yükleme kılavuzdur. Temel bileşenleri ve altyapı çalışma sırayla ve biz açıklamak kavramları desteklemeye hazır varsayıyoruz.

### <a name="who-should-read-this"></a>Bu kimler içindir?

Bu makaledeki bilgiler, yedekleme yöneticilerinin, depolama yöneticilerinin ve depolama, Windows Server 2012 R2, Ethernet, bulut Hizmetleri ve VERITAS NetBackup bilgisine sahip depolama mimarları için en yararlı olacaktır.

### <a name="supported-versions"></a>Desteklenen sürümler

-   NetBackup 7.7.x ve sonraki sürümler
-   [StorSimple güncelleştirme 3 ve sonraki sürümler](storsimple-overview.md#storsimple-workload-summary)


## <a name="why-storsimple-as-a-backup-target"></a>Neden StorSimple yedekleme hedefi olarak?

StorSimple yedekleme hedefi için iyi bir seçimdir çünkü:

-   Herhangi bir değişiklik yapılmadan bir hızlı yedekleme hedefi olarak kullanmak üzere yedekleme uygulamaları için standart, yerel depolama sağlar. StorSimple bir hızlı son yedekleri geri yükleme için de kullanabilirsiniz.
-   Kendi bulut katmanlandırma düşük maliyetli Azure depolama kullanmak için bir Azure bulut depolama hesabıyla sorunsuz bir şekilde tümleşiktir.
-   Otomatik olarak site dışı depolama için olağanüstü durum kurtarma sağlar.

## <a name="key-concepts"></a>Önemli kavramlar

Tüm depolama çözümü ile gibi dikkatli bir değerlendirme çözümün depolama performansını SLA, değişiklik ve kapasite büyüme gereksinimlerine başarısı için çok önemli hızıdır. Ana, erişim zamanları ve kapatma işini yapmak için StorSimple yeteneklerini temel bir rolde bulut çalmak için bir bulut katmanı sunarak olur.

StorSimple verileri (etkin) iyi tanımlanmış çalışma kümesinde çalışan uygulamalar için depolama sağlamak için tasarlanmıştır. Bu model, çalışma kümesi, veri yerel katmanları üzerinde depolanır ve kalan dışı/soğuk/arşivlenen veri kümesi bulut için katmanlı. Bu model aşağıdaki şekilde gösterilir. Neredeyse Düz yeşil bir çizgi StorSimple cihaz yerel katmanlarda üzerinde depolanan verileri temsil eder. Kırmızı çizgi toplam tüm katmanlar arasında StorSimple çözüm üzerinde depolanan veri miktarını temsil eder. Düz yeşil bir çizgi ve üstel kırmızı eğri arasındaki boşluğu toplam bulutta depolanan veri miktarını temsil eder.

**StorSimple katmanlama**
![StorSimple katmanlama diyagramı](./media/storsimple-configure-backup-target-using-netbackup/image1.jpg)

Aklınızda bu mimari ile StorSimple yedekleme hedefi olarak çalışmak için idealdir olduğunu göreceksiniz. StorSimple için kullanabilirsiniz:
-   En sık rastlanan geri yüklemeler veri yerel çalışma kümesinden gerçekleştirin.
-   Bulut geri yüklemeler daha az sıklıkta olduğu site dışı olağanüstü durum kurtarma ve eski verileri için kullanın.

## <a name="storsimple-benefits"></a>StorSimple avantajları

StorSimple Microsoft Azure ile sorunsuz erişim şirket içi yararlanarak sorunsuz olarak tümleşik bir şirket içi çözümü sağlar ve bulut depolama.

StorSimple katı hal aygıt (SSD) ve seri bağlı SCSI (SAS) depolama olan şirket içi cihaz ve Azure Storage arasında otomatik katmanlama kullanır. Otomatik katmanlama sık erişilen verileri SSD ve SAS katmanda bulunan yerel tutar. Bu, Azure depolama birimine seyrek erişilen verileri taşır.

StorSimple avantajlar sunar:

-   Eşi görülmemiş yinelenenleri kaldırma düzeyleri elde etmek için bulut kullanan benzersiz yinelenenleri kaldırma ve sıkıştırma algoritmaları
-   Yüksek kullanılabilirlik
-   Azure coğrafi çoğaltma kullanarak coğrafi çoğaltma
-   Azure tümleştirme
-   Bulut veri şifrelemesi
-   Geliştirilmiş olağanüstü durum kurtarma ve uyumluluk

StorSimple temelde iki ana dağıtım senaryoları (Yedekleme hedefi birincil ve ikincil yedekleme hedefi) gösterir, ama bir düz, blok depolama cihazı vardır. StorSimple tüm sıkıştırma yapar ve yinelenenleri kaldırma. Sorunsuz bir şekilde gönderir ve bulut ile uygulama ve dosya sistemi arasındaki verileri alır.

StorSimple hakkında daha fazla bilgi için bkz: [StorSimple 8000 serisi: karma bulut depolama çözümü](storsimple-overview.md). Ayrıca, gözden geçirebilirsiniz [teknik StorSimple 8000 serisi özellikleri](storsimple-technical-specifications-and-compliance.md).

> [!IMPORTANT]
> Bir StorSimple kullanarak aygıt yedekleme hedefi olarak yalnızca StorSimple 8000 güncelleştirme 3 ve sonraki sürümler için desteklenir.

## <a name="architecture-overview"></a>Mimariye genel bakış

Aşağıdaki tablolarda, cihaz modeli mimari ilk yönergeleri gösterilmektedir.

**StorSimple kapasiteler, yerel ve bulut depolama**

| Depolama kapasitesi       | 8100          | 8600            |
|------------------------|---------------|-----------------|
| Yerel depolama kapasitesi | &lt;10 Tıb\*  | &lt;20 Tıb\*  |
| Bulut depolama kapasitesi | &gt;200 Tıb\* | &gt;500 Tıb\* |
\*Depolama boyutu hiçbir yinelenenleri kaldırma veya sıkıştırma varsayar.

**Birincil ve ikincil yedeklemeleri StorSimple kapasiteleri**

| Yedekleme senaryosu  | Yerel depolama kapasitesi  | Bulut depolama kapasitesi  |
|---|---|---|
| Birincil yedekleme  | Kurtarma noktası hedefi (RPO) karşılamak için son yedeklemelerini Hızlı Kurtarma için yerel depolama birimine depolanır | Bulut kapasitesi Yedekleme geçmişi (RPO) uygun |
| İkincil yedekleme | İkincil kopya yedekleme verilerinin bulut kapasitesi depolanabilir  | Yok  |

## <a name="storsimple-as-a-primary-backup-target"></a>StorSimple birincil yedekleme hedefi olarak

Bu senaryoda, StorSimple birimlerini yedekleme uygulamasına yedeklemeler için tek depo olarak sunulur. Aşağıdaki şekilde tüm yedeklemeler kullanım StorSimple birimler yedekleme ve geri yüklemeler için katmanlı bir çözüm mimarisini göstermektedir.

![StorSimple olarak birincil yedekleme hedefi mantıksal diyagramı](./media/storsimple-configure-backup-target-using-netbackup/primarybackuptargetlogicaldiagram.png)

### <a name="primary-target-backup-logical-steps"></a>Birincil hedef yedekleme mantıksal adımları

1.  Hedef Yedekleme aracısı yedekleme sunucusu ile iletişim kurar ve yedekleme aracısını veri yedekleme sunucusuna iletir.
2.  Yedekleme sunucusuna StorSimple verileri yazar katmanlı birimler.
3.  Yedekleme sunucusuna katalog veritabanını güncelleştirir ve yedekleme işi tamamlandıktan.
4.  Bir anlık görüntü betik StorSimple anlık görüntü Yöneticisi (başlatma veya silme) tetikler.
5.  Yedekleme sunucusuna bir bekletme ilkesi temel alınarak süresi dolan yedeklemeleri siler.

### <a name="primary-target-restore-logical-steps"></a>Birincil hedef geri yükleme mantıksal adımları

1.  Yedek sunucu uygun veri depolama depodan geri yüklemeyi başlatır.
2.  Yedekleme aracısı yedekleme sunucusundan verileri alır.
3.  Yedekleme sunucusuna geri yükleme işi tamamlar.

## <a name="storsimple-as-a-secondary-backup-target"></a>İkincil bir yedekleme hedefi olarak StorSimple

Bu senaryoda, StorSimple birimlerini öncelikle uzun vadeli bekletme veya arşivleme için kullanılır.

Aşağıdaki şekilde, hangi ilk yedeklemelerin bir mimari gösterilir ve yüksek performanslı hedef birim geri yükler. Bu yedeklemeler kopyalanır ve bir StorSimple arşivlenmiş katmanlı birim üzerinde ayarlanmış bir planlamada.

Bekletme İlkesi kapasite ve performans gereksinimlerinizi işleyebilecek şekilde yüksek performanslı biriminiz boyutlandırmak önemlidir.

![StorSimple olarak ikincil yedekleme hedefi mantıksal diyagramı](./media/storsimple-configure-backup-target-using-netbackup/secondarybackuptargetlogicaldiagram.png)

### <a name="secondary-target-backup-logical-steps"></a>İkincil hedef yedekleme mantıksal adımları

1.  Hedef Yedekleme aracısı yedekleme sunucusu ile iletişim kurar ve yedekleme aracısını veri yedekleme sunucusuna iletir.
2.  Yedekleme sunucusu, yüksek performanslı depolama verileri yazar.
3.  Yedekleme sunucusuna katalog veritabanını güncelleştirir ve yedekleme işi tamamlandıktan.
4.  Yedekleme sunucusuna bir bekletme ilkesi temel alınarak StorSimple yedeklemeler kopyalar.
5.  Bir anlık görüntü betik StorSimple anlık görüntü Yöneticisi (başlatma veya silme) tetikler.
6.  Yedekleme sunucusuna bir bekletme ilkesi temel alınarak süresi dolan yedeklemeleri siler.

### <a name="secondary-target-restore-logical-steps"></a>İkincil hedef geri yükleme mantıksal adımları

1.  Yedek sunucu uygun veri depolama depodan geri yüklemeyi başlatır.
2.  Yedekleme aracısı yedekleme sunucusundan verileri alır.
3.  Yedekleme sunucusuna geri yükleme işi tamamlar.

## <a name="deploy-the-solution"></a>Çözümü dağıtma

Bu çözümü üç adımı gerektirir:
1. Ağ altyapısını hazırlayın.
2. StorSimple Cihazınızı yedekleme hedefi olarak dağıtın.
3. VERITAS NetBackup dağıtın.

Her adım, aşağıdaki bölümlerde ayrıntılı olarak ele alınmıştır.

### <a name="set-up-the-network"></a>Ağ kurma

StorSimple Azure Bulutu ile tümleşik bir çözüm olduğundan, StorSimple etkin ve Azure Bulutu çalışma bağlantısı gerektirir. Eski katmanı için daha az verileri Azure bulut depolama alanına ve bu bağlantı bulut anlık görüntüleri, veri yönetimi ve meta veri aktarımı gibi işlemleri için kullanılır.

Çözüm göstermesi için bu ağ en iyi uygulamaları izlemenizi öneririz:

-   Katmanlama StorSimple Azure'a bağlanan bağlantı, bant genişliği gereksinimlerini karşılaması gerekir. Bunu başarmak için doğru hizmet kalitesi (QoS) seviyesi altyapınız için geçerli RPO ve kurtarma eşleşecek şekilde anahtarları süresi hedefi (RTO) SLA'ları.

-   Azure Blob Depolama erişimi gecikmelerini en fazla yaklaşık 80 ms olmalıdır.

### <a name="deploy-storsimple"></a>StorSimple dağıtma

Adım adım StorSimple dağıtım yönergeleri için bkz: [şirket içi StorSimple Cihazınızı dağıtma](storsimple-deployment-walkthrough-u2.md).

### <a name="deploy-netbackup"></a>NetBackup dağıtma

Adım adım NetBackup 7.7.x dağıtım yönergeleri için bkz: [NetBackup 7.7.x belgelerine](http://www.veritas.com/docs/000094423).

## <a name="set-up-the-solution"></a>Çözümü ayarlama

Bu bölümde bazı yapılandırma örnekleri gösterilmektedir. Aşağıdaki örnekler ve önerileri en temel ve temel uygulama gösterilmektedir. Bu uygulama doğrudan belirli yedekleme gereksinimlerinizi için geçerli olmayabilir.

### <a name="set-up-storsimple"></a>StorSimple ayarlayın

| StorSimple dağıtım görevleri  | Ek Açıklamalar |
|---|---|
| Şirket içi StorSimple Cihazınızı dağıtma. | Desteklenen sürümleri: Update 3 ve sonraki sürümleri. |
| Yedekleme hedef açın. | Bu komutlar, açma veya yedekleme hedefi modunu devre dışı bırakmak ve durumunu almak için kullanın. Daha fazla bilgi için bkz: [StorSimple cihazı uzaktan bağlanma](storsimple-remote-connect.md).</br> Yedekleme modunu açmak için: `Set-HCSBackupApplianceMode -enable`. </br> Yedekleme modunu devre dışı bırakmak için: `Set-HCSBackupApplianceMode -disable`. </br> Yedekleme modu ayarları geçerli durumunu almak için: `Get-HCSBackupApplianceMode`. |
| Yedekleme verilerini depolayan biriminiz için ortak bir birim kapsayıcısı oluşturun. Tüm veriler bir birim kapsayıcısı, yinelenenleri kaldırılmış. | StorSimple birim kapsayıcıları yinelenenleri kaldırma etki alanlarını tanımlayın.  |
| StorSimple birim oluşturun. | Birim boyutu süresini anlık görüntü bulut etkilediğinden birimler beklenen kullanım yakın boyutlarıyla mümkün olduğunca oluşturun. Bir birimi boyutu hakkında daha fazla bilgi için bilgiyi [bekletme ilkeleri](#retention-policies).</br> </br> Kullanım StorSimple katmanlı birimler ve seçin **bu birimi daha az sıklıkta erişilen arşiv verileri için kullanın** onay kutusu. </br> Yerel olarak sabitlenmiş birimleri yalnızca kullanılması desteklenmez. |
| Tüm yedekleme hedefi birimleri için benzersiz bir StorSimple yedekleme ilkesi oluşturun. | Bir StorSimple yedekleme İlkesi birim tutarlılık grubu tanımlar. |
| Anlık görüntüleri süresi dolduğundan zamanlama devre dışı bırakın. | Anlık görüntüler işlem sonrası bir işlem olarak tetiklenir. |

### <a name="set-up-the-host-backup-server-storage"></a>Ana bilgisayar yedekleme sunucusu depolama alanı ayarlama

Bu yönergelerine göre konak yedekleme sunucusu depolama ayarlayın:  

- Dağıtılmış birimler (Windows Disk Yönetimi tarafından oluşturulan); kullanmayın Dağıtılmış birimler desteklenmez.
- 64 KB ayırma boyutu NTFS kullanılarak birimlerinizi biçimlendirin.
- StorSimple birimlerini doğrudan NetBackup sunucuya eşleyin.
    - İSCSI fiziksel sunucuları için kullanın.
    - Geçiş disklerini sanal sunucular için kullanın.


## <a name="best-practices-for-storsimple-and-netbackup"></a>StorSimple ve NetBackup için en iyi yöntemler

Aşağıdaki yönergelerine göre çözümünüz ayarlama birkaç bölümler.

### <a name="operating-system-best-practices"></a>İşletim sistemi en iyi uygulamalar

-   Windows Server şifreleme ve yinelenenleri kaldırma için NTFS dosya sistemi devre dışı bırakın.
-   Windows Server birleştirme StorSimple birimlerde devre dışı bırakın.
-   Windows Server StorSimple birimlerde dizin oluşturmayı devre dışı bırakın.
-   Kaynak ana (değil karşı StorSimple birimlerini) bir virüsten koruma taraması çalıştırın.
-   Varsayılan devre dışı bırakma [Windows Server bakım](https://msdn.microsoft.com/library/windows/desktop/hh848037.aspx) Görev Yöneticisi'nde. Bu aşağıdaki yollardan birini yapın:
    - Windows Görev Zamanlayıcısı'ndaki bakım configurator kapatın.
    - Karşıdan [PsExec](https://technet.microsoft.com/sysinternals/bb897553.aspx) Windows SysInternals gelen. PsExec indirdikten sonra Windows PowerShell bir Yöneticiyseniz ve türü çalıştırın:
      ```powershell
      psexec \\%computername% -s schtasks /change /tn “MicrosoftWindowsTaskSchedulerMaintenance Configurator" /disable
      ```

### <a name="storsimple-best-practices"></a>StorSimple en iyi uygulamalar

-   StorSimple cihazı için güncelleştirildiğinden emin olun [güncelleştirme 3 veya sonraki](storsimple-install-update-3.md).
-   Yalıtmaya iSCSI ve bulut trafiği. StorSimple yedekleme sunucusu arasındaki trafiği için ayrılmış iSCSI bağlantıları kullanın.
-   StorSimple Cihazınızı adanmış bir yedekleme hedefi olduğundan emin olun. RTO ve RPO etkilediğinden karma iş yükleri desteklenmez.

### <a name="netbackup-best-practices"></a>NetBackup en iyi uygulamalar

-   NetBackup veritabanı, sunucu için yerel ve StorSimple birim üzerinde bulunan değil.
-   Olağanüstü durum kurtarma için bir StorSimple biriminin NetBackup veritabanını yedekleyin.
-   Bu çözüm için (aynı zamanda, NetBackup artımlı yedekleri denir) NetBackup tam ve artımlı yedeklemeler destekliyoruz. Yapay ve toplu artımlı yedeklemeler kullanmamanızı öneririz.
-   Yedek veri dosyaları yalnızca belirli bir iş verilerini içermelidir. Örneğin, ortam genelinde ekler farklı işleri izin verilir.

Bu gereksinimleri uygulamak için en iyi yöntemler ve en son NetBackup ayarları görmek için NetBackup belgelerine [www.veritas.com](https://www.veritas.com).


## <a name="retention-policies"></a>Elde tutma ilkeleri

En yaygın yedekleme bekletme ilkesi türlerinden birini Dedenizin, öğe ve Son (GFS) bir ilkedir. Bir GFS ilke tam yedekleme haftalık ve aylık yapılır ve artımlı yedekleme günlük gerçekleştirilir. Bu ilke sonuçları altı StorSimple katmanlı birimler: bir birimi içeren haftalık, aylık ve yıllık tam yedekleme; diğer beş birimleri günlük artımlı yedeklemeleri depolar.

Aşağıdaki örnekte, GFS döndürme kullanırız. Aşağıdaki örnekte varsayılır:

-   Olmayan yinelenenleri kaldırılan veya sıkıştırılmış veri kullanılır.
-   Tam yedeklemeler 1 Tıb ' dir.
-   Günlük artımlı yedeklemeler 500 Gib'den ' dir.
-   Dört haftalık yedeklemeler bir ay için tutulur.
-   12 aylık yedeklemeler bir yıl için tutulur.
-   Bir yıllık yedekleme için 10 yıl tutulur.

Önceki varsayımları temel alarak 26 Tıb oluşturma StorSimple katmanlı birim aylık ve yıllık tam yedeklemeler için. 5 Tıb oluşturma StorSimple katmanlı birim her artımlı günlük yedeklemeler için.

| Yedekleme türü bekletme | Boyut (Tıb) | GFS çarpanı\* | Toplam Kapasite (Tıb)  |
|---|---|---|---|
| Haftalık tam | 1 | 4  | 4 |
| Günlük artımlı | 0.5 | 20 (döngüleri eşit hafta sayısı her ay) | (2 ek kota için) 12 |
| Aylık tam | 1 | 12 | 12 |
| Yıllık tam | 1  | 10 | 10 |
| GFS gereksinimi |   | 38 |   |
| Ek kota  | 4  |   | 42 toplam GFS gereksinim  |
\*GFS çarpanı koruma ve yedekleme İlkesi gereksinimlerinizi karşılayacak şekilde korumak için ihtiyacınız kopya sayısıdır.

## <a name="set-up-netbackup-storage"></a>NetBackup depolama alanı ayarlama

### <a name="to-set-up-netbackup-storage"></a>NetBackup depolama alanı ayarlamak için

1.  NetBackup yönetim konsolunda seçin **medya ve Aygıt Yönetimi** > **aygıtları** > **Disk havuzları**. Disk havuzu Yapılandırma Sihirbazı'nda depolama sunucu türünü seçin **AdvancedDisk**ve ardından **sonraki**.

    ![NetBackup yönetim konsolunda, Disk havuzu Yapılandırma Sihirbazı](./media/storsimple-configure-backup-target-using-netbackup/nbimage1.png)

2.  Sunucunuzu seçin ve ardından **sonraki**.

    ![NetBackup yönetim konsolunda, sunucu seçin](./media/storsimple-configure-backup-target-using-netbackup/nbimage2.png)

3.  StorSimple birim seçin.

    ![NetBackup yönetim konsolunda, StorSimple birim disk seçin](./media/storsimple-configure-backup-target-using-netbackup/nbimage3.png)

4.  Yedekleme hedefi için bir ad girin ve ardından **sonraki** > **sonraki** Sihirbazı tamamlamak için.

5.  Ayarları gözden geçirin ve ardından **son**.

6.  Her bir birim atama sonunda bu önerilen eşleştiği için depolama aygıtı ayarlarını değiştirmeniz [en iyi uygulamalar StorSimple ve NetBackup](#best-practices-for-storsimple-and-netbackup).

7. StorSimple birimlerinizi atama tamamlandı olana kadar 1-6 adımlarını yineleyin.

    ![NetBackup yönetim konsolunda, disk yapılandırması](./media/storsimple-configure-backup-target-using-netbackup/nbimage5.png)

## <a name="set-up-storsimple-as-a-primary-backup-target"></a>StorSimple birincil yedekleme hedefi olarak ayarlayın

> [!NOTE]
> Verileri geri yüklemeler buluta katmanlı bir yedekten bulut hızlarda oluşur.

Aşağıdaki şekilde bir yedekleme işi tipik bir birime eşleme gösterilmektedir. Bu durumda, haftalık yedekleri Cumartesi tam disk eşleme ve artımlı yedeklemeler Pazartesi-Cuma artımlı disklere eşleyin. Tüm yedekleme ve geri yüklemeler bir StorSimple biriminin katmanlı.

![Birincil yedekleme hedefi yapılandırma mantıksal diyagramı ](./media/storsimple-configure-backup-target-using-netbackup/primarybackuptargetdiagram.png)

### <a name="storsimple-as-a-primary-backup-target-gfs-schedule-example"></a>StorSimple birincil yedekleme hedefi GFS olarak örnek zamanlama

Aylık ve yıllık dört hafta için GFS döndürme zamanlama bir örneği burada verilmiştir:

| Sıklık/yedekleme türü | Tam | Artımlı (günleri 1-5)  |   
|---|---|---|
| Haftalık (hafta 1-4) | Cumartesi | Pazartesi-Cuma |
| Aylık  | Cumartesi  |   |
| Yıllık | Cumartesi  |   |   |

## <a name="assigning-storsimple-volumes-to-a-netbackup-backup-job"></a>StorSimple birimlerini NetBackup yedekleme işi atama

Aşağıdaki sırada NetBackup ve hedef ana bilgisayarın NetBackup Aracısı yönergelerine uygun olarak yapılandırıldığını varsayar.

### <a name="to-assign-storsimple-volumes-to-a-netbackup-backup-job"></a>StorSimple birimlerini NetBackup yedekleme işi atamak için

1.  NetBackup yönetim konsolunda seçin **NetBackup Yönetim**, sağ **ilkeleri**ve ardından **yeni ilke**.

    ![NetBackup yönetim konsolunda, yeni bir ilke oluşturun](./media/storsimple-configure-backup-target-using-netbackup/nbimage6.png)

2.  İçinde **yeni bir ilke ekleme** iletişim kutusu, ilke için bir ad girin ve ardından **kullanım ilkesi Yapılandırma Sihirbazı'nı** onay kutusu. **Tamam**’ı seçin.

    ![NetBackup yönetim konsolunda, yeni ilke iletişim kutusu ekleme](./media/storsimple-configure-backup-target-using-netbackup/nbimage7.png)

3.  Yedekleme türü ve ardından yedekleme ilkesini Yapılandırma Sihirbazı'nda seçmediğiniz **sonraki**.

    ![NetBackup yönetim konsolunda, yedekleme türünü seçin](./media/storsimple-configure-backup-target-using-netbackup/nbimage8.png)

4.  İlke türünü ayarlamak için seçin **standart**ve ardından **sonraki**.

    ![NetBackup yönetim konsolunda, select ilke türü](./media/storsimple-configure-backup-target-using-netbackup/nbimage9.png)

5.  Ana bilgisayarınız seçin, **istemci işletim sistemini algılar** onay kutusunu işaretleyin ve ardından **Ekle**. Seçin **sonraki**.

    ![NetBackup yönetim konsolunda, yeni bir ilke listesi istemcileri](./media/storsimple-configure-backup-target-using-netbackup/nbimage10.png)

6.  Yedeklemek istediğiniz sürücüleri seçin.

    ![NetBackup yönetim konsolunda, yeni bir ilke için yedekleme seçimleri](./media/storsimple-configure-backup-target-using-netbackup/nbimage11.png)

7.  Rotasyon gereksinimlerinizi karşılayacak sıklığı ve bekletme değerleri seçin.

    ![NetBackup Yönetim Konsolu, yedekleme sıklığı ve yeni bir ilke için bir döndürme](./media/storsimple-configure-backup-target-using-netbackup/nbimage12.png)

8.  Seçin **sonraki** > **sonraki** > **son**.  İlkesi oluşturulduktan sonra zamanlamasını değiştirebilirsiniz.

9.  Oluşturulan yeni İlkesi'ni genişletin ve ardından seçmek için Seç **zamanlamaları**.

    ![NetBackup yönetim konsolunda, yeni bir ilke için zamanlamaları](./media/storsimple-configure-backup-target-using-netbackup/nbimage13.png)

10.  Sağ **fark Inc**seçin **yeni kopyalayın**ve ardından **Tamam**.

    ![NetBackup yönetim konsolunda, yeni bir ilkeye kopyalama zamanlaması](./media/storsimple-configure-backup-target-using-netbackup/nbimage14.png)

11.  Yeni oluşturulan zamanlamayı sağ tıklatın ve ardından **değişiklik**.

12.  Üzerinde **öznitelikleri** sekmesine **geçersiz kılma ilkesi depolama seçimi** onay kutusunu işaretleyin ve ardından Pazartesi artımlı yedeklemeler nereye birim seçin.

    ![NetBackup yönetim konsolunda, zamanlamasını değiştirme](./media/storsimple-configure-backup-target-using-netbackup/nbimage15.png)

13.  Üzerinde **Başlat penceresi** sekmesinde, zaman penceresi yedeklemeleriniz için seçin.

    ![NetBackup yönetim konsolunda, değişiklik başlangıç penceresi](./media/storsimple-configure-backup-target-using-netbackup/nbimage16.png)

14.  **Tamam**’ı seçin.

15.  Adımları 10-14 her artımlı yedekleme için yineleyin. Oluşturduğunuz her yedekleme zamanlamasını ve uygun birimi seçin.

16.  Sağ **fark Inc** zamanlamak ve silin.

17.  Yedekleme gereksinimlerinizi karşılamak için tam zamanlamanızı değiştirin.

    ![NetBackup yönetim konsolunda, tam zamanlamasını değiştirme](./media/storsimple-configure-backup-target-using-netbackup/nbimage17.png)

18.  Başlangıç penceresi değiştirin.

    ![NetBackup yönetim konsolunda, başlangıç penceresi değiştirme](./media/storsimple-configure-backup-target-using-netbackup/nbimage18.png)

19.  Son zamanlamayı şuna benzer:

    ![NetBackup Yönetim Konsolu, son zamanlama](./media/storsimple-configure-backup-target-using-netbackup/nbimage19.png)

## <a name="set-up-storsimple-as-a-secondary-backup-target"></a>StorSimple ikincil bir yedekleme hedefi olarak ayarlayın

> [!NOTE]
>Verileri geri yüklemeler buluta katmanlı bir yedekten bulut hızlarda oluşur.

Bu modelde, geçici bir önbellek olarak hizmet vermek için bir depolama medyasına (dışında StorSimple) olması gerekir. Örneğin, boşluk, giriş/çıkış (g/ç) ve bant genişliği uyum sağlayacak şekilde bağımsız diskler (RAID) birim yedek dizisi kullanabilirsiniz. RAID 5, 50 ve 10 kullanmanızı öneririz.

Aşağıdaki şekil tipik kısa vadeli bekletme (sunucu) yerel birimleri gösterir ve uzun vadeli bekletme birimleri arşivler. Bu senaryoda, tüm yedeklemeler Yerelden (sunucu) RAID birim üzerinde çalıştırın. Bu yedeklemeler düzenli aralıklarla yinelenen ve arşivler birime arşivledi. Kısa vadeli bekletme kapasite ve performans gereksinimlerinizi işleyebilecek şekilde Yerelden (sunucu) RAID biriminiz boyutlandırmak önemlidir.

### <a name="storsimple-as-a-secondary-backup-target-gfs-example"></a>StorSimple ikincil yedekleme hedefi GFS örnek olarak

![StorSimple olarak ikincil yedekleme hedefi mantıksal diyagramı](./media/storsimple-configure-backup-target-using-netbackup/secondarybackuptargetdiagram.png)

Aşağıdaki tabloda, yedekler yerel ve StorSimple diskler üzerinde çalışacak şekilde ayarlanmış gösterilmektedir. Tek tek ve toplam kapasite gereksinimlerini içerir.

### <a name="backup-configuration-and-capacity-requirements"></a>Yedekleme Yapılandırması ve kapasite gereksinimleri

| Yedekleme türü ve bekletme | Yapılandırılan depolama | Boyut (Tıb) | GFS çarpanı | Toplam Kapasite\* (Tıb) |
|---|---|---|---|---|
| Hafta 1 (tam ve artımlı) |Yerel disk (kısa vadeli)| 1 | 1 | 1 |
| StorSimple hafta 2-4 |StorSimple disk (uzun süreli) | 1 | 4 | 4 |
| Aylık tam |StorSimple disk (uzun süreli) | 1 | 12 | 12 |
| Yıllık tam |StorSimple disk (uzun süreli) | 1 | 1 | 1 |
|GFS birim boyutu gereksinimini |  |  |  | 18*|
\*Toplam Kapasite 17 TiB, StorSimple diskleri ve yerel RAID birimi 1 TiB içerir.


### <a name="gfs-example-schedule-gfs-rotation-weekly-monthly-and-yearly-schedule"></a>GFS örnek zamanlama: GFS döndürme haftalık, aylık ve yıllık zamanlama

| Hafta | Tam | Artımlı günü 1 | Artımlı günü 2 | Artımlı Günü 3 | Artımlı günü 4 | Artımlı günü 5 |
|---|---|---|---|---|---|---|
| 1 hafta | Yerel RAID birimi  | Yerel RAID birimi | Yerel RAID birimi | Yerel RAID birimi | Yerel RAID birimi | Yerel RAID birimi |
| 2 hafta | StorSimple hafta 2-4 |   |   |   |   |   |
| Hafta 3 | StorSimple hafta 2-4 |   |   |   |   |   |
| 4 hafta | StorSimple hafta 2-4 |   |   |   |   |   |
| Aylık | StorSimple aylık |   |   |   |   |   |
| Yıllık | StorSimple yıllık  |   |   |   |   |   |   |


## <a name="assign-storsimple-volumes-to-a-netbackup-archive-and-duplication-job"></a>StorSimple birimlerini NetBackup Arşiv ve yineleme iş atayın

NetBackup çok çeşitli seçenekler ortamı ve Depolama Yönetimi için sağladığından, depolama yaşam döngüsü ilkesi (SLP'den) gereksinimlerinizi düzgün bir şekilde değerlendirmek için VERITAS veya NetBackup Mimarı ile başvurun öneririz.

İlk disk havuzları tanımladıktan sonra dört ilkeleri toplam üç ek depolama alanı yaşam döngüsü ilkeleri tanımlamanız gerekir:
* LocalRAIDVolume
* StorSimpleWeek2-4
* StorSimpleMonthlyFulls
* StorSimpleYearlyFulls

### <a name="to-assign-storsimple-volumes-to-a-netbackup-archive-and-duplication-job"></a>StorSimple birimlerini NetBackup Arşiv ve yineleme iş atamak için

1.  NetBackup yönetim konsolunda seçin **depolama** > **depolama yaşam döngüsü ilkeleri** > **yeni depolama yaşam döngüsü ilkesi**.

    ![NetBackup yönetim konsolunda, yeni depolama yaşam döngüsü ilkesi](./media/storsimple-configure-backup-target-using-netbackup/nbimage20.png)

2.  Anlık görüntü için bir ad girin ve ardından **Ekle**.

3.  İçinde **yeni işlem** iletişim kutusundaki **özellikleri** sekmesinde için **işlemi**seçin **yedekleme**. İçin istediğiniz değerleri seçin **hedef depolama**, **bekletme türü**, ve **saklama dönemi**. **Tamam**’ı seçin.

    ![NetBackup yönetim konsolunda, yeni işlem iletişim kutusu](./media/storsimple-configure-backup-target-using-netbackup/nbimage22.png)

    Bu ilk yedekleme işlemi ve depo tanımlar.

4.  Önceki işlem vurgulayın ve ardından seçmek için Seç **Ekle**. İçinde **değişiklik depolama işlemi** iletişim kutusunda, için istediğiniz değerleri seçin **hedef depolama**, **bekletme türü**, ve **saklama dönemi**.

    ![NetBackup yönetim konsolunda, değişiklik depolama işlemi iletişim kutusu](./media/storsimple-configure-backup-target-using-netbackup/nbimage23.png)

5.  Önceki işlem vurgulayın ve ardından seçmek için Seç **Ekle**. İçinde **yeni depolama yaşam döngüsü ilkesi** iletişim kutusunda, bir yıl için aylık yedeklemeler ekleyin.

    ![NetBackup yönetim konsolunda, yeni depolama yaşam döngüsü ilkesi iletişim kutusu](./media/storsimple-configure-backup-target-using-netbackup/nbimage24.png)

6.  Gereksinim duyduğunuz kapsamlı SLP'den bekletme ilkesi oluşturuncaya kadar 4-5 adımlarını tekrarlayın.

    ![NetBackup yönetim konsolunda, yeni depolama yaşam döngüsü ilkesi iletişim kutusunda Ekle'yi ilkeleri](./media/storsimple-configure-backup-target-using-netbackup/nbimage25.png)

7.  Altında SLP'den bekletme ilkesini tanımlama bittiğinde **İlkesi**, ayrıntılı adımları izleyerek bir yedekleme ilkesi tanımlama [atama StorSimple birimlerini NetBackup yedekleme işi için](#assigning-storsimple-volumes-to-a-netbackup-backup-job).

8.  Altında **zamanlamaları**, **zamanlamasını değiştirme** iletişim kutusu, sağ **tam**ve ardından **değişiklik**.

    ![NetBackup yönetim konsolunda, zamanlamayı Değiştir iletişim kutusu](./media/storsimple-configure-backup-target-using-netbackup/nbimage26.png)

9.  Seçin **geçersiz kılma ilkesi depolama seçimi** onay kutusunu işaretleyin ve ardından 1-6. adımda oluşturduğunuz SLP'den bekletme ilkesi seçin.

    ![NetBackup yönetim konsolunda, geçersiz kılma ilkesi depolama seçimi](./media/storsimple-configure-backup-target-using-netbackup/nbimage27.png)

10.  Seçin **Tamam**ve artımlı yedekleme zamanlaması için yineleyin.

    ![NetBackup yönetim konsolunda, artımlı yedeklemeler için zamanlamayı Değiştir iletişim kutusu](./media/storsimple-configure-backup-target-using-netbackup/nbimage28.png)


| Yedekleme türü bekletme | Boyut (Tıb) | GFS çarpanı\* | Toplam Kapasite (Tıb)  |
|---|---|---|---|
| Haftalık tam |  1  |  4 | 4  |
| Günlük artımlı  | 0.5  | 20 (döngüleri eşit aylık hafta sayısı) | (2 ek kota için) 12 |
| Aylık tam  | 1 | 12 | 12 |
| Yıllık tam | 1  | 10 | 10 |
| GFS gereksinimi  |     |     | 38 |
| Ek kota  | 4  |    | 42 toplam GFS gereksinim |
\*GFS çarpanı koruma ve yedekleme İlkesi gereksinimlerinizi karşılayacak şekilde korumak için ihtiyacınız kopya sayısıdır.

## <a name="storsimple-cloud-snapshots"></a>StorSimple bulut anlık görüntüleri

StorSimple bulut anlık görüntüleri, StorSimple Cihazınızı bulunan veriler koruyun. Bir bulut anlık görüntü oluşturmak için bir site dışı tesis yerel yedekleme bantlarını sevkiyat için eşdeğerdir. Azure coğrafi olarak yedekli depolama kullanırsanız, bir bulut anlık görüntüsü oluşturma birden fazla siteye yedekleme bantlarını sevkiyat için eşdeğerdir. Bir aygıt bir olağanüstü durum sonra geri yüklemeniz gerekiyorsa, başka bir StorSimple cihaz çevrimiçi duruma getirin ve bir yük devretme işlemi gerçekleştirin. Yük devretme sonrasında, en son bulut anlık görüntüden verilere (bulut hızlarında) gerçekleştirebilir.

Aşağıdaki bölümde başlatın ve yedekleme sonrası işleme sırasında StorSimple bulut anlık görüntüleri silmek için kısa bir komut dosyasının nasıl oluşturulacağını açıklar.

> [!NOTE]
> El ile veya program aracılığıyla oluşturulan anlık görüntüler, StorSimple anlık görüntü süre sonu ilkesi izlemeyin. Bu anlık görüntüleri el ile veya programlama silinmesi gerekir.

### <a name="start-and-delete-cloud-snapshots-by-using-a-script"></a>Başlatın ve bir komut dosyası kullanarak bulut anlık görüntüleri silin

> [!NOTE]
> StorSimple anlık görüntü silmeden önce uyumluluk ve veri bekletme varsa dikkatle değerlendirin. Bir yedekleme sonrası betik çalıştırma hakkında daha fazla bilgi için bkz: [NetBackup belgelerine](http://www.veritas.com/docs/000094423).

### <a name="backup-lifecycle"></a>Yedekleme yaşam döngüsü

![Yedekleme yaşam döngüsü diyagramı](./media/storsimple-configure-backup-target-using-netbackup/backuplifecycle.png)

### <a name="requirements"></a>Gereksinimler

-   Betik çalıştıran sunucunun Azure bulut kaynaklarına erişimi olması gerekir.
-   Kullanıcı hesabının gerekli izinlere sahip olmalıdır.
-   İlişkili StorSimple birimlerini StorSimple yedekleme ilkesiyle ayarlanan ancak açık değil.
-   Gerekir StorSimple kaynak adı, kayıt anahtarı, cihaz adını ve yedekleme ilke kimliği

### <a name="to-start-or-delete-a-cloud-snapshot"></a>Başlatmak veya Bulut anlık görüntüyü silmek için

1.  [Azure PowerShell'i yükleme](/powershell/azure/overview).
2. Karşıdan yükleme ve Kurulum [Yönet CloudSnapshots.ps1](https://github.com/anoobbacker/storsimpledevicemgmttools/blob/master/Manage-CloudSnapshots.ps1) PowerShell Betiği.
3. PowerShell Betiği çalıştıran sunucuda yönetici olarak çalıştırın. Komut dosyasının çalışmasını sağlamak `-WhatIf $true` ne betik değişiklikler görmek için hale getirir. Doğrulama tamamlandıktan sonra geçirmek `-WhatIf $false`. Çalıştır komutunu aşağıda:
```powershell
.\Manage-CloudSnapshots.ps1 -SubscriptionId [Subscription Id] -TenantId [Tenant ID] -ResourceGroupName [Resource Group Name] -ManagerName [StorSimple Device Manager Name] -DeviceName [device name] -BackupPolicyName [backup policyname] -RetentionInDays [Retention days] -WhatIf [$true or $false]
```
4.  Yedekleme işi, NetBackup betiği ekleyin. Bunu yapmak için önceden işlenmesi ve sonrası Komutlar işleme NetBackup iş seçeneklerinizi düzenleyin.

> [!NOTE]
> Günlük yedekleme işi sonunda işlem sonrası bir komut dosyası olarak StorSimple bulut anlık görüntü yedekleme ilkenizi çalıştırmanızı öneririz. Yedekleme ve geri yükleme RPO ve RTO karşılamanıza yardımcı olmak için yedekleme uygulaması ortamınız hakkında daha fazla bilgi için lütfen ile yedekleme, Mimarı bakın.

## <a name="storsimple-as-a-restore-source"></a>Geri yükleme kaynağı olarak StorSimple

Herhangi bir blok depolama aygıtından geri yüklemeler gibi StorSimple cihazı çalışma alanından geri yükler. Buluta katmanlı veri geri yüklemeler bulut hızlarda oluşur. Yerel veri için cihaz yerel disk hızında geri yüklemeler oluşur. Bir geri yükleme gerçekleştirme hakkında daha fazla bilgi için bkz: [NetBackup belgelerine](http://www.veritas.com/docs/000094423). NetBackup geri yükleme en iyi uygulamalara uygun öneririz.

## <a name="storsimple-failover-and-disaster-recovery"></a>StorSimple yük devretme ve olağanüstü durum kurtarma

> [!NOTE]
> Yedekleme hedefi senaryoları için bir geri yükleme hedefi olarak StorSimple bulut uygulaması desteklenmiyor.

Bir olağanüstü durum çeşitli etkenlere göre neden olabilir. Aşağıdaki tabloda, genel olağanüstü durum kurtarma senaryoları listeler.

| Senaryo | Etkisi | Nasıl kurtarılır | Notlar |
|---|---|---|---|
| StorSimple cihaz hatası | Yedekleme ve geri yükleme işlemleri kesilir. | Başarısız aygıt değiştirin ve gerçekleştirmek [StorSimple yük devretme ve olağanüstü durum kurtarma](storsimple-device-failover-disaster-recovery.md). | Cihaz kurtarma işleminden sonra geri yüklemeyi gerçekleştirmek gerekiyorsa, tam veri çalışma kümeleri için yeni cihaz buluttan alınır. Bulut hızlarda tüm işlemleridir. Dizin ve işlemi yeniden tarama işlemi katalog tararken ve bu da zaman alan bir işlem olabilir yerel aygıt katmanına bulut Katmanı'ndan çekilen tüm yedekleme kümelerini neden olabilir. |
| NetBackup sunucu hatası | Yedekleme ve geri yükleme işlemleri kesilir. | Yedekleme sunucusunu yeniden oluşturmak ve veritabanı geri yükleme gerçekleştirin. | Yeniden oluşturmanız veya olağanüstü durum kurtarma sitesini NetBackup sunucuda geri yükleyin. Veritabanını geri yüklemek için en son noktası. Geri yüklenen NetBackup veritabanı son yedekleme işlerinizi ile eşitlenmiş durumda değilse, dizin oluşturma ve Katalog gereklidir. Bu dizin ve işlemi yeniden tarama işlemi katalog taranan ve bulut Katmanı'ndan yerel aygıt katmanına çekilen tüm yedekleme kümelerini neden olabilir. Bu, daha fazla zaman yoğunluklu kolaylaştırır. |
| Yedekleme sunucusu ve StorSimple kaybı ile sonuçlanır site hatası | Yedekleme ve geri yükleme işlemleri kesilir. | StorSimple önce geri yükleme ve NetBackup geri yükleyin. | StorSimple önce geri yükleme ve NetBackup geri yükleyin. Cihaz kurtarma işleminden sonra geri yüklemeyi gerçekleştirmek gerekiyorsa, tam veri çalışma kümeleri için yeni cihaz buluttan alınır. Bulut hızlarda tüm işlemleridir. |

## <a name="references"></a>Başvurular

Aşağıdaki belgeler için bu makalenin başvurulan:

- [StorSimple çok yollu g/ç Kurulumu](storsimple-configure-mpio-windows-server.md)
- [Depolama senaryoları: ölçülü kaynak sağlama](http://msdn.microsoft.com/library/windows/hardware/dn265487.aspx)
- [GPT kullanarak sürücüler](http://msdn.microsoft.com/windows/hardware/gg463524.aspx#EHD)
- [Paylaşılan klasörler için gölge kopyaları ayarlama](http://technet.microsoft.com/library/cc771893.aspx)

## <a name="next-steps"></a>Sonraki adımlar

- Nasıl yapılır hakkında daha fazla bilgi [bir yedekleme kümesi geri](storsimple-restore-from-backup-set-u2.md).
- Nasıl yapılacağı hakkında daha fazla bilgi [aygıt yük devretme ve olağanüstü durum kurtarma](storsimple-device-failover-disaster-recovery.md).
