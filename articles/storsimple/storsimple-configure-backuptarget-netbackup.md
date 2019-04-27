---
title: Yedekleme hedefi NetBackup ile StorSimple 8000 serisi | Microsoft Docs
description: Veritas NetBackup ile StorSimple yedekleme hedefi yapılandırmayı açıklar.
services: storsimple
documentationcenter: ''
author: harshakirank
manager: matd
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/15/2017
ms.author: hkanna
ms.openlocfilehash: 17428405a0be45854a2eaaef831864f529ed145a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60725371"
---
# <a name="storsimple-as-a-backup-target-with-netbackup"></a>Yedekleme hedefi olarak StorSimple NetBackup ile

## <a name="overview"></a>Genel Bakış

Azure StorSimple bir Microsoft karma bulut depolama çözümüdür. StorSimple şirket içi çözümün bir uzantısı olarak Azure depolama hesabı kullanarak ve şirket içi depolama ve bulut depolama alanı üzerinden veri otomatik katmanlama üstel veri büyümesi karmaşıklığını ele alır.

Bu makalede, StorSimple tümleştirmesiyle Veritas NetBackup ve her iki çözüm de tümleştirmeye yönelik en iyi uygulamalar ele alır. Biz de en iyi StorSimple ile tümleştirmek için Veritas NetBackup konusunda önerilerde. Veritas en iyi yöntemleri, yedekleme mimarlar ve Yöneticiler için ayrı ayrı yedekleme gereksinimlerini ve hizmet düzeyi sözleşmelerine (SLA) karşılamak üzere Veritas NetBackup ayarlamak en iyi yolu erteleyin.

Biz yapılandırma adımları ve temel kavramları göstermek olsa da, bu makalede göre Hayır olmadığı bir adım adım yapılandırma veya yükleme kılavuzudur. Temel bileşenlere ve altyapılara çalışma sırası ve açıklanmaktadır kavramları desteklemeye hazır olduğunu varsayıyoruz.

### <a name="who-should-read-this"></a>Bu kimler içindir?

Bu makaledeki bilgiler, yedekleme yöneticilerinin, depolama yöneticilerinin ve depolama, Windows Server 2012 R2, Ethernet, bulut Hizmetleri ve Veritas NetBackup bilgisine sahip depolama mimarları için en faydalı olacaktır.

### <a name="supported-versions"></a>Desteklenen sürümler

