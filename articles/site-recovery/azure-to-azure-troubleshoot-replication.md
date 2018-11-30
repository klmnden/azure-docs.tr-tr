---
title: Azure Site Recovery Azure'dan Azure'a çoğaltma sorunlarını ve hataları için sorun giderme | Microsoft Docs
description: Olağanüstü durum kurtarma için Azure sanal makineleri çoğaltırken hatalarını ve sorunlarını giderme
services: site-recovery
author: asgang
manager: rochakm
ms.service: site-recovery
ms.devlang: na
ms.topic: troubleshooting
ms.date: 10/30/2018
ms.author: asgang
ms.openlocfilehash: 22ea3d955fe2910dc99ab4015165008da899d48e
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2018
ms.locfileid: "52312859"
---
# <a name="troubleshoot-azure-to-azure-vm-ongoing-replication-issues"></a>Azure'dan Azure'a VM devam eden çoğaltma sorunlarını giderme

Bu makalede Azure Site Recovery çoğaltma ve Azure sanal makineleri bir bölgesinden başka bir bölgeye kurtarma giren yaygın sorunların ve bunları nasıl giderebileceğinizden açıklar. Desteklenen yapılandırmalar hakkında daha fazla bilgi için bkz. [Azure Vm'lerini çoğaltma için destek matrisi](site-recovery-support-matrix-azure-to-azure.md).


## <a name="recovery-points-not-getting-generated"></a>Kurtarma noktaları oluşturulmuyor

HATA iletisi: Kilitlenme tutarlı kurtarma noktası yok son 60 dakika içinde VM için kullanılabilir.</br>
HATA KİMLİĞİ: 153007 </br>

Azure Site Recovery, tutarlı bir şekilde veri kaynak bölgesinden olağanüstü durum kurtarma bölgeye çoğaltır. ve 5 dakikada kilitlenmeyle tutarlı noktası oluşturur. Site Recovery 60 dakika için kurtarma noktaları oluşturamıyor, ardından kullanıcıyı uyarır. Aşağıda bu hataya neden nedenleri şunlardır:

**1. neden: [yüksek veri değişim oranı kaynak sanal makinede](#high-data-change-rate-on-the-source-virtal-machine)**    
**2. neden: [ağ bağlantısı sorunu ](#Network-connectivity-issue)**

## <a name="causes-and-solutions"></a>Nedenler ve çözümler

### <a name="high-data-change-rate-on-the-source-virtal-machine"></a>Yüksek veri değişim oranı kaynak sanal makine
Azure Site Recovery, kaynak sanal makinede veri değişim hızı desteklenen sınırları yüksek ise bir olay tetikler. Sorunu nedeniyle yüksek değişim sıklığı olup olmadığını denetlemek için çoğaltılan öğelerine gidin > VM > tıklayarak "olaylar-son 72 saat".
Aşağıdaki ekran görüntüsünde gösterildiği gibi olay "veri değişim hızı desteklenen sınırları aşıyor" görmeniz gerekir

![data_change_rate_high](./media/site-recovery-azure-to-azure-troubleshoot/data_change_event.png)

Olayda tıklarsanız aşağıdaki ekran görüntüsünde gösterildiği gibi tam disk bilgilerini görmelisiniz

![data_change_rate_event](./media/site-recovery-azure-to-azure-troubleshoot/data_change_event2.png)


#### <a name="azure-site-recovery-limits"></a>Azure Site Recovery limitleri
Aşağıdaki tablo, Azure Site Recovery sınırlarını sağlar. Bu limitler yaptığımız testleri temel alsa da mümkün olan tüm uygulama G/Ç birleşimlerini kapsamamaktadır. Gerçek sonuçlar, uygulamanızın G/Ç karışımına göre değişebilir. Biz de, veri değişim sıklığı ve sanal makinenin veri değişim sıklığı disk başına dikkate alınması gereken iki sınır olmadığını unutmamalısınız.
Örneğin, P20 Premium disk baktığımızda aşağıdaki tabloda, en fazla beş tür disk VM başına 25 MB/sn toplam değişim sıklığı, VM sınırı nedeniyle disk ile 5 MB/sn değişim işlemek için Site kurtarma yapabilirsiniz.

**Çoğaltma depolama hedefi** | **Ortalama kaynak disk G/Ç boyutu** |**Ortalama kaynak disk veri değişim sıklığı** | **Günlük toplam kaynak disk veri değişim sıklığı**
---|---|---|---
Standart depolama | 8 KB | 2 MB/sn | Disk başına 168 GB
Premium P10 veya P15 disk | 8 KB  | 2 MB/sn | Disk başına 168 GB
Premium P10 veya P15 disk | 16 KB | 4 MB/sn |  Disk başına 336 GB
Premium P10 veya P15 disk | 32 KB veya daha büyük | 8 MB/sn | Disk başına 672 GB
Premium P20 veya P30 veya P40 veya P50 disk | 8 KB    | 5 MB/sn | Disk başına 421 GB
Premium P20 veya P30 veya P40 veya P50 disk | 16 KB veya daha büyük |10 MB/sn | Disk başına 842 GB

### <a name="solution"></a>Çözüm
Azure Site Recovery disk türüne göre oran sınırlarını değiştirme verisine sahip olduğunu anlamanız gerekir. Bu sorunu yinelenen veya olmayabileceği bulmak önemli olan bilmek oranı düzeni etkilenen sanal makinenin veri değiştirin.
Veri değişikliği oranı etkilenen sanal makinenin bulunacak. Kaynak sanal makinenin gidin > İzleme bölümünden ölçümleri ve ölçümler, aşağıda gösterildiği gibi ekleyin.

![high_data_change_rate](./media/site-recovery-azure-to-azure-troubleshoot/churn.png)

1. "Üzerinde ölçüm Ekle"'a tıklayın ve "İşletim sistemi diski yazma bayt/sn" ve "Veri diski yazma bayt/sn" ekleyin.
2. Depo, ekran görüntüsünde gösterildiği gibi izleyin.
3. İşletim sistemi diski ve tüm veri diskleri birleştirilmiş arasında gerçekleştirilecek işlem toplam Yazar gösterilir. Artık Bu ölçümler, disk düzeyi bilgileri söyleyebilir değil ancak toplam veri değişim sıklığı iyi bir göstergesidir.

Gibi durumlarda bir arada sırada veri veri bloğu ise ve veri değişim hızı (Premium için) 10 MB/sn ile 2 MB/sn (standart için) bir süre değerinden büyükse ve aşağı, gelen çoğaltma Kaçırdığınız. Değişim sıklığı, çoğu zaman iyi desteklenen sınırı aşan ise, ancak ardından aşağıdakilerden birini dikkate almanız gereken mümkünse seçeneği altında:

**1. seçenek:** yüksek veri değişim hızı neden diskini dışarıda tutma: </br>
Kullanarak disk şu anda hariç tutabilirsiniz [Site Recovery Powershell](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-powershell#replicate-azure-virtual-machine)

**2. seçenek:** olağanüstü durum kurtarma depolama diski katmanını değiştirebilir: </br>
Disk veri değişim sıklığı, 10 MB/sn ise bu seçenek yalnızca mümkündür. Bir VM ile P10 söyleyin izin disk 8 MB/sn ancak 10 MB/sn'den büyük bir veri değişim sıklığı yaşıyor. Müşteri P30 disk için hedef depolama sırasında koruma kullanıyorsanız, sorun çözülebilir.

### <a name="Network-connectivity-issue"></a>Ağ bağlantısı sorunu

#### <a name="network-latency-to-cache-storage-account-"></a>Önbellek depolama hesabına ağ gecikmesi:
 Site Recovery, önbellek depolama hesabına çoğaltılan veriler gönderir ve daha yavaş sanal makineden önbellek depolama hesabı için verileri karşıya yükleme sorunu meydana gelebilir 3 saniye, 4 MB. Herhangi bir sorun olup olmadığını denetlemek için gecikme kullanımıyla ilgili [azcopy](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy) verileri sanal makineden önbellek depolama hesabına yüklemek için.<br>
Gecikme süresi yüksek ise vm'lerden giden ağ trafiğinizi denetlemek için bir ağ sanal Gereçleri kullandığınızı kontrol edin. Tüm çoğaltma trafiğinin NVA üzerinden geçerse gereç kısıtlanan. Çoğaltma trafiği için NVA geçmez, bir ağ hizmet uç noktaları sanal ağınızda bulunan "Depolama için" oluşturmanızı öneririz. Başvuru [ağ sanal Gereci yapılandırması](https://docs.microsoft.com/en-us/azure/site-recovery/azure-to-azure-about-networking#network-virtual-appliance-configuration)

#### <a name="network-connectivity"></a>Ağ bağlantısı
Site Recovery çoğaltması için iş, giden bağlantı için özel URL veya IP aralıkları VM'den gerekli. Sanal makinenize bir güvenlik duvarının arkasındaysa ya da giden bağlantıyı denetlemek için ağ güvenlik grubu (NSG) kuralları kullanıyorsa bu sorunlardan biri karşılaşıyor.</br>
Başvurmak [Site kurtarma URL'ler için giden bağlantı](https://docs.microsoft.com/en-us/azure/site-recovery/azure-to-azure-about-networking#outbound-connectivity-for-ip-address-ranges) tüm URL'leri bağlandığınızdan emin olmak için 