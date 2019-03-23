---
title: Azure Backup mimarisi
description: Mimari, bileşenler ve işlemler Azure Backup hizmeti tarafından kullanılan genel bir bakış sağlar.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 02/19/2019
ms.author: raynew
ms.openlocfilehash: 98ffe145103b4be04014627ed04d04dcf7542015
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58368970"
---
# <a name="azure-backup-architecture"></a>Azure Backup mimarisi

Kullanabileceğiniz [Azure Backup hizmeti](backup-overview.md) verileri yedeklemek için Microsoft Azure bulut platformu için. Bu makalede, Azure Backup mimarisi, bileşenler ve işlemler özetlenir. 

## <a name="what-does-azure-backup-do"></a>Azure Backup ne yapar?

Azure yedekleme, veri, makine durumunu ve şirket içi makinelerin ve Azure sanal makine (VM) örnekleri üzerinde çalışan iş yüklerini yedekler. Azure Backup senaryolar vardır.

## <a name="how-does-azure-backup-work"></a>Azure Backup nasıl çalışır?

Makineleri ve verileri bir dizi yöntem kullanarak yedekleyebilirsiniz:

- **Şirket içi makineleri yedekleyin**:
    - Şirket içi Windows makinelerini Azure Backup Microsoft Azure kurtarma Hizmetleri (MARS) Aracısı'nı kullanarak, doğrudan Azure'a yedekleyebilirsiniz. Linux makineleri desteklenmez.
    - Bir yedekleme sunucusuna (System Center Data Protection Manager (DPM) veya Microsoft Azure Backup sunucusu (MABS)) şirket içi makineleri yedekleyebilir. Ardından, azure'daki bir kurtarma Hizmetleri kasasına yedekleme sunucusunu da yedekleyebilirsiniz.

- **Azure Vm'lerini yedekleme**:
    - Doğrudan Azure sanal makinelerini yedekleyebilirsiniz. Azure yedekleme Azure VM Aracısı, VM'de çalışan yedekleme uzantısını yükler. Bu uzantı, VM'nin tamamını yedekler.
    - MARS Aracısı'nı çalıştırarak belirli dosyaları ve klasörleri Azure VM üzerinde yedekleyebilirsiniz.
    - Azure'da çalışan MABS için Azure Vm'lerini yedekleme ve ardından MABS bir kurtarma Hizmetleri kasasına yedekleyebilirsiniz.

Daha fazla bilgi edinin [neleri yedekleyebilir](backup-overview.md) ve yaklaşık [yedekleme senaryoları desteklenen](backup-support-matrix.md).

## <a name="where-is-data-backed-up"></a>Burada veri yedeklenir mi?

Azure yedekleme, Kurtarma Hizmetleri kasasında yedeklenen verileri depolar. Bir kasası azure'da yedek kopyalar, Kurtarma noktaları ve yedekleme ilkeleri gibi verileri tutmak için kullanılan bir çevrimiçi depolama varlıktır.

Kurtarma Hizmetleri kasaları, aşağıdaki özelliklere sahiptir:

- Kasaları, yedekleme verilerinizi, yönetim yükünü en aza indirerek düzenlemek kolaylaştırır.
- Her Azure aboneliği, en fazla 500 kasa oluşturabilirsiniz.
- Şirket içi ve Azure sanal makineler dahil olmak üzere bir kasadaki yedekleme öğeleri izleyebilirsiniz.
- Azure kasası erişimi yönetebilirsiniz [rol tabanlı erişim denetimi (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal).
- Kasadaki veri yedekliliği nasıl çoğaltıldığını belirtirsiniz:
    - **Yerel olarak yedekli depolama (LRS)**: Bir veri merkezinde arızasına karşı korumak için LRS kullanabilirsiniz. LRS, verileri bir depolama Ölçek birimine çoğaltır. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/storage/common/storage-redundancy-lrs).
    - **Coğrafi olarak yedekli depolama (GRS)**: Bölge çapında kesintilerden karşı korumak için GRS kullanabilirsiniz. GRS, verilerinizi ikincil bölgeye çoğaltır. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/storage/common/storage-redundancy-grs). 
    - Varsayılan olarak, Kurtarma Hizmetleri kasaları GRS kullanın. 

