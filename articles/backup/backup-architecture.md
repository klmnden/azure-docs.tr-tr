---
title: Azure Backup mimarisi
description: Mimari, bileşenler ve işlemler Azure Backup hizmeti tarafından kullanılan genel bir bakış sağlar.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 01/15/2019
ms.author: raynew
ms.openlocfilehash: 4f26c805c42f027409127232fcfef9840939e8d9
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/16/2019
ms.locfileid: "56329192"
---
# <a name="azure-backup-architecture"></a>Azure Backup mimarisi

Kullanabileceğiniz [Azure Backup hizmeti](backup-overview.md) verilerini Microsoft Azure bulutuna yedekleyebilirsiniz. Bu makalede, Azure Backup mimarisi, bileşenler ve işlemler özetlenir. 


## <a name="what-does-azure-backup-do"></a>Azure Backup ne yapar?

Azure yedekleme verileri, makine durumunu ve şirket içi makinelerin ve Azure Vm'leri üzerinde çalışan iş yüklerini yedekler. Azure Backup senaryolar vardır.

- **Şirket içi makineleri yedekleyin**:
    - Doğrudan Azure Yedekleme'yi kullanarak Azure'da şirket içi makineleri yedekleyebilir.
    - System Center Data Protection Manager (DPM) veya Microsoft Azure Backup sunucusu (MABS) ile şirket içi makineleri koruyun ve ardından sırayla korumalı verileri DPM/MABS için Azure Yedekleme'yi kullanarak Azure'da yedekleyin.
- **Azure Vm'lerini yedekleme**:
    - Azure sanal makineleri doğrudan Azure yedekleme ile yedekleyebilirsiniz.
    - DPM veya MABS Azure'da çalışan Azure sanal makinelerini koruyun ve ardından sırayla korumalı verileri Azure yedekleme ile DPM/MABS verileri yedekleyin.

Daha fazla bilgi edinin [neleri yedekleyebilir](backup-overview.md), ve [yedekleme senaryoları desteklenen](backup-support-matrix.md).


## <a name="where-is-data-backed-up"></a>Burada veri yedeklenir mi?

Kurtarma Hizmetleri kasasındaki verileri Azure yedekleme depoları yedeklendi. Bir kasası azure'da yedek kopyalar, Kurtarma noktaları ve yedekleme ilkeleri gibi verileri tutmak için kullanılan bir çevrimiçi depolama varlıktır.

- Kurtarma Hizmetleri kasaları, yedekleme verilerinizi düzenlemeyi kolaylaştırırken yönetim zorluklarını da en aza indirir.
- Her Azure aboneliği, en çok 500 kurtarma Hizmetleri kasası oluşturabilirsiniz. 
- Şirket içi ve Azure sanal makineler dahil olmak üzere, bir kasada yedeklenen öğeleri izleyebilirsiniz.
- Azure kasası erişimi yönetebilirsiniz [rol tabanlı erişim denetimi (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal).
- Kasadaki veri yedekliliği nasıl çoğaltıldığını belirtirsiniz:
    - **LRS**: Yerel olarak yedekli depolama (LRS), bir veri merkezinde hatalarına karşı koruduğu için kullanabilirsiniz. LRS, verileri bir depolama Ölçek birimine çoğaltır. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/storage/common/storage-redundancy-lrs).
    - **GRS**: Coğrafi olarak yedekli depolama (GRS) kullanabilirsiniz: Bölge çapında kesintilerden karşı korur. Bu, verilerinizi ikincil bölgeye çoğaltır. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/storage/common/storage-redundancy-grs). 
    - Varsayılan olarak, GRS yedekleme için kurtarma Hizmetleri kasalarını kullanın. 



## <a name="how-does-azure-backup-work"></a>Azure Backup nasıl çalışır?

Yedekleme işlerinin varsayılan değer temelinde veya özelleştirilmiş yedekleme İlkesi, azure yedekleme çalıştırır. Şekilde, Azure Yedekleme'de yedekleme gerçekleştirir, senaryoya bağlıdır.

