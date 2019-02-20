---
title: Azure yedekleme destek matrisi
description: Destek ayarları ve sınırlamalar için Azure Backup hizmeti bir özetini sağlar.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 02/17/2019
ms.author: raynew
ms.openlocfilehash: ff4ee1d88bd13e647d0f6218d7e9c9b2c57a5a01
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/20/2019
ms.locfileid: "56429579"
---
# <a name="azure-backup-support-matrix"></a>Azure yedekleme destek matrisi

Kullanabileceğiniz [Azure Backup hizmeti](backup-overview.md) verilerini Microsoft Azure bulutuna yedekleyebilirsiniz. Bu makaleler, genel destek ayarları ve Azure yedekleme senaryoları ve dağıtımlar için sınırlamaları özetlenmektedir.

Diğer destek matrisi kullanılabilir:

[Destek matrisi](backup-support-matrix-iaas.md) Azure VM yedeklemesi için [destek matrisi](backup-support-matrix-mabs-dpm.md) System Center DPM/Microsoft Azure Backup sunucusu (MABS) kullanarak yedekleme [destek matrisi](backup-support-matrix-mars-agent.md) için Microsoft Azure'u kullanarak yedekleme Kurtarma Hizmetleri (MARS) aracısı

## <a name="vault-support"></a>Kasa desteği

Yedeklenen verileri düzenlemek ve yedekleri yönetme ve depolamak için kurtarma Hizmetleri kasaları azure yedeklemeleri kullanır.

