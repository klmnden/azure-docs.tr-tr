---
title: "Windows Azure paketi sanal makineleri Azure yığınından yönetme | Microsoft Docs"
description: "Windows Azure Pack (WAP) VM'ler Azure yığınında Kullanıcı Portalı'ndan yönetmeyi öğrenin."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 213c2792-d404-4b44-8340-235adf3f8f0b
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/28/2018
ms.author: mabrigg
ms.openlocfilehash: a7e4896c84938b392a86f4d9609c4932324c785d
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="manage-windows-azure-pack-virtual-machines-from-azure-stack"></a>Windows Azure paketi sanal makineleri Azure yığınından yönetme

*Uygulandığı öğe: Azure yığın Geliştirme Seti*

Azure yığın Development Kit'te Kiracı Windows Azure Pack üzerinde çalışan sanal makineler için Azure yığın Kullanıcı Portalı'ndan erişimi etkinleştirebilirsiniz. Kullanıcılar kendi mevcut Iaas sanal makineler ve sanal ağlar yönetmek için Azure yığın Portalı'nı kullanabilirsiniz. Bu kaynaklar üzerinde Windows Azure Pack temel Service Provider Foundation (SPF) ve Virtual Machine Manager (VMM) bileşeni kullanılabilir hale getirilir. Özellikle, kullanıcılar şunları yapabilir:

* Kaynakları bulun
* Yapılandırma değerleri inceleyin
* Bir sanal makine başlatma veya durdurma
* Bir sanal makineye Uzak Masaüstü Protokolü (RDP) aracılığıyla bağlanma
* Oluşturma ve denetim noktalarını yönetme
* Sanal makineler ve sanal ağları Sil

Bu işlev Azure yığın (Önizleme) Windows Azure Pack bağlayıcı tarafından sağlanır. Bu makalede, bir tek düğümlü Azure yığın ortamı bağlayıcısının nasıl yapılandırılacağı gösterilmektedir.

Bu önizleme sürümü bağlayıcı için aşağıdakilere dikkat edin:
* Windows Azure Pack Bağlayıcısı yalnızca test ortamları (için Azure yığını ve Microsoft Azure Pack için) ve üretim dağıtımlarında kullanın.
* Windows Azure Pack güncelleştirme paketi (UR) 9.1 veya üstü ve System Center SPF ve VMM UR 9 veya üzeri olması gerekir.
* VMM ve SPF bileşenleri, System Center 2012 R2 veya System Center 2016 olabilir.
* Her iki Azure yığında ve Microsoft Azure Pack yapılandırma adımlarını gerçekleştirmeniz gerekir.
* Yönergeler olmayan - bulut platformu sistem CPS ortamlar için geçerlidir.
* Bilinen sorunlar gözden geçirmek için bkz: [Microsoft Azure yığın sorun giderme](azure-stack-troubleshooting.md).


## <a name="architecture"></a>Mimari
Aşağıdaki diyagramda, Windows Azure Pack bağlayıcı ana bileşenlerini gösterir.

![Windows Azure Pack bağlayıcı bileşenleri](media/azure-stack-manage-wap/image1.png)

Aşağıdaki ayrıntıları dikkat edin:
* Azure yığın Kullanıcı Portalı kaynak bilgileri hem bulutlardan (Azure yığını ve Microsoft Azure Pack) erişir.
* Kullanıcı her iki ortamlarda geçerli olan bir hesap olması gerekir.
* Azure yığın Kullanıcı Portalı, Windows Azure Pack üzerinde çalışan bileşenleri için ağ erişimi olması gerekir.
* İçinde **WAP** bölüm diyagramı yeni Windows Azure Pack bağlayıcı modülleri (WAP uzantısı ve bağlayıcı) ve mevcut Microsoft Azure Pack Kiracı API'si SPF ve VMM bileşenleri ile görebilirsiniz.


## <a name="identity-management"></a>Kimlik yönetimi
Windows Azure paketi Kiracı API'si Azure yığın güvenlik belirteci hizmeti (STS) güvenmelidir.