**Senaryo** | **Ayrıntılar** 
--- | ---
**Doğrudan şirket içi makineleri yedekleme** | Doğrudan şirket içi makineleri yedeklemek için Microsoft Azure kurtarma Hizmetleri (MARS) aracısı Azure Backup'ı kullanır. Aracı, yedeklemek istediğiniz her makineye yüklenir. <br/><br/> Bu yedekleme türünü, şirket içi Linux makineler için kullanılamaz. 
**Doğrudan Azure Vm'lerini yedekleme** | Doğrudan Azure Vm'lerini yedekleme için bir Azure VM uzantısı sanal makinede ilk kez çalıştırmaları yedekleme sanal makine için yüklenir. 
**Makineleri ve DPM veya MABS tarafından korunan uygulamalar yedekleyin** | Bu senaryoda, makine/uygulama için DPM veya MABS yerel depolama ilk yedeklenir. Ardından, DPM/MABS verileri kasaya Azure Backup tarafından yedeklenir. DPM/şirket içinde çalışan MABS tarafından şirket içi makineler korunabilir. Azure sanal makineleri, Azure'da çalışan DPM/MABS tarafından korunabilir.

[Genel bakışın](backup-overview.md)görüp [nelerin desteklendiği](backup-support-matrix.md) her senaryo için.


### <a name="backup-agents"></a>Yedekleme aracıları

Azure Backup, yedekleme türüne bağlı olarak farklı aracılar sağlar.

**Aracı** | **Ayrıntılar** 
--- | --- 
**Microsoft Azure kurtarma Hizmetleri (MARS) aracısı** | Bu aracı, dosyaları, klasörleri ve sistem durumu yedekleme için tek tek şirket içi Windows sunucularında çalışır<br/><br/> Bu aracı, DPM/MABS yerel depolama diski yedeklemek için DPM/MABS sunucuları üzerinde çalışır. Makineleri ve uygulamaları yerel olarak bu DPM/MABS diske yedeklenir.
**Azure VM uzantısı** | Azure Vm'lerini yedekleme için VM'ler üzerinde çalışan Azure VM Aracısı bir yedek Dahili hat eklenir. 


## <a name="backup-types"></a>Yedekleme türleri

**Yedekleme türü** | **Ayrıntılar** | **Kullanım**
--- | --- | ---
**Tam** | Yedek veri kaynağının tamamını içerir.<br/><br/> Tam yedekleme, daha fazla ağ bant genişliğini alır. | İlk yedekleme için kullanılır.
**Fark** |  İlk tam yedeklemeden sonra değiştirilen blokları depolar. Daha küçük miktarda ağ ve depolama alanı kullanır ve, değişmeyen verilerin gereksiz kopyalarını korumaz.<br/><br/> Sonraki yedeklerin arasında değiştirilmeyen veri blokları aktarıldığı ve depolandığı için yetersiz. | Azure Backup tarafından kullanılmaz.
**Artımlı** | Yüksek depolama ve ağ verimliliği. Yalnızca önceki yedeklemeden itibaren değişmiş olan veri bloklarını depolar. <br/><br/> Gerekli olmadığı için artımlı yedekleme ile tam yedeklemeler için gerek yoktur. | Disk yedekleme için DPM/MABS tarafından kullanılan ve azure'a tüm yedeklemeler kullanılır.

### <a name="comparison"></a>Karşılaştırma

Depolama alanı tüketimi, Kurtarma süresi hedefi (RTO) ve ağ tüketimi yedekleme her tür için farklılık gösterir. Aşağıdaki görüntüde, Yedekleme türleri bir karşılaştırmasını gösterir.
- Veri kaynağı A 10 depolama oluşur, aylık yedeklenen A1-A10 engeller.
- A2, A3, A4 ve A9 blokları ilk ayda, A5 bloku ise sonraki ayda değişmektedir.
- Farklı yedeklemeler için ikinci ayda değiştirilen A2, A3, A4 ve A9 blokları yedeklenir. Üçüncü ayda değiştirilen A5 blokuna ek olarak aynı bloklar tekrar yedeklenir. Sonraki tam yedeklemeye kadar değiştirilen bloklar yedeklenmeye devam eder.
- İlk ay tam yedekleme aldıktan sonra artımlı yedeklemeler için olarak işaretlenmiş A2, A3, A4 ve A9 blokları değiştirilmiş ve ikinci aya aktarılır. Üçüncü ayda yalnızca değiştirilen A5 bloku işaretlenir ve aktarılır. 

![yedekleme yöntemlerinin karşılaştırmasını gösteren görüntü](./media/backup-architecture/backup-method-comparison.png)

## <a name="backup-features"></a>Yedekleme özellikleri

Aşağıdaki tabloda farklı yedekleme türleri için özellikler özetlenmektedir.

