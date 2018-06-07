---
title: Azure yığın dosyaları ve uygulamaları yedekleme
description: Azure Backup, yedekleme ve Azure yığın dosyaları ve uygulamaları Azure yığın ortamınıza kurtarmak için kullanın.
services: backup
author: adiganmsft
manager: shivamg
ms.service: backup
ms.topic: conceptual
ms.date: 5/18/2018
ms.author: adigan
ms.openlocfilehash: 6c7fcc0182add05b68a7b41ab6fe50e18427f6ea
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34607312"
---
# <a name="back-up-files-and-applications-on-azure-stack"></a>Dosyalar ve uygulamalar Azure yığında yedekleme
Koruma (veya yedeklemek için) Azure Yedekleme'yi kullanabilirsiniz dosyalar ve uygulamalar Azure yığında. Dosya ve uygulamaların yedeklemek için Azure yığın üzerinde çalışan bir sanal makine olarak Microsoft Azure yedekleme sunucusu yükleyin. Aynı sanal ağda herhangi bir Azure yığın sunucuda çalışan uygulamaların, koruyabilirsiniz. Azure yedekleme sunucusu kez yüklediğiniz, kısa vadeli yedekleme verileri için kullanılabilir yerel depolama artırmak için Azure disk ekleyin. Azure yedekleme sunucusu uzun vadeli bekletme için Azure storage kullanır.

> [!NOTE]
> Azure yedekleme sunucusu ve System Center Data Protection Manager (DPM) benzer olmakla birlikte, DPM Azure yığını ile kullanım için desteklenmez.
>


## <a name="azure-backup-server-protection-matrix"></a>Azure Backup Sunucusu koruma matrisi
Azure yedekleme sunucusu aşağıdaki Azure yığın sanal makine iş yüklerini korur.

| Korunan veri kaynağı | Koruma ve kurtarma |
| --------------------- | ----------------------- |
| Windows Server yarı yıllık kanalı - kuruluş/veri merkezi/standart | Birimler, dosyalar, klasörler |
| Windows Server 2016 - kuruluş/veri merkezi/standart | Birimler, dosyalar, klasörler |
| Windows Server 2012 R2 - kuruluş/veri merkezi/standart | Birimler, dosyalar, klasörler |
| Windows Server 2012 - Entprise/veri merkezi/standart | Birimler, dosyalar, klasörler |
| Windows Server 2008 R2 - kuruluş/veri merkezi/standart | Birimler, dosyalar, klasörler |
| SQL Server 2016 | Database |
| SQL Server 2014 | Database |
| SQL Server 2012 SP1 | Database |
| SharePoint 2013 | Grup, veritabanı, ön uç, web sunucusu |
| SharePoint 2010 | Grup, veritabanı, ön uç, web sunucusu |


## <a name="install-azure-backup-server"></a>Azure yedekleme Sunucusu'nu Yükle
Bir Azure yığın sanal makineye Azure yedekleme sunucusu yüklemek için bkz [Azure yedekleme sunucusu kullanarak iş yüklerini Yedeklemeye hazırlanma](backup-mabs-install-azure-stack.md). Yükleyip Azure yedekleme sunucusu yapılandırmadan önce aşağıdakilere dikkat edin:

### <a name="determining-size-of-virtual-machine"></a>Sanal makine boyutunu belirleme
Azure yedekleme sunucusu bir Azure yığın sanal makine üzerinde çalıştırılacak boyutunu A2 kullanın veya daha büyük. Bir sanal makine boyutu seçerken Yardım almak için indirin [Azure yığın VM boyutu hesaplayıcı](https://www.microsoft.com/download/details.aspx?id=56832).

### <a name="virtual-networks-on-azure-stack-virtual-machines"></a>Azure yığın sanal makinelerde sanal ağlar
Bir Azure yığın iş yükü kullanılan tüm sanal makineler aynı Azure sanal ağı ve Azure aboneliğine ait olmalıdır.

### <a name="storing-backup-data-on-local-disk-and-in-azure"></a>Yerel disk ve Azure yedekleme veri depolama
Azure Backup sunucusu işletimsel kurtarma için sanal makineye bağlı Azure disklerde yedek verileri depolar. Diskler ve depolama alanı sanal makineye bağlı sonra Azure yedekleme sunucusu depolama yönetir. Yedek veri depolama alanı miktarı sayısını ve her birine bağlı disklerin boyutunu bağlıdır [Azure yığın sanal makine](../azure-stack/user/azure-stack-storage-overview.md). Her Azure yığın VM boyutu en fazla sayıda sanal makineye bağlı diskler vardır. Örneğin, A2 dört diskler ' dir. A3 sekiz diskler ' dir. A4 16 disk ' dir. Yeniden disk sayısı ve boyutu, toplam yedekleme depolama havuzunu belirler.

> [!IMPORTANT]
> Yapmanız gerekenler **değil** beş günden fazla bir süre için Azure yedekleme sunucusuna bağlı disklerde işletimsel Kurtarma (Yedekleme) verileri korur.
>

Yedek veri depolama, yedekleme altyapısı Azure yığında azaltır. Veri beş günden daha eski ise, Azure'da depolanması gerekir.

Azure'da yedek verileri depolamak için oluşturun veya bir kurtarma Hizmetleri kasası kullanın. Azure yedekleme sunucusu iş yükü çalışma yedeklemek hazırlarken, [kurtarma Hizmetleri kasası yapılandırma](backup-azure-microsoft-azure-backup.md#create-a-recovery-services-vault). Bir kere yapılandırıldığında, yedekleme işi her çalıştığında, kasaya bir kurtarma noktası oluşturulur. Her kurtarma Hizmetleri kasası kadar 9999 kurtarma noktalarını tutar. Oluşturulan kurtarma noktaları ve ne kadar süreyle saklanacağını sayısına bağlı olarak, birçok yıldır yedekleme verileri koruyabilirsiniz. Örneğin, aylık kurtarma noktaları oluşturmak ve bunları beş yıl boyunca bekletmek.
 
### <a name="using-sql-server"></a>SQL Server kullanma
Azure yedekleme sunucusu veritabanı için uzak bir SQL Server kullanmak isterseniz, yalnızca bir Azure yığın SQL Server çalıştıran VM seçin.

### <a name="azure-backup-server-vm-performance"></a>Azure yedekleme sunucusu VM performans
Diğer sanal makinelerle paylaşılan, depolama hesabı boyut ve IOPS sınırları Azure yedekleme sunucusu sanal makine performansını etkileyebilir. Bu nedenle, Azure yedekleme sunucusu sanal makine için ayrı bir depolama hesabı kullanmanız gerekir. Azure yedekleme sunucusu üzerinde çalışan Azure Yedekleme aracısı için geçici depolama gerekir:
    - kendi kullanımı (bir önbellek konumu)
    - (yerel hazırlama alanı) buluttan geri yüklenen veriler
  
### <a name="configuring-azure-backup-temporary-disk-storage"></a>Azure Backup geçici disk depolamayı yapılandırma
Her Azure yığın sanal makine toplu olarak kullanıcı için kullanılabilir geçici disk depolama ile birlikte gelen `D:\`. Azure Yedekleme'nin ihtiyaç duyduğu yerel hazırlama alanı bulunması için yapılandırılabilir `D:\`, ve önbellek konumu üzerinde yerleştirilebilir `C:\`. Bu şekilde, depolama Azure yedekleme sunucusu sanal makinesine bağlı veri disklerinden çıktığınızda yontulmuş gerekiyor.

### <a name="scaling-deployment"></a>Dağıtım ölçeklendirme
Dağıtımınız ölçeklendirmek istiyorsanız aşağıdaki seçenekleriniz vardır:
  - Ölçeği artırma - D serisinin serisinden Azure yedekleme sunucusu sanal makineye boyutunu artırır ve yerel depolama alanını büyütmek [Azure yığın sanal makine yönergeleri başına](../azure-stack/user/azure-stack-manage-vm-disks.md).
  - Veri Boşalt - eski verileri Azure yedekleme sunucusuna göndermek ve yalnızca en yeni verileri Azure yedekleme sunucusuna bağlı depolama korur.
  - -Daha fazla Azure yedekleme iş yüklerini korumak için sunucu eklemek ölçeğini.

## <a name="see-also"></a>Ayrıca bkz.
Diğer iş yüklerini korumak için Azure Backup'ı kullanma hakkında daha fazla bilgi için aşağıdaki makalelere birine bakın:
- [SharePoint grubunun kurulumu yedekleyin](backup-azure-backup-sharepoint-mabs.md)
- [SQL server'ı Yedekle](backup-azure-sql-mabs.md)
