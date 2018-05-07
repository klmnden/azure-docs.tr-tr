---
title: SQL veritabanları Azure yığında kullanarak | Microsoft Docs
description: SQL veritabanları Azure yığını ve hızlı adımlar SQL Server Kaynak sağlayıcısı bağdaştırıcısı dağıtmak için bir hizmet olarak nasıl dağıtabileceğini öğrenin.
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
ms.date: 05/01/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: 53436d131672622ae1a72a1bb84d5aa83fdbdc0c
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/04/2018
---
# <a name="maintenance-operations"></a>Bakım işlemleri 
SQL kaynak kilitli bir sanal makineyi sağlayıcıdır. Kaynak sağlayıcısı sanal makinenin güvenlik güncelleştirme PowerShell yalnızca yetecek kadar Yönetim (JEA) uç noktası aracılığıyla yapılabilir _DBAdapterMaintenance_. Bir betik bu işlemleri kolaylaştırmak için RP'ın yükleme paketi ile birlikte sağlanır.

## <a name="patching-and-updating"></a>Düzeltme eki uygulama ve güncelleştirme
Bir eklenti bileşeni olduğu gibi SQL kaynak sağlayıcısı Azure yığın bir parçası olarak bakım değil. Microsoft, gerektiği gibi SQL kaynak sağlayıcısı için güncelleştirmeleri sağlarken. Üzerinde SQL kaynak sağlayıcısı örneği bir _kullanıcı_ varsayılan sağlayıcı abonelik altında sanal makine. Bu nedenle, Windows düzeltme ekleri, virüsten koruma imzaları vb. sağlamak gereklidir. Windows güncelleştirme, düzeltme ve güncelleştirme döngüsünün parçası Windows VM güncelleştirmeleri uygulamak için kullanılabilir olarak sağlanan paketleri. Güncelleştirilmiş bir bağdaştırıcı yayımlandığında, bir komut dosyası güncelleştirmeyi uygulamak için sağlanır. Bu komut dosyasını yeni bir RP VM oluşturur ve zaten herhangi bir durum geçirme.

 ## <a name="backuprestoredisaster-recovery"></a>Yedekleme/geri yükleme/olağanüstü durum kurtarma
 Bir eklenti bileşeni olduğu gibi SQL kaynak sağlayıcısı Azure yığın BC-DR işleminin bir parçası yedeklenmez. Komut dosyaları kolaylaştırmak için sağlanır:
- (Bir Azure yığın depolama hesabında depolanır) gerekli durum bilgisinin yedekleme
- Tam yığını kurtarma gerekli hale gelmesi durumunda RP geri yükleniyor.
Veritabanı sunucuları ilk (gerekirse) kurtarılması gerekiyor, önce kaynak sağlayıcısı geri yüklenir.

## <a name="updating-sql-credentials"></a>SQL kimlik bilgileri güncelleştiriliyor
Oluşturma ve SQL Server sistem yönetici hesaplarında korumaya yönelik sorumluluğu size aittir. Kaynak sağlayıcısının veritabanlarını kullanıcılar adına yönetmek için bu ayrıcalıklarına sahip bir hesap gerekir - bu veritabanları veri erişimi gerekmez. SQL Server sa parolalarını güncellemeniz gerekiyorsa, kaynak sağlayıcı tarafından kullanılan depolanan parolayı değiştirmek için kaynak sağlayıcısının Yöneticisi arabirimini güncelleştirme özelliğini kullanabilirsiniz. Bu parolalar bir anahtar kasası, Azure yığın örneğinde depolanır.

Ayarları değiştirmek için tıklatın **Gözat** &gt; **yönetim KAYNAKLARININ** &gt; **SQL barındırma sunucuları** &gt; **SQL oturum açma bilgileri** ve bir oturum açma adı seçin. Değişiklik SQL örneğinde önce yapılmalıdır (ve gerekirse, tüm yinelemeleri). İçinde **ayarları** paneli, tıklayın **parola**.

![Yönetici parolasını güncelleştirin](./media/azure-stack-sql-rp-deploy/sqlrp-update-password.PNG)

## <a name="update-the-virtual-machine-operating-system"></a>Sanal makine işletim sistemini güncelleştirmek
Windows Server VM güncelleştirmek için birkaç yolu vardır:
* Şu anda düzeltme eki yüklenmiş bir Windows Server 2016 Core görüntüsünü kullanarak en son kaynak sağlayıcısı paketini yükle
* RP güncelleştirilmesini veya yükleme sırasında Windows Update paket yükleme

## <a name="update-the-virtual-machine-windows-defender-definitions"></a>Sanal makine Windows Defender tanımlarını güncelleştirme
Defender tanımlarını güncelleştirmek için aşağıdaki adımları izleyin:

1. Windows Defender tanımlarını Update'ten indirme [Windows Defender tanım](https://www.microsoft.com/en-us/wdsi/definitions)

    Bu sayfada "El ile yükleyip tanımları altında" indir "Windows 10 ve Windows 8.1" 64-bit dosya için Windows Defender Antivirus. 
    
    Doğrudan bağlantı: https://go.microsoft.com/fwlink/?LinkID=121721&arch=x64