**Özellik** | **Şirket içi Windows makineleri (doğrudan)** | **Azure Vm'leri** | **DPM/MABS ile makineleri/uygulamaları**
--- | --- | --- | ---
Kasa için yedekleme | ![Evet][green] | ![Evet][green] | ![Evet][green] 
DPM/MABS disk ardından Azure yedekleme | | | ![Evet][green] 
Yedekleme için gönderilen veri sıkıştırma | ![Evet][green] | Veri aktarımı sıkıştırma kullanılır. Depolama biraz şişirileceğini, ancak geri yükleme işlemi daha hızlıdır.  | ![Evet][green] 
Artımlı yedeklemeyi çalıştırma |![Evet][green] |![Evet][green] |![Evet][green] 
Yinelenenleri kaldırılmış diskleri yedekleme | | | ![Kısmi][yellow]<br/><br/> DPM/MABS sunucuları yalnızca şirket içi dağıtıldı. 

![tablo anahtarı](./media/backup-architecture/table-key.png)


## <a name="architecture-direct-backup-of-on-premises-windows-machines"></a>Mimari: Doğrudan yedekleme, şirket içi Windows makineler

1. Senaryonun kurulumunu yapmak için indirin ve makineye Microsoft Azure kurtarma Hizmetleri (MARS) aracısı yükleyin, nelerin yedekleneceğini, yedekleme ne zaman çalışacağını ve ne kadar Azure'da saklanması seçin.
2. İlk yedekleme, yedekleme ayarlarına uygun şekilde çalışır.
3. MARS Aracısı, yedekleme için seçilen birimlerin zaman içinde nokta anlık görüntüsünü almak için Windows birim gölge kopyası (VSS) hizmeti kullanır.
    - MARS Aracısı yalnızca Windows sistemi yazma anlık görüntü yakalamak için kullanır.
    - Aracı, herhangi bir uygulama VSS yazıcılarını kullanmaz ve bu nedenle uygulamayla tutarlı anlık görüntüleri yakalamaz.
3. VSS anlık görüntü aldıktan sonra MARS Aracısı VHD yedekleme yapılandırıldığında, belirtilen önbellek klasöründe oluşturur ve her veri bloğu için sağlama toplamları depolar. 
4. Geçici yedekleme çalıştırmadığınız sürece artımlı yedeklemeler belirttiğiniz zamanlamaya uygun olarak çalıştırın.
5. Artımlı yedeklemeler, değiştirilmiş dosyalar tanımlanır ve yeni bir VHD oluşturulur. Bu sıkıştırılmış ve şifrelenmiş ve kasaya gönderilir.
6. Artımlı Yedekleme tamamlandıktan sonra yeni VHD için devam eden yedekleme karşılaştırma için kullanılacak en son durumu sağlama ilk çoğaltmadan sonra oluşturulan VHD ile birleştirilir. 

![MARS Aracısı ile şirket içi Windows Server Yedekleme](./media/backup-architecture/architecture-on-premises-mars.png)


## <a name="architecture-direct-backup-of-azure-vms"></a>Mimari: Azure sanal makinelerinin doğrudan yedekleme

1. Bir Azure VM için yedeklemeyi etkinleştirdiğinizde, belirttiğiniz zamanlamaya uygun olarak bir yedekleme çalıştırır.
2. Çalışıyorsa ilk yedekleme sırasında VM üzerinde bir yedekleme uzantısı yüklenir.
    - Windows VM'ler için VMSnapshot uzantısı yüklenir.
    - Linux Vm'leri için VMSnapshot Linux uzantısı yüklenir.
3. Uzantı depolama düzeyi anlık görüntüsünü alır. 
    - Windows çalıştıran VM'ler için yedekleme, sanal makinenin uygulama ile tutarlı bir anlık görüntüsünü almak için VSS ile düzenler. Varsayılan olarak, VSS yedeklemeleri tam yedeklemesini alır. Ardından yedekleme uygulamayla tutarlı bir anlık görüntüsünü almak kuramazsa, bir dosya tutarlı anlık görüntü alır.
    - Linux sanal makineleri yedekleme için dosya tutarlı yedeklemesini alır. Uygulamayla tutarlı anlık görüntüler için el ile ön/son betik özelleştirmeniz gerekir.
    - Yedekleme, paralel her VM disk yedekleyerek en iyi duruma getirilmiştir. Azure Backup, yedeklenmekte olan her bir disk için disk üzerindeki blokları okur ve yalnızca değiştirilen verileri depolar. 
