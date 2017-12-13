---
title: "Azure yığın Geliştirme Seti dağıtma | Microsoft Docs"
description: "Azure yığın Geliştirme Seti hazırlamak ve bunu dağıtmak için PowerShell betiğini çalıştırmak öğrenin."
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 0ad02941-ed14-4888-8695-b82ad6e43c66
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/11/2017
ms.author: jeffgilb
ms.reviewer: wfayed
ms.openlocfilehash: 7c320c6ba51ae0800407aab7aee92c42b2b441a7
ms.sourcegitcommit: d247d29b70bdb3044bff6a78443f275c4a943b11
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2017
---
# <a name="deploy-the-azure-stack-development-kit"></a>Azure yığın Geliştirme Seti dağıtma

*Uygulandığı öğe: Azure yığın Geliştirme Seti*

Dağıtmak için [Azure yığın Geliştirme Seti](azure-stack-poc.md), aşağıdaki adımları tamamlamanız gerekir:

1. [Dağıtım paketini karşıdan](https://azure.microsoft.com/overview/azure-stack/try/?v=try) Cloudbuilder.vhdx alınamıyor.
2. Geliştirme Seti yüklemek istediğiniz bilgisayarı (Geliştirme Seti ana bilgisayarı) yapılandırmak için cloudbuilder.vhdx hazırlayın. Bu adımdan sonra Geliştirme Seti konak Cloudbuilder.vhdx önyükleme yapmaz.
3. Geliştirme Seti konak Geliştirme Seti dağıtın.

> [!NOTE]
> Bağlantısı kesilmiş bir Azure yığın ortam kullanmak istiyorsanız bile, en iyi sonuçlar için internet'e bağlı değilken dağıtmak en iyisidir. Böylece, dağıtım sırasında Geliştirme Seti yüklemeye dahil Windows Server 2016 Değerlendirme sürümü etkinleştirilebilir.

## <a name="download-and-extract-the-development-kit"></a>İndirmeyi ve ayıklamayı Geliştirme Seti
1. Yükleme başlamadan önce bilgisayarınızın aşağıdaki önkoşulları karşıladığından emin olun:

  - Bilgisayarda en az 60 Ayrıca işletim sistemi diski boş dört ayrı, aynı mantıksal sabit sürücülerde disk alanı GB olmalıdır.
  - [.NET framework 4.6 (veya sonraki bir sürümünü)](https://aka.ms/r6mkiy) yüklü olması gerekir.

2. [Gidilecek Başlarken sayfa](https://azure.microsoft.com/overview/azure-stack/try/?v=try) burada Azure yığın Geliştirme Seti indirebilir, bilgilerinizi verin ve ardından **gönderme**.
3. İndirme ve çalıştırma [Azure yığın Geliştirme Seti için dağıtım denetleyicisi](https://go.microsoft.com/fwlink/?LinkId=828735&clcid=0x409) önkoşul denetleyicisini komut dosyası. Bu tek başına komut dosyası Azure yığın Geliştirme Seti için kurulumu tarafından gerçekleştirilen bir ön koşul denetimleri geçer. Azure yığın Geliştirme Seti için büyük paketini yüklemeden önce donanım ve yazılım gereksinimleri karşılayıp onaylamak için bir yol sağlar.
4. Altında **yazılımı yüklemek**, tıklatın **Azure yığın Geliştirme Seti**.

  > [!NOTE]
  > ASDK yükleme (AzureStackDevelopmentKit.exe) approximiately 10 GB olan tek başına. Ayrıca Windows Server 2016 Değerlendirme sürümü ISO dosyasını karşıdan yüklemeyi seçerseniz, indirme boyutu yaklaşık 17 GB artırır. Bu ISO dosyasını oluşturup ASDK yükleme tamamlandıktan sonra Azure yığın Markete bir Windows Server 2016 sanal makine görüntüsü eklemek için kullanabilirsiniz. Bu Windows Server 2016 değerlendirme görüntü yalnızca ASDK ile kullanılabilir ve ASDK Lisans Koşulları'na bağlı olduğunu unutmayın.

5. Yükleme tamamlandıktan sonra **çalıştırmak** ASDK ayıklayıcısı (AzureStackDevelopmentKit.exe) başlatın.
6. Gözden geçirin ve görüntülenen Lisans Sözleşmesi'nden kabul **Lisans Sözleşmesi'ni** ayıklayıcısı Sihirbazı'nı ve ardından sayfanın **sonraki**.
7. Görüntülenen gizlilik bildirimi bilgilerini gözden **önemli duyuru** ayıklayıcısı Sihirbazı'nı ve ardından sayfanın **sonraki**.
8. Göre ayıklanacak Azure yığın kurulum dosyalarının konumunu seçin **hedef konum seçin** ayıklayıcısı Sihirbazı'nı ve ardından sayfanın **sonraki**. Varsayılan konum *geçerli klasörde*\Azure yığın Geliştirme Seti. 
9. Hedef konumun özetini gözden **Extract hazır** ayıklayıcısı Sihirbazı'nı ve ardından sayfanın **ayıklamak** CloudBuilder.vhdx (yaklaşık 25 GB) ayıklayın ve ThirdPartyLicenses.rtf dosyaları. Bu işlemin tamamlanması biraz zaman alır.
10. Kopyalama veya CloudBuilder.vhdx dosyayı C:\ sürücüsü (C:\CloudBuilder.vhdx) kök dizinine ASDK ana bilgisayara taşıyın.

> [!NOTE]
> Dosyaları ayıkla sonra silebilirsiniz. EXE ve. Sabit disk alanını kurtarmak için DEPO dosyaları. Ya da ASDK dağıtmanız gerekiyorsa dosyaları yeniden karşıdan yüklemeniz gerekmez, bu dosyaları yedekleyebilir.

## <a name="deploy-the-asdk-using-a-guided-experience"></a>Kılavuzlu bir deneyim kullanarak ASDK dağıtma
ASDK yükleme ve asdk installer.ps1 PowerShell Betiği çalıştırma sağlanan bir grafik kullanıcı arabirimi (GUI) kullanılarak dağıtılabilir.

> [!NOTE]
> Azure yığın Geliştirme Seti için yükleyici kullanıcı arabirimi WCF ve PowerShell dayalı bir açık kaynaklı komut dosyasıdır.

### <a name="prepare-the-development-kit-host-using-a-guided-user-experience"></a>Geliştirme Seti ana destekli kullanıcı deneyimi kullanarak hazırlama
Ana bilgisayara ASDK yükleyebilmek için önce ASDK ortamı hazırlanmalıdır.
1. Bir yerel yönetici olarak ASDK ana bilgisayara oturum açın.
2. CloudBuilder.vhdx dosya C:\ sürücüsü (C:\CloudBuilder.vhdx) kök dizinine taşındığından emin olun.
3. Geliştirme seti yükleyicisini (asdk-installer.ps1) indirin için aşağıdaki betiği çalıştırın [Azure yığın GitHub araçları deposu](https://github.com/Azure/AzureStack-Tools) için **C:\AzureStack_Installer** klasöründe, Geliştirme Seti ana bilgisayarı:

  ```powershell
  # Variables
  $Uri = 'https://raw.githubusercontent.com/Azure/AzureStack-Tools/master/Deployment/asdk-installer.ps1'
  $LocalPath = 'C:\AzureStack_Installer'
  # Create folder
  New-Item $LocalPath -Type directory
  # Download file
  Invoke-WebRequest $uri -OutFile ($LocalPath + '\' + 'asdk-installer.ps1')
  ```

4. Yükseltilmiş bir PowerShell konsolundan Başlat **C:\AzureStack_Installer\asdk-installer.ps1** komut dosyası ve ardından **hazırlama ortamı**.
5. Üzerinde **seçin Cloudbuilder vhdx** yükleyici, bulun ve seçin sayfasında **cloudbuilder.vhdx** indirmiş ve önceki adımlarda ayıklanan dosya. Ayrıca, isteğe bağlı olarak yapabileceğiniz bu sayfada etkinleştirmek **sürücüleri ekleme** Geliştirme Seti ana bilgisayara ek sürücüler eklemeniz gerekiyorsa, kutuyu.  
6. Üzerinde **isteğe bağlı ayarlar** sayfasında, Geliştirme Seti ana bilgisayarda yerel yönetici hesabı sağlayın. 

  > [!IMPORTANT]
  > Bu kimlik bilgileri sağlamazsanız, Geliştirme Seti kurulumunun bir parçası olarak bilgisayar yeniden başlatıldıktan sonra doğrudan ya da barındırmak için KVM erişim gerekir.

7. Bu isteğe bağlı ayarlar üzerinde sağlayabilir **isteğe bağlı ayarlar** sayfa:
    - **ComputerName**: Bu seçenek Geliştirme Seti ana bilgisayar adını ayarlar. Ad FQDN gereksinimlere uygun olmalıdır ve 15 karakter veya daha az olmalıdır. Varsayılan Windows tarafından oluşturulan bir rastgele bilgisayar adıdır.
    - **Saat dilimi**: Geliştirme Seti ana bilgisayar için saat dilimini ayarlar. Varsayılan değer (UTC-8:00) Pasifik Saati (ABD ve Kanada).
    - **Statik IP yapılandırması**: dağıtımınızı statik bir IP adresi kullanacak şekilde ayarlar. Aksi takdirde, yükleyici cloudbuilder.vhx yeniden başlatıldığında, ağ arabirimleri DHCP ile yapılandırılır.
11. **İleri**’ye tıklayın.
12. Bir statik IP yapılandırması önceki adımda seçtiğiniz istiyorsanız, artık gerekir:
    - Bir ağ bağdaştırıcısı seçin. Tıklamadan önce bağdaştırıcıya bağlanabilir emin olun **sonraki**.
    - Olduğundan emin olun **IP adresi**, **ağ geçidi**, ve **DNS** değerleri doğru olduğundan ve ardından **sonraki**.
13. Tıklatın **sonraki** hazırlama işlemini başlatmak için.
14. Zaman hazırlık gösterdiği **tamamlandı**, tıklatın **sonraki**.
15. ' I tıklatın **artık yeniden** cloudbuilder.vhdx önyükleme ve dağıtım işlemine devam edin.


### <a name="deploy-the-development-kit-using-a-guided-experience"></a>Kılavuzlu bir deneyim kullanarak Geliştirme Seti dağıtma
ASDK ana bilgisayar hazırladıktan sonra aşağıdaki adımları kullanarak CloudBuilder.vhdx görüntüsüne ASDK dağıtılabilir. 
1. Ana bilgisayar CloudBuilder.vhdx görüntüsüne başarıyla başlatıldıktan sonra önceki adımlarda belirtilen yönetici kimlik bilgilerini kullanarak oturum açın. 
2. Yükseltilmiş bir PowerShell konsolunu açın ve Çalıştır **\AzureStack_Installer\asdk-installer.ps1** (olabilen şimdi CloudBuilder.vhdx görüntüdeki farklı bir sürücüde) komut dosyası. **Yükle**'ye tıklayın.
3. İçinde **türü** açılan kutusunda **Azure bulut** veya **AD FS**.
    - **Azure bulut**: yapılandırır Azure Active Directory (Azure AD) kimlik sağlayıcısı. Bu seçeneği kullanmak için tam adı Azure AD, bir internet bağlantısına ihtiyacınız vardır dizin Kiracı biçiminde *domainname*. onmicrosoft.com ve belirtilen dizin için genel yönetici kimlik bilgileri. 
    - **AD FS**: dizin hizmeti, kimlik sağlayıcısı olarak kullanılacak varsayılan Damga. Oturum açmak için varsayılan hesaptır azurestackadmin@azurestack.local, ve kullanmak için kurulumunun bir parçası sağlanan bir paroladır.
4. Altında **yerel yönetici parolasını**, **parola** kutusuna (hangi geçerli yapılandırılmış yerel yönetici parolası eşleşmelidir) yerel yönetici parolasını yazın ve ardından**Sonraki**.
5. Geliştirme Seti için kullanın ve ardından bir ağ bağdaştırıcısı seçin **sonraki**.
6. DHCP veya BGPNAT01 sanal makine için statik ağ yapılandırması seçin.
    - **DHCP** (varsayılan): sanal makine IP ağ yapılandırması DHCP sunucusundan alır.
    - **Statik**: DHCP Internet'e erişmek için Azure yığın için geçerli bir IP adresi atanamıyor, yalnızca bu seçeneği kullanın. Statik bir IP adresi, alt ağ maskesi uzunluğunu CIDR biçiminde (örneğin, 10.0.0.5/24) ile belirtilmelidir.
7. İsteğe bağlı olarak, aşağıdaki değerleri ayarlayın:
    - **VLAN kimliği**: VLAN kimliğini ayarlar Yalnızca AzS BGPNAT01 ve konak fiziksel ağ (ve Internet) erişmek için VLAN kimliği yapılandırmanız gerekir, bu seçeneği kullanın. 
    - **DNS ileticisi**: bir DNS sunucusu, Azure yığın dağıtımının bir parçası oluşturulur. Damga dışında adları çözümlemek için çözüm içinde izin vermek için mevcut altyapısını DNS sunucunuzun sağlar. Damga DNS sunucusu Bilinmeyen ad çözümleme isteklerinin bu sunucusuna iletir.
    - **Saat sunucusu**: Bu alan ayarlar Geliştirme Seti tarafından kullanılacak saat sunucusu gereklidir. Bu parametre, geçerli saat sunucusu IP adresi olarak sağlanmalıdır. Sunucu adları desteklenmez.
  
  > [!TIP]
  > Bir saat sunucusu IP adresini bulmak için ziyaret [pool.ntp.org](http:\\pool.ntp.org) veya time.windows.com ping işlemi yapın. 
  
8. **İleri**’ye tıklayın. 
9. Üzerinde **ağ arabirimi kartı özelliklerini doğrulama** sayfasında, bir ilerleme çubuğu görürsünüz. 
    - Bunu diyorsa **bir güncelleştirme karşıdan**, sayfasındaki yönergeleri izleyin.
    - Ne zaman yazacaktır **tamamlandı**, tıklatın **sonraki**.
10. Üzerinde **Özet** sayfasında, **dağıtma**. Burada, Geliştirme Seti yüklemek için kullanılan PowerShell Kurulum komutları de kopyalayabilirsiniz.
11. Bir Azure AD dağıtımı kullanıyorsanız, Kurulum başladıktan sonra birkaç dakika Azure AD genel yönetici hesabı kimlik bilgilerinizi girmeniz istenir.
12. Dağıtım işlemi sırasında sistem otomatik olarak bir kez yeniden birkaç saat sürebilir. Dağıtım başarılı olduğunda, PowerShell konsolunda görüntüler: **tamamlandı: eylem 'Dağıtım'**. Dağıtım başarısız olursa şunları yapabilirsiniz [dağıtmanız](azure-stack-redeploy.md) baştan ya da kullanım aşağıdaki PowerShell, aynı yükseltilmiş PowerShell penceresinden, dağıtım son başarılı adımdan yeniden başlatmak için komutları:

  ```powershell
  cd C:\CloudDeployment\Setup
  .\InstallAzureStackPOC.ps1 -Rerun
  ```

   > [!IMPORTANT]
   > Dağıtımının ilerleme durumunu izlemek isterseniz, Kurulum sırasında Geliştirme Seti ana bilgisayar yeniden başlatıldığında azurestack\AzureStackAdmin oturum açın. Makine etki alanına katılan sonra yerel yönetici oturum açarsanız, dağıtımının ilerleme durumunu göremezsiniz. Dağıtımı yeniden çalıştır bulunamadı, bunun yerine azurestack\AzureStackAdmin çalıştığını doğrulamak oturum açın.
   

## <a name="deploy-the-asdk-using-powershell"></a>PowerShell kullanarak ASDK dağıtma
Önceki adımları destekli kullanıcı deneyimi kullanarak ASDK dağıtma aracılığıyla gitti. Alternatif olarak, bu bölümdeki adımları izleyerek ASDK Geliştirme Seti konakta dağıtmak için PowerShell'i kullanabilirsiniz.

### <a name="configure-the-asdk-host-computer-to-boot-from-cloudbuildervhdx"></a>CloudBuilder.vhdx önyükleme ASDK ana bilgisayarı yapılandırma
Bu komutlar, indirildi ve ayıklanan Azure yığın sanal sabit disk (CloudBuilder.vhdx) önyükleme ASDK ana bilgisayarınız yapılandırır. Bu adımları tamamladıktan sonra ASDK ana bilgisayarı yeniden başlatın.

1. Komut istemini yönetici olarak başlatın.
2. `bcdedit /copy {current} /d "Azure Stack"` öğesini çalıştırın
3. Gerekli {} dahil olmak üzere döndürülen kopyala (CTRL + C) CLSID değeri ' s. Bu değer, {CLSID} olarak adlandırılır ve kalan adımları (CTRL + V veya sağ tıklama) yapıştırılmasına gerekir.
4. `bcdedit /set {CLSID} device vhd=[C:]\CloudBuilder.vhdx` öğesini çalıştırın 
5. `bcdedit /set {CLSID} osdevice vhd=[C:]\CloudBuilder.vhdx` öğesini çalıştırın 
6. `bcdedit /set {CLSID} detecthal on` öğesini çalıştırın 
7. `bcdedit /default {CLSID}` öğesini çalıştırın
8. Önyükleme ayarlarını doğrulamak için çalıştırın `bcdedit`. 
9. Dosya C:\ sürücüsü (C:\CloudBuilder.vhdx) kök dizinine taşınır ve Geliştirme Seti ana bilgisayarı yeniden CloudBuilder.vhdx emin olun. ASDK ana bilgisayar yeniden başlatıldığında, artık CloudBuilder.vhdx VM'den önyüklemek için varsayılan olarak. 

### <a name="prepare-the-development-kit-host-using-powershell"></a>PowerShell kullanarak Geliştirme Seti konak hazırlama 
Geliştirme Seti ana bilgisayarı başarıyla CloudBuilder.vhdx görüntüsüne başlatıldıktan sonra yükseltilmiş bir PowerShell konsolu açın ve Geliştirme Seti konaktaki ASDK dağıtmak için bu bölümdeki komutlar çalıştırın.

> [!IMPORTANT] 
> ASDK yükleme tam olarak bir ağ arabirim kartı (NIC) için ağ iletişimi destekler. Birden çok NIC varsa, yalnızca bir etkin (ve diğer tüm kullanıcılar devre dışı bırakılır) dağıtım betiği çalıştırmadan önce emin olun.

Azure AD veya AD FS ile Azure yığın kimlik sağlayıcısı olarak dağıtabilirsiniz. Azure yığını, kaynak sağlayıcıları ve diğer uygulamaların her ikisi de aynı şekilde çalışır. Azure yığında AD FS ile desteklenen özellikler hakkında daha fazla bilgi için bkz: [anahtar özelliklerinin ve kavramlarının](.\azure-stack-key-features.md) makalesi.

> [!TIP]
> (InstallAzureStackPOC.ps1 isteğe bağlı parametreler ve örneklere bakın) herhangi bir kurulum parametre girmezseniz, gerekli parametreleri istenir.

### <a name="deploy-azure-stack-using-azure-ad"></a>Azure AD kullanarak Azure yığın dağıtma 
Azure yığın dağıtmak için **Azure AD kimlik sağlayıcısı olarak kullanarak**, doğrudan ya da saydam bir proxy üzerinden internet bağlantısı olması gerekir. Azure AD kullanarak Geliştirme Seti dağıtmak için aşağıdaki PowerShell komutlarını çalıştırın:

  ```powershell
  cd C:\CloudDeployment\Setup     
  $adminpass = Get-Credential Administrator     
  .\InstallAzureStackPOC.ps1 -AdminPassword $adminpass.Password
  ```

  Birkaç dakika içinde ASDK yükleme için Azure AD kimlik bilgileri istenir. Azure AD kiracınız için genel yönetici kimlik bilgilerini sağlamanız gerekir. 

### <a name="deploy-azure-stack-using-ad-fs"></a>Azure AD FS kullanarak yığın dağıtma 
Geliştirme Seti dağıtmak için **kimlik sağlayıcısı olarak AD FS kullanarak**, (yeterlidir - UseADFS parametre eklemek için) aşağıdaki PowerShell komutlarını çalıştırın: 

  ```powershell
  cd C:\CloudDeployment\Setup     
  $adminpass = Get-Credential Administrator 
  .\InstallAzureStackPOC.ps1 -AdminPassword $adminpass.Password -UseADFS
  ```

AD FS dağıtımında, dizin hizmeti varsayılan damga kimlik sağlayıcısı olarak kullanılır. Oturum açmak için varsayılan hesaptır azurestackadmin@azurestack.local, ve PowerShell Kurulum komutları bir parçası olarak sağlanan için parola ayarlanır.

Dağıtım işlemi sırasında hangi zaman sistem otomatik olarak bir kez yeniden başlatılır birkaç saat sürebilir. Dağıtım başarılı olduğunda, PowerShell konsolunda görüntüler: **tamamlandı: eylem 'Dağıtım'**. Dağıtım başarısız olursa, komut dosyası kullanarak yeniden çalıştırmayı deneyebilirsiniz yeniden çalıştır parametresi. Veya, [ASDK dağıtmanız](.\azure-stack-redeploy.md) sıfırdan.
> [!IMPORTANT]
> ASDK ana bilgisayar yeniden başlatıldıktan sonra dağıtımının ilerleme durumunu izlemek isterseniz, AzureStack\AzureStackAdmin imzalamanız gerekir. Ana bilgisayar yeniden (ve azurestack.local etki alanına katılan sonra) yerel bir yönetici olarak oturum açarsanız dağıtımının ilerleme durumunu göremezsiniz. Dağıtımı yeniden çalıştır bulunamadı, bunun yerine azurestack çalıştığını doğrulamak oturum açın.
>

#### <a name="azure-ad-deployment-script-examples"></a>Azure AD dağıtım komut dosyası örnekleri
Tüm komut Azure AD dağıtımı. Bazı isteğe bağlı parametreleri açıklamalı bazı örnekler şunlardır.

Azure AD kimlik bilgilerinizi yalnızca ile ilişkili ise **bir** Azure AD dizini:
```powershell
cd C:\CloudDeployment\Setup 
$adminpass = Get-Credential Administrator 
$aadcred = Get-Credential "<Azure AD global administrator account name>" 
.\InstallAzureStackPOC.ps1 -AdminPassword $adminpass.Password -InfraAzureDirectoryTenantAdminCredential $aadcred -TimeServer 52.168.138.145 #Example time server IP address.
```

Azure AD kimlik bilgilerinizi ilişkilendirilen **birden büyük** Azure AD dizini:
```powershell
cd C:\CloudDeployment\Setup 
$adminpass = Get-Credential Administrator 
$aadcred = Get-Credential "<Azure AD global administrator account name>" #Example: user@AADDirName.onmicrosoft.com 
.\InstallAzureStackPOC.ps1 -AdminPassword $adminpass.Password -InfraAzureDirectoryTenantAdminCredential $aadcred -InfraAzureDirectoryTenantName "<specific Azure AD directory in the form of domainname.onmicrosoft.com>" -TimeServer 52.168.138.145 #Example time server IP address.
```

Varsa ortamınızı **yok** aşağıdaki ek parametreleri (örnek kullanım sağlanan) yukarıdaki seçeneklerden birini içermelidir etkinleştirilirse DHCP sahip: 

```powershell
.\InstallAzureStackPOC.ps1 -AdminPassword $adminpass.Password -InfraAzureDirectoryTenantAdminCredential $aadcred -NatIPv4Subnet 10.10.10.0/24 -NatIPv4Address 10.10.10.3 -NatIPv4DefaultGateway 10.10.10.1 -TimeServer 10.222.112.26
```

### <a name="asdk-installazurestackpocps1-optional-parameters"></a>ASDK InstallAzureStackPOC.ps1 isteğe bağlı parametreler
|Parametre|Gerekli/isteğe bağlı|Açıklama|
|-----|-----|-----|
|AdminPassword|Gerekli|Geliştirme Seti dağıtımının bir parçası oluşturulan tüm sanal makineler üzerinde yerel yönetici hesabı ve diğer tüm kullanıcı hesaplarını ayarlar. Bu parola ana bilgisayardaki geçerli yerel yönetici parolası eşleşmelidir.|
|InfraAzureDirectoryTenantName|Gerekli|Kiracı dizinini ayarlar. AAD hesabıyla birden çok dizin Yönetme iznine sahip olduğu belirli bir dizin belirtmek için bu parametreyi kullanın. Tam bir AAD dizini Kiracı biçiminde adı. onmicrosoft.com.|
|Zaman sunucusunu|Gerekli|Belirli bir saat sunucusu belirtmek için bu parametreyi kullanın. Bu parametre, geçerli saat sunucusu IP adresi olarak sağlanmalıdır. Sunucu adları desteklenmez.|
|InfraAzureDirectoryTenantAdminCredential|İsteğe bağlı|Azure Active Directory kullanıcı adı ve parola ayarlar. Bu Azure kimlik bilgileri kuruluş kimliği olmalıdır|
|InfraAzureEnvironment|İsteğe bağlı|Bu Azure yığın dağıtımına kaydetmek istediğiniz Azure ortamı seçin. Seçenekler, ortak Azure, Azure - Çin'de Azure - ABD devlet kurumları içerir.|
|DNSForwarder|İsteğe bağlı|Bir DNS sunucusu Azure yığın dağıtımının bir parçası oluşturulur. Damga dışında adları çözümlemek için çözüm içinde izin vermek için mevcut altyapısını DNS sunucunuzun sağlar. Damga DNS sunucusu Bilinmeyen ad çözümleme isteklerinin bu sunucusuna iletir.|
|NatIPv4Address|DHCP NAT desteği için gerekli|Statik bir IP adresi için MAS BGPNAT01 ayarlar. Bu parametreyi yalnızca DHCP'nin, İnternet erişimi için geçerli bir IP adresi atayamadığı durumlarda kullanın.|
|NatIPv4Subnet|DHCP NAT desteği için gerekli|NAT destek DHCP için kullanılan IP alt ağ önek. Bu parametreyi yalnızca DHCP'nin, İnternet erişimi için geçerli bir IP adresi atayamadığı durumlarda kullanın.|
|PublicVlanId|İsteğe bağlı|VLAN kimliğini ayarlar Yalnızca MAS BGPNAT01 ve konak fiziksel ağ (ve Internet) erişmek için VLAN kimliği yapılandırmanız gerekir, bu parametreyi kullanın. Örneğin,.\InstallAzureStackPOC.ps1-Verbose - PublicVLan 305|
|Yeniden çalıştırın|İsteğe bağlı|Dağıtım yeniden çalıştırmak için bu bayrağı kullanın. Tüm önceki giriş kullanılır. Çeşitli benzersiz değerler oluşturulur ve dağıtım için kullanılır çünkü daha önce sağlanan verileri yeniden girmeden desteklenmiyor.|

## <a name="activate-the-administrator-and-tenant-portals"></a>Yönetici ve Kiracı portalı etkinleştir
Azure AD kullanan dağıtımlar sonra Azure yığın yönetici ve Kiracı portalı etkinleştirmeniz gerekir. Bu etkinleştirme, Azure yığın portalı ve Azure Resource Manager (onay sayfasında listelenen) doğru izinler tüm kullanıcılar için dizin vermenizi izin.

- Yönetici portalı'nı https://adminportal.local.azurestack.external/guest/signup için gidin, bilgileri okuyun ve ardından **kabul**. Kabul ettikten sonra aynı zamanda dizin Kiracı yönetici olmayan hizmet yöneticileri ekleyebilirsiniz.

- Kiracı portalı için https://portal.local.azurestack.external/guest/signup gidin, bilgileri okuyun ve ardından **kabul**. Kabul ettikten sonra dizindeki kullanıcıları Kiracı portalında oturum açabilir. 

> [!NOTE] 
> Portalları etkin değil, yalnızca dizin yönetici oturum açabilir ve portalları kullanın. Başka bir kullanıcı oturum açtığında bunların yönetici izinleri diğer kullanıcılara vermemiş bildiren bir hata görürsünüz. Yönetici yerel olarak Azure yığın kayıtlı dizinine ait değil, Azure yığın dizin etkinleştirme URL'sine eklenmesi gerekir. Örneğin, Azure yığın fabrikam.onmicrosoft.com ve yönetici kullanıcı için kayıtlı ise admin@contoso.com, portal etkinleştirmek için https://portal.local.azurestack.external/guest/signup/fabrikam.onmicrosoft.com için gidin. 

## <a name="reset-the-password-expiration-policy"></a>Parola süre sonu ilkesi Sıfırla 
ASDK dağıttıktan sonra Değerlendirme dönemi sona ermeden önce Geliştirme Seti ana bilgisayar için parola süresi sona ermiyor emin olmak için aşağıdaki adımları izleyin.

### <a name="to-change-the-password-expiration-policy-from-powershell"></a>Parola süre sonu ilkesi Powershell'den değiştirmek için:
Yükseltilmiş bir Powershell konsolundan komutu çalıştırın:

```powershell
Set-ADDefaultDomainPasswordPolicy -MaxPasswordAge 180.00:00:00 -Identity azurestack.local
```

### <a name="to-change-the-password-expiration-policy-manually"></a>Parola süre sonu ilkesi el ile değiştirmek için:
1. Geliştirme Seti konakta açmak **Grup İlkesi Yönetimi** gidin **Grup İlkesi Yönetimi** – **orman: azurestack.local** – **etkialanları** – **azurestack.local**.
2. Sağ tıklayın **varsayılan etki alanı ilkesi** tıklatıp **Düzenle**.
3. Grup İlkesi Yönetimi Düzenleyicisi'nde gidin **Bilgisayar Yapılandırması** – **ilkeleri** – **Windows ayarları** – **güvenlik ayarları**– **Hesap ilkeleri** – **parola ilkesi**.
4. Sağ bölmede, çift **en uzun parola geçerlilik süresi**.
5. İçinde **en uzun parola geçerlilik süresi özellikleri** iletişim kutusu, değişiklik **parola içinde dolacak** değeri için 180 ve ardından **Tamam**.


## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack’e bağlanma](azure-stack-connect-azure-stack.md)

[PowerShell Azure yığın ortamlar için Kur](azure-stack-powershell-configure-quickstart.md)

[Azure yığını, Azure aboneliğiniz ile kaydetme](azure-stack-register.md)



