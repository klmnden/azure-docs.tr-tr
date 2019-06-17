---
title: VMware Vm'leri ve fiziksel sunucuları Azure Site Recovery ile olağanüstü durum kurtarma için Mobility hizmeti hakkında | Microsoft Docs
description: VMware Vm'lerini ve fiziksel sunucuları Azure Site Recovery hizmetini kullanarak azure'a olağanüstü durum kurtarma için Mobility Hizmeti Aracısı hakkında bilgi edinin.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 03/25/2019
ms.author: ramamill
ms.openlocfilehash: 929a4e4366c9e94ed4e1915406914991624f6baa
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60565591"
---
# <a name="about-the-mobility-service-for-vmware-vms-and-physical-servers"></a>VMware Vm'lerini ve fiziksel sunucular için Mobility hizmeti hakkında

Ayarladığınızda olağanüstü durum kurtarma için VMware Vm'lerini ve fiziksel sunucuları kullanarak [Azure Site Recovery](site-recovery-overview.md), fiziksel sunucu ve Site Recovery Mobility hizmeti her şirket içi VMware VM'ye yükleyin.  Mobility hizmeti yakalar makinede veri yazar ve onları Site Recovery işlem sunucusuna gönderir. Aşağıdaki yöntemleri kullanarak Mobility hizmetini dağıtabilirsiniz:

- [Göndererek](#push-installation): Azure Portalı aracılığıyla koruma etkinleştirildiğinde site Recovery, mobility Aracısı sunucusuna yükler.
- El ile yükleyin: Mobility hizmetini el ile her bir makineye yükleyebilirsiniz [UI](#install-mobility-agent-through-ui) veya [komut istemi](#install-mobility-agent-through-command-prompt).
- [Otomatik dağıtım](vmware-azure-mobility-install-configuration-mgr.md): Yazılım dağıtım araçları gibi System Center Configuration Manager yüklemesiyle otomatik hale getirebilirsiniz.

## <a name="anti-virus-on-replicated-machines"></a>Çoğaltılan makinelerde virüsten koruma

Çoğaltmak istediğiniz makineleri çalışan active virüsten koruma yazılımı varsa, virüsten koruma işlemlerini Mobility hizmeti yükleme klasör dışlama emin olun (*C:\ProgramData\ASR\agent*). Bu, çoğaltmanın beklendiği gibi çalıştığını sağlar.

## <a name="push-installation"></a>Push yüklemesi

Push yüklemesi bir parçası olan "[çoğaltmayı etkinleştirme](vmware-azure-enable-replication.md#enable-replication)" Proje Portalı'nda tetiklendi. Koruyun ve "Çoğaltmayı etkinleştir" tetiklemek istediğiniz sanal makineler kümesini seçerek sonra yapılandırma sunucusu mobility Aracısı sunucuları açın iter, yapılandırma sunucusu ile aracı kaydınızı tamamlamak ve aracıyı yükler. Bu işlemin başarıyla tamamlanması için

- Tüm göndererek yükleme olun [önkoşulları](vmware-azure-install-mobility-service.md) karşılanır.
- Tüm yapılandırmalar sunucuları altında kalan emin olmak [Azure DR senaryosu için destek matrisi VMware](vmware-physical-azure-support-matrix.md).

Anında iletme yükleme iş akışının ayrıntılarını açıklanan aşağıdaki bölümlerde.

### <a name="from-923-versionhttpssupportmicrosoftcomen-inhelp4494485update-rollup-35-for-azure-site-recovery-onwards"></a>Gelen [9.23 sürüm](https://support.microsoft.com/en-in/help/4494485/update-rollup-35-for-azure-site-recovery) ve sonraki sürümler

Mobility Aracısı göndererek yükleme sırasında aşağıdaki adımları gerçekleştirilir.

1. Bildirim aracı kaynak makineyi açın. Kopyalama aracı kaynak makineyi açın, ortam birden çok hata nedeniyle başarısız olabilir. Ziyaret [kılavuzumuzu](vmware-azure-troubleshoot-push-install.md) anında yükleme hataları giderme.
2. Aracı başarıyla oturum kopyalandıktan sonra sunucusu önkoşul denetimlerini sunucu üzerinde gerçekleştirilir. Yüklenmesi başarısız olursa, bir veya daha fazla [önkoşulları](vmware-physical-azure-support-matrix.md) karşılanmadı. Tüm Önkoşullar karşılanıyorsa, yükleme tetiklenir.
3. Azure Site Recovery VSS sağlayıcısı sunucusunda Mobility Aracısı yüklemesinin bir parçası olarak yüklenir. Bu sağlayıcı uygulama tutarlı noktaları oluşturmak için kullanılır. VSS sağlayıcısı yüklemesi başarısız olursa, bu adımın atlanır ve aracı yüklemesi devam eder.
4. Ardından iş durumu, aracı yüklemesi başarılı ancak VSS sağlayıcısı yüklemesi başarısız olursa, "Uyarı" olarak işaretlenir. Kilitlenme tutarlılığı noktaları üretimi etkilemez.

    a. Uygulama tutarlı noktaları oluşturmak için başvurmak [kılavuzumuzu](vmware-physical-manage-mobility-service.md#install-site-recovery-vss-provider-on-source-machine) Site Recovery VSS sağlayıcısı yüklemesi el ile tamamlamak için. </br>
    b.  Oluşturulacak, uygulama tutarlı noktaları istemiyorsanız [çoğaltma ilkesini değiştirmek](vmware-azure-set-up-replication.md#create-a-policy) uygulama tutarlı noktaları açmak için.

### <a name="before-922-versions"></a>9\.22 sürümlerinden önce

1. Bildirim aracı kaynak makineyi açın. Kopyalama aracı kaynak makineyi açın, ortam birden çok hata nedeniyle başarısız olabilir. Ziyaret [kılavuzumuzu](vmware-azure-troubleshoot-push-install.md) anında yükleme hataları giderme.
2. Aracı başarıyla oturum kopyalandıktan sonra sunucusu önkoşul denetimlerini sunucu üzerinde gerçekleştirilir. Yüklenmesi başarısız olursa, bir veya daha fazla [önkoşulları](vmware-physical-azure-support-matrix.md) karşılanmadı. Tüm Önkoşullar karşılanıyorsa, yükleme tetiklenir.
3. Azure Site Recovery VSS sağlayıcısı sunucusunda Mobility Aracısı yüklemesinin bir parçası olarak yüklenir. Bu sağlayıcı uygulama tutarlı noktaları oluşturmak için kullanılır. VSS sağlayıcısı yüklemesi başarısız olursa, aracı yüklemesi başarısız olur. Mobility Aracısı yükleme hatasını önlemek için [9.23 sürüm](https://support.microsoft.com/en-in/help/4494485/update-rollup-35-for-azure-site-recovery) veya üzeri kilitlenmeyle tutarlı noktaları oluşturmak ve VSS sağlayıcısını el ile yükleyin.

## <a name="install-mobility-agent-through-ui"></a>Mobility Aracısı kullanıcı Arabirimi aracılığıyla yükleyin

### <a name="prerequisite"></a>Önkoşul

- Tüm yapılandırmalar sunucuları altında kalan emin olmak [Azure DR senaryosu için destek matrisi VMware](vmware-physical-azure-support-matrix.md).
- [Yükleyici bulun](#locate-installer-files) sunucusunun işletim sistemini temel alan.

>[!IMPORTANT]
> Bu yöntem, Azure Iaas VM bir Azure bölgesinden diğerine çoğaltma yapıyorsanız, kullanmayın. Komut satırı tabanlı yükleme yöntemi kullanın.

1. Makineye yükleme dosyasını kopyalayın ve çalıştırın.
2. İçinde **yükleme seçeneği**seçin **mobility hizmetini yükleme**.
3. Yükleme konumunu seçin > **yükleme**.

    ![Mobility hizmeti yükleme seçeneği sayfası](./media/vmware-physical-mobility-service-install-manual/mobility1.png)

4. Yükleme izleme **yükleme ilerleme durumu**. Yükleme tamamlandıktan sonra seçin **yapılandırmasına devam** hizmeti ile yapılandırma sunucusunu kaydetmek için.

    ![Mobility hizmeti kayıt sayfası](./media/vmware-physical-mobility-service-install-manual/mobility3.png)

5. içinde **yapılandırma sunucusu ayrıntıları**, yapılandırdığınız parolayı ve IP adresi belirtin.  

    ![Mobility hizmeti kayıt sayfası](./media/vmware-physical-mobility-service-install-manual/mobility4.png)

6. Seçin **kaydetme** kaydı tamamlamak amacıyla.

    ![Mobility hizmeti kayıt son sayfa](./media/vmware-physical-mobility-service-install-manual/mobility5.png)

## <a name="install-mobility-agent-through-command-prompt"></a>Mobility Aracısı komut istemi üzerinden yükleme

### <a name="prerequisite"></a>Önkoşul

- Tüm yapılandırmalar sunucuları altında kalan emin olmak [Azure DR senaryosu için destek matrisi VMware](vmware-physical-azure-support-matrix.md).
- [Yükleyici bulun](#locate-installer-files) sunucusunun işletim sistemini temel alan.

### <a name="on-a-windows-machine"></a>Bir Windows makinede

- Yükleyiciyi, korumak istediğiniz sunucuda yerel bir klasöre (örneğin, C:\Temp) kopyalayın.

    ```
    cd C:\Temp
    ren Microsoft-ASR_UA*Windows*release.exe MobilityServiceInstaller.exe
    MobilityServiceInstaller.exe /q /x:C:\Temp\Extracted
    cd C:\Temp\Extracted
    ```

- Aşağıda gösterildiği gibi yükleyin:

    ```
    UnifiedAgent.exe /Role "MS" /InstallLocation "C:\Program Files (x86)\Microsoft Azure Site Recovery" /Platform "VmWare" /Silent
    ```

- Aracı yapılandırma sunucusuna kaydedin.

    ```
    cd C:\Program Files (x86)\Microsoft Azure Site Recovery\agent
    UnifiedAgentConfigurator.exe  /CSEndPoint <CSIP> /PassphraseFilePath <PassphraseFilePath>
    ```

#### <a name="installation-settings"></a>Yükleme ayarları
**Ayar** | **Ayrıntılar**
--- | ---
Kullanım | UnifiedAgent.exe/role < MS/MT > /InstallLocation  <Install Location> /Platform "VmWare" / silent
Kurulum günlükleri | % ProgramData%\ASRSetupLogs\ASRUnifiedAgentInstaller.log altında.
/ Rol | Zorunlu yükleme parametresi. Mobility hizmetinin (MS) veya ana hedef (MT) yüklü olup olmadığını belirtir.
/InstallLocation| İsteğe bağlı parametre. Mobility hizmeti yükleme konumuna (herhangi bir klasör) belirtir.
/ Platform | Zorunlu. Mobility hizmetinin yüklendiği platformunu belirtir. **VMware** VMware Vm'lerini/fiziksel sunucuları için; **Azure** Azure sanal makineler için.
/ Silent| İsteğe bağlı. Yükleyici sessiz modda çalıştırılıp çalıştırılmayacağını belirtir.

#### <a name="registration-settings"></a>Kayıt ayarları
**Ayar** | **Ayrıntılar**
--- | ---
Kullanım | UnifiedAgentConfigurator.exe/csendpoint \<CSIP >/passphrasefilepath \<PassphraseFilePath >
Aracı yapılandırma günlükleri | % ProgramData%\ASRSetupLogs\ASRUnifiedAgentConfigurator.log altında.
/CSEndPoint | Zorunlu parametre. Yapılandırma sunucusunun IP adresini belirtir. Herhangi bir geçerli IP adresi kullanın.
/PassphraseFilePath |  Zorunlu. Parola dosyasının konumu. Herhangi bir geçerli UNC veya yerel dosya yolu kullanın.

### <a name="on-a-linux-machine"></a>Bir Linux makinesinde

1. Yükleyiciyi, korumak istediğiniz sunucuda yerel bir klasöre (örneğin, / tmp) kopyalayın. Bir terminal penceresinde aşağıdaki komutları çalıştırın:

    ```
    cd /tmp ;
    tar -xvf Microsoft-ASR_UA*release.tar.gz
    ```

2. Aşağıda gösterildiği gibi yükleyin:

    ```
    sudo ./install -d <Install Location> -r MS -v VmWare -q
    ```

3. Mobility hizmeti yüklemesi tamamlandıktan sonra yapılandırma sunucusuna kaydedilmesi gerekir. Mobility hizmeti ile yapılandırma sunucusunu kaydetmek için aşağıdaki komutu çalıştırın:

    ```
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <CSIP> -P /var/passphrase.txt
    ```

#### <a name="installation-settings"></a>Yükleme ayarları
**Ayar** | **Ayrıntılar**
--- | ---
Kullanım | . / install -d <Install Location> - r < MS/MT > - v VmWare - q
-r | Zorunlu yükleme parametresi. Mobility hizmetinin (MS) veya ana hedef (MT) yüklü olup olmadığını belirtir.
-d | İsteğe bağlı parametre. Mobility hizmeti yükleme konumunu belirtir: /usr/local/ASR.
-v | Zorunlu. Mobility hizmetinin yüklendiği platformunu belirtir. **VMware** VMware Vm'lerini/fiziksel sunucuları için; **Azure** Azure sanal makineler için.
-q | İsteğe bağlı. Yükleyici sessiz modda çalıştırılıp çalıştırılmayacağını belirtir.

#### <a name="registration-settings"></a>Kayıt ayarları
**Ayar** | **Ayrıntılar**
--- | ---
Kullanım | CD /usr/local/ASR/Vx/bin<br/><br/> UnifiedAgentConfigurator.sh -i \<CSIP > -P \<PassphraseFilePath >
-i | Zorunlu parametre. Yapılandırma sunucusunun IP adresini belirtir. Herhangi bir geçerli IP adresi kullanın.
-P |  Zorunlu. Parola kaydedildiği dosyasının tam dosya yolu. Herhangi bir geçerli klasörü kullanın.

## <a name="azure-virtual-machine-agent"></a>Azure sanal makine Aracısı

- **Windows Vm'leri**: Mobility hizmetinin 9.7.0.0 sürümünden [Azure VM Aracısı](../virtual-machines/extensions/features-windows.md#azure-vm-agent) Mobility hizmeti yükleyicisinin tarafından yüklenir. Bu, üzerinden Azure'a makine başarısız olduğunda, Azure VM aracı yüklemesi için herhangi bir Vm uzantısı kullanarak önkoşul karşıladığını sağlar.
- **Linux Vm'leri**: [WALinuxAgent](https://docs.microsoft.com/azure/virtual-machines/extensions/update-linux-agent) yük devretme sonrasında Azure VM'de el ile yüklenmelidir.

## <a name="locate-installer-files"></a>Yükleyici dosyalarını bulun

Yapılandırma sunucusundaki %ProgramData%\ASR\home\svsystems\pushinstallsvc\repository klasöre gidin. İşletim sistemi temelinde ihtiyacınız yükleyiciyi denetleyin. Aşağıdaki tabloda, her bir VMware VM ve fiziksel sunucu işletim sistemi için yükleyici dosyalarını özetlenmektedir. Gözden geçirebilirsiniz [desteklenen işletim sistemleri](vmware-physical-azure-support-matrix.md#replicated-machines) başlamadan önce.

**Yükleyici dosyası** | **İşletim sistemi (yalnızca 64-bit)**
--- | ---
Microsoft-ASR\_UA\*Windows\*release.exe | Windows Server 2016; Windows Server 2012 R2; Windows Server 2012; Windows Server 2008 R2 SP1
Microsoft ASR\_UA\*RHEL6 64\*release.tar.gz | Red Hat Enterprise Linux (RHEL) 6.* </br> CentOS 6.*
Microsoft-ASR\_UA\*RHEL7-64\*release.tar.gz | Red Hat Enterprise Linux (RHEL) 7.* </br> CentOS 7.*
Microsoft ASR\_UA\*SLES12 64\*release.tar.gz | SUSE Linux Enterprise Server 12 SP1, SP2 SP3
Microsoft-ASR\_UA\*SLES11-SP3-64\*release.tar.gz| SUSE Linux Enterprise Server 11 SP3
Microsoft-ASR\_UA\*SLES11-SP4-64\*release.tar.gz| SUSE Linux Enterprise Server 11 SP4
Microsoft-ASR\_UA\*OL6-64\*release.tar.gz | Oracle Enterprise Linux 6.4, 6.5
Microsoft ASR\_UA\*UBUNTU 14.04 64\*release.tar.gz | Ubuntu Linux 14.04
Microsoft ASR\_UA\*UBUNTU-16.04-64\*release.tar.gz | Ubuntu Linux 16.04 LTS server
Microsoft ASR_UA\*DEBIAN7 64\*release.tar.gz | Debian 7
Microsoft ASR_UA\*DEBIAN8 64\*release.tar.gz | Debian 8

## <a name="next-steps"></a>Sonraki adımlar

[Mobility hizmetinin göndererek yüklenmesine ayarlama](vmware-azure-install-mobility-service.md).