4. Anlık görüntünün alınma sonra veriler kasaya aktarılır. 
    - Yalnızca kopyalanıp kopyalanmayacağını son yedeklemeden bu yana değişmiş olan veri bloklarını.
    - Verileri şifreli değil. Azure Backup, Azure Disk şifrelemesi (ADE) kullanılarak şifrelenmiş Azure sanal makinelerini yedekleyebilirsiniz.
    - Anlık görüntü verileri hemen kasaya kopyalanmaması. Bu, yoğun saatlerde bazı saat sürebilir. Bir VM için toplam yedek saat, 24 saat için günlük yedekleme ilkelerini daha az olacaktır.
5. Verileri kasaya gönderildikten sonra anlık görüntü kaldırılır ve bir kurtarma noktası oluşturulur.


![Azure VM yedekleme](./media/backup-architecture/architecture-azure-vm.png)


## <a name="architecture-back-up-to-dpmmabs"></a>Mimari: DPM/MABS yedekleme

1. Korumak istediğiniz makinelere DPM veya MABS koruma aracısını yükleyin ve DPM koruma grubu için makineleri ekleyin.
    - Şirket içi makineleri korumak için DPM veya MABS sunucuları şirket içinde olması gerekir.
    - Azure Vm'leri korumak için DPM veya MABS sunucuları bir Azure VM olarak çalışan azure'da bulunması gerekir.
    - DPM/MABS kullanarak geri birimler, paylaşımlar, dosya ve klasör yedeklemek koruyabilirsiniz. Makine sistem durumu/tam koruma ve uygulama durumunu algılayan Yedekleme ayarlarını belirli uygulamalarla koruyun.
2. Bir makine veya uygulamasına dpm'de koruma ayarladığınızda, DPM yerel diske kısa vadeli depolama için ve (çevrimiçi korumadan) azure'a yedeklemek için seçin. Ayrıca Yedekleme yerel DPM/MABS depolama için ne zaman çalışması gerektiğini ve Azure çevrimiçi yedeklemeyi ne zaman çalışması gerektiğini belirtin.
3. Korunan iş yükü disk yerel DPM disk ve Azure, belirttiğiniz zamanlamaya uygun olarak yedeklenir.
4. Çevrimiçi Yedekleme kasasına DPM/MABS sunucu üzerinde çalıştığını MARS aracısı tarafından işlenebilir.

![DPM veya MABS ile korunan makineler/iş yüklerinin yedeklenmesi](./media/backup-architecture/architecture-dpm-mabs.png)







## <a name="azure-vm-storage"></a>Azure VM depolama

- Azure sanal makineleri, kendi işletim sistemini, uygulamalarını ve verilerini depolamak için diskleri kullanır.
- Azure VM'ler, en az iki diskin sahiptir. Bir işletim sistemi ve geçici disk için. Bunlar, veri diskleri için uygulama verilerini de içerebilir. Disk VHD olarak depolanır.
- VHD'ler, azure'daki standart veya premium depolama hesaplarındaki sayfa blobları olarak depolanır.
    - Standart Depolama: Gecikme süresi için hassas olmayan iş yükleri çalıştıran VM'ler için güvenilir, düşük maliyetli disk desteği. Standart depolama, standart SSD disk veya standart HDD diskler kullanabilirsiniz.
    - Premium Depolama: Yüksek performanslı disk desteği. Premium SSD diskleri kullanır.
- Diskler için farklı performans katmanı vardır:
    - Standart HDD disk: HDD'ler ile desteklenir ve düşük maliyetli depolama için kullanılır.
    - Standart SSD disk: Öğesinin premium SSD ve daha tutarlı performans ve güvenilirlik daha HDD, ancak yine de uygun maliyetli sunan standart HDD disklerle birleştirir.
    - Premium SSD disk: SSD, g/ç kullanımlı iş yükleri çalıştıran VM'ler için yüksek performanslı ve düşük gecikme süresi sağlayan tarafından desteklenir.
- Diskler yönetilen veya yönetilmeyen:
    - Yönetilmeyen diskler: Diskler, VM'ler tarafından kullanılan geleneksel türleri. Bu diskler için kendi depolama hesabı oluşturma ve disk oluşturduğunuzda, bunu belirtmeniz gerekir. Vm'leriniz için depolama kaynaklarını en üst düzeye çıkarmak nasıl anlamanız gerekir.
    - Yönetilen diskler: Azure, oluşturma ve sizin için depolama hesaplarını yönetme işler. Disk boyutuna, performans katmanına belirtin ve Azure oluşturur, diskleri sizin için yönetebilir. Azure depolama, disk ekleyin ve VM'yi ölçeklendirme gibi işler.

