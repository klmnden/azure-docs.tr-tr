---
title: "PowerShell Azure yığın - dağıtma | Microsoft Docs"
description: "Bu öğreticide, komut satırından ASDK yükleyin."
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 03/16/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 48ccccaba6b7f5780f1d42dfbe5d9747c5e30292
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/17/2018
---
# <a name="tutorial-deploy-the-asdk-from-the-command-line"></a>Öğretici: komut satırından ASDK dağıtma
Bu öğreticide, Azure yığın Geliştirme Seti (ASDK) üretim dışı bir ortamda komut satırından dağıtın. 

ASDK değerlendirmek ve Azure yığın özelliklerin ve hizmetler için dağıtabileceğiniz bir test ve geliştirme ortamıdır. Bunu almak için hazır ve çalışır, ortam donanımını hazırlayın ve (Bu işlem birkaç saat sürebilir) bazı kodlar çalıştırmak ihtiyacınız. Bundan sonra yönetici ve kullanıcı portalı için Azure yığın kullanmaya başlamak için oturum açabilir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Karşıdan yükleme ve dağıtım paketi ayıklayın
> * Geliştirme Seti ana bilgisayarı hazırla 
> * Dağıtım sonrası yapılandırmalar gerçekleştir
> * Azure ile kaydedin

## <a name="prerequisites"></a>Önkoşullar 
Geliştirme Seti ana bilgisayar hazırlayın. Donanım, yazılım ve ağ planlayın. Geliştirme Seti (Geliştirme Seti ana bilgisayarı) barındıran bilgisayarın donanım, yazılım ve ağ gereksinimleri karşılaması gerekir. Ayrıca, Azure Active Directory (Azure AD) veya Active Directory Federasyon Hizmetleri (AD FS) kullanma arasında seçmeniz gerekir. Yükleme işlemi sorunsuz çalışır dağıtımınıza başlamadan önce bu önkoşulları yerine uyumlu emin olun. 

ASDK dağıtmadan önce planlanan Geliştirme Seti ana bilgisayarın donanım, işletim sistemi, hesap ve ağ yapılandırmaları ASDK yüklemeye yönelik en düşük gereksinimleri karşıladığından emin olun.

**[ASDK dağıtım planlama konuları gözden geçirin](asdk-deploy-considerations.md)**

