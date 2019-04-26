---
title: "Öğretici:  Parola Karması eşitleme (PHS) kullanarak azure'da tek bir AD ormanında tümleştirme | Microsoft Docs"
description: Parola Karması eşitleme kullanarak bir karma kimlik ortamını ayarlama işlemi gösterilmektedir.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/17/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 45379f8f955c50e2598ebcebd34e971c29b2c81c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60295701"
---
# <a name="tutorial--integrate-a-single-ad-forest-using-password-hash-sync-phs"></a>Öğretici:  Parola karması eşitleme (PHS) kullanarak tek bir AD ormanında tümleştirme

![Oluştur](media/tutorial-password-hash-sync/diagram.png)

Aşağıdaki öğreticiye parola karması eşitleme kullanarak bir karma kimlik ortamı oluşturma işleminde size yol gösterir.  Bu ortam, test etmek veya bir karma kimlik nasıl çalışır hakkında daha fazla bilgi almak için kullanılabilir.

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticiyi tamamlamak için gereken önkoşulları şunlardır:
- Bir bilgisayarla [Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-technology-overview) yüklü.  Üzerinde ya da bunu yapmak için önerilen bir [Windows 10](https://docs.microsoft.com/virtualization/hyper-v-on-windows/about/supported-guest-os) veya [Windows Server 2016](https://docs.microsoft.com/windows-server/virtualization/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows) bilgisayar.
- Bir [dış ağ bağdaştırıcısı](https://docs.microsoft.com/virtualization/hyper-v-on-windows/quick-start/connect-to-network) sanal makinenin internet ile iletişim kurmasına izin vermek için.
- [Azure aboneliği](https://azure.microsoft.com/free)
- Windows Server 2016'ın bir kopyalama

> [!NOTE]
> Bu öğreticide, hızlı sürede öğretici ortamı oluşturabilmeniz PowerShell betikleri kullanılır.  Her komut betikleri başında bildirilen değişkenleri kullanır.  Yapabilir ve ortamınızı yansıtmak üzere değişkenleri değiştirmeniz gerekir.
>
>Kullanılan komut dosyaları, Azure AD Connect'i yüklemeden önce genel bir Active Directory ortamı oluşturun.  Bunlar tüm öğreticileri için uygundur.
>
> Bu öğreticide kullanılan PowerShell betikleri kopyalarını Github'da bulunmaktadır [burada](https://github.com/billmath/tutorial-phs).

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma
Şirket içi Active Directory Sunucumuz kullanılacak bir sanal makine oluşturmak için çalışan yapmak için karma kimlik ortamımızda ürününü ihtiyacımız ve ilk şey.  Şunları yapın:

1. Yönetici olarak PowerShell ISE'yi açın.
2. Aşağıdaki betiği çalıştırın.

```powershell
#Declare variables
$VMName = 'DC1'
$Switch = 'External'
$InstallMedia = 'D:\ISO\en_windows_server_2016_updated_feb_2018_x64_dvd_11636692.iso'
$Path = 'D:\VM'
$VHDPath = 'D:\VM\DC1\DC1.vhdx'
$VHDSize = '64424509440'

#Create New Virtual Machine
New-VM -Name $VMName -MemoryStartupBytes 16GB -BootDevice VHD -Path $Path -NewVHDPath $VHDPath -NewVHDSizeBytes $VHDSize  -Generation 2 -Switch $Switch  

#Set the memory to be non-dynamic
Set-VMMemory $VMName -DynamicMemoryEnabled $false

#Add DVD Drive to Virtual Machine
Add-VMDvdDrive -VMName $VMName -ControllerNumber 0 -ControllerLocation 1 -Path $InstallMedia

#Mount Installation Media
$DVDDrive = Get-VMDvdDrive -VMName $VMName

#Configure Virtual Machine to Boot from DVD
Set-VMFirmware -VMName $VMName -FirstBootDevice $DVDDrive 
```

## <a name="complete-the-operating-system-deployment"></a>İşletim sistemi dağıtımını tamamlama
Sanal makine oluşturma işlemini tamamlamak için işletim sistemi yüklemesinin tamamlanması gerekir.

1. Hyper-V Yöneticisi, sanal makinede çift tıklatın
2. Başlat düğmesine tıklayın.
3. 'CD veya DVD'den önyükleme için bir tuşa basın için ' istenir. Devam edin ve bunu yapın.
4. Windows Server başlangıç ekranı dilinizi seçin ve tıklayın **sonraki**.
5. Tıklayın **Şimdi Yükle**.
6. Lisans anahtarınızı girip __iade **sonraki**.
7. Denetleme ** lisans koşullarını kabul edin ve tıklayın **sonraki**.
8. Seçin **özel:  Yalnızca Windows yükleyin (Gelişmiş)**
9. **İleri**’ye tıklayın
10. Yükleme tamamlandıktan sonra VM'yi en güncel olduğundan emin olmak için oturum açma ve çalıştırma Windows güncelleştirmeleri, sanal makineyi yeniden başlatın.  En son güncelleştirmeleri yüklersiniz.

## <a name="install-active-directory-prerequisites"></a>Active Directory önkoşulları yükleyin
Bir sanal makine sahibiz, Active Directory yüklemeden önce birkaç şey yapmanız gerekir.  Diğer bir deyişle, sanal makine yeniden adlandırma, bir statik IP adresi ve DNS bilgilerini ayarlayabilir ve Uzak Sunucu Yönetim Araçları'nı yükleme ihtiyacımız var.   Şunları yapın:

1. Yönetici olarak PowerShell ISE'yi açın.
2. Aşağıdaki betiği çalıştırın.

```powershell
#Declare variables
$ipaddress = "10.0.1.117" 
$ipprefix = "24" 
$ipgw = "10.0.1.1" 
$ipdns = "10.0.1.117"
$ipdns2 = "8.8.8.8" 
$ipif = (Get-NetAdapter).ifIndex 
$featureLogPath = "c:\poshlog\featurelog.txt" 
$newname = "DC1"
$addsTools = "RSAT-AD-Tools" 

#Set static IP address
New-NetIPAddress -IPAddress $ipaddress -PrefixLength $ipprefix -InterfaceIndex $ipif -DefaultGateway $ipgw 

# Set the DNS servers
Set-DnsClientServerAddress -InterfaceIndex $ipif -ServerAddresses ($ipdns, $ipdns2)

#Rename the computer 
Rename-Computer -NewName $newname -force 

#Install features 
New-Item $featureLogPath -ItemType file -Force 
Add-WindowsFeature $addsTools 
Get-WindowsFeature | Where installed >>$featureLogPath 

#Restart the computer 
Restart-Computer
```

## <a name="create-a-windows-server-ad-environment"></a>Windows Server AD ortamı oluşturma
Oluşturulan VM sunuyoruz ve yeniden adlandırıldı ve bir statik IP adresine sahip olduğunuzdan, biz devam edin ve yükleyebilir ve Active Directory Domain Services'ı yapılandırın.  Şunları yapın:

1. Yönetici olarak PowerShell ISE'yi açın.
2. Aşağıdaki betiği çalıştırın.

```powershell 
#Declare variables
$DatabasePath = "c:\windows\NTDS"
$DomainMode = "WinThreshold"
$DomainName = "contoso.com"
$DomaninNetBIOSName = "CONTOSO"
$ForestMode = "WinThreshold"
$LogPath = "c:\windows\NTDS"
$SysVolPath = "c:\windows\SYSVOL"
$featureLogPath = "c:\poshlog\featurelog.txt" 
$Password = "Pass1w0rd"
$SecureString = ConvertTo-SecureString $Password -AsPlainText -Force

#Install AD DS, DNS and GPMC 
start-job -Name addFeature -ScriptBlock { 
Add-WindowsFeature -Name "ad-domain-services" -IncludeAllSubFeature -IncludeManagementTools 
Add-WindowsFeature -Name "dns" -IncludeAllSubFeature -IncludeManagementTools 
Add-WindowsFeature -Name "gpmc" -IncludeAllSubFeature -IncludeManagementTools } 
Wait-Job -Name addFeature 
Get-WindowsFeature | Where installed >>$featureLogPath

#Create New AD Forest
Install-ADDSForest -CreateDnsDelegation:$false -DatabasePath $DatabasePath -DomainMode $DomainMode -DomainName $DomainName -SafeModeAdministratorPassword $SecureString -DomainNetbiosName $DomainNetBIOSName -ForestMode $ForestMode -InstallDns:$true -LogPath $LogPath -NoRebootOnCompletion:$false -SysvolPath $SysVolPath -Force:$true
```

## <a name="create-a-windows-server-ad-user"></a>Bir Windows Server AD kullanıcısı oluşturun
Active Directory ortamımızda sahibiz, bir test hesabına ihtiyacımız var.  Bu hesap, şirket içinde oluşturulacak AD ortamını ve ardından Azure AD'ye eşitlenen.  Şunları yapın:

1. Yönetici olarak PowerShell ISE'yi açın.
2. Aşağıdaki betiği çalıştırın.

```powershell 
#Declare variables
$Givenname = "Allie"
$Surname = "McCray"
$Displayname = "Allie McCray"
$Name = "amccray"
$Password = "Pass1w0rd"
$Identity = "CN=ammccray,CN=Users,DC=contoso,DC=com"
$SecureString = ConvertTo-SecureString $Password -AsPlainText -Force


#Create the user
New-ADUser -Name $Name -GivenName $Givenname -Surname $Surname -DisplayName $Displayname -AccountPassword $SecureString

#Set the password to never expire
Set-ADUser -Identity $Identity -PasswordNeverExpires $true -ChangePasswordAtLogon $false -Enabled $true
```

## <a name="create-an-azure-ad-tenant"></a>Azure AD kiracısı oluşturma
Artık Azure AD kiracısı oluşturun, böylece biz kullanıcılarımız için bulut eşitlemek ihtiyacımız var.  Yeni bir Azure AD oluşturmak için Kiracı, aşağıdakileri yapın.

1. Gözat [Azure portalında](https://portal.azure.com) ve bir Azure aboneliğine sahip bir hesapla oturum açın.
2. Seçin **simgesini (+) artı** araması **Azure Active Directory**.
3. Seçin **Azure Active Directory** arama sonuçlarında.
4. **Oluştur**’u seçin.</br>
![Oluşturma](media/tutorial-password-hash-sync/create1.png)</br>
5. Sağlayan bir **kuruluş adı** ile birlikte **ilk etki alanı adı**. Ardından **Oluştur**’u seçin. Bu dizin oluşturur.
6. Bu tamamlandıktan sonra tıklayın **burada** dizini yönetmek için bağlantı.

## <a name="create-a-global-administrator-in-azure-ad"></a>Azure AD genel Yöneticisi oluşturma
Azure AD kiracısı sahibiz, genel yönetici hesabını oluşturacağız.  Bu hesap, Azure AD Connect yüklemesi sırasında Azure AD Bağlayıcısı hesabı oluşturmak için kullanılır.  Azure AD Bağlayıcısı hesabı bilgileri Azure AD'ye yazmak için kullanılır.   Genel yönetici oluşturmak için hesabı aşağıdakileri yapın.

1.  **Yönet** bölümünde **Kullanıcılar**’ı seçin.</br>
![Oluşturma](media/tutorial-password-hash-sync/gadmin1.png)</br>
2.  Seçin **tüm kullanıcılar** seçip **+ yeni kullanıcı**.
3.  Bu kullanıcı için bir ad ve kullanıcı adı sağlayın. Bu kiracının genel Yöneticisi olacaktır. Ayrıca değiştirmek isteyeceksiniz **dizin rolü** için **genel yönetici.** Geçici parolayı da gösterebilirsiniz. İşiniz bittiğinde **Oluştur**’u seçin.</br>
![Oluşturma](media/tutorial-password-hash-sync/gadmin2.png)</br>
4. Bu tamamlandıktan sonra yeni bir web tarayıcısı açın ve yeni genel yönetici hesabını ve geçici parolayı kullanarak myapps.microsoft.com için oturum açın.
5. Genel yönetici, hatırlanır bir şey için parolayı değiştirin.

## <a name="download-and-install-azure-ad-connect"></a>Azure AD Connect yükleme ve indirme
Artık Azure AD Connect karşıdan yüklenip kurulacak zamanı geldi.  Yüklendikten sonra hızlı yükleme çalıştıracağız.  Şunları yapın:

1. İndirme [Azure AD'ye bağlanma](https://www.microsoft.com/download/details.aspx?id=47594)
2. **AzureADConnect.msi** öğesine gidin ve çift tıklayın.
3. Hoş Geldiniz ekranında, lisans koşullarını kabul ettiğinizi belirten kutuyu seçin ve **Devam**'a tıklayın.  
4. Hızlı ayarlar ekranında **Hızlı ayarları kullan**'a tıklayın.</br>  
![Oluşturma](media/tutorial-password-hash-sync/express1.png)</br>
5. Bağlanma ekranında Azure AD, Azure AD için kullanıcı ve genel yönetici parolasını girin. **İleri**’ye tıklayın.  
6. AD DS'ye Bağlanma ekranında kuruluş yöneticisi hesabına ilişkin kullanıcı adını ve parolayı girin. **İleri**’ye tıklayın.  
7. Yapılandırma için hazır ekranında **Yükle**'ye tıklayın.
8. Yükleme tamamlandığında **Çıkış**'a tıklayın.
9. Yükleme tamamlandıktan sonra oturumu kapatın ve Eşitleme Hizmeti Yöneticisi'ni veya Synchronization Rule Editor'ı kullanmadan önce yeniden oturum açın.


## <a name="verify-users-are-created-and-synchronization-is-occurring"></a>Kullanıcılar oluşturulur ve eşitleme gerçekleşen doğrulayın
Artık, şirket içi dizininde vardı kullanıcıları eşitlenmiştir ve artık Azure AD kiracısı mevcut doğrulayacağız.  Bunun tamamlanması birkaç saat sürebileceğini unutmayın.  Kullanıcı eşitlenmişse doğrulamak için aşağıdakileri yapın.


1. Gözat [Azure portalında](https://portal.azure.com) ve bir Azure aboneliğine sahip bir hesapla oturum açın.
2. Sol tarafta, seçin **Azure Active Directory**
3. **Yönet** bölümünde **Kullanıcılar**’ı seçin.
4. Yeni kullanıcılar kendi kiracısında gördüğünüzü doğrulayın</br>
![Eşitleme](media/tutorial-password-hash-sync/synch1.png)</br>

## <a name="test-signing-in-with-one-of-our-users"></a>Kullanıcılarımızın biriyle oturum açma testi

1. Göz atın [https://myapps.microsoft.com](https://myapps.microsoft.com)
2. Sunduğumuz yeni kiracıda oluşturduğunuz kullanıcı hesabı ile oturum.  Aşağıdaki biçimi kullanarak oturum açmanız gerekir: (user@domain.onmicrosoft.com). Kullanıcının oturum açmak için kullandığı aynı parolayı kullanan şirket içi.</br>
   ![Doğrulayın](media/tutorial-password-hash-sync/verify1.png)</br>

Şimdi başarıyla test edin ve hangi Azure sunduğu ile kendinizi alıştırın için kullanabileceğiniz bir karma kimlik ortamı vardır.

## <a name="next-steps"></a>Sonraki Adımlar


- [Donanım ve önkoşullar](how-to-connect-install-prerequisites.md) 
- [Hızlı ayarlar](how-to-connect-install-express.md)
- [Parola karması eşitlemesi](how-to-connect-password-hash-synchronization.md)|
