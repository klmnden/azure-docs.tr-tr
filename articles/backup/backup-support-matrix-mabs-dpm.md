---
title: Microsoft Azure Backup sunucusu ve System Center DPM destek matrisi
description: Şirket içi ve Azure VM kaynakları yedeklemek için Microsoft Azure Backup sunucusu veya System Center DPM kullanırken bu makalede Azure Backup desteği özetler.
services: backup
author: rayne-wiselman
ms.service: backup
ms.date: 02/17/2019
ms.topic: conceptual
ms.author: raynew
manager: carmonm
ms.openlocfilehash: 0544782763bebac42b4b68fda42cff80b18ff067
ms.sourcegitcommit: 8ca6cbe08fa1ea3e5cdcd46c217cfdf17f7ca5a7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/22/2019
ms.locfileid: "56670743"
---
# <a name="support-matrix-for-backup-with-microsoft-azure-backup-serversystem-center-dpm"></a>Microsoft Azure Backup sunucusu/System Center DPM ile yedekleme için destek matrisi

Kullanabileceğiniz [Azure Backup hizmeti](backup-overview.md) şirket içi makinelerin ve iş yükleri ve Azure sanal makinelerini yedeklemek için. Bu makalede, destek ayarları ve Microsoft Azure Backup sunucusu (MABS) veya System Center Data Protection Manager (DPM) ve Azure Backup kullanarak makineleri yedekleme için sınırlamaları özetlenmektedir.

## <a name="about-mabsdpm"></a>MABS/DPM hakkında

