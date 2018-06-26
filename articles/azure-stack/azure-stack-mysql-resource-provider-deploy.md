---
title: MySQL veritabanları Azure yığında kullanarak | Microsoft Docs
description: Azure yığını ve hızlı adımlar MySQL Server Kaynak sağlayıcısı bağdaştırıcısı dağıtmak için bir hizmet olarak MySQL veritabanları nasıl dağıtılacağı hakkında bilgi edinin.
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
ms.date: 06/25/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: e4c3eb1d7dfd4894576d5fbed52cf4de5fed9e44
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36938126"
---
# <a name="deploy-the-mysql-resource-provider-on-azure-stack"></a>MySQL kaynak sağlayıcı Azure yığında dağıtma

MySQL veritabanları Azure yığın hizmet olarak kullanıma sunmak için MySQL Server Kaynak sağlayıcısı kullanın. MySQL kaynak sağlayıcısı, bir Windows Server 2016 Server Core sanal makinede (VM) bir hizmet olarak çalışır.

## <a name="prerequisites"></a>Önkoşullar

Azure yığın MySQL kaynak sağlayıcı dağıtmadan önce karşılanması gereken birkaç önkoşul vardır. Bu gereksinimleri karşılamak için VM ayrıcalıklı uç noktasına erişebildiğinden bir bilgisayarda bu makaledeki adımları tamamlayın.

