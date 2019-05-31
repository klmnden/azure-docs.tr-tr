---
title: Azure Site Recovery için devam eden Azure'dan Azure'a çoğaltma sorunlarını giderme | Microsoft Docs
description: Olağanüstü durum kurtarma için Azure sanal makineleri çoğaltırken hatalarını ve sorunlarını giderme
services: site-recovery
author: asgang
manager: rochakm
ms.service: site-recovery
ms.topic: troubleshooting
ms.date: 11/27/2018
ms.author: asgang
ms.openlocfilehash: bf24b2d1395e128dc73361670ea93ac938574146
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66258778"
---
# <a name="troubleshoot-ongoing-problems-in-azure-to-azure-vm-replication"></a>Azure'dan Azure'a VM çoğaltması devam eden sorunlarını giderme

Çoğaltma ve Azure sanal makineleri bir bölgesinden başka bir bölgeye kurtarma Bu makale Azure Site recovery'de yaygın sorunları açıklar. Ayrıca, bunları nasıl giderebileceğinizden açıklar. Desteklenen yapılandırmalar hakkında daha fazla bilgi için bkz. [Azure Vm'lerini çoğaltma için destek matrisi](site-recovery-support-matrix-azure-to-azure.md).

Azure Site Recovery, tutarlı bir şekilde veri kaynak bölgesi olağanüstü durum kurtarma bölgeye çoğaltır. ve 5 dakikada bir kilitlenme ile tutarlı kurtarma noktası oluşturur. Site Recovery kurtarma noktaları için 60 dakika oluşturamıyorsanız, bu bilgileri kullanarak uyarıları:

Hata iletisi: "Kilitlenme tutarlı kurtarma noktası yok son 60 dakika içinde VM için kullanılabilir."</br>
Hata Kimliği: 153007 </br>

Aşağıdaki bölümlerde, nedenler ve çözümler açıklanmaktadır.

## <a name="high-data-change-rate-on-the-source-virtal-machine"></a>Kaynak sanal makinedeki yüksek veri değişim hızı
Azure Site Recovery, kaynak sanal makinede veri değişim hızı desteklenen sınırları yüksek ise bir olay tetikler. Sorunu nedeniyle yüksek değişim sıklığı olup olmadığını denetlemek için Git **çoğaltılan öğeler** > **VM** > **olaylar-son 72 saat**.
"Veri hızı desteklenen sınırları aşıyor Değiştir" olay görmeniz gerekir:

![data_change_rate_high](./media/site-recovery-azure-to-azure-troubleshoot/data_change_event.png)

Olay seçerseniz, tam disk bilgileri görmeniz gerekir:

![data_change_rate_event](./media/site-recovery-azure-to-azure-troubleshoot/data_change_event2.png)


### <a name="azure-site-recovery-limits"></a>Azure Site Recovery limitleri
Aşağıdaki tablo, Azure Site Recovery sınırlarını sağlar. Bu limitler yaptığımız testleri temel temel alır, ancak bunlar tüm olası uygulama g/ç birleşimlerini kapsamamaktadır. Gerçek sonuçlar, uygulamanızın G/Ç karışımına göre değişebilir. 

Dikkate alınması gereken iki sınır, disk başına veri değişim sıklığı ve sanal makine başına veri değişim sıklığı vardır. Örneğin, aşağıdaki tabloda P20 Premium disk göz atalım. Site Recovery, 5 MB/sn değişim disk başına en fazla beş tür diskler, VM başına 25 MB/sn'lik toplam değişim sıklığı, VM sınırı nedeniyle başa çıkabilir.

**Çoğaltma depolama hedefi** | **Kaynak disk için ortalama g/ç boyutu** |**Kaynak disk için ortalama veri değişim sıklığı** | **Kaynak veri diski için günlük toplam veri değişim sıklığı**
---|---|---|---
Standart depolama | 8 KB | 2 MB/sn | Disk başına 168 GB
Premium P10 veya P15 disk | 8 KB  | 2 MB/sn | Disk başına 168 GB
Premium P10 veya P15 disk | 16 KB | 4 MB/sn |  Disk başına 336 GB
Premium P10 veya P15 disk | 32 KB veya daha büyük | 8 MB/sn | Disk başına 672 GB
Premium P20 veya P30 veya P40 veya P50 disk | 8 KB    | 5 MB/sn | Disk başına 421 GB
Premium P20 veya P30 veya P40 veya P50 disk | 16 KB veya daha büyük |10 MB/sn | Disk başına 842 GB

### <a name="solution"></a>Çözüm
Azure Site Recovery veri değişim hızı, disk türüne göre üzerinde limitleri vardır. Bu sorunun yinelenen veya kopan olup olmadığını öğrenmek için veri değişikliği oranı etkilenen sanal makinenin bulun. Kaynak sanal makineye gidin, ölçümleri altında bulabilirsiniz **izleme**, bu ekran görüntüsünde gösterildiği gibi ölçümleri ekleyin:

![Veri değişikliği hızını bulmak için üç adımlık bir işlemdir](./media/site-recovery-azure-to-azure-troubleshoot/churn.png)

