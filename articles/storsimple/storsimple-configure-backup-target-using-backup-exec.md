---
title: Backup Exec ile yedekleme hedefi olarak StorSimple 8000 serisi | Microsoft Docs
description: Veritas Backup Exec ile StorSimple yedekleme hedefi yapılandırmayı açıklar.
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
ms.date: 12/05/2016
ms.author: hkanna
ms.openlocfilehash: e11d541f0450c0de4ba6d60f889fc7471b1fa1aa
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60724521"
---
# <a name="storsimple-as-a-backup-target-with-backup-exec"></a>Backup Exec ile yedekleme hedefi olarak StorSimple

## <a name="overview"></a>Genel Bakış

Azure StorSimple bir Microsoft karma bulut depolama çözümüdür. StorSimple şirket içi çözümün bir uzantısı olarak Azure depolama hesabı kullanarak ve şirket içi depolama ve bulut depolama alanı üzerinden veri otomatik katmanlama üstel veri büyümesi karmaşıklığını ele alır.

Bu makalede, StorSimple tümleştirmesiyle Veritas Backup Exec ve her iki çözüm de tümleştirmeye yönelik en iyi uygulamalar ele alır. Biz de en iyi StorSimple ile tümleştirmek için Backup Exec konusunda önerilerde. Veritas en iyi yöntemleri, yedekleme mimarlar ve Yöneticiler için ayrı ayrı yedekleme gereksinimlerini ve hizmet düzeyi sözleşmelerine (SLA) karşılamak üzere Backup Exec ayarlamak en iyi yolu erteleyin.

Biz yapılandırma adımları ve temel kavramları göstermek olsa da, bu makalede göre Hayır olmadığı bir adım adım yapılandırma veya yükleme kılavuzudur. Temel bileşenlere ve altyapılara çalışma sırası ve açıklanmaktadır kavramları desteklemeye hazır olduğunu varsayıyoruz.

### <a name="who-should-read-this"></a>Bu kimler içindir?

Bu makaledeki bilgiler, yedekleme yöneticilerinin, depolama yöneticilerinin ve depolama, Windows Server 2012 R2, Ethernet, bulut Hizmetleri ve Backup Exec bilgisine sahip depolama mimarları için en faydalı olacaktır.

## <a name="supported-versions"></a>Desteklenen sürümler