Daha fazla bilgi edinin:

- Disk depolama hakkında daha fazla bilgi [Windows](../virtual-machines/windows/managed-disks-overview.md) ve [Linux](../virtual-machines/linux/managed-disks-overview.md) VM'ler.
- Kullanılabilir hakkında bilgi edinin [disk türleri](../virtual-machines/windows/disks-types.md) gibi standart ve premium.


### <a name="backing-up-and-restoring-azure-vms-with-premium-storage"></a>Yedekleme ve premium depolama ile Azure Vm'lerini geri yükleme 

Azure Backup ile premium depolama kullanarak Azure Vm'leri yedekleyebilir:

- Premium depolama Vm'lerini yedeklerken, Backup hizmeti "AzureBackup-" adlı bir depolama hesabında geçici hazırlama konumu oluşturur. Hazırlama konumunun boyutu, kurtarma noktası anlık görüntüsünü boyutuna eşittir.
- Premium depolama hesabında geçici hazırlama konumu barındırmak için yeterli boş alan olduğundan emin olun. [Daha fazla bilgi edinin](../storage/common/storage-scalability-targets.md#premium-storage-account-scale-limits). Hazırlama konumunu değiştirmeyin.
- Yedekleme işi tamamlandıktan sonra hazırlama konumu silinir.
- Hazırlama konumu için kullanılan depolama alanının fiyatı tutarlıdır [premium depolama fiyatlandırması](../virtual-machines/windows/disks-types.md#billing).

Azure premium depolama kullanarak Vm'leri geri yüklediğinizde, bunları premium veya standart depolama alanına geri yükleyebilirsiniz. Premium genellikle geri yüklenebilir, ancak yalnızca VM'den dosyaların bir alt kümesine ihtiyacınız varsa standart geri yüklemek için uygun maliyetli olabilir.

### <a name="backing-up-and-restoring-managed-disks"></a>Yönetilen diskleri yedekleme ve geri yükleme

Yönetilen diskler ile Azure sanal makinelerini yedekleyebilirsiniz.
- Vm'leri yönetilen disklerle aynı şekilde bunu herhangi bir Azure VM yedekleme. VM'yi doğrudan sanal makine ayarlarından yedekleme veya kurtarma Hizmetleri Kasası'nda sanal makineler için yedeklemeyi etkinleştirebilirsiniz.
- Yönetilen diskler üzerindeki VM'ler, yönetilen diskler üzerinde oluşturulmuş RestorePoint koleksiyonları ile yedeklenebilir.
- Azure Backup, yönetilen disk Vm'lerinin Azure Disk şifrelemesi (ADE) kullanılarak şifrelenmiş yedekleme de destekler.

Vm'leri yönetilen disklerle geri yüklediğinizde, yönetilen disklerle tüm VM'yi veya bir depolama hesabına geri yükleyebilirsiniz.
- Azure yönetilen diskler geri yükleme işlemi sırasında işleme ve depolama hesabı seçeneği ile geri yükleme sırasında oluşturduğunuz depolama hesabını yönetin.
- Şifreli yönetilen bir sanal makine geri yüklerseniz geri yükleme işlemine başlamadan önce VM anahtarları ve gizli anahtar Kasası'nda çıkmalıdır.




## <a name="next-steps"></a>Sonraki adımlar

- [Gözden geçirme](backup-support-matrix.md) destek matrisini desteklenen özellikler ve sınırlamalar için yedekleme senaryoları hakkında bilgi edinin.
- Yedekleme senaryoları biri için ayarlayın:
    - [Azure VM'lerini yedekleme](backup-azure-arm-vms-prepare.md)
    - [Doğrudan Windows makinelerini yedekleme](tutorial-backup-windows-server-to-azure.md), bir yedek sunucu olmadan.
    - [MABS kümesi](backup-azure-microsoft-azure-backup.md) yedekleme Azure ve ardından Yedekleme iş yükleri için MABS için.
    - [DPM ayarlama](backup-azure-dpm-introduction.md) Azure'a ve ardından iş yüklerini yedekleme DPM yedekleme.


[green]: ./media/backup-architecture/green.png
[yellow]: ./media/backup-architecture/yellow.png
[red]: ./media/backup-architecture/red.png

