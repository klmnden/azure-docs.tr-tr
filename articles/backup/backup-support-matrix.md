---
title: Azure yedekleme destek matrisi
description: Destek ayarları ve sınırlamalar için Azure Backup hizmeti bir özetini sağlar.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: overview
ms.date: 01/09/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: cb3a60995a4edfe5eb00f1a5e88812146816806a
ms.sourcegitcommit: b4755b3262c5b7d546e598c0a034a7c0d1e261ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54883713"
---
# <a name="azure-backup-support-matrix"></a>Azure yedekleme destek matrisi

Kullanabileceğiniz [Azure Backup hizmeti](backup-overview.md) verilerini Microsoft Azure bulutuna yedekleyebilirsiniz. Bu makaleler, destek ayarları ve Azure yedekleme senaryoları ve dağıtımlar için sınırlamalar özetler.

## <a name="vault-support"></a>Kasa desteği

Yedeklenen verileri düzenlemek ve yedekleri yönetme ve depolamak için kurtarma Hizmetleri kasaları azure yedeklemeleri kullanır.

**Ayar** | **Ayrıntılar**
--- | ---
Kasaları sayısı | Tek bir abonelikte en fazla 500 kurtarma Hizmetleri kasası.
Bir kasadaki makineler | Tek bir kasada en fazla 1000 Azure Vm'leri.<br/><br/> 50 şirket içi kadar Azure Backup aracısını (Microsoft Azure kurtarma Hizmetleri Aracısı (MABS)) çalışan makinelerin tek bir kasaya kaydedilebilir.
Veri kaynağında depolama kasası | En fazla 54400 GB. Azure VM yedeklemeleri için bir sınır yoktur.
Yedekleme kasası için | Azure sanal makineler: gün; bir kez DPM/MABS tarafından korunan makineler: günde iki kez; Makine yedeklenmekte MARS agent'ı kullanarak doğrudan: günde üç kez.  
Kasa Taşı | Bir kurtarma Hizmetleri kasasına taşımak için bir özel önizlemeye kaydolmanız gerekir. Denemek için yazma AskAzureBackupTeam@microsoft.com.
Kasalar arasında veri taşıma | Yedeklenen verileri kasalar arasında taşıma desteklenmiyor.
Depolama çoğaltma türü | Yedeklemeleri depolanmadan önce bir kasa için depolama çoğaltma türü (GRS/LRS) değiştirebilirsiniz. Yedekleme Kasası'nda başladıktan sonra çoğaltma türü değiştirilemez.



## <a name="on-premises-backup-support"></a>Şirket içi yedekleme desteği

Şirket içi makineleri yedeklemek istiyorsanız, nelerin desteklendiği aşağıda verilmiştir.

