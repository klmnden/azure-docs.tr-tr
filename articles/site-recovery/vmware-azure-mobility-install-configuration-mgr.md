---
title: VMware vm'lerinin olağanüstü durum kurtarma için Azure Site Recovery Mobility hizmeti yüklemesi ve fiziksel sunucuları Azure System Center Configuration Manager kullanarak otomatik hale getirin | Microsoft Docs
description: Bu makalede VMware Vm'lerinin ve fiziksel sunucuları Azure Site Recovery ile olağanüstü durum kurtarma için System Center Configuration Manager ile Mobility hizmetini yükleme otomatikleştirmenize yardımcı olur.
author: Rajeswari-Mamilla
ms.service: site-recovery
ms.topic: conceptual
ms.date: 04/14/2019
ms.author: ramamill
ms.openlocfilehash: 35c317c4b73e9a22e3b0d6192abcfc2a596066b8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60598271"
---
# <a name="automate-mobility-service-installation-with-system-center-configuration-manager"></a>Mobility hizmeti yüklemesi System Center Configuration Manager ile otomatik hale getirin

VMware Vm'lerini ve fiziksel sunucuları azure'a çoğaltmak istediğiniz Mobility hizmeti yüklü [Azure Site Recovery](site-recovery-overview.md)

Bu makalede, bir VMware sanal makinesinde Azure Site Recovery Mobility hizmeti dağıtmak için System Center Configuration Manager nasıl kullanabileceğinize bir örnek sağlar. Configuration Manager aşağıdaki avantajlara sahiptir gibi bir yazılım dağıtım aracı kullanarak:

* Temiz yüklemelere ve yükseltmelere, yazılım güncelleştirmeleri için planlı bakım penceresi sırasında zamanlayın
* Aynı anda yüzlerce sunucu dağıtıma ölçeklendirin

Bu makalede, System Center Configuration Manager 2012 R2 dağıtım etkinlik göstermek için kullanır. Varsayar sürümü kullandığınız **9.9.4510.1** veya mobilite hizmetinin daha yüksek.

Alternatif olarak, Mobility hizmeti yüklemesi ile otomatikleştirebilirsiniz [Azure Automation DSC](vmware-azure-mobility-deploy-automation-dsc.md).

## <a name="prerequisites"></a>Önkoşullar

