---
title: Azure Site Recovery güncelleştirmeleri | Microsoft Docs
description: Hizmet güncelleştirmeleri ve Azure Site Recovery'de kullanılan bileşenleri yükseltme genel bir bakış sağlar.
services: site-recovery
author: rajani-janaki-ram
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 04/25/2019
ms.author: rajanaki
ms.openlocfilehash: bde341063fb6742bbe2a92592981d4a2a437d214
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67203431"
---
# <a name="service-updates-in-azure-site-recovery"></a>Azure Site Recovery hizmeti güncelleştirmeleri
Bir kuruluş olarak plansız kesintiler ve verilerinizin güvenliğini korumak için nasıl gideceğinizi ve Planlı çalışan uygulamaları/iş yüklerini out şekil yapmanız gerekir. Azure Site Recovery, Vm'leri ve fiziksel sunucuları bir site dışı kalırsa kullanılabilir çalışan uygulamalarınızı tutarak, BCDR stratejinize katkıda bulunur. Site Recovery, VM ve fiziksel sunucularda çalışan iş yüklerini çoğaltarak birincil sitenin kullanılamaz hale gelmesi durumunda bunların ikincil bir konumda kullanılabilir kalmasını sağlar. Birincil site yeniden çalışmaya başladığında birincil sitede iş yüklerini kurtarır.

Site Recovery, şunlar için çoğaltmayı yönetebilir:

- [Azure bölgeleri arasında çoğaltılan azure Vm'leri](azure-to-azure-tutorial-dr-drill.md).
- Azure’a veya ikincil bir siteye çoğaltılan şirket içi sanal makineler ve fiziksel sunucular.
Daha fazla bilgi için belgelere bakın bilmek [burada](https://docs.microsoft.com/azure/site-recovery) .

Azure Site Recovery destek matrisi ve hata düzeltmeleri varsa yeni özellikler, iyileştirmeler eklenmesi de dahil olmak üzere düzenli aralıklarla - hizmet güncelleştirmeleri yayımlar. Kullanıcıların geçerli yararlanın tüm en son özellikler ve geliştirmeler ve hata düzeltmeleri varsa kalmak için her zaman Azure SIte Recovery bileşenlerinin en son sürümlerine güncelleştirmek için önerilir. 
 
## <a name="support-statement-for-azure-site-recovery"></a>Azure Site Recovery için destek bildirimi 

> [!IMPORTANT]
> **Her yeni sürümü ile yayınlanan, bir Azure Site Recovery bileşeni 'N-4 altındaki tüm sürümleri 'N'' destek kapsamı dışında kabul**. Bu nedenle, her zaman kullanılabilir en son sürümlerine yükseltmek için önerilir.

> [!IMPORTANT]
> Yükseltme resmi desteği dandır > (en son sürümü olan N) N sürümüne N-4. N-6 üzerinde ise N-4 için yükseltmeniz ve ardından N'ye yükseltme gerekiyor

## <a name="expiry-of-components"></a>Süre sonu bileşenleri
Site Recovery bileşenlerinin dolmak üzere müşterilerin bildirir veya (, bunlara abone olan ise) e-posta bildirimleri süresi zaten dolmuş veya Portalı'nda kasa panosunda. Kasa Panosu bildirimler artık itibarıyla hYpe rV VM koruyorsanız kullanılamaz. Ayrıca, senaryonuz için karşılık gelen altyapı görünümüne giderseniz, olacaktır indirmeleri en son sürümleri bağlantılarını yönlendirilecek bileşen yanında bir 'güncelleştirme var' düğmesi.

Süre sonu bir bileşenleri yaklaşan olduğunda e-posta uyarılarının sıklığı aşağıda verilmiştir.
- 60 gün önce dolmak üzere bileşendir: haftada bir kez
- Sonraki 53 gün: haftada bir kez
- Son 7 gün: Günlük bir kez
- Sonra süresi dolmuş: haftada bir kez



### <a name="upgrading-when-the-difference-between-current-version-and-latest-released-version-is-greater-than-4"></a>Geçerli sürümü ve en son yayımlanan sürümü arasındaki fark, 4'ten büyük olduğunda yükseltme

1. İlk adım, şu anda yüklenen bileşenin sürüm say N N + 4'e yükseltme ve ardından sonraki uyumlu sürüme taşıyın. 9,24 geçerli sürümüdür ve 9.16, ilk yükseltmeden 9.20 ve ardından 9,24 olduğunu varsayalım.
2. Senaryoya bağlı olarak tüm bileşenlerin aynı süreci izleyin.

### <a name="support-for-latest-oskernel-versions"></a>En son işletim sistemi/kernel sürümleri için destek

> [!NOTE]
> Zamanlanmış bir bakım penceresi varsa ve bir yeniden başlatma aynı parçası ise ilk Site Recovery bileşenlerini yükseltmek ve zamanlanmış etkinlikleri geri kalanıyla devam öneririz.

1. Çekirdek/OS sürümü yükseltmeden önce ilk hedef sürümünü Azure Site Recovery tarafından desteklenip desteklenmediğini denetleyin. Azure Vm'leri için Belgelerimizdeki bilgileri bulabilirsiniz [VMware Vm'lerini](vmware-physical-azure-support-matrix.md) & Hyper-V Vm'lerini
2. Başvuru bizim [hizmet güncelleştirmeleri](https://azure.microsoft.com/updates/?product=site-recovery) Site Recovery hangi sürümünü bulmak için bileşenleri desteklemek için yükseltmek istediğiniz belirli bir sürüm.
3. İlk olarak, son Site Kurtar sürüme yükseltin.
4. Şimdi, işletim sistemi/çekirdek istenen sürümüne yükseltin.
5. Yeniden başlatma gerçekleştirin.
6. Bu işletim sistemi/çekirdek sürümü makinelerinizde yükseltilir sağlayacak en son sürümü ve bu da, yeni sürüm desteği için gerekli olan en son Site Recovery değişiklikleri kaynak makinede de yüklenir.



## <a name="azure-vm-disaster-recovery-to-azure"></a>Azure'a Azure VM olağanüstü durum kurtarma
Bu senaryoda, öneririz, [etkinleştirme](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-autoupdate) otomatik güncelleştirmeler. Site Recovery, aşağıdaki yollarla güncelleştirmeleri yönetmek izin vermek seçebilirsiniz:

- Etkinleştirme çoğaltma adımının bir parçası
- İki durumlu uzantısı kasa içinde ayarlarını güncelleştirme

El ile güncelleştirmeleri yönetmek seçtiğiniz durumda aşağıdaki adımları izleyin:

1. Azure portalına gidin ve ardından "Kurtarma Hizmetleri kasanıza." gidin
2. Azure portalında "Kurtarma Hizmetleri kasası." için "Çoğaltılan öğeler" bölmesine gidin
3. Ekranın üst kısmındaki şu bildirime tıklayın:
    
    *Yeni Site Recovery çoğaltma aracısı güncelleştirmesi kullanılabilir*
    
    *-> Yüklemek için tıklayın*

4. Güncelleştirme uygulayın ve ardından istediğiniz Vm'leri seçin **Tamam**.

## <a name="between-two-on-premises-vmm-sites"></a>İki şirket içi VMM sitesi arasında
1. Microsoft Azure Site Recovery sağlayıcısı en son güncelleştirme paketi yükleyin.
2. Güncelleştirme paketi, ilk kurtarma sitesini yönetmek, şirket içi VMM sunucusuna yükleyin.
3. Site kurtarma işleminden sonra güncelleştirilir, güncelleştirme paketini birincil sitenin yönetme VMM sunucusunda yükleyin.

> [!NOTE]
> VMM, bir yüksek oranda kullanılabilir VMM (VMM) kümelenmiş ise, yükseltme, VMM hizmetinin yüklü olduğu kümenin tüm düğümlerine yüklediğinizden emin olun.

## <a name="between-an-on-premises-vmm-site-and-azure"></a>Bir şirket içi VMM sitesi ile Azure arasında
1. Microsoft Azure Site Recovery sağlayıcısı için güncelleştirme paketi yükleyin.
2. Güncelleştirme paketi, şirket içi VMM sunucusuna yükleyin.
3. En son aracı MARS aracısının tüm Hyper-V ana bilgisayarlarına yükleyin.

> [!NOTE]
> VMM, bir yüksek oranda kullanılabilir VMM (VMM) kümelenmiş ise, yükseltme, VMM hizmetinin yüklü olduğu kümenin tüm düğümlerine yüklediğinizden emin olun.

## <a name="between-an-on-premises-hyper-v-site-and-azure"></a>Bir şirket içi Hyper-V site ile Azure arasında

1. Microsoft Azure Site Recovery sağlayıcısı için güncelleştirme paketi yükleyin.
2. Azure Site Recovery, kayıtlı Hyper-V sunucuların her düğümde sağlayıcıyı yükleyin.

> [!NOTE]
> Hyper-V konağı kümelenmiş Hyper-V sunucusu ise, yükseltme kümenin tüm düğümlerine yüklediğinizden emin olun

## <a name="between-an-on-premises-vmware-or-physical-site-to-azure"></a>Arasında bir şirket içi VMware veya fiziksel sitesinden azure'a

Güncelleştirmeler ile devam etmeden önce bkz [Site Recovery destek bildirimiyle](#support-statement-for-azure-site-recovery) yükseltme yolu anlamak için.

1. Yukarıda verilen geçerli sürümü ve Destek Ekstrenizi bağlı olarak, ilk şirket içi yönetim sunucunuza verilen yönergeleri izleyerek güncelleştirmeyi [burada](vmware-azure-deploy-configuration-server.md#upgrade-the-configuration-server). Bu yapılandırma sunucusu ve işlem sunucusu olan sunucusudur.
2. Genişleme işlem sunucuları, bunları aşağıdaki kuralları tarafından verilen sonraki güncelleştirme varsa [burada](vmware-azure-manage-process-server.md#upgrade-a-process-server).
3. Ardından, korunan her öğenin mobility aracısını güncelleştirmek için Azure portalına gidin ve ardından Git **korunan öğeler** > **çoğaltılan öğeler** sayfası. Bu sayfada bir VM'yi seçin. Seçin **Windows Update Aracısı** her VM için sayfanın en altında görünen düğme. Bu, tüm korunan vm'lerde Mobility Hizmeti Aracısı güncelleştirir.

### <a name="reboot-of-source-machine-after-mobility-agent-upgrade"></a>Mobility Aracısı yükselttikten sonra kaynak makineyi yeniden başlatın.

Yeniden başlatma, Mobility Aracısı her bir yükseltmeden sonra tüm son değişiklikleri kaynak makinede yüklü olduğundan emin olmak için önerilir. Ancak **zorunlu**. Son yeniden başlatma sırasında aracı sürümü geçerli sürümü arasındaki fark, 4'ten büyükse, yeniden başlatma zorunludur. Ayrıntılı açıklaması için aşağıdaki tabloya bakın.

|**Son yeniden başlatma sırasında aracı sürümü** | **Yükseltme** | **Olduğundan yeniden zorunlu?**|
|---------|---------|---------|
|9.16 |  9.18 | Zorunlu değil|
|9.16 | 9.19 | Zorunlu değil|
| 9.16 | 9.20 | Zorunlu değil
 | 9.16 | 9.21 | Evet, 9.20 için yükseltmeniz ve ardından yeniden sürümleri arasındaki fark olarak 9.21 için yükseltmeden önce (burada son yeniden başlatma işleminin gerçekleştirildiği 9.16 ve hedef sürümü 9.21) olan > 4,

## <a name="links-to-currently-supported-update-rollups"></a>Şu anda desteklenen güncelleştirme paketlerini bağlantılar

|Güncelleştirme paketi  |Sağlayıcı  |Birleşik Kurulum| OVF  |MARS|
|---------|---------|---------|---------|--------|
|[Güncelleştirme paketi 37](https://support.microsoft.com/help/4508614/update-rollup-37-for-azure-site-recovery)     |   5.1.4300.0  |  9.25.5241.1   |  5.1.4300.0  | 2.0.9163.0
|[Güncelleştirme paketi 36](https://support.microsoft.com/en-in/help/4503156)     |   5.1.4150.0  |  9.24.5211.1   |  5.1.4150.0  | 2.0.9160.0
|[Güncelleştirme paketi 35](https://support.microsoft.com/en-us/help/4494485/update-rollup-35-for-azure-site-recovery)     |   5.1.4000.0  |  9.23.5163.1   |  5.1.4000.0  | 2.0.9156.0
|[Güncelleştirme paketi 34](https://support.microsoft.com/en-us/help/4490016/update-rollup-34-for-azure-site-recovery) -düzeltme     |   5.1.3950.0  |  9.22.5142.1   |  5.1.3950.0  | 2.0.9155.0
|[Güncelleştirme paketi 33](https://support.microsoft.com/en-us/help/4489582/update-rollup-33-for-azure-site-recovery)     |   5.1.3900.0  |  9.22.5109.1   |  5.1.3900.0  | 2.0.9155.0
|[Güncelleştirme paketi 32](https://support.microsoft.com/en-us/help/4485985/update-rollup-32-for-azure-site-recovery)     |   5.1.3800.0  |  9.21.5091.1   |  5.1.3800.0  |2.0.9144.0

## <a name="previous-update-rollups"></a>Önceki güncelleştirme paketleri

- [Güncelleştirme Paketi 31](https://support.microsoft.com/help/4478871/update-rollup-31-for-azure-site-recovery)
- [Güncelleştirme paketi 30](https://support.microsoft.com/help/4468181/azure-site-recovery-update-rollup-30)
- [Güncelleştirme paketi 29](https://support.microsoft.com/help/4466466/update-rollup-29-for-azure-site-recovery)
- [Güncelleştirme paketi 28](https://support.microsoft.com/help/4460079/update-rollup-28-for-azure-site-recovery)
- [Güncelleştirme Paketi 27](https://support.microsoft.com/help/4055712/update-rollup-27-for-azure-site-recovery)
- [Güncelleştirme paketi 26](https://support.microsoft.com/help/4344054/update-rollup-26-for-azure-site-recovery)  
- [Güncelleştirme paketi 25](https://support.microsoft.com/help/4278275/update-rollup-25-for-azure-site-recovery) 
- [Güncelleştirme paketi 23](https://support.microsoft.com/help/4091311/update-rollup-23-for-azure-site-recovery) 
- [Güncelleştirme paketi 22](https://support.microsoft.com/help/4072852/update-rollup-22-for-azure-site-recovery) 
- [Güncelleştirme paketi 21](https://support.microsoft.com/help/4051380/update-rollup-21-for-azure-site-recovery) 
- [Güncelleştirme Paketi 20](https://support.microsoft.com/help/4041105/update-rollup-20-for-azure-site-recovery) 
- [Güncelleştirme paketi 19](https://support.microsoft.com/help/4034599/update-rollup-19-for-azure-site-recovery) 
