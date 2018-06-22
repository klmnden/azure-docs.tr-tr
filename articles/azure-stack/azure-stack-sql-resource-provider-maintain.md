---
title: SQL kaynak sağlayıcısı Azure yığında koruma | Microsoft Docs
description: SQL kaynak sağlayıcısı hizmeti Azure yığında nasıl koruyabilirsiniz öğrenin.
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
ms.date: 06/20/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: ad899739dab1dc51d64368d2136ab87f73f6f3a0
ms.sourcegitcommit: ea5193f0729e85e2ddb11bb6d4516958510fd14c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2018
ms.locfileid: "36300919"
---
# <a name="sql-resource-provider-maintenance-operations"></a>SQL kaynak sağlayıcısı bakım işlemleri

SQL kaynak sağlayıcısı kilitli bir sanal makine üzerinde çalışır. Bakım işlemlerini etkinleştirmek için sanal makinenin güvenlik güncelleştirmesi gerekir. En az ayrıcalık ilkesini kullanarak bunu yapmak için kullanabileceğiniz [PowerShell yalnızca yetecek kadar Yönetim (JEA)](https://docs.microsoft.com/en-us/powershell/jea/overview) endpoint *DBAdapterMaintenance*. Kaynak Sağlayıcı yükleme paketi bu işlem için bir komut dosyası içerir.

## <a name="patching-and-updating"></a>Düzeltme eki uygulama ve güncelleştirme

Bir eklenti bileşeni olduğundan SQL kaynak sağlayıcısı Azure yığın bir parçası olarak bakım değil. Microsoft SQL kaynak sağlayıcı gerekli güncelleştirmeler sağlar. Güncelleştirilmiş SQL bağdaştırıcıyı yayımlandığında, bir komut dosyası güncelleştirmeyi uygulamak için sağlanır. Bu komut dosyasını yeni bir kaynak sağlayıcısı VM, eski sağlayıcısı VM durumu için yeni bir VM geçirme oluşturur. Daha fazla bilgi için bkz: [SQL kaynak sağlayıcısını güncelle](azure-stack-sql-resource-provider-update.md).

### <a name="provider-virtual-machine"></a>Sağlayıcı sanal makine

Kaynak sağlayıcısı çalıştığından bir *kullanıcı* sanal makine, serbest zaman güncelleştirmeleri ve gerekli düzeltme eklerini uygulamak vermeniz gerekir. VM güncelleştirmeleri uygulamak için düzeltme ve güncelleştirme döngüsü bir parçası olarak sağlanan Windows güncelleştirme paketleri kullanabilirsiniz.

## <a name="backuprestoredisaster-recovery"></a>Yedekleme/geri yükleme/olağanüstü durum kurtarma

 SQL kaynak sağlayıcısı, bir eklenti bileşeni olduğundan, bir Azure yığın iş sürekliliği olağanüstü durum kurtarma (BCDR) işleminin bir parçası yedeklenmediğini. Komut dosyaları için aşağıdaki işlemleri sağlanır:

- Yedekleme durumu bilgilerini (bir Azure yığın depolama hesabında depolanır.)
- Tam yığını kurtarma gerekliyse, kaynak sağlayıcısı geri yükleniyor.

>[!NOTE]
>Bir kurtarma yapmanız gerekiyorsa, kaynak sağlayıcısı geri yüklenmeden önce veritabanı sunucularının kurtarıldı gerekir.

## <a name="updating-sql-credentials"></a>SQL kimlik bilgileri güncelleştiriliyor

Oluşturma ve SQL Server sysadmin hesaplarında sürdürmek için sorumlu. Kaynak sağlayıcısının kullanıcıların veritabanlarını yönetmek için bu ayrıcalıklarına sahip bir hesap gerekir, ancak kullanıcıların verilere erişimi gerekmez. SQL Server sysadmin parolalarını güncellemeniz gerekiyorsa, depolanmış parolayı değiştirmek için kaynak sağlayıcısı yönetici arabirimi kullanabilirsiniz. Bu parolalar bir anahtar kasası, Azure yığın örneğinde depolanır.

Ayarları değiştirmek üzere seçin **Gözat** &gt; **yönetim KAYNAKLARININ** &gt; **SQL barındırma sunucuları** &gt; **SQL oturum açma bilgileri** ve bir kullanıcı adı seçin. Değişiklik SQL örneğinde önce yapılmalıdır (ve gerekirse, tüm çoğaltmalar.) Altında **ayarları**seçin **parola**.

![Yönetici parolasını güncelleştirin](./media/azure-stack-sql-rp-deploy/sqlrp-update-password.PNG)

## <a name="secrets-rotation"></a>Gizli anahtarları döndürme

*Bu yönergeler yalnızca Azure yığın tümleşik sistemleri sürüm 1804 ve sonrası için geçerlidir. Öncesi 1804 Azure yığın sürümleri parolaları döndürmek denemeyin.*

SQL ve MySQL kaynak sağlayıcıları ile Azure yığın kullanarak sistemleri tümleştirildiğinde aşağıdaki altyapı (dağıtım) gizlilikler döndürebilirsiniz:

- Dış SSL sertifikası [dağıtım sırasında sağladığınız](azure-stack-pki-certs.md).
- Dağıtım sırasında sağlanan kaynak sağlayıcısı VM yerel yönetici hesabı parolası.
- Kaynak sağlayıcı Tanılama kullanıcı (dbadapterdiag) parolası.

### <a name="powershell-examples-for-rotating-secrets"></a>Gizli anahtarları döndürme için PowerShell örnekleri

**Tüm gizli aynı anda değiştirin.**

```powershell
.\SecretRotationSQLProvider.ps1 `
    -Privilegedendpoint $Privilegedendpoint `
    -CloudAdminCredential $cloudCreds `
    -AzCredential $adminCreds `
    –DiagnosticsUserPassword $passwd `
    -DependencyFilesLocalPath $certPath `
    -DefaultSSLCertificatePassword $certPasswd  `
    -VMLocalCredential $localCreds
```

**Tanılama kullanıcı parolasını değiştirin.**

```powershell
.\SecretRotationSQLProvider.ps1 `
    -Privilegedendpoint $Privilegedendpoint `
    -CloudAdminCredential $cloudCreds `
    -AzCredential $adminCreds `
    –DiagnosticsUserPassword  $passwd
```

**VM yerel yönetici hesabının parolasını değiştirin.**

```powershell
.\SecretRotationSQLProvider.ps1 `
    -Privilegedendpoint $Privilegedendpoint `
    -CloudAdminCredential $cloudCreds `
    -AzCredential $adminCreds `
    -VMLocalCredential $localCreds
```

**SSL sertifikası parolayı değiştirin.**

```powershell
.\SecretRotationSQLProvider.ps1 `
    -Privilegedendpoint $Privilegedendpoint `
    -CloudAdminCredential $cloudCreds `
    -AzCredential $adminCreds `
    -DependencyFilesLocalPath $certPath `
    -DefaultSSLCertificatePassword $certPasswd
```

### <a name="secretrotationsqlproviderps1-parameters"></a>SecretRotationSQLProvider.ps1 parametreleri

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

**Sorunu**: gizli anahtarları döndürme günlükleri.<br>
Gizli döndürme özel komut dosyası çalışırken başarısız olursa gizli anahtarları döndürme için günlükleri otomatik olarak toplanan değil.

**Geçici çözüm**:<br>
C:\Logs kaydedilen AzureStack.DatabaseAdapter.SecretRotation.ps1_*.log dahil olmak üzere tüm kaynak sağlayıcısı günlükleri toplamak için Get-AzsDBAdapterLogs cmdlet'ini kullanın.

## <a name="update-the-virtual-machine-operating-system"></a>Sanal makine işletim sistemini güncelleştirmek

Sanal makine işletim sistemini güncelleştirmek için aşağıdaki yöntemlerden birini kullanın.

- Şu anda düzeltme eki yüklenmiş bir Windows Server 2016 Core görüntüsünü kullanarak en son kaynak sağlayıcısı paketini yükleyin.
- Bir Windows Update paketi yüklemesi sırasında yüklemek veya kaynak sağlayıcısı için güncelleştirin.

## <a name="update-the-virtual-machine-windows-defender-definitions"></a>Sanal makine Windows Defender tanımlarını güncelleştirme

Windows Defender tanımlarını güncelleştirmek için:

1. Windows Defender tanımlarını Update'ten indirme [Windows Defender tanım](https://www.microsoft.com/en-us/wdsi/definitions).

   Tanımlarını sayfa güncelleştirmesi, "El ile yükleyip tanımları" öğesine gidin. "Windows 10 ve Windows 8.1 için Windows Defender Antivirus" 64-bit dosyasını indirin.

   Alternatif olarak, kullanın [bu doğrudan bağlantı](https://go.microsoft.com/fwlink/?LinkID=121721&arch=x64) için yükleme/fpam fe.exe dosyasını çalıştırın.

2. SQL kaynak sağlayıcısı bağdaştırıcısı sanal makinenin bakım uç noktası için bir PowerShell oturumu oluşturun.

3. Bakım uç nokta oturumu kullanarak sanal makine tanımları güncelleştirme dosyasını kopyalayın.

4. Bakım PowerShell oturumunda çalıştırmak *güncelleştirme DBAdapterWindowsDefenderDefinitions* komutu.

5. Tanımları yükledikten sonra tanımları güncelleştirme dosyasını kullanarak silme öneririz *Kaldır ItemOnUserDrive* komutu.

**Tanımları güncelleştirmek için PowerShell komut dosyası örneği.**

Düzenle ve Defender tanımlarını güncelleştirmek için aşağıdaki betiği çalıştırın. Komut dosyası değerleri ortamınıza gelen değerlerle değiştirin.

```powershell
# Set credentials for local admin on the resource provider VM.
$vmLocalAdminPass = ConvertTo-SecureString "<local admin user password>" -AsPlainText -Force
$vmLocalAdminUser = "<local admin user name>"
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential `
    ($vmLocalAdminUser, $vmLocalAdminPass)

# Provide the public IP address for the adapter VM.
$databaseRPMachine  = "<RP VM IP address>"
$localPathToDefenderUpdate = "C:\DefenderUpdates\mpam-fe.exe"

# Download the Windows Defender update definitions file from https://www.microsoft.com/en-us/wdsi/definitions.
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
    {Update-AzSDBAdapterWindowsDefenderDefinition -DefinitionsUpdatePackageFile "User:\mpam-fe.exe"}
# Cleanup the definitions package file and session.
Invoke-Command -Session $session -ScriptBlock `
    {Remove-AzSItemOnUserDrive -ItemPath "User:\mpam-fe.exe"}
$session | Remove-PSSession
```

## <a name="collect-diagnostic-logs"></a>Tanılama günlüklerini toplayın

Kilitli sanal makineden günlükleri toplamak için PowerShell yalnızca yetecek kadar Yönetim (JEA) uç noktası kullanabilirsiniz *DBAdapterDiagnostics*. Bu uç noktaya aşağıdakileri sağlar:

- **Get-AzsDBAdapterLog**. Bu komut, kaynak sağlayıcısı tanılama günlüklerinin zip paketini oluşturur ve oturumunun kullanıcı sürücüde dosyası kaydeder. Hiçbir parametre olmadan bu komutu çalıştırabilirsiniz ve günlükleri son dört saatlik toplanır.
- **Remove-AzsDBAdapterLog**. Bu komut kaynak sağlayıcıdaki VM mevcut günlük paketleri kaldırır.

### <a name="endpoint-requirements-and-process"></a>Uç nokta gereksinimleri ve süreci

Bir kaynak sağlayıcısı yüklü veya güncelleştirildiğinde, **dbadapterdiag** kullanıcı hesabı oluşturulur. Bu hesap, tanılama günlüklerini toplamak için kullanacaksınız.

>[!NOTE]
>Dbadapterdiag hesap parolası bir sağlayıcı dağıtım veya güncelleştirme sırasında oluşturulan sanal makine üzerinde yerel yönetici için kullanılan parola ile aynıdır.

Kullanılacak *DBAdapterDiagnostics* komutlar, kaynak sağlayıcısı sanal makineye uzak PowerShell oturumu oluşturmak ve çalıştırmak **Get-AzsDBAdapterLog** komutu.

Kullanarak günlük toplama için zaman aralığını ayarlamanıza **FromDate** ve **ToDate** parametreleri. Birini veya her ikisini bu parametreleri belirtmezseniz, aşağıdaki varsayılanlar kullanılır:

- FromDate değeri, geçerli tarihten önce dört saattir.
- ToDate geçerli saattir.

**Günlüklerinin toplanması için PowerShell komut dosyası örneği.**

Aşağıdaki betik, kaynak Sağlayıcısı'ndan VM tanılama günlükleri toplamak gösterilmektedir.

```powershell
# Create a new diagnostics endpoint session.
$databaseRPMachineIP = '<RP VM IP address>'
$diagnosticsUserName = 'dbadapterdiag'
$diagnosticsUserPassword = '<Enter Diagnostic password>'

$diagCreds = New-Object System.Management.Automation.PSCredential `
        ($diagnosticsUserName, (ConvertTo-SecureString -String $diagnosticsUserPassword -AsPlainText -Force))
$session = New-PSSession -ComputerName $databaseRPMachineIP -Credential $diagCreds `
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

[Sunucuları barındıran SQL Server ekleyin](azure-stack-sql-resource-provider-hosting-servers.md)
