---
title: MySQL kaynak sağlayıcı Azure yığında koruma | Microsoft Docs
description: MySQL kaynak sağlayıcısı hizmeti Azure yığında nasıl koruyabilirsiniz öğrenin.
services: azure-stack
documentationCenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: a1fadd0cfdb38452e86cfce643bebbfd746e8643
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37085801"
---
# <a name="mysql-resource-provider-maintenance-operations"></a>MySQL kaynak sağlayıcı bakım işlemleri

MySQL kaynak sağlayıcı kilitli bir sanal makine üzerinde çalışır. Bakım işlemlerini etkinleştirmek için sanal makinenin güvenlik güncelleştirmesi gerekir. En az ayrıcalık ilkesini kullanarak bunu için PowerShell yalnızca yetecek kadar Yönetim (JEA) uç noktası DBAdapterMaintenance kullanabilirsiniz. Kaynak Sağlayıcı yükleme paketi bu işlem için bir komut dosyası içerir.

## <a name="update-the-virtual-machine-operating-system"></a>Sanal makine işletim sistemini güncelleştirmek

Kaynak sağlayıcısı çalıştığından bir *kullanıcı* sanal makine, serbest zaman güncelleştirmeleri ve gerekli düzeltme eklerini uygulamak vermeniz gerekir. VM güncelleştirmeleri uygulamak için düzeltme ve güncelleştirme döngüsü bir parçası olarak sağlanan Windows güncelleştirme paketleri kullanabilirsiniz.

Aşağıdaki yöntemlerden birini kullanarak sağlayıcısı sanal makine güncelleştirmesi:

- Şu anda düzeltme eki yüklenmiş bir Windows Server 2016 Core görüntüsünü kullanarak en son kaynak sağlayıcısı paketini yükleyin.
- Bir Windows Update paketi yüklemesi sırasında yüklemek veya kaynak sağlayıcıya güncelleştirin.

## <a name="update-the-virtual-machine-windows-defender-definitions"></a>Sanal makine Windows Defender tanımlarını güncelleştirme

Defender tanımlarını güncelleştirmek için aşağıdaki adımları izleyin:

1. Windows Defender tanımlarını Update'ten indirme [Windows Defender tanım](https://www.microsoft.com/en-us/wdsi/definitions).

    Tanımları sayfasında "El ile yükleyip tanımları aşağıya doğru" kaydırın. "Windows 10 ve Windows 8.1 için Windows Defender Antivirus" 64-bit dosyasını indirin.

    Alternatif olarak, kullanın [bu doğrudan bağlantı](https://go.microsoft.com/fwlink/?LinkID=121721&arch=x64) için yükleme/fpam fe.exe dosyasını çalıştırın.

2. MySQL kaynak sağlayıcı bağdaştırıcısı sanal makinenin bakım uç noktası için bir PowerShell oturumu açın.

3. Kaynak sağlayıcı bağdaştırıcıya bakım uç nokta oturumu kullanarak VM tanımları güncelleştirme dosyasını kopyalayın.

4. Bakım PowerShell oturumunda çalıştırmak _güncelleştirme DBAdapterWindowsDefenderDefinitions_ komutu.

5. Tanımları yükledikten sonra tanımları güncelleştirme dosyasını kullanarak silme öneririz _Kaldır ItemOnUserDrive)_ komutu.

**Tanımları güncelleştirmek için PowerShell komut dosyası örneği.**

Düzenle ve Defender tanımlarını güncelleştirmek için aşağıdaki betiği çalıştırın. Komut dosyası değerleri ortamınıza gelen değerlerle değiştirin.

```powershell
# Set credentials for the local admin on the resource provider VM.
$vmLocalAdminPass = ConvertTo-SecureString "<local admin user password>" -AsPlainText -Force
$vmLocalAdminUser = "<local admin user name>"
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential `
    ($vmLocalAdminUser, $vmLocalAdminPass)

# Provide the public IP address for the adapter VM.
$databaseRPMachine  = "<RP VM IP address>"
$localPathToDefenderUpdate = "C:\DefenderUpdates\mpam-fe.exe"

