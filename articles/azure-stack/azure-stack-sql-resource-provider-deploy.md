---
title: "SQL veritabanları Azure yığında kullanarak | Microsoft Docs"
description: "SQL veritabanları Azure yığını ve hızlı adımlar SQL Server Kaynak sağlayıcısı bağdaştırıcısı dağıtmak için bir hizmet olarak nasıl dağıtabileceğini öğrenin."
services: azure-stack
documentationCenter: 
author: JeffGoldner
manager: bradleyb
editor: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2018
ms.author: JeffGo
ms.openlocfilehash: bf52ed4986b4e0930b57721c0e38bbf748045a36
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
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

Gereken SQL Server'ın bir (veya daha fazla) intances oluşturma ve/veya dış SQL Server örnekleri için erişim sağlar.

## <a name="deploy-the-resource-provider"></a>Kaynak sağlayıcısı dağıtma

1. Zaten yapmadıysanız, Geliştirme Seti kaydolun ve Windows Server 2016 Datacenter Core görüntünün Market yönetiminden indirilebilir indirin. Bir Windows Server 2016 Core görüntüsü kullanmanız gerekir. Bir komut dosyası oluşturmak için de kullanabilirsiniz bir [Windows Server 2016 görüntü](https://docs.microsoft.com/azure/azure-stack/azure-stack-add-default-image). (Çekirdek seçeneğini belirlediğinizden emin olun). .NET 3.5 çalışma zamanı artık gerekli değildir.

2. VM ayrıcalıklı uç noktasına erişebildiğinden bir ana bilgisayara oturum açın.

    - Azure yığın Geliştirme Seti yüklemelerinde fiziksel ana bilgisayara oturum açın.

    - Birden çok düğümlü sistemlerde konak ayrıcalıklı uç noktasına erişebildiğinden bir sistem olmalıdır.
    
    >[!NOTE]
    > Burada betik çalıştırılıyor sistem *gerekir* yüklü .NET çalışma zamanı en son sürümü Windows 10 veya Windows Server 2016 sistemiyle olabilir. Aksi takdirde yükleme başarısız olur. Azure yığın SDK konak bu ölçütü karşılayan.


3. SQL kaynak sağlayıcısı ikili indirin. Daha sonra içeriği geçici bir dizine ayıklayın ayıklayıcısı çalıştırın.

    >[!NOTE] 
    > Kaynak sağlayıcı yapı Azure yığın derlemeleri karşılık gelir. Doğru ikili çalıştıran Azure yığın sürümü yüklediğinizden emin olun.

    | Azure yığın derleme | SQL kaynak sağlayıcısı yükleyicisi |
    | --- | --- |
    |1.0.180102.3, 1.0.180103.2 veya 1.0.180106.1 (çok düğümlü) | [SQL RP sürüm 1.1.14.0](https://aka.ms/azurestacksqlrp1712) |
    | 1.0.171122.1 | [SQL RP sürüm 1.1.12.0](https://aka.ms/azurestacksqlrp1711) |
    | 1.0.171028.1 | [SQL RP sürüm 1.1.8.0](https://aka.ms/azurestacksqlrp1710) |
  

4. Azure yığın kök sertifikası ayrıcalıklı uç noktasından alınır. Azure yığın SDK'sı için bu işlemin bir parçası olarak otomatik olarak imzalanan bir sertifika oluşturulur. Birden çok düğümlü için uygun bir sertifika sağlamanız gerekir.

   Kendi sertifikanızı sağlamak için bir .pfx dosyasına yerleştirin **DependencyFilesLocalPath** gibi:

    - İçin bir joker karakter sertifika \*.dbadapter.\< Bölge\>.\< Dış fqdn\> veya sqladapter.dbadapter bir ortak adı tek bir site sertifikayla.\< Bölge\>.\< Dış fqdn\>.

    - Bu sertifikayı Güvenilir olması gerekir. Yani, güven zinciri ara sertifika gerektirmeden bulunması gerekir.

    - Yalnızca tek bir sertifika dosyası DependencyFilesLocalPath bulunmaktadır.

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

> [!NOTE]
> Yükleme 90 dakikadan uzun sürerse, başarısız olabilir. Başarısız olursa bir hata iletisi ekranında ve günlük dosyasına bakın, ancak dağıtımı başarısız olan adımdan yeniden denenir. Önerilen bellek ve vCPU belirtimleri karşılamıyor sistemleri SQL kaynak kaydının sağlayıcısı dağıtmak mümkün olmayabilir.
>

PowerShell isteminden çalıştırabilirsiniz örnek aşağıda verilmiştir. (Gerektiğinde parola ve hesap bilgilerini değiştirdiğinizden emin olun.)

```
# Install the AzureRM.Bootstrapper module, set the profile, and install the AzureRM and AzureStack modules.
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2017-03-09-profile
Install-Module -Name AzureStack -RequiredVersion 1.2.11 -Force

# Use the NetBIOS name for the Azure Stack domain. On the Azure Stack SDK, the default is AzureStack and the default prefix is AzS.
# For integrated systems, the domain and the prefix are the same.
$domain = "AzureStack"
$prefix = "AzS"
$privilegedEndpoint = "$prefix-ERCS01"

# Point to the directory where the resource provider installation files were extracted.
$tempDir = 'C:\TEMP\SQLRP'

# The service admin account (can be Azure Active Directory or Active Directory Federation Services).
$serviceAdmin = "admin@mydomain.onmicrosoft.com"
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass)

# Set credentials for the new Resource Provider VM.
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
Azure yığın derleme güncelleştirildiğinde, yeni bir SQL kaynak sağlayıcısı bağdaştırıcısı yayımlanır. Varolan bağdaştırıcısı çalışmaya devam edebilir. Ancak, Azure yığın güncelleştirildikten sonra en son sürüme mümkün olan en kısa sürede güncelleştirilmesi önerilir. 

Güncelleştirme işlemi, daha önce açıklanan yükleme işlemine benzer. Son kaynak sağlayıcısı kodu ile yeni bir VM oluşturun. Ayrıca, ayarları yeni bu örneğe barındırma sunucusu bilgilerini ve veritabanı dahil olmak üzere geçiş. Ayrıca, gerekli DNS kaydı de geçirin.

Daha önce açıklanan aynı bağımsız değişkenlere UpdateSQLProvider.ps1 komut dosyası kullanın. Sertifika burada de sağlamanız gerekir.

> [!NOTE]
> Bu güncelleştirme işlemi, yalnızca birden çok düğümlü sistemlerde desteklenir.

```
# Install the AzureRM.Bootstrapper module, set the profile, and install the AzureRM and AzureStack modules.
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2017-03-09-profile
Install-Module -Name AzureStack -RequiredVersion 1.2.11 -Force

# Use the NetBIOS name for the Azure Stack domain. On the Azure Stack SDK, the default is AzureStack and the default prefix is AzS.
# For integrated systems, the domain and the prefix are the same.
$domain = "AzureStack"
$prefix = "AzS"
$privilegedEndpoint = "$prefix-ERCS01"

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
