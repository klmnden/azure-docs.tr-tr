---
title: Azure Site Recovery ile azure'a geçiş sonrasında Azure Vm'leri için olağanüstü durum kurtarma ayarlama | Microsoft Docs
description: Bu makalede Azure Site Recovery ile azure'a geçiş sonrasında Azure bölgeleri arasında olağanüstü durum kurtarma ayarlama için makineleri hazırlamayı öğrenin.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 05/30/2019
ms.author: raynew
ms.openlocfilehash: 4b764c8e7c3d97ff521add05033265f705c4136f
ms.sourcegitcommit: d89032fee8571a683d6584ea87997519f6b5abeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66399530"
---
# <a name="set-up-disaster-recovery-for-azure-vms-after-migration-to-azure"></a>Azure'a geçişten sonra Azure VM'leri için olağanüstü durumdan kurtarmayı ayarlama 


Belirttiyseniz bu makaleyi takip [şirket içi makinelerin Azure Vm'lerine taşıdınız](tutorial-migrate-on-premises-to-azure.md) kullanarak [Site Recovery](site-recovery-overview.md) hizmeti ve artık istediğiniz ikincil bir Azure bölgesine olağanüstü durum kurtarma için ayarlanmış Vm'leri almak. Makalede nasıl geçirilen Vm'lerinde Azure VM aracısı yüklü olduğundan emin olun ve geçişten sonra artık gerekli olan Site Recovery Mobility hizmetinin nasıl kaldırılacağı açıklanmaktadır.



## <a name="verify-migration"></a>Geçişi doğrulama

Olağanüstü durum kurtarma işlemini ayarladığınız önce geçiş beklendiği gibi tamamlanmış olduğundan emin olun. Bir geçiş yük devretme işleminden sonra başarıyla tamamlanması için seçmelisiniz **geçişi Tamamla** geçirmek istediğiniz her makine için bir seçenek. 

## <a name="verify-the-azure-vm-agent"></a>Azure VM Aracısı'nı doğrulayın

Her Azure VM olmalıdır [Azure VM Aracısı](../virtual-machines/extensions/agent-windows.md) yüklü. Azure Vm'lerini çoğaltmak için Site Recovery Aracısı'nı bir uzantı yükler.

- Makine sürüm 9.7.0.0 çalıştırıyorsa veya daha sonra Site Recovery Mobility hizmeti, Azure VM Aracısı Windows vm'lerinde Mobility hizmeti tarafından otomatik olarak yüklenir. Mobility hizmetinin daha önceki sürümlerinde, aracıyı otomatik olarak yüklemeniz gerekir.
- Linux VM'ler için Azure VM aracısını el ile yüklemeniz gerekir. Yalnızca geçirilen makineye yüklü mobilite hizmeti v9.6 ise Azure VM aracısını yüklemeniz gerekir ya da daha önce.


### <a name="install-the-agent-on-windows-vms"></a>Windows sanal makinelerinde aracıyı yükleme

Site Recovery mobility hizmeti 9.7.0.0'den önceki bir sürümünü çalıştırıyorsanız veya aracıyı el ile yüklemek için diğer bazı gereksinimi varsa, aşağıdakileri yapın:  

1. VM üzerinde yönetici izinlerine sahip olun.
2. İndirme [VM Aracısı yükleyicisi](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409).
3. Yükleyici dosyasını çalıştırın.

#### <a name="validate-the-installation"></a>Yüklemeyi doğrulama
Aracının yüklü olduğunu denetlemek için:

1. Azure VM'de C:\WindowsAzure\Packages klasöründeki WaAppAgent.exe dosyasını görmeniz gerekir.
2. Dosyaya sağ tıklayın ve **özellikleri**seçin **ayrıntıları** sekmesi.
3. Doğrulayın **ürün sürümü** alan gösterir 2.6.1198.718 veya üzeri.

[Daha fazla bilgi edinin](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-windows) Windows için aracı yüklemesi hakkında.

### <a name="install-the-agent-on-linux-vms"></a>Linux Vm'lerde aracı yükleme

Yükleme [Azure Linux VM](../virtual-machines/extensions/agent-linux.md) aracısını aşağıdaki gibi el ile:

1. Makine üzerinde yönetici izinlerine sahip olduğunuzdan emin olun.
2. Dağıtımınıza ait bir paket deposundaki bir RPM veya DEB paketini kullanarak Linux VM aracısını yüklemeniz önerilir. Tüm [dağıtım sağlayıcıları onaylı](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) Azure Linux Aracısı paketi, görüntüleri ve depoları tümleştirme.
    - Aracının yalnızca bir dağıtım deposu aracılığıyla güncelleştirmeniz önerilir.
    - Doğrudan Github'dan Linux VM aracısını yükleme ve güncelleştirirken önerilmemektedir.
    -  Dağıtımınız için en son aracıyı yüklemek yönergeler mevcut, ilgili kişi dağıtım desteği değilse. 

#### <a name="validate-the-installation"></a>Yüklemeyi doğrulama 

1. Bu komutu çalıştırın: **ps -e** Azure aracısını Linux VM üzerinde çalıştığından emin olmak için.
2. İşlem çalışıyor durumda değilse, aşağıdaki komutları kullanarak yeniden başlatın:
    - Ubuntu için: **hizmet walinuxagent başlangıcı**
    - Diğer dağıtımlar için: **hizmet waagent başlangıcı**


## <a name="uninstall-the-mobility-service"></a>Mobility hizmetini kaldırın

1. Mobility hizmetini el ile aşağıdaki yöntemlerden birini kullanarak Azure sanal makineden kaldırın. 
    - Denetim Masası'nda Windows için > **Program Ekle/Kaldır**, kaldırma **Microsoft Azure Site Recovery Mobility hizmeti/ana hedef sunucusu**. Yükseltilmiş bir komut isteminde çalıştırın:
        ```
        MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} /L+*V "C:\ProgramData\ASRSetupLogs\UnifiedAgentMSIUninstall.log"
        ```
    - Linux için kök kullanıcı olarak oturum açın. Bir terminal penceresinde Git **/user/local/ASR**, ve aşağıdaki komutu çalıştırın:
        ```
        ./uninstall.sh -Y
        ```
2. Çoğaltma yapılandırmadan önce VM'yi yeniden başlatın.

## <a name="next-steps"></a>Sonraki adımlar

[Gözden geçirme sorun giderme](site-recovery-extension-troubleshoot.md) Azure VM Aracısı Site Recovery uzantının.
[Hızlı bir şekilde çoğaltmak](azure-to-azure-quickstart.md) Azure VM'LERİNİ ikincil bir bölgeye.
