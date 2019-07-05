---
title: Bir ana VHD görüntüsü üzerinde - Azure Office yükleyin
description: Nasıl yükleyin ve azure'a bir Windows sanal masaüstü Önizleme ana görüntü Office özelleştirin.
services: virtual-desktop
author: ChJenk
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 05/02/2019
ms.author: v-chjenk
ms.openlocfilehash: 6d2bb7efdd5567da377e1e15fec4935b7d4a3a6f
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67444125"
---
# <a name="install-office-on-a-master-vhd-image"></a>Ana VHD görüntüsü üzerinde Office yükleme

Bu makalede, bir ana sanal sabit disk (VHD) görüntü yüklemek için Azure üzerinde Office 365 ProPlus, OneDrive ve diğer genel uygulamalara yükleme bildirir. Kullanıcılarınızın iş kolu (LOB) uygulamaları belirli bir satırı erişmeniz gerekiyorsa, bunları bu makaledeki yönergeleri tamamladıktan sonra yüklemeniz önerilir.

Bu makalede, bir sanal makine (VM) önceden oluşturduğunuz varsayılır. Aksi takdirde bkz [hazırlama ve bir ana VHD görüntüsünü özelleştirme](set-up-customize-master-image.md#create-a-vm)

Bu makalede Azure veya Hyper-V Yöneticisi'ni sağlanan olup olmadığını erişim sanal makinede, yükseltilmiş da varsayılır. Aksi takdirde bkz [tüm Azure abonelik ve Yönetim grupları yönetmek için erişimini yükseltme](https://docs.microsoft.com/azure/role-based-access-control/elevate-access-global-admin).

>[!NOTE]
>Kuruluşunuzun mevcut işlemleri ile kullanılabilecek bir Windows sanal masaüstü Önizleme özgü yapılandırma için bu yönergeleri yöneliktir.

## <a name="install-office-in-shared-computer-activation-mode"></a>Office paylaşılan bilgisayar etkinleştirme modunda yükleme

Paylaşılan bilgisayar etkinleştirmeye, Office 365 ProPlus kuruluşunuzdaki birden çok kullanıcı tarafından erişilen bir bilgisayara dağıtmanızı sağlar. Paylaşılan bilgisayar etkinleştirme hakkında daha fazla bilgi için bkz. [Office 365 ProPlus için paylaşılan bilgisayar etkinleştirmeye genel bakış](https://docs.microsoft.com/DeployOffice/overview-of-shared-computer-activation-for-office-365-proplus).

Kullanım [Office dağıtım aracı](https://www.microsoft.com/download/details.aspx?id=49117) Office yüklenecek. Windows 10 Enterprise çok oturum, yalnızca Office aşağıdaki sürümlerini destekler:
- Office 365 ProPlus
- Microsoft 365 iş aboneliğiyle birlikte gelen office 365 iş

Office dağıtım aracı, yapılandırma XML dosyası gerektirir. Aşağıdaki örnek özelleştirmek için bkz: [Office Dağıtım Aracı'nı yapılandırma seçenekleri](https://docs.microsoft.com/deployoffice/configuration-options-for-the-office-2016-deployment-tool).

Bu örnek yapılandırmanın sağladık XML şunları yapar:

- Office Insiders kanaldan yükleyin ve bunlar yürütme sırasında güncelleştirmeleri Insiders kanaldan sunun.
- Kullanım x64 mimari.
- Otomatik Güncelleştirmeler devre dışı bırakın.
- Visio ve Project yükleyin.
- Tüm mevcut Office yüklemesini kaldırın ve ayarlarını geçirme.
- Paylaşılan bilgisayar etkinleştirmeye olanak tanıyabilirsiniz.

>[!NOTE]
>Şablon arama özelliğini Visio'da Windows sanal masaüstü Önizleme yapılandırması sırasında çalışmaz.

Bu örnek yapılandırma XML neler olmaz aşağıda verilmiştir:

- İşletme için Skype'ı yükleyin
- OneDrive, kullanıcı başına modunda yükleyin. Daha fazla bilgi için bkz. [makine başına modunda yükleme OneDrive](#install-onedrive-in-per-machine-mode).

>[!NOTE]
>Paylaşılan bilgisayar etkinleştirmeye, Grup İlkesi nesneleri (GPO'lar) veya kayıt defteri ayarları ayarlanabilir. GPO şu konumdadır **Bilgisayar Yapılandırması\\ilkeleri\\Yönetim Şablonları\\Microsoft Office 2016 (makine)\\lisans ayarları**

Office dağıtım aracı setup.exe içerir. Office yüklemek için komut satırında aşağıdaki komutu çalıştırın:

```batch
Setup.exe /configure configuration.xml
```

#### <a name="sample-configurationxml"></a>Örnek configuration.xml

Aşağıdaki XML örneği Insiders sürümüne yüklemek.

```xml
<Configuration>
    <Add OfficeClientEdition="64" SourcePath="https://officecdn.microsoft.com/pr/5440fd1f-7ecb-4221-8110-145efaa6372f">
        <Product ID="O365ProPlusRetail">
            <Language ID="en-US" />
            <Language ID="MatchOS" Fallback = "en-US"/>
            <Language ID="MatchPreviousMSI" />
            <ExcludeApp ID="Groove" />
            <ExcludeApp ID="Lync" />
            <ExcludeApp ID="OneDrive" />
            <ExcludeApp ID="Teams" />
        </Product>
        <Product ID="VisioProRetail">
            <Language ID="en-US" />
            <Language ID="MatchOS" Fallback = "en-US"/>
            <Language ID="MatchPreviousMSI" />
            <ExcludeApp ID="Teams" /> 
        </Product>
        <Product ID="ProjectProRetail">
            <Language ID="en-US" />
            <Language ID="MatchOS" Fallback = "en-US"/>
            <Language ID="MatchPreviousMSI" />
            <ExcludeApp ID="Teams" />
        </Product>
    </Add>
    <RemoveMSI All="True" />
    <Updates Enabled="FALSE" UpdatePath="https://officecdn.microsoft.com/pr/5440fd1f-7ecb-4221-8110-145efaa6372f" />
    <Display Level="None" AcceptEULA="TRUE" />
    <Logging Level="Verbose" Path="%temp%\WVDOfficeInstall" />
    <Property Value="TRUE" Name="FORCEAPPSHUTDOWN"/>
    <Property Value="1" Name="SharedComputerLicensing"/>
    <Property Value="TRUE" Name="PinIconsToTaskbar"/>
</Configuration>
```

>[!NOTE]
>Office ekibi, 64 bitlik yüklemesi için kullanılmasını önerir **OfficeClientEdition** parametresi.

Office yükledikten sonra varsayılan Office davranışı güncelleştirebilirsiniz. Tek tek veya toplu iş dosyası davranışı güncelleştirmek için aşağıdaki komutları çalıştırın.

```batch
rem Mount the default user registry hive
reg load HKU\TempDefault C:\Users\Default\NTUSER.DAT
rem Must be executed with default registry hive mounted.
reg add HKU\TempDefault\SOFTWARE\Policies\Microsoft\office\16.0\common /v InsiderSlabBehavior /t REG_DWORD /d 2 /f
rem Set Outlook's Cached Exchange Mode behavior
rem Must be executed with default registry hive mounted.
reg add "HKU\TempDefault\software\policies\microsoft\office\16.0\outlook\cached mode" /v enable /t REG_DWORD /d 1 /f
reg add "HKU\TempDefault\software\policies\microsoft\office\16.0\outlook\cached mode" /v syncwindowsetting /t REG_DWORD /d 1 /f
reg add "HKU\TempDefault\software\policies\microsoft\office\16.0\outlook\cached mode" /v CalendarSyncWindowSetting /t REG_DWORD /d 1 /f
reg add "HKU\TempDefault\software\policies\microsoft\office\16.0\outlook\cached mode" /v CalendarSyncWindowSettingMonths  /t REG_DWORD /d 1 /f
rem Unmount the default user registry hive
reg unload HKU\TempDefault

rem Set the Office Update UI behavior.
reg add HKLM\SOFTWARE\Policies\Microsoft\office\16.0\common\officeupdate /v hideupdatenotifications /t REG_DWORD /d 1 /f
reg add HKLM\SOFTWARE\Policies\Microsoft\office\16.0\common\officeupdate /v hideenabledisableupdates /t REG_DWORD /d 1 /f
```

## <a name="install-onedrive-in-per-machine-mode"></a>OneDrive makine başına modunda yükleme

OneDrive, kullanıcı başına normalde yüklü değil. Bu ortamda olmalıdır makine başına yüklü.

OneDrive makine başına modunda yüklemek nasıl aşağıda verilmiştir:

1. İlk olarak OneDrive yükleyici hazırlanacağı bir konum oluşturun. Yerel diskteki bir klasörü veya [\\\\unc] (file://unc) konumdur ince ayar yapma.

2. Bu bağlantıyla hazırlanmış konumunuza OneDriveSetup.exe indirin: <https://aka.ms/OneDriveWVD-Installer>

3. Gt;(yok) office ile OneDrive yüklediyseniz  **\<ExcludeApp kimliği = "OneDrive" /\>** , tüm mevcut OneDrive kullanıcı başına yüklemeleri aşağıdakini çalıştırarak yükseltilmiş bir komut istemi'nden kaldırın. komut:
    
    ```batch
    "[staged location]\OneDriveSetup.exe" /uninstall
    ```

4. Bu komut ayarlamak için yükseltilmiş bir komut istemiyle çalıştırılabilen **AllUsersInstall** kayıt defteri değeri:

    ```batch
    REG ADD "HKLM\Software\Microsoft\OneDrive" /v "AllUsersInstall" /t REG_DWORD /d 1 /reg:64
    ```

5. OneDrive makine başına modunda yüklemek için şu komutu çalıştırın:

    ```batch
    Run "[staged location]\OneDriveSetup.exe" /allusers
    ```

6. Tüm kullanıcılar için oturum açma sırasında başlatmak için OneDrive'ı yapılandırmak için şu komutu çalıştırın:

    ```batch
    REG ADD "HKLM\Software\Microsoft\Windows\CurrentVersion\Run" /v OneDrive /t REG_SZ /d "C:\Program Files (x86)\Microsoft OneDrive\OneDrive.exe /background" /f
    ```

7. Etkinleştirme **sessizce kullanıcı hesabını yapılandıracak** aşağıdaki komutu çalıştırarak.

    ```batch
    REG ADD "HKLM\SOFTWARE\Policies\Microsoft\OneDrive" /v "SilentAccountConfig" /t REG_DWORD /d 1 /f
    ```

8. Yeniden yönlendirme ve OneDrive klasörüne aşağıdaki komutu çalıştırarak bilinen Windows taşıyın.

    ```batch
    REG ADD "HKLM\SOFTWARE\Policies\Microsoft\OneDrive" /v "KFMSilentOptIn" /t REG_SZ /d "<your-AzureAdTenantId>" /f
    ```

## <a name="teams-and-skype"></a>Takımlar ve Skype Kurumsal

Windows sanal masaüstü, iş ve takımlar için Skype desteklemiyor.

## <a name="next-steps"></a>Sonraki adımlar

Office görüntüsünü ekledikten sonra ana, VHD görüntüsünü özelleştirmek devam edebilirsiniz. Bkz: [hazırlama ve ana bir VHD görüntüsü özelleştirme](set-up-customize-master-image.md).
