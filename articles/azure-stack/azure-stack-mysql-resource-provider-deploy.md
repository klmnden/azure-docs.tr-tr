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
ms.date: 06/20/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: 061d9431890622dafafa8c180027b8df735c100e
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36295761"
---
# <a name="use-mysql-databases-on-microsoft-azure-stack"></a>Microsoft Azure yığında MySQL veritabanları kullanın
MySQL veritabanları Azure yığınının hizmet olarak kullanıma sunmak için Azure yığın MySQL Server Kaynak sağlayıcısı kullanın. MySQL kaynak sağlayıcısı hizmeti MySQL kaynak sağlayıcı bir Windows Server çekirdek sanal makine VM üzerinde çalışır.

## <a name="deploy-the-resource-provider"></a>Kaynak sağlayıcısı dağıtma 
1. Zaten yapmadıysanız, Geliştirme Seti kaydolun ve Windows Server 2016 Datacenter Core görüntünün Market yönetiminden indirilebilir indirin. Bir Windows Server 2016 Core görüntüsü kullanmanız gerekir. Bir komut dosyası oluşturmak için de kullanabilirsiniz bir [Windows Server 2016 görüntü](https://docs.microsoft.com/azure/azure-stack/azure-stack-add-default-image). (Çekirdek seçeneğini belirlediğinizden emin olun.) 

2. VM ayrıcalıklı uç noktasına erişebildiğinden bir ana bilgisayara oturum açın:
    - Azure SDK'sı yüklemelerinde fiziksel ana bilgisayara oturum açın.  
    - Tümleşik sistemlerde, konak ayrıcalıklı uç noktasına erişebildiğinden bir sistem olmalıdır. 
    
    >[!NOTE] 
    > Betik çalıştırılıyor sistem *gerekir* yüklü .NET çalışma zamanı en son sürümü Windows 10 veya Windows Server 2016 sistemiyle olabilir. Aksi takdirde yükleme başarısız olur. Azure yığın ASDK konak bu ölçütü karşılayan. 
    
3. İkili MySQL kaynak sağlayıcıyı yükleyin. Daha sonra içeriği geçici bir dizine ayıklayın ayıklayıcısı çalıştırın. 
    >[!NOTE]  
    > Kaynak sağlayıcısı bir minimum karşılık gelen Azure yapı yığınına sahiptir. Doğru ikili çalıştıran Azure yığın sürümü yüklediğinizden emin olun.

    | Azure yığın sürümü | MySQL RP sürümü| 
    | --- | --- | 
    | Sürüm 1804 (1.0.180513.1)|[MySQL RP sürüm 1.1.24.0](https://aka.ms/azurestackmysqlrp1804) | 
    | Sürüm 1802 (1.0.180302.1) | [MySQL RP sürüm 1.1.18.0](https://aka.ms/azurestackmysqlrp1802) | 
    | Sürüm 1712 (1.0.180102.3 veya 1.0.180106.1 (tümleşik sistemler için)) | [MySQL RP sürüm 1.1.14.0](https://aka.ms/azurestackmysqlrp1712) | 

4.  ASDK için bu işlemin bir parçası olarak otomatik olarak imzalanan bir sertifika oluşturulur. Tümleşik sistemler için uygun bir sertifika sağlamanız gerekir. Kendi sertifikanızı sağlamanız gerekiyorsa, bir .pfx dosyasına yerleştirin **DependencyFilesLocalPath** ve şu ölçütlere uymalıdır: 
    - İçin bir joker karakter sertifika \*.dbadapter.\< Bölge\>.\< Dış fqdn\> veya mysqladapter.dbadapter bir ortak adı tek bir site sertifikayla.\< Bölge\>.\< Dış fqdn\>. 
    - Bu sertifikayı Güvenilir olması gerekir. Yani, güven zinciri ara sertifika gerektirmeden bulunması gerekir. 
    - Yalnızca tek bir sertifika dosyası DependencyFilesLocalPath bulunmaktadır. 
    - Dosya adı, herhangi bir özel karakter veya boşluk içermemelidir. 

5. Açık bir **yeni** yükseltilmiş (Yönetim) PowerShell Konsolu. Ardından dosyaları ayıkladığınız dizine geçin. Sistemde zaten yüklü olan yanlış PowerShell modüllerden doğabilecek sorunları önlemek için yeni bir pencere kullanın. 

6. Çalıştırma **DeployMySqlProvider.ps1** komut dosyası. Komut dosyası, aşağıdaki adımları gerçekleştirir: 
    - (Bu çevrimdışı sağlanabilir) MySQL bağlayıcı ikili indirir. 
    - Sertifikaları ve diğer yapıları depolama hesabı Azure yığında yükler. 
    - SQL veritabanları Galerisine dağıtabilirsiniz galeri paketleri yayınlar. 
    - Barındırma sunucuları dağıtmak için bir galeri paketi yayımlar. 
    - Bir Windows Server 2016 Azure yığın Market görüntüsü kullanarak bir VM dağıtır ve kaynak Sağlayıcısı'nı yükler. 
    - Kaynak sağlayıcınız VM eşler yerel bir DNS kaydı kaydeder. 
    - Kaynak sağlayıcısı yerel Azure Resource Manager ile (Kiracı ve yönetim) kaydeder. 

    Komut satırında gerekli parametreleri belirtin olabilir veya hiçbir parametre olmadan komut dosyasını çalıştırmak ve bunları istendiğinde girin. 

    PowerShell isteminden çalıştırabilirsiniz örnek aşağıda verilmiştir. Hesap bilgileri ve gerektiğinde parolaları değiştirdiğinizden emin olun: 

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
    # Find the ERCS01 IP address first, and make sure the certificate 
    # file is in the specified directory. 
    $tempDir\DeployMySQLProvider.ps1 -AzCredential $AdminCreds ` 
        -VMLocalCredential $vmLocalAdminCreds ` 
        -CloudAdminCredential $cloudAdminCreds ` 
        -PrivilegedEndpoint $privilegedEndpoint ` 
        -DefaultSSLCertificatePassword $PfxPass ` 
        -DependencyFilesLocalPath $tempDir\cert ` 
        -AcceptLicense 
    ``` 

### <a name="deploymysqlproviderps1-parameters"></a>DeployMySqlProvider.ps1 parametreleri 
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

> [!NOTE] 
> SKU'ları portalında görünür olması için bir saat sürebilir. SKU oluşturulana kadar bir veritabanı oluşturulamıyor. 

## <a name="verify-the-deployment-by-using-the-azure-stack-portal"></a>Azure yığın portalını kullanarak dağıtımı doğrulama 
> [!NOTE] 
>  Yükleme komut dosyası çalışması bittikten sonra Yönetim dikey penceresini görmek için portal yenilemeniz gerekir. 

1. Yönetim portalında Hizmet Yöneticisi olarak oturum açın. 
2. Dağıtım başarılı olduğunu doğrulayın. Git **kaynak grupları**ve ardından **sistem.\< Konum\>.mysqladapter** kaynak grubu. Tüm dört dağıtımlarının başarılı olduğunu doğrulayın:

      ![MySQL RP dağıtımını doğrulayın](./media/azure-stack-mysql-rp-deploy/mysqlrp-verify.png) 


## <a name="next-steps"></a>Sonraki adımlar
[Barındırma sunucuları ekleme](azure-stack-mysql-resource-provider-hosting-servers.md)