**Makine** | **Konum** | **Yedekleme** | **Özellikler**
--- | --- | --- | ---
**Windows fiziksel/sanal (yedekleme sunucusuna yok)** | Dosyaları, klasörleri, sistem durumu | Kurtarma Hizmetleri kasasına yedeklendi | Yedekleme günde üç kez.<br/><br/> Uygulama durumunu algılayan yedekleme.<br/><br/> Dosya, klasör, birim geri yükleyin.
**Linux fiziksel/sanal (yedekleme sunucusuna yok)** | Yedekleme desteklenmez.
**Fiziksel/sanal DPM ile** | Dosyaları, klasörleri, birimleri, sistem durumu, uygulama verileri. | DPM'e yedeklenen (için DPM sunucusuna veya banda yerel olarak bağlı disk.<br/><br/> DPM ardından kasaya yedekler. | Uygulama kullanan anlık görüntüler<br/><br/> Yedekleme ve kurtarma için tam ayrıntı düzeyi.<br/><br/> Linux sanal makineleri (Hyper-V/VMware) için desteklenir.<br/><br/>. Oracle desteklenmiyor.
**Fiziksel MABS ile sanal** | Dosyaları, klasörleri, birimleri, sistem durumu, uygulama verileri. | (MABS sunucusuna yerel olarak bağlı diske. MABS için yedeklenen Bant desteklenmiyor<br/><br/> MABS ardından kasaya yedekler. | Uygulama kullanan anlık görüntüler<br/><br/> Yedekleme ve kurtarma için tam ayrıntı düzeyi.<br/><br/> Linux sanal makineleri (Hyper-V/VMware) için desteklenir.<br/><br/>. Oracle desteklenmiyor.


## <a name="azure-vms"></a>Azure VM’leri

### <a name="azure-vm-limits"></a>Azure VM sınırları

**Sınırı** | **Ayrıntılar**
--- | ---
Azure VM veri diski | 16 sınırı.
Azure VM veri diski boyutu | 4095 GB'a kadar tek bir disk olabilir.


### <a name="azure-vm-backup-options"></a>Azure VM yedekleme seçenekleri

Azure Vm'lerini yedeklemek istiyorsanız, nelerin desteklendiği aşağıda verilmiştir.

**Makine** | **Konum** | **Yedekleme** | **Özellikler**
--- | --- | --- | ---
**Azure sanal makineler (yedekleme sunucusuna yok)** | Dosyaları, klasörleri, sistem durumu | Kasaya yedeklenen. | Yedekleme günde bir kez.<br/><br/> Windows VM'ler için Linux Vm'lerinin dosyayla tutarlı yedekleme için uygulama durumunu algılayan yedekleme. Uygulama tutarlılığı Linux makineler için özel betikler kullanarak yapılandırabilirsiniz.<br/><br/> VM/disk geri yükleyin.<br/><br/> Azure sanal makinesinde, bir şirket içi konuma yedekleyemezsiniz.
**DPM ile Azure VM** | Dosyaları, klasörleri, birimleri, sistem durumu, uygulama verileri. | (', Yerel olarak DPM sunucusuna bağlı bir diskte. Azure üzerinde çalışan DPM yedeklendi Bant desteklenmez.<br/><br/> DPM ardından kasaya yedekler. | Uygulama kullanan anlık görüntüler<br/><br/> Yedekleme ve kurtarma için tam ayrıntı düzeyi.<br/><br/> Linux sanal makineleri (Hyper-V/VMware) için desteklenir.<br/><br/>. Oracle desteklenmiyor.
**MABS ile Azure VM** | Dosyaları, klasörleri, birimleri, sistem durumu, uygulama verileri. | (MABS sunucusuna yerel olarak bağlı diske) Azure'da çalışan MABS için yedeklenen. Bant desteklenmiyor<br/><br/> MABS ardından kasaya yedekler. | Uygulama kullanan anlık görüntüler<br/><br/> Yedekleme ve kurtarma için tam ayrıntı düzeyi.<br/><br/> Linux sanal makineleri (Hyper-V/VMware) için desteklenir.<br/><br/>. Oracle desteklenmiyor.





## <a name="linux-backup-support"></a>Linux yedekleme desteği

Linux makineleri yedeklemek istiyorsanız, nelerin desteklendiği aşağıda verilmiştir.

**Backup** | **Linux (Azure destekli)**
--- | ---
**Şirket içi Linux makinesi (DPM veya MABS) olmadan**. | Hayır. MARS Aracısı, yalnızca Windows makinelerde yüklenebilir.
**Azure VM (olmadan, DPM veya MABS)** | Uygulamayla tutarlı Yedekleme kullanarak [özel betikler](backup-azure-linux-app-consistent.md).<br/><br/> Dosya düzeyinde kurtarma.<br/><br/> Bir kurtarma noktası veya diskten bir sanal makine oluşturarak geri yükleyin.
**Şirket içi makine/Azure DPM ile VM** | Hyper-V ve VMWare üzerinde Linux Konuk VM’lerinin dosyayla tutarlı yedeklemesi<br/><br/> Hyper-V ve VMWare Linux Konuk VM’lerinin VM geri yüklemesi</br></br> Dosyayla tutarlı yedekleme Azure Vm'leri için kullanılamıyor
**Şirket içi makine/Azure MABS ile VM** | Hyper-V ve VMWare üzerinde Linux Konuk VM’lerinin dosyayla tutarlı yedeklemesi<br/><br/> Hyper-V ve VMWare Linux Konuk Vm'lerinin VM geri yükleme</br></br> Dosyayla tutarlı yedekleme Azure Vm'leri için kullanılamaz.

## <a name="disk-support"></a>Disk desteği

Disk yinelenenleri kaldırma desteği aşağıdaki gibidir:
- Windows çalıştıran Hyper-V Vm'lerini yedekleme için DPM veya MABs kullandığınızda disk yinelenenleri kaldırma desteklenen şirket içi kullanılır. Windows Server, yedekleme alanı olarak sanal makineye bağlanmış olan sanal makine sabit diskleri (VHD'ler) üzerinde yinelenenleri kaldırma özelliğini kullanır.
- Yinelenenleri kaldırma özelliği, Azure'da Backup bileşenlerinden herhangi biri için desteklenmez. System Center DPM ve Backup sunucusu Azure'da dağıtıldığında, VM'ye bağlı depolama diskleri için yinelenenler kaldırılamaz.


## <a name="securityencryption-support"></a>Güvenlik/şifreleme desteği

Azure Backup, aktarım sırasında ve bekleyen veriler için Şifreleme destekler:

Ağ trafiği azure'a:
- Kurtarma Hizmetleri kasası sunucularından yedekleme trafiği, Gelişmiş Şifreleme Standardı 256 kullanılarak şifrelenir.
- Yedekleme verileri güvenli bir HTTPS bağlantısı üzerinden gönderilir.
- Yedekleme verileri, şifreli biçimde kurtarma Hizmetleri kasasında depolanır.
- Yalnızca bu verilerin kilidini açmak için parola gerekir. Microsoft, herhangi bir noktada yedekleme verilerinin şifresini çözemez.
    > [!WARNING]
    > Yalnızca kasası ayarladıktan sonra şifreleme anahtarına erişime sahip. Microsoft hiçbir zaman bir kopyasını tutar ve erişim anahtarına sahip değil. Anahtarın kaybedilmesi durumunda Microsoft Yedekleme verilerini kurtaramaz.
Veri güvenliği:
- Azure Vm'lerini yedeklerken şifrelemesi ayarlamak gereken *içinde* sanal makine.
- Azure Backup, Windows sanal makinelerde BitLocker, Linux sanal makinelerde ise **dm-crypt** kullanan Azure Disk Şifrelemesi özelliğini destekler.
- Azure Backup arka uçta bekleyen verileri koruyan [Azure Depolama Hizmeti şifrelemesini](../storage/common/storage-service-encryption.md) kullanır.


**Makine** | **Yoldaki** | **Bekleyen**
--- | --- | ---
DPM/MABS olmadan şirket içi Windows makineler | ![Evet][green] | ![Evet][green]
Azure VM’leri | ![Evet][green] | ![Evet][green]
DPM ile şirket içi/Azure Vm'leri | ![Evet][green] | ![Evet][green]
MABS ile şirket içi/Azure Vm'leri | ![Evet][green] | ![Evet][green]



## <a name="compression-support"></a>Sıkıştırma desteği

Aşağıdaki tabloda özetlendiği gibi yedekleme sıkıştırma, yedekleme trafiğinin destekler. Şunlara dikkat edin:

- Bu trafiğin sıkıştırılması gerekmez, yani Azure Vm'leri için depolama ağı aracılığıyla doğrudan Azure depolama hesabından veri VM uzantısı okur.
- DPM veya MABS kullanıyorsanız, DPM/bant genişliğinden tasarruf etmek MABS için yedeklenir önce verileri sıkıştırabilirsiniz.

**Makine** | **Sıkıştırma MABS/DPM sunucusuna (TCP)** | **(HTTPS) kasaya Sıkıştır**
--- | --- | ---
DPM/MABS olmadan şirket içi Windows makineler | NA | Evet
Azure VM’leri | NA | NA
DPM ile şirket içi/Azure Vm'leri | ![Evet][green] | ![Evet][green]
MABS ile şirket içi/Azure Vm'leri | ![Evet][green] | ![Evet][green]



## <a name="retention-limits"></a>Bekletme sınırları

**Ayar** | **Limitler**
--- | ---
Korunan örnek (makine/iş yükü başına en fazla kurtarma noktası | 9999
Bir kurtarma noktası için en fazla süre sonu zamanı | Sınırsız
DPM/MABS en fazla yedekleme sıklığı | SQL Server için 15 dakikada bir<br/><br/> Bir saatte diğer iş yükleri için.
Kasa için en fazla yedekleme sıklığını | Şirket içi MARS çalıştıran Windows makineleri/Azure Vm'leri: üç günde<br/><br/> DPM/MABS: İki günde<br/><br/> Azure VM yedeklemesi: Günde bir kez
Kurtarma noktası bekletme | Günlük, haftalık, aylık, yıllık.
En uzun bekletme süresi | Yedekleme sıklığına bağlıdır.
DPM/MABS diskteki kurtarma noktaları | dosya sunucuları, uygulama sunucuları için 448 için 64.<br/><br/> Bant kurtarma noktaları için şirket içi DPM sınırsızdır.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure VM'lerini yedekleme](backup-azure-arm-vms-prepare.md)
- [Doğrudan Windows makinelerini yedekleme](tutorial-backup-windows-server-to-azure.md), bir yedek sunucu olmadan.
- [MABS kümesi](backup-azure-microsoft-azure-backup.md) yedekleme Azure ve ardından Yedekleme iş yükleri için MABS için.
- [DPM ayarlama](backup-azure-dpm-introduction.md) Azure'a ve ardından iş yüklerini yedekleme DPM yedekleme.

[green]: ./media/backup-support-matrix/green.png
[yellow]: ./media/backup-support-matrix/yellow.png
[red]: ./media/backup-support-matrix/red.png