Portal, bir kullanıcı Windows Azure Pack kaynakları hedefleyen Azure yığın portal aracılığıyla bir eylem gerçekleştirdiğinde, Windows Azure paketi Kiracı API'sini kullanır. Bu nedenle, sağlanan kullanıcı kimlik doğrulama belirteci güvenilir bir STS gelmelidir. Aşağıdaki diyagramda bakın:

![Windows Azure Pack bağlayıcı kimlik doğrulama diyagramı](media/azure-stack-manage-wap/image2.png)

Geliştirme Seti ortamında Microsoft Azure Pack ve Azure yığın bağımsız kimlik sağlayıcıları vardır. Azure yığın Portalı'ndan her iki ortama erişen kullanıcılar, iki kimlik sağlayıcıları aynı kullanıcı asıl adı (UPN) adı olmalıdır. Örneğin, hesabı  *azurestackadmin@azurestack.local*  STS için Windows Azure Pack içinde de bulunmalıdır. AD FS giden güven ilişkileri desteklemek için ayarlanmamış olduğunda, AD FS Azure yığın örneğini Microsoft Azure Pack bileşenlerini (Kiracı API'si) güven oluşturacaktır.

## <a name="prerequisites"></a>Önkoşullar

### <a name="download-the-windows-azure-pack-connector"></a>Windows Azure Pack Bağlayıcısı'nı indirin
Üzerinde [Microsoft Download Center](https://aka.ms/wapconnectorazurestackdlc).exe dosyasını indirin ve yerel bilgisayarınıza ayıklayın. Daha sonra içeriği Microsoft Azure Pack ortamınıza erişmek için bir bilgisayara kopyalayın.

### <a name="deployment-option-requirement"></a>Dağıtım seçeneği gereksinimi
Microsoft Azure Pack ile tümleştirmek için AD FS veya Azure Active Directory seçeneğini kullanarak Azure yığın dağıtabilirsiniz.

### <a name="connectivity-requirements"></a>Bağlantı gereksinimleri
1. Azure yığın Kullanıcı Portalı erişim bilgisayardan web tarayıcısı üzerinden Windows Azure paketi Kiracı portalı erişebildiğinden emin olun.
2. Azure yığın AzS WASP01 sanal makinede Microsoft Azure Pack Kiracı portalı bilgisayara bağlanabilir olmalıdır. Ağ bağlantısını doğrulamak için ping.exe kullanın. 
3.  Yeni Bağlayıcı Hizmetleri için geçerli sertifikaların olması gerekir. Bu SSL sertifikaları güvenilen sertifika yetkilisi (CA) tarafından verilmesi gerekir. Otomatik olarak imzalanan sertifikalar kullanamazsınız. SSL sertifikaları Azure yığını (özellikle AzS WASP01 VM) ve Kiracı Azure yığın kullanıcı portalına erişmek için kullandığınız herhangi bir bilgisayar tarafından güvenilmesi gerekir.
 
    >[!NOTE]
    Sunucu Çekirdeği yükleme seçeneğiyle Windows Server AzS WASP01 çalıştığı için sertifikayı içeri aktarmak için Certutil.ext gibi komut satırı aracını kullanabilirsiniz. Örneğin, AzS WASP01 üzerinde c:\temp .cer dosyasını kopyalayın ve ardından komutu çalıştırın **certutil - addstore "CA" "c:\temp\certname.cer"**.

4.  Windows Azure paketi Kiracı sanal makinelere Azure yığın portal üzerinden RDP bağlantı kurmak için Microsoft Azure Pack ortamında Kiracı VM'ler için Uzak Masaüstü trafiğe izin vermelidir.
5.  Azure yığın Kiracı sanal makinelerini ve Windows Azure paketi Kiracı sanal makineleri arasındaki bağlanabilirlik için dış IP adreslerini iki ortamlar genelinde yönlendirilebilir olması gerekir. Bu bağlantı ortamlar arasında sanal makine adları çözümlemek için bir DNS sunucusu ilişkilendirme de içerebilir.

### <a name="iis-requirements"></a>IIS gereksinimleri
(Olabilen bir fiziksel ana bilgisayar, sanal makine ya da birden çok sanal makine) Windows Azure paketi Kiracı portalı barındıran bilgisayarda yüklü URL yeniden yazma IIS uzantısı olması gerekir. Zaten yüklü değilse, buradan indirebilirsiniz [burada](https://www.iis.net/downloads/microsoft/url-rewrite). Birden çok sanal makine Kiracı portalı barındırıyorsanız, uzantı her sanal makineye yükleyin.

## <a name="configure-azure-stack"></a>Azure yığını yapılandırma
Windows Azure Pack bağlayıcı yapılandırmadan önce Azure yığın Kullanıcı Portalı'nda çok bulut modunu etkinleştirmeniz gerekir. Bu mod hangi buluttan kaynaklarına erişmek için kullanıcıların seçmesine olanak sağlar.

Birden çok bulut modunu etkinleştirmek için Azure yığın dağıtımdan sonra Ekle AzurePackConnector.ps1 komut dosyasını çalıştırmanız gerekir. Aşağıdaki tablo, betik parametreleri açıklar.


|  Parametre | Açıklama | Örnek |   
| -------- | ------------- | ------- |  
| AzurePackClouds | Windows Azure Pack bağlayıcılar URI. Bu URI için Windows Azure paketi Kiracı portalı karşılık gelmelidir. | @{CloudName = "AzurePack1"; CloudEndpoint = "https://waptenantportal1:40005"},@{CloudName = "AzurePack2"; CloudEndpoint = "https://waptenantportal2:40005"}<br><br>  (Varsayılan olarak, bağlantı noktası 40005 değerdir.) |  
| AzureStackCloudName | Yerel Azure yığın bulut göstermek için etiket.| "AzureStack" |
| DisableMultiCloud | Birden çok bulut modu devre dışı bırakmak için bir anahtar.| Yok |
| | |

Dağıtım, hemen sonra veya üzeri Ekle AzurePackConnector.ps1 komut dosyası çalıştırabilirsiniz. Dağıtımdan hemen sonra komut dosyasını çalıştırmak için burada Azure yığın dağıtımını tamamladığınız aynı Windows PowerShell oturumu kullanın. Aksi takdirde (azurestackadmin hesabıyla açtığınız) yönetici olarak yeni bir Windows PowerShell oturumu açabilirsiniz.

1. Add-AzurePackConnector.ps1 komut dosyası aşağıdaki komutlarla (değerleri ortamınıza özgü) kullanarak çalıştırın. Add-AzurePackConnector komut dosyası, birden fazla Windows Azure Pack bağlayıcı uç eklemenize olanak tanır dikkat edin.
 
 ```powershell
    $cred = New-Object System.Management.Automation.PSCredential("cloudadmin@azurestack.local", `
    (ConvertTo-SecureString -String "<password>" -AsPlainText -Force))
    $session = New-PSSession -ComputerName 'azs-ercs01' `
     -Credential $cred `
     -ConfigurationName PrivilegedEndpoint `
     -Authentication Credssp

    # Enable Multicloud
    Invoke-Command -Session $session -ScriptBlock { Add-AzurePackConnector -AzurePackClouds `
    @{CloudName = "AzurePack_1"; CloudEndpoint = "https://waptenantportal1:40005"},`
    @{CloudName = "AzurePack_2"; CloudEndpoint = "https://waptenantportal2:40005" } `
    -AzureStackCloudName "AzureStack" }

 ```
> [!NOTE]
> Geçerli yapı içinde olduğunda bir sorun Ekle AzurePackConnector komut bittikten sonra bir Yoklama Döngüsü süresi (birkaç dakika) uzun bir süre için kadar kalır burada sona erer. İleti gördükten sonra **VERBOSE: adım 'Azure Pack Connector'ı Yapılandır' durum: 'Başarılı'**, betiği veya tek başına durduruluncaya kadar bekleyin. Yapılandırma başarılı oldu çünkü bir fark yapmaz.

2. Bu komut, belirttiğiniz her Windows Azure Pack ortam için bir tarafından üretilen çıkış dosyalarının not edin. Dosyaları şu adreste bulunabilir: \\\su1fileserver\SU1_Infrastructure_1\AzurePackConnectorOutput. Bu dosyalar, hedef Windows Azure Pack ortamlarını yapılandırmak için gereken bilgileri içerir. Bir komut dosyası daha sonra bu yönergeleri için parametre olarak bu dosyayı geçirin. Bu dosya, aşağıdaki bilgileri içerir:

    * **AzurePackConnectorEndpoint**: Windows Azure Pack bağlayıcı hizmeti için uç nokta içerir.
    * **AuthenticationIdentityProviderPartner**: aşağıdaki değer çifti içerir:
        * Windows Azure paketi Kiracı API'si Azure yığın portal uzantısı gelen çağrıları kabul etmek için güven gerekiyor sertifika imzalama kimlik doğrulama belirteci.

        * "Bölgesi" imzalama sertifikası ile ilişkilendirilmiş. Örneğin: https://adfs.local.azurestack.global.external/adfs/c1d72562-534e-4aa5-92aa-d65df289a107/.

3.  Çıkış dosyaları içeren klasöre gidin (\\su1fileserver\SU1_Infrastructure_1\AzurePackConnectorOutput) ve dosyalarını yerel bilgisayarınıza kopyalayın. Dosyalar için şuna benzer: AzurePack-06-27-15-50.txt.

4.  Yapılandırmayı test etme.

    a. Tarayıcınızı açın ve Azure yığın Kullanıcı Portalı'na (https://portal.local.azurestack.external/) oturum açın.
    
    b. Bir kiracı ve portal yükleri oturum açtıktan sonra abonelikleri veya uzantılar Azure Pack buluttan fetch yazdıramama ilgili hatalar görürsünüz. Tıklatın **Tamam** bu iletiler kapatın. (Microsoft Azure Pack yapılandırdıktan sonra bu hata iletileri kaybolur.)

    c. Bildirim **bulut** portalın sol üst köşedeki aşağı açılan liste.

    ![Azure yığın kullanıcı arabiriminde bulut Seçici](media/azure-stack-manage-wap/image3.png)

## <a name="configure-windows-azure-pack"></a>Yapılandırma Windows Azure paketi
Bu bağlayıcı yalnızca önizleme sürümü için Microsoft Azure Pack elle yapılandırma gerektirir.

>[!IMPORTANT]
Yalnızca test ortamları ve üretim dağıtımlarında, bu Önizleme sürümü için Windows Azure Pack Bağlayıcısı'nı kullanın.

1.  Windows Azure paketi Kiracı portalı sanal makine bağlayıcı MSI dosyalarını yüklemek ve sertifikaları yükleyin. (Birden çok Kiracı portalı sanal makineler varsa, her sanal makinede bu adımı tamamlamalısınız.)

    a. Dosya Gezgini'nde, kopyalama **WAPConnector** klasörü (ne daha önce indirdiğiniz) adlı bir klasöre **c:\temp** Kiracı portalı sanal makinede.

    b. Kiracı portalı sanal makineye bir konsolu veya RDP bağlantı açın.

    c. Değiştirme dizinleri **c:\temp\wapconnector\setup\scripts**, çalıştırıp **yükleme Connector.ps1** üç MSI dosyaları yüklemek için komut dosyası. Hiçbir parametre gerekli değildir.

     ```powershell
    cd C:\temp\wapconnector\setup\scripts\

    .\Install-Connector.ps1
    ```
     d. Değiştirme dizinleri **c:\inetpub** ve üç yeni siteler yüklü olduğunu doğrulayın:

       * MgmtSvc-Connector

       * MgmtSvc-ConnectorExtension

       * MgmtSvc-ConnectorController

    e. Aynı **c:\temp\wapconnector\setup\scripts** klasöründe şunu çalıştırın **yapılandırma Certificates.ps1** sertifikaları yüklemek için komut dosyası. Varsayılan olarak, Windows Azure Pack Kiracı Portal sitesi için kullanılabilir aynı sertifikayı kullanır. Bu (Azure yığın AzS-WASP01 sanal makine ile Azure yığın portala erişir herhangi bir istemci bilgisayar tarafından güvenilen) geçerli bir sertifika olduğundan emin olun. Aksi takdirde, iletişim çalışmaz. (Alternatif olarak, açıkça bir sertifika parmak izi parametre olarak kullanarak geçirebilirsiniz parmak izi parametresi.)

     ```powershell
        cd C:\temp\wapconnector\setup\scripts\

        .\Configure-Certificates.ps1
    ```

    f. Bu üç Hizmetleri yapılandırmasını tamamlamak için çalıştırmanız **yapılandırma WapConnector.ps1** Web.config dosyası parametrelerine güncelleştirmek için komut dosyası.

    |  Parametre | Açıklama | Örnek |   
    | -------- | ------------- | ------- |  
    | TenantPortalFQDN | Windows Azure paketi Kiracı portalı FQDN. | tenant.contoso.com | 
    | TenantAPIFQDN | Windows Azure paketi Kiracı API'si FQDN'SİDİR. | tenantapi.contoso.com  |
    | AzureStackPortalFQDN | Azure yığın Kullanıcı Portalı FQDN. | portal.local.azurestack.external |
    | | |
    
     ```powershell
    .\Configure-WapConnector.ps1 -TenantPortalFQDN "tenant.contoso.com" `
         -TenantAPIFQDN "tenantapi.contoso.com" `
         -AzureStackPortalFQDN "portal.local.azurestack.external"
    ```
    g. 1. adım, birden çok Kiracı portalı sanal makineler varsa, her bu sanal makineler için yineleyin.

2. Yeni Kiracı API'si MSI her Windows Azure paketi Kiracı API'si sanal makineye yükleyin.

    a. Bir yük dengeleyici kullanımdaysa, sanal makineyi çevrimdışı olarak işaretlemek isteyebilirsiniz.

    b. Dosya Gezgini'nde, kopyalama **WAPConnector** klasöründen adlı bir klasöre **c:\temp** her Kiracı API'si makinede.

    c. Daha önce kaydettiğiniz AzurePackConnectorOutput.txt dosyasını kopyalamak için **c:\temp\WAPConnector**.

    d. Kiracı API'si dosyaları kopyaladığınız VM konsolu veya RDP bağlantı açın.
    
    e. Değiştirme dizinleri **c:\temp\wapconnector\setup\scripts**, çalıştırıp **güncelleştirme TenantAPI.ps1**. Bu yeni sürümü WAP Kiracı API'si, yalnızca geçerli STS ile aynı zamanda Azure yığında AD FS örneği ile bir güven ilişkisi etkinleştirmek için bir değişiklik içerir.

     ```powershell
    cd C:\temp\wapconnector\setup\packages\

    .\Update-TenantAPI.ps1
    ```

    f.  Tüm diğer Kiracı API'si çalıştıran sanal makine 2 adımı yineleyin.
3. Gelen **yalnızca bir** Azure yığında Kiracı API'si ve AD FS örneği arasında bir güven ilişkisi eklemek için yapılandırma TrustAzureStack.ps1 komut dosyasını çalıştırmak Kiracı API'si VM'lerin. Microsoft.MgmtSvc.Store veritabanının sysadmin erişimi olan bir hesabı kullanmanız gerekir. Bu betiği aşağıdaki parametreleri içerir:

    | Parametre | Açıklama | Örnek |
    | --------- | ------------| ------- |
   | SqlServer | Microsoft.MgmtSvc.Store veritabanını içeren SQL Server'ın adı. Bu parametre gereklidir. | SQLServer | 
   | DataFile | Azure yığını çok bulut modu yapılandırmasını sırasında daha önce oluşturulan çıktı dosyası. Bu parametre gereklidir. | AzurePack-06-27-15-50.txt | 
   | PromptForSqlCredential | Komut dosyası, SQL Server örneğine bağlanırken kullanılacak etkileşimli olarak SQL kimlik doğrulaması kimlik bilgileri ister gösterir. Verilen kimlik bilgisi veritabanları, şemalar, kaldırmak için yeterli izinlere sahip ve kullanıcı oturumu silmeniz gerekir. Sağlanırsa, komut dosyası, geçerli kullanıcı içeriğini erişimi varsayar. | Herhangi bir değer gereklidir. |
   |  |  |

    SQL Server kullanacak şekilde bilmiyorsanız, bulabilir. Kiracı API'si bilgisayara bağlanın, Kiracı API'si Web.config dosyasını korumasını ve sunucu adı bağlantı dizesinde aramak için Unprotect-MgmtSvc komutunu kullanın. Kiracı API'si Web.config dosyasını yeniden korumak için koruma-MgmtSvc çalıştırmak unutmayın.

  ```powershell
  cd C:\temp\wapconnector\setup\scripts\

 .\Configure-TrustAzureStack.ps1 -SqlServer "SQLServer" `
       -DataFile "C:\temp\wapconnector\AzurePackConnectorOutput.txt"
  ```

