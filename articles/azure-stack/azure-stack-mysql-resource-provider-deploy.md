---
title: "MySQL veritabanları PaaS Azure yığında kullanın | Microsoft Docs"
description: "MySQL kaynak sağlayıcı dağıtmak ve Azure yığında bir hizmet olarak MySQL veritabanları sağlar nasıl bilgi edinin"
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
ms.openlocfilehash: badaefb4986f573362babea81d704bf2be067d6b
ms.sourcegitcommit: 804db51744e24dca10f06a89fe950ddad8b6a22d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2017
---
# <a name="use-mysql-databases-on-microsoft-azure-stack"></a>Microsoft Azure yığında MySQL veritabanları kullanın

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Bir MySQL kaynak sağlayıcı Azure yığında dağıtabilirsiniz. Kaynak sağlayıcısı dağıttıktan sonra MySQL sunucuları ve Azure Resource Manager dağıtım şablonları aracılığıyla veritabanları oluşturabilir ve bir hizmet olarak MySQL veritabanları sağlar. Web sitelerinde yaygın olarak kullanılan MySQL veritabanları, birçok Web sitesi platformları destekler. Kaynak sağlayıcısı dağıttıktan sonra örnek olarak, WordPress Web siteleri Azure Web Apps platformundan hizmet (PaaS) eklentisi için Azure yığın oluşturabilirsiniz.