* Bunu zaten bunu yapmadıysanız [Azure yığın kaydetmek](.\azure-stack-registration.md) Azure Market öğesi indirebilmesi için Azure ile.
* Gerekli Windows Server çekirdek VM yükleyerek Azure yığın Market eklemek **Windows Server 2016 Datacenter - Sunucu Çekirdeği** görüntü. Bir komut dosyası oluşturmak için de kullanabilirsiniz bir [Windows Server 2016 görüntü](https://docs.microsoft.com/azure/azure-stack/azure-stack-add-default-image). Komut dosyasını çalıştırdığınızda Çekirdeği seçeneğini seçtiğinizden emin olun.

  >[!NOTE]
  >Bir güncelleştirme yüklemeniz gerekiyorsa, tek bir yerleştirebilirsiniz. Yerel bağımlılık yolundaki MSU paketi. Birden fazla ise. MSU dosyası bulundu, MySQL kaynak sağlayıcısı yükleme başarısız olur.

* MySQL kaynak sağlayıcı ikili indirin ve içeriğini geçici bir dizine ayıklayın ayıklayıcısı çalıştırın.

  >[!NOTE]
  >Internet erişimi olmayan bir sistemde MySQL sağlayıcı dağıtmak için kopyalama [bağlayıcı net 6.10.5.msi mysql](https://dev.mysql.com/get/Download/sConnector-Net/mysql-connector-net-6.10.5.msi) dosyasını bir yerel paylaşıma. Bunun için istendiğinde paylaşım adı belirtin. Azure ve Azure yığın PowerShell modülleri yüklemeniz gerekir.

* Kaynak sağlayıcısı bir minimum karşılık gelen Azure yapı yığınına sahiptir. Kullanmakta olduğunuz Azure yığın sürümü için doğru ikili yüklediğinizden emin olun.

    | Azure yığın sürümü | MySQL RP sürümü|
    | --- | --- |
    | Sürüm 1804 (1.0.180513.1)|[MySQL RP sürüm 1.1.24.0](https://aka.ms/azurestackmysqlrp1804) |
    | Sürüm 1802 (1.0.180302.1) | [MySQL RP sürüm 1.1.18.0](https://aka.ms/azurestackmysqlrp1802) |
    | Sürüm 1712 (1.0.180102.3 veya 1.0.180106.1 (tümleşik sistemler için)) | [MySQL RP sürüm 1.1.14.0](https://aka.ms/azurestackmysqlrp1712) |

### <a name="certificates"></a>Sertifikalar

ASDK için bu yükleme işleminin bir parçası olarak otomatik olarak imzalanan bir sertifika oluşturulur. Bir Azure tümleşik yığını sistemi için uygun bir sertifika sağlamanız gerekir. Kendi sertifikanızı sağlamanız gerekiyorsa, bir .pfx dosyasına yerleştirin **DependencyFilesLocalPath** ve şu ölçütlere uymalıdır:

* İçin bir joker karakter sertifika \*.dbadapter.\< Bölge\>.\< Dış fqdn\> veya mysqladapter.dbadapter bir ortak adı tek bir site sertifikayla.\< Bölge\>.\< Dış fqdn\>.
* Sertifika güvenilir olması gerekir. Güven zinciri ara sertifika gerektirmeden mevcut olması gerekir.
* Yalnızca tek bir sertifika dosyası DependencyFilesLocalPath bulunmaktadır.
* Dosya adı, herhangi bir özel karakter veya boşluk olamaz.

## <a name="deploy-the-resource-provider"></a>Kaynak sağlayıcısı dağıtma

Yüklü tüm Önkoşullar kurduktan sonra çalıştırmak **DeployMySqlProvider.ps1** MYSQL kaynak sağlayıcı dağıtmak için komut dosyası. DeployMySqlProvider.ps1 komut dosyası Azure yığın sürümünüz için yüklediğiniz MySQL kaynak sağlayıcı ikili bir parçası olarak ayıklanır.

> [!IMPORTANT]
> Komut dosyası kullanmakta olduğunuz sistemi yüklü .NET çalışma zamanı en son sürümü Windows 10 veya Windows Server 2016 sistemiyle olmalıdır.

MySQL kaynak sağlayıcı dağıtmak için yeni yükseltilmiş bir PowerShell konsol penceresi açın ve MySQL kaynak sağlayıcısı ikili dosyaları ayıkladığınız dizine geçin. Önceden yüklenen PowerShell modülleri tarafından neden olası sorunları önlemek için yeni bir PowerShell penceresi kullanmanızı öneririz.

Çalıştırma **DeployMySqlProvider.ps1** komut dosyası aşağıdaki görevleri tamamlar:

* Sertifikaları ve diğer yapıları depolama hesabı Azure yığında yükler.
* MySQL veritabanları galeriyi kullanarak dağıtabilirsiniz galeri paketleri yayınlar.
* Barındırma sunucuları dağıtmak için bir galeri paketi yayımlar.
* İndirilen ve MySQL kaynak Sağlayıcısı'nı yükler Windows Server 2016 çekirdek görüntü kullanarak bir VM dağıtır.
* Kaynak sağlayıcınız VM eşler yerel bir DNS kaydı kaydeder.
* Kaynak sağlayıcısı ile yerel Azure Resource Manager işleci ve kullanıcı hesapları için kaydeder.
* İsteğe bağlı olarak, tek bir Windows Server güncelleştirme sırasında kaynak sağlayıcısı yükleme yükler.

> [!NOTE]
> MySQL kaynak sağlayıcı dağıtım başladığında, **system.local.mysqladapter** kaynak grubu oluşturulur. Bu kaynak grubuna gereken dört dağıtımları son 75 dakika kadar sürebilir.

### <a name="deploymysqlproviderps1-parameters"></a>DeployMySqlProvider.ps1 parametreleri

Komut satırından bu parametreleri belirtebilirsiniz. Bunu yapmazsanız veya hiçbir parametre doğrulaması başarısız olursa, gerekli parametreler sağlamanız istenir.

| Parametre adı | Açıklama | Açıklama veya varsayılan değer |
| --- | --- | --- |
| **CloudAdminCredential** | Bulut Yöneticisi, ayrıcalıklı endpoint erişmek için gerekli kimlik bilgileri. | _Gerekli_ |
| **AzCredential** | Azure yığın hizmet yönetici hesabı için kimlik bilgileri. Azure yığın dağıtmak için kullanılan kimlik bilgilerini kullanın. | _Gerekli_ |
| **VMLocalCredential** | MySQL kaynak sağlayıcı VM yerel yönetici hesabı için kimlik bilgileri. | _Gerekli_ |
| **PrivilegedEndpoint** | IP adresi veya ayrıcalıklı uç noktanın DNS adı. |  _Gerekli_ |
| **DependencyFilesLocalPath** | İçeren bir yerel paylaşıma yoluna [bağlayıcı net 6.10.5.msi mysql](https://dev.mysql.com/get/Downloads/Connector-Net/mysql-connector-net-6.10.5.msi). Bu yollarından biri, sağlarsanız, sertifika dosyasının bu dizinde yerleştirilmelidir. | _İsteğe bağlı_ tek düğüm için _zorunlu_ çok düğümlü için. |
| **DefaultSSLCertificatePassword** | .Pfx sertifika parolası. | _Gerekli_ |
| **MaxRetryCount** | Bir hata olduğunda her işlemini yeniden denemek istiyor sayısı.| 2 |
| **RetryDuration** | Saniye cinsinden yeniden denemeler arasındaki zaman aşımı aralığı. | 120 |
| **Kaldırma** | (Aşağıdaki notlara bakın) ilişkili tüm kaynakları ve kaynak sağlayıcısını kaldırır. | Hayır |
| **DebugMode** | Otomatik temizleme hatasında engeller. | Hayır |
| **AcceptLicense** | GPL lisansı kabul etmek için istemi atlar.  <http://www.gnu.org/licenses/old-licenses/gpl-2.0.html> | |

> [!NOTE]
> SKU'ları portalında görünür olması için bir saat sürebilir. SKU dağıtılmış ve çalışır olana kadar bir veritabanı oluşturulamıyor.

## <a name="deploy-the-mysql-resource-provider-using-a-custom-script"></a>MySQL kaynak sağlayıcıyı özel bir komut dosyası kullanarak dağıtın

Kaynak sağlayıcısı dağıtırken herhangi bir el ile yapılandırma ortadan kaldırmak için aşağıdaki komut dosyası özelleştirebilirsiniz. Varsayılan hesap bilgilerini ve parolaları Azure yığın dağıtımınız için gerektiği gibi değiştirin.

```powershell
# Install the AzureRM.Bootstrapper module and set the profile.
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2017-03-09-profile

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

# Run the installation script from the folder where you extracted the installation files.
# Find the ERCS01 IP address first, and make sure the certificate file is in the specified directory.
. $tempDir\DeployMySQLProvider.ps1 `
    -AzCredential $AdminCreds `
    -VMLocalCredential $vmLocalAdminCreds `
    -CloudAdminCredential $cloudAdminCreds `
    -PrivilegedEndpoint $privilegedEndpoint `
    -DefaultSSLCertificatePassword $PfxPass `
    -DependencyFilesLocalPath $tempDir\cert `
    -AcceptLicense

```

Kaynak Sağlayıcı yükleme komut dosyası sona erdiğinde, en son güncelleştirmeleri görebildiğinizden emin olmak için tarayıcınızı yenileyin.

## <a name="verify-the-deployment-by-using-the-azure-stack-portal"></a>Azure yığın portalını kullanarak dağıtımı doğrulama

1. Yönetim portalında Hizmet Yöneticisi olarak oturum açın.
2. Seçin **kaynak grupları**
3. Seçin **sistem.\< Konum\>.mysqladapter** kaynak grubu.
4. Özet sayfasında kaynak için genel bakış, iletinin altında grup **dağıtımları** olmalıdır **3 Başarılı**.
5. Altında kaynak sağlayıcısı dağıtım hakkında daha ayrıntılı bilgi alabileceğiniz **ayarları**. Seçin **dağıtımları** gibi bilgileri almak için: durum, zaman damgası ve her dağıtım süre.

## <a name="next-steps"></a>Sonraki adımlar

[Barındırma sunucuları ekleme](azure-stack-mysql-resource-provider-hosting-servers.md)
