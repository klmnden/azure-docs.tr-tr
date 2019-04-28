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
ms.openlocfilehash: 9ff756270c368d39b7ef78d7c1046f7c91169668
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62103755"
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

* **Bir yüksek veri değişim hızı neden olan bir diski hariç**: Kullanarak bir diski hariç tutabilirsiniz [PowerShell](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-powershell#replicate-azure-virtual-machine).
* **Olağanüstü Durum Kurtarma Depolama diski katmanını değiştirme**: Bu seçenek yalnızca disk veri değişim sıklığı, 10 MB/sn olması durumunda mümkündür. Bir VM ile bir P10 disk 8 MB/sn ancak 10 MB/sn'den büyük bir veri değişim sıklığı yaşıyor varsayalım. Koruma sırasında müşteri P30 disk için hedef depolama kullanabilir, sorun çözülebilir.

## <a name="Network-connectivity-problem"></a>Ağ bağlantısı sorunları

### <a name="network-latency-to-a-cache-storage-account"></a>Bir önbellek depolama hesabına ağ gecikmesi
Site Recovery, önbellek depolama hesabına çoğaltılan verileri gönderir. Bir sanal makineden önbellek depolama hesabı için verileri karşıya yükleme, ağ gecikmesi 3 saniye içinde 4 MB'den daha yavaş olup olmadığını görebilirsiniz. 

Gecikme süresi için ilgili bir sorun denetlemek için kullanmak [azcopy](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy) verileri sanal makineden önbellek depolama hesabına yüklemek için. Gecikme süresi yüksek ise vm'lerden giden ağ trafiğinizi denetlemek için bir ağ sanal Gereci (NVA) kullanıp kullanmadığınızı kontrol edin. Tüm çoğaltma trafiğinin NVA üzerinden geçerse gereç kısıtlanan. 

Çoğaltma trafiği için NVA Git değil, bir ağ hizmet uç noktaları sanal ağınızda bulunan "Depolama için" oluşturmanızı öneririz. Daha fazla bilgi için [ağ sanal Gereci Yapılandırması](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-about-networking#network-virtual-appliance-configuration).

### <a name="network-connectivity"></a>Ağ bağlantısı
Site Recovery çoğaltması için iş, giden bağlantı için özel URL veya IP aralıkları VM'den gerekli. Sanal makinenize bir güvenlik duvarının arkasındaysa ya da giden bağlantıyı denetlemek için ağ güvenlik grubu (NSG) kuralları kullanıyorsa bu sorunlarından biri, yüz tanıma. Tüm URL'leri bağlandığınızdan emin olmak için bkz: [Site kurtarma URL'ler için giden bağlantı](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-about-networking#outbound-connectivity-for-ip-address-ranges). 