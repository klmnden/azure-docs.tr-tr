---
title: Azure Stack'te MySQL veritabanlarını kullanma | Microsoft Docs
description: Azure Stack ve hızlı adımları MySQL Server Kaynak sağlayıcısı bağdaştırıcısını dağıtmak için bir hizmet olarak MySQL veritabanlarını nasıl dağıtılacağı hakkında bilgi edinin.
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
ms.date: 07/13/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: 2f5661ddac16a3024335bd633623f7ada2fc5870
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2018
ms.locfileid: "42056926"
---
# <a name="deploy-the-mysql-resource-provider-on-azure-stack"></a>Azure Stack'te MySQL kaynak sağlayıcısı dağıtma

MySQL veritabanları Azure Stack hizmet olarak kullanıma sunmak için MySQL Server Kaynak Sağlayıcısı'nı kullanın. MySQL kaynak sağlayıcısı, bir hizmet olarak Windows Server 2016 Server Core sanal makinede (VM) çalışır.

## <a name="prerequisites"></a>Önkoşullar

Azure Stack MySQL kaynak sağlayıcısını dağıtmadan önce karşılanması gereken birkaç önkoşul vardır. Bu gereksinimleri karşılamak için VM ayrıcalıklı uç noktasına erişebildiğinden bir bilgisayarda bu makaledeki adımları tamamlayın.

* Bunu zaten bunu yapmadıysanız [Azure Stack kayıt](.\azure-stack-registration.md) Azure Market öğeleri indirebilmesi Azure ile.
* Azure ve Azure Stack PowerShell modülleri bu yüklemeyi çalıştıracağınız sisteme yüklemeniz gerekir. Bu sistem, .NET çalışma zamanının en son sürümünü içeren bir Windows 10 veya Windows Server 2016 görüntüsü olması gerekir. Bkz: [Azure Stack için PowerShell yükleme](.\azure-stack-powershell-install.md).
* Gerekli Windows Server core VM için Azure Stack marketini indirerek ekleme **Windows Server 2016 Datacenter - Sunucu Çekirdeği** görüntü.

