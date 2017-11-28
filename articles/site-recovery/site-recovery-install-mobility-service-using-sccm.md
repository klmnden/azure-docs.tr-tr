---
title: "Yazılım dağıtım araçları kullanarak Azure Site Recovery Mobility hizmeti yüklemesi otomatikleştirmek | Microsoft Docs"
description: "Bu makalede, System Center Configuration Manager gibi yazılım dağıtım araçları kullanarak Mobility hizmetinin yüklenmesini otomatikleştirmek yardımcı olur."
services: site-recovery
documentationcenter: 
author: AnoopVasudavan
manager: gauravd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.author: anoopkv
ms.openlocfilehash: cfcc20b55b9e9b662ea8eec0802727b9a5a0ebf0
ms.sourcegitcommit: 5bced5b36f6172a3c20dbfdf311b1ad38de6176a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/27/2017
---
# <a name="automate-mobility-service-installation-by-using-software-deployment-tools"></a>Yazılım dağıtım araçları kullanarak mobilite hizmeti yükleme otomatikleştirme

>[!IMPORTANT]
Bu belge sürümü kullandığınız varsayılır **9.9.4510.1** ya da daha yüksek.

Bu makalede, Azure Site Recovery Mobility hizmeti, veri merkezinizde dağıtmak için System Center Configuration Manager nasıl kullanabileceğinize bir örnek sağlar. Configuration Manager'ı aşağıdaki avantajlara sahip gibi bir yazılım dağıtım aracını kullanma:
* Yazılım güncelleştirmeleri için planlanan bakım penceresi sırasında yeni yüklemeleri ve yükseltmeleri, dağıtımını planlama
* Yüzlerce dağıtımına aynı anda ölçeklendirme


> [!NOTE]
> Bu makalede, bir dağıtım etkinliğiyle göstermek için System Center Configuration Manager 2012 R2 kullanır. Mobility hizmeti yükleme kullanarak da otomatikleştirmek [Azure Automation ve istenen durum Yapılandırması](site-recovery-automate-mobility-service-install.md).