**Ayar** | **Ayrıntılar**
--- | ---
**Abonelik kasalarında** | Tek bir abonelikte en fazla 500 kurtarma Hizmetleri kasası.
**Bir kasadaki makineler** | Tek bir kasada en fazla 1000 Azure Vm'leri.<br/><br/> En fazla 50 MABS sunucuları tek bir kasaya kaydedilebilir.
**Kasa Depolama veri kaynakları** | En fazla 54400 GB. Azure VM yedeklemeleri için bir sınır yoktur.
**Yedekleme kasası için** | Azure sanal makineler: günde bir kez<br/><br/>DPM/MABS tarafından korunan makineler: günde<br/><br/> Makine yedeklenmekte MARS agent'ı kullanarak doğrudan: günde üç kez. 
**Yedekleme kasaları arasında** | Bir bölge içinde yedeklemedir.<br/><br/> Yedeklemek istediğiniz Vm'leri içeren her Azure bölgesi bir kasada ihtiyacınız vardır. Farklı bir bölgeye yedekleyemezsiniz. 
**Kasa Taşı** | Yapabilecekleriniz [kasaları taşıma](https://review.docs.microsoft.com/azure/backup/backup-azure-move-recovery-services-vault) aynı Abonelikteki kaynak grupları arasında veya abonelikler arasında.
**Kasalar arasında veri taşıma** | Yedeklenen verileri kasalar arasında taşıma desteklenmiyor.
**Kasa Depolama türünü değiştirme** | Yedeklemeleri depolanmadan önce bir kasa için depolama çoğaltma türü (GRS/LRS) değiştirebilirsiniz. Yedekleme Kasası'nda başladıktan sonra çoğaltma türü değiştirilemez.



## <a name="on-premises-backup-support"></a>Şirket içi yedekleme desteği

Şirket içi makineleri yedeklemek istiyorsanız, nelerin desteklendiği aşağıda verilmiştir.

**Makine** | **Yedeklenen** | **Konum** | **Özellikler**
--- | --- | --- | ---
**Windows makine MARS Aracısı ile doğrudan yedekleme** | Dosyaları, klasörleri, sistem durumu | Kurtarma Hizmetleri kasasına yedekleme | Yedekleme günde üç kez.<br/><br/> Uygulama durumunu algılayan yedekleme.<br/><br/> Dosya, klasör, birim geri yükleyin.
**MARS Aracısı ile Linux makinesinin doğrudan yedekleme** | Yedekleme desteklenmez.
**DPM yedekleme** | Dosyaları, klasörleri, birimleri, sistem durumu, uygulama verileri. | Yerel DPM depolama alanına yedekleyin. DPM ardından kasaya yedekler. | Uygulama kullanan anlık görüntüler<br/><br/> Yedekleme ve kurtarma için tam ayrıntı düzeyi.<br/><br/> Linux sanal makineleri (Hyper-V/VMware) için desteklenir.<br/><br/>. Oracle desteklenmiyor.
**Geri MABS için** | Dosyaları, klasörleri, birimleri, sistem durumu, uygulama verileri. | MABS yerel depolama alanına yedekleyin. MABS ardından kasaya yedekler. | Uygulama kullanan anlık görüntüler<br/><br/> Yedekleme ve kurtarma için tam ayrıntı düzeyi.<br/><br/> Linux sanal makineleri (Hyper-V/VMware) için desteklenir.<br/><br/>. Oracle desteklenmiyor.


## <a name="azure-vm-backup-support"></a>Azure VM yedekleme desteği

### <a name="azure-vm-limits"></a>Azure VM sınırları

**Sınırı** | **Ayrıntılar**
--- | ---
Azure VM veri diski | 16 sınırı.
Azure VM veri diski boyutu | 4095 GB'a kadar tek bir disk olabilir.


### <a name="azure-vm-backup-options"></a>Azure VM yedekleme seçenekleri

Azure Vm'lerini yedeklemek istiyorsanız, nelerin desteklendiği aşağıda verilmiştir.

**Makine** | **Yedeklenen** | **Konum** | **Özellikler**
--- | --- | --- | ---
**VM uzantısı kullanarak azure VM yedeklemesi** | Tüm VM | Yedekleme kasası | Bir VM için yedeklemeyi etkinleştirdiğinizde yüklü uzantısı.<br/><br/> Yedekleme günde bir kez.<br/><br/> Windows VM'ler için Linux Vm'lerinin dosyayla tutarlı yedekleme için uygulama durumunu algılayan yedekleme. Uygulama tutarlılığı Linux makineler için özel betikler kullanarak yapılandırabilirsiniz.<br/><br/> VM/disk geri yükleyin.<br/><br/> Azure sanal makinesinde, bir şirket içi konuma yedekleyemezsiniz.
**MARS Aracısı'nı kullanarak azure VM yedeklemesi** | Dosyalar, klasörler | Yedekleme kasası | Yedekleme günde üç kez.<br/><br/> MARS Aracısı, tüm VM yerine belirli dosyaları/klasörleri yedeklemek istiyorsanız VM uzantıyla birlikte çalıştırabilirsiniz.
**DPM ile Azure VM** | Dosyaları, klasörleri, birimleri, sistem durumu, uygulama verileri. | Azure VM'de DPM çalıştıran yerel depolama alanına yedekleyin. DPM ardından kasaya yedekler. | Uygulama kullanan anlık görüntüler<br/><br/> Yedekleme ve kurtarma için tam ayrıntı düzeyi.<br/><br/> Linux sanal makineleri (Hyper-V/VMware) için desteklenir.<br/><br/>. Oracle desteklenmiyor.
**MABS ile Azure VM** | Dosyaları, klasörleri, birimleri, sistem durumu, uygulama verileri. | Azure VM MABS çalıştıran yerel depolama birimine yedeklendi. MABS ardından kasaya yedekler. | Uygulama kullanan anlık görüntüler<br/><br/> Yedekleme ve kurtarma için tam ayrıntı düzeyi.<br/><br/> Linux sanal makineleri (Hyper-V/VMware) için desteklenir.<br/><br/> Oracle desteklenmiyor.





## <a name="linux-backup-support"></a>Linux yedekleme desteği

Linux makineleri yedeklemek istiyorsanız, nelerin desteklendiği aşağıda verilmiştir.

**Backup** | **Linux (Azure destekli)**
--- | ---
**Linux çalıştıran şirket içi makinenin doğrudan yedekleme**. | Hayır. MARS Aracısı, yalnızca Windows makinelerde yüklenebilir.
**(Aracı uzantısı kullanarak) Linux çalıştıran Azure VM'yi yedekleme** | Uygulamayla tutarlı Yedekleme kullanarak [özel betikler](backup-azure-linux-app-consistent.md).<br/><br/> Dosya düzeyinde kurtarma.<br/><br/> Bir kurtarma noktası veya diskten bir sanal makine oluşturarak geri yükleyin.
**Şirket içi veya DPM kullanarak Linux çalıştıran Azure VM yedekleme** | Hyper-V ve VMWare üzerinde Linux Konuk VM’lerinin dosyayla tutarlı yedeklemesi<br/><br/> Hyper-V ve VMWare Linux Konuk VM’lerinin VM geri yüklemesi</br></br> Dosyayla tutarlı yedekleme Azure Vm'leri için kullanılamıyor
**Şirket içi makine/Azure VM yedekleme MABS kullanarak Linux çalıştıran** | Hyper-V ve VMWare üzerinde Linux Konuk VM’lerinin dosyayla tutarlı yedeklemesi<br/><br/> Hyper-V ve VMWare Linux Konuk Vm'lerinin VM geri yükleme</br></br> Dosyayla tutarlı yedekleme Azure Vm'leri için kullanılamaz.

## <a name="daylight-saving-support"></a>Gün ışığından yararlanma desteği

Azure Backup, Azure VM yedeklemeleri için ışığından değişikliklerin otomatik saat ayarlama desteklememektedir. Yedekleme ilkelerini el ile gerektiği gibi değiştirin.


## <a name="disk-deduplication-support"></a>Disk yinelenenleri kaldırma desteği

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

Aşağıdaki tabloda özetlendiği gibi yedekleme sıkıştırma, yedekleme trafiğinin destekler. 

- Bu trafiğin sıkıştırılması gerekmez, yani Azure Vm'leri için depolama ağı aracılığıyla doğrudan Azure depolama hesabından veri VM uzantısı okur.
- DPM veya MABS kullanıyorsanız, DPM/bant genişliğinden tasarruf etmek MABS için yedeklenir önce verileri sıkıştırabilirsiniz.

**Makine** | **Sıkıştırma MABS/DPM sunucusuna (TCP)** | **(HTTPS) kasaya Sıkıştır**
--- | --- | ---
**Doğrudan yedekleme, şirket içi Windows makineler** | NA | Evet
**Azure VM uzantısı kullanarak Vm'leri yedekleme** | NA | NA
** MABS/DPM kullanarak şirket içi/Azure makinelerde yedekleme | ![Evet][green] | ![Evet][green]



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

- [Gözden geçirme destek matrisi](backup-support-matrix-iaas.md) Azure VM yedeklemesi için.


[green]: ./media/backup-support-matrix/green.png
[yellow]: ./media/backup-support-matrix/yellow.png
[red]: ./media/backup-support-matrix/red.png
