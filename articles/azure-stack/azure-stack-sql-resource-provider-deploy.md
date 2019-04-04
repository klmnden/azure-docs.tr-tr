---
title: Azure Stack'te SQL veritabanlarını kullanma | Microsoft Docs
description: Azure Stack ve hızlı adımları SQL Server Kaynak sağlayıcısı bağdaştırıcısını dağıtmak için bir hizmet olarak SQL veritabanlarını nasıl dağıtılacağı hakkında bilgi edinin.
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
ms.date: 03/29/2019
ms.lastreviewed: 03/18/2019
ms.author: jeffgilb
ms.reviewer: jiahan
ms.openlocfilehash: ee64106c97a07e1b3ceb84c4ca932b19bc6d83b8
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58652612"
---
# <a name="deploy-the-sql-server-resource-provider-on-azure-stack"></a>Azure Stack'te SQL Server Kaynak sağlayıcısı dağıtma

SQL veritabanları Azure Stack hizmet olarak kullanıma sunmak için Azure Stack SQL Server Kaynak Sağlayıcısı'nı kullanın. SQL kaynak sağlayıcısı, bir hizmet olarak Windows Server 2016 Server Core sanal makinede (VM) çalışır.

> [!IMPORTANT]
> Yalnızca kaynak sağlayıcısı, ana bilgisayar SQL veya MySQL sunucuları üzerinde öğeleri oluşturmak için desteklenir. Kaynak sağlayıcısı tarafından oluşturulmamış bir ana bilgisayar sunucusunda oluşturulan öğeler, eşleşmeyen bir duruma neden olabilir.

## <a name="prerequisites"></a>Önkoşullar

Azure Stack SQL kaynak sağlayıcısını dağıtmadan önce karşılanması gereken birkaç önkoşul vardır. Bu gereksinimleri karşılamak için VM ayrıcalıklı uç noktasına erişebildiğinden bir bilgisayarda aşağıdaki adımları tamamlayın:

- Bunu zaten bunu yapmadıysanız [Azure Stack kayıt](azure-stack-registration.md) Azure Market öğeleri indirebilmesi Azure ile.
- Azure ve Azure Stack PowerShell modülleri bu yüklemeyi çalıştıracağınız sisteme yüklemeniz gerekir. Bu sistem, .NET çalışma zamanının en son sürümünü içeren bir Windows 10 veya Windows Server 2016 görüntüsü olması gerekir. Bkz: [Azure Stack için PowerShell yükleme](./azure-stack-powershell-install.md).
- Gerekli Windows Server core VM için Azure Stack marketini indirerek ekleme **Windows Server 2016 Datacenter - Sunucu Çekirdeği** görüntü.
- İkili SQL kaynak Sağlayıcısı'nı indirin ve geçici bir dizine içeriğini ayıklamak için ayıklayıcısı çalıştırın. Kaynak sağlayıcısı bir minimum karşılık gelen Azure yapı yığınına sahiptir.

  |Azure Stack en düşük sürüm|SQL RP sürümü|
  |-----|-----|
  |Sürüm 1808 (1.1808.0.97)|[SQL RP 1.1.33.0 sürümü](https://aka.ms/azurestacksqlrp11330)|  
  |Sürüm 1808 (1.1808.0.97)|[SQL RP 1.1.30.0 sürümü](https://aka.ms/azurestacksqlrp11300)|
  |Sürüm 1804 (1.0.180513.1)|[SQL RP 1.1.24.0 sürümü](https://aka.ms/azurestacksqlrp11240)
  |     |     |

- Veri Merkezi tümleştirmesi önkoşulların karşılandığından emin olun:

    |Önkoşul|Başvuru|
    |-----|-----|
    |DNS koşullu iletme doğru şekilde ayarlanır.|[Azure Stack veri merkezi tümleştirmesi - DNS](azure-stack-integrate-dns.md)|
    |Kaynak sağlayıcıları için gelen bağlantı noktaları açıktır.|[Azure Stack veri merkezi tümleştirmesi - uç noktalarını yayımlama](azure-stack-integrate-endpoints.md#ports-and-protocols-inbound)|
    |PKI sertifika konusu ve SAN doğru şekilde ayarlayın.|[Azure Stack dağıtım zorunlu PKI önkoşulları](azure-stack-pki-certs.md#mandatory-certificates)[Azure Stack dağıtım PaaS sertifika önkoşulları](azure-stack-pki-certs.md#optional-paas-certificates)|
    |     |     |

### <a name="certificates"></a>Sertifikalar

_Tümleşik sistemler yüklemeleri_. İsteğe bağlı PaaS sertifikaları bölümünde açıklanan SQL PaaS PKI sertifikasını sağlamalısınız [Azure Stack dağıtım PKI gereksinimleri](./azure-stack-pki-certs.md#optional-paas-certificates). .Pfx dosyası tarafından belirtilen konuma yerleştirin **DependencyFilesLocalPath** parametresi. Bir sertifika ASDK sistemler için sağlamaz.

## <a name="deploy-the-sql-resource-provider"></a>SQL kaynak sağlayıcısı dağıtma

Tüm Önkoşullar yükledikten sonra çalıştırabileceğiniz **DeploySqlProvider.ps1** SQL kaynak sağlayıcısı dağıtma için betiği. DeploySqlProvider.ps1 betiği, Azure Stack sürümünüz için indirdiğiniz SQL kaynak sağlayıcısı ikili bir parçası olarak ayıklanır.

 > [!IMPORTANT]
 > Kaynak sağlayıcısını dağıtmadan önce yeni işlevler, düzeltmeler ve dağıtımınızı etkileyebilecek bilinen sorunlar hakkında bilgi edinmek için sürüm notlarını gözden geçirin.
 
SQL kaynak sağlayıcısı dağıtmak için açık bir **yeni** yükseltilmiş bir PowerShell penceresi (PowerShell ISE değil) ve SQL kaynak sağlayıcısı ikili dosyaları ayıkladığınız dizine. Zaten yüklenmiş olan PowerShell modülleri tarafından kaynaklanan olası sorunları önlemek için yeni bir PowerShell penceresi kullanmanızı öneririz.

Aşağıdaki görevleri tamamlar DeploySqlProvider.ps1 betiği çalıştırın:

- Sertifikaları ve diğer yapıtları Azure Stack'te bir depolama hesabına yükler.
- Galeriyi kullanarak SQL veritabanlarını ucunuzun galeri paketleri yayımlar.
- Barındırma sunucuları dağıtmak için bir galeri paketi yayımlar.
- İndirilen ve SQL kaynak Sağlayıcısı'nı yükleyen Windows Server 2016 temel görüntüyü kullanarak bir VM dağıtır.
- Kaynak sağlayıcınızı VM eşler yerel bir DNS kaydı kaydeder.
- Kaynak sağlayıcınız ile yerel Azure Resource Manager işleci hesabı için kaydeder.

> [!NOTE]
> SQL kaynak sağlayıcısı dağıtım başladığında, **system.local.sqladapter** kaynak grubu oluşturulur. Bu, gerekli dağıtımlarda bu kaynak grubu için son 75 dakika kadar sürebilir.

### <a name="deploysqlproviderps1-parameters"></a>DeploySqlProvider.ps1 parametreleri

Komut satırından aşağıdaki parametreleri belirtebilirsiniz. Yoksa veya herhangi bir parametre doğrulaması başarısız olursa, gerekli parametreleri sağlamanız istenir.

| Parametre adı | Açıklama | Açıklama veya varsayılan değeri |
| --- | --- | --- |
| **CloudAdminCredential** | Ayrıcalıklı uç nokta erişmek için gerekli bulut Yöneticisi kimlik bilgileri. | _Gerekli_ |
| **AzCredential** | Azure Stack hizmet yönetici hesabının kimlik bilgileri. Azure Stack dağıtmak için kullanılan kimlik bilgilerini kullanın. | _Gerekli_ |
| **VMLocalCredential** | SQL kaynak sağlayıcısı VM yerel yönetici hesabı için kimlik bilgileri. | _Gerekli_ |
| **PrivilegedEndpoint** | Ayrıcalıklı uç noktasının DNS adı veya IP adresi. |  _Gerekli_ |
| **AzureEnvironment** | Azure Stack dağıtmak için kullanılan hizmet yönetici hesabının Azure ortamı. Yalnızca Azure AD dağıtımları için gereklidir. Desteklenen ortam adları **AzureCloud**, **AzureUSGovernment**, veya bir Çin'de Azure Active Directory'yi kullanarak **AzureChinaCloud**. | AzureCloud |
| **DependencyFilesLocalPath** | Yalnızca tümleşik sistemler için sertifika .pfx dosyanızı bu dizine yerleştirilmelidir. İsteğe bağlı olarak bir Windows güncelleştirmesi MSU paket buraya kopyalayabilirsiniz. | _İsteğe bağlı_ (_zorunlu_ tümleşik sistemler için) |
| **DefaultSSLCertificatePassword** | .Pfx sertifika için parola. | _Gerekli_ |
| **MaxRetryCount** | Her işlem bir hata olursa yeniden denemek istiyor sayısı.| 2 |
| **RetryDuration** | Saniye cinsinden yeniden denemeler arasındaki zaman aşımı aralığı. | 120 |
| **Kaldırma** | (Aşağıdaki notlara bakın) ilişkili tüm kaynakları ve kaynak sağlayıcısını kaldırır. | Hayır |
| **DebugMode** | Otomatik temizleme başarısız engeller. | Hayır |

## <a name="deploy-the-sql-resource-provider-using-a-custom-script"></a>Özel bir komut dosyası kullanarak SQL kaynak sağlayıcısı dağıtma

Kaynak sağlayıcısı dağıtırken herhangi bir el ile yapılandırma ortadan kaldırmak için aşağıdaki betiği özelleştirebilirsiniz.  

Varsayılan hesap bilgilerini ve parolaları Azure Stack dağıtımınız için gereken şekilde değiştirin.


```powershell
# Install the AzureRM.Bootstrapper module, set the profile and install the AzureStack module
# Note that this might not be the most currently available version of Azure Stack PowerShell
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2018-03-01-hybrid -Force
Install-Module -Name AzureStack -RequiredVersion 1.6.0

# Use the NetBIOS name for the Azure Stack domain. On the Azure Stack SDK, the default is AzureStack but could have been changed at install time.
$domain = "AzureStack"

# For integrated systems, use the IP address of one of the ERCS virtual machines
$privilegedEndpoint = "AzS-ERCS01"

# Provide the Azure environment used for deploying Azure Stack. Required only for Azure AD deployments. Supported values for the <environment name> parameter are AzureCloud, AzureChinaCloud or AzureUSGovernment depending which Azure subscription you are using. 
$AzureEnvironment = "<EnvironmentName>"

# Point to the directory where the resource provider installation files were extracted.
$tempDir = 'C:\TEMP\SQLRP'

# The service admin account can be Azure Active Directory or Active Directory Federation Services.
$serviceAdmin = "admin@mydomain.onmicrosoft.com"
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass)

# Set credentials for the new resource provider VM local administrator account.
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("sqlrpadmin", $vmLocalAdminPass)

# Add the cloudadmin credential that's required for privileged endpoint access.
$CloudAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass)

# Change the following as appropriate.
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force

# Clear the existing login information from the Azure PowerShell context.
Clear-AzureRMContext -Scope CurrentUser -Force
Clear-AzureRMContext -Scope Process -Force

# Change to the directory folder where you extracted the installation files. Do not provide a certificate on ASDK!
. $tempDir\DeploySQLProvider.ps1 `
    -AzCredential $AdminCreds `
    -VMLocalCredential $vmLocalAdminCreds `
    -CloudAdminCredential $cloudAdminCreds `
    -PrivilegedEndpoint $privilegedEndpoint `
    -AzureEnvironment $AzureEnvironment `
    -DefaultSSLCertificatePassword $PfxPass `
    -DependencyFilesLocalPath $tempDir\cert

 ```

Kaynak Sağlayıcı yükleme betiği sona erdiğinde, en son güncelleştirmeleri gördüğünüz emin olmak için tarayıcınızı yenileyin.

## <a name="verify-the-deployment-using-the-azure-stack-portal"></a>Azure Stack portalını kullanarak dağıtımı doğrulama

Aşağıdaki kullanabileceğiniz adımları SQL kaynak sağlayıcısı başarıyla dağıtıldığından emin olun.

1. Yönetim portalına Hizmet Yöneticisi olarak oturum açın.
2. Seçin **kaynak grupları**.
3. Seçin **sistem.\< Konum\>.sqladapter** kaynak grubu.
4. Kaynak grubu genel bakış için Özet sayfasında, hiçbir dağıtımları başarısız olmalıdır.
      ![SQL kaynak sağlayıcısı, dağıtımı doğrulama](./media/azure-stack-sql-rp-deploy/sqlrp-verify.png)
5. Son olarak, seçin **sanal makineler** doğrulamak için Yönetim Portalı'nda SQL kaynak sağlayıcısı sanal makine başarıyla oluşturuldu ve çalışıyor.

## <a name="next-steps"></a>Sonraki adımlar

[Barındırma sunucuları ekleme](azure-stack-sql-resource-provider-hosting-servers.md)