> [!TIP]
> Kullanabileceğiniz [Azure yığın dağıtım gereksinimlerini denetleyin aracı](https://gallery.technet.microsoft.com/Deployment-Checker-for-50e0f51b) donanımınız tüm gereksinimleri karşıladığını onaylamak için işletim sisteminin yükledikten sonra.

## <a name="download-and-extract-the-deployment-package"></a>Karşıdan yükleme ve dağıtım paketi ayıklayın
Geliştirme Seti ana bilgisayarınız ASDK yüklemek için temel gereksinimleri karşıladığından emin olduktan sonra yüklemek ve ASDK dağıtım paketi ayıklamak için sonraki adım olacaktır. Dağıtım paketi, önyüklenebilir bir işletim sistemi ve Azure yığın yükleme dosyalarını içeren bir sanal sabit sürücü Cloudbuilder.vhdx dosyası içerir.

Dağıtım paketi Geliştirme Seti ana bilgisayara veya başka bir bilgisayara yükleyebilirsiniz. Başka bir bilgisayar kullanılarak Geliştirme Seti ana bilgisayar için donanım gereksinimlerini azaltmaya yardımcı olacak ayıklanan dağıtım dosyaları 60 GB boş disk alanı alın.

**[İndirmeyi ve ayıklamayı Azure yığın Geliştirme Seti (ASDK)](asdk-download.md)**

## <a name="prepare-the-development-kit-host-computer"></a>Geliştirme Seti ana bilgisayarı hazırla
Ana bilgisayara ASDK yükleyebilmek için önce ortamı hazırlanmalıdır ve sistem VHD'den önyükleme için yapılandırılmış. Bu adımdan sonra Geliştirme Seti konak Cloudbuilder.vhdx (önyüklenebilir bir işletim sistemi ve Azure yığın yükleme dosyalarını içeren bir sanal sabit sürücü) önyükleme yapmaz.

CloudBuilder.vhdx önyükleme ASDK ana bilgisayarı yapılandırmak için PowerShell kullanın. Bu komutlar, indirildi ve ayıklanan Azure yığın sanal sabit disk (CloudBuilder.vhdx) önyükleme ASDK ana bilgisayarınız yapılandırın. Bu adımları tamamladıktan sonra ASDK ana bilgisayarı yeniden başlatın.

CloudBuilder.vhdx önyükleme ASDK ana bilgisayarı yapılandırmak için:

  1. Komut istemini yönetici olarak başlatın.
  2. `bcdedit /copy {current} /d "Azure Stack"`'i çalıştırın.
  3. Gerekli {} dahil olmak üzere döndürülen kopyala (CTRL + C) CLSID değeri ' s. Bu değer, {CLSID} olarak adlandırılır ve kalan adımları (CTRL + V veya sağ tıklama) yapıştırılmasına gerekir.
  4. `bcdedit /set {CLSID} device vhd=[C:]\CloudBuilder.vhdx`'i çalıştırın. 
  5. `bcdedit /set {CLSID} osdevice vhd=[C:]\CloudBuilder.vhdx`'i çalıştırın. 
  6. `bcdedit /set {CLSID} detecthal on`'i çalıştırın. 
  7. `bcdedit /default {CLSID}`'i çalıştırın.
  8. Önyükleme ayarlarını doğrulamak için çalıştırın `bcdedit`. 
  9. Dosya C:\ sürücüsü (C:\CloudBuilder.vhdx) kök dizinine taşınır ve Geliştirme Seti ana bilgisayarı yeniden CloudBuilder.vhdx emin olun. ASDK ana bilgisayar yeniden başlatıldığında ASDK dağıtımına başlamak için CloudBuilder.vhdx sanal makine sabit sürücüden önyükleme yapmanız gerekir. 

> [!IMPORTANT]
> Yeniden başlatmadan önce Geliştirme Seti konak bilgisayara doğrudan fiziksel veya KVM erişiminiz emin olun. VM ilk kez başlatıldığında, Windows Server Kurulumu tamamlamak için ister. Geliştirme Seti ana bilgisayarında oturum açmak için kullandığınız aynı yönetici kimlik bilgilerini sağlayın. 

### <a name="prepare-the-development-kit-host-using-powershell"></a>PowerShell kullanarak Geliştirme Seti konak hazırlama 
Geliştirme Seti sonra ana bilgisayarı başarıyla CloudBuilder.vhdx görüntüyü, Geliştirme Seti ana bilgisayarında oturum açmak için kullanılan (ve Windows Server sonuçlandırılıyor bir parçası olarak sağlanan aynı yerel yönetici kimlik bilgileri oturum önyüklenir Ana bilgisayar VHD'den önyüklendiğinde Kurulumu). 

> [!NOTE]
> İsteğe bağlı olarak da yapılandırabilirsiniz [Azure yığın telemetri ayarlarını](asdk-telemetry.md#set-telemetry-level-in-the-windows-registry) *önce* ASDK yükleme.

Yükseltilmiş bir PowerShell konsolu açın ve Geliştirme Seti konaktaki ASDK dağıtmak için bu bölümdeki komutlar çalıştırın.

> [!IMPORTANT] 
> ASDK yükleme tam olarak bir ağ arabirim kartı (NIC) için ağ iletişimi destekler. Birden çok NIC varsa, yalnızca bir etkin (ve diğer tüm kullanıcılar devre dışı bırakılır) dağıtım betiği çalıştırmadan önce emin olun.

Azure AD ile Azure yığın dağıtabilir veya Windows Server AD FS kimlik sağlayıcısı. Azure yığını, kaynak sağlayıcıları ve diğer uygulamaların her ikisi de aynı şekilde çalışır.

> [!TIP]
> (InstallAzureStackPOC.ps1 isteğe bağlı parametreler ve örneklere bakın) herhangi bir kurulum parametre girmezseniz, gerekli parametreleri istenir.

### <a name="deploy-azure-stack-using-azure-ad"></a>Azure AD kullanarak Azure yığın dağıtma 
Azure yığın dağıtmak için **Azure AD kimlik sağlayıcısı olarak kullanarak**, doğrudan ya da saydam bir proxy üzerinden internet bağlantısı olması gerekir. 

Azure AD kullanarak Geliştirme Seti dağıtmak için aşağıdaki PowerShell komutlarını çalıştırın:

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

Dağıtım işlemi sırasında hangi zaman sistem otomatik olarak bir kez yeniden başlatılır birkaç saat sürebilir. Dağıtım başarılı olduğunda, PowerShell konsolunda görüntüler: **tamamlandı: eylem 'Dağıtım'**. Dağıtım başarısız olursa, komut dosyası kullanarak yeniden çalıştırmayı deneyebilirsiniz yeniden çalıştır parametresi. Veya, [ASDK dağıtmanız](asdk-redeploy.md) sıfırdan.

> [!IMPORTANT]
> ASDK ana bilgisayar yeniden başlatıldıktan sonra dağıtımının ilerleme durumunu izlemek isterseniz, AzureStack\AzureStackAdmin imzalamanız gerekir. Ana bilgisayar yeniden (ve azurestack.local etki alanına katılan sonra) yerel bir yönetici olarak oturum açarsanız dağıtımının ilerleme durumunu göremezsiniz. Dağıtımı yeniden çalıştır bulunamadı, bunun yerine azurestack çalıştığını doğrulamak oturum açın.


#### <a name="azure-ad-deployment-script-examples"></a>Azure AD dağıtım komut dosyası örnekleri
Tüm komut Azure AD dağıtımı. Burada, bazı isteğe bağlı parametreler dahil birkaç açıklamalı örnekler verilmiştir.

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
.\InstallAzureStackPOC.ps1 -AdminPassword $adminpass.Password -InfraAzureDirectoryTenantAdminCredential $aadcred -InfraAzureDirectoryTenantName "<Azure AD directory in the form of domainname.onmicrosoft.com or an Azure AD verified custom domain name>" -TimeServer 52.168.138.145 #Example time server IP address.
```

Ortamınız DHCP etkin yoksa, aşağıdaki ek parametreleri (örnek kullanım sağlanan) yukarıdaki seçeneklerden birini eklemeniz gerekir: 

```powershell
.\InstallAzureStackPOC.ps1 -AdminPassword $adminpass.Password -InfraAzureDirectoryTenantAdminCredential $aadcred -NatIPv4Subnet 10.10.10.0/24 -NatIPv4Address 10.10.10.3 -NatIPv4DefaultGateway 10.10.10.1 -TimeServer 10.222.112.26
```

### <a name="asdk-installazurestackpocps1-optional-parameters"></a>ASDK InstallAzureStackPOC.ps1 isteğe bağlı parametreler
|Parametre|Gerekli/isteğe bağlı|Açıklama|
|-----|-----|-----|
|AdminPassword|Gerekli|Geliştirme Seti dağıtımının bir parçası oluşturulan tüm sanal makineler üzerinde yerel yönetici hesabı ve diğer tüm kullanıcı hesaplarını ayarlar. Bu parola ana bilgisayardaki geçerli yerel yönetici parolası eşleşmelidir.|
|InfraAzureDirectoryTenantName|Gerekli|Kiracı dizinini ayarlar. AAD hesabıyla birden çok dizin Yönetme iznine sahip olduğu belirli bir dizin belirtmek için bu parametreyi kullanın. Tam bir AAD dizini Kiracı biçiminde adı. onmicrosoft.com veya Azure AD doğrulandı özel etki alanı adı.|
|Zaman sunucusunu|Gerekli|Belirli bir saat sunucusu belirtmek için bu parametreyi kullanın. Bu parametre, geçerli saat sunucusu IP adresi olarak sağlanmalıdır. Sunucu adları desteklenmez.|
|InfraAzureDirectoryTenantAdminCredential|İsteğe bağlı|Azure Active Directory kullanıcı adı ve parola ayarlar. Bu Azure kimlik bilgileri kuruluş kimliği olmalıdır|
|InfraAzureEnvironment|İsteğe bağlı|Bu Azure yığın dağıtımına kaydetmek istediğiniz Azure ortamı seçin. Seçenekler, ortak Azure, Azure - Çin'de Azure - ABD devlet kurumları içerir.|
|DNSForwarder|İsteğe bağlı|Bir DNS sunucusu Azure yığın dağıtımının bir parçası oluşturulur. Damga dışında adları çözümlemek için çözüm içinde izin vermek için mevcut altyapısını DNS sunucunuzun sağlar. Damga DNS sunucusu Bilinmeyen ad çözümleme isteklerinin bu sunucusuna iletir.|
|NatIPv4Address|DHCP NAT desteği için gerekli|Statik bir IP adresi için MAS BGPNAT01 ayarlar. Bu parametreyi yalnızca DHCP'nin, İnternet erişimi için geçerli bir IP adresi atayamadığı durumlarda kullanın.|
|NatIPv4Subnet|DHCP NAT desteği için gerekli|NAT destek DHCP için kullanılan IP alt ağ önek. Bu parametreyi yalnızca DHCP'nin, İnternet erişimi için geçerli bir IP adresi atayamadığı durumlarda kullanın.|
|PublicVlanId|İsteğe bağlı|VLAN kimliğini ayarlar Yalnızca MAS BGPNAT01 ve konak fiziksel ağ (ve Internet) erişmek için VLAN kimliği yapılandırmanız gerekir, bu parametreyi kullanın. Örneğin,.\InstallAzureStackPOC.ps1-Verbose - PublicVLan 305|
|Yeniden çalıştır|İsteğe bağlı|Dağıtım yeniden çalıştırmak için bu bayrağı kullanın. Tüm önceki giriş kullanılır. Çeşitli benzersiz değerler oluşturulur ve dağıtım için kullanılır çünkü daha önce sağlanan verileri yeniden girmeden desteklenmiyor.|


## <a name="perform-post-deployment-configurations"></a>Dağıtım sonrası yapılandırmalar gerçekleştir
ASDK yükledikten sonra birkaç önerilen yükleme sonrası denetler ve vardır, yapılması gereken yapılandırma değişikliklerini. Kurulumunuzu doğrulamak için test AzureStack cmdlet'ini kullanarak başarıyla yüklendi ve Azure yığın PowerShell ve GitHub Araçları'nı yükleyin. 

Ayrıca, Değerlendirme dönemi sona ermeden önce Geliştirme Seti ana bilgisayar için parola süresi sona ermiyor emin olmak için parola süresi dolma ilkesini sıfırlaması gerekir.

> [!NOTE]
> İsteğe bağlı olarak da yapılandırabilirsiniz [Azure yığın telemetri ayarlarını](asdk-telemetry.md#enable-or-disable-telemetry-after-deployment) *sonra* ASDK yükleme.

**[Sonrası ASDK dağıtım görevleri](asdk-post-deploy.md)**

## <a name="register-with-azure"></a>Azure ile kaydedin
Yapabilmeniz Azure yığın Azure ile kaydetmeniz gerekir [Azure Market öğesi karşıdan](asdk-marketplace-item.md) Azure yığınına.

**[Azure ile Azure yığın kaydedin](asdk-register.md)**

## <a name="next-steps"></a>Sonraki adımlar
Tebrikler! Bu adımları tamamladıktan sonra hem bir geliştirme seti ortamını gerekir [yönetici](https://adminportal.local.azurestack.external) ve [kullanıcı](https://portal.local.azurestack.external) portalları. 

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Karşıdan yükleme ve dağıtım paketi ayıklayın
> * Geliştirme Seti ana bilgisayarı hazırla 
> * Dağıtım sonrası yapılandırmalar gerçekleştir
> * Azure ile kaydedin

Azure yığın Market öğe ekleme konusunda bilgi almak için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [Bir Azure yığın Market öğesi ekleme](asdk-marketplace-item.md)