## <a name="backup-agents"></a>Yedekleme aracıları

Azure yedekleme, farklı yedekleme aracıları sağlar, makine türüne bağlı olarak yedeklenen:

**Aracı** | **Ayrıntılar** 
--- | --- 
**MARS Aracısı** | <ul><li>Dosyaları, klasörleri ve sistem durumu yedekleme için tek tek şirket içi Windows Server makinelerde çalışır.</li> <li>Dosyaları, klasörleri ve sistem durumunu yedeklemek için Azure Vm'leri üzerinde çalışır.</li> <li>DPM/MABS yerel depolama diski Azure'a yedeklemek için DPM/MABS sunucuları üzerinde çalışır.</li></ul> 
**Azure VM uzantısı** | Bir kasa kadar bunları yedeklemek için Azure Vm'leri üzerinde çalışır.

## <a name="backup-types"></a>Yedekleme türleri

Aşağıdaki tabloda, yedekleme ve alışık değilseniz farklı türleri açıklanmaktadır:

**Yedekleme türü** | **Ayrıntılar** | **Kullanım**
--- | --- | ---
**Tam** | Tam yedekleme, veri kaynağının tamamını içerir. Fark veya artımlı yedeklemeler değerinden daha fazla ağ bant genişliğini alır. | İlk yedekleme için kullanılır.
**Fark** |  Değişiklik yedeği ilk tam yedeklemeden sonra değiştirilen blokları depolar. Değişmeyen verilerin gereksiz kopyalarını korumayarak ve daha küçük miktarda ağ ve depolama alanı kullanır.<br/><br/> Sonraki yedeklemeler arasında değişmeden veri blokları aktarıldığı ve depolandığı için yetersiz. | Azure Backup tarafından kullanılmaz.
**Artımlı** | Artımlı yedekleme, yalnızca önceki yedeklemeden itibaren değişmiş olan veri bloklarını depolar. Yüksek depolama ve ağ verimliliği. <br/><br/> Artımlı yedekleme ile tam yedeklemeler için gerek yoktur. | Disk yedekleme için DPM/MABS tarafından kullanılan ve azure'a tüm yedeklemeler kullanılır.

## <a name="sql-server-backup-types"></a>SQL Server Yedekleme türleri

Aşağıdaki tabloda, SQL Server veritabanları ve ne sıklıkta kullanılan için kullanılan yedekleme farklı türleri açıklanmaktadır:

**Yedekleme türü** | **Ayrıntılar** | **Kullanım**
--- | --- | ---
**Tam yedekleme** | Tam veritabanı yedeği tüm veritabanını yedekler. Bu, tüm verileri belirli bir veritabanı veya dosya grubu veya dosyaları içerir. Tam yedekleme, ayrıca bu verileri kurtarmak için yeterli günlükleri içerir. | En fazla günde bir tam yedekleme tetikleyin.<br/><br/> Günlük veya haftalık bir aralıkta bir tam yedekleme yapmak seçebilirsiniz.
**Fark yedekleme** | Değişiklik yedeği en son, önceki tam veri yedekleme temel alır.<br/><br/> Bu, yalnızca tam yedeklemeden bu yana değişmiş olan verileri yakalar. |  En fazla günde bir fark yedekleme tetikleyebilirsiniz.<br/><br/> Aynı gün tam yedekleme ve bir değişiklik yedeği yapılandıramazsınız.
**İşlem günlüğü yedeklemesi** | Bir günlük yedeği, belirli bir saniye kadar zaman içinde nokta geri yükleme sağlar. | En fazla 15 dakikada bir işlem günlüğü yedeklemeleri yapılandırabilirsiniz.

