---
title: Microsoft Azure kurtarma Hizmetleri (MARS) aracısı Azure Backup ile çalışan makinelerin yedeklenmesi için destek matrisi
description: Microsoft Azure kurtarma Hizmetleri (MARS) Aracısı'nı çalıştıran makineleri yedeklemek, bu makalede Azure Backup desteği özetler.
services: backup
author: rayne-wiselman
ms.service: backup
ms.date: 02/17/2019
ms.topic: conceptual
ms.author: raynew
manager: carmonm
ms.openlocfilehash: 61afefb955914c75606c4fff36ebcc05a4ad0057
ms.sourcegitcommit: 15e9613e9e32288e174241efdb365fa0b12ec2ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2019
ms.locfileid: "57010914"
---
# <a name="support-matrix-for-backup-with-the-microsoft-azure-recovery-services-mars-agent"></a>Microsoft Azure kurtarma Hizmetleri (MARS) aracısı ile yedekleme destek matrisi

Kullanabileceğiniz [Azure Backup hizmeti](backup-overview.md) şirket içi makineleri ve uygulamaları ve Azure sanal makinelerini yedeklemek için. Bu makalede, destek ayarları ve Microsoft Azure kurtarma Hizmetleri (MARS) aracısı ile makineleri yedeklemeyi sınırlamaları özetlenmektedir.

## <a name="about-the-mars-agent"></a>MARS Aracısı hakkında

MARS Aracısı Azure Backup tarafından Azure'da şirket içi makinelerin ve Azure sanal makinelerini bir yedekleme kurtarma Hizmetleri verileri başka bir yedekleme kasası için kullanılır. MARS aracısı aşağıdaki gibi kullanılabilir:
- Böylece bunlar doğrudan Azure yedekleme kurtarma Hizmetleri kasası için Yedekleme aracısı şirket içi Windows makineler üzerinde çalıştırın.
- Böylece doğrudan bir kasasına yedekleyebilirsiniz aracı Windows Azure Vm'leri üzerinde çalışacak.
- Bir Microsoft Azure Backup sunucusu (MABS) veya bir System Center Data Protection - Manager (DPM) sunucu Aracısı'nı çalıştırın. Bu senaryoda, MABS/DPM makineler ve iş yüklerini yedeklemek ve ardından MARS Aracısı'nı kullanarak azure'daki bir kasasına yedeklemeye MABS/DPM desteklenir. 

Neleri yedekleyebilir, aracının yüklü olduğu bağlıdır.

