---
title: Azure Backup destek matrisi
description: Destek ayarları ve sınırlamalar için Azure Backup hizmeti bir özetini sağlar.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 02/17/2019
ms.author: raynew
ms.openlocfilehash: 51bd4b935b32bea20d3f5de0b8cda62dfdbf07b8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60236640"
---
# <a name="azure-backup-support-matrix"></a>Azure Backup destek matrisi

Kullanabileceğiniz [Azure Backup](backup-overview.md) verileri yedeklemek için Microsoft Azure bulut platformu için. Bu makalede, Azure yedekleme senaryoları ve dağıtımlar için sınırlamalar ve genel destek ayarları özetlenmektedir.

Diğer destek matrisi kullanılabilir:

- Destek matrisi [Azure sanal makine (VM) yedekleme](backup-support-matrix-iaas.md)
- Kullanarak yedekleme destek matrisi [System Center Data Protection Manager (DPM) / Microsoft Azure Backup sunucusu (MABS)](backup-support-matrix-mabs-dpm.md)
- Kullanarak yedekleme destek matrisi [Microsoft Azure kurtarma Hizmetleri (MARS) aracısı](backup-support-matrix-mars-agent.md)

## <a name="vault-support"></a>Kasa desteği

Azure Backup, Kurtarma Hizmetleri kasaları düzenlemek ve yedekleri yönetme için kullanır. Kasaları yedeklenmiş verileri depolamak için de kullanır. 

Aşağıdaki tabloda Kurtarma Hizmetleri kasaları özellikleri açıklar:

**Özellik** | **Ayrıntılar**
--- | ---
**Abonelik kasalarında** | Tek bir abonelikte en fazla 500 kurtarma Hizmetleri kasası.
**Bir kasadaki makineler** | Tek bir kasada en fazla 1.000 Azure Vm'leri.<br/><br/> En fazla 50 MABS sunucuları tek bir kasaya kaydedilebilir.
**Kasa Depolama veri kaynakları** | En fazla 54,400 GB. Azure VM yedeklemeleri için bir sınır yoktur.
**Yedekleme kasası için** | **Azure sanal makineler:** Günde bir kez.<br/><br/>**DPM/MABS tarafından korunan makineler:** Günde iki kez.<br/><br/> **MARS aracısı kullanarak doğrudan yedeklenen makineler:** Günde üç kez. 
**Yedekleme kasaları arasında** | Bir bölge içinde yedeklemedir.<br/><br/> Yedeklemek istediğiniz Vm'leri içeren her Azure bölgesi bir kasada ihtiyacınız vardır. Farklı bir bölgeye yedekleyemezsiniz. 
**Kasaları Taşı** | Yapabilecekleriniz [kasaları taşıma](https://review.docs.microsoft.com/azure/backup/backup-azure-move-recovery-services-vault) aynı Abonelikteki kaynak grupları arasında veya abonelikler arasında.
**Kasalar arasında veri taşıma** | Yedeklenen verilerin kasalar arasında taşıma desteklenmiyor.
**Kasa Depolama türünü değiştirme** | Yedeklemeleri depolanmadan önce bir kasa için depolama çoğaltma türü (coğrafi olarak yedekli depolama veya yerel olarak yedekli depolama) değiştirebilirsiniz. Yedekleme Kasası'nda başladıktan sonra çoğaltma türü değiştirilemez.

## <a name="on-premises-backup-support"></a>Şirket içi yedekleme desteği

Şirket içi makineleri yedeklemek istiyorsanız, nelerin desteklendiği aşağıda verilmiştir:

**Makine** | **Ne yedeklenir** | **Konum** | **Özellikler**
--- | --- | --- | ---
**Windows makine MARS Aracısı ile doğrudan yedekleme** | Dosyaları, klasörleri, sistem durumu | Kurtarma Hizmetleri kasasına yedekleyin. | Günde üç kez yedekleme<br/><br/> Uygulama durumunu algılayan yedekleme<br/><br/> Dosya, klasör, birim geri yükleme
**MARS Aracısı ile Linux makinesinin doğrudan yedekleme** | Yedekleme desteklenmiyor
**En fazla DPM'yi yedekleme** | Dosyaları, klasörleri, birimleri, sistem durumu, uygulama verileri | Yerel DPM depolama alanına yedekleyin. DPM ardından kasaya yedekler. | Uygulama kullanan anlık görüntüler<br/><br/> Yedekleme ve kurtarma için tam ayrıntı düzeyi<br/><br/> Linux sanal makineleri (Hyper-V/VMware) için desteklenen<br/><br/> Oracle desteklenmiyor
**Geri MABS kadar** | Dosyaları, klasörleri, birimleri, sistem durumu, uygulama verileri | MABS yerel depolama alanına yedekleyin. MABS ardından kasaya yedekler. | Uygulama kullanan anlık görüntüler<br/><br/> Yedekleme ve kurtarma için tam ayrıntı düzeyi<br/><br/> Linux sanal makineleri (Hyper-V/VMware) için desteklenen<br/><br/> Oracle desteklenmiyor

## <a name="azure-vm-backup-support"></a>Azure VM yedekleme desteği

### <a name="azure-vm-limits"></a>Azure VM sınırları

**Sınırı** | **Ayrıntılar**
--- | ---
**Azure VM veri diski** | 16 sınırı
**Azure VM veri diski boyutu** | Tek disklerin 4.095 GB'a kadar olabilir

### <a name="azure-vm-backup-options"></a>Azure VM yedekleme seçenekleri

Azure Vm'lerini yedeklemek istiyorsanız, nelerin desteklendiği aşağıda verilmiştir:

**Makine** | **Ne yedeklenir** | **Konum** | **Özellikler**
--- | --- | --- | ---
**VM uzantısı kullanarak azure VM yedeklemesi** | Tüm VM | Kasaya yedekleyin. | Bir VM için yedeklemeyi etkinleştirdiğinizde yüklü uzantısı.<br/><br/> Günde bir kez yedekleyin.<br/><br/> Windows VM'ler için uygulama durumunu algılayan yedekleme; Linux VM'ler için dosyayla tutarlı yedekleme. Uygulama tutarlılığı Linux makineler için özel betikler kullanarak yapılandırabilirsiniz.<br/><br/> VM veya disk geri yükleyin.<br/><br/> Bir Azure VM'yi bir şirket içi konuma yedekleyemezsiniz.
**MARS Aracısı'nı kullanarak azure VM yedeklemesi** | Dosyalar, klasörler | Kasaya yedekleyin. | Günde üç kez yedekleyin.<br/><br/> MARS Aracısı, belirli dosyaları veya klasörleri yerine tüm VM'yi yedeklemek istiyorsanız VM uzantıyla birlikte çalıştırabilirsiniz.
**DPM ile Azure VM** | Dosyaları, klasörleri, birimleri, sistem durumu, uygulama verileri | DPM çalıştıran bir Azure VM yerel depolama alanına yedekleyin. DPM ardından kasaya yedekler. | Uygulama kullanan anlık görüntüler.<br/><br/> Yedekleme ve kurtarma için tam ayrıntı düzeyi.<br/><br/> Linux sanal makineleri (Hyper-V/VMware) için desteklenir.<br/><br/> Oracle desteklenmiyor.
**MABS ile Azure VM** | Dosyaları, klasörleri, birimleri, sistem durumu, uygulama verileri | Azure VM, MABS çalıştıran yerel depolama alanına yedekleyin. MABS ardından kasaya yedekler. | Uygulama kullanan anlık görüntüler.<br/><br/> Yedekleme ve kurtarma için tam ayrıntı düzeyi.<br/><br/> Linux sanal makineleri (Hyper-V/VMware) için desteklenir.<br/><br/> Oracle desteklenmiyor.

## <a name="linux-backup-support"></a>Linux yedekleme desteği

Linux makineleri yedeklemek istiyorsanız, nelerin desteklendiği aşağıda verilmiştir:

**Yedekleme türü** | **Linux (Azure destekli)**
--- | ---
**Linux çalıştıran şirket içi makinenin doğrudan yedekleme** | Desteklenmiyor. MARS Aracısı, yalnızca Windows makinelerde yüklenebilir.
**Agent uzantısını kullanarak Linux çalıştıran Azure VM'yi yedekleme** | Kullanarak uygulamayla tutarlı yedekleme [özel betikler](backup-azure-linux-app-consistent.md).<br/><br/> Dosya düzeyinde kurtarma.<br/><br/> Bir kurtarma noktası veya diskten bir sanal makine oluşturarak geri yükleyin.
**Şirket içi veya Linux çalıştıran bir Azure VM yedekleme için DPM'yi kullanma** | Hyper-V ve VMWare Linux Konuk vm'lerinin dosyayla tutarlı yedekleme.<br/><br/> Hyper-V ve VMWare Linux Konuk Vm'lerinin VM geri yükleme.<br/><br/> Dosyayla tutarlı yedekleme Azure VM için kullanılamaz.
**Şirket içi makine veya Linux çalıştıran bir Azure VM yedekleme için MABS kullanma** | Hyper-V ve VMWare Linux Konuk vm'lerinin dosyayla tutarlı yedekleme.<br/><br/> Hyper-V ve VMWare Linux Konuk Vm'lerinin VM geri yükleme.<br/><br/> Dosyayla tutarlı yedekleme Azure Vm'leri için kullanılamaz.

## <a name="daylight-saving-time-support"></a>Gün ışığından yararlanma desteği

Azure Backup, Azure VM yedeklemeleri için Yaz Saati için otomatik saat ayarlama desteklememektedir. Yedekleme ilkelerini el ile gerektiği gibi değiştirin.

## <a name="disk-deduplication-support"></a>Disk yinelenenleri kaldırma desteği

Disk yinelenenleri kaldırma desteği aşağıdaki gibidir:

- Windows çalıştıran Hyper-V Vm'lerini yedekleme için DPM veya MABs kullandığınızda, disk için yinelenenleri kaldırma desteklenen şirket içi kullanılır. Windows Server sanal sabit VM yedekleme depolama alanı olarak eklenmiş diskleri (VHD'ler) üzerinde yinelenen verileri kaldırma (ana bilgisayar düzeyinde) gerçekleştirir.
- Yinelenenleri kaldırma özelliği, Azure'da Backup bileşenlerinden herhangi biri için desteklenmez. DPM ve MABS Azure'da dağıtıldığında, VM'ye bağlı depolama diskleri için yinelenenler kaldırılamaz.