İnternet erişimi olmayan bir sistemde MySQL sağlayıcı dağıtmak için dosyayı kopyalayabilirsiniz [bağlayıcı net 6.9.9.msi mysql](https://dev.mysql.com/get/Download/sConnector-Net/mysql-connector-net-6.9.9.msi) bir yerel paylaşıma. Ardından, istendiğinde bu paylaşım adı sağlayın. Azure ve Azure yığın PowerShell modüllerini de yüklemeniz gerekir.


## <a name="mysql-server-resource-provider-adapter-architecture"></a>MySQL Server Kaynak sağlayıcısı bağdaştırıcısı mimarisi

Kaynak sağlayıcısı üç bileşenlerden oluşur:

- **MySQL kaynak sağlayıcı bağdaştırıcısı VM**, hangi sağlayıcı hizmetlerini çalıştıran bir Windows sanal makine.
- **Kaynak sağlayıcısı**, sağlama isteklerini işler ve kullanıma sunar kaynaklarını veritabanı.
- **MySQL Server barındıran sunucular**, veritabanları için kapasite sağlayan barındırma sunucuları denir.

Bu sürüm, artık bir MySQL örneği oluşturur. Siz bunları oluşturmanız veya dış SQL örnekleri erişim sağlamak. Ziyaret [Azure yığın hızlı başlama Galerisi](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/mysql-standalone-server-windows) için bir örnek şablonu için:
- sizin için bir MySQL server oluşturun
- indirin ve MySQL Server marketten dağıtın.

! [NOT] Barındırma sunucuları bir çok düğümlü Azure yığında yüklü bir kiracı abonelikten oluşturulması gerekir. Varsayılan sağlayıcı abonelikten oluşturulamıyor. Diğer bir deyişle, Kiracı portalından veya uygun bir oturum açma ile bir PowerShell oturumu oluşturulmalıdır. Tüm barındırma sunucuları ücrete tabi VM'ler ve uygun izinlere sahip olmalıdır. Hizmet Yöneticisi, bu aboneliğin sahibi olabilir.

### <a name="required-privileges"></a>Gerekli ayrıcalıklar
Sistem hesabı aşağıdaki ayrıcalıklara sahip olmalıdır:

1.  Veritabanı: Oluşturma, bırakma
2.  Oturum açma: Oluşturma, ayarlayın, bırakma, vermek, iptal etme

## <a name="deploy-the-resource-provider"></a>Kaynak sağlayıcısı dağıtma

1. Zaten yapmadıysanız, Geliştirme Seti kaydolun ve Windows Server 2016 Datacenter Core görüntünün Market yönetiminden indirilebilir indirin. Bir Windows Server 2016 Core görüntüsü kullanmanız gerekir. Bir komut dosyası oluşturmak için de kullanabilirsiniz bir [Windows Server 2016 görüntü](https://docs.microsoft.com/azure/azure-stack/azure-stack-add-default-image) -çekirdek seçeneğini belirlediğinizden emin olun. .NET 3.5 çalışma zamanı artık gerekli değildir.


2. Ayrıcalıklı uç nokta VM erişebileceği bir ana bilgisayara oturum açın.

    a. Azure yığın Geliştirme Seti (ASDK) yüklemelerinde fiziksel ana bilgisayara oturum açın.

    b. Birden çok düğümlü sistemlerde konak ayrıcalıklı uç noktasına erişebildiğinden bir sistem olmalıdır.

3. [MySQL kaynak sağlayıcı ikili dosya indirme](https://aka.ms/azurestackmysqlrp) ve geçici bir dizine içeriği ayıklamak için ayıklayıcısı yürütün.

4.  Azure yığın kök sertifikası ayrıcalıklı uç noktasından alınır. ASDK için bu işlemin bir parçası olarak otomatik olarak imzalanan bir sertifika oluşturulur. Birden çok düğümlü için uygun bir sertifika sağlamanız gerekir.

    Kendi sertifikanızı sağlamanız gerekiyorsa, aşağıdaki sertifika gerekir:

    Joker sertifikası \*.dbadapter.\< Bölge\>.\< Dış fqdn\>. Bu sertifikayı Güvenilir olması gerekir, gibi bir sertifika yetkilisi tarafından verilmiş. Yani, güven zinciri ara sertifika gerektirmeden bulunması gerekir. Tek bir site sertifika yüklemesi sırasında kullanılan açık VM adı [mysqladapter] ile kullanılabilir.


5. Açık bir **yeni** yükseltilmiş (Yönetim) PowerShell konsolu ve dosyaları ayıkladığınız dizine geçin. Sistemde zaten yüklü yanlış PowerShell modüllerden ortaya çıkabilecek sorunları önlemek için yeni bir pencere kullanın.

6. [Azure PowerShell sürüm 1.2.11 yükleme](azure-stack-powershell-install.md).

7. DeploySqlProvider.ps1 komut dosyasını çalıştırın.

Komut dosyası, aşağıdaki adımları gerçekleştirir:

* (Bu çevrimdışı sağlanabilir) MySQL bağlayıcı ikili indirin.
* Sertifikaları ve diğer yapıları depolama hesabı, Azure yığında karşıya yükleyin.
* Böylece SQL veritabanları Galerisine dağıtabilirsiniz galeri paketleri yayımlama.
* Barındırma sunucuları dağıtmak için bir galeri paketi Yayımla
* 1. adımda oluşturduğunuz Windows Server 2016 görüntüsünü kullanan bir VM'yi dağıtmak ve kaynak sağlayıcısını yükleyin.
* Kaynak sağlayıcınız VM eşler yerel bir DNS kaydı kaydedin.
* Kaynak sağlayıcısı yerel Azure Resource Manager ile (Kiracı ve yönetim) kaydedin.


Şunları yapabilirsiniz:
- en az komut satırında gerekli parametreleri belirtin
- veya hiçbir parametre olmadan çalıştırırsanız, bunları istendiğinde girin.

Powershell'den çalıştırabilirsiniz örneği sor (ancak hesap bilgilerini ve parolaları gerektiği gibi değiştirin):


```
# Install the AzureRM.Bootstrapper module, set the profile, and install AzureRM and AzureStack modules
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2017-03-09-profile
Install-Module -Name AzureStack -RequiredVersion 1.2.11 -Force

# Use the NetBIOS name for the Azure Stack domain. On ASDK, the default is AzureStack
$domain = 'AzureStack'
# Point to the directory where the RP installation files were extracted
$tempDir = 'C:\TEMP\MYSQLRP'

# The service admin account (can be AAD or ADFS)
$serviceAdmin = "admin@mydomain.onmicrosoft.com"
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass)

# Set the credentials for the Resource Provider VM
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("mysqlrpadmin", $vmLocalAdminPass)

# and the cloudadmin credential required for Privleged Endpoint access
$CloudAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass)

# change the following as appropriate
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force

# Run the installation script from the folder where you extracted the installation files
# Find the ERCS01 IP address first and make sure the certificate
# file is in the specified directory
.$tempDir\DeployMySQLProvider.ps1 -AzCredential $AdminCreds `
  -VMLocalCredential $vmLocalAdminCreds `
  -CloudAdminCredential $cloudAdminCreds `
  -PrivilegedEndpoint '10.10.10.10' `
  -DefaultSSLCertificatePassword $PfxPass -DependencyFilesLocalPath $tempDir\cert `
  -AcceptLicense

 ```

### <a name="deploymysqlproviderps1-parameters"></a>DeployMySqlProvider.ps1 parametreleri

Komut satırında bu parametreleri belirtebilirsiniz. Bunu yapmanız veya hiçbir parametre doğrulaması başarısız olursa, gerekli olanları sağlamanız istenir.

| Parametre Adı | Açıklama | Açıklama veya varsayılan değeri |
| --- | --- | --- |
| **CloudAdminCredential** | Privleged Endpoint erişmek için gerekli bulut yönetici kimlik bilgileri. | _Gerekli_ |
| **AzCredential** | Azure yığın Hizmet yöneticisi hesabı için kimlik bilgilerini sağlayın. Azure yığın dağıtmak için kullanılan aynı kimlik bilgilerini kullanın). | _Gerekli_ |
| **VMLocalCredential** | MySQL kaynak sağlayıcısı VM yerel yönetici hesabının kimlik bilgilerini tanımlar. | _Gerekli_ |
| **PrivilegedEndpoint** | IP adresi veya ayrıcalıklı uç noktanın DNS adı sağlayın. |  _Gerekli_ |
| **DependencyFilesLocalPath** | İçeren bir yerel paylaşıma yolu [bağlayıcı net 6.9.9.msi mysql](https://dev.mysql.com/get/Downloads/Connector-Net/mysql-connector-net-6.9.9.msi). Bir sağlarsanız, sertifika dosyasının bu dizinde yerleştirilmelidir. | _İsteğe bağlı_ (_zorunlu_ çok düğümlü için) |
| **DefaultSSLCertificatePassword** | .Pfx sertifika için parola | _Gerekli_ |
| **MaxRetryCount** | Bir hata varsa her işlemini yeniden denemek istiyor kaç kez tanımlayın.| 2 |
| **RetryDuration** | Zaman aşımı saniye içinde yeniden denemeler arasında tanımlayın. | 120 |
| **Kaldırma** | (Aşağıdaki notlara bakın) ilişkili tüm kaynakları ve kaynak sağlayıcısını Kaldır | Hayır |
| **DebugMode** | Otomatik temizleme hatasında engeller | Hayır |
| **AcceptLicense** | GPL lisansı (http://www.gnu.org/licenses/old-licenses/gpl-2.0.html) kabul etmek için istemi atlanıyor | |


Sistem performansı ve indirme hızına bağlı olarak, yükleme olarak birkaç saat 20 dakika veya uzun olarak biraz sürebilir. MySQLAdapter dikey kullanılabilir durumda değilse, Yönetici portalı yenileyin.

> [!NOTE]
> Yükleme 90 dakikadan uzun sürerse, başarısız olabilir ve ekranında ve günlük dosyasında bir hata iletisi görürsünüz. Dağıtımı başarısız olan adımdan denenir. Önerilen bellek ve çekirdek belirtimleri karşılamıyor sistemleri MySQL RP dağıtmak mümkün olmayabilir.



## <a name="verify-the-deployment-using-the-azure-stack-portal"></a>Azure yığın Portalı'nı kullanarak dağıtımı doğrulama

> [!NOTE]
>  Yükleme komut dosyası tamamlandıktan sonra Yönetim dikey penceresini görmek için portal yenilemeniz gerekir.


1. Yönetim portalında Hizmet Yöneticisi olarak oturum açın.

2. Dağıtım başarılı olduğunu doğrulayın. Göz **kaynak grupları** &gt;, tıklayın **sistem.\< Konum\>.mysqladapter** kaynak grubunu ve tüm dört dağıtımlarının başarılı olduğunu doğrulayın.

      ![MySQL RP dağıtımını doğrulayın](./media/azure-stack-mysql-rp-deploy/mysqlrp-verify.png)

## <a name="provide-capacity-by-connecting-to-a-mysql-hosting-server"></a>Bir MySQL barındırma sunucusuna bağlanarak kapasitesi sağlar

1. Azure yığın portalında hizmet yönetici olarak oturum açın

2. Gözat **yönetim KAYNAKLARININ** &gt; **sunucuları barındırma MySQL** &gt; **+ Ekle**.

    **MySQL barındırma sunucuları** dikey olduğu yere MySQL Server'ın kaynak sağlayıcısının arka ucu olarak hizmet gerçek örneklerini MySQL Server Kaynak sağlayıcısı bağlanabilir.

    ![Barındırma sunucuları](./media/azure-stack-mysql-rp-deploy/mysql-add-hosting-server-2.png)

3. Form, MySQL Server örneği ile bağlantı ayrıntıları doldurun. Tam etki alanı adı (FQDN) veya geçerli bir IPv4 adresi ve değil kısa VM adını sağlayın. Bu yükleme artık varsayılan MySQL örneği sağlar. Sağlanan boyutu veritabanı kapasitesine yönetmek kaynak sağlayıcısı yardımcı olur. Veritabanı sunucusu fiziksel kapasite yakın olması gerekir.

    > [!NOTE]
    > MySQL örneğine Kiracı ve yönetici Azure Resource Manager tarafından erişilebilen sürece, kaynak sağlayıcısı denetiminde yerleştirilebilir. MySQL örneğine __gerekir__ özel olarak RP ayrılamadı.

4. Sunucu eklemek gibi ayrıştırması hizmet tekliflerinin izin vermek için bir yeni veya var olan SKU'ya atamanız gerekir.
  Örneğin, bir kuruluş örneği sağlama sahip olabilir:
    - Veritabanı kapasite
    - otomatik yedekleme
    - tek tek departmanlar için yüksek performanslı sunucuları
    - ve benzeri.
    Böylece kiracılar kendi veritabanlarını uygun şekilde yerleştirebilirsiniz SKU adı özelliklerini yansıtmalıdır. Bir SKU tüm barındırma sunucuları aynı olanaklara sahip olmalıdır.

    ![Bir MySQL SKU oluşturma](./media/azure-stack-mysql-rp-deploy/mysql-new-sku.png)


>[!NOTE]
SKU'ları portalında görünür olması için bir saat sürebilir. SKU oluşturulana kadar bir veritabanı oluşturulamıyor.


## <a name="to-test-your-deployment-create-your-first-mysql-database"></a>Dağıtımınızı test etmek için ilk MySQL veritabanınızı oluşturma


1. Azure yığın portalında hizmet yönetici olarak oturum açın

2. Tıklatın **+ yeni** düğmesini &gt; **veri + depolama** &gt; **MySQL veritabanı**.

3. Veritabanı ayrıntılarla formu doldurun.

    ![MySQL veritabanı test oluşturma](./media/azure-stack-mysql-rp-deploy/mysql-create-db.png)

4. Bir SKU seçin.

    ![Bir SKU seçin](./media/azure-stack-mysql-rp-deploy/mysql-select-a-sku.png)

5. Bir oturum açma ayarı oluşturun. Oturum açma ayarı yeniden kullanılabilir veya yeni bir tane oluşturulur. Bu ayar, kullanıcı adını ve veritabanı parolasını içerir.

    ![Yeni bir veritabanı oturum açmayı oluşturma](./media/azure-stack-mysql-rp-deploy/create-new-login.png)

    Bağlantı dizesi gerçek veritabanı sunucusu adını içerir. Portal'dan kopyalayın.

    ![MySQL veritabanı için bağlantı dizesi alma](./media/azure-stack-mysql-rp-deploy/mysql-db-created.png)

> [!NOTE]
> Kullanıcı adları uzunluk MySQL 5.7 ile 32 karakter veya daha önceki sürümlerinde 16 karakter aşamaz.


## <a name="add-capacity"></a>Kapasite ekleme

Kapasite Azure yığın Portalı'nda ek MySQL sunucuları ekleyerek ekleyin. Ek sunucu için yeni veya varolan bir SKU eklenebilir. Sunucu özellikleri aynı olduğundan emin olun.


## <a name="making-mysql-databases-available-to-tenants"></a>MySQL veritabanları kiracılar için kullanılabilir hale getirme
Planları ve MySQL veritabanlarını kiracıların kullanımına sunmak için teklifleri oluşturun. Microsoft.MySqlAdapter hizmeti eklemek, kota, vb. ekleyin.

![Planları ve veritabanlarını içerecek şekilde teklifleri oluştur](./media/azure-stack-mysql-rp-deploy/mysql-new-plan.png)

## <a name="updating-the-administrative-password"></a>Yönetici parolasını güncelleştiriliyor
Parola ilk, MySQL server örneğinde değiştirerek değiştirebilirsiniz. Gözat **yönetim KAYNAKLARININ** &gt; **MySQL barındırma sunucuları** &gt; ve barındırma sunucusundaki'ı tıklatın. Parola Ayarları panelinde tıklayın.

![Yönetici parolasını güncelleştirin](./media/azure-stack-mysql-rp-deploy/mysql-update-password.png)

## <a name="removing-the-mysql-adapter-resource-provider"></a>MySQL bağdaştırıcısı kaynak sağlayıcısı kaldırılıyor

Kaynak sağlayıcısı kaldırmak için önce tüm bağımlılıklarını kaldırmanız gereklidir.

1. Kaynak Sağlayıcısı'nın bu sürümü için indirdiğiniz özgün dağıtım paketi olduğundan emin olun.

2. Tüm Kiracı veritabanı (Bu verileri silmez) kaynak Sağlayıcısı'ndan silinmesi gerekir. Bu, kiracılar tarafından gerçekleştirilmelidir.

3. Kiracılar ad alanından kayıttan çıkarmanız gerekir.

4. Yönetici barındırma sunucuları MySQL bağdaştırıcısından silmeniz gerekir

5. Yönetici, MySQL bağdaştırıcısı başvuru herhangi bir plan silmeniz gerekir.

6. Yönetici, MySQL bağdaştırıcıya ilişkili tüm kotalar silmeniz gerekir.

7. Dağıtım betiği ile yeniden parametresi, Azure Resource Manager uç noktaları, DirectoryTenantID ve hizmet yönetici hesabının kimlik bilgilerini kaldırın.




## <a name="next-steps"></a>Sonraki adımlar


Diğer deneyin [PaaS Hizmetleri](azure-stack-tools-paas-services.md) gibi [SQL Server Kaynak sağlayıcısı](azure-stack-sql-resource-provider-deploy.md) ve [uygulama hizmetleri kaynak sağlayıcı](azure-stack-app-service-overview.md).