- [Daha fazla bilgi edinin](backup-architecture.md#architecture-direct-backup-of-on-premises-windows-machinesazure-vm-filesfolders) MARS Aracısı'nı kullanarak yedekleme mimarisi hakkında.
- MABS/DPM hakkında daha fazla bilgi [yedekleme mimarisi](backup-architecture.md#architecture-back-up-to-dpmmabs)ve [gereksinimleri](backup-support-matrix-mabs-dpm.md).


## <a name="supported-installation"></a>Desteklenen yükleme

**Yükleme** | **Ayrıntılar**
--- | ---
**En yeni MARS Aracısı'nı indirme** | Kasadan, aracının en son sürümünü indirebilirsiniz veya [doğrudan indirin](https://aka.ms/azurebackup_agent).
**Doğrudan bir makineye yükleyin** | MARS Aracısı doğrudan şirket içi Windows server veya Windows Azure herhangi birini çalıştıran VM yükleyebilirsiniz [desteklenen işletim sistemleri](backup-support-matrix-mabs-dpm.md#supported-mabs-and-dpm-operating-systems).
**Bir yedekleme sunucusuna yükleyin** | Azure'a yedeklemek için DPM veya MABS ayarlamak, indirin ve MARS aracısının sunucuya yükleyin. Aracı ile uyumlu olarak yüklenebilir [desteklenen işletim sistemleri](backup-support-matrix-mabs-dpm.md#supported-mabs-and-dpm-operating-systems) yedek sunucu destek matrisi içinde.

> [!NOTE]
> Varsayılan Azure Vm'leri için yedekleme etkin bir Azure Backup uzantısı yükleme sahip. Bu uzantı, VM'nin tamamını yedekler. Yükleyin ve belirli klasörleri ve dosyaları yerine tam VM yedeklemek istiyorsanız, MARS Aracısı uzantısı yanı sıra Azure sanal makinesinde çalıştırın.
> Bir Azure sanal makinesinde MARS Aracısı çalıştırdığınızda, geçici depolama VM'de bulunan dosyaların/klasörlerin yedekler. Dosyaları/klasörleri geçici depolama alanından kaldırılırsa veya geçici depolama kaldırılırsa, yedeklemeler başarısız olur.


## <a name="cache-folder-support"></a>Önbellek klasör desteği

MARS Aracısı ile yedekleme, aracı verilerin anlık görüntüsünü alır ve Azure'a göndermeden önce bir yerel önbellek klasörüne kaydeder. Önbellek (sıfırdan) klasörü çeşitli gereksinimler vardır.

**Önbellek** | **Ayrıntılar**
--- | ---
**Önbellek boyutu** |  Yedekleme verilerinizi toplam boyutunu en az 5-%10 önbelleği klasör boyutunu boş alan olmalıdır. 
**Önbellek konumu** | Önbellek klasörü, yedeklenmekte olan makinenin için yerel olmalıdır ve çevrimiçi olması gerekir.<br/><br/> Önbellek klasörü, bir ağ paylaşımına, çıkarılabilir medya veya çevrimdışı bir birim olmamalıdır. 
**Önbellek klasörü** | Önbellek klasörü, yinelenenleri kaldırılan bir birimde veya sıkıştırılmış/seyrek/ayrıştırma-noktasıdır bir klasörde şifrelenmelidir
**Önbellek konumunu değiştirme** | Yedekleme Altyapısı (net stop bengine) durduruluyor ve önbellek klasörü yeni bir sürücüye kopyalanması önbellek konumunu değiştirebilirsiniz (yeterli alana sahip olduğundan emin olun). Ardından yeni konumuna HKLM\SOFTWARE\Microsoft\Windows Azure Backup (Config/ScratchLocation ve CloudBackupProvider/Config/ScratchLocation) altında iki kayıt defteri girdisini güncelleştirin ve altyapısını yeniden başlatın.

## <a name="networking-and-access-support"></a>Ağ ve erişim desteği

### <a name="url-access"></a>URL erişimi

MARS Aracısı şu URL'lere erişimi olmalıdır:

- http://www.msftncsi.com/ncsi.txt
- *.Microsoft.com
- *.WindowsAzure.com
- *.microsoftonline.com
- *.windows.net

### <a name="throttling-support"></a>Azaltma desteği

**Özellik** | **Ayrıntılar**
--- | ---
Bant genişliği denetimi | Desteklenen<br/><br/> Kullanım **özelliklerini değiştirme** bant genişliğini ayarlamak için MARS Aracısı.
Ağ kapasitesi azaltma | Desteklenen Windows Server 2008 R2, Windows Server 2008 SP2 veya Windows 7 çalıştıran makineleri için kullanılabilir değil.

## <a name="support-for-direct-backups"></a>Doğrudan yedeklemeler için destek

MARS Aracısı ile azure'a doğrudan şirket içi makinelerin ve Azure Vm'lerinde çalışan hangi işletim sistemleri yedeklenebilir aşağıdaki tabloda özetlenmiştir.

- Tüm işletim sistemlerine, 64-bittir.
- Tüm işletim sistemlerini en son hizmet paketleri ve güncelleştirmeler çalıştırıyor olmalıdır.
- Hangi DPM ve MABS sunucuları yedeklenebilir MARS Aracısı ile ilgili ayrıntılar için gözden [bu tabloda](backup-support-matrix-mabs-dpm.md#supported-mabs-and-dpm-operating-systems).

**İşletim sistemi** | **Dosyaların/klasörlerin** | **Sistem durumu**
--- | --- | ---
Windows 10 (Enterprise, Pro, giriş) | Evet | Hayır
Windows 8.1 (Enterprise, Pro)| Evet |Hayır
Windows 8 (Enterprise, Pro) | Evet | Hayır
Windows 7 (Ultimate, Enterprise, Pro, Home Premium/temel, Başlangıç) | Evet | Hayır
Windows Server 2016 (Standard, Datacenter, Essentials) | Evet | Evet
Windows Server 2012 R2 (Standard, Datacenter, Foundation, Essentials) | Evet | Evet
Windows Server 2012 (Standard, Datacenter, Foundation) | Evet | Evet
Windows Server 2008 R2 (Standard, Enterprise, Datacenter, Foundation) | Evet | Evet
Windows Server 2008 SP2 (Standard, Datacenter, Foundation) | Evet | Hayır
Windows Storage Server 2016/2012 R2'in / 2012 (standart, çalışma grubu | Evet | Hayır


## <a name="backup-limits"></a>Yedekleme sınırları

Azure yedekleme yedeklenebilecek dosya/klasör bir veri kaynağı boyutu sınırı yerleştirir. Tek bir birim yedeklemesi için seçilen öğelerin boyutu tabloda özetlenen boyutları aşamaz.

**İşletim sistemi** | **Boyut sınırı**
--- | ---
Windows Server 2012 veya üzeri |  54.400 GB
Windows Server 2008 R2 SP1 |    1700 GB
Windows Server 2008 SP2 | 1700 GB
Windows 8 veya üzeri  | 54.400 GB
Windows 7   | 1700 GB


## <a name="supported-file-types-for-backup"></a>Yedekleme için desteklenen dosya türleri

**Tür** | **Destekleniyor** 
--- | --- 
Şifreli   | Evet 
Sıkıştırılmış | Evet
Seyrek | Evet 
Sıkıştırılmış ve aralıklı | Evet
Sabit bağlantılar  | Desteklenmiyor<br/><br/> Atlandı
Yeniden ayrıştırma noktası   | Desteklenmiyor<br/><br/> Atlandı
Şifrelenmiş ve aralıklı |  Desteklenmiyor<br/><br/> Atlandı
Sıkıştırılmış akış   | Desteklenmiyor<br/><br/> Atlandı
Aralıklı akış   | Desteklenmiyor<br/><br/> Atlandı
Bir sürücü (eşitlenmiş dosyaları seyrek akışlarıdır)    | Desteklenmiyor 

## <a name="supported-drivesvolumes-for-backup"></a>Desteklenen sürücüler/birimler yedekleme

**Sürücü/birim** | **Destekleniyor** | **Ayrıntılar**
--- | --- | ---
Salt okunur birimler   | Desteklenmiyor | Toplu iş için VSS yazılabilir olmalıdır.
Çevrimdışı birimler | Desteklenmiyor |   Birimin çalışması VSS'ye yönelik olarak çevrimiçi olması gerekir.
Ağ paylaşımı   | Desteklenmiyor |   Birim, yedekleme için sunucuda yerel olmalıdır.
BitLocker korumalı birimler | Desteklenmiyor |   Birim önce çalışmaya yedekleme için kilidi gerekir.
Dosya sistemi tanımlaması  | Desteklenmiyor |   Yalnızca NTFS.
Çıkarılabilir medya | Desteklenmiyor |   Tüm yedekleme öğesi kaynaklarının sabit durumunu olması gerekir.
Yinelenenleri kaldırılan sürücüler | Destekleniyor.<br/><br/> Azure yedekleme, yinelenenleri kaldırılmış verileri normal verilere dönüştürür. Bu verileri iyileştirir ve şifreler, depolar ve kasaya gönderir.

## <a name="support-for-initial-offline-backup"></a>Çevrimdışı ilk yedekleme desteği

Azure Backup destekler "Çevrimdışı dengeli dağıtım" diskler kullanarak Azure'a ilk yedekleme verilerini aktarmak için. İlk yedeklemeniz terabayt (TB'a) aralığında olması olasılığı varsa, bu yararlıdır. Çevrimdışı Yedekleme için desteklenir:

- MARS Aracısı çalıştıran şirket içi makineleri üzerindeki dosya ve klasörleri doğrudan yedeğini.
- İş yükleri ve bir DPM sunucusu veya MABS dosyaları yedekleme.
- Çevrimdışı Yedekleme sistem durumu dosyaları için kullanılamaz.


## <a name="support-for-restore"></a>Geri yükleme desteği

- Yeni [anında geri yükleme](backup-instant-restore-capability.md) Azure Backup sürümü kasaya kopyalanmış önce verileri geri yüklemenize olanak sağlar.<br/><br/> Bu özelliği kullanmak için yedeklenen makine .NET Framework 4.5.2 çalıştırılması gerekir ya da daha yüksek.
- İşletim sisteminin önceki bir sürümü çalıştıran bir hedef makine yedekleri geri yüklenemez. Örneğin, Windows 7 bilgisayarda gerçekleştirilen bir yedekleme Windows 8 veya daha sonra geri yüklenebilir. Ancak, Windows 7 bilgisayara Windows 8 bilgisayarda gerçekleştirilen bir yedekleme geri yüklenemez.


## <a name="next-steps"></a>Sonraki adımlar
- [Daha fazla bilgi edinin](backup-architecture.md#architecture-direct-backup-of-on-premises-windows-machinesazure-vm-filesfolders) MARS Aracısı ile yedekleme mimarisi hakkında.
- [Bilgi](backup-support-matrix-mabs-dpm.md) MARS Aracısı Microsoft Azure Backup sunucusu (MABS) veya System Center DPM üzerinde çalıştırdığınızda ne desteklenir.


