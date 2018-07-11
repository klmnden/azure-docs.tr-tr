---
title: Azure Site Recovery ile azure'a geçiş sonrasında Azure Vm'leri için olağanüstü durum kurtarma ayarlama | Microsoft Docs
description: Bu makalede Azure Site Recovery ile azure'a geçiş sonrasında Azure bölgeleri arasında olağanüstü durum kurtarma ayarlama için makineleri hazırlamayı öğrenin.
services: site-recovery
author: ponatara
ms.service: site-recovery
ms.topic: article
ms.date: 07/06/2018
ms.author: ponatara
ms.openlocfilehash: 85ebb141390e0fa6b4dfbd77d7b7d3f6844950d7
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2018
ms.locfileid: "37916471"
---
# <a name="set-up-disaster-recovery-for-azure-vms-after-migration-to-azure"></a>Azure geçişten sonra Azure Vm'leri için olağanüstü durum kurtarmayı ayarlayın 


Sonra bu makaleyi kullanın [şirket içi makinelerin Azure Vm'lerine taşıdınız](tutorial-migrate-on-premises-to-azure.md) kullanarak [Site Recovery](site-recovery-overview.md) hizmeti. Bu makalede, Azure sanal makinelerini Site Recovery ile ikincil bir Azure bölgesine olağanüstü durum kurtarmayı ayarlama hazırlamanıza yardımcı olur.



## <a name="before-you-start"></a>Başlamadan önce

Olağanüstü durum kurtarma işlemini ayarladığınız önce geçiş beklendiği gibi tamamlanmış olduğundan emin olun. Bir geçiş yük devretme işleminden sonra başarıyla tamamlanması için seçmelisiniz **geçişi Tamamla** geçirmek istediğiniz her makine için bir seçenek. 



## <a name="install-the-azure-vm-agent"></a>Azure VM aracısını yükleyin

Azure [VM Aracısı](../virtual-machines/extensions/agent-windows.md) Site Recovery çoğaltma, sanal makinede yüklü olmalıdır.


1. Windows çalıştıran Vm'lerde VM aracısı yüklemek için indirme ve çalıştırma [Aracısı yükleyicisi](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Yüklemeyi tamamlamak için VM üzerinde yönetici ayrıcalıkları gerekir.
2. Linux çalıştıran Vm'leri üzerinde VM aracısı yüklemek için en son yükleme [Linux Aracısı](../virtual-machines/extensions/agent-linux.md). Yüklemeyi tamamlamak için yönetici ayrıcalıkları gerekir. Dağıtım deponuzdan yüklemeniz önerilir. Linux VM Aracısı, doğrudan Github'dan yükleme önerilmemektedir. 


## <a name="validate-the-installation-on-windows-vms"></a>Windows vm'lerinde yüklemeyi doğrulama

1. Azure VM'de C:\WindowsAzure\Packages klasöründeki WaAppAgent.exe dosyasını görmeniz gerekir.
2. Dosyaya sağ tıklayın ve **özellikleri**seçin **ayrıntıları** sekmesi.
3. Doğrulayın **ürün sürümü** alan gösterir 2.6.1198.718 veya üzeri.



## <a name="migration-from-vmware-vms-or-physical-servers"></a>VMware Vm'lerini veya fiziksel sunucuları geçiş

Şirket içi VMware Vm'leri (veya fiziksel sunucuları) Azure'a geçiş işlemi gerçekleştirirseniz, dikkat edin:

- Yalnızca geçirilen makineye yüklü mobilite hizmeti v9.6 ise Azure VM aracısını yüklemeniz gerekir ya da daha önce.
- Mobility hizmetinin sürümü 9.7.0.0 ve sonraki sürümlerde çalışan Windows Vm'lerinde hizmeti yükleyicisinin en son kullanılabilir Azure VM aracısı yükler. Geçiş yaptığınızda, bu Vm'lere Site Recovery uzantısı dahil olmak üzere tüm VM uzantısı için önkoşul aracı yüklemesi zaten karşılayın.
- Mobility hizmeti aşağıdaki yöntemlerden birini kullanarak Azure sanal makineden el ile kaldırmanız gerekir. Çoğaltma yapılandırmadan önce VM'yi yeniden başlatın.
    - Denetim Masası'nda Windows için > **Program Ekle/Kaldır**, kaldırma **Microsoft Azure Site Recovery Mobility hizmeti/ana hedef sunucusu**. Yükseltilmiş bir komut isteminde çalıştırın:
        ```
        MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} /L+*V "C:\ProgramData\ASRSetupLogs\UnifiedAgentMSIUninstall.log"
        ```
    - Linux için oturum bir kök kullanıcı. Bir terminal penceresinde Git **/user/local/ASR**, ve aşağıdaki komutu çalıştırın:
        ```
        uninstall.sh -Y
        ```


## <a name="next-steps"></a>Sonraki adımlar

[Hızlı bir şekilde çoğaltmak](azure-to-azure-quickstart.md) Azure VM'LERİNİ ikincil bir bölgeye.
