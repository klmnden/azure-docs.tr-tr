---
title: Azure Site Recovery ile VMware Vm'lerini ve fiziksel sunucuları olağanüstü durum kurtarma için el ile Mobility hizmetini yükleme | Microsoft Docs
description: VMware Vm'lerini ve fiziksel sunucuları olağanüstü durum kurtarma için Mobility Hizmeti Aracısı Azure Site Recovery hizmetini kullanarak Azure'a yüklemeyi öğrenin.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/04/2018
ms.author: raynew
ms.openlocfilehash: dfed0209131379843b97ff8050c2f2ba7294537d
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51019391"
---
# <a name="install-the-mobility-service-manually-on-vmware-vms-and-physical-servers"></a>VMware Vm'leri ve fiziksel sunucuları üzerinde Mobility hizmetini el ile yükleme

Ayarladığınızda olağanüstü durum kurtarma için VMware Vm'lerini ve fiziksel sunucuları kullanarak [Azure Site Recovery](site-recovery-overview.md), yüklediğiniz [Site Recovery Mobility hizmetini](vmware-physical-mobility-service-overview.md) her bir şirket içi VMware VM ve fiziksel sunucu.  Mobility hizmeti yakalar makinede veri yazar ve onları Site Recovery işlem sunucusuna gönderir.

Bu makalede, Mobility hizmetini el ile korumak istediğiniz her makinede nasıl yükleneceği açıklanır.

## <a name="create-a-passphrase"></a>Bir parola oluştur

Yüklemeden önce yükleme sırasında kullanılacak bir parola oluşturun.

1. Yapılandırma sunucusuna oturum açın.
2. Yönetici olarak bir komut istemi açın.
3. Bin klasörüne erişmeye dizini değiştirin ve ardından bir parola dosyası oluşturun.

    ```
    cd %ProgramData%\ASR\home\svsystems\bin
    genpassphrase.exe -v > MobSvc.passphrase
    ```
3. Parola dosyası güvenli bir konuma Store. 


## <a name="install-the-service-from-the-ui"></a>Kullanıcı Arabiriminden Hizmeti'ni yükleme

>[!IMPORTANT]
> Bu yöntem, Azure Iaas VM bir Azure bölgesinden diğerine çoğaltma yapıyorsanız kullanmayın. Komut satırı tabanlı yükleme yöntemi insteadl kullanın.