# Download Windows Defender update definitions file from https://www.microsoft.com/en-us/wdsi/definitions.  
Invoke-WebRequest -Uri 'https://go.microsoft.com/fwlink/?LinkID=121721&arch=x64' `
    -Outfile $localPathToDefenderUpdate  

# Create a session to the maintenance endpoint.
$session = New-PSSession -ComputerName $databaseRPMachine `
    -Credential $vmLocalAdminCreds -ConfigurationName DBAdapterMaintenance

# Copy the defender update file to the adapter virtual machine.
Copy-Item -ToSession $session -Path $localPathToDefenderUpdate `
     -Destination "User:\"

# Install the update definitions.
Invoke-Command -Session $session -ScriptBlock `
    {Update-AzSDBAdapterWindowsDefenderDefinition -DefinitionsUpdatePackageFile "User:\"}

# Cleanup the definitions package file and session.
Invoke-Command -Session $session -ScriptBlock `
    {Remove-AzSItemOnUserDrive -ItemPath "User:\"}
$session | Remove-PSSession

```

## <a name="secrets-rotation"></a>Gizli anahtarları döndürme

*Bu yönergeler yalnızca Azure yığın tümleşik sistemleri sürüm 1804 ve sonrası için geçerlidir. Gizli anahtarları Azure yığın öncesi 1804 sürümlerinde döndürmek denemeyin.*

SQL ve MySQL kaynak sağlayıcıları ile Azure yığın kullanarak sistemleri tümleştirildiğinde aşağıdaki altyapı (dağıtım) gizlilikler döndürebilirsiniz:

- Dış SSL sertifikası [dağıtım sırasında sağladığınız](azure-stack-pki-certs.md).
- Dağıtım sırasında sağlanan kaynak sağlayıcısı VM yerel yönetici hesabı parolası.
- Kaynak sağlayıcı Tanılama kullanıcı (dbadapterdiag) parolası.

### <a name="powershell-examples-for-rotating-secrets"></a>Gizli anahtarları döndürme için PowerShell örnekleri

**Tüm gizli aynı anda değiştirin.**

```powershell
.\SecretRotationMySQLProvider.ps1 `
    -Privilegedendpoint $Privilegedendpoint `
    -CloudAdminCredential $cloudCreds `
    -AzCredential $adminCreds `
    –DiagnosticsUserPassword $passwd `
    -DependencyFilesLocalPath $certPath `
    -DefaultSSLCertificatePassword $certPasswd `  
    -VMLocalCredential $localCreds

```

**Tanılama kullanıcı parolasını değiştirin.**

```powershell
.\SecretRotationMySQLProvider.ps1 `
    -Privilegedendpoint $Privilegedendpoint `
    -CloudAdminCredential $cloudCreds `
    -AzCredential $adminCreds `
    –DiagnosticsUserPassword  $passwd

```

**VM yerel yönetici hesabının parolasını değiştirin.**

```powershell
.\SecretRotationMySQLProvider.ps1 `
    -Privilegedendpoint $Privilegedendpoint `
    -CloudAdminCredential $cloudCreds `
    -AzCredential $adminCreds `
    -VMLocalCredential $localCreds

```

**SSL sertifikası parolayı değiştirin.**

```powershell
.\SecretRotationMySQLProvider.ps1 `
    -Privilegedendpoint $Privilegedendpoint `
    -CloudAdminCredential $cloudCreds `
    -AzCredential $adminCreds `
    -DependencyFilesLocalPath $certPath `
    -DefaultSSLCertificatePassword $certPasswd

```

### <a name="secretrotationmysqlproviderps1-parameters"></a>SecretRotationMySQLProvider.ps1 parametreleri

|Parametre|Açıklama|
|-----|-----|
|AzCredential|Azure yığın hizmet yönetici hesabı kimlik bilgileri.|
|CloudAdminCredential|Azure yığın bulut yönetici etki alanı hesabı kimlik bilgileri.|
|PrivilegedEndpoint|Get-AzureStackStampInformation erişmek için ayrıcalıklı uç noktası'ı seçin.|
|DiagnosticsUserPassword|Tanılama kullanıcı hesabı parolası.|
|VMLocalCredential|MySQLAdapter VM üzerinde yerel yönetici hesabı.|
|DefaultSSLCertificatePassword|Varsayılan SSL sertifikası (* pfx) parola.|
|DependencyFilesLocalPath|Bağımlılık dosyalarını yerel yolu.|
|     |     |

### <a name="known-issues"></a>Bilinen sorunlar

**Sorun:**<br>
Gizli döndürme betik çalışırken başarısız olursa gizli anahtarları döndürme için günlükleri otomatik olarak toplanan değil.

**Geçici çözüm:**<br>
C:\Logs kaydedilen AzureStack.DatabaseAdapter.SecretRotation.ps1_*.log dahil olmak üzere tüm kaynak sağlayıcısı günlükleri toplamak için Get-AzsDBAdapterLogs cmdlet'ini kullanın.

## <a name="collect-diagnostic-logs"></a>Tanılama günlüklerini toplayın

Kilitli sanal makineden günlükleri toplamak için PowerShell yalnızca yetecek kadar Yönetim (JEA) uç noktası DBAdapterDiagnostics kullanabilirsiniz. Bu uç noktaya aşağıdakileri sağlar:

- **Get-AzsDBAdapterLog**. Bu komut, kaynak sağlayıcısı tanılama günlüklerinin zip paketini oluşturur ve oturumunun kullanıcı sürücüde dosyası kaydeder. Hiçbir parametre olmadan bu komutu çalıştırabilirsiniz ve günlükleri son dört saatlik toplanır.

- **Remove-AzsDBAdapterLog**. Bu komut kaynak sağlayıcıdaki VM mevcut günlük paketleri kaldırır.

### <a name="endpoint-requirements-and-process"></a>Uç nokta gereksinimleri ve süreci

Bir kaynak sağlayıcısı yüklü veya güncelleştirildiğinde dbadapterdiag kullanıcı hesabı oluşturulur. Bu hesap, tanılama günlüklerini toplamak için kullanacaksınız.

>[!NOTE]
>Dbadapterdiag hesap parolası bir sağlayıcı dağıtım veya güncelleştirme sırasında oluşturulan sanal makine üzerinde yerel yönetici için kullanılan parola ile aynıdır.

Kullanılacak _DBAdapterDiagnostics_ komutlar, kaynak sağlayıcısı sanal makineye uzak PowerShell oturumu oluşturmak ve çalıştırmak **Get-AzsDBAdapterLog** komutu.

Kullanarak günlük toplama için zaman aralığını ayarlamanıza **FromDate** ve **ToDate** parametreleri. Birini veya her ikisini bu parametreleri belirtmezseniz, aşağıdaki varsayılanlar kullanılır:

* FromDate değeri, geçerli tarihten önce dört saattir.
* ToDate geçerli saattir.

**Günlüklerinin toplanması için PowerShell komut dosyası örneği.**

Aşağıdaki betik, kaynak Sağlayıcısı'ndan VM tanılama günlükleri toplamak gösterilmektedir.

```powershell
# Create a new diagnostics endpoint session.
$databaseRPMachineIP = '<RP VM IP address>'
$diagnosticsUserName = 'dbadapterdiag'
$diagnosticsUserPassword = '<Enter Diagnostic password>'
$diagCreds = New-Object System.Management.Automation.PSCredential `
        ($diagnosticsUserName, (ConvertTo-SecureString -String $diagnosticsUserPassword -AsPlainText -Force))
$session = New-PSSession -ComputerName $databaseRPMachineIP -Credential $diagCreds
        -ConfigurationName DBAdapterDiagnostics

# Sample that captures logs from the previous hour.
$fromDate = (Get-Date).AddHours(-1)
$dateNow = Get-Date
$sb = {param($d1,$d2) Get-AzSDBAdapterLog -FromDate $d1 -ToDate $d2}
$logs = Invoke-Command -Session $session -ScriptBlock $sb -ArgumentList $fromDate,$dateNow

# Copy the logs to the user drive.
$sourcePath = "User:\{0}" -f $logs
$destinationPackage = Join-Path -Path (Convert-Path '.') -ChildPath $logs
Copy-Item -FromSession $session -Path $sourcePath -Destination $destinationPackage

# Cleanup the logs.
$cleanup = Invoke-Command -Session $session -ScriptBlock {Remove- AzsDBAdapterLog }
# Close the session.
$session | Remove-PSSession

```

## <a name="next-steps"></a>Sonraki adımlar

[MySQL kaynak sağlayıcısını kaldırma](azure-stack-mysql-resource-provider-remove.md)