[System Center DPM](https://docs.microsoft.com/system-center/dpm/dpm-overview?view=sc-dpm-1807) yapılandırır, kolaylaştırır ve yedekleme ve kurtarma enterprise makineleri ve verileri yöneten kurumsal bir çözümdür. Parçasıdır [System Center](https://www.microsoft.com/cloud-platform/system-center-pricing) ürün paketi.


Microsoft Azure Backup sunucusu (MABS) şirket içi fiziksel sunucuları, sanal makineleri (VM'ler) ve bunları ve Azure Vm'lerinde çalışan uygulamaları yedeklemek için kullanılan sunucusunun bir üründür.

- MABS, System Center Data Protection Manager (DPM) üzerinde temel alır ve birkaç farklılıkla ile benzer bir işlevsellik sağlar:
    - System Center lisans, MABS çalıştırmak için gereklidir.
    - MABS hem de DPM Azure için uzun vadeli yedekleme alanı sağlar. Ayrıca, DPM verileri uzun vadeli depolama için banda yedeklemek sağlar. MABS, bu işlevsellik sağlamaz.
- MABS sunucudan [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=57520). Şirket içinde çalıştırılabilir veya azure'da bir Azure sanal makinesi üzerinde.

DPM ve MABS çok çeşitli uygulamaları ve sunucu ve istemci işletim sistemleri yedekleme destekler. Birden çok yedekleme senaryoları sağlar:
    - Makine düzeyinde sistem durumu veya tam yedekleme ile yedekleyebilirsiniz.
    - Belirli birimler, paylaşımlar, klasörler ve dosyaları yedekleyebilirsiniz.
    - Belirli uygulamalarını en iyi duruma getirilmiş bir uygulama ile uyumlu ayarları kullanarak yedekleyebilirsiniz.

## <a name="mabsdpm-backup"></a>MABS/DPM yedeklemesi

MABS/DPM ve Azure Backup kullanarak yedekleme gibi çalışır:

1. DPM/MABS koruma aracısının yedeklenecek her makinede yüklü. 
2. DPM/MABS makineleri/uygulamaları yerel depolama birimine yedeklenir.
3. Microsoft Azure kurtarma Hizmetleri (MARS) aracısı, DPM sunucusu/MABS yüklenir.
4. MARS Aracısı DPM/MABS disklerin yedek bir kurtarma Hizmetleri kasasına Azure Yedekleme'yi kullanarak azure'da yedekler.

Daha fazla bilgi için:
- [Daha fazla bilgi edinin](backup-architecture.md#architecture-back-up-to-dpmmabs) MABS mimarisi hakkında.
- [Gözden geçirme](backup-support-matrix-mars-agent.md) ne MARS aracısı için desteklenir.

## <a name="supported-scenarios"></a>Desteklenen senaryolar 

**Senaryo** | **Aracı** | **Konum**
--- | --- | ---
**Şirket içi makineleri/iş yüklerini yedekleme** | DPM/MABS koruma aracısını yedeklemek istediğiniz makineleri çalışır<br/><br/> MARS Aracısı MABS/DPM sunucusunda | Şirket içi MABS/DPM çalıştırılması gerekir
**Azure Vm'leri/iş yüklerini yedekleme** | DPM/MABS koruma aracısını korumalı makine<br/><br/> MARS Aracısı'nı MABS/DPM | DPM/MABS, bir Azure sanal makinesinde çalıştırmalıdır.

## <a name="supported-deployments"></a>Desteklenen dağıtımlar

DPM/MABS, tabloda özetlendiği gibi dağıtılabilir.

**Dağıtım** | **Destek** | **Ayrıntılar**
--- | --- | ---
**Şirket içinde dağıtılabilir** | Fiziksel sunucu<br/><br/>Hyper-V VM<br/><br/> VMware VM | VMware VM olarak DPM/MABS yüklü değilse yalnızca VMware Vm'leri ve bu sanal makineler üzerinde çalışan iş yüklerini yedekler.
**Azure Stack VM dağıtılır** | Yalnızca MABS | DPM, Azure Stack Vm'lerini yedekleme için kullanılamaz.
**Azure VM dağıtılır** | Azure sanal makineleri ve bu sanal makineler üzerinde çalışan iş yüklerini korur. | MABS/DPM Azure'da çalışan şirket içi makineleri yedekleyemezsiniz.


## <a name="supported-mabs-and-dpm-operating-systems"></a>Desteklenen MABS ve DPM işletim sistemleri

Azure yedekleme, MABS/aşağıdaki işletim sistemlerinden birini çalıştıran DPM'yi yedekleyebilirsiniz. İşletim sistemlerini en son hizmet paketleri ve güncelleştirmeler çalıştırıyor olmalıdır.

**Senaryo** | **MABS/DPM** 
--- | --- 
**Bir Azure VM'de MABS** | Windows Server 2012 R2<br/><br/> Windows 2016 Datacenter<br/><br/> Windows 2019 veri merkezi<br/><br/> Marketten bir görüntü ile başlamanız önerilir.<br/><br/> En düşük A2 standart iki çekirdek ve 3,5 GB RAM. 
**Bir Azure VM'de DPM** | System Center 2012 R2 Update 3 veya sonraki bir sürümü.<br/><br/> Windows işletim sistemi olarak [System Center tarafından gerekli](https://docs.microsoft.com/system-center/dpm/prepare-environment-for-dpm?view=sc-dpm-1807#dpm-server).<br/><br/> Marketten bir görüntü ile başlamanız önerilir.<br/><br/> En düşük A2 standart iki çekirdek ve 3,5 GB RAM. 
**MABS şirket içi** | 64-bit işletim sistemlerinde desteklenir:<br/><br/> MABS v3 ve sonraki sürümler: Windows Server 2019 (Standard, Datacenter, Essentials) <br/><br/> MABS v2 ve sonraki sürümler: Windows Server 2016 (Standard, Datacenter, Essentials)<br/><br/> Tüm MABS sürümleri: Windows Server 2012 R2/2012 (Standard, Datacenter, Foundation)<br/><br/>Tüm MABS sürümleri: Windows Storage Server 2012 R2/2012 (standart/çalışma grubu)
**DPM şirket içi** | Fiziksel sunucusu/Hyper-V VM: System Center 2012 SP1 veya üzeri.<br/><br/> VMware VM: System Center 2012 R2 güncelleştirme 5 veya üzeri. 



## <a name="management-support"></a>Yönetim desteği
**Sorunu** | **Ayrıntılar**
--- | ---
**Yükleme** | DPM/MABS, tek amaçlı bir makineye yüklenmesi gerekir.<br/><br/> DPM/MABS, bir etki alanı denetleyicisinde, uygulama sunucusu rolü yüklemesiyle bir makinede, Exchange Server veya System Center Operations Manager çalıştıran bir makineye veya bir küme düğümünde yüklemeyin.<br/><br/> [Gözden geçirme](https://docs.microsoft.com/system-center/dpm/prepare-environment-for-dpm?view=sc-dpm-1807#dpm-server) tüm DPM sistem gereksinimleri.
**Etki alanı** | DPM sunucusu/MABS bir etki alanına katılması. Önce yükleyin ve ardından DPM/MABS bir etki alanına ekleyin. Dağıtım sonra DPM/MABS yeni bir etki alanına taşıma desteklenmiyor.
**Depolama** | Modern yedekleme depolama alanı (MB), DPM 2016/MABS v2'den başlayarak desteklenir. MABS v1 için kullanılamaz.
**MABS yükseltme** | Doğrudan, MABS v3 yükleme veya MABS v3 MABS v2'den yükseltme. [Daha fazla bilgi edinin](backup-azure-microsoft-azure-backup.md#upgrade-mabs).
**MABS taşıma** | MB kullanıyorsanız, MABS depolama korurken yeni bir sunucuya taşınması desteklenir.<br/><br/> Sunucunun özgün adıyla aynı olmalıdır. Aynı depolama havuzuna tutmak ve veri kurtarma noktalarını depolamak için aynı MABS veritabanı kullanmak istiyorsanız adı değiştirilemiyor.<br/><br/> Geri yüklemek gerekeceği için MABS veritabanının bir yedeğini gerekir.




## <a name="mabs-support-on-azure-stack"></a>Azure Stack'te MABS desteği

Tek bir konumdan Azure Stack Vm'leri ve iş yüklerini yedekleme yönetebilmeniz için Azure Stack VM'deki MABS dağıtabilirsiniz.

**Bileşen** | **Ayrıntılar**
--- | --- 
**Azure Stack VM üzerinde MABS** | En az A2 boyutu. Azure Market'ten bir Windows Server 2012 R2 veya Windows Server 2016 görüntüsü ile başlamanız önerilir.<br/><br/> Başka bir şey MABS VM'de yüklememelisiniz.
**MABS depolama** | MAB VM için ayrı bir depolama hesabı kullanın. MABS üzerinde çalışan MARS aracısının, bir önbellek konumuna ve buluttan veri geri yükleme tutmak için geçici depolama gerekir.
**MABS depolama havuzu** | MABS depolama havuzunun boyutunu, sayısını ve MABS VM'ye disk boyutu tarafından belirlenir. Her Azure Stack VM boyutu en fazla sayıda diske sahip. Örneğin, dört diskleri A2 olur.
**MABS bekletme** | Beş günden fazla bir süre için yerel MABS disk üzerindeki yedeklenen verileri korumanız gerekir.
**MABS ölçeği artırma** | Dağıtımınızın ölçeğini artırmak için MABS VM'nin boyutunu artırabilirsiniz. Örneğin A D serisi için.<br/><br/> Ayrıca, verileri Azure yedekleme ile düzenli olarak boşaltma ve gerekirse ek MABS sunucuları dağıtabilirsiniz emin olabilirsiniz.
.NET framework MABS | MABS VM gerekli .NET Framework 3.3 SP1 veya üzeri yüklü.
**MABS etki alanı** | MABS VM bir etki alanına katılması gerekir. Yönetici ayrıcalıklarına sahip bir etki alanı kullanıcı MABS, VM üzerinde yüklemeniz gerekir.
**Azure Stack VM verilerini yedekleme** | Dosyaları, klasörleri ve uygulamaları yedekleyebilirsiniz.
**Desteklenen yedekleme** | Bu işletim sistemleri, yedeklemek istediğiniz sanal makineler için desteklenir:<br/><br/> Windows Server yarı yıllık kanal (veri merkezi/Enterprise/standart)<br/><br/> Windows Server 2016/2012 R2/2012/2008 R2.
**Azure Stack Vm'leri için SQL desteği** | SQL Server 2016/2014/2012 SP1 ' yedekleyin.<br/><br/> Yedeklemek ve veritabanını kurtarın.
**Azure Stack VM'ler için SharePoint desteği** | SharePoint 2016/2013/2010.<br/><br/> Yedekleme ve kurtarma grubu, veritabanı, ön uç, web sunucusu.
**Yedeklenen sanal makineleri için ağ gereksinimleri** | Azure Stack iş yükünde tüm VM'ler aynı Vnet'e ve aynı aboneliğe ait gerekir.


## <a name="dpmmabs-networking-support"></a>DPM/MABS ağ desteği

### <a name="url-access"></a>URL erişimi

DPM sunucusu/MABS şu URL'lere erişimi olmalıdır:

- http://www.msftncsi.com/ncsi.txt
- *.Microsoft.com
- *.WindowsAzure.com
- *.microsoftonline.com
- *.windows.net

### <a name="dpmmabs-connectivity-to-azure-backup"></a>Azure yedekleme DPM/MABS bağlantısı

Azure Backup hizmeti bağlantısı düzgün çalışması yedeklemeler için gereklidir ve Azure aboneliğinin etkin olmalıdır. Aşağıdaki tabloda, bu iki şey oluşmaz davranış gösterir.

**MABS azure'a** | **Abonelik** | **Yedekleme/geri yükleme** 
--- | --- | --- 
Bağlanıldı | Etkin | DPM/MABS diske yedekleme<br/><br/> Azure’da Yedekleme<br/><br/> Diskten geri yükleme<br/><br/> Azure'dan geri yükleme
Bağlanıldı | Süresi doldu ve sağlaması | Disk ya da Azure yedekleme.<br/><br/> Abonelik süresi dolmuşsa, disk ya da Azure geri yükleyebilirsiniz.<br/><br/> Abonelik yetkisi alınmış ise disk ya da Azure geri yükleyemezsiniz. Azure kurtarma noktası silinir.
15 günden fazla bir bağlantı yok | Etkin | Disk ya da Azure yedekleme.<br/><br/> Disk ya da Azure geri yükleyebilirsiniz.
15 günden fazla bir bağlantı yok | Süresi doldu ve sağlaması | Disk ya da Azure yedekleme.<br/><br/> Abonelik süresi dolmuşsa, disk ya da Azure geri yükleyebilirsiniz.<br/><br/> Abonelik yetkisi alınmış ise disk ya da Azure geri yükleyemezsiniz. Azure kurtarma noktası silinir.



## <a name="dpmmabs-storage-support"></a>DPM/MABS depolama desteği

DPM/MABS Yedeklenen veriler, yerel disk depolama alanında depolanır. 

**Depolama** | **Ayrıntılar**
--- | ---
**MB** | Modern yedekleme depolama alanı (MB), DPM 2016/MABS v2'den başlayarak desteklenir. MABS v1 için kullanılamaz.
**Azure sanal makinesinde MABS depolama** | Verileri DPM/MABS VM'ye ve DPM/MABS yönetilen Azure disklerinde depolanır. DPM/MABS depolama havuzu için kullanılan disk sayısı, sanal makinenin boyutuyla sınırlıdır.<br/><br/> A2 VM: 4. disks; A3 VM: 8 disk; A4 VM: en fazla 1 TB'ın her disk için 16 disk. Bu, kullanılabilir toplam yedekleme depolama havuzunu belirler.<br/><br/> Sayı ve ekli disklerin boyutu Yedekleyebileceğiniz veri miktarına bağlıdır.
**Azure sanal makinesinde MABS veri saklama** | Bir gün DPM/MABS Azure disk üzerindeki verileri korumak ve daha uzun bekletme süresi için kasa DPM/MABS yedekleme öneririz. Bu nedenle, Azure Yedekleme'ye boşaltarak daha büyük miktarda veri koruyabilirsiniz.


### <a name="modern-backup-storage-mbs"></a>Modern yedekleme depolama alanı (MB)
DPM 2016/MABS v2'den (Windows Server 2016 çalıştıran) ve sonraki sürümlerde, modern yedekleme depolama alanı (MBS) yararlanabilir.

- MB yedeklemeler, dayanıklı dosya sistemi (ReFS) diskte depolanır.
- MB kullandığı ReFS kopyalama engelleme ve daha hızlı yedekleme ve daha verimli depolama alanı kullanır.
- Yerel DPM/MABS depolama havuzuna birimler eklediğinizde, sürücü harfleri ile yapılandırılmış. Bu gibi durumlarda, iş yükü depolama ardından farklı birimlerde yapılandırabilirsiniz.
- DPM/MABS için verileri yedeklemek için koruma grupları oluşturduğunuzda, kullanmak istediğiniz sürücüyü seçin. Örneğin, yedeklemeleri SQL veya diğer yüksek IOPS iş yükleri için yüksek performanslı bir sürücü ve daha düşük bir performans sürücüsünde daha az sıklıkla yedeklenen iş yüklerini saklayabilirsiniz.


## <a name="supported-backups-to-mabs"></a>MABS desteklenen yedeklemeler

Aşağıdaki tabloda, hangi MABS için şirket içi makinelerin ve Azure Vm'leri yedeklenebilir özetlenmektedir.


**Backup** | **Sürümleri** | **MABS** | **Ayrıntılar**
--- | --- | --- | ---
**Windows 10, 8.1, 8, 7**<br/><br/>(64/32-bit) | MABS v3, v2, V1 | Şirket içi | Birim/paylaşım/klasör/dosya<br/><br/> Desteklenen yinelenenleri kaldırılmış birimler<br/><br/> Birimler en az 1 GB ve NTFS olmalıdır.
**Windows Server 2016 (Datacenter, Standard, nano hariç)**<br/><br/> 64/32 bit | MABS v3, v2 | Şirket içi/Azure VM.| Birim/paylaşım/klasör/dosya; Sistem-durumu/tam<br/><br/> Yinelenenleri kaldırılmış birimler desteklenir.
**Windows Server 2012 R2 (Datacenter ve Standard)**<br/><br/> 64/32 bit | MABS v3, v2, v1 | Şirket içi/Azure VM. | **Şirket içi koruma**: Birim/paylaşım/klasör/dosya; Sistem-durumu/tam<br/><br/> **Azure VMprotection**: Birim/paylaşım/klasör/dosya<br/><br/> Yinelenenleri kaldırılmış birimler desteklenir.
**Itanium tabanlı sistemler için Windows Server 2012 SP1 (Datacenter ve Standard)**<br/><br/> 64/32 bit | MABS v3, v2, v1<br/><br/> [Windows Management Framework 4.0](https://www.microsoft.com/download/details.aspx?id=40855) yüklü olması gerekir. | Şirket içi/Azure VM. | **Şirket içi koruma**: Birim/paylaşım/klasör/dosya; Sistem-durumu/tam<br/><br/> **Azure VM koruma**: Birim/paylaşım/klasör/dosya<br/><br/> Yinelenenleri kaldırılmış birimler desteklenir.
**Windows Server 2012 (Datacenter ve Standard)**<br/><br/> 64/32 bit | MABS v1 | Şirket içi/Azure VM. | **Şirket içi koruma**: Birim/paylaşım/klasör/dosya; Sistem-durumu/tam<br/><br/> **Azure VM koruma**: Birim/paylaşım/klasör/dosya<br/><br/> Yinelenenleri kaldırılmış birimler desteklenir.
**Windows 2008 R2 SP1 (Standard ve Enterprise)**<br/><br/> 64/32 bit | MABS v3, v2, v1 tarafından desteklenmiyor.<br/><br/> [Windows Management Framework 4.0](https://www.microsoft.com/download/details.aspx?id=40855) yüklü olması gerekir. | Şirket içi/Azure VM. |   **Şirket içi koruma**: Birim/paylaşım/klasör/dosya; Sistem-durumu/tam<br/><br/> **Azure VM koruma**: Birim/paylaşım/klasör/dosya<br/><br/> Yinelenenleri kaldırılmış birimler desteklenir.
**Windows 2008 R2 (Standard ve Enterprise)**<br/><br/> 64/32 bit | MABS v1. MABS için SP1 v2/v3 işletim sistemini çalıştırmalıdır. | Şirket içi/Azure VM. | **Şirket içi koruma**: Birim/paylaşım/klasör/dosya; Sistem-durumu/tam<br/><br/> **Azure VM koruma**: Birim/paylaşım/klasör/dosya<br/><br/> Yinelenenleri kaldırılmış birimler desteklenir.
**Itanium tabanlı sistemler için Windows Server 2008 SP2**<br/><br/> 64/32 bit | MABS v1, v2, v3 | MABS şirket içi fiziksel makineyi veya Hyper-V VM olarak dağıtıldığında yalnızca v1 desteklenmektedir.<br/><br/> MABS v1, 2, 3, MABS bir VMware sanal makinesi dağıtıldığında desteklenir.<br/><br/> Azure VM'de çalışan MABS için desteklenmiyor | Birim/paylaşım/klasör/dosya; Sistem-durumu/tam.
**Windows Storage Server 2008** | MABS v1, v2, v3 | Şirket içinde fiziksel sunucusu/Hyper-V VM olarak MABS | Azure VM'de çalışan MABS için desteklenmiyor. | Birim/paylaşım/klasör/dosya; Sistem-durumu/tam.
**SQL Server 2017** | MABS v3 | Şirket içi/Azure VM.| SQL Server veritabanını yedekleyin.<br/><br/> Desteklenen SQL Server kümesi yedekleme.<br/><br/>Veritabanları, desteklenmeyen Csv'lere depolanır.
**SQL Server 2016/2016 SP1** | MABS v3, v2 | Şirket içi/Azure VM.| SQL Server veritabanını yedekleyin.<br/><br/> Desteklenen SQL Server kümesi yedekleme.<br/><br/>Veritabanları, desteklenmeyen Csv'lere depolanır.
**SQL Server 2014**<br/><br/> **SQL Server 2012/SP1/SP2**<br/><br/> **SQL Server 2008 R2**<br/><br/> **SQL Server 2008** | MABS v3, v2, v1 | Şirket içi/Azure VM.| SQL Server veritabanını yedekleyin.<br/><br/> Desteklenen SQL Server kümesi yedekleme.<br/><br/>Veritabanları, desteklenmeyen Csv'lere depolanır.
**Exchange 2016**<br/><br/> **Exchange 2013**<br/><br/> **Exchange 2010** | MABS v3, v2, V1 | Şirket içi. | Tek başına Exchange sunucusu, DAG altındaki bir veritabanı yedekleme<br/><br/> Posta kutusu, DAG altındaki posta kutusu veritabanını kurtarın.<br/><br/> ReFS, desteklenmiyor.<br/><br/> Paylaşılmayan disk kümelerini yedekleyin.<br/><br/> Sürekli çoğaltma için yapılandırılmış Exchange Server yedekleyin.
**SharePoint 2016**<br/><br/> **SharePoint 2013**<br/><br/> **SharePoint 2010** | MABS v3, v2, v1 | Şirket içi/Azure VM | Grup, ön uç web sunucusu yedekleyin.<br/><br/> Grup, veritabanı, web uygulaması, dosya veya liste öğesi, SharePoint arama, ön uç web sunucusu kurtarma.<br/><br/> İçerik veritabanları için AlwaysOn kullanan bir grubu yedekleyemezsiniz.
**Windows Server 2016 Hyper-V**<br/><br/> **Windows Server 2012 R2/2012** (veri merkezi/standart)<br/><br/> **Windows Server 2008 R2 (SP1 ile)** | MABS v3, v2, v1 | Şirket içi | **Hyper-V konağında MABS aracı**: Tüm Vm'leri ve ana bilgisayar veri dosyaları yedekleme. Yerel depolama, SMB dosya sunucusu depolama ile sanal makineleri CSV depolama ile küme içindeki Vm'leri ile Vm'leri yedekleyin.<br/><br/> **VM Konuk aracısında MABS**: Sanal makinede çalışan iş yüklerini yedekleyin. Csv'ler.<br/><br/> **Kurtarma**: VM, VHD/birim/klasör/dosya öğe düzeyinde kurtarma.<br/><br/> **Linux Vm'leri**: Hyper-V Windows Server 2012 R2'de ve sonraki sürümlerde çalışırken yedekleyin. Linux VM'ler için kurtarma için tüm makinedir.
**VMware Vm'lerini: vCenter/vSphere ESXi 5.5/6.0/6.5** | MABS v3, v2, v1.<br/><br/> MABS v1 Güncelleştirme Paketi 1) | Şirket içi | VMware sanal makinelerini csv, NFS ve SAN depolama üzerinde yedekleyin.<br/><br/> Tüm VM kurtarma.<br/><br/> Windows/Linux yedekleme.<br/><br/> Klasör/dosya Windows Vm'leri için yalnızca öğe düzeyinde kurtarma.<br/><br/> VMware Vapps'i desteklenmez.<br/><br/> Linux VM'ler için kurtarma için tüm makinedir.



## <a name="supported-backups-to-dpm"></a>DPM için desteklenen yedekleri

Aşağıdaki tabloda, hangi DPM için şirket içi makinelerin ve Azure Vm'leri yedeklenebilir özetlenmektedir.



**Backup** | **DPM** | **Ayrıntılar**
--- | --- | --- 
**Windows 10, 8.1, 8, 7**<br/><br/>(64/32-bit) | Yalnızca şirket içi.<br/><br/> DPM 2012 R2 ile Windows 10 yedeklemek için yüklemenizi öneririz [güncelleştirme 11](https://support.microsoft.com/help/3209592/update-rollup-12-for-system-center-2012-r2-data-protection-manager). | Birim/paylaşım/klasör/dosya<br/><br/> Desteklenen yinelenenleri kaldırılmış birimler<br/><br/> Birimler en az 1 GB ve NTFS olmalıdır.
**Windows Server 2016 (Datacenter, Standard, nano hariç)**<br/><br/> 64/32 bit | Şirket içi/Azure VM.<br/><br/> Yalnızca DPM 2016.| Birim/paylaşım/klasör/dosya; Sistem-durumu/tam<br/><br/> Yinelenenleri kaldırılmış birimler desteklenir.
**Windows Server 2012 R2 (Datacenter ve Standard)**<br/><br/> 64/32 bit | Şirket içi/Azure VM | **Şirket içi koruma**: Birim/paylaşım/klasör/dosya; Sistem-durumu/tam<br/><br/> **Azure VM koruma**: Birim/paylaşım/klasör/dosya<br/><br/> Yinelenenleri kaldırılmış birimler içeren DPM 2012 R2 ve sonraki sürümlerde desteklenir.
**Itanium tabanlı sistemler için Windows Server 2012 SP1 (Datacenter ve Standard)**<br/><br/> 64/32 bit | Şirket içi/Azure VM. | **Şirket içi koruma**: Birim/paylaşım/klasör/dosya; Sistem-durumu/tam<br/><br/> **Azure VM koruma**: Birim/paylaşım/klasör/dosya<br/><br/> Yinelenenleri kaldırılmış birimler içeren DPM 2012 R2 ve sonraki sürümlerde desteklenir.
**Windows Server 2012 (Datacenter ve Standard)**<br/><br/> 64/32 bit |  Şirket içi/Azure VM. | **Şirket içi koruma**: Birim/paylaşım/klasör/dosya; Sistem-durumu/tam<br/><br/> **Azure VM koruma**: Birim/paylaşım/klasör/dosya<br/><br/> Yinelenenleri kaldırılmış birimler içeren DPM 2012 R2 ve sonraki sürümlerde desteklenir.
**Windows 2008 R2 SP1 (Standard ve Enterprise)**<br/><br/> 64/32 bit | Şirket içi/Azure VM <br/><br/> [Windows Management Framework 4.0](https://www.microsoft.com/download/details.aspx?id=40855) yüklü olması gerekir. |   **Şirket içi koruma**: Birim/paylaşım/klasör/dosya; Sistem-durumu/tam<br/><br/> **Azure VM koruma**: Birim/paylaşım/klasör/dosya.
**Windows 2008 R2 (Standard ve Enterprise)**<br/><br/> 64/32 bit | Şirket içi.<br/><br/> VMware VM olarak DPM yüklenemez.<br/><br/> DPM çalıştıran bir Azure sanal makinesinde desteklenmez. | **Şirket içi koruma**: Birim/paylaşım/klasör/dosya; Sistem-durumu/tam
**Itanium tabanlı sistemler için Windows Server 2008 SP2**<br/><br/> 64/32 bit | Yalnızca şirket içi.<br/><br/> DPM, bir VMware sanal makinesi çalışırken desteklenir. Fiziksel sunucu veya Hyper-V VM olarak çalışan desteklenmez. | Birim/paylaşım/klasör/dosya; Sistem-durumu/tam.
**Windows Storage Server 2008** | DPM fiziksel sunucu veya Hyper-V VM olarak çalışan şirket içi. | Birim/paylaşım/klasör/dosya; Sistem-durumu/tam.
**SQL Server 2017** | DPM SAC; DPM 2016 Güncelleştirme Paketi 5 veya üzeri çalıştırması<br/><br/> Şirket içi/Azure VM.| SQL Server veritabanını yedekleyin.<br/><br/> Desteklenen SQL Server kümesi yedekleme.<br/><br/>Veritabanları, desteklenmeyen Csv'lere depolanır.
**SQL Server 2016 SP1** | DPM 2012 R2 için desteklenmiyor; DPM 2016 güncelleştirme paketi 4 veya sonraki sürümünü çalıştıran DPM SAC için desteklenmiyor.<br/><br/> Şirket içi/Azure VM.| SQL Server veritabanını yedekleyin.<br/><br/> Desteklenen SQL Server kümesi yedekleme.<br/><br/>Veritabanları, desteklenmeyen Csv'lere depolanır.
**SQL Server 2016** | DPM 2012 R2 için desteklenmiyor. SAC DPM, DPM 2016'dan başlayarak, güncelleştirme paketi 2'için desteklenmiyor.<br/><br/> Şirket içi/Azure VM.| SQL Server veritabanını yedekleyin.<br/><br/> Desteklenen SQL Server kümesi yedekleme.<br/><br/>Veritabanları, desteklenmeyen Csv'lere depolanır.
**SQL Server 2014**<br/><br/> **SQL Server 2012/SP1/SP2**<br/><br/> **SQL Server 2008 R2**<br/><br/> **SQL Server 2008** | SQL Server 2014 güncelleştirme paketi 4 veya sonraki sürümleri çalıştıran DPM 2012 R2 ile.<br/><br/> Şirket içi/Azure VM.| SQL Server veritabanını yedekleyin.<br/><br/> Desteklenen SQL Server kümesi yedekleme.<br/><br/>Veritabanları, desteklenmeyen Csv'lere depolanır.
**Exchange 2016**<br/><br/> **Exchange 2013**<br/><br/> **Exchange 2010** | Exchange 2016 için DPM 2012 R2 güncelleştirme paketi 9 veya üzeri gerekir.<br/><br/> Şirket içi | Tek başına Exchange sunucusu, DAG altındaki bir veritabanı yedekleme<br/><br/> Posta kutusu, DAG altındaki posta kutusu veritabanını kurtarın.<br/><br/> ReFS, desteklenmiyor.<br/><br/> Paylaşılmayan disk kümelerini yedekleyin.<br/><br/> Sürekli çoğaltma için yapılandırılmış Exchange Server yedekleyin.
**SharePoint 2016**<br/><br/> **SharePoint 2013**<br/><br/> **SharePoint 2010** | SharePoint 2016'da DPM 2016 ve sonraki sürümler.<br/><br/>Şirket içi/Azure VM | Grup, ön uç web sunucusu yedekleyin.<br/><br/> Grup, veritabanı, web uygulaması, dosya veya liste öğesi, SharePoint arama, ön uç web sunucusu kurtarma.<br/><br/> İçerik veritabanları için AlwaysOn kullanan bir grubu yedekleyemezsiniz.
**Windows Server 2016 Hyper-V**<br/><br/> **Windows Server 2012 R2/2012** (veri merkezi/standart)<br/><br/> **Windows Server 2008 R2 (SP1 ile)** | Hyper-V'de, 2016 için DPM 2016 ve sonraki sürümlerde desteklenir.<br/><br/> Şirket içi | **Hyper-V konağında MABS aracı**: Tüm Vm'leri ve ana bilgisayar veri dosyaları yedekleme. Yerel depolama, SMB dosya sunucusu depolama ile sanal makineleri CSV depolama ile küme içindeki Vm'leri ile Vm'leri yedekleyin.<br/><br/> **VM Konuk aracısında MABS**: Sanal makinede çalışan iş yüklerini yedekleyin. Csv'ler.<br/><br/> **Kurtarma**: VM, VHD/birim/klasör/dosya öğe düzeyinde kurtarma.<br/><br/> **Linux Vm'leri**: Hyper-V Windows Server 2012 R2'de ve sonraki sürümlerde çalışırken yedekleyin. Linux VM'ler için kurtarma için tüm makinedir.
**VMware Vm'lerini: vCenter/vSphere ESXi 5.5/6.0/6.5** | MABS v3, v2, v1.<br/><br/> DPM 2012 R2, System Center Güncelleştirme Paketi 1 gerektirir) | Şirket içi | VMware sanal makinelerini csv, NFS ve SAN depolama üzerinde yedekleyin.<br/><br/> Tüm VM kurtarma.<br/><br/> Windows/Linux yedekleme.<br/><br/> Klasör/dosya Windows Vm'leri için yalnızca öğe düzeyinde kurtarma.<br/><br/> VMware Vapps'i desteklenmez.<br/><br/> Linux VM'ler için kurtarma için tüm makinedir.


- DPM/MABS tarafından yedeklenen kümelenmiş iş yükleri DPM/MABS aynı etki alanında veya alt ve güvenilen bir etki alanında gerektiğini unutmayın. 
- Güvenilmeyen etki alanlarındaki veya çalışma gruplarındaki verileri yedeklemek için NTLM/sertifika kimlik doğrulaması'nı kullanabilirsiniz.



## <a name="next-steps"></a>Sonraki adımlar

- [Daha fazla bilgi edinin](backup-architecture.md#architecture-back-up-to-dpmmabs) MABS mimarisi hakkında.
- [Gözden geçirme](backup-support-matrix-mars-agent.md) ne MARS aracısı için desteklenir.
- [Ayarlanan](backup-azure-microsoft-azure-backup.md) MABS sunucusu.
- [DPM ayarlama](https://docs.microsoft.com/system-center/dpm/install-dpm?view=sc-dpm-180)
