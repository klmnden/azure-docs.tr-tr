---
title: VMware Vm'leri ve fiziksel sunucuları Azure Site Recovery ile olağanüstü durum kurtarma için Mobility hizmeti hakkında | Microsoft Docs
description: VMware Vm'lerini ve fiziksel sunucuları Azure Site Recovery hizmetini kullanarak azure'a olağanüstü durum kurtarma için Mobility Hizmeti Aracısı hakkında bilgi edinin.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: ramamill
ms.openlocfilehash: e3967319cd41399209bd50886bce88efc8ba6ba6
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52956525"
---
# <a name="about-the-mobility-service-for-vmware-vms-and-physical-servers"></a>VMware Vm'lerini ve fiziksel sunucular için Mobility hizmeti hakkında

Ayarladığınızda olağanüstü durum kurtarma için VMware Vm'lerini ve fiziksel sunucuları kullanarak [Azure Site Recovery](site-recovery-overview.md), fiziksel sunucu ve Site Recovery Mobility hizmeti her şirket içi VMware VM'ye yükleyin.  Mobility hizmeti yakalar makinede veri yazar ve onları Site Recovery işlem sunucusuna gönderir. Aşağıdaki yöntemleri kullanarak Mobility hizmetini dağıtabilirsiniz:

[Gönderme yüklemesi](vmware-azure-install-mobility-service.md): Site Recovery Mobility hizmetinin göndererek yükleme gerçekleştirmek için yapılandırma: olağanüstü durum kurtarma işlemini ayarladığınız zaman bunu yapmak için de Site Recovery işlem sunucusu VM veya fiziksel sunucu erişmek için kullanabileceğiniz bir hesap ayarlama Hizmet yükleme amacıyla.
[El ile yükleme](vmware-physical-mobility-service-install-manual.md): Mobility hizmetini el ile kullanıcı Arabirimi veya komut istemi kullanarak her makineye yükleyebilirsiniz.
[Otomatik dağıtım](vmware-azure-mobility-install-configuration-mgr.md): yükleme yazılım dağıtım araçları gibi System Center Configuration Manager ile otomatik hale getirebilirsiniz.

## <a name="azure-virtual-machine-agent"></a>Azure sanal makine Aracısı

- **Windows Vm'leri**: Mobility hizmetini 9.7.0.0 sürümünden [Azure VM Aracısı](../virtual-machines/extensions/features-windows.md#azure-vm-agent) Mobility hizmeti yükleyicisinin tarafından yüklenir. Bu, üzerinden Azure'a makine başarısız olduğunda, Azure VM aracı yüklemesi için herhangi bir Vm uzantısı kullanarak önkoşul karşıladığını sağlar.
- **Linux Vm'leri**: [WALinuxAgent](https://docs.microsoft.com/azure/virtual-machines/extensions/update-linux-agent) yük devretme sonrasında Azure VM'de el ile yüklenmelidir.

## <a name="installer-files"></a>Yükleyici dosyaları