### <a name="comparison-of-backup-types"></a>Yedekleme türleri karşılaştırması

Depolama alanı tüketimi, Kurtarma süresi hedefi (RTO) ve ağ tüketimi yedekleme her tür için farklılık gösterir. Aşağıdaki görüntüde, Yedekleme türleri karşılaştırması gösterilmektedir:

- Veri kaynağı A, aylık yedeklenen 10 depolama blokları, A1-A10 oluşur.
- A2, A3, A4 ve A9 blokları ilk ayda, A5 bloku ise sonraki ayda değişmektedir.
- Farklı yedeklemeler için ikinci ayda değiştirilen A2, A3, A4 ve A9 blokları yedeklenir. Üçüncü ayda değiştirilen A5 blokuna ek olarak aynı bloklar tekrar yedeklenir. Sonraki tam yedeklemeye kadar değiştirilen bloklar yedeklenmeye devam eder.
- İkinci ayda artımlı yedeklemeler için olarak işaretlenmiş A2, A3, A4 ve A9 blokları değiştirilmiş ve aktarılmaz. Üçüncü ayda yalnızca değiştirilen A5 bloku işaretlenir ve aktarılır. 

![yedekleme yöntemlerinin karşılaştırmasını gösteren resim](./media/backup-architecture/backup-method-comparison.png)

## <a name="backup-features"></a>Yedekleme özellikleri

Aşağıdaki tabloda farklı yedekleme türleri için desteklenen özellikler özetlenmiştir:

**Özellik** | **Şirket içi Windows Server makineleri (doğrudan)** | **Azure Vm'leri** | **Makineler veya DPM/MABS uygulamalarını**
--- | --- | --- | ---
Kasa için yedekleme | ![Evet][green] | ![Evet][green] | ![Evet][green] 
DPM/MABS diske, ardından Azure yedekleme | | | ![Evet][green] 
Yedekleme için gönderilen veri sıkıştırma | ![Evet][green] | Veri aktarımı sıkıştırma kullanılır. Depolama biraz şişirileceğini, ancak geri yükleme işlemi daha hızlıdır.  | ![Evet][green] 
Artımlı yedeklemeyi çalıştırma |![Evet][green] |![Evet][green] |![Evet][green] 
Yinelenenleri kaldırılmış diskleri yedekleme | | | ![Kısmi][yellow]<br/><br/> DPM/MABS sunucuları yalnızca şirket içi dağıtıldı. 

![Tablo anahtarı](./media/backup-architecture/table-key.png)

## <a name="architecture-direct-backup-of-azure-vms"></a>Mimari: Azure sanal makinelerinin doğrudan yedekleme

1. Bir Azure VM için yedeklemeyi etkinleştirdiğinizde, belirttiğiniz zamanlamaya göre yedeklemeyi başlatır.
1. Sanal makine çalışıyorsa ilk yedekleme sırasında sanal makinede yedekleme uzantısı yüklenir.
    - Windows Vm'leri için VMSnapshot uzantısı yüklenir.
    - Linux VM'ler için VMSnapshot Linux uzantısı yüklenir.
1. Uzantı, depolama düzeyinde anlık görüntüsünü alır. 
    - Windows çalıştıran VM'ler için yedekleme, Windows Birim Gölge Kopyası Hizmeti (sanal makinenin uygulama ile tutarlı bir anlık görüntüsünü almak için VSS ile) düzenler. Varsayılan olarak, VSS yedeklemeleri tam yedekleme gerçekleştirir. Ardından yedekleme uygulamayla tutarlı bir anlık görüntüsünü almak kuramazsa, bir dosya tutarlı anlık görüntü alır.
    - Linux VM'ler için yedekleme dosya tutarlı anlık görüntü alır. Uygulamayla tutarlı anlık görüntüler için el ile ön/son betik özelleştirmeniz gerekir.
    - Yedekleme, paralel her VM disk yedekleyerek en iyi duruma getirilmiştir. Azure Backup, yedeklenmekte olan her bir disk için disk üzerindeki blokları okur ve yalnızca değiştirilen verileri depolar. 
