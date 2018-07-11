---
title: Azure Stack'te SQL kaynak sağlayıcısı bakımını yapma | Microsoft Docs
description: Azure Stack'te SQL kaynak sağlayıcısı hizmeti nasıl koruyabilirsiniz öğrenin.
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
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "36300919"
---
# <a name="sql-resource-provider-maintenance-operations"></a>SQL kaynak sağlayıcısı bakım işlemleri

SQL kaynak sağlayıcısı kilitli bir sanal makinede çalıştırır. Bakım işlemlerini etkinleştirmek için sanal makinenin güvenlik güncelleştirmesi gerekir. En düşük öncelik ilkesini kullanarak bunu yapmak için kullanabileceğiniz [PowerShell yeterli yönetim (JEA)](https://docs.microsoft.com/en-us/powershell/jea/overview) uç nokta *DBAdapterMaintenance*. Kaynak Sağlayıcı yükleme paketi, bu işlem için bir betik içerir.

## <a name="patching-and-updating"></a>Düzeltme eki uygulama ve güncelleştirme

SQL kaynak sağlayıcısı, Azure Stack bir parçası olarak bir eklenti bileşeni olduğundan hizmet değil. Microsoft SQL kaynak sağlayıcısı gerekli güncelleştirmeler sağlar. Güncelleştirilmiş SQL bağdaştırıcıyı yayınlandığı zaman, güncelleştirmeyi uygulamak için bir betik sağlanmıştır. Bu betik yeni bir kaynak sağlayıcısı eski sağlayıcısı VM durumunu yeni VM geçişi, VM oluşturur. Daha fazla bilgi için [SQL kaynak sağlayıcısını güncelle](azure-stack-sql-resource-provider-update.md).

### <a name="provider-virtual-machine"></a>Sağlayıcı sanal makine

Kaynak sağlayıcının çalıştığından bir *kullanıcı* sanal makinenin gereksinim yayınlanacak gerekli düzeltme eklerini ve güncelleştirmeleri uygulamak. VM için güncelleştirmeleri uygulamak için düzeltme ve güncelleştirme döngüsü bir parçası olarak sağlanan Windows güncelleştirme paketlerini kullanabilirsiniz.

## <a name="backuprestoredisaster-recovery"></a>Yedekleme/geri yükleme/olağanüstü durum kurtarma

 SQL kaynak sağlayıcısı, bir eklenti bileşeni olduğundan, bir Azure Stack iş sürekliliği olağanüstü durum kurtarma (BCDR) işleminin bir parçası yedeklenmediğini. Şu işlemler için betikler sağlanacaktır:

- Yedekleme durumu bilgilerini (bir Azure Stack depolama hesabında depolanır.)
- Kaynak sağlayıcısı, tam yığın kurtarma gerekiyorsa geri yükleniyor.

>[!NOTE]
>Bir kurtarma yapmanız gerekiyorsa, kaynak sağlayıcısı geri yüklenmeden önce veritabanı sunucularının kurtarılmaları gerekir.

## <a name="updating-sql-credentials"></a>SQL kimlik bilgileri güncelleştiriliyor

Oluşturma ve SQL Server sysadmin hesapları korumak için sorumlu olursunuz. Kaynak sağlayıcısı kullanıcıların veritabanlarını yönetmek için bu ayrıcalıklarına sahip bir hesap gerekir, ancak kullanıcıların verilere erişim gerektirmez. SQL Server sysadmin parolaları güncelleştirmeniz gerekiyorsa, depolanan bir parolayı değiştirmek için kaynak sağlayıcısının Yöneticisi arabirimini kullanabilirsiniz. Bu parolalar, Azure Stack örneğinde bir anahtar Kasası'nda depolanır.

Ayarları değiştirmek için seçin **Gözat** &gt; **yönetim kaynakları** &gt; **SQL barındırma sunucuları** &gt; **SQL oturum açmaları** ve bir kullanıcı adı seçin. Değişiklik SQL örneğinde önce yapılmalıdır (ve gerekirse tüm çoğaltmalar.) Altında **ayarları**seçin **parola**.

![Yönetici parolasını güncelleştirme](./media/azure-stack-sql-rp-deploy/sqlrp-update-password.PNG)

## <a name="secrets-rotation"></a>Gizli anahtarları döndürme

*Bu yönergeler yalnızca Azure Stack tümleşik sistemleri sürüm 1804 ve sonrası için geçerlidir. Gizli dizileri öncesi 1804 Azure Stack sürümlerinde döndürme çalışmayın.*

SQL ve MySQL kaynak sağlayıcılarını kullanarak Azure Stack ile tümleştirilmiş sistemlerle, aşağıdaki altyapı (dağıtım) gizli dizileri döndürebilirsiniz:

- Dış SSL sertifikası [dağıtım sırasında sağlanan](azure-stack-pki-certs.md).
- Dağıtım sırasında sağlanan kaynak sağlayıcısı VM yerel yönetici hesabının parolası.
- Kaynak sağlayıcısı (dbadapterdiag) Tanılama kullanıcı parolası.

### <a name="powershell-examples-for-rotating-secrets"></a>Gizli dizileri döndürme için PowerShell örnekleri

**Tüm gizli dizileri aynı anda değiştirin.**

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

**SSL sertifika parolası değiştirin.**

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
|AzCredential|Azure Stack Hizmet yöneticisi hesabı kimlik bilgisi.|
|CloudAdminCredential|Azure Stack bulut yönetici etki alanı hesabı kimlik bilgisi.|
|PrivilegedEndpoint|Get-AzureStackStampInformation erişmek için ayrıcalıklı uç noktası'ı seçin.|
|DiagnosticsUserPassword|Tanılama kullanıcı hesabı parolası.|
|VMLocalCredential|MySQLAdapter VM'deki yerel yönetici hesabı.|
|DefaultSSLCertificatePassword|Varsayılan SSL sertifikası (* pfx) parola.|
|DependencyFilesLocalPath|Bağımlılık dosyaların yerel yolu.|
|     |     |

### <a name="known-issues"></a>Bilinen sorunlar

**Sorunu**: gizli anahtarları döndürme günlükleri.<br>
Gizli döndürme özel betik çalıştırıldığında başarısız olursa gizli anahtarları döndürme için günlükleri otomatik olarak toplanan değildir.

**Geçici çözüm**:<br>
İçinde C:\Logs kaydedilmiş AzureStack.DatabaseAdapter.SecretRotation.ps1_*.log, dahil olmak üzere tüm kaynak sağlayıcısı günlükleri toplamak için Get-AzsDBAdapterLogs cmdlet'ini kullanın.

## <a name="update-the-virtual-machine-operating-system"></a>Sanal makine işletim sistemi güncelleştirme

Sanal makine işletim sistemini güncelleştirmek için aşağıdaki yöntemlerden birini kullanın.

- Şu anda Yamalı bir Windows Server 2016 Core görüntü kullanarak en son kaynak sağlayıcısı paketini yükleyin.
- Windows güncelleştirme paketi yüklemesi sırasında yükleyebilir veya için kaynak sağlayıcısını güncelle.

## <a name="update-the-virtual-machine-windows-defender-definitions"></a>Sanal makine Windows Defender tanımlarını güncelleştir

Windows Defender tanımlarına güncelleştirmek için:

1. Windows Defender tanımlarına Update'ten indirme [Windows Defender tanım](https://www.microsoft.com/en-us/wdsi/definitions).

   Tanımlarını sayfa güncelleştirmesi, "El ile indirin ve tanımları Yükle" öğesine gidin. "Windows 10 ve Windows 8.1 için Windows Defender Antivirus" 64-bit dosyasını indirin.

   Alternatif olarak, [bu doğrudan bağlantı](https://go.microsoft.com/fwlink/?LinkID=121721&arch=x64) fpam fe.exe dosya indirme/çalıştırma için.

2. SQL kaynak sağlayıcısı bağdaştırıcısını sanal makinenin bakım uç noktası için bir PowerShell oturumu oluşturun.

3. Bakım bitiş noktası oturumu kullanarak sanal makineye tanımları güncelleştirme dosyasını kopyalayın.

4. Bakım PowerShell oturumunda çalıştırın *güncelleştirme DBAdapterWindowsDefenderDefinitions* komutu.

5. Tanımları yükledikten sonra tanımları güncelleştirme dosyasını kullanarak silmenizi öneririz *Remove-ItemOnUserDrive* komutu.

**Tanımları güncelleştirmek için PowerShell Betiği örneği.**

Düzenle ve Defender tanımları güncelleştirmek için aşağıdaki betiği çalıştırın. Betik değerleri, ortamınızdaki değerlerle değiştirin.

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

## <a name="collect-diagnostic-logs"></a>Tanılama günlükleri toplama

Kilitli sanal makineden günlükleri toplamak için PowerShell yeterli yönetim (JEA) uç noktası kullanabilirsiniz *DBAdapterDiagnostics*. Bu uç nokta, aşağıdaki komutları sağlar:

- **Get-AzsDBAdapterLog**. Bu komut, kaynak sağlayıcısı tanılama günlükleri zip paketini oluşturur ve dosyayı oturumunun kullanıcı sürücüde kaydeder. Bu komut hiçbir parametre olmadan çalıştırabilirsiniz ve günlüklerin son dört saat toplanır.
- **Remove-AzsDBAdapterLog**. Bu komut, VM kaynak sağlayıcısındaki mevcut günlük paketlerini kaldırır.

### <a name="endpoint-requirements-and-process"></a>Uç nokta gereksinimleri ve süreci

Bir kaynak sağlayıcısı yüklü veya güncelleştirildiğinde, **dbadapterdiag** kullanıcı hesabı oluşturulur. Bu hesap, tanılama günlüklerini toplamak için kullanacaksınız.

>[!NOTE]
>Dbadapterdiag hesap parolası sağlayıcısı dağıtım veya güncelleştirme sırasında oluşturulan sanal makinede yerel yönetici için kullanılan parola ile aynıdır.

Kullanılacak *DBAdapterDiagnostics* komutlar, kaynak sağlayıcısı sanal makineye bir uzak PowerShell oturumu oluşturmak ve çalıştırmak **Get-AzsDBAdapterLog** komutu.

Kullanarak günlük toplama için zaman aralığını ayarlamanıza **FromDate** ve **ToDate** parametreleri. Birini veya ikisini de bu parametre belirtmezseniz, aşağıdaki varsayılan değerler kullanılır:

- FromDate dört saat geçerli saatten önce olur.
- ToDate geçerli süredir.

**Günlüklerinin toplanması için PowerShell Betiği örneği.**

Aşağıdaki betik, kaynak Sağlayıcısı'ndan VM tanılama günlük toplama işlemi gösterilmektedir.

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

[SQL Server barındırma sunucuları ekleme](azure-stack-sql-resource-provider-hosting-servers.md)
