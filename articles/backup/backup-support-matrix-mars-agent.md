---
title: Microsoft Azure kurtarma Hizmetleri (MARS) aracısı Azure Backup ile çalışan makinelerin yedeklenmesi için destek matrisi
description: Microsoft Azure kurtarma Hizmetleri (MARS) aracısı çalıştıran makineleri yedeklemek, bu makalede Azure Backup desteği özetler.
services: backup
author: rayne-wiselman
ms.service: backup
ms.date: 02/17/2019
ms.topic: conceptual
ms.author: raynew
manager: carmonm
ms.openlocfilehash: 9799914cdabf1f64fccfd6bfd891f9498b860e39
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64922997"
---
# <a name="support-matrix-for-backup-with-the-microsoft-azure-recovery-services-mars-agent"></a>Microsoft Azure kurtarma Hizmetleri (MARS) aracısı ile yedekleme destek matrisi

Kullanabileceğiniz [Azure Backup hizmeti](backup-overview.md) şirket içi makineleri ve uygulamaları yedeklemek için ve Azure sanal makinelerini (VM) yedekleme. Bu makalede, makineleri yedeklemek için Microsoft Azure kurtarma Hizmetleri (MARS) Aracısı'nı kullandığınızda, destek ayarları ve sınırlamaları özetlenmektedir.

## <a name="the-mars-agent"></a>MARS Aracısı

Azure Backup, verileri şirket içi makinelerin ve Azure sanal makinelerini bir yedekleme Azure kurtarma Hizmetleri kasasına yedeklemek için MARS Aracısı kullanır. MARS Aracısı yapabilirsiniz:
- Böylece bunlar doğrudan Azure yedekleme kurtarma Hizmetleri kasası için yedekleme, şirket içi Windows makinelerde çalıştırın.
- Windows sanal makinelerinde çalıştırın, böylece bunlar doğrudan bir kasasına yedekleyebilirsiniz.
- Microsoft Azure Backup sunucusu (MABS) veya bir System Center Data Protection Manager (DPM) sunucusunda çalıştırın. Bu senaryoda, makine ve iş yüklerini MABS veya DPM sunucusuna yedekleme. MARS Aracısı, bu sunucuyu daha sonra azure'da bir kasaya yedekler.