-   NetBackup 7.7.x ve sonraki sürümler
-   [StorSimple güncelleştirme 3 ve sonraki sürümler](storsimple-overview.md#storsimple-workload-summary)


## <a name="why-storsimple-as-a-backup-target"></a>Neden bir yedekleme hedefi olarak StorSimple?

StorSimple yedekleme hedefi için iyi bir seçimdir çünkü:

-   Bu, herhangi bir değişiklik yapmadan bir hızlı yedekleme hedefi olarak kullanmak üzere yedekleme uygulamaları için standart, yerel depolama sağlar. StorSimple, son yedeklemelerin hızlı geri yükleme için de kullanabilirsiniz.
-   Bulut katmanlaması Hesaplı Azure depolama kullanmak için bir Azure bulut depolama hesabıyla sorunsuz bir şekilde tümleşiktir.
-   Otomatik olarak site dışında depolama için olağanüstü durum kurtarma sağlar.

## <a name="key-concepts"></a>Önemli kavramlar

Bir depolama çözümü olarak, çözümün depolama performansını SLA'lar, dikkatli bir değerlendirme ile değiştirin ve kapasite büyüme ihtiyacını başarısı için kritik hızıdır. Buradaki ana fikir, erişim zamanları ve aktarım hızı bulut play kullanılabilmesi için StorSimple yeteneklerini temel bir rol, bir bulut katmanı sunarak olmasıdır.

StorSimple, verilerin (sık erişimli veriler) iyi tanımlanmış çalışma kümesinde çalışan uygulamalar için depolama sağlamak için tasarlanmıştır. Bu modelde, veri çalışma kümesini yerel katmanlarda depolanır ve kalan çalışma dışı/soğuk/arşivlenen veri kümesi, buluta katmanlı. Bu model aşağıdaki şekilde temsil edilir. Neredeyse Düz yeşil çizginin StorSimple cihaz yerel katmanlarda üzerinde depolanan verileri temsil eder. Kırmızı çizgi, tüm katmanlarda StorSimple çözümünde depolanan verilerin toplam miktarı temsil eder. Düz yeşil çizginin üssel kırmızı bir eğri arasındaki boşluk, bulutta depolanan verilerin toplam miktarı temsil eder.

**StorSimple katmanlama**
![StorSimple katmanlama diyagramı](./media/storsimple-configure-backup-target-using-netbackup/image1.jpg)

Bu mimari ile StorSimple yedekleme hedefi çalışmak için idealdir olduğunu bulabilirsiniz. StorSimple için kullanabilirsiniz:
-   En sık rastlanan yüklemeleriniz, verileri yerel çalışma kümesinden gerçekleştirin.
-   Bulut, geri yükleme işlemleri daha az sıklıkta olduğu site dışı olağanüstü durum kurtarma ve daha eski verileri için kullanın.

## <a name="storsimple-benefits"></a>StorSimple avantajları

StorSimple, şirket içi sorunsuz erişim avantajlarından yararlanarak Microsoft Azure ile sorunsuz bir şekilde tümleştirilmiş bir şirket içi çözümü sağlar ve bulut depolama.

StorSimple, otomatik katmanlama (SSD) katı hal cihaz ve seri ekli SCSI (SAS) depolama olan şirket içi cihaz ve Azure depolama kullanır. Otomatik katmanlama sık erişilen verileri SSD ve SAS katmanlarda yerel tutar. Bu, nadiren erişilen veriler Azure depolama alanına taşınır.

StorSimple, bu avantajlar sunar:

-   Eşi görülmemiş bir yinelenen verileri kaldırma seviyelerine ulaşmasını sağlamak için bulut kullanan benzersiz yinelenenleri kaldırma ve sıkıştırma algoritmaları
-   Yüksek kullanılabilirlik
-   Azure coğrafi çoğaltma kullanarak coğrafi çoğaltma
-   Azure tümleştirme
-   Bulutta veri şifreleme
-   Geliştirilmiş olağanüstü durum kurtarma ve uyumluluk

StorSimple temelde iki ana dağıtım senaryoları (birincil yedekleme hedefi ve ikincil yedekleme hedefi) gösterir, ama düz, blok depolama cihazı vardır. StorSimple sıkıştırma yapar ve yinelenenleri kaldırma. Sorunsuz bir şekilde gönderir ve bulut uygulama ve dosya sistemi arasındaki verileri alır.

StorSimple hakkında daha fazla bilgi için bkz: [StorSimple 8000 serisi: Hibrit bulut depolaması çözümü](storsimple-overview.md). Ayrıca, gözden geçirebilirsiniz [teknik StorSimple 8000 serisi özellikleri](storsimple-technical-specifications-and-compliance.md).

> [!IMPORTANT]
> Bir StorSimple kullanarak cihaz yedekleme hedefi olarak yalnızca StorSimple 8000 güncelleştirme 3 ve sonraki sürümlerinde desteklenir.

## <a name="architecture-overview"></a>Mimariye genel bakış

Aşağıdaki tablolar, cihaz modeli mimarisi başlangıç düzeyi bir kılavuz gösterir.

**Yerel için StorSimple kapasiteler ve bulut depolama**

| Depolama kapasitesi       | 8100          | 8600            |
|------------------------|---------------|-----------------|
| Yerel depolama kapasitesi | &lt; 10 TiB\*  | &lt; 20 TiB\*  |
| Bulut depolama kapasitesi | &gt; 200 TiB\* | &gt; 500 TiB\* |

\* Depolama boyutu, yinelenenleri kaldırma ya da sıkıştırma varsayar.

**Birincil ve ikincil yedeklemeleri için StorSimple kapasiteler**

| Yedekleme senaryosu  | Yerel depolama kapasitesi  | Bulut depolama kapasitesi  |
|---|---|---|
| Birincil yedekleme  | Kurtarma noktası hedefi (RPO) karşılayacak şekilde son yedeklemelerin Hızlı Kurtarma için yerel depolamada depolanan | Yedekleme geçmişi (RPO) en uygun bulut kapasite |
| İkincil yedekleme | İkincil kopya yedekleme verilerinin bulut kapasite depolanabilir.  | Yok  |

## <a name="storsimple-as-a-primary-backup-target"></a>Birincil yedekleme hedefi olarak StorSimple

Bu senaryoda, StorSimple birimlerini yedekleme uygulamasına yedeklemeler için tek depo olarak sunulur. Aşağıdaki şekil içinde tüm yedeklemeler kullanım StorSimple katmanlı birimleri yedekleme ve geri yüklemeler için bir çözüm mimarisini göstermektedir.

![Mantıksal Diyagram birincil yedekleme hedefi olarak StorSimple](./media/storsimple-configure-backup-target-using-netbackup/primarybackuptargetlogicaldiagram.png)

### <a name="primary-target-backup-logical-steps"></a>Birincil hedef yedekleme mantıksal adımlara

1.  Hedef Yedekleme aracısı yedekleme sunucusu ile iletişim kurar ve yedekleme aracısını veri yedekleme sunucusuna iletir.
2.  StorSimple için verileri yedekleme sunucusuna Yazar katmanlı birimler.
3.  Yedekleme sunucusuna katalog veritabanını güncelleştirir ve ardından yedekleme işi tamamlandıktan.
4.  Anlık görüntü betik StorSimple anlık görüntü Yöneticisi (başlatma veya silme) tetiklenir.
5.  Yedekleme sunucusuna bir bekletme ilkesi temel alınarak süresi dolmuş yedeklemeleri siler.

### <a name="primary-target-restore-logical-steps"></a>Birincil hedef geri yükleme mantıksal adımlara

1.  Yedek sunucu uygun verileri depolama depodan geri başlatır.
2.  Yedekleme aracısı yedekleme sunucusundan verileri alır.
3.  Yedekleme sunucusuna geri yükleme işi tamamlar.

## <a name="storsimple-as-a-secondary-backup-target"></a>İkincil yedekleme hedefi olarak StorSimple

Bu senaryoda, StorSimple birimlerini öncelikli olarak uzun süreli saklama veya arşivleme için kullanılır.

Aşağıdaki şekil, ilk hangi yedeklemelerde bir mimari gösterilir ve yüksek performanslı hedef birim geri yükler. Bu yedeklemeler kopyalanır ve arşivlenmiş bir StorSimple için katmanlı birim üzerinde ayarlanmış bir planlamada.

Bekletme İlkesi kapasite ve performans gereksinimlerinizi işleyebilmeniz yüksek performanslı toplu boyutlandırmak önemlidir.

![Mantıksal Diyagram ikincil yedekleme hedefi olarak StorSimple](./media/storsimple-configure-backup-target-using-netbackup/secondarybackuptargetlogicaldiagram.png)

### <a name="secondary-target-backup-logical-steps"></a>İkincil hedef yedekleme mantıksal adımlara

1.  Hedef Yedekleme aracısı yedekleme sunucusu ile iletişim kurar ve yedekleme aracısını veri yedekleme sunucusuna iletir.
2.  Backup sunucusu, yüksek performanslı depolama alanına verileri yazar.
3.  Yedekleme sunucusuna katalog veritabanını güncelleştirir ve ardından yedekleme işi tamamlandıktan.
4.  Backup sunucusu yedekleme bekletme ilkesi temel alınarak StorSimple kopyalar.
5.  Anlık görüntü betik StorSimple anlık görüntü Yöneticisi (başlatma veya silme) tetiklenir.
6.  Yedekleme sunucusuna bir bekletme ilkesi temel alınarak süresi dolmuş yedeklemeler siler.

### <a name="secondary-target-restore-logical-steps"></a>İkincil hedef geri yükleme mantıksal adımlara

1.  Yedek sunucu uygun verileri depolama depodan geri başlatır.
2.  Yedekleme aracısı yedekleme sunucusundan verileri alır.
3.  Yedekleme sunucusuna geri yükleme işi tamamlar.

## <a name="deploy-the-solution"></a>Çözümü dağıtma

Bu çözümü dağıtmak için üç adımı gerektirir:
1. Ağ altyapısını hazırlayın.
2. Bir yedekleme hedefi olarak StorSimple Cihazınızı dağıtma.
3. Veritas NetBackup dağıtın.

Her adım, aşağıdaki bölümlerde ayrıntılı olarak ele alınmıştır.

### <a name="set-up-the-network"></a>Ağı ayarlama

StorSimple, StorSimple Azure bulutla tümleşik bir çözüm olduğundan, bir etkin ve Azure bulut çalışan bağlantısı gerektirir. Katmanı için eski, daha az erişilen verileri Azure bulut depolama ve bu bağlantı, bulut anlık görüntüleri, veri yönetimi ve meta veri aktarımı gibi işlemleri için kullanılır.

Çözüm göstermesi bu ağ en iyi uygulamaları izlemenizi öneririz:

-   Katmanlama StorSimple Azure'a bağlanan bağlantı, bant genişliği gereksinimlerini karşılaması gerekir. Bunu başarmak için altyapınız için doğru hizmet kalitesi (QoS) düzeyi geçerli RPO ve kurtarma eşleştirilecek anahtarları süresi hedefi (RTO) SLA'lar.

-   Maksimum Azure Blob Depolama erişim gecikmeleri yaklaşık 80 ms olmalıdır.

### <a name="deploy-storsimple"></a>StorSimple'ı dağıtma

Adım adım StorSimple dağıtım yönergeleri için bkz. [şirket içi StorSimple Cihazınızı dağıtma](storsimple-deployment-walkthrough-u2.md).

### <a name="deploy-netbackup"></a>NetBackup dağıtma

Adım adım NetBackup 7.7.x dağıtım yönergeleri için bkz. [NetBackup 7.7.x belgeleri](http://www.veritas.com/docs/000094423).

## <a name="set-up-the-solution"></a>Çözümü ayarlama

Bu bölümde, bazı yapılandırma örneği gösterilmektedir. Aşağıdaki örnekler ve öneriler, en temel ve temel uygulama gösterilmektedir. Bu uygulama, doğrudan belirli yedekleme gereksinimlerinizi için geçerli olmayabilir.

### <a name="set-up-storsimple"></a>StorSimple ' ayarlayın

| StorSimple dağıtım görevleri  | Ek açıklamalar |
|---|---|
| Şirket içi StorSimple Cihazınızı dağıtın. | Desteklenen sürümler: Güncelleştirme 3 ve sonraki sürümler. |
| Yedekleme hedefi üzerinde açın. | Yedekleme hedefi modunu devre dışı bırakmak veya etkinleştirmek ve durumu almak için şu komutları kullanın. Daha fazla bilgi için [bir StorSimple cihazı uzaktan bağlanma](storsimple-remote-connect.md).</br> Yedekleme modunu açmak için: `Set-HCSBackupApplianceMode -enable`. </br> Yedekleme modunu devre dışı bırakmak için: `Set-HCSBackupApplianceMode -disable`. </br> Yedekleme modu ayarları geçerli durumunu almak için: `Get-HCSBackupApplianceMode`. |
| Yedekleme verilerini depolayan biriminiz için ortak bir birim kapsayıcısı oluşturun. Bir birim kapsayıcısındaki tüm veriler yinelenen verileri kaldırma işlemi. | StorSimple birim kapsayıcıları, yinelenenleri kaldırma etki alanlarını tanımlayın.  |
| StorSimple birimler oluşturun. | Birim boyutu bulut anlık görüntü süresini etkilediğinden birimler öngörülen kullanımınıza yakın boyutlarıyla mümkün olduğunca oluşturun. Bir birimi boyutu hakkında daha fazla bilgi için okuyun [bekletme ilkeleri](#retention-policies).</br> </br> StorSimple kullanın, katmanlı birimleri ve seçin **bu birimi daha az sıklıkta erişilen arşiv verileri için kullanın** onay kutusu. </br> Yerel olarak sabitlenmiş birimler yalnızca kullanılması desteklenmiyor. |
| Tüm yedekleme hedefi birimler için benzersiz bir StorSimple yedekleme ilkesi oluşturun. | Bir StorSimple yedekleme ilkesine birim tutarlılık grubu tanımlar. |
| Anlık görüntülerin süresi dolduğundan zamanlama devre dışı bırakın. | Anlık bir işlem sonrası bir işlem olarak tetiklenir. |

### <a name="set-up-the-host-backup-server-storage"></a>Konak sunucu yedekleme depolama alanı ayarlama

Bu yönergelere göre konak yedek sunucu depolama ayarlayın:  

- Dağıtılmış birimler (Windows Disk Management tarafından oluşturulan); kullanmayın Dağıtılmış birimler desteklenmez.
- Boyutu 64 KB ayırma NTFS kullanılarak, birimleri biçimlendirin.
- StorSimple birimlerini NetBackup sunucunun doğrudan eşleyin.
    - Fiziksel sunucuları için iSCSI kullanın.
    - Geçiş disklerini sanal sunucular için kullanın.


## <a name="best-practices-for-storsimple-and-netbackup"></a>StorSimple ve NetBackup için en iyi uygulamalar

Çözümünüzü aşağıdaki yönergelere göre ayarlama birkaç bölümler.

### <a name="operating-system-best-practices"></a>İşletim sistemi en iyi uygulamalar

- Windows Server şifreleme ve yinelenenleri kaldırma NTFS dosya sistemi devre dışı bırakın.
- Windows Server birleştirme StorSimple birimlerde devre dışı bırakın.
- Windows Server StorSimple birimlerde dizin oluşturmayı devre dışı bırakın.
- Kaynak ana bilgisayar (değil karşı StorSimple birimlerini) bir virüsten koruma taraması çalıştırın.
- Varsayılan devre dışı kapatma [Windows Server bakım](https://msdn.microsoft.com/library/windows/desktop/hh848037.aspx) Görev Yöneticisi'nde. Bu aşağıdaki yollardan birini yapın:
  - Windows Görev Zamanlayıcısı'nda bakım configurator devre dışı bırakın.
  - İndirme [PsExec](https://technet.microsoft.com/sysinternals/bb897553.aspx) Windows SysInternals öğesinden. PsExec indirdikten sonra Windows PowerShell bir Yöneticiyseniz ve türü çalıştırın:
    ```powershell
    psexec \\%computername% -s schtasks /change /tn “MicrosoftWindowsTaskSchedulerMaintenance Configurator" /disable
    ```

### <a name="storsimple-best-practices"></a>StorSimple en iyi uygulamalar

-   StorSimple cihazı için güncelleştirildiğinden emin olun [güncelleştirme 3 veya üzeri](storsimple-install-update-3.md).
-   Yalıtım iSCSI ve bulut trafiği. StorSimple ve backup sunucusu arasındaki trafiği adanmış iSCSI bağlantıları kullanın.
-   StorSimple Cihazınızı adanmış bir yedekleme hedefi olduğundan emin olun. RTO ve RPO etkilediğinden, karma iş yükleri desteklenmez.

### <a name="netbackup-best-practices"></a>NetBackup en iyi uygulamalar

-   NetBackup veritabanı, yerel sunucuya ve bir StorSimple biriminde değil.
-   Olağanüstü durum kurtarma için bir StorSimple birimde NetBackup veritabanını yedekleyin.
-   Bu çözüm için (aynı zamanda artımlı yedekleri, NetBackup olarak adlandırılır) NetBackup tam ve artımlı yedeklemeler destekliyoruz. Yapay ve toplu artımlı yedeklemeler kullanmamanızı öneririz.
-   Yedek veri dosyaları, yalnızca belirli bir işin verileri içermelidir. Örneğin, arasında medya ekler farklı işleri izin verilir.

Bu gereksinimleri uygulamak için en iyi ve en son NetBackup ayarları görmek için NetBackup belgelerine [www.veritas.com](https://www.veritas.com).


## <a name="retention-policies"></a>Elde tutma ilkeleri

Yaygın yedekleme bekletme ilkesi türlerinden birini Dedenizin Baba ve Son (GFS) bir ilkedir. GFS ilkesinde artımlı yedekleme günlük gerçekleştirilir ve tam yedekleme haftalık ve aylık olarak gerçekleştirilir. Bu ilke sonuçlarda altı StorSimple katmanlı birimler: bir birimin içerdiğini haftalık, aylık ve yıllık tam yedekleme; diğer beş birimleri günlük artımlı yedeklemeleri depolar.

Aşağıdaki örnekte, bir GFS döndürme kullanırız. Aşağıdaki örnekte varsayılır:

-   Yinelenenleri kaldırılan olmayan veya sıkıştırılmış veriler kullanılır.
-   Tam yedeklemeler her biri 1 TiB ' dir.
-   Günlük artımlı yedeklemeleri her biri 500 GiB ' dir.
-   Dört haftalık yedeklemeler için bir ay tutulur.
-   On iki aylık yedeklemeler, bir yıl boyunca tutulur.
-   Bir yıllık yedekleme için 10 yıl tutulur.

Önceki varsayımları temel alarak 26 TiB oluşturma StorSimple aylık ve yıllık tam yedekleme için birim katmanlı. 5 TiB oluşturma StorSimple katmanlı birim her artımlı günlük yedeklemeleri için.

| Yedekleme türü bekletme | Boyut (TiB) | GFS çarpanı\* | Toplam Kapasite (TiB)  |
|---|---|---|---|
| Haftalık tam | 1 | 4  | 4 |
| Günlük artımlı | 0,5 | 20 (döngüleri eşit sayıda hafta / ay) | 12 (2 ek kota için) |
| Aylık tam | 1 | 12 | 12 |
| Yıllık tam | 1  | 10 | 10 |
| GFS gereksinimi |   | 38 |   |
| Ek kota  | 4  |   | 42 toplam GFS gereksinimi  |

\* GFS çarpan koruma ve yedekleme İlkesi gereksinimlerinizi karşılayacak şekilde korumak için ihtiyaç duyduğunuz kopya sayısıdır.

## <a name="set-up-netbackup-storage"></a>NetBackup depolamayı ayarlama

### <a name="to-set-up-netbackup-storage"></a>NetBackup depolama alanı ayarlama için

1.  NetBackup yönetim konsolunda seçin **medya ve cihaz Yönetimi** > **cihazları** > **Disk havuzları**. Disk havuzu Yapılandırma Sihirbazı'nda depolama sunucu türü seçin **AdvancedDisk**ve ardından **sonraki**.

    ![NetBackup yönetim konsolunda, Disk havuzu Yapılandırma Sihirbazı](./media/storsimple-configure-backup-target-using-netbackup/nbimage1.png)

2.  Sunucunuzu seçin ve ardından **sonraki**.

    ![NetBackup Yönetim Konsolu sunucusunu seçin](./media/storsimple-configure-backup-target-using-netbackup/nbimage2.png)

3.  StorSimple toplu seçin.

    ![NetBackup yönetim konsolunda, StorSimple birim diskini seçin](./media/storsimple-configure-backup-target-using-netbackup/nbimage3.png)

4.  Yedekleme hedefi için bir ad girin ve ardından **sonraki** > **sonraki** Sihirbazı tamamlamak için.

5.  Ayarları gözden geçirin ve ardından **son**.

6.  Her birim atama sonunda, bu önerilen ile eşleşen depolama cihaz ayarlarını değiştirmek [en iyi uygulamalar için StorSimple ve NetBackup](#best-practices-for-storsimple-and-netbackup).

7. StorSimple birimlerinizi atama tamamlandı olana kadar 1-6. adımları tekrarlayın.

    ![NetBackup yönetim konsolunda, disk yapılandırması](./media/storsimple-configure-backup-target-using-netbackup/nbimage5.png)

## <a name="set-up-storsimple-as-a-primary-backup-target"></a>Birincil bir yedekleme hedefi olarak StorSimple ayarlama

> [!NOTE]
> Bulut hızlarında buluta katmanlanmış bir yedekten verileri geri yüklemeler oluşur.

Aşağıdaki şekilde, bir yedekleme işi için tipik bir birimin eşlemeyi gösterir. Bu durumda, haftalık yedekleri Cumartesi tam disk eşleyin ve artımlı yedeklemeler Pazartesi-Cuma artımlı disklere eşleyin. Tüm yedekleme ve geri yüklemeler Storsimple'dan verileri, birim katmanlı.

![Birincil yedekleme hedefi yapılandırma mantıksal diyagramı](./media/storsimple-configure-backup-target-using-netbackup/primarybackuptargetdiagram.png)

### <a name="storsimple-as-a-primary-backup-target-gfs-schedule-example"></a>Birincil GFS yedekleme hedefi olarak StorSimple örnek zamanlama

Dört hafta, aylık ve yıllık GFS döndürme tablosunun bir örnek aşağıda verilmiştir:

| Sıklığı/yedekleme türü | Tam | Artımlı (gün 1-5)  |   
|---|---|---|
| Haftalık (hafta 1-4) | Cumartesi | Pazartesi-Cuma |
| Aylık  | Cumartesi  |   |
| Yıllık | Cumartesi  |   |

## <a name="assigning-storsimple-volumes-to-a-netbackup-backup-job"></a>StorSimple birimlerini NetBackup yedekleme işi için atama

Aşağıdaki sırayı NetBackup aracı yönergelerine uygun olarak NetBackup ve hedef ana bilgisayarın yapılandırıldığını varsayar.

### <a name="to-assign-storsimple-volumes-to-a-netbackup-backup-job"></a>NetBackup yedekleme işi için StorSimple birim atamak için

1. NetBackup yönetim konsolunda seçin **NetBackup Yönetim**, sağ **ilkeleri**ve ardından **yeni ilke**.

   ![NetBackup yönetim konsolunda, yeni bir ilke oluşturun](./media/storsimple-configure-backup-target-using-netbackup/nbimage6.png)

2. İçinde **yeni ilke Ekle** iletişim kutusu, ilke için bir ad girin ve ardından **kullanım ilkesi Yapılandırma Sihirbazı'nı** onay kutusu. **Tamam**’ı seçin.

   ![NetBackup yönetim konsolunda, yeni ilke iletişim kutusu Ekle](./media/storsimple-configure-backup-target-using-netbackup/nbimage7.png)

3. Yedekleme türü ve ardından yedekleme ilkesini Yapılandırma Sihirbazı'nda seçmediğiniz **sonraki**.

   ![NetBackup yönetim konsolunda, select yedekleme türü](./media/storsimple-configure-backup-target-using-netbackup/nbimage8.png)

4. İlke türü ayarlamak için seçin **standart**ve ardından **sonraki**.

   ![NetBackup yönetim konsolunda, select ilke türü](./media/storsimple-configure-backup-target-using-netbackup/nbimage9.png)

5. Konağınız seçin, **istemci işletim sistemini algılar** onay kutusunu işaretleyin ve ardından **Ekle**. **İleri**’yi seçin.

   ![NetBackup yönetim konsolunda, yeni bir ilke listesi istemcileri](./media/storsimple-configure-backup-target-using-netbackup/nbimage10.png)

6. Yedeklemek istediğiniz sürücüleri seçin.

   ![NetBackup yönetim konsolunda, yeni bir ilke için yedekleme seçimleri](./media/storsimple-configure-backup-target-using-netbackup/nbimage11.png)

7. Dönüşümlü yedekleme gereksinimlerinizi karşılayacak sıklığı ve bekletme değerleri seçin.

   ![NetBackup yönetim konsolunda, yedekleme sıklığı ve döndürme için yeni bir ilke](./media/storsimple-configure-backup-target-using-netbackup/nbimage12.png)

8. Seçin **sonraki** > **sonraki** > **son**.  İlke oluşturulduktan sonra zamanlamasını değiştirebilirsiniz.

9. Oluşturulan yeni ilke genişletin ve ardından seçmek için Seç **zamanlamaları**.

   ![NetBackup yönetim konsolunda, yeni bir ilke için zamanlamaları](./media/storsimple-configure-backup-target-using-netbackup/nbimage13.png)

10. Sağ **fark dahil edilen**seçin **yeni kopyalayın**ve ardından **Tamam**.

    ![NetBackup yönetim konsolunda, Yeni ilkeye kopyalama zamanlaması](./media/storsimple-configure-backup-target-using-netbackup/nbimage14.png)

11. Yeni oluşturulan zamanlama sağ tıklayın ve ardından **değişiklik**.

12. Üzerinde **öznitelikleri** sekmesinde **ilkesi depolama seçimi geçersiz kılma** onay kutusunu işaretleyin ve ardından Pazartesi artımlı yedeklemeler nereye birimi seçin.

    ![NetBackup Yönetim Konsolu'nda zamanlamasını değiştirme](./media/storsimple-configure-backup-target-using-netbackup/nbimage15.png)

13. Üzerinde **başlangıç penceresi** sekmesinde, zaman penceresi yedeklemeleriniz için seçin.

    ![NetBackup yönetim konsolunda, değişiklik başlangıç penceresi](./media/storsimple-configure-backup-target-using-netbackup/nbimage16.png)

14. **Tamam**’ı seçin.

15. İçin her bir artımlı yedekleme 10-14 adımları yineleyin. Oluşturduğunuz her bir yedekleme zamanlaması ve uygun birimi seçin.

16. Sağ **fark dahil edilen** zamanlayın ve silin.

17. Yedekleme gereksinimlerinizi karşılamak için tam zamanlamanızı değiştirin.

    ![NetBackup yönetim konsolunda, tam zamanlamasını değiştirme](./media/storsimple-configure-backup-target-using-netbackup/nbimage17.png)

18. Başlangıç penceresi değiştirin.

    ![NetBackup Yönetim Konsolu'nda değiştirme başlangıç penceresi](./media/storsimple-configure-backup-target-using-netbackup/nbimage18.png)

19. Son zamanlama şöyle görünür:

    ![NetBackup Yönetim Konsolu, son zamanlama](./media/storsimple-configure-backup-target-using-netbackup/nbimage19.png)

## <a name="set-up-storsimple-as-a-secondary-backup-target"></a>İkincil bir yedekleme hedefi olarak StorSimple ayarlama

> [!NOTE]
>Bulut hızlarında buluta katmanlanmış bir yedekten verileri geri yüklemeler oluşur.

Bu modelde, geçici bir önbellek olarak görev yapacak bir depolama medyasına (dışında StorSimple) olmalıdır. Örneğin, boşluk, giriş/çıkış (g/ç) ve bant genişliği uyum sağlamak için yedek birim bağımsız diskler (RAID) dizisi kullanabilirsiniz. RAID 5, 50 ve 10 kullanmanızı öneririz.

Aşağıdaki şekilde normal kısa vadeli bekletme yerel (sunucu) birimleri gösterir ve uzun süreli saklama birimleri arşivler. Bu senaryoda, tüm yedeklemeler (sunucu) yerel RAID birimine çalıştırın. Bu yedeklemeler düzenli aralıklarla yinelenen ve bir arşivleri birimine arşivlenir. Kısa vadeli bekletme kapasite ve performans gereksinimlerinizi işleyebilmeniz yerel (sunucu) RAID toplu boyutlandırmak önemlidir.

### <a name="storsimple-as-a-secondary-backup-target-gfs-example"></a>İkincil yedekleme hedefi GFS örnek olarak StorSimple

![Mantıksal Diyagram ikincil yedekleme hedefi olarak StorSimple](./media/storsimple-configure-backup-target-using-netbackup/secondarybackuptargetdiagram.png)

Aşağıdaki tabloda, yedekler yerel ve StorSimple diskler üzerinde çalıştırmak için ayarlama işlemi gösterilmektedir. Bu, tek tek ve toplam kapasite gereksinimlerini içerir.

### <a name="backup-configuration-and-capacity-requirements"></a>Yedekleme Yapılandırması ve kapasite gereksinimleri

| Yedekleme türü ve saklama | Yapılandırılmış depolama | Boyut (TiB) | GFS çarpanı | Toplam Kapasite\* (TiB) |
|---|---|---|---|---|
| Hafta 1 (tam ve artımlı) |Yerel disk (kısa vadeli)| 1 | 1. | 1 |
| StorSimple hafta 2-4 |StorSimple disk (uzun süreli) | 1 | 4 | 4 |
| Aylık tam |StorSimple disk (uzun süreli) | 1 | 12 | 12 |
| Yıllık tam |StorSimple disk (uzun süreli) | 1 | 1. | 1 |
|GFS birim boyutu gereksinimini |  |  |  | 18*|

\* Toplam Kapasite 17 TiB StorSimple'nın, diskler ve yerel RAID birimi 1 TiB içerir.


### <a name="gfs-example-schedule-gfs-rotation-weekly-monthly-and-yearly-schedule"></a>GFS örnek zamanlaması: GFS döndürme haftalık, aylık ve yıllık zamanlama

| Hafta | Tam | Artımlı günlük 1 | Artımlı günlük 2 | Artımlı günlük 3 | Artımlı günlük 4 | Artımlı günlük 5 |
|---|---|---|---|---|---|---|
| 1 hafta | Yerel RAID birimi  | Yerel RAID birimi | Yerel RAID birimi | Yerel RAID birimi | Yerel RAID birimi | Yerel RAID birimi |
| 2 hafta | StorSimple hafta 2-4 |   |   |   |   |   |
| 3 hafta | StorSimple hafta 2-4 |   |   |   |   |   |
| 4 hafta | StorSimple hafta 2-4 |   |   |   |   |   |
| Aylık | StorSimple aylık |   |   |   |   |   |
| Yıllık | Yıllık StorSimple  |   |   |   |   |   |


## <a name="assign-storsimple-volumes-to-a-netbackup-archive-and-duplication-job"></a>StorSimple birim atamak NetBackup Arşiv ve çoğaltma işi

Çok çeşitli depolama ve medya yönetimi için seçenekleri NetBackup sağladığından, Veritas veya depolama yaşam döngüsü ilkesi (SLP'den) gereksinimleriniz düzgün bir şekilde değerlendirmek için NetBackup Mimarı ile başvurun öneririz.

İlk disk havuzları tanımladıktan sonra toplam dört İlkesi üç ek depolama yaşam döngüsü ilkeleri tanımlar gerekir:
* LocalRAIDVolume
* StorSimpleWeek2-4
* StorSimpleMonthlyFulls
* StorSimpleYearlyFulls

### <a name="to-assign-storsimple-volumes-to-a-netbackup-archive-and-duplication-job"></a>StorSimple birim NetBackup Arşiv ve çoğaltma işi atamak için

1. NetBackup yönetim konsolunda seçin **depolama** > **depolama yaşam döngüsü ilkeleri** > **yeni depolama yaşam döngüsü ilkesi**.

   ![NetBackup yönetim konsolunda, yeni depolama yaşam döngüsü ilkesi](./media/storsimple-configure-backup-target-using-netbackup/nbimage20.png)

2. Anlık görüntü için bir ad girin ve ardından **Ekle**.

3. İçinde **yeni işlem** iletişim kutusundaki **özellikleri** sekmesinde için **işlemi**seçin **yedekleme**. İstediğiniz değerleri seçin **hedef depolama**, **saklama türü**, ve **saklama süresi**. **Tamam**’ı seçin.

   ![NetBackup yönetim konsolunda, yeni işlem iletişim kutusu](./media/storsimple-configure-backup-target-using-netbackup/nbimage22.png)

   Bu ilk yedekleme işlemi ve depo tanımlar.

4. Önceki işlem vurgulayın ve ardından seçmek için Seç **Ekle**. İçinde **değiştirme depolama işlemi** iletişim kutusunda, istediğiniz değerleri seçin **hedef depolama**, **saklama türü**, ve **saklamasüresi**.

   ![NetBackup yönetim konsolunda, depolama işlemi Değiştir iletişim kutusu](./media/storsimple-configure-backup-target-using-netbackup/nbimage23.png)

5. Önceki işlem vurgulayın ve ardından seçmek için Seç **Ekle**. İçinde **yeni depolama yaşam döngüsü ilkesi** iletişim kutusunda, bir yıl boyunca aylık yedeklemeler ekleyin.

   ![NetBackup yönetim konsolunda, yeni depolama yaşam döngüsü ilkesi iletişim kutusu](./media/storsimple-configure-backup-target-using-netbackup/nbimage24.png)

6. Gereksinim duyduğunuz kapsamlı SLP'den bekletme ilkesi oluşturuncaya kadar 4-5 adımlarını tekrarlayın.

   ![NetBackup yönetim konsolunda, yeni depolama yaşam döngüsü ilkesi iletişim kutusu ilkelerini Ekle](./media/storsimple-configure-backup-target-using-netbackup/nbimage25.png)

7. İşiniz bittiğinde, SLP'den bekletme ilkesi altında tanımlama **ilke**, ayrıntılı adımları izleyerek bir yedekleme ilkesi tanımlama [NetBackup yedekleme işi için atama StorSimple birimlerini](#assigning-storsimple-volumes-to-a-netbackup-backup-job).

8. Altında **zamanlamaları**, **zamanlamasını değiştirme** iletişim kutusunda sağ **tam**ve ardından **değişiklik**.

   ![NetBackup yönetim konsolunda, zamanlamayı Değiştir iletişim kutusu](./media/storsimple-configure-backup-target-using-netbackup/nbimage26.png)

9. Seçin **ilkesi depolama seçimi geçersiz kılma** onay kutusunu işaretleyin ve ardından 1-6. adımda oluşturduğunuz SLP'den bekletme ilkesi seçin.

   ![NetBackup yönetim konsolunda, geçersiz kılma ilkesi depolama seçimi](./media/storsimple-configure-backup-target-using-netbackup/nbimage27.png)

10. Seçin **Tamam**ve artımlı yedekleme zamanlaması için yineleyin.

    ![NetBackup yönetim konsolunda, artımlı yedeklemeler için zamanlamayı Değiştir iletişim kutusu](./media/storsimple-configure-backup-target-using-netbackup/nbimage28.png)


| Yedekleme türü bekletme | Boyut (TiB) | GFS çarpanı\* | Toplam Kapasite (TiB)  |
|---|---|---|---|
| Haftalık tam |  1  |  4 | 4  |
| Günlük artımlı  | 0,5  | 20 (döngüleri hafta / ay sayısı eşittir) | 12 (2 ek kota için) |
| Aylık tam  | 1 | 12 | 12 |
| Yıllık tam | 1  | 10 | 10 |
| GFS gereksinimi  |     |     | 38 |
| Ek kota  | 4  |    | 42 toplam GFS gereksinimi |

\* GFS çarpan koruma ve yedekleme İlkesi gereksinimlerinizi karşılayacak şekilde korumak için ihtiyaç duyduğunuz kopya sayısıdır.

## <a name="storsimple-cloud-snapshots"></a>StorSimple bulut anlık görüntüleri

StorSimple bulut anlık görüntüleri, StorSimple Cihazınızı bulunan verileri koruyun. Bir bulut anlık görüntüsü oluşturma, bir şirket dışı özelliği için yerel yedekleme bantlarının sevkiyat için eşdeğerdir. Azure coğrafi olarak yedekli depolamayı kullanıyorsanız, bir bulut anlık görüntüsü oluşturma için birden çok site için yedekleme bantlarının sevkiyat eşdeğerdir. Bir cihaz bir olağanüstü durumdan sonra geri yüklemeniz gerekirse, başka bir StorSimple cihazı çevrimiçi duruma getirin ve yük devri gerçekleştirmeden. Yük devretmeden sonra en son bulut anlık görüntüden (bulut hızlarında) verilere erişmek mümkün olacaktır.

Aşağıdaki bölümde, başlatın ve yedekleme sonrası işleme sırasında StorSimple bulut anlık görüntüleri silmek için kısa bir komut dosyası oluşturmayı açıklar.

> [!NOTE]
> El ile veya programlama yoluyla oluşturulan anlık görüntüler, StorSimple anlık görüntü süre sonu ilkesi izlemeyin. El ile veya programlama yoluyla bu anlık görüntülerin silinmesi gerekir.

### <a name="start-and-delete-cloud-snapshots-by-using-a-script"></a>Bir komut dosyası kullanarak bulut anlık görüntüleri silin ve başlatın

> [!NOTE]
> StorSimple anlık görüntü silmeden önce uyumluluk ve veri saklama varsa dikkatlice değerlendirin. Bir yedekleme sonrası betik çalıştırma hakkında daha fazla bilgi için bkz. [NetBackup belgeleri](http://www.veritas.com/docs/000094423).

### <a name="backup-lifecycle"></a>Yedekleme yaşam döngüsü

![Yedekleme yaşam döngüsü diyagramı](./media/storsimple-configure-backup-target-using-netbackup/backuplifecycle.png)

### <a name="requirements"></a>Gereksinimler

-   Betik çalıştıran sunucuda, Azure bulut kaynaklarını erişiminiz olması gerekir.
-   Kullanıcı hesabı gerekli izinlere sahip olmalıdır.
-   Bir StorSimple yedekleme İlkesi ile ilişkili StorSimple birimlerini ayarlanmış ancak açık değil.
-   Gerekir StorSimple kaynak adı, kayıt anahtarı, cihaz adını ve yedekleme ilkesi kimliği.

### <a name="to-start-or-delete-a-cloud-snapshot"></a>Başlatın veya bir bulut anlık görüntüsünü silmek için

1. [Azure PowerShell'i yükleme](/powershell/azure/overview).
2. Karşıdan yükleme ve Kurulum [Yönet CloudSnapshots.ps1](https://github.com/anoobbacker/storsimpledevicemgmttools/blob/master/Manage-CloudSnapshots.ps1) PowerShell Betiği.
3. Betik çalıştıran sunucuda PowerShell'i yönetici olarak çalıştırın. Komut dosyasını çalıştırdığınızdan emin olun `-WhatIf $true` hangi kodun değiştiğini görmek için yapar. Doğrulama tamamlandıktan sonra geçmesi `-WhatIf $false`. Çalıştırma aşağıdaki komutu:
   ```powershell
   .\Manage-CloudSnapshots.ps1 -SubscriptionId [Subscription Id] -TenantId [Tenant ID] -ResourceGroupName [Resource Group Name] -ManagerName [StorSimple Device Manager Name] -DeviceName [device name] -BackupPolicyName [backup policyname] -RetentionInDays [Retention days] -WhatIf [$true or $false]
   ```
4. Yedekleme işi, NetBackup komut dosyası ekleyin. Bunu yapmak için ön işleme ve sonrası Komutlar işleme NetBackup proje seçenekleri düzenleyin.

> [!NOTE]
> Günlük yedekleme işi sonunda işlem sonrası bir betik olarak StorSimple bulut anlık görüntü yedekleme ilkenizi çalıştırmanızı öneririz. RTO ve RPO karşılamanıza yardımcı olmak için yedekleme Uygulama ortamınızı geri hakkında daha fazla bilgi için lütfen ile yedekleme, Mimarı başvurun.

## <a name="storsimple-as-a-restore-source"></a>Geri yükleme kaynağı olarak StorSimple

StorSimple cihaz iş herhangi bir blok depolama CİHAZDAN geri yüklemeler gibi geri yükler. Geri yüklemeler buluta katmanlanmış verileri bulut hızlarda gerçekleşir. Yerel veri için cihaz yerel disk hızında geri yüklemeler oluşur. Bir geri yükleme gerçekleştirme hakkında daha fazla bilgi için bkz: [NetBackup belgeleri](http://www.veritas.com/docs/000094423). NetBackup geri yükleme en iyi uygulamalar için uygun öneririz.

## <a name="storsimple-failover-and-disaster-recovery"></a>StorSimple yük devretme ve olağanüstü durum kurtarma

> [!NOTE]
> Yedekleme hedefi senaryoları için bir geri yükleme hedefi olarak StorSimple Cloud Appliance desteklenmiyor.

Olağanüstü bir durum tarafından çeşitli etkenler neden olabilir. Aşağıdaki tabloda olağanüstü durum kurtarma senaryoları listelenmektedir.

| Senaryo | Etki | Kurtarma | Notlar |
|---|---|---|---|
| StorSimple cihaz arızası | Yedekleme ve geri yükleme işlemlerini kesintiye uğramaz. | Başarısız aygıt değiştirin ve gerçekleştirme [StorSimple yük devretme ve olağanüstü durum kurtarma](storsimple-device-failover-disaster-recovery.md). | Bir geri yüklemeden sonra cihaz kurtarma gerçekleştirmeniz gerekirse, tam veri çalışma kümeleri için yeni cihaz buluttan alınır. Bulut hızlarda tüm işlemlerdir. Dizin ve işlemi yeniden tarama işlemi katalog taranır ve zaman alan bir işlem olabilir yerel cihaz katmanı için bulut katmanı çekilen tüm yedekleme kümelerini neden olabilir. |
| NetBackup sunucu hatası | Yedekleme ve geri yükleme işlemlerini kesintiye uğramaz. | Yedekleme sunucusuna yeniden oluşturun ve veritabanı geri yükleme gerçekleştirin. | Yeniden oluşturmanız veya olağanüstü durum kurtarma siteniz NetBackup sunucuda geri gerekir. Veritabanını geri yüklemek için en son noktası. Geri yüklenen NetBackup veritabanını en son yedekleme işleriniz ile eşitlenmiş durumda değilse, dizin oluşturma ve Katalog gereklidir. Bu dizin ve işlemi yeniden tarama işlemi katalog taranır ve yerel cihaz katmana bulut katmandan oluşan bir derleme tüm yedekleme kümelerini neden olabilir. Bu, daha fazla zaman yoğun kolaylaştırır. |
| Backup sunucusu ve StorSimple kaybı ile sonuçlanır site hatası | Yedekleme ve geri yükleme işlemlerini kesintiye uğramaz. | StorSimple önce geri ve ardından NetBackup geri yükleyin. | StorSimple önce geri ve ardından NetBackup geri yükleyin. Bir geri yüklemeden sonra cihaz kurtarma gerçekleştirmeniz gerekirse, tam veri çalışma kümeleri için yeni cihaz buluttan alınır. Bulut hızlarda tüm işlemlerdir. |

## <a name="references"></a>Başvurular

Aşağıdaki belgeler, bu makale için başvurulan:

- [StorSimple çok yollu g/ç Kurulumu](storsimple-configure-mpio-windows-server.md)
- [Depolama senaryoları: ölçülü kaynak sağlama](https://msdn.microsoft.com/library/windows/hardware/dn265487.aspx)
- [GPT kullanarak sürücüleri](https://msdn.microsoft.com/windows/hardware/gg463524.aspx#EHD)
- [Paylaşılan klasörler için gölge kopyaları ayarlama](https://technet.microsoft.com/library/cc771893.aspx)

## <a name="next-steps"></a>Sonraki adımlar

- Kullanma hakkında daha fazla bilgi edinin [bir yedekleme kümesinden geri yükleme](storsimple-restore-from-backup-set-u2.md).
- Nasıl gerçekleştirileceği hakkında daha fazla bilgi [cihaz yük devretme ve olağanüstü durum kurtarma](storsimple-device-failover-disaster-recovery.md).
