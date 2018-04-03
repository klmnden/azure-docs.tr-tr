---
title: SQL veritabanları Azure yığında kullanarak | Microsoft Docs
description: SQL veritabanları Azure yığını ve hızlı adımlar SQL Server Kaynak sağlayıcısı bağdaştırıcısı dağıtmak için bir hizmet olarak nasıl dağıtabileceğini öğrenin.
services: azure-stack
documentationCenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2018
ms.author: mabrigg
ms.reviewer: jeffgo
ms.openlocfilehash: d0b287eb61087e90c898aad5273ab5be8c1f98b2
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="use-sql-databases-on-microsoft-azure-stack"></a>Microsoft Azure yığın üzerinde SQL veritabanları kullanın

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

SQL veritabanları hizmet olarak kullanıma sunmak için SQL Server Kaynak sağlayıcısı bağdaştırıcısını kullanmak [Azure yığın](azure-stack-poc.md). Kaynak sağlayıcısını yüklemek ve bir veya daha fazla SQL Server örneklerine bağlamak sonra siz ve kullanıcılarınız oluşturabilirsiniz:
- Veritabanları bulut yerel uygulamalar için.
- SQL tabanlı Web siteleri.
- SQL tabanlı iş yükleri.
Bir sanal makine (VM) barındıran SQL Server her zaman sağlamak zorunda değilsiniz.

