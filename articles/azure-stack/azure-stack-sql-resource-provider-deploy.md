---
title: "SQL veritabanları Azure yığında kullanarak | Microsoft Docs"
description: "SQL veritabanları Azure yığını ve hızlı adımlar SQL Server Kaynak sağlayıcısı bağdaştırıcısı dağıtmak için bir hizmet olarak nasıl dağıtabileceğini öğrenin"
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
ms.date: 10/10/2017
ms.author: JeffGo
ms.openlocfilehash: 31ffd31b5d540617c4a7a1224e6cf0ee656c9678
ms.sourcegitcommit: 4ea06f52af0a8799561125497f2c2d28db7818e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2017
---
# <a name="use-sql-databases-on-microsoft-azure-stack"></a>Microsoft Azure yığın üzerinde SQL veritabanları kullanın

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

SQL veritabanları hizmet olarak kullanıma sunmak için SQL Server Kaynak sağlayıcısı bağdaştırıcısını kullanmak [Azure yığın](azure-stack-poc.md). Kaynak sağlayıcısını yüklemek ve bir veya daha fazla SQL Server örneklerine bağlamak sonra siz ve kullanıcılarınız oluşturabilirsiniz:
- veritabanları için bulut yerel uygulamalar
- SQL tabanlı Web siteleri
- SQL, temel iş yükleri bir sanal makine (VM) barındıran SQL Server her zaman sağlamak zorunda değilsiniz.