## <a name="prerequisites"></a>Ön koşullar
1. Yazılım dağıtım aracı, yapılandırma, ortamınızda dağıtılmış Manager gibi.
  İki oluşturmak [cihaz koleksiyonları](https://technet.microsoft.com/library/gg682169.aspx), tüm için bir tane **Windows sunucuları**, diğeri tüm **Linux sunucuları**, Site Recovery kullanarak korumak istediğiniz.
3. Site Recovery ile zaten kayıtlı bir yapılandırma sunucusu.
4. Configuration Manager sunucusu tarafından erişilebilecek güvenli bir ağ dosya paylaşımına (sunucu ileti bloğu paylaşım).

## <a name="deploy-mobility-service-on-computers-running-windows"></a>Mobility hizmetinin Windows çalıştıran bilgisayarlara dağıtma
> [!NOTE]
> Bu makalede yapılandırma sunucusu IP adresini 192.168.3.121 olduğunu ve güvenli bir ağ dosya paylaşımına olduğunu varsayar \\\ContosoSecureFS\MobilityServiceInstallers.

### <a name="step-1-prepare-for-deployment"></a>Adım 1: dağıtıma Hazırlan
1. Ağ paylaşımında bir klasör oluşturun ve adlandırın **MobSvcWindows**.
2. Yapılandırma sunucunuza oturum açın ve bir yönetici komut istemi açın.
3. Bir parola dosyası oluşturmak için aşağıdaki komutları çalıştırın:

    `cd %ProgramData%\ASR\home\svsystems\bin`

    `genpassphrase.exe -v > MobSvc.passphrase`
4. Kopya **MobSvc.passphrase** içine dosya **MobSvcWindows** klasör, ağ paylaşımında.
5. Yapılandırma sunucusundaki yükleyici deposu için aşağıdaki komutu çalıştırarak göz atın:

   `cd %ProgramData%\ASR\home\svsystems\puhsinstallsvc\repository`

6. Kopya  **Microsoft ASR\_UA\_*sürüm*\_Windows\_GA\_*tarih* \_İçin Release.exe** **MobSvcWindows** klasör, ağ paylaşımında.
7. Aşağıdaki kodu kopyalayın ve kaydedileceği **install.bat** içine **MobSvcWindows** klasör.

   > [!NOTE]
   > Bu komut dosyasındaki [CSIP] yer tutucuları yapılandırma sunucunuzun IP adresini gerçek değerlerle değiştirin.

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

### <a name="step-2-create-a-package"></a>2. adım: bir paket oluşturun

1. Configuration Manager konsolunuza oturum açın.
2. Gözat **yazılım Kitaplığı** > **Uygulama Yönetimi** > **paketleri**.
3. Sağ **paketleri**seçip **Paket Oluştur**.
4. Ad, açıklama, üreticisi, dil ve sürüm için değerler sağlayın.
5. Seçin **bu paket kaynak dosyalarını içeren** onay kutusu.
6. Tıklatın **Gözat**, yükleyici depolandığı ağ paylaşımı seçin (\\\ContosoSecureFS\MobilityServiceInstaller\MobSvcWindows).

  ![Ekran görüntüsü, paket ve Program Sihirbazı](./media/site-recovery-install-mobility-service-using-sccm/create_sccm_package.png)

7. Üzerinde **oluşturmak istediğiniz program türünü seçin** sayfasında, **standart Program**, tıklatıp **sonraki**.

  ![Ekran görüntüsü, paket ve Program Sihirbazı](./media/site-recovery-install-mobility-service-using-sccm/sccm-standard-program.png)

8. Üzerinde **bu standart programla ilgili bilgiler belirt** sayfasında, aşağıdaki girişleri yapın ve tıklayın **sonraki**. (Diğer girişleri varsayılan değerleri kullanabilirsiniz.)

  | **Parametre adı** | **Değer** |
  |--|--|
  | Ad | Microsoft Azure Mobility hizmetini (Windows) yükleme |
  | Komut satırı | install.bat |
  | Program çalışabilir | Bir kullanıcı olup olmadığına oturum |

  ![Ekran görüntüsü, paket ve Program Sihirbazı](./media/site-recovery-install-mobility-service-using-sccm/sccm-program-properties.png)

9. Sonraki sayfada, hedef işletim sistemlerini seçin. Mobility hizmeti, yalnızca Windows Server 2012 R2, Windows Server 2012 ve Windows Server 2008 R2 üzerinde yüklenebilir.

  ![Ekran görüntüsü, paket ve Program Sihirbazı](./media/site-recovery-install-mobility-service-using-sccm/sccm-program-properties-page2.png)

10. Sihirbazı tamamlamak için tıklatın **sonraki** iki kez.


> [!NOTE]
> Komut dosyası Mobility hizmeti aracısı ve yüklü olan aracıları güncelleştirmeleri hem yeni yüklemeleri destekler.

### <a name="step-3-deploy-the-package"></a>Adım 3: Paket dağıtma
1. Configuration Manager konsolunda, pakete sağ tıklayın ve seçin **içeriği Dağıt**.
  ![Ekran görüntüsü, Configuration Manager Konsolu](./media/site-recovery-install-mobility-service-using-sccm/sccm_distribute.png)
2. Seçin  **[dağıtım noktaları](https://technet.microsoft.com/library/gg712321.aspx#BKMK_PlanForDistributionPoints)**  paketleri kopyalanmalıdır açın.
3. Sihirbazı tamamlayın. Paket sonra belirtilen dağıtım noktalarına çoğaltmaya başlar.
4. Paket dağıtımı yapıldıktan sonra pakete sağ tıklayın ve seçin **dağıtma**.
  ![Ekran görüntüsü, Configuration Manager Konsolu](./media/site-recovery-install-mobility-service-using-sccm/sccm_deploy.png)
5. Dağıtımı için hedef koleksiyonu olarak önkoşullar bölümünde oluşturduğunuz Windows Server cihaz koleksiyonunu seçin.

  ![Ekran görüntüsü, yazılımı Dağıt Sihirbazı](./media/site-recovery-install-mobility-service-using-sccm/sccm-select-target-collection.png)

6. Üzerinde **içerik hedefini belirt** sayfasında, sizin **dağıtım noktaları**.
7. Üzerinde **bu yazılımın nasıl dağıtılacağını denetlemek için ayarları belirtin** sayfasında, amacı olduğundan emin olun **gerekli**.

  ![Ekran görüntüsü, yazılımı Dağıt Sihirbazı](./media/site-recovery-install-mobility-service-using-sccm/sccm-deploy-select-purpose.png)

8. Üzerinde **bu dağıtım için zamanlamayı belirtin** sayfasında, bir zamanlama belirtin. Daha fazla bilgi için bkz: [paketleri zamanlama](https://technet.microsoft.com/library/gg682178.aspx).
9. Üzerinde **dağıtım noktaları** sayfasında, veri merkeziniz gereksinimlerine göre özelliklerini yapılandırın. Sihirbazı tamamlayın.

> [!TIP]
> Gereksiz yeniden başlatmalardan önlemek için paket yükleme aylık bakım penceresi veya yazılım güncelleştirmeleri penceresi sırasında zamanlayın.

Configuration Manager konsolunu kullanarak dağıtımının ilerleme durumunu izleyebilirsiniz. Git **izleme** > **dağıtımları** > *[paket adınız]*.

  ![Dağıtımlarını izlemek için Configuration Manager ekran seçeneği](./media/site-recovery-install-mobility-service-using-sccm/report.PNG)

## <a name="deploy-mobility-service-on-computers-running-linux"></a>Mobility hizmeti Linux çalıştıran bilgisayarlara dağıtma
> [!NOTE]
> Bu makalede yapılandırma sunucusu IP adresini 192.168.3.121 olduğunu ve güvenli bir ağ dosya paylaşımına olduğunu varsayar \\\ContosoSecureFS\MobilityServiceInstallers.

### <a name="step-1-prepare-for-deployment"></a>Adım 1: dağıtıma Hazırlan
1. Ağ paylaşımında bir klasör oluşturun ve olarak adlandırın **MobSvcLinux**.
2. Yapılandırma sunucunuza oturum açın ve bir yönetici komut istemi açın.
3. Bir parola dosyası oluşturmak için aşağıdaki komutları çalıştırın:

    `cd %ProgramData%\ASR\home\svsystems\bin`

    `genpassphrase.exe -v > MobSvc.passphrase`
4. Kopya **MobSvc.passphrase** içine dosya **MobSvcLinux** klasör, ağ paylaşımında.
5. Yapılandırma sunucusundaki yükleyici deponuza komutunu çalıştırarak göz atın:

   `cd %ProgramData%\ASR\home\svsystems\puhsinstallsvc\repository`

6. Aşağıdaki dosyaları kopyalayın **MobSvcLinux** ağ paylaşımınızda klasörü:
   * Microsoft ASR\_UA\*RHEL6 64*release.tar.gz
   * Microsoft ASR\_UA\*RHEL7 64\*release.tar.gz
   * Microsoft ASR\_UA\*SLES11 SP3 64\*release.tar.gz
   * Microsoft ASR\_UA\*SLES11 SP4 64\*release.tar.gz
   * Microsoft ASR\_UA\*OL6 64\*release.tar.gz
   * Microsoft ASR\_UA\*UBUNTU 14.04 64\*release.tar.gz


7. Aşağıdaki kodu kopyalayın ve kaydedileceği **install_linux.sh** içine **MobSvcLinux** klasör.
   > [!NOTE]
   > Bu komut dosyasındaki [CSIP] yer tutucuları yapılandırma sunucunuzun IP adresini gerçek değerlerle değiştirin.

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

### <a name="step-2-create-a-package"></a>2. adım: bir paket oluşturun

1. Configuration Manager konsolunuza oturum açın.
2. Gözat **yazılım Kitaplığı** > **Uygulama Yönetimi** > **paketleri**.
3. Sağ **paketleri**seçip **Paket Oluştur**.
4. Ad, açıklama, üreticisi, dil ve sürüm için değerler sağlayın.
5. Seçin **bu paket kaynak dosyalarını içeren** onay kutusu.
6. Tıklatın **Gözat**, yükleyici depolandığı ağ paylaşımı seçin (\\\ContosoSecureFS\MobilityServiceInstaller\MobSvcLinux).

  ![Ekran görüntüsü, paket ve Program Sihirbazı](./media/site-recovery-install-mobility-service-using-sccm/create_sccm_package-linux.png)

7. Üzerinde **oluşturmak istediğiniz program türünü seçin** sayfasında, **standart Program**, tıklatıp **sonraki**.

  ![Ekran görüntüsü, paket ve Program Sihirbazı](./media/site-recovery-install-mobility-service-using-sccm/sccm-standard-program.png)

8. Üzerinde **bu standart programla ilgili bilgiler belirt** sayfasında, aşağıdaki girişleri yapın ve tıklayın **sonraki**. (Diğer girişleri varsayılan değerleri kullanabilirsiniz.)

    | **Parametre adı** | **Değer** |
  |--|--|
  | Ad | Microsoft Azure Mobility hizmetini (Linux) yükleme |
  | Komut satırı | ./install_linux.sh |
  | Program çalışabilir | Bir kullanıcı olup olmadığına oturum |

  ![Ekran görüntüsü, paket ve Program Sihirbazı](./media/site-recovery-install-mobility-service-using-sccm/sccm-program-properties-linux.png)

9. Sonraki sayfada seçin **Bu program herhangi bir platformda çalışabilir**.
  ![Ekran görüntüsü, paket ve Program Sihirbazı](./media/site-recovery-install-mobility-service-using-sccm/sccm-program-properties-page2-linux.png)

10. Sihirbazı tamamlamak için tıklatın **sonraki** iki kez.

> [!NOTE]
> Komut dosyası Mobility hizmeti aracısı ve yüklü olan aracıları güncelleştirmeleri hem yeni yüklemeleri destekler.

### <a name="step-3-deploy-the-package"></a>Adım 3: Paket dağıtma
1. Configuration Manager konsolunda, pakete sağ tıklayın ve seçin **içeriği Dağıt**.
  ![Ekran görüntüsü, Configuration Manager Konsolu](./media/site-recovery-install-mobility-service-using-sccm/sccm_distribute.png)
2. Seçin  **[dağıtım noktaları](https://technet.microsoft.com/library/gg712321.aspx#BKMK_PlanForDistributionPoints)**  paketleri kopyalanmalıdır açın.
3. Sihirbazı tamamlayın. Paket sonra belirtilen dağıtım noktalarına çoğaltmaya başlar.
4. Paket dağıtımı yapıldıktan sonra pakete sağ tıklayın ve seçin **dağıtma**.
  ![Ekran görüntüsü, Configuration Manager Konsolu](./media/site-recovery-install-mobility-service-using-sccm/sccm_deploy.png)
5. Dağıtımı için hedef koleksiyonu olarak önkoşullar bölümünde oluşturduğunuz Linux sunucusu cihaz koleksiyonunu seçin.

  ![Ekran görüntüsü, yazılımı Dağıt Sihirbazı](./media/site-recovery-install-mobility-service-using-sccm/sccm-select-target-collection-linux.png)

6. Üzerinde **içerik hedefini belirt** sayfasında, sizin **dağıtım noktaları**.
7. Üzerinde **bu yazılımın nasıl dağıtılacağını denetlemek için ayarları belirtin** sayfasında, amacı olduğundan emin olun **gerekli**.

  ![Ekran görüntüsü, yazılımı Dağıt Sihirbazı](./media/site-recovery-install-mobility-service-using-sccm/sccm-deploy-select-purpose.png)

8. Üzerinde **bu dağıtım için zamanlamayı belirtin** sayfasında, bir zamanlama belirtin. Daha fazla bilgi için bkz: [paketleri zamanlama](https://technet.microsoft.com/library/gg682178.aspx).
9. Üzerinde **dağıtım noktaları** sayfasında, veri merkeziniz gereksinimlerine göre özelliklerini yapılandırın. Sihirbazı tamamlayın.

Mobility hizmeti Linux sunucusu cihaz koleksiyonunda, yapılandırdığınız zamanlamaya göre yüklü.

## <a name="other-methods-to-install-mobility-service"></a>Mobilite hizmetinin yüklenmesi için diğer yöntemleri
Mobility hizmetinin yüklenmesi için diğer bazı seçenekleri şunlardır:
* [GUI kullanarak el ile yükleme](http://aka.ms/mobsvcmanualinstall)
* [Komut satırı kullanarak el ile yükleme](http://aka.ms/mobsvcmanualinstallcli)
* [Yapılandırma sunucusu kullanarak yükleme](http://aka.ms/pushinstall)
* [Azure Otomasyonu & istenen durum Yapılandırması'nı kullanarak otomatik yükleme](http://aka.ms/mobsvcdscinstall)

## <a name="uninstall-mobility-service"></a>Mobility hizmetini kaldırma
Mobility hizmetini kaldırmak için Configuration Manager paketler oluşturabilirsiniz. Bunu yapmak için aşağıdaki komut dosyasını kullanın:

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
Artık hazırsınız [korumayı etkinleştir](https://docs.microsoft.com/en-us/azure/site-recovery/site-recovery-vmware-to-azure#step-6-replicate-applications) , sanal makineleriniz için.