1. Anlık görüntünün alınma sonra veriler kasaya aktarılır. 
    - Yalnızca kopyalanıp kopyalanmayacağını son yedeklemeden beri değiştirilen veri bloklarını.
    - Verileri şifreli değil. Azure Backup, Azure Disk şifrelemesi kullanılarak şifrelenmiş Azure Vm'leri yedekleyebilir.
    - Anlık görüntü verileri hemen kasaya kopyalanmaması. Yoğun saatlerde yedekleme bazı saat sürebilir. Bir VM için toplam yedek süresi 24 saatten daha kısa bir süre için günlük yedekleme ilkelerini olacaktır.
1. Veriler kasaya gönderildikten sonra anlık görüntü kaldırılır ve bir kurtarma noktası oluşturulur.

Azure Vm'leri için denetim komutlarını internet erişimi gerekir. İş yükleri (örneğin, SQL Server veritabanı yedeklemeleri) VM içindeki yedekliyorsanız, arka uç veri ayrıca internet erişimi gerekir. 

![Azure VM yedekleme](./media/backup-architecture/architecture-azure-vm.png)

## <a name="architecture-direct-backup-of-on-premises-windows-server-machines-or-azure-vm-files-or-folders"></a>Mimari: Doğrudan yedekleme, şirket içi Windows Server makineleri, Azure VM dosyaları veya klasörleri

1. Senaryoyu ayarlamak için indirme ve MARS Aracısı makinesine yükleyin. Ardından Nelerin yedekleneceğini, yedekleme ne zaman çalışacağını ve ne kadar Azure'da saklanması seçin.
1. İlk yedeklemeyi yedekleme ayarlarınıza göre çalıştırır.
1. MARS Aracısı, yedekleme için seçilen birimlerin zaman içinde nokta anlık görüntüsünü almak için VSS kullanır.
    - MARS Aracısı, anlık görüntü yakalamak için yalnızca Windows sistemi yazma işlemi kullanır.
    - Aracı, herhangi bir uygulama VSS yazıcılarını kullanmadığı, uygulamayla tutarlı anlık görüntüleri yakalamaz.
1. VSS anlık görüntü aldıktan sonra MARS aracısının yedekleme yapılandırıldığında, belirtilen önbellek klasöründe bir sanal sabit disk (VHD) oluşturur. Aracı, ayrıca her veri bloğu için sağlama toplamları depolar.
1. Geçici yedekleme çalıştırmadığınız sürece artımlı yedeklemeler belirttiğiniz zamanlamaya göre çalıştırın.
1. Artımlı yedeklemeler, değiştirilmiş dosyalar tanımlanır ve yeni bir VHD oluşturulur. VHD sıkıştırılır ve şifrelenir ve ardından kasaya gönderilir.
1. Artımlı Yedekleme tamamlandıktan sonra yeni VHD ilk çoğaltmadan sonra oluşturulan VHD ile birleştirilir. Bu birleştirilmiş VHD devam eden yedekleme için karşılaştırma için kullanılacak en son durumu sağlar.

![Şirket içi Windows Server makineleri MARS Aracısı ile yedekleme](./media/backup-architecture/architecture-on-premises-mars.png)

## <a name="architecture-back-up-to-dpmmabs"></a>Mimari: DPM/MABS yedekleme

1. Korumak istediğiniz makinelerde'de DPM veya MABS koruma aracısını yükleyin. Ardından DPM koruma grubu için makineleri ekleyin.
    - Şirket içi makineleri korumak için DPM veya MABS sunucuları şirket içinde olması gerekir.
    - Azure Vm'leri korumak için MABS sunucunun bir Azure VM olarak çalışan azure'da bulunması gerekir.
    - / MABS, DPM yedekleme birimleri, paylaşımları, dosyaları ve klasörleri koruyabilirsiniz. Ayrıca bir makinenin sistem durumu (tam) koruyabilir ve belirli uygulamaları ile uygulama durumunu algılayan Yedekleme ayarlarını koruyabilirsiniz.