Tablo, her bir VMware VM ve fiziksel sunucu işletim sistemi için yükleyici dosyalarını özetler. Gözden geçirebilirsiniz [desteklenen işletim sistemleri](vmware-physical-azure-support-matrix.md#replicated-machines) başlamadan önce.


**Yükleyici dosyası** | **İşletim sistemi (yalnızca 64-bit)** 
--- | ---
Microsoft ASR\_UA\*Windows\*release.exe | Windows Server 2016; Windows Server 2012 R2'de; Windows Server 2012; Windows Server 2008 R2 SP1 
Microsoft ASR\_UA\*RHEL6 64\*release.tar.gz | Red Hat Enterprise Linux (RHEL) 6.* </br> CentOS 6.*
Microsoft ASR\_UA\*RHEL7 64\*release.tar.gz | Red Hat Enterprise Linux (RHEL) 7.* </br> CentOS 7.* 
Microsoft ASR\_UA\*SLES12 64\*release.tar.gz | SUSE Linux Enterprise Server 12 SP1, SP2 SP3 
Microsoft ASR\_UA\*SLES11 SP3 64\*release.tar.gz| SUSE Linux Enterprise Server 11 SP3 
Microsoft ASR\_UA\*SLES11 SP4 64\*release.tar.gz| SUSE Linux Enterprise Server 11 SP4 
Microsoft ASR\_UA\*OL6 64\*release.tar.gz | Oracle Enterprise Linux 6.4, 6.5
Microsoft ASR\_UA\*UBUNTU 14.04 64\*release.tar.gz | Ubuntu Linux 14.04
Microsoft ASR\_UA\*UBUNTU-16.04-64\*release.tar.gz | Ubuntu Linux 16.04 LTS server
Microsoft ASR_UA\*DEBIAN7 64\*release.tar.gz | Debian 7 
Microsoft ASR_UA\*DEBIAN8 64\*release.tar.gz | Debian 8

## <a name="anti-virus-on-replicated-machines"></a>Çoğaltılan makinelerde virüsten koruma

Çoğaltmak istediğiniz makineleri çalışan active virüsten koruma yazılımı varsa, virüsten koruma işlemlerini Mobility hizmeti yükleme klasör dışlama emin olun (*C:\ProgramData\ASR\agent*). Bu, çoğaltmanın beklendiği gibi çalıştığını sağlar.

## <a name="update-the-mobility-service"></a>Mobility hizmetini güncelleştirme

1. Başlamadan önce Mobility hizmetinin korunan makinelere güncelleştirmeden önce yapılandırma sunucusunu, ölçeği genişletilmiş işlem sunucularını ve dağıtımınızın bir parçası olan herhangi bir ana hedef sunucuları güncelleştirildiğinden emin olun.
2. Kasa portalda açın > **çoğaltılan öğeler**.
3. Yapılandırma sunucusunu en son sürüm ise "Yeni Site recovery çoğaltma aracısı güncelleştirmesi kullanılabilir. okuyan bir bildirim görür Yüklemek için tıklayın."

     ![Çoğaltılan öğeler penceresi](./media/vmware-azure-install-mobility-service/replicated-item-notif.png)

4. Bildirime tıklayın ve **Aracısı güncelleştirmesi**, Mobility hizmetini yükseltmek istediğiniz makineleri seçin. Daha sonra, **Tamam**'a tıklayın.

     ![Çoğaltılan öğeler VM listesi](./media/vmware-azure-install-mobility-service/update-okpng.png)

5. Her seçili makineler için Mobility hizmetini güncelleştirme işlemi başlatır.

## <a name="update-the-acount-used-for-push-installation-of-the-mobility-service"></a>Mobility hizmetinin gönderme yüklemesi için kullanılan hesap güncelleştir

Mobility hizmetinin göndererek yüklenmesine ilişkin etkinleştirmek için Site Recovery dağıttığınızda makinelerine erişebilir ve makine için çoğaltma etkinleştirildiğinde, hizmeti yüklemek için Site Recovery işlem sunucusu kullanan bir hesap belirtilene. Bu hesabın kimlik bilgilerini güncelleştirmek istiyorsanız, izleyin [bu yönergeleri](vmware-azure-manage-configuration-server.md).

## <a name="uninstall-the-mobility-service"></a>Mobility hizmetini kaldırın

### <a name="on-a-windows-machine"></a>Bir Windows makinede

Kullanıcı Arabirimi veya komut istemi'ni kaldırın.

- **Kullanıcı arabiriminden**: Denetim Masası'nda seçin makinenin **programlar**. Seçin **Microsoft Azure Site Recovery Mobility hizmeti/ana hedef sunucusu** > **kaldırma**.
- **Bir komut isteminden**: makinede bir yönetici olarak bir komut istemi penceresi açın. Şu komutu çalıştırın: 
    ```
    MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} /L+*V "C:\ProgramData\ASRSetupLogs\UnifiedAgentMSIUninstall.log"
    ```

## <a name="on-a-linux-machine"></a>Bir Linux makinesinde
1. Linux makinesinde oturum olarak bir **kök** kullanıcı.
2. Bir terminal penceresinde /user/local/ASR için gidin.
3. Şu komutu çalıştırın:

    ```
    uninstall.sh -Y
    ```

## <a name="next-steps"></a>Sonraki adımlar

[Mobility hizmetinin göndererek yüklenmesine ayarlama](vmware-azure-install-mobility-service.md).