2. Bir PowerShell oturumuna SQL RP bağdaştırıcısı sanal makinenin bakım uç noktası oluşturma
3. Bakım uç nokta oturumu kullanarak DB bağdaştırıcısı makineye tanımları güncelleştirme dosyasını kopyalayın
4. Bakım PowerShell oturumu çağırma _güncelleştirme DBAdapterWindowsDefenderDefinitions_ komutu
5. Yüklemeden sonra kullanılan tanımları güncelleştirme dosyası kaldırmak için önerilir. Bakım kullanarak oturum kaldırılabilir _Kaldır ItemOnUserDrive)_ komutu.


(Adresi veya gerçek değer ile sanal makine adını değiştirin) Defender tanımlarını güncelleştirmek için örnek bir betik verilmiştir:

```powershell
# Set credentials for the diagnostic user
$diagPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$diagCreds = New-Object System.Management.Automation.PSCredential `
    ("dbadapterdiag", $vmLocalAdminPass)$diagCreds = Get-Credential

# Public IP Address of the DB adapter machine
$databaseRPMachine  = "XX.XX.XX.XX"
$localPathToDefenderUpdate = "C:\DefenderUpdates\mpam-fe.exe"
 
# Download Windows Defender update definitions file from https://www.microsoft.com/en-us/wdsi/definitions. 
Invoke-WebRequest -Uri https://go.microsoft.com/fwlink/?LinkID=121721&arch=x64 `
    -Outfile $localPathToDefenderUpdate 

# Create session to the maintenance endpoint
$session = New-PSSession -ComputerName $databaseRPMachine `
    -Credential $diagCreds -ConfigurationName DBAdapterMaintenance
# Copy defender update file to the db adapter machine
Copy-Item -ToSession $session -Path $localPathToDefenderUpdate `
     -Destination "User:\mpam-fe.exe"
# Install the update file
Invoke-Command -Session $session -ScriptBlock `
    {Update-AzSDBAdapterWindowsDefenderDefinitions -DefinitionsUpdatePackageFile "User:\mpam-fe.exe"}
# Cleanup the definitions package file and session
Invoke-Command -Session $session -ScriptBlock `
    {Remove-AzSItemOnUserDrive -ItemPath "User:\mpam-fe.exe"}
$session | Remove-PSSession
```


## <a name="collect-diagnostic-logs"></a>Tanılama günlüklerini toplayın
SQL kaynak kilitli bir sanal makineyi sağlayıcıdır. Bu sanal makineden bir PowerShell yalnızca yetecek kadar Yönetim (JEA) uç noktası günlükleri toplamak için gerekli olur _DBAdapterDiagnostics_ bu amaç için sağlanır. Bu uç noktası aracılığıyla kullanılabilen iki komut vardır:

* Get-AzsDBAdapterLog - RP tanılama günlükleri içeren bir zip paketini hazırlar ve oturum kullanıcı sürücüde yerleştirir. Komut parametresiz çağrılabilir ve günlükleri son dört saatlik toplar.
* Remove-AzsDBAdapterLog - kaynak sağlayıcısı VM mevcut günlük paketlerini temizler

Bir kullanıcı hesabı olarak adlandırılan _dbadapterdiag_ RP dağıtım veya RP günlükleri ayıklanacağı tanılama uç noktasına bağlanmak için güncelleştirme işlemi sırasında oluşturulur. Bu hesabın parolasını dağıtım/güncelleştirme sırasında yerel yönetici hesabı için girilen parola ile aynıdır.

Bu komutları kullanmak için kaynak sağlayıcısı sanal makineye uzak PowerShell oturumu oluşturmak ve komut çağırma gerekecektir. İsteğe bağlı olarak FromDate ve ToDate parametreleri sağlayabilir. Aşağıdakilerden birini veya bunların her ikisi de belirtmezseniz, geçerli tarihten önce dört saat FromDate olacaktır ve ToDate geçerli saati olacaktır.

Bu örnek betik, bu komutları kullanımını göstermektedir:

```powershell
# Create a new diagnostics endpoint session.
$databaseRPMachineIP = '<RP VM IP>'
$diagnosticsUserName = 'dbadapterdiag'
$diagnosticsUserPassword = '<see above>'

$diagCreds = New-Object System.Management.Automation.PSCredential `
        ($diagnosticsUserName, $diagnosticsUserPassword)
$session = New-PSSession -ComputerName $databaseRPMachineIP -Credential $diagCreds `
        -ConfigurationName DBAdapterDiagnostics

# Sample captures logs from the previous one hour
$fromDate = (Get-Date).AddHours(-1)
$dateNow = Get-Date
$sb = {param($d1,$d2) Get-AzSDBAdapterLog -FromDate $d1 -ToDate $d2}
$logs = Invoke-Command -Session $session -ScriptBlock $sb -ArgumentList $fromDate,$dateNow

# Copy the logs
$sourcePath = "User:\{0}" -f $logs
$destinationPackage = Join-Path -Path (Convert-Path '.') -ChildPath $logs
Copy-Item -FromSession $session -Path $sourcePath -Destination $destinationPackage

# Cleanup logs
$cleanup = Invoke-Command -Session $session -ScriptBlock {Remove- AzsDBAdapterLog }
# Close the session
$session | Remove-PSSession
```

## <a name="next-steps"></a>Sonraki adımlar
[Sunucuları barındıran SQL Server ekleyin](azure-stack-sql-resource-provider-hosting-servers.md)