## <a name="example"></a>Örnek
Aşağıdaki örnek, bir tek düğümlü Azure yığın yapılandırması üzerinde tam bir Windows Azure Pack bağlayıcı dağıtımı ve iki Windows Azure Pack Express yüklemeleri gösterir. (Her Express örnek adlar ile tek bir bilgisayara yüklendikten *wapcomputer1* ve*wapcomputer2*.)

```powershell
# Run the following script on the Azure Stack host
$cred = New-Object System.Management.Automation.PSCredential("cloudadmin@azurestack.local",`
     (ConvertTo-SecureString -String "p@ssw0rd" -AsPlainText -Force))
$session = New-PSSession -ComputerName 'azs-ercs01' -Credential $cred `
     -ConfigurationName PrivilegedEndpoint -Authentication Credssp
 
# Enable Multicloud
invoke-command -Session $session -ScriptBlock { Add-AzurePackConnector -AzurePackClouds `
     @{CloudName = "AzurePack_1"; CloudEndpoint = "https://wapcomputer1.contoso.com:40005"},`
     @{CloudName = "AzurePack_2"; CloudEndpoint = "https://wapcomputer2.contoso.com:40005"}`
     -AzureStackCloudName "AzureStack" }  

```
.exe dosyasını indirin [Microsoft Download Center](https://aka.ms/wapconnectorazurestackdlc)ayıklayın ve WAPConnector klasörüne kopyalayın bir **c:\temp** Microsoft Azure Pack bilgisayardaki klasör. Önceki komut çıktısında olarak oluşturulan dosyaları kopyalayın (konumunda bulunan \\\su1fileserver\SU1_Infrastructure_1\AzurePackConnectorOutput) için **c:\temp\WAPConnector** klasör. (Dosyalar şuna benzer görünür olur: AzurePack-06-27-15-50.txt.) Ardından, Microsoft Azure Pack örneği başına bir kez aşağıdaki betiği çalıştırın:

 ```powershell