1. Yazılım dağıtım aracı, yapılandırma, ortamınızda dağıtılmış Manager gibi.
2. İki oluşturmalısınız [cihaz koleksiyonları](https://technet.microsoft.com/library/gg682169.aspx), tümü için bir **Windows sunucuları**, diğeri tüm **Linux sunucuları**, Site Recovery kullanarak korumak istediğiniz.
3. Kurtarma Hizmetleri kasasında zaten kayıtlı bir yapılandırma sunucusu.
4. Configuration manager makine tarafından erişilebilen bir güvenli ağ dosya paylaşımı (SMB paylaşımı).

## <a name="deploy-on-windows-machines"></a>Windows makinelerinde dağıtma
> [!NOTE]
> Bu makalede, yapılandırma sunucusunun IP adresi 192.168.3.121 olduğunu ve güvenli bir ağ dosya paylaşımına olduğunu varsayar \\\ContosoSecureFS\MobilityServiceInstallers.

### <a name="prepare-for-deployment"></a>Dağıtım için hazırlanma
1. Ağ paylaşımında bir klasör oluşturun ve adlandırın **MobSvcWindows**.
2. Yapılandırma sunucunuzda oturum açın ve bir yönetici komut istemi açın.
3. Parola dosyası oluşturmak için aşağıdaki komutları çalıştırın:

    `cd %ProgramData%\ASR\home\svsystems\bin`

    `genpassphrase.exe -v > MobSvc.passphrase`
4. Kopyalama **MobSvc.passphrase** doyasını **MobSvcWindows** klasör, ağ paylaşımında.
5. Yükleyiciyi depoya yapılandırma sunucusunda aşağıdaki komutu çalıştırarak göz atın:

   `cd %ProgramData%\ASR\home\svsystems\pushinstallsvc\repository`

6. Kopyalama **Microsoft ASR\_UA\_*sürüm*\_Windows\_GA\_*tarih*\_Release.exe**  için **MobSvcWindows** klasör, ağ paylaşımında.
7. Aşağıdaki kodu kopyalayın ve kaydedileceği **install.bat** içine **MobSvcWindows** klasör.

   > [!NOTE]
   > Bu betik [CSIP] yer tutucuları, yapılandırma sunucusunun IP adresinin gerçek değerlerle değiştirin.

```DOS
Time /t >> C:\Temp\logfile.log
REM ==================================================
REM ==== Clean up the folders ========================
RMDIR /S /q %temp%\MobSvc
MKDIR %Temp%\MobSvc
MKDIR C:\Temp
REM ==================================================

REM ==== Copy new files ==============================
COPY M*.* %Temp%\MobSvc
CD %Temp%\MobSvc
REN Micro*.exe MobSvcInstaller.exe
REM ==================================================

REM ==== Extract the installer =======================
MobSvcInstaller.exe /q /x:%Temp%\MobSvc\Extracted
REM ==== Wait 10s for extraction to complete =========
TIMEOUT /t 10
REM =================================================

REM ==== Perform installation =======================
REM =================================================

CD %Temp%\MobSvc\Extracted
whoami >> C:\Temp\logfile.log
SET PRODKEY=HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall
REG QUERY %PRODKEY%\{275197FC-14FD-4560-A5EB-38217F80CBD1}
IF NOT %ERRORLEVEL% EQU 0 (
    echo "Product is not installed. Goto INSTALL." >> C:\Temp\logfile.log
    GOTO :INSTALL
) ELSE (
    echo "Product is installed." >> C:\Temp\logfile.log

    echo "Checking for Post-install action status." >> C:\Temp\logfile.log
    GOTO :POSTINSTALLCHECK
)

:POSTINSTALLCHECK
    REG QUERY "HKLM\SOFTWARE\Wow6432Node\InMage Systems\Installed Products\5" /v "PostInstallActions" | Find "Succeeded"
    If %ERRORLEVEL% EQU 0 (
        echo "Post-install actions succeeded. Checking for Configuration status." >> C:\Temp\logfile.log
        GOTO :CONFIGURATIONCHECK
    ) ELSE (
        echo "Post-install actions didn't succeed. Goto INSTALL." >> C:\Temp\logfile.log
        GOTO :INSTALL
    )

:CONFIGURATIONCHECK
    REG QUERY "HKLM\SOFTWARE\Wow6432Node\InMage Systems\Installed Products\5" /v "AgentConfigurationStatus" | Find "Succeeded"
    If %ERRORLEVEL% EQU 0 (
        echo "Configuration has succeeded. Goto UPGRADE." >> C:\Temp\logfile.log
        GOTO :UPGRADE
    ) ELSE (
        echo "Configuration didn't succeed. Goto CONFIGURE." >> C:\Temp\logfile.log
        GOTO :CONFIGURE
    )


:INSTALL
    echo "Perform installation." >> C:\Temp\logfile.log
    UnifiedAgent.exe /Role MS /InstallLocation "C:\Program Files (x86)\Microsoft Azure Site Recovery" /Platform "VmWare" /Silent
    IF %ERRORLEVEL% EQU 0 (
        echo "Installation has succeeded." >> C:\Temp\logfile.log
        (GOTO :CONFIGURE)
    ) ELSE (
        echo "Installation has failed." >> C:\Temp\logfile.log
        GOTO :ENDSCRIPT
    )

:CONFIGURE
    echo "Perform configuration." >> C:\Temp\logfile.log
    cd "C:\Program Files (x86)\Microsoft Azure Site Recovery\agent"
    UnifiedAgentConfigurator.exe  /CSEndPoint "[CSIP]" /PassphraseFilePath %Temp%\MobSvc\MobSvc.passphrase
    IF %ERRORLEVEL% EQU 0 (
        echo "Configuration has succeeded." >> C:\Temp\logfile.log
    ) ELSE (
        echo "Configuration has failed." >> C:\Temp\logfile.log
    )
    GOTO :ENDSCRIPT

:UPGRADE
    echo "Perform upgrade." >> C:\Temp\logfile.log
    UnifiedAgent.exe /Platform "VmWare" /Silent
    IF %ERRORLEVEL% EQU 0 (
        echo "Upgrade has succeeded." >> C:\Temp\logfile.log
    ) ELSE (
        echo "Upgrade has failed." >> C:\Temp\logfile.log
    )
    GOTO :ENDSCRIPT

:ENDSCRIPT
    echo "End of script." >> C:\Temp\logfile.log


```

### <a name="create-a-package"></a>Paket oluşturma

1. Configuration Manager konsolunuza oturum açın.
2. Gözat **yazılım Kitaplığı** > **Uygulama Yönetimi** > **paketleri**.
3. Sağ **paketleri**seçip **Paket Oluştur**.
4. Ad, açıklama, üretici, dil ve sürümü için değerler sağlayın.
5. Seçin **bu paket kaynak dosyaları içerir** onay kutusu.
6. Tıklayın **Gözat**, yükleyici depolandığı ağ paylaşımı seçin (\\\ContosoSecureFS\MobilityServiceInstaller\MobSvcWindows).

   ![Ekran görüntüsü, paket ve Program Sihirbazı](./media/vmware-azure-mobility-install-configuration-mgr/create_sccm_package.png)

7. Üzerinde **oluşturmak istediğiniz program türünü seçin** sayfasında **standart Program**, tıklatıp **sonraki**.

   ![Ekran görüntüsü, paket ve Program Sihirbazı](./media/vmware-azure-mobility-install-configuration-mgr/sccm-standard-program.png)

8. Üzerinde **bu standart programla ilgili bilgileri belirtin** sayfasında, aşağıdaki girişleri ve tıklayın **sonraki**. (Diğer girişler varsayılan değerlerine kullanabilirsiniz.)

   | **Parametre adı** | **Değer** |
   |--|--|
   | Ad | Microsoft Azure Mobility hizmetini (Windows) yükleme |
   | Komut satırı | install.bat |
   | Program çalışabilir | Bir kullanıcı olup olmadığını kaydedilir |

   ![Ekran görüntüsü, paket ve Program Sihirbazı](./media/vmware-azure-mobility-install-configuration-mgr/sccm-program-properties.png)

9. Sonraki sayfada, hedef işletim sistemlerini seçin. Mobility hizmeti, yalnızca Windows Server 2012 R2, Windows Server 2012 ve Windows Server 2008 R2 üzerinde yüklenebilir.

   ![Ekran görüntüsü, paket ve Program Sihirbazı](./media/vmware-azure-mobility-install-configuration-mgr/sccm-program-properties-page2.png)

10. Sihirbazı tamamlamak için tıklatın **sonraki** iki kez.


> [!NOTE]
> Betik, Mobility hizmeti aracısı ve önceden yüklenen aracılar için güncelleştirmeleri hem yeni yüklemeleri destekler.

### <a name="deploy-the-package"></a>Paketi dağıtma
1. Configuration Manager konsolunda, pakete sağ tıklayın ve seçin **içeriği Dağıt**.
   ![Ekran görüntüsü, Configuration Manager Konsolu](./media/vmware-azure-mobility-install-configuration-mgr/sccm_distribute.png)
2. Seçin **[dağıtım noktaları](https://technet.microsoft.com/library/gg712321.aspx#BKMK_PlanForDistributionPoints)** hangi paketlerin kopyalanması gereken açın.
3. Sihirbazı tamamlayın. Paket, ardından belirtilen dağıtım noktalarına çoğaltmaya başlar.
4. Paket dağıtımı yapıldıktan sonra pakete sağ tıklayın ve seçin **Dağıt**.
   ![Ekran görüntüsü, Configuration Manager Konsolu](./media/vmware-azure-mobility-install-configuration-mgr/sccm_deploy.png)
5. Dağıtım için hedef koleksiyonu olarak önkoşullar bölümünde oluşturduğunuz Windows Server cihaz koleksiyonunu seçin.

   ![Dağıtma yazılım ekran Sihirbazı](./media/vmware-azure-mobility-install-configuration-mgr/sccm-select-target-collection.png)

6. Üzerinde **içerik hedefini belirt** sayfasında, **dağıtım noktaları**.
7. Üzerinde **bu yazılımın nasıl dağıtılacağını denetlemek için ayarları belirtin** sayfasını, amacı olduğundan emin olun **gerekli**.

   ![Dağıtma yazılım ekran Sihirbazı](./media/vmware-azure-mobility-install-configuration-mgr/sccm-deploy-select-purpose.png)

8. Üzerinde **bu dağıtım için zamanlamayı belirtin** sayfasında, bir zamanlama belirtin. Daha fazla bilgi için [paketleri zamanlama](https://technet.microsoft.com/library/gg682178.aspx).
9. Üzerinde **dağıtım noktaları** sayfasında, veri merkezinizi ihtiyaçlarına göre özelliklerini yapılandırın. Ardından Sihirbazı tamamlayın.

> [!TIP]
> Gereksiz yeniden başlatmalardan önlemek için paket yükleme aylık bir bakım penceresi ya da yazılım güncelleştirmeleri penceresi sırasında zamanlayın.

Configuration Manager konsolunu kullanarak dağıtımın ilerleme durumunu izleyebilirsiniz. Git **izleme** > **dağıtımları** > *[paketinizin adını]*.

  ![Dağıtımları izlemek için Configuration Manager ekran seçeneği](./media/vmware-azure-mobility-install-configuration-mgr/report.PNG)

## <a name="deploy-on-linux-machines"></a>Linux makinelerinde dağıtma
> [!NOTE]
> Bu makalede, yapılandırma sunucusunun IP adresi 192.168.3.121 olduğunu ve güvenli bir ağ dosya paylaşımına olduğunu varsayar \\\ContosoSecureFS\MobilityServiceInstallers.

### <a name="prepare-for-deployment"></a>Dağıtım için hazırlanma
1. Ağ paylaşımında bir klasör oluşturun ve olarak adlandırın **MobSvcLinux**.
2. Yapılandırma sunucunuzda oturum açın ve bir yönetici komut istemi açın.
3. Parola dosyası oluşturmak için aşağıdaki komutları çalıştırın:

    `cd %ProgramData%\ASR\home\svsystems\bin`

    `genpassphrase.exe -v > MobSvc.passphrase`
4. Kopyalama **MobSvc.passphrase** doyasını **MobSvcLinux** klasör, ağ paylaşımında.
5. Yapılandırma sunucusunda yükleyiciyi depoya komutunu çalıştırarak göz atın:

   `cd %ProgramData%\ASR\home\svsystems\pushinstallsvc\repository`

6. Aşağıdaki dosyaları kopyalamak **MobSvcLinux** ağ paylaşımınızda klasörü:
   * Microsoft ASR\_UA\*RHEL6-64\*release.tar.gz
   * Microsoft-ASR\_UA\*RHEL7-64\*release.tar.gz
   * Microsoft-ASR\_UA\*SLES11-SP3-64\*release.tar.gz
   * Microsoft-ASR\_UA\*SLES11-SP4-64\*release.tar.gz
   * Microsoft-ASR\_UA\*OL6-64\*release.tar.gz
   * Microsoft ASR\_UA\*UBUNTU 14.04 64\*release.tar.gz


7. Aşağıdaki kodu kopyalayın ve kaydedileceği **install_linux.sh** içine **MobSvcLinux** klasör.
   > [!NOTE]
   > Bu betik [CSIP] yer tutucuları, yapılandırma sunucusunun IP adresinin gerçek değerlerle değiştirin.

```Bash
#!/usr/bin/env bash

rm -rf /tmp/MobSvc
mkdir -p /tmp/MobSvc
INSTALL_DIR='/usr/local/ASR'
VX_VERSION_FILE='/usr/local/.vx_version'

echo "=============================" >> /tmp/MobSvc/sccm.log
echo `date` >> /tmp/MobSvc/sccm.log
echo "=============================" >> /tmp/MobSvc/sccm.log

if [ -f /etc/oracle-release ] && [ -f /etc/redhat-release ]; then
    if grep -q 'Oracle Linux Server release 6.*' /etc/oracle-release; then
        if uname -a | grep -q x86_64; then
            OS="OL6-64"
            echo $OS >> /tmp/MobSvc/sccm.log
            cp *OL6*.tar.gz /tmp/MobSvc
        fi
    fi
elif [ -f /etc/redhat-release ]; then
    if grep -q 'Red Hat Enterprise Linux Server release 6.* (Santiago)' /etc/redhat-release || \
        grep -q 'CentOS Linux release 6.* (Final)' /etc/redhat-release || \
        grep -q 'CentOS release 6.* (Final)' /etc/redhat-release; then
        if uname -a | grep -q x86_64; then
            OS="RHEL6-64"
            echo $OS >> /tmp/MobSvc/sccm.log
            cp *RHEL6*.tar.gz /tmp/MobSvc
        fi
    elif grep -q 'Red Hat Enterprise Linux Server release 7.* (Maipo)' /etc/redhat-release || \
        grep -q 'CentOS Linux release 7.* (Core)' /etc/redhat-release; then
        if uname -a | grep -q x86_64; then
            OS="RHEL7-64"
            echo $OS >> /tmp/MobSvc/sccm.log
            cp *RHEL7*.tar.gz /tmp/MobSvc
                fi
    fi
elif [ -f /etc/SuSE-release ] && grep -q 'VERSION = 11' /etc/SuSE-release; then
    if grep -q "SUSE Linux Enterprise Server 11" /etc/SuSE-release && grep -q 'PATCHLEVEL = 3' /etc/SuSE-release; then
        if uname -a | grep -q x86_64; then
            OS="SLES11-SP3-64"
            echo $OS >> /tmp/MobSvc/sccm.log
            cp *SLES11-SP3*.tar.gz /tmp/MobSvc
        fi
    elif grep -q "SUSE Linux Enterprise Server 11" /etc/SuSE-release && grep -q 'PATCHLEVEL = 4' /etc/SuSE-release; then
        if uname -a | grep -q x86_64; then
            OS="SLES11-SP4-64"
            echo $OS >> /tmp/MobSvc/sccm.log
            cp *SLES11-SP4*.tar.gz /tmp/MobSvc
        fi
    fi
elif [ -f /etc/lsb-release ] ; then
    if grep -q 'DISTRIB_RELEASE=14.04' /etc/lsb-release ; then
       if uname -a | grep -q x86_64; then
           OS="UBUNTU-14.04-64"
           echo $OS >> /tmp/MobSvc/sccm.log
           cp *UBUNTU-14*.tar.gz /tmp/MobSvc
       fi
    fi
else
    exit 1
fi

if [ -z "$OS" ]; then
    exit 1
fi

Install()
{
    echo "Perform Installation." >> /tmp/MobSvc/sccm.log
    ./install -q -d ${INSTALL_DIR} -r MS -v VmWare
    RET_VAL=$?
    echo "Installation Returncode: $RET_VAL" >> /tmp/MobSvc/sccm.log
    if [ $RET_VAL -eq 0 ]; then
        echo "Installation has succeeded. Proceed to configuration." >> /tmp/MobSvc/sccm.log
        Configure
    else
        echo "Installation has failed." >> /tmp/MobSvc/sccm.log
        exit $RET_VAL
    fi
}

Configure()
{
    echo "Perform configuration." >> /tmp/MobSvc/sccm.log
    ${INSTALL_DIR}/Vx/bin/UnifiedAgentConfigurator.sh -i [CSIP] -P MobSvc.passphrase
    RET_VAL=$?
    echo "Configuration Returncode: $RET_VAL" >> /tmp/MobSvc/sccm.log
    if [ $RET_VAL -eq 0 ]; then
        echo "Configuration has succeeded." >> /tmp/MobSvc/sccm.log
    else
        echo "Configuration has failed." >> /tmp/MobSvc/sccm.log
        exit $RET_VAL
    fi
}

Upgrade()
{
    echo "Perform Upgrade." >> /tmp/MobSvc/sccm.log
    ./install -q -v VmWare
    RET_VAL=$?
    echo "Upgrade Returncode: $RET_VAL" >> /tmp/MobSvc/sccm.log
    if [ $RET_VAL -eq 0 ]; then
        echo "Upgrade has succeeded." >> /tmp/MobSvc/sccm.log
    else
        echo "Upgrade has failed." >> /tmp/MobSvc/sccm.log
        exit $RET_VAL
    fi
}

cp MobSvc.passphrase /tmp/MobSvc
cd /tmp/MobSvc

tar -zxvf *.tar.gz

if [ -e ${VX_VERSION_FILE} ]; then
    echo "${VX_VERSION_FILE} exists. Checking for configuration status." >> /tmp/MobSvc/sccm.log
    agent_configuration=$(grep ^AGENT_CONFIGURATION_STATUS "${VX_VERSION_FILE}" | cut -d"=" -f2 | tr -d " ")
    echo "agent_configuration=$agent_configuration" >> /tmp/MobSvc/sccm.log
     if [ "$agent_configuration" == "Succeeded" ]; then
        echo "Agent is already configured. Proceed to Upgrade." >> /tmp/MobSvc/sccm.log
        Upgrade
    else
        echo "Agent is not configured. Proceed to Configure." >> /tmp/MobSvc/sccm.log
        Configure
    fi
else
    Install
fi

cd /tmp

```

### <a name="create-a-package"></a>Paket oluşturma

1. Configuration Manager konsolunuza oturum açın.
2. Gözat **yazılım Kitaplığı** > **Uygulama Yönetimi** > **paketleri**.
3. Sağ **paketleri**seçip **Paket Oluştur**.
4. Ad, açıklama, üretici, dil ve sürümü için değerler sağlayın.
5. Seçin **bu paket kaynak dosyaları içerir** onay kutusu.
6. Tıklayın **Gözat**, yükleyici depolandığı ağ paylaşımı seçin (\\\ContosoSecureFS\MobilityServiceInstaller\MobSvcLinux).

   ![Ekran görüntüsü, paket ve Program Sihirbazı](./media/vmware-azure-mobility-install-configuration-mgr/create_sccm_package-linux.png)

7. Üzerinde **oluşturmak istediğiniz program türünü seçin** sayfasında **standart Program**, tıklatıp **sonraki**.

   ![Ekran görüntüsü, paket ve Program Sihirbazı](./media/vmware-azure-mobility-install-configuration-mgr/sccm-standard-program.png)

8. Üzerinde **bu standart programla ilgili bilgileri belirtin** sayfasında, aşağıdaki girişleri ve tıklayın **sonraki**. (Diğer girişler varsayılan değerlerine kullanabilirsiniz.)

    | **Parametre adı** | **Değer** |
   |--|--|
   | Ad | Microsoft Azure Mobility hizmetini (Linux) yükleme |
   | Komut satırı | ./install_linux.sh |
   | Program çalışabilir | Bir kullanıcı olup olmadığını kaydedilir |

   ![Ekran görüntüsü, paket ve Program Sihirbazı](./media/vmware-azure-mobility-install-configuration-mgr/sccm-program-properties-linux.png)

9. Sonraki sayfada seçin **Bu program herhangi bir platformda çalışabilir**.
   ![Ekran görüntüsü, paket ve Program Sihirbazı](./media/vmware-azure-mobility-install-configuration-mgr/sccm-program-properties-page2-linux.png)

10. Sihirbazı tamamlamak için tıklatın **sonraki** iki kez.

> [!NOTE]
> Betik, Mobility hizmeti aracısı ve önceden yüklenen aracılar için güncelleştirmeleri hem yeni yüklemeleri destekler.

### <a name="deploy-the-package"></a>Paketi dağıtma
1. Configuration Manager konsolunda, pakete sağ tıklayın ve seçin **içeriği Dağıt**.
   ![Ekran görüntüsü, Configuration Manager Konsolu](./media/vmware-azure-mobility-install-configuration-mgr/sccm_distribute.png)
2. Seçin **[dağıtım noktaları](https://technet.microsoft.com/library/gg712321.aspx#BKMK_PlanForDistributionPoints)** hangi paketlerin kopyalanması gereken açın.
3. Sihirbazı tamamlayın. Paket, ardından belirtilen dağıtım noktalarına çoğaltmaya başlar.
4. Paket dağıtımı yapıldıktan sonra pakete sağ tıklayın ve seçin **Dağıt**.
   ![Ekran görüntüsü, Configuration Manager Konsolu](./media/vmware-azure-mobility-install-configuration-mgr/sccm_deploy.png)
5. Dağıtım için hedef koleksiyonu olarak önkoşullar bölümünde oluşturduğunuz Linux sunucusu cihaz koleksiyonunu seçin.

   ![Dağıtma yazılım ekran Sihirbazı](./media/vmware-azure-mobility-install-configuration-mgr/sccm-select-target-collection-linux.png)

6. Üzerinde **içerik hedefini belirt** sayfasında, **dağıtım noktaları**.
7. Üzerinde **bu yazılımın nasıl dağıtılacağını denetlemek için ayarları belirtin** sayfasını, amacı olduğundan emin olun **gerekli**.

   ![Dağıtma yazılım ekran Sihirbazı](./media/vmware-azure-mobility-install-configuration-mgr/sccm-deploy-select-purpose.png)

8. Üzerinde **bu dağıtım için zamanlamayı belirtin** sayfasında, bir zamanlama belirtin. Daha fazla bilgi için [paketleri zamanlama](https://technet.microsoft.com/library/gg682178.aspx).
9. Üzerinde **dağıtım noktaları** sayfasında, veri merkezinizi ihtiyaçlarına göre özelliklerini yapılandırın. Ardından Sihirbazı tamamlayın.

Mobility hizmetini Linux sunucusuna cihaz koleksiyonunda, yapılandırdığınız zamanlamaya göre yüklenir.


## <a name="uninstall-the-mobility-service"></a>Mobility hizmetini kaldırın
Mobility hizmetini kaldırmak için Configuration Manager paketleri de oluşturabilirsiniz. Bunu yapmak için aşağıdaki betiği kullanın:

```
Time /t >> C:\logfile.log
REM ==================================================
REM ==== Check if Mob Svc is already installed =======
REM ==== If not installed no operation required ========
REM ==== Else run uninstall command =====================
REM ==== {275197FC-14FD-4560-A5EB-38217F80CBD1} is ====
REM ==== guid for Mob Svc Installer ====================
whoami >> C:\logfile.log
NET START | FIND "InMage Scout Application Service"
IF  %ERRORLEVEL% EQU 1 (GOTO :INSTALL) ELSE GOTO :UNINSTALL
:NOOPERATION
                echo "No Operation Required." >> c:\logfile.log
                GOTO :ENDSCRIPT
:UNINSTALL
                echo "Uninstall" >> C:\logfile.log
                MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} /L+*V "C:\ProgramData\ASRSetupLogs\UnifiedAgentMSIUninstall.log"
:ENDSCRIPT

```

## <a name="next-steps"></a>Sonraki adımlar
Artık hazırsınız [korumayı etkinleştirme](vmware-azure-enable-replication.md) sanal makineleriniz için.