1. Seçin **ölçüm Ekle**ve ekleme **işletim sistemi diski yazma bayt/sn** ve **veri diski yazma bayt/sn**.
2. Depo, ekran görüntüsünde gösterildiği gibi izleyin.
3. Görünüm toplam yazma işlemlerini işletim sistemi diskleri ve tüm veri diskleri birleştirilmiş arasında'olmuyor. Bu ölçüm disk başına düzeyinde bilgi sağlamayabilir, ancak bunlar toplam veri değişim sıklığı desenini gösterir.

Bir depo olan veri bloğu bir arada sırada veri ve veri değiştirirseniz oranıdır 10 MB/sn (Premium için) 2 MB'dan büyük ve/bazı (standart) s saat ve gelir, çoğaltma yakalar. Değişim sıklığı da desteklenen dışında olup olmadığını, ancak çoğu zaman limit, mümkünse bu seçeneklerden birini göz önünde bulundurun:

* **Bir yüksek veri değişim hızı neden olan bir diski hariç**: Kullanarak bir diski hariç tutabilirsiniz [PowerShell](./azure-to-azure-exclude-disks.md). Diski dışlamak için önce çoğaltmayı devre dışı bırakmanız gerekir. 
* **Olağanüstü Durum Kurtarma Depolama diski katmanını değiştirme**: Bu seçenek yalnızca disk veri değişim sıklığı, 10 MB/sn olması durumunda mümkündür. Bir VM ile bir P10 disk 8 MB/sn ancak 10 MB/sn'den büyük bir veri değişim sıklığı yaşıyor varsayalım. Koruma sırasında müşteri P30 disk için hedef depolama kullanabilir, sorun çözülebilir.

## <a name="Network-connectivity-problem"></a>Ağ bağlantısı sorunları

### <a name="network-latency-to-a-cache-storage-account"></a>Bir önbellek depolama hesabına ağ gecikmesi
Site Recovery, önbellek depolama hesabına çoğaltılan verileri gönderir. Bir sanal makineden önbellek depolama hesabı için verileri karşıya yükleme, ağ gecikmesi 3 saniye içinde 4 MB'den daha yavaş olup olmadığını görebilirsiniz. 

Gecikme süresi için ilgili bir sorun denetlemek için kullanmak [azcopy](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy) verileri sanal makineden önbellek depolama hesabına yüklemek için. Gecikme süresi yüksek ise vm'lerden giden ağ trafiğinizi denetlemek için bir ağ sanal Gereci (NVA) kullanıp kullanmadığınızı kontrol edin. Tüm çoğaltma trafiğinin NVA üzerinden geçerse gereç kısıtlanan. 