1. Makine işletim sistemi için gereksinim duyduğunuz yükleyici sürümünü bulun. Yükleyicileri %ProgramData%\ASR\home\svsystems\pushinstallsvc\repository klasöründe bulunur. [Denetleme](vmware-physical-mobility-service-overview.md#installer-files) ihtiyacınız yükleyiciyi.
2. Makineye yükleme dosyasını kopyalayın ve çalıştırın.
3. İçinde **yükleme seçeneği**seçin **mobility hizmetini yükleme**.
4. Yükleme konumunu seçin > **yükleme**.

    ![Mobility hizmeti yükleme seçeneği sayfası](./media/vmware-physical-mobility-service-install-manual/mobility1.png)

5. Yükleme izleme **yükleme ilerleme durumu**. Yükleme tamamlandıktan sonra seçin **yapılandırmasına devam** hizmeti ile yapılandırma sunucusunu kaydetmek için.

    ![Mobility hizmeti kayıt sayfası](./media/vmware-physical-mobility-service-install-manual/mobility3.png)

6.  içinde **yapılandırma sunucusu ayrıntıları**, yapılandırdığınız parolayı ve IP adresi belirtin.  

    ![Mobility hizmeti kayıt sayfası](./media/vmware-physical-mobility-service-install-manual/mobility4.png)

7. Seçin **kaydetme** kaydı tamamlamak amacıyla.

    ![Mobility hizmeti kayıt son sayfa](./media/vmware-physical-mobility-service-install-manual/mobility5.png)

## <a name="install-the-service-from-the-command-prompt"></a>Komut İstemi'nden Hizmeti'ni yükleme

### <a name="on-a-windows-machine"></a>Bir Windows makinede

1. Yükleyiciyi, korumak istediğiniz sunucuda yerel bir klasöre (örneğin, C:\Temp) kopyalayın. 

  ```
  cd C:\Temp
  ren Microsoft-ASR_UA*Windows*release.exe MobilityServiceInstaller.exe
  MobilityServiceInstaller.exe /q /x:C:\Temp\Extracted
  cd C:\Temp\Extracted.
  ```
2. Aşağıda gösterildiği gibi yükleyin:

  ```
  UnifiedAgent.exe /Role "MS" /InstallLocation "C:\Program Files (x86)\Microsoft Azure Site Recovery" /Platform "VmWare" /Silent
  ```

3. Aracı yapılandırma sunucusuna kaydedin.

  ```
  cd C:\Program Files (x86)\Microsoft Azure Site Recovery\agent
  UnifiedAgentConfigurator.exe  /CSEndPoint <CSIP> /PassphraseFilePath <PassphraseFilePath>
  ```

#### <a name="installation-settings"></a>Yükleme ayarları
**Ayar** | **Ayrıntılar**
--- | ---
Kullanım | UnifiedAgent.exe/role < MS|MT > /InstallLocation  <Install Location> /Platform "VmWare" / silent
Kurulum günlükleri | % ProgramData%\ASRSetupLogs\ASRUnifiedAgentInstaller.log altında.
/ Rol | Zorunlu yükleme parametresi. Mobility hizmetinin (MS) veya ana hedef (MT) yüklü olup olmadığını belirtir.
/InstallLocation| İsteğe bağlı parametre. Mobility hizmeti yükleme konumuna (herhangi bir klasör) belirtir.
/ Platform | Zorunlu. Mobility hizmetinin yüklendiği platformunu belirtir. **VMware** Mware VM'ler/fiziksel sunucuları için; **Azure** Azure sanal makineler için. 
/ Silent| İsteğe bağlı. Yükleyici sessiz modda çalıştırılıp çalıştırılmayacağını belirtir.

#### <a name="registration-settings"></a>Kayıt ayarları
**Ayar** | **Ayrıntılar**
--- | ---
Kullanım | UnifiedAgentConfigurator.exe/csendpoint  <CSIP> /passphrasefilepath <PassphraseFilePath>
Aracı yapılandırma günlükleri | % ProgramData%\ASRSetupLogs\ASRUnifiedAgentConfigurator.log altında.
/CSEndPoint | Zorunlu parametre. Yapılandırma sunucusunun IP adresini belirtir. Herhangi bir geçerli IP adresi kullanın.
/PassphraseFilePath |  Zorunlu. Parola dosyasının konumu. Herhangi bir geçerli UNC veya yerel dosya yolu kullanın.


### <a name="on-a-linux-machine"></a>Bir Linux makinesinde

1. Yükleyiciyi, korumak istediğiniz sunucuda yerel bir klasöre (örneğin, / tmp) kopyalayın. Bir terminal penceresinde aşağıdaki komutları çalıştırın:
  ```
  cd /tmp ;

  tar -xvzf Microsoft-ASR_UA*release.tar.gz
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
Kullanım | . / install -d <Install Location> - r < MS|MT > - v, VmWare - q
-r | Zorunlu yükleme parametresi. Mobility hizmetinin (MS) veya ana hedef (MT) yüklü olup olmadığını belirtir.
-d | İsteğe bağlı parametre. Mobility hizmeti yükleme konumunu belirtir: /usr/local/ASR.
-v | Zorunlu. Mobility hizmetinin yüklendiği platformunu belirtir. **VMware** Mware VM'ler/fiziksel sunucuları için; **Azure** Azure sanal makineler için. 
-q | İsteğe bağlı. Yükleyici sessiz modda çalıştırılıp çalıştırılmayacağını belirtir.

#### <a name="registration-settings"></a>Kayıt ayarları
**Ayar** | **Ayrıntılar**
--- | ---
Kullanım | CD /usr/local/ASR/Vx/bin<br/><br/> UnifiedAgentConfigurator.sh -i <CSIP> - P <PassphraseFilePath>
-i | Zorunlu parametre. Yapılandırma sunucusunun IP adresini belirtir. Herhangi bir geçerli IP adresi kullanın.
-P |  Zorunlu. Parola kaydedildiği dosyasının tam dosya yolu. Herhangi bir geçerli klasörü kullanın

## <a name="next-steps"></a>Sonraki adımlar
- [VMware Vm'leri için olağanüstü durum kurtarmayı ayarlama](vmware-azure-tutorial.md)
- [Fiziksel sunucular için olağanüstü durum kurtarmayı ayarlama](physical-azure-disaster-recovery.md)
