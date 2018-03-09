---
title: "Mobility hizmetinin (VMware veya fiziksel Azure) yükleme | Microsoft Docs"
description: "Şirket içi VMware Vm'lerini ve fiziksel sunucuları Azure Site Recovery ile korumak için Mobility hizmeti aracısı yüklemeyi öğrenin."
services: site-recovery
author: AnoopVasudavan
manager: gauravd
ms.service: site-recovery
ms.topic: article
ms.date: 03/05/2018
ms.author: anoopkv
ms.openlocfilehash: 445a5f10eac0959dab57e10680659c0792ad6fba
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="install-the-mobility-service"></a>Mobility hizmetini yükleme 

Azure Site Recovery Mobility hizmeti VMware Vm'lerini ve Azure'a çoğaltmak istediğiniz fiziksel sunucularda yüklenir. Hizmet, bir bilgisayardaki veri yazma yakalar ve bunları işlem sunucusuna gönderir. Azure'a çoğaltmak istediğiniz her bilgisayarda (VMware VM veya fiziksel sunucu) için Mobility hizmetini dağıtın. Sunucularda Mobility hizmeti ve aşağıdaki yöntemleri kullanarak korumak istediğiniz VMware Vm'leri dağıtabilirsiniz:


* [Yazılım dağıtım araçları gibi System Center Configuration Manager kullanarak yükleme](vmware-azure-mobility-install-configuration-mgr.md)
* [Azure Otomasyonu istenen durum yapılandırması (Automation DSC) ile yükleme](vmware-azure-mobility-deploy-automation-dsc.md)
* [Kullanıcı Arabiriminden el ile yükleyin](vmware-azure-install-mobility-service.md#install-mobility-service-manually-by-using-the-gui)
* [Bir komut isteminden el ile yükleyin](vmware-azure-install-mobility-service.md#install-mobility-service-manually-at-a-command-prompt)
* [Site Recovery anında yükleme kullanarak yükleme](vmware-azure-install-mobility-service.md#install-mobility-service-by-push-installation-from-azure-site-recovery)


>[!IMPORTANT]
> Mobility hizmeti sürümü 9.7.0.0, Windows vm'lerinde başlayarak yükleyici de en son kullanılabilir yükler [Azure VM Aracısı](../virtual-machines/windows/extensions-features.md#azure-vm-agent). Bir bilgisayar üzerinden Azure'a başarısız olduğunda, bilgisayarın tüm VM uzantısı kullanılarak için önkoşul aracı yüklemesi karşılar.

## <a name="prerequisites"></a>Önkoşullar
Sunucunuzda el ile Mobility hizmetini yüklemeden önce önkoşul adımları tamamlayın:
1. Yapılandırma sunucusunda oturum açın ve ardından yönetici olarak bir komut istemi penceresi açın.
2. Bin klasörüne dizini değiştirin ve ardından bir parola dosya oluşturun.

    ```
    cd %ProgramData%\ASR\home\svsystems\bin
    genpassphrase.exe -v > MobSvc.passphrase
    ```
3. Parola dosyasını güvenli bir konumda depolayın. Mobility hizmeti yüklemesi sırasında dosyasını kullanın.
4. Tüm desteklenen işletim sistemleri için Mobility hizmeti yükleyiciler %ProgramData%\ASR\home\svsystems\pushinstallsvc\repository klasöründe bulunur.

### <a name="mobility-service-installer-to-operating-system-mapping"></a>Mobility hizmeti işletim yükleyici sistem eşlemesi

| Yükleyici dosyası şablonu adı| İşletim sistemi |
|---|--|
|Microsoft-ASR\_UA\*Windows\*release.exe | Windows Server 2008 R2 SP1 (64 bit) </br> Windows Server 2012 (64 bit) </br> Windows Server 2012 R2 (64 bit) </br> Windows Server 2016 (64 bit) |
|Microsoft-ASR\_UA\*RHEL6-64*release.tar.gz| Red Hat Enterprise Linux (RHEL) 6.4, 6.5, 6.6, 6.7, 6.8, 6.9 (64-bit only) </br> CentOS 6.4 6.5, 6.6, 6.7, 6,8, 6.9 (yalnızca 64 bit) |
|Microsoft-ASR\_UA\*RHEL7-64\*release.tar.gz | Red Hat Enterprise Linux (RHEL) 7.1, 7.2, 7.3 (64-bit only) </br> CentOS 7.0, 7.1, 7.2, 7.3 (yalnızca 64 bit) |
|Microsoft-ASR\_UA\*SLES11-SP3-64\*release.tar.gz| SUSE Linux Enterprise Server 11 SP3 (64-bit only)|
|Microsoft ASR\_UA\*SLES11 SP4 64\*release.tar.gz| SUSE Linux Enterprise Server 11 SP4 (yalnızca 64 bit)|
|Microsoft-ASR\_UA\*OL6-64\*release.tar.gz | Oracle Enterprise Linux 6.4, 6.5 (yalnızca 64 bit)|
|Microsoft ASR\_UA\*UBUNTU 14.04 64\*release.tar.gz | Ubuntu Linux 14.04 (yalnızca 64 bit)|
|Microsoft ASR\_UA\*UBUNTU 16.04 64\*release.tar.gz | Ubuntu Linux 16.04 LTS sunucu (yalnızca 64 bit)|
|Microsoft-ASR_UA\*DEBIAN7-64\*release.tar.gz | Debian 7 (yalnızca 64 bit)|
|Microsoft-ASR_UA\*DEBIAN8-64\*release.tar.gz | Debian 8 (yalnızca 64 bit)|


## <a name="install-mobility-service-manually-by-using-the-gui"></a>GUI kullanarak Mobility hizmeti el ile yükleyin

>[!IMPORTANT]
> Azure Iaas sanal makineleri bir Azure aboneliği/bölgesinden çoğaltmak için bir yapılandırma sunucusu kullanıyorsanız, komut satırı tabanlı yükleme yöntemi kullanın.

[!INCLUDE [site-recovery-install-mob-svc-gui](../../includes/site-recovery-install-mob-svc-gui.md)]

## <a name="install-mobility-service-manually-at-a-command-prompt"></a>Mobility hizmeti el ile bir komut isteminden yükleme

### <a name="command-line-installation-on-a-windows-computer"></a>Bir Windows bilgisayarda komut satırı yüklemesi
[!INCLUDE [site-recovery-install-mob-svc-win-cmd](../../includes/site-recovery-install-mob-svc-win-cmd.md)]

### <a name="command-line-installation-on-a-linux-computer"></a>Linux bilgisayarda komut satırı yükleme
[!INCLUDE [site-recovery-install-mob-svc-lin-cmd](../../includes/site-recovery-install-mob-svc-lin-cmd.md)]


## <a name="install-mobility-service-by-push-installation-from-azure-site-recovery"></a>Azure Site Recovery anında yüklemesinden tarafından Mobility hizmetini yükleme
Mobility hizmetinin göndermeli yüklemesi Site RECOVERY'yi kullanarak yapabilirsiniz. Tüm hedef bilgisayarlar aşağıdaki önkoşulları yerine getirmeniz gerekir.

[!INCLUDE [site-recovery-prepare-push-install-mob-svc-win](../../includes/site-recovery-prepare-push-install-mob-svc-win.md)]

[!INCLUDE [site-recovery-prepare-push-install-mob-svc-lin](../../includes/site-recovery-prepare-push-install-mob-svc-lin.md)]


> [!NOTE]
Mobility hizmeti, Azure portalında yükledikten sonra seçin **+ Çoğalt** bu sanal makineleri korumaya başlamak için.

## <a name="update-mobility-service"></a>Mobility hizmeti güncelleştirmesi

> [!WARNING]
> Mobility hizmeti korumalı sunucularda güncelleştirme başlamadan önce yapılandırma sunucusu, genişleme işlem sunucuları ve dağıtımınızın bir parçası olan tüm ana hedef sunucularının güncel emin olun.

1. Azure Portal'da Gözat *kasanızı adını* > **öğeleri çoğaltılan** görünümü.
2. Yapılandırma sunucusunun en son sürümüne güncelleştirilmişse, "Yeni Site kurtarma çoğaltma aracısını güncelleştirme kullanılabilir. okuyan bir bildirim görür Yüklemek için tıklatın."

     ![Çoğaltılan öğeler penceresi](.\media\vmware-azure-install-mobility-service\replicated-item-notif.png)
3. Sanal makine seçimi sayfasını açmak için bildirim seçin.
4. Mobility hizmeti yükseltmeniz ve seçmek için istediğiniz sanal makineleri seçin **Tamam**.

     ![Çoğaltılan öğeler VM listesi](.\media\vmware-azure-install-mobility-service\update-okpng.png)

Güncelleştirme Mobility hizmeti işi her seçili sanal makineleri için başlatır.

> [!NOTE]
> [Daha fazla bilgi](vmware-azure-manage-configuration-server.md) nasıl mobilite hizmetini yüklemek için kullanılan hesap parolasını güncelleştirin.

## <a name="uninstall-mobility-service-on-a-windows-server-computer"></a>Bir Windows Server bilgisayarında Mobility hizmetini kaldırma
Bir Windows Server bilgisayarında Mobility hizmetini kaldırmak için aşağıdaki yöntemlerden birini kullanın.

### <a name="uninstall-by-using-the-gui"></a>GUI kullanarak kaldırma
1. Denetim Masası'nda seçin **programlar**.
2. Seçin **Microsoft Azure Site Recovery Mobility hizmeti/ana hedef sunucu**ve ardından **kaldırma**.

### <a name="uninstall-at-a-command-prompt"></a>Bir komut isteminde kaldırma
1. Yönetici olarak bir komut istemi penceresi açın.
2. Mobility hizmetini kaldırmak için aşağıdaki komutu çalıştırın:

    ```
    MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} /L+*V "C:\ProgramData\ASRSetupLogs\UnifiedAgentMSIUninstall.log"
    ```

## <a name="uninstall-mobility-service-on-a-linux-computer"></a>Bir Linux bilgisayarda Mobility hizmetini kaldırma
1. Linux sunucunuzda olarak oturum açın bir **kök** kullanıcı.
2. Bir terminale için /user/local/ASR gidin.
3. Mobility hizmetini kaldırmak için aşağıdaki komutu çalıştırın:

    ```
    uninstall.sh -Y
    ```