Kaynak sağlayıcısı tüm veritabanı yönetim özelliklerini desteklemez [Azure SQL veritabanı](https://azure.microsoft.com/services/sql-database/). Örneğin, esnek veritabanı havuzları ve veritabanı performansını yukarı ve aşağı otomatik arama özelliği kullanılabilir değil. Ancak, kaynak sağlayıcısı yoksa desteği benzer oluşturma, okuma, güncelleştirme ve Sil (CRUD) işlemleridir. API SQL DB ile uyumlu değil.

## <a name="sql-resource-provider-adapter-architecture"></a>SQL kaynak sağlayıcısı bağdaştırıcısı mimarisi
Kaynak sağlayıcısı üç bileşenlerden oluşur:

- **SQL kaynak sağlayıcısı bağdaştırıcısı VM**, hangi sağlayıcı hizmetlerini çalıştıran bir Windows sanal makine.
- **Kaynak sağlayıcısı**, sağlama isteklerini işler ve kullanıma sunar kaynaklarını veritabanı.
- **SQL Server'ı barındıran sunucular**, veritabanları için kapasite sağlayan barındırma sunucuları denir.

Siz bir (veya daha fazla) SQL Server'lar oluşturun veya dış SQL örnekleri erişim sağlamak.

## <a name="deploy-the-resource-provider"></a>Kaynak sağlayıcısı dağıtma

1. Zaten yapmadıysanız, Geliştirme Seti kaydolun ve Windows Server 2016 Datacenter Core görüntünün Market yönetiminden indirilebilir indirin. Bir Windows Server 2016 Core görüntüsü kullanmanız gerekir. Bir komut dosyası oluşturmak için de kullanabilirsiniz bir [Windows Server 2016 görüntü](https://docs.microsoft.com/azure/azure-stack/azure-stack-add-default-image) -çekirdek seçeneğini belirlediğinizden emin olun. .NET 3.5 çalışma zamanı artık gerekli değildir.

2. Ayrıcalıklı uç nokta VM erişebileceği bir ana bilgisayara oturum açın.

    a. Azure yığın Geliştirme Seti (ASDK) yüklemelerinde fiziksel ana bilgisayara oturum açın.

    b. Birden çok düğümlü sistemlerde konak ayrıcalıklı uç noktasına erişebildiğinden bir sistem olmalıdır.

3. [SQL kaynak sağlayıcısı ikili dosya indirme](https://aka.ms/azurestacksqlrp) ve geçici bir dizine içeriği ayıklamak için ayıklayıcısı yürütün.

    > [!NOTE]
    > Bir Azure yığın üzerinde çalışan 20170928.3 oluşturursanız veya önceki sürümleri, [bu sürümü yüklemek](https://aka.ms/azurestacksqlrp1709).

4. Azure yığın kök sertifikası ayrıcalıklı uç noktasından alınır. ASDK için bu işlemin bir parçası olarak otomatik olarak imzalanan bir sertifika oluşturulur. Birden çok düğümlü için uygun bir sertifika sağlamanız gerekir.

    Kendi sertifikanızı sağlamanız gerekiyorsa, aşağıdaki sertifika gerekir:

    Joker sertifikası \*.dbadapter.\< Bölge\>.\< Dış fqdn\>. Bu sertifikayı Güvenilir olması gerekir, gibi bir sertifika yetkilisi tarafından verilmiş. Yani, güven zinciri ara sertifika gerektirmeden bulunması gerekir. Tek bir site sertifika yüklemesi sırasında kullanılan açık VM adı [sqladapter] ile kullanılabilir.


5. Açık bir **yeni** yükseltilmiş (Yönetim) PowerShell konsolu ve dosyaları ayıkladığınız dizine geçin. Sistemde zaten yüklü yanlış PowerShell modüllerden ortaya çıkabilecek sorunları önlemek için yeni bir pencere kullanın.

6. [Azure PowerShell sürüm 1.2.11 yükleme](azure-stack-powershell-install.md).

7. Aşağıda listelenen parametrelerle DeploySqlProvider.ps1 komut dosyasını çalıştırın.

Komut dosyası, aşağıdaki adımları gerçekleştirir:

- Sertifikaları ve diğer yapıları depolama hesabı, Azure yığında karşıya yükleyin.
- Böylece SQL veritabanları Galerisine dağıtabilirsiniz galeri paketleri yayımlama.
- Barındırma sunucuları dağıtmak için bir galeri paketi Yayımla
- 1. adımda oluşturduğunuz Windows Server 2016 görüntüsünü kullanan bir VM'yi dağıtmak ve kaynak sağlayıcısını yükleyin.
- Kaynak sağlayıcınız VM eşler yerel bir DNS kaydı kaydedin.
- Kaynak sağlayıcısı yerel Azure Resource Manager ile (kullanıcı ve yönetim) kaydedin.

> [!NOTE]
> Yükleme 90 dakikadan uzun sürerse, başarısız olabilir ve bir hata iletisi ekranında ve günlük dosyasına bakın, ancak dağıtımı başarısız olan adımdan yeniden denenir. Önerilen bellek ve vCPU belirtimleri karşılamıyor sistemleri SQL RP dağıtmak mümkün olmayabilir.
>

Powershell'den çalıştırabilirsiniz örneği sor (ancak hesap bilgilerini ve parolaları gerektiği gibi değiştirin):

```
# Install the AzureRM.Bootstrapper module, set the profile, and install AzureRM and AzureStack modules
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2017-03-09-profile
Install-Module -Name AzureStack -RequiredVersion 1.2.11 -Force

# Use the NetBIOS name for the Azure Stack domain. On ASDK, the default is AzureStack and the default prefix is AzS
# For integrated systems, the domain and the prefix will be the same.
$domain = "AzureStack"
$prefix = "AzS"
$privilegedEndpoint = "$prefix-ERCS01"

# Point to the directory where the RP installation files were extracted
$tempDir = 'C:\TEMP\SQLRP'

# The service admin account (can be AAD or ADFS)
$serviceAdmin = "admin@mydomain.onmicrosoft.com"
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass)

# Set the credentials for the Resource Provider VM
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("sqlrpadmin", $vmLocalAdminPass)

# and the cloudadmin credential required for Privileged Endpoint access
$CloudAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass)

# change the following as appropriate
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force

# Change directory to the folder where you extracted the installation files
# and adjust the endpoints
. $tempDir\DeploySQLProvider.ps1 -AzCredential $AdminCreds `
  -VMLocalCredential $vmLocalAdminCreds `
  -CloudAdminCredential $cloudAdminCreds `
  -PrivilegedEndpoint $privilegedEndpoint `
  -DefaultSSLCertificatePassword $PfxPass `
  -DependencyFilesLocalPath $tempDir\cert
 ```

### <a name="deploysqlproviderps1-parameters"></a>DeploySqlProvider.ps1 parametreleri
Komut satırında bu parametreleri belirtebilirsiniz. Bunu yapmanız veya hiçbir parametre doğrulaması başarısız olursa, gerekli olanları sağlamanız istenir.

| Parametre Adı | Açıklama | Açıklama veya varsayılan değeri |
| --- | --- | --- |
| **CloudAdminCredential** | Bulut Yöneticisi, ayrıcalıklı Endpoint erişmek için gerekli kimlik bilgileri. | _Gerekli_ |
| **AzCredential** | Azure yığın Hizmet yöneticisi hesabı için kimlik bilgilerini sağlayın. Azure yığın dağıtmak için kullanılan aynı kimlik bilgilerini kullanın). | _Gerekli_ |
| **VMLocalCredential** | SQL kaynak sağlayıcısının VM yerel yönetici hesabının kimlik bilgilerini tanımlar. | _Gerekli_ |
| **PrivilegedEndpoint** | IP adresi veya Privleged uç noktanın DNS adı sağlayın. |  _Gerekli_ |
| **DependencyFilesLocalPath** | Sertifika PFX dosyasının bu dizinde yerleştirilmelidir. | _İsteğe bağlı_ (_zorunlu_ çok düğümlü için) |
| **DefaultSSLCertificatePassword** | .Pfx sertifika için parola | _Gerekli_ |
| **MaxRetryCount** | Bir hata varsa her işlemini yeniden denemek istiyor kaç kez tanımlayın.| 2 |
| **RetryDuration** | Zaman aşımı saniye içinde yeniden denemeler arasında tanımlayın. | 120 |
| **Kaldırma** | (Aşağıdaki notlara bakın) ilişkili tüm kaynakları ve kaynak sağlayıcısını Kaldır | Hayır |
| **DebugMode** | Otomatik temizleme hatasında engeller | Hayır |


## <a name="verify-the-deployment-using-the-azure-stack-portal"></a>Azure yığın Portalı'nı kullanarak dağıtımı doğrulama

> [!NOTE]
>  Yükleme komut dosyası tamamlandıktan sonra Yönetim dikey penceresini görmek için portal yenilemeniz gerekir.


1. Yönetim portalında Hizmet Yöneticisi olarak oturum açın.

2. Dağıtım başarılı olduğunu doğrulayın. Göz **kaynak grupları** &gt;, tıklayın **sistem.\< Konum\>.sqladapter** kaynak grubunu ve tüm dört dağıtımlarının başarılı olduğunu doğrulayın.

      ![SQL RP dağıtımını doğrulayın](./media/azure-stack-sql-rp-deploy/sqlrp-verify.png)


## <a name="remove-the-sql-resource-provider-adapter"></a>SQL kaynak sağlayıcısı bağdaştırıcısını Kaldır

Kaynak sağlayıcısı kaldırmak için önce tüm bağımlılıklarını kaldırmanız gereklidir.

1. SQL kaynak sağlayıcısı bağdaştırıcısı bu sürümü için indirdiğiniz özgün dağıtım paketi olduğundan emin olun.

2. Tüm kullanıcı veritabanlarında (Bu verileri silmez) kaynak Sağlayıcısı'ndan silinmesi gerekir. Bu kullanıcılar tarafından gerçekleştirilmelidir.

3. Yönetici barındırma sunucuları SQL kaynak sağlayıcısı bağdaştırıcısından silmeniz gerekir

4. Yönetici SQL kaynak sağlayıcısı bağdaştırıcısı başvuru herhangi bir plan silmeniz gerekir.

5. Yönetici, tüm SKU'ları ve SQL kaynak sağlayıcısı bağdaştırıcıya ilişkili kotaları silmeniz gerekir.

6. Dağıtım betiği ile yeniden parametresi, Azure Resource Manager uç noktaları, DirectoryTenantID ve hizmet yönetici hesabının kimlik bilgilerini kaldırın.


## <a name="next-steps"></a>Sonraki adımlar

[Barındırma sunucuları ekleme](azure-stack-sql-resource-provider-hosting-servers.md) ve [oluşturma veritabanları](azure-stack-sql-resource-provider-databases.md).

Diğer deneyin [PaaS Hizmetleri](azure-stack-tools-paas-services.md) gibi [MySQL Server Kaynak sağlayıcısı](azure-stack-mysql-resource-provider-deploy.md) ve [uygulama hizmetleri kaynak sağlayıcı](azure-stack-app-service-overview.md).