-   [Yedekleme Exec 16 ve sonraki sürümler](http://backupexec.com/compatibility)
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
![StorSimple katmanlama diyagramı](./media/storsimple-configure-backup-target-using-backup-exec/image1.jpg)

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

![Mantıksal Diyagram birincil yedekleme hedefi olarak StorSimple](./media/storsimple-configure-backup-target-using-backup-exec/primarybackuptargetlogicaldiagram.png)

### <a name="primary-target-backup-logical-steps"></a>Birincil hedef yedekleme mantıksal adımlara

1.  Hedef Yedekleme aracısı yedekleme sunucusu ile iletişim kurar ve yedekleme aracısını veri yedekleme sunucusuna iletir.
2.  StorSimple için verileri yedekleme sunucusuna Yazar katmanlı birimler.
3.  Yedekleme sunucusuna katalog veritabanını güncelleştirir ve ardından yedekleme işi tamamlandıktan.
4.  Anlık görüntü betik StorSimple bulut anlık görüntü Yöneticisi (başlatma veya silme) tetiklenir.
5.  Yedekleme sunucusuna bir bekletme ilkesi temel alınarak süresi dolmuş yedeklemeleri siler.


### <a name="primary-target-restore-logical-steps"></a>Birincil hedef geri yükleme mantıksal adımlara

1.  Yedek sunucu uygun verileri depolama depodan geri başlatır.
2.  Yedekleme aracısı yedekleme sunucusundan verileri alır.
3.  Yedekleme sunucusuna geri yükleme işi tamamlar.

## <a name="storsimple-as-a-secondary-backup-target"></a>İkincil yedekleme hedefi olarak StorSimple

Bu senaryoda, StorSimple birimlerini öncelikli olarak uzun süreli saklama veya arşivleme için kullanılır.

Aşağıdaki şekil, ilk hangi yedeklemelerde bir mimari gösterilir ve yüksek performanslı hedef birim geri yükler. Bu yedeklemeler kopyalanır ve arşivlenmiş bir StorSimple için katmanlı birim üzerinde ayarlanmış bir planlamada.

Bekletme İlkesi kapasite ve performans gereksinimlerinizi işleyebilmeniz yüksek performanslı toplu boyutlandırmak önemlidir.

![Mantıksal Diyagram ikincil yedekleme hedefi olarak StorSimple](./media/storsimple-configure-backup-target-using-backup-exec/secondarybackuptargetlogicaldiagram.png)

### <a name="secondary-target-backup-logical-steps"></a>İkincil hedef yedekleme mantıksal adımlara

1.  Hedef Yedekleme aracısı yedekleme sunucusu ile iletişim kurar ve yedekleme aracısını veri yedekleme sunucusuna iletir.
2.  Backup sunucusu, yüksek performanslı depolama alanına verileri yazar.
3.  Yedekleme sunucusuna katalog veritabanını güncelleştirir ve ardından yedekleme işi tamamlandıktan.
4.  Backup sunucusu yedekleme bekletme ilkesi temel alınarak StorSimple kopyalar.
5.  Anlık görüntü betik StorSimple bulut anlık görüntü Yöneticisi (başlatma veya silme) tetiklenir.
6.  Yedekleme sunucusuna bir bekletme ilkesi temel alınarak süresi dolmuş yedeklemeleri siler.

### <a name="secondary-target-restore-logical-steps"></a>İkincil hedef geri yükleme mantıksal adımlara

1.  Yedek sunucu uygun verileri depolama depodan geri başlatır.
2.  Yedekleme aracısı yedekleme sunucusundan verileri alır.
3.  Yedekleme sunucusuna geri yükleme işi tamamlar.

## <a name="deploy-the-solution"></a>Çözümü dağıtma

Çözüm dağıtımı, üç adımı gerektirir:
1. Ağ altyapısını hazırlayın.
2. Bir yedekleme hedefi olarak StorSimple Cihazınızı dağıtma.
3. Yedekleme Exec dağıtın.

Her adım, aşağıdaki bölümlerde ayrıntılı olarak ele alınmıştır.

### <a name="set-up-the-network"></a>Ağı ayarlama

StorSimple, StorSimple Azure bulutla tümleşik bir çözüm olduğundan, bir etkin ve Azure bulut çalışan bağlantısı gerektirir. Katmanı için eski, daha az erişilen verileri Azure bulut depolama ve bu bağlantı, bulut anlık görüntüleri, yönetim ve meta veri aktarımı gibi işlemleri için kullanılır.

Çözüm göstermesi bu ağ en iyi uygulamaları izlemenizi öneririz:

-   Katmanlama, StorSimple Azure'a bağlanan bağlantı, bant genişliği gereksinimlerini karşılaması gerekir. Bunu başarmak için altyapınız için gerekli hizmet kalitesi (QoS) düzeyi geçerli RPO ve kurtarma eşleştirilecek anahtarları süresi hedefi (RTO) SLA'lar.
-   Maksimum Azure Blob Depolama erişim gecikmeleri yaklaşık 80 ms olmalıdır.

### <a name="deploy-storsimple"></a>StorSimple'ı dağıtma

Bir adım adım StorSimple dağıtım yönergeleri için bkz. [şirket içi StorSimple Cihazınızı dağıtma](storsimple-deployment-walkthrough-u2.md).

### <a name="deploy-backup-exec"></a>Yedekleme Exec dağıtma

Backup Exec yükleme en iyi yöntemler için bkz: [Backup Exec yükleme için en iyi yöntemler](https://www.veritas.com/content/support/en_US/doc/72686287-131623464-0/v70444238-131623464).

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

- Dağıtılmış birimler (Windows Disk Management tarafından oluşturulan) kullanmayın. Dağıtılmış diskler desteklenmez.
- Boyutu 64 KB ayırma NTFS kullanılarak, birimleri biçimlendirin.
- StorSimple birimlerini Backup Exec sunucunun doğrudan eşleyin.
    - Fiziksel sunucuları için iSCSI kullanın.
    - Geçiş disklerini sanal sunucular için kullanın.

## <a name="best-practices-for-storsimple-and-backup-exec"></a>StorSimple ve Backup Exec için en iyi uygulamalar

Aşağıdaki bölümlerde, çözümünüzün yönergelerine göre ayarlayın.

### <a name="operating-system-best-practices"></a>İşletim sistemi en iyi uygulamalar

- Windows Server şifreleme ve yinelenenleri kaldırma NTFS dosya sistemi devre dışı bırakın.
- Windows Server birleştirme StorSimple birimlerde devre dışı bırakın.
- Windows Server StorSimple birimlerde dizin oluşturmayı devre dışı bırakın.
- Kaynak ana bilgisayar (değil karşı StorSimple birimlerini) bir virüsten koruma taraması çalıştırın.
- Varsayılan devre dışı kapatma [Windows Server bakım](https://msdn.microsoft.com/library/windows/desktop/hh848037.aspx) Görev Yöneticisi'nde. Bu aşağıdaki yollardan birini yapın:
  - Windows Görev Zamanlayıcısı'nda bakım configurator devre dışı bırakın.
  - İndirme [PsExec](https://technet.microsoft.com/sysinternals/bb897553.aspx) Windows SysInternals öğesinden. PsExec indirdikten sonra Azure PowerShell bir Yöneticiyseniz ve türü çalıştırın:
    ```powershell
    psexec \\%computername% -s schtasks /change /tn “MicrosoftWindowsTaskSchedulerMaintenance Configurator" /disable
    ```

### <a name="storsimple-best-practices"></a>StorSimple en iyi uygulamalar

  -   StorSimple cihazı için güncelleştirildiğinden emin olun [güncelleştirme 3 veya üzeri](storsimple-install-update-3.md).
  -   Yalıtım iSCSI ve bulut trafiği. StorSimple ve backup sunucusu arasındaki trafiği adanmış iSCSI bağlantıları kullanın.
  -   StorSimple Cihazınızı adanmış bir yedekleme hedefi olduğundan emin olun. RTO ve RPO etkilediğinden, karma iş yükleri desteklenmez.

### <a name="backup-exec-best-practices"></a>Yedekleme Exec en iyi uygulamalar

-   Sunucusunun yerel bir sürücü ve bir StorSimple birimde yedekleme Exec yüklenmesi gerekir.
-   Backup Exec depolamayı **eşzamanlı yazma işlemlerini** için izin verilen üst sınırı.
    -   Backup Exec depolamayı **blok ve arabellek boyutu** 512 KB.
    -   Backup Exec depolama kapatma **arabelleğe okuma ve yazma**.
-   StorSimple, Backup Exec tam ve artımlı yedeklemeler destekler. Yapay ve farklı yedeklemelerini kullanmamanızı öneririz.
-   Yedek veri dosyaları, yalnızca belirli bir işin verileri içermelidir. Örneğin, arasında medya ekler farklı işleri izin verilir.
-   İş doğrulama devre dışı bırakın. Gerekirse, en son yedekleme işinden sonra doğrulama zamanlanmalıdır. Bu iş, Yedekleme penceresi etkileyeceğini anlamak önemlidir.
-   Seçin **depolama** > **diskinizi** > **ayrıntıları** > **özellikleri**. Devre dışı **önceden disk alanı Ayır**.

En son Backup Exec ayarları ve bu gereksinimleri uygulamak için en iyi yöntemler için bkz: [Veritas Web sitesi](https://www.veritas.com).

## <a name="retention-policies"></a>Elde tutma ilkeleri

Yaygın yedekleme bekletme ilkesi türlerinden birini Dedenizin Baba ve Son (GFS) bir ilkedir. GFS ilkesinde artımlı yedekleme günlük gerçekleştirilir ve tam yedekleme haftalık ve aylık olarak gerçekleştirilir. Bu ilke sonuçlarda altı StorSimple katmanlı birimleri. Bir birim, haftalık, aylık ve yıllık tam yedeklemeler içerir. Diğer beş birimleri günlük artımlı yedeklemeleri depolar.

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

## <a name="set-up-backup-exec-storage"></a>Backup Exec depolamayı ayarlama

### <a name="to-set-up-backup-exec-storage"></a>Backup Exec depolama alanı ayarlama için

1.  Backup Exec yönetim konsolunda seçin **depolama** > **yapılandırma depolama** > **Disk tabanlı depolama**  >   **Sonraki**.

    ![Exec Yönetim Konsolu yedekleme, yapılandırma depolama sayfası](./media/storsimple-configure-backup-target-using-backup-exec/image4.png)

2.  Seçin **Disk Depolama**ve ardından **sonraki**.

    ![Yedekleme Exec Yönetim Konsolu, depolama seçme sayfası](./media/storsimple-configure-backup-target-using-backup-exec/image5.png)

3.  Temsili bir ad girin, örneğin, **Cumartesi tam**ve bir açıklama. **İleri**’yi seçin.

    ![Yedekleme Exec yönetim konsolunu, ad ve açıklama sayfası](./media/storsimple-configure-backup-target-using-backup-exec/image7.png)

4.  Disk depolama cihazı oluşturma ve ardından istediğiniz diski seçin **sonraki**.

    ![Exec Yönetim Konsolu, depolama, disk Seçimi sayfasında yedekleme](./media/storsimple-configure-backup-target-using-backup-exec/image9.png)

5.  Yazma işlemlerinin sayısını artırmak **16**ve ardından **sonraki**.

    ![Yedekleme Exec Yönetim Konsolu, eş zamanlı yazma işlemleri Ayarları sayfası](./media/storsimple-configure-backup-target-using-backup-exec/image10.png)

6.  Ayarları gözden geçirin ve ardından **son**.

    ![Yedekleme Exec Yönetim Konsolu, depolama yapılandırması Özet sayfası](./media/storsimple-configure-backup-target-using-backup-exec/image11.png)

7.  Her birim atama sonunda, bu önerilen eşleşecek şekilde depolama cihaz ayarlarını değiştirme [en iyi uygulamalar için StorSimple ve Backup Exec](#best-practices-for-storsimple-and-backup-exec).

    ![Yedekleme Exec Yönetim Konsolu, depolama cihaz Ayarları sayfası](./media/storsimple-configure-backup-target-using-backup-exec/image12.png)

8.  StorSimple birimlerinizi Backup Exec atama tamamlandı olana kadar 1-7. adımları tekrarlayın.

## <a name="set-up-storsimple-as-a-primary-backup-target"></a>Birincil bir yedekleme hedefi olarak StorSimple ayarlama

> [!NOTE]
> Bulut hızlarında buluta katmanlanmış bir yedekten verileri geri yükleme gerçekleşir.

Aşağıdaki şekilde, bir yedekleme işi için tipik bir birimin eşlemeyi gösterir. Bu durumda, haftalık yedekleri Cumartesi tam disk eşleyin ve artımlı yedeklemeler Pazartesi-Cuma artımlı disklere eşleyin. Tüm yedekleme ve geri yüklemeler Storsimple'dan verileri, birim katmanlı.

![Birincil yedekleme hedefi yapılandırma mantıksal diyagramı](./media/storsimple-configure-backup-target-using-backup-exec/primarybackuptargetdiagram.png)

### <a name="storsimple-as-a-primary-backup-target-gfs-schedule-example"></a>Birincil GFS yedekleme hedefi olarak StorSimple örnek zamanlama

Dört hafta, aylık ve yıllık GFS döndürme tablosunun bir örnek aşağıda verilmiştir:

| Sıklığı/yedekleme türü | Tam | Artımlı (gün 1-5)  |   
|---|---|---|
| Haftalık (hafta 1-4) | Cumartesi | Pazartesi-Cuma |
| Aylık  | Cumartesi  |   |
| Yıllık | Cumartesi  |   |


### <a name="assign-storsimple-volumes-to-a-backup-exec-backup-job"></a>StorSimple birim atamak için Backup Exec yedekleme işi

Aşağıdaki sırayı Backup Exec aracı yönergelerine uygun olarak, Backup Exec ve hedef ana bilgisayarın yapılandırıldığını varsayar.

#### <a name="to-assign-storsimple-volumes-to-a-backup-exec-backup-job"></a>Bir Backup Exec yedekleme işi için StorSimple birim atamak için

1.  Backup Exec yönetim konsolunda seçin **konak** > **yedekleme** > **diske yedekleme**.

    ![Yedekleme Exec Yönetim Konsolu, seçilen konağın, yedekleme ve diske yedekleme](./media/storsimple-configure-backup-target-using-backup-exec/image14.png)

2.  İçinde **yedekleme tanımı özellikleri** iletişim kutusunun **yedekleme**seçin **Düzenle**.

    ![Yedekleme Exec Yönetim Konsolu, yedekleme tanımı Özellikleri iletişim kutusu](./media/storsimple-configure-backup-target-using-backup-exec/image15.png)

3.  Tam ve artımlı yedeklemeleri ayarlama, RPO ve RTO gereksinimlerinizi karşılayacak ve Veritas en iyi uygulamalar için uygun olacak şekilde ayarlayın.

4.  İçinde **yedekleme seçenekleri** iletişim kutusunda **depolama**.

    ![Yedekleme Exec Yönetim Konsolu, yedekleme seçeneklerini depolama iletişim kutusu](./media/storsimple-configure-backup-target-using-backup-exec/image16.png)

5.  StorSimple birimlerini karşılık gelen yedekleme zamanlamanızı atayın.

    > [!NOTE]
    > **Sıkıştırma** ve **şifreleme türü** ayarlandığından **hiçbiri**.

6.  Altında **doğrulama**seçin **bu iş için veri doğrulamayın** onay kutusu. Bu seçeneği kullanarak StorSimple katmanlama etkileyebilir.

    > [!NOTE]
    > Birleştirme, dizin oluşturma ve arka plan doğrulama StorSimple katmanlama olumsuz şekilde etkileyebilir.

    ![Yedekleme Exec Yönetim Konsolu, yedekleme seçenekleri ayarlar doğrulayın](./media/storsimple-configure-backup-target-using-backup-exec/image17.png)

7.  Yedekleme seçeneklerinizin gereksinimlerinizi karşılayacak şekilde geri kalanını belirledikten sonra seçin **Tamam** tamamlanması.

## <a name="set-up-storsimple-as-a-secondary-backup-target"></a>İkincil bir yedekleme hedefi olarak StorSimple ayarlama

> [!NOTE]
>Bulut hızlarında buluta katmanlanmış bir yedekten verileri geri yüklemeler oluşur.

Bu modelde, geçici bir önbellek olarak görev yapacak bir depolama medyasına (dışında StorSimple) olmalıdır. Örneğin, boşluk, giriş/çıkış (g/ç) ve bant genişliği uyum sağlamak için yedek birim bağımsız diskler (RAID) dizisi kullanabilirsiniz. RAID 5, 50 ve 10 kullanmanızı öneririz.

Aşağıdaki şekilde normal kısa vadeli bekletme yerel (sunucu) birimleri gösterir ve uzun süreli saklama birimleri arşivler. Bu senaryoda, tüm yedeklemeler (sunucu) yerel RAID birimine çalıştırın. Bu yedeklemeler düzenli aralıklarla yinelenen ve bir arşivleri birimine arşivlenir. Kısa vadeli bekletme kapasite ve performans gereksinimlerinizi işleyebilmeniz yerel (sunucu) RAID toplu boyutlandırmak önemlidir.

### <a name="storsimple-as-a-secondary-backup-target-gfs-example"></a>İkincil yedekleme hedefi GFS örnek olarak StorSimple

![Mantıksal diyagramı ikincil yedekleme hedefi olarak StorSimple](./media/storsimple-configure-backup-target-using-backup-exec/secondarybackuptargetdiagram.png)

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


### <a name="assign-storsimple-volumes-to-a-backup-exec-archive-and-deduplication-job"></a>StorSimple birim atamak için bir Backup Exec Arşiv ve iş yinelenenleri kaldırma

#### <a name="to-assign-storsimple-volumes-to-a-backup-exec-archive-and-duplication-job"></a>Bir Backup Exec Arşiv ve çoğaltma işi StorSimple birim atamak için

1.  Backup Exec yönetim konsolunda, sağ tıklayın, bir StorSimple biriminin için arşiv ve ardından istediğiniz iş **yedekleme tanımı özellikleri** > **Düzenle**.

    ![Yedekleme Exec Yönetim Konsolu, yedekleme tanımı Özellikleri sekmesi](./media/storsimple-configure-backup-target-using-backup-exec/image19.png)

2.  Seçin **aşama ekleyin** > **yinelenen Disk** > **Düzenle**.

    ![Exec Yönetim Konsolu yedekleme, aşama ekleyin](./media/storsimple-configure-backup-target-using-backup-exec/image20.png)

3.  İçinde **yinelenen seçenekleri** iletişim kutusunda, kullanmak istediğiniz değerleri seçin **kaynak** ve **zamanlama**.

    ![Exec Yönetim Konsolu, yedekleme tanımları özellikleri ve yinelenen seçenekleri yedekleme](./media/storsimple-configure-backup-target-using-backup-exec/image21.png)

4.  İçinde **depolama** aşağı açılan listesinde, istediğiniz verileri depolamak için arşivleme işi StorSimple birimini seçin.

    ![Exec Yönetim Konsolu, yedekleme tanımları özellikleri ve yinelenen seçenekleri yedekleme](./media/storsimple-configure-backup-target-using-backup-exec/image22.png)

5.  Seçin **doğrulama**ve ardından **bu iş için veri doğrulamayın** onay kutusu.

    ![Exec Yönetim Konsolu, yedekleme tanımları özellikleri ve yinelenen seçenekleri yedekleme](./media/storsimple-configure-backup-target-using-backup-exec/image23.png)

6.  **Tamam**’ı seçin.

    ![Exec Yönetim Konsolu, yedekleme tanımları özellikleri ve yinelenen seçenekleri yedekleme](./media/storsimple-configure-backup-target-using-backup-exec/image24.png)

7.  İçinde **yedekleme** sütununda, yeni aşama ekleyin. Kaynak kullanan **artımlı**. Hedef için artımlı yedekleme işi nerede arşivlenir StorSimple birimini seçin. 1-6. adımları tekrarlayın.

## <a name="storsimple-cloud-snapshots"></a>StorSimple bulut anlık görüntüleri

StorSimple bulut anlık görüntüleri, StorSimple Cihazınızı bulunan verileri koruyun. Bir bulut anlık görüntüsü oluşturma, bir şirket dışı özelliği için yerel yedekleme bantlarının sevkiyat için eşdeğerdir. Azure coğrafi olarak yedekli depolamayı kullanıyorsanız, bir bulut anlık görüntüsü oluşturma için birden çok site için yedekleme bantlarının sevkiyat eşdeğerdir. Bir cihaz bir olağanüstü durumdan sonra geri yüklemeniz gerekirse, başka bir StorSimple cihazı çevrimiçi duruma getirin ve yük devri gerçekleştirmeden. Yük devretmeden sonra en son bulut anlık görüntüden (bulut hızlarında) verilere erişmek mümkün olacaktır.

Aşağıdaki bölümde, başlatın ve yedekleme sonrası işleme sırasında StorSimple bulut anlık görüntüleri silmek için kısa bir komut dosyası oluşturmayı açıklar.

> [!NOTE]
> El ile veya programlama yoluyla oluşturulan anlık görüntüler, StorSimple anlık görüntü süre sonu ilkesi izlemeyin. El ile veya programlama yoluyla bu anlık görüntülerin silinmesi gerekir.

### <a name="start-and-delete-cloud-snapshots-by-using-a-script"></a>Bir komut dosyası kullanarak bulut anlık görüntüleri silin ve başlatın

> [!NOTE]
> StorSimple anlık görüntü silmeden önce uyumluluk ve veri saklama varsa dikkatlice değerlendirin. Bir yedekleme sonrası betik çalıştırma hakkında daha fazla bilgi için bkz. [Backup Exec belgeleri](https://www.veritas.com/support/en_US/article.100032497.html).

### <a name="backup-lifecycle"></a>Yedekleme yaşam döngüsü

![Yedekleme yaşam döngüsü diyagramı](./media/storsimple-configure-backup-target-using-backup-exec/backuplifecycle.png)

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
4. Betik yedekleme, yedekleme Exec işinde Backup Exec proje seçenekleri ön işleme düzenleme ve sonrası Komutlar işleme ekleyin.

   ![Backup Exec konsol, yedekleme seçenekleri, ön ve son işlem komutları sekmesi](./media/storsimple-configure-backup-target-using-backup-exec/image25.png)

> [!NOTE]
> Günlük yedekleme işi sonunda işlem sonrası bir betik olarak StorSimple bulut anlık görüntü yedekleme ilkenizi çalıştırmanızı öneririz. RTO ve RPO karşılamanıza yardımcı olmak için yedekleme Uygulama ortamınızı geri hakkında daha fazla bilgi için lütfen ile yedekleme, Mimarı başvurun.

## <a name="storsimple-as-a-restore-source"></a>Geri yükleme kaynağı olarak StorSimple

StorSimple cihaz iş herhangi bir blok depolama CİHAZDAN geri yüklemeler gibi geri yükler. Geri yüklemeler buluta katmanlanmış verileri bulut hızlarda gerçekleşir. Yerel veri için cihaz yerel disk hızında geri yüklemeler oluşur. Bir geri yükleme gerçekleştirme hakkında daha fazla bilgi için Backup Exec belgelerine bakın. Backup Exec geri yükleme en iyi uygulamalar için uygun öneririz.

## <a name="storsimple-failover-and-disaster-recovery"></a>StorSimple yük devretme ve olağanüstü durum kurtarma

> [!NOTE]
> Yedekleme hedefi senaryoları için bir geri yükleme hedefi olarak StorSimple Cloud Appliance desteklenmiyor.

Olağanüstü bir durum tarafından çeşitli etkenler neden olabilir. Aşağıdaki tabloda olağanüstü durum kurtarma senaryoları listelenmektedir.

| Senaryo | Etki | Kurtarma | Notlar |
|---|---|---|---|
| StorSimple cihaz arızası | Yedekleme ve geri yükleme işlemlerini kesintiye uğramaz. | Başarısız aygıt değiştirin ve gerçekleştirme [StorSimple yük devretme ve olağanüstü durum kurtarma](storsimple-device-failover-disaster-recovery.md). | Bir geri yüklemeden sonra cihaz kurtarma gerçekleştirmeniz gerekirse, tam veri çalışma kümeleri için yeni cihaz buluttan alınır. Bulut hızlarda tüm işlemlerdir. Dizin oluşturma ve kataloglama rescanning işlem taranır ve zaman alan bir işlem olabilir yerel cihaz katmanı için bulut katmanı çekilen tüm yedekleme kümelerini neden olabilir. |
| Yedekleme Exec sunucu hatası | Yedekleme ve geri yükleme işlemlerini kesintiye uğramaz. | Yedekleme sunucusuna yeniden oluşturun ve veritabanı geri yükleme ayrıntılı olarak açıklandığı gibi gerçekleştirin [el ile yedekleme ve geri yükleme, yedekleme Exec (BEDB) veritabanı nasıl](http://www.veritas.com/docs/000041083). | Yeniden oluşturmanız veya olağanüstü durum kurtarma siteniz Backup Exec sunucuda geri gerekir. Veritabanını geri yüklemek için en son noktası. Geri yüklenen veritabanı yedekleme Exec en son yedekleme işleriniz ile eşitlenmiş durumda değilse, dizin oluşturma ve Katalog gereklidir. Bu dizin ve işlemi yeniden tarama işlemi katalog taranır ve yerel cihaz katmana bulut katmandan oluşan bir derleme tüm yedekleme kümelerini neden olabilir. Bu, daha fazla zaman yoğun kolaylaştırır. |
| Backup sunucusu ve StorSimple kaybı ile sonuçlanır site hatası | Yedekleme ve geri yükleme işlemlerini kesintiye uğramaz. | StorSimple önce geri ve ardından Backup Exec geri yükleyin. | StorSimple önce geri ve ardından Backup Exec geri yükleyin. Bir geri yüklemeden sonra cihaz kurtarma gerçekleştirmeniz gerekirse, tam veri çalışma kümeleri için yeni cihaz buluttan alınır. Bulut hızlarda tüm işlemlerdir. |

## <a name="references"></a>Başvurular

Aşağıdaki belgeler, bu makale için başvurulan:

- [StorSimple çok yollu g/ç Kurulumu](storsimple-configure-mpio-windows-server.md)
- [Depolama senaryoları: ölçülü kaynak sağlama](https://msdn.microsoft.com/library/windows/hardware/dn265487.aspx)
- [GPT kullanarak sürücüleri](https://msdn.microsoft.com/windows/hardware/gg463524.aspx#EHD)
- [Paylaşılan klasörler için gölge kopyaları ayarlama](https://technet.microsoft.com/library/cc771893.aspx)

## <a name="next-steps"></a>Sonraki adımlar

- Kullanma hakkında daha fazla bilgi edinin [bir yedekleme kümesinden geri yükleme](storsimple-restore-from-backup-set-u2.md).
- Nasıl gerçekleştirileceği hakkında daha fazla bilgi [cihaz yük devretme ve olağanüstü durum kurtarma](storsimple-device-failover-disaster-recovery.md).