Kaynak sağlayıcısı tüm veritabanı yönetim özelliklerini desteklemez [Azure SQL veritabanı](https://azure.microsoft.com/services/sql-database/). Örneğin, esnek veritabanı havuzları ve veritabanı performansını yukarı ve aşağı otomatik arama özelliği kullanılabilir değil. Ancak, kaynak sağlayıcısı yoksa desteği benzer oluşturma, okuma, güncelleştirme ve Sil (CRUD) işlemleridir. API SQL veritabanı ile uyumlu değil.

## <a name="sql-resource-provider-adapter-architecture"></a>SQL kaynak sağlayıcı bağdaştırıcısı mimarisi
Kaynak sağlayıcısı üç bileşenden oluşur:

- **SQL kaynak sağlayıcısı bağdaştırıcısı VM**, sağlayıcı hizmetlerini çalıştıran bir Windows sanal makine olduğu.
- **Kaynak sağlayıcısı**, sağlama isteklerini işler ve kullanıma sunar kaynaklarını veritabanı.
- **SQL Server'ı barındıran sunucular**, veritabanları için kapasite sağlayan barındırma sunucuları denir.

Siz bir (veya daha fazla) SQL Server örneklerini oluşturmak veya dış SQL Server örnekleri için erişim sağlamak.

> [!NOTE]
> Barındırma Azure yığın üzerinde yüklü olan sunucuları tümleşik sistemleri Kiracı abonelikten oluşturulması gerekir. Varsayılan sağlayıcı abonelikten oluşturulamıyor. Bunlar, Kiracı portalından veya bir uygun oturum açma ile bir PowerShell oturumu oluşturulmalıdır. Tüm barındırma sunucuları ücrete tabi VM'ler ve uygun izinlere sahip olmalıdır. Hizmet Yöneticisi Kiracı aboneliğin sahibi olabilir.

## <a name="deploy-the-resource-provider"></a>Kaynak sağlayıcısı dağıtma

1. Zaten yapmadıysanız, Geliştirme Seti kaydolun ve Windows Server 2016 Datacenter Core görüntünün Market yönetiminden indirilebilir indirin. Bir Windows Server 2016 Core görüntüsü kullanmanız gerekir. Bir komut dosyası oluşturmak için de kullanabilirsiniz bir [Windows Server 2016 görüntü](https://docs.microsoft.com/azure/azure-stack/azure-stack-add-default-image). (Çekirdek seçeneğini belirlediğinizden emin olun.)

2. VM ayrıcalıklı uç noktasına erişebildiğinden bir ana bilgisayara oturum açın.

    - Azure yığın Geliştirme Seti yüklemelerinde fiziksel ana bilgisayara oturum açın.

    - Birden çok düğümlü sistemlerde konak ayrıcalıklı uç noktasına erişebildiğinden bir sistem olmalıdır.
    
    >[!NOTE]
    > Burada betik çalıştırılıyor sistem *gerekir* yüklü .NET çalışma zamanı en son sürümü Windows 10 veya Windows Server 2016 sistemiyle olabilir. Aksi takdirde yükleme başarısız olur. Azure yığın SDK konak bu ölçütü karşılayan.


3. SQL kaynak sağlayıcısı ikili indirin. Daha sonra içeriği geçici bir dizine ayıklayın ayıklayıcısı çalıştırın.

    >[!NOTE] 
    > Kaynak sağlayıcısı bir minimum karşılık gelen Azure yapı yığınına sahiptir. Doğru ikili çalıştıran Azure yığın sürümü yüklediğinizden emin olun.

    | Azure yığın derleme | SQL kaynak sağlayıcısı yükleyicisi |
    | --- | --- |
    | 1802: 1.0.180302.1 | [SQL RP sürüm 1.1.18.0](https://aka.ms/azurestacksqlrp1802) |
    | 1712: 1.0.180102.3, 1.0.180103.2 veya 1.0.180106.1 (çok düğümlü) | [SQL RP sürüm 1.1.14.0](https://aka.ms/azurestacksqlrp1712) |
    | 1711: 1.0.171122.1 | [SQL RP sürüm 1.1.12.0](https://aka.ms/azurestacksqlrp1711) |
    | 1710: 1.0.171028.1 | [SQL RP sürüm 1.1.8.0](https://aka.ms/azurestacksqlrp1710) |
  

4. Azure yığın SDK'sı için bu işlemin bir parçası olarak otomatik olarak imzalanan bir sertifika oluşturulur. Tümleşik sistemler için uygun bir sertifika sağlamanız gerekir.

   Kendi sertifikanızı sağlamak için bir .pfx dosyasına yerleştirin **DependencyFilesLocalPath** gibi:

    - İçin bir joker karakter sertifika \*.dbadapter.\< Bölge\>.\< Dış fqdn\> veya sqladapter.dbadapter bir ortak adı tek bir site sertifikayla.\< Bölge\>.\< Dış fqdn\>.

    - Bu sertifikayı Güvenilir olması gerekir. Yani, güven zinciri ara sertifika gerektirmeden bulunması gerekir.

    - Yalnızca tek bir sertifika dosyası DependencyFilesLocalPath parametresiyle işaret dizininde bulunabilir.

    - Dosya adı özel karakterler içermemelidir.


5. Açık bir **yeni** yükseltilmiş (Yönetim) PowerShell konsolu ve dosyaları ayıkladığınız dizine geçin. Sistemde zaten yüklü olan yanlış PowerShell modüllerden doğabilecek sorunları önlemek için yeni bir pencere kullanın.

6. [Azure PowerShell sürüm 1.2.11 yükleme](azure-stack-powershell-install.md).

7. Bu adımları gerçekleştirir DeploySqlProvider.ps1 komut dosyasını çalıştırın:

    - Sertifikaları ve diğer yapıları depolama hesabı Azure yığında yükler.
    - SQL veritabanları Galerisine dağıtabilirsiniz galeri paketleri yayınlar.
    - Barındırma sunucuları dağıtmak için bir galeri paketi yayımlar.
    - 1. adımda oluşturulan ve ardından kaynak sağlayıcısı yükleyen Windows Server 2016 görüntüsünü kullanarak bir VM dağıtır.
    - Kaynak sağlayıcınız VM eşler yerel bir DNS kaydı kaydeder.
    - Kaynak sağlayıcısı yerel Azure Resource Manager ile (kullanıcı ve yönetim) kaydeder.
    - İsteğe bağlı olarak tek bir Windows güncelleştirmesi RP yükleme sırasında yükler

8. Market Yönetimi'nden en son Windows Server 2016 Core görüntüyü indirmeyi öneririz. Bir güncelleştirme yüklemeniz gerekiyorsa, tek bir yerleştirebilirsiniz. Yerel bağımlılık yolundaki MSU paketi. Birden fazla ise. MSU dosyası bulundu, komut dosyası başarısız olur.


PowerShell isteminden çalıştırabilirsiniz örnek aşağıda verilmiştir. (Gerektiğinde parola ve hesap bilgilerini değiştirdiğinizden emin olun.)

```
# Install the AzureRM.Bootstrapper module, set the profile, and install the AzureRM and AzureStack modules.
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2017-03-09-profile
Install-Module -Name AzureStack -RequiredVersion 1.2.11 -Force

# Use the NetBIOS name for the Azure Stack domain. On the Azure Stack SDK, the default is AzureStack but could have been changed at install time.
$domain = "AzureStack"

# For integrated systems, use the IP address of one of the ERCS virtual machines
$privilegedEndpoint = "AzS-ERCS01"

# Point to the directory where the resource provider installation files were extracted.
$tempDir = 'C:\TEMP\SQLRP'

# The service admin account (can be Azure Active Directory or Active Directory Federation Services).
$serviceAdmin = "admin@mydomain.onmicrosoft.com"
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass)

# Set credentials for the new resource provider VM local administrator account
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("sqlrpadmin", $vmLocalAdminPass)

# And the cloudadmin credential that's required for privileged endpoint access.
$CloudAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass)

# Change the following as appropriate.
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force

# Change directory to the folder where you extracted the installation files.
# Then adjust the endpoints.
. $tempDir\DeploySQLProvider.ps1 -AzCredential $AdminCreds `
  -VMLocalCredential $vmLocalAdminCreds `
  -CloudAdminCredential $cloudAdminCreds `
  -PrivilegedEndpoint $privilegedEndpoint `
  -DefaultSSLCertificatePassword $PfxPass `
  -DependencyFilesLocalPath $tempDir\cert
 ```

### <a name="deploysqlproviderps1-parameters"></a>DeploySqlProvider.ps1 parameters
Komut satırında bu parametreleri belirtebilirsiniz. Bunu yapmazsanız veya hiçbir parametre doğrulaması başarısız olursa, gerekli parametreler sağlamanız istenir.

| Parametre adı | Açıklama | Açıklama veya varsayılan değer |
| --- | --- | --- |
| **CloudAdminCredential** | Bulut Yöneticisi, ayrıcalıklı endpoint erişmek için gerekli kimlik bilgileri. | _Gerekli_ |
| **AzCredential** | Azure yığın hizmet yönetici hesabı için kimlik bilgileri. Azure yığın dağıtmak için kullanılan kimlik bilgilerinin aynısını kullanın. | _Gerekli_ |
| **VMLocalCredential** | SQL kaynak sağlayıcısının VM yerel yönetici hesabı için kimlik bilgileri. | _Gerekli_ |
| **PrivilegedEndpoint** | IP adresi veya ayrıcalıklı uç noktanın DNS adı. |  _Gerekli_ |
| **DependencyFilesLocalPath** | Sertifika .pfx dosyasının bu dizinde yerleştirilmelidir. | _İsteğe bağlı_ (_zorunlu_ çok düğümlü için) |
| **DefaultSSLCertificatePassword** | .Pfx sertifika parolası. | _Gerekli_ |
| **MaxRetryCount** | Bir hata olduğunda her işlemini yeniden denemek istiyor sayısı.| 2 |
| **RetryDuration** | Saniye cinsinden yeniden denemeler arasındaki zaman aşımı aralığı. | 120 |
| **Kaldırma** | (Aşağıdaki notlara bakın) ilişkili tüm kaynakları ve kaynak sağlayıcısını kaldırır. | Hayır |
| **DebugMode** | Otomatik temizleme hatasında engeller. | Hayır |


## <a name="verify-the-deployment-using-the-azure-stack-portal"></a>Azure yığın Portalı'nı kullanarak dağıtımı doğrulama

> [!NOTE]
>  Yükleme komut dosyası çalışması bittikten sonra Yönetim dikey penceresini görmek için portal yenilemeniz gerekir.


1. Yönetim portalında Hizmet Yöneticisi olarak oturum açın.

2. Dağıtım başarılı olduğunu doğrulayın. Git **kaynak grupları**. Ardından **sistem.\< Konum\>.sqladapter** kaynak grubu. Tüm dört dağıtımlarının başarılı olduğunu doğrulayın.

      ![SQL kaynak sağlayıcısı dağıtımını doğrulayın](./media/azure-stack-sql-rp-deploy/sqlrp-verify.png)


## <a name="update-the-sql-resource-provider-adapter-multi-node-only-builds-1710-and-later"></a>SQL kaynak sağlayıcısı bağdaştırıcısını (çok düğümlü yalnızca derlemeleri 1710 ve üzeri) güncelleştir
Azure yığın derlemeleri güncelleştirildiğinde yeni bir SQL kaynak sağlayıcısı bağdaştırıcısı yayımlanan. Varolan bağdaştırıcısı çalışmaya devam ederken, en son sürüme mümkün olan en kısa sürede güncelleştirilmesi önerilir. Güncelleştirmeleri sırayla yüklü olmalıdır: sürümleri atlayamazsınız (adım 3 / bölümündeki tabloya bakın [kaynak sağlayıcısı dağıtmak](#deploy-the-resource-provider)).

Kullandığınız kaynak sağlayıcısı güncelleştirmek için *UpdateSQLProvider.ps1* komut dosyası. İşlem bölümünde açıklandığı gibi bir kaynak Sağlayıcısı'nı yüklemek için kullanılan işlem benzer [kaynak sağlayıcısı dağıtmak](#deploy-the-resource-provider) bu makalenin. Betik kaynak sağlayıcısı yükleme ile dahil edilir.

*UpdateSQLProvider.ps1* komut dosyası en son kaynak sağlayıcısı kodu ile yeni bir VM oluşturur ve yeni VM'ye eski sanal makineden ayarları geçirir. Geçiş ayarları veritabanı ve barındırma sunucusu bilgilerini içerir ve gerekli DNS kaydı.

Komut dosyası için DeploySqlProvider.ps1 komut açıklanan aynı bağımsız değişkenlere kullanılmasını gerektirir. Sertifika burada da sağlar. 

Market Yönetimi'nden en son Windows Server 2016 Core görüntüyü indirmeyi öneririz. Bir güncelleştirme yüklemeniz gerekiyorsa, tek bir yerleştirebilirsiniz. Yerel bağımlılık yolundaki MSU paketi. Birden fazla ise. MSU dosyası bulundu, komut dosyası başarısız olur.

Aşağıdaki örneği verilmiştir *UpdateSQLProvider.ps1* PowerShell isteminden çalıştırıp komut dosyası. Hesap bilgileri ve gerektiğinde parolaları değiştirdiğinizden emin olun: 

> [!NOTE]
> Güncelleştirme işlemi yalnızca tümleşik sistemleri için geçerlidir.

```
# Install the AzureRM.Bootstrapper module, set the profile, and install the AzureRM and AzureStack modules.
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2017-03-09-profile
Install-Module -Name AzureStack -RequiredVersion 1.2.11 -Force

# Use the NetBIOS name for the Azure Stack domain. On the Azure Stack SDK, the default is AzureStack but could have been changed at install time.
$domain = "AzureStack"

# For integrated systems, use the IP address of one of the ERCS virtual machines
$privilegedEndpoint = "AzS-ERCS01"

# Point to the directory where the resource provider installation files were extracted.
$tempDir = 'C:\TEMP\SQLRP'

# The service admin account (can be Azure AD or AD FS).
$serviceAdmin = "admin@mydomain.onmicrosoft.com"
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass)

# Set credentials for the new Resource Provider VM.
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("sqlrpadmin", $vmLocalAdminPass)

# And the cloudadmin credential required for privileged endpoint access.
$CloudAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass)

# Change the following as appropriate.
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force

# Change directory to the folder where you extracted the installation files.
# Then adjust the endpoints.
. $tempDir\UpdateSQLProvider.ps1 -AzCredential $AdminCreds `
  -VMLocalCredential $vmLocalAdminCreds `
  -CloudAdminCredential $cloudAdminCreds `
  -PrivilegedEndpoint $privilegedEndpoint `
  -DefaultSSLCertificatePassword $PfxPass `
  -DependencyFilesLocalPath $tempDir\cert
 ```

### <a name="updatesqlproviderps1-parameters"></a>UpdateSQLProvider.ps1 parametreleri
Komut satırında bu parametreleri belirtebilirsiniz. Bunu yapmazsanız veya hiçbir parametre doğrulaması başarısız olursa, gerekli parametreler sağlamanız istenir.

| Parametre adı | Açıklama | Açıklama veya varsayılan değer |
| --- | --- | --- |
| **CloudAdminCredential** | Bulut Yöneticisi, ayrıcalıklı endpoint erişmek için gerekli kimlik bilgileri. | _Gerekli_ |
| **AzCredential** | Azure yığın hizmet yönetici hesabı için kimlik bilgileri. Azure yığın dağıtmak için kullanılan kimlik bilgilerini kullanın. | _Gerekli_ |
| **VMLocalCredential** | SQL kaynak sağlayıcısının VM yerel yönetici hesabı için kimlik bilgileri. | _Gerekli_ |
| **PrivilegedEndpoint** | IP adresi veya ayrıcalıklı uç noktanın DNS adı. |  _Gerekli_ |
| **DependencyFilesLocalPath** | Sertifika .pfx dosyasının bu dizinde yerleştirilmelidir. | _İsteğe bağlı_ (_zorunlu_ çok düğümlü için) |
| **DefaultSSLCertificatePassword** | .Pfx sertifika parolası. | _Gerekli_ |
| **MaxRetryCount** | Bir hata olduğunda her işlemini yeniden denemek istiyor sayısı.| 2 |
| **RetryDuration** |Saniye cinsinden yeniden denemeler arasındaki zaman aşımı aralığı. | 120 |
| **Kaldırma** | (Aşağıdaki notlara bakın) ilişkili tüm kaynakları ve kaynak sağlayıcısını kaldırır. | Hayır |
| **DebugMode** | Otomatik temizleme hatasında engeller. | Hayır |


## <a name="collect-diagnostic-logs"></a>Tanılama günlüklerini toplayın
SQL kaynak kilitli bir sanal makineyi sağlayıcıdır. Bu sanal makineden bir PowerShell yalnızca yetecek kadar Yönetim (JEA) uç noktası günlükleri toplamak için gerekli olur _DBAdapterDiagnostics_ bu amaç için sağlanır. Bu uç noktası aracılığıyla kullanılabilen iki komut vardır:

* Get-AzsDBAdapterLog - RP tanılama günlükleri içeren bir zip paketini hazırlar ve oturum kullanıcı sürücüde yerleştirir. Komut parametresiz çağrılabilir ve günlükleri son dört saatlik toplar.
* Remove-AzsDBAdapterLog - kaynak sağlayıcısı VM mevcut günlük paketlerini temizler

Bir kullanıcı hesabı olarak adlandırılan _dbadapterdiag_ RP dağıtım veya RP günlükleri ayıklanacağı tanılama uç noktasına bağlanmak için güncelleştirme işlemi sırasında oluşturulur. Bu hesabın parolasını dağıtım/güncelleştirme sırasında yerel yönetici hesabı için girilen parola ile aynıdır.

Bu komutları kullanmak için kaynak sağlayıcısı sanal makineye uzak PowerShell oturumu oluşturmak ve komut çağırma gerekecektir. İsteğe bağlı olarak FromDate ve ToDate parametreleri sağlayabilir. Aşağıdakilerden birini veya bunların her ikisi de belirtmezseniz, geçerli tarihten önce dört saat FromDate olacaktır ve ToDate geçerli saati olacaktır.

Bu örnek betik, bu komutları kullanımını göstermektedir:

```
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

## <a name="maintenance-operations-integrated-systems"></a>Bakım işlemleri (tümleşik sistemler için)
SQL kaynak kilitli bir sanal makineyi sağlayıcıdır. Kaynak sağlayıcısı sanal makinenin güvenlik güncelleştirme PowerShell yalnızca yetecek kadar Yönetim (JEA) uç noktası aracılığıyla yapılabilir _DBAdapterMaintenance_.

Bir betik bu işlemleri kolaylaştırmak için RP'ın yükleme paketi ile birlikte sağlanır.

### <a name="update-the-virtual-machine-operating-system"></a>Sanal makine işletim sistemini güncelleştirmek
Windows Server VM güncelleştirmek için birkaç yolu vardır:
* Şu anda düzeltme eki yüklenmiş bir Windows Server 2016 Core görüntüsünü kullanarak en son kaynak sağlayıcısı paketini yükle
* RP güncelleştirilmesini veya yükleme sırasında Windows Update paket yükleme


### <a name="update-the-virtual-machine-windows-defender-definitions"></a>Sanal makine Windows Defender tanımlarını güncelleştirme

Defender tanımlarını güncelleştirmek için aşağıdaki adımları izleyin:

1. Windows Defender tanımlarını Update'ten indirme [Windows Defender tanım](https://www.microsoft.com/en-us/wdsi/definitions)

    Bu sayfada "El ile yükleyip tanımları altında" indir "Windows 10 ve Windows 8.1" 64-bit dosya için Windows Defender Antivirus. 
    
    Doğrudan bağlantı: https://go.microsoft.com/fwlink/?LinkID=121721&arch=x64

2. Bir PowerShell oturumuna SQL RP bağdaştırıcısı sanal makinenin bakım uç noktası oluşturma
3. Bakım uç nokta oturumu kullanarak DB bağdaştırıcısı makineye tanımları güncelleştirme dosyasını kopyalayın
4. Bakım PowerShell oturumu çağırma _güncelleştirme DBAdapterWindowsDefenderDefinitions_ komutu
5. Yüklemeden sonra kullanılan tanımları güncelleştirme dosyası kaldırmak için önerilir. Bakım kullanarak oturum kaldırılabilir _Kaldır ItemOnUserDrive)_ komutu.


(Adresi veya gerçek değer ile sanal makine adını değiştirin) Defender tanımlarını güncelleştirmek için örnek bir betik verilmiştir:

```
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

## <a name="remove-the-sql-resource-provider-adapter"></a>SQL kaynak sağlayıcısı bağdaştırıcısını Kaldır

Kaynak sağlayıcısı kaldırmak için önce tüm bağımlılıklarını kaldırmanız gereklidir.

1. SQL kaynak sağlayıcısı bağdaştırıcısı bu sürümü için indirdiğiniz özgün dağıtım paketi olduğundan emin olun.

2. Tüm kullanıcı veritabanlarını kaynak Sağlayıcısı'ndan silinmesi gerekir. (Kullanıcı veritabanlarını silerek verileri silmez.) Bu görev, kullanıcılar tarafından gerçekleştirilmelidir.

3. Yönetici barındırma sunucuları SQL kaynak sağlayıcısı bağdaştırıcısından silmeniz gerekir.

4. Yönetici SQL kaynak sağlayıcısı bağdaştırıcısı başvuru herhangi bir plan silmeniz gerekir.

5. Yönetici, tüm SKU'ları ve SQL kaynak sağlayıcısı bağdaştırıcısı ile ilişkili kotaları silmeniz gerekir.

6. Aşağıdaki öğeleri içeren dağıtım betiği yeniden çalıştırın:
    - Parametre kaldırma
    - Azure Resource Manager uç noktaları
    - DirectoryTenantID
    - Hizmet yöneticisi hesabı için kimlik bilgileri


## <a name="next-steps"></a>Sonraki adımlar

[Barındırma sunucuları ekleme](azure-stack-sql-resource-provider-hosting-servers.md) ve [veritabanları oluşturma](azure-stack-sql-resource-provider-databases.md).

Diğer deneyin [PaaS Hizmetleri](azure-stack-tools-paas-services.md) gibi [MySQL Server Kaynak sağlayıcısı](azure-stack-mysql-resource-provider-deploy.md) ve [uygulama hizmetleri kaynak sağlayıcı](azure-stack-app-service-overview.md).