# Install Connector components
cd C:\temp\WAPConnector\Setup\Scripts
.\Install-Connector.ps1

# Configure Certificates for the new Connector services
.\Configure-Certificates.ps1

# Configure the Connector services
.\Configure-WapConnector.ps1 -TenantPortalFQDN "wapcomputer1.contoso.com" `
     -TenantAPIFQDN "wapcomputer1.contoso.com" `
     -AzureStackPortalFQDN "portal.local.azurestack.external"

# Install the updated TenantAPI
.\Update-TenantAPI.ps1

# Establish trust with the Azure Stack AD FS
.\Configure-TrustAzureStack.ps1 -SqlServer "wapcomputer1" `
     -DataFile "C:\temp\wapconnector\AzurePack-06-27-15-50.txt" 

```
## <a name="troubleshooting-tips"></a>Sorun giderme ipuçları
1.  Azure yığını ve Microsoft Azure Pack arasında ağ bağlantısı olduğundan emin olun. Bu bağlantı Azure yığın portala erişir herhangi bir kiracı bilgisayar ve Windows Azure paketi Kiracı portalı sanal makine arasında yeni Bağlayıcı Hizmetleri çalıştırdığı olması gerekir.
2.  Belirtilen tüm FQDN'ler doğru olduğundan emin olun.
3.  Azure yığını (özellikle AzS WASP01 VM) tarafından güvenilen yeni Bağlayıcı Hizmetleri kullanılan SSL sertifikaları ve başka bir bilgisayarda Kiracı Azure yığın kullanıcı portalına erişmek için kullandığınız emin olun.
4. Bilinen sorunlar için bkz: [Microsoft Azure yığın sorun giderme](azure-stack-troubleshooting.md).


## <a name="next-steps"></a>Sonraki adımlar
[Azure yığınında yönetici ve kullanıcı portalı kullanma](azure-stack-manage-portals.md)
