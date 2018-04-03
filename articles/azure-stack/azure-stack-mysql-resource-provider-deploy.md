---
title: MySQL veritabanları PaaS Azure yığında kullanın | Microsoft Docs
description: MySQL kaynak sağlayıcı dağıtmak ve Azure yığında bir hizmet olarak MySQL veritabanları sağlar nasıl öğrenin.
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
ms.openlocfilehash: 66e1d5691b431be0c3d040570b13e8d16b1669ef
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="use-mysql-databases-on-microsoft-azure-stack"></a>Microsoft Azure yığında MySQL veritabanları kullanın

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Bir MySQL kaynak sağlayıcı Azure yığında dağıtabilirsiniz. Kaynak sağlayıcısı dağıttıktan sonra MySQL sunucuları ve Azure Resource Manager dağıtım şablonları aracılığıyla veritabanları oluşturabilirsiniz. Bu gibi durumlarda, MySQL veritabanları aynı zamanda hizmet olarak sağlayabilirsiniz. 

Web sitelerinde yaygın olarak kullanılan MySQL veritabanları, birçok Web sitesi platformları destekler. Kaynak sağlayıcısı dağıttıktan sonra Örneğin, WordPress Web siteleri Web Apps platformundan hizmet (PaaS) eklenti gibi Azure yığını için oluşturabilirsiniz.