* İkili MySQL kaynak Sağlayıcısı'nı indirin ve geçici bir dizine içeriğini ayıklamak için ayıklayıcısı çalıştırın.

  >[!NOTE]
  >Internet erişimi olmayan bir sisteme MySQL sağlayıcısı dağıtmak için kopyalama [mysql-connector-net-6.10.5.msi](https://dev.mysql.com/get/Downloads/Connector-Net/mysql-connector-net-6.10.5.msi) dosyasının yerel yolu. Yol adını kullanarak sağlamak **DependencyFilesLocalPath** parametresi.

* Kaynak sağlayıcısı bir minimum karşılık gelen Azure yapı yığınına sahiptir. Kullanmakta olduğunuz Azure Stack sürümü için doğru ikili yüklediğinizden emin olun:

    | Azure Stack sürümü | RP MySQL sürümü|
    | --- | --- |
    | Sürüm 1804 (1.0.180513.1)|[MySQL RP 1.1.24.0 sürümü](https://aka.ms/azurestackmysqlrp1804) |
    | Sürüm 1802 (1.0.180302.1) | [MySQL RP 1.1.18.0 sürümü](https://aka.ms/azurestackmysqlrp1802)|
    |     |     |

- Veri Merkezi tümleştirmesi önkoşulların karşılandığından emin olun:

    |Önkoşul|Başvuru|
    |-----|-----|
    |DNS koşullu iletme doğru şekilde ayarlanır.|[Azure Stack veri merkezi tümleştirmesi - DNS](azure-stack-integrate-dns.md)|
    |Kaynak sağlayıcıları için gelen bağlantı noktaları açıktır.|[Azure Stack veri merkezi tümleştirmesi - uç noktalarını yayımlama](azure-stack-integrate-endpoints.md#ports-and-protocols-inbound)|
    |PKI sertifika konusu ve SAN doğru şekilde ayarlayın.|[Azure Stack dağıtım zorunlu PKI önkoşulları](azure-stack-pki-certs.md#mandatory-certificates)<br>[Azure Stack dağıtım PaaS sertifika önkoşulları](azure-stack-pki-certs.md#optional-paas-certificates)|
    |     |     |

### <a name="certificates"></a>Sertifikalar

_Tümleşik sistemler yüklemeleri_. İsteğe bağlı PaaS sertifikaları bölümünde açıklanan SQL PaaS PKI sertifikasını sağlamalısınız [Azure Stack dağıtım PKI gereksinimleri](.\azure-stack-pki-certs.md#optional-paas-certificates). .Pfx dosyası tarafından belirtilen konuma yerleştirin **DependencyFilesLocalPath** parametresi. Bir sertifika ASDK sistemler için sağlamaz.

## <a name="deploy-the-resource-provider"></a>Kaynak sağlayıcısı dağıtma

Tümünde Önkoşullar kendinizi çalıştırılmak **DeployMySqlProvider.ps1** MYSQL kaynak sağlayıcısı dağıtma için betiği. DeployMySqlProvider.ps1 betiği, Azure Stack sürümünüz için indirdiğiniz MySQL kaynak sağlayıcısı ikili bir parçası olarak ayıklanır.

MySQL kaynak sağlayıcısı dağıtma için yeni yükseltilmiş bir PowerShell penceresi (PowerShell ISE değil) açın ve MySQL kaynak sağlayıcısı ikili dosyaları ayıkladığınız dizine geçin. Zaten yüklenmiş olan PowerShell modülleri tarafından kaynaklanan olası sorunları önlemek için yeni bir PowerShell penceresi kullanmanızı öneririz.

Çalıştırma **DeployMySqlProvider.ps1** komut dosyası aşağıdaki görevleri tamamlar:

* Sertifikaları ve diğer yapıtları Azure Stack'te bir depolama hesabına yükler.
* MySQL veritabanları galerisini kullanarak dağıtabilmeniz adına o galeri paketleri yayımlar.
* Barındırma sunucuları dağıtmak için bir galeri paketi yayımlar.
* İndirilen ve MySQL kaynak Sağlayıcısı'nı yükleyen Windows Server 2016 temel görüntüyü kullanarak bir VM dağıtır.
* Kaynak sağlayıcınızı VM eşler yerel bir DNS kaydı kaydeder.
* Kaynak sağlayıcınız ile yerel Azure Resource Manager işleci hesabı için kaydeder.

> [!NOTE]
> MySQL kaynak sağlayıcısı dağıtım başladığında, **system.local.mysqladapter** kaynak grubu oluşturulur. Bu, bu kaynak grubuna gerekli dağıtımlar son 75 dakika kadar sürebilir.

### <a name="deploymysqlproviderps1-parameters"></a>DeployMySqlProvider.ps1 parametreleri

Komut satırından bu parametreleri belirtebilirsiniz. Yoksa veya herhangi bir parametre doğrulaması başarısız olursa, gerekli parametreleri sağlamanız istenir.

| Parametre adı | Açıklama | Açıklama veya varsayılan değeri |
| --- | --- | --- |
| **CloudAdminCredential** | Ayrıcalıklı uç nokta erişmek için gerekli bulut Yöneticisi kimlik bilgileri. | _Gerekli_ |
| **AzCredential** | Azure Stack hizmet yönetici hesabının kimlik bilgileri. Azure Stack dağıtmak için kullanılan kimlik bilgilerini kullanın. | _Gerekli_ |
| **VMLocalCredential** | MySQL kaynak sağlayıcısı VM yerel yönetici hesabı için kimlik bilgileri. | _Gerekli_ |
| **PrivilegedEndpoint** | Ayrıcalıklı uç noktasının DNS adı veya IP adresi. |  _Gerekli_ |
| **DependencyFilesLocalPath** | Yalnızca tümleşik sistemler için sertifika .pfx dosyanızı bu dizine yerleştirilmelidir. Bağlantısı kesilmiş enviroments için indirme [mysql-connector-net-6.10.5.msi](https://dev.mysql.com/get/Downloads/Connector-Net/mysql-connector-net-6.10.5.msi) bu dizine. İsteğe bağlı olarak bir Windows güncelleştirmesi MSU paket buraya kopyalayabilirsiniz. | _İsteğe bağlı_ (_zorunlu_ tümleşik sistemler veya bağlantısı kesilmiş ortamlarda) |
| **DefaultSSLCertificatePassword** | .Pfx sertifika için parola. | _Gerekli_ |
| **MaxRetryCount** | Her işlem bir hata olursa yeniden denemek istiyor sayısı.| 2 |
| **RetryDuration** | Saniye cinsinden yeniden denemeler arasındaki zaman aşımı aralığı. | 120 |
| **Kaldırma** | (Aşağıdaki notlara bakın) ilişkili tüm kaynakları ve kaynak sağlayıcısını kaldırır. | Hayır |
| **DebugMode** | Otomatik temizleme başarısız engeller. | Hayır |
| **AcceptLicense** | GPL lisansı kabul etmek için istemi atlar.  <http://www.gnu.org/licenses/old-licenses/gpl-2.0.html> | |

## <a name="deploy-the-mysql-resource-provider-using-a-custom-script"></a>Özel bir betik kullanarak MySQL kaynak sağlayıcısı dağıtma

Kaynak sağlayıcısı dağıtırken herhangi bir el ile yapılandırma ortadan kaldırmak için aşağıdaki betiği özelleştirebilirsiniz. Varsayılan hesap bilgilerini ve parolaları Azure Stack dağıtımınız için gereken şekilde değiştirin.

```powershell
# Install the AzureRM.Bootstrapper module, set the profile and install the AzureStack module
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2017-03-09-profile
Install-Module -Name AzureStack -RequiredVersion 1.4.0

# Use the NetBIOS name for the Azure Stack domain. On the Azure Stack SDK, the default is AzureStack but could have been changed at install time.
$domain = "AzureStack"  

# For integrated systems, use the IP address of one of the ERCS virtual machines
$privilegedEndpoint = "AzS-ERCS01"

# Point to the directory where the resource provider installation files were extracted.
$tempDir = 'C:\TEMP\MYSQLRP'

# The service admin account (can be Azure Active Directory or Active Directory Federation Services).
$serviceAdmin = "admin@mydomain.onmicrosoft.com"
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass)

# Set the credentials for the new resource provider VM local administrator account
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("mysqlrpadmin", $vmLocalAdminPass)

# And the cloudadmin credential required for privileged endpoint access.
$CloudAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass)

# Change the following as appropriate.
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force

# Change to the directory folder where you extracted the installation files. Do not provide a certificate on ASDK!
. $tempDir\DeployMySQLProvider.ps1 `
    -AzCredential $AdminCreds `
    -VMLocalCredential $vmLocalAdminCreds `
    -CloudAdminCredential $cloudAdminCreds `
    -PrivilegedEndpoint $privilegedEndpoint `
    -DefaultSSLCertificatePassword $PfxPass `
    -DependencyFilesLocalPath $tempDir\cert `
    -AcceptLicense

```

Kaynak Sağlayıcı yükleme betiği sona erdiğinde, en son güncelleştirmeleri gördüğünüz emin olmak için tarayıcınızı yenileyin.

## <a name="verify-the-deployment-by-using-the-azure-stack-portal"></a>Azure Stack portalını kullanarak dağıtımı doğrulama

1. Yönetim portalına Hizmet Yöneticisi olarak oturum açın.
2. Seçin **kaynak grupları**
3. Seçin **sistem.\< Konum\>.mysqladapter** kaynak grubu.
4. Kaynak grubu genel bakış için Özet sayfasında, hiçbir dağıtımları başarısız olmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

[Barındırma sunucuları ekleme](azure-stack-mysql-resource-provider-hosting-servers.md)