Çoğaltma trafiği için NVA Git değil, bir ağ hizmet uç noktaları sanal ağınızda bulunan "Depolama için" oluşturmanızı öneririz. Daha fazla bilgi için [ağ sanal Gereci Yapılandırması](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-about-networking#network-virtual-appliance-configuration).

### <a name="network-connectivity"></a>Ağ bağlantısı
Site Recovery çoğaltması için iş, giden bağlantı için özel URL veya IP aralıkları VM'den gerekli. Sanal makinenize bir güvenlik duvarının arkasındaysa ya da giden bağlantıyı denetlemek için ağ güvenlik grubu (NSG) kuralları kullanıyorsa bu sorunlarından biri, yüz tanıma. Tüm URL'leri bağlandığınızdan emin olmak için bkz: [Site kurtarma URL'ler için giden bağlantı](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-about-networking#outbound-connectivity-for-ip-address-ranges). 

## <a name="error-id-153006---no-app-consistent-recovery-point-available-for-the-vm-in-the-last-xxx-minutes"></a>Hata kimliği 153006 - kullanılabilir son 'XXX' dakika içinde VM için uygulamayla tutarlı kurtarma noktası yok

En yaygın sorunlardan bazılarını aşağıda listelenmiştir

#### <a name="cause-1-known-issue-in-sql-server-20082008-r2"></a>1. neden: Sorun SQL Server 2008/2008 R2 bilinen 
**Nasıl düzeltileceğini** : 2008/2008 R2'in SQL server ile ilgili bilinen bir sorun yoktur. Lütfen bu KB makalesinde bakın [başarısız SQL Server 2008 R2 barındıran bir sunucu için Azure Site Recovery aracısı veya diğer bileşen olmayan VSS yedekleme](https://support.microsoft.com/help/4504103/non-component-vss-backup-fails-for-server-hosting-sql-server-2008-r2)

#### <a name="cause-2-azure-site-recovery-jobs-fail-on-servers-hosting-any-version-of-sql-server-instances-with-autoclose-dbs"></a>2. neden: Herhangi bir sürümünü None veritabanları ile SQL Server örneklerini barındıran sunucularda Azure Site Recovery işleri başarısız 
**Nasıl düzeltileceğini** : KB başvuran [makale](https://support.microsoft.com/help/4504104/non-component-vss-backups-such-as-azure-site-recovery-jobs-fail-on-ser) 


#### <a name="cause-3-known-issue-in-sql-server-2016-and-2017"></a>3. neden: SQL Server 2016 ve 2017'deki bilinen sorun
**Nasıl düzeltileceğini** : KB başvuran [makale](https://support.microsoft.com/help/4493364/fix-error-occurs-when-you-back-up-a-virtual-machine-with-non-component) 

#### <a name="cause-4-you-are-using-storage-spaces-direct-configuration"></a>4. neden: Depolama alanları doğrudan yapılandırması kullanıyorsanız
**Nasıl düzeltileceğini** : Azure Site Recovery uygulamayla tutarlı kurtarma noktası için depolama alanları doğrudan yapılandırması oluşturulamıyor. Lütfen doğru makaleye başvurun [çoğaltma ilkesi yapılandırma](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-how-to-enable-replication-s2d-vms)

### <a name="more-causes-due-to-vss-related-issues"></a>Daha fazla VSS nedeni ilgili sorunlar:

Daha fazla sorun giderme için hata tam hata kodu almak için kaynak makinedeki dosyaları denetleyin:
    
    C:\Program Files (x86)\Microsoft Azure Site Recovery\agent\Application Data\ApplicationPolicyLogs\vacp.log

Dosyada hataları bulmak nasıl?
Üstteki "vacpError" vacp.log dosya bir düzenleyicide açıp arama
        
    Ex: vacpError:220#Following disks are in FilteringStopped state [\\.\PHYSICALDRIVE1=5, ]#220|^|224#FAILED: CheckWriterStatus().#2147754994|^|226#FAILED to revoke tags.FAILED: CheckWriterStatus().#2147754994|^|

Yukarıdaki örnekte **2147754994** aşağıda gösterildiği gibi hakkında bir hata bildirir hata kodu.

#### <a name="vss-writer-is-not-installed---error-2147221164"></a>VSS Yazıcı yüklü değil - hata 2147221164 

*Nasıl düzeltileceğini*: Uygulama tutarlılık etiketi oluşturmak için Azure Site Recovery, Microsoft Birim Gölge Kopyası Hizmeti (VSS) kullanır. Uygulama tutarlılığı anlık görüntülerini almak için VSS sağlayıcısı, işlemi için'nı yükler. Bu VSS sağlayıcısı, hizmet olarak yüklenir. VSS sağlayıcısı hizmeti yüklü değil durumunda, uygulama tutarlılığı anlık görüntü oluşturma "Sınıfı kaydedilmemiş" 0x80040154 hata koduyla başarısız oluyor. </br>
Başvuru [makale için VSS Yazıcı, yükleme sorunlarını giderme](https://docs.microsoft.com/azure/site-recovery/vmware-azure-troubleshoot-push-install#vss-installation-failures) 

#### <a name="vss-writer-is-disabled---error-2147943458"></a>VSS yazıcısını devre dışı - hata 2147943458

**Nasıl düzeltileceğini**: Uygulama tutarlılık etiketi oluşturmak için Azure Site Recovery, Microsoft Birim Gölge Kopyası Hizmeti (VSS) kullanır. Uygulama tutarlılığı anlık görüntülerini almak için VSS sağlayıcısı, işlemi için'nı yükler. Bu VSS sağlayıcısı, hizmet olarak yüklenir. VSS sağlayıcısı hizmeti devre dışı durumda uygulama tutarlılığı anlık görüntü oluşturma "Belirtilen hizmet devre dışı bırakılır ve started(0x80070422) olamaz" hata koduyla başarısız. </br>

- VSS devre dışıysa,
    - VSS sağlayıcısı hizmeti başlangıç türü değerine ayarlandığını doğrulayın **otomatik**.
    - Şu hizmetleri yeniden başlatın:
        - VSS hizmeti
        - Azure Site Recovery VSS sağlayıcısı
        - VDS hizmeti

####  <a name="vss-provider-notregistered---error-2147754756"></a>VSS SAĞLAYICISI NOT_REGISTERED - hata 2147754756

**Nasıl düzeltileceğini**: Uygulama tutarlılık etiketi oluşturmak için Azure Site Recovery, Microsoft Birim Gölge Kopyası Hizmeti (VSS) kullanır. Azure Site Recovery VSS sağlayıcısı hizmet veya yüklü olup olmadığını denetleyin. </br>

- Aşağıdaki komutları kullanarak sağlayıcı kurulumunu yeniden deneyin:
- Mevcut sağlayıcısını Kaldır: C:\Program dosyaları (x86) \Microsoft Azure Site Recovery\agent\InMageVSSProvider_Uninstall.cmd
- Yeniden yükleyin: C:\Program dosyaları (x86) \Microsoft Azure Site Recovery\agent\InMageVSSProvider_Install.cmd
 
VSS sağlayıcısı hizmeti başlangıç türü değerine ayarlandığını doğrulayın **otomatik**.
    - Şu hizmetleri yeniden başlatın:
        - VSS hizmeti
        - Azure Site Recovery VSS sağlayıcısı
        - VDS hizmeti
