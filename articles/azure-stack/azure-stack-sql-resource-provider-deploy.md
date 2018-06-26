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
ms.date: 06/25/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: e1505761a0bd1ea9dabdd0b2cbab7af902198311
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36938341"
---
# <a name="deploy-the-sql-server-resource-provider-on-azure-stack"></a>SQL Server Kaynak sağlayıcısı Azure yığında dağıtma

SQL veritabanları Azure yığın hizmet olarak kullanıma sunmak için Azure yığın SQL Server Kaynak sağlayıcısı kullanın. SQL kaynak sağlayıcısı, bir Windows Server 2016 Server Core sanal makinede (VM) bir hizmet olarak çalışır.

## <a name="prerequisites"></a>Önkoşullar

Azure yığın SQL kaynak sağlayıcısı dağıtmadan önce karşılanması gereken birkaç önkoşul vardır. Bu gereksinimleri karşılamak için VM ayrıcalıklı uç noktasına erişebildiğinden bir bilgisayarda aşağıdaki adımları tamamlayın:

- Bunu zaten bunu yapmadıysanız [Azure yığın kaydetmek](.\azure-stack-registration.md) Azure Market öğesi indirebilmesi için Azure ile.
- Gerekli Windows Server çekirdek VM yükleyerek Azure yığın Market eklemek **Windows Server 2016 Datacenter - Sunucu Çekirdeği** görüntü. Bir komut dosyası oluşturmak için de kullanabilirsiniz bir [Windows Server 2016 görüntü](https://docs.microsoft.com/azure/azure-stack/azure-stack-add-default-image). Komut dosyasını çalıştırdığınızda Çekirdeği seçeneğini seçtiğinizden emin olun.

  >[!NOTE]
  >Bir güncelleştirme yüklemeniz gerekiyorsa, tek bir MSU paket yerel bağımlılık yolunda yerleştirebilirsiniz. Birden fazla MSU dosya bulunursa, SQL kaynak sağlayıcısı yükleme başarısız olur.

- SQL kaynak sağlayıcısı ikili indirin ve içeriğini geçici bir dizine ayıklayın ayıklayıcısı çalıştırın. Kaynak sağlayıcısı bir minimum karşılık gelen Azure yapı yığınına sahiptir. Kullanmakta olduğunuz Azure yığın sürümü için doğru ikili yüklediğinizden emin olun.

    |Azure yığın sürümü|SQL RP sürümü|
    |-----|-----|
    |Sürüm 1804 (1.0.180513.1)|[SQL RP sürüm 1.1.24.0](https://aka.ms/azurestacksqlrp1804)
    |Sürüm 1802 (1.0.180302.1)|[SQL RP sürüm 1.1.18.0](https://aka.ms/azurestacksqlrp1802)|
    |Sürüm 1712 (1.0.180102.3, 1.0.180103.2 veya 1.0.180106.1 (tümleşik sistemler için))|[SQL RP sürüm 1.1.14.0](https://aka.ms/azurestacksqlrp1712)|
    |     |     |

### <a name="certificates"></a>Sertifikalar

Yalnızca tümleşik sistemleri yüklemeleri için. İsteğe bağlı PaaS sertifikaları bölümünde anlatılan SQL PaaS PKI sertifikası sağlamalısınız [Azure yığın dağıtım PKI gereksinimleri](.\azure-stack-pki-certs.md#optional-paas-certificates). .Pfx dosyasını tarafından belirtilen konuma yerleştirin **DependencyFilesLocalPath** parametresi.

## <a name="deploy-the-sql-resource-provider"></a>SQL kaynak sağlayıcısı dağıtma

Yüklü tüm Önkoşullar kurduktan sonra çalıştırmak **DeploySqlProvider.ps1** SQL kaynak sağlayıcısı dağıtmak için komut dosyası. DeploySqlProvider.ps1 komut dosyası Azure yığın sürümünüz için yüklediğiniz SQL kaynak sağlayıcısı ikili bir parçası olarak ayıklanır.

> [!IMPORTANT]
> Komut dosyası kullanmakta olduğunuz sistemi yüklü .NET çalışma zamanı en son sürümü Windows 10 veya Windows Server 2016 sistemiyle olmalıdır.

SQL kaynak sağlayıcısı dağıtmak için açık bir **yeni** yükseltilmiş PowerShell konsol penceresinde ve SQL kaynak sağlayıcısı ikili dosyaları ayıkladığınız dizine geçin. Önceden yüklenen PowerShell modülleri tarafından neden olası sorunları önlemek için yeni bir PowerShell penceresi kullanmanızı öneririz.

Aşağıdaki görevleri tamamlar DeploySqlProvider.ps1 komut dosyasını çalıştırın:

- Sertifikaları ve diğer yapıları depolama hesabı Azure yığında yükler.
- Galeri kullanarak SQL veritabanlarını dağıtabilmek için Galeri paketleri yayımlar.
- Barındırma sunucuları dağıtmak için bir galeri paketi yayımlar.
- İndirilen ve SQL kaynak Sağlayıcısı'nı yükler Windows Server 2016 çekirdek görüntü kullanarak bir VM dağıtır.
- Kaynak sağlayıcınız VM eşler yerel bir DNS kaydı kaydeder.
- Kaynak sağlayıcısı ile yerel Azure Resource Manager işleci ve kullanıcı hesapları için kaydeder.
- İsteğe bağlı olarak, tek bir Windows Server güncelleştirme sırasında kaynak sağlayıcısı yükleme yükler.

> [!NOTE]
> SQL kaynak sağlayıcısı dağıtımı başladığında, **system.local.sqladapter** kaynak grubu oluşturulur. Bu kaynak grubu için dört gerekli dağıtımlarda son 75 dakika kadar sürebilir.

### <a name="deploysqlproviderps1-parameters"></a>DeploySqlProvider.ps1 parametreleri

Komut satırından aşağıdaki parametreleri belirtebilirsiniz. Bunu yapmazsanız veya hiçbir parametre doğrulaması başarısız olursa, gerekli parametreler sağlamanız istenir.

| Parametre adı | Açıklama | Açıklama veya varsayılan değer |
| --- | --- | --- |
| **CloudAdminCredential** | Bulut Yöneticisi, ayrıcalıklı endpoint erişmek için gerekli kimlik bilgileri. | _Gerekli_ |
| **AzCredential** | Azure yığın hizmet yönetici hesabı için kimlik bilgileri. Azure yığın dağıtmak için kullanılan kimlik bilgilerini kullanın. | _Gerekli_ |
| **VMLocalCredential** | SQL kaynak sağlayıcısının VM yerel yönetici hesabı için kimlik bilgileri. | _Gerekli_ |
| **PrivilegedEndpoint** | IP adresi veya ayrıcalıklı uç noktanın DNS adı. |  _Gerekli_ |
| **DependencyFilesLocalPath** | Sertifika .pfx dosyasının bu dizinde yerleştirilmelidir. | _İsteğe bağlı_ (_zorunlu_ tümleşik sistemler için) |
| **DefaultSSLCertificatePassword** | .Pfx sertifika parolası. | _Gerekli_ |
| **MaxRetryCount** | Bir hata olduğunda her işlemini yeniden denemek istiyor sayısı.| 2 |
| **RetryDuration** | Saniye cinsinden yeniden denemeler arasındaki zaman aşımı aralığı. | 120 |
| **Kaldırma** | (Aşağıdaki notlara bakın) ilişkili tüm kaynakları ve kaynak sağlayıcısını kaldırır. | Hayır |
| **DebugMode** | Otomatik temizleme hatasında engeller. | Hayır |

>[!NOTE]
> SKU'ları portalında görünür olması için bir saat sürebilir. SKU dağıtılmış ve çalışır olana kadar bir veritabanı oluşturulamıyor.

## <a name="deploy-the-sql-resource-provider-using-a-custom-script"></a>Özel bir komut dosyası kullanarak SQL kaynak sağlayıcısı dağıtma

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

# Change to the directory If folder where you extracted the installation files.
# Then adjust the endpoints.
. $tempDir\DeploySQLProvider.ps1 `
    -AzCredential $AdminCreds `
    -VMLocalCredential $vmLocalAdminCreds `
    -CloudAdminCredential $cloudAdminCreds `
    -PrivilegedEndpoint $privilegedEndpoint `
    -DefaultSSLCertificatePassword $PfxPass `
    -DependencyFilesLocalPath $tempDir\cert

 ```

Kaynak Sağlayıcı yükleme komut dosyası sona erdiğinde, en son güncelleştirmeleri görebildiğinizden emin olmak için tarayıcınızı yenileyin.

## <a name="verify-the-deployment-using-the-azure-stack-portal"></a>Azure yığın Portalı'nı kullanarak dağıtımı doğrulama

Aşağıdakileri kullanabilirsiniz adımları doğrulayın SQL kaynak sağlayıcısı başarıyla dağıtılır.

1. Yönetim portalında Hizmet Yöneticisi olarak oturum açın.
2. Seçin **kaynak grupları**.
3. Seçin **sistem.\< Konum\>.sqladapter** kaynak grubu.
4. İletinin altında **dağıtımları**, sonraki ekran görüntüsünde gösterilen olmalıdır **4 başarılı**.

      ![SQL kaynak sağlayıcısı dağıtımını doğrulayın](./media/azure-stack-sql-rp-deploy/sqlrp-verify.png)

5. Altında kaynak sağlayıcısı dağıtım hakkında daha ayrıntılı bilgi alabileceğiniz **ayarları**. Seçin **dağıtımları** gibi bilgileri almak için: durum, zaman damgası ve her dağıtım süre.

## <a name="next-steps"></a>Sonraki adımlar

[Barındırma sunucuları ekleme](azure-stack-sql-resource-provider-hosting-servers.md)