## <a name="security-and-encryption-support"></a>Güvenlik ve şifreleme desteği

Azure Backup, aktarım sırasında ve bekleyen veriler için Şifreleme destekler.

### <a name="network-traffic-to-azure"></a>Azure ağ trafiği

- Kurtarma Hizmetleri kasası sunucularından yedekleme trafiği, Gelişmiş Şifreleme Standardı 256 kullanılarak şifrelenir.
- Yedekleme verileri güvenli bir HTTPS bağlantısı üzerinden gönderilir.
- Yedekleme verilerini, Kurtarma Hizmetleri kasasında şifreli biçimde depolanır.
- Yalnızca bu verilerin kilidini açmak için parola gerekir. Microsoft, herhangi bir noktada yedekleme verilerinin şifresini çözemez.

    > [!WARNING]
    > Yalnızca kasası ayarladıktan sonra şifreleme anahtarına erişime sahip. Microsoft hiçbir zaman bir kopyasını tutar ve erişim anahtarına sahip değil. Anahtarın kaybedilmesi durumunda Microsoft Yedekleme verilerini kurtaramaz.

### <a name="data-security"></a>Veri güvenliği

- Azure Vm'lerini yedekleme yapıyorsanız, şifrelemeyi ayarlama yapmanız *içinde* sanal makine.
- Azure Backup, Windows sanal makinelerde BitLocker, Linux sanal makinelerde ise **dm-crypt** kullanan Azure Disk Şifrelemesi özelliğini destekler.
- Arka uçta Azure Backup kullanan [Azure depolama hizmeti şifrelemesi](../storage/common/storage-service-encryption.md), bekleyen verileri korur.

**Makine** | **Yoldaki** | **Bekleyen**
--- | --- | ---
**DPM/MABS olmadan şirket içi Windows makineler** | ![Evet][green] | ![Evet][green]
**Azure Vm'leri** | ![Evet][green] | ![Evet][green]
**Windows makineleri şirket içi veya DPM ile Azure Vm'leri** | ![Evet][green] | ![Evet][green]
**Windows makineleri şirket içi veya MABS ile Azure Vm'leri** | ![Evet][green] | ![Evet][green]

## <a name="compression-support"></a>Sıkıştırma desteği

Aşağıdaki tabloda özetlendiği gibi yedekleme sıkıştırma, yedekleme trafiğinin destekler.

- Bu trafiğin sıkıştırılması gerekmez şekilde Azure Vm'leri için depolama ağı aracılığıyla doğrudan Azure depolama hesabından veri VM uzantısı okur.
- DPM veya MABS kullanıyorsanız, verilerin yedeklendiği, önce sıkıştırarak bant genişliği kaydedebilirsiniz.

**Makine** | **Sıkıştırma MABS/DPM sunucusuna (TCP)** | **Kasa (HTTPS) için Sıkıştır**
--- | --- | ---
**Doğrudan yedekleme, şirket içi Windows makineler** | NA | ![Evet][green]
**VM uzantısı kullanarak Azure vm'leri yedekleme** | NA | NA
**MABS/DPM kullanarak şirket içi/Azure makinelerde yedekleme** | ![Evet][green] | ![Evet][green]

## <a name="retention-limits"></a>Bekletme sınırları

**Ayar** | **Limitler**
--- | ---
**(Makine veya iş yükü) korumalı örnek başına en fazla kurtarma noktası** | 9,999
**Bir kurtarma noktası için en fazla süre sonu zamanı** | Sınırsız
**DPM/MABS en fazla yedekleme sıklığı** | SQL Server için 15 dakikada bir<br/><br/> Diğer iş yükleri için saatte bir kez
**Kasa için en fazla yedekleme sıklığını** | **Windows makine ya da Azure MARS çalıştıran VM'ler şirket içinde:** Üç gün başına<br/><br/> **DPM/MABS:** İki günde<br/><br/> **Azure VM yedeklemesi:** Günde bir
**Kurtarma noktası bekletme** | Günlük, haftalık, aylık, yıllık
**En uzun saklama süresi** | Yedekleme sıklığına bağlıdır
**DPM/MABS diskteki kurtarma noktaları** | dosya sunucuları için 64; uygulama sunucuları için 448 <br/><br/>Şirket içi DPM için sınırsız bant kurtarma noktaları

## <a name="next-steps"></a>Sonraki adımlar

- [Gözden geçirme destek matrisi](backup-support-matrix-iaas.md) Azure VM yedeklemesi için.

[green]: ./media/backup-support-matrix/green.png
[yellow]: ./media/backup-support-matrix/yellow.png
[red]: ./media/backup-support-matrix/red.png