Yedekleme seçeneklerinizin, aracının yüklü olduğu bağlıdır. Daha fazla bilgi için [MARS agent'ı kullanarak Azure Backup mimarisi](backup-architecture.md#architecture-direct-backup-of-on-premises-windows-server-machines-or-azure-vm-files-or-folders). MABS ve DPM yedekleme mimarisi hakkında daha fazla bilgi için bkz: [DPM veya MABS](backup-architecture.md#architecture-back-up-to-dpmmabs). Ayrıca bkz: [gereksinimleri](backup-support-matrix-mabs-dpm.md) yedekleme mimarisi.

**Yükleme** | **Ayrıntılar**
--- | ---
En yeni MARS Aracısı'nı indirme | Kasadan, aracının en son sürümünü indirebilirsiniz veya [doğrudan indirin](https://aka.ms/azurebackup_agent).
Doğrudan bir makineye yükleyin | MARS Aracısı doğrudan şirket içi Windows server veya herhangi birini çalıştıran bir Windows VM'de yükleyebilirsiniz [desteklenen işletim sistemleri](https://docs.microsoft.com/azure/backup/backup-support-matrix-mabs-dpm#supported-mabs-and-dpm-operating-systems).
Bir yedekleme sunucusuna yükleyin | Azure'a yedeklemek için DPM veya MABS ayarlamak, indirin ve MARS aracısının sunucuya yükleyin. Aracıyı yükleyebilirsiniz [desteklenen işletim sistemleri](backup-support-matrix-mabs-dpm.md#supported-mabs-and-dpm-operating-systems) yedek sunucu destek matrisi içinde.

> [!NOTE]
> Varsayılan olarak, yedekleme için etkin bir Azure Vm'leri Azure Backup uzantısı yükleme sahiptir. Bu uzantı, VM'nin tamamını yedekler. Yükleyin ve belirli klasörleri ve dosyaları yerine tam VM yedeklemek istiyorsanız, MARS Aracısı uzantısı yanı sıra Azure sanal makinesinde çalıştırın.
> Bir Azure sanal makinesinde MARS Aracısı çalıştırdığınızda, dosyaları veya klasörleri geçici depolama VM'de bulunan yedekler. Dosya veya klasörleri geçici depolama alanından kaldırılırsa veya geçici depolama kaldırılırsa, yedeklemeleri başarısız olur.


## <a name="cache-folder-support"></a>Önbellek klasör desteği

Verileri yedeklemek için MARS Aracısı'nı kullandığınızda aracı verilerin anlık görüntüsünü alır ve Azure'a veri göndermeden önce bir yerel önbellek klasörüne kaydeder. Önbellek (sıfırdan) klasörü birkaç gereksinim vardır:

**Önbellek** | **Ayrıntılar**
--- | ---
Boyut |  Önbellek klasörü boş alanı, yedekleme verilerinizi toplam boyutunu en az yüzde 5-10 olması gerekir.
Location | Önbellek klasörü, yedeklenen makine üzerinde yerel olarak depolanmalıdır ve çevrimiçi olmalıdır. Önbellek klasörü, bir ağ paylaşımına, çıkarılabilir medya veya çevrimdışı bir birim olmamalıdır.
Klasör | Önbellek klasörü, yinelenenleri kaldırılan bir birimde veya, sıkıştırılmış, seyrek olan veya bir yeniden ayrıştırma noktası olan bir klasörde şifrelenmelidir.
Konum değişiklikleri | Yedekleme Altyapısı durdurarak önbellek konumunu değiştirebilirsiniz (`net stop bengine`) ve önbellek klasörü için yeni bir sürücüye kopyalanması. (Yeni sürücü yeterli boş alan olduğundan emin olun.) Ardından altında iki kayıt defteri girdisini güncelleştirin **HKLM\SOFTWARE\Microsoft\Windows Azure Backup** (**Config/ScratchLocation** ve **Config/CloudBackupProvider/ScratchLocation**) yeni konuma ve yeniden başlatma altyapısı.

## <a name="networking-and-access-support"></a>Ağ ve erişim desteği

### <a name="url-access"></a>URL erişimi

MARS Aracısı şu URL'lere erişimi olmalıdır:

- http://www.msftncsi.com/ncsi.txt
- *.Microsoft.com
- *.WindowsAzure.com
- *.MicrosoftOnline.com
- *.Windows.net

### <a name="throttling-support"></a>Azaltma desteği

**Özellik** | **Ayrıntılar**
--- | ---
Bant genişliği denetimi | Destekleniyor. MARS aracısı kullanmak **özelliklerini değiştirme** bant genişliğini ayarlamak için.
Ağ kapasitesi azaltma | Windows Server 2008 R2, Windows Server 2008 SP2 veya Windows 7 çalıştıran yedeklenen makine için kullanılamaz.

## <a name="support-for-direct-backups"></a>Doğrudan yedeklemeler için destek

Şirket içi makinelerin ve Azure Vm'lerinde çalışan bazı işletim sistemlerinde doğrudan Azure'a yedeklemek için MARS Aracısı'nı kullanabilirsiniz. İşletim sistemlerinin 64 bit olmalıdır ve en son hizmet paketleri ve güncelleştirmelerini çalıştırması olması gerekir. Aşağıdaki tabloda, bu işletim sistemleri özetlenmektedir:

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
Windows Storage Server 2016/2012 R2'in / 2012 (standart, çalışma grubu) | Evet | Hayır

Daha fazla bilgi için [desteklenen MABS ve DPM işletim sistemleri](backup-support-matrix-mabs-dpm.md#supported-mabs-and-dpm-operating-systems).

## <a name="backup-limits"></a>Yedekleme sınırları

Azure yedekleme yedeklenebilecek bir dosya veya klasör veri kaynağı boyutu sınırlar. Tek bir biriminden yedeklediğiniz öğeleri bu tabloda özetlenen bir boyutu sınırı:

**İşletim sistemi** | **Boyut sınırı**
--- | ---
Windows Server 2012 veya üzeri |  54.400 GB
Windows Server 2008 R2 SP1 |    1700 GB
Windows Server 2008 SP2 | 1700 GB
Windows 8 veya üzeri  | 54.400 GB
Windows 7   | 1700 GB


## <a name="supported-file-types-for-backup"></a>Yedekleme için desteklenen dosya türleri

**Tür** | **Destek**
--- | ---
Şifreli   | Destekleniyor.
Sıkıştırılmış | Destekleniyor.
Seyrek | Destekleniyor.
Sıkıştırılmış ve aralıklı | Destekleniyor.
Sabit bağlantılar  | Desteklenmiyor. Atlandı.
Yeniden ayrıştırma noktası   | Desteklenmiyor. Atlandı.
Şifrelenmiş ve aralıklı |  Desteklenmiyor. Atlandı.
Sıkıştırılmış akış   | Desteklenmiyor. Atlandı.
Aralıklı akış   | Desteklenmiyor. Atlandı.
OneDrive (eşitlenmiş dosyaları seyrek akışlarıdır)  | Desteklenmiyor.

## <a name="supported-drives-or-volumes-for-backup"></a>Desteklenen sürücüler veya yedekleme birimleri

**Sürücü/birim** | **Destek** | **Ayrıntılar**
--- | --- | ---
Salt okunur birimler   | Desteklenmiyor | Birim kopyalama gölge hizmeti (VSS) birim yazılabilir ise çalışır.
Çevrimdışı birimler | Desteklenmiyor |   VSS yalnızca birim çevrimiçi ise çalışır.
Ağ paylaşımı   | Desteklenmiyor |   Birim sunucuda yerel olmalıdır.
BitLocker korumalı birimler | Desteklenmiyor |   Yedekleme başlamadan önce birimin kilidinin açık olması gerekir.
Dosya sistemi tanımlaması  | Desteklenmiyor |   Yalnızca NTFS desteklenmiyor.
Çıkarılabilir medya | Desteklenmiyor |   Tüm yedekleme öğesi kaynaklarının olmalıdır bir *sabit* durumu.
Yinelenenleri kaldırılan sürücüler | Desteklenen | Azure yedekleme, yinelenenleri kaldırılmış verileri normal verilere dönüştürür. En iyi duruma getirir, şifreler, depolar ve veriler kasaya gönderir.

## <a name="support-for-initial-offline-backup"></a>Çevrimdışı ilk yedekleme desteği

Azure Backup destekler *çevrimdışı dengeli dağıtım* diskleri kullanarak ilk yedek verileri Azure'a aktarmak için. Bu destek, ilk yedekleme terabayt (TB'a) boyutu aralığında olması olasılığı varsa yararlıdır. Çevrimdışı Yedekleme için desteklenir:

- MARS Aracısı çalıştıran şirket içi makineleri üzerindeki dosya ve klasörleri doğrudan yedeğini.
- İş yükleri ve bir DPM sunucusu veya MABS dosyaları yedekleme.

Çevrimdışı Yedekleme sistem durumu dosyaları için kullanılamaz.

## <a name="support-for-data-restoration"></a>Verileri geri yükleme desteği

Kullanarak [anında geri yükleme](backup-instant-restore-capability.md) Özelliği Azure Backup, kasaya kopyalamadan önce verileri geri yükleyebilirsiniz. .NET Framework 4.5.2 yedekleme makine çalıştırılması gerekir ya da daha yüksek.

İşletim sisteminin önceki bir sürümünü çalıştıran bir hedef makine yedekleri geri yüklenemez. Örneğin, Windows 7 çalıştıran bir bilgisayardan gerçekleştirilen bir yedekleme Windows 8 veya daha sonra geri yüklenebilir. Ancak, Windows 7 çalıştıran bir bilgisayarda Windows 8 çalıştıran bir bilgisayardan gerçekleştirilen bir yedekleme geri yüklenemez.

## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinin [yedekleme MARS Aracısı kullanan mimarisi](backup-architecture.md#architecture-direct-backup-of-on-premises-windows-server-machines-or-azure-vm-files-or-folders).
- Ne zaman desteklemiştir öğrenin, [MARS Aracısı MABS veya bir DPM sunucusunda çalıştırın](backup-support-matrix-mabs-dpm.md).
