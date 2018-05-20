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
ms.openlocfilehash: 20b289c16a73bd20ed020987116975c8abe893f0
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="use-sql-databases-on-microsoft-azure-stack"></a>Microsoft Azure yığın üzerinde SQL veritabanları kullanın
SQL veritabanları Azure yığınının hizmet olarak kullanıma sunmak için Azure yığın SQL Server Kaynak sağlayıcısı kullanın. SQL kaynak sağlayıcısı hizmeti SQL kaynak sağlayıcısı bir Windows Server çekirdek sanal makine VM üzerinde çalışır.

## <a name="prerequisites"></a>Önkoşullar
Azure yığın SQL kaynak sağlayıcısı dağıtmadan önce karşılanması gereken birkaç önkoşul vardır. VM ayrıcalıklı uç noktasına erişebildiğinden bir bilgisayarda aşağıdaki adımları gerçekleştirin:

- Zaten bunu yapmadıysanız [Azure yığın kaydetmek](.\azure-stack-registration.md) Azure ile Azure Market öğesi indirebilmesi.
- Gerekli Windows Server çekirdek VM yükleyerek Azure yığın Market eklemek **Windows Server 2016 Sunucu Çekirdeği** görüntü. Bir güncelleştirme yüklemeniz gerekiyorsa, tek bir yerleştirebilirsiniz. Yerel bağımlılık yolundaki MSU paketi. Birden fazla ise. MSU dosyası bulundu, SQL kaynak sağlayıcısı yükleme başarısız olur.
- SQL kaynak sağlayıcısı ikili indirin ve içeriğini geçici bir dizine ayıklayın ayıklayıcısı çalıştırın. Kaynak sağlayıcısı bir minimum karşılık gelen Azure yapı yığınına sahiptir. Kullanmakta olduğunuz Azure yığın sürümü için doğru ikili yüklediğinizden emin olun:
    - Azure yığın sürüm 1802 (1.0.180302.1): [SQL RP sürüm 1.1.18.0](https://aka.ms/azurestacksqlrp1802).
    - Azure yığın sürüm 1712 (1.0.180102.3, 1.0.180103.2 veya 1.0.180106.1 (tümleşik sistemler için)): [SQL RP sürüm 1.1.14.0](https://aka.ms/azurestacksqlrp1712).
- Yalnızca tümleşik sistemleri yüklemeler için SQL PaaS PKI sertifikası isteğe bağlı PaaS sertifikaları bölümünde açıklandığı gibi sağlamalısınız [Azure yığın dağıtım PKI gereksinimleri](.\azure-stack-pki-certs.md#optional-paas-certificates), .pfx dosyasını konuma yerleştirerek tarafından belirtilen **DependencyFilesLocalPath** parametresi.
- Sahip olduğundan emin olun [Azure yığın PowerShell'in en son sürümünü](.\azure-stack-powershell-install.md) (v1.2.11) yüklü. 

## <a name="deploy-the-sql-resource-provider"></a>SQL kaynak sağlayıcısı dağıtma
Tüm Önkoşullar toplantı göre SQL kaynak Sağlayıcısı'nı yüklemek başarıyla hazırladıktan sonra artık çalıştırabileceğiniz **DeploySqlProvider.ps1** SQL kaynak sağlayıcısı dağıtmak için komut dosyası. DeploySqlProvider.ps1 komut dosyası Azure yığın sürüme karşılık gelen yüklediğiniz SQL kaynak sağlayıcısı ikili bir parçası olarak ayıklanır. 

> [!IMPORTANT]
> Komut dosyası çalıştırdığı sistem yüklü .NET çalışma zamanı en son sürümü Windows 10 veya Windows Server 2016 sistemiyle olması gerekir.


SQL kaynak sağlayıcısı dağıtmak için (Yönetici) yeni yükseltilmiş bir PowerShell konsolu açın ve SQL kaynak sağlayıcısı ikili dosyaları ayıkladığınız dizine geçin.

> [!NOTE]
> Sistemde zaten yüklü olan yanlış PowerShell modüllerden doğabilecek sorunları önlemek için yeni bir PowerShell konsol penceresi kullanın.

Aşağıdaki adımları gerçekleştirir DeploySqlProvider.ps1 komut dosyasını çalıştırın:
- Sertifikaları ve diğer yapıları depolama hesabı Azure yığında yükler.
- SQL veritabanları Galerisine dağıtabilirsiniz galeri paketleri yayınlar.
- Barındırma sunucuları dağıtmak için bir galeri paketi yayımlar.
- 1. adımda oluşturulan ve ardından kaynak sağlayıcısı yükleyen Windows Server 2016 görüntüsünü kullanarak bir VM dağıtır.
- Kaynak sağlayıcınız VM eşler yerel bir DNS kaydı kaydeder.
- Kaynak sağlayıcısı yerel Azure Resource Manager ile (kullanıcı ve yönetim) kaydeder.
- İsteğe bağlı olarak tek bir Windows güncelleştirmesi RP yükleme sırasında yükler.

SQL kaynak sağlayıcısı dağıtımı başlar ve system.local.sqladapter kaynak grubu oluşturur. Bu kaynak grubu için dört gerekli dağıtımlarda tamamlanması 75 dakika sürebilir.

### <a name="deploysqlproviderps1-parameters"></a>DeploySqlProvider.ps1 parametreleri
Komut satırında bu parametreleri belirtebilirsiniz. Bunu yapmazsanız veya hiçbir parametre doğrulaması başarısız olursa, gerekli parametreler sağlamanız istenir.

| Parametre adı | Açıklama | Açıklama veya varsayılan değer |
| --- | --- | --- |
| **CloudAdminCredential** | Bulut Yöneticisi, ayrıcalıklı endpoint erişmek için gerekli kimlik bilgileri. | _Gerekli_ |
| **AzCredential** | Azure yığın hizmet yönetici hesabı için kimlik bilgileri. Azure yığın dağıtmak için kullanılan kimlik bilgilerinin aynısını kullanın. | _Gerekli_ |
| **VMLocalCredential** | SQL kaynak sağlayıcısının VM yerel yönetici hesabı için kimlik bilgileri. | _Gerekli_ |
| **PrivilegedEndpoint** | IP adresi veya ayrıcalıklı uç noktanın DNS adı. |  _Gerekli_ |
| **DependencyFilesLocalPath** | Sertifika .pfx dosyasının bu dizinde yerleştirilmelidir. | _İsteğe bağlı_ (_zorunlu_ tümleşik sistemler için) |
| **DefaultSSLCertificatePassword** | .Pfx sertifika parolası. | _Gerekli_ |
| **MaxRetryCount** | Bir hata olduğunda her işlemini yeniden denemek istiyor sayısı.| 2 |
| **RetryDuration** | Saniye cinsinden yeniden denemeler arasındaki zaman aşımı aralığı. | 120 |
| **Kaldırma** | (Aşağıdaki notlara bakın) ilişkili tüm kaynakları ve kaynak sağlayıcısını kaldırır. | Hayır |
| **DebugMode** | Otomatik temizleme hatasında engeller. | Hayır |

>[!NOTE]
> SKU'ları portalında görünür olması için bir saat sürebilir. SKU oluşturulana kadar bir veritabanı oluşturulamıyor.


## <a name="deploy-the-sql-resource-provider-using-a-custom-script"></a>Özel bir komut dosyası kullanarak SQL kaynak sağlayıcısı dağıtma
DeploySqlProvider.ps1 komut dosyası çalıştığında gerekli bilgileri el ile girmeyi önlemek için aşağıdaki kod örneği varsayılan hesap bilgileri ve gerektiğinde parolaları değiştirerek özelleştirebilirsiniz:

```powershell
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

## <a name="verify-the-deployment-using-the-azure-stack-portal"></a>Azure yığın Portalı'nı kullanarak dağıtımı doğrulama
Bu bölümdeki adımları SQL kaynak sağlayıcısı başarılı bir şekilde dağıtıldı emin olmak için kullanılabilir.

> [!NOTE]
>  Yükleme komut dosyası çalışması bittikten sonra Yönetim dikey penceresinde ve galeri öğeleri görmek için portal yenilemeniz gerekir.

1. Yönetim portalında Hizmet Yöneticisi olarak oturum açın.

2. Dağıtım başarılı olduğunu doğrulayın. Git **kaynak grupları**. Ardından **sistem.\< Konum\>.sqladapter** kaynak grubu. Tüm dört dağıtımlarının başarılı olduğunu doğrulayın.

      ![SQL kaynak sağlayıcısı dağıtımını doğrulayın](./media/azure-stack-sql-rp-deploy/sqlrp-verify.png)


## <a name="next-steps"></a>Sonraki adımlar

[Barındırma sunucuları ekleme](azure-stack-sql-resource-provider-hosting-servers.md)