1. Bir makine ya da DPM/MABS uygulamada için koruma ayarlama, MABS/DPM yerel diske kısa vadeli depolama için ve çevrimiçi koruma için Azure'ı yedeklemek için seçin. Ayrıca Yedekleme yerel DPM/MABS depolama için ne zaman çalışması gerektiğini ve Azure çevrimiçi yedeklemeyi ne zaman çalışması gerektiğini belirtin.
1. Korunan iş yükü disk belirttiğiniz zamanlamaya göre yerel MABS/DPM diskleri yedeklenir.
4. DPM/MABS diskleri kasaya DPM/MABS sunucu üzerinde çalıştığını MARS aracısı tarafından yedeklenir.

![Makineleri ve DPM veya MABS ile korunan iş yüklerini yedekleme](./media/backup-architecture/architecture-dpm-mabs.png)

## <a name="azure-vm-storage"></a>Azure VM depolama

Azure sanal makineleri, kendi işletim sistemini, uygulamalarını ve verilerini depolamak için diskleri kullanır. Her Azure VM en az iki diskin vardır: bir disk için işletim sistemi ve geçici disk. Azure sanal makineleri, veri diskleri için uygulama verilerini de içerebilir. Disk VHD olarak depolanır.

- VHD'ler, azure'daki standart veya premium depolama hesaplarındaki sayfa blobları olarak depolanır:
    - **Standart Depolama:** Gecikme süresi için hassas olmayan iş yükleri çalıştıran VM'ler için güvenilir, düşük maliyetli disk desteği. Standart depolama, standart katı hal sürücüsü (SSD) diskleri veya standart sabit disk sürücüsü (HDD) diskleri kullanabilirsiniz.
    - **Premium Depolama:** Yüksek performanslı disk desteği. Premium SSD diskleri kullanır.
- Diskler için farklı performans katmanı vardır:
    - **Standart HDD disk:** HDD'ler ile desteklenir ve düşük maliyetli depolama için kullanılır.
    - **Standart SSD disk:** Premium SSD ve HDD disklerle standart öğelerini birleştirir. Daha tutarlı performans ve güvenilirlik daha HDD, ancak yine de uygun maliyetli sunar.
    - **Premium SSD disk:** SSD'ler ile desteklenir ve g/Ç açısından yoğun iş yükleri çalıştıran VM'ler için yüksek performanslı ve düşük gecikme süresi sağlar.
- Diskler yönetilen veya yönetilmeyen:
    - **Yönetilmeyen diskler:** Diskler, VM'ler tarafından kullanılan geleneksel türdeki. Bu diskler için kendi depolama hesabı oluşturma ve disk oluşturduğunuzda, bunu belirtmeniz gerekir. Ardından, sanal makineleriniz için depolama kaynaklarını en iyi şekilde nasıl ekleyeceğimi gerekir.
    - **Yönetilen diskler:** Azure, oluşturur ve depolama hesapları sizin yerinize yönetir. Disk boyutuna, performans katmanına belirtin ve Azure yönetilen diskleri sizin için oluşturur. Disk ekleyin ve VM'yi ölçeklendirme gibi Azure depolama hesapları işler.

Sanal makineler için disk depolama alanı ve kullanılabilir disk türleri hakkında daha fazla bilgi için şu makalelere bakın:

- [Windows VM'ler için Azure yönetilen diskler](../virtual-machines/windows/managed-disks-overview.md)
- [Linux VM'ler için Azure yönetilen diskler](../virtual-machines/linux/managed-disks-overview.md)
- [Sanal makineler için kullanılabilir disk türleri](../virtual-machines/windows/disks-types.md)

### <a name="back-up-and-restore-azure-vms-with-premium-storage"></a>Yedekleme ve premium depolama ile Azure Vm'lerini geri yükleme 

Azure Backup ile premium Depolama'yı kullanarak Azure sanal makinelerini yedekleyebilirsiniz:

- Premium depolama ile Vm'leri yedekleme işlemi sırasında Backup hizmeti adlı geçici bir hazırlama konumu oluşturur. *AzureBackup -*, depolama hesabındaki. Hazırlama konumunun boyutu, kurtarma noktası anlık görüntüsünü boyutuna eşittir.
- Premium depolama hesabında geçici hazırlama konumu barındırmak için yeterli boş alan olduğundan emin olun. [Daha fazla bilgi edinin](../storage/common/storage-scalability-targets.md#premium-performance-storage-account-scale-limits). Hazırlama konumunu değiştirmeyin.
- Yedekleme işi tamamlandıktan sonra hazırlama konumu silinir.
- Hazırlama konumu için kullanılan depolama alanının fiyatı tutarlıdır [premium depolama fiyatlandırması](../virtual-machines/windows/disks-types.md#billing).

Premium Depolama'yı kullanarak Azure Vm'leri geri yüklerken, bunları premium veya standart depolama alanına geri yükleyebilirsiniz. Genellikle, bunları premium depolama birimine geri yüklenebilir. Ancak VM'den dosyaların yalnızca bir kısmı gerekiyorsa, bunları standart Depolama'ya geri yüklenmesi uygun maliyetli olabilir.

### <a name="back-up-and-restore-managed-disks"></a>Yedekleme ve geri yükleme yönetilen diskler

Yönetilen diskler ile Azure sanal makinelerini yedekleyebilirsiniz:

- Vm'leri yönetilen disklerle aynı şekilde bunu herhangi bir Azure VM yedekleme. VM'yi doğrudan sanal makine ayarlarından yedekleme veya kurtarma Hizmetleri Kasası'nda sanal makineler için yedeklemeyi etkinleştirebilirsiniz.
- Yönetilen diskler üzerindeki VM'ler, yönetilen diskler üzerinde oluşturulmuş RestorePoint koleksiyonları ile yedeklenebilir.
- Azure Backup ayrıca Azure Disk şifrelemesi kullanılarak şifrelenmiş yönetilen disklere sahip sanal makineleri yedeklemeye destekler.

Vm'leri yönetilen disklerle geri yüklediğinizde, yönetilen disklerle tüm VM'yi veya bir depolama hesabına geri yükleyebilirsiniz:

- Geri yükleme işlemi sırasında yönetilen disk işlemleri Azure gerçekleştirir. Depolama hesabı seçeneği kullanıyorsanız, geri yükleme işlemi sırasında oluşturduğunuz depolama hesabını yönetin.
- Şifreli yönetilen bir sanal makine geri yüklerseniz, sanal makinenin anahtarları emin olun ve geri yükleme işlemi başlamadan önce gizli anahtar kasasında mevcut.

## <a name="next-steps"></a>Sonraki adımlar

- Gözden geçirme için destek matrisi [desteklenen özellikler ve sınırlamalar için yedekleme senaryoları hakkında bilgi edinin](backup-support-matrix.md).
- Bu senaryolardan biri, yedekleme ayarlayın:
    - [Azure Vm'lerini yedekleme](backup-azure-arm-vms-prepare.md).
    - [Doğrudan Windows makinelerini yedekleme](tutorial-backup-windows-server-to-azure.md), bir yedek sunucu olmadan.
    - [MABS kümesi](backup-azure-microsoft-azure-backup.md) yedekleme Azure ve ardından Yedekleme iş yükleri için MABS için.
    - [DPM ayarlama](backup-azure-dpm-introduction.md) Azure'a ve ardından iş yüklerini yedekleme DPM yedekleme.


[green]: ./media/backup-architecture/green.png
[yellow]: ./media/backup-architecture/yellow.png
[red]: ./media/backup-architecture/red.png

