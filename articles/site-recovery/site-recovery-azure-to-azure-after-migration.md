---
title: "Azure Site Recovery ile azure'a geçişten sonra Azure VM'ler için olağanüstü durum kurtarma ayarlama | Microsoft Docs"
description: "Bu makalede, Azure Site Kurtarma'yı kullanarak Azure geçişten sonra Azure bölgeler arasında olağanüstü durum kurtarma ayarlamak için makineleri hazırlamanın açıklar."
services: site-recovery
author: ponatara
ms.service: site-recovery
ms.topic: article
ms.date: 01/07/2018
ms.author: ponatara
ms.openlocfilehash: c06af21cd6e273b98c004e8bd0e6eac61ba7d644
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2018
---
# <a name="set-up-disaster-recovery-for-azure-vms-after-migration-to-azure"></a>Azure geçişten sonra Azure VM'ler için olağanüstü durum kurtarma ayarlayın 

>[!NOTE]
> Azure Site Kurtarma'yı kullanarak Azure VM'ler için olağanüstü durum kurtarma şu anda önizlemede değil.

Sonra bu makaleyi kullanın [Azure VM'ler için şirket içi makineler geçirilen](tutorial-migrate-on-premises-to-azure.md) kullanarak [Site Recovery](site-recovery-overview.md) hizmet. Bu makalede, Azure sanal makinelerini Site Recovery kullanarak bir ikincil bir Azure bölgesine olağanüstü durum kurtarma ayarlamak için hazırlamanıza yardımcı olur.



## <a name="before-you-start"></a>Başlamadan önce

Olağanüstü durum kurtarma ayarlamadan önce geçiş beklendiği gibi tamamlandığından emin olun. Bir geçiş yük devretme sonrasında başarıyla tamamlanması için seçmelisiniz **tam geçiş** seçeneği, geçirmek istediğiniz her makine için. 



## <a name="install-the-azure-vm-agent"></a>Azure VM Aracısı yükleme

Azure [VM Aracısı](../virtual-machines/windows/agent-user-guide.md) Site Recovery çoğaltmak için VM üzerinde yüklü olmalıdır.


1. Windows çalıştıran Vm'lerde VM aracısı yüklemek için indirme ve çalıştırma [aracı yükleyici](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Yüklemeyi tamamlamak için VM üzerinde yönetici ayrıcalıkları gerekir.
2. Linux çalıştıran sanal makinelerin VM Aracısı'nı yüklemek için en son yükleme [Linux Aracısı](../virtual-machines/linux/agent-user-guide.md). Yüklemeyi tamamlamak için yönetici ayrıcalıkları gerekir. Dağıtım depodan yüklemeniz önerilir. Linux VM Aracısı doğrudan Github'dan yüklemenizi öneririz yok. 


## <a name="validate-the-installation-on-windows-vms"></a>Windows sanal makineleri üzerinde yüklemeyi doğrulama

1. Azure VM'de C:\WindowsAzure\Packages klasöründeki WaAppAgent.exe dosyasını görmeniz gerekir.
2. Dosyaya sağ tıklayın ve **özellikleri**seçin **ayrıntıları** sekmesi.
3. Doğrulayın **ürün sürümü** alan gösterir 2.6.1198.718 veya üzeri.



## <a name="migration-from-vmware-vms-or-physical-servers"></a>VMware Vm'lerini veya fiziksel sunucuları geçiş

Azure için şirket içi VMware sanal makinelerini (veya fiziksel sunucuları) geçirirseniz, dikkat edin:

- Yalnızca geçirilen makinede Mobility hizmeti v9.6 değilse Azure VM Aracısı'nı yüklemeniz gerekir veya önceki sürümleri.
- Windows Mobility hizmeti 9.7.0.0 sürümü veya sonraki sürümleri çalıştıran VM'ler üzerinde hizmet yükleyici en son kullanılabilir Azure VM Aracısı'nı yükler. Geçirdiğinizde, bu sanal makineleri Site Recovery uzantısına dahil tüm VM uzantısı için önkoşul aracı yüklemesi zaten karşılar.
- Mobility hizmeti Azure aşağıdaki yöntemlerden birini kullanarak sanal makineden el ile kaldırmanız gerekir. Çoğaltma yapılandırmadan önce VM'yi yeniden başlatın.
    - Windows, Denetim Masası'nda > **Program Ekle/Kaldır**, kaldırma **Microsoft Azure Site Recovery Mobility hizmeti/ana hedef sunucu**. Yükseltilmiş bir komut isteminde çalıştırın:
        ```
        MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} /L+*V "C:\ProgramData\ASRSetupLogs\UnifiedAgentMSIUninstall.log"
        ```
    - Linux için oturum bir kök kullanıcı. Bir terminale Git **/user/local/ASR**, ve aşağıdaki komutu çalıştırın:
        ```
        uninstall.sh -Y
        ```


## <a name="next-steps"></a>Sonraki adımlar

[Hızlı bir şekilde çoğaltmak](azure-to-azure-quickstart.md) bir ikincil bölge için bir Azure VM.