Internet erişimi olmayan bir sistemde MySQL sağlayıcı dağıtmak için dosyayı kopyalama [bağlayıcı net 6.10.5.msi mysql](https://dev.mysql.com/get/Download/sConnector-Net/mysql-connector-net-6.10.5.msi) bir yerel paylaşıma. Bunun için istendiğinde bu paylaşım adı belirtin. Azure ve Azure yığın PowerShell modülleri yüklemeniz gerekir.


## <a name="mysql-server-resource-provider-adapter-architecture"></a>MySQL Server Kaynak sağlayıcısı bağdaştırıcısı mimarisi

Kaynak sağlayıcısı üç bileşenlerden oluşur:

- **MySQL kaynak sağlayıcı bağdaştırıcısı VM**, sağlayıcı hizmetlerini çalıştıran bir Windows sanal makine olduğu.

- **Kaynak sağlayıcısı**, sağlama isteklerini işler ve kullanıma sunar kaynaklarını veritabanı.

- **MySQL Server barındıran sunucular**, barındırma sunucuları adı verilen veritabanları için kapasite sağlar.

Bu sürümde artık MySQL örneklerini oluşturur. Bu, kendiniz oluşturmanız ve/veya dış SQL örnekleri erişim sağlamak gerektiği anlamına gelir. Ziyaret [Azure yığın hızlı başlama Galerisi](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/mysql-standalone-server-windows) için bir örnek şablonu için:
- MySQL sunucusu için oluşturun.
- İndirin ve MySQL Server Azure Marketi'nden dağıtın.

> [!NOTE]
> Barındırma Azure yığın üzerinde yüklü olan sunucuları tümleşik sistemleri Kiracı abonelikten oluşturulması gerekir. Varsayılan sağlayıcı abonelikten oluşturulamıyor. Bunlar, Kiracı portalından veya bir uygun oturum açma ile bir PowerShell oturumu oluşturulmalıdır. Tüm barındırma sunucuları ücrete tabi VM'ler ve uygun izinlere sahip olmalıdır. Hizmet Yöneticisi Kiracı aboneliğin sahibi olabilir.

### <a name="required-privileges"></a>Gerekli ayrıcalıklar
Sistem hesabı aşağıdaki ayrıcalıklara sahip olmalıdır:

1.  Veritabanı: Oluşturma, bırakma
2.  Oturum açma: Oluşturma, ayarlama, bırakma, vermek, iptal etme

## <a name="deploy-the-resource-provider"></a>Kaynak sağlayıcısı dağıtma

1. Zaten yapmadıysanız, Geliştirme Seti kaydolun ve Windows Server 2016 Datacenter Core görüntünün Market yönetiminden indirilebilir indirin. Bir Windows Server 2016 Core görüntüsü kullanmanız gerekir. Bir komut dosyası oluşturmak için de kullanabilirsiniz bir [Windows Server 2016 görüntü](https://docs.microsoft.com/azure/azure-stack/azure-stack-add-default-image). (Çekirdek seçeneğini belirlediğinizden emin olun.)


2. VM ayrıcalıklı uç noktasına erişebildiğinden bir ana bilgisayara oturum açın.

    - Azure SDK'sı yüklemelerinde fiziksel ana bilgisayara oturum açın. 
    - Birden çok düğümlü sistemlerde konak ayrıcalıklı uç noktasına erişebildiğinden bir sistem olmalıdır.
    
    >[!NOTE]
    > Betik çalıştırılıyor sistem *gerekir* yüklü .NET çalışma zamanı en son sürümü Windows 10 veya Windows Server 2016 sistemiyle olabilir. Aksi takdirde yükleme başarısız olur. Azure yığın SDK konak bu ölçütü karşılayan.
    

3. İkili MySQL kaynak sağlayıcıyı yükleyin. Daha sonra içeriği geçici bir dizine ayıklayın ayıklayıcısı çalıştırın.

    >[!NOTE] 
    > Kaynak sağlayıcısı bir minimum karşılık gelen Azure yapı yığınına sahiptir. Doğru ikili çalıştıran Azure yığın sürümü yüklediğinizden emin olun.

    | Azure yığın derleme | MySQL RP yükleyici |
    | --- | --- |
    | 1802: 1.0.180302.1 | [MySQL RP sürüm 1.1.18.0](https://aka.ms/azurestackmysqlrp1802) |
    | 1712: 1.0.180102.3 veya 1.0.180106.1 (çok düğümlü) | [MySQL RP sürüm 1.1.14.0](https://aka.ms/azurestackmysqlrp1712) |
    | 1711: 1.0.171122.1 | [MySQL RP sürüm 1.1.12.0](https://aka.ms/azurestackmysqlrp1711) |
    | 1710: 1.0.171028.1 | [MySQL RP sürüm 1.1.8.0](https://aka.ms/azurestackmysqlrp1710) |

4.  İçin Azure SDK, bu işlemin bir parçası olarak otomatik olarak imzalanan bir sertifika oluşturulur. Birden çok düğümlü için uygun bir sertifika sağlamanız gerekir.

    Kendi sertifikanızı sağlamanız gerekiyorsa, bir .pfx dosyasına yerleştirin **DependencyFilesLocalPath** ve şu ölçütlere uymalıdır:

    - İçin bir joker karakter sertifika \*.dbadapter.\< Bölge\>.\< Dış fqdn\> veya mysqladapter.dbadapter bir ortak adı tek bir site sertifikayla.\< Bölge\>.\< Dış fqdn\>.

    - Bu sertifikayı Güvenilir olması gerekir. Yani, güven zinciri ara sertifika gerektirmeden bulunması gerekir.

    - Yalnızca tek bir sertifika dosyası DependencyFilesLocalPath bulunmaktadır.
    
    - Dosya adı, herhangi bir özel karakter veya boşluk içermemelidir.


5. Açık bir **yeni** yükseltilmiş (Yönetim) PowerShell Konsolu. Ardından dosyaları ayıkladığınız dizine geçin. Sistemde zaten yüklü olan yanlış PowerShell modüllerden doğabilecek sorunları önlemek için yeni bir pencere kullanın.

6. [Azure PowerShell sürüm 1.2.11 yükleme](azure-stack-powershell-install.md).

7. `DeployMySqlProvider.ps1` betiğini çalıştırın.

Komut dosyası, aşağıdaki adımları gerçekleştirir:

* (Bu çevrimdışı sağlanabilir) MySQL bağlayıcı ikili indirir.
* Sertifikaları ve diğer yapıları depolama hesabı Azure yığında yükler.
* SQL veritabanları Galerisine dağıtabilirsiniz galeri paketleri yayınlar.
* Barındırma sunucuları dağıtmak için bir galeri paketi yayımlar.
* 1. adımda oluşturulan Windows Server 2016 görüntüsünü kullanarak bir VM dağıtır. Ayrıca, kaynak sağlayıcısı yükler.
* Kaynak sağlayıcınız VM eşler yerel bir DNS kaydı kaydeder.
* Kaynak sağlayıcısı yerel Azure Resource Manager ile (Kiracı ve yönetim) kaydeder.


Şunları yapabilirsiniz:
- Komut satırında gerekli parametreleri belirtin.
- Hiçbir parametre olmadan çalıştırın ve bunları istendiğinde girin.

PowerShell isteminden çalıştırabilirsiniz örnek aşağıda verilmiştir. Hesap bilgileri ve gerektiğinde parolaları değiştirdiğinizden emin olun:


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
# Find the ERCS01 IP address first, and make sure the certificate
# file is in the specified directory.
. $tempDir\DeployMySQLProvider.ps1 -AzCredential $AdminCreds `
  -VMLocalCredential $vmLocalAdminCreds `
  -CloudAdminCredential $cloudAdminCreds `
  -PrivilegedEndpoint $privilegedEndpoint `
  -DefaultSSLCertificatePassword $PfxPass `
  -DependencyFilesLocalPath $tempDir\cert `
  -AcceptLicense

 ```


### <a name="deploymysqlproviderps1-parameters"></a>DeployMySqlProvider.ps1 parameters
Komut satırında bu parametreleri belirtebilirsiniz. Bunu yapmazsanız veya hiçbir parametre doğrulaması başarısız olursa, gerekli parametreler sağlamanız istenir.

| Parametre adı | Açıklama | Açıklama veya varsayılan değer |
| --- | --- | --- |
| **CloudAdminCredential** | Bulut Yöneticisi, ayrıcalıklı endpoint erişmek için gerekli kimlik bilgileri. | _Gerekli_ |
| **AzCredential** | Azure yığın hizmet yönetici hesabı için kimlik bilgileri. Azure yığın dağıtmak için kullanılan kimlik bilgilerini kullanın. | _Gerekli_ |
| **VMLocalCredential** | MySQL kaynak sağlayıcı VM yerel yönetici hesabı için kimlik bilgileri. | _Gerekli_ |
| **PrivilegedEndpoint** | IP adresi veya ayrıcalıklı uç noktanın DNS adı. |  _Gerekli_ |
| **DependencyFilesLocalPath** | İçeren bir yerel paylaşıma yoluna [bağlayıcı net 6.10.5.msi mysql](https://dev.mysql.com/get/Downloads/Connector-Net/mysql-connector-net-6.10.5.msi). Bu yollarından biri, sağlarsanız, sertifika dosyasının bu dizinde yerleştirilmelidir. | _İsteğe bağlı_ (_zorunlu_ çok düğümlü için) |
| **DefaultSSLCertificatePassword** | .Pfx sertifika parolası. | _Gerekli_ |
| **MaxRetryCount** | Bir hata olduğunda her işlemini yeniden denemek istiyor sayısı.| 2 |
| **RetryDuration** | Saniye cinsinden yeniden denemeler arasındaki zaman aşımı aralığı. | 120 |
| **Kaldırma** | (Aşağıdaki notlara bakın) ilişkili tüm kaynakları ve kaynak sağlayıcısını kaldırır. | Hayır |
| **DebugMode** | Otomatik temizleme hatasında engeller. | Hayır |
| **AcceptLicense** | GPL lisansı kabul etmek için istemi atlar.  (http://www.gnu.org/licenses/old-licenses/gpl-2.0.html) | |


## <a name="verify-the-deployment-by-using-the-azure-stack-portal"></a>Azure yığın portalını kullanarak dağıtımı doğrulama

> [!NOTE]
>  Yükleme komut dosyası çalışması bittikten sonra Yönetim dikey penceresini görmek için portal yenilemeniz gerekir.


1. Yönetim portalında Hizmet Yöneticisi olarak oturum açın.

2. Dağıtım başarılı olduğunu doğrulayın. Git **kaynak grupları**ve ardından **sistem.\< Konum\>.mysqladapter** kaynak grubu. Tüm dört dağıtımlarının başarılı olduğunu doğrulayın.

      ![MySQL RP dağıtımını doğrulayın](./media/azure-stack-mysql-rp-deploy/mysqlrp-verify.png)

## <a name="provide-capacity-by-connecting-to-a-mysql-hosting-server"></a>Bir MySQL barındırma sunucusuna bağlanarak kapasitesi sağlar

1. Azure yığın portalında hizmet yönetici olarak oturum açın

2. Seçin **yönetim KAYNAKLARININ** > **sunucuları barındırma MySQL** > **+ Ekle**.

    Üzerinde **MySQL barındırma sunucuları** dikey penceresinde, MySQL Server'ın kaynak sağlayıcısının arka ucu olarak hizmet gerçek örnekleri için MySQL Server Kaynak sağlayıcısı bağlanabilir.

    ![Barındırma sunucuları](./media/azure-stack-mysql-rp-deploy/mysql-add-hosting-server-2.png)

3. MySQL Server örneğinizi bağlantı ayrıntılarını sağlayın. Tam etki alanı adı (FQDN) veya geçerli bir IPv4 adresi ve değil kısa VM adını verdiğinizden emin olun. Bu yükleme artık varsayılan MySQL örneği sağlar. Sağlanan boyutu veritabanı kapasitesine yönetmek kaynak sağlayıcısı yardımcı olur. Veritabanı sunucusu fiziksel kapasite yakın olması gerekir.

    > [!NOTE]
    >MySQL örneğine Kiracı ve yönetici Azure Resource Manager tarafından erişilebiliyorsa, kaynak sağlayıcısı denetiminde yerleştirilebilir. MySQL örneğine *gerekir* özel olarak kaynak sağlayıcısı ayrılamadı.

4. Sunucu eklemek gibi ayrıştırması hizmet tekliflerinin izin vermek için bir yeni veya var olan SKU'ya atamanız gerekir.
  Örneğin, bir kuruluş örneği sağlama sahip olabilir:
    - Veritabanı kapasite
    - Otomatik yedekleme
    - tek tek departmanlar için yüksek performanslı sunucuları
 

Böylece kiracılar kendi veritabanlarını uygun şekilde yerleştirebilirsiniz SKU adı özelliklerini yansıtmalıdır. Bir SKU tüm barındırma sunucuları aynı olanaklara sahip olmalıdır.

![Bir MySQL SKU oluşturma](./media/azure-stack-mysql-rp-deploy/mysql-new-sku.png)


>[!NOTE]
> SKU'ları portalında görünür olması için bir saat sürebilir. SKU oluşturulana kadar bir veritabanı oluşturulamıyor.


## <a name="test-your-deployment-by-creating-your-first-mysql-database"></a>İlk MySQL veritabanınızı oluşturarak dağıtımınızı test edin


1. Azure yığın portalında hizmet yönetici olarak oturum açın

2. Seçin **+ yeni** > **veri + depolama** > **MySQL veritabanı**.

3. Veritabanı bilgileri sağlar.

    ![MySQL veritabanı test oluşturma](./media/azure-stack-mysql-rp-deploy/mysql-create-db.png)

4. Bir SKU seçin.

    ![Bir SKU seçin](./media/azure-stack-mysql-rp-deploy/mysql-select-a-sku.png)

5. Bir oturum açma ayarı oluşturun. Varolan bir oturum açma ayarı yeniden kullanabilir veya yeni bir tane oluşturun. Bu ayar, kullanıcı adını ve veritabanı parolasını içerir.

    ![Yeni bir veritabanı oturum açmayı oluşturma](./media/azure-stack-mysql-rp-deploy/create-new-login.png)

    Bağlantı dizesi gerçek veritabanı sunucusu adını içerir. Portal'dan kopyalayın.

    ![MySQL veritabanı için bağlantı dizesi alma](./media/azure-stack-mysql-rp-deploy/mysql-db-created.png)

> [!NOTE]
> Kullanıcı adları uzunluk MySQL 5.7 32 karakteri aşamaz. Önceki sürümlerinde, 16 karakterden uzun olamaz.


## <a name="add-capacity"></a>Kapasite ekleme

Kapasite Azure yığın Portalı'nda ek MySQL sunucuları ekleyerek ekleyin. Ek sunucu için yeni veya varolan bir SKU eklenebilir. Sunucu özellikleri aynı olduğundan emin olun.


## <a name="make-mysql-databases-available-to-tenants"></a>MySQL veritabanları kiracılar için kullanılabilir hale
Planları ve MySQL veritabanlarını kiracıların kullanımına sunmak için teklifleri oluşturun. Örneğin, Microsoft.MySqlAdapter hizmeti eklemek, kota eklemek ve benzeri.

![Planları ve veritabanlarını içerecek şekilde teklifleri oluştur](./media/azure-stack-mysql-rp-deploy/mysql-new-plan.png)

## <a name="update-the-administrative-password"></a>Yönetici parolasını güncelleştirin
Parola ilk, MySQL server örneğinde değiştirerek değiştirebilirsiniz. Seçin **yönetim KAYNAKLARININ** > **sunucuları barındırma MySQL**. Ardından barındırma sunucusu seçin. İçinde **ayarları** paneli, select **parola**.

![Yönetici parolasını güncelleştirin](./media/azure-stack-mysql-rp-deploy/mysql-update-password.png)

## <a name="update-the-mysql-resource-provider-adapter-multi-node-only-builds-1710-and-later"></a>MySQL kaynak sağlayıcı bağdaştırıcısını (çok düğümlü yalnızca derlemeleri 1710 ve üzeri) güncelleştir
Azure yığın derlemeleri güncelleştirildiğinde yeni bir SQL kaynak sağlayıcısı bağdaştırıcısı yayımlanan. Varolan bağdaştırıcısı çalışmaya devam ederken, en son sürüme mümkün olan en kısa sürede güncelleştirilmesi önerilir. 

Kullandığınız kaynak sağlayıcısı güncelleştirmek için *UpdateMySQLProvider.ps1* komut dosyası. İşlem bölümünde açıklandığı gibi bir kaynak Sağlayıcısı'nı yüklemek için kullanılan işlem benzer [kaynak sağlayıcısı dağıtmak](#deploy-the-resource-provider) bu makalenin. Betik kaynak sağlayıcısı yükleme ile dahil edilir.

*UpdateMySQLProvider.ps1* komut dosyası en son kaynak sağlayıcısı kodu ile yeni bir VM oluşturur ve yeni VM'ye eski sanal makineden ayarları geçirir. Geçiş ayarları veritabanı ve barındırma sunucusu bilgilerini içerir ve gerekli DNS kaydı.

Komut dosyası için DeployMySqlProvider.ps1 komut açıklanan aynı bağımsız değişkenlere kullanılmasını gerektirir. Sertifika burada da sağlar. 

Aşağıdaki örneği verilmiştir *UpdateMySQLProvider.ps1* PowerShell isteminden çalıştırıp komut dosyası. Hesap bilgileri ve gerektiğinde parolaları değiştirdiğinizden emin olun: 

> [!NOTE]
> Güncelleştirme işlemi yalnızca tümleşik sistemleri için geçerlidir.

```
# Install the AzureRM.Bootstrapper module, set the profile, and install AzureRM and AzureStack modules.
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2017-03-09-profile
Install-Module -Name AzureStack -RequiredVersion 1.2.11 -Force

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

# Set credentials for the new resource provider VM.
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("sqlrpadmin", $vmLocalAdminPass)

# And the cloudadmin credential required for privileged endpoint access.
$CloudAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass)

# Change the following as appropriate.
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force

# Change directory to the folder where you extracted the installation files.
# Then adjust the endpoints.
. $tempDir\UpdateMySQLProvider.ps1 -AzCredential $AdminCreds `
  -VMLocalCredential $vmLocalAdminCreds `
  -CloudAdminCredential $cloudAdminCreds `
  -PrivilegedEndpoint $privilegedEndpoint `
  -DefaultSSLCertificatePassword $PfxPass `
  -DependencyFilesLocalPath $tempDir\cert `
  -AcceptLicense
 ```

### <a name="updatemysqlproviderps1-parameters"></a>UpdateMySQLProvider.ps1 parametreleri
Komut satırında bu parametreleri belirtebilirsiniz. Bunu yapmazsanız veya hiçbir parametre doğrulaması başarısız olursa, gerekli parametreler sağlamanız istenir.

| Parametre Adı | Açıklama | Açıklama veya varsayılan değer |
| --- | --- | --- |
| **CloudAdminCredential** | Bulut Yöneticisi, ayrıcalıklı endpoint erişmek için gerekli kimlik bilgileri. | _Gerekli_ |
| **AzCredential** | Azure yığın hizmet yönetici hesabı için kimlik bilgileri. Azure yığın dağıtmak için kullanılan kimlik bilgilerinin aynısını kullanın. | _Gerekli_ |
| **VMLocalCredential** |SQL kaynak sağlayıcısının VM yerel yönetici hesabı için kimlik bilgileri. | _Gerekli_ |
| **PrivilegedEndpoint** | IP adresi veya ayrıcalıklı uç noktanın DNS adı. |  _Gerekli_ |
| **DependencyFilesLocalPath** | Sertifika .pfx dosyasının bu dizinde yerleştirilmelidir. | _İsteğe bağlı_ (_zorunlu_ çok düğümlü için) |
| **DefaultSSLCertificatePassword** | .Pfx sertifika için parola | _Gerekli_ |
| **MaxRetryCount** | Bir hata olduğunda her işlemini yeniden denemek istiyor sayısı.| 2 |
| **RetryDuration** | Saniye cinsinden yeniden denemeler arasındaki zaman aşımı aralığı. | 120 |
| **Kaldırma** | (Aşağıdaki notlara bakın) ilişkili tüm kaynakları ve kaynak sağlayıcısı kaldırın. | Hayır |
| **DebugMode** | Otomatik temizleme hatasında engeller. | Hayır |
| **AcceptLicense** | GPL lisansı kabul etmek için istemi atlar.  (http://www.gnu.org/licenses/old-licenses/gpl-2.0.html) | |


## <a name="collect-diagnostic-logs"></a>Tanılama günlüklerini toplayın
MySQL kaynak kilitli bir sanal makineyi sağlayıcıdır. Bu sanal makineden bir PowerShell yalnızca yetecek kadar Yönetim (JEA) uç noktası günlükleri toplamak için gerekli olur _DBAdapterDiagnostics_ bu amaç için sağlanır. Bu uç noktası aracılığıyla kullanılabilen iki komut vardır:

* Get-AzsDBAdapterLog - RP tanılama günlükleri içeren bir zip paketini hazırlar ve oturum kullanıcı sürücüde yerleştirir. Komut parametresiz çağrılabilir ve günlükleri son dört saatlik toplar.
* Remove-AzsDBAdapterLog - kaynak sağlayıcısı VM mevcut günlük paketlerini temizler

Bir kullanıcı hesabı olarak adlandırılan _dbadapterdiag_ RP dağıtım veya RP günlükleri ayıklanacağı tanılama uç noktasına bağlanmak için güncelleştirme işlemi sırasında oluşturulur. Bu hesabın parolasını dağıtım/güncelleştirme sırasında yerel yönetici hesabı için girilen parola ile aynıdır.

Bu komutları kullanmak için kaynak sağlayıcısı sanal makineye uzak PowerShell oturumu oluşturmak ve komut çağırma gerekir. İsteğe bağlı olarak FromDate ve ToDate parametreleri sağlayabilir. Aşağıdakilerden birini veya bunların her ikisi de belirtmezseniz, geçerli tarihten önce dört saat FromDate olacaktır ve ToDate geçerli saati olacaktır.

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
MySQL kaynak kilitli bir sanal makineyi sağlayıcıdır. Kaynak sağlayıcısı sanal makinenin güvenlik güncelleştirme PowerShell yalnızca yetecek kadar Yönetim (JEA) uç noktası aracılığıyla yapılabilir _DBAdapterMaintenance_.

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

2. Bir PowerShell oturumuna MySQL RP bağdaştırıcısı sanal makinenin bakım uç noktası oluşturma
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


## <a name="remove-the-mysql-resource-provider-adapter"></a>MySQL kaynak sağlayıcı bağdaştırıcısını Kaldır

Kaynak sağlayıcısı kaldırmak için önce tüm bağımlılıklarını kaldırmanız gereklidir.

1. Kaynak Sağlayıcısı'nın bu sürümü için indirdiğiniz özgün dağıtım paketi olduğundan emin olun.

2. Tüm Kiracı veritabanları kaynak Sağlayıcısı'ndan silinmesi gerekir. (Kiracı veritabanlarını silerek verileri silmez.) Bu görev, kiracılar tarafından gerçekleştirilmelidir.

3. Kiracılar ad alanından kayıttan çıkarmanız gerekir.

4. Yönetici barındırma sunucuları MySQL bağdaştırıcısından silmeniz gerekir.

5. Yönetici, MySQL bağdaştırıcısı başvuru herhangi bir plan silmeniz gerekir.

6. Yönetici, MySQL bağdaştırıcısı ile ilişkili tüm kotalar silmeniz gerekir.

7. Aşağıdaki öğeleri içeren dağıtım betiği yeniden çalıştırın:
    - Parametre kaldırma
    - Azure Resource Manager uç noktaları
    - DirectoryTenantID
    - Hizmet yöneticisi hesabı için kimlik bilgileri
