---
title: Mobility hizmetini (VMware veya fiziksel-Azure) yükleme | Microsoft Docs
description: Şirket içi VMware Vm'leri ve fiziksel sunucuları Azure Site Recovery ile koruma için Mobility hizmeti aracısı yüklemeyi öğrenin.
author: Rajeswari-Mamilla
ms.service: site-recovery
ms.topic: article
ms.date: 07/06/2018
ms.author: ramamill
ms.openlocfilehash: bc0ec09e28c5540eb919ac4e5f970f877ae27e44
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37919096"
---
# <a name="install-the-mobility-service"></a>Mobility hizmetini yükleme 

Azure Site Recovery Mobility hizmeti, VMware Vm'leri ve fiziksel sunucuları Azure'a çoğaltmak istediğiniz yüklenir. Hizmet, bir bilgisayardaki veri yazma yakalar ve ardından bunları işlem sunucusuna gönderir. Azure'a çoğaltmak istediğiniz her bilgisayarda (VMware VM veya fiziksel sunucu) için Mobility hizmetini dağıtma. Sunucuları üzerindeki Mobility hizmetinin ve aşağıdaki yöntemleri kullanarak korumak istediğiniz VMware Vm'leri dağıtabilirsiniz:


* [Yazılım dağıtım araçları gibi System Center Configuration Manager kullanarak yükleme](vmware-azure-mobility-install-configuration-mgr.md)
* [Azure Otomasyonu ve Desired State Configuration (Automation DSC) yükleme](vmware-azure-mobility-deploy-automation-dsc.md)
* [Kullanıcı Arabiriminden el ile yükleyin](vmware-azure-install-mobility-service.md#install-mobility-service-manually-by-using-the-gui)
* [Bir komut isteminden el ile yükleme](vmware-azure-install-mobility-service.md#install-mobility-service-manually-at-a-command-prompt)
* [Site Recovery anında yükleme kullanarak yükleme](vmware-azure-install-mobility-service.md#install-mobility-service-by-push-installation-from-azure-site-recovery)


>[!IMPORTANT]
> Mobility hizmetinin sürümü 9.7.0.0, Windows vm'lerinde başlayarak yükleyici de en son kullanılabilir yükler [Azure VM Aracısı](../virtual-machines/extensions/features-windows.md#azure-vm-agent). Bir bilgisayar üzerinden Azure'a başarısız olduğunda, bilgisayarın aracı yüklemesi için herhangi bir VM uzantısı kullanarak önkoşul karşılar.

## <a name="prerequisites"></a>Önkoşullar
Sunucunuz üzerinde Mobility hizmetini el ile yüklemeden önce önkoşul adımları tamamlayın:
1. Yapılandırma sunucunuzda oturum açın ve ardından yönetici olarak bir komut istemi penceresi açın.
2. Bin klasörüne erişmeye dizini değiştirin ve ardından bir parola dosyası oluşturun.

    ```
    cd %ProgramData%\ASR\home\svsystems\bin
    genpassphrase.exe -v > MobSvc.passphrase
    ```
3. Parola dosyası güvenli bir konuma Store. Mobility hizmeti yüklemesi sırasında dosya kullanırsınız.
4. Tüm desteklenen işletim sistemleri için Mobility hizmeti yükleyiciler %ProgramData%\ASR\home\svsystems\pushinstallsvc\repository klasöründedir.

### <a name="mobility-service-installer-to-operating-system-mapping"></a>Mobility hizmeti yükleyicisi-işletim sistem eşlemesi

| Yükleyici dosyası şablonu adı| İşletim sistemi |
|---|--|
|Microsoft ASR\_UA\*Windows\*release.exe | Windows Server 2008 R2 SP1 (64-bit) </br> Windows Server 2012 (64-bit) </br> Windows Server 2012 R2 (64-bit) </br> Windows Server 2016 (64-bit) |
|Microsoft ASR\_UA\*RHEL6-64\*release.tar.gz| Red Hat Enterprise Linux (RHEL) 6.4, 6.5, 6.6, 6.7, 6,8, 6.9 (yalnızca 64-bit) </br> CentOS 6.4, 6.5, 6.6, 6.7, 6,8, 6.9 (yalnızca 64-bit) |
|Microsoft ASR\_UA\*RHEL7 64\*release.tar.gz | Red Hat Enterprise Linux (RHEL) 7.1, 7.2, 7.3 (yalnızca 64-bit) </br> CentOS 7.0, 7.1, 7.2, 7.3 (yalnızca 64-bit) |
|Microsoft ASR\_UA\*SLES11 SP3 64\*release.tar.gz| SUSE Linux Enterprise Server 11 SP3 (yalnızca 64-bit)|
|Microsoft ASR\_UA\*SLES11 SP4 64\*release.tar.gz| SUSE Linux Enterprise Server 11 SP4 (yalnızca 64-bit)|
|Microsoft ASR\_UA\*OL6 64\*release.tar.gz | Oracle Enterprise Linux 6.4, 6.5 (yalnızca 64-bit)|
|Microsoft ASR\_UA\*UBUNTU 14.04 64\*release.tar.gz | Ubuntu Linux 14.04 (yalnızca 64-bit)|
|Microsoft ASR\_UA\*UBUNTU-16.04-64\*release.tar.gz | Ubuntu Linux 16.04 LTS server (yalnızca 64-bit)|
|Microsoft ASR_UA\*DEBIAN7 64\*release.tar.gz | Debian 7 (yalnızca 64 bit)|
|Microsoft ASR_UA\*DEBIAN8 64\*release.tar.gz | Debian 8 (yalnızca 64-bit)|


## <a name="install-mobility-service-manually-by-using-the-gui"></a>GUI kullanarak Mobility hizmetini el ile yükleme

>[!IMPORTANT]
> Bir Azure aboneliği/bölgesinden Azure Iaas sanal makineleri çoğaltmak için bir yapılandırma sunucusu kullanıyorsanız, komut satırı tabanlı yükleme yöntemi kullanın.

[!INCLUDE [site-recovery-install-mob-svc-gui](../../includes/site-recovery-install-mob-svc-gui.md)]

## <a name="install-mobility-service-manually-at-a-command-prompt"></a>Mobility hizmetini el ile bir komut isteminde yükleme

### <a name="command-line-installation-on-a-windows-computer"></a>Bir Windows bilgisayarda komut satırı yüklemesi
[!INCLUDE [site-recovery-install-mob-svc-win-cmd](../../includes/site-recovery-install-mob-svc-win-cmd.md)]

### <a name="command-line-installation-on-a-linux-computer"></a>Bir Linux bilgisayarda komut satırı yüklemesi
[!INCLUDE [site-recovery-install-mob-svc-lin-cmd](../../includes/site-recovery-install-mob-svc-lin-cmd.md)]


## <a name="install-mobility-service-by-push-installation-from-azure-site-recovery"></a>Azure Site recovery'den anında yükleme yöntemiyle Mobility hizmetini yükleme
Site Recovery kullanılarak Mobility hizmeti göndererek yükleme yapabilirsiniz. Tüm hedef bilgisayarlar aşağıdaki önkoşulları karşılamalıdır.

[!INCLUDE [site-recovery-prepare-push-install-mob-svc-win](../../includes/site-recovery-prepare-push-install-mob-svc-win.md)]

[!INCLUDE [site-recovery-prepare-push-install-mob-svc-lin](../../includes/site-recovery-prepare-push-install-mob-svc-lin.md)]


> [!NOTE]
Mobility hizmeti, Azure portalında yükledikten sonra seçin **+ Çoğalt** bu Vm'leri korumaya başlamak için.

## <a name="update-mobility-service"></a>Mobility hizmetini güncelleştirme

> [!WARNING]
> Korumalı sunucularda Mobility hizmeti güncelleştiriliyor başlamadan önce yapılandırma sunucusunu, ölçeği genişletilmiş işlem sunucularını ve dağıtımınızın bir parçası olan herhangi bir ana hedef sunucuları güncelleştirildiğinden emin olun.

1. Azure Portal'da Gözat *kasanızın ad* > **çoğaltılan öğeler** görünümü.
2. Yapılandırma sunucusu zaten en son sürüme güncelleştirildi "Yeni Site recovery çoğaltma aracısı güncelleştirmesi kullanılabilir. okuyan bir bildirim görürsünüz Yüklemek için tıklayın."

     ![Çoğaltılan öğeler penceresi](.\media\vmware-azure-install-mobility-service\replicated-item-notif.png)
3. Sanal makine seçimi sayfasını açmak için bildirimi seçin.
4. İstediğiniz üzerinde mobility hizmetini ve sanal makineleri seçin **Tamam**.

     ![Çoğaltılan öğeler VM listesi](.\media\vmware-azure-install-mobility-service\update-okpng.png)

Her bir seçili sanal makine için Mobility hizmetini güncelleştirme işlemi başlatır.

> [!NOTE]
> [Daha fazla bilgi edinin](vmware-azure-manage-configuration-server.md) nasıl Mobility hizmetini yüklemek için kullanılan hesap parolasını güncelleştirin.

## <a name="uninstall-mobility-service-on-a-windows-server-computer"></a>Windows Server yüklü bir bilgisayara mobilite hizmetini kaldırabilir
Windows Server yüklü bir bilgisayara Mobility hizmetini kaldırmak için aşağıdaki yöntemlerden birini kullanın.

### <a name="uninstall-by-using-the-gui"></a>GUI kullanarak kaldırma
1. Denetim Masası'ndaki seçin **programlar**.
2. Seçin **Microsoft Azure Site Recovery Mobility hizmeti/ana hedef sunucusu**ve ardından **kaldırma**.

### <a name="uninstall-at-a-command-prompt"></a>Bir komut isteminde kaldırma
1. Yönetici olarak bir komut istemi penceresi açın.
2. Mobility hizmetini kaldırmak için aşağıdaki komutu çalıştırın:

    ```
    MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} /L+*V "C:\ProgramData\ASRSetupLogs\UnifiedAgentMSIUninstall.log"
    ```

## <a name="uninstall-mobility-service-on-a-linux-computer"></a>Bir Linux bilgisayarı üzerinde Mobility hizmetini kaldırın
1. Olarak Linux sunucunuzda oturum bir **kök** kullanıcı.
2. Bir terminal penceresinde /user/local/ASR için gidin.
3. Mobility hizmetini kaldırmak için aşağıdaki komutu çalıştırın:

    ```
    uninstall.sh -Y
    ```
